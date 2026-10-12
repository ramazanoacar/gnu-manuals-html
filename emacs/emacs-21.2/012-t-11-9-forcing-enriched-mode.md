### T.11.9 Forcing Enriched Mode

Normally, Emacs knows when you are editing formatted text because it
recognizes the special annotations used in the file that you visited.
However, there are situations in which you must take special actions
to convert file contents or turn on Enriched mode:

* When you visit a file that was created with some other editor, Emacs may
  not recognize the file as being in the text/enriched format. In this
  case, when you visit the file you will see the formatting commands
  rather than the formatted text. Type `M-x format-decode-buffer` to
  translate it.

  * When you *insert* a file into a buffer, rather than visiting it.
    Emacs does the necessary conversions on the text which you insert, but
    it does not enable Enriched mode. If you wish to do that, type `M-x
    enriched-mode`.

The command `format-decode-buffer` translates text in various
formats into Emacs's internal format. It asks you to specify the format
to translate from; however, normally you can type just `RET`, which
tells Emacs to guess the format.

If you wish to look at text/enriched file in its raw form, as a
sequence of characters rather than as formatted text, use the `M-x
find-file-literally` command. This visits a file, like
`find-file`, but does not do format conversion. It also inhibits
character code conversion (see section [Q.7 Coding Systems](emacs.html#SEC220)) and automatic
uncompression (see section [M.11 Accessing Compressed Files](emacs.html#SEC166)). To disable format conversion
but allow character code conversion and/or automatic uncompression if
appropriate, use `format-find-file` with suitable arguments.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC267) ] | [ [>](emacs.html#SEC269) ] | [ [<<](emacs.html#SEC233) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC316) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# U. Editing Programs

Emacs provides many features to facilitate editing programs. Some
of these features can

* Find or move over top-level definitions (see section [U.2 Top-Level Definitions, or Defuns](emacs.html#SEC270)).* Apply the usual indentation conventions of the language
    (see section [U.3 Indentation for Programs](emacs.html#SEC275)).* Insert, kill or align comments (see section [U.5 Manipulating Comments](emacs.html#SEC285)).* Balance parentheses (see section [U.4 Commands for Editing with Parentheses](emacs.html#SEC281)).* Highlight program syntax (see section [J.2 Font Lock mode](emacs.html#SEC79)).

This chapter describes these features and many more.

> |  |  |  |
> | --- | --- | --- |
> | [U.1 Major Modes for Programming Languages](emacs.html#SEC269) |  | Major modes for editing programs. |
> | [U.2 Top-Level Definitions, or Defuns](emacs.html#SEC270) |  | Commands to operate on major top-level parts of a program. |
> | [U.3 Indentation for Programs](emacs.html#SEC275) |  | Adjusting indentation to show the nesting. |
> | [U.5 Manipulating Comments](emacs.html#SEC285) |  | Inserting, killing, and aligning comments. |
> | [U.4 Commands for Editing with Parentheses](emacs.html#SEC281) |  | Commands that operate on parentheses. |
> | [U.6 Documentation Lookup](emacs.html#SEC289) |  | Getting documentation of functions you plan to call. |
> | [U.7 Hideshow minor mode](emacs.html#SEC293) |  | Displaying blocks selectively. |
> | [U.8 Completion for Symbol Names](emacs.html#SEC294) |  | Completion on symbol names of your program or language. |
> | [U.9 Glasses minor mode](emacs.html#SEC295) |  | Making identifiersLikeThis more readable. |
> | [U.10 Other Features Useful for Editing Programs](emacs.html#SEC296) |  | Other Emacs features useful for editing programs. |
> | [U.11 C and Related Modes](emacs.html#SEC297) |  | Special commands of C, C++, Objective-C, Java, and Pike modes. |
> | [U.12 Fortran Mode](emacs.html#SEC303) |  | Fortran mode and its special features. |
> | [U.13 Asm Mode](emacs.html#SEC315) |  | Asm mode and its special features. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC268) ] | [ [>](emacs.html#SEC270) ] | [ [<<](emacs.html#SEC268) ] | [ [Up](emacs.html#SEC268) ] | [ [>>](emacs.html#SEC316) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## U.1 Major Modes for Programming Languages

Emacs has specialized major modes for various programming languages.
See section [R. Major Modes](emacs.html#SEC227). A programming language major mode typically
specifies the syntax of expressions, the customary rules for
indentation, how to do syntax highlighting for the language, and how
to find the beginning of a function definition. It often customizes
or provides facilities for compiling and debugging programs as well.

Ideally, Emacs should provide a major mode for each programming
language that you might want to edit; if it doesn't have a mode for
your favorite language, you can contribute one. But often the mode
for one language can serve for other syntactically similar languages.
The major mode for language l is called `l-mode`,
and you can select it by typing `M-x l-mode RET`.
See section [R.1 How Major Modes are Chosen](emacs.html#SEC228).

The existing programming language major modes include Lisp, Scheme (a
variant of Lisp) and the Scheme-based DSSSL expression language, Ada,
Awk, C, C++, Delphi (Object Pascal), Fortran (free format and fixed
format), Icon, IDL (CORBA), IDLWAVE, Java, Metafont (TeX's
companion for font creation), Modula2, Objective-C, Octave, Pascal,
Perl, Pike, PostScript, Prolog, Simula, Tcl, and VHDL. There is
also a major mode for makefiles, called Makefile mode. An alternative
mode for Perl is called CPerl mode. Modes are available for the
scripting languages of the common GNU and Unix shells, VMS DCL, and
MS-DOS/MS-Windows `` `BAT' `` files. There are also major modes for
editing various sorts of configuration files.

In most programming languages, indentation should vary from line to
line to illustrate the structure of the program. So the major modes
for programming languages arrange for `TAB` to update the
indentation of the current line. They also rebind `DEL` to treat
a tab as if it were the equivalent number of spaces; this lets you
delete one column of indentation without worrying whether the
whitespace consists of spaces or tabs. Use `C-b C-d` to delete a
tab character before point, in these modes.

Separate manuals are available for the modes for Ada (see section `Ada Mode' in Ada Mode), C/C++/Objective C/Java/Corba IDL
(see section `CC Mode' in CC Mode) and the IDLWAVE modes
(see section `IDLWAVE' in IDLWAVE User Manual).

Turning on a major mode runs a normal hook called the *mode
hook*, which is the value of a Lisp variable. Each major mode has a
mode hook, and the hook's name is always made from the mode command's
name by adding `` `-hook' ``. For example, turning on C mode runs the
hook `c-mode-hook`, while turning on Lisp mode runs the hook
`lisp-mode-hook`. The purpose of the mode hook is to give you a
place to set up customizations for that major mode. See section [AD.2.3 Hooks](emacs.html#SEC484).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC269) ] | [ [>](emacs.html#SEC271) ] | [ [<<](emacs.html#SEC275) ] | [ [Up](emacs.html#SEC268) ] | [ [>>](emacs.html#SEC275) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## U.2 Top-Level Definitions, or Defuns

In Emacs, a major definition at the top level in the buffer is
called a *defun*. The name comes from Lisp, but in Emacs we use
it for all languages.

In most programming language modes, Emacs assumes that a defun is
any pair of parentheses (or braces, if the language uses braces this
way) that starts at the left margin. For example, in C, the body of a
function definition is normally a defun, because the open-brace that
begins it is normally at the left margin. A variable's initializer
can also count as a defun, if the open-brace that begins the
initializer is at the left margin.

However, some language modes provide their own code for recognizing
defuns in a way that suits the language syntax and conventions better.

> |  |  |  |
> | --- | --- | --- |
> | [U.2.1 Left Margin Convention](emacs.html#SEC271) |  | An open-paren or similar opening delimiter starts a defun if it is at the left margin. |
> | [U.2.2 Moving by Defuns](emacs.html#SEC272) |  | Commands to move over or mark a major definition. |
> | [U.2.3 Imenu](emacs.html#SEC273) |  | Making buffer indexes as menus. |
> | [U.2.4 Which Function Mode](emacs.html#SEC274) |  | Which Function mode shows which function you are in. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC270) ] | [ [>](emacs.html#SEC272) ] | [ [<<](emacs.html#SEC275) ] | [ [Up](emacs.html#SEC270) ] | [ [>>](emacs.html#SEC275) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.2.1 Left Margin Convention

In most major modes, Emacs assumes that any opening delimiter found
at the left margin is the start of a top-level definition, or defun.
Therefore, **never put an opening delimiter at the left margin
unless it should have that significance.** For instance, never put an
open-parenthesis at the left margin in a Lisp file unless it is the
start of a top-level list. Never put an open-brace or other opening
delimiter at the beginning of a line of C code unless it is at top
level.

If you don't follow this convention, not only will you have trouble
when you explicitly use the commands for motion by defuns; other
features that use them will also give you trouble. This includes
the indentation commands (see section [U.3 Indentation for Programs](emacs.html#SEC275)) and Font Lock
mode (see section [J.2 Font Lock mode](emacs.html#SEC79)).

The most likely problem case is when you want an opening delimiter
at the start of a line inside a string. To avoid trouble, put an
escape character (`` `\' ``, in C and Emacs Lisp, `` `/' `` in some
other Lisp dialects) before the opening delimiter. This will not
affect the contents of the string, but will prevent that opening
delimiter from starting a defun. Here's an example:

|  |  |
| --- | --- |
|  | ```   (insert "Foo: \(bar) ") ``` |

In the earliest days, the original Emacs found defuns by moving
upward a level of parentheses or braces until there were no more
levels to go up. This always required scanning all the way back to
the beginning of the buffer, even for a small function. To speed up
the operation, we changed Emacs to assume that any opening delimiter
at the left margin is the start of a defun. This heuristic is nearly
always right, and avoids the need to scan back to the beginning of the
buffer. However, it mandates following the convention described
above.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC271) ] | [ [>](emacs.html#SEC273) ] | [ [<<](emacs.html#SEC273) ] | [ [Up](emacs.html#SEC270) ] | [ [>>](emacs.html#SEC275) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.2.2 Moving by Defuns

These commands move point or set up the region based on top-level
major definitions, also called *defuns*.

`C-M-a`: Move to beginning of current or preceding defun (`beginning-of-defun`). `C-M-e`: Move to end of current or following defun (`end-of-defun`). `C-M-h`: Put region around whole current or following defun (`mark-defun`).

The commands to move to the beginning and end of the current defun
are `C-M-a` (`beginning-of-defun`) and `C-M-e`
(`end-of-defun`). If you repeat one of these commands, or use a
positive numeric argument, each repetition moves to the next defun in
the direction of motion.

`C-M-a` with a negative argument -n moves forward
n times to the next beginning of a defun. This is not exactly
the same place that `C-M-e` with argument n would move to;
the end of this defun is not usually exactly the same place as the
beginning of the following defun. (Whitespace, comments, and perhaps
declarations can separate them.) Likewise, `C-M-e` with a
negative argument moves back to an end of a defun, which is not quite
the same as `C-M-a` with a positive argument.

To operate on the current defun, use `C-M-h` (`mark-defun`)
which puts point at the beginning and mark at the end of the current
defun. This is the easiest way to get ready to kill the defun in
order to move it to a different place in the file. If you use the
command while point is between defuns, it uses the following defun.

In C mode, `C-M-h` runs the function `c-mark-function`,
which is almost the same as `mark-defun`; the difference is that
it backs up over the argument declarations, function name and returned
data type so that the entire C function is inside the region. This is
an example of how major modes adjust the standard key bindings so that
they do their standard jobs in a way better fitting a particular
language. Other major modes may replace any or all of these key
bindings for that purpose.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC272) ] | [ [>](emacs.html#SEC274) ] | [ [<<](emacs.html#SEC274) ] | [ [Up](emacs.html#SEC270) ] | [ [>>](emacs.html#SEC275) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.2.3 Imenu

The Imenu facility offers a way to find the the major definitions in
a file by name. It is also useful in text formatter major modes,
where it treats each chapter, section, etc., as a definition.
(See section [W.2 Tags Tables](emacs.html#SEC334), for a more powerful feature that handles multiple files
together.)

If you type `M-x imenu`, it reads the name of a definition using
the minibuffer, then moves point to that definition. You can use
completion to specify the name; the command always displays the whole
list of valid names.

Alternatively, you can bind the command `imenu` to a mouse
click. Then it displays mouse menus for you to select a definition
name. You can also add the buffer's index to the menu bar by calling
`imenu-add-menubar-index`. If you want to have this menu bar
item available for all buffers in a certain major mode, you can do
this by adding `imenu-add-menubar-index` to its mode hook. But
if you have done that, you will have to wait each time you visit a
file in that mode, while Emacs finds all the definitions in that
buffer.

When you change the contents of a buffer, if you add or delete
definitions, you can update the buffer's index based on the
new contents by invoking the `` `*Rescan*' `` item in the menu.
Rescanning happens automatically if you set `imenu-auto-rescan` to
a non-`nil` value. There is no need to rescan because of small
changes in the text.

You can customize the way the menus are sorted by setting the
variable `imenu-sort-function`. By default, names are ordered as
they occur in the buffer; if you want alphabetic sorting, use the
symbol `imenu--sort-by-name` as the value. You can also
define your own comparison function by writing Lisp code.

Imenu provides the information to guide Which Function mode
(see section [U.2.4 Which Function Mode](emacs.html#SEC274)).
The Speedbar can also use it (see section [P.9 Making and Using a Speedbar Frame](emacs.html#SEC200)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC273) ] | [ [>](emacs.html#SEC275) ] | [ [<<](emacs.html#SEC275) ] | [ [Up](emacs.html#SEC270) ] | [ [>>](emacs.html#SEC275) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.2.4 Which Function Mode

Which Function mode is a minor mode that displays the current
function name in the mode line, updating it as you move around in a
buffer.

To enable (or disable) Which Function mode, use the command `M-x
which-function-mode`. This command is global; it applies to all
buffers, both existing ones and those yet to be created. However,
it only takes effect in certain major modes, those listed in the value of
`which-func-modes`. If the value is `t`, then Which
Function mode applies to all major modes that know how to support
it--in other words, all the major modes that support Imenu.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC274) ] | [ [>](emacs.html#SEC276) ] | [ [<<](emacs.html#SEC281) ] | [ [Up](emacs.html#SEC268) ] | [ [>>](emacs.html#SEC281) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## U.3 Indentation for Programs

The best way to keep a program properly indented is to use Emacs to
reindent it as you change it. Emacs has commands to indent properly
either a single line, a specified number of lines, or all of the lines
inside a single parenthetical grouping.

> |  |  |  |
> | --- | --- | --- |
> | [U.3.1 Basic Program Indentation Commands](emacs.html#SEC276) |  | Indenting a single line. |
> | [U.3.2 Indenting Several Lines](emacs.html#SEC277) |  | Commands to reindent many lines at once. |
> | [U.3.3 Customizing Lisp Indentation](emacs.html#SEC278) |  | Specifying how each Lisp function should be indented. |
> | [U.3.4 Commands for C Indentation](emacs.html#SEC279) |  | Extra features for indenting C and related modes. |
> | [U.3.5 Customizing C Indentation](emacs.html#SEC280) |  | Controlling indentation style for C and related modes. |

Emacs also provides a Lisp pretty-printer in the library `pp`.
This program reformats a Lisp object with indentation chosen to look nice.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC275) ] | [ [>](emacs.html#SEC277) ] | [ [<<](emacs.html#SEC281) ] | [ [Up](emacs.html#SEC275) ] | [ [>>](emacs.html#SEC281) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.3.1 Basic Program Indentation Commands

The basic indentation commands indent a single line according to the
usual conventions of the language you are editing.

`TAB`: Adjust indentation of current line. `C-j`: Equivalent to `RET` followed by `TAB` (`newline-and-indent`). `LINEFEED`: This key, if the keyboard has it, is another way to enter `C-j`.

The basic indentation command is `TAB`, which gives the current line
the correct indentation as determined from the previous lines. The
function that `TAB` runs depends on the major mode; it is
`indent-for-tab-command`
in Lisp mode, `c-indent-command` in C mode, etc. These functions
understand the syntax and conventions of different languages, but they all do
conceptually the same job: `TAB` in any programming-language major mode
inserts or deletes whitespace at the beginning of the current line,
independent of where point is in the line. If point was inside the
whitespace at the beginning of the line, `TAB` puts it at the end of
that whitespace; otherwise, `TAB` keeps point fixed with respect to
the characters around it.

Use `C-q TAB` to insert a tab at point.

When entering lines of new code, use `C-j`
(`newline-and-indent`), which is equivalent to a `RET`
followed by a `TAB`. `C-j` at the end of a line creates a
blank line and then gives it the appropriate indentation.

`TAB` indents lines that start within a parenthetical grouping
each under the preceding line (or the text after the parenthesis).
Therefore, if you manually give one of these lines a nonstandard
indentation, the lines below will tend to follow it. This behavior is
convenient in cases where you have overridden the standard result of
`TAB` because you find it unaesthetic for a particular line.

Remember that an open-parenthesis, open-brace or other opening delimiter
at the left margin is assumed by Emacs (including the indentation routines)
to be the start of a function. Therefore, you must never have an opening
delimiter in column zero that is not the beginning of a function, not even
inside a string. This restriction is vital for making the indentation
commands fast; you must simply accept it. See section [U.2.1 Left Margin Convention](emacs.html#SEC271),
for more information on this.

Normally, lines are indented with tabs and spaces. If you want Emacs
to use spaces only, see [S.3 Tabs vs. Spaces](emacs.html#SEC232).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC276) ] | [ [>](emacs.html#SEC278) ] | [ [<<](emacs.html#SEC278) ] | [ [Up](emacs.html#SEC275) ] | [ [>>](emacs.html#SEC281) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.3.2 Indenting Several Lines

When you wish to reindent several lines of code which have been
altered or moved to a different level in the parenthesis structure,
you have several commands available.

`C-M-q`: Reindent all the lines within one parenthetical grouping(`indent-sexp`). `C-M-\`: Reindent all lines in the region (`indent-region`). `C-u TAB`: Shift an entire parenthetical grouping rigidly sideways so that its first line is properly indented. `M-x indent-code-rigidly`: Shift all the lines in the region rigidly sideways, but do not alter lines that start inside comments and strings.

You can reindent the contents of a single parenthetical grouping by
positioning point before the beginning of it and typing `C-M-q`
(`indent-sexp` in Lisp mode, `c-indent-exp` in C mode; also
bound to other suitable commands in other modes). The indentation of
the line where the grouping starts is not changed; therefore, this
changes only the relative indentation within the grouping, not its
overall indentation. To correct that as well, type `TAB` first.

Another way to specify the range to be reindented is with the
region. The command `C-M-\` (`indent-region`) applies
`TAB` to every line whose first character is between point and
mark.

If you like the relative indentation within a grouping, but not the
indentation of its first line, you can type `C-u TAB` to
reindent the whole grouping as a rigid unit. (This works in Lisp
modes and C and related modes.) `TAB` with a numeric argument
reindents the current line as usual, then reindents by the same amount
all the lines in the parenthetical grouping starting on the current
line. It is clever, though, and does not alter lines that start
inside strings, or C preprocessor lines when in C mode.

You can also perform this operation on the region, using the command
`M-x indent-code-rigidly`. It rigidly shifts all the lines in the
region sideways, like `indent-rigidly` does (see section [S.1 Indentation Commands and Techniques](emacs.html#SEC230)). It doesn't alter the indentation of lines that start
inside a comment or a string, unless the region starts inside that
comment or string.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC277) ] | [ [>](emacs.html#SEC279) ] | [ [<<](emacs.html#SEC279) ] | [ [Up](emacs.html#SEC275) ] | [ [>>](emacs.html#SEC281) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.3.3 Customizing Lisp Indentation

The indentation pattern for a Lisp expression can depend on the function
called by the expression. For each Lisp function, you can choose among
several predefined patterns of indentation, or define an arbitrary one with
a Lisp program.

The standard pattern of indentation is as follows: the second line of the
expression is indented under the first argument, if that is on the same
line as the beginning of the expression; otherwise, the second line is
indented underneath the function name. Each following line is indented
under the previous line whose nesting depth is the same.

If the variable `lisp-indent-offset` is non-`nil`, it overrides
the usual indentation pattern for the second line of an expression, so that
such lines are always indented `lisp-indent-offset` more columns than
the containing list.

Certain functions override the standard pattern. Functions whose
names start with `def` treat the the second lines as the start of
a *body*, by indenting the second line `lisp-body-indent`
additional columns beyond the open-parenthesis that starts the
expression.

You can override the standard pattern in various ways for individual
functions, according to the `lisp-indent-function` property of the
function name. There are four possibilities for this property:

`nil`: This is the same as no property--use the standard indentation pattern. `defun`: Handle this function like a `` `def' `` construct: treat the second line as the start of a *body*. a number, number: The first number arguments of the function are *distinguished* arguments; the rest are considered the body of the expression. A line in the expression is indented according to whether the first argument on it is distinguished or not. If the argument is part of the body, the line is indented `lisp-body-indent` more columns than the open-parenthesis starting the containing expression. If the argument is distinguished and is either the first or second argument, it is indented *twice* that many extra columns. If the argument is distinguished and not the first or second argument, the line uses the standard pattern. a symbol, symbol: symbol should be a function name; that function is called to calculate the indentation of a line within this expression. The function receives two arguments: state: The value returned by `parse-partial-sexp` (a Lisp primitive for indentation and nesting computation) when it parses up to the beginning of this line. pos: The position at which the line being indented begins. It should return either a number, which is the number of columns of indentation for that line, or a list whose car is such a number. The difference between returning a number and returning a list is that a number says that all following lines at the same nesting level should be indented just like this one; a list says that following lines might call for different indentations. This makes a difference when the indentation is being computed by `C-M-q`; if the value is a number, `C-M-q` need not recalculate indentation for the following lines until the end of the list.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC278) ] | [ [>](emacs.html#SEC280) ] | [ [<<](emacs.html#SEC280) ] | [ [Up](emacs.html#SEC275) ] | [ [>>](emacs.html#SEC281) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.3.4 Commands for C Indentation

Here are special features for indentation in C mode and related modes:

`C-c C-q`: Reindent the current top-level function definition or aggregate type declaration (`c-indent-defun`). `C-M-q`: Reindent each line in the balanced expression that follows point (`c-indent-exp`). A prefix argument inhibits error checking and warning messages about invalid syntax. `TAB`: Reindent the current line, and/or in some cases insert a tab character (`c-indent-command`). If `c-tab-always-indent` is `t`, this command always reindents the current line and does nothing else. This is the default. If that variable is `nil`, this command reindents the current line only if point is at the left margin or in the line's indentation; otherwise, it inserts a tab (or the equivalent number of spaces, if `indent-tabs-mode` is `nil`). Any other value (not `nil` or `t`) means always reindent the line, and also insert a tab if within a comment, a string, or a preprocessor directive.

To reindent the whole current buffer, type `C-x h C-M-\`. This
first selects the whole buffer as the region, then reindents that
region.

To reindent the current block, use `C-M-u C-M-q`. This moves
to the front of the block and then reindents it all.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC279) ] | [ [>](emacs.html#SEC281) ] | [ [<<](emacs.html#SEC281) ] | [ [Up](emacs.html#SEC275) ] | [ [>>](emacs.html#SEC281) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.3.5 Customizing C Indentation

C mode and related modes use a simple yet flexible mechanism for
customizing indentation. The mechanism works in two steps: first it
classifies the line syntactically according to its contents and context;
second, it associates each kind of syntactic construct with an
indentation offset based on your selected *style*.

`M-x c-set-style RET style RET`: Select predefined indentation style style.

A style is a named collection of indentation customizations that can
be used in C mode and the related modes. Emacs comes with several
predefined styles, including `gnu`, `k&r`, `bsd`,
`stroustrup`, `linux`, `python`, `java`,
`whitesmith`, `ellemtel`, `cc-mode`, and `user`.
Some of these styles are primarily intended for one language, but any
of them can be used with any of the languages supported by these
modes. To find out what a style looks like, select it and reindent
some code, e.g., by typing `C-M-q` at the start of a function
definition.

To choose a style for the current buffer, use the command `M-x
c-set-style`. Specify a style name as an argument (case is not
significant). This command affects the current buffer only, and it
affects only future invocations of the indentation commands; it does
not reindent the code in the buffer. To reindent the whole buffer in
the new style, you can type `C-x h C-M-\`.

You can also set the variable `c-default-style` to specify the
default style for various major modes. Its value should be an alist,
in which each element specifies one major mode and which indentation
style to use for it. For example,

|  |  |
| --- | --- |
|  | ``` (setq c-default-style       '((java-mode . "java") (other . "gnu"))) ``` |

specifies an explicit choice for Java mode, and the default `` `gnu' ``
style for the other C-like modes. This variable takes effect when you
select one of the C-like major modes; thus, if you specify a new
default style for Java mode, you can make it take effect in an
existing Java mode buffer by typing `M-x java-mode` there.

The `gnu` style specifies the formatting recommended by the GNU
Project for C; it is the default, so as to encourage use of our
recommended style.

See section `Customizing Indentation' in the CC Mode Manual, for
more information on customizing indentation for C and related modes,
including how to override parts of an existing style and how to define
your own styles.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC280) ] | [ [>](emacs.html#SEC282) ] | [ [<<](emacs.html#SEC285) ] | [ [Up](emacs.html#SEC268) ] | [ [>>](emacs.html#SEC285) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## U.4 Commands for Editing with Parentheses

This section describes the commands and features that take advantage
of the parenthesis structure in a program, or help you keep it
balanced.

When talking about these facilities, the term "parenthesis" also
includes braces, brackets, or whatever delimiters are defined to match
in pairs. The major mode controls which delimiters are significant,
through the syntax table (see section [AD.6 The Syntax Table](emacs.html#SEC504)). In Lisp, only parentheses
count; in C, these commands apply to braces and brackets too.

You can use `M-x check-parens` to find any unbalanced
parentheses and unbalanced string quotes in the buffer.

> |  |  |  |
> | --- | --- | --- |
> | [U.4.1 Expressions with Balanced Parentheses](emacs.html#SEC282) |  | Expressions with balanced parentheses. |
> | [U.4.2 Moving in the Parenthesis Structure](emacs.html#SEC283) |  | Commands for moving up, down and across in the structure of parentheses. |
> | [U.4.3 Automatic Display Of Matching Parentheses](emacs.html#SEC284) |  | Insertion of a close-delimiter flashes matching open. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC281) ] | [ [>](emacs.html#SEC283) ] | [ [<<](emacs.html#SEC285) ] | [ [Up](emacs.html#SEC281) ] | [ [>>](emacs.html#SEC285) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.4.1 Expressions with Balanced Parentheses

These commands deal with balanced expressions, also called
*sexps*[(9)](emacs.html#FOOT9).

`C-M-f`: Move forward over a balanced expression (`forward-sexp`). `C-M-b`: Move backward over a balanced expression(`backward-sexp`). `C-M-k`: Kill balanced expression forward (`kill-sexp`). `C-M-DEL`: Kill balanced expression backward (`backward-kill-sexp`). `C-M-t`: Transpose expressions (`transpose-sexps`). `C-M-@`: Put mark after following expression (`mark-sexp`).

Each programming language major mode customizes the definition of
balanced expressions to suit that language. Balanced expressions
typically include symbols, numbers, and string constants, as well as
any pair of matching delimiters and their contents. Some languages
have obscure forms of expression syntax that nobody has bothered to
implement in Emacs.

By convention, the keys for these commands are all Control-Meta
characters. They usually act on expressions just as the corresponding
Meta characters act on words. For instance, the command `C-M-b`
moves backward over a balanced expression, just as `M-b` moves
back over a word.

To move forward over a balanced expression, use `C-M-f`
(`forward-sexp`). If the first significant character after point
is an opening delimiter (`` `(' `` in Lisp; `` `(' ``, `` `[' `` or
`` `{' `` in C), `C-M-f` moves past the matching closing
delimiter. If the character begins a symbol, string, or number,
`C-M-f` moves over that.

The command `C-M-b` (`backward-sexp`) moves backward over a
balanced expression. The detailed rules are like those above for
`C-M-f`, but with directions reversed. If there are prefix
characters (single-quote, backquote and comma, in Lisp) preceding the
expression, `C-M-b` moves back over them as well. The balanced
expression commands move across comments as if they were whitespace,
in most modes.

`C-M-f` or `C-M-b` with an argument repeats that operation the
specified number of times; with a negative argument, it moves in the
opposite direction.

Killing a whole balanced expression can be done with `C-M-k`
(`kill-sexp`) or `C-M-DEL` (`backward-kill-sexp`).
`C-M-k` kills the characters that `C-M-f` would move over, and
`C-M-DEL` kills the characters that `C-M-b` would move
over. On some machines, `C-M-DEL` typed on the console is a
command to reboot; when that is so, you cannot use it as an Emacs
command. This conflict is rare, though: usually the `DEL` key for
Emacs is really `BACKSPACE`, and the reboot command is
`C-M-DELETE`, so there is no conflict.

A somewhat random-sounding command which is nevertheless handy is
`C-M-t` (`transpose-sexps`), which drags the previous
balanced expression across the next one. An argument serves as a
repeat count, and a negative argument drags the previous balanced
expression backwards across those before it (thus canceling out the
effect of `C-M-t` with a positive argument). An argument of zero,
rather than doing nothing, transposes the balanced expressions ending
at or after point and the mark.

To set the region around the next balanced expression in the buffer,
use `C-M-@` (`mark-sexp`), which sets mark at the same place
that `C-M-f` would move to. `C-M-@` takes arguments like
`C-M-f`. In particular, a negative argument is useful for putting
the mark at the beginning of the previous balanced expression.

In languages that use infix operators, such as C, it is not possible
to recognize all balanced expressions as such because there can be
multiple possibilities at a given position. For example, C mode does
not treat `` `foo + bar' `` as a single expression, even though it
*is* one C expression; instead, it recognizes `` `foo' `` as one
expression and `` `bar' `` as another, with the `` `+' `` as punctuation
between them. Both `` `foo + bar' `` and `` `foo' `` are legitimate
choices for "the expression following point" when point is at the
`` `f' ``, so the expression commands must perforce choose one or the
other to operate on. Note that `` `(foo + bar)' `` is recognized as a
single expression in C mode, because of the parentheses.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC282) ] | [ [>](emacs.html#SEC284) ] | [ [<<](emacs.html#SEC284) ] | [ [Up](emacs.html#SEC281) ] | [ [>>](emacs.html#SEC285) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.4.2 Moving in the Parenthesis Structure

The Emacs commands for handling parenthetical groupings see nothing
except parentheses (or whatever characters must balance in the
language you are working with), and the escape characters that might
be used to quote those. They are mainly intended for editing
programs, but can be useful for editing any text that has parentheses.
They are sometimes called "list" commands because in Lisp these
groupings are lists.

`C-M-n`: Move forward over a parenthetical group (`forward-list`). `C-M-p`: Move backward over a parenthetical group(`backward-list`). `C-M-u`: Move up in parenthesis structure (`backward-up-list`). `C-M-d`: Move down in parenthesis structure (`down-list`).

The "list" commands `C-M-n` (`forward-list`) and
`C-M-p` (`backward-list`) move over one (or n)
parenthetical groupings, skipping blithely over any amount of text
that doesn't include meaningful parentheses (symbols, strings, etc.).

`C-M-n` and `C-M-p` try to stay at the same level in the
parenthesis structure. To move *up* one (or n) levels, use
`C-M-u` (`backward-up-list`). `C-M-u` moves backward up
past one unmatched opening delimiter. A positive argument serves as a
repeat count; a negative argument reverses the direction of motion, so
that the command moves forward and up one or more levels.

To move *down* in the parenthesis structure, use `C-M-d`
(`down-list`). In Lisp mode, where `` `(' `` is the only opening
delimiter, this is nearly the same as searching for a `` `(' ``. An
argument specifies the number of levels to go down.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC283) ] | [ [>](emacs.html#SEC285) ] | [ [<<](emacs.html#SEC285) ] | [ [Up](emacs.html#SEC281) ] | [ [>>](emacs.html#SEC285) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.4.3 Automatic Display Of Matching Parentheses

The Emacs parenthesis-matching feature is designed to show
automatically how parentheses (and other matching delimiters) match in
the text. Whenever you type a self-inserting character that is a
closing delimiter, the cursor moves momentarily to the location of the
matching opening delimiter, provided that is on the screen. If it is
not on the screen, Emacs displays some of the text near it in the echo
area. Either way, you can tell which grouping you are closing off.

If the opening delimiter and closing delimiter are mismatched--such
as in `` `[x)' ``---a warning message is displayed in the echo area.

Three variables control parenthesis match display.
`blink-matching-paren` turns the feature on or off: `nil`
disables it, but the default is `t` to enable match display.

`blink-matching-delay` says how many seconds to leave the
cursor on the matching opening delimiter, before bringing it back to
the real location of point; the default is 1, but on some systems it
is useful to specify a fraction of a second.

`blink-matching-paren-distance` specifies how many characters
back to search to find the matching opening delimiter. If the match
is not found in that distance, scanning stops, and nothing is displayed.
This is to prevent the scan for the matching delimiter from wasting
lots of time when there is no match. The default is 25600.

Show Paren mode provides a more powerful kind of automatic matching.
Whenever point is after a closing delimiter, that delimiter and its
matching opening delimiter are both highlighted; otherwise, if point
is before an opening delimiter, the matching closing delimiter is
highlighted. (There is no need to highlight the opening delimiter in
that case, because the cursor appears on top of that character.) Use
the command `M-x show-paren-mode` to enable or disable this mode.

By default, `show-paren-mode` uses colors to highlight the
parentheses. However, if your display doesn't support colors, you can
customize the faces `show-paren-match-face` and
`show-paren-mismatch-face` to use other attributes, such as bold or
underline. See section [AD.2.2.3 Customizing Faces](emacs.html#SEC482).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC284) ] | [ [>](emacs.html#SEC286) ] | [ [<<](emacs.html#SEC289) ] | [ [Up](emacs.html#SEC268) ] | [ [>>](emacs.html#SEC289) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## U.5 Manipulating Comments

Because comments are such an important part of programming, Emacs
provides special commands for editing and inserting comments.

> |  |  |  |
> | --- | --- | --- |
> | [U.5.1 Comment Commands](emacs.html#SEC286) |  | Inserting, killing, and indenting comments. |
> | [U.5.2 Multiple Lines of Comments](emacs.html#SEC287) |  | Commands for adding and editing multi-line comments. |
> | [U.5.3 Options Controlling Comments](emacs.html#SEC288) |  | Customizing the comment features. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC285) ] | [ [>](emacs.html#SEC287) ] | [ [<<](emacs.html#SEC289) ] | [ [Up](emacs.html#SEC285) ] | [ [>>](emacs.html#SEC289) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.5.1 Comment Commands

The comment commands in this table insert, kill and align comments.
They are described in this section and following sections.

`M-;`: Insert or realign comment on current line; alternatively, comment or uncomment the region (`comment-dwim`). `C-u M-;`: Kill comment on current line (`comment-kill`). `C-x ;`: Set comment column (`comment-set-column`). `C-M-j`: Like `RET` followed by inserting and aligning a comment (`comment-indent-new-line`). `M-x comment-region`: Add or remove comment delimiters on all the lines in the region.

The command to create or align a comment is `M-;`
(`comment-dwim`). The word "dwim" is an acronym for "Do What
I Mean"; it indicates that this command can be used for many
different jobs relating to comments, depending on the situation where
you use it.

If there is no comment already on the line, `M-;` inserts a new
comment, aligned at a specific column called the *comment column*.
The new comment begins with the string Emacs thinks comments should
start with (the value of `comment-start`; see below). Point is
after that string, so you can insert the text of the comment right
away. If the major mode has specified a string to terminate comments,
`M-;` inserts that too, to keep the syntax valid.

If the text of the line extends past the comment column, then the
comment start string is indented to a suitable boundary (usually, at
least one space is inserted).

You can also use `M-;` to align an existing comment. If a line
already contains the comment-start string, `M-;` reindents it to
the conventional alignment and moves point after it. (Exception:
comments starting in column 0 are not moved.) Even when an existing
comment is properly aligned, `M-;` is still useful for moving
directly to the start of the text inside the comment.

`C-u M-;` kills any comment on the current line, along with the
whitespace before it. To reinsert the comment on another line, move
to the end of that line, do `C-y`, and then do `M-;` to
realign it.

Note that `C-u M-;` is not a distinct key; it is `M-;`
(`comment-dwim`) with a prefix argument. That command is
programmed so that when it receives a prefix argument it calls
`comment-kill`. However, `comment-kill` is a valid command
in its own right, and you can bind it directly to a key if you wish.

`M-;` does two other jobs when used with an active region in
Transient Mark mode (see section [H.2 Transient Mark Mode](emacs.html#SEC54)). Then it either adds or
removes comment delimiters on each line of the region. (If every line
is a comment, it removes comment delimiters from each; otherwise, it
adds comment delimiters to each.) If you are not using Transient Mark
mode, then you should use the commands `comment-region` and
`uncomment-region` to do these jobs (see section [U.5.2 Multiple Lines of Comments](emacs.html#SEC287)).
A prefix argument used in these circumstances specifies how many
comment delimiters to add or how many to delete.

Some major modes have special rules for indenting certain kinds of
comments in certain contexts. For example, in Lisp code, comments which
start with two semicolons are indented as if they were lines of code,
instead of at the comment column. Comments which start with three
semicolons are supposed to start at the left margin. Emacs understands
these conventions by indenting a double-semicolon comment using `TAB`,
and by not changing the indentation of a triple-semicolon comment at all.

|  |  |
| --- | --- |
|  | ``` ;; This function is just an example ;;; Here either two or three semicolons are appropriate. (defun foo (x) ;;; And now, the first part of the function:   ;; The following line adds one.   (1+ x))           ; This line adds one. ``` |

In C code, a comment preceded on its line by nothing but whitespace
is indented like a line of code.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC286) ] | [ [>](emacs.html#SEC288) ] | [ [<<](emacs.html#SEC288) ] | [ [Up](emacs.html#SEC285) ] | [ [>>](emacs.html#SEC289) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.5.2 Multiple Lines of Comments

If you are typing a comment and wish to continue it on another line,
you can use the command `C-M-j` (`comment-indent-new-line`).
This terminates the comment you are typing, creates a new blank line
afterward, and begins a new comment indented under the old one. When
Auto Fill mode is on, going past the fill column while typing a comment
causes the comment to be continued in just this fashion. If point is
not at the end of the line when `C-M-j` is typed, the text on
the rest of the line becomes part of the new comment line.

To turn existing lines into comment lines, use the `M-x
comment-region` command. It adds comment delimiters to the lines that start
in the region, thus commenting them out. With a negative argument, it
does the opposite--it deletes comment delimiters from the lines in the
region.

With a positive argument, `comment-region` duplicates the last
character of the comment start sequence it adds; the argument specifies
how many copies of the character to insert. Thus, in Lisp mode,
`C-u 2 M-x comment-region` adds `` `;;' `` to each line. Duplicating
the comment delimiter is a way of calling attention to the comment. It
can also affect how the comment is indented. In Lisp, for proper
indentation, you should use an argument of two or three, if between defuns;
if within a defun, it must be three.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC287) ] | [ [>](emacs.html#SEC289) ] | [ [<<](emacs.html#SEC289) ] | [ [Up](emacs.html#SEC285) ] | [ [>>](emacs.html#SEC289) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.5.3 Options Controlling Comments

The comment column is stored in the variable `comment-column`. You
can set it to a number explicitly. Alternatively, the command `C-x ;`
(`comment-set-column`) sets the comment column to the column point is
at. `C-u C-x ;` sets the comment column to match the last comment
before point in the buffer, and then does a `M-;` to align the
current line's comment under the previous one.

The variable `comment-column` is per-buffer: setting the variable
in the normal fashion affects only the current buffer, but there is a
default value which you can change with `setq-default`.
See section [AD.2.4 Local Variables](emacs.html#SEC485). Many major modes initialize this variable for the
current buffer.

The comment commands recognize comments based on the regular
expression that is the value of the variable `comment-start-skip`.
Make sure this regexp does not match the null string. It may match more
than the comment starting delimiter in the strictest sense of the word;
for example, in C mode the value of the variable is
`"/\\*+ *\\|//+ *""`, which matches extra stars and spaces
after the `` `/*' `` itself, and accepts C++ style comments also.
(Note that `` `\\' `` is needed in Lisp syntax to include a `` `\' `` in
the string, which is needed to deny the first star its special meaning
in regexp syntax. See section [K.5 Syntax of Regular Expressions](emacs.html#SEC97).)

When a comment command makes a new comment, it inserts the value of
`comment-start` to begin it. The value of `comment-end` is
inserted after point, so that it will follow the text that you will insert
into the comment. In C mode, `comment-start` has the value
`"/* "` and `comment-end` has the value `" */"`.

The variable `comment-padding` specifies how many spaces
`comment-region` should insert on each line between the
comment delimiter and the line's original text. The default is 1,
to insert one space.

The variable `comment-multi-line` controls how `C-M-j`
(`indent-new-comment-line`) behaves when used inside a comment. If
`comment-multi-line` is `nil`, as it normally is, then the
comment on the starting line is terminated and a new comment is started
on the new following line. If `comment-multi-line` is not
`nil`, then the new following line is set up as part of the same
comment that was found on the starting line. This is done by not
inserting a terminator on the old line, and not inserting a starter on
the new line. In languages where multi-line comments work, the choice
of value for this variable is a matter of taste.

The variable `comment-indent-function` should contain a function
that will be called to compute the indentation for a newly inserted
comment or for aligning an existing comment. It is set differently by
various major modes. The function is called with no arguments, but with
point at the beginning of the comment, or at the end of a line if a new
comment is to be inserted. It should return the column in which the
comment ought to start. For example, in Lisp mode, the indent hook
function bases its decision on how many semicolons begin an existing
comment, and on the code in the preceding lines.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC288) ] | [ [>](emacs.html#SEC290) ] | [ [<<](emacs.html#SEC293) ] | [ [Up](emacs.html#SEC268) ] | [ [>>](emacs.html#SEC293) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## U.6 Documentation Lookup

Emacs provides several features you can use to look up the
documentation of functions, variables and commands that you plan to
use in your program.

> |  |  |  |
> | --- | --- | --- |
> | [U.6.1 Info Documentation Lookup](emacs.html#SEC290) |  | Looking up library functions and commands in Info files. |
> | [U.6.2 Man Page Lookup](emacs.html#SEC291) |  | Looking up man pages of library functions and commands. |
> | [U.6.3 Emacs Lisp Documentation Lookup](emacs.html#SEC292) |  | Looking up Emacs Lisp functions, etc. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC289) ] | [ [>](emacs.html#SEC291) ] | [ [<<](emacs.html#SEC293) ] | [ [Up](emacs.html#SEC289) ] | [ [>>](emacs.html#SEC293) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.6.1 Info Documentation Lookup

For C, Lisp, and other languages that have documentation in Info,
you can use `C-h C-i` (`info-lookup-symbol`) to view the Info
documentation for a symbol. You specify the symbol with the
minibuffer; the default is the symbol appearing in the buffer at
point.

The major mode determines where to look for documentation for the
symbol--which Info files to look in, and which indices to search.
You can also use `M-x info-lookup-file` to look for documentation
for a file name.

This feature currently supports the modes Awk, Autoconf, Bison, C,
Emacs Lisp, LaTeX, M4, Makefile, Octave, Perl, Scheme, and Texinfo,
provided you have installed the relevant Info files, which are
typically available with the appropriate GNU package.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC290) ] | [ [>](emacs.html#SEC292) ] | [ [<<](emacs.html#SEC292) ] | [ [Up](emacs.html#SEC289) ] | [ [>>](emacs.html#SEC293) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.6.2 Man Page Lookup

On Unix, the main form of on-line documentation was the *manual
page* or *man page*. In the GNU operating system, we hope to
replace man pages with better-organized manuals that you can browse
with Info (see section [G.8 Other Help Commands](emacs.html#SEC50)). This process is not finished, so it is
still useful to read manual pages.

You can read the man page for an operating system command, library
function, or system call, with the `M-x manual-entry` command. It
runs the `man` program to format the man page; if the system
permits, it runs `man` asynchronously, so that you can keep on
editing while the page is being formatted. (On MS-DOS and MS-Windows
3, you cannot edit while Emacs waits for `man` to finish.) The
result goes in a buffer named `` `*Man topic*' ``. These buffers
use a special major mode, Man mode, that facilitates scrolling and
jumping to other manual pages. For details, type `C-h m` while in
a man page buffer.

Each man page belongs to one of ten or more *sections*, each
named by a digit or by a digit and a letter. Sometimes there are
multiple man pages with the same name in different sections. To read
a man page from a specific section, type
`` `topic(section)' `` or `` `section topic' ``
when `M-x manual-entry` prompts for the topic. For example, to
read the man page for the C library function `chmod` (as opposed
to a command of the same name), type `M-x manual-entry RET
chmod(2) RET` (`chmod` is a system call, so it is in
section `` `2' ``).

If you do not specify a section, the results depend on how the
`man` program works on your system. Some of them display only
the first man page they find. Others display all man pages that have
the specified name, so you can move between them with the `M-n`
and `M-p` keys[(10)](emacs.html#FOOT10).
The mode line shows how many manual pages are present in the Man buffer.

By default, Emacs highlights the text in man pages. For a long man
page, highlighting can take substantial time. You can turn off
highlighting of man pages by setting the variable
`Man-fontify-manpage-flag` to `nil`.

If you insert the text of a man page into an Emacs buffer in some
other fashion, you can use the command `M-x Man-fontify-manpage` to
perform the same conversions that `M-x manual-entry` does.

An alternative way of reading manual pages is the `M-x woman`
command[(11)](emacs.html#FOOT11). Unlike `M-x man`, it does not run any external
programs to format and display the man pages; instead it does the job
in Emacs Lisp, so it works on systems such as MS-Windows, where the
`man` program (and the other programs it uses) are not generally
available.

`M-x woman` prompts for a name of a manual page, and provides
completion based on the list of manual pages that are installed on
your machine; the list of available manual pages is computed
automatically the first time you invoke `woman`. The word at
point in the current buffer is used to suggest the default for the
name the manual page.

With a numeric argument, `M-x woman` recomputes the list of the
manual pages used for completion. This is useful if you add or delete
manual pages.

If you type a name of a manual page and `M-x woman` finds that
several manual pages by the same name exist in different sections, it
pops up a window with possible candidates asking you to choose one of
them.

By default, `M-x woman` looks for manual pages in the
directories specified in the `MANPATH` environment variable. (If
`MANPATH` is not set, `woman` uses a suitable default value,
which can be customized.) More precisely, `woman` looks for
subdirectories that match the shell wildcard pattern `man\*' in each one
of these directories, and tries to find the manual pages in those
subdirectories. When first invoked, `M-x woman` converts the
value of `MANPATH` to a list of directory names and stores that
list in the `woman-manpath` variable. Changing the value of this
variable is another way to control the list of directories used.

You can also augment the list of directories searched by
`woman` by setting the value of the `woman-path` variable.
This variable should hold a list of specific directories which
`woman` should search, in addition to those in
`woman-manpath`. Unlike `woman-manpath`, the directories in
`woman-path` are searched for the manual pages, not for
`man\*' subdirectories.

Occasionally, you might need to display manual pages that are not in
any of the directories listed by `woman-manpath` and
`woman-path`. The `M-x woman-find-file` command prompts for a
name of a manual page file, with completion, and then formats and
displays that file like `M-x woman` does.

The first time you invoke `M-x woman`, it defines the Dired
`W` key to run the `woman-find-file` command on the current
line's file. You can disable this by setting the variable
`woman-dired-keys` to `nil`. See section [AB. Dired, the Directory Editor](emacs.html#SEC396). In addition,
the Tar-mode `w` key is define to invoke `woman-find-file` on
the current line's archive member.

For more information about setting up and using `M-x woman`, see
section `Browse UN\*X Manual Pages WithOut Man' in The WoMan Manual.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC291) ] | [ [>](emacs.html#SEC293) ] | [ [<<](emacs.html#SEC293) ] | [ [Up](emacs.html#SEC289) ] | [ [>>](emacs.html#SEC293) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.6.3 Emacs Lisp Documentation Lookup

As you edit Lisp code to be run in Emacs, you can use the commands
`C-h f` (`describe-function`) and `C-h v`
(`describe-variable`) to view documentation of functions and
variables that you want to use. These commands use the minibuffer to
read the name of a function or variable to document, and display the
documentation in a window. Their default arguments are based on the
code in the neighborhood of point. For `C-h f`, the default is
the function called in the innermost list containing point. `C-h
v` uses the symbol name around or adjacent to point as its default.

A more automatic but less powerful method is Eldoc mode. This minor
mode constantly displays in the echo area the argument list for the
function being called at point. (In other words, it finds the
function call that point is contained in, and displays the argument
list of that function.) Eldoc mode applies in Emacs Lisp and Lisp
Interaction modes only. Use the command `M-x eldoc-mode` to
enable or disable this feature.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC292) ] | [ [>](emacs.html#SEC294) ] | [ [<<](emacs.html#SEC294) ] | [ [Up](emacs.html#SEC268) ] | [ [>>](emacs.html#SEC316) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## U.7 Hideshow minor mode

Hideshow minor mode provides selective display of portions of a
program, known as *blocks*. You can use `M-x hs-minor-mode`
to enable or disable this mode, or add `hs-minor-mode` to the
mode hook for certain major modes in order to enable it automatically
for those modes.

Just what constitutes a block depends on the major mode. In C mode
or C++ mode, they are delimited by braces, while in Lisp mode and
similar modes they are delimited by parentheses. Multi-line comments
also count as blocks.

`C-c @ C-h`: Hide the current block (`hs-hide-block`). `C-c @ C-s`: Show the current block (`hs-show-block`). `C-c @ C-c`: Either hide or show the current block (`hs-toggle-hiding`) `S-Mouse-2`: Either hide or show the block you click on (`hs-mouse-toggle-hiding`) `C-c @ C-M-h`: Hide all top-level blocks (`hs-hide-all`). `C-c @ C-M-s`: Show everything in the buffer (`hs-show-all`). `C-c @ C-l`: Hide all blocks n levels below this block (`hs-hide-level`).

These user options exist for customizing Hideshow mode.

`hs-hide-comments-when-hiding-all`: Non-`nil` says that `hs-hide-all` should hide comments too. `hs-isearch-open`: Specifies what kind of hidden blocks to open in Isearch mode. The value should be one of these four symbols. `code`: Open only code blocks. `comment`: Open only comments. `t`: Open both code blocks and comments. `nil`: Open neither code blocks nor comments. `hs-special-modes-alist`: A list of elements, each specifying how to initialize Hideshow variables for one major mode. See the variable's documentation string for more information.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC293) ] | [ [>](emacs.html#SEC295) ] | [ [<<](emacs.html#SEC295) ] | [ [Up](emacs.html#SEC268) ] | [ [>>](emacs.html#SEC316) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## U.8 Completion for Symbol Names

In Emacs, completion is something you normally do in the minibuffer.
But one kind of completion is available in all buffers: completion for
symbol names.

The character `M-TAB` runs a command to complete the
partial symbol before point against the set of meaningful symbol
names. This command inserts at point any additional characters that
it can determine from the partial name.

If the partial name in the buffer has multiple possible completions
that differ in the very next character, so that it is impossible to
complete even one more character, `M-TAB` displays a list of
all possible completions in another window.

In most programming language major modes, `M-TAB` runs the
command `complete-symbol`, which provides two kinds of completion.
Normally it does completion based on a tags table (see section [W.2 Tags Tables](emacs.html#SEC334)); with a
numeric argument (regardless of the value), it does completion based on
the names listed in the Info file indexes for your language. Thus, to
complete the name of a symbol defined in your own program, use
`M-TAB` with no argument; to complete the name of a standard
library function, use `C-u M-TAB`. Of course, Info-based
completion works only if there is an Info file for the standard library
functions of your language, and only if it is installed at your site.

In Emacs-Lisp mode, the name space for completion normally consists of
nontrivial symbols present in Emacs--those that have function
definitions, values or properties. However, if there is an
open-parenthesis immediately before the beginning of the partial symbol,
only symbols with function definitions are considered as completions.
The command which implements this is `lisp-complete-symbol`.

In Text mode and related modes, `M-TAB` completes words
based on the spell-checker's dictionary. See section [L.4 Checking and Correcting Spelling](emacs.html#SEC109).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC294) ] | [ [>](emacs.html#SEC296) ] | [ [<<](emacs.html#SEC268) ] | [ [Up](emacs.html#SEC268) ] | [ [>>](emacs.html#SEC316) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## U.9 Glasses minor mode

Glasses minor mode makes `` `unreadableIdentifiersLikeThis' ``
readable by altering the way they display. It knows two different
ways to do this: by displaying underscores between a lower-case letter
and the following capital letter, and by emboldening the capital
letters. It does not alter the buffer text, only the way they
display, so you can use it even on read-only buffers. You can use the
command `M-x glasses-mode` to enable or disable the mode in the
current buffer; you can also add `glasses-mode` to the mode hook
of the programming language major modes in which you normally want
to use Glasses mode.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC295) ] | [ [>](emacs.html#SEC297) ] | [ [<<](emacs.html#SEC270) ] | [ [Up](emacs.html#SEC268) ] | [ [>>](emacs.html#SEC316) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## U.10 Other Features Useful for Editing Programs

A number of Emacs commands that aren't designed specifically for
editing programs are useful for that nonetheless.

The Emacs commands that operate on words, sentences and paragraphs
are useful for editing code. Most symbols names contain words
(see section [T.1 Words](emacs.html#SEC234)); sentences can be found in strings and comments
(see section [T.2 Sentences](emacs.html#SEC235)). Paragraphs in the strict sense can be found in
program code (in long comments), but the paragraph commands are useful
in other places too, because programming language major modes define
paragraphs to begin and end at blank lines (see section [T.3 Paragraphs](emacs.html#SEC236)).
Judicious use of blank lines to make the program clearer will also
provide useful chunks of text for the paragraph commands to work on.
Auto Fill mode, if enabled in a programming language major mode,
indents the new lines which it creates.

The selective display feature is useful for looking at the overall
structure of a function (see section [J.9 Selective Display](emacs.html#SEC86)). This feature
hides the lines that are indented more than a specified amount.
Programming modes often support Outline minor mode (see section [T.8 Outline Mode](emacs.html#SEC246)). The Foldout package provides folding-editor features
(see section [T.8.5 Folding Editing](emacs.html#SEC251)).

The "automatic typing" features may be useful for writing programs.
See section `Autotyping' in Autotyping.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC296) ] | [ [>](emacs.html#SEC298) ] | [ [<<](emacs.html#SEC270) ] | [ [Up](emacs.html#SEC268) ] | [ [>>](emacs.html#SEC303) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## U.11 C and Related Modes

This section gives a brief description of the special features
available in C, C++, Objective-C, Java, CORBA IDL, and Pike modes.
(These are called "C mode and related modes.") See section `ccmode' in CC Mode, for a more extensive description of these modes
and their special features.

> |  |  |  |
> | --- | --- | --- |
> | [U.11.1 C Mode Motion Commands](emacs.html#SEC298) |  | Commands to move by C statements, etc. |
> | [U.11.2 Electric C Characters](emacs.html#SEC299) |  | Colon and other chars can automatically reindent. |
> | [U.11.3 Hungry Delete Feature in C](emacs.html#SEC300) |  | A more powerful DEL command. |
> | [U.11.4 Other Commands for C Mode](emacs.html#SEC301) |  | Filling comments, viewing expansion of macros, and other neat features. |
> | [U.11.5 Comments in C Modes](emacs.html#SEC302) |  | Options for customizing comment style. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC297) ] | [ [>](emacs.html#SEC299) ] | [ [<<](emacs.html#SEC270) ] | [ [Up](emacs.html#SEC297) ] | [ [>>](emacs.html#SEC303) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.11.1 C Mode Motion Commands

This section describes commands for moving point, in C mode and
related modes.

`C-c C-u`: Move point back to the containing preprocessor conditional, leaving the mark behind. A prefix argument acts as a repeat count. With a negative argument, move point forward to the end of the containing preprocessor conditional. When going backwards, `#elif` is treated like `#else` followed by `#if`. When going forwards, `#elif` is ignored. `C-c C-p`: Move point back over a preprocessor conditional, leaving the mark behind. A prefix argument acts as a repeat count. With a negative argument, move forward. `C-c C-n`: Move point forward across a preprocessor conditional, leaving the mark behind. A prefix argument acts as a repeat count. With a negative argument, move backward. `M-a`: Move point to the beginning of the innermost C statement (`c-beginning-of-statement`). If point is already at the beginning of a statement, move to the beginning of the preceding statement. With prefix argument n, move back n - 1 statements. If point is within a string or comment, or next to a comment (only whitespace between them), this command moves by sentences instead of statements. When called from a program, this function takes three optional arguments: the numeric prefix argument, a buffer position limit (don't move back before that place), and a flag that controls whether to do sentence motion when inside of a comment. `M-e`: Move point to the end of the innermost C statement; like `M-a` except that it moves in the other direction (`c-end-of-statement`). `M-x c-backward-into-nomenclature`: Move point backward to beginning of a C++ nomenclature section or word. With prefix argument n, move n times. If n is negative, move forward. C++ nomenclature means a symbol name in the style of NamingSymbolsWithMixedCaseAndNoUnderlines; each capital letter begins a section or word. In the GNU project, we recommend using underscores to separate words within an identifier in C or C++, rather than using case distinctions. `M-x c-forward-into-nomenclature`: Move point forward to end of a C++ nomenclature section or word. With prefix argument n, move n times.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC298) ] | [ [>](emacs.html#SEC300) ] | [ [<<](emacs.html#SEC300) ] | [ [Up](emacs.html#SEC297) ] | [ [>>](emacs.html#SEC303) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### U.11.2 Electric C Characters

In C mode and related modes, certain printing characters are
"electric"---in addition to inserting themselves, they also reindent
the current line and may insert newlines. This feature is controlled by
the variable `c-auto-newline`. The "electric" characters are
`{`, `}`, `:`, `#`, `;`, `,`, `<`,
`>`, `/`, `*`, `(`, and `)`.

Electric characters insert newlines only when the *auto-newline*
feature is enabled (indicated by `` `/a' `` in the mode line after the
mode name). This feature is controlled by the variable
`c-auto-newline`. You can turn this feature on or off with the
command `C-c C-a`:

`C-c C-a`: Toggle the auto-newline feature (`c-toggle-auto-state`). With a prefix argument, this command turns the auto-newline feature on if the argument is positive, and off if it is negative.

The colon character is electric because that is appropriate for a
single colon. But when you want to insert a double colon in C++, the
electric behavior of colon is inconvenient. You can insert a double
colon with no reindentation or newlines by typing `C-c :`:

`C-c :`: Insert a double colon scope operator at point, without reindenting the line or adding any newlines (`c-scope-operator`).

The electric `#` key reindents the line if it appears to be the
beginning of a preprocessor directive. This happens when the value of
`c-electric-pound-behavior` is `(alignleft)`. You can turn
this feature off by setting `c-electric-pound-behavior` to
`nil`.

The variable `c-hanging-braces-alist` controls the insertion of
newlines before and after inserted braces. It is an association list
with elements of the following form: `(syntactic-symbol
. nl-list)`. Most of the syntactic symbols that appear in
`c-offsets-alist` are meaningful here as well.

The list nl-list may contain either of the symbols
`before` or `after`, or both; or it may be `nil`. When a
brace is inserted, the syntactic context it defines is looked up in
`c-hanging-braces-alist`; if it is found, the nl-list is used
to determine where newlines are inserted: either before the brace,
after, or both. If not found, the default is to insert a newline both
before and after braces.

The variable `c-hanging-colons-alist` controls the insertion of
newlines before and after inserted colons. It is an association list
with elements of the following form: `(syntactic-symbol
. nl-list)`. The list nl-list may contain either of the
symbols `before` or `after`, or both; or it may be `nil`.

When a colon is inserted, the syntactic symbol it defines is looked
up in this list, and if found, the nl-list is used to determine
where newlines are inserted: either before the brace, after, or both.
If the syntactic symbol is not found in this list, no newlines are
inserted.

Electric characters can also delete newlines automatically when the
auto-newline feature is enabled. This feature makes auto-newline more
acceptable, by deleting the newlines in the most common cases where you
do not want them. Emacs can recognize several cases in which deleting a
newline might be desirable; by setting the variable
`c-cleanup-list`, you can specify *which* of these cases that
should happen. The variable's value is a list of symbols, each
describing one case for possible deletion of a newline. Here are the
meaningful symbols, and their meanings:

`brace-catch-brace`: Clean up `` `} catch (condition) {' `` constructs by placing the entire construct on a single line. The clean-up occurs when you type the `` `{' ``, if there is nothing between the braces aside from `catch` and condition. `brace-else-brace`: Clean up `` `} else {' `` constructs by placing the entire construct on a single line. The clean-up occurs when you type the `` `{' `` after the `else`, but only if there is nothing but white space between the braces and the `else`. `brace-elseif-brace`: Clean up `` `} else if (...) {' `` constructs by placing the entire construct on a single line. The clean-up occurs when you type the `` `{' ``, if there is nothing but white space between the `` `}' `` and `` `{' `` aside from the keywords and the `if`-condition. `empty-defun-braces`: Clean up empty defun braces by placing the braces on the same line. Clean-up occurs when you type the closing brace. `defun-close-semi`: Clean up the semicolon after a `struct` or similar type declaration, by placing the semicolon on the same line as the closing brace. Clean-up occurs when you type the semicolon. `list-close-comma`: Clean up commas following braces in array and aggregate initializers. Clean-up occurs when you type the comma. `scope-operator`: Clean up double colons which may designate a C++ scope operator, by placing the colons together. Clean-up occurs when you type the second colon, but only when the two colons are separated by nothing but whitespace.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC299) ] | [ [>](emacs.html#SEC301) ] | [ [<<](emacs.html#SEC301) ] | [ [Up](emacs.html#SEC297) ] | [ [>>](emacs.html#SEC303) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
