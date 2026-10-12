Here the file name is given as an *argument* to the command `C-x
C-f` (`find-file`). That command uses the *minibuffer* to
read the argument, and you type `RET` to terminate the argument
(see section [E. The Minibuffer](emacs.html#SEC31)).

Emacs obeys the command by *visiting* the file: creating a buffer,
copying the contents of the file into the buffer, and then displaying
the buffer for you to edit. If you alter the text, you can *save*
the new text in the file by typing `C-x C-s` (`save-buffer`).
This makes the changes permanent by copying the altered buffer contents
back into the file `/usr/rms/foo.c'. Until you save, the changes
exist only inside Emacs, and the file `foo.c' is unaltered.

To create a file, just visit the file with `C-x C-f` as if it
already existed. This creates an empty buffer in which you can insert
the text you want to put in the file. The file is actually created when
you save this buffer with `C-x C-s`.

Of course, there is a lot more to learn about using files. See section [M. File Handling](emacs.html#SEC110).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC24) ] | [ [>](emacs.html#SEC26) ] | [ [<<](emacs.html#SEC26) ] | [ [Up](emacs.html#SEC19) ] | [ [>>](emacs.html#SEC31) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## D.6 Help

If you forget what a key does, you can find out with the Help
character, which is `C-h` (or `F1`, which is an alias for
`C-h`). Type `C-h k` followed by the key you want to know
about; for example, `C-h k C-n` tells you all about what `C-n`
does. `C-h` is a prefix key; `C-h k` is just one of its
subcommands (the command `describe-key`). The other subcommands of
`C-h` provide different kinds of help. Type `C-h` twice to get
a description of all the help facilities. See section [G. Help](emacs.html#SEC42).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC25) ] | [ [>](emacs.html#SEC27) ] | [ [<<](emacs.html#SEC27) ] | [ [Up](emacs.html#SEC19) ] | [ [>>](emacs.html#SEC31) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## D.7 Blank Lines

Here are special commands and techniques for putting in and taking out
blank lines.

`C-o`: Insert one or more blank lines after the cursor (`open-line`). `C-x C-o`: Delete all but one of many consecutive blank lines (`delete-blank-lines`).

When you want to insert a new line of text before an existing line, you
can do it by typing the new line of text, followed by `RET`.
However, it may be easier to see what you are doing if you first make a
blank line and then insert the desired text into it. This is easy to do
using the key `C-o` (`open-line`), which inserts a newline
after point but leaves point in front of the newline. After `C-o`,
type the text for the new line. `C-o F O O` has the same effect as
`F O O RET`, except for the final location of point.

You can make several blank lines by typing `C-o` several times, or
by giving it a numeric argument to tell it how many blank lines to make.
See section [D.10 Numeric Arguments](emacs.html#SEC29), for how. If you have a fill prefix, then `C-o`
command inserts the fill prefix on the new line, when you use it at the
beginning of a line. See section [T.5.4 The Fill Prefix](emacs.html#SEC242).

The easy way to get rid of extra blank lines is with the command
`C-x C-o` (`delete-blank-lines`). `C-x C-o` in a run of
several blank lines deletes all but one of them. `C-x C-o` on a
solitary blank line deletes that blank line. When point is on a
nonblank line, `C-x C-o` deletes any blank lines following that
nonblank line.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC26) ] | [ [>](emacs.html#SEC28) ] | [ [<<](emacs.html#SEC28) ] | [ [Up](emacs.html#SEC19) ] | [ [>>](emacs.html#SEC31) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## D.8 Continuation Lines

If you add too many characters to one line without breaking it with
`RET`, the line grows to occupy two (or more) lines on the screen.
On graphical displays, Emacs indicates line wrapping with small bent
arrows in the fringes to the left and right of the window. On
text-only terminals, Emacs displays a `` `\' `` character at the right
margin of a screen line if it is not the last in its text line. This
`` `\' `` character says that the following screen line is not really a
distinct line in the text, just a *continuation* of a line too
long to fit the screen. Continuation is also called *line
wrapping*.

When line wrapping occurs before a character that is wider than one
column, some columns at the end of the previous screen line may be
"empty." In this case, Emacs displays additional `` `\' ``
characters in the "empty" columns, just before the `` `\' ``
character that indicates continuation.

Sometimes it is nice to have Emacs insert newlines automatically when
a line gets too long. Continuation on the screen does not do that. Use
Auto Fill mode (see section [T.5 Filling Text](emacs.html#SEC238)) if that's what you want.

As an alternative to continuation, Emacs can display long lines by
*truncation*. This means that all the characters that do not fit
in the width of the screen or window do not appear at all. They
remain in the buffer, temporarily invisible. On terminals, `` `$' ``
in the last column informs you that the line has been truncated on the
display. On window systems, a small straight arrow in the fringe to
the right of the window indicates a truncated line.

Truncation instead of continuation happens whenever horizontal
scrolling is in use, and optionally in all side-by-side windows
(see section [O. Multiple Windows](emacs.html#SEC183)). You can enable or disable truncation for a
particular buffer with the command `M-x toggle-truncate-lines`.

See section [J.12 Customization of Display](emacs.html#SEC89), for additional variables that affect how text is
displayed.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC27) ] | [ [>](emacs.html#SEC29) ] | [ [<<](emacs.html#SEC19) ] | [ [Up](emacs.html#SEC19) ] | [ [>>](emacs.html#SEC31) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## D.9 Cursor Position Information

Here are commands to get information about the size and position of
parts of the buffer, and to count lines.

`M-x what-page`: Display the page number of point, and the line number within the page. `M-x what-line`: Display the line number of point in the buffer. `M-x line-number-mode`: `M-x column-number-mode`: Toggle automatic display of current line number or column number. See section [J.10 Optional Mode Line Features](emacs.html#SEC87). `M-=`: Display the number of lines in the current region (`count-lines-region`). See section [H. The Mark and the Region](emacs.html#SEC52), for information about the region. `C-x =`: Display the character code of character after point, character position of point, and column of point (`what-cursor-position`). `M-x hl-line-mode`: Enable or disable highlighting of the current line. See section [J.13 Displaying the Cursor](emacs.html#SEC90).

There are two commands for working with line numbers. `M-x
what-line` computes the current line number and displays it in the echo
area. To go to a given line by number, use `M-x goto-line`; it
prompts you for the number. These line numbers count from one at the
beginning of the buffer.

You can also see the current line number in the mode line; see [B.3 The Mode Line](emacs.html#SEC11). If you narrow the buffer, then the line number in the mode line
is relative to the accessible portion (see section [AC.22 Narrowing](emacs.html#SEC462)). By contrast,
`what-line` shows both the line number relative to the narrowed
region and the line number relative to the whole buffer.

`M-x what-page` counts pages from the beginning of the file, and
counts lines within the page, showing both numbers in the echo area.
See section [T.4 Pages](emacs.html#SEC237).

While on this subject, we might as well mention `M-=` (`count-lines-region`),
which displays the number of lines in the region (see section [H. The Mark and the Region](emacs.html#SEC52)).
See section [T.4 Pages](emacs.html#SEC237), for the command `C-x l` which counts the lines in the
current page.

The command `C-x =` (`what-cursor-position`) can be used to find out
the column that the cursor is in, and other miscellaneous information about
point. It displays a line in the echo area that looks like this:

|  |  |
| --- | --- |
|  | ``` Char: c (0143, 99, 0x63)  point=21044 of 26883(78%)  column 53 ``` |

(In fact, this is the output produced when point is before the
`` `column' `` in the example.)

The four values after `` `Char:' `` describe the character that follows
point, first by showing it and then by giving its character code in
octal, decimal and hex. For a non-ASCII multibyte character, these are
followed by `` `ext' `` and the character's representation, in hex, in
the buffer's coding system, if that coding system encodes the character
safely and with a single byte (see section [Q.7 Coding Systems](emacs.html#SEC220)). If the
character's encoding is longer than one byte, Emacs shows `` `ext ...' ``.

`` `point=' `` is followed by the position of point expressed as a character
count. The front of the buffer counts as position 1, one character later
as 2, and so on. The next, larger, number is the total number of characters
in the buffer. Afterward in parentheses comes the position expressed as a
percentage of the total size.

`` `column' `` is followed by the horizontal position of point, in
columns from the left edge of the window.

If the buffer has been narrowed, making some of the text at the
beginning and the end temporarily inaccessible, `C-x =` displays
additional text describing the currently accessible range. For example, it
might display this:

|  |  |
| --- | --- |
|  | ``` Char: C (0103, 67, 0x43)  point=252 of 889(28%) <231 - 599>  column 0 ``` |

where the two extra numbers give the smallest and largest character
position that point is allowed to assume. The characters between those
two positions are the accessible ones. See section [AC.22 Narrowing](emacs.html#SEC462).

If point is at the end of the buffer (or the end of the accessible
part), the `C-x =` output does not describe a character after
point. The output might look like this:

|  |  |
| --- | --- |
|  | ``` point=26957 of 26956(100%)  column 0 ``` |

`C-u C-x =` displays additional information about a character,
in place of the buffer coordinates and column: the character set name
and the codes that identify the character within that character set;
ASCII characters are identified as belonging to the `ASCII`
character set. In addition, the full character encoding, even if it
takes more than a single byte, is shown after `` `ext' ``. Here's an
example for a Latin-1 character A with a grave accent in a buffer whose
coding system is iso-2022-7bit[(1)](emacs.html#FOOT1):

|  |  |
| --- | --- |
|  | ``` Char: À (04300, 2240, 0x8c0, ext ESC , A @) (latin-iso8859-1 64) ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC28) ] | [ [>](emacs.html#SEC30) ] | [ [<<](emacs.html#SEC21) ] | [ [Up](emacs.html#SEC19) ] | [ [>>](emacs.html#SEC31) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## D.10 Numeric Arguments

In mathematics and computer usage, the word *argument* means
"data provided to a function or operation." You can give any Emacs
command a *numeric argument* (also called a *prefix argument*).
Some commands interpret the argument as a repetition count. For
example, `C-f` with an argument of ten moves forward ten characters
instead of one. With these commands, no argument is equivalent to an
argument of one. Negative arguments tell most such commands to move or
act in the opposite direction.

If your terminal keyboard has a `META` key, the easiest way to
specify a numeric argument is to type digits and/or a minus sign while
holding down the `META` key. For example,

|  |  |
| --- | --- |
|  | ``` M-5 C-n ``` |

would move down five lines. The characters `Meta-1`, `Meta-2`,
and so on, as well as `Meta--`, do this because they are keys bound
to commands (`digit-argument` and `negative-argument`) that
are defined to contribute to an argument for the next command.
`Meta--` without digits normally means -1. Digits and
`-` modified with Control, or Control and Meta, also specify numeric
arguments.

Another way of specifying an argument is to use the `C-u`
(`universal-argument`) command followed by the digits of the
argument. With `C-u`, you can type the argument digits without
holding down modifier keys; `C-u` works on all terminals. To type a
negative argument, type a minus sign after `C-u`. Just a minus sign
without digits normally means -1.

`C-u` followed by a character which is neither a digit nor a minus
sign has the special meaning of "multiply by four." It multiplies the
argument for the next command by four. `C-u` twice multiplies it by
sixteen. Thus, `C-u C-u C-f` moves forward sixteen characters. This
is a good way to move forward "fast," since it moves about 1/5 of a line
in the usual size screen. Other useful combinations are `C-u C-n`,
`C-u C-u C-n` (move down a good fraction of a screen), `C-u C-u
C-o` (make "a lot" of blank lines), and `C-u C-k` (kill four
lines).

Some commands care only about whether there is an argument, and not about
its value. For example, the command `M-q` (`fill-paragraph`) with
no argument fills text; with an argument, it justifies the text as well.
(See section [T.5 Filling Text](emacs.html#SEC238), for more information on `M-q`.) Plain `C-u` is a
handy way of providing an argument for such commands.

Some commands use the value of the argument as a repeat count, but do
something peculiar when there is no argument. For example, the command
`C-k` (`kill-line`) with argument n kills n lines,
including their terminating newlines. But `C-k` with no argument is
special: it kills the text up to the next newline, or, if point is right at
the end of the line, it kills the newline itself. Thus, two `C-k`
commands with no arguments can kill a nonblank line, just like `C-k`
with an argument of one. (See section [H.7 Deletion and Killing](emacs.html#SEC59), for more information on
`C-k`.)

A few commands treat a plain `C-u` differently from an ordinary
argument. A few others may treat an argument of just a minus sign
differently from an argument of -1. These unusual cases are
described when they come up; they are always for reasons of convenience
of use of the individual command.

You can use a numeric argument to insert multiple copies of a
character. This is straightforward unless the character is a digit; for
example, `C-u 6 4 a` inserts 64 copies of the character `` `a' ``.
But this does not work for inserting digits; `C-u 6 4 1` specifies
an argument of 641, rather than inserting anything. To separate the
digit to insert from the argument, type another `C-u`; for example,
`C-u 6 4 C-u 1` does insert 64 copies of the character `` `1' ``.

We use the term "prefix argument" as well as "numeric argument" to
emphasize that you type the argument before the command, and to
distinguish these arguments from minibuffer arguments that come after
the command.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC29) ] | [ [>](emacs.html#SEC31) ] | [ [<<](emacs.html#SEC21) ] | [ [Up](emacs.html#SEC19) ] | [ [>>](emacs.html#SEC31) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## D.11 Repeating a Command

Many simple commands, such as those invoked with a single key or
with `M-x command-name RET`, can be repeated by
invoking them with a numeric argument that serves as a repeat count
(see section [D.10 Numeric Arguments](emacs.html#SEC29)). However, if the command you want to repeat
prompts for some input, or uses a numeric argument in another way,
repetition using a numeric argument might be problematical.

The command `C-x z` (`repeat`) provides another way to repeat
an Emacs command many times. This command repeats the previous Emacs
command, whatever that was. Repeating a command uses the same arguments
that were used before; it does not read new arguments each time.

To repeat the command more than once, type additional `z`'s: each
`z` repeats the command one more time. Repetition ends when you
type a character other than `z`, or press a mouse button.

For example, suppose you type `C-u 2 0 C-d` to delete 20
characters. You can repeat that command (including its argument) three
additional times, to delete a total of 80 characters, by typing `C-x
z z z`. The first `C-x z` repeats the command once, and each
subsequent `z` repeats it once again.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC30) ] | [ [>](emacs.html#SEC32) ] | [ [<<](emacs.html#SEC19) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC41) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# E. The Minibuffer

The *minibuffer* is the facility used by Emacs commands to read
arguments more complicated than a single number. Minibuffer arguments
can be file names, buffer names, Lisp function names, Emacs command
names, Lisp expressions, and many other things, depending on the command
reading the argument. You can use the usual Emacs editing commands in
the minibuffer to edit the argument text.

When the minibuffer is in use, it appears in the echo area, and the
terminal's cursor moves there. The beginning of the minibuffer line
displays a *prompt* which says what kind of input you should supply and
how it will be used. Often this prompt is derived from the name of the
command that the argument is for. The prompt normally ends with a colon.

Sometimes a *default argument* appears in parentheses after the
colon; it too is part of the prompt. The default will be used as the
argument value if you enter an empty argument (that is, just type
`RET`). For example, commands that read buffer names always show a
default, which is the name of the buffer that will be used if you type
just `RET`.

The simplest way to enter a minibuffer argument is to type the text
you want, terminated by `RET` which exits the minibuffer. You can
cancel the command that wants the argument, and get out of the
minibuffer, by typing `C-g`.

Since the minibuffer uses the screen space of the echo area, it can
conflict with other ways Emacs customarily uses the echo area. Here is how
Emacs handles such conflicts:

* If a command gets an error while you are in the minibuffer, this does
  not cancel the minibuffer. However, the echo area is needed for the
  error message and therefore the minibuffer itself is hidden for a
  while. It comes back after a few seconds, or as soon as you type
  anything.

  * If in the minibuffer you use a command whose purpose is to display a
    message in the echo area, such as `C-x =`, the message hides the
    minibuffer for a while. The minibuffer contents come back after a few
    seconds, or as soon as you type anything.

    * Echoing of keystrokes does not take place while the minibuffer is in
      use.

> |  |  |  |
> | --- | --- | --- |
> | [E.1 Minibuffers for File Names](emacs.html#SEC32) |  | Entering file names with the minibuffer. |
> | [E.2 Editing in the Minibuffer](emacs.html#SEC33) |  | How to edit in the minibuffer. |
> | [E.3 Completion](emacs.html#SEC34) |  | An abbreviation facility for minibuffer input. |
> | [E.4 Minibuffer History](emacs.html#SEC39) |  | Reusing recent minibuffer arguments. |
> | [E.5 Repeating Minibuffer Commands](emacs.html#SEC40) |  | Re-executing commands that used the minibuffer. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC31) ] | [ [>](emacs.html#SEC33) ] | [ [<<](emacs.html#SEC31) ] | [ [Up](emacs.html#SEC31) ] | [ [>>](emacs.html#SEC41) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## E.1 Minibuffers for File Names

Sometimes the minibuffer starts out with text in it. For example, when
you are supposed to give a file name, the minibuffer starts out containing
the *default directory*, which ends with a slash. This is to inform
you which directory the file will be found in if you do not specify a
directory.

For example, the minibuffer might start out with these contents:

|  |  |
| --- | --- |
|  | ``` Find File: /u2/emacs/src/ ``` |

where `` `Find File: ' `` is the prompt. Typing `buffer.c`
specifies the file `/u2/emacs/src/buffer.c'. To find files in
nearby directories, use `..`; thus, if you type
`../lisp/simple.el`, you will get the file named
`/u2/emacs/lisp/simple.el'. Alternatively, you can kill with
`M-DEL` the directory names you don't want (see section [T.1 Words](emacs.html#SEC234)).

If you don't want any of the default, you can kill it with `C-a
C-k`. But you don't need to kill the default; you can simply ignore it.
Insert an absolute file name, one starting with a slash or a tilde,
after the default directory. For example, to specify the file
`/etc/termcap', just insert that name, giving these minibuffer
contents:

|  |  |
| --- | --- |
|  | ``` Find File: /u2/emacs/src//etc/termcap ``` |

GNU Emacs gives a special meaning to a double slash (which is not
normally a useful thing to write): it means, "ignore everything before
the second slash in the pair." Thus, `` `/u2/emacs/src/' `` is ignored
in the example above, and you get the file `/etc/termcap'.

If you set `insert-default-directory` to `nil`, the default
directory is not inserted in the minibuffer. This way, the minibuffer
starts out empty. But the name you type, if relative, is still
interpreted with respect to the same default directory.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC32) ] | [ [>](emacs.html#SEC34) ] | [ [<<](emacs.html#SEC34) ] | [ [Up](emacs.html#SEC31) ] | [ [>>](emacs.html#SEC41) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## E.2 Editing in the Minibuffer

The minibuffer is an Emacs buffer (albeit a peculiar one), and the usual
Emacs commands are available for editing the text of an argument you are
entering.

Since `RET` in the minibuffer is defined to exit the minibuffer,
you can't use it to insert a newline in the minibuffer. To do that,
type `C-o` or `C-q C-j`. (Recall that a newline is really the
character control-J.)

The minibuffer has its own window which always has space on the screen
but acts as if it were not there when the minibuffer is not in use. When
the minibuffer is in use, its window is just like the others; you can
switch to another window with `C-x o`, edit text in other windows and
perhaps even visit more files, before returning to the minibuffer to submit
the argument. You can kill text in another window, return to the
minibuffer window, and then yank the text to use it in the argument.
See section [O. Multiple Windows](emacs.html#SEC183).

There are some restrictions on the use of the minibuffer window,
however. You cannot switch buffers in it--the minibuffer and its
window are permanently attached. Also, you cannot split or kill the
minibuffer window. But you can make it taller in the normal fashion
with `C-x ^`.

The minibuffer window expands vertically as necessary to hold the
text that you put in the minibuffer, if `resize-mini-windows` is
non-`nil`. If `resize-mini-windows` is `t`, the window
is always resized to fit the size of the text it displays. If
`resize-mini-windows` is the symbol `grow-only`, the window
grows when the size of displayed text increases, but shrinks (back to
the normal size) only when the minibuffer becomes inactive.

The variable `max-mini-window-height` controls the maximum
height for resizing the minibuffer window: a floating-point number
specifies a fraction of the frame's height; an integer specifies the
maximum number of lines; `nil` means do not resize the minibuffer
window automatically. The default value is 0.25.

If while in the minibuffer you issue a command that displays help text
of any sort in another window, you can use the `C-M-v` command while
in the minibuffer to scroll the help text. This lasts until you exit
the minibuffer. This feature is especially useful when you display
a buffer listing possible completions. See section [O.3 Using Other Windows](emacs.html#SEC186).

Emacs normally disallows most commands that use the minibuffer while
the minibuffer is active. This rule is to prevent recursive minibuffers
from confusing novice users. If you want to be able to use such
commands in the minibuffer, set the variable
`enable-recursive-minibuffers` to a non-`nil` value.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC33) ] | [ [>](emacs.html#SEC35) ] | [ [<<](emacs.html#SEC39) ] | [ [Up](emacs.html#SEC31) ] | [ [>>](emacs.html#SEC39) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## E.3 Completion

For certain kinds of arguments, you can use *completion* to enter
the argument value. Completion means that you type part of the
argument, then Emacs visibly fills in the rest, or as much as
can be determined from the part you have typed.

When completion is available, certain keys---`TAB`, `RET`, and
`SPC`---are rebound to complete the text present in the minibuffer
into a longer string that it stands for, by matching it against a set of
*completion alternatives* provided by the command reading the
argument. `?` is defined to display a list of possible completions
of what you have inserted.

For example, when `M-x` uses the minibuffer to read the name of a
command, it provides a list of all available Emacs command names to
complete against. The completion keys match the text in the minibuffer
against all the command names, find any additional name characters
implied by the ones already present in the minibuffer, and add those
characters to the ones you have given. This is what makes it possible
to type `M-x ins SPC b RET` instead of `M-x
insert-buffer RET` (for example).

Case is normally significant in completion, because it is significant
in most of the names that you can complete (buffer names, file names and
command names). Thus, `` `fo' `` does not complete to `` `Foo' ``.
Completion does ignore case distinctions for certain arguments in which
case does not matter.

> |  |  |  |
> | --- | --- | --- |
> | [E.3.1 Completion Example](emacs.html#SEC35) |  |  |
> | [E.3.2 Completion Commands](emacs.html#SEC36) |  |  |
> | [E.3.3 Strict Completion](emacs.html#SEC37) |  |  |
> | [E.3.4 Completion Options](emacs.html#SEC38) |  |  |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC34) ] | [ [>](emacs.html#SEC36) ] | [ [<<](emacs.html#SEC39) ] | [ [Up](emacs.html#SEC34) ] | [ [>>](emacs.html#SEC39) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### E.3.1 Completion Example

A concrete example may help here. If you type `M-x au TAB`,
the `TAB` looks for alternatives (in this case, command names) that
start with `` `au' ``. There are several, including
`auto-fill-mode` and `auto-save-mode`---but they are all the
same as far as `auto-`, so the `` `au' `` in the minibuffer changes
to `` `auto-' ``.

If you type `TAB` again immediately, there are multiple
possibilities for the very next character--it could be any of
`` `cfilrs' ``---so no more characters are added; instead, `TAB`
displays a list of all possible completions in another window.

If you go on to type `f TAB`, this `TAB` sees
`` `auto-f' ``. The only command name starting this way is
`auto-fill-mode`, so completion fills in the rest of that. You now
have `` `auto-fill-mode' `` in the minibuffer after typing just `au
TAB f TAB`. Note that `TAB` has this effect because in
the minibuffer it is bound to the command `minibuffer-complete`
when completion is available.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC35) ] | [ [>](emacs.html#SEC37) ] | [ [<<](emacs.html#SEC37) ] | [ [Up](emacs.html#SEC34) ] | [ [>>](emacs.html#SEC39) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### E.3.2 Completion Commands

Here is a list of the completion commands defined in the minibuffer
when completion is available.

`TAB`: Complete the text in the minibuffer as much as possible (`minibuffer-complete`). `SPC`: Complete the minibuffer text, but don't go beyond one word (`minibuffer-complete-word`). `RET`: Submit the text in the minibuffer as the argument, possibly completing first as described below (`minibuffer-complete-and-exit`). `?`: Display a list of all possible completions of the text in the minibuffer (`minibuffer-list-completions`).

`SPC` completes much like `TAB`, but never goes beyond the
next hyphen or space. If you have `` `auto-f' `` in the minibuffer and
type `SPC`, it finds that the completion is `` `auto-fill-mode' ``,
but it stops completing after `` `fill-' ``. This gives
`` `auto-fill-' ``. Another `SPC` at this point completes all the
way to `` `auto-fill-mode' ``. The command that implements this
behavior is called `minibuffer-complete-word`.

Here are some commands you can use to choose a completion from a
window that displays a list of completions:

`Mouse-2`: Clicking mouse button 2 on a completion in the list of possible completions chooses that completion (`mouse-choose-completion`). You normally use this command while point is in the minibuffer, but you must click in the list of completions, not in the minibuffer itself. `PRIOR`: `M-v`: Typing `PRIOR` or `PAGE-UP`, or `M-v`, while in the minibuffer, selects the window showing the completion list buffer (`switch-to-completions`). This paves the way for using the commands below. (Selecting that window in the usual ways has the same effect, but this way is more convenient.) `RET`: Typing `RET` *in the completion list buffer* chooses the completion that point is in or next to (`choose-completion`). To use this command, you must first switch windows to the window that shows the list of completions. `RIGHT`: Typing the right-arrow key `RIGHT` *in the completion list buffer* moves point to the following completion (`next-completion`). `LEFT`: Typing the left-arrow key `LEFT` *in the completion list buffer* moves point toward the beginning of the buffer, to the previous completion (`previous-completion`).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC36) ] | [ [>](emacs.html#SEC38) ] | [ [<<](emacs.html#SEC38) ] | [ [Up](emacs.html#SEC34) ] | [ [>>](emacs.html#SEC39) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### E.3.3 Strict Completion

There are three different ways that `RET` can work in completing
minibuffers, depending on how the argument will be used.

* *Strict* completion is used when it is meaningless to give any
  argument except one of the known alternatives. For example, when
  `C-x k` reads the name of a buffer to kill, it is meaningless to
  give anything but the name of an existing buffer. In strict
  completion, `RET` refuses to exit if the text in the minibuffer
  does not complete to an exact match.

  * *Cautious* completion is similar to strict completion, except that
    `RET` exits only if the text was an exact match already, not
    needing completion. If the text is not an exact match, `RET` does
    not exit, but it does complete the text. If it completes to an exact
    match, a second `RET` will exit.

    Cautious completion is used for reading file names for files that must
    already exist.

    * *Permissive* completion is used when any string whatever is
      meaningful, and the list of completion alternatives is just a guide.
      For example, when `C-x C-f` reads the name of a file to visit, any
      file name is allowed, in case you want to create a file. In
      permissive completion, `RET` takes the text in the minibuffer
      exactly as given, without completing it.

The completion commands display a list of all possible completions in
a window whenever there is more than one possibility for the very next
character. Also, typing `?` explicitly requests such a list. If
the list of completions is long, you can scroll it with `C-M-v`
(see section [O.3 Using Other Windows](emacs.html#SEC186)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC37) ] | [ [>](emacs.html#SEC39) ] | [ [<<](emacs.html#SEC39) ] | [ [Up](emacs.html#SEC34) ] | [ [>>](emacs.html#SEC39) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
