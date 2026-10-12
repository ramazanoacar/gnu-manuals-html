### 22.7 Input Events [¶](#Input-Events-1)

The Emacs command loop reads a sequence of *input events* that
represent keyboard or mouse activity, or system events sent to Emacs.
The events for keyboard activity are characters or symbols; other
events are always lists. This section describes the representation
and meaning of input events in detail.

Function: **eventp** object [¶](#index-eventp)
:   This function returns non-`nil` if object is an input event
    or event type.

    Note that any non-`nil` symbol might be used as an event or an
    event type; `eventp` cannot distinguish whether a symbol is
    intended by Lisp code to be used as an event.

* [Keyboard Events](#Keyboard-Events)
* [Function Keys](#Function-Keys)
* [Mouse Events](#Mouse-Events)
* [Click Events](#Click-Events)
* [Drag Events](#Drag-Events)
* [Button-Down Events](#Button_002dDown-Events)
* [Repeat Events](#Repeat-Events)
* [Motion Events](#Motion-Events)
* [Touchscreen Events](#Touchscreen-Events)
* [Focus Events](#Focus-Events)
* [Xwidget events](#Xwidget-Events)
* [Miscellaneous System Events](#Misc-Events)
* [Event Examples](#Event-Examples)
* [Classifying Events](#Classifying-Events)
* [Accessing Mouse Events](#Accessing-Mouse)
* [Accessing Scroll Bar Events](#Accessing-Scroll)
* [Putting Keyboard Events in Strings](#Strings-of-Events)

---

Next: [Function Keys](#Function-Keys), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.1 Keyboard Events [¶](#Keyboard-Events-1)

There are two kinds of input you can get from the keyboard: ordinary
keys, and function keys. Ordinary keys correspond to (possibly
modified) characters; the events they generate are represented in Lisp
as characters. The event type of a *character event* is the
character itself (an integer), which might have some modifier bits
set; see [Classifying Events](#Classifying-Events).

An input character event consists of a *basic code* between 0 and
524287, plus any or all of these *modifier bits*:

meta
:   The
    2\*\*27
    bit in the character code indicates a character
    typed with the meta key held down.

control
:   The
    2\*\*26
    bit in the character code indicates a non-ASCII
    control character.

    ASCII control characters such as `C-a` have special basic
    codes of their own, so Emacs needs no special bit to indicate them.
    Thus, the code for `C-a` is just 1.

    But if you type a control combination not in ASCII, such as
    `%` with the control key, the numeric value you get is the code
    for `%` plus
    2\*\*26
    (assuming the terminal supports non-ASCII
    control characters), i.e. with the 27th bit set.

shift
:   The
    2\*\*25
    bit (the 26th bit) in the character event code indicates an
    ASCII control character typed with the shift key held down.

    For letters, the basic code itself indicates upper versus lower case;
    for digits and punctuation, the shift key selects an entirely different
    character with a different basic code. In order to keep within the
    ASCII character set whenever possible, Emacs avoids using the
    2\*\*25
    bit for those character events.

    However, ASCII provides no way to distinguish `C-A` from
    `C-a`, so Emacs uses the
    2\*\*25
    bit in `C-A` and not in
    `C-a`.

hyper
:   The
    2\*\*24
    bit in the character event code indicates a character
    typed with the hyper key held down.

super
:   The
    2\*\*23
    bit in the character event code indicates a character
    typed with the super key held down.

alt
:   The
    2\*\*22
    bit in the character event code indicates a character typed with the
    alt key held down. (The key labeled `Alt` on most keyboards is
    actually treated as the meta key, not this.)

It is best to avoid mentioning specific bit numbers in your program.
To test the modifier bits of a character, use the function
`event-modifiers` (see [Classifying Events](#Classifying-Events)). When making key
bindings with `keymap-set`, you specify these events using
strings like ‘`C-H-x`’ instead (for “control hyper x”)
(see [Changing Key Bindings](#Changing-Key-Bindings)).

---

Next: [Mouse Events](#Mouse-Events), Previous: [Keyboard Events](#Keyboard-Events), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.2 Function Keys [¶](#Function-Keys-1)

Most keyboards also have *function keys*—keys that have names or
symbols that are not characters. Function keys are represented in
Emacs Lisp as symbols; the symbol’s name is the function key’s label,
in lower case. For example, pressing a key labeled `F1` generates
an input event represented by the symbol `f1`.

The event type of a function key event is the event symbol itself.
See [Classifying Events](#Classifying-Events).

Here are a few special cases in the symbol-naming convention for
function keys:

`backspace`, `tab`, `newline`, `return`, `delete`
:   These keys correspond to common ASCII control characters that have
    special keys on most keyboards.

    In ASCII, `C-i` and `TAB` are the same character. If the
    terminal can distinguish between them, Emacs conveys the distinction to
    Lisp programs by representing the former as the integer 9, and the
    latter as the symbol `tab`.

    Most of the time, it’s not useful to distinguish the two. So normally
    `local-function-key-map` (see [Keymaps for Translating Sequences of Events](#Translation-Keymaps)) is set up
    to map `tab` into 9. Thus, a key binding for character code 9
    (the character `C-i`) also applies to `tab`. Likewise for
    the other symbols in this group. The function `read-char`
    likewise converts these events into characters.

    In ASCII, `BS` is really `C-h`. But `backspace`
    converts into the character code 127 (`DEL`), not into code 8
    (`BS`). This is what most users prefer.

`left`, `up`, `right`, `down`
:   Cursor arrow keys

`kp-add`, `kp-decimal`, `kp-divide`, …
:   Keypad keys (to the right of the regular keyboard).

`kp-0`, `kp-1`, …
:   Keypad keys with digits.

`kp-f1`, `kp-f2`, `kp-f3`, `kp-f4`
:   Keypad PF keys.

`kp-home`, `kp-left`, `kp-up`, `kp-right`, `kp-down`
:   Keypad arrow keys. Emacs normally translates these into the
    corresponding non-keypad keys `home`, `left`, …

`kp-prior`, `kp-next`, `kp-end`, `kp-begin`, `kp-insert`, `kp-delete`
:   Additional keypad duplicates of keys ordinarily found elsewhere. Emacs
    normally translates these into the like-named non-keypad keys.

You can use the modifier keys `ALT`, `CTRL`, `HYPER`,
`META`, `SHIFT`, and `SUPER` with function keys. The way to
represent them is with prefixes in the symbol name:

‘`A-`’
:   The alt modifier.

‘`C-`’
:   The control modifier.

‘`H-`’
:   The hyper modifier.

‘`M-`’
:   The meta modifier.

‘`S-`’
:   The shift modifier.

‘`s-`’
:   The super modifier.

Thus, the symbol for the key `F3` with `META` held down is
`M-f3`. When you use more than one prefix, we recommend you
write them in alphabetical order; but the order does not matter in
arguments to the key-binding lookup and modification functions.

---

Next: [Click Events](#Click-Events), Previous: [Function Keys](#Function-Keys), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.3 Mouse Events [¶](#Mouse-Events-1)

Emacs supports four kinds of mouse events: click events, drag events,
button-down events, and motion events. All mouse events are represented
as lists. The CAR of the list is the event type; this says which
mouse button was involved, and which modifier keys were used with it.
The event type can also distinguish double or triple button presses
(see [Repeat Events](#Repeat-Events)). The rest of the list elements give position
and time information.

For key lookup, only the event type matters: two events of the same type
necessarily run the same command. The command can access the full
values of these events using the ‘`e`’ interactive code.
See [Code Characters for `interactive`](#Interactive-Codes).

A key sequence that starts with a mouse event is read using the keymaps
of the buffer in the window that the mouse was in, not the current
buffer. This does not imply that clicking in a window selects that
window or its buffer—that is entirely under the control of the command
binding of the key sequence.

---

Next: [Drag Events](#Drag-Events), Previous: [Mouse Events](#Mouse-Events), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.4 Click Events [¶](#Click-Events-1)

When the user presses a mouse button and releases it at the same
location, that generates a *click* event. Depending on how your
window-system reports mouse-wheel events, turning the mouse wheel can
generate either a mouse click or a mouse-wheel event. All mouse event
share the same format:

```
(event-type position click-count)
```

event-type
:   This is a symbol that indicates which mouse button was used. It is
    one of the symbols `mouse-1`, `mouse-2`, …, where the
    buttons are numbered left to right. For mouse-wheel event, it can be
    `wheel-up` or `wheel-down`.

    You can also use prefixes ‘`A-`’, ‘`C-`’, ‘`H-`’, ‘`M-`’,
    ‘`S-`’ and ‘`s-`’ for modifiers alt, control, hyper, meta, shift
    and super, just as you would with function keys.

    This symbol also serves as the event type of the event. Key bindings
    describe events by their types; thus, if there is a key binding for
    `mouse-1`, that binding would apply to all events whose
    event-type is `mouse-1`.

position [¶](#index-mouse-position-list)
:   This is a *mouse position list* specifying where the mouse event
    occurred; see below for details.

click-count
:   This is the number of rapid repeated presses so far of the same mouse
    button or the number of repeated turns of the wheel. See [Repeat Events](#Repeat-Events).

To access the contents of a mouse position list in the
position slot of a mouse event, you should typically use the
functions documented in [Accessing Mouse Events](#Accessing-Mouse).

The explicit format of the list depends on where the event occurred.
For clicks in the text area, mode line, header line, tab line, or in
the fringe or marginal areas, the mouse position list has the form

```
(window pos-or-area (x . y) timestamp
 object text-pos (col . row)
 image (dx . dy) (width . height))
```

The meanings of these list elements are as follows:

window
:   The window in which the mouse event occurred.

pos-or-area
:   The buffer position of the character clicked on in the text area; or,
    if the event was outside the text area, the window area where it
    occurred. It is one of the symbols `mode-line`,
    `header-line`, `tab-line`, `vertical-line`,
    `left-margin`, `right-margin`, `left-fringe`,
    or `right-fringe`.

    In one special case, pos-or-area is a list containing a symbol
    (one of the symbols listed above) instead of just the symbol. This
    happens after the imaginary prefix keys for the event are registered
    by Emacs. See [Key Sequence Input](#Key-Sequence-Input).

x, y
:   The relative pixel coordinates of the event. For events in the text
    area of a window, the coordinate origin `(0 . 0)` is taken to be
    the top left corner of the text area. See [Window Sizes](#Window-Sizes). For
    events in a mode line, header line or tab line, the coordinate origin
    is the top left corner of the window itself. For fringes, margins,
    and the vertical border, x does not have meaningful data.
    For fringes and margins, y is relative to the bottom edge of the
    header line. In all cases, the x and y coordinates
    increase rightward and downward respectively.

timestamp
:   The time at which the event occurred, as an integer number of
    milliseconds since a system-dependent initial time.

object
:   Either `nil`, which means the event occurred on buffer text, or a
    cons cell of the form (string . string-pos) if there
    is a string from a text property or an overlay at the event position.

    string
    :   The string which was clicked on, including any properties.

    string-pos
    :   The position in the string where the click occurred.

text-pos
:   For clicks on a marginal area or on a fringe, this is the buffer
    position of the first visible character in the corresponding line in
    the window. For clicks on the mode line, the header line or the tab
    line, this is `nil`. For other events, it is the buffer position
    closest to the click.

col, row
:   These are the actual column and row coordinate numbers of the glyph
    under the x, y position. If x lies beyond the last
    column of actual text on its line, col is reported by adding
    fictional extra columns that have the default character width.
    Row 0 is taken to be the header line if the window has one, or Row 1
    if the window also has the tab line, or the topmost row of
    the text area otherwise. Column 0 is taken to be the leftmost
    column of the text area for clicks on a window text area, or the
    leftmost mode line or header line column for clicks there. For clicks
    on fringes or vertical borders, these have no meaningful data. For
    clicks on margins, col is measured from the left edge of the
    margin area and row is measured from the top of the margin area.

image
:   If there is an image at the click location, this is the image object
    as returned by `find-image` (see [Defining Images](#Defining-Images)); otherwise
    this is `nil`.

dx, dy
:   These are the pixel offsets of the click relative to the top left
    corner of the object’s glyph that is the nearest one to the
    click. The relevant objects can be either a buffer, or a string,
    or an image, see above. If object is `nil` or a string,
    the coordinates are relative to the top left corner of the character
    glyph clicked on. Note that the offsets are always zero on text-mode
    frames, when object is `nil`, since each glyph there is
    considered to have exactly 1x1 pixel dimensions.

width, height
:   If the click is on a character, either from buffer text or from
    overlay or display string, these are the pixel width and height of
    that character’s glyph; otherwise they are dimensions of object
    clicked on.

For clicks on a scroll bar, position has this form:

```
(window area (portion . whole) timestamp part)
```

window
:   The window whose scroll bar was clicked on.

area
:   This is the symbol `vertical-scroll-bar`.

portion
:   The number of pixels from the top of the scroll bar to the click
    position. On some toolkits, including GTK+, Emacs cannot extract this
    data, so the value is always `0`.

whole
:   The total length, in pixels, of the scroll bar. On some toolkits,
    including GTK+, Emacs cannot extract this data, so the value is always
    `0`.

timestamp
:   The time at which the event occurred, in milliseconds. On some
    toolkits, including GTK+, Emacs cannot extract this data, so the value
    is always `0`.

part
:   The part of the scroll bar on which the click occurred. It is one of
    the symbols `handle` (the scroll bar handle), `above-handle`
    (the area above the handle), `below-handle` (the area below the
    handle), `up` (the up arrow at one end of the scroll bar), or
    `down` (the down arrow at one end of the scroll bar).

For clicks on the frame’s internal border (see [Frame Layout](#Frame-Layout)), the
frame’s tool bar (see [Tool bars](#Tool-Bar)) or tab bar or menu bar,
position has this form:

```
 (frame part (X . Y) timestamp object)
```

frame
:   The frame whose internal border or tool bar or tab bar or menu bar was
    clicked on.

part
:   The part of the frame which was clicked on. This can be one
    of the following:

    `tool-bar` [¶](#index-tool_002dbar-mouse-events)
    :   The frame has a tool bar, and the event was in the tool-bar area.

    `tab-bar` [¶](#index-tab_002dbar-mouse-events)
    :   The frame has a tab bar, and the event was in the tab-bar area.

    `menu-bar` [¶](#index-menu_002dbar-mouse-events)
    :   The event was on the frame’s menu bar area. This kind of click event
        can happen only on text-only frames or on X frames in a non-toolkit
        build of Emacs. (In toolkit builds of Emacs, menu-bar clicks are
        handled by the toolkit, and are not visible to Emacs as click events.)

    `left-edge`

    `top-edge`

    `right-edge`

    `bottom-edge`
    :   The click was on the corresponding border at an offset of at least one
        canonical character from the border’s nearest corner.

    `top-left-corner`

    `top-right-corner`

    `bottom-right-corner`

    `bottom-left-corner`
    :   The click was on the corresponding corner of the internal border.

    `nil`
    :   The frame does not have an internal border, and the event was not on
        the tab bar or the tool bar. This usually happens on text-mode
        frames. This can also happen on GUI frames with internal border if
        the frame doesn’t have its `drag-internal-border` parameter
        (see [Mouse Dragging Parameters](#Mouse-Dragging-Parameters)) set to a non-`nil` value.

object
:   This member is present only for clicks on the tab bar, and it is the
    propertized string with information about the clicked button.

---

Next: [Button-Down Events](#Button_002dDown-Events), Previous: [Click Events](#Click-Events), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.5 Drag Events [¶](#Drag-Events-1)

With Emacs, you can have a drag event without even changing your
clothes. A *drag event* happens every time the user presses a mouse
button and then moves the mouse to a different character position before
releasing the button. Like all mouse events, drag events are
represented in Lisp as lists. The lists record both the starting mouse
position and the final position, like this:

```
(event-type start-position end-position)
```

For a drag event, the name of the symbol event-type contains the
prefix ‘`drag-`’. For example, dragging the mouse with button 2
held down generates a `drag-mouse-2` event. The second and third
elements of the event, start-position and end-position in
the foregoing illustration, are set to the start and end positions of
the drag as mouse position lists (see [Click Events](#Click-Events)). You can
access the second element of any mouse event in the same way.
However, the drag event may end outside the boundaries of the frame
that was initially selected. In that case, the third element’s
position list contains that frame in place of a window.

The ‘`drag-`’ prefix follows the modifier key prefixes such as
‘`C-`’ and ‘`M-`’.

If `read-key-sequence` receives a drag event that has no key
binding, and the corresponding click event does have a binding, it
changes the drag event into a click event at the drag’s starting
position. This means that you don’t have to distinguish between click
and drag events unless you want to.

---

Next: [Repeat Events](#Repeat-Events), Previous: [Drag Events](#Drag-Events), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.6 Button-Down Events [¶](#Button_002dDown-Events-1)

Click and drag events happen when the user releases a mouse button.
They cannot happen earlier, because there is no way to distinguish a
click from a drag until the button is released.

If you want to take action as soon as a button is pressed, you need to
handle *button-down* events.[20](#FOOT20) These occur as soon as a button is
pressed. They are represented by lists that look exactly like click
events (see [Click Events](#Click-Events)), except that the event-type symbol
name contains the prefix ‘`down-`’. The ‘`down-`’ prefix follows
modifier key prefixes such as ‘`C-`’ and ‘`M-`’.

The function `read-key-sequence` ignores any button-down events
that don’t have command bindings; therefore, the Emacs command loop
ignores them too. This means that you need not worry about defining
button-down events unless you want them to do something. The usual
reason to define a button-down event is so that you can track mouse
motion (by reading motion events) until the button is released.
See [Motion Events](#Motion-Events).

---

Next: [Motion Events](#Motion-Events), Previous: [Button-Down Events](#Button_002dDown-Events), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.7 Repeat Events [¶](#Repeat-Events-1)

If you press the same mouse button more than once in quick succession
without moving the mouse, Emacs generates special *repeat* mouse
events for the second and subsequent presses.

The most common repeat events are *double-click* events. Emacs
generates a double-click event when you click a button twice; the event
happens when you release the button (as is normal for all click
events).

The event type of a double-click event contains the prefix
‘`double-`’. Thus, a double click on the second mouse button with
`meta` held down comes to the Lisp program as
`M-double-mouse-2`. If a double-click event has no binding, the
binding of the corresponding ordinary click event is used to execute
it. Thus, you need not pay attention to the double click feature
unless you really want to.

When the user performs a double click, Emacs generates first an ordinary
click event, and then a double-click event. Therefore, you must design
the command binding of the double click event to assume that the
single-click command has already run. It must produce the desired
results of a double click, starting from the results of a single click.

This is convenient, if the meaning of a double click somehow builds
on the meaning of a single click—which is recommended user interface
design practice for double clicks.

If you click a button, then press it down again and start moving the
mouse with the button held down, then you get a *double-drag* event
when you ultimately release the button. Its event type contains
‘`double-drag`’ instead of just ‘`drag`’. If a double-drag event
has no binding, Emacs looks for an alternate binding as if the event
were an ordinary drag.

Before the double-click or double-drag event, Emacs generates a
*double-down* event when the user presses the button down for the
second time. Its event type contains ‘`double-down`’ instead of just
‘`down`’. If a double-down event has no binding, Emacs looks for an
alternate binding as if the event were an ordinary button-down event.
If it finds no binding that way either, the double-down event is
ignored.

To summarize, when you click a button and then press it again right
away, Emacs generates a down event and a click event for the first
click, a double-down event when you press the button again, and finally
either a double-click or a double-drag event.

If you click a button twice and then press it again, all in quick
succession, Emacs generates a *triple-down* event, followed by
either a *triple-click* or a *triple-drag*. The event types of
these events contain ‘`triple`’ instead of ‘`double`’. If any
triple event has no binding, Emacs uses the binding that it would use
for the corresponding double event.

If you click a button three or more times and then press it again, the
events for the presses beyond the third are all triple events. Emacs
does not have separate event types for quadruple, quintuple, etc.
events. However, you can look at the event list to find out precisely
how many times the button was pressed.

Function: **event-click-count** event [¶](#index-event_002dclick_002dcount)
:   This function returns the number of consecutive button presses that led
    up to event. If event is a double-down, double-click or
    double-drag event, the value is 2. If event is a triple event,
    the value is 3 or greater. If event is an ordinary mouse event
    (not a repeat event), the value is 1.

User Option: **double-click-fuzz** [¶](#index-double_002dclick_002dfuzz)
:   To generate repeat events, successive mouse button presses must be at
    approximately the same screen position. The value of
    `double-click-fuzz` specifies the maximum number of pixels the
    mouse may be moved (horizontally or vertically) between two successive
    clicks to make a double-click.

    This variable is also the threshold for motion of the mouse to count
    as a drag.

User Option: **double-click-time** [¶](#index-double_002dclick_002dtime)
:   To generate repeat events, the number of milliseconds between
    successive button presses must be less than the value of
    `double-click-time`. Setting `double-click-time` to
    `nil` disables multi-click detection entirely. Setting it to
    `t` removes the time limit; Emacs then detects multi-clicks by
    position only.

---

Next: [Touchscreen Events](#Touchscreen-Events), Previous: [Repeat Events](#Repeat-Events), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.8 Motion Events [¶](#Motion-Events-1)

Emacs sometimes generates *mouse motion* events to describe motion
of the mouse without any button activity. Mouse motion events are
represented by lists that look like this:

```
(mouse-movement POSITION)
```

position is a mouse position list (see [Click Events](#Click-Events)),
specifying the current position of the mouse cursor. As with the
end-position of a drag event, this position list may represent a
location outside the boundaries of the initially selected frame, in
which case the list contains that frame in place of a window.

The `track-mouse` macro enables generation of motion
events within its body. Outside of `track-mouse` body, Emacs
does not generate events for mere motion of the mouse, and these
events do not appear. See [Mouse Tracking](#Mouse-Tracking).

Variable: **mouse-fine-grained-tracking** [¶](#index-mouse_002dfine_002dgrained_002dtracking)
:   When non-`nil`, mouse motion events are generated even for very
    small movements. Otherwise, motion events are not generated as long
    as the mouse cursor remains pointing to the same glyph in the text.

---

Next: [Focus Events](#Focus-Events), Previous: [Motion Events](#Motion-Events), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.9 Touchscreen Events [¶](#Touchscreen-Events-1)

Some window systems provide support for input devices that react to
the user’s touching the screen and moving fingers while touching the
screen. These input devices are known as touchscreens, and Emacs
reports the events they generate as *touchscreen events*.

Most individual events generated by a touchscreen only have meaning as
part of a larger sequence of other events: for instance, the simple
operation of tapping the touchscreen involves the user placing and
raising a finger on the touchscreen, and swiping the display to
scroll it involves placing a finger, moving it many times upwards or
downwards, and then raising the finger.

While a simplistic model consisting of one finger is adequate for taps
and scrolling, more complicated gestures require support for keeping
track of multiple fingers, where the position of each finger is
represented by a *touch point*. For example, a “pinch to zoom”
gesture might consist of the user placing two fingers and moving them
individually in opposite directions, where the distance between the
positions of their individual points determine the amount by which to
zoom the display, and the center of an imaginary line between those
positions determines where to pan the display after zooming.

The low-level touchscreen events described below can be used to
implement all the touch sequences described above. In those events,
each point is represented by a cons of an arbitrary number identifying
the point and a mouse position list (see [Click Events](#Click-Events)) specifying
the position of the finger when the event occurred.

`(touchscreen-begin point)` [¶](#index-touchscreen_002dbegin-event)
:   This event is sent when point is created by the user pressing a
    finger against the touchscreen.

    Imaginary prefix keys are also affixed to these events
    `read-key-sequence` when they originate on top of a special part
    of a frame or window. See [Key Sequence Input](#Key-Sequence-Input).

`(touchscreen-update points)` [¶](#index-touchscreen_002dupdate-event)
:   This event is sent when a point on the touchscreen has changed
    position. points is a list of touch points containing the
    up-to-date positions of each touch point currently on the touchscreen.

`(touchscreen-end point canceled)` [¶](#index-touchscreen_002dend-event)
:   This event is sent when point is no longer present on the
    display, because another program took the grab, or because the user
    raised the finger from the touchscreen.

    canceled is non-`nil` if the touch sequence has been
    intercepted by another program (such as the window manager), and Emacs
    should undo or avoid any editing commands that would otherwise result
    from the touch sequence.

    Imaginary prefix keys are also affixed to these events
    `read-key-sequence` when they originate on top of a special part
    of a frame or window.

If a touchpoint is pressed against the menu bar, then Emacs will not
generate any corresponding `touchscreen-begin` or
`touchscreen-end` events; instead, the menu bar may be displayed
after `touchscreen-end` would have been delivered under other
circumstances.

When no command is bound to `touchscreen-begin`,
`touchscreen-end` or `touchscreen-update`, Emacs calls a
“key translation function” (see [Keymaps for Translating Sequences of Events](#Translation-Keymaps)) to
translate key sequences containing touch screen events into ordinary
mouse events (see [Mouse Events](#Mouse-Events).) Since Emacs doesn’t support
distinguishing events originating from separate mouse devices, it
assumes that a maximum of two touchpoints are active while translation
takes place, and does not place any guarantees on the results of event
translation when that restriction is overstepped.

Emacs applies two different strategies for translating touch events
into mouse events, contingent on factors such as the commands bound to
keymaps that are active at the location of the
`touchscreen-begin` event. If a command is bound to
`down-mouse-1` at that location, the initial translation consists
of a single `down-mouse-1` event, with subsequent
`touchscreen-update` events translated to mouse motion events
(see [Motion Events](#Motion-Events)), and a final `touchscreen-end` event
translated to a `mouse-1` or `drag-mouse-1` event (unless
the `touchscreen-end` event indicates that the touch sequence has
been intercepted by another program.) This is dubbed “simple
translation”, and produces a simple correspondence between touchpoint
motion and mouse motion.

However, some commands bound to
`down-mouse-1`–`mouse-drag-region`, for example–either
conflict with defined touch screen gestures (such as “long-press to
drag”), or with user expectations for touch input, and should not
subject the touch sequence to simple translation. If a command whose
name contains the property (see [Symbol Properties](#Symbol-Properties))
`ignored-mouse-command` is encountered or there is no command
bound to `down-mouse-1`, a more irregular form of translation
takes place: here, Emacs processes touch screen gestures
(see [Touchscreens](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Touchscreens) in The GNU Emacs Manual) first, and
finally attempts to translate touch screen events into mouse events if
no gesture was detected prior to a closing `touchscreen-end`
event (with its canceled parameter `nil`, as with simple
translation) and a command is bound to `mouse-1` at the location
of that event. Before generating the `mouse-1` event, point is
also set to the location of the `touchscreen-end` event, and the
window containing the position of that event is selected, as a
compromise for packages which assume `mouse-drag-region` has
already set point to the location of any mouse click and selected the
window where it took place.

To prevent unwanted `mouse-1` events from arriving after a mouse
menu is dismissed (see [Menus and the Mouse](#Mouse-Menus)), Emacs also disables simple
translation if `down-mouse-1` is bound to a keymap, making it a
prefix key. In lieu of simple translation, it translates the closing
`touchscreen-end` to a `down-mouse-1` event with the starting
position of the touch sequence, consequently displaying the mouse menu.

Simple conversion will be enabled without regard to the existence of
command or menu bindings if the variable
`touch-screen-simple-mouse-conversion` is bound or set to a
non-`nil` value, as, for example, it may be by a caller of
`read-key` expecting to receive `mouse-movement` and
`drag-mouse-1` events.

Since certain commands are also bound to `down-mouse-1` for the
purpose of displaying pop-up menus, Emacs additionally behaves as
illustrated in the last paragraph if `down-mouse-1` is bound to a
command whose name has the property `mouse-1-menu-command`.

When a second touch point is registered as a touch point is already
being translated, gesture translation is terminated, and the distance
from the second touch point (the *ancillary tool*) to the first is
measured. Subsequent motion from either of those touch points will
yield `touchscreen-pinch` events incorporating the ratio formed
by the distance between their new positions and the distance measured
at the outset, as illustrated in the following table.

If touch gestures are detected during translation, one of the
following input events may be generated:

`(touchscreen-scroll window dx dy)` [¶](#index-touchscreen_002dscroll-event)
:   If a “scrolling” gesture is detected during the translation process,
    each subsequent `touchscreen-update` event is translated to a
    `touchscreen-scroll` event, where dx and dy specify,
    in pixels, the relative motion of the touchpoint from the position of
    the `touchscreen-begin` event that started the sequence or the
    last `touchscreen-scroll` event, whichever came later.

`(touchscreen-hold posn)` [¶](#index-touchscreen_002dhold-event)
:   If the single active touchpoint remains stationary for more than
    `touch-screen-delay` seconds after a `touchscreen-begin` is
    generated, a “long-press” gesture is detected during the translation
    process, and a `touchscreen-hold` event is sent, with posn
    set to a mouse position list containing the position of the
    `touchscreen-begin` event.

`(touchscreen-drag posn)` [¶](#index-touchscreen_002ddrag-event)
:   If a “long-press” gesture is detected while translating the current
    touch sequence or “drag-to-select” is being resumed as a result of
    the `touch-screen-extend-selection` user option, a
    `touchscreen-drag` event is sent upon each subsequent
    `touchscreen-update` event with posn set to the new
    position of the touchpoint.

`(touchscreen-restart-drag posn)` [¶](#index-touchscreen_002drestart_002ddrag-event)
:   This event is sent upon the start of a touch sequence resulting in the
    continuation of a “drag-to-select” gesture (subject to the
    aforementioned user option) with posn set to the position list of
    the initial `touchscreen-begin` event within that touch sequence.

`(touchscreen-pinch posn ratio pan-x pan-y ratio-diff)` [¶](#index-touchscreen_002dpinch-event)
:   This event is delivered upon significant changes to the positions of
    either active touch point when an ancillary tool is active.

    posn is a mouse position list for the midpoint of a line drawn
    from the ancillary tool to the other touch point being observed.

    ratio is the distance between both touch points being observed
    divided by that distance when the ancillary point was first
    registered; which is to say, the scale of the “pinch” gesture.

    pan-x and pan-y are the difference between the pixel
    position of posn and this position within the last event
    delivered appertaining to this series of touch events, or in the case
    that no such event exists, the centerpoint between both touch points
    when the ancillary tool was first registered.

    ratio-diff is the difference between this event’s ratio and
    ratio in the last event delivered; it is ratio if no such
    event exists.

    Such events are sent when the magnitude of the changes they represent
    will yield a ratio which differs by more than `0.2` from
    that in the previous event, or the sum of pan-x and pan-y
    will surpass half the frame’s character width in pixels (see [Frame Font](#Frame-Font)).

Several functions are provided for Lisp programs that handle touch
screen events. The intended use of the first two functions described
below is from commands bound directly to `touchscreen-begin`
events; they allow responding to commonly used touch screen gestures
separately from mouse event translation.

Function: **touch-screen-track-tap** event &optional update data threshold [¶](#index-touch_002dscreen_002dtrack_002dtap)
:   This function is used to track a single “tap” gesture originating
    from the `touchscreen-begin` event event, often used to
    set the point or to activate a button. It waits for a
    `touchscreen-end` event with the same touch identifier to arrive,
    at which point it returns `t`, signifying the end of the gesture.

    If a `touchscreen-update` event arrives in the mean time and
    contains at least one touchpoint with the same identifier as in
    event, the function update is called with two arguments,
    the list of touchpoints in that `touchscreen-update` event, and
    data.

    If threshold is non-`nil` and such an event indicates that
    the touchpoint represented by event has moved beyond a threshold
    of either threshold or 10 pixels if it is not a number from the
    position of event, `nil` is returned and mouse event
    translation is resumed for that touchpoint, so as not to impede the
    recognition of any subsequent touchscreen gesture arising from its
    sequence.

    If any other event arrives in the mean time, `nil` is returned.
    The caller should not perform any action in that case.

Function: **touch-screen-track-drag** event update &optional data [¶](#index-touch_002dscreen_002dtrack_002ddrag)
:   This function is used to track a single “drag” gesture originating
    from the `touchscreen-begin` event `event`.

    It behaves like `touch-screen-track-tap`, except that it returns
    `no-drag` and refrains from calling update if the
    touchpoint in `event` did not move far enough (by default, 5
    pixels from its position in `event`) to qualify as an actual
    drag.

In addition to those two functions, a function is provided for
commands bound to some types of events generated through mouse event
translation to prevent unwanted events from being generated after it
is called.

Function: **touch-screen-inhibit-drag** [¶](#index-touch_002dscreen_002dinhibit_002ddrag)
:   This function inhibits the generation of `touchscreen-drag`
    events during mouse event translation for the duration of the touch
    sequence being translated after it is called. It must be called from
    a command which is bound to a `touchscreen-hold` or
    `touchscreen-drag` event, and signals an error otherwise.

    Since this function can only be called after a gesture is already
    recognized during mouse event translation, no mouse events will be
    generated from touch events constituting the previously mentioned
    touch sequence after it is called.

---

Next: [Xwidget events](#Xwidget-Events), Previous: [Touchscreen Events](#Touchscreen-Events), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.10 Focus Events [¶](#Focus-Events-1)

This section talks about both window systems and Emacs frames. When
talking about just “frames” or “windows”, it refers to Emacs frames
and Emacs windows. When talking about window system windows, which are
also Emacs frames, this section always says “window system window”.

Window systems provide general ways for the user to control which window
system window, or Emacs frame, gets keyboard input. This choice of
window system window is called the *focus*. When the user does
something to switch between Emacs frames, that generates a *focus
event*. Emacs also generates focus events when using
mouse-autoselect-window to switch between Emacs windows within
Emacs frames.

A focus event in the middle of a key sequence would garble the
sequence. So Emacs never generates a focus event in the middle of a key
sequence. If the user changes focus in the middle of a key
sequence—that is, after a prefix key—then Emacs reorders the events
so that the focus event comes either before or after the multi-event key
sequence, and not within it.

#### Focus events for frames [¶](#Focus-events-for-frames)

The normal definition of a focus event that switches frames, in the
global keymap, is to select that new frame within Emacs, as the user
would expect. See [Input Focus](#Input-Focus), which also describes hooks related to
focus events for frames. Focus events for frames are represented in
Lisp as lists that look like this:

```
(switch-frame new-frame)
```

where new-frame is the frame switched to.

Some X window managers are set up so that just moving the mouse into a
frame is enough to set the focus there. Usually, there is no need for a
Lisp program to know about the focus change until some other kind of
input arrives. Emacs generates a focus event only when the user
actually types a keyboard key or presses a mouse button in the new
frame; just moving the mouse between frames does not generate a focus
event.

#### Focus events for windows [¶](#Focus-events-for-windows)

When mouse-autoselect-window is set, moving the mouse over a new
window within a frame can also switch the selected window. See [Mouse Window Auto-selection](#Mouse-Window-Auto_002dselection), which describes the behavior for different
values. When the mouse is moved over a new window, a focus event for
switching windows is generated. Focus events for windows are
represented in Lisp as lists that look like this:

```
(select-window new-window)
```

where new-window is the window switched to.

---

Next: [Miscellaneous System Events](#Misc-Events), Previous: [Focus Events](#Focus-Events), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.11 Xwidget events [¶](#Xwidget-events)

Xwidgets (see [Embedded Native Widgets](#Xwidgets)) can send events to update Lisp programs on
their status. These events are dubbed `xwidget-events`, and
contain various data describing the nature of the change.

`(xwidget-event kind xwidget arg)` [¶](#index-xwidget_002devent-event)
:   This event is sent whenever some kind of update occurs in
    xwidget. There are several types of updates, identified by
    their kind.

    It is a special event (see [Special Events](#Special-Events)), which should be
    handled by adding a callback to an xwidget that is called whenever an
    xwidget event for xwidget is received.

    You can add a callback by setting the `callback` of an xwidget’s
    property list, which should be a function that accepts xwidget
    and kind as arguments.

    `load-changed` [¶](#index-load_002dchanged-xwidget-event)
    :   This xwidget event indicates that the xwidget has reached a
        particular point of the page-loading process. When these events are
        sent, arg will contain a string that further describes the status
        of the widget:

        ‘`load-started`’ [¶](#index-load_002dstarted-in-xwidgets)
        :   This means the widget has begun a page-loading operation.

        ‘`load-finished`’ [¶](#index-load_002dfinished-in-xwidgets)
        :   This means the xwidget has finished processing whatever
            page-loading operation that it was previously performing.

        ‘`load-redirected`’ [¶](#index-load_002dredirected-in-xwidgets)
        :   This means the xwidget has encountered and followed a redirect
            during the page-loading operation.

        ‘`load-committed`’ [¶](#index-load_002dcommitted-in-xwidgets)
        :   This means the xwidget has committed to a given URL during the
            page-loading operation, i.e. the URL is the final URL that will be
            rendered by xwidget during the current page-loading operation.

    `download-callback` [¶](#index-download_002dcallback-xwidget-events)
    :   This event indicates that a download of some kind has been completed.

    In the above events, there can be arguments after arg, which
    itself indicates the URL from which the download file was retrieved:
    the first argument after arg indicates the MIME type of the
    download, as a string, while the second argument contains the full
    file name of the downloaded file.

    `download-started` [¶](#index-download_002dstarted-xwidget-events)
    :   This event indicates that a download has been started. In these
        events, arg contains the URL of the file that is currently being
        downloaded.

    `javascript-callback` [¶](#index-javascript_002dcallback-xwidget-events)
    :   This event contains JavaScript callback data. These events are used
        internally by `xwidget-webkit-execute-script`.

`(xwidget-display-event xwidget source)` [¶](#index-xwidget_002ddisplay_002devent-event)
:   This event is sent whenever an xwidget requests that another xwidget
    be displayed. xwidget is the xwidget that should be displayed,
    and source is the xwidget that asked to display xwidget.

    It is also a special event which should be handled through callbacks.
    You can add such a callback by setting the `display-callback` of
    source’s property list, which should be a function that accepts
    xwidget and source as arguments.

    xwidget’s buffer will be set to a temporary buffer. When
    displaying the widget, care should be taken to replace the buffer with
    the buffer in which the xwidget will be displayed, using
    `set-xwidget-buffer` (see [Embedded Native Widgets](#Xwidgets)).

---

Next: [Event Examples](#Event-Examples), Previous: [Xwidget events](#Xwidget-Events), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.12 Miscellaneous System Events [¶](#Miscellaneous-System-Events)

A few other event types represent occurrences within the system.

`text-conversion` [¶](#index-text_002dconversion-event)
:   This kind of event is sent **after** a system-wide input method
    performs an edit to one or more buffers.

    Once the event is sent, the input method may already have made changes
    to multiple buffers inside many different frames. To determine which
    buffers have been changed, and what edits have been made to them, use
    the variable `text-conversion-edits`, which is set prior to each
    `text-conversion` event being sent; it is a list of the form:

    ```
    ((buffer beg end ephemeral) ...)
    ```

    Where ephemeral is the buffer which was modified, beg and
    end are markers set to the positions of the edit at the time it
    was completed, and ephemeral is either a string, containing any
    text which was inserted (or any text before point which was deleted),
    `t`, meaning that the edit is a temporary edit made by the input
    method, or `nil`, meaning that some text was deleted after point.

    Whether or not this event is sent depends on the value of the
    buffer-local variable `text-conversion-style`, which determines
    how an input method that wishes to make edits to buffer contents will
    behave.

    This variable can have one of four values:

    `nil`
    :   This means that the input method will be disabled entirely, and key
        events will be sent instead of text conversion events.

    `action`
    :   This means that the input method will be enabled, but `RET` will
        be sent whenever the input method wants to insert a new line.

    `password`
    :   This is largely identical to `action`, but also requests an input
        method capable of inserting ASCII characters, and instructs it not to
        save input in locations from which it might be subsequently retrieved
        by features of the input method that cannot handle sensitive
        information, such as text suggestions.

    `t`
    :   This, or any other value, means that the input method will be enabled
        and make edits followed by `text-conversion` events.

    Changes to the value of this variable will only take effect upon the
    next redisplay after the buffer becomes the selected buffer of a
    frame. If you need to disable text conversion in a way that takes
    immediate effect, call the function `set-text-conversion-style`
    instead. This has the potential to lock up the input method for a
    significant amount of time, and should be used with care.

    In addition, text conversion is automatically disabled after a prefix
    key is read by the command loop or `read-key-sequence`. This can
    be disabled by setting or binding the variable
    `disable-inhibit-text-conversion` to a non-`nil` value.

`(delete-frame (frame))` [¶](#index-delete_002dframe-event)
:   This kind of event indicates that the user gave the window manager
    a command to delete a particular window, which happens to be an Emacs frame.

    The standard definition of the `delete-frame` event is to delete frame.

`(iconify-frame (frame))` [¶](#index-iconify_002dframe-event)
:   This kind of event indicates that the user iconified frame using
    the window manager. Its standard definition is `ignore`; since the
    frame has already been iconified, Emacs has no work to do. The purpose
    of this event type is so that you can keep track of such events if you
    want to.

`(make-frame-visible (frame))` [¶](#index-make_002dframe_002dvisible-event)
:   This kind of event indicates that the user deiconified frame using
    the window manager. Its standard definition is `ignore`; since the
    frame has already been made visible, Emacs has no work to do.

`(touch-end (position))` [¶](#index-touch_002dend-event)
:   This kind of event indicates that the user’s finger moved off the
    mouse wheel or the touchpad. The position element is a mouse
    position list (see [Click Events](#Click-Events)), specifying the position of the
    mouse cursor when the finger moved off the mouse wheel.

`(wheel-up position clicks lines pixel-delta)` [¶](#index-wheel_002dup-event)

`(wheel-down position clicks lines pixel-delta)`
:   These events are generated by moving a mouse wheel. The
    position element is a mouse position list (see [Click Events](#Click-Events)), specifying the position of the mouse cursor when the event
    occurred.

    clicks, if present, is the number of times that the wheel was
    moved in quick succession. See [Repeat Events](#Repeat-Events). lines, if
    present and not `nil`, is the positive number of screen lines
    that should be scrolled (either up, when the event is `wheel-up`,
    or down when the event is `wheel-down`). pixel-delta, if
    present, is a cons cell of the form `(x . y)`,
    where x and y are the numbers of pixels by which to scroll
    in each axis, a.k.a. *pixelwise deltas*. Usually, only one of
    the two will be non-zero, the other will be either zero or very close
    to zero; the larger number indicates the axis to scroll the window.
    When the variable `mwheel-coalesce-scroll-events` is `nil`,
    the scroll commands ignore the lines element, even if it’s
    non-`nil`, and use the pixel-delta data instead; in that
    case, the direction of scrolling is determined by the sign of the
    pixelwise deltas, and the direction (up or down) implied by the event
    kind is ignored.

    You can use these x and y pixelwise deltas to determine
    how much the mouse wheel has actually moved at pixel resolution. For
    example, the pixelwise deltas could be used to scroll the display at
    pixel resolution, exactly according to the user’s turning the mouse
    wheel. This pixelwise scrolling is possible only when
    `mwheel-coalesce-scroll-events` is `nil`, and in general the
    pixel-delta data is not generated when that variable is
    non-`nil`.

    The `wheel-up` and `wheel-down` events are generated only on
    some kinds of systems. On other systems, other events like `mouse-4` and
    `mouse-5` are used instead. Portable code should handle both
    `wheel-up` and `wheel-down` events as well as the events
    specified in the variables `mouse-wheel-up-event` and
    `mouse-wheel-down-event`, defined in `mwheel.el`.
    Beware that for historical reasons the `mouse-wheel-up-event`
    is the variable that holds an event that should be handled similarly to
    `wheel-down` and vice versa.

    The same holds for the horizontal wheel movements which are usually
    represented by `wheel-left` and `wheel-right` events, but
    for which portable code should also obey the variables
    `mouse-wheel-left-event` and `mouse-wheel-right-event`,
    defined in `mwheel.el`.
    However, some mice also generate other events at the same time as
    they’re generating these scroll events which may get in the way.
    The way to fix this is generally to unbind these events (for instance,
    `mouse-6` or `mouse-7`, but this is very hardware and
    operating system dependent).

`(pinch position dx dy scale angle)` [¶](#index-pinch-event)
:   This kind of event is generated by the user performing a “pinch”
    gesture by placing two fingers on a touchpad and moving them towards
    or away from each other. position is a mouse position list
    (see [Click Events](#Click-Events)) that provides the position of the mouse pointer
    when the event occurred, dx is the change in the horizontal
    distance between the fingers since the last event in the same sequence,
    dy is the vertical movement of the fingers since the last event
    in the same sequence, scale is the ratio of the current distance
    between the fingers to that distance at the start of the sequence, and
    angle is the angular difference in degrees between the direction
    of the line connecting the fingers in this event and the direction of
    that line in the last event of the same sequence.

    As pinch events are only sent at the beginning or during a pinch
    sequence, they do not report gestures where the user moves two fingers
    on a touchpad in a rotating fashion without pinching the fingers.

    All arguments after position are floating point numbers.

    This event is usually sent as part of a sequence, which begins with
    the user placing two fingers on the touchpad, and ends with the user
    removing those fingers. dx, dy, and angle will be
    `0.0` in the first event of a sequence; subsequent events will
    report non-zero values for these members of the event structure.

    dx and dy are reported in imaginary relative units, in
    which `1.0` is the width and height of the touchpad
    respectively. They are usually interpreted as being relative to the
    size of the object beneath the gesture: image, window, etc.

`(preedit-text arg)` [¶](#index-preedit_002dtext-event)
:   This event is sent when a system input method tells Emacs to display
    some text to indicate to the user what will be inserted. The contents
    of arg are dependent on the window system being used.

    On X, arg is a string describing some text to place behind the
    cursor. It can be `nil`, which means to remove any text
    previously displayed.

    On PGTK frames (see [Frames](#Frames)), arg is a list of strings with
    information about their color and underline attributes. It has the
    following form:

    ```
       ((string1
         (ul . underline-color)
         (bg . background-color)
         (fg . foreground-color))
        (string2
         (ul . underline-color)
         (bg . background-color)
         (fg . foreground-color))
        ...
       )
    ```

    Color information can be omitted, leaving just the text of the
    strings. underline-color can be `t`, meaning underlined
    text with default underline color, or it can be a string, the name of
    the color to draw the underline.

    This is a special event (see [Special Events](#Special-Events)), which normally
    should not be bound by the user to any command. Emacs will typically
    display the text contained in the event in an overlay behind point
    when it is received.

`(drag-n-drop position files)` [¶](#index-drag_002dn_002ddrop-event)
:   This kind of event is generated when a group of files is
    selected in an application outside of Emacs, and then dragged and
    dropped onto an Emacs frame.

    The element position is a list describing the position of the
    event, in the same format as used in a mouse-click event (see [Click Events](#Click-Events)), and files is the list of file names that were dragged
    and dropped. The usual way to handle this event is by visiting these
    files.

    This kind of event is generated, at present, only on some kinds of
    systems.

`help-echo` [¶](#index-help_002decho-event)
:   This kind of event is generated when a mouse pointer moves onto a
    portion of buffer text which has a `help-echo` text property.
    The generated event has this form:

    ```
    (help-echo frame help window object pos)
    ```

    The precise meaning of the event parameters and the way these
    parameters are used to display the help-echo text are described in
    [Text help-echo](#Text-help_002decho).

`sigusr1` [¶](#index-sigusr1-event)

`sigusr2`
:   These events are generated when the Emacs process receives
    the signals `SIGUSR1` and `SIGUSR2`. They contain no
    additional data because signals do not carry additional information.
    They can be useful for debugging (see [Entering the Debugger on an Error](#Error-Debugging)).

    To catch a user signal, bind the corresponding event to an interactive
    command in the `special-event-map` (see [Controlling the Active Keymaps](#Controlling-Active-Maps)).
    The command is called with no arguments, and the specific signal event is
    available in `last-input-event` (see [Miscellaneous Event Input Features](#Event-Input-Misc). For
    example:

    ```
    (defun sigusr-handler ()
      (interactive)
      (message "Caught signal %S" last-input-event))

    (keymap-set special-event-map "<sigusr1>" 'sigusr-handler)
    ```

    To test the signal handler, you can make Emacs send a signal to itself:

    ```
    (signal-process (emacs-pid) 'sigusr1)
    ```

`(sleep-event state)` [¶](#index-sleep_002devent-event)
:   This event is injected when the device Emacs is running on is about to
    enter a sleep state, or has just awoken from one. state will be
    the symbol `pre-sleep` or `post-wake`.

    This is implemented on GNU/Linux, macOS, and MS-Windows.

`language-change` [¶](#index-language_002dchange-event)
:   This kind of event is generated on MS-Windows when the input language
    has changed. This typically means that the keyboard keys will send to
    Emacs characters from a different language. The generated event has
    this form:

    ```
    (language-change frame codepage language-id)
    ```

    Here frame is the frame which was current when the input
    language changed; codepage is the new codepage number; and
    language-id is the numerical ID of the new input language. The
    coding-system (see [Coding Systems](#Coding-Systems)) that corresponds to
    codepage is `cpcodepage` or
    `windows-codepage`. To convert language-id to a
    string (e.g., to use it for various language-dependent features, such
    as `set-language-environment`), use the
    `w32-get-locale-info` function, like this:

    ```
    ;; Get the abbreviated language name, such as "ENU" for English
    (w32-get-locale-info language-id)
    ;; Get the full English name of the language,
    ;; such as "English (United States)"
    (w32-get-locale-info language-id 4097)
    ;; Get the full localized name of the language
    (w32-get-locale-info language-id t)
    ```

`end-session` [¶](#index-end_002dsession-event)
:   This event is generated on MS-Windows when the operating system
    informs Emacs that the user terminated the interactive session, or
    that the system is shutting down. The standard definition of this
    event is to invoke the `kill-emacs` command (see [Killing Emacs](#Killing-Emacs)) so as to shut down Emacs in an orderly fashion; if there are
    unsaved changes, this will produce auto-save files
    (see [Auto-Saving](#Auto_002dSaving)) that the user can use after restarting the
    session to restore the unsaved edits.

If one of these events arrives in the middle of a key sequence—that
is, after a prefix key—then Emacs reorders the events so that this
event comes either before or after the multi-event key sequence, not
within it.

Some of these special events, such as `delete-frame`, invoke
Emacs commands by default; others are not bound. If you want to
arrange for a special event to invoke a command, you can do that via
`special-event-map`. The command you bind to a function key in
that map can then examine the full event which invoked it in
`last-input-event`. See [Special Events](#Special-Events).

---

Next: [Classifying Events](#Classifying-Events), Previous: [Miscellaneous System Events](#Misc-Events), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.13 Event Examples [¶](#Event-Examples-1)

If the user presses and releases the left mouse button over the same
location, that generates a sequence of events like this:

```
(down-mouse-1 (#<window 18 on NEWS> 2613 (0 . 38) -864320))
(mouse-1      (#<window 18 on NEWS> 2613 (0 . 38) -864180))
```

While holding the control key down, the user might hold down the
second mouse button, and drag the mouse from one line to the next.
That produces two events, as shown here:

```
(C-down-mouse-2 (#<window 18 on NEWS> 3440 (0 . 27) -731219))
(C-drag-mouse-2 (#<window 18 on NEWS> 3440 (0 . 27) -731219)
                (#<window 18 on NEWS> 3510 (0 . 28) -729648))
```

While holding down the meta and shift keys, the user might press the
second mouse button on the window’s mode line, and then drag the mouse
into another window. That produces a pair of events like these:

```
(M-S-down-mouse-2 (#<window 18 on NEWS> mode-line (33 . 31) -457844))
(M-S-drag-mouse-2 (#<window 18 on NEWS> mode-line (33 . 31) -457844)
                  (#<window 20 on carlton-sanskrit.tex> 161 (33 . 3)
                   -453816))
```

The frame with input focus might not take up the entire screen, and
the user might move the mouse outside the scope of the frame. Inside
the `track-mouse` macro, that produces an event like this:

```
(mouse-movement (#<frame *ielm* 0x102849a30> nil (563 . 205) 532301936))
```

---

Next: [Accessing Mouse Events](#Accessing-Mouse), Previous: [Event Examples](#Event-Examples), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.14 Classifying Events [¶](#Classifying-Events-1)

Every event has an *event type*, which classifies the event for
key binding purposes. For a keyboard event, the event type equals the
event value; thus, the event type for a character is the character, and
the event type for a function key symbol is the symbol itself. For
events that are lists, the event type is the symbol in the CAR of
the list. Thus, the event type is always a symbol or a character.

Two events of the same type are equivalent where key bindings are
concerned; thus, they always run the same command. That does not
necessarily mean they do the same things, however, as some commands look
at the whole event to decide what to do. For example, some commands use
the location of a mouse event to decide where in the buffer to act.

Sometimes broader classifications of events are useful. For example,
you might want to ask whether an event involved the `META` key,
regardless of which other key or mouse button was used.

The functions `event-modifiers` and `event-basic-type` are
provided to get such information conveniently.

Function: **event-modifiers** event [¶](#index-event_002dmodifiers)
:   This function returns a list of the modifiers that event has. The
    modifiers are symbols; they include `shift`, `control`,
    `meta`, `alt`, `hyper` and `super`. In addition,
    the modifiers list of a mouse event symbol always contains one of
    `click`, `drag`, and `down`. For double or triple
    events, it also contains `double` or `triple`.

    The argument event may be an entire event object, or just an
    event type. If event is a symbol that has never been used in an
    event that has been read as input in the current Emacs session, then
    `event-modifiers` can return `nil`, even when event
    actually has modifiers.

    Here are some examples:

    ```
    (event-modifiers ?a)
         ⇒ nil
    (event-modifiers ?A)
         ⇒ (shift)
    (event-modifiers ?\C-a)
         ⇒ (control)
    (event-modifiers ?\C-%)
         ⇒ (control)
    (event-modifiers ?\C-\S-a)
         ⇒ (control shift)
    (event-modifiers 'f5)
         ⇒ nil
    (event-modifiers 's-f5)
         ⇒ (super)
    (event-modifiers 'M-S-f5)
         ⇒ (meta shift)
    (event-modifiers 'mouse-1)
         ⇒ (click)
    (event-modifiers 'down-mouse-1)
         ⇒ (down)
    ```

    The modifiers list for a click event explicitly contains `click`,
    but the event symbol name itself does not contain ‘`click`’.
    Similarly, the modifiers list for an ASCII control
    character, such as ‘`C-a`’, contains `control`, even though
    reading such an event via `read-char` will return the value 1
    with the control modifier bit removed.

Function: **event-basic-type** event [¶](#index-event_002dbasic_002dtype)
:   This function returns the key or mouse button that event
    describes, with all modifiers removed. The event argument is as
    in `event-modifiers`. For example:

    ```
    (event-basic-type ?a)
         ⇒ 97
    (event-basic-type ?A)
         ⇒ 97
    (event-basic-type ?\C-a)
         ⇒ 97
    (event-basic-type ?\C-\S-a)
         ⇒ 97
    (event-basic-type 'f5)
         ⇒ f5
    (event-basic-type 's-f5)
         ⇒ f5
    (event-basic-type 'M-S-f5)
         ⇒ f5
    (event-basic-type 'down-mouse-1)
         ⇒ mouse-1
    ```

Function: **mouse-movement-p** object [¶](#index-mouse_002dmovement_002dp)
:   This function returns non-`nil` if object is a mouse movement
    event. See [Motion Events](#Motion-Events).

---

Next: [Accessing Scroll Bar Events](#Accessing-Scroll), Previous: [Classifying Events](#Classifying-Events), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.15 Accessing Mouse Events [¶](#Accessing-Mouse-Events)

This section describes convenient functions for accessing the data in
a mouse button or motion event. Keyboard event data can be accessed
using the same functions, but data elements that aren’t applicable to
keyboard events are zero or `nil`.

The following two functions return a mouse position list
(see [Click Events](#Click-Events)), specifying the position of a mouse event.

Function: **event-start** event [¶](#index-event_002dstart)
:   This returns the starting position of event.

    If event is a click or button-down event, this returns the
    location of the event. If event is a drag event, this returns the
    drag’s starting position.

Function: **event-end** event [¶](#index-event_002dend)
:   This returns the ending position of event.

    If event is a drag event, this returns the position where the user
    released the mouse button. If event is a click or button-down
    event, the value is actually the starting position, which is the only
    position such events have.

Function: **posnp** object [¶](#index-posnp)
:   This function returns non-`nil` if object is a mouse
    position list, in the format documented in [Click Events](#Click-Events)); and
    `nil` otherwise.

These functions take a mouse position list as argument, and return
various parts of it:

Function: **posn-window** position [¶](#index-posn_002dwindow)
:   Return the window that position is in. If position
    represents a location outside the frame where the event was initiated,
    return that frame instead.

Function: **posn-area** position [¶](#index-posn_002darea)
:   Return the window area recorded in position. It returns `nil`
    when the event occurred in the text area of the window; otherwise, it
    is a symbol identifying the area in which the event occurred.

Function: **posn-point** position [¶](#index-posn_002dpoint)
:   Return the buffer position in position. When the event occurred
    in the text area of the window, in a marginal area, or on a fringe,
    this is an integer specifying a buffer position. Otherwise, the value
    is undefined.

Function: **posn-x-y** position [¶](#index-posn_002dx_002dy)
:   Return the pixel-based x and y coordinates in position, as a
    cons cell `(x . y)`. These coordinates are
    relative to the window given by `posn-window`.

    This example shows how to convert the window-relative coordinates in
    the text area of a window into frame-relative coordinates:

    ```
    (defun frame-relative-coordinates (position)
      "Return frame-relative coordinates from POSITION.
    POSITION is assumed to lie in a window text area."
      (let* ((x-y (posn-x-y position))
             (window (posn-window position))
             (edges (window-inside-pixel-edges window)))
        (cons (+ (car x-y) (car edges))
              (+ (cdr x-y) (cadr edges)))))
    ```

Function: **posn-col-row** position &optional use-window [¶](#index-posn_002dcol_002drow)
:   This function returns a cons cell `(col . row)`,
    containing the estimated column and row corresponding to buffer
    position described by position. The return value is given in
    units of the frame’s default character width and default line height
    (including spacing), as computed from the x and y values
    corresponding to position. (So, if the actual characters have
    non-default sizes, the actual row and column may differ from these
    computed values.) If the optional window argument is
    non-`nil`, use the default character width in the window
    indicated by position instead of the frame. (This makes a
    difference if that window is showing a buffer with a non-default
    zooming level, for instance.)

    Note that row is counted from the top of the text area. If the
    window given by position possesses a header line (see [Window Header Lines](#Header-Lines)) or a tab line, they are *not* included in the row
    count.

Function: **posn-actual-col-row** position [¶](#index-posn_002dactual_002dcol_002drow)
:   Return the actual row and column in position, as a cons cell
    `(col . row)`. The values are the actual row and
    column numbers in the window given by position. See [Click Events](#Click-Events), for details. The function returns `nil` if
    position does not include actual position values; in that case
    `posn-col-row` can be used to get approximate values.

    Note that this function doesn’t account for the visual width of
    characters on display, like the number of visual columns taken by a
    tab character or an image. If you need the coordinates in canonical
    character units, use `posn-col-row` instead.

Function: **posn-string** position [¶](#index-posn_002dstring)
:   Return the string object described by position, either
    `nil` (which means position describes buffer text), or a
    cons cell `(string . string-pos)`.

Function: **posn-image** position [¶](#index-posn_002dimage)
:   Return the image object in position, either `nil` (if
    there’s no image at position), or an image spec `(image …)`.

Function: **posn-object** position [¶](#index-posn_002dobject)
:   Return the image or string object described by position, either
    `nil` (which means position describes buffer text), an
    image `(image …)`, or a cons cell
    `(string . string-pos)`.

Function: **posn-object-x-y** position [¶](#index-posn_002dobject_002dx_002dy)
:   Return the pixel-based x and y coordinates relative to the upper left
    corner of the object described by position, as a cons cell
    `(dx . dy)`. If the position describes
    buffer text, return the relative coordinates of the buffer-text character
    closest to that position.

Function: **posn-object-width-height** position [¶](#index-posn_002dobject_002dwidth_002dheight)
:   Return the pixel width and height of the object described by
    position, as a cons cell `(width . height)`.
    If the position describes a buffer position, return the size of
    the character at that position.

Function: **posn-timestamp** position [¶](#index-posn_002dtimestamp)
:   Return the timestamp in position. This is the time at which the
    event occurred, in milliseconds. Such a timestamp is reported
    relative to an arbitrary starting time that varies according to the
    window system in use. On the X Window System, for example, it is the
    number of milliseconds since the X server was started.

These functions compute a position list given particular buffer
position or screen position. You can access the data in this position
list with the functions described above.

Function: **posn-at-point** &optional pos window [¶](#index-posn_002dat_002dpoint)
:   This function returns a position list for position pos in
    window. pos defaults to point in window;
    window defaults to the selected window.

    `posn-at-point` returns `nil` if pos is not visible in
    window.

Function: **posn-at-x-y** x y &optional frame-or-window whole [¶](#index-posn_002dat_002dx_002dy)
:   This function returns position information corresponding to pixel
    coordinates x and y in a specified frame or window,
    frame-or-window, which defaults to the selected window.
    The coordinates x and y are relative to the
    text area of the selected window.
    If whole is non-`nil`, the x coordinate is relative
    to the entire window area including scroll bars, margins and fringes.

User Option: **mouse-prefer-closest-glyph** [¶](#index-mouse_002dprefer_002dclosest_002dglyph)
:   If this variable is non-`nil`, the `posn-point` of a mouse
    position list will be set to the position of the glyph whose leftmost
    edge is the closest to the mouse click, as opposed to the position of
    the glyph underneath the mouse pointer itself. For example, if
    `posn-at-x-y` is called with x set to `9`, which is
    contained within a character of width 10 displayed at column 0, the
    point saved within the mouse position list will be *after* that
    character, not *before* it.

---

Next: [Putting Keyboard Events in Strings](#Strings-of-Events), Previous: [Accessing Mouse Events](#Accessing-Mouse), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.16 Accessing Scroll Bar Events [¶](#Accessing-Scroll-Bar-Events)

These functions are useful for decoding scroll bar events.

Function: **scroll-bar-event-ratio** event [¶](#index-scroll_002dbar_002devent_002dratio)
:   This function returns the fractional vertical position of a scroll bar
    event within the scroll bar. The value is a cons cell
    `(portion . whole)` containing two integers whose ratio
    is the fractional position.

Function: **scroll-bar-scale** ratio total [¶](#index-scroll_002dbar_002dscale)
:   This function multiplies (in effect) ratio by total,
    rounding the result to an integer. The argument ratio is not a
    number, but rather a pair `(num . denom)`—typically a
    value returned by `scroll-bar-event-ratio`.

    This function is handy for scaling a position on a scroll bar into a
    buffer position. Here’s how to do that:

    ```
    (+ (point-min)
       (scroll-bar-scale
          (posn-x-y (event-start event))
          (- (point-max) (point-min))))
    ```

    Recall that scroll bar events have two integers forming a ratio, in place
    of a pair of x and y coordinates.

---

Previous: [Accessing Scroll Bar Events](#Accessing-Scroll), Up: [Input Events](#Input-Events)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.7.17 Putting Keyboard Events in Strings [¶](#Putting-Keyboard-Events-in-Strings)

In most of the places where strings are used, we conceptualize the
string as containing text characters—the same kind of characters found
in buffers or files. Occasionally Lisp programs use strings that
conceptually contain keyboard characters; for example, they may be key
sequences or keyboard macro definitions. However, storing keyboard
characters in a string is a complex matter, for reasons of historical
compatibility, and it is not always possible.

We recommend that new programs avoid dealing with these complexities
by not storing keyboard events in strings containing control
characters or the like, but instead store them in the common Emacs
format as understood by `key-valid-p`.

If you read a key sequence with `read-key-sequence-vector` (or
`read-key-sequence`), or access a key sequence with
`this-command-keys-vector` (or `this-command-keys`), you can
transform this to the recommended format by using `key-description`.

The complexities stem from the modifier bits that keyboard input
characters can include. Aside from the Meta modifier, none of these
modifier bits can be included in a string, and the Meta modifier is
allowed only in special cases.

The earliest GNU Emacs versions represented meta characters as codes
in the range of 128 to 255. At that time, the basic character codes
ranged from 0 to 127, so all keyboard character codes did fit in a
string. Many Lisp programs used ‘`\M-`’ in string constants to stand
for meta characters, especially in arguments to `define-key` and
similar functions, and key sequences and sequences of events were always
represented as strings.

When we added support for larger basic character codes beyond 127, and
additional modifier bits, we had to change the representation of meta
characters. Now the flag that represents the Meta modifier in a
character is
2\*\*27
and such numbers cannot be included in a string.

To support programs with ‘`\M-`’ in string constants, there are
special rules for including certain meta characters in a string.
Here are the rules for interpreting a string as a sequence of input
characters:

* If the keyboard character value is in the range of 0 to 127, it can go
  in the string unchanged.
* The meta variants of those characters, with codes in the range of
  2\*\*27
  to
  2\*\*27+127,
  can also go in the string, but you must change their
  numeric values. You must set the
  2\*\*7
  bit instead of the
  2\*\*27
  bit, resulting in a value between 128 and 255. Only a unibyte string
  can include these codes.
* Non-ASCII characters above 256 can be included in a multibyte string.
* Other keyboard character events cannot fit in a string. This includes
  keyboard events in the range of 128 to 255.
