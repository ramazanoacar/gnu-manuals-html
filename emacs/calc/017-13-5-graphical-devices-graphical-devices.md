### 13.5 Graphical Devices [¶](#Graphical-Devices)

The `g D` (`calc-graph-device`) command sets the device name
(or “terminal name” in GNUPLOT lingo) to be used by `g p` commands
on this graph. It does not affect the permanent default device name.
If you enter a blank name, the device name reverts to the default.
Enter ‘`?`’ to see a list of supported devices.

With a positive numeric prefix argument, `g D` instead sets
the default device name, used by all plots in the future which do
not override it with a plain `g D` command. If you enter a
blank line this command shows you the current default. The special
name `default` signifies that Calc should choose `x11` if
the X window system is in use (as indicated by the presence of a
`DISPLAY` environment variable), `windows` on MS-Windows, or
otherwise `dumb` under GNUPLOT 3.0 and later, or
`postscript` under GNUPLOT 2.0. This is the initial default
value.

The `dumb` device is an interface to “dumb terminals,” i.e.,
terminals with no special graphics facilities. It writes a crude
picture of the graph composed of characters like `-` and `|`
to a buffer called `*Gnuplot Trail*`, which Calc then displays.
The graph is made the same size as the Emacs screen, which on most
dumb terminals will be
80x24
characters. The graph is displayed in
an Emacs “recursive edit”; type `q` or `C-c C-c` to exit
the recursive edit and return to Calc. Note that the `dumb`
device is present only in GNUPLOT 3.0 and later versions.

The word `dumb` may be followed by two numbers separated by
spaces. These are the desired width and height of the graph in
characters. Also, the device name `big` is like `dumb`
but creates a graph four times the width and height of the Emacs
screen. You will then have to scroll around to view the entire
graph. In the `*Gnuplot Trail*` buffer, `SPC`, `DEL`,
`<`, and `>` are defined to scroll by one screenful in each
of the four directions.

With a negative numeric prefix argument, `g D` sets or displays
the device name used by `g P` (`calc-graph-print`). This
is initially `postscript`. If you don’t have a PostScript
printer, you may decide once again to use `dumb` to create a
plot on any text-only printer.

The `g O` (`calc-graph-output`) command sets the name of the
output file used by GNUPLOT. For some devices, notably `x11` and
`windows`, there is no output file and this information is not
used. Many other “devices” are really file formats like
`postscript`; in these cases the output in the desired format
goes into the file you name with `g O`. Type `g O stdout
RET` to set GNUPLOT to write to its standard output stream,
i.e., to `*Gnuplot Trail*`. This is the default setting.

Another special output name is `tty`, which means that GNUPLOT
is going to write graphics commands directly to its standard output,
which you wish Emacs to pass through to your terminal. Tektronix
graphics terminals, among other devices, operate this way. Calc does
this by telling GNUPLOT to write to a temporary file, then running a
sub-shell executing the command ‘`cat tempfile >/dev/tty`’. On
typical Unix systems, this will copy the temporary file directly to
the terminal, bypassing Emacs entirely. You will have to type `C-l`
to Emacs afterwards to refresh the screen.

Once again, `g O` with a positive or negative prefix argument
sets the default or printer output file names, respectively. In each
case you can specify `auto`, which causes Calc to invent a temporary
file name for each `g p` (or `g P`) command. This temporary file
will be deleted once it has been displayed or printed. If the output file
name is not `auto`, the file is not automatically deleted.

The default and printer devices and output files can be saved
permanently by the `m m` (`calc-save-modes`) command. The
default number of data points (see `g N`) and the X geometry
(see `g X`) are also saved. Other graph information is *not*
saved; you can save a graph’s configuration simply by saving the contents
of the `*Gnuplot Commands*` buffer.

You may wish to configure the default and
printer devices and output files for the whole system. The relevant
Lisp variables are `calc-gnuplot-default-device` and `-output`,
and `calc-gnuplot-print-device` and `-output`. The output
file names must be either strings as described above, or Lisp
expressions which are evaluated on the fly to get the output file names.

Other important Lisp variables are `calc-gnuplot-plot-command` and
`calc-gnuplot-print-command`, which give the system commands to
display or print the output of GNUPLOT, respectively. These may be
`nil` if no command is necessary, or strings which can include
‘`%s`’ to signify the name of the file to be displayed or printed.
Or, these variables may contain Lisp expressions which are evaluated
to display or print the output. These variables are customizable
(see [Customizing Calc](#Customizing-Calc)).

The `g x` (`calc-graph-display`) command lets you specify
on which X window system display your graphs should be drawn. Enter
a blank line to see the current display name. This command has no
effect unless the current device is `x11`.

The `g X` (`calc-graph-geometry`) command is a similar
command for specifying the position and size of the X window.
The normal value is `default`, which generally means your
window manager will let you place the window interactively.
Entering ‘`800x500+0+0`’ would create an 800-by-500 pixel
window in the upper-left corner of the screen. This command has no
effect if the current device is `windows`.

The buffer called `*Gnuplot Trail*` holds a transcript of the
session with GNUPLOT. This shows the commands Calc has “typed” to
GNUPLOT and the responses it has received. Calc tries to notice when an
error message has appeared here and display the buffer for you when
this happens. You can check this buffer yourself if you suspect
something has gone wrong[1](#FOOT1).

The `g C` (`calc-graph-command`) command prompts you to
enter any line of text, then simply sends that line to the current
GNUPLOT process. The `*Gnuplot Trail*` buffer looks deceptively
like a Shell buffer but you can’t type commands in it yourself.
Instead, you must use `g C` for this purpose.

The `g v` (`calc-graph-view-commands`) and `g V`
(`calc-graph-view-trail`) commands display the `*Gnuplot Commands*`
and `*Gnuplot Trail*` buffers, respectively, in another window.
This happens automatically when Calc thinks there is something you
will want to see in either of these buffers. If you type `g v`
or `g V` when the relevant buffer is already displayed, the
buffer is hidden again. (Note that on MS-Windows, the `*Gnuplot
Trail*` buffer will usually show nothing of interest, because
GNUPLOT’s responses are not communicated back to Calc.)

One reason to use `g v` is to add your own commands to the
`*Gnuplot Commands*` buffer. Press `g v`, then use
`C-x o` to switch into that window. For example, GNUPLOT has
‘`set label`’ and ‘`set arrow`’ commands that allow you to
annotate your plots. Since Calc doesn’t understand these commands,
you have to add them to the `*Gnuplot Commands*` buffer
yourself, then use `g p` to replot using these new commands. Note
that your commands must appear *before* the `plot` command.
To get help on any GNUPLOT feature, type, e.g., `g C help set label`.
You may have to type `g C RET` a few times to clear the
“press return for more” or “subtopic of …” requests.
Note that Calc always sends commands (like ‘`set nolabel`’) to
reset all plotting parameters to the defaults before each plot, so
to delete a label all you need to do is delete the ‘`set label`’
line you added (or comment it out with ‘`#`’) and then replot
with `g p`.

You can use `g q` (`calc-graph-quit`) to kill the GNUPLOT
process that is running. The next graphing command you give will
start a fresh GNUPLOT process. The word ‘`Graph`’ appears in
the Calc window’s mode line whenever a GNUPLOT process is currently
running. The GNUPLOT process is automatically killed when you
exit Emacs if you haven’t killed it manually by then.

The `g K` (`calc-graph-kill`) command is like `g q`
except that it also views the `*Gnuplot Trail*` buffer so that
you can see the process being killed. This is better if you are
killing GNUPLOT because you think it has gotten stuck.

---

Next: [Keypad Mode](#Keypad-Mode), Previous: [Graphics](#Graphics), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 14 Kill and Yank Functions [¶](#Kill-and-Yank-Functions)

The commands in this chapter move information between the Calculator and
other Emacs editing buffers.

In many cases Embedded mode is an easier and more natural way to
work with Calc from a regular editing buffer. See [Embedded Mode](#Embedded-Mode).

* [Killing from the Stack](#Killing-From-Stack)
* [Yanking into the Stack](#Yanking-Into-Stack)
* [Saving into Registers](#Saving-Into-Registers)
* [Inserting from Registers](#Inserting-From-Registers)
* [Grabbing from Other Buffers](#Grabbing-From-Buffers)
* [Yanking into Other Buffers](#Yanking-Into-Buffers)
* [X Cut and Paste](#X-Cut-and-Paste)

---

Next: [Yanking into the Stack](#Yanking-Into-Stack), Up: [Kill and Yank Functions](#Kill-and-Yank)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 14.1 Killing from the Stack [¶](#Killing-from-the-Stack)

*Kill* commands are Emacs commands that insert text into the “kill
ring,” from which it can later be “yanked” by a `C-y` command.
Three common kill commands in normal Emacs are `C-k`, which kills
one line, `C-w`, which kills the region between mark and point, and
`M-w`, which puts the region into the kill ring without actually
deleting it. All of these commands work in the Calculator, too,
although in the Calculator they operate on whole stack entries, so they
“round up” the specified region to encompass full lines. (To copy
only parts of lines, the `M-C-w` command in the Calculator will copy
the region to the kill ring without any “rounding up”, just like the
`M-w` command in normal Emacs.) Also, `M-k` has been provided
to complete the set; it puts the current line into the kill ring without
deleting anything.

The kill commands are unusual in that they pay attention to the location
of the cursor in the Calculator buffer. If the cursor is on or below
the bottom line, the kill commands operate on the top of the stack.
Otherwise, they operate on whatever stack element the cursor is on. The
text is copied into the kill ring exactly as it appears on the screen,
including line numbers if they are enabled.

A numeric prefix argument to `C-k` or `M-k` affects the number
of lines killed. A positive argument kills the current line and ‘`n-1`’
lines below it. A negative argument kills the ‘`-n`’ lines above the
current line. Again this mirrors the behavior of the standard Emacs
`C-k` command. Although a whole line is always deleted, `C-k`
with no argument copies only the number itself into the kill ring, whereas
`C-k` with a prefix argument of 1 copies the number with its trailing
newline.

You can customize `calc-kill-line-numbering` to `nil` to
exclude line numbering from kills and copies made by `calc-kill`
and `calc-copy-as-kill`. This option does not affect calc kill
and copy commands which operate on the region, as that would not make
sense.

---

Next: [Saving into Registers](#Saving-Into-Registers), Previous: [Killing from the Stack](#Killing-From-Stack), Up: [Kill and Yank Functions](#Kill-and-Yank)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 14.2 Yanking into the Stack [¶](#Yanking-into-the-Stack)

The `C-y` command yanks the most recently killed text back into the
Calculator. It pushes this value onto the top of the stack regardless of
the cursor position. In general it re-parses the killed text as a number
or formula (or a list of these separated by commas or newlines). However if
the thing being yanked is something that was just killed from the Calculator
itself, its full internal structure is yanked. For example, if you have
set the floating-point display mode to show only four significant digits,
then killing and re-yanking 3.14159 (which displays as 3.142) will yank the
full 3.14159, even though yanking it into any other buffer would yank the
number in its displayed form, 3.142. (Since the default display modes
show all objects to their full precision, this feature normally makes no
difference.)

The `C-y` command can be given a prefix, which will interpret the
text being yanked with a different radix. If the text being yanked can be
interpreted as a binary, octal, hexadecimal, or decimal number, then a
prefix of `2`, `8`, `6` or `0` will have Calc
interpret the yanked text as a number in the appropriate base. For example,
if ‘`111`’ has just been killed and is yanked into Calc with a command
of `C-2 C-y`, then the number ‘`7`’ will be put on the stack.
If you use the plain prefix `C-u`, then you will be prompted for a
base to use, which can be any integer from 2 to 36. If Calc doesn’t
allow the text being yanked to be read in a different base (such as if
the text is an algebraic expression), then the prefix will have no
effect.

---

Next: [Inserting from Registers](#Inserting-From-Registers), Previous: [Yanking into the Stack](#Yanking-Into-Stack), Up: [Kill and Yank Functions](#Kill-and-Yank)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 14.3 Saving into Registers [¶](#Saving-into-Registers)

An alternative to killing and yanking stack entries is using
registers in Calc. Saving stack entries in registers is like
saving text in normal Emacs registers; although, like Calc’s kill
commands, register commands always operate on whole stack
entries.

Registers in Calc are places to store stack entries for later use;
each register is indexed by a single character. To store the current
region (rounded up, of course, to include full stack entries) into a
register, use the command `r s` (`calc-copy-to-register`).
You will then be prompted for a register to use, the next character
you type will be the index for the register. To store the region in
register r, the full command will be `r s r`. With an
argument, `C-u r s r`, the region being copied to the
register will be deleted from the Calc buffer.

It is possible to add additional stack entries to a register. The
command `M-x calc-append-to-register` will prompt for a register,
then add the stack entries in the region to the end of the register
contents. The command `M-x calc-prepend-to-register` will
similarly prompt for a register and add the stack entries in the
region to the beginning of the register contents. Both commands take
`C-u` arguments, which will cause the region to be deleted after being
added to the register.

---

Next: [Grabbing from Other Buffers](#Grabbing-From-Buffers), Previous: [Saving into Registers](#Saving-Into-Registers), Up: [Kill and Yank Functions](#Kill-and-Yank)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 14.4 Inserting from Registers [¶](#Inserting-from-Registers)

The command `r i` (`calc-insert-register`) will prompt for a
register, then insert the contents of that register into the
Calculator. If the contents of the register were placed there from
within Calc, then the full internal structure of the contents will be
inserted into the Calculator, otherwise whatever text is in the
register is reparsed and then inserted into the Calculator.

---

Next: [Yanking into Other Buffers](#Yanking-Into-Buffers), Previous: [Inserting from Registers](#Inserting-From-Registers), Up: [Kill and Yank Functions](#Kill-and-Yank)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 14.5 Grabbing from Other Buffers [¶](#Grabbing-from-Other-Buffers)

The `C-x * g` (`calc-grab-region`) command takes the text between
point and mark in the current buffer and attempts to parse it as a
vector of values. Basically, it wraps the text in vector brackets
‘`[ ]`’ unless the text already is enclosed in vector brackets,
then reads the text as if it were an algebraic entry. The contents
of the vector may be numbers, formulas, or any other Calc objects.
If the `C-x * g` command works successfully, it does an automatic
`C-x * c` to enter the Calculator buffer.

A numeric prefix argument grabs the specified number of lines around
point, ignoring the mark. A positive prefix grabs from point to the
‘`n`’th following newline (so that `M-1 C-x * g` grabs from point
to the end of the current line); a negative prefix grabs from point
back to the ‘`n+1`’st preceding newline. In these cases the text
that is grabbed is exactly the same as the text that `C-k` would
delete given that prefix argument.

A prefix of zero grabs the current line; point may be anywhere on the
line.

A plain `C-u` prefix interprets the region between point and mark
as a single number or formula rather than a vector. For example,
`C-x * g` on the text ‘`2 a b`’ produces the vector of three
values ‘`[2, a, b]`’, but `C-u C-x * g` on the same region
reads a formula which is a product of three things: ‘`2 a b`’.
(The text ‘`a + b`’, on the other hand, will be grabbed as a
vector of one element by plain `C-x * g` because the interpretation
‘`[a, +, b]`’ would be a syntax error.)

If a different language has been specified (see [Language Modes](#Language-Modes)),
the grabbed text will be interpreted according to that language.

The `C-x * r` (`calc-grab-rectangle`) command takes the text between
point and mark and attempts to parse it as a matrix. If point and mark
are both in the leftmost column, the lines in between are parsed in their
entirety. Otherwise, point and mark define the corners of a rectangle
whose contents are parsed.

Each line of the grabbed area becomes a row of the matrix. The result
will actually be a vector of vectors, which Calc will treat as a matrix
only if every row contains the same number of values.

If a line contains a portion surrounded by square brackets (or curly
braces), that portion is interpreted as a vector which becomes a row
of the matrix. Any text surrounding the bracketed portion on the line
is ignored.

Otherwise, the entire line is interpreted as a row vector as if it
were surrounded by square brackets. Leading line numbers (in the
format used in the Calc stack buffer) are ignored. If you wish to
force this interpretation (even if the line contains bracketed
portions), give a negative numeric prefix argument to the
`C-x * r` command.

If you give a numeric prefix argument of zero or plain `C-u`, each
line is instead interpreted as a single formula which is converted into
a one-element vector. Thus the result of `C-u C-x * r` will be a
one-column matrix. For example, suppose one line of the data is the
expression ‘`2 a`’. A plain `C-x * r` will interpret this as
‘`[2 a]`’, which in turn is read as a two-element vector that forms
one row of the matrix. But a `C-u C-x * r` will interpret this row
as ‘`[2*a]`’.

If you give a positive numeric prefix argument n, then each line
will be split up into columns of width n; each column is parsed
separately as a matrix element. If a line contained
‘`2 +/- 3 4 +/- 5`’, then grabbing with a prefix argument of 8
would correctly split the line into two error forms.

See [Vector/Matrix Functions](#Matrix-Functions), to see how to pull the matrix apart into its
constituent rows and columns. (If it is a
1x1
matrix, just hit `v u` (`calc-unpack`) twice.)

The `C-x * :` (`calc-grab-sum-down`) command is a handy way to
grab a rectangle of data and sum its columns. It is equivalent to
typing `C-x * r`, followed by `V R : +` (the vector reduction
command that sums the columns of a matrix; see [Reducing](#Reducing)). The
result of the command will be a vector of numbers, one for each column
in the input data. The `C-x * _` (`calc-grab-sum-across`) command
similarly grabs a rectangle and sums its rows by executing `V R _ +`.

As well as being more convenient, `C-x * :` and `C-x * _` are also
much faster because they don’t actually place the grabbed vector on
the stack. In a `C-x * r V R : +` sequence, formatting the vector
for display on the stack takes a large fraction of the total time
(unless you have planned ahead and used `v .` and `t .` modes).

For example, suppose we have a column of numbers in a file which we
wish to sum. Go to one corner of the column and press `C-@` to
set the mark; go to the other corner and type `C-x * :`. Since there
is only one column, the result will be a vector of one number, the sum.
(You can type `v u` to unpack this vector into a plain number if
you want to do further arithmetic with it.)

To compute the product of the column of numbers, we would have to do
it “by hand” since there’s no special grab-and-multiply command.
Use `C-x * r` to grab the column of numbers into the calculator in
the form of a column matrix. The statistics command `u *` is a
handy way to find the product of a vector or matrix of numbers.
See [Statistical Operations on Vectors](#Statistical-Operations). Another approach would be to use
an explicit column reduction command, `V R : *`.

---

Next: [X Cut and Paste](#X-Cut-and-Paste), Previous: [Grabbing from Other Buffers](#Grabbing-From-Buffers), Up: [Kill and Yank Functions](#Kill-and-Yank)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 14.6 Yanking into Other Buffers [¶](#Yanking-into-Other-Buffers)

The plain `y` (`calc-copy-to-buffer`) command inserts the number
at the top of the stack into the most recently used normal editing buffer.
(More specifically, this is the most recently used buffer which is displayed
in a window and whose name does not begin with ‘`*`’. If there is no
such buffer, this is the most recently used buffer except for Calculator
and Calc Trail buffers.) The number is inserted exactly as it appears and
without a newline. (If line-numbering is enabled, the line number is
normally not included.) The number is *not* removed from the stack.

With a prefix argument, `y` inserts several numbers, one per line.
A positive argument inserts the specified number of values from the top
of the stack. A negative argument inserts the ‘`n`’th value from the
top of the stack. An argument of zero inserts the entire stack. Note
that `y` with an argument of 1 is slightly different from `y`
with no argument; the former always copies full lines, whereas the
latter strips off the trailing newline.

With a lone `C-u` as a prefix argument, `y` *replaces* the
region in the other buffer with the yanked text, then quits the
Calculator, leaving you in that buffer. A typical use would be to use
`C-x * g` to read a region of data into the Calculator, operate on the
data to produce a new matrix, then type `C-u y` to replace the
original data with the new data. One might wish to alter the matrix
display style (see [Vector and Matrix Display Formats](#Vector-and-Matrix-Formats)) or change the current
display language (see [Language Modes](#Language-Modes)) before doing this. Also, note
that this command replaces a linear region of text (as grabbed by
`C-x * g`), not a rectangle (as grabbed by `C-x * r`).

If the editing buffer is in overwrite (as opposed to insert) mode,
and the `C-u` prefix was not used, then the yanked number will
overwrite the characters following point rather than being inserted
before those characters. The usual conventions of overwrite mode
are observed; for example, characters will be inserted at the end of
a line rather than overflowing onto the next line. Yanking a multi-line
object such as a matrix in overwrite mode overwrites the next n
lines in the buffer, lengthening or shortening each line as necessary.
Finally, if the thing being yanked is a simple integer or floating-point
number (like ‘`-1.2345e-3`’) and the characters following point also
make up such a number, then Calc will replace that number with the new
number, lengthening or shortening as necessary. The concept of
“overwrite mode” has thus been generalized from overwriting characters
to overwriting one complete number with another.

The `C-x * y` key sequence is equivalent to `y` except that
it can be typed anywhere, not just in Calc. This provides an easy
way to guarantee that Calc knows which editing buffer you want to use!

---

Previous: [Yanking into Other Buffers](#Yanking-Into-Buffers), Up: [Kill and Yank Functions](#Kill-and-Yank)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 14.7 X Cut and Paste [¶](#X-Cut-and-Paste-1)

If you are using Emacs with the X window system, there is an easier
way to move small amounts of data into and out of the calculator:
Use the mouse-oriented cut and paste facilities of X.

The default bindings for a three-button mouse cause the left button
to move the Emacs cursor to the given place, the right button to
select the text between the cursor and the clicked location, and
the middle button to yank the selection into the buffer at the
clicked location. So, if you have a Calc window and an editing
window on your Emacs screen, you can use left-click/right-click
to select a number, vector, or formula from one window, then
middle-click to paste that value into the other window. When you
paste text into the Calc window, Calc interprets it as an algebraic
entry. It doesn’t matter where you click in the Calc window; the
new value is always pushed onto the top of the stack.

The `xterm` program that is typically used for general-purpose
shell windows in X interprets the mouse buttons in the same way.
So you can use the mouse to move data between Calc and any other
Unix program. One nice feature of `xterm` is that a double
left-click selects one word, and a triple left-click selects a
whole line. So you can usually transfer a single number into Calc
just by double-clicking on it in the shell, then middle-clicking
in the Calc window.

---

Next: [Embedded Mode](#Embedded-Mode), Previous: [Kill and Yank Functions](#Kill-and-Yank), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 15 Keypad Mode [¶](#Keypad-Mode-1)

The `C-x * k` (`calc-keypad`) command starts the Calculator
and displays a picture of a calculator-style keypad. If you are using
the X window system, you can click on any of the “keys” in the
keypad using the left mouse button to operate the calculator.
The original window remains the selected window; in Keypad mode
you can type in your file while simultaneously performing
calculations with the mouse.

If you have used `C-x * b` first, `C-x * k` instead invokes
the `full-calc-keypad` command, which takes over the whole
Emacs screen and displays the keypad, the Calc stack, and the Calc
trail all at once. This mode would normally be used when running
Calc standalone (see [Standalone Operation](#Standalone-Operation)).

If you aren’t using the X window system, you must switch into
the `*Calc Keypad*` window, place the cursor on the desired
“key,” and type `SPC` or `RET`. If you think this
is easier than using Calc normally, go right ahead.

Calc commands are more or less the same in Keypad mode. Certain
keypad keys differ slightly from the corresponding normal Calc
keystrokes; all such deviations are described below.

Keypad mode includes many more commands than will fit on the keypad
at once. Click the right mouse button [`calc-keypad-menu`]
to switch to the next menu. The bottom five rows of the keypad
stay the same; the top three rows change to a new set of commands.
To return to earlier menus, click the middle mouse button
[`calc-keypad-menu-back`] or simply advance through the menus
until you wrap around. Typing `TAB` inside the keypad window
is equivalent to clicking the right mouse button there.

You can always click the `EXEC` button and type any normal
Calc key sequence. This is equivalent to switching into the
Calc buffer, typing the keys, then switching back to your
original buffer.

* [Main Menu](#Keypad-Main-Menu)
* [Functions Menu](#Keypad-Functions-Menu)
* [Binary Menu](#Keypad-Binary-Menu)
* [Vectors Menu](#Keypad-Vectors-Menu)
* [Modes Menu](#Keypad-Modes-Menu)

---

Next: [Functions Menu](#Keypad-Functions-Menu), Up: [Keypad Mode](#Keypad-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 15.1 Main Menu [¶](#Main-Menu)

```
|----+----+--Calc---+----+----1
|FLR |CEIL|RND |TRNC|CLN2|FLT |
|----+----+----+----+----+----|
| LN |EXP |    |ABS |IDIV|MOD |
|----+----+----+----+----+----|
|SIN |COS |TAN |SQRT|y^x |1/x |
|----+----+----+----+----+----|
|  ENTER  |+/- |EEX |UNDO| <- |
|-----+---+-+--+--+-+---++----|
| INV |  7  |  8  |  9  |  /  |
|-----+-----+-----+-----+-----|
| HYP |  4  |  5  |  6  |  *  |
|-----+-----+-----+-----+-----|
|EXEC |  1  |  2  |  3  |  -  |
|-----+-----+-----+-----+-----|
| OFF |  0  |  .  | PI  |  +  |
|-----+-----+-----+-----+-----+
```

This is the menu that appears the first time you start Keypad mode.
It will show up in a vertical window on the right side of your screen.
Above this menu is the traditional Calc stack display. On a 24-line
screen you will be able to see the top three stack entries.

The ten digit keys, decimal point, and `EEX` key are used for
entering numbers in the obvious way. `EEX` begins entry of an
exponent in scientific notation. Just as with regular Calc, the
number is pushed onto the stack as soon as you press `ENTER`
or any other function key.

The `+/-` key corresponds to normal Calc’s `n` key. During
numeric entry it changes the sign of the number or of the exponent.
At other times it changes the sign of the number on the top of the
stack.

The `INV` and `HYP` keys modify other keys. As well as
having the effects described elsewhere in this manual, Keypad mode
defines several other “inverse” operations. These are described
below and in the following sections.

The `ENTER` key finishes the current numeric entry, or otherwise
duplicates the top entry on the stack.

The `UNDO` key undoes the most recent Calc operation.
`INV UNDO` is the “redo” command, and `HYP UNDO` is
“last arguments” (`M-RET`).

The `<-` key acts as a “backspace” during numeric entry.
At other times it removes the top stack entry. `INV <-`
clears the entire stack. `HYP <-` takes an integer from
the stack, then removes that many additional stack elements.

The `EXEC` key prompts you to enter any keystroke sequence
that would normally work in Calc mode. This can include a
numeric prefix if you wish. It is also possible simply to
switch into the Calc window and type commands in it; there is
nothing “magic” about this window when Keypad mode is active.

The other keys in this display perform their obvious calculator
functions. `CLN2` rounds the top-of-stack by temporarily
reducing the precision by 2 digits. `FLT` converts an
integer or fraction on the top of the stack to floating-point.

The `INV` and `HYP` keys combined with several of these keys
give you access to some common functions even if the appropriate menu
is not displayed. Obviously you don’t need to learn these keys
unless you find yourself wasting time switching among the menus.

`INV +/-`
:   is the same as `1/x`.

`INV +`
:   is the same as `SQRT`.

`INV -`
:   is the same as `CONJ`.

`INV *`
:   is the same as `y^x`.

`INV /`
:   is the same as `INV y^x` (the ‘`x`’th root of ‘`y`’).

`HYP/INV 1`
:   are the same as `SIN` / `INV SIN`.

`HYP/INV 2`
:   are the same as `COS` / `INV COS`.

`HYP/INV 3`
:   are the same as `TAN` / `INV TAN`.

`INV/HYP 4`
:   are the same as `LN` / `HYP LN`.

`INV/HYP 5`
:   are the same as `EXP` / `HYP EXP`.

`INV 6`
:   is the same as `ABS`.

`INV 7`
:   is the same as `RND` (`calc-round`).

`INV 8`
:   is the same as `CLN2`.

`INV 9`
:   is the same as `FLT` (`calc-float`).

`INV 0`
:   is the same as `IMAG`.

`INV .`
:   is the same as `PREC`.

`INV ENTER`
:   is the same as `SWAP`.

`HYP ENTER`
:   is the same as `RLL3`.

`INV HYP ENTER`
:   is the same as `OVER`.

`HYP +/-`
:   packs the top two stack entries as an error form.

`HYP EEX`
:   packs the top two stack entries as a modulo form.

`INV EEX`
:   creates an interval form; this removes an integer which is one
    of 0 ‘`[]`’, 1 ‘`[)`’, 2 ‘`(]`’ or 3 ‘`()`’, followed
    by the two limits of the interval.

The `OFF` key turns Calc off; typing `C-x * k` or `C-x * *`
again has the same effect. This is analogous to typing `q` or
hitting `C-x * c` again in the normal calculator. If Calc is
running standalone (the `full-calc-keypad` command appeared in the
command line that started Emacs), then `OFF` is replaced with
`EXIT`; clicking on this actually exits Emacs itself.

---

Next: [Binary Menu](#Keypad-Binary-Menu), Previous: [Main Menu](#Keypad-Main-Menu), Up: [Keypad Mode](#Keypad-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 15.2 Functions Menu [¶](#Functions-Menu)

```
|----+----+----+----+----+----2
|IGAM|BETA|IBET|ERF |BESJ|BESY|
|----+----+----+----+----+----|
|IMAG|CONJ| RE |ATN2|RAND|RAGN|
|----+----+----+----+----+----|
|GCD |FACT|DFCT|BNOM|PERM|NXTP|
|----+----+----+----+----+----|
```

This menu provides various operations from the `f` and `k`
prefix keys.

`IMAG` multiplies the number on the stack by the imaginary
number ‘`i = (0, 1)`’.

`RE` extracts the real part a complex number. `INV RE`
extracts the imaginary part.

`RAND` takes a number from the top of the stack and computes
a random number greater than or equal to zero but less than that
number. (See [Random Numbers](#Random-Numbers).) `RAGN` is the “random
again” command; it computes another random number using the
same limit as last time.

`INV GCD` computes the LCM (least common multiple) function.

`INV FACT` is the gamma function.
‘`gamma(x) = (x-1)!`’.

`PERM` is the number-of-permutations function, which is on the
`H k c` key in normal Calc.

`NXTP` finds the next prime after a number. `INV NXTP`
finds the previous prime.

---

Next: [Vectors Menu](#Keypad-Vectors-Menu), Previous: [Functions Menu](#Keypad-Functions-Menu), Up: [Keypad Mode](#Keypad-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 15.3 Binary Menu [¶](#Binary-Menu)

```
|----+----+----+----+----+----3
|AND | OR |XOR |NOT |LSH |RSH |
|----+----+----+----+----+----|
|DEC |HEX |OCT |BIN |WSIZ|ARSH|
|----+----+----+----+----+----|
| A  | B  | C  | D  | E  | F  |
|----+----+----+----+----+----|
```

The keys in this menu perform operations on binary integers.
Note that both logical and arithmetic right-shifts are provided.
`INV LSH` rotates one bit to the left.

The “difference” function (normally on `b d`) is on `INV AND`.
The “clip” function (normally on `b c`) is on `INV NOT`.

The `DEC`, `HEX`, `OCT`, and `BIN` keys select the
current radix for display and entry of numbers: Decimal, hexadecimal,
octal, or binary. The six letter keys `A` through `F` are used
for entering hexadecimal numbers.

The `WSIZ` key displays the current word size for binary operations
and allows you to enter a new word size. You can respond to the prompt
using either the keyboard or the digits and `ENTER` from the keypad.
The initial word size is 32 bits.

---

Next: [Modes Menu](#Keypad-Modes-Menu), Previous: [Binary Menu](#Keypad-Binary-Menu), Up: [Keypad Mode](#Keypad-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 15.4 Vectors Menu [¶](#Vectors-Menu)

```
|----+----+----+----+----+----4
|SUM |PROD|MAX |MAP*|MAP^|MAP$|
|----+----+----+----+----+----|
|MINV|MDET|MTRN|IDNT|CRSS|"x" |
|----+----+----+----+----+----|
|PACK|UNPK|INDX|BLD |LEN |... |
|----+----+----+----+----+----|
```

The keys in this menu operate on vectors and matrices.

`PACK` removes an integer n from the top of the stack;
the next n stack elements are removed and packed into a vector,
which is replaced onto the stack. Thus the sequence
`1 ENTER 3 ENTER 5 ENTER 3 PACK` enters the vector
‘`[1, 3, 5]`’ onto the stack. To enter a matrix, build each row
on the stack as a vector, then use a final `PACK` to collect the
rows into a matrix.

`UNPK` unpacks the vector on the stack, pushing each of its
components separately.

`INDX` removes an integer n, then builds a vector of
integers from 1 to n. `INV INDX` takes three numbers
from the stack: The vector size n, the starting number,
and the increment. `BLD` takes an integer n and any
value x and builds a vector of n copies of x.

`IDNT` removes an integer n, then builds an n-by-n
identity matrix.

`LEN` replaces a vector by its length, an integer.

`...` turns on or off “abbreviated” display mode for large vectors.

`MINV`, `MDET`, `MTRN`, and `CROSS` are the matrix
inverse, determinant, and transpose, and vector cross product.

`SUM` replaces a vector by the sum of its elements. It is
equivalent to `u +` in normal Calc (see [Statistical Operations on Vectors](#Statistical-Operations)).
`PROD` computes the product of the elements of a vector, and
`MAX` computes the maximum of all the elements of a vector.

`INV SUM` computes the alternating sum of the first element
minus the second, plus the third, minus the fourth, and so on.
`INV MAX` computes the minimum of the vector elements.

`HYP SUM` computes the mean of the vector elements.
`HYP PROD` computes the sample standard deviation.
`HYP MAX` computes the median.

`MAP*` multiplies two vectors elementwise. It is equivalent
to the `V M *` command. `MAP^` computes powers elementwise.
The arguments must be vectors of equal length, or one must be a vector
and the other must be a plain number. For example, `2 MAP^` squares
all the elements of a vector.

`MAP$` maps the formula on the top of the stack across the
vector in the second-to-top position. If the formula contains
several variables, Calc takes that many vectors starting at the
second-to-top position and matches them to the variables in
alphabetical order. The result is a vector of the same size as
the input vectors, whose elements are the formula evaluated with
the variables set to the various sets of numbers in those vectors.
For example, you could simulate `MAP^` using `MAP$` with
the formula ‘`x^y`’.

The `"x"` key pushes the variable name ‘`x`’ onto the
stack. To build the formula ‘`x^2 + 6`’, you would use the
key sequence `"x" 2 y^x 6 +`. This formula would then be
suitable for use with the `MAP$` key described above.
With `INV`, `HYP`, or `INV` and `HYP`, the
`"x"` key pushes the variable names ‘`y`’, ‘`z`’, and
‘`t`’, respectively.

---

Previous: [Vectors Menu](#Keypad-Vectors-Menu), Up: [Keypad Mode](#Keypad-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 15.5 Modes Menu [¶](#Modes-Menu)

```
|----+----+----+----+----+----5
|FLT |FIX |SCI |ENG |GRP |    |
|----+----+----+----+----+----|
|RAD |DEG |FRAC|POLR|SYMB|PREC|
|----+----+----+----+----+----|
|SWAP|RLL3|RLL4|OVER|STO |RCL |
|----+----+----+----+----+----|
```

The keys in this menu manipulate modes, variables, and the stack.

The `FLT`, `FIX`, `SCI`, and `ENG` keys select
floating-point, fixed-point, scientific, or engineering notation.
`FIX` displays two digits after the decimal by default; the
others display full precision. With the `INV` prefix, these
keys pop a number-of-digits argument from the stack.

The `GRP` key turns grouping of digits with commas on or off.
`INV GRP` enables grouping to the right of the decimal point as
well as to the left.

The `RAD` and `DEG` keys switch between radians and degrees
for trigonometric functions.

The `FRAC` key turns Fraction mode on or off. This affects
whether commands like `/` with integer arguments produce
fractional or floating-point results.

The `POLR` key turns Polar mode on or off, determining whether
polar or rectangular complex numbers are used by default.

The `SYMB` key turns Symbolic mode on or off, in which
operations that would produce inexact floating-point results
are left unevaluated as algebraic formulas.

The `PREC` key selects the current precision. Answer with
the keyboard or with the keypad digit and `ENTER` keys.

The `SWAP` key exchanges the top two stack elements.
The `RLL3` key rotates the top three stack elements upwards.
The `RLL4` key rotates the top four stack elements upwards.
The `OVER` key duplicates the second-to-top stack element.

The `STO` and `RCL` keys are analogous to `s t` and
`s r` in regular Calc. See [Storing and Recalling](#Store-and-Recall). Click the
`STO` or `RCL` key, then one of the ten digits. (Named
variables are not available in Keypad mode.) You can also use,
for example, `STO + 3` to add to register 3.

---

Next: [Programming](#Programming), Previous: [Keypad Mode](#Keypad-Mode), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 16 Embedded Mode [¶](#Embedded-Mode-1)

Embedded mode in Calc provides an alternative to copying numbers
and formulas back and forth between editing buffers and the Calc
stack. In Embedded mode, your editing buffer becomes temporarily
linked to the stack and this copying is taken care of automatically.

* [Basic Embedded Mode](#Basic-Embedded-Mode)
* [More About Embedded Mode](#More-About-Embedded-Mode)
* [Assignments in Embedded Mode](#Assignments-in-Embedded-Mode)
* [Mode Settings in Embedded Mode](#Mode-Settings-in-Embedded-Mode)
* [Customizing Embedded Mode](#Customizing-Embedded-Mode)

---

Next: [More About Embedded Mode](#More-About-Embedded-Mode), Up: [Embedded Mode](#Embedded-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 16.1 Basic Embedded Mode [¶](#Basic-Embedded-Mode-1)

To enter Embedded mode, position the Emacs point (cursor) on a
formula in any buffer and press `C-x * e` (`calc-embedded`).
Note that `C-x * e` is not to be used in the Calc stack buffer
like most Calc commands, but rather in regular editing buffers that
are visiting your own files.

Calc will try to guess an appropriate language based on the major mode
of the editing buffer. (See [Language Modes](#Language-Modes).) If the current buffer is
in `latex-mode`, for example, Calc will set its language to LaTeX.
Similarly, Calc will use TeX language for `tex-mode`,
`plain-tex-mode` and `context-mode`, C language for
`c-mode` and `c++-mode`, FORTRAN language for
`fortran-mode` and `f90-mode`, Pascal for `pascal-mode`,
and eqn for `nroff-mode` (see [Customizing Calc](#Customizing-Calc)).
These can be overridden with Calc’s mode
changing commands (see [Mode Settings in Embedded Mode](#Mode-Settings-in-Embedded-Mode)). If no
suitable language is available, Calc will continue with its current language.

Calc normally scans backward and forward in the buffer for the
nearest opening and closing *formula delimiters*. The simplest
delimiters are blank lines. Other delimiters that Embedded mode
understands are:

1. The TeX and LaTeX math delimiters ‘`$ $`’, ‘`$$ $$`’,
   ‘`\[ \]`’, and ‘`\( \)`’;
2. Lines beginning with ‘`\begin`’ and ‘`\end`’ (except matrix delimiters);
3. Lines beginning with ‘`@`’ (Texinfo delimiters).
4. Lines beginning with ‘`.EQ`’ and ‘`.EN`’ (*eqn* delimiters);
5. Lines containing a single ‘`%`’ or ‘`.\"`’ symbol and nothing else.

See [Customizing Embedded Mode](#Customizing-Embedded-Mode), to see how to make Calc recognize
your own favorite delimiters. Delimiters like ‘`$ $`’ can appear
on their own separate lines or in-line with the formula.

If you give a positive or negative numeric prefix argument, Calc
instead uses the current point as one end of the formula, and includes
that many lines forward or backward (respectively, including the current
line). Explicit delimiters are not necessary in this case.

With a prefix argument of zero, Calc uses the current region (delimited
by point and mark) instead of formula delimiters. With a prefix
argument of `C-u` only, Calc uses the current line as the formula.

The `C-x * w` (`calc-embedded-word`) command will start Embedded
mode on the current “word”; in this case Calc will scan for the first
non-numeric character (i.e., the first character that is not a digit,
sign, decimal point, or upper- or lower-case ‘`e`’) forward and
backward to delimit the formula.

When you enable Embedded mode for a formula, Calc reads the text
between the delimiters and tries to interpret it as a Calc formula.
Calc can generally identify TeX formulas and
Big-style formulas even if the language mode is wrong. If Calc
can’t make sense of the formula, it beeps and refuses to enter
Embedded mode. But if the current language is wrong, Calc can
sometimes parse the formula successfully (but incorrectly);
for example, the C expression ‘`atan(a[1])`’ can be parsed
in Normal language mode, but the `atan` won’t correspond to
the built-in `arctan` function, and the ‘`a[1]`’ will be
interpreted as ‘`a`’ times the vector ‘`[1]`’!

If you press `C-x * e` or `C-x * w` to activate an embedded
formula which is blank, say with the cursor on the space between
the two delimiters ‘`$ $`’, Calc will immediately prompt for
an algebraic entry.

Only one formula in one buffer can be enabled at a time. If you
move to another area of the current buffer and give Calc commands,
Calc turns Embedded mode off for the old formula and then tries
to restart Embedded mode at the new position. Other buffers are
not affected by Embedded mode.

When Embedded mode begins, Calc pushes the current formula onto
the stack. No Calc stack window is created; however, Calc copies
the top-of-stack position into the original buffer at all times.
You can create a Calc window by hand with `C-x * o` if you
find you need to see the entire stack.

For example, typing `C-x * e` while somewhere in the formula
‘`n>2`’ in the following line enables Embedded mode on that
inequality:

```
We define $F_n = F_(n-1)+F_(n-2)$ for all $n>2$.
```

The formula ‘`n>2`’ will be pushed onto the Calc stack, and
the top of stack will be copied back into the editing buffer.
This means that spaces will appear around the ‘`>`’ symbol
to match Calc’s usual display style:

```
We define $F_n = F_(n-1)+F_(n-2)$ for all $n > 2$.
```

No spaces have appeared around the ‘`+`’ sign because it’s
in a different formula, one which we have not yet touched with
Embedded mode.

Now that Embedded mode is enabled, keys you type in this buffer
are interpreted as Calc commands. At this point we might use
the “commute” command `j C` to reverse the inequality.
This is a selection-based command for which we first need to
move the cursor onto the operator (‘`>`’ in this case) that
needs to be commuted.

```
We define $F_n = F_(n-1)+F_(n-2)$ for all $2 < n$.
```

The `C-x * o` command is a useful way to open a Calc window
without actually selecting that window. Giving this command
verifies that ‘`2 < n`’ is also on the Calc stack. Typing
`17 RET` would produce:

```
We define $F_n = F_(n-1)+F_(n-2)$ for all $17$.
```

with ‘`2 < n`’ and ‘`17`’ on the stack; typing `TAB`
at this point will exchange the two stack values and restore
‘`2 < n`’ to the embedded formula. Even though you can’t
normally see the stack in Embedded mode, it is still there and
it still operates in the same way. But, as with old-fashioned
RPN calculators, you can only see the value at the top of the
stack at any given time (unless you use `C-x * o`).

Typing `C-x * e` again turns Embedded mode off. The Calc
window reveals that the formula ‘`2 < n`’ is automatically
removed from the stack, but the ‘`17`’ is not. Entering
Embedded mode always pushes one thing onto the stack, and
leaving Embedded mode always removes one thing. Anything else
that happens on the stack is entirely your business as far as
Embedded mode is concerned.

If you press `C-x * e` in the wrong place by accident, it is
possible that Calc will be able to parse the nearby text as a
formula and will mangle that text in an attempt to redisplay it
“properly” in the current language mode. If this happens,
press `C-x * e` again to exit Embedded mode, then give the
regular Emacs “undo” command (`C-_` or `C-x u`) to put
the text back the way it was before Calc edited it. Note that Calc’s
own Undo command (typed before you turn Embedded mode back off)
will not do you any good, because as far as Calc is concerned
you haven’t done anything with this formula yet.

---

Next: [Assignments in Embedded Mode](#Assignments-in-Embedded-Mode), Previous: [Basic Embedded Mode](#Basic-Embedded-Mode), Up: [Embedded Mode](#Embedded-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 16.2 More About Embedded Mode [¶](#More-About-Embedded-Mode-1)

When Embedded mode “activates” a formula, i.e., when it examines
the formula for the first time since the buffer was created or
loaded, Calc tries to sense the language in which the formula was
written. If the formula contains any LaTeX-like ‘`\`’ sequences,
it is parsed (i.e., read) in LaTeX mode. If the formula appears to
be written in multi-line Big mode, it is parsed in Big mode. Otherwise,
it is parsed according to the current language mode.

Note that Calc does not change the current language mode according
the formula it reads in. Even though it can read a LaTeX formula when
not in LaTeX mode, it will immediately rewrite this formula using
whatever language mode is in effect.

Calc’s parser is unable to read certain kinds of formulas. For
example, with `v ]` (`calc-matrix-brackets`) you can
specify matrix display styles which the parser is unable to
recognize as matrices. The `d p` (`calc-show-plain`)
command turns on a mode in which a “plain” version of a
formula is placed in front of the fully-formatted version.
When Calc reads a formula that has such a plain version in
front, it reads the plain version and ignores the formatted
version.

Plain formulas are preceded and followed by ‘`%%%`’ signs
by default. This notation has the advantage that the ‘`%`’
character begins a comment in TeX and LaTeX, so if your formula is
embedded in a TeX or LaTeX document its plain version will be
invisible in the final printed copy. Certain major modes have different
delimiters to ensure that the “plain” version will be
in a comment for those modes, also.
See [Customizing Embedded Mode](#Customizing-Embedded-Mode), to see how to change the “plain”
formula delimiters.

There are several notations which Calc’s parser for “big”
formatted formulas can’t yet recognize. In particular, it can’t
read the large symbols for `sum`, `prod`, and `integ`,
and it can’t handle ‘`=>`’ with the righthand argument omitted.
Also, Calc won’t recognize special formats you have defined with
the `Z C` command (see [User-Defined Compositions](#User_002dDefined-Compositions)). In
these cases it is important to use “plain” mode to make sure
Calc will be able to read your formula later.

Another example where “plain” mode is important is if you have
specified a float mode with few digits of precision. Normally
any digits that are computed but not displayed will simply be
lost when you save and re-load your embedded buffer, but “plain”
mode allows you to make sure that the complete number is present
in the file as well as the rounded-down number.

Embedded buffers remember active formulas for as long as they
exist in Emacs memory. Suppose you have an embedded formula
which is ‘`pi`’ to the normal 12 decimal places, and then
type `C-u 5 d n` to display only five decimal places.
If you then type `d n`, all 12 places reappear because the
full number is still there on the Calc stack. More surprisingly,
even if you exit Embedded mode and later re-enter it for that
formula, typing `d n` will restore all 12 places because
each buffer remembers all its active formulas. However, if you
save the buffer in a file and reload it in a new Emacs session,
all non-displayed digits will have been lost unless you used
“plain” mode.

In some applications of Embedded mode, you will want to have a
sequence of copies of a formula that show its evolution as you
work on it. For example, you might want to have a sequence
like this in your file (elaborating here on the example from
the “Getting Started” chapter):

```
The derivative of

                              ln(ln(x))

is

                  (the derivative of ln(ln(x)))

whose value at x = 2 is

                            (the value)

and at x = 3 is

                            (the value)
```

The `C-x * d` (`calc-embedded-duplicate`) command is a
handy way to make sequences like this. If you type `C-x * d`,
the formula under the cursor (which may or may not have Embedded
mode enabled for it at the time) is copied immediately below and
Embedded mode is then enabled for that copy.

For this example, you would start with just

```
The derivative of

                              ln(ln(x))
```

and press `C-x * d` with the cursor on this formula. The result
is

```
The derivative of

                              ln(ln(x))

                              ln(ln(x))
```

with the second copy of the formula enabled in Embedded mode.
You can now press `a d x RET` to take the derivative, and
`C-x * d C-x * d` to make two more copies of the derivative.
To complete the computations, type `3 s l x RET` to evaluate
the last formula, then move up to the second-to-last formula
and type `2 s l x RET`.

Finally, you would want to press `C-x * e` to exit Embedded
mode, then go up and insert the necessary text in between the
various formulas and numbers.

The `C-x * f` (`calc-embedded-new-formula`) command
creates a new embedded formula at the current point. It inserts
some default delimiters, which are usually just blank lines,
and then does an algebraic entry to get the formula (which is
then enabled for Embedded mode). This is just shorthand for
typing the delimiters yourself, positioning the cursor between
the new delimiters, and pressing `C-x * e`. The key sequence
`C-x * '` is equivalent to `C-x * f`.

The `C-x * n` (`calc-embedded-next`) and `C-x * p`
(`calc-embedded-previous`) commands move the cursor to the
next or previous active embedded formula in the buffer. They
can take positive or negative prefix arguments to move by several
formulas. Note that these commands do not actually examine the
text of the buffer looking for formulas; they only see formulas
which have previously been activated in Embedded mode. In fact,
`C-x * n` and `C-x * p` are a useful way to tell which
embedded formulas are currently active. Also, note that these
commands do not enable Embedded mode on the next or previous
formula, they just move the cursor.

The `` C-x * ` `` (`calc-embedded-edit`) command edits the
embedded formula at the current point as if by `` ` `` (`calc-edit`).
Embedded mode does not have to be enabled for this to work. Press
`C-c C-c` to finish the edit, or `C-x k` to cancel.

---

Next: [Mode Settings in Embedded Mode](#Mode-Settings-in-Embedded-Mode), Previous: [More About Embedded Mode](#More-About-Embedded-Mode), Up: [Embedded Mode](#Embedded-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 16.3 Assignments in Embedded Mode [¶](#Assignments-in-Embedded-Mode-1)

The ‘`:=`’ (assignment) and ‘`=>`’ (“evaluates-to”) operators
are especially useful in Embedded mode. They allow you to make
a definition in one formula, then refer to that definition in
other formulas embedded in the same buffer.

An embedded formula which is an assignment to a variable, as in

```
foo := 5
```

records ‘`5`’ as the stored value of `foo` for the
purposes of Embedded mode operations in the current buffer. It
does *not* actually store ‘`5`’ as the “global” value
of `foo`, however. Regular Calc operations, and Embedded
formulas in other buffers, will not see this assignment.

One way to use this assigned value is simply to create an
Embedded formula elsewhere that refers to `foo`, and to press
`=` in that formula. However, this permanently replaces the
`foo` in the formula with its current value. More interesting
is to use ‘`=>`’ elsewhere:

```
foo + 7 => 12
```

See [The Evaluates-To Operator](#Evaluates_002dTo-Operator), for a general discussion of ‘`=>`’.

If you move back and change the assignment to `foo`, any
‘`=>`’ formulas which refer to it are automatically updated.

```
foo := 17

foo + 7 => 24
```

The obvious question then is, *how* can one easily change the
assignment to `foo`? If you simply select the formula in
Embedded mode and type 17, the assignment itself will be replaced
by the 17. The effect on the other formula will be that the
variable `foo` becomes unassigned:

```
17

foo + 7 => foo + 7
```

The right thing to do is first to use a selection command (`j 2`
will do the trick) to select the righthand side of the assignment.
Then, `17 TAB DEL` will swap the 17 into place (see [Selecting Sub-Formulas](#Selecting-Subformulas), to see how this works).

The `C-x * j` (`calc-embedded-select`) command provides an
easy way to operate on assignments. It is just like `C-x * e`,
except that if the enabled formula is an assignment, it uses
`j 2` to select the righthand side. If the enabled formula
is an evaluates-to, it uses `j 1` to select the lefthand side.
A formula can also be a combination of both:

```
bar := foo + 3 => 20
```

in which case `C-x * j` will select the middle part (‘`foo + 3`’).

The formula is automatically deselected when you leave Embedded
mode.

Another way to change the assignment to `foo` would simply be
to edit the number using regular Emacs editing rather than Embedded
mode. Then, we have to find a way to get Embedded mode to notice
the change. The `C-x * u` (`calc-embedded-update-formula`)
command is a convenient way to do this.

```
foo := 6

foo + 7 => 13
```

Pressing `C-x * u` is much like pressing `C-x * e = C-x * e`, that
is, temporarily enabling Embedded mode for the formula under the
cursor and then evaluating it with `=`. But `C-x * u` does
not actually use `C-x * e`, and in fact another formula somewhere
else can be enabled in Embedded mode while you use `C-x * u` and
that formula will not be disturbed.

With a numeric prefix argument, `C-x * u` updates all active
‘`=>`’ formulas in the buffer. Formulas which have not yet
been activated in Embedded mode, and formulas which do not have
‘`=>`’ as their top-level operator, are not affected by this.
(This is useful only if you have used `m C`; see below.)

With a plain `C-u` prefix, `C-u C-x * u` updates only in the
region between mark and point rather than in the whole buffer.

`C-x * u` is also a handy way to activate a formula, such as an
‘`=>`’ formula that has freshly been typed in or loaded from a
file.

The `C-x * a` (`calc-embedded-activate`) command scans
through the current buffer and activates all embedded formulas
that contain ‘`:=`’ or ‘`=>`’ symbols. This does not mean
that Embedded mode is actually turned on, but only that the
formulas’ positions are registered with Embedded mode so that
the ‘`=>`’ values can be properly updated as assignments are
changed.

It is a good idea to type `C-x * a` right after loading a file
that uses embedded ‘`=>`’ operators. Emacs includes a nifty
“buffer-local variables” feature that you can use to do this
automatically. The idea is to place near the end of your file
a few lines that look like this:

```
--- Local Variables: ---
--- eval:(calc-embedded-activate) ---
--- End: ---
```

where the leading and trailing ‘`---`’ can be replaced by
any suitable strings (which must be the same on all three lines)
or omitted altogether; in a TeX or LaTeX file, ‘`%`’ would be a good
leading string and no trailing string would be necessary. In a
C program, ‘`/*`’ and ‘`*/`’ would be good leading and
trailing strings.

When Emacs loads a file into memory, it checks for a Local Variables
section like this one at the end of the file. If it finds this
section, it does the specified things (in this case, running
`C-x * a` automatically) before editing of the file begins.
The Local Variables section must be within 3000 characters of the
end of the file for Emacs to find it, and it must be in the last
page of the file if the file has any page separators.
See [Local Variables in Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#File-Variables) in the
Emacs manual.

Note that `C-x * a` does not update the formulas it finds.
To do this, type, say, `M-1 C-x * u` after `C-x * a`.
Generally this should not be a problem, though, because the
formulas will have been up-to-date already when the file was
saved.

Normally, `C-x * a` activates all the formulas it finds, but
any previous active formulas remain active as well. With a
positive numeric prefix argument, `C-x * a` first deactivates
all current active formulas, then actives the ones it finds in
its scan of the buffer. With a negative prefix argument,
`C-x * a` simply deactivates all formulas.

Embedded mode has two symbols, ‘`Active`’ and ‘`~Active`’,
which it puts next to the major mode name in a buffer’s mode line.
It puts ‘`Active`’ if it has reason to believe that all
formulas in the buffer are active, because you have typed `C-x * a`
and Calc has not since had to deactivate any formulas (which can
happen if Calc goes to update an ‘`=>`’ formula somewhere because
a variable changed, and finds that the formula is no longer there
due to some kind of editing outside of Embedded mode). Calc puts
‘`~Active`’ in the mode line if some, but probably not all,
formulas in the buffer are active. This happens if you activate
a few formulas one at a time but never use `C-x * a`, or if you
used `C-x * a` but then Calc had to deactivate a formula
because it lost track of it. If neither of these symbols appears
in the mode line, no embedded formulas are active in the buffer
(e.g., before Embedded mode has been used, or after a `M-- C-x * a`).

Embedded formulas can refer to assignments both before and after them
in the buffer. If there are several assignments to a variable, the
nearest preceding assignment is used if there is one, otherwise the
following assignment is used.

```
x => 1

x := 1

x => 1

x := 2

x => 2
```

As well as simple variables, you can also assign to subscript
expressions of the form ‘`var_number`’ (as in
`x_0`), or ‘`var_var`’ (as in `x_max`).
Assignments to other kinds of objects can be represented by Calc,
but the automatic linkage between assignments and references works
only for plain variables and these two kinds of subscript expressions.

If there are no assignments to a given variable, the global
stored value for the variable is used (see [Storing Variables](#Storing-Variables)),
or, if no value is stored, the variable is left in symbolic form.
Note that global stored values will be lost when the file is saved
and loaded in a later Emacs session, unless you have used the
`s p` (`calc-permanent-variable`) command to save them;
see [Other Operations on Variables](#Operations-on-Variables).

The `m C` (`calc-auto-recompute`) command turns automatic
recomputation of ‘`=>`’ forms on and off. If you turn automatic
recomputation off, you will have to use `C-x * u` to update these
formulas manually after an assignment has been changed. If you
plan to change several assignments at once, it may be more efficient
to type `m C`, change all the assignments, then use `M-1 C-x * u`
to update the entire buffer afterwards. The `m C` command also
controls ‘`=>`’ formulas on the stack; see [The Evaluates-To Operator](#Evaluates_002dTo-Operator). When you turn automatic recomputation back on, the
stack will be updated but the Embedded buffer will not; you must
use `C-x * u` to update the buffer by hand.

---

Next: [Customizing Embedded Mode](#Customizing-Embedded-Mode), Previous: [Assignments in Embedded Mode](#Assignments-in-Embedded-Mode), Up: [Embedded Mode](#Embedded-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 16.4 Mode Settings in Embedded Mode [¶](#Mode-Settings-in-Embedded-Mode-1)

The mode settings can be changed while Calc is in embedded mode, but
by default they will revert to their original values when embedded mode
is ended. However, the modes saved when the mode-recording mode is
`Save` (see below) and the modes in effect when the `m e`
(`calc-embedded-preserve-modes`) command is given
will be preserved when embedded mode is ended.

Embedded mode has a rather complicated mechanism for handling mode
settings in Embedded formulas. It is possible to put annotations
in the file that specify mode settings either global to the entire
file or local to a particular formula or formulas. In the latter
case, different modes can be specified for use when a formula
is the enabled Embedded mode formula.

When you give any mode-setting command, like `m f` (for Fraction
mode) or `d s` (for scientific notation), Embedded mode adds
a line like the following one to the file just before the opening
delimiter of the formula.

```
% [calc-mode: fractions: t]
% [calc-mode: float-format: (sci 0)]
```

When Calc interprets an embedded formula, it scans the text before
the formula for mode-setting annotations like these and sets the
Calc buffer to match these modes. Modes not explicitly described
in the file are not changed. Calc scans all the way to the top of
the file, or up to a line of the form

```
% [calc-defaults]
```

which you can insert at strategic places in the file if this backward
scan is getting too slow, or just to provide a barrier between one
“zone” of mode settings and another.

If the file contains several annotations for the same mode, the
closest one before the formula is used. Annotations after the
formula are never used (except for global annotations, described
below).

The scan does not look for the leading ‘`%` ’, only for the
square brackets and the text they enclose. In fact, the leading
characters are different for different major modes. You can edit the
mode annotations to a style that works better in context if you wish.
See [Customizing Embedded Mode](#Customizing-Embedded-Mode), to see how to change the style
that Calc uses when it generates the annotations. You can write
mode annotations into the file yourself if you know the syntax;
the easiest way to find the syntax for a given mode is to let
Calc write the annotation for it once and see what it does.

If you give a mode-changing command for a mode that already has
a suitable annotation just above the current formula, Calc will
modify that annotation rather than generating a new, conflicting
one.

Mode annotations have three parts, separated by colons. (Spaces
after the colons are optional.) The first identifies the kind
of mode setting, the second is a name for the mode itself, and
the third is the value in the form of a Lisp symbol, number,
or list. Annotations with unrecognizable text in the first or
second parts are ignored. The third part is not checked to make
sure the value is of a valid type or range; if you write an
annotation by hand, be sure to give a proper value or results
will be unpredictable. Mode-setting annotations are case-sensitive.

While Embedded mode is enabled, the word `Local` appears in
the mode line. This is to show that mode setting commands generate
annotations that are “local” to the current formula or set of
formulas. The `m R` (`calc-mode-record-mode`) command
causes Calc to generate different kinds of annotations. Pressing
`m R` repeatedly cycles through the possible modes.

`LocEdit` and `LocPerm` modes generate annotations
that look like this, respectively:

```
% [calc-edit-mode: float-format: (sci 0)]
% [calc-perm-mode: float-format: (sci 5)]
```

The first kind of annotation will be used only while a formula
is enabled in Embedded mode. The second kind will be used only
when the formula is *not* enabled. (Whether the formula
is “active” or not, i.e., whether Calc has seen this formula
yet, is not relevant here.)

`Global` mode generates an annotation like this at the end
of the file:

```
% [calc-global-mode: fractions t]
```

Global mode annotations affect all formulas throughout the file,
and may appear anywhere in the file. This allows you to tuck your
mode annotations somewhere out of the way, say, on a new page of
the file, as long as those mode settings are suitable for all
formulas in the file.

Enabling a formula with `C-x * e` causes a fresh scan for local
mode annotations; you will have to use this after adding annotations
above a formula by hand to get the formula to notice them. Updating
a formula with `C-x * u` will also re-scan the local modes, but
global modes are only re-scanned by `C-x * a`.

Another way that modes can get out of date is if you add a local
mode annotation to a formula that has another formula after it.
In this example, we have used the `d s` command while the
first of the two embedded formulas is active. But the second
formula has not changed its style to match, even though by the
rules of reading annotations the ‘`(sci 0)`’ applies to it, too.

```
% [calc-mode: float-format: (sci 0)]
1.23e2

456.
```

We would have to go down to the other formula and press `C-x * u`
on it in order to get it to notice the new annotation.

Two more mode-recording modes selectable by `m R` are available
which are also available outside of Embedded mode.
(see [General Mode Commands](#General-Mode-Commands).) They are `Save`, in which mode
settings are recorded permanently in your Calc init file (the file given
by the variable `calc-settings-file`, typically `~/.emacs.d/calc.el`)
rather than by annotating the current document, and no-recording
mode (where there is no symbol like `Save` or `Local` in
the mode line), in which mode-changing commands do not leave any
annotations at all.

When Embedded mode is not enabled, mode-recording modes except
for `Save` have no effect.

---

Previous: [Mode Settings in Embedded Mode](#Mode-Settings-in-Embedded-Mode), Up: [Embedded Mode](#Embedded-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 16.5 Customizing Embedded Mode [¶](#Customizing-Embedded-Mode-1)

You can modify Embedded mode’s behavior by setting various Lisp
variables described here. These variables are customizable
(see [Customizing Calc](#Customizing-Calc)), or you can use `M-x set-variable`
to adjust a variable on the fly.
(Another possibility would be to use a file-local variable annotation at
the end of the file;
see [Local Variables in Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#File-Variables) in the Emacs manual.)
Many of the variables given mentioned here can be set to depend on the
major mode of the editing buffer (see [Customizing Calc](#Customizing-Calc)).

The `calc-embedded-open-formula` variable holds a regular
expression for the opening delimiter of a formula. See [Regular Expression Search](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Regexp-Search) in the Emacs manual, to see
how regular expressions work. Basically, a regular expression is a
pattern that Calc can search for. A regular expression that considers
blank lines, ‘`$`’, and ‘`$$`’ to be opening delimiters is
`` "\\`\\|^\n\\|\\$\\$?" ``. Just in case the meaning of this
regular expression is not completely plain, let’s go through it
in detail.

The surrounding ‘`" "`’ marks quote the text between them as a
Lisp string. If you left them off, `set-variable` (for example)
would try to read the regular expression as a Lisp program.

The most obvious property of this regular expression is that it
contains indecently many backslashes. There are actually two levels
of backslash usage going on here. First, when Lisp reads a quoted
string, all pairs of characters beginning with a backslash are
interpreted as special characters. Here, `\n` changes to a
new-line character, and `\\` changes to a single backslash.
So the actual regular expression seen by Calc is
‘`` \`\|^ (newline) \|\$\$? ``’.

Regular expressions also consider pairs beginning with backslash
to have special meanings. Sometimes the backslash is used to quote
a character that otherwise would have a special meaning in a regular
expression, like ‘`$`’, which normally means “end-of-line,”
or ‘`?`’, which means that the preceding item is optional. So
‘`\$\$?`’ matches either one or two dollar signs.

The other codes in this regular expression are ‘`^`’, which matches
“beginning-of-line,” ‘`\|`’, which means “or,” and ‘`` \` ``’,
which matches “beginning-of-buffer.” So the whole pattern means
that a formula begins at the beginning of the buffer, or on a newline
that occurs at the beginning of a line (i.e., a blank line), or at
one or two dollar signs.

The default value of `calc-embedded-open-formula` looks just
like this example, with several more alternatives added on to
recognize various other common kinds of delimiters.

By the way, the reason to use ‘`^\n`’ rather than ‘`^$`’
or ‘`\n\n`’, which also would appear to match blank lines,
is that the former expression actually “consumes” only one
newline character as *part of* the delimiter, whereas the
latter expressions consume zero or two newlines, respectively.
The former choice gives the most natural behavior when Calc
must operate on a whole formula including its delimiters.

See the Emacs manual for complete details on regular expressions.
But just for your convenience, here is a list of all characters
which must be quoted with backslash (like ‘`\$`’) to avoid
some special interpretation: ‘`. * + ? [ ] ^ $ \`’. (Note
the backslash in this list; for example, to match ‘`\[`’ you
must use `"\\\\\\["`. An exercise for the reader is to
account for each of these six backslashes!)

The `calc-embedded-close-formula` variable holds a regular
expression for the closing delimiter of a formula. A closing
regular expression to match the above example would be
`"\\'\\|\n$\\|\\$\\$?"`. This is almost the same as the
other one, except it now uses ‘`\'`’ (“end-of-buffer”) and
‘`\n$`’ (newline occurring at end of line, yet another way
of describing a blank line that is more appropriate for this
case).

The `calc-embedded-word-regexp` variable holds a regular expression
used to define an expression to look for (a “word”) when you type
`C-x * w` to enable Embedded mode.

The `calc-embedded-open-plain` variable is a string which
begins a “plain” formula written in front of the formatted
formula when `d p` mode is turned on. Note that this is an
actual string, not a regular expression, because Calc must be able
to write this string into a buffer as well as to recognize it.
The default string is `"%%% "` (note the trailing space), but may
be different for certain major modes.

The `calc-embedded-close-plain` variable is a string which
ends a “plain” formula. The default is `" %%%\n"`, but may be
different for different major modes. Without
the trailing newline here, the first line of a Big mode formula
that followed might be shifted over with respect to the other lines.

The `calc-embedded-open-new-formula` variable is a string
which is inserted at the front of a new formula when you type
`C-x * f`. Its default value is `"\n\n"`. If this
string begins with a newline character and the `C-x * f` is
typed at the beginning of a line, `C-x * f` will skip this
first newline to avoid introducing unnecessary blank lines in
the file.

The `calc-embedded-close-new-formula` variable is the corresponding
string which is inserted at the end of a new formula. Its default
value is also `"\n\n"`. The final newline is omitted by
`C-x * f` if typed at the end of a line. (It follows that if
`C-x * f` is typed on a blank line, both a leading opening
newline and a trailing closing newline are omitted.)

The `calc-embedded-announce-formula` variable is a regular
expression which is sure to be followed by an embedded formula.
The `C-x * a` command searches for this pattern as well as for
‘`=>`’ and ‘`:=`’ operators. Note that `C-x * a` will
not activate just anything surrounded by formula delimiters; after
all, blank lines are considered formula delimiters by default!
But if your language includes a delimiter which can only occur
actually in front of a formula, you can take advantage of it here.
The default pattern is `"%Embed\n\\(% .*\n\\)*"`, but may be
different for different major modes.
This pattern will check for ‘`%Embed`’ followed by any number of
lines beginning with ‘`%`’ and a space. This last is important to
make Calc consider mode annotations part of the pattern, so that the
formula’s opening delimiter really is sure to follow the pattern.

The `calc-embedded-open-mode` variable is a string (not a
regular expression) which should precede a mode annotation.
Calc never scans for this string; Calc always looks for the
annotation itself. But this is the string that is inserted before
the opening bracket when Calc adds an annotation on its own.
The default is `"% "`, but may be different for different major
modes.

The `calc-embedded-close-mode` variable is a string which
follows a mode annotation written by Calc. Its default value
is simply a newline, `"\n"`, but may be different for different
major modes. If you change this, it is a good idea still to end with a
newline so that mode annotations will appear on lines by themselves.

---

Next: [GNU GENERAL PUBLIC LICENSE](#Copying), Previous: [Embedded Mode](#Embedded-Mode), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 17 Programming [¶](#Programming-1)

There are several ways to “program” the Emacs Calculator, depending
on the nature of the problem you need to solve.

1. *Keyboard macros* allow you to record a sequence of keystrokes
   and play them back at a later time. This is just the standard Emacs
   keyboard macro mechanism, dressed up with a few more features such
   as loops and conditionals.
2. *Algebraic definitions* allow you to use any formula to define a
   new function. This function can then be used in algebraic formulas or
   as an interactive command.
3. *Rewrite rules* are discussed in the section on algebra commands.
   See [Rewrite Rules](#Rewrite-Rules). If you put your rewrite rules in the variable
   `EvalRules`, they will be applied automatically to all Calc
   results in just the same way as an internal “rule” is applied to
   evaluate ‘`sqrt(9)`’ to 3 and so on. See [Automatic Rewrites](#Automatic-Rewrites).
4. *Lisp* is the programming language that Calc (and most of Emacs)
   is written in. If the above techniques aren’t powerful enough, you
   can write Lisp functions to do anything that built-in Calc commands
   can do. Lisp code is also somewhat faster than keyboard macros or
   rewrite rules.

Programming features are available through the `z` and `Z`
prefix keys. New commands that you define are two-key sequences
beginning with `z`. Commands for managing these definitions
use the shift-`Z` prefix. (The `Z T` (`calc-timing`)
command is described elsewhere; see [Troubleshooting Commands](#Troubleshooting-Commands).
The `Z C` (`calc-user-define-composition`) command is also
described elsewhere; see [User-Defined Compositions](#User_002dDefined-Compositions).)

* [Creating User Keys](#Creating-User-Keys)
* [Programming with Keyboard Macros](#Keyboard-Macros)
* [Invocation Macros](#Invocation-Macros)
* [Programming with Formulas](#Algebraic-Definitions)
* [Programming Calc, with Lisp](#Lisp-Definitions)

---

Next: [Programming with Keyboard Macros](#Keyboard-Macros), Up: [Programming](#Programming)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
