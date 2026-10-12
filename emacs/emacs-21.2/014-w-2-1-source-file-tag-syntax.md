### W.2.1 Source File Tag Syntax

Here is how tag syntax is defined for the most popular languages:

* In C code, any C function or typedef is a tag, and so are definitions of
  `struct`, `union` and `enum`.
  `#define` macro definitions and `enum` constants are also
  tags, unless you specify `` `--no-defines' `` when making the tags table.
  Similarly, global variables are tags, unless you specify
  `` `--no-globals' ``. Use of `` `--no-globals' `` and `` `--no-defines' ``
  can make the tags table file much smaller.

  You can tag function declarations and external variables in addition
  to function definitions by giving the `` `--declarations' `` option to
  `etags`.

  * In C++ code, in addition to all the tag constructs of C code, member
    functions are also recognized, and optionally member variables if you
    use the `` `--members' `` option. Tags for variables and functions in
    classes are named `` `class::variable' `` and
    `` `class::function' ``. `operator` definitions have
    tag names like `` `operator+' ``.

    * In Java code, tags include all the constructs recognized in C++, plus
      the `interface`, `extends` and `implements` constructs.
      Tags for variables and functions in classes are named
      `` `class.variable' `` and `` `class.function' ``.

      * In LaTeX text, the argument of any of the commands `\chapter`,
        `\section`, `\subsection`, `\subsubsection`,
        `\eqno`, `\label`, `\ref`, `\cite`, `\bibitem`,
        `\part`, `\appendix`, `\entry`, or `\index`, is a
        tag.

        Other commands can make tags as well, if you specify them in the
        environment variable `TEXTAGS` before invoking `etags`. The
        value of this environment variable should be a colon-separated list of
        command names. For example,

        |  |  |
        | --- | --- |
        |  | ``` TEXTAGS="def:newcommand:newenvironment" export TEXTAGS ``` |

        specifies (using Bourne shell syntax) that the commands `` `\def' ``,
        `` `\newcommand' `` and `` `\newenvironment' `` also define tags.

        * In Lisp code, any function defined with `defun`, any variable
          defined with `defvar` or `defconst`, and in general the first
          argument of any expression that starts with `` `(def' `` in column zero, is
          a tag.

          * In Scheme code, tags include anything defined with `def` or with a
            construct whose name starts with `` `def' ``. They also include variables
            set with `set!` at top level in the file.

Several other languages are also supported:

* In Ada code, functions, procedures, packages, tasks, and types are
  tags. Use the `` `--packages-only' `` option to create tags for
  packages only.

  In Ada, the same name can be used for different kinds of entity
  (e.g., for a procedure and for a function). Also, for things like
  packages, procedures and functions, there is the spec (i.e. the
  interface) and the body (i.e. the implementation). To make it
  easier to pick the definition you want, Ada tag name have suffixes
  indicating the type of entity:

  `` `/b' ``: package body. `` `/f' ``: function. `` `/k' ``: task. `` `/p' ``: procedure. `` `/s' ``: package spec. `` `/t' ``: type.

  Thus, `M-x find-tag RET bidule/b RET` will go
  directly to the body of the package `bidule`, while `M-x
  find-tag RET bidule RET` will just search for any tag
  `bidule`.

  * In assembler code, labels appearing at the beginning of a line,
    followed by a colon, are tags.

    * In Bison or Yacc input files, each rule defines as a tag the nonterminal
      it constructs. The portions of the file that contain C code are parsed
      as C code.

      * In Cobol code, tags are paragraph names; that is, any word starting in
        column 8 and followed by a period.

        * In Erlang code, the tags are the functions, records, and macros defined
          in the file.

          * In Fortran code, functions, subroutines and block data are tags.

            * In makefiles, targets are tags.

              * In Objective C code, tags include Objective C definitions for classes,
                class categories, methods, and protocols.

                * In Pascal code, the tags are the functions and procedures defined in
                  the file.

                  * In Perl code, the tags are the procedures defined by the `sub`,
                    `my` and `local` keywords. Use `` `--globals' `` if you want
                    to tag global variables.

                    * In PostScript code, the tags are the functions.

                      * In Prolog code, a tag name appears at the left margin.

                        * In Python code, `def` or `class` at the beginning of a line
                          generate a tag.

You can also generate tags based on regexp matching (see section [W.2.3 Etags Regexps](emacs.html#SEC337)) to handle other formats and languages.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC335) ] | [ [>](emacs.html#SEC337) ] | [ [<<](emacs.html#SEC337) ] | [ [Up](emacs.html#SEC334) ] | [ [>>](emacs.html#SEC342) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### W.2.2 Creating Tags Tables

The `etags` program is used to create a tags table file. It knows
the syntax of several languages, as described in
[W.2.1 Source File Tag Syntax](emacs.html#SEC335).
Here is how to run `etags`:

|  |  |
| --- | --- |
|  | ``` etags inputfiles... ``` |

The `etags` program reads the specified files, and writes a tags
table named `TAGS' in the current working directory.

If the specified files don't exist, `etags` looks for
compressed versions of them and uncompresses them to read them. Under
MS-DOS, `etags` also looks for file names like `mycode.cgz'
if it is given `` `mycode.c' `` on the command line and `mycode.c'
does not exist.

`etags` recognizes the language used in an input file based on
its file name and contents. You can specify the language with the
`` `--language=name' `` option, described below.

If the tags table data become outdated due to changes in the files
described in the table, the way to update the tags table is the same
way it was made in the first place. If the tags table fails to record
a tag, or records it for the wrong file, then Emacs cannot possibly
find its definition until you update the tags table. However, if the
position recorded in the tags table becomes a little bit wrong (due to
other editing), the only consequence is a slight delay in finding the
tag. Even if the stored position is very far wrong, Emacs will still
find the tag, after searching most of the file for it. Even that
delay is hardly noticeable with today's computers.

So you should update a tags table when you define new tags that you want
to have listed, or when you move tag definitions from one file to another,
or when changes become substantial. Normally there is no need to update
the tags table after each edit, or even every day.

One tags table can virtually include another. Specify the included
tags file name with the `` `--include=file' `` option when
creating the file that is to include it. The latter file then acts as
if it covered all the source files specified in the included file, as
well as the files it directly contains.

If you specify the source files with relative file names when you run
`etags`, the tags file will contain file names relative to the
directory where the tags file was initially written. This way, you can
move an entire directory tree containing both the tags file and the
source files, and the tags file will still refer correctly to the source
files.

If you specify absolute file names as arguments to `etags`, then
the tags file will contain absolute file names. This way, the tags file
will still refer to the same files even if you move it, as long as the
source files remain in the same place. Absolute file names start with
`` `/' ``, or with `` `device:/' `` on MS-DOS and MS-Windows.

When you want to make a tags table from a great number of files, you
may have problems listing them on the command line, because some systems
have a limit on its length. The simplest way to circumvent this limit
is to tell `etags` to read the file names from its standard input,
by typing a dash in place of the file names, like this:

|  |  |
| --- | --- |
|  | ``` find . -name "*.[chCH]" -print | etags - ``` |

Use the option `` `--language=name' `` to specify the language
explicitly. You can intermix these options with file names; each one
applies to the file names that follow it. Specify
`` `--language=auto' `` to tell `etags` to resume guessing the
language from the file names and file contents. Specify
`` `--language=none' `` to turn off language-specific processing
entirely; then `etags` recognizes tags by regexp matching alone
(see section [W.2.3 Etags Regexps](emacs.html#SEC337)).

`` `etags --help' `` prints the list of the languages `etags`
knows, and the file name rules for guessing the language. It also prints
a list of all the available `etags` options, together with a short
explanation.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC336) ] | [ [>](emacs.html#SEC338) ] | [ [<<](emacs.html#SEC338) ] | [ [Up](emacs.html#SEC334) ] | [ [>>](emacs.html#SEC342) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### W.2.3 Etags Regexps

The `` `--regex' `` option provides a general way of recognizing tags
based on regexp matching. You can freely intermix it with file names.
Each `` `--regex' `` option adds to the preceding ones, and applies only
to the following files. The syntax is:

|  |  |
| --- | --- |
|  | ``` --regex=/tagregexp[/nameregexp]/ ``` |

where tagregexp is used to match the lines to tag. It is always
anchored, that is, it behaves as if preceded by `` `^' ``. If you want
to account for indentation, just match any initial number of blanks by
beginning your regular expression with `` `[ \t]*' ``. In the regular
expressions, `` `\' `` quotes the next character, and `` `\t' `` stands
for the tab character. Note that `etags` does not handle the other
C escape sequences for special characters.

The syntax of regular expressions in `etags` is the same as in
Emacs, augmented with the *interval operator*, which works as in
`grep` and `ed`. The syntax of an interval operator is
`` `\{m,n\}' ``, and its meaning is to match the preceding
expression at least m times and up to n times.

You should not match more characters with tagregexp than that
needed to recognize what you want to tag. If the match is such that
more characters than needed are unavoidably matched by tagregexp
(as will usually be the case), you should add a nameregexp, to
pick out just the tag. This will enable Emacs to find tags more
accurately and to do completion on tag names more reliably. You can
find some examples below.

The option `` `--ignore-case-regex' `` (or `` `-c' ``) works like
`` `--regex' ``, except that matching ignores case. This is
appropriate for certain programming languages.

The `` `-R' `` option deletes all the regexps defined with
`` `--regex' `` options. It applies to the file names following it, as
you can see from the following example:

|  |  |
| --- | --- |
|  | ``` etags --regex=/reg1/ voo.doo --regex=/reg2/ \     bar.ber -R --lang=lisp los.er ``` |

Here `etags` chooses the parsing language for `voo.doo' and
`bar.ber' according to their contents. `etags` also uses
reg1 to recognize additional tags in `voo.doo', and both
reg1 and reg2 to recognize additional tags in
`bar.ber'. `etags` uses the Lisp tags rules, and no regexp
matching, to recognize tags in `los.er'.

You can specify a regular expression for a particular language, by
writing `` `{lang}' `` in front of it. Then `etags` will use
the regular expression only for files of that language. (`` `etags
--help' `` prints the list of languages recognized by `etags`.) The
following example tags the `DEFVAR` macros in the Emacs source
files, for the C language only:

|  |  |
| --- | --- |
|  | ``` --regex='{c}/[ \t]*DEFVAR_[A-Z_ \t(]+"\([^"]+\)"/' ``` |

This feature is particularly useful when you store a list of regular
expressions in a file. The following option syntax instructs
`etags` to read two files of regular expressions. The regular
expressions contained in the second file are matched without regard to
case.

|  |  |
| --- | --- |
|  | ``` --regex=@first-file --ignore-case-regex=@second-file ``` |

A regex file contains one regular expressions per line. Empty lines,
and lines beginning with space or tab are ignored. When the first
character in a line is `` `@' ``, `etags` assumes that the rest
of the line is the name of a file of regular expressions; thus, one
such file can include another file. All the other lines are taken to
be regular expressions. If the first non-whitespace text on the line
is `` `--' ``, that line is a comment.

For example, one can create a file called `` `emacs.tags' `` with the
following contents:

|  |  |
| --- | --- |
|  | ```         -- This is for GNU Emacs C source files {c}/[ \t]*DEFVAR_[A-Z_ \t(]+"\([^"]+\)"/\1/ ``` |

and then use it like this:

|  |  |
| --- | --- |
|  | ``` etags --regex=@emacs.tags *.[ch] */*.[ch] ``` |

Here are some more examples. The regexps are quoted to protect them
from shell interpretation.

* Tag Octave files:

  |  |  |
  | --- | --- |
  |  | ``` etags --language=none \       --regex='/[ \t]*function.*=[ \t]*\([^ \t]*\)[ \t]*(/\1/' \       --regex='/###key \(.*\)/\1/' \       --regex='/[ \t]*global[ \t].*/' \       *.m ``` |

  Note that tags are not generated for scripts, so that you have to add
  a line by yourself of the form `` `###key scriptname' `` if you
  want to jump to it.

  * Tag Tcl files:

    |  |  |
    | --- | --- |
    |  | ``` etags --language=none --regex='/proc[ \t]+\([^ \t]+\)/\1/' *.tcl ``` |

    * Tag VHDL files:

      |  |  |
      | --- | --- |
      |  | ``` etags --language=none \   --regex='/[ \t]*\(ARCHITECTURE\|CONFIGURATION\) +[^ ]* +OF/' \   --regex='/[ \t]*\(ATTRIBUTE\|ENTITY\|FUNCTION\|PACKAGE\   \( BODY\)?\|PROCEDURE\|PROCESS\|TYPE\)[ \t]+\([^ \t(]+\)/\3/' ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC337) ] | [ [>](emacs.html#SEC339) ] | [ [<<](emacs.html#SEC339) ] | [ [Up](emacs.html#SEC334) ] | [ [>>](emacs.html#SEC342) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### W.2.4 Selecting a Tags Table

Emacs has at any time one *selected* tags table, and all the commands
for working with tags tables use the selected one. To select a tags table,
type `M-x visit-tags-table`, which reads the tags table file name as an
argument. The name `TAGS' in the default directory is used as the
default file name.

All this command does is store the file name in the variable
`tags-file-name`. Emacs does not actually read in the tags table
contents until you try to use them. Setting this variable yourself is just
as good as using `visit-tags-table`. The variable's initial value is
`nil`; that value tells all the commands for working with tags tables
that they must ask for a tags table file name to use.

Using `visit-tags-table` when a tags table is already loaded
gives you a choice: you can add the new tags table to the current list
of tags tables, or start a new list. The tags commands use all the tags
tables in the current list. If you start a new list, the new tags table
is used *instead* of others. If you add the new table to the
current list, it is used *as well as* the others. When the tags
commands scan the list of tags tables, they don't always start at the
beginning of the list; they start with the first tags table (if any)
that describes the current file, proceed from there to the end of the
list, and then scan from the beginning of the list until they have
covered all the tables in the list.

You can specify a precise list of tags tables by setting the variable
`tags-table-list` to a list of strings, like this:

|  |  |
| --- | --- |
|  | ``` (setq tags-table-list       '("~/emacs" "/usr/local/lib/emacs/src")) ``` |

This tells the tags commands to look at the `TAGS' files in your
`~/emacs' directory and in the `/usr/local/lib/emacs/src'
directory. The order depends on which file you are in and which tags
table mentions that file, as explained above.

Do not set both `tags-file-name` and `tags-table-list`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC338) ] | [ [>](emacs.html#SEC340) ] | [ [<<](emacs.html#SEC340) ] | [ [Up](emacs.html#SEC334) ] | [ [>>](emacs.html#SEC342) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### W.2.5 Finding a Tag

The most important thing that a tags table enables you to do is to find
the definition of a specific tag.

`M-. tag RET`: Find first definition of tag (`find-tag`). `C-u M-.`: Find next alternate definition of last tag specified. `C-u - M-.`: Go back to previous tag found. `C-M-. pattern RET`: Find a tag whose name matches pattern (`find-tag-regexp`). `C-u C-M-.`: Find the next tag whose name matches the last pattern used. `C-x 4 . tag RET`: Find first definition of tag, but display it in another window (`find-tag-other-window`). `C-x 5 . tag RET`: Find first definition of tag, and create a new frame to select the buffer (`find-tag-other-frame`). `M-*`: Pop back to where you previously invoked `M-.` and friends.

`M-.` (`find-tag`) is the command to find the definition of
a specified tag. It searches through the tags table for that tag, as a
string, and then uses the tags table info to determine the file that the
definition is in and the approximate character position in the file of
the definition. Then `find-tag` visits that file, moves point to
the approximate character position, and searches ever-increasing
distances away to find the tag definition.

If an empty argument is given (just type `RET`), the balanced
expression in the buffer before or around point is used as the
tag argument. See section [U.4.1 Expressions with Balanced Parentheses](emacs.html#SEC282).

You don't need to give `M-.` the full name of the tag; a part
will do. This is because `M-.` finds tags in the table which
contain tag as a substring. However, it prefers an exact match
to a substring match. To find other tags that match the same
substring, give `find-tag` a numeric argument, as in `C-u
M-.`; this does not read a tag name, but continues searching the tags
table's text for another tag containing the same substring last used.
If you have a real `META` key, `M-0 M-.` is an easier
alternative to `C-u M-.`.

Like most commands that can switch buffers, `find-tag` has a
variant that displays the new buffer in another window, and one that
makes a new frame for it. The former is `C-x 4 .`, which invokes
the command `find-tag-other-window`. The latter is `C-x 5 .`,
which invokes `find-tag-other-frame`.

To move back to places you've found tags recently, use `C-u -
M-.`; more generally, `M-.` with a negative numeric argument. This
command can take you to another buffer. `C-x 4 .` with a negative
argument finds the previous tag location in another window.

As well as going back to places you've found tags recently, you can go
back to places *from where* you found them. Use `M-*`, which
invokes the command `pop-tag-mark`, for this. Typically you would
find and study the definition of something with `M-.` and then
return to where you were with `M-*`.

Both `C-u - M-.` and `M-*` allow you to retrace your steps to
a depth determined by the variable `find-tag-marker-ring-length`.

The command `C-M-.` (`find-tag-regexp`) visits the tags that
match a specified regular expression. It is just like `M-.` except
that it does regexp matching instead of substring matching.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC339) ] | [ [>](emacs.html#SEC341) ] | [ [<<](emacs.html#SEC341) ] | [ [Up](emacs.html#SEC334) ] | [ [>>](emacs.html#SEC342) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### W.2.6 Searching and Replacing with Tags Tables

The commands in this section visit and search all the files listed in the
selected tags table, one by one. For these commands, the tags table serves
only to specify a sequence of files to search.

`M-x tags-search RET regexp RET`: Search for regexp through the files in the selected tags table. `M-x tags-query-replace RET regexp RET replacement RET`: Perform a `query-replace-regexp` on each file in the selected tags table. `M-,`: Restart one of the commands above, from the current location of point (`tags-loop-continue`).

`M-x tags-search` reads a regexp using the minibuffer, then
searches for matches in all the files in the selected tags table, one
file at a time. It displays the name of the file being searched so you
can follow its progress. As soon as it finds an occurrence,
`tags-search` returns.

Having found one match, you probably want to find all the rest. To find
one more match, type `M-,` (`tags-loop-continue`) to resume the
`tags-search`. This searches the rest of the current buffer, followed
by the remaining files of the tags table.

`M-x tags-query-replace` performs a single
`query-replace-regexp` through all the files in the tags table. It
reads a regexp to search for and a string to replace with, just like
ordinary `M-x query-replace-regexp`. It searches much like `M-x
tags-search`, but repeatedly, processing matches according to your
input. See section [K.7 Replacement Commands](emacs.html#SEC99), for more information on query replace.

You can control the case-sensitivity of tags search commands by
customizing the value of the variable `tags-case-fold-search`. The
default is to use the same setting as the value of
`case-fold-search` (see section [K.6 Searching and Case](emacs.html#SEC98)).

It is possible to get through all the files in the tags table with a
single invocation of `M-x tags-query-replace`. But often it is
useful to exit temporarily, which you can do with any input event that
has no special query replace meaning. You can resume the query replace
subsequently by typing `M-,`; this command resumes the last tags
search or replace command that you did.

The commands in this section carry out much broader searches than the
`find-tag` family. The `find-tag` commands search only for
definitions of tags that match your substring or regexp. The commands
`tags-search` and `tags-query-replace` find every occurrence
of the regexp, as ordinary search commands and replace commands do in
the current buffer.

These commands create buffers only temporarily for the files that they
have to search (those which are not already visited in Emacs buffers).
Buffers in which no match is found are quickly killed; the others
continue to exist.

It may have struck you that `tags-search` is a lot like
`grep`. You can also run `grep` itself as an inferior of
Emacs and have Emacs show you the matching lines one by one. This works
much like running a compilation; finding the source locations of the
`grep` matches works like finding the compilation errors.
See section [V.1 Running Compilations under Emacs](emacs.html#SEC317).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC340) ] | [ [>](emacs.html#SEC342) ] | [ [<<](emacs.html#SEC342) ] | [ [Up](emacs.html#SEC334) ] | [ [>>](emacs.html#SEC342) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### W.2.7 Tags Table Inquiries

`M-x list-tags RET file RET`: Display a list of the tags defined in the program file file. `M-x tags-apropos RET regexp RET`: Display a list of all tags matching regexp.

`M-x list-tags` reads the name of one of the files described by
the selected tags table, and displays a list of all the tags defined in
that file. The "file name" argument is really just a string to
compare against the file names recorded in the tags table; it is read as
a string rather than as a file name. Therefore, completion and
defaulting are not available, and you must enter the file name the same
way it appears in the tags table. Do not include a directory as part of
the file name unless the file name recorded in the tags table includes a
directory.

`M-x tags-apropos` is like `apropos` for tags
(see section [G.4 Apropos](emacs.html#SEC46)). It finds all the tags in the selected tags table
whose entries match regexp, and displays them. If the variable
`tags-apropos-verbose` is non-`nil`, it displays the names
of the tags files together with the tag names.

You can customize the appearance of the output with the face
`tags-tag-face`. You can display additional output with `M-x
tags-apropos` by customizing the variable
`tags-apropos-additional-actions`---see its documentation for
details.

You can also use the collection of tag names to complete a symbol
name in the buffer. See section [U.8 Completion for Symbol Names](emacs.html#SEC294).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC341) ] | [ [>](emacs.html#SEC343) ] | [ [<<](emacs.html#SEC332) ] | [ [Up](emacs.html#SEC332) ] | [ [>>](emacs.html#SEC350) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## W.3 Merging Files with Emerge

It's not unusual for programmers to get their signals crossed and modify
the same program in two different directions. To recover from this
confusion, you need to merge the two versions. Emerge makes this
easier. See also [M.9 Comparing Files](emacs.html#SEC164), for commands to compare
in a more manual fashion, and section `Top' in The Ediff Manual.

> |  |  |  |
> | --- | --- | --- |
> | [W.3.1 Overview of Emerge](emacs.html#SEC343) |  | How to start Emerge. Basic concepts. |
> | [W.3.2 Submodes of Emerge](emacs.html#SEC344) |  | Fast mode vs. Edit mode. Skip Prefers mode and Auto Advance mode. |
> | [W.3.3 State of a Difference](emacs.html#SEC345) |  | You do the merge by specifying state A or B for each difference. |
> | [W.3.4 Merge Commands](emacs.html#SEC346) |  | Commands for selecting a difference, changing states of differences, etc. |
> | [W.3.5 Exiting Emerge](emacs.html#SEC347) |  | What to do when you've finished the merge. |
> | [W.3.6 Combining the Two Versions](emacs.html#SEC348) |  | How to keep both alternatives for a difference. |
> | [W.3.7 Fine Points of Emerge](emacs.html#SEC349) |  | Misc. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC342) ] | [ [>](emacs.html#SEC344) ] | [ [<<](emacs.html#SEC332) ] | [ [Up](emacs.html#SEC342) ] | [ [>>](emacs.html#SEC350) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### W.3.1 Overview of Emerge

To start Emerge, run one of these four commands:

`M-x emerge-files`: Merge two specified files. `M-x emerge-files-with-ancestor`: Merge two specified files, with reference to a common ancestor. `M-x emerge-buffers`: Merge two buffers. `M-x emerge-buffers-with-ancestor`: Merge two buffers with reference to a common ancestor in a third buffer.

The Emerge commands compare two files or buffers, and display the
comparison in three buffers: one for each input text (the *A buffer*
and the *B buffer*), and one (the *merge buffer*) where merging
takes place. The merge buffer shows the full merged text, not just the
differences. Wherever the two input texts differ, you can choose which
one of them to include in the merge buffer.

The Emerge commands that take input from existing buffers use only the
accessible portions of those buffers, if they are narrowed
(see section [AC.22 Narrowing](emacs.html#SEC462)).

If a common ancestor version is available, from which the two texts to
be merged were both derived, Emerge can use it to guess which
alternative is right. Wherever one current version agrees with the
ancestor, Emerge presumes that the other current version is a deliberate
change which should be kept in the merged version. Use the
`` `with-ancestor' `` commands if you want to specify a common ancestor
text. These commands read three file or buffer names--variant A,
variant B, and the common ancestor.

After the comparison is done and the buffers are prepared, the
interactive merging starts. You control the merging by typing special
*merge commands* in the merge buffer. The merge buffer shows you a
full merged text, not just differences. For each run of differences
between the input texts, you can choose which one of them to keep, or
edit them both together.

The merge buffer uses a special major mode, Emerge mode, with commands
for making these choices. But you can also edit the buffer with
ordinary Emacs commands.

At any given time, the attention of Emerge is focused on one
particular difference, called the *selected* difference. This
difference is marked off in the three buffers like this:

|  |  |
| --- | --- |
|  | ``` vvvvvvvvvvvvvvvvvvvv text that differs ^^^^^^^^^^^^^^^^^^^^ ``` |

Emerge numbers all the differences sequentially and the mode
line always shows the number of the selected difference.

Normally, the merge buffer starts out with the A version of the text.
But when the A version of a difference agrees with the common ancestor,
then the B version is initially preferred for that difference.

Emerge leaves the merged text in the merge buffer when you exit. At
that point, you can save it in a file with `C-x C-w`. If you give a
numeric argument to `emerge-files` or
`emerge-files-with-ancestor`, it reads the name of the output file
using the minibuffer. (This is the last file name those commands read.)
Then exiting from Emerge saves the merged text in the output file.

Normally, Emerge commands save the output buffer in its file when you
exit. If you abort Emerge with `C-]`, the Emerge command does not
save the output buffer, but you can save it yourself if you wish.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC343) ] | [ [>](emacs.html#SEC345) ] | [ [<<](emacs.html#SEC345) ] | [ [Up](emacs.html#SEC342) ] | [ [>>](emacs.html#SEC350) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### W.3.2 Submodes of Emerge

You can choose between two modes for giving merge commands: Fast mode
and Edit mode. In Fast mode, basic merge commands are single
characters, but ordinary Emacs commands are disabled. This is
convenient if you use only merge commands. In Edit mode, all merge
commands start with the prefix key `C-c C-c`, and the normal Emacs
commands are also available. This allows editing the merge buffer, but
slows down Emerge operations.

Use `e` to switch to Edit mode, and `C-c C-c f` to switch to
Fast mode. The mode line indicates Edit and Fast modes with `` `E' ``
and `` `F' ``.

Emerge has two additional submodes that affect how particular merge
commands work: Auto Advance mode and Skip Prefers mode.

If Auto Advance mode is in effect, the `a` and `b` commands
advance to the next difference. This lets you go through the merge
faster as long as you simply choose one of the alternatives from the
input. The mode line indicates Auto Advance mode with `` `A' ``.

If Skip Prefers mode is in effect, the `n` and `p` commands
skip over differences in states prefer-A and prefer-B (see section [W.3.3 State of a Difference](emacs.html#SEC345)). Thus you see only differences for which neither version
is presumed "correct." The mode line indicates Skip Prefers mode with
`` `S' ``.

Use the command `s a` (`emerge-auto-advance-mode`) to set or
clear Auto Advance mode. Use `s s`
(`emerge-skip-prefers-mode`) to set or clear Skip Prefers mode.
These commands turn on the mode with a positive argument, turns it off
with a negative or zero argument, and toggle the mode with no argument.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC344) ] | [ [>](emacs.html#SEC346) ] | [ [<<](emacs.html#SEC346) ] | [ [Up](emacs.html#SEC342) ] | [ [>>](emacs.html#SEC350) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### W.3.3 State of a Difference

In the merge buffer, a difference is marked with lines of `` `v' `` and
`` `^' `` characters. Each difference has one of these seven states:

A: The difference is showing the A version. The `a` command always produces this state; the mode line indicates it with `` `A' ``. B: The difference is showing the B version. The `b` command always produces this state; the mode line indicates it with `` `B' ``. default-A: default-B: The difference is showing the A or the B state by default, because you haven't made a choice. All differences start in the default-A state (and thus the merge buffer is a copy of the A buffer), except those for which one alternative is "preferred" (see below). When you select a difference, its state changes from default-A or default-B to plain A or B. Thus, the selected difference never has state default-A or default-B, and these states are never displayed in the mode line. The command `d a` chooses default-A as the default state, and `d b` chooses default-B. This chosen default applies to all differences which you haven't ever selected and for which no alternative is preferred. If you are moving through the merge sequentially, the differences you haven't selected are those following the selected one. Thus, while moving sequentially, you can effectively make the A version the default for some sections of the merge buffer and the B version the default for others by using `d a` and `d b` between sections. prefer-A: prefer-B: The difference is showing the A or B state because it is *preferred*. This means that you haven't made an explicit choice, but one alternative seems likely to be right because the other alternative agrees with the common ancestor. Thus, where the A buffer agrees with the common ancestor, the B version is preferred, because chances are it is the one that was actually changed. These two states are displayed in the mode line as `` `A*' `` and `` `B*' ``. combined: The difference is showing a combination of the A and B states, as a result of the `x c` or `x C` commands. Once a difference is in this state, the `a` and `b` commands don't do anything to it unless you give them a numeric argument. The mode line displays this state as `` `comb' ``.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC345) ] | [ [>](emacs.html#SEC347) ] | [ [<<](emacs.html#SEC347) ] | [ [Up](emacs.html#SEC342) ] | [ [>>](emacs.html#SEC350) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### W.3.4 Merge Commands

Here are the Merge commands for Fast mode; in Edit mode, precede them
with `C-c C-c`:

`p`: Select the previous difference. `n`: Select the next difference. `a`: Choose the A version of this difference. `b`: Choose the B version of this difference. `C-u n j`: Select difference number n. `.`: Select the difference containing point. You can use this command in the merge buffer or in the A or B buffer. `q`: Quit--finish the merge. `C-]`: Abort--exit merging and do not save the output. `f`: Go into Fast mode. (In Edit mode, this is actually `C-c C-c f`.) `e`: Go into Edit mode. `l`: Recenter (like `C-l`) all three windows. `-`: Specify part of a prefix numeric argument. `digit`: Also specify part of a prefix numeric argument. `d a`: Choose the A version as the default from here down in the merge buffer. `d b`: Choose the B version as the default from here down in the merge buffer. `c a`: Copy the A version of this difference into the kill ring. `c b`: Copy the B version of this difference into the kill ring. `i a`: Insert the A version of this difference at point. `i b`: Insert the B version of this difference at point. `m`: Put point and mark around the difference. `^`: Scroll all three windows down (like `M-v`). `v`: Scroll all three windows up (like `C-v`). `<`: Scroll all three windows left (like `C-x <`). `>`: Scroll all three windows right (like `C-x >`). `|`: Reset horizontal scroll on all three windows. `x 1`: Shrink the merge window to one line. (Use `C-u l` to restore it to full size.) `x c`: Combine the two versions of this difference (see section [W.3.6 Combining the Two Versions](emacs.html#SEC348)). `x f`: Show the names of the files/buffers Emerge is operating on, in a Help window. (Use `C-u l` to restore windows.) `x j`: Join this difference with the following one. (`C-u x j` joins this difference with the previous one.) `x s`: Split this difference into two differences. Before you use this command, position point in each of the three buffers at the place where you want to split the difference. `x t`: Trim identical lines off the top and bottom of the difference. Such lines occur when the A and B versions are identical but differ from the ancestor version.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC346) ] | [ [>](emacs.html#SEC348) ] | [ [<<](emacs.html#SEC348) ] | [ [Up](emacs.html#SEC342) ] | [ [>>](emacs.html#SEC350) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### W.3.5 Exiting Emerge

The `q` command (`emerge-quit`) finishes the merge, storing
the results into the output file if you specified one. It restores the
A and B buffers to their proper contents, or kills them if they were
created by Emerge and you haven't changed them. It also disables the
Emerge commands in the merge buffer, since executing them later could
damage the contents of the various buffers.

`C-]` aborts the merge. This means exiting without writing the
output file. If you didn't specify an output file, then there is no
real difference between aborting and finishing the merge.

If the Emerge command was called from another Lisp program, then its
return value is `t` for successful completion, or `nil` if you
abort.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC347) ] | [ [>](emacs.html#SEC349) ] | [ [<<](emacs.html#SEC349) ] | [ [Up](emacs.html#SEC342) ] | [ [>>](emacs.html#SEC350) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### W.3.6 Combining the Two Versions

Sometimes you want to keep *both* alternatives for a particular
difference. To do this, use `x c`, which edits the merge buffer
like this:

|  |  |
| --- | --- |
|  | ``` #ifdef NEW version from A buffer #else /* not NEW */ version from B buffer #endif /* not NEW */ ``` |

While this example shows C preprocessor conditionals delimiting the two
alternative versions, you can specify the strings to use by setting
the variable `emerge-combine-versions-template` to a string of your
choice. In the string, `` `%a' `` says where to put version A, and
`` `%b' `` says where to put version B. The default setting, which
produces the results shown above, looks like this:

|  |  |
| --- | --- |
|  | ``` "#ifdef NEW\n%a#else /* not NEW */\n%b#endif /* not NEW */\n" ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC348) ] | [ [>](emacs.html#SEC350) ] | [ [<<](emacs.html#SEC332) ] | [ [Up](emacs.html#SEC342) ] | [ [>>](emacs.html#SEC350) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
