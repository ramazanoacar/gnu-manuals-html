### 29.24 Vertical Fractional Scrolling [¶](#Vertical-Fractional-Scrolling)

*Vertical fractional scrolling* means shifting text in a window
up or down by a specified multiple or fraction of a line. Emacs uses
it, for example, on images and screen lines which are taller than the
window. Each window has a *vertical scroll position*, which is a
number, never less than zero. It specifies how far to raise the
contents of the window when displaying them. Raising the window
contents generally makes all or part of some lines disappear off the
top, and all or part of some other lines appear at the bottom. The
usual value is zero.

The vertical scroll position is measured in units of the normal line
height, which is the height of the default font. Thus, if the value is
.5, that means the window contents will be scrolled up half the normal
line height. If it is 3.3, that means the window contents are scrolled
up somewhat over three times the normal line height.

What fraction of a line the vertical scrolling covers, or how many
lines, depends on what the lines contain. A value of .5 could scroll a
line whose height is very short off the screen, while a value of 3.3
could scroll just part of the way through a tall line or an image.

Function: **window-vscroll** &optional window pixels-p [¶](#index-window_002dvscroll)
:   This function returns the current vertical scroll position of
    window. The default for window is the selected window.
    If pixels-p is non-`nil`, the return value is measured in
    pixels, rather than in units of the normal line height.

    ```
    (window-vscroll)
         ⇒ 0
    ```

Function: **set-window-vscroll** window lines &optional pixels-p preserve-vscroll-p [¶](#index-set_002dwindow_002dvscroll)
:   This function sets window’s vertical scroll position to
    lines. If window is `nil`, the selected window is
    used. The argument lines should be zero or positive; if not, it
    is taken as zero.

    The actual vertical scroll position must always correspond
    to an integral number of pixels, so the value you specify
    is rounded accordingly.

    The return value is the result of this rounding.

    ```
    (set-window-vscroll (selected-window) 1.2)
         ⇒ 1.13
    ```

    If pixels-p is non-`nil`, lines specifies a number of
    pixels. In this case, the return value is lines.

    Normally, the vscroll does not take effect on windows that aren’t the
    `minibuffer-scroll-window` or the selected window when the
    mini-window is resized (see [Minibuffer Windows](#Minibuffer-Windows)). This “frozen”
    behavior is disabled when the preserve-vscroll-p parameter is
    non-`nil`, which means to set the vscroll as usual.

Variable: **auto-window-vscroll** [¶](#index-auto_002dwindow_002dvscroll)
:   If this variable is non-`nil`, the `line-move`,
    `scroll-up`, and `scroll-down` functions will automatically
    modify the vertical scroll position to scroll through display rows
    that are taller than the height of the window, for example in the
    presence of large images.

---

Next: [Coordinates and Windows](#Coordinates-and-Windows), Previous: [Vertical Fractional Scrolling](#Vertical-Scrolling), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.25 Horizontal Scrolling [¶](#Horizontal-Scrolling-1)

*Horizontal scrolling* means shifting the image in the window left
or right by a specified multiple of the normal character width. Each
window has a *horizontal scroll position*, which is a number, never
less than zero. It specifies how far to shift the contents left.
Shifting the window contents left generally makes all or part of some
characters disappear off the left, and all or part of some other
characters appear at the right. The usual value is zero.

The horizontal scroll position is measured in units of the normal
character width, which is the width of space in the default font. Thus,
if the value is 5, that means the window contents are scrolled left by 5
times the normal character width. How many characters actually
disappear off to the left depends on their width, and could vary from
line to line.

Because we read from side to side in the inner loop, and from top
to bottom in the outer loop, the effect of horizontal scrolling is
not like that of textual or vertical scrolling. Textual scrolling
involves selection of a portion of text to display, and vertical
scrolling moves the window contents contiguously; but horizontal
scrolling causes part of *each line* to go off screen.

Usually, no horizontal scrolling is in effect; then the leftmost
column is at the left edge of the window. In this state, scrolling to
the right is meaningless, since there is no data to the left of the edge
to be revealed by it; so this is not allowed. Scrolling to the left is
allowed; it scrolls the first columns of text off the edge of the window
and can reveal additional columns on the right that were truncated
before. Once a window has a nonzero amount of leftward horizontal
scrolling, you can scroll it back to the right, but only so far as to
reduce the net horizontal scroll to zero. There is no limit to how far
left you can scroll, but eventually all the text will disappear off the
left edge.

If `auto-hscroll-mode` is set, redisplay automatically alters
the horizontal scrolling of a window as necessary to ensure that point
is always visible. However, you can still set the horizontal
scrolling value explicitly. The value you specify serves as a lower
bound for automatic scrolling, i.e., automatic scrolling will not
scroll a window to a column less than the specified one.

The default value of `auto-hscroll-mode` is `t`; setting
it to `current-line` activates a variant of automatic horizontal
scrolling whereby only the line showing the cursor is horizontally
scrolled to make point visible, the rest of the window is left either
unscrolled, or at the minimum scroll amount set by `scroll-left`
and `scroll-right`, see below.

Command: **scroll-left** &optional count set-minimum [¶](#index-scroll_002dleft)
:   This function scrolls the selected window count columns to the
    left (or to the right if count is negative). The default
    for count is the window width, minus 2.

    The return value is the total amount of leftward horizontal scrolling in
    effect after the change—just like the value returned by
    `window-hscroll` (below).

    Note that text in paragraphs whose base direction is right-to-left
    (see [Bidirectional Display](#Bidirectional-Display)) moves in the opposite direction: e.g.,
    it moves to the right when `scroll-left` is invoked with a
    positive value of count.

    Once you scroll a window as far right as it can go, back to its normal
    position where the total leftward scrolling is zero, attempts to scroll
    any farther right have no effect.

    If set-minimum is non-`nil`, the new scroll amount becomes
    the lower bound for automatic scrolling; that is, automatic scrolling
    will not scroll a window to a column less than the value returned by
    this function. Interactive calls pass non-`nil` for
    set-minimum.

Command: **scroll-right** &optional count set-minimum [¶](#index-scroll_002dright)
:   This function scrolls the selected window count columns to the
    right (or to the left if count is negative). The default
    for count is the window width, minus 2. Aside from the direction
    of scrolling, this works just like `scroll-left`.

Function: **window-hscroll** &optional window [¶](#index-window_002dhscroll)
:   This function returns the total leftward horizontal scrolling of
    window—the number of columns by which the text in window
    is scrolled left past the left margin. (In right-to-left paragraphs,
    the value is the total amount of the rightward scrolling instead.)
    The default for window is the selected window.

    The return value is never negative. It is zero when no horizontal
    scrolling has been done in window (which is usually the case).

    ```
    (window-hscroll)
         ⇒ 0
    ```

    ```
    (scroll-left 5)
         ⇒ 5
    ```

    ```
    (window-hscroll)
         ⇒ 5
    ```

Function: **set-window-hscroll** window columns [¶](#index-set_002dwindow_002dhscroll)
:   This function sets horizontal scrolling of window. The value of
    columns specifies the amount of scrolling, in terms of columns
    from the left margin (right margin in right-to-left paragraphs). The
    argument columns should be zero or positive; if not, it is taken
    as zero. Fractional values of columns are not supported at
    present.

    Note that `set-window-hscroll` may appear not to work if you test
    it by evaluating a call with `M-:` in a simple way. What happens
    is that the function sets the horizontal scroll value and returns, but
    then redisplay adjusts the horizontal scrolling to make point visible,
    and this overrides what the function did. You can observe the
    function’s effect if you call it while point is sufficiently far from
    the left margin that it will remain visible.

    The value returned is columns.

    ```
    (set-window-hscroll (selected-window) 10)
         ⇒ 10
    ```

Here is how you can determine whether a given position position
is off the screen due to horizontal scrolling:

```
(defun hscroll-on-screen (window position)
  (save-excursion
    (goto-char position)
    (and
     (>= (- (current-column) (window-hscroll window)) 0)
     (< (- (current-column) (window-hscroll window))
        (window-width window)))))
```

---

Next: [Mouse Window Auto-selection](#Mouse-Window-Auto_002dselection), Previous: [Horizontal Scrolling](#Horizontal-Scrolling), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.26 Coordinates and Windows [¶](#Coordinates-and-Windows-1)

This section describes functions that report positions of and within a
window. Most of these functions report positions relative to an origin
at the native position of the window’s frame (see [Frame Geometry](#Frame-Geometry)).
Some functions report positions relative to the origin of the display of
the window’s frame. In any case, the origin has the coordinates (0, 0)
and X and Y coordinates increase rightward and downward respectively.

For the following functions, X and Y coordinates are reported in
integer character units, i.e., numbers of lines and columns
respectively. On a graphical display, each “line” and “column”
corresponds to the height and width of the default character specified by
the frame’s default font (see [Frame Font](#Frame-Font)).

Function: **window-edges** &optional window body absolute pixelwise [¶](#index-window_002dedges)
:   This function returns a list of the edge coordinates of window.
    If window is omitted or `nil`, it defaults to the selected
    window.

    The return value has the form `(left top right
    bottom)`. These list elements are, respectively, the X
    coordinate of the leftmost column occupied by the window, the Y
    coordinate of the topmost row, the X coordinate one column to the
    right of the rightmost column, and the Y coordinate one row down from
    the bottommost row.

    Note that these are the actual outer edges of the window, including any
    of its decorations. On a text terminal, if the window has a neighbor on
    its right, its right edge includes the separator line between the window
    and its neighbor.

    If the optional argument body is `nil`, this means to
    return the edges corresponding to the total size of window.
    body non-`nil` means to return the edges of window’s
    body. If body is non-`nil`, window must specify a
    live window.

    If the optional argument absolute is `nil`, this means to
    return edges relative to the native position of window’s frame.
    absolute non-`nil` means to return coordinates relative to
    the origin (0, 0) of window’s display. On non-graphical systems
    this argument has no effect.

    If the optional argument pixelwise is `nil`, this means to
    return the coordinates in terms of the default character width and
    height of window’s frame (see [Frame Font](#Frame-Font)), rounded if
    necessary. pixelwise non-`nil` means to return the
    coordinates in pixels. Note that the pixel specified by right and
    bottom is immediately outside of these edges. If absolute
    is non-`nil`, pixelwise is implicitly non-`nil` too.

Function: **window-body-edges** &optional window [¶](#index-window_002dbody_002dedges)
:   This function returns the edges of window’s body (see [Window Sizes](#Window-Sizes)). Calling `(window-body-edges window)` is equivalent to
    calling `(window-edges window t)`, see above.

The following functions can be used to relate a set of
frame-relative coordinates to a window:

Function: **window-at** x y &optional frame [¶](#index-window_002dat)
:   This function returns the live window at the coordinates x and
    y given in default character sizes (see [Frame Font](#Frame-Font)) relative
    to the native position of frame (see [Frame Geometry](#Frame-Geometry)).

    If there is no window at that position, the return value is `nil`.
    If frame is omitted or `nil`, it defaults to the selected
    frame.

Function: **coordinates-in-window-p** coordinates window [¶](#index-coordinates_002din_002dwindow_002dp)
:   This function checks whether a window window occupies the frame
    relative coordinates coordinates, and if so, which part of the
    window that is. window should be a live window.

    coordinates should be a cons cell of the form `(x
    . y)`, where x and y are given in default character
    sizes (see [Frame Font](#Frame-Font)) relative to the native position of
    window’s frame (see [Frame Geometry](#Frame-Geometry)).

    If there is no window at the specified position, the return value is
    `nil` . Otherwise, the return value is one of the following:

    `(relx . rely)`
    :   The coordinates are inside window. The numbers relx and
        rely are the equivalent window-relative coordinates for the
        specified position, counting from 0 at the top left corner of the
        window.

    `mode-line`
    :   The coordinates are in the mode line of window.

    `header-line`
    :   The coordinates are in the header line of window.

    `tab-line`
    :   The coordinates are in the tab line of window.

    `right-divider`
    :   The coordinates are in the divider separating window from a
        window on the right.

    `bottom-divider`
    :   The coordinates are in the divider separating window from a
        window beneath.

    `vertical-line`
    :   The coordinates are in the vertical line between window and its
        neighbor to the right. This value occurs only if the window doesn’t
        have a scroll bar; positions in a scroll bar are considered outside the
        window for these purposes.

    `left-fringe`

    `right-fringe`
    :   The coordinates are in the left or right fringe of the window.

    `left-margin`

    `right-margin`
    :   The coordinates are in the left or right margin of the window.

    `nil`
    :   The coordinates are not in any part of window.

    The function `coordinates-in-window-p` does not require a frame as
    argument because it always uses the frame that window is on.

The following functions return window positions in pixels, rather
than character units. Though mostly useful on graphical displays,
they can also be called on text terminals, where the screen area of
each text character is taken to be one pixel.

Function: **window-pixel-edges** &optional window [¶](#index-window_002dpixel_002dedges)
:   This function returns a list of pixel coordinates for the edges of
    window. Calling `(window-pixel-edges window)` is equivalent
    to calling `(window-edges window nil nil t)`, see above.

Function: **window-body-pixel-edges** &optional window [¶](#index-window_002dbody_002dpixel_002dedges)
:   This function returns the pixel edges of window’s body. Calling
    `(window-body-pixel-edges window)` is equivalent to calling
    `(window-edges window t nil t)`, see above.

The following functions return window positions in pixels, relative to
the origin of the display screen rather than that of the frame:

Function: **window-absolute-pixel-edges** &optional window [¶](#index-window_002dabsolute_002dpixel_002dedges)
:   This function returns the pixel coordinates of window relative to
    an origin at (0, 0) of the display of window’s frame. Calling
    `(window-absolute-pixel-edges)` is equivalent to calling
    `(window-edges window nil t t)`, see above.

Function: **window-absolute-body-pixel-edges** &optional window [¶](#index-window_002dabsolute_002dbody_002dpixel_002dedges)
:   This function returns the pixel coordinates of window’s body
    relative to an origin at (0, 0) of the display of window’s frame.
    Calling `(window-absolute-body-pixel-edges window)` is equivalent
    to calling `(window-edges window t t t)`, see above.

    Combined with `set-mouse-absolute-pixel-position`, this function
    can be used to move the mouse pointer to an arbitrary buffer position
    visible in some window:

    ```
    (let ((edges (window-absolute-body-pixel-edges))
          (position (pos-visible-in-window-p nil nil t)))
      (set-mouse-absolute-pixel-position
       (+ (nth 0 edges) (nth 0 position))
       (+ (nth 1 edges) (nth 1 position))))
    ```

    On a graphical terminal this form “warps” the mouse cursor to the
    upper left corner of the glyph at the selected window’s point. A
    position calculated this way can be also used to show a tooltip window
    there.

The following function returns the screen coordinates of a buffer
position visible in a window:

Function: **window-absolute-pixel-position** &optional position window [¶](#index-window_002dabsolute_002dpixel_002dposition)
:   If the buffer position position is visible in window window,
    this function returns the display coordinates of the upper/left corner
    of the glyph at position. The return value is a cons of the X-
    and Y-coordinates of that corner, relative to an origin at (0, 0) of
    window’s display. It returns `nil` if position is not
    visible in window.

    window must be a live window and defaults to the selected
    window. position defaults to the value of `window-point`
    of window.

    This means that in order to move the mouse pointer to the position of
    point in the selected window, it’s sufficient to write:

    ```
    (let ((position (window-absolute-pixel-position)))
      (set-mouse-absolute-pixel-position
       (car position) (cdr position)))
    ```

The following function returns the largest rectangle that can be
inscribed in a window without covering text displayed in that window.

Function: **window-largest-empty-rectangle** &optional window count min-width min-height positions left [¶](#index-window_002dlargest_002dempty_002drectangle)
:   This function calculates the dimensions of the largest empty rectangle
    that can be inscribed in the specified window’s text area.
    window must be a live window and defaults to the selected one.

    The return value is a triple of the width and the start and end
    y-coordinates of the largest rectangle that can be inscribed into the
    empty space (space not displaying any text) of the text area of
    window. No x-coordinates are returned by this function—any such
    rectangle is assumed to end at the right edge of window’s text
    area. If no empty space can be found, the return value is `nil`.

    The optional argument count, if non-`nil`, specifies a
    maximum number of rectangles to return. This means that the return
    value is a list of triples specifying rectangles with the largest
    rectangle first. count can be also a cons cell whose car
    specifies the number of rectangles to return and whose CDR, if
    non-`nil`, states that all rectangles returned must be disjoint.

    The optional arguments min-width and min-height, if
    non-`nil`, specify the minimum width and height of any rectangle
    returned.

    The optional argument positions, if non-`nil`, is a cons cell
    whose CAR specifies the uppermost and whose CDR specifies the
    lowermost pixel position that must be covered by any rectangle returned.
    These positions measure from the start of the text area of window.

    The optional argument left, if non-`nil`, means to return
    values suitable for buffers displaying right to left text. In that
    case, any rectangle returned is assumed to start at the left edge of
    window’s text area.

    Note that this function has to retrieve the dimensions of each line of
    window’s glyph matrix via `window-lines-pixel-dimensions`
    (see [Size of Displayed Text](#Size-of-Displayed-Text)). Hence, this function may also return
    `nil` when the current glyph matrix of window is not
    up-to-date.

---

Next: [Window Configurations](#Window-Configurations), Previous: [Coordinates and Windows](#Coordinates-and-Windows), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.27 Mouse Window Auto-selection [¶](#Mouse-Window-Auto_002dselection-1)

The following option enables automatically selecting the window under
the mouse pointer. This accomplishes a policy similar to that of
window managers that give focus to a frame (and thus trigger its
subsequent selection) whenever the mouse pointer enters its
window-system window (see [Input Focus](#Input-Focus), see [Focus Events](#Focus-Events)).

User Option: **mouse-autoselect-window** [¶](#index-mouse_002dautoselect_002dwindow)
:   If this variable is non-`nil`, Emacs will try to automatically
    select the window under the mouse pointer. The following values are
    meaningful:

    A positive number
    :   This specifies a delay in seconds after which auto-selection triggers.
        The window under the mouse pointer is selected after the mouse has
        remained in it for the entire duration of the delay.

    A negative number
    :   A negative number has a similar effect as a positive number, but selects
        the window under the mouse pointer only after the mouse pointer has
        remained in it for the entire duration of the absolute value of that
        number and in addition has stopped moving.

    Other value
    :   Any other non-`nil` value means to select a window instantaneously
        as soon as the mouse pointer enters it.

    In either case, the mouse pointer must enter the text area of a window in
    order to trigger its selection. Dragging the scroll bar slider or the
    mode line of a window conceptually should not cause its auto-selection.

    Mouse auto-selection selects the minibuffer window only if it is active,
    and never deselects the active minibuffer window.

Mouse auto-selection can be used to emulate a focus follows mouse policy
for child frames (see [Child Frames](#Child-Frames)) which usually are not tracked by
the window manager. This requires setting the value of
`focus-follows-mouse` (see [Input Focus](#Input-Focus)) to a non-`nil`
value. If the value of `focus-follows-mouse` is `auto-raise`,
entering a child frame with the mouse will raise it automatically above
all other child frames of that frame’s parent frame.

---

Next: [Window Parameters](#Window-Parameters), Previous: [Mouse Window Auto-selection](#Mouse-Window-Auto_002dselection), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.28 Window Configurations [¶](#Window-Configurations-1)

A *window configuration* records the entire layout of one
frame—all windows, their sizes, their decorations, which buffers they
contain, how those buffers are scrolled, and their value of point, It
also includes the value of `minibuffer-scroll-window`. As a
special exception, the window configuration does not record the value of
point in the selected window for the current buffer.

You can bring back an entire frame layout by restoring a previously
saved window configuration. If you want to record the layout of all
frames instead of just one, use a frame configuration instead of a
window configuration. See [Frame Configurations](#Frame-Configurations).

Function: **current-window-configuration** &optional frame [¶](#index-current_002dwindow_002dconfiguration)
:   This function returns a new object representing frame’s current
    window configuration. The default for frame is the selected
    frame. The variable `window-persistent-parameters` specifies
    which window parameters (if any) are saved by this function.
    See [Window Parameters](#Window-Parameters).

Function: **set-window-configuration** configuration &optional dont-set-frame dont-set-miniwindow [¶](#index-set_002dwindow_002dconfiguration)
:   This function restores the configuration of windows and buffers as
    specified by configuration, for the frame that
    configuration was created for, regardless of whether that frame
    is selected or not. The argument configuration must be a value
    that was previously returned by `current-window-configuration`
    for that frame. Normally the function also selects the frame which is
    recorded in the configuration, but if dont-set-frame is
    non-`nil`, it leaves selected the frame which was already
    selected at the start of the function.

    Normally the function restores the saved minibuffer (if any), but if
    dont-set-miniwindow is non-`nil`, the minibuffer current
    at the start of the function (if any) remains in the mini-window.

    If the frame from which configuration was saved is dead, all
    this function does is to restore the value of the variable
    `minibuffer-scroll-window` and to adjust the value returned by
    `minibuffer-selected-window`. In this case, the function returns
    `nil`. Otherwise, it returns `t`.

    This function consults the variable
    `window-restore-killed-buffer-windows` (see below) when it tries to
    restore a window whose buffer was killed after configuration was
    recorded.

    Here is a way of using this function to get the same effect as
    `save-window-excursion`:

    ```
    (let ((config (current-window-configuration)))
      (unwind-protect
          (progn (split-window-below nil)
                 ...)
        (set-window-configuration config)))
    ```

Macro: **save-window-excursion** forms… [¶](#index-save_002dwindow_002dexcursion)
:   This macro records the window configuration of the selected frame,
    executes forms in sequence, then restores the earlier window
    configuration. The return value is the value of the final form in
    forms.

    Most Lisp code should not use this macro; `save-selected-window`
    is typically sufficient. In particular, this macro cannot reliably
    prevent the code in forms from opening new windows, because new
    windows might be opened in other frames (see [Choosing a Window for Displaying a Buffer](#Choosing-Window)), and
    `save-window-excursion` only saves and restores the window
    configuration on the current frame.

Function: **window-configuration-p** object [¶](#index-window_002dconfiguration_002dp)
:   This function returns `t` if object is a window configuration.

Function: **window-configuration-equal-p** config1 config2 [¶](#index-window_002dconfiguration_002dequal_002dp)
:   This function says whether two window configurations have the same
    window layout, but ignores the values of point and the saved scrolling
    positions—it can return `t` even if those aspects differ.

Function: **window-configuration-frame** config [¶](#index-window_002dconfiguration_002dframe)
:   This function returns the frame for which the window configuration
    config was made.

Other primitives to look inside of window configurations would make
sense, but are not implemented because we did not need them. See the
file `winner.el` for some more operations on windows
configurations.

The objects returned by `current-window-configuration` die
together with the Emacs process. In order to store a window
configuration on disk and read it back in another Emacs session, you
can use the functions described next. These functions are also useful
to clone the state of a frame into an arbitrary live window
(`set-window-configuration` effectively clones the windows of a
frame into the root window of that very frame only).

Function: **window-state-get** &optional window writable [¶](#index-window_002dstate_002dget)
:   This function returns the state of window as a Lisp object. The
    argument window must be a valid window and defaults to the root
    window of the selected frame.

    If the optional argument writable is non-`nil`, this means to
    not use markers for sampling positions like `window-point` or
    `window-start`. This argument should be non-`nil` when the
    state will be written to disk and read back in another session.

    Together, writable and the variable
    `window-persistent-parameters` specify which window parameters are
    saved by this function. The default is to save the `clone-of`
    parameter if writable is `nil`. See [Window Parameters](#Window-Parameters).

    Bind `window-state-normalize-buffer-name` to non-`nil` to
    normalize buffer names under `uniquify` management by removing its
    prefixes and suffixes. This helps restore window buffers across Emacs
    sessions. See [Uniquify](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Uniquify) in The GNU Emacs Manual.

The value returned by `window-state-get` can be used in the same
session to clone a window (see below). It can be also written to disk
and read back in another session. In either case, use the following
function to restore the state of the window.

Function: **window-state-put** state &optional window ignore [¶](#index-window_002dstate_002dput)
:   This function puts the window state state into window. The
    argument state should be the state of a window returned by an
    earlier invocation of `window-state-get`, see above. The optional
    argument window can be either a live window or an internal window
    (see [Windows and Frames](#Windows-and-Frames)). If window is not a live window, it
    is replaced by a new live window created on the same frame before
    putting state into it. If window is `nil`, it puts the
    window state into a new window.

    This function assigns all window parameters that have been stored in
    state. By default, this is the `clone-of` parameter provided
    state was obtained from an invocation of `window-state-get`
    with the argument writable `nil`. See [Window Parameters](#Window-Parameters).

    This function consults the variable
    `window-restore-killed-buffer-windows` (see below) when it tries to
    restore a window whose buffer was killed after state was recorded.

    If the optional argument ignore is non-`nil`, it means to ignore
    minimum window sizes and fixed-size restrictions. If ignore
    is `safe`, this means windows can get as small as one line
    and/or two columns.

In the context of window states, the *clone of a window* is a window
that has the same decorations and contents as the window whose state was
used to produce it, but is actually represented by another window
object. Hence it does not have the same identity as the original
window. Operating on the original or the clone of a window does not
affect the other in any way.

Note that while `set-window-configuration` always restores the
identity of the windows it puts back into their frame,
`window-state-put` never does that. Note also that while
`window-state-get` clones existing windows, these clones are not
yet valid windows. They become valid only after `window-state-put`
has put them into a live frame.

By default, `set-window-configuration` and `window-state-put`
may delete a window from the restored configuration or state when they
find out that its buffer was killed since the corresponding
configuration or state has been recorded. The variable described next
can be used to fine-tune that behavior.

Variable: **window-restore-killed-buffer-windows** [¶](#index-window_002drestore_002dkilled_002dbuffer_002dwindows)
:   This variable specifies how `set-window-configuration` and
    `window-state-put` shall handle a window whose buffer has been
    killed since the corresponding configuration or state was recorded. Any
    such window may be live—in which case it shows some other buffer—or
    dead at the time one of these functions is called. Usually,
    `set-window-configuration` leaves the window alone if it is live
    while `window-state-put` deletes it.

    The following values can be used to override the default behavior for
    dead windows in the case of `set-window-configuration` and for dead
    and live windows in the case of `window-state-put`.

    `t`
    :   This value means to unconditionally restore the window and show some
        other buffer in it.

    `delete`
    :   This means to unconditionally try to delete the window.

    `dedicated`
    :   This means to try to delete the window if and only if it is dedicated to
        its buffer.

    `nil`
    :   This is the default, and it means that `set-window-configuration`
        will try to delete the window if and only if it is dedicated to its
        buffer, and `window-state-put` will unconditionally try to delete
        it.

    a function
    :   This means to restore the window and show some other buffer in it, like
        if the value is `t`, and also add an entry for that window to a
        list that will be later passed as the second argument to that function.

    If a window cannot be deleted (typically, because it is the last window
    on its frame), `set-window-configuration` and
    `window-state-put` will show another buffer in it.

    If the value of this variable is a function, that function should take
    three arguments. The first argument specifies the frame whose windows
    have been restored. The third argument is either the symbol
    `configuration` if the windows are restored by
    `set-window-configuration`, or the symbol `state` if the
    windows are restored by `window-state-put`.

    The second argument specifies a list of entries for *all* windows
    whose previous buffers have been found dead at the time
    `set-window-configuration` or `window-state-put` tried to
    restore them (minibuffer windows are excluded). This means that the
    function may also delete windows which were found live by
    `set-window-configuration`.

    Each entry in the list that is passed as the second argument to the
    function is itself a list of six values: the window whose buffer was
    found dead, the dead buffer or its name, the positions of window start
    (see [The Window Start and End Positions](#Window-Start-and-End)) and window point (see [Windows and Point](#Window-Point))
    of the buffer in that window, the dedicated state of the window as
    previously reported by `window-dedicated-p` and a flag that is
    `t` if the window has been found to be alive by
    `set-window-configuration` and `nil` otherwise.

The functions `window-state-get` and `window-state-put` also
allow exchanging the contents of two live windows. The following
function does precisely that:

Command: **window-swap-states** &optional window-1 window-2 size [¶](#index-window_002dswap_002dstates)
:   This command swaps the states of the two live windows window-1 and
    window-2. window-1 must specify a live window and defaults
    to the selected one. window-2 must specify a live window and
    defaults to the window following window-1 in the cyclic ordering
    of windows, excluding minibuffer windows and including live windows on
    all visible frames.

    Optional argument size non-`nil` means to try swapping the
    sizes of window-1 and window-2 as well. A value of
    `height` means to swap heights only, a value of `width`
    means to swap widths only, while `t` means to swap both widths
    and heights, if possible. Frames are not resized by this function.

---

Next: [Hooks for Window Scrolling and Changes](#Window-Hooks), Previous: [Window Configurations](#Window-Configurations), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.29 Window Parameters [¶](#Window-Parameters-1)

This section describes the window parameters that can be used to
associate additional information with windows.

Function: **window-parameter** window parameter [¶](#index-window_002dparameter)
:   This function returns window’s value for parameter. The
    default for window is the selected window. If window has no
    setting for parameter, this function returns `nil`.

Function: **window-parameters** &optional window [¶](#index-window_002dparameters)
:   This function returns all parameters of window and their values.
    The default for window is the selected window. The return value
    is either `nil`, or an association list whose elements have the form
    `(parameter . value)`.

Function: **set-window-parameter** window parameter value [¶](#index-set_002dwindow_002dparameter)
:   This function sets window’s value of parameter to
    value and returns value. The default for window
    is the selected window.

By default, the functions that save and restore window configurations
or the states of windows (see [Window Configurations](#Window-Configurations)) do not care
about window parameters. This means that when you change the value of
a parameter within the body of a `save-window-excursion`, the
previous value is not restored when that macro exits. It also means
that when you restore via `window-state-put` a window state saved
earlier by `window-state-get`, all cloned windows have their
parameters reset to `nil`. The following variable allows you to
override the standard behavior:

Variable: **window-persistent-parameters** [¶](#index-window_002dpersistent_002dparameters)
:   This variable is an alist specifying which parameters get saved by
    `current-window-configuration` and `window-state-get`, and
    subsequently restored by `set-window-configuration` and
    `window-state-put`. See [Window Configurations](#Window-Configurations).

    The CAR of each entry of this alist is a symbol specifying the
    parameter. The CDR should be one of the following:

    `nil`
    :   This value means the parameter is saved neither by
        `window-state-get` nor by `current-window-configuration`.

    `t`
    :   This value specifies that the parameter is saved by
        `current-window-configuration` and (provided its writable
        argument is `nil`) by `window-state-get`.

    `writable`
    :   This means that the parameter is saved unconditionally by both
        `current-window-configuration` and `window-state-get`. This
        value should not be used for parameters whose values do not have a read
        syntax. Otherwise, invoking `window-state-put` in another session
        may fail with an `invalid-read-syntax` error.

Some functions (notably `delete-window`,
`delete-other-windows` and `split-window`), may behave
specially when the window specified by their window argument has
a parameter whose name is equal to the function’s name. You can
override such special behavior by binding the following variable to a
non-`nil` value:

Variable: **ignore-window-parameters** [¶](#index-ignore_002dwindow_002dparameters)
:   If this variable is non-`nil`, some standard functions do not
    process window parameters. The functions currently affected by this are
    `split-window`, `delete-window`, `delete-other-windows`,
    and `other-window`.

    An application can bind this variable to a non-`nil` value around
    calls to these functions. If it does so, the application is fully
    responsible for correctly assigning the parameters of all involved
    windows when exiting that function.

The following parameters are currently used by the window management
code:

`delete-window` [¶](#index-delete_002dwindow_002c-a-window-parameter)
:   This parameter affects the execution of `delete-window`
    (see [Deleting Windows](#Deleting-Windows)).

`delete-other-windows` [¶](#index-delete_002dother_002dwindows_002c-a-window-parameter)
:   This parameter affects the execution of `delete-other-windows`
    (see [Deleting Windows](#Deleting-Windows)).

`no-delete-other-windows` [¶](#index-no_002ddelete_002dother_002dwindows_002c-a-window-parameter)
:   This parameter marks the window as not deletable by
    `delete-other-windows` (see [Deleting Windows](#Deleting-Windows)).

`split-window` [¶](#index-split_002dwindow_002c-a-window-parameter)
:   This parameter affects the execution of `split-window`
    (see [Splitting Windows](#Splitting-Windows)).

`other-window` [¶](#index-other_002dwindow_002c-a-window-parameter)
:   This parameter affects the execution of `other-window`
    (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)).

`no-other-window` [¶](#index-no_002dother_002dwindow_002c-a-window-parameter)
:   This parameter marks the window as not selectable by `other-window`
    (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)). It is by default `t` for tooltip
    windows (see [Basic Concepts of Emacs Windows](#Basic-Windows)). Use `window-no-other-p`
    (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)) to check whether it applies to a
    specific window.

`clone-of` [¶](#index-clone_002dof_002c-a-window-parameter)
:   This parameter specifies the window that this one has been cloned from.
    It is persistent by default and as such gets saved by
    `window-state-get` and installed by `window-state-put`
    (see [Window Configurations](#Window-Configurations)).

`window-preserved-size` [¶](#index-window_002dpreserved_002dsize_002c-a-window-parameter)
:   This parameter specifies a buffer, a direction where `nil` means
    vertical and `t` horizontal, and a size in pixels. If this window
    displays the specified buffer and its size in the indicated direction
    equals the size specified by this parameter, then Emacs will try to
    preserve the size of this window in the indicated direction. This
    parameter is installed and updated by the function
    `window-preserve-size` (see [Preserving Window Sizes](#Preserving-Window-Sizes)).

`quit-restore`

`quit-restore-prev` [¶](#index-quit_002drestore_002c-a-window-parameter)
:   These parameters are installed by the buffer display functions
    (see [Choosing a Window for Displaying a Buffer](#Choosing-Window)) and consulted by `quit-restore-window`
    (see [Quitting Windows](#Quitting-Windows)). They are lists of four elements, see the
    description of `quit-restore-window` in [Quitting Windows](#Quitting-Windows) for
    details.

`window-side` [¶](#index-window_002dside_002c-a-window-parameter)

`window-slot`
:   These parameters are used internally for implementing side windows
    (see [Side Windows](#Side-Windows)).

`window-atom` [¶](#index-window_002datom_002c-a-window-parameter)
:   This parameter is used internally for implementing atomic windows, see
    [Atomic Windows](#Atomic-Windows).

`mode-line-format` [¶](#index-mode_002dline_002dformat_002c-a-window-parameter)
:   This parameter replaces the value of the buffer-local variable
    `mode-line-format` (see [Mode Line Basics](#Mode-Line-Basics)) of this window’s
    buffer whenever this window is displayed. The symbol `none` means
    to suppress display of a mode line for this window. Display and
    contents of the mode line on other windows showing this buffer are not
    affected.

`header-line-format` [¶](#index-header_002dline_002dformat_002c-a-window-parameter)
:   This parameter replaces the value of the buffer-local variable
    `header-line-format` (see [Mode Line Basics](#Mode-Line-Basics)) of this window’s
    buffer whenever this window is displayed. The symbol `none` means
    to suppress display of a header line for this window. Display and
    contents of the header line on other windows showing this buffer are not
    affected.

`tab-line-format` [¶](#index-tab_002dline_002dformat_002c-a-window-parameter)
:   This parameter replaces the value of the buffer-local variable
    `tab-line-format` (see [Mode Line Basics](#Mode-Line-Basics)) of this window’s
    buffer whenever this window is displayed. The symbol `none` means
    to suppress display of a tab line for this window. Display and
    contents of the tab line on other windows showing this buffer are not
    affected.

`min-margins` [¶](#index-min_002dmargins_002c-a-window-parameter)
:   The value of this parameter is a cons cell whose CAR and
    CDR, if non-`nil`, specify the minimum values (in columns)
    for the left and right margin of this window (see [Displaying in the Margins](#Display-Margins)).
    When present, Emacs will use these values instead of the actual margin
    widths for determining whether a window can be split or shrunk
    horizontally.

    Emacs never auto-adjusts the margins of any window after splitting or
    resizing it. It is the sole responsibility of any application setting
    this parameter to adjust the margins of this window as well as those of
    any new window that inherits this window’s margins due to a split.
    Both `window-configuration-change-hook` and
    `window-size-change-functions` (see [Hooks for Window Scrolling and Changes](#Window-Hooks)) should be
    employed for this purpose.

    This parameter was introduced in Emacs version 25.1 to support
    applications that use large margins to center buffer text within a
    window and should be used, with due care, exclusively by those
    applications. It might be replaced by an improved solution in future
    versions of Emacs.

---

Previous: [Window Parameters](#Window-Parameters), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.30 Hooks for Window Scrolling and Changes [¶](#Hooks-for-Window-Scrolling-and-Changes)

This section describes how Lisp programs can take action after a
window has been scrolled or other window modifications occurred. We
first consider the case where a window shows a different part of its
buffer.

Variable: **window-scroll-functions** [¶](#index-window_002dscroll_002dfunctions)
:   This variable holds a list of functions that Emacs should call before
    redisplaying a window with scrolling. Displaying a different buffer
    in a window and making a new window also call these functions.

    This variable is not a normal hook, because each function is called
    with two arguments: the window, and its new display-start position.
    At the time of the call, the display-start position of the argument
    window is already set to its new value, and the buffer to be displayed
    in the window is set as the current buffer.

    These functions must take care when using `window-end`
    (see [The Window Start and End Positions](#Window-Start-and-End)); if you need an up-to-date value, you
    must use the update argument to ensure you get it.

    **Warning:** don’t use this feature to alter the way the window
    is scrolled. It’s not designed for that, and such use probably won’t
    work.

In addition, you can use `jit-lock-register` to register a Font
Lock fontification function, which will be called whenever parts of a
buffer are (re)fontified because a window was scrolled or its size
changed. See [Other Font Lock Variables](#Other-Font-Lock-Variables).

The remainder of this section covers six hooks that are called
during redisplay provided a significant, non-scrolling change of a
window has been detected. For simplicity, these hooks and the
functions they call will be collectively referred to as *window
change functions*. As any hook, these hooks can be set either
globally or buffer-locally via the local argument of
`add-hook` (see [Setting Hooks](#Setting-Hooks)) when the hook is installed.

The first of these hooks is run after a *window buffer change* is
detected, which means that a window was created, deleted or assigned
another buffer.

Variable: **window-buffer-change-functions** [¶](#index-window_002dbuffer_002dchange_002dfunctions)
:   This variable specifies functions called during redisplay when window
    buffers have changed. The value should be a list of functions that
    take one argument.

    Functions specified buffer-locally are called for each window showing
    the corresponding buffer if and only if that window has been added or
    changed its buffer since the last redisplay. Note that functions may be
    run twice for such a window: Once for the buffer shown in the window at
    the time of the last redisplay (provided that buffer is still live) and
    once for the buffer currently shown in the window. In either case the
    window is passed as argument and the respective buffer is made
    temporarily current.

    If you are using the buffer-local version of this hook, you may want to
    discern the case where the buffer was removed from a window from the
    case where the buffer is now shown in that window. If window
    denotes the window argument passed to a function on this hook, then to
    detect the case where the buffer was removed from window use the
    idiom

    ```
    (eq (window-old-buffer window) (current-buffer))
    ```

    while to detect whether the buffer is now shown in window use

    ```
    (eq (window-buffer window) (current-buffer))
    ```

    instead.

    Functions specified by the default value are called for a frame if at
    least one window on that frame has been added, deleted or assigned
    another buffer since the last time window change functions were run.
    In this case the frame is passed as argument.

The second of these hooks is run when a *window size change* has
been detected which means that a window was created, assigned another
buffer, or changed its total size or that of its text area.

Variable: **window-size-change-functions** [¶](#index-window_002dsize_002dchange_002dfunctions)
:   This variable specifies functions called during redisplay when a
    window size change occurred. The value should be a list of functions
    that take one argument.

    Functions specified buffer-locally are called for any window showing the
    corresponding buffer if that window has been added or assigned another
    buffer or changed its total or body size since the last time window
    change functions were run. In this case the window is passed as
    argument and the respective buffer is temporarily made current.

    Functions specified by the default value are called for a frame if at
    least one window on that frame has been added or assigned another
    buffer or changed its total or body size since the last time window
    change functions were run. In this case the frame is passed as
    argument.

The third of these hooks is run when a *window selection change*
has selected another window since the last redisplay.

Variable: **window-selection-change-functions** [¶](#index-window_002dselection_002dchange_002dfunctions)
:   This variable specifies functions called during redisplay when the
    selected window or a frame’s selected window has changed. The value
    should be a list of functions that take one argument.

    Functions specified buffer-locally are called for any window showing the
    corresponding buffer if that window has been selected or deselected
    (among all windows or among all windows on its frame) since the last
    time window change functions were run. In this case the window is
    passed as argument and the respective buffer is temporarily made
    current.

    Functions specified by the default value are called for a frame if
    that frame has been selected or deselected or the frame’s selected
    window has changed since the last time window change functions were
    run. In this case the frame is passed as argument.

The fourth of these hooks is run when a *window state change* has
been detected, which means that at least one of the three preceding
window changes has occurred.

Variable: **window-state-change-functions** [¶](#index-window_002dstate_002dchange_002dfunctions)
:   This variable specifies functions called during redisplay when a
    window buffer or size change occurred or the selected window or a
    frame’s selected window has changed. The value should be a list of
    functions that take one argument.

    Functions specified buffer-locally are called for any window showing the
    corresponding buffer if that window has been added or assigned another
    buffer, changed its total or body size or has been selected or
    deselected (among all windows or among all windows on its frame) since
    the last time window change functions were run. In this case the window
    is passed as argument and the respective buffer is temporarily made
    current.

    Functions specified by the default value are called for a frame if at
    least one window on that frame has been added, deleted or assigned
    another buffer, changed its total or body size or that frame has been
    selected or deselected or the frame’s selected window has changed
    since the last time window change functions were run. In this case
    the frame is passed as argument.

    Functions specified by the default value are also run for a frame when
    that frame’s window state change flag (see below) has been set since
    last redisplay.

The fifth of these hooks is run when a *window configuration
change* has been detected which means that either the buffer or the
size of a window changed. It differs from the four preceding hooks in
the way it is run.

Variable: **window-configuration-change-hook** [¶](#index-window_002dconfiguration_002dchange_002dhook)
:   This variable specifies functions called during redisplay when either
    the buffer or the size of a window has changed. The value should be a
    list of functions that take no argument.

    Functions specified buffer-locally are called for any window showing
    the corresponding buffer if at least one window on that frame has been
    added, deleted or assigned another buffer or changed its total or
    body size since the last time window change functions were run. Each
    call is performed with the window showing the buffer temporarily
    selected and its buffer current.

    Functions specified by the default value are called for each frame if
    at least one window on that frame has been added, deleted or assigned
    another buffer or changed its total or body size since the last time
    window change functions were run. Each call is performed with the
    frame temporarily selected and the selected window’s buffer current.

Finally, Emacs runs a normal hook that generalizes the behavior of
`window-state-change-functions`.

Variable: **window-state-change-hook** [¶](#index-window_002dstate_002dchange_002dhook)
:   The default value of this variable specifies functions called during
    redisplay when a window state change has been detected or the window
    state change flag has been set on at least one frame. The value
    should be a list of functions that take no argument.

    Applications should put a function on this hook only if they want to
    react to changes that happened on (or have been signaled for) two or
    more frames since last redisplay. In every other case, putting the
    function on `window-state-change-functions` should be preferred.

Window change functions are called during redisplay for each frame as
follows: First, any buffer-local window buffer change function, window
size change function, selected window change and window state change
functions are called in this order. Next, the default values for
these functions are called in the same order. Then any buffer-local
window configuration change functions are called followed by functions
specified by the default value of those functions. Finally, functions
on `window-state-change-hook` are run.

Window change functions are run for a specific frame only if a
corresponding change was registered for that frame earlier. Such
changes include the creation or deletion of a window or the assignment
of another buffer or size to a window. Note that even when such a
change has been registered, this does not mean that any of the hooks
described above is run. If, for example, a change was registered
within the scope of a window excursion (see [Window Configurations](#Window-Configurations)), this will trigger a call of window change functions
only if that excursion still persists at the time change functions are
run. If it is exited earlier, hooks will be run only if registered by
a change outside the scope of that excursion.

The *window state change flag* of a frame, if set, will cause
the default values of `window-state-change-functions` (for that
frame) and `window-state-change-hook` to be run during next
redisplay regardless of whether a window state change actually
occurred for that frame or not. After running any functions on these
hooks, the flag is reset for each frame. Applications can set that
flag and inspect its value using the following functions.

Function: **set-frame-window-state-change** &optional frame arg [¶](#index-set_002dframe_002dwindow_002dstate_002dchange)
:   This function sets frame’s window state change flag if arg
    is non-`nil` and resets it otherwise. frame must be a live
    frame and defaults to the selected one.

Function: **frame-window-state-change** &optional frame [¶](#index-frame_002dwindow_002dstate_002dchange)
:   This functions returns `t` if frame’s window state change
    flag is set and `nil` otherwise. frame must be a live
    frame and defaults to the selected one.

While window change functions are run, the functions described next
can be called to get more insight into what has changed for a specific
window or frame since the last redisplay. All these functions take a
live window as single, optional argument, defaulting to the selected
window.

Function: **window-old-buffer** &optional window [¶](#index-window_002dold_002dbuffer)
:   This function returns the buffer shown in window at the last time
    window change functions were run for window’s frame. If it
    returns `nil`, window is either an internal window or has
    been created after that. If it returns `t`, window was not
    shown at that time but has been restored from a previously saved window
    configuration afterwards. Otherwise, the return value is the buffer
    shown by window at that time. As a special case, if window
    has been deleted, this function returns the last buffer window had
    shown at that time. window can be any window and defaults to the
    selected one.

Function: **window-old-pixel-width** &optional window [¶](#index-window_002dold_002dpixel_002dwidth)
:   This function returns the total pixel width of window the
    last time window change functions found window live on its
    frame. It is zero if window was created after that.

Function: **window-old-pixel-height** &optional window [¶](#index-window_002dold_002dpixel_002dheight)
:   This function returns the total pixel height of window the last
    time window change functions found window live on its frame.
    It is zero if window was created after that.

Function: **window-old-body-pixel-width** &optional window [¶](#index-window_002dold_002dbody_002dpixel_002dwidth)
:   This function returns the pixel width of window’s text area the
    last time window change functions found window live on its
    frame. It is zero if window was created after that.

Function: **window-old-body-pixel-height** &optional window [¶](#index-window_002dold_002dbody_002dpixel_002dheight)
:   This function returns the pixel height of window’s text area the
    last time window change functions found window live on its
    frame. It is zero if window was created after that.

In order to find out which window or frame was selected the last time
window change functions were run, the following functions can be used:

Function: **frame-old-selected-window** &optional frame [¶](#index-frame_002dold_002dselected_002dwindow)
:   This function returns the selected window of frame at the last
    time window change functions were run. If omitted or `nil`
    frame defaults to the selected frame.

Function: **old-selected-window** [¶](#index-old_002dselected_002dwindow)
:   This function returns the selected window at the last time window
    change functions were run.

Function: **old-selected-frame** [¶](#index-old_002dselected_002dframe)
:   This function returns the selected frame at the last time window
    change functions were run.

Note that window change functions provide no information about which
windows have been deleted since the last time they were run. If
necessary, applications should remember any window showing a specific
buffer in a local variable of that buffer and update it in a function
run by the default values of any of the hooks that are run when a
window buffer change was detected.

The following caveats should be considered when adding a function
to window change functions:

* Some operations will not trigger a call of window change functions.
  These include showing another buffer in a minibuffer window or any
  change of a tooltip window.
* Window change functions should not create or delete windows or change
  the buffer, size or selection status of any window because there is no
  guarantee that the information about such a change will be propagated
  to other window change functions. If at all, any such change should
  be executed only by the last function listed by the default value of
  `window-state-change-hook`.
* Macros like `save-window-excursion`, `with-selected-window`
  or `with-current-buffer` can be used when running window change
  functions.
* Running window change functions does not save and restore match data.
  Unless running `window-configuration-change-hook` it does not
  save or restore the selected window or frame or the current buffer
  either.
* Any redisplay triggering the run of window change functions may be
  aborted. If the abort occurs before window change functions have run
  to their completion, they will be run again with the previous values,
  that is, as if redisplay had not been performed. If aborted later,
  they will be run with the new values, that is, as if redisplay had
  been actually performed.

---

Next: [Positions](#Positions), Previous: [Windows](#Windows), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 30 Frames [¶](#Frames-1)

A *frame* is a screen object that contains one or more Emacs
windows (see [Windows](#Windows)). It is the kind of object called a
“window” in the terminology of graphical environments; but we can’t
call it a “window” here, because Emacs uses that word in a different
way. In Emacs Lisp, a *frame object* is a Lisp object that
represents a frame on the screen. See [Frame Type](#Frame-Type).

A frame initially contains a single main window and/or a minibuffer
window; you can subdivide the main window vertically or horizontally
into smaller windows. See [Splitting Windows](#Splitting-Windows).

A *terminal* is a display device capable of displaying one or
more Emacs frames. In Emacs Lisp, a *terminal object* is a Lisp
object that represents a terminal. See [Terminal Type](#Terminal-Type).

There are two classes of terminals: *text terminals* and
*graphical terminals*. Text terminals are non-graphics-capable
displays, including `xterm` and other terminal emulators. On a
text terminal, each Emacs frame occupies the terminal’s entire screen;
although you can create additional frames and switch between them, the
terminal only shows one non-child frame at a time. This frame is
referred to as the *top frame* of that terminal and can be retrieved
with the function `tty-top-frame` described below. A top frame may
have child frames (see [Child Frames](#Child-Frames)), which will be shown together
with it, but it cannot be a child frame itself.

Graphical terminals, on the other hand, are managed by graphical display
systems such as the X Window System, which allow Emacs to show multiple
frames simultaneously on the same display.

On GNU and Unix systems, you can create additional frames on any
available terminal, within a single Emacs session, regardless of whether
Emacs was started on a text or graphical terminal. Emacs can display on
both, graphical and text terminals, simultaneously. This comes in
handy, for instance, when you connect to the same session from several
remote locations. See [Multiple Terminals](#Multiple-Terminals).

Function: **framep** object [¶](#index-framep)
:   This predicate returns a non-`nil` value if object is a
    frame, and `nil` otherwise. For a frame, the value indicates which
    kind of display the frame uses:

    `t`
    :   The frame is displayed on a text terminal.

    `x`
    :   The frame is displayed on an X graphical terminal.

    `w32`
    :   The frame is displayed on a MS-Windows graphical terminal.

    `ns`
    :   The frame is displayed on a GNUstep or Macintosh Cocoa graphical
        terminal.

    `pc`
    :   The frame is displayed on an MS-DOS terminal.

    `haiku`
    :   The frame is displayed using the Haiku Application Kit.

    `pgtk`
    :   The frame is displayed using pure GTK facilities.

    `android`
    :   The frame is displayed on an Android device.

Function: **frame-terminal** &optional frame [¶](#index-frame_002dterminal)
:   This function returns the terminal object that displays frame. If
    frame is `nil` or unspecified, it defaults to the selected
    frame (see [Input Focus](#Input-Focus)).

Function: **terminal-live-p** object [¶](#index-terminal_002dlive_002dp)
:   This predicate returns a non-`nil` value if object is a
    terminal that is live (i.e., not deleted), and `nil` otherwise.
    For live terminals, the return value indicates what kind of frames are
    displayed on that terminal; the list of possible values is the same as
    for `framep` above.

Function: **frame-initial-p** &optional frame [¶](#index-frame_002dinitial_002dp)
:   This predicate returns non-`nil` if frame is or holds the
    initial text frame that is used internally during daemon mode
    (see [daemon](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Initial-Options) in The GNU Emacs Manual), batch
    mode (see [Batch Mode](#Batch-Mode)), and the early stages of startup
    (see [Summary: Sequence of Actions at Startup](#Startup-Summary)). Interactive and graphical programs, for
    instance, can use this predicate to avoid operating on the initial
    frame, which is never displayed.

    If frame is a terminal, this function returns non-`nil` if
    frame holds the initial frame. If frame is omitted or
    `nil`, it defaults to the selected one.

On a graphical terminal we distinguish two types of frames: A normal
*top-level frame* is a frame whose window-system window is a child
of the window-system’s root window for that terminal. A child frame is
a frame whose window-system window is the child of the window-system
window of another Emacs frame. See [Child Frames](#Child-Frames).

On a text terminal you can get its top frame with the following
function:

Function: **tty-top-frame** &optional terminal [¶](#index-tty_002dtop_002dframe)
:   This function returns the top frame on terminal. terminal
    should be a terminal object, a frame (meaning that frame’s terminal), or
    `nil` (meaning the selected frame’s terminal). If it does not
    refer to a text terminal, the return value is `nil`. A top frame
    must be a root frame, which means it cannot be a child frame itself
    (see [Child Frames](#Child-Frames)), but may have an arbitrary number of child frames
    descending from it.

Function: **frame-id** &optional frame [¶](#index-frame_002did)
:   This function returns the unique identifier of a frame, an integer,
    assigned to frame. If frame is `nil` or unspecified,
    it defaults to the selected frame (see [Input Focus](#Input-Focus)). This can be
    used to unambiguously identify a frame in a context where you do not or
    cannot use a frame object.

    A frame undeleted using `undelete-frame` will retain its
    identifier. A frame cloned using `clone-frame` will not retain
    its original identifier. See [Frame Commands](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Frame-Commands) in the Emacs
    Manual.

    Frame identifiers are not persisted using the desktop library
    (see [Desktop Save Mode](#Desktop-Save-Mode)), `frameset-to-register`, or
    `frameset-save`, and each of their restored frames will bear a new
    unique id.

* [Creating Frames](#Creating-Frames)
* [Multiple Terminals](#Multiple-Terminals)
* [Frame Geometry](#Frame-Geometry)
* [Frame Parameters](#Frame-Parameters)
* [Terminal Parameters](#Terminal-Parameters)
* [Frame Titles](#Frame-Titles)
* [Deleting Frames](#Deleting-Frames)
* [Finding All Frames](#Finding-All-Frames)
* [Splitting and Merging Frames](#Splitting-and-Merging-Frames)
* [Minibuffers and Frames](#Minibuffers-and-Frames)
* [Input Focus](#Input-Focus)
* [Visibility of Frames](#Visibility-of-Frames)
* [Raising, Lowering and Restacking Frames](#Raising-and-Lowering)
* [Frame Configurations](#Frame-Configurations)
* [Child Frames](#Child-Frames)
* [Mouse Tracking](#Mouse-Tracking)
* [Mouse Position](#Mouse-Position)
* [Pop-Up Menus](#Pop_002dUp-Menus)
* [On-Screen Keyboards](#On_002dScreen-Keyboards)
* [Dialog Boxes](#Dialog-Boxes)
* [Pointer Shape](#Pointer-Shape)
* [Window System Selections](#Window-System-Selections)
* [Accessing Selections](#Accessing-Selections)
* [Yanking Media](#Yanking-Media)
* [Drag and Drop](#Drag-and-Drop)
* [Color Names](#Color-Names)
* [Text Terminal Colors](#Text-Terminal-Colors)
* [X Resources](#Resources)
* [Display Feature Testing](#Display-Feature-Testing)

---

Next: [Multiple Terminals](#Multiple-Terminals), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.1 Creating Frames [¶](#Creating-Frames-1)

To create a new frame, call the function `make-frame`.

Command: **make-frame** &optional parameters [¶](#index-make_002dframe)
:   This function creates and returns a new frame, displaying the current
    buffer.

    The parameters argument is an alist that specifies frame
    parameters for the new frame. See [Frame Parameters](#Frame-Parameters). If you specify
    the `terminal` parameter in parameters, the new frame is
    created on that terminal. Otherwise, if you specify the
    `window-system` frame parameter in parameters, that
    determines whether the frame should be displayed on a text terminal or a
    graphical terminal. See [Window Systems](#Window-Systems). If neither is specified,
    the new frame is created in the same terminal as the selected frame.

    Any parameters not mentioned in parameters default to the values
    in the alist `default-frame-alist` (see [Initial Frame Parameters](#Initial-Parameters));
    parameters not specified there default from the X resources or its
    equivalent on your operating system (see [X Resources](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#X-Resources) in The GNU Emacs Manual). After the frame is created, this
    function applies any parameters specified in
    `frame-inherited-parameters` (see below) it has no assigned yet,
    taking the values from the frame that was selected when
    `make-frame` was called.

    Note that on multi-monitor displays (see [Multiple Terminals](#Multiple-Terminals)), the
    window manager might position the frame differently than specified by
    the positional parameters in parameters (see [Position Parameters](#Position-Parameters)). For example, some window managers have a policy of
    displaying the frame on the monitor that contains the largest part of
    the window (a.k.a. the *dominating* monitor).

    This function itself does not make the new frame the selected frame.
    See [Input Focus](#Input-Focus). The previously selected frame remains selected.
    On graphical terminals, however, the window system may select the
    new frame for its own reasons.

    By default this function does not display the current buffer in the new
    frame if the buffer is hidden, that is, if its name starts with a space.
    In this case it will show another buffer—one that could be returned by
    the function `other-buffer` (see [The Buffer List](#Buffer-List))—instead.
    However, if the variable `expose-hidden-buffer` is non-`nil`,
    this function will display the current buffer even if it is hidden.

Variable: **before-make-frame-hook** [¶](#index-before_002dmake_002dframe_002dhook)
:   A normal hook run by `make-frame` before it creates the frame.

Variable: **after-make-frame-functions** [¶](#index-after_002dmake_002dframe_002dfunctions)
:   An abnormal hook run by `make-frame` after it created the frame.
    Each function in `after-make-frame-functions` receives one
    argument, the frame just created.

    You can consult the frame parameters `cloned-from` and
    `undeleted` in your function to determine if a frame was cloned
    using `clone-frame`, or if it was undeleted using
    `undelete-frame`. See [Frame Parameters](#Frame-Parameters).

Note that any functions added to these hooks by your initial file are
usually not run for the initial frame, since Emacs reads the initial
file only after creating that frame. However, if the initial frame is
specified to use a separate minibuffer frame (see [Minibuffers and Frames](#Minibuffers-and-Frames)), the functions will be run for both, the minibuffer-less and
the minibuffer frame. Alternatively, you can add functions to these
hooks in your “early init file” (see [The Init File](#Init-File)), in which case
they will be in effect for the initial frame as well.

Variable: **frame-inherited-parameters** [¶](#index-frame_002dinherited_002dparameters)
:   This variable specifies the list of frame parameters that a newly
    created frame inherits from the currently selected frame. For each
    parameter (a symbol) that is an element in this list and has not been
    assigned earlier when processing `make-frame`, the function sets
    the value of that parameter in the created frame to its value in the
    selected frame.

User Option: **server-after-make-frame-hook** [¶](#index-server_002dafter_002dmake_002dframe_002dhook)
:   A normal hook run when the Emacs server starts using a client frame.
    When this hook is called, the client frame is the selected one. Note
    that, depending on how `emacsclient` was invoked
    (see [Invoking emacsclient](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Invoking-emacsclient) in The GNU Emacs Manual), this
    client frame could be a new frame created for the client, or it could
    be an existing frame that the server reused for handling the client
    commands. See [Emacs Server](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Emacs-Server) in The GNU Emacs Manual.

---

Next: [Frame Geometry](#Frame-Geometry), Previous: [Creating Frames](#Creating-Frames), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
