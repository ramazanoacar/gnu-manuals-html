### 29.10 Resurrecting Windows [¶](#Resurrecting-Windows-1)

After a window has been deleted (see [Deleting Windows](#Deleting-Windows)) it cannot be
used any more by functions that require a valid window as their argument
even if some Lisp variable still references that window. When the last
reference to a window has ceased to exist, the window’s Lisp object will
be eventually recycled by the garbage collector (see [Garbage Collection](#Garbage-Collection)).

There are two ways to resurrect a deleted window whose object has not
been yet recycled by the collector: The first is to keep a reference to
that window in a saved window configuration (see [Window Configurations](#Window-Configurations)) and then call `set-window-configuration` with that
configuration as argument. The second one is to keep a reference to
that window in a variable or let-bind it and then use that reference as
refer argument in `split-window` (see [Splitting Windows](#Splitting-Windows)).

The major difference between these two is that
`set-window-configuration` restores the frame layout as it existed
before deleting the window. The `split-window` approach, on the
other hand, allows for arbitrary variations of the layout.

Consider the following example starting with a frame containing a single
window showing the buffer `*scratch*`:

```
(let* ((old (selected-window))
       (new (split-window old nil 'right))
       overlay)
  (with-current-buffer (get-buffer-create "*Messages*")
    (set-window-buffer new (current-buffer))
    (setq overlay (make-overlay (point-min) (point-max)))
    (overlay-put overlay 'face 'highlight)
    (overlay-put overlay 'window new)
    (message "new %s parent %s" new (window-parent new))
    (sit-for 3))
  (delete-window new)
  (setq new (split-window old nil 'left))
  (set-window-buffer new (get-buffer-create "*Messages*"))
  (format "new %s parent %s" new (window-parent new)))
```

When you run that code in `*scratch*` it will first split the
window showing `*scratch*` to display `*Messages*` in a new
window on the right. It also sets up an overlay with a window property
to highlight the text of `*Messages*` in the new window and
displays a message showing the new window and its parent in the window
tree. It then deletes the new window and resurrects it on the left of
the `*scratch*` window again displaying a message showing the new
window and its parent in the window tree.

Note that both, new window and its parent have changed after moving the
`*Messages*` window to the left. Also, the highlighting disappears
because any properties set up for the new window on the right are lost
when the new window is re-created on the left.

The following code uses the refer argument of `split-window`
instead.

```
(let* ((old (selected-window))
       (new (split-window old nil 'right))
       overlay)
  (with-current-buffer (get-buffer-create "*Messages*")
    (set-window-buffer new (current-buffer))
    (setq overlay (make-overlay (point-min) (point-max)))
    (overlay-put overlay 'face 'highlight)
    (overlay-put overlay 'window new)
    (message "new %s parent %s" new (window-parent new))
    (sit-for 3))
  (delete-window new)
  (split-window old nil 'left nil new)
  (format "new %s parent %s" new (window-parent new)))
```

Note that all properties of the resurrected window like its decorations,
parameters as well as any overlays with a window property are preserved
as if that window had never been deleted. The only things that changed
are its position in the window tree and consequently the values returned
by `window-left-child` of its parent window as well as the values
returned by `window-prev-sibling` and `window-next-sibling` of
the window and its sibling.

The following code passes both, the new window on the right and its
parent, via the refer argument to `split-window` instead.

```
(let* ((old (selected-window))
       (new (split-window old nil 'right))
       (parent (window-parent new))
       overlay)
  (with-current-buffer (get-buffer-create "*Messages*")
    (set-window-buffer new (current-buffer))
    (setq overlay (make-overlay (point-min) (point-max)))
    (overlay-put overlay 'face 'highlight)
    (overlay-put overlay 'window new)
    (message "new %s parent %s" new (window-parent new))
    (sit-for 3))
  (delete-window new)
  (split-window old nil 'left nil (cons new parent))
  (format "new %s parent %s" new (window-parent new)))
```

Note that the parent window has been resurrected along with the new
window.

Resurrecting dead windows is useful to preserve the identity of windows
in actions that are supposed to do that like moving windows around on a
frame or hiding them temporarily. Any properties of such a window like
its decorations, the buffer it has shown previously, that buffer’s start
and point position in the window, the window’s dedicated status or its
cursor type are left untouched and there’s no need to recreate them from
scratch. For internal windows, the value of that window’s combination
limit is preserved which means that the window can be recombined
(see [Recombining Windows](#Recombining-Windows)) as before.

---

Next: [Cyclic Ordering of Windows](#Cyclic-Window-Ordering), Previous: [Resurrecting Windows](#Resurrecting-Windows), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.11 Changing Window Layouts [¶](#Changing-Window-Layouts-1)

Sometimes it can be useful to change the layout of windows such that two
or more windows occupy the locations of each other. The commands
described in this section do that in an orderly manner. Conceptually,
these commands affect a rectangular subset of the windows on a frame.
As a rule, they neither affect the location of a frame’s minibuffer
window nor of any of its side windows (see [Side Windows](#Side-Windows)).

Common to all these commands is that they preserve the identity of
windows by deleting them first and resurrecting them
(see [Resurrecting Windows](#Resurrecting-Windows)) in their new locations afterwards. No
window gets lost and no window is added or duplicated. Where possible,
these commands try to preserve the relative size of windows as well as
all other non-geometric properties including parameters. These commands
will signal an error if windows about to change their locations are
atomic or of fixed size. The option `transpose-dedicated-windows`
described below decides whether dedicated windows may change their
locations.

The window argument of all functions described in this section
must specify a valid parent window (see [Windows and Frames](#Windows-and-Frames)). If it
is `nil`, it stands for the main window (see [Side Windows](#Side-Windows)) of
the selected frame. Interactively, a prefix argument means to operate
on the parent window of the selected window. In the examples given
below, we shall always assume that the windows displayed constitute
their frame’s main window and the window argument is `nil`
unless stated otherwise.

The first two commands *rotate a window layout* either clockwise
or counterclockwise.

Command: **window-layout-rotate-clockwise** &optional window [¶](#index-window_002dlayout_002drotate_002dclockwise)
:   This command rotates the window layout clockwise by 90 degrees. Imagine
    a layout with three live windows A, B and C as
    depicted on the left below. Then this command will produce the layout
    on the right.

    ```
         ___________       ___________
        |           |     |     |     |
        |     A     |     |  B  |     |
        |___________| --> |_____|  A  |
        |     |     |     |     |     |
        |  B  |  C  |     |  C  |     |
        |_____|_____|     |_____|_____|
    ```

Command: **window-layout-rotate-anticlockwise** &optional window [¶](#index-window_002dlayout_002drotate_002danticlockwise)
:   This is like `window-layout-rotate-clockwise` but rotates the
    layout in the opposite direction as demonstrated in the example below.

    ```
         ___________       ___________
        |           |     |     |     |
        |     A     |     |     |  C  |
        |___________| --> |  A  |_____|
        |     |     |     |     |     |
        |  B  |  C  |     |     |  B  |
        |_____|_____|     |_____|_____|
    ```

The next two commands *flip the window layout*—rotate it around an
imaginary horizontal or vertical axis. If the number of windows to be
flipped is odd, the window in the middle remains in its original place.

Command: **window-layout-flip-topdown** &optional window [¶](#index-window_002dlayout_002dflip_002dtopdown)
:   This command flips windows such that windows on the bottom become
    windows on the top and vice-versa as in the example below.

    ```
         ___________       ___________
        |           |     |     |     |
        |     A     |     |  B  |  C  |
        |___________| --> |_____|_____|
        |     |     |     |           |
        |  B  |  C  |     |     A     |
        |_____|_____|     |___________|
    ```

Command: **window-layout-flip-leftright** &optional window [¶](#index-window_002dlayout_002dflip_002dleftright)
:   This command rearranges window in a way that the windows on the right
    become the windows on the left, and vice-versa.

    ```
         ___________        ___________
        |           |     |           |
        |     A     |     |     A     |
        |___________| --> |___________|
        |     |     |     |     |     |
        |  B  |  C  |     |  C  |  B  |
        |_____|_____|     |_____|_____|
    ```

The next command can be used to *transpose windows*—changing
horizontal splits to vertical ones and vice-versa.

Command: **window-layout-transpose** &optional window [¶](#index-window_002dlayout_002dtranspose)
:   This command reorganizes windows such that every horizontal split
    becomes a vertical split and vice versa.

    ```
         ___________       ___________
        |           |     |     |     |
        |     A     |     |     |  B  |
        |___________| --> |  A  |_____|
        |     |     |     |     |     |
        |  B  |  C  |     |     |  C  |
        |_____|_____|     |_____|_____|
    ```

The final two commands can be used to *rotate windows* within the
existing layout. They are like the commands that rotate the layout but
leave the underlying structure of the layout unchanged. What actually
changes are the positions of windows within the existing layout. This
happens in a way that the space formerly occupied by any window will be
occupied by the window preceding (following if reverse is
non-`nil`) it in the cyclic ordering of windows (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)) on window’s frame.

Command: **rotate-windows** &optional window reverse [¶](#index-rotate_002dwindows)
:   This command rotates window’s child windows in cyclic ordering.
    The optional argument reverse means to rotate windows backwards,
    in reverse cyclic order.

    ```
         ___________        ___________
        |           |      |           |
        |     A     |      |     C     |
        |___________| -->  |___________|
        |     |     |      |     |     |
        |  B  |  C  |      |  A  |  B  |
        |_____|_____|      |_____|_____|
    ```

Command: **rotate-windows-back** &optional window [¶](#index-rotate_002dwindows_002dback)
:   This command rotates window’s child windows backwards in cyclic
    ordering.

    ```
         ___________        ___________
        |           |      |           |
        |     A     |      |     B     |
        |___________| -->  |___________|
        |     |     |      |     |     |
        |  B  |  C  |      |  C  |  A  |
        |_____|_____|      |_____|_____|
    ```

All commands described above are subject to the following option.

User Option: **transpose-dedicated-windows** [¶](#index-transpose_002ddedicated_002dwindows)
:   If this is `nil`, the functions described in this section will
    report an error when they encounter a dedicated window (see [Dedicated Windows](#Dedicated-Windows)). If this is `t`, they handle dedicated windows like
    non-dedicated ones.

The last two commands are subject to the following option.

User Option: **rotate-windows-change-selected** [¶](#index-rotate_002dwindows_002dchange_002dselected)
:   If this is `nil`, the selected window will remain unaffected by
    `rotate-windows` and `rotate-windows-back`. If this is
    `t` (the default), the selected window will change to the window
    that appears at the location of the window selected before any of these
    commands were invoked.

---

Next: [Buffers and Windows](#Buffers-and-Windows), Previous: [Changing Window Layouts](#Changing-Window-Layouts), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.12 Cyclic Ordering of Windows [¶](#Cyclic-Ordering-of-Windows)

When you use the command `C-x o` (`other-window`) to select
some other window, it moves through live windows in a specific order.
For any given configuration of windows, this order never varies. It
is called the *cyclic ordering of windows*.

The ordering is determined by a depth-first traversal of each frame’s
window tree, retrieving the live windows which are the leaf nodes of the
tree (see [Windows and Frames](#Windows-and-Frames)). If the minibuffer is active, the
minibuffer window is included too. The ordering is cyclic, so the last
window in the sequence is followed by the first one.

Function: **next-window** &optional window minibuf all-frames [¶](#index-next_002dwindow)
:   This function returns a live window, the one following window in
    the cyclic ordering of windows. window should be a live window;
    if omitted or `nil`, it defaults to the selected window.

    The optional argument minibuf specifies whether minibuffer windows
    should be included in the cyclic ordering. Normally, when minibuf
    is `nil`, a minibuffer window is included only if it is currently
    active; this matches the behavior of `C-x o`. (Note that a
    minibuffer window is active as long as its minibuffer is in use; see
    [Minibuffers](#Minibuffers)).

    If minibuf is `t`, the cyclic ordering includes all
    minibuffer windows. If minibuf is neither `t` nor
    `nil`, minibuffer windows are not included even if they are active.

    The optional argument all-frames specifies which frames to
    consider:

    * `nil`
      means to consider windows on window’s frame. If the minibuffer
      window is considered (as specified by the minibuf argument),
      then frames that share the minibuffer window are considered too.
    * `t`
      means to consider windows on all existing frames.
    * `visible`
      means to consider windows on all visible frames.
    * 0
      means to consider windows on all visible or iconified frames.
    * A frame
      means to consider windows on that specific frame.
    * Anything else
      means to consider windows on window’s frame, and no others.

    If more than one frame is considered, the cyclic ordering is obtained
    by appending the orderings for those frames, in the same order as the
    list of all live frames (see [Finding All Frames](#Finding-All-Frames)).

Function: **previous-window** &optional window minibuf all-frames [¶](#index-previous_002dwindow)
:   This function returns a live window, the one preceding window in
    the cyclic ordering of windows. The other arguments are handled like
    in `next-window`.

Command: **other-window** count &optional all-frames [¶](#index-other_002dwindow)
:   This function selects a live window, one count places from the
    selected window in the cyclic ordering of windows. If count is
    a positive number, it skips count windows forwards; if
    count is negative, it skips −count windows
    backwards; if count is zero, that simply re-selects the selected
    window. When called interactively, count is the numeric prefix
    argument.

    The optional argument all-frames has the same meaning as in
    `next-window`, like a `nil` minibuf argument to
    `next-window`. When called interactively, all-frames is
    always `nil`, so only windows on the selected frame can be
    selected.

    If the `other-window` parameter of the selected window is a
    function, and `ignore-window-parameters` is `nil`, that
    function will be called with the arguments count and
    all-frames instead of the normal operation of this function.

    Otherwise, this function does not select a window for which the function
    `window-no-other-p` (see next) returns `nil`.

Function: **window-no-other-p** &optional window [¶](#index-window_002dno_002dother_002dp)
:   This function returns non-`nil` if window should not be used
    as “other” window by commands like `other-window` or functions
    like `get-lru-window` (see below). It’s also called when deleting
    the selected window (see [Deleting Windows](#Deleting-Windows)) and a new selected window
    has to be chosen.

    More precisely, this function returns non-`nil` if the
    `no-other-window` parameter of window is non-`nil` and
    `ignore-window-parameters` is `nil`. It returns `nil` in
    any other case.

    Note that if this function returns `nil`, this does not necessarily
    mean that window can be selected. A tooltip window (see [Basic Concepts of Emacs Windows](#Basic-Windows)), for example, has its `no-other-window` parameter set to
    `t` by default but cannot be selected.

Function: **walk-windows** fun &optional minibuf all-frames [¶](#index-walk_002dwindows)
:   This function calls the function fun once for each live window,
    with the window as the argument.

    It follows the cyclic ordering of windows. The optional arguments
    minibuf and all-frames specify the set of windows
    included; these have the same arguments as in `next-window`. If
    all-frames specifies a frame, the first window walked is the
    first window on that frame (the one returned by
    `frame-first-window`), not necessarily the selected window.

    If fun changes the window configuration by splitting or deleting
    windows, that does not alter the set of windows walked, which is
    determined prior to calling fun for the first time.

Function: **one-window-p** &optional no-mini all-frames [¶](#index-one_002dwindow_002dp)
:   This function returns `t` if the selected window is the only live
    window, and `nil` otherwise.

    If the minibuffer window is active, it is normally considered (so that
    this function returns `nil`). However, if the optional argument
    no-mini is non-`nil`, the minibuffer window is ignored even
    if active. The optional argument all-frames has the same
    meaning as for `next-window`.

The following functions return a window which satisfies some
criterion, without selecting it:

Function: **get-lru-window** &optional all-frames dedicated not-selected no-other [¶](#index-get_002dlru_002dwindow)
:   This function returns a live window which is heuristically the least
    recently used one. The *least recently used window* is the least
    recently selected one—the window whose use time is less than the use
    time of all other live windows (see [Selecting Windows](#Selecting-Windows)). The
    optional argument all-frames has the same meaning as in
    `next-window`.

    If any full-width windows are present, only those windows are
    considered. A minibuffer window is never a candidate. A dedicated
    window (see [Dedicated Windows](#Dedicated-Windows)) is never a candidate unless the
    optional argument dedicated is non-`nil`. The selected
    window is never returned, unless it is the only candidate. However, if
    the optional argument not-selected is non-`nil`, this
    function returns `nil` in that case. The optional argument
    no-other, if non-`nil`, means to never return a window for
    which `window-no-other-p` (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)) returns
    non-`nil`.

Function: **get-mru-window** &optional all-frames dedicated not-selected no-other [¶](#index-get_002dmru_002dwindow)
:   This function is like `get-lru-window`, but it returns the most
    recently used window instead. The *most recently used window* is
    the most recently selected one—the window whose use time exceeds the
    use time of all other live windows (see [Selecting Windows](#Selecting-Windows)). The
    meaning of the arguments is the same as for `get-lru-window`.

    Since in practice the most recently used window is always the selected
    one, it usually makes sense to call this function with a non-`nil`
    not-selected argument only.

Function: **get-largest-window** &optional all-frames dedicated not-selected no-other [¶](#index-get_002dlargest_002dwindow)
:   This function returns the window with the largest area (height times
    width). If there are two candidate windows of the same size, it prefers
    the one that comes first in the cyclic ordering of windows, starting
    from the selected window. The meaning of the arguments is the same as
    for `get-lru-window`.

Function: **get-window-with-predicate** predicate &optional minibuf all-frames default [¶](#index-get_002dwindow_002dwith_002dpredicate)
:   This function calls the function predicate for each of the
    windows in the cyclic order of windows in turn, passing it the window
    as an argument. If the predicate returns non-`nil` for any
    window, this function stops and returns that window. If no such
    window is found, the return value is default (which defaults to
    `nil`).

    The optional arguments minibuf and all-frames specify the
    windows to search, and have the same meanings as in
    `next-window`.

---

Next: [Switching to a Buffer in a Window](#Switching-Buffers), Previous: [Cyclic Ordering of Windows](#Cyclic-Window-Ordering), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.13 Buffers and Windows [¶](#Buffers-and-Windows-1)

This section describes low-level functions for examining and setting
the contents of windows. See [Switching to a Buffer in a Window](#Switching-Buffers), for higher-level
functions for displaying a specific buffer in a window.

Function: **window-buffer** &optional window [¶](#index-window_002dbuffer)
:   This function returns the buffer that window is displaying. If
    window is omitted or `nil` it defaults to the selected
    window. If window is an internal window, this function returns
    `nil`.

Function: **set-window-buffer** window buffer-or-name &optional keep-margins [¶](#index-set_002dwindow_002dbuffer)
:   This function makes window display buffer-or-name.
    window should be a live window; if `nil`, it defaults to
    the selected window. buffer-or-name should be a buffer, or the
    name of an existing buffer. This function does not change which
    window is selected, nor does it directly change which buffer is
    current (see [The Current Buffer](#Current-Buffer)). Its return value is `nil`.

    If window is *strongly dedicated* to a buffer and
    buffer-or-name does not specify that buffer, this function
    signals an error. See [Dedicated Windows](#Dedicated-Windows).

    By default, this function resets window’s position, display
    margins, fringe widths, and scroll bar settings, based on the local
    variables in the specified buffer. However, if the optional argument
    keep-margins is non-`nil`, it leaves window’s display
    margins, fringes and scroll bar settings alone.

    When writing an application, you should normally use
    `display-buffer` (see [Choosing a Window for Displaying a Buffer](#Choosing-Window)) or the higher-level
    functions described in [Switching to a Buffer in a Window](#Switching-Buffers), instead of calling
    `set-window-buffer` directly.

    This runs `window-scroll-functions`, followed by
    `window-configuration-change-hook`. See [Hooks for Window Scrolling and Changes](#Window-Hooks).

Variable: **buffer-display-count** [¶](#index-buffer_002ddisplay_002dcount)
:   This buffer-local variable records the number of times a buffer has been
    displayed in a window. It is incremented each time
    `set-window-buffer` is called for the buffer.

Variable: **buffer-display-time** [¶](#index-buffer_002ddisplay_002dtime)
:   This buffer-local variable records the time at which a buffer was last
    displayed in a window. The value is `nil` if the buffer has
    never been displayed. It is updated each time
    `set-window-buffer` is called for the buffer, with the value
    returned by `current-time` (see [Time of Day](#Time-of-Day)).

Function: **get-buffer-window** &optional buffer-or-name all-frames [¶](#index-get_002dbuffer_002dwindow)
:   This function returns the first window displaying buffer-or-name
    in the cyclic ordering of windows, starting from the selected window
    (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)). If no such window exists, the
    return value is `nil`.

    buffer-or-name should be a buffer or the name of a buffer; if
    omitted or `nil`, it defaults to the current buffer. The
    optional argument all-frames specifies which windows to
    consider:

    * `t` means consider windows on all existing frames.
    * `visible` means consider windows on all visible frames.
    * 0 means consider windows on all visible or iconified frames.
    * A frame means consider windows on that frame only.
    * Any other value means consider windows on the selected frame.

    Note that these meanings differ slightly from those of the
    all-frames argument to `next-window` (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)). This function may be changed in a future version of Emacs
    to eliminate this discrepancy.

The following function can tell for a specific window whether its buffer
shares the text of some other buffer (see [Indirect Buffers](#Indirect-Buffers)).

Function: **window-indirect-buffer-p** &optional window buffer-or-name [¶](#index-window_002dindirect_002dbuffer_002dp)
:   This function returns non-`nil` if window is indirectly
    related to buffer-or-name. window must be a live window and
    defaults to the selected window. buffer-or-name may be a buffer
    or the name of an existing buffer and defaults to the current buffer.

    window is indirectly related to buffer-or-name if one of the
    following conditions hold:

    * buffer-or-name specifies an indirect buffer and window’s
      buffer is its base buffer.
    * window’s buffer is an indirect buffer whose base buffer is the
      buffer specified by buffer-or-name.
    * Both, window’s buffer and the buffer specified by
      buffer-or-name, are indirect buffer’s sharing the same base
      buffer.

    It returns `nil` if none of the above holds.

Function: **get-buffer-window-list** &optional buffer-or-name minibuf all-frames indirect [¶](#index-get_002dbuffer_002dwindow_002dlist)
:   This function returns a list of all windows currently displaying
    buffer-or-name. buffer-or-name should be a buffer or the
    name of an existing buffer. If omitted or `nil`, it defaults to
    the current buffer. If the currently selected window displays
    buffer-or-name, it will be the first in the list returned by
    this function.

    The arguments minibuf and all-frames have the same
    meanings as in the function `next-window` (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)). Note that the all-frames argument does *not*
    behave exactly like in `get-buffer-window`.

    The optional argument indirect non-`nil` means to append to
    the list of windows showing buffer-or-name a list of all windows
    that are indirectly related to buffer-or-name, that is, windows
    for which `window-indirect-buffer-p` (see above) with the window
    and the buffer specified by buffer-or-name as arguments returns
    non-`nil`.

Command: **replace-buffer-in-windows** &optional buffer-or-name [¶](#index-replace_002dbuffer_002din_002dwindows)
:   This command replaces buffer-or-name with some other buffer, in
    all windows displaying it. buffer-or-name should be a buffer, or
    the name of an existing buffer; if omitted or `nil`, it defaults to
    the current buffer.

    The replacement buffer in each window is usually chosen via
    `switch-to-prev-buffer` (see [Window History](#Window-History)). With the
    exception of side windows (see [Side Windows](#Side-Windows)), any dedicated window
    displaying buffer-or-name is deleted if possible (see [Dedicated Windows](#Dedicated-Windows)). If such a window is the only window on its frame and there
    are other frames on the same terminal, the frame is deleted as well. If
    the dedicated window is the only window on the only frame on its
    terminal, the buffer is replaced anyway.

    The main purpose of this function is to decide what to do with windows
    whose buffers are about to be killed by `kill-buffer`
    (see [Killing Buffers](#Killing-Buffers)). It will, however, also remove the buffer
    specified by buffer-or-name from the lists of previous and next
    buffers (see [Window History](#Window-History)) of all windows (including dead windows
    that are only referenced by window configurations) and remove any
    `quit-restore` or `quit-restore-prev` parameters
    (see [Window Parameters](#Window-Parameters)) referencing that buffer.

    This function does not replace the buffer specified by
    buffer-or-name in any minibuffer window showing it, nor does it
    delete minibuffer windows or minibuffer frames. It removes, however,
    that buffer from the lists of previous and next buffers of all
    minibuffer windows.

By default, `replace-buffer-in-windows` deletes only windows
dedicated to their buffers and ignores any `quit-restore` or
`quit-restore-prev` parameters of the windows it works on. The
following option is useful for circumventing these restrictions.

User Option: **kill-buffer-quit-windows** [¶](#index-kill_002dbuffer_002dquit_002dwindows)
:   If this option is `nil`, `kill-buffer` (and in consequence
    `replace-buffer-in-windows`) may only delete windows that are
    dedicated to the buffer about to be killed. If this is non-`nil`,
    `replace-buffer-in-windows` has `quit-restore-window`
    (see [Quitting Windows](#Quitting-Windows)) deal with any such window. That function may
    delete such a window even if it’s not dedicated to its buffer. Also,
    `delete-windows-on` will use `quit-restore-window` as fallback
    when a window cannot be deleted and another buffer must be shown in it.

---

Next: [Displaying a Buffer in a Suitable Window](#Displaying-Buffers), Previous: [Buffers and Windows](#Buffers-and-Windows), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 29.14 Switching to a Buffer in a Window [¶](#Switching-to-a-Buffer-in-a-Window)

This section describes high-level functions for switching to a specified
buffer in some window. In general, “switching to a buffer” means to
(1) show the buffer in some window, (2) make that window the selected
window (and its frame the selected frame), and (3) make the buffer the
current buffer.

Do *not* use these functions to make a buffer temporarily
current just so a Lisp program can access or modify it. They have
side-effects, such as changing window histories (see [Window History](#Window-History)), which will surprise the user if used that way. If you want
to make a buffer current to modify it in Lisp, use
`with-current-buffer`, `save-current-buffer`, or
`set-buffer`. See [The Current Buffer](#Current-Buffer).

Command: **switch-to-buffer** buffer-or-name &optional norecord force-same-window [¶](#index-switch_002dto_002dbuffer)
:   This command attempts to display buffer-or-name in the selected
    window and make it the current buffer. It is often used interactively
    (as the binding of `C-x b`), as well as in Lisp programs. The
    return value is the buffer switched to.

    If buffer-or-name is `nil`, it defaults to the buffer
    returned by `other-buffer` (see [The Buffer List](#Buffer-List)). If
    buffer-or-name is a string that is not the name of any existing
    buffer, this function creates a new buffer with that name; the new
    buffer’s major mode is determined by the variable `major-mode`
    (see [Major Modes](#Major-Modes)).

    Normally, the specified buffer is put at the front of the buffer
    list—both the global buffer list and the selected frame’s buffer
    list (see [The Buffer List](#Buffer-List)). However, this is not done if the
    optional argument norecord is non-`nil`.

    Sometimes, the selected window may not be suitable for displaying the
    buffer. This happens if the selected window is a minibuffer window, or
    if the selected window is strongly dedicated to its buffer
    (see [Dedicated Windows](#Dedicated-Windows)). In such cases, the command normally tries
    to display the buffer in some other window, by invoking
    `pop-to-buffer` (see below).

    If the optional argument force-same-window is non-`nil` and
    the selected window is not suitable for displaying the buffer, this
    function always signals an error when called non-interactively. In
    interactive use, if the selected window is a minibuffer window, this
    function will try to use some other window instead. If the selected
    window is strongly dedicated to its buffer, the option
    `switch-to-buffer-in-dedicated-window` described next can be used
    to proceed.

User Option: **switch-to-buffer-in-dedicated-window** [¶](#index-switch_002dto_002dbuffer_002din_002ddedicated_002dwindow)
:   This option, if non-`nil`, allows `switch-to-buffer` to
    proceed when called interactively and the selected window is strongly
    dedicated to its buffer.

    The following values are respected:

    `nil`
    :   Disallows switching and signals an error as in non-interactive use.

    `prompt`
    :   Prompts the user whether to allow switching.

    `pop`
    :   Invokes `pop-to-buffer` to proceed.

    `t`
    :   Marks the selected window as non-dedicated and proceeds.

    This option does not affect non-interactive calls of
    `switch-to-buffer`.

By default, `switch-to-buffer` tries to preserve
`window-point`. This behavior can be tuned using the following
option.

User Option: **switch-to-buffer-preserve-window-point** [¶](#index-switch_002dto_002dbuffer_002dpreserve_002dwindow_002dpoint)
:   If this variable is `nil`, `switch-to-buffer` displays the
    buffer specified by buffer-or-name at the position of that
    buffer’s `point`. If this variable is `already-displayed`, it
    tries to display the buffer at its previous position in the selected
    window, provided the buffer is currently displayed in some other window
    on any visible or iconified frame. If this variable is `t`,
    `switch-to-buffer` unconditionally tries to display the buffer at
    its previous position in the selected window.

    This variable is ignored if the buffer is already displayed in the
    selected window or never appeared in it before, or if
    `switch-to-buffer` calls `pop-to-buffer` to display the
    buffer.

User Option: **switch-to-buffer-obey-display-actions** [¶](#index-switch_002dto_002dbuffer_002dobey_002ddisplay_002dactions)
:   If this variable is non-`nil`, `switch-to-buffer` respects
    display actions specified by `display-buffer-overriding-action`,
    `display-buffer-alist` (see [Action Functions for Buffer Display](#Buffer-Display-Action-Functions))
    and other buffer display related variables (see [Action Alists for Buffer Display](#Buffer-Display-Action-Alists)).

The next two commands are similar to `switch-to-buffer`, except for
the described features.

Command: **switch-to-buffer-other-window** buffer-or-name &optional norecord [¶](#index-switch_002dto_002dbuffer_002dother_002dwindow)
:   This function displays the buffer specified by buffer-or-name in
    some window other than the selected window. It uses the function
    `pop-to-buffer` internally (see below).

    If the selected window already displays the specified buffer, it
    continues to do so, but another window is nonetheless found to display
    it as well.

    The buffer-or-name and norecord arguments have the same
    meanings as in `switch-to-buffer`.

Command: **switch-to-buffer-other-frame** buffer-or-name &optional norecord [¶](#index-switch_002dto_002dbuffer_002dother_002dframe)
:   This function displays the buffer specified by buffer-or-name in a
    new frame. It uses the function `pop-to-buffer` internally (see
    below).

    If the specified buffer is already displayed in another window, in any
    frame on the current terminal, this switches to that window instead of
    creating a new frame. However, the selected window is never used for
    this.

    The buffer-or-name and norecord arguments have the same
    meanings as in `switch-to-buffer`.

The above commands use the function `pop-to-buffer`, which
flexibly displays a buffer in some window and selects that window for
editing. In turn, `pop-to-buffer` uses `display-buffer` for
displaying the buffer. Hence, all the variables affecting
`display-buffer` will affect it as well. See [Choosing a Window for Displaying a Buffer](#Choosing-Window),
for the documentation of `display-buffer`.

Command: **pop-to-buffer** buffer-or-name &optional action norecord [¶](#index-pop_002dto_002dbuffer)
:   This function makes buffer-or-name the current buffer and
    displays it in some window, preferably not the window currently
    selected. It then selects the displaying window. If that window is
    on a different graphical frame, that frame is given input focus if
    possible (see [Input Focus](#Input-Focus)).

    If buffer-or-name is `nil`, it defaults to the buffer
    returned by `other-buffer` (see [The Buffer List](#Buffer-List)). If
    buffer-or-name is a string that is not the name of any existing
    buffer, this function creates a new buffer with that name; the new
    buffer’s major mode is determined by the variable `major-mode`
    (see [Major Modes](#Major-Modes)). In any case, that buffer is made current and
    returned, even when no suitable window was found to display it.

    If action is non-`nil`, it should be a display action to
    pass to `display-buffer` (see [Choosing a Window for Displaying a Buffer](#Choosing-Window)).
    Alternatively, a non-`nil`, non-list value means to pop to a
    window other than the selected one—even if the buffer is already
    displayed in the selected window.

    Like `switch-to-buffer`, this function updates the buffer list
    unless norecord is non-`nil`.

---

Next: [Window History](#Window-History), Previous: [Switching to a Buffer in a Window](#Switching-Buffers), Up: [Windows](#Windows)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
