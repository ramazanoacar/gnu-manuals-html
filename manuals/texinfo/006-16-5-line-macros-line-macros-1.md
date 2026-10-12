### 16.5 Line Macros [¶](#Line-Macros-1)

You can also define a macro using the ‘`@linemacro`’ command.
The syntax for line macro definitions is similar to that for
`@macro`.

However, the syntax for using a macro defined this way is different.
The macro call uses the rest of the line, with arguments separated
by spaces. The line macro facility is mainly intended to allow you
to define commands that operate similarly to the builtin commands
for providing definitions (see [Definition Commands](#Definition-Commands)).

For example:

```
@linemacro defbuiltin {name, args}
@defline {Builtin} \name\ \args\
@end linemacro
```

The above macro may be used as

```
@defblock
@defbuiltin foo (bar)
Explanation
@end defblock
```

This produces the following result:

Builtin: **foo** (bar)
:   Explanation

Here the first argument is ‘`foo`’, and is substituted where
‘`\name\`’ appears in the macro body. Likewise, ‘`(bar)`’
is substituted for ‘`\args\`’.

You can include spaces in a macro argument by surrounding the argument
by braces. Any pair of braces enclosing an argument is removed
before substitution. Any empty arguments (including the last one)
must be given as ‘`{}`’. Additionally, non-initial spaces may
appear in the final argument in the argument list without surrounding
braces (as in the example below).

If an argument line for a line macro ends in a ‘`@`’ character,
then this character together with the following newline are included
in a macro argument, and the following line in the input file is also
used to get the arguments for the macro. Note that any substitution
of the ‘`@<newline>`’ sequence must be in a valid context,
such as a definition line (see [Definition Command Continuation Lines](#Def-Cmd-Continuation-Lines)).

Another example:

```
@linemacro deffunc {type, name, args}
@findex \name\
@deftypeline {Func} {\type\} \name\ \args\
@end linemacro

@defblock
@deffunc {long int} F (int @var{one}, int @var{two}, @
int @var{three}, int @var{four}, int @var{five})
Explanation
@end defblock
```

Note how braces are needed around ‘`\type\`’ in the
macro body in case the `type` argument contains a space.
This example produces the output below and enters an index entry
for `F`.

Func: `long int` **F** `(int one, int two, int three, int four, int five)`
:   Explanation

Line macros produce a whole number of lines of output in their expansion.
There may be better compatibility between the Texinfo processors
(`texi2any` and TeX with `texinfo.tex`) for macros defined
with `@linemacro` than for those defined with `@macro`.

---

Next: [External Macro Processors: Line Directives](#External-Macro-Processors), Previous: [Line Macros](#Line-Macros), Up: [Defining New Texinfo Commands](#Defining-New-Texinfo-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 16.6 `@definfoenclose`: Customized Highlighting [¶](#g_t_0040definfoenclose_003a-Customized-Highlighting)

An `@definfoenclose` command may be used to define a
highlighting command for online output formats. A command
defined using `@definfoenclose` marks text by enclosing it in
strings that precede and follow the text.

In practice, there is little use for this command, and we do not
recommend you use it. Support for `@definfoenclose` may be
removed in future releases of Texinfo.

Write a `@definfoenclose` command at the beginning of a line
followed by three comma-separated arguments. The first argument to
`@definfoenclose` is the @-command name (without the
`@`); the second argument is the start delimiter string; and the
third argument is the end delimiter string. The latter two arguments
enclose the highlighted text in the output.

A delimiter string may contain spaces. Neither the start nor end
delimiter is required. If you do not want a start delimiter but do
want an end delimiter, you must follow the command name with two
commas in a row; otherwise, the end delimiter string you intended will
naturally be (mis)interpreted as the start delimiter string.

An enclosure command defined this way takes one argument in braces,
since it is intended for new markup commands (see [Marking Text, Words and Phrases](#Marking-Text)).

For example, you can write:

```
@definfoenclose phoo,//,\\
```

near the beginning of a Texinfo file to define `@phoo` as an Info
and HTML formatting command that inserts ‘//’ before and ‘\\’ after the
argument to `@phoo`. You can then write `@phoo{bar}`
wherever you want ‘//bar\\’ highlighted in Info and HTML.

For TeX formatting, you could write

```
@iftex
@alias phoo = i
@end iftex
```

to define `@phoo` as a command that causes TeX to typeset the
argument to `@phoo` in italics.

Each definition applies to its own formatter: one for TeX, the
other for online formats. The TeX definitions need to be in
‘`@iftex`’. `@definfoenclose` command need not be within
‘`@ifinfo`’ unless you want to use different definitions for
different online output formats. `@definfoenclose` defined
commands have no effect in DocBook and LaTeX output, their
argument is output as-is. An `@alias` could also be used
for these formats.

`@definfoenclose` definitions must not be recursive, directly or
indirectly.

---

Previous: [`@definfoenclose`: Customized Highlighting](#g_t_0040definfoenclose), Up: [Defining New Texinfo Commands](#Defining-New-Texinfo-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 16.7 External Macro Processors: Line Directives [¶](#External-Macro-Processors_003a-Line-Directives)

Texinfo macros (and its other text substitution facilities) work fine
in straightforward cases. If your document needs unusually complex
processing, however, their fragility and limitations can be a problem.
In this case, you may want to use a different macro processor
altogether, such as M4 (see [M4](https://www.gnu.org/software/m4/manual/m4.html#Top)) or CPP (see [The C Preprocessor](https://gcc.gnu.org/onlinedocs/cpp/index.html#Top)).

With one exception, Texinfo does not need to know whether its input is
“original” source or preprocessed from some other source file.
Therefore, you can arrange your build system to invoke whatever
programs you like to handle macro expansion or other preprocessing
needs. Texinfo does not offer built-in support for any particular
preprocessor, since no one program seemed likely to suffice for the
requirements of all documents.

The one exception is line numbers in error messages. In that case,
the line number should refer to the original source file, whatever it
may be. There’s a well-known mechanism for this: the so-called
‘`#line`’ directive. Texinfo supports this.

* [‘`#line`’ Directive](#g_t_0023line-Directive)
* [‘`#line`’ and TeX](#g_t_0023line-and-TeX)
* [‘`#line`’ Syntax Details](#g_t_0023line-Syntax-Details)

---

Next: [‘`#line`’ and TeX](#g_t_0023line-and-TeX), Up: [External Macro Processors: Line Directives](#External-Macro-Processors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 16.7.1 ‘`#line`’ Directive [¶](#g_t_0023line-Directive-1)

An input line such as this:

```
#line 100 "foo.ptexi"
```

indicates that the next line was line 100 of the file
`foo.ptexi`, and so that’s what an error message should refer to.
Both M4 (see [Preprocessor features](https://www.gnu.org/software/m4/manual/m4.html#Preprocessor-features) in GNU M4) and CPP
(see [Line Control](https://gcc.gnu.org/onlinedocs/cpp/Line-Control.html#Line-Control) in The C Preprocessor, and
[Preprocessor Output](https://gcc.gnu.org/onlinedocs/cpp/Preprocessor-Output.html#Preprocessor-Output) in The C Preprocessor) can generate
such lines.

The `texi2any` program recognizes these lines by default,
except within `@verbatim` blocks (see [`@verbatim`: Literal Text](#g_t_0040verbatim)).
Their recognition can be turned off completely with
`CPP_LINE_DIRECTIVES` (see [Other Customization Variables](#Other-Customization-Variables)),
though there is normally no reason to do so.

For those few programs (M4, CPP, Texinfo) which need to document
‘`#line`’ directives and therefore have examples which would
otherwise match the pattern, the command `@hashchar{}` can be
used (see [Inserting ‘#’ with `@hashchar{}`](#Inserting-a-Hashsign)). The example line above looks
like this in the source for this manual:

```
@hashchar{}line 100 "foo.ptexi"
```

The `@hashchar` command was added to Texinfo in 2013. If you
don’t want to rely on it, you can also use `@set` and
`@value` to insert the literal ‘`#`’:

```
@set hash #
@value{hash}line 1 "example.c"
```

Or, if suitable, a `@verbatim` environment can be used instead
of `@example`. As mentioned above, `#line`-recognition is
disabled inside verbatim blocks.

---

Next: [‘`#line`’ Syntax Details](#g_t_0023line-Syntax-Details), Previous: [‘`#line`’ Directive](#g_t_0023line-Directive), Up: [External Macro Processors: Line Directives](#External-Macro-Processors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 16.7.2 ‘`#line`’ and TeX [¶](#g_t_0023line-and-TeX-1)

As mentioned, `texi2any` recognizes the ‘`#line`’
directives described in the previous section. However,
`texinfo.tex` does not and cannot. Therefore, such a line will
be incorrectly typeset verbatim if TeX sees it. The solution is to
use `texi2any`’s macro expansion options before running
TeX. There are three approaches:

* If you run `texi2dvi` or its variants (see [Format with `texi2dvi`](#Format-with-texi2dvi)), you can pass `-E` and `texi2dvi`
  will run `texi2any` first to expand macros and eliminate
  ‘`#line`’.
* If you run `texi2any`, you can specify `--no-ifinfo
  --iftex -E somefile.out`, and then give `somefile.out` to
  `texi2dvi` in a separate command.
* Or you can run `texi2any --dvi --Xopt -E`. (Or `--pdf`
  instead of `--dvi`.) `texi2any` will then call
  `texi2dvi -E`.

One last caveat regarding use with TeX: since the `#line`
directives are not recognized, the line numbers emitted by the
`@errormsg{}` command (see [Conditional Commands](#Conditional-Commands)), or by
TeX itself, are the (incorrect) line numbers from the derived file
which TeX is reading, rather than the preprocessor-specified line
numbers.

---

Previous: [‘`#line`’ and TeX](#g_t_0023line-and-TeX), Up: [External Macro Processors: Line Directives](#External-Macro-Processors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 16.7.3 ‘`#line`’ Syntax Details [¶](#g_t_0023line-Syntax-Details-1)

Syntax details for the ‘`#line`’ directive: the ‘`#`’ character
can be preceded or followed by whitespace, the word ‘`line`’ is
optional, and the file name can be followed by a whitespace-separated
list of integers (these are so-called “flags” output by CPP in some
cases). For those who like to know the gory details, the actual
(Perl) regular expression which is matched is this:

```
/^\s*#\s*(line)? (\d+)(( "([^"]+)")(\s+\d+)*)?\s*$/
```

As far as we’ve been able to tell, the trailing integer flags only
occur in conjunction with a file name, so that is reflected in the
regular expression.

As an example, the following is a syntactically valid ‘`#line`’
directive, meaning line 1 of `/usr/include/stdio.h`:

```
# 1 "/usr/include/stdio.h" 2 3 4
```

Unfortunately, the quoted file name (‘`"..."`’) has to be optional,
because M4 (especially) can often generate ‘`#line`’ directives
within a single file. Since the ‘`line`’ is also optional, the
result is that lines might match which you wouldn’t expect, e.g.,

```
# 1
```

The possible solutions are described above (see [‘`#line`’ Directive](#g_t_0023line-Directive)).

---

Next: [Formatting and Printing with TeX](#Hardcopy-with-TeX), Previous: [Defining New Texinfo Commands](#Defining-New-Texinfo-Commands), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 17 Include Files [¶](#Include-Files-1)

When a Texinfo processor sees an `@include` command in a Texinfo
file, it processes the contents of the file named by the
`@include` and incorporates them into the output files being
created. Include files thus let you keep a single large document as a
collection of conveniently small parts.

* [How to Use Include Files](#Using-Include-Files)
* [Sample File with `@include`](#Sample-Include-File)
* [`@verbatiminclude` file: Include a File Verbatim](#g_t_0040verbatiminclude)

---

Next: [Sample File with `@include`](#Sample-Include-File), Up: [Include Files](#Include-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 17.1 How to Use Include Files [¶](#How-to-Use-Include-Files)

To include another file within a Texinfo file, write the
`@include` command at the beginning of a line and follow it on
the same line by the name of a file to be included. For example:

```
@include buffers.texi
```

`@value{var}` references are expanded on the `@include`
line. Other than that, the only @-commands allowed are `@@`,
`@{`, `@}` and associated @-commands such as
`@atchar{}`.

An included file should simply be a segment of text that you expect to
be included as is into the overall or *outer* Texinfo file; it
should not contain the standard beginning and end parts of a Texinfo
file. In particular, you should not start an included file with a
line saying ‘`\input texinfo`’; if you do, that text is inserted
into the output file literally. Likewise, you should not end an
included file with a `@bye` command; nothing after `@bye`
is formatted.

In the long-ago past, you were required to write an
`@setfilename` line at the beginning of an included file, but no
longer. Now, it does not matter whether you write such a line. If an
`@setfilename` line exists in an included file, it is ignored.

GNU Emacs Texinfo mode provides `texinfo-multiple-files-update`
to update node pointers and master menu with multiple include files.
See [Update Outer File and Include Files](#Update-Multiple-Files).

---

Next: [`@verbatiminclude` file: Include a File Verbatim](#g_t_0040verbatiminclude), Previous: [How to Use Include Files](#Using-Include-Files), Up: [Include Files](#Include-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 17.2 Sample File with `@include` [¶](#Sample-File-with-_0040include)

Here is an example of an outer Texinfo file with `@include` files
within it:

```
\input texinfo @c -*-texinfo-*-

@settitle Include Example
```

```

```

```
@node Top
@top Include Example
```

```

```

```
@include foo.texi
@include bar.texi
@include concept-index.texi
@bye
```

An included file, such as `foo.texi`, might look like this:

```
@node First
@chapter First Chapter

Contents of first chapter ...
```

The full contents of `concept-index.texi` might be as simple as this:

```
@node Concept Index
@unnumbered Concept Index

@printindex cp
```

The outer Texinfo source file for The GNU Emacs Lisp Reference
Manual is named `elisp.texi`. This outer file
has already contained a master menu with 417 entries and a list of
41 `@include` files.

---

Previous: [Sample File with `@include`](#Sample-Include-File), Up: [Include Files](#Include-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 17.3 `@verbatiminclude` file: Include a File Verbatim [¶](#g_t_0040verbatiminclude-file_003a-Include-a-File-Verbatim)

You can include the exact contents of a file in the document with the
`@verbatiminclude` command:

```
@verbatiminclude filename
```

The contents of filename is printed in a verbatim environment
(see [`@verbatim`: Literal Text](#g_t_0040verbatim)). Generally, the file is printed exactly
as it is, with all special characters and white space retained. No
indentation is added; if you want indentation, enclose the
`@verbatiminclude` within `@example`
(see [`@example`: Example Text](#g_t_0040example)).

`@value{var}` references are expanded on the
`@verbatiminclude` line. This makes it possible to
include files in other directories within a distribution,
for instance:

```
@verbatiminclude @value{top_srcdir}/NEWS
```

(You still have to get `top_srcdir` defined in the
first place.)

Other than that, the only @-commands allowed are `@@`,
`@{`, `@}` and associated @-commands such as
`@atchar{}`.

For a method on printing the file contents in a smaller font size, see
the end of the section on `@verbatim`.

---

Next: [`texi2any`: The Translator for Texinfo](#Generic-Translator-texi2any), Previous: [Include Files](#Include-Files), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 18 Formatting and Printing with TeX [¶](#Formatting-and-Printing-with-TeX)

Running the `texi2dvi` or `texi2pdf` command is the
simplest way to create printable output. These commands are installed
as part of the Texinfo package.

In more detail, typesetting and printing a Texinfo file is a multi-step
process in which you use the TeX program to create a file for printing
(called a DVI or PDF file), and then print the file. Optionally,
you may also create indices using the `texindex` command
after first running TeX; and then you must run TeX again.
`texi2dvi` takes care of all of this, running TeX and
`texindex` as needed (see [Format with `texi2dvi`](#Format-with-texi2dvi)).

When you use the shell commands, you
can either work directly in the operating system shell or work within
a shell inside GNU Emacs (or some other computing environment).
You can give formatting and printing commands from a shell within GNU
Emacs, just like any other shell command. To create a shell within
Emacs, type `M-x shell` (see [Shell](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Shell) in The GNU Emacs Manual).
If you are using GNU Emacs, you can also use commands provided by Texinfo
mode instead of shell commands. See [Formatting and Printing with Emacs](#Printing-with-Emacs).

For specifing details of the printed output such as paper size, see
[Global Document Commands](#Global-Document-Commands).

* [Use TeX](#Use-TeX)
* [Format with `texi2dvi` or `texi2pdf`](#Format-with-texi2dvi-or-texi2pdf)
* [Format with `tex`/`texindex`](#Format-with-tex_002ftexindex)
* [Print with `lpr` from Shell](#Print-with-lpr)
* [Preparing for TeX](#Preparing-for-TeX)
* [Overfull “hboxes”](#Overfull-hboxes)

---

Next: [Format with `texi2dvi` or `texi2pdf`](#Format-with-texi2dvi-or-texi2pdf), Up: [Formatting and Printing with TeX](#Hardcopy-with-TeX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 18.1 Use TeX [¶](#Use-TeX-1)

The typesetting program called TeX is used to format a Texinfo
document for printable output. TeX is a very powerful typesetting
program and, when used correctly, does an exceptionally good job. It is
not included in the Texinfo package, being a vast suite of software
in itself.

TeX is a document formatter that is used by the FSF for its
documentation. It is the easiest way to get printed output (e.g., PDF
and PostScript) for Texinfo manuals. TeX is freely redistributable,
and you can get it over the Internet or on physical media. See
<http://tug.org/texlive>.

---

Next: [Format with `tex`/`texindex`](#Format-with-tex_002ftexindex), Previous: [Use TeX](#Use-TeX), Up: [Formatting and Printing with TeX](#Hardcopy-with-TeX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 18.2 Format with `texi2dvi` or `texi2pdf` [¶](#Format-with-texi2dvi-or-texi2pdf-1)

The `texi2dvi` program takes care of all the steps for producing
a TeX DVI file from a Texinfo document. Similarly, `texi2pdf`
produces a PDF file[9](#FOOT9).

To run `texi2dvi` or `texi2pdf` on an input file
`foo.texi`, do this (where ‘`prompt$` ’ is your shell prompt):

```
prompt$ texi2dvi foo.texi
prompt$ texi2pdf foo.texi
```

As shown in this example, the file names given to `texi2dvi` and
`texi2pdf` must include any extension, such as ‘`.texi`’.

For a list of all the options, run ‘`texi2dvi --help`’. Some of the
options are discussed below.

With the `--pdf` option, `texi2dvi` produces PDF output
instead of DVI, by running `pdftex`
instead of `tex`. Alternatively, the command
`texi2pdf` is an abbreviation for running ‘`texi2dvi
--pdf`’. The command `pdftexi2dvi` is also provided as a
convenience for AUC-TeX (see [AUC-TeX](https://www.gnu.org/software/auctex/manual/auctex.html#Top)), as it
prefers to merely prepend ‘`pdf`’ to DVI producing tools to have
PDF producing tools.

With the `--dvipdf` option, `texi2dvi` produces PDF
output by running TeX and then a DVI-to-PDF program: if the
`DVIPDF` environment variable is set, that value is used, else the
first program extant among `dvipdfmx`, `dvipdfm`,
`dvipdf`, `dvi2pdf`, `dvitopdf`. This method generally
supports CJK typesetting better than `pdftex`.

With the `--ps` option, `texi2dvi` produces PostScript
instead of DVI, by running `tex` and then `dvips`
(see [Dvips](https://tug.org/texinfohtml/dvips.html#Top)). (Or the value of the `DVIPS`
environment variable, if set.)

`texi2dvi` can also be used to process LaTeX files.
Normally `texi2dvi` is able to guess the input file language
by its contents and file name extension; however, if it guesses wrong
you can explicitly specify the input language using
`--language=lang` command line option, where lang
is either ‘`latex`’ or ‘`texinfo`’.

One useful option to `texi2dvi` is ‘`--command=cmd`’.
This inserts cmd on a line by itself at the start of the file
in a temporary copy of the input file, before
running TeX. With this, you can specify different printing
formats, such as `@smallbook` (see [`@smallbook`: Printing “Small” Books](#g_t_0040smallbook)),
`@afourpaper` (see [Printing on A4 Paper](#A4-Paper)), or `@pagesizes`
(see [`@pagesizes` [width][, height]: Custom Page Sizes](#g_t_0040pagesizes)), without actually changing the document
source. (You can also do this on a site-wide basis with
`texinfo.cnf`; see [Preparing for TeX](#Preparing-for-TeX)).

The option `-E` (equivalently, `-e` and
`--expand`) does Texinfo macro expansion using
`texi2any` instead of the TeX implementation (see [Macro Details and Caveats](#Macro-Details)). Each implementation has its own limitations and
advantages.

`texi2dvi` takes the `--build=mode` option to
specify where the TeX compilation takes place, and, as a
consequence, how auxiliary files are treated. The build mode
can also be set using the environment variable
`TEXI2DVI_BUILD_MODE`. The valid values for mode are:

‘`local`’
:   Compile in the current directory, leaving all the auxiliary
    files around. This is the traditional TeX use.

‘`tidy`’
:   Compile in a local `*.t2d` directory, where the auxiliary files
    are left. Output files are copied back to the original file.

    Using the ‘`tidy`’ mode brings several advantages:

    * the current directory is not cluttered with plethora of temporary files.
    * clutter can be even further reduced using `--build-dir=dir`: all
      the `*.t2d` directories are stored there.
    * clutter can be reduced to zero using, e.g.,
      `--build-dir=/tmp/\$USER.t2d` or `--build-dir=\$HOME/.t2d`.
    * the output file is updated after every successful TeX run, for
      sake of concurrent visualization of the output. In a ‘`local`’ build
      the viewer stops during the whole TeX run.
    * if the compilation fails, the previous state of the output file
      is preserved.
    * PDF and DVI compilation are kept in separate
      subdirectories
      preventing any possibility of auxiliary file incompatibility.

    On the other hand, because ‘`tidy`’ compilation takes place in another
    directory, occasionally TeX won’t be able to find some files (e.g., when
    using `\graphicspath`): in that case, use `-I` to specify the
    additional directories to consider.

‘`clean`’
:   Same as ‘`tidy`’, but remove the auxiliary directory afterwards.
    Every compilation therefore requires the full cycle.

`texi2dvi` will use `etex` if
it is available, because it runs faster in some cases, and
provides additional tracing information when debugging
`texinfo.tex`. Nevertheless, this extended version of TeX is
not required, and the DVI output is identical.

`texi2dvi` attempts to detect auxiliary files output by TeX,
either by using the `-recorder` option, or by scanning for
‘`\openout`’ in the log file that a run of TeX produces. You may
control how `texi2dvi` does this with the `TEXI2DVI_USE_RECORDER`
environment variable. Valid values are:

‘`yes`’
:   use the `-recorder` option, no checks.

‘`no`’
:   scan for ‘`\openout`’ in the log file, no checks.

‘`yesmaybe`’
:   check whether `-recorder` option is supported, and if yes
    use it, otherwise check for tracing ‘`\openout`’ in the log file is
    supported, and if yes use it, else it is an error.

‘`nomaybe`’
:   same as ‘`yesmaybe`’, except that the ‘`\openout`’ trace in log
    file is checked first.

The default is ‘`nomaybe`’. This environment variable is provided
for troubleshooting purposes, and may change or disappear in the future.

---

Next: [Print with `lpr` from Shell](#Print-with-lpr), Previous: [Format with `texi2dvi` or `texi2pdf`](#Format-with-texi2dvi-or-texi2pdf), Up: [Formatting and Printing with TeX](#Hardcopy-with-TeX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 18.3 Format with `tex`/`texindex` [¶](#Format-with-tex_002ftexindex-1)

You can do the basic formatting of a Texinfo file with the shell
command `tex` followed by the name of the Texinfo file. For
example:

```
tex foo.texi
```

TeX will produce a *DVI file* as well as several auxiliary
files containing information for indices, cross-references, etc. The
DVI file (for *DeVice Independent* file) can be printed on
virtually any device, perhaps after a further conversion (see the
previous section).

The `tex` formatting command itself does not sort the indices; it
writes an output file of unsorted index data. To generate a printed
index after running the `tex` command, you first need a sorted
index to work from. The `texindex` command sorts indices.
(`texi2dvi`, described in the previous section, runs
`tex` and `texindex` as necessary.)

`tex` outputs unsorted index files under names following a
standard convention: the name of your main input file with any
‘`.texi`’ or similar extension replaced by the two letter index
name. For example, the raw index output files for the input file
`foo.texi` would be, by default, `foo.cp`, `foo.vr`,
`foo.fn`, `foo.tp`, `foo.pg` and `foo.ky`. Those
are exactly the arguments to give to `texindex`.

Instead of specifying all the unsorted index file names explicitly,
it’s typical to use ‘`??`’ as shell wildcards and give the command
in this form:

```
texindex foo.??
```

This command will run `texindex` on all the unsorted index files,
including any two letter indices that you have defined yourself using
`@defindex` or `@defcodeindex`. You can safely run
‘`texindex foo.??`’ even if there are files with two letter
extensions that are not index files, such as ‘`foo.el`’. The
`texindex` command reports but otherwise ignores such files.

For each file specified, `texindex` generates a sorted index file
whose name is made by appending ‘`s`’ to the input file name; for
example, `foo.cps` is made from `foo.cp`. The
`@printindex` command looks for a file with that name
(see [Printing Indices and Menus](#Printing-Indices-_0026-Menus)). TeX does not read the raw
index output file, and `texindex` does not alter it.

After you have sorted the indices, you need to rerun `tex` on the
Texinfo file. This regenerates the output file, this time with
up-to-date index entries.

Finally, you may need to run `tex` one more time, to get the page
numbers in the cross-references correct.

To summarize, this is a five-step process. (Alternatively, it’s a
one-step process: run `texi2dvi`; see the previous section.)

1. Run `tex` on your Texinfo file. This generates a DVI file (with
   undefined cross-references and no indices), and the raw index files
   (with two letter extensions).
2. Run `texindex` on the raw index files. This creates the
   corresponding sorted index files (with three letter extensions).
3. Run `tex` again on your Texinfo file. This regenerates the DVI
   file, this time with indices and defined cross-references, but with
   page numbers for the cross-references from the previous run, generally
   incorrect.
4. Sort the indices again, with `texindex`.
5. Run `tex` one last time. This time the correct page numbers are
   written for the cross-references.

To generate PDF, you can run the `pdftex`
program instead of plain `tex`. That is, run `pdftex
foo.texi` instead of ‘`tex foo.texi`’ in the examples above.

* [Formatting Partial Documents](#Formatting-Partial-Documents)
* [Details of `texindex`](#Details-of-texindex)

---

Next: [Details of `texindex`](#Details-of-texindex), Up: [Format with `tex`/`texindex`](#Format-with-tex_002ftexindex)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 18.3.1 Formatting Partial Documents [¶](#Formatting-Partial-Documents-1)

Sometimes you may wish to print a document while you know it is
incomplete, or to print just one chapter of a document. In such a
case, the usual auxiliary files that TeX creates and warnings
TeX gives about undefined cross-references are just nuisances. You
can avoid them with the `@novalidate` command, which you must
give *before* any sectioning or cross-reference commands.

Thus, the beginning of your file would look approximately like this:

```
\input texinfo
@novalidate
...
```

`@novalidate` also turns off validation in
`texi2any`, just like its `--no-validate` option
(see [Invoking `texi2any` from a Shell](#Invoking-texi2any)).

Furthermore, you need not run `texindex` each time after you run
`tex`. The `tex` formatting command simply uses whatever
sorted index files happen to exist from a previous use of
`texindex`. If those are out of date, that is usually ok while
you are creating or debugging a document.

---

Previous: [Formatting Partial Documents](#Formatting-Partial-Documents), Up: [Format with `tex`/`texindex`](#Format-with-tex_002ftexindex)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 18.3.2 Details of `texindex` [¶](#Details-of-texindex-1)

In Texinfo version 6, released in 2015, the `texindex` program
was completely reimplemented. The principal functional difference is
that index entries beginning with a left brace or right brace
(‘`{`’ resp. ‘`}`’) can work properly. For example, these
simple index entries are processed correctly, including the “index
initial” shown in the index:

```
@cindex @{
@cindex @}
...
@printindex cp
```

Although not a matter of functionality, readers may be interested to
know that the new `texindex` is a literate program
(<https://en.wikipedia.org/wiki/Literate_programming>) using
Texinfo for documentation and (portable) `awk` for code. A
single source file, `texindex/ti.twjr` in this case, produces the
runnable program, a printable document, and an online document.

The system is called TexiWeb Jr. and was created by Arnold
Robbins, who also wrote the new `texindex`. Not
coincidentally, he is also the long-time maintainer of `gawk`
(GNU Awk, see [The GNU Awk User’s Guide](https://www.gnu.org/software/gawk/manual/gawk.html#Top)). The file
`texindex/Makefile.am` shows example usage of the system.

---

Next: [Preparing for TeX](#Preparing-for-TeX), Previous: [Format with `tex`/`texindex`](#Format-with-tex_002ftexindex), Up: [Formatting and Printing with TeX](#Hardcopy-with-TeX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 18.4 Print with `lpr` from Shell [¶](#Print-with-lpr-from-Shell)

The way to print a DVI file depends on your system installation. Two
common ones are ‘`dvips foo.dvi -o`’ to make a PostScript file
first and then print that, and ‘`lpr -d foo.dvi`’ to print a DVI
file directly.

For example, the following commands will (probably) suffice to sort
the indices, format, and print this manual using the `texi2dvi`
shell script (see [Format with `texi2dvi`](#Format-with-texi2dvi)).

```
texi2dvi texinfo.texi
dvips texinfo.dvi -o
lpr texinfo.ps
```

Depending on the `lpr` setup on your machine, you might be able to
combine the last two steps into `lpr -d texinfo.dvi`.

You can also generate a PDF file by running `texi2dvi` instead of
`texi2dvi`; a PDF is often directly printable. Or you can
generate a PCL file by using `dvilj` instead of `dvips`, if
you have a printer that prefers that format.

`lpr` is a standard program on Unix systems, but it is usually
absent on MS-DOS/MS-Windows. If so, just create a PostScript or PDF
or PCL file, whatever is most convenient, and print that in the usual
way for your machine (e.g., by sending to the appropriate port,
usually ‘`PRN`’).

---

Next: [Overfull “hboxes”](#Overfull-hboxes), Previous: [Print with `lpr` from Shell](#Print-with-lpr), Up: [Formatting and Printing with TeX](#Hardcopy-with-TeX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 18.5 Preparing for TeX [¶](#Preparing-for-TeX-1)

TeX needs to find the `texinfo.tex` file that the
‘`\input texinfo`’ command on the first line reads. The
`texinfo.tex` file tells TeX how to handle @-commands; it is
included in all standard GNU distributions. The latest version
released for general use is available
from the usual GNU servers and mirrors:

```
http://ftp.gnu.org/gnu/texinfo/texinfo.tex
http://ftpmirror.gnu.org/texinfo/texinfo.tex
```

The latest development version is available from the Texinfo source
repository:

```
http://git.savannah.gnu.org/cgit/texinfo.git/plain/doc/texinfo.tex
```

`texinfo.tex` is essentially a standalone file,
so, if you need or want to try a newer version
than came with your system, it nearly always suffices to download it
and put it anywhere that TeX will find it. You can replace
any existing `texinfo.tex` with a newer version (of course saving
the original in case of disaster).

Also, you should install `epsf.tex`, if it is not already installed
from another distribution. More details are at the end of the description
of the `@image` command (see [Inserting Images](#Images)).

To use quotation marks other than those used in English, you’ll need
to have the European Computer Modern fonts (e.g., `ecrm1000`) and
(for PDF output) CM-Super fonts (see [Inserting Quotation Marks](#Inserting-Quotation-Marks)).

To use the `@euro` command, you’ll need the ‘`feym*`’ fonts
(e.g., `feymr10`). See [`@euro` (€): Euro Currency Symbol](#g_t_0040euro).

All of the above files should be installed by default in a reasonable
TeX installation.

Optionally, you may create a file `texinfo.cnf` for site configuration.
When processing a Texinfo file, TeX looks for this file
in its search path, which includes the current directory and standard
installation directories.
You can use this file for local conventions. For example, if
`texinfo.cnf` contains the line
‘`@afourpaper`’ (see [Printing on A4 Paper](#A4-Paper)), then all Texinfo documents
will be processed with that page size in effect. If you have nothing
to put in `texinfo.cnf`, you do not need to create it.

You can set the `TEXINPUTS` environment variable
to allow TeX to find `texinfo.cnf`.
(This also works for `texinfo.tex` and any other file TeX
might read). For example, if you are using a Bourne shell-compatible shell
(`sh`, `bash`, `ksh`, …), your `.profile` file
could contain the lines:

```
TEXINPUTS=.:/home/me/mylib:
export TEXINPUTS
```

These settings would cause TeX first to look for an `\input` file
in the current directory, indicated by the ‘`.`’, then in a
hypothetical user ‘`me`’’s `mylib` directory, and finally in
the system directories. (A leading, trailing, or doubled ‘`:`’
indicates searching the system directories at that point.)

---

Previous: [Preparing for TeX](#Preparing-for-TeX), Up: [Formatting and Printing with TeX](#Hardcopy-with-TeX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 18.6 Overfull “hboxes” [¶](#Overfull-_0060_0060hboxes_0027_0027)

TeX is sometimes unable to typeset a line within the normal
margins. This most often occurs when TeX comes upon what it
interprets as a long word that it cannot hyphenate, such as an
electronic mail network address or a very long identifier. When this
happens, TeX prints an error message like this:

```
Overfull @hbox (20.76302pt too wide)
```

(In TeX, lines are in “horizontal boxes”, hence the term, “hbox”.
‘`@hbox`’ is a TeX primitive not used in the Texinfo language.)

TeX also provides the line number in the Texinfo source file and
the text of the offending line, which is marked at all the places that
TeX considered hyphenation. See [Debugging with TeX](#Debugging-with-TeX), for more
information about typesetting errors.

If the Texinfo file has an overfull hbox, you can rewrite the sentence
so the overfull hbox does not occur, or you can decide to leave it. A
small excursion into the right margin often does not matter and may not
even be noticeable.

If you have many overfull boxes and/or an antipathy to rewriting, you
can coerce TeX into greatly increasing the allowable interword
spacing, thus (if you’re lucky) avoiding many of the bad line breaks,
like this:

```
@tex
\global\emergencystretch = .9\hsize
@end tex
```

(You should adjust the fraction as needed.) This huge value for
`\emergencystretch` cannot be the default, since then the typeset
output would generally be of noticeably lower quality; its default
value is ‘`.15\hsize`’. `\hsize` is the TeX dimension
containing the current line width.

For any overfull boxes you do have, TeX will print a large, ugly,
black rectangle beside the line that contains the overfull hbox unless
told otherwise. This is so you will notice the location of the
problem if you are correcting a draft.

To prevent such a monstrosity from marring your final printout, write
the following in the beginning of the Texinfo file on a line of its own,
before the `@titlepage` command:

```
@finalout
```

---

Next: [Creating and Installing Info Files](#Creating-and-Installing-Info-Files), Previous: [Formatting and Printing with TeX](#Hardcopy-with-TeX), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 19 `texi2any`: The Translator for Texinfo [¶](#texi2any_003a-The-Translator-for-Texinfo)

`texi2any` is the generic translator for Texinfo that can
produce different output formats and is highly customizable. It
supports these formats:

```
Info (by default, or with --info),
HTML (with --html),
EPUB 3 (with --epub3),
plain text (with --plaintext),
LaTeX (with --latex),
DocBook (with --docbook),
Texinfo XML (with --xml).
```

`makeinfo` is an alias for `texi2any`. By default,
both `texi2any` and `makeinfo` generate Info output;
indeed, there are no differences in behavior based on the name.

Beside these default formats, command line options to
`texi2any` can change many aspects of the output. Beyond
that, initialization files provide even more control over the final
output—nearly anything not specified in the Texinfo input file.
Initialization files are written in Perl, like the main program, and
anything which can be specified on the command line can also be
specified within a initialization file.

* [Invoking `texi2any` from a Shell](#Invoking-texi2any)
* [Environment Variables Recognized by `texi2any`](#texi2any-Environment-Variables)
* [`texi2any` Printed Output](#texi2any-Printed-Output)
* [Customization Variables](#Customization-Variables)
* [Internationalization of Document Strings](#Internationalization-of-Document-Strings)
* [Invoking `pod2texi`: Convert Pod to Texinfo](#Invoking-pod2texi)
* [`texi2html`: Ancestor of `texi2any`](#texi2html)

---

Next: [Environment Variables Recognized by `texi2any`](#texi2any-Environment-Variables), Up: [`texi2any`: The Translator for Texinfo](#Generic-Translator-texi2any)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 19.1 Invoking `texi2any` from a Shell [¶](#Invoking-texi2any-from-a-Shell)

To process a Texinfo file, invoke `texi2any` followed by the name
of the Texinfo file. Also select the format you want to output with the
appropriate command line option (default is Info). Thus, to create
the Info file for Bison, type the following to the shell:

```
texi2any --info bison.texi
```

You can specify more than one input file name; each is processed in
turn. If an input file name is ‘`-`’, standard input is read.

The `texi2any` program accepts many options. Perhaps the
most basic are those that change the output format. By default,
`texi2any` outputs Info.

Each command line option is either a long name preceded by ‘`--`’
or a single letter preceded by ‘`-`’. You can use abbreviations
for the long option names as long as they are unique.

For example, you could use the following shell command to create an
Info file for `bison.texi` in which lines are filled to only
68 columns:

```
texi2any --fill-column=68 bison.texi
```

You can write two or more options in sequence, like this:

```
texi2any --no-split --fill-column=70 ...
```

(This would keep the Info file together as one possibly very long
file and would also set the fill column to 70.)

The options are (approximately in alphabetical order):

`--commands-in-node-names` [¶](#index-_002d_002dcommands_002din_002dnode_002dnames)
:   This option now does nothing, but remains for compatibility. (It used
    to ensure that @-commands in node names were expanded throughout the
    document, especially `@value`. This is now done by default.)

`--conf-dir=dir` [¶](#index-_002d_002dconf_002ddir_003ddir)
:   Prepend dir to the directory search list for finding
    customization files that may be loaded with `--init-file` (see
    below). The dir value can be a single directory, or a list of
    several directories separated by the usual path separator character
    (‘`:`’ on Unix-like systems, ‘`;`’ on Windows).

`--css-include=file` [¶](#index-_002d_002dcss_002dinclude)
:   When producing HTML, literally include the contents of file,
    which should contain Cascading Style Sheets specifications, in the
    ‘`<style>`’ block of the HTML output. If file is ‘`-`’,
    read standard input. See [HTML CSS](#HTML-CSS).

`--css-ref=url` [¶](#index-_002d_002dcss_002dref)
:   When producing HTML, add a ‘`<link>`’ tag to the output which
    references a cascading style sheet at url. This allows using
    standalone style sheets.

`-D var` [¶](#index-_002dD-var)

`-D 'var value'`
:   Cause the Texinfo variable var to be defined. This is
    equivalent to `@set var` in the Texinfo file
    (see [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value)).

    The argument to the option is always one word to the shell; if it
    contains internal whitespace, the first word is taken as the variable
    name and the remainder as the value. For example, `-D 'myvar
    someval'` is equivalent to `@set myvar someval`.

`--docbook` [¶](#index-_002d_002ddocbook)
:   Generate DocBook output (rather than Info).

`--document-language=lang` [¶](#index-_002d_002ddocument_002dlanguage)
:   Use lang to translate Texinfo keywords which end up in the
    output document. The default is the locale specified by the
    `@documentlanguage` command if there is one, otherwise English
    (see [`@documentlanguage ll[_cc]`: Set the Document Language](#g_t_0040documentlanguage)).

`--dvi` [¶](#index-_002d_002ddvi)
:   Generate a TeX DVI file using `texi2dvi`, rather than Info
    (see [`texi2any` Printed Output](#texi2any-Printed-Output)).

`--dvipdf` [¶](#index-_002d_002ddvipdf)
:   Generate a PDF file using `texi2dvi --dvipdf`, rather than
    Info (see [`texi2any` Printed Output](#texi2any-Printed-Output)).

`--enable-encoding` [¶](#index-_002d_002denable_002dencoding)

`--disable-encoding`
:   By default, or with `--enable-encoding`, output accented and
    special characters in Info and plain text output based on
    the document encoding. With `--disable-encoding`, 7-bit
    ASCII transliterations are output. Also obeyed in other output
    formats for index keys sorting and for some plain text output.

`--epub3` [¶](#index-_002d_002depub3)
:   Generate EPUB 3 output.

`--error-limit=limit` [¶](#index-_002d_002derror_002dlimit_003dlimit)

`-e limit`
:   Report limit errors before aborting (on the assumption that
    continuing would be useless); default 100.

`--fill-column=width` [¶](#index-_002d_002dfill_002dcolumn_003dwidth)

`-f width`
:   Specify the maximum number of columns in a line; this is the
    right-hand edge of a line. Paragraphs that are filled will be filled
    to this width. (Filling is the process of breaking up and connecting
    lines so that lines are the same length as or shorter than the number
    specified as the fill column. Lines are broken between words.) The
    default value is 72.

`--footnote-style=style` [¶](#index-_002d_002dfootnote_002dstyle_003dstyle)

`-s style`
:   Set the footnote style to style: either ‘`end`’ for the end
    node style (the default) or ‘`separate`’ for the separate node
    style. The value set by this option overrides the value set in a
    Texinfo file by a `@footnotestyle` command (see [Footnote Styles](#Footnote-Styles)).

    In Info, when the footnote style is ‘`separate`’, `texi2any` makes a
    new node containing the footnotes found in the current node. When the
    footnote style is ‘`end`’, `texi2any` places the footnote
    references at the end of the current node.

    In HTML, when the footnote style is ‘`end`’, or if the output is
    not split, footnotes are put at the end of the output. If set to
    ‘`separate`’, and the output is split, they are placed in a
    separate file.

`--force` [¶](#index-_002d_002dforce)

`-F`
:   Ordinarily, if the input file has errors, the output files are not
    created. With this option, they are preserved.

`--help` [¶](#index-_002d_002dhelp_002c-for-texi2any)

`-h`
:   Print a message with available options and basic usage, then exit
    successfully.

`--html` [¶](#index-_002d_002dhtml)
:   Generate HTML output (rather than Info). By default, the HTML output
    is split into one output file per node, and the split output is written
    into a subdirectory based on the name of the input file.
    See [Generating HTML](#Generating-HTML).

`-I dir` [¶](#index-_002dI-dir)
:   Append dir to the directory search list for finding files that
    are included using the `@include` command. By default,
    `texi2any` searches only the current directory. If dir is
    not given, the current directory is appended. The dir value
    can be a single directory or a list of several directories separated
    by the usual path separator character (‘`:`’ on Unix-like systems,
    ‘`;`’ on Windows).

`--ifdocbook` [¶](#index-_002d_002difxml)

`--ifhtml`

`--ifinfo`

`--iflatex`

`--ifplaintext`

`--iftex`

`--ifxml`
:   For the given format, process ‘`@ifformat`’ and
    ‘`@format`’ commands, and do not process
    ‘`@ifnotformat`’, regardless of the format being output.
    For instance, if `--iftex` is given, then ‘`@iftex`’ and
    ‘`@tex`’ blocks will be read, and ‘`@ifnottex`’ blocks will be
    ignored.

`--no-ifdocbook` [¶](#index-_002d_002dno_002difxml)

`--no-ifhtml`

`--no-ifinfo`

`--no-iflatex`

`--no-ifplaintext`

`--no-iftex`

`--no-ifxml`
:   For the given format, do not process ‘`@ifformat`’ and
    ‘`@format`’ commands, and do process
    ‘`@ifnotformat`’, regardless of the format being output.
    For instance, if `--no-ifhtml` is given, then ‘`@ifhtml`’
    and ‘`@html`’ blocks will not be read, and ‘`@ifnothtml`’
    blocks will be.

`--info` [¶](#index-_002d_002dinfo)
:   Generate Info output. By default, if the output file contains more
    than about 300,000 bytes, it is split into shorter subfiles of about
    that size. The name of the output file and any subfiles is determined
    by the input file name, or by `@setfilename`, if present
    (see [Setting the Output File Name](#Setting-the-Output-File-Name)). See [Tag Files and Split Files](#Tag-and-Split-Files).

`--init-file=file` [¶](#index-_002d_002dinit_002dfile_003dfile)
:   Load file as code to modify the behavior and output of the
    generated manual. It is customary to use the `.pm` or the
    `.init` extensions for these customization files, but that is not
    enforced; the file name can be anything. The
    `--conf-dir` option (see above) can be used to add to the list
    of directories in which these customization files are searched for.

`--internal-links=file` [¶](#index-_002d_002dinternal_002dlinks_003dfile)
:   In HTML mode, output a tab-separated file containing three columns:
    the internal link to an indexed item or item in the table of contents,
    the name of the index (or table of contents) in which it occurs, and
    the term which was indexed or entered. The items are in the natural
    sorting order for the given element. This dump can be useful for
    post-processors.

`--latex` [¶](#index-_002d_002dlatex)
:   Generate LaTeX output.

`--macro-expand=file` [¶](#index-_002d_002dmacro_002dexpand_003dfile)

`-E file`
:   Output the Texinfo source, with all Texinfo macros expanded, to
    file. Normally, the result of macro expansion is used
    internally by `texi2any` and then discarded.

`--no-headers` [¶](#index-_002d_002dno_002dheaders)
:   Do not include menus or node separator lines in the output.

    When generating Info, this is the same as using `--plaintext`,
    resulting in a simple plain text file. Furthermore, output is
    to standard output unless overridden with `-o`. (This
    behavior is for backward compatibility.)

    When generating HTML, and output is split, also output navigation
    links only at the beginning of each file. If output is not split, do
    not include navigation links at the top of each node at all.
    See [Generating HTML](#Generating-HTML).

`--node-files` [¶](#index-_002d_002dnode_002dfiles)

`--no-node-files`
:   With `--node-files`, when generating HTML, create redirection
    files for anchors and any nodes not already output with the file name
    corresponding to the node name (see [HTML Cross-reference Node Name Expansion](#HTML-Xref-Node-Name-Expansion)).
    This makes it possible for section- and chapter-level cross-manual
    references to succeed (see [HTML Cross-reference Configuration: `htmlxref.cnf`](#HTML-Xref-Configuration)).

    If the output is split, this is enabled by default. If the output is
    not split, `--node-files` enables the creation of the
    redirection files, in addition to the monolithic main output file.
    `--no-node-files` suppresses the creation of redirection files
    in any case. This option has no effect with any output format other
    than HTML. See [Generating HTML](#Generating-HTML).

`--no-validate` [¶](#index-_002d_002dno_002dvalidate)

`--no-pointer-validate`
:   Suppress the pointer-validation phase of `texi2any`—a dangerous
    thing to do. This can also be done with the `@novalidate`
    command (see [Formatting Partial Documents](#Formatting-Partial-Documents)).

    If you do not suppress pointer validation, `texi2any` will check
    the validity of cross-references and menu entries in the Texinfo file, as
    well as node pointers if they are given explicitly.

`--no-warn` [¶](#index-_002d_002dno_002dwarn)
:   Suppress warning messages (but not error messages).

`--number-footnotes` [¶](#index-_002d_002dno_002dnumber_002dfootnotes)

`--no-number-footnotes`
:   With `--no-number-footnotes`, suppress automatic footnote
    numbering. By default, footnotes are numbered sequentially within a node,
    i.e., the current footnote number is reset to 1 at the start of each node.

`--number-sections` [¶](#index-_002d_002dnumber_002dsections)

`--no-number-sections`
:   With `--number-sections` (the default), output chapter,
    section, and appendix numbers as in printed manuals. This works only
    with hierarchically structured manuals. You should specify
    `--no-number-sections` if your manual is not normally structured.

`--output=file` [¶](#index-_002d_002doutput_003dfile)

`-o file`
:   Specify that the output should be directed to file. This
    overrides any file name specified in a `@setfilename` command
    found in the Texinfo source. If neither `@setfilename` nor this
    option are specified, the input file name is used to determine the
    output name. See [Setting the Output File Name](#Setting-the-Output-File-Name).

    If file is ‘`-`’, output goes to standard output and
    ‘`--no-split`’ is implied.

    If file is a directory or ends with a ‘`/`’ the usual rules
    are used to determine the output file name (namely, use
    `@setfilename` or the input file name) but the files are written
    to the file directory. For example, ‘`texi2any -o bar/
    foo.texi`’, with or without `--no-split`, will write
    `bar/foo.info`, and possibly other files, under `bar/`.

    When generating HTML and output is split, file is used as the
    name for the directory into which all files are written. For example,
    ‘`texi2any -o bar --html foo.texi`’ will write
    `bar/index.html`, among other files.

    When generating EPUB a container directory for the files and directories needed
    for the EPUB format is created, as well as the EPUB output file. If file
    corresponds to a directory, the container directory is placed within file.
    The EPUB output file is never put in this directory. If file corresponds
    to a file, it is used for the EPUB output file name.

`--output-indent=val` [¶](#index-_002d_002doutputindent)
:   This option now does nothing, but remains for compatibility. (It used
    to alter indentation in XML/DocBook output.)

`-P path` [¶](#index-_002dP-path)
:   Prepend path to the directory search list for `@include`.
    If path is not given, the current directory is prepended. See
    ‘`-I`’ above.

`--paragraph-indent=indent` [¶](#index-_002d_002dparagraph_002dindent_003dindent)

`-p indent`
:   Set the paragraph indentation style to indent. The value set by
    this option overrides the value set in a Texinfo file by an
    `@paragraphindent` command (see [`@paragraphindent`: Controlling Paragraph Indentation](#g_t_0040paragraphindent)).
    The value of indent is interpreted as follows:

    ‘`asis`’
    :   Preserve any existing indentation (or lack thereof) at the beginnings
        of paragraphs.

    ‘`0`’ or ‘`none`’
    :   Delete any existing indentation.

    num
    :   Indent each paragraph by num spaces.

    The default is to indent by two spaces, except for paragraphs
    following a section heading, which are not indented.

`--pdf` [¶](#index-_002d_002dpdf)
:   Generate a PDF file using `texi2dvi --pdf`, rather than Info
    (see [`texi2any` Printed Output](#texi2any-Printed-Output)).

`--plaintext` [¶](#index-_002d_002dplaintext)
:   Output a plain text file (rather than Info): do not include menus or
    node separator lines in the output. This results in a straightforward
    plain text file that you can (for example) send in email without
    complications, or include in a distribution (for example, an
    `INSTALL` file).

    With this option the output goes to standard output by default, instead
    of to a file with a name based on the input file name or `@setfilename`;
    this can be overridden with `-o`.

`--ps` [¶](#index-_002d_002dps)
:   Generate a PostScript file using `texi2dvi --ps`, rather than
    Info (see [`texi2any` Printed Output](#texi2any-Printed-Output)).

`--set-customization-variable var=value` [¶](#index-_002d_002dset_002dcustomization_002dvariable-var_003dvalue)

`-c var=value`
:   Set the customization variable var to value. The `=`
    is optional, but both var and value must be quoted to the
    shell as necessary so the result is a single word. Many aspects of
    `texi2any` behavior and output may be controlled by
    customization variables, beyond what can be set in the document by
    @-commands and with other command line switches. See [Customization Variables](#Customization-Variables).

`--split=how` [¶](#index-_002d_002dsplit_003dhow)

`--no-split`
:   When generating Info, by default large output files are split into
    smaller subfiles, of approximately 300k bytes. When generating HTML,
    by default each output file contains one node (see [Generating HTML](#Generating-HTML)). `--no-split` suppresses this splitting of the output.

    Alternatively, `--split=how` may be used to specify at
    which level the HTML output should be split. The possible values for
    how are:

    ‘`chapter`’
    :   The output is split at `@chapter` and other sectioning
        @-commands at this level (`@appendix`, etc.).

    ‘`section`’
    :   The output is split at `@section` and similar.

    ‘`node`’
    :   The output is split at every node. This is the default.

    Plain text output can be split similarly to HTML. This may be useful
    for extracting sections from a Texinfo document and making them
    available as separate files.

`--split-size=num` [¶](#index-_002d_002dsplit_002dsize_003dnum)
:   Keep Info files to at most num characters if possible; default
    is 300,000. (However, a single node will never be split across Info
    files.)

`--transliterate-file-names` [¶](#index-_002d_002dtransliterate_002dfile_002dnames)
:   Enable transliteration of non-ASCII characters in node names for the
    purpose of file name creation. See [HTML Cross-reference 8-bit Character Expansion](#HTML-Xref-8_002dbit-Character-Expansion).

`-U var`
:   Cause var to be undefined. This is equivalent to `@clear
    var` in the Texinfo file (see [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value)).

`--verbose` [¶](#index-_002d_002dverbose)
:   Cause `texi2any` to display messages saying what it is doing.
    Normally, `texi2any` only outputs messages if there are errors or
    warnings.

`--version` [¶](#index-_002d_002dversion_002c-for-texi2any)

`-V`
:   Print the version number, then exit successfully.

`--Xopt str` [¶](#index-_002d_002dXopt-str)
:   Pass str (a single shell word) to `texi2dvi`; may be
    repeated (see [`texi2any` Printed Output](#texi2any-Printed-Output)).

`--xml` [¶](#index-_002d_002dxml)
:   Generate Texinfo XML output (rather than Info).

---

Next: [`texi2any` Printed Output](#texi2any-Printed-Output), Previous: [Invoking `texi2any` from a Shell](#Invoking-texi2any), Up: [`texi2any`: The Translator for Texinfo](#Generic-Translator-texi2any)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 19.2 Environment Variables Recognized by `texi2any` [¶](#Environment-Variables-Recognized-by-texi2any)

`texi2any` also reads the environment variable
`TEXINFO_OUTPUT_FORMAT` to determine the output format, if not
overridden by a command line option. The value should be one of:

```
docbook  dvi  dvipdf  epub3  html  info  latex  pdf  plaintext  ps  xml
```

If not set or otherwise specified, Info output is the default.

`TEXINFO_OUTPUT_FORMAT` may take some other special values, which do
not correspond to any command-line settable output format
(see [Customization Variables and Options](#Customization-Variables-and-Options)).

The customization variable of the same name is also read; if set, that
overrides an environment variable setting, but not a command-line
option. See [Customization Variables and Options](#Customization-Variables-and-Options).

You can control `texi2any`’s use of Perl extension modules
by setting the `TEXINFO_XS` environment variable. These modules
are compiled native code that the interpreted Perl code can use.
Ideally, these extension modules should just work, and the only noticeable
difference they should make is that `texi2any` finishes running
sooner. However, you can use this environment variable for the purposes
of troubleshooting: for example, if you have problems with the output of
`texi2any` varying depending on whether the extension modules are
in use.

The following values of `TEXINFO_XS` are recognized by
`texi2any`:

‘`default`’
:   The default behavior. Try to load extension modules, and silently fall
    back to the interpreted Perl implementations if this fails.

‘`warn`’
:   Try to load extension modules, and if this fails, give a warning message
    before falling back to the interpreted Perl implementations.

‘`debug`’
:   Try to load extension modules, printing many messages while doing so.

‘`omit`’
:   Do not use extension modules.

Set `TEXINFO_XS_PARSER` to ‘`0`’ to disable the use of the
native code implementation of the parser module. This is the part of
`texi2any` that converts Texinfo input into an internal tree
format used for further processing into output formats. This may
be useful for working around bugs or incompatibilities between the
native code implementation and the implementation in pure Perl code.

---

Next: [Customization Variables](#Customization-Variables), Previous: [Environment Variables Recognized by `texi2any`](#texi2any-Environment-Variables), Up: [`texi2any`: The Translator for Texinfo](#Generic-Translator-texi2any)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 19.3 `texi2any` Printed Output [¶](#texi2any-Printed-Output-1)

To justify the name Texinfo-to-*any*, `texi2any` has
basic support for creating printed output in the various formats:
TeX DVI, PDF, and PostScript. This is done via the simple method
of executing the `texi2dvi` program when those output formats
are requested, after checking the validity of the input to give users
the benefit of `texi2any`’s error checking. If you don’t want
such error checking, perhaps because your manual plays advanced TeX
tricks together with `texinfo.tex`, just invoke
`texi2dvi` directly.

The output format options for this are `--dvi`,
`--dvipdf`, `--pdf`, and `--ps`. See [Format with `texi2dvi`](#Format-with-texi2dvi), for more details on these options and general
`texi2dvi` operation. In addition, the `--verbose`,
`--silent`, and `--quiet` options are passed on if
specified; the `-I` and `-o` options are likewise passed
on with their arguments, and `--debug` without its argument.

The only option remaining that is related to the `texi2dvi`
invocation is `--Xopt`. Here, just the argument is passed on
and multiple `--Xopt` options accumulate. This provides a way
to construct an arbitrary command line for `texi2dvi`. For
example, running

```
texi2any --Xopt -t --Xopt @a4paper --pdf foo.texi
```

is equivalent to running

```
texi2dvi -t @a4paper --pdf foo.texi
```

except for the validity check.

Although one might wish that other options to `texi2any` would
take effect, they don’t. For example, running ‘`texi2any
--no-number-sections --dvi foo.texi`’ still results in a DVI file with
numbered sections. (Perhaps this could be improved in the future, if
requests are received.)

The actual name of the command that is invoked is specified by the
`TEXI2DVI` customization variable (see [Other Customization Variables](#Other-Customization-Variables)). As you might guess, the default is ‘`texi2dvi`’.

`texi2any` itself does not generate any normal output when it
invokes `texi2dvi`, only diagnostic messages.

---

Next: [Internationalization of Document Strings](#Internationalization-of-Document-Strings), Previous: [`texi2any` Printed Output](#texi2any-Printed-Output), Up: [`texi2any`: The Translator for Texinfo](#Generic-Translator-texi2any)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]
