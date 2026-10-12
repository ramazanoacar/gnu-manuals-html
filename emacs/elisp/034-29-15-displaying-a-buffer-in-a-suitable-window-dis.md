### 29.15 Displaying a Buffer in a Suitable Window [¶](#Displaying-a-Buffer-in-a-Suitable-Window)

This section describes lower-level functions Emacs uses to find or
create a window for displaying a specified buffer. The common
workhorse of these functions is `display-buffer` which eventually
handles all incoming requests for buffer display (see [Choosing a Window for Displaying a Buffer](#Choosing-Window)).

`display-buffer` delegates the task of finding a suitable
window to so-called action functions (see [Action Functions for Buffer Display](#Buffer-Display-Action-Functions)). First, `display-buffer` compiles a so-called action
alist—a special association list that action functions can use to
fine-tune their behavior. Then it passes that alist on to each action
function it calls (see [Action Alists for Buffer Display](#Buffer-Display-Action-Alists)).

The behavior of `display-buffer` is highly customizable. To
understand how customizations are used in practice, you may wish to
study examples illustrating the order of precedence which
`display-buffer` uses to call action functions (see [Precedence of Action Functions](#Precedence-of-Action-Functions)). To avoid conflicts between Lisp programs
calling `display-buffer` and user customizations of its behavior,
it may make sense to follow a number of guidelines which are sketched
in the final part of this section (see [The Zen of Buffer Display](#The-Zen-of-Buffer-Display)).

* [Choosing a Window for Displaying a Buffer](#Choosing-Window)
* [Action Functions for Buffer Display](#Buffer-Display-Action-Functions)
* [Action Alists for Buffer Display](#Buffer-Display-Action-Alists)
* [Additional Options for Displaying Buffers](#Choosing-Window-Options)
* [Precedence of Action Functions](#Precedence-of-Action-Functions)
* [The Zen of Buffer Display](#The-Zen-of-Buffer-Display)

---

Next: [Action Functions for Buffer Display](#Buffer-Display-Action-Functions), Up: [Displaying a Buffer in a Suitable Window](#Displaying-Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 29.15.1 Choosing a Window for Displaying a Buffer [¶](#Choosing-a-Window-for-Displaying-a-Buffer)

The command `display-buffer` flexibly chooses a window for
display, and displays a specified buffer in that window. It can be
called interactively, via the key binding `C-x 4 C-o`. It is also
used as a subroutine by many functions and commands, including
`switch-to-buffer` and `pop-to-buffer` (see [Switching to a Buffer in a Window](#Switching-Buffers)).

This command performs several complex steps to find a window to
display in. These steps are described by means of *display
actions*, which have the form `(functions . alist)`.
Here, functions is either a single function or a list of
functions, referred to as “action functions” (see [Action Functions for Buffer Display](#Buffer-Display-Action-Functions)); and alist is an association list, referred
to as “action alist” (see [Action Alists for Buffer Display](#Buffer-Display-Action-Alists)).
See [The Zen of Buffer Display](#The-Zen-of-Buffer-Display), for samples of display actions.

An action function accepts two arguments: the buffer to display and
an action alist. It attempts to display the buffer in some window,
picking or creating a window according to its own criteria. If
successful, it returns the window; otherwise, it returns `nil`.

`display-buffer` works by combining display actions from
several sources, and calling the action functions in turn, until one
of them manages to display the buffer and returns a non-`nil`
value.

Command: **display-buffer** buffer-or-name &optional action frame [¶](#index-display_002dbuffer)
:   This command makes buffer-or-name appear in some window, without
    selecting the window or making the buffer current. The argument
    buffer-or-name must be a buffer or the name of an existing
    buffer. The return value is the window chosen to display the buffer,
    or `nil` if no suitable window was found.

    The optional argument action, if non-`nil`, should normally
    be a display action (described above). `display-buffer` builds a
    list of action functions and an action alist, by consolidating display
    actions from the following sources (in order of their precedence,
    from highest to lowest):

    * The variable `display-buffer-overriding-action`.
    * The user option `display-buffer-alist`.
    * The action argument.
    * The user option `display-buffer-base-action`.
    * The constant `display-buffer-fallback-action`.

    In practice this means that `display-buffer` builds a list of all
    action functions specified by these display actions. The first
    element of this list is the first action function specified by
    `display-buffer-overriding-action`, if any. Its last element is
    `display-buffer-pop-up-frame`—the last action function
    specified by `display-buffer-fallback-action`. Duplicates are
    not removed from this list—hence one and the same action function
    may be called multiple times during one call of `display-buffer`.

    `display-buffer` calls the action functions specified by this
    list in turn, passing the buffer as the first argument and the
    combined action alist as the second argument, until one of the
    functions returns non-`nil`. See [Precedence of Action Functions](#Precedence-of-Action-Functions), for examples how display actions specified by different
    sources are processed by `display-buffer`.

    Note that the second argument is always the list of *all* action
    alist entries specified by the sources named above. Hence, the first
    element of that list is the first action alist entry specified by
    `display-buffer-overriding-action`, if any. Its last element is
    the last alist entry of `display-buffer-base-action`, if any (the
    action alist of `display-buffer-fallback-action` is empty).

    Note also, that the combined action alist may contain duplicate
    entries and entries for the same key with different values. As a
    rule, action functions always use the first association of a key they
    find. Hence, the association an action function uses is not
    necessarily the association provided by the display action that
    specified that action function,

    The argument action can also have a non-`nil`, non-list
    value. This has the special meaning that the buffer should be
    displayed in a window other than the selected one, even if the
    selected window is already displaying it. If called interactively
    with a prefix argument, action is `t`. Lisp programs
    should always supply a list value.

    The optional argument frame, if non-`nil`, specifies which
    frames to check when deciding whether the buffer is already displayed.
    It is equivalent to adding an element `(reusable-frames . frame)` to the action alist of action (see [Action Alists for Buffer Display](#Buffer-Display-Action-Alists)). The frame argument is provided for
    compatibility reasons, Lisp programs should not use it.

Variable: **display-buffer-overriding-action** [¶](#index-display_002dbuffer_002doverriding_002daction)
:   The value of this variable should be a display action, which is
    treated with the highest priority by `display-buffer`. The
    default value is an empty display action, i.e., `(nil . nil)`.

User Option: **display-buffer-alist** [¶](#index-display_002dbuffer_002dalist)
:   The value of this option is an alist mapping conditions to display
    actions. Each condition is passed to `buffer-match-p`
    (see [The Buffer List](#Buffer-List)), along with the buffer name and the action
    argument passed to `display-buffer`. If it returns a
    non-`nil` value, then `display-buffer` uses the
    corresponding display action to display the buffer. Caveat: if you
    use `derived-mode` or `major-mode` as condition,
    `buffer-match-p` could fail to report a match if
    `display-buffer` is called before the major mode of the buffer is
    set.

    If the caller of `display-buffer` passes a category as a symbol
    in its action argument, then you can use the same category in
    `display-buffer-alist` to match buffers with different names,
    for example:

    ```
    (add-to-list 'display-buffer-alist
                 '((category . comint)
                   (display-buffer-same-window)
                   (inhibit-same-window . nil)))

    (display-buffer (get-buffer-create "*my-shell*")
                    '(nil (category . comint)))
    ```

    See [`buffer-match-p`](#Buffer-List).

    Regardless of the displayed buffer’s name the caller defines a category
    as a symbol `comint`. Then `display-buffer-alist` matches
    this category for all buffers displayed with the same category.
    This avoids the need to construct a complex regular expression
    that matches a buffer name.

    You can also match on the command now being executed. This is useful
    when different commands display buffers with the same name, but you want
    different command’s buffers to be displayed differently. For example:

    ```
    (add-to-list 'display-buffer-alist
                 '((and "^\\*vc-diff\\*"
                        (not (this-command . vc-root-diff)))
    	       nil
    	       (post-command-select-window . nil)))
    ```

    This means that windows displaying VC diffs, such as those generated by
    `C-x v =` (`vc-diff`, see [Old Revisions](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Old-Revisions) in the Emacs
    Manual) are not left selected, except for windows displaying buffers
    generated by `C-x v D` (`vc-root-diff`), which are left
    selected.

User Option: **display-buffer-base-action** [¶](#index-display_002dbuffer_002dbase_002daction)
:   The value of this option should be a display action. This option can
    be used to define a standard display action for calls to
    `display-buffer`.

Constant: **display-buffer-fallback-action** [¶](#index-display_002dbuffer_002dfallback_002daction)
:   This display action specifies the fallback behavior for
    `display-buffer` if no other display actions are given.

---

Next: [Action Alists for Buffer Display](#Buffer-Display-Action-Alists), Previous: [Choosing a Window for Displaying a Buffer](#Choosing-Window), Up: [Displaying a Buffer in a Suitable Window](#Displaying-Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 29.15.2 Action Functions for Buffer Display [¶](#Action-Functions-for-Buffer-Display)

An *action function* is a function `display-buffer` calls for
choosing a window to display a buffer. Action functions take two
arguments: buffer, the buffer to display, and alist, an
action alist (see [Action Alists for Buffer Display](#Buffer-Display-Action-Alists)). They are
supposed to return a window displaying buffer if they succeed
and `nil` if they fail.

The following basic action functions are defined in Emacs.

Function: **display-buffer-same-window** buffer alist [¶](#index-display_002dbuffer_002dsame_002dwindow)
:   This function tries to display buffer in the selected window.
    It fails if the selected window is a minibuffer window or is dedicated
    to another buffer (see [Dedicated Windows](#Dedicated-Windows)). It also fails if
    alist has a non-`nil` `inhibit-same-window` entry.

Function: **display-buffer-reuse-window** buffer alist [¶](#index-display_002dbuffer_002dreuse_002dwindow)
:   This function tries to display buffer by finding a window that
    is already displaying it. Windows on the selected frame are preferred
    to windows on other frames.

    If alist has a non-`nil` `inhibit-same-window` entry,
    the selected window is not eligible for reuse. The set of frames to
    search for a window already displaying buffer can be specified
    with the help of the `reusable-frames` action alist entry. If
    alist contains no `reusable-frames` entry, this function
    searches just the selected frame.

    If this function chooses a window on another frame, it makes that
    frame visible and, unless alist contains an
    `inhibit-switch-frame` entry, raises that frame if necessary.

    If alist has a non-`nil` `reuse-indirect` entry and no
    window showing buffer has been found, this function tries to find
    a window that is indirectly related to buffer—a window for which
    `window-indirect-buffer-p` (see [Buffers and Windows](#Buffers-and-Windows)) with the
    window and buffer as arguments returns non-`nil`. If such a
    window has been found and the CDR of the `reuse-indirect`
    entry equals the symbol `buffer`, it does not replace the buffer of
    that window with buffer but returns the window with its old buffer
    in place. Otherwise, it puts buffer into that window and returns
    that window.

Function: **display-buffer-reuse-mode-window** buffer alist [¶](#index-display_002dbuffer_002dreuse_002dmode_002dwindow)
:   This function tries to display buffer by finding a window
    that is displaying a buffer in a given mode.

    If alist contains a `mode` entry, its value specifies a
    major mode (a symbol) or a list of major modes. If alist
    contains no `mode` entry, the current major mode of buffer
    is used instead. A window is a candidate if it displays a buffer
    whose mode derives from one of the modes specified thusly.

    The behavior is also controlled by alist entries for
    `inhibit-same-window`, `reusable-frames` and
    `inhibit-switch-frame`, like `display-buffer-reuse-window`
    does.

Function: **display-buffer-pop-up-window** buffer alist [¶](#index-display_002dbuffer_002dpop_002dup_002dwindow)
:   This function tries to display buffer by splitting the largest
    or least recently-used window (usually located on the selected frame).
    It actually performs the split by calling the function specified by
    `split-window-preferred-function` (see [Additional Options for Displaying Buffers](#Choosing-Window-Options)).

    The size of the new window can be adjusted by supplying
    `window-height` and `window-width` entries in alist.
    If alist contains a `preserve-size` entry, Emacs will also
    try to preserve the size of the new window during future resize
    operations (see [Preserving Window Sizes](#Preserving-Window-Sizes)).

    This function fails if no window can be split. More often than not,
    this happens because no window is large enough to allow splitting.
    Setting `split-height-threshold` or `split-width-threshold`
    to lower values may help in this regard. Splitting also fails when
    the selected frame has an `unsplittable` frame parameter;
    see [Buffer Parameters](#Buffer-Parameters).

Function: **display-buffer-in-previous-window** buffer alist [¶](#index-display_002dbuffer_002din_002dprevious_002dwindow)
:   This function tries to display buffer in a window where it was
    displayed previously.

    If alist contains a non-`nil` `inhibit-same-window`
    entry, the selected window is not eligible for use. A dedicated
    window is usable only if it already shows buffer. If
    alist contains a `previous-window` entry, the window
    specified by that entry is usable even if it never showed buffer
    before.

    If alist contains a `reusable-frames` entry (see [Action Alists for Buffer Display](#Buffer-Display-Action-Alists)), its value determines which frames to search
    for a suitable window. If alist contains no
    `reusable-frames` entry, this function searches just the selected
    frame if `display-buffer-reuse-frames` and `pop-up-frames`
    are both `nil`; it searches all frames on the current terminal if
    either of those variables is non-`nil`.

    If more than one window qualifies as usable according to these rules,
    this function makes a choice in the following order of preference:

    * The window specified by any `previous-window` alist entry,
      provided it is not the selected window.
    * A window that showed buffer before, provided it is not the
      selected window.
    * The selected window if it is either specified by a
      `previous-window` alist entry or showed buffer
      before.

Function: **display-buffer-use-some-window** buffer alist [¶](#index-display_002dbuffer_002duse_002dsome_002dwindow)
:   This function tries to display buffer by choosing an existing
    window and displaying the buffer in that window. It first tries to find
    a window that has not been used recently (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)) on any frame specified by a `lru-frames` alist
    entry, falling back to the selected frame if no such entry exists. It
    also prefers windows that satisfy the constraints specified by
    `window-min-width` and `window-min-height` alist
    entries; preferring full-width windows if no `window-min-width`
    entry is found. Finally, it will not return a window whose use time is
    higher than that specified by any `lru-time` entry provided by
    alist.

    If no less recently used window is found, this function will try to use
    some other window, preferably a large window on some visible frame. It
    can fail if all windows are dedicated to other buffers (see [Dedicated Windows](#Dedicated-Windows)).

    The above describes the behavior when the `some-window` alist
    entry is `lru` or `nil` which is the default. Another
    possible value is `mru`. If, for example,
    `display-buffer-base-action` is customized to `(nil . ((some-window . mru)))`, then this function will prefer the most
    recently used window. This will try to display several buffers from
    consecutive calls of `display-buffer` in the same window. Consider
    a configuration of three or more windows where a user wants to consult,
    in a non-selected window, one after the other, the results of a query
    spread among several buffers. With the `lru` strategy, Emacs may
    continuously choose another window because the least recently used
    window changes with every call of `display-buffer-use-some-window`.
    With the `mru` strategy, the window chosen would always remain the
    same, resulting in a predictable user experience.

Function: **display-buffer-use-least-recent-window** buffer alist [¶](#index-display_002dbuffer_002duse_002dleast_002drecent_002dwindow)
:   This function is similar to `display-buffer-use-some-window`, but
    will try harder to not use a recently used window. In particular,
    it does not use the selected window. In addition, it will first try to
    reuse a window that shows buffer already, base the decision
    whether it should use a window showing another buffer on that window’s
    use time alone and pop up a new window if no usable window is found.

    Finally, this function will bump the use time (see [Selecting Windows](#Selecting-Windows)) of any window it returns in order to avoid that further
    invocations will use that window for showing another buffer. An
    application that wants to display several buffers in a row can help this
    function by providing a `lru-time` alist entry it has
    initially set to the value of the selected window’s use time. Each
    invocation of this function will then bump the use time of the window
    returned to a value higher than that and a subsequent invocation will
    inhibit this function to use a window it returned earlier.

Function: **display-buffer-in-direction** buffer alist [¶](#index-display_002dbuffer_002din_002ddirection)
:   This function tries to display buffer at a location specified by
    alist. For this purpose, alist should contain a
    `direction` entry whose value is one of `left`, `above`
    (or `up`), `right` and `below` (or `down`). Other
    values are usually interpreted as `below`.

    If alist also contains a `window` entry, its value
    specifies a reference window. That value can be a special symbol like
    `main` which stands for the selected frame’s main window
    (see [Side Window Options and Functions](#Side-Window-Options-and-Functions)) or `root` standing
    for the selected frame’s root window (see [Windows and Frames](#Windows-and-Frames)). It
    can also specify an arbitrary valid window. Any other value (or
    omitting the `window` entry entirely) means to use the selected
    window as reference window.

    This function first tries to reuse a window in the specified direction
    that already shows buffer. If no such window exists, it tries
    to split the reference window in order to produce a new window in the
    specified direction. If this fails as well, it will try to display
    buffer in an existing window in the specified direction. In
    either case, the window chosen will appear on the side of the
    reference window specified by the `direction` entry, sharing at
    least one edge with the reference window.

    If the reference window is live, the edge the chosen window will share
    with it is always the opposite of the one specified by the
    `direction` entry. For example, if the value of the
    `direction` entry is `left`, the chosen window’s right edge
    coordinate (see [Coordinates and Windows](#Coordinates-and-Windows)) will equal the reference
    window’s left edge coordinate.

    If the reference window is internal, a reused window must share with
    it the edge specified by the `direction` entry. Hence if, for
    example, the reference window is the frame’s root window and the value
    of the `direction` entry is `left`, a reused window must be
    on the left of the frame. This means that the left edge coordinate of
    the chosen window and that of the reference window are the same.

    A new window, however, will be created by splitting the reference
    window such that the chosen window will share the opposite edge with
    the reference window. In our example, a new root window would be
    created with a new live window and the reference window as its
    children. The chosen window’s right edge coordinate would then equal
    the left edge coordinate of the reference window. Its left edge
    coordinate would equal the left edge coordinate of the frame’s new
    root window.

    Four special values for `direction` entries allow implicitly
    specifying the selected frame’s main window as the reference window:
    `leftmost`, `top`, `rightmost` and `bottom`. This
    means that instead of, for example, `(direction . left) (window . main)` one can just specify `(direction . leftmost)`. An existing `window` alist entry is ignored
    in such cases.

Function: **display-buffer-below-selected** buffer alist [¶](#index-display_002dbuffer_002dbelow_002dselected)
:   This function tries to display buffer in a window below the
    selected window. If there is a window below the selected one and that
    window already displays buffer, it reuses that window.

    If there is no such window, this function tries to create a new window
    by splitting the selected one, and displays buffer there. It will
    also try to adjust that window’s size provided alist contains a
    suitable `window-height` or `window-width` entry, see above.

    If splitting the selected window fails and there is a non-dedicated
    window below the selected one showing some other buffer, this function
    tries to use that window for showing buffer.

    If alist contains a `window-min-height` entry, this
    function ensures that the window used is or can become at least as
    high as specified by that entry’s value. Note that this is only a
    guarantee. In order to actually resize the window used, alist
    must also provide an appropriate `window-height` entry.

Function: **display-buffer-at-bottom** buffer alist [¶](#index-display_002dbuffer_002dat_002dbottom)
:   This function tries to display buffer in a window at the bottom
    of the selected frame.

    This either tries to split the window at the bottom of the frame or the
    frame’s main window, or to reuse an existing window at the bottom of the
    selected frame.

Function: **display-buffer-pop-up-frame** buffer alist [¶](#index-display_002dbuffer_002dpop_002dup_002dframe)
:   This function creates a new frame, and displays the buffer in that
    frame’s window. It actually performs the frame creation by calling
    the function specified in `pop-up-frame-function`
    (see [Additional Options for Displaying Buffers](#Choosing-Window-Options)). If alist contains a
    `pop-up-frame-parameters` entry, the associated value is added to
    the newly created frame’s parameters.

Function: **display-buffer-full-frame** buffer alist [¶](#index-display_002dbuffer_002dfull_002dframe)
:   This function displays the buffer on the current frame, deleting all
    other windows so that it takes up the full frame.

Function: **display-buffer-in-child-frame** buffer alist [¶](#index-display_002dbuffer_002din_002dchild_002dframe)
:   This function tries to display buffer in a child frame
    (see [Child Frames](#Child-Frames)) of the selected frame, either reusing an
    existing child frame or by making a new one. If alist has a
    non-`nil` `child-frame-parameters` entry, the corresponding
    value is an alist of frame parameters to give the new frame. A
    `parent-frame` parameter specifying the selected frame is
    provided by default. If the child frame should become the child of
    another frame, a corresponding entry must be added to alist.

    The appearance of child frames is largely dependent on the parameters
    provided via alist. It is advisable to use at least ratios to
    specify the size (see [Size Parameters](#Size-Parameters)) and the position
    (see [Position Parameters](#Position-Parameters)) of the child frame, and to add a
    `keep-ratio` parameter (see [Frame Interaction Parameters](#Frame-Interaction-Parameters)), in
    order to make sure that the child frame remains visible. For other
    parameters that should be considered see [Child Frames](#Child-Frames).

Function: **display-buffer-use-some-frame** buffer alist [¶](#index-display_002dbuffer_002duse_002dsome_002dframe)
:   This function tries to display buffer by finding a frame that
    meets a predicate (by default any frame other than the selected
    frame).

    If this function chooses a window on another frame, it makes that
    frame visible and, unless alist contains an
    `inhibit-switch-frame` entry, raises that frame if necessary.

    If alist has a non-`nil` `frame-predicate` entry, its
    value is a function taking one argument (a frame), returning
    non-`nil` if the frame is a candidate; this function replaces the
    default predicate.

    If alist has a non-`nil` `inhibit-same-window` entry,
    the selected window is not used; thus if the selected frame has a
    single window, it is not used.

Function: **display-buffer-in-new-tab** buffer alist [¶](#index-display_002dbuffer_002din_002dnew_002dtab)
:   This function tries to display buffer in a new tab.

    If alist contains a non-`nil` `tab-name` entry (which
    may be a string or a function), the buffer is displayed in a new tab
    with that name. If the `tab-name` entry is a function, it is
    called with two arguments (buffer and alist), and should
    return the tab name.

    If the `tab-name` entry is omitted or `nil`, a new tab is
    created without an explicit name.

    If alist contains a non-`nil` `tab-group` entry, this
    defines the tab group, overriding user option
    `tab-bar-new-tab-group`. This entry may again be a string or a
    function which is called in the same manner as `tab-name`.

Function: **display-buffer-in-tab** buffer alist [¶](#index-display_002dbuffer_002din_002dtab)
:   This function tries to display buffer in a new or existing tab.

    If alist contains a non-`nil` `reusable-frames` entry
    then the frames indicated by its value are searched for an existing tab
    which already displays the buffer. The possible values of
    `reusable-frames` are:

    * `t`
      means consider all existing frames.
    * `visible`
      means consider all visible frames.
    * 0
      means consider all frames on the current terminal.
    * A frame
      means consider that frame only.
    * Any other non-`nil` value
      means consider the selected frame.
    * `nil`
      means do not search any frames (equivalent to omitting the entry). Note
      that this is different to the typical meaning of the value `nil`
      for a `reusable-frames` entry in a buffer display action alist.

    If alist contains a non-`nil` `ignore-current-tab`
    entry, then the current tab is skipped when searching for a reusable
    tab. Otherwise the current tab is used by preference if it already
    displays the buffer.

    If a window displaying the buffer is located in any reusable tab then
    that tab and window are selected.

    If no such window is located, the buffer is displayed in a new or
    existing tab based on the alist entry `tab-name` (which may
    be a string or a function). If a tab with this name already exists then
    that tab is selected, otherwise a new tab with that name is created. If
    the `tab-name` entry is a function, it is called with two arguments
    (buffer and alist), and should return the tab name. If the
    `tab-name` entry is omitted or `nil`, a new tab is created
    without an explicit name.

    If a new tab is created and alist contains a non-`nil`
    `tab-group` entry, this defines the tab group, overriding user
    option `tab-bar-new-tab-group`. This entry may again be a string
    or a function which is called in the same manner as `tab-name`.

Function: **display-buffer-no-window** buffer alist [¶](#index-display_002dbuffer_002dno_002dwindow)
:   If alist has a non-`nil` `allow-no-window` entry, then
    this function does not display buffer and returns the symbol
    `fail`. This constitutes the only exception to the convention
    that an action function returns either `nil` or a window showing
    buffer. If alist has no such `allow-no-window`
    entry, this function returns `nil`.

    If this function returns `fail`, `display-buffer` will skip
    the execution of any further display actions and return `nil`
    immediately. If this function returns `nil`,
    `display-buffer` will continue with the next display action, if
    any.

    It is assumed that when a caller of `display-buffer` specifies a
    non-`nil` `allow-no-window` entry, it is also able to handle
    a `nil` return value.

Two other action functions are described in their proper
sections—`display-buffer-in-side-window` (see [Displaying Buffers in Side Windows](#Displaying-Buffers-in-Side-Windows)) and `display-buffer-in-atom-window`
(see [Atomic Windows](#Atomic-Windows)).

---

Next: [Additional Options for Displaying Buffers](#Choosing-Window-Options), Previous: [Action Functions for Buffer Display](#Buffer-Display-Action-Functions), Up: [Displaying a Buffer in a Suitable Window](#Displaying-Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 29.15.3 Action Alists for Buffer Display [¶](#Action-Alists-for-Buffer-Display)

An *action alist* is an association list mapping predefined
symbols recognized by action functions to values these functions are
supposed to interpret accordingly. In each call,
`display-buffer` constructs a new, possibly empty action alist
and passes that entire list on to any action function it calls.

By design, action functions are free in their interpretation of
action alist entries. In fact, some entries like
`allow-no-window` or `previous-window` have a meaning only
for one or a few action functions, and are ignored by the rest. Other
entries, like `inhibit-same-window` or `window-parameters`,
are supposed to be respected by most action functions, including those
provided by application programs and external packages.

In the previous subsection we have described in detail how
individual action functions interpret the action alist entries they
care about. Here we give a reference list of all known action alist
entries according to their symbols, together with their values and
action functions (see [Action Functions for Buffer Display](#Buffer-Display-Action-Functions)) that
recognize them. Throughout this list, the terms “buffer” will refer
to the buffer `display-buffer` is supposed to display, and
“value” refers to the entry’s value.

`inhibit-same-window` [¶](#index-inhibit_002dsame_002dwindow_002c-a-buffer-display-action-alist-entry)
:   If the value is non-`nil`, this signals that the selected window
    must not be used for displaying the buffer. All action functions that
    (re-)use an existing window should respect this entry.

`previous-window` [¶](#index-previous_002dwindow_002c-a-buffer-display-action-alist-entry)
:   The value must specify a window that may have displayed the buffer
    previously. `display-buffer-in-previous-window` will give
    preference to such a window provided it is still live and not
    dedicated to another buffer.

`mode` [¶](#index-mode_002c-a-buffer-display-action-alist-entry)
:   The value is either a major mode or a list of major modes.
    `display-buffer-reuse-mode-window` may reuse a window whenever
    the value specified by this entry matches the major mode of that
    window’s buffer. Other action functions ignore such entries.

`frame-predicate` [¶](#index-frame_002dpredicate_002c-a-buffer-display-action-alist-entry)
:   The value must be a function taking one argument (a frame), supposed
    to return non-`nil` if that frame is a candidate for displaying
    the buffer. This entry is used by
    `display-buffer-use-some-frame`.

`reusable-frames` [¶](#index-reusable_002dframes_002c-a-buffer-display-action-alist-entry)
:   The value specifies the set of frames to search for a window that can
    be reused because it already displays the buffer. It can be set as
    follows:

    * `nil` means consider only windows on the selected frame.
      (Actually, the last frame used that is not a minibuffer-only frame.)
    * `visible` means consider windows on all visible frames.
    * 0 means consider windows on all visible or iconified frames.
    * A frame means consider windows on that frame only.
    * `t` means consider windows on all frames. (Note that this value
      is rarely the right thing to use—it might also return a tooltip frame.)

    Note that the meaning of `nil` differs slightly from that of the
    all-frames argument to `next-window` (see [Cyclic Ordering of Windows](#Cyclic-Window-Ordering)).

    A major client of this is `display-buffer-reuse-window`, but all
    other action functions that try to reuse a window are affected as
    well. `display-buffer-in-previous-window` consults it when
    searching for a window that previously displayed the buffer on another
    frame.

    The action function `display-buffer-in-tab` searches the tabs of
    the frame(s) identified by this entry, and also interprets the value
    `nil` differently.

`inhibit-switch-frame` [¶](#index-inhibit_002dswitch_002dframe_002c-a-buffer-display-action-alist-entry)
:   A non-`nil` value prevents another frame from being raised or
    selected, if the window chosen by `display-buffer` is displayed
    there. Primarily affected by this are
    `display-buffer-use-some-frame` and
    `display-buffer-reuse-window`. Ideally,
    `display-buffer-pop-up-frame` should be affected as well, but there
    is no guarantee that the window manager will comply.

`window-parameters` [¶](#index-window_002dparameters_002c-a-buffer-display-action-alist-entry)
:   The value specifies an alist of window parameters to give the chosen
    window. All action functions that choose a window should process this
    entry.

`window-min-width` [¶](#index-window_002dmin_002dwidth_002c-a-buffer-display-action-alist-entry)
:   The value specifies a minimum width of the window used, in canonical
    frame columns. The special value `full-width` means the chosen
    window should be one that has no other windows on the left or right of
    it in its frame.

    This entry is currently honored by `display-buffer-use-some-window`
    and `display-buffer-use-least-recent-window`, which try hard to avoid
    returning a less recently used window that does not satisfy the entry.

    Note that providing such an entry alone does not necessarily make the
    window as wide as specified by its value. To actually resize an
    existing window or make a new window as wide as specified by this
    entry’s value, a `window-width` entry specifying that value
    should be provided as well. Such a `window-width` entry can,
    however, specify a completely different value, or ask the window width
    to fit that of its buffer, in which case the
    `window-min-width` entry provides the guaranteed minimum width of
    the window.

`window-min-height` [¶](#index-window_002dmin_002dheight_002c-a-buffer-display-action-alist-entry)
:   The value specifies a minimum height of the window used, in canonical
    frame lines. The special value `full-height` means the chosen
    window should be a full-height window, one that has no other windows
    above or below it in its frame.

    This entry is currently honored by `display-buffer-below-selected`
    which does not use a window that is not as high as specified by this
    entry. It’s also honored by `display-buffer-use-some-window` and
    `display-buffer-use-least-recent-window` which try hard to avoid
    returning a less recently used window if it does not satisfy this
    constraint.

    Note that providing such an entry alone does not necessarily make the
    window as tall as specified by its value. To actually resize an
    existing window or make a new window as tall as specified by that
    value, a `window-height` entry specifying that value should be
    provided as well. Such a `window-height` entry can, however,
    specify a completely different value or ask the window height to be
    fit to that of its buffer in which case the `window-min-height`
    entry provides the guaranteed minimum height of the window used.

`window-height` [¶](#index-window_002dheight_002c-a-buffer-display-action-alist-entry)
:   The value specifies whether and how to adjust the height of the chosen
    window and can be one of the following:

    * `nil` means to leave the height of the chosen window alone.
    * An integer number specifies the desired total height of the chosen
      window in lines.
    * A floating-point number specifies the fraction of the chosen window’s
      desired total height with respect to the total height of its frame’s
      root window.
    * A cons cell whose CAR is `body-lines` and whose CDR is an
      integer that specifies the height of the chosen window’s body in frame
      lines.
    * If the value specifies a function, that function is called with one
      argument—the chosen window. The function is supposed to adjust the
      height of the window; its return value is ignored. Suitable functions
      are `fit-window-to-buffer` and
      `shrink-window-if-larger-than-buffer`, see [Resizing Windows](#Resizing-Windows).

    By convention, the height of the chosen window is adjusted only if the
    window is part of a vertical combination (see [Windows and Frames](#Windows-and-Frames))
    to avoid changing the height of other, unrelated windows. Also, this
    entry should be processed only under certain conditions which are
    specified right below this list.

`window-width` [¶](#index-window_002dwidth_002c-a-buffer-display-action-alist-entry)
:   This entry is similar to the `window-height` entry described
    before, but used to adjust the chosen window’s width instead. The
    value can be one of the following:

    * `nil` means to leave the width of the chosen window alone.
    * An integer specifies the desired total width of the chosen window in
      columns.
    * A floating-point number specifies the fraction of the chosen window’s
      desired total width with respect to the total width of the frame’s
      root window.
    * A cons cell whose CAR is `body-columns` and whose CDR is
      an integer that specifies the width of the chosen window’s body in frame
      columns.
    * If the value specifies a function, that function is called with one
      argument—the chosen window. The function is supposed to adjust the
      width of the window; its return value is ignored.

`window-size` [¶](#index-window_002dsize_002c-a-buffer-display-action-alist-entry)
:   This entry is a combination of the two preceding ones and can be used to
    adjust the chosen window’s height *and* width. Since windows can
    be resized in one direction only without affecting other windows,
    `window-size` is effective only to set up the size of a window
    appearing alone on a frame. The value can be one of the following:

    * `nil` means to leave the size of the chosen window alone.
    * A cons cell of two integers specifies the desired total width and height
      of the chosen window in lines and columns. It’s effect is to adjust the
      size of the frame accordingly.
    * A cons cell whose CAR equals `body-chars` and whose CDR
      is a cons cell of two integers—the desired body width and height of
      the chosen window in frame columns and lines. It’s effect is to adjust
      the size of the frame accordingly.
    * If the value specifies a function, that function is called with one
      argument—the chosen window. The function is supposed to adjust the
      size of the window’s frame; its return value is ignored.

    This entry should be processed under only certain conditions which are
    specified right below this list.

`dedicated` [¶](#index-dedicated_002c-a-buffer-display-action-alist-entry)
:   If non-`nil`, such an entry tells `display-buffer` to mark any
    window it creates as dedicated to its buffer (see [Dedicated Windows](#Dedicated-Windows)). It does that by calling `set-window-dedicated-p` with
    the chosen window as first argument and the entry’s value as second.
    Side windows are by default dedicated with the value `side`
    (see [Side Window Options and Functions](#Side-Window-Options-and-Functions)). Windows that are reused
    are not marked as dedicated regardless of whether they already showed
    the buffer before or not. If a window dedicated to its buffer gets
    reused for showing the same buffer again, its dedicated flag is left
    alone.

`preserve-size` [¶](#index-preserve_002dsize_002c-a-buffer-display-action-alist-entry)
:   If non-`nil` such an entry tells Emacs to preserve the size of
    the window chosen (see [Preserving Window Sizes](#Preserving-Window-Sizes)). The value should
    be either `(t . nil)` to preserve the width of the window,
    `(nil . t)` to preserve its height or `(t . t)` to
    preserve both, its width and its height. This entry should be
    processed only under certain conditions which are specified right
    after this list.

`lru-frames` [¶](#index-lru_002dframes_002c-a-buffer-display-action-alist-entry)
:   The value specifies the set of frames to search for a window that can be
    used to display the buffer. It is honored by
    `display-buffer-use-some-window` and
    `display-buffer-use-least-recent-window` when trying to find a less
    recently used window showing some other buffer. Its values are the same
    as for the `reusable-frames` entry described above.

`lru-time` [¶](#index-lru_002dtime_002c-a-buffer-display-action-alist-entry)
:   The value is supposed to specify a use time (see [Selecting Windows](#Selecting-Windows)).
    This entry is honored by `display-buffer-use-some-window` and
    `display-buffer-use-least-recent-window` when trying to find a less
    recently used window showing some other buffer. If a window’s use time
    is higher than the value specified by this option, these action
    functions will not consider such a window for displaying the buffer.

`bump-use-time` [¶](#index-bump_002duse_002dtime_002c-a-buffer-display-action-alist-entry)
:   If non-`nil`, such an entry will cause `display-buffer` to
    bump the use time (see [Selecting Windows](#Selecting-Windows)) of the window it uses.
    This should avoid later use of this window by action functions
    like `display-buffer-use-some-window` and
    `display-buffer-use-least-recent-window` for showing another
    buffer.

    There is a fine difference between using this entry and using the action
    function `display-buffer-use-least-recent-window`. Calling the
    latter means to only bump the use times of windows that function uses
    for displaying the buffer. The entry described here will cause
    `display-buffer` to bump the use time of *any* window used for
    displaying a buffer.

`pop-up-frame-parameters` [¶](#index-pop_002dup_002dframe_002dparameters_002c-a-buffer-display-action-alist-entry)
:   The value specifies an alist of frame parameters to give a new frame,
    if one is created. `display-buffer-pop-up-frame` is its one and
    only addressee.

`pop-up-frames` [¶](#index-pop_002dup_002dframes_002c-a-buffer-display-action-alist-entry)
:   The value controls whether `display-buffer` may display buffers by
    making new frames. It has the same meaning as the `pop-up-frames`
    variable and takes precedence over it when present. Its main intended
    purpose is to override a non-`nil` value of the variable for
    particular buffers which the user prefers to keep in the selected frame.

`parent-frame` [¶](#index-parent_002dframe_002c-a-buffer-display-action-alist-entry)
:   The value specifies the parent frame to be used when the buffer is
    displayed on a child frame. This entry is used only by
    `display-buffer-in-child-frame`.

`child-frame-parameters` [¶](#index-child_002dframe_002dparameters_002c-a-buffer-display-action-alist-entry)
:   The value specifies an alist of frame parameters to use when the buffer
    is displayed on a child frame. This entry is used only by
    `display-buffer-in-child-frame`.

`side` [¶](#index-side_002c-a-buffer-display-action-alist-entry)
:   The value denotes the side of the frame or window where a new window
    displaying the buffer shall be created. This entry is used by
    `display-buffer-in-side-window` to indicate the side of the frame
    where a new side window shall be placed (see [Displaying Buffers in Side Windows](#Displaying-Buffers-in-Side-Windows)). It is also used by
    `display-buffer-in-atom-window` to indicate the side of an
    existing window where the new window shall be located (see [Atomic Windows](#Atomic-Windows)).

`slot` [¶](#index-slot_002c-a-buffer-display-action-alist-entry)
:   If non-`nil`, the value specifies the slot of the side window
    supposed to display the buffer. This entry is used only by
    `display-buffer-in-side-window`.

`direction` [¶](#index-direction_002c-a-buffer-display-action-alist-entry)
:   The value specifies a direction which, together with a `window`
    entry, allows `display-buffer-in-direction` to determine the
    location of the window to display the buffer.

`window` [¶](#index-window_002c-a-buffer-display-action-alist-entry)
:   The value specifies a window that is in some way related to the window
    chosen by `display-buffer`. This entry is currently used by
    `display-buffer-in-atom-window` to indicate the window on whose
    side the new window shall be created. It is also used by
    `display-buffer-in-direction` to specify the reference window on
    whose side the resulting window shall appear.

`allow-no-window` [¶](#index-allow_002dno_002dwindow_002c-a-buffer-display-action-alist-entry)
:   If the value is non-`nil`, `display-buffer` does not
    necessarily have to display the buffer and the caller is prepared to
    accept that. This entry is not intended for user customizations,
    since there is no guarantee that an arbitrary caller of
    `display-buffer` will be able to handle the case that no window
    will display the buffer. `display-buffer-no-window` is the only
    action function that cares about this entry.

`some-window` [¶](#index-some_002dwindow_002c-a-buffer-display-action-alist-entry)
:   If the value is `nil` or `lru`, `display-buffer-use-some-window`
    prefers the least recently used window while avoiding selecting windows
    that are not full-width and windows on another frame. If the value is
    `mru`, it prefers the most recently used window not considering the
    selected window and windows on any frame but the selected one. If the
    value is a function, it is called with two arguments: a buffer and an
    alist, and should return the window where to display the buffer.

`body-function` [¶](#index-body_002dfunction_002c-a-buffer-display-action-alist-entry)
:   The value must be a function taking one argument (a displayed window).
    This function can be used to fill the displayed window’s body with
    some contents that might depend on dimensions of the displayed window.
    It is called *after* the buffer is displayed, and *before*
    the entries `window-height`, `window-width` and
    `preserve-size` are applied that could resize the window to fit
    it to the inserted contents.

`post-command-select-window` [¶](#index-post_002dcommand_002dselect_002dwindow_002c-a-buffer-display-action-alist-entry)
:   If the value is non-`nil`, the buffer displayed by `display-buffer`
    is selected after the current command is executed by running the hook
    `post-command-hook` (see [Command Loop Overview](#Command-Overview)).
    If the value is `nil`, the buffer selected by such functions as
    `pop-to-buffer` is deselected, and the window that was selected
    before calling this function will remain selected regardless of which
    windows were selected afterwards within this command.

`category` [¶](#index-category_002c-a-buffer-display-action-alist-entry)
:   If the caller of `display-buffer` passes an alist entry
    `(category . symbol)` in its action argument, then you
    can match the displayed buffer by using the same category symbol in the
    condition part of `display-buffer-alist` entries. See [`buffer-match-p`](#Buffer-List). Thus, if a Lisp program uses a particular
    symbol as the category when calling `display-buffer`, users
    can customize how these buffers will be displayed by including such an
    entry in `display-buffer-alist`.

`tab-name` [¶](#index-tab_002dname_002c-a-buffer-display-action-alist-entry)
:   The value names the tab in which the buffer should be displayed. This
    entry is used by `display-buffer-in-new-tab` and (conditionally) by
    `display-buffer-in-tab`.

`tab-group` [¶](#index-tab_002dgroup_002c-a-buffer-display-action-alist-entry)
:   The value names the tab group to use when creating a new tab, overriding
    user option `tab-bar-new-tab-group`. This entry is used by
    `display-buffer-in-new-tab` and `display-buffer-in-tab`.

By convention, the entries `window-height`, `window-width`
and `preserve-size` are applied after the chosen window’s buffer
has been set up and if and only if that window never showed another
buffer before. More precisely, the latter means that the window must
have been either created by the current `display-buffer` call or
the window was created earlier by `display-buffer` to show the
buffer and never was used to show another buffer until it was reused
by the current invocation of `display-buffer`.

If no `window-height`, `window-width` or `window-size`
entry was specified, the window may still be resized automatically when
the buffer is temporary and `temp-buffer-resize-mode` has been
enabled, [Temporary Displays](#Temporary-Displays). In that case, the CDR of a
`window-height`, `window-width` or `window-size` entry
can be used to inhibit or override the default behavior of
`temp-buffer-resize-mode` for specific buffers or invocations of
`display-buffer`.

---

Next: [Precedence of Action Functions](#Precedence-of-Action-Functions), Previous: [Action Alists for Buffer Display](#Buffer-Display-Action-Alists), Up: [Displaying a Buffer in a Suitable Window](#Displaying-Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 29.15.4 Additional Options for Displaying Buffers [¶](#Additional-Options-for-Displaying-Buffers)

The behavior of buffer display actions (see [Choosing a Window for Displaying a Buffer](#Choosing-Window)) can
be further modified by the following user options.

User Option: **pop-up-windows** [¶](#index-pop_002dup_002dwindows)
:   If the value of this variable is non-`nil`, `display-buffer`
    is allowed to split an existing window to make a new window for
    displaying in. This is the default.

    This variable is provided for backward compatibility only. It is
    obeyed by `display-buffer` via a special mechanism in
    `display-buffer-fallback-action`, which calls the action function
    `display-buffer-pop-up-window` (see [Action Functions for Buffer Display](#Buffer-Display-Action-Functions)) when the value of this option is non-`nil`. It is
    not consulted by `display-buffer-pop-up-window` itself, which the
    user may specify directly in `display-buffer-alist` etc.

User Option: **split-window-preferred-function** [¶](#index-split_002dwindow_002dpreferred_002dfunction)
:   This variable specifies a function for splitting a window, in order to
    make a new window for displaying a buffer. It is used by the
    `display-buffer-pop-up-window` action function to actually split
    the window.

    The value must be a function that takes one argument, a window, and
    returns either a new window (which will be used to display the desired
    buffer) or `nil` (which means the splitting failed). The default
    value is `split-window-sensibly`, which is documented next.

Function: **split-window-sensibly** &optional window [¶](#index-split_002dwindow_002dsensibly)
:   This function tries to split window and return the newly created
    window. If window cannot be split, it returns `nil`. If
    window is omitted or `nil`, it defaults to the selected
    window.

    This function obeys the usual rules that determine when a window may be
    split (see [Splitting Windows](#Splitting-Windows)). It first tries either a vertical
    split by placing the new window below, subject to the restriction
    imposed by `split-height-threshold` (see below), or a horizontal
    split that places the new window to the right, subject to
    `split-width-threshold`, in addition to any other restrictions.
    Whether it tries first to split vertically or horizontally depends on
    the value of the user option `split-window-preferred-direction`.
    If splitting along the first dimension fails, it tries to split along
    the other dimension. If that also fails, and the window is the only
    window on its frame, this function again tries to split and place the
    new window below, disregarding `split-height-threshold`. If this
    fails as well, this function gives up and returns `nil`.

User Option: **split-height-threshold** [¶](#index-split_002dheight_002dthreshold)
:   This variable specifies whether `split-window-sensibly` is
    allowed to split the window placing the new window below. If it is an
    integer, that means to split only if the original window has at least
    that many lines. If it is `nil`, that means not to split this
    way.

User Option: **split-width-threshold** [¶](#index-split_002dwidth_002dthreshold)
:   This variable specifies whether `split-window-sensibly` is
    allowed to split the window placing the new window to the right. If
    the value is an integer, that means to split only if the original
    window has at least that many columns. If the value is `nil`,
    that means not to split this way.

User Option: **split-window-preferred-direction** [¶](#index-split_002dwindow_002dpreferred_002ddirection)
:   This variable determines the first dimension along which
    `split-window-sensibly` tries to split the window, if the window
    could be split both vertically and horizontally, as determined by the
    values of `split-height-threshold` and
    `split-width-threshold`. The default value is `longest`,
    which means to split vertically if the height of the window’s frame is
    greater or equal to its width, and horizontally otherwise. The values
    `vertical` and `horizontal` specify the direction in which to
    attempt the first split.

User Option: **even-window-sizes** [¶](#index-even_002dwindow_002dsizes)
:   This variable, if non-`nil`, causes `display-buffer` to even
    window sizes whenever it reuses an existing window, and that window is
    adjacent to the selected one.

    If its value is `width-only`, sizes are evened only if the reused
    window is on the left or right of the selected one and the selected
    window is wider than the reused one. If its value is `height-only`
    sizes are evened only if the reused window is above or beneath the
    selected window and the selected window is higher than the reused one.
    Any other non-`nil` value means to even sizes in any of these cases
    provided the selected window is larger than the reused one in the sense
    of their combination.

User Option: **pop-up-frames** [¶](#index-pop_002dup_002dframes)
:   If the value of this variable is non-`nil`, that means
    `display-buffer` may display buffers by making new frames. The
    default is `nil`.

    A non-`nil` value also means that when `display-buffer` is
    looking for a window already displaying buffer-or-name, it can
    search any visible or iconified frame, not just the selected frame.

    An entry by the same name in `display-buffer`’s alist
    takes precedence over the variable.

    This variable is provided mainly for backward compatibility. It is
    obeyed by `display-buffer` via a special mechanism in
    `display-buffer-fallback-action`, which calls the action function
    `display-buffer-pop-up-frame` (see [Action Functions for Buffer Display](#Buffer-Display-Action-Functions)) if the value is non-`nil`. (This is done before
    attempting to split a window.) This variable is not consulted by
    `display-buffer-pop-up-frame` itself, which the user may specify
    directly in `display-buffer-alist` etc.

User Option: **pop-up-frame-function** [¶](#index-pop_002dup_002dframe_002dfunction)
:   This variable specifies a function for creating a new frame, in order
    to make a new window for displaying a buffer. It is used by the
    `display-buffer-pop-up-frame` action function.

    The value should be a function that takes no arguments and returns a
    frame, or `nil` if no frame could be created. The default value
    is a function that creates a frame using the parameters specified by
    `pop-up-frame-alist` (see below).

User Option: **pop-up-frame-alist** [¶](#index-pop_002dup_002dframe_002dalist)
:   This variable holds an alist of frame parameters (see [Frame Parameters](#Frame-Parameters)), which is used by the function specified by
    `pop-up-frame-function` to make a new frame. The default is
    `nil`.

    This option is provided for backward compatibility only. Note, that
    when `display-buffer-pop-up-frame` calls the function specified
    by `pop-up-frame-function`, it prepends the value of all
    `pop-up-frame-parameters` action alist entries to
    `pop-up-frame-alist` so that the values specified by the action
    alist entry effectively override any corresponding values of
    `pop-up-frame-alist`.

    Hence, users should set up a `pop-up-frame-parameters` action
    alist entry in `display-buffer-alist` instead of customizing
    `pop-up-frame-alist`. Only this will guarantee that the value of
    a parameter specified by the user overrides the value of that
    parameter specified by the caller of `display-buffer`.

Many efforts in the design of `display-buffer` have been given
to maintain compatibility with code that uses older options like
`pop-up-windows`, `pop-up-frames`,
`pop-up-frame-alist`, `same-window-buffer-names` and
`same-window-regexps`. Lisp Programs and users should refrain
from using these options. Above we already warned against customizing
`pop-up-frame-alist`. Here we describe how to convert the
remaining options to use display actions instead.

`pop-up-windows` [¶](#index-pop_002dup_002dwindows_002c-replacement-for)
:   This variable is `t` by default. Instead of customizing it to
    `nil` and thus telling `display-buffer` what not to do, it’s
    much better to list in `display-buffer-base-action` the action
    functions it should try instead as, for example:

    ```
    (setopt
     display-buffer-base-action
     '((display-buffer-reuse-window display-buffer-same-window
        display-buffer-in-previous-window
        display-buffer-use-some-window)))
    ```

`pop-up-frames` [¶](#index-pop_002dup_002dframes_002c-replacement-for)
:   Instead of customizing this variable to `t`, you can customize
    `display-buffer-base-action`, for example, as follows:

    ```
    (setopt display-buffer-base-action '(nil (pop-up-frames . t)))
    ```

`same-window-buffer-names` [¶](#index-same_002dwindow_002dbuffer_002dnames_002c-replacement-for)

`same-window-regexps`
:   Instead of adding a buffer name or a regular expression to one of
    these options use a `display-buffer-alist` entry for that buffer
    specifying the action function `display-buffer-same-window`.

    ```
    (setopt
     display-buffer-alist
     (cons '("\\*foo\\*" (display-buffer-same-window))
            display-buffer-alist))
    ```

---

Next: [The Zen of Buffer Display](#The-Zen-of-Buffer-Display), Previous: [Additional Options for Displaying Buffers](#Choosing-Window-Options), Up: [Displaying a Buffer in a Suitable Window](#Displaying-Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 29.15.5 Precedence of Action Functions [¶](#Precedence-of-Action-Functions-1)

From the past subsections we already know that `display-buffer`
must be supplied with a number of display actions (see [Choosing a Window for Displaying a Buffer](#Choosing-Window)) in order to display a buffer. In a completely uncustomized
Emacs, these actions are specified by
`display-buffer-fallback-action` in the following order of
precedence: Reuse a window, pop up a new window on the same frame, use
a window previously showing the buffer, use some window and pop up a
new frame. (Note that the remaining actions named by
`display-buffer-fallback-action` are void in an uncustomized
Emacs).

Consider the following form:

```
(display-buffer (get-buffer-create "*foo*"))
```

Evaluating this form in the buffer `*scratch*` of an uncustomized
Emacs session will usually fail to reuse a window that shows
`*foo*` already, but succeed in popping up a new window.
Evaluating the same form again will now not cause any visible
changes—`display-buffer` reused the window already showing
`*foo*` because that action was applicable and had the highest
precedence among all applicable actions.

Popping up a new window will fail if there is not enough space on
the selected frame. In an uncustomized Emacs it typically fails when
there are already two windows on a frame. For example, if you now
type `C-x 1` followed by `C-x 2` and evaluate the form
once more, `*foo*` should show up in the lower
window—`display-buffer` just used “some” window. If, before
typing `C-x 2` you had typed `C-x o`, `*foo*`
would have been shown in the upper window because “some” window
stands for the “least recently used” window and the selected window
has been least recently used if and only if it is alone on its frame.

Let’s assume you did not type `C-x o` and `*foo*` is
shown in the lower window. Type `C-x o` to get there followed
by `C-x left` and evaluate the form again. This should
display `*foo*` in the same, lower window because that window had
already shown `*foo*` previously and was therefore chosen instead
of some other window.

So far we have only observed the default behavior in an uncustomized
Emacs session. To see how this behavior can be customized, let’s
consider the option `display-buffer-base-action`. It provides a
very coarse customization which conceptually affects the display of
*any* buffer. It can be used to supplement the actions supplied
by `display-buffer-fallback-action` by reordering them or by
adding actions that are not present there but fit more closely the
user’s editing practice. However, it can also be used to change the
default behavior in a more profound way.

Let’s consider a user who, as a rule, prefers to display buffers on
another frame. Such a user might provide the following customization:

```
(setopt
 display-buffer-base-action
 '((display-buffer-reuse-window display-buffer-pop-up-frame)
   (reusable-frames . 0)))
```

This setting will cause `display-buffer` to first try to find a
window showing the buffer on a visible or iconified frame and, if no
such frame exists, pop up a new frame. You can observe this behavior
on a graphical system by typing `C-x 1` in the window showing
`*scratch*` and evaluating our canonical `display-buffer`
form. This will usually create (and give focus to) a new frame whose
root window shows `*foo*`. Iconify that frame and evaluate the
canonical form again: `display-buffer` will reuse the window on
the new frame (usually raising the frame and giving it focus too).

Only if creating a new frame fails, `display-buffer` will
apply the actions supplied by `display-buffer-fallback-action`
which means to again try reusing a window, popping up a new window and
so on. A trivial way to make frame creation fail is supplied by the
following form:

```
(let ((pop-up-frame-function 'ignore))
  (display-buffer (get-buffer-create "*foo*")))
```

We will forget about that form immediately after observing that it
fails to create a new frame and uses a fallback action instead.

Note that `display-buffer-reuse-window` appears redundant in
the customization of `display-buffer-base-action` because it is
already part of `display-buffer-fallback-action` and should be
tried there anyway. However, that would fail because due to the
precedence of `display-buffer-base-action` over
`display-buffer-fallback-action`, at that time
`display-buffer-pop-up-frame` would have already won the race.
In fact, this:

```
(setopt
 display-buffer-base-action
 '(display-buffer-pop-up-frame (reusable-frames . 0)))
```

would cause `display-buffer` to *always* pop up a new frame
which is probably not what our user wants.

So far, we have only shown how *users* can customize the
default behavior of `display-buffer`. Let us now see how
*applications* can change the course of `display-buffer`.
The canonical way to do that is to use the action argument of
`display-buffer` or a function that calls it, like, for example,
`pop-to-buffer` (see [Switching to a Buffer in a Window](#Switching-Buffers)).

Suppose an application wants to display `*foo*` preferably
below the selected window (to immediately attract the attention of the
user to the new window) or, if that fails, in a window at the bottom
of the frame. It could do that with a call like this:

```
(display-buffer
 (get-buffer-create "*foo*")
 '((display-buffer-below-selected display-buffer-at-bottom)))
```

In order to see how this new, modified form works, delete any frame
showing `*foo*`, type `C-x 1` followed by `C-x 2` in the
window showing `*scratch*`, and subsequently evaluate that form.
`display-buffer` should split the upper window, and show
`*foo*` in the new window. Alternatively, if after `C-x 2`
you had typed `C-x o`, `display-buffer` would have split the
window at the bottom instead.

Suppose now that, before evaluating the new form, you have made the
selected window as small as possible, for example, by evaluating the
form `(fit-window-to-buffer)` in that window. In that case,
`display-buffer` would have failed to split the selected window
and would have split the frame’s root window instead, effectively
displaying `*foo*` at the bottom of the frame.

In either case, evaluating the new form a second time should reuse
the window already showing `*foo*` since both functions supplied
by the action argument try to reuse such a window first.

By setting the action argument, an application effectively
overrules any customization of `display-buffer-base-action`. Our
user can now either accept the choice of the application, or redouble
by customizing the option `display-buffer-alist` as follows:

```
(setopt
 display-buffer-alist
 '(("\\*foo\\*"
    (display-buffer-reuse-window display-buffer-pop-up-frame))))
```

Trying this with the new, modified form above in a configuration that
does not show `*foo*` anywhere, will display `*foo*` on a
separate frame, completely ignoring the action argument of
`display-buffer`.

Note that we didn’t care to specify a `reusable-frames` action
alist entry in our specification of `display-buffer-alist`.
`display-buffer` always takes the first one it finds—in our
case the one specified by `display-buffer-base-action`. If we
wanted to use a different specification, for example, to exclude
iconified frames showing `*foo*` from the list of reusable ones,
we would have to specify that separately, however:

```
(setopt
 display-buffer-alist
 '(("\\*foo\\*"
    (display-buffer-reuse-window display-buffer-pop-up-frame)
    (reusable-frames . visible))))
```

If you try this, you will notice that repeated attempts to display
`*foo*` will succeed to reuse a frame only if that frame is
visible.

The above example would allow the conclusion that users customize
`display-buffer-alist` for the sole purpose to overrule the
action argument chosen by applications. Such a conclusion would
be incorrect. `display-buffer-alist` is the standard option for
users to direct the course of display of specific buffers in a
preferred way regardless of whether the display is also guided by an
action argument.

We can, however, reasonably conclude that customizing
`display-buffer-alist` differs from customizing
`display-buffer-base-action` in two major aspects: it is stronger
because it overrides the action argument of
`display-buffer`, and it enables you to explicitly specify the
affected buffers. In fact, displaying other buffers is not affected
in any way by a customization for `*foo*`. For example,

```
(display-buffer (get-buffer-create "*bar*"))
```

continues being governed by the settings of
`display-buffer-base-action` and
`display-buffer-fallback-action` only.

We could stop with our examples here but Lisp programs still have
an ace up their sleeves which they can use to overrule any
customization of `display-buffer-alist`. It’s the variable
`display-buffer-overriding-action` which they can bind around
`display-buffer` calls as follows:

```
(let ((display-buffer-overriding-action
       '((display-buffer-same-window))))
  (display-buffer
   (get-buffer-create "*foo*")
   '((display-buffer-below-selected display-buffer-at-bottom))))
```

Evaluating this form will usually display `*foo*` in the selected
window regardless of the action argument and any user
customizations. (Usually, an application will not bother to also
provide an action argument. Here it just serves to illustrate
the fact that it gets overridden.)

It might be illustrative to look at the list of action functions
`display-buffer` would have tried to display `*foo*` with
the customizations we provided here. The list (including comments
explaining who added this and the subsequent elements) is:

```
(display-buffer-same-window  ;; `display-buffer-overriding-action'
 display-buffer-reuse-window ;; `display-buffer-alist'
 display-buffer-pop-up-frame
 display-buffer-below-selected ;; ACTION argument
 display-buffer-at-bottom
 display-buffer-reuse-window ;; `display-buffer-base-action'
 display-buffer-pop-up-frame
 display-buffer--maybe-same-window ;; `display-buffer-fallback-action'
 display-buffer-reuse-window
 display-buffer--maybe-pop-up-frame-or-window
 display-buffer-in-previous-window
 display-buffer-use-some-window
 display-buffer-pop-up-frame)
```

Note that among the internal functions listed here,
`display-buffer--maybe-same-window` is effectively ignored while
`display-buffer--maybe-pop-up-frame-or-window` actually runs
`display-buffer-pop-up-window`.

The action alist passed in each function call is:

```
((reusable-frames . visible)
 (reusable-frames . 0))
```

which shows that we have used the second specification of
`display-buffer-alist` above, overriding the specification
supplied by `display-buffer-base-action`. Suppose our user had
written that as

```
(setopt
 display-buffer-alist
 '(("\\*foo\\*"
    (display-buffer-reuse-window display-buffer-pop-up-frame)
    (inhibit-same-window . t)
    (reusable-frames . visible))))
```

In this case the `inhibit-same-window` alist entry will
successfully invalidate the `display-buffer-same-window`
specification from `display-buffer-overriding-action` and
`display-buffer` will show `*foo*` on another frame. To
make `display-buffer-overriding-action` more robust in this
regard, the application would have to specify an appropriate
`inhibit-same-window` entry too, for example, as follows:

```
(let ((display-buffer-overriding-action
       '(display-buffer-same-window (inhibit-same-window . nil))))
  (display-buffer (get-buffer-create "*foo*")))
```

This last example shows that while the precedence order of action
functions is fixed, as described in [Choosing a Window for Displaying a Buffer](#Choosing-Window), an action
alist entry specified by a display action ranked lower in that order
can affect the execution of a higher ranked display action.

---

Previous: [Precedence of Action Functions](#Precedence-of-Action-Functions), Up: [Displaying a Buffer in a Suitable Window](#Displaying-Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 29.15.6 The Zen of Buffer Display [¶](#The-Zen-of-Buffer-Display-1)

In its most simplistic form, a frame accommodates always one single
window that can be used for displaying a buffer. As a consequence, it
is always the latest call of `display-buffer` that will have
succeeded in placing its buffer there.

Since working with such a frame is not very practical, Emacs by
default allows for more complex frame layouts controlled by the
default values of the frame size and the `split-height-threshold`
and `split-width-threshold` options. Displaying a buffer not yet
shown on a frame then either splits the single window on that frame or
(re-)uses one of its two windows.

The default behavior is abandoned as soon as the user customizes
one of these thresholds or manually changes the frame’s layout. The
default behavior is also abandoned when calling `display-buffer`
with a non-`nil` action argument or the user customizes one
of the options mentioned in the previous subsections. Mastering
`display-buffer` soon may become a frustrating experience due to
the plethora of applicable display actions and the resulting frame
layouts.

However, refraining from using buffer display functions and falling
back on a split & delete windows metaphor is not a good idea either.
Buffer display functions give Lisp programs and users a framework to
reconcile their different needs; no comparable framework exists for
splitting and deleting windows. Buffer display functions also allow
to at least partially restore the layout of a frame when removing a
buffer from it later (see [Quitting Windows](#Quitting-Windows)).

Below we will give a number of guidelines to redeem the frustration
mentioned above and thus to avoid literally losing buffers in-between
the windows of a frame.

Write display actions without stress
:   Writing display actions can be a pain because one has to lump together
    action functions and action alists in one huge list. (Historical
    reasons prevented us from having `display-buffer` support
    separate arguments for these.) It might help to memorize some basic
    forms like the ones listed below:

    ```
    '(nil (inhibit-same-window . t))
    ```

    specifies an action alist entry only and no action function. Its sole
    purpose is to inhibit a `display-buffer-same-window` function
    specified elsewhere from showing the buffer in the same window, see
    also the last example of the preceding subsection.

    ```
    '(display-buffer-below-selected)
    ```

    on the other hand, specifies one action function and an empty action
    alist. To combine the effects of the above two specifications one
    would write the form

    ```
    '(display-buffer-below-selected (inhibit-same-window . t))
    ```

    to add another action function one would write

    ```
    '((display-buffer-below-selected display-buffer-at-bottom)
      (inhibit-same-window . t))
    ```

    and to add another alist entry one would write

    ```
    '((display-buffer-below-selected display-buffer-at-bottom)
      (inhibit-same-window . t)
      (window-height . fit-window-to-buffer))
    ```

    That last form can be used as action argument of
    `display-buffer` in the following way:

    ```
    (display-buffer
     (get-buffer-create "*foo*")
     '((display-buffer-below-selected display-buffer-at-bottom)
       (inhibit-same-window . t)
       (window-height . fit-window-to-buffer)))
    ```

    In a customization of `display-buffer-alist` it would be used as
    follows:

    ```
    (setopt
     display-buffer-alist
     '(("\\*foo\\*"
        (display-buffer-below-selected display-buffer-at-bottom)
        (inhibit-same-window . t)
        (window-height . fit-window-to-buffer))))
    ```

    To add a customization for a second buffer one would then write:

    ```
    (setopt
     display-buffer-alist
     '(("\\*foo\\*"
        (display-buffer-below-selected display-buffer-at-bottom)
        (inhibit-same-window . t)
        (window-height . fit-window-to-buffer))
       ("\\*bar\\*"
        (display-buffer-reuse-window display-buffer-pop-up-frame)
        (reusable-frames . visible))))
    ```

Treat each other with respect
:   `display-buffer-alist` and `display-buffer-base-action` are
    user options—Lisp programs must never set or rebind them.
    `display-buffer-overriding-action`, on the other hand, is
    reserved for applications—who seldom use that option and if they use
    it, then with utmost care.

    Older implementations of `display-buffer` frequently caused
    users and applications to fight over the settings of user options like
    `pop-up-frames` and `pop-up-windows` (see [Additional Options for Displaying Buffers](#Choosing-Window-Options)). This was one major reason for redesigning
    `display-buffer`—to provide a clear framework specifying what
    users and applications should be allowed to do.

    Lisp programs must be prepared that user customizations may
    cause buffers to get displayed in an unexpected way. They should
    never assume in their subsequent behavior, that the buffer has been
    shown precisely the way they asked for in the action argument of
    `display-buffer`.

    Users should not pose too many and too severe restrictions on how
    arbitrary buffers get displayed. Otherwise, they will risk to lose
    the characteristics of showing a buffer for a certain purpose.
    Suppose a Lisp program has been written to compare different versions
    of a buffer in two windows side-by-side. If the customization of
    `display-buffer-alist` prescribes that any such buffer should be
    always shown in or below the selected window, the program will have a
    hard time to set up the desired window configuration via
    `display-buffer`.
