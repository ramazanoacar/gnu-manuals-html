### 30.9 Splitting and Merging Frames [¶](#Splitting-and-Merging-Frames-1)

The following two functions are useful to transfer parts of the window
layout (see [Basic Concepts of Emacs Windows](#Basic-Windows)) of one frame to two separate frames and
to merge the window layouts of two separate frames into one frame.

Command: **split-frame** &optional frame arg [¶](#index-split_002dframe)
:   This command transfers some windows of the specified frame to a
    new frame and subsequently deletes them on frame. frame
    must be a live frame and defaults to the selected frame. arg
    specifies the number of windows to transfer and defaults to 1.
    Interactively, arg is the prefix argument.

    In a first step, it divides the child windows (see [Windows and Frames](#Windows-and-Frames)) of frame’s main window (see [Side Windows](#Side-Windows)) into two
    parts. The first part includes the first arg child windows if
    arg is positive, and the −arg last child windows if
    arg is negative. The second part includes the remaining child
    windows of frame’s main window.

    In a second step, it clones (see [Window Configurations](#Window-Configurations)) into a newly
    created frame each of the windows of the part which does not include
    frame’s selected window (see [Selecting Windows](#Selecting-Windows)) and removes
    those windows from frame. Note that the original identity
    (see [Basic Concepts of Emacs Windows](#Basic-Windows)) of the cloned windows is lost in this step. The
    identity of the windows retained on frame is preserved, however.
    In a final step, it deletes the windows that have been cloned from
    frame and returns the newly created frame.

    This function signals an error if arg is either zero or not a
    number, or if frame’s main window is either live or does not have
    more child windows than specified by the absolute value of arg.

Command: **merge-frames** &optional frame1 frame2 vertical [¶](#index-merge_002dframes)
:   This command transfers the main window (see [Side Windows](#Side-Windows)) of
    frame2 to frame1. Both frame1 and frame2 must
    specify live frames.

    In the first step it splits the main window of frame1. If
    vertical is non-`nil`, it makes the new window below the old
    main window of frame1. Otherwise, it makes the new window on the
    right of frame1’s main window.

    In the second step it makes the new window a clone (see [Window Configurations](#Window-Configurations)) of the main window of frame2. The original
    identity (see [Basic Concepts of Emacs Windows](#Basic-Windows)) of the cloned windows is lost in that
    step. In the final step, it deletes frame2 if the merge completed
    successfully and returns frame1.

    Interactively, vertical is the prefix argument, frame1 is
    the selected frame and frame2 is the frame following frame1
    in the list of all frames (see [Finding All Frames](#Finding-All-Frames)). If frame1
    and frame2 are omitted or `nil` in calls from Lisp, they
    default to the above values.

In general you cannot “undo” a `split-frame` call with a
subsequent `merge-frames` call for the following reasons: In its
second step `merge-frame` puts the main window of frame2 into
an internal window split off in the first step. No such window was
present in the configuration of frame1 before calling
`split-frame`. Also, after merging is complete, the window cloned
from the main window of frame2 will occupy as much space as the
main window of frame1 before merging. This might not match the
space distribution of the original layout.

Hence, if your preferred workflow is to temporarily split off windows
for working on them in a separate frame and later continue with the
original layout of the original frame, you should proceed as follows:
Save the configuration (see [Window Configurations](#Window-Configurations)) of the original
frame, call `split-frame` to create the separate frame and finally
restore the saved configuration of the original frame and delete the
separate frame.

---

Next: [Input Focus](#Input-Focus), Previous: [Splitting and Merging Frames](#Splitting-and-Merging-Frames), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.10 Minibuffers and Frames [¶](#Minibuffers-and-Frames-1)

Normally, each frame has its own minibuffer window at the bottom, which
is used whenever that frame is selected. You can get that window with
the function `minibuffer-window` (see [Minibuffer Windows](#Minibuffer-Windows)).

However, you can also create a frame without a minibuffer. Such a frame
must use the minibuffer window of some other frame. That other frame
will serve as *surrogate minibuffer frame* for this frame and cannot
be deleted via `delete-frame` (see [Deleting Frames](#Deleting-Frames)) as long as
this frame is live.

When you create the frame, you can explicitly specify its minibuffer
window (in some other frame) with the `minibuffer` frame parameter
(see [Buffer Parameters](#Buffer-Parameters)). If you don’t, then the minibuffer is found
in the frame which is the value of the variable
`default-minibuffer-frame`. Its value should be a frame that does
have a minibuffer.

If you use a minibuffer-only frame, you might want that frame to raise
when you enter the minibuffer. If so, set the variable
`minibuffer-auto-raise` to `t`. See [Raising, Lowering and Restacking Frames](#Raising-and-Lowering).

Variable: **default-minibuffer-frame** [¶](#index-default_002dminibuffer_002dframe)
:   This variable specifies the frame to use for the minibuffer window, by
    default. It does not affect existing frames. It is always local to
    the current terminal and cannot be buffer-local. See [Multiple Terminals](#Multiple-Terminals).

---

Next: [Visibility of Frames](#Visibility-of-Frames), Previous: [Minibuffers and Frames](#Minibuffers-and-Frames), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.11 Input Focus [¶](#Input-Focus-1)

At any time, one frame in Emacs is the *selected frame*. The
selected window (see [Selecting Windows](#Selecting-Windows)) always resides on the
selected frame.

When Emacs displays its frames on several terminals (see [Multiple Terminals](#Multiple-Terminals)), each terminal has its own selected frame. But only one
of these is *the* selected frame: it’s the frame that belongs
to the terminal from which the most recent input came. That is, when
Emacs runs a command that came from a certain terminal, the selected
frame is the one of that terminal. Since Emacs runs only a single
command at any given time, it needs to consider only one selected
frame at a time; this frame is what we call *the selected frame*
in this manual. The display on which the selected frame is shown is
the *selected frame’s display*.

Function: **selected-frame** [¶](#index-selected_002dframe)
:   This function returns the selected frame.

Some window systems and window managers direct keyboard input to the
window object that the mouse is in; others require explicit clicks or
commands to *shift the focus* to various window objects. Either
way, Emacs automatically keeps track of which frames have focus. To
explicitly switch to a different frame from a Lisp function, call
`select-frame-set-input-focus`.

The plural “frames” in the previous paragraph is deliberate: while
Emacs itself has only one selected frame, Emacs can have frames on
many different terminals (recall that a connection to a window system
counts as a terminal), and each terminal has its own idea of which
frame has input focus. Under the X Window System, where user input is
organized into individual “seats” of input, each seat in turn can
have its own specific input focus. When you set the input focus to a
frame, you set the focus for that frame’s terminal on the last seat
which interacted with Emacs, but frames on other terminals and seats
may still remain focused.

If the input focus is set before any user interaction has occurred on
the specified terminal, then the X server picks a random seat
(normally the one with the lowest number) and sets the input focus
there.

Lisp programs can switch frames temporarily by calling the function
`select-frame`. This does not alter the window system’s concept
of focus; rather, it escapes from the window manager’s control until
that control is somehow reasserted.

When using a text terminal, only one frame can be displayed at a time
on the terminal, so after a call to `select-frame`, the next
redisplay actually displays the newly selected frame. This frame
remains selected until a subsequent call to `select-frame`. Each
frame on a text terminal has a number which appears in the mode line
before the buffer name (see [Variables Used in the Mode Line](#Mode-Line-Variables)).

Function: **select-frame-set-input-focus** frame &optional norecord [¶](#index-select_002dframe_002dset_002dinput_002dfocus)
:   This function selects frame, raises it (should it happen to be
    obscured by other frames) and tries to give it the window system’s
    focus. On a text terminal, the next redisplay displays the new frame
    on the entire terminal screen. The optional argument norecord
    has the same meaning as for `select-frame` (see below).
    The return value of this function is not significant.

Ideally, the function described next should focus a frame without also
raising it above other frames. Unfortunately, many window-systems or
window managers may refuse to comply.

Function: **x-focus-frame** frame &optional noactivate [¶](#index-x_002dfocus_002dframe)
:   This function gives frame the focus of the X server without
    necessarily raising it. frame `nil` means use the selected
    frame. Under X, the optional argument noactivate, if
    non-`nil`, means to avoid making frame’s window-system window
    the “active” window which should insist a bit more on avoiding to
    raise frame above other frames.

    On MS-Windows the noactivate argument has no effect. However, if
    frame is a child frame (see [Child Frames](#Child-Frames)), this function
    usually focuses frame without raising it above other child
    frames.

    If there is no window system support, this function does nothing.

Command: **select-frame** frame &optional norecord [¶](#index-select_002dframe)
:   This function selects frame frame, temporarily disregarding the
    focus of the X server if any. The selection of frame lasts until
    the next time the user does something to select a different frame, or
    until the next time this function is called. (If you are using a
    window system, the previously selected frame may be restored as the
    selected frame after return to the command loop, because it still may
    have the window system’s input focus.)

    The specified frame becomes the selected frame, and its terminal
    becomes the selected terminal. This function then calls
    `select-window` as a subroutine, passing the window selected
    within frame as its first argument and norecord as its
    second argument (hence, if norecord is non-`nil`, this
    avoids changing the order of recently selected windows and the buffer
    list). See [Selecting Windows](#Selecting-Windows).

    This function returns frame, or `nil` if frame has
    been deleted.

    In general, you should never use `select-frame` in a way that
    could switch to a different terminal without switching back when
    you’re done.

Function: **frame-use-time** &optional frame [¶](#index-frame_002duse_002dtime)
:   This function returns the use time of frame frame. frame
    must be a live frame and defaults to the selected one.

    The *use time of a frame* is the highest use time as reported by
    `window-use-time` of any window on frame.

Function: **get-mru-frame** &optional all-frames exclude-child-frames exclude-frame [¶](#index-get_002dmru_002dframe)
:   This function returns the frame with the highest use time as reported by
    `frame-use-time`. It returns `nil` if no candidate frames are
    found which usually happens if frames are excluded with the help of the
    optional arguments.

    By default, tooltip and minibuffer-only frames are never candidates. If
    the optional argument exclude-child-frames is non-nil, child
    frames are excluded too. The exclude-frame argument, if present,
    excludes the frame it specifies too. Since in practice the most
    recently used frame is always the selected one, it usually makes sense
    to call this function with a non-`nil` exclude-frame argument
    specifying the selected frame.

    The optional argument all-frames specifies which frames to
    consider:

    * `visible`
      means to consider all visible frames on the current terminal or
      exclude-frame’s terminal.
    * 0
      means to consider all visible or iconified frames on the current
      terminal or exclude-frame’s terminal.
    * Anything else
      means to consider all frames.

Function: **get-mru-frames** &optional all-frames exclude-child-frames exclude-frame [¶](#index-get_002dmru_002dframes)
:   This function returns a list of frames sorted by highest use time as
    reported by `frame-use-time` which is computed using each frame’s
    most recently used window.

    By default, tooltip and minibuffer-only frames are never candidates. If
    the optional argument exclude-child-frames is non-nil, child
    frames are excluded too. The exclude-frame argument, if present,
    excludes the frame it specifies too.

    It can return `nil` which can happen if frames are excluded with
    the help of the optional arguments, for example, if there is a single
    frame and exclude-frame is the selected frame.

    The optional argument all-frames specifies which frames to
    consider:

    * `visible`
      means to consider all visible frames on the current terminal or
      exclude-frame’s terminal.
    * 0
      means to consider all visible or iconified frames on the current
      terminal or exclude-frame’s terminal.
    * Anything else
      means to consider all frames.

Command: **select-frame-by-id** id &optional noerror [¶](#index-select_002dframe_002dby_002did)
:   This function searches open and undeletable frames for a matching frame
    identifier id (see [Frames](#Frames)). If found, its frame is undeleted,
    if necessary, then raised, given focus, and made the selected frame. On
    a text terminal, raising a frame causes it to occupy the entire terminal
    display.

    This function returns the selected frame or signals an error if id
    is not found, unless noerror is non-`nil`, in which case it
    returns `nil`.

Command: **undelete-frame-by-id** id &optional noerror [¶](#index-undelete_002dframe_002dby_002did)
:   This function searches undeletable frames for a matching frame
    identifier id (see [Frames](#Frames)). If found, its frame is undeleted,
    raised, given focus, and made the selected frame. On a text terminal,
    raising a frame causes it to occupy the entire terminal display.

    This function returns the undeleted frame or signals an error if
    id is not found, unless noerror is non-`nil`, in which
    case it returns `nil`.

Emacs cooperates with the window system by arranging to select frames
as the server and window manager request. When a window system
informs Emacs that one of its frames has been selected, Emacs
internally generates a *focus-in* event. When an Emacs frame is
displayed on a text-terminal emulator, such as `xterm`, which
supports reporting of focus-change notification, the focus-in and
focus-out events are available even for text-mode frames. Focus
events are normally handled by `handle-focus-in`.

Command: **handle-focus-in** event [¶](#index-handle_002dfocus_002din)
:   This function handles focus-in events from window systems and
    terminals that support explicit focus notifications. It updates the
    per-frame focus flags that `frame-focus-state` queries and calls
    `after-focus-change-function`. In addition, it generates a
    `switch-frame` event in order to switch the Emacs notion of the
    selected frame to the frame most recently focused in some terminal.
    It’s important to note that this switching of the Emacs selected frame
    to the most recently focused frame does not mean that other frames do
    not continue to have the focus in their respective terminals. Do not
    invoke this function yourself: instead, attach logic to
    `after-focus-change-function`.

Command: **handle-switch-frame** frame [¶](#index-handle_002dswitch_002dframe)
:   This function handles a switch-frame event, which Emacs generates for
    itself upon focus notification or under various other circumstances
    involving an input event arriving at a different frame from the last
    event. Do not invoke this function yourself.

Function: **redirect-frame-focus** frame &optional focus-frame [¶](#index-redirect_002dframe_002dfocus)
:   This function redirects focus from frame to focus-frame.
    This means that focus-frame will receive subsequent keystrokes and
    events intended for frame. After such an event, the value of
    `last-event-frame` will be focus-frame. Also, switch-frame
    events specifying frame will instead select focus-frame.

    If focus-frame is omitted or `nil`, that cancels any existing
    redirection for frame, which therefore once again receives its own
    events.

    One use of focus redirection is for frames that don’t have minibuffers.
    These frames use minibuffers on other frames. Activating a minibuffer
    on another frame redirects focus to that frame. This puts the focus on
    the minibuffer’s frame, where it belongs, even though the mouse remains
    in the frame that activated the minibuffer.

    Selecting a frame can also change focus redirections. Selecting frame
    `bar`, when `foo` had been selected, changes any redirections
    pointing to `foo` so that they point to `bar` instead. This
    allows focus redirection to work properly when the user switches from
    one frame to another using `select-window`.

    This means that a frame whose focus is redirected to itself is treated
    differently from a frame whose focus is not redirected.
    `select-frame` affects the former but not the latter.

    The redirection lasts until `redirect-frame-focus` is called to
    change it.

Function: **frame-focus-state** &optional frame [¶](#index-frame_002dfocus_002dstate)
:   This function retrieves the last known focus state of frame.
    frame defaults to the selected frame.

    It returns `nil` if the frame is known not to be focused,
    `t` if the frame is known to be focused, or `unknown` if
    Emacs does not know the focus state of the frame. (You may see this
    last state in TTY frames running on terminals that do not support
    explicit focus notifications.)

Variable: **after-focus-change-function** [¶](#index-after_002dfocus_002dchange_002dfunction)
:   This function is called with no arguments when Emacs notices that a
    frame may have gotten or lost focus. Focus events are delivered
    asynchronously, and may not be delivered in the expected order, so
    code that wants to do something depending on the state of focused
    frames have go through all the frames and check.

    For instance, here’s a simple example function that sets the
    background color based on whether the frame has focus or not:

    ```
    (add-function :after after-focus-change-function
                  #'my-change-background)
    (defun my-change-background ()
      (dolist (frame (frame-list))
        (pcase (frame-focus-state frame)
          (`t (set-face-background 'default "black" frame))
          (`nil (set-face-background 'default "#404040" frame)))))
    ```

    Multiple frames may appear to have input focus simultaneously due to
    focus event delivery differences, the presence of multiple Emacs
    terminals, and other factors, and code should be robust in the face of
    this situation.

    Depending on window system, focus events may also be delivered
    repeatedly and with different focus states before settling to the
    expected values. Code relying on focus notifications should
    “debounce” any user-visible updates arising from focus changes,
    perhaps by deferring work until redisplay.

    This function may be called in arbitrary contexts, including from
    inside `read-event`, so take the same care as you might when
    writing a process filter.

User Option: **focus-follows-mouse** [¶](#index-focus_002dfollows_002dmouse)
:   This option informs Emacs whether and how the window manager transfers
    focus when you move the mouse pointer into a frame. It can have three
    meaningful values:

    `nil`
    :   The default value `nil` should be used when your window manager
        follows a “click-to-focus” policy where you have to click the mouse
        inside of a frame in order for that frame to gain focus.

    `t`
    :   The value `t` should be used when your window manager has the focus
        automatically follow the position of the mouse pointer but a frame that
        gains focus is not raised automatically and may even remain occluded by
        other window-system windows.

    `auto-raise`
    :   The value `auto-raise` should be used when your window manager has
        the focus automatically follow the position of the mouse pointer and a
        frame that gains focus is raised automatically.

    If this option is non-`nil`, Emacs moves the mouse pointer to the
    frame selected by `select-frame-set-input-focus`. That function is
    used by a number of commands like, for example, `other-frame` and
    `pop-to-buffer`.

    The distinction between the values `t` and `auto-raise` is not
    needed for “normal” frames because the window manager usually takes
    care of raising them. It is useful to automatically raise child frames
    via `mouse-autoselect-window` (see [Mouse Window Auto-selection](#Mouse-Window-Auto_002dselection)).

    Note that this option does not distinguish “sloppy” focus (where the
    frame that previously had focus retains focus as long as the mouse
    pointer does not move into another window-system window) from “strict”
    focus (where a frame immediately loses focus when it’s left by the mouse
    pointer). Neither does it recognize whether your window manager
    supports delayed focusing or auto-raising where you can explicitly
    specify the time until a new frame gets focus or is auto-raised.

    You can supply a “focus follows mouse” policy for individual Emacs
    windows by customizing the variable `mouse-autoselect-window`
    (see [Mouse Window Auto-selection](#Mouse-Window-Auto_002dselection)).

---

Next: [Raising, Lowering and Restacking Frames](#Raising-and-Lowering), Previous: [Input Focus](#Input-Focus), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.12 Visibility of Frames [¶](#Visibility-of-Frames-1)

A frame on a graphical display may be *visible*, *invisible*, or
*iconified*. If it is visible, its contents are displayed in the
usual manner. If it is iconified, its contents are not displayed, but
there is a little icon somewhere to bring the frame back into view (some
window managers refer to this state as *minimized* rather than
*iconified*, but from Emacs’s point of view they are the same thing).
If a frame is invisible, it is not displayed at all.

On a text terminal a frame may be only visible or invisible. The top
frame (see [Frames](#Frames)) of a terminal cannot be invisible.

On graphical displays the concept of visibility is strongly related to
that of (un-)mapped frames. A frame (or, more precisely, its
window-system window) is and becomes *mapped* when it is displayed
for the first time and whenever it changes its state of visibility from
`iconified` or `invisible` to `visible`. Conversely, a
frame is and becomes *unmapped* whenever it changes its status from
`visible` to `iconified` or `invisible`.

Function: **frame-visible-p** frame [¶](#index-frame_002dvisible_002dp)
:   This function returns the visibility status of frame frame. The
    value is `t` if frame is visible, `nil` if it is
    invisible, and `icon` if it is iconified.

    Note that the visibility status of a frame as reported by this function
    (and by the `visibility` frame parameter, see [Frame Interaction Parameters](#Frame-Interaction-Parameters)) does not necessarily tell whether the frame is actually
    seen on display. Any such frame can be partially or completely obscured
    by other window manager windows on the same graphical terminal. Whether
    that completely hides the frame may then depend on the transparency of
    the obscuring window. A frame may also reside on a virtual desktop
    different from the current one and can be seen only when making that
    desktop the current one. One notable restriction holds for child frames
    (see [Child Frames](#Child-Frames)): A child frame can be seen if and only if this
    function returns true for all its ancestors including the frame itself
    and its root frame.

    On a text terminal only that terminal’s top frame and its child frames
    can be actually seen. Other root frames and their child frames cannot
    be seen even if they are considered visible by this function.

Command: **iconify-frame** &optional frame [¶](#index-iconify_002dframe)
:   This function iconifies frame frame. If you omit frame, it
    iconifies the selected frame. This function also removes any child
    frames (see [Child Frames](#Child-Frames)) of frame and their descendants from
    display. If frame is a child frame itself, the behavior depends
    on the value of the variable `iconify-child-frame`. If frame
    is the top frame of a text terminal (see [Frames](#Frames)), this function has
    no effect.

Command: **make-frame-visible** &optional frame [¶](#index-make_002dframe_002dvisible)
:   This function makes frame frame visible. If you omit frame,
    it makes the selected frame visible. This does not raise the frame, but
    you can do that with `raise-frame` if you wish (see [Raising, Lowering and Restacking Frames](#Raising-and-Lowering)).

    Making a frame visible makes all its child frames with visible ancestors
    appear on display again (see [Child Frames](#Child-Frames)).

Command: **make-frame-invisible** &optional frame force [¶](#index-make_002dframe_002dinvisible)
:   This function makes frame frame invisible. If you omit
    frame, it makes the selected frame invisible. Usually, this makes
    all child frames of frame (and their descendants) invisible too
    (see [Child Frames](#Child-Frames)).

    Unless force is non-`nil`, this function refuses to make
    frame invisible if all other frames are invisible. On a text
    terminal this will make frame invisible if and only if it is a
    child frame (see [Child Frames](#Child-Frames)). In this case, if frame is
    selected, it will select the first visible ancestor of frame
    instead. In addition, it will remove all child frames with frame
    as their ancestor from display.

The visibility status of a frame is also available as a frame
parameter. You can read or change it as such. See [Frame Interaction Parameters](#Frame-Interaction-Parameters). The user can also iconify and deiconify frames with the
window manager. This happens below the level at which Emacs can exert
any control, but Emacs does provide events that you can use to keep
track of such changes. See [Miscellaneous System Events](#Misc-Events).

Function: **x-double-buffered-p** &optional frame [¶](#index-x_002ddouble_002dbuffered_002dp)
:   This function returns non-`nil` if frame is currently
    being rendered with double buffering. frame defaults to the
    selected frame.

---

Next: [Frame Configurations](#Frame-Configurations), Previous: [Visibility of Frames](#Visibility-of-Frames), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.13 Raising, Lowering and Restacking Frames [¶](#Raising_002c-Lowering-and-Restacking-Frames)

Most window systems use a desktop metaphor. Part of this metaphor is
the idea that system-level windows (representing, e.g., Emacs frames)
are stacked in a notional third dimension perpendicular to the screen
surface. The order induced by stacking is total and usually referred to
as stacking (or Z-) order. Where the areas of two windows overlap, the
one higher up in that order will (partially) cover the one underneath.

You can *raise* a frame to the top of that order or *lower* a
frame to its bottom by using the functions `raise-frame` and
`lower-frame`. You can *restack* a frame directly above or
below another frame using the function `frame-restack`.

Note that all functions described below will respect the adherence of
frames (and all other window-system windows) to their respective z-group
(see [Position Parameters](#Position-Parameters)). For example, you usually cannot lower a
frame below that of the desktop window and you cannot raise a frame
whose `z-group` parameter is `nil` above the window-system’s
taskbar or tooltip window.

Command: **raise-frame** &optional frame [¶](#index-raise_002dframe)
:   This function raises frame frame (default, the selected frame)
    above all other frames belonging to the same or a lower z-group as
    frame. If frame is invisible or iconified, this makes it
    visible. If frame is a child frame (see [Child Frames](#Child-Frames)), this
    raises frame above all other child frames of its parent. For
    non-child frames on a text terminal this function has no effect.

Command: **lower-frame** &optional frame [¶](#index-lower_002dframe)
:   This function lowers frame frame (default, the selected frame)
    below all other frames belonging to the same or a higher z-group as
    frame. If frame is a child frame (see [Child Frames](#Child-Frames)),
    this lowers frame below all other child frames of its parent. For
    non-child frames on a text terminal this function has no effect.

Function: **frame-restack** frame1 frame2 &optional above [¶](#index-frame_002drestack)
:   This function restacks frame1 below frame2. This implies
    that if both frames are visible and their display areas overlap,
    frame2 will (partially) obscure frame1. If the optional
    third argument above is non-`nil`, this function restacks
    frame1 above frame2. This means that if both frames are
    visible and their display areas overlap, frame1 will (partially)
    obscure frame2.

    Technically, this function may be thought of as an atomic action
    performed in two steps: The first step removes frame1’s
    window-system window from the display. The second step reinserts
    frame1’s window into the display below (above if above is
    true) that of frame2. Hence the position of frame2 in its
    display’s Z (stacking) order relative to all other frames excluding
    frame1 remains unaltered.

    Some window managers may refuse to restack windows. This function has
    not been implemented on text terminals yet.

Note that the effect of restacking will only hold as long as neither of
the involved frames is iconified or made invisible. You can use the
`z-group` (see [Position Parameters](#Position-Parameters)) frame parameter to add a
frame to a group of frames permanently shown above or below other
frames. As long as a frame belongs to one of these groups, restacking
it will only affect its relative stacking position within that group.
The effect of restacking frames belonging to different z-groups is
undefined. You can list frames in their current stacking order with the
function `frame-list-z-order` (see [Finding All Frames](#Finding-All-Frames)).

User Option: **minibuffer-auto-raise** [¶](#index-minibuffer_002dauto_002draise)
:   If this is non-`nil`, activation of the minibuffer raises the frame
    that the minibuffer window is in. This variable has no effect on text
    terminals.

On window systems, you can also enable auto-raising (on frame
selection) or auto-lowering (on frame deselection) using frame
parameters. See [Window Management Parameters](#Management-Parameters).

---

Next: [Child Frames](#Child-Frames), Previous: [Raising, Lowering and Restacking Frames](#Raising-and-Lowering), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.14 Frame Configurations [¶](#Frame-Configurations-1)

A *frame configuration* records the current arrangement of frames,
all their properties, and the window configuration of each one.
(See [Window Configurations](#Window-Configurations).)

Function: **current-frame-configuration** [¶](#index-current_002dframe_002dconfiguration)
:   This function returns a frame configuration list that describes
    the current arrangement of frames and their contents.

Function: **set-frame-configuration** configuration &optional nodelete [¶](#index-set_002dframe_002dconfiguration)
:   This function restores the state of frames described in
    configuration. However, this function does not restore deleted
    frames.

    Ordinarily, this function deletes all existing frames not listed in
    configuration. But if nodelete is non-`nil`, the
    unwanted frames are iconified instead.

---

Next: [Mouse Tracking](#Mouse-Tracking), Previous: [Frame Configurations](#Frame-Configurations), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.15 Child Frames [¶](#Child-Frames-1)

Child frames are objects halfway between windows (see [Windows](#Windows)) and
“normal” frames. Like windows, they are attached to an owning frame.
Unlike windows, they may overlap each other—changing the size or
position of one child frame does not change the size or position of any
of its sibling child frames.

By design, operations to make or modify child frames are implemented
with the help of frame parameters (see [Frame Parameters](#Frame-Parameters)) without any
specialized functions or customizable variables. Child frames
are meaningful on graphical and text terminals.

* [Child Frame Operations](#Child-Frame-Operations)
* [Child Frame Properties](#Child-Frame-Properties)
* [Child Frame Peculiarities](#Child-Frame-Peculiarities)

---

Next: [Child Frame Properties](#Child-Frame-Properties), Up: [Child Frames](#Child-Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.15.1 Child Frame Operations [¶](#Child-Frame-Operations-1)

To create a new child frame or to convert a normal frame into a child
frame, set that frame’s `parent-frame` parameter (see [Frame Interaction Parameters](#Frame-Interaction-Parameters)) to that of an already existing frame. The
frame specified by that parameter will then be the frame’s parent frame
as long as the parameter is not changed or reset. Technically, on a GUI
this makes the child frame’s window-system window a child window of the
parent frame’s window-system window. On a text terminal, this makes the
frame usually appear on the same terminal as its parent frame, obscuring
some part of it.

The `parent-frame` parameter can be changed at any time. Setting
it to another frame *reparents* the child frame. Setting it to
another child frame makes the frame a *nested* child frame. Setting
it to `nil` restores the frame’s status as a top-level frame—a
frame whose window-system window is a child of its display’s root
window.[25](#FOOT25) On text terminals,
top-level frames are called root frames (see below).

Since child frames can be arbitrarily nested, a frame can be both a
child and a parent frame. Also, the relative roles of child and parent
frame may be reversed at any time (though it’s usually a good idea to
keep the size of a child frame sufficiently smaller than that of its
parent). An error will be signaled for the attempt to make a frame an
ancestor of itself.

When a parent frame is about to be deleted (see [Deleting Frames](#Deleting-Frames)),
its child frames are recursively deleted before it. There is one
exception to this rule: When the child frame serves as a surrogate
minibuffer frame (see [Minibuffers and Frames](#Minibuffers-and-Frames)) for another frame, it
is retained until the parent frame has been deleted. If, at this time,
no remaining frame uses the child frame as its minibuffer frame, Emacs
will try to delete the child frame too. If that deletion fails for
whatever reason, the child frame is made a top-level frame. Since on
text terminals no such conversion is possible, deleting a frame may
throw an error if a surrogate minibuffer frame to be deleted is used by
a frame that will not be deleted too.

The following three functions help to understand how parent and child
frames related to each other.

Function: **frame-parent** &optional frame [¶](#index-frame_002dparent)
:   This function returns the parent frame of frame. It returns
    `nil` if frame has no parent frame.

Function: **frame-ancestor-p** ancestor descendant [¶](#index-frame_002dancestor_002dp)
:   This functions returns non-`nil` if ancestor is an ancestor
    of descendant. ancestor is an ancestor of descendant
    when it is either descendant’s parent frame or it is an ancestor
    of descendant’s parent frame. Both, ancestor and
    descendant must specify live frames.

Function: **frame-root-frame** &optional frame [¶](#index-frame_002droot_002dframe)
:   This function returns the root frame of the specified frame.
    frame must be a live frame and defaults to the selected one. The
    root frame of frame is the frame obtained by following the chain
    of parent frames starting with frame until a frame is reached that
    has no parent. If frame has no parent, its root frame is
    frame itself.

On a text terminal, a root frame is always positioned at the top left
edge of its terminal and always occupies the full size of its terminal.

---

Next: [Child Frame Peculiarities](#Child-Frame-Peculiarities), Previous: [Child Frame Operations](#Child-Frame-Operations), Up: [Child Frames](#Child-Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.15.2 Child Frame Properties [¶](#Child-Frame-Properties-1)

Most window-systems clip child frames at the native edges (see [Frame Geometry](#Frame-Geometry)) of their parent frame—everything outside these edges is
usually invisible. A child frame’s `left` and `top`
parameters specify a position relative to the top-left corner of its
parent’s native frame. When the parent frame is resized, this position
remains conceptually unaltered.

NS builds and text terminals do not clip child frames at the parent
frame’s edges, allowing them to be positioned so they do not obscure the
parent frame while still being visible themselves.

Note also the function `window-largest-empty-rectangle`
(see [Coordinates and Windows](#Coordinates-and-Windows)) which can be used to inscribe a child
frame in the largest empty area of an existing window. This can be
useful to avoid that a child frame obscures any text shown in that
window.

Usually, moving a parent frame moves along all its child frames and
their descendants as well, keeping their relative positions unaltered.
Note that the hook `move-frame-functions` (see [Frame Position](#Frame-Position))
is run for a child frame only when the position of the child frame
relative to its parent frame changes.

When a parent frame is resized, its child frames conceptually retain
their previous sizes and their positions relative to the left upper
corner of the parent. This means that a child frame may become
(partially) invisible when its parent frame shrinks. The parameter
`keep-ratio` (see [Frame Interaction Parameters](#Frame-Interaction-Parameters)) can be used to
resize and reposition a child frame proportionally whenever its parent
frame is resized. This may avoid obscuring parts of a frame when its
parent frame is shrunk.

A visible child frame always appears on top of its parent frame thus
obscuring parts of it, except on NS builds where it may be positioned
beneath the parent. This is comparable to the window-system window of a
top-level frame which also always appears on top of its parent
window—the desktop’s root window. When a parent frame is iconified or
made invisible (see [Visibility of Frames](#Visibility-of-Frames)), any child frames
descending from it will not be shown either even if
`frame-visible-p` returns `t` for them. When a parent frame
is deiconified or made visible, any child frames descending from it will
be shown again (provided they and all their ancestor frames are visible
too). If a child frame is used as surrogate minibuffer frame
(see [Minibuffers and Frames](#Minibuffers-and-Frames)), it’s up to the application to
guarantee the frame’s visibility whenever the minibuffer is activated.

Whether a child frame can have a menu or tool bar is window-system or
window manager dependent. Most window-systems explicitly disallow menu
bars for child frames. It seems advisable to disable both, menu and
tool bars, via the frame’s initial parameters settings. On a text
terminal, child frames use the menu bar of their root frame (provided it
has one).

Usually, child frames do not exhibit window manager decorations like a
title bar or external borders (see [Frame Geometry](#Frame-Geometry)). When the child
frame does not show a menu or tool bar, any other of the frame’s borders
(see [Layout Parameters](#Layout-Parameters)) can be used instead of the external borders.

In particular, under X (but not when building with GTK+), the frame’s
outer border can be used. On MS-Windows, specifying a non-zero outer
border width will show a one-pixel wide external border. Under all
window-systems, the internal border can be used. In either case, it’s
advisable to disable a child frame’s window manager decorations with the
`undecorated` frame parameter (see [Window Management Parameters](#Management-Parameters)). On
a text terminal, on the other hand, it’s better to leave that parameter
alone so your child frame will be drawn with an outer border.

To resize or move a border-less child frame with the mouse, special
frame parameters (see [Mouse Dragging Parameters](#Mouse-Dragging-Parameters)) have to be used.
The internal border of a child frame, if present, can be used to resize
the frame with the mouse, provided that frame has a non-`nil`
`drag-internal-border` parameter. If set, the `snap-width`
parameter indicates the number of pixels where the frame *snaps* at
the respective edge or corner of its parent frame. On a text terminal,
the outer border can used for resizing.

There are three ways to drag an entire child frame with the mouse: The
`drag-with-mode-line` parameter, if non-`nil`, enables
dragging a frame without minibuffer window (see [Minibuffer Windows](#Minibuffer-Windows))
via the mode line area of its bottommost window. The
`drag-with-header-line` and the `drag-with-tab-line`
parameters, if non-`nil`, enable dragging the frame via the header
or tab line area of its topmost window (dragging with the tab line is
possible only when there’s no header line above the tab line).

In order to give a child frame a draggable header or mode line, the
window parameters `mode-line-format` and `header-line-format`
are handy (see [Window Parameters](#Window-Parameters)). These allow removing an
unwanted mode line (when `drag-with-header-line` is chosen) and to
remove mouse-sensitive areas which might interfere with frame dragging.

When the user drags a frame with a mouse and overshoots, it’s easy
to drag a frame out of the screen area of its parent. Retrieving such
a frame can be hairy once the mouse button has been released. To
prevent such a situation, it is advisable to set the frame’s
`top-visible` or `bottom-visible` parameter (see [Mouse Dragging Parameters](#Mouse-Dragging-Parameters)).

Set the `top-visible` parameter of a child frame to a number when
you intend to allow dragging that frame by its header or tab
line. Setting `top-visible` to a number inhibits dragging the top
edge of the child frame above the top edge of its parent. Set the
`bottom-visible` parameter to a number when you intend to drag that
frame via its mode line; this inhibits dragging the bottom edge of the
child frame beneath the bottom edge of its parent. In either case, that
number also specifies width and height (in pixels) of the area of the
child frame that remains visible during dragging.

When a child frame is used for displaying a buffer via
`display-buffer-in-child-frame` (see [Action Functions for Buffer Display](#Buffer-Display-Action-Functions)), the frame’s `auto-hide-function` parameter
(see [Frame Interaction Parameters](#Frame-Interaction-Parameters)) can be set to a function, in
order to appropriately deal with the frame when the window displaying
the buffer shall be quit.

When a child frame is used during minibuffer interaction, for example,
to display completions in a separate window, the `minibuffer-exit`
parameter (see [Frame Interaction Parameters](#Frame-Interaction-Parameters)) is useful in order to
deal with the frame when the minibuffer is exited.

---

Previous: [Child Frame Properties](#Child-Frame-Properties), Up: [Child Frames](#Child-Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.15.3 Child Frame Peculiarities [¶](#Child-Frame-Peculiarities-1)

The behavior of child frames deviates from that of normal frames in a
number of peculiar ways. Here we sketch a few of them:

* The semantics of maximizing and iconifying child frames is highly
  window-system dependent. As a rule, applications should never invoke
  these operations on child frames. By default, invoking
  `iconify-frame` on a child frame will try to iconify the top-level
  frame corresponding to that child frame instead. To obtain a different
  behavior, users may customize the option `iconify-child-frame`
  described below.
* Raising, lowering and restacking child frames (see [Raising, Lowering and Restacking Frames](#Raising-and-Lowering)) or changing the `z-group` (see [Position Parameters](#Position-Parameters))
  of a child frame changes only the stacking order of child frames with
  the same parent. Restacking has not been implemented on text terminals.
* Many window-systems are not able to change the opacity (see [Font and Color Parameters](#Font-and-Color-Parameters)) of child frames.
* Transferring focus from a child frame to an ancestor that is not its
  parent by clicking with the mouse in a visible part of that ancestor’s
  window may fail with some window-systems. You may have to click into
  the direct parent’s window-system window first.
* Window managers might not bother to extend their focus follows mouse
  policy to child frames. Customizing `mouse-autoselect-window` can
  help in this regard (see [Mouse Window Auto-selection](#Mouse-Window-Auto_002dselection)).
* Dropping (see [Drag and Drop](#Drag-and-Drop)) on child frames is not guaranteed to
  work on all window-systems. Some will drop the object on the parent
  frame or on some ancestor instead.

Customizing the following option can be useful to tweak the behavior of
`iconify-frame` for child frames.

User Option: **iconify-child-frame** [¶](#index-iconify_002dchild_002dframe)
:   This option tells Emacs how to proceed when it is asked to iconify a
    child frame. If it is `nil`, `iconify-frame` will do nothing
    when invoked on a child frame. If it is `iconify-top-level`, Emacs
    will try to iconify the root frame of this child frame instead. If it
    is `make-invisible`, Emacs will try to make this child frame
    invisible instead of iconifying it.

    Any other value means to try iconifying the child frame. Since such an
    attempt may not be honored by all window managers and can even lead to
    making the child frame unresponsive to user actions, the default is to
    iconify the root frame instead.

    On a text terminal the only feasible values are `nil` and
    `make-invisible`.

On text terminals exist a few restrictions with respect to reparenting:
One is that a top frame (see [Frames](#Frames)) cannot be directly made a child
frame—you first have to make another root frame the new top frame of
its terminal. If, on the other hand, you want a child frame to become
the new top frame of its terminal, you have to make it a root frame
first.

Also, the surrogate minibuffer window of any frame on a text terminal
must reside on a frame with the same root frame. Reparenting will throw
an error whenever it violates this restriction. It also means that it’s
more tricky to make a minibuffer-less frame whose minibuffer window
resides on a minibuffer-only child frame. On a GUI, Emacs proceeds as
follows when a user has specified the value `child-frame` for the
`minibuffer` parameter in `initial-frame-alist`
(see [Initial Frame Parameters](#Initial-Parameters)):

1. Create a minibuffer-only frame.
2. Create a minibuffer-less frame with its `minibuffer` parameter set
   to the window of the minibuffer-only frame.
3. Make the minibuffer-less frame the parent frame of the minibuffer-only
   frame.
4. Delete the originally selected frame.

On a text terminal you have to perform these operations manually as
sketched in the following snippet:

```
(let* ((selected (selected-frame))
       (mini-only
        (make-frame
         `((parent-frame . ,selected)
           (minibuffer . only)
           (left . 1) (top . -1) (width . 20) (height . 1))))
       (mini-less
        (make-frame
         (append `((parent-frame . ,selected)
                   (minibuffer . ,(minibuffer-window mini-only)))))))
  (set-frame-parameter mini-only 'parent-frame mini-less)
  (set-frame-parameter mini-less 'parent-frame nil)
  (select-frame mini-less)
  (delete-frame selected))
```

This means that you first have to install the minibuffer-less and the
minibuffer-only frames both as child frames of the selected frame with
the `minibuffer` parameter of the minibuffer-less frame set to the
minibuffer window of the minibuffer-only frame. Then make the
minibuffer-only frame a child frame of the minibuffer-less frame and
make the minibuffer-less frame a new root frame. Finally, select the
minibuffer-less frame and delete the originally selected frame.

---

Next: [Mouse Position](#Mouse-Position), Previous: [Child Frames](#Child-Frames), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.16 Mouse Tracking [¶](#Mouse-Tracking-1)

Sometimes it is useful to *track* the mouse, which means to display
something to indicate where the mouse is and move the indicator as the
mouse moves. For efficient mouse tracking, you need a way to wait until
the mouse actually moves.

The convenient way to track the mouse is to ask for events to represent
mouse motion. Then you can wait for motion by waiting for an event. In
addition, you can easily handle any other sorts of events that may
occur. That is useful, because normally you don’t want to track the
mouse forever—only until some other event, such as the release of a
button.

Macro: **track-mouse** body… [¶](#index-track_002dmouse)
:   This macro executes body, with generation of mouse motion events
    enabled. Typically, body would use `read-event` to read
    the motion events and modify the display accordingly. See [Motion Events](#Motion-Events), for the format of mouse motion events.

    The value of `track-mouse` is that of the last form in
    body. You should design body to return when it sees the
    up-event that indicates the release of the button, or whatever kind of
    event means it is time to stop tracking. Its value also controls how
    mouse events are reported while a mouse button is held down: if it is
    `dropping` or `drag-source`, the motion events are reported
    relative to the frame underneath the pointer. If there is no such
    frame, the events will be reported relative to the frame the mouse
    buttons were first pressed on. In addition, the `posn-window` of
    the mouse position list will be `nil` if the value is
    `drag-source`. This is useful to determine if a frame is not
    directly visible underneath the mouse pointer.

    The `track-mouse` macro causes Emacs to generate mouse motion
    events by binding the variable `track-mouse` to a
    non-`nil` value. If that variable has the special value
    `dragging`, it additionally instructs the display engine to
    refrain from changing the shape of the mouse pointer. This is
    desirable in Lisp programs that require mouse dragging across large
    portions of Emacs display, which might otherwise cause the mouse
    pointer to change its shape according to the display portion it hovers
    on (see [Pointer Shape](#Pointer-Shape)). Therefore, Lisp programs that need the
    mouse pointer to retain its original shape during dragging should bind
    `track-mouse` to the value `dragging` at the beginning of
    their body.

The usual purpose of tracking mouse motion is to indicate on the screen
the consequences of pushing or releasing a button at the current
position.

In many cases, you can avoid the need to track the mouse by using
the `mouse-face` text property (see [Properties with Special Meanings](#Special-Properties)).
That works at a much lower level and runs more smoothly than
Lisp-level mouse tracking.

---

Next: [Pop-Up Menus](#Pop_002dUp-Menus), Previous: [Mouse Tracking](#Mouse-Tracking), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.17 Mouse Position [¶](#Mouse-Position-1)

The functions `mouse-position` and `set-mouse-position`
give access to the current position of the mouse.

Function: **mouse-position** [¶](#index-mouse_002dposition)
:   This function returns a description of the position of the mouse. The
    value looks like `(frame x . y)`, where x
    and y are integers giving the (possibly rounded) position in
    multiples of the default character size of frame (see [Frame Font](#Frame-Font)) relative to the native position of frame (see [Frame Geometry](#Frame-Geometry)).

Variable: **mouse-position-function** [¶](#index-mouse_002dposition_002dfunction)
:   If non-`nil`, the value of this variable is a function for
    `mouse-position` to call. `mouse-position` calls this
    function just before returning, with its normal return value as the
    sole argument, and it returns whatever this function returns to it.

    This abnormal hook exists for the benefit of packages like
    `xt-mouse.el` that need to do mouse handling at the Lisp level.

Variable: **tty-menu-calls-mouse-position-function** [¶](#index-tty_002dmenu_002dcalls_002dmouse_002dposition_002dfunction)
:   If non-`nil`, TTY menus will call `mouse-position-function`
    as described above. This exists for cases where
    `mouse-position-function` is not safe to be called by the TTY
    menus, such as if it could trigger redisplay.

Function: **set-mouse-position** frame x y [¶](#index-set_002dmouse_002dposition)
:   This function *warps the mouse* to position x, y in
    frame frame. The arguments x and y are integers,
    giving the position in multiples of the default character size of
    frame (see [Frame Font](#Frame-Font)) relative to the native position of
    frame (see [Frame Geometry](#Frame-Geometry)).

    The resulting mouse position is constrained to the native frame of
    frame. If frame is not visible, this function does nothing.
    The return value is not significant.

Function: **mouse-pixel-position** [¶](#index-mouse_002dpixel_002dposition)
:   This function is like `mouse-position` except that it returns
    coordinates in units of pixels rather than units of characters.

Function: **set-mouse-pixel-position** frame x y [¶](#index-set_002dmouse_002dpixel_002dposition)
:   This function warps the mouse like `set-mouse-position` except that
    x and y are in units of pixels rather than units of
    characters.

    The resulting mouse position is not constrained to the native frame of
    frame. If frame is not visible, this function does nothing.
    The return value is not significant.

On a graphical terminal the following two functions allow the absolute
position of the mouse cursor to be retrieved and set.

Function: **mouse-absolute-pixel-position** [¶](#index-mouse_002dabsolute_002dpixel_002dposition)
:   This function returns a cons cell (x . y) of the coordinates
    of the mouse cursor position in pixels, relative to a position (0, 0) of
    the selected frame’s display.

Function: **set-mouse-absolute-pixel-position** x y [¶](#index-set_002dmouse_002dabsolute_002dpixel_002dposition)
:   This function moves the mouse cursor to the position (x, y).
    The coordinates x and y are interpreted in pixels relative
    to a position (0, 0) of the selected frame’s display.

The following function can tell whether the mouse cursor is currently
visible on a frame:

Function: **frame-pointer-visible-p** &optional frame [¶](#index-frame_002dpointer_002dvisible_002dp)
:   This predicate function returns non-`nil` if the mouse pointer
    displayed on frame is visible; otherwise it returns `nil`.
    frame omitted or `nil` means the selected frame. This is
    useful when `make-pointer-invisible` is set to `t`: it
    allows you to know if the pointer has been hidden.
    See [Mouse Avoidance](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Mouse-Avoidance) in The Emacs Manual.

---

Next: [On-Screen Keyboards](#On_002dScreen-Keyboards), Previous: [Mouse Position](#Mouse-Position), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.18 Pop-Up Menus [¶](#Pop_002dUp-Menus-1)

A Lisp program can pop up a menu so that the user can choose an
alternative with the mouse. On a text terminal, if the mouse is not
available, the user can choose an alternative using the keyboard
motion keys—`C-n`, `C-p`, or up- and down-arrow keys.

Function: **x-popup-menu** position menu [¶](#index-x_002dpopup_002dmenu)
:   This function displays a pop-up menu and returns an indication of
    what selection the user makes.

    The argument position specifies where on the screen to put the
    top left corner of the menu. It can be either a mouse button or
    `touchscreen-begin` event (which says to put the menu where the
    user actuated the button) or a list of this form:

    ```
    ((xoffset yoffset) window)
    ```

    where xoffset and yoffset are coordinates, measured in
    pixels, counting from the top left corner of window. window
    may be a window or a frame.

    If position is `t`, it means to use the current mouse
    position (or the top-left corner of the frame if the mouse is not
    available on a text terminal). If position is `nil`, it
    means to precompute the key binding equivalents for the keymaps
    specified in menu, without actually displaying or popping up the
    menu.

    The argument menu says what to display in the menu. It can be a
    keymap or a list of keymaps (see [Menu Keymaps](#Menu-Keymaps)). In this case, the
    return value is the list of events corresponding to the user’s choice.
    This list has more than one element if the choice occurred in a
    submenu. (Note that `x-popup-menu` does not actually execute the
    command bound to that sequence of events.) On text terminals and
    toolkits that support menu titles, the title is taken from the prompt
    string of menu if menu is a keymap, or from the prompt
    string of the first keymap in menu if it is a list of keymaps
    (see [Defining Menus](#Defining-Menus)).

    Alternatively, menu can have the following form:

    ```
    (title pane1 pane2...)
    ```

    where each pane is a list of form

    ```
    (title item1 item2...)
    ```

    Each item should be a cons cell, `(line . value)`,
    where line is a string and value is the value to return if
    that line is chosen. Unlike in a menu keymap, a `nil`
    value does not make the menu item non-selectable.
    Alternatively, each item can be a string rather than a cons
    cell; this makes a non-selectable menu item.

    If the user gets rid of the menu without making a valid choice, for
    instance by clicking the mouse away from a valid choice or by typing
    `C-g`, then this normally results in a quit and
    `x-popup-menu` does not return. But if position is a mouse
    button event (indicating that the user invoked the menu with the
    mouse) then no quit occurs and `x-popup-menu` returns `nil`.

**Usage note:** Don’t use `x-popup-menu` to display a menu
if you could do the job with a prefix key defined with a menu keymap.
If you use a menu keymap to implement a menu, `C-h c` and `C-h
a` can see the individual items in that menu and provide help for them.
If instead you implement the menu by defining a command that calls
`x-popup-menu`, the help facilities cannot know what happens inside
that command, so they cannot give any help for the menu’s items.

The menu bar mechanism, which lets you switch between submenus by
moving the mouse, cannot look within the definition of a command to see
that it calls `x-popup-menu`. Therefore, if you try to implement a
submenu using `x-popup-menu`, it cannot work with the menu bar in
an integrated fashion. This is why all menu bar submenus are
implemented with menu keymaps within the parent menu, and never with
`x-popup-menu`. See [The Menu Bar](#Menu-Bar).

If you want a menu bar submenu to have contents that vary, you should
still use a menu keymap to implement it. To make the contents vary, add
a hook function to `menu-bar-update-hook` to update the contents of
the menu keymap as necessary.

Variable: **x-pre-popup-menu-hook** [¶](#index-x_002dpre_002dpopup_002dmenu_002dhook)
:   A normal hook run immediately before a pop-up menu is displayed,
    either directly by calling `x-popup-menu`, or through a menu
    keymap. It won’t be called if `x-popup-menu` returns for some
    other reason without displaying a pop-up menu.

---

Next: [Dialog Boxes](#Dialog-Boxes), Previous: [Pop-Up Menus](#Pop_002dUp-Menus), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.19 On-Screen Keyboards [¶](#On_002dScreen-Keyboards-1)

An on-screen keyboard is a special kind of pop up provided by the
system, with rows of clickable buttons that act as a real keyboard.

On certain systems (see [On-Screen Keyboards](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#On_002dScreen-Keyboards) in The Emacs
Manual), Emacs is supposed to display and hide the on screen keyboard
depending on whether or not the user is about to type something.

Function: **frame-toggle-on-screen-keyboard** frame hide [¶](#index-frame_002dtoggle_002don_002dscreen_002dkeyboard)
:   This function displays or hides the on-screen keyboard on behalf of
    the frame frame. If hide is non-`nil`, then the
    on-screen keyboard is hidden; otherwise, it is displayed.

    It returns whether or not the on screen keyboard **may** have
    been displayed, which should be used to determine whether or not to
    hide the on-screen keyboard later.

    This has no effect if the system automatically detects when to display
    the on-screen keyboard, or when it does not provide any on-screen
    keyboard.

---

Next: [Pointer Shape](#Pointer-Shape), Previous: [On-Screen Keyboards](#On_002dScreen-Keyboards), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.20 Dialog Boxes [¶](#Dialog-Boxes-1)

A dialog box is a variant of a pop-up menu—it looks a little
different, it always appears in the center of a frame, and it has just
one level and one or more buttons. The main use of dialog boxes is
for asking questions that the user can answer with “yes”, “no”,
and a few other alternatives. With a single button, they can also
force the user to acknowledge important information. The functions
`y-or-n-p` and `yes-or-no-p` use dialog boxes instead of the
keyboard, when called from commands invoked by mouse clicks.

Function: **x-popup-dialog** position contents &optional header [¶](#index-x_002dpopup_002ddialog)
:   This function displays a pop-up dialog box and returns an indication of
    what selection the user makes. The argument contents specifies
    the alternatives to offer; it has this format:

    ```
    (title (string . value)...)
    ```

    which looks like the list that specifies a single pane for
    `x-popup-menu`.

    The return value is value from the chosen alternative.

    As for `x-popup-menu`, an element of the list may be just a
    string instead of a cons cell `(string . value)`.
    That makes a box that cannot be selected.

    If `nil` appears in the list, it separates the left-hand items from
    the right-hand items; items that precede the `nil` appear on the
    left, and items that follow the `nil` appear on the right. If you
    don’t include a `nil` in the list, then approximately half the
    items appear on each side.

    Dialog boxes always appear in the center of a frame; the argument
    position specifies which frame. The possible values are as in
    `x-popup-menu`, but the precise coordinates or the individual
    window don’t matter; only the frame matters.

    If header is non-`nil`, the frame title for the box is
    ‘`Information`’, otherwise it is ‘`Question`’. The former is used
    for `message-box` (see [message-box](#message_002dbox)). (On text terminals, the
    box title is not displayed.)

    In some configurations, Emacs cannot display a real dialog box; so
    instead it displays the same items in a pop-up menu in the center of the
    frame.

    If the user gets rid of the dialog box without making a valid choice,
    for instance using the window manager, then this produces a quit and
    `x-popup-dialog` does not return.

---

Next: [Window System Selections](#Window-System-Selections), Previous: [Dialog Boxes](#Dialog-Boxes), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.21 Pointer Shape [¶](#Pointer-Shape-1)

You can specify the mouse pointer style for particular text or
images using the `pointer` text property, and for images with the
`:pointer` and `:map` image properties. The values you can
use in these properties are in the table below. The actual shapes
may vary between systems; the descriptions are examples.

`text`

`nil`
:   The usual mouse pointer style used over text (an “I”-like shape).

`arrow`

`vdrag`

`modeline`
:   An arrow that points north-west.

`hand`
:   A hand that points upwards.

`hdrag`
:   A right-left arrow.

`nhdrag`
:   An up-down arrow.

`hourglass`
:   A rotating ring.

Over void parts of the window (parts that do not correspond to any
of the buffer contents), the mouse pointer usually uses the
`arrow` style, but you can specify a different style (one of
those above) by setting `void-text-area-pointer`.

User Option: **void-text-area-pointer** [¶](#index-void_002dtext_002darea_002dpointer)
:   This variable specifies the mouse pointer style for void text areas.
    These include the areas after the end of a line or below the last line
    in the buffer. The default is to use the `arrow` (non-text)
    pointer style.

When using some window systems, you can specify what the `text`
pointer style really looks like by setting the variable
`x-pointer-shape`.

Variable: **x-pointer-shape** [¶](#index-x_002dpointer_002dshape)
:   This variable specifies the pointer shape to use ordinarily in the
    Emacs frame, for the `text` pointer style.

Variable: **x-sensitive-text-pointer-shape** [¶](#index-x_002dsensitive_002dtext_002dpointer_002dshape)
:   This variable specifies the pointer shape to use when the mouse
    is over mouse-sensitive text.

These variables affect newly created frames. They do not normally
affect existing frames; however, if you set the mouse color of a
frame, that also installs the current value of those two variables.
See [Font and Color Parameters](#Font-and-Color-Parameters).

The values you can use, to specify either of these pointer shapes, are
defined in the file `lisp/term/x-win.el`. Use `M-x apropos
RET x-pointer RET` to see a list of them.

---

Next: [Accessing Selections](#Accessing-Selections), Previous: [Pointer Shape](#Pointer-Shape), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.22 Window System Selections [¶](#Window-System-Selections-1)

In window systems, such as X, data can be transferred between
different applications by means of *selections*. Each window
system defines an arbitrary number of *selection types*, all
storing their own data; however, only three are commonly used: the
*clipboard*, *primary selection*, and *secondary
selection*. See [Cut and Paste](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Cut-and-Paste) in The GNU Emacs
Manual, for Emacs commands that make use of these selections. This
section documents the low-level functions for reading and setting
window-system selections; See [Accessing Selections](#Accessing-Selections), for
documentation concerning selection types and data formats under
particular window systems.

Command: **gui-set-selection** type data [¶](#index-gui_002dset_002dselection)
:   This function sets a window-system selection. It takes two arguments:
    a selection type type, and the value to assign to it, data.

    type should be a symbol; it is usually one of `PRIMARY`,
    `SECONDARY` or `CLIPBOARD`. These are generally symbols
    with upper-case names, in accord with X Window System conventions. If
    type is `nil`, that stands for `PRIMARY`.

    If data is `nil`, it means to clear out the selection.
    Otherwise, data may be a string, a symbol, an integer, an
    overlay, or a cons of two markers pointing to the same buffer. An
    overlay or a pair of markers stands for text in the overlay or between
    the markers. The argument data may also be a vector of valid
    non-vector selection values.

    If data is a string, then its text properties can specify values
    used for individual data types. For example, if data has a
    property named `text/uri-list`, then a call to
    `gui-get-selection` with the data type `text/uri-list` will
    result in the value of that property being used instead of data
    itself.

    This function returns data.

Function: **gui-get-selection** &optional type data-type [¶](#index-gui_002dget_002dselection)
:   This function accesses selections set up by Emacs or by other
    programs. It takes two optional arguments, type and
    data-type. The default for type, the selection type, is
    `PRIMARY`.

    The data-type argument specifies the form of data conversion to
    use, to convert the raw data obtained from another program into Lisp
    data. It defaults to `STRING`. See [X Selections](#X-Selections), for an
    enumeration of data types valid on X, and see [Other Selections](#Other-Selections) for
    those elsewhere. On X Window system, we recommend to always specify a
    particular data-type, especially if the selection is expected to
    be non-ASCII text (in which case Lisp programs should prefer
    `UTF8_STRING` as the value of data-type). This is because
    the default data-type value, `STRING`, can only support
    Latin-1 text, which in many cases is nowadays inadequate.

User Option: **selection-coding-system** [¶](#index-selection_002dcoding_002dsystem)
:   This variable provides a coding system (see [Coding Systems](#Coding-Systems)) which
    is used to encode selection data, and takes effect on MS-Windows and
    X. It is also used in the MS-DOS port when it runs on MS-Windows and
    can access the Windows clipboard text.

    On X, the value of this variable provides the coding system which
    `gui-get-selection` will use to decode selection data for a
    subset of text data types, and also forces replies to selection
    requests for the polymorphic `TEXT` data type to be encoded by
    the `compound-text-with-extensions` coding system rather than
    Unicode.

    On MS-Windows, this variable is generally ignored, as the MS-Windows
    clipboard provides the information about decoding as part of the
    clipboard data, and uses either UTF-16 or locale-specific encoding
    automatically as appropriate. We recommend to set the value of this
    variable only on the older Windows 9X, as it is otherwise used only in
    the very rare cases when the information provided by the clipboard
    data is unusable for some reason.

    The default value of this variable is the system code page under
    MS-Windows 98 or Me, `utf-16le-dos` on Windows
    NT/W2K/XP/Vista/7/8/10/11, `iso-latin-1-dos` on MS-DOS, and
    `nil` elsewhere.

For backward compatibility, there are obsolete aliases
`x-get-selection` and `x-set-selection`, which were the
names of `gui-get-selection` and `gui-set-selection` before
Emacs 25.1.

---

Next: [Yanking Media](#Yanking-Media), Previous: [Window System Selections](#Window-System-Selections), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
