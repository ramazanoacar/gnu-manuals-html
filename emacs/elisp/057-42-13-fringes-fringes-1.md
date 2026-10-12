filename
    :   The file name of the font as a string. This can be `nil` if the
        font back-end does not provide a way to find out the font’s file name.

    capability
    :   A list whose first element is a symbol representing the font type, one
        of `x`, `opentype`, `truetype`, `type1`,
        `pcf`, or `bdf`. For OpenType fonts, the list includes 2
        additional elements describing the GSUB and GPOS features
        supported by the font. Each of these elements is a list of the form
        `((script (langsys feature …) …)
        …)`, where script is a symbol representing an OpenType
        script tag, langsys is a symbol representing an OpenType langsys
        tag (or `nil`, which stands for the default langsys), and each
        feature is a symbol representing an OpenType feature tag.

Function: **query-font** font-object [¶](#index-query_002dfont)
:   This function returns information about a font-object. (This is
    in contrast to `font-info`, which takes the font name, a string,
    as its argument.)

    The value returned by the function is a vector of the form
    `[name filename pixel-size max-width
    ascent descent space-width average-width
    capability]`. Here’s the description of each components of this
    vector:

    name
    :   The font name, a string.

    filename
    :   The file name of the font as a string. This can be `nil` if the
        font back-end does not provide a way to find out the font’s file name.

    pixel-size
    :   The pixel size of the font used to open the font.

    max-width
    :   The maximum advance width of the font.

    ascent

    descent
    :   The ascent and descent of this font. The sum of these two numbers
        gives the font height.

    space-width
    :   The width, in pixels, of the font’s space character.

    average-width
    :   The average width of the font characters. If this is zero, Emacs uses
        the value of space-width instead, when it calculates text layout
        on display.

    capability
    :   A list whose first element is a symbol representing the font type, one
        of `x`, `opentype`, `truetype`, `type1`,
        `pcf`, or `bdf`. For OpenType fonts, the list includes 2
        additional elements describing the GSUB and GPOS features
        supported by the font. Each of these elements is a list of the form
        `((script (langsys feature …) …)
        …)`, where script is a symbol representing an OpenType
        script tag, langsys is a symbol representing an OpenType langsys
        tag (or `nil`, which stands for the default langsys), and each
        feature is a symbol representing an OpenType feature tag.

The following four functions return size information about fonts used
by various faces, allowing various layout considerations in Lisp
programs. These functions take face remapping into consideration,
returning information about the remapped face, if the face in question
was remapped. See [Face Remapping](#Face-Remapping).

Function: **default-font-width** [¶](#index-default_002dfont_002dwidth)
:   This function returns the average width in pixels of the font used by
    the current buffer’s default face, as that face is defined for the
    selected frame.

Function: **default-font-height** [¶](#index-default_002dfont_002dheight)
:   This function returns the height in pixels of the font used by the
    current buffer’s default face, as that face is defined for the
    selected frame.

Function: **window-font-width** &optional window face [¶](#index-window_002dfont_002dwidth)
:   This function returns the average width in pixels for the font used by
    face in window. The specified window must be a live
    window. If `nil` or omitted, window defaults to the
    selected window, and face defaults to the default face in
    window.

Function: **window-font-height** &optional window face [¶](#index-window_002dfont_002dheight)
:   This function returns the height in pixels for the font used by
    face in window. The specified window must be a live
    window. If `nil` or omitted, window defaults to the
    selected window, and face defaults to the default face in
    window.

---

Next: [Scroll Bars](#Scroll-Bars), Previous: [Faces](#Faces), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.13 Fringes [¶](#Fringes-1)

On graphical displays, Emacs draws *fringes* next to each
window: thin vertical strips down the sides which can display bitmaps
indicating truncation, continuation, horizontal scrolling, and so on.

* [Fringe Size and Position](#Fringe-Size_002fPos)
* [Fringe Indicators](#Fringe-Indicators)
* [Fringe Cursors](#Fringe-Cursors)
* [Fringe Bitmaps](#Fringe-Bitmaps)
* [Customizing Fringe Bitmaps](#Customizing-Bitmaps)
* [The Overlay Arrow](#Overlay-Arrow)

---

Next: [Fringe Indicators](#Fringe-Indicators), Up: [Fringes](#Fringes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.13.1 Fringe Size and Position [¶](#Fringe-Size-and-Position)

The following buffer-local variables control the position and width
of fringes in windows showing that buffer.

Variable: **fringes-outside-margins** [¶](#index-fringes_002doutside_002dmargins)
:   The fringes normally appear between the display margins and the window
    text. If the value is non-`nil`, they appear outside the display
    margins. See [Displaying in the Margins](#Display-Margins).

Variable: **left-fringe-width** [¶](#index-left_002dfringe_002dwidth)
:   This variable, if non-`nil`, specifies the width of the left
    fringe in pixels. A value of `nil` means to use the left fringe
    width from the window’s frame.

Variable: **right-fringe-width** [¶](#index-right_002dfringe_002dwidth)
:   This variable, if non-`nil`, specifies the width of the right
    fringe in pixels. A value of `nil` means to use the right fringe
    width from the window’s frame.

Any buffer which does not specify values for these variables uses
the values specified by the `left-fringe` and `right-fringe`
frame parameters (see [Layout Parameters](#Layout-Parameters)).

The above variables actually take effect via the function
`set-window-buffer` (see [Buffers and Windows](#Buffers-and-Windows)), which calls
`set-window-fringes` as a subroutine. If you change one of these
variables, the fringe display is not updated in existing windows
showing the buffer, unless you call `set-window-buffer` again in
each affected window. You can also use `set-window-fringes` to
control the fringe display in individual windows.

Function: **set-window-fringes** window left &optional right outside-margins persistent [¶](#index-set_002dwindow_002dfringes)
:   This function sets the fringe widths of window window.
    If window is `nil`, the selected window is used.

    The argument left specifies the width in pixels of the left
    fringe, and likewise right for the right fringe. A value of
    `nil` for either one stands for the default width. If
    outside-margins is non-`nil`, that specifies that fringes
    should appear outside of the display margins.

    If window is not large enough to accommodate fringes of the
    desired width, this leaves the fringes of window unchanged.

    The values specified here may be later overridden by invoking
    `set-window-buffer` (see [Buffers and Windows](#Buffers-and-Windows)) on window
    with its keep-margins argument `nil` or omitted. However,
    if the optional fifth argument persistent is non-`nil` and
    the other arguments are processed successfully, the values specified
    here unconditionally survive subsequent invocations of
    `set-window-buffer`. This can be used to permanently turn off
    fringes in the minibuffer window, consult the description of
    `set-window-scroll-bars` for an example (see [Scroll Bars](#Scroll-Bars)).

Function: **window-fringes** &optional window [¶](#index-window_002dfringes)
:   This function returns information about the fringes of a window
    window. If window is omitted or `nil`, the selected
    window is used. The value has the form `(left-width
    right-width outside-margins persistent)`.

---

Next: [Fringe Cursors](#Fringe-Cursors), Previous: [Fringe Size and Position](#Fringe-Size_002fPos), Up: [Fringes](#Fringes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.13.2 Fringe Indicators [¶](#Fringe-Indicators-1)

*Fringe indicators* are tiny icons displayed in the window
fringe to indicate truncated or continued lines, buffer boundaries,
etc.

User Option: **indicate-empty-lines** [¶](#index-indicate_002dempty_002dlines)
:   When this is non-`nil`, Emacs displays a special glyph in the
    fringe of each empty line at the end of the buffer, on graphical
    displays. See [Fringes](#Fringes). This variable is automatically
    buffer-local in every buffer.

User Option: **indicate-buffer-boundaries** [¶](#index-indicate_002dbuffer_002dboundaries)
:   This buffer-local variable controls how the buffer boundaries and
    window scrolling are indicated in the window fringes.

    Emacs can indicate the buffer boundaries—that is, the first and last
    line in the buffer—with angle icons when they appear on the screen.
    In addition, Emacs can display an up-arrow in the fringe to show
    that there is text above the screen, and a down-arrow to show
    there is text below the screen.

    There are three kinds of basic values:

    `nil`
    :   Don’t display any of these fringe icons.

    `left`
    :   Display the angle icons and arrows in the left fringe.

    `right`
    :   Display the angle icons and arrows in the right fringe.

    any non-alist
    :   Display the angle icons in the left fringe
        and don’t display the arrows.

    Otherwise the value should be an alist that specifies which fringe
    indicators to display and where. Each element of the alist should
    have the form `(indicator . position)`. Here,
    indicator is one of `top`, `bottom`, `up`,
    `down`, and `t` (which covers all the icons not yet
    specified), while position is one of `left`, `right`
    and `nil`.

    For example, `((top . left) (t . right))` places the top angle
    bitmap in left fringe, and the bottom angle bitmap as well as both
    arrow bitmaps in right fringe. To show the angle bitmaps in the left
    fringe, and no arrow bitmaps, use `((top . left) (bottom . left))`.

Variable: **fringe-indicator-alist** [¶](#index-fringe_002dindicator_002dalist)
:   This buffer-local variable specifies the mapping from logical fringe
    indicators to the actual bitmaps displayed in the window fringes. The
    value is an alist of elements `(indicator
    . bitmaps)`, where indicator specifies a logical indicator
    type and bitmaps specifies the fringe bitmaps to use for that
    indicator.

    Each indicator should be one of the following symbols:

    `truncation`, `continuation`.
    :   Used for truncation and continuation lines.

    `up`, `down`, `top`, `bottom`, `top-bottom`
    :   Used when `indicate-buffer-boundaries` is non-`nil`:
        `up` and `down` indicate a buffer boundary lying above or
        below the window edge; `top` and `bottom` indicate the
        topmost and bottommost buffer text line; and `top-bottom`
        indicates where there is just one line of text in the buffer.

    `empty-line`
    :   Used to indicate empty lines after the buffer end when
        `indicate-empty-lines` is non-`nil`.

    `overlay-arrow`
    :   Used for overlay arrows (see [The Overlay Arrow](#Overlay-Arrow)).

    Each bitmaps value may be a list of symbols `(left
    right [left1 right1])`. The left and
    right symbols specify the bitmaps shown in the left and/or right
    fringe, for the specific indicator. left1 and right1 are
    specific to the `bottom` and `top-bottom` indicators, and
    are used to indicate that the last text line has no final newline.
    Alternatively, bitmaps may be a single symbol which is used in
    both left and right fringes.

    See [Fringe Bitmaps](#Fringe-Bitmaps), for a list of standard bitmap symbols and how
    to define your own. In addition, `nil` represents the empty
    bitmap (i.e., an indicator that is not shown).

    When `fringe-indicator-alist` has a buffer-local value, and
    there is no bitmap defined for a logical indicator, or the bitmap is
    `t`, the corresponding value from the default value of
    `fringe-indicator-alist` is used.

---

Next: [Fringe Bitmaps](#Fringe-Bitmaps), Previous: [Fringe Indicators](#Fringe-Indicators), Up: [Fringes](#Fringes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.13.3 Fringe Cursors [¶](#Fringe-Cursors-1)

When a line is exactly as wide as the window, Emacs displays the
cursor in the right fringe instead of using two lines. Different
bitmaps are used to represent the cursor in the fringe depending on
the current buffer’s cursor type.

User Option: **overflow-newline-into-fringe** [¶](#index-overflow_002dnewline_002dinto_002dfringe)
:   If this is non-`nil`, lines exactly as wide as the window (not
    counting the final newline character) are not continued. Instead,
    when point is at the end of the line, the cursor appears in the right
    fringe.

Variable: **fringe-cursor-alist** [¶](#index-fringe_002dcursor_002dalist)
:   This variable specifies the mapping from logical cursor type to the
    actual fringe bitmaps displayed in the right fringe. The value is an
    alist where each element has the form `(cursor-type
    . bitmap)`, which means to use the fringe bitmap bitmap to
    display cursors of type cursor-type.

    Each cursor-type should be one of `box`, `hollow`,
    `bar`, `hbar`, or `hollow-small`. The first four have
    the same meanings as in the `cursor-type` frame parameter
    (see [Cursor Parameters](#Cursor-Parameters)). The `hollow-small` type is used
    instead of `hollow` when the normal `hollow-rectangle`
    bitmap is too tall to fit on a specific display line.

    Each bitmap should be a symbol specifying the fringe bitmap to
    be displayed for that logical cursor type.
    See [Fringe Bitmaps](#Fringe-Bitmaps).

    When `fringe-cursor-alist` has a buffer-local value, and there is
    no bitmap defined for a cursor type, the corresponding value from the
    default value of `fringes-indicator-alist` is used.

---

Next: [Customizing Fringe Bitmaps](#Customizing-Bitmaps), Previous: [Fringe Cursors](#Fringe-Cursors), Up: [Fringes](#Fringes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.13.4 Fringe Bitmaps [¶](#Fringe-Bitmaps-1)

The *fringe bitmaps* are the actual bitmaps which represent the
logical fringe indicators for truncated or continued lines, buffer
boundaries, overlay arrows, etc. Each bitmap is represented by a
symbol.
These symbols are referred to by the variable
`fringe-indicator-alist`, which maps fringe indicators to bitmaps
(see [Fringe Indicators](#Fringe-Indicators)), and the variable
`fringe-cursor-alist`, which maps fringe cursors to bitmaps
(see [Fringe Cursors](#Fringe-Cursors)).

Lisp programs can also directly display a bitmap in the left or
right fringe, by using a `display` property for one of the
characters appearing in the line (see [Other Display Specifications](#Other-Display-Specs)). Such
a display specification has the form

```
(fringe bitmap [face])
```

fringe is either the symbol `left-fringe` or
`right-fringe`. bitmap is a symbol identifying the bitmap
to display. The optional face names a face whose foreground and
background colors are to be used to display the bitmap, using the
attributes of the `fringe` face for colors that face didn’t
specify. If face is omitted, that means to use the attributes
of the `default` face for the colors which the `fringe` face
didn’t specify. For predictable results that don’t depend on the
attributes of the `default` and `fringe` faces, we recommend
you never omit face, but always provide a specific face. In
particular, if you want the bitmap to be always displayed in the
`fringe` face, use `fringe` as face.

For instance, to display an arrow in the left fringe, using the
`warning` face, you could say something like:

```
(overlay-put
 (make-overlay (point) (point))
 'before-string (propertize
                 "x" 'display
                 `(left-fringe right-arrow warning)))
```

Here is a list of the standard fringe bitmaps defined in Emacs, and
how they are currently used in Emacs (via
`fringe-indicator-alist` and `fringe-cursor-alist`):

`left-arrow`, `right-arrow`
:   Used to indicate truncated lines.

`left-curly-arrow`, `right-curly-arrow`
:   Used to indicate continued lines.

`right-triangle`, `left-triangle`
:   The former is used by overlay arrows. The latter is unused.

`up-arrow`, `down-arrow`

`bottom-left-angle`, `bottom-right-angle`

`top-left-angle`, `top-right-angle`

`left-bracket`, `right-bracket`

`empty-line`
:   Used to indicate buffer boundaries.

`filled-rectangle`, `hollow-rectangle`

`filled-square`, `hollow-square`

`vertical-bar`, `horizontal-bar`
:   Used for different types of fringe cursors.

`exclamation-mark`, `question-mark`

`large-circle`
:   Not used by core Emacs features.

The next subsection describes how to define your own fringe bitmaps.

Function: **fringe-bitmaps-at-pos** &optional pos window [¶](#index-fringe_002dbitmaps_002dat_002dpos)
:   This function returns the fringe bitmaps of the display line
    containing position pos in window window. The return
    value has the form `(left right ov)`, where left
    is the symbol for the fringe bitmap in the left fringe (or `nil`
    if no bitmap), right is similar for the right fringe, and ov
    is non-`nil` if there is an overlay arrow in the left fringe.

    The value is `nil` if pos is not visible in window.
    If window is `nil`, that stands for the selected window.
    If pos is `nil`, that stands for the value of point in
    window.

---

Next: [The Overlay Arrow](#Overlay-Arrow), Previous: [Fringe Bitmaps](#Fringe-Bitmaps), Up: [Fringes](#Fringes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.13.5 Customizing Fringe Bitmaps [¶](#Customizing-Fringe-Bitmaps)

Function: **define-fringe-bitmap** bitmap bits &optional height width align [¶](#index-define_002dfringe_002dbitmap)
:   This function defines the symbol bitmap as a new fringe bitmap,
    or replaces an existing bitmap with that name.

    The argument bits specifies the image to use. It should be
    either a string or a vector of integers, where each element (an
    integer) corresponds to one row of the bitmap. Each bit of an integer
    corresponds to one pixel of the bitmap, where the low bit corresponds
    to the rightmost pixel of the bitmap. (Note that this order of bits
    is the opposite of the order in XBM images; see [XBM Images](#XBM-Images).)

    The height is normally the length of bits. However, you
    can specify a different height with non-`nil` height. The width
    is normally 8, but you can specify a different width with non-`nil`
    width. The width must be an integer between 1 and 16.

    The argument align specifies the positioning of the bitmap
    relative to the range of rows where it is used; the default is to
    center the bitmap. The allowed values are `top`, `center`,
    or `bottom`.

    The align argument may also be a list `(align
    periodic)` where align is interpreted as described above.
    If periodic is non-`nil`, it specifies that the rows in
    `bits` should be repeated enough times to reach the specified
    height.

Function: **destroy-fringe-bitmap** bitmap [¶](#index-destroy_002dfringe_002dbitmap)
:   This function destroys the fringe bitmap identified by bitmap.
    If bitmap identifies a standard fringe bitmap, it actually
    restores the standard definition of that bitmap, instead of
    eliminating it entirely.

Function: **set-fringe-bitmap-face** bitmap &optional face [¶](#index-set_002dfringe_002dbitmap_002dface)
:   This sets the face for the fringe bitmap bitmap to face.
    If face is `nil`, it selects the `fringe` face. The
    bitmap’s face controls the color to draw it in.

    face is merged with the `fringe` face, so normally
    face should specify only the foreground color.

---

Previous: [Customizing Fringe Bitmaps](#Customizing-Bitmaps), Up: [Fringes](#Fringes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.13.6 The Overlay Arrow [¶](#The-Overlay-Arrow)

The *overlay arrow* is useful for directing the user’s attention
to a particular line in a buffer. For example, in the modes used for
interface to debuggers, the overlay arrow indicates the line of code
about to be executed. This feature has nothing to do with
*overlays* (see [Overlays](#Overlays)).

Variable: **overlay-arrow-string** [¶](#index-overlay_002darrow_002dstring)
:   This variable holds the string to display to call attention to a
    particular line, or `nil` if the arrow feature is not in use.
    On a graphical display the contents of the string are ignored if the
    left fringe is shown; instead a glyph is displayed in the fringe area
    to the left of the display area.

Variable: **overlay-arrow-position** [¶](#index-overlay_002darrow_002dposition)
:   This variable holds a marker that indicates where to display the overlay
    arrow. It should point at the beginning of a line. On a non-graphical
    display, or when the left fringe is not shown, the arrow text
    appears at the beginning of that line, overlaying any text that would
    otherwise appear. Since the arrow is usually short, and the line
    usually begins with indentation, normally nothing significant is
    overwritten.

    The overlay-arrow string is displayed in any given buffer if the value
    of `overlay-arrow-position` in that buffer points into that
    buffer. Thus, it is possible to display multiple overlay arrow strings
    by creating buffer-local bindings of `overlay-arrow-position`.
    However, it is usually cleaner to use
    `overlay-arrow-variable-list` to achieve this result.

You can do a similar job by creating an overlay with a
`before-string` property. See [Overlay Properties](#Overlay-Properties).

You can define multiple overlay arrows via the variable
`overlay-arrow-variable-list`.

Variable: **overlay-arrow-variable-list** [¶](#index-overlay_002darrow_002dvariable_002dlist)
:   This variable’s value is a list of variables, each of which specifies
    the position of an overlay arrow. The variable
    `overlay-arrow-position` has its normal meaning because it is on
    this list.

Each variable on this list can have properties
`overlay-arrow-string` and `overlay-arrow-bitmap` that
specify an overlay arrow string (for text terminals or graphical
terminals without the left fringe shown) or fringe bitmap
(for graphical terminals with a left fringe) to display at the
corresponding overlay arrow position. If either property is not set,
the default `overlay-arrow-string` or `overlay-arrow` fringe
indicator is used.

---

Next: [Window Dividers](#Window-Dividers), Previous: [Fringes](#Fringes), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.14 Scroll Bars [¶](#Scroll-Bars-1)

Normally the frame parameter `vertical-scroll-bars` controls
whether the windows in the frame have vertical scroll bars, and whether
they are on the left or right. The frame parameter
`scroll-bar-width` specifies how wide they are (`nil` meaning
the default).

The frame parameter `horizontal-scroll-bars` controls whether
the windows in the frame have horizontal scroll bars. The frame
parameter `scroll-bar-height` specifies how high they are
(`nil` meaning the default). See [Layout Parameters](#Layout-Parameters).

Horizontal scroll bars are not available on all platforms. The
function `horizontal-scroll-bars-available-p` which takes no
argument returns non-`nil` if they are available on your system.

The following three functions take as argument a live frame which
defaults to the selected one.

Function: **frame-current-scroll-bars** &optional frame [¶](#index-frame_002dcurrent_002dscroll_002dbars)
:   This function reports the scroll bar types for frame frame. The
    value is a cons cell `(vertical-type .
    horizontal-type)`, where vertical-type is either
    `left`, `right`, or `nil` (which means no vertical scroll
    bar.) horizontal-type is either `bottom` or `nil`
    (which means no horizontal scroll bar).

Function: **frame-scroll-bar-width** &optional frame [¶](#index-frame_002dscroll_002dbar_002dwidth)
:   This function returns the width of vertical scroll bars of frame
    in pixels.

Function: **frame-scroll-bar-height** &optional frame [¶](#index-frame_002dscroll_002dbar_002dheight)
:   This function returns the height of horizontal scroll bars of
    frame in pixels.

You can override the frame specific settings for individual windows by
using the following function:

Function: **set-window-scroll-bars** window &optional width vertical-type height horizontal-type persistent [¶](#index-set_002dwindow_002dscroll_002dbars)
:   This function sets the width and/or height and the types of scroll bars
    for window window. If window is `nil`, the selected
    window is used.

    width specifies the width of the vertical scroll bar in pixels
    (`nil` means use the width specified for the frame).
    vertical-type specifies whether to have a vertical scroll bar and,
    if so, where. The possible values are `left`, `right`,
    `t`, which means to use the frame’s default, and `nil` for no
    vertical scroll bar.

    height specifies the height of the horizontal scroll bar in
    pixels (`nil` means use the height specified for the frame).
    horizontal-type specifies whether to have a horizontal scroll
    bar. The possible values are `bottom`, `t`, which means to
    use the frame’s default, and `nil` for no horizontal scroll bar.
    Note that for a mini window the value `t` has the same meaning as
    `nil`, namely to not show a horizontal scroll bar. You have to
    explicitly specify `bottom` in order to show a horizontal scroll
    bar in a mini window.

    If window is not large enough to accommodate a scroll bar of the
    desired dimension, this leaves the corresponding scroll bar unchanged.

    The values specified here may be later overridden by invoking
    `set-window-buffer` (see [Buffers and Windows](#Buffers-and-Windows)) on window
    with its keep-margins argument `nil` or omitted. However,
    if the optional fifth argument persistent is non-`nil` and
    the other arguments are processed successfully, the values specified
    here unconditionally survive subsequent invocations of
    `set-window-buffer`.

Using the persistent argument of `set-window-scroll-bars`
and `set-window-fringes` (see [Fringe Size and Position](#Fringe-Size_002fPos)) you can
reliably and permanently turn off scroll bars and/or fringes in any
minibuffer window by adding the following snippet to your early init
file (see [The Init File](#Init-File)).

```
(add-hook 'after-make-frame-functions
          (lambda (frame)
            (set-window-scroll-bars
             (minibuffer-window frame) 0 nil 0 nil t)
            (set-window-fringes
             (minibuffer-window frame) 0 0 nil t)))
```

The following four functions take as argument a live window which
defaults to the selected one.

Function: **window-scroll-bars** &optional window [¶](#index-window_002dscroll_002dbars)
:   This function returns a list of the form `(width
    columns vertical-type height lines
    horizontal-type persistent)`.

    The value width is the value that was specified for the width of
    the vertical scroll bar (which may be `nil`); columns is the
    (possibly rounded) number of columns that the vertical scroll bar
    actually occupies.

    The value height is the value that was specified for the height of
    the horizontal scroll bar (which may be `nil`); lines is the
    (possibly rounded) number of lines that the horizontally scroll bar
    actually occupies.

    The value of persistent is the value specified for window
    with the last successful invocation of `set-window-scroll-bars`,
    `nil` if there never was one.

Function: **window-current-scroll-bars** &optional window [¶](#index-window_002dcurrent_002dscroll_002dbars)
:   This function reports the scroll bar type for window window. The
    value is a cons cell `(vertical-type .
    horizontal-type)`. Unlike `window-scroll-bars`, this reports
    the scroll bar type actually used, once frame defaults and
    `scroll-bar-mode` are taken into account.

Function: **window-scroll-bar-width** &optional window [¶](#index-window_002dscroll_002dbar_002dwidth)
:   This function returns the width in pixels of window’s vertical
    scrollbar.

Function: **window-scroll-bar-height** &optional window [¶](#index-window_002dscroll_002dbar_002dheight)
:   This function returns the height in pixels of window’s horizontal
    scrollbar.

If you do not specify a window’s scroll bar settings via
`set-window-scroll-bars`, the buffer-local variables
`vertical-scroll-bar`, `horizontal-scroll-bar`,
`scroll-bar-width` and `scroll-bar-height` in the buffer being
displayed control the window’s scroll bars. The function
`set-window-buffer` examines these variables. If you change them
in a buffer that is already visible in a window, you can make the window
take note of the new values by calling `set-window-buffer`
specifying the same buffer that is already displayed.

You can control the appearance of scroll bars for a particular buffer by
setting the following variables which automatically become buffer-local
when set.

Variable: **vertical-scroll-bar** [¶](#index-vertical_002dscroll_002dbar)
:   This variable specifies the location of the vertical scroll bar. The
    possible values are `left`, `right`, `t`, which means to
    use the frame’s default, and `nil` for no scroll bar.

Variable: **horizontal-scroll-bar** [¶](#index-horizontal_002dscroll_002dbar)
:   This variable specifies the location of the horizontal scroll bar. The
    possible values are `bottom`, `t`, which means to use the
    frame’s default, and `nil` for no scroll bar.

Variable: **scroll-bar-width** [¶](#index-scroll_002dbar_002dwidth)
:   This variable specifies the width of the buffer’s vertical scroll bars,
    measured in pixels. A value of `nil` means to use the value
    specified by the frame.

Variable: **scroll-bar-height** [¶](#index-scroll_002dbar_002dheight)
:   This variable specifies the height of the buffer’s horizontal scroll
    bar, measured in pixels. A value of `nil` means to use the value
    specified by the frame.

Finally you can toggle the display of scroll bars on all frames by
customizing the variables `scroll-bar-mode` and
`horizontal-scroll-bar-mode`.

User Option: **scroll-bar-mode** [¶](#index-scroll_002dbar_002dmode)
:   This variable controls whether and where to put vertical scroll bars in
    all frames. The possible values are `nil` for no scroll bars,
    `left` to put scroll bars on the left and `right` to put
    scroll bars on the right.

User Option: **horizontal-scroll-bar-mode** [¶](#index-horizontal_002dscroll_002dbar_002dmode)
:   This variable controls whether to display horizontal scroll bars on all
    frames.

---

Next: [The `display` Property](#Display-Property), Previous: [Scroll Bars](#Scroll-Bars), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.15 Window Dividers [¶](#Window-Dividers-1)

Window dividers are bars drawn between a frame’s windows. A right
divider is drawn between a window and any adjacent windows on the right.
Its width (thickness) is specified by the frame parameter
`right-divider-width`. A bottom divider is drawn between a
window and adjacent windows on the bottom or the echo area. Its width
is specified by the frame parameter `bottom-divider-width`. In
either case, specifying a width of zero means to not draw such dividers.
See [Layout Parameters](#Layout-Parameters).

Technically, a right divider belongs to the window on its left,
which means that its width contributes to the total width of that
window. A bottom divider belongs to the window above it, which
means that its width contributes to the total height of that window.
See [Window Sizes](#Window-Sizes). When a window has both, a right and a bottom
divider, the bottom divider prevails. This means that a bottom
divider is drawn over the full total width of its window while the right
divider ends above the bottom divider.

Dividers can be dragged with the mouse and are therefore useful for
adjusting the sizes of adjacent windows with the mouse. They also serve
to visually set apart adjacent windows when no scroll bars or mode lines
are present. The following three faces allow the customization of the
appearance of dividers:

`window-divider`
:   When a divider is less than three pixels wide, it is drawn solidly with
    the foreground of this face. For larger dividers this face is used for
    the inner part only, excluding the first and last pixel.

`window-divider-first-pixel`
:   This is the face used for drawing the first pixel of a divider that is
    at least three pixels wide. To obtain a solid appearance, set this to
    the same value used for the `window-divider` face.

`window-divider-last-pixel`
:   This is the face used for drawing the last pixel of a divider that is at
    least three pixels wide. To obtain a solid appearance, set this to the
    same value used for the `window-divider` face.

You can get the sizes of the dividers of a specific window with the
following two functions.

Function: **window-right-divider-width** &optional window [¶](#index-window_002dright_002ddivider_002dwidth)
:   Return the width (thickness) in pixels of window’s right divider.
    window must be a live window and defaults to the selected one.
    The return value is always zero for a rightmost window.

Function: **window-bottom-divider-width** &optional window [¶](#index-window_002dbottom_002ddivider_002dwidth)
:   Return the width (thickness) in pixels of window’s bottom divider.
    window must be a live window and defaults to the selected one.
    The return value is zero for the minibuffer window or a bottommost
    window on a minibuffer-less frame.

---

Next: [Images](#Images), Previous: [Window Dividers](#Window-Dividers), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.16 The `display` Property [¶](#The-display-Property)

The `display` text property (or overlay property) is used to
insert images into text, and to control other aspects of how text
displays. Display specifications in the same `display`
property value generally apply in parallel to the text they cover.

If several sources (overlays and/or a text property) specify values
for the `display` property, only one of the values takes effect,
following the rules of `get-char-property`. See [Examining Text Properties](#Examining-Properties).

The value of the `display` property should be a display
specification, or a list or vector containing several display
specifications.

Function: **get-display-property** position spec &optional object properties [¶](#index-get_002ddisplay_002dproperty)
:   This convenience function can be used to get the value of a specific
    display specification, no matter whether the `display` property is
    a vector, a list or a single display specification. This is like
    `get-text-property` (see [Examining Text Properties](#Examining-Properties)), but works on
    the `display` property only. For specifications with a single
    value (e.g. `height`), this returns the value itself; for
    properties with a list of values (e.g. `slice`), this returns the
    list of values.

    position is the position in the buffer or string to examine, and
    spec is the CAR of the display specification to return. The
    optional object argument should be either a string or a buffer,
    and defaults to the current buffer. If the optional properties
    argument is non-`nil`, it should be a `display` property, and
    in that case, position and object are ignored. (This can be
    useful if you’ve already gotten the `display` property with
    `get-char-property`, for instance (see [Examining Text Properties](#Examining-Properties)).

Function: **add-display-text-property** start end spec value &optional object [¶](#index-add_002ddisplay_002dtext_002dproperty)
:   Add the display specification `(prop value)` to the
    text from start to end.

    If any text in the region has a non-`nil` `display`
    property, those properties are retained. For instance:

    ```
    (add-display-text-property 4 8 'height 2.0)
    (add-display-text-property 2 12 'raise 0.5)
    ```

    After doing this, the region from 2 to 4 will have the `raise`
    display specification, the region from 4 to 8 will have both the
    `raise` and `height` display specifications, and finally the
    region from 8 to 12 will only have the `raise` display
    specification.

    object is either a string or a buffer to add the specification to.
    If omitted, object defaults to the current buffer.

Function: **remove-display-text-property** start end spec &optional object [¶](#index-remove_002ddisplay_002dtext_002dproperty)
:   Remove the display specification spec from the text from
    start to end. spec is the CAR of the display
    specification to remove, e.g. `height` or `'(margin nil)`.

    If any text in the region has any other `display` properties, those
    properties are retained. For instance:

    ```
    (add-display-text-property 1 8 'raise 0.5)
    (add-display-text-property 4 8 'height 2.0)
    (remove-display-text-property 2 6 'raise)
    ```

    After doing this, the text will have the following `display`
    properties:

    * The region from 1 to 2, only `raise`
    * The region from 2 to 4, no properties
    * The region from 4 to 6, only `height`
    * The region from 6 to 8, both `raise` and `height`

    object is either a string or a buffer to remove the specification
    from. If omitted, object defaults to the current buffer.

Some of the display specifications allow inclusion of Lisp forms,
which are evaluated at display time. This could be unsafe in certain
situations, e.g., when the display specification was generated by some
external program/agent. Wrapping a display specification in a list
that begins with the special symbol `disable-eval`, as in
`(disable-eval spec)`, will disable evaluation of any
Lisp in spec, while still supporting all the other display
property features.

The rest of this section describes several kinds of
display specifications and what they mean.

* [Display Specs That Replace The Text](#Replacing-Specs)
* [Specified Spaces](#Specified-Space)
* [Pixel Specification for Spaces](#Pixel-Specification)
* [Other Display Specifications](#Other-Display-Specs)
* [Displaying in the Margins](#Display-Margins)

---

Next: [Specified Spaces](#Specified-Space), Up: [The `display` Property](#Display-Property)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.16.1 Display Specs That Replace The Text [¶](#Display-Specs-That-Replace-The-Text)

Some kinds of display specifications specify something to display
instead of the text that has the property. These are called
*replacing* display specifications. Emacs does not allow the user
to interactively move point into the middle of buffer text that is
replaced in this way.

If a list of display specifications includes more than one replacing
display specification, the first overrides the rest. Replacing
display specifications make most other display specifications
irrelevant, since those don’t apply to the replacement. In addition,
any `invisible` and `composition` properties of the text that
is replaced are ignored, because the replaced text is skipped and its
properties are not processed.

For replacing display specifications, *the text that has the
property* means all the consecutive characters that have the same
Lisp object as their `display` property; these characters are
replaced as a single unit. If two characters have different Lisp
objects as their `display` properties (i.e., objects which are
not `eq`), they are handled separately.

Here is an example which illustrates this point. A string serves as
a replacing display specification, which replaces the text that has
the property with the specified string (see [Other Display Specifications](#Other-Display-Specs)).
Consider the following function:

```
(defun foo ()
  (dotimes (i 5)
    (let ((string (concat "A"))
          (start (+ i i (point-min))))
      (put-text-property start (1+ start) 'display string)
      (put-text-property start (+ 2 start) 'display string))))
```

This function gives each of the first ten characters in the buffer a
`display` property which is a string `"A"`, but they don’t
all get the same string object. The first two characters get the same
string object, so they are replaced with one ‘`A`’; the fact that
the display property was assigned in two separate calls to
`put-text-property` is irrelevant. Similarly, the next two
characters get a second string (`concat` creates a new string
object), so they are replaced with one ‘`A`’; and so on. Thus, the
ten characters appear as five A’s.

Note: Using `:box` face attribute (see [Face Attributes](#Face-Attributes)) on a
replacing `display` string that is adjacent to normal text with
the same `:box` style can lead to display artifacts when moving
the cursor across the text with this face attribute. These can be
avoided by applying the `:box` attribute directly to the text
being replaced, rather than (or in addition to) the `display`
string itself. Here’s an example:

```
;; Causes display artifacts when moving the cursor across text
(progn
  (put-text-property 1 2 'display (propertize "  [" 'face '(:box t)))
  (put-text-property 2 3 'face '(:box t))
  (put-text-property 3 4 'display (propertize "]  " 'face '(:box t))))
```

```

```

```
;; No display artifacts due to `:box'
(progn
  (add-text-properties 1 2 '(face (:box t) display "  ["))
  (put-text-property 2 3 'face '(:box t))
  (add-text-properties 3 4 '(face (:box t) display "]  ")))
```

---

Next: [Pixel Specification for Spaces](#Pixel-Specification), Previous: [Display Specs That Replace The Text](#Replacing-Specs), Up: [The `display` Property](#Display-Property)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.16.2 Specified Spaces [¶](#Specified-Spaces)

To display a space of specified width and/or height, use a display
specification of the form `(space . props)`, where
props is a property list (a list of alternating properties and
values). You can put this property on one or more consecutive
characters; a space of the specified height and width is displayed in
place of *all* of those characters. These are the properties you
can use in props to specify the weight of the space:

`:width width`
:   If width is a number, it specifies
    that the space width should be width times the normal character
    width. width can also be a *pixel width* specification
    (see [Pixel Specification for Spaces](#Pixel-Specification)).

`:relative-width factor`
:   Specifies that the width of the stretch should be computed from the
    first character in the group of consecutive characters that have the
    same `display` property. The space width is the pixel width of
    that character, multiplied by factor. (On text-mode terminals,
    the “pixel width” of a character is usually 1, but it could be more
    for TABs and double-width CJK characters.)

`:align-to hpos`
:   Specifies that the space should be wide enough to reach the column
    hpos. If hpos is a number, it is a column number, and is
    measured in units of the canonical character width (see [Frame Font](#Frame-Font)). hpos can also be a *pixel width* specification
    (see [Pixel Specification for Spaces](#Pixel-Specification)). When the current line is wider than
    the window, and is either displayed by one or more continuation lines,
    or is truncated and possibly scrolled horizontally (see [Horizontal Scrolling](#Horizontal-Scrolling)), hpos is measured from the beginning of the logical
    line, not from the visual beginning of the screen line. This way,
    alignment produced by `:align-to` is consistent with functions
    that count columns, such as `current-column` and
    `move-to-column` (see [Counting Columns](#Columns)). (There’s a single exception
    from this rule: when `:align-to` is used to specify whitespace of
    the `wrap-prefix` variable or text property, see [Truncation](#Truncation).)

You should use one and only one of the above properties. You can
also specify the height of the space, with these properties:

`:height height`
:   Specifies the height of the space.
    If height is a number, it specifies
    that the space height should be height times the normal character
    height. The height may also be a *pixel height* specification
    (see [Pixel Specification for Spaces](#Pixel-Specification)).

`:relative-height factor`
:   Specifies the height of the space, multiplying the ordinary height
    of the text having this display specification by factor.

`:ascent ascent`
:   If the value of ascent is a non-negative number no greater than
    100, it specifies that ascent percent of the height of the space
    should be considered as the ascent of the space—that is, the part
    above the baseline. The ascent may also be specified in pixel units
    with a *pixel ascent* specification (see [Pixel Specification for Spaces](#Pixel-Specification)).

Don’t use both `:height` and `:relative-height` together.

The `:width` and `:align-to` properties are supported on
non-graphic terminals, but the other space properties in this section
are not.

Note that space properties are treated as paragraph separators for
the purposes of reordering bidirectional text for display.
See [Bidirectional Display](#Bidirectional-Display), for the details.

---

Next: [Other Display Specifications](#Other-Display-Specs), Previous: [Specified Spaces](#Specified-Space), Up: [The `display` Property](#Display-Property)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.16.3 Pixel Specification for Spaces [¶](#Pixel-Specification-for-Spaces)

The value of the `:width`, `:align-to`, `:height`,
and `:ascent` properties can be a special kind of expression that
is evaluated during redisplay. The result of the evaluation is used
as an absolute number of pixels.

The following expressions are supported:

```
  expr ::= num | (num) | unit | elem | pos | image | xwidget | form
  num  ::= integer | float | symbol
  unit ::= in | mm | cm | width | height
```

```
  elem ::= left-fringe | right-fringe | left-margin | right-margin
        |  scroll-bar | text
  pos  ::= left | center | right
  form ::= (num . expr) | (op expr ...)
  op   ::= + | -
```

The form num specifies a fraction of the default frame font
height or width. The form `(num)` specifies an absolute
number of pixels. If num is a symbol, symbol, its
buffer-local variable binding is used; that binding can be either a
number or a cons cell of the forms shown above (including yet another
cons cell whose `car` is a symbol that has a buffer-local
binding).

The `in`, `mm`, and `cm` units specify the number of
pixels per inch, millimeter, and centimeter, respectively. The
`width` and `height` units correspond to the default width
and height of the current face. An image specification of the form
`(image . props)` (see [Image Descriptors](#Image-Descriptors))
corresponds to the width or height of the specified image. Similarly,
an xwidget specification of the form `(xwidget . props)`
stands for the width or height of the specified xwidget.
See [Embedded Native Widgets](#Xwidgets).

The elements `left-fringe`, `right-fringe`,
`left-margin`, `right-margin`, `scroll-bar`, and
`text` specify the width of the corresponding area of the window.
When the window displays line numbers (see [Size of Displayed Text](#Size-of-Displayed-Text)), the width of the `text` area is decreased by the screen
space taken by the line-number display.

The `left`, `center`, and `right` positions can be
used with `:align-to` to specify a position relative to the left
edge, center, or right edge of the text area. When the window
displays line numbers, and `:align-to` is used in display
properties of buffer text (as opposed to header line, see below), the
`left` and the `center` positions are offset to account for
the screen space taken by the line-number display.

Any of the above window elements (except `text`) can also be
used with `:align-to` to specify that the position is relative to
the left edge of the given area. Once the base offset for a relative
position has been set (by the first occurrence of one of these
symbols), further occurrences of these symbols are interpreted as the
width of the specified area. For example, to align to the center of
the left-margin, use

```
:align-to (+ left-margin (0.5 . left-margin))
```

If no specific base offset is set for alignment, it is always
relative to the left edge of the text area. For example,
‘`:align-to 0`’ aligns with the first text column in the text area.
When the window displays line numbers, the text is considered to start
where the space used for line-number display ends.

A value of the form `(num . expr)` stands for the
product of the values of num and expr. For example,
`(2 . in)` specifies a width of 2 inches, while `(0.5 .
image)` specifies half the width (or height) of the specified
image (which should be given by its image spec).

The form `(+ expr ...)` adds up the value of the
expressions. The form `(- expr ...)` negates or subtracts
the value of the expressions.

Text shown in the header line that uses `:align-to` display
specifications is not automatically realigned when
`display-line-numbers-mode` is turned on and off, or when the
width of line numbers on display changes. To arrange for the
header-line text alignment to be updated, thus keeping the header-line
text aligned with the buffer text, turn on the
`header-line-indent-mode` in the buffer and use its two
variables, `header-line-indent` and
`header-line-indent-width`, in the display specification.
See [Window Header Lines](#Header-Lines). Here’s a simple example:

```
(setq header-line-format
      (concat (propertize " "
                          'display
                          '(space :align-to
                                  (+ header-line-indent-width 10)))
              "Column"))
```

This will keep the text ‘`Column`’ on the header line aligned with
column 10 of buffer text, regardless of whether
`display-line-numbers-mode` is on or off, and also when
line-number display changes its width.

---

Next: [Displaying in the Margins](#Display-Margins), Previous: [Pixel Specification for Spaces](#Pixel-Specification), Up: [The `display` Property](#Display-Property)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.16.4 Other Display Specifications [¶](#Other-Display-Specifications)

Here are the other sorts of display specifications that you can use
in the `display` text property.

`string`
:   Display string instead of the text that has this property.

    Recursive display specifications are not supported—string’s
    `display` properties, if any, are not used.

`(image . image-props)`
:   This kind of display specification is an image descriptor (see [Image Descriptors](#Image-Descriptors)).
    When used as a display specification, it means to display the image
    instead of the text that has the display specification.

`(slice x y width height)`
:   This specification together with `image` specifies a *slice*
    (a partial area) of the image to display. More precisely, the
    specification should have the following form:

    ```
     ((slice x y width height) image-desc)
    ```

    where image-desc is an image descriptor described above. The
    elements x and y specify the top left corner of the slice,
    within the image; width and height specify the width and
    height of the slice. Integers are numbers of pixels. A floating-point
    number in the range 0.0–1.0 stands for that fraction of the width or
    height of the entire image.

`((margin nil) string)`
:   A display specification of this form means to display string
    instead of the text that has the display specification, at the same
    position as that text. It is equivalent to using just string,
    but it is done as a special case of marginal display (see [Displaying in the Margins](#Display-Margins)).

`(left-fringe bitmap [face])`

`(right-fringe bitmap [face])`
:   This display specification on any character of a line of text causes
    the specified bitmap be displayed in the left or right fringes
    for that line, instead of the characters that have the display
    specification. The optional face specifies the face whose
    colors are to be used for the bitmap display. See [Fringe Bitmaps](#Fringe-Bitmaps),
    for the details.

    It also possible to add context help for fringe bitmaps through the
    `show-help-function` mechanism by using `left-fringe-help` and
    `right-fringe-help` text properties (see [Properties with Special Meanings](#Special-Properties)).

`(space-width factor)`
:   This display specification affects all the space characters within the
    text that has the specification. It displays all of these spaces
    factor times as wide as normal. The element factor should
    be an integer or float. Characters other than spaces are not affected
    at all; in particular, this has no effect on tab characters.

`(min-width (width))`
:   This display specification ensures the text that has it takes at least
    width space on display, by adding a stretch of white space to
    the end of the text if the text is shorter than width. The text
    is partitioned using the identity of the parameter, which is why the
    parameter is a list with one element. For instance:

    ```
    (insert (propertize "foo" 'display '(min-width (6.0))))
    ```

    This will add padding after ‘`foo`’ bringing the total width up to
    the width of six normal characters. Note that the affected characters
    are identified by the `(6.0)` list in the display property,
    compared with `eq`. The element width can be either an
    integer or a float specifying the required minimum width of the text
    (see [Pixel Specification for Spaces](#Pixel-Specification)).

`(height height)`
:   This display specification makes the text taller or shorter.
    Here are the possibilities for height:

    `(+ n)`
    :   This means to use a font that is n steps larger. A *step* is
        defined by the set of available fonts—specifically, those that match
        what was otherwise specified for this text, in all attributes except
        height. Each size for which a suitable font is available counts as
        another step. n should be an integer.

    `(- n)`
    :   This means to use a font that is n steps smaller.

    a number, factor
    :   A number, factor, means to use a font that is factor times
        as tall as the default font.

    a symbol, function
    :   A symbol is a function to compute the height. It is called with the
        current height as argument, and should return the new height to use.

    anything else, form
    :   If the height value doesn’t fit the previous possibilities, it is
        a form. Emacs evaluates it to get the new height, with the symbol
        `height` bound to the current specified font height.

`(raise factor)`
:   This kind of display specification raises or lowers the text
    it applies to, relative to the baseline of the line. It is mainly
    meant to support display of subscripts and superscripts.

    The factor must be a number, which is interpreted as a multiple
    of the height of the affected text. If it is positive, that means to
    display the characters raised. If it is negative, that means to
    display them lower down.

    Note that if the text also has a `height` display specification,
    which was specified before (i.e. to the left of) `raise`, the
    latter will affect the amount of raising or lowering in pixels,
    because that is based on the height of the text being raised.
    Therefore, if you want to display a sub- or superscript that is
    smaller than the normal text height, consider specifying `raise`
    before `height`.

You can make any display specification conditional. To do that,
package it in another list of the form
`(when condition . spec)`.
Then the specification spec applies only when
condition evaluates to a non-`nil` value. During the
evaluation, `object` is bound to the string or buffer having the
conditional `display` property. `position` and
`buffer-position` are bound to the position within `object`
and the buffer position where the `display` property was found,
respectively. Both positions can be different when `object` is a
string.

Note that condition will only be evaluated when redisplay
examines the text where this display spec is located, so this feature
is best suited for conditions that are relatively stable, i.e.
yield, for each particular buffer position, the same results on every
evaluation. If the results change for the same text location, e.g.,
if the result depends on the position of point, then the conditional
specification might not do what you want, because redisplay examines
only those parts of buffer text where it has reasons to assume that
something changed since the last display cycle.

---

Previous: [Other Display Specifications](#Other-Display-Specs), Up: [The `display` Property](#Display-Property)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.16.5 Displaying in the Margins [¶](#Displaying-in-the-Margins)

A buffer can have blank areas called *display margins* on the
left and on the right. Ordinary text never appears in these areas,
but you can put things into the display margins using the
`display` property.

The way to display something in the margins is to specify it in a
margin display specification in the `display` property of some
text. This is a replacing display specification, meaning that the
text you put it on does not get displayed; the margin display appears,
but that text does not.

A margin display specification looks like `((margin
right-margin) spec)` or `((margin left-margin) spec)`.
Here, spec is another display specification that says what to
display in the margin. Typically it is a string of text to display,
or an image descriptor.

To display something in the margin *in association with*
certain buffer text, without altering or preventing the display of
that text, put on that text an overlay with a `before-string`
property, and put the margin display specification on the contents of
the before-string.

Note that if the string to be displayed in the margin doesn’t fully
specify its face, the nonspecified attributes are inherited from the
`margin` face (see [Basic Faces](#Basic-Faces)). If you want a margin string
to have a specific appearance independent of the `margin` face,
make sure the string has a face specifying all required attributes.

Before the display margins can display anything, you must give
them a nonzero width. The usual way to do that is to set these
variables:

Variable: **left-margin-width** [¶](#index-left_002dmargin_002dwidth)
:   This variable specifies the width of the left margin, in character
    cell (a.k.a. “column”) units. It is buffer-local in all buffers.
    A value of `nil` means no left marginal area.

Variable: **right-margin-width** [¶](#index-right_002dmargin_002dwidth)
:   This variable specifies the width of the right margin, in character
    cell units. It is buffer-local in all buffers. A value of `nil`
    means no right marginal area.

Setting these variables does not immediately affect the window. These
variables are checked when a new buffer is displayed in the window.
Thus, you can make changes take effect by calling
`set-window-buffer`. Do not use these variables to try to
determine the current width of the left or right margin. Instead, use
the function `window-margins`.

You can also set the margin widths immediately.

Function: **set-window-margins** window left &optional right [¶](#index-set_002dwindow_002dmargins)
:   This function specifies the margin widths for window window, in
    character cell units. The argument left controls the left
    margin, and right controls the right margin (default `0`).

    If window is not large enough to accommodate margins of the
    desired width, this leaves the margins of window unchanged.

    The values specified here may be later overridden by invoking
    `set-window-buffer` (see [Buffers and Windows](#Buffers-and-Windows)) on window
    with its keep-margins argument `nil` or omitted.

Function: **window-margins** &optional window [¶](#index-window_002dmargins)
:   This function returns the width of the left and right margins of
    window as a cons cell of the form `(left . right)`. If one of the two marginal areas does not exist,
    its width is returned as `nil`; if neither of the two margins exist,
    the function returns `(nil)`. If window is `nil`, the
    selected window is used.

---

Next: [Icons](#Icons), Previous: [The `display` Property](#Display-Property), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
