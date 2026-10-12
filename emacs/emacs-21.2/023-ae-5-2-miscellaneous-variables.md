### AE.5.2 Miscellaneous Variables

These variables are used only on particular configurations:

`COMSPEC`: On MS-DOS and MS-Windows, the name of the command interpreter to use when invoking batch files and commands internal to the shell. On MS-DOS this is also used to make a default value for the `SHELL` environment variable. `NAME`: On MS-DOS, this variable defaults to the value of the `USER` variable. `TEMP`: `TMP`: On MS-DOS and MS-Windows, these specify the name of the directory for storing temporary files in. `EMACSTEST`: On MS-DOS, this specifies a file to use to log the operation of the internal terminal emulator. This feature is useful for submitting bug reports. `EMACSCOLORS`: On MS-DOS, this specifies the screen colors. It is useful to set them this way, since otherwise Emacs would display the default colors momentarily when it starts up. The value of this variable should be the two-character encoding of the foreground (the first character) and the background (the second character) colors of the default face. Each character should be the hexadecimal code for the desired color on a standard PC text-mode display. For example, to get blue text on a light gray background, specify `` `EMACSCOLORS=17' ``, since 1 is the code of the blue color and 7 is the code of the light gray color. The PC display usually supports only eight background colors. However, Emacs switches the DOS display to a mode where all 16 colors can be used for the background, so all four bits of the background color are actually used. `WINDOW_GFX`: Used when initializing the Sun windows system.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC535) ] | [ [>](emacs.html#SEC537) ] | [ [<<](emacs.html#SEC537) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.6 Specifying the Display Name

The environment variable `DISPLAY` tells all X clients, including
Emacs, where to display their windows. Its value is set by default
in ordinary circumstances, when you start an X server and run jobs
locally. Occasionally you may need to specify the display yourself; for
example, if you do a remote login and want to run a client program
remotely, displaying on your local screen.

With Emacs, the main reason people change the default display is to
let them log into another system, run Emacs on that system, but have the
window displayed at their local terminal. You might need to log in
to another system because the files you want to edit are there, or
because the Emacs executable file you want to run is there.

The syntax of the `DISPLAY` environment variable is
`` `host:display.screen' ``, where host is the
host name of the X Window System server machine, display is an
arbitrarily-assigned number that distinguishes your server (X terminal)
from other servers on the same machine, and screen is a
rarely-used field that allows an X server to control multiple terminal
screens. The period and the screen field are optional. If
included, screen is usually zero.

For example, if your host is named `` `glasperle' `` and your server is
the first (or perhaps the only) server listed in the configuration, your
`DISPLAY` is `` `glasperle:0.0' ``.

You can specify the display name explicitly when you run Emacs, either
by changing the `DISPLAY` variable, or with the option `` `-d
display' `` or `` `--display=display' ``. Here is an example:

|  |  |
| --- | --- |
|  | ``` emacs --display=glasperle:0 & ``` |

You can inhibit the direct use of the window system and GUI with the
`` `-nw' `` option. It tells Emacs to display using ordinary ASCII on
its controlling terminal. This is also an initial option.

Sometimes, security arrangements prevent a program on a remote system
from displaying on your local system. In this case, trying to run Emacs
produces messages like this:

|  |  |
| --- | --- |
|  | ``` Xlib:  connection to "glasperle:0.0" refused by server ``` |

You might be able to overcome this problem by using the `xhost`
command on the local system to give permission for access from your
remote machine.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC536) ] | [ [>](emacs.html#SEC538) ] | [ [<<](emacs.html#SEC538) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.7 Font Specification Options

By default, Emacs displays text in the font named `` `9x15' ``, which
makes each character nine pixels wide and fifteen pixels high. You can
specify a different font on your command line through the option
`` `-fn name' `` (or `` `--font' ``, which is an alias for
`` `-fn' ``).

`` `-fn name' ``: `` `--font=name' ``: Use font name as the default font.

Under X, each font has a long name which consists of eleven words or
numbers, separated by dashes. Some fonts also have shorter
nicknames---`` `9x15' `` is such a nickname. You can use either kind of
name. You can use wildcard patterns for the font name; then Emacs lets
X choose one of the fonts that match the pattern. Here is an example,
which happens to specify the font whose nickname is `` `6x13' ``:

|  |  |
| --- | --- |
|  | ``` emacs -fn "-misc-fixed-medium-r-semicondensed--13-*-*-*-c-60-iso8859-1" & ``` |

You can also specify the font in your `.Xdefaults' file:

|  |  |
| --- | --- |
|  | ``` emacs.font: -misc-fixed-medium-r-semicondensed--13-*-*-*-c-60-iso8859-1 ``` |

A long font name has the following form:

|  |  |
| --- | --- |
|  | ``` -maker-family-weight-slant-widthtype-style... ...-pixels-height-horiz-vert-spacing-width-charset ``` |

maker: This is the name of the font manufacturer. family: This is the name of the font family--for example, `` `courier' ``. weight: This is normally `` `bold' ``, `` `medium' `` or `` `light' ``. Other words may appear here in some font names. slant: This is `` `r' `` (roman), `` `i' `` (italic), `` `o' `` (oblique), `` `ri' `` (reverse italic), or `` `ot' `` (other). widthtype: This is normally `` `condensed' ``, `` `extended' ``, `` `semicondensed' `` or `` `normal' ``. Other words may appear here in some font names. style: This is an optional additional style name. Usually it is empty--most long font names have two hyphens in a row at this point. pixels: This is the font height, in pixels. height: This is the font height on the screen, measured in tenths of a printer's point--approximately 1/720 of an inch. In other words, it is the point size of the font, times ten. For a given vertical resolution, height and pixels are proportional; therefore, it is common to specify just one of them and use `` `*' `` for the other. horiz: This is the horizontal resolution, in pixels per inch, of the screen for which the font is intended. vert: This is the vertical resolution, in pixels per inch, of the screen for which the font is intended. Normally the resolution of the fonts on your system is the right value for your screen; therefore, you normally specify `` `*' `` for this and horiz. spacing: This is `` `m' `` (monospace), `` `p' `` (proportional) or `` `c' `` (character cell). width: This is the average character width, in pixels, multiplied by ten. charset: This is the character set that the font depicts. Normally you should use `` `iso8859-1' ``.

You will probably want to use a fixed-width default font--that is,
a font in which all characters have the same width. Any font with
`` `m' `` or `` `c' `` in the spacing field of the long name is a
fixed-width font. Here's how to use the `xlsfonts` program to
list all the fixed-width fonts available on your system:

|  |  |
| --- | --- |
|  | ``` xlsfonts -fn '*x*' | egrep "^[0-9]+x[0-9]+" xlsfonts -fn '*-*-*-*-*-*-*-*-*-*-*-m*' xlsfonts -fn '*-*-*-*-*-*-*-*-*-*-*-c*' ``` |

To see what a particular font looks like, use the `xfd` command.
For example:

|  |  |
| --- | --- |
|  | ``` xfd -fn 6x13 ``` |

displays the entire font `` `6x13' ``.

While running Emacs, you can set the font of the current frame
(see section [P.12 Setting Frame Parameters](emacs.html#SEC203)) or for a specific kind of text
(see section [J.1 Using Multiple Typefaces](emacs.html#SEC78)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC537) ] | [ [>](emacs.html#SEC539) ] | [ [<<](emacs.html#SEC539) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.8 Window Color Options

On a color display, you can specify which color to use for various
parts of the Emacs display. To find out what colors are available on
your system, type `M-x list-colors-display`, or press
`C-Mouse-2` and select `` `Display Colors' `` from the pop-up menu.
If you do not specify colors, on windowed displays the default for the
background is white and the default for all other colors is black. On a
monochrome display, the foreground is black, the background is white,
and the border is gray if the display supports that. On terminals, the
background is usually black and the foreground is white.

Here is a list of the command-line options for specifying colors:

`` `-fg color' ``: `` `--foreground-color=color' ``: Specify the foreground color. color should be a standard color name, or a numeric specification of the color's red, green, and blue components as in `` `#4682B4' `` or `` `RGB:46/82/B4' ``. `` `-bg color' ``: `` `--background-color=color' ``: Specify the background color. `` `-bd color' ``: `` `--border-color=color' ``: Specify the color of the border of the X window. `` `-cr color' ``: `` `--cursor-color=color' ``: Specify the color of the Emacs cursor which indicates where point is. `` `-ms color' ``: `` `--mouse-color=color' ``: Specify the color for the mouse cursor when the mouse is in the Emacs window. `` `-r' ``: `` `-rv' ``: `` `--reverse-video' ``: Reverse video--swap the foreground and background colors.

For example, to use a coral mouse cursor and a slate blue text cursor,
enter:

|  |  |
| --- | --- |
|  | ``` emacs -ms coral -cr 'slate blue' & ``` |

You can reverse the foreground and background colors through the
`` `-rv' `` option or with the X resource `` `reverseVideo' ``.

The `` `-fg' ``, `` `-bg' ``, and `` `-rv' `` options function on
text-only terminals as well as on window systems.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC538) ] | [ [>](emacs.html#SEC540) ] | [ [<<](emacs.html#SEC528) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.9 Options for Window Geometry

The `` `--geometry' `` option controls the size and position of the
initial Emacs frame. Here is the format for specifying the window
geometry:

`` `-g widthxheight[{+-}xoffset{+-}yoffset]]' ``: Specify window size width and height (measured in character columns and lines), and positions xoffset and yoffset (measured in pixels). `` `--geometry=widthxheight[{+-}xoffset{+-}yoffset]]' ``: This is another way of writing the same thing.

`{+-}` means either a plus sign or a minus sign. A plus
sign before xoffset means it is the distance from the left side of
the screen; a minus sign means it counts from the right side. A plus
sign before yoffset means it is the distance from the top of the
screen, and a minus sign there indicates the distance from the bottom.
The values xoffset and yoffset may themselves be positive or
negative, but that doesn't change their meaning, only their direction.

Emacs uses the same units as `xterm` does to interpret the geometry.
The width and height are measured in characters, so a large font
creates a larger frame than a small font. (If you specify a proportional
font, Emacs uses its maximum bounds width as the width unit.) The
xoffset and yoffset are measured in pixels.

Since the mode line and the echo area occupy the last 2 lines of the
frame, the height of the initial text window is 2 less than the height
specified in your geometry. In non-X-toolkit versions of Emacs, the
menu bar also takes one line of the specified number. But in the X
toolkit version, the menu bar is additional and does not count against
the specified height. The tool bar, if present, is also additional.

You do not have to specify all of the fields in the geometry
specification.

If you omit both xoffset and yoffset, the window manager
decides where to put the Emacs frame, possibly by letting you place
it with the mouse. For example, `` `164x55' `` specifies a window 164
columns wide, enough for two ordinary width windows side by side, and 55
lines tall.

The default width for Emacs is 80 characters and the default height is
40 lines. You can omit either the width or the height or both. If
you start the geometry with an integer, Emacs interprets it as the
width. If you start with an `` `x' `` followed by an integer, Emacs
interprets it as the height. Thus, `` `81' `` specifies just the width;
`` `x45' `` specifies just the height.

If you start with `` `+' `` or `` `-' ``, that introduces an offset,
which means both sizes are omitted. Thus, `` `-3' `` specifies the
xoffset only. (If you give just one offset, it is always
xoffset.) `` `+3-3' `` specifies both the xoffset and the
yoffset, placing the frame near the bottom left of the screen.

You can specify a default for any or all of the fields in
`.Xdefaults' file, and then override selected fields with a
`` `--geometry' `` option.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC539) ] | [ [>](emacs.html#SEC541) ] | [ [<<](emacs.html#SEC530) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.10 Internal and External Borders

An Emacs frame has an internal border and an external border. The
internal border is an extra strip of the background color around the
text portion of the frame. Emacs itself draws the internal border.
The external border is added by the window manager outside the frame;
depending on the window manager you use, it may contain various boxes
you can click on to move or iconify the window.

`` `-ib width' ``: `` `--internal-border=width' ``: Specify width as the width of the internal border, in pixels. `` `-bw width' ``: `` `--border-width=width' ``: Specify width as the width of the main border, in pixels.

When you specify the size of the frame, that does not count the
borders. The frame's position is measured from the outside edge of the
external border.

Use the `` `-ib n' `` option to specify an internal border
n pixels wide. The default is 1. Use `` `-bw n' `` to
specify the width of the external border (though the window manager may
not pay attention to what you specify). The default width of the
external border is 2.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC540) ] | [ [>](emacs.html#SEC542) ] | [ [<<](emacs.html#SEC530) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.11 Frame Titles

An Emacs frame may or may not have a specified title. The frame
title, if specified, appears in window decorations and icons as the
name of the frame. If an Emacs frame has no specified title, the
default title has the form `` `invocation-name@machine' ``
(if there is only one frame) or the selected window's buffer name (if
there is more than one frame).

You can specify a title for the initial Emacs frame with a command
line option:

`` `-title title' ``: `` `--title=title' ``: `` `-T title' ``: Specify title as the title for the initial Emacs frame.

The `` `--name' `` option (see section [AE.13 X Resources](emacs.html#SEC543)) also specifies the title
for the initial Emacs frame.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC541) ] | [ [>](emacs.html#SEC543) ] | [ [<<](emacs.html#SEC530) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.12 Icons

Most window managers allow the user to "iconify" a frame, removing
it from sight, and leaving a small, distinctive "icon" window in its
place. Clicking on the icon window makes the frame itself appear again.
If you have many clients running at once, you can avoid cluttering up
the screen by iconifying most of the clients.

`` `-i' ``: `` `--icon-type' ``: Use a picture of a gnu as the Emacs icon. `` `-iconic' ``: `` `--iconic' ``: Start Emacs in iconified state.

The `` `-i' `` or `` `--icon-type' `` option tells Emacs to use an icon
window containing a picture of the GNU gnu. If omitted, Emacs lets the
window manager choose what sort of icon to use--usually just a small
rectangle containing the frame's title.

The `` `-iconic' `` option tells Emacs to begin running as an icon,
rather than showing a frame right away. In this situation, the icon
is the only indication that Emacs has started; the text frame doesn't
appear until you deiconify it.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC542) ] | [ [>](emacs.html#SEC544) ] | [ [<<](emacs.html#SEC530) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.13 X Resources

Programs running under the X Window System organize their user options
under a hierarchy of classes and resources. You can specify default
values for these options in your X resources file, usually named
`~/.Xdefaults'.

Each line in the file specifies a value for one option or for a
collection of related options, for one program or for several programs
(optionally even for all programs).

MS-Windows systems don't support `~/.Xdefaults' files, but
Emacs compiled for Windows looks for X resources in the Windows
Registry, under the keys `` `HKEY_CURRENT_USER\SOFTWARE\GNU\Emacs' ``
and `` `HKEY_LOCAL_MACHINE\SOFTWARE\GNU\Emacs' ``.

Programs define named resources with particular meanings. They also
define how to group resources into named classes. For instance, in
Emacs, the `` `internalBorder' `` resource controls the width of the
internal border, and the `` `borderWidth' `` resource controls the width
of the external border. Both of these resources are part of the
`` `BorderWidth' `` class. Case distinctions are significant in these
names.

In `~/.Xdefaults', you can specify a value for a single resource
on one line, like this:

|  |  |
| --- | --- |
|  | ``` emacs.borderWidth: 2 ``` |

Or you can use a class name to specify the same value for all resources
in that class. Here's an example:

|  |  |
| --- | --- |
|  | ``` emacs.BorderWidth: 2 ``` |

If you specify a value for a class, it becomes the default for all
resources in that class. You can specify values for individual
resources as well; these override the class value, for those particular
resources. Thus, this example specifies 2 as the default width for all
borders, but overrides this value with 4 for the external border:

|  |  |
| --- | --- |
|  | ``` emacs.BorderWidth: 2 emacs.borderWidth: 4 ``` |

The order in which the lines appear in the file does not matter.
Also, command-line options always override the X resources file.

The string `` `emacs' `` in the examples above is also a resource
name. It actually represents the name of the executable file that you
invoke to run Emacs. If Emacs is installed under a different name, it
looks for resources under that name instead of `` `emacs' ``.

`` `-name name' ``: `` `--name=name' ``: Use name as the resource name (and the title) for the initial Emacs frame. This option does not affect subsequent frames, but Lisp programs can specify frame names when they create frames. If you don't specify this option, the default is to use the Emacs executable's name as the resource name. `` `-xrm resource-values' ``: `` `--xrm=resource-values' ``: Specify X resource values for this Emacs job (see below).

For consistency, `` `-name' `` also specifies the name to use for
other resource values that do not belong to any particular frame.

The resources that name Emacs invocations also belong to a class; its
name is `` `Emacs' ``. If you write `` `Emacs' `` instead of
`` `emacs' ``, the resource applies to all frames in all Emacs jobs,
regardless of frame titles and regardless of the name of the executable
file. Here is an example:

|  |  |
| --- | --- |
|  | ``` Emacs.BorderWidth: 2 Emacs.borderWidth: 4 ``` |

You can specify a string of additional resource values for Emacs to
use with the command line option `` `-xrm resources' ``. The text
resources should have the same format that you would use inside a file
of X resources. To include multiple resource specifications in
resources, put a newline between them, just as you would in a file.
You can also use `` `#include "filename"' `` to include a file full
of resource specifications. Resource values specified with `` `-xrm' ``
take precedence over all other resource specifications.

The following table lists the resource names that designate options
for Emacs, each with the class that it belongs to:

`background` (class `Background`): Background color name. `bitmapIcon` (class `BitmapIcon`): Use a bitmap icon (a picture of a gnu) if `` `on' ``, let the window manager choose an icon if `` `off' ``. `borderColor` (class `BorderColor`): Color name for the external border. `borderWidth` (class `BorderWidth`): Width in pixels of the external border. `cursorColor` (class `Foreground`): Color name for text cursor (point). `font` (class `Font`): Font name for text (or fontset name, see section [Q.10 Fontsets](emacs.html#SEC223)). `foreground` (class `Foreground`): Color name for text. `geometry` (class `Geometry`): Window size and position. Be careful not to specify this resource as `` `emacs*geometry' ``, because that may affect individual menus as well as the Emacs frame itself. If this resource specifies a position, that position applies only to the initial Emacs frame (or, in the case of a resource for a specific frame name, only that frame). However, the size, if specified here, applies to all frames. `iconName` (class `Title`): Name to display in the icon. `internalBorder` (class `BorderWidth`): Width in pixels of the internal border. `lineSpacing` (class `LineSpacing`): Additional space (*leading*) between lines, in pixels. `menuBar` (class `MenuBar`): Give frames menu bars if `` `on' ``; don't have menu bars if `` `off' ``. `toolBar` (class `ToolBar`): Number of lines to reserve for the tool bar. A zero value suppresses the tool bar. If the value is non-zero and `auto-resize-tool-bars` is non-`nil`, the tool bar's size will be changed automatically so that all tool bar items are visible. `minibuffer` (class `Minibuffer`): If `` `none' ``, don't make a minibuffer in this frame. It will use a separate minibuffer frame instead. `paneFont` (class `Font`): Font name for menu pane titles, in non-toolkit versions of Emacs. `pointerColor` (class `Foreground`): Color of the mouse cursor. `reverseVideo` (class `ReverseVideo`): Switch foreground and background default colors if `` `on' ``, use colors as specified if `` `off' ``. `screenGamma` (class `ScreenGamma`): Gamma correction for colors, equivalent to the frame parameter `screen-gamma`. `selectionFont` (class `Font`): Font name for pop-up menu items, in non-toolkit versions of Emacs. (For toolkit versions, see [AE.14 Lucid Menu X Resources](emacs.html#SEC544), also see [AE.15 LessTif Menu X Resources](emacs.html#SEC545).) `synchronous` (class `Synchronous`): Run Emacs in synchronous mode if `` `on' ``. Synchronous mode is useful for debugging X problems. `title` (class `Title`): Name to display in the title bar of the initial Emacs frame. `verticalScrollBars` (class `ScrollBars`): Give frames scroll bars if `` `on' ``; don't have scroll bars if `` `off' ``.

Here are resources for controlling the appearance of particular faces
(see section [J.1 Using Multiple Typefaces](emacs.html#SEC78)):

`face.attributeFont`: Font for face face. `face.attributeForeground`: Foreground color for face face. `face.attributeBackground`: Background color for face face. `face.attributeUnderline`: Underline flag for face face. Use `` `on' `` or `` `true' `` for yes. `face.attributeFamily`: Font family for face face. `face.attributeWidth`: Relative proportional width of the font to use for face face. It should be one of `ultra-condensed`, `extra-condensed`, `condensed`, `semi-condensed`, `normal`, `semi-expanded`, `expanded`, `extra-expanded`, or `ultra-expanded`. `face.attributeHeight`: Height of the font to use for face face: either an integer specifying the height in units of 1/10pt, or a floating point number that specifies a scale factor to scale the underlying face's default font, or a function to be called with the default height which will return a new height. `face.attributeWeight`: A weight to use for the face face. It must be one of `ultra-bold`, `extra-bold`, `bold`, `semi-bold`, `normal`, `semi-light`, `light`, `extra-light`, `ultra-light`. `face.attributeSlant`: The slant to use for the font of face face. It must be one of `italic`, `oblique`, `normal`, `reverse-italic`, or `reverse-oblique`. `face.attributeStrikeThrough`: Whether the face face should be drawn with a line striking through the characters. `face.attributeOverline`: Whether the characters in the face face should be overlined. `face.attributeBox`: Whether to draw a box around the characters in face face. `face.attributeInverse`: Whether to display the characters in face face in inverse video. `face.attributeStipple`: The name of a pixmap data file to use for the stipple pattern, or `false` to not use stipple for the face face. `face.attributeBackgroundPixmap`: The background pixmap for the face face. Should be a name of a pixmap file or `false`. `face.attributeBold`: Whether to draw the characters in the face face as bold. `face.attributeItalic`: Whether to draw the characters in the face face as italic.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC543) ] | [ [>](emacs.html#SEC545) ] | [ [<<](emacs.html#SEC530) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.14 Lucid Menu X Resources

If the Emacs installed at your site was built to use the X toolkit
with the Lucid menu widgets, then the menu bar is a separate widget and
has its own resources. The resource names contain `` `pane.menubar' ``
(following, as always, the name of the Emacs invocation, or `` `Emacs' ``,
which stands for all Emacs invocations). Specify them like this:

|  |  |
| --- | --- |
|  | ``` Emacs.pane.menubar.resource:  value ``` |

For example, to specify the font `` `8x16' `` for the menu-bar items,
write this:

|  |  |
| --- | --- |
|  | ``` Emacs.pane.menubar.font:  8x16 ``` |

Resources for *non-menubar* toolkit pop-up menus have
`` `menu*' ``, in like fashion. For example, to specify the font
`` `8x16' `` for the pop-up menu items, write this:

|  |  |
| --- | --- |
|  | ``` Emacs.menu*.font:	8x16 ``` |

For dialog boxes, use `` `dialog' `` instead of `` `menu' ``:

|  |  |
| --- | --- |
|  | ``` Emacs.dialog*.font:	8x16 ``` |

Experience shows that on some systems you may need to add
`` `shell.' `` before the `` `pane.menubar' `` or `` `menu*' ``. On
some other systems, you must not add `` `shell.' ``.

Here is a list of the specific resources for menu bars and pop-up menus:

`font`: Font for menu item text. `foreground`: Color of the foreground. `background`: Color of the background. `buttonForeground`: In the menu bar, the color of the foreground for a selected item. `horizontalSpacing`: Horizontal spacing in pixels between items. Default is 3. `verticalSpacing`: Vertical spacing in pixels between items. Default is 1. `arrowSpacing`: Horizontal spacing between the arrow (which indicates a submenu) and the associated text. Default is 10. `shadowThickness`: Thickness of shadow line around the widget. `margin`: The margin of the menu bar, in characters. The default of 4 makes the menu bar appear like the LessTif/Motif one.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC544) ] | [ [>](emacs.html#SEC546) ] | [ [<<](emacs.html#SEC530) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.15 LessTif Menu X Resources

If the Emacs installed at your site was built to use the X toolkit
with the LessTif or Motif widgets, then the menu bar, the dialog
boxes, the pop-up menus, and the file-selection box are separate
widgets and have their own resources.

The resource names for the menu bar contain `` `pane.menubar' ``
(following, as always, the name of the Emacs invocation, or
`` `Emacs' ``, which stands for all Emacs invocations). Specify them
like this:

|  |  |
| --- | --- |
|  | ``` Emacs.pane.menubar.subwidget.resource:  value ``` |

Each individual string in the menu bar is a subwidget; the subwidget's
name is the same as the menu item string. For example, the word
`` `File' `` in the menu bar is part of a subwidget named
`` `emacs.pane.menubar.File' ``. Most likely, you want to specify the
same resources for the whole menu bar. To do this, use `` `*' `` instead
of a specific subwidget name. For example, to specify the font
`` `8x16' `` for the menu-bar items, write this:

|  |  |
| --- | --- |
|  | ``` Emacs.pane.menubar.*.fontList:  8x16 ``` |

This also specifies the resource value for submenus.

Each item in a submenu in the menu bar also has its own name for X
resources; for example, the `` `File' `` submenu has an item named
`` `Save (current buffer)' ``. A resource specification for a submenu
item looks like this:

|  |  |
| --- | --- |
|  | ``` Emacs.pane.menubar.popup_*.menu.item.resource: value ``` |

For example, here's how to specify the font for the `` `Save (current
buffer)' `` item:

|  |  |
| --- | --- |
|  | ``` Emacs.pane.menubar.popup_*.File.Save (current buffer).fontList: 8x16 ``` |

For an item in a second-level submenu, such as `` `Complete Word' ``
under `` `Spell Checking' `` under `` `Tools' ``, the resource fits this
template:

|  |  |
| --- | --- |
|  | ``` Emacs.pane.menubar.popup_*.popup_*.menu.resource: value ``` |

For example,

|  |  |
| --- | --- |
|  | ``` Emacs.pane.menubar.popup_*.popup_*.Spell Checking.Complete Word: value ``` |

(This should be one long line.)
It's impossible to specify a resource for all the menu-bar items
without also specifying it for the submenus as well. So if you want the
submenu items to look different from the menu bar itself, you must ask
for that in two steps. First, specify the resource for all of them;
then, override the value for submenus alone. Here is an example:

|  |  |
| --- | --- |
|  | ``` Emacs.pane.menubar.*.fontList:  8x16 Emacs.pane.menubar.popup_*.fontList: 8x16 ``` |

For LessTif pop-up menus, use `` `menu*' `` instead of
`` `pane.menubar' ``. For example, to specify the font `` `8x16' `` for
the pop-up menu items, write this:

|  |  |
| --- | --- |
|  | ``` Emacs.menu*.fontList:  8x16 ``` |

For LessTif dialog boxes, use `` `dialog' `` instead of `` `menu' ``:

|  |  |
| --- | --- |
|  | ``` Emacs.dialog*.fontList: 8x16 Emacs.dialog*.foreground: hotpink ``` |

To specify resources for the LessTif file-selection box, use
`` `fsb*' ``, like this:

|  |  |
| --- | --- |
|  | ``` Emacs.fsb*.fontList: 8x16 ``` |

Here is a list of the specific resources for LessTif menu bars and
pop-up menus:

`armColor`: The color to show in an armed button. `fontList`: The font to use. `marginBottom`: `marginHeight`: `marginLeft`: `marginRight`: `marginTop`: `marginWidth`: Amount of space to leave around the item, within the border. `borderWidth`: The width of the border around the menu item, on all sides. `shadowThickness`: The width of the border shadow. `bottomShadowColor`: The color for the border shadow, on the bottom and the right. `topShadowColor`: The color for the border shadow, on the top and the left.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC545) ] | [ [>](emacs.html#SEC547) ] | [ [<<](emacs.html#SEC528) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC547) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# AF. Emacs 20 Antinews

For those users who live backwards in time, here is information about
downgrading to Emacs version 20. We hope you will enjoy the greater
simplicity that results from the absence of many Emacs 21 features.

* The display engine has been greatly simplified by eliminating support
  for variable-size characters and other non-text display features. This
  avoids the complexity of display layout in Emacs 21. To wit:

  + Variable-size characters are not supported in Emacs 20. You cannot use
    fonts which contain oversized characters, and using italic fonts can
    result in illegible display. However, text which uses variable-size
    fonts is unreadable anyway. With all characters in a frame laid out on
    a regular grid, each character having the same height and width, text is
    much easier to read.

    + Emacs does not display images, or play sounds. It just displays text,
      as you would expect from a **text** editor.

      + Specification of the font for a face now uses an XLFD font name, for
        compatibility with other X applications. This means that font
        attributes cannot be merged when combining faces; however, experience
        shows that mergers are bad economics. Face inheritance has also been
        removed, so no one can accumulate "too much face."

        + Several face appearance attributes, including 3D, strike-through, and
          overline, have been eliminated.

          + Emacs now provides its own "lean and mean" scroll bars instead of using
            those from the X toolkit. Toggle buttons and radio buttons in menus now
            look just like any other menu item, which simplifies them, and prevents
            them from standing out and distracting your attention from the other
            menu items.

            + There are no toolbars and no tooltips; in particular, GUD
              mode cannot display variable values in a tooltip when you click on
              that variable's name. Instead, Emacs 20 provides a direct interface to
              the debugger, so that you can type appropriate debugger commands, such
              as `display foo` and `print bar`. As these commands use
              explicit words, their meaning is more self-evident.

              + Colors are not available on text-only terminals. If you *must*
                have colors, but cannot afford to run X, you can now use the MS-DOG
                version of Emacs inside a DOS emulator.

                + The mode line is not mouse-sensitive, since it is meant only to
                  display information. Use keyboard commands to switch between buffers,
                  toggle read-only and modified status, switch minor modes on and off,
                  etc.

                  + The support for "wheeled" mice under X has been removed, because
                    of their slow scroll rate, and because you will find fewer and fewer of
                    these mice as you go back in time. Instead Emacs 20 provides the
                    `C-v` and `M-v` keys for scrolling. (You can also use the
                    scroll bar, but be advised that it, too, may be absent in yet earlier
                    Emacs versions.)

                    + Busy-cursor display is gone, as it was found to be too hard to draw on
                      displays whose resolution is getting lower and lower. This means that
                      you get the standard kind of cursor blinking that your terminal
                      provides.

                      + Some aspects of Emacs appearance, such as the colors of the scroll bar
                        and the menus, can only be controlled via X resources. Since colors
                        aren't supported except on X, it doesn't make any sense to do this in
                        any way but the X way. For those users who aren't privy to X arcana,
                        we've provided good default colors that should make everybody happy.

                        + Emacs 20 adds new lines to the buffer when you move down from the last
                          line with `C-n` or a down-arrow.

                          + The variable `show-trailing-whitespace` has no special meaning, so
                            trailing whitespace on a line is now always displayed correctly: as
                            empty space. To see if a line ends with spaces or tabs, type `C-e`
                            on that line. Likewise, empty lines at the end of the buffer are not
                            marked in any way; use `M->` to see where the end of the buffer is.

                            + The spacing between text lines on the display now always follows the
                              font design and the rules of your window manager. This provides for
                              predictable appearance of the displayed text.

  * Emacs 20 has simpler support for multi-lingual editing. While not as
    radical a simplification as Emacs 19 will be, it goes a long way toward
    eliminating some of the annoying features:

    + Translations of the Emacs reference cards to other languages are no
      longer part of the distribution, because in the past we expect
      computer users to speak English.

      + To avoid extra confusion, many language environments have been
        eliminated. For example, `` `Polish' `` and `` `Celtic' `` (Latin-8)
        environments are not supported. The Latin-9 environment is gone,
        too, because you won't need the Euro sign in the past.

        + Emacs 20 always asks you which coding system to use when saving
          a buffer, unless it can use the same one that it used to read the buffer.
          It does not try to see if the preferred coding system is suitable.

          + Commands which provide detailed information about character sets and
            coding systems, such as `list-charset-chars`,
            `describe-character-set`, and the `C-u C-x =` key-sequence,
            no longer exist. The less said about non-ASCII characters, the
            better.

            + The terminal coding system cannot be set to something CCL-based, so
              keyboards which produce `KOI8` and DOS/Windows codepage codes
              cannot be supported directly. Instead, you should use one of the input
              methods provided in the Leim package.

    * As you move back through time, some systems will become unimportant or
      enter the vaporware phase, so Emacs 20 does not support them:

      + Emacs 20 cannot be built on GNU/Linux systems running on IA64 machines,
        and you cannot build a 64-bit Emacs on Solaris or Irix even though there
        are still 64-bit versions of those OSes.

        + LynxOS is also not supported, and neither is the Macintosh, though they
          still exist.

      * The arrangement of menu bar items differs from most other GUI
        programs. We think that uniformity of look-and-feel is boring, and that
        Emacs' unique features require its unique menu-bar configuration.

        * You cannot save the options that you set from the `` `Options' ``
          menu-bar menu; instead, you need to set all the options again each time
          you start a new session. However, if you follow the recommended
          practice and keep a single Emacs session running until you log out,
          you won't have to set the options very often.

          * Emacs 20 does not pop up a buffer with error messages when an error is
            signaled during loading of the user's init file. Instead, it simply
            announces the fact that an error happened. To know where in the init
            file that was, insert `(message "foo")` lines judiciously into the
            file and look for those messages in the `` `*Messages*' `` buffer.

            * Some commands no longer treat Transient Mark mode specially. For
              example, `ispell` doesn't spell-check the region when Transient
              Mark mode is in effect and the mark is active; instead, it checks the
              current buffer. (Transient Mark mode is alien to the spirit of Emacs,
              so we are planning to remove it altogether in an earlier version.)

              * `C-Down-Mouse-3` does not show what would be in the menu bar
                when the menu bar is not displayed.

                * For uniformity, the `delete` function key in Emacs 20 works exactly like
                  the `DEL` key, on both text-only terminals and window systems--it
                  always deletes backward. This eliminates the inconsistency of Emacs 21,
                  where the key labeled `delete` deletes forward when you are using a
                  window system, and backward on a text-only terminals.

                  * The ability to place backup files in special subdirectories (controlled
                    by `backup-directory-alist`) has been eliminated. This makes
                    finding your backup files much easier: they are always in the same
                    directory as the original files.

                    * Emacs no longer refuses to load Lisp files compiled by incompatible
                      versions of Emacs, which may contain invalid byte-code. Instead,
                      Emacs now dumps core when it encounters such byte-code. However, this
                      is a rare occurrence, and it won't happen at all when all Emacs
                      versions merge together, in the distant past.

                      * The `C-x 5 1` command has been eliminated. If you want to delete
                        all the frames but the current one, delete them one by one instead.

                        * CC Mode now enforces identical values for some customizable options,
                          such as indentation style, for better consistency. In particular, if
                          you select an indentation style for Java, the same style is used
                          for C and C++ buffers as well.

                          * Isearch does not highlight other possible matches; it shows only the
                            current match, to avoid distracting your attention. `Mouse-2` in
                            the echo area during incremental search now signals an error, instead of
                            inserting the current selection into the search string. But you can
                            accomplish more or less the same job by typing `M-y`.

                            * The ability to specify a port number when editing remote files with
                              `ange-ftp` was removed. Instead, Emacs 20 provides undocumented
                              features in the function `ange-ftp-normal-login` (Use the
                              source, Luke!) to specify the port.

                              * Emacs 20 does not check for changing time stamps of remote files, since
                                the old FTP programs you will encounter in the past could not provide
                                the time stamp anyway. Windows-style FTP clients which output the
                                `` `^M' `` character at the end of each line get special handling from
                                `ange-ftp` in Emacs 20, with unexpected results that should make
                                your life more interesting.

                                * Many complicated display features, including highlighting of
                                  mouse-sensitive text regions and popping up help strings for menu items,
                                  don't work in the MS-DOS version. Spelling doesn't work on MS-DOS,
                                  and Eshell doesn't exist, so there's no workable shell-mode, either.
                                  This fits the spirit of MS-DOS, which resembles a dumb character
                                  terminal.

                                  * The `woman` package has been removed, so Emacs users on non-Posix
                                    systems will need *a real man* to read manual pages. (Users who
                                    are not macho can read the Info documentation instead.)

                                    * `recentf` has been removed, because we figure that you can remember
                                      the names of the files you edit frequently. With decreasing disk size,
                                      you should have fewer files anyway, so you won't notice the absence of
                                      this feature.

                                      * The `field` property does not exist in Emacs 20, so various
                                        packages that run subsidiary programs in Emacs buffers cannot in general
                                        distinguish which text was user input and which was output from the
                                        subprocess. If you need to try to do this nonetheless, Emacs 20
                                        provides a variable `comint-prompt-regexp`, which lets you try to
                                        distinguish input by recognizing prompt strings.

                                        * We have eliminated the special major modes for Delphi sources,
                                          PostScript files, context diffs, and `TODO' files. Use Fundamental
                                          Mode instead.

                                          * Many additional packages that unnecessarily complicate your life in
                                            Emacs 21 are absent in Emacs 20. You cannot browse C++ classes with
                                            Ebrowse, access SQL data bases, access LDAP and
                                            other directory servers, or mix shell commands and Lisp functions using
                                            Eshell.

                                            * To keep up with decreasing computer memory capacity and disk space, many
                                              other functions and files have been eliminated in Emacs 20.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC546) ] | [ [>](emacs.html#SEC548) ] | [ [<<](emacs.html#SEC546) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC554) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# AG. Emacs and the Mac OS

Emacs built on the Mac OS supports many of its major features:
multiple frames, colors, scroll bars, menu bars, use of the mouse,
fontsets, international characters, input methods, coding systems, and
synchronous subprocesses (`call-process`). Much of this works in
the same way as on other platforms and is therefore documented in the
rest of this manual. This section describes the peculiarities of using
Emacs under the Mac OS.

The following features of Emacs are not yet supported on the Mac:
unexec (`dump-emacs`), asynchronous subprocesses
(`start-process`), and networking (`open-network-stream`).
As a result, packages such as Gnus, GUD, and Comint do not work.

Since external programs to handle commands such as
`print-buffer` and `diff` are not available on the Mac OS,
they are not supported in the Mac OS version.

> |  |  |  |
> | --- | --- | --- |
> | [AG.1 Keyboard Input on the Mac](emacs.html#SEC548) |  | Keyboard input on the Mac. |
> | [AG.2 International Character Set Support on the Mac](emacs.html#SEC549) |  | International character sets on the Mac. |
> | [AG.3 Environment Variables and Command Line Arguments.](emacs.html#SEC550) |  | Setting environment variables for Emacs. |
> | [AG.4 Volumes and Directories on the Mac](emacs.html#SEC551) |  | Volumes and directories on the Mac. |
> | [AG.5 Specifying Fonts on the Mac](emacs.html#SEC552) |  | Specifying fonts on the Mac. |
> | [AG.6 Mac-Specific Lisp Functions](emacs.html#SEC553) |  | Mac-specific Lisp functions. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC547) ] | [ [>](emacs.html#SEC549) ] | [ [<<](emacs.html#SEC547) ] | [ [Up](emacs.html#SEC547) ] | [ [>>](emacs.html#SEC554) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AG.1 Keyboard Input on the Mac

On the Mac, Emacs can use either the `option` key or the
`command` key as the `META` key. If the value of the variable
`mac-command-key-is-meta` is non-`nil` (its default value),
Emacs uses the `command` key as the `META` key. Otherwise it
uses the `option` key as the `META` key.

Most people should want to use the `command` key as the `META` key,
so that dead-key processing with the `option` key will still work. This is
useful for entering non-ASCII Latin characters directly from the Mac
keyboard, for example.

Emacs recognizes the setting in the Keyboard control panel and
supports international and alternative keyboard layouts (e.g., Dvorak).
Selecting one of the layouts from the keyboard layout pull-down menu
will affect how the keys typed on the keyboard are interpreted.

The Mac OS intercepts and handles certain key combinations (e.g.,
`command`-`SPC` for switching input languages). These will not
be passed to Emacs.

The Mac keyboard ordinarily generates characters in the Mac Roman
encoding. To use it for entering ISO Latin-1 characters directly, set
the value of the variable `mac-keyboard-text-encoding` to
`kTextEncodingISOLatin1`. Note that not all Mac Roman characters
that can be entered at the keyboard can be converted to ISO Latin-1
characters.

To enter ISO Latin-2 characters directly from the Mac keyboard, set
the value of `mac-keyboard-text-encoding` to
`kTextEncodingISOLatin2`. Then let Emacs know that the keyboard
generates Latin-2 codes, by typing `C-x RET k iso-latin-2
RET`. To make this setting permanent, put this in your
`.emacs' init file:

|  |  |
| --- | --- |
|  | ``` (set-keyboard-coding-system 'iso-latin-2) ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC548) ] | [ [>](emacs.html#SEC550) ] | [ [<<](emacs.html#SEC550) ] | [ [Up](emacs.html#SEC547) ] | [ [>>](emacs.html#SEC554) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AG.2 International Character Set Support on the Mac

The Mac uses a non-standard encoding for the upper 128 single-byte
characters. It also deviates from the ISO 2022 standard by using
character codes in the range 128-159. The coding system
`mac-roman` is used to represent this Mac encoding. It is used
for editing files stored in this native encoding, and for displaying
file names in Dired mode.

Any native (non-symbol) Mac font can be used to correctly display
characters in the `mac-roman` coding system.

The fontset `fontset-mac` is created automatically when Emacs
is run on the Mac. It displays characters in the `mac-roman`
coding system using 12-point Monaco.

To insert characters directly in the `mac-roman` coding system,
type `C-x RET k mac-roman RET`, customize the option
`keyboard-coding-system`, or put this in your init file:

|  |  |
| --- | --- |
|  | ``` (set-keyboard-coding-system 'mac-roman) ``` |

This is useful for editing documents in native Mac encoding.

You can use input methods provided either by LEIM (see section [Q.4 Input Methods](emacs.html#SEC217)) or the Mac OS to enter international characters.

To use the former, see the International Character Set Support section
of the manual (see section [Q. International Character Set Support](emacs.html#SEC213)).

To use input methods provided by the Mac OS, set the keyboard coding
system accordingly using the `C-x RET k` command
(`set-keyboard-coding-system`). For example, for Traditional
Chinese, use `` `chinese-big5' `` as keyboard coding system; for
Japanese, use `` `sjis' ``, etc. Then select the desired input method in
the keyboard layout pull-down menu.

The Mac clipboard and the Emacs kill ring (see section [H.7 Deletion and Killing](emacs.html#SEC59)) are
connected as follows: the most recent kill is copied to the clipboard
when Emacs is suspended and the contents of the clipboard is inserted
into the kill ring when Emacs resumes. The result is that you can yank
a piece of text and paste it into another Mac application, or cut or copy
one in another Mac application and yank it into a Emacs buffer.

The encoding of text selections must be specified using the commands
`C-x RET x` (`set-selection-coding-system`) or `C-x
RET X` (`set-next-selection-coding-system`) (e.g., for
Traditional Chinese, use `` `chinese-big5-mac' `` and for Japanese,
`` `sjis-mac' ``). See section [Q.9 Specifying a Coding System](emacs.html#SEC222), for more details.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC549) ] | [ [>](emacs.html#SEC551) ] | [ [<<](emacs.html#SEC551) ] | [ [Up](emacs.html#SEC547) ] | [ [>>](emacs.html#SEC554) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AG.3 Environment Variables and Command Line Arguments.

Environment variables and command line arguments for Emacs can be set
by modifying the `` `STR#' `` resources 128 and 129, respectively. A common
environment variable that one may want to set is `` `HOME' ``.

The way to set an environment variable is by adding a string of the
form

|  |  |
| --- | --- |
|  | ``` ENV_VAR=VALUE ``` |

to resource `` `STR#' `` number 128 using `ResEdit`. To set up the
program to use unibyte characters exclusively, for example, add the
string

|  |  |
| --- | --- |
|  | ``` EMACS_UNIBYTE=1 ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC550) ] | [ [>](emacs.html#SEC552) ] | [ [<<](emacs.html#SEC552) ] | [ [Up](emacs.html#SEC547) ] | [ [>>](emacs.html#SEC554) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AG.4 Volumes and Directories on the Mac

The directory structure in the Mac OS is seen by Emacs as

|  |  |
| --- | --- |
|  | ``` /volumename/filename ``` |

So when Emacs requests a file name, doing file name completion on
`/' will display all volumes on the system. You can use `..'
to go up a directory level.

To access files and folders on the desktop, look in the folder
`Desktop Folder' in your boot volume (this folder is usually
invisible in the Mac `Finder`).

Emacs creates the Mac folder `:Preferences:Emacs:' in the
`System Folder' and uses it as the temporary directory. Emacs
maps the directory name `/tmp/' to that. Therefore it
is best to avoid naming a volume `tmp'. If everything works
correctly, the program should leave no files in it when it exits. You
should be able to set the environment variable `TMPDIR` to use
another directory but this folder will still be created.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC551) ] | [ [>](emacs.html#SEC553) ] | [ [<<](emacs.html#SEC553) ] | [ [Up](emacs.html#SEC547) ] | [ [>>](emacs.html#SEC554) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AG.5 Specifying Fonts on the Mac

It is rare that you need to specify a font name in Emacs; usually
you specify face attributes instead. But when you do need to specify
a font name in Emacs on the Mac, use a standard X font name:

|  |  |
| --- | --- |
|  | ``` -maker-family-weight-slant-widthtype-style... ...-pixels-height-horiz-vert-spacing-width-charset ``` |

See section [AE.7 Font Specification Options](emacs.html#SEC537). Wildcards are supported as they are on X.

Native Apple fonts in Mac Roman encoding has maker name `apple`
and charset `mac-roman`. For example 12-point Monaco can be
specified by the name `` `-apple-monaco-*-12-*-mac-roman' ``.

Native Apple Traditional Chinese, Simplified Chinese, Japanese, and
Korean fonts have charsets `` `big5-0' ``, `` `gb2312.1980-0' ``,
`` `jisx0208.1983-sjis' ``, and `` `ksc5601.1989-0' ``, respectively.

Single-byte fonts converted from GNU fonts in BDF format, which are not
in the Mac Roman encoding, have foundry, family, and character sets
encoded in the names of their font suitcases. E.g., the font suitcase
`` `ETL-Fixed-ISO8859-1' `` contains fonts which can be referred to by
the name `` `-ETL-fixed-*-iso8859-1' ``.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC552) ] | [ [>](emacs.html#SEC554) ] | [ [<<](emacs.html#SEC547) ] | [ [Up](emacs.html#SEC547) ] | [ [>>](emacs.html#SEC554) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AG.6 Mac-Specific Lisp Functions

The function `do-applescript` takes a string argument,
executes it as an AppleScript command, and returns the result as a
string.

The function `mac-file-name-to-posix` takes a Mac file name and
returns the GNU or Unix equivalent. The function
`posix-file-name-to-mac` performs the opposite conversion. They
are useful for constructing AppleScript commands to be passed to
`do-applescript`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC553) ] | [ [>](emacs.html#SEC555) ] | [ [<<](emacs.html#SEC547) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC564) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# AH. Emacs and MS-DOS

This section briefly describes the peculiarities of using Emacs under
the MS-DOS "operating system" (also known as "MS-DOG"). If you
build Emacs for MS-DOS, the binary will also run on Windows 3.X, Windows
NT, Windows 9X/ME, Windows 2000, or OS/2 as a DOS application; the
information in this chapter applies for all of those systems, if you use
an Emacs that was built for MS-DOS.

Note that it is possible to build Emacs specifically for Windows NT/2K
or Windows 9X/ME. If you do that, most of this chapter does not apply;
instead, you get behavior much closer to what is documented in the rest
of the manual, including support for long file names, multiple frames,
scroll bars, mouse menus, and subprocesses. However, the section on
text files and binary files does still apply. There are also two
sections at the end of this chapter which apply specifically for the
Windows version.

> |  |  |  |
> | --- | --- | --- |
> | [AH.1 Keyboard and Mouse on MS-DOS](emacs.html#SEC555) |  | Keyboard and mouse usage on MS-DOS. |
> | [AH.2 Display on MS-DOS](emacs.html#SEC556) |  | Fonts, frames and display size on MS-DOS. |
> | [AH.3 File Names on MS-DOS](emacs.html#SEC557) |  | File name conventions on MS-DOS. |
> | [AH.4 Text Files and Binary Files](emacs.html#SEC558) |  | Text files on MS-DOS use CRLF to separate lines. |
> | [AH.5 Printing and MS-DOS](emacs.html#SEC559) |  | How to specify the printer on MS-DOS. |
> | [AH.6 International Support on MS-DOS](emacs.html#SEC560) |  | Support for internationalization on MS-DOS. |
> | [AH.7 Subprocesses on MS-DOS](emacs.html#SEC561) |  | Running subprocesses on MS-DOS. |
> | [AH.8 Subprocesses on Windows 9X/ME and Windows NT/2K](emacs.html#SEC562) |  | Running subprocesses on Windows. |
> | [AH.9 Using the System Menu on Windows](emacs.html#SEC563) |  | Controlling what the ALT key does. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC554) ] | [ [>](emacs.html#SEC556) ] | [ [<<](emacs.html#SEC554) ] | [ [Up](emacs.html#SEC554) ] | [ [>>](emacs.html#SEC564) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AH.1 Keyboard and Mouse on MS-DOS

The PC keyboard maps use the left `ALT` key as the `META` key.
You have two choices for emulating the `SUPER` and `HYPER` keys:
choose either the right `CTRL` key or the right `ALT` key by
setting the variables `dos-hyper-key` and `dos-super-key` to 1
or 2 respectively. If neither `dos-super-key` nor
`dos-hyper-key` is 1, then by default the right `ALT` key is
also mapped to the `META` key. However, if the MS-DOS international
keyboard support program `KEYB.COM' is installed, Emacs will
*not* map the right `ALT` to `META`, since it is used for
accessing characters like `~` and `@` on non-US keyboard
layouts; in this case, you may only use the left `ALT` as `META`
key.

The variable `dos-keypad-mode` is a flag variable that controls
what key codes are returned by keys in the numeric keypad. You can also
define the keypad `ENTER` key to act like `C-j`, by putting the
following line into your `\_emacs' file:

|  |  |
| --- | --- |
|  | ``` ;; Make the ENTER key from the numeric keypad act as C-j. (define-key function-key-map [kp-enter] [?\C-j]) ``` |

The key that is called `DEL` in Emacs (because that's how it is
designated on most workstations) is known as `BS` (backspace) on a
PC. That is why the PC-specific terminal initialization remaps the
`BS` key to act as `DEL`; the `DEL` key is remapped to act
as `C-d` for the same reasons.

Emacs built for MS-DOS recognizes `C-BREAK` as a quit
character, just like `C-g`. This is because Emacs cannot detect
that you have typed `C-g` until it is ready for more input. As a
consequence, you cannot use `C-g` to stop a running command
(see section [AD.8 Quitting and Aborting](emacs.html#SEC510)). By contrast, `C-BREAK` *is* detected
as soon as you type it (as `C-g` is on other systems), so it can be
used to stop a running command and for emergency escape
(see section [AD.9.8 Emergency Escape](emacs.html#SEC519)).

Emacs on MS-DOS supports a mouse (on the default terminal only).
The mouse commands work as documented, including those that use menus
and the menu bar (see section [B.4 The Menu Bar](emacs.html#SEC12)). Scroll bars don't work in
MS-DOS Emacs. PC mice usually have only two buttons; these act as
`Mouse-1` and `Mouse-2`, but if you press both of them
together, that has the effect of `Mouse-3`. If the mouse does have
3 buttons, Emacs detects that at startup, and all the 3 buttons function
normally, as on X.

Help strings for menu-bar and pop-up menus are displayed in the echo
area when the mouse pointer moves across the menu items.
Highlighting of mouse-sensitive text (see section [P.4 Following References with the Mouse](emacs.html#SEC195)) is also
supported.

Some versions of mouse drivers don't report the number of mouse
buttons correctly. For example, mice with a wheel report that they
have 3 buttons, but only 2 of them are passed to Emacs; the clicks on
the wheel, which serves as the middle button, are not passed. In
these cases, you can use the `M-x msdos-set-mouse-buttons` command
to tell Emacs how many mouse buttons to expect. You could make such a
setting permanent by adding this fragment to your `\_emacs' init
file:

|  |  |
| --- | --- |
|  | ``` ;; Treat the mouse like a 2-button mouse. (msdos-set-mouse-buttons 2) ``` |

Emacs built for MS-DOS supports clipboard operations when it runs on
Windows. Commands that put text on the kill ring, or yank text from the
ring, check the Windows clipboard first, just as Emacs does on the X
Window System (see section [P.1 Mouse Commands for Editing](emacs.html#SEC192)). Only the primary selection and
the cut buffer are supported by MS-DOS Emacs on Windows; the secondary
selection always appears as empty.

Due to the way clipboard access is implemented by Windows, the
length of text you can put into the clipboard is limited by the amount
of free DOS memory that is available to Emacs. Usually, up to 620KB of
text can be put into the clipboard, but this limit depends on the system
configuration and is lower if you run Emacs as a subprocess of
another program. If the killed text does not fit, Emacs outputs a
message saying so, and does not put the text into the clipboard.

Null characters also cannot be put into the Windows clipboard. If the
killed text includes null characters, Emacs does not put such text into
the clipboard, and displays in the echo area a message to that effect.

The variable `dos-display-scancodes`, when non-`nil`,
directs Emacs to display the ASCII value and the keyboard scan code of
each keystroke; this feature serves as a complement to the
`view-lossage` command, for debugging.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC555) ] | [ [>](emacs.html#SEC557) ] | [ [<<](emacs.html#SEC557) ] | [ [Up](emacs.html#SEC554) ] | [ [>>](emacs.html#SEC564) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AH.2 Display on MS-DOS

Display on MS-DOS cannot use font variants, like bold or italic,
but it does support
multiple faces, each of which can specify a foreground and a background
color. Therefore, you can get the full functionality of Emacs packages
that use fonts (such as `font-lock`, Enriched Text mode, and
others) by defining the relevant faces to use different colors. Use the
`list-colors-display` command (see section [P.12 Setting Frame Parameters](emacs.html#SEC203)) and the
`list-faces-display` command (see section [J.1 Using Multiple Typefaces](emacs.html#SEC78)) to see what colors and
faces are available and what they look like.

See section [AH.6 International Support on MS-DOS](emacs.html#SEC560), later in this chapter, for information on
how Emacs displays glyphs and characters that aren't supported by the
native font built into the DOS display.

When Emacs starts, it changes the cursor shape to a solid box. This
is for compatibility with other systems, where the box cursor is the
default in Emacs. This default shape can be changed to a bar by
specifying the `cursor-type` parameter in the variable
`default-frame-alist` (see section [P.7 Creating Frames](emacs.html#SEC198)). The MS-DOS
terminal doesn't support a vertical-bar cursor, so the bar cursor is
horizontal, and the `width` parameter, if specified by the
frame parameters, actually determines its height. As an extension,
the bar cursor specification can include the starting scan line of the
cursor as well as its width, like this:

|  |  |
| --- | --- |
|  | ```  '(cursor-type bar width . start) ``` |

In addition, if the width parameter is negative, the cursor bar
begins at the top of the character cell.

The MS-DOS terminal can only display a single frame at a time. The
Emacs frame facilities work on MS-DOS much as they do on text-only
terminals (see section [P. Frames and X Windows](emacs.html#SEC191)). When you run Emacs from a DOS window on
MS-Windows, you can make the visible frame smaller than the full
screen, but Emacs still cannot display more than a single frame at a
time.

The `mode4350` command switches the display to 43 or 50
lines, depending on your hardware; the `mode25` command switches
to the default 80x25 screen size.

By default, Emacs only knows how to set screen sizes of 80 columns by
25, 28, 35, 40, 43 or 50 rows. However, if your video adapter has
special video modes that will switch the display to other sizes, you can
have Emacs support those too. When you ask Emacs to switch the frame to
n rows by m columns dimensions, it checks if there is a
variable called `screen-dimensions-nxm`, and if so,
uses its value (which must be an integer) as the video mode to switch
to. (Emacs switches to that video mode by calling the BIOS `Set
Video Mode` function with the value of
`screen-dimensions-nxm` in the `AL` register.)
For example, suppose your adapter will switch to 66x80 dimensions when
put into video mode 85. Then you can make Emacs support this screen
size by putting the following into your `\_emacs' file:

|  |  |
| --- | --- |
|  | ``` (setq screen-dimensions-66x80 85) ``` |

Since Emacs on MS-DOS can only set the frame size to specific
supported dimensions, it cannot honor every possible frame resizing
request. When an unsupported size is requested, Emacs chooses the next
larger supported size beyond the specified size. For example, if you
ask for 36x80 frame, you will get 40x80 instead.

The variables `screen-dimensions-nxm` are used only
when they exactly match the specified size; the search for the next
larger supported size ignores them. In the above example, even if your
VGA supports 38x80 dimensions and you define a variable
`screen-dimensions-38x80` with a suitable value, you will still get
40x80 screen when you ask for a 36x80 frame. If you want to get the
38x80 size in this case, you can do it by setting the variable named
`screen-dimensions-36x80` with the same video mode value as
`screen-dimensions-38x80`.

Changing frame dimensions on MS-DOS has the effect of changing all the
other frames to the new dimensions.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC556) ] | [ [>](emacs.html#SEC558) ] | [ [<<](emacs.html#SEC558) ] | [ [Up](emacs.html#SEC554) ] | [ [>>](emacs.html#SEC564) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AH.3 File Names on MS-DOS

MS-DOS normally uses a backslash, `` `\' ``, to separate name units
within a file name, instead of the slash used on other systems. Emacs
on MS-DOS permits use of either slash or backslash, and also knows
about drive letters in file names.

On MS-DOS, file names are case-insensitive and limited to eight
characters, plus optionally a period and three more characters. Emacs
knows enough about these limitations to handle file names that were
meant for other operating systems. For instance, leading dots `` `.' ``
in file names are invalid in MS-DOS, so Emacs transparently converts
them to underscores `` `_' ``; thus your default init file (see section [AD.7 The Init File, `~/.emacs'](emacs.html#SEC505)) is called `\_emacs' on MS-DOS. Excess characters before or
after the period are generally ignored by MS-DOS itself; thus, if you
visit the file `LongFileName.EvenLongerExtension', you will
silently get `longfile.eve', but Emacs will still display the long
file name on the mode line. Other than that, it's up to you to specify
file names which are valid under MS-DOS; the transparent conversion as
described above only works on file names built into Emacs.

The above restrictions on the file names on MS-DOS make it almost
impossible to construct the name of a backup file (see section [M.3.1.1 Single or Numbered Backups](emacs.html#SEC115)) without losing some of the original file name characters. For
example, the name of a backup file for `docs.txt' is
`docs.tx~' even if single backup is used.

If you run Emacs as a DOS application under Windows 9X, Windows ME, or
Windows 2000, you can turn on support for long file names. If you do
that, Emacs doesn't truncate file names or convert them to lower case;
instead, it uses the file names that you specify, verbatim. To enable
long file name support, set the environment variable `LFN` to
`` `y' `` before starting Emacs. Unfortunately, Windows NT doesn't allow
DOS programs to access long file names, so Emacs built for MS-DOS will
only see their short 8+3 aliases.

MS-DOS has no notion of home directory, so Emacs on MS-DOS pretends
that the directory where it is installed is the value of `HOME`
environment variable. That is, if your Emacs binary,
`emacs.exe', is in the directory `c:/utils/emacs/bin', then
Emacs acts as if `HOME` were set to `` `c:/utils/emacs' ``. In
particular, that is where Emacs looks for the init file `\_emacs'.
With this in mind, you can use `` `~' `` in file names as an alias for
the home directory, as you would on GNU or Unix. You can also set
`HOME` variable in the environment before starting Emacs; its
value will then override the above default behavior.

Emacs on MS-DOS handles the directory name `/dev' specially,
because of a feature in the emulator libraries of DJGPP that pretends
I/O devices have names in that directory. We recommend that you avoid
using an actual directory named `/dev' on any disk.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC557) ] | [ [>](emacs.html#SEC559) ] | [ [<<](emacs.html#SEC559) ] | [ [Up](emacs.html#SEC554) ] | [ [>>](emacs.html#SEC564) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AH.4 Text Files and Binary Files

GNU Emacs uses newline characters to separate text lines. This is the
convention used on GNU and Unix.

MS-DOS and MS-Windows normally use carriage-return linefeed, a
two-character sequence, to separate text lines. (Linefeed is the same
character as newline.) Therefore, convenient editing of typical files
with Emacs requires conversion of these end-of-line (EOL) sequences.
And that is what Emacs normally does: it converts carriage-return
linefeed into newline when reading files, and converts newline into
carriage-return linefeed when writing files. The same mechanism that
handles conversion of international character codes does this conversion
also (see section [Q.7 Coding Systems](emacs.html#SEC220)).

One consequence of this special format-conversion of most files is
that character positions as reported by Emacs (see section [D.9 Cursor Position Information](emacs.html#SEC28)) do
not agree with the file size information known to the operating system.

In addition, if Emacs recognizes from a file's contents that it uses
newline rather than carriage-return linefeed as its line separator, it
does not perform EOL conversion when reading or writing that file.
Thus, you can read and edit files from GNU and Unix systems on MS-DOS
with no special effort, and they will retain their Unix-style
end-of-line convention after you edit them.

The mode line indicates whether end-of-line translation was used for
the current buffer. If MS-DOS end-of-line translation is in use for the
buffer, a backslash `` `\' `` is displayed after the coding system
mnemonic near the beginning of the mode line (see section [B.3 The Mode Line](emacs.html#SEC11)). If no
EOL translation was performed, the string `` `(Unix)' `` is displayed
instead of the backslash, to alert you that the file's EOL format is not
the usual carriage-return linefeed.

To visit a file and specify whether it uses DOS-style or Unix-style
end-of-line, specify a coding system (see section [Q.9 Specifying a Coding System](emacs.html#SEC222)). For
example, `C-x RET c unix RET C-x C-f foobar.txt`
visits the file `foobar.txt' without converting the EOLs; if some
line ends with a carriage-return linefeed pair, Emacs will display
`` `^M' `` at the end of that line. Similarly, you can direct Emacs to
save a buffer in a specified EOL format with the `C-x RET f`
command. For example, to save a buffer with Unix EOL format, type
`C-x RET f unix RET C-x C-s`. If you visit a file
with DOS EOL conversion, then save it with Unix EOL format, that
effectively converts the file to Unix EOL style, like `dos2unix`.

When you use NFS or Samba to access file systems that reside on
computers using GNU or Unix systems, Emacs should not perform
end-of-line translation on any files in these file systems--not even
when you create a new file. To request this, designate these file
systems as *untranslated* file systems by calling the function
`add-untranslated-filesystem`. It takes one argument: the file
system name, including a drive letter and optionally a directory. For
example,

|  |  |
| --- | --- |
|  | ``` (add-untranslated-filesystem "Z:") ``` |

designates drive Z as an untranslated file system, and

|  |  |
| --- | --- |
|  | ``` (add-untranslated-filesystem "Z:\\foo") ``` |

designates directory `\foo' on drive Z as an untranslated file
system.

Most often you would use `add-untranslated-filesystem` in your
`\_emacs' file, or in `site-start.el' so that all the users at
your site get the benefit of it.

To countermand the effect of `add-untranslated-filesystem`, use
the function `remove-untranslated-filesystem`. This function takes
one argument, which should be a string just like the one that was used
previously with `add-untranslated-filesystem`.

Designating a file system as untranslated does not affect character
set conversion, only end-of-line conversion. Essentially, it directs
Emacs to create new files with the Unix-style convention of using
newline at the end of a line. See section [Q.7 Coding Systems](emacs.html#SEC220).
