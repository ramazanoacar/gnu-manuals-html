### 30.2 Multiple Terminals [¶](#Multiple-Terminals-1)

Emacs represents each terminal as a *terminal object* data type
(see [Terminal Type](#Terminal-Type)). On GNU and Unix systems, Emacs can use
multiple terminals simultaneously in each session. On other systems,
it can only use a single terminal. Each terminal object has the
following attributes:

* The name of the device used by the terminal (e.g., ‘`:0.0`’ or
  `/dev/tty`).
* The terminal and keyboard coding systems used on the terminal.
  See [Terminal I/O Encoding](#Terminal-I_002fO-Encoding).
* The kind of display associated with the terminal. This is the symbol
  returned by the function `terminal-live-p` (i.e., `x`,
  `t`, `w32`, `ns`, `pc`, `haiku`, `pgtk`,
  or `android`). See [Frames](#Frames).
* A list of terminal parameters. See [Terminal Parameters](#Terminal-Parameters).

There is no primitive for creating terminal objects. Emacs creates
them as needed, such as when you call `make-frame-on-display`
(described below).

Function: **terminal-name** &optional terminal [¶](#index-terminal_002dname)
:   This function returns the file name of the device used by
    terminal. If terminal is omitted or `nil`, it
    defaults to the selected frame’s terminal. terminal can also be
    a frame, meaning that frame’s terminal.

Function: **terminal-list** [¶](#index-terminal_002dlist)
:   This function returns a list of all live terminal objects.

Function: **get-device-terminal** device [¶](#index-get_002ddevice_002dterminal)
:   This function returns a terminal whose device name is given by
    device. If device is a string, it can be either the file
    name of a terminal device, or the name of an X display of the form
    ‘`host:server.screen`’. If device is a
    frame, this function returns that frame’s terminal; `nil` means
    the selected frame. Finally, if device is a terminal object
    that represents a live terminal, that terminal is returned. The
    function signals an error if its argument is none of the above.

Function: **delete-terminal** &optional terminal force [¶](#index-delete_002dterminal)
:   This function deletes all frames on terminal and frees the
    resources used by it. It runs the abnormal hook
    `delete-terminal-functions`, passing terminal as the
    argument to each function.

    If terminal is omitted or `nil`, it defaults to the
    selected frame’s terminal. terminal can also be a frame,
    meaning that frame’s terminal.

    Normally, this function signals an error if you attempt to delete the
    sole active terminal, but if force is non-`nil`, you are
    allowed to do so. Emacs automatically calls this function when the
    last frame on a terminal is deleted (see [Deleting Frames](#Deleting-Frames)).

Variable: **delete-terminal-functions** [¶](#index-delete_002dterminal_002dfunctions)
:   An abnormal hook run by `delete-terminal`. Each function
    receives one argument, the terminal argument passed to
    `delete-terminal`. Due to technical details, the functions may
    be called either just before the terminal is deleted, or just
    afterwards.

A few Lisp variables are *terminal-local*; that is, they have a
separate binding for each terminal. The binding in effect at any time
is the one for the terminal that the currently selected frame belongs
to. These variables include `default-minibuffer-frame`,
`defining-kbd-macro`, `last-kbd-macro`, and
`system-key-alist`. They are always terminal-local, and can
never be buffer-local (see [Buffer-Local Variables](#Buffer_002dLocal-Variables)).

On GNU and Unix systems, each X display is a separate graphical
terminal. When Emacs is started from within the X window system, it
uses the X display specified by the `DISPLAY` environment
variable, or by the ‘`--display`’ option (see [Initial Options](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Initial-Options) in The GNU Emacs Manual). Emacs can connect to other X displays
via the command `make-frame-on-display`. Each X display has its
own selected frame and its own minibuffer windows; however, only one
of those frames is *the* selected frame at any given moment
(see [Input Focus](#Input-Focus)). Emacs can even connect to other text
terminals, by interacting with the `emacsclient` program.
See [Emacs Server](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Emacs-Server) in The GNU Emacs Manual.

A single X server can handle more than one display. Each X display
has a three-part name,
‘`hostname:displaynumber.screennumber`’. The
first part, hostname, specifies the name of the machine to which
the display is physically connected. The second part,
displaynumber, is a zero-based number that identifies one or
more monitors connected to that machine that share a common keyboard
and pointing device (mouse, tablet, etc.). The third part,
screennumber, identifies a zero-based screen number (a separate
monitor) that is part of a single monitor collection on that X server.
When you use two or more screens belonging to one server, Emacs knows
by the similarity in their names that they share a single keyboard.

Systems that don’t use the X window system, such as MS-Windows,
don’t support the notion of X displays, and have only one display on
each host. The display name on these systems doesn’t follow the above
3-part format; for example, the display name on MS-Windows systems is
a constant string ‘`w32`’, and exists for compatibility, so that
you could pass it to functions that expect a display name.

Command: **make-frame-on-display** display &optional parameters [¶](#index-make_002dframe_002don_002ddisplay)
:   This function creates and returns a new frame on display, taking
    the other frame parameters from the alist parameters.
    display should be the name of an X display (a string).

    Before creating the frame, this function ensures that Emacs is set
    up to display graphics. For instance, if Emacs has not processed X
    resources (e.g., if it was started on a text terminal), it does so at
    this time. In all other respects, this function behaves like
    `make-frame` (see [Creating Frames](#Creating-Frames)).

Function: **x-display-list** [¶](#index-x_002ddisplay_002dlist)
:   This function returns a list that indicates which X displays Emacs has
    a connection to. The elements of the list are strings, and each one
    is a display name.

Function: **x-open-connection** display &optional xrm-string must-succeed [¶](#index-x_002dopen_002dconnection)
:   This function opens a connection to the X display display,
    without creating a frame on that display. Normally, Emacs Lisp
    programs need not call this function, as `make-frame-on-display`
    calls it automatically. The only reason for calling it is to check
    whether communication can be established with a given X display.

    The optional argument xrm-string, if not `nil`, is a string
    of resource names and values, in the same format used in the
    `.Xresources` file. See [X Resources](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#X-Resources) in The
    GNU Emacs Manual. These values apply to all Emacs frames created on
    this display, overriding the resource values recorded in the X server.
    Here’s an example of what this string might look like:

    ```
    "*BorderWidth: 3\n*InternalBorder: 2\n"
    ```

    If must-succeed is non-`nil`, failure to open the connection
    terminates Emacs. Otherwise, it is an ordinary Lisp error.

Function: **x-close-connection** display [¶](#index-x_002dclose_002dconnection)
:   This function closes the connection to display display. Before
    you can do this, you must first delete all the frames that were open
    on that display (see [Deleting Frames](#Deleting-Frames)).

On some multi-monitor setups, a single X display outputs to more
than one physical monitor. You can use the functions
`display-monitor-attributes-list` and `frame-monitor-attributes`
to obtain information about such setups.

Function: **display-monitor-attributes-list** &optional display [¶](#index-display_002dmonitor_002dattributes_002dlist)
:   This function returns a list of physical monitor attributes on
    display, which can be a display name (a string), a terminal, or
    a frame; if omitted or `nil`, it defaults to the selected frame’s
    display. Each element of the list is an association list,
    representing the attributes of a physical monitor. The first element
    corresponds to the primary monitor. The attribute keys and values
    are:

    ‘`geometry`’
    :   Position of the top-left corner of the monitor’s screen and its size,
        in pixels, as ‘`(x y width height)`’. Note
        that, if the monitor is not the primary monitor, some of the
        coordinates might be negative.

    ‘`workarea`’
    :   Position of the top-left corner and size of the work area (usable
        space) in pixels as ‘`(x y width height)`’.
        This may be different from ‘`geometry`’ in that space occupied by
        various window manager features (docks, taskbars, etc.) may be
        excluded from the work area. Whether or not such features actually
        subtract from the work area depends on the platform and environment.
        Again, if the monitor is not the primary monitor, some of the
        coordinates might be negative.

    ‘`mm-size`’
    :   Width and height in millimeters as ‘`(width height)`’

    ‘`frames`’
    :   List of frames that this physical monitor dominates (see below).

    ‘`name`’
    :   Name of the physical monitor as string.

    ‘`source`’
    :   Source of the multi-monitor information as string;
        on X, it could be ‘`XRandR 1.5`’, ‘`XRandr`’, ‘`Xinerama`’,
        ‘`Gdk`’, or ‘`fallback`’. The last value of ‘`source`’ means
        that Emacs was built without GTK and without XRandR or Xinerama
        extensions, in which case the information about multiple physical
        monitors will be provided as if they all as a whole formed a single
        monitor.

    x, y, width, and height are integers.
    ‘`name`’ and ‘`source`’ may be absent.

    A frame is *dominated* by a physical monitor when either the
    largest area of the frame resides in that monitor, or (if the frame
    does not intersect any physical monitors) that monitor is the closest
    to the frame. Every (non-tooltip) frame (whether visible or not) in a
    graphical display is dominated by exactly one physical monitor at a
    time, though the frame can span multiple (or no) physical monitors.

    Here’s an example of the data produced by this function on a 2-monitor
    display:

    ```
      (display-monitor-attributes-list)
      ⇒
      (((geometry 0 0 1920 1080) ;; Left-hand, primary monitor
        (workarea 0 0 1920 1050) ;; A taskbar occupies some of the height
        (mm-size 677 381)
        (name . "DISPLAY1")
        (frames #<frame emacs@host *Messages* 0x11578c0>
                #<frame emacs@host *scratch* 0x114b838>))
       ((geometry 1920 0 1680 1050) ;; Right-hand monitor
        (workarea 1920 0 1680 1050) ;; Whole screen can be used
        (mm-size 593 370)
        (name . "DISPLAY2")
        (frames)))
    ```

Function: **frame-monitor-attributes** &optional frame [¶](#index-frame_002dmonitor_002dattributes)
:   This function returns the attributes of the physical monitor
    dominating (see above) frame, which defaults to the selected frame.

On multi-monitor displays it is possible to use the command
`make-frame-on-monitor` to make frames on the specified monitor.

Command: **make-frame-on-monitor** monitor &optional display parameters [¶](#index-make_002dframe_002don_002dmonitor)
:   This function creates and returns a new frame on monitor located
    on display, taking the other frame parameters from the alist
    parameters. monitor should be the name of the physical
    monitor, the same string as returned by the function
    `display-monitor-attributes-list` in the attribute `name`.
    display should be the name of an X display (a string).

Variable: **display-monitors-changed-functions** [¶](#index-display_002dmonitors_002dchanged_002dfunctions)
:   This variable is an abnormal hook run when the monitor configuration
    changes, which can happen if a monitor is rotated, moved, added or
    removed from a multiple-monitor setup, if the primary monitor changes,
    or if the resolution of a monitor changes. It is called with a single
    argument consisting of the terminal on which the monitor configuration
    changed. Programs should call `display-monitor-attributes-list`
    with the terminal as the argument to retrieve the new monitor
    configuration on that terminal.

---

Next: [Frame Parameters](#Frame-Parameters), Previous: [Multiple Terminals](#Multiple-Terminals), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.3 Frame Geometry [¶](#Frame-Geometry-1)

The geometry of a frame depends on the toolkit that was used to build
this instance of Emacs and the terminal that displays the frame. This
chapter describes these dependencies and some of the functions to deal
with them. Note that the frame argument of all of these functions
has to specify a live frame (see [Deleting Frames](#Deleting-Frames)). If omitted or
`nil`, it specifies the selected frame (see [Input Focus](#Input-Focus)).

* [Frame Layout](#Frame-Layout)
* [Frame Font](#Frame-Font)
* [Frame Position](#Frame-Position)
* [Frame Size](#Frame-Size)
* [Implied Frame Resizing](#Implied-Frame-Resizing)

---

Next: [Frame Font](#Frame-Font), Up: [Frame Geometry](#Frame-Geometry)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.3.1 Frame Layout [¶](#Frame-Layout-1)

A visible frame occupies a rectangular area on its terminal’s display.
This area may contain a number of nested rectangles, each serving a
different purpose. The drawing below sketches the layout of a frame on
a graphical terminal:

```
        <------------ Outer Frame Width ----------->
        ____________________________________________
     ^(0)  ________ External/Outer Border _______   |
     | |  |_____________ Title Bar ______________|  |
     | | (1)_____________ Menu Bar ______________|  | ^
     | | (2)_____________ Tool Bar ______________|  | ^
     | | (3)_____________ Tab Bar _______________|  | ^
     | |  |  _________ Internal Border ________  |  | ^
     | |  | |   ^                              | |  | |
     | |  | |   |                              | |  | |
Outer  |  | | Inner                            | |  | Native
Frame  |  | | Frame                            | |  | Frame
Height |  | | Height                           | |  | Height
     | |  | |   |                              | |  | |
     | |  | |<--+--- Inner Frame Width ------->| |  | |
     | |  | |   |                              | |  | |
     | |  | |___v______________________________| |  | |
     | |  |___________ Internal Border __________|  | |
     | | (4)__________ Bottom Tool Bar __________|  | v
     v |___________ External/Outer Border __________|
           <-------- Native Frame Width -------->
```

In practice not all of the areas shown in the drawing will or may be
present. The meaning of these areas is described below.

Outer Frame [¶](#index-outer-frame)
:   The *outer frame* is a rectangle comprising all areas shown in the
    drawing. The edges of that rectangle are called the *outer edges*
    of the frame. Together, the *outer width* and *outer height* of
    the frame specify the *outer size* of that rectangle.

    Knowing the outer size of a frame is useful for fitting a frame into the
    working area of its display (see [Multiple Terminals](#Multiple-Terminals)) or for placing
    two frames adjacent to each other on the screen. Usually, the outer
    size of a frame is available only after the frame has been mapped (made
    visible, see [Visibility of Frames](#Visibility-of-Frames)) at least once. For the initial
    frame or a frame that has not been created yet, the outer size can be
    only estimated or must be calculated from the window-system’s or window
    manager’s defaults. One workaround is to obtain the differences of the
    outer and native (see below) sizes of a mapped frame and use them for
    calculating the outer size of the new frame.

    The position of the upper left corner of the outer frame (indicated by
    ‘`(0)`’ in the drawing above) is the *outer position* of the
    frame. The outer position of a graphical frame is also referred to as
    “the position” of the frame because it usually remains unchanged on
    its display whenever the frame is resized or its layout is changed.

    The outer position is specified by and can be set via the `left`
    and `top` frame parameters (see [Position Parameters](#Position-Parameters)). For a
    normal, top-level frame these parameters usually represent its absolute
    position (see below) with respect to its display’s origin. For a child
    frame (see [Child Frames](#Child-Frames)) these parameters represent its position
    relative to the native position (see below) of its parent frame. For
    root frames (see [Child Frames](#Child-Frames)) on text terminals the values of these
    parameters are meaningless and always zero.

External Border [¶](#index-external-border)
:   The *external border* is part of the decorations supplied by the
    window manager. It is typically used for resizing the frame with the
    mouse and is therefore not shown on “fullboth” and maximized frames
    (see [Size Parameters](#Size-Parameters)). Its width is determined by the window
    manager and cannot be changed by Emacs’s functions.

    External borders don’t exist on text terminal frames. For graphical
    frames, their display can be suppressed by setting the
    `override-redirect` or `undecorated` frame parameter
    (see [Window Management Parameters](#Management-Parameters)).

Outer Border [¶](#index-outer-border)
:   The *outer border* is a separate border whose width can be specified
    with the `border-width` frame parameter (see [Layout Parameters](#Layout-Parameters)). In practice, either the external or the outer border of a
    frame are displayed but never both at the same time. Usually, the outer
    border is shown only for special frames that are not (fully) controlled
    by the window manager like tooltip frames (see [Tooltips](#Tooltips)), child
    frames (see [Child Frames](#Child-Frames)) and `undecorated` or
    `override-redirect` frames (see [Window Management Parameters](#Management-Parameters)).

    As a rule, outer borders are never shown on text terminal frames and on
    frames generated by GTK+ routines. For a child frame on a text terminal
    you can emulate the outer border by setting the `undecorated`
    parameter of that frame to `nil` (see [Layout Parameters](#Layout-Parameters)). On
    MS-Windows, the outer border is emulated with the help of a one pixel
    wide external border. Non-toolkit builds on X allow changing the color
    of the outer border by setting the `border-color` frame parameter
    (see [Layout Parameters](#Layout-Parameters)).

Title Bar [¶](#index-title-bar)
:   The *title bar*, a.k.a. *caption bar*, is also part of the
    window manager’s decorations and typically displays the title of the
    frame (see [Frame Titles](#Frame-Titles)) as well as buttons for minimizing,
    maximizing and deleting the frame. It can be also used for dragging
    the frame with the mouse. The title bar is usually not displayed for
    fullboth (see [Size Parameters](#Size-Parameters)), tooltip (see [Tooltips](#Tooltips)) and
    child frames (see [Child Frames](#Child-Frames)) and doesn’t exist for terminal
    frames. Display of the title bar can be suppressed by setting the
    `override-redirect` or the `undecorated` frame parameters
    (see [Window Management Parameters](#Management-Parameters)).

Menu Bar [¶](#index-internal-menu-bar)
:   The menu bar (see [The Menu Bar](#Menu-Bar)) can be either internal (drawn by Emacs
    itself) or external (drawn by the toolkit). Most builds (GTK+, Lucid,
    Motif and MS-Windows) rely on an external menu bar. NS also uses an
    external menu bar which, however, is not part of the outer frame.
    Non-toolkit builds can provide an internal menu bar. On text terminal
    frames, the menu bar is part of the frame’s root window (see [Windows and Frames](#Windows-and-Frames)). As a rule, menu bars are never shown on child frames
    (see [Child Frames](#Child-Frames)). Display of the menu bar can be suppressed by
    setting the `menu-bar-lines` parameter (see [Layout Parameters](#Layout-Parameters))
    to zero.

    Whether the menu bar is wrapped or truncated whenever its width
    becomes too large to fit on its frame depends on the toolkit .
    Usually, only Motif and MS-Windows builds can wrap the menu bar. When
    they (un-)wrap the menu bar, they try to keep the outer height of the
    frame unchanged, so the native height of the frame (see below) will
    change instead.

Tool Bar [¶](#index-internal-tool-bar)
:   Like the menu bar, the tool bar (see [Tool bars](#Tool-Bar)) can be either
    internal (drawn by Emacs itself) or external (drawn by a toolkit). The
    GTK+ and NS builds have the tool bar drawn by the toolkit. The
    remaining builds use internal tool bars. With GTK+ the tool bar can be
    located on either side of the frame, immediately outside the internal
    border, see below. Tool bars are usually not shown for child frames
    (see [Child Frames](#Child-Frames)). Display of the tool bar can be suppressed by
    setting the `tool-bar-lines` parameter (see [Layout Parameters](#Layout-Parameters)) to zero.

    If the variable `auto-resize-tool-bars` is non-`nil`, Emacs
    wraps the internal tool bar when its width becomes too large for its
    frame. If and when Emacs (un-)wraps the internal tool bar, it by
    default keeps the outer height of the frame unchanged, so the native
    height of the frame (see below) will change instead. Emacs built with
    GTK+, on the other hand, never wraps the tool bar but may
    automatically increase the outer width of a frame in order to
    accommodate an overlong tool bar.

Tab Bar [¶](#index-tab-bar)
:   The tab bar (see [Tab Bars](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Tab-Bars) in The GNU Emacs Manual) is always
    drawn by Emacs itself. The tab bar appears above the tool bar in
    Emacs built with an internal tool bar, and below the tool bar in
    builds with an external tool bar.
    Display of the tab bar can be suppressed by setting the
    `tab-bar-lines` parameter (see [Layout Parameters](#Layout-Parameters)) to zero.

Native Frame [¶](#index-native-frame)
:   The *native frame* is a rectangle located entirely within the outer
    frame. It excludes the areas occupied by an external or outer border,
    the title bar and any external menu or tool bar. The edges of the
    native frame are called the *native edges* of the frame. Together,
    the *native width* and *native height* of a frame specify the
    *native size* of the frame.

    The native size of a frame is the size Emacs passes to the window-system
    or window manager when creating or resizing the frame from within Emacs.
    It is also the size Emacs receives from the window-system or window
    manager whenever these resize the frame’s window-system window, for
    example, after maximizing the frame by clicking on the corresponding
    button in the title bar or when dragging its external border with the
    mouse.

    The position of the top left corner of the native frame specifies the
    *native position* of the frame. (1)–(3) in the drawing above
    indicate that position for the various builds:

    * (1) non-toolkit, Android, Haiku, and terminal frames
    * (2) Lucid, Motif, and MS-Windows frames
    * (3) GTK+ and NS frames

    Accordingly, the native height of a frame may include the height of the
    tool bar but not that of the menu bar (Lucid, Motif, MS-Windows) or
    those of the menu bar and the tool bar (non-toolkit and text terminal
    frames).

    If the native position would otherwise be (2), but the tool bar is
    placed at the bottom of the frame as depicted in (4), the native
    position of the frame becomes that of the tab bar.

    The native position of a frame is the reference position for functions
    that set or return the current position of the mouse (see [Mouse Position](#Mouse-Position)) and for functions dealing with the position of windows like
    `window-edges`, `window-at` or `coordinates-in-window-p`
    (see [Coordinates and Windows](#Coordinates-and-Windows)). It also specifies the (0, 0) origin
    for locating and positioning child frames within this frame
    (see [Child Frames](#Child-Frames)).

    Note also that the native position of a frame usually remains unaltered
    on its display when removing or adding the window manager decorations by
    changing the frame’s `override-redirect` or `undecorated`
    parameter (see [Window Management Parameters](#Management-Parameters)).

Internal Border
:   The internal border is a border drawn by Emacs around the inner frame
    (see below). The specification of its appearance depends on whether
    or not the given frame is a child frame (see [Child Frames](#Child-Frames)).

    For normal frames its width is specified by the `internal-border-width`
    frame parameter (see [Layout Parameters](#Layout-Parameters)), and its color is specified by the
    background of the `internal-border` face.

    For child frames its width is specified by the `child-frame-border-width`
    frame parameter (but will use the `internal-border-width` parameter as
    fallback), and its color is specified by the background of the
    `child-frame-border` face.

Inner Frame [¶](#index-inner-frame)
:   The *inner frame* is the rectangle reserved for the frame’s windows.
    It’s enclosed by the internal border which, however, is not part of the
    inner frame. Its edges are called the *inner edges* of the frame.
    The *inner width* and *inner height* specify the *inner
    size* of the rectangle. The inner frame is sometimes also referred to
    as the *display area* of the frame.

    As a rule, the inner frame is subdivided into the frame’s root window
    (see [Windows and Frames](#Windows-and-Frames)) and the frame’s minibuffer window
    (see [Minibuffer Windows](#Minibuffer-Windows)). There are two notable exceptions to this
    rule: A *minibuffer-less frame* contains a root window only and does
    not contain a minibuffer window. A *minibuffer-only frame* contains
    only a minibuffer window which also serves as that frame’s root window.
    See [Initial Frame Parameters](#Initial-Parameters), for how to create such frame configurations.

Text Area [¶](#index-text-area)
:   The *text area* of a frame is a somewhat fictitious area that can be
    embedded in the native frame. Its position is unspecified. Its width
    can be obtained by removing from that of the native width the widths of
    the internal border, one vertical scroll bar, and one left and one right
    fringe if they are specified for this frame, see [Layout Parameters](#Layout-Parameters). Its height can be obtained by removing from that of the
    native height the widths of the internal border and the heights of the
    frame’s internal menu and tool bars, the tab bar and one horizontal
    scroll bar if specified for this frame.

The *absolute position* of a frame is given as a pair (X, Y) of
horizontal and vertical pixel offsets relative to an origin (0, 0) of
the frame’s display. Correspondingly, the *absolute edges* of a
frame are given as pixel offsets from that origin.

Note that with multiple monitors, the origin of the display does not
necessarily coincide with the top-left corner of the entire usable
display area of the terminal. Hence the absolute position of a frame
can be negative in such an environment even when that frame is
completely visible.

By convention, vertical offsets increase “downwards”. This means
that the height of a frame is obtained by subtracting the offset of its
top edge from that of its bottom edge. Horizontal offsets increase
“rightwards”, as expected, so a frame’s width is calculated by
subtracting the offset of its left edge from that of its right edge.

For a frame on a graphical terminal the following function returns the
sizes of the areas described above:

Function: **frame-geometry** &optional frame [¶](#index-frame_002dgeometry)
:   This function returns geometric attributes of frame. The return
    value is an association list of the attributes listed below. All
    coordinate, height and width values are integers counting pixels. Note
    that if frame has not been mapped yet, (see [Visibility of Frames](#Visibility-of-Frames)) some of the return values may only represent approximations of
    the actual values—those that can be seen after the frame has been
    mapped.

    `outer-position`
    :   A cons representing the absolute position of the outer frame,
        relative to the origin at position (0, 0) of frame’s display.

    `outer-size`
    :   A cons of the outer width and height of frame.

    `external-border-size`
    :   A cons of the horizontal and vertical width of frame’s external
        borders as supplied by the window manager. If the window manager
        doesn’t supply these values, Emacs will try to guess them from the
        coordinates of the outer and inner frame.

    `outer-border-width`
    :   The width of the outer border of frame. The value is meaningful
        for non-GTK+ X builds only.

    `title-bar-size`
    :   A cons of the width and height of the title bar of frame as
        supplied by the window manager or operating system. If both of them are
        zero, the frame has no title bar. If only the width is zero, Emacs was
        not able to retrieve the width information.

    `menu-bar-external`
    :   If non-`nil`, this means the menu bar is external (not part of the
        native frame of frame).

    `menu-bar-size`
    :   A cons of the width and height of the menu bar of frame.

    `tool-bar-external`
    :   If non-`nil`, this means the tool bar is external (not part of the
        native frame of frame).

    `tool-bar-position`
    :   This tells on which side the tool bar on frame is and can be one
        of `left`, `top`, `right` or `bottom`.

        The values `left` and `right` are only supported on builds
        using the GTK+ toolkit; `bottom` is supported on all builds other
        than NS, and `top` is supported everywhere.

    `tool-bar-size`
    :   A cons of the width and height of the tool bar of frame.

    `internal-border-width`
    :   The width of the internal border of frame.

The following function can be used to retrieve the edges of the outer,
native and inner frame.

Function: **frame-edges** &optional frame type [¶](#index-frame_002dedges)
:   This function returns the absolute edges of the outer, native or inner
    frame of frame. frame must be a live frame and defaults to
    the selected one. The returned list has the form `(left top right bottom)` where all values are in pixels
    relative to the origin of frame’s display. For terminal frames
    the values returned for left and top are always zero.

    Optional argument type specifies the type of the edges to return:
    `outer-edges` means to return the outer edges of frame,
    `native-edges` (or `nil`) means to return its native edges and
    `inner-edges` means to return its inner edges.

    By convention, the pixels of the display at the values returned for
    left and top are considered to be inside (part of)
    frame. Hence, if left and top are both zero, the
    pixel at the display’s origin is part of frame. The pixels at
    bottom and right, on the other hand, are considered to lie
    immediately outside frame. This means that if you have, for
    example, two side-by-side frames positioned such that the right outer
    edge of the frame on the left equals the left outer edge of the frame on
    the right, the pixels at that edge show a part of the frame on the
    right.

---

Next: [Frame Position](#Frame-Position), Previous: [Frame Layout](#Frame-Layout), Up: [Frame Geometry](#Frame-Geometry)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.3.2 Frame Font [¶](#Frame-Font-1)

Each frame has a *default font* which specifies the default
character size for that frame. This size is meant when retrieving or
changing the size of a frame in terms of columns or lines
(see [Size Parameters](#Size-Parameters)). It is also used when resizing (see [Window Sizes](#Window-Sizes)) or splitting (see [Splitting Windows](#Splitting-Windows)) windows.

The terms *line height* and *canonical character height* are
sometimes used instead of “default character height”. Similarly, the
terms *column width* and *canonical character width* are used
instead of “default character width”.

Function: **frame-char-height** &optional frame [¶](#index-frame_002dchar_002dheight)

Function: **frame-char-width** &optional frame [¶](#index-frame_002dchar_002dwidth)
:   These functions return the default height and width of a character in
    frame, measured in pixels. Together, these values establish the
    size of the default font on frame. The values depend on the
    choice of font for frame, see [Font and Color Parameters](#Font-and-Color-Parameters).

The default font can be also set directly with the following function:

Command: **set-frame-font** font &optional keep-size frames [¶](#index-set_002dframe_002dfont)
:   This sets the default font to font. When called interactively, it
    prompts for the name of a font, and uses that font on the selected
    frame. When called from Lisp, font should be a font name (a
    string), a font object, font entity, or a font spec.

    If the optional argument keep-size is `nil`, this keeps the
    number of frame lines and columns fixed. (If non-`nil`, the option
    `frame-inhibit-implied-resize` described in the next section will
    override this.) If keep-size is non-`nil` (or with a prefix
    argument), it tries to keep the size of the display area of the current
    frame fixed by adjusting the number of lines and columns.

    If the optional argument frames is `nil`, this applies the
    font to the selected frame only. If frames is non-`nil`, it
    should be a list of frames to act upon, or `t` meaning all existing
    and all future graphical frames.

---

Next: [Frame Size](#Frame-Size), Previous: [Frame Font](#Frame-Font), Up: [Frame Geometry](#Frame-Geometry)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.3.3 Frame Position [¶](#Frame-Position-1)

On graphical systems, the position of a normal top-level frame is
specified as the absolute position of its outer frame (see [Frame Geometry](#Frame-Geometry)). The position of a child frame (see [Child Frames](#Child-Frames)) is
specified via pixel offsets of its outer edges relative to the native
position of its parent frame.

You can access or change the position of a frame using the frame
parameters `left` and `top` (see [Position Parameters](#Position-Parameters)).
Here are two additional functions for working with the positions of an
existing, visible frame. For both functions, the argument frame
must denote a live frame and defaults to the selected frame.

Function: **frame-position** &optional frame [¶](#index-frame_002dposition)
:   For a normal, non-child frame this function returns a cons of the pixel
    coordinates of its outer position (see [Frame Layout](#Frame-Layout)) with respect to
    the origin `(0, 0)` of its display. For a child frame
    (see [Child Frames](#Child-Frames)) this function returns the pixel coordinates of
    its outer position with respect to an origin `(0, 0)` at the native
    position of frame’s parent.

    Negative values never indicate an offset from the right or bottom
    edge of frame’s display or parent frame. Rather, they mean that
    frame’s outer position is on the left and/or above the origin of
    its display or the native position of its parent frame. This usually
    means that frame is only partially visible (or completely
    invisible). However, on systems where the display’s origin does not
    coincide with its top-left corner, the frame may be visible on a
    secondary monitor.

    On a text terminal frame both values are zero for root frames
    (see [Child Frames](#Child-Frames)).

Function: **set-frame-position** frame x y [¶](#index-set_002dframe_002dposition)
:   This function sets the outer frame position of frame to (x,
    y). The latter arguments specify pixels and normally count from
    the origin at the position (0, 0) of frame’s display. For child
    frames, they count from the native position of frame’s parent
    frame.

    Negative parameter values position the right edge of the outer frame by
    -x pixels left from the right edge of the screen (or the parent
    frame’s native rectangle) and the bottom edge by -y pixels up from
    the bottom edge of the screen (or the parent frame’s native rectangle).

    Note that negative values do not permit aligning the right or bottom
    edge of frame exactly at the right or bottom edge of its display
    or parent frame. Neither do they allow specifying a position that does
    not lie within the edges of the display or parent frame. The frame
    parameters `left` and `top` (see [Position Parameters](#Position-Parameters))
    allow doing that, but may still fail to provide good results for the
    initial or a new frame.

    This function has no effect on text terminal frames.

Variable: **move-frame-functions** [¶](#index-move_002dframe_002dfunctions)
:   This hook specifies the functions that are run when an Emacs frame is moved
    (assigned a new position) by the window-system or window manager. The
    functions are run with one argument, the frame that moved. For a child
    frame (see [Child Frames](#Child-Frames)), the functions are run only when the
    position of the frame changes in relation to that of its parent frame.

---

Next: [Implied Frame Resizing](#Implied-Frame-Resizing), Previous: [Frame Position](#Frame-Position), Up: [Frame Geometry](#Frame-Geometry)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.3.4 Frame Size [¶](#Frame-Size-1)

The canonical way to specify the *size of a frame* from within Emacs
is by specifying its *text size*—a tuple of the width and height
of the frame’s text area (see [Frame Layout](#Frame-Layout)). It can be measured
either in pixels or in terms of the frame’s canonical character size
(see [Frame Font](#Frame-Font)).

For frames with an internal menu or tool bar, the frame’s native
height cannot be told exactly before the frame has been actually drawn.
This means that in general you cannot use the native size to specify the
initial size of a frame. As soon as you know the native size of a
visible frame, you can calculate its outer size (see [Frame Layout](#Frame-Layout))
by adding in the remaining components from the return value of
`frame-geometry`. For invisible frames or for frames that have
yet to be created, however, the outer size can only be estimated. This
also means that calculating an exact initial position of a frame
specified via offsets from the right or bottom edge of the screen
(see [Frame Position](#Frame-Position)) is impossible.

The text size of any frame can be set and retrieved with the help of
the `height` and `width` frame parameters (see [Size Parameters](#Size-Parameters)). The text size of the initial frame can be also set with
the help of an X-style geometry specification. See [Command Line Arguments for Emacs Invocation](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Emacs-Invocation) in The GNU Emacs
Manual. Below we list some functions to access and set the size of an
existing, visible frame, by default the selected one.

Function: **frame-height** &optional frame [¶](#index-frame_002dheight)

Function: **frame-width** &optional frame [¶](#index-frame_002dwidth)
:   These functions return the height and width of the text area of
    frame, measured in units of the default font height and width of
    frame (see [Frame Font](#Frame-Font)). These functions are plain shorthands
    for writing `(frame-parameter frame 'height)` and
    `(frame-parameter frame 'width)`.

    If the text area of frame measured in pixels is not a multiple of
    its default font size, the values returned by these functions are
    rounded down to the number of characters of the default font that fully
    fit into the text area.

The functions following next return the pixel widths and heights of the
native, outer and inner frame and the text area (see [Frame Layout](#Frame-Layout))
of a given frame. For a text terminal, the results are in characters
rather than pixels.

Function: **frame-outer-width** &optional frame [¶](#index-frame_002douter_002dwidth)

Function: **frame-outer-height** &optional frame [¶](#index-frame_002douter_002dheight)
:   These functions return the outer width and height of frame in
    pixels.

Function: **frame-native-height** &optional frame [¶](#index-frame_002dnative_002dheight)

Function: **frame-native-width** &optional frame [¶](#index-frame_002dnative_002dwidth)
:   These functions return the native width and height of frame in
    pixels.

Function: **frame-inner-width** &optional frame [¶](#index-frame_002dinner_002dwidth)

Function: **frame-inner-height** &optional frame [¶](#index-frame_002dinner_002dheight)
:   These functions return the inner width and height of frame in
    pixels.

Function: **frame-text-width** &optional frame [¶](#index-frame_002dtext_002dwidth)

Function: **frame-text-height** &optional frame [¶](#index-frame_002dtext_002dheight)
:   These functions return the width and height of the text area of
    frame in pixels.

On window systems that support it, Emacs tries by default to make the
text size of a frame measured in pixels a multiple of the frame’s
character size. This, however, usually means that a frame can be
resized only in character size increments when dragging its external
borders. It also may break attempts to truly maximize the frame or
making it “fullheight” or “fullwidth” (see [Size Parameters](#Size-Parameters))
leaving some empty space below and/or on the right of the frame. The
following option may help in that case.

User Option: **frame-resize-pixelwise** [¶](#index-frame_002dresize_002dpixelwise)
:   If this option is `nil` (the default), a frame’s text pixel size is
    usually rounded to a multiple of the current values of that frame’s
    `frame-char-height` and `frame-char-width` whenever the frame
    is resized. If this is non-`nil`, no rounding occurs, hence frame
    sizes can increase/decrease by one pixel.

    Setting this variable usually causes the next resize operation to pass
    the corresponding size hints to the window manager. This means that
    this variable should be set only in a user’s initial file; applications
    should never bind it temporarily.

    The precise meaning of a value of `nil` for this option depends on
    the toolkit used. Dragging the external border with the mouse is done
    character-wise provided the window manager is willing to process the
    corresponding size hints. Calling `set-frame-size` (see below)
    with arguments that do not specify the frame size as an integer multiple
    of its character size, however, may: be ignored, cause a rounding
    (GTK+), or be accepted (Lucid, Motif, MS-Windows).

    With some window managers you may have to set this to non-`nil` in
    order to make a frame appear truly maximized or full-screen.

Function: **set-frame-size** frame width height &optional pixelwise [¶](#index-set_002dframe_002dsize)
:   This function sets the size of the text area of frame, measured in
    terms of the canonical height and width of a character on frame
    (see [Frame Font](#Frame-Font)).

    The optional argument pixelwise non-`nil` means to measure
    the new width and height in units of pixels instead. Note that if
    `frame-resize-pixelwise` is `nil`, some toolkits may refuse to
    truly honor the request if it does not increase/decrease the frame size
    to a multiple of its character size.

Function: **set-frame-height** frame height &optional pretend pixelwise [¶](#index-set_002dframe_002dheight)
:   This function resizes the text area of frame to a height of
    height lines. The sizes of existing windows in frame are
    altered proportionally to fit.

    If pretend is non-`nil`, then Emacs displays height
    lines of output in frame, but does not change its value for the
    actual height of the frame. This is only useful on text terminals.
    Using a smaller height than the terminal actually implements may be
    useful to reproduce behavior observed on a smaller screen, or if the
    terminal malfunctions when using its whole screen. Setting the frame
    height directly does not always work, because knowing the correct
    actual size may be necessary for correct cursor positioning on
    text terminals.

    The optional fourth argument pixelwise non-`nil` means that
    frame should be height pixels high. Note that if
    `frame-resize-pixelwise` is `nil`, some window managers may
    refuse to truly honor the request if it does not increase/decrease the
    frame height to a multiple of its character height.

    When used interactively, this command will ask the user for the number
    of lines to set the height of the currently selected frame. You can
    also provide this value with a numeric prefix.

Function: **set-frame-width** frame width &optional pretend pixelwise [¶](#index-set_002dframe_002dwidth)
:   This function sets the width of the text area of frame, measured
    in characters. The argument pretend has the same meaning as in
    `set-frame-height`.

    The optional fourth argument pixelwise non-`nil` means that
    frame should be width pixels wide. Note that if
    `frame-resize-pixelwise` is `nil`, some window managers may
    refuse to fully honor the request if it does not increase/decrease the
    frame width to a multiple of its character width.

    When used interactively, this command will ask the user for the number
    of columns to set the width of the currently selected frame. You can
    also provide this value with a numeric prefix.

None of these three functions will make a frame smaller than needed to
display all of its windows together with their scroll bars, fringes,
margins, dividers, mode and header lines. This contrasts with requests
by the window manager triggered, for example, by dragging the external
border of a frame with the mouse. Such requests are always honored by
clipping, if necessary, portions that cannot be displayed at the right,
bottom corner of the frame. The parameters `min-width` and
`min-height` (see [Size Parameters](#Size-Parameters)) can be used to obtain a
similar behavior when changing the frame size from within Emacs.

When a frame is in a fullscreen state (see [Size Parameters](#Size-Parameters)),
requests to change the frame size via one of these functions may be
supported or refused either by Emacs itself or by the window manager.
The following variable provides more control of the resulting behavior.

User Option: **alter-fullscreen-frames** [¶](#index-alter_002dfullscreen_002dframes)
:   This options controls how to handle requests to alter fullscreen frames.
    Emacs consults it when asked to resize a fullscreen frame via functions
    like `set-frame-size` or when setting the `width` or
    `height` parameter of a frame. The following values are provided:

    `nil`
    :   This will forward the resize request to the window manager and leave it
        to the latter how to proceed.

    `t`
    :   This will first reset the fullscreen status and then forward the resize
        request on to the window manager.

    `inhibit`
    :   This will reject the resize request and leave the fullscreen status
        unchanged.

    The default is `inhibit` on NS builds and `nil` everywhere
    else.

Function: **set-frame-size-and-position** &optional frame width height left top [¶](#index-set_002dframe_002dsize_002dand_002dposition)
:   This function sets the new size and position of frame in a single
    step; frame defaults to the selected frame. The size and position
    are specified as with the corresponding frame parameters `width`,
    `height`, `left`, and `top` (see [Size Parameters](#Size-Parameters), and
    see [Position Parameters](#Position-Parameters)). If any of these 4 arguments is omitted or
    `nil`, that means not to change the corresponding parameter of
    frame.

Function: **set-frame-size-and-position-pixelwise** frame width height x y &optional gravity [¶](#index-set_002dframe_002dsize_002dand_002dposition_002dpixelwise)
:   This function sets the new size and position of a frame, similarly to
    `set-frame-size-and-position`, but in pixel units. The optional
    argument gravity specifies the new *gravity* of frame
    and must be a value between 0 and 10; it defaults to 1. Gravity is
    window-system dependent and conceptually determines which corner of a
    frame retains its position relative to that of its parent frame when the
    size of that frame or its parent changes. (Top-level frames have no
    parent frame, the window-system root window will be considered their
    parent in this regard.) The value 1 specifies NorthWest gravity and
    means that the top-left corner of the frame should keep its relative
    position. Many window managers seem to ignore gravity.

The abnormal hook `window-size-change-functions` (see [Hooks for Window Scrolling and Changes](#Window-Hooks)) tracks all changes of the inner size of a frame including those
induced by request of the window-system or window manager. To rule out
false positives that might occur when changing only the sizes of a
frame’s windows without actually changing the size of the inner frame,
use the following function.

Function: **frame-size-changed-p** &optional frame [¶](#index-frame_002dsize_002dchanged_002dp)
:   This function returns non-`nil` when the inner width or height of
    frame has changed since `window-size-change-functions` was
    run the last time for frame. It always returns `nil`
    immediately after running `window-size-change-functions` for
    frame.

---

Previous: [Frame Size](#Frame-Size), Up: [Frame Geometry](#Frame-Geometry)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.3.5 Implied Frame Resizing [¶](#Implied-Frame-Resizing-1)

By default, Emacs tries to keep the number of lines and columns of a
frame’s text area (see [Frame Layout](#Frame-Layout)) unaltered when, for example,
toggling the menu or tool bar, changing the default font or setting the
default width of scroll bars on that frame. When any of these
decorations is drawn by a toolkit, this usually means that Emacs has to
work against that toolkit because the latter usually tries to keep the
outer frame size unaltered when the size of a decoration changes, thus
implicitly changing the size of the frame’s text area.

In practice this means that whenever Emacs issues a request to add or
remove such a decoration, it will issue a second request with the
intention to restore the original size of the frame’s text area. When
any of these decorations is drawn by Emacs itself (like the tool bar
with the Lucid or MS-Windows builds or the tab bar), Emacs may still
have to alter the size of the native frame accordingly and issue a
resize request because these decorations should not be accounted for by
the text area of the frame.

Occasionally, such *implied frame resizing* may be unwanted, for
example, when a frame has been maximized or made full-screen (where
it’s turned off by default). In general, users can disable implied
resizing with the following option:

User Option: **frame-inhibit-implied-resize** [¶](#index-frame_002dinhibit_002dimplied_002dresize)
:   If this option is `nil`, changing a frame’s font, menu bar, tool
    bar, internal borders, fringes or scroll bars may resize its outer frame
    in order to keep the number of columns or lines of its text area
    unaltered.

    If this option is `t`, Emacs will not resize a frame in any of
    these cases once it has agreed with the window manager on the final
    initial size of that frame. More precisely, this means that Emacs may
    resize a frame implicitly until all of its decorations have been taken
    into account and it has been given the initial size requested by the
    user. Any further changes of decorations will not cause an implied
    resizing of the frame.

    If this option equals the symbol `force`, Emacs will not perform
    any implied resizing of a frame even before it has agreed with the
    window manager on the final initial size of that frame. As a
    consequence, the initial size of a frame’s text area may not necessarily
    reflect the one specified by the user. This value can be useful with
    tiling window managers where the initial size of a frame is determined
    by external means.

    The value of this option can be also a list of frame parameters. In
    that case, implied resizing of a frame is inhibited for the change of
    any parameters that appears in this list once Emacs has agreed with the
    window manager on the final initial size of that frame. Parameters
    currently handled by this option are `font`, `font-backend`,
    `internal-border-width`, `menu-bar-lines` and
    `tool-bar-lines`.

    Changing any of the `scroll-bar-width`, `scroll-bar-height`,
    `vertical-scroll-bars`, `horizontal-scroll-bars`,
    `left-fringe` and `right-fringe` frame parameters is handled
    as if the frame contained just one live window. This means, for
    example, that removing vertical scroll bars on a frame containing
    several side by side windows will shrink the outer frame width by the
    width of one scroll bar provided this option is `nil` and keep it
    unchanged if this option is `t` or a list containing
    `vertical-scroll-bars`.

    The default value is `(tab-bar-lines tool-bar-lines)` for Lucid,
    Motif and MS-Windows (which means that adding/removing a tool or tab
    bar there does not change the outer frame height),
    `(tab-bar-lines)` on all other window systems including GTK+
    (which means that changing any of the parameters listed above with the
    exception of `tab-bar-lines` may change the size of the outer
    frame), and `t` otherwise (which means the outer frame size never
    changes implicitly when there’s no window system support).

    Note that when a frame is not large enough to accommodate a change of
    any of the parameters listed above, Emacs may try to enlarge the frame
    even if this option is non-`nil`.

    Note also that window managers usually do not ask for resizing a frame
    when they change the number of lines occupied by an external menu or
    tool bar. Typically, such “wrappings” occur when a user shrinks a
    frame horizontally, making it impossible to display all elements of its
    menu or tool bar. They may also result from a change of the major mode
    altering the number of items of a menu or tool bar. Any such wrappings
    may implicitly alter the number of lines of a frame’s text area and are
    unaffected by the setting of this option.

---

Next: [Terminal Parameters](#Terminal-Parameters), Previous: [Frame Geometry](#Frame-Geometry), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
