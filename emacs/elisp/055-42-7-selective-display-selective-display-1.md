### 42.7 Selective Display [¶](#Selective-Display-1)

*Selective display* refers to a pair of related features for
hiding certain lines on the screen.

The first variant, explicit selective display, was designed for use in a Lisp
program: it controls which lines are hidden by altering the text. This kind of
hiding is now obsolete and deprecated; instead you should use the
`invisible` property (see [Invisible Text](#Invisible-Text)) to get the same effect.

In the second variant, the choice of lines to hide is made
automatically based on indentation. This variant is designed to be a
user-level feature.

The way you control explicit selective display is by replacing a
newline (control-j) with a carriage return (control-m). The text that
was formerly a line following that newline is now hidden. Strictly
speaking, it is temporarily no longer a line at all, since only
newlines can separate lines; it is now part of the previous line.

Selective display does not directly affect editing commands. For
example, `C-f` (`forward-char`) moves point unhesitatingly
into hidden text. However, the replacement of newline characters with
carriage return characters affects some editing commands. For
example, `next-line` skips hidden lines, since it searches only
for newlines. Modes that use selective display can also define
commands that take account of the newlines, or that control which
parts of the text are hidden.

When you write a selectively displayed buffer into a file, all the
control-m’s are output as newlines. This means that when you next read
in the file, it looks OK, with nothing hidden. The selective display
effect is seen only within Emacs.

Variable: **selective-display** [¶](#index-selective_002ddisplay)
:   This buffer-local variable enables selective display. This means that
    lines, or portions of lines, may be made hidden.

    * If the value of `selective-display` is `t`, then the character
      control-m marks the start of hidden text; the control-m, and the rest
      of the line following it, are not displayed. This is explicit selective
      display.
    * If the value of `selective-display` is a positive integer, then
      lines that start with more than that many columns of indentation are not
      displayed.

    When some portion of a buffer is hidden, the vertical movement
    commands operate as if that portion did not exist, allowing a single
    `next-line` command to skip any number of hidden lines.
    However, character movement commands (such as `forward-char`) do
    not skip the hidden portion, and it is possible (if tricky) to insert
    or delete text in a hidden portion.

    In the examples below, we show the *display appearance* of the
    buffer `foo`, which changes with the value of
    `selective-display`. The *contents* of the buffer do not
    change.

    ```
    (setq selective-display nil)
         ⇒ nil

    ---------- Buffer: foo ----------
    1 on this column
     2on this column
      3n this column
      3n this column
     2on this column
    1 on this column
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (setq selective-display 2)
         ⇒ 2

    ---------- Buffer: foo ----------
    1 on this column
     2on this column
     2on this column
    1 on this column
    ---------- Buffer: foo ----------
    ```

User Option: **selective-display-ellipses** [¶](#index-selective_002ddisplay_002dellipses)
:   If this buffer-local variable is non-`nil`, then Emacs displays
    ‘`…`’ at the end of a line that is followed by hidden text.
    This example is a continuation of the previous one.

    ```
    (setq selective-display-ellipses t)
         ⇒ t

    ---------- Buffer: foo ----------
    1 on this column
     2on this column ...
     2on this column
    1 on this column
    ---------- Buffer: foo ----------
    ```

    You can use a display table to substitute other text for the ellipsis
    (‘`…`’). See [Display Tables](#Display-Tables).

---

Next: [Overlays](#Overlays), Previous: [Selective Display](#Selective-Display), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.8 Temporary Displays [¶](#Temporary-Displays-1)

Temporary displays are used by Lisp programs to put output into a
buffer and then present it to the user for perusal rather than for
editing. Many help commands use this feature.

Macro: **with-output-to-temp-buffer** buffer-name body… [¶](#index-with_002doutput_002dto_002dtemp_002dbuffer)
:   This function executes the forms in body while arranging to insert
    any output they print into the buffer named buffer-name, which is
    first created if necessary, and put into Help mode. (See the similar
    form `with-temp-buffer-window` below.) Finally, the buffer is
    displayed in some window, but that window is not selected.

    If the forms in body do not change the major mode in the output
    buffer, so that it is still Help mode at the end of their execution,
    then `with-output-to-temp-buffer` makes this buffer read-only at
    the end, and also scans it for function and variable names to make them
    into clickable cross-references. See [Tips for
    Documentation Strings](#Docstring-hyperlinks), in particular the item on hyperlinks in
    documentation strings, for more details.

    The string buffer-name specifies the temporary buffer, which need
    not already exist. The argument must be a string, not a buffer. The
    buffer is erased initially (with no questions asked), and it is marked
    as unmodified after `with-output-to-temp-buffer` exits.

    `with-output-to-temp-buffer` binds `standard-output` to the
    temporary buffer, then it evaluates the forms in body. Output
    using the Lisp output functions within body goes by default to
    that buffer (but screen display and messages in the echo area, although
    they are “output” in the general sense of the word, are not affected).
    See [Output Functions](#Output-Functions).

    Several hooks are available for customizing the behavior
    of this construct; they are listed below.

    The value of the last form in body is returned.

    ```
    ---------- Buffer: foo ----------
     This is the contents of foo.
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (with-output-to-temp-buffer "foo"
        (print 20)
        (print standard-output))
    ⇒ #<buffer foo>

    ---------- Buffer: foo ----------

    20

    #<buffer foo>

    ---------- Buffer: foo ----------
    ```

User Option: **temp-buffer-show-function** [¶](#index-temp_002dbuffer_002dshow_002dfunction)
:   If this variable is non-`nil`, `with-output-to-temp-buffer`
    calls it as a function to do the job of displaying a help buffer. The
    function gets one argument, which is the buffer it should display.

    It is a good idea for this function to run `temp-buffer-show-hook`
    just as `with-output-to-temp-buffer` normally would, inside of
    `save-selected-window` and with the chosen window and buffer
    selected.

Variable: **temp-buffer-setup-hook** [¶](#index-temp_002dbuffer_002dsetup_002dhook)
:   This normal hook is run by `with-output-to-temp-buffer` before
    evaluating body. When the hook runs, the temporary buffer is
    current. This hook is normally set up with a function to put the
    buffer in Help mode.

Variable: **temp-buffer-show-hook** [¶](#index-temp_002dbuffer_002dshow_002dhook)
:   This normal hook is run by `with-output-to-temp-buffer` after
    displaying the temporary buffer. When the hook runs, the temporary buffer
    is current, and the window it was displayed in is selected.

Macro: **with-temp-buffer-window** buffer-or-name action quit-function body… [¶](#index-with_002dtemp_002dbuffer_002dwindow)
:   This macro is similar to `with-output-to-temp-buffer`. Like that
    construct, it executes body while arranging to insert any output
    it prints into the buffer named buffer-or-name and displays that
    buffer in some window. Unlike `with-output-to-temp-buffer`,
    however, it does not automatically switch that buffer to Help mode.

    The argument buffer-or-name specifies the temporary buffer. It
    can be either a buffer, which must already exist, or a string, in which
    case a buffer of that name is created, if necessary. The buffer is
    marked as unmodified and read-only when `with-temp-buffer-window`
    exits.

    This macro does not call `temp-buffer-show-function`. Rather, it
    passes the action argument to `display-buffer`
    (see [Choosing a Window for Displaying a Buffer](#Choosing-Window)) in order to display the buffer.

    The value of the last form in body is returned, unless the
    argument quit-function is specified. In that case, it is called
    with two arguments: the window showing the buffer and the result of
    body. The final return value is then whatever quit-function
    returns.

    This macro uses the normal hooks `temp-buffer-window-setup-hook`
    and `temp-buffer-window-show-hook` in place of the analogous hooks
    run by `with-output-to-temp-buffer`.

The two constructs described next are mostly identical to
`with-temp-buffer-window` but differ from it as specified:

Macro: **with-current-buffer-window** buffer-or-name action quit-function &rest body [¶](#index-with_002dcurrent_002dbuffer_002dwindow)
:   This macro is like `with-temp-buffer-window` but unlike that makes
    the buffer specified by buffer-or-name current for running
    body.

A window showing a temporary buffer can be fitted to the size of that
buffer using the following mode:

User Option: **temp-buffer-resize-mode** [¶](#index-temp_002dbuffer_002dresize_002dmode)
:   When this minor mode is enabled, windows showing a temporary buffer are
    automatically resized to fit their buffer’s contents.

    A window is resized if and only if it has been specially created for the
    buffer. In particular, windows that have shown another buffer before
    are not resized. By default, this mode uses `fit-window-to-buffer`
    (see [Resizing Windows](#Resizing-Windows)) for resizing. You can specify a different
    function by customizing the options `temp-buffer-max-height` and
    `temp-buffer-max-width` below.

    The effect of this option can be overridden by providing a suitable
    `window-height`, `window-width` or `window-size` action
    alist entry for `display-buffer` (see [Action Alists for Buffer Display](#Buffer-Display-Action-Alists)).

User Option: **temp-buffer-max-height** [¶](#index-temp_002dbuffer_002dmax_002dheight)
:   This option specifies the maximum height (in lines) of a window
    displaying a temporary buffer when `temp-buffer-resize-mode` is
    enabled. It can also be a function to be called to choose the height
    for such a buffer. It gets one argument, the buffer, and should return
    a positive integer. At the time the function is called, the window to
    be resized is selected.

User Option: **temp-buffer-max-width** [¶](#index-temp_002dbuffer_002dmax_002dwidth)
:   This option specifies the maximum width of a window (in columns)
    displaying a temporary buffer when `temp-buffer-resize-mode` is
    enabled. It can also be a function to be called to choose the width for
    such a buffer. It gets one argument, the buffer, and should return a
    positive integer. At the time the function is called, the window to be
    resized is selected.

The following function uses the current buffer for temporary display:

Function: **momentary-string-display** string position &optional char message [¶](#index-momentary_002dstring_002ddisplay)
:   This function momentarily displays string in the current buffer at
    position. It has no effect on the undo list or on the buffer’s
    modification status.

    The momentary display remains until the next input event. If the next
    input event is char, `momentary-string-display` ignores it
    and returns. Otherwise, that event remains buffered for subsequent use
    as input. Thus, typing char will simply remove the string from
    the display, while typing (say) `C-f` will remove the string from
    the display and later (presumably) move point forward. The argument
    char is a space by default.

    The return value of `momentary-string-display` is not meaningful.

    If the string string does not contain control characters, you can
    do the same job in a more general way by creating (and then subsequently
    deleting) an overlay with a `before-string` property.
    See [Overlay Properties](#Overlay-Properties).

    If message is non-`nil`, it is displayed in the echo area
    while string is displayed in the buffer. If it is `nil`, a
    default message says to type char to continue.

    In this example, point is initially located at the beginning of the
    second line:

    ```
    ---------- Buffer: foo ----------
    This is the contents of foo.
    ∗Second line.
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (momentary-string-display
      "**** Important Message! ****"
      (point) ?\r
      "Type RET when done reading")
    ⇒ t
    ```

    ```

    ```

    ```
    ---------- Buffer: foo ----------
    This is the contents of foo.
    **** Important Message! ****Second line.
    ---------- Buffer: foo ----------

    ---------- Echo Area ----------
    Type RET when done reading
    ---------- Echo Area ----------
    ```

---

Next: [Size of Displayed Text](#Size-of-Displayed-Text), Previous: [Temporary Displays](#Temporary-Displays), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.9 Overlays [¶](#Overlays-1)

You can use *overlays* to alter the appearance of a buffer’s text on
the screen, for the sake of presentation features. An overlay is an
object that belongs to a particular buffer, and has a specified
beginning and end. It also has properties that you can examine and set;
these affect the display of the text within the overlaid portion of
the buffer.

Editing the text of the buffer adjusts the beginning and end of each
overlay so that it stays with the text. When you create the overlay,
you can specify whether text inserted at the beginning should be
inside the overlay or outside, and likewise for the end of the overlay.

* [Managing Overlays](#Managing-Overlays)
* [Overlay Properties](#Overlay-Properties)
* [Searching for Overlays](#Finding-Overlays)

---

Next: [Overlay Properties](#Overlay-Properties), Up: [Overlays](#Overlays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.9.1 Managing Overlays [¶](#Managing-Overlays-1)

This section describes the functions to create, delete and move
overlays, and to examine their contents. Overlay changes are not
recorded in the buffer’s undo list, since the overlays are not
considered part of the buffer’s contents.

Function: **overlayp** object [¶](#index-overlayp)
:   This function returns `t` if object is an overlay.

Function: **make-overlay** start end &optional buffer front-advance rear-advance [¶](#index-make_002doverlay)
:   This function creates and returns an overlay that belongs to
    buffer and ranges from start to end. Both start
    and end must specify buffer positions; they may be integers or
    markers. If buffer is omitted, the overlay is created in the
    current buffer.

    An overlay whose start and end specify the same buffer
    position is known as *empty*. A non-empty overlay can become
    empty if the text between its start and end is deleted.
    When that happens, the overlay is by default not deleted, but you can
    cause it to be deleted by giving it the ‘`evaporate`’ property
    (see [evaporate property](#Overlay-Properties)).

    The arguments front-advance and rear-advance specify what
    happens when text is inserted at the beginning (i.e., before
    start) and at the end. If they are both `nil`, the
    default, then the overlay extends to include any text inserted at the
    beginning, but not text inserted at the end. If front-advance
    is non-`nil`, text inserted at the beginning of the overlay is
    excluded from the overlay. If rear-advance is non-`nil`,
    text inserted at the end of the overlay is included in the overlay.

Function: **overlay-start** overlay [¶](#index-overlay_002dstart)
:   This function returns the position at which overlay starts,
    as an integer.

Function: **overlay-end** overlay [¶](#index-overlay_002dend)
:   This function returns the position at which overlay ends,
    as an integer.

Function: **overlay-buffer** overlay [¶](#index-overlay_002dbuffer)
:   This function returns the buffer that overlay belongs to. It
    returns `nil` if overlay has been deleted.

Function: **delete-overlay** overlay [¶](#index-delete_002doverlay)
:   This function deletes the specified overlay. The overlay
    continues to exist as a Lisp object, and its property list is
    unchanged, but it ceases to be attached to the buffer it belonged to,
    and ceases to have any effect on display.

    A deleted overlay is not permanently disconnected. You can give it a
    position in a buffer again by calling `move-overlay`.

Function: **move-overlay** overlay start end &optional buffer [¶](#index-move_002doverlay)
:   This function moves overlay to buffer, and places its
    bounds at start and end in that buffer. Both arguments
    start and end must specify buffer positions; they may be
    integers or markers.

    If buffer is omitted, overlay stays in the same buffer it
    was already associated with; if overlay was previously deleted
    (and thus isn’t associated with any buffer), it goes into the current
    buffer.

    The return value is overlay.

    This function is the only valid way to change the endpoints of an
    overlay.

Function: **remove-overlays** &optional start end name value [¶](#index-remove_002doverlays)
:   This function clears the text in the region between start and
    end of any overlays whose property named name has the
    specified value, such that no such overlay will affect the text in
    the region. To do this, the function can remove overlays in the region,
    or move their endpoints, or split them, or do some combination of these.
    Specifically:

    * Overlays that start at or after start and end before end
      will be removed completely.
    * Overlays that start before start and end after start, but
      before end, will be altered so that they end at start.
    * Overlays that start at or after start and end after end will
      be altered so that they start at end.
    * Overlays that start before start and end after end will be
      split into two overlays: one that ends at start and the other that
      begins at end.
    * Empty overlays (see [empty overlays](#empty-overlays)) will be removed if they are
      either at start, or strictly between start and end
      excluding end, or at end when end denotes the position
      at the end of the buffer.

    If name is omitted or `nil`, it means to delete/modify all
    overlays that affect text in the specified region. If start
    and/or end are omitted or `nil`, they default to the
    beginning and end of the buffer, respectively. Therefore,
    `(remove-overlays)` removes all the overlays in the current buffer.

    Values of the named overlay property are compared using `eq`, which
    is important if the values are anything but symbols or fixnums
    (see [Equality Predicates](#Equality-Predicates)). It means the values passed to the
    function must be the same values used to set the overlay property, not
    their copies; objects which are different will not compare equal even if
    they have identical contents.

    The optional arguments name and value should either both be
    passed and non-`nil`, or both omitted or `nil`.

Function: **copy-overlay** overlay [¶](#index-copy_002doverlay)
:   This function returns a copy of overlay. The copy has the same
    endpoints and properties as overlay. However, the text
    insertion type for the start of the overlay and for the end of the
    overlay are set to their default values.

Here are some examples:

```
;; Create an overlay.
(setq foo (make-overlay 1 10))
     ⇒ #<overlay from 1 to 10 in display.texi>
```

```
(overlay-start foo)
     ⇒ 1
```

```
(overlay-end foo)
     ⇒ 10
(overlay-buffer foo)
     ⇒ #<buffer display.texi>
;; Give it a property we can check later.
(overlay-put foo 'happy t)
     ⇒ t
;; Verify the property is present.
(overlay-get foo 'happy)
     ⇒ t
```

```
;; Move the overlay.
(move-overlay foo 5 20)
     ⇒ #<overlay from 5 to 20 in display.texi>
(overlay-start foo)
     ⇒ 5
(overlay-end foo)
     ⇒ 20
```

```
;; Delete the overlay.
(delete-overlay foo)
     ⇒ nil
;; Verify it is deleted.
foo
     ⇒ #<overlay in no buffer>
;; A deleted overlay has no position.
(overlay-start foo)
     ⇒ nil
(overlay-end foo)
     ⇒ nil
(overlay-buffer foo)
     ⇒ nil
```

```
;; Undelete the overlay.
(move-overlay foo 1 20)
     ⇒ #<overlay from 1 to 20 in display.texi>
;; Verify the results.
(overlay-start foo)
     ⇒ 1
(overlay-end foo)
     ⇒ 20
(overlay-buffer foo)
     ⇒ #<buffer display.texi>
```

```
;; Moving and deleting the overlay does not change its properties.
(overlay-get foo 'happy)
     ⇒ t
```

---

Next: [Searching for Overlays](#Finding-Overlays), Previous: [Managing Overlays](#Managing-Overlays), Up: [Overlays](#Overlays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.9.2 Overlay Properties [¶](#Overlay-Properties-1)

Overlay properties are like text properties in that the properties that
alter how a character is displayed can come from either source. But in
most respects they are different. See [Text Properties](#Text-Properties), for comparison.

Text properties are considered a part of the text; overlays and
their properties are specifically considered not to be part of the
text. Thus, copying text between various buffers and strings
preserves text properties, but does not try to preserve overlays.
Changing a buffer’s text properties marks the buffer as modified,
while moving an overlay or changing its properties does not. Unlike
text property changes, overlay property changes are not recorded in
the buffer’s undo list.

Since more than one overlay can specify a property value for the
same character, Emacs lets you specify a priority value of each
overlay. The priority value is used to decide which of the
overlapping overlays will “win”.

These functions read and set the properties of an overlay:

Function: **overlay-get** overlay prop [¶](#index-overlay_002dget)
:   This function returns the value of property prop recorded in
    overlay, if any. If overlay does not record any value for
    that property, but it does have a `category` property which is a
    symbol, that symbol’s prop property is used. Otherwise, the value
    is `nil`.

Function: **overlay-put** overlay prop value [¶](#index-overlay_002dput)
:   This function sets the value of property prop recorded in
    overlay to value. It returns value.

Function: **overlay-properties** overlay [¶](#index-overlay_002dproperties)
:   This returns a copy of the property list of overlay.

See also the function `get-char-property` which checks both
overlay properties and text properties for a given character.
See [Examining Text Properties](#Examining-Properties).

Many overlay properties have special meanings; here is a table
of them:

`priority` [¶](#index-priority-_0028overlay-property_0029)
:   This property’s value determines the priority of the overlay. If you
    want to specify a priority value, use either `nil` (or zero), or
    a positive integer, or a cons of two values. Any other value triggers
    undefined behavior.

    The priority matters when two or more overlays cover the same
    character and both specify the same property with different values;
    the one whose `priority` value is higher overrides the other.
    (For the `face` property, the higher priority overlay’s value
    does not completely override the other value; instead, its individual
    face attributes override the corresponding face attributes of the
    `face` property whose priority is lower.) If two overlays have
    the same priority value, and one is “nested” in the other (i.e.,
    covers fewer buffer or string positions), then the inner one will
    prevail over the outer one. If neither is nested in the other then
    you should not make assumptions about which overlay will prevail.

    When a Lisp program puts overlays with defined priorities on text that
    might have overlays without priorities, this could cause undesirable
    results, because any overlay with a positive priority value will
    override all the overlays without a priority. Since most Emacs
    features that use overlays don’t specify priorities for their
    overlays, integer priorities should be used with care. Instead of
    using integer priorities and risk overriding other overlays, you can
    use priority values of the form `(primary . secondary)`,
    where the primary value is used as described above, and
    secondary is the fallback value used when primary and the
    nesting considerations fail to resolve the precedence between
    overlays. In particular, priority value `(nil . n)`,
    with n a positive integer, enables you to have the overlays
    ordered by priority when necessary without completely overriding other
    overlays.

    Currently, all overlays take priority over text properties.

    If you need to put overlays in priority order, use the sorted
    argument of `overlays-at`. See [Searching for Overlays](#Finding-Overlays).

`window` [¶](#index-window-_0028overlay-property_0029)
:   If the `window` property is non-`nil`, then the overlay
    applies only on that window.

`category` [¶](#index-category-_0028overlay-property_0029)
:   If an overlay has a `category` property, we call it the
    *category* of the overlay. It should be a symbol. The properties
    of the symbol serve as defaults for the properties of the overlay.

`face` [¶](#index-face-_0028overlay-property_0029)
:   This property controls the appearance of the text (see [Faces](#Faces)).
    The value of the property can be the following:

    * A face name (a symbol or string).
    * An anonymous face: a property list of the form `(keyword
      value …)`, where each keyword is a face attribute
      name and value is a value for that attribute.
    * A list of faces. Each list element should be either a face name or an
      anonymous face. This specifies a face which is an aggregate of the
      attributes of each of the listed faces. Faces occurring earlier in
      the list have higher priority.
    * A cons cell of the form `(foreground-color . color-name)`
      or `(background-color . color-name)`. This specifies the
      foreground or background color, similar to `(:foreground
      color-name)` or `(:background color-name)`. This
      form is supported for backward compatibility only, and should be
      avoided.

`mouse-face` [¶](#index-mouse_002dface-_0028overlay-property_0029)
:   This property is used instead of `face` when the mouse is within
    the range of the overlay. However, Emacs ignores all face attributes
    from this property that alter the text size (e.g., `:height`,
    `:weight`, and `:slant`); those attributes are always the
    same as in the unhighlighted text.

`display` [¶](#index-display-_0028overlay-property_0029)
:   This property activates various features that change the
    way text is displayed. For example, it can make text appear taller
    or shorter, higher or lower, wider or narrower, or replaced with an image.
    See [The `display` Property](#Display-Property). Note that, if the `display` property is a
    *replacing* one (see [Display Specs That Replace The Text](#Replacing-Specs)), the `invisible`
    property of the same overlay will be ignored.

`help-echo` [¶](#index-help_002decho-_0028overlay-property_0029)
:   If an overlay has a `help-echo` property, then when you move the
    mouse onto the text in the overlay, Emacs displays a help string in
    the echo area, or as a tooltip. For details see [Text help-echo](#Text-help_002decho).

`field` [¶](#index-field-_0028overlay-property_0029)
:   Consecutive characters with the same `field` property constitute a
    *field*. Some motion functions including `forward-word` and
    `beginning-of-line` stop moving at a field boundary.
    See [Defining and Using Fields](#Fields).

`modification-hooks` [¶](#index-modification_002dhooks-_0028overlay-property_0029)
:   This property’s value is a list of functions to be called if any
    character within the overlay is changed or if text is inserted strictly
    within the overlay.

    The hook functions are called both before and after each change.
    If the functions save the information they receive, and compare notes
    between calls, they can determine exactly what change has been made
    in the buffer text.

    When called before a change, each function receives four arguments: the
    overlay, `nil`, and the beginning and end of the text range to be
    modified.

    When called after a change, each function receives five arguments: the
    overlay, `t`, the beginning and end of the text range just
    modified, and the length of the pre-change text replaced by that range.
    (For an insertion, the pre-change length is zero; for a deletion, that
    length is the number of characters deleted, and the post-change
    beginning and end are equal.)

    When these functions are called, `inhibit-modification-hooks` is
    bound to non-`nil`. If the functions modify the buffer, you
    might want to bind `inhibit-modification-hooks` to `nil`, so
    as to cause the change hooks to run for these modifications. However,
    doing this may call your own change hook recursively, so be sure to
    prepare for that. See [Change Hooks](#Change-Hooks).

    Text properties also support the `modification-hooks` property,
    but the details are somewhat different (see [Properties with Special Meanings](#Special-Properties)).

`insert-in-front-hooks` [¶](#index-insert_002din_002dfront_002dhooks-_0028overlay-property_0029)
:   This property’s value is a list of functions to be called before and
    after inserting text right at the beginning of the overlay. The calling
    conventions are the same as for the `modification-hooks` functions.

`insert-behind-hooks` [¶](#index-insert_002dbehind_002dhooks-_0028overlay-property_0029)
:   This property’s value is a list of functions to be called before and
    after inserting text right at the end of the overlay. The calling
    conventions are the same as for the `modification-hooks` functions.

`invisible` [¶](#index-invisible-_0028overlay-property_0029)
:   The `invisible` property can make the text in the overlay
    invisible, which means that it does not appear on the screen. However,
    if the text covered by the overlay has a *replacing* `display`
    property, the `invisible` property will be ignored, because a
    replacing `display` property skips the text without examining its
    properties. See [Invisible Text](#Invisible-Text), for details.

`intangible` [¶](#index-intangible-_0028overlay-property_0029)
:   The `intangible` property on an overlay works just like the
    `intangible` text property. It is obsolete. See [Properties with Special Meanings](#Special-Properties), for details.

`isearch-open-invisible`
:   This property tells incremental search (see [Incremental Search](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Incremental-Search) in The GNU Emacs Manual) how to make an invisible overlay
    visible, permanently, if the final match overlaps it. See [Invisible Text](#Invisible-Text).

`isearch-open-invisible-temporary`
:   This property tells incremental search how to make an invisible overlay
    visible, temporarily, during the search. See [Invisible Text](#Invisible-Text).

`before-string` [¶](#index-before_002dstring-_0028overlay-property_0029)
:   This property’s value is a string to add to the display at the beginning
    of the overlay. The string does not appear in the buffer in any
    sense—only on the screen. Note that if the text at the beginning of
    the overlay is made invisible, the string will not be displayed.

`after-string` [¶](#index-after_002dstring-_0028overlay-property_0029)
:   This property’s value is a string to add to the display at the end of
    the overlay. The string does not appear in the buffer in any
    sense—only on the screen. Note that if the text at the end of the
    overlay is made invisible, the string will not be displayed.

`line-prefix`
:   This property specifies a display spec to prepend to each
    non-continuation line at display-time. See [Truncation](#Truncation).

`wrap-prefix`
:   This property specifies a display spec to prepend to each continuation
    line at display-time. See [Truncation](#Truncation).

`evaporate` [¶](#index-evaporate-_0028overlay-property_0029)
:   If this property is non-`nil`, the overlay is deleted automatically
    if it becomes empty (i.e., if its length becomes zero). If you give an
    empty overlay (see [empty overlays](#empty-overlays)) a non-`nil` `evaporate`
    property, that deletes it immediately. Note that, unless an overlay has
    this property, it will not be deleted when the text between its starting
    and ending positions is deleted from the buffer.

`display-line-numbers-disable` [¶](#index-display_002dline_002dnumbers_002ddisable-_0028overlay-property_0029)
:   This property prevents display of line numbers (see [display-line-numbers](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Display-Custom) in The GNU Emacs Manual) for the text which
    is within an overlay having this property. One situation where using an
    overlay with this property is useful is an empty overlay at
    end-of-buffer, since otherwise there’s no way of preventing the display
    of the line number there.

`keymap` [¶](#index-keymap-of-character-_0028and-overlays_0029)
:   If this property is non-`nil`, it specifies a keymap for a
    portion of the text. This keymap takes precedence over most other
    keymaps (see [Active Keymaps](#Active-Keymaps)), and it is used when point is within
    the overlay, where the front-
    and rear-advance properties define whether the boundaries are
    considered as being *within* or not.

`local-map` [¶](#index-local_002dmap-_0028overlay-property_0029)
:   The `local-map` property is similar to `keymap` but replaces the
    buffer’s local map rather than augmenting existing keymaps. This also means it
    has lower precedence than minor mode keymaps.

The `keymap` and `local-map` properties do not affect a
string displayed by the `before-string`, `after-string`, or
`display` properties. This is only relevant for mouse clicks and
other mouse events that fall on the string, since point is never on
the string. To bind special mouse events for the string, assign it a
`keymap` or `local-map` text property. See [Properties with Special Meanings](#Special-Properties).

---

Previous: [Overlay Properties](#Overlay-Properties), Up: [Overlays](#Overlays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.9.3 Searching for Overlays [¶](#Searching-for-Overlays)

Function: **overlays-at** pos &optional sorted [¶](#index-overlays_002dat)
:   This function returns a list of all the overlays that contain the character at
    position pos in the current buffer. If sorted is non-`nil`,
    the returned list is in decreasing order of priority, otherwise it is in no
    particular order. An overlay contains the character at pos if it begins
    at or before pos, and ends after pos.

    To illustrate usage, here is a Lisp function that returns a list of the
    overlays that specify property prop for the character at point:

    ```
    (defun find-overlays-specifying (prop)
      (let ((overlays (overlays-at (point)))
            found)
        (while overlays
          (let ((overlay (car overlays)))
            (if (overlay-get overlay prop)
                (setq found (cons overlay found))))
          (setq overlays (cdr overlays)))
        found))
    ```

    Empty overlays (see [empty overlays](#empty-overlays)) do not contain any characters,
    so this function does not return them. Use `overlays-in`,
    described below, instead if empty overlays are of interest.

    Note that this function can return overlays outside of the current
    narrowing of the buffer (see [Narrowing](#Narrowing)) if pos is outside of
    the narrowing.

Function: **overlays-in** beg end [¶](#index-overlays_002din)
:   This function returns a list of the overlays that overlap with the region
    beg through end. An overlay overlaps with a region if it
    contains one or more characters between beg and end; this
    excludes the characters at (i.e. after) end.

    Empty overlays do not contain any characters, so the rule for including
    them is different: they are said to overlap if they are at beg,
    strictly between beg and end excluding end, or at
    end when end denotes the position at the end of the buffer.
    (The special handling of empty overlays at end of buffer is to allow
    such overlays to be found and processed. For any other value of
    end you could increment the end position instead.) Note
    that if beg and end are the same position, an empty overlay
    that is at beg is also at end, so this case is somewhat
    similar to an empty overlay at the end of the buffer.

    The order in which the overlays appear in the returned list is
    unpredictable.

    Note that this function can return overlays outside of the current
    narrowing of the buffer if beg and/or end are outside of
    the narrowing.

Function: **next-overlay-change** pos [¶](#index-next_002doverlay_002dchange)
:   This function returns the buffer position of the next beginning or end
    of an overlay, after pos. If there is none, it returns
    `(point-max)`.

Function: **previous-overlay-change** pos [¶](#index-previous_002doverlay_002dchange)
:   This function returns the buffer position of the previous beginning or
    end of an overlay, before pos. If there is none, it returns
    `(point-min)`.

As an example, here’s a simplified (and inefficient) version of the
primitive function `next-single-char-property-change`
(see [Text Property Search Functions](#Property-Search)). It searches forward from position
pos for the next position where the value of a given property
`prop`, as obtained from either overlays or text properties,
changes.

```
(defun next-single-char-property-change (position prop)
  (save-excursion
    (goto-char position)
    (let ((propval (get-char-property (point) prop)))
      (while (and (not (eobp))
                  (eq (get-char-property (point) prop) propval))
        (goto-char (min (next-overlay-change (point))
                        (next-single-property-change (point) prop)))))
    (point)))
```

---

Next: [Line Height](#Line-Height), Previous: [Overlays](#Overlays), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.10 Size of Displayed Text [¶](#Size-of-Displayed-Text-1)

Since not all characters have the same width, these functions let you
check the width of a character. See [Indentation Primitives](#Primitive-Indent), and
[Motion by Screen Lines](#Screen-Lines), for related functions.

Function: **char-width** char [¶](#index-char_002dwidth)
:   This function returns the width in columns of the character
    char, if it were displayed in the current buffer (i.e., taking
    into account the buffer’s display table, if any; see [Display Tables](#Display-Tables)). The width of a tab character is usually `tab-width`
    (see [Usual Display Conventions](#Usual-Display)).

Function: **char-uppercase-p** char [¶](#index-char_002duppercase_002dp)
:   Return non-`nil` if char is an upper-case character
    according to Unicode. Note that a character is considered to be
    upper-case if the Unicode Standard defines a corresponding lower-case
    character.

Function: **string-width** string &optional from to [¶](#index-string_002dwidth)
:   This function returns the width in columns of the string string,
    if it were displayed in the current buffer and the selected window.
    Optional arguments from and to specify the substring of
    string to consider, and are interpreted as in `substring`
    (see [Creating Strings](#Creating-Strings)).

    The return value is an approximation: it only considers the values
    returned by `char-width` for the constituent characters, always
    takes a tab character as taking `tab-width` columns, ignores
    display properties and fonts, etc. For these reasons, we recommend
    using `window-text-pixel-size` or `string-pixel-width`,
    described below, instead.

Function: **truncate-string-to-width** string width &optional start-column padding ellipsis ellipsis-text-property [¶](#index-truncate_002dstring_002dto_002dwidth)
:   This function returns a new string that is a truncation of string
    which fits within width columns on display.

    If string is narrower than width, the result is equal to
    string; otherwise excess characters are omitted from the result.
    If a multi-column character in string exceeds the goal
    width, that character is omitted from the result. Thus, the
    result can sometimes fall short of width, but cannot go beyond
    it.

    The optional argument start-column specifies the starting
    column; it defaults to zero. If this is non-`nil`, then the
    first start-column columns of the string are omitted from the
    result. If one multi-column character in string extends across
    the column start-column, that character is omitted.

    The optional argument padding, if non-`nil`, is a padding
    character added at the beginning and end of the result string, to
    extend it to exactly width columns. The padding character is
    appended at the end of the result if it falls short of width, as
    many times as needed to reach width. It is also prepended at
    the beginning of the result if a multi-column character in
    string extends across the column start-column.

    If ellipsis is non-`nil`, it should be a string which will
    replace the end of string when it is truncated. In this case,
    more characters will be removed from string to free enough space
    for ellipsis to fit within width columns. However, if
    the display width of string is less than the display width of
    ellipsis, ellipsis will not be appended to the result. If
    ellipsis is non-`nil` and not a string, it stands for the
    value returned by the function `truncate-string-ellipsis`,
    described below.

    The optional argument ellipsis-text-property, if non-`nil`,
    means hide the excess parts of string with a `display` text
    property (see [The `display` Property](#Display-Property)) showing the ellipsis, instead of
    actually truncating the string.

    See also the function `truncate-string-pixelwise` for pixel-level
    resolution.

    ```
    (truncate-string-to-width "\tab\t" 12 4)
         ⇒ "ab"
    (truncate-string-to-width "\tab\t" 12 4 ?\s)
         ⇒ "    ab  "
    ```

    This function uses `string-width` and `char-width` to find
    the suitable truncation point when string is too wide, so it
    suffers from the same basic issues as `string-width` does. In
    particular, when character composition happens within string,
    the display width of a string could be smaller than the sum of widths
    of the constituent characters, and this function might return
    inaccurate results.

Function: **truncate-string-ellipsis** [¶](#index-truncate_002dstring_002dellipsis)
:   This function returns the string to be used as an ellipses in
    `truncate-string-to-width` and other similar contexts. The value
    is that of the variable `truncate-string-ellipsis`, if it’s
    non-`nil`, the string with the single character U+2026
    HORIZONTAL ELLIPSIS if that character can be displayed on the
    selected frame, and the string ‘`...`’ otherwise.

The following function returns the size in pixels of text as if it were
displayed in a given window. This function is used by
`fit-window-to-buffer` and `fit-frame-to-buffer`
(see [Resizing Windows](#Resizing-Windows)) to make a window exactly as large as the text
it contains.

Function: **window-text-pixel-size** &optional window from to x-limit y-limit mode-lines ignore-line-at-end [¶](#index-window_002dtext_002dpixel_002dsize)
:   This function returns the dimensions of the text of window’s buffer in
    pixels. window must be a live window and defaults to the
    selected one. The return value is a cons of the maximum pixel-width
    of any text line and the maximum pixel-height of all text lines. This
    function exists to allow Lisp programs to adjust the dimensions of
    window to the buffer text it needs to display, and for other
    similar situations.

    The return value can also optionally (see below) include the buffer
    position of the first line whose dimensions were measured.

    The optional argument from, if non-`nil`, specifies the
    first text position to consider, and defaults to the minimum
    accessible position of the buffer. If from is `t`, it
    stands for the minimum accessible position that is not a newline
    character. If from is a cons, its `car` specifies a buffer
    position, and its `cdr` specifies the vertical offset in pixels
    from that position to the first screen line whose text is to be
    measured. (The measurement will start from the visual beginning of
    that screen line.) In that case, the return value will instead be a
    list of the pixel-width, pixel-height, and the buffer position of the
    first line that was measured. The optional argument to, if
    non-`nil`, specifies the last text position to consider, and
    defaults to the maximum accessible position of the buffer. If
    to is `t`, it stands for the maximum accessible position
    that is not a newline character.

    The optional argument x-limit, if non-`nil`, specifies the
    maximum X coordinate beyond which text should be ignored; it is
    therefore also the largest value of pixel-width that the function can
    return. If x-limit `nil` or omitted, it means to use the
    pixel-width of window’s body (see [Window Sizes](#Window-Sizes)); this
    default means that text of truncated lines wider than the window will
    be ignored. This default is useful when the caller does not intend to
    change the width of window. Otherwise, the caller should
    specify here the maximum width window’s body may assume; in
    particular, if truncated lines are expected and their text needs to be
    accounted for, x-limit should be set to a large value. Since
    calculating the width of long lines can take some time, it’s always a
    good idea to make this argument as small as needed; in particular, if
    the buffer might contain long lines that will be truncated anyway.

    The optional argument y-limit, if non-`nil`, specifies the
    maximum Y coordinate beyond which text is to be ignored; it is
    therefore also the maximum pixel-height that the function can return.
    If y-limit is `nil` or omitted, it means to consider all the
    lines of text till the buffer position specified by to. Since
    calculating the pixel-height of a large buffer can take some time, it
    makes sense to specify this argument; in particular, if the caller
    does not know the size of the buffer.

    The optional argument mode-lines `nil` or omitted means to
    not include the height of the mode-, tab- or header-line of window
    in the return value. If it is either the symbol `mode-line`,
    `tab-line` or `header-line`, include only the height of that
    line, if present, in the return value. If it is `t`, include the
    height of all of these lines, if present, in the return value.

The optional argument ignore-line-at-end controls whether or
not to count the height of text in to’s screen line as part of
the returned pixel-height. This is useful if your Lisp program is
only interested in the dimensions of text up to and excluding the
visual beginning of to’s screen line.

`window-text-pixel-size` treats the text displayed in a window as a
whole and does not care about the size of individual lines. The
following function does.

Function: **window-lines-pixel-dimensions** &optional window first last body inverse left [¶](#index-window_002dlines_002dpixel_002ddimensions)
:   This function calculates the pixel dimensions of each line displayed in
    the specified window. It does so by walking window’s
    current glyph matrix—a matrix storing the glyph (see [Glyphs](#Glyphs)) of
    each buffer character currently displayed in window. If
    successful, it returns a list of cons pairs representing the x- and
    y-coordinates of the lower right corner of the last character of each
    line. Coordinates are measured in pixels from an origin (0, 0) at the
    top-left corner of window. window must be a live window and
    defaults to the selected one.

    If the optional argument first is an integer, it denotes the index
    (starting with 0) of the first line of window’s glyph matrix to be
    returned. Note that if window has a header line, the line with
    index 0 is that header line. If first is `nil`, the first line to
    be considered is determined by the value of the optional argument
    body: If body is non-`nil`, this means to start with
    the first line of window’s body, skipping any header line, if
    present. Otherwise, this function will start with the first line of
    window’s glyph matrix, possibly the header line.

    If the optional argument last is an integer, it denotes the index
    of the last line of window’s glyph matrix that shall be returned.
    If last is `nil`, the last line to be considered is determined by
    the value of body: If body is non-`nil`, this means to
    use the last line of window’s body, omitting window’s mode
    line, if present. Otherwise, this means to use the last line of
    window which may be the mode line.

    The optional argument inverse, if `nil`, means that the
    y-pixel value returned for any line specifies the distance in pixels
    from the left edge (body edge if body is non-`nil`) of
    window to the right edge of the last glyph of that line.
    inverse non-`nil` means that the y-pixel value returned for
    any line specifies the distance in pixels from the right edge of the
    last glyph of that line to the right edge (body edge if body is
    non-`nil`) of window. This is useful for determining the
    amount of slack space at the end of each line.

    The optional argument left, if non-`nil` means to return the
    x- and y-coordinates of the lower left corner of the leftmost character
    on each line. This is the value that should be used for windows that
    mostly display text from right to left.

    If left is non-`nil` and inverse is `nil`, this
    means that the y-pixel value returned for any line specifies the
    distance in pixels from the left edge of the last (leftmost) glyph of
    that line to the right edge (body edge if body is non-`nil`)
    of window. If left and inverse are both
    non-`nil`, the y-pixel value returned for any line specifies the
    distance in pixels from the left edge (body edge if body is
    non-`nil`) of window to the left edge of the last (leftmost)
    glyph of that line.

    This function returns `nil` if the current glyph matrix of
    window is not up-to-date which usually happens when Emacs is busy,
    for example, when processing a command. The value should be retrievable
    though when this function is run from an idle timer with a delay of zero
    seconds.

Function: **buffer-text-pixel-size** &optional buffer-or-name window x-limit y-limit [¶](#index-buffer_002dtext_002dpixel_002dsize)
:   This is much like `window-text-pixel-size`, but can be used when
    the buffer isn’t shown in a window. (`window-text-pixel-size` is
    faster when it is, so this function shouldn’t be used in that case.)

    buffer-or-name must specify a live buffer or the name of a live
    buffer and defaults to the current buffer. window must be a
    live window and defaults to the selected one; the function will
    compute the text dimensions as if buffer is displayed in
    window. The return value is a cons of the maximum pixel-width
    of any text line and the pixel-height of all the text lines of the
    accessible portion of the buffer specified by buffer-or-name.

    The optional arguments x-limit and y-limit have the same
    meaning as with `window-text-pixel-size`.

    If you want to measure dimensions of some part of the buffer text,
    narrow the buffer to that part before calling this function
    (see [Narrowing](#Narrowing)).

Function: **string-pixel-width** string &optional buffer [¶](#index-string_002dpixel_002dwidth)
:   This is a convenience function that uses `window-text-pixel-size`
    to compute the width of string (in pixels). Caveat: if you call
    this function to measure the width of a string with embedded newlines,
    it will then return the width of the widest substring that does not
    include newlines. The meaning of this result is the widest line taken
    by the string if inserted into a buffer. If buffer is
    non-`nil`, use any face remappings (see [Face Remapping](#Face-Remapping)) from
    that buffer when computing the width of string.

Function: **truncate-string-pixelwise** string max-pixels &optional buffer ellipsis ellipsis-pixels [¶](#index-truncate_002dstring_002dpixelwise)
:   This is a convenience function that uses `window-text-pixel-size`
    to truncate string to max-pixels pixels. Caveat: if you
    call this function to measure the width of a string with embedded
    newlines, it will then return the width of the widest substring that
    does not include newlines. The meaning of this result is the widest
    line taken by the string if inserted into a buffer. If buffer is
    non-`nil`, use any face remappings (see [Face Remapping](#Face-Remapping)) from
    that buffer when computing the width of string.

    If ellipsis is non-`nil`, it should be a string which will
    replace the end of string when it is truncated. In this case,
    more characters will be removed from string to free enough space
    for ellipsis to fit within max-pixels pixels. However, if
    the pixel width of string is less than the pixel width of
    ellipsis, ellipsis will not be appended to the result. If
    ellipsis is non-`nil` and not a string, it stands for the
    value returned by the function `truncate-string-ellipsis`,
    described above.

    If ellipsis-pixels is non-`nil` and ellipsis is
    non-`nil`, it should be the number of pixels of ellipsis that
    you should precompute using `string-pixel-width`, specifying the
    same buffer. This is useful to avoid the cost of recomputing this value
    repeatedly when you have many strings to truncate using the same
    ellipsis string.

    See also the function `truncate-string-to-width` for
    character-level resolution.

Function: **line-pixel-height** [¶](#index-line_002dpixel_002dheight)
:   This function returns the height in pixels of the line at point in the
    selected window. The value includes the line spacing of the line
    (see [Line Height](#Line-Height)).

Function: **string-glyph-split** string [¶](#index-string_002dglyph_002dsplit)
:   When character compositions are in effect, sequence of characters can
    be composed for display to form *grapheme clusters*, for example
    to display accented characters, or ligatures, or Emoji, or when
    complex text shaping requires that for some scripts. When that
    happens, characters no longer map in a simple way to display columns,
    and display layout decisions with such strings, such as truncating too
    wide strings, can be a complex job. This function helps in performing
    such jobs: it splits up its argument string into a list of
    substrings, where each substring produces a single grapheme cluster
    that should be displayed as a unit. Lisp programs can then use this
    list to construct visually-valid substrings of string which will
    look correctly on display, or compute the width of any substring of
    string by adding the width of its constituents in the returned
    list, etc.

    For instance, if you want to display a string without the first glyph,
    you can say:

    ```
    (apply #'insert (cdr (string-glyph-split string))))
    ```

    Caveat: for this function to recognize and process character
    compositions, `auto-composition-mode` must be enabled, and the
    current buffer must be displayed in some window.

When a buffer is displayed with line numbers (see [Display Custom](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Display-Custom) in The GNU Emacs Manual), it is sometimes useful to know the
width taken for displaying the line numbers. The following function
is for Lisp programs which need this information for layout
calculations.

Function: **line-number-display-width** &optional pixelwise [¶](#index-line_002dnumber_002ddisplay_002dwidth)
:   This function returns the width used for displaying the line numbers
    in the selected window. If the optional argument pixelwise is
    the symbol `columns`, the return value is a float number of the
    frame’s canonical columns; if pixelwise is `t` or any other
    non-`nil` value, the value is an integer and is measured in
    pixels. If pixelwise is omitted or `nil`, the value is the
    integer number of columns of the font defined for the
    `line-number` face, and doesn’t include the 2 columns used to pad
    the numbers on display. If line numbers are not displayed in the
    selected window, the value is zero regardless of the value of
    pixelwise. Use `with-selected-window` (see [Selecting Windows](#Selecting-Windows)) if you need this information about another window.

---

Next: [Faces](#Faces), Previous: [Size of Displayed Text](#Size-of-Displayed-Text), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.11 Line Height [¶](#Line-Height-1)

The total height of each display line consists of the height of the
contents of the line, plus optional additional vertical line spacing
above or below the display line.

The height of the line contents is the maximum height of any character
or image on that display line, including the final newline if there is
one. (A display line that is continued doesn’t include a final
newline.) That is the default line height, if you do nothing to specify
a greater height. (In the most common case, this equals the height of
the corresponding frame’s default font, see [Frame Font](#Frame-Font).)

There are several ways to explicitly specify a larger line height,
either by specifying an absolute height for the display line, or by
specifying vertical space. However, no matter what you specify, the
actual line height can never be less than the default.

A newline can have a `line-height` text or overlay property
that controls the total height of the display line ending in that
newline. The property value can be one of several forms:

`t`
:   If the property value is `t`, the newline character has no
    effect on the displayed height of the line—the visible contents
    alone determine the height. The `line-spacing` property of the
    newline, described below, is also ignored in this case. This is
    useful for tiling small images (or image slices) without adding blank
    areas between the images.

`(height total)`
:   If the property value is a list of the form shown, that adds extra
    space *below* the display line. First Emacs uses height as
    a height spec to control extra space *above* the line; then it
    adds enough space *below* the line to bring the total line height
    up to total. In this case, any value of `line-spacing`
    property for the newline is ignored.

Any other kind of property value is a height spec, which translates
into a number—the specified line height. There are several ways to
write a height spec; here’s how each of them translates into a number:

`integer`
:   If the height spec is a positive integer, the height value is that integer.

`float`
:   If the height spec is a float, float, the numeric height value
    is float times the frame’s default line height.

`(face . ratio)`
:   If the height spec is a cons of the format shown, the numeric height
    is ratio times the height of face face. ratio can
    be any type of number, or `nil` which means a ratio of 1.
    If face is `t`, it refers to the current face.

`(nil . ratio)`
:   If the height spec is a cons of the format shown, the numeric height
    is ratio times the height of the contents of the line.

Thus, any valid height spec determines the height in pixels, one way
or another. If the line contents’ height is less than that, Emacs
adds extra vertical space above the line to achieve the specified
total height.

If you don’t specify the `line-height` property, the line’s
height consists of the contents’ height plus the line spacing.
There are several ways to specify the line spacing for different
parts of Emacs text.

On graphical terminals, you can specify the line spacing for all
lines in a frame, using the `line-spacing` frame parameter
(see [Layout Parameters](#Layout-Parameters)). However, if the default value of
`line-spacing` is non-`nil`, it overrides the
frame’s `line-spacing` parameter. An integer specifies the
number of pixels put below lines. A floating-point number specifies
the spacing relative to the frame’s default line height.

You can specify the line spacing for all lines in a buffer via the
buffer-local `line-spacing` variable. An integer specifies the
number of pixels put below lines. A floating-point number specifies the
spacing relative to the default frame line height. A cons cell of
integers or floating-point numbers specifies the spacing put above and
below the line, allowing for vertically centering text. This overrides
line spacings specified for the frame.

Finally, a newline can have a `line-spacing` text or overlay
property that can enlarge the default frame line spacing and the
buffer local `line-spacing` variable: if its value is larger than
the buffer or frame defaults, that larger value is used instead, for
the display line ending in that newline (unless the newline also has
the `line-height` property whose value is one of the special
values which cause `line-spacing` to be ignored, see above).

One way or another, these mechanisms specify a Lisp value for the
spacing of each line. The value is a height spec, and it translates
into a Lisp value as described above. However, in this case the
numeric height value specifies the line spacing, rather than the line
height.

On text terminals, the line spacing cannot be altered.

---

Next: [Fringes](#Fringes), Previous: [Line Height](#Line-Height), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
