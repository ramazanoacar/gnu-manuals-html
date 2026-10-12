### 33.2 Examining Buffer Contents [¶](#Examining-Buffer-Contents)

This section describes functions that allow a Lisp program to
convert any portion of the text in the buffer into a string.

Function: **buffer-substring** start end [¶](#index-buffer_002dsubstring)
:   This function returns a string containing a copy of the text of the
    region defined by positions start and end in the current
    buffer. If the arguments are not positions in the accessible portion
    of the buffer, `buffer-substring` signals an
    `args-out-of-range` error.

    Here’s an example which assumes Font-Lock mode is not enabled:

    ```
    ---------- Buffer: foo ----------
    This is the contents of buffer foo

    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (buffer-substring 1 10)
         ⇒ "This is t"
    ```

    ```
    (buffer-substring (point-max) 10)
         ⇒ "he contents of buffer foo\n"
    ```

    If the text being copied has any text properties, these are copied into
    the string along with the characters they belong to. See [Text Properties](#Text-Properties). However, overlays (see [Overlays](#Overlays)) in the buffer and
    their properties are ignored, not copied.

    For example, if Font-Lock mode is enabled, you might get results like
    these:

    ```
    (buffer-substring 1 10)
         ⇒ #("This is t" 0 1 (fontified t) 1 9 (fontified t))
    ```

Function: **buffer-substring-no-properties** start end [¶](#index-buffer_002dsubstring_002dno_002dproperties)
:   This is like `buffer-substring`, except that it does not copy text
    properties, just the characters themselves. See [Text Properties](#Text-Properties).

Function: **buffer-string** [¶](#index-buffer_002dstring)
:   This function returns the contents of the entire accessible portion of
    the current buffer, as a string. If the text being copied has any
    text properties, these are copied into the string along with the
    characters they belong to.

If you need to make sure the resulting string, when copied to a
different location, will not change its visual appearance due to
reordering of bidirectional text, use the
`buffer-substring-with-bidi-context` function
(see [buffer-substring-with-bidi-context](#Bidirectional-Display)).

Function: **filter-buffer-substring** start end &optional delete [¶](#index-filter_002dbuffer_002dsubstring)
:   This function filters the buffer text between start and end
    using a function specified by the variable
    `filter-buffer-substring-function`, and returns the result. The
    order of buffer positions specified by start and end should
    not matter, since this function is frequently used for extracting text
    passed to kill functions (see [Functions for Killing](#Kill-Functions)).

    The default filter function consults the obsolete wrapper hook
    `filter-buffer-substring-functions` (see the documentation string
    of the macro `with-wrapper-hook` for the details about this
    obsolete facility). If it is `nil`, it returns the unaltered
    text from the buffer, i.e., what `buffer-substring` would return.

    If delete is non-`nil`, the function deletes the text
    between start and end after copying it, like
    `delete-and-extract-region`.

    Lisp code should use this function instead of `buffer-substring`,
    `buffer-substring-no-properties`,
    or `delete-and-extract-region` when copying into user-accessible
    data structures such as the kill-ring, X clipboard, and registers.
    Major and minor modes can modify `filter-buffer-substring-function`
    to alter such text as it is copied out of the buffer.

Variable: **filter-buffer-substring-function** [¶](#index-filter_002dbuffer_002dsubstring_002dfunction)
:   The value of this variable is a function that `filter-buffer-substring`
    will call to do the actual work. The function receives three
    arguments, the same as those of `filter-buffer-substring`,
    which it should treat as per the documentation of that function. It
    should return the filtered text (and optionally delete the source text).
    Note that it must process the start and end arguments
    regardless of the order of the buffer positions they specify, to comply
    with the protocol of extracting text for the purposes of killing it.

The following two variables are obsoleted by
`filter-buffer-substring-function`, but are still supported for
backward compatibility.

Variable: **filter-buffer-substring-functions** [¶](#index-filter_002dbuffer_002dsubstring_002dfunctions)
:   This obsolete variable is a wrapper hook, whose members should be functions
    that accept four arguments: fun, start, end, and
    delete. fun is a function that takes three arguments
    (start, end, and delete), and returns a string. In
    both cases, the start, end, and delete arguments are
    the same as those of `filter-buffer-substring`.

    The first hook function is passed a fun that is equivalent to
    the default operation of `filter-buffer-substring`, i.e., it
    returns the buffer-substring between start and end and
    optionally deletes the original text from the buffer. In most cases,
    the hook function will call fun once, and then do its own
    processing of the result. The next hook function receives a fun
    equivalent to this, and so on. The actual return value is the result
    of all the hook functions acting in sequence.

Function: **current-word** &optional strict really-word [¶](#index-current_002dword)
:   This function returns the symbol (or word) at or near point, as a
    string. The return value includes no text properties.

    If the optional argument really-word is non-`nil`, it finds a
    word; otherwise, it finds a symbol (which includes both word
    characters and symbol constituent characters).

    If the optional argument strict is non-`nil`, then point
    must be in or next to the symbol or word—if no symbol or word is
    there, the function returns `nil`. Otherwise, a nearby symbol or
    word on the same line is acceptable.

Function: **thing-at-point** thing &optional no-properties [¶](#index-thing_002dat_002dpoint)
:   Return the thing around or next to point, as a string.

    The argument thing is a symbol which specifies a kind of
    syntactic entity. Possibilities include `symbol`, `list`,
    `sexp`, `defun`, `filename`, `existing-filename`,
    `url`, `word`, `sentence`, `whitespace`,
    `line`, `page`, `string`, and others.

    When the optional argument no-properties is non-`nil`, this
    function strips text properties from the return value.

    ```
    ---------- Buffer: foo ----------
    Gentlemen may cry ``Pea∗ce! Peace!,''
    but there is no peace.
    ---------- Buffer: foo ----------

    (thing-at-point 'word)
         ⇒ "Peace"
    (thing-at-point 'line)
         ⇒ "Gentlemen may cry ``Peace! Peace!,''\n"
    (thing-at-point 'whitespace)
         ⇒ nil
    ```

    Variable: **thing-at-point-provider-alist** [¶](#index-thing_002dat_002dpoint_002dprovider_002dalist)
    :   This variable allows users and modes to tweak how
        `thing-at-point` works. It’s an association list of things
        and functions (called with zero parameters) to return that thing.
        Entries for thing will be evaluated in turn until a
        non-`nil` result is returned.

        For instance, a major mode could say:

        ```
        (setq-local thing-at-point-provider-alist
                    (append thing-at-point-provider-alist
                            '((url . my-mode--url-at-point))))
        ```

        If no providers have a non-`nil` return, the thing will be
        computed the standard way.

---

Next: [Inserting Text](#Insertion), Previous: [Examining Buffer Contents](#Buffer-Contents), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.3 Comparing Text [¶](#Comparing-Text-1)

This function lets you compare portions of the text in a buffer, without
copying them into strings first.

Function: **compare-buffer-substrings** buffer1 start1 end1 buffer2 start2 end2 [¶](#index-compare_002dbuffer_002dsubstrings)
:   This function lets you compare two substrings of the same buffer or two
    different buffers. The first three arguments specify one substring,
    giving a buffer (or a buffer name) and two positions within the
    buffer. The last three arguments specify the other substring in the
    same way. You can use `nil` for buffer1, buffer2, or
    both to stand for the current buffer.

    The value is negative if the first substring is less, positive if the
    first is greater, and zero if they are equal. The absolute value of
    the result is one plus the index of the first differing characters
    within the substrings.

    This function ignores case when comparing characters
    if `case-fold-search` is non-`nil`. It always ignores
    text properties.

    Suppose you have the text ‘`foobarbar haha!rara!`’ in the
    current buffer; then in this example the two substrings are ‘`rbar`’ and ‘`rara!`’. The value is 2 because the first substring is
    greater at the second character.

    ```
    (compare-buffer-substrings nil 6 11 nil 16 21)
         ⇒ 2
    ```

---

Next: [User-Level Insertion Commands](#Commands-for-Insertion), Previous: [Comparing Text](#Comparing-Text), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.4 Inserting Text [¶](#Inserting-Text)

*Insertion* means adding new text to a buffer. The inserted text
goes at point—between the character before point and the character
after point. Some insertion functions leave point before the inserted
text, while other functions leave it after. We call the former
insertion *after point* and the latter insertion *before point*.

Insertion moves markers located at positions after the insertion
point, so that they stay with the surrounding text (see [Markers](#Markers)).
When a marker points at the place of insertion, insertion may or may
not relocate the marker, depending on the marker’s insertion type
(see [Marker Insertion Types](#Marker-Insertion-Types)). Certain special functions such as
`insert-before-markers` relocate all such markers to point after
the inserted text, regardless of the markers’ insertion type.

Insertion functions signal an error if the current buffer is
read-only (see [Read-Only Buffers](#Read-Only-Buffers)) or if they insert within
read-only text (see [Properties with Special Meanings](#Special-Properties)).

These functions copy text characters from strings and buffers along
with their properties. The inserted characters have exactly the same
properties as the characters they were copied from. By contrast,
characters specified as separate arguments, not part of a string or
buffer, inherit their text properties from the neighboring text.

The insertion functions convert text from unibyte to multibyte in
order to insert in a multibyte buffer, and vice versa—if the text
comes from a string or from a buffer. However, they do not convert
unibyte character codes 128 through 255 to multibyte characters, not
even if the current buffer is a multibyte buffer. See [Converting Text Representations](#Converting-Representations).

Function: **insert** &rest args [¶](#index-insert)
:   This function inserts the strings and/or characters args into the
    current buffer, at point, moving point forward. In other words, it
    inserts the text before point. An error is signaled unless all
    args are either strings or characters. The value is `nil`.

Function: **insert-before-markers** &rest args [¶](#index-insert_002dbefore_002dmarkers)
:   This function inserts the strings and/or characters args into the
    current buffer, at point, moving point forward. An error is signaled
    unless all args are either strings or characters. The value is
    `nil`.

    This function is unlike the other insertion functions in that it
    relocates markers initially pointing at the insertion point, to point
    after the inserted text. If an overlay begins at the insertion point,
    the inserted text falls outside the overlay; if a nonempty overlay
    ends at the insertion point, the inserted text falls inside that
    overlay.

Command: **insert-char** character &optional count inherit [¶](#index-insert_002dchar)
:   This command inserts count instances of character into the
    current buffer before point. The argument count must be an
    integer, and character must be a character.

    If called interactively, this command prompts for character
    using its Unicode name or its code point. See [Inserting Text](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Inserting-Text) in The GNU Emacs Manual.

    This function does not convert unibyte character codes 128 through 255
    to multibyte characters, not even if the current buffer is a multibyte
    buffer. See [Converting Text Representations](#Converting-Representations).

    If inherit is non-`nil`, the inserted characters inherit
    sticky text properties from the two characters before and after the
    insertion point. See [Stickiness of Text Properties](#Sticky-Properties).

Function: **insert-buffer-substring** from-buffer-or-name &optional start end [¶](#index-insert_002dbuffer_002dsubstring)
:   This function inserts a portion of buffer from-buffer-or-name
    into the current buffer before point. The text inserted is the region
    between start (inclusive) and end (exclusive). (These
    arguments default to the beginning and end of the accessible portion
    of that buffer.) This function returns `nil`.

    In this example, the form is executed with buffer ‘`bar`’ as the
    current buffer. We assume that buffer ‘`bar`’ is initially empty.

    ```
    ---------- Buffer: foo ----------
    We hold these truths to be self-evident, that all
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (insert-buffer-substring "foo" 1 20)
         ⇒ nil

    ---------- Buffer: bar ----------
    We hold these truth∗
    ---------- Buffer: bar ----------
    ```

Function: **insert-buffer-substring-no-properties** from-buffer-or-name &optional start end [¶](#index-insert_002dbuffer_002dsubstring_002dno_002dproperties)
:   This is like `insert-buffer-substring` except that it does not
    copy any text properties.

Function: **insert-into-buffer** to-buffer &optional start end [¶](#index-insert_002dinto_002dbuffer)
:   This is like `insert-buffer-substring`, but works in the opposite
    direction: The text is copied from the current buffer into
    to-buffer. The block of text is copied to the current point in
    to-buffer, and point (in that buffer) is advanced to after the
    end of the copied text. Is `start`/`end` is `nil`, the
    entire text in the current buffer is copied over.

See [Stickiness of Text Properties](#Sticky-Properties), for other insertion functions that inherit
text properties from the nearby text in addition to inserting it.
Whitespace inserted by indentation functions also inherits text
properties.

---

Next: [Deleting Text](#Deletion), Previous: [Inserting Text](#Insertion), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.5 User-Level Insertion Commands [¶](#User_002dLevel-Insertion-Commands)

This section describes higher-level commands for inserting text,
commands intended primarily for the user but useful also in Lisp
programs.

Command: **insert-buffer** from-buffer-or-name [¶](#index-insert_002dbuffer)
:   This command inserts the entire accessible contents of
    from-buffer-or-name (which must exist) into the current buffer
    after point. It leaves the mark after the inserted text. The value
    is `nil`.

Command: **self-insert-command** count &optional char [¶](#index-self_002dinsert_002dcommand)
:   This command inserts the character char (the last character typed);
    it does so count times, before point, and returns `nil`.
    Most printing characters are bound to this command. In routine use,
    `self-insert-command` is the most frequently called function in Emacs,
    but programs rarely use it except to install it on a keymap.

    In an interactive call, count is the numeric prefix argument.

    Self-insertion translates the input character through
    `translation-table-for-input`. See [Translation of Characters](#Translation-of-Characters).

    This command calls `auto-fill-function` whenever that is
    non-`nil` and the character inserted is in the table
    `auto-fill-chars` (see [Auto Filling](#Auto-Filling)).

    This command performs abbrev expansion if Abbrev mode is enabled and
    the inserted character does not have word-constituent
    syntax. (See [Abbrevs and Abbrev Expansion](#Abbrevs), and [Table of Syntax Classes](#Syntax-Class-Table).) It is also
    responsible for calling `blink-paren-function` when the inserted
    character has close parenthesis syntax (see [Blinking Parentheses](#Blinking)).

    The final thing this command does is to run the hook
    `post-self-insert-hook`. You could use this to automatically
    reindent text as it is typed, for example. The functions on this hook
    can use `last-command-event` (see [Information from the Command Loop](#Command-Loop-Info)) to
    access the character just inserted.

    If any function on this hook needs to act on the region (see [The Region](#The-Region)), it should make sure Delete Selection mode (see [Delete Selection](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Using-Region) in The GNU Emacs Manual) doesn’t
    delete the region before `post-self-insert-hook` functions are
    invoked. The way to do so is to add a function that returns
    `nil` to `self-insert-uses-region-functions`, a special hook
    that tells Delete Selection mode it should not delete the region.

    Do not try substituting your own definition of
    `self-insert-command` for the standard one. The editor command
    loop handles this function specially.

Command: **newline** &optional number-of-newlines interactive [¶](#index-newline-1)
:   This command inserts newlines into the current buffer before point.
    If number-of-newlines is supplied, that many newline characters
    are inserted. In an interactive call, number-of-newlines is the
    numeric prefix argument.

    This command calls `self-insert-command` to insert newlines,
    which may subsequently break the preceding line by calling
    `auto-fill-function` (see [Auto Filling](#Auto-Filling)). Typically what
    `auto-fill-function` does is insert a newline; thus, the overall
    result in this case is to insert two newlines at different places: one
    at point, and another earlier in the line. `newline` does not
    auto-fill if number-of-newlines is non-`nil`.

    This command does not run the hook `post-self-insert-hook` unless
    called interactively or interactive is non-`nil`.

    This command indents to the left margin if that is not zero.
    See [Margins for Filling](#Margins).

    The value returned is `nil`.

Command: **ensure-empty-lines** &optional number-of-empty-lines [¶](#index-ensure_002dempty_002dlines)
:   This command can be used to ensure that you have a specific number of
    empty lines before point. (An “empty line” is here defined as a
    line with no characters on it—a line with space characters isn’t an
    empty line.) It defaults to ensuring that there’s a single empty line
    before point.

    If point isn’t at the beginning of a line, a newline character is
    inserted first. If there’s more empty lines before point than
    specified, the number of empty lines is reduced. Otherwise it’s
    increased to the specified number.

Variable: **overwrite-mode** [¶](#index-overwrite_002dmode)
:   This variable controls whether overwrite mode is in effect. The value
    should be `overwrite-mode-textual`, `overwrite-mode-binary`,
    or `nil`. `overwrite-mode-textual` specifies textual
    overwrite mode (treats newlines and tabs specially), and
    `overwrite-mode-binary` specifies binary overwrite mode (treats
    newlines and tabs like any other characters).

---

Next: [User-Level Deletion Commands](#User_002dLevel-Deletion), Previous: [User-Level Insertion Commands](#Commands-for-Insertion), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.6 Deleting Text [¶](#Deleting-Text)

Deletion means removing part of the text in a buffer, without saving
it in the kill ring (see [The Kill Ring](#The-Kill-Ring)). Deleted text can’t be
yanked, but can be reinserted using the undo mechanism (see [Undo](#Undo)).
Some deletion functions do save text in the kill ring in some special
cases.

All of the deletion functions operate on the current buffer.

Command: **erase-buffer** [¶](#index-erase_002dbuffer)
:   This function deletes the entire text of the current buffer
    (*not* just the accessible portion), leaving it
    empty. If the buffer is read-only, it signals a `buffer-read-only`
    error; if some of the text in it is read-only, it signals a
    `text-read-only` error. Otherwise, it deletes the text without
    asking for any confirmation. It returns `nil`.

    Normally, deleting a large amount of text from a buffer inhibits further
    auto-saving of that buffer because it has shrunk. However,
    `erase-buffer` does not do this, the idea being that the future
    text is not really related to the former text, and its size should not
    be compared with that of the former text.

Command: **delete-region** start end [¶](#index-delete_002dregion)
:   This command deletes the text between positions start and
    end in the current buffer, and returns `nil`. If point was
    inside the deleted region, its value afterward is start.
    Otherwise, point relocates with the surrounding text, as markers do.

Function: **delete-and-extract-region** start end [¶](#index-delete_002dand_002dextract_002dregion)
:   This function deletes the text between positions start and
    end in the current buffer, and returns a string containing the
    text just deleted.

    If point was inside the deleted region, its value afterward is
    start. Otherwise, point relocates with the surrounding text, as
    markers do.

Command: **delete-char** count &optional killp [¶](#index-delete_002dchar)
:   This command deletes count characters directly after point, or
    before point if count is negative. If killp is
    non-`nil`, then it saves the deleted characters in the kill ring.

    In an interactive call, count is the numeric prefix argument, and
    killp is the unprocessed prefix argument. Therefore, if a prefix
    argument is supplied, the text is saved in the kill ring. If no prefix
    argument is supplied, then one character is deleted, but not saved in
    the kill ring.

    The value returned is always `nil`.

Command: **delete-backward-char** count &optional killp [¶](#index-delete_002dbackward_002dchar)
:   This command deletes count characters directly before point, or
    after point if count is negative. If killp is
    non-`nil`, then it saves the deleted characters in the kill ring.

    In an interactive call, count is the numeric prefix argument, and
    killp is the unprocessed prefix argument. Therefore, if a prefix
    argument is supplied, the text is saved in the kill ring. If no prefix
    argument is supplied, then one character is deleted, but not saved in
    the kill ring.

    The value returned is always `nil`.

Command: **backward-delete-char-untabify** count &optional killp [¶](#index-backward_002ddelete_002dchar_002duntabify)
:   This command deletes count characters backward, changing tabs
    into spaces. When the next character to be deleted is a tab, it is
    first replaced with the proper number of spaces to preserve alignment
    and then one of those spaces is deleted instead of the tab. If
    killp is non-`nil`, then the command saves the deleted
    characters in the kill ring.

    Conversion of tabs to spaces happens only if count is positive.
    If it is negative, exactly −count characters after point
    are deleted.

    In an interactive call, count is the numeric prefix argument, and
    killp is the unprocessed prefix argument. Therefore, if a prefix
    argument is supplied, the text is saved in the kill ring. If no prefix
    argument is supplied, then one character is deleted, but not saved in
    the kill ring.

    The value returned is always `nil`.

User Option: **backward-delete-char-untabify-method** [¶](#index-backward_002ddelete_002dchar_002duntabify_002dmethod)
:   This option specifies how `backward-delete-char-untabify` should
    deal with whitespace. Possible values include `untabify`, the
    default, meaning convert a tab to many spaces and delete one;
    `hungry`, meaning delete all tabs and spaces before point with
    one command; `all` meaning delete all tabs, spaces and newlines
    before point, and `nil`, meaning do nothing special for
    whitespace characters.

---

Next: [The Kill Ring](#The-Kill-Ring), Previous: [Deleting Text](#Deletion), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.7 User-Level Deletion Commands [¶](#User_002dLevel-Deletion-Commands)

This section describes higher-level commands for deleting text,
commands intended primarily for the user but useful also in Lisp
programs.

Command: **delete-horizontal-space** &optional backward-only [¶](#index-delete_002dhorizontal_002dspace)
:   This function deletes all spaces and tabs around point. It returns
    `nil`.

    If backward-only is non-`nil`, the function deletes
    spaces and tabs before point, but not after point.

    In the following examples, we call `delete-horizontal-space` four
    times, once on each line, with point between the second and third
    characters on the line each time.

    ```
    ---------- Buffer: foo ----------
    I ∗thought
    I ∗     thought
    We∗ thought
    Yo∗u thought
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (delete-horizontal-space)   ; Four times.
         ⇒ nil

    ---------- Buffer: foo ----------
    Ithought
    Ithought
    Wethought
    You thought
    ---------- Buffer: foo ----------
    ```

Command: **delete-indentation** &optional join-following-p beg end [¶](#index-delete_002dindentation)
:   This function joins the line point is on to the previous line, deleting
    any whitespace at the join and in some cases replacing it with one
    space. If join-following-p is non-`nil`,
    `delete-indentation` joins this line to the following line
    instead. Otherwise, if beg and end are non-`nil`,
    this function joins all lines in the region they define.

    In an interactive call, join-following-p is the prefix argument,
    and beg and end are, respectively, the start and end of
    the region if it is active, else `nil`. The function returns
    `nil`.

    If there is a fill prefix, and the second of the lines being joined
    starts with the prefix, then `delete-indentation` deletes the
    fill prefix before joining the lines. See [Margins for Filling](#Margins).

    In the example below, point is located on the line starting
    ‘`events`’, and it makes no difference if there are trailing spaces
    in the preceding line.

    ```
    ---------- Buffer: foo ----------
    When in the course of human
    ∗    events, it becomes necessary
    ---------- Buffer: foo ----------
    ```

    ```
    (delete-indentation)
         ⇒ nil
    ```

    ```
    ---------- Buffer: foo ----------
    When in the course of human∗ events, it becomes necessary
    ---------- Buffer: foo ----------
    ```

    After the lines are joined, the function `fixup-whitespace` is
    responsible for deciding whether to leave a space at the junction.

Command: **fixup-whitespace** [¶](#index-fixup_002dwhitespace)
:   This function replaces all the horizontal whitespace surrounding point
    with either one space or no space, according to the context. It
    returns `nil`.

    At the beginning or end of a line, the appropriate amount of space is
    none. Before a character with close parenthesis syntax, or after a
    character with open parenthesis or expression-prefix syntax, no space is
    also appropriate. Otherwise, one space is appropriate. See [Table of Syntax Classes](#Syntax-Class-Table).

    In the example below, `fixup-whitespace` is called the first time
    with point before the word ‘`spaces`’ in the first line. For the
    second invocation, point is directly after the ‘`(`’.

    ```
    ---------- Buffer: foo ----------
    This has too many     ∗spaces
    This has too many spaces at the start of (∗   this list)
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (fixup-whitespace)
         ⇒ nil
    (fixup-whitespace)
         ⇒ nil
    ```

    ```

    ```

    ```
    ---------- Buffer: foo ----------
    This has too many spaces
    This has too many spaces at the start of (this list)
    ---------- Buffer: foo ----------
    ```

Command: **just-one-space** &optional n [¶](#index-just_002done_002dspace)
:   This command replaces any spaces and tabs around point with a single
    space, or n spaces if n is specified. It returns
    `nil`.

Command: **delete-blank-lines** [¶](#index-delete_002dblank_002dlines)
:   This function deletes blank lines surrounding point. If point is on a
    blank line with one or more blank lines before or after it, then all but
    one of them are deleted. If point is on an isolated blank line, then it
    is deleted. If point is on a nonblank line, the command deletes all
    blank lines immediately following it.

    A blank line is defined as a line containing only tabs and spaces.

    `delete-blank-lines` returns `nil`.

Command: **delete-trailing-whitespace** &optional start end [¶](#index-delete_002dtrailing_002dwhitespace)
:   Delete trailing whitespace in the region defined by start and
    end.

    This command deletes whitespace characters after the last
    non-whitespace character in each line in the region.

    If this command acts on the entire buffer (i.e., if called
    interactively with the mark inactive, or called from Lisp with
    end `nil`), it also deletes all trailing lines at the end of the
    buffer if the variable `delete-trailing-lines` is non-`nil`.

---

Next: [Undo](#Undo), Previous: [User-Level Deletion Commands](#User_002dLevel-Deletion), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.8 The Kill Ring [¶](#The-Kill-Ring-1)

*Kill functions* delete text like the deletion functions, but save
it so that the user can reinsert it by *yanking*. Most of these
functions have ‘`kill-`’ in their name. By contrast, the functions
whose names start with ‘`delete-`’ normally do not save text for
yanking (though they can still be undone); these are deletion
functions.

Most of the kill commands are primarily for interactive use, and are
not described here. What we do describe are the functions provided for
use in writing such commands. You can use these functions to write
commands for killing text. When you need to delete text for internal
purposes within a Lisp function, you should normally use deletion
functions, so as not to disturb the kill ring contents.
See [Deleting Text](#Deletion).

Killed text is saved for later yanking in the *kill ring*. This
is a list that holds a number of recent kills, not just the last text
kill. We call this a “ring” because yanking treats it as having
elements in a cyclic order. The list is kept in the variable
`kill-ring`, and can be operated on with the usual functions for
lists; there are also specialized functions, described in this section,
that treat it as a ring.

Some people think this use of the word “kill” is unfortunate, since
it refers to operations that specifically *do not* destroy the
entities killed. This is in sharp contrast to ordinary life, in
which death is permanent and killed entities do not come back to
life. Therefore, other metaphors have been proposed. For example, the
term “cut ring” makes sense to people who, in pre-computer days, used
scissors and paste to cut up and rearrange manuscripts. However, it
would be difficult to change the terminology now.

* [Kill Ring Concepts](#Kill-Ring-Concepts)
* [Functions for Killing](#Kill-Functions)
* [Yanking](#Yanking)
* [Functions for Yanking](#Yank-Commands)
* [Low-Level Kill Ring](#Low_002dLevel-Kill-Ring)
* [Internals of the Kill Ring](#Internals-of-Kill-Ring)

---

Next: [Functions for Killing](#Kill-Functions), Up: [The Kill Ring](#The-Kill-Ring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.8.1 Kill Ring Concepts [¶](#Kill-Ring-Concepts-1)

The kill ring records killed text as strings in a list, most recent
first. A short kill ring, for example, might look like this:

```
("some text" "a different piece of text" "even older text")
```

When the list reaches `kill-ring-max` entries in length, adding a
new entry automatically deletes the last entry.

When kill commands are interwoven with other commands, each kill
command makes a new entry in the kill ring. Multiple kill commands in
succession build up a single kill ring entry, which would be yanked as a
unit; the second and subsequent consecutive kill commands add text to
the entry made by the first one.

For yanking, one entry in the kill ring is designated the front of
the ring. Some yank commands rotate the ring by designating a
different element as the front. But this virtual rotation doesn’t
change the list itself—the most recent entry always comes first in the
list.

---

Next: [Yanking](#Yanking), Previous: [Kill Ring Concepts](#Kill-Ring-Concepts), Up: [The Kill Ring](#The-Kill-Ring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.8.2 Functions for Killing [¶](#Functions-for-Killing)

`kill-region` is the usual subroutine for killing text. Any
command that calls this function is a kill command (and should
probably have ‘`kill`’ in its name). `kill-region` puts the
newly killed text in a new element at the beginning of the kill ring or
adds it to the most recent element. It determines automatically (using
`last-command`) whether the previous command was a kill command,
and if so appends the killed text to the most recent entry.

The commands described below can filter the killed text before they
save it in the kill ring. They call `filter-buffer-substring`
(see [Examining Buffer Contents](#Buffer-Contents)) to perform the filtering. By default,
there’s no filtering, but major and minor modes and hook functions can
set up filtering, so that text saved in the kill ring is different
from what was in the buffer.

Command: **kill-region** start end &optional region [¶](#index-kill_002dregion)
:   This function kills the stretch of text between start and
    end; but if the optional argument region is
    non-`nil`, it ignores start and end, and kills the
    text in the current region instead. The text is deleted but saved in
    the kill ring, along with its text properties. The value is always
    `nil`.

    In an interactive call, start and end are point and
    the mark, and region is always non-`nil`, so the command
    always kills the text in the current region.

    If the buffer or text is read-only, `kill-region` modifies the kill
    ring just the same, then signals an error without modifying the buffer.
    This is convenient because it lets the user use a series of kill
    commands to copy text from a read-only buffer into the kill ring.

User Option: **kill-read-only-ok** [¶](#index-kill_002dread_002donly_002dok)
:   If this option is non-`nil`, `kill-region` does not signal an
    error if the buffer or text is read-only. Instead, it simply returns,
    updating the kill ring but not changing the buffer.

Command: **copy-region-as-kill** start end &optional region [¶](#index-copy_002dregion_002das_002dkill)
:   This function saves the stretch of text between start and
    end on the kill ring (including text properties), but does not
    delete the text from the buffer. However, if the optional argument
    region is non-`nil`, the function ignores start and
    end, and saves the current region instead. It always returns
    `nil`.

    In an interactive call, start and end are point and
    the mark, and region is always non-`nil`, so the command
    always saves the text in the current region.

    The command does not set `this-command` to `kill-region`, so a
    subsequent kill command does not append to the same kill ring entry.

---

Next: [Functions for Yanking](#Yank-Commands), Previous: [Functions for Killing](#Kill-Functions), Up: [The Kill Ring](#The-Kill-Ring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.8.3 Yanking [¶](#Yanking-1)

Yanking means inserting text from the kill ring, but it does not
insert the text blindly. The `yank` command, and related
commands, use `insert-for-yank` to perform special processing on
the text before it is inserted.

Function: **insert-for-yank** string [¶](#index-insert_002dfor_002dyank)
:   This function works like `insert`, except that it processes the
    text in string according to the `yank-handler` text
    property, as well as the variables `yank-handled-properties` and
    `yank-excluded-properties` (see below), before inserting the
    result into the current buffer.

    string will be run through `yank-transform-functions` (see
    below) before inserting.

Function: **insert-buffer-substring-as-yank** buf &optional start end [¶](#index-insert_002dbuffer_002dsubstring_002das_002dyank)
:   This function resembles `insert-buffer-substring`, except that it
    processes the text according to `yank-handled-properties` and
    `yank-excluded-properties`. (It does not handle the
    `yank-handler` property, which does not normally occur in buffer
    text anyway.)

If you put a `yank-handler` text property on all or part of a
string, that alters how `insert-for-yank` inserts the string. If
different parts of the string have different `yank-handler`
values (comparison being done with `eq`), each substring is
handled separately. The property value must be a list of one to four
elements, with the following format (where elements after the first
may be omitted):

```
(function param noexclude undo)
```

Here is what the elements do:

function
:   When function is non-`nil`, it is called instead of
    `insert` to insert the string, with one argument—the string to
    insert.

param
:   If param is present and non-`nil`, it replaces string
    (or the substring of string being processed) as the object
    passed to function (or `insert`). For example, if
    function is `yank-rectangle`, param should be a list
    of strings to insert as a rectangle.

noexclude
:   If noexclude is present and non-`nil`, that disables the
    normal action of `yank-handled-properties` and
    `yank-excluded-properties` on the inserted string.

undo
:   If undo is present and non-`nil`, it is a function that will be
    called by `yank-pop` to undo the insertion of the current object.
    It is called with two arguments, the start and end of the current
    region. function can set `yank-undo-function` to override
    the undo value.

User Option: **yank-handled-properties** [¶](#index-yank_002dhandled_002dproperties)
:   This variable specifies special text property handling conditions for
    yanked text. It takes effect after the text has been inserted (either
    normally, or via the `yank-handler` property), and prior to
    `yank-excluded-properties` taking effect.

    The value should be an alist of elements `(prop
    . fun)`. Each alist element is handled in order. The inserted
    text is scanned for stretches of text having text properties `eq`
    to prop; for each such stretch, fun is called with three
    arguments: the value of the property, and the start and end positions
    of the text.

User Option: **yank-excluded-properties** [¶](#index-yank_002dexcluded_002dproperties)
:   The value of this variable is the list of properties to remove from
    inserted text. Its default value contains properties that might lead
    to annoying results, such as causing the text to respond to the mouse
    or specifying key bindings. It takes effect after
    `yank-handled-properties`.

Variable: **yank-transform-functions** [¶](#index-yank_002dtransform_002dfunctions)
:   This variable is a list of functions. Each function is called (in
    order) with the string to be yanked as the argument, and should
    return a (possibly transformed) string. This variable can be set
    globally, but can also be used to create new commands that are
    variations on `yank`. For instance, to create a command that
    works like `yank`, but cleans up whitespace before inserting, you
    could say something like:

    ```
    (defun yank-with-clean-whitespace ()
      (interactive)
      (let ((yank-transform-functions
    	 '(string-clean-whitespace)))
        (call-interactively #'yank)))
    ```

---

Next: [Low-Level Kill Ring](#Low_002dLevel-Kill-Ring), Previous: [Yanking](#Yanking), Up: [The Kill Ring](#The-Kill-Ring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.8.4 Functions for Yanking [¶](#Functions-for-Yanking)

This section describes higher-level commands for yanking, which are
intended primarily for the user but useful also in Lisp programs.
Both `yank` and `yank-pop` honor the
`yank-excluded-properties` variable and `yank-handler` text
property (see [Yanking](#Yanking)).

Command: **yank** &optional arg [¶](#index-yank)
:   This command inserts before point the text at the front of the kill
    ring. It sets the mark at the beginning of that text, using
    `push-mark` (see [The Mark](#The-Mark)), and puts point at the end.

    If arg is a non-`nil` list (which occurs interactively when
    the user types `C-u` with no digits), then `yank` inserts the
    text as described above, but puts point before the yanked text and
    sets the mark after it.

    If arg is a number, then `yank` inserts the argth
    most recently killed text—the argth element of the kill ring
    list, counted cyclically from the front, which is considered the
    first element for this purpose.

    `yank` does not alter the contents of the kill ring, unless it
    used text provided by another program, in which case it pushes that text
    onto the kill ring. However if arg is an integer different from
    one, it rotates the kill ring to place the yanked string at the front.

    `yank` returns `nil`.

Command: **yank-pop** &optional arg [¶](#index-yank_002dpop)
:   When invoked immediately after a `yank` or another
    `yank-pop`, this command replaces the just-yanked entry from the
    kill ring with a different entry from the kill ring. When this
    command is invoked like that, the region contains text that was just
    inserted by another yank command. `yank-pop` deletes that text
    and inserts in its place a different piece of killed text. It does
    not add the deleted text to the kill ring, since it is already in the
    kill ring somewhere. It does however rotate the kill ring to place
    the newly yanked string at the front.

    If arg is `nil`, then the replacement text is the previous
    element of the kill ring. If arg is numeric, the replacement is
    the argth previous kill. If arg is negative, a more recent
    kill is the replacement.

    The sequence of kills in the kill ring wraps around, so if
    `yank-pop` is invoked repeatedly and reaches the oldest kill, the
    one that comes after it is the newest one, and the one before the
    newest one is the oldest one.

    This command can also be invoked after a command that is not a yank
    command. In that case, it prompts in the minibuffer for a kill-ring
    entry, with completion, and uses the kill ring elements as the
    minibuffer history (see [Minibuffer History](#Minibuffer-History)). This allows the user
    to interactively select one of the previous kills recorded in the kill
    ring.

    The return value is always `nil`.

Variable: **yank-undo-function** [¶](#index-yank_002dundo_002dfunction)
:   If this variable is non-`nil`, the function `yank-pop` uses
    its value instead of `delete-region` to delete the text
    inserted by the previous `yank` or
    `yank-pop` command. The value must be a function of two
    arguments, the start and end of the current region.

    The function `insert-for-yank` automatically sets this variable
    according to the undo element of the `yank-handler`
    text property, if there is one.

---

Next: [Internals of the Kill Ring](#Internals-of-Kill-Ring), Previous: [Functions for Yanking](#Yank-Commands), Up: [The Kill Ring](#The-Kill-Ring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.8.5 Low-Level Kill Ring [¶](#Low_002dLevel-Kill-Ring-1)

These functions and variables provide access to the kill ring at a
lower level, but are still convenient for use in Lisp programs,
because they take care of interaction with window system selections
(see [Window System Selections](#Window-System-Selections)).

Function: **current-kill** n &optional do-not-move [¶](#index-current_002dkill)
:   The function `current-kill` rotates the yanking pointer, which
    designates the front of the kill ring, by n places (from newer
    kills to older ones), and returns the text at that place in the ring.

    If the optional second argument do-not-move is non-`nil`,
    then `current-kill` doesn’t alter the yanking pointer; it just
    returns the nth kill, counting from the current yanking pointer.

    If n is zero, indicating a request for the latest kill,
    `current-kill` calls the value of
    `interprogram-paste-function` (documented below) before
    consulting the kill ring. If that value is a function and calling it
    returns a string or a list of several strings, `current-kill`
    pushes the strings onto the kill ring and returns the first string.
    It also sets the yanking pointer to point to the kill-ring entry of
    the first string returned by `interprogram-paste-function`,
    regardless of the value of do-not-move. Otherwise,
    `current-kill` does not treat a zero value for n specially:
    it returns the entry pointed at by the yanking pointer and does not
    move the yanking pointer.

Function: **kill-new** string &optional replace [¶](#index-kill_002dnew)
:   This function pushes the text string onto the kill ring and
    makes the yanking pointer point to it. It discards the oldest entry
    if appropriate. It also invokes the values of
    `interprogram-paste-function` (subject to
    the user option `save-interprogram-paste-before-kill`)
    and `interprogram-cut-function` (see below).

    If replace is non-`nil`, then `kill-new` replaces the
    first element of the kill ring with string, rather than pushing
    string onto the kill ring.

Function: **kill-append** string before-p [¶](#index-kill_002dappend)
:   This function appends the text string to the first entry in the
    kill ring and makes the yanking pointer point to the combined entry.
    Normally string goes at the end of the entry, but if
    before-p is non-`nil`, it goes at the beginning. This
    function calls `kill-new` as a subroutine, thus causing the
    values of `interprogram-cut-function` and possibly
    `interprogram-paste-function` (see below) to be invoked by
    extension.

Variable: **interprogram-paste-function** [¶](#index-interprogram_002dpaste_002dfunction)
:   This variable provides a way of transferring killed text from other
    programs, when you are using a window system. Its value should be
    `nil` or a function of no arguments.

    If the value is a function, `current-kill` calls it to get the
    most recent kill. If the function returns a non-`nil` value,
    then that value is used as the most recent kill. If it returns
    `nil`, then the front of the kill ring is used.

    To facilitate support for window systems that support multiple
    selections, this function may also return a list of strings. In that
    case, the first string is used as the most recent kill, and all
    the other strings are pushed onto the kill ring, for easy access by
    `yank-pop`.

    The normal use of this function is to get the window system’s
    clipboard as the most recent kill, even if the selection belongs to
    another application. See [Window System Selections](#Window-System-Selections). However, if
    the clipboard contents come from the current Emacs session, this
    function should return `nil`.

Variable: **interprogram-cut-function** [¶](#index-interprogram_002dcut_002dfunction)
:   This variable provides a way of communicating killed text to other
    programs, when you are using a window system. Its value should be
    `nil` or a function of one required argument.

    If the value is a function, `kill-new` and `kill-append` call
    it with the new first element of the kill ring as the argument.

    The normal use of this function is to put newly killed text in the
    window system’s clipboard. See [Window System Selections](#Window-System-Selections).

---

Previous: [Low-Level Kill Ring](#Low_002dLevel-Kill-Ring), Up: [The Kill Ring](#The-Kill-Ring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.8.6 Internals of the Kill Ring [¶](#Internals-of-the-Kill-Ring)

The variable `kill-ring` holds the kill ring contents, in the
form of a list of strings. The most recent kill is always at the front
of the list.

The `kill-ring-yank-pointer` variable points to a link in the
kill ring list, whose CAR is the text to yank next. We say it
identifies the front of the ring. Moving
`kill-ring-yank-pointer` to a different link is called
*rotating the kill ring*. We call the kill ring a “ring” because
the functions that move the yank pointer wrap around from the end of the
list to the beginning, or vice-versa. Rotation of the kill ring is
virtual; it does not change the value of `kill-ring`.

Both `kill-ring` and `kill-ring-yank-pointer` are Lisp
variables whose values are normally lists. The word “pointer” in the
name of the `kill-ring-yank-pointer` indicates that the variable’s
purpose is to identify one element of the list for use by the next yank
command.

The value of `kill-ring-yank-pointer` is always `eq` to one
of the links in the kill ring list. The element it identifies is the
CAR of that link. Kill commands, which change the kill ring, also
set this variable to the value of `kill-ring`. The effect is to
rotate the ring so that the newly killed text is at the front.

Here is a diagram that shows the variable `kill-ring-yank-pointer`
pointing to the second entry in the kill ring `("some text" "a
different piece of text" "yet older text")`.

```
kill-ring                  ---- kill-ring-yank-pointer
  |                       |
  |                       v
  |     --- ---          --- ---      --- ---
   --> |   |   |------> |   |   |--> |   |   |--> nil
        --- ---          --- ---      --- ---
         |                |            |
         |                |            |
         |                |             -->"yet older text"
         |                |
         |                 --> "a different piece of text"
         |
          --> "some text"
```

This state of affairs might occur after `C-y` (`yank`)
immediately followed by `M-y` (`yank-pop`).

Variable: **kill-ring** [¶](#index-kill_002dring)
:   This variable holds the list of killed text sequences, most recently
    killed first.

Variable: **kill-ring-yank-pointer** [¶](#index-kill_002dring_002dyank_002dpointer)
:   This variable’s value indicates which element of the kill ring is at the
    front of the ring for yanking. More precisely, the value is a tail
    of the value of `kill-ring`, and its CAR is the kill string
    that `C-y` should yank.

User Option: **kill-ring-max** [¶](#index-kill_002dring_002dmax)
:   The value of this variable is the maximum length to which the kill
    ring can grow, before elements are thrown away at the end. The default
    value for `kill-ring-max` is 120.

---

Next: [Maintaining Undo Lists](#Maintaining-Undo), Previous: [The Kill Ring](#The-Kill-Ring), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.9 Undo [¶](#Undo-1)

Most buffers have an *undo list*, which records all changes made
to the buffer’s text so that they can be undone. (The buffers that
don’t have one are usually special-purpose buffers for which Emacs
assumes that undoing is not useful. In particular, any buffer whose
name begins with a space has its undo recording off by default;
see [Buffer Names](#Buffer-Names).) All the primitives that modify the
text in the buffer automatically add elements to the front of the undo
list, which is in the variable `buffer-undo-list`.

Variable: **buffer-undo-list** [¶](#index-buffer_002dundo_002dlist)
:   This buffer-local variable’s value is the undo list of the current
    buffer. A value of `t` disables the recording of undo information.

Here are the kinds of elements an undo list can have:

`position`
:   This kind of element records a previous value of point; undoing this
    element moves point to position. Ordinary cursor motion does not
    make any sort of undo record, but deletion operations use these entries
    to record where point was before the command if re-inserting the deleted
    text by undoing the deletion will not already place point at the right
    position. Lisp programs that need to ensure correct restoration of
    point by undoing a command can explicitly push `(point)` to
    `buffer-undo-list` to record the position to restore.

`(beg . end)`
:   This kind of element indicates how to delete text that was inserted.
    Upon insertion, the text occupied the range beg–end in the
    buffer.

`(text . position)`
:   This kind of element indicates how to reinsert text that was deleted.
    The deleted text itself is the string text. The place to
    reinsert it is `(abs position)`. If position is
    positive, point was at the beginning of the deleted text, otherwise it
    was at the end. Zero or more (marker . adjustment)
    elements follow immediately after this element.

`(t . time-flag)`
:   This kind of element indicates that an unmodified buffer became
    modified. A time-flag that is a non-integer Lisp timestamp
    represents the visited file’s modification time as of
    when it was previously visited or saved, using the same format as
    `current-time`; see [Time of Day](#Time-of-Day).
    A time-flag of 0 means the buffer does not correspond to any file;
    −1 means the visited file previously did not exist.
    `primitive-undo` uses these
    values to determine whether to mark the buffer as unmodified once again;
    it does so only if the file’s status matches that of time-flag.

`(nil property value beg . end)`
:   This kind of element records a change in a text property.
    Here’s how you might undo the change:

    ```
    (put-text-property beg end property value)
    ```

`(marker . adjustment)`
:   This kind of element records the fact that the marker marker was
    relocated due to deletion of surrounding text, and that it moved
    adjustment character positions. If the marker’s location is
    consistent with the (text . position) element preceding it
    in the undo list, then undoing this element moves marker
    − adjustment characters.

`(apply funname . args)`
:   This is an extensible undo item, which is undone by calling
    funname with arguments args.

`(apply delta beg end funname . args)`
:   This is an extensible undo item, which records a change limited to the
    range beg to end, which increased the size of the buffer
    by delta characters. It is undone by calling funname with
    arguments args.

    This kind of element enables undo limited to a region to determine
    whether the element pertains to that region.

`nil`
:   This element is a boundary. The elements between two boundaries are
    called a *change group*; normally, each change group corresponds to
    one keyboard command, and undo commands normally undo an entire group as
    a unit.

Function: **undo-boundary** [¶](#index-undo_002dboundary)
:   This function places a boundary element in the undo list. The undo
    command stops at such a boundary, and successive undo commands undo
    to earlier and earlier boundaries. This function returns `nil`.

    Calling this function explicitly is useful for splitting the effects of
    a command into more than one unit. For example, `query-replace`
    calls `undo-boundary` after each replacement, so that the user can
    undo individual replacements one by one.

    Mostly, however, this function is called automatically at an
    appropriate time.

Function: **undo-auto-amalgamate** [¶](#index-undo_002dauto_002damalgamate)
:   The editor command loop automatically calls `undo-boundary` just
    before executing each key sequence, so that each undo normally undoes
    the effects of one command. A few exceptional commands are
    *amalgamating*: these commands generally cause small changes to
    buffers, so with these a boundary is inserted only every 20th command,
    allowing the changes to be undone as a group. By default, the commands
    `self-insert-command`, which produces self-inserting input
    characters (see [User-Level Insertion Commands](#Commands-for-Insertion)), and `delete-char`,
    which deletes characters (see [Deleting Text](#Deletion)), are amalgamating.
    Where a command affects the contents of several buffers, as may happen,
    for example, when a function on the `post-command-hook` affects a
    buffer other than the `current-buffer`, then `undo-boundary`
    will be called in each of the affected buffers.

    This function can be called before an amalgamating command. It
    removes the previous `undo-boundary` if a series of such calls
    have been made.

    The maximum number of changes that can be amalgamated is controlled by
    the `amalgamating-undo-limit` variable. If this variable is 1,
    no changes are amalgamated.

A Lisp program can amalgamate a series of changes into a single change
group by calling `undo-amalgamate-change-group` (see [Atomic Change Groups](#Atomic-Changes)). Note that `amalgamating-undo-limit` has no effect on
the groups produced by that function.

Variable: **undo-auto-current-boundary-timer** [¶](#index-undo_002dauto_002dcurrent_002dboundary_002dtimer)
:   Some buffers, such as process buffers, can change even when no
    commands are executing. In these cases, `undo-boundary` is
    normally called periodically by the timer in this variable. Setting
    this variable to non-`nil` prevents this behavior.

Variable: **undo-in-progress** [¶](#index-undo_002din_002dprogress)
:   This variable is normally `nil`, but the undo commands bind it to
    `t`. This is so that various kinds of change hooks can tell when
    they’re being called for the sake of undoing.

Function: **primitive-undo** count list [¶](#index-primitive_002dundo)
:   This is the basic function for undoing elements of an undo list.
    It undoes the first count elements of list, returning
    the rest of list.

    `primitive-undo` adds elements to the buffer’s undo list when it
    changes the buffer. Undo commands avoid confusion by saving the undo
    list value at the beginning of a sequence of undo operations. Then the
    undo operations use and update the saved value. The new elements added
    by undoing are not part of this saved value, so they don’t interfere with
    continuing to undo.

    This function does not bind `undo-in-progress`.

Macro: **with-undo-amalgamate** body… [¶](#index-with_002dundo_002damalgamate)
:   This macro removes all the undo boundaries inserted during the
    execution of body so that it can be undone as a single step.

Some commands leave the region active after execution in such a way that
it interferes with selective undo of that command. To make `undo`
ignore the active region when invoked immediately after such a command,
set the property `undo-inhibit-region` of the command’s function
symbol to a non-`nil` value. See [Standard Symbol Properties](#Standard-Properties).

---

Next: [Filling](#Filling), Previous: [Undo](#Undo), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.10 Maintaining Undo Lists [¶](#Maintaining-Undo-Lists)

This section describes how to enable and disable undo information for
a given buffer. It also explains how the undo list is truncated
automatically so it doesn’t get too big.

Recording of undo information in a newly created buffer is normally
enabled to start with; but if the buffer name starts with a space, the
undo recording is initially disabled. You can explicitly enable or
disable undo recording with the following two functions, or by setting
`buffer-undo-list` yourself.

Command: **buffer-enable-undo** &optional buffer-or-name [¶](#index-buffer_002denable_002dundo)
:   This command enables recording undo information for buffer
    buffer-or-name, so that subsequent changes can be undone. If no
    argument is supplied, then the current buffer is used. This function
    does nothing if undo recording is already enabled in the buffer. It
    returns `nil`.

    In an interactive call, buffer-or-name is the current buffer.
    You cannot specify any other buffer.

Command: **buffer-disable-undo** &optional buffer-or-name [¶](#index-buffer_002ddisable_002dundo)
:   This function discards the undo list of buffer-or-name, and disables
    further recording of undo information. As a result, it is no longer
    possible to undo either previous changes or any subsequent changes. If
    the undo list of buffer-or-name is already disabled, this function
    has no effect.

    In an interactive call, BUFFER-OR-NAME is the current buffer. You
    cannot specify any other buffer. This function returns `nil`.

As editing continues, undo lists get longer and longer. To prevent
them from using up all available memory space, garbage collection trims
them back to size limits you can set. (For this purpose, the size
of an undo list measures the cons cells that make up the list, plus the
strings of deleted text.) Three variables control the range of acceptable
sizes: `undo-limit`, `undo-strong-limit` and
`undo-outer-limit`. In these variables, size is counted as the
number of bytes occupied, which includes both saved text and other
data.

User Option: **undo-limit** [¶](#index-undo_002dlimit)
:   This is the soft limit for the acceptable size of an undo list. The
    change group at which this size is exceeded is the last one kept.

User Option: **undo-strong-limit** [¶](#index-undo_002dstrong_002dlimit)
:   This is the upper limit for the acceptable size of an undo list. The
    change group at which this size is exceeded is discarded itself (along
    with all older change groups). There is one exception: the very latest
    change group is only discarded if it exceeds `undo-outer-limit`.

User Option: **undo-outer-limit** [¶](#index-undo_002douter_002dlimit)
:   If at garbage collection time the undo info for the current command
    exceeds this limit, Emacs discards the info and displays a warning.
    This is a last ditch limit to prevent memory overflow.

User Option: **undo-ask-before-discard** [¶](#index-undo_002dask_002dbefore_002ddiscard)
:   If this variable is non-`nil`, when the undo info exceeds
    `undo-outer-limit`, Emacs asks in the echo area whether to
    discard the info. The default value is `nil`, which means to
    discard it automatically.

    This option is mainly intended for debugging. Garbage collection is
    inhibited while the question is asked, which means that Emacs might
    leak memory if the user waits too long before answering the question.

---

Next: [Margins for Filling](#Margins), Previous: [Maintaining Undo Lists](#Maintaining-Undo), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.11 Filling [¶](#Filling-1)

*Filling* means adjusting the lengths of lines (by moving the line
breaks) so that they are nearly (but no greater than) a specified
maximum width. Additionally, lines can be *justified*, which means
inserting spaces to make the left and/or right margins line up
precisely. The width is controlled by the variable `fill-column`.
For ease of reading, lines should be no longer than 70 or so columns.

You can use Auto Fill mode (see [Auto Filling](#Auto-Filling)) to fill text
automatically as you insert it, but changes to existing text may leave
it improperly filled. Then you must fill the text explicitly.

Most of the commands in this section return values that are not
meaningful. All the functions that do filling take note of the current
left margin, current right margin, and current justification style
(see [Margins for Filling](#Margins)). If the current justification style is
`none`, the filling functions don’t actually do anything.

Several of the filling functions have an argument justify.
If it is non-`nil`, that requests some kind of justification. It
can be `left`, `right`, `full`, or `center`, to
request a specific style of justification. If it is `t`, that
means to use the current justification style for this part of the text
(see `current-justification`, below). Any other value is treated
as `full`.

When you call the filling functions interactively, using a prefix
argument implies the value `full` for justify.

Command: **fill-paragraph** &optional justify region [¶](#index-fill_002dparagraph)
:   This command fills the paragraph at or after point. If
    justify is non-`nil`, each line is justified as well.
    It uses the ordinary paragraph motion commands to find paragraph
    boundaries. See [Paragraphs](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Paragraphs) in The GNU Emacs Manual.

    When region is non-`nil`, then if Transient Mark mode is
    enabled and the mark is active, this command calls `fill-region`
    to fill all the paragraphs in the region, instead of filling only the
    current paragraph. When this command is called interactively,
    region is `t`.

Command: **unfill-paragraph** arg &optional beg end [¶](#index-unfill_002dparagraph)
:   This command is the inverse of the previous one: it joins up the lines
    of the current paragraph. With a numeric prefix argument arg, it
    applies the operation to that many paragraphs. With no prefix argument
    and an active region, unfill each paragraph in that region. When called
    from Lisp with arg `nil` and beg and end
    non-`nil`, unfill paragraphs in the region delimited by beg
    and end.

Command: **fill-region** start end &optional justify nosqueeze to-eop [¶](#index-fill_002dregion)
:   This command fills each of the paragraphs in the region from start
    to end. It justifies as well if justify is
    non-`nil`.

    If nosqueeze is non-`nil`, that means to leave whitespace
    other than line breaks untouched. If to-eop is non-`nil`,
    that means to keep filling to the end of the paragraph—or the next hard
    newline, if `use-hard-newlines` is enabled (see below).

    The variable `paragraph-separate` controls how to distinguish
    paragraphs. See [Standard Regular Expressions Used in Editing](#Standard-Regexps).

Function: **pixel-fill-region** start end pixel-width [¶](#index-pixel_002dfill_002dregion)
:   Most Emacs buffers use monospaced text, so all the filling functions
    (like `fill-region`) work based on the number of characters and
    `char-width`. However, Emacs can render other types of things,
    like text that contains images and using proportional fonts, and the
    `pixel-fill-region` exists to handle that. It fills the region
    of text between start and end at pixel granularity, so
    text using variable-pitch fonts or several different fonts looks
    filled regardless of different character sizes. The argument
    pixel-width specifies the maximum pixel width a line is allowed
    to have after filling; it is the pixel-resolution equivalent of the
    `fill-column` in `fill-region`. For instance, this Lisp
    snippet will insert text using a proportional font, and then fill this
    to be no wider than 300 pixels:

    ```
    (insert (propertize
    	 "This is a sentence that's ends here."
    	 'face 'variable-pitch))
    (pixel-fill-region (point) (point-max) 300)
    ```

    If start isn’t at the start of a line, the horizontal position
    of start, converted to pixel units, will be used as the
    indentation prefix on subsequent lines.

    The `pixel-fill-width` helper function can be used to compute the
    pixel width to use. If given no arguments, it’ll return a value
    slightly less than the width of the current window. The first
    optional value, columns, specifies the number of columns using
    the standard, monospaced fonts, for example `fill-column`. The second
    optional value is the window to use. You’d typically use it like
    this:

    ```
    (pixel-fill-region
      start end (pixel-fill-width fill-column))
    ```

Command: **fill-individual-paragraphs** start end &optional justify citation-regexp [¶](#index-fill_002dindividual_002dparagraphs)
:   This command fills each paragraph in the region according to its
    individual fill prefix. Thus, if the lines of a paragraph were indented
    with spaces, the filled paragraph will remain indented in the same
    fashion.

    The first two arguments, start and end, are the beginning
    and end of the region to be filled. The third and fourth arguments,
    justify and citation-regexp, are optional. If
    justify is non-`nil`, the paragraphs are justified as
    well as filled. If citation-regexp is non-`nil`, it means the
    function is operating on a mail message and therefore should not fill
    the header lines. If citation-regexp is a string, it is used as
    a regular expression; if it matches the beginning of a line, that line
    is treated as a citation marker.

    Ordinarily, `fill-individual-paragraphs` regards each change in
    indentation as starting a new paragraph. If
    `fill-individual-varying-indent` is non-`nil`, then only
    separator lines separate paragraphs. That mode can handle indented
    paragraphs with additional indentation on the first line.

User Option: **fill-individual-varying-indent** [¶](#index-fill_002dindividual_002dvarying_002dindent)
:   This variable alters the action of `fill-individual-paragraphs` as
    described above.

Command: **fill-region-as-paragraph** start end &optional justify nosqueeze squeeze-after [¶](#index-fill_002dregion_002das_002dparagraph)
:   This command considers a region of text as a single paragraph and fills
    it. The behavior of this command is controlled by the variable
    `fill-region-as-paragraph-function`, with the default
    implementation being `fill-region-as-paragraph-default`, which is
    described in detail below.

    If the region was made up of many paragraphs, the blank lines
    between paragraphs are removed. This function justifies as well as
    filling when justify is non-`nil`.

    If nosqueeze is non-`nil`, that means to leave whitespace
    other than line breaks untouched. If squeeze-after is
    non-`nil`, it specifies a position in the region, and means
    that whitespace other than line breaks should be left untouched before
    that position.

    In Adaptive Fill mode, this command calls `fill-context-prefix` to
    choose a fill prefix by default. See [Adaptive Fill Mode](#Adaptive-Fill).

Command: **justify-current-line** &optional how eop nosqueeze [¶](#index-justify_002dcurrent_002dline)
:   This command inserts spaces between the words of the current line so
    that the line ends exactly at `fill-column`. It returns
    `nil`.

    The argument how, if non-`nil` specifies explicitly the style
    of justification. It can be `left`, `right`, `full`,
    `center`, or `none`. If it is `t`, that means to
    follow specified justification style (see `current-justification`,
    below). `nil` means to do full justification.

    If eop is non-`nil`, that means do only left-justification
    if `current-justification` specifies full justification. This is
    used for the last line of a paragraph; even if the paragraph as a
    whole is fully justified, the last line should not be.

    If nosqueeze is non-`nil`, that means do not change interior
    whitespace.

User Option: **default-justification** [¶](#index-default_002djustification)
:   This variable’s value specifies the style of justification to use for
    text that doesn’t specify a style with a text property. The possible
    values are `left`, `right`, `full`, `center`, or
    `none`. The default value is `left`.

Function: **current-justification** [¶](#index-current_002djustification)
:   This function returns the proper justification style to use for filling
    the text around point.

    This returns the value of the `justification` text property at
    point, or the variable `default-justification` if there is no such
    text property. However, it returns `nil` rather than `none`
    to mean “don’t justify”.

User Option: **sentence-end-double-space** [¶](#index-sentence_002dend_002ddouble_002dspace)
:   If this variable is non-`nil`, a period followed by just one space
    does not count as the end of a sentence, and the filling functions
    avoid breaking the line at such a place.

User Option: **sentence-end-without-period** [¶](#index-sentence_002dend_002dwithout_002dperiod)
:   If this variable is non-`nil`, a sentence can end without a
    period. This is used for languages like Thai, where sentences end
    with a double space but without a period.

User Option: **sentence-end-without-space** [¶](#index-sentence_002dend_002dwithout_002dspace)
:   If this variable is non-`nil`, it should be a string of
    characters that can end a sentence without following spaces.

User Option: **fill-separate-heterogeneous-words-with-space** [¶](#index-fill_002dseparate_002dheterogeneous_002dwords_002dwith_002dspace)
:   If this variable is non-`nil`, two words of different kind (e.g.,
    English and CJK) will be separated with a space when concatenating one
    that is in the end of a line and the other that is in the beginning of
    the next line for filling.

Variable: **fill-paragraph-function** [¶](#index-fill_002dparagraph_002dfunction)
:   This variable provides a way to override the filling of paragraphs.
    If its value is non-`nil`, `fill-paragraph` calls this
    function to do the work. If the function returns a non-`nil`
    value, `fill-paragraph` assumes the job is done, and immediately
    returns that value.

    The usual use of this feature is to fill comments in programming
    language modes. If the function needs to fill a paragraph in the usual
    way, it can do so as follows:

    ```
    (let ((fill-paragraph-function nil))
      (fill-paragraph arg))
    ```

Variable: **fill-forward-paragraph-function** [¶](#index-fill_002dforward_002dparagraph_002dfunction)
:   This variable provides a way to override how the filling functions,
    such as `fill-region` and `fill-paragraph`, move forward to
    the next paragraph. Its value should be a function, which is called
    with a single argument n, the number of paragraphs to move, and
    should return the difference between n and the number of
    paragraphs actually moved. The default value of this variable is
    `forward-paragraph`. See [Paragraphs](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Paragraphs) in The GNU Emacs
    Manual.

Variable: **fill-region-as-paragraph-function** [¶](#index-fill_002dregion_002das_002dparagraph_002dfunction)
:   This variable provides a way to override how functions like
    `fill-paragraph` and `fill-region` fill text. Its value
    should be a function, which should accept the arguments defined by
    `fill-region-as-paragraph` and return the fill prefix used for
    filling. The default value of this variable is
    `fill-region-as-paragraph-default`.

Variable: **use-hard-newlines** [¶](#index-use_002dhard_002dnewlines)
:   If this variable is non-`nil`, the filling functions do not delete
    newlines that have the `hard` text property. These hard
    newlines act as paragraph separators. See [Hard and Soft Newlines](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Hard-and-Soft-Newlines) in The GNU Emacs Manual.

---

Next: [Adaptive Fill Mode](#Adaptive-Fill), Previous: [Filling](#Filling), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
