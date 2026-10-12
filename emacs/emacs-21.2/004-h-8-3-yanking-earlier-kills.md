### H.8.3 Yanking Earlier Kills

To recover killed text that is no longer the most recent kill, use the
`M-y` command (`yank-pop`). It takes the text previously
yanked and replaces it with the text from an earlier kill. So, to
recover the text of the next-to-the-last kill, first use `C-y` to
yank the last kill, and then use `M-y` to replace it with the
previous kill. `M-y` is allowed only after a `C-y` or another
`M-y`.

You can understand `M-y` in terms of a "last yank" pointer which
points at an entry in the kill ring. Each time you kill, the "last
yank" pointer moves to the newly made entry at the front of the ring.
`C-y` yanks the entry which the "last yank" pointer points to.
`M-y` moves the "last yank" pointer to a different entry, and the
text in the buffer changes to match. Enough `M-y` commands can move
the pointer to any entry in the ring, so you can get any entry into the
buffer. Eventually the pointer reaches the end of the ring; the next
`M-y` loops back around to the first entry again.

`M-y` moves the "last yank" pointer around the ring, but it does
not change the order of the entries in the ring, which always runs from
the most recent kill at the front to the oldest one still remembered.

`M-y` can take a numeric argument, which tells it how many entries
to advance the "last yank" pointer by. A negative argument moves the
pointer toward the front of the ring; from the front of the ring, it
moves "around" to the last entry and continues forward from there.

Once the text you are looking for is brought into the buffer, you can
stop doing `M-y` commands and it will stay there. It's just a copy
of the kill ring entry, so editing it in the buffer does not change
what's in the ring. As long as no new killing is done, the "last
yank" pointer remains at the same place in the kill ring, so repeating
`C-y` will yank another copy of the same previous kill.

If you know how many `M-y` commands it would take to find the
text you want, you can yank that text in one step using `C-y` with
a numeric argument. `C-y` with an argument restores the text from
the specified kill ring entry, counting back from the most recent as
1. Thus, `C-u 2 C-y` gets the next-to-the-last block of killed
text--it is equivalent to `C-y M-y`. `C-y` with a numeric
argument starts counting from the "last yank" pointer, and sets the
"last yank" pointer to the entry that it yanks.

The length of the kill ring is controlled by the variable
`kill-ring-max`; no more than that many blocks of killed text are
saved.

The actual contents of the kill ring are stored in a variable named
`kill-ring`; you can view the entire contents of the kill ring with
the command `C-h v kill-ring`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC66) ] | [ [>](emacs.html#SEC68) ] | [ [<<](emacs.html#SEC63) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC68) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## H.9 Accumulating Text

Usually we copy or move text by killing it and yanking it, but there
are other methods convenient for copying one block of text in many
places, or for copying many scattered blocks of text into one place. To
copy one block to many places, store it in a register
(see section [I. Registers](emacs.html#SEC69)). Here we describe the commands to accumulate
scattered pieces of text into a buffer or into a file.

`M-x append-to-buffer`: Append region to the contents of a specified buffer. `M-x prepend-to-buffer`: Prepend region to the contents of a specified buffer. `M-x copy-to-buffer`: Copy region into a specified buffer, deleting that buffer's old contents. `M-x insert-buffer`: Insert the contents of a specified buffer into current buffer at point. `M-x append-to-file`: Append region to the contents of a specified file, at the end.

To accumulate text into a buffer, use `M-x append-to-buffer`.
This reads a buffer name, then inserts a copy of the region into the
buffer specified. If you specify a nonexistent buffer,
`append-to-buffer` creates the buffer. The text is inserted
wherever point is in that buffer. If you have been using the buffer for
editing, the copied text goes into the middle of the text of the buffer,
starting from wherever point happens to be at that moment.

Point in that buffer is left at the end of the copied text, so
successive uses of `append-to-buffer` accumulate the text in the
specified buffer in the same order as they were copied. Strictly
speaking, `append-to-buffer` does not always append to the text
already in the buffer--it appends only if point in that buffer is at the end.
However, if `append-to-buffer` is the only command you use to alter
a buffer, then point is always at the end.

`M-x prepend-to-buffer` is just like `append-to-buffer`
except that point in the other buffer is left before the copied text, so
successive prependings add text in reverse order. `M-x
copy-to-buffer` is similar, except that any existing text in the other
buffer is deleted, so the buffer is left containing just the text newly
copied into it.

To retrieve the accumulated text from another buffer, use the
command `M-x insert-buffer`; this too takes buffername as an
argument. It inserts a copy of the whole text in buffer
buffername into the current buffer at point, and sets the mark
after the inserted text. Alternatively, you can select the other
buffer for editing, then copy text from it by killing.
See section [N. Using Multiple Buffers](emacs.html#SEC172), for background information on buffers.

Instead of accumulating text within Emacs, in a buffer, you can append
text directly into a file with `M-x append-to-file`, which takes
filename as an argument. It adds the text of the region to the end
of the specified file. The file is changed immediately on disk.

You should use `append-to-file` only with files that are
*not* being visited in Emacs. Using it on a file that you are
editing in Emacs would change the file behind Emacs's back, which
can lead to losing some of your editing.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC67) ] | [ [>](emacs.html#SEC69) ] | [ [<<](emacs.html#SEC67) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC69) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## H.10 Rectangles

The rectangle commands operate on rectangular areas of the text: all
the characters between a certain pair of columns, in a certain range of
lines. Commands are provided to kill rectangles, yank killed rectangles,
clear them out, fill them with blanks or text, or delete them. Rectangle
commands are useful with text in multicolumn formats, and for changing
text into or out of such formats.

When you must specify a rectangle for a command to work on, you do it
by putting the mark at one corner and point at the opposite corner. The
rectangle thus specified is called the *region-rectangle* because
you control it in much the same way as the region is controlled. But
remember that a given combination of point and mark values can be
interpreted either as a region or as a rectangle, depending on the
command that uses them.

If point and the mark are in the same column, the rectangle they
delimit is empty. If they are in the same line, the rectangle is one
line high. This asymmetry between lines and columns comes about
because point (and likewise the mark) is between two columns, but within
a line.

`C-x r k`: Kill the text of the region-rectangle, saving its contents as the "last killed rectangle" (`kill-rectangle`). `C-x r d`: Delete the text of the region-rectangle (`delete-rectangle`). `C-x r y`: Yank the last killed rectangle with its upper left corner at point (`yank-rectangle`). `C-x r o`: Insert blank space to fill the space of the region-rectangle (`open-rectangle`). This pushes the previous contents of the region-rectangle rightward. `M-x clear-rectangle`: Clear the region-rectangle by replacing its contents with spaces. `M-x delete-whitespace-rectangle`: Delete whitespace in each of the lines on the specified rectangle, starting from the left edge column of the rectangle. `C-x r t string RET`: Replace rectangle contents with string on each line. (`string-rectangle`). `M-x string-insert-rectangle RET string RET`: Insert string on each line of the rectangle.

The rectangle operations fall into two classes: commands for
deleting and inserting rectangles, and commands for blank rectangles.

There are two ways to get rid of the text in a rectangle: you can
discard the text (delete it) or save it as the "last killed"
rectangle. The commands for these two ways are `C-x r d`
(`delete-rectangle`) and `C-x r k` (`kill-rectangle`). In
either case, the portion of each line that falls inside the rectangle's
boundaries is deleted, causing any following text on the line to
move left into the gap.

Note that "killing" a rectangle is not killing in the usual sense; the
rectangle is not stored in the kill ring, but in a special place that
can only record the most recent rectangle killed. This is because yanking
a rectangle is so different from yanking linear text that different yank
commands have to be used and yank-popping is hard to make sense of.

To yank the last killed rectangle, type `C-x r y`
(`yank-rectangle`). Yanking a rectangle is the opposite of killing
one. Point specifies where to put the rectangle's upper left corner.
The rectangle's first line is inserted there, the rectangle's second
line is inserted at the same horizontal position, but one line
vertically down, and so on. The number of lines affected is determined
by the height of the saved rectangle.

You can convert single-column lists into double-column lists using
rectangle killing and yanking; kill the second half of the list as a
rectangle and then yank it beside the first line of the list.
See section [AC.23 Two-Column Editing](emacs.html#SEC463), for another way to edit multi-column text.

You can also copy rectangles into and out of registers with `C-x r
r r` and `C-x r i r`. See section [Rectangle Registers](emacs.html#SEC72).

There are two commands you can use for making blank rectangles:
`M-x clear-rectangle` which blanks out existing text, and `C-x r
o` (`open-rectangle`) which inserts a blank rectangle. Clearing a
rectangle is equivalent to deleting it and then inserting a blank
rectangle of the same size.

The command `M-x delete-whitespace-rectangle` deletes horizontal
whitespace starting from a particular column. This applies to each of
the lines in the rectangle, and the column is specified by the left
edge of the rectangle. The right edge of the rectangle does not make
any difference to this command.

The command `C-x r t` (`string-rectangle`) replaces the
contents of a region-rectangle with a string on each line. The
string's width need not be the same as the width of the rectangle. If
the string's width is less, the text after the rectangle shifts left;
if the string is wider than the rectangle, the text after the
rectangle shifts right.

The command `M-x string-insert-rectangle` is similar to
`string-rectangle`, but inserts the string on each line,
shifting the original text to the right.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC68) ] | [ [>](emacs.html#SEC70) ] | [ [<<](emacs.html#SEC68) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC77) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# I. Registers

Emacs *registers* are compartments where you can save text,
rectangles, positions, and other things for later use. Once you save
text or a rectangle in a register, you can copy it into the buffer
once, or many times; you can move point to a position saved in a
register once, or many times.

Each register has a name, which consists of a single character.
A register can
store a piece of text, a rectangle, a position, a window configuration,
or a file name, but only one thing at any given time. Whatever you
store in a register remains there until you store something else in that
register. To see what a register r contains, use `M-x
view-register`.

`M-x view-register RET r`: Display a description of what register r contains.

> |  |  |  |
> | --- | --- | --- |
> | [I.1 Saving Positions in Registers](emacs.html#SEC70) |  | Saving positions in registers. |
> | [I.2 Saving Text in Registers](emacs.html#SEC71) |  | Saving text in registers. |
> | [I.3 Saving Rectangles in Registers](emacs.html#SEC72) |  | Saving rectangles in registers. |
> | [I.4 Saving Window Configurations in Registers](emacs.html#SEC73) |  | Saving window configurations in registers. |
> | [I.6 Keeping File Names in Registers](emacs.html#SEC75) |  | File names in registers. |
> | [I.5 Keeping Numbers in Registers](emacs.html#SEC74) |  | Numbers in registers. |
> | [I.7 Bookmarks](emacs.html#SEC76) |  | Bookmarks are like registers, but persistent. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC69) ] | [ [>](emacs.html#SEC71) ] | [ [<<](emacs.html#SEC69) ] | [ [Up](emacs.html#SEC69) ] | [ [>>](emacs.html#SEC77) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## I.1 Saving Positions in Registers

Saving a position records a place in a buffer so that you can move
back there later. Moving to a saved position switches to that buffer
and moves point to that place in it.

`C-x r SPC r`: Save position of point in register r (`point-to-register`). `C-x r j r`: Jump to the position saved in register r (`jump-to-register`).

To save the current position of point in a register, choose a name
r and type `C-x r SPC r`. The register r
retains the position thus saved until you store something else in that
register.

The command `C-x r j r` moves point to the position recorded
in register r. The register is not affected; it continues to
hold the same position. You can jump to the saved position any number
of times.

If you use `C-x r j` to go to a saved position, but the buffer it
was saved from has been killed, `C-x r j` tries to create the buffer
again by visiting the same file. Of course, this works only for buffers
that were visiting files.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC70) ] | [ [>](emacs.html#SEC72) ] | [ [<<](emacs.html#SEC72) ] | [ [Up](emacs.html#SEC69) ] | [ [>>](emacs.html#SEC77) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## I.2 Saving Text in Registers

When you want to insert a copy of the same piece of text several
times, it may be inconvenient to yank it from the kill ring, since each
subsequent kill moves that entry further down the ring. An alternative
is to store the text in a register and later retrieve it.

`C-x r s r`: Copy region into register r (`copy-to-register`). `C-x r i r`: Insert text from register r (`insert-register`). `M-x append-to-register RET r`: Append region to text in register r. `M-x prepend-to-register RET r`: Prepend region to text in register r.

`C-x r s r` stores a copy of the text of the region into
the register named r. `C-u C-x r s r`, the same
command with a numeric argument, deletes the text from the buffer as
well; you can think of this as "moving" the region text into the register.

`M-x append-to-register RET r` appends the copy of
the text in the region to the text already stored in the register
named r. If invoked with a numeric argument, it deletes the
region after appending it to the register. A similar command
`prepend-to-register` works the same, except that it
*prepends* the region text to the text in the register, rather
than *appending* it.

`C-x r i r` inserts in the buffer the text from register
r. Normally it leaves point before the text and places the mark
after, but with a numeric argument (`C-u`) it puts point after the
text and the mark before.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC71) ] | [ [>](emacs.html#SEC73) ] | [ [<<](emacs.html#SEC73) ] | [ [Up](emacs.html#SEC69) ] | [ [>>](emacs.html#SEC77) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## I.3 Saving Rectangles in Registers

A register can contain a rectangle instead of linear text. The
rectangle is represented as a list of strings. See section [H.10 Rectangles](emacs.html#SEC68), for
basic information on how to specify a rectangle in the buffer.

`C-x r r r`: Copy the region-rectangle into register r (`copy-rectangle-to-register`). With numeric argument, delete it as well. `C-x r i r`: Insert the rectangle stored in register r (if it contains a rectangle) (`insert-register`).

The `C-x r i r` command inserts a text string if the
register contains one, and inserts a rectangle if the register contains
one.

See also the command `sort-columns`, which you can think of
as sorting a rectangle. See section [AC.21 Sorting Text](emacs.html#SEC461).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC72) ] | [ [>](emacs.html#SEC74) ] | [ [<<](emacs.html#SEC74) ] | [ [Up](emacs.html#SEC69) ] | [ [>>](emacs.html#SEC77) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## I.4 Saving Window Configurations in Registers

You can save the window configuration of the selected frame in a
register, or even the configuration of all windows in all frames, and
restore the configuration later.

`C-x r w r`: Save the state of the selected frame's windows in register r (`window-configuration-to-register`). `C-x r f r`: Save the state of all frames, including all their windows, in register r (`frame-configuration-to-register`).

Use `C-x r j r` to restore a window or frame configuration.
This is the same command used to restore a cursor position. When you
restore a frame configuration, any existing frames not included in the
configuration become invisible. If you wish to delete these frames
instead, use `C-u C-x r j r`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC73) ] | [ [>](emacs.html#SEC75) ] | [ [<<](emacs.html#SEC75) ] | [ [Up](emacs.html#SEC69) ] | [ [>>](emacs.html#SEC77) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## I.5 Keeping Numbers in Registers

There are commands to store a number in a register, to insert
the number in the buffer in decimal, and to increment it. These commands
can be useful in keyboard macros (see section [AD.3 Keyboard Macros](emacs.html#SEC487)).

`C-u number C-x r n r`: Store number into register r (`number-to-register`). `C-u number C-x r + r`: Increment the number in register r by number (`increment-register`). `C-x r g r`: Insert the number from register r into the buffer.

`C-x r g` is the same command used to insert any other sort of
register contents into the buffer. `C-x r +` with no numeric
argument increments the register value by 1; `C-x r n` with no
numeric argument stores zero in the register.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC74) ] | [ [>](emacs.html#SEC76) ] | [ [<<](emacs.html#SEC76) ] | [ [Up](emacs.html#SEC69) ] | [ [>>](emacs.html#SEC77) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## I.6 Keeping File Names in Registers

If you visit certain file names frequently, you can visit them more
conveniently if you put their names in registers. Here's the Lisp code
used to put a file name in a register:

|  |  |
| --- | --- |
|  | ``` (set-register ?r '(file . name)) ``` |

For example,

|  |  |
| --- | --- |
|  | ``` (set-register ?z '(file . "/gd/gnu/emacs/19.0/src/ChangeLog")) ``` |

puts the file name shown in register `` `z' ``.

To visit the file whose name is in register r, type `C-x r j
r`. (This is the same command used to jump to a position or
restore a frame configuration.)

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC75) ] | [ [>](emacs.html#SEC77) ] | [ [<<](emacs.html#SEC69) ] | [ [Up](emacs.html#SEC69) ] | [ [>>](emacs.html#SEC77) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## I.7 Bookmarks

*Bookmarks* are somewhat like registers in that they record
positions you can jump to. Unlike registers, they have long names, and
they persist automatically from one Emacs session to the next. The
prototypical use of bookmarks is to record "where you were reading" in
various files.

`C-x r m RET`: Set the bookmark for the visited file, at point. `C-x r m bookmark RET`: Set the bookmark named bookmark at point (`bookmark-set`). `C-x r b bookmark RET`: Jump to the bookmark named bookmark (`bookmark-jump`). `C-x r l`: List all bookmarks (`list-bookmarks`). `M-x bookmark-save`: Save all the current bookmark values in the default bookmark file.

The prototypical use for bookmarks is to record one current position
in each of several files. So the command `C-x r m`, which sets a
bookmark, uses the visited file name as the default for the bookmark
name. If you name each bookmark after the file it points to, then you
can conveniently revisit any of those files with `C-x r b`, and move
to the position of the bookmark at the same time.

To display a list of all your bookmarks in a separate buffer, type
`C-x r l` (`list-bookmarks`). If you switch to that buffer,
you can use it to edit your bookmark definitions or annotate the
bookmarks. Type `C-h m` in the bookmark buffer for more
information about its special editing commands.

When you kill Emacs, Emacs offers to save your bookmark values in your
default bookmark file, `~/.emacs.bmk', if you have changed any
bookmark values. You can also save the bookmarks at any time with the
`M-x bookmark-save` command. The bookmark commands load your
default bookmark file automatically. This saving and loading is how
bookmarks persist from one Emacs session to the next.

If you set the variable `bookmark-save-flag` to 1, then each
command that sets a bookmark will also save your bookmarks; this way,
you don't lose any bookmark values even if Emacs crashes. (The value,
if a number, says how many bookmark modifications should go by between
saving.)

Bookmark position values are saved with surrounding context, so that
`bookmark-jump` can find the proper position even if the file is
modified slightly. The variable `bookmark-search-size` says how
many characters of context to record on each side of the bookmark's
position.

Here are some additional commands for working with bookmarks:

`M-x bookmark-load RET filename RET`: Load a file named filename that contains a list of bookmark values. You can use this command, as well as `bookmark-write`, to work with other files of bookmark values in addition to your default bookmark file. `M-x bookmark-write RET filename RET`: Save all the current bookmark values in the file filename. `M-x bookmark-delete RET bookmark RET`: Delete the bookmark named bookmark. `M-x bookmark-insert-location RET bookmark RET`: Insert in the buffer the name of the file that bookmark bookmark points to. `M-x bookmark-insert RET bookmark RET`: Insert in the buffer the *contents* of the file that bookmark bookmark points to.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC76) ] | [ [>](emacs.html#SEC78) ] | [ [<<](emacs.html#SEC69) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC91) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# J. Controlling the Display

Since only part of a large buffer fits in the window, Emacs tries to
show a part that is likely to be interesting. Display-control commands
allow you to specify which part of the text you want to see, and how to
display it.

> |  |  |  |
> | --- | --- | --- |
> | [J.1 Using Multiple Typefaces](emacs.html#SEC78) |  | How to change the display style using faces. |
> | [J.2 Font Lock mode](emacs.html#SEC79) |  | Minor mode for syntactic highlighting using faces. |
> | [J.3 Highlight Changes Mode](emacs.html#SEC80) |  | Using colors to show where you changed the buffer. |
> | [J.4 Interactive Highlighting by Matching](emacs.html#SEC81) |  | Tell Emacs what text to highlight. |
> | [J.5 Trailing Whitespace](emacs.html#SEC82) |  | Showing possibly-spurious trailing whitespace. |
> | [J.6 Scrolling](emacs.html#SEC83) |  | Moving text up and down in a window. |
> | [J.7 Horizontal Scrolling](emacs.html#SEC84) |  | Moving text left and right in a window. |
> | [J.8 Follow Mode](emacs.html#SEC85) |  | Follow mode lets two windows scroll as one. |
> | [J.9 Selective Display](emacs.html#SEC86) |  | Hiding lines with lots of indentation. |
> | [J.10 Optional Mode Line Features](emacs.html#SEC87) |  | Optional mode line display features. |
> | [J.11 How Text Is Displayed](emacs.html#SEC88) |  | How text characters are normally displayed. |
> | [J.12 Customization of Display](emacs.html#SEC89) |  | Information on variables for customizing display. |
> | [J.13 Displaying the Cursor](emacs.html#SEC90) |  | Features for displaying the cursor. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC77) ] | [ [>](emacs.html#SEC79) ] | [ [<<](emacs.html#SEC77) ] | [ [Up](emacs.html#SEC77) ] | [ [>>](emacs.html#SEC91) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## J.1 Using Multiple Typefaces

When using Emacs with a window system, you can set up multiple
styles of displaying characters. Each style is called a *face*.
Each face can specify various attributes, such as the height, weight
and slant of the characters, the foreground and background color, and
underlining. But it does not have to specify all of them.

Emacs on a character terminal supports only part of face attributes.
Which attributes are supported depends on your display type, but many
displays support inverse video, bold, and underline attributes, and
some support colors.

Features which rely on text in multiple faces (such as Font Lock mode)
will also work on non-windowed terminals that can display more than one
face, whether by colors or underlining and emboldening. This includes
the console on GNU/Linux, an `xterm` which supports colors, the
MS-DOS display (see section [AH. Emacs and MS-DOS](emacs.html#SEC554)), and the MS-Windows version invoked with
the `` `-nw' `` option. Emacs determines automatically whether the
terminal has this capability.

You control the appearance of a part of the text in the buffer by
specifying the face or faces to use for it. The style of display used
for any given character is determined by combining the attributes of
all the applicable faces specified for that character. Any attribute
that isn't specified by these faces is taken from the `default` face,
whose attributes reflect the default settings of the frame itself.

Enriched mode, the mode for editing formatted text, includes several
commands and menus for specifying faces for text in the buffer.
See section [T.11.4 Faces in Formatted Text](emacs.html#SEC262), for how to specify the font for text in the
buffer. See section [T.11.5 Colors in Formatted Text](emacs.html#SEC263), for how to specify the foreground and
background color.

To alter the appearance of a face, use the customization buffer.
See section [AD.2.2.3 Customizing Faces](emacs.html#SEC482). You can also use X resources to specify
attributes of particular faces (see section [AE.13 X Resources](emacs.html#SEC543)). Alternatively,
you can change the foreground and background colors of a specific face
with `M-x set-face-foreground` and `M-x set-face-background`.
These commands prompt in the minibuffer for a face name and a color
name, with completion, and then set that face to use the specified
color. Changing the colors of the `default` face also changes
the foreground and background colors on all frames, both existing and
those to be created in the future. (You can also set foreground and
background colors for the current frame only; see [P.12 Setting Frame Parameters](emacs.html#SEC203).)

Emacs 21 can correctly display variable-width fonts, but Emacs
commands that calculate width and indentation do not know how to
calculate variable widths. This can sometimes lead to incorrect
results when you use variable-width fonts. In particular, indentation
commands can give inconsistent results, so we recommend you avoid
variable-width fonts for editing program source code. Filling will
sometimes make lines too long or too short. We plan to address these
issues in future Emacs versions.

To see what faces are currently defined, and what they look like, type
`M-x list-faces-display`. It's possible for a given face to look
different in different frames; this command shows the appearance in the
frame in which you type it. Here's a list of the standard defined
faces:

`default`: This face is used for ordinary text that doesn't specify any other face. `mode-line`: This face is used for mode lines. By default, it's drawn with shadows for a "raised" effect on window systems, and drawn as the inverse of the default face on non-windowed terminals. See section [J.12 Customization of Display](emacs.html#SEC89). `header-line`: Similar to `mode-line` for a window's header line. Most modes don't use the header line, but the Info mode does. `highlight`: This face is used for highlighting portions of text, in various modes. For example, mouse-sensitive text is highlighted using this face. `isearch`: This face is used for highlighting Isearch matches. `isearch-lazy-highlight-face`: This face is used for lazy highlighting of Isearch matches other than the current one. `region`: This face is used for displaying a selected region (when Transient Mark mode is enabled--see below). `secondary-selection`: This face is used for displaying a secondary X selection (see section [P.2 Secondary Selection](emacs.html#SEC193)). `bold`: This face uses a bold variant of the default font, if it has one. `italic`: This face uses an italic variant of the default font, if it has one. `bold-italic`: This face uses a bold italic variant of the default font, if it has one. `underline`: This face underlines text. `fixed-pitch`: The basic fixed-pitch face. `fringe`: The face for the fringes to the left and right of windows on graphic displays. (The fringes are the narrow portions of the Emacs frame between the text area and the window's right and left borders.) `scroll-bar`: This face determines the visual appearance of the scroll bar. `border`: This face determines the color of the frame border. `cursor`: This face determines the color of the cursor. `mouse`: This face determines the color of the mouse pointer. `tool-bar`: This is the basic tool-bar face. No text appears in the tool bar, but the colors of this face affect the appearance of tool bar icons. `tooltip`: This face is used for tooltips. `menu`: This face determines the colors and font of Emacs's menus. Setting the font of LessTif/Motif menus is currently not supported; attempts to set the font are ignored in this case. `trailing-whitespace`: The face for highlighting trailing whitespace when `show-trailing-whitespace` is non-nil; see [J.5 Trailing Whitespace](emacs.html#SEC82). `variable-pitch`: The basic variable-pitch face.

When Transient Mark mode is enabled, the text of the region is
highlighted when the mark is active. This uses the face named
`region`; you can control the style of highlighting by changing the
style of this face (see section [AD.2.2.3 Customizing Faces](emacs.html#SEC482)). See section [H.2 Transient Mark Mode](emacs.html#SEC54),
for more information about Transient Mark mode and activation and
deactivation of the mark.

One easy way to use faces is to turn on Font Lock mode. This minor
mode, which is always local to a particular buffer, arranges to
choose faces according to the syntax of the text you are editing. It
can recognize comments and strings in most languages; in several
languages, it can also recognize and properly highlight various other
important constructs. See section [J.2 Font Lock mode](emacs.html#SEC79), for more information about
Font Lock mode and syntactic highlighting.

You can print out the buffer with the highlighting that appears
on your screen using the command `ps-print-buffer-with-faces`.
See section [AC.19 PostScript Hardcopy](emacs.html#SEC459).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC78) ] | [ [>](emacs.html#SEC80) ] | [ [<<](emacs.html#SEC80) ] | [ [Up](emacs.html#SEC77) ] | [ [>>](emacs.html#SEC91) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## J.2 Font Lock mode

Font Lock mode is a minor mode, always local to a particular
buffer, which highlights (or "fontifies") using various faces
according to the syntax of the text you are editing. It can
recognize comments and strings in most languages; in several
languages, it can also recognize and properly highlight various other
important constructs--for example, names of functions being defined
or reserved keywords.

The command `M-x font-lock-mode` turns Font Lock mode on or off
according to the argument, and toggles the mode when it has no argument.
The function `turn-on-font-lock` unconditionally enables Font Lock
mode. This is useful in mode-hook functions. For example, to enable
Font Lock mode whenever you edit a C file, you can do this:

|  |  |
| --- | --- |
|  | ``` (add-hook 'c-mode-hook 'turn-on-font-lock) ``` |

To turn on Font Lock mode automatically in all modes which support
it, customize the user option `global-font-lock-mode` or use the
function `global-font-lock-mode` in your `.emacs' file, like
this:

|  |  |
| --- | --- |
|  | ``` (global-font-lock-mode 1) ``` |

Font Lock mode uses several specifically named faces to do its job,
including `font-lock-string-face`, `font-lock-comment-face`,
and others. The easiest way to find them all is to use completion
on the face name in `set-face-foreground`.

To change the colors or the fonts used by Font Lock mode to fontify
different parts of text, just change these faces. There are
two ways to do it:

* Invoke `M-x set-face-foreground` or `M-x set-face-background`
  to change the colors of a particular face used by Font Lock.
  See section [J.1 Using Multiple Typefaces](emacs.html#SEC78). The command `M-x list-faces-display` displays all
  the faces currently known to Emacs, including those used by Font Lock.

  * Customize the faces interactively with `M-x customize-face`, as
    described in [AD.2.2.3 Customizing Faces](emacs.html#SEC482).

To get the full benefit of Font Lock mode, you need to choose a
default font which has bold, italic, and bold-italic variants; or else
you need to have a color or gray-scale screen.

The variable `font-lock-maximum-decoration` specifies the
preferred level of fontification, for modes that provide multiple
levels. Level 1 is the least amount of fontification; some modes
support levels as high as 3. The normal default is "as high as
possible." You can specify an integer, which applies to all modes, or
you can specify different numbers for particular major modes; for
example, to use level 1 for C/C++ modes, and the default level
otherwise, use this:

|  |  |
| --- | --- |
|  | ``` (setq font-lock-maximum-decoration       '((c-mode . 1) (c++-mode . 1))) ``` |

Fontification can be too slow for large buffers, so you can suppress
it. The variable `font-lock-maximum-size` specifies a buffer size,
beyond which buffer fontification is suppressed.

Comment and string fontification (or "syntactic" fontification)
relies on analysis of the syntactic structure of the buffer text. For
the sake of speed, some modes, including C mode and Lisp mode,
rely on a special convention: an open-parenthesis or open-brace in the
leftmost column always defines the beginning of a defun, and is
thus always outside any string or comment. (See section [U.2.1 Left Margin Convention](emacs.html#SEC271).) If you don't follow this convention, Font Lock mode can
misfontify the text that follows an open-parenthesis or open-brace in
the leftmost column that is inside a string or comment.

The variable `font-lock-beginning-of-syntax-function` (always
buffer-local) specifies how Font Lock mode can find a position
guaranteed to be outside any comment or string. In modes which use the
leftmost column parenthesis convention, the default value of the variable
is `beginning-of-defun`---that tells Font Lock mode to use the
convention. If you set this variable to `nil`, Font Lock no longer
relies on the convention. This avoids incorrect results, but the price
is that, in some cases, fontification for a changed text must rescan
buffer text from the beginning of the buffer. This can considerably
slow down redisplay while scrolling, particularly if you are close to
the end of a large buffer.

Font Lock highlighting patterns already exist for many modes, but you
may want to fontify additional patterns. You can use the function
`font-lock-add-keywords`, to add your own highlighting patterns for
a particular mode. For example, to highlight `` `FIXME:' `` words in C
comments, use this:

|  |  |
| --- | --- |
|  | ``` (font-lock-add-keywords  'c-mode  '(("\\<\\(FIXME\\):" 1 font-lock-warning-face t))) ``` |

To remove keywords from the font-lock highlighting patterns, use the
function `font-lock-remove-keywords`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC79) ] | [ [>](emacs.html#SEC81) ] | [ [<<](emacs.html#SEC81) ] | [ [Up](emacs.html#SEC77) ] | [ [>>](emacs.html#SEC91) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## J.3 Highlight Changes Mode

Use `M-x highlight-changes-mode` to enable a minor mode
that uses faces (colors, typically) to indicate which parts of
the buffer were changed most recently.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC80) ] | [ [>](emacs.html#SEC82) ] | [ [<<](emacs.html#SEC82) ] | [ [Up](emacs.html#SEC77) ] | [ [>>](emacs.html#SEC91) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## J.4 Interactive Highlighting by Matching

It is sometimes useful to highlight the strings that match a certain
regular expression. For example, you might wish to see all the
references to a certain variable in a program source file, or highlight
certain parts in a voluminous output of some program, or make certain
cliches stand out in an article.

Use the `M-x hi-lock-mode` command to turn on a minor mode that
allows you to specify regular expressions of the text to be
highlighted. Hi-lock mode works like Font Lock (see section [J.2 Font Lock mode](emacs.html#SEC79)),
except that it lets you specify explicitly what parts of text to
highlight. You control Hi-lock mode with these commands:

`C-x w h regexp RET face RET`: Highlight text that matches regexp using face face (`highlight-regexp`). By using this command more than once, you can highlight various parts of the text in different ways. `C-x w r regexp RET`: Unhighlight regexp (`unhighlight-regexp`). You must enter one of the regular expressions currently specified for highlighting. (You can use completion, or choose from a menu, to enter one of them conveniently.) `C-x w l regexp RET face RET`: Highlight entire lines containing a match for regexp, using face face (`highlight-lines-matching-regexp`). `C-x w b`: Insert all the current highlighting regexp/face pairs into the buffer at point, with comment delimiters to prevent them from changing your program. This key binding runs the `hi-lock-write-interactive-patterns` command. These patterns will be read the next time you visit the file while Hi-lock mode is enabled, or whenever you use the `M-x hi-lock-find-patterns` command. `C-x w i`: Re-read regexp/face pairs in the current buffer (`hi-lock-write-interactive-patterns`). The list of pairs is found no matter where in the buffer it may be. This command does nothing if the major mode is a member of the list `hi-lock-exclude-modes`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC81) ] | [ [>](emacs.html#SEC83) ] | [ [<<](emacs.html#SEC83) ] | [ [Up](emacs.html#SEC77) ] | [ [>>](emacs.html#SEC91) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## J.5 Trailing Whitespace

It is easy to leave unnecessary spaces at the end of a line without
realizing it. In most cases, this *trailing whitespace* has no
effect, but there are special circumstances where it matters.

You can make trailing whitespace visible on the screen by setting the
buffer-local variable `show-trailing-whitespace` to `t`. Then
Emacs displays trailing whitespace in the face
`trailing-whitespace`.

Trailing whitespace is defined as spaces or tabs at the end of a
line. But trailing whitespace is not displayed specially if point is
at the end of the line containing the whitespace. (Doing that looks
ugly while you are typing in new text, and the location of point is
enough in that case to show you that the spaces are present.)

To delete all trailing whitespace within the current buffer's
restriction (see section [AC.22 Narrowing](emacs.html#SEC462)), type `M-x
delete-trailing-whitespace RET`. (This command does not remove
the form-feed characters.)

Emacs can indicate empty lines at the end of the buffer with a
special bitmap on the left fringe of the window. To enable this
feature, set the buffer-local variable `indicate-empty-lines` to
a non-`nil` value. The default value of this variable is
controlled by the variable `default-indicate-empty-lines`;
by setting that variable, you can enable or disable this feature
for all new buffers.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC82) ] | [ [>](emacs.html#SEC84) ] | [ [<<](emacs.html#SEC84) ] | [ [Up](emacs.html#SEC77) ] | [ [>>](emacs.html#SEC91) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## J.6 Scrolling

If a buffer contains text that is too large to fit entirely within a
window that is displaying the buffer, Emacs shows a contiguous portion of
the text. The portion shown always contains point.

*Scrolling* means moving text up or down in the window so that
different parts of the text are visible. Scrolling forward means that text
moves up, and new text appears at the bottom. Scrolling backward moves
text down and new text appears at the top.

Scrolling happens automatically if you move point past the bottom or top
of the window. You can also explicitly request scrolling with the commands
in this section.

`C-l`: Clear screen and redisplay, scrolling the selected window to center point vertically within it (`recenter`). `C-v`: Scroll forward (a windowful or a specified number of lines) (`scroll-up`). `NEXT`: `PAGEDOWN`: Likewise, scroll forward. `M-v`: Scroll backward (`scroll-down`). `PRIOR`: `PAGEUP`: Likewise, scroll backward. `arg C-l`: Scroll so point is on line arg (`recenter`). `C-M-l`: Scroll heuristically to bring useful information onto the screen (`reposition-window`).

The most basic scrolling command is `C-l` (`recenter`) with
no argument. It clears the entire screen and redisplays all windows.
In addition, it scrolls the selected window so that point is halfway
down from the top of the window.

To read the buffer a windowful at a time, use `C-v`
(`scroll-up`) with no argument. This scrolls forward by nearly
the whole window height. The effect is to take the two lines at the
bottom of the window and put them at the top, followed by nearly a
whole windowful of lines that were not previously visible. If point
was in the text that scrolled off the top, it ends up at the new top
of the window.

`M-v` (`scroll-down`) with no argument scrolls backward in
a similar way, also with overlap. The number of lines of overlap
across a `C-v` or `M-v` is controlled by the variable
`next-screen-context-lines`; by default, it is 2. The function
keys `NEXT` and `PRIOR`, or `PAGEDOWN` and `PAGEUP`,
are equivalent to `C-v` and `M-v`.

The commands `C-v` and `M-v` with a numeric argument scroll
the text in the selected window up or down a few lines. `C-v`
with an argument moves the text and point up, together, that many
lines; it brings the same number of new lines into view at the bottom
of the window. `M-v` with numeric argument scrolls the text
downward, bringing that many new lines into view at the top of the
window. `C-v` with a negative argument is like `M-v` and vice
versa.

The names of scroll commands are based on the direction that the
text moves in the window. Thus, the command to scroll forward is
called `scroll-up` because it moves the text upward on the
screen. The keys `PAGEDOWN` and `PAGEUP` derive their names
and customary meanings from a different convention that developed
elsewhere; hence the strange result that `PAGEDOWN` runs
`scroll-up`.

Some users like the full-screen scroll commands to keep point at the
same screen line. To enable this behavior, set the variable
`scroll-preserve-screen-position` to a non-`nil` value. This
mode is convenient for browsing through a file by scrolling by
screenfuls; if you come back to the screen where you started, point goes
back to the line where it started. However, this mode is inconvenient
when you move to the next screen in order to move point to the text
there.

Another way to do scrolling is with `C-l` with a numeric argument.
`C-l` does not clear the screen when given an argument; it only scrolls
the selected window. With a positive argument n, it repositions text
to put point n lines down from the top. An argument of zero puts
point on the very top line. Point does not move with respect to the text;
rather, the text and point move rigidly on the screen. `C-l` with a
negative argument puts point that many lines from the bottom of the window.
For example, `C-u - 1 C-l` puts point on the bottom line, and `C-u
- 5 C-l` puts it five lines from the bottom. `C-u C-l` scrolls to put
point at the center (vertically) of the selected window.

The `C-M-l` command (`reposition-window`) scrolls the current
window heuristically in a way designed to get useful information onto
the screen. For example, in a Lisp file, this command tries to get the
entire current defun onto the screen if possible.

Scrolling happens automatically when point moves out of the visible
portion of the text. Normally, automatic scrolling centers point
vertically within the window. However, if you set
`scroll-conservatively` to a small number n, then if you
move point just a little off the screen--less than n
lines--then Emacs scrolls the text just far enough to bring point
back on screen. By default, `scroll-conservatively` is 0.

When the window does scroll by a longer distance, you can control
how aggressively it scrolls, by setting the variables
`scroll-up-aggressively` and `scroll-down-aggressively`.
The value of `scroll-up-aggressively` should be either
`nil`, or a fraction f between 0 and 1. A fraction
specifies where on the screen to put point when scrolling upward.
More precisely, when a window scrolls up because point is above the
window start, the new start position is chosen to put point f
part of the window height from the top. The larger f, the more
aggressive the scrolling.

`nil`, which is the default, scrolls to put point at the center.
So it is equivalent to .5.

Likewise, `scroll-down-aggressively` is used for scrolling
down. The value, f, specifies how far point should be placed
from the bottom of the window; thus, as with
`scroll-up-aggressively`, a larger value is more aggressive.

The variable `scroll-margin` restricts how close point can come
to the top or bottom of a window. Its value is a number of screen
lines; if point comes within that many lines of the top or bottom of the
window, Emacs recenters the window. By default, `scroll-margin` is
0.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC83) ] | [ [>](emacs.html#SEC85) ] | [ [<<](emacs.html#SEC85) ] | [ [Up](emacs.html#SEC77) ] | [ [>>](emacs.html#SEC91) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## J.7 Horizontal Scrolling

*Horizontal scrolling* means shifting all the lines sideways
within a window--so that some of the text near the left margin is not
displayed at all. Emacs does this automatically in any window that
uses line truncation rather than continuation: whenever point moves
off the left or right edge of the screen, Emacs scrolls the buffer
horizontally to make point visible.

When a window has been scrolled horizontally, text lines are truncated
rather than continued (see section [D.8 Continuation Lines](emacs.html#SEC27)), with a `` `$' ``
appearing in the first column when there is text truncated to the left,
and in the last column when there is text truncated to the right.

You can use these commands to do explicit horizontal scrolling.

`C-x <`: Scroll text in current window to the left (`scroll-left`). `C-x >`: Scroll to the right (`scroll-right`).

The command `C-x <` (`scroll-left`) scrolls the selected
window to the left by n columns with argument n. This moves
part of the beginning of each line off the left edge of the window.
With no argument, it scrolls by almost the full width of the window (two
columns less, to be precise).

`C-x >` (`scroll-right`) scrolls similarly to the right. The
window cannot be scrolled any farther to the right once it is displayed
normally (with each line starting at the window's left margin);
attempting to do so has no effect. This means that you don't have to
calculate the argument precisely for `C-x >`; any sufficiently large
argument will restore the normal display.

If you scroll a window horizontally by hand, that sets a lower bound
for automatic horizontal scrolling. Automatic scrolling will continue
to scroll the window, but never farther to the right than the amount
you previously set by `scroll-left`.

To disable automatic horizontal scrolling, set the variable
`automatic-hscrolling` to `nil`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC84) ] | [ [>](emacs.html#SEC86) ] | [ [<<](emacs.html#SEC86) ] | [ [Up](emacs.html#SEC77) ] | [ [>>](emacs.html#SEC91) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## J.8 Follow Mode

*Follow mode* is a minor mode that makes two windows showing the
same buffer scroll as one tall "virtual window." To use Follow mode,
go to a frame with just one window, split it into two side-by-side
windows using `C-x 3`, and then type `M-x follow-mode`. From
then on, you can edit the buffer in either of the two windows, or scroll
either one; the other window follows it.

In Follow mode, if you move point outside the portion visible in one
window and into the portion visible in the other window, that selects
the other window--again, treating the two as if they were parts of
one large window.

To turn off Follow mode, type `M-x follow-mode` a second time.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC85) ] | [ [>](emacs.html#SEC87) ] | [ [<<](emacs.html#SEC77) ] | [ [Up](emacs.html#SEC77) ] | [ [>>](emacs.html#SEC91) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## J.9 Selective Display

Emacs has the ability to hide lines indented more than a certain number
of columns (you specify how many columns). You can use this to get an
overview of a part of a program.

To hide lines, type `C-x $` (`set-selective-display`) with a
numeric argument n. Then lines with at least n columns of
indentation disappear from the screen. The only indication of their
presence is that three dots (`` `...' ``) appear at the end of each
visible line that is followed by one or more hidden ones.

The commands `C-n` and `C-p` move across the hidden lines as
if they were not there.

The hidden lines are still present in the buffer, and most editing
commands see them as usual, so you may find point in the middle of the
hidden text. When this happens, the cursor appears at the end of the
previous line, after the three dots. If point is at the end of the
visible line, before the newline that ends it, the cursor appears before
the three dots.

To make all lines visible again, type `C-x $` with no argument.

If you set the variable `selective-display-ellipses` to
`nil`, the three dots do not appear at the end of a line that
precedes hidden lines. Then there is no visible indication of the
hidden lines. This variable becomes local automatically when set.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC86) ] | [ [>](emacs.html#SEC88) ] | [ [<<](emacs.html#SEC79) ] | [ [Up](emacs.html#SEC77) ] | [ [>>](emacs.html#SEC91) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## J.10 Optional Mode Line Features

The current line number of point appears in the mode line when Line
Number mode is enabled. Use the command `M-x line-number-mode` to
turn this mode on and off; normally it is on. The line number appears
before the buffer percentage pos, with the letter `` `L' `` to
indicate what it is. See section [AD.1 Minor Modes](emacs.html#SEC476), for more information about
minor modes and about how to use this command.

If you have narrowed the buffer (see section [AC.22 Narrowing](emacs.html#SEC462)), the displayed
line number is relative to the accessible portion of the buffer.

If the buffer is very large (larger than the value of
`line-number-display-limit`), then the line number doesn't appear.
Emacs doesn't compute the line number when the buffer is large, because
that would be too slow. Set it to `nil` to remove the limit.

Line-number computation can also be slow if the lines in the buffer
are too long. For this reason, Emacs normally doesn't display line
numbers if the average width, in characters, of lines near point is
larger than the value of the variable
`line-number-display-limit-width`. The default value is 200
characters.

You can also display the current column number by turning on Column
Number mode. It displays the current column number preceded by the
letter `` `C' ``. Type `M-x column-number-mode` to toggle this mode.

Emacs can optionally display the time and system load in all mode
lines. To enable this feature, type `M-x display-time` or customize
the option `display-time-mode`. The information added to the mode
line usually appears after the buffer name, before the mode names and
their parentheses. It looks like this:

|  |  |
| --- | --- |
|  | ``` hh:mmpm l.ll ``` |

Here hh and mm are the hour and minute, followed always by
`` `am' `` or `` `pm' ``. l.ll is the average number of running
processes in the whole system recently. (Some fields may be missing if
your operating system cannot support them.) If you prefer time display
in 24-hour format, set the variable `display-time-24hr-format`
to `t`.

The word `` `Mail' `` appears after the load level if there is mail
for you that you have not read yet. On a graphical display you can use
an icon instead of `` `Mail' `` by customizing
`display-time-use-mail-icon`; this may save some space on the mode
line. You can customize `display-time-mail-face` to make the mail
indicator prominent.

By default, the mode line is drawn on graphics displays with
3D-style highlighting, like that of a button when it is not being
pressed. If you don't like this effect, you can disable the 3D
highlighting of the mode line, by customizing the attributes of the
`mode-line` face in your `.emacs' init file, like this:

|  |  |
| --- | --- |
|  | ``` (set-face-attribute 'mode-line nil :box nil) ``` |

Alternatively, you can turn off the box attribute in your
`.Xdefaults' file:

|  |  |
| --- | --- |
|  | ``` Emacs.mode-line.AttributeBox: off ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC87) ] | [ [>](emacs.html#SEC89) ] | [ [<<](emacs.html#SEC79) ] | [ [Up](emacs.html#SEC77) ] | [ [>>](emacs.html#SEC91) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## J.11 How Text Is Displayed

ASCII printing characters (octal codes 040 through 0176) in Emacs
buffers are displayed with their graphics, as are non-ASCII multibyte
printing characters (octal codes above 0400).

Some ASCII control characters are displayed in special ways. The
newline character (octal code 012) is displayed by starting a new line.
The tab character (octal code 011) is displayed by moving to the next
tab stop column (normally every 8 columns).

Other ASCII control characters are normally displayed as a caret
(`` `^' ``) followed by the non-control version of the character; thus,
control-A is displayed as `` `^A' ``.

Non-ASCII characters 0200 through 0237 (octal) are displayed with
octal escape sequences; thus, character code 0230 (octal) is displayed
as `` `\230' ``. The display of character codes 0240 through 0377
(octal) may be either as escape sequences or as graphics. They do not
normally occur in multibyte buffers, but if they do, they are displayed
as Latin-1 graphics. In unibyte mode, if you enable European display
they are displayed using their graphics (assuming your terminal supports
them), otherwise as escape sequences. See section [Q.13 Single-byte Character Set Support](emacs.html#SEC226).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC88) ] | [ [>](emacs.html#SEC90) ] | [ [<<](emacs.html#SEC79) ] | [ [Up](emacs.html#SEC77) ] | [ [>>](emacs.html#SEC91) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## J.12 Customization of Display

This section contains information for customization only. Beginning
users should skip it.

The variable `mode-line-inverse-video` is an obsolete way of
controlling whether the mode line is displayed in inverse video; the
preferred way of doing this is to change the `mode-line` face.
See section [B.3 The Mode Line](emacs.html#SEC11). However, if `mode-line-inverse-video` has a
value of `nil`, then the `mode-line` face will be ignored,
and mode-lines will be drawn using the default text face.
See section [J.1 Using Multiple Typefaces](emacs.html#SEC78).

If the variable `inverse-video` is non-`nil`, Emacs attempts
to invert all the lines of the display from what they normally are.

If the variable `visible-bell` is non-`nil`, Emacs attempts
to make the whole screen blink when it would normally make an audible bell
sound. This variable has no effect if your terminal does not have a way
to make the screen blink.

When you reenter Emacs after suspending, Emacs normally clears the
screen and redraws the entire display. On some terminals with more than
one page of memory, it is possible to arrange the termcap entry so that
the `` `ti' `` and `` `te' `` strings (output to the terminal when Emacs
is entered and exited, respectively) switch between pages of memory so
as to use one page for Emacs and another page for other output. Then
you might want to set the variable `no-redraw-on-reenter`
non-`nil`; this tells Emacs to assume, when resumed, that the
screen page it is using still contains what Emacs last wrote there.

The variable `echo-keystrokes` controls the echoing of multi-character
keys; its value is the number of seconds of pause required to cause echoing
to start, or zero meaning don't echo at all. See section [B.2 The Echo Area](emacs.html#SEC10).

If the variable `ctl-arrow` is `nil`, all control characters in
the buffer are displayed with octal escape sequences, except for newline
and tab. Altering the value of `ctl-arrow` makes it local to the
current buffer; until that time, the default value is in effect. The
default is initially `t`. See section `Display Tables' in The Emacs Lisp Reference Manual.

Normally, a tab character in the buffer is displayed as whitespace which
extends to the next display tab stop position, and display tab stops come
at intervals equal to eight spaces. The number of spaces per tab is
controlled by the variable `tab-width`, which is made local by
changing it, just like `ctl-arrow`. Note that how the tab character
in the buffer is displayed has nothing to do with the definition of
`TAB` as a command. The variable `tab-width` must have an
integer value between 1 and 1000, inclusive.

If the variable `truncate-lines` is non-`nil`, then each
line of text gets just one screen line for display; if the text line is
too long, display shows only the part that fits. If
`truncate-lines` is `nil`, then long text lines display as
more than one screen line, enough to show the whole text of the line.
See section [D.8 Continuation Lines](emacs.html#SEC27). Altering the value of `truncate-lines`
makes it local to the current buffer; until that time, the default value
is in effect. The default is initially `nil`.

If the variable `truncate-partial-width-windows` is
non-`nil`, it forces truncation rather than continuation in any
window less than the full width of the screen or frame, regardless of
the value of `truncate-lines`. For information about side-by-side
windows, see [O.2 Splitting Windows](emacs.html#SEC185). See also section `Display' in The Emacs Lisp Reference Manual.

The variable `baud-rate` holds the output speed of the
terminal, as far as Emacs knows. Setting this variable does not
change the speed of actual data transmission, but the value is used
for calculations. On terminals, it affects padding, and decisions
about whether to scroll part of the screen or redraw it instead.
It also affects the behavior of incremental search.

On window-systems, `baud-rate` is only used to determine how
frequently to look for pending input during display updating. A
higher value of `baud-rate` means that check for pending input
will be done less frequently.

You can customize the way any particular character code is displayed
by means of a display table. See section `Display Tables' in The Emacs Lisp Reference Manual.

On a window system, Emacs can optionally display the mouse pointer
in a special shape to say that Emacs is busy. To turn this feature on
or off, customize the group `cursor`. You can also control the
amount of time Emacs must remain busy before the busy indicator is
displayed, by setting the variable `hourglass-delay`.

On some text-only terminals, bold face and inverse video together
result in text that is hard to read. Call the function
`tty-suppress-bold-inverse-default-colors` with a non-`nil`
argument to suppress the effect of bold-face in this case.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC89) ] | [ [>](emacs.html#SEC91) ] | [ [<<](emacs.html#SEC79) ] | [ [Up](emacs.html#SEC77) ] | [ [>>](emacs.html#SEC91) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## J.13 Displaying the Cursor

There are a number of ways to customize the display of the cursor.
`M-x hl-line-mode` enables or disables a minor mode which
highlights the line containing point. On window systems, the command
`M-x blink-cursor-mode` turns on or off the blinking of the
cursor. (On terminals, the terminal itself blinks the cursor, and
Emacs has no control over it.)

You can customize the cursor's color, and whether it blinks, using
the `cursor` Custom group (see section [AD.2.2 Easy Customization Interface](emacs.html#SEC479)).

When displaying on a window system, Emacs can optionally draw the
block cursor as wide as the character under the cursor--for example,
if the cursor is on a tab character, it would cover the full width
occupied by that tab character. To enable this feature, set the
variable `x-stretch-cursor` to a non-`nil` value.

Normally, the cursor in non-selected windows is shown as a hollow box.
To turn off cursor display in non-selected windows, customize the option
`cursor-in-non-selected-windows` to assign it a `nil` value.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC90) ] | [ [>](emacs.html#SEC92) ] | [ [<<](emacs.html#SEC77) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC105) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# K. Searching and Replacement

Like other editors, Emacs has commands for searching for occurrences of
a string. The principal search command is unusual in that it is
*incremental*; it begins to search before you have finished typing the
search string. There are also nonincremental search commands more like
those of other editors.

Besides the usual `replace-string` command that finds all
occurrences of one string and replaces them with another, Emacs has a
more flexible replacement command called `query-replace`, which
asks interactively which occurrences to replace.

> |  |  |  |
> | --- | --- | --- |
> | [K.1 Incremental Search](emacs.html#SEC92) |  | Search happens as you type the string. |
> | [K.2 Nonincremental Search](emacs.html#SEC94) |  | Specify entire string and then search. |
> | [K.3 Word Search](emacs.html#SEC95) |  | Search for sequence of words. |
> | [K.4 Regular Expression Search](emacs.html#SEC96) |  | Search for match for a regexp. |
> | [K.5 Syntax of Regular Expressions](emacs.html#SEC97) |  | Syntax of regular expressions. |
> | [K.6 Searching and Case](emacs.html#SEC98) |  | To ignore case while searching, or not. |
> | [K.7 Replacement Commands](emacs.html#SEC99) |  | Search, and replace some or all matches. |
> | [K.8 Other Search-and-Loop Commands](emacs.html#SEC104) |  | Operating on all matches for some regexp. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC91) ] | [ [>](emacs.html#SEC93) ] | [ [<<](emacs.html#SEC91) ] | [ [Up](emacs.html#SEC91) ] | [ [>>](emacs.html#SEC94) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## K.1 Incremental Search

An incremental search begins searching as soon as you type the first
character of the search string. As you type in the search string, Emacs
shows you where the string (as you have typed it so far) would be
found. When you have typed enough characters to identify the place you
want, you can stop. Depending on what you plan to do next, you may or
may not need to terminate the search explicitly with `RET`.

`C-s`: Incremental search forward (`isearch-forward`). `C-r`: Incremental search backward (`isearch-backward`).

`C-s` starts a forward incremental search. It reads characters
from the keyboard, and moves point past the next occurrence of those
characters. If you type `C-s` and then `F`, that puts the
cursor after the first `` `F' `` (the first following the starting point, since
this is a forward search). Then if you type an `O`, you will see
the cursor move just after the first `` `FO' `` (the `` `F' `` in that
`` `FO' `` may or may not be the first `` `F' ``). After another
`O`, the cursor moves after the first `` `FOO' `` after the place
where you started the search. At each step, the buffer text that
matches the search string is highlighted, if the terminal can do that;
the current search string is always displayed in the echo area.

If you make a mistake in typing the search string, you can cancel
characters with `DEL`. Each `DEL` cancels the last character of
search string. This does not happen until Emacs is ready to read another
input character; first it must either find, or fail to find, the character
you want to erase. If you do not want to wait for this to happen, use
`C-g` as described below.

When you are satisfied with the place you have reached, you can type
`RET`, which stops searching, leaving the cursor where the search
brought it. Also, any command not specially meaningful in searches
stops the searching and is then executed. Thus, typing `C-a`
would exit the search and then move to the beginning of the line.
`RET` is necessary only if the next command you want to type is a
printing character, `DEL`, `RET`, or another character that is
special within searches (`C-q`, `C-w`, `C-r`, `C-s`,
`C-y`, `M-y`, `M-r`, `M-s`, and some other
meta-characters).

Sometimes you search for `` `FOO' `` and find one, but not the one you
expected to find. There was a second `` `FOO' `` that you forgot
about, before the one you were aiming for. In this event, type
another `C-s` to move to the next occurrence of the search string.
You can repeat this any number of times. If you overshoot, you can
cancel some `C-s` characters with `DEL`.

After you exit a search, you can search for the same string again by
typing just `C-s C-s`: the first `C-s` is the key that invokes
incremental search, and the second `C-s` means "search again."

To reuse earlier search strings, use the *search ring*. The
commands `M-p` and `M-n` move through the ring to pick a search
string to reuse. These commands leave the selected search ring element
in the minibuffer, where you can edit it. Type `C-s` or `C-r`
to terminate editing the string and search for it.

If your string is not found at all, the echo area says `` `Failing
I-Search' ``. The cursor is after the place where Emacs found as much of your
string as it could. Thus, if you search for `` `FOOT' ``, and there is no
`` `FOOT' ``, you might see the cursor after the `` `FOO' `` in `` `FOOL' ``.
At this point there are several things you can do. If your string was
mistyped, you can rub some of it out and correct it. If you like the place
you have found, you can type `RET` or some other Emacs command to
remain there. Or you can type `C-g`, which
removes from the search string the characters that could not be found (the
`` `T' `` in `` `FOOT' ``), leaving those that were found (the `` `FOO' `` in
`` `FOOT' ``). A second `C-g` at that point cancels the search
entirely, returning point to where it was when the search started.

An upper-case letter in the search string makes the search
case-sensitive. If you delete the upper-case character from the search
string, it ceases to have this effect. See section [K.6 Searching and Case](emacs.html#SEC98).

To search for a newline, type `C-j`. To search for another
control character, such as control-S or carriage return, you must quote
it by typing `C-q` first. This function of `C-q` is analogous
to its use for insertion (see section [D.1 Inserting Text](emacs.html#SEC20)): it causes the
following character to be treated the way any "ordinary" character is
treated in the same context. You can also specify a character by its
octal code: enter `C-q` followed by a sequence of octal digits.

To search for non-ASCII characters, you must use an input method
(see section [Q.4 Input Methods](emacs.html#SEC217)). If an input method is enabled in the
current buffer when you start the search, you can use it while you
type the search string also. Emacs indicates that by including the
input method mnemonic in its prompt, like this:

|  |  |
| --- | --- |
|  | ``` I-search [im]: ``` |

where im is the mnemonic of the active input method. You can
toggle (enable or disable) the input method while you type the search
string with `C-\` (`isearch-toggle-input-method`). You can
turn on a certain (non-default) input method with `C-^`
(`isearch-toggle-specified-input-method`), which prompts for the
name of the input method. The input method you enable during
incremental search remains enabled in the current buffer afterwards.

If a search is failing and you ask to repeat it by typing another
`C-s`, it starts again from the beginning of the buffer.
Repeating a failing reverse search with `C-r` starts again from
the end. This is called *wrapping around*, and `` `Wrapped' ``
appears in the search prompt once this has happened. If you keep on
going past the original starting point of the search, it changes to
`` `Overwrapped' ``, which means that you are revisiting matches that
you have already seen.

The `C-g` "quit" character does special things during searches;
just what it does depends on the status of the search. If the search has
found what you specified and is waiting for input, `C-g` cancels the
entire search. The cursor moves back to where you started the search. If
`C-g` is typed when there are characters in the search string that have
not been found--because Emacs is still searching for them, or because it
has failed to find them--then the search string characters which have not
been found are discarded from the search string. With them gone, the
search is now successful and waiting for more input, so a second `C-g`
will cancel the entire search.

You can change to searching backwards with `C-r`. If a search fails
because the place you started was too late in the file, you should do this.
Repeated `C-r` keeps looking for more occurrences backwards. A
`C-s` starts going forwards again. `C-r` in a search can be canceled
with `DEL`.

If you know initially that you want to search backwards, you can use
`C-r` instead of `C-s` to start the search, because `C-r` as
a key runs a command (`isearch-backward`) to search backward. A
backward search finds matches that are entirely before the starting
point, just as a forward search finds matches that begin after it.

The characters `C-y` and `C-w` can be used in incremental
search to grab text from the buffer into the search string. This makes
it convenient to search for another occurrence of text at point.
`C-w` copies the word after point as part of the search string,
advancing point over that word. Another `C-s` to repeat the search
will then search for a string including that word. `C-y` is similar
to `C-w` but copies all the rest of the current line into the search
string. Both `C-y` and `C-w` convert the text they copy to
lower case if the search is currently not case-sensitive; this is so the
search remains case-insensitive.

The character `M-y` copies text from the kill ring into the search
string. It uses the same text that `C-y` as a command would yank.
`Mouse-2` in the echo area does the same.
See section [H.8 Yanking](emacs.html#SEC63).

When you exit the incremental search, it sets the mark to where point
*was*, before the search. That is convenient for moving back
there. In Transient Mark mode, incremental search sets the mark without
activating it, and does so only if the mark is not already active.

When you pause for a little while during incremental search, it
highlights all other possible matches for the search string. This
makes it easier to anticipate where you can get to by typing `C-s`
or `C-r` to repeat the search. The short delay before highlighting
other matches helps indicate which match is the current one.
If you don't like this feature, you can turn it off by setting
`isearch-lazy-highlight` to `nil`.

You can control how this highlighting looks by customizing the faces
`isearch` (used for the current match) and
`isearch-lazy-highlight-face` (for all the other matches).

To customize the special characters that incremental search understands,
alter their bindings in the keymap `isearch-mode-map`. For a list
of bindings, look at the documentation of `isearch-mode` with
`C-h f isearch-mode RET`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC92) ] | [ [>](emacs.html#SEC94) ] | [ [<<](emacs.html#SEC91) ] | [ [Up](emacs.html#SEC91) ] | [ [>>](emacs.html#SEC105) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
