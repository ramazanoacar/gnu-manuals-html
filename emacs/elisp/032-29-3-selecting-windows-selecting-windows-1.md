### 29.3 Selecting Windows [¶](#Selecting-Windows-1)

In each frame, at any time, exactly one Emacs window is designated
as *selected within the frame*. For the selected frame, that
window is called the *selected window*—the one in which most
editing takes place, and in which the cursor for selected windows
appears (see [Cursor Parameters](#Cursor-Parameters)). Keyboard input that inserts or
deletes text is also normally directed to this window. The selected
window’s buffer is usually also the current buffer, except when
`set-buffer` has been used (see [The Current Buffer](#Current-Buffer)). As for
non-selected frames, the window selected within the frame becomes the
selected window if the frame is ever selected.

Function: **selected-window** [¶](#index-selected_002dwindow)
:   This function returns the selected window (which is always a live
    window).

The following function explicitly selects a window and its frame.

Function: **select-window** window &optional norecord [¶](#index-select_002dwindow)
:   This function makes window the selected window and the window
    selected within its frame, and selects that frame. It also makes
    window’s buffer (see [Buffers and Windows](#Buffers-and-Windows)) current and sets
    that buffer’s value of `point` to the value of `window-point`
    (see [Windows and Point](#Window-Point)) in window. window must be a live
    window. The return value is window.

    By default, this function also moves window’s buffer to the front
    of the buffer list (see [The Buffer List](#Buffer-List)) and makes window the most
    recently selected window. If the optional argument norecord is
    non-`nil`, these additional actions are omitted.

    In addition, this function by default also tells the display engine to
    update the display of window when its frame gets redisplayed the
    next time. If norecord is non-`nil`, such updates are
    usually not performed. If, however, norecord equals the special
    symbol `mark-for-redisplay`, the additional actions mentioned above
    are omitted but window’s display will be nevertheless updated.

    Note that sometimes selecting a window is not enough to show it, or
    make its frame the top-most frame on display: you may also need to
    raise the frame or make sure input focus is directed to that frame.
    See [Input Focus](#Input-Focus).

For historical reasons, Emacs does not run a separate hook whenever a
window gets selected. Applications and internal routines often
temporarily select a window to perform a few actions on it. They do
that either to simplify coding—because many functions by default
operate on the selected window when no window argument is
specified—or because some functions did not (and still do not) take a
window as argument and always operate(d) on the selected window instead.
Running a hook every time a window gets selected for a short time and
once more when the previously selected window gets restored is not
useful.

However, when its norecord argument is `nil`,
`select-window` updates the buffer list and thus indirectly runs
the normal hook `buffer-list-update-hook` (see [The Buffer List](#Buffer-List)).
Consequently, that hook provides one way to run a function whenever a
window gets selected more “permanently”.

Since `buffer-list-update-hook` is also run by functions that are
not related to window management, it will usually make sense to save the
value of the selected window somewhere and compare it with the value of
`selected-window` while running that hook. Also, to avoid false
positives when using `buffer-list-update-hook`, it is good practice
that every `select-window` call supposed to select a window only
temporarily passes a non-`nil` norecord argument. If
possible, the macro `with-selected-window` (see below) should be
used in such cases.

Emacs also runs the hook `window-selection-change-functions`
whenever the redisplay routine detects that another window has been
selected since last redisplay. See [Hooks for Window Scrolling and Changes](#Window-Hooks), for a detailed
explanation. `window-state-change-functions` (described in the
same section) is another abnormal hook run after a different window
has been selected but is triggered by other window changes as well.

The sequence of calls to `select-window` with a non-`nil`
norecord argument determines an ordering of windows by their
selection or use time, see below. The function `get-lru-window`,
for example, can then be used to retrieve the least recently selected
window (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)).

Function: **frame-selected-window** &optional frame [¶](#index-frame_002dselected_002dwindow)
:   This function returns the window on frame that is selected
    within that frame. frame should be a live frame; if omitted or
    `nil`, it defaults to the selected frame.

Function: **set-frame-selected-window** frame window &optional norecord [¶](#index-set_002dframe_002dselected_002dwindow)
:   This function makes window the window selected within the frame
    frame. frame should be a live frame; if `nil`, it
    defaults to the selected frame. window must denote a live window.

    If frame is the selected frame, this makes window the
    selected window.

    If the optional argument norecord is non-`nil`, this function
    does not alter the ordering of the most recently selected windows, nor
    the buffer list.

The following macros are useful to temporarily select a window without
affecting the ordering of recently selected windows or the buffer list.

Macro: **save-selected-window** forms… [¶](#index-save_002dselected_002dwindow)
:   This macro records the selected frame, as well as the selected window
    of each frame, executes forms in sequence, then restores the
    earlier selected frame and windows. It also saves and restores the
    current buffer. It returns the value of the last form in forms.

    This macro does not save or restore anything about the sizes,
    arrangement or contents of windows; therefore, if forms change
    them, the change persists. If the previously selected window of some
    frame is no longer live at the time of exit from forms, that
    frame’s selected window is left alone. If the previously selected
    window is no longer live, then whatever window is selected at the end of
    forms remains selected. The current buffer is restored if and
    only if it is still live when exiting forms.

    This macro changes neither the ordering of recently selected windows nor
    the buffer list.

Macro: **with-selected-window** window forms… [¶](#index-with_002dselected_002dwindow)
:   This macro selects window, executes forms in sequence, then
    restores the previously selected window and current buffer. The
    ordering of recently selected windows and the buffer list remain
    unchanged unless you deliberately change them within forms; for
    example, by calling `select-window` with argument norecord
    `nil`. Hence, this macro is the preferred way to temporarily work
    with window as the selected window without needlessly running
    `buffer-list-update-hook`.

    Note that this macro temporarily puts the window management code in an
    unstable state. In particular, the most recently used window (see below)
    will not necessarily match the selected one. Hence, functions like
    `get-lru-window` and `get-mru-window` may return unexpected
    results when called from the body of this macro.

Macro: **with-selected-frame** frame forms… [¶](#index-with_002dselected_002dframe)
:   This macro executes forms with frame as the selected
    frame. The value returned is the value of the last form in
    forms. This macro saves and restores the selected frame, and
    changes the order of neither the recently selected windows nor the
    buffers in the buffer list.

Function: **window-use-time** &optional window [¶](#index-window_002duse_002dtime)
:   This function returns the use time of window window. window
    must be a live window and defaults to the selected one.

    The *use time* of a window is not really a time value, but an
    integer that does increase monotonically with each call of
    `select-window` with a `nil` norecord argument. The
    window with the lowest use time is usually called the least recently
    used window. The window with the highest use time is called the most
    recently used one (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)) and is usually the
    selected window unless `with-selected-window` has been used.

Function: **window-bump-use-time** &optional window [¶](#index-window_002dbump_002duse_002dtime)
:   This function marks window as being the second most recently
    used one (after the selected window). It does nothing if window
    is the selected window or the selected window does not have the
    highest use time among all windows which may happen within the scope
    of `with-selected-window`.

Sometimes several windows collectively and
cooperatively display a buffer, for example, under the management of
Follow Mode (see [(emacs)Follow Mode](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Follow-Mode)), where the windows together
display a bigger portion of the buffer than one window could alone.
It is often useful to consider such a *window group* as a single
entity. Several functions such as `window-group-start`
(see [The Window Start and End Positions](#Window-Start-and-End)) allow you to do this by supplying, as
an argument, one of the windows as a stand-in for the whole group.

Function: **selected-window-group** [¶](#index-selected_002dwindow_002dgroup)
:   When the selected window is a member of a group of windows, this
    function returns a list of the windows in the group, ordered such that
    the first window in the list is displaying the earliest part of the
    buffer, and so on. Otherwise the function returns a list containing
    just the selected window.

    The selected window is considered part of a group when the buffer
    local variable `selected-window-group-function` is set to a
    function. In this case, `selected-window-group` calls it with no
    arguments and returns its result (which should be the list of windows
    in the group).

---

Next: [Resizing Windows](#Resizing-Windows), Previous: [Selecting Windows](#Selecting-Windows), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.4 Window Sizes [¶](#Window-Sizes-1)

Emacs provides miscellaneous functions for finding the height and width
of a window. The return value of many of these functions can be
specified either in units of pixels or in units of canonical lines and
columns. On a graphical display, the latter actually correspond to the
height and width of a default character specified by the frame’s default
font as returned by `frame-char-height` and `frame-char-width`
(see [Frame Font](#Frame-Font)). Thus, if a window is displaying text with a
different font or size, the reported line height and column width for
that window may differ from the actual number of text lines or columns
displayed within it.

The *total height* of a window is the number of lines comprising
its body and its top and bottom decorations (see [Basic Concepts of Emacs Windows](#Basic-Windows)).

Function: **window-total-height** &optional window round [¶](#index-window_002dtotal_002dheight)
:   This function returns the total height, in canonical lines, of the
    window window. If window is omitted or `nil`, it
    defaults to the selected window. If window is an internal window,
    the return value is the total height occupied by its descendant windows.

    If a window’s pixel height is not an integral multiple of its frame’s
    default character height, the number of lines occupied by the window is
    rounded internally. This is done in a way such that, if the window is a
    parent window, the sum of the total heights of all its child windows
    internally equals the total height of their parent. This means that
    although two windows have the same pixel height, their internal total
    heights may differ by one line. This means also, that if a window is
    vertically combined and has a next sibling, the topmost row of that
    sibling can be calculated as the sum of this window’s topmost row and
    total height (see [Coordinates and Windows](#Coordinates-and-Windows))

    If the optional argument round is `ceiling`, this function
    returns the smallest integer larger than window’s pixel height
    divided by the canonical character height of its frame; if it is
    `floor`, it returns the largest integer smaller than said value;
    with any other value of round it returns the internal value of
    windows’s total height.

The *total width* of a window is the number of columns comprising its
body and its left and right decorations (see [Basic Concepts of Emacs Windows](#Basic-Windows)).

Function: **window-total-width** &optional window round [¶](#index-window_002dtotal_002dwidth)
:   This function returns the total width, in canonical columns, of the
    window window. If window is omitted or `nil`, it
    defaults to the selected window. If window is internal, the
    return value is the total width occupied by its descendant windows.

    If a window’s pixel width is not an integral multiple of its frame’s
    canonical character width, the number of columns occupied by the window
    is rounded internally. This is done in a way such that, if the window
    is a parent window, the sum of the total widths of all its children
    internally equals the total width of their parent. This means that
    although two windows have the same pixel width, their internal total
    widths may differ by one column. This means also, that if this window
    is horizontally combined and has a next sibling, the leftmost column of
    that sibling can be calculated as the sum of this window’s leftmost
    column and total width (see [Coordinates and Windows](#Coordinates-and-Windows)). The optional
    argument round behaves as it does for `window-total-height`.

Function: **window-total-size** &optional window horizontal round [¶](#index-window_002dtotal_002dsize)
:   This function returns either the total height in lines or the total
    width in columns of the window window. If horizontal is
    omitted or `nil`, this is equivalent to calling
    `window-total-height` for window; otherwise it is equivalent
    to calling `window-total-width` for window. The optional
    argument round behaves as it does for `window-total-height`.

The following two functions can be used to return the total size of a
window in units of pixels.

Function: **window-pixel-height** &optional window [¶](#index-window_002dpixel_002dheight)
:   This function returns the total height of window window in pixels.
    window must be a valid window and defaults to the selected one.

    The return value includes the heights of window’s top and bottom
    decorations. If window is an internal window, its pixel height is
    the pixel height of the screen areas spanned by its children.

Function: **window-pixel-width** &optional window [¶](#index-window_002dpixel_002dwidth)
:   This function returns the width of window window in pixels.
    window must be a valid window and defaults to the selected one.

    The return value includes the widths of window’s left and right
    decorations. If window is an internal window, its pixel width is
    the width of the screen areas spanned by its children.

The following functions can be used to determine whether a given
window has any adjacent windows.

Function: **window-full-height-p** &optional window [¶](#index-window_002dfull_002dheight_002dp)
:   This function returns non-`nil` if window has no other window
    above or below it in its frame. More precisely, this means that the
    total height of window equals the total height of the root window
    on that frame. The minibuffer window does not count in this regard. If
    window is omitted or `nil`, it defaults to the selected
    window.

Function: **window-full-width-p** &optional window [¶](#index-window_002dfull_002dwidth_002dp)
:   This function returns non-`nil` if window has no other
    window to the left or right in its frame, i.e., its total width equals
    that of the root window on that frame. If window is omitted or
    `nil`, it defaults to the selected window.

The *body height* of a window is the height of its body, which
does not include any of its top or bottom decorations (see [Basic Concepts of Emacs Windows](#Basic-Windows)).

Function: **window-body-height** &optional window pixelwise [¶](#index-window_002dbody_002dheight)
:   This function returns the height, in canonical lines, of the body of
    window window. If window is omitted or `nil`, it
    defaults to the selected window; otherwise it must be a live window.

    The optional argument pixelwise defines the units to use for the
    height. If `nil`, return the body height of window in
    canonical characters, rounded down to the nearest integer, if necessary.
    This means that if a line at the bottom of the text area is only
    partially visible, that line is not counted. It also means that the
    height of a window’s body can never exceed its total height as returned
    by `window-total-height`.

    If pixelwise is `remap` and the default face is remapped
    (see [Face Remapping](#Face-Remapping)), use the remapped face to determine the
    character height. For any other non-`nil` value, return the
    height in pixels.

The *body width* of a window is the width of its body and of the
text area, which does not include any of its left or right decorations
(see [Basic Concepts of Emacs Windows](#Basic-Windows)).

Note that when one or both fringes are removed (by setting their width
to zero), the display engine reserves two character cells, one on each
side of the window, for displaying the continuation and truncation
glyphs, which leaves 2 columns less for text display. (The function
`window-max-chars-per-line`, described below, takes this
peculiarity into account.)

Function: **window-body-width** &optional window pixelwise [¶](#index-window_002dbody_002dwidth)
:   This function returns the width, in canonical columns, of the body of
    window window. If window is omitted or `nil`, it
    defaults to the selected window; otherwise it must be a live window.

    The optional argument pixelwise defines the units to use for the
    width. If `nil`, return the body width of window in
    canonical characters, rounded down to the nearest integer, if necessary.
    This means that if a column on the right of the text area is only
    partially visible, that column is not counted. It also means that the
    width of a window’s body can never exceed its total width as returned by
    `window-total-width`.

    If pixelwise is `remap` and the default face is remapped
    (see [Face Remapping](#Face-Remapping)), use the remapped face to determine the
    character width. For any other non-`nil` value, return the width
    in pixels.

Function: **window-body-size** &optional window horizontal pixelwise [¶](#index-window_002dbody_002dsize)
:   This function returns the body height or body width of window. If
    horizontal is omitted or `nil`, it is equivalent to calling
    `window-body-height` for window; otherwise it is equivalent
    to calling `window-body-width`. In either case, the optional
    argument pixelwise is passed to the function called.

The pixel heights of a window’s mode, tab and header line can be
retrieved with the functions given below. Their return value is usually
accurate unless the window has not been displayed before: In that case,
the return value is based on an estimate of the font used for the
window’s frame.

Function: **window-mode-line-height** &optional window [¶](#index-window_002dmode_002dline_002dheight)
:   This function returns the height in pixels of window’s mode line.
    window must be a live window and defaults to the selected one. If
    window has no mode line, the return value is zero.

Function: **window-tab-line-height** &optional window [¶](#index-window_002dtab_002dline_002dheight-1)
:   This function returns the height in pixels of window’s tab line.
    window must be a live window and defaults to the selected one. If
    window has no tab line, the return value is zero.

Function: **window-header-line-height** &optional window [¶](#index-window_002dheader_002dline_002dheight-1)
:   This function returns the height in pixels of window’s header
    line. window must be a live window and defaults to the selected
    one. If window has no header line, the return value is zero.

Functions for retrieving the height and/or width of window dividers
(see [Window Dividers](#Window-Dividers)), fringes (see [Fringes](#Fringes)), scroll bars
(see [Scroll Bars](#Scroll-Bars)), and display margins (see [Displaying in the Margins](#Display-Margins)) are
described in the corresponding sections.

If your Lisp program needs to make layout decisions, you will find the
following function useful:

Function: **window-max-chars-per-line** &optional window face [¶](#index-window_002dmax_002dchars_002dper_002dline)
:   This function returns the number of characters displayed in the
    specified face face in the specified window window (which
    must be a live window). If face was remapped (see [Face Remapping](#Face-Remapping)), the information is returned for the remapped face. If
    omitted or `nil`, face defaults to the default face, and
    window defaults to the selected window.

    Unlike `window-body-width`, this function accounts for the actual
    size of face’s font, instead of working in units of the canonical
    character width of window’s frame (see [Frame Font](#Frame-Font)). It also
    accounts for space used by the continuation glyph, if window lacks
    one or both of its fringes.

Commands that change the size of windows (see [Resizing Windows](#Resizing-Windows)),
or split them (see [Splitting Windows](#Splitting-Windows)), obey the variables
`window-min-height` and `window-min-width`, which specify the
smallest allowable window height and width. They also obey the variable
`window-size-fixed`, with which a window can be *fixed* in
size (see [Preserving Window Sizes](#Preserving-Window-Sizes)).

User Option: **window-min-height** [¶](#index-window_002dmin_002dheight-1)
:   This option specifies the minimum total height, in lines, of any window.
    Its value has to accommodate at least one text line and any top or
    bottom decorations.

User Option: **window-min-width** [¶](#index-window_002dmin_002dwidth-1)
:   This option specifies the minimum total width, in columns, of any
    window. Its value has to accommodate at least two text columns and any
    left or right decorations.

The following function tells how small a specific window can get taking
into account the sizes of its areas and the values of
`window-min-height`, `window-min-width` and
`window-size-fixed` (see [Preserving Window Sizes](#Preserving-Window-Sizes)).

Function: **window-min-size** &optional window horizontal ignore pixelwise [¶](#index-window_002dmin_002dsize)
:   This function returns the minimum size of window. window
    must be a valid window and defaults to the selected one. The optional
    argument horizontal non-`nil` means to return the minimum
    number of columns of window; otherwise return the minimum number
    of window’s lines.

    The return value makes sure that all components of window remain
    fully visible if window’s size were actually set to it. With
    horizontal `nil` it includes any top or bottom decorations.
    With horizontal non-`nil` it includes any left or right
    decorations of window.

    The optional argument ignore, if non-`nil`, means ignore
    restrictions imposed by fixed size windows, `window-min-height` or
    `window-min-width` settings. If ignore equals `safe`,
    live windows may get as small as `window-safe-min-height` lines and
    `window-safe-min-width` columns. If ignore is a window,
    ignore restrictions for that window only. Any other non-`nil`
    value means ignore all of the above restrictions for all windows.

    The optional argument pixelwise non-`nil` means to return the
    minimum size of window counted in pixels.

---

Next: [Preserving Window Sizes](#Preserving-Window-Sizes), Previous: [Window Sizes](#Window-Sizes), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.5 Resizing Windows [¶](#Resizing-Windows-1)

This section describes functions for resizing a window without changing
the size of its frame. Because live windows do not overlap, these
functions are meaningful only on frames that contain two or more
windows: resizing a window also changes the size of at least one other
window. If there is just one window on a frame, its size cannot be
changed except by resizing the frame (see [Frame Size](#Frame-Size)).

Except where noted, these functions also accept internal windows as
arguments. Resizing an internal window causes its child windows to be
resized to fit the same space.

Function: **window-resizable** window delta &optional horizontal ignore pixelwise [¶](#index-window_002dresizable)
:   This function returns delta if the size of window can be
    changed vertically by delta lines. If the optional argument
    horizontal is non-`nil`, it instead returns delta if
    window can be resized horizontally by delta columns. It
    does not actually change the window size.

    If window is `nil`, it defaults to the selected window.

    A positive value of delta means to check whether the window can be
    enlarged by that number of lines or columns; a negative value of
    delta means to check whether the window can be shrunk by that many
    lines or columns. If delta is non-zero, a return value of 0 means
    that the window cannot be resized.

    Normally, the variables `window-min-height` and
    `window-min-width` specify the smallest allowable window size
    (see [Window Sizes](#Window-Sizes)). However, if the optional argument ignore
    is non-`nil`, this function ignores `window-min-height` and
    `window-min-width`, as well as `window-size-fixed`. Instead,
    it considers the minimum height of a window as the sum of its top and
    bottom decorations plus the text of one line; and its minimum width
    as the sum of its left and right decorations plus text that takes two
    columns.

    If the optional argument pixelwise is non-`nil`,
    delta is interpreted as pixels.

Function: **window-resize** window delta &optional horizontal ignore pixelwise [¶](#index-window_002dresize)
:   This function resizes window by delta increments. If
    horizontal is `nil`, it changes the height by delta
    lines; otherwise, it changes the width by delta columns. A
    positive delta means to enlarge the window, and a negative
    delta means to shrink it.

    If window is `nil`, it defaults to the selected window. If
    the window cannot be resized as demanded, an error is signaled.

    The optional argument ignore has the same meaning as for the
    function `window-resizable` above.

    If the optional argument pixelwise is non-`nil`,
    delta will be interpreted as pixels.

    The choice of which window edges this function alters depends on the
    values of the option `window-combination-resize` and the
    combination limits of the involved windows; in some cases, it may alter
    both edges. See [Recombining Windows](#Recombining-Windows). To resize by moving only the
    bottom or right edge of a window, use the function
    `adjust-window-trailing-edge`.

Function: **adjust-window-trailing-edge** window delta &optional horizontal pixelwise [¶](#index-adjust_002dwindow_002dtrailing_002dedge)
:   This function moves window’s bottom edge by delta lines.
    If optional argument horizontal is non-`nil`, it instead
    moves the right edge by delta columns. If window is
    `nil`, it defaults to the selected window.

    If the optional argument pixelwise is non-`nil`,
    delta is interpreted as pixels.

    A positive delta moves the edge downwards or to the right; a
    negative delta moves it upwards or to the left. If the edge
    cannot be moved as far as specified by delta, this function
    moves it as far as possible but does not signal an error.

    This function tries to resize windows adjacent to the edge that is
    moved. If this is not possible for some reason (e.g., if that adjacent
    window is fixed-size), it may resize other windows.

User Option: **window-resize-pixelwise** [¶](#index-window_002dresize_002dpixelwise)
:   If the value of this option is non-`nil`, Emacs resizes windows in
    units of pixels. This currently affects functions like
    `split-window` (see [Splitting Windows](#Splitting-Windows)), `maximize-window`,
    `minimize-window`, `fit-window-to-buffer`,
    `fit-frame-to-buffer` and
    `shrink-window-if-larger-than-buffer` (all listed below).

    Note that when a frame’s pixel size is not a multiple of its character
    size, at least one window may get resized pixelwise even if this
    option is `nil`. The default value is `nil`.

The following commands resize windows in more specific ways. When
called interactively, they act on the selected window.

Command: **fit-window-to-buffer** &optional window max-height min-height max-width min-width preserve-size [¶](#index-fit_002dwindow_002dto_002dbuffer)
:   This command adjusts the height or width of window to fit the text
    in it. It returns non-`nil` if it was able to resize window,
    and `nil` otherwise. If window is omitted or `nil`, it
    defaults to the selected window. Otherwise, it should be a live window.

    If window is part of a vertical combination, this function adjusts
    window’s height. The new height is calculated from the actual
    height of the accessible portion of its buffer. The optional argument
    max-height, if non-`nil`, specifies the maximum total height
    that this function can give window. The optional argument
    min-height, if non-`nil`, specifies the minimum total height
    that it can give, which overrides the variable `window-min-height`.
    Both max-height and min-height are specified in lines and
    include any top or bottom decorations of window.

    If window is part of a horizontal combination and the value of the
    option `fit-window-to-buffer-horizontally` (see below) is
    non-`nil`, this function adjusts window’s width. The new
    width of window is calculated from the maximum length of its
    buffer’s lines that follow the current start position of window.
    The optional argument max-width specifies a maximum width and
    defaults to the width of window’s frame. The optional argument
    min-width specifies a minimum width and defaults to
    `window-min-width`. Both max-width and min-width are
    specified in columns and include any left or right decorations of
    window.

    The optional argument preserve-size, if non-`nil`, will
    install a parameter to preserve the size of window during future
    resize operations (see [Preserving Window Sizes](#Preserving-Window-Sizes)).

    If the option `fit-frame-to-buffer` (see below) is non-`nil`,
    this function will try to resize the frame of window to fit its
    contents by calling `fit-frame-to-buffer` (see below).

User Option: **fit-window-to-buffer-horizontally** [¶](#index-fit_002dwindow_002dto_002dbuffer_002dhorizontally)
:   If this is non-`nil`, `fit-window-to-buffer` can resize
    windows horizontally. If this is `nil` (the default)
    `fit-window-to-buffer` never resizes windows horizontally. If this
    is `only`, it can resize windows horizontally only. Any other
    value means `fit-window-to-buffer` can resize windows in both
    dimensions.

User Option: **fit-frame-to-buffer** [¶](#index-fit_002dframe_002dto_002dbuffer-1)
:   If this option is non-`nil`, `fit-window-to-buffer` can fit a
    frame to its buffer. A frame is fit if and only if its root window is a
    live window and this option is non-`nil`. If this is
    `horizontally`, frames are fit horizontally only. If this is
    `vertically`, frames are fit vertically only. Any other
    non-`nil` value means frames can be resized in both dimensions.

If you have a frame that displays only one window, you can fit that
frame to its buffer using the command `fit-frame-to-buffer`.

Command: **fit-frame-to-buffer** &optional frame max-height min-height max-width min-width only [¶](#index-fit_002dframe_002dto_002dbuffer)
:   This command adjusts the size of frame to display the contents of
    its buffer exactly. frame can be any live frame and defaults to
    the selected one. Fitting is done only if frame’s root window is
    a live window. On window systems that use size hints, exact fitting can
    be often achieved if and only if `frame-resize-pixelwise`
    (see [Frame Size](#Frame-Size)) is non-`nil`.

    The arguments max-height, min-height, max-width and
    min-width, if non-`nil`, specify bounds on the new body size
    of frame’s root window. A non-`nil` value specified by any
    of these arguments overrides the corresponding value specified by
    the option `fit-frame-to-buffer-sizes` described below.

    If the optional argument only is `vertically`, this function
    may resize the frame vertically only. If only is
    `horizontally`, it may resize the frame horizontally only.

The behavior of `fit-frame-to-buffer` can be controlled with the
help of the two options listed next.

User Option: **fit-frame-to-buffer-margins** [¶](#index-fit_002dframe_002dto_002dbuffer_002dmargins)
:   This option can be used to specify margins around frames to be fit by
    `fit-frame-to-buffer`. Such margins can be useful to avoid, for
    example, that the resized frame overlaps the taskbar or parts of its
    parent frame.

    It specifies the numbers of pixels to be left free on the left, above,
    the right, and below a frame that shall be fit. The default specifies
    `nil` for each which means to use no margins. The value specified
    here can be overridden for a specific frame by that frame’s
    `fit-frame-to-buffer-margins` parameter, if present.

User Option: **fit-frame-to-buffer-sizes** [¶](#index-fit_002dframe_002dto_002dbuffer_002dsizes)
:   This option specifies size boundaries for `fit-frame-to-buffer`.
    It specifies the maximum and minimum lines and maximum and minimum
    columns of the root window’s body of any frame that shall be fit to its
    buffer. Any value this option specifies will be overridden by the
    corresponding argument of `fit-frame-to-buffer`, if non-`nil`.

Command: **shrink-window-if-larger-than-buffer** &optional window [¶](#index-shrink_002dwindow_002dif_002dlarger_002dthan_002dbuffer)
:   This command attempts to reduce window’s height as much as
    possible while still showing its full buffer, but no less than
    `window-min-height` lines. The return value is non-`nil` if
    the window was resized, and `nil` otherwise. If window is
    omitted or `nil`, it defaults to the selected window. Otherwise,
    it should be a live window.

    This command does nothing if the window is already too short to
    display all of its buffer, or if any of the buffer is scrolled
    off-screen, or if the window is the only live window in its frame.

    This command calls `fit-window-to-buffer` (see above) to do its
    work.

Command: **balance-windows** &optional window-or-frame [¶](#index-balance_002dwindows)
:   This function balances windows in a way that gives more space to
    full-width and/or full-height windows. If window-or-frame
    specifies a frame, it balances all windows on that frame. If
    window-or-frame specifies a window, it balances only that window
    and its siblings (see [Windows and Frames](#Windows-and-Frames)).

Command: **balance-windows-area** [¶](#index-balance_002dwindows_002darea)
:   This function attempts to give all windows on the selected frame
    approximately the same share of the screen area. Full-width or
    full-height windows are not given more space than other windows.

Command: **maximize-window** &optional window [¶](#index-maximize_002dwindow)
:   This function attempts to make window as large as possible, in
    both dimensions, without resizing its frame or deleting other windows.
    If window is omitted or `nil`, it defaults to the selected
    window.

Command: **minimize-window** &optional window [¶](#index-minimize_002dwindow)
:   This function attempts to make window as small as possible, in
    both dimensions, without deleting it or resizing its frame. If
    window is omitted or `nil`, it defaults to the selected
    window.

---

Next: [Splitting Windows](#Splitting-Windows), Previous: [Resizing Windows](#Resizing-Windows), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.6 Preserving Window Sizes [¶](#Preserving-Window-Sizes-1)

A window can get resized explicitly by using one of the functions from
the preceding section or implicitly, for example, when resizing an
adjacent window, when splitting or deleting a window (see [Splitting Windows](#Splitting-Windows), see [Deleting Windows](#Deleting-Windows)) or when resizing the window’s frame
(see [Frame Size](#Frame-Size)).

It is possible to avoid implicit resizing of a specific window when
there are one or more other resizable windows on the same frame. For
this purpose, Emacs must be advised to *preserve* the size of that
window. There are two basic ways to do that.

Variable: **window-size-fixed** [¶](#index-window_002dsize_002dfixed)
:   If this buffer-local variable is non-`nil`, the size of any window
    displaying the buffer cannot normally be changed. Deleting a window or
    changing the frame’s size may still change the window’s size, if there
    is no choice.

    If the value is `height`, then only the window’s height is fixed;
    if the value is `width`, then only the window’s width is fixed.
    Any other non-`nil` value fixes both the width and the height.

    If this variable is `nil`, this does not necessarily mean that any
    window showing the buffer can be resized in the desired direction. To
    determine that, use the function `window-resizable`.
    See [Resizing Windows](#Resizing-Windows).

Often `window-size-fixed` is overly aggressive because it inhibits
any attempt to explicitly resize or split an affected window as well.
This may even happen after the window has been resized implicitly, for
example, when deleting an adjacent window or resizing the window’s
frame. The following function tries hard to never disallow resizing
such a window explicitly:

Function: **window-preserve-size** &optional window horizontal preserve [¶](#index-window_002dpreserve_002dsize)
:   This function (un-)marks the height of window window as preserved
    for future resize operations. window must be a live window and
    defaults to the selected one. If the optional argument horizontal
    is non-`nil`, it (un-)marks the width of window as preserved.

    If the optional argument preserve is `t`, this means to
    preserve the current height/width of window’s body. The
    height/width of window will change only if Emacs has no better
    choice. Resizing a window whose height/width is preserved by this
    function never throws an error.

    If preserve is `nil`, this means to stop preserving the
    height/width of window, lifting any respective restraint induced
    by a previous call of this function for window. Calling
    `enlarge-window`, `shrink-window` or
    `fit-window-to-buffer` with window as argument may also
    remove the respective restraint.

`window-preserve-size` is currently invoked by the following
functions:

`fit-window-to-buffer`
:   If the optional argument preserve-size of that function
    (see [Resizing Windows](#Resizing-Windows)) is non-`nil`, the size established by
    that function is preserved.

`display-buffer`
:   If the alist argument of that function (see [Choosing a Window for Displaying a Buffer](#Choosing-Window))
    contains a `preserve-size` entry, the size of the window produced
    by that function is preserved.

`window-preserve-size` installs a window parameter (see [Window Parameters](#Window-Parameters)) called `window-preserved-size` which is consulted by the
window resizing functions. This parameter will not prevent resizing the
window when the window shows another buffer than the one when
`window-preserve-size` was invoked or if its size has changed since
then.

The following function can be used to check whether the height of a
particular window is preserved:

Function: **window-preserved-size** &optional window horizontal [¶](#index-window_002dpreserved_002dsize)
:   This function returns the preserved height of window window in
    pixels. window must be a live window and defaults to the selected
    one. If the optional argument horizontal is non-`nil`, it
    returns the preserved width of window. It returns `nil` if
    the size of window is not preserved.

---

Next: [Deleting Windows](#Deleting-Windows), Previous: [Preserving Window Sizes](#Preserving-Window-Sizes), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.7 Splitting Windows [¶](#Splitting-Windows-1)

This section describes functions for creating a new window by
*splitting* an existing one. Note that some windows are special in
the sense that these functions may fail to split them as described here.
Examples of such windows are side windows (see [Side Windows](#Side-Windows)) and
atomic windows (see [Atomic Windows](#Atomic-Windows)).

Function: **split-window** &optional window size side pixelwise refer [¶](#index-split_002dwindow)
:   This function creates a new live window next to the window
    window. If window is omitted or `nil`, it defaults
    to the selected window. That window is split, and reduced in
    size. The space is taken up by the new window, which is returned.

    The optional second argument size determines the sizes of
    window and/or the new window. If it is omitted or `nil`,
    both windows are given equal sizes; if there is an odd line, it is
    allocated to the new window. If size is a positive number,
    window is given size lines (or columns, depending on the
    value of side). If size is a negative number, the new
    window is given −size lines (or columns).

    If size is `nil`, this function obeys the variables
    `window-min-height` and `window-min-width` (see [Window Sizes](#Window-Sizes)). Thus, it signals an error if splitting would result in making
    a window smaller than those variables specify. However, a
    non-`nil` value for size causes those variables to be
    ignored; in that case, the smallest allowable window is considered to be
    one that has space for a text that is one line tall and/or two columns
    wide.

    Hence, if size is specified, it’s the caller’s responsibility to
    check whether the emanating windows are large enough to encompass all of
    their decorations like a mode line or a scroll bar. The function
    `window-min-size` (see [Window Sizes](#Window-Sizes)) can be used to determine
    the minimum requirements of window in this regard. Since the new
    window usually inherits areas like the mode line or the scroll bar from
    window, that function is also a good guess for the minimum size of
    the new window. The caller should specify a smaller size only if it
    correspondingly removes an inherited area before the next redisplay.

    The optional third argument side determines the position of the
    new window relative to window. If it is `nil` or
    `below`, the new window is placed below window. If it is
    `above`, the new window is placed above window. In both
    these cases, size specifies a total window height, in lines.

    If side is `t` or `right`, the new window is placed on
    the right of window. If side is `left`, the new
    window is placed on the left of window. In both these cases,
    size specifies a total window width, in columns.

    As a rule, if window already forms a combination (see [Windows and Frames](#Windows-and-Frames)) that matches side (a horizontal combination matches
    side if it is `left` or `right`, a vertical combination
    matches side if it is `above` or `below`) and
    `window-combination-limit` (see [Recombining Windows](#Recombining-Windows)) is
    `nil`, this function reuses window’s parent in the window
    tree as parent of the new window.

    However, if window is in a combination that does not match
    side or if `window-combination-limit` is non-`nil`, this
    function makes a new parent window that replaces window in the
    window tree and makes window and the new window its sole child
    windows. This standard behavior can be overridden via the refer
    argument.

    The optional fourth argument pixelwise, if non-`nil`, means
    to interpret size in units of pixels, instead of lines and
    columns.

    If the optional fifth argument refer is non-`nil`, it
    specifies a reference window used for setting up properties of the new
    window. If non-`nil`, refer can be either a window or a cons
    cell of two windows.

    If refer is a cons cell, its CAR has to specify a deleted,
    former live window – a window that has shown a buffer before – on the
    same frame as window. That buffer must be still live. The
    CDR has to specify a deleted window that was, before its deletion,
    a parent window on the same frame as window. In this case, rather
    then making new windows, this function replaces window with the
    CDR of refer in the window tree and makes window and
    refer’s CAR its new child windows. Buffer, start and point
    positions of refer’s CAR are set to the values they had
    immediately before refer’s CAR was deleted the last time.
    Decorations and parameters remain unaltered from their values before
    refer’s CAR and CDR were deleted.

    Alternatively, refer may specify a deleted, former live window – a
    window that has shown a buffer before – on the same frame as
    window. That buffer must be still live. In this case, this
    function does not make a new window but rather makes refer live
    again and inserts it into the window tree at the position and with the
    sizes the new window would have been given. Buffer, start and point
    positions of refer are set to the values they had immediately
    before refer was deleted the last time. Decorations and
    parameters remain unaltered from their values before refer was
    deleted. The parent of refer is then determined as if it were a
    window created anew.

    In any other case, refer must specify a live window. Then the new
    window will inherit properties like buffer, start and point positions
    and some decorations from refer. If refer is `nil` or
    omitted, then if window is live, the new window inherits these
    properties from window. If, however, window is an internal
    window, the new window will inherit these properties from the window
    selected on window’s frame.

    The behavior of this function may be altered by the window parameters
    of window, so long as the variable
    `ignore-window-parameters` is `nil`. If the value of
    the `split-window` window parameter is `t`, this function
    ignores all other window parameters. Otherwise, if the value of the
    `split-window` window parameter is a function, that function is
    called with the arguments window, size, and side, in
    lieu of the usual action of `split-window`. Otherwise, this
    function obeys the `window-atom` or `window-side` window
    parameter, if any. See [Window Parameters](#Window-Parameters).

As an example, here is a sequence of `split-window` calls that
yields the window configuration discussed in [Windows and Frames](#Windows-and-Frames).
This example demonstrates splitting a live window as well as splitting
an internal window. We begin with a frame containing a single window
(a live root window), which we denote by W4. Calling
`(split-window W4)` yields this window configuration:

```
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W4_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W5_________________||
    |__________________W3__________________|
```

The `split-window` call has created a new live window, denoted by
W5. It has also created a new internal window, denoted by
W3, which becomes the root window and the parent of both
W4 and W5.

Next, we call `(split-window W3 nil 'left)`, passing the
internal window W3 as the argument. The result:

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

A new live window W2 is created, to the left of the internal
window W3. A new internal window W1 is created, becoming
the new root window.

For interactive use, Emacs provides two commands which always split
the selected window. These call `split-window` internally.

Command: **split-window-right** &optional size window-to-split [¶](#index-split_002dwindow_002dright)
:   This function splits the window window-to-split into two
    side-by-side windows, putting window-to-split on the left.
    window-to-split defaults to the selected window. If size
    is positive, the left window gets size columns; if size is
    negative, the right window gets −size columns.

Command: **split-window-below** &optional size window-to-split [¶](#index-split_002dwindow_002dbelow)
:   This function splits the window window-to-split into two
    windows, one above the other, leaving the upper window selected.
    window-to-split defaults to the selected window. If size
    is positive, the upper window gets size lines; if size is
    negative, the lower window gets −size lines.

Command: **split-root-window-below** &optional size [¶](#index-split_002droot_002dwindow_002dbelow)
:   This function splits the whole frame in two. The current window
    configuration is retained on the top, and a new window is created
    below, taking up the whole width of the frame. size is treated
    as by `split-window-below`.

Command: **split-root-window-right** &optional size [¶](#index-split_002droot_002dwindow_002dright)
:   This function splits the whole frame in two. The current window
    configuration is retained on the left, and a new window is created on
    the right, taking up the whole height of the frame. size is treated
    as by `split-window-right`.

User Option: **split-window-keep-point** [¶](#index-split_002dwindow_002dkeep_002dpoint)
:   If the value of this variable is non-`nil` (the default),
    `split-window-below` behaves as described above.

    If it is `nil`, `split-window-below` adjusts point in each
    of the two windows to minimize redisplay. (This is useful on slow
    terminals.) It selects whichever window contains the screen line that
    point was previously on. Note that this only affects
    `split-window-below`, not the lower-level `split-window`
    function.

---

Next: [Recombining Windows](#Recombining-Windows), Previous: [Splitting Windows](#Splitting-Windows), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.8 Deleting Windows [¶](#Deleting-Windows-1)

*Deleting* a window removes it from the frame’s window tree. If
the window is a live window, it disappears from the screen. If the
window is an internal window, its child windows are deleted too.

Even after a window is deleted, it continues to exist as a Lisp
object, until there are no more references to it. Window deletion can
be reversed, by restoring a saved window configuration (see [Window Configurations](#Window-Configurations)).

Command: **delete-window** &optional window [¶](#index-delete_002dwindow)
:   This function removes window from display and returns
    `nil`. If window is omitted or `nil`, it defaults to
    the selected window.

    If deleting the window would leave no more windows in the window tree
    (e.g., if it is the only live window in the frame) or all remaining
    windows on window’s frame are side windows (see [Side Windows](#Side-Windows)), an error is signaled. If window is part of an atomic
    window (see [Atomic Windows](#Atomic-Windows)), this function tries to delete the
    root of that atomic window instead.

    By default, the space taken up by window is given to one of its
    adjacent sibling windows, if any. However, if the variable
    `window-combination-resize` is non-`nil`, the space is
    proportionally distributed among any remaining windows in the same
    window combination. See [Recombining Windows](#Recombining-Windows).

    The behavior of this function may be altered by the window parameters of
    window, so long as the variable `ignore-window-parameters` is
    `nil`. If the value of the `delete-window` window parameter
    is `t`, this function ignores all other window parameters.
    Otherwise, if the value of the `delete-window` window parameter is
    a function, that function is called with the argument window, in
    lieu of the usual action of `delete-window`. See [Window Parameters](#Window-Parameters).

When `delete-window` deletes the selected window of its frame, it
has to make another window the new selected window of that frame. The
following option allows configuring which window is chosen.

User Option: **delete-window-choose-selected** [¶](#index-delete_002dwindow_002dchoose_002dselected)
:   This option allows specifying which window should become a frame’s
    selected window after `delete-window` has deleted the previously
    selected one. Possible choices are

    * `mru`
      (the default) means to choose the most recently used window
      (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)) other than the selected one on that
      frame.
    * `pos`
      means to choose the window comprising the position of
      `window-point` (see [Windows and Point](#Window-Point)) of the previously selected
      window on that frame.
    * `nil`
      means to choose the first window (the window returned by
      `frame-first-window`, see [Windows and Frames](#Windows-and-Frames)) on that frame.

    A window for which `window-no-other-p` (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)) returns non-`nil` is chosen only if all other windows on
    that frame have their `no-other-window` parameter set to a
    non-`nil` value too.

Command: **delete-other-windows** &optional window [¶](#index-delete_002dother_002dwindows)
:   This function makes window fill its frame, deleting other
    windows as necessary. If window is omitted or `nil`, it
    defaults to the selected window. An error is signaled if window
    is a side window (see [Side Windows](#Side-Windows)). If window is part of
    an atomic window (see [Atomic Windows](#Atomic-Windows)), this function tries to make
    the root of that atomic window fill its frame. The return
    value is `nil`.

    The behavior of this function may be altered by the window parameters of
    window, so long as the variable `ignore-window-parameters` is
    `nil`. If the value of the `delete-other-windows` window
    parameter is `t`, this function ignores all other window
    parameters. Otherwise, if the value of the `delete-other-windows`
    window parameter is a function, that function is called with the
    argument window, in lieu of the usual action of
    `delete-other-windows`. See [Window Parameters](#Window-Parameters).

    Also, if `ignore-window-parameters` is `nil`, this function
    does not delete any window whose `no-delete-other-windows`
    parameter is non-`nil`.

Command: **delete-windows-on** &optional buffer-or-name frame [¶](#index-delete_002dwindows_002don)
:   This function deletes all windows showing buffer-or-name, by
    calling `delete-window` on those windows. buffer-or-name
    should be a buffer, or the name of a buffer; if omitted or `nil`,
    it defaults to the current buffer. If there are no windows showing
    the specified buffer, this function does nothing. If the specified
    buffer is a minibuffer, an error is signaled.

    If there is a dedicated window showing the buffer, and that window is
    the only one on its frame, this function also deletes that frame if it
    is not the only frame on the terminal.

    The optional argument frame specifies which frames to operate
    on:

    * `nil`
      means operate on all frames.
    * `t`
      means operate on the selected frame.
    * `visible`
      means operate on all visible frames.
    * `0`
      means operate on all visible or iconified frames.
    * A frame
      means operate on that frame.

    Note that this argument does not have the same meaning as in other
    functions which scan all live windows (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)). Specifically, the meanings of `t` and `nil` here
    are the opposite of what they are in those other functions.

The above commands delete windows explicitly. However, Emacs may also
delete a window implicitly when it thinks that it’s more intuitive to
eliminate it rather than showing some unrelated buffer in it. Functions
that may delete windows implicitly are `kill-buffer`
(see [Killing Buffers](#Killing-Buffers)), `quit-restore-window` (see [Quitting Windows](#Quitting-Windows)) and `bury-buffer` (see [The Buffer List](#Buffer-List)). Some of these
delete a window if and only if that window is dedicated to its buffer
(see [Dedicated Windows](#Dedicated-Windows)). Others delete a window when that window
has been created by `display-buffer` (see [Displaying a Buffer in a Suitable Window](#Displaying-Buffers)).
Some will also try to delete a window’s frame together with the window,
provided there are other frames on the same terminal and the frame does
not host the active minibuffer window.

The hook described next can be used to avoid that a window gets
deleted by these functions.

User Option: **window-deletable-functions** [¶](#index-window_002ddeletable_002dfunctions)
:   This is an abnormal hook that can be used to avoid that a window gets
    deleted implicitly. The value should be a list of functions that take
    two arguments. The first argument is the window about to be deleted.
    The second argument, if non-`nil`, means that the window is the
    only window on its frame and would be deleted together with its frame.
    The window’s buffer is current when running this hook.

    If any of these functions returns `nil`, the window will not be
    deleted and another buffer will be shown in it. This hook is run
    (indirectly) by the functions `quit-restore-window`,
    `kill-buffer` and `bury-buffer`. It is not run by functions
    that delete windows explicitly like `delete-window`,
    `delete-other-windows` or `delete-windows-on`.

    The purpose of this hook is to give its clients a chance to save a
    window or its frame from deletion because they might still want to use
    that window or frame for their own purposes.

---

Next: [Resurrecting Windows](#Resurrecting-Windows), Previous: [Deleting Windows](#Deleting-Windows), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.9 Recombining Windows [¶](#Recombining-Windows-1)

When deleting the last sibling of a window W, its parent window
is deleted too, with W replacing it in the window tree. This
means that W must be recombined with its parent’s siblings to
form a new window combination (see [Windows and Frames](#Windows-and-Frames)). In some
occasions, deleting a live window may even entail the deletion of two
internal windows.

```
     ______________________________________
    | ______  ____________________________ |
    ||      || __________________________ ||
    ||      ||| ___________  ___________ |||
    ||      ||||           ||           ||||
    ||      ||||____W6_____||_____W7____||||
    ||      |||____________W4____________|||
    ||      || __________________________ ||
    ||      |||                          |||
    ||      |||                          |||
    ||      |||____________W5____________|||
    ||__W2__||_____________W3_____________ |
    |__________________W1__________________|
```

Deleting W5 in this configuration normally causes the deletion of
W3 and W4. The remaining live windows W2,
W6 and W7 are recombined to form a new horizontal
combination with parent W1.

Sometimes, however, it makes sense to not delete a parent window like
W4. In particular, a parent window should not be removed when it
was used to preserve a combination embedded in a combination of the same
type. Such embeddings make sense to assure that when you split a window
and subsequently delete the new window, Emacs reestablishes the layout
of the associated frame as it existed before the splitting.

Consider a scenario starting with two live windows W2 and
W3 and their parent W1.

```
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W2_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W3_________________||
    |__________________W1__________________|
```

Split W2 to make a new window W4 as follows.

```
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W2_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W4_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W3_________________||
    |__________________W1__________________|
```

Now, when enlarging a window vertically, Emacs tries to obtain the
corresponding space from its lower sibling, provided such a window
exists. In our scenario, enlarging W4 will steal space from
W3.

```
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W2_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W4_________________||
    | ____________________________________ |
    ||_________________W3_________________||
    |__________________W1__________________|
```

Deleting W4 will now give its entire space to W2,
including the space earlier stolen from W3.

```
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W2_________________||
    | ____________________________________ |
    ||_________________W3_________________||
    |__________________W1__________________|
```

This can be counterintuitive, in particular if W4 were used for
displaying a buffer only temporarily (see [Temporary Displays](#Temporary-Displays)), and
you want to continue working with the initial layout.

The behavior can be fixed by making a new parent window when splitting
W2. The variable described next allows that to be done.

User Option: **window-combination-limit** [¶](#index-window_002dcombination_002dlimit-1)
:   This variable controls whether splitting a window shall make a new
    parent window. The following values are recognized:

    `nil`
    :   This means that the new live window is allowed to share the existing
        parent window, if one exists, provided the split occurs in the same
        direction as the existing window combination (otherwise, a new internal
        window is created anyway).

    `window-size`
    :   This means that `display-buffer` makes a new parent window when
        it splits a window and is passed a `window-height` or
        `window-width` entry in the alist argument (see [Action Functions for Buffer Display](#Buffer-Display-Action-Functions)). Otherwise, window splitting behaves as
        for a value of `nil`.

    `temp-buffer-resize`
    :   In this case `with-temp-buffer-window` makes a new parent window
        when it splits a window and `temp-buffer-resize-mode` is enabled
        (see [Temporary Displays](#Temporary-Displays)). Otherwise, window splitting behaves as
        for `nil`.

    `temp-buffer`
    :   In this case `with-temp-buffer-window` always makes a new parent
        window when it splits an existing window (see [Temporary Displays](#Temporary-Displays)).
        Otherwise, window splitting behaves as for `nil`.

    `display-buffer`
    :   This means that when `display-buffer` (see [Choosing a Window for Displaying a Buffer](#Choosing-Window))
        splits a window it always makes a new parent window. Otherwise, window
        splitting behaves as for `nil`.

    `t`
    :   This means that splitting a window always creates a new parent window.
        Thus, if the value of this variable is at all times `t`, then at
        all times every window tree is a binary tree (a tree where each window
        except the root window has exactly one sibling).

    The default is `window-size`. Other values are reserved for future
    use.

    If, as a consequence of this variable’s setting, `split-window`
    makes a new parent window, it also calls
    `set-window-combination-limit` (see below) on the newly-created
    internal window. This affects how the window tree is rearranged when
    the child windows are deleted (see below).

If `window-combination-limit` is `t`, splitting W2 in
the initial configuration of our scenario would have produced this:

```
     ______________________________________
    | ____________________________________ |
    || __________________________________ ||
    |||                                  |||
    |||________________W2________________|||
    || __________________________________ ||
    |||                                  |||
    |||________________W4________________|||
    ||_________________W5_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W3_________________||
    |__________________W1__________________|
```

A new internal window W5 has been created; its children are
W2 and the new live window W4. Now, W2 is the only
sibling of W4, so enlarging W4 will try to shrink
W2, leaving W3 unaffected. Observe that W5
represents a vertical combination of two windows embedded in the
vertical combination W1.

Function: **set-window-combination-limit** window limit [¶](#index-set_002dwindow_002dcombination_002dlimit)
:   This function sets the *combination limit* of the window
    window to limit. This value can be retrieved via the
    function `window-combination-limit`. See below for its effects;
    note that it is only meaningful for internal windows. The
    `split-window` function automatically calls this function, passing
    it `t` as limit, provided the value of the variable
    `window-combination-limit` is `t` when it is called.

Function: **window-combination-limit** window [¶](#index-window_002dcombination_002dlimit)
:   This function returns the combination limit for window.

    The combination limit is meaningful only for an internal window. If it
    is `nil`, then Emacs is allowed to automatically delete
    window, in response to a window deletion, in order to group the
    child windows of window with its sibling windows to form a new
    window combination. If the combination limit is `t`, the child
    windows of window are never automatically recombined with its
    siblings.

    If, in the configuration shown at the beginning of this section, the
    combination limit of W4 (the parent window of W6 and
    W7) is `t`, deleting W5 will not implicitly delete
    W4 too.

Alternatively, the problems sketched above can be avoided by always
resizing all windows in the same combination whenever one of its windows
is split or deleted. This also permits splitting windows that would be
otherwise too small for such an operation.

User Option: **window-combination-resize** [¶](#index-window_002dcombination_002dresize)
:   If this variable is `nil`, `split-window` can only split a
    window (denoted by window) if window’s screen area is large
    enough to accommodate both itself and the new window.

    If this variable is `t`, `split-window` tries to resize all
    windows that are part of the same combination as window, in order
    to accommodate the new window. In particular, this may allow
    `split-window` to succeed even if window is a fixed-size
    window or too small to ordinarily split. Furthermore, subsequently
    resizing or deleting window may resize all other windows in its
    combination.

    The default is `nil`. Other values are reserved for future use. A
    specific split operation may ignore the value of this variable if it is
    affected by a non-`nil` value of `window-combination-limit`.

    If you plan to use a sequence of `split-window` calls to produce a
    specific, predefined layout of windows on a frame, bind this variable
    temporarily to `nil`. `window-state-put` (see [Window Configurations](#Window-Configurations)) and the functions which change window layouts
    (see [Changing Window Layouts](#Changing-Window-Layouts)) do that in order to restore a previous
    or systematically changed layout of a frame.

To illustrate the effect of `window-combination-resize`, consider
the following frame layout.

```
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W2_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W3_________________||
    |__________________W1__________________|
```

If `window-combination-resize` is `nil`, splitting window
W3 leaves the size of W2 unchanged:

```
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W2_________________||
    | ____________________________________ |
    ||                                    ||
    ||_________________W3_________________||
    | ____________________________________ |
    ||                                    ||
    ||_________________W4_________________||
    |__________________W1__________________|
```

If `window-combination-resize` is `t`, splitting W3
instead leaves all three live windows with approximately the same
height:

```
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W2_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W3_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W4_________________||
    |__________________W1__________________|
```

Deleting any of the live windows W2, W3 or W4 will
distribute its space proportionally among the two remaining live
windows.

The following two functions allow to expressly make a new parent window
for several adjacent windows and to undo that operation.

Function: **combine-windows** first last [¶](#index-combine_002dwindows)
:   This function combines the windows from first to last
    inclusive. first and last must be valid windows in the same
    combination, that is, windows with the same parent window. If neither
    first has a previous nor last has a next sibling, it returns
    that parent window. Otherwise, it makes a new parent window whose first
    child window becomes first and whose last child window becomes
    last, inserts that parent window in the window tree in lieu of the
    windows starting with first and ending with last and returns
    the new parent window.

    This function is useful to operate on several siblings of the same
    parent window as a whole. Consider three sibling windows w1,
    w2 and w3 aligned side by side. To create a window below
    w1 and w2 but not below w3, first combine w1 and
    w2 using this function, and then split the window it returned. You
    can also call `window-state-get` (see [Window Configurations](#Window-Configurations)) on
    the return value of `combine-windows` to obtain the combined state
    of w1 and w2.

Function: **uncombine-window** window [¶](#index-uncombine_002dwindow)
:   This function uncombines the specified window. window
    should be an internal window whose parent window is an internal window
    of the same type. This means that window and its parent should
    be either both horizontal or both vertical window combinations. If this
    is the case, the function makes the child windows of window become
    child windows of window’s parent, and returns `t`.
    Otherwise, it leaves the current configuration of window’s frame
    unchanged and returns `nil`.

    This function is useful to undo the effect of `combine-windows`.
    For example, to calculate the state of two adjacent windows w1 and
    w2, proceed as follows: first, call `combine-windows` with w1
    and w2 as arguments, and store the result in w. Then call
    `window-state-get` with the argument w. Finally, call
    `uncombine-window` with w as its argument to obtain the
    window configuration that existed before the call to `combine-windows`.

---

Next: [Changing Window Layouts](#Changing-Window-Layouts), Previous: [Recombining Windows](#Recombining-Windows), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
