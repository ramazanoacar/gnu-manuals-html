### E.9 C Integer Types [¶](#C-Integer-Types-1)

Here are some guidelines for use of integer types in the Emacs C
source code. These guidelines sometimes give competing advice; common
sense is advised.

* Avoid arbitrary limits. For example, avoid `int len = strlen
  (s);` unless the length of `s` is required for other reasons to
  fit in `int` range.
* Do not assume that signed integer arithmetic wraps around on overflow.
  This is no longer true of Emacs porting targets: signed integer
  overflow has undefined behavior in practice, and can dump core or
  even cause earlier or later code to behave illogically. Unsigned
  overflow does wrap around reliably, modulo a power of two,
  if all operand types are unsigned and are `unsigned int` or wider.
* Use the macros of `<stdckdint.h>` to check for integer overflow
  or to implement wraparound arithmetic reliably with integer types
  that are signed or are narrower than `unsigned int`.
  Although `<stdckdint.h>` was not standardized until C23,
  on non-C23 platforms Emacs internally provides a fallback substitute.
  Avoid complex arguments to its macros `ckd_add`, `ckd_sub` and
  `ckd_mul`, as the fallback macros might evaluate arguments more than once.
* Prefer signed types to unsigned, as code gets confusing when signed
  and unsigned types are combined. Many other guidelines assume that
  types are signed; in the rarer cases where unsigned types are needed,
  similar advice may apply to the unsigned counterparts (e.g.,
  `size_t` instead of `ptrdiff_t`, or `uintptr_t` instead
  of `intptr_t`).
* Prefer `int` for Emacs character codes, in the range 0 .. 0x3FFFFF.
  More generally, prefer `int` for integers known to be in
  `int` range, e.g., screen column counts.
* Prefer `ptrdiff_t` for sizes, i.e., for integers bounded by the
  maximum size of any individual C object or by the maximum number of
  elements in any C array. This is part of Emacs’s general preference
  for signed types. Using `ptrdiff_t` limits objects to
  `PTRDIFF_MAX` bytes, but larger objects would cause trouble
  anyway since they would break pointer subtraction, so this does not
  impose an arbitrary limit.
* Avoid `ssize_t` except when communicating to low-level APIs that
  have `ssize_t`-related limitations. Although it’s equivalent to
  `ptrdiff_t` on typical platforms, `ssize_t` is occasionally
  narrower, so using it for size-related calculations could overflow.
  Also, `ptrdiff_t` is more ubiquitous and better-standardized, has
  standard `printf` formats, and is the basis for Emacs’s internal
  size-overflow checking. When using `ssize_t`, please note that
  POSIX requires support only for values in the range −1 ..
  `SSIZE_MAX`.
* Normally, prefer `intptr_t` for internal representations of pointers, or
  for integers bounded only by the number of objects that can exist at
  any given time or by the total number of bytes that can be allocated.
  However, prefer `uintptr_t` to represent pointer arithmetic that
  could cross page boundaries. For example, on a machine with a 32-bit
  address space an array could cross the 0x7fffffff/0x80000000 boundary,
  which would cause an integer overflow when adding 1 to
  `(intptr_t) 0x7fffffff`.
* Prefer the Emacs-defined type `EMACS_INT` for representing values
  converted to or from Emacs Lisp fixnums, as fixnum arithmetic is based
  on `EMACS_INT`.
* When representing a system value (such as a file size or a count of
  seconds since the Epoch), prefer the corresponding system type (e.g.,
  `off_t`, `time_t`). Do not assume that a system type is
  signed, unless this assumption is known to be safe. For example,
  although `off_t` is always signed, `time_t` need not be.
* Prefer `intmax_t` for representing values that might be any
  signed integer value in machine range.
  A `printf`-family function can print such a value
  via a format like `"%"PRIdMAX`.
* Prefer Emacs integers, which are either fixnums or bignums,
  for representing values that might be outside machine range.
  Although low level code uses GMP directly for efficiency,
  Emacs integers are typically more convenient at higher levels of
  abstraction.
* Prefer `bool`, `false` and `true` for booleans.
  Using `bool` can make programs easier to read and a bit faster than
  using `int`. Although it is also OK to use `int`, `0`
  and `1`, this older style is gradually being phased out. When
  using `bool`, respect the limitations of the replacement
  implementation of `bool`. In particular,
  boolean bitfields should be of type
  `bool_bf`, not `bool`, so that they work correctly even when
  compiling Objective C with standard GCC.
* In bitfields, prefer `unsigned int` or `signed int` to
  `int`, as `int` is less portable: it might be signed, and
  might not be. Single-bit bit fields should be `unsigned int` or
  `bool_bf` so that their values are 0 or 1.

---

Next: [Standard Keymaps](#Standard-Keymaps), Previous: [GNU Emacs Internals](#GNU-Emacs-Internals), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Appendix F Standard Errors [¶](#Standard-Errors-1)

Here is a list of the more important error symbols in standard Emacs, grouped
by concept. The list includes each symbol’s message and a cross reference
to a description of how the error can occur.

Each error symbol has a set of parent error conditions that is a
list of symbols. Normally this list includes the error symbol itself
and the symbol `error`. Occasionally it includes additional
symbols, which are intermediate classifications, narrower than
`error` but broader than a single error symbol. For example, all
the errors in accessing files have the condition `file-error`. If
we do not say here that a certain error symbol has additional error
conditions, that means it has none.

As a special exception, the error symbols `quit` and
`minibuffer-quit` don’t have the condition `error`, because
quitting is not considered an error.

Most of these error symbols are defined in C (mainly `data.c`),
but some are defined in Lisp. For example, the file `userlock.el`
defines the `file-locked` and `file-supersession` errors.
Several of the specialized Lisp libraries distributed with Emacs
define their own error symbols. We do not attempt to list of all
those here.

See [Errors](#Errors), for an explanation of how errors are generated and
handled.

`error`
:   The message is ‘`error`’. See [Errors](#Errors).

`quit`
:   The message is ‘`Quit`’. See [Quitting](#Quitting).

`minibuffer-quit`
:   The message is ‘`Quit`’. This is a subcategory of `quit`.
    See [Quitting](#Quitting).

`args-out-of-range`
:   The message is ‘`Args out of range`’. This happens when trying to
    access an element beyond the range of a sequence, buffer, or other
    container-like object. See [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors), and see
    [Text](#Text).

`arith-error`
:   The message is ‘`Arithmetic error`’. This occurs when trying to
    perform integer division by zero. See [Numeric Conversions](#Numeric-Conversions), and
    see [Arithmetic Operations](#Arithmetic-Operations).

`beginning-of-buffer`
:   The message is ‘`Beginning of buffer`’. See [Motion by Characters](#Character-Motion).

`buffer-read-only`
:   The message is ‘`Buffer is read-only`’. See [Read-Only Buffers](#Read-Only-Buffers).

`circular-list`
:   The message is ‘`List contains a loop`’. This happens when a
    circular structure is encountered. See [Read Syntax for Circular Objects](#Circular-Objects).

`cl-assertion-failed`
:   The message is ‘`Assertion failed`’. This happens when the
    `cl-assert` macro fails a test. See [Assertions](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Assertions) in Common Lisp
    Extensions.

`coding-system-error`
:   The message is ‘`Invalid coding system`’. See [Coding Systems in Lisp](#Lisp-and-Coding-Systems).

`cyclic-function-indirection`
:   The message is ‘`Symbol's chain of function indirections contains
    a loop`’. See [Symbol Function Indirection](#Function-Indirection).

`cyclic-variable-indirection`
:   The message is ‘`Symbol's chain of variable indirections contains
    a loop`’. See [Variable Aliases](#Variable-Aliases).

`dbus-error`
:   The message is ‘`D-Bus error`’. See [Errors and Events](https://www.gnu.org/software/emacs/manual/html_mono/dbus.html#Errors-and-Events) in D-Bus integration in Emacs.

`end-of-buffer`
:   The message is ‘`End of buffer`’. See [Motion by Characters](#Character-Motion).

`end-of-file`
:   The message is ‘`End of file during parsing`’. Note that this is
    not a subcategory of `file-error`, because it pertains to the
    Lisp reader, not to file I/O. See [Input Functions](#Input-Functions).

`file-already-exists`
:   This is a subcategory of `file-error`. See [Writing to Files](#Writing-to-Files).

`permission-denied`
:   This is a subcategory of `file-error`, which occurs when the OS
    doesn’t allow Emacs to access a file or a directory for some reason.

`file-date-error`
:   This is a subcategory of `file-error`. It occurs when
    `copy-file` tries and fails to set the last-modification time of
    the output file. See [Changing File Names and Attributes](#Changing-Files).

`file-error`
:   We do not list the error-strings of this error and its subcategories,
    because the error message is normally constructed from the data items
    alone when the error condition `file-error` is present. Thus,
    the error-strings are not very relevant. However, these error symbols
    do have `error-message` properties, and if no data is provided,
    the `error-message` property *is* used. See [Files](#Files).

`file-missing`
:   This is a subcategory of `file-error`. It occurs when an
    operation attempts to act on a file that is missing. See [Changing File Names and Attributes](#Changing-Files).

`compression-error`
:   This is a subcategory of `file-error`, which results from
    problems handling a compressed file. See [How Programs Do Loading](#How-Programs-Do-Loading).

`file-locked`
:   This is a subcategory of `file-error`. See [File Locks](#File-Locks).

`file-supersession`
:   This is a subcategory of `file-error`. See [Buffer Modification Time](#Modification-Time).

`file-notify-error`
:   This is a subcategory of `file-error`. It happens, when a file
    could not be watched for changes. See [Notifications on File Changes](#File-Notifications).

`remote-file-error`
:   This is a subcategory of `file-error`, which results from
    problems in accessing a remote file. See [Remote Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Remote-Files) in The
    GNU Emacs Manual. Often, this error appears when timers, process
    filters, process sentinels or special events in general try to access
    a remote file, and collide with another remote file operation. In
    general it is a good idea to write a bug report.
    See [Bugs](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Bugs) in The GNU Emacs Manual.

`ftp-error`
:   This is a subcategory of `remote-file-error`, which results from
    problems in accessing a remote file using ftp. See [Remote Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Remote-Files) in The GNU Emacs Manual.

`invalid-function`
:   The message is ‘`Invalid function`’. See [Symbol Function Indirection](#Function-Indirection).

`invalid-read-syntax`
:   The message is usually ‘`Invalid read syntax`’. See [Printed Representation and Read Syntax](#Printed-Representation). This error can also be raised by commands like
    `eval-expression` when there’s text following an expression. In
    that case, the message is ‘`Trailing garbage following expression`’.

`invalid-regexp`
:   The message is ‘`Invalid regexp`’. See [Regular Expressions](#Regular-Expressions).

`mark-inactive`
:   The message is ‘`The mark is not active now`’. See [The Mark](#The-Mark).

`no-catch`
:   The message is ‘`No catch for tag`’. See [Explicit Nonlocal Exits: `catch` and `throw`](#Catch-and-Throw).

`range-error`
:   The message is `Arithmetic range error`.

`overflow-error`
:   The message is ‘`Arithmetic overflow error`’. This is a subcategory
    of `range-error`.
    This can happen with integers exceeding the `integer-width` limit.
    See [Integer Basics](#Integer-Basics).

`scan-error`
:   The message is ‘`Scan error`’. This happens when certain
    syntax-parsing functions find invalid syntax or mismatched
    parentheses. Conventionally raised with three argument: a
    human-readable error message, the start of the obstacle that cannot be
    moved over, and the end of the obstacle. See [Moving over Balanced Expressions](#List-Motion), and
    see [Parsing Expressions](#Parsing-Expressions).

`search-failed`
:   The message is ‘`Search failed`’. See [Searching and Matching](#Searching-and-Matching).

`setting-constant`
:   The message is ‘`Attempt to set a constant symbol`’. This happens
    when attempting to assign values to `nil`, `t`,
    `most-positive-fixnum`, `most-negative-fixnum`, and keyword
    symbols. It also happens when attempting to assign values to
    `enable-multibyte-characters` and some other symbols whose direct
    assignment is not allowed for some reason. See [Variables that Never Change](#Constant-Variables).

`text-read-only`
:   The message is ‘`Text is read-only`’. This is a subcategory of
    `buffer-read-only`. See [Properties with Special Meanings](#Special-Properties).

`undefined-color`
:   The message is ‘`Undefined color`’. See [Color Names](#Color-Names).

`user-error`
:   The message is the empty string. See [How to Signal an Error](#Signaling-Errors).

`user-search-failed`
:   This is like ‘`search-failed`’, but doesn’t trigger the debugger,
    like ‘`user-error`’. See [How to Signal an Error](#Signaling-Errors), and see [Searching and Matching](#Searching-and-Matching). This is used for searching in Info files, see [Search
    Text](https://www.gnu.org/software/emacs/manual/html_mono/info.html#Search-Text) in Info.

`void-function`
:   The message is ‘`Symbol's function definition is void`’.
    See [Accessing Function Cell Contents](#Function-Cells).

`void-variable`
:   The message is ‘`Symbol's value as variable is void`’.
    See [Accessing Variable Values](#Accessing-Variables).

`wrong-number-of-arguments`
:   The message is ‘`Wrong number of arguments`’. See [Features of Argument Lists](#Argument-List).

`wrong-type-argument`
:   The message is ‘`Wrong type argument`’. See [Type Predicates](#Type-Predicates).

`unknown-image-type`
:   The message is ‘`Cannot determine image type`’. See [Images](#Images).

`inhibited-interaction`
:   The message is ‘`User interaction while inhibited`’. This error is
    signaled when `inhibit-interaction` is non-`nil` and a user
    interaction function (like `read-from-minibuffer`) is called.

---

Next: [Standard Hooks](#Standard-Hooks), Previous: [Standard Errors](#Standard-Errors), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Appendix G Standard Keymaps [¶](#Standard-Keymaps-1)

In this section we list some of the more general keymaps. Many of
these exist when Emacs is first started, but some are loaded only when
the respective feature is accessed.

There are many other, more specialized, maps than these; in particular
those associated with major and minor modes. The minibuffer uses
several keymaps (see [Minibuffer Commands that Do Completion](#Completion-Commands)). For more details on
keymaps, see [Keymaps](#Keymaps).

`2C-mode-map`
:   A sparse keymap for subcommands of the prefix `C-x 6`.  
    See [Two-Column Editing](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Two_002dColumn) in The GNU Emacs Manual.

`abbrev-map` [¶](#index-abbrev_002dmap)
:   A sparse keymap for subcommands of the prefix `C-x a`.  
    See [Defining Abbrevs](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Defining-Abbrevs) in The GNU Emacs Manual.

`button-buffer-map`
:   A sparse keymap useful for buffers containing buttons.  
    You may want to use this as a parent keymap. See [Buttons](#Buttons).

`button-map`
:   A sparse keymap used by buttons.

`ctl-x-4-map`
:   A sparse keymap for subcommands of the prefix `C-x 4`.

`ctl-x-5-map`
:   A sparse keymap for subcommands of the prefix `C-x 5`.

`ctl-x-map`
:   A full keymap for `C-x` commands.

`ctl-x-r-map` [¶](#index-ctl_002dx_002dr_002dmap)
:   A sparse keymap for subcommands of the prefix `C-x r`.  
    See [Registers](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Registers) in The GNU Emacs Manual.

`esc-map`
:   A full keymap for `ESC` (or `Meta`) commands.

`function-key-map`
:   The parent keymap of all `local-function-key-map` (q.v.) instances.

`global-map`
:   The full keymap containing default global key bindings.  
    Modes should not modify the Global map.

`goto-map`
:   A sparse keymap used for the `M-g` prefix key.

`help-map`
:   A sparse keymap for the keys following the help character `C-h`.  
    See [Help Functions](#Help-Functions).

`Helper-help-map`
:   A full keymap used by the help utility package.  
    It has the same keymap in its value cell and in its function cell.

`input-decode-map`
:   The keymap for translating keypad and function keys.  
    If there are none, then it contains an empty sparse keymap.
    See [Keymaps for Translating Sequences of Events](#Translation-Keymaps).

`key-translation-map`
:   A keymap for translating keys. This one overrides ordinary key
    bindings, unlike `local-function-key-map`. See [Keymaps for Translating Sequences of Events](#Translation-Keymaps).

`kmacro-keymap` [¶](#index-kmacro_002dkeymap)
:   A sparse keymap for keys that follows the `C-x C-k` prefix search.  
    See [Keyboard Macros](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Keyboard-Macros) in The GNU Emacs Manual.

`local-function-key-map`
:   The keymap for translating key sequences to preferred alternatives.  
    If there are none, then it contains an empty sparse keymap.
    See [Keymaps for Translating Sequences of Events](#Translation-Keymaps).

`menu-bar-file-menu` [¶](#index-menu-bar-keymaps)

`menu-bar-edit-menu`

`menu-bar-options-menu`

`global-buffers-menu-map`

`menu-bar-tools-menu`

`menu-bar-help-menu`
:   These keymaps display the main, top-level menus in the menu bar.  
    Some of them contain sub-menus. For example, the Edit menu contains
    `menu-bar-search-menu`, etc. See [The Menu Bar](#Menu-Bar).

`minibuffer-inactive-mode-map` [¶](#index-minibuffer_002dinactive_002dmode_002dmap)
:   A full keymap used in the minibuffer when it is not active.  
    See [Editing in the Minibuffer](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Minibuffer-Edit) in The GNU Emacs Manual.

`mode-line-coding-system-map` [¶](#index-mode_002dline_002dcoding_002dsystem_002dmap)

`mode-line-input-method-map`

`mode-line-column-line-number-mode-map`
:   These keymaps control various areas of the mode line.  
    See [Mode Line Format](#Mode-Line-Format).

`mode-specific-map`
:   The keymap for characters following `C-c`. Note, this is in the
    global map. This map is not actually mode-specific: its name was chosen
    to be informative in `C-h b` (`display-bindings`),
    where it describes the main use of the `C-c` prefix key.

`mouse-appearance-menu-map` [¶](#index-mouse_002dappearance_002dmenu_002dmap)
:   A sparse keymap used for the `S-mouse-1` key.

`mule-keymap`
:   The global keymap used for the `C-x RET` prefix key.

`narrow-map` [¶](#index-narrow_002dmap)
:   A sparse keymap for subcommands of the prefix `C-x n`.

`prog-mode-map` [¶](#index-prog_002dmode_002dmap)
:   The keymap used by Prog mode.  
    See [Basic Major Modes](#Basic-Major-Modes).

`query-replace-map`

`multi-query-replace-map`
:   A sparse keymap used for responses in `query-replace` and related
    commands; also for `y-or-n-p` and `map-y-or-n-p`. The functions
    that use this map do not support prefix keys; they look up one event at a
    time. `multi-query-replace-map` extends `query-replace-map`
    for multi-buffer replacements. See [query-replace-map](#Search-and-Replace).

`search-map`
:   A sparse keymap that provides global bindings for search-related commands.

`special-mode-map` [¶](#index-special_002dmode_002dmap)
:   The keymap used by Special mode.  
    See [Basic Major Modes](#Basic-Major-Modes).

`tab-prefix-map`
:   The global keymap used for the `C-x t` prefix key for tab-bar related commands.  
    See [Tab Bars](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Tab-Bars) in The GNU Emacs Manual.

`tab-bar-map` [¶](#index-tab_002dbar_002dmap)
:   The keymap defining the contents of the tab bar.  
    See [Tab Bars](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Tab-Bars) in The GNU Emacs Manual.

`tool-bar-map`
:   The keymap defining the contents of the tool bar.  
    See [Tool bars](#Tool-Bar).

`universal-argument-map` [¶](#index-universal_002dargument_002dmap)
:   A sparse keymap used while processing `C-u`.  
    See [Prefix Command Arguments](#Prefix-Command-Arguments).

`vc-prefix-map`
:   The global keymap used for the `C-x v` prefix key.

`x-alternatives-map` [¶](#index-x_002dalternatives_002dmap)
:   A sparse keymap used to map certain keys under graphical frames.  
    The function `x-setup-function-keys` uses this.

---

Next: [Index](#Index), Previous: [Standard Keymaps](#Standard-Keymaps), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Appendix H Standard Hooks [¶](#Standard-Hooks-1)

The following is a list of some hook variables that let you provide
functions to be called from within Emacs on suitable occasions.

Most of these variables have names ending with ‘`-hook`’. They are
*normal hooks*, run by means of `run-hooks`. The value of such
a hook is a list of functions; the functions are called with no
arguments and their values are completely ignored. The recommended way
to put a new function on such a hook is to call `add-hook`.
See [Hooks](#Hooks), for more information about using hooks.

The variables whose names end in ‘`-functions`’ are usually
*abnormal hooks* (some old code may also use the deprecated
‘`-hooks`’ suffix). Their values are lists of functions, but these
functions are called in a special way: they are either passed
arguments, or their return values are used in some way. The variables
whose names end in ‘`-function`’ have single functions as their
values.

This is not an exhaustive list, it only covers the more general hooks.
For example, every major mode defines a hook named
‘`modename-mode-hook`’. The major mode command runs this
normal hook with `run-mode-hooks` as the very last thing it does.
See [Mode Hooks](#Mode-Hooks). Most minor modes have mode hooks too.

A special feature allows you to specify expressions to evaluate if and
when a file is loaded (see [Hooks for Loading](#Hooks-for-Loading)). That feature is
not exactly a hook, but does a similar job.

`activate-mark-hook`

`deactivate-mark-hook`
:   See [The Mark](#The-Mark).

`after-change-functions`

`before-change-functions`

`first-change-hook`
:   See [Change Hooks](#Change-Hooks).

`after-change-major-mode-hook`

`change-major-mode-after-body-hook`
:   See [Mode Hooks](#Mode-Hooks).

`after-init-hook`

`before-init-hook`

`emacs-startup-hook`

`window-setup-hook`
:   See [The Init File](#Init-File).

`after-insert-file-functions`

`write-region-annotate-functions`

`write-region-post-annotation-function`
:   See [File Format Conversion](#Format-Conversion).

`after-make-frame-functions`

`before-make-frame-hook`

`server-after-make-frame-hook`
:   See [Creating Frames](#Creating-Frames).

`after-save-hook`

`before-save-hook`

`write-contents-functions`

`write-file-functions`
:   See [Saving Buffers](#Saving-Buffers).

`after-setting-font-hook` [¶](#index-after_002dsetting_002dfont_002dhook)
:   Hook run after a frame’s font changes.

`auto-save-hook`
:   See [Auto-Saving](#Auto_002dSaving).

`before-hack-local-variables-hook`

`hack-local-variables-hook`
:   See [File Local Variables](#File-Local-Variables).

`buffer-access-fontify-functions`
:   See [Lazy Computation of Text Properties](#Lazy-Properties).

`buffer-list-update-hook` [¶](#index-buffer_002dlist_002dupdate_002dhook-1)
:   Hook run when the buffer list changes (see [The Buffer List](#Buffer-List)).

`buffer-quit-function` [¶](#index-buffer_002dquit_002dfunction)
:   Function to call to quit the current buffer.

`change-major-mode-hook`
:   See [Creating and Deleting Buffer-Local Bindings](#Creating-Buffer_002dLocal).

`comint-password-function`
:   This abnormal hook permits a derived mode to supply a password for the
    underlying command interpreter without prompting the user.

`command-line-functions`
:   See [Command-Line Arguments](#Command_002dLine-Arguments).

`delayed-warnings-hook` [¶](#index-delayed_002dwarnings_002dhook-1)
:   The command loop runs this soon after `post-command-hook` (q.v.).

`focus-in-hook` [¶](#index-focus_002dout_002dhook)

`focus-out-hook`
:   See [Input Focus](#Input-Focus).

`delete-frame-functions`

`after-delete-frame-functions`
:   See [Deleting Frames](#Deleting-Frames).

`delete-terminal-functions`
:   See [Multiple Terminals](#Multiple-Terminals).

`pop-up-frame-function`

`split-window-preferred-function`
:   See [Additional Options for Displaying Buffers](#Choosing-Window-Options).

`echo-area-clear-hook`
:   See [Echo Area Customization](#Echo-Area-Customization).

`find-file-hook`

`find-file-not-found-functions`
:   See [Functions for Visiting Files](#Visiting-Functions).

`font-lock-extend-after-change-region-function`
:   See [Region to Fontify after a Buffer Change](#Region-to-Refontify).

`font-lock-extend-region-functions`
:   See [Multiline Font Lock Constructs](#Multiline-Font-Lock).

`font-lock-fontify-buffer-function`

`font-lock-fontify-region-function`

`font-lock-mark-block-function`

`font-lock-unfontify-buffer-function`

`font-lock-unfontify-region-function`
:   See [Other Font Lock Variables](#Other-Font-Lock-Variables).

`fontification-functions`
:   See [Automatic Face Assignment](#Auto-Faces).

`frame-auto-hide-function`
:   See [Quitting Windows](#Quitting-Windows).

`quit-window-hook`
:   See [Quitting Windows](#Quitting-Windows).

`kill-buffer-hook`

`kill-buffer-query-functions`
:   See [Killing Buffers](#Killing-Buffers).

`kill-emacs-hook`

`kill-emacs-query-functions`
:   See [Killing Emacs](#Killing-Emacs).

`menu-bar-update-hook`
:   See [The Menu Bar](#Menu-Bar).

`minibuffer-setup-hook`

`minibuffer-exit-hook`
:   See [Minibuffer Miscellany](#Minibuffer-Misc).

`mouse-leave-buffer-hook` [¶](#index-mouse_002dleave_002dbuffer_002dhook)
:   Hook run when the user mouse-clicks in a window.

`mouse-position-function`
:   See [Mouse Position](#Mouse-Position).

`prefix-command-echo-keystrokes-functions` [¶](#index-prefix_002dcommand_002decho_002dkeystrokes_002dfunctions)
:   An abnormal hook run by prefix commands (such as `C-u`) which
    should return a string describing the current prefix state. For
    example, `C-u` produces ‘`C-u-`’ and ‘`C-u 1 2 3-`’. Each
    hook function is called with no arguments and should return a string
    describing the current prefix state, or `nil` if there’s no
    prefix state. See [Prefix Command Arguments](#Prefix-Command-Arguments).

`prefix-command-preserve-state-hook` [¶](#index-prefix_002dcommand_002dpreserve_002dstate_002dhook)
:   Hook run when a prefix command needs to preserve the prefix by passing
    the current prefix command state to the next command. For example,
    `C-u` needs to pass the state to the next command when the user
    types `C-u -` or follows `C-u` with a digit.

`pre-redisplay-functions`
:   Hook run in each window just before redisplaying it. See [Forcing Redisplay](#Forcing-Redisplay).

`post-command-hook`

`pre-command-hook`
:   See [Command Loop Overview](#Command-Overview).

`post-gc-hook`
:   See [Garbage Collection](#Garbage-Collection).

`post-self-insert-hook`
:   See [Keymaps and Minor Modes](#Keymaps-and-Minor-Modes).

`suspend-hook`

`suspend-resume-hook`

`suspend-tty-functions`

`resume-tty-functions`
:   See [Suspending Emacs](#Suspending-Emacs).

`syntax-begin-function`

`syntax-propertize-extend-region-functions`

`syntax-propertize-function`

`font-lock-syntactic-face-function`
:   See [Syntactic Font Lock](#Syntactic-Font-Lock). See [Syntax Properties](#Syntax-Properties).

`temp-buffer-setup-hook`

`temp-buffer-show-function`

`temp-buffer-show-hook`
:   See [Temporary Displays](#Temporary-Displays).

`tty-setup-hook`
:   See [Terminal-Specific Initialization](#Terminal_002dSpecific).

`window-configuration-change-hook`

`window-scroll-functions`

`window-size-change-functions`
:   See [Hooks for Window Scrolling and Changes](#Window-Hooks).

---

Previous: [Standard Hooks](#Standard-Hooks), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Index [¶](#Index-1)

|  |  |
| --- | --- |
| Jump to: | [**-**](#Index_fn_symbol-1)   [**,**](#Index_fn_symbol-2)   [**;**](#Index_fn_symbol-3)   [**:**](#Index_fn_symbol-4)   [**?**](#Index_fn_symbol-5)   [**.**](#Index_fn_symbol-6)   [**'**](#Index_fn_symbol-7)   [**"**](#Index_fn_symbol-8)   [**(**](#Index_fn_symbol-9)   [**)**](#Index_fn_symbol-10)   [**[**](#Index_fn_symbol-11)   [**]**](#Index_fn_symbol-12)   [**@**](#Index_fn_symbol-13)   [**\***](#Index_fn_symbol-14)   [**/**](#Index_fn_symbol-15)   [**\**](#Index_fn_symbol-16)   [**&**](#Index_fn_symbol-17)   [**#**](#Index_fn_symbol-18)   [**%**](#Index_fn_symbol-19)   [**`**](#Index_fn_symbol-20)   [**^**](#Index_fn_symbol-21)   [**+**](#Index_fn_symbol-22)   [**<**](#Index_fn_symbol-23)   [**=**](#Index_fn_symbol-24)   [**>**](#Index_fn_symbol-25)   [**|**](#Index_fn_symbol-26)   [**$**](#Index_fn_symbol-27)   [**0**](#Index_fn_symbol-28)   [**1**](#Index_fn_symbol-29)   [**2**](#Index_fn_symbol-30)   [**3**](#Index_fn_symbol-31)     [**A**](#Index_fn_letter-A)   [**B**](#Index_fn_letter-B)   [**C**](#Index_fn_letter-C)   [**D**](#Index_fn_letter-D)   [**E**](#Index_fn_letter-E)   [**F**](#Index_fn_letter-F)   [**G**](#Index_fn_letter-G)   [**H**](#Index_fn_letter-H)   [**I**](#Index_fn_letter-I)   [**J**](#Index_fn_letter-J)   [**K**](#Index_fn_letter-K)   [**L**](#Index_fn_letter-L)   [**M**](#Index_fn_letter-M)   [**N**](#Index_fn_letter-N)   [**O**](#Index_fn_letter-O)   [**P**](#Index_fn_letter-P)   [**Q**](#Index_fn_letter-Q)   [**R**](#Index_fn_letter-R)   [**S**](#Index_fn_letter-S)   [**T**](#Index_fn_letter-T)   [**U**](#Index_fn_letter-U)   [**V**](#Index_fn_letter-V)   [**W**](#Index_fn_letter-W)   [**X**](#Index_fn_letter-X)   [**Y**](#Index_fn_letter-Y)   [**Z**](#Index_fn_letter-Z) |

|  |  |  |
| --- | --- | --- |
|  | Index Entry | Section |
| --- | | |
| - |
|  | [`-`](#index-_002d) | [Arithmetic Operations](#Arithmetic-Operations) |
|  | [`--disable-build-details` option to `configure`](#index-_002d_002ddisable_002dbuild_002ddetails-option-to-configure) | [Building Emacs](#Building-Emacs) |
|  | [`--enable-locallisppath` option to `configure`](#index-_002d_002denable_002dlocallisppath-option-to-configure) | [Building Emacs](#Building-Emacs) |
|  | [`--temacs` option, and dumping method](#index-_002d_002dtemacs-option_002c-and-dumping-method) | [Building Emacs](#Building-Emacs) |
|  | [–enable-profiling option of configure](#index-_002d_002denable_002dprofiling-option-of-configure) | [Profiling](#Profiling) |
| --- | | |
| , |
|  | [`, (with backquote)`](#index-_002c-_0028with-backquote_0029) | [Backquote](#Backquote) |
|  | [`,@ (with backquote)`](#index-_002c_0040-_0028with-backquote_0029) | [Backquote](#Backquote) |
| --- | | |
| ; |
|  | [‘`;`’ for commenting](#index-_003b-for-commenting) | [Comments](#Comments) |
| --- | | |
| : |
|  | [`:` in rx](#index-_003a-in-rx) | [Rx Constructs](#Rx-Constructs) |
|  | [`:advertised-binding` (symbol property)](#index-_003aadvertised_002dbinding-_0028symbol-property_0029) | [Keys in Documentation](#Keys-in-Documentation) |
|  | [`:autoload-end`](#index-_003aautoload_002dend) | [Autoload](#Autoload) |
|  | [`:deferred, JSONRPC keyword`](#index-_003adeferred_002c-JSONRPC-keyword) | [JSONRPC deferred requests](#JSONRPC-deferred-requests) |
|  | [`:documentation`](#index-_003adocumentation) | [Function Documentation](#Function-Documentation) |
|  | [`:eq?`](#index-_003aeq_003f) | [Pattern Matching](#Pattern-Matching) |
|  | [`:match?`](#index-_003amatch_003f) | [Pattern Matching](#Pattern-Matching) |
|  | [`:notification-dispatcher`](#index-_003anotification_002ddispatcher) | [JSONRPC Overview](#JSONRPC-Overview) |
|  | [`:pred?`](#index-_003apred_003f) | [Pattern Matching](#Pattern-Matching) |
|  | [`:repeat`](#index-_003arepeat) | [Creating Keymaps](#Creating-Keymaps) |
|  | [`:request-dispatcher`](#index-_003arequest_002ddispatcher) | [JSONRPC Overview](#JSONRPC-Overview) |
| --- | | |
| ? |
|  | [‘`?`’ in character constant](#index-_003f-in-character-constant) | [Basic Char Syntax](#Basic-Char-Syntax) |
|  | [`?` in minibuffer](#index-_003f-in-minibuffer) | [Text from Minibuffer](#Text-from-Minibuffer) |
|  | [‘`?`’ in regexp](#index-_003f-in-regexp) | [Regexp Special](#Regexp-Special) |
|  | [`?` in rx](#index-_003f-in-rx) | [Rx Constructs](#Rx-Constructs) |
|  | [`??` in rx](#index-_003f_003f-in-rx) | [Rx Constructs](#Rx-Constructs) |
| --- | | |
| . |
|  | [‘`.`’ in lists](#index-_002e-in-lists) | [Dotted Pair Notation](#Dotted-Pair-Notation) |
|  | [‘`.`’ in regexp](#index-_002e-in-regexp) | [Regexp Special](#Regexp-Special) |
|  | [.#, lock file names](#index-_002e_0023_002c-lock-file-names) | [File Locks](#File-Locks) |
|  | [`.elpaignore` file](#index-_002eelpaignore-file) | [Multi-file Packages](#Multi_002dfile-Packages) |
|  | [`.emacs`](#index-_002eemacs) | [Init File](#Init-File) |
| --- | | |
| ' |
|  | [‘`'`’ for quoting](#index-_0027-for-quoting) | [Quoting](#Quoting) |
| --- | | |
| " |
|  | [‘`"`’ in printing](#index-_0022-in-printing) | [Output Functions](#Output-Functions) |
|  | [‘`"`’ in strings](#index-_0022-in-strings) | [Syntax for Strings](#Syntax-for-Strings) |
| --- | | |
| ( |
|  | [‘`(`’ in regexp](#index-_0028-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [‘`(?:`’ in regexp](#index-_0028_003f_003a-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [‘`(…)`’ in lists](#index-_0028_2026_0029-in-lists) | [Cons Cell Type](#Cons-Cell-Type) |
|  | [`(emacs_env`](#index-_0028emacs_005fenv) | [Module Values](#Module-Values) |
| --- | | |
| ) |
|  | [‘`)`’ in regexp](#index-_0029-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
| --- | | |
| [ |
|  | [‘`[`’ in regexp](#index-_005b-in-regexp) | [Regexp Special](#Regexp-Special) |
|  | [[…] (Edebug specification)](#index-_005b_2026_005d-_0028Edebug-specification_0029) | [Specification List](#Specification-List) |
| --- | | |
| ] |
|  | [‘`]`’ in regexp](#index-_005d-in-regexp) | [Regexp Special](#Regexp-Special) |
| --- | | |
| @ |
|  | [‘`@`’ in `interactive`](#index-_0040-in-interactive) | [Using Interactive](#Using-Interactive) |
| --- | | |
| \* |
|  | [`*`](#index-_002a) | [Arithmetic Operations](#Arithmetic-Operations) |
|  | [‘`*`’ in `interactive`](#index-_002a-in-interactive) | [Using Interactive](#Using-Interactive) |
|  | [‘`*`’ in regexp](#index-_002a-in-regexp) | [Regexp Special](#Regexp-Special) |
|  | [`*` in rx](#index-_002a-in-rx) | [Rx Constructs](#Rx-Constructs) |
|  | [`*?` in rx](#index-_002a_003f-in-rx) | [Rx Constructs](#Rx-Constructs) |
|  | [`**` in rx](#index-_002a_002a-in-rx) | [Rx Constructs](#Rx-Constructs) |
|  | [`*scratch*`](#index-_002ascratch_002a) | [Auto Major Mode](#Auto-Major-Mode) |
| --- | | |
| / |
|  | [`/`](#index-_002f) | [Arithmetic Operations](#Arithmetic-Operations) |
|  | [`/=`](#index-_002f_003d) | [Comparison of Numbers](#Comparison-of-Numbers) |
|  | [`/dev/tty`](#index-_002fdev_002ftty) | [Serial Ports](#Serial-Ports) |
| --- | | |
| \ |
|  | [‘`\`’ in character constant](#index-_005c-in-character-constant) | [General Escape Syntax](#General-Escape-Syntax) |
|  | [‘`\`’ in display](#index-_005c-in-display) | [Truncation](#Truncation) |
|  | [‘`\`’ in printing](#index-_005c-in-printing) | [Output Functions](#Output-Functions) |
|  | [‘`\`’ in regexp](#index-_005c-in-regexp) | [Regexp Special](#Regexp-Special) |
|  | [‘`\`’ in replacement](#index-_005c-in-replacement) | [Replacing Match](#Replacing-Match) |
|  | [‘`\`’ in strings](#index-_005c-in-strings) | [Syntax for Strings](#Syntax-for-Strings) |
|  | [‘`\`’ in symbols](#index-_005c-in-symbols) | [Symbol Type](#Symbol-Type) |
|  | [‘`\_<`’ in regexp](#index-_005c_005f_003c-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [‘`\_>`’ in regexp](#index-_005c_005f_003e-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [‘`\'`’ in regexp](#index-_005c_0027-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [\( in strings](#index-_005c_0028-in-strings) | [List Motion](#List-Motion) |
|  | [‘`` \` ``’ in regexp](#index-_005c_0060-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [‘`\<`’ in regexp](#index-_005c_003c-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [‘`\=`’ in regexp](#index-_005c_003d-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [‘`\>`’ in regexp](#index-_005c_003e-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [‘`\a`’](#index-_005ca) | [Basic Char Syntax](#Basic-Char-Syntax) |
|  | [‘`\b`’](#index-_005cb) | [Basic Char Syntax](#Basic-Char-Syntax) |
|  | [‘`\b`’ in regexp](#index-_005cb-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [‘`\B`’ in regexp](#index-_005cB-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [‘`\e`’](#index-_005ce) | [Basic Char Syntax](#Basic-Char-Syntax) |
|  | [‘`\f`’](#index-_005cf) | [Basic Char Syntax](#Basic-Char-Syntax) |
|  | [‘`\n`’](#index-_005cn) | [Basic Char Syntax](#Basic-Char-Syntax) |
|  | [‘`\n`’ in print](#index-_005cn-in-print) | [Output Variables](#Output-Variables) |
|  | [‘`\n`’ in replacement](#index-_005cn-in-replacement) | [Replacing Match](#Replacing-Match) |
|  | [‘`\r`’](#index-_005cr) | [Basic Char Syntax](#Basic-Char-Syntax) |
|  | [‘`\s`’](#index-_005cs) | [Basic Char Syntax](#Basic-Char-Syntax) |
|  | [‘`\s`’ in regexp](#index-_005cs-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [‘`\S`’ in regexp](#index-_005cS-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [‘`\t`’](#index-_005ct) | [Basic Char Syntax](#Basic-Char-Syntax) |
|  | [‘`\v`’](#index-_005cv) | [Basic Char Syntax](#Basic-Char-Syntax) |
|  | [‘`\w`’ in regexp](#index-_005cw-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [‘`\W`’ in regexp](#index-_005cW-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
| --- | | |
| & |
|  | [‘`&`’ in replacement](#index-_0026-in-replacement) | [Replacing Match](#Replacing-Match) |
|  | [`&optional`](#index-_0026optional) | [Argument List](#Argument-List) |
|  | [`&rest`](#index-_0026rest) | [Argument List](#Argument-List) |
| --- | | |
| # |
|  | [‘`#:`’ read syntax](#index-_0023_003a-read-syntax) | [Symbol Type](#Symbol-Type) |
|  | [‘`#'`’ syntax](#index-_0023_0027-syntax) | [Anonymous Functions](#Anonymous-Functions) |
|  | [‘`#(`’ read syntax](#index-_0023_0028-read-syntax) | [Text Props and Strings](#Text-Props-and-Strings) |
|  | [‘`#@count`’](#index-_0023_0040count) | [Docs and Compilation](#Docs-and-Compilation) |
|  | [‘`##`’ read syntax](#index-_0023_0023-read-syntax) | [Symbol Type](#Symbol-Type) |
|  | [‘`#^`’ read syntax](#index-_0023_005e-read-syntax) | [Char-Table Type](#Char_002dTable-Type) |
|  | [‘`#$`’](#index-_0023_0024) | [Docs and Compilation](#Docs-and-Compilation) |
|  | [‘`#n#`’ read syntax](#index-_0023n_0023-read-syntax) | [Circular Objects](#Circular-Objects) |
|  | [‘`#n=`’ read syntax](#index-_0023n_003d-read-syntax) | [Circular Objects](#Circular-Objects) |
| --- | | |
| % |
|  | [`%`](#index-_0025) | [Arithmetic Operations](#Arithmetic-Operations) |
|  | [‘`%`’ in format](#index-_0025-in-format) | [Formatting Strings](#Formatting-Strings) |
|  | [`%`-constructs in the mode line](#index-_0025_002dconstructs-in-the-mode-line) | [%-Constructs](#g_t_0025_002dConstructs) |
| --- | | |
| ` |
|  | [`` ` ``](#index-_0060) | [Backquote](#Backquote) |
|  | [‘ (list substitution)](#index-_0060-_0028list-substitution_0029) | [Backquote](#Backquote) |
| --- | | |
| ^ |
|  | [‘`^`’ in `interactive`](#index-_005e-in-interactive) | [Using Interactive](#Using-Interactive) |
|  | [‘`^`’ in regexp](#index-_005e-in-regexp) | [Regexp Special](#Regexp-Special) |
| --- | | |
| + |
|  | [`+`](#index-_002b) | [Arithmetic Operations](#Arithmetic-Operations) |
|  | [‘`+`’ in regexp](#index-_002b-in-regexp) | [Regexp Special](#Regexp-Special) |
|  | [`+` in rx](#index-_002b-in-rx) | [Rx Constructs](#Rx-Constructs) |
|  | [`+?` in rx](#index-_002b_003f-in-rx) | [Rx Constructs](#Rx-Constructs) |
| --- | | |
| < |
|  | [`<`](#index-_003c) | [Comparison of Numbers](#Comparison-of-Numbers) |
|  | [`<=`](#index-_003c_003d) | [Comparison of Numbers](#Comparison-of-Numbers) |
| --- | | |
| = |
|  | [`=`](#index-_003d) | [Comparison of Numbers](#Comparison-of-Numbers) |
|  | [`=` in rx](#index-_003d-in-rx) | [Rx Constructs](#Rx-Constructs) |
| --- | | |
| > |
|  | [`>`](#index-_003e) | [Comparison of Numbers](#Comparison-of-Numbers) |
|  | [`>=`](#index-_003e_003d) | [Comparison of Numbers](#Comparison-of-Numbers) |
|  | [`>=` in rx](#index-_003e_003d-in-rx) | [Rx Constructs](#Rx-Constructs) |
| --- | | |
| | |
|  | [‘`|`’ in regexp](#index-_007c-in-regexp) | [Regexp Backslash](#Regexp-Backslash) |
|  | [`|` in rx](#index-_007c-in-rx) | [Rx Constructs](#Rx-Constructs) |
| --- | | |
| $ |
|  | [‘`$`’ in display](#index-_0024-in-display) | [Truncation](#Truncation) |
|  | [‘`$`’ in regexp](#index-_0024-in-regexp) | [Regexp Special](#Regexp-Special) |
| --- | | |
| 0 |
|  | [`0+` in rx](#index-0_002b-in-rx) | [Rx Constructs](#Rx-Constructs) |
| --- | | |
| 1 |
|  | [`1-`](#index-1_002d) | [Arithmetic Operations](#Arithmetic-Operations) |
|  | [`1+`](#index-1_002b) | [Arithmetic Operations](#Arithmetic-Operations) |
|  | [`1+` in rx](#index-1_002b-in-rx) | [Rx Constructs](#Rx-Constructs) |
|  | [`1value`](#index-1value) | [Test Coverage](#Test-Coverage) |
| --- | | |
| 2 |
|  | [`2C-mode-map`](#index-2C_002dmode_002dmap) | [Prefix Keys](#Prefix-Keys) |
|  | [2D box](#index-2D-box) | [Face Attributes](#Face-Attributes) |
| --- | | |
| 3 |
|  | [3D box](#index-3D-box) | [Face Attributes](#Face-Attributes) |
| --- | | |
| A |
|  | [abbrev](#index-abbrev) | [Abbrevs](#Abbrevs) |
|  | [abbrev properties](#index-abbrev-properties) | [Abbrev Properties](#Abbrev-Properties) |
|  | [abbrev table properties](#index-abbrev-table-properties) | [Abbrev Table Properties](#Abbrev-Table-Properties) |
|  | [abbrev tables](#index-abbrev-tables) | [Abbrev Tables](#Abbrev-Tables) |
|  | [abbrev tables in modes](#index-abbrev-tables-in-modes) | [Major Mode Conventions](#Major-Mode-Conventions) |
|  | [`abbrev-all-caps`](#index-abbrev_002dall_002dcaps) | [Abbrev Expansion](#Abbrev-Expansion) |
|  | [`abbrev-expand-function`](#index-abbrev_002dexpand_002dfunction) | [Abbrev Expansion](#Abbrev-Expansion) |
|  | [`abbrev-expansion`](#index-abbrev_002dexpansion) | [Abbrev Expansion](#Abbrev-Expansion) |
|  | [`abbrev-file-name`](#index-abbrev_002dfile_002dname) | [Abbrev Files](#Abbrev-Files) |
|  | [`abbrev-get`](#index-abbrev_002dget) | [Abbrev Properties](#Abbrev-Properties) |
|  | [`abbrev-insert`](#index-abbrev_002dinsert) | [Abbrev Expansion](#Abbrev-Expansion) |
|  | [`abbrev-map`](#index-abbrev_002dmap) | [Standard Keymaps](#Standard-Keymaps) |
|  | [`abbrev-minor-mode-table-alist`](#index-abbrev_002dminor_002dmode_002dtable_002dalist) | [Standard Abbrev Tables](#Standard-Abbrev-Tables) |
|  | [`abbrev-prefix-mark`](#index-abbrev_002dprefix_002dmark) | [Abbrev Expansion](#Abbrev-Expansion) |
|  | [`abbrev-put`](#index-abbrev_002dput) | [Abbrev Properties](#Abbrev-Properties) |
|  | [`abbrev-start-location`](#index-abbrev_002dstart_002dlocation) | [Abbrev Expansion](#Abbrev-Expansion) |
|  | [`abbrev-start-location-buffer`](#index-abbrev_002dstart_002dlocation_002dbuffer) | [Abbrev Expansion](#Abbrev-Expansion) |
|  | [`abbrev-symbol`](#index-abbrev_002dsymbol) | [Abbrev Expansion](#Abbrev-Expansion) |
|  | [`abbrev-table-get`](#index-abbrev_002dtable_002dget) | [Abbrev Table Properties](#Abbrev-Table-Properties) |
|  | [`abbrev-table-name-list`](#index-abbrev_002dtable_002dname_002dlist) | [Abbrev Tables](#Abbrev-Tables) |
|  | [`abbrev-table-p`](#index-abbrev_002dtable_002dp) | [Abbrev Tables](#Abbrev-Tables) |
|  | [`abbrev-table-put`](#index-abbrev_002dtable_002dput) | [Abbrev Table Properties](#Abbrev-Table-Properties) |
|  | [`abbreviate-file-name`](#index-abbreviate_002dfile_002dname) | [Directory Names](#Directory-Names) |
|  | [abbreviated file names](#index-abbreviated-file-names) | [Directory Names](#Directory-Names) |
|  | [`abbrevs-changed`](#index-abbrevs_002dchanged) | [Abbrev Files](#Abbrev-Files) |
|  | [abbrevs, looking up and expanding](#index-abbrevs_002c-looking-up-and-expanding) | [Abbrev Expansion](#Abbrev-Expansion) |
|  | [abnormal hook](#index-abnormal-hook) | [Hooks](#Hooks) |
|  | [`abort-recursive-edit`](#index-abort_002drecursive_002dedit) | [Recursive Editing](#Recursive-Editing) |
|  | [`abort-recursive-edit (Edebug)`](#index-abort_002drecursive_002dedit-_0028Edebug_0029) | [Edebug Misc](#Edebug-Misc) |
|  | [aborting](#index-aborting) | [Recursive Editing](#Recursive-Editing) |
|  | [`abs`](#index-abs) | [Comparison of Numbers](#Comparison-of-Numbers) |
|  | [absolute edges](#index-absolute-edges) | [Frame Layout](#Frame-Layout) |
|  | [absolute file name](#index-absolute-file-name) | [Relative File Names](#Relative-File-Names) |
|  | [absolute frame edges](#index-absolute-frame-edges) | [Frame Layout](#Frame-Layout) |
|  | [absolute frame position](#index-absolute-frame-position) | [Frame Layout](#Frame-Layout) |
|  | [absolute position](#index-absolute-position) | [Frame Layout](#Frame-Layout) |
|  | [accept input from processes](#index-accept-input-from-processes) | [Accepting Output](#Accepting-Output) |
|  | [`accept-change-group`](#index-accept_002dchange_002dgroup) | [Atomic Changes](#Atomic-Changes) |
|  | [`accept-process-output`](#index-accept_002dprocess_002doutput) | [Accepting Output](#Accepting-Output) |
|  | [access control list](#index-access-control-list) | [Extended Attributes](#Extended-Attributes) |
|  | [access minibuffer contents](#index-access-minibuffer-contents) | [Minibuffer Contents](#Minibuffer-Contents) |
|  | [`access-file`](#index-access_002dfile) | [Testing Accessibility](#Testing-Accessibility) |
|  | [accessibility of a file](#index-accessibility-of-a-file) | [Testing Accessibility](#Testing-Accessibility) |
|  | [accessible portion (of a buffer)](#index-accessible-portion-_0028of-a-buffer_0029) | [Narrowing](#Narrowing) |
|  | [`accessible-keymaps`](#index-accessible_002dkeymaps) | [Scanning Keymaps](#Scanning-Keymaps) |
|  | [accessing documentation strings](#index-accessing-documentation-strings) | [Accessing Documentation](#Accessing-Documentation) |
|  | [accessing events within a key translation function](#index-accessing-events-within-a-key-translation-function) | [Translation Keymaps](#Translation-Keymaps) |
|  | [accessing hash tables](#index-accessing-hash-tables) | [Hash Access](#Hash-Access) |
|  | [accessing plist properties](#index-accessing-plist-properties) | [Plist Access](#Plist-Access) |
|  | [ACL entries](#index-ACL-entries) | [Extended Attributes](#Extended-Attributes) |
|  | [`acos`](#index-acos) | [Math Functions](#Math-Functions) |
|  | [`action (button property)`](#index-action-_0028button-property_0029) | [Button Properties](#Button-Properties) |
|  | [action alist for buffer display](#index-action-alist-for-buffer-display) | [Buffer Display Action Alists](#Buffer-Display-Action-Alists) |
|  | [action function, for buffer display](#index-action-function_002c-for-buffer-display) | [Buffer Display Action Functions](#Buffer-Display-Action-Functions) |
|  | [`action, customization keyword`](#index-action_002c-customization-keyword) | [Type Keywords](#Type-Keywords) |
|  | [`activate-change-group`](#index-activate_002dchange_002dgroup) | [Atomic Changes](#Atomic-Changes) |
|  | [`activate-mark-hook`](#index-activate_002dmark_002dhook) | [The Mark](#The-Mark) |
|  | [active display table](#index-active-display-table) | [Active Display Table](#Active-Display-Table) |
|  | [active keymap](#index-active-keymap) | [Active Keymaps](#Active-Keymaps) |
|  | [active keymap, controlling](#index-active-keymap_002c-controlling) | [Controlling Active Maps](#Controlling-Active-Maps) |
|  | [active minibuffer](#index-active-minibuffer) | [Intro to Minibuffers](#Intro-to-Minibuffers) |
|  | [`active-minibuffer-window`](#index-active_002dminibuffer_002dwindow) | [Minibuffer Windows](#Minibuffer-Windows) |
|  | [`ad-activate`](#index-ad_002dactivate) | [Porting Old Advice](#Porting-Old-Advice) |
|  | [`adaptive-fill-first-line-regexp`](#index-adaptive_002dfill_002dfirst_002dline_002dregexp) | [Adaptive Fill](#Adaptive-Fill) |
|  | [`adaptive-fill-function`](#index-adaptive_002dfill_002dfunction) | [Adaptive Fill](#Adaptive-Fill) |
|  | [`adaptive-fill-mode`](#index-adaptive_002dfill_002dmode) | [Adaptive Fill](#Adaptive-Fill) |
|  | [`adaptive-fill-regexp`](#index-adaptive_002dfill_002dregexp) | [Adaptive Fill](#Adaptive-Fill) |
|  | [`add-display-text-property`](#index-add_002ddisplay_002dtext_002dproperty) | [Display Property](#Display-Property) |
|  | [`add-face-text-property`](#index-add_002dface_002dtext_002dproperty) | [Changing Properties](#Changing-Properties) |
|  | [`add-function`](#index-add_002dfunction) | [Core Advising Primitives](#Core-Advising-Primitives) |
|  | [`add-hook`](#index-add_002dhook) | [Setting Hooks](#Setting-Hooks) |
|  | [`add-name-to-file`](#index-add_002dname_002dto_002dfile) | [Changing Files](#Changing-Files) |
|  | [`add-text-properties`](#index-add_002dtext_002dproperties) | [Changing Properties](#Changing-Properties) |
|  | [`add-to-history`](#index-add_002dto_002dhistory) | [Minibuffer History](#Minibuffer-History) |
|  | [`add-to-invisibility-spec`](#index-add_002dto_002dinvisibility_002dspec) | [Invisible Text](#Invisible-Text) |
|  | [`add-to-list`](#index-add_002dto_002dlist) | [List Variables](#List-Variables) |
|  | [`add-to-ordered-list`](#index-add_002dto_002dordered_002dlist) | [List Variables](#List-Variables) |
|  | [`add-variable-watcher`](#index-add_002dvariable_002dwatcher) | [Watching Variables](#Watching-Variables) |
|  | [address field of register](#index-address-field-of-register) | [Cons Cell Type](#Cons-Cell-Type) |
|  | [`adjust-window-trailing-edge`](#index-adjust_002dwindow_002dtrailing_002dedge) | [Resizing Windows](#Resizing-Windows) |
|  | [adjusting point](#index-adjusting-point) | [Adjusting Point](#Adjusting-Point) |
|  | [advertised binding](#index-advertised-binding) | [Keys in Documentation](#Keys-in-Documentation) |
|  | [`advertised-calling-convention` (`declare` spec)](#index-advertised_002dcalling_002dconvention-_0028declare-spec_0029) | [Declare Form](#Declare-Form) |
|  | [`advice-add`](#index-advice_002dadd) | [Advising Named Functions](#Advising-Named-Functions) |
|  | [`advice-eval-interactive-spec`](#index-advice_002deval_002dinteractive_002dspec) | [Core Advising Primitives](#Core-Advising-Primitives) |
|  | [`advice-function-mapc`](#index-advice_002dfunction_002dmapc) | [Core Advising Primitives](#Core-Advising-Primitives) |
|  | [`advice-function-member-p`](#index-advice_002dfunction_002dmember_002dp) | [Core Advising Primitives](#Core-Advising-Primitives) |
|  | [`advice-mapc`](#index-advice_002dmapc) | [Advising Named Functions](#Advising-Named-Functions) |
|  | [`advice-member-p`](#index-advice_002dmember_002dp) | [Advising Named Functions](#Advising-Named-Functions) |
|  | [`advice-remove`](#index-advice_002dremove) | [Advising Named Functions](#Advising-Named-Functions) |
|  | [advice, add and remove](#index-advice_002c-add-and-remove) | [Core Advising Primitives](#Core-Advising-Primitives) |
|  | [advices, porting from `defadvice`](#index-advices_002c-porting-from-defadvice) | [Porting Old Advice](#Porting-Old-Advice) |
|  | [advising functions](#index-advising-functions) | [Advising Functions](#Advising-Functions) |
|  | [advising named functions](#index-advising-named-functions) | [Advising Named Functions](#Advising-Named-Functions) |
|  | [AEAD cipher](#index-AEAD-cipher) | [GnuTLS Cryptography](#GnuTLS-Cryptography) |
|  | [`affixation-function`, in completion](#index-affixation_002dfunction_002c-in-completion) | [Programmed Completion](#Programmed-Completion) |
|  | [after-change notifier, for tree-sitter parse-tree](#index-after_002dchange-notifier_002c-for-tree_002dsitter-parse_002dtree) | [Using Parser](#Using-Parser) |
|  | [`after-change-functions`](#index-after_002dchange_002dfunctions) | [Change Hooks](#Change-Hooks) |
|  | [`after-change-major-mode-hook`](#index-after_002dchange_002dmajor_002dmode_002dhook) | [Mode Hooks](#Mode-Hooks) |
|  | [`after-delete-frame-functions`](#index-after_002ddelete_002dframe_002dfunctions) | [Deleting Frames](#Deleting-Frames) |
|  | [`after-find-file`](#index-after_002dfind_002dfile) | [Subroutines of Visiting](#Subroutines-of-Visiting) |
|  | [`after-focus-change-function`](#index-after_002dfocus_002dchange_002dfunction) | [Input Focus](#Input-Focus) |
|  | [`after-init-hook`](#index-after_002dinit_002dhook) | [Init File](#Init-File) |
|  | [`after-init-time`](#index-after_002dinit_002dtime) | [Startup Summary](#Startup-Summary) |
|  | [`after-insert-file-functions`](#index-after_002dinsert_002dfile_002dfunctions) | [Format Conversion Piecemeal](#Format-Conversion-Piecemeal) |
|  | [`after-load-functions`](#index-after_002dload_002dfunctions) | [Hooks for Loading](#Hooks-for-Loading) |
|  | [`after-make-frame-functions`](#index-after_002dmake_002dframe_002dfunctions) | [Creating Frames](#Creating-Frames) |
|  | [`after-pdump-load-hook`](#index-after_002dpdump_002dload_002dhook) | [Building Emacs](#Building-Emacs) |
|  | [`after-revert-hook`](#index-after_002drevert_002dhook) | [Reverting](#Reverting) |
|  | [`after-save-hook`](#index-after_002dsave_002dhook) | [Saving Buffers](#Saving-Buffers) |
|  | [`after-setting-font-hook`](#index-after_002dsetting_002dfont_002dhook) | [Standard Hooks](#Standard-Hooks) |
|  | [`after-string (overlay property)`](#index-after_002dstring-_0028overlay-property_0029) | [Overlay Properties](#Overlay-Properties) |
|  | [alias, for coding systems](#index-alias_002c-for-coding-systems) | [Coding System Basics](#Coding-System-Basics) |
|  | [alias, for faces](#index-alias_002c-for-faces) | [Face Functions](#Face-Functions) |
|  | [alias, for functions](#index-alias_002c-for-functions) | [Defining Functions](#Defining-Functions) |
|  | [alias, for variables](#index-alias_002c-for-variables) | [Variable Aliases](#Variable-Aliases) |
|  | [aligning header line, when line numbers are displayed](#index-aligning-header-line_002c-when-line-numbers-are-displayed) | [Pixel Specification](#Pixel-Specification) |
|  | [alist](#index-alist) | [Association Lists](#Association-Lists) |
|  | [alist vs. plist](#index-alist-vs_002e-plist) | [Plists and Alists](#Plists-and-Alists) |
|  | [`alist-get`](#index-alist_002dget) | [Association Lists](#Association-Lists) |
|  | [`all`](#index-all) | [List Elements](#List-Elements) |
|  | [`all-completions`](#index-all_002dcompletions) | [Basic Completion](#Basic-Completion) |
|  | [`all-threads`](#index-all_002dthreads) | [Basic Thread Functions](#Basic-Thread-Functions) |
|  | [`allow-no-window, a buffer display action alist entry`](#index-allow_002dno_002dwindow_002c-a-buffer-display-action-alist-entry) | [Buffer Display Action Alists](#Buffer-Display-Action-Alists) |
|  | [alnum character class, regexp](#index-alnum-character-class_002c-regexp) | [Char Classes](#Char-Classes) |
|  | [alpha character class, regexp](#index-alpha-character-class_002c-regexp) | [Char Classes](#Char-Classes) |
|  | [`alpha-background, a frame parameter`](#index-alpha_002dbackground_002c-a-frame-parameter) | [Font and Color Parameters](#Font-and-Color-Parameters) |
|  | [`alpha, a frame parameter`](#index-alpha_002c-a-frame-parameter) | [Font and Color Parameters](#Font-and-Color-Parameters) |
|  | [alt characters](#index-alt-characters) | [Other Char Bits](#Other-Char-Bits) |
|  | [`alter-fullscreen-frames`](#index-alter_002dfullscreen_002dframes) | [Frame Size](#Frame-Size) |
|  | [altering the size of fullscreen frames](#index-altering-the-size-of-fullscreen-frames) | [Frame Size](#Frame-Size) |
|  | [alternative commands, defining](#index-alternative-commands_002c-defining) | [Generic Commands](#Generic-Commands) |
|  | [`always`](#index-always) | [Calling Functions](#Calling-Functions) |
|  | [amalgamating commands, and undo](#index-amalgamating-commands_002c-and-undo) | [Undo](#Undo) |
|  | [`amalgamating-undo-limit`](#index-amalgamating_002dundo_002dlimit) | [Undo](#Undo) |
|  | [ancestor directory of file](#index-ancestor-directory-of-file) | [Contents of Directories](#Contents-of-Directories) |
|  | [ancestor frame](#index-ancestor-frame) | [Child Frame Operations](#Child-Frame-Operations) |
|  | [ancestry, of major modes](#index-ancestry_002c-of-major-modes) | [Derived Modes](#Derived-Modes) |
|  | [`and`](#index-and) | [Combining Conditions](#Combining-Conditions) |
|  | [`and` in rx](#index-and-in-rx) | [Rx Constructs](#Rx-Constructs) |
|  | [`and-let*`](#index-and_002dlet_002a) | [Conditionals](#Conditionals) |
|  | [Android selections](#index-Android-selections) | [Other Selections](#Other-Selections) |
|  | [`android-notifications-notify`](#index-android_002dnotifications_002dnotify) | [Desktop Notifications](#Desktop-Notifications) |
|  | [animation](#index-animation) | [Multi-Frame Images](#Multi_002dFrame-Images) |
|  | [`annotation-function`, in completion](#index-annotation_002dfunction_002c-in-completion) | [Programmed Completion](#Programmed-Completion) |
|  | [anonymous face](#index-anonymous-face) | [Faces](#Faces) |
|  | [anonymous function](#index-anonymous-function) | [Anonymous Functions](#Anonymous-Functions) |
|  | [anonymous node, tree-sitter](#index-anonymous-node_002c-tree_002dsitter) | [Language Grammar](#Language-Grammar) |
|  | [`anonymous`, treesit-defined thing](#index-anonymous_002c-treesit_002ddefined-thing) | [User-defined Things](#User_002ddefined-Things) |
|  | [`any`](#index-any) | [List Elements](#List-Elements) |
|  | [`any` in rx](#index-any-in-rx) | [Rx Constructs](#Rx-Constructs) |
|  | [`anychar` in rx](#index-anychar-in-rx) | [Rx Constructs](#Rx-Constructs) |
|  | [`anything` in rx](#index-anything-in-rx) | [Rx Constructs](#Rx-Constructs) |
|  | [apostrophe for quoting](#index-apostrophe-for-quoting) | [Quoting](#Quoting) |
|  | [apostrophe, quoting in documentation strings](#index-apostrophe_002c-quoting-in-documentation-strings) | [Documentation Tips](#Documentation-Tips) |
|  | [`append`](#index-append) | [Building Lists](#Building-Lists) |
|  | [`append-to-file`](#index-append_002dto_002dfile) | [Writing to Files](#Writing-to-Files) |
|  | [`apply`](#index-apply) | [Calling Functions](#Calling-Functions) |
|  | [`apply-partially`](#index-apply_002dpartially) | [Calling Functions](#Calling-Functions) |
|  | [`apply`, and debugging](#index-apply_002c-and-debugging) | [Internals of Debugger](#Internals-of-Debugger) |
|  | [applying customizations](#index-applying-customizations) | [Applying Customizations](#Applying-Customizations) |
|  | [`apropos`](#index-apropos) | [Help Functions](#Help-Functions) |
|  | [archive web server](#index-archive-web-server) | [Archive Web Server](#Archive-Web-Server) |
|  | [`aref`](#index-aref) | [Array Functions](#Array-Functions) |
|  | [`args, customization keyword`](#index-args_002c-customization-keyword) | [Composite Types](#Composite-Types) |
|  | [argument](#index-argument) | [What Is a Function](#What-Is-a-Function) |
|  | [argument binding](#index-argument-binding) | [Argument List](#Argument-List) |
|  | [argument lists, features](#index-argument-lists_002c-features) | [Argument List](#Argument-List) |
|  | [arguments for shell commands](#index-arguments-for-shell-commands) | [Shell Arguments](#Shell-Arguments) |
|  | [arguments, interactive entry](#index-arguments_002c-interactive-entry) | [Using Interactive](#Using-Interactive) |
|  | [arguments, reading](#index-arguments_002c-reading) | [Minibuffers](#Minibuffers) |
|  | [`argv`](#index-argv) | [Command-Line Arguments](#Command_002dLine-Arguments) |
|  | [`arith-error` example](#index-arith_002derror-example) | [Handling Errors](#Handling-Errors) |
|  | [`arith-error` in division](#index-arith_002derror-in-division) | [Arithmetic Operations](#Arithmetic-Operations) |
|  | [arithmetic operations](#index-arithmetic-operations) | [Arithmetic Operations](#Arithmetic-Operations) |
|  | [arithmetic shift](#index-arithmetic-shift) | [Bitwise Operations](#Bitwise-Operations) |
|  | [array](#index-array) | [Arrays](#Arrays) |
|  | [array elements](#index-array-elements) | [Array Functions](#Array-Functions) |
|  | [`arrayp`](#index-arrayp) | [Array Functions](#Array-Functions) |
|  | [ascii character class, regexp](#index-ascii-character-class_002c-regexp) | [Char Classes](#Char-Classes) |
|  | [ASCII character codes](#index-ASCII-character-codes) | [Character Type](#Character-Type) |
|  | [ASCII control characters](#index-ASCII-control-characters) | [Usual Display](#Usual-Display) |
|  | [`ascii-case-table`](#index-ascii_002dcase_002dtable) | [Case Tables](#Case-Tables) |
|  | [`aset`](#index-aset) | [Array Functions](#Array-Functions) |
|  | [`ash`](#index-ash) | [Bitwise Operations](#Bitwise-Operations) |
|  | [`asin`](#index-asin) | [Math Functions](#Math-Functions) |
|  | [`ask-user-about-lock`](#index-ask_002duser_002dabout_002dlock) | [File Locks](#File-Locks) |
|  | [`ask-user-about-supersession-threat`](#index-ask_002duser_002dabout_002dsupersession_002dthreat) | [Modification Time](#Modification-Time) |
|  | [asking the user questions](#index-asking-the-user-questions) | [Yes-or-No Queries](#Yes_002dor_002dNo-Queries) |
|  | [`assoc`](#index-assoc) | [Association Lists](#Association-Lists) |
|  | [`assoc-default`](#index-assoc_002ddefault) | [Association Lists](#Association-Lists) |
|  | [`assoc-delete-all`](#index-assoc_002ddelete_002dall) | [Association Lists](#Association-Lists) |
|  | [`assoc-string`](#index-assoc_002dstring) | [Text Comparison](#Text-Comparison) |
|  | [association list](#index-association-list) | [Association Lists](#Association-Lists) |
|  | [`assq`](#index-assq) | [Association Lists](#Association-Lists) |
|  | [`assq-delete-all`](#index-assq_002ddelete_002dall) | [Association Lists](#Association-Lists) |
|  | [asynchronous native compilation, disable](#index-asynchronous-native-compilation_002c-disable) | [Native-Compilation Variables](#Native_002dCompilation-Variables) |
|  | [asynchronous subprocess](#index-asynchronous-subprocess) | [Asynchronous Processes](#Asynchronous-Processes) |
|  | [`atan`](#index-atan) | [Math Functions](#Math-Functions) |
|  | [`atom`](#index-atom) | [List-related Predicates](#List_002drelated-Predicates) |
|  | [atomic changes](#index-atomic-changes) | [Atomic Changes](#Atomic-Changes) |
|  | [atomic windows](#index-atomic-windows) | [Atomic Windows](#Atomic-Windows) |
|  | [atoms](#index-atoms) | [Cons Cell Type](#Cons-Cell-Type) |
|  | [attributes of text](#index-attributes-of-text) | [Text Properties](#Text-Properties) |
|  | [Auto Fill mode](#index-Auto-Fill-mode) | [Auto Filling](#Auto-Filling) |
|  | [`auto-coding-alist`](#index-auto_002dcoding_002dalist) | [Default Coding Systems](#Default-Coding-Systems) |
|  | [`auto-coding-functions`](#index-auto_002dcoding_002dfunctions) | [Default Coding Systems](#Default-Coding-Systems) |
|  | [`auto-coding-regexp-alist`](#index-auto_002dcoding_002dregexp_002dalist) | [Default Coding Systems](#Default-Coding-Systems) |
|  | [`auto-fill-chars`](#index-auto_002dfill_002dchars) | [Auto Filling](#Auto-Filling) |
|  | [`auto-fill-function`](#index-auto_002dfill_002dfunction) | [Auto Filling](#Auto-Filling) |
|  | [`auto-hide-function, a frame parameter`](#index-auto_002dhide_002dfunction_002c-a-frame-parameter) | [Frame Interaction Parameters](#Frame-Interaction-Parameters) |
|  | [`auto-hscroll-mode`](#index-auto_002dhscroll_002dmode) | [Horizontal Scrolling](#Horizontal-Scrolling) |
|  | [`auto-lower, a frame parameter`](#index-auto_002dlower_002c-a-frame-parameter) | [Management Parameters](#Management-Parameters) |
|  | [`auto-mode-alist`](#index-auto_002dmode_002dalist) | [Auto Major Mode](#Auto-Major-Mode) |
|  | [`auto-raise-tool-bar-buttons`](#index-auto_002draise_002dtool_002dbar_002dbuttons) | [Tool Bar](#Tool-Bar) |
|  | [`auto-raise, a frame parameter`](#index-auto_002draise_002c-a-frame-parameter) | [Management Parameters](#Management-Parameters) |
|  | [`auto-resize-tool-bars`](#index-auto_002dresize_002dtool_002dbars) | [Tool Bar](#Tool-Bar) |
|  | [`auto-revert-buffer-in-progress`](#index-auto_002drevert_002dbuffer_002din_002dprogress) | [Reverting](#Reverting) |
|  | [`auto-save-default`](#index-auto_002dsave_002ddefault) | [Auto-Saving](#Auto_002dSaving) |
|  | [`auto-save-file-name-p`](#index-auto_002dsave_002dfile_002dname_002dp) | [Auto-Saving](#Auto_002dSaving) |
|  | [`auto-save-file-name-transforms`](#index-auto_002dsave_002dfile_002dname_002dtransforms) | [Auto-Saving](#Auto_002dSaving) |
|  | [`auto-save-hook`](#index-auto_002dsave_002dhook) | [Auto-Saving](#Auto_002dSaving) |
|  | [`auto-save-interval`](#index-auto_002dsave_002dinterval) | [Auto-Saving](#Auto_002dSaving) |
|  | [`auto-save-list-file-name`](#index-auto_002dsave_002dlist_002dfile_002dname) | [Auto-Saving](#Auto_002dSaving) |
|  | [`auto-save-list-file-prefix`](#index-auto_002dsave_002dlist_002dfile_002dprefix) | [Auto-Saving](#Auto_002dSaving) |
|  | [`auto-save-mode`](#index-auto_002dsave_002dmode) | [Auto-Saving](#Auto_002dSaving) |
|  | [`auto-save-timeout`](#index-auto_002dsave_002dtimeout) | [Auto-Saving](#Auto_002dSaving) |
|  | [`auto-save-visited-file-name`](#index-auto_002dsave_002dvisited_002dfile_002dname) | [Auto-Saving](#Auto_002dSaving) |
|  | [auto-selection of window](#index-auto_002dselection-of-window) | [Mouse Window Auto-selection](#Mouse-Window-Auto_002dselection) |
|  | [`auto-window-vscroll`](#index-auto_002dwindow_002dvscroll) | [Vertical Scrolling](#Vertical-Scrolling) |
|  | [`autoload`](#index-autoload-1) | [Autoload](#Autoload) |
|  | [autoload](#index-autoload) | [Autoload](#Autoload) |
|  | [autoload by prefix](#index-autoload-by-prefix) | [Autoload by Prefix](#Autoload-by-Prefix) |
|  | [autoload cookie](#index-autoload-cookie) | [Autoload](#Autoload) |
|  | [autoload cookie, and safe values of variable](#index-autoload-cookie_002c-and-safe-values-of-variable) | [File Local Variables](#File-Local-Variables) |
|  | [autoload errors](#index-autoload-errors) | [Autoload](#Autoload) |
|  | [autoload object](#index-autoload-object) | [What Is a Function](#What-Is-a-Function) |
|  | [`autoload-compute-prefixes`](#index-autoload_002dcompute_002dprefixes) | [Autoload by Prefix](#Autoload-by-Prefix) |
|  | [`autoload-do-load`](#index-autoload_002ddo_002dload) | [Autoload](#Autoload) |
|  | [`autoload-macro` (`declare` spec)](#index-autoload_002dmacro-_0028declare-spec_0029) | [Declare Form](#Declare-Form) |
|  | [autoload, when to use](#index-autoload_002c-when-to-use) | [When to Autoload](#When-to-Autoload) |
|  | [`autoloadp`](#index-autoloadp) | [Autoload](#Autoload) |
|  | [automatic face assignment](#index-automatic-face-assignment) | [Auto Faces](#Auto-Faces) |
|  | [automatically buffer-local](#index-automatically-buffer_002dlocal) | [Intro to Buffer-Local](#Intro-to-Buffer_002dLocal) |
| --- | | |
| B |
|  | [`back-to-indentation`](#index-back_002dto_002dindentation) | [Motion by Indent](#Motion-by-Indent) |
|  | [`background-color, a frame parameter`](#index-background_002dcolor_002c-a-frame-parameter) | [Font and Color Parameters](#Font-and-Color-Parameters) |
|  | [`background-mode, a frame parameter`](#index-background_002dmode_002c-a-frame-parameter) | [Font and Color Parameters](#Font-and-Color-Parameters) |
|  | [backing store](#index-backing-store) | [Display Feature Testing](#Display-Feature-Testing) |
|  | [backquote (list substitution)](#index-backquote-_0028list-substitution_0029) | [Backquote](#Backquote) |
|  | [backquote-style patterns](#index-backquote_002dstyle-patterns) | [Backquote Patterns](#Backquote-Patterns) |
|  | [`backref` in rx](#index-backref-in-rx) | [Rx Constructs](#Rx-Constructs) |
|  | [backslash in character constants](#index-backslash-in-character-constants) | [General Escape Syntax](#General-Escape-Syntax) |
|  | [backslash in regular expressions](#index-backslash-in-regular-expressions) | [Regexp Backslash](#Regexp-Backslash) |
|  | [backslash in strings](#index-backslash-in-strings) | [Syntax for Strings](#Syntax-for-Strings) |
|  | [backslash in symbols](#index-backslash-in-symbols) | [Symbol Type](#Symbol-Type) |
|  | [backspace](#index-backspace) | [Basic Char Syntax](#Basic-Char-Syntax) |
|  | [`backtrace`](#index-backtrace) | [Internals of Debugger](#Internals-of-Debugger) |
|  | [backtrace buffer](#index-backtrace-buffer) | [Backtraces](#Backtraces) |
|  | [backtrace from emacsclient’s `--eval`](#index-backtrace-from-emacsclient_0027s-_002d_002deval) | [Error Debugging](#Error-Debugging) |
|  | [backtrace of thread](#index-backtrace-of-thread) | [The Thread List](#The-Thread-List) |
|  | [`backtrace-debug`](#index-backtrace_002ddebug) | [Internals of Debugger](#Internals-of-Debugger) |
|  | [`backtrace-frame`](#index-backtrace_002dframe) | [Internals of Debugger](#Internals-of-Debugger) |
|  | [`backtrace-on-error-noninteractive`](#index-backtrace_002don_002derror_002dnoninteractive) | [Invoking the Debugger](#Invoking-the-Debugger) |
|  | [`backtrace-on-redisplay-error`](#index-backtrace_002don_002dredisplay_002derror) | [Debugging Redisplay](#Debugging-Redisplay) |
|  | [backtracking](#index-backtracking) | [Backtracking](#Backtracking) |
|  | [backtracking and POSIX regular expressions](#index-backtracking-and-POSIX-regular-expressions) | [Longest Match](#Longest-Match) |
|  | [backtracking and regular expressions](#index-backtracking-and-regular-expressions) | [Regexp Special](#Regexp-Special) |
|  | [backup file](#index-backup-file) | [Backup Files](#Backup-Files) |
|  | [backup files, rename or copy](#index-backup-files_002c-rename-or-copy) | [Rename or Copy](#Rename-or-Copy) |
|  | [`backup-buffer`](#index-backup_002dbuffer) | [Making Backups](#Making-Backups) |
|  | [`backup-by-copying`](#index-backup_002dby_002dcopying) | [Rename or Copy](#Rename-or-Copy) |
|  | [`backup-by-copying-when-linked`](#index-backup_002dby_002dcopying_002dwhen_002dlinked) | [Rename or Copy](#Rename-or-Copy) |
|  | [`backup-by-copying-when-mismatch`](#index-backup_002dby_002dcopying_002dwhen_002dmismatch) | [Rename or Copy](#Rename-or-Copy) |
|  | [`backup-by-copying-when-privileged-mismatch`](#index-backup_002dby_002dcopying_002dwhen_002dprivileged_002dmismatch) | [Rename or Copy](#Rename-or-Copy) |
|  | [`backup-directory-alist`](#index-backup_002ddirectory_002dalist) | [Making Backups](#Making-Backups) |
|  | [`backup-enable-predicate`](#index-backup_002denable_002dpredicate) | [Making Backups](#Making-Backups) |
|  | [`backup-file-name-p`](#index-backup_002dfile_002dname_002dp) | [Backup Names](#Backup-Names) |
|  | [`backup-inhibited`](#index-backup_002dinhibited) | [Making Backups](#Making-Backups) |
|  | [backups and auto-saving](#index-backups-and-auto_002dsaving) | [Backups and Auto-Saving](#Backups-and-Auto_002dSaving) |
|  | [`backward-button`](#index-backward_002dbutton) | [Button Buffer Commands](#Button-Buffer-Commands) |
|  | [`backward-char`](#index-backward_002dchar) | [Character Motion](#Character-Motion) |
|  | [`backward-delete-char-untabify`](#index-backward_002ddelete_002dchar_002duntabify) | [Deletion](#Deletion) |
|  | [`backward-delete-char-untabify-method`](#index-backward_002ddelete_002dchar_002duntabify_002dmethod) | [Deletion](#Deletion) |
|  | [`backward-list`](#index-backward_002dlist) | [List Motion](#List-Motion) |
|  | [`backward-prefix-chars`](#index-backward_002dprefix_002dchars) | [Motion and Syntax](#Motion-and-Syntax) |
|  | [`backward-sentence`](#index-backward_002dsentence) | [List Motion](#List-Motion) |
|  | [`backward-sexp`](#index-backward_002dsexp) | [List Motion](#List-Motion) |
|  | [`backward-sexp, and tree-sitter`](#index-backward_002dsexp_002c-and-tree_002dsitter) | [List Motion](#List-Motion) |
|  | [`backward-to-indentation`](#index-backward_002dto_002dindentation) | [Motion by Indent](#Motion-by-Indent) |
|  | [`backward-up-list`](#index-backward_002dup_002dlist) | [List Motion](#List-Motion) |
|  | [`backward-word`](#index-backward_002dword) | [Word Motion](#Word-Motion) |
|  | [`backward-word-strictly`](#index-backward_002dword_002dstrictly) | [Word Motion](#Word-Motion) |
|  | [`balance-windows`](#index-balance_002dwindows) | [Resizing Windows](#Resizing-Windows) |
|  | [`balance-windows-area`](#index-balance_002dwindows_002darea) | [Resizing Windows](#Resizing-Windows) |
|  | [balanced parenthesis motion](#index-balanced-parenthesis-motion) | [List Motion](#List-Motion) |
|  | [balancing parentheses](#index-balancing-parentheses) | [Blinking](#Blinking) |
|  | [balancing window sizes](#index-balancing-window-sizes) | [Resizing Windows](#Resizing-Windows) |
|  | [bare symbol](#index-bare-symbol) | [Symbols with Position](#Symbols-with-Position) |
|  | [`bare-symbol`](#index-bare_002dsymbol) | [Symbols with Position](#Symbols-with-Position) |
|  | [`barf-if-buffer-read-only`](#index-barf_002dif_002dbuffer_002dread_002donly) | [Read Only Buffers](#Read-Only-Buffers) |
|  | [base 64 encoding](#index-base-64-encoding) | [Base 64](#Base-64) |
|  | [base buffer](#index-base-buffer) | [Indirect Buffers](#Indirect-Buffers) |
|  | [base coding system](#index-base-coding-system) | [Coding System Basics](#Coding-System-Basics) |
|  | [base direction of a paragraph](#index-base-direction-of-a-paragraph) | [Bidirectional Display](#Bidirectional-Display) |
|  | [base for reading an integer](#index-base-for-reading-an-integer) | [Integer Basics](#Integer-Basics) |
|  | [base remapping, faces](#index-base-remapping_002c-faces) | [Face Remapping](#Face-Remapping) |
|  | [base type, in bindat specification](#index-base-type_002c-in-bindat-specification) | [Bindat Types](#Bindat-Types) |
|  | [`base64-decode-region`](#index-base64_002ddecode_002dregion) | [Base 64](#Base-64) |
|  | [`base64-decode-string`](#index-base64_002ddecode_002dstring) | [Base 64](#Base-64) |
|  | [`base64-encode-region`](#index-base64_002dencode_002dregion) | [Base 64](#Base-64) |
|  | [`base64-encode-string`](#index-base64_002dencode_002dstring) | [Base 64](#Base-64) |
|  | [`base64url-encode-region`](#index-base64url_002dencode_002dregion) | [Base 64](#Base-64) |
|  | [`base64url-encode-string`](#index-base64url_002dencode_002dstring) | [Base 64](#Base-64) |
|  | [basic code (of input character)](#index-basic-code-_0028of-input-character_0029) | [Keyboard Events](#Keyboard-Events) |
|  | [basic faces](#index-basic-faces) | [Basic Faces](#Basic-Faces) |
|  | [batch mode](#index-batch-mode) | [Batch Mode](#Batch-Mode) |
|  | [`batch-byte-compile`](#index-batch_002dbyte_002dcompile) | [Compilation Functions](#Compilation-Functions) |
|  | [`batch-native-compile`](#index-batch_002dnative_002dcompile) | [Native-Compilation Functions](#Native_002dCompilation-Functions) |
|  | [`baud-rate`](#index-baud_002drate) | [Terminal Output](#Terminal-Output) |
|  | [baud, in serial connections](#index-baud_002c-in-serial-connections) | [Serial Ports](#Serial-Ports) |
|  | [`beep`](#index-beep) | [Beeping](#Beeping) |
|  | [before point, insertion](#index-before-point_002c-insertion) | [Insertion](#Insertion) |
|  | [`before-change-functions`](#index-before_002dchange_002dfunctions) | [Change Hooks](#Change-Hooks) |
|  | [`before-hack-local-variables-hook`](#index-before_002dhack_002dlocal_002dvariables_002dhook) | [File Local Variables](#File-Local-Variables) |
|  | [`before-init-hook`](#index-before_002dinit_002dhook) | [Init File](#Init-File) |
|  | [`before-init-time`](#index-before_002dinit_002dtime) | [Startup Summary](#Startup-Summary) |
|  | [`before-make-frame-hook`](#index-before_002dmake_002dframe_002dhook) | [Creating Frames](#Creating-Frames) |
|  | [`before-revert-hook`](#index-before_002drevert_002dhook) | [Reverting](#Reverting) |
|  | [`before-save-hook`](#index-before_002dsave_002dhook) | [Saving Buffers](#Saving-Buffers) |
|  | [`before-string (overlay property)`](#index-before_002dstring-_0028overlay-property_0029) | [Overlay Properties](#Overlay-Properties) |
|  | [beginning of line](#index-beginning-of-line) | [Text Lines](#Text-Lines) |
|  | [beginning of line in regexp](#index-beginning-of-line-in-regexp) | [Regexp Special](#Regexp-Special) |
|  | [`beginning-of-buffer`](#index-beginning_002dof_002dbuffer) | [Buffer End Motion](#Buffer-End-Motion) |
|  | [`beginning-of-defun`](#index-beginning_002dof_002ddefun) | [List Motion](#List-Motion) |
|  | [`beginning-of-defun-function`](#index-beginning_002dof_002ddefun_002dfunction) | [List Motion](#List-Motion) |
|  | [`beginning-of-line`](#index-beginning_002dof_002dline) | [Text Lines](#Text-Lines) |
|  | [bell](#index-bell) | [Beeping](#Beeping) |
|  | [bell character](#index-bell-character) | [Basic Char Syntax](#Basic-Char-Syntax) |
|  | [`benchmark.el`](#index-benchmark_002eel) | [Profiling](#Profiling) |
|  | [benchmarking](#index-benchmarking) | [Profiling](#Profiling) |
|  | [best practices](#index-best-practices) | [Tips](#Tips) |
|  | [`bidi-display-reordering`](#index-bidi_002ddisplay_002dreordering) | [Bidirectional Display](#Bidirectional-Display) |
|  | [`bidi-find-overridden-directionality`](#index-bidi_002dfind_002doverridden_002ddirectionality) | [Bidirectional Display](#Bidirectional-Display) |
|  | [`bidi-paragraph-direction`](#index-bidi_002dparagraph_002ddirection) | [Bidirectional Display](#Bidirectional-Display) |
|  | [`bidi-paragraph-separate-re`](#index-bidi_002dparagraph_002dseparate_002dre) | [Bidirectional Display](#Bidirectional-Display) |
|  | [`bidi-paragraph-start-re`](#index-bidi_002dparagraph_002dstart_002dre) | [Bidirectional Display](#Bidirectional-Display) |
|  | [`bidi-string-mark-left-to-right`](#index-bidi_002dstring_002dmark_002dleft_002dto_002dright) | [Bidirectional Display](#Bidirectional-Display) |
|  | [bidirectional class of characters](#index-bidirectional-class-of-characters) | [Character Properties](#Character-Properties) |
|  | [bidirectional display](#index-bidirectional-display) | [Bidirectional Display](#Bidirectional-Display) |
|  | [bidirectional reordering](#index-bidirectional-reordering) | [Bidirectional Display](#Bidirectional-Display) |
|  | [big endian, in bindat specification](#index-big-endian_002c-in-bindat-specification) | [Bindat Types](#Bindat-Types) |
|  | [bignum range](#index-bignum-range) | [Integer Basics](#Integer-Basics) |
|  | [`bignump`](#index-bignump) | [Predicates on Numbers](#Predicates-on-Numbers) |
|  | [`binary coding system`](#index-binary-coding-system) | [Coding System Basics
