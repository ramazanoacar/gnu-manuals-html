### AC.28.4 Finding Function and Variable Definitions

`M-x find-function RET function RET`: Find the definition of function in its source file. `M-x find-variable RET variable RET`: Find the definition of variable in its source file. `M-x find-function-on-key RET key`: Find the definition of the function that key invokes.

These commands provide an easy way to find the definitions of Emacs
Lisp functions and variables. They are similar in purpose to the Tags
facility (see section [W.2 Tags Tables](emacs.html#SEC334)), but don't require a tags table; on the other
hand, they only work for function and variable definitions that are
already loaded in the Emacs session.

To find the definition of a function, use `M-x find-function`.
`M-x find-variable` finds the definition of a specified variable.
`M-x find-function-on-key` finds the definition of the function
bound to a specified key.

To use these commands, you must have the Lisp source (`` `.el' ``)
files available along with the compiled (`` `.elc' ``) files, in
directories in `load-path`. You can use compressed source files
if you enable Auto Compression mode. These commands only handle
definitions written in Lisp, not primitive functions or variables
defined in the C code of Emacs.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC472) ] | [ [>](emacs.html#SEC474) ] | [ [<<](emacs.html#SEC468) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC474) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.29 Dissociated Press

`M-x dissociated-press` is a command for scrambling a file of text
either word by word or character by character. Starting from a buffer of
straight English, it produces extremely amusing output. The input comes
from the current Emacs buffer. Dissociated Press writes its output in a
buffer named `` `*Dissociation*' ``, and redisplays that buffer after every
couple of lines (approximately) so you can read the output as it comes out.

Dissociated Press asks every so often whether to continue generating
output. Answer `n` to stop it. You can also stop at any time by
typing `C-g`. The dissociation output remains in the
`` `*Dissociation*' `` buffer for you to copy elsewhere if you wish.

Dissociated Press operates by jumping at random from one point in the
buffer to another. In order to produce plausible output rather than
gibberish, it insists on a certain amount of overlap between the end of
one run of consecutive words or characters and the start of the next.
That is, if it has just output `president' and then decides to jump
to a different point in the file, it might spot the `ent' in `pentagon'
and continue from there, producing `presidentagon'.[(15)](emacs.html#FOOT15) Long sample texts produce the best results.

A positive argument to `M-x dissociated-press` tells it to operate
character by character, and specifies the number of overlap characters. A
negative argument tells it to operate word by word and specifies the number
of overlap words. In this mode, whole words are treated as the elements to
be permuted, rather than characters. No argument is equivalent to an
argument of two. For your againformation, the output goes only into the
buffer `` `*Dissociation*' ``. The buffer you start with is not changed.

Dissociated Press produces nearly the same results as a Markov chain
based on a frequency table constructed from the sample text. It is,
however, an independent, ignoriginal invention. Dissociated Press
techniquitously copies several consecutive characters from the sample
between random choices, whereas a Markov chain would choose randomly for
each word or character. This makes for more plausible sounding results,
and runs faster.

It is a mustatement that too much use of Dissociated Press can be a
developediment to your real work. Sometimes to the point of outragedy.
And keep dissociwords out of your documentation, if you want it to be well
userenced and properbose. Have fun. Your buggestions are welcome.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC473) ] | [ [>](emacs.html#SEC475) ] | [ [<<](emacs.html#SEC473) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC475) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.30 Other Amusements

If you are a little bit bored, you can try `M-x hanoi`. If you are
considerably bored, give it a numeric argument. If you are very, very
bored, try an argument of 9. Sit back and watch.

If you want a little more personal involvement, try `M-x gomoku`,
which plays the game Go Moku with you.

`M-x blackbox`, `M-x mpuz` and `M-x 5x5` are kinds of puzzles.
`blackbox` challenges you to determine the location of objects
inside a box by tomography. `mpuz` displays a multiplication
puzzle with letters standing for digits in a code that you must
guess--to guess a value, type a letter and then the digit you think it
stands for. The aim of `5x5` is to fill in all the squares.

`M-x decipher` helps you to cryptanalyze a buffer which is encrypted
in a simple monoalphabetic substitution cipher.

`M-x dunnet` runs an adventure-style exploration game, which is
a bigger sort of puzzle.

`M-x lm` runs a relatively non-participatory game in which a robot
attempts to maneuver towards a tree at the center of the window based on
unique olfactory cues from each of the four directions.

`M-x life` runs Conway's "Life" cellular automaton.

`M-x morse-region` converts text in a region to Morse code and
`M-x unmorse-region` converts it back. No cause for remorse.

`M-x pong` plays a Pong-like game, bouncing the ball off opposing
bats.

`M-x solitaire` plays a game of solitaire in which you jump pegs
across other pegs.

`M-x studlify-region` studlify-cases the region, producing
text like this:

|  |  |
| --- | --- |
|  | ``` M-x stUdlIfY-RegioN stUdlIfY-CaSeS thE region. ``` |

`M-x tetris` runs an implementation of the well-known Tetris game.
Likewise, `M-x snake` provides an implementation of Snake.

When you are frustrated, try the famous Eliza program. Just do
`M-x doctor`. End each input by typing `RET` twice.

When you are feeling strange, type `M-x yow`.

The command `M-x zone` plays games with the display when Emacs is
idle.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC474) ] | [ [>](emacs.html#SEC476) ] | [ [<<](emacs.html#SEC474) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC510) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# AD. Customization

This chapter talks about various topics relevant to adapting the
behavior of Emacs in minor ways. See The Emacs Lisp Reference
Manual for how to make more far-reaching changes.

Customization that you do within Emacs normally affects only the
particular Emacs session that you do it in--it does not persist
between sessions unless you save the customization in a file such as
`.emacs' or `.Xdefaults' that will affect future sessions.
See section [AD.7 The Init File, `~/.emacs'](emacs.html#SEC505). In the customization buffer, when you save
customizations for future sessions, this actually works by editing
`.emacs' for you.

> |  |  |  |
> | --- | --- | --- |
> | [AD.1 Minor Modes](emacs.html#SEC476) |  | Each minor mode is one feature you can turn on independently of any others. |
> | [AD.2 Variables](emacs.html#SEC477) |  | Many Emacs commands examine Emacs variables to decide what to do; by setting variables, you can control their functioning. |
> | [AD.3 Keyboard Macros](emacs.html#SEC487) |  | A keyboard macro records a sequence of keystrokes to be replayed with a single command. |
> | [AD.4 Customizing Key Bindings](emacs.html#SEC491) |  | The keymaps say what command each key runs. By changing them, you can "redefine keys". |
> | [AD.5 Keyboard Translations](emacs.html#SEC503) |  | If your keyboard passes an undesired code for a key, you can tell Emacs to substitute another code. |
> | [AD.6 The Syntax Table](emacs.html#SEC504) |  | The syntax table controls how words and expressions are parsed. |
> | [AD.7 The Init File, `~/.emacs'](emacs.html#SEC505) |  | How to write common customizations in the |
>
> `.emacs' file. 

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC475) ] | [ [>](emacs.html#SEC477) ] | [ [<<](emacs.html#SEC475) ] | [ [Up](emacs.html#SEC475) ] | [ [>>](emacs.html#SEC510) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AD.1 Minor Modes

Minor modes are optional features which you can turn on or off. For
example, Auto Fill mode is a minor mode in which `SPC` breaks lines
between words as you type. All the minor modes are independent of each
other and of the selected major mode. Most minor modes say in the mode
line when they are on; for example, `` `Fill' `` in the mode line means
that Auto Fill mode is on.

Append `-mode` to the name of a minor mode to get the name of a
command function that turns the mode on or off. Thus, the command to
enable or disable Auto Fill mode is called `M-x auto-fill-mode`. These
commands are usually invoked with `M-x`, but you can bind keys to them
if you wish. With no argument, the function turns the mode on if it was
off and off if it was on. This is known as *toggling*. A positive
argument always turns the mode on, and an explicit zero argument or a
negative argument always turns it off.

Some minor modes are global: while enabled, they affect everything
you do in the Emacs session, in all buffers. Other minor modes are
buffer-local; they apply only to the current buffer, so you can enable
the mode in certain buffers and not others.

For most minor modes, the command name is also the name of a
variable which directly controls the mode. The mode is enabled
whenever this variable's value is non-`nil`, and the minor-mode
command works by setting the variable. For example, the command
`outline-minor-mode` works by setting the value of
`outline-minor-mode` as a variable; it is this variable that
directly turns Outline minor mode on and off. To check whether a
given minor mode works this way, use `C-h v` to ask for
documentation on the variable name.

These minor-mode variables provide a good way for Lisp programs to turn
minor modes on and off; they are also useful in a file's local variables
list. But please think twice before setting minor modes with a local
variables list, because most minor modes are matter of user
preference--other users editing the same file might not want the same
minor modes you prefer.

The buffer-local minor modes include Abbrev mode, Auto Fill mode,
Auto Save mode, Font-Lock mode, Glasses mode, ISO Accents mode,
Outline minor mode, Overwrite mode, and Binary Overwrite mode.

Abbrev mode allows you to define abbreviations that automatically expand
as you type them. For example, `` `amd' `` might expand to `` `abbrev
mode' ``. See section [X. Abbrevs](emacs.html#SEC350), for full information.

Auto Fill mode allows you to enter filled text without breaking lines
explicitly. Emacs inserts newlines as necessary to prevent lines from
becoming too long. See section [T.5 Filling Text](emacs.html#SEC238).

Auto Save mode causes the contents of a buffer to be saved
periodically to reduce the amount of work you can lose in case of a
system crash. See section [M.5 Auto-Saving: Protection Against Disasters](emacs.html#SEC122).

Enriched mode enables editing and saving of formatted text.
See section [T.11 Editing Formatted Text](emacs.html#SEC258).

Flyspell mode automatically highlights misspelled words.
See section [L.4 Checking and Correcting Spelling](emacs.html#SEC109).

Font-Lock mode automatically highlights certain textual units found in
programs, such as comments, strings, and function names being defined.
This requires a window system that can display multiple fonts.
See section [J.1 Using Multiple Typefaces](emacs.html#SEC78).

ISO Accents mode makes the characters ``` ``' ```, `` `'' ``, `` `"' ``,
`` `^' ``, `` `/' `` and `` `~' `` combine with the following letter, to
produce an accented letter in the ISO Latin-1 character set. The
newer and more general feature of input methods more or less
supersedes ISO Accents mode. See section [Q.13 Single-byte Character Set Support](emacs.html#SEC226).

Outline minor mode provides the same facilities as the major mode
called Outline mode; but since it is a minor mode instead, you can
combine it with any major mode. See section [T.8 Outline Mode](emacs.html#SEC246).

Overwrite mode causes ordinary printing characters to replace existing
text instead of shoving it to the right. For example, if point is in
front of the `` `B' `` in `` `FOOBAR' ``, then in Overwrite mode typing a
`G` changes it to `` `FOOGAR' ``, instead of producing `` `FOOGBAR' ``
as usual. In Overwrite mode, the command `C-q` inserts the next
character whatever it may be, even if it is a digit--this gives you a
way to insert a character instead of replacing an existing character.

The command `overwrite-mode` is an exception to the rule that
commands which toggle minor modes are normally not bound to keys: it is
bound to the `INSERT` function key. This is because many other
programs bind `INSERT` to similar functions.

Binary Overwrite mode is a variant of Overwrite mode for editing
binary files; it treats newlines and tabs like other characters, so that
they overwrite other characters and can be overwritten by them.
In Binary Overwrite mode, digits after `C-q` specify an
octal character code, as usual.

The following minor modes normally apply to all buffers at once.
Since each is enabled or disabled by the value of a variable, you
*can* set them differently for particular buffers, by explicitly
making the corresponding variables local in those buffers.
See section [AD.2.4 Local Variables](emacs.html#SEC485).

Icomplete mode displays an indication of available completions when
you are in the minibuffer and completion is active. See section [E.3.4 Completion Options](emacs.html#SEC38).

Line Number mode enables continuous display in the mode line of the
line number of point, and Column Number mode enables display of the
column number. See section [B.3 The Mode Line](emacs.html#SEC11).

Scroll Bar mode gives each window a scroll bar (see section [P.13 Scroll Bars](emacs.html#SEC204)).
Menu Bar mode gives each frame a menu bar (see section [P.15 Menu Bars](emacs.html#SEC206)). Both of
these modes are enabled by default when you use the X Window System.

In Transient Mark mode, every change in the buffer contents
"deactivates" the mark, so that commands that operate on the region
will get an error. This means you must either set the mark, or
explicitly "reactivate" it, before each command that uses the region.
The advantage of Transient Mark mode is that Emacs can display the
region highlighted (currently only when using X). See section [H. The Mark and the Region](emacs.html#SEC52).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC476) ] | [ [>](emacs.html#SEC478) ] | [ [<<](emacs.html#SEC487) ] | [ [Up](emacs.html#SEC475) ] | [ [>>](emacs.html#SEC487) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AD.2 Variables

A *variable* is a Lisp symbol which has a value. The symbol's
name is also called the name of the variable. A variable name can
contain any characters that can appear in a file, but conventionally
variable names consist of words separated by hyphens. A variable can
have a documentation string which describes what kind of value it should
have and how the value will be used.

Lisp allows any variable to have any kind of value, but most variables
that Emacs uses require a value of a certain type. Often the value should
always be a string, or should always be a number. Sometimes we say that a
certain feature is turned on if a variable is "non-`nil`," meaning
that if the variable's value is `nil`, the feature is off, but the
feature is on for *any* other value. The conventional value to use to
turn on the feature--since you have to pick one particular value when you
set the variable--is `t`.

Emacs uses many Lisp variables for internal record keeping, as any
Lisp program must, but the most interesting variables for you are the
ones that exist for the sake of customization. Emacs does not (usually)
change the values of these variables; instead, you set the values, and
thereby alter and control the behavior of certain Emacs commands. These
variables are called *user options*. Most user options are
documented in this manual, and appear in the Variable Index
(see section [Variable Index](emacs.html#SEC578)).

One example of a variable which is a user option is `fill-column`, which
specifies the position of the right margin (as a number of characters from
the left margin) to be used by the fill commands (see section [T.5 Filling Text](emacs.html#SEC238)).

> |  |  |  |
> | --- | --- | --- |
> | [AD.2.1 Examining and Setting Variables](emacs.html#SEC478) |  | Examining or setting one variable's value. |
> | [AD.2.2 Easy Customization Interface](emacs.html#SEC479) |  | Convenient and easy customization of variables. |
> | [AD.2.3 Hooks](emacs.html#SEC484) |  | Hook variables let you specify programs for parts of Emacs to run on particular occasions. |
> | [AD.2.4 Local Variables](emacs.html#SEC485) |  | Per-buffer values of variables. |
> | [AD.2.5 Local Variables in Files](emacs.html#SEC486) |  | How files can specify variable values. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC477) ] | [ [>](emacs.html#SEC479) ] | [ [<<](emacs.html#SEC487) ] | [ [Up](emacs.html#SEC477) ] | [ [>>](emacs.html#SEC487) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.2.1 Examining and Setting Variables

`C-h v var RET`: Display the value and documentation of variable var (`describe-variable`). `M-x set-variable RET var RET value RET`: Change the value of variable var to value.

To examine the value of a single variable, use `C-h v`
(`describe-variable`), which reads a variable name using the
minibuffer, with completion. It displays both the value and the
documentation of the variable. For example,

|  |  |
| --- | --- |
|  | ``` C-h v fill-column RET ``` |

displays something like this:

|  |  |
| --- | --- |
|  | ``` fill-column's value is 70  Documentation: *Column beyond which automatic line-wrapping should happen. Automatically becomes buffer-local when set in any fashion. ``` |

The star at the beginning of the documentation indicates that this
variable is a user option. `C-h v` is not restricted to user
options; it allows any variable name.

The most convenient way to set a specific user option is with `M-x
set-variable`. This reads the variable name with the minibuffer (with
completion), and then reads a Lisp expression for the new value using
the minibuffer a second time. For example,

|  |  |
| --- | --- |
|  | ``` M-x set-variable RET fill-column RET 75 RET ``` |

sets `fill-column` to 75.

`M-x set-variable` is limited to user option variables, but you can
set any variable with a Lisp expression, using the function `setq`.
Here is a `setq` expression to set `fill-column`:

|  |  |
| --- | --- |
|  | ``` (setq fill-column 75) ``` |

To execute an expression like this one, go to the `` `*scratch*' ``
buffer, type in the expression, and then type `C-j`. See section [V.9 Lisp Interaction Buffers](emacs.html#SEC330).

Setting variables, like all means of customizing Emacs except where
otherwise stated, affects only the current Emacs session.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC478) ] | [ [>](emacs.html#SEC480) ] | [ [<<](emacs.html#SEC484) ] | [ [Up](emacs.html#SEC477) ] | [ [>>](emacs.html#SEC484) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.2.2 Easy Customization Interface

A convenient way to find the user option variables that you want to
change, and then change them, is with `M-x customize`. This command
creates a *customization buffer* with which you can browse through
the Emacs user options in a logically organized structure, then edit and
set their values. You can also use the customization buffer to save
settings permanently. (Not all Emacs user options are included in this
structure as of yet, but we are adding the rest.)

The appearance of the example buffers in the following is typically
different under a window system where faces can be used to indicate the
active fields and other features.

> |  |  |  |
> | --- | --- | --- |
> | [AD.2.2.1 Customization Groups](emacs.html#SEC480) |  | How options are classified in a structure. |
> | [AD.2.2.2 Changing an Option](emacs.html#SEC481) |  | How to edit a value and set an option. |
> | [AD.2.2.3 Customizing Faces](emacs.html#SEC482) |  | How to edit the attributes of a face. |
> | [AD.2.2.4 Customizing Specific Items](emacs.html#SEC483) |  | Making a customization buffer for specific options, faces, or groups. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC479) ] | [ [>](emacs.html#SEC481) ] | [ [<<](emacs.html#SEC484) ] | [ [Up](emacs.html#SEC479) ] | [ [>>](emacs.html#SEC484) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### AD.2.2.1 Customization Groups

For customization purposes, user options are organized into
*groups* to help you find them. Groups are collected into bigger
groups, all the way up to a master group called `Emacs`.

`M-x customize` creates a customization buffer that shows the
top-level `Emacs` group and the second-level groups immediately
under it. It looks like this, in part:

|  |  |
| --- | --- |
|  | ``` /- Emacs group: ---------------------------------------------------\       [State]: visible group members are all at standard settings.    Customization of the One True Editor.    See also [Manual].  Confirm Kill Emacs: [Hide] [Value Menu] Don't confirm    [State]: this option is unchanged from its standard setting. How to ask for confirmation when leaving Emacs. [More]  Editing group: [Go to Group]  Basic text editing facilities.  External group: [Go to Group]  Interfacing to external utilities.  more second-level groups  \- Emacs group end ------------------------------------------------/ ``` |

This says that the buffer displays the contents of the `Emacs`
group. The other groups are listed because they are its contents. But
they are listed differently, without indentation and dashes, because
*their* contents are not included. Each group has a single-line
documentation string; the `Emacs` group also has a `` `[State]' ``
line.

Most of the text in the customization buffer is read-only, but it
typically includes some *editable fields* that you can edit. There
are also *active fields*; this means a field that does something
when you *invoke* it. To invoke an active field, either click on it
with `Mouse-1`, or move point to it and type `RET`.

For example, the phrase `` `[Go to Group]' `` that appears in a
second-level group is an active field. Invoking the `` `[Go to
Group]' `` field for a group creates a new customization buffer, which
shows that group and its contents. This field is a kind of hypertext
link to another group.

The `Emacs` group includes a few user options itself, but
mainly it contains other groups, which contain more groups, which
contain the user options. By browsing the hierarchy of groups, you
will eventually find the feature you are interested in customizing.
Then you can use the customization buffer to set the options and faces
pertaining to that feature. You can also go straight to a particular
group by name, using the command `M-x customize-group`.

You can view the structure of customization groups on a larger scale
with `M-x customize-browse`. This command creates a special kind of
customization buffer which shows only the names of the groups (and
options and faces), and their structure.

In this buffer, you can show the contents of a group by invoking
`` `[+]' ``. When the group contents are visible, this button changes to
`` `[-]' ``; invoking that hides the group contents.

Each group, option or face name in this buffer has an active field
which says `` `[Group]' ``, `` `[Option]' `` or `` `[Face]' ``. Invoking
that active field creates an ordinary customization buffer showing just
that group and its contents, just that option, or just that face.
This is the way to set values in it.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC480) ] | [ [>](emacs.html#SEC482) ] | [ [<<](emacs.html#SEC482) ] | [ [Up](emacs.html#SEC479) ] | [ [>>](emacs.html#SEC484) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### AD.2.2.2 Changing an Option

Here is an example of what a user option looks like in the
customization buffer:

|  |  |
| --- | --- |
|  | ``` Kill Ring Max: [Hide] 60    [State]: this option is unchanged from its standard setting. Maximum length of kill ring before oldest elements are thrown away. ``` |

The text following `` `[Hide]' ``, `` `60' `` in this case, indicates
the current value of the option. If you see `` `[Show]' `` instead of
`` `[Hide]' ``, it means that the value is hidden; the customization
buffer initially hides values that take up several lines. Invoke
`` `[Show]' `` to show the value.

The line after the option name indicates the *customization state*
of the option: in the example above, it says you have not changed the
option yet. The word `` `[State]' `` at the beginning of this line is
active; you can get a menu of various operations by invoking it with
`Mouse-1` or `RET`. These operations are essential for
customizing the variable.

The line after the `` `[State]' `` line displays the beginning of the
option's documentation string. If there are more lines of
documentation, this line ends with `` `[More]' ``; invoke this to show
the full documentation string.

To enter a new value for `` `Kill Ring Max' ``, move point to the value
and edit it textually. For example, you can type `M-d`, then insert
another number.

When you begin to alter the text, you will see the `` `[State]' `` line
change to say that you have edited the value:

|  |  |
| --- | --- |
|  | ``` [State]: you have edited the value as text, but not set the option. ``` |

Editing the value does not actually set the option variable. To do
that, you must *set* the option. To do this, invoke the word
`` `[State]' `` and choose `` `Set for Current Session' ``.

The state of the option changes visibly when you set it:

|  |  |
| --- | --- |
|  | ``` [State]: you have set this option, but not saved it for future sessions. ``` |

You don't have to worry about specifying a value that is not valid;
setting the option checks for validity and will not really install an
unacceptable value.

While editing a value or field that is a file name, directory name,
command name, or anything else for which completion is defined, you can
type `M-TAB` (`widget-complete`) to do completion.

Some options have a small fixed set of possible legitimate values.
These options don't let you edit the value textually. Instead, an
active field `` `[Value Menu]' `` appears before the value; invoke this
field to edit the value. For a boolean "on or off" value, the active
field says `` `[Toggle]' ``, and it changes to the other value.
`` `[Value Menu]' `` and `` `[Toggle]' `` edit the buffer; the changes
take effect when you use the `` `Set for Current Session' `` operation.

Some options have values with complex structure. For example, the
value of `file-coding-system-alist` is an association list. Here
is how it appears in the customization buffer:

|  |  |
| --- | --- |
|  | ``` File Coding System Alist: [Hide] [INS] [DEL] File regexp: \.elc\'             Choice: [Value Menu] Encoding/decoding pair:             Decoding: emacs-mule             Encoding: emacs-mule [INS] [DEL] File regexp: \(\`\|/\)loaddefs.el\'             Choice: [Value Menu] Encoding/decoding pair:             Decoding: raw-text             Encoding: raw-text-unix [INS] [DEL] File regexp: \.tar\'             Choice: [Value Menu] Encoding/decoding pair:             Decoding: no-conversion             Encoding: no-conversion [INS] [DEL] File regexp:             Choice: [Value Menu] Encoding/decoding pair:             Decoding: undecided             Encoding: nil [INS]    [State]: this option is unchanged from its standard setting. Alist to decide a coding system to use for a file I/O operation. [Hide] The format is ((PATTERN . VAL) ...), where PATTERN is a regular expression matching a file name, [...more lines of documentation...] ``` |

Each association in the list appears on four lines, with several
editable or "active" fields. You can edit the regexps and coding
systems using ordinary editing commands. You can also invoke
`` `[Value Menu]' `` to switch to a kind of value--for instance, to
specify a function instead of a pair of coding systems.

To delete an association from the list, invoke the `` `[DEL]' `` button
for that item. To add an association, invoke `` `[INS]' `` at the
position where you want to add it. There is an `` `[INS]' `` button
between each pair of association, another at the beginning and another
at the end, so you can add the new association at any position in the
list.

Two special commands, `TAB` and `S-TAB`, are useful for
moving through the customization buffer. `TAB`
(`widget-forward`) moves forward to the next active or editable
field; `S-TAB` (`widget-backward`) moves backward to the
previous active or editable field.

Typing `RET` on an editable field also moves forward, just like
`TAB`. We set it up this way because people often type `RET`
when they are finished editing a field. To insert a newline within an
editable field, use `C-o` or `C-q C-j`.

Setting the option changes its value in the current Emacs session;
*saving* the value changes it for future sessions as well. This
works by writing code into your `~/.emacs' file so as to set the
option variable again each time you start Emacs. To save the option,
invoke `` `[State]' `` and select the `` `Save for Future Sessions' ``
operation.

If Emacs was invoked with the `` `-q' `` or `` `--no-init-file' ``
options (see section [AE.2 Initial Options](emacs.html#SEC530)), it will not let you save your
customizations in your `~/.emacs' init file. This is because
saving customizations from such a session would wipe out all the other
customizations you might have on your init file.

You can also restore the option to its standard value by invoking
`` `[State]' `` and selecting the `` `Erase Customization' ``
operation. There are actually three reset operations:

`` `Reset' ``: If you have made some modifications and not yet set the option, this restores the text in the customization buffer to match the actual value. `` `Reset to Saved' ``: This restores the value of the option to the last saved value, and updates the text accordingly. `` `Erase Customization' ``: This sets the option to its standard value, and updates the text accordingly. This also eliminates any saved value for the option, so that you will get the standard value in future Emacs sessions.

Sometimes it is useful to record a comment about a specific
customization. Use the `` `Add Comment' `` item from the
`` `[State]' `` menu to create a field for entering the comment. The
comment you enter will be saved, and displayed again if you again view
the same option in a customization buffer, even in another session.

The state of a group indicates whether anything in that group has been
edited, set or saved. You can select `` `Set for Current Session' ``,
`` `Save for Future Sessions' `` and the various kinds of `` `Reset' ``
operation for the group; these operations on the group apply to all
options in the group and its subgroups.

Near the top of the customization buffer there are two lines
containing several active fields:

|  |  |
| --- | --- |
|  | ```  [Set for Current Session] [Save for Future Sessions]  [Reset] [Reset to Saved] [Erase Customization]   [Finish] ``` |

Invoking `` `[Finish]' `` either buries or kills this customization
buffer according to the setting of the option
`custom-buffer-done-function`; the default is to bury the buffer.
Each of the other fields performs an operation--set, save or
reset--on each of the items in the buffer that could meaningfully be
set, saved or reset.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC481) ] | [ [>](emacs.html#SEC483) ] | [ [<<](emacs.html#SEC483) ] | [ [Up](emacs.html#SEC479) ] | [ [>>](emacs.html#SEC484) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### AD.2.2.3 Customizing Faces

In addition to user options, some customization groups also include
faces. When you show the contents of a group, both the user options and
the faces in the group appear in the customization buffer. Here is an
example of how a face looks:

|  |  |
| --- | --- |
|  | ``` Custom Changed Face: (sample) [Hide]    [State]: this face is unchanged from its standard setting. Parent groups: [Custom Magic Faces] Attributes: [ ] Font family: [Value Menu] *             [ ] Width: [Value Menu] *             [ ] Height: [Value Menu] *             [ ] Weight: [Value Menu] *             [ ] Slant: [Value Menu] *             [ ] Underline: [Value Menu] *             [ ] Overline: [Value Menu] *             [ ] Strike-through: [Value Menu] *             [ ] Box around text: [Value Menu] *             [ ] Inverse-video: [Value Menu] *             [X] Foreground: [Value Menu] Color: white       (sample)             [X] Background: [Value Menu] Color: blue        (sample)             [ ] Stipple: [Value Menu] *             [ ] Inherit: ``` |

Each face attribute has its own line. The `` `[x]' `` field
before the attribute name indicates whether the attribute is
*enabled*; `` `X' `` means that it is. You can enable or disable the
attribute by invoking that field. When the attribute is enabled, you
can change the attribute value in the usual ways.

On a black-and-white display, the colors you can use for the
background are `` `black' ``, `` `white' ``, `` `gray' ``, `` `gray1' ``,
and `` `gray3' ``. Emacs supports these shades of gray by using
background stipple patterns instead of a color.

Setting, saving and resetting a face work like the same operations for
options (see section [AD.2.2.2 Changing an Option](emacs.html#SEC481)).

A face can specify different appearances for different types of
display. For example, a face can make text red on a color display, but
use a bold font on a monochrome display. To specify multiple
appearances for a face, select `` `Show all display specs' `` in the menu you
get from invoking `` `[State]' ``.

Another more basic way to set the attributes of a specific face is
with `M-x modify-face`. This command reads the name of a face, then
reads the attributes one by one. For the color and stipple attributes,
the attribute's current value is the default--type just `RET` if
you don't want to change that attribute. Type `` `none' `` if you want
to clear out the attribute.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC482) ] | [ [>](emacs.html#SEC484) ] | [ [<<](emacs.html#SEC484) ] | [ [Up](emacs.html#SEC479) ] | [ [>>](emacs.html#SEC484) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### AD.2.2.4 Customizing Specific Items

Instead of finding the options you want to change by moving down
through the structure of groups, you can specify the particular option,
face or group that you want to customize.

`M-x customize-option RET option RET`: Set up a customization buffer with just one option, option. `M-x customize-face RET face RET`: Set up a customization buffer with just one face, face. `M-x customize-group RET group RET`: Set up a customization buffer with just one group, group. `M-x customize-apropos RET regexp RET`: Set up a customization buffer with all the options, faces and groups that match regexp. `M-x customize-changed-options RET version RET`: Set up a customization buffer with all the options, faces and groups whose meaning has changed since Emacs version version. `M-x customize-saved`: Set up a customization buffer containing all options and faces that you have saved with customization buffers. `M-x customize-customized`: Set up a customization buffer containing all options and faces that you have customized but not saved.

If you want to alter a particular user option variable with the
customization buffer, and you know its name, you can use the command
`M-x customize-option` and specify the option name. This sets up
the customization buffer with just one option--the one that you asked
for. Editing, setting and saving the value work as described above, but
only for the specified option.

Likewise, you can modify a specific face, chosen by name, using
`M-x customize-face`.

You can also set up the customization buffer with a specific group,
using `M-x customize-group`. The immediate contents of the chosen
group, including option variables, faces, and other groups, all appear
as well. However, these subgroups' own contents start out hidden. You
can show their contents in the usual way, by invoking `` `[Show]' ``.

To control more precisely what to customize, you can use `M-x
customize-apropos`. You specify a regular expression as argument; then
all options, faces and groups whose names match this regular expression
are set up in the customization buffer. If you specify an empty regular
expression, this includes *all* groups, options and faces in the
customization buffer (but that takes a long time).

When you upgrade to a new Emacs version, you might want to customize
new options and options whose meanings or default values have changed.
To do this, use `M-x customize-changed-options` and specify a
previous Emacs version number using the minibuffer. It creates a
customization buffer which shows all the options (and groups) whose
definitions have been changed since the specified version.

If you change option values and then decide the change was a mistake,
you can use two special commands to revisit your previous changes. Use
`M-x customize-saved` to look at the options and faces that you have
saved. Use `M-x customize-customized` to look at the options and
faces that you have set but not saved.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC483) ] | [ [>](emacs.html#SEC485) ] | [ [<<](emacs.html#SEC485) ] | [ [Up](emacs.html#SEC477) ] | [ [>>](emacs.html#SEC487) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.2.3 Hooks

*Hooks* are an important mechanism for customization of Emacs. A
hook is a Lisp variable which holds a list of functions, to be called on
some well-defined occasion. (This is called *running the hook*.)
The individual functions in the list are called the *hook functions*
of the hook. With rare exceptions, hooks in Emacs are empty when Emacs
starts up, so the only hook functions in any given hook are the ones you
explicitly put there as customization.

Most major modes run one or more *mode hooks* as the last step of
initialization. This makes it easy for you to customize the behavior of
the mode, by setting up a hook function to override the local variable
assignments already made by the mode. But hooks are also used in other
contexts. For example, the hook `suspend-hook` runs just before
Emacs suspends itself (see section [C.1 Exiting Emacs](emacs.html#SEC18)).

Most Emacs hooks are *normal hooks*. This means that running the
hook operates by calling all the hook functions, unconditionally, with
no arguments. We have made an effort to keep most hooks normal so that
you can use them in a uniform way. Every variable in Emacs whose name
ends in `` `-hook' `` is a normal hook.

There are also a few *abnormal hooks*. These variables' names end
in `` `-hooks' `` or `` `-functions' ``, instead of `` `-hook' ``. What
makes these hooks abnormal is that there is something peculiar about the
way its functions are called--perhaps they are given arguments, or
perhaps the values they return are used in some way. For example,
`find-file-not-found-hooks` (see section [M.2 Visiting Files](emacs.html#SEC112)) is abnormal because
as soon as one hook function returns a non-`nil` value, the rest
are not called at all. The documentation of each abnormal hook variable
explains in detail what is peculiar about it.

The recommended way to add a hook function to a hook (either normal or
abnormal) is by calling `add-hook`. You can use any valid Lisp
function as the hook function, provided it can handle the proper number
of arguments (zero arguments, in the case of a normal hook). Of course,
not every Lisp function is *useful* in any particular hook.

For example, here's how to set up a hook to turn on Auto Fill mode
when entering Text mode and other modes based on Text mode:

|  |  |
| --- | --- |
|  | ``` (add-hook 'text-mode-hook 'turn-on-auto-fill) ``` |

The next example shows how to use a hook to customize the indentation
of C code. (People often have strong personal preferences for one
format compared to another.) Here the hook function is an anonymous
lambda expression.

|  |  |
| --- | --- |
|  | ``` (setq my-c-style   '((c-comment-only-line-offset . 4)     (c-cleanup-list . (scope-operator 		       empty-defun-braces 		       defun-close-semi))     (c-offsets-alist . ((arglist-close . c-lineup-arglist) 			(substatement-open . 0)))))  (add-hook 'c-mode-common-hook   '(lambda ()      (c-add-style "my-style" my-c-style t))) ``` |

It is best to design your hook functions so that the order in which
they are executed does not matter. Any dependence on the order is
"asking for trouble." However, the order is predictable: the most
recently added hook functions are executed first.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC484) ] | [ [>](emacs.html#SEC486) ] | [ [<<](emacs.html#SEC486) ] | [ [Up](emacs.html#SEC477) ] | [ [>>](emacs.html#SEC487) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.2.4 Local Variables

`M-x make-local-variable RET var RET`: Make variable var have a local value in the current buffer. `M-x kill-local-variable RET var RET`: Make variable var use its global value in the current buffer. `M-x make-variable-buffer-local RET var RET`: Mark variable var so that setting it will make it local to the buffer that is current at that time.

Almost any variable can be made *local* to a specific Emacs
buffer. This means that its value in that buffer is independent of its
value in other buffers. A few variables are always local in every
buffer. Every other Emacs variable has a *global* value which is in
effect in all buffers that have not made the variable local.

`M-x make-local-variable` reads the name of a variable and makes it
local to the current buffer. Further changes in this buffer will not
affect others, and further changes in the global value will not affect this
buffer.

`M-x make-variable-buffer-local` reads the name of a variable and
changes the future behavior of the variable so that it will become local
automatically when it is set. More precisely, once a variable has been
marked in this way, the usual ways of setting the variable automatically
do `make-local-variable` first. We call such variables
*per-buffer* variables.

Major modes (see section [R. Major Modes](emacs.html#SEC227)) always make variables local to the
buffer before setting the variables. This is why changing major modes
in one buffer has no effect on other buffers. Minor modes also work by
setting variables--normally, each minor mode has one controlling
variable which is non-`nil` when the mode is enabled (see section [AD.1 Minor Modes](emacs.html#SEC476)). For most minor modes, the controlling variable is per buffer.

Emacs contains a number of variables that are always per-buffer.
These include `abbrev-mode`, `auto-fill-function`,
`case-fold-search`, `comment-column`, `ctl-arrow`,
`fill-column`, `fill-prefix`, `indent-tabs-mode`,
`left-margin`, `mode-line-format`, `overwrite-mode`,
`selective-display-ellipses`, `selective-display`,
`tab-width`, and `truncate-lines`. Some other variables are
always local in every buffer, but they are used for internal
purposes.

A few variables cannot be local to a buffer because they are always
local to each display instead (see section [P.10 Multiple Displays](emacs.html#SEC201)). If you try to
make one of these variables buffer-local, you'll get an error message.

`M-x kill-local-variable` reads the name of a variable and makes
it cease to be local to the current buffer. The global value of the
variable henceforth is in effect in this buffer. Setting the major mode
kills all the local variables of the buffer except for a few variables
specially marked as *permanent locals*.

To set the global value of a variable, regardless of whether the
variable has a local value in the current buffer, you can use the Lisp
construct `setq-default`. This construct is used just like
`setq`, but it sets variables' global values instead of their local
values (if any). When the current buffer does have a local value, the
new global value may not be visible until you switch to another buffer.
Here is an example:

|  |  |
| --- | --- |
|  | ``` (setq-default fill-column 75) ``` |

`setq-default` is the only way to set the global value of a variable
that has been marked with `make-variable-buffer-local`.

Lisp programs can use `default-value` to look at a variable's
default value. This function takes a symbol as argument and returns its
default value. The argument is evaluated; usually you must quote it
explicitly. For example, here's how to obtain the default value of
`fill-column`:

|  |  |
| --- | --- |
|  | ``` (default-value 'fill-column) ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC485) ] | [ [>](emacs.html#SEC487) ] | [ [<<](emacs.html#SEC487) ] | [ [Up](emacs.html#SEC477) ] | [ [>>](emacs.html#SEC487) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.2.5 Local Variables in Files

A file can specify local variable values for use when you edit the
file with Emacs. Visiting the file checks for local variable
specifications; it automatically makes these variables local to the
buffer, and sets them to the values specified in the file.

There are two ways to specify local variable values: in the first
line, or with a local variables list. Here's how to specify them in the
first line:

|  |  |
| --- | --- |
|  | ``` -*- mode: modename; var: value; ... -*- ``` |

You can specify any number of variables/value pairs in this way, each
pair with a colon and semicolon as shown above. `mode:
modename;` specifies the major mode; this should come first in the
line. The values are not evaluated; they are used literally.
Here is an example that specifies Lisp mode and sets two variables with
numeric values:

|  |  |
| --- | --- |
|  | ``` ;; -*- mode: Lisp; fill-column: 75; comment-column: 50; -*- ``` |

You can also specify the coding system for a file in this way: just
specify a value for the "variable" named `coding`. The "value"
must be a coding system name that Emacs recognizes. See section [Q.7 Coding Systems](emacs.html#SEC220).

The `eval` pseudo-variable, described below, can be specified in
the first line as well.

In shell scripts, the first line is used to identify the script
interpreter, so you cannot put any local variables there. To accommodate
for this, when Emacs visits a shell script, it looks for local variable
specifications in the *second* line.

A *local variables list* goes near the end of the file, in the
last page. (It is often best to put it on a page by itself.) The local
variables list starts with a line containing the string `` `Local
Variables:' ``, and ends with a line containing the string `` `End:' ``. In
between come the variable names and values, one set per line, as
`` `variable: value' ``. The values are not
evaluated; they are used literally. If a file has both a local
variables list and a `` `-*-' `` line, Emacs processes *everything*
in the `` `-*-' `` line first, and *everything* in the local
variables list afterward.

Here is an example of a local variables list:

|  |  |
| --- | --- |
|  | ``` ;;; Local Variables: *** ;;; mode:lisp *** ;;; comment-column:0 *** ;;; comment-start: ";;; "  *** ;;; comment-end:"***" *** ;;; End: *** ``` |

As you see, each line starts with the prefix `` `;;; ' `` and each line
ends with the suffix `` ` ***' ``. Emacs recognizes these as the prefix
and suffix based on the first line of the list, by finding them
surrounding the magic string `` `Local Variables:' ``; then it
automatically discards them from the other lines of the list.

The usual reason for using a prefix and/or suffix is to embed the
local variables list in a comment, so it won't confuse other programs
that the file is intended as input for. The example above is for a
language where comment lines start with `` `;;; ' `` and end with
`` `***' ``; the local values for `comment-start` and
`comment-end` customize the rest of Emacs for this unusual syntax.
Don't use a prefix (or a suffix) if you don't need one.

Two "variable names" have special meanings in a local variables
list: a value for the variable `mode` really sets the major mode,
and a value for the variable `eval` is simply evaluated as an
expression and the value is ignored. `mode` and `eval` are
not real variables; setting variables named `mode` and `eval`
in any other context has no special meaning. *If `mode` is
used to set a major mode, it should be the first "variable" in the
list.* Otherwise, the entries that precede it in the list of the local
variables are likely to be ignored, since most modes kill all local
variables as part of their initialization.

You can use the `mode` "variable" to set minor modes as well as
major modes; in fact, you can use it more than once, first to set the
major mode and then to set minor modes which are specific to particular
buffers. But most minor modes should not be specified in the file in
any fashion, because they represent user preferences.

For example, you may be tempted to try to turn on Auto Fill mode with
a local variable list. That is a mistake. The choice of Auto Fill mode
or not is a matter of individual taste, not a matter of the contents of
particular files. If you want to use Auto Fill, set up major mode hooks
with your `.emacs' file to turn it on (when appropriate) for you
alone (see section [AD.7 The Init File, `~/.emacs'](emacs.html#SEC505)). Don't use a local variable list to impose
your taste on everyone.

The start of the local variables list must be no more than 3000
characters from the end of the file, and must be in the last page if the
file is divided into pages. Otherwise, Emacs will not notice it is
there. The purpose of this rule is so that a stray `` `Local
Variables:' `` not in the last page does not confuse Emacs, and so that
visiting a long file that is all one page and has no local variables
list need not take the time to search the whole file.

Use the command `normal-mode` to reset the local variables and
major mode of a buffer according to the file name and contents,
including the local variables list if any. See section [R.1 How Major Modes are Chosen](emacs.html#SEC228).

The variable `enable-local-variables` controls whether to process
local variables in files, and thus gives you a chance to override them.
Its default value is `t`, which means do process local variables in
files. If you set the value to `nil`, Emacs simply ignores local
variables in files. Any other value says to query you about each file
that has local variables, showing you the local variable specifications
so you can judge.

The `eval` "variable," and certain actual variables, create a
special risk; when you visit someone else's file, local variable
specifications for these could affect your Emacs in arbitrary ways.
Therefore, the option `enable-local-eval` controls whether Emacs
processes `eval` variables, as well variables with names that end
in `` `-hook' ``, `` `-hooks' ``, `` `-function' `` or `` `-functions' ``,
and certain other variables. The three possibilities for the option's
value are `t`, `nil`, and anything else, just as for
`enable-local-variables`. The default is `maybe`, which is
neither `t` nor `nil`, so normally Emacs does ask for
confirmation about file settings for these variables.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC486) ] | [ [>](emacs.html#SEC488) ] | [ [<<](emacs.html#SEC491) ] | [ [Up](emacs.html#SEC475) ] | [ [>>](emacs.html#SEC491) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AD.3 Keyboard Macros

A *keyboard macro* is a command defined by the user to stand for
another sequence of keys. For example, if you discover that you are
about to type `C-n C-d` forty times, you can speed your work by
defining a keyboard macro to do `C-n C-d` and calling it with a
repeat count of forty.

`C-x (`: Start defining a keyboard macro (`start-kbd-macro`). `C-x )`: End the definition of a keyboard macro (`end-kbd-macro`). `C-x e`: Execute the most recent keyboard macro (`call-last-kbd-macro`). `C-u C-x (`: Re-execute last keyboard macro, then add more keys to its definition. `C-x q`: When this point is reached during macro execution, ask for confirmation (`kbd-macro-query`). `M-x name-last-kbd-macro`: Give a command name (for the duration of the session) to the most recently defined keyboard macro. `M-x insert-kbd-macro`: Insert in the buffer a keyboard macro's definition, as Lisp code. `C-x C-k`: Edit a previously defined keyboard macro (`edit-kbd-macro`). `M-x apply-macro-to-region-lines`: Run the last keyboard macro on each complete line in the region.

Keyboard macros differ from ordinary Emacs commands in that they are
written in the Emacs command language rather than in Lisp. This makes it
easier for the novice to write them, and makes them more convenient as
temporary hacks. However, the Emacs command language is not powerful
enough as a programming language to be useful for writing anything
intelligent or general. For such things, Lisp must be used.

You define a keyboard macro while executing the commands which are the
definition. Put differently, as you define a keyboard macro, the
definition is being executed for the first time. This way, you can see
what the effects of your commands are, so that you don't have to figure
them out in your head. When you are finished, the keyboard macro is
defined and also has been, in effect, executed once. You can then do the
whole thing over again by invoking the macro.

> |  |  |  |
> | --- | --- | --- |
> | [AD.3.1 Basic Use](emacs.html#SEC488) |  | Defining and running keyboard macros. |
> | [AD.3.2 Naming and Saving Keyboard Macros](emacs.html#SEC489) |  | Giving keyboard macros names; saving them in files. |
> | [AD.3.3 Executing Macros with Variations](emacs.html#SEC490) |  | Making keyboard macros do different things each time. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC487) ] | [ [>](emacs.html#SEC489) ] | [ [<<](emacs.html#SEC491) ] | [ [Up](emacs.html#SEC487) ] | [ [>>](emacs.html#SEC491) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.3.1 Basic Use

To start defining a keyboard macro, type the `C-x (` command
(`start-kbd-macro`). From then on, your keys continue to be
executed, but also become part of the definition of the macro. `` `Def' ``
appears in the mode line to remind you of what is going on. When you are
finished, the `C-x )` command (`end-kbd-macro`) terminates the
definition (without becoming part of it!). For example,

|  |  |
| --- | --- |
|  | ``` C-x ( M-f foo C-x ) ``` |

defines a macro to move forward a word and then insert `` `foo' ``.

The macro thus defined can be invoked again with the `C-x e`
command (`call-last-kbd-macro`), which may be given a repeat count
as a numeric argument to execute the macro many times. `C-x )` can
also be given a repeat count as an argument, in which case it repeats
the macro that many times right after defining it, but defining the
macro counts as the first repetition (since it is executed as you define
it). Therefore, giving `C-x )` an argument of 4 executes the macro
immediately 3 additional times. An argument of zero to `C-x e` or
`C-x )` means repeat the macro indefinitely (until it gets an error
or you type `C-g` or, on MS-DOS, `C-BREAK`).

If you wish to repeat an operation at regularly spaced places in the
text, define a macro and include as part of the macro the commands to move
to the next place you want to use it. For example, if you want to change
each line, you should position point at the start of a line, and define a
macro to change that line and leave point at the start of the next line.
Then repeating the macro will operate on successive lines.

When a command reads an argument with the minibuffer, your
minibuffer input becomes part of the macro along with the command. So
when you replay the macro, the command gets the same argument as
when you entered the macro. For example,

|  |  |
| --- | --- |
|  | ``` C-x ( C-a C-SPC C-n M-w C-x b f o o RET C-y C-x b RET C-x ) ``` |

defines a macro that copies the current line into the buffer
`` `foo' ``, then returns to the original buffer.

You can use function keys in a keyboard macro, just like keyboard
keys. You can even use mouse events, but be careful about that: when
the macro replays the mouse event, it uses the original mouse position
of that event, the position that the mouse had while you were defining
the macro. The effect of this may be hard to predict. (Using the
current mouse position would be even less predictable.)

One thing that doesn't always work well in a keyboard macro is the
command `C-M-c` (`exit-recursive-edit`). When this command
exits a recursive edit that started within the macro, it works as you'd
expect. But if it exits a recursive edit that started before you
invoked the keyboard macro, it also necessarily exits the keyboard macro
as part of the process.

After you have terminated the definition of a keyboard macro, you can add
to the end of its definition by typing `C-u C-x (`. This is equivalent
to plain `C-x (` followed by retyping the whole definition so far. As
a consequence it re-executes the macro as previously defined.

You can edit a keyboard macro already defined by typing `C-x C-k`
(`edit-kbd-macro`). Follow that with the keyboard input that you
would use to invoke the macro---`C-x e` or `M-x name` or
some other key sequence. This formats the macro definition in a buffer
and enters a specialized major mode for editing it. Type `C-h m`
once in that buffer to display details of how to edit the macro. When
you are finished editing, type `C-c C-c`.

The command `M-x apply-macro-to-region-lines` repeats the last
defined keyboard macro on each complete line within the current region.
It does this line by line, by moving point to the beginning of the line
and then executing the macro.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC488) ] | [ [>](emacs.html#SEC490) ] | [ [<<](emacs.html#SEC490) ] | [ [Up](emacs.html#SEC487) ] | [ [>>](emacs.html#SEC491) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.3.2 Naming and Saving Keyboard Macros

If you wish to save a keyboard macro for longer than until you define the
next one, you must give it a name using `M-x name-last-kbd-macro`.
This reads a name as an argument using the minibuffer and defines that name
to execute the macro. The macro name is a Lisp symbol, and defining it in
this way makes it a valid command name for calling with `M-x` or for
binding a key to with `global-set-key` (see section [AD.4.1 Keymaps](emacs.html#SEC492)). If you
specify a name that has a prior definition other than another keyboard
macro, an error message is shown and nothing is changed.

Once a macro has a command name, you can save its definition in a file.
Then it can be used in another editing session. First, visit the file
you want to save the definition in. Then use this command:

|  |  |
| --- | --- |
|  | ``` M-x insert-kbd-macro RET macroname RET ``` |

This inserts some Lisp code that, when executed later, will define the
same macro with the same definition it has now. (You need not
understand Lisp code to do this, because `insert-kbd-macro` writes
the Lisp code for you.) Then save the file. You can load the file
later with `load-file` (see section [V.7 Libraries of Lisp Code for Emacs](emacs.html#SEC328)). If the file you
save in is your init file `~/.emacs' (see section [AD.7 The Init File, `~/.emacs'](emacs.html#SEC505)) then the
macro will be defined each time you run Emacs.

If you give `insert-kbd-macro` a numeric argument, it makes
additional Lisp code to record the keys (if any) that you have bound to the
keyboard macro, so that the macro will be reassigned the same keys when you
load the file.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC489) ] | [ [>](emacs.html#SEC491) ] | [ [<<](emacs.html#SEC491) ] | [ [Up](emacs.html#SEC487) ] | [ [>>](emacs.html#SEC491) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.3.3 Executing Macros with Variations

Using `C-x q` (`kbd-macro-query`), you can get an effect
similar to that of `query-replace`, where the macro asks you each
time around whether to make a change. While defining the macro,
type `C-x q` at the point where you want the query to occur. During
macro definition, the `C-x q` does nothing, but when you run the
macro later, `C-x q` asks you interactively whether to continue.

The valid responses when `C-x q` asks are `SPC` (or `y`),
`DEL` (or `n`), `RET` (or `q`), `C-l` and `C-r`.
The answers are the same as in `query-replace`, though not all of
the `query-replace` options are meaningful.

These responses include `SPC` to continue, and `DEL` to skip
the remainder of this repetition of the macro and start right away with
the next repetition. `RET` means to skip the remainder of this
repetition and cancel further repetitions. `C-l` redraws the screen
and asks you again for a character to say what to do.

`C-r` enters a recursive editing level, in which you can perform
editing which is not part of the macro. When you exit the recursive
edit using `C-M-c`, you are asked again how to continue with the
keyboard macro. If you type a `SPC` at this time, the rest of the
macro definition is executed. It is up to you to leave point and the
text in a state such that the rest of the macro will do what you
want.

`C-u C-x q`, which is `C-x q` with a numeric argument,
performs a completely different function. It enters a recursive edit
reading input from the keyboard, both when you type it during the
definition of the macro, and when it is executed from the macro. During
definition, the editing you do inside the recursive edit does not become
part of the macro. During macro execution, the recursive edit gives you
a chance to do some particularized editing on each repetition.
See section [AC.26 Recursive Editing Levels](emacs.html#SEC466).

Another way to vary the behavior of a keyboard macro is to use a
register as a counter, incrementing it on each repetition of the macro.
See section [I.5 Keeping Numbers in Registers](emacs.html#SEC74).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC490) ] | [ [>](emacs.html#SEC492) ] | [ [<<](emacs.html#SEC503) ] | [ [Up](emacs.html#SEC475) ] | [ [>>](emacs.html#SEC503) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AD.4 Customizing Key Bindings

This section describes *key bindings*, which map keys to commands,
and *keymaps*, which record key bindings. It also explains how
to customize key bindings.

Recall that a command is a Lisp function whose definition provides for
interactive use. Like every Lisp function, a command has a function
name which usually consists of lower-case letters and hyphens.

> |  |  |  |
> | --- | --- | --- |
> | [AD.4.1 Keymaps](emacs.html#SEC492) |  | Generalities. The global keymap. |
> | [AD.4.2 Prefix Keymaps](emacs.html#SEC493) |  | Keymaps for prefix keys. |
> | [AD.4.3 Local Keymaps](emacs.html#SEC494) |  | Major and minor modes have their own keymaps. |
> | [AD.4.4 Minibuffer Keymaps](emacs.html#SEC495) |  | The minibuffer uses its own local keymaps. |
> | [AD.4.5 Changing Key Bindings Interactively](emacs.html#SEC496) |  | How to redefine one key's meaning conveniently. |
> | [AD.4.6 Rebinding Keys in Your Init File](emacs.html#SEC497) |  | Rebinding keys with your init file, `.emacs'. |
> | [AD.4.7 Rebinding Function Keys](emacs.html#SEC498) |  | Rebinding terminal function keys. |
> | [AD.4.8 Named ASCII Control Characters](emacs.html#SEC499) |  | Distinguishing `TAB` from `C-i`, and so on. |
> | [AD.4.9 Non-ASCII Characters on the Keyboard](emacs.html#SEC500) |  | Rebinding non-ASCII characters such as Latin-1. |
> | [AD.4.10 Rebinding Mouse Buttons](emacs.html#SEC501) |  | Rebinding mouse buttons in Emacs. |
> | [AD.4.11 Disabling Commands](emacs.html#SEC502) |  | Disabling a command means confirmation is required before it can be executed. This is done to protect beginners from surprises. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC491) ] | [ [>](emacs.html#SEC493) ] | [ [<<](emacs.html#SEC503) ] | [ [Up](emacs.html#SEC491) ] | [ [>>](emacs.html#SEC503) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.4.1 Keymaps

The bindings between key sequences and command functions are recorded
in data structures called *keymaps*. Emacs has many of these, each
used on particular occasions.

Recall that a *key sequence* (*key*, for short) is a sequence
of *input events* that have a meaning as a unit. Input events
include characters, function keys and mouse buttons--all the inputs
that you can send to the computer with your terminal. A key sequence
gets its meaning from its *binding*, which says what command it
runs. The function of keymaps is to record these bindings.

The *global* keymap is the most important keymap because it is
always in effect. The global keymap defines keys for Fundamental mode;
most of these definitions are common to most or all major modes. Each
major or minor mode can have its own keymap which overrides the global
definitions of some keys.

For example, a self-inserting character such as `g` is
self-inserting because the global keymap binds it to the command
`self-insert-command`. The standard Emacs editing characters such
as `C-a` also get their standard meanings from the global keymap.
Commands to rebind keys, such as `M-x global-set-key`, actually work
by storing the new binding in the proper place in the global map.
See section [AD.4.5 Changing Key Bindings Interactively](emacs.html#SEC496).

Meta characters work differently; Emacs translates each Meta
character into a pair of characters starting with `ESC`. When you
type the character `M-a` in a key sequence, Emacs replaces it with
`ESC a`. A meta key comes in as a single input event, but
becomes two events for purposes of key bindings. The reason for this is
historical, and we might change it someday.

Most modern keyboards have function keys as well as character keys.
Function keys send input events just as character keys do, and keymaps
can have bindings for them.

On many terminals, typing a function key actually sends the computer a
sequence of characters; the precise details of the sequence depends on
which function key and on the model of terminal you are using. (Often
the sequence starts with `ESC [`.) If Emacs understands your
terminal type properly, it recognizes the character sequences forming
function keys wherever they occur in a key sequence (not just at the
beginning). Thus, for most purposes, you can pretend the function keys
reach Emacs directly and ignore their encoding as character sequences.

Mouse buttons also produce input events. These events come with other
data--the window and position where you pressed or released the button,
and a time stamp. But only the choice of button matters for key
bindings; the other data matters only if a command looks at it.
(Commands designed for mouse invocation usually do look at the other
data.)

A keymap records definitions for single events. Interpreting a key
sequence of multiple events involves a chain of keymaps. The first
keymap gives a definition for the first event; this definition is
another keymap, which is used to look up the second event in the
sequence, and so on.

Key sequences can mix function keys and characters. For example,
`C-x SELECT` is meaningful. If you make `SELECT` a prefix
key, then `SELECT C-n` makes sense. You can even mix mouse
events with keyboard events, but we recommend against it, because such
key sequences are inconvenient to use.

As a user, you can redefine any key; but it is usually best to stick
to key sequences that consist of `C-c` followed by a letter.
These keys are "reserved for users," so they won't conflict with any
properly designed Emacs extension. The function keys `F5` through
`F9` are also reserved for users. If you redefine some other key,
your definition may be overridden by certain extensions or major modes
which redefine the same key.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC492) ] | [ [>](emacs.html#SEC494) ] | [ [<<](emacs.html#SEC494) ] | [ [Up](emacs.html#SEC491) ] | [ [>>](emacs.html#SEC503) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.4.2 Prefix Keymaps

A prefix key such as `C-x` or `ESC` has its own keymap,
which holds the definition for the event that immediately follows
that prefix.

The definition of a prefix key is usually the keymap to use for
looking up the following event. The definition can also be a Lisp
symbol whose function definition is the following keymap; the effect is
the same, but it provides a command name for the prefix key that can be
used as a description of what the prefix key is for. Thus, the binding
of `C-x` is the symbol `Ctl-X-Prefix`, whose function
definition is the keymap for `C-x` commands. The definitions of
`C-c`, `C-x`, `C-h` and `ESC` as prefix keys appear in
the global map, so these prefix keys are always available.

Aside from ordinary prefix keys, there is a fictitious "prefix key"
which represents the menu bar; see section `Menu Bar' in The Emacs Lisp Reference Manual, for special information about menu bar key bindings.
Mouse button events that invoke pop-up menus are also prefix keys; see
section `Menu Keymaps' in The Emacs Lisp Reference Manual, for more
details.

Some prefix keymaps are stored in variables with names:

* `ctl-x-map` is the variable name for the map used for characters that
  follow `C-x`.* `help-map` is for characters that follow `C-h`.* `esc-map` is for characters that follow `ESC`. Thus, all Meta
      characters are actually defined by this map.* `ctl-x-4-map` is for characters that follow `C-x 4`.* `mode-specific-map` is for characters that follow `C-c`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC493) ] | [ [>](emacs.html#SEC495) ] | [ [<<](emacs.html#SEC495) ] | [ [Up](emacs.html#SEC491) ] | [ [>>](emacs.html#SEC503) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.4.3 Local Keymaps

So far we have explained the ins and outs of the global map. Major
modes customize Emacs by providing their own key bindings in *local
keymaps*. For example, C mode overrides `TAB` to make it indent the
current line for C code. Portions of text in the buffer can specify
their own keymaps to substitute for the keymap of the buffer's major
mode.

Minor modes can also have local keymaps. Whenever a minor mode is
in effect, the definitions in its keymap override both the major
mode's local keymap and the global keymap.

The local keymaps for Lisp mode and several other major modes always
exist even when not in use. These are kept in variables named
`lisp-mode-map` and so on. For major modes less often used, the
local keymap is normally constructed only when the mode is used for the
first time in a session. This is to save space. If you wish to change
one of these keymaps, you must use the major mode's *mode
hook*---see below.

All minor mode keymaps are created in advance. There is no way to
defer their creation until the first time the minor mode is enabled.

A local keymap can locally redefine a key as a prefix key by defining
it as a prefix keymap. If the key is also defined globally as a prefix,
then its local and global definitions (both keymaps) effectively
combine: both of them are used to look up the event that follows the
prefix key. Thus, if the mode's local keymap defines `C-c` as
another keymap, and that keymap defines `C-z` as a command, this
provides a local meaning for `C-c C-z`. This does not affect other
sequences that start with `C-c`; if those sequences don't have their
own local bindings, their global bindings remain in effect.

Another way to think of this is that Emacs handles a multi-event key
sequence by looking in several keymaps, one by one, for a binding of the
whole key sequence. First it checks the minor mode keymaps for minor
modes that are enabled, then it checks the major mode's keymap, and then
it checks the global keymap. This is not precisely how key lookup
works, but it's good enough for understanding ordinary circumstances.

To change the local bindings of a major mode, you must change the
mode's local keymap. Normally you must wait until the first time the
mode is used, because most major modes don't create their keymaps until
then. If you want to specify something in your `~/.emacs' file to
change a major mode's bindings, you must use the mode's mode hook to
delay the change until the mode is first used.

For example, the command `texinfo-mode` to select Texinfo mode
runs the hook `texinfo-mode-hook`. Here's how you can use the hook
to add local bindings (not very useful, we admit) for `C-c n` and
`C-c p` in Texinfo mode:

|  |  |
| --- | --- |
|  | ``` (add-hook 'texinfo-mode-hook           '(lambda ()              (define-key texinfo-mode-map "\C-cp"                          'backward-paragraph)              (define-key texinfo-mode-map "\C-cn"                          'forward-paragraph))) ``` |

See section [AD.2.3 Hooks](emacs.html#SEC484).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC494) ] | [ [>](emacs.html#SEC496) ] | [ [<<](emacs.html#SEC496) ] | [ [Up](emacs.html#SEC491) ] | [ [>>](emacs.html#SEC503) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.4.4 Minibuffer Keymaps

The minibuffer has its own set of local keymaps; they contain various
completion and exit commands.

* `minibuffer-local-map` is used for ordinary input (no completion).* `minibuffer-local-ns-map` is similar, except that `SPC` exits
    just like `RET`. This is used mainly for Mocklisp compatibility.* `minibuffer-local-completion-map` is for permissive completion.* `minibuffer-local-must-match-map` is for strict completion and
        for cautious completion.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC495) ] | [ [>](emacs.html#SEC497) ] | [ [<<](emacs.html#SEC497) ] | [ [Up](emacs.html#SEC491) ] | [ [>>](emacs.html#SEC503) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
