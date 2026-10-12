### AD.4.5 Changing Key Bindings Interactively

The way to redefine an Emacs key is to change its entry in a keymap.
You can change the global keymap, in which case the change is effective in
all major modes (except those that have their own overriding local
definitions for the same key). Or you can change the current buffer's
local map, which affects all buffers using the same major mode.

`M-x global-set-key RET key cmd RET`: Define key globally to run cmd. `M-x local-set-key RET key cmd RET`: Define key locally (in the major mode now in effect) to run cmd. `M-x global-unset-key RET key`: Make key undefined in the global map. `M-x local-unset-key RET key`: Make key undefined locally (in the major mode now in effect).

For example, suppose you like to execute commands in a subshell within
an Emacs buffer, instead of suspending Emacs and executing commands in
your login shell. Normally, `C-z` is bound to the function
`suspend-emacs` (when not using the X Window System), but you can
change `C-z` to invoke an interactive subshell within Emacs, by
binding it to `shell` as follows:

|  |  |
| --- | --- |
|  | ``` M-x global-set-key RET C-z shell RET ``` |

`global-set-key` reads the command name after the key. After you
press the key, a message like this appears so that you can confirm that
you are binding the key you want:

|  |  |
| --- | --- |
|  | ``` Set key C-z to command: ``` |

You can redefine function keys and mouse events in the same way; just
type the function key or click the mouse when it's time to specify the
key to rebind.

You can rebind a key that contains more than one event in the same
way. Emacs keeps reading the key to rebind until it is a complete key
(that is, not a prefix key). Thus, if you type `C-f` for
key, that's the end; the minibuffer is entered immediately to
read cmd. But if you type `C-x`, another character is read;
if that is `4`, another character is read, and so on. For
example,

|  |  |
| --- | --- |
|  | ``` M-x global-set-key RET C-x 4 $ spell-other-window RET ``` |

redefines `C-x 4 $` to run the (fictitious) command
`spell-other-window`.

The two-character keys consisting of `C-c` followed by a letter
are reserved for user customizations. Lisp programs are not supposed to
define these keys, so the bindings you make for them will be available
in all major modes and will never get in the way of anything.

You can remove the global definition of a key with
`global-unset-key`. This makes the key *undefined*; if you
type it, Emacs will just beep. Similarly, `local-unset-key` makes
a key undefined in the current major mode keymap, which makes the global
definition (or lack of one) come back into effect in that major mode.

If you have redefined (or undefined) a key and you subsequently wish
to retract the change, undefining the key will not do the job--you need
to redefine the key with its standard definition. To find the name of
the standard definition of a key, go to a Fundamental mode buffer and
use `C-h c`. The documentation of keys in this manual also lists
their command names.

If you want to prevent yourself from invoking a command by mistake, it
is better to disable the command than to undefine the key. A disabled
command is less work to invoke when you really want to.
See section [AD.4.11 Disabling Commands](emacs.html#SEC502).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC496) ] | [ [>](emacs.html#SEC498) ] | [ [<<](emacs.html#SEC498) ] | [ [Up](emacs.html#SEC491) ] | [ [>>](emacs.html#SEC503) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.4.6 Rebinding Keys in Your Init File

If you have a set of key bindings that you like to use all the time,
you can specify them in your `.emacs' file by using their Lisp
syntax. (See section [AD.7 The Init File, `~/.emacs'](emacs.html#SEC505).)

The simplest method for doing this works for ASCII characters and
Meta-modified ASCII characters only. This method uses a string to
represent the key sequence you want to rebind. For example, here's how
to bind `C-z` to `shell`:

|  |  |
| --- | --- |
|  | ``` (global-set-key "\C-z" 'shell) ``` |

This example uses a string constant containing one character, `C-z`.
The single-quote before the command name, `shell`, marks it as a
constant symbol rather than a variable. If you omit the quote, Emacs
would try to evaluate `shell` immediately as a variable. This
probably causes an error; it certainly isn't what you want.

Here is another example that binds a key sequence two characters long:

|  |  |
| --- | --- |
|  | ``` (global-set-key "\C-xl" 'make-symbolic-link) ``` |

To put `TAB`, `RET`, `ESC`, or `DEL` in the
string, you can use the Emacs Lisp escape sequences, `` `\t' ``,
`` `\r' ``, `` `\e' ``, and `` `\d' ``. Here is an example which binds
`C-x TAB`:

|  |  |
| --- | --- |
|  | ``` (global-set-key "\C-x\t" 'indent-rigidly) ``` |

These examples show how to write some other special ASCII characters
in strings for key bindings:

|  |  |
| --- | --- |
|  | ``` (global-set-key "\r" 'newline)               ;; RET (global-set-key "\d" 'delete-backward-char)  ;; DEL (global-set-key "\C-x\e\e" 'repeat-complex-command)  ;; ESC ``` |

When the key sequence includes function keys or mouse button events,
or non-ASCII characters such as `C-=` or `H-a`, you must use
the more general method of rebinding, which uses a vector to specify the
key sequence.

The way to write a vector in Emacs Lisp is with square brackets around
the vector elements. Use spaces to separate the elements. If an
element is a symbol, simply write the symbol's name--no other
delimiters or punctuation are needed. If a vector element is a
character, write it as a Lisp character constant: `` `?' `` followed by
the character as it would appear in a string.

Here are examples of using vectors to rebind `C-=` (a control
character not in ASCII), `C-M-=` (not in ASCII because `C-=`
is not), `H-a` (a Hyper character; ASCII doesn't have Hyper at
all), `F7` (a function key), and `C-Mouse-1` (a
keyboard-modified mouse button):

|  |  |
| --- | --- |
|  | ``` (global-set-key [?\C-=] 'make-symbolic-link) (global-set-key [?\M-\C-=] 'make-symbolic-link) (global-set-key [?\H-a] 'make-symbolic-link) (global-set-key [f7] 'make-symbolic-link) (global-set-key [C-mouse-1] 'make-symbolic-link) ``` |

You can use a vector for the simple cases too. Here's how to
rewrite the first three examples above, using vectors to bind
`C-z`, `C-x l`, and `C-x TAB`:

|  |  |
| --- | --- |
|  | ``` (global-set-key [?\C-z] 'shell) (global-set-key [?\C-x ?l] 'make-symbolic-link) (global-set-key [?\C-x ?\t] 'indent-rigidly) (global-set-key [?\r] 'newline) (global-set-key [?\d] 'delete-backward-char) (global-set-key [?\C-x ?\e ?\e] 'repeat-complex-command) ``` |

As you see, you represent a multi-character key sequence with a vector
by listing each of the characters within the square brackets that
delimit the vector.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC497) ] | [ [>](emacs.html#SEC499) ] | [ [<<](emacs.html#SEC499) ] | [ [Up](emacs.html#SEC491) ] | [ [>>](emacs.html#SEC503) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.4.7 Rebinding Function Keys

Key sequences can contain function keys as well as ordinary
characters. Just as Lisp characters (actually integers) represent
keyboard characters, Lisp symbols represent function keys. If the
function key has a word as its label, then that word is also the name of
the corresponding Lisp symbol. Here are the conventional Lisp names for
common function keys:

`left`, `up`, `right`, `down`: Cursor arrow keys. `begin`, `end`, `home`, `next`, `prior`: Other cursor repositioning keys. `select`, `print`, `execute`, `backtab`: `insert`, `undo`, `redo`, `clearline`: `insertline`, `deleteline`, `insertchar`, `deletechar`: Miscellaneous function keys. `f1`, `f2`, ... `f35`: Numbered function keys (across the top of the keyboard). `kp-add`, `kp-subtract`, `kp-multiply`, `kp-divide`: `kp-backtab`, `kp-space`, `kp-tab`, `kp-enter`: `kp-separator`, `kp-decimal`, `kp-equal`: Keypad keys (to the right of the regular keyboard), with names or punctuation. `kp-0`, `kp-1`, ... `kp-9`: Keypad keys with digits. `kp-f1`, `kp-f2`, `kp-f3`, `kp-f4`: Keypad PF keys.

These names are conventional, but some systems (especially when using
X) may use different names. To make certain what symbol is used for a
given function key on your terminal, type `C-h c` followed by that
key.

A key sequence which contains function key symbols (or anything but
ASCII characters) must be a vector rather than a string. The vector
syntax uses spaces between the elements, and square brackets around the
whole vector. Thus, to bind function key `` `f1' `` to the command
`rmail`, write the following:

|  |  |
| --- | --- |
|  | ``` (global-set-key [f1] 'rmail) ``` |

To bind the right-arrow key to the command `forward-char`, you can
use this expression:

|  |  |
| --- | --- |
|  | ``` (global-set-key [right] 'forward-char) ``` |

This uses the Lisp syntax for a vector containing the symbol
`right`. (This binding is present in Emacs by default.)

See section [AD.4.6 Rebinding Keys in Your Init File](emacs.html#SEC497), for more information about using vectors for
rebinding.

You can mix function keys and characters in a key sequence. This
example binds `C-x NEXT` to the command `forward-page`.

|  |  |
| --- | --- |
|  | ``` (global-set-key [?\C-x next] 'forward-page) ``` |

where `?\C-x` is the Lisp character constant for the character
`C-x`. The vector element `next` is a symbol and therefore
does not take a question mark.

You can use the modifier keys `CTRL`, `META`, `HYPER`,
`SUPER`, `ALT` and `SHIFT` with function keys. To represent
these modifiers, add the strings `` `C-' ``, `` `M-' ``, `` `H-' ``,
`` `s-' ``, `` `A-' `` and `` `S-' `` at the front of the symbol name.
Thus, here is how to make `Hyper-Meta-RIGHT` move forward a
word:

|  |  |
| --- | --- |
|  | ``` (global-set-key [H-M-right] 'forward-word) ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC498) ] | [ [>](emacs.html#SEC500) ] | [ [<<](emacs.html#SEC500) ] | [ [Up](emacs.html#SEC491) ] | [ [>>](emacs.html#SEC503) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.4.8 Named ASCII Control Characters

`TAB`, `RET`, `BS`, `LFD`, `ESC` and `DEL`
started out as names for certain ASCII control characters, used so often
that they have special keys of their own. Later, users found it
convenient to distinguish in Emacs between these keys and the "same"
control characters typed with the `CTRL` key.

Emacs distinguishes these two kinds of input, when the keyboard
reports these keys to Emacs. It treats the "special" keys as function
keys named `tab`, `return`, `backspace`, `linefeed`,
`escape`, and `delete`. These function keys translate
automatically into the corresponding ASCII characters *if* they
have no bindings of their own. As a result, neither users nor Lisp
programs need to pay attention to the distinction unless they care to.

If you do not want to distinguish between (for example) `TAB` and
`C-i`, make just one binding, for the ASCII character `TAB`
(octal code 011). If you do want to distinguish, make one binding for
this ASCII character, and another for the "function key" `tab`.

With an ordinary ASCII terminal, there is no way to distinguish
between `TAB` and `C-i` (and likewise for other such pairs),
because the terminal sends the same character in both cases.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC499) ] | [ [>](emacs.html#SEC501) ] | [ [<<](emacs.html#SEC503) ] | [ [Up](emacs.html#SEC491) ] | [ [>>](emacs.html#SEC503) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.4.9 Non-ASCII Characters on the Keyboard

If your keyboard has keys that send non-ASCII characters, such as
accented letters, rebinding these keys is a bit tricky. There are two
solutions you can use. One is to specify a keyboard coding system,
using `set-keyboard-coding-system` (see section [Q.9 Specifying a Coding System](emacs.html#SEC222)).
Then you can bind these keys in the usual way[(16)](emacs.html#FOOT16), like this:

|  |  |
| --- | --- |
|  | ``` (global-set-key [?char] 'some-function) ``` |

Type `C-q` followed by the key you want to bind, to insert char.

If you don't specify the keyboard coding system, that approach won't
work. Instead, you need to find out the actual code that the terminal
sends. The easiest way to do this in Emacs is to create an empty buffer
with `C-x b temp RET`, make it unibyte with `M-x
toggle-enable-multibyte-characters RET`, then type the key to
insert the character into this buffer.

Move point before the character, then type `C-x =`. This
displays a message in the minibuffer, showing the character code in
three ways, octal, decimal and hexadecimal, all within a set of
parentheses. Use the second of the three numbers, the decimal one,
inside the vector to bind:

|  |  |
| --- | --- |
|  | ``` (global-set-key [decimal-code] 'some-function) ``` |

If you bind 8-bit characters like this in your init file, you may find it
convenient to specify that it is unibyte. See section [Q.2 Enabling Multibyte Characters](emacs.html#SEC215).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC500) ] | [ [>](emacs.html#SEC502) ] | [ [<<](emacs.html#SEC493) ] | [ [Up](emacs.html#SEC491) ] | [ [>>](emacs.html#SEC503) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.4.10 Rebinding Mouse Buttons

Emacs uses Lisp symbols to designate mouse buttons, too. The ordinary
mouse events in Emacs are *click* events; these happen when you
press a button and release it without moving the mouse. You can also
get *drag* events, when you move the mouse while holding the button
down. Drag events happen when you finally let go of the button.

The symbols for basic click events are `mouse-1` for the leftmost
button, `mouse-2` for the next, and so on. Here is how you can
redefine the second mouse button to split the current window:

|  |  |
| --- | --- |
|  | ``` (global-set-key [mouse-2] 'split-window-vertically) ``` |

The symbols for drag events are similar, but have the prefix
`` `drag-' `` before the word `` `mouse' ``. For example, dragging the
first button generates a `drag-mouse-1` event.

You can also define bindings for events that occur when a mouse button
is pressed down. These events start with `` `down-' `` instead of
`` `drag-' ``. Such events are generated only if they have key bindings.
When you get a button-down event, a corresponding click or drag event
will always follow.

If you wish, you can distinguish single, double, and triple clicks. A
double click means clicking a mouse button twice in approximately the
same place. The first click generates an ordinary click event. The
second click, if it comes soon enough, generates a double-click event
instead. The event type for a double-click event starts with
`` `double-' ``: for example, `double-mouse-3`.

This means that you can give a special meaning to the second click at
the same place, but it must act on the assumption that the ordinary
single click definition has run when the first click was received.

This constrains what you can do with double clicks, but user interface
designers say that this constraint ought to be followed in any case. A
double click should do something similar to the single click, only
"more so." The command for the double-click event should perform the
extra work for the double click.

If a double-click event has no binding, it changes to the
corresponding single-click event. Thus, if you don't define a
particular double click specially, it executes the single-click command
twice.

Emacs also supports triple-click events whose names start with
`` `triple-' ``. Emacs does not distinguish quadruple clicks as event
types; clicks beyond the third generate additional triple-click events.
However, the full number of clicks is recorded in the event list, so you
can distinguish if you really want to. We don't recommend distinct
meanings for more than three clicks, but sometimes it is useful for
subsequent clicks to cycle through the same set of three meanings, so
that four clicks are equivalent to one click, five are equivalent to
two, and six are equivalent to three.

Emacs also records multiple presses in drag and button-down events.
For example, when you press a button twice, then move the mouse while
holding the button, Emacs gets a `` `double-drag-' `` event. And at the
moment when you press it down for the second time, Emacs gets a
`` `double-down-' `` event (which is ignored, like all button-down
events, if it has no binding).

The variable `double-click-time` specifies how much time can
elapse between clicks and still allow them to be grouped as a multiple
click. Its value is in units of milliseconds. If the value is
`nil`, double clicks are not detected at all. If the value is
`t`, then there is no time limit. The default is 500.

The variable `double-click-fuzz` specifies how much the mouse
can move between clicks still allow them to be grouped as a multiple
click. Its value is in units of pixels on windowed displays and in
units of 1/8 of a character cell on text-mode terminals; the default is
3.

The symbols for mouse events also indicate the status of the modifier
keys, with the usual prefixes `` `C-' ``, `` `M-' ``, `` `H-' ``,
`` `s-' ``, `` `A-' `` and `` `S-' ``. These always precede `` `double-' ``
or `` `triple-' ``, which always precede `` `drag-' `` or `` `down-' ``.

A frame includes areas that don't show text from the buffer, such as
the mode line and the scroll bar. You can tell whether a mouse button
comes from a special area of the screen by means of dummy "prefix
keys." For example, if you click the mouse in the mode line, you get
the prefix key `mode-line` before the ordinary mouse-button symbol.
Thus, here is how to define the command for clicking the first button in
a mode line to run `scroll-up`:

|  |  |
| --- | --- |
|  | ``` (global-set-key [mode-line mouse-1] 'scroll-up) ``` |

Here is the complete list of these dummy prefix keys and their
meanings:

`mode-line`: The mouse was in the mode line of a window. `vertical-line`: The mouse was in the vertical line separating side-by-side windows. (If you use scroll bars, they appear in place of these vertical lines.) `vertical-scroll-bar`: The mouse was in a vertical scroll bar. (This is the only kind of scroll bar Emacs currently supports.)

You can put more than one mouse button in a key sequence, but it isn't
usual to do so.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC501) ] | [ [>](emacs.html#SEC503) ] | [ [<<](emacs.html#SEC493) ] | [ [Up](emacs.html#SEC491) ] | [ [>>](emacs.html#SEC503) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.4.11 Disabling Commands

Disabling a command marks the command as requiring confirmation before it
can be executed. The purpose of disabling a command is to prevent
beginning users from executing it by accident and being confused.

An attempt to invoke a disabled command interactively in Emacs
displays a window containing the command's name, its documentation, and
some instructions on what to do immediately; then Emacs asks for input
saying whether to execute the command as requested, enable it and
execute it, or cancel. If you decide to enable the command, you are
asked whether to do this permanently or just for the current session.
(Enabling permanently works by automatically editing your `.emacs'
file.) You can also type `!` to enable *all* commands,
for the current session only.

The direct mechanism for disabling a command is to put a
non-`nil` `disabled` property on the Lisp symbol for the
command. Here is the Lisp program to do this:

|  |  |
| --- | --- |
|  | ``` (put 'delete-region 'disabled t) ``` |

If the value of the `disabled` property is a string, that string
is included in the message displayed when the command is used:

|  |  |
| --- | --- |
|  | ``` (put 'delete-region 'disabled      "It's better to use `kill-region' instead.\n") ``` |

You can make a command disabled either by editing the `.emacs'
file directly or with the command `M-x disable-command`, which edits
the `.emacs' file for you. Likewise, `M-x enable-command`
edits `.emacs' to enable a command permanently. See section [AD.7 The Init File, `~/.emacs'](emacs.html#SEC505).

Whether a command is disabled is independent of what key is used to
invoke it; disabling also applies if the command is invoked using
`M-x`. Disabling a command has no effect on calling it as a
function from Lisp programs.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC502) ] | [ [>](emacs.html#SEC504) ] | [ [<<](emacs.html#SEC504) ] | [ [Up](emacs.html#SEC475) ] | [ [>>](emacs.html#SEC510) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AD.5 Keyboard Translations

Some keyboards do not make it convenient to send all the special
characters that Emacs uses. The most common problem case is the
`DEL` character. Some keyboards provide no convenient way to type
this very important character--usually because they were designed to
expect the character `C-h` to be used for deletion. On these
keyboards, if you press the key normally used for deletion, Emacs handles
the `C-h` as a prefix character and offers you a list of help
options, which is not what you want.

You can work around this problem within Emacs by setting up keyboard
translations to turn `C-h` into `DEL` and `DEL` into
`C-h`, as follows:

|  |  |
| --- | --- |
|  | ``` ;; Translate C-h to DEL. (keyboard-translate ?\C-h ?\C-?)  ;; Translate DEL to C-h. (keyboard-translate ?\C-? ?\C-h) ``` |

Keyboard translations are not the same as key bindings in keymaps
(see section [AD.4.1 Keymaps](emacs.html#SEC492)). Emacs contains numerous keymaps that apply in
different situations, but there is only one set of keyboard
translations, and it applies to every character that Emacs reads from
the terminal. Keyboard translations take place at the lowest level of
input processing; the keys that are looked up in keymaps contain the
characters that result from keyboard translation.

On a window system, the keyboard key named `DELETE` is a function
key and is distinct from the ASCII character named `DEL`.
See section [AD.4.8 Named ASCII Control Characters](emacs.html#SEC499). Keyboard translations affect only ASCII
character input, not function keys; thus, the above example used on a
window system does not affect the `DELETE` key. However, the
translation above isn't necessary on window systems, because Emacs can
also distinguish between the `BACKSPACE` key and `C-h`; and it
normally treats `BACKSPACE` as `DEL`.

For full information about how to use keyboard translations, see
section `Translating Input' in The Emacs Lisp Reference Manual.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC503) ] | [ [>](emacs.html#SEC505) ] | [ [<<](emacs.html#SEC505) ] | [ [Up](emacs.html#SEC475) ] | [ [>>](emacs.html#SEC510) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AD.6 The Syntax Table

All the Emacs commands which parse words or balance parentheses are
controlled by the *syntax table*. The syntax table says which
characters are opening delimiters, which are parts of words, which are
string quotes, and so on. It does this by assigning each character to
one of fifteen-odd *syntax classes*. In some cases it specifies
some additional information also.

Each major mode has its own syntax table (though related major modes
sometimes share one syntax table) which it installs in each buffer
that uses the mode. The syntax table installed in the current buffer
is the one that all commands use, so we call it "the" syntax table.

To display a description of the contents of the current syntax
table, type `C-h s` (`describe-syntax`). The description of
each character includes both the string you would have to give to
`modify-syntax-entry` to set up that character's current syntax,
starting with the character which designates its syntax class, plus
some English text to explain its meaning.

A syntax table is actually a Lisp object, a char-table, whose
elements are cons cells. For full information on the syntax table,
see section `Syntax Tables' in The Emacs Lisp Reference Manual.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC504) ] | [ [>](emacs.html#SEC506) ] | [ [<<](emacs.html#SEC510) ] | [ [Up](emacs.html#SEC475) ] | [ [>>](emacs.html#SEC510) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AD.7 The Init File, `~/.emacs'

When Emacs is started, it normally loads a Lisp program from the file
`.emacs' or `.emacs.el' in your home directory. We call this
file your *init file* because it specifies how to initialize Emacs
for you. You can use the command line switch `` `-q' `` to prevent
loading your init file, and `` `-u' `` (or `` `--user' ``) to specify a
different user's init file (see section [C. Entering and Exiting Emacs](emacs.html#SEC17)).

There can also be a *default init file*, which is the library
named `default.el', found via the standard search path for
libraries. The Emacs distribution contains no such library; your site
may create one for local customizations. If this library exists, it is
loaded whenever you start Emacs (except when you specify `` `-q' ``).
But your init file, if any, is loaded first; if it sets
`inhibit-default-init` non-`nil`, then `default' is not
loaded.

Your site may also have a *site startup file*; this is named
`site-start.el', if it exists. Like `default.el', Emacs
finds this file via the standard search path for Lisp libraries.
Emacs loads this library before it loads your init file. To inhibit
loading of this library, use the option `` `-no-site-file' ``.
See section [AE.2 Initial Options](emacs.html#SEC530).

You can place `default.el' and `site-start.el' in any of
the directories which Emacs searches for Lisp libraries. The variable
`load-path` (see section [V.7 Libraries of Lisp Code for Emacs](emacs.html#SEC328)) specifies these directories.
Many sites put these files in the `site-lisp' subdirectory of the
Emacs installation directory, typically
`/usr/local/share/emacs/site-lisp'.

If you have a large amount of code in your `.emacs' file, you
should rename it to `~/.emacs.el', and byte-compile it. See section `Byte Compilation' in the Emacs Lisp Reference Manual,
for more information about compiling Emacs Lisp programs.

If you are going to write actual Emacs Lisp programs that go beyond
minor customization, you should read the Emacs Lisp Reference Manual.
See section `Emacs Lisp' in the Emacs Lisp Reference Manual.

> |  |  |  |
> | --- | --- | --- |
> | [AD.7.1 Init File Syntax](emacs.html#SEC506) |  | Syntax of constants in Emacs Lisp. |
> | [AD.7.2 Init File Examples](emacs.html#SEC507) |  | How to do some things with an init file. |
> | [AD.7.3 Terminal-specific Initialization](emacs.html#SEC508) |  | Each terminal type can have an init file. |
> | [AD.7.4 How Emacs Finds Your Init File](emacs.html#SEC509) |  | How Emacs finds the init file. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC505) ] | [ [>](emacs.html#SEC507) ] | [ [<<](emacs.html#SEC510) ] | [ [Up](emacs.html#SEC505) ] | [ [>>](emacs.html#SEC510) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.7.1 Init File Syntax

The `.emacs' file contains one or more Lisp function call
expressions. Each of these consists of a function name followed by
arguments, all surrounded by parentheses. For example, `(setq
fill-column 60)` calls the function `setq` to set the variable
`fill-column` (see section [T.5 Filling Text](emacs.html#SEC238)) to 60.

The second argument to `setq` is an expression for the new value of
the variable. This can be a constant, a variable, or a function call
expression. In `.emacs', constants are used most of the time. They can be:

Numbers:: Numbers are written in decimal, with an optional initial minus sign. Strings:: Lisp string syntax is the same as C string syntax with a few extra features. Use a double-quote character to begin and end a string constant. In a string, you can include newlines and special characters literally. But often it is cleaner to use backslash sequences for them: `` `\n' `` for newline, `` `\b' `` for backspace, `` `\r' `` for carriage return, `` `\t' `` for tab, `` `\f' `` for formfeed (control-L), `` `\e' `` for escape, `` `\\' `` for a backslash, `` `\"' `` for a double-quote, or `` `\ooo' `` for the character whose octal code is ooo. Backslash and double-quote are the only characters for which backslash sequences are mandatory. `` `\C-' `` can be used as a prefix for a control character, as in `` `\C-s' `` for ASCII control-S, and `` `\M-' `` can be used as a prefix for a Meta character, as in `` `\M-a' `` for `Meta-A` or `` `\M-\C-a' `` for `Control-Meta-A`. If you want to include non-ASCII characters in strings in your init file, you should consider putting a `` `-*-coding: coding-system-*-' `` tag on the first line which states the coding system used to save your `.emacs', as explained in [Q.8 Recognizing Coding Systems](emacs.html#SEC221). This is because the defaults for decoding non-ASCII text might not yet be set up by the time Emacs reads those parts of your init file which use such strings, possibly leading Emacs to decode those strings incorrectly. Characters:: Lisp character constant syntax consists of a `` `?' `` followed by either a character or an escape sequence starting with `` `\' ``. Examples: `?x`, `?\n`, `?\"`, `?\)`. Note that strings and characters are not interchangeable in Lisp; some contexts require one and some contexts require the other. See section [AD.4.9 Non-ASCII Characters on the Keyboard](emacs.html#SEC500), for information about binding commands to keys which send non-ASCII characters. True:: `t` stands for `true'. False:: `nil` stands for `false'. Other Lisp objects:: Write a single-quote (`'`) followed by the Lisp object you want.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC506) ] | [ [>](emacs.html#SEC508) ] | [ [<<](emacs.html#SEC508) ] | [ [Up](emacs.html#SEC505) ] | [ [>>](emacs.html#SEC510) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.7.2 Init File Examples

Here are some examples of doing certain commonly desired things with
Lisp expressions:

* Make `TAB` in C mode just insert a tab if point is in the middle of a
  line.

  |  |  |
  | --- | --- |
  |  | ``` (setq c-tab-always-indent nil) ``` |

  Here we have a variable whose value is normally `t` for `true'
  and the alternative is `nil` for `false'.

  * Make searches case sensitive by default (in all buffers that do not
    override this).

    |  |  |
    | --- | --- |
    |  | ``` (setq-default case-fold-search nil) ``` |

    This sets the default value, which is effective in all buffers that do
    not have local values for the variable. Setting `case-fold-search`
    with `setq` affects only the current buffer's local value, which
    is not what you probably want to do in an init file.

    * Specify your own email address, if Emacs can't figure it out correctly.

      |  |  |
      | --- | --- |
      |  | ``` (setq user-mail-address "coon@yoyodyne.com") ``` |

      Various Emacs packages that need your own email address use the value of
      `user-mail-address`.

      * Make Text mode the default mode for new buffers.

        |  |  |
        | --- | --- |
        |  | ``` (setq default-major-mode 'text-mode) ``` |

        Note that `text-mode` is used because it is the command for
        entering Text mode. The single-quote before it makes the symbol a
        constant; otherwise, `text-mode` would be treated as a variable
        name.

        * Set up defaults for the Latin-1 character set
          which supports most of the languages of Western Europe.

          |  |  |
          | --- | --- |
          |  | ``` (set-language-environment "Latin-1") ``` |

          * Turn on Auto Fill mode automatically in Text mode and related modes.

            |  |  |
            | --- | --- |
            |  | ``` (add-hook 'text-mode-hook   '(lambda () (auto-fill-mode 1))) ``` |

            This shows how to add a hook function to a normal hook variable
            (see section [AD.2.3 Hooks](emacs.html#SEC484)). The function we supply is a list starting with
            `lambda`, with a single-quote in front of it to make it a list
            constant rather than an expression.

            It's beyond the scope of this manual to explain Lisp functions, but for
            this example it is enough to know that the effect is to execute
            `(auto-fill-mode 1)` when Text mode is entered. You can replace
            that with any other expression that you like, or with several
            expressions in a row.

            Emacs comes with a function named `turn-on-auto-fill` whose
            definition is `(lambda () (auto-fill-mode 1))`. Thus, a simpler
            way to write the above example is as follows:

            |  |  |
            | --- | --- |
            |  | ``` (add-hook 'text-mode-hook 'turn-on-auto-fill) ``` |

            * Load the installed Lisp library named `foo' (actually a file
              `foo.elc' or `foo.el' in a standard Emacs directory).

              |  |  |
              | --- | --- |
              |  | ``` (load "foo") ``` |

              When the argument to `load` is a relative file name, not starting
              with `` `/' `` or `` `~' ``, `load` searches the directories in
              `load-path` (see section [V.7 Libraries of Lisp Code for Emacs](emacs.html#SEC328)).

              * Load the compiled Lisp file `foo.elc' from your home directory.

                |  |  |
                | --- | --- |
                |  | ``` (load "~/foo.elc") ``` |

                Here an absolute file name is used, so no searching is done.

                * Tell Emacs to find the definition for the function `myfunction`
                  by loading a Lisp library named `mypackage' (i.e. a file
                  `mypackage.elc' or `mypackage.el'):

                  |  |  |
                  | --- | --- |
                  |  | ``` (autoload 'myfunction "mypackage" "Do what I say." t) ``` |

                  Here the string `"Do what I say."` is the function's
                  documentation string. You specify it in the `autoload`
                  definition so it will be available for help commands even when the
                  package is not loaded. The last argument, `t`, indicates that
                  this function is interactive; that is, it can be invoked interactively
                  by typing `M-x myfunction RET` or by binding it to a key.
                  If the function is not interactive, omit the `t` or use
                  `nil`.

                  * Rebind the key `C-x l` to run the function `make-symbolic-link`.

                    |  |  |
                    | --- | --- |
                    |  | ``` (global-set-key "\C-xl" 'make-symbolic-link) ``` |

                    or

                    |  |  |
                    | --- | --- |
                    |  | ``` (define-key global-map "\C-xl" 'make-symbolic-link) ``` |

                    Note once again the single-quote used to refer to the symbol
                    `make-symbolic-link` instead of its value as a variable.

                    * Do the same thing for Lisp mode only.

                      |  |  |
                      | --- | --- |
                      |  | ``` (define-key lisp-mode-map "\C-xl" 'make-symbolic-link) ``` |

                      * Redefine all keys which now run `next-line` in Fundamental mode
                        so that they run `forward-line` instead.

                        |  |  |
                        | --- | --- |
                        |  | ``` (substitute-key-definition 'next-line 'forward-line                            global-map) ``` |

                        * Make `C-x C-v` undefined.

                          |  |  |
                          | --- | --- |
                          |  | ``` (global-unset-key "\C-x\C-v") ``` |

                          One reason to undefine a key is so that you can make it a prefix.
                          Simply defining `C-x C-v anything` will make `C-x C-v` a
                          prefix, but `C-x C-v` must first be freed of its usual non-prefix
                          definition.

                          * Make `` `$' `` have the syntax of punctuation in Text mode.
                            Note the use of a character constant for `` `$' ``.

                            |  |  |
                            | --- | --- |
                            |  | ``` (modify-syntax-entry ?\$ "." text-mode-syntax-table) ``` |

                            * Enable the use of the command `narrow-to-region` without confirmation.

                              |  |  |
                              | --- | --- |
                              |  | ``` (put 'narrow-to-region 'disabled nil) ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC507) ] | [ [>](emacs.html#SEC509) ] | [ [<<](emacs.html#SEC509) ] | [ [Up](emacs.html#SEC505) ] | [ [>>](emacs.html#SEC510) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.7.3 Terminal-specific Initialization

Each terminal type can have a Lisp library to be loaded into Emacs when
it is run on that type of terminal. For a terminal type named
termtype, the library is called `term/termtype' and it is
found by searching the directories `load-path` as usual and trying the
suffixes `` `.elc' `` and `` `.el' ``. Normally it appears in the
subdirectory `term' of the directory where most Emacs libraries are
kept.

The usual purpose of the terminal-specific library is to map the
escape sequences used by the terminal's function keys onto more
meaningful names, using `function-key-map`. See the file
`term/lk201.el' for an example of how this is done. Many function
keys are mapped automatically according to the information in the
Termcap data base; the terminal-specific library needs to map only the
function keys that Termcap does not specify.

When the terminal type contains a hyphen, only the part of the name
before the first hyphen is significant in choosing the library name.
Thus, terminal types `` `aaa-48' `` and `` `aaa-30-rv' `` both use
the library `term/aaa'. The code in the library can use
`(getenv "TERM")` to find the full terminal type name.

The library's name is constructed by concatenating the value of the
variable `term-file-prefix` and the terminal type. Your `.emacs'
file can prevent the loading of the terminal-specific library by setting
`term-file-prefix` to `nil`.

Emacs runs the hook `term-setup-hook` at the end of
initialization, after both your `.emacs' file and any
terminal-specific library have been read in. Add hook functions to this
hook if you wish to override part of any of the terminal-specific
libraries and to define initializations for terminals that do not have a
library. See section [AD.2.3 Hooks](emacs.html#SEC484).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC508) ] | [ [>](emacs.html#SEC510) ] | [ [<<](emacs.html#SEC510) ] | [ [Up](emacs.html#SEC505) ] | [ [>>](emacs.html#SEC510) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.7.4 How Emacs Finds Your Init File

Normally Emacs uses the environment variable `HOME` to find
`.emacs'; that's what `` `~' `` means in a file name. But if you
run Emacs from a shell started by `su`, Emacs tries to find your
own `.emacs', not that of the user you are currently pretending
to be. The idea is that you should get your own editor customizations
even if you are running as the super user.

More precisely, Emacs first determines which user's init file to use.
It gets the user name from the environment variables `LOGNAME` and
`USER`; if neither of those exists, it uses effective user-ID.
If that user name matches the real user-ID, then Emacs uses `HOME`;
otherwise, it looks up the home directory corresponding to that user
name in the system's data base of users.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC509) ] | [ [>](emacs.html#SEC511) ] | [ [<<](emacs.html#SEC475) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC511) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AD.8 Quitting and Aborting

`C-g`: `C-BREAK (MS-DOS only)`: Quit: cancel running or partially typed command. `C-]`: Abort innermost recursive editing level and cancel the command which invoked it (`abort-recursive-edit`). `ESC ESC ESC`: Either quit or abort, whichever makes sense (`keyboard-escape-quit`). `M-x top-level`: Abort all recursive editing levels that are currently executing. `C-x u`: Cancel a previously made change in the buffer contents (`undo`).

There are two ways of canceling commands which are not finished
executing: *quitting* with `C-g`, and *aborting* with
`C-]` or `M-x top-level`. Quitting cancels a partially typed
command or one which is already running. Aborting exits a recursive
editing level and cancels the command that invoked the recursive edit.
(See section [AC.26 Recursive Editing Levels](emacs.html#SEC466).)

Quitting with `C-g` is used for getting rid of a partially typed
command, or a numeric argument that you don't want. It also stops a
running command in the middle in a relatively safe way, so you can use
it if you accidentally give a command which takes a long time. In
particular, it is safe to quit out of killing; either your text will
*all* still be in the buffer, or it will *all* be in the kill
ring (or maybe both). Quitting an incremental search does special
things documented under searching; in general, it may take two
successive `C-g` characters to get out of a search
(see section [K.1 Incremental Search](emacs.html#SEC92)).

On MS-DOS, the character `C-BREAK` serves as a quit character
like `C-g`. The reason is that it is not feasible, on MS-DOS, to
recognize `C-g` while a command is running, between interactions
with the user. By contrast, it *is* feasible to recognize
`C-BREAK` at all times. See section [AH.1 Keyboard and Mouse on MS-DOS](emacs.html#SEC555).

`C-g` works by setting the variable `quit-flag` to `t`
the instant `C-g` is typed; Emacs Lisp checks this variable
frequently and quits if it is non-`nil`. `C-g` is only
actually executed as a command if you type it while Emacs is waiting for
input. In that case, the command it runs is `keyboard-quit`.

If you quit with `C-g` a second time before the first `C-g` is
recognized, you activate the "emergency escape" feature and return to
the shell. See section [AD.9.8 Emergency Escape](emacs.html#SEC519).

There may be times when you cannot quit. When Emacs is waiting for
the operating system to do something, quitting is impossible unless
special pains are taken for the particular system call within Emacs
where the waiting occurs. We have done this for the system calls that
users are likely to want to quit from, but it's possible you will find
another. In one very common case--waiting for file input or output
using NFS--Emacs itself knows how to quit, but many NFS implementations
simply do not allow user programs to stop waiting for NFS when the NFS
server is hung.

Aborting with `C-]` (`abort-recursive-edit`) is used to get
out of a recursive editing level and cancel the command which invoked
it. Quitting with `C-g` does not do this, and could not do this,
because it is used to cancel a partially typed command *within* the
recursive editing level. Both operations are useful. For example, if
you are in a recursive edit and type `C-u 8` to enter a numeric
argument, you can cancel that argument with `C-g` and remain in the
recursive edit.

The command `ESC ESC ESC`
(`keyboard-escape-quit`) can either quit or abort. This key was
defined because `ESC` is used to "get out" in many PC programs.
It can cancel a prefix argument, clear a selected region, or get out of
a Query Replace, like `C-g`. It can get out of the minibuffer or a
recursive edit, like `C-]`. It can also get out of splitting the
frame into multiple windows, like `C-x 1`. One thing it cannot do,
however, is stop a command that is running. That's because it executes
as an ordinary command, and Emacs doesn't notice it until it is ready
for a command.

The command `M-x top-level` is equivalent to "enough" `C-]`
commands to get you out of all the levels of recursive edits that you
are in. `C-]` gets you out one level at a time, but `M-x
top-level` goes out all levels at once. Both `C-]` and `M-x
top-level` are like all other commands, and unlike `C-g`, in that
they take effect only when Emacs is ready for a command. `C-]` is
an ordinary key and has its meaning only because of its binding in the
keymap. See section [AC.26 Recursive Editing Levels](emacs.html#SEC466).

`C-x u` (`undo`) is not strictly speaking a way of canceling
a command, but you can think of it as canceling a command that already
finished executing. See section [D.4 Undoing Changes](emacs.html#SEC23), for more information
about the undo facility.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC510) ] | [ [>](emacs.html#SEC512) ] | [ [<<](emacs.html#SEC510) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC521) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AD.9 Dealing with Emacs Trouble

This section describes various conditions in which Emacs fails to work
normally, and how to recognize them and correct them. For a list of
additional problems you might encounter, see section `Bugs and problems' in GNU Emacs FAQ, and the file `etc/PROBLEMS'
in the Emacs distribution. Type `C-h F` to read the FAQ; type
`C-h P` to read the `PROBLEMS' file.

> |  |  |  |
> | --- | --- | --- |
> | [AD.9.1 If `DEL` Fails to Delete](emacs.html#SEC512) |  | What to do if `DEL` doesn't delete. |
> | [AD.9.2 Recursive Editing Levels](emacs.html#SEC513) |  | `[...]' in mode line around the parentheses. |
> | [AD.9.3 Garbage on the Screen](emacs.html#SEC514) |  | Garbage on the screen. |
> | [AD.9.4 Garbage in the Text](emacs.html#SEC515) |  | Garbage in the text. |
> | [AD.9.5 Spontaneous Entry to Incremental Search](emacs.html#SEC516) |  | Spontaneous entry to incremental search. |
> | [AD.9.6 Running out of Memory](emacs.html#SEC517) |  | How to cope when you run out of memory. |
> | [AD.9.7 Recovery After a Crash](emacs.html#SEC518) |  | Recovering editing in an Emacs session that crashed. |
> | [AD.9.8 Emergency Escape](emacs.html#SEC519) |  | Emergency escape--- What to do if Emacs stops responding. |
> | [AD.9.9 Help for Total Frustration](emacs.html#SEC520) |  | When you are at your wits' end. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC511) ] | [ [>](emacs.html#SEC513) ] | [ [<<](emacs.html#SEC511) ] | [ [Up](emacs.html#SEC511) ] | [ [>>](emacs.html#SEC521) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.9.1 If `DEL` Fails to Delete

Every keyboard has a large key, a little ways above the `RET` or
`ENTER` key, which you normally use outside Emacs to erase the
last character that you typed. We call this key *the usual
erasure key*. In Emacs, it is supposed to be equivalent to `DEL`,
and when Emacs is properly configured for your terminal, it translates
that key into the character `DEL`.

When Emacs starts up using a window system, it determines
automatically which key should be `DEL`. In some unusual cases
Emacs gets the wrong information from the system. If the usual
erasure key deletes forwards instead of backwards, that is probably
what happened--Emacs ought to be treating the `DELETE` key as
`DEL`, but it isn't.

With a window system, if the usual erasure key is labeled
`BACKSPACE` and there is a `DELETE` key elsewhere, but the
`DELETE` key deletes backward instead of forward, that too
suggests Emacs got the wrong information--but in the opposite sense.
It ought to be treating the `BACKSPACE` key as `DEL`, and
treating `DELETE` differently, but it isn't.

On a text-only terminal, if you find the usual erasure key prompts
for a Help command, like `Control-h`, instead of deleting a
character, it means that key is actually sending the `BS`
character. Emacs ought to be treating `BS` as `DEL`, but it
isn't.

In all of those cases, the immediate remedy is the same: use the
command `M-x normal-erase-is-backspace-mode`. This toggles
between the two modes that Emacs supports for handling `DEL`, so
if Emacs starts in the wrong mode, it should switch to the right mode.
On a text-only terminal, if you want to ask for help when `BS` is
treated as `DEL`, use `F1`; `C-?` may also work, if it
sends character code 127.

To fix the problem automatically for every Emacs session, you can
put one of the following lines into your `.emacs' file
(see section [AD.7 The Init File, `~/.emacs'](emacs.html#SEC505)). For the first case above, where `DELETE`
deletes forwards instead of backwards, use this line to make
`DELETE` act as `DEL` (resulting in behavior compatible
with Emacs 20 and previous versions):

|  |  |
| --- | --- |
|  | ``` (normal-erase-is-backspace-mode 0) ``` |

For the other two cases, where `BACKSPACE` ought to act as
`DEL`, use this line:

|  |  |
| --- | --- |
|  | ``` (normal-erase-is-backspace-mode 1) ``` |

Another way to fix the problem for every Emacs session is to
customize the variable `normal-erase-is-backspace`: the value
`t` specifies the mode where `BS` or `BACKSPACE` is
`DEL`, and `nil` specifies the other mode. See section [AD.2.2 Easy Customization Interface](emacs.html#SEC479).

With a window system, it can also happen that the usual erasure key
is labeled `BACKSPACE`, there is a `DELETE` key elsewhere, and
both keys delete forward. This probably means that someone has
redefined your `BACKSPACE` key as a `DELETE` key. With X,
this is typically done with a command to the `xmodmap` program
when you start the server or log in. The most likely motive for this
customization was to support old versions of Emacs, so we recommend
you simply remove it now.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC512) ] | [ [>](emacs.html#SEC514) ] | [ [<<](emacs.html#SEC514) ] | [ [Up](emacs.html#SEC511) ] | [ [>>](emacs.html#SEC521) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.9.2 Recursive Editing Levels

Recursive editing levels are important and useful features of Emacs, but
they can seem like malfunctions to the user who does not understand them.

If the mode line has square brackets `` `[...]' `` around the parentheses
that contain the names of the major and minor modes, you have entered a
recursive editing level. If you did not do this on purpose, or if you
don't understand what that means, you should just get out of the recursive
editing level. To do so, type `M-x top-level`. This is called getting
back to top level. See section [AC.26 Recursive Editing Levels](emacs.html#SEC466).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC513) ] | [ [>](emacs.html#SEC515) ] | [ [<<](emacs.html#SEC515) ] | [ [Up](emacs.html#SEC511) ] | [ [>>](emacs.html#SEC521) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.9.3 Garbage on the Screen

If the data on the screen looks wrong, the first thing to do is see
whether the text is really wrong. Type `C-l` to redisplay the
entire screen. If the screen appears correct after this, the problem
was entirely in the previous screen update. (Otherwise, see the following
section.)

Display updating problems often result from an incorrect termcap entry
for the terminal you are using. The file `etc/TERMS' in the Emacs
distribution gives the fixes for known problems of this sort.
`INSTALL' contains general advice for these problems in one of its
sections. Very likely there is simply insufficient padding for certain
display operations. To investigate the possibility that you have this sort
of problem, try Emacs on another terminal made by a different manufacturer.
If problems happen frequently on one kind of terminal but not another kind,
it is likely to be a bad termcap entry, though it could also be due to a
bug in Emacs that appears for terminals that have or that lack specific
features.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC514) ] | [ [>](emacs.html#SEC516) ] | [ [<<](emacs.html#SEC516) ] | [ [Up](emacs.html#SEC511) ] | [ [>>](emacs.html#SEC521) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.9.4 Garbage in the Text

If `C-l` shows that the text is wrong, try undoing the changes to it
using `C-x u` until it gets back to a state you consider correct. Also
try `C-h l` to find out what command you typed to produce the observed
results.

If a large portion of text appears to be missing at the beginning or
end of the buffer, check for the word `` `Narrow' `` in the mode line.
If it appears, the text you don't see is probably still present, but
temporarily off-limits. To make it accessible again, type `C-x n
w`. See section [AC.22 Narrowing](emacs.html#SEC462).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC515) ] | [ [>](emacs.html#SEC517) ] | [ [<<](emacs.html#SEC517) ] | [ [Up](emacs.html#SEC511) ] | [ [>>](emacs.html#SEC521) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.9.5 Spontaneous Entry to Incremental Search

If Emacs spontaneously displays `` `I-search:' `` at the bottom of the
screen, it means that the terminal is sending `C-s` and `C-q`
according to the poorly designed xon/xoff "flow control" protocol.

If this happens to you, your best recourse is to put the terminal in a
mode where it will not use flow control, or give it so much padding that
it will never send a `C-s`. (One way to increase the amount of
padding is to set the variable `baud-rate` to a larger value. Its
value is the terminal output speed, measured in the conventional units
of baud.)

If you don't succeed in turning off flow control, the next best thing
is to tell Emacs to cope with it. To do this, call the function
`enable-flow-control`.

Typically there are particular terminal types with which you must use
flow control. You can conveniently ask for flow control on those
terminal types only, using `enable-flow-control-on`. For example,
if you find you must use flow control on VT-100 and H19 terminals, put
the following in your `.emacs' file:

|  |  |
| --- | --- |
|  | ``` (enable-flow-control-on "vt100" "h19") ``` |

When flow control is enabled, you must type `C-\` to get the
effect of a `C-s`, and type `C-^` to get the effect of a
`C-q`. (These aliases work by means of keyboard translations; see
[AD.5 Keyboard Translations](emacs.html#SEC503).)

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC516) ] | [ [>](emacs.html#SEC518) ] | [ [<<](emacs.html#SEC518) ] | [ [Up](emacs.html#SEC511) ] | [ [>>](emacs.html#SEC521) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.9.6 Running out of Memory

If you get the error message `` `Virtual memory exceeded' ``, save your
modified buffers with `C-x s`. This method of saving them has the
smallest need for additional memory. Emacs keeps a reserve of memory
which it makes available when this error happens; that should be enough
to enable `C-x s` to complete its work.

Once you have saved your modified buffers, you can exit this Emacs job
and start another, or you can use `M-x kill-some-buffers` to free
space in the current Emacs job. If you kill buffers containing a
substantial amount of text, you can safely go on editing. Emacs refills
its memory reserve automatically when it sees sufficient free space
available, in case you run out of memory another time.

Do not use `M-x buffer-menu` to save or kill buffers when you run
out of memory, because the buffer menu needs a fair amount of memory
itself, and the reserve supply may not be enough.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC517) ] | [ [>](emacs.html#SEC519) ] | [ [<<](emacs.html#SEC519) ] | [ [Up](emacs.html#SEC511) ] | [ [>>](emacs.html#SEC521) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.9.7 Recovery After a Crash

If Emacs or the computer crashes, you can recover the files you were
editing at the time of the crash from their auto-save files. To do
this, start Emacs again and type the command `M-x recover-session`.

This command initially displays a buffer which lists interrupted
session files, each with its date. You must choose which session to
recover from. Typically the one you want is the most recent one. Move
point to the one you choose, and type `C-c C-c`.

Then `recover-session` asks about each of the files that you were
editing during that session; it asks whether to recover that file. If
you answer `y` for a file, it shows the dates of that file and its
auto-save file, then asks once again whether to recover that file. For
the second question, you must confirm with `yes`. If you do, Emacs
visits the file but gets the text from the auto-save file.

When `recover-session` is done, the files you've chosen to
recover are present in Emacs buffers. You should then save them. Only
this--saving them--updates the files themselves.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC518) ] | [ [>](emacs.html#SEC520) ] | [ [<<](emacs.html#SEC520) ] | [ [Up](emacs.html#SEC511) ] | [ [>>](emacs.html#SEC521) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.9.8 Emergency Escape

Because at times there have been bugs causing Emacs to loop without
checking `quit-flag`, a special feature causes Emacs to be suspended
immediately if you type a second `C-g` while the flag is already set,
so you can always get out of GNU Emacs. Normally Emacs recognizes and
clears `quit-flag` (and quits!) quickly enough to prevent this from
happening. (On MS-DOS and compatible systems, type `C-BREAK`
twice.)

When you resume Emacs after a suspension caused by multiple `C-g`, it
asks two questions before going back to what it had been doing:

|  |  |
| --- | --- |
|  | ``` Auto-save? (y or n) Abort (and dump core)? (y or n) ``` |

Answer each one with `y` or `n` followed by `RET`.

Saying `y` to `` `Auto-save?' `` causes immediate auto-saving of all
modified buffers in which auto-saving is enabled.

Saying `y` to `` `Abort (and dump core)?' `` causes an illegal instruction to be
executed, dumping core. This is to enable a wizard to figure out why Emacs
was failing to quit in the first place. Execution does not continue
after a core dump. If you answer `n`, execution does continue. With
luck, GNU Emacs will ultimately check `quit-flag` and quit normally.
If not, and you type another `C-g`, it is suspended again.

If Emacs is not really hung, just slow, you may invoke the double
`C-g` feature without really meaning to. Then just resume and answer
`n` to both questions, and you will arrive at your former state.
Presumably the quit you requested will happen soon.

The double `C-g` feature is turned off when Emacs is running under
the X Window System, since you can use the window manager to kill Emacs
or to create another window and run another program.

On MS-DOS and compatible systems, the emergency escape feature is
sometimes unavailable, even if you press `C-BREAK` twice, when
some system call (MS-DOS or BIOS) hangs, or when Emacs is stuck in a
very tight endless loop (in C code, **not** in Lisp code).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC519) ] | [ [>](emacs.html#SEC521) ] | [ [<<](emacs.html#SEC511) ] | [ [Up](emacs.html#SEC511) ] | [ [>>](emacs.html#SEC521) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.9.9 Help for Total Frustration

If using Emacs (or something else) becomes terribly frustrating and none
of the techniques described above solve the problem, Emacs can still help
you.

First, if the Emacs you are using is not responding to commands, type
`C-g C-g` to get out of it and then start a new one.

Second, type `M-x doctor RET`.

The doctor will help you feel better. Each time you say something to
the doctor, you must end it by typing `RET` `RET`. This lets
the doctor know you are finished.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC520) ] | [ [>](emacs.html#SEC522) ] | [ [<<](emacs.html#SEC511) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC526) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AD.10 Reporting Bugs

Sometimes you will encounter a bug in Emacs. Although we cannot
promise we can or will fix the bug, and we might not even agree that it
is a bug, we want to hear about problems you encounter. Often we agree
they are bugs and want to fix them.

To make it possible for us to fix a bug, you must report it. In order
to do so effectively, you must know when and how to do it.

Before reporting a bug, it is a good idea to see if it is already
known. You can find the list of known problems in the file
`etc/PROBLEMS' in the Emacs distribution; type `C-h P` to read
it. Some additional user-level problems can be found in section `Bugs and problems' in GNU Emacs FAQ. Looking up your
problem in these two documents might provide you with a solution or a
work-around, or give you additional information about related issues.

> |  |  |  |
> | --- | --- | --- |
> | [AD.10.1 When Is There a Bug](emacs.html#SEC522) |  | Have you really found a bug? |
> | [AD.10.2 Understanding Bug Reporting](emacs.html#SEC523) |  | How to report a bug effectively. |
> | [AD.10.3 Checklist for Bug Reports](emacs.html#SEC524) |  | Steps to follow for a good bug report. |
> | [AD.10.4 Sending Patches for GNU Emacs](emacs.html#SEC525) |  | How to send a patch for GNU Emacs. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC521) ] | [ [>](emacs.html#SEC523) ] | [ [<<](emacs.html#SEC521) ] | [ [Up](emacs.html#SEC521) ] | [ [>>](emacs.html#SEC526) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.10.1 When Is There a Bug

If Emacs executes an illegal instruction, or dies with an operating
system error message that indicates a problem in the program (as opposed to
something like "disk full"), then it is certainly a bug.

If Emacs updates the display in a way that does not correspond to what is
in the buffer, then it is certainly a bug. If a command seems to do the
wrong thing but the problem corrects itself if you type `C-l`, it is a
case of incorrect display updating.

Taking forever to complete a command can be a bug, but you must make
certain that it was really Emacs's fault. Some commands simply take a
long time. Type `C-g` (`C-BREAK` on MS-DOS) and then `C-h l`
to see whether the input Emacs received was what you intended to type;
if the input was such that you *know* it should have been processed
quickly, report a bug. If you don't know whether the command should
take a long time, find out by looking in the manual or by asking for
assistance.

If a command you are familiar with causes an Emacs error message in a
case where its usual definition ought to be reasonable, it is probably a
bug.

If a command does the wrong thing, that is a bug. But be sure you know
for certain what it ought to have done. If you aren't familiar with the
command, or don't know for certain how the command is supposed to work,
then it might actually be working right. Rather than jumping to
conclusions, show the problem to someone who knows for certain.

Finally, a command's intended definition may not be the best
possible definition for editing with. This is a very important sort
of problem, but it is also a matter of judgment. Also, it is easy to
come to such a conclusion out of ignorance of some of the existing
features. It is probably best not to complain about such a problem
until you have checked the documentation in the usual ways, feel
confident that you understand it, and know for certain that what you
want is not available. If you are not sure what the command is
supposed to do after a careful reading of the manual, check the index
and glossary for any terms that may be unclear.

If after careful rereading of the manual you still do not understand
what the command should do, that indicates a bug in the manual, which
you should report. The manual's job is to make everything clear to
people who are not Emacs experts--including you. It is just as
important to report documentation bugs as program bugs.

If the on-line documentation string of a function or variable disagrees
with the manual, one of them must be wrong; that is a bug.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC522) ] | [ [>](emacs.html#SEC524) ] | [ [<<](emacs.html#SEC524) ] | [ [Up](emacs.html#SEC521) ] | [ [>>](emacs.html#SEC526) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.10.2 Understanding Bug Reporting

When you decide that there is a bug, it is important to report it and to
report it in a way which is useful. What is most useful is an exact
description of what commands you type, starting with the shell command to
run Emacs, until the problem happens.

The most important principle in reporting a bug is to report
*facts*. Hypotheses and verbal descriptions are no substitute for
the detailed raw data. Reporting the facts is straightforward, but many
people strain to posit explanations and report them instead of the
facts. If the explanations are based on guesses about how Emacs is
implemented, they will be useless; meanwhile, lacking the facts, we will
have no real information about the bug.

For example, suppose that you type `C-x C-f /glorp/baz.ugh
RET`, visiting a file which (you know) happens to be rather large,
and Emacs displayed `` `I feel pretty today' ``. The best way to report
the bug is with a sentence like the preceding one, because it gives all
the facts.

A bad way would be to assume that the problem is due to the size of
the file and say, "I visited a large file, and Emacs displayed `` `I
feel pretty today' ``." This is what we mean by "guessing
explanations." The problem is just as likely to be due to the fact
that there is a `` `z' `` in the file name. If this is so, then when we
got your report, we would try out the problem with some "large file,"
probably with no `` `z' `` in its name, and not see any problem. There
is no way in the world that we could guess that we should try visiting a
file with a `` `z' `` in its name.

Alternatively, the problem might be due to the fact that the file starts
with exactly 25 spaces. For this reason, you should make sure that you
inform us of the exact contents of any file that is needed to reproduce the
bug. What if the problem only occurs when you have typed the `C-x C-a`
command previously? This is why we ask you to give the exact sequence of
characters you typed since starting the Emacs session.

You should not even say "visit a file" instead of `C-x C-f` unless
you *know* that it makes no difference which visiting command is used.
Similarly, rather than saying "if I have three characters on the line,"
say "after I type `RET A B C RET C-p`," if that is
the way you entered the text.

So please don't guess any explanations when you report a bug. If you
want to actually *debug* the problem, and report explanations that
are more than guesses, that is useful--but please include the facts as
well.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC523) ] | [ [>](emacs.html#SEC525) ] | [ [<<](emacs.html#SEC525) ] | [ [Up](emacs.html#SEC521) ] | [ [>>](emacs.html#SEC526) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
