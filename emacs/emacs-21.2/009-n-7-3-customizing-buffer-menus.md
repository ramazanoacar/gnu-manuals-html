### N.7.3 Customizing Buffer Menus

`M-x bs-show`: Make a list of buffers similarly to `M-x list-buffers` but customizable.

`M-x bs-show` pops up a buffer list similar to the one normally
displayed by `C-x C-b` but which you can customize. If you prefer
this to the usual buffer list, you can bind this command to `C-x
C-b`. To customize this buffer list, use the `bs` Custom group
(see section [AD.2.2 Easy Customization Interface](emacs.html#SEC479)).

MSB global minor mode ("MSB" stands for "mouse select buffer")
provides a different and customizable mouse buffer menu which you may
prefer. It replaces the bindings of `mouse-buffer-menu`,
normally on `C-Down-Mouse-1`, and the menu bar buffer menu. You
can customize the menu in the `msb` Custom group.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC182) ] | [ [>](emacs.html#SEC184) ] | [ [<<](emacs.html#SEC172) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC191) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# O. Multiple Windows

Emacs can split a frame into two or many windows. Multiple windows
can display parts of different buffers, or different parts of one
buffer. Multiple frames always imply multiple windows, because each
frame has its own set of windows. Each window belongs to one and only
one frame.

> |  |  |  |
> | --- | --- | --- |
> | [O.1 Concepts of Emacs Windows](emacs.html#SEC184) |  | Introduction to Emacs windows. |
> | [O.2 Splitting Windows](emacs.html#SEC185) |  | New windows are made by splitting existing windows. |
> | [O.3 Using Other Windows](emacs.html#SEC186) |  | Moving to another window or doing something to it. |
> | [O.4 Displaying in Another Window](emacs.html#SEC187) |  | Finding a file or buffer in another window. |
> | [O.5 Forcing Display in the Same Window](emacs.html#SEC188) |  | Forcing certain buffers to appear in the selected window rather than in another window. |
> | [O.6 Deleting and Rearranging Windows](emacs.html#SEC189) |  | Deleting windows and changing their sizes. |
> | [O.7 Window Handling Convenience Features and Customization](emacs.html#SEC190) |  | Convenience functions for window handling. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC183) ] | [ [>](emacs.html#SEC185) ] | [ [<<](emacs.html#SEC183) ] | [ [Up](emacs.html#SEC183) ] | [ [>>](emacs.html#SEC191) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## O.1 Concepts of Emacs Windows

Each Emacs window displays one Emacs buffer at any time. A single
buffer may appear in more than one window; if it does, any changes in
its text are displayed in all the windows where it appears. But the
windows showing the same buffer can show different parts of it, because
each window has its own value of point.

At any time, one of the windows is the *selected window*; the
buffer this window is displaying is the current buffer. The terminal's
cursor shows the location of point in this window. Each other window
has a location of point as well, but since the terminal has only one
cursor there is no way to show where those locations are. When multiple
frames are visible in X, each frame has a cursor which appears in the
frame's selected window. The cursor in the selected frame is solid; the
cursor in other frames is a hollow box.

Commands to move point affect the value of point for the selected Emacs
window only. They do not change the value of point in any other Emacs
window, even one showing the same buffer. The same is true for commands
such as `C-x b` to change the current buffer in the selected window;
they do not affect other windows at all. However, there are other commands
such as `C-x 4 b` that select a different window and switch buffers in
it. Also, all commands that display information in a window, including
(for example) `C-h f` (`describe-function`) and `C-x C-b`
(`list-buffers`), work by switching buffers in a nonselected window
without affecting the selected window.

When multiple windows show the same buffer, they can have different
regions, because they can have different values of point. However,
they all have the same value for the mark, because each buffer has
only one mark position.

Each window has its own mode line, which displays the buffer name,
modification status and major and minor modes of the buffer that is
displayed in the window. See section [B.3 The Mode Line](emacs.html#SEC11), for full details on the mode
line.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC184) ] | [ [>](emacs.html#SEC186) ] | [ [<<](emacs.html#SEC186) ] | [ [Up](emacs.html#SEC183) ] | [ [>>](emacs.html#SEC191) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## O.2 Splitting Windows

`C-x 2`: Split the selected window into two windows, one above the other (`split-window-vertically`). `C-x 3`: Split the selected window into two windows positioned side by side (`split-window-horizontally`). `C-Mouse-2`: In the mode line or scroll bar of a window, split that window.

The command `C-x 2` (`split-window-vertically`) breaks the
selected window into two windows, one above the other. Both windows start
out displaying the same buffer, with the same value of point. By default
the two windows each get half the height of the window that was split; a
numeric argument specifies how many lines to give to the top window.

`C-x 3` (`split-window-horizontally`) breaks the selected
window into two side-by-side windows. A numeric argument specifies how
many columns to give the one on the left. A line of vertical bars
separates the two windows. Windows that are not the full width of the
screen have mode lines, but they are truncated. On terminals where
Emacs does not support highlighting, truncated mode lines sometimes do
not appear in inverse video.

You can split a window horizontally or vertically by clicking
`C-Mouse-2` in the mode line or the scroll bar. (This does not
work in scroll bars implemented by X toolkits.) The line of splitting
goes through the place where you click: if you click on the mode line,
the new scroll bar goes above the spot; if you click in the scroll
bar, the mode line of the split window is side by side with your
click.

When a window is less than the full width, text lines too long to fit are
frequent. Continuing all those lines might be confusing. The variable
`truncate-partial-width-windows` can be set non-`nil` to force
truncation in all windows less than the full width of the screen,
independent of the buffer being displayed and its value for
`truncate-lines`. See section [D.8 Continuation Lines](emacs.html#SEC27).

Horizontal scrolling is often used in side-by-side windows.
See section [J. Controlling the Display](emacs.html#SEC77).

If `split-window-keep-point` is non-`nil`, the default,
both of the windows resulting from `C-x 2` inherit the value of
point from the window that was split. This means that scrolling is
inevitable. If this variable is `nil`, then `C-x 2` tries to
avoid scrolling the text currently visible on the screen, by putting
point in each window at a position already visible in the window. It
also selects whichever window contain the screen line that the cursor
was previously on. Some users prefer the latter mode on slow
terminals.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC185) ] | [ [>](emacs.html#SEC187) ] | [ [<<](emacs.html#SEC187) ] | [ [Up](emacs.html#SEC183) ] | [ [>>](emacs.html#SEC191) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## O.3 Using Other Windows

`C-x o`: Select another window (`other-window`). That is `o`, not zero. `C-M-v`: Scroll the next window (`scroll-other-window`). `M-x compare-windows`: Find next place where the text in the selected window does not match the text in the next window. `Mouse-1`: `Mouse-1`, in a window's mode line, selects that window but does not move point in it (`mouse-select-window`).

To select a different window, click with `Mouse-1` on its mode
line. With the keyboard, you can switch windows by typing `C-x o`
(`other-window`). That is an `o`, for "other," not a zero.
When there are more than two windows, this command moves through all the
windows in a cyclic order, generally top to bottom and left to right.
After the rightmost and bottommost window, it goes back to the one at
the upper left corner. A numeric argument means to move several steps
in the cyclic order of windows. A negative argument moves around the
cycle in the opposite order. When the minibuffer is active, the
minibuffer is the last window in the cycle; you can switch from the
minibuffer window to one of the other windows, and later switch back and
finish supplying the minibuffer argument that is requested.
See section [E.2 Editing in the Minibuffer](emacs.html#SEC33).

The usual scrolling commands (see section [J. Controlling the Display](emacs.html#SEC77)) apply to the selected
window only, but there is one command to scroll the next window.
`C-M-v` (`scroll-other-window`) scrolls the window that
`C-x o` would select. It takes arguments, positive and negative,
like `C-v`. (In the minibuffer, `C-M-v` scrolls the window
that contains the minibuffer help display, if any, rather than the
next window in the standard cyclic order.)

The command `M-x compare-windows` lets you compare two files or
buffers visible in two windows, by moving through them to the next
mismatch. See section [M.9 Comparing Files](emacs.html#SEC164), for details.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC186) ] | [ [>](emacs.html#SEC188) ] | [ [<<](emacs.html#SEC188) ] | [ [Up](emacs.html#SEC183) ] | [ [>>](emacs.html#SEC191) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## O.4 Displaying in Another Window

`C-x 4` is a prefix key for commands that select another window
(splitting the window if there is only one) and select a buffer in that
window. Different `C-x 4` commands have different ways of finding the
buffer to select.

`C-x 4 b bufname RET`: Select buffer bufname in another window. This runs `switch-to-buffer-other-window`. `C-x 4 C-o bufname RET`: Display buffer bufname in another window, but don't select that buffer or that window. This runs `display-buffer`. `C-x 4 f filename RET`: Visit file filename and select its buffer in another window. This runs `find-file-other-window`. See section [M.2 Visiting Files](emacs.html#SEC112). `C-x 4 d directory RET`: Select a Dired buffer for directory directory in another window. This runs `dired-other-window`. See section [AB. Dired, the Directory Editor](emacs.html#SEC396). `C-x 4 m`: Start composing a mail message in another window. This runs `mail-other-window`; its same-window analogue is `C-x m` (see section [Z. Sending Mail](emacs.html#SEC363)). `C-x 4 .`: Find a tag in the current tags table, in another window. This runs `find-tag-other-window`, the multiple-window variant of `M-.` (see section [W.2 Tags Tables](emacs.html#SEC334)). `C-x 4 r filename RET`: Visit file filename read-only, and select its buffer in another window. This runs `find-file-read-only-other-window`. See section [M.2 Visiting Files](emacs.html#SEC112).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC187) ] | [ [>](emacs.html#SEC189) ] | [ [<<](emacs.html#SEC189) ] | [ [Up](emacs.html#SEC183) ] | [ [>>](emacs.html#SEC191) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## O.5 Forcing Display in the Same Window

Certain Emacs commands switch to a specific buffer with special
contents. For example, `M-x shell` switches to a buffer named
`` `*Shell*' ``. By convention, all these commands are written to pop up
the buffer in a separate window. But you can specify that certain of
these buffers should appear in the selected window.

If you add a buffer name to the list `same-window-buffer-names`,
the effect is that such commands display that particular buffer by
switching to it in the selected window. For example, if you add the
element `"*grep*"` to the list, the `grep` command will
display its output buffer in the selected window.

The default value of `same-window-buffer-names` is not
`nil`: it specifies buffer names `` `*info*' ``, `` `*mail*' `` and
`` `*shell*' `` (as well as others used by more obscure Emacs packages).
This is why `M-x shell` normally switches to the `` `*shell*' ``
buffer in the selected window. If you delete this element from the
value of `same-window-buffer-names`, the behavior of `M-x
shell` will change--it will pop up the buffer in another window
instead.

You can specify these buffers more generally with the variable
`same-window-regexps`. Set it to a list of regular expressions;
then any buffer whose name matches one of those regular expressions is
displayed by switching to it in the selected window. (Once again, this
applies only to buffers that normally get displayed for you in a
separate window.) The default value of this variable specifies Telnet
and rlogin buffers.

An analogous feature lets you specify buffers which should be
displayed in their own individual frames. See section [P.11 Special Buffer Frames](emacs.html#SEC202).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC188) ] | [ [>](emacs.html#SEC190) ] | [ [<<](emacs.html#SEC190) ] | [ [Up](emacs.html#SEC183) ] | [ [>>](emacs.html#SEC191) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## O.6 Deleting and Rearranging Windows

`C-x 0`: Delete the selected window (`delete-window`). The last character in this key sequence is a zero. `C-x 1`: Delete all windows in the selected frame except the selected window (`delete-other-windows`). `C-x 4 0`: Delete the selected window and kill the buffer that was showing in it (`kill-buffer-and-window`). The last character in this key sequence is a zero. `C-x ^`: Make selected window taller (`enlarge-window`). `C-x }`: Make selected window wider (`enlarge-window-horizontally`). `C-x {`: Make selected window narrower (`shrink-window-horizontally`). `C-x -`: Shrink this window if its buffer doesn't need so many lines (`shrink-window-if-larger-than-buffer`). `C-x +`: Make all windows the same height (`balance-windows`). `Drag-Mouse-1`: Dragging a window's mode line up or down with `Mouse-1` changes window heights. `Mouse-2`: `Mouse-2` in a window's mode line deletes all other windows in the frame (`mouse-delete-other-windows`). `Mouse-3`: `Mouse-3` in a window's mode line deletes that window (`mouse-delete-window`), unless the frame has only one window, in which case it buries the current buffer instead and switches to another buffer.

To delete a window, type `C-x 0` (`delete-window`). (That is
a zero.) The space occupied by the deleted window is given to an
adjacent window (but not the minibuffer window, even if that is active
at the time). Once a window is deleted, its attributes are forgotten;
only restoring a window configuration can bring it back. Deleting the
window has no effect on the buffer it used to display; the buffer
continues to exist, and you can select it in any window with `C-x
b`.

`C-x 4 0` (`kill-buffer-and-window`) is a stronger command
than `C-x 0`; it kills the current buffer and then deletes the
selected window.

`C-x 1` (`delete-other-windows`) is more powerful in a
different way; it deletes all the windows except the selected one (and
the minibuffer); the selected window expands to use the whole frame
except for the echo area.

You can also delete a window by clicking on its mode line with
`Mouse-2`, and delete all the windows in a frame except one window
by clicking on that window's mode line with `Mouse-3`.

The easiest way to adjust window heights is with a mouse. If you
press `Mouse-1` on a mode line, you can drag that mode line up or
down, changing the heights of the windows above and below it.

To readjust the division of space among vertically adjacent windows,
use `C-x ^` (`enlarge-window`). It makes the currently
selected window get one line bigger, or as many lines as is specified
with a numeric argument. With a negative argument, it makes the
selected window smaller. `C-x }`
(`enlarge-window-horizontally`) makes the selected window wider by
the specified number of columns. `C-x {`
(`shrink-window-horizontally`) makes the selected window narrower
by the specified number of columns.

When you make a window bigger, the space comes from one of its
neighbors. If this makes any window too small, it is deleted and its
space is given to an adjacent window. The minimum size is specified by
the variables `window-min-height` and `window-min-width`.

The command `C-x -` (`shrink-window-if-larger-than-buffer`)
reduces the height of the selected window, if it is taller than
necessary to show the whole text of the buffer it is displaying. It
gives the extra lines to other windows in the frame.

You can also use `C-x +` (`balance-windows`) to even out the
heights of all the windows in the selected frame.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC189) ] | [ [>](emacs.html#SEC191) ] | [ [<<](emacs.html#SEC183) ] | [ [Up](emacs.html#SEC183) ] | [ [>>](emacs.html#SEC191) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## O.7 Window Handling Convenience Features and Customization

`M-x winner-mode` is a global minor mode that records the
changes in the window configuration (i.e. how the frames are
partitioned into windows), so that you can "undo" them. To undo,
use `C-x left` (`winner-undo`). If you change your mind
while undoing, you can redo the changes you had undone using `C-x
right` (`M-x winner-redo`). Another way to enable Winner mode is
by customizing the variable `winner-mode`.

The Windmove commands move directionally between neighboring windows in
a frame. `M-x windmove-right` selects the window immediately to the
right of the currently selected one, and similarly for the "left," "up,"
and "down" counterparts. `M-x windmove-default-keybindings` binds
these commands to `S-right` etc. (Not all terminals support shifted
arrow keys, however.)

Follow minor mode (`M-x follow-mode`) synchronizes several
windows on the same buffer so that they always display adjacent
sections of that buffer. See section [J.8 Follow Mode](emacs.html#SEC85).

`M-x scroll-all-mode` provides commands to scroll all visible
windows together. You can also turn it on by customizing the variable
`scroll-all-mode`. The commands provided are `M-x
scroll-all-scroll-down-all`, `M-x scroll-all-page-down-all` and
their corresponding "up" equivalents. To make this mode useful,
you should bind these commands to appropriate keys.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC190) ] | [ [>](emacs.html#SEC192) ] | [ [<<](emacs.html#SEC183) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# P. Frames and X Windows

When using the X Window System, you can create multiple windows at the
X level in a single Emacs session. Each X window that belongs to Emacs
displays a *frame* which can contain one or several Emacs windows.
A frame initially contains a single general-purpose Emacs window which
you can subdivide vertically or horizontally into smaller windows. A
frame normally contains its own echo area and minibuffer, but you can
make frames that don't have these--they use the echo area and
minibuffer of another frame.

Editing you do in one frame also affects the other frames. For
instance, if you put text in the kill ring in one frame, you can yank it
in another frame. If you exit Emacs through `C-x C-c` in one frame,
it terminates all the frames. To delete just one frame, use `C-x 5
0` (that is zero, not `o`).

To avoid confusion, we reserve the word "window" for the
subdivisions that Emacs implements, and never use it to refer to a
frame.

Emacs compiled for MS-DOS emulates some aspects of the window system
so that you can use many of the features described in this chapter.
See section [AH.1 Keyboard and Mouse on MS-DOS](emacs.html#SEC555), for more information.

Emacs compiled for MS Windows mostly supports the same features as
under X. However, images, tool bars, and tooltips are not yet
available in Emacs version 21.1 on MS-Windows.

> |  |  |  |
> | --- | --- | --- |
> | [P.1 Mouse Commands for Editing](emacs.html#SEC192) |  | Moving, cutting, and pasting, with the mouse. |
> | [P.2 Secondary Selection](emacs.html#SEC193) |  | Cutting without altering point and mark. |
> | [P.3 Using the Clipboard](emacs.html#SEC194) |  | Using the clipboard for selections. |
> | [P.4 Following References with the Mouse](emacs.html#SEC195) |  | Using the mouse to select an item from a list. |
> | [P.5 Mouse Clicks for Menus](emacs.html#SEC196) |  | Mouse clicks that bring up menus. |
> | [P.6 Mode Line Mouse Commands](emacs.html#SEC197) |  | Mouse clicks on the mode line. |
> | [P.7 Creating Frames](emacs.html#SEC198) |  | Creating additional Emacs frames with various contents. |
> | [P.8 Frame Commands](emacs.html#SEC199) |  | Iconifying, deleting, and switching frames. |
> | [P.9 Making and Using a Speedbar Frame](emacs.html#SEC200) |  | How to make and use a speedbar frame. |
> | [P.10 Multiple Displays](emacs.html#SEC201) |  | How one Emacs job can talk to several displays. |
> | [P.11 Special Buffer Frames](emacs.html#SEC202) |  | You can make certain buffers have their own frames. |
> | [P.12 Setting Frame Parameters](emacs.html#SEC203) |  | Changing the colors and other modes of frames. |
> | [P.13 Scroll Bars](emacs.html#SEC204) |  | How to enable and disable scroll bars; how to use them. |
> | [P.14 Scrolling With "Wheeled" Mice](emacs.html#SEC205) |  | Using mouse wheels for scrolling. |
> | [P.15 Menu Bars](emacs.html#SEC206) |  | Enabling and disabling the menu bar. |
> | [P.16 Tool Bars](emacs.html#SEC207) |  | Enabling and disabling the tool bar. |
> | [P.17 Using Dialog Boxes](emacs.html#SEC208) |  | Controlling use of dialog boxes. |
> | [P.18 Tooltips (or "Balloon Help")](emacs.html#SEC209) |  | Showing "tooltips", AKA "balloon help" for active text. |
> | [P.19 Mouse Avoidance](emacs.html#SEC210) |  | Moving the mouse pointer out of the way. |
> | [P.20 Non-Window Terminals](emacs.html#SEC211) |  | Multiple frames on terminals that show only one. |
> | [P.21 Using a Mouse in Terminal Emulators](emacs.html#SEC212) |  | Using the mouse in an XTerm terminal emulator. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC191) ] | [ [>](emacs.html#SEC193) ] | [ [<<](emacs.html#SEC191) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.1 Mouse Commands for Editing

The mouse commands for selecting and copying a region are mostly
compatible with the `xterm` program. You can use the same mouse
commands for copying between Emacs and other X client programs.

If you select a region with any of these mouse commands, and then
immediately afterward type the `DELETE` function key, it deletes the
region that you selected. The `BACKSPACE` function key and the
ASCII character `DEL` do not do this; if you type any other key
in between the mouse command and `DELETE`, it does not do this.

`Mouse-1`: Move point to where you click (`mouse-set-point`). This is normally the left button. `Drag-Mouse-1`: Set the region to the text you select by dragging, and copy it to the kill ring (`mouse-set-region`). You can specify both ends of the region with this single command. If you move the mouse off the top or bottom of the window while dragging, the window scrolls at a steady rate until you move the mouse back into the window. This way, you can select regions that don't fit entirely on the screen. The number of lines scrolled per step depends on how far away from the window edge the mouse has gone; the variable `mouse-scroll-min-lines` specifies a minimum step size. `Mouse-2`: Yank the last killed text, where you click (`mouse-yank-at-click`). This is normally the middle button. `Mouse-3`: This command, `mouse-save-then-kill`, has several functions depending on where you click and the status of the region. The most basic case is when you click `Mouse-1` in one place and then `Mouse-3` in another. This selects the text between those two positions as the region. It also copies the new region to the kill ring, so that you can copy it to someplace else. If you click `Mouse-1` in the text, scroll with the scroll bar, and then click `Mouse-3`, it remembers where point was before scrolling (where you put it with `Mouse-1`), and uses that position as the other end of the region. This is so that you can select a region that doesn't fit entirely on the screen. More generally, if you do not have a highlighted region, `Mouse-3` selects the text between point and the click position as the region. It does this by setting the mark where point was, and moving point to where you click. If you have a highlighted region, or if the region was set just before by dragging button 1, `Mouse-3` adjusts the nearer end of the region by moving it to where you click. The adjusted region's text also replaces the old region's text in the kill ring. If you originally specified the region using a double or triple `Mouse-1`, so that the region is defined to consist of entire words or lines, then adjusting the region with `Mouse-3` also proceeds by entire words or lines. If you use `Mouse-3` a second time consecutively, at the same place, that kills the region already selected. `Double-Mouse-1`: This key sets the region around the word which you click on. If you click on a character with "symbol" syntax (such as underscore, in C mode), it sets the region around the symbol surrounding that character. If you click on a character with open-parenthesis or close-parenthesis syntax, it sets the region around the parenthetical grouping which that character starts or ends. If you click on a character with string-delimiter syntax (such as a singlequote or doublequote in C), it sets the region around the string constant (using heuristics to figure out whether that character is the beginning or the end of it). `Double-Drag-Mouse-1`: This key selects a region made up of the words you drag across. `Triple-Mouse-1`: This key sets the region around the line you click on. `Triple-Drag-Mouse-1`: This key selects a region made up of the lines you drag across.

The simplest way to kill text with the mouse is to press `Mouse-1`
at one end, then press `Mouse-3` twice at the other end.
See section [H.7 Deletion and Killing](emacs.html#SEC59). To copy the text into the kill ring without deleting it
from the buffer, press `Mouse-3` just once--or just drag across the
text with `Mouse-1`. Then you can copy it elsewhere by yanking it.

To yank the killed or copied text somewhere else, move the mouse there
and press `Mouse-2`. See section [H.8 Yanking](emacs.html#SEC63). However, if
`mouse-yank-at-point` is non-`nil`, `Mouse-2` yanks at
point. Then it does not matter where you click, or even which of the
frame's windows you click on. The default value is `nil`. This
variable also affects yanking the secondary selection.

To copy text to another X window, kill it or save it in the kill ring.
Under X, this also sets the *primary selection*. Then use the
"paste" or "yank" command of the program operating the other window
to insert the text from the selection.

To copy text from another X window, use the "cut" or "copy"
command of the program operating the other window, to select the text
you want. Then yank it in Emacs with `C-y` or `Mouse-2`.

The standard coding system for X selections is `compound-text`.
To specify another coding system for X selections, use `C-x
RET x` or `C-x RET X`. See section [Q.9 Specifying a Coding System](emacs.html#SEC222).

These cutting and pasting commands also work on MS-Windows.

When Emacs puts text into the kill ring, or rotates text to the front
of the kill ring, it sets the *primary selection* in the X server.
This is how other X clients can access the text. Emacs also stores the
text in the cut buffer, but only if the text is short enough
(the value of `x-cut-buffer-max` specifies the maximum number of
characters); putting long strings in the cut buffer can be slow.

The commands to yank the first entry in the kill ring actually check
first for a primary selection in another program; after that, they check
for text in the cut buffer. If neither of those sources provides text
to yank, the kill ring contents are used.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC192) ] | [ [>](emacs.html#SEC194) ] | [ [<<](emacs.html#SEC194) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.2 Secondary Selection

The *secondary selection* is another way of selecting text using
X. It does not use point or the mark, so you can use it to kill text
without setting point or the mark.

`M-Drag-Mouse-1`: Set the secondary selection, with one end at the place where you press down the button, and the other end at the place where you release it (`mouse-set-secondary`). The highlighting appears and changes as you drag. You can control the appearance of the highlighting by customizing the `secondary-selection` face (see section [AD.2.2.3 Customizing Faces](emacs.html#SEC482)). If you move the mouse off the top or bottom of the window while dragging, the window scrolls at a steady rate until you move the mouse back into the window. This way, you can mark regions that don't fit entirely on the screen. `M-Mouse-1`: Set one endpoint for the *secondary selection* (`mouse-start-secondary`). `M-Mouse-3`: Make a secondary selection, using the place specified with `M-Mouse-1` as the other end (`mouse-secondary-save-then-kill`). A second click at the same place kills the secondary selection just made. `M-Mouse-2`: Insert the secondary selection where you click (`mouse-yank-secondary`). This places point at the end of the yanked text.

Double or triple clicking of `M-Mouse-1` operates on words and
lines, much like `Mouse-1`.

If `mouse-yank-at-point` is non-`nil`, `M-Mouse-2`
yanks at point. Then it does not matter precisely where you click; all
that matters is which window you click on. See section [P.1 Mouse Commands for Editing](emacs.html#SEC192).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC193) ] | [ [>](emacs.html#SEC195) ] | [ [<<](emacs.html#SEC195) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.3 Using the Clipboard

As well as the primary and secondary selection types, X supports a
*clipboard* selection type which is used by some applications,
particularly under OpenWindows and Gnome.

The command `M-x menu-bar-enable-clipboard` makes the `Cut`,
`Paste` and `Copy` menu items, as well as the keys of the same
names, all use the clipboard.
You can customize the option `x-select-enable-clipboard` to make
the Emacs yank functions consult the clipboard before the primary
selection, and to make the kill functions to store in the clipboard as
well as the primary selection. Otherwise they do not access the
clipboard at all. Using the clipboard is the default on MS-Windows,
unlike most systems.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC194) ] | [ [>](emacs.html#SEC196) ] | [ [<<](emacs.html#SEC196) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.4 Following References with the Mouse

Some Emacs buffers display lists of various sorts. These include
lists of files, of buffers, of possible completions, of matches for
a pattern, and so on.

Since yanking text into these buffers is not very useful, most of them
define `Mouse-2` specially, as a command to use or view the item you
click on.

For example, if you click `Mouse-2` on a file name in a Dired
buffer, you visit that file. If you click `Mouse-2` on an error
message in the `` `*Compilation*' `` buffer, you go to the source code
for that error message. If you click `Mouse-2` on a completion in
the `` `*Completions*' `` buffer, you choose that completion.

You can usually tell when `Mouse-2` has this special sort of
meaning because the sensitive text highlights when you move the mouse
over it.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC195) ] | [ [>](emacs.html#SEC197) ] | [ [<<](emacs.html#SEC197) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.5 Mouse Clicks for Menus

Mouse clicks modified with the `CTRL` and `SHIFT` keys
bring up menus.

`C-Mouse-1`: This menu is for selecting a buffer. The MSB ("mouse select buffer") global minor mode makes this menu smarter and more customizable. See section [N.7.3 Customizing Buffer Menus](emacs.html#SEC182). `C-Mouse-2`: This menu is for specifying faces and other text properties for editing formatted text. See section [T.11 Editing Formatted Text](emacs.html#SEC258). `C-Mouse-3`: This menu is mode-specific. For most modes if Menu-bar mode is on, this menu has the same items as all the mode-specific menu-bar menus put together. Some modes may specify a different menu for this button.[(2)](emacs.html#FOOT2) If Menu-bar mode is off, this menu contains all the items which would be present in the menu bar--not just the mode-specific ones--so that you can access them without having to display the menu bar. `S-Mouse-1`: This menu is for specifying the frame's principal font.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC196) ] | [ [>](emacs.html#SEC198) ] | [ [<<](emacs.html#SEC198) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.6 Mode Line Mouse Commands

You can use mouse clicks on window mode lines to select and manipulate
windows.

`Mouse-1`: `Mouse-1` on a mode line selects the window above. By dragging `Mouse-1` on the mode line, you can move it, thus changing the height of the windows above and below. `Mouse-2`: `Mouse-2` on a mode line expands that window to fill its frame. `Mouse-3`: `Mouse-3` on a mode line deletes the window above. If the frame has only one window, it buries the current buffer instead and switches to another buffer. `C-Mouse-2`: `C-Mouse-2` on a mode line splits the window above horizontally, above the place in the mode line where you click.

`C-Mouse-2` on a scroll bar splits the corresponding window
vertically, unless you are using an X toolkit's implementation of
scroll bars. See section [O.2 Splitting Windows](emacs.html#SEC185).

The commands above apply to areas of the mode line which do not have
special mouse bindings of their own. Some areas, such as the buffer
name and the major mode name, have their own special mouse bindings.
Emacs displays information about these bindings when you hold the
mouse over such a place (see section [P.18 Tooltips (or "Balloon Help")](emacs.html#SEC209)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC197) ] | [ [>](emacs.html#SEC199) ] | [ [<<](emacs.html#SEC199) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.7 Creating Frames

The prefix key `C-x 5` is analogous to `C-x 4`, with parallel
subcommands. The difference is that `C-x 5` commands create a new
frame rather than just a new window in the selected frame (see section [O.4 Displaying in Another Window](emacs.html#SEC187)). If an existing visible or iconified frame already displays
the requested material, these commands use the existing frame, after
raising or deiconifying as necessary.

The various `C-x 5` commands differ in how they find or create the
buffer to select:

`C-x 5 2`: Create a new frame (`make-frame-command`). `C-x 5 b bufname RET`: Select buffer bufname in another frame. This runs `switch-to-buffer-other-frame`. `C-x 5 f filename RET`: Visit file filename and select its buffer in another frame. This runs `find-file-other-frame`. See section [M.2 Visiting Files](emacs.html#SEC112). `C-x 5 d directory RET`: Select a Dired buffer for directory directory in another frame. This runs `dired-other-frame`. See section [AB. Dired, the Directory Editor](emacs.html#SEC396). `C-x 5 m`: Start composing a mail message in another frame. This runs `mail-other-frame`. It is the other-frame variant of `C-x m`. See section [Z. Sending Mail](emacs.html#SEC363). `C-x 5 .`: Find a tag in the current tag table in another frame. This runs `find-tag-other-frame`, the multiple-frame variant of `M-.`. See section [W.2 Tags Tables](emacs.html#SEC334). `C-x 5 r filename RET`: Visit file filename read-only, and select its buffer in another frame. This runs `find-file-read-only-other-frame`. See section [M.2 Visiting Files](emacs.html#SEC112).

You can control the appearance of new frames you create by setting the
frame parameters in `default-frame-alist`. You can use the
variable `initial-frame-alist` to specify parameters that affect
only the initial frame. See section `Initial Parameters' in The Emacs Lisp Reference Manual, for more information.

The easiest way to specify the principal font for all your Emacs
frames is with an X resource (see section [AE.7 Font Specification Options](emacs.html#SEC537)), but you can also do it by
modifying `default-frame-alist` to specify the `font`
parameter, as shown here:

|  |  |
| --- | --- |
|  | ``` (add-to-list 'default-frame-alist '(font . "10x20")) ``` |

Here's a similar example for specifying a foreground color:

|  |  |
| --- | --- |
|  | ``` (add-to-list 'default-frame-alist '(background-color . "blue")) ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC198) ] | [ [>](emacs.html#SEC200) ] | [ [<<](emacs.html#SEC200) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.8 Frame Commands

The following commands let you create, delete and operate on frames:

`C-z`: Iconify the selected Emacs frame (`iconify-or-deiconify-frame`). The normal meaning of `C-z`, to suspend Emacs, is not useful under a window system, so it has a different binding in that case. If you type this command on an Emacs frame's icon, it deiconifies the frame. `C-x 5 0`: Delete the selected frame (`delete-frame`). This is not allowed if there is only one frame. `C-x 5 o`: Select another frame, raise it, and warp the mouse to it so that it stays selected. If you repeat this command, it cycles through all the frames on your terminal. `C-x 5 1`: Delete all frames except the selected one.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC199) ] | [ [>](emacs.html#SEC201) ] | [ [<<](emacs.html#SEC191) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.9 Making and Using a Speedbar Frame

An Emacs frame can have a *speedbar*, which is a vertical window
that serves as a scrollable menu of files you could visit and tags
within those files. To create a speedbar, type `M-x speedbar`; this
creates a speedbar window for the selected frame. From then on, you can
click on a file name in the speedbar to visit that file in the
corresponding Emacs frame, or click on a tag name to jump to that tag in
the Emacs frame.

Initially the speedbar lists the immediate contents of the current
directory, one file per line. Each line also has a box, `` `[+]' `` or
`` `<+>' ``, that you can click on with `Mouse-2` to "open up" the
contents of that item. If the line names a directory, opening it adds
the contents of that directory to the speedbar display, underneath the
directory's own line. If the line lists an ordinary file, opening it up
adds a list of the tags in that file to the speedbar display. When a
file is opened up, the `` `[+]' `` changes to `` `[-]' ``; you can click
on that box to "close up" that file (hide its contents).

Some major modes, including Rmail mode, Info, and GUD, have
specialized ways of putting useful items into the speedbar for you to
select. For example, in Rmail mode, the speedbar shows a list of Rmail
files, and lets you move the current message to another Rmail file by
clicking on its `` `<M>' `` box.

A speedbar belongs to one Emacs frame, and always operates on that
frame. If you use multiple frames, you can make a speedbar for some or
all of the frames; type `M-x speedbar` in any given frame to make a
speedbar for it.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC200) ] | [ [>](emacs.html#SEC202) ] | [ [<<](emacs.html#SEC193) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.10 Multiple Displays

A single Emacs can talk to more than one X display. Initially, Emacs
uses just one display--the one specified with the `DISPLAY`
environment variable or with the `` `--display' `` option (see section [AE.2 Initial Options](emacs.html#SEC530)). To connect to another display, use the command
`make-frame-on-display`:

`M-x make-frame-on-display RET display RET`: Create a new frame on display display.

A single X server can handle more than one screen. When you open
frames on two screens belonging to one server, Emacs knows they share a
single keyboard, and it treats all the commands arriving from these
screens as a single stream of input.

When you open frames on different X servers, Emacs makes a separate
input stream for each server. This way, two users can type
simultaneously on the two displays, and Emacs will not garble their
input. Each server also has its own selected frame. The commands you
enter with a particular X server apply to that server's selected frame.

Despite these features, people using the same Emacs job from different
displays can still interfere with each other if they are not careful.
For example, if any one types `C-x C-c`, that exits the Emacs job
for all of them!

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC201) ] | [ [>](emacs.html#SEC203) ] | [ [<<](emacs.html#SEC193) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.11 Special Buffer Frames

You can make certain chosen buffers, for which Emacs normally creates
a second window when you have just one window, appear in special frames
of their own. To do this, set the variable
`special-display-buffer-names` to a list of buffer names; any
buffer whose name is in that list automatically gets a special frame,
when an Emacs command wants to display it "in another window."

For example, if you set the variable this way,

|  |  |
| --- | --- |
|  | ``` (setq special-display-buffer-names       '("*Completions*" "*grep*" "*tex-shell*")) ``` |

then completion lists, `grep` output and the TeX mode shell
buffer get individual frames of their own. These frames, and the
windows in them, are never automatically split or reused for any other
buffers. They continue to show the buffers they were created for,
unless you alter them by hand. Killing the special buffer deletes its
frame automatically.

More generally, you can set `special-display-regexps` to a list
of regular expressions; then a buffer gets its own frame if its name
matches any of those regular expressions. (Once again, this applies only
to buffers that normally get displayed for you in a separate window.)

The variable `special-display-frame-alist` specifies the frame
parameters for these frames. It has a default value, so you don't need
to set it.

For those who know Lisp, an element of
`special-display-buffer-names` or `special-display-regexps`
can also be a list. Then the first element is the buffer name or
regular expression; the rest of the list specifies how to create the
frame. It can be an association list specifying frame parameter values;
these values take precedence over parameter values specified in
`special-display-frame-alist`. Alternatively, it can have this
form:

|  |  |
| --- | --- |
|  | ``` (function args...) ``` |

where function is a symbol. Then the frame is constructed by
calling function; its first argument is the buffer, and its
remaining arguments are args.

An analogous feature lets you specify buffers which should be
displayed in the selected window. See section [O.5 Forcing Display in the Same Window](emacs.html#SEC188). The
same-window feature takes precedence over the special-frame feature;
therefore, if you add a buffer name to
`special-display-buffer-names` and it has no effect, check to see
whether that feature is also in use for the same buffer name.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC202) ] | [ [>](emacs.html#SEC204) ] | [ [<<](emacs.html#SEC193) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.12 Setting Frame Parameters

This section describes commands for altering the display style and
window management behavior of the selected frame.

`M-x set-foreground-color RET color RET`: Specify color color for the foreground of the selected frame. (This also changes the foreground color of the default face.) `M-x set-background-color RET color RET`: Specify color color for the background of the selected frame. (This also changes the background color of the default face.) `M-x set-cursor-color RET color RET`: Specify color color for the cursor of the selected frame. `M-x set-mouse-color RET color RET`: Specify color color for the mouse cursor when it is over the selected frame. `M-x set-border-color RET color RET`: Specify color color for the border of the selected frame. `M-x list-colors-display`: Display the defined color names and show what the colors look like. This command is somewhat slow. `M-x auto-raise-mode`: Toggle whether or not the selected frame should auto-raise. Auto-raise means that every time you move the mouse onto the frame, it raises the frame. Note that this auto-raise feature is implemented by Emacs itself. Some window managers also implement auto-raise. If you enable auto-raise for Emacs frames in your X window manager, it should work, but it is beyond Emacs's control and therefore `auto-raise-mode` has no effect on it. `M-x auto-lower-mode`: Toggle whether or not the selected frame should auto-lower. Auto-lower means that every time you move the mouse off the frame, the frame moves to the bottom of the stack of X windows. The command `auto-lower-mode` has no effect on auto-lower implemented by the X window manager. To control that, you must use the appropriate window manager features. `M-x set-frame-font RET font RET`: Specify font font as the principal font for the selected frame. The principal font controls several face attributes of the `default` face (see section [J.1 Using Multiple Typefaces](emacs.html#SEC78)). For example, if the principal font has a height of 12 pt, all text will be drawn in 12 pt fonts, unless you use another face that specifies a different height. See section [AE.7 Font Specification Options](emacs.html#SEC537), for ways to list the available fonts on your system. You can also set a frame's principal font through a pop-up menu. Press `S-Mouse-1` to activate this menu.

In Emacs versions that use an X toolkit, the color-setting and
font-setting functions don't affect menus and the menu bar, since they
are displayed by their own widget classes. To change the appearance of
the menus and menu bar, you must use X resources (see section [AE.13 X Resources](emacs.html#SEC543)).
See section [AE.8 Window Color Options](emacs.html#SEC538), regarding colors. See section [AE.7 Font Specification Options](emacs.html#SEC537), regarding choice of
font.

Colors, fonts, and other attributes of the frame's display can also
be customized by setting frame parameters in the variable
`default-frame-alist` (see section [P.7 Creating Frames](emacs.html#SEC198)). For a detailed
description of frame parameters and customization, see section `Frame Parameters' in The Emacs Lisp Reference Manual.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC203) ] | [ [>](emacs.html#SEC205) ] | [ [<<](emacs.html#SEC193) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.13 Scroll Bars

When using X, Emacs normally makes a *scroll bar* at the left of
each Emacs window.[(3)](emacs.html#FOOT3)
The scroll bar runs the height of the window, and shows a moving
rectangular inner box which represents the portion of the buffer
currently displayed. The entire height of the scroll bar represents the
entire length of the buffer.

You can use `Mouse-2` (normally, the middle button) in the scroll
bar to move or drag the inner box up and down. If you move it to the
top of the scroll bar, you see the top of the buffer. If you move it to
the bottom of the scroll bar, you see the bottom of the buffer.

The left and right buttons in the scroll bar scroll by controlled
increments. `Mouse-1` (normally, the left button) moves the line at
the level where you click up to the top of the window. `Mouse-3`
(normally, the right button) moves the line at the top of the window
down to the level where you click. By clicking repeatedly in the same
place, you can scroll by the same distance over and over.

If you are using Emacs's own implementation of scroll bars, as opposed
to scroll bars from an X toolkit, you can also click `C-Mouse-2` in
the scroll bar to split a window vertically. The split occurs on the
line where you click.

You can enable or disable Scroll Bar mode with the command `M-x
scroll-bar-mode`. With no argument, it toggles the use of scroll bars.
With an argument, it turns use of scroll bars on if and only if the
argument is positive. This command applies to all frames, including
frames yet to be created. Customize the option `scroll-bar-mode`
to control the use of scroll bars at startup. You can use it to specify
that they are placed at the right of windows if you prefer that. You
can use the X resource `` `verticalScrollBars' `` to control the initial
setting of Scroll Bar mode similarly. See section [AE.13 X Resources](emacs.html#SEC543).

To enable or disable scroll bars for just the selected frame, use the
`M-x toggle-scroll-bar` command.

You can control the scroll bar width by changing the value of the
`scroll-bar-width` frame parameter.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC204) ] | [ [>](emacs.html#SEC206) ] | [ [<<](emacs.html#SEC193) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.14 Scrolling With "Wheeled" Mice

Some mice have a "wheel" instead of a third button. You can
usually click the wheel to act as either `Mouse-2` or
`Mouse-3`, depending on the setup. You can also use the wheel to
scroll windows instead of using the scroll bar or keyboard commands.
To do so, turn on Mouse Wheel global minor mode with the command
`M-x mouse-wheel-mode` or by customizing the option
`mouse-wheel-mode`. Support for the wheel depends on the system
generating appropriate events for Emacs.

The variables `mouse-wheel-follow-mouse` and
`mouse-wheel-scroll-amount` determine where and by how much
buffers are scrolled.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC205) ] | [ [>](emacs.html#SEC207) ] | [ [<<](emacs.html#SEC193) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.15 Menu Bars

You can turn display of menu bars on or off with `M-x
menu-bar-mode` or by customizing the option `menu-bar-mode`.
With no argument, this command toggles Menu Bar mode, a
minor mode. With an argument, the command turns Menu Bar mode on if the
argument is positive, off if the argument is not positive. You can use
the X resource `` `menuBarLines' `` to control the initial setting of
Menu Bar mode. See section [AE.13 X Resources](emacs.html#SEC543).

Expert users often turn off the menu bar, especially on text-only
terminals, where this makes one additional line available for text.
If the menu bar is off, you can still pop up a menu of its contents
with `C-Mouse-3` on a display which supports pop-up menus.
See section [P.5 Mouse Clicks for Menus](emacs.html#SEC196).

See section [B.4 The Menu Bar](emacs.html#SEC12), for information on how to invoke commands with the
menu bar.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC206) ] | [ [>](emacs.html#SEC208) ] | [ [<<](emacs.html#SEC193) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.16 Tool Bars

The *tool bar* is a line (or multiple lines) of icons at the top
of the Emacs window. You can click on these icons with the mouse
to do various jobs.

The global tool bar contains general commands. Some major modes
define their own tool bars to replace it. A few "special" modes
that are not designed for ordinary editing remove some items from the
global tool bar.

Tool bars work only on a graphical display. The tool bar uses colored
XPM icons if Emacs was built with XPM support. Otherwise, the tool
bar uses monochrome icons (PBM or XBM format).

You can turn display of tool bars on or off with `M-x
tool-bar-mode`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC207) ] | [ [>](emacs.html#SEC209) ] | [ [<<](emacs.html#SEC193) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.17 Using Dialog Boxes

A dialog box is a special kind of menu for asking you a yes-or-no
question or some other special question. Many Emacs commands use a
dialog box to ask a yes-or-no question, if you used the mouse to
invoke the command to begin with.

You can customize the option `use-dialog-box` to suppress the
use of dialog boxes. This also controls whether to use file selection
windows (but those are not supported on all platforms).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC208) ] | [ [>](emacs.html#SEC210) ] | [ [<<](emacs.html#SEC193) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.18 Tooltips (or "Balloon Help")

Tooltips are small X windows displaying a help string at the current
mouse position, typically over text--including the mode line--which
can be activated with the mouse or other keys. (This facility is
sometimes known as *balloon help*.) Help text may be available for
menu items too.

To use tooltips, enable Tooltip mode with the command `M-x
tooltip-mode`. The customization group `tooltip` controls
various aspects of how tooltips work. When Tooltip mode is disabled,
the help text is displayed in the echo area instead.

As of Emacs 21.1, tooltips are not supported on MS-Windows.
So help text always appears in the echo area.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC209) ] | [ [>](emacs.html#SEC211) ] | [ [<<](emacs.html#SEC193) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.19 Mouse Avoidance

Mouse Avoidance mode keeps the window system mouse pointer away from
point, to avoid obscuring text. Whenever it moves the mouse, it also
raises the frame. To use Mouse Avoidance mode, customize the option
`mouse-avoidance-mode`. You can set this to various values to
move the mouse in several ways:

`banish`: Move the mouse to the upper-right corner on any key-press; `exile`: Move the mouse to the corner only if the cursor gets too close, and allow it to return once the cursor is out of the way; `jump`: If the cursor gets too close to the mouse, displace the mouse a random distance & direction; `animate`: As `jump`, but shows steps along the way for illusion of motion; `cat-and-mouse`: The same as `animate`; `proteus`: As `animate`, but changes the shape of the mouse pointer too.

You can also use the command `M-x mouse-avoidance-mode` to enable
the mode.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC210) ] | [ [>](emacs.html#SEC212) ] | [ [<<](emacs.html#SEC194) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.20 Non-Window Terminals

If your terminal does not have a window system that Emacs supports,
then it can display only one Emacs frame at a time. However, you can
still create multiple Emacs frames, and switch between them. Switching
frames on these terminals is much like switching between different
window configurations.

Use `C-x 5 2` to create a new frame and switch to it; use `C-x
5 o` to cycle through the existing frames; use `C-x 5 0` to delete
the current frame.

Each frame has a number to distinguish it. If your terminal can
display only one frame at a time, the selected frame's number n
appears near the beginning of the mode line, in the form
`` `Fn' ``.

`` `Fn' `` is actually the frame's name. You can also specify a
different name if you wish, and you can select a frame by its name. Use
the command `M-x set-frame-name RET name RET` to
specify a new name for the selected frame, and use `M-x
select-frame-by-name RET name RET` to select a frame
according to its name. The name you specify appears in the mode line
when the frame is selected.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC211) ] | [ [>](emacs.html#SEC213) ] | [ [<<](emacs.html#SEC194) ] | [ [Up](emacs.html#SEC191) ] | [ [>>](emacs.html#SEC213) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## P.21 Using a Mouse in Terminal Emulators

Some terminal emulators under X support mouse clicks in the terminal
window. In a terminal emulator which is compatible with `xterm`,
you can use `M-x xterm-mouse-mode` to enable simple use of the
mouse--only single clicks are supported. The normal `xterm` mouse
functionality is still available by holding down the `SHIFT` key
when you press the mouse button. The Linux console supports this
mode if it has support for the mouse enabled, e.g. using the
`gpm` daemon.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC212) ] | [ [>](emacs.html#SEC214) ] | [ [<<](emacs.html#SEC191) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC227) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# Q. International Character Set Support

Emacs supports a wide variety of international character sets,
including European variants of the Latin alphabet, as well as Chinese,
Cyrillic, Devanagari (Hindi and Marathi), Ethiopic, Greek, Hebrew, IPA,
Japanese, Korean, Lao, Thai, Tibetan, and Vietnamese scripts. These features
have been merged from the modified version of Emacs known as MULE (for
"MULti-lingual Enhancement to GNU Emacs")

Emacs also supports various encodings of these characters used by
other internationalized software, such as word processors and mailers.

Emacs allows editing text with international characters by supporting
all the related activities:

* You can visit files with non-ASCII characters, save non-ASCII text, and
  pass non-ASCII text between Emacs and programs it invokes (such as
  compilers, spell-checkers, and mailers). Setting your language
  environment (see section [Q.3 Language Environments](emacs.html#SEC216)) takes care of setting up the
  coding systems and other options for a specific language or culture.
  Alternatively, you can specify how Emacs should encode or decode text
  for each command; see [Q.9 Specifying a Coding System](emacs.html#SEC222).

  * You can display non-ASCII characters encoded by the various scripts.
    This works by using appropriate fonts on X and similar graphics
    displays (see section [Q.11 Defining fontsets](emacs.html#SEC224)), and by sending special codes to
    text-only displays (see section [Q.9 Specifying a Coding System](emacs.html#SEC222)). If some characters are
    displayed incorrectly, refer to [Q.12 Undisplayable Characters](emacs.html#SEC225), which
    describes possible problems and explains how to solve them.

    * You can insert non-ASCII characters or search for them. To do that,
      you can specify an input method (see section [Q.5 Selecting an Input Method](emacs.html#SEC218)) suitable
      for your language, or use the default input method set up when you set
      your language environment. (Emacs input methods are part of the Leim
      package, which must be installed for you to be able to use them.) If
      your keyboard can produce non-ASCII characters, you can select an
      appropriate keyboard coding system (see section [Q.9 Specifying a Coding System](emacs.html#SEC222)), and Emacs
      will accept those characters. Latin-1 characters can also be input by
      using the `C-x 8` prefix, see [C-x 8](emacs.html#SEC226). On X Window systems, your locale should be set to an
      appropriate value to make sure Emacs interprets keyboard input
      correctly, see [locales](emacs.html#SEC216).

The rest of this chapter describes these issues in detail.

> |  |  |  |
> | --- | --- | --- |
> | [Q.1 Introduction to International Character Sets](emacs.html#SEC214) |  | Basic concepts of multibyte characters. |
> | [Q.2 Enabling Multibyte Characters](emacs.html#SEC215) |  | Controlling whether to use multibyte characters. |
> | [Q.3 Language Environments](emacs.html#SEC216) |  | Setting things up for the language you use. |
> | [Q.4 Input Methods](emacs.html#SEC217) |  | Entering text characters not on your keyboard. |
> | [Q.5 Selecting an Input Method](emacs.html#SEC218) |  | Specifying your choice of input methods. |
> | [Q.6 Unibyte and Multibyte Non-ASCII characters](emacs.html#SEC219) |  | How single-byte characters convert to multibyte. |
> | [Q.7 Coding Systems](emacs.html#SEC220) |  | Character set conversion when you read and write files, and so on. |
> | [Q.8 Recognizing Coding Systems](emacs.html#SEC221) |  | How Emacs figures out which conversion to use. |
> | [Q.9 Specifying a Coding System](emacs.html#SEC222) |  | Various ways to choose which conversion to use. |
> | [Q.10 Fontsets](emacs.html#SEC223) |  | Fontsets are collections of fonts that cover the whole spectrum of characters. |
> | [Q.11 Defining fontsets](emacs.html#SEC224) |  | Defining a new fontset. |
> | [Q.12 Undisplayable Characters](emacs.html#SEC225) |  | When characters don't display. |
> | [Q.13 Single-byte Character Set Support](emacs.html#SEC226) |  | You can pick one European character set to use without multibyte characters. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC213) ] | [ [>](emacs.html#SEC215) ] | [ [<<](emacs.html#SEC213) ] | [ [Up](emacs.html#SEC213) ] | [ [>>](emacs.html#SEC227) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Q.1 Introduction to International Character Sets

The users of international character sets and scripts have established
many more-or-less standard coding systems for storing files. Emacs
internally uses a single multibyte character encoding, so that it can
intermix characters from all these scripts in a single buffer or string.
This encoding represents each non-ASCII character as a sequence of bytes
in the range 0200 through 0377. Emacs translates between the multibyte
character encoding and various other coding systems when reading and
writing files, when exchanging data with subprocesses, and (in some
cases) in the `C-q` command (see section [Q.6 Unibyte and Multibyte Non-ASCII characters](emacs.html#SEC219)).

The command `C-h h` (`view-hello-file`) displays the file
`etc/HELLO', which shows how to say "hello" in many languages.
This illustrates various scripts. If some characters can't be
displayed on your terminal, they appear as `` `?' `` or as hollow boxes
(see section [Q.12 Undisplayable Characters](emacs.html#SEC225)).

Keyboards, even in the countries where these character sets are used,
generally don't have keys for all the characters in them. So Emacs
supports various *input methods*, typically one for each script or
language, to make it convenient to type them.

The prefix key `C-x RET` is used for commands that pertain
to multibyte characters, coding systems, and input methods.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC214) ] | [ [>](emacs.html#SEC216) ] | [ [<<](emacs.html#SEC216) ] | [ [Up](emacs.html#SEC213) ] | [ [>>](emacs.html#SEC227) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Q.2 Enabling Multibyte Characters

You can enable or disable multibyte character support, either for
Emacs as a whole, or for a single buffer. When multibyte characters are
disabled in a buffer, then each byte in that buffer represents a
character, even codes 0200 through 0377. The old features for
supporting the European character sets, ISO Latin-1 and ISO Latin-2,
work as they did in Emacs 19 and also work for the other ISO 8859
character sets.

However, there is no need to turn off multibyte character support to
use ISO Latin; the Emacs multibyte character set includes all the
characters in these character sets, and Emacs can translate
automatically to and from the ISO codes.

By default, Emacs starts in multibyte mode, because that allows you to
use all the supported languages and scripts without limitations.

To edit a particular file in unibyte representation, visit it using
`find-file-literally`. See section [M.2 Visiting Files](emacs.html#SEC112). To convert a buffer in
multibyte representation into a single-byte representation of the same
characters, the easiest way is to save the contents in a file, kill the
buffer, and find the file again with `find-file-literally`. You
can also use `C-x RET c`
(`universal-coding-system-argument`) and specify `` `raw-text' `` as
the coding system with which to find or save a file. See section [Q.9 Specifying a Coding System](emacs.html#SEC222). Finding a file as `` `raw-text' `` doesn't disable format
conversion, uncompression and auto mode selection as
`find-file-literally` does.

To turn off multibyte character support by default, start Emacs with
the `` `--unibyte' `` option (see section [AE.2 Initial Options](emacs.html#SEC530)), or set the
environment variable `EMACS_UNIBYTE`. You can also customize
`enable-multibyte-characters` or, equivalently, directly set the
variable `default-enable-multibyte-characters` to `nil` in
your init file to have basically the same effect as `` `--unibyte' ``.

To convert a unibyte session to a multibyte session, set
`default-enable-multibyte-characters` to `t`. Buffers which
were created in the unibyte session before you turn on multibyte support
will stay unibyte. You can turn on multibyte support in a specific
buffer by invoking the command `toggle-enable-multibyte-characters`
in that buffer.

With `` `--unibyte' ``, multibyte strings are not created during
initialization from the values of environment variables,
`/etc/passwd' entries etc. that contain non-ASCII 8-bit
characters.

Emacs normally loads Lisp files as multibyte, regardless of whether
you used `` `--unibyte' ``. This includes the Emacs initialization
file, `.emacs', and the initialization files of Emacs packages
such as Gnus. However, you can specify unibyte loading for a
particular Lisp file, by putting `` `-*-unibyte: t;-*-' `` in a
comment on the first line. Then that file is always loaded as unibyte
text, even if you did not start Emacs with `` `--unibyte' ``. The
motivation for these conventions is that it is more reliable to always
load any particular Lisp file in the same way. However, you can load
a Lisp file as unibyte, on any one occasion, by typing `C-x
RET c raw-text RET` immediately before loading it.

The mode line indicates whether multibyte character support is enabled
in the current buffer. If it is, there are two or more characters (most
often two dashes) before the colon near the beginning of the mode line.
When multibyte characters are not enabled, just one dash precedes the
colon.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC215) ] | [ [>](emacs.html#SEC217) ] | [ [<<](emacs.html#SEC217) ] | [ [Up](emacs.html#SEC213) ] | [ [>>](emacs.html#SEC227) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Q.3 Language Environments

All supported character sets are supported in Emacs buffers whenever
multibyte characters are enabled; there is no need to select a
particular language in order to display its characters in an Emacs
buffer. However, it is important to select a *language environment*
in order to set various defaults. The language environment really
represents a choice of preferred script (more or less) rather than a
choice of language.

The language environment controls which coding systems to recognize
when reading text (see section [Q.8 Recognizing Coding Systems](emacs.html#SEC221)). This applies to files,
incoming mail, netnews, and any other text you read into Emacs. It may
also specify the default coding system to use when you create a file.
Each language environment also specifies a default input method.

To select a language environment, customize the option
`current-language-environment` or use the command `M-x
set-language-environment`. It makes no difference which buffer is
current when you use this command, because the effects apply globally to
the Emacs session. The supported language environments include:

> Chinese-BIG5, Chinese-CNS, Chinese-GB, Cyrillic-ALT, Cyrillic-ISO,
> Cyrillic-KOI8, Czech, Devanagari, Dutch, English, Ethiopic, German,
> Greek, Hebrew, IPA, Japanese, Korean, Lao, Latin-1, Latin-2, Latin-3,
> Latin-4, Latin-5, Latin-8 (Celtic), Latin-9 (updated Latin-1, with the
> Euro sign), Polish, Romanian, Slovak, Slovenian, Spanish, Thai,
> Tibetan, Turkish, and Vietnamese.

To display the script(s) used by your language environment on a
graphical display, you need to have a suitable font. If some of the
characters appear as empty boxes, you should install the GNU Intlfonts
package, which includes fonts for all supported scripts.[(4)](emacs.html#FOOT4)
See section [Q.10 Fontsets](emacs.html#SEC223), for more details about setting up your fonts.

Some operating systems let you specify the character-set locale you
are using by setting the locale environment variables `LC_ALL`,
`LC_CTYPE`, or `LANG`.[(5)](emacs.html#FOOT5) During startup, Emacs looks up your character-set locale's
name in the system locale alias table, matches its canonical name
against entries in the value of the variables
`locale-charset-language-names` and `locale-language-names`,
and selects the corresponding language environment if a match is found.
(The former variable overrides the latter.) It also adjusts the display
table and terminal coding system, the locale coding system, the
preferred coding system as needed for the locale, and--last but not
least--the way Emacs decodes non-ASCII characters sent by your keyboard.

If you modify the `LC_ALL`, `LC_CTYPE`, or `LANG`
environment variables while running Emacs, you may want to invoke the
`set-locale-environment` function afterwards to readjust the
language environment from the new locale.

The `set-locale-environment` function normally uses the preferred
coding system established by the language environment to decode system
messages. But if your locale matches an entry in the variable
`locale-preferred-coding-systems`, Emacs uses the corresponding
coding system instead. For example, if the locale `` `ja_JP.PCK' ``
matches `japanese-shift-jis` in
`locale-preferred-coding-systems`, Emacs uses that encoding even
though it might normally use `japanese-iso-8bit`.

You can override the language environment chosen at startup with
explicit use of the command `set-language-environment`, or with
customization of `current-language-environment` in your init
file.

To display information about the effects of a certain language
environment lang-env, use the command `C-h L lang-env
RET` (`describe-language-environment`). This tells you which
languages this language environment is useful for, and lists the
character sets, coding systems, and input methods that go with it. It
also shows some sample text to illustrate scripts used in this language
environment. By default, this command describes the chosen language
environment.

You can customize any language environment with the normal hook
`set-language-environment-hook`. The command
`set-language-environment` runs that hook after setting up the new
language environment. The hook functions can test for a specific
language environment by checking the variable
`current-language-environment`. This hook is where you should
put non-default settings for specific language environment, such as
coding systems for keyboard input and terminal output, the default
input method, etc.

Before it starts to set up the new language environment,
`set-language-environment` first runs the hook
`exit-language-environment-hook`. This hook is useful for undoing
customizations that were made with `set-language-environment-hook`.
For instance, if you set up a special key binding in a specific language
environment using `set-language-environment-hook`, you should set
up `exit-language-environment-hook` to restore the normal binding
for that key.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC216) ] | [ [>](emacs.html#SEC218) ] | [ [<<](emacs.html#SEC218) ] | [ [Up](emacs.html#SEC213) ] | [ [>>](emacs.html#SEC227) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Q.4 Input Methods

An *input method* is a kind of character conversion designed
specifically for interactive input. In Emacs, typically each language
has its own input method; sometimes several languages which use the same
characters can share one input method. A few languages support several
input methods.

The simplest kind of input method works by mapping ASCII letters
into another alphabet; this allows you to use one other alphabet
instead of ASCII. The Greek and Russian input methods
work this way.
