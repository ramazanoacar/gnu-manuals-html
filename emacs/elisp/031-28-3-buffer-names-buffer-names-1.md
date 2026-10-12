### 28.3 Buffer Names [¶](#Buffer-Names-1)

Each buffer has a unique name, which is a string. Many of the
functions that work on buffers accept either a buffer or a buffer name
as an argument. Any argument called buffer-or-name is of this
sort, and an error is signaled if it is neither a string nor a buffer.
Any argument called buffer must be an actual buffer
object, not a name.

Buffers that are ephemeral and generally uninteresting to the user
have names starting with a space, so that the `list-buffers` and
`buffer-menu` commands don’t mention them (but if such a buffer
visits a file, it **is** mentioned). A name starting with
space also initially disables recording undo information; see
[Undo](#Undo).

Function: **buffer-name** &optional buffer [¶](#index-buffer_002dname)
:   This function returns the name of buffer as a string.
    buffer defaults to the current buffer.

    If `buffer-name` returns `nil`, it means that buffer
    has been killed. See [Killing Buffers](#Killing-Buffers).

    ```
    (buffer-name)
         ⇒ "buffers.texi"
    ```

    ```

    ```

    ```
    (setq foo (get-buffer "temp"))
         ⇒ #<buffer temp>
    ```

    ```
    (kill-buffer foo)
         ⇒ nil
    ```

    ```
    (buffer-name foo)
         ⇒ nil
    ```

    ```
    foo
         ⇒ #<killed buffer>
    ```

Command: **rename-buffer** newname &optional unique [¶](#index-rename_002dbuffer)
:   This function renames the current buffer to newname. An error
    is signaled if newname is not a string.

    Ordinarily, `rename-buffer` signals an error if newname is
    already in use. However, if unique is non-`nil`, it modifies
    newname to make a name that is not in use. Interactively, you can
    make unique non-`nil` with a numeric prefix argument.
    (This is how the command `rename-uniquely` is implemented.)

    This function returns the name actually given to the buffer.

Function: **get-buffer** buffer-or-name [¶](#index-get_002dbuffer)
:   This function returns the buffer specified by buffer-or-name.
    If buffer-or-name is a string and there is no buffer with that
    name, the value is `nil`. If buffer-or-name is a buffer, it
    is returned as given; that is not very useful, so the argument is usually
    a name. For example:

    ```
    (setq b (get-buffer "lewis"))
         ⇒ #<buffer lewis>
    ```

    ```
    (get-buffer b)
         ⇒ #<buffer lewis>
    ```

    ```
    (get-buffer "Frazzle-nots")
         ⇒ nil
    ```

    See also the function `get-buffer-create` in [Creating Buffers](#Creating-Buffers).

Function: **generate-new-buffer-name** starting-name &optional ignore [¶](#index-generate_002dnew_002dbuffer_002dname)
:   This function returns a name that would be unique for a new buffer—but
    does not create the buffer. It starts with starting-name, and
    produces a name not currently in use for any buffer by appending a
    number inside of ‘`<…>`’. It starts at 2 and keeps
    incrementing the number until it is not the name of an existing buffer.

    If the optional second argument ignore is non-`nil`, it
    should be a string, a potential buffer name. It means to consider
    that potential buffer acceptable, if it is tried, even if it is the
    name of an existing buffer (which would normally be rejected). Thus,
    if buffers named ‘`foo`’, ‘`foo<2>`’, ‘`foo<3>`’ and
    ‘`foo<4>`’ exist,

    ```
    (generate-new-buffer-name "foo")
         ⇒ "foo<5>"
    (generate-new-buffer-name "foo" "foo<3>")
         ⇒ "foo<3>"
    (generate-new-buffer-name "foo" "foo<6>")
         ⇒ "foo<5>"
    ```

    See the related function `generate-new-buffer` in [Creating Buffers](#Creating-Buffers).

Function: **buffer-last-name** &optional buffer [¶](#index-buffer_002dlast_002dname)
:   This function returns the previous name of buffer, before it was
    killed or before the last time it was renamed. If nil or omitted,
    buffer defaults to the current buffer.

---

Next: [Buffer Modification](#Buffer-Modification), Previous: [Buffer Names](#Buffer-Names), Up: [Buffers](#Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 28.4 Buffer File Name [¶](#Buffer-File-Name-1)

The *buffer file name* is the name of the file that is visited in
that buffer. When a buffer is not visiting a file, its buffer file name
is `nil`. Most of the time, the buffer name is the same as the
nondirectory part of the buffer file name, but the buffer file name and
the buffer name are distinct and can be set independently.
See [Visiting Files](#Visiting-Files).

Function: **buffer-file-name** &optional buffer [¶](#index-buffer_002dfile_002dname)
:   This function returns the fully-expanded absolute file name of the file that
    buffer is visiting. If buffer is not visiting any file,
    `buffer-file-name` returns `nil`. If buffer is not
    supplied, it defaults to the current buffer.

    ```
    (buffer-file-name (other-buffer))
         ⇒ "/usr/user/lewis/manual/files.texi"
    ```

Variable: **buffer-file-name** [¶](#index-buffer_002dfile_002dname-1)
:   This buffer-local variable contains the absolute name of the file being visited
    in the current buffer, or `nil` if it is not visiting a file. It
    is a permanent local variable, unaffected by
    `kill-all-local-variables`. The value is identical to the one
    which the `buffer-file-name` function returns.

    ```
    buffer-file-name
         ⇒ "/usr/user/lewis/manual/buffers.texi"
    ```

    It is risky to change this variable’s value without doing various other
    things. Normally it is better to use `set-visited-file-name` (see
    below); some of the things done there, such as changing the buffer name,
    are not strictly necessary, but others are essential to avoid confusing
    Emacs.

Variable: **buffer-file-truename** [¶](#index-buffer_002dfile_002dtruename)
:   This buffer-local variable holds the abbreviated truename of the file
    visited in the current buffer, or `nil` if no file is visited.
    It is a permanent local, unaffected by
    `kill-all-local-variables`. See [Truenames](#Truenames), and
    [abbreviate-file-name](#abbreviate_002dfile_002dname).

Variable: **buffer-file-number** [¶](#index-buffer_002dfile_002dnumber)
:   This buffer-local variable holds the inode number and device
    identifier of the file visited in the current buffer, or `nil` if no
    file or a nonexistent file is visited. It is a permanent local,
    unaffected by `kill-all-local-variables`.

    The value is normally a list of the form `(inodenum
    device)`. This tuple uniquely identifies the file among
    all files accessible on the system. See the function
    `file-attributes`, in [File Attributes](#File-Attributes), for more information
    about them.

    If `buffer-file-name` is the name of a symbolic link, then both
    inodenum and device refer to the recursive target of the link.

Function: **get-file-buffer** filename [¶](#index-get_002dfile_002dbuffer)
:   This function returns the buffer visiting file filename. If
    there is no such buffer, it returns `nil`. The argument
    filename, which must be a string, is expanded (see [Functions that Expand Filenames](#File-Name-Expansion)), then compared against the visited file names of all live
    buffers. Note that the buffer’s `buffer-file-name` must match
    the expansion of filename exactly. This function will not
    recognize other names for the same file.

    ```
    (get-file-buffer "buffers.texi")
        ⇒ #<buffer buffers.texi>
    ```

    In unusual circumstances, there can be more than one buffer visiting
    the same file name. In such cases, this function returns the first
    such buffer in the buffer list.

Function: **find-buffer-visiting** filename &optional predicate [¶](#index-find_002dbuffer_002dvisiting)
:   This is like `get-file-buffer`, except that it can return any
    buffer visiting the file *possibly under a different name*. That
    is, the buffer’s `buffer-file-name` does not need to match the
    expansion of filename exactly, it only needs to refer to the
    same file. If predicate is non-`nil`, it should be a
    function of one argument, a buffer visiting filename. The
    buffer is only considered a suitable return value if predicate
    returns non-`nil`. If it can not find a suitable buffer to
    return, `find-buffer-visiting` returns `nil`.

Command: **set-visited-file-name** filename &optional no-query along-with-file [¶](#index-set_002dvisited_002dfile_002dname)
:   If filename is a non-empty string, this function changes the
    name of the file visited in the current buffer to filename. (If the
    buffer had no visited file, this gives it one.) The *next time*
    the buffer is saved it will go in the newly-specified file.

    This command marks the buffer as modified, since it does not (as far
    as Emacs knows) match the contents of filename, even if it
    matched the former visited file. It also renames the buffer to
    correspond to the new file name, unless the new name is already in
    use.

    If filename is `nil` or the empty string, that stands for
    “no visited file”. In this case, `set-visited-file-name` marks
    the buffer as having no visited file, without changing the buffer’s
    modified flag.

    Normally, this function asks the user for confirmation if there
    already is a buffer visiting filename. If no-query is
    non-`nil`, that prevents asking this question. If there already
    is a buffer visiting filename, and the user confirms or
    no-query is non-`nil`, this function makes the new
    buffer name unique by appending a number inside of ‘`<…>`’ to
    filename.

    If along-with-file is non-`nil`, that means to assume that
    the former visited file has been renamed to filename. In this
    case, the command does not change the buffer’s modified flag, nor the
    buffer’s recorded last file modification time as reported by
    `visited-file-modtime` (see [Buffer Modification Time](#Modification-Time)). If
    along-with-file is `nil`, this function clears the recorded
    last file modification time, after which `visited-file-modtime`
    returns zero.

    When the function `set-visited-file-name` is called
    interactively, it prompts for filename in the minibuffer.

Variable: **list-buffers-directory** [¶](#index-list_002dbuffers_002ddirectory)
:   This buffer-local variable specifies a string to display in a buffer
    listing where the visited file name would go, for buffers that don’t
    have a visited file name. Dired buffers use this variable.

---

Next: [Buffer Modification Time](#Modification-Time), Previous: [Buffer File Name](#Buffer-File-Name), Up: [Buffers](#Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 28.5 Buffer Modification [¶](#Buffer-Modification-1)

Emacs keeps a flag called the *modified flag* for each buffer, to
record whether you have changed the text of the buffer. This flag is
set to `t` whenever you alter the contents of the buffer, and
cleared to `nil` when you save it. Thus, the flag shows whether
there are unsaved changes. The flag value is normally shown in the mode
line (see [Variables Used in the Mode Line](#Mode-Line-Variables)), and controls saving (see [Saving Buffers](#Saving-Buffers)) and auto-saving (see [Auto-Saving](#Auto_002dSaving)).

Some Lisp programs set the flag explicitly. For example, the function
`set-visited-file-name` sets the flag to `t`, because the text
does not match the newly-visited file, even if it is unchanged from the
file formerly visited.

The functions that modify the contents of buffers are described in
[Text](#Text).

Function: **buffer-modified-p** &optional buffer [¶](#index-buffer_002dmodified_002dp)
:   This function returns non-`nil` if buffer has
    been modified since it was last read in from a file or saved, or
    `nil` otherwise. If buffer has been auto-saved since the
    time it was last modified, this function returns the symbol
    `autosaved`. If buffer is `nil` or omitted, it
    defaults to the current buffer.

Function: **set-buffer-modified-p** flag [¶](#index-set_002dbuffer_002dmodified_002dp)
:   This function marks the current buffer as modified if flag is
    non-`nil`, or as unmodified if the flag is `nil`.

    Another effect of calling this function is to cause unconditional
    redisplay of the mode line for the current buffer. In fact, the
    function `force-mode-line-update` works by doing this:

    ```
    (set-buffer-modified-p (buffer-modified-p))
    ```

Function: **restore-buffer-modified-p** flag [¶](#index-restore_002dbuffer_002dmodified_002dp)
:   Like `set-buffer-modified-p`, but does not force redisplay of
    mode lines. This function also allows flag’s value to be
    the symbol `autosaved`, which marks the buffer as modified and
    auto-saved after the last modification.

Command: **not-modified** &optional arg [¶](#index-not_002dmodified)
:   This command marks the current buffer as unmodified, and not needing
    to be saved. If arg is non-`nil`, it marks the buffer as
    modified, so that it will be saved at the next suitable occasion.
    Interactively, arg is the prefix argument.

    Don’t use this function in programs, since it prints a message in the
    echo area; use `set-buffer-modified-p` (above) instead.

Function: **buffer-modified-tick** &optional buffer [¶](#index-buffer_002dmodified_002dtick)
:   This function returns buffer’s modification-count. This is a
    counter that increments every time the buffer is modified. If
    buffer is `nil` (or omitted), the current buffer is used.

Function: **buffer-chars-modified-tick** &optional buffer [¶](#index-buffer_002dchars_002dmodified_002dtick)
:   This function returns buffer’s character-change modification-count.
    Changes to text properties leave this counter unchanged; however, each
    time text is inserted or removed from the buffer, the counter is reset
    to the value that would be returned by `buffer-modified-tick`.
    By comparing the values returned by two `buffer-chars-modified-tick`
    calls, you can tell whether a character change occurred in that buffer
    in between the calls. If buffer is `nil` (or omitted), the
    current buffer is used.

Sometimes there’s a need for modifying buffer in a way that doesn’t
really change its text, like if only its text properties are changed.
If your program needs to modify a buffer without triggering any hooks
and features that react to buffer modifications, use the
`with-silent-modifications` macro.

Macro: **with-silent-modifications** body… [¶](#index-with_002dsilent_002dmodifications)
:   Execute body pretending it does not modify the buffer. This
    includes checking whether the buffer’s file is locked (see [File Locks](#File-Locks)), running buffer modification hooks (see [Change Hooks](#Change-Hooks)),
    etc. Note that if body actually modifies the buffer text (as
    opposed to its text properties), its undo data may become corrupted.

---

Next: [Read-Only Buffers](#Read-Only-Buffers), Previous: [Buffer Modification](#Buffer-Modification), Up: [Buffers](#Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 28.6 Buffer Modification Time [¶](#Buffer-Modification-Time)

Suppose that you visit a file and make changes in its buffer, and
meanwhile the file itself is changed on disk. At this point, saving the
buffer would overwrite the changes in the file. Occasionally this may
be what you want, but usually it would lose valuable information. Emacs
therefore checks the file’s modification time using the functions
described below before saving the file. (See [File Attributes](#File-Attributes),
for how to examine a file’s modification time.)

Function: **verify-visited-file-modtime** &optional buffer [¶](#index-verify_002dvisited_002dfile_002dmodtime)
:   This function compares what buffer (by default, the
    current-buffer) has recorded for the modification time of its visited
    file against the actual modification time of the file as recorded by the
    operating system. The two should be the same unless some other process
    has written the file since Emacs visited or saved it.

    The function returns `t` if the last actual modification time and
    Emacs’s recorded modification time are the same, `nil` otherwise.
    It also returns `t` if the buffer has no recorded last
    modification time, that is if `visited-file-modtime` would return
    zero.

    It always returns `t` for buffers that are not visiting a file,
    even if `visited-file-modtime` returns a non-zero value. For
    instance, it always returns `t` for dired buffers. It returns
    `t` for buffers that are visiting a file that does not exist and
    never existed, but `nil` for file-visiting buffers whose file has
    been deleted.

Function: **clear-visited-file-modtime** [¶](#index-clear_002dvisited_002dfile_002dmodtime)
:   This function clears out the record of the last modification time of
    the file being visited by the current buffer. As a result, the next
    attempt to save this buffer will not complain of a discrepancy in
    file modification times.

    This function is called in `set-visited-file-name` and other
    exceptional places where the usual test to avoid overwriting a changed
    file should not be done.

Function: **visited-file-modtime** [¶](#index-visited_002dfile_002dmodtime)
:   This function returns the current buffer’s recorded last file
    modification time, as a Lisp timestamp (see [Time of Day](#Time-of-Day)).

    If the buffer has no recorded last modification time, this function
    returns zero. This case occurs, for instance, if the buffer is not
    visiting a file or if the time has been explicitly cleared by
    `clear-visited-file-modtime`. Note, however, that
    `visited-file-modtime` returns a timestamp for some non-file buffers
    too. For instance, in a Dired buffer listing a directory, it returns
    the last modification time of that directory, as recorded by Dired.

    If the buffer is visiting a file that doesn’t exist, this function
    returns −1.

Function: **set-visited-file-modtime** &optional time [¶](#index-set_002dvisited_002dfile_002dmodtime)
:   This function updates the buffer’s record of the last modification time
    of the visited file, to the value specified by time if time
    is not `nil`, and otherwise to the last modification time of the
    visited file.

    If time is neither `nil` nor an integer flag returned
    by `visited-file-modtime`, it should be a Lisp time value
    (see [Time of Day](#Time-of-Day)).

    This function is useful if the buffer was not read from the file
    normally, or if the file itself has been changed for some known benign
    reason.

Function: **ask-user-about-supersession-threat** filename [¶](#index-ask_002duser_002dabout_002dsupersession_002dthreat)
:   This function is used to ask a user how to proceed after an attempt to
    modify a buffer visiting file filename when the file is newer
    than the buffer text. Emacs detects this because the modification
    time of the file on disk is newer than the last save-time and its contents
    have changed.
    This means some other program has probably altered the file.

    Depending on the user’s answer, the function may return normally, in
    which case the modification of the buffer proceeds, or it may signal a
    `file-supersession` error with data `(filename)`, in which
    case the proposed buffer modification is not allowed.

    This function is called automatically by Emacs on the proper
    occasions. It exists so you can customize Emacs by redefining it.
    See the file `userlock.el` for the standard definition.

    See also the file locking mechanism in [File Locks](#File-Locks).

---

Next: [The Buffer List](#Buffer-List), Previous: [Buffer Modification Time](#Modification-Time), Up: [Buffers](#Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 28.7 Read-Only Buffers [¶](#Read_002dOnly-Buffers)

If a buffer is *read-only*, then you cannot change its contents,
although you may change your view of the contents by scrolling and
narrowing.

Read-only buffers are used in two kinds of situations:

* A buffer visiting a write-protected file is normally read-only.

  Here, the purpose is to inform the user that editing the buffer with the
  aim of saving it in the file may be futile or undesirable. The user who
  wants to change the buffer text despite this can do so after clearing
  the read-only flag with `C-x C-q`.
* Modes such as Dired and Rmail make buffers read-only when altering the
  contents with the usual editing commands would probably be a mistake.

  The special commands of these modes bind `buffer-read-only` to
  `nil` (with `let`) or bind `inhibit-read-only` to
  `t` around the places where they themselves change the text.

Variable: **buffer-read-only** [¶](#index-buffer_002dread_002donly)
:   This buffer-local variable specifies whether the buffer is read-only.
    The buffer is read-only if this variable is non-`nil`. However,
    characters that have the `inhibit-read-only` text property can
    still be modified. See [inhibit-read-only](#Special-Properties).

Variable: **inhibit-read-only** [¶](#index-inhibit_002dread_002donly)
:   If this variable is non-`nil`, then read-only buffers and,
    depending on the actual value, some or all read-only characters may be
    modified. Read-only characters in a buffer are those that have a
    non-`nil` `read-only` text property. See [Properties with Special Meanings](#Special-Properties), for more information about text properties.

    If `inhibit-read-only` is `t`, all `read-only` character
    properties have no effect. If `inhibit-read-only` is a list, then
    `read-only` character properties have no effect if they are members
    of the list (comparison is done with `eq`).

Command: **read-only-mode** &optional arg [¶](#index-read_002donly_002dmode)
:   This is the mode command for Read Only minor mode, a buffer-local
    minor mode. When the mode is enabled, `buffer-read-only` is
    non-`nil` in the buffer; when disabled, `buffer-read-only`
    is `nil` in the buffer. The calling convention is the same as
    for other minor mode commands (see [Conventions for Writing Minor Modes](#Minor-Mode-Conventions)).

    This minor mode mainly serves as a wrapper for
    `buffer-read-only`; unlike most minor modes, there is no separate
    `read-only-mode` variable. Even when Read Only mode is disabled,
    characters with non-`nil` `read-only` text properties remain
    read-only. To temporarily ignore all read-only states, bind
    `inhibit-read-only`, as described above.

    When enabling Read Only mode, this mode command also enables View mode
    if the option `view-read-only` is non-`nil`. See [Miscellaneous Buffer Operations](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Misc-Buffer) in The GNU Emacs Manual.
    When disabling Read Only mode, it disables View mode if View mode was
    enabled.

Function: **barf-if-buffer-read-only** &optional position [¶](#index-barf_002dif_002dbuffer_002dread_002donly)
:   This function signals a `buffer-read-only` error if the current
    buffer is read-only. If the text at position (which defaults to
    point) has the `inhibit-read-only` text property set, the error
    will not be raised.

    See [Using `interactive`](#Using-Interactive), for another way to signal an error if the
    current buffer is read-only.

---

Next: [Creating Buffers](#Creating-Buffers), Previous: [Read-Only Buffers](#Read-Only-Buffers), Up: [Buffers](#Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 28.8 The Buffer List [¶](#The-Buffer-List)

The *buffer list* is a list of all live buffers. The order of the
buffers in this list is based primarily on how recently each buffer has
been displayed in a window. Several functions, notably
`other-buffer`, use this ordering. A buffer list displayed for the
user also follows this order.

Creating a buffer adds it to the end of the buffer list, and killing
a buffer removes it from that list. A buffer moves to the front of
this list whenever it is chosen for display in a window
(see [Switching to a Buffer in a Window](#Switching-Buffers)) or a window displaying it is selected
(see [Selecting Windows](#Selecting-Windows)). A buffer moves to the end of the list
when it is buried (see `bury-buffer`, below). There are no
functions available to the Lisp programmer which directly manipulate
the buffer list.

In addition to the fundamental buffer list just described, Emacs
maintains a local buffer list for each frame, in which the buffers that
have been displayed (or had their windows selected) in that frame come
first. (This order is recorded in the frame’s `buffer-list` frame
parameter; see [Buffer Parameters](#Buffer-Parameters).) Buffers never displayed in
that frame come afterward, ordered according to the fundamental buffer
list.

Function: **buffer-list** &optional frame [¶](#index-buffer_002dlist)
:   This function returns the buffer list, including all buffers, even those
    whose names begin with a space. The elements are actual buffers, not
    their names.

    If frame is a frame, this returns frame’s local buffer list.
    If frame is `nil` or omitted, the fundamental buffer list is
    used: the buffers appear in order of most recent display or selection,
    regardless of which frames they were displayed on.

    ```
    (buffer-list)
         ⇒ (#<buffer buffers.texi>
             #<buffer  *Minibuf-1*> #<buffer buffer.c>
             #<buffer *Help*> #<buffer TAGS>)
    ```

    ```

    ```

    ```
    ;; Note that the name of the minibuffer
    ;;   begins with a space!
    (mapcar #'buffer-name (buffer-list))
        ⇒ ("buffers.texi" " *Minibuf-1*"
            "buffer.c" "*Help*" "TAGS")
    ```

The list returned by `buffer-list` is constructed specifically;
it is not an internal Emacs data structure, and modifying it has no
effect on the order of buffers. If you want to change the order of
buffers in the fundamental buffer list, here is an easy way:

```
(defun reorder-buffer-list (new-list)
  (while new-list
    (bury-buffer (car new-list))
    (setq new-list (cdr new-list))))
```

With this method, you can specify any order for the list, but there is
no danger of losing a buffer or adding something that is not a valid
live buffer.

To change the order or value of a specific frame’s buffer list, set
that frame’s `buffer-list` parameter with
`modify-frame-parameters` (see [Access to Frame Parameters](#Parameter-Access)).

Function: **other-buffer** &optional buffer visible-ok frame [¶](#index-other_002dbuffer)
:   This function returns the first buffer in the buffer list other than
    buffer. Usually, this is the buffer appearing in the most
    recently selected window (in frame frame or else the selected
    frame, see [Input Focus](#Input-Focus)), aside from buffer. Buffers whose
    names start with a space are not considered at all.

    If buffer is not supplied (or if it is not a live buffer), then
    `other-buffer` returns the first buffer in the selected frame’s
    local buffer list. (If frame is non-`nil`, it returns the
    first buffer in frame’s local buffer list instead.)

    If frame has a non-`nil` `buffer-predicate` parameter,
    then `other-buffer` uses that predicate to decide which buffers to
    consider. It calls the predicate once for each buffer, and if the value
    is `nil`, that buffer is ignored. See [Buffer Parameters](#Buffer-Parameters).

    If visible-ok is `nil`, `other-buffer` avoids returning
    a buffer visible in any window on any visible frame, except as a last
    resort. If visible-ok is non-`nil`, then it does not matter
    whether a buffer is displayed somewhere or not.

    If no suitable buffer exists, the buffer `*scratch*` is returned
    (and created, if necessary).

Function: **last-buffer** &optional buffer visible-ok frame [¶](#index-last_002dbuffer)
:   This function returns the last buffer in frame’s buffer list other
    than buffer. If frame is omitted or `nil`, it uses the
    selected frame’s buffer list.

    The argument visible-ok is handled as with `other-buffer`,
    see above. If no suitable buffer can be found, the buffer
    `*scratch*` is returned.

Command: **bury-buffer** &optional buffer-or-name [¶](#index-bury_002dbuffer)
:   This command puts buffer-or-name at the end of the buffer list,
    without changing the order of any of the other buffers on the list.
    This buffer therefore becomes the least desirable candidate for
    `other-buffer` to return. The argument can be either a buffer
    itself or the name of one.

    This function operates on each frame’s `buffer-list` parameter as
    well as the fundamental buffer list; therefore, the buffer that you bury
    will come last in the value of `(buffer-list frame)` and in
    the value of `(buffer-list)`. In addition, it also puts the buffer
    at the end of the list of buffers of the selected window (see [Window History](#Window-History)) provided it is shown in that window.

    If buffer-or-name is `nil` or omitted, this means to bury the
    current buffer. In addition, if the current buffer is displayed in the
    selected window (see [Selecting Windows](#Selecting-Windows)), this makes sure that the
    window is either deleted or another buffer is shown in it. More
    precisely, if the selected window is dedicated (see [Dedicated Windows](#Dedicated-Windows)) and there are other windows on its frame, the window is
    deleted. If it is the only window on its frame and that frame is not
    the only frame on its terminal, the frame is dismissed by calling the
    function specified by `frame-auto-hide-function` (see [Quitting Windows](#Quitting-Windows)). Otherwise, it calls `switch-to-prev-buffer`
    (see [Window History](#Window-History)) to show another buffer in that window. If
    buffer-or-name is displayed in some other window, it remains
    displayed there.

    To replace a buffer in all the windows that display it, use
    `replace-buffer-in-windows`, See [Buffers and Windows](#Buffers-and-Windows).

Command: **unbury-buffer** [¶](#index-unbury_002dbuffer)
:   This command switches to the last buffer in the local buffer list of
    the selected frame. More precisely, it calls the function
    `switch-to-buffer` (see [Switching to a Buffer in a Window](#Switching-Buffers)), to display the
    buffer returned by `last-buffer` (see above), in the selected
    window.

Variable: **buffer-list-update-hook** [¶](#index-buffer_002dlist_002dupdate_002dhook)
:   This is a normal hook run whenever the buffer list changes. Functions
    (implicitly) running this hook are `get-buffer-create`
    (see [Creating Buffers](#Creating-Buffers)), `rename-buffer` (see [Buffer Names](#Buffer-Names)), `kill-buffer` (see [Killing Buffers](#Killing-Buffers)),
    `bury-buffer` (see above), and `select-window`
    (see [Selecting Windows](#Selecting-Windows)). This hook is not run for internal or
    temporary buffers created by `get-buffer-create` or
    `generate-new-buffer` with a non-`nil` argument
    inhibit-buffer-hooks.

    Functions run by this hook should avoid calling `select-window`
    with a `nil` norecord argument since this may lead to
    infinite recursion.

Function: **buffer-match-p** condition buffer-or-name &rest args [¶](#index-buffer_002dmatch_002dp)
:   This function checks if a buffer designated by `buffer-or-name`
    satisfies the specified condition. Optional arguments
    args are passed to the predicate function in condition. A
    valid condition can be one of the following:

    * A string, interpreted as a regular expression. The buffer
      satisfies the condition if the regular expression matches the buffer
      name.
    * A predicate function, which should return non-`nil` if the buffer
      matches. It is called with
      buffer-or-name as the first argument followed by args.
    * A cons-cell `(oper . expr)` where oper is one
      of

      `(not cond)`
      :   Satisfied if cond doesn’t satisfy `buffer-match-p` with
          the same buffer and `args`.

      `(or conds…)`
      :   Satisfied if *any* condition in conds satisfies
          `buffer-match-p`, with the same buffer and `args`.

      `(and conds…)`
      :   Satisfied if *all* the conditions in conds satisfy
          `buffer-match-p`, with the same buffer and `args`.

      `derived-mode`
      :   Satisfied if the buffer’s major mode derives from expr.
          The value of expr can be either a single mode symbol
          or a list of mode symbols as accepted by the function
          `provided-mode-derived-p` (see [Defining Derived Modes](#Derived-Modes)).
          Note that this condition might fail to report a match if
          `buffer-match-p` is invoked before the major mode of
          the buffer has been established.

      `major-mode`
      :   Satisfied if the buffer’s major mode is equal to expr. Prefer
          using `derived-mode` instead, when both can work. Note that this
          condition might fail to report a match if `buffer-match-p` is
          invoked before the major mode of the buffer has been established.

      `category`
      :   This is pertinent only when this function is called by
          `display-buffer` (see [Action Alists for Buffer Display](#Buffer-Display-Action-Alists)), and is
          satisfied if the action alist with which `display-buffer` was
          called includes `(category . expr)` in the value of its
          action argument. See [Action Alists for Buffer Display](#Buffer-Display-Action-Alists).

      `this-command` [¶](#index-this_002dcommand_002c-a-buffer-display-condition-entry)
      :   This too is pertinent only when this function is called by
          `display-buffer` (see [Action Alists for Buffer Display](#Buffer-Display-Action-Alists)), and is
          satisfied when the command now being executed is equal to expr, or
          if expr is a list, when the command now being executed is a member
          of that list.
    * t
      Satisfied by any buffer. A convenient alternative to `""` (empty
      string) or `(and)` (empty conjunction).

Function: **match-buffers** condition &optional buffer-list &rest args [¶](#index-match_002dbuffers)
:   This function returns a list of all buffers that satisfy the
    condition. If no buffers match, the function returns
    `nil`. The argument condition is as defined in
    `buffer-match-p` above. By default, all the buffers are
    considered, but this can be restricted via the optional argument
    `buffer-list`, which should be a list of buffers to consider.
    Remaining arguments args will be passed to condition in
    the same way as `buffer-match-p` does.

---

Next: [Killing Buffers](#Killing-Buffers), Previous: [The Buffer List](#Buffer-List), Up: [Buffers](#Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 28.9 Creating Buffers [¶](#Creating-Buffers-1)

This section describes the two primitives for creating buffers.
`get-buffer-create` creates a buffer if it finds no existing
buffer with the specified name; `generate-new-buffer` always
creates a new buffer and gives it a unique name.

Both functions accept an optional argument inhibit-buffer-hooks.
If it is non-`nil`, the buffer they create does not run the hooks
`kill-buffer-hook`, `kill-buffer-query-functions`
(see [Killing Buffers](#Killing-Buffers)), and `buffer-list-update-hook`
(see [The Buffer List](#Buffer-List)). This avoids slowing down internal or temporary
buffers that are never presented to users or passed on to other
applications.

Other functions you can use to create buffers include
`with-output-to-temp-buffer` (see [Temporary Displays](#Temporary-Displays)) and
`create-file-buffer` (see [Visiting Files](#Visiting-Files)). Starting a
subprocess can also create a buffer (see [Processes](#Processes)).

Function: **get-buffer-create** buffer-or-name &optional inhibit-buffer-hooks [¶](#index-get_002dbuffer_002dcreate)
:   This function returns a buffer named buffer-or-name. The buffer
    returned does not become the current buffer—this function does not
    change which buffer is current.

    buffer-or-name must be either a string or an existing buffer. If
    it is a string and a live buffer with that name already exists,
    `get-buffer-create` returns that buffer. If no such buffer exists,
    it creates a new buffer. If buffer-or-name is a buffer instead of
    a string, it is returned as given, even if it is dead.

    ```
    (get-buffer-create "foo")
         ⇒ #<buffer foo>
    ```

    The major mode for a newly created buffer is set to Fundamental mode.
    (The default value of the variable `major-mode` is handled at a higher
    level; see [How Emacs Chooses a Major Mode](#Auto-Major-Mode).) If the name begins with a space, the
    buffer initially disables undo information recording (see [Undo](#Undo)).

Function: **generate-new-buffer** name &optional inhibit-buffer-hooks [¶](#index-generate_002dnew_002dbuffer)
:   This function returns a newly created, empty buffer, but does not make
    it current. The name of the buffer is generated by passing name
    to the function `generate-new-buffer-name` (see [Buffer Names](#Buffer-Names)). Thus, if there is no buffer named name, then that is
    the name of the new buffer; if that name is in use, a suffix of the
    form ‘`<n>`’, where n is an integer, is appended to
    name.

    An error is signaled if name is not a string.

    ```
    (generate-new-buffer "bar")
         ⇒ #<buffer bar>
    ```

    ```
    (generate-new-buffer "bar")
         ⇒ #<buffer bar<2>>
    ```

    ```
    (generate-new-buffer "bar")
         ⇒ #<buffer bar<3>>
    ```

    The major mode for the new buffer is set to Fundamental mode. The default
    value of the variable `major-mode` is handled at a higher level.
    See [How Emacs Chooses a Major Mode](#Auto-Major-Mode).

---

Next: [Indirect Buffers](#Indirect-Buffers), Previous: [Creating Buffers](#Creating-Buffers), Up: [Buffers](#Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 28.10 Killing Buffers [¶](#Killing-Buffers-1)

*Killing a buffer* makes its name unknown to Emacs and makes the
memory space it occupied available for other use.

The buffer object for the buffer that has been killed remains in
existence as long as anything refers to it, but it is specially marked
so that you cannot make it current or display it. Killed buffers retain
their identity, however; if you kill two distinct buffers, they remain
distinct according to `eq` although both are dead.

If you kill a buffer that is current or displayed in a window, Emacs
automatically selects or displays some other buffer instead. This
means that killing a buffer can change the current buffer. Therefore,
when you kill a buffer, you should also take the precautions
associated with changing the current buffer (unless you happen to know
that the buffer being killed isn’t current). See [The Current Buffer](#Current-Buffer).

If you kill a buffer that is the base buffer of one or more indirect
buffers (see [Indirect Buffers](#Indirect-Buffers)),
the indirect buffers are automatically killed as well.

The `buffer-name` of a buffer is `nil` if, and only if,
the buffer is killed. A buffer that has not been killed is called a
*live* buffer. To test whether a buffer is live or killed, use
the function `buffer-live-p` (see below).

Command: **kill-buffer** &optional buffer-or-name [¶](#index-kill_002dbuffer)
:   This function kills the buffer buffer-or-name, freeing all its
    memory for other uses or to be returned to the operating system. If
    buffer-or-name is `nil` or omitted, it kills the current
    buffer.

    Any processes that have this buffer as the `process-buffer` are
    sent the `SIGHUP` (hangup) signal, which normally causes them
    to terminate. See [Sending Signals to Processes](#Signals-to-Processes).

    If the buffer is visiting a file and contains unsaved changes,
    `kill-buffer` asks the user to confirm before the buffer is killed.
    It does this even if not called interactively. To prevent the request
    for confirmation, clear the modified flag before calling
    `kill-buffer`. See [Buffer Modification](#Buffer-Modification).

    This function calls `replace-buffer-in-windows` for cleaning up
    all windows currently displaying the buffer to be killed.

    Killing a buffer that is already dead has no effect.

    This function returns `t` if it actually killed the buffer. It
    returns `nil` if the user refuses to confirm or if
    buffer-or-name was already dead.

    ```
    (kill-buffer "foo.unchanged")
         ⇒ t
    (kill-buffer "foo.changed")

    ---------- Buffer: Minibuffer ----------
    Buffer foo.changed modified; kill anyway? (yes or no) yes
    ---------- Buffer: Minibuffer ----------

         ⇒ t
    ```

Variable: **kill-buffer-query-functions** [¶](#index-kill_002dbuffer_002dquery_002dfunctions)
:   Before confirming unsaved changes, `kill-buffer` calls the functions
    in the list `kill-buffer-query-functions`, in order of appearance,
    with no arguments. The buffer being killed is the current buffer when
    they are called. The idea of this feature is that these functions will
    ask for confirmation from the user. If any of them returns `nil`,
    `kill-buffer` spares the buffer’s life.

    This hook is not run for internal or temporary buffers created by
    `get-buffer-create` or `generate-new-buffer` with a
    non-`nil` argument inhibit-buffer-hooks.

Variable: **kill-buffer-hook** [¶](#index-kill_002dbuffer_002dhook)
:   This is a normal hook run by `kill-buffer` after asking all the
    questions it is going to ask, just before actually killing the buffer.
    The buffer to be killed is current when the hook functions run.
    See [Hooks](#Hooks). This variable is a permanent local, so its local binding
    is not cleared by changing major modes.

    This hook is not run for internal or temporary buffers created by
    `get-buffer-create` or `generate-new-buffer` with a
    non-`nil` argument inhibit-buffer-hooks.

User Option: **buffer-offer-save** [¶](#index-buffer_002doffer_002dsave)
:   This variable, if non-`nil` in a particular buffer, tells
    `save-buffers-kill-emacs` to offer to save that buffer, just as
    it offers to save file-visiting buffers. If `save-some-buffers`
    is called with the second optional argument set to `t`, it will
    also offer to save the buffer. Lastly, if this variable is set to the
    symbol `always`, both `save-buffers-kill-emacs` and
    `save-some-buffers` will always offer to save. See [Definition of save-some-buffers](#Definition-of-save_002dsome_002dbuffers). The variable `buffer-offer-save`
    automatically becomes buffer-local when set for any reason.
    See [Buffer-Local Variables](#Buffer_002dLocal-Variables).

Variable: **buffer-save-without-query** [¶](#index-buffer_002dsave_002dwithout_002dquery)
:   This variable, if non-`nil` in a particular buffer, tells
    `save-buffers-kill-emacs` and `save-some-buffers` to save
    this buffer (if it’s modified) without asking the user. The variable
    automatically becomes buffer-local when set for any reason.

Function: **buffer-live-p** object [¶](#index-buffer_002dlive_002dp)
:   This function returns `t` if object is a live buffer (a
    buffer which has not been killed), `nil` otherwise.

---

Next: [Swapping Text Between Two Buffers](#Swapping-Text), Previous: [Killing Buffers](#Killing-Buffers), Up: [Buffers](#Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 28.11 Indirect Buffers [¶](#Indirect-Buffers-1)

An *indirect buffer* shares the text of some other buffer, which
is called the *base buffer* of the indirect buffer. In some ways it
is the analogue, for buffers, of a symbolic link among files. The base
buffer may not itself be an indirect buffer.

The text of the indirect buffer is always identical to the text of its
base buffer; changes made by editing either one are visible immediately
in the other. This includes the text properties as well as the characters
themselves.

In all other respects, the indirect buffer and its base buffer are
completely separate. They have different names, independent values of
point, independent narrowing, independent markers and overlays (though
inserting or deleting text in either buffer relocates the markers and
overlays for both), independent major modes, and independent
buffer-local variable bindings.

An indirect buffer cannot visit a file, but its base buffer can. If
you try to save the indirect buffer, that actually saves the base
buffer.

Killing an indirect buffer has no effect on its base buffer. Killing
the base buffer effectively kills the indirect buffer in that it cannot
ever again be the current buffer.

Command: **make-indirect-buffer** base-buffer name &optional clone inhibit-buffer-hooks [¶](#index-make_002dindirect_002dbuffer)
:   This creates and returns an indirect buffer named name whose
    base buffer is base-buffer. The argument base-buffer may
    be a live buffer or the name (a string) of an existing buffer. If
    name is the name of an existing buffer, an error is signaled.

    If clone is non-`nil`, then the indirect buffer originally
    shares the state of base-buffer such as major mode, minor
    modes, buffer local variables and so on. If clone is omitted
    or `nil` the indirect buffer’s state is set to the default state
    for new buffers.

    If base-buffer is an indirect buffer, its base buffer is used as
    the base for the new buffer. If, in addition, clone is
    non-`nil`, the initial state is copied from the actual base
    buffer, not from base-buffer.

    See [Creating Buffers](#Creating-Buffers), for the meaning of inhibit-buffer-hooks.

Command: **clone-indirect-buffer** newname display-flag &optional norecord [¶](#index-clone_002dindirect_002dbuffer)
:   This function creates and returns a new indirect buffer that shares
    the current buffer’s base buffer and copies the rest of the current
    buffer’s attributes. (If the current buffer is not indirect, it is
    used as the base buffer.)

    If display-flag is non-`nil`, as it always is in
    interactive calls, that means to display the new buffer by calling
    `pop-to-buffer`. If norecord is non-`nil`, that means
    not to put the new buffer to the front of the buffer list.

Function: **buffer-base-buffer** &optional buffer [¶](#index-buffer_002dbase_002dbuffer)
:   This function returns the base buffer of buffer, which defaults
    to the current buffer. If buffer is not indirect, the value is
    `nil`. Otherwise, the value is another buffer, which is never an
    indirect buffer.

---

Next: [The Buffer Gap](#Buffer-Gap), Previous: [Indirect Buffers](#Indirect-Buffers), Up: [Buffers](#Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 28.12 Swapping Text Between Two Buffers [¶](#Swapping-Text-Between-Two-Buffers)

Specialized modes sometimes need to let the user access from the
same buffer several vastly different types of text. For example, you
may need to display a summary of the buffer text, in addition to
letting the user access the text itself.

This could be implemented with multiple buffers (kept in sync when
the user edits the text), or with narrowing (see [Narrowing](#Narrowing)). But
these alternatives might sometimes become tedious or prohibitively
expensive, especially if each type of text requires expensive
buffer-global operations in order to provide correct display and
editing commands.

Emacs provides another facility for such modes: you can quickly swap
buffer text between two buffers with `buffer-swap-text`. This
function is very fast because it doesn’t move any text, it only
changes the internal data structures of the buffer object to point to
a different chunk of text. Using it, you can pretend that a group of
two or more buffers are actually a single virtual buffer that holds
the contents of all the individual buffers together.

Function: **buffer-swap-text** buffer [¶](#index-buffer_002dswap_002dtext)
:   This function swaps the text of the current buffer and that of its
    argument buffer. It signals an error if one of the two buffers
    is an indirect buffer (see [Indirect Buffers](#Indirect-Buffers)) or is a base buffer
    of an indirect buffer.

    All the buffer properties that are related to the buffer text are
    swapped as well: the positions of point and mark, all the markers, the
    overlays, the text properties, the undo list, the value of the
    `enable-multibyte-characters` flag (see [enable-multibyte-characters](#Text-Representations)), etc.

    **Warning:** If this function is called from within a
    `save-excursion` form, the current buffer will be set to
    buffer upon leaving the form, since the marker used by
    `save-excursion` to save the position and buffer will be swapped
    as well.

If you use `buffer-swap-text` on a file-visiting buffer, you
should set up a hook to save the buffer’s original text rather than
what it was swapped with. `write-region-annotate-functions`
works for this purpose. You should probably set
`buffer-saved-size` to −2 in the buffer, so that changes
in the text it is swapped with will not interfere with auto-saving.

---

Previous: [Swapping Text Between Two Buffers](#Swapping-Text), Up: [Buffers](#Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 28.13 The Buffer Gap [¶](#The-Buffer-Gap)

Emacs buffers are implemented using an invisible *gap* to make
insertion and deletion faster. Insertion works by filling in part of
the gap, and deletion adds to the gap. Of course, this means that the
gap must first be moved to the locus of the insertion or deletion.
Emacs moves the gap only when you try to insert or delete. This is why
your first editing command in one part of a large buffer, after
previously editing in another far-away part, sometimes involves a
noticeable delay.

This mechanism works invisibly, and Lisp code should never be affected
by the gap’s current location, but these functions are available for
getting information about the gap status.

Function: **gap-position** [¶](#index-gap_002dposition)
:   This function returns the current gap position in the current buffer.

Function: **gap-size** [¶](#index-gap_002dsize)
:   This function returns the current gap size of the current buffer.

---

Next: [Frames](#Frames), Previous: [Buffers](#Buffers), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 29 Windows [¶](#Windows-1)

This chapter describes the functions and variables related to Emacs
windows. See [Frames](#Frames), for how windows are assigned an area of screen
available for Emacs to use. See [Emacs Display](#Display), for information on how text
is displayed in windows.

* [Basic Concepts of Emacs Windows](#Basic-Windows)
* [Windows and Frames](#Windows-and-Frames)
* [Selecting Windows](#Selecting-Windows)
* [Window Sizes](#Window-Sizes)
* [Resizing Windows](#Resizing-Windows)
* [Preserving Window Sizes](#Preserving-Window-Sizes)
* [Splitting Windows](#Splitting-Windows)
* [Deleting Windows](#Deleting-Windows)
* [Recombining Windows](#Recombining-Windows)
* [Resurrecting Windows](#Resurrecting-Windows)
* [Changing Window Layouts](#Changing-Window-Layouts)
* [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)
* [Buffers and Windows](#Buffers-and-Windows)
* [Switching to a Buffer in a Window](#Switching-Buffers)
* [Displaying a Buffer in a Suitable Window](#Displaying-Buffers)
* [Window History](#Window-History)
* [Dedicated Windows](#Dedicated-Windows)
* [Quitting Windows](#Quitting-Windows)
* [Side Windows](#Side-Windows)
* [Atomic Windows](#Atomic-Windows)
* [Windows and Point](#Window-Point)
* [The Window Start and End Positions](#Window-Start-and-End)
* [Textual Scrolling](#Textual-Scrolling)
* [Vertical Fractional Scrolling](#Vertical-Scrolling)
* [Horizontal Scrolling](#Horizontal-Scrolling)
* [Coordinates and Windows](#Coordinates-and-Windows)
* [Mouse Window Auto-selection](#Mouse-Window-Auto_002dselection)
* [Window Configurations](#Window-Configurations)
* [Window Parameters](#Window-Parameters)
* [Hooks for Window Scrolling and Changes](#Window-Hooks)

---

Next: [Windows and Frames](#Windows-and-Frames), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.1 Basic Concepts of Emacs Windows [¶](#Basic-Concepts-of-Emacs-Windows)

A *window* is an area of the screen that can be used to display a
buffer (see [Buffers](#Buffers)). Windows are grouped into frames
(see [Frames](#Frames)). Each frame contains at least one window; the user can
subdivide a frame into multiple, non-overlapping windows to view several
buffers at once. Lisp programs can use multiple windows for a variety
of purposes. In Rmail, for example, you can view a summary of message
titles in one window, and the contents of the selected message in
another window.

Emacs uses the term “window” with a different meaning than in
graphical desktop environments and window systems, such as the X Window
System. When Emacs is run on X, each graphical X window owned by the
Emacs process corresponds to one Emacs frame. When Emacs is run on a
text terminal, each Emacs frame fills the entire terminal screen. In
either case, the frame may contain one or more Emacs windows. For
disambiguation, we use the term *window-system window* when we mean
the window-system window corresponding to an Emacs frame.

Unlike X windows, Emacs windows are *tiled*; they never overlap
within the area of their frame. When a window is created, resized, or
deleted, the change in window space is taken from or given to other
windows on the same frame, so that the total area of the frame is
unchanged. The *window layout* of a frame is the result of all
tiling operations for that frame.

In Emacs Lisp, windows are represented by a special Lisp object type
(see [Window Type](#Window-Type)). Two windows are identical if and only if they are
represented by the same Lisp object.

Function: **windowp** object [¶](#index-windowp)
:   This function returns `t` if object is a window (whether or
    not it displays a buffer). Otherwise, it returns `nil`.

A *live window* is one that is actually displaying a buffer in a
frame.

Function: **window-live-p** object [¶](#index-window_002dlive_002dp)
:   This function returns `t` if object is a live window and
    `nil` otherwise. A live window is one that displays a buffer.

The windows in each frame are organized into a *window tree*.
See [Windows and Frames](#Windows-and-Frames). The leaf nodes of each window tree are live
windows—the ones actually displaying buffers. The internal nodes of
the window tree are *internal windows*, which are not live.

A *valid window* is one that is either live or internal. A valid
window can be *deleted*, i.e., removed from its frame
(see [Deleting Windows](#Deleting-Windows)); then it is no longer valid, but the Lisp
object representing it might be still referenced from other Lisp
objects. A deleted window may be made valid again by restoring a saved
window configuration (see [Window Configurations](#Window-Configurations)).

You can distinguish valid windows from deleted windows with
`window-valid-p`.

Function: **window-valid-p** object [¶](#index-window_002dvalid_002dp)
:   This function returns `t` if object is a live window, or an
    internal window in a window tree. Otherwise, it returns `nil`,
    including for the case where object is a deleted window.

The following schematic shows the structure of a live window:

```
        ____________________________________________
       |________________ Tab Line _______________|RD| ^
       |______________ Header Line ______________|  | |
     ^ |LS|LM|LF|                       |RF|RM|RS|  | |
     | |  |  |  |                       |  |  |  |  | |
Window |  |  |  |                       |  |  |  |  | Window
Body | |  |  |  |      Window Body      |  |  |  |  | Total
Height |  |  |  |                       |  |  |  |  | Height
     | |  |  |  |<- Window Body Width ->|  |  |  |  | |
     v |__|__|__|_______________________|__|__|__|  | |
       |_________ Horizontal Scroll Bar _________|  | |
       |_______________ Mode Line _______________|__| |
       |_____________ Bottom Divider _______________| v
        <---------- Window Total Width ------------>
```

At the center of that window is the *body*, where the buffer
text is displayed. The body can be surrounded by a series of optional
areas which we will call *window decorations*. On the left and
right, from innermost to outermost, these are the left and right
fringes, denoted by LF and RF (see [Fringes](#Fringes)); the left and right
margins, denoted by LM and RM in the schematic (see [Displaying in the Margins](#Display-Margins)); the left or right vertical scroll bar, only one of which is
present at any time, denoted by LS and RS (see [Scroll Bars](#Scroll-Bars)); and
the right divider, denoted by RD (see [Window Dividers](#Window-Dividers)). Together
these are the window’s *left and right decorations*.

At the top of the window are the tab line and the header line
(see [Window Header Lines](#Header-Lines)). The *text area* of the window includes
the header line and the tab line, if they are present in the window.
At the bottom of the window are the horizontal scroll bar
(see [Scroll Bars](#Scroll-Bars)); the mode line (see [Mode Line Format](#Mode-Line-Format)); and
the bottom divider (see [Window Dividers](#Window-Dividers)). Together these form the
window’s *top and bottom decorations*.

There are two special areas omitted in the schematic:

* When any of the fringes is missing, the display engine may use one
  character cell in its place for showing a continuation or truncation
  glyph provided a text line doesn’t fit in a window.
* When both, the vertical scroll bar and the right divider are missing,
  the display engine usurps one pixel for drawing a vertical divider line
  between this window and the window on its right, provided such a window
  exists. On a text terminal, this divider always occupies an entire
  character cell.

In either case, the resulting artifact is considered part of the
window’s body although its screen space cannot be used for displaying
buffer text.

Note also, that line numbers (and their surrounding whitespace) as
displayed by `display-line-numbers-mode` (see [Display Custom](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Display-Custom) in The GNU Emacs Manual) do not count as decorations either; they
are part of the window’s body too.

Internal windows neither show any text nor do they have decorations.
Hence, the concept of “body” does not make sense for them. In fact,
most functions operating on the body of a window will yield an error
when applied to an internal window.

By default, an Emacs frame exhibits one special live window that is
used for displaying messages and accepting user input—the
*minibuffer window* (see [Minibuffer Windows](#Minibuffer-Windows)). Since the
minibuffer window is used for displaying text, it has a body but it does
not have a tab or header line or any margins. Finally, a *tooltip
window* which is used for displaying a tooltip in a tooltip frame
(see [Tooltips](#Tooltips)) has a body too but no decorations at all.

---

Next: [Selecting Windows](#Selecting-Windows), Previous: [Basic Concepts of Emacs Windows](#Basic-Windows), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.2 Windows and Frames [¶](#Windows-and-Frames-1)

Each window belongs to exactly one frame (see [Frames](#Frames)). For all
windows belonging to a specific frame, we sometimes also say that these
windows are *owned* by that frame or simply that they are on that
frame.

Function: **window-frame** &optional window [¶](#index-window_002dframe)
:   This function returns the specified window’s frame—the frame
    that window belongs to. If window is omitted or `nil`,
    it defaults to the selected window (see [Selecting Windows](#Selecting-Windows)).

Function: **window-list** &optional frame minibuffer window [¶](#index-window_002dlist)
:   This function returns a list of all live windows owned by the specified
    frame. If frame is omitted or `nil`, it defaults to
    the selected frame (see [Input Focus](#Input-Focus)).

    The optional argument minibuffer specifies whether to include the
    minibuffer window (see [Minibuffer Windows](#Minibuffer-Windows)) in that list. If
    minibuffer is `t`, the minibuffer window is included. If
    `nil` or omitted, the minibuffer window is included only if it is
    active. If minibuffer is neither `nil` nor `t`, the
    minibuffer window is never included.

    The optional argument window, if non-`nil`, must be a live
    window on the specified frame; then window will be the first
    element in the returned list. If window is omitted or `nil`,
    the window selected within frame (see [Selecting Windows](#Selecting-Windows)) is
    the first element.

Windows on the same frame are organized into a *window tree*,
whose leaf nodes are the live windows. The internal nodes of a window
tree are not live; they exist for the purpose of organizing the
relationships between live windows. The root node of a window tree is
called the *root window*. It is either a live window or an
internal window. If it is a live window, then the frame has just one
window besides the minibuffer window, or the frame is a
minibuffer-only frame, see [Frame Layout](#Frame-Layout).

A minibuffer window (see [Minibuffer Windows](#Minibuffer-Windows)) that is not alone on
its frame does not have a parent window, so it strictly speaking is not
part of its frame’s window tree. Nonetheless, it is a sibling window of
the frame’s root window, and thus can be reached from the root window via
`window-next-sibling`, see below. Also, the function
`window-tree` described at the end of this section lists the
minibuffer window alongside the actual window tree.

Function: **frame-root-window** &optional frame-or-window [¶](#index-frame_002droot_002dwindow)
:   This function returns the root window for frame-or-window. The
    argument frame-or-window should be either a window or a frame;
    if omitted or `nil`, it defaults to the selected frame. If
    frame-or-window is a window, the return value is the root window
    of that window’s frame.

When a live window is split (see [Splitting Windows](#Splitting-Windows)), there are two
live windows where previously there was one. One of these is
represented by the same Lisp window object as the original window, and
the other is represented by a newly-created Lisp window object. Both of
these live windows become leaf nodes of the window tree, as *child
windows* of a single internal window. If necessary, Emacs automatically
creates this internal window, which is also called the *parent
window*, and assigns it to the appropriate position in the window tree.
The set of windows that share the same parent are called *siblings*.

Function: **window-parent** &optional window [¶](#index-window_002dparent)
:   This function returns the parent window of window. If
    window is omitted or `nil`, it defaults to the selected
    window. The return value is `nil` if window has no parent
    (i.e., it is a minibuffer window or the root window of its frame).

A parent window always has at least two child windows. If this number
were to fall to one as a result of window deletion (see [Deleting Windows](#Deleting-Windows)), Emacs automatically deletes the parent window too, and its
sole remaining child window takes its place in the window tree.

A child window can be either a live window, or an internal window
(which in turn would have its own child windows). Therefore, each
internal window can be thought of as occupying a certain rectangular
*screen area*—the union of the areas occupied by the live windows
that are ultimately descended from it.

For each internal window, the screen areas of the immediate children
are arranged either vertically or horizontally (never both). If the
child windows are arranged one above the other, they are said to form
a *vertical combination*; if they are arranged side by side, they
are said to form a *horizontal combination*. Consider the
following example:

```
     ______________________________________
    | ______  ____________________________ |
    ||      || __________________________ ||
    ||      |||                          |||
    ||      |||                          |||
    ||      |||                          |||
    ||      |||____________W4____________|||
    ||      || __________________________ ||
    ||      |||                          |||
    ||      |||                          |||
    ||      |||____________W5____________|||
    ||__W2__||_____________W3_____________ |
    |__________________W1__________________|
```

The root window of this frame is an internal window, W1. Its
child windows form a horizontal combination, consisting of the live
window W2 and the internal window W3. The child windows
of W3 form a vertical combination, consisting of the live
windows W4 and W5. Hence, the live windows in this
window tree are W2, W4, and W5.

The following functions can be used to retrieve a child window of an
internal window, and the siblings of a child window. Their window
argument always defaults to the selected window (see [Selecting Windows](#Selecting-Windows)).

Function: **window-top-child** &optional window [¶](#index-window_002dtop_002dchild)
:   This function returns the topmost child window of window, if
    window is an internal window whose children form a vertical
    combination. For any other type of window, the return value is
    `nil`.

Function: **window-left-child** &optional window [¶](#index-window_002dleft_002dchild)
:   This function returns the leftmost child window of window, if
    window is an internal window whose children form a horizontal
    combination. For any other type of window, the return value is
    `nil`.

Function: **window-child** window [¶](#index-window_002dchild)
:   This function returns the first child window of the internal window
    window—the topmost child window for a vertical combination, or
    the leftmost child window for a horizontal combination. If
    window is a live window, the return value is `nil`.

Function: **window-combined-p** &optional window horizontal [¶](#index-window_002dcombined_002dp)
:   This function returns a non-`nil` value if and only if
    window is part of a vertical combination.

    If the optional argument horizontal is non-`nil`, this
    means to return non-`nil` if and only if window is part of
    a horizontal combination.

Function: **window-next-sibling** &optional window [¶](#index-window_002dnext_002dsibling)
:   This function returns the next sibling of the specified window. The
    return value is `nil` if window is the last child of its
    parent.

Function: **window-prev-sibling** &optional window [¶](#index-window_002dprev_002dsibling)
:   This function returns the previous sibling of the specified window.
    The return value is `nil` if window is the first child of its
    parent.

The functions `window-next-sibling` and `window-prev-sibling`
should not be confused with the functions `next-window` and
`previous-window`, which return the next and previous window in the
cyclic ordering of windows (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)).

The following functions can be useful to locate a window within its
frame.

Function: **frame-first-window** &optional frame-or-window [¶](#index-frame_002dfirst_002dwindow)
:   This function returns the live window at the upper left corner of the
    frame specified by frame-or-window. The argument
    frame-or-window must denote a window or a live frame and defaults
    to the selected frame. If frame-or-window specifies a window,
    this function returns the first window on that window’s frame. Under
    the assumption that the frame from our canonical example is selected
    `(frame-first-window)` returns W2.

Function: **window-at-side-p** &optional window side [¶](#index-window_002dat_002dside_002dp)
:   This function returns `t` if window is located at
    side of its containing frame. The argument window must be
    a valid window and defaults to the selected one. The argument
    side can be any of the symbols `left`, `top`,
    `right` or `bottom`. The default value `nil` is
    handled like `bottom`.

    Note that this function disregards the minibuffer window
    (see [Minibuffer Windows](#Minibuffer-Windows)). Hence, with side equal to
    `bottom` it may return `t` also when the minibuffer window
    appears right below window.

Function: **window-in-direction** direction &optional window ignore sign wrap minibuf [¶](#index-window_002din_002ddirection)
:   This function returns the nearest live window in direction
    direction as seen from the position of window’s point
    (see [Windows and Point](#Window-Point)). The argument direction must be one of
    `above`, `below`, `left` or `right`. The optional
    argument window must denote a live window and defaults to the
    selected one.

    This function does not return a window for which
    `window-no-other-p` (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)) returns
    non-`nil`. If the nearest window’s `no-other-window`
    parameter is non-`nil`, this function tries to find another window
    in the indicated direction whose `no-other-window` parameter is
    `nil`. However, if the optional argument ignore is
    non-`nil`, a window may be returned even if its
    `no-other-window` parameter is non-`nil`.

    If the optional argument sign is a negative number, it means to
    use the right or bottom edge of window as reference position
    instead of `window-point`. If sign is a positive number, it
    means to use the left or top edge of window as reference position.

    If the optional argument wrap is non-`nil`, this means to
    wrap direction around frame borders. For example, if window
    is at the top of the frame and direction is `above`, then
    this function usually returns the frame’s minibuffer window if it’s
    active and a window at the bottom of the frame otherwise.

    If the optional argument minibuf is `t`, this function may
    return the minibuffer window even when it’s not active. If the
    optional argument minibuf is `nil`, this means to return
    the minibuffer window if and only if it is currently active.
    If minibuf is neither `nil` nor `t`, this function
    never returns the minibuffer window. However, if wrap is
    non-`nil`, it always acts as if minibuf were `nil`.

    If it doesn’t find a suitable window, this function returns
    `nil`.

    Don’t use this function to check whether there is *no* window in
    direction. Calling `window-at-side-p` described above is a
    much more efficient way to do that.

The following function retrieves the entire window tree of a frame:

Function: **window-tree** &optional frame [¶](#index-window_002dtree)
:   This function returns a list representing the window tree for frame
    frame. If frame is omitted or `nil`, it defaults to
    the selected frame.

    The return value is a list of the form `(root mini)`,
    where root represents the window tree of the frame’s root
    window, and mini is the frame’s minibuffer window.

    If the root window is live, root is that window itself.
    Otherwise, root is a list `(dir edges w1
    w2 ...)` where dir is `nil` for a horizontal
    combination and `t` for a vertical combination, edges gives
    the size and position of the combination, and the remaining elements
    are the child windows. Each child window may again be a window object
    (for a live window) or a list with the same format as above (for an
    internal window). The edges element is a list `(left
    top right bottom)`, similar to the value returned by
    `window-edges` (see [Coordinates and Windows](#Coordinates-and-Windows)).

---

Next: [Window Sizes](#Window-Sizes), Previous: [Windows and Frames](#Windows-and-Frames), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
