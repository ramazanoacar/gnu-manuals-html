### 21.5 Initial Input [¶](#Initial-Input-1)

Several of the functions for minibuffer input have an argument called
initial. This is a mostly-deprecated
feature for specifying that the minibuffer should start out with
certain text, instead of empty as usual.

If initial is a string, the minibuffer starts out containing the
text of the string, with point at the end, when the user starts to
edit the text. If the user simply types `RET` to exit the
minibuffer, it will use the initial input string to determine the
value to return.

**We discourage use of a non-`nil` value for
initial**, because initial input is an intrusive interface.
History lists and default values provide a much more convenient method
to offer useful default inputs to the user.

There is just one situation where you should specify a string for an
initial argument. This is when you specify a cons cell for the
history argument. See [Minibuffer History](#Minibuffer-History).

initial can also be a cons cell of the form `(string
. position)`. This means to insert string in the
minibuffer but put point at position within the string’s text.

As a historical accident, position was implemented
inconsistently in different functions. In `completing-read`,
position’s value is interpreted as origin-zero; that is, a value
of 0 means the beginning of the string, 1 means after the first
character, etc. In `read-minibuffer`, and the other
non-completion minibuffer input functions that support this argument,
1 means the beginning of the string, 2 means after the first character,
etc.

Use of a cons cell as the value for initial arguments is deprecated.

---

Next: [Yes-or-No Queries](#Yes_002dor_002dNo-Queries), Previous: [Initial Input](#Initial-Input), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 21.6 Completion [¶](#Completion-1)

*Completion* is a feature that fills in the rest of a name
starting from an abbreviation for it. Completion works by comparing the
user’s input against a list of valid names and determining how much of
the name is determined uniquely by what the user has typed. For
example, when you type `C-x b` (`switch-to-buffer`) and then
type the first few letters of the name of the buffer to which you wish
to switch, and then type `TAB` (`minibuffer-complete`), Emacs
extends the name as far as it can.

Standard Emacs commands offer completion for names of symbols, files,
buffers, and processes; with the functions in this section, you can
implement completion for other kinds of names.

The `try-completion` function is the basic primitive for
completion: it returns the longest determined completion of a given
initial string, with a given set of strings to match against.

The function `completing-read` provides a higher-level interface
for completion. A call to `completing-read` specifies how to
determine the list of valid names. The function then activates the
minibuffer with a local keymap that binds a few keys to commands useful
for completion. Other functions provide convenient simple interfaces
for reading certain kinds of names with completion.

* [Basic Completion Functions](#Basic-Completion)
* [Completion and the Minibuffer](#Minibuffer-Completion)
* [Minibuffer Commands that Do Completion](#Completion-Commands)
* [High-Level Completion Functions](#High_002dLevel-Completion)
* [Reading File Names](#Reading-File-Names)
* [Completion Variables](#Completion-Variables)
* [Programmed Completion](#Programmed-Completion)
* [Completion in Ordinary Buffers](#Completion-in-Buffers)

---

Next: [Completion and the Minibuffer](#Minibuffer-Completion), Up: [Completion](#Completion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 21.6.1 Basic Completion Functions [¶](#Basic-Completion-Functions)

The following completion functions have nothing in themselves to do
with minibuffers. We describe them here to keep them near the
higher-level completion features that do use the minibuffer.

Function: **try-completion** string collection &optional predicate [¶](#index-try_002dcompletion)
:   This function returns the longest common substring of all possible
    completions of string in collection.

    collection is called the *completion table*. Its value must
    be a list of strings or cons cells, an obarray, a hash table, or a
    completion function.

    `try-completion` compares string against each of the
    permissible completions specified by the completion table. If no
    permissible completions match, it returns `nil`. If there is
    just one matching completion, and the match is exact, it returns
    `t`. Otherwise, it returns the longest initial sequence common
    to all possible matching completions.

    If collection is a list, the permissible completions are
    specified by the elements of the list, each of which should be either
    a string, or a cons cell whose CAR is either a string or a symbol
    (a symbol is converted to a string using `symbol-name`). If the
    list contains elements of any other type, those are ignored.

    If collection is an obarray (see [Creating and Interning Symbols](#Creating-Symbols)), the names
    of all symbols in the obarray form the set of permissible completions.

    If collection is a hash table, then the keys that are strings or
    symbols are the possible completions. Other keys are ignored.

    You can also use a function as collection. Then the function is
    solely responsible for performing completion; `try-completion`
    returns whatever this function returns. The function is called with
    three arguments: string, predicate and `nil` (the
    third argument is so that the same function can be used
    in `all-completions` and do the appropriate thing in either
    case). See [Programmed Completion](#Programmed-Completion).

    If the argument predicate is non-`nil`, then it must be a
    function of one argument, unless collection is a hash table, in
    which case it should be a function of two arguments. It is used to
    test each possible match, and the match is accepted only if
    predicate returns non-`nil`. The argument given to
    predicate is either a string or a cons cell (the CAR of
    which is a string) from the alist, or a symbol (*not* a symbol
    name) from the obarray. If collection is a hash table,
    predicate is called with two arguments, the string key and the
    associated value.

    In addition, to be acceptable, a completion must also match all the
    regular expressions in `completion-regexp-list`. (Unless
    collection is a function, in which case that function has to
    handle `completion-regexp-list` itself.)

    In the first of the following examples, the string ‘`foo`’ is
    matched by three of the alist CARs. All of the matches begin with
    the characters ‘`fooba`’, so that is the result. In the second
    example, there is only one possible match, and it is exact, so the
    return value is `t`.

    ```
    (try-completion
     "foo"
     '(("foobar1" 1) ("barfoo" 2) ("foobaz" 3) ("foobar2" 4)))
         ⇒ "fooba"
    ```

    ```

    ```

    ```
    (try-completion "foo" '(("barfoo" 2) ("foo" 3)))
         ⇒ t
    ```

    In the following example, numerous symbols begin with the characters
    ‘`forw`’, and all of them begin with the word ‘`forward`’. In
    most of the symbols, this is followed with a ‘`-`’, but not in all,
    so no more than ‘`forward`’ can be completed.

    ```
    (try-completion "forw" obarray)
         ⇒ "forward"
    ```

    Finally, in the following example, only two of the three possible
    matches pass the predicate `test` (the string ‘`foobaz`’ is
    too short). Both of those begin with the string ‘`foobar`’.

    ```
    (defun test (s)
      (> (length (car s)) 6))
         ⇒ test
    ```

    ```
    (try-completion
     "foo"
     '(("foobar1" 1) ("barfoo" 2) ("foobaz" 3) ("foobar2" 4))
     'test)
         ⇒ "foobar"
    ```

Function: **all-completions** string collection &optional predicate [¶](#index-all_002dcompletions)
:   This function returns a list of all possible completions of
    string. The arguments to this function
    are the same as those of `try-completion`, and it
    uses `completion-regexp-list` in the same way that
    `try-completion` does.

    If collection is a function, it is called with three arguments:
    string, predicate and `t`; then `all-completions`
    returns whatever the function returns. See [Programmed Completion](#Programmed-Completion).

    Here is an example, using the function `test` shown in the
    example for `try-completion`:

    ```
    (defun test (s)
      (> (length (car s)) 6))
         ⇒ test
    ```

    ```

    ```

    ```
    (all-completions
     "foo"
     '(("foobar1" 1) ("barfoo" 2) ("foobaz" 3) ("foobar2" 4))
     'test)
         ⇒ ("foobar1" "foobar2")
    ```

Function: **test-completion** string collection &optional predicate [¶](#index-test_002dcompletion)
:   This function returns non-`nil` if string is a valid
    completion alternative specified by collection and
    predicate. The arguments are the same as in
    `try-completion`. For instance, if collection is a list of
    strings, this is true if string appears in the list and
    predicate is satisfied.

    This function uses `completion-regexp-list` in the same
    way that `try-completion` does.

    If predicate is non-`nil` and if collection contains
    several strings that are equal to each other, as determined by
    `compare-strings` according to `completion-ignore-case`,
    then predicate should accept either all or none of them.
    Otherwise, the return value of `test-completion` is essentially
    unpredictable.

    If collection is a function, it is called with three arguments,
    the values string, predicate and `lambda`; whatever
    it returns, `test-completion` returns in turn.

Function: **completion-boundaries** string collection predicate suffix [¶](#index-completion_002dboundaries)
:   This function returns the boundaries of the field on which collection
    will operate, assuming that string holds the text before point
    and suffix holds the text after point.

    Normally completion operates on the whole string, so for all normal
    collections, this will always return `(0 . (length
    suffix))`. But more complex completion, such as completion on
    files, is done one field at a time. For example, completion of
    `"/usr/sh"` will include `"/usr/share/"` but not
    `"/usr/share/doc"` even if `"/usr/share/doc"` exists.
    Also `all-completions` on `"/usr/sh"` will not include
    `"/usr/share/"` but only `"share/"`. So if string is
    `"/usr/sh"` and suffix is `"e/doc"`,
    `completion-boundaries` will return `(5 . 1)` which tells us
    that the collection will only return completion information that
    pertains to the area after `"/usr/"` and before `"/doc"`.
    `try-completion` is not affected by nontrivial boundaries; e.g.,
    `try-completion` on `"/usr/sh"` might still return
    `"/usr/share/"`, not `"share/"`.

If you store a completion alist in a variable, you should mark the
variable as risky by giving it a non-`nil`
`risky-local-variable` property. See [File Local Variables](#File-Local-Variables).

Variable: **completion-ignore-case** [¶](#index-completion_002dignore_002dcase)
:   If the value of this variable is non-`nil`, case is not
    considered significant in completion. Within `read-file-name`,
    this variable is overridden by
    `read-file-name-completion-ignore-case` (see [Reading File Names](#Reading-File-Names)); within `read-buffer`, it is overridden by
    `read-buffer-completion-ignore-case` (see [High-Level Completion Functions](#High_002dLevel-Completion)).

Variable: **completion-regexp-list** [¶](#index-completion_002dregexp_002dlist)
:   This is a list of regular expressions. The completion functions only
    consider a completion acceptable if it matches all regular expressions
    in this list, with `case-fold-search` (see [Searching and Case](#Searching-and-Case))
    bound to the value of `completion-ignore-case`.

    Do not set this variable to a non-`nil` value globally, as that
    is not safe and will probably cause errors in completion commands.
    This variable should be only let-bound to non-`nil` values around
    calls to basic completion functions: `try-completion`,
    `test-completion`, and `all-completions`.

Macro: **lazy-completion-table** var fun [¶](#index-lazy_002dcompletion_002dtable)
:   This macro provides a way to initialize the variable var as a
    collection for completion in a lazy way, not computing its actual
    contents until they are first needed. You use this macro to produce a
    value that you store in var. The actual computation of the
    proper value is done the first time you do completion using var.
    It is done by calling fun with no arguments. The
    value fun returns becomes the permanent value of var.

    Here is an example:

    ```
    (defvar foo (lazy-completion-table foo make-my-alist))
    ```

There are several functions that take an existing completion table and
return a modified version. `completion-table-case-fold` returns
a case-insensitive table. `completion-table-in-turn` and
`completion-table-merge` combine multiple input tables in
different ways. `completion-table-subvert` alters a table to use
a different initial prefix. `completion-table-with-quoting`
returns a table suitable for operating on quoted text.
`completion-table-with-predicate` filters a table with a
predicate function. `completion-table-with-terminator` adds a
terminating string.

---

Next: [Minibuffer Commands that Do Completion](#Completion-Commands), Previous: [Basic Completion Functions](#Basic-Completion), Up: [Completion](#Completion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 21.6.2 Completion and the Minibuffer [¶](#Completion-and-the-Minibuffer)

This section describes the basic interface for reading from the
minibuffer with completion.

Function: **completing-read** prompt collection &optional predicate require-match initial history default inherit-input-method [¶](#index-completing_002dread)
:   This function reads a string in the minibuffer, assisting the user by
    providing completion. It activates the minibuffer with prompt
    prompt, which must be a string.

    The actual completion is done by passing the completion table
    collection and the completion predicate predicate to the
    function `try-completion` (see [Basic Completion Functions](#Basic-Completion)). This
    happens in certain commands bound in the local keymaps used for
    completion. Some of these commands also call `test-completion`.
    Thus, if predicate is non-`nil`, it should be compatible
    with collection and `completion-ignore-case`.
    See [Definition of test-completion](#Definition-of-test_002dcompletion).

    See [Programmed Completion](#Programmed-Completion), for detailed requirements when
    collection is a function.

    The value of the optional argument require-match determines how
    the user may exit the minibuffer:

    * If `nil`, the usual minibuffer exit commands work regardless of
      the input in the minibuffer.
    * If `t`, the usual minibuffer exit commands won’t exit unless the
      input completes to an element of collection.
    * If `confirm`, the user can exit with any input, but is asked for
      confirmation if the input is not an element of collection.
    * If `confirm-after-completion`, the user can exit with any input,
      but is asked for confirmation if the preceding command was a
      completion command (i.e., one of the commands in
      `minibuffer-confirm-exit-commands`) and the resulting input is
      not an element of collection. See [Minibuffer Commands that Do Completion](#Completion-Commands).
    * If a function, it is called with the input as the only argument. The
      function should return a non-`nil` value if the input is
      acceptable.
    * Any other value of require-match behaves like `t`, except
      that the exit commands won’t exit if it performs completion.

    However, empty input is always permitted, regardless of the value of
    require-match; in that case, `completing-read` returns the
    first element of default, if it is a list; `""`, if
    default is `nil`; or default. The string or strings
    in default are also available to the user through the history
    commands (see [Minibuffer Commands](#Minibuffer-Commands)). In addition, the completion
    candidates are added to the “future history” when the values in
    default are exhausted by `M-n`; see [minibuffer-default-add-function](#Minibuffer-History).

    The function `completing-read` uses
    `minibuffer-local-completion-map` as the keymap if
    require-match is `nil`, and uses
    `minibuffer-local-must-match-map` if require-match is
    non-`nil`. See [Minibuffer Commands that Do Completion](#Completion-Commands).

    The argument history specifies which history list variable to
    use for saving the input and for minibuffer history commands. It
    defaults to `minibuffer-history`. If history is the symbol
    `t`, history is not recorded. See [Minibuffer History](#Minibuffer-History).

    The argument initial is mostly deprecated; we recommend using a
    non-`nil` value only in conjunction with specifying a cons cell
    for history. See [Initial Input](#Initial-Input). For default input, use
    default instead.

    If the argument inherit-input-method is non-`nil`, then the
    minibuffer inherits the current input method (see [Input Methods](#Input-Methods)) and the setting of `enable-multibyte-characters`
    (see [Text Representations](#Text-Representations)) from whichever buffer was current before
    entering the minibuffer.

    If the variable `completion-ignore-case` is
    non-`nil`, completion ignores case when comparing the input
    against the possible matches. See [Basic Completion Functions](#Basic-Completion). In this mode
    of operation, predicate must also ignore case, or you will get
    surprising results.

    Here’s an example of using `completing-read`:

    ```
    (completing-read
     "Complete a foo: "
     '(("foobar1" 1) ("barfoo" 2) ("foobaz" 3) ("foobar2" 4))
     nil t "fo")
    ```

    ```

    ```

    ```
    ;; After evaluation of the preceding expression,
    ;;   the following appears in the minibuffer:

    ---------- Buffer: Minibuffer ----------
    Complete a foo: fo∗
    ---------- Buffer: Minibuffer ----------
    ```

    If the user then types `DEL DEL b RET`,
    `completing-read` returns `barfoo`.

    The `completing-read` function binds variables to pass
    information to the commands that actually do completion.
    They are described in the following section.

Variable: **completing-read-function** [¶](#index-completing_002dread_002dfunction)
:   The value of this variable must be a function, which is called by
    `completing-read` to actually do its work. It should accept the
    same arguments as `completing-read`. This can be bound to a
    different function to completely override the normal behavior of
    `completing-read`.

If you need to prompt the user for several strings, like several
elements of a list or several parameters (e.g., user, host, and port) of
a connection, you can use `completing-read-multiple`. It allows
typing several strings separated by a separator string (by default, tabs
and commas; customize `crm-separator` to change that), and provides
completion for each individual string the user types. It returns the
strings that were read, as a list.

---

Next: [High-Level Completion Functions](#High_002dLevel-Completion), Previous: [Completion and the Minibuffer](#Minibuffer-Completion), Up: [Completion](#Completion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 21.6.3 Minibuffer Commands that Do Completion [¶](#Minibuffer-Commands-that-Do-Completion)

This section describes the keymaps, commands and user options used
in the minibuffer to do completion.

Variable: **minibuffer-completion-table** [¶](#index-minibuffer_002dcompletion_002dtable)
:   The value of this variable is the completion table (see [Basic Completion Functions](#Basic-Completion)) used for completion in the minibuffer. This is the
    buffer-local variable that contains what `completing-read` passes to
    `try-completion`. It is used by minibuffer completion commands
    such as `minibuffer-complete`.

Variable: **minibuffer-completion-predicate** [¶](#index-minibuffer_002dcompletion_002dpredicate)
:   This variable’s value is the predicate that `completing-read`
    passes to `try-completion`. The variable is also used by the other
    minibuffer completion functions.

Variable: **minibuffer-completion-confirm** [¶](#index-minibuffer_002dcompletion_002dconfirm)
:   This variable determines whether Emacs asks for confirmation before
    exiting the minibuffer; `completing-read` sets this variable,
    and the function `minibuffer-complete-and-exit` checks the value
    before exiting. If the value is `nil`, confirmation is not
    required. If the value is `confirm`, the user may exit with an
    input that is not a valid completion alternative, but Emacs asks for
    confirmation. If the value is `confirm-after-completion`, the
    user may exit with an input that is not a valid completion
    alternative, but Emacs asks for confirmation if the user submitted the
    input right after any of the completion commands in
    `minibuffer-confirm-exit-commands`.

Variable: **minibuffer-confirm-exit-commands** [¶](#index-minibuffer_002dconfirm_002dexit_002dcommands)
:   This variable holds a list of commands that cause Emacs to ask for
    confirmation before exiting the minibuffer, if the require-match
    argument to `completing-read` is `confirm-after-completion`.
    The confirmation is requested if the user attempts to exit the
    minibuffer immediately after calling any command in this list.

Command: **minibuffer-complete-word** [¶](#index-minibuffer_002dcomplete_002dword)
:   This function completes the minibuffer contents by at most a single
    word. Even if the minibuffer contents have only one completion,
    `minibuffer-complete-word` does not add any characters beyond the
    first character that is not a word constituent. See [Syntax Tables](#Syntax-Tables).

Command: **minibuffer-complete** [¶](#index-minibuffer_002dcomplete)
:   This function completes the minibuffer contents as far as possible.

Command: **minibuffer-complete-and-exit** [¶](#index-minibuffer_002dcomplete_002dand_002dexit)
:   This function completes the minibuffer contents, and exits if
    confirmation is not required, i.e., if
    `minibuffer-completion-confirm` is `nil`. If confirmation
    *is* required, it is given by repeating this command
    immediately—the command is programmed to work without confirmation
    when run twice in succession.

Command: **minibuffer-completion-help** [¶](#index-minibuffer_002dcompletion_002dhelp)
:   This function creates a list of the possible completions of the
    current minibuffer contents. It works by calling `all-completions`
    using the value of the variable `minibuffer-completion-table` as
    the collection argument, and the value of
    `minibuffer-completion-predicate` as the predicate argument.
    The list of completions is displayed as text in a buffer named
    `*Completions*`.

Function: **display-completion-list** completions [¶](#index-display_002dcompletion_002dlist)
:   This function displays completions to the stream in
    `standard-output`, usually a buffer. (See [Reading and Printing Lisp Objects](#Read-and-Print), for more
    information about streams.) The argument completions is normally
    a list of completions just returned by `all-completions`, but it
    does not have to be. Each element may be a symbol or a string, either
    of which is simply printed. It can also be a list of two strings,
    which is printed as if the strings were concatenated. The first of
    the two strings is the actual completion, the second string serves as
    annotation.

    This function is called by `minibuffer-completion-help`. A
    common way to use it is together with
    `with-output-to-temp-buffer`, like this:

    ```
    (with-output-to-temp-buffer "*Completions*"
      (display-completion-list
        (all-completions (buffer-string) my-alist)))
    ```

User Option: **completion-auto-help** [¶](#index-completion_002dauto_002dhelp)
:   If this variable is non-`nil`, the completion commands
    automatically display a list of possible completions whenever nothing
    can be completed because the next character is not uniquely determined.

Variable: **minibuffer-local-completion-map** [¶](#index-minibuffer_002dlocal_002dcompletion_002dmap)
:   `completing-read` uses this value as the local keymap when an
    exact match of one of the completions is not required. By default, this
    keymap makes the following bindings:

    `?`
    :   `minibuffer-completion-help`

    `SPC`
    :   `minibuffer-complete-word`

    `TAB`
    :   `minibuffer-complete`

    and uses `minibuffer-local-map` as its parent keymap
    (see [Definition of minibuffer-local-map](#Definition-of-minibuffer_002dlocal_002dmap)).

Variable: **minibuffer-local-must-match-map** [¶](#index-minibuffer_002dlocal_002dmust_002dmatch_002dmap)
:   `completing-read` uses this value as the local keymap when an
    exact match of one of the completions is required. Therefore, no keys
    are bound to `exit-minibuffer`, the command that exits the
    minibuffer unconditionally. By default, this keymap makes the following
    bindings:

    `C-j`
    :   `minibuffer-complete-and-exit`

    `RET`
    :   `minibuffer-complete-and-exit`

    and uses `minibuffer-local-completion-map` as its parent keymap.

Variable: **minibuffer-local-filename-completion-map** [¶](#index-minibuffer_002dlocal_002dfilename_002dcompletion_002dmap)
:   This is a sparse keymap that simply unbinds `SPC`; because
    filenames can contain spaces. The function `read-file-name`
    combines this keymap with either `minibuffer-local-completion-map`
    or `minibuffer-local-must-match-map`.

Variable: **minibuffer-beginning-of-buffer-movement** [¶](#index-minibuffer_002dbeginning_002dof_002dbuffer_002dmovement)
:   If non-`nil`, the `M-<` command will move to the end of the
    prompt if point is after the end of the prompt. If point is at or
    before the end of the prompt, move to the start of the buffer. If
    this variable is `nil`, the command behaves like
    `beginning-of-buffer`.

---

Next: [Reading File Names](#Reading-File-Names), Previous: [Minibuffer Commands that Do Completion](#Completion-Commands), Up: [Completion](#Completion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 21.6.4 High-Level Completion Functions [¶](#High_002dLevel-Completion-Functions)

This section describes the higher-level convenience functions for
reading certain sorts of names with completion.

In most cases, you should not call these functions in the middle of a
Lisp function. When possible, do all minibuffer input as part of
reading the arguments for a command, in the `interactive`
specification. See [Defining Commands](#Defining-Commands).

Function: **read-buffer** prompt &optional default require-match predicate [¶](#index-read_002dbuffer)
:   This function reads the name of a buffer and returns it as a string.
    It prompts with prompt. The argument default is the
    default name to use, the value to return if the user exits with an
    empty minibuffer. If non-`nil`, it should be a string, a list of
    strings, or a buffer. If it is a list, the default value is the first
    element of this list. It is mentioned in the prompt, but is not
    inserted in the minibuffer as initial input.

    The argument prompt should be a string ending with a colon and a
    space. If default is non-`nil`, the function inserts it in
    prompt before the colon to follow the convention for reading from
    the minibuffer with a default value (see [Emacs Programming Tips](#Programming-Tips)).

    The optional argument require-match has the same meaning as in
    `completing-read`. See [Completion and the Minibuffer](#Minibuffer-Completion).

    The optional argument predicate, if non-`nil`, specifies a
    function to filter the buffers that should be considered: the function
    will be called with every potential candidate as its argument, and
    should return `nil` to reject the candidate, non-`nil` to
    accept it.

    In the following example, the user enters ‘`minibuffer.t`’, and
    then types `RET`. The argument require-match is `t`,
    and the only buffer name starting with the given input is
    ‘`minibuffer.texi`’, so that name is the value.

    ```
    (read-buffer "Buffer name: " "foo" t)
    ```

    ```
    ;; After evaluation of the preceding expression,
    ;;   the following prompt appears,
    ;;   with an empty minibuffer:
    ```

    ```

    ```

    ```
    ---------- Buffer: Minibuffer ----------
    Buffer name (default foo): ∗
    ---------- Buffer: Minibuffer ----------
    ```

    ```

    ```

    ```
    ;; The user types minibuffer.t RET.
         ⇒ "minibuffer.texi"
    ```

User Option: **read-buffer-function** [¶](#index-read_002dbuffer_002dfunction)
:   This variable, if non-`nil`, specifies a function for reading
    buffer names. `read-buffer` calls this function instead of doing
    its usual work, with the same arguments passed to `read-buffer`.

User Option: **read-buffer-completion-ignore-case** [¶](#index-read_002dbuffer_002dcompletion_002dignore_002dcase)
:   If this variable is non-`nil`, `read-buffer` ignores case
    when performing completion while reading the buffer name.

Function: **read-command** prompt &optional default [¶](#index-read_002dcommand)
:   This function reads the name of a command and returns it as a Lisp
    symbol. The argument prompt is used as in
    `read-from-minibuffer`. Recall that a command is anything for
    which `commandp` returns `t`, and a command name is a symbol
    for which `commandp` returns `t`. See [Interactive Call](#Interactive-Call).

    The argument default specifies what to return if the user enters
    null input. It can be a symbol, a string or a list of strings. If it
    is a string, `read-command` interns it before returning it.
    If it is a list, `read-command` interns the first element of this list.
    If default is `nil`, that means no default has been
    specified; then if the user enters null input, the return value is
    `(intern "")`, that is, a symbol whose name is an empty string,
    and whose printed representation is `##` (see [Symbol Type](#Symbol-Type)).

    ```
    (read-command "Command name? ")
    ```

    ```
    ;; After evaluation of the preceding expression,
    ;;   the following prompt appears with an empty minibuffer:
    ```

    ```

    ```

    ```
    ---------- Buffer: Minibuffer ----------
    Command name?
    ---------- Buffer: Minibuffer ----------
    ```

    If the user types `forward-c RET`, then this function returns
    `forward-char`.

    The `read-command` function is a simplified interface to
    `completing-read`. It uses the variable `obarray` so as to
    complete in the set of extant Lisp symbols, and it uses the
    `commandp` predicate so as to accept only command names:

    ```
    (read-command prompt)
    ≡
    (intern (completing-read prompt obarray
                             'commandp t nil))
    ```

Function: **read-variable** prompt &optional default [¶](#index-read_002dvariable)
:   This function reads the name of a customizable variable and returns it
    as a symbol. Its arguments have the same form as those of
    `read-command`. It behaves just like `read-command`, except
    that it uses the predicate `custom-variable-p` instead of
    `commandp`.

Command: **read-color** &optional prompt convert allow-empty display foreground face [¶](#index-read_002dcolor)
:   This function reads a string that is a color specification, either the
    color’s name or an RGB hex value such as `#RRRGGGBBB`. It
    prompts with prompt (default: `"Color (name or #RGB triplet):"`)
    and provides completion for color names, but not for hex RGB values.
    In addition to names of standard colors, completion candidates include
    the foreground and background colors at point.

    Valid RGB values are described in [Color Names](#Color-Names).

    The function’s return value is the string typed by the user in the
    minibuffer. However, when called interactively or if the optional
    argument convert is non-`nil`, it converts any input color
    name into the corresponding RGB value string and instead returns that.
    This function requires a valid color specification to be input.
    Empty color names are allowed when allow-empty is
    non-`nil` and the user enters null input.

    Interactively, or when display is non-`nil`, the return
    value is also displayed in the echo area.

    The optional arguments foreground and face control the
    appearance of the completion candidates in the `*Completions*`
    buffer. The candidates are displayed in the specified face but
    with different colors: if foreground is non-`nil`, the
    foreground color is changed to be the color of the candidate,
    otherwise the background is changed to the candidate’s color.

See also the functions `read-coding-system` and
`read-non-nil-coding-system`, in [User-Chosen Coding Systems](#User_002dChosen-Coding-Systems),
and `read-input-method-name`, in [Input Methods](#Input-Methods).

---

Next: [Completion Variables](#Completion-Variables), Previous: [High-Level Completion Functions](#High_002dLevel-Completion), Up: [Completion](#Completion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 21.6.5 Reading File Names [¶](#Reading-File-Names-1)

The high-level completion functions `read-file-name`,
`read-directory-name`, and `read-shell-command` are designed
to read file names, directory names, and shell commands, respectively.
They provide special features, including automatic insertion of the
default directory.

Function: **read-file-name** prompt &optional directory default require-match initial predicate [¶](#index-read_002dfile_002dname)
:   This function reads a file name, prompting with prompt and
    providing completion.

    As an exception, this function reads a file name using a graphical
    file dialog instead of the minibuffer, if all of the following are
    true:

    1. It is invoked via a mouse command.
    2. The selected frame is on a graphical display supporting such dialogs.
    3. The variable `use-dialog-box` is non-`nil`.
       See [Dialog Boxes](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Dialog-Boxes) in The GNU Emacs Manual.
    4. The directory argument, described below, does not specify a
       remote file. See [Remote Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Remote-Files) in The GNU Emacs Manual.

    The exact behavior when using a graphical file dialog is
    platform-dependent. Here, we simply document the behavior when using
    the minibuffer.

    `read-file-name` does not automatically expand the returned file
    name. You can call `expand-file-name` yourself if an absolute
    file name is required.

    The optional argument require-match has the same meaning as in
    `completing-read`. See [Completion and the Minibuffer](#Minibuffer-Completion).

    The argument directory specifies the directory to use for
    completing relative file names. It should be an absolute directory
    name. If the variable `insert-default-directory` is non-`nil`,
    directory is also inserted in the minibuffer as initial input.
    It defaults to the current buffer’s value of `default-directory`.

    If you specify initial, that is an initial file name to insert
    in the buffer (after directory, if that is inserted). In this
    case, point goes at the beginning of initial. The default for
    initial is `nil`—don’t insert any file name. To see what
    initial does, try the command `C-x C-v` in a buffer visiting
    a file. **Please note:** we recommend using default rather
    than initial in most cases.

    If default is non-`nil`, then the function returns
    default if the user exits the minibuffer with the same non-empty
    contents that `read-file-name` inserted initially. The initial
    minibuffer contents are always non-empty if
    `insert-default-directory` is non-`nil`, as it is by
    default. default is not checked for validity, regardless of the
    value of require-match. However, if require-match is
    non-`nil`, the initial minibuffer contents should be a valid file
    (or directory) name. Otherwise `read-file-name` attempts
    completion if the user exits without any editing, and does not return
    default. default is also available through the history
    commands.

    If default is `nil`, `read-file-name` tries to find a
    substitute default to use in its place, which it treats in exactly the
    same way as if it had been specified explicitly. If default is
    `nil`, but initial is non-`nil`, then the default is
    the absolute file name obtained from directory and
    initial. If both default and initial are `nil`
    and the buffer is visiting a file, `read-file-name` uses the
    absolute file name of that file as default. If the buffer is not
    visiting a file, then there is no default. In that case, if the user
    types `RET` without any editing, `read-file-name` simply
    returns the pre-inserted contents of the minibuffer.

    If the user types `RET` in an empty minibuffer, this function
    returns an empty string, regardless of the value of
    require-match. This is, for instance, how the user can make the
    current buffer visit no file using `M-x set-visited-file-name`.

    If predicate is non-`nil`, it specifies a function of one
    argument that decides which file names are acceptable completion
    alternatives. A file name is an acceptable value if predicate
    returns non-`nil` for it.

    Here is an example of using `read-file-name`:

    ```
    (read-file-name "The file is ")

    ;; After evaluation of the preceding expression,
    ;;   the following appears in the minibuffer:
    ```

    ```

    ```

    ```
    ---------- Buffer: Minibuffer ----------
    The file is /gp/gnu/elisp/∗
    ---------- Buffer: Minibuffer ----------
    ```

    Typing `manual TAB` results in the following:

    ```
    ---------- Buffer: Minibuffer ----------
    The file is /gp/gnu/elisp/manual.texi∗
    ---------- Buffer: Minibuffer ----------
    ```

    If the user types `RET`, `read-file-name` returns the file name
    as the string `"/gp/gnu/elisp/manual.texi"`.

Variable: **read-file-name-function** [¶](#index-read_002dfile_002dname_002dfunction)
:   If non-`nil`, this should be a function that accepts the same
    arguments as `read-file-name`. When `read-file-name` is
    called, it calls this function with the supplied arguments instead of
    doing its usual work.

User Option: **read-file-name-completion-ignore-case** [¶](#index-read_002dfile_002dname_002dcompletion_002dignore_002dcase)
:   If this variable is non-`nil`, `read-file-name` ignores case
    when performing completion.

Function: **read-directory-name** prompt &optional directory default require-match initial predicate [¶](#index-read_002ddirectory_002dname)
:   This function is like `read-file-name` but allows only directory
    names as completion alternatives.

    If default is `nil` and initial is non-`nil`,
    `read-directory-name` constructs a substitute default by
    combining directory (or the current buffer’s default directory
    if directory is `nil`) and initial. If both
    default and initial are `nil`, this function uses
    directory as substitute default, or the current buffer’s default
    directory if directory is `nil`.

User Option: **insert-default-directory** [¶](#index-insert_002ddefault_002ddirectory)
:   This variable is used by `read-file-name`, and thus, indirectly,
    by most commands reading file names. (This includes all commands that
    use the code letters ‘`f`’ or ‘`F`’ in their interactive form.
    See [Code Characters for interactive](#Interactive-Codes).) Its
    value controls whether `read-file-name` starts by placing the
    name of the default directory in the minibuffer, plus the initial file
    name, if any. If the value of this variable is `nil`, then
    `read-file-name` does not place any initial input in the
    minibuffer (unless you specify initial input with the initial
    argument). In that case, the default directory is still used for
    completion of relative file names, but is not displayed.

    If this variable is `nil` and the initial minibuffer contents are
    empty, the user may have to explicitly fetch the next history element
    to access a default value. If the variable is non-`nil`, the
    initial minibuffer contents are always non-empty and the user can
    always request a default value by immediately typing `RET` in an
    unedited minibuffer. (See above.)

    For example:

    ```
    ;; Here the minibuffer starts out with the default directory.
    (let ((insert-default-directory t))
      (read-file-name "The file is "))
    ```

    ```

    ```

    ```
    ---------- Buffer: Minibuffer ----------
    The file is ~lewis/manual/∗
    ---------- Buffer: Minibuffer ----------
    ```

    ```

    ```

    ```
    ;; Here the minibuffer is empty and only the prompt
    ;;   appears on its line.
    (let ((insert-default-directory nil))
      (read-file-name "The file is "))
    ```

    ```

    ```

    ```
    ---------- Buffer: Minibuffer ----------
    The file is ∗
    ---------- Buffer: Minibuffer ----------
    ```

Function: **read-shell-command** prompt &optional initial history &rest args [¶](#index-read_002dshell_002dcommand)
:   This function reads a shell command from the minibuffer, prompting
    with prompt and providing intelligent completion. It completes
    the first word of the command using candidates that are appropriate
    for command names, and the rest of the command words as file names.

    This function uses `minibuffer-local-shell-command-map` as the
    keymap for minibuffer input. The history argument specifies the
    history list to use; if is omitted or `nil`, it defaults to
    `shell-command-history` (see [shell-command-history](#Minibuffer-History)). The optional argument initial
    specifies the initial content of the minibuffer (see [Initial Input](#Initial-Input)). The rest of args, if present, are used as the
    default and inherit-input-method arguments in
    `read-from-minibuffer` (see [Reading Text Strings with the Minibuffer](#Text-from-Minibuffer)).

Variable: **minibuffer-local-shell-command-map** [¶](#index-minibuffer_002dlocal_002dshell_002dcommand_002dmap)
:   This keymap is used by `read-shell-command` for completing
    command and file names that are part of a shell command. It uses
    `minibuffer-local-map` as its parent keymap, and binds `TAB`
    to `completion-at-point`.

---

Next: [Programmed Completion](#Programmed-Completion), Previous: [Reading File Names](#Reading-File-Names), Up: [Completion](#Completion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 21.6.6 Completion Variables [¶](#Completion-Variables-1)

Here are some variables that can be used to alter the default
completion behavior.

User Option: **completion-styles** [¶](#index-completion_002dstyles)
:   The value of this variable is a list of completion style (symbols) to
    use for performing completion. A *completion style* is a set of
    rules for generating completions. Each symbol occurring this list
    must have a corresponding entry in `completion-styles-alist`.

Variable: **completion-styles-alist** [¶](#index-completion_002dstyles_002dalist)
:   This variable stores a list of available completion styles. Each
    element in the list has the form

    ```
    (style try-completion all-completions doc)
    ```

    Here, style is the name of the completion style (a symbol),
    which may be used in the `completion-styles` variable to refer to
    this style; try-completion is the function that does the
    completion; all-completions is the function that lists the
    completions; and doc is a string describing the completion
    style.

    The try-completion and all-completions functions should
    each accept four arguments: string, collection,
    predicate, and point. The string, collection,
    and predicate arguments have the same meanings as in
    `try-completion` (see [Basic Completion Functions](#Basic-Completion)), and the point
    argument is the position of point within string. Each function
    should return a non-`nil` value if it performed its job, and
    `nil` if it did not (e.g., if there is no way to complete
    string according to the completion style).

    When the user calls a completion command like
    `minibuffer-complete` (see [Minibuffer Commands that Do Completion](#Completion-Commands)), Emacs looks
    for the first style listed in `completion-styles` and calls its
    try-completion function. If this function returns `nil`,
    Emacs moves to the next listed completion style and calls its
    try-completion function, and so on until one of the
    try-completion functions successfully performs completion and
    returns a non-`nil` value. A similar procedure is used for
    listing completions, via the all-completions functions.

    See [Completion Styles](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Completion-Styles) in The GNU Emacs Manual, for a
    description of the available completion styles.

User Option: **completion-category-overrides** [¶](#index-completion_002dcategory_002doverrides)
:   This variable specifies special completion styles and other completion
    behaviors to use when completing certain types of text. Its value
    should be an alist with elements of the form `(category
    . alist)`. category is a symbol describing what is being
    completed; currently, the `buffer`, `file`, and
    `unicode-name` categories are defined, but others can be defined
    via specialized completion functions (see [Programmed Completion](#Programmed-Completion)).
    alist is an association list describing how completion should
    behave for the corresponding category. The following alist keys are
    supported:

    `styles`
    :   The value should be a list of completion styles (symbols).

    `cycle`
    :   The value should be a value for `completion-cycle-threshold`
        (see [Completion Options](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Completion-Options) in The GNU Emacs Manual) for this
        category.

    `cycle-sort-function`
    :   The function to sort entries when cycling.

    `display-sort-function`
    :   The function to sort entries in the `*Completions*` buffer.
        The possible values are: `nil`, which means to use either the
        sorting function from metadata or if that is `nil`, fall back to
        `completions-sort`; `identity`, which means not to sort at
        all, leaving the original order; or any other value out of those used
        in `completions-sort` (see [Completion Options](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Completion-Options) in The
        GNU Emacs Manual).

    `group-function`
    :   The function to group completions.

    `annotation-function`
    :   The function to add annotations to completions.

    `affixation-function`
    :   The function to add prefixes and suffixes to completions.

    See [Programmed Completion](#Programmed-Completion), for a complete list of metadata entries.

Variable: **completion-extra-properties** [¶](#index-completion_002dextra_002dproperties)
:   This variable is used to specify extra properties of the current
    completion command. It is intended to be let-bound by specialized
    completion commands. Its value should be a list of property and value
    pairs. The following properties are supported:

    `:category`
    :   The value should be a symbol describing what kind of text the
        completion function is trying to complete. If the symbol matches one
        of the keys in `completion-category-overrides` described above,
        the usual completion behavior is overridden.

    `:annotation-function`
    :   The value should be a function to add annotations in the completions
        buffer. This function must accept one argument, a completion, and
        should either return `nil` or a string to be displayed next to
        the completion. Unless this function puts own face on the annotation
        suffix string, the `completions-annotations` face is added by
        default to that string.

    `:affixation-function`
    :   The value should be a function to add prefixes and suffixes to
        completions. This function must accept one argument, a list of
        completions, and should return a list of annotated completions. Each
        element of the returned list must be a three-element list, the
        completion, a prefix string, and a suffix string. This function takes
        priority over `:annotation-function`.

    `:group-function`
    :   The function to group completions.

    `:display-sort-function`
    :   The function to sort entries in the `*Completions*` buffer.

    `:cycle-sort-function`
    :   The function to sort entries when cycling.

    `:exit-function`
    :   The value should be a function to run after performing completion.
        The function should accept two arguments, string and
        status, where string is the text to which the field was
        completed, and status indicates what kind of operation happened:
        `finished` if text is now complete, `sole` if the text
        cannot be further completed but completion is not finished, or
        `exact` if the text is a valid completion but may be further
        completed.

---

Next: [Completion in Ordinary Buffers](#Completion-in-Buffers), Previous: [Completion Variables](#Completion-Variables), Up: [Completion](#Completion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 21.6.7 Programmed Completion [¶](#Programmed-Completion-1)

Sometimes it is not possible or convenient to create an alist or
an obarray containing all the intended possible completions ahead
of time. In such a case, you can supply your own function to compute
the completion of a given string. This is called *programmed
completion*. Emacs uses programmed completion when completing file
names (see [File Name Completion](#File-Name-Completion)), among many other cases.

To use this feature, pass a function as the collection
argument to `completing-read`. The function
`completing-read` arranges to pass your completion function along
to `try-completion`, `all-completions`, and other basic
completion functions, which will then let your function do all
the work.

The completion function should accept three arguments:

* The string to be completed.
* A predicate function with which to filter possible matches, or
  `nil` if none. The function should call the predicate for each
  possible match, and ignore the match if the predicate returns
  `nil`.
* A flag specifying the type of completion operation to perform; see
  [Basic Completion Functions](#Basic-Completion), for the details of those operations. This
  flag may be one of the following values.

  `nil`
  :   This specifies a `try-completion` operation. The function should
      return `nil` if there are no matches; it should return `t`
      if the specified string is a unique and exact match; and it should
      return the longest common prefix substring of all matches otherwise.

  `t`
  :   This specifies an `all-completions` operation. The function
      should return a list of all possible completions of the specified
      string.

  `lambda`
  :   This specifies a `test-completion` operation. The function
      should return `t` if the specified string is an exact match for
      some completion alternative; `nil` otherwise.

  `(boundaries . suffix)`
  :   This specifies a `completion-boundaries` operation. The function
      should return `(boundaries start . end)`, where
      start is the position of the beginning boundary in the specified
      string, and end is the position of the end boundary in
      suffix.

      If a Lisp program returns nontrivial boundaries, it should make sure that the
      `all-completions` operation is consistent with them. The
      completions returned by `all-completions` should only pertain to
      the piece of the prefix and suffix covered by the completion
      boundaries. See [Basic Completion Functions](#Basic-Completion), for the precise expected semantics
      of completion boundaries.

  `metadata` [¶](#index-completion-metadata)
  :   This specifies a request for information about the state of the
      current completion. The return value should have the form
      `(metadata . alist)`, where alist is an alist whose
      elements are described below.

  If the flag has any other value, the completion function should return
  `nil`.

The following is a list of metadata entries that a completion function
may return in response to a `metadata` flag argument:

`category` [¶](#index-category_002c-in-completion)
:   The value should be a symbol describing what kind of text the
    completion function is trying to complete. If the symbol matches one
    of the keys in `completion-category-overrides`, the usual
    completion behavior is overridden. See [Completion Variables](#Completion-Variables).

`annotation-function` [¶](#index-annotation_002dfunction_002c-in-completion)
:   The value should be a function for *annotating* completions. The
    function should take one argument, string, which is a possible
    completion. It should return a string, which is displayed after the
    completion string in the `*Completions*` buffer.
    Unless this function puts own face on the annotation suffix string,
    the `completions-annotations` face is added by default to
    that string.

`affixation-function` [¶](#index-affixation_002dfunction_002c-in-completion)
:   The value should be a function for adding prefixes and suffixes to
    completions. The function should take one argument,
    completions, which is a list of possible completions. It should
    return such a list of completions where each element contains a list
    of three elements: a completion, a prefix which is displayed before
    the completion string in the `*Completions*` buffer, and
    a suffix displayed after the completion string. This function
    takes priority over `annotation-function`.

`group-function` [¶](#index-group_002dfunction_002c-in-completion)
:   The value should be a function for grouping the completion candidates.
    The function must take two arguments, completion, which is a
    completion candidate and transform, which is a boolean flag. If
    transform is `nil`, the function must return the group
    title of the group to which the candidate belongs. The returned title
    can also be `nil`. Otherwise the function must return the
    transformed candidate. The transformation can for example remove a
    redundant prefix, which is displayed in the group title.

`display-sort-function` [¶](#index-display_002dsort_002dfunction_002c-in-completion)
:   The value should be a function for sorting completions. The function
    should take one argument, a list of completion strings, and return a
    sorted list of completion strings. It is allowed to alter the input
    list destructively.

`cycle-sort-function` [¶](#index-cycle_002dsort_002dfunction_002c-in-completion)
:   The value should be a function for sorting completions, when
    `completion-cycle-threshold` is non-`nil` and the user is
    cycling through completion alternatives. See [Completion Options](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Completion-Options) in The GNU Emacs Manual. Its argument list and return value are
    the same as for `display-sort-function`.

Function: **completion-table-dynamic** function &optional switch-buffer [¶](#index-completion_002dtable_002ddynamic)
:   This function is a convenient way to write a function that can act as
    a programmed completion function. The argument function should
    be a function that takes one argument, a string, and returns a
    completion table (see [Basic Completion Functions](#Basic-Completion)) containing all the
    possible completions. The table returned by function can also
    include elements that don’t match the string argument; they are
    automatically filtered out by `completion-table-dynamic`. In
    particular, function can ignore its argument and return a full
    list of all possible completions. You can think of
    `completion-table-dynamic` as a transducer between function
    and the interface for programmed completion functions.

    If the optional argument switch-buffer is non-`nil`, and
    completion is performed in the minibuffer, function will be
    called with current buffer set to the buffer from which the minibuffer
    was entered.

    The return value of `completion-table-dynamic` is a function that
    can be used as the 2nd argument to `try-completion` and
    `all-completions`. Note that this function will always return
    empty metadata and trivial boundaries.

Function: **completion-table-with-cache** function &optional ignore-case [¶](#index-completion_002dtable_002dwith_002dcache)
:   This is a wrapper for `completion-table-dynamic` that saves the
    last argument-result pair. This means that multiple lookups with the
    same argument only need to call function once. This can be useful
    when a slow operation is involved, such as calling an external process.

Function: **completion-table-with-metadata** table metadata [¶](#index-completion_002dtable_002dwith_002dmetadata)
:   This function creates a completion table that behaves like the existing
    completion table table, except that the new table uses the alist
    metadata as its completion metadata.

---

Previous: [Programmed Completion](#Programmed-Completion), Up: [Completion](#Completion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 21.6.8 Completion in Ordinary Buffers [¶](#Completion-in-Ordinary-Buffers)

Although completion is usually done in the minibuffer, the
completion facility can also be used on the text in ordinary Emacs
buffers. In many major modes, in-buffer completion is performed by
the `C-M-i` or `M-TAB` command, bound to
`completion-at-point`. See [Symbol Completion](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Symbol-Completion) in The GNU
Emacs Manual. This command uses the abnormal hook variable
`completion-at-point-functions`:

Variable: **completion-at-point-functions** [¶](#index-completion_002dat_002dpoint_002dfunctions)
:   The value of this abnormal hook should be a list of functions, which
    are used to compute a completion table (see [Basic Completion Functions](#Basic-Completion)) for
    completing the text at point. It can be used by major modes to
    provide mode-specific completion tables (see [Major Mode Conventions](#Major-Mode-Conventions)).

    When the command `completion-at-point` runs, it calls the
    functions in the list one by one, without any argument. Each function
    should return `nil` unless it can and wants to take
    responsibility for the completion data for the text at point.
    Otherwise it should return a list of the following form:

    ```
    (start end collection . props)
    ```

    start and end delimit the text to complete (which should
    enclose point). collection is a completion table for completing
    that text, in a form suitable for passing as the second argument to
    `try-completion` (see [Basic Completion Functions](#Basic-Completion)); completion
    alternatives will be generated from this completion table in the usual
    way, via the completion styles defined in `completion-styles`
    (see [Completion Variables](#Completion-Variables)). props is a property list for
    additional information; any of the properties in
    `completion-extra-properties` are recognized (see [Completion Variables](#Completion-Variables)), as well as the following additional ones:

    `:predicate`
    :   The value should be a predicate that completion candidates need to
        satisfy.

    `:exclusive`
    :   If the value is `no`, then if the completion table fails to match
        the text at point, `completion-at-point` moves on to the
        next function in `completion-at-point-functions` instead of
        reporting a completion failure.

    The functions on this hook should generally return quickly, since they
    may be called very often (e.g., from `post-command-hook`).
    Supplying a function for collection is strongly recommended if
    generating the list of completions is an expensive operation. Emacs
    may internally call functions in `completion-at-point-functions`
    many times, but care about the value of collection for only some
    of these calls. By supplying a function for collection, Emacs
    can defer generating completions until necessary. You can use
    `completion-table-dynamic` to create a wrapper function:

    ```
    ;; Avoid this pattern.
    (let ((beg ...) (end ...) (my-completions (my-make-completions)))
      (list beg end my-completions))

    ;; Use this instead.
    (let ((beg ...) (end ...))
      (list beg
            end
            (completion-table-dynamic
              (lambda (_)
                (my-make-completions)))))
    ```

    Additionally, the collection should generally not be
    pre-filtered based on the current text between start and
    end, because that is the responsibility of the caller of
    `completion-at-point-functions` to do that according to the
    completion styles it decides to use.

    A function in `completion-at-point-functions` may also return a
    function instead of a list as described above. In that case, that
    returned function is called, with no argument, and it is entirely
    responsible for performing the completion. We discourage this usage;
    it is only intended to help convert old code to using
    `completion-at-point`.

    The first function in `completion-at-point-functions` to return a
    non-`nil` value is used by `completion-at-point`. The
    remaining functions are not called. The exception to this is when
    there is an `:exclusive` specification, as described above.

The following function provides a convenient way to perform
completion on an arbitrary stretch of text in an Emacs buffer:

Function: **completion-in-region** start end collection &optional predicate [¶](#index-completion_002din_002dregion)
:   This function completes the text in the current buffer between the
    positions start and end, using collection. The
    argument collection has the same meaning as in
    `try-completion` (see [Basic Completion Functions](#Basic-Completion)).

    This function inserts the completion text directly into the current
    buffer. Unlike `completing-read` (see [Completion and the Minibuffer](#Minibuffer-Completion)), it does not activate the minibuffer.

    For this function to work, point must be somewhere between start
    and end.

---

Next: [Asking Multiple-Choice Questions](#Multiple-Queries), Previous: [Completion](#Completion), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 21.7 Yes-or-No Queries [¶](#Yes_002dor_002dNo-Queries-1)

This section describes functions used to ask the user a yes-or-no
question. The function `y-or-n-p` can be answered with a single
character; it is useful for questions where an inadvertent wrong answer
will not have serious consequences. `yes-or-no-p` is suitable for
more momentous questions, since it requires three or four characters to
answer.

If either of these functions is called in a command that was
invoked using the mouse or some other window-system gesture, or in a
command invoked via a menu, then they use a dialog box or pop-up menu
to ask the question if dialog boxes are supported. Otherwise, they
use keyboard input. You can force use either of the mouse or of
keyboard input by binding `last-nonmenu-event` to a suitable
value around the call—bind it to `t` to force keyboard
interaction, and to a list to force dialog boxes.

Both `yes-or-no-p` and `y-or-n-p` use the minibuffer.

Function: **y-or-n-p** prompt [¶](#index-y_002dor_002dn_002dp)
:   This function asks the user a question, expecting input in the minibuffer.
    It returns `t` if the user types `y`, `nil` if the user
    types `n`. This function also accepts `SPC` to mean yes and
    `DEL` to mean no. It accepts `C-]` and `C-g` to quit,
    because the question uses the minibuffer and for that reason the user
    might try to use `C-]` to get out. The answer is a single
    character, with no `RET` needed to terminate it. Upper and lower
    case are equivalent.

    “Asking the question” means printing prompt in the minibuffer,
    followed by the string ‘`(y or n)`’. If the input is not one of
    the expected answers (`y`, `n`, `SPC`,
    `DEL`, or something that quits), the function responds
    ‘`Please answer y or n.`’, and repeats the request.

    If prompt is a non-empty string, and it ends with a non-space
    character, a ‘`SPC`’ character will be appended to it.

    This function actually uses the minibuffer, but does not allow editing
    of the answer. The cursor moves to the minibuffer while the question
    is being asked.

    The answers and their meanings, even ‘`y`’ and ‘`n`’, are not
    hardwired, and are specified by the keymap `query-replace-map`
    (see [Search and Replace](#Search-and-Replace)). In particular, if the user enters the
    special responses `recenter`, `scroll-up`,
    `scroll-down`, `scroll-other-window`, or
    `scroll-other-window-down` (respectively bound to `C-l`,
    `C-v`, `M-v`, `C-M-v` and `C-M-S-v` in
    `query-replace-map`), this function performs the specified window
    recentering or scrolling operation, and poses the question again.

    If you bind `help-form` (see [Help Functions](#Help-Functions)) to
    a non-`nil` value while calling `y-or-n-p`, then pressing
    `help-char` causes it to evaluate `help-form` and display
    the result. `help-char` is automatically added to prompt.

Function: **y-or-n-p-with-timeout** prompt seconds default [¶](#index-y_002dor_002dn_002dp_002dwith_002dtimeout)
:   Like `y-or-n-p`, except that if the user fails to answer within
    seconds seconds, this function stops waiting and returns
    default. It works by setting up a timer; see [Timers for Delayed Execution](#Timers).
    The argument seconds should be a number.

Function: **yes-or-no-p** prompt [¶](#index-yes_002dor_002dno_002dp)
:   This function asks the user a question, expecting input in the
    minibuffer. It returns `t` if the user enters ‘`yes`’,
    `nil` if the user types ‘`no`’. The user must type `RET` to
    finalize the response. Upper and lower case are equivalent.

    `yes-or-no-p` starts by displaying prompt in the
    minibuffer, followed by the value of `yes-or-no-prompt` (default ‘`(yes or no)`’). The user must type one of the expected
    responses; otherwise, the function responds ‘`Please answer yes or no.`’, waits about two seconds and repeats the request.

    If prompt is a non-empty string, and it ends with a non-space
    character, a ‘`SPC`’ character will be appended to it.

    `yes-or-no-p` requires more work from the user than
    `y-or-n-p` and is appropriate for more crucial decisions.

    Here is an example:

    ```
    (yes-or-no-p "Do you really want to remove everything? ")

    ;; After evaluation of the preceding expression,
    ;;   the following prompt appears,
    ;;   with an empty minibuffer:
    ```

    ```

    ```

    ```
    ---------- Buffer: minibuffer ----------
    Do you really want to remove everything? (yes or no)
    ---------- Buffer: minibuffer ----------
    ```

    If the user first types `y RET`, which is invalid because this
    function demands the entire word ‘`yes`’, it responds by displaying
    these prompts, with a brief pause between them:

    ```
    ---------- Buffer: minibuffer ----------
    Please answer yes or no.
    Do you really want to remove everything? (yes or no)
    ---------- Buffer: minibuffer ----------
    ```

---

Next: [Reading a Password](#Reading-a-Password), Previous: [Yes-or-No Queries](#Yes_002dor_002dNo-Queries), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 21.8 Asking Multiple-Choice Questions [¶](#Asking-Multiple_002dChoice-Questions)

This section describes facilities for asking the user more complex
questions or several similar questions.

When you have a series of similar questions to ask, such as “Do you
want to save this buffer?” for each buffer in turn, you should use
`map-y-or-n-p` to ask the collection of questions, rather than
asking each question individually. This gives the user certain
convenient facilities such as the ability to answer the whole series at
once.

Function: **map-y-or-n-p** prompter actor list &optional help action-alist no-cursor-in-echo-area [¶](#index-map_002dy_002dor_002dn_002dp)
:   This function asks the user a series of questions, reading a
    single-character answer in the minibuffer for each one. However, if
    `y-or-n-p-use-read-key` is non-`nil` (see [Key Sequence Input](#Key-Sequence-Input)), it reads a key sequence from the echo area.

    The value of list specifies the objects to ask questions about.
    It should be either a list of objects or a generator function. If it
    is a function, it will be called with no arguments, and should return
    either the next object to ask about, or `nil`, meaning to stop
    asking questions.

    The argument prompter specifies how to ask each question. If
    prompter is a string, the question text is computed like this:

    ```
    (format prompter object)
    ```

    where object is the next object to ask about (as obtained from
    list). See [Formatting Strings](#Formatting-Strings), for more information about
    `format`.

    If prompter is not a string, it should be a function of one
    argument (the object to ask about) and should return the question text
    for that object. If the value prompter returns is a string,
    that is the question to ask the user. The function can also return
    `t`, meaning to act on this object without asking the user, or
    `nil`, which means to silently ignore this object.

    The argument actor says how to act on the objects for which the
    user answers yes. It should be a function of one argument, and will
    be called with each object from list for which the user answers
    yes.

    If the argument help is given, it should be a list of this form:

    ```
    (singular plural action)
    ```

    where singular is a string containing a singular noun that
    describes a single object to be acted on, plural is the
    corresponding plural noun, and action is a transitive verb
    describing what actor does with the objects.

    If you don’t specify help, it defaults to the list
    `("object" "objects" "act on")`.

    Each time a question is asked, the user can answer as follows:

    `y`, `Y`, or `SPC`
    :   act on the object

    `n`, `N`, or `DEL`
    :   skip the object

    `!`
    :   act on all the following objects

    `ESC` or `q`
    :   exit (skip all following objects)

    `.` (period)
    :   act on the object and then exit

    `C-h`
    :   get help

    These are the same answers that `query-replace` accepts. The
    keymap `query-replace-map` defines their meaning for
    `map-y-or-n-p` as well as for `query-replace`; see
    [Search and Replace](#Search-and-Replace).

    You can use action-alist to specify additional possible answers
    and what they mean. If provided, action-alist should be an
    alist whose elements are of the form `(char function help)`. Each of the alist elements defines one
    additional answer. In each element, char is a character (the
    answer); function is a function of one argument (an object from
    list); and help is a string. When the user responds with
    char, `map-y-or-n-p` calls function. If it returns
    non-`nil`, the object is considered to have been acted upon, and
    `map-y-or-n-p` advances to the next object in list. If it
    returns `nil`, the prompt is repeated for the same object. If
    the user requests help, the text in help is used to describe
    these additional answers.

    Normally, `map-y-or-n-p` binds `cursor-in-echo-area` while
    prompting. But if no-cursor-in-echo-area is non-`nil`, it
    does not do that.

    If `map-y-or-n-p` is called in a command that was invoked using
    the mouse or some other window-system gesture, or a command invoked
    via a menu, then it uses a dialog box or pop-up menu to ask the
    question if dialog boxes are supported. In this case, it does not use
    keyboard input or the echo area. You can force use either of the
    mouse or of keyboard input by binding `last-nonmenu-event` to a
    suitable value around the call—bind it to `t` to force keyboard
    interaction, and to a list to force dialog boxes.

    The return value of `map-y-or-n-p` is the number of objects acted on.

If you need to ask the user a question that might have more than just
2 answers, use `read-answer`.

Function: **read-answer** question answers [¶](#index-read_002danswer)
:   This function prompts the user with text in question, which
    should end in the ‘`SPC`’ character. The function includes in the
    prompt the possible responses in answers by appending them to
    the end of question. The possible responses are provided in
    answers as an alist whose elements are of the following form:

    ```
    (long-answer short-answer help-message)
    ```

    where long-answer is the complete text of the user response, a
    string; short-answer is a short form of the same response, a
    single character or a function key; and help-message is the text
    that describes the meaning of the answer. If the variable
    `read-answer-short` is non-`nil`, the prompt will show the
    short variants of the possible answers and the user is expected to
    type the single characters/keys shown in the prompt; otherwise the
    prompt will show the long variants of the answers, and the user is
    expected to type the full text of one of the answers and end by
    pressing `RET`. If `use-dialog-box` is non-`nil`, and
    this function was invoked by mouse events, the question and the
    answers will be displayed in a GUI dialog box.

    The function returns the text of the long-answer selected by the
    user, regardless of whether long or short answers were shown in the
    prompt and typed by the user.

    Here is an example of using this function:

    ```
    (let ((read-answer-short t))
      (read-answer "Foo "
         '(("yes"  ?y "perform the action")
           ("no"   ?n "skip to the next")
           ("all"  ?! "perform for the rest without more questions")
           ("help" ?h "show help")
           ("quit" ?q "exit"))))
    ```

Function: **read-char-from-minibuffer** prompt &optional chars history [¶](#index-read_002dchar_002dfrom_002dminibuffer)
:   This function uses the minibuffer to read and return a single
    character. Optionally, it ignores any input that is not a member of
    chars, a list of accepted characters. The history
    argument specifies the history list symbol to use; if it is omitted or
    `nil`, this function doesn’t use the history.

    If you bind `help-form` (see [Help Functions](#Help-Functions)) to
    a non-`nil` value while calling `read-char-from-minibuffer`,
    then pressing `help-char` causes it to evaluate `help-form`
    and display the result.

---

Next: [Minibuffer Commands](#Minibuffer-Commands), Previous: [Asking Multiple-Choice Questions](#Multiple-Queries), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 21.9 Reading a Password [¶](#Reading-a-Password-1)

To read a password to pass to another program, you can use the
function `read-passwd`.

Function: **read-passwd** prompt &optional confirm default [¶](#index-read_002dpasswd)
:   This function reads a password, prompting with prompt. It does
    not echo the password as the user types it; instead, it echoes
    ‘`*`’ for each character in the password. If you want to apply
    another character to hide the password, let-bind the variable
    `read-hide-char` with that character.

    The optional argument confirm, if non-`nil`, says to read the
    password twice and insist it must be the same both times. If it isn’t
    the same, the user has to type it over and over until the last two
    times match.

    The optional argument default specifies the default password to
    return if the user enters empty input. If default is `nil`,
    then `read-passwd` returns the null string in that case.

    This function uses `read-passwd-mode`, a minor mode. It binds two
    keys in the minbuffer: `C-u` (`delete-minibuffer-contents`)
    deletes the password, and `TAB`
    (`read-passwd--toggle-visibility`) toggles the visibility of the
    password. There is also an additional icon in the mode-line’s
    `global-mode-string`. Clicking on this icon with `mouse-1`
    toggles the visibility of the password as well.

---

Next: [Minibuffer Windows](#Minibuffer-Windows), Previous: [Reading a Password](#Reading-a-Password), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
