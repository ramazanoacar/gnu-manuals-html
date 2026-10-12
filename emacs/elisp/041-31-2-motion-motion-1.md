### 31.2 Motion [¶](#Motion-1)

Motion functions change the value of point, either relative to the
current value of point, relative to the beginning or end of the buffer,
or relative to the edges of the selected window. See [Point](#Point).

* [Motion by Characters](#Character-Motion)
* [Motion by Words](#Word-Motion)
* [Motion to an End of the Buffer](#Buffer-End-Motion)
* [Motion by Text Lines](#Text-Lines)
* [Motion by Screen Lines](#Screen-Lines)
* [Moving over Balanced Expressions](#List-Motion)
* [Skipping Characters](#Skipping-Characters)

---

Next: [Motion by Words](#Word-Motion), Up: [Motion](#Motion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 31.2.1 Motion by Characters [¶](#Motion-by-Characters)

These functions move point based on a count of characters.
`goto-char` is the fundamental primitive; the other functions use
that.

Command: **goto-char** position [¶](#index-goto_002dchar)
:   This function sets point in the current buffer to the value
    position.

    If narrowing is in effect, position still counts from the
    beginning of the buffer, but point cannot go outside the accessible
    portion. If position is out of range, `goto-char` moves
    point to the beginning or the end of the accessible portion.

    When this function is called interactively, position is the
    numeric prefix argument, if provided; otherwise it is read from the
    minibuffer.

    `goto-char` returns position.

Command: **forward-char** &optional count [¶](#index-forward_002dchar)
:   This function moves point count characters forward, towards the
    end of the buffer (or backward, towards the beginning of the buffer, if
    count is negative). If count is `nil`, the default
    is 1.

    If this attempts to move past the beginning or end of the buffer (or
    the limits of the accessible portion, when narrowing is in effect), it
    signals an error with error symbol `beginning-of-buffer` or
    `end-of-buffer`.

    In an interactive call, count is the numeric prefix argument.

Command: **backward-char** &optional count [¶](#index-backward_002dchar)
:   This is just like `forward-char` except that it moves
    in the opposite direction.

---

Next: [Motion to an End of the Buffer](#Buffer-End-Motion), Previous: [Motion by Characters](#Character-Motion), Up: [Motion](#Motion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 31.2.2 Motion by Words [¶](#Motion-by-Words)

The functions for parsing words described below use the syntax table
and `char-script-table` to decide whether a given character is
part of a word. See [Syntax Tables](#Syntax-Tables), and see [Character Properties](#Character-Properties).

Command: **forward-word** &optional count [¶](#index-forward_002dword)
:   This function moves point forward count words (or backward if
    count is negative). If count is omitted or `nil`, it
    defaults to 1. In an interactive call, count is specified by
    the numeric prefix argument.

    “Moving one word” means moving until point crosses a
    word-constituent character, which indicates the beginning of a word,
    and then continue moving until the word ends. By default, characters
    that begin and end words, known as *word boundaries*, are defined
    by the current buffer’s syntax table (see [Table of Syntax Classes](#Syntax-Class-Table)), but
    modes can override that by setting up a suitable
    `find-word-boundary-function-table`, described below. Characters
    that belong to different scripts (as defined by
    `char-script-table`), also define a word boundary
    (see [Character Properties](#Character-Properties)). In any case, this function cannot
    move point past the boundary of the accessible portion of the buffer,
    or across a field boundary (see [Defining and Using Fields](#Fields)). The most common case of
    a field boundary is the end of the prompt in the minibuffer.

    If it is possible to move count words, without being stopped
    prematurely by the buffer boundary or a field boundary, the value is
    `t`. Otherwise, the return value is `nil` and point stops at
    the buffer boundary or field boundary.

    If `inhibit-field-text-motion` is non-`nil`,
    this function ignores field boundaries.

Command: **backward-word** &optional count [¶](#index-backward_002dword)
:   This function is just like `forward-word`, except that it moves
    backward until encountering the front of a word, rather than forward.

User Option: **words-include-escapes** [¶](#index-words_002dinclude_002descapes)
:   This variable affects the behavior of `forward-word` and
    `backward-word`, and everything that uses them. If it is
    non-`nil`, then characters in the escape and character-quote
    syntax classes count as part of words. Otherwise, they do not.

Variable: **inhibit-field-text-motion** [¶](#index-inhibit_002dfield_002dtext_002dmotion)
:   If this variable is non-`nil`, certain motion functions including
    `forward-word`, `forward-sentence`, and
    `forward-paragraph` ignore field boundaries.

Variable: **find-word-boundary-function-table** [¶](#index-find_002dword_002dboundary_002dfunction_002dtable)
:   This variable affects the behavior of `forward-word` and
    `backward-word`, and everything that uses them. Its value is a
    char-table (see [Char-Tables](#Char_002dTables)) of functions to search for word
    boundaries. If a character has a non-`nil` entry in this table,
    then when a word starts or ends with that character, the corresponding
    function will be called with 2 arguments: pos and limit.
    The function should return the position of the other word boundary.
    Specifically, if pos is smaller than limit, then pos
    is at the beginning of a word, and the function should return the
    position after the last character of the word; otherwise, pos is
    at the last character of a word, and the function should return the
    position of that word’s first character.

Function: **forward-word-strictly** &optional count [¶](#index-forward_002dword_002dstrictly)
:   This function is like `forward-word`, but it is not affected by
    `find-word-boundary-function-table`. Lisp programs that should
    not change behavior when word movement is modified by modes which set
    that table, such as `subword-mode`, should use this function
    instead of `forward-word`.

Function: **backward-word-strictly** &optional count [¶](#index-backward_002dword_002dstrictly)
:   This function is like `backward-word`, but it is not affected by
    `find-word-boundary-function-table`. Like with
    `forward-word-strictly`, use this function instead of
    `backward-word` when movement by words should only consider
    syntax tables.

---

Next: [Motion by Text Lines](#Text-Lines), Previous: [Motion by Words](#Word-Motion), Up: [Motion](#Motion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 31.2.3 Motion to an End of the Buffer [¶](#Motion-to-an-End-of-the-Buffer)

To move point to the beginning of the buffer, write:

```
(goto-char (point-min))
```

Likewise, to move to the end of the buffer, use:

```
(goto-char (point-max))
```

Here are two commands that users use to do these things. They are
documented here to warn you not to use them in Lisp programs, because
they set the mark and display messages in the echo area.

Command: **beginning-of-buffer** &optional n [¶](#index-beginning_002dof_002dbuffer)
:   This function moves point to the beginning of the buffer (or the limits
    of the accessible portion, when narrowing is in effect), setting the
    mark at the previous position (except in Transient Mark mode, if
    the mark is already active, it does not set the mark.)

    If n is non-`nil`, then it puts point n tenths of the
    way from the beginning of the accessible portion of the buffer. In an
    interactive call, n is the numeric prefix argument, if provided;
    otherwise n defaults to `nil`.

    **Warning:** Don’t use this function in Lisp programs!

Command: **end-of-buffer** &optional n [¶](#index-end_002dof_002dbuffer)
:   This function moves point to the end of the buffer (or the limits of
    the accessible portion, when narrowing is in effect), setting the mark
    at the previous position (except in Transient Mark mode when the mark
    is already active). If n is non-`nil`, then it puts point
    n tenths of the way from the end of the accessible portion of
    the buffer.

    In an interactive call, n is the numeric prefix argument,
    if provided; otherwise n defaults to `nil`.

    **Warning:** Don’t use this function in Lisp programs!

---

Next: [Motion by Screen Lines](#Screen-Lines), Previous: [Motion to an End of the Buffer](#Buffer-End-Motion), Up: [Motion](#Motion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 31.2.4 Motion by Text Lines [¶](#Motion-by-Text-Lines)

Text lines are portions of the buffer delimited by newline characters,
which are regarded as part of the previous line. The first text line
begins at the beginning of the buffer, and the last text line ends at
the end of the buffer whether or not the last character is a newline.
The division of the buffer into text lines is not affected by the width
of the window, by line continuation in display, or by how tabs and
control characters are displayed.

Command: **beginning-of-line** &optional count [¶](#index-beginning_002dof_002dline)
:   This function moves point to the beginning of the current line. With an
    argument count not `nil` or 1, it moves forward
    count−1 lines and then to the beginning of the line.

    This function does not move point across a field boundary
    (see [Defining and Using Fields](#Fields)) unless doing so would move beyond there to a
    different line; therefore, if count is `nil` or 1, and
    point starts at a field boundary, point does not move. To ignore
    field boundaries, either bind `inhibit-field-text-motion` to
    `t`, or use the `forward-line` function instead. For
    instance, `(forward-line 0)` does the same thing as
    `(beginning-of-line)`, except that it ignores field boundaries.

    If this function reaches the end of the buffer (or of the accessible
    portion, if narrowing is in effect), it positions point there. No error
    is signaled.

Function: **line-beginning-position** &optional count [¶](#index-line_002dbeginning_002dposition)
:   Return the position that `(beginning-of-line count)`
    would move to.

Command: **end-of-line** &optional count [¶](#index-end_002dof_002dline)
:   This function moves point to the end of the current line. With an
    argument count not `nil` or 1, it moves forward
    count−1 lines and then to the end of the line.

    This function does not move point across a field boundary
    (see [Defining and Using Fields](#Fields)) unless doing so would move beyond there to a
    different line; therefore, if count is `nil` or 1, and
    point starts at a field boundary, point does not move. To ignore
    field boundaries, bind `inhibit-field-text-motion` to `t`.

    If this function reaches the end of the buffer (or of the accessible
    portion, if narrowing is in effect), it positions point there. No error
    is signaled.

Function: **line-end-position** &optional count [¶](#index-line_002dend_002dposition)
:   Return the position that `(end-of-line count)`
    would move to.

Function: **pos-bol** &optional count [¶](#index-pos_002dbol)
:   Like `line-beginning-position`, but ignores fields (and is more
    efficient).

Function: **pos-eol** &optional count [¶](#index-pos_002deol)
:   Like `line-end-position`, but ignores fields (and is more
    efficient).

Command: **forward-line** &optional count [¶](#index-forward_002dline)
:   This function moves point forward count lines, to the beginning of
    the line following that. If count is negative, it moves point
    −count lines backward, to the beginning of a line
    preceding that. If count is zero, it moves point to the
    beginning of the current line. If count is `nil`, that
    means 1.

    If `forward-line` encounters the beginning or end of the buffer (or
    of the accessible portion) before finding that many lines, it sets point
    there. No error is signaled.

    `forward-line` returns the difference between count and the
    number of lines actually moved. If you attempt to move down five lines
    from the beginning of a buffer that has only three lines, point stops at
    the end of the last line, and the value will be 2. As an explicit
    exception, if the last accessible line is non-empty, but has no
    newline (e.g., if the buffer ends without a newline), the function
    sets point to the end of that line, and the value returned by the
    function counts that line as one line successfully moved.

    In an interactive call, count is the numeric prefix argument.

Function: **count-lines** start end &optional ignore-invisible-lines [¶](#index-count_002dlines)
:   This function returns the number of lines between the positions
    start and end in the current buffer. If start and
    end are equal, then it returns 0. Otherwise it returns at least
    1, even if start and end are on the same line. This is
    because the text between them, considered in isolation, must contain at
    least one line unless it is empty.

    If the optional ignore-invisible-lines is non-`nil`,
    invisible lines will not be included in the count.

Command: **count-words** start end [¶](#index-count_002dwords)
:   This function returns the number of words between the positions
    start and end in the current buffer.

    This function can also be called interactively. In that case, it
    prints a message reporting the number of lines, words, and characters
    in the buffer, or in the region if the region is active.

Function: **line-number-at-pos** &optional pos absolute [¶](#index-line_002dnumber_002dat_002dpos)
:   This function returns the line number in the current buffer
    corresponding to the buffer position pos. If pos is
    `nil` or omitted, the current buffer position is used. If
    absolute is `nil`, the default, counting starts at
    `(point-min)`, so the value refers to the contents of the
    accessible portion of the (potentially narrowed) buffer. If
    absolute is non-`nil`, ignore any narrowing and return
    the absolute line number.

Also see the functions `bolp` and `eolp` in [Examining Text Near Point](#Near-Point).
These functions do not move point, but test whether it is already at the
beginning or end of a line.

---

Next: [Moving over Balanced Expressions](#List-Motion), Previous: [Motion by Text Lines](#Text-Lines), Up: [Motion](#Motion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 31.2.5 Motion by Screen Lines [¶](#Motion-by-Screen-Lines)

The line functions in the previous section count text lines, delimited
only by newline characters. By contrast, these functions count screen
lines, which are defined by the way the text appears on the screen. A
text line is a single screen line if it is short enough to fit the width
of the selected window, but otherwise it may occupy several screen
lines.

In some cases, text lines are truncated on the screen rather than
continued onto additional screen lines. In these cases,
`vertical-motion` moves point much like `forward-line`.
See [Truncation](#Truncation).

Because the width of a given string depends on the flags that control
the appearance of certain characters, `vertical-motion` behaves
differently, for a given piece of text, depending on the buffer it is
in, and even on the selected window (because the width, the truncation
flag, and display table may vary between windows). See [Usual Display Conventions](#Usual-Display).

These functions scan text to determine where screen lines break, and
thus take time proportional to the distance scanned.

Function: **vertical-motion** count &optional window cur-col [¶](#index-vertical_002dmotion)
:   This function moves point to the start of the screen line count
    screen lines down from the screen line containing point. If count
    is negative, it moves up instead. If count is zero, point moves
    to the visual start of the current screen line.

    The count argument can be a cons cell, `(cols . lines)`, instead of an integer. Then the function moves by
    lines screen lines, as described for count above, and puts
    point cols columns from the visual start of that screen line.
    The value of cols can be a float, and is interpreted in units of
    the frame’s canonical character width (see [Frame Font](#Frame-Font)); this
    allows specifying accurate horizontal position of point when the
    target screen line uses variable fonts. Note that cols are
    counted from the *visual* start of the line; if the window is
    scrolled horizontally (see [Horizontal Scrolling](#Horizontal-Scrolling)), the column where
    point will end is in addition to the number of columns by which the
    text is scrolled, and if the target line is a continuation line, its
    leftmost column is considered column zero (unlike column-oriented
    functions, see [Counting Columns](#Columns)).

    The return value is the number of screen lines over which point was
    moved. The value may be less in absolute value than count if
    the beginning or end of the buffer was reached.

    The window window is used for obtaining parameters such as the
    width, the horizontal scrolling, and the display table. But
    `vertical-motion` always operates on the current buffer, even if
    window currently displays some other buffer.

    The optional argument cur-col specifies the current column when
    the function is called. This is the window-relative horizontal
    coordinate of point, measured in units of font width of the frame’s
    default face. Providing it speeds up the function, especially in very
    long lines, because the function doesn’t have to go back in the buffer
    in order to determine the current column. Note that cur-col is
    also counted from the visual start of the line.

Function: **count-screen-lines** &optional beg end count-final-newline window [¶](#index-count_002dscreen_002dlines)
:   This function returns the number of screen lines in the text from
    beg to end. The number of screen lines may be different
    from the number of actual lines, due to line continuation, the display
    table, etc. If beg and end are `nil` or omitted,
    they default to the beginning and end of the accessible portion of the
    buffer.

    If the region ends with a newline, that is ignored unless the optional
    third argument count-final-newline is non-`nil`.

    The optional fourth argument window specifies the window for
    obtaining parameters such as width, horizontal scrolling, and so on.
    The default is to use the selected window’s parameters.

    Like `vertical-motion`, `count-screen-lines` always uses the
    current buffer, regardless of which buffer is displayed in
    window. This makes possible to use `count-screen-lines` in
    any buffer, whether or not it is currently displayed in some window.

Command: **move-to-window-line** count [¶](#index-move_002dto_002dwindow_002dline)
:   This function moves point with respect to the text currently displayed
    in the selected window. It moves point to the beginning of the screen
    line count screen lines from the top of the window; zero means
    the topmost line. If count is negative, that specifies a
    position −count lines from the bottom (or the last
    line of the buffer, if the buffer ends above the specified screen
    position); thus, count of −1 specifies the last fully visible
    screen line of the window.

    If count is `nil`, then point moves to the beginning of the
    line in the middle of the window. If the absolute value of count
    is greater than the size of the window, then point moves to the place
    that would appear on that screen line if the window were tall enough.
    This will probably cause the next redisplay to scroll to bring that
    location onto the screen.

    In an interactive call, count is the numeric prefix argument.

    The value returned is the screen line number point has moved to,
    relative to the top line of the window.

Function: **move-to-window-group-line** count [¶](#index-move_002dto_002dwindow_002dgroup_002dline)
:   This function is like `move-to-window-line`, except that when the
    selected window is a part of a group of windows (see [Window Group](#Window-Group)), `move-to-window-group-line` will move to a position with
    respect to the entire group, not just the single window. This
    condition holds when the buffer local variable
    `move-to-window-group-line-function` is set to a function. In
    this case, `move-to-window-group-line` calls the function with
    the argument count, then returns its result.

Function: **compute-motion** from frompos to topos width offsets window [¶](#index-compute_002dmotion)
:   This function scans the current buffer, calculating screen positions.
    It scans the buffer forward from position from, assuming that is
    at screen coordinates frompos, to position to or coordinates
    topos, whichever comes first. It returns the ending buffer
    position and screen coordinates.

    The coordinate arguments frompos and topos are cons cells of
    the form `(hpos . vpos)`.

    The argument width is the number of columns available to display
    text; this affects handling of continuation lines. `nil` means
    the actual number of usable text columns in the window, which is
    equivalent to the value returned by `(window-width window)`.

    The argument offsets is either `nil` or a cons cell of the
    form `(hscroll . tab-offset)`. Here hscroll is
    the number of columns not being displayed at the left margin; most
    callers get this by calling `window-hscroll`. Meanwhile,
    tab-offset is the offset between column numbers on the screen and
    column numbers in the buffer. This can be nonzero in a continuation
    line, when the previous screen lines’ widths do not add up to a multiple
    of `tab-width`. It is always zero in a non-continuation line.

    The window window serves only to specify which display table to
    use. `compute-motion` always operates on the current buffer,
    regardless of what buffer is displayed in window.

    The return value is a list of five elements:

    ```
    (pos hpos vpos prevhpos contin)
    ```

    Here pos is the buffer position where the scan stopped, vpos
    is the vertical screen position, and hpos is the horizontal screen
    position.

    The result prevhpos is the horizontal position one character back
    from pos. The result contin is `t` if the last line
    was continued after (or within) the previous character.

    For example, to find the buffer position of column col of screen line
    line of a certain window, pass the window’s display start location
    as from and the window’s upper-left coordinates as frompos.
    Pass the buffer’s `(point-max)` as to, to limit the scan to
    the end of the accessible portion of the buffer, and pass line and
    col as topos. Here’s a function that does this:

    ```
    (defun coordinates-of-position (col line)
      (car (compute-motion (window-start)
                           '(0 . 0)
                           (point-max)
                           (cons col line)
                           (window-width)
                           (cons (window-hscroll) 0)
                           (selected-window))))
    ```

    When you use `compute-motion` for the minibuffer, you need to use
    `minibuffer-prompt-width` to get the horizontal position of the
    beginning of the first screen line. See [Minibuffer Contents](#Minibuffer-Contents).

---

Next: [Skipping Characters](#Skipping-Characters), Previous: [Motion by Screen Lines](#Screen-Lines), Up: [Motion](#Motion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 31.2.6 Moving over Balanced Expressions [¶](#Moving-over-Balanced-Expressions)

Here are several functions concerned with balanced-parenthesis
expressions (also called *sexps* in connection with moving across
them in Emacs). The syntax table controls how these functions interpret
various characters; see [Syntax Tables](#Syntax-Tables). See [Parsing Expressions](#Parsing-Expressions), for lower-level primitives for scanning sexps or parts of
sexps. For user-level commands, see [Commands for
Editing with Parentheses](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Parentheses) in The GNU Emacs Manual.

Command: **forward-list** &optional arg [¶](#index-forward_002dlist)
:   This function moves forward across arg (default 1) balanced groups of
    parentheses. (Other syntactic entities such as words or paired string
    quotes are ignored.)

Command: **backward-list** &optional arg [¶](#index-backward_002dlist)
:   This function moves backward across arg (default 1) balanced groups of
    parentheses. (Other syntactic entities such as words or paired string
    quotes are ignored.)

Command: **up-list** &optional arg escape-strings no-syntax-crossing [¶](#index-up_002dlist)
:   This function moves forward out of arg (default 1) levels of
    parentheses. A negative argument means move backward but still to a
    less deep spot. If escape-strings is non-`nil` (as it is
    interactively), move out of enclosing strings as well. If
    no-syntax-crossing is non-`nil` (as it is interactively), prefer
    to break out of any enclosing string instead of moving to the start of
    a list broken across multiple strings. On error, location of point is
    unspecified.

Command: **backward-up-list** &optional arg escape-strings no-syntax-crossing [¶](#index-backward_002dup_002dlist)
:   This function is just like `up-list`, but with a negated argument.

Command: **down-list** &optional arg [¶](#index-down_002dlist)
:   This function moves forward into arg (default 1) levels of
    parentheses. A negative argument means move backward but still go
    deeper in parentheses (−arg levels).

Command: **forward-sexp** &optional arg [¶](#index-forward_002dsexp)
:   This function moves forward across arg (default 1) balanced expressions.
    Balanced expressions include both those delimited by parentheses and
    other kinds, such as words and string constants.
    See [Parsing Expressions](#Parsing-Expressions). For example,

    ```
    ---------- Buffer: foo ----------
    (concat∗ "foo " (car x) y z)
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (forward-sexp 3)
         ⇒ nil

    ---------- Buffer: foo ----------
    (concat "foo " (car x) y∗ z)
    ---------- Buffer: foo ----------
    ```

    `forward-sexp` calls the function that is the value of the variable
    `forward-sexp-function`, if that is non-`nil`, to do the
    actual work, passing it the same arguments as those with which the
    command was called. Major modes can define their own functions for
    moving over balanced expressions as appropriate for the mode, and set
    this variable to that function.

Command: **backward-sexp** &optional arg [¶](#index-backward_002dsexp)
:   This function moves backward across arg (default 1) balanced expressions.

Command: **beginning-of-defun** &optional arg [¶](#index-beginning_002dof_002ddefun)
:   This function moves back to the argth beginning of a defun. If
    arg is negative, this actually moves forward, but it still moves
    to the beginning of a defun, not to the end of one. arg defaults
    to 1.

Command: **end-of-defun** &optional arg [¶](#index-end_002dof_002ddefun)
:   This function moves forward to the argth end of a defun. If
    arg is negative, this actually moves backward, but it still moves
    to the end of a defun, not to the beginning of one. arg defaults
    to 1.

User Option: **defun-prompt-regexp** [¶](#index-defun_002dprompt_002dregexp)
:   If non-`nil`, this buffer-local variable holds a regular
    expression that specifies what text can appear before the
    open-parenthesis that starts a defun. That is to say, a defun begins
    on a line that starts with a match for this regular expression,
    followed by a character with open-parenthesis syntax.

User Option: **open-paren-in-column-0-is-defun-start** [¶](#index-open_002dparen_002din_002dcolumn_002d0_002dis_002ddefun_002dstart)
:   If this variable’s value is non-`nil`, an open parenthesis in
    column 0 is considered to be the start of a defun. If it is
    `nil`, an open parenthesis in column 0 has no special meaning.
    The default is `t`. If a string literal happens to have a
    parenthesis in column 0, escape it with a backslash to avoid a false
    positive.

Variable: **beginning-of-defun-function** [¶](#index-beginning_002dof_002ddefun_002dfunction)
:   If non-`nil`, this variable holds a function for finding the
    beginning of a defun. The function `beginning-of-defun`
    calls this function instead of using its normal method, passing it its
    optional argument. If the argument is non-`nil`, the function
    should move back by that many functions, like
    `beginning-of-defun` does.

Variable: **end-of-defun-function** [¶](#index-end_002dof_002ddefun_002dfunction)
:   If non-`nil`, this variable holds a function for finding the end of
    a defun. The function `end-of-defun` calls this function instead
    of using its normal method.

If Emacs is compiled with tree-sitter, it can use the tree-sitter
parser information to move across syntax constructs. Since what
exactly is considered a defun varies between languages, a major mode
should set `treesit-defun-type-regexp` to determine that. Then
the mode can get navigation-by-defun functionality for free, by using
`treesit-beginning-of-defun` and `treesit-end-of-defun`.

Variable: **treesit-defun-type-regexp** [¶](#index-treesit_002ddefun_002dtype_002dregexp)
:   This variable determines which nodes are considered defuns by Emacs.
    It can be a regexp that matches the type of defun nodes. (For
    “node” and “node type”, see [Parsing Program Source](#Parsing-Program-Source).)

    For example, `python-mode` sets this variable to a regexp that
    matches either ‘`function_definition`’ or ‘`class_definition`’.

    Sometimes not all nodes matched by the regexp are valid defuns.
    Therefore, this variable can also be a cons cell of the form
    (regexp . pred), where pred should be a function
    that takes a node as its argument, and returns non-`nil` if the
    node is a valid defun, or `nil` if it is not valid.

Variable: **treesit-defun-tactic** [¶](#index-treesit_002ddefun_002dtactic)
:   This variable determines how Emacs treats nested defuns. If the value
    is `top-level`, navigation functions only move across top-level
    defuns. If the value is `nested`, navigation functions recognize
    nested defuns.

The function that is the value of the variable
`forward-sentence-function` determines how to move across syntax
constructs known as *sentences*. Major modes can assign their own
functions to this variable to customize the behavior of
`forward-sentence` command. If Emacs is compiled with tree-sitter,
it can use the tree-sitter parser information to move across syntax
constructs. Since what exactly is considered a sentence varies between
languages, a major mode should set `treesit-thing-settings` to
determine that. Then `forward-sentence-function` will be set to
`treesit-forward-sentence`, and the mode will get
navigation-by-sentence functionality for free, by using
`forward-sentence` and `backward-sentence`(see [Moving by
Sentences](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Moving-by-Sentences) in The extensible self-documenting text editor).

If Emacs is compiled with tree-sitter, it can use the tree-sitter
parser information to move across syntax constructs. Since what
exactly is considered a sexp varies between languages, a major mode
should set `treesit-thing-settings` to determine that. Then
`forward-sexp-function` will be set to `treesit-forward-sexp`,
and the mode can get navigation-by-sexp functionality for free, by using
`forward-sexp` and `backward-sexp`(see [Expressions](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Expressions) in The extensible self-documenting text editor).

---

Previous: [Moving over Balanced Expressions](#List-Motion), Up: [Motion](#Motion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 31.2.7 Skipping Characters [¶](#Skipping-Characters-1)

The following two functions move point over a specified set of
characters. For example, they are often used to skip whitespace. For
related functions, see [Motion and Syntax](#Motion-and-Syntax).

These functions convert the set string to multibyte if the buffer is
multibyte, and they convert it to unibyte if the buffer is unibyte, as
the search functions do (see [Searching and Matching](#Searching-and-Matching)).

Function: **skip-chars-forward** character-set &optional limit [¶](#index-skip_002dchars_002dforward)
:   This function moves point in the current buffer forward, skipping over a
    given set of characters. It examines the character following point,
    then advances point if the character matches character-set. This
    continues until it reaches a character that does not match. The
    function returns the number of characters moved over.

    The argument character-set is a string, like the inside of a
    ‘`[…]`’ in a regular expression except that ‘`]`’ does not
    terminate it, and ‘`\`’ quotes ‘`^`’, ‘`-`’ or ‘`\`’.
    Thus, `"a-zA-Z"` skips over all letters, stopping before the
    first nonletter, and `"^a-zA-Z"` skips nonletters stopping before
    the first letter (see [Regular Expressions](#Regular-Expressions)). Character classes
    can also be used, e.g., `"[:alnum:]"` (see [Character Classes](#Char-Classes)).

    If limit is supplied (it must be a number or a marker), it
    specifies the maximum position in the buffer that point can be skipped
    to. Point will stop at or before limit.

    In the following example, point is initially located directly before the
    ‘`T`’. After the form is evaluated, point is located at the end of
    that line (between the ‘`t`’ of ‘`hat`’ and the newline). The
    function skips all letters and spaces, but not newlines.

    ```
    ---------- Buffer: foo ----------
    I read "∗The cat in the hat
    comes back" twice.
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (skip-chars-forward "a-zA-Z ")
         ⇒ 18

    ---------- Buffer: foo ----------
    I read "The cat in the hat∗
    comes back" twice.
    ---------- Buffer: foo ----------
    ```

Function: **skip-chars-backward** character-set &optional limit [¶](#index-skip_002dchars_002dbackward)
:   This function moves point backward, skipping characters that match
    character-set, until limit. It is just like
    `skip-chars-forward` except for the direction of motion.

    The return value indicates the distance traveled. It is an integer that
    is zero or less.

---

Next: [Narrowing](#Narrowing), Previous: [Motion](#Motion), Up: [Positions](#Positions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 31.3 Excursions [¶](#Excursions-1)

It is often useful to move point temporarily within a localized
portion of the program. This is called an *excursion*, and it is
done with the `save-excursion` special form. This construct
remembers the initial identity of the current buffer, and its value
of point, and restores them after the excursion
completes. It is the standard way to move point within one part of a
program and avoid affecting the rest of the program, and is used
thousands of times in the Lisp sources of Emacs.

If you only need to save and restore the identity of the current
buffer, use `save-current-buffer` or `with-current-buffer`
instead (see [The Current Buffer](#Current-Buffer)). If you need to save or restore
window configurations, see the forms described in [Window Configurations](#Window-Configurations) and in [Frame Configurations](#Frame-Configurations).

Special Form: **save-excursion** body… [¶](#index-save_002dexcursion)
:   This special form saves the identity of the current buffer and the
    value of point in it, evaluates body, and finally
    restores the buffer and its saved value of point. Both saved values are
    restored even in case of an abnormal exit via
    `throw` or error (see [Nonlocal Exits](#Nonlocal-Exits)).

    The value returned by `save-excursion` is the result of the last
    form in body, or `nil` if no body forms were given.

Because `save-excursion` only saves point for the
buffer that was current at the start of the excursion, any changes
made to point in other buffers, during the excursion, will
remain in effect afterward. This frequently leads to unintended
consequences, so the byte compiler warns if you call `set-buffer`
during an excursion:

```
Warning: Use ‘with-current-buffer’ rather than
         save-excursion+set-buffer
```

To avoid such problems, you should call `save-excursion` only
after setting the desired current buffer, as in the following example:

```
(defun append-string-to-buffer (string buffer)
  "Append STRING to the end of BUFFER."
  (with-current-buffer buffer
    (save-excursion
      (goto-char (point-max))
      (insert string))))
```

Likewise, `save-excursion` does not restore window-buffer
correspondences altered by functions such as `switch-to-buffer`.

**Warning:** Ordinary insertion of text adjacent to the saved
point value relocates the saved value, just as it relocates all
markers. More precisely, the saved value is a marker with insertion
type `nil`. See [Marker Insertion Types](#Marker-Insertion-Types). Therefore, when the
saved point value is restored, it normally comes before the inserted
text.

Macro: **save-mark-and-excursion** body… [¶](#index-save_002dmark_002dand_002dexcursion)
:   This macro is like `save-excursion`, but also saves and restores
    the mark location and `mark-active`. This macro does what
    `save-excursion` did before Emacs 25.1.

---

Previous: [Excursions](#Excursions), Up: [Positions](#Positions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 31.4 Narrowing [¶](#Narrowing-1)

*Narrowing* means limiting the text addressable by Emacs editing
commands to a limited range of characters in a buffer. The text that
remains addressable is called the *accessible portion* of the
buffer.

Narrowing is specified with two buffer positions, which become the
beginning and end of the accessible portion. For most editing
commands and primitives, these positions replace the values of the
beginning and end of the buffer. While narrowing is in effect, no
text outside the accessible portion is displayed, and point cannot
move outside the accessible portion. Note that narrowing does not
alter actual buffer positions (see [Point](#Point)); it only determines
which positions are considered the accessible portion of the buffer.
Most functions refuse to operate on text that is outside the
accessible portion.

Commands for saving buffers are unaffected by narrowing; they save
the entire buffer regardless of any narrowing.

If you need to display in a single buffer several very different
types of text, consider using an alternative facility described in
[Swapping Text Between Two Buffers](#Swapping-Text).

Command: **narrow-to-region** start end [¶](#index-narrow_002dto_002dregion)
:   This function sets the accessible portion of the current buffer to start
    at start and end at end. Both arguments should be character
    positions.

    In an interactive call, start and end are set to the bounds
    of the current region (point and the mark, with the smallest first).

    However, when the narrowing has been set by `with-restriction` with
    a label argument (see below), `narrow-to-region` can be used only
    within the limits of that narrowing. If start or end are
    outside these limits, the corresponding limit set by
    `with-restriction` is used instead. To gain access to other
    portions of the buffer, use `without-restriction` with the same
    label.

Command: **narrow-to-page** &optional move-count [¶](#index-narrow_002dto_002dpage)
:   This function sets the accessible portion of the current buffer to
    include just the current page. An optional first argument
    move-count non-`nil` means to move forward or backward by
    move-count pages and then narrow to one page. The variable
    `page-delimiter` specifies where pages start and end
    (see [Standard Regular Expressions Used in Editing](#Standard-Regexps)).

    In an interactive call, move-count is set to the numeric prefix
    argument.

Command: **widen** [¶](#index-widen)
:   This function cancels any narrowing in the current buffer, so that the
    entire contents are accessible. This is called *widening*.
    It is equivalent to the following expression:

    ```
    (narrow-to-region 1 (1+ (buffer-size)))
    ```

    However, when a narrowing has been set by `with-restriction` with a
    label argument (see below), the limits set by `with-restriction`
    are restored, instead of canceling the narrowing. To gain access to
    other portions of the buffer, use `without-restriction` with the
    same label.

Function: **buffer-narrowed-p** [¶](#index-buffer_002dnarrowed_002dp)
:   This function returns non-`nil` if the buffer is narrowed, and
    `nil` otherwise.

Special Form: **save-restriction** body… [¶](#index-save_002drestriction)
:   This special form saves the current bounds of the accessible portion,
    evaluates the body forms, and finally restores the saved bounds,
    thus restoring the same state of narrowing (or absence thereof) formerly
    in effect. The state of narrowing is restored even in the event of an
    abnormal exit via `throw` or error (see [Nonlocal Exits](#Nonlocal-Exits)).
    Therefore, this construct is a clean way to narrow a buffer temporarily.

    This construct also saves and restores the narrowings that were set by
    `with-restriction` with a label argument (see below).

    The value returned by `save-restriction` is that returned by the
    last form in body, or `nil` if no body forms were given.

    **Caution:** it is easy to make a mistake when using the
    `save-restriction` construct. Read the entire description here
    before you try it.

    If body changes the current buffer, `save-restriction` still
    restores the restrictions on the original buffer (the buffer whose
    restrictions it saved from), but it does not restore the identity of the
    current buffer.

    `save-restriction` does *not* restore point; use
    `save-excursion` for that. If you use both `save-restriction`
    and `save-excursion` together, `save-excursion` should come
    first (on the outside). Otherwise, the old point value would be
    restored with temporary narrowing still in effect. If the old point
    value were outside the limits of the temporary narrowing, this would
    fail to restore it accurately.

    Here is a simple example of correct use of `save-restriction`:

    ```
    ---------- Buffer: foo ----------
    This is the contents of foo
    This is the contents of foo
    This is the contents of foo∗
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (save-excursion
      (save-restriction
        (goto-char 1)
        (forward-line 2)
        (narrow-to-region 1 (point))
        (goto-char (point-min))
        (replace-string "foo" "bar")))

    ---------- Buffer: foo ----------
    This is the contents of bar
    This is the contents of bar
    This is the contents of foo∗
    ---------- Buffer: foo ----------
    ```

Special Form: **with-restriction** start end [:label label] body [¶](#index-with_002drestriction)
:   This special form saves the current bounds of the accessible portion
    of the buffer, sets the accessible portion to start at start and
    end at end, evaluates the body forms, and restores the
    saved bounds. In that case it is equivalent to

    ```
    (save-restriction
      (narrow-to-region start end)
      body)
    ```

    When the optional argument label, which is evaluated to get the
    label to use and must yield a non-`nil` value, is present, the
    narrowing is *labeled*. A labeled narrowing differs from a
    non-labeled one in several ways:

    * During the evaluation of the body form, `narrow-to-region`
      and `widen` can be used only within the start and end
      limits.
    * To lift the restriction introduced by `with-restriction` and gain
      access to other portions of the buffer, use `without-restriction`
      with the same label argument. (Another way to gain access to
      other portions of the buffer is to use an indirect buffer
      (see [Indirect Buffers](#Indirect-Buffers)).)
    * Labeled narrowings can be nested.
    * Labeled narrowings can only be used in Lisp programs: they are never
      visible on display, and never interfere with narrowings set by the
      user.

    If you use `with-restriction` with the optional label
    argument, we recommend documenting the label in the doc strings
    of the functions which use it, so that other Lisp programs your code
    calls could lift the labeled narrowing if and when it needs.

Special Form: **without-restriction** [:label label] body [¶](#index-without_002drestriction)
:   This special form saves the current bounds of the accessible portion
    of the buffer, widens the buffer, evaluates the body forms, and
    restores the saved bounds. In that case it is equivalent to

    ```
    (save-restriction
      (widen)
      body)
    ```

    When the optional argument label is present, the narrowing set
    by `with-restriction` with the same label argument is
    lifted.

---

Next: [Text](#Text), Previous: [Positions](#Positions), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 32 Markers [¶](#Markers-1)

A *marker* is a Lisp object used to specify a position in a buffer
relative to the surrounding text. A marker changes its offset from the
beginning of the buffer automatically whenever text is inserted or
deleted, so that it stays with the two characters on either side of it.

* [Overview of Markers](#Overview-of-Markers)
* [Predicates on Markers](#Predicates-on-Markers)
* [Functions that Create Markers](#Creating-Markers)
* [Information from Markers](#Information-from-Markers)
* [Marker Insertion Types](#Marker-Insertion-Types)
* [Moving Marker Positions](#Moving-Markers)
* [The Mark](#The-Mark)
* [The Region](#The-Region)

---

Next: [Predicates on Markers](#Predicates-on-Markers), Up: [Markers](#Markers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 32.1 Overview of Markers [¶](#Overview-of-Markers-1)

A marker specifies a buffer and a position in that buffer. A
marker can be used to represent a position in functions that
require one, just as an integer could be used. In that case, the
marker’s buffer is normally ignored. Of course, a marker used in this
way usually points to a position in the buffer that the function
operates on, but that is entirely the programmer’s responsibility.
See [Positions](#Positions), for a complete description of positions.

A marker has three attributes: the marker position, the marker
buffer, and the insertion type. The marker position is an integer
that is equivalent (at a given time) to the marker as a position in
that buffer. But the marker’s position value can change during
the life of the marker, and often does. Insertion and deletion of
text in the buffer relocate the marker. The idea is that a marker
positioned between two characters remains between those two characters
despite insertion and deletion elsewhere in the buffer. Relocation
changes the integer equivalent of the marker.

Deleting text around a marker’s position leaves the marker between the
characters immediately before and after the deleted text. Inserting
text at the position of a marker normally leaves the marker either in
front of or after the new text, depending on the marker’s *insertion
type* (see [Marker Insertion Types](#Marker-Insertion-Types))—unless the insertion is done
with `insert-before-markers` (see [Inserting Text](#Insertion)).

Insertion and deletion in a buffer must check all the markers and
relocate them if necessary. This slows processing in a buffer with a
large number of markers. For this reason, it is a good idea to make a
marker point nowhere if you are sure you don’t need it any more.
Markers that can no longer be accessed are eventually removed
(see [Garbage Collection](#Garbage-Collection)).

Because it is common to perform arithmetic operations on a marker
position, most of these operations (including `+` and
`-`) accept markers as arguments. In such cases, the marker
stands for its current position.

Here are examples of creating markers, setting markers, and moving point
to markers:

```
;; Make a new marker that initially does not point anywhere:
(setq m1 (make-marker))
     ⇒ #<marker in no buffer>
```

```

```

```
;; Set m1 to point between the 99th and 100th characters
;;   in the current buffer:
(set-marker m1 100)
     ⇒ #<marker at 100 in markers.texi>
```

```

```

```
;; Now insert one character at the beginning of the buffer:
(goto-char (point-min))
     ⇒ 1
(insert "Q")
     ⇒ nil
```

```

```

```
;; m1 is updated appropriately.
m1
     ⇒ #<marker at 101 in markers.texi>
```

```

```

```
;; Two markers that point to the same position
;;   are not eq, but they are equal.
(setq m2 (copy-marker m1))
     ⇒ #<marker at 101 in markers.texi>
(eq m1 m2)
     ⇒ nil
(equal m1 m2)
     ⇒ t
```

```

```

```
;; When you are finished using a marker, make it point nowhere.
(set-marker m1 nil)
     ⇒ #<marker in no buffer>
```

---

Next: [Functions that Create Markers](#Creating-Markers), Previous: [Overview of Markers](#Overview-of-Markers), Up: [Markers](#Markers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 32.2 Predicates on Markers [¶](#Predicates-on-Markers-1)

You can test an object to see whether it is a marker, or whether it is
either an integer or a marker. The latter test is useful in connection
with the arithmetic functions that work with both markers and integers.

Function: **markerp** object [¶](#index-markerp)
:   This function returns `t` if object is a marker, `nil`
    otherwise. Note that integers are not markers, even though many
    functions will accept either a marker or an integer.

Function: **integer-or-marker-p** object [¶](#index-integer_002dor_002dmarker_002dp)
:   This function returns `t` if object is an integer or a marker,
    `nil` otherwise.

Function: **number-or-marker-p** object [¶](#index-number_002dor_002dmarker_002dp)
:   This function returns `t` if object is a number (either
    integer or floating point) or a marker, `nil` otherwise.

---

Next: [Information from Markers](#Information-from-Markers), Previous: [Predicates on Markers](#Predicates-on-Markers), Up: [Markers](#Markers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 32.3 Functions that Create Markers [¶](#Functions-that-Create-Markers)

When you create a new marker, you can make it point nowhere, or point
to the present position of point, or to the beginning or end of the
accessible portion of the buffer, or to the same place as another given
marker.

The next four functions all return markers with insertion type
`nil`. See [Marker Insertion Types](#Marker-Insertion-Types).

Function: **make-marker** [¶](#index-make_002dmarker)
:   This function returns a newly created marker that does not point
    anywhere.

    ```
    (make-marker)
         ⇒ #<marker in no buffer>
    ```

Function: **point-marker** [¶](#index-point_002dmarker)
:   This function returns a new marker that points to the present position
    of point in the current buffer. See [Point](#Point). For an example, see
    `copy-marker`, below.

Function: **point-min-marker** [¶](#index-point_002dmin_002dmarker)
:   This function returns a new marker that points to the beginning of the
    accessible portion of the buffer. This will be the beginning of the
    buffer unless narrowing is in effect. See [Narrowing](#Narrowing).

Function: **point-max-marker** [¶](#index-point_002dmax_002dmarker)
:   This function returns a new marker that points to the end of the
    accessible portion of the buffer. This will be the end of the buffer
    unless narrowing is in effect. See [Narrowing](#Narrowing).

    Here are examples of this function and `point-min-marker`, shown in
    a buffer containing a version of the source file for the text of this
    chapter.

    ```
    (point-min-marker)
         ⇒ #<marker at 1 in markers.texi>
    (point-max-marker)
         ⇒ #<marker at 24080 in markers.texi>
    ```

    ```

    ```

    ```
    (narrow-to-region 100 200)
         ⇒ nil
    ```

    ```
    (point-min-marker)
         ⇒ #<marker at 100 in markers.texi>
    ```

    ```
    (point-max-marker)
         ⇒ #<marker at 200 in markers.texi>
    ```

Function: **copy-marker** &optional marker-or-integer insertion-type [¶](#index-copy_002dmarker)
:   If passed a marker as its argument, `copy-marker` returns a
    new marker that points to the same place and the same buffer as does
    marker-or-integer. If passed an integer as its argument,
    `copy-marker` returns a new marker that points to position
    marker-or-integer in the current buffer.

    The new marker’s insertion type is specified by the argument
    insertion-type. See [Marker Insertion Types](#Marker-Insertion-Types).

    ```
    (copy-marker 0)
         ⇒ #<marker at 1 in markers.texi>
    ```

    ```

    ```

    ```
    (copy-marker 90000)
         ⇒ #<marker at 24080 in markers.texi>
    ```

    An error is signaled if marker is neither a marker nor an
    integer.

Two distinct markers are considered `equal` (even though not
`eq`) to each other if they have the same position and buffer, or
if they both point nowhere.

```
(setq p (point-marker))
     ⇒ #<marker at 2139 in markers.texi>
```

```

```

```
(setq q (copy-marker p))
     ⇒ #<marker at 2139 in markers.texi>
```

```

```

```
(eq p q)
     ⇒ nil
```

```

```

```
(equal p q)
     ⇒ t
```

---

Next: [Marker Insertion Types](#Marker-Insertion-Types), Previous: [Functions that Create Markers](#Creating-Markers), Up: [Markers](#Markers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 32.4 Information from Markers [¶](#Information-from-Markers-1)

This section describes the functions for accessing the components of a
marker object.

Function: **marker-position** marker [¶](#index-marker_002dposition)
:   This function returns the position that marker points to, or
    `nil` if it points nowhere.

Function: **marker-last-position** marker [¶](#index-marker_002dlast_002dposition)
:   This function returns the last known position of marker in its
    buffer. It behaves like `marker-position` with one exception: if
    the buffer of marker has been killed, it returns the last position
    of marker in that buffer before the buffer was killed, instead of
    returning `nil`.

Function: **marker-buffer** marker [¶](#index-marker_002dbuffer)
:   This function returns the buffer that marker points into, or
    `nil` if it points nowhere.

    ```
    (setq m (make-marker))
         ⇒ #<marker in no buffer>
    ```

    ```
    (marker-position m)
         ⇒ nil
    ```

    ```
    (marker-buffer m)
         ⇒ nil
    ```

    ```

    ```

    ```
    (set-marker m 3770 (current-buffer))
         ⇒ #<marker at 3770 in markers.texi>
    ```

    ```
    (marker-buffer m)
         ⇒ #<buffer markers.texi>
    ```

    ```
    (marker-position m)
         ⇒ 3770
    ```

---

Next: [Moving Marker Positions](#Moving-Markers), Previous: [Information from Markers](#Information-from-Markers), Up: [Markers](#Markers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 32.5 Marker Insertion Types [¶](#Marker-Insertion-Types-1)

When you insert text directly at the place where a marker points,
there are two possible ways to relocate that marker: it can point before
the inserted text, or point after it. You can specify which one a given
marker should do by setting its *insertion type*. Note that use of
`insert-before-markers` ignores markers’ insertion types, always
relocating a marker to point after the inserted text.

Function: **set-marker-insertion-type** marker type [¶](#index-set_002dmarker_002dinsertion_002dtype)
:   This function sets the insertion type of marker marker to
    type. If type is `t`, marker will advance when
    text is inserted at its position. If type is `nil`,
    marker does not advance when text is inserted there.

Function: **marker-insertion-type** marker [¶](#index-marker_002dinsertion_002dtype)
:   This function reports the current insertion type of marker.

All functions that create markers without accepting an argument that
specifies the insertion type, create them with insertion type
`nil` (see [Functions that Create Markers](#Creating-Markers)). Also, the mark has, by
default, insertion type `nil`.

---

Next: [The Mark](#The-Mark), Previous: [Marker Insertion Types](#Marker-Insertion-Types), Up: [Markers](#Markers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 32.6 Moving Marker Positions [¶](#Moving-Marker-Positions)

This section describes how to change the position of an existing
marker. When you do this, be sure you know whether the marker is used
outside of your program, and, if so, what effects will result from
moving it—otherwise, confusing things may happen in other parts of
Emacs.

Function: **set-marker** marker position &optional buffer [¶](#index-set_002dmarker)
:   This function moves marker to position
    in buffer. If buffer is not provided, it defaults to
    the current buffer.

    If position is `nil` or a marker that points nowhere, then
    marker is set to point nowhere.

    The value returned is marker.

    ```
    (setq m (point-marker))
         ⇒ #<marker at 4714 in markers.texi>
    ```

    ```
    (set-marker m 55)
         ⇒ #<marker at 55 in markers.texi>
    ```

    ```
    (setq b (get-buffer "foo"))
         ⇒ #<buffer foo>
    ```

    ```
    (set-marker m 0 b)
         ⇒ #<marker at 1 in foo>
    ```

Function: **move-marker** marker position &optional buffer [¶](#index-move_002dmarker)
:   This is another name for `set-marker`.

---

Next: [The Region](#The-Region), Previous: [Moving Marker Positions](#Moving-Markers), Up: [Markers](#Markers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 32.7 The Mark [¶](#The-Mark-1)

Each buffer has a special marker, which is designated *the
mark*. When a buffer is newly created, this marker exists but does
not point anywhere; this means that the mark doesn’t exist in that
buffer yet. Subsequent commands can set the mark.

The mark specifies a position to bound a range of text for many
commands, such as `kill-region` and `indent-rigidly`. These
commands typically act on the text between point and the mark, which
is called the *region*. If you are writing a command that
operates on the region, don’t examine the mark directly; instead, use
`interactive` with the ‘`r`’ specification. This provides the
values of point and the mark as arguments to the command in an
interactive call, but permits other Lisp programs to specify arguments
explicitly. See [Code Characters for `interactive`](#Interactive-Codes).

Some commands set the mark as a side-effect. Commands should do
this only if it has a potential use to the user, and never for their
own internal purposes. For example, the `replace-regexp` command
sets the mark to the value of point before doing any replacements,
because this enables the user to move back there conveniently after
the replace is finished.

Once the mark exists in a buffer, it normally never ceases to
exist. However, it may become *inactive*, if Transient Mark mode
is enabled. The buffer-local variable `mark-active`, if
non-`nil`, means that the mark is active. A command can call the
function `deactivate-mark` to deactivate the mark directly, or it
can request deactivation of the mark upon return to the editor command
loop by setting the variable `deactivate-mark` to a
non-`nil` value.

If Transient Mark mode is enabled, certain editing commands that
normally apply to text near point, apply instead to the region when
the mark is active. This is the main motivation for using Transient
Mark mode. (Another is that this enables highlighting of the region
when the mark is active. See [Emacs Display](#Display).)

In addition to the mark, each buffer has a *mark ring* which is a
list of markers containing previous values of the mark. When editing
commands change the mark, they should normally save the old value of the
mark on the mark ring. The variable `mark-ring-max` specifies the
maximum number of entries in the mark ring; once the list becomes this
long, adding a new element deletes the last element.

There is also a separate global mark ring, but that is used only in a
few particular user-level commands, and is not relevant to Lisp
programming. So we do not describe it here.

Function: **mark** &optional force [¶](#index-mark)
:   This function returns the current buffer’s mark position as an integer,
    or `nil` if no mark has ever been set in this buffer.

    If Transient Mark mode is enabled, and `mark-even-if-inactive` is
    `nil`, `mark` signals an error if the mark is inactive.
    However, if force is non-`nil`, then `mark` disregards
    inactivity of the mark, and returns the mark position (or `nil`)
    anyway.

Function: **mark-marker** [¶](#index-mark_002dmarker)
:   This function returns the marker that represents the current buffer’s
    mark. It is not a copy, it is the marker used internally. Therefore,
    changing this marker’s position will directly affect the buffer’s
    mark. Don’t do that unless that is the effect you want.

    ```
    (setq m (mark-marker))
         ⇒ #<marker at 3420 in markers.texi>
    ```

    ```
    (set-marker m 100)
         ⇒ #<marker at 100 in markers.texi>
    ```

    ```
    (mark-marker)
         ⇒ #<marker at 100 in markers.texi>
    ```

    Like any marker, this marker can be set to point at any buffer you
    like. If you make it point at any buffer other than the one of which
    it is the mark, it will yield perfectly consistent, but rather odd,
    results. We recommend that you not do it!

Function: **set-mark** position [¶](#index-set_002dmark)
:   This function sets the mark to position, and activates the mark.
    The old value of the mark is *not* pushed onto the mark ring.

    **Please note:** Use this function only if you want the user to
    see that the mark has moved, and you want the previous mark position to
    be lost. Normally, when a new mark is set, the old one should go on the
    `mark-ring`. For this reason, most applications should use
    `push-mark` and `pop-mark`, not `set-mark`.

    Novice Emacs Lisp programmers often try to use the mark for the wrong
    purposes. The mark saves a location for the user’s convenience. An
    editing command should not alter the mark unless altering the mark is
    part of the user-level functionality of the command. (And, in that
    case, this effect should be documented.) To remember a location for
    internal use in the Lisp program, store it in a Lisp variable. For
    example:

    ```
    (let ((beg (point)))
      (forward-line 1)
      (delete-region beg (point))).
    ```

Function: **push-mark** &optional position nomsg activate [¶](#index-push_002dmark)
:   This function sets the current buffer’s mark to position, and
    pushes a copy of the previous mark onto `mark-ring`. If
    position is `nil`, then the value of point is used.

    The function `push-mark` normally *does not* activate the
    mark. To do that, specify `t` for the argument activate.

    A ‘`Mark set`’ message is displayed unless nomsg is
    non-`nil`.

Function: **pop-mark** [¶](#index-pop_002dmark)
:   This function pops off the top element of `mark-ring` and makes
    that mark become the buffer’s actual mark. This does not move point in
    the buffer, and it does nothing if `mark-ring` is empty. It
    deactivates the mark.

User Option: **transient-mark-mode** [¶](#index-transient_002dmark_002dmode)
:   This variable, if non-`nil`, enables Transient Mark mode. In
    Transient Mark mode, every buffer-modifying primitive sets
    `deactivate-mark`. As a consequence, most commands that modify
    the buffer also deactivate the mark.

    When Transient Mark mode is enabled and the mark is active, many
    commands that normally apply to the text near point instead apply to
    the region. Such commands should use the function `use-region-p`
    to test whether they should operate on the region. See [The Region](#The-Region).

    Lisp programs can set `transient-mark-mode` to non-`nil`,
    non-`t` values to enable Transient Mark mode temporarily. If the
    value is `lambda`, Transient Mark mode is automatically turned
    off after any action, such as buffer modification, that would normally
    deactivate the mark. If the value is `(only . oldval)`,
    then `transient-mark-mode` is set to the value oldval after
    any subsequent command that moves point and is not shift-translated
    (see [shift-translation](#Key-Sequence-Input)), or after any other
    action that would normally deactivate the mark. (Marking a region
    with the mouse will temporarily enable `transient-mark-mode` in
    this way.)

User Option: **mark-even-if-inactive** [¶](#index-mark_002deven_002dif_002dinactive)
:   If this is non-`nil`, Lisp programs and the Emacs user can use the
    mark even when it is inactive. This option affects the behavior of
    Transient Mark mode. When the option is non-`nil`, deactivation of
    the mark turns off region highlighting, but commands that use the mark
    behave as if the mark were still active.

Variable: **deactivate-mark** [¶](#index-deactivate_002dmark-1)
:   If an editor command sets this variable non-`nil`, then the editor
    command loop deactivates the mark after the command returns (if
    Transient Mark mode is enabled). All the primitives that change the
    buffer set `deactivate-mark`, to deactivate the mark when the
    command is finished. Setting this variable makes it buffer-local.

    To write Lisp code that modifies the buffer without causing
    deactivation of the mark at the end of the command, bind
    `deactivate-mark` to `nil` around the code that does the
    modification. For example:

    ```
    (let (deactivate-mark)
      (insert " "))
    ```

Function: **deactivate-mark** &optional force [¶](#index-deactivate_002dmark)
:   If Transient Mark mode is enabled or force is non-`nil`,
    this function deactivates the mark and runs the normal hook
    `deactivate-mark-hook`. Otherwise, it does nothing.

Variable: **mark-active** [¶](#index-mark_002dactive)
:   The mark is active when this variable is non-`nil`. This
    variable is always buffer-local in each buffer. Do *not* use the
    value of this variable to decide whether a command that normally
    operates on text near point should operate on the region instead. Use
    the function `use-region-p` for that (see [The Region](#The-Region)).

Variable: **activate-mark-hook** [¶](#index-activate_002dmark_002dhook)

Variable: **deactivate-mark-hook** [¶](#index-deactivate_002dmark_002dhook)
:   These normal hooks are run, respectively, when the mark becomes active
    and when it becomes inactive. The hook `activate-mark-hook` is
    also run when the region is reactivated, for instance after using a
    command that switches back to a buffer that has an active mark.

Function: **handle-shift-selection** [¶](#index-handle_002dshift_002dselection)
:   This function implements the shift-selection behavior of
    point-motion commands. See [Shift Selection](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Shift-Selection) in The GNU Emacs
    Manual. It is called automatically by the Emacs command loop
    whenever a command with a ‘`^`’ character in its `interactive`
    spec is invoked, before the command itself is executed
    (see [^](#Interactive-Codes)).

    If `shift-select-mode` is non-`nil` and the current command
    was invoked via shift translation (see [shift-translation](#Key-Sequence-Input)), this function sets the mark and temporarily
    activates the region, unless the region was already temporarily
    activated in this way. Otherwise, if the region has been activated
    temporarily, it deactivates the mark and restores the variable
    `transient-mark-mode` to its earlier value.

Variable: **mark-ring** [¶](#index-mark_002dring)
:   The value of this buffer-local variable is the list of saved former
    marks of the current buffer, most recent first.

    ```
    mark-ring
    ⇒ (#<marker at 11050 in markers.texi>
        #<marker at 10832 in markers.texi>
        ...)
    ```

User Option: **mark-ring-max** [¶](#index-mark_002dring_002dmax)
:   The value of this variable is the maximum size of `mark-ring`. If
    more marks than this are pushed onto the `mark-ring`,
    `push-mark` discards an old mark when it adds a new one.

When Delete Selection mode (see [Delete Selection](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Using-Region) in The GNU Emacs Manual) is enabled, commands that operate on the
active region (a.k.a. “selection”) behave slightly differently.
This works by adding the function `delete-selection-pre-hook` to
the `pre-command-hook` (see [Command Loop Overview](#Command-Overview)). That function
calls `delete-selection-helper` to delete the selection as
appropriate for the command. If you want to adapt a command to Delete
Selection mode, put the `delete-selection` property on the
function’s symbol (see [Accessing Symbol Properties](#Symbol-Plists)); commands that don’t have
this property on their symbol won’t delete the selection. This
property can have one of several values to tailor the behavior to what
the command is supposed to do; see the doc strings of
`delete-selection-pre-hook` and `delete-selection-helper`
for the details.

---

Previous: [The Mark](#The-Mark), Up: [Markers](#Markers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 32.8 The Region [¶](#The-Region-1)

The text between point and the mark is known as *the region*.
Various functions operate on text delimited by point and the mark, but
only those functions specifically related to the region itself are
described here.

The next two functions signal an error if the mark does not point
anywhere. If Transient Mark mode is enabled and
`mark-even-if-inactive` is `nil`, they also signal an error
if the mark is inactive.

Function: **region-beginning** [¶](#index-region_002dbeginning)
:   This function returns the position of the beginning of the region (as
    an integer). This is the position of either point or the mark,
    whichever is smaller.

Function: **region-end** [¶](#index-region_002dend)
:   This function returns the position of the end of the region (as an
    integer). This is the position of either point or the mark, whichever is
    larger.

Instead of using `region-beginning` and `region-end`, a
command designed to operate on a region should normally use
`interactive` with either a ‘`r`’ or ‘`R`’ code letter to
find the beginning and end of the region. This lets other Lisp programs
specify the bounds explicitly as arguments. See [Code Characters for `interactive`](#Interactive-Codes).

Function: **use-region-p** [¶](#index-use_002dregion_002dp)
:   This function returns `t` if Transient Mark mode is enabled, the
    mark is active, and there is a valid region in the buffer. This
    function is intended to be used by commands that operate on the
    region, instead of on text near point, when the mark is active.

    A region is valid if it has a non-zero size, or if the user option
    `use-empty-active-region` is non-`nil` (by default, it is
    `nil`). The function `region-active-p` is similar to
    `use-region-p`, but considers all regions as valid. In most
    cases, you should not use `region-active-p`, since if the region
    is empty it is often more appropriate to operate on point.

---

Next: [Non-ASCII Characters](#Non_002dASCII-Characters), Previous: [Markers](#Markers), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 33 Text [¶](#Text-1)

This chapter describes the functions that deal with the text in a
buffer. Most examine, insert, or delete text in the current buffer,
often operating at point or on text adjacent to point. Many are
interactive. All the functions that change the text provide for undoing
the changes (see [Undo](#Undo)).

Many text-related functions operate on a region of text defined by two
buffer positions passed in arguments named start and end.
These arguments should be either markers (see [Markers](#Markers)) or numeric
character positions (see [Positions](#Positions)). The order of these arguments
does not matter; it is all right for start to be the end of the
region and end the beginning. For example, `(delete-region 1
10)` and `(delete-region 10 1)` are equivalent. An
`args-out-of-range` error is signaled if either start or
end is outside the accessible portion of the buffer. In an
interactive call, point and the mark are used for these arguments.

Throughout this chapter, “text” refers to the characters in the
buffer, together with their properties (when relevant). Keep in mind
that point is always between two characters, and the cursor appears on
the character after point.

* [Examining Text Near Point](#Near-Point)
* [Examining Buffer Contents](#Buffer-Contents)
* [Comparing Text](#Comparing-Text)
* [Inserting Text](#Insertion)
* [User-Level Insertion Commands](#Commands-for-Insertion)
* [Deleting Text](#Deletion)
* [User-Level Deletion Commands](#User_002dLevel-Deletion)
* [The Kill Ring](#The-Kill-Ring)
* [Undo](#Undo)
* [Maintaining Undo Lists](#Maintaining-Undo)
* [Filling](#Filling)
* [Margins for Filling](#Margins)
* [Adaptive Fill Mode](#Adaptive-Fill)
* [Auto Filling](#Auto-Filling)
* [Sorting Text](#Sorting)
* [Counting Columns](#Columns)
* [Indentation](#Indentation)
* [Case Changes](#Case-Changes)
* [Text Properties](#Text-Properties)
* [Substituting for a Character Code](#Substitution)
* [Registers](#Registers)
* [Transposition of Text](#Transposition)
* [Replacing Buffer Text](#Replacing)
* [Dealing With Compressed Data](#Decompression)
* [Base 64 Encoding](#Base-64)
* [Checksum/Hash](#Checksum_002fHash)
* [Suspicious Text](#Suspicious-Text)
* [GnuTLS Cryptography](#GnuTLS-Cryptography)
* [Database](#Database)
* [Parsing HTML and XML](#Parsing-HTML_002fXML)
* [Parsing and generating JSON values](#Parsing-JSON)
* [JSONRPC communication](#JSONRPC)
* [Atomic Change Groups](#Atomic-Changes)
* [Change Hooks](#Change-Hooks)

---

Next: [Examining Buffer Contents](#Buffer-Contents), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.1 Examining Text Near Point [¶](#Examining-Text-Near-Point)

Many functions are provided to look at the characters around point.
Several simple functions are described here. See also `looking-at`
in [Regular Expression Searching](#Regexp-Search).

In the following four functions, “beginning” or “end” of buffer
refers to the beginning or end of the accessible portion.

Function: **char-after** &optional position [¶](#index-char_002dafter)
:   This function returns the character in the current buffer at (i.e.,
    immediately after) position position. If position is out of
    range for this purpose, either before the beginning of the buffer, or at
    or beyond the end, then the value is `nil`. The default for
    position is point.

    In the following example, assume that the first character in the
    buffer is ‘`@`’:

    ```
    (string (char-after 1))
         ⇒ "@"
    ```

Function: **char-before** &optional position [¶](#index-char_002dbefore)
:   This function returns the character in the current buffer immediately
    before position position. If position is out of range for
    this purpose, either at or before the beginning of the buffer, or beyond
    the end, then the value is `nil`. The default for
    position is point.

Function: **following-char** [¶](#index-following_002dchar)
:   This function returns the character following point in the current
    buffer. This is similar to `(char-after (point))`. However, if
    point is at the end of the buffer, then `following-char` returns 0.

    Remember that point is always between characters, and the cursor
    normally appears over the character following point. Therefore, the
    character returned by `following-char` is the character the
    cursor is over.

    In this example, point is between the ‘`a`’ and the ‘`c`’.

    ```
    ---------- Buffer: foo ----------
    Gentlemen may cry ``Pea∗ce! Peace!,''
    but there is no peace.
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (string (preceding-char))
         ⇒ "a"
    (string (following-char))
         ⇒ "c"
    ```

Function: **preceding-char** [¶](#index-preceding_002dchar)
:   This function returns the character preceding point in the current
    buffer. See above, under `following-char`, for an example. If
    point is at the beginning of the buffer, `preceding-char` returns
    0.

Function: **bobp** [¶](#index-bobp)
:   This function returns `t` if point is at the beginning of the
    buffer. If narrowing is in effect, this means the beginning of the
    accessible portion of the text. See also `point-min` in
    [Point](#Point).

Function: **eobp** [¶](#index-eobp)
:   This function returns `t` if point is at the end of the buffer.
    If narrowing is in effect, this means the end of accessible portion of
    the text. See also `point-max` in See [Point](#Point).

Function: **bolp** [¶](#index-bolp)
:   This function returns `t` if point is at the beginning of a line.
    See [Motion by Text Lines](#Text-Lines). The beginning of the buffer (or of its accessible
    portion) always counts as the beginning of a line.

Function: **eolp** [¶](#index-eolp)
:   This function returns `t` if point is at the end of a line. The
    end of the buffer (or of its accessible portion) is always considered
    the end of a line.

---

Next: [Comparing Text](#Comparing-Text), Previous: [Examining Text Near Point](#Near-Point), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
