### 25.7 Documentation Groups [¶](#Documentation-Groups-1)

Emacs can list functions based on various groupings. For instance,
`string-trim` and `mapconcat` are “string” functions, so
`M-x shortdoc RET string RET` will give an overview of these and
other functions that operate on strings.

The documentation groups are created with the
`define-short-documentation-group` macro. See [Tips for Documentation Groups](#Documentation-Group-Tips), for how to write good documentation groups.

Macro: **define-short-documentation-group** group &rest functions [¶](#index-define_002dshort_002ddocumentation_002dgroup)
:   Define group as a group of functions, and provide short
    summaries of using those functions. The optional argument
    functions is a list whose elements are of the form:

    ```
    (func [keyword val]...)
    ```

    The following keywords are recognized:

    `:eval`
    :   The value should be a form that has no side effect when evaluated.
        The form will be used in the documentation by printing it with
        `prin1` (see [Output Functions](#Output-Functions)). However, if the form is a
        string, it will be inserted as-is, and the string will then be
        `read` to yield the form. In any case, the form will then be
        evaluated, and the result used. For instance:

        ```
        :eval (concat "foo" "bar" "zot")
        :eval "(make-string 5 ?x)"
        ```

        will result in:

        ```
        (concat "foo" "bar" "zot")
        ⇒ "foobarzot"
        (make-string 5 ?x)
        ⇒ "xxxxx"
        ```

        (The reason for allowing both Lisp forms and strings here is so that
        printing could be controlled in the few cases where a certain
        presentation of the form is wished for. In the example, ‘`?x`’
        would otherwise have been printed as ‘`120`’ if it hadn’t been
        included in a string.)

    `:no-eval`
    :   This is like `:eval`, except that the form will not be evaluated.
        In these cases, a `:result` element of some kind (see below)
        should be included.

        ```
        :no-eval (file-symlink-p "/tmp/foo")
        :eg-result t
        ```

    `:no-eval*`
    :   Like `:no-eval`, but always inserts ‘`[it depends]`’ as the
        result. For instance:

        ```
        :no-eval* (buffer-string)
        ```

        will result in:

        ```
        (buffer-string)
        → [it depends]
        ```

    `:no-value`
    :   Like `:no-eval`, but is used when the function in question has no
        well-defined return value, and is used for side effect only.

    `:result`
    :   Used to output the result from non-evaluating example forms.

        ```
        :no-eval (setcar list 'c)
        :result c
        ```

    `:eg-result`
    :   Used to output an example result from non-evaluating example forms.
        For instance:

        ```
        :no-eval (looking-at "f[0-9]")
        :eg-result t
        ```

        will result in:

        ```
        (looking-at "f[0-9]")
        eg. → t
        ```

    `:result-string`

    `:eg-result-string`
    :   These two are the same as `:result` and `:eg-result`,
        respectively, but are inserted as is. This is useful when the result
        is unreadable or should be of a particular form:

        ```
        :no-eval (find-file "/tmp/foo")
        :eg-result-string "#<buffer foo>"
        :no-eval (default-file-modes)
        :eg-result-string "#o755"
        ```

    `:no-manual`
    :   Indicates that this function is not documented in the manual.

    `:args`
    :   By default, the function’s actual argument list is shown. If
        `:args` is present, they are used instead.

        ```
        :args (regexp string)
        ```

    Here’s a very short example:

    ```
    (define-short-documentation-group string
      "Creating Strings"
      (substring
       :eval (substring "foobar" 0 3)
       :eval (substring "foobar" 3))
      (concat
       :eval (concat "foo" "bar" "zot")))
    ```

    The first argument is the name of the group to be defined, and then
    follows any number of function descriptions.

A function can belong to any number of documentation groups.

In addition to function descriptions, the list can also have string
elements, which are used to divide a documentation group into
sections.

Function: **shortdoc-add-function** group section elem [¶](#index-shortdoc_002dadd_002dfunction)
:   Lisp packages can add functions to groups with this function. Each
    elem should be a function description, as described above.
    group is the function group, and section is what section
    in the function group to insert the function into.

    If group doesn’t exist, it will be created. If section
    doesn’t exist, it will be added to the end of the function group.

You can also query the examples of use of functions defined in
shortdoc groups.

Function: **shortdoc-function-examples** function [¶](#index-shortdoc_002dfunction_002dexamples)
:   This function returns all shortdoc examples for function. The
    return value is an alist with items of the form
    `(group . examples)`, where group is a
    documentation group where function appears, and examples
    is a string with the examples of functions use as defined in
    group.

    `shortdoc-function-examples` returns `nil` if function
    is not a function or if it doesn’t have any shortdoc examples.

Function: **shortdoc-help-fns-examples-function** function [¶](#index-shortdoc_002dhelp_002dfns_002dexamples_002dfunction)
:   This function queries the registered shortdoc groups and inserts
    examples of use of a given Emacs Lisp function into the current
    buffer. It is suitable for addition to the
    `help-fns-describe-function-functions` hook, in which case
    examples from shortdoc of using a function will be displayed in the
    `*Help*` buffer when the documentation of the function is
    requested.

---

Next: [Backups and Auto-Saving](#Backups-and-Auto_002dSaving), Previous: [Documentation](#Documentation), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 26 Files [¶](#Files-1)

This chapter describes the Emacs Lisp functions and variables to
find, create, view, save, and otherwise work with files and
directories. A few other file-related functions are described in
[Buffers](#Buffers), and those related to backups and auto-saving are
described in [Backups and Auto-Saving](#Backups-and-Auto_002dSaving).

Many of the file functions take one or more arguments that are file
names. A file name is a string. Most of these functions expand file
name arguments using the function `expand-file-name`, so that
`~` is handled correctly, as are relative file names (including
`../` and the empty string). See [Functions that Expand Filenames](#File-Name-Expansion).

In addition, certain *magic* file names are handled specially.
For example, when a remote file name is specified, Emacs accesses the
file over the network via an appropriate protocol. See [Remote Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Remote-Files) in The GNU Emacs Manual. This handling is
done at a very low level, so you may assume that all the functions
described in this chapter accept magic file names as file name
arguments, except where noted. See [Making Certain File Names “Magic”](#Magic-File-Names), for details.

When file I/O functions signal Lisp errors, they usually use the
condition `file-error` (see [Writing Code to Handle Errors](#Handling-Errors)). The error
message is in most cases obtained from the operating system, according
to locale `system-messages-locale`, and decoded using coding system
`locale-coding-system` (see [Locales](#Locales)).

* [Visiting Files](#Visiting-Files)
* [Saving Buffers](#Saving-Buffers)
* [Reading from Files](#Reading-from-Files)
* [Writing to Files](#Writing-to-Files)
* [File Locks](#File-Locks)
* [Information about Files](#Information-about-Files)
* [Changing File Names and Attributes](#Changing-Files)
* [Files and Secondary Storage](#Files-and-Storage)
* [File Names](#File-Names)
* [Contents of Directories](#Contents-of-Directories)
* [Creating, Copying and Deleting Directories](#Create_002fDelete-Dirs)
* [Making Certain File Names “Magic”](#Magic-File-Names)
* [File Format Conversion](#Format-Conversion)

---

Next: [Saving Buffers](#Saving-Buffers), Up: [Files](#Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 26.1 Visiting Files [¶](#Visiting-Files-1)

Visiting a file means reading a file into a buffer. Once this is
done, we say that the buffer is *visiting* that file, and call the
file *the visited file* of the buffer.

A file and a buffer are two different things. A file is information
recorded permanently in the computer (unless you delete it). A
buffer, on the other hand, is information inside of Emacs that will
vanish at the end of the editing session (or when you kill the
buffer). When a buffer is visiting a file, it contains information
copied from the file. The copy in the buffer is what you modify with
editing commands. Changes to the buffer do not change the file; to
make the changes permanent, you must *save* the buffer, which
means copying the altered buffer contents back into the file.

Despite the distinction between files and buffers, people often
refer to a file when they mean a buffer and vice-versa. Indeed, we
say, “I am editing a file”, rather than, “I am editing a buffer
that I will soon save as a file of the same name”. Humans do not
usually need to make the distinction explicit. When dealing with a
computer program, however, it is good to keep the distinction in mind.

* [Functions for Visiting Files](#Visiting-Functions)
* [Subroutines of Visiting](#Subroutines-of-Visiting)

---

Next: [Subroutines of Visiting](#Subroutines-of-Visiting), Up: [Visiting Files](#Visiting-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.1.1 Functions for Visiting Files [¶](#Functions-for-Visiting-Files)

This section describes the functions normally used to visit files.
For historical reasons, these functions have names starting with
‘`find-`’ rather than ‘`visit-`’. See [Buffer File Name](#Buffer-File-Name), for
functions and variables that access the visited file name of a buffer or
that find an existing buffer by its visited file name.

In a Lisp program, if you want to look at the contents of a file but
not alter it, the fastest way is to use `insert-file-contents` in a
temporary buffer. Visiting the file is not necessary and takes longer.
See [Reading from Files](#Reading-from-Files).

Command: **find-file** filename &optional wildcards [¶](#index-find_002dfile)
:   This command selects a buffer visiting the file filename,
    using an existing buffer if there is one, and otherwise creating a
    new buffer and reading the file into it. It also returns that buffer.

    Aside from some technical details, the body of the `find-file`
    function is basically equivalent to:

    ```
    (switch-to-buffer (find-file-noselect filename nil nil wildcards))
    ```

    (See `switch-to-buffer` in [Switching to a Buffer in a Window](#Switching-Buffers).)

    If wildcards is non-`nil`, which is always true in an
    interactive call, then `find-file` expands wildcard characters in
    filename and visits all the matching files.

    When `find-file` is called interactively, it prompts for
    filename in the minibuffer.

Command: **find-file-literally** filename [¶](#index-find_002dfile_002dliterally)
:   This command visits filename, like `find-file` does, but it
    does not perform any format conversions (see [File Format Conversion](#Format-Conversion)),
    character code conversions (see [Coding Systems](#Coding-Systems)), or end-of-line
    conversions (see [End of line conversion](#Coding-System-Basics)).
    The buffer visiting the file is made unibyte, and its major mode is
    Fundamental mode, regardless of the file name. File local variable
    specifications in the file (see [File Local Variables](#File-Local-Variables)) are
    ignored, and automatic decompression and adding a newline at the end
    of the file due to `require-final-newline` (see [require-final-newline](#Saving-Buffers)) are also disabled.

    Note that if Emacs already has a buffer visiting the same file
    non-literally, it will not visit the same file literally, but instead
    just switch to the existing buffer. If you want to be sure of
    accessing a file’s contents literally, you should create a temporary
    buffer and then read the file contents into it using
    `insert-file-contents-literally` (see [Reading from Files](#Reading-from-Files)).

Function: **find-file-noselect** filename &optional nowarn rawfile wildcards [¶](#index-find_002dfile_002dnoselect)
:   This function is the guts of all the file-visiting functions. It
    returns a buffer visiting the file filename. You may make the
    buffer current or display it in a window if you wish, but this
    function does not do so.

    The function returns an existing buffer if there is one; otherwise it
    creates a new buffer and reads the file into it. When
    `find-file-noselect` uses an existing buffer, it first verifies
    that the file has not changed since it was last visited or saved in
    that buffer (unless nowarn is non-`nil`, see below). If the
    file has changed, this function asks the user whether to reread the
    changed file. If the user says ‘`yes`’, any edits previously made in
    the buffer are lost.

    Reading the file involves decoding the file’s contents (see [Coding Systems](#Coding-Systems)), including end-of-line conversion, and format conversion
    (see [File Format Conversion](#Format-Conversion)). If wildcards is non-`nil`,
    then `find-file-noselect` expands wildcard characters in
    filename and visits all the matching files.

    This function displays warning or advisory messages in various peculiar
    cases, unless the optional argument nowarn is non-`nil`. For
    example, if it needs to create a buffer, and there is no file named
    filename, it displays the message ‘`(New file)`’ in the echo
    area, and leaves the buffer empty. The verification of the file’s last
    change is also bypassed if nowarn is non-`nil`.

    The `find-file-noselect` function normally calls
    `after-find-file` after reading the file (see [Subroutines of Visiting](#Subroutines-of-Visiting)). That function sets the buffer major mode, parses local
    variables, warns the user if there exists an auto-save file more recent
    than the file just visited, and finishes by running the functions in
    `find-file-hook`.

    If the optional argument rawfile is non-`nil`, then
    `after-find-file` is not called, and the
    `find-file-not-found-functions` are not run in case of failure.
    What’s more, a non-`nil` rawfile value suppresses coding
    system conversion and format conversion.

    The `find-file-noselect` function usually returns the buffer that
    is visiting the file filename. But, if wildcards are actually
    used and expanded, it returns a list of buffers that are visiting the
    various files.

    ```
    (find-file-noselect "/etc/fstab")
         ⇒ #<buffer fstab>
    ```

Command: **find-file-other-window** filename &optional wildcards [¶](#index-find_002dfile_002dother_002dwindow)
:   This command selects a buffer visiting the file filename, but
    does so in a window other than the selected window. It may use
    another existing window or split a window; see [Switching to a Buffer in a Window](#Switching-Buffers).

    When this command is called interactively, it prompts for
    filename.

Command: **find-file-read-only** filename &optional wildcards [¶](#index-find_002dfile_002dread_002donly)
:   This command selects a buffer visiting the file filename, like
    `find-file`, but it marks the buffer as read-only. See [Read-Only Buffers](#Read-Only-Buffers), for related functions and variables.

    When this command is called interactively, it prompts for
    filename.

User Option: **find-file-wildcards** [¶](#index-find_002dfile_002dwildcards)
:   If this variable is non-`nil`, then the various `find-file`
    commands check for wildcard characters and visit all the files that
    match them (when invoked interactively or when their wildcards
    argument is non-`nil`). If this option is `nil`, then
    the `find-file` commands ignore their wildcards argument
    and never treat wildcard characters specially.

User Option: **find-file-hook** [¶](#index-find_002dfile_002dhook)
:   The value of this variable is a list of functions to be called after a
    file is visited. The file’s local-variables specification (if any) will
    have been processed before the hooks are run. The buffer visiting the
    file is current when the hook functions are run.

    This variable is a normal hook. See [Hooks](#Hooks).

Variable: **find-file-not-found-functions** [¶](#index-find_002dfile_002dnot_002dfound_002dfunctions)
:   The value of this variable is a list of functions to be called when
    `find-file` or `find-file-noselect` is passed a nonexistent
    file name. `find-file-noselect` calls these functions as soon as
    it detects a nonexistent file. It calls them in the order of the list,
    until one of them returns non-`nil`. `buffer-file-name` is
    already set up.

    This is not a normal hook because the values of the functions are
    used, and in many cases only some of the functions are called.

Variable: **find-file-literally** [¶](#index-find_002dfile_002dliterally-1)
:   This buffer-local variable, if set to a non-`nil` value, makes
    `save-buffer` behave as if the buffer were visiting its file
    literally, i.e., without conversions of any kind. The command
    `find-file-literally` sets this variable’s local value, but other
    equivalent functions and commands can do that as well, e.g., to avoid
    automatic addition of a newline at the end of the file. This variable
    is permanent local, so it is unaffected by changes of major modes.

---

Previous: [Functions for Visiting Files](#Visiting-Functions), Up: [Visiting Files](#Visiting-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.1.2 Subroutines of Visiting [¶](#Subroutines-of-Visiting-1)

The `find-file-noselect` function uses two important subroutines
which are sometimes useful in user Lisp code: `create-file-buffer`
and `after-find-file`. This section explains how to use them.

Function: **create-file-buffer** filename [¶](#index-create_002dfile_002dbuffer)
:   This function creates a suitably named buffer for visiting
    filename, and returns it. It uses filename (sans directory)
    as the name if that name is free; otherwise, it appends a string such as
    ‘`<2>`’ to get an unused name. See also [Creating Buffers](#Creating-Buffers).
    Note that the `uniquify` library affects the result of this
    function. See [Uniquify](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Uniquify) in The GNU Emacs Manual.

    **Please note:** `create-file-buffer` does *not*
    associate the new buffer with a file and does not select the buffer.
    It also does not use the default major mode.

    ```
    (create-file-buffer "foo")
         ⇒ #<buffer foo>
    ```

    ```
    (create-file-buffer "foo")
         ⇒ #<buffer foo<2>>
    ```

    ```
    (create-file-buffer "foo")
         ⇒ #<buffer foo<3>>
    ```

    This function is used by `find-file-noselect`.
    It uses `generate-new-buffer` (see [Creating Buffers](#Creating-Buffers)).

Function: **after-find-file** &optional error warn noauto after-find-file-from-revert-buffer nomodes [¶](#index-after_002dfind_002dfile)
:   This function sets the buffer major mode, and parses local variables
    (see [How Emacs Chooses a Major Mode](#Auto-Major-Mode)). It is called by `find-file-noselect`
    and by the default revert function (see [Reverting](#Reverting)).

    If reading the file got an error because the file does not exist, but
    its directory does exist, the caller should pass a non-`nil` value
    for error. In that case, `after-find-file` issues a warning:
    ‘`(New file)`’. For more serious errors, the caller should usually not
    call `after-find-file`.

    If warn is non-`nil`, then this function issues a warning
    if an auto-save file exists and is more recent than the visited file.

    If noauto is non-`nil`, that says not to enable or disable
    Auto-Save mode. The mode remains enabled if it was enabled before.

    If after-find-file-from-revert-buffer is non-`nil`, that
    means this call was from `revert-buffer`. This has no direct
    effect, but some mode functions and hook functions check the value
    of this variable.

    If nomodes is non-`nil`, that means don’t alter the buffer’s
    major mode, don’t process local variables specifications in the file,
    and don’t run `find-file-hook`. This feature is used by
    `revert-buffer` in some cases.

    The last thing `after-find-file` does is call all the functions
    in the list `find-file-hook`.

---

Next: [Reading from Files](#Reading-from-Files), Previous: [Visiting Files](#Visiting-Files), Up: [Files](#Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 26.2 Saving Buffers [¶](#Saving-Buffers-1)

When you edit a file in Emacs, you are actually working on a buffer
that is visiting that file—that is, the contents of the file are
copied into the buffer and the copy is what you edit. Changes to the
buffer do not change the file until you *save* the buffer, which
means copying the contents of the buffer into the file. Buffers which
are not visiting a file can still be “saved”, in a sense, using
functions in the buffer-local `write-contents-functions` hook.

Command: **save-buffer** &optional backup-option [¶](#index-save_002dbuffer)
:   This function saves the contents of the current buffer in its visited
    file if the buffer has been modified since it was last visited or saved.
    Otherwise it does nothing.

    `save-buffer` is responsible for making backup files. Normally,
    backup-option is `nil`, and `save-buffer` makes a backup
    file only if this is the first save since visiting the file. Other
    values for backup-option request the making of backup files in
    other circumstances:

    * With an argument of 4 or 64, reflecting 1 or 3 `C-u`’s, the
      `save-buffer` function marks this version of the file to be
      backed up when the buffer is next saved.
    * With an argument of 16 or 64, reflecting 2 or 3 `C-u`’s, the
      `save-buffer` function unconditionally backs up the previous
      version of the file before saving it.
    * With an argument of 0, unconditionally do *not* make any backup file.

Command: **save-some-buffers** &optional save-silently-p pred [¶](#index-save_002dsome_002dbuffers)
:   This command saves some modified file-visiting buffers. Normally it
    asks the user about each buffer. But if save-silently-p is
    non-`nil`, it saves all the file-visiting buffers without
    querying the user.

    The optional pred argument provides a predicate that controls
    which buffers to ask about (or to save silently if
    save-silently-p is non-`nil`). If pred is
    `nil`, that means to use the value of
    `save-some-buffers-default-predicate` instead of pred. If
    the result is `nil`, it means ask only about file-visiting
    buffers. If it is `t`, that means also offer to save certain
    other non-file buffers—those that have a non-`nil` buffer-local
    value of `buffer-offer-save` (see [Killing Buffers](#Killing-Buffers)). A user
    who says ‘`yes`’ to saving a non-file buffer is asked to specify
    the file name to use. The `save-buffers-kill-emacs` function
    passes the value `t` for pred.

    If the predicate is neither `t` nor `nil`, then it should be
    a function of no arguments. It will be called in each buffer to decide
    whether to offer to save that buffer. If it returns a non-`nil`
    value in a certain buffer, that means do offer to save that buffer.

Command: **write-file** filename &optional confirm [¶](#index-write_002dfile)
:   This function writes the current buffer into file filename, makes
    the buffer visit that file, and marks it not modified. Then it renames
    the buffer based on filename, appending a string like ‘`<2>`’
    if necessary to make a unique buffer name. It does most of this work by
    calling `set-visited-file-name` (see [Buffer File Name](#Buffer-File-Name)) and
    `save-buffer`.

    If confirm is non-`nil`, that means to ask for confirmation
    before overwriting an existing file. Interactively, confirmation is
    required, unless the user supplies a prefix argument.

    If filename is a directory name (see [Directory Names](#Directory-Names)),
    `write-file` uses the name of the visited file, in directory
    filename. If the buffer is not visiting a file, it uses the
    buffer name instead.

Saving a buffer runs several hooks. It also performs format
conversion (see [File Format Conversion](#Format-Conversion)). Note that these hooks,
described below, are only run by `save-buffer`, they are not run
by other primitives and functions that write buffer text to files, and
in particular auto-saving (see [Auto-Saving](#Auto_002dSaving)) doesn’t run these
hooks.

Variable: **write-file-functions** [¶](#index-write_002dfile_002dfunctions)
:   The value of this variable is a list of functions to be called before
    writing out a buffer to its visited file. If one of them returns
    non-`nil`, the file is considered already written and the rest of
    the functions are not called, nor is the usual code for writing the file
    executed.

    If a function in `write-file-functions` returns non-`nil`, it
    is responsible for making a backup file (if that is appropriate).
    To do so, execute the following code:

    ```
    (or buffer-backed-up (backup-buffer))
    ```

    You might wish to save the file modes value returned by
    `backup-buffer` and use that (if non-`nil`) to set the mode
    bits of the file that you write. This is what `save-buffer`
    normally does. See [Making Backup Files](#Making-Backups).

    The hook functions in `write-file-functions` are also responsible
    for encoding the data (if desired): they must choose a suitable coding
    system and end-of-line conversion (see [Coding Systems in Lisp](#Lisp-and-Coding-Systems)),
    perform the encoding (see [Explicit Encoding and Decoding](#Explicit-Encoding)), and set
    `last-coding-system-used` to the coding system that was used
    (see [Encoding and I/O](#Encoding-and-I_002fO)).

    If you set this hook locally in a buffer, it is assumed to be
    associated with the file or the way the contents of the buffer were
    obtained. Thus the variable is marked as a permanent local, so that
    changing the major mode does not alter a buffer-local value. On the
    other hand, calling `set-visited-file-name` will reset it.
    If this is not what you want, you might like to use
    `write-contents-functions` instead.

    Even though this is not a normal hook, you can use `add-hook` and
    `remove-hook` to manipulate the list. See [Hooks](#Hooks).

Variable: **write-contents-functions** [¶](#index-write_002dcontents_002dfunctions)
:   This works just like `write-file-functions`, but it is intended
    for hooks that pertain to the buffer’s contents, not to the particular
    visited file or its location, and can be used to create arbitrary save
    processes for buffers that aren’t visiting files at all. Such hooks
    are usually set up by major modes, as buffer-local bindings for this
    variable. This variable automatically becomes buffer-local whenever
    it is set; switching to a new major mode always resets this variable,
    but calling `set-visited-file-name` does not.

    If any of the functions in this hook returns non-`nil`, the file
    is considered already written and the rest are not called and neither
    are the functions in `write-file-functions`.

    When using this hook to save buffers that are not visiting files (for
    instance, special-mode buffers), keep in mind that, if the function
    fails to save correctly and returns a `nil` value,
    `save-buffer` will go on to prompt the user for a file to save
    the buffer in. If this is undesirable, consider having the function
    fail by raising an error.

User Option: **before-save-hook** [¶](#index-before_002dsave_002dhook)
:   This normal hook runs before a buffer is saved in its visited file,
    regardless of whether that is done normally or by one of the hooks
    described above. For instance, the `copyright.el` program uses
    this hook to make sure the file you are saving has the current year in
    its copyright notice.

User Option: **after-save-hook** [¶](#index-after_002dsave_002dhook)
:   This normal hook runs after a buffer has been saved in its visited file.

User Option: **file-precious-flag** [¶](#index-file_002dprecious_002dflag)
:   If this variable is non-`nil`, then `save-buffer` protects
    against I/O errors while saving by writing the new file to a temporary
    name instead of the name it is supposed to have, and then renaming it to
    the intended name after it is clear there are no errors. This procedure
    prevents problems such as a lack of disk space from resulting in an
    invalid file.

    As a side effect, backups are necessarily made by copying. See [Backup by Renaming or by Copying?](#Rename-or-Copy). Yet, at the same time, saving a precious file always breaks
    all hard links between the file you save and other file names.

    Some modes give this variable a non-`nil` buffer-local value
    in particular buffers.

User Option: **require-final-newline** [¶](#index-require_002dfinal_002dnewline)
:   This variable determines whether files may be written out that do
    *not* end with a newline. If the value of the variable is
    `t`, then `save-buffer` silently adds a newline at the end
    of the buffer whenever it does not already end in one. If the value
    is `visit`, Emacs adds a missing newline just after it visits the
    file. If the value is `visit-save`, Emacs adds a missing newline
    both on visiting and on saving. For any other non-`nil` value,
    `save-buffer` asks the user whether to add a newline each time
    the case arises.

    If the value of the variable is `nil`, then `save-buffer`
    doesn’t add newlines at all. `nil` is the default value, but a few
    major modes set it to `t` in particular buffers.

See also the function `set-visited-file-name` (see [Buffer File Name](#Buffer-File-Name)).

---

Next: [Writing to Files](#Writing-to-Files), Previous: [Saving Buffers](#Saving-Buffers), Up: [Files](#Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 26.3 Reading from Files [¶](#Reading-from-Files-1)

To copy the contents of a file into a buffer, use the function
`insert-file-contents`. (Don’t use the command
`insert-file` in a Lisp program, as that sets the mark.)

Function: **insert-file-contents** filename &optional visit beg end replace [¶](#index-insert_002dfile_002dcontents)
:   This function inserts the contents of file filename into the
    current buffer after point. It returns a list of the absolute file name
    and the length of the data inserted. An error is signaled if
    filename does not name a file that can be read, or names a
    directory.

    This function checks the file contents against the defined file
    formats, and converts the file contents if appropriate and also calls
    the functions in the list `after-insert-file-functions`.
    See [File Format Conversion](#Format-Conversion). Normally, one of the functions in the
    `after-insert-file-functions` list determines the coding system
    (see [Coding Systems](#Coding-Systems)) used for decoding the file’s contents,
    including end-of-line conversion. However, if the file contains null
    bytes, it is by default visited without any code conversions.
    See [inhibit-null-byte-detection](#Lisp-and-Coding-Systems).

    If visit is non-`nil`, this function additionally marks the
    buffer as unmodified and sets up various fields in the buffer so that it
    is visiting the file filename: these include the buffer’s visited
    file name and its last save file modtime. This feature is used by
    `find-file-noselect` and you probably should not use it yourself.

    If beg and end are non-`nil`, they should be numbers
    that are byte offsets specifying the portion of the file to insert.
    In this case, visit must be `nil`. For example,

    ```
    (insert-file-contents filename nil 0 500)
    ```

    inserts the characters coded by the first 500 bytes of a file.

    If beg or end happens to be in the middle of a character’s
    multibyte sequence, Emacs’s character code conversion will insert one
    or more eight-bit characters (a.k.a. “raw bytes”)
    (see [Character Sets](#Character-Sets)) into the buffer. If you want to read part of
    a file this way, we recommend to bind `coding-system-for-read` to
    a suitable value around the call to this function (see [Specifying a Coding System for One Operation](#Specifying-Coding-Systems)), and to write Lisp code which will check for raw
    bytes at the boundaries, read the entire sequence of these bytes, and
    convert them back to valid characters.

    If the argument replace is non-`nil`, it means to replace the
    contents of the buffer (actually, just the accessible portion) with the
    contents of the file. This is better than simply deleting the buffer
    contents and inserting the whole file, because (1) it preserves some
    marker positions and (2) it puts less data in the undo list.

    It is possible to read a special file (such as a FIFO or an I/O
    device) with `insert-file-contents`, as long as replace is
    `nil` or `if-regular`, and visit and beg are
    `nil`. However, you should normally use an end argument
    for these files to avoid inserting (potentially) unlimited data into
    the buffer (for instance, when inserting data from
    `/dev/urandom`).

Function: **insert-file-contents-literally** filename &optional visit beg end replace [¶](#index-insert_002dfile_002dcontents_002dliterally)
:   This function works like `insert-file-contents` except that each
    byte in the file is handled separately, being converted into an
    eight-bit character if needed. It does not run
    `after-insert-file-functions`, and does not do format decoding,
    character code conversion, automatic uncompression, and so on.

If you want to pass a file name to another process so that another
program can read the file, use the function `file-local-copy`; see
[Making Certain File Names “Magic”](#Magic-File-Names).

---

Next: [File Locks](#File-Locks), Previous: [Reading from Files](#Reading-from-Files), Up: [Files](#Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 26.4 Writing to Files [¶](#Writing-to-Files-1)

You can write the contents of a buffer, or part of a buffer, directly
to a file on disk using the `append-to-file` and
`write-region` functions. Don’t use these functions to write to
files that are being visited; that could cause confusion in the
mechanisms for visiting.

Command: **append-to-file** start end filename [¶](#index-append_002dto_002dfile)
:   This function appends the contents of the region delimited by
    start and end in the current buffer to the end of file
    filename. If that file does not exist, it is created. This
    function returns `nil`.

    An error is signaled if you cannot write or create filename.

    When called from Lisp, this function is completely equivalent to:

    ```
    (write-region start end filename t)
    ```

Command: **write-region** start end filename &optional append visit lockname mustbenew [¶](#index-write_002dregion)
:   This function writes the region delimited by start and end
    in the current buffer into the file specified by filename.

    If start is `nil`, then the command writes the entire buffer
    contents (*not* just the accessible portion) to the file and
    ignores end.

    If start is a string, then `write-region` writes or appends
    that string, rather than text from the buffer. end is ignored in
    this case.

    If append is non-`nil`, then the specified text is appended
    to the existing file contents (if any). If append is a
    number, `write-region` seeks to that byte offset from the start
    of the file and writes the data from there.

    If mustbenew is non-`nil`, then `write-region` asks
    for confirmation if filename names an existing file. If
    mustbenew is the symbol `excl`, then `write-region`
    does not ask for confirmation, but instead it signals an error
    `file-already-exists` if the file already exists. Although
    `write-region` normally follows a symbolic link and creates the
    pointed-to file if the symbolic link is dangling, it does not follow
    symbolic links if mustbenew is `excl`.

    The test for an existing file, when mustbenew is `excl`, uses
    a special system feature. At least for files on a local disk, there is
    no chance that some other program could create a file of the same name
    before Emacs does, without Emacs’s noticing.

    If visit is `t`, then Emacs establishes an association
    between the buffer and the file: the buffer is then visiting that file.
    It also sets the last file modification time for the current buffer to
    filename’s modtime, and marks the buffer as not modified. This
    feature is used by `save-buffer`, but you probably should not use
    it yourself.

    If visit is a string, it specifies the file name to visit. This
    way, you can write the data to one file (filename) while recording
    the buffer as visiting another file (visit). The argument
    visit is used in the echo area message and also for file locking;
    visit is stored in `buffer-file-name`. This feature is used
    to implement `file-precious-flag`; don’t use it yourself unless you
    really know what you’re doing.

    The optional argument lockname, if non-`nil`, specifies the
    file name to use for purposes of locking and unlocking, overriding
    filename and visit for that purpose.

    The function `write-region` converts the data which it writes to
    the appropriate file formats specified by `buffer-file-format`
    and also calls the functions in the list
    `write-region-annotate-functions`.
    See [File Format Conversion](#Format-Conversion).

    Normally, `write-region` displays the message ‘`Wrote
    filename`’ in the echo area. This message is inhibited if
    visit is neither `t` nor `nil` nor a string, or if
    Emacs is operating in batch mode (see [Batch Mode](#Batch-Mode)). This
    feature is useful for programs that use files for internal purposes,
    files that the user does not need to know about.

Variable: **write-region-inhibit-fsync** [¶](#index-write_002dregion_002dinhibit_002dfsync)
:   If this variable’s value is `nil`, `write-region` uses the
    `fsync` system call after writing a file. If the value is
    `t`, Emacs does not use `fsync`. The default value is
    `t`. See [Files and Secondary Storage](#Files-and-Storage).

Macro: **with-temp-file** file body… [¶](#index-with_002dtemp_002dfile)
:   The `with-temp-file` macro evaluates the body forms with a
    temporary buffer as the current buffer; then, at the end, it writes the
    buffer contents into file file. It kills the temporary buffer
    when finished, restoring the buffer that was current before the
    `with-temp-file` form. Then it returns the value of the last form
    in body.

    The current buffer is restored even in case of an abnormal exit via
    `throw` or error (see [Nonlocal Exits](#Nonlocal-Exits)).

    Like `with-temp-buffer` (see [Current Buffer](#Definition-of-with_002dtemp_002dbuffer)), the temporary buffer used by this macro does not run
    the hooks `kill-buffer-hook`, `kill-buffer-query-functions`
    (see [Killing Buffers](#Killing-Buffers)), and `buffer-list-update-hook`
    (see [The Buffer List](#Buffer-List)).

---

Next: [Information about Files](#Information-about-Files), Previous: [Writing to Files](#Writing-to-Files), Up: [Files](#Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 26.5 File Locks [¶](#File-Locks-1)

When two users edit the same file at the same time, they are likely
to interfere with each other. Emacs tries to prevent this situation
from arising by recording a *file lock* when a file is being
modified.
Emacs can then detect the first attempt to modify a buffer visiting a
file that is locked by another Emacs job, and ask the user what to do.
The file lock is really a file, a symbolic link with a special name,
stored in the same directory as the file you are editing. The name is
constructed by prepending `.#` to the file name of the buffer.
The target of the symbolic link will be of the form
`user@host.pid:boot`, where user
is replaced with the current username (from `user-login-name`),
host with the name of the host where Emacs is running (from
`system-name`), pid with Emacs’s process id, and boot
with the time since the last reboot. `:boot` is omitted if
the boot time is unavailable. (On file systems that do not support
symbolic links, a regular file is used instead, with contents of the
form `user@host.pid:boot`.)

When you access files using NFS, there may be a small probability that
you and another user will both lock the same file simultaneously.
If this happens, it is possible for the two users to make changes
simultaneously, but Emacs will still warn the user who saves second.
Also, the detection of modification of a buffer visiting a file changed
on disk catches some cases of simultaneous editing; see
[Buffer Modification Time](#Modification-Time).

Function: **file-locked-p** filename [¶](#index-file_002dlocked_002dp)
:   This function returns `nil` if the file filename is not
    locked. It returns `t` if it is locked by this Emacs process, and
    it returns the name of the user who has locked it if it is locked by
    some other job.

    ```
    (file-locked-p "foo")
         ⇒ nil
    ```

Function: **lock-buffer** &optional filename [¶](#index-lock_002dbuffer)
:   This function locks the file filename, if the current buffer is
    modified. The argument filename defaults to the current buffer’s
    visited file. Nothing is done if the current buffer is not visiting a
    file, or is not modified, or if the option `create-lockfiles` is
    `nil`.

Function: **unlock-buffer** [¶](#index-unlock_002dbuffer)
:   This function unlocks the file being visited in the current buffer,
    if the buffer is modified. If the buffer is not modified, then
    the file should not be locked, so this function does nothing. It also
    does nothing if the current buffer is not visiting a file, or is not locked.
    This function handles file system errors by calling `display-warning`
    and otherwise ignores the error.

User Option: **create-lockfiles** [¶](#index-create_002dlockfiles)
:   If this variable is `nil`, Emacs does not lock files.

User Option: **lock-file-name-transforms** [¶](#index-lock_002dfile_002dname_002dtransforms)
:   By default, Emacs creates the lock files in the same directory as the
    files that are being locked. This can be changed by customizing this
    variable. Is has the same syntax as
    `auto-save-file-name-transforms` (see [Auto-Saving](#Auto_002dSaving)). For
    instance, to make Emacs write all the lock files to `/var/tmp/`,
    you could say something like:

    ```
    (setq lock-file-name-transforms
          '(("\\`/.*/\\([^/]+\\)\\'" "/var/tmp/\\1" t)))
    ```

Function: **ask-user-about-lock** file other-user [¶](#index-ask_002duser_002dabout_002dlock)
:   This function is called when the user tries to modify file, but it
    is locked by another user named other-user. The default
    definition of this function asks the user to say what to do. The value
    this function returns determines what Emacs does next:

    * A value of `t` says to grab the lock on the file. Then
      this user may edit the file and other-user loses the lock.
    * A value of `nil` says to ignore the lock and let this
      user edit the file anyway.
    * This function may instead signal a `file-locked` error, in which
      case the change that the user was about to make does not take place.

      The error message for this error looks like this:

      ```
      error→ File is locked: file other-user
      ```

      where `file` is the name of the file and other-user is the
      name of the user who has locked the file.

    If you wish, you can replace the `ask-user-about-lock` function
    with your own version that makes the decision in another way.

User Option: **remote-file-name-inhibit-locks** [¶](#index-remote_002dfile_002dname_002dinhibit_002dlocks)
:   You can prevent the creation of remote lock files by setting the
    variable `remote-file-name-inhibit-locks` to `t`.

Command: **lock-file-mode** [¶](#index-lock_002dfile_002dmode)
:   This command, called interactively, toggles the local value of
    `create-lockfiles` in the current buffer.

---

Next: [Changing File Names and Attributes](#Changing-Files), Previous: [File Locks](#File-Locks), Up: [Files](#Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 26.6 Information about Files [¶](#Information-about-Files-1)

This section describes the functions for retrieving various types of
information about files (or directories or symbolic links), such as
whether a file is readable or writable, and its size. These functions
all take arguments which are file names. Except where noted, these
arguments need to specify existing files, or an error is signaled.

Be careful with file names that end in spaces. On some filesystems
(notably, MS-Windows), trailing whitespace characters in file names
are silently and automatically ignored.

* [Testing Accessibility](#Testing-Accessibility)
* [Distinguishing Kinds of Files](#Kinds-of-Files)
* [Truenames](#Truenames)
* [File Attributes](#File-Attributes)
* [Extended File Attributes](#Extended-Attributes)
* [Locating Files in Standard Places](#Locating-Files)

---

Next: [Distinguishing Kinds of Files](#Kinds-of-Files), Up: [Information about Files](#Information-about-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.6.1 Testing Accessibility [¶](#Testing-Accessibility-1)

These functions test for permission to access a file for reading,
writing, or execution. Unless explicitly stated otherwise, they
follow symbolic links. See [Distinguishing Kinds of Files](#Kinds-of-Files).

On some operating systems, more complex sets of access permissions
can be specified, via mechanisms such as Access Control Lists (ACLs).
See [Extended File Attributes](#Extended-Attributes), for how to query and set those
permissions.

Function: **file-exists-p** filename [¶](#index-file_002dexists_002dp)
:   This function returns `t` if a file named filename appears
    to exist. This does not mean you can necessarily read the file, only
    that you can probably find out its attributes. (On GNU and other POSIX-like
    systems, this is true if the file exists and you have execute
    permission on the containing directories, regardless of the
    permissions of the file itself.)

    If the file does not exist, or if there was trouble determining
    whether the file exists, this function returns `nil`.

    Since a file name that is an empty string is interpreted relative to
    the current buffer’s default directory (see [Absolute and Relative File Names](#Relative-File-Names)),
    calling `file-exists-p` with an argument that is an empty string
    will report about the buffer’s default directory.

    Directories are files, so `file-exists-p` can return `t`
    when given a directory. However, because `file-exists-p` follows
    symbolic links, it returns `t` for a symbolic link name only if
    the target of the link exists; if your Lisp program needs to consider
    *dangling symlinks* whose target doesn’t exist as existing files,
    use `file-attributes` (see [File Attributes](#File-Attributes)) instead of
    `file-exists-p`.

Function: **file-readable-p** filename [¶](#index-file_002dreadable_002dp)
:   This function returns `t` if a file named filename exists
    and you can read it. It returns `nil` otherwise.

Function: **file-executable-p** filename [¶](#index-file_002dexecutable_002dp)
:   This function returns `t` if a file named filename exists
    and you can execute it. It returns `nil` otherwise. On GNU and
    other POSIX-like systems, if the file is a directory, execute
    permission means you can check the existence and attributes of files
    inside the directory, and open those files if their modes permit.

Function: **file-writable-p** filename [¶](#index-file_002dwritable_002dp)
:   This function returns `t` if the file filename can be written
    or created by you, and `nil` otherwise. A file is writable if the
    file exists and you can write it. It is creatable if it does not exist,
    but its parent directory does exist and you can write in that
    directory.

    In the example below, `foo` is not writable because the parent
    directory does not exist, even though the user could create such a
    directory.

    ```
    (file-writable-p "~/no-such-dir/foo")
         ⇒ nil
    ```

Function: **file-accessible-directory-p** dirname [¶](#index-file_002daccessible_002ddirectory_002dp)
:   This function returns `t` if you have permission to open existing
    files in the directory whose name as a file is dirname;
    otherwise (e.g., if there is no such directory), it returns `nil`.
    The value of dirname may be either a directory name (such as
    `/foo/`) or the file name of a file which is a directory
    (such as `/foo`, without the final slash).

    For example, from the following we deduce that any attempt to read a
    file in `/foo/` will give an error:

    ```
    (file-accessible-directory-p "/foo")
         ⇒ nil
    ```

Macro: **with-existing-directory** body… [¶](#index-with_002dexisting_002ddirectory)
:   This macro ensures that `default-directory` is bound to an
    existing directory before executing body. If
    `default-directory` already exists, that’s preferred, and
    otherwise some other directory is used. This macro can be useful, for
    instance, when calling an external command that requires that it’s
    running in a directory that exists. The chosen directory is not
    guaranteed to be writable.

Function: **access-file** filename string [¶](#index-access_002dfile)
:   If you can read filename this function returns `nil`;
    otherwise it signals an error
    using string as the error message text.

    If the user option `remote-file-name-access-timeout` is a
    positive number, the function signals an error when it doesn’t finish
    after that time (in seconds). This applies only to remote files, and
    only when there is no additional time spent while reading passwords or
    alike.

Function: **file-ownership-preserved-p** filename &optional group [¶](#index-file_002downership_002dpreserved_002dp)
:   This function returns `t` if deleting the file filename and
    then creating it anew would keep the file’s owner unchanged. It also
    returns `t` for nonexistent files.

    If the optional argument group is non-`nil`, this function
    also checks that the file’s group would be unchanged.

    This function does not follow symbolic links.

Function: **file-modes** filename &optional flag [¶](#index-file_002dmodes)
:   This function returns the *mode bits* of filename—an
    integer summarizing its read, write, and execution permissions.
    This function follows symbolic links. If the file does not exist, the
    return value is `nil`.

    See [File permissions](https://www.gnu.org/software/coreutils/manual/coreutils.html#File-permissions) in The GNU `Coreutils`
    Manual, for a description of mode bits. For example, if the
    low-order bit is 1, the file is executable by all users; if the
    second-lowest-order bit is 1, the file is writable by all users; etc.
    The highest possible value is 4095 (7777 octal), meaning that everyone
    has read, write, and execute permission, the SUID bit is set
    for both others and group, and the sticky bit is set.

    By default this function follows symbolic links. However, if the
    optional argument flag is the symbol `nofollow`, this
    function does not follow filename if it is a symbolic link;
    this can help prevent inadvertently obtaining the mode bits of a file
    somewhere else, and is more consistent with `file-attributes`
    (see [File Attributes](#File-Attributes)).

    See [Changing File Names and Attributes](#Changing-Files), for the `set-file-modes` function, which
    can be used to set these permissions.

    ```
    (file-modes "~/junk/diffs" 'nofollow)
         ⇒ 492               ; Decimal integer.
    ```

    ```
    (format "%o" 492)
         ⇒ "754"             ; Convert to octal.
    ```

    ```

    ```

    ```
    (set-file-modes "~/junk/diffs" #o666 'nofollow)
         ⇒ nil
    ```

    ```

    ```

    ```
    $ ls -l diffs
    -rw-rw-rw- 1 lewis lewis 3063 Oct 30 16:00 diffs
    ```

    **MS-DOS note:** On MS-DOS, there is no such thing as an
    executable file mode bit. So `file-modes` considers a file
    executable if its name ends in one of the standard executable
    extensions, such as `.com`, `.bat`, `.exe`, and some
    others. Files that begin with the POSIX-standard ‘`#!`’ signature,
    such as shell and Perl scripts, are also considered executable.
    Directories are also reported as executable, for compatibility with
    POSIX. These conventions are also followed by `file-attributes`
    (see [File Attributes](#File-Attributes)).

---

Next: [Truenames](#Truenames), Previous: [Testing Accessibility](#Testing-Accessibility), Up: [Information about Files](#Information-about-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.6.2 Distinguishing Kinds of Files [¶](#Distinguishing-Kinds-of-Files)

This section describes how to distinguish various kinds of files, such
as directories, symbolic links, and ordinary files.

Symbolic links are ordinarily followed wherever they appear. For
example, to interpret the file name `a/b/c`, any of `a`,
`a/b`, and `a/b/c` can be symbolic links that are followed,
possibly recursively if the link targets are themselves symbolic
links. However, a few functions do not follow symbolic links at the
end of a file name (`a/b/c` in this example). Such a function
is said to *not follow symbolic links*.

Function: **file-symlink-p** filename [¶](#index-file_002dsymlink_002dp)
:   If the file filename is a symbolic link, this function does not
    follow it and instead returns its link target
    as a string. (The link target string is not necessarily the full
    absolute file name of the target; determining the full file name that
    the link points to is nontrivial, see below.)

    If the file filename is not a symbolic link, or does not exist,
    or if there is trouble determining whether it is a symbolic link,
    `file-symlink-p` returns `nil`.

    Here are a few examples of using this function:

    ```
    (file-symlink-p "not-a-symlink")
         ⇒ nil
    ```

    ```
    (file-symlink-p "sym-link")
         ⇒ "not-a-symlink"
    ```

    ```
    (file-symlink-p "sym-link2")
         ⇒ "sym-link"
    ```

    ```
    (file-symlink-p "/bin")
         ⇒ "/pub/bin"
    ```

    Note that in the third example, the function returned `sym-link`,
    but did not proceed to resolve it, although that file is itself a
    symbolic link. That is because this function does not follow symbolic
    links—the process of following the symbolic links does not apply to
    the last component of the file name.

    The string that this function returns is what is recorded in the
    symbolic link; it may or may not include any leading directories.
    This function does *not* expand the link target to produce a
    fully-qualified file name, and in particular does not use the leading
    directories, if any, of the filename argument if the link target
    is not an absolute file name. Here’s an example:

    ```
    (file-symlink-p "/foo/bar/baz")
         ⇒ "some-file"
    ```

    Here, although `/foo/bar/baz` was given as a fully-qualified file
    name, the result is not, and in fact does not have any leading
    directories at all. And since `some-file` might itself be a
    symbolic link, you cannot simply prepend leading directories to it,
    nor even naively use `expand-file-name` (see [Functions that Expand Filenames](#File-Name-Expansion)) to produce its absolute file name.

    For this reason, this function is seldom useful if you need to
    determine more than just the fact that a file is or isn’t a symbolic
    link. If you actually need the file name of the link target, use
    `file-chase-links` or `file-truename`, described in
    [Truenames](#Truenames).

Function: **file-directory-p** filename [¶](#index-file_002ddirectory_002dp)
:   This function returns `t` if filename is the name of an
    existing directory. It returns `nil` if filename does
    not name a directory, or if there is trouble determining whether
    it is a directory.
    This function follows symbolic links.

    ```
    (file-directory-p "~rms")
         ⇒ t
    ```

    ```
    (file-directory-p "~rms/lewis/files.texi")
         ⇒ nil
    ```

    ```
    (file-directory-p "~rms/lewis/no-such-file")
         ⇒ nil
    ```

    ```
    (file-directory-p "$HOME")
         ⇒ nil
    ```

    ```
    (file-directory-p
     (substitute-in-file-name "$HOME"))
         ⇒ t
    ```

Function: **file-regular-p** filename [¶](#index-file_002dregular_002dp)
:   This function returns `t` if the file filename exists and is
    a regular file (not a directory, named pipe, terminal, or
    other I/O device).
    It returns `nil` if filename does not exist or is not a regular
    file, or if there is trouble determining whether it is a regular file.
    This function follows symbolic links.

---

Next: [File Attributes](#File-Attributes), Previous: [Distinguishing Kinds of Files](#Kinds-of-Files), Up: [Information about Files](#Information-about-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.6.3 Truenames [¶](#Truenames-1)

The *truename* of a file is the name that you get by following
symbolic links at all levels until none remain, then simplifying away
‘`.`’ and ‘`..`’ appearing as name components. This results
in a sort of canonical name for the file. A file does not always have a
unique truename; the number of distinct truenames a file has is equal to
the number of hard links to the file. However, truenames are useful
because they eliminate symbolic links as a cause of name variation.

Function: **file-truename** filename [¶](#index-file_002dtruename)
:   This function returns the truename of the file filename. If the
    argument is not an absolute file name, this function first expands it
    against `default-directory`.

    This function does not expand environment variables. Only
    `substitute-in-file-name` does that. See [Definition of substitute-in-file-name](#Definition-of-substitute_002din_002dfile_002dname).

    If you may need to follow symbolic links preceding ‘`..`’
    appearing as a name component, call `file-truename` without prior
    direct or indirect calls to `expand-file-name`. Otherwise, the
    file name component immediately preceding ‘`..`’ will be
    simplified away before `file-truename` is called. To
    eliminate the need for a call to `expand-file-name`,
    `file-truename` handles ‘`~`’ in the same way that
    `expand-file-name` does.

    If the target of a symbolic links has remote file name syntax,
    `file-truename` returns it quoted. See [Functions that Expand Filenames](#File-Name-Expansion).

Function: **file-chase-links** filename &optional limit [¶](#index-file_002dchase_002dlinks)
:   This function follows symbolic links, starting with filename,
    until it finds a file name which is not the name of a symbolic link.
    Then it returns that file name. This function does *not* follow
    symbolic links at the level of parent directories.

    If you specify a number for limit, then after chasing through
    that many links, the function just returns what it has even if that is
    still a symbolic link.

To illustrate the difference between `file-chase-links` and
`file-truename`, suppose that `/usr/foo` is a symbolic link to
the directory `/home/foo`, and `/home/foo/hello` is an
ordinary file (or at least, not a symbolic link) or nonexistent. Then
we would have:

```
(file-chase-links "/usr/foo/hello")
     ;; This does not follow the links in the parent directories.
     ⇒ "/usr/foo/hello"
(file-truename "/usr/foo/hello")
     ;; Assuming that /home is not a symbolic link.
     ⇒ "/home/foo/hello"
```

Function: **file-equal-p** file1 file2 [¶](#index-file_002dequal_002dp)
:   This function returns `t` if the files file1 and
    file2 name the same file. This is similar to comparing their
    truenames, except that remote file names are also handled in an
    appropriate manner. If file1 or file2 does not exist, the
    return value is unspecified.

Function: **file-name-case-insensitive-p** filename [¶](#index-file_002dname_002dcase_002dinsensitive_002dp)
:   Sometimes file names or their parts need to be compared as strings, in
    which case it’s important to know whether the underlying filesystem is
    case-insensitive. This function returns `t` if file
    filename is on a case-insensitive filesystem. It always returns
    `t` on MS-DOS and MS-Windows. On Cygwin and macOS,
    filesystems may or may not be case-insensitive, and the function tries
    to determine case-sensitivity by a runtime test. If the test is
    inconclusive, the function returns `t` on Cygwin and `nil`
    on macOS.

    Currently this function always returns `nil` on platforms other
    than MS-DOS, MS-Windows, Cygwin, and macOS. It does not detect
    case-insensitivity of mounted filesystems, such as Samba shares or
    NFS-mounted Windows volumes. On remote hosts, it assumes `t` for
    the ‘`smb`’ method. For all other connection methods, runtime
    tests are performed.

Function: **vc-responsible-backend** file [¶](#index-vc_002dresponsible_002dbackend)
:   This function determines the responsible VC backend of the given
    file. For example, if `emacs.c` is a file tracked by Git,
    `(vc-responsible-backend "emacs.c")` returns ‘`Git`’.
    Note that if file is a symbolic link,
    `vc-responsible-backend` will not resolve it—the backend of the
    symbolic link file itself is reported. To get the backend VC of the
    file to which file refers, wrap file with a symbolic link
    resolving function such as `file-chase-links`:

    ```
    (vc-responsible-backend (file-chase-links "emacs.c"))
    ```

---

Next: [Extended File Attributes](#Extended-Attributes), Previous: [Truenames](#Truenames), Up: [Information about Files](#Information-about-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.6.4 File Attributes [¶](#File-Attributes-1)

This section describes the functions for getting detailed
information about a file, including the owner and group numbers, the
number of names, the inode number, the size, and the times of access
and modification.

Function: **file-newer-than-file-p** filename1 filename2 [¶](#index-file_002dnewer_002dthan_002dfile_002dp)
:   This function returns `t` if the file filename1 is
    newer than file filename2. If filename1 does not
    exist, it returns `nil`. If filename1 does exist, but
    filename2 does not, it returns `t`. Otherwise, it compares
    the times of last modification of the files.

    In the following example, assume that the file `aug-19` was written
    on the 19th, `aug-20` was written on the 20th, and the file
    `no-file` doesn’t exist at all.

    ```
    (file-newer-than-file-p "aug-19" "aug-20")
         ⇒ nil
    ```

    ```
    (file-newer-than-file-p "aug-20" "aug-19")
         ⇒ t
    ```

    ```
    (file-newer-than-file-p "aug-19" "no-file")
         ⇒ t
    ```

    ```
    (file-newer-than-file-p "no-file" "aug-19")
         ⇒ nil
    ```

Function: **file-has-changed-p** filename tag [¶](#index-file_002dhas_002dchanged_002dp)
:   This function returns non-`nil` if the time stamp of
    filename has changed since the last call. When called for the
    first time for some filename, it records the last modification
    time and size of the file, and returns non-`nil` when
    filename exists. Thereafter, when called for the same
    filename, it compares the current time stamp and size with the
    recorded ones, and returns non-`nil` only if either the time
    stamp or the size (or both) are different. This is useful when a Lisp
    program wants to re-read a file whenever it changes. With an optional
    argument tag, which must be a symbol, the size and modification
    time comparisons are limited to calls with the same tag.

Function: **file-attributes** filename &optional id-format [¶](#index-file_002dattributes)
:   This function returns a list of attributes of file filename. If
    the specified file does not exist, it returns `nil`.
    This function does not follow symbolic links.
    The optional parameter id-format specifies the preferred format
    of attributes UID and GID (see below)—the
    valid values are `'string` and `'integer`. The latter is
    the default, but we plan to change that, so you should specify a
    non-`nil` value for id-format if you use the returned
    UID or GID.

    On GNU platforms when operating on a local file, this function is
    atomic: if the filesystem is simultaneously being changed by some
    other process, this function returns the file’s attributes either
    before or after the change. Otherwise this function is not atomic,
    and might return `nil` if it detects the race condition, or might
    return a hodgepodge of the previous and current file attributes.

    Accessor functions are provided to access the elements in this list.
    The accessors are mentioned along with the descriptions of the
    elements below.

    The elements of the list, in order, are:

    0. `t` for a directory, a string for a symbolic link (the name
       linked to), or `nil` for a text file
       (`file-attribute-type`).
    1. The number of names the file has (`file-attribute-link-number`).
       Alternate names, also known as hard links, can be created by using the
       `add-name-to-file` function (see [Changing File Names and Attributes](#Changing-Files)).
    2. The file’s UID, normally as a string
       (`file-attribute-user-id`). However, if it does not correspond
       to a named user, the value is an integer.
    3. The file’s GID, likewise (`file-attribute-group-id`).
    4. The time of last access as a Lisp timestamp
       (`file-attribute-access-time`). The timestamp is in the
       style of `current-time` (see [Time of Day](#Time-of-Day)) and is truncated
       to that of the filesystem’s timestamp resolution; for example, on some
       FAT-based filesystems, only the date of last access is recorded, so
       this time will always hold the midnight of the day of the last access.
    5. The time of last modification as a Lisp timestamp
       (`file-attribute-modification-time`). This is the last time when
       the file’s contents were modified.
    6. The time of last status change as a Lisp timestamp
       (`file-attribute-status-change-time`). This is the time of the
       last change to the file’s access mode bits, its owner and group, and
       other information recorded in the filesystem for the file, beyond the
       file’s contents.
    7. The size of the file in bytes (`file-attribute-size`).
    8. The file’s modes, as a string of ten letters or dashes, as in
       ‘`ls -l`’ (`file-attribute-modes`).
    9. An unspecified value, present for backward compatibility.
    10. The file’s inode number (`file-attribute-inode-number`),
        a nonnegative integer.
    11. The filesystem’s identifier of the device that the file is on
        (`file-attribute-device-number`), an integer or a cons cell of
        two integers. The latter is sometimes used by remote files, in order
        to distinguish remote filesystems from local ones.

    The file’s inode and device together give enough information
    to distinguish any two files on the system—no two files can have the
    same values for both of these attributes. This tuple that uniquely
    identifies the file is returned by `file-attribute-file-identifier`.

    For example, here are the file attributes for `files.texi`:

    ```
    (file-attributes "files.texi" 'string)
         ⇒  (nil 1 "lh" "users"
              (20614 64019 50040 152000)
              (20000 23 0 0)
              (20614 64555 902289 872000)
              122295 "-rw-rw-rw-"
              t 6473924464520138
              1014478468)
    ```

    and here is how the result is interpreted:

    `nil`
    :   is neither a directory nor a symbolic link.

    `1`
    :   has only one name (the name `files.texi` in the current default
        directory).

    `"lh"`
    :   is owned by the user with name ‘`lh`’.

    `"users"`
    :   is in the group with name ‘`users`’.

    `(20614 64019 50040 152000)`
    :   was last accessed on October 23, 2012, at 20:12:03.050040152 UTC.
        (This timestamp is `(1351023123050040152 . 1000000000)`
        if `current-time-list` is `nil`.)

    `(20000 23 0 0)`
    :   was last modified on July 15, 2001, at 08:53:43.000000000 UTC.
        (This timestamp is `(1310720023000000000 . 1000000000)`
        if `current-time-list` is `nil`.)

    `(20614 64555 902289 872000)`
    :   last had its status changed on October 23, 2012, at 20:20:59.902289872 UTC.
        (This timestamp is `(1351023659902289872 . 1000000000)`
        if `current-time-list` is `nil`.)

    `122295`
    :   is 122295 bytes long. (It may not contain 122295 characters, though,
        if some of the bytes belong to multibyte sequences, and also if the
        end-of-line format is CR-LF.)

    `"-rw-rw-rw-"`
    :   has a mode of read and write access for the owner, group, and world.

    `t`
    :   is merely a placeholder; it carries no information.

    `6473924464520138`
    :   has an inode number of 6473924464520138.

    `1014478468`
    :   is on the file-system device whose number is 1014478468.

Function: **file-nlinks** filename [¶](#index-file_002dnlinks)
:   This function returns the number of names (i.e., hard links) that
    file filename has. If the file does not exist, this function
    returns `nil`. Note that symbolic links have no effect on this
    function, because they are not considered to be names of the files
    they link to. This function does not follow symbolic links.

    ```
    $ ls -l foo*
    -rw-rw-rw- 2 rms rms 4 Aug 19 01:27 foo
    -rw-rw-rw- 2 rms rms 4 Aug 19 01:27 foo1
    ```

    ```

    ```

    ```
    (file-nlinks "foo")
         ⇒ 2
    ```

    ```
    (file-nlinks "doesnt-exist")
         ⇒ nil
    ```

---

Next: [Locating Files in Standard Places](#Locating-Files), Previous: [File Attributes](#File-Attributes), Up: [Information about Files](#Information-about-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.6.5 Extended File Attributes [¶](#Extended-File-Attributes)

On some operating systems, each file can be associated with arbitrary
*extended file attributes*. At present, Emacs supports querying
and setting two specific sets of extended file attributes: Access
Control Lists (ACLs) and SELinux contexts. These extended file
attributes are used, on some systems, to impose more sophisticated
file access controls than the basic Unix-style permissions
discussed in the previous sections.

A detailed explanation of ACLs and SELinux is beyond the scope of
this manual. For our purposes, each file can be associated with an
*ACL*, which specifies its properties under an ACL-based file
control system, and/or an *SELinux context*, which specifies its
properties under the SELinux system.

Function: **file-acl** filename [¶](#index-file_002dacl)
:   This function returns the ACL for the file filename. The exact
    Lisp representation of the ACL is unspecified (and may change in
    future Emacs versions), but it is the same as what `set-file-acl`
    takes for its acl argument (see [Changing File Names and Attributes](#Changing-Files)).

    The underlying ACL implementation is platform-specific; on GNU/Linux
    and BSD, Emacs uses the POSIX ACL interface, while on MS-Windows Emacs
    emulates the POSIX ACL interface with native file security APIs.

    If ACLs are not supported or the file does not exist,
    then the return value is `nil`.

Function: **file-selinux-context** filename [¶](#index-file_002dselinux_002dcontext)
:   This function returns the SELinux context of the file filename,
    as a list of the form `(user role type
    range)`. The list elements are the context’s user, role, type,
    and range respectively, as Lisp strings; see the SELinux documentation
    for details about what these actually mean. The return value has the
    same form as what `set-file-selinux-context` takes for its
    context argument (see [Changing File Names and Attributes](#Changing-Files)).

    If SELinux is not supported or the file does not exist,
    then the return value is `(nil nil nil nil)`.

Function: **file-extended-attributes** filename [¶](#index-file_002dextended_002dattributes)
:   This function returns an alist of the Emacs-recognized extended
    attributes of file filename. Currently, it serves as a
    convenient way to retrieve both the ACL and SELinux context; you can
    then call the function `set-file-extended-attributes`, with the
    returned alist as its second argument, to apply the same file access
    attributes to another file (see [Changing File Names and Attributes](#Changing-Files)).

    One of the elements is `(acl . acl)`, where acl has
    the same form returned by `file-acl`.

    Another element is `(selinux-context . context)`, where
    context is the SELinux context, in the same form returned by
    `file-selinux-context`.

---

Previous: [Extended File Attributes](#Extended-Attributes), Up: [Information about Files](#Information-about-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.6.6 Locating Files in Standard Places [¶](#Locating-Files-in-Standard-Places)

This section explains how to search for a file in a list of
directories (a *path*), or for an executable file in the standard
list of executable file directories.

To search for a user-specific configuration file, See [Standard File Names](#Standard-File-Names), for the `locate-user-emacs-file` function.

Function: **locate-file** filename path &optional suffixes predicate [¶](#index-locate_002dfile)
:   This function searches for a file whose name is filename in a
    list of directories given by path, trying the suffixes in
    suffixes. If it finds such a file, it returns the file’s
    absolute file name (see [Absolute and Relative File Names](#Relative-File-Names)); otherwise it returns
    `nil`.

    The optional argument suffixes gives the list of file-name
    suffixes to append to filename when searching.
    `locate-file` tries each possible directory with each of these
    suffixes. If suffixes is `nil`, or `("")`, then there
    are no suffixes, and filename is used only as-is. Typical
    values of suffixes are `exec-suffixes` (see [Functions that Create Subprocesses](#Subprocess-Creation)), `load-suffixes`, `load-file-rep-suffixes` and
    the return value of the function `get-load-suffixes` (see [Load Suffixes](#Load-Suffixes)).

    Typical values for path are `exec-path` (see [Functions that Create Subprocesses](#Subprocess-Creation)) when looking for executable programs, or `load-path`
    (see [Library Search](#Library-Search)) when looking for Lisp files. If
    filename is absolute, path has no effect, but the suffixes
    in suffixes are still tried.

    The optional argument predicate, if non-`nil`, specifies a
    predicate function for testing whether a candidate file is suitable.
    The predicate is passed the candidate file name as its single
    argument. If predicate is `nil` or omitted,
    `locate-file` uses `file-readable-p` as the predicate.
    See [Distinguishing Kinds of Files](#Kinds-of-Files), for other useful predicates, e.g.,
    `file-executable-p` and `file-directory-p`.

    This function will normally skip directories, so if you want it to
    find directories, make sure the predicate function returns
    `dir-ok` for them. For example:

    ```
    (locate-file "html" '("/var/www" "/srv") nil
                 (lambda (f) (if (file-directory-p f) 'dir-ok)))
    ```

    For compatibility, predicate can also be one of the symbols
    `executable`, `readable`, `writable`, `exists`, or
    a list of one or more of these symbols.

Function: **executable-find** program &optional remote [¶](#index-executable_002dfind)
:   This function searches for the executable file of the named
    program and returns the absolute file name of the executable,
    including its file-name extensions, if any. It returns `nil` if
    the file is not found. The function searches in all the directories
    in `exec-path`, and tries all the file-name extensions in
    `exec-suffixes` (see [Functions that Create Subprocesses](#Subprocess-Creation)).

    If remote is non-`nil`, and `default-directory` is a
    remote directory, program is searched on the respective remote host.

---

Next: [Files and Secondary Storage](#Files-and-Storage), Previous: [Information about Files](#Information-about-Files), Up: [Files](#Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
