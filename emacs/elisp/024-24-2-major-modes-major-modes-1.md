### 24.2 Major Modes [¶](#Major-Modes-1)

Major modes specialize Emacs for editing or interacting with
particular kinds of text. Each buffer has exactly one major mode at a
time. Every major mode is associated with a *major mode command*,
whose name should end in ‘`-mode`’. This command takes care of
switching to that mode in the current buffer, by setting various
buffer-local variables such as a local keymap. See [Major Mode Conventions](#Major-Mode-Conventions). Note that unlike minor modes there is no way to “turn
off” a major mode, instead the buffer must be switched to a different
one. However, you can temporarily *suspend* a major mode and later
*restore* the suspended mode, see below.

The least specialized major mode is called *Fundamental mode*,
which has no mode-specific definitions or variable settings.

Command: **fundamental-mode** [¶](#index-fundamental_002dmode)
:   This is the major mode command for Fundamental mode. Unlike other mode
    commands, it does *not* run any mode hooks (see [Major Mode Conventions](#Major-Mode-Conventions)), since you are not supposed to customize this mode.

Function: **major-mode-suspend** [¶](#index-major_002dmode_002dsuspend)
:   This function works like `fundamental-mode`, in that it kills all
    buffer-local variables, but it also records the major mode in effect,
    so that it could subsequently be restored. This function and
    `major-mode-restore` (described next) are useful when you need to
    put a buffer under some specialized mode other than the one Emacs
    chooses for it automatically (see [How Emacs Chooses a Major Mode](#Auto-Major-Mode)), but would also
    like to be able to switch back to the original mode later.

Function: **major-mode-restore** &optional avoided-modes [¶](#index-major_002dmode_002drestore)
:   This function restores the major mode recorded by
    `major-mode-suspend`. If no major mode was recorded, this
    function calls `normal-mode` (see [normal-mode](#Auto-Major-Mode)), but tries to force it not to choose any modes in
    avoided-modes, if that argument is non-`nil`.

Function: **clean-mode** [¶](#index-clean_002dmode)
:   Changing the major mode clears out most local variables, but it
    doesn’t remove all artifacts in the buffer (like text properties and
    overlays). It’s rare to change a buffer from one major mode to
    another (except from `fundamental-mode` to everything else), so
    this is usually not a concern. It can sometimes be convenient (mostly
    when debugging a problem in a buffer) to do a “full reset” of the
    buffer, and that’s what the `clean-mode` major mode offers. It
    will kill all local variables (even the permanently local ones), and
    also removes all overlays and text properties.

The easiest way to write a major mode is to use the macro
`define-derived-mode`, which sets up the new mode as a variant of
an existing major mode. See [Defining Derived Modes](#Derived-Modes). We recommend using
`define-derived-mode` even if the new mode is not an obvious
derivative of another mode, as it automatically enforces many coding
conventions for you. See [Basic Major Modes](#Basic-Major-Modes), for common modes to
derive from.

Writing major modes based on the tree-sitter library has some special
aspects and conventions; see [Developing major modes with tree-sitter](#Tree_002dsitter-Major-Modes).

The standard GNU Emacs Lisp directory tree contains the code for
several major modes, in files such as `text-mode.el`,
`texinfo.el`, `lisp-mode.el`, and `rmail.el`. You can
study these libraries to see how modes are written.

User Option: **major-mode** [¶](#index-major_002dmode)
:   The buffer-local value of this variable holds the symbol for the current
    major mode. Its default value holds the default major mode for new
    buffers. The standard default value is `fundamental-mode`.

    If the default value is `nil`, then whenever Emacs creates a new
    buffer via a command such as `C-x b` (`switch-to-buffer`), the
    new buffer is put in the major mode of the previously current buffer.
    As an exception, if the major mode of the previous buffer has a
    `mode-class` symbol property with value `special`, the new
    buffer is put in Fundamental mode (see [Major Mode Conventions](#Major-Mode-Conventions)).

* [Major Mode Conventions](#Major-Mode-Conventions)
* [How Emacs Chooses a Major Mode](#Auto-Major-Mode)
* [Getting Help about a Major Mode](#Mode-Help)
* [Defining Derived Modes](#Derived-Modes)
* [Basic Major Modes](#Basic-Major-Modes)
* [Mode Hooks](#Mode-Hooks)
* [Tabulated List mode](#Tabulated-List-Mode)
* [Generic Modes](#Generic-Modes)
* [Major Mode Examples](#Example-Major-Modes)

---

Next: [How Emacs Chooses a Major Mode](#Auto-Major-Mode), Up: [Major Modes](#Major-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.2.1 Major Mode Conventions [¶](#Major-Mode-Conventions-1)

The code for every major mode should follow various coding
conventions, including conventions for local keymap and syntax table
initialization, function and variable names, and hooks.

If you use the `define-derived-mode` macro, it will take care of
many of these conventions automatically. See [Defining Derived Modes](#Derived-Modes). Note
also that Fundamental mode is an exception to many of these conventions,
because it represents the default state of Emacs.

The following list of conventions is only partial. Each major mode
should aim for consistency in general with other Emacs major modes, as
this makes Emacs as a whole more coherent. It is impossible to list
here all the possible points where this issue might come up; if the
Emacs developers point out an area where your major mode deviates from
the usual conventions, please make it compatible.

* Define a major mode command whose name ends in ‘`-mode`’. When
  called with no arguments, this command should switch to the new mode in
  the current buffer by setting up the keymap, syntax table, and
  buffer-local variables in an existing buffer. It should not change the
  buffer’s contents.
* Write a documentation string for this command that describes the special
  commands available in this mode. See [Getting Help about a Major Mode](#Mode-Help).

  The documentation string may include the special documentation
  substrings, ‘`\[command]`’, ‘`\{keymap}`’, and
  ‘`\<keymap>`’, which allow the help display to adapt
  automatically to the user’s own key bindings. See [Substituting Key Bindings in Documentation](#Keys-in-Documentation).
* The major mode command should start by calling
  `kill-all-local-variables`. This runs the normal hook
  `change-major-mode-hook`, then gets rid of the buffer-local
  variables of the major mode previously in effect. See [Creating and Deleting Buffer-Local Bindings](#Creating-Buffer_002dLocal).
* The major mode command should set the variable `major-mode` to the
  major mode command symbol. This is how `describe-mode` discovers
  which documentation to print.
* The major mode command should set the variable `mode-name` to the
  “pretty” name of the mode, usually a string (but see [The Data Structure of the Mode Line](#Mode-Line-Data), for other possible forms). The name of the mode appears
  in the mode line.
* Calling the major mode command twice in direct succession should not
  fail and should do the same thing as calling the command only once.
  In other words, the major mode command should be idempotent.
* Since all global names are in the same name space, all the global
  variables, constants, and functions that are part of the mode should
  have names that start with the major mode name (or with an abbreviation
  of it if the name is long). See [Emacs Lisp Coding Conventions](#Coding-Conventions).
* In a major mode for editing some kind of structured text, such as a
  programming language, indentation of text according to structure is
  probably useful. So the mode should set `indent-line-function`
  to a suitable function, and probably customize other variables
  for indentation. See [Automatic Indentation of code](#Auto_002dIndentation).
* The major mode should usually have its own keymap, which is used as the
  local keymap in all buffers in that mode. The major mode command should
  call `use-local-map` to install this local map. See [Active Keymaps](#Active-Keymaps), for more information.

  This keymap should be stored permanently in a global variable named
  `modename-mode-map`. Normally the library that defines the
  mode sets this variable.

  See [Tips for Defining Variables Robustly](#Tips-for-Defining), for advice about how to write the code to set
  up the mode’s keymap variable.
* The key sequences bound in a major mode keymap should usually start with
  `C-c`, followed by a control character, a digit, or `{`,
  `}`, `<`, `>`, `:` or `;`. The other punctuation
  characters are reserved for minor modes, and ordinary letters are
  reserved for users.

  A major mode can also rebind the keys `M-n`, `M-p` and
  `M-s`. The bindings for `M-n` and `M-p` should normally
  be some kind of moving forward and backward, but this does not
  necessarily mean cursor motion.

  It is legitimate for a major mode to rebind a standard key sequence if
  it provides a command that does the same job in a way better
  suited to the text this mode is used for. For example, a major mode
  for editing a programming language might redefine `C-M-a` to
  move to the beginning of a function in a way that works better for
  that language. The recommended way of tailoring `C-M-a` to the
  needs of a major mode is to set `beginning-of-defun-function`
  (see [Moving over Balanced Expressions](#List-Motion)) to invoke the function specific to the mode.

  It is also legitimate for a major mode to rebind a standard key
  sequence whose standard meaning is rarely useful in that mode. For
  instance, minibuffer modes rebind `M-r`, whose standard meaning is
  rarely of any use in the minibuffer. Major modes such as Dired or
  Rmail that do not allow self-insertion of text can reasonably redefine
  letters and other printing characters as special commands.
* Major modes for editing text should not define `RET` to do
  anything other than insert a newline. However, it is ok for
  specialized modes for text that users don’t directly edit, such as
  Dired and Info modes, to redefine `RET` to do something entirely
  different.
* Major modes should not alter options that are primarily a matter of user
  preference, such as whether Auto-Fill mode is enabled. Leave this to
  each user to decide. However, a major mode should customize other
  variables so that Auto-Fill mode will work usefully *if* the user
  decides to use it.
* The mode may have its own syntax table or may share one with other
  related modes. If it has its own syntax table, it should store this in
  a variable named `modename-mode-syntax-table`. See [Syntax Tables](#Syntax-Tables). (Major modes based on the tree-sitter library use the parsers
  provided by tree-sitter for this, see [Parser-based Font Lock](#Parser_002dbased-Font-Lock).)
* If the mode handles a language that has a syntax for comments, it should
  set the variables that define the comment syntax. See [Options Controlling Comments](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Options-for-Comments) in The GNU Emacs Manual.
* The mode may have its own abbrev table or may share one with other
  related modes. If it has its own abbrev table, it should store this
  in a variable named `modename-mode-abbrev-table`. If the
  major mode command defines any abbrevs itself, it should pass `t`
  for the system-flag argument to `define-abbrev`.
  See [Defining Abbrevs](#Defining-Abbrevs).
* The mode should specify how to do highlighting for Font Lock mode, by
  setting up a buffer-local value for the variable
  `font-lock-defaults` (see [Font Lock Mode](#Font-Lock-Mode)). For a major mode
  based on tree-sitter, see [Parser-based Font Lock](#Parser_002dbased-Font-Lock).
* Each face that the mode defines should, if possible, inherit from an
  existing Emacs face. See [Basic Faces](#Basic-Faces), and [Faces for Font Lock](#Faces-for-Font-Lock).
* Consider adding a mode-specific menu to the menu bar. This should
  preferably include the most important menu-specific settings and
  commands that will allow users discovering the main features quickly
  and efficiently.
* Consider adding mode-specific context menus for the mode, to be used
  if and when users activate the `context-menu-mode` (see [Menu
  Mouse Clicks](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Menu-Mouse-Clicks) in The Emacs Manual). To this end, define a
  mode-specific function which builds one or more menus depending on the
  location of the `mouse-3` click in the buffer, and then add that
  function to the buffer-local value of `context-menu-functions`.
* The mode should specify how Imenu should find the definitions or
  sections of a buffer, by setting up a buffer-local value for the
  variable `imenu-generic-expression`, for the two variables
  `imenu-prev-index-position-function` and
  `imenu-extract-index-name-function`, or for the variable
  `imenu-create-index-function` (see [Imenu](#Imenu)).
* The mode should specify how Outline minor mode should find the
  heading lines, by setting up a buffer-local value for the variables
  `outline-regexp` or `outline-search-function`, and also
  for the variable `outline-level` (see [Outline Minor Mode](#Outline-Minor-Mode)).
* The mode can tell ElDoc mode how to retrieve different types of
  documentation for whatever is at point, by adding one or more
  buffer-local entries to the special hook
  `eldoc-documentation-functions`.
* The mode can specify how to complete various keywords by adding one or
  more buffer-local entries to the special hook
  `completion-at-point-functions`. See [Completion in Ordinary Buffers](#Completion-in-Buffers).
* To make a buffer-local binding for an Emacs customization variable, use
  `make-local-variable` in the major mode command, not
  `make-variable-buffer-local`. The latter function would make the
  variable local to every buffer in which it is subsequently set, which
  would affect buffers that do not use this mode. It is undesirable for a
  mode to have such global effects. See [Buffer-Local Variables](#Buffer_002dLocal-Variables).

  With rare exceptions, the only reasonable way to use
  `make-variable-buffer-local` in a Lisp package is for a variable
  which is used only within that package. Using it on a variable used by
  other packages would interfere with them.
* Each major mode should have a normal *mode hook* named
  `modename-mode-hook`. The very last thing the major mode command
  should do is to call `run-mode-hooks`. This runs the normal
  hook `change-major-mode-after-body-hook`, the mode hook, the
  function `hack-local-variables` (when the buffer is visiting a file),
  and then the normal hook `after-change-major-mode-hook`.
  See [Mode Hooks](#Mode-Hooks).
* The major mode command may start by calling some other major mode
  command (called the *parent mode*) and then alter some of its
  settings. A mode that does this is called a *derived mode*. The
  recommended way to define one is to use the `define-derived-mode`
  macro, but this is not required. Such a mode should call the parent
  mode command inside a `delay-mode-hooks` form. (Using
  `define-derived-mode` does this automatically.) See [Defining Derived Modes](#Derived-Modes), and [Mode Hooks](#Mode-Hooks).
* If something special should be done if the user switches a buffer from
  this mode to any other major mode, this mode can set up a buffer-local
  value for `change-major-mode-hook` (see [Creating and Deleting Buffer-Local Bindings](#Creating-Buffer_002dLocal)).
* If this mode is appropriate only for specially-prepared text produced by
  the mode itself (rather than by the user typing at the keyboard or by an
  external file), then the major mode command symbol should have a
  property named `mode-class` with value `special`, put on as
  follows:

  ```
  (put 'funny-mode 'mode-class 'special)
  ```

  This tells Emacs that new buffers created while the current buffer is in
  Funny mode should not be put in Funny mode, even though the default
  value of `major-mode` is `nil`. By default, the value of
  `nil` for `major-mode` means to use the current buffer’s major
  mode when creating new buffers (see [How Emacs Chooses a Major Mode](#Auto-Major-Mode)), but with such
  `special` modes, Fundamental mode is used instead. Modes such as
  Dired, Rmail, and Buffer List use this feature.

  The function `view-buffer` does not enable View mode in buffers
  whose mode-class is special, because such modes usually provide their
  own View-like bindings.

  The `define-derived-mode` macro automatically marks the derived
  mode as special if the parent mode is special. Special mode is a
  convenient parent for such modes to inherit from; See [Basic Major Modes](#Basic-Major-Modes).
* If you want to make the new mode the default for files with certain
  recognizable names, add an element to `auto-mode-alist` to select
  the mode for those file names (see [How Emacs Chooses a Major Mode](#Auto-Major-Mode)). If you
  define the mode command to autoload, you should add this element in
  the same file that calls `autoload`. If you use an autoload
  cookie for the mode command, you can also use an autoload cookie for
  the form that adds the element (see [autoload cookie](#autoload-cookie)). If you do
  not autoload the mode command, it is sufficient to add the element in
  the file that contains the mode definition.
* The top-level forms in the file defining the mode should be written so
  that they may be evaluated more than once without adverse consequences.
  For instance, use `defvar` or `defcustom` to set mode-related
  variables, so that they are not reinitialized if they already have a
  value (see [Defining Global Variables](#Defining-Variables)).

---

Next: [Getting Help about a Major Mode](#Mode-Help), Previous: [Major Mode Conventions](#Major-Mode-Conventions), Up: [Major Modes](#Major-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.2.2 How Emacs Chooses a Major Mode [¶](#How-Emacs-Chooses-a-Major-Mode)

When Emacs visits a file, it automatically selects a major mode for
the buffer based on information in the file name or in the file itself.
It also processes local variables specified in the file text.

Command: **normal-mode** &optional find-file [¶](#index-normal_002dmode)
:   This function establishes the proper major mode and buffer-local
    variable bindings for the current buffer. It calls
    `set-auto-mode` (see below). As of Emacs 26.1, it no longer
    runs `hack-local-variables`, this now being done in
    `run-mode-hooks` at the initialization of major modes
    (see [Mode Hooks](#Mode-Hooks)).

    If the find-file argument to `normal-mode` is non-`nil`,
    `normal-mode` assumes that the `find-file` function is calling
    it. In this case, it may process local variables in the ‘`-*-`’
    line or at the end of the file. The variable
    `enable-local-variables` controls whether to do so. See [Local Variables in Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#File-Variables) in The GNU Emacs Manual,
    for the syntax of the local variables section of a file.

    If you run `normal-mode` interactively, the argument
    find-file is normally `nil`. In this case,
    `normal-mode` unconditionally processes any file local variables.

    The function calls `set-auto-mode` to choose and set a major
    mode. If this does not specify a mode, the buffer stays in the major
    mode determined by the default value of `major-mode` (see below).

    `normal-mode` uses `condition-case` around the call to the
    major mode command, so errors are caught and reported as a ‘`File
    mode specification error`’, followed by the original error message.

Function: **set-auto-mode** &optional keep-mode-if-same [¶](#index-set_002dauto_002dmode)
:   This function selects and sets the major mode that is appropriate
    for the current buffer. It bases its decision (in order of
    precedence) on the ‘`-*-`’ line, on any ‘`mode:`’ local
    variable near the end of a file, on the ‘`#!`’ line (using
    `interpreter-mode-alist`), on the text at the beginning of the
    buffer (using `magic-mode-alist`), and finally on the visited
    file name (using `auto-mode-alist`). See [How
    Major Modes are Chosen](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Choosing-Modes) in The GNU Emacs Manual. If
    `enable-local-variables` is `nil`, `set-auto-mode` does
    not check the ‘`-*-`’ line, or near the end of the file, for
    any mode tag.

    There are some file types where it is not appropriate to scan the file
    contents for a mode specifier. For example, a tar archive may happen to
    contain, near the end of the file, a member file that has a local
    variables section specifying a mode for that particular file. This
    should not be applied to the containing tar file. Similarly, a tiff
    image file might just happen to contain a first line that seems to
    match the ‘`-*-`’ pattern. For these reasons, both these file
    extensions are members of the list `inhibit-local-variables-regexps`.
    Add patterns to this list to prevent Emacs searching them for local
    variables of any kind (not just mode specifiers).

    If keep-mode-if-same is non-`nil`, this function does not
    call the mode command if the buffer is already in the proper major
    mode. For instance, `set-visited-file-name` sets this to
    `t` to avoid killing buffer local variables that the user may
    have set.

Function: **set-buffer-major-mode** buffer [¶](#index-set_002dbuffer_002dmajor_002dmode)
:   This function sets the major mode of buffer to the default value of
    `major-mode`; if that is `nil`, it uses the
    current buffer’s major mode (if that is suitable). As an exception,
    if buffer’s name is `*scratch*`, it sets the mode to
    `initial-major-mode`.

    The low-level primitives for creating buffers do not use this function,
    but medium-level commands such as `switch-to-buffer` and
    `find-file-noselect` use it whenever they create buffers.

User Option: **initial-major-mode** [¶](#index-initial_002dmajor_002dmode)
:   The value of this variable determines the major mode of the initial
    `*scratch*` buffer. The value should be a symbol that is a major
    mode command. The default value is `lisp-interaction-mode`.

Variable: **interpreter-mode-alist** [¶](#index-interpreter_002dmode_002dalist)
:   This variable specifies major modes to use for scripts that specify a
    command interpreter in a ‘`#!`’ line. Its value is an alist with
    elements of the form `(regexp . mode)`; this says to
    use mode mode if the file specifies an interpreter which matches
    `` \\`regexp\\' ``. For example, one of the default elements
    is `("python[0-9.]*" . python-mode)`.

Variable: **magic-mode-alist** [¶](#index-magic_002dmode_002dalist)
:   This variable’s value is an alist with elements of the form
    `(regexp . function)`, where regexp is a
    regular expression and function is a function or `nil`.
    After visiting a file, `set-auto-mode` calls function if
    the text at the beginning of the buffer matches regexp and
    function is non-`nil`; if function is `nil`,
    `auto-mode-alist` gets to decide the mode.

Variable: **magic-fallback-mode-alist** [¶](#index-magic_002dfallback_002dmode_002dalist)
:   This works like `magic-mode-alist`, except that it is handled
    only if `auto-mode-alist` does not specify a mode for this file.

Variable: **auto-mode-alist** [¶](#index-auto_002dmode_002dalist)
:   This variable contains an association list of file name patterns
    (regular expressions) and corresponding major mode commands. Usually,
    the file name patterns test for suffixes, such as ‘`.el`’ and
    ‘`.c`’, but this need not be the case. An ordinary element of the
    alist looks like `(regexp . mode-function)`.

    For example,

    ```
    (("\\`/tmp/fol/" . text-mode)
     ("\\.texinfo\\'" . texinfo-mode)
     ("\\.texi\\'" . texinfo-mode)
    ```

    ```
     ("\\.el\\'" . emacs-lisp-mode)
     ("\\.c\\'" . c-mode)
     ("\\.h\\'" . c-mode)
     ...)
    ```

    When you visit a file whose expanded file name (see [Functions that Expand Filenames](#File-Name-Expansion)), with version numbers and backup suffixes removed using
    `file-name-sans-versions` (see [File Name Components](#File-Name-Components)), matches
    a regexp, `set-auto-mode` calls the corresponding
    mode-function. This feature enables Emacs to select the proper
    major mode for most files.

    If an element of `auto-mode-alist` has the form `(regexp
    function t)`, then after calling function, Emacs searches
    `auto-mode-alist` again for a match against the portion of the file
    name that did not match before. This feature is useful for
    uncompression packages: an entry of the form `("\\.gz\\'"
    function t)` can uncompress the file and then put the uncompressed
    file in the proper mode according to the name sans ‘`.gz`’.

    If `auto-mode-alist` has more than one element whose regexp
    matches the file name, Emacs will use the first match.

    Here is an example of how to prepend several pattern pairs to
    `auto-mode-alist`. (You might use this sort of expression in your
    init file.)

    ```
    (setq auto-mode-alist
      (append
       ;; File name (within directory) starts with a dot.
       '(("/\\.[^/]*\\'" . fundamental-mode)
         ;; File name has no dot.
         ("/[^\\./]*\\'" . fundamental-mode)
         ;; File name ends in ‘.C’.
         ("\\.C\\'" . c++-mode))
       auto-mode-alist))
    ```

Variable: **major-mode-remap-defaults** [¶](#index-major_002dmode_002dremap_002ddefaults)
:   This variable contains an association list indicating which function
    to call to activate a given major mode. This is used for file formats
    that can be supported by various major modes, where this variable can be
    used to indicate which alternative should be used by default.

    For example, a third-party package providing a much improved Pascal
    major mode, can use the following to tell `normal-mode` to use
    `spiffy-pascal-mode` for all the files that would normally use `pascal-mode`:

    ```
    (add-to-list 'major-mode-remap-defaults '(pascal-mode . spiffy-pascal-mode))
    ```

    This variable has the same format as `major-mode-remap-alist`.
    If both lists match a major mode, the entry in
    `major-mode-remap-alist` takes precedence.

Function: **major-mode-remap** mode [¶](#index-major_002dmode_002dremap)
:   This function returns the major mode to use instead of mode
    according to `major-mode-remap-alist` and
    `major-mode-remap-defaults`. It returns mode if the mode
    is not remapped by those variables.

    When a package wants to activate a major mode for a particular file
    format, it should use this function, passing as `mode` argument the
    canonical major mode for that file format, to find which specific major
    mode to activate, so as to take into account the user’s preferences.

---

Next: [Defining Derived Modes](#Derived-Modes), Previous: [How Emacs Chooses a Major Mode](#Auto-Major-Mode), Up: [Major Modes](#Major-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.2.3 Getting Help about a Major Mode [¶](#Getting-Help-about-a-Major-Mode)

The `describe-mode` function provides information about major
modes. It is normally bound to `C-h m`. It uses the value of the
variable `major-mode` (see [Major Modes](#Major-Modes)), which is why every
major mode command needs to set that variable.

Command: **describe-mode** &optional buffer [¶](#index-describe_002dmode)
:   This command displays the documentation of the current buffer’s major
    mode and minor modes. It uses the `documentation` function to
    retrieve the documentation strings of the major and minor mode
    commands (see [Access to Documentation Strings](#Accessing-Documentation)).

    If called from Lisp with a non-`nil` buffer argument, this
    function displays the documentation for that buffer’s major and minor
    modes, rather than those of the current buffer.

---

Next: [Basic Major Modes](#Basic-Major-Modes), Previous: [Getting Help about a Major Mode](#Mode-Help), Up: [Major Modes](#Major-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.2.4 Defining Derived Modes [¶](#Defining-Derived-Modes)

The recommended way to define a new major mode is to derive it from an
existing one using `define-derived-mode`. If there is no closely
related mode, you should inherit from either `text-mode`,
`special-mode`, or `prog-mode`. See [Basic Major Modes](#Basic-Major-Modes). If
none of these are suitable, you can inherit from `fundamental-mode`
(see [Major Modes](#Major-Modes)).

Macro: **define-derived-mode** variant parent name docstring keyword-args… body… [¶](#index-define_002dderived_002dmode)
:   This macro defines variant as a major mode command, using
    name as the string form of the mode name. variant and
    parent should be unquoted symbols.

    The new command variant is defined to call the function
    parent, then override certain aspects of that parent mode:

    * The new mode has its own sparse keymap, named
      `variant-map`. `define-derived-mode`
      makes the parent mode’s keymap the parent of the new map, unless
      `variant-map` is already set and already has a parent.
    * The new mode has its own syntax table, kept in the variable
      `variant-syntax-table`, unless you override this using the
      `:syntax-table` keyword (see below). `define-derived-mode`
      makes the parent mode’s syntax-table the parent of
      `variant-syntax-table`, unless the latter is already set
      and already has a parent different from the standard syntax table.
    * The new mode has its own abbrev table, kept in the variable
      `variant-abbrev-table`, unless you override this using the
      `:abbrev-table` keyword (see below).
    * The new mode has its own mode hook, `variant-hook`. It
      runs this hook, after running the hooks of its ancestor modes, with
      `run-mode-hooks`, as the last thing it does, apart from running
      any `:after-hook` form it may have. See [Mode Hooks](#Mode-Hooks).

    In addition, you can specify how to override other aspects of
    parent with body. The command variant
    evaluates the forms in body after setting up all its usual
    overrides, just before running the mode hooks.

    If parent has a non-`nil` `mode-class` symbol
    property, then `define-derived-mode` sets the `mode-class`
    property of variant to the same value. This ensures, for
    example, that if parent is a special mode, then variant is
    also a special mode (see [Major Mode Conventions](#Major-Mode-Conventions)).

    You can also specify `nil` for parent. This gives the new
    mode no parent. Then `define-derived-mode` behaves as described
    above, but, of course, omits all actions connected with parent.
    Conversely, you can use `derived-mode-set-parent` and
    `derived-mode-add-parents`, described below, to explicitly set
    the ancestry of the new mode.

    The argument docstring specifies the documentation string for the
    new mode. `define-derived-mode` adds some general information
    about the mode’s hook, followed by the mode’s keymap, at the end of this
    documentation string. If you omit docstring,
    `define-derived-mode` generates a documentation string.

    The keyword-args are pairs of keywords and values. The values,
    except for `:after-hook`’s, are evaluated. The following
    keywords are currently supported:

    `:syntax-table`
    :   You can use this to explicitly specify a syntax table for the new
        mode. If you specify a `nil` value, the new mode uses the same
        syntax table as parent, or the standard syntax table if
        parent is `nil`. (Note that this does *not* follow
        the convention used for non-keyword arguments that a `nil` value
        is equivalent with not specifying the argument.)

    `:abbrev-table`
    :   You can use this to explicitly specify an abbrev table for the new
        mode. If you specify a `nil` value, the new mode uses the same
        abbrev table as parent, or `fundamental-mode-abbrev-table`
        if parent is `nil`. (Again, a `nil` value is
        *not* equivalent to not specifying this keyword.)

    `:interactive`
    :   Modes are interactive commands by default. If you specify a
        `nil` value, the mode defined here won’t be interactive. This is
        useful for modes that are never meant to be activated by users
        manually, but are only supposed to be used in some specially-formatted
        buffer.

    `:group`
    :   If this is specified, the value should be the customization group for
        this mode. (Not all major modes have one.) The command
        `customize-mode` uses this. `define-derived-mode` does
        *not* automatically define the specified customization group.

    `:after-hook`
    :   This optional keyword specifies a single Lisp form to evaluate as the
        final act of the mode function, after the mode hooks have been run.
        It should not be quoted. Since the form might be evaluated after the
        mode function has terminated, it should not access any element of the
        mode function’s local state. An `:after-hook` form is useful for
        setting up aspects of the mode which depend on the user’s settings,
        which in turn may have been changed in a mode hook.

    Here is a hypothetical example:

    ```
    (defvar-keymap hypertext-mode-map
      "<down-mouse-3>" #'do-hyper-link)

    (define-derived-mode hypertext-mode
      text-mode "Hypertext"
      "Major mode for hypertext."
      (setq-local case-fold-search nil))
    ```

    Do not write an `interactive` spec in the definition;
    `define-derived-mode` does that automatically.

Function: **derived-mode-p** modes [¶](#index-derived_002dmode_002dp)
:   This function returns non-`nil` if the current major mode is
    derived from any of the major modes given by the list of symbols
    in modes.
    Instead of a list, modes can also be a single mode symbol.

    Furthermore, we still support a deprecated calling convention where the
    modes were passed as separate arguments.

    When examining the parent modes of the current major mode, this
    function takes into consideration the current mode’s parents set by
    `define-derived-mode`, and also its additional parents set by
    `derived-mode-add-parents`, described below.

Function: **provided-mode-derived-p** mode modes [¶](#index-provided_002dmode_002dderived_002dp)
:   This function returns non-`nil` if mode is derived from any
    of the major modes given by the list of symbols in modes. Like
    with `derived-mode-p`, modes can also be a single symbol,
    and this function also supports a deprecated calling convention where
    the modes were passed as separate symbol arguments.

    When examining the parent modes of mode, this function takes
    into consideration the parents of mode set by
    `define-derived-mode`, and also its additional parents set by
    `derived-mode-add-parents`, described below.

The graph of a major mode’s ancestry can be accessed and modified with
the following lower-level functions:

Function: **derived-mode-set-parent** mode parent [¶](#index-derived_002dmode_002dset_002dparent)
:   This function declares that mode inherits from `parent`.
    This is the function that `define-derived-mode` calls after
    defining mode to register the fact that mode was defined
    by reusing `parent`.

Function: **derived-mode-add-parents** mode extra-parents [¶](#index-derived_002dmode_002dadd_002dparents)
:   This function makes it possible to register additional parents beside
    the one that was used when defining mode. This can be used when
    the similarity between mode and the modes in extra-parents
    is such that it makes sense to treat mode as a child of those
    modes for purposes like applying directory-local variables and other
    mode-specific settings. The additional parent modes are specified as
    a list of symbols in extra-parents. Those additional parent
    modes will be considered as one of the modes parents by
    `derived-mode-p` and `provided-mode-derived-p`.

Function: **derived-mode-all-parents** mode [¶](#index-derived_002dmode_002dall_002dparents)
:   This function returns the list of all the modes in the ancestry of
    mode, ordered from the most specific to the least specific, and
    starting with mode itself. This includes the additional parent
    modes, if any, added by calling `derived-mode-add-parents`.

---

Next: [Mode Hooks](#Mode-Hooks), Previous: [Defining Derived Modes](#Derived-Modes), Up: [Major Modes](#Major-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.2.5 Basic Major Modes [¶](#Basic-Major-Modes-1)

Apart from Fundamental mode, there are three major modes that other
major modes commonly derive from: Text mode, Prog mode, and Special
mode. While Text mode is useful in its own right (e.g., for editing
files ending in `.txt`), Prog mode and Special mode exist mainly to
let other modes derive from them.

As far as possible, new major modes should be derived, either directly
or indirectly, from one of these three modes. One reason is that this
allows users to customize a single mode hook
(e.g., `prog-mode-hook`) for an entire family of relevant modes
(e.g., all programming language modes).

Command: **text-mode** [¶](#index-text_002dmode)
:   Text mode is a major mode for editing human languages. It defines the
    ‘`"`’ and ‘`\`’ characters as having punctuation syntax
    (see [Table of Syntax Classes](#Syntax-Class-Table)), and arranges for
    `completion-at-point` to complete words based on the spelling
    dictionary (see [Completion in Ordinary Buffers](#Completion-in-Buffers)).

    An example of a major mode derived from Text mode is HTML mode.
    See [SGML and HTML Modes](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#HTML-Mode) in The GNU Emacs Manual.

Command: **prog-mode** [¶](#index-prog_002dmode)
:   Prog mode is a basic major mode for buffers containing programming
    language source code. Most of the programming language major modes
    built into Emacs are derived from it.

    Prog mode binds `parse-sexp-ignore-comments` to `t`
    (see [Motion Commands Based on Parsing](#Motion-via-Parsing)) and `bidi-paragraph-direction` to
    `left-to-right` (see [Bidirectional Display](#Bidirectional-Display)).

Command: **special-mode** [¶](#index-special_002dmode)
:   Special mode is a basic major mode for buffers containing text that is
    produced specially by Emacs, rather than directly from a file. Major
    modes derived from Special mode are given a `mode-class` property
    of `special` (see [Major Mode Conventions](#Major-Mode-Conventions)).

    Special mode sets the buffer to read-only. Its keymap defines several
    common bindings, including `q` for `quit-window` and `g`
    for `revert-buffer` (see [Reverting](#Reverting)).

    An example of a major mode derived from Special mode is Buffer Menu
    mode, which is used by the `*Buffer List*` buffer. See [Listing Existing Buffers](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#List-Buffers) in The GNU Emacs Manual.

In addition, modes for buffers of tabulated data can inherit from
Tabulated List mode, which is in turn derived from Special mode.
See [Tabulated List mode](#Tabulated-List-Mode).

---

Next: [Tabulated List mode](#Tabulated-List-Mode), Previous: [Basic Major Modes](#Basic-Major-Modes), Up: [Major Modes](#Major-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.2.6 Mode Hooks [¶](#Mode-Hooks-1)

Every major mode command should finish by running the mode-independent
normal hook `change-major-mode-after-body-hook`, its mode hook,
and the normal hook `after-change-major-mode-hook`.
It does this by calling `run-mode-hooks`. If the major mode is a
derived mode, that is if it calls another major mode (the parent mode)
in its body, it should do this inside `delay-mode-hooks` so that
the parent won’t run these hooks itself. Instead, the derived mode’s
call to `run-mode-hooks` runs the parent’s mode hook too.
See [Major Mode Conventions](#Major-Mode-Conventions).

Emacs versions before Emacs 22 did not have `delay-mode-hooks`.
Versions before 24 did not have `change-major-mode-after-body-hook`.
When user-implemented major modes do not use `run-mode-hooks` and
have not been updated to use these newer features, they won’t entirely
follow these conventions: they may run the parent’s mode hook too early,
or fail to run `after-change-major-mode-hook`. This will
have undesirable effects such as preventing minor modes defined
with `define-globalized-minor-mode` from being enabled in
buffers using these major modes. If you encounter
such a major mode, please correct it to follow these conventions.

When you define a major mode using `define-derived-mode`, it
automatically makes sure these conventions are followed. If you
define a major mode “by hand”, not using `define-derived-mode`,
use the following functions to handle these conventions automatically.

Function: **run-mode-hooks** &rest hookvars [¶](#index-run_002dmode_002dhooks)
:   Major modes should run their mode hook using this function. It is
    similar to `run-hooks` (see [Hooks](#Hooks)), but it also runs
    `change-major-mode-after-body-hook`, `hack-local-variables`
    (when the buffer is visiting a file) (see [File Local Variables](#File-Local-Variables)),
    and `after-change-major-mode-hook`. The last thing it does is to
    evaluate any `:after-hook` forms declared by parent modes
    (see [Defining Derived Modes](#Derived-Modes)).

    When this function is called during the execution of a
    `delay-mode-hooks` form, it does not run the hooks or
    `hack-local-variables` or evaluate the forms immediately.
    Instead, it arranges for the next call to `run-mode-hooks` to run
    them.

Macro: **delay-mode-hooks** body… [¶](#index-delay_002dmode_002dhooks)
:   When one major mode command calls another, it should do so inside of
    `delay-mode-hooks`.

    This macro executes body, but tells all `run-mode-hooks`
    calls during the execution of body to delay running their hooks.
    The hooks will actually run during the next call to
    `run-mode-hooks` after the end of the `delay-mode-hooks`
    construct.

Variable: **change-major-mode-after-body-hook** [¶](#index-change_002dmajor_002dmode_002dafter_002dbody_002dhook)
:   This is a normal hook run by `run-mode-hooks`. It is run before
    the mode hooks.

Variable: **after-change-major-mode-hook** [¶](#index-after_002dchange_002dmajor_002dmode_002dhook)
:   This is a normal hook run by `run-mode-hooks`. It is run at the
    very end of every properly-written major mode command.

---

Next: [Generic Modes](#Generic-Modes), Previous: [Mode Hooks](#Mode-Hooks), Up: [Major Modes](#Major-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.2.7 Tabulated List mode [¶](#Tabulated-List-mode)

Tabulated List mode is a major mode for displaying tabulated data,
i.e., data consisting of *entries*, each entry occupying one row
of text with its contents divided into columns. Tabulated List mode
provides facilities for pretty-printing rows and columns, and sorting
the rows according to the values in each column. It is derived from
Special mode (see [Basic Major Modes](#Basic-Major-Modes)).

Tabulated List mode is geared towards displaying text using
monospaced fonts, using a single font and text size. If you want to
display a table using variable pitch fonts or images,
`make-vtable` can be used instead. vtable also support having
more than a single table in a buffer, or having a buffer that contains
both a table and additional text in it. See [(vtable)Introduction](vtable.html#Introduction),
for more information.

Tabulated List mode is intended to be used as a parent mode by a more
specialized major mode. Examples include Process Menu mode
(see [Process Information](#Process-Information)) and Package Menu mode (see [Package
Menu](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Package-Menu) in The GNU Emacs Manual).

Such a derived mode should use `define-derived-mode` in the usual
way, specifying `tabulated-list-mode` as the second argument
(see [Defining Derived Modes](#Derived-Modes)). The body of the `define-derived-mode`
form should specify the format of the tabulated data, by assigning
values to the variables documented below; optionally, it can then call
the function `tabulated-list-init-header`, which will populate a
header with the names of the columns.

The derived mode should also define a *listing command*. This,
not the mode command, is what the user calls (e.g., `M-x
list-processes`). The listing command should create or switch to a
buffer, turn on the derived mode, specify the tabulated data, and
finally call `tabulated-list-print` to populate the buffer.

User Option: **tabulated-list-gui-sort-indicator-asc** [¶](#index-tabulated_002dlist_002dgui_002dsort_002dindicator_002dasc)
:   This variable specifies the character to be used on GUI frames as an
    indication that the column is sorted in the ascending order.

    Whenever you change the sort direction in Tabulated List buffers, this
    indicator toggles between ascending (“asc”) and descending (“desc”).

User Option: **tabulated-list-gui-sort-indicator-desc** [¶](#index-tabulated_002dlist_002dgui_002dsort_002dindicator_002ddesc)
:   Like `tabulated-list-gui-sort-indicator-asc`, but used when the
    column is sorted in the descending order.

User Option: **tabulated-list-tty-sort-indicator-asc** [¶](#index-tabulated_002dlist_002dtty_002dsort_002dindicator_002dasc)
:   Like `tabulated-list-gui-sort-indicator-asc`, but used for
    text-mode frames.

User Option: **tabulated-list-tty-sort-indicator-desc** [¶](#index-tabulated_002dlist_002dtty_002dsort_002dindicator_002ddesc)
:   Like `tabulated-list-tty-sort-indicator-asc`, but used when the
    column is sorted in the descending order.

Variable: **tabulated-list-format** [¶](#index-tabulated_002dlist_002dformat)
:   This buffer-local variable specifies the format of the Tabulated List
    data. Its value should be a vector. Each element of the vector
    represents a data column, and should be a list `(name
    width sort . props)`, where

    * name is the column’s name (a string).
    * width is the width to reserve for the column (an integer). This
      is meaningless for the last column, which runs to the end of each line.
    * sort specifies how to sort entries by the column. If `nil`,
      the column cannot be used for sorting. If `t`, the column is
      sorted by comparing string values. Otherwise, this should be a
      predicate function for `sort` (see [Functions that Rearrange Lists](#Rearrangement)), which
      accepts two arguments with the same form as the elements of
      `tabulated-list-entries` (see below).
    * props is a plist (see [Property Lists](#Property-Lists)) of additional column
      properties. If the value of the property `:right-align` is
      non-`nil` then the column should be right-aligned. And the
      property `:pad-right` specifies the number of additional padding
      spaces to the right of the column (by default 1 if omitted).

Variable: **tabulated-list-entries** [¶](#index-tabulated_002dlist_002dentries)
:   This buffer-local variable specifies the entries displayed in the
    Tabulated List buffer. Its value should be either a list, or a
    function.

    If the value is a list, each list element corresponds to one entry, and
    should have the form `(id contents)`, where

    * id is either `nil`, or a Lisp object that identifies the
      entry. If the latter, the cursor stays on the same entry when
      re-sorting entries. Comparison is done with `equal`.
    * contents is a vector with the same number of elements as
      `tabulated-list-format`. Each vector element is either a string,
      which is inserted into the buffer as-is; an image descriptor, which is
      used to insert an image (see [Image Descriptors](#Image-Descriptors)); or a list
      `(label . properties)`, which means to insert a
      text button by calling `insert-text-button` with label and
      properties as arguments (see [Making Buttons](#Making-Buttons)).

      There should be no newlines in any of these strings.

    Otherwise, the value should be a function which returns a list of the
    above form when called with no arguments.

Variable: **tabulated-list-groups** [¶](#index-tabulated_002dlist_002dgroups)
:   This buffer-local variable specifies the groups of entries displayed in
    the Tabulated List buffer. Its value should be either a list or a
    function.

    If the value is a list, each list element corresponds to one group, and
    should have the form
    `(group-name entry1 entry2 …)`, where
    group-name is a string inserted before all group entries, and
    entry1, entry2 and so on each have the same format as an
    element of `tabulated-list-entries` (see above).

    Otherwise, the value should be a function which returns a list of the
    above form when called with no arguments.

    You can use `seq-group-by` to create `tabulated-list-groups`
    from `tabulated-list-entries`. For example:

    ```
    (setq tabulated-list-groups
          (seq-group-by 'Buffer-menu-group-by-mode
                        tabulated-list-entries))
    ```

    where you can define `Buffer-menu-group-by-mode` like this:

    ```
    (defun Buffer-menu-group-by-mode (entry)
      (concat "* " (aref (cadr entry) 5)))
    ```

Variable: **tabulated-list-revert-hook** [¶](#index-tabulated_002dlist_002drevert_002dhook)
:   This normal hook is run prior to reverting a Tabulated List buffer. A
    derived mode can add a function to this hook to recompute
    `tabulated-list-entries`.

Variable: **tabulated-list-printer** [¶](#index-tabulated_002dlist_002dprinter)
:   The value of this variable is the function called to insert an entry at
    point, including its terminating newline. The function should accept
    two arguments, id and contents, having the same meanings as
    in `tabulated-list-entries`. The default value is a function which
    inserts an entry in a straightforward way; a mode which uses Tabulated
    List mode in a more complex way can specify another function.

Variable: **tabulated-list-sort-key** [¶](#index-tabulated_002dlist_002dsort_002dkey)
:   The value of this variable specifies the current sort key for the
    Tabulated List buffer. If it is `nil`, no sorting is done.
    Otherwise, it should have the form `(name . flip)`,
    where name is a string matching one of the column names in
    `tabulated-list-format`, and flip, if non-`nil`, means
    to invert the sort order.

Function: **tabulated-list-init-header** [¶](#index-tabulated_002dlist_002dinit_002dheader)
:   This function computes and sets `header-line-format` for the
    Tabulated List buffer (see [Window Header Lines](#Header-Lines)), and assigns a keymap to
    the header line to allow sorting entries by clicking on column headers.

    Modes derived from Tabulated List mode should call this after setting
    the above variables (in particular, only after setting
    `tabulated-list-format`).

Function: **tabulated-list-print** &optional remember-pos update [¶](#index-tabulated_002dlist_002dprint)
:   This function populates the current buffer with entries. It should be
    called by the listing command. It erases the buffer, sorts the entries
    specified by `tabulated-list-entries` according to
    `tabulated-list-sort-key`, then calls the function specified by
    `tabulated-list-printer` to insert each entry.

    If the optional argument remember-pos is non-`nil`, this
    function looks for the id element on the current line, if any, and
    tries to move to that entry after all the entries are (re)inserted.

    If the optional argument update is non-`nil`, this function
    will only erase or add entries that have changed since the last print.
    This is several times faster if most entries haven’t changed since the
    last time this function was called. The only difference in outcome is
    that tags placed via `tabulated-list-put-tag` will not be removed
    from entries that haven’t changed (normally all tags are removed).

Function: **tabulated-list-delete-entry** [¶](#index-tabulated_002dlist_002ddelete_002dentry)
:   This function deletes the entry at point.

    It returns a list `(id cols)`, where id is the
    ID of the deleted entry and cols is a vector of its column
    descriptors. It moves point to the beginning of the current line. It
    returns `nil` if there is no entry at point.

    Note that this function only changes the buffer contents; it does not
    alter `tabulated-list-entries`.

Function: **tabulated-list-get-id** &optional pos [¶](#index-tabulated_002dlist_002dget_002did)
:   This `defsubst` returns the ID object from
    `tabulated-list-entries` (if that is a list) or from the list
    returned by `tabulated-list-entries` (if it is a function). If
    omitted or `nil`, pos defaults to point.

Function: **tabulated-list-get-entry** &optional pos [¶](#index-tabulated_002dlist_002dget_002dentry)
:   This `defsubst` returns the entry object from
    `tabulated-list-entries` (if that is a list) or from the list
    returned by `tabulated-list-entries` (if it is a function). This
    will be a vector for the ID at pos. If there is no entry at
    pos, then the function returns `nil`.

Function: **tabulated-list-header-overlay-p** &optional POS [¶](#index-tabulated_002dlist_002dheader_002doverlay_002dp)
:   This `defsubst` returns non-`nil` if there is a fake header at
    pos. A fake header is used if
    `tabulated-list-use-header-line` is `nil` to put the column
    names at the beginning of the buffer. If omitted or `nil`,
    pos defaults to `point-min`.

Function: **tabulated-list-put-tag** tag &optional advance [¶](#index-tabulated_002dlist_002dput_002dtag)
:   This function puts tag in the padding area of the current line.
    The padding area can be empty space at the beginning of the line, the
    width of which is governed by `tabulated-list-padding`.
    tag should be a string, with a length less than or equal to
    `tabulated-list-padding`. If advance is non-`nil`, this
    function advances point by one line.

Function: **tabulated-list-clear-all-tags** [¶](#index-tabulated_002dlist_002dclear_002dall_002dtags)
:   This function clears all tags from the padding area in the current
    buffer.

Function: **tabulated-list-set-col** col desc &optional change-entry-data [¶](#index-tabulated_002dlist_002dset_002dcol)
:   This function changes the tabulated list entry at point, setting
    col to desc. col is the column number to change, or
    the name of the column to change. desc is the new column
    descriptor, which is inserted via `tabulated-list-print-col`.

    If change-entry-data is non-`nil`, this function modifies the
    underlying data (usually the column descriptor in the list
    `tabulated-list-entries`) by setting the column descriptor of the
    vector to `desc`.

---

Next: [Major Mode Examples](#Example-Major-Modes), Previous: [Tabulated List mode](#Tabulated-List-Mode), Up: [Major Modes](#Major-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.2.8 Generic Modes [¶](#Generic-Modes-1)

*Generic modes* are simple major modes with basic support for
comment syntax and Font Lock mode. To define a generic mode, use the
macro `define-generic-mode`. See the file `generic-x.el`
for some examples of the use of `define-generic-mode`.

Macro: **define-generic-mode** mode comment-list keyword-list font-lock-list auto-mode-list function-list &optional docstring [¶](#index-define_002dgeneric_002dmode)
:   This macro defines a generic mode command named mode (a symbol,
    not quoted). The optional argument docstring is the
    documentation for the mode command. If you do not supply it,
    `define-generic-mode` generates one by default.

    The argument comment-list is a list in which each element is
    either a character, a string of one or two characters, or a cons cell.
    A character or a string is set up in the mode’s syntax table as a
    comment starter. If the entry is a cons cell, the CAR is set
    up as a comment starter and the CDR as a comment ender.
    (Use `nil` for the latter if you want comments to end at the end
    of the line.) Note that the syntax table mechanism has limitations
    about what comment starters and enders are actually possible.
    See [Syntax Tables](#Syntax-Tables).

    The argument keyword-list is a list of keywords to highlight
    with `font-lock-keyword-face`. Each keyword should be a string.
    Meanwhile, font-lock-list is a list of additional expressions to
    highlight. Each element of this list should have the same form as an
    element of `font-lock-keywords`. See [Search-based Fontification](#Search_002dbased-Fontification).

    The argument auto-mode-list is a list of regular expressions to
    add to the variable `auto-mode-alist`. They are added by the execution
    of the `define-generic-mode` form, not by expanding the macro call.

    Finally, function-list is a list of functions for the mode
    command to call for additional setup. It calls these functions just
    before it runs the mode hook variable `mode-hook`.

---

Previous: [Generic Modes](#Generic-Modes), Up: [Major Modes](#Major-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.2.9 Major Mode Examples [¶](#Major-Mode-Examples)

Text mode is perhaps the simplest mode besides Fundamental mode.
Here are excerpts from `text-mode.el` that illustrate many of
the conventions listed above:

```
;; Create the syntax table for this mode.
(defvar text-mode-syntax-table
  (let ((st (make-syntax-table)))
    (modify-syntax-entry ?\" ".   " st)
    (modify-syntax-entry ?\\ ".   " st)
    ;; Add 'p' so M-c on 'hello' leads to 'Hello', not 'hello'.
    (modify-syntax-entry ?' "w p" st)
    ...
    st)
  "Syntax table used while in `text-mode'.")
```

Here is how the actual mode command is defined now:

```
(define-derived-mode text-mode nil "Text"
  "Major mode for editing text written for humans to read.
In this mode, paragraphs are delimited only by blank or white lines.
You can thus get the full benefit of adaptive filling
 (see the variable `adaptive-fill-mode').
\\{text-mode-map}
Turning on Text mode runs the normal hook `text-mode-hook'."
  (setq-local require-final-newline mode-require-final-newline))
```

The three Lisp modes (Lisp mode, Emacs Lisp mode, and Lisp Interaction
mode) have more features than Text mode and the code is correspondingly
more complicated. Here are excerpts from `lisp-mode.el` that
illustrate how these modes are written.

Here is how the Lisp mode syntax and abbrev tables are defined:

```
;; Create mode-specific table variables.
(define-abbrev-table 'lisp-mode-abbrev-table ()
  "Abbrev table for Lisp mode.")

(defvar lisp-mode-syntax-table
  (let ((table (make-syntax-table lisp--mode-syntax-table)))
    (modify-syntax-entry ?\[ "_   " table)
    (modify-syntax-entry ?\] "_   " table)
    (modify-syntax-entry ?# "' 14" table)
    (modify-syntax-entry ?| "\" 23bn" table)
    table)
  "Syntax table used in `lisp-mode'.")
```

The three modes for Lisp share much of their code. For instance,
Lisp mode and Emacs Lisp mode inherit from Lisp Data mode and Lisp
Interaction Mode inherits from Emacs Lisp mode.

Amongst other things, Lisp Data mode sets up the `comment-start`
variable to handle Lisp comments:

```
  (setq-local comment-start ";")
  ...
```

Each of the different Lisp modes has a slightly different keymap. For
example, Lisp mode binds `C-c C-z` to `run-lisp`, but the other
Lisp modes do not. However, all Lisp modes have some commands in
common. The following code sets up the common commands:

```
(defvar-keymap lisp-mode-shared-map
  :parent prog-mode-map
  :doc "Keymap for commands shared by all sorts of Lisp modes."
  "C-M-q" #'indent-sexp
  "DEL" #'backward-delete-char-untabify)
```

And here is the code to set up the keymap for Lisp mode:

```
(defvar-keymap lisp-mode-map
  :doc "Keymap for ordinary Lisp mode.
All commands in `lisp-mode-shared-map' are inherited by this map."
  :parent lisp-mode-shared-map
  "C-M-x" #'lisp-eval-defun
  "C-c C-z" #'run-lisp)
```

Finally, here is the major mode command for Lisp mode:

```
(define-derived-mode lisp-mode lisp-data-mode "Lisp"
  "Major mode for editing Lisp code for Lisps other than GNU Emacs Lisp.
Commands:
Delete converts tabs to spaces as it moves back.
Blank lines separate paragraphs.  Semicolons start comments.

\\{lisp-mode-map}
Note that `run-lisp' may be used either to start an inferior Lisp job
or to switch back to an existing one."
```

```
  (setq-local find-tag-default-function 'lisp-find-tag-default)
  (setq-local comment-start-skip
              "\\(\\(^\\|[^\\\n]\\)\\(\\\\\\\\\\)*\\)\\(;+\\|#|\\) *")
  (setq imenu-case-fold-search t))
```

---

Next: [Mode Line Format](#Mode-Line-Format), Previous: [Major Modes](#Major-Modes), Up: [Major and Minor Modes](#Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
