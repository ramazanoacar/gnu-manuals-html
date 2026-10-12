To specify a preference for showing an arbitrary buffer, users
    should customize `display-buffer-base-action`. An example of how
    users who prefer working with multiple frames would do that was given
    in the previous subsection. `display-buffer-alist` should be
    reserved for displaying specific buffers in a specific way.

Consider reusing a window that already shows the buffer
:   Generally, it’s always a good idea for users and Lisp
    programmers to be prepared for the case that a window already shows
    the buffer in question and to reuse that window. In the preceding
    subsection we have shown that failing to do so properly may cause
    `display-buffer` to continuously pop up a new frame although a
    frame showing that buffer existed already. In a few cases only, it
    might be undesirable to reuse a window, for example, when a different
    portion of the buffer should be shown in that window.

    Hence, `display-buffer-reuse-window` is one action function
    that should be used as often as possible, both in action
    arguments and customizations. An `inhibit-same-window` entry in
    the action argument usually takes care of the most common case
    where reusing a window showing the buffer should be avoided—that
    where the window in question is the selected one.

Attract focus to the window chosen
:   This is a no-brainer for people working with multiple frames—the
    frame showing the buffer will automatically raise and get focus unless
    an `inhibit-switch-frame` entry forbids it. For single frame
    users this task can be considerably more difficult. In particular,
    `display-buffer-pop-up-window` and
    `display-buffer-use-some-window` can become obtrusive in this
    regard. They split or use a seemingly arbitrary (often the largest or
    least recently used) window, distracting the user’s attention.

    Some Lisp programs therefore try to choose a window at the bottom of
    the frame, for example, in order to display the buffer in vicinity of
    the minibuffer window where the user is expected to answer a question
    related to the new window. For non-input related actions
    `display-buffer-below-selected` might be preferable because the
    selected window usually already has the user’s attention.

Take care which window is selected
:   Many applications call `display-buffer` from within window
    excursions produced by `with-selected-window` or
    `select-window` calls with a non-`nil` norecord
    argument. This is almost always a bad idea because the window selected
    within such an excursion is usually not the window selected in the
    configuration presented to the user.

    If, for example, a user had added an `inhibit-same-window` alist
    entry, that entry would have avoided the window selected within the
    scope of the excursion and not the window selected in the resulting
    configuration. Even if no such entry has been added, the resulting
    behavior might be strange. While in a frame containing one live
    window, evaluating the following form

    ```
    (progn
      (split-window)
      (display-buffer "*Messages*"))
    ```

    will display a window showing the `*Messages*` buffer at the bottom
    and leave the other window selected. Evaluating the next form

    ```
    (with-selected-window (split-window)
      (display-buffer "*Messages*"))
    ```

    will display `*Messages*` in a window on the top and select it
    which is usually not what `display-buffer` is supposed to do.

    On the other hand, while evaluating the following form

    ```
    (progn
      (split-window)
      (pop-to-buffer "*Messages*"))
    ```

    will correctly select the `*Messages*` buffer, the next form

    ```
    (progn
      (split-window)
      (with-selected-window (selected-window)
        (pop-to-buffer "*Messages*")))
    ```

    will not.

    Also, invocations of action functions like
    `display-buffer-use-some-window` and
    `display-buffer-use-least-recent-window` that expect the selected
    window to have the highest use time among all windows, may fail to
    produce a window according to their specifications.

    Hence, an application that relies on using a window excursion should try
    to postpone the `display-buffer` call until after the excursion has
    terminated.

---

Next: [Dedicated Windows](#Dedicated-Windows), Previous: [Displaying a Buffer in a Suitable Window](#Displaying-Buffers), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.16 Window History [¶](#Window-History-1)

Each window remembers in a list the buffers it has previously
displayed, and the order in which these buffers were removed from it.
This history is used, for example, by `replace-buffer-in-windows`
(see [Buffers and Windows](#Buffers-and-Windows)), and when quitting windows
(see [Quitting Windows](#Quitting-Windows)). The list is automatically maintained by
Emacs, but you can use the following functions to explicitly inspect
or alter it:

Function: **window-prev-buffers** &optional window [¶](#index-window_002dprev_002dbuffers)
:   This function returns a list specifying the previous contents of
    window. The optional argument window should be a live
    window and defaults to the selected one.

    Each list element has the form `(buffer window-start
    window-pos)`, where buffer is a buffer previously shown in
    the window, window-start is a marker that gives the window start
    position (see [The Window Start and End Positions](#Window-Start-and-End)) when that buffer was last shown,
    and window-pos is a marker that gives the window point position
    (see [Windows and Point](#Window-Point)) when that buffer was last shown in window.

    The list is ordered so that earlier elements correspond to more
    recently-shown buffers, and the first element usually corresponds to the
    buffer most recently removed from the window.

    As a rule, elements are removed from this list when their buffer gets
    killed. In rare cases, however, elements may have survived the killing
    of their buffer. It’s a good idea therefore to always check buffers for
    liveness when processing the return value of this function.

Function: **set-window-prev-buffers** window prev-buffers [¶](#index-set_002dwindow_002dprev_002dbuffers)
:   This function sets window’s previous buffers to the value of
    prev-buffers. The argument window must be a live window
    and defaults to the selected one. The argument prev-buffers
    should be a list of the same form as that returned by
    `window-prev-buffers`.

In addition, each window maintains a list of *next buffers*, which
is a list of buffers re-shown by `switch-to-prev-buffer` (see
below). This list is mainly used by `switch-to-prev-buffer` and
`switch-to-next-buffer` for choosing buffers to switch to.

Function: **window-next-buffers** &optional window [¶](#index-window_002dnext_002dbuffers)
:   This function returns the list of buffers recently re-shown in
    window via `switch-to-prev-buffer`. The window
    argument must denote a live window or `nil` (meaning the selected
    window).

Function: **set-window-next-buffers** window next-buffers [¶](#index-set_002dwindow_002dnext_002dbuffers)
:   This function sets the next buffer list of window to
    next-buffers. The window argument should be a live window
    or `nil` (meaning the selected window). The argument
    next-buffers should be a list of buffers.

The following commands can be used to cycle through the global buffer
list, much like `bury-buffer` and `unbury-buffer`. However,
they cycle according to the specified window’s history list, rather
than the global buffer list. In addition, they restore
window-specific window start and point positions, and may show a
buffer even if it is already shown in another window. The
`switch-to-prev-buffer` command, in particular, is used by
`replace-buffer-in-windows`, `bury-buffer` and
`quit-window` to find a replacement buffer for a window.

Command: **switch-to-prev-buffer** &optional window bury-or-kill [¶](#index-switch_002dto_002dprev_002dbuffer)
:   This command displays the previous buffer in window. The
    argument window should be a live window or `nil` (meaning
    the selected window). If the optional argument bury-or-kill is
    non-`nil`, this means that the buffer currently shown in
    window is about to be buried or killed and consequently should
    not be switched to in future invocations of this command.

    The previous buffer is usually the buffer shown before the buffer
    currently shown in window. However, a buffer that has been buried
    or killed, or has been already shown by a recent invocation of
    `switch-to-prev-buffer`, does not qualify as previous buffer.

    If repeated invocations of this command have already shown all buffers
    previously shown in window, further invocations will show buffers
    from the buffer list of the frame window appears on (see [The Buffer List](#Buffer-List)).

    The option `switch-to-prev-buffer-skip` described below can be
    used to inhibit switching to certain buffers, for example, to those
    already shown in another window. Also, if window’s frame has a
    `buffer-predicate` parameter (see [Buffer Parameters](#Buffer-Parameters)), that
    predicate may inhibit switching to certain buffers.

Command: **switch-to-next-buffer** &optional window [¶](#index-switch_002dto_002dnext_002dbuffer)
:   This command switches to the next buffer in window, thus undoing
    the effect of the last `switch-to-prev-buffer` command in
    window. The argument window must be a live window and
    defaults to the selected one.

    If there is no recent invocation of `switch-to-prev-buffer` that
    can be undone, this function tries to show a buffer from the buffer list
    of the frame window appears on (see [The Buffer List](#Buffer-List)).

    The option `switch-to-prev-buffer-skip` and the
    `buffer-predicate` (see [Buffer Parameters](#Buffer-Parameters)) of window’s
    frame affect this command as they do for `switch-to-prev-buffer`.

By default `switch-to-prev-buffer` and
`switch-to-next-buffer` can switch to a buffer that is already
shown in another window. The following option can be used to override
this behavior.

User Option: **switch-to-prev-buffer-skip** [¶](#index-switch_002dto_002dprev_002dbuffer_002dskip)
:   If this variable is `nil`, `switch-to-prev-buffer` may
    switch to any buffer, including those already shown in other windows.

    If this variable is non-`nil`, `switch-to-prev-buffer` will
    refrain from switching to certain buffers. The following values can
    be used:

    * `this` means do not switch to a buffer shown on the frame that
      hosts the window `switch-to-prev-buffer` is acting upon.
    * `visible` means do not switch to a buffer shown on any visible
      frame.
    * 0 (the number zero) means do not switch to a buffer shown on any
      visible or iconified frame.
    * `t` means do not switch to a buffer shown on any live frame.
    * A function that takes three arguments—the window argument of
      `switch-to-prev-buffer`, a buffer `switch-to-prev-buffer`
      intends to switch to and the bury-or-kill argument of
      `switch-to-prev-buffer`. If that function returns
      non-`nil`, `switch-to-prev-buffer` will refrain from
      switching to the buffer specified by the second argument.

    The command `switch-to-next-buffer` obeys this option in a
    similar way. If this option specifies a function,
    `switch-to-next-buffer` will call that function with the third
    argument always `nil`.

    Note that since `switch-to-prev-buffer` is called by
    `bury-buffer`, `replace-buffer-in-windows` and
    `quit-restore-window` as well, customizing this option may also
    affect the behavior of Emacs when a window is quit or a buffer gets
    buried or killed.

    Note also that under certain circumstances
    `switch-to-prev-buffer` and `switch-to-next-buffer` may
    ignore this option, for example, when there is only one buffer left
    these functions can switch to.

User Option: **switch-to-prev-buffer-skip-regexp** [¶](#index-switch_002dto_002dprev_002dbuffer_002dskip_002dregexp)
:   This user option should be either a regular expression or a list of
    regular expressions. Buffers whose names match one of those regular
    expressions will be ignored by `switch-to-prev-buffer` and
    `switch-to-next-buffer` (except when there’s no other buffer to
    switch to).

---

Next: [Quitting Windows](#Quitting-Windows), Previous: [Window History](#Window-History), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.17 Dedicated Windows [¶](#Dedicated-Windows-1)

Functions for displaying a buffer can be told to not use specific
windows by marking these windows as *dedicated* to their buffers.
`display-buffer` (see [Choosing a Window for Displaying a Buffer](#Choosing-Window)) never uses a dedicated
window for displaying another buffer in it. `get-lru-window` and
`get-largest-window` (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)) do not
consider dedicated windows as candidates when their dedicated
argument is non-`nil`. The behavior of `set-window-buffer`
(see [Buffers and Windows](#Buffers-and-Windows)) with respect to dedicated windows is
slightly different, see below.

Functions supposed to remove a buffer from a window or a window from
a frame can behave specially when a window they operate on is dedicated.
We will distinguish four basic cases, namely where (1) the window is
not the only window on its frame, (2) the window is the only window on
its frame but there are other frames on the same terminal left, (3)
the window is the only window on the only frame on the same terminal,
and (4) the dedication’s value is `side`
(see [Displaying Buffers in Side Windows](#Displaying-Buffers-in-Side-Windows)).

In particular, `delete-windows-on` (see [Deleting Windows](#Deleting-Windows))
handles case (2) by deleting the associated frame and cases (3) and (4)
by showing another buffer in that frame’s only window. The function
`replace-buffer-in-windows` (see [Buffers and Windows](#Buffers-and-Windows)) which is
called when a buffer gets killed, deletes the window in case (1) and
behaves like `delete-windows-on` otherwise.

When `bury-buffer` (see [The Buffer List](#Buffer-List)) operates on the
selected window (which shows the buffer that shall be buried), it
handles case (2) by calling `frame-auto-hide-function`
(see [Quitting Windows](#Quitting-Windows)) to deal with the selected frame. The other
two cases are handled as with `replace-buffer-in-windows`.

Function: **window-dedicated-p** &optional window [¶](#index-window_002ddedicated_002dp)
:   This function returns non-`nil` if window is dedicated to its
    buffer and `nil` otherwise. More precisely, the return value is
    the value assigned by the last call of `set-window-dedicated-p` for
    window, or `nil` if that function was never called with
    window as its argument. The default for window is the
    selected window.

Function: **set-window-dedicated-p** window flag [¶](#index-set_002dwindow_002ddedicated_002dp)
:   This function marks window as dedicated to its buffer if
    flag is non-`nil`, and non-dedicated otherwise.
    Interactively you can use the `C-x w d`
    (`toggle-window-dedicated`) command to do the same.

    As a special case, if flag is `t`, window becomes
    *strongly* dedicated to its buffer. `set-window-buffer`
    signals an error when the window it acts upon is strongly dedicated to
    its buffer and does not already display the buffer it is asked to
    display. Other functions do not treat `t` differently from any
    non-`nil` value.

You can also tell `display-buffer` to mark a window it creates as
dedicated to its buffer by providing a suitable `dedicated`
action alist entry (see [Action Alists for Buffer Display](#Buffer-Display-Action-Alists)).

---

Next: [Side Windows](#Side-Windows), Previous: [Dedicated Windows](#Dedicated-Windows), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.18 Quitting Windows [¶](#Quitting-Windows-1)

After a command uses `display-buffer` to put a buffer on the
screen, the user may decide to hide it and return to the previous
configuration of the Emacs display. We call that *quitting the
window*. The way to do this is to call `quit-window` while the
window used by `display-buffer` is the selected window.

The right way to restore the previous configuration of the display
depends on what was done to the window where the buffer now appears.
It might be right to delete that window, or delete its frame, or just
display another buffer in that window. One complication is that the
user may have changed the window configuration since the act of
displaying that buffer, and it would be undesirable to undo the user’s
explicitly requested changes.

To enable `quit-window` to do the right thing,
`display-buffer` saves information about what it did in the
window’s `quit-restore` parameter (see [Window Parameters](#Window-Parameters)).

Command: **quit-window** &optional kill window [¶](#index-quit_002dwindow)
:   This command quits window and buries its buffer. (It could also
    kill the buffer, subject to `quit-window-kill-buffer`, see below.)
    The argument window must be a live window and defaults to the
    selected one. With prefix argument kill non-`nil`, it kills
    the buffer instead of burying it.

    The function `quit-window` first runs `quit-window-hook`.
    Then it calls the function `quit-restore-window`, described below,
    which does the hard work.

The following option tells `quit-window` whether it should kill or
bury window’s buffer when the kill argument is `nil`.

User Option: **quit-window-kill-buffer** [¶](#index-quit_002dwindow_002dkill_002dbuffer)
:   This variable controls whether `quit-window` kills the buffer of
    its window argument. If the value is `nil` (the default)
    `quit-window` will kill window’s buffer only if the
    kill argument is non-`nil`. If this is `t`,
    `quit-window` will try to kill window’s buffer regardless of
    the value of the kill argument. Otherwise, this should be a list
    of major modes; `quit-window` will kill the buffer of window
    regardless of the value of kill if that buffer’s major mode is
    either a member of this list or is derived from a member of this list.
    In any other case, `quit-window` will kill the buffer only if
    kill is non-`nil` and bury it otherwise.

You can get more control by calling `quit-restore-window` instead
of `quit-window`.

Function: **quit-restore-window** &optional window bury-or-kill [¶](#index-quit_002drestore_002dwindow)
:   This function handles window and its buffer after quitting. The
    optional argument window must be a live window and defaults to
    the selected one. The function takes account of the window’s
    `quit-restore` parameter.

    The optional argument bury-or-kill specifies how to deal with
    window’s buffer. The following values are meaningful:

    `nil`
    :   This means to not deal with the buffer in any particular way. As a
        consequence, if window is not deleted, invoking
        `switch-to-prev-buffer` will usually show the buffer again.

    `append`
    :   This means that if window is not deleted, its buffer is moved to
        the end of window’s list of previous buffers (see [Window History](#Window-History)), so it’s less likely that future invocations of
        `switch-to-prev-buffer` will switch to it. Also, it moves the
        buffer to the end of the frame’s buffer list (see [The Buffer List](#Buffer-List)).

    `bury`
    :   This means that if window is not deleted, its buffer is removed
        from window’s list of previous buffers. Also, it moves the
        buffer to the end of the frame’s buffer list. This is the most
        reliable way to prevent `switch-to-prev-buffer` from switching to
        this buffer again, short of killing the buffer.

    `kill`
    :   This means to kill window’s buffer.

    `killing`
    :   This is handled like `kill` but assumes that window’s buffer
        gets killed elsewhere. This value is used by
        `replace-buffer-in-windows` and `quit-windows-on`.

    `burying`
    :   This is handled like `bury` but assumes that window’s buffer
        gets buried elsewhere. This value is used by `quit-windows-on`.

    The argument bury-or-kill also specifies what to do with
    window’s frame when window should be deleted, if it is the
    only window on its frame, and there are other frames on that frame’s
    terminal. If bury-or-kill equals `kill`, it means to
    delete the frame. Otherwise, the fate of the frame is determined by
    calling `frame-auto-hide-function` (see below) with that frame as
    sole argument.

A window’s `quit-restore` and `quit-restore-prev` parameters
(see [Window Parameters](#Window-Parameters)) guide `quit-restore-window` through its
process of dealing with its window argument. If such a parameter
is absent or `nil`, this usually means that the window has been
created by a command like `split-window-below` or
`split-window-right` (see [Split Window](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Split-Window) in The GNU Emacs
Manual) and `quit-restore-window` will delete the window only if
it was dedicated to its buffer.

If non-`nil`, any such parameter is a list of four elements:

```
(method obuffer owindow this-buffer)
```

The first element, method, is one of the four symbols
`window`, `frame`, `same` and `other`.
`frame` and `window` control how to delete window,
while `same` and `other` control displaying some other
buffer in it.

Specifically, `window` means that the window has been specially
created by `display-buffer`; `frame` means that a separate
frame has been created; `same`, that the window has only ever
displayed this buffer; `other`, that the window showed another
buffer before.

The second element, obuffer, is either one of the symbols
`window` or `frame`, or a list of the form

```
(prev-buffer prev-window-start prev-window-point height)
```

which says which buffer was shown in window before, that
buffer’s window start (see [The Window Start and End Positions](#Window-Start-and-End)) and window point
(see [Windows and Point](#Window-Point)) positions at that time, and
window’s height at that time. If prev-buffer is still
live when quitting window, quitting the window may reuse
window to display prev-buffer.

The third element, owindow, is the window that was selected
just before the displaying was done. If quitting deletes
window, it tries to select owindow.

The fourth element, this-buffer, is the buffer whose displaying
set the `quit-restore` parameter. Quitting window will use
the information stored by that parameter if and only if it still shows
that buffer.

`quit-restore-window` tries to delete its window if (1)
method is either `window` or `frame`, (2) the window has
no history of previously-displayed buffers and (3) this-buffer
equals the buffer currently displayed in window. If window
is part of an atomic window (see [Atomic Windows](#Atomic-Windows)), it will try to
delete the root of that atomic window instead. In either case, it tries
to avoid signaling an error when window cannot be deleted.

If obuffer is a list, and prev-buffer is still live,
quitting displays prev-buffer in window according to the
rest of the elements of obuffer. This includes resizing the
window to height if it was temporarily resized to display
this-buffer.

Otherwise, if window was previously used for displaying other
buffers (see [Window History](#Window-History)), the most recent buffer in that
history will be displayed.

Conceptually, the `quit-restore` parameter is used for undoing
the first buffer display operation for a specific window. The
`quit-restore-prev` parameter is used for undoing the last buffer
display operation in a row of such operations for a specific window.
Any buffer display operation for that window that happened in between,
is undone by displaying the buffer previously shown in that window.

`display-buffer` sets up the `quit-restore` parameter of any
window it uses when that window has been either created by it or has no
non-`nil` `quit-restore` parameter. If the window already has
a `quit-restore` parameter, `display-buffer` adds a
`quit-restore-prev` parameter whose method element is either
`same` or `other`. In either case, if the window already has
a `quit-restore-prev` or `quit-restore` parameter, it may
update that parameter’s contents.

`quit-restore-window` now first tries to find a suitable
`quit-restore-prev` parameter for its window telling which buffer
to show instead. If it doesn’t find one, it will look for a suitable
`quit-restore` parameter to either delete the window or show
another buffer in it.

Once it has used one of these parameters, `quit-restore-window`
resets it to `nil`. Parameters it did not use are left alone. Any
of these parameters are also reset by `replace-buffer-in-windows`
(see [Buffers and Windows](#Buffers-and-Windows)) when they reference a buffer that is about
to be killed, either as the buffer specified by prev-buffer or as
the buffer specified by this-buffer.

All operations on these parameters are supposed to preserve the
following invariant: If a window has a non-`nil`
`quit-restore-prev` parameter, it also has a non-`nil`
`quit-restore` parameter.

The following option allows `quit-restore-window` to delete its
window more aggressively.

User Option: **quit-restore-window-no-switch** [¶](#index-quit_002drestore_002dwindow_002dno_002dswitch)
:   If this option is `nil`, `quit-restore-window` will always
    call `switch-to-prev-buffer` unless the window has been made by
    `display-buffer`. If this is `t`, `quit-restore-window`
    will try hard to switch only to buffers previously shown in that window.
    If this is the symbol `skip-first`, it will switch to a previous
    buffer if and only the window has at least two previous buffers.

    In either case, if `quit-restore-window` doesn’t switch to a
    previous buffer, it will try to delete the window (and maybe its
    frame) instead. Note also that if a window is dedicated,
    `quit-restore-window` will usually not switch to a previous
    buffer in it either.

The following option specifies a function to do the right thing with a
frame containing one window when quitting that window.

User Option: **frame-auto-hide-function** [¶](#index-frame_002dauto_002dhide_002dfunction)
:   The function specified by this option is called to automatically hide
    frames. This function is called with one argument—a frame.

    The function specified here is called by `bury-buffer`
    (see [The Buffer List](#Buffer-List)) when the selected window is dedicated and shows
    the buffer to bury. It is also called by `quit-restore-window`
    (see above) when the frame of the window to quit has been specially
    created for displaying that window’s buffer and the buffer is not
    killed.

    The default is to call `iconify-frame` (see [Visibility of Frames](#Visibility-of-Frames)). Alternatively, you may specify either `delete-frame`
    (see [Deleting Frames](#Deleting-Frames)) to remove the frame from its display,
    `make-frame-invisible` to make the frame invisible, `ignore`
    to leave the frame unchanged, or any other function that can take a
    frame as its sole argument.

    Note that the function specified by this option is called only if the
    specified frame contains just one live window and there is at least one
    other frame on the same terminal.

    For a particular frame, the value specified here may be overridden by
    that frame’s `auto-hide-function` frame parameter (see [Frame Interaction Parameters](#Frame-Interaction-Parameters)).

---

Next: [Atomic Windows](#Atomic-Windows), Previous: [Quitting Windows](#Quitting-Windows), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.19 Side Windows [¶](#Side-Windows-1)

Side windows are special windows positioned at any of the four sides of
a frame’s root window (see [Windows and Frames](#Windows-and-Frames)). In practice, this
means that the area of the frame’s root window is subdivided into a main
window and a number of side windows surrounding that main window. The
main window is either a “normal” live window or specifies the area
containing all the normal windows.

In their most simple form of use, side windows allow displaying
specific buffers always in the same area of a frame. Hence they can
be regarded as a generalization of the concept provided by
`display-buffer-at-bottom` (see [Action Functions for Buffer Display](#Buffer-Display-Action-Functions)) to the remaining sides of a frame. With suitable
customizations, however, side windows can be also used to provide
frame layouts similar to those found in so-called integrated
development environments (IDEs).

* [Displaying Buffers in Side Windows](#Displaying-Buffers-in-Side-Windows)
* [Side Window Options and Functions](#Side-Window-Options-and-Functions)
* [Frame Layouts with Side Windows](#Frame-Layouts-with-Side-Windows)

---

Next: [Side Window Options and Functions](#Side-Window-Options-and-Functions), Up: [Side Windows](#Side-Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 29.19.1 Displaying Buffers in Side Windows [¶](#Displaying-Buffers-in-Side-Windows-1)

The following action function for `display-buffer` (see [Action Functions for Buffer Display](#Buffer-Display-Action-Functions)) creates or reuses a side window for
displaying the specified buffer.

Function: **display-buffer-in-side-window** buffer alist [¶](#index-display_002dbuffer_002din_002dside_002dwindow)
:   This function displays buffer in a side window of the selected
    frame. It returns the window used for displaying buffer,
    `nil` if no such window can be found or created.

    alist is an association list of symbols and values as for
    `display-buffer`. The following symbols in alist are special
    for this function:

    `side`
    :   Denotes the side of the frame where the window shall be located. Valid
        values are `left`, `top`, `right` and `bottom`. If
        unspecified, the window is located at the bottom of the frame.

    `slot`
    :   Denotes a slot at the specified side where to locate the window. A
        value of zero means to preferably position the window in the middle of
        the specified side. A negative value means to use a slot preceding
        (that is, above or on the left of) the middle slot. A positive value
        means to use a slot following (that is, below or on the right of) the
        middle slot. Hence, all windows on a specific side are ordered by their
        `slot` value. If unspecified, the window is located in the middle
        of the specified side.

    `dedicated`
    :   The dedicated flag (see [Dedicated Windows](#Dedicated-Windows)) has a slightly different
        meaning for side windows. When a side window is created, that flag is
        set to the value `side` to prevent `display-buffer` to use the
        window in other action functions. Its value persists across invocations
        of `quit-window`, `kill-buffer`, `previous-buffer` and
        `next-buffer`.

        In particular, these commands will refrain from showing, in a side
        window, buffers that have not been displayed in that window before.
        They will also refrain from having a normal, non-side window show a
        buffer that has been already displayed in a side window. A notable
        exception to the latter rule occurs when an application, after
        displaying a buffer, resets that buffer’s local variables. To override
        these rules and always delete a side window with `quit-window` or
        `kill-buffer`, and eventually prevent the use of
        `previous-buffer` and `next-buffer`, set this value to
        `t` or specify a value via `display-buffer-mark-dedicated`.

    If you specify the same slot on the same side for two or more different
    buffers, the buffer displayed last is shown in the corresponding window.
    Hence, slots can be used for sharing the same side window between
    buffers.

    This function installs the `window-side` and `window-slot`
    parameters (see [Window Parameters](#Window-Parameters)) and makes them persistent. It
    does not install any other window parameters unless they have been
    explicitly provided via a `window-parameters` entry in alist.

By default, side windows cannot be split via `split-window`
(see [Splitting Windows](#Splitting-Windows)). Also, a side window is not reused or
split by any buffer display action (see [Action Functions for Buffer Display](#Buffer-Display-Action-Functions)) unless it is explicitly specified as target of that
action. Note also that `delete-other-windows` cannot make a side
window the only window on its frame (see [Deleting Windows](#Deleting-Windows)).

---

Next: [Frame Layouts with Side Windows](#Frame-Layouts-with-Side-Windows), Previous: [Displaying Buffers in Side Windows](#Displaying-Buffers-in-Side-Windows), Up: [Side Windows](#Side-Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 29.19.2 Side Window Options and Functions [¶](#Side-Window-Options-and-Functions-1)

The following options provide additional control over the placement of
side windows.

User Option: **window-sides-vertical** [¶](#index-window_002dsides_002dvertical)
:   If non-`nil`, the side windows on the left and right of a frame
    occupy the frame’s full height. Otherwise, the side windows on the top
    and bottom of the frame occupy the frame’s full width.

User Option: **window-sides-slots** [¶](#index-window_002dsides_002dslots)
:   This option specifies the maximum number of side windows on each side of
    a frame. The value is a list of four elements specifying the number of
    side window slots on (in this order) the left, top, right and bottom of
    each frame. If an element is a number, it means to display at most that
    many windows on the corresponding side. If an element is `nil`, it
    means there’s no bound on the number of slots on that side.

    If any of the specified values is zero, no window can be created on the
    corresponding side. `display-buffer-in-side-window` will not
    signal an error in that case, but will return `nil`. If a specified
    value just forbids the creation of an additional side window, the most
    suitable window on that side is reused and may have its
    `window-slot` parameter changed accordingly.

User Option: **window-sides-reversed** [¶](#index-window_002dsides_002dreversed)
:   This option specifies whether top/bottom side windows should appear in
    reverse order. When this is `nil`, side windows on the top and
    bottom of a frame are always drawn from left to right with increasing
    slot values. When this is `t`, the drawing order is reversed and
    side windows on the top and bottom of a frame are drawn from right to
    left with increasing slot values.

    When this is `bidi`, the drawing order is reversed if and only if
    the value of `bidi-paragraph-direction` (see [Bidirectional Display](#Bidirectional-Display)) is `right-to-left` in the buffer displayed in the window
    most recently selected within the main window area of this frame.
    Sometimes that window may be hard to find, so heuristics are used to
    avoid that the drawing order changes inadvertently when another window
    gets selected.

    The layout of side windows on the left or right of a frame is not
    affected by the value of this variable.

When a frame has side windows, the following function returns the main
window of that frame.

Function: **window-main-window** &optional frame [¶](#index-window_002dmain_002dwindow)
:   This function returns the main window of the specified frame. The
    optional argument frame must be a live frame and defaults to the
    selected one.

    If frame has no side windows, it returns frame’s root
    window. Otherwise, it returns either an internal non-side window such
    that all other non-side windows on frame descend from it, or the
    single live non-side window of frame. Note that the main window
    of a frame cannot be deleted via `delete-window`.

The following command is handy to toggle the appearance of all side
windows on a specified frame.

Command: **window-toggle-side-windows** &optional frame [¶](#index-window_002dtoggle_002dside_002dwindows)
:   This command toggles side windows on the specified frame. The
    optional argument frame must be a live frame and defaults to the
    selected one.

    If frame has at least one side window, this command saves the
    state of frame’s root window in the frame’s
    `window-state` frame parameter and deletes all side windows on
    frame afterwards.

    If frame has no side windows, but does have a `window-state`
    parameter, this command uses that parameter’s value to restore the side
    windows on frame leaving frame’s main window alone.

    An error is signaled if frame has no side windows and no saved
    state is found for it.

---

Previous: [Side Window Options and Functions](#Side-Window-Options-and-Functions), Up: [Side Windows](#Side-Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 29.19.3 Frame Layouts with Side Windows [¶](#Frame-Layouts-with-Side-Windows-1)

Side windows can be used to create more complex frame layouts like those
provided by integrated development environments (IDEs). In such
layouts, the area of the main window is where the normal editing
activities take place. Side windows are not conceived for editing in
the usual sense. Rather, they are supposed to display information
complementary to the current editing activity, like lists of files, tags
or buffers, help information, search or grep results or shell output.

The layout of such a frame might appear as follows:

```
     ___________________________________
    |          *Buffer List*            |
    |___________________________________|
    |     |                       |     |
    |  *  |                       |  *  |
    |  d  |                       |  T  |
    |  i  |                       |  a  |
    |  r  |   Main Window Area    |  g  |
    |  e  |                       |  s  |
    |  d  |                       |  *  |
    |  *  |                       |     |
    |_____|_______________________|_____|
    | *help*/*grep*/  |  *shell*/       |
    | *Completions*   |  *compilation*  |
    |_________________|_________________|
    |             Echo Area             |
    |___________________________________|
```

The following example illustrates how window parameters (see [Window Parameters](#Window-Parameters)) can be used with `display-buffer-in-side-window`
(see [Displaying Buffers in Side Windows](#Displaying-Buffers-in-Side-Windows)) to set up code for
producing the frame layout sketched above.

```
(defvar parameters
  '(window-parameters . ((no-other-window . t)
                         (no-delete-other-windows . t))))

(setq fit-window-to-buffer-horizontally t)
(setq window-resize-pixelwise t)

(setq
 display-buffer-alist
 `(("\\*Buffer List\\*" display-buffer-in-side-window
    (side . top) (slot . 0) (window-height . fit-window-to-buffer)
    (preserve-size . (nil . t)) ,parameters)
   ("\\*Tags List\\*" display-buffer-in-side-window
    (side . right) (slot . 0) (window-width . fit-window-to-buffer)
    (preserve-size . (t . nil)) ,parameters)
   ("\\*\\(?:help\\|grep\\|Completions\\)\\*"
    display-buffer-in-side-window
    (side . bottom) (slot . -1) (preserve-size . (nil . t))
    ,parameters)
   ("\\*\\(?:shell\\|compilation\\)\\*" display-buffer-in-side-window
    (side . bottom) (slot . 1) (preserve-size . (nil . t))
    ,parameters)))
```

This specifies `display-buffer-alist` entries (see [Choosing a Window for Displaying a Buffer](#Choosing-Window)) for buffers with fixed names. In particular, it asks for
showing `*Buffer List*` with adjustable height at the top of the
frame and `*Tags List*` with adjustable width on the frame’s right.
It also asks for having the `*help*`, `*grep*` and
`*Completions*` buffers share a window on the bottom left side of
the frame and the `*shell*` and `*compilation*` buffers appear
in a window on the bottom right side of the frame.

Note that the option `fit-window-to-buffer-horizontally` must
have a non-`nil` value in order to allow horizontal adjustment of
windows. Entries are also added that ask for preserving the height of
side windows at the top and bottom of the frame and the width of side
windows at the left or right of the frame. To assure that side windows
retain their respective sizes when maximizing the frame, the variable
`window-resize-pixelwise` is set to a non-`nil` value.
See [Resizing Windows](#Resizing-Windows).

The last form also makes sure that none of the created side windows
are accessible via `C-x o` by installing the `no-other-window`
parameter for each of these windows. In addition, it makes sure that
side windows are not deleted via `C-x 1` by installing the
`no-delete-other-windows` parameter for each of these windows.

Since `dired` buffers have no fixed names, we use a special
function `dired-default-directory-on-left` in order to display a
lean directory buffer on the left side of the frame.

```
(defun dired-default-directory-on-left ()
  "Display `default-directory' in side window on left, hiding details."
  (interactive)
  (let ((buffer (dired-noselect default-directory)))
    (with-current-buffer buffer (dired-hide-details-mode t))
    (display-buffer-in-side-window
     buffer `((side . left) (slot . 0)
              (window-width . fit-window-to-buffer)
              (preserve-size . (t . nil)) ,parameters))))
```

Evaluating the preceding forms and typing, in any order, `M-x list-buffers`, `C-h f`, `M-x shell`, `M-x list-tags`,
and `M-x dired-default-directory-on-left` should now reproduce the
frame layout sketched above.

---

Next: [Windows and Point](#Window-Point), Previous: [Side Windows](#Side-Windows), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.20 Atomic Windows [¶](#Atomic-Windows-1)

Atomic windows are rectangular compositions of at least two live
windows. They have the following distinctive characteristics:

* The function `split-window` (see [Splitting Windows](#Splitting-Windows)), when
  applied to a constituent of an atomic window, will try to create the new
  window outside of the atomic window.
* The function `delete-window` (see [Deleting Windows](#Deleting-Windows)), when
  applied to a constituent of an atomic window, will try to delete the
  entire atomic window instead.
* The function `delete-other-windows` (see [Deleting Windows](#Deleting-Windows)),
  when applied to a constituent of an atomic window, will try to make the
  atomic window fill its frame or main window (see [Side Windows](#Side-Windows)).

This means that the basic groups of functions that alter the window
structure treat an atomic window like a live one, thus preserving the
internal structure of the atomic window.

Atomic windows are useful to construct and preserve window layouts
that are meaningful only when all involved buffers are shown
simultaneously in a specific manner, such as when showing differences
between file revisions, or the same text in different languages or
markups. They can also be used to permanently display information
pertinent to a specific window in bars on that window’s sides.

Atomic windows are implemented with the help of the reserved
`window-atom` window parameter (see [Window Parameters](#Window-Parameters)) and an
internal window (see [Basic Concepts of Emacs Windows](#Basic-Windows)) called the root window of the
atomic window. All windows that are part of the same atomic window have
this root window as their common ancestor and are assigned a
non-`nil` `window-atom` parameter.

The following function returns the root of the atomic window a
specified window is part of:

Function: **window-atom-root** &optional window [¶](#index-window_002datom_002droot)
:   This functions returns the root of the atomic window window is a
    part of. The specified window must be a valid window and defaults
    to the selected one. It returns `nil` if window is not part
    of an atomic window.

The most simple approach to make a new atomic window is to take an
existing internal window and apply the following function:

Function: **window-make-atom** window [¶](#index-window_002dmake_002datom)
:   This function converts window into an atomic window. The
    specified window must be an internal window. All this function
    does is to set the `window-atom` parameter of each descendant of
    window to `t`.

To create a new atomic window from an existing live window or to add a
new window to an existing atomic window, the following buffer display
action function (see [Action Functions for Buffer Display](#Buffer-Display-Action-Functions)) can be used:

Function: **display-buffer-in-atom-window** buffer alist [¶](#index-display_002dbuffer_002din_002datom_002dwindow)
:   This function tries to display buffer in a new window that will be
    combined with an existing window to form an atomic window. If the
    existing window is already part of an atomic window, it adds the new
    window to that atomic window.

    The specified alist is an association list of symbols and values.
    The following symbols have a special meaning:

    `window`
    :   The value of such an element specifies an existing window the new window
        shall be combined with. If it specifies an internal window, all
        children of that window become part of the atomic window too. If no
        window is specified, the new window becomes a sibling of the selected
        window. The `window-atom` parameter of the existing window is set
        to `main` provided that window is live and its `window-atom`
        parameter was not already set.

    `side`
    :   The value of such an element denotes the side of the existing window
        where the new window shall be located. Valid values are `below`,
        `right`, `above` and `left`. The default is
        `below`. The `window-atom` parameter of the new window is set
        to this value.

    The return value is the new window, `nil` when creating that window
    failed.

Note that the value of the `window-atom` parameter does not really
matter as long as it is non-`nil`. The values assigned by
`display-buffer-in-atom-window` just allow for easy retrieval of
the original and the new window after that function has been applied.
Note also that the `window-atom` parameter is the only window
parameter assigned by `display-buffer-in-atom-window`. Further
parameters have to be set by the application explicitly via a
`window-parameters` entry in alist.

Atomic windows automatically cease to exist when one of their
constituents gets deleted. To dissolve an atomic window manually,
reset the `window-atom` parameter of its constituents—the root
of the atomic window and all its descendants.

The following code snippet, when applied to a single-window frame,
first splits the selected window and makes the selected and the new
window constituents of an atomic window with their parent as root. It
then displays the buffer `*Messages*` in a new window at the
frame’s bottom and makes that new window part of the atomic window just
created.

```
(let ((window (split-window-right)))
  (window-make-atom (window-parent window))
  (display-buffer-in-atom-window
   (get-buffer-create "*Messages*")
   `((window . ,(window-parent window)) (window-height . 5))))
```

At this moment typing `C-x 2` in any window of that frame
produces a new window at the bottom of the frame. Typing `C-x 3` instead will put the new window at the frame’s right. In either
case, typing now `C-x 1` in any window of the atomic window will
remove the new window only. Typing `C-x 0` in any window of the
atomic window will make that new window fill the frame.

---

Next: [The Window Start and End Positions](#Window-Start-and-End), Previous: [Atomic Windows](#Atomic-Windows), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.21 Windows and Point [¶](#Windows-and-Point)

Each window has its own value of point (see [Point](#Point)), independent of
the value of point in other windows displaying the same buffer. This
makes it useful to have multiple windows showing one buffer.

* Window point is established when a window is first created; it is
  initialized from the buffer’s point, or from window point of another
  window opened on the buffer if such a window exists.
* Selecting a window sets the value of point in its buffer from the
  window’s value of point. Conversely, deselecting a window sets the
  window’s value of point from that of the buffer. Thus, when you switch
  between windows that display a given buffer, the point value for the
  selected window is in effect in the buffer, while the point values for
  the other windows are stored in those windows.
* As long as the selected window displays the current buffer, the window’s
  point and the buffer’s point always move together; they remain equal.
* Many Emacs functions temporarily select a window in order to operate on
  its contents. This will move point (see [Point](#Point)) of that
  window’s buffer to the position of window point of that window and
  not restore buffer point to its previous position when terminating
  the temporary selection. This means that when one and the same buffer
  is simultaneously displayed in more than one window, its buffer point
  may change in unpredictable ways to the position of window point of any
  of these windows as a side-effect of things like redisplay, calling
  `with-selected-window` (see [Selecting Windows](#Selecting-Windows)) or running
  `window-configuration-change-hook` (see [Hooks for Window Scrolling and Changes](#Window-Hooks)).

Emacs displays the cursor, by default as a rectangular block, in
each window at the position of that window’s point. When the user
switches to another buffer in a window, Emacs moves that window’s
cursor to where point is in that buffer. If the exact position of
point is hidden behind some display element, such as a display string
or an image, Emacs displays the cursor immediately before or after
that display element.

Function: **window-point** &optional window [¶](#index-window_002dpoint)
:   This function returns the current position of point in window.
    For a nonselected window, this is the value point would have (in that
    window’s buffer) if that window were selected. The default for
    window is the selected window.

    When window is the selected window, the value returned is the
    value of point in that window’s buffer. Strictly speaking, it would be
    more correct to return the top-level value of point, outside of any
    `save-excursion` forms. But that value is hard to find.

Function: **set-window-point** window position [¶](#index-set_002dwindow_002dpoint)
:   This function positions point in window at position
    position in window’s buffer. It returns position.

    If window is selected, this simply does `goto-char` in
    window’s buffer.

Variable: **window-point-insertion-type** [¶](#index-window_002dpoint_002dinsertion_002dtype)
:   This variable specifies the marker insertion type (see [Marker Insertion Types](#Marker-Insertion-Types)) of `window-point`. The default is `nil`,
    so `window-point` will stay behind text inserted there.

Function: **set-window-cursor-type** window type [¶](#index-set_002dwindow_002dcursor_002dtype)
:   This function sets the cursor shape for window. This setting
    takes precedence over the `cursor-type` variable, and type
    has the same format as the value of that variable. See [Cursor Parameters](#Cursor-Parameters). If window is `nil`, it means to set the cursor
    type for the selected window.

    The initial value for new windows is `t`, which says to respect the
    buffer-local value of `cursor-type`. The value set by this
    function persists across buffers shown in window, so
    `set-window-buffer` does not reset it. See [Buffers and Windows](#Buffers-and-Windows).

Function: **window-cursor-type** &optional window [¶](#index-window_002dcursor_002dtype)
:   This function returns the cursor type of window, defaulting to the
    selected window.

Function: **window-cursor-info** &optional window [¶](#index-window_002dcursor_002dinfo)
:   This function returns information about the cursor of window,
    defaulting to the selected window.

    The value returned by the function is a vector of the form
    `[type x y width height ascent]`. Here’s the description of each components of this
    vector:

    type
    :   The type of the cursor, a symbol. This is the same value returned by
        `window-cursor-type`.

    x

    y
    :   The pixel coordinates of the cursor’s top-left corner, relative to the
        top-left corner of window’s text area.

    width

    height
    :   The pixel dimensions of the cursor.

    ascent
    :   The number of pixels the cursor extends above the baseline.

    If the cursor is not currently displayed for window, this function
    returns `nil`.

    Any element except the first one in the returned vector may be
    `-1`, meaning the actual value is currently unavailable.

---

Next: [Textual Scrolling](#Textual-Scrolling), Previous: [Windows and Point](#Window-Point), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.22 The Window Start and End Positions [¶](#The-Window-Start-and-End-Positions)

Each window maintains a marker used to keep track of a buffer position
that specifies where in the buffer display should start. This position
is called the *display-start* position of the window (or just the
*start*). The character after this position is the one that appears
at the upper left corner of the window. It is usually, but not
inevitably, at the beginning of a text line.

After switching windows or buffers, and in some other cases, if the
window start is in the middle of a line, Emacs adjusts the window
start to the start of a line. This prevents certain operations from
leaving the window start at a meaningless point within a line. This
feature may interfere with testing some Lisp code by executing it
using the commands of Lisp mode, because they trigger this
readjustment. To test such code, put it into a command and bind the
command to a key.

Function: **window-start** &optional window [¶](#index-window_002dstart)
:   This function returns the display-start position of window
    window. If window is `nil`, the selected window is
    used.

    When you create a window, or display a different buffer in it, the
    display-start position is set to a display-start position recently used
    for the same buffer, or to `point-min` if the buffer doesn’t have
    any.

    Redisplay updates the window-start position (if you have not specified
    it explicitly since the previous redisplay)—to make sure point appears
    on the screen. Nothing except redisplay automatically changes the
    window-start position; if you move point, do not expect the window-start
    position to change in response until after the next redisplay.

Function: **window-group-start** &optional window [¶](#index-window_002dgroup_002dstart)
:   This function is like `window-start`, except that when
    window is a part of a group of windows (see [Window Group](#Window-Group)),
    `window-group-start` returns the start position of the entire
    group. This condition holds when the buffer local variable
    `window-group-start-function` is set to a function. In this
    case, `window-group-start` calls the function with the single
    argument window, then returns its result.

Function: **window-end** &optional window update [¶](#index-window_002dend)
:   This function returns the position where display of its buffer ends in
    window. The default for window is the selected window.

    Simply changing the buffer text or moving point does not update the
    value that `window-end` returns. The value is updated only when
    Emacs redisplays and redisplay completes without being preempted.

    If the last redisplay of window was preempted, and did not finish,
    Emacs does not know the position of the end of display in that window.
    In that case, this function returns `nil`.

    If update is non-`nil`, `window-end` always returns an
    up-to-date value for where display ends, based on the current
    `window-start` value. If a previously saved value of that position
    is still valid, `window-end` returns that value; otherwise it
    computes the correct value by scanning the buffer text.

    Even if update is non-`nil`, `window-end` does not
    attempt to scroll the display if point has moved off the screen, the
    way real redisplay would do. It does not alter the
    `window-start` value. In effect, it reports where the displayed
    text will end if scrolling is not required. Note that the position it
    returns might be only partially visible.

Function: **window-group-end** &optional window update [¶](#index-window_002dgroup_002dend)
:   This function is like `window-end`, except that when window
    is a part of a group of windows (see [Window Group](#Window-Group)),
    `window-group-end` returns the end position of the entire group.
    This condition holds when the buffer local variable
    `window-group-end-function` is set to a function. In this case,
    `window-group-end` calls the function with the two arguments
    window and update, then returns its result. The argument
    update has the same meaning as in `window-end`.

Function: **set-window-start** window position &optional noforce [¶](#index-set_002dwindow_002dstart)
:   This function sets the display-start position of window to
    position in window’s buffer. It returns position.

    The display routines insist that the position of point be visible when a
    buffer is displayed. Normally, they select the display-start position
    according to their internal logic (and scroll the window if necessary)
    to make point visible. However, if you specify the start position
    with this function using `nil` for noforce, it means you
    want display to start at position even if that would put the
    location of point off the screen. If this does place point off
    screen, the display routines attempt to move point to the left margin
    on the middle line in the window.

    For example, if point is 1 and you set the start of the window
    to 37, the start of the next line, point will be above the top
    of the window. The display routines will automatically move point if
    it is still 1 when redisplay occurs. Here is an example:

    ```
    ;; Here is what ‘foo’ looks like before executing
    ;;   the set-window-start expression.
    ```

    ```

    ```

    ```
    ---------- Buffer: foo ----------
    ∗This is the contents of buffer foo.
    2
    3
    4
    5
    6
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (set-window-start
     (selected-window)
     (save-excursion
       (goto-char 1)
       (forward-line 1)
       (point)))
    ⇒ 37
    ```

    ```

    ```

    ```
    ;; Here is what ‘foo’ looks like after executing
    ;;   the set-window-start expression.
    ---------- Buffer: foo ----------
    2
    3
    ∗4
    5
    6
    ---------- Buffer: foo ----------
    ```

    If the attempt to make point visible (i.e., in a fully-visible screen
    line) fails, the display routines will disregard the requested
    window-start position and compute a new one anyway. Thus, for
    reliable results Lisp programs that call this function should always
    move point to be inside the window whose display starts at
    position.

    If noforce is non-`nil`, and position would place point
    off screen at the next redisplay, then redisplay computes a new window-start
    position that works well with point, and thus position is not used.

Function: **set-window-group-start** window position &optional noforce [¶](#index-set_002dwindow_002dgroup_002dstart)
:   This function is like `set-window-start`, except that when
    window is a part of a group of windows (see [Window Group](#Window-Group)),
    `set-window-group-start` sets the start position of the entire
    group. This condition holds when the buffer local variable
    `set-window-group-start-function` is set to a function. In this
    case, `set-window-group-start` calls the function with the three
    arguments window, position, and noforce, then
    returns its result. The arguments position and noforce in
    this function have the same meaning as in `set-window-start`.

Function: **pos-visible-in-window-p** &optional position window partially [¶](#index-pos_002dvisible_002din_002dwindow_002dp)
:   This function returns non-`nil` if position is within the
    range of text currently visible on the screen in window. It
    returns `nil` if position is scrolled vertically out of
    view. Locations that are partially obscured are not considered
    visible unless partially is non-`nil`. The argument
    position defaults to the current position of point in
    window; window defaults to the selected window. If
    position is `t`, that means to check either the first
    visible position of the last screen line in window, or the
    end-of-buffer position, whichever comes first.

    This function considers only vertical scrolling. If position is
    out of view only because window has been scrolled horizontally,
    `pos-visible-in-window-p` returns non-`nil` anyway.
    See [Horizontal Scrolling](#Horizontal-Scrolling).

    If position is visible, `pos-visible-in-window-p` returns
    `t` if partially is `nil`; if partially is
    non-`nil`, and the character following position is fully
    visible, it returns a list of the form `(x y)`, where
    x and y are the pixel coordinates relative to the top left
    corner of the window; otherwise it returns an extended list of the form
    `(x y rtop rbot rowh vpos)`,
    where rtop and rbot specify the number of off-window pixels
    at the top and bottom of the row at position, rowh specifies
    the visible height of that row, and vpos specifies the vertical
    position (zero-based row number) of that row.

    Here is an example:

    ```
    ;; If point is off the screen now, recenter it now.
    (or (pos-visible-in-window-p
         (point) (selected-window))
        (recenter 0))
    ```

Function: **pos-visible-in-window-group-p** &optional position window partially [¶](#index-pos_002dvisible_002din_002dwindow_002dgroup_002dp)
:   This function is like `pos-visible-in-window-p`, except that when
    window is a part of a group of windows (see [Window Group](#Window-Group)),
    `pos-visible-in-window-group-p` tests the visibility of pos
    in the entire group, not just in the single window. This
    condition holds when the buffer local variable
    `pos-visible-in-window-group-p-function` is set to a function.
    In this case `pos-visible-in-window-group-p` calls the function
    with the three arguments position, window, and
    partially, then returns its result. The arguments
    position and partially have the same meaning as in
    `pos-visible-in-window-p`.

Function: **window-line-height** &optional line window [¶](#index-window_002dline_002dheight)
:   This function returns the height of text line line in
    window. If line is one of `header-line` or
    `mode-line`, `window-line-height` returns information about
    the corresponding line of the window. Otherwise, line is a text
    line number starting from 0. A negative number counts from the end of
    the window. The default for line is the current line in
    window; the default for window is the selected window.

    If the display is not up to date, `window-line-height` returns
    `nil`. In that case, `pos-visible-in-window-p` may be used
    to obtain related information.

    If there is no line corresponding to the specified line,
    `window-line-height` returns `nil`. Otherwise, it returns
    a list `(height vpos ypos offbot)`,
    where height is the height in pixels of the visible part of the
    line, vpos and ypos are the vertical position in lines and
    pixels of the line relative to the top of the first text line, and
    offbot is the number of off-window pixels at the bottom of the
    text line. If there are off-window pixels at the top of the (first)
    text line, ypos is negative.

---

Next: [Vertical Fractional Scrolling](#Vertical-Scrolling), Previous: [The Window Start and End Positions](#Window-Start-and-End), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.23 Textual Scrolling [¶](#Textual-Scrolling-1)

*Textual scrolling* means moving the text up or down through a
window. It works by changing the window’s display-start location. It
may also change the value of `window-point` to keep point on the
screen (see [Windows and Point](#Window-Point)).

The basic textual scrolling functions are `scroll-up` (which
scrolls forward) and `scroll-down` (which scrolls backward). In
these function names, “up” and “down” refer to the direction of
motion of the buffer text relative to the window. Imagine that the
text is written on a long roll of paper and that the scrolling
commands move the paper up and down. Thus, if you are looking at the
middle of a buffer and repeatedly call `scroll-down`, you will
eventually see the beginning of the buffer.

Unfortunately, this sometimes causes confusion, because some people
tend to think in terms of the opposite convention: they
imagine the window moving over text that remains in place, so that
“down” commands take you to the end of the buffer. This convention
is consistent with fact that such a command is bound to a key named
`PageDown` on modern keyboards.

Textual scrolling functions (aside from `scroll-other-window`)
have unpredictable results if the current buffer is not the one
displayed in the selected window. See [The Current Buffer](#Current-Buffer).

If the window contains a row taller than the height of the window
(for example in the presence of a large image), the scroll functions
will adjust the window’s vertical scroll position to scroll the
partially visible row. Lisp callers can disable this feature by
binding the variable `auto-window-vscroll` to `nil`
(see [Vertical Fractional Scrolling](#Vertical-Scrolling)).

Command: **scroll-up** &optional count [¶](#index-scroll_002dup)
:   This function scrolls forward by count lines in the selected
    window.

    If count is negative, it scrolls backward instead. If
    count is `nil` (or omitted), the distance scrolled is
    `next-screen-context-lines` lines less than the height of the
    window’s body.

    If the selected window cannot be scrolled any further, this function
    signals an error. Otherwise, it returns `nil`.

Command: **scroll-down** &optional count [¶](#index-scroll_002ddown)
:   This function scrolls backward by count lines in the selected
    window.

    If count is negative, it scrolls forward instead. In other
    respects, it behaves the same way as `scroll-up` does.

Command: **scroll-up-command** &optional count [¶](#index-scroll_002dup_002dcommand)
:   This behaves like `scroll-up`, except that if the selected window
    cannot be scrolled any further and the value of the variable
    `scroll-error-top-bottom` is `t`, it tries to move to the
    end of the buffer instead. If point is already there, it signals an
    error.

Command: **scroll-down-command** &optional count [¶](#index-scroll_002ddown_002dcommand)
:   This behaves like `scroll-down`, except that if the selected
    window cannot be scrolled any further and the value of the variable
    `scroll-error-top-bottom` is `t`, it tries to move to the
    beginning of the buffer instead. If point is already there, it
    signals an error.

Command: **scroll-other-window** &optional count [¶](#index-scroll_002dother_002dwindow)
:   This function scrolls the text in another window upward count
    lines. Negative values of count, or `nil`, are handled
    as in `scroll-up`.

    You can specify which buffer to scroll by setting the variable
    `other-window-scroll-buffer` to a buffer. If that buffer isn’t
    already displayed, `scroll-other-window` displays it in some
    window.

    When the selected window is the minibuffer, the next window is normally
    the leftmost one immediately above it. You can specify a different
    window to scroll, when the minibuffer is selected, by setting the variable
    `minibuffer-scroll-window`. This variable has no effect when any
    other window is selected. When it is non-`nil` and the
    minibuffer is selected, it takes precedence over
    `other-window-scroll-buffer`. See [Definition of minibuffer-scroll-window](#Definition-of-minibuffer_002dscroll_002dwindow).

Command: **scroll-other-window-down** &optional count [¶](#index-scroll_002dother_002dwindow_002ddown)
:   This function scrolls the text in another window downward count
    lines. Negative values of count, or `nil`, are handled as
    in `scroll-down`. In other respects, it behaves the same way as
    `scroll-other-window` does.

Variable: **other-window-scroll-buffer** [¶](#index-other_002dwindow_002dscroll_002dbuffer)
:   If this variable is non-`nil`, it tells `scroll-other-window`
    which buffer’s window to scroll.

User Option: **scroll-margin** [¶](#index-scroll_002dmargin)
:   This option specifies the size of the scroll margin—a minimum number
    of lines between point and the top or bottom of a window. Whenever
    point gets within this many lines of the top or bottom of the window,
    redisplay scrolls the text automatically (if possible) to move point
    out of the margin, closer to the center of the window.

User Option: **maximum-scroll-margin** [¶](#index-maximum_002dscroll_002dmargin)
:   This variable limits the effective value of `scroll-margin` to a
    fraction of the current window line height. For example, if the
    current window has 20 lines and `maximum-scroll-margin` is 0.1,
    then the scroll margins will never be larger than 2 lines, no matter
    how big `scroll-margin` is.

    `maximum-scroll-margin` itself has a maximum value of 0.5, which
    allows setting margins large to keep the cursor at the middle line of
    the window (or two middle lines if the window has an even number of
    lines). If it’s set to a larger value (or any value other than a
    float between 0.0 and 0.5) then the default value of 0.25 will be used
    instead.

User Option: **scroll-conservatively** [¶](#index-scroll_002dconservatively)
:   This variable controls how scrolling is done automatically when point
    moves off the screen (or into the scroll margin). If the value is a
    positive integer n, then redisplay scrolls the text up to
    n lines in either direction, if that will bring point back into
    proper view. This behavior is called *conservative scrolling*.
    Otherwise, scrolling happens in the usual way, under the control of
    other variables such as `scroll-up-aggressively` and
    `scroll-down-aggressively`.

    The default value is zero, which means that conservative scrolling
    never happens.

User Option: **scroll-down-aggressively** [¶](#index-scroll_002ddown_002daggressively)
:   The value of this variable should be either `nil` or a fraction
    f between 0 and 1. If it is a fraction, that specifies where on
    the screen to put point when scrolling down. More precisely, when a
    window scrolls down because point is above the window start, the new
    start position is chosen to put point f part of the window
    height from the top. The larger f, the more aggressive the
    scrolling.

    A value of `nil` is equivalent to .5, since its effect is to center
    point. This variable automatically becomes buffer-local when set in any
    fashion.

User Option: **scroll-up-aggressively** [¶](#index-scroll_002dup_002daggressively)
:   Likewise, for scrolling up. The value, f, specifies how far
    point should be placed from the bottom of the window; thus, as with
    `scroll-down-aggressively`, a larger value scrolls more
    aggressively.

User Option: **scroll-step** [¶](#index-scroll_002dstep)
:   This variable is an older variant of `scroll-conservatively`.
    The difference is that if its value is n, that permits scrolling
    only by precisely n lines, not a smaller number. This feature
    does not work with `scroll-margin`. The default value is zero.

User Option: **scroll-preserve-screen-position** [¶](#index-scroll_002dpreserve_002dscreen_002dposition)
:   If this option is `t`, whenever a scrolling command moves point
    off-window, Emacs tries to adjust point to keep the cursor at its old
    vertical position in the window, rather than the window edge.

    If the value is non-`nil` and not `t`, Emacs adjusts point
    to keep the cursor at the same vertical position, even if the
    scrolling command didn’t move point off-window.

    This option affects all scroll commands that have a non-`nil`
    `scroll-command` symbol property.

User Option: **next-screen-context-lines** [¶](#index-next_002dscreen_002dcontext_002dlines)
:   The value of this variable is the number of lines of continuity to
    retain when scrolling by full screens. For example, `scroll-up`
    with an argument of `nil` scrolls so that this many lines at the
    bottom of the window appear instead at the top. The default value is
    `2`.

User Option: **scroll-error-top-bottom** [¶](#index-scroll_002derror_002dtop_002dbottom)
:   If this option is `nil` (the default), `scroll-up-command`
    and `scroll-down-command` simply signal an error when no more
    scrolling is possible.

    If the value is `t`, these commands instead move point to the
    beginning or end of the buffer (depending on scrolling direction);
    only if point is already on that position do they signal an error.

Command: **recenter** &optional count redisplay [¶](#index-recenter)
:   This function scrolls the text in the selected window so that point is
    displayed at a specified vertical position within the window. It does
    not move point with respect to the text.

    If count is a non-negative number, that puts the line containing
    point count lines down from the top of the window. If
    count is a negative number, then it counts upward from the
    bottom of the window, so that −1 stands for the last usable
    line in the window.

    If count is `nil` (or a non-`nil` list),
    `recenter` puts the line containing point in the middle of the
    window. If count is `nil` and redisplay is
    non-`nil`, this function may redraw the frame, according to the
    value of `recenter-redisplay`. Thus, omitting the second
    argument can be used to countermand the effect of
    `recenter-redisplay` being non-`nil`. Interactive calls
    pass non-`nil` for redisplay.

    When `recenter` is called interactively, count is the raw
    prefix argument. Thus, typing `C-u` as the prefix sets the
    count to a non-`nil` list, while typing `C-u 4` sets
    count to 4, which positions the current line four lines from the
    top.

    With an argument of zero, `recenter` positions the current line at
    the top of the window. The command `recenter-top-bottom` offers
    a more convenient way to achieve this.

Function: **recenter-window-group** &optional count [¶](#index-recenter_002dwindow_002dgroup)
:   This function is like `recenter`, except that when the selected
    window is part of a group of windows (see [Window Group](#Window-Group)),
    `recenter-window-group` scrolls the entire group. This condition
    holds when the buffer local variable
    `recenter-window-group-function` is set to a function. In this
    case, `recenter-window-group` calls the function with the
    argument count, then returns its result. The argument
    count has the same meaning as in `recenter`, but with
    respect to the entire window group.

User Option: **recenter-redisplay** [¶](#index-recenter_002dredisplay)
:   If this variable is non-`nil`, calling `recenter` with a
    `nil` count argument and non-`nil` redisplay
    argument redraws the frame. The default value is `tty`, which
    means only redraw the frame if it is a tty frame.

Command: **recenter-top-bottom** &optional count [¶](#index-recenter_002dtop_002dbottom)
:   This command, which is the default binding for `C-l`, acts like
    `recenter`, except if called with no argument. In that case,
    successive calls place point according to the cycling order defined
    by the variable `recenter-positions`.

User Option: **recenter-positions** [¶](#index-recenter_002dpositions)
:   This variable controls how `recenter-top-bottom` behaves when
    called with no argument. The default value is `(middle top
    bottom)`, which means that successive calls of
    `recenter-top-bottom` with no argument cycle between placing
    point at the middle, top, and bottom of the window.

---

Next: [Horizontal Scrolling](#Horizontal-Scrolling), Previous: [Textual Scrolling](#Textual-Scrolling), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
