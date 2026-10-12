### 33.12 Margins for Filling [¶](#Margins-for-Filling)

User Option: **fill-prefix** [¶](#index-fill_002dprefix)
:   This buffer-local variable, if non-`nil`, specifies a string of
    text that appears at the beginning of normal text lines and should be
    disregarded when filling them. Any line that fails to start with the
    fill prefix is considered the start of a paragraph; so is any line
    that starts with the fill prefix followed by additional whitespace.
    Lines that start with the fill prefix but no additional whitespace are
    ordinary text lines that can be filled together. The resulting filled
    lines also start with the fill prefix.

    The fill prefix follows the left margin whitespace, if any.

User Option: **fill-column** [¶](#index-fill_002dcolumn)
:   This buffer-local variable specifies the maximum width of filled lines.
    Its value should be an integer, which is a number of columns. All the
    filling, justification, and centering commands are affected by this
    variable, including Auto Fill mode (see [Auto Filling](#Auto-Filling)).

    As a practical matter, if you are writing text for other people to
    read, you should set `fill-column` to no more than 70. Otherwise
    the line will be too long for people to read comfortably, and this can
    make the text seem clumsy.

    The default value for `fill-column` is 70. To disable Auto Fill
    mode in a specific mode, you could say something like:

    ```
    (add-hook 'foo-mode-hook (lambda () (auto-fill-mode -1))
    ```

Command: **set-left-margin** from to margin [¶](#index-set_002dleft_002dmargin)
:   This sets the `left-margin` property on the text from from to
    to to the value margin. If Auto Fill mode is enabled, this
    command also refills the region to fit the new margin.

Command: **set-right-margin** from to margin [¶](#index-set_002dright_002dmargin)
:   This sets the `right-margin` property on the text from from
    to to to the value margin. If Auto Fill mode is enabled,
    this command also refills the region to fit the new margin.

Function: **current-left-margin** [¶](#index-current_002dleft_002dmargin)
:   This function returns the proper left margin value to use for filling
    the text around point. The value is the sum of the `left-margin`
    property of the character at the start of the current line (or zero if
    none), and the value of the variable `left-margin`.

Function: **current-fill-column** [¶](#index-current_002dfill_002dcolumn)
:   This function returns the proper fill column value to use for filling
    the text around point. The value is the value of the `fill-column`
    variable, minus the value of the `right-margin` property of the
    character after point.

Command: **move-to-left-margin** &optional n force [¶](#index-move_002dto_002dleft_002dmargin)
:   This function moves point to the left margin of the current line. The
    column moved to is determined by calling the function
    `current-left-margin`. If the argument n is non-`nil`,
    `move-to-left-margin` moves forward n−1 lines first.

    If force is non-`nil`, that says to fix the line’s
    indentation if that doesn’t match the left margin value.

Function: **delete-to-left-margin** &optional from to [¶](#index-delete_002dto_002dleft_002dmargin)
:   This function removes left margin indentation from the text between
    from and to. The amount of indentation to delete is
    determined by calling `current-left-margin`. In no case does this
    function delete non-whitespace. If from and to are omitted,
    they default to the whole buffer.

Function: **indent-to-left-margin** [¶](#index-indent_002dto_002dleft_002dmargin)
:   This function adjusts the indentation at the beginning of the current
    line to the value specified by the variable `left-margin`. (That
    may involve either inserting or deleting whitespace.) This function
    is the value of `indent-line-function` in Paragraph-Indent Text mode.

User Option: **left-margin** [¶](#index-left_002dmargin)
:   This variable specifies the base left margin column. In Fundamental
    mode, `RET` indents to this column. This variable automatically
    becomes buffer-local when set in any fashion.

User Option: **fill-nobreak-predicate** [¶](#index-fill_002dnobreak_002dpredicate)
:   This variable gives major modes a way to specify not to break a line
    at certain places. Its value should be a list of functions. Whenever
    filling considers breaking the line at a certain place in the buffer,
    it calls each of these functions with no arguments and with point
    located at that place. If any of the functions returns
    non-`nil`, then the line won’t be broken there.

---

Next: [Auto Filling](#Auto-Filling), Previous: [Margins for Filling](#Margins), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.13 Adaptive Fill Mode [¶](#Adaptive-Fill-Mode)

When *Adaptive Fill Mode* is enabled, Emacs determines the fill
prefix automatically from the text in each paragraph being filled
rather than using a predetermined value. During filling, this fill
prefix gets inserted at the start of the second and subsequent lines
of the paragraph as described in [Filling](#Filling), and in [Auto Filling](#Auto-Filling).

User Option: **adaptive-fill-mode** [¶](#index-adaptive_002dfill_002dmode)
:   Adaptive Fill mode is enabled when this variable is non-`nil`.
    It is `t` by default.

Function: **fill-context-prefix** from to [¶](#index-fill_002dcontext_002dprefix)
:   This function implements the heart of Adaptive Fill mode; it chooses a
    fill prefix based on the text between from and to,
    typically the start and end of a paragraph. It does this by looking
    at the first two lines of the paragraph, based on the variables
    described below.

    Usually, this function returns the fill prefix, a string. However,
    before doing this, the function makes a final check (not specially
    mentioned in the following) that a line starting with this prefix
    wouldn’t look like the start of a paragraph. Should this happen, the
    function signals the anomaly by returning `nil` instead.

    In detail, `fill-context-prefix` does this:

    1. It takes a candidate for the fill prefix from the first line—it
       tries first the function in `adaptive-fill-function` (if any),
       then the regular expression `adaptive-fill-regexp` (see below).
       The first non-`nil` result of these, or the empty string if
       they’re both `nil`, becomes the first line’s candidate.
    2. If the paragraph has as yet only one line, the function tests the
       validity of the prefix candidate just found. The function then
       returns the candidate if it’s valid, or a string of spaces otherwise.
       (see the description of `adaptive-fill-first-line-regexp` below).
    3. When the paragraph already has two lines, the function next looks for
       a prefix candidate on the second line, in just the same way it did for
       the first line. If it doesn’t find one, it returns `nil`.
    4. The function now compares the two candidate prefixes heuristically: if
       the non-whitespace characters in the line 2 candidate occur in the
       same order in the line 1 candidate, the function returns the line 2
       candidate. Otherwise, it returns the largest initial substring which
       is common to both candidates (which might be the empty string).

User Option: **adaptive-fill-regexp** [¶](#index-adaptive_002dfill_002dregexp)
:   Adaptive Fill mode matches this regular expression against the text
    starting after the left margin whitespace (if any) on a line; the
    characters it matches are that line’s candidate for the fill prefix.

    The default value matches whitespace with certain punctuation
    characters intermingled.

User Option: **adaptive-fill-first-line-regexp** [¶](#index-adaptive_002dfill_002dfirst_002dline_002dregexp)
:   Used only in one-line paragraphs, this regular expression acts as an
    additional check of the validity of the one available candidate fill
    prefix: the candidate must match this regular expression, or match
    `comment-start-skip`. If it doesn’t, `fill-context-prefix`
    replaces the candidate with a string of spaces of the same width
    as it.

    The default value of this variable is `` "\\`[ \t]*\\'" ``, which
    matches only a string of whitespace. The effect of this default is to
    force the fill prefixes found in one-line paragraphs always to be pure
    whitespace.

User Option: **adaptive-fill-function** [¶](#index-adaptive_002dfill_002dfunction)
:   You can specify more complex ways of choosing a fill prefix
    automatically by setting this variable to a function. The function is
    called with point after the left margin (if any) of a line, and it
    must preserve point. It should return either that line’s fill
    prefix or `nil`, meaning it has failed to determine a prefix.

---

Next: [Sorting Text](#Sorting), Previous: [Adaptive Fill Mode](#Adaptive-Fill), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.14 Auto Filling [¶](#Auto-Filling-1)

Auto Fill mode is a minor mode that fills lines automatically as text is
inserted. See [Auto Fill](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Auto-Fill) in The GNU Emacs Manual. This
section describes some variables used by Auto Fill mode. For a
description of functions that you can call explicitly to fill and
justify existing text, see [Filling](#Filling).

Auto Fill mode also enables the functions that change the margins and
justification style to refill portions of the text. See [Margins for Filling](#Margins).

Variable: **auto-fill-function** [¶](#index-auto_002dfill_002dfunction)
:   The value of this buffer-local variable should be a function (of no
    arguments) to be called after self-inserting a character from the table
    `auto-fill-chars`, see below. It may be `nil`, in which case
    nothing special is done in that case.

    The value of `auto-fill-function` is `do-auto-fill` when Auto
    Fill mode is enabled. That is a function whose sole purpose is to
    implement the usual strategy for breaking a line.

Variable: **normal-auto-fill-function** [¶](#index-normal_002dauto_002dfill_002dfunction)
:   This variable specifies the function to use for
    `auto-fill-function`, if and when Auto Fill is turned on. Major
    modes can set buffer-local values for this variable to alter how Auto
    Fill works.

Variable: **auto-fill-chars** [¶](#index-auto_002dfill_002dchars)
:   A char table of characters which invoke `auto-fill-function` when
    self-inserted—space and newline in most language environments. They
    have an entry `t` in the table.

User Option: **comment-auto-fill-only-comments** [¶](#index-comment_002dauto_002dfill_002donly_002dcomments)
:   This variable, if non-`nil`, means to fill lines automatically
    within comments only. More precisely, this means that if a comment
    syntax was defined for the current buffer, then self-inserting a
    character outside of a comment will not call `auto-fill-function`.

---

Next: [Counting Columns](#Columns), Previous: [Auto Filling](#Auto-Filling), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.15 Sorting Text [¶](#Sorting-Text)

The sorting functions described in this section all rearrange text in
a buffer. This is in contrast to the function `sort`, which
rearranges the order of the elements of a list (see [Functions that Rearrange Lists](#Rearrangement)).
The values returned by these functions are not meaningful.

Function: **sort-subr** reverse nextrecfun endrecfun &optional startkeyfun endkeyfun predicate [¶](#index-sort_002dsubr)
:   This function is the general text-sorting routine that subdivides a
    buffer into records and then sorts them. Most of the commands in this
    section use this function.

    To understand how `sort-subr` works, consider the whole accessible
    portion of the buffer as being divided into disjoint pieces called
    *sort records*. The records may or may not be contiguous, but they
    must not overlap. A portion of each sort record (perhaps all of it) is
    designated as the sort key. Sorting rearranges the records in order by
    their sort keys.

    Usually, the records are rearranged in order of ascending sort key.
    If the first argument to the `sort-subr` function, reverse,
    is non-`nil`, the sort records are rearranged in order of
    descending sort key.

    The next four arguments to `sort-subr` are functions that are
    called to move point across a sort record. They are called many times
    from within `sort-subr`.

    1. nextrecfun is called with point at the end of a record. This
       function moves point to the start of the next record. The first record
       is assumed to start at the position of point when `sort-subr` is
       called. Therefore, you should usually move point to the beginning of
       the buffer before calling `sort-subr`.

       This function can indicate there are no more sort records by leaving
       point at the end of the buffer.
    2. endrecfun is called with point within a record. It moves point to
       the end of the record.
    3. startkeyfun is called to move point from the start of a record to
       the start of the sort key. This argument is optional; if it is omitted,
       the whole record is the sort key. If supplied, the function should
       either return a non-`nil` value to be used as the sort key, or
       return `nil` to indicate that the sort key is in the buffer
       starting at point. In the latter case, endkeyfun is called to
       find the end of the sort key.
    4. endkeyfun is called to move point from the start of the sort key
       to the end of the sort key. This argument is optional. If
       startkeyfun returns `nil` and this argument is omitted (or
       `nil`), then the sort key extends to the end of the record. There
       is no need for endkeyfun if startkeyfun returns a
       non-`nil` value.

    The argument predicate is the function to use to compare keys.
    It is called with two arguments, the keys to compare, and should
    return non-`nil` if the first key should come before the second
    in the sorting order. What exactly are the key arguments depends on
    what startkeyfun and endkeyfun return. If predicate
    is omitted or `nil`, it defaults to `<` if the keys are
    numbers, to `compare-buffer-substrings` if the keys are cons
    cells (whose `car` and `cdr` are start and end buffer
    positions of the key), and to `string<` otherwise (with keys
    assumed to be strings).

    As an example of `sort-subr`, here is the complete function
    definition for `sort-lines`:

    ```
    ;; Note that the first two lines of doc string
    ;; are effectively one line when viewed by a user.
    (defun sort-lines (reverse beg end)
      "Sort lines in region alphabetically;\
     argument means descending order.
    Called from a program, there are three arguments:
    ```

    ```
    REVERSE (non-nil means reverse order),\
     BEG and END (region to sort).
    The variable `sort-fold-case' determines\
     whether alphabetic case affects
    the sort order."
    ```

    ```
      (interactive "P\nr")
      (save-excursion
        (save-restriction
          (narrow-to-region beg end)
          (goto-char (point-min))
          (let ((inhibit-field-text-motion t))
            (sort-subr reverse 'forward-line 'end-of-line)))))
    ```

    Here `forward-line` moves point to the start of the next record,
    and `end-of-line` moves point to the end of record. We do not pass
    the arguments startkeyfun and endkeyfun, because the entire
    record is used as the sort key.

    The `sort-paragraphs` function is very much the same, except that
    its `sort-subr` call looks like this:

    ```
    (sort-subr reverse
               (lambda ()
                 (while (and (not (eobp))
                             (looking-at paragraph-separate))
                   (forward-line 1)))
               'forward-paragraph)
    ```

    Markers pointing into any sort records are left with no useful
    position after `sort-subr` returns.

User Option: **sort-fold-case** [¶](#index-sort_002dfold_002dcase)
:   If this variable is non-`nil`, `sort-subr` and the other
    buffer sorting functions ignore case when comparing strings.

Command: **sort-regexp-fields** reverse record-regexp key-regexp start end [¶](#index-sort_002dregexp_002dfields)
:   This command sorts the region between start and end
    alphabetically as specified by record-regexp and key-regexp.
    If reverse is a negative integer, then sorting is in reverse
    order.

    Alphabetical sorting means that two sort keys are compared by
    comparing the first characters of each, the second characters of each,
    and so on. If a mismatch is found, it means that the sort keys are
    unequal; the sort key whose character is less at the point of first
    mismatch is the lesser sort key. The individual characters are compared
    according to their numerical character codes in the Emacs character set.

    The value of the record-regexp argument specifies how to divide
    the buffer into sort records. At the end of each record, a search is
    done for this regular expression, and the text that matches it is taken
    as the next record. For example, the regular expression ‘`^.+$`’,
    which matches lines with at least one character besides a newline, would
    make each such line into a sort record. See [Regular Expressions](#Regular-Expressions), for
    a description of the syntax and meaning of regular expressions.

    The value of the key-regexp argument specifies what part of each
    record is the sort key. The key-regexp could match the whole
    record, or only a part. In the latter case, the rest of the record has
    no effect on the sorted order of records, but it is carried along when
    the record moves to its new position.

    The key-regexp argument can refer to the text matched by a
    subexpression of record-regexp, or it can be a regular expression
    on its own.

    If key-regexp is:

    ‘`\digit`’
    :   then the text matched by the digitth ‘`\(...\)`’ parenthesis
        grouping in record-regexp is the sort key.

    ‘`\&`’
    :   then the whole record is the sort key.

    a regular expression
    :   then `sort-regexp-fields` searches for a match for the regular
        expression within the record. If such a match is found, it is the sort
        key. If there is no match for key-regexp within a record then
        that record is ignored, which means its position in the buffer is not
        changed. (The other records may move around it.)

    For example, if you plan to sort all the lines in the region by the
    first word on each line starting with the letter ‘`f`’, you should
    set record-regexp to ‘`^.*$`’ and set key-regexp to
    ‘`\<f\w*\>`’. The resulting expression looks like this:

    ```
    (sort-regexp-fields nil "^.*$" "\\<f\\w*\\>"
                        (region-beginning)
                        (region-end))
    ```

    If you call `sort-regexp-fields` interactively, it prompts for
    record-regexp and key-regexp in the minibuffer.

Command: **sort-lines** reverse start end [¶](#index-sort_002dlines)
:   This command alphabetically sorts lines in the region between
    start and end. If reverse is non-`nil`, the sort
    is in reverse order.

Command: **sort-paragraphs** reverse start end [¶](#index-sort_002dparagraphs)
:   This command alphabetically sorts paragraphs in the region between
    start and end. If reverse is non-`nil`, the sort
    is in reverse order.

Command: **sort-pages** reverse start end [¶](#index-sort_002dpages)
:   This command alphabetically sorts pages in the region between
    start and end. If reverse is non-`nil`, the sort
    is in reverse order.

Command: **sort-fields** field start end [¶](#index-sort_002dfields)
:   This command sorts lines in the region between start and
    end, comparing them alphabetically by the fieldth field
    of each line. Fields are separated by whitespace and numbered starting
    from 1. If field is negative, sorting is by the
    −fieldth field from the end of the line. This command
    is useful for sorting tables.

Command: **sort-numeric-fields** field start end [¶](#index-sort_002dnumeric_002dfields)
:   This command sorts lines in the region between start and
    end, comparing them numerically by the fieldth field of
    each line. Fields are separated by whitespace and numbered starting
    from 1. The specified field must contain a number in each line of the
    region. Numbers starting with 0 are treated as octal, and numbers
    starting with ‘`0x`’ are treated as hexadecimal.

    If field is negative, sorting is by the
    −fieldth field from the end of the line. This
    command is useful for sorting tables.

User Option: **sort-numeric-base** [¶](#index-sort_002dnumeric_002dbase)
:   This variable specifies the default radix for
    `sort-numeric-fields` to parse numbers.

Command: **sort-columns** reverse &optional beg end [¶](#index-sort_002dcolumns)
:   This command sorts the lines in the region between beg and
    end, comparing them alphabetically by a certain range of
    columns. The column positions of beg and end bound the
    range of columns to sort on.

    If reverse is non-`nil`, the sort is in reverse order.

    One unusual thing about this command is that the entire line
    containing position beg, and the entire line containing position
    end, are included in the region sorted.

    Note that `sort-columns` rejects text that contains tabs, because
    tabs could be split across the specified columns. Use `M-x
    untabify` to convert tabs to spaces before sorting.

    When possible, this command actually works by calling the `sort`
    utility program.

---

Next: [Indentation](#Indentation), Previous: [Sorting Text](#Sorting), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.16 Counting Columns [¶](#Counting-Columns)

The column functions convert between a character position (counting
characters from the beginning of the buffer) and a column position
(counting screen characters from the beginning of a line).

These functions count each character according to the number of
columns it occupies on the screen. This means control characters count
as occupying 2 or 4 columns, depending upon the value of
`ctl-arrow`, and tabs count as occupying a number of columns that
depends on the value of `tab-width` and on the column where the tab
begins. See [Usual Display Conventions](#Usual-Display).

Column number computations ignore the width of the window and the
amount of horizontal scrolling. Consequently, a column value can be
arbitrarily high. The first (or leftmost) column is numbered 0. They
also ignore overlays and text properties, aside from invisibility.
Invisible text is considered as having zero width, unless
`buffer-invisibility-spec` specifies that invisible text should
be displayed as ellipsis (see [Invisible Text](#Invisible-Text)).

Function: **current-column** [¶](#index-current_002dcolumn)
:   This function returns the horizontal position of point, measured in
    columns, counting from 0 at the left margin. The column position is the
    sum of the widths of all the displayed representations of the characters
    between the start of the current line and point.

Command: **move-to-column** column &optional force [¶](#index-move_002dto_002dcolumn)
:   This function moves point to column in the current line. The
    calculation of column takes into account the widths of the
    displayed representations of the characters between the start of the
    line and point.

    When called interactively, column is the value of prefix numeric
    argument. If column is not an integer, an error is signaled.

    If it is impossible to move to column column because that is in
    the middle of a multicolumn character such as a tab, point moves to the
    end of that character. However, if force is non-`nil`, and
    column is in the middle of a tab, then `move-to-column`
    either converts the tab into spaces (when `indent-tabs-mode` is
    `nil`), or inserts enough spaces before it (otherwise), so that
    point can move precisely to column column. Other multicolumn
    characters can cause anomalies despite force, since there is no
    way to split them.

    The argument force also has an effect if the line isn’t long
    enough to reach column column; if it is `t`, that means to
    add whitespace at the end of the line to reach that column.

    The return value is the column number actually moved to.

---

Next: [Case Changes](#Case-Changes), Previous: [Counting Columns](#Columns), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.17 Indentation [¶](#Indentation-1)

The indentation functions are used to examine, move to, and change
whitespace that is at the beginning of a line. Some of the functions
can also change whitespace elsewhere on a line. Columns and indentation
count from zero at the left margin.

* [Indentation Primitives](#Primitive-Indent)
* [Indentation Controlled by Major Mode](#Mode_002dSpecific-Indent)
* [Indenting an Entire Region](#Region-Indent)
* [Indentation Relative to Previous Lines](#Relative-Indent)
* [Adjustable Tab Stops](#Indent-Tabs)
* [Indentation-Based Motion Commands](#Motion-by-Indent)

---

Next: [Indentation Controlled by Major Mode](#Mode_002dSpecific-Indent), Up: [Indentation](#Indentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.17.1 Indentation Primitives [¶](#Indentation-Primitives)

This section describes the primitive functions used to count and
insert indentation. The functions in the following sections use these
primitives. See [Size of Displayed Text](#Size-of-Displayed-Text), for related functions.

Function: **current-indentation** [¶](#index-current_002dindentation)
:   This function returns the indentation of the current line, which is
    the horizontal position of the first nonblank character. If the
    contents are entirely blank, then this is the horizontal position of the
    end of the line.

    This function considers invisible text as having zero width, unless
    `buffer-invisibility-spec` specifies that invisible text should
    be displayed as ellipsis. See [Invisible Text](#Invisible-Text).

Command: **indent-to** column &optional minimum [¶](#index-indent_002dto)
:   This function indents from point with tabs and spaces until column
    is reached. If minimum is specified and non-`nil`, then at
    least that many spaces are inserted even if this requires going beyond
    column. Otherwise the function does nothing if point is already
    beyond column. The value is the column at which the inserted
    indentation ends.

    The inserted whitespace characters inherit text properties from the
    surrounding text (usually, from the preceding text only). See [Stickiness of Text Properties](#Sticky-Properties).

User Option: **indent-tabs-mode** [¶](#index-indent_002dtabs_002dmode)
:   If this variable is non-`nil`, indentation functions can insert
    tabs as well as spaces. Otherwise, they insert only spaces. Setting
    this variable automatically makes it buffer-local in the current buffer.

---

Next: [Indenting an Entire Region](#Region-Indent), Previous: [Indentation Primitives](#Primitive-Indent), Up: [Indentation](#Indentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.17.2 Indentation Controlled by Major Mode [¶](#Indentation-Controlled-by-Major-Mode)

An important function of each major mode is to customize the `TAB`
key to indent properly for the language being edited. This section
describes the mechanism of the `TAB` key and how to control it.
The functions in this section return unpredictable values.

Command: **indent-for-tab-command** &optional rigid [¶](#index-indent_002dfor_002dtab_002dcommand)
:   This is the command bound to `TAB` in most editing modes. Its
    usual action is to indent the current line, but it can alternatively
    insert a tab character or indent a region.

    Here is what it does:

    * First, it checks whether Transient Mark mode is enabled and the region
      is active. If so, it calls `indent-region` to indent all the
      text in the region (see [Indenting an Entire Region](#Region-Indent)).
    * Otherwise, if the indentation function in `indent-line-function`
      is `indent-to-left-margin` (a trivial command that inserts a tab
      character), or if the variable `tab-always-indent` specifies that
      a tab character ought to be inserted (see below), then it inserts a
      tab character.
    * Otherwise, it indents the current line; this is done by calling the
      function in `indent-line-function`. If the line is already
      indented, and the value of `tab-always-indent` is `complete`
      (see below), it tries completing the text at point.

    If rigid is non-`nil` (interactively, with a prefix
    argument), then after this command indents a line or inserts a tab, it
    also rigidly indents the entire balanced expression which starts at
    the beginning of the current line, in order to reflect the new
    indentation. This argument is ignored if the command indents the
    region.

Variable: **indent-line-function** [¶](#index-indent_002dline_002dfunction)
:   This variable’s value is the function to be used by
    `indent-for-tab-command`, and various other indentation commands,
    to indent the current line. It is usually assigned by the major mode;
    for instance, Lisp mode sets it to `lisp-indent-line`, C mode
    sets it to `c-indent-line`, and so on. The default value is
    `indent-relative`. See [Automatic Indentation of code](#Auto_002dIndentation).

Command: **indent-according-to-mode** [¶](#index-indent_002daccording_002dto_002dmode)
:   This command calls the function in `indent-line-function` to
    indent the current line in a way appropriate for the current major mode.

Command: **newline-and-indent** [¶](#index-newline_002dand_002dindent)
:   This function inserts a newline, then indents the new line (the one
    following the newline just inserted) according to the major mode. It
    does indentation by calling `indent-according-to-mode`.

Command: **reindent-then-newline-and-indent** [¶](#index-reindent_002dthen_002dnewline_002dand_002dindent)
:   This command reindents the current line, inserts a newline at point,
    and then indents the new line (the one following the newline just
    inserted). It does indentation on both lines by calling
    `indent-according-to-mode`.

User Option: **tab-always-indent** [¶](#index-tab_002dalways_002dindent)
:   This variable can be used to customize the behavior of the `TAB`
    (`indent-for-tab-command`) command. If the value is `t`
    (the default), the command normally just indents the current line. If
    the value is `nil`, the command indents the current line only if
    point is at the left margin or in the line’s indentation; otherwise,
    it inserts a tab character. If the value is `complete`, the
    command first tries to indent the current line, and if the line was
    already indented, it calls `completion-at-point` to complete the
    text at point (see [Completion in Ordinary Buffers](#Completion-in-Buffers)).

User Option: **tab-first-completion** [¶](#index-tab_002dfirst_002dcompletion)
:   If `tab-always-indent` is `complete`, whether to expand or
    indent can be further customized via the `tab-first-completion`
    variable. The following values can be used:

    `eol`
    :   Only complete if point is at the end of a line.

    `word`
    :   Complete unless the next character has word syntax.

    `word-or-paren`
    :   Complete unless the next character has word syntax or is a
        parenthesis.

    `word-or-paren-or-punct`
    :   Complete unless the next character has word syntax, or is a
        parenthesis, or is punctuation.

    In any case, typing `TAB` a second time always results in
    completion.

Some major modes need to support embedded regions of text whose
syntax belongs to a different major mode. Examples include
*literate programming* source files that combine documentation and
snippets of source code, Yacc/Bison programs that include snippets of
Python or JS code, etc. To correctly indent the embedded chunks, the primary
mode needs to delegate the indentation to another mode’s indentation
engine (e.g., call `js-indent-line` for JS code or
`python-indent-line` for Python), while providing it with some
context to guide the indentation. Major modes, for their part, should
avoid calling `widen` in their indentation code and obey
`prog-first-column`.

Variable: **prog-indentation-context** [¶](#index-prog_002dindentation_002dcontext)
:   This variable, when non-`nil`, holds the indentation context for
    the sub-mode’s indentation engine provided by the superior major mode.
    The value should be a list of the form `(first-column . rest)`.
    The members of the list have the following meaning:

    first-column
    :   The column to be used for top-level constructs. This replaces the
        default value of the top-level column used by the sub-mode, usually
        zero.

    rest
    :   This value is currently unused.

The following convenience function should be used by major mode’s
indentation engine in support of invocations as sub-modes of another
major mode.

Function: **prog-first-column** [¶](#index-prog_002dfirst_002dcolumn)
:   Call this function instead of using a literal value (usually, zero) of
    the column number for indenting top-level program constructs. The
    function’s value is the column number to use for top-level constructs.
    When no superior mode is in effect, this function returns zero.

---

Next: [Indentation Relative to Previous Lines](#Relative-Indent), Previous: [Indentation Controlled by Major Mode](#Mode_002dSpecific-Indent), Up: [Indentation](#Indentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.17.3 Indenting an Entire Region [¶](#Indenting-an-Entire-Region)

This section describes commands that indent all the lines in the
region. They return unpredictable values.

Command: **indent-region** start end &optional to-column [¶](#index-indent_002dregion)
:   This command indents each nonblank line starting between start
    (inclusive) and end (exclusive). If to-column is
    `nil`, `indent-region` indents each nonblank line by calling
    the current mode’s indentation function, the value of
    `indent-line-function`.

    If to-column is non-`nil`, it should be an integer
    specifying the number of columns of indentation; then this function
    gives each line exactly that much indentation, by either adding or
    deleting whitespace.

    If there is a fill prefix, `indent-region` indents each line
    by making it start with the fill prefix.

Variable: **indent-region-function** [¶](#index-indent_002dregion_002dfunction)
:   The value of this variable is a function that can be used by
    `indent-region` as a short cut. It should take two arguments, the
    start and end of the region. You should design the function so
    that it will produce the same results as indenting the lines of the
    region one by one, but presumably faster.

    If the value is `nil`, there is no short cut, and
    `indent-region` actually works line by line.

    A short-cut function is useful in modes such as C mode and Lisp mode,
    where the `indent-line-function` must scan from the beginning of
    the function definition: applying it to each line would be quadratic in
    time. The short cut can update the scan information as it moves through
    the lines indenting them; this takes linear time. In a mode where
    indenting a line individually is fast, there is no need for a short cut.

    `indent-region` with a non-`nil` argument to-column has
    a different meaning and does not use this variable.

Command: **indent-rigidly** start end count [¶](#index-indent_002drigidly)
:   This function indents all lines starting between start
    (inclusive) and end (exclusive) sideways by count columns.
    This preserves the shape of the affected region, moving it as a
    rigid unit.

    This is useful not only for indenting regions of unindented text, but
    also for indenting regions of formatted code. For example, if
    count is 3, this command adds 3 columns of indentation to every
    line that begins in the specified region.

    If called interactively with no prefix argument, this command invokes
    a transient mode for adjusting indentation rigidly. See [Indentation
    Commands](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Indentation-Commands) in The GNU Emacs Manual.

Command: **indent-code-rigidly** start end columns &optional nochange-regexp [¶](#index-indent_002dcode_002drigidly)
:   This is like `indent-rigidly`, except that it doesn’t alter lines
    that start within strings or comments.

    In addition, it doesn’t alter a line if nochange-regexp matches at
    the beginning of the line (if nochange-regexp is non-`nil`).

---

Next: [Adjustable Tab Stops](#Indent-Tabs), Previous: [Indenting an Entire Region](#Region-Indent), Up: [Indentation](#Indentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.17.4 Indentation Relative to Previous Lines [¶](#Indentation-Relative-to-Previous-Lines)

This section describes two commands that indent the current line
based on the contents of previous lines.

Command: **indent-relative** &optional first-only unindented-ok [¶](#index-indent_002drelative)
:   This command inserts whitespace at point, extending to the same
    column as the next *indent point* of the previous nonblank line. An
    indent point is a non-whitespace character following whitespace. The
    next indent point is the first one at a column greater than the current
    column of point. For example, if point is underneath and to the left of
    the first non-blank character of a line of text, it moves to that column
    by inserting whitespace.

    If the previous nonblank line has no next indent point (i.e., none at a
    great enough column position), `indent-relative` either does
    nothing (if unindented-ok is non-`nil`) or calls
    `tab-to-tab-stop`. Thus, if point is underneath and to the right
    of the last column of a short line of text, this command ordinarily
    moves point to the next tab stop by inserting whitespace.

    If first-only is non-`nil`, only the first indent point is
    considered.

    The return value of `indent-relative` is unpredictable.

    In the following example, point is at the beginning of the second
    line:

    ```
                This line is indented twelve spaces.
    ∗The quick brown fox jumped.
    ```

    Evaluation of the expression `(indent-relative nil)` produces the
    following:

    ```
                This line is indented twelve spaces.
                ∗The quick brown fox jumped.
    ```

    In this next example, point is between the ‘`m`’ and ‘`p`’ of
    ‘`jumped`’:

    ```
                This line is indented twelve spaces.
    The quick brown fox jum∗ped.
    ```

    Evaluation of the expression `(indent-relative nil)` produces the
    following:

    ```
                This line is indented twelve spaces.
    The quick brown fox jum  ∗ped.
    ```

Command: **indent-relative-first-indent-point** [¶](#index-indent_002drelative_002dfirst_002dindent_002dpoint)
:   This command indents the current line like the previous nonblank line,
    by calling `indent-relative` with `t` as the
    first-only argument. The return value is unpredictable.

    If the previous nonblank line has no indent points beyond the current
    column, this command does nothing.

---

Next: [Indentation-Based Motion Commands](#Motion-by-Indent), Previous: [Indentation Relative to Previous Lines](#Relative-Indent), Up: [Indentation](#Indentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.17.5 Adjustable Tab Stops [¶](#Adjustable-Tab-Stops)

This section explains the mechanism for user-specified tab stops
and the mechanisms that use and set them. The name “tab stops” is
used because the feature is similar to that of the tab stops on a
typewriter. The feature works by inserting an appropriate number of
spaces and tab characters to reach the next tab stop column; it does not
affect the display of tab characters in the buffer (see [Usual Display Conventions](#Usual-Display)). Note that the `TAB` character as input uses this tab
stop feature only in a few major modes, such as Text mode.
See [Tab Stops](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Tab-Stops) in The GNU Emacs Manual.

Command: **tab-to-tab-stop** [¶](#index-tab_002dto_002dtab_002dstop)
:   This command inserts spaces or tabs before point, up to the next tab
    stop column defined by `tab-stop-list`.

User Option: **tab-stop-list** [¶](#index-tab_002dstop_002dlist)
:   This variable defines the tab stop columns used by `tab-to-tab-stop`.
    It should be either `nil`, or a list of increasing integers,
    which need not be evenly spaced. The list is implicitly
    extended to infinity through repetition of the interval between the
    last and penultimate elements (or `tab-width` if the list has
    fewer than two elements). A value of `nil` means a tab stop
    every `tab-width` columns.

    Use `M-x edit-tab-stops` to edit the location of tab stops interactively.

---

Previous: [Adjustable Tab Stops](#Indent-Tabs), Up: [Indentation](#Indentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.17.6 Indentation-Based Motion Commands [¶](#Indentation_002dBased-Motion-Commands)

These commands, primarily for interactive use, act based on the
indentation in the text.

Command: **back-to-indentation** [¶](#index-back_002dto_002dindentation)
:   This command moves point to the first non-whitespace character in the
    current line (which is the line in which point is located).

Command: **backward-to-indentation** &optional arg [¶](#index-backward_002dto_002dindentation)
:   This command moves point backward arg lines and then to the
    first nonblank character on that line. If arg is omitted or
    `nil`, it defaults to 1.

Command: **forward-to-indentation** &optional arg [¶](#index-forward_002dto_002dindentation)
:   This command moves point forward arg lines and then to the first
    nonblank character on that line. If arg is omitted or
    `nil`, it defaults to 1.

---

Next: [Text Properties](#Text-Properties), Previous: [Indentation](#Indentation), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.18 Case Changes [¶](#Case-Changes-1)

The case change commands described here work on text in the current
buffer. See [Case Conversion in Lisp](#Case-Conversion), for case conversion functions that work
on strings and characters. See [The Case Table](#Case-Tables), for how to customize
which characters are upper or lower case and how to convert them.

Command: **capitalize-region** start end [¶](#index-capitalize_002dregion)
:   This function capitalizes all words in the region defined by
    start and end. To capitalize means to convert each word’s
    first character to upper case and convert the rest of each word to lower
    case. The function returns `nil`.

    If one end of the region is in the middle of a word, the part of the
    word within the region is treated as an entire word.

    When `capitalize-region` is called interactively, start and
    end are point and the mark, with the smallest first.

    ```
    ---------- Buffer: foo ----------
    This is the contents of the 5th foo.
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (capitalize-region 1 37)
    ⇒ nil

    ---------- Buffer: foo ----------
    This Is The Contents Of The 5th Foo.
    ---------- Buffer: foo ----------
    ```

Command: **downcase-region** start end [¶](#index-downcase_002dregion)
:   This function converts all of the letters in the region defined by
    start and end to lower case. The function returns
    `nil`.

    When `downcase-region` is called interactively, start and
    end are point and the mark, with the smallest first.

Command: **upcase-region** start end [¶](#index-upcase_002dregion)
:   This function converts all of the letters in the region defined by
    start and end to upper case. The function returns
    `nil`.

    When `upcase-region` is called interactively, start and
    end are point and the mark, with the smallest first.

Command: **capitalize-word** count [¶](#index-capitalize_002dword)
:   This function capitalizes count words after point, moving point
    over as it does. To capitalize means to convert each word’s first
    character to upper case and convert the rest of each word to lower case.
    If count is negative, the function capitalizes the
    −count previous words but does not move point. The value
    is `nil`.

    If point is in the middle of a word, the part of the word before point
    is ignored when moving forward. The rest is treated as an entire word.

    When `capitalize-word` is called interactively, count is
    set to the numeric prefix argument.

Command: **downcase-word** count [¶](#index-downcase_002dword)
:   This function converts the count words after point to all lower
    case, moving point over as it does. If count is negative, it
    converts the −count previous words but does not move point.
    The value is `nil`.

    When `downcase-word` is called interactively, count is set
    to the numeric prefix argument.

Command: **upcase-word** count [¶](#index-upcase_002dword)
:   This function converts the count words after point to all upper
    case, moving point over as it does. If count is negative, it
    converts the −count previous words but does not move point.
    The value is `nil`.

    When `upcase-word` is called interactively, count is set to
    the numeric prefix argument.

---

Next: [Substituting for a Character Code](#Substitution), Previous: [Case Changes](#Case-Changes), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
