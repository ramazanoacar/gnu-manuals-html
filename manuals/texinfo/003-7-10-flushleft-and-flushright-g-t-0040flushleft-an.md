### 7.10 `@flushleft` and `@flushright` [¶](#g_t_0040flushleft-and-_0040flushright)

The `@flushleft` and `@flushright` commands line up the
ends of lines on the left and right margins of a page,
but do not fill the text. The commands are written on lines of their
own, without braces. The `@flushleft` and `@flushright`
commands are ended by `@end flushleft` and `@end
flushright` commands on lines of their own.

For example,

```
@flushleft
This text is
written flushleft.
@end flushleft
```

produces

> This text is
> written flushleft.

`@flushright` produces the type of indentation often used in the
return address of letters. For example,

```
@flushright
Here is an example of text written
flushright.  The @code{@flushright} command
right justifies every line but leaves the
left end ragged.
@end flushright
```

produces

Here is an example of text written
flushright. The `@flushright` command
right justifies every line but leaves the
left end ragged.

---

Next: [`@noindent`: Omitting Indentation](#g_t_0040noindent), Previous: [`@flushleft` and `@flushright`](#g_t_0040flushleft-_0040flushright), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 7.11 `@raggedright`: Ragged Right Text [¶](#g_t_0040raggedright_003a-Ragged-Right-Text)

The `@raggedright` fills text as usual, but the text is only
justified on the left; the right margin is ragged. The command is
written on a line of its own, without braces. The
`@raggedright` command is ended by `@end raggedright` on a
line of its own. This command only has an effect in output
formats where text is justified on the left, but
not in output formats where text is always set ragged right, such as
Info or HTML.

The `@raggedright` command can be useful with paragraphs
containing lists of commands with long names, when it is known in
advance that justifying the text on both margins will make the
paragraph look bad.

An example (from elsewhere in this manual):

```
@raggedright
Commands for double and single angle quotation marks:
@code{@@guillemetleft@{@}}, @code{@@guillemetright@{@}},
@code{@@guillemotleft@{@}}, @code{@@guillemotright@{@}},
@code{@@guilsinglleft@{@}}, @code{@@guilsinglright@{@}}.
@end raggedright
```

produces

Commands for double and single angle quotation marks:
`@guillemetleft{}`, `@guillemetright{}`,
`@guillemotleft{}`, `@guillemotright{}`,
`@guilsinglleft{}`, `@guilsinglright{}`.

---

Next: [`@indent`: Forcing Indentation](#g_t_0040indent), Previous: [`@raggedright`: Ragged Right Text](#g_t_0040raggedright), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 7.12 `@noindent`: Omitting Indentation [¶](#g_t_0040noindent_003a-Omitting-Indentation)

An example or other inclusion can break a paragraph into segments.
Ordinarily, the formatters indent text that follows an example as a new
paragraph. You can prevent this on a case-by-case basis by writing
`@noindent` at the beginning of a line, preceding the continuation
text. You can also disable indentation for all paragraphs globally with
`@paragraphindent` (see [`@paragraphindent`: Controlling Paragraph Indentation](#g_t_0040paragraphindent)).

Here is an example showing how to eliminate the normal indentation of
the text after an `@example`, a common situation:

```
@example
This is an example
@end example

@noindent
This line is not indented.  As you can see, the
beginning of the line is fully flush left with the
line that follows after it.
```

produces:

```
This is an example
```

```
This line is not indented.  As you can see, the
beginning of the line is fully flush left with the
line that follows after it.
```

The standard usage of `@noindent` is just as above: at the
beginning of what would otherwise be a paragraph, to eliminate the
indentation that normally happens there. It can either be followed by
text or be on a line by itself. There is no reason to use it
in other contexts, such as in the middle of a paragraph or inside an
environment (see [Quotations and Examples](#Quotations-and-Examples)).

You can control the number of blank lines in the Info file output by
adjusting the input as desired: a line containing just
`@noindent` does not generate a blank line, and neither does an
`@end` line for an environment.

Do not put braces after a `@noindent` command; they are not
used, since `@noindent` is a command used outside of paragraphs
(see [@-Command Syntax](#Command-Syntax)).

---

Next: [`@cartouche`: Rounded Rectangles](#g_t_0040cartouche), Previous: [`@noindent`: Omitting Indentation](#g_t_0040noindent), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 7.13 `@indent`: Forcing Indentation [¶](#g_t_0040indent_003a-Forcing-Indentation)

To complement the `@noindent` command (see the previous
section), Texinfo provides the `@indent` command to force a
paragraph to be indented. For instance, this paragraph (the first in
this section) is indented using an `@indent` command.

And indeed, the first paragraph of a section is the most likely place
to use `@indent`, to override the normal behavior of no
indentation there (see [`@paragraphindent`: Controlling Paragraph Indentation](#g_t_0040paragraphindent)). It can either be
followed by text or be on a line by itself.

As a special case, when `@indent` is used in an environment
where text is not filled, it produces a paragraph indentation space in
the TeX output. (These environments are where a line of input
produces a line of output, such as `@example` and
`@display`; for a summary of all environments, see [Block Enclosing Commands](#Block-Enclosing-Commands).)

Do not put braces after an `@indent` command; they are not used,
since `@indent` is a command used outside of paragraphs
(see [@-Command Syntax](#Command-Syntax)).

---

Next: [`@small…` Block Commands](#small), Previous: [`@indent`: Forcing Indentation](#g_t_0040indent), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 7.14 `@cartouche`: Rounded Rectangles [¶](#g_t_0040cartouche_003a-Rounded-Rectangles)

Where the output format allows, the `@cartouche` command draws a box
with rounded corners around its contents. In HTML, a normal rectangle is
drawn. You can use this command to isolate a portion of the manual from the
main flow. You can also further highlight an example or quotation
with `@cartouche`.

For instance, you could write a manual in which one type of example is
surrounded by a cartouche for emphasis. For example,

```
@cartouche
@example
% pwd
/usr/local/share/emacs
@end example
@end cartouche
```

surrounds the two-line example with a box with rounded corners, in the
printed manual.

The output from the example looks like this (if you’re reading this in
Info, you’ll see the `@cartouche` had no effect):

|  |
| --- |
| ``` % pwd /usr/local/share/emacs ``` |

`@cartouche` takes one optional argument, given on the remainder
of the line. This text, if present, is the cartouche title. It is
output in bold or otherwise emphasized at the beginning of the cartouche,
and is centered in some output formats.

The following example illustrates a cartouche with a title:

```
@cartouche Important
Text explaining something important out of the main
flow of the text.
@end cartouche
```

The cartouche with a title looks like this:

| Important |
| --- |
| Text explaining something important out of the main flow of the text. |

A cartouche is output on a single page in printed output, similarly to
`@group` (see [`@group`: Prevent Page Breaks](#g_t_0040group)).

---

Previous: [`@cartouche`: Rounded Rectangles](#g_t_0040cartouche), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 7.15 `@small…` Block Commands [¶](#g_t_0040small_2026-Block-Commands)

In addition to the regular `@example` and similar commands,
Texinfo has “small” example-style commands. These are
`@smallquotation`, `@smallindentedblock`,
`@smalldisplay`, `@smallexample`, `@smallformat`,
and `@smalllisp`.

In most output formats, the `@small…` commands are
equivalent to their non-small companion commands.

In printed output, however, the `@small…` commands typeset text in
a smaller font than the non-small example commands. Thus, for
instance, code examples can contain longer lines and still fit on a
page without needing to be rewritten.

Mark the end of a `@small…` block with a corresponding
`@end small…`. For example, pair `@smallexample` with
`@end smallexample`.

Here is an example of the font used by the `@smallexample`
command (in most output formats, the output will be the same as usual):

```
... to make sure that you have the freedom to
distribute copies of free software (and charge for
this service if you wish), that you receive source
code or can get it if you want it, that you can
change the software or use pieces of it in new free
programs; and that you know you can do these things.
```

The `@small…` commands use the same font style as their
normal counterparts: `@smallexample` and `@smalllisp` use
a fixed-width font, and everything else uses the regular font.
They also have the same behavior in other respects—whether filling
is done and whether margins are narrowed.

As a general rule, it’s better to just use the regular
commands (such as `@example` instead of `@smallexample`),
unless you have a good reason for it.

---

Next: [Special Displays](#Special-Displays), Previous: [Quotations and Examples](#Quotations-and-Examples), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 8 Lists and Tables [¶](#Lists-and-Tables-1)

Texinfo has several ways of making lists and tables. Lists can be
bulleted or numbered; two-column tables can highlight the items in
the first column; multi-column tables are also supported.

* [Introducing Lists](#Introducing-Lists)
* [`@itemize`: Making an Itemized List](#g_t_0040itemize)
* [`@enumerate`: Making a Numbered or Lettered List](#g_t_0040enumerate)
* [Making a Two-column Table](#Two_002dcolumn-Tables)
* [`@multitable`: Multi-column Tables](#Multi_002dcolumn-Tables)

---

Next: [`@itemize`: Making an Itemized List](#g_t_0040itemize), Up: [Lists and Tables](#Lists-and-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 8.1 Introducing Lists [¶](#Introducing-Lists-1)

Texinfo automatically indents the text in lists or tables, and numbers
an enumerated list. This last feature is useful if you modify the
list, since you do not need to renumber it yourself.

Numbered lists and tables begin with the appropriate @-command at the
beginning of a line, and end with the corresponding `@end`
command on a line by itself. The table and itemized-list commands
also require that you write formatting information on the same line as
the beginning @-command.

Begin an enumerated list, for example, with an `@enumerate`
command and end the list with an `@end enumerate` command.
Begin an itemized list with an `@itemize` command, followed on
the same line by a formatting command such as `@bullet`, and end
the list with an `@end itemize` command.

Precede each element of a list with an `@item` or `@itemx`
command.

  

Here is an itemized list of the different kinds of table and lists:

* Itemized lists with and without bullets.
* Enumerated lists, using numbers or letters.
* Two-column tables with highlighting.

  

Here is an enumerated list with the same items:

1. Itemized lists with and without bullets.
2. Enumerated lists, using numbers or letters.
3. Two-column tables with highlighting.

  

And here is a two-column table with the same items and their
@-commands:

`@itemize`
:   Itemized lists with and without bullets.

`@enumerate`
:   Enumerated lists, using numbers or letters.

`@table`

`@ftable`

`@vtable`
:   Two-column tables, optionally with indexing.

---

Next: [`@enumerate`: Making a Numbered or Lettered List](#g_t_0040enumerate), Previous: [Introducing Lists](#Introducing-Lists), Up: [Lists and Tables](#Lists-and-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 8.2 `@itemize`: Making an Itemized List [¶](#g_t_0040itemize_003a-Making-an-Itemized-List)

The `@itemize` command produces a sequence of “items”, each
starting with a bullet or other mark inside the left margin, and
generally indented.

Begin an itemized list by writing `@itemize` at the beginning of
a line. Follow the command, on the same line, with a character or a
Texinfo command that generates a mark. Usually, you will use
`@bullet` after `@itemize`, but you can use
`@minus`, or any command or character that results in a single
character in the Info file. (When you write the mark command such as
`@bullet` after an `@itemize` command, you may omit the
‘`{}`’.) If you don’t specify a mark command, the default is
`@bullet`. If you don’t want any mark at all, but still want
logical items, use `@w{}` (in this case the braces are
required).

After the `@itemize`, write your items, each starting with
`@item`. Text can follow on the same line as the `@item`.
The text of an item can continue for more than one paragraph.

There should be at least one `@item` inside the `@itemize`
environment. If none are present, `texi2any` gives a warning.
If you just want indented text and not a list of items, use
`@indentedblock`; see [`@indentedblock`: Indented text blocks](#g_t_0040indentedblock).

Index entries and comments that are given before an `@item`
including the first, are automatically moved (internally) to after the
`@item`, so the output is as expected. Historically this has
been a common practice.

Usually, you should put a blank line between items. This puts a blank
line in the Info file. (TeX inserts the proper vertical space in
any case.) Except when the entries are very brief, these blank lines
make the list look better.

Here is an example of the use of `@itemize`, followed by the
output it produces. `@bullet` produces an ‘`*`’ in Info and
a round dot in other output formats.

```
@itemize @bullet
@item
Some text for foo.

@item
Some text
for bar.
@end itemize
```

This produces:

> * Some text for foo.
> * Some text
>   for bar.

Itemized lists may be embedded within other itemized lists. Here is a
list marked with dashes embedded in a list marked with bullets:

```
@itemize @bullet
@item
First item.

@itemize @minus
@item
Inner item.

@item
Second inner item.
@end itemize

@item
Second outer item.
@end itemize
```

This produces:

> * First item.
>   + Inner item.
>   + Second inner item.
> * Second outer item.

---

Next: [Making a Two-column Table](#Two_002dcolumn-Tables), Previous: [`@itemize`: Making an Itemized List](#g_t_0040itemize), Up: [Lists and Tables](#Lists-and-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 8.3 `@enumerate`: Making a Numbered or Lettered List [¶](#g_t_0040enumerate_003a-Making-a-Numbered-or-Lettered-List)

`@enumerate` is like `@itemize` (see [`@itemize`: Making an Itemized List](#g_t_0040itemize)),
except that the labels on the items are successive integers or letters
instead of bullets.

Write the `@enumerate` command at the beginning of a line. The
command does not require an argument, but accepts either a number or a
letter as an option. Without an argument, `@enumerate` starts the
list with the number ‘`1`’. With a numeric argument, such as
‘`3`’, the command starts the list with that number. With an upper-
or lowercase letter, such as ‘`a`’ or ‘`A`’, the command starts
the list with that letter.

Write the text of the enumerated list in the same way as an itemized
list: write a line starting with `@item` at the beginning of
each item in the enumeration. It is ok to have text following the
`@item`, and the text for an item can continue for several
paragraphs.

You should put a blank line between entries in the list.
This generally makes it easier to read the Info file.

Here is an example of `@enumerate` without an argument:

```
@enumerate
@item
Underlying causes.

@item
Proximate causes.
@end enumerate
```

This produces:

1. Underlying causes.
2. Proximate causes.

  

Here is an example with an argument of `3`:

  

```
@enumerate 3
@item
Predisposing causes.

@item
Precipitating causes.

@item
Perpetuating causes.
@end enumerate
```

This produces:

3. Predisposing causes.
4. Precipitating causes.
5. Perpetuating causes.

  

Here is a summary:

  

1. `@enumerate`

   Without an argument, produce a numbered list, with the first item
   numbered 1.
2. `@enumerate unsigned-integer`

   With an (unsigned) numeric argument, start a numbered list with that
   number. You can use this to continue a list that you interrupted with
   other text.
3. `@enumerate upper-case-letter`

   With an uppercase letter as argument, start a list
   in which each item is marked
   by a letter, beginning with that uppercase letter.
4. `@enumerate lower-case-letter`

   With a lowercase letter as argument, start a list
   in which each item is marked by
   a letter, beginning with that lowercase letter.

You can also nest enumerated lists, as in an outline.

---

Next: [`@multitable`: Multi-column Tables](#Multi_002dcolumn-Tables), Previous: [`@enumerate`: Making a Numbered or Lettered List](#g_t_0040enumerate), Up: [Lists and Tables](#Lists-and-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 8.4 Making a Two-column Table [¶](#Making-a-Two_002dcolumn-Table)

`@table` is similar to `@itemize`
(see [`@itemize`: Making an Itemized List](#g_t_0040itemize)), but allows you to specify a name or
heading line for each item. The `@table` command is used to
produce two-column tables, and is especially useful for glossaries,
explanatory exhibits, and command-line option summaries.

* [Using the `@table` Command](#g_t_0040table)
* [`@ftable` and `@vtable`](#g_t_0040ftable-_0040vtable)
* [`@itemx`: Second and Subsequent Items](#g_t_0040itemx)

---

Next: [`@ftable` and `@vtable`](#g_t_0040ftable-_0040vtable), Up: [Making a Two-column Table](#Two_002dcolumn-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 8.4.1 Using the `@table` Command [¶](#Using-the-_0040table-Command)

Use the `@table` command to produce a two-column table. This
command is typically used when you have a list of items and a brief text
with each one, such as a list of definitions.

Write the `@table` command at the beginning of a line, after a
blank line, and follow it on the same line with an argument that is an
‘indicating’ command, such as `@code`, `@samp`,
`@var`, `@option`, or `@kbd` (see [Indicating Definitions, Commands, etc.](#Indicating)).
This command will be applied to the text in the first column. For
example, `@table @code` will cause the text in the first column
to be output as if it had been the argument to a `@code` command.

You may use the `@asis` command as an argument to
`@table`. `@asis` is a command that does nothing: if you
use this command after `@table`, the first column entries are
output without added highlighting (“as is”).

The `@table` command works with other commands besides those
explicitly mentioned here. However, you can only use predefined
Texinfo commands that take an argument in braces. You cannot
reliably use a new command defined with `@macro`, although an
`@alias` (for a suitable predefined command) is acceptable.
See [Defining New Texinfo Commands](#Defining-New-Texinfo-Commands).

Begin each table entry with an `@item` command at the beginning
of a line. Write the text for the first column on the same line as the
`@item` command. Write the text for the second column on the line
following the `@item` line and on subsequent lines. You may
write as many lines of supporting text as you wish, even several
paragraphs. But only the text on the same line as the `@item`
will be placed in the first column (including any footnotes).
You do not need to type anything for an empty second column.

Normally, you should put a blank line between table entries. This puts
a blank line in the Info file, which looks better unless the entries
are very brief.

End the table with a line consisting of `@end table`.
Follow the end of the table by a blank line for consistent formatting
across output formats.

For example, the following table highlights the text in the first
column with the `@samp` command:

```
@table @samp
@item foo
This is the text for
@samp{foo}.

@item bar
Text for @samp{bar}.
@end table
```

This produces:

‘`foo`’
:   This is the text for
    ‘`foo`’.

‘`bar`’
:   Text for ‘`bar`’.

If you want to list two or more named items with a single block of
text, use the `@itemx` command. (See [`@itemx`: Second and Subsequent Items](#g_t_0040itemx).)

The `@table` command (see [Using the `@table` Command](#g_t_0040table)) is not supported
inside `@display`. Since `@display` is line-oriented, it
doesn’t make sense to use them together. If you want to indent a
table, try `@quotation` (see [`@quotation`: Block Quotations](#g_t_0040quotation)) or
`@indentedblock` (see [`@indentedblock`: Indented text blocks](#g_t_0040indentedblock)).

---

Next: [`@itemx`: Second and Subsequent Items](#g_t_0040itemx), Previous: [Using the `@table` Command](#g_t_0040table), Up: [Making a Two-column Table](#Two_002dcolumn-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 8.4.2 `@ftable` and `@vtable` [¶](#g_t_0040ftable-and-_0040vtable)

The `@ftable` and `@vtable` commands are the same as the
`@table` command except that `@ftable` automatically enters
each of the items in the first column of the table into the index of
functions and `@vtable` automatically enters each of the items in
the first column of the table into the index of variables. This
simplifies the task of creating indices. Only the items on the same
line as the `@item` or `@itemx` commands are indexed, and
they are indexed in exactly the form that they appear on that line.
See [Indices](#Indices), for more information about indices.

Begin a two-column table using `@ftable` or `@vtable` by
writing the @-command at the beginning of a line, followed on the same
line by an argument that is a Texinfo command such as `@code`,
exactly as you would for a `@table` command; and end the table
with an `@end ftable` or `@end vtable` command on a line by
itself.

See the example for `@table` in the previous section.

---

Previous: [`@ftable` and `@vtable`](#g_t_0040ftable-_0040vtable), Up: [Making a Two-column Table](#Two_002dcolumn-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 8.4.3 `@itemx`: Second and Subsequent Items [¶](#g_t_0040itemx_003a-Second-and-Subsequent-Items)

Use the `@itemx` command inside a table when you have two or more
first column entries for the same item, each of which should appear on a
line of its own.

Use `@item` for the first entry, and `@itemx` for all
subsequent entries; `@itemx` must always follow an `@item`
command, with no blank line intervening.

The `@itemx` command works exactly like `@item` except that
it does not generate extra vertical space above the first column text in some
output formats. If you have multiple consecutive `@itemx` commands, do
not insert any blank lines between them.

For example,

```
@table @code
@item upcase
@itemx downcase
These two functions accept a character or a string as
argument, and return the corresponding uppercase (lowercase)
character or string.
@end table
```

This produces:

`upcase`

`downcase`
:   These two functions accept a character or a string as
    argument, and return the corresponding uppercase (lowercase)
    character or string.

(Note also that this example illustrates multi-line supporting text in
a two-column table.)

---

Previous: [Making a Two-column Table](#Two_002dcolumn-Tables), Up: [Lists and Tables](#Lists-and-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 8.5 `@multitable`: Multi-column Tables [¶](#g_t_0040multitable_003a-Multi_002dcolumn-Tables)

`@multitable` allows you to construct tables with any number of
columns, with each column having any width you like.

You define the column widths on the `@multitable` line itself, and
write each row of the actual table following an `@item` command,
with columns separated by a `@tab` command. Finally, `@end
multitable` completes the table. Details in the sections below.

* [Multitable Column Widths](#Multitable-Column-Widths)
* [Multitable Rows](#Multitable-Rows)

---

Next: [Multitable Rows](#Multitable-Rows), Up: [`@multitable`: Multi-column Tables](#Multi_002dcolumn-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 8.5.1 Multitable Column Widths [¶](#Multitable-Column-Widths-1)

You can define the column widths for a multitable in two ways: as
fractions of the line length; or with a prototype row. Mixing the two
methods is not supported. In either case, the widths are defined
entirely on the same line as the `@multitable` command.

1. To specify column widths as fractions of the line length, write
   `@columnfractions` and the decimal numbers (presumably less than
   1; a leading zero is allowed and ignored) after the
   `@multitable` command, as in:

   ```
   @multitable @columnfractions .33 .33 .33
   ```

   The fractions need not add up exactly to 1.0, as these do not. This
   allows you to produce tables that do not need the full line length.

   When using `@columnfractions`, the leftmost column may appear
   slightly wider than you might expect, relative to the other columns.
   This is due to spacing between columns being included in the width of
   the other columns.
2. To specify a prototype row, write the longest entry for each column
   enclosed in braces after the `@multitable` command. For example:

   ```
   @multitable {some text for column one} {for column two}
   ```

   The first column will then have the width of the typeset ‘some text for
   column one’, and the second column the width of ‘for column two’.

   The prototype entries need not appear in the table itself.

   Although we used simple text in this example, the prototype entries can
   contain Texinfo commands; markup commands such as `@code` are
   particularly likely to be useful.

   Prototype rows have no effect in HTML output.

---

Previous: [Multitable Column Widths](#Multitable-Column-Widths), Up: [`@multitable`: Multi-column Tables](#Multi_002dcolumn-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 8.5.2 Multitable Rows [¶](#Multitable-Rows-1)

After the `@multitable` command defining the column widths (see
the previous section), you begin each row in the body of a multitable
with `@item`, and separate the column entries with `@tab`.
Line breaks are not special within the table body, and you may break
input lines in your source file as necessary.

You can also use `@headitem` instead of `@item` to produce
a *heading row*. The TeX output for such a row is in bold, and
the HTML and DocBook output uses the `<thead>` tag. In Info, the
heading row is followed by a separator line made of dashes (‘`-`’
characters).

The command `@headitemfont` can be used in templates when the
entries in a `@headitem` row need to be used in a template. It
is a synonym for `@b`, but using `@headitemfont` avoids
any dependency on that particular font style, in case we provide a way
to change it in the future.

Here is a complete example of a multi-column table (the text is from
The GNU Emacs Manual, see [Splitting Windows](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Split-Window) in The GNU Emacs Manual):

```
@multitable @columnfractions .15 .45 .4
@headitem Key @tab Command @tab Description
@item C-x 2
@tab @code{split-window-vertically}
@tab Split the selected window into two windows,
with one above the other.
@item C-x 3
@tab @code{split-window-horizontally}
@tab Split the selected window into two windows
positioned side by side.
@item C-Mouse-2
@tab
@tab In the mode line or scroll bar of a window,
split that window.
@end multitable
```

produces:

| Key | Command | Description |
| --- | --- | --- |
| C-x 2 | `split-window-vertically` | Split the selected window into two windows, with one above the other. |
| C-x 3 | `split-window-horizontally` | Split the selected window into two windows positioned side by side. |
| C-Mouse-2 |  | In the mode line or scroll bar of a window, split that window. |

---

Next: [Indices](#Indices), Previous: [Lists and Tables](#Lists-and-Tables), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 9 Special Displays [¶](#Special-Displays-1)

The commands in this chapter allow you to write text that is specially
displayed (output format permitting), outside of the normal document
flow.

One set of such commands is for creating “floats”, that is, figures,
tables, and the like, set off from the main text, possibly numbered,
captioned, and/or referred to from elsewhere in the document. Images
are often included in these displays.

Another group of commands is for creating footnotes in Texinfo.

* [Floats](#Floats)
* [Inserting Images](#Images)
* [Footnotes](#Footnotes)

---

Next: [Inserting Images](#Images), Up: [Special Displays](#Special-Displays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 9.1 Floats [¶](#Floats-1)

A *float* is a display which is set off from the main text. It is
typically labeled as being a “Figure”, “Table”, “Example”, or
some similar type.

A float is so-named because, in principle, it can be moved to the
bottom or top of the current page, or to a following page, in the
printed output. (Floating does not make sense in other output
formats.) In every output format except for LaTeX, however,
this floating is unfortunately not yet implemented. Instead, the
floating material is simply output at the current location, more
or less as if it were an `@group` (see [`@group`: Prevent Page Breaks](#g_t_0040group)).

* [`@float` [type][,label]: Floating Material](#g_t_0040float)
* [`@caption` & `@shortcaption`](#g_t_0040caption-_0040shortcaption)
* [`@listoffloats`: Tables of Contents for Floats](#g_t_0040listoffloats)

---

Next: [`@caption` & `@shortcaption`](#g_t_0040caption-_0040shortcaption), Up: [Floats](#Floats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 9.1.1 `@float` [type][,label]: Floating Material [¶](#g_t_0040float-_005btype_005d_005b_002clabel_005d_003a-Floating-Material)

To produce floating material, enclose the material you want to be
displayed separate between `@float` and `@end float`
commands, on lines by themselves.

Floating material often uses `@image` to display an
already-existing graphic (see [Inserting Images](#Images)), or `@multitable` to
display a table (see [`@multitable`: Multi-column Tables](#Multi_002dcolumn-Tables)). However, the contents
of the float can be anything. Here’s an example with simple text:

```
@float Figure,fig:ex1
This is an example float.
@end float
```

And the output:

This is an example float.

**Figure 9.1**

As shown in the example, `@float` takes two arguments (separated
by a comma), type and label. Both are optional.

type
:   Specifies the sort of float this is; typically a word such as
    “Figure”, “Table”, etc. If this is not given, and label is,
    any cross-referencing will simply use a bare number.

label
:   Specifies a cross-reference label for this float. If given, this
    float is automatically given a number, and will appear in any
    `@listoffloats` output (see [`@listoffloats`: Tables of Contents for Floats](#g_t_0040listoffloats)). Cross
    references to label are allowed. For example,
    ‘`see @ref{fig:ex1}`’ will produce see [Figure 9.1](#fig_003aex1).

    On the other hand, if label is not given, then the float will
    not be numbered and consequently will not appear in the
    `@listoffloats` output or be cross-referenceable.

Ordinarily, you specify both type and label, to get a
labeled and numbered float.

In the LaTeX output, code loading the `float` package is
output in the preamble if `@float` are present.
A `@float` with type ‘`figure`’ or ‘`table`’
(case insensitive) is already defined by the package. Other
float types lead to the definition of a new float environment,
with names based on the `@float` type with anything else
than letters and ‘`-`’ removed.

In Texinfo, all floats are numbered in the same way: with the chapter
number (or appendix letter), a period, and the float number, which
simply counts 1, 2, 3, …, and is reset at each chapter. Each
float type is counted independently.

Floats within an `@unnumbered`, or outside of any chapter, are
simply numbered consecutively from 1.

These numbering conventions are not, at present, changeable.

---

Next: [`@listoffloats`: Tables of Contents for Floats](#g_t_0040listoffloats), Previous: [`@float` [type][,label]: Floating Material](#g_t_0040float), Up: [Floats](#Floats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 9.1.2 `@caption` & `@shortcaption` [¶](#g_t_0040caption-_0026-_0040shortcaption)

You may write a `@caption` anywhere within a `@float`
environment, to define a caption for the float. It is not allowed in
any other context. `@caption` takes a single argument, enclosed
in braces. Here’s an example:

```
@float
An example float, with caption.
@caption{Caption for example float.}
@end float
```

The output is:

An example float, with caption.

Caption for example float.

`@caption` can appear anywhere within the float; it is not
processed until the `@end float`. The caption text is usually a
sentence or two, but may consist of several paragraphs if necessary.

In the output, the caption always appears below the float; this is not
currently changeable. It is preceded by the float type and/or number,
as specified to the `@float` command (see the previous section).

The `@shortcaption` command likewise may be used only within
`@float`, and takes a single argument in braces. The short
caption text is used instead of the caption text in a list of floats
(see the next section). Thus, you can write a long caption for the
main document, and a short title to appear in the list of floats. For
example:

```
@float
... as above ...
@shortcaption{Text for list of floats.}
@end float
```

The text for `@shortcaption` may not contain comments
(`@c`), verbatim text (`@verb`), environments such as
`@example`, footnotes (`@footnote`), multiple paragraphs,
or other complex constructs. The same constraints apply to
`@caption` unless there is a `@shortcaption`. In
LaTeX a multi-paragraph `@caption` will lead to an error
when formatted, unless there is a `@shortcaption`.

---

Previous: [`@caption` & `@shortcaption`](#g_t_0040caption-_0040shortcaption), Up: [Floats](#Floats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 9.1.3 `@listoffloats`: Tables of Contents for Floats [¶](#g_t_0040listoffloats_003a-Tables-of-Contents-for-Floats)

You can write a `@listoffloats` command to generate a list of
floats for a given float type (see [`@float` [type][,label]: Floating Material](#g_t_0040float)), analogous to
the document’s overall table of contents. Typically, it is written in
its own `@unnumbered` node to provide a heading and structure,
rather like `@printindex` (see [Printing Indices and Menus](#Printing-Indices-_0026-Menus)).

`@listoffloats` takes one optional argument, the float type.
Here’s an example:

```
@node List of Figures
@unnumbered List of Figures
@listoffloats Figure
```

Without any argument, `@listoffloats` generates a list of
floats for which no float type was specified, i.e., no first argument to the
`@float` command (see [`@float` [type][,label]: Floating Material](#g_t_0040float)).

Here’s what the output from `@listoffloats`
looks like, given the example figure earlier in this chapter:

[Figure 9.1](#fig_003aex1)

Usually, each line in the list of floats contains the float type (if any),
the float number, and the caption, if any—the `@shortcaption`
argument, if it was specified, else the `@caption` argument.
The page number may also be included, depending on output format.

Unnumbered floats (those without cross-reference labels) are omitted
from the list of floats.

The formatting of `@listoffloats` depends on the output
format. In Info, for example, a `@listoffloats` is formatted
as a menu.

In LaTeX output, `\listoffigures` is output for
the ‘`figure`’ (case insensitive) float type, `\listoftables` is output
for the ‘`table`’ (case insensitive) float type. For other float types, a
specific `\listof` command is output.

---

Next: [Footnotes](#Footnotes), Previous: [Floats](#Floats), Up: [Special Displays](#Special-Displays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 9.2 Inserting Images [¶](#Inserting-Images)

You can insert an image given in an external file with the
`@image` command. Although images can be used anywhere,
including the middle of a paragraph, we describe them in this chapter
since they are most often part of a displayed figure or example.

* [Image Syntax](#Image-Syntax)
* [Image Scaling](#Image-Scaling)

---

Next: [Image Scaling](#Image-Scaling), Up: [Inserting Images](#Images)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 9.2.1 Image Syntax [¶](#Image-Syntax-1)

Here is the synopsis of the `@image` command:

```
@image{filename[, width[, height[, alttext[, extension]]]]}
```

The filename argument is mandatory, and must not have an
extension, because the different processors support different formats:

* TeX (DVI output) reads the file `filename.eps`
  (Encapsulated PostScript format).
* TeX (PDF output) reads `filename.pdf`, `filename.png`,
  `filename.jpg`, or `filename.jpeg` (in that
  order). It also tries uppercase versions of the extensions. The PDF
  format does not support EPS images, so such must be converted first.
* In Info, `filename.txt` is included verbatim
  (more or less as if it were in `@verbatim`). The Info output
  may also include a reference to `filename.png` or
  `filename.jpg`. (See below.)
* In HTML, a reference to `filename.png`,
  `filename.jpg`, `filename.jpeg` or
  `filename.gif` (in that order) is output. If
  none of those exist, it gives an error, and outputs a
  reference to `filename.jpg` anyway.
* In DocBook, references to
  `filename.eps`, `filename.gif`,
  `filename.jpeg`, `filename.jpg`,
  `filename.pdf`, `filename.png` and
  `filename.svg` are output, for every file found. Also,
  `filename.txt` is included verbatim, if present. (The
  subsequent DocBook processor is supposed to choose the appropriate one.)
* For LaTeX, `filename` without any extension is used;
  the subsequent LaTeX processor is supposed to choose the
  appropriate image type.
* For Info and HTML output, the optional fifth argument extension
  to `@image` is used for the file extension,
  if it is specified and the file is found. Any leading period should
  be included in extension. For example:

  ```
  @image{foo,,,,.xpm}
  ```

If you want to install image files for use by Info readers too, we
recommend putting them in a subdirectory like ‘`foo-figures`’
for a package foo. Copying the files into
`$(infodir)/foo-figures/` should be done in your
`Makefile`.

The width and height arguments are described in the next
section.

If an image is the first thing in a paragraph and followed by
more text, then you should precede the `@image` command with
`@indent` or `@noindent` to indicate the beginning of
paragraph formatting. This is especially important for TeX output
to get correct paragraph indentation.

Use `@center` to center an image
(see [`@titlefont`, `@center`, and `@sp`](#g_t_0040titlefont-_0040center-_0040sp)).

For HTML output, the *alt attribute* for
inline images is set to the optional alttext (fourth) argument to
`@image`, if supplied. If not supplied, the full file name of
the image being displayed is used. The alttext is
processed as Texinfo text, so special characters such as ‘`"`’ and
‘`<`’ and ‘`&`’ are escaped in the HTML output; also, you can
get an empty `alt` string with `@-` (a command that
produces no output; see [`@-` and `@hyphenation`: Hyphenation in Printed Output](#g_t_0040_002d-_0040hyphenation)).

For Info output, the alttext string is also processed as Texinfo
text and output. In this case, ‘`\`’ is escaped as ‘`\\`’ and
‘`"`’ as ‘`\"`’; no other escapes are done.

In Info output, a reference to the binary image file is written
(trying filename suffixed with `extension`,
`.extension`, `.png`, or `.jpg`, in that order)
if one exists. The `.txt` file is also literally included, if one
exists. This way, Info readers which can display images (such as the
Emacs Info browser, running under X) can do so, whereas Info readers
which can only use text (such as the standalone Info reader) can
display the textual version.

---

Previous: [Image Syntax](#Image-Syntax), Up: [Inserting Images](#Images)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 9.2.2 Image Scaling [¶](#Image-Scaling-1)

The optional width and height arguments to the
`@image` command (see the previous section) specify the size to
which to scale the image. They are only taken into account in printed
output.

In TeX, if neither is specified, the image is presented in its natural size
(given in the file); if only one is specified, the other is scaled
proportionately; and if both are specified, both are respected, thus likely
distorting the original image by changing its aspect ratio.

The width and height may be specified using any valid TeX
dimension, namely:

pt [¶](#index-Points-_0028dimension_0029)
:   point (72.27pt = 1in)

pc [¶](#index-Picas)
:   pica (1pc = 12pt)

bp [¶](#index-Big-points)
:   big point (72bp = 1in)

in [¶](#index-Inches)
:   inch

cm [¶](#index-Centimeters)
:   centimeter (2.54cm = 1in)

mm [¶](#index-Millimeters)
:   millimeter (10mm = 1cm)

dd [¶](#index-Didot-points)
:   didôt point (1157dd = 1238pt)

cc [¶](#index-Ciceros)
:   cicero (1cc = 12dd)

sp [¶](#index-Scaled-points)
:   scaled point (65536sp = 1pt)

For example, the following will scale a file `ridt.eps` to one
inch vertically, with the width scaled proportionately:

```
@image{ridt,,1in}
```

For `@image` to work with TeX, the file `epsf.tex` must be
installed somewhere that TeX can find it. (The standard location is
`texmf/tex/generic/dvips/epsf.tex`, where texmf is a
root of your TeX directory tree.) This file is included in the
Texinfo distribution and is also available from
<ftp://tug.org/tex/epsf.tex>, among other places.

`@image` can be used within a line as well as for displayed
figures. Therefore, if you intend it to be displayed, be sure to leave
a blank line before the command, or the output will run into the
preceding text.

Image scaling is presently implemented only in printed output, not in
any other sort of output.

---

Previous: [Inserting Images](#Images), Up: [Special Displays](#Special-Displays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 9.3 Footnotes [¶](#Footnotes-1)

A *footnote* is for a reference that documents or elucidates the
primary text.[5](#FOOT5)

Footnotes are distracting; use them sparingly at most, and it is best
to avoid them completely. Standard bibliographical references are
usually better placed in a bibliography at the end of a document
instead of in footnotes throughout.

* [Footnote Commands](#Footnote-Commands)
* [Footnote Styles](#Footnote-Styles)

---

Next: [Footnote Styles](#Footnote-Styles), Up: [Footnotes](#Footnotes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 9.3.1 Footnote Commands [¶](#Footnote-Commands-1)

In Texinfo, footnotes are created with the `@footnote` command.
This command is followed immediately by a left brace, then by the text
of the footnote, and then by a terminating right brace. Footnotes may
be of any length (they will be broken across pages if necessary), but
are usually short. The template is:

```
ordinary text@footnote{text of footnote}
```

As shown here, the `@footnote` command should come right after the
text being footnoted, with no intervening space; otherwise, the footnote
marker might end up starting a line.

For example, this clause is followed by a sample footnote[6](#FOOT6); in the Texinfo source, it looks like
this:

```
...a sample footnote@footnote{Here is the sample
footnote.}; in the Texinfo source...
```

As you can see, this source includes two punctuation marks next to
each other; in this case, ‘`.};`’ is the sequence. This is normal
(the first ends the footnote and the second belongs to the sentence
being footnoted), so don’t worry that it looks odd. (Another style,
perfectly acceptable, is to put the footnote after punctuation
belonging to the sentence, as in ‘`;@footnote{...`’.)

In printed output formats, the reference mark for a footnote is a
small, superscripted number; the text of the footnote appears at the
bottom of the page, below a horizontal line.

In Info, the reference mark for a footnote is a pair of parentheses
with the footnote number between them, like this: ‘`(1)`’. The
reference mark is followed by a cross-reference link to the footnote
text if footnotes are put in separate nodes (see [Footnote Styles](#Footnote-Styles)).

In the HTML output, footnote references are generally marked with a
small, superscripted number which is rendered as a hypertext link to
the footnote text.

Footnotes cannot be nested, and cannot appear in section headings of
any kind or other “unusual” places.

A final tip: footnotes in the argument of an `@item` command for
an `@table` must be entirely on the same line as the
`@item` (as usual). See [Making a Two-column Table](#Two_002dcolumn-Tables).

---

Previous: [Footnote Commands](#Footnote-Commands), Up: [Footnotes](#Footnotes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 9.3.2 Footnote Styles [¶](#Footnote-Styles-1)

Online formats have two footnote styles, which determine where
the text of the footnote is located, the ‘`end`’ and ‘`separate`’
footnote style.

‘`end`’
:   For Info, in the ‘`end`’ node style, all the footnotes for a single
    node are placed at the end of that node. The footnotes are separated
    from the rest of the node by a line of dashes with the word
    ‘`Footnotes`’ within it. Each footnote begins with an
    ‘`(n)`’ reference mark.

    Here is an example of the Info output for a single footnote in the
    end-of-node style:

    ```
    --------- Footnotes ---------

    (1)  Here is a sample footnote.
    ```

    In HTML, when the footnote style is ‘`end`’, or if the output is
    not split, footnotes are put at the end of each output file.

‘`separate`’
:   For Info, in the ‘`separate`’ node style, all the footnotes
    for a single node are placed in an automatically constructed
    node of their own. In this style, a “footnote reference” follows
    each ‘`(n)`’ reference mark in the body of the
    node. The footnote reference is actually a cross-reference
    which you use to reach the footnote node.

    The name of the node with the footnotes is constructed
    by appending ‘`-Footnotes`’ to the name of the node
    that contains the footnotes. (Consequently, the footnotes’
    node for the `Footnotes` node is
    `Footnotes-Footnotes`!) The footnotes’ node has an
    ‘Up’ node pointer that leads back to its parent node.

    Here is how the first footnote in this manual looks after being
    formatted for Info in the separate node style:

    ```
    File: texinfo.info  Node: Overview-Footnotes, Up: Overview

    (1) The first syllable of "Texinfo" is pronounced like
    "speck", not "hex". ...
    ```

    In HTML, when the footnote style is ‘`separate`’, and the output
    is split, footnotes are placed in a separate file.

Unless your document has long and important footnotes (as in, say,
Gibbon’s Decline and Fall …), we recommend the ‘`end`’
style, as it is simpler for readers to follow.

Use the `@footnotestyle` command to specify the
footnote style. Write this command at the beginning of a line followed
by an argument, either ‘`end`’ for the end node style or
‘`separate`’ for the separate node style.

For example,

```
@footnotestyle end
```

or

```
@footnotestyle separate
```

Write a `@footnotestyle` command in the Texinfo file preamble.

---

Next: [Special Insertions](#Insertions), Previous: [Special Displays](#Special-Displays), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 10 Indices [¶](#Indices-1)

Using Texinfo, you can generate indices without having to sort and
collate entries manually. In an index, the entries are listed in
alphabetical order, together with information on how to find the
discussion of each entry. In a printed manual, this information
consists of page numbers. In other formats, links to the index
entries location or to the associated nodes are generated.

Texinfo provides several predefined kinds of indices: an index for
functions, an index for variables, an index for concepts, and so on.
You can combine indices or use them for other than their canonical
purpose. Lastly, you can define your own new indices.

* [Predefined Indices](#Predefined-Indices)
* [Defining the Entries of an Index](#Indexing-Commands)
* [Advanced Indexing Commands](#Advanced-Indexing)
* [Making Index Entries](#Index-Entries)
* [Printing Indices and Menus](#Printing-Indices-_0026-Menus)
* [Combining Indices](#Combining-Indices)
* [Defining New Indices](#New-Indices)

---

Next: [Defining the Entries of an Index](#Indexing-Commands), Up: [Indices](#Indices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 10.1 Predefined Indices [¶](#Predefined-Indices-1)

Texinfo provides six predefined indices. Here are their nominal
meanings, abbreviations, and the corresponding index entry commands:

‘`cp`’ [¶](#index-cp-_0028concept_0029-index)
:   (`@cindex`) Concept index, for general concepts.

‘`fn`’ [¶](#index-fn-_0028function_0029-index)
:   (`@findex`) Function index, for function and function-like
    names (such as entry points of libraries).

‘`ky`’ [¶](#index-ky-_0028keystroke_0029-index)
:   (`@kindex`) Keystroke index, for keyboard commands.

‘`pg`’ [¶](#index-pg-_0028program_0029-index)
:   (`@pindex`) Program index, for names of programs.

‘`tp`’ [¶](#index-tp-_0028data-type_0029-index)
:   (`@tindex`) Data type index, for type names (such as structures
    defined in header files).

‘`vr`’ [¶](#index-vr-_0028variable_0029-index)
:   (`@vindex`) Variable index, for variable names (such as library global
    variables).

Not every manual needs all of these, and most manuals use only two or
three at most. The present manual, for example, has two indices: a
concept index and an @-command index. (The latter is actually the function
index but is called a command index in the chapter heading.)

You are not required to use the predefined indices strictly for their
canonical purposes. For example, suppose you wish to index some C
preprocessor macros. You could put them in the function index along
with actual functions, just by writing `@findex` commands for
them; then, when you print the “Function Index” as an unnumbered
chapter, you could give it the title ‘Function and Macro Index’ and
all will be consistent for the reader.

On the other hand, it is best not to stray too far from the meaning of
the predefined indices. Otherwise, in the event that your text is
combined with other text from other manuals, the index entries will
not match up. Instead, define your own new index (see [Defining New Indices](#New-Indices)).

We recommend having a single index in the final document whenever
possible, however many source indices you use, since then readers have
only one place to look. Two or more source indices can be combined
into one output index by using the `@synindex` or
`@syncodeindex` commands (see [Combining Indices](#Combining-Indices)).

Index entry commands based on the the two letter index names are also
valid, for example `@cpindex` can be used instead of `@cindex`.

---

Next: [Advanced Indexing Commands](#Advanced-Indexing), Previous: [Predefined Indices](#Predefined-Indices), Up: [Indices](#Indices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 10.2 Defining the Entries of an Index [¶](#Defining-the-Entries-of-an-Index)

The data to make an index come from many individual indexing commands
scattered throughout the Texinfo source file. Each command says to add
one entry to a particular index; after formatting, the index will give
the current page number or node name as the reference.

An index entry consists of an indexing command at the beginning of a
line followed, on the rest of the line, by the entry.

For example, this section begins with the following five entries for
the concept index:

```
@cindex Defining indexing entries
@cindex Index entries, defining
@cindex Entries for an index
@cindex Specifying index entries
@cindex Creating index entries
```

Each predefined index has its own indexing command—`@cindex`
for the concept index, `@findex` for the function index, and so
on, as listed in the previous section.

Index entries should precede the visible material that is being
indexed. For instance:

```
@cindex hello
Hello, there!
```

Among other reasons, that way following indexing links (in
whatever context) ends up before the material, where readers want to
be, instead of after.

In Info, an index is usually formatted as a menu. Try to avoid using
a colon in index entries, as this may confuse some Info readers.
See [The Parts of a Menu](#Menu-Parts) for more information about the structure of a menu entry.

By default, entries for a concept index are printed in a roman
font and entries for the other indices are printed in a typewriter font.
You may change the way part of an entry is printed with the usual
Texinfo commands, such as `@file` for
file names (see [Marking Text, Words and Phrases](#Marking-Text)), and `@r` for the normal roman
font (see [Fonts for Printing](#Fonts)).

You may specify an explicit sort key for an
index entry using `@sortas` following either the index command
or the text of the entry. For example: ‘`@findex @sortas{\}
\ @r{(literal \ in @code{@@math})`’ sorts the index entry this
produces under backslash.

You may choose to ignore certain characters in index entries
for the purposes of sorting. The characters that you can
currently choose to ignore are ‘`\`’, ‘`-`’, ‘`<`’
and ‘`@`’, which are ignored by giving as an argument to the
`@set` command, respectively, `txiindexbackslashignore`,
`txiindexhyphenignore`, `txiindexlessthanignore` and
`txiindexatsignignore`. For example, specifying ‘`@set
txiindexbackslashignore`’ causes the ‘`\mathopsup`’ entry in the
index for this manual to be sorted as if it were ‘`mathopsup`’,
so that it appears among the other entries beginning with ‘M’.
This avoids the need to provide explicit sort keys for index
entries containing these characters.

When using these options, it is possible to get an index entry with
an empty sort key. To avoid this, specify a `@sortas` directive
in the index entry. For example:

```
@set txiindexbackslashignore
@findex @sortas{\} \
```

---

Next: [Making Index Entries](#Index-Entries), Previous: [Defining the Entries of an Index](#Indexing-Commands), Up: [Indices](#Indices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 10.3 Advanced Indexing Commands [¶](#Advanced-Indexing-Commands)

Texinfo provides several further commands for indexing.

First, you can create *multilevel* index entries, allowing you
to group many related subtopics under the same higher-level topic.
You do this by separating the parts of such an entry with the
`@subentry` command. Such commands might look like this:

```
@cindex Superhumans @subentry villains
@cindex Superhumans @subentry heroes
```

You may have up to three levels in an entry:

```
@cindex coffee makers @subentry electric @subentry pink
@cindex coffee makers @subentry electric @subentry blue
```

You can use the `@sortas` command mentioned earlier with any or
all of the three parts of an entry to cause them to sort differently
than they would by default.

Second, you may provide an index entry that points to another,
using the `@seeentry` (“see entry”) command. For example:

```
@cindex Indexes @seeentry{Indices}
```

Such an entry should be unique in your document; the idea is to
redirect the reader to the other entry where they will find all
the information they are looking for.

Finally, you may provide a “see also” entry using the `@seealso`
command. These entries go along with regular entries, and are grouped
together with them in the final printed index. For example:

```
@cindex Coffee
@cindex Coffee @subentry With milk and sugar
@cindex Coffee @subentry With doughnuts
@cindex Coffee @subentry Decaffeinated
@cindex Coffee @seealso{Tea}
```

When using all three of these advanced commands, *do not*
place a comma between the different parts of the index text. The
`texindex` program, which sorts the index entries and
generates the indexing formatting commands, takes care of placing
commas in the correct places for you.

Do not interrupt an index or `@subentry` entry by `@sortas`
or advanced commands with braces; place the commands with braces at
the beginning or at the end of the entry.

These features are the most useful with printed documents, and
when translating Texinfo to DocBook.

---

Next: [Printing Indices and Menus](#Printing-Indices-_0026-Menus), Previous: [Advanced Indexing Commands](#Advanced-Indexing), Up: [Indices](#Indices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 10.4 Making Index Entries [¶](#Making-Index-Entries)

Concept index entries consist of text. The best way to write an index
is to devise entries which are terse yet clear. If you can do this,
the index usually looks better if the entries are written just as they
would appear in the middle of a sentence, that is, capitalizing only
proper names and acronyms that always call for uppercase letters.
This is the case convention we use in most GNU manuals’ indices.

If you don’t see how to make an entry terse yet clear, make it longer
and clear—not terse and confusing. If many of the entries are
several words long, the index may look better if you use a different
convention: capitalize the first word of each entry. Whichever
case convention you use, use it consistently.

In any event, do not ever capitalize a case-sensitive name such as a C
or Lisp function name or a shell command; that would be a spelling
error. Entries in indices other than the concept index are symbol
names in programming languages, or program names; these names are
usually case-sensitive, so likewise use upper- and lowercase as
required.

It is a good idea to make index entries unique wherever feasible.
That way, people using the printed output or online completion of
index entries don’t see undifferentiated lists. Consider this an
opportunity to make otherwise-identical index entries be more
specific, so readers can more easily find the exact place they are
looking for.
The advanced indexing features described in [Advanced Indexing Commands](#Advanced-Indexing)
can help with this, as well.

When you are making index entries, it is good practice to think of the
different ways people may look for something. Different people
*do not* think of the same words when they look something up. A
helpful index will have items indexed under all the different words
that people may use. For example, one reader may think it obvious
that the two-letter names for indices should be listed under
“Indices, two-letter names”, since “Indices” are the general
concept. But another reader may remember the specific concept of
two-letter names and search for the entry listed as “Two letter names
for indices”. A good index will have both entries and will help both
readers.

Like typesetting, the construction of an index is a skilled art, the
subtleties of which may not be appreciated until you need to do it
yourself.

---

Next: [Combining Indices](#Combining-Indices), Previous: [Making Index Entries](#Index-Entries), Up: [Indices](#Indices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 10.5 Printing Indices and Menus [¶](#Printing-Indices-and-Menus)

`@printindex` takes one argument, a two-letter index
abbreviation. You must include the `@printindex` command at the
place in the document where you want the index to appear. This does
not happen automatically just because you use `@cindex` or other
index-entry generating commands in the Texinfo file; those just cause
the raw data for the index to be accumulated.

You should precede the `@printindex` command with
a suitable section or chapter command (usually `@appendix` or
`@unnumbered`) to supply the chapter heading and put the index
into the table of contents. Precede the chapter heading with an
`@node` line as usual.

For example:

```
@node Variable Index
@unnumbered Variable Index

@printindex vr
```

```

```

```
@node Concept Index
@unnumbered Concept Index

@printindex cp
```

The text ‘`Index`’ needs to appear in the name of the node containing the
index for the index to be found by Info readers.

If you have more than one index, we recommend placing the concept index last.

Other details of index output in output formats:

* As part of the process of creating a printed manual with TeX,
  you run a program called `texindex` (see [Formatting and Printing with TeX](#Hardcopy-with-TeX))
  to sort the raw data to produce a sorted index file. The sorted index
  file is what is actually used to print the index.

  `@printindex` reads the corresponding sorted index file and produces
  a traditional two-column index, with index terms and page numbers.
* In Info output, `@printindex` produces a special menu containing
  the line number of the entry, relative to the start of the node. Info
  readers can use this to go to the exact line of an entry, not just the
  containing node. (Older Info readers will just go to the node.)
  Here’s an example:

  ```
  * First index entry:   Top.   (line  7)
  ```
* In plain text output, `@printindex` formatting is usually similar to a
  menu in Info, showing the line number of each entry relative to the start of
  the file.
* In HTML output, `@printindex` produces links to the index
  entries.
* In DocBook and LaTeX output, it simply records the index to be printed.

---

Next: [Defining New Indices](#New-Indices), Previous: [Printing Indices and Menus](#Printing-Indices-_0026-Menus), Up: [Indices](#Indices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 10.6 Combining Indices [¶](#Combining-Indices-1)

Sometimes you will want to combine two disparate indices such as
functions and concepts, perhaps because you have few enough entries
that a separate index would look silly.

You could put functions into the concept index by writing
`@cindex` commands for them instead of `@findex` commands,
and produce a consistent manual by printing the concept index with the
title ‘Function and Concept Index’ and not printing the ‘Function
Index’ at all; but this is not a robust procedure. It works only if
your document is never included as part of another document that is
designed to have a separate function index; if your document were to
be included with such a document, the functions from your document and
those from the other would not end up together. Also, to make your
function names appear in the right font in the concept index, you
would need to enclose every one of them between the braces of
`@code`.

* [`@syncodeindex`: Combining Indices Using `@code`](#g_t_0040syncodeindex)
* [`@synindex`: Combining Indices](#g_t_0040synindex)

---

Next: [`@synindex`: Combining Indices](#g_t_0040synindex), Up: [Combining Indices](#Combining-Indices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 10.6.1 `@syncodeindex`: Combining Indices Using `@code` [¶](#g_t_0040syncodeindex_003a-Combining-Indices-Using-_0040code)

When you want to combine functions and concepts into one index, you
should index the functions with `@findex` and index the concepts
with `@cindex`, and use the `@syncodeindex` command to
redirect the function index entries into the concept index.

The `@syncodeindex` command takes two arguments; they are the name
of the index to redirect, and the name of the index to redirect it to.
The template looks like this:

```
@syncodeindex from to
```

For this purpose, the indices are given two-letter names:

‘`cp`’
:   Concept index

‘`fn`’
:   Function index

‘`ky`’
:   Key index

‘`pg`’
:   Program index

‘`tp`’
:   Data type index

‘`vr`’
:   Variable index

Write a `@syncodeindex` command before or shortly after the
end-of-header line at the beginning of a Texinfo file. For example,
to merge a function index with a concept index, write the
following:

```
@syncodeindex fn cp
```

This causes all entries designated for the function index to merge
in with the concept index instead.

To merge both a variable index and a function index into a concept
index, write the following:

```
@syncodeindex vr cp
@syncodeindex fn cp
```

The `@syncodeindex` command puts all the entries from the ‘from’
index (the redirected index) into the `@code` font, overriding
whatever default font is used by the index to which the entries are
now directed. This way, if you direct function names from a function
index into a concept index, all the function names are printed in the
`@code` font as you would expect.

---

Previous: [`@syncodeindex`: Combining Indices Using `@code`](#g_t_0040syncodeindex), Up: [Combining Indices](#Combining-Indices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 10.6.2 `@synindex`: Combining Indices [¶](#g_t_0040synindex_003a-Combining-Indices)

The `@synindex` command is nearly the same as the
`@syncodeindex` command, except that it does not put the ‘from’
index entries into the `@code` font; rather it puts them in the
roman font. Thus, you use `@synindex` when you merge a concept
index into a function index.

See [Printing Indices and Menus](#Printing-Indices-_0026-Menus), for information about printing an index
at the end of a book or creating an index menu in an Info file.

---

Previous: [Combining Indices](#Combining-Indices), Up: [Indices](#Indices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 10.7 Defining New Indices [¶](#Defining-New-Indices)

In addition to the predefined indices (see [Predefined Indices](#Predefined-Indices)),
you may use the `@defindex` and `@defcodeindex` commands
to define new indices. These commands create new indexing @-commands
with which you mark index entries. The `@defindex` command is
used like this:

```
@defindex name
```

New index names are usually two-letter words, such as ‘`au`’.
For example:

```
@defindex au
```

This defines a new index, called the ‘`au`’ index. At the same
time, it creates a new indexing command, `@auindex`, that you
can use to make index entries. Use this new indexing command just as
you would use a predefined indexing command.

For example, here is a section heading followed by a concept index
entry and two ‘`au`’ index entries.

```
@section Cognitive Semantics
@cindex kinesthetic image schemas
@auindex Johnson, Mark
@auindex Lakoff, George
```

(Evidently, ‘`au`’ serves here as an abbreviation for “author”.)

Texinfo constructs the new indexing command by concatenating the name
of the index with ‘`index`’; thus, defining an ‘`xy`’ index
leads to the automatic creation of an `@xyindex` command.

Use the `@printindex` command to print the index, as you do with
the predefined indices. For example:

```
@node Author Index
@unnumbered Author Index

@printindex au
```

The `@defcodeindex` command is like the `@defindex` command,
except that, in the printed output, it prints entries in an
`@code` font by default instead of in a roman font.

You should define new indices before the end-of-header line of a
Texinfo file, and (of course) before any `@synindex` or
`@syncodeindex` commands (see [Texinfo File Header](#Texinfo-File-Header)).

As mentioned earlier (see [Predefined Indices](#Predefined-Indices)), we recommend having
a single index in the final document whenever possible (no matter how many
source indices you use), since then readers have only one place to
look.

When creating an index, TeX creates a file whose extension is the
name of the index (see [Names of index files](#Names-of-index-files)). Therefore you
should avoid using index names that collide with extensions used for
other purposes, such as ‘`.aux`’ or ‘`.xml`’.
`texi2any` already reports an error if a new index conflicts
well-known extension name.

---

Next: [Forcing and Preventing Breaks](#Breaks), Previous: [Indices](#Indices), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 11 Special Insertions [¶](#Special-Insertions)

Texinfo provides several commands for inserting characters that have
special meaning in Texinfo, such as braces, and for other graphic
elements that do not correspond to simple characters you can type.

* [Special Characters: Inserting @ {} , \ # &](#Special-Characters)
* [Inserting Quote Characters](#Inserting-Quote-Characters)
* [Inserting Space](#Inserting-Space)
* [Inserting Accents](#Inserting-Accents)
* [Inserting Quotation Marks](#Inserting-Quotation-Marks)
* [`@sub` and `@sup`: Inserting Subscripts and Superscripts](#Inserting-Subscripts-and-Superscripts)
* [`@math` and `@displaymath`: Formatting Mathematics](#Inserting-Math)
* [Glyphs for Text](#Glyphs-for-Text)
* [Glyphs for Programming](#Glyphs-for-Programming)
* [Inserting Unicode: `@U`](#Inserting-Unicode)

---

Next: [Inserting Quote Characters](#Inserting-Quote-Characters), Up: [Special Insertions](#Insertions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]
