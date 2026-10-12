### 30.4 Frame Parameters [¶](#Frame-Parameters-1)

A frame has many parameters that control its appearance and behavior.
Just what parameters are meaningful for a frame depends on what display
mechanism it uses. Many frame parameters exist mostly for the sake of
graphical displays and have no effect when applied to the top frame
(see [Frames](#Frames)) of a text terminal.

By default, frame parameters are saved and restored by the desktop
library functions (see [Desktop Save Mode](#Desktop-Save-Mode)) when the variable
`desktop-restore-frames` is non-`nil`. It’s the
responsibility of applications that their parameters are included in
`frameset-persistent-filter-alist` to avoid that they get
meaningless or even harmful values in restored sessions.

* [Access to Frame Parameters](#Parameter-Access)
* [Initial Frame Parameters](#Initial-Parameters)
* [Window Frame Parameters](#Window-Frame-Parameters)
* [Geometry](#Geometry)

---

Next: [Initial Frame Parameters](#Initial-Parameters), Up: [Frame Parameters](#Frame-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.4.1 Access to Frame Parameters [¶](#Access-to-Frame-Parameters)

These functions let you read and change the parameter values of a
frame.

Function: **frame-parameter** frame parameter [¶](#index-frame_002dparameter)
:   This function returns the value of the parameter parameter (a
    symbol) of frame. If frame is `nil`, it returns the
    selected frame’s parameter. If frame has no setting for
    parameter, this function returns `nil`.

Function: **frame-parameters** &optional frame [¶](#index-frame_002dparameters)
:   The function `frame-parameters` returns an alist listing all the
    parameters of frame and their values. If frame is
    `nil` or omitted, this returns the selected frame’s parameters

Function: **modify-frame-parameters** frame alist [¶](#index-modify_002dframe_002dparameters)
:   This function alters the frame frame based on the elements of
    alist. Each element of alist has the form
    `(parm . value)`, where parm is a symbol naming
    a parameter. If you don’t mention a parameter in alist, its
    value doesn’t change. If frame is `nil`, it defaults to
    the selected frame.

    Some parameters are only meaningful for frames on certain kinds of
    display (see [Frames](#Frames)). If alist includes parameters that are
    not meaningful for the frame’s display, this function will
    change its value in the frame’s parameter list, but will otherwise
    ignore it.

    When alist specifies more than one parameter whose value can
    affect the new size of frame, the final size of the frame may
    differ according to the toolkit used. For example, specifying that a
    frame should from now on have a menu and/or tool bar instead of none and
    simultaneously specifying the new height of the frame will inevitably
    lead to a recalculation of the frame’s height. Conceptually, in such
    case, this function will try to have the explicit height specification
    prevail. It cannot be excluded, however, that the addition (or removal)
    of the menu or tool bar, when eventually performed by the toolkit, will
    defeat this intention.

    Sometimes, binding `frame-inhibit-implied-resize` (see [Implied Frame Resizing](#Implied-Frame-Resizing)) to a non-`nil` value around calls to this function
    may fix the problem sketched here. Sometimes, however, exactly such
    binding may be hit by the problem.

Function: **set-frame-parameter** frame parm value [¶](#index-set_002dframe_002dparameter)
:   This function sets the frame parameter parm to the specified
    value. If frame is `nil`, it defaults to the selected
    frame.

Function: **modify-all-frames-parameters** alist [¶](#index-modify_002dall_002dframes_002dparameters)
:   This function alters the frame parameters of all existing frames
    according to alist, then modifies `default-frame-alist`
    (and, if necessary, `initial-frame-alist`) to apply the same
    parameter values to frames that will be created henceforth.

---

Next: [Window Frame Parameters](#Window-Frame-Parameters), Previous: [Access to Frame Parameters](#Parameter-Access), Up: [Frame Parameters](#Frame-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.4.2 Initial Frame Parameters [¶](#Initial-Frame-Parameters)

You can specify the parameters for the initial startup frame by
setting `initial-frame-alist` in your init file (see [The Init File](#Init-File)).

User Option: **initial-frame-alist** [¶](#index-initial_002dframe_002dalist)
:   This variable’s value is an alist of parameter values used when
    creating the initial frame. You can set this variable to specify the
    appearance of the initial frame without altering subsequent frames.
    Each element has the form:

    ```
    (parameter . value)
    ```

    Emacs creates the initial frame before it reads your init
    file. After reading that file, Emacs checks `initial-frame-alist`,
    and applies the parameter settings in the altered value to the already
    created initial frame.

    If these settings affect the frame geometry and appearance, you’ll see
    the frame appear with the wrong ones and then change to the specified
    ones. If that bothers you, you can specify the same geometry and
    appearance with X resources; those do take effect before the frame is
    created. See [X Resources](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#X-Resources) in The GNU Emacs Manual.

    X resource settings typically apply to all frames. If you want to
    specify some X resources solely for the sake of the initial frame, and
    you don’t want them to apply to subsequent frames, here’s how to achieve
    this. Specify parameters in `default-frame-alist` to override the
    X resources for subsequent frames; then, to prevent these from affecting
    the initial frame, specify the same parameters in
    `initial-frame-alist` with values that match the X resources.

If these parameters include `(minibuffer . nil)`, that indicates
that the initial frame should have no minibuffer. In this case, Emacs
creates a separate *minibuffer-only frame* as well.

User Option: **minibuffer-frame-alist** [¶](#index-minibuffer_002dframe_002dalist)
:   This variable’s value is an alist of parameter values used when
    creating an initial minibuffer-only frame (i.e., the minibuffer-only
    frame that Emacs creates if `initial-frame-alist` specifies a
    frame with no minibuffer).

User Option: **default-frame-alist** [¶](#index-default_002dframe_002dalist)
:   This is an alist specifying default values of frame parameters for all
    Emacs frames—the first frame, and subsequent frames. When using the X
    Window System, you can get the same results by means of X resources
    in many cases.

    Setting this variable does not affect existing frames. Furthermore,
    functions that display a buffer in a separate frame may override the
    default parameters by supplying their own parameters.

If you invoke Emacs with command-line options that specify frame
appearance, those options take effect by adding elements to either
`initial-frame-alist` or `default-frame-alist`. Options
which affect just the initial frame, such as ‘`--geometry`’ and
‘`--maximized`’, add to `initial-frame-alist`; the others add
to `default-frame-alist`. see [Command Line
Arguments for Emacs Invocation](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Emacs-Invocation) in The GNU Emacs Manual.

---

Next: [Geometry](#Geometry), Previous: [Initial Frame Parameters](#Initial-Parameters), Up: [Frame Parameters](#Frame-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.4.3 Window Frame Parameters [¶](#Window-Frame-Parameters-1)

Just what parameters a frame has depends on what display mechanism
it uses. This section describes the parameters that have special
meanings on some or all kinds of terminals.

* [Basic Parameters](#Basic-Parameters)
* [Position Parameters](#Position-Parameters)
* [Size Parameters](#Size-Parameters)
* [Layout Parameters](#Layout-Parameters)
* [Buffer Parameters](#Buffer-Parameters)
* [Frame Interaction Parameters](#Frame-Interaction-Parameters)
* [Mouse Dragging Parameters](#Mouse-Dragging-Parameters)
* [Window Management Parameters](#Management-Parameters)
* [Cursor Parameters](#Cursor-Parameters)
* [Font and Color Parameters](#Font-and-Color-Parameters)

---

Next: [Position Parameters](#Position-Parameters), Up: [Window Frame Parameters](#Window-Frame-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.4.3.1 Basic Parameters [¶](#Basic-Parameters-1)

These frame parameters give the most basic information about the
frame. `title` and `name` are meaningful on all terminals.

`display` [¶](#index-display_002c-a-frame-parameter)
:   The display on which to open this frame. It should be a string of the
    form ‘`host:dpy.screen`’, just like the
    `DISPLAY` environment variable. See [Multiple Terminals](#Multiple-Terminals), for
    more details about display names.

`display-type` [¶](#index-display_002dtype_002c-a-frame-parameter)
:   This parameter describes the range of possible colors that can be used
    in this frame. Its value is `color`, `grayscale` or
    `mono`.

`title` [¶](#index-title_002c-a-frame-parameter)
:   If a frame has a non-`nil` title, that title appears in the window
    system’s title bar at the top of the frame, and also in the mode line
    of windows in that frame if `mode-line-frame-identification` uses
    ‘`%F`’ (see [`%`-Constructs in the Mode Line](#g_t_0025_002dConstructs)). This is normally the case when
    Emacs is not using a window system, and can only display one frame at
    a time. When Emacs is using a window system, this parameter, if
    non-`nil`, overrides the title determined by the `name`
    parameter and the implicit title calculated according to
    `frame-title-format`. It also overrides the title determined by
    `icon-title-format` for iconified frames. See [Frame Titles](#Frame-Titles).

`name` [¶](#index-name_002c-a-frame-parameter)
:   The name of the frame. If you don’t specify a name via this
    parameter, Emacs sets the frame name automatically, as specified by
    `frame-title-format` and `icon-title-format`, and that is
    the frame’s title that will appear on display when Emacs uses a window
    system (unless the `title` parameter overrides it).

    If you specify the frame name explicitly when you create the frame, the
    name is also used (instead of the name of the Emacs executable) when
    looking up X resources for the frame.

`explicit-name` [¶](#index-explicit_002dname_002c-a-frame-parameter)
:   If the frame name was specified explicitly when the frame was created,
    this parameter will be that name. If the frame wasn’t explicitly
    named, this parameter will be `nil`.

`environment` [¶](#index-environment_002c-a-frame-parameter)
:   The list of strings, each one describing one environment variable. This
    list is similar to the one in `process-environment` (see [Operating System Environment](#System-Environment)), but it lists variables and values specific to the frame.

---

Next: [Size Parameters](#Size-Parameters), Previous: [Basic Parameters](#Basic-Parameters), Up: [Window Frame Parameters](#Window-Frame-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.4.3.2 Position Parameters [¶](#Position-Parameters-1)

Parameters describing the X- and Y-offsets of a frame are always
measured in pixels. For a normal, non-child frame they specify the
frame’s outer position (see [Frame Geometry](#Frame-Geometry)) relative to its
display’s origin. For a child frame (see [Child Frames](#Child-Frames)) they specify
the frame’s outer position relative to the native position of the
frame’s parent frame. On a text terminal these parameters are
meaningful for child frames only.

`left` [¶](#index-left_002c-a-frame-parameter)
:   The position, in pixels, of the left outer edge of the frame with
    respect to the left edge of the frame’s display or parent frame. It can
    be specified in one of the following ways.

    an integer
    :   A positive integer always relates the left edge of the frame to the left
        edge of its display or parent frame. A negative integer relates the
        right frame edge to the right edge of the display or parent frame.

    `(+ pos)`
    :   This specifies the position of the left frame edge relative to the left
        edge of its display or parent frame. The integer pos may be
        positive or negative; a negative value specifies a position outside the
        screen or parent frame or on a monitor other than the primary one (for
        multi-monitor displays).

    `(- pos)`
    :   This specifies the position of the right frame edge relative to the
        right edge of the display or parent frame. The integer pos may be
        positive or negative; a negative value specifies a position outside the
        screen or parent frame or on a monitor other than the primary one (for
        multi-monitor displays).

    a floating-point value [¶](#index-left-position-ratio)
    :   A floating-point value in the range 0.0 to 1.0 specifies the left edge’s
        offset via the *left position ratio* of the frame—the ratio of the
        left edge of its outer frame to the width of the frame’s workarea
        (see [Multiple Terminals](#Multiple-Terminals)) or its parent’s native frame (see [Child Frames](#Child-Frames)) minus the width of the outer frame. Thus, a left position
        ratio of 0.0 flushes a frame to the left, a ratio of 0.5 centers it and
        a ratio of 1.0 flushes it to the right of its display or parent frame.
        Similarly, the *top position ratio* of a frame is the ratio of the
        frame’s top position to the height of its workarea or parent frame minus
        the height of the frame.

        Emacs will try to keep the position ratios of a child frame unaltered if
        that frame has a non-`nil` `keep-ratio` parameter
        (see [Frame Interaction Parameters](#Frame-Interaction-Parameters)) and its parent frame is resized.

        Since the outer size of a frame (see [Frame Geometry](#Frame-Geometry)) is usually
        unavailable before a frame has been made visible, it is generally not
        advisable to use floating-point values when creating decorated frames.
        Floating-point values are more suited for ensuring that an (undecorated)
        child frame is positioned nicely within the area of its parent frame.

        Floating-point values are currently not handled on text terminal frames.

    Some window managers ignore program-specified positions. If you want to
    be sure the position you specify is not ignored, specify a
    non-`nil` value for the `user-position` parameter as in the
    following example:

    ```
    (modify-frame-parameters
      nil '((user-position . t) (left . (+ -4))))
    ```

    In general, it is not a good idea to position a frame relative to the
    right or bottom edge of its display. Positioning the initial or a new
    frame is either not accurate (because the size of the outer frame is not
    yet fully known before the frame has been made visible) or will cause
    additional flicker (if the frame has to be repositioned after becoming
    visible).

    Note also, that positions specified relative to the right/bottom edge
    of a display, workarea or parent frame as well as floating-point offsets
    are stored internally as integer offsets relative to the left/top edge
    of the display, workarea or parent frame edge. They are also returned
    as such by functions like `frame-parameters` and restored as such
    by the desktop saving routines.

`top` [¶](#index-top_002c-a-frame-parameter)
:   The screen position of the top (or bottom) edge, in pixels, with respect
    to the top (or bottom) edge of the display or parent frame. It works
    just like `left`, except vertically instead of horizontally.

`icon-left` [¶](#index-icon_002dleft_002c-a-frame-parameter)
:   The screen position of the left edge of the frame’s icon, in pixels,
    counting from the left edge of the screen. This takes effect when the
    frame is iconified, if the window manager supports this feature. If you
    specify a value for this parameter, then you must also specify a value
    for `icon-top` and vice versa. This parameter has no meaning on a
    text terminal.

`icon-top` [¶](#index-icon_002dtop_002c-a-frame-parameter)
:   The screen position of the top edge of the frame’s icon, in pixels,
    counting from the top edge of the screen. This takes effect when the
    frame is iconified, if the window manager supports this feature. This
    parameter has no meaning on a text terminal.

`user-position` [¶](#index-user_002dposition_002c-a-frame-parameter)
:   When you create a frame and specify its screen position with the
    `left` and `top` parameters, use this parameter to say whether
    the specified position was user-specified (explicitly requested in some
    way by a human user) or merely program-specified (chosen by a program).
    A non-`nil` value says the position was user-specified. This
    parameter has no meaning on a text terminal.

    Window managers generally heed user-specified positions, and some heed
    program-specified positions too. But many ignore program-specified
    positions, placing the window in a default fashion or letting the user
    place it with the mouse. Some window managers, including `twm`,
    let the user specify whether to obey program-specified positions or
    ignore them.

    When you call `make-frame`, you should specify a non-`nil`
    value for this parameter if the values of the `left` and `top`
    parameters represent the user’s stated preference; otherwise, use
    `nil`.

`z-group` [¶](#index-z_002dgroup_002c-a-frame-parameter)
:   This parameter specifies a relative position of the frame’s
    window-system window in the stacking (Z-) order of the frame’s display.
    It has not been implemented yet on text terminals.

    If this is `above`, the window-system will display the window
    that corresponds to the frame above all other window-system windows
    that do not have the `above` property set. If this is
    `nil`, the frame’s window is displayed below all windows that
    have the `above` property set and above all windows that have the
    `below` property set. If this is `below`, the frame’s
    window is displayed below all windows that do not have the
    `below` property set.

    To position the frame above or below a specific other frame use the
    function `frame-restack` (see [Raising, Lowering and Restacking Frames](#Raising-and-Lowering)).

---

Next: [Layout Parameters](#Layout-Parameters), Previous: [Position Parameters](#Position-Parameters), Up: [Window Frame Parameters](#Window-Frame-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.4.3.3 Size Parameters [¶](#Size-Parameters-1)

Frame parameters usually specify frame sizes in character units. On
graphical displays, the `default` face determines the actual pixel
sizes of these character units (see [Face Attributes](#Face-Attributes)). On text
terminals size parameters affect child frames only.

`width` [¶](#index-width_002c-a-frame-parameter)
:   This parameter specifies the width of the frame. It can be specified as
    in the following ways:

    an integer
    :   A positive integer specifies the width of the frame’s text area
        (see [Frame Geometry](#Frame-Geometry)) in characters.

    a cons cell
    :   If this is a cons cell with the symbol `text-pixels` in its
        CAR, the CDR of that cell specifies the width of the frame’s
        text area in pixels.

    a floating-point value [¶](#index-frame-width-ratio)
    :   A floating-point number between 0.0 and 1.0 can be used to specify the
        width of a frame via its *width ratio*—the ratio of its outer
        width (see [Frame Geometry](#Frame-Geometry)) to the width of the frame’s workarea
        (see [Multiple Terminals](#Multiple-Terminals)) or its parent frame’s (see [Child Frames](#Child-Frames)) native frame. Thus, a value of 0.5 makes the frame occupy half
        of the width of its workarea or parent frame, a value of 1.0 the full
        width. Similarly, the *height ratio* of a frame is the ratio of its
        outer height to the height of its workarea or its parent’s native frame.

        Emacs will try to keep the width and height ratio of a child frame
        unaltered if that frame has a non-`nil` `keep-ratio` parameter
        (see [Frame Interaction Parameters](#Frame-Interaction-Parameters)) and its parent frame is resized.

        Since the outer size of a frame is usually unavailable before a frame
        has been made visible, it is generally not advisable to use
        floating-point values when creating decorated frames. Floating-point
        values are more suited to ensure that a child frame always fits within
        the area of its parent frame as, for example, when customizing
        `display-buffer-alist` (see [Choosing a Window for Displaying a Buffer](#Choosing-Window)) via
        `display-buffer-in-child-frame`.

    Regardless of how this parameter was specified, functions reporting the
    value of this parameter like `frame-parameters` always report the
    width of the frame’s text area in characters as an integer rounded, if
    necessary, to a multiple of the frame’s default character width. That
    value is also used by the desktop saving routines.

`height` [¶](#index-height_002c-a-frame-parameter)
:   This parameter specifies the height of the frame. It works just like
    `width`, except vertically instead of horizontally.

`user-size` [¶](#index-user_002dsize_002c-a-frame-parameter)
:   This does for the size parameters `height` and `width` what
    the `user-position` parameter (see [user-position](#Position-Parameters)) does for the position parameters `top` and
    `left`. This parameter has no meaning on a text terminal.

`min-width` [¶](#index-min_002dwidth_002c-a-frame-parameter)
:   This parameter specifies the minimum native width (see [Frame Geometry](#Frame-Geometry)) of the frame, in characters. Normally, the functions that
    establish a frame’s initial width or resize a frame horizontally make
    sure that all the frame’s windows, vertical scroll bars, fringes,
    margins and vertical dividers can be displayed. This parameter, if
    non-`nil` enables making a frame narrower than that with the
    consequence that any components that do not fit will be clipped by the
    window manager.

`min-height` [¶](#index-min_002dheight_002c-a-frame-parameter)
:   This parameter specifies the minimum native height (see [Frame Geometry](#Frame-Geometry)) of the frame, in characters. Normally, the functions that
    establish a frame’s initial size or resize a frame make sure that all
    the frame’s windows, horizontal scroll bars and dividers, mode and
    header lines, the echo area and the internal menu and tool bar can be
    displayed. This parameter, if non-`nil` enables making a frame
    smaller than that with the consequence that any components that do not
    fit will be clipped by the window manager.

`fullscreen` [¶](#index-fullboth-frames)
:   This parameter specifies whether to maximize the frame’s width, height
    or both. It has no meaning on a text terminal. Its value can be
    `fullwidth`, `fullheight`, `fullboth`, or
    `maximized`.[24](#FOOT24) A
    *fullwidth* frame is as wide as possible, a *fullheight* frame
    is as tall as possible, and a *fullboth* frame is both as wide and
    as tall as possible. A *maximized* frame is like a “fullboth”
    frame, except that it usually keeps its title bar and the buttons for
    resizing and closing the frame. Also, maximized frames typically avoid
    hiding any task bar or panels displayed on the desktop. A “fullboth”
    frame, on the other hand, usually omits the title bar and occupies the
    entire available screen space.

    Full-height and full-width frames are more similar to maximized
    frames in this regard. However, these typically display an external
    border which might be absent with maximized frames. Hence the heights
    of maximized and full-height frames and the widths of maximized and
    full-width frames often differ by a few pixels.

    With some window managers you may have to customize the variable
    `frame-resize-pixelwise` (see [Frame Size](#Frame-Size)) in order to make a
    frame truly appear maximized or full-screen. Moreover, some window
    managers might not support smooth transition between the various
    full-screen or maximization states. Customizing the variable
    `x-frame-normalize-before-maximize` can help to overcome that.

    Full-screen on macOS hides both the tool-bar and the menu-bar, however
    both will be displayed if the mouse pointer is moved to the top of the
    screen.

`fullscreen-restore` [¶](#index-fullscreen_002drestore_002c-a-frame-parameter)
:   This parameter specifies the desired fullscreen state of the frame
    after invoking the `toggle-frame-fullscreen` command (see [Frame
    Commands](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Frame-Commands) in The GNU Emacs Manual) in the “fullboth” state.
    Normally this parameter is installed automatically by that command when
    toggling the state to fullboth. If, however, you start Emacs in the
    “fullboth” state, you have to specify the desired behavior in your initial
    file as, for example

    ```
    (setq default-frame-alist
        '((fullscreen . fullboth)
          (fullscreen-restore . fullheight)))
    ```

    This will give a new frame full height after typing in it `F11` for
    the first time. This parameter has no meaning on a text terminal.

`fit-frame-to-buffer-margins` [¶](#index-fit_002dframe_002dto_002dbuffer_002dmargins_002c-a-frame-parameter)
:   This parameter enables overriding the value of the option
    `fit-frame-to-buffer-margins` when fitting this frame to the
    buffer of its root window with `fit-frame-to-buffer`
    (see [Resizing Windows](#Resizing-Windows)).

`fit-frame-to-buffer-sizes` [¶](#index-fit_002dframe_002dto_002dbuffer_002dsizes_002c-a-frame-parameter)
:   This parameter enables overriding the value of the option
    `fit-frame-to-buffer-sizes` when fitting this frame to the buffer
    of its root window with `fit-frame-to-buffer` (see [Resizing Windows](#Resizing-Windows)).

---

Next: [Buffer Parameters](#Buffer-Parameters), Previous: [Size Parameters](#Size-Parameters), Up: [Window Frame Parameters](#Window-Frame-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.4.3.4 Layout Parameters [¶](#Layout-Parameters-1)

These frame parameters enable or disable various parts of the frame,
or control their sizes. Unless stated otherwise, these parameters have
no meaning on text terminals.

`undecorated` [¶](#index-undecorated_002c-a-frame-parameter)
:   If non-`nil`, then on a graphical system this frame’s window-system
    window is drawn without decorations, like the title, minimize/maximize
    boxes and external borders. This usually means that the window cannot
    be dragged, resized, iconified, maximized or deleted with the mouse. If
    `nil`, the frame’s window is usually drawn with all the elements
    listed above unless their display has been suspended via window manager
    settings.

    Under X, Emacs uses the Motif window manager hints to turn off
    decorations. Some window managers may not honor these hints.

    NS builds consider the tool bar to be a decoration, and therefore hide
    it on an undecorated frame.

    On a text terminal, this parameter, if non-`nil`, will make a child
    frame show an outer border, which allows to resize that frame via mouse
    dragging (see [Mouse Dragging Parameters](#Mouse-Dragging-Parameters)).

`border-width` [¶](#index-border_002dwidth_002c-a-frame-parameter)
:   The width in pixels of the frame’s outer border (see [Frame Geometry](#Frame-Geometry)).

`internal-border-width` [¶](#index-internal_002dborder_002dwidth_002c-a-frame-parameter)
:   The width in pixels of the frame’s internal border (see [Frame Geometry](#Frame-Geometry)).

`child-frame-border-width` [¶](#index-child_002dframe_002dborder_002dwidth_002c-a-frame-parameter)
:   The width in pixels of the frame’s internal border (see [Frame Geometry](#Frame-Geometry)) if the given frame is a child frame (see [Child Frames](#Child-Frames)).
    If this is `nil`, the value specified by the
    `internal-border-width` parameter is used instead.

`vertical-scroll-bars` [¶](#index-vertical_002dscroll_002dbars_002c-a-frame-parameter)
:   Whether the frame has scroll bars (see [Scroll Bars](#Scroll-Bars)) for vertical
    scrolling, and which side of the frame they should be on. The possible
    values are `left`, `right`, and `nil` for no scroll bars.

`horizontal-scroll-bars` [¶](#index-horizontal_002dscroll_002dbars_002c-a-frame-parameter)
:   Whether the frame has scroll bars for horizontal scrolling (`t` and
    `bottom` mean yes, `nil` means no).

`scroll-bar-width` [¶](#index-scroll_002dbar_002dwidth_002c-a-frame-parameter)
:   The width of vertical scroll bars, in pixels, or `nil` meaning to
    use the default width.

`scroll-bar-height` [¶](#index-scroll_002dbar_002dheight_002c-a-frame-parameter)
:   The height of horizontal scroll bars, in pixels, or `nil` meaning
    to use the default height.

`left-fringe` [¶](#index-left_002dfringe_002c-a-frame-parameter)

`right-fringe`
:   The default width of the left and right fringes of windows in this
    frame (see [Fringes](#Fringes)). If either of these is zero, that effectively
    removes the corresponding fringe.

    When you use `frame-parameter` to query the value of either of
    these two frame parameters, the return value is always an integer.
    When using `set-frame-parameter`, passing a `nil` value
    imposes an actual default value of 8 pixels.

`right-divider-width` [¶](#index-right_002ddivider_002dwidth_002c-a-frame-parameter)
:   The width (thickness) reserved for the right divider (see [Window Dividers](#Window-Dividers)) of any window on the frame, in pixels. A value of zero means
    to not draw right dividers.

`bottom-divider-width` [¶](#index-bottom_002ddivider_002dwidth_002c-a-frame-parameter)
:   The width (thickness) reserved for the bottom divider (see [Window Dividers](#Window-Dividers)) of any window on the frame, in pixels. A value of zero means
    to not draw bottom dividers.

`menu-bar-lines` [¶](#index-menu_002dbar_002dlines_002c-a-frame-parameter)
:   The number of lines to allocate at the top of the frame for a menu bar
    (see [The Menu Bar](#Menu-Bar)). The default is 1 if Menu Bar mode is enabled and 0
    otherwise. See [Menu Bars](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Menu-Bars) in The GNU Emacs Manual. For an
    external menu bar (see [Frame Layout](#Frame-Layout)), this value remains unchanged
    even when the menu bar wraps to two or more lines. In that case, the
    `menu-bar-size` value returned by `frame-geometry`
    (see [Frame Geometry](#Frame-Geometry)) can be used to establish whether the menu bar
    actually occupies one or more lines. This parameter affects the
    presence of a menu bar on the root frame (see [Child Frames](#Child-Frames)) of a
    text terminal too. On a text terminal the value may be only 0 or 1.

`tool-bar-lines` [¶](#index-tool_002dbar_002dlines_002c-a-frame-parameter)
:   The number of lines to use for the tool bar (see [Tool bars](#Tool-Bar)). The
    default is one if Tool Bar mode is enabled and zero otherwise.
    See [Tool Bars](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Tool-Bars) in The GNU Emacs Manual. This value may change
    whenever the tool bar wraps (see [Frame Layout](#Frame-Layout)).

`tool-bar-position` [¶](#index-tool_002dbar_002dposition_002c-a-frame-parameter)
:   The position of the tool bar. Its value can be one of `top`,
    `bottom` `left`, `right`. The default is `top`.

    It can be set to `bottom` on Emacs built with any toolkit other
    than Nextstep, and `left` or `right` on builds using GTK+.

`tab-bar-lines` [¶](#index-tab_002dbar_002dlines_002c-a-frame-parameter)
:   The number of lines to use for the tab bar (see [Tab Bars](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Tab-Bars) in The
    GNU Emacs Manual). The default is one if Tab Bar mode is enabled and
    zero otherwise. This value may change whenever the tab bar wraps
    (see [Frame Layout](#Frame-Layout)). This parameter affects the presence of a tab
    bar on the root frame (see [Child Frames](#Child-Frames)) of a text terminal too.

`line-spacing` [¶](#index-line_002dspacing_002c-a-frame-parameter)
:   Additional space to leave below each text line, in pixels (a positive
    integer). See [Line Height](#Line-Height), for more information.

`no-special-glyphs` [¶](#index-no_002dspecial_002dglyphs_002c-a-frame-parameter)
:   If this is non-`nil`, it suppresses the display of any truncation
    (see [Truncation](#Truncation)) and continuation glyphs for all the buffers
    displayed by this frame. This is useful to eliminate such glyphs when
    fitting a frame to its buffer via `fit-frame-to-buffer`
    (see [Resizing Windows](#Resizing-Windows)). This frame parameter has effect only for
    GUI frames shown on graphical displays, and only if the fringes are
    disabled. This parameter is intended as a purely-presentation feature,
    and in particular should not be used for frames where the user can
    interactively insert text, or more generally where the cursor is shown.
    A notable example of frames where this is used is tooltip frames
    (see [Tooltips](#Tooltips)). This parameter affects text terminals as well.

---

Next: [Frame Interaction Parameters](#Frame-Interaction-Parameters), Previous: [Layout Parameters](#Layout-Parameters), Up: [Window Frame Parameters](#Window-Frame-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.4.3.5 Buffer Parameters [¶](#Buffer-Parameters-1)

These frame parameters, meaningful on all kinds of terminals, deal
with which buffers have been, or should, be displayed in the frame.

`minibuffer` [¶](#index-minibuffer_002c-a-frame-parameter)
:   Whether this frame has its own minibuffer. The value `t` means
    yes, `nil` means no, `only` means this frame is just a
    minibuffer. If the value is a minibuffer window (in some other
    frame), the frame uses that minibuffer.

    This parameter takes effect when the frame is created. If specified as
    `nil`, Emacs will try to set it to the minibuffer window of
    `default-minibuffer-frame` (see [Minibuffers and Frames](#Minibuffers-and-Frames)). For
    an existing frame, this parameter can be used exclusively to specify
    another minibuffer window. It is not allowed to change it from a
    minibuffer window to `t` and vice-versa, or from `t` to
    `nil`. If the parameter specifies a minibuffer window already,
    setting it to `nil` has no effect.

    The special value `child-frame` means to make a minibuffer-only
    child frame (see [Child Frames](#Child-Frames)) whose parent becomes the frame
    created. As if specified as `nil`, Emacs will set this parameter
    to the minibuffer window of the child frame but will not select the
    child frame after its creation. The value `child-frame` has no
    effect on text terminals where you have to create a minibuffer-only
    frame manually (see [Child Frame Peculiarities](#Child-Frame-Peculiarities)).

`buffer-predicate` [¶](#index-buffer_002dpredicate_002c-a-frame-parameter)
:   The buffer-predicate function for this frame. The function
    `other-buffer` uses this predicate (from the selected frame) to
    decide which buffers it should consider, if the predicate is not
    `nil`. It calls the predicate with one argument, a buffer, once for
    each buffer; if the predicate returns a non-`nil` value, it
    considers that buffer.

`buffer-list` [¶](#index-buffer_002dlist_002c-a-frame-parameter)
:   A list of buffers that have been selected in this frame, ordered
    most-recently-selected first.

`unsplittable` [¶](#index-unsplittable_002c-a-frame-parameter)
:   If non-`nil`, this frame’s window is never split automatically.

---

Next: [Mouse Dragging Parameters](#Mouse-Dragging-Parameters), Previous: [Buffer Parameters](#Buffer-Parameters), Up: [Window Frame Parameters](#Window-Frame-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.4.3.6 Frame Interaction Parameters [¶](#Frame-Interaction-Parameters-1)

These parameters supply forms of interactions between different frames.

`visibility` [¶](#index-visibility_002c-a-frame-parameter)
:   The state of visibility of the frame. There are three possibilities:
    `nil` for invisible, `t` for visible, and `icon` for
    iconified. See [Visibility of Frames](#Visibility-of-Frames).

`parent-frame` [¶](#index-parent_002dframe_002c-a-frame-parameter)
:   If non-`nil`, this means that this frame is a child frame
    (see [Child Frames](#Child-Frames)), and this parameter specifies its parent frame.
    If `nil`, this means that this frame is a normal, top-level frame.

`delete-before` [¶](#index-delete_002dbefore_002c-a-frame-parameter)
:   If non-`nil`, this parameter specifies another frame whose deletion
    will automatically trigger the deletion of this frame. See [Deleting Frames](#Deleting-Frames).

`mouse-wheel-frame` [¶](#index-mouse_002dwheel_002dframe_002c-a-frame-parameter)
:   If non-`nil`, this parameter specifies the frame whose windows will
    be scrolled whenever the mouse wheel is scrolled with the mouse pointer
    hovering over this frame, see [Mouse Commands](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Mouse-Commands) in The GNU
    Emacs Manual. This parameter has no meaning on a text terminal.

`no-other-frame` [¶](#index-no_002dother_002dframe_002c-a-frame-parameter)
:   If this is non-`nil`, then this frame is not eligible as candidate
    for the functions `next-frame`, `previous-frame`
    (see [Finding All Frames](#Finding-All-Frames)) and `other-frame`, see [Frame
    Commands](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Frame-Commands) in The GNU Emacs Manual.

`auto-hide-function` [¶](#index-auto_002dhide_002dfunction_002c-a-frame-parameter)
:   When this parameter specifies a function, that function will be called
    instead of the function specified by the variable
    `frame-auto-hide-function` when quitting the frame’s only window
    (see [Quitting Windows](#Quitting-Windows)) and there are other frames left. This
    parameter has not been yet implemented on text terminals.

`minibuffer-exit` [¶](#index-minibuffer_002dexit_002c-a-frame-parameter)
:   When this parameter is non-`nil`, Emacs will by default make this
    frame invisible whenever the minibuffer (see [Minibuffers](#Minibuffers)) is exited.
    Alternatively, it can specify the functions `iconify-frame` and
    `delete-frame`. This parameter is useful to make a child frame
    disappear automatically (similar to how Emacs deals with a window) when
    exiting the minibuffer. This parameter has not been yet implemented on
    text terminals.

`keep-ratio` [¶](#index-keep_002dratio_002c-a-frame-parameter)
:   This parameter is currently meaningful for child frames (see [Child Frames](#Child-Frames)) only. If it is non-`nil`, then Emacs will try to keep the
    frame’s size (width and height) ratios (see [Size Parameters](#Size-Parameters)) as well
    as its left and right position ratios (see [Position Parameters](#Position-Parameters))
    unaltered whenever its parent frame is resized.

    If the value of this parameter is `nil`, the frame’s position and
    size remain unaltered when the parent frame is resized, so the position
    and size ratios may change. If the value of this parameter is `t`,
    Emacs will try to preserve the frame’s size and position ratios, hence
    the frame’s size and position relative to its parent frame may change.

    More individual control is possible by using a cons cell: In that case
    the frame’s width ratio is preserved if the CAR of the cell is
    either `t` or `width-only`. The height ratio is preserved if
    the CAR of the cell is either `t` or `height-only`. The
    left position ratio is preserved if the CDR of the cell is either
    `t` or `left-only`. The top position ratio is preserved if
    the CDR of the cell is either `t` or `top-only`. This
    parameter has not been yet implemented on text terminals.

`cloned-from` [¶](#index-cloned_002dfrom_002c-a-frame-parameter)
:   The original frame if this frame was made via `clone-frame`
    (see [Creating Frames](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Creating-Frames) in the Emacs Manual).

`undeleted` [¶](#index-undeleted_002c-a-frame-parameter)
:   This is non-`nil` if this frame was undeleted using the command
    `undelete-frame` (see [Frame Commands](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Frame-Commands) in the Emacs
    Manual).

---

Next: [Window Management Parameters](#Management-Parameters), Previous: [Frame Interaction Parameters](#Frame-Interaction-Parameters), Up: [Window Frame Parameters](#Window-Frame-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.4.3.7 Mouse Dragging Parameters [¶](#Mouse-Dragging-Parameters-1)

The parameters described below provide support for resizing a frame by
dragging its internal borders with the mouse. They also allow moving a
frame with the mouse by dragging the header or tab line of its topmost
or the mode line of its bottommost window.

These parameters are mostly useful for child frames (see [Child Frames](#Child-Frames)) that come without window manager decorations. If necessary,
they can be used for undecorated top-level frames as well. On text
terminals these parameters affect child frames only.

`drag-internal-border` [¶](#index-drag_002dinternal_002dborder_002c-a-frame-parameter)
:   If non-`nil`, the frame can be resized by dragging its internal
    borders, if present, with the mouse. On text terminals, the decoration
    of a child frame must be dragged instead.

`drag-with-header-line` [¶](#index-drag_002dwith_002dheader_002dline_002c-a-frame-parameter)
:   If non-`nil`, the frame can be moved with the mouse by dragging the
    header line of its topmost window.

`drag-with-tab-line` [¶](#index-drag_002dwith_002dtab_002dline_002c-a-frame-parameter)
:   If non-`nil`, the frame can be moved with the mouse by dragging the
    tab line of its topmost window.

`drag-with-mode-line` [¶](#index-drag_002dwith_002dmode_002dline_002c-a-frame-parameter)
:   If non-`nil`, the frame can be moved with the mouse by dragging the
    mode line of its bottommost window. Note that such a frame is not
    allowed to have its own minibuffer window.

`snap-width` [¶](#index-snap_002dwidth_002c-a-frame-parameter)
:   A frame that is moved with the mouse will “snap” at the border(s) of
    the display or its parent frame whenever it is dragged as near to such
    an edge as the number of pixels specified by this parameter.

`top-visible` [¶](#index-top_002dvisible_002c-a-frame-parameter)
:   If this parameter is a number, the top edge of the frame never appears
    above the top edge of its display or parent frame. Moreover, as many
    pixels of the frame as specified by that number will remain visible when
    the frame is moved against any of the remaining edges of its display or
    parent frame. Setting this parameter is useful to guard against
    dragging a child frame with a non-`nil`
    `drag-with-header-line` parameter completely out of the area
    of its parent frame.

`bottom-visible` [¶](#index-bottom_002dvisible_002c-a-frame-parameter)
:   If this parameter is a number, the bottom edge of the frame never
    appears below the bottom edge of its display or parent frame. Moreover,
    as many pixels of the frame as specified by that number will remain
    visible when the frame is moved against any of the remaining edges of
    its display or parent frame. Setting this parameter is useful to guard
    against dragging a child frame with a non-`nil`
    `drag-with-mode-line` parameter completely out of the area of
    its parent frame.

---

Next: [Cursor Parameters](#Cursor-Parameters), Previous: [Mouse Dragging Parameters](#Mouse-Dragging-Parameters), Up: [Window Frame Parameters](#Window-Frame-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.4.3.8 Window Management Parameters [¶](#Window-Management-Parameters)

The following frame parameters control various aspects of the frame’s
interaction with the window manager or window system. They have no
effect on text terminals.

`auto-raise` [¶](#index-auto_002draise_002c-a-frame-parameter)
:   If non-`nil`, Emacs automatically raises the frame when it is
    selected. Some window managers do not allow this.

`auto-lower` [¶](#index-auto_002dlower_002c-a-frame-parameter)
:   If non-`nil`, Emacs automatically lowers the frame when it is
    deselected. Some window managers do not allow this.

`icon-type` [¶](#index-icon_002dtype_002c-a-frame-parameter)
:   The type of icon to use for this frame. If the value is a string,
    that specifies a file containing a bitmap to use; `nil` specifies
    no icon (in which case the window manager decides what to show); any
    other non-`nil` value specifies the default Emacs icon.

`icon-name` [¶](#index-icon_002dname_002c-a-frame-parameter)
:   The name to use in the icon for this frame, when and if the icon
    appears. If this is `nil`, the frame’s title is used.

`window-id` [¶](#index-window_002did_002c-a-frame-parameter)
:   The ID number which the graphical display uses for this frame. Emacs
    assigns this parameter when the frame is created; changing the
    parameter has no effect on the actual ID number.

`outer-window-id` [¶](#index-outer_002dwindow_002did_002c-a-frame-parameter)
:   The ID number of the outermost window-system window in which the frame
    exists. As with `window-id`, changing this parameter has no
    actual effect.

`wait-for-wm` [¶](#index-wait_002dfor_002dwm_002c-a-frame-parameter)
:   If non-`nil`, tell Xt to wait for the window manager to confirm
    geometry changes. Some window managers, including versions of Fvwm2
    and KDE, fail to confirm, so Xt hangs. Set this to `nil` to
    prevent hanging with those window managers.

`sticky` [¶](#index-sticky_002c-a-frame-parameter)
:   If non-`nil`, the frame is visible on all virtual desktops on systems
    with virtual desktops.

`shaded` [¶](#index-shaded_002c-a-frame-parameter)
:   If non-`nil`, tell the window manager to display the frame in a
    way that its contents are hidden, leaving only the title bar.

`use-frame-synchronization` [¶](#index-use_002dframe_002dsynchronization_002c-a-frame-parameter)
:   If non-`nil`, synchronize the frame redisplay with the refresh
    rate of the monitor to avoid graphics tearing. At present, this is
    only implemented on Haiku and the X window system inside no-toolkit
    and X toolkit builds, does not work correctly with toolkit scroll
    bars, and requires a compositing manager supporting the relevant
    display synchronization protocols. The `synchronizeResize` X
    resource must also be set to the string `"extended"`.

`inhibit-double-buffering` [¶](#index-inhibit_002ddouble_002dbuffering_002c-a-frame-parameter)
:   If non-`nil`, the frame is drawn to the screen without double
    buffering. Emacs normally attempts to use double buffering, where
    available, to reduce flicker; nevertheless, this parameter is provided
    for circumstances where double-buffering induces display corruption,
    and for those eccentrics wistful for the immemorial flicker that once
    beset Emacs.

`skip-taskbar` [¶](#index-skip_002dtaskbar_002c-a-frame-parameter)
:   If non-`nil`, this tells the window manager to remove the frame’s
    icon from the taskbar associated with the frame’s display and inhibit
    switching to the frame’s window via the combination `Alt-TAB`.
    On MS-Windows, iconifying such a frame will "roll in" its window-system
    window at the bottom of the desktop. Some window managers may not honor
    this parameter.

`no-focus-on-map` [¶](#index-no_002dfocus_002don_002dmap_002c-a-frame-parameter)
:   If non-`nil`, this means that the frame does not want to receive
    input focus when it is mapped (see [Visibility of Frames](#Visibility-of-Frames)). Some
    window managers may not honor this parameter.

`no-accept-focus` [¶](#index-no_002daccept_002dfocus_002c-a-frame-parameter)
:   If non-`nil`, this means that the frame does not want to receive
    input focus via explicit mouse clicks or when moving the mouse into it
    either via `focus-follows-mouse` (see [Input Focus](#Input-Focus)) or
    `mouse-autoselect-window` (see [Mouse Window Auto-selection](#Mouse-Window-Auto_002dselection)).
    This may have the unwanted side-effect that a user cannot scroll a
    non-selected frame with the mouse. Some window managers may not honor
    this parameter. On Haiku, it also has the side-effect that the window
    will not be able to receive any keyboard input from the user, not even
    if the user switches to the frame using the key combination
    `Alt-TAB`.

`override-redirect` [¶](#index-override-redirect-frames)
:   If non-`nil`, this means that this is an *override redirect*
    frame—a frame not handled by window managers under X. Override
    redirect frames have no window manager decorations, can be positioned
    and resized only via Emacs’s positioning and resizing functions and are
    usually drawn on top of all other frames. Setting this parameter has
    no effect on MS-Windows.

`ns-appearance` [¶](#index-ns_002dappearance_002c-a-frame-parameter)
:   Only available on macOS, if set to `dark` draw this frame’s
    window-system window using the “vibrant dark” theme, and if set to
    `light` use the “aqua” theme, otherwise use the system default.
    The “vibrant dark” theme can be used to set the toolbar and
    scrollbars to a dark appearance when using an Emacs theme with a dark
    background.

`ns-transparent-titlebar` [¶](#index-ns_002dtransparent_002dtitlebar_002c-a-frame-parameter)
:   Only available on macOS, if non-`nil`, set the titlebar and
    toolbar to be transparent. This effectively sets the background color
    of both to match the Emacs background color.

---

Next: [Font and Color Parameters](#Font-and-Color-Parameters), Previous: [Window Management Parameters](#Management-Parameters), Up: [Window Frame Parameters](#Window-Frame-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.4.3.9 Cursor Parameters [¶](#Cursor-Parameters-1)

This frame parameter controls the way the cursor looks.

`cursor-type` [¶](#index-cursor_002dtype_002c-a-frame-parameter)
:   How to display the cursor. Legitimate values are:

    `box`
    :   Display a filled box. (This is the default.)

    `(box . size)`
    :   Display a filled box. However, display it as a hollow box if point is
        under masked image larger than size pixels in either dimension.

    `hollow`
    :   Display a hollow box.

    `nil`
    :   Don’t display a cursor.

    `bar`
    :   Display a vertical bar between characters.

    `(bar . width)`
    :   Display a vertical bar width pixels wide between characters.

    `hbar`
    :   Display a horizontal bar.

    `(hbar . height)`
    :   Display a horizontal bar height pixels high.

The `cursor-type` frame parameter may be overridden by
`set-window-cursor-type` (see [Windows and Point](#Window-Point)), and by the
variables `cursor-type` and `cursor-in-non-selected-windows`:

User Option: **cursor-type** [¶](#index-cursor_002dtype-1)
:   This buffer-local variable controls how the cursor looks in a selected
    window showing the buffer. If its value is `t`, that means to
    use the cursor specified by the `cursor-type` frame parameter.
    Otherwise, the value should be one of the cursor types listed above,
    and it overrides the `cursor-type` frame parameter.

User Option: **cursor-in-non-selected-windows** [¶](#index-cursor_002din_002dnon_002dselected_002dwindows)
:   This buffer-local variable controls how the cursor looks in a window
    that is not selected. It supports the same values as the
    `cursor-type` frame parameter; also, `nil` means don’t
    display a cursor in nonselected windows, and `t` (the default)
    means use a standard modification of the usual cursor type (solid box
    becomes hollow box, and bar becomes a narrower bar).

User Option: **x-stretch-cursor** [¶](#index-x_002dstretch_002dcursor)
:   This variable controls the width of the block cursor displayed on
    extra-wide glyphs such as a tab or a stretch of white space. By
    default, the block cursor is only as wide as the font’s default
    character, and will not cover all of the width of the glyph under it
    if that glyph is extra-wide. A non-`nil` value of this variable
    means draw the block cursor as wide as the glyph under it. The
    default value is `nil`.

    This variable has no effect on text-mode frames, since the text-mode
    cursor is drawn by the terminal out of Emacs’s control.

User Option: **blink-cursor-alist** [¶](#index-blink_002dcursor_002dalist)
:   This variable specifies how to blink the cursor. Each element has the
    form `(on-state . off-state)`. Whenever the cursor
    type equals on-state (comparing using `equal`), the
    corresponding off-state specifies what the cursor looks like
    when it blinks off. Both on-state and off-state
    should be suitable values for the `cursor-type` frame parameter.

    There are various defaults for how to blink each type of cursor, if
    the type is not mentioned as an on-state here. Changes in this
    variable do not take effect immediately, only when you specify the
    `cursor-type` frame parameter.

---

Previous: [Cursor Parameters](#Cursor-Parameters), Up: [Window Frame Parameters](#Window-Frame-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.4.3.10 Font and Color Parameters [¶](#Font-and-Color-Parameters-1)

These frame parameters control the use of fonts and colors.

`font-backend` [¶](#index-font_002dbackend_002c-a-frame-parameter)
:   A list of symbols, specifying the *font backends* to use for
    drawing characters on the frame, in order of priority. In Emacs built
    without Cairo drawing on X, there are currently three potentially
    available font backends: `x` (the X core font driver), `xft`
    (the Xft font driver), and `xfthb` (the Xft font driver with
    HarfBuzz text shaping). If built with Cairo drawing, there are also
    three potentially available font backends on X: `x`, `ftcr`
    (the FreeType font driver on Cairo), and `ftcrhb` (the FreeType
    font driver on Cairo with HarfBuzz text shaping). When Emacs is built
    with HarfBuzz, the default font driver is `ftcrhb`, although use
    of the `ftcr` driver is still possible, but not recommended. On
    MS-Windows, there are currently three available font backends:
    `gdi` (the core MS-Windows font driver), `uniscribe` (font
    driver for OTF and TTF fonts with text shaping by the Uniscribe
    engine), and `harfbuzz` (font driver for OTF and TTF fonts with
    HarfBuzz text shaping) (see [Windows Fonts](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Windows-Fonts) in The GNU Emacs
    Manual). The `harfbuzz` driver is similarly recommended. On
    Haiku, there can be several font drivers (see [Haiku Fonts](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Haiku-Fonts) in The GNU Emacs Manual), as on Android (see [Android Fonts](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Android-Fonts) in The GNU Emacs Manual).

    On other systems, there is only one available font backend, so it does
    not make sense to modify this frame parameter.

`background-mode` [¶](#index-background_002dmode_002c-a-frame-parameter)
:   This parameter is either `dark` or `light`, according
    to whether the background color is a light one or a dark one.

`tty-color-mode` [¶](#index-standard-colors-for-character-terminals)
:   This parameter overrides the terminal’s color support as given by the
    system’s terminal capabilities database in that this parameter’s value
    specifies the color mode to use on a text terminal. The value can be
    either a symbol or a number. A number specifies the number of colors
    to use (and, indirectly, what commands to issue to produce each
    color). For example, `(tty-color-mode . 8)` specifies use of the
    ANSI escape sequences for 8 standard text colors. A value of −1 turns
    off color support.

    If the parameter’s value is a symbol, it specifies a number through
    the value of `tty-color-mode-alist`, and the associated number is
    used instead. This parameter supports dynamic changes during a running
    Emacs session (but not on MS-Windows and MS-DOS).

`screen-gamma` [¶](#index-gamma-correction)
:   If this is a number, Emacs performs gamma correction which adjusts
    the brightness of all colors. The value should be the screen gamma of
    your display.

    Usual PC monitors have a screen gamma of 2.2, so color values in
    Emacs, and in X windows generally, are calibrated to display properly
    on a monitor with that gamma value. If you specify 2.2 for
    `screen-gamma`, that means no correction is needed. Other values
    request correction, designed to make the corrected colors appear on
    your screen the way they would have appeared without correction on an
    ordinary monitor with a gamma value of 2.2.

    If your monitor displays colors too light, you should specify a
    `screen-gamma` value smaller than 2.2. This requests correction
    that makes colors darker. A screen gamma value of 1.5 may give good
    results for LCD color displays.

`alpha` [¶](#index-opacity_002c-frame)
:   This parameter specifies the opacity of the frame, on graphical
    displays that support variable opacity. It should be an integer
    between 0 and 100, where 0 means completely transparent and 100 means
    completely opaque. It can also have a `nil` value, which tells
    Emacs not to set the frame opacity (leaving it to the window manager).

    To prevent the frame from disappearing completely from view, the
    variable `frame-alpha-lower-limit` defines a lower opacity limit.
    If the value of the frame parameter is less than the value of this
    variable, Emacs uses the latter. By default,
    `frame-alpha-lower-limit` is 20.

    The `alpha` frame parameter can also be a cons cell
    `(active . inactive)`, where active is the
    opacity of the frame when it is selected, and inactive is the
    opacity when it is not selected.

    Some window systems do not support the `alpha` parameter for child
    frames (see [Child Frames](#Child-Frames)).

`alpha-background` [¶](#index-opacity_002c-frame-1)
:   Sets the background transparency of the frame. Unlike the `alpha`
    frame parameter, this only controls the transparency of the background
    while keeping foreground elements such as text fully opaque. It
    should be an integer between 0 and 100, where 0 means
    completely transparent and 100 means completely opaque (default). The
    value can also be a float between 0 and 1.0, where 1.0 means completely
    opaque.

`borders-respect-alpha-background` [¶](#index-opacity_002c-frame-2)
:   When non-`nil`, internal borders and window dividers are
    transparent according to `alpha-background`.

The following frame parameters are semi-obsolete in that they are
automatically equivalent to particular face attributes of particular
faces (see [Standard Faces](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Standard-Faces) in The Emacs Manual):

`font` [¶](#index-font_002c-a-frame-parameter)
:   The name of the font for displaying text in the frame. This is a
    string, either a valid font name for your system or the name of an Emacs
    fontset (see [Fontsets](#Fontsets)). It is equivalent to the `font`
    attribute of the `default` face.

`foreground-color` [¶](#index-foreground_002dcolor_002c-a-frame-parameter)
:   The color to use for characters. It is equivalent to
    the `:foreground` attribute of the `default` face.

`background-color` [¶](#index-background_002dcolor_002c-a-frame-parameter)
:   The color to use for the background of characters. It is equivalent to
    the `:background` attribute of the `default` face.

`mouse-color` [¶](#index-mouse_002dcolor_002c-a-frame-parameter)
:   The color for the mouse pointer. It is equivalent to the `:background`
    attribute of the `mouse` face (see [Basic Faces](#Basic-Faces)).

`cursor-color` [¶](#index-cursor_002dcolor_002c-a-frame-parameter)
:   The color for the cursor that shows point. It is equivalent to the
    `:background` attribute of the `cursor` face.

`border-color` [¶](#index-border_002dcolor_002c-a-frame-parameter)
:   The color for the border of the frame. It is equivalent to the
    `:background` attribute of the `border` face.

`scroll-bar-foreground` [¶](#index-scroll_002dbar_002dforeground_002c-a-frame-parameter)
:   If non-`nil`, the color for the foreground of scroll bars. It is
    equivalent to the `:foreground` attribute of the
    `scroll-bar` face.

`scroll-bar-background` [¶](#index-scroll_002dbar_002dbackground_002c-a-frame-parameter)
:   If non-`nil`, the color for the background of scroll bars. It is
    equivalent to the `:background` attribute of the
    `scroll-bar` face.

---

Previous: [Window Frame Parameters](#Window-Frame-Parameters), Up: [Frame Parameters](#Frame-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.4.4 Geometry [¶](#Geometry-1)

Here’s how to examine the data in an X-style window geometry
specification:

Function: **x-parse-geometry** geom [¶](#index-x_002dparse_002dgeometry)
:   The function `x-parse-geometry` converts a standard X window
    geometry string to an alist that you can use as part of the argument to
    `make-frame`.

    The alist describes which parameters were specified in geom, and
    gives the values specified for them. Each element looks like
    `(parameter . value)`. The possible parameter
    values are `left`, `top`, `width`, and `height`.

    For the size parameters, the value must be an integer. The position
    parameter names `left` and `top` are not totally accurate,
    because some values indicate the position of the right or bottom edges
    instead. The value possibilities for the position parameters are:
    an integer, a list `(+ pos)`, or a list `(- pos)`;
    as previously described (see [Position Parameters](#Position-Parameters)).

    Here is an example:

    ```
    (x-parse-geometry "35x70+0-0")
         ⇒ ((height . 70) (width . 35)
             (top - 0) (left . 0))
    ```

---

Next: [Frame Titles](#Frame-Titles), Previous: [Frame Parameters](#Frame-Parameters), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.5 Terminal Parameters [¶](#Terminal-Parameters-1)

Each terminal has a list of associated parameters. These
*terminal parameters* are mostly a convenient way of storage for
terminal-local variables, but some terminal parameters have a special
meaning.

This section describes functions to read and change the parameter values
of a terminal. They all accept as their argument either a terminal or
a frame; the latter means use that frame’s terminal. An argument of
`nil` means the selected frame’s terminal.

Function: **terminal-parameters** &optional terminal [¶](#index-terminal_002dparameters)
:   This function returns an alist listing all the parameters of
    terminal and their values.

Function: **terminal-parameter** terminal parameter [¶](#index-terminal_002dparameter)
:   This function returns the value of the parameter parameter (a
    symbol) of terminal. If terminal has no setting for
    parameter, this function returns `nil`.

Function: **set-terminal-parameter** terminal parameter value [¶](#index-set_002dterminal_002dparameter)
:   This function sets the parameter parameter of terminal to the
    specified value, and returns the previous value of that
    parameter.

Here’s a list of a few terminal parameters that have a special
meaning:

`background-mode`
:   The classification of the terminal’s background color, either
    `light` or `dark`.

`normal-erase-is-backspace`
:   Value is either 1 or 0, depending on whether
    `normal-erase-is-backspace-mode` is turned on or off on this
    terminal. See [DEL Does Not Delete](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#DEL-Does-Not-Delete) in The Emacs Manual.

`terminal-initted`
:   After the terminal is initialized, this is set to the
    terminal-specific initialization function.

`tty-mode-set-strings`
:   When present, a list of strings containing escape sequences that Emacs
    will output while configuring a tty for rendering. Emacs emits these
    strings only when configuring a terminal: if you want to enable a mode
    on a terminal that is already active (for example, while in
    `tty-setup-hook`), explicitly output the necessary escape
    sequence using `send-string-to-terminal` in addition to adding
    the sequence to `tty-mode-set-strings`.

`tty-mode-reset-strings`
:   When present, a list of strings that undo the effects of the strings
    in `tty-mode-set-strings`. Emacs emits these strings when
    exiting, deleting a terminal, or suspending itself.

---

Next: [Deleting Frames](#Deleting-Frames), Previous: [Terminal Parameters](#Terminal-Parameters), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.6 Frame Titles [¶](#Frame-Titles-1)

Every frame has a `name` parameter; this serves as the default
for the frame title which window systems typically display at the top of
the frame. You can specify a name explicitly by setting the `name`
frame property.

Normally you don’t specify the name explicitly, and Emacs computes the
frame name automatically based on a template stored in the variable
`frame-title-format`. Emacs recomputes the name each time the
frame is redisplayed.

Variable: **frame-title-format** [¶](#index-frame_002dtitle_002dformat)
:   This variable specifies how to compute a name for a frame when you
    have not explicitly specified one (via the frame’s parameters;
    see [Basic Parameters](#Basic-Parameters)). The variable’s value is actually a mode
    line construct, just like `mode-line-format`, except that the
    ‘`%c`’, ‘`%C`’, and ‘`%l`’ constructs are ignored.
    See [The Data Structure of the Mode Line](#Mode-Line-Data).

Variable: **icon-title-format** [¶](#index-icon_002dtitle_002dformat)
:   This variable specifies how to compute the name for an iconified frame
    when you have not explicitly specified the frame’s name via the
    frame’s parameters. The resulting title appears in the frame’s icon
    itself. If the value is a string, is should be a mode line construct
    like that of `frame-title-format`. The value can also be
    `t`, which means to use `frame-title-format` instead; this
    avoids problems with some window managers and desktop environments,
    where a change in a frame’s title (when a frame is iconified) is
    interpreted as a request to raise the frame and/or give it input
    focus. It is also useful if you want the frame’s title to be the same
    no matter if the frame is iconified or not. The default value is a
    string identical to the default value of `frame-title-format`.

Variable: **multiple-frames** [¶](#index-multiple_002dframes)
:   This variable is set automatically by Emacs. Its value is `t` when
    there are two or more frames (not counting minibuffer-only frames or
    invisible frames). The default value of `frame-title-format` uses
    `multiple-frames` so as to put the buffer name in the frame title
    only when there is more than one frame.

    The value of this variable is not guaranteed to be accurate except
    while processing `frame-title-format` or
    `icon-title-format`.

---

Next: [Finding All Frames](#Finding-All-Frames), Previous: [Frame Titles](#Frame-Titles), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.7 Deleting Frames [¶](#Deleting-Frames-1)

A *live frame* is one that has not been deleted. When a frame is
deleted, it is removed from its terminal display, although it may
continue to exist as a Lisp object until there are no more references to
it.

Command: **delete-frame** &optional frame force [¶](#index-delete_002dframe)
:   This function deletes the frame frame. The argument frame
    must specify a live frame (see below) and defaults to the selected
    frame.

    It first deletes any child frame of frame (see [Child Frames](#Child-Frames))
    and any frame whose `delete-before` frame parameter (see [Frame Interaction Parameters](#Frame-Interaction-Parameters)) specifies frame. All such deletions are
    performed recursively; so this step makes sure that no other frames with
    frame as their ancestor will exist. Then, unless frame
    specifies a tooltip, this function runs the hook
    `delete-frame-functions` (each function getting one argument,
    frame) before actually killing the frame. After actually killing
    the frame and removing the frame from the frame list, `delete-frame`
    runs `after-delete-frame-functions`.

    Note that a frame cannot be deleted as long as its minibuffer serves as
    surrogate minibuffer for another frame (see [Minibuffers and Frames](#Minibuffers-and-Frames)).
    Normally, you cannot delete a frame if all other frames are invisible,
    but if force is non-`nil`, then you are allowed to do so.
    Also, the initial terminal frame of an Emacs process running as daemon
    (see [daemon](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Initial-Options) in The GNU Emacs Manual) can be
    deleted if and only if force is non-`nil`.

Function: **frame-live-p** frame [¶](#index-frame_002dlive_002dp)
:   This function returns non-`nil` if the frame frame has not
    been deleted. The possible non-`nil` return values are like those
    of `framep`. See [Frames](#Frames).

Some window managers provide a command to delete a window. These work
by sending a special message to the program that operates the window.
When Emacs gets one of these commands, it generates a
`delete-frame` event, whose normal definition is a command that
calls the function `delete-frame`. See [Miscellaneous System Events](#Misc-Events).

Command: **delete-other-frames** &optional frame iconify [¶](#index-delete_002dother_002dframes)
:   This command deletes all frames on frame’s terminal, except
    frame. If frame uses another frame’s minibuffer, that
    minibuffer frame is left untouched. The argument frame must
    specify a live frame and defaults to the selected frame. Internally,
    this command works by calling `delete-frame` with force
    `nil` for all frames that shall be deleted.

    This function does not delete any of frame’s child frames
    (see [Child Frames](#Child-Frames)). If frame is a child frame, it deletes
    frame’s siblings only.

    With the prefix argument iconify, the frames are iconified rather
    than deleted.

The following function checks whether a frame can be safely deleted. It
is useful for avoiding the situation whereby a subsequent call of
`delete-frame` fails to delete its argument frame and/or
signals an error. To that end, your Lisp program should call
`delete-frame` only if the following function returns
non-`nil`.

Function: **frame-deletable-p** &optional frame [¶](#index-frame_002ddeletable_002dp)
:   This function returns non-`nil` if the frame specified by
    frame can be safely deleted. frame must be a live frame and
    defaults to the selected frame.

    A frame cannot be safely deleted in the following cases:

    * It is the only visible or iconified frame (see [Visibility of Frames](#Visibility-of-Frames)) on its terminal.
    * It hosts the active minibuffer window and minibuffer windows do not
      follow the selected frame (see [(emacs)Basic Minibuffer](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Basic-Minibuffer)).
    * All other visible or iconified frames on the same terminal are either
      child frames (see [Child Frames](#Child-Frames)) or have a non-`nil`
      `delete-before` parameter.
    * The frame or one of its descendants hosts the minibuffer window of a
      frame that is not a descendant of the frame (see [Child Frames](#Child-Frames)).

    These conditions cover most cases where `delete-frame` might fail
    when called from top-level. They do not catch some special cases like,
    for example, deleting a frame during a drag-and-drop operation
    (see [Drag and Drop](#Drag-and-Drop)). In any such case, it will be better to wrap
    the `delete-frame` call in a `condition-case` form.

When the selected frame gets deleted Emacs has to select another frame
instead. The following option lets you choose which one.

User Option: **delete-frame-choose-selected** [¶](#index-delete_002dframe_002dchoose_002dselected)
:   This variable specifies which frame to select after deleting the
    selected frame. The default value `mru` means `delete-frame`
    should try to select the most recently used frame as returned by
    `get-mru-frame` (see [Input Focus](#Input-Focus)) on the same terminal. The
    value `nil` means that `delete-frame` will try to select the
    oldest visible frame on the same terminal as the deleted one.

---

Next: [Splitting and Merging Frames](#Splitting-and-Merging-Frames), Previous: [Deleting Frames](#Deleting-Frames), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.8 Finding All Frames [¶](#Finding-All-Frames-1)

Function: **frame-list** [¶](#index-frame_002dlist)
:   This function returns a list of all the live frames, i.e., those that
    have not been deleted. It is analogous to `buffer-list` for
    buffers, and includes frames on all terminals with the exception of
    tooltip frames (see [Tooltips](#Tooltips)). The list that you get is newly
    created, so modifying the list doesn’t have any effect on the internals
    of Emacs.

Function: **visible-frame-list** [¶](#index-visible_002dframe_002dlist)
:   This function returns a list of just the currently visible frames.
    See [Visibility of Frames](#Visibility-of-Frames). Frames on text terminals will count as
    visible even though only the selected one is actually displayed.

Function: **frame-list-z-order** &optional display [¶](#index-frame_002dlist_002dz_002dorder)
:   This function returns a list of Emacs’s frames, in Z (stacking) order
    (see [Raising, Lowering and Restacking Frames](#Raising-and-Lowering)). The optional argument display
    specifies which display to poll. display should be either a frame
    or a display name (a string). If omitted or `nil`, that stands for
    the selected frame’s display. It returns `nil` if display
    contains no Emacs frame.

    Frames are listed from topmost (first) to bottommost (last). As a
    special case, if display is non-`nil` and specifies a live
    frame, it returns the child frames of that frame in Z (stacking) order.

    This function is not meaningful on text terminals.

Function: **next-frame** &optional frame minibuf [¶](#index-next_002dframe)
:   This function lets you cycle conveniently through all the frames on a
    specific terminal from an arbitrary starting point. It returns the
    frame following frame, in the list of all live frames, on
    frame’s terminal. The argument frame must specify a live
    frame and defaults to the selected frame. It does not return a frame
    whose `no-other-frame` parameter (see [Frame Interaction Parameters](#Frame-Interaction-Parameters)) is non-`nil`.

    The second argument, minibuf, says which frames to consider when
    deciding what the next frame should be:

    `nil`
    :   Consider all frames except minibuffer-only frames.

    `visible`
    :   Consider only visible frames.

    0
    :   Consider only visible or iconified frames.

    a window
    :   Consider only the frames using that particular window as their
        minibuffer window.

    anything else
    :   Consider all frames.

    If this function does not find a suitable frame, it returns frame
    even if it would not qualify according to the minibuf argument or
    its `no-other-frame` parameter.

Function: **previous-frame** &optional frame minibuf [¶](#index-previous_002dframe)
:   Like `next-frame`, but cycles through all frames in the opposite
    direction.

See also `next-window` and `previous-window`, in [Cyclic Ordering of Windows](#Cyclic-Window-Ordering).

Some Lisp programs need to find one or more frames that satisfy
given criteria. The function `filtered-frame-list` is provided for
this purpose.

Function: **filtered-frame-list** predicate [¶](#index-filtered_002dframe_002dlist)
:   This function returns the list of all the live frames which satisfy the
    specified predicate. The argument predicate must be a
    function of one argument, a frame to be tested against the filtering
    criteria, and should return non-`nil` if the frame satisfies the
    criteria.

---

Next: [Minibuffers and Frames](#Minibuffers-and-Frames), Previous: [Finding All Frames](#Finding-All-Frames), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
