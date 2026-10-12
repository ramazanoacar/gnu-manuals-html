Functions such as `read-key-sequence` that construct strings of
keyboard input characters follow these rules: they construct vectors
instead of strings, when the events won’t fit in a string.

When you use the read syntax ‘`\M-`’ in a string, it produces a
code in the range of 128 to 255—the same code that you get if you
modify the corresponding keyboard event to put it in the string. Thus,
meta events in strings work consistently regardless of how they get into
the strings.

However, most programs would do well to avoid these issues by
following the recommendations at the beginning of this section.

---

Next: [Special Events](#Special-Events), Previous: [Input Events](#Input-Events), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.8 Reading Input [¶](#Reading-Input-1)

The editor command loop reads key sequences using the function
`read-key-sequence`, which uses `read-event`. These and other
functions for event input are also available for use in Lisp programs.
See also `momentary-string-display` in [Temporary Displays](#Temporary-Displays),
and `sit-for` in [Waiting for Elapsed Time or Input](#Waiting). See [Terminal Input](#Terminal-Input), for
functions and variables for controlling terminal input modes and
debugging terminal input.

For higher-level input facilities, see [Minibuffers](#Minibuffers).

* [Key Sequence Input](#Key-Sequence-Input)
* [Reading One Event](#Reading-One-Event)
* [Modifying and Translating Input Events](#Event-Mod)
* [Invoking the Input Method](#Invoking-the-Input-Method)
* [Quoted Character Input](#Quoted-Character-Input)
* [Miscellaneous Event Input Features](#Event-Input-Misc)

---

Next: [Reading One Event](#Reading-One-Event), Up: [Reading Input](#Reading-Input)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.8.1 Key Sequence Input [¶](#Key-Sequence-Input-1)

The command loop reads input a key sequence at a time, by calling
`read-key-sequence`. Lisp programs can also call this function;
for example, `describe-key` uses it to read the key to describe.

Function: **read-key-sequence** prompt &optional continue-echo dont-downcase-last switch-frame-ok command-loop disable-text-conversion [¶](#index-read_002dkey_002dsequence)
:   This function reads a key sequence and returns it as a string or
    vector. It keeps reading events until it has accumulated a complete key
    sequence; that is, enough to specify a non-prefix command using the
    currently active keymaps. (Remember that a key sequence that starts
    with a mouse event is read using the keymaps of the buffer in the
    window that the mouse was in, not the current buffer.)

    If the events are all characters and all can fit in a string, then
    `read-key-sequence` returns a string (see [Putting Keyboard Events in Strings](#Strings-of-Events)).
    Otherwise, it returns a vector, since a vector can hold all kinds of
    events—characters, symbols, and lists. The elements of the string or
    vector are the events in the key sequence.

    Reading a key sequence includes translating the events in various
    ways. See [Keymaps for Translating Sequences of Events](#Translation-Keymaps).

    The argument prompt is either a string to be displayed in the
    echo area as a prompt, or `nil`, meaning not to display a prompt.
    The argument continue-echo, if non-`nil`, means to echo
    this key as a continuation of the previous key.

    Normally any upper case event is converted to lower case if the
    original event is undefined and the lower case equivalent is defined.
    The argument dont-downcase-last, if non-`nil`, means do not
    convert the last event to lower case. This is appropriate for reading
    a key sequence to be defined.

    The argument switch-frame-ok, if non-`nil`, means that this
    function should process a `switch-frame` event if the user
    switches frames before typing anything. If the user switches frames
    in the middle of a key sequence, or at the start of the sequence but
    switch-frame-ok is `nil`, then the event will be put off
    until after the current key sequence.

    The argument command-loop, if non-`nil`, means that this
    key sequence is being read by something that will read commands one
    after another. It should be `nil` if the caller will read just
    one key sequence.

    The argument disable-text-conversion, if non-`nil`, means
    that system input methods will not directly perform edits to buffer
    text while this key sequence is being read; user input will always
    generated individual key events instead. See [Miscellaneous System Events](#Misc-Events), for more
    about text conversion.

    In the following example, Emacs displays the prompt ‘`?`’ in the
    echo area, and then the user types `C-x C-f`.

    ```
    (read-key-sequence "?")
    ```

    ```
    ---------- Echo Area ----------
    ?C-x C-f
    ---------- Echo Area ----------

         ⇒ "^X^F"
    ```

    The function `read-key-sequence` suppresses quitting: `C-g`
    typed while reading with this function works like any other character,
    and does not set `quit-flag`. See [Quitting](#Quitting).

Function: **read-key-sequence-vector** prompt &optional continue-echo dont-downcase-last switch-frame-ok command-loop disable-text-conversion [¶](#index-read_002dkey_002dsequence_002dvector)
:   This is like `read-key-sequence` except that it always
    returns the key sequence as a vector, never as a string.
    See [Putting Keyboard Events in Strings](#Strings-of-Events).

If an input character is upper-case (or has the shift modifier) and
has no key binding, but its lower-case equivalent has one, then
`read-key-sequence` converts the character to lower case. (This
behavior can be disabled by setting the
`translate-upper-case-key-bindings` user option to `nil`.)
Note that `lookup-key` does not perform case conversion in this
way.

When reading input results in such a *shift-translation*, Emacs
sets the variable `this-command-keys-shift-translated` to a
non-`nil` value. Lisp programs can examine this variable if they
need to modify their behavior when invoked by shift-translated keys.
For example, the function `handle-shift-selection` examines the
value of this variable to determine how to activate or deactivate the
region (see [handle-shift-selection](#The-Mark)).

The function `read-key-sequence` also transforms some mouse events.
It converts unbound drag events into click events, and discards unbound
button-down events entirely. It also reshuffles focus events and
miscellaneous window events so that they never appear in a key sequence
with any other events.

When mouse or `touchscreen-begin` and `touchscreen-end`
events occur in special parts of a window or frame, such as a mode
line or a scroll bar, the event type shows nothing special—it is the
same symbol that would normally represent that combination of mouse
button and modifier keys. The information about the window part is
kept elsewhere in the event—in the coordinates. But
`read-key-sequence` translates this information into imaginary
prefix keys, all of which are symbols: `tab-line`,
`header-line`, `horizontal-scroll-bar`, `menu-bar`,
`tab-bar`, `mode-line`, `vertical-line`,
`vertical-scroll-bar`, `left-margin`, `right-margin`,
`left-fringe`, `right-fringe`, `right-divider`, and
`bottom-divider`. You can define meanings for mouse clicks in
special window parts by defining key sequences using these imaginary
prefix keys.

For example, if you call `read-key-sequence` and then click the
mouse on the window’s mode line, you get two events, like this:

```
(read-key-sequence "Click on the mode line: ")
     ⇒ [mode-line
         (mouse-1
          (#<window 6 on NEWS> mode-line
           (40 . 63) 5959987))]
```

Variable: **num-input-keys** [¶](#index-num_002dinput_002dkeys)
:   This variable’s value is the number of key sequences processed so far in
    this Emacs session. This includes key sequences read from the terminal
    and key sequences read from keyboard macros being executed.

---

Next: [Modifying and Translating Input Events](#Event-Mod), Previous: [Key Sequence Input](#Key-Sequence-Input), Up: [Reading Input](#Reading-Input)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.8.2 Reading One Event [¶](#Reading-One-Event-1)

The lowest level functions for command input are `read-event`,
`read-char`, and `read-char-exclusive`.

If you need a function to read a character using the minibuffer, use
`read-char-from-minibuffer` (see [Asking Multiple-Choice Questions](#Multiple-Queries)).

Function: **read-event** &optional prompt inherit-input-method seconds [¶](#index-read_002devent)
:   This function reads and returns the next event of command input,
    waiting if necessary until an event is available.

    The returned event may come directly from the user, or from a keyboard
    macro. It is not decoded by the keyboard’s input coding system
    (see [Terminal I/O Encoding](#Terminal-I_002fO-Encoding)).

    If the optional argument prompt is non-`nil`, it should be
    a string to display in the echo area as a prompt. If prompt is
    `nil` or the string ‘`""`’, `read-event` does not display
    any message to indicate it is waiting for input; instead, it prompts
    by echoing: it displays descriptions of the events that led to or were
    read by the current command. See [The Echo Area](#The-Echo-Area).

    If inherit-input-method is non-`nil`, then the current input
    method (if any) is employed to make it possible to enter a
    non-ASCII character. Otherwise, input method handling is disabled
    for reading this event.

    If `cursor-in-echo-area` is non-`nil`, then `read-event`
    moves the cursor temporarily to the echo area, to the end of any message
    displayed there. Otherwise `read-event` does not move the cursor.

    If seconds is non-`nil`, it should be a number specifying
    the maximum time to wait for input, in seconds. If no input arrives
    within that time, `read-event` stops waiting and returns
    `nil`. A floating point seconds means to wait
    for a fractional number of seconds. Some systems support only a whole
    number of seconds; on these systems, seconds is rounded down.
    If seconds is `nil`, `read-event` waits as long as
    necessary for input to arrive.

    If seconds is `nil`, Emacs is considered idle while waiting
    for user input to arrive. Idle timers—those created with
    `run-with-idle-timer` (see [Idle Timers](#Idle-Timers))—can run during this
    period. However, if seconds is non-`nil`, the state of
    idleness remains unchanged. If Emacs is non-idle when
    `read-event` is called, it remains non-idle throughout the
    operation of `read-event`; if Emacs is idle (which can happen if
    the call happens inside an idle timer), it remains idle.

    If `read-event` gets an event that is defined as a help character,
    then in some cases `read-event` processes the event directly without
    returning. See [Help Functions](#Help-Functions). Certain other events, called
    *special events*, are also processed directly within
    `read-event` (see [Special Events](#Special-Events)).

    Here is what happens if you call `read-event` and then press the
    right-arrow function key:

    ```
    (read-event)
         ⇒ right
    ```

Function: **read-char** &optional prompt inherit-input-method seconds [¶](#index-read_002dchar)
:   This function reads and returns a character input event. If the
    user generates an event which is not a character (i.e., a mouse click or
    function key event), `read-char` signals an error. The arguments
    work as in `read-event`.

    If the event has modifiers, Emacs attempts to resolve them and return
    the code of the corresponding character. For example, if the user
    types `C-a`, the function returns 1, which is the ASCII
    code of the ‘`C-a`’ character. If some of the modifiers cannot be
    reflected in the character code, `read-char` leaves the
    unresolved modifier bits set in the returned event. For example, if
    the user types `C-M-a`, the function returns 134217729, 8000001 in
    hex, i.e. ‘`C-a`’ with the Meta modifier bit set. This value is
    not a valid character code: it fails the `characterp` test
    (see [Character Codes](#Character-Codes)). Use `event-basic-type`
    (see [Classifying Events](#Classifying-Events)) to recover the character code with the
    modifier bits removed; use `event-modifiers` to test for
    modifiers in the character event returned by `read-char`.

    In the first example below, the user types the character `1`
    (ASCII code 49). The second example shows a keyboard macro
    definition that calls `read-char` from the minibuffer using
    `eval-expression`. `read-char` reads the keyboard macro’s
    very next character, which is `1`. Then `eval-expression`
    displays its return value in the echo area.

    ```
    (read-char)
         ⇒ 49
    ```

    ```

    ```

    ```
    ;; We assume here you use M-: to evaluate this.
    (symbol-function 'foo)
         ⇒ "^[:(read-char)^M1"
    ```

    ```
    (execute-kbd-macro 'foo)
         -| 49
         ⇒ nil
    ```

Function: **read-char-exclusive** &optional prompt inherit-input-method seconds [¶](#index-read_002dchar_002dexclusive)
:   This function reads and returns a character input event. If the
    user generates an event which is not a character event,
    `read-char-exclusive` ignores it and reads another event, until it
    gets a character. The arguments work as in `read-event`. The
    returned value may include modifier bits, as with `read-char`.

None of the above functions suppress quitting.

Variable: **num-nonmacro-input-events** [¶](#index-num_002dnonmacro_002dinput_002devents)
:   This variable holds the total number of input events received so far
    from the terminal—not counting those generated by keyboard macros.

We emphasize that, unlike `read-key-sequence`, the functions
`read-event`, `read-char`, and `read-char-exclusive` do
not perform the translations described in [Keymaps for Translating Sequences of Events](#Translation-Keymaps).
If you wish to read a single key taking these translations into
account (for example, to read [Function Keys](#Function-Keys) in a terminal or
[Mouse Events](#Mouse-Events) from `xterm-mouse-mode`), use the function
`read-key`:

Function: **read-key** &optional prompt disable-fallbacks [¶](#index-read_002dkey)
:   This function reads a single key. It is intermediate between
    `read-key-sequence` and `read-event`. Unlike the former, it
    reads a single key, not a key sequence. Unlike the latter, it does
    not return a raw event, but decodes and translates the user input
    according to `input-decode-map`, `local-function-key-map`,
    and `key-translation-map` (see [Keymaps for Translating Sequences of Events](#Translation-Keymaps)).

    The argument prompt is either a string to be displayed in the
    echo area as a prompt, or `nil`, meaning not to display a prompt.

    If argument disable-fallbacks is non-`nil` then the usual
    fallback logic for unbound keys in `read-key-sequence` is not
    applied. This means that mouse button-down and multi-click events
    will not be discarded and `local-function-key-map` and
    `key-translation-map` will not get applied. If `nil` or
    unspecified, the only fallback disabled is downcasing of the last
    event.

Function: **read-char-choice** prompt chars &optional inhibit-quit [¶](#index-read_002dchar_002dchoice)
:   This function uses `read-from-minibuffer` to read and return a
    single character that is a member of chars, which should be a
    list of single characters. It discards any input characters that are
    not members of chars, and shows a message to that effect.

    The optional argument inhibit-quit is by default ignored, but if
    the variable `read-char-choice-use-read-key` is non-`nil`,
    this function uses `read-key` instead of
    `read-from-minibuffer`, and in that case inhibit-quit
    non-`nil` means ignore keyboard-quit events while waiting for
    valid input. In addition, if `read-char-choice-use-read-key` is
    non-`nil`, binding `help-form` (see [Help Functions](#Help-Functions)) to a
    non-`nil` value while calling this function causes it to evaluate
    `help-form` and display the result when the user presses
    `help-char`; it then continues to wait for a valid input
    character, or for keyboard-quit.

Function: **read-multiple-choice** prompt choices &optional help-string show-help long-form [¶](#index-read_002dmultiple_002dchoice)
:   Ask user a multiple-choice question and return user’s choice.
    prompt should be a string to display as the prompt.

    choices is an alist where the first element in each entry is a
    character the user can type, the second element is a short name for the
    entry to be displayed while prompting (if there’s room, it might be
    shortened), and the third, optional entry is a longer explanation that
    will be displayed in a help buffer if the user requests more help.

    The return value is the entry from choices that matches the
    character the user types.

    If optional argument help-string is non-`nil`, it should be
    a string with a more detailed description of all choices. It will be
    displayed in a help buffer instead of the default auto-generated
    description when the user types `?`.

    If optional argument show-help is non-`nil`, the help
    buffer will be displayed immediately, before any user input. If it is
    a string, use it as the name of the help buffer.

    If optional argument long-form is non-`nil`, the user
    will have to type in long-form answers (using `completing-read`)
    instead of hitting a single key. The answers must be among the second
    elements of the values in the choices list.

    By default, this function uses `read-from-minibuffer` (see [Reading Text Strings with the Minibuffer](#Text-from-Minibuffer)) to read user input, but if the variable
    `read-char-choice-use-read-key` is non-`nil`, it uses
    `read-key` instead.

    ```
    (read-multiple-choice
     "Continue connecting?"
     '((?a "always" "Accept certificate for this and future sessions.")
       (?s "session only" "Accept certificate this session only.")
       (?n "no" "Refuse to use certificate, close connection.")))
    ```

    The `read-multiple-choice-face` face is used to highlight the
    matching characters in the name string on graphical terminals.

---

Next: [Invoking the Input Method](#Invoking-the-Input-Method), Previous: [Reading One Event](#Reading-One-Event), Up: [Reading Input](#Reading-Input)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.8.3 Modifying and Translating Input Events [¶](#Modifying-and-Translating-Input-Events)

Emacs modifies every event it reads according to
`extra-keyboard-modifiers`, then translates it through
`keyboard-translate-table` (if applicable), before returning it
from `read-event`.

Variable: **extra-keyboard-modifiers** [¶](#index-extra_002dkeyboard_002dmodifiers)
:   This variable lets Lisp programs “press” the modifier keys on the
    keyboard. The value is a character. Only the modifiers of the
    character matter. Each time the user types a keyboard key, it is
    altered as if those modifier keys were held down. For instance, if
    you bind `extra-keyboard-modifiers` to `?\C-\M-a`, then all
    keyboard input characters typed during the scope of the binding will
    have the control and meta modifiers applied to them. The character
    `?\C-@`, equivalent to the integer 0, does not count as a control
    character for this purpose, but as a character with no modifiers.
    Thus, setting `extra-keyboard-modifiers` to zero cancels any
    modification.

    When using a window system, the program can press any of the
    modifier keys in this way. Otherwise, only the `CTL` and `META`
    keys can be virtually pressed.

    Note that this variable applies only to events that really come from
    the keyboard, and has no effect on mouse events or any other events.

Variable: **keyboard-translate-table** [¶](#index-keyboard_002dtranslate_002dtable)
:   This terminal-local variable is the translate table for keyboard
    characters. It lets you reshuffle the keys on the keyboard without
    changing any command bindings. Its value is normally a char-table, or
    else `nil`. (It can also be a string or vector, but this is
    considered obsolete.)

    If `keyboard-translate-table` is a char-table
    (see [Char-Tables](#Char_002dTables)), then each character read from the keyboard is
    looked up in this char-table. If the value found there is
    non-`nil`, then it is used instead of the actual input character.

    Note that this translation is the first thing that happens to a
    character after it is read from the terminal. Record-keeping features
    such as `recent-keys` and dribble files record the characters after
    translation.

    Note also that this translation is done before the characters are
    supplied to input methods (see [Input Methods](#Input-Methods)). Use
    `translation-table-for-input` (see [Translation of Characters](#Translation-of-Characters)),
    if you want to translate characters after input methods operate.

Command: **key-translate** from to [¶](#index-key_002dtranslate)
:   This command modifies `keyboard-translate-table` to translate
    character code from into character code to. Interactively,
    it prompts for from and to. It creates the
    keyboard translate table if necessary. Both from and to
    should be strings that satisfy `key-valid-p` (see [Key Sequences](#Key-Sequences)). If to is `nil`, the function removes any
    existing translation for from.

Here’s an example of using the `keyboard-translate-table` to
make `C-x`, `C-c` and `C-v` perform the cut, copy and paste
operations:

```
(key-translate "C-x" "<control-x>")
(key-translate "C-c" "<control-c>")
(key-translate "C-v" "<control-v>")
(keymap-global-set "<control-x>" 'kill-region)
(keymap-global-set "<control-c>" 'kill-ring-save)
(keymap-global-set "<control-v>" 'yank)
```

On a graphical terminal that supports extended ASCII input,
you can still get the standard Emacs meanings of one of those
characters by typing it with the shift key. That makes it a different
character as far as keyboard translation is concerned, but it has the
same usual meaning.

See [Keymaps for Translating Sequences of Events](#Translation-Keymaps), for mechanisms that translate event sequences
at the level of `read-key-sequence`. If you need to translate
input events that are not characters (i.e., `characterp` returns
`nil` for them), you must use the event translation mechanism
described there.

Command: **key-translate-remove** from [¶](#index-key_002dtranslate_002dremove)
:   This command prompts for a key from and its translation, and
    removes the translation from the translation table. When calling from
    Lisp, specify just the key from, without its translation.

---

Next: [Quoted Character Input](#Quoted-Character-Input), Previous: [Modifying and Translating Input Events](#Event-Mod), Up: [Reading Input](#Reading-Input)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.8.4 Invoking the Input Method [¶](#Invoking-the-Input-Method-1)

The event-reading functions invoke the current input method, if any
(see [Input Methods](#Input-Methods)). If the value of `input-method-function`
is non-`nil`, it should be a function; when `read-event` reads
a printing character (including `SPC`) with no modifier bits, it
calls that function, passing the character as an argument.

Variable: **input-method-function** [¶](#index-input_002dmethod_002dfunction)
:   If this is non-`nil`, its value specifies the current input method
    function.

    **Warning:** don’t bind this variable with `let`. It is often
    buffer-local, and if you bind it around reading input (which is exactly
    when you *would* bind it), switching buffers asynchronously while
    Emacs is waiting will cause the value to be restored in the wrong
    buffer.

The input method function should return a list of events which should
be used as input. (If the list is `nil`, that means there is no
input, so `read-event` waits for another event.) These events are
processed before the events in `unread-command-events`
(see [Miscellaneous Event Input Features](#Event-Input-Misc)). Events
returned by the input method function are not passed to the input method
function again, even if they are printing characters with no modifier
bits.

If the input method function calls `read-event` or
`read-key-sequence`, it should bind `input-method-function` to
`nil` first, to prevent recursion.

The input method function is not called when reading the second and
subsequent events of a key sequence. Thus, these characters are not
subject to input method processing. The input method function should
test the values of `overriding-local-map` and
`overriding-terminal-local-map`; if either of these variables is
non-`nil`, the input method should put its argument into a list and
return that list with no further processing.

---

Next: [Miscellaneous Event Input Features](#Event-Input-Misc), Previous: [Invoking the Input Method](#Invoking-the-Input-Method), Up: [Reading Input](#Reading-Input)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.8.5 Quoted Character Input [¶](#Quoted-Character-Input-1)

You can use the function `read-quoted-char` to ask the user to
specify a character, and allow the user to specify a control or meta
character conveniently, either literally or as an octal character code.
The command `quoted-insert` uses this function.

Function: **read-quoted-char** &optional prompt [¶](#index-read_002dquoted_002dchar)
:   This function is like `read-char`, except that if the first
    character read is an octal digit (0–7), it reads any number of octal
    digits (but stopping if a non-octal digit is found), and returns the
    character represented by that numeric character code. If the
    character that terminates the sequence of octal digits is `RET`,
    it is discarded. Any other terminating character is used as input
    after this function returns.

    Quitting is suppressed when the first character is read, so that the
    user can enter a `C-g`. See [Quitting](#Quitting).

    If prompt is supplied, it specifies a string for prompting the
    user. The prompt string is always displayed in the echo area, followed
    by a single ‘`-`’.

    In the following example, the user types in the octal number 177 (which
    is 127 in decimal).

    ```
    (read-quoted-char "What character")
    ```

    ```
    ---------- Echo Area ----------
    What character 1 7 7-
    ---------- Echo Area ----------

         ⇒ 127
    ```

---

Previous: [Quoted Character Input](#Quoted-Character-Input), Up: [Reading Input](#Reading-Input)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.8.6 Miscellaneous Event Input Features [¶](#Miscellaneous-Event-Input-Features)

This section describes how to peek ahead at events without using
them up, how to check for pending input, and how to discard pending
input. See also the function `read-passwd` (see [Reading a Password](#Reading-a-Password)).

Variable: **unread-command-events** [¶](#index-unread_002dcommand_002devents)
:   This variable holds a list of events waiting to be read as command
    input. The events are used in the order they appear in the list, and
    removed one by one as they are used.

    The variable is needed because in some cases a function reads an event
    and then decides not to use it. Storing the event in this variable
    causes it to be processed normally, by the command loop or by the
    functions to read command input.

    For example, the function that implements numeric prefix arguments reads
    any number of digits. When it finds a non-digit event, it must unread
    the event so that it can be read normally by the command loop.
    Likewise, incremental search uses this feature to unread events with no
    special meaning in a search, because these events should exit the search
    and then execute normally.

    The reliable and easy way to extract events from a key sequence so as
    to put them in `unread-command-events` is to use
    `listify-key-sequence` (see below).

    Normally you add events to the front of this list, so that the events
    most recently unread will be reread first.

    Events read from this list are not normally added to the current
    command’s key sequence (as returned by, e.g., `this-command-keys`),
    as the events will already have been added once as they were read for
    the first time. An element of the form `(t . event)`
    forces event to be added to the current command’s key sequence.

    Elements read from this list are normally recorded by the
    record-keeping features (see [Recording Input](#Recording-Input)) and while defining a
    keyboard macro (see [Keyboard Macros](#Keyboard-Macros)). However, an element of the
    form `(no-record . event)` causes event to be
    processed normally without recording it.

Function: **listify-key-sequence** key [¶](#index-listify_002dkey_002dsequence)
:   This function converts the string or vector key to a list of
    individual events, which you can put in `unread-command-events`.

Function: **input-pending-p** &optional check-timers [¶](#index-input_002dpending_002dp)
:   This function determines whether any command input is currently
    available to be read. It returns immediately, with value `t` if
    there is available input, `nil` otherwise. On rare occasions it
    may return `t` when no input is available.

    If the optional argument check-timers is non-`nil`, then if
    no input is available, Emacs runs any timers which are ready.
    See [Timers for Delayed Execution](#Timers).

Variable: **last-input-event** [¶](#index-last_002dinput_002devent)
:   This variable records the last terminal input event read, whether
    as part of a command or explicitly by a Lisp program.

    In the example below, the Lisp program reads the character `1`,
    ASCII code 49. It becomes the value of `last-input-event`,
    while `C-e` (we assume `C-x C-e` command is used to evaluate
    this expression) remains the value of `last-command-event`.

    ```
    (progn (print (read-char))
           (print last-command-event)
           last-input-event)
         -| 49
         -| 5
         ⇒ 49
    ```

Macro: **while-no-input** body… [¶](#index-while_002dno_002dinput)
:   This construct runs the body forms and returns the value of the
    last one—but only if no input arrives. If any input arrives during
    the execution of the body forms, it aborts them (working much
    like a quit). The `while-no-input` form returns `nil` if
    aborted by a real quit, and returns `t` if aborted by arrival of
    other input.

    If a part of body binds `inhibit-quit` to non-`nil`,
    arrival of input during those parts won’t cause an abort until
    the end of that part.

    If you want to be able to distinguish all possible values computed
    by body from both kinds of abort conditions, write the code
    like this:

    ```
    (while-no-input
      (list
        (progn . body)))
    ```

Variable: **while-no-input-ignore-events** [¶](#index-while_002dno_002dinput_002dignore_002devents)
:   This variable allow setting which special events `while-no-input`
    should ignore. It is a list of event symbols (see [Event Examples](#Event-Examples)).

Function: **discard-input** [¶](#index-discard_002dinput)
:   This function discards the contents of the terminal input buffer and
    cancels any keyboard macro that might be in the process of definition.
    It returns `nil`.

    In the following example, the user may type a number of characters right
    after starting the evaluation of the form. After the `sleep-for`
    finishes sleeping, `discard-input` discards any characters typed
    during the sleep.

    ```
    (progn (sleep-for 2)
           (discard-input))
         ⇒ nil
    ```

---

Next: [Waiting for Elapsed Time or Input](#Waiting), Previous: [Reading Input](#Reading-Input), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.9 Special Events [¶](#Special-Events-1)

Certain *special events* are handled at a very low level—as soon
as they are read. The `read-event` function processes these
events itself, and never returns them. Instead, it keeps waiting for
the first event that is not special and returns that one.

Special events do not echo, they are never grouped into key
sequences, and they never appear in the value of
`last-command-event` or `(this-command-keys)`. They do not
discard a numeric argument, they cannot be unread with
`unread-command-events`, they may not appear in a keyboard macro,
and they are not recorded in a keyboard macro while you are defining
one.

Special events do, however, appear in `last-input-event`
immediately after they are read, and this is the way for the event’s
definition to find the actual event.

The events types `iconify-frame`, `make-frame-visible`,
`delete-frame`, `drag-n-drop`, `language-change`, and
user signals like `sigusr1` are normally handled in this way.
The keymap which defines how to handle special events—and which
events are special—is in the variable `special-event-map`
(see [Controlling the Active Keymaps](#Controlling-Active-Maps)). See [Miscellaneous System Events](#Misc-Events), for more details
about these and other special events.

Function: **insert-special-event** event [¶](#index-insert_002dspecial_002devent)
:   This function inserts the special event into the input event
    queue. Only event types which are contained in the
    `special-event-map` keymap are accepted. As a result, the handler
    specified in the keymap is invoked.

    The function returns `nil`. Example:

    ```
    (defun my-event-handler (event)
      (interactive "e")
      (message "Event arrived: %S" event))
    ⇒ my-event-handler

    (keymap-set special-event-map "<sleep-event>" #'my-event-handler)
    ⇒ my-event-handler

    (insert-special-event '(sleep-event pre-sleep))
    ⇒ nil
    ⇒ "Event arrived: (sleep-event pre-sleep)"
    ```

---

Next: [Quitting](#Quitting), Previous: [Special Events](#Special-Events), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.10 Waiting for Elapsed Time or Input [¶](#Waiting-for-Elapsed-Time-or-Input)

The wait functions are designed to wait for a certain amount of time
to pass or until there is input. For example, you may wish to pause in
the middle of a computation to allow the user time to view the display.
`sit-for` pauses and updates the screen, and returns immediately if
input comes in, while `sleep-for` pauses without updating the
screen.

Function: **sit-for** seconds &optional nodisp [¶](#index-sit_002dfor)
:   This function performs redisplay (provided there is no pending input
    from the user), then waits seconds seconds, or until input is
    available. The usual purpose of `sit-for` is to give the user
    time to read text that you display. The value is `t` if
    `sit-for` waited the full time with no input arriving
    (see [Miscellaneous Event Input Features](#Event-Input-Misc)). Otherwise, the value is `nil`.

    The argument seconds need not be an integer. If it is floating
    point, `sit-for` waits for a fractional number of seconds.
    Some systems support only a whole number of seconds; on these systems,
    seconds is rounded down.

    The expression `(sit-for 0)` is equivalent to `(redisplay)`,
    i.e., it requests a redisplay, without any delay, if there is no pending input.
    See [Forcing Redisplay](#Forcing-Redisplay).

    If nodisp is non-`nil`, then `sit-for` does not
    redisplay, but it still returns as soon as input is available (or when
    the timeout elapses).

    In batch mode (see [Batch Mode](#Batch-Mode)), `sit-for` cannot be
    interrupted, even by input from the standard input descriptor. It is
    thus equivalent to `sleep-for`, which is described below.

Function: **sleep-for** seconds [¶](#index-sleep_002dfor)
:   This function simply pauses for seconds seconds without updating
    the display. It pays no attention to available input. It returns
    `nil`.

    The argument seconds need not be an integer. If it is floating
    point, `sleep-for` waits for a fractional number of seconds.

    It is also possible to call `sleep-for` with two arguments,
    as `(sleep-for seconds millisec)`,
    but that is considered obsolete and will be removed in the future.

    Use `sleep-for` when you wish to guarantee a delay.

See [Time of Day](#Time-of-Day), for functions to get the current time.

---

Next: [Prefix Command Arguments](#Prefix-Command-Arguments), Previous: [Waiting for Elapsed Time or Input](#Waiting), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.11 Quitting [¶](#Quitting-1)

Typing `C-g` while a Lisp function is running causes Emacs to
*quit* whatever it is doing. This means that control returns to the
innermost active command loop.

Typing `C-g` while the command loop is waiting for keyboard input
does not cause a quit; it acts as an ordinary input character. In the
simplest case, you cannot tell the difference, because `C-g`
normally runs the command `keyboard-quit`, whose effect is to quit.
However, when `C-g` follows a prefix key, they combine to form an
undefined key. The effect is to cancel the prefix key as well as any
prefix argument.

In the minibuffer, `C-g` has a different definition: it aborts out
of the minibuffer. This means, in effect, that it exits the minibuffer
and then quits. (Simply quitting would return to the command loop
*within* the minibuffer.) The reason why `C-g` does not quit
directly when the command reader is reading input is so that its meaning
can be redefined in the minibuffer in this way. `C-g` following a
prefix key is not redefined in the minibuffer, and it has its normal
effect of canceling the prefix key and prefix argument. This too
would not be possible if `C-g` always quit directly.

When `C-g` does directly quit, it does so by setting the variable
`quit-flag` to `t`. Emacs checks this variable at appropriate
times and quits if it is not `nil`. Setting `quit-flag`
non-`nil` in any way thus causes a quit.

At the level of C code, quitting cannot happen just anywhere; only at the
special places that check `quit-flag`. The reason for this is
that quitting at other places might leave an inconsistency in Emacs’s
internal state. Because quitting is delayed until a safe place, quitting
cannot make Emacs crash.

Certain functions such as `read-key-sequence` or
`read-quoted-char` prevent quitting entirely even though they wait
for input. Instead of quitting, `C-g` serves as the requested
input. In the case of `read-key-sequence`, this serves to bring
about the special behavior of `C-g` in the command loop. In the
case of `read-quoted-char`, this is so that `C-q` can be used
to quote a `C-g`.

You can prevent quitting for a portion of a Lisp function by binding
the variable `inhibit-quit` to a non-`nil` value. Then,
although `C-g` still sets `quit-flag` to `t` as usual, the
usual result of this—a quit—is prevented. Eventually,
`inhibit-quit` will become `nil` again, such as when its
binding is unwound at the end of a `let` form. At that time, if
`quit-flag` is still non-`nil`, the requested quit happens
immediately. This behavior is ideal when you wish to make sure that
quitting does not happen within a critical section of the program.

In some functions (such as `read-quoted-char`), `C-g` is
handled in a special way that does not involve quitting. This is done
by reading the input with `inhibit-quit` bound to `t`, and
setting `quit-flag` to `nil` before `inhibit-quit`
becomes `nil` again. This excerpt from the definition of
`read-quoted-char` shows how this is done; it also shows that
normal quitting is permitted after the first character of input.

```
(defun read-quoted-char (&optional prompt)
  "...documentation..."
  (let ((message-log-max nil) done (first t) (code 0) char)
    (while (not done)
      (let ((inhibit-quit first)
            ...)
        (and prompt (message "%s-" prompt))
        (setq char (read-event))
        (if inhibit-quit (setq quit-flag nil)))
      ...set the variable code...)
    code))
```

Variable: **quit-flag** [¶](#index-quit_002dflag)
:   If this variable is non-`nil`, then Emacs quits immediately, unless
    `inhibit-quit` is non-`nil`. Typing `C-g` ordinarily sets
    `quit-flag` non-`nil`, regardless of `inhibit-quit`.

Variable: **inhibit-quit** [¶](#index-inhibit_002dquit)
:   This variable determines whether Emacs should quit when `quit-flag`
    is set to a value other than `nil`. If `inhibit-quit` is
    non-`nil`, then `quit-flag` has no special effect.

Macro: **with-local-quit** body… [¶](#index-with_002dlocal_002dquit)
:   This macro executes body forms in sequence, but allows quitting, at
    least locally, within body even if `inhibit-quit` was
    non-`nil` outside this construct. It returns the value of the
    last form in body, unless exited by quitting, in which case
    it returns `nil`.

    If `inhibit-quit` is `nil` on entry to `with-local-quit`,
    it only executes the body, and setting `quit-flag` causes
    a normal quit. However, if `inhibit-quit` is non-`nil` so
    that ordinary quitting is delayed, a non-`nil` `quit-flag`
    triggers a special kind of local quit. This ends the execution of
    body and exits the `with-local-quit` body with
    `quit-flag` still non-`nil`, so that another (ordinary) quit
    will happen as soon as that is allowed. If `quit-flag` is
    already non-`nil` at the beginning of body, the local quit
    happens immediately and the body doesn’t execute at all.

    This macro is mainly useful in functions that can be called from
    timers, process filters, process sentinels, `pre-command-hook`,
    `post-command-hook`, and other places where `inhibit-quit` is
    normally bound to `t`.

Command: **keyboard-quit** [¶](#index-keyboard_002dquit)
:   This function signals the `quit` condition with `(signal 'quit
    nil)`. This is the same thing that quitting does. (See `signal`
    in [Errors](#Errors).)

To quit without aborting a keyboard macro definition or execution,
you can signal the `minibuffer-quit` condition. This has almost
the same effect as the `quit` condition except that the error
handling in the command loop handles it without exiting keyboard macro
definition or execution.

You can specify a character other than `C-g` to use for quitting.
See the function `set-input-mode` in [Input Modes](#Input-Modes).

---

Next: [Recursive Editing](#Recursive-Editing), Previous: [Quitting](#Quitting), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.12 Prefix Command Arguments [¶](#Prefix-Command-Arguments-1)

Most Emacs commands can use a *prefix argument*, a number
specified before the command itself. (Don’t confuse prefix arguments
with prefix keys.) The prefix argument is at all times represented by a
value, which may be `nil`, meaning there is currently no prefix
argument. Each command may use the prefix argument or ignore it.

There are two representations of the prefix argument: *raw* and
*numeric*. The editor command loop uses the raw representation
internally, and so do the Lisp variables that store the information, but
commands can request either representation.

Here are the possible values of a raw prefix argument:

* `nil`, meaning there is no prefix argument. Its numeric value is
  1, but numerous commands make a distinction between `nil` and the
  integer 1.
* An integer, which stands for itself.
* A list of one element, which is an integer. This form of prefix
  argument results from one or a succession of `C-u`s with no
  digits. The numeric value is the integer in the list, but some
  commands make a distinction between such a list and an integer alone.
* The symbol `-`. This indicates that `M--` or `C-u -` was
  typed, without following digits. The equivalent numeric value is
  −1, but some commands make a distinction between the integer
  −1 and the symbol `-`.

We illustrate these possibilities by calling the following function with
various prefixes:

```
(defun display-prefix (arg)
  "Display the value of the raw prefix arg."
  (interactive "P")
  (message "%s" arg))
```

Here are the results of calling `display-prefix` with various
raw prefix arguments:

```
        M-x display-prefix  -| nil

C-u     M-x display-prefix  -| (4)

C-u C-u M-x display-prefix  -| (16)

C-u 3   M-x display-prefix  -| 3

M-3     M-x display-prefix  -| 3      ; (Same as C-u 3.)

C-u -   M-x display-prefix  -| -

M--     M-x display-prefix  -| -      ; (Same as C-u -.)

C-u - 7 M-x display-prefix  -| -7

M-- 7   M-x display-prefix  -| -7     ; (Same as C-u -7.)
```

Emacs uses two variables to store the prefix argument:
`prefix-arg` and `current-prefix-arg`. Commands such as
`universal-argument` that set up prefix arguments for other
commands store them in `prefix-arg`. In contrast,
`current-prefix-arg` conveys the prefix argument to the current
command, so setting it has no effect on the prefix arguments for future
commands.

Normally, commands specify which representation to use for the prefix
argument, either numeric or raw, in the `interactive` specification.
(See [Using `interactive`](#Using-Interactive).) Alternatively, functions may look at the
value of the prefix argument directly in the variable
`current-prefix-arg`, but this is less clean.

Function: **prefix-numeric-value** arg [¶](#index-prefix_002dnumeric_002dvalue)
:   This function returns the numeric meaning of a valid raw prefix argument
    value, arg. The argument may be a symbol, a number, or a list.
    If it is `nil`, the value 1 is returned; if it is `-`, the
    value −1 is returned; if it is a number, that number is returned;
    if it is a list, the CAR of that list (which should be a number) is
    returned.

Variable: **current-prefix-arg** [¶](#index-current_002dprefix_002darg)
:   This variable holds the raw prefix argument for the *current*
    command. Commands may examine it directly, but the usual method for
    accessing it is with `(interactive "P")`.

Variable: **prefix-arg** [¶](#index-prefix_002darg)
:   The value of this variable is the raw prefix argument for the
    *next* editing command. Commands such as `universal-argument`
    that specify prefix arguments for the following command work by setting
    this variable.

Variable: **last-prefix-arg** [¶](#index-last_002dprefix_002darg)
:   The raw prefix argument value used by the previous command.

The following commands exist to set up prefix arguments for the
following command. Do not call them for any other reason.

Command: **universal-argument** [¶](#index-universal_002dargument)
:   This command reads input and specifies a prefix argument for the
    following command. Don’t call this command yourself unless you know
    what you are doing.

Command: **digit-argument** arg [¶](#index-digit_002dargument)
:   This command adds to the prefix argument for the following command. The
    argument arg is the raw prefix argument as it was before this
    command; it is used to compute the updated prefix argument. Don’t call
    this command yourself unless you know what you are doing.

Command: **negative-argument** arg [¶](#index-negative_002dargument)
:   This command adds to the numeric argument for the next command. The
    argument arg is the raw prefix argument as it was before this
    command; its value is negated to form the new prefix argument. Don’t
    call this command yourself unless you know what you are doing.

---

Next: [Disabling Commands](#Disabling-Commands), Previous: [Prefix Command Arguments](#Prefix-Command-Arguments), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.13 Recursive Editing [¶](#Recursive-Editing-1)

The Emacs command loop is entered automatically when Emacs starts up.
This top-level invocation of the command loop never exits; it keeps
running as long as Emacs does. Lisp programs can also invoke the
command loop. Since this makes more than one activation of the command
loop, we call it *recursive editing*. A recursive editing level has
the effect of suspending whatever command invoked it and permitting the
user to do arbitrary editing before resuming that command.

The commands available during recursive editing are the same ones
available in the top-level editing loop and defined in the keymaps.
Only a few special commands exit the recursive editing level; the others
return to the recursive editing level when they finish. (The special
commands for exiting are always available, but they do nothing when
recursive editing is not in progress.)

All command loops, including recursive ones, set up all-purpose error
handlers so that an error in a command run from the command loop will
not exit the loop.

Minibuffer input is a special kind of recursive editing. It has a few
special wrinkles, such as enabling display of the minibuffer and the
minibuffer window, but fewer than you might suppose. Certain keys
behave differently in the minibuffer, but that is only because of the
minibuffer’s local map; if you switch windows, you get the usual Emacs
commands.

To invoke a recursive editing level, call the function
`recursive-edit`. This function contains the command loop; it
also contains a call to `catch` with tag `exit`, which makes
it possible to exit the recursive editing level by throwing to
`exit` (see [Explicit Nonlocal Exits: `catch` and `throw`](#Catch-and-Throw)). Throwing a `t` value
causes `recursive-edit` to quit, so that control returns to the
command loop one level up. This is called *aborting*, and is done
by `C-]` (`abort-recursive-edit`). Similarly, you can throw
a string value to make `recursive-edit` signal an error, printing
this string as the message. If you throw a function,
`recursive-edit` will call it without arguments before returning.
Throwing any other value, will make `recursive-edit` return
normally to the function that called it. The command `C-M-c`
(`exit-recursive-edit`) does this.

Most applications should not use recursive editing, except as part of
using the minibuffer. Usually it is more convenient for the user if you
change the major mode of the current buffer temporarily to a special
major mode, which should have a command to go back to the previous mode.
(The `e` command in Rmail uses this technique.) Or, if you wish to
give the user different text to edit recursively, create and select
a new buffer in a special mode. In this mode, define a command to
complete the processing and go back to the previous buffer. (The
`m` command in Rmail does this.)

Recursive edits are useful in debugging. You can insert a call to
`debug` into a function definition as a sort of breakpoint, so that
you can look around when the function gets there. `debug` invokes
a recursive edit but also provides the other features of the debugger.

Recursive editing levels are also used when you type `C-r` in
`query-replace` or use `C-x q` (`kbd-macro-query`).

Command: **recursive-edit** [¶](#index-recursive_002dedit)
:   This function invokes the editor command loop. It is called
    automatically by the initialization of Emacs, to let the user begin
    editing. When called from a Lisp program, it enters a recursive editing
    level.

    If the current buffer is not the same as the selected window’s buffer,
    `recursive-edit` saves and restores the current buffer. Otherwise,
    if you switch buffers, the buffer you switched to is current after
    `recursive-edit` returns.

    In the following example, the function `simple-rec` first
    advances point one word, then enters a recursive edit, printing out a
    message in the echo area. The user can then do any editing desired, and
    then type `C-M-c` to exit and continue executing `simple-rec`.

    ```
    (defun simple-rec ()
      (forward-word 1)
      (message "Recursive edit in progress")
      (recursive-edit)
      (forward-word 1))
         ⇒ simple-rec
    (simple-rec)
         ⇒ nil
    ```

Command: **exit-recursive-edit** [¶](#index-exit_002drecursive_002dedit)
:   This function exits from the innermost recursive edit (including
    minibuffer input). Its definition is effectively `(throw 'exit
    nil)`.

Command: **abort-recursive-edit** [¶](#index-abort_002drecursive_002dedit)
:   This function aborts the command that requested the innermost recursive
    edit (including minibuffer input), by signaling `quit`
    after exiting the recursive edit. Its definition is effectively
    `(throw 'exit t)`. See [Quitting](#Quitting).

Command: **top-level** [¶](#index-top_002dlevel)
:   This function exits all recursive editing levels; it does not return a
    value, as it jumps completely out of any computation directly back to
    the main command loop.

Function: **recursion-depth** [¶](#index-recursion_002ddepth)
:   This function returns the current depth of recursive edits. When no
    recursive edit is active, it returns 0.

---

Next: [Command History](#Command-History), Previous: [Recursive Editing](#Recursive-Editing), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.14 Disabling Commands [¶](#Disabling-Commands-1)

*Disabling a command* marks the command as requiring user
confirmation before it can be executed. Disabling is used for commands
which might be confusing to beginning users, to prevent them from using
the commands by accident.

The low-level mechanism for disabling a command is to put a
non-`nil` `disabled` property on the Lisp symbol for the
command. These properties are normally set up by the user’s
init file (see [The Init File](#Init-File)) with Lisp expressions such as this:

```
(put 'upcase-region 'disabled t)
```

For a few commands, these properties are present by default (you can
remove them in your init file if you wish).

If the value of the `disabled` property is a string, the message
saying the command is disabled includes that string. For example:

```
(put 'delete-region 'disabled
     "Text deleted this way cannot be yanked back!\n")
```

See [Disabling](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Disabling) in The GNU Emacs Manual, for the details on
what happens when a disabled command is invoked interactively.
Disabling a command has no effect on calling it as a function from Lisp
programs.

The value of the `disabled` property can also be a list where
the first element is the symbol `query`. In that case, the user
will be queried whether to execute the command. The second element in
the list should be `nil` or non-`nil` to say whether to use
`y-or-n-p` or `yes-or-no-p`, respectively, and the third
element is the question to use. The `command-query` convenience
function should be used to enable querying for a command.

Command: **enable-command** command [¶](#index-enable_002dcommand)
:   Allow command (a symbol) to be executed without special
    confirmation from now on, and alter the user’s init file (see [The Init File](#Init-File)) so that this will apply to future sessions.

Command: **disable-command** command [¶](#index-disable_002dcommand)
:   Require special confirmation to execute command from now on, and
    alter the user’s init file so that this will apply to future sessions.

Variable: **disabled-command-function** [¶](#index-disabled_002dcommand_002dfunction)
:   The value of this variable should be a function. When the user
    invokes a disabled command interactively, this function is called
    instead of the disabled command. It can use `this-command-keys`
    to determine what the user typed to run the command, and thus find the
    command itself.

    The value may also be `nil`. Then all commands work normally,
    even disabled ones.

    By default, the value is a function that asks the user whether to
    proceed.

---

Next: [Keyboard Macros](#Keyboard-Macros), Previous: [Disabling Commands](#Disabling-Commands), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.15 Command History [¶](#Command-History-1)

The command loop keeps a history of the complex commands that have
been executed, to make it convenient to repeat these commands. A
*complex command* is one for which the interactive argument reading
uses the minibuffer. This includes any `M-x` command, any
`M-:` command, and any command whose `interactive`
specification reads an argument from the minibuffer. Explicit use of
the minibuffer during the execution of the command itself does not cause
the command to be considered complex.

Variable: **command-history** [¶](#index-command_002dhistory)
:   This variable’s value is a list of recent complex commands, each
    represented as a form to evaluate. It continues to accumulate all
    complex commands for the duration of the editing session, but when it
    reaches the maximum size (see [Minibuffer History](#Minibuffer-History)), the oldest
    elements are deleted as new ones are added.

    ```
    command-history
    ⇒ ((switch-to-buffer "chistory.texi")
        (describe-key "^X^[")
        (visit-tags-table "~/emacs/src/")
        (find-tag "repeat-complex-command"))
    ```

This history list is actually a special case of minibuffer history
(see [Minibuffer History](#Minibuffer-History)), with one special twist: the elements are
expressions rather than strings.

There are a number of commands devoted to the editing and recall of
previous commands. The commands `repeat-complex-command`, and
`list-command-history` are described in the user manual
(see [Repetition](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Repetition) in The GNU Emacs Manual). Within the
minibuffer, the usual minibuffer history commands are available.

---

Previous: [Command History](#Command-History), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.16 Keyboard Macros [¶](#Keyboard-Macros-1)

A *keyboard macro* is a canned sequence of input events that can
be considered a command and made the definition of a key. The Lisp
representation of a keyboard macro is a string or vector containing the
events. Don’t confuse keyboard macros with Lisp macros
(see [Macros](#Macros)).

Function: **execute-kbd-macro** kbdmacro &optional count loopfunc [¶](#index-execute_002dkbd_002dmacro)
:   This function executes kbdmacro as a sequence of events. If
    kbdmacro is a string or vector, then the events in it are executed
    exactly as if they had been input by the user. The sequence is
    *not* expected to be a single key sequence; normally a keyboard
    macro definition consists of several key sequences concatenated.

    If kbdmacro is a symbol, then its function definition is used in
    place of kbdmacro. If that is another symbol, this process repeats.
    Eventually the result should be a string or vector. If the result is
    not a symbol, string, or vector, an error is signaled.

    The argument count is a repeat count; kbdmacro is executed that
    many times. If count is omitted or `nil`, kbdmacro is
    executed once. If it is 0, kbdmacro is executed over and over until it
    encounters an error or a failing search.

    If loopfunc is non-`nil`, it is a function that is called,
    without arguments, prior to each iteration of the macro. If
    loopfunc returns `nil`, then this stops execution of the macro.

    See [Reading One Event](#Reading-One-Event), for an example of using `execute-kbd-macro`.

Variable: **executing-kbd-macro** [¶](#index-executing_002dkbd_002dmacro)
:   This variable contains the string or vector that defines the keyboard
    macro that is currently executing. It is `nil` if no macro is
    currently executing. A command can test this variable so as to behave
    differently when run from an executing macro. Do not set this variable
    yourself.

Variable: **defining-kbd-macro** [¶](#index-defining_002dkbd_002dmacro)
:   This variable is non-`nil` if and only if a keyboard macro is
    being defined. A command can test this variable so as to behave
    differently while a macro is being defined. The value is
    `append` while appending to the definition of an existing macro.
    The commands `start-kbd-macro`, `kmacro-start-macro` and
    `end-kbd-macro` set this variable—do not set it yourself.

    The variable is always local to the current terminal and cannot be
    buffer-local. See [Multiple Terminals](#Multiple-Terminals).

Variable: **last-kbd-macro** [¶](#index-last_002dkbd_002dmacro)
:   This variable is the definition of the most recently defined keyboard
    macro. Its value is a string or vector, or `nil`.

    The variable is always local to the current terminal and cannot be
    buffer-local. See [Multiple Terminals](#Multiple-Terminals).

Variable: **kbd-macro-termination-hook** [¶](#index-kbd_002dmacro_002dtermination_002dhook)
:   This normal hook is run when a keyboard macro terminates, regardless
    of what caused it to terminate (reaching the macro end or an error
    which ended the macro prematurely).

---

Next: [Major and Minor Modes](#Modes), Previous: [Command Loop](#Command-Loop), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 23 Keymaps [¶](#Keymaps-1)

The command bindings of input events are recorded in data structures
called *keymaps*. Each entry in a keymap associates (or
*binds*) an individual event type, either to another keymap or to
a command. When an event type is bound to a keymap, that keymap is
used to look up the next input event; this continues until a command
is found. The whole process is called *key lookup*.

* [Keymap Basics](#Keymap-Basics)
* [Changing Key Bindings](#Changing-Key-Bindings)
* [Format of Keymaps](#Format-of-Keymaps)
* [Creating Keymaps](#Creating-Keymaps)
* [Inheritance and Keymaps](#Inheritance-and-Keymaps)
* [Prefix Keys](#Prefix-Keys)
* [Active Keymaps](#Active-Keymaps)
* [Searching the Active Keymaps](#Searching-Keymaps)
* [Controlling the Active Keymaps](#Controlling-Active-Maps)
* [Key Lookup](#Key-Lookup)
* [Functions for Key Lookup](#Functions-for-Key-Lookup)
* [Key Sequences](#Key-Sequences)
* [Low-Level Key Binding](#Low_002dLevel-Key-Binding)
* [Remapping Commands](#Remapping-Commands)
* [Keymaps for Translating Sequences of Events](#Translation-Keymaps)
* [Commands for Binding Keys](#Key-Binding-Commands)
* [Scanning Keymaps](#Scanning-Keymaps)
* [Menu Keymaps](#Menu-Keymaps)

---

Next: [Changing Key Bindings](#Changing-Key-Bindings), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.1 Keymap Basics [¶](#Keymap-Basics-1)

A keymap is a Lisp data structure that specifies *key bindings*
for various key sequences.

A single keymap directly specifies definitions for individual
events. When a key sequence consists of a single event, its binding
in a keymap is the keymap’s definition for that event. The binding of
a longer key sequence is found by an iterative process: first find the
definition of the first event (which must itself be a keymap); then
find the second event’s definition in that keymap, and so on until all
the events in the key sequence have been processed.

If the binding of a key sequence is a keymap, we call the key sequence
a *prefix key*. Otherwise, we call it a *complete key* (because
no more events can be added to it). If the binding is `nil`,
we call the key *undefined*. Examples of prefix keys are `C-c`,
`C-x`, and `C-x 4`. Examples of defined complete keys are
`X`, `RET`, and `C-x 4 C-f`. Examples of undefined complete
keys are `C-x C-g`, and `C-c 3`. See [Prefix Keys](#Prefix-Keys), for more
details.

The rule for finding the binding of a key sequence assumes that the
intermediate bindings (found for the events before the last) are all
keymaps; if this is not so, the sequence of events does not form a
unit—it is not really one key sequence. In other words, removing one
or more events from the end of any valid key sequence must always yield
a prefix key. For example, `C-f C-n` is not a key sequence;
`C-f` is not a prefix key, so a longer sequence starting with
`C-f` cannot be a key sequence.

The set of possible multi-event key sequences depends on the bindings
for prefix keys; therefore, it can be different for different keymaps,
and can change when bindings are changed. However, a one-event sequence
is always a key sequence, because it does not depend on any prefix keys
for its well-formedness.

At any time, several primary keymaps are *active*—that is, in
use for finding key bindings. These are the *global map*, which is
shared by all buffers; the *local keymap*, which is usually
associated with a specific major mode; and zero or more *minor mode
keymaps*, which belong to currently enabled minor modes. (Not all minor
modes have keymaps.) The local keymap bindings shadow (i.e., take
precedence over) the corresponding global bindings. The minor mode
keymaps shadow both local and global keymaps. See [Active Keymaps](#Active-Keymaps),
for details.

---

Next: [Format of Keymaps](#Format-of-Keymaps), Previous: [Keymap Basics](#Keymap-Basics), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.2 Changing Key Bindings [¶](#Changing-Key-Bindings-1)

The way to rebind a key is to change its entry in a keymap. If you
change a binding in the global keymap, the change is effective in all
buffers (though it has no direct effect in buffers that shadow the
global binding with a local one). If you change the current buffer’s
local map, that usually affects all buffers using the same major mode.
The `keymap-global-set` and `keymap-local-set` functions are
convenient interfaces for these operations (see [Commands for Binding Keys](#Key-Binding-Commands)). You can also use `keymap-set`, a more general
function; then you must explicitly specify the map to change.

When choosing the key sequences for Lisp programs to rebind, please
follow the Emacs conventions for use of various keys (see [Key Binding Conventions](#Key-Binding-Conventions)).

The functions below signal an error if keymap is not a keymap,
or if key is not a valid key.

key is a string representing a single key or a series of key
strokes, and must satisfy `key-valid-p`. Key strokes are
separated by a single space character.

Each key stroke is either a single character, or the name of an
event, surrounded by angle brackets. In addition, any key stroke
may be preceded by one or more modifier keys. Finally, a limited
number of characters have a special shorthand syntax. Here’s some
example key sequences:

`f`
:   The key `f`.

`S o m`
:   A three key sequence of the keys `S`, `o` and `m`.

`C-c o`
:   A two key sequence of the keys `c` with the control modifier and
    then the key `o`

`H-LEFT`
:   The key named `LEFT` with the hyper modifier.

`M-RET`
:   The `RET` or `Return` key with a meta modifier.

`C-M-SPC`
:   The `SPC` key with both the control and meta modifiers.

The only keys that have a special shorthand syntax are `NUL`,
`RET`, `TAB`, `LFD`, `ESC`, `SPC` and `DEL`.

The modifiers have to be specified in alphabetical order:
‘`A-C-H-M-S-s`’, which is ‘`Alt-Control-Hyper-Meta-Shift-super`’.

Function: **keymap-set** keymap key binding [¶](#index-keymap_002dset-1)
:   This function sets the binding for key in keymap. The
    argument key is a string that must satisfy `key-valid-p`
    (see [Key Sequences](#Key-Sequences)). The argument binding can be any Lisp
    object, but only certain types are meaningful. (For a list of
    meaningful types, see [Key Lookup](#Key-Lookup).) The value returned by
    `keymap-set` is binding.

    Every prefix of key must be a prefix key (i.e., bound to a keymap)
    or undefined; otherwise an error is signaled. If some prefix of
    key is undefined, then `keymap-set` defines it as a prefix
    key so that the rest of key can be defined as specified.

    If there was previously no binding for key in keymap, the
    new binding is added at the beginning of keymap. The order of
    bindings in a keymap makes no difference for keyboard input, but it
    does matter for menu keymaps (see [Menu Keymaps](#Menu-Keymaps)).

Function: **keymap-unset** keymap key &optional remove [¶](#index-keymap_002dunset-1)
:   This function is the inverse of `keymap-set`, it unsets the
    binding for key in keymap, which is the same as setting
    the binding to `nil`. In order to instead remove the binding
    completely, specify remove as non-`nil`. This only makes a
    difference if keymap has a parent keymap: if you just unset a key
    in a child map, it will still shadow the same key in the parent
    keymap; using remove instead will allow the key in the parent keymap
    to be used.

Note: using `keymap-unset` with remove non-`nil` is
intended for users to put in their init file; Emacs packages should
avoid using it if possible, since they have complete control over
their own keymaps anyway, and they should not be altering other
packages’ keymaps.

This example creates a sparse keymap and makes a number of
bindings in it:

```
(setq map (make-sparse-keymap))
    ⇒ (keymap)
```

```
(keymap-set map "C-f" 'forward-char)
    ⇒ forward-char
```

```
map
    ⇒ (keymap (6 . forward-char))
```

```

```

```
;; Build sparse submap for C-x and bind f in that.
(keymap-set map "C-x f" 'forward-word)
    ⇒ forward-word
```

```
map
⇒ (keymap
    (24 keymap                ; C-x
        (102 . forward-word)) ;      f
    (6 . forward-char))       ; C-f
```

```

```

```
;; Bind C-p to the ctl-x-map.
(keymap-set map "C-p" ctl-x-map)
;; ctl-x-map
⇒ [nil ... find-file ... backward-kill-sentence]
```

```

```

```
;; Bind C-f to foo in the ctl-x-map.
(keymap-set map "C-p C-f" 'foo)
⇒ 'foo
```

```
map
⇒ (keymap     ; Note foo in ctl-x-map.
    (16 keymap [nil ... foo ... backward-kill-sentence])
    (24 keymap
        (102 . forward-word))
    (6 . forward-char))
```

Note that storing a new binding for `C-p C-f` actually works by
changing an entry in `ctl-x-map`, and this has the effect of
changing the bindings of both `C-p C-f` and `C-x C-f` in the
default global map.

`keymap-set` is the general work horse for defining a key in a
keymap. When writing modes, however, you frequently have to bind a
large number of keys at once, and using `keymap-set` on them all
can be tedious and error-prone. Instead you can use
`define-keymap`, which creates a keymap and binds a number of
keys. See [Creating Keymaps](#Creating-Keymaps), for details.

The function `substitute-key-definition` scans a keymap for
keys that have a certain binding and rebinds them with a different
binding. Another feature which is cleaner and can often produce the
same results is to remap one command into another (see [Remapping Commands](#Remapping-Commands)).

Function: **substitute-key-definition** olddef newdef keymap &optional oldmap [¶](#index-substitute_002dkey_002ddefinition)
:   This function replaces olddef with newdef for any keys in
    keymap that were bound to olddef. In other words,
    olddef is replaced with newdef wherever it appears. The
    function returns `nil`.

    For example, this redefines `C-x C-f`, if you do it in an Emacs with
    standard bindings:

    ```
    (substitute-key-definition
     'find-file 'find-file-read-only (current-global-map))
    ```

    If oldmap is non-`nil`, that changes the behavior of
    `substitute-key-definition`: the bindings in oldmap determine
    which keys to rebind. The rebindings still happen in keymap, not
    in oldmap. Thus, you can change one map under the control of the
    bindings in another. For example,

    ```
    (substitute-key-definition
      'delete-backward-char 'my-funny-delete
      my-map global-map)
    ```

    puts the special deletion command in `my-map` for whichever keys
    are globally bound to the standard deletion command.

    Here is an example showing a keymap before and after substitution:

    ```
    (setq map (list 'keymap
                    (cons ?1 olddef-1)
                    (cons ?2 olddef-2)
                    (cons ?3 olddef-1)))
    ⇒ (keymap (49 . olddef-1) (50 . olddef-2) (51 . olddef-1))
    ```

    ```

    ```

    ```
    (substitute-key-definition 'olddef-1 'newdef map)
    ⇒ nil
    ```

    ```
    map
    ⇒ (keymap (49 . newdef) (50 . olddef-2) (51 . newdef))
    ```

Function: **suppress-keymap** keymap &optional nodigits [¶](#index-suppress_002dkeymap)
:   This function changes the contents of the full keymap keymap by
    remapping `self-insert-command` to the command `undefined`
    (see [Remapping Commands](#Remapping-Commands)). This has the effect of undefining all
    printing characters, thus making ordinary insertion of text impossible.
    `suppress-keymap` returns `nil`.

    If nodigits is `nil`, then `suppress-keymap` defines
    digits to run `digit-argument`, and `-` to run
    `negative-argument`. Otherwise it makes them undefined like the
    rest of the printing characters.

    The `suppress-keymap` function does not make it impossible to
    modify a buffer, as it does not suppress commands such as `yank`
    and `quoted-insert`. To prevent any modification of a buffer, make
    it read-only (see [Read-Only Buffers](#Read-Only-Buffers)).

    Since this function modifies keymap, you would normally use it
    on a newly created keymap. Operating on an existing keymap
    that is used for some other purpose is likely to cause trouble; for
    example, suppressing `global-map` would make it impossible to use
    most of Emacs.

    This function can be used to initialize the local keymap of a major
    mode for which insertion of text is not desirable. But usually such a
    mode should be derived from `special-mode` (see [Basic Major Modes](#Basic-Major-Modes)); then its keymap will automatically inherit from
    `special-mode-map`, which is already suppressed. Here is how
    `special-mode-map` is defined:

    ```
    (defvar special-mode-map
      (let ((map (make-sparse-keymap)))
        (suppress-keymap map)
        (keymap-set map "q" 'quit-window)
        ...
        map))
    ```

---

Next: [Creating Keymaps](#Creating-Keymaps), Previous: [Changing Key Bindings](#Changing-Key-Bindings), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
