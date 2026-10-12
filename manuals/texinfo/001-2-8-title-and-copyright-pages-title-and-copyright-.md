### 2.8 Title and Copyright Pages [¶](#Title-and-Copyright-Pages)

In hard copy output, the manual’s name and author are usually printed on
a title page. Copyright information is usually printed on the back
(verso) of the title page. This segment must be enclosed between
`@titlepage` and `@end titlepage` commands:

```
@titlepage
@title Sample Title
```

```

```

```
@c The following two commands start the copyright page.
@page
@vskip 0pt plus 1filll
@insertcopying
@end titlepage
```

We use the `@insertcopying` command to
include the permission text from the previous section, instead of
writing it out again.

The title and copyright pages appear in printed manuals, but not in
most other output formats. In HTML, the best way to get a title page
similar to printed manuals is to set the `NO_TOP_NODE_OUTPUT`
customization variable (see [`NO_TOP_NODE_OUTPUT`](#NO_005fTOP_005fNODE_005fOUTPUT)).

* [`@titlepage`](#g_t_0040titlepage)
* [`@title`, `@subtitle`, and `@author`](#g_t_0040title-_0040subtitle-_0040author)
* [`@titlefont`, `@center`, and `@sp`](#g_t_0040titlefont-_0040center-_0040sp)
* [Copyright Page](#Copyright)
* [Heading Generation](#Heading-Generation)

---

Next: [`@title`, `@subtitle`, and `@author`](#g_t_0040title-_0040subtitle-_0040author), Up: [Title and Copyright Pages](#Titlepage-_0026-Copyright-Page)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 2.8.1 `@titlepage` [¶](#g_t_0040titlepage-1)

Start the material for the title page and following copyright page
with `@titlepage` on a line by itself and end it with
`@end titlepage` on a line by itself.

The `@end titlepage` command starts a new page and turns on page
numbering (see [Heading Generation](#Heading-Generation)). All the
material that you want to appear on unnumbered pages should be put
between the `@titlepage` and `@end titlepage` commands.

By using the `@page` command, you can force a page break within the
region delineated by the `@titlepage` and `@end titlepage`
commands and thereby create more than one unnumbered page. This is how
the copyright page is produced. (The `@titlepage` command might
perhaps have been better named the `@titleandadditionalpages`
command, but that would have been rather long!)

When you write a manual about a computer program, you should write the
version of the program to which the manual applies on the title page.
If the manual changes more frequently than the program or is independent
of it, you should also include an edition number[2](#FOOT2) for the manual.
This helps readers keep track of which manual is for which version of
the program. (The ‘Top’ node should also contain this information; see
[The ‘Top’ Node and Master Menu](#The-Top-Node).)

One method uses the `@title`, `@subtitle`, and
`@author` commands to create a title page. With this method,
you do not specify any of the actual formatting of the title page.
You specify the text you want, and Texinfo does the formatting.
The usual formatting consist of black rules under
the title and author lines and the subtitle text set flush to the
right-hand side of the page.

Texinfo also provides a second method for creating a title page.
using typesetting commands that are not to be used in the main text.
This method uses uses the `@titlefont`, `@sp`, and `@center`
commands to generate a title page in which the words on the page are
centered.

For sufficiently simple documents, and for the bastard title page in
traditional book frontmatter, Texinfo also provides a command
`@shorttitlepage` which takes the rest of the line as the title.
The argument is typeset on a page by itself and followed by a blank
page. In HTML, `@shorttitlepage` can play the same role as
`@settitle`, if `@settitle` is not set. See [`@settitle`: Set the Document Title](#g_t_0040settitle).

---

Next: [`@titlefont`, `@center`, and `@sp`](#g_t_0040titlefont-_0040center-_0040sp), Previous: [`@titlepage`](#g_t_0040titlepage), Up: [Title and Copyright Pages](#Titlepage-_0026-Copyright-Page)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 2.8.2 `@title`, `@subtitle`, and `@author` [¶](#g_t_0040title_002c-_0040subtitle_002c-and-_0040author)

You can use the `@title`, `@subtitle`, and `@author`
commands to create a title page in which the vertical and horizontal
spacing is done for you automatically.

Write the `@title`, `@subtitle`, or `@author`
commands at the beginning of a line followed by the title, subtitle,
or author. The `@author` command may be used for a quotation in
an `@quotation` block (see [`@quotation`: Block Quotations](#g_t_0040quotation));
except for that, it is an error to use any of these commands outside
of `@titlepage`.

The `@title` command normally produces a line in which the title is set
flush to the left-hand side of the page in a larger than normal font.
The title is underlined with a black rule. The title must be given on
a single line in the source file; it will be broken into multiple
lines of output is needed.

For long titles, the `@*` command may be used to specify the
line breaks in long titles if the automatic breaks do not suit. Such
explicit line breaks are generally reflected in all output formats; if
you only want to specify them for the printed output, use a
conditional (see [Conditionally Visible Text](#Conditionals)). For example:

```
@title This Long Title@inlinefmt{tex,@*} Is Broken in @TeX{}
```

The `@subtitle` command normally sets subtitles in a normal-sized font
flush to the right-hand side of the page.

The `@author` command normally sets the names of the author
or authors in a middle-sized font flush to the left-hand side of the
page on a line near the bottom of the title page. The names are
followed by a black rule that is thinner than the rule that normally
underlines the title.

There are two ways to use the `@author` command: you can write
the name or names on the remaining part of the line that starts with
an `@author` command:

```
@author by Jane Smith and John Doe
```

or you can write the names one above each other by using multiple
`@author` commands:

```
@author Jane Smith
@author John Doe
```

A template for this method looks like this:

```
@titlepage
@title name-of-manual-when-printed
@subtitle subtitle-if-any
@subtitle second-subtitle
@author author
@page
...
@end titlepage
```

---

Next: [Copyright Page](#Copyright), Previous: [`@title`, `@subtitle`, and `@author`](#g_t_0040title-_0040subtitle-_0040author), Up: [Title and Copyright Pages](#Titlepage-_0026-Copyright-Page)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 2.8.3 `@titlefont`, `@center`, and `@sp` [¶](#g_t_0040titlefont_002c-_0040center_002c-and-_0040sp)

You can also use the `@titlefont`, `@sp`, and `@center`
commands to create a title page for a printed document.

Use the `@titlefont` command to select a large font suitable for
the title itself. You can use `@titlefont` more than once if you
have an especially long title.

For HTML output, each `@titlefont` command produces an
`<h1>` heading, but the HTML document `<title>` is not
affected. For that, you could put a `@settitle` command before
the `@titlefont` command (see [`@settitle`: Set the Document Title](#g_t_0040settitle)).

For example:

```
@titlefont{Texinfo}
```

Use the `@center` command at the beginning of a line to center
the remaining text on that line. Thus,

```
@center @titlefont{Texinfo}
```

centers the title, which in this example is “Texinfo” printed
in the title font.

Use the `@sp` command to insert vertical space. For example:

```
@sp 2
```

This inserts two blank lines on the printed page.
(See [`@sp` n: Insert Blank Lines](#g_t_0040sp), for more information about the `@sp`
command.)

A template for this method looks like this:

```
@titlepage
@sp 10
@center @titlefont{name-of-manual-when-printed}
@sp 2
@center subtitle-if-any
@sp 2
@center author
...
@end titlepage
```

The spacing of the example fits an 8.5 by 11 inch manual.

---

Next: [Heading Generation](#Heading-Generation), Previous: [`@titlefont`, `@center`, and `@sp`](#g_t_0040titlefont-_0040center-_0040sp), Up: [Title and Copyright Pages](#Titlepage-_0026-Copyright-Page)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 2.8.4 Copyright Page [¶](#Copyright-Page)

By international treaty, the copyright notice for a book must be either
on the title page or on the back of the title page. When the copyright
notice is on the back of the title page, that page is customarily not
numbered. Therefore, in Texinfo, the information on the copyright page
should be within `@titlepage` and `@end titlepage`
commands.

Use the `@page` command to cause a page break. To push the
copyright notice and the other text on the copyright page towards the
bottom of the page, use the following incantation after `@page`:

```
@vskip 0pt plus 1filll
```

The `@vskip` command inserts whitespace in the TeX output; it
is ignored in all other output formats. The ‘`0pt plus 1filll`’
means to put in zero points of mandatory whitespace, and as much
optional whitespace as needed to push the following text to the bottom
of the page. Note the use of three ‘`l`’s in the word
‘`filll`’; this is correct.

To insert the copyright text itself, write `@insertcopying`
next (see [Document Permissions](#Document-Permissions)):

```
@insertcopying
```

Follow the copying text by the publisher, ISBN numbers, cover art
credits, and other such information.

Here is an example putting all this together:

```
@titlepage
...
@page
@vskip 0pt plus 1filll
@insertcopying

Published by ...

Cover art by ...
@end titlepage
```

We have one more special case to consider: for plain text output, you
must insert the copyright information explicitly if you want it to
appear. For instance, you could have the following after the copyright
page:

```
@ifplaintext
@insertcopying
@end ifplaintext
```

You could include other title-like information for the plain text
output in the same place.

---

Previous: [Copyright Page](#Copyright), Up: [Title and Copyright Pages](#Titlepage-_0026-Copyright-Page)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 2.8.5 Heading Generation [¶](#Heading-Generation-1)

Texinfo has two standard page heading formats, one for documents
printed on one side of each sheet of paper (single-sided printing),
and the other for documents printed on both sides of each sheet
(double-sided printing).

In full generality, you can control the headings in different ways:

* The conventional way is to write a `@setchapternewpage` command
  before the title page commands.

  Most documents are formatted with the standard single-sided or
  double-sided headings, (sometimes) using `@setchapternewpage
  odd` for double-sided printing and (almost always) no
  `@setchapternewpage` command for single-sided printing
  (see [`@setchapternewpage`: Blank Pages Before Chapters](#g_t_0040setchapternewpage)).
* Alternatively, you can use the `@headings` command to prevent
  page headings from being generated or to start them for either single
  or double-sided printing. To turn off headings, write `@headings off`.
  See [The `@headings` Command](#g_t_0040headings).
* Or, you may specify your own page heading and footing format.
  See [Page Headings](#Headings).

---

Next: [The ‘Top’ Node and Master Menu](#The-Top-Node), Previous: [Title and Copyright Pages](#Titlepage-_0026-Copyright-Page), Up: [Writing a Texinfo File](#Writing-a-Texinfo-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 2.9 Generating a Table of Contents [¶](#Generating-a-Table-of-Contents)

The `@chapter`, `@section`, and other structuring commands
(see [Chapter Structuring](#Chapter-Structuring)) supply the information to make up a
table of contents, but they do not cause an actual table to appear in
the manual. To do this, you must use the `@contents` and/or
`@summarycontents` command(s).

`@contents`
:   Generates a table of contents in a printed manual, including all
    chapters, sections, subsections, etc., as well as appendices and
    unnumbered chapters. Headings generated by `@majorheading`,
    `@chapheading`, and the other `@…heading` commands
    do not appear in the table of contents (see [Structuring Command Types](#Structuring-Command-Types)).

`@shortcontents`

`@summarycontents`
:   (`@summarycontents` is a synonym for `@shortcontents`.)

    Generates a short or summary table of contents that lists only the
    chapters, appendices, and unnumbered chapters. Sections, subsections
    and subsubsections are omitted. Only a long manual needs a short
    table of contents in addition to the full table of contents.

Both contents commands should be written on a line by themselves, and
placed near the beginning of the file, after the `@end
titlepage` (see [`@titlepage`](#g_t_0040titlepage)), before any sectioning
command. The contents commands automatically generate a chapter-like
heading at the top of the first table of contents page, so don’t
include any sectioning command such as `@unnumbered` before
them.

Since an Info file uses menus instead of tables of contents, the Info
formatting commands ignore the contents commands. But the contents
are included in plain text output and in other output formats, such
as HTML.

In HTML output, the links in the short table of contents point to
corresponding entries in the full table of contents rather than the text
of the document. The links in the full table of contents point to the
main text of the document.

`@shortcontents` is not implemented for LaTeX output.

---

Next: [The Body of the Document](#The-Body-of-the-Document), Previous: [Generating a Table of Contents](#Contents), Up: [Writing a Texinfo File](#Writing-a-Texinfo-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 2.10 The ‘Top’ Node and Master Menu [¶](#The-_0060Top_0027-Node-and-Master-Menu)

The ‘Top’ node is the node in which a reader enters an Info manual.
As such, it should contain a very brief description of the manual
(including the version number). The contents of the ‘Top’ node do
not appear in printed output nor in DocBook output.

It is conventional to write a `@top` sectioning command
line containing the title of the document immediately after the
`@node Top` line (see [The `@top` Sectioning Command](#g_t_0040top-Command)).

We repeat the short description from the beginning of
the ‘`@copying`’ text, but there’s no need to repeat the copyright
information, so we don’t use ‘`@insertcopying`’ here.

The ‘Top’ node contains a top-level *menu* listing the chapters,
and possibly a *detailed menu* listing all the nodes in the
entire document.

```
@node Top
@top Short Sample

This is a short sample Texinfo file.
```

```
@menu
* First Chapter::    The first chapter is the
                       only chapter in this sample.
* Index::            Complete index.
@end menu
```

* [Parts of a Master Menu](#Master-Menu-Parts)

---

Up: [The ‘Top’ Node and Master Menu](#The-Top-Node)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 2.10.1 Parts of a Master Menu [¶](#Parts-of-a-Master-Menu)

A *master menu* is the main menu. It is customary to include a
detailed menu listing all the nodes in the document in this menu.
Like any other menu, a master menu is enclosed in `@menu` and
`@end menu` and does not appear in the printed output nor in
DocBook output.

The master menu contains entries for the major nodes in the Texinfo
file: the nodes for the chapters, chapter-like sections, and the
appendices, followed by nodes for the indices.

You may choose to follow these entries with a *detailed menu*.
This lists other, lower-level nodes, often ordered by chapter.
These items may be a convenience for an inquirer who can go directly
to a particular node when searching for specific information, rather
than going through an intermediate menu. If you use a detailed menu
in your master menu, mark it with the `@detailmenu …
@end detailmenu` environment.

Each section in the menu can be introduced by a descriptive line. So
long as the line does not begin with an asterisk, it will not be
treated as a menu entry. (See [Writing a Menu](#Writing-a-Menu), for more
information.)

For example, the master menu for this manual looks like the following
(but has many more entries):

```
@menu
* Copying Conditions::  Your rights.
* Overview::            Texinfo in brief.
...
```

```
* Command and Variable Index::
* General Index::
```

```

```

```
@detailmenu
--- The Detailed Node Listing ---

Overview of Texinfo

* Reporting Bugs:: ...
...
```

```

```

```
Beginning a Texinfo File

* Sample Beginning:: ...
...
@end detailmenu
@end menu
```

---

Next: [Ending a Texinfo File](#Ending-a-File), Previous: [The ‘Top’ Node and Master Menu](#The-Top-Node), Up: [Writing a Texinfo File](#Writing-a-Texinfo-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 2.11 The Body of the Document [¶](#The-Body-of-the-Document-1)

The body segment contains all the text of the document.
A manual is divided into one or more *nodes* (see [Nodes](#Nodes)).
The example illustrates a chapter made of three nodes, one
for introductory material in the chapter, and two sections.
The introductory material contains an enumerated list.

```
@node First Chapter
@chapter First Chapter

@cindex chapter, first
This is the first chapter.
@cindex index entry, another

Here is a numbered list.

@enumerate
@item
This is the first item.

@item
This is the second item.
@end enumerate

@node First Section
@section First Section

First section of first chapter.

@node Second Section
@section Second Section

Second section of first chapter.
```

In the Info output, the ‘`First Chapter`’ node will contain a
menu listing the two sections in the chapter. Similarly, when this
node is output in its own HTML file, it will contain a table of contents
for the chapter.

Here is what the contents of this chapter will look like:## 1. First Chapter [¶](#g_t1_002e-First-Chapter)

This is the first chapter.

Here is a numbered list.

1. This is the first item.
2. This is the second item.

### 1.1 First Section [¶](#g_t1_002e1-First-Section)

First section of first chapter.

### 1.2 Second Section [¶](#g_t1_002e2-Second-Section)

Second section of first chapter.

(In the Info and HTML output, the chapter would also be split
into nodes.)

---

Previous: [The Body of the Document](#The-Body-of-the-Document), Up: [Writing a Texinfo File](#Writing-a-Texinfo-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 2.12 Ending a Texinfo File [¶](#Ending-a-Texinfo-File)

The end of a Texinfo file should include commands to create indices
(see [Printing Indices and Menus](#Printing-Indices-_0026-Menus)), and the `@bye` command to mark
the last line to be processed. For example:

```
@node Index
@unnumbered Index

@printindex cp

@bye
```

A `@bye` command terminates Texinfo processing. It should be on
a line by itself. Anything following `@bye` is completely
ignored.

---

Next: [Chapter Structuring](#Chapter-Structuring), Previous: [Writing a Texinfo File](#Writing-a-Texinfo-File), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 3 Nodes [¶](#Nodes-1)

A *node* is a region of text that begins at a `@node`
command, and continues until the next `@node` command.
To specify a node, write a `@node` command at the beginning of
a line, and follow it with the name of the node.
Info readers display one node at a time, and provide commands for the
user to move to related nodes. The HTML output can be similarly navigated.

Nodes are used as the targets of cross-references. Cross-references,
such as the one at the end of this sentence, are made with `@xref`
and related commands; see [Cross-references](#Cross-References). Cross-references can
be sprinkled throughout the text. Other @-commands may also
be the target of cross-references (see [`@anchor`: Defining Arbitrary Cross-reference Targets](#g_t_0040anchor), see [Floats](#Floats)).

Normally, you put a node command immediately before each chapter
structuring command—for example, an `@section` or
`@subsection` line. (See [Chapter Structuring](#Chapter-Structuring).)
You should do this even if you do not intend to format the file for Info.
This is because printed output uses both `@node` names and
chapter-structuring names in the output for cross-references. The only
time you are likely to use the chapter structuring commands without also
using nodes is if you are writing a document that contains no cross
references and will only be printed, not transformed into Info, HTML, or
other formats.

* [Writing a `@node` Line](#Writing-a-Node)
* [Choosing Node Names](#Node-Names)
* [`@node` Line Requirements](#Node-Line-Requirements)
* [The First Node](#First-Node)
* [The `@top` Sectioning Command](#g_t_0040top-Command)
* [Texinfo Document Structure](#Texinfo-Document-Structure)
* [Node and Menu Illustration](#Node-Menu-Illustration)
* [Node Descriptions](#Node-Descriptions)
* [Menus](#Menus)

---

Next: [Choosing Node Names](#Node-Names), Up: [Nodes](#Nodes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 3.1 Writing a `@node` Line [¶](#Writing-a-_0040node-Line)

Write `@node` at the beginning of a line followed by the name of
the node, like this:

```
@node node-name
```

After you have inserted a `@node` line, you should immediately
write the @-command for the associated chapter or section (if any)
and insert its name.

You may optionally follow the node name argument to `@node`
with up to three optional arguments on the rest of the same line,
separating the arguments with commas. These are the names of the
‘Next’, ‘Previous’, and ‘Up’ pointers, in that order. Hence,
the template for a fully-written-out node line with ‘Next’, ‘Previous’,
and ‘Up’ pointers looks like this:

```
@node node-name, next, previous, up
```

The node-name argument must be present, but the others are
optional. If you wish to specify some but not others, just insert
commas as needed, as in: ‘`@node mynode,,,uppernode`’.
Any spaces before or after each name on the `@node` line are
ignored. However, if your Texinfo document is hierarchically organized,
as virtually all are, we recommend leaving off all the pointers and
letting `texi2any` determine them.

The `texi2any` program automatically determines node pointers
for a hierarchically organized document. For it to do this, each
`@node` command should be followed immediately by a sectioning
command such as `@chapter` or `@section` (except that
comment lines may intervene). Finally, you must follow the ‘Top’
`@node` line with a line beginning with `@top` to mark
the top-level node in the file. See [The `@top` Sectioning Command](#g_t_0040top-Command).

Even when you explicitly specify all pointers, you cannot write the
nodes in the Texinfo source file in an arbitrary order.
You must write the nodes in the order you wish them to appear in the
output. For Info format one can imagine that the order may not matter,
but it matters for the other formats.

In most cases, you will want to take advantage of the pointer creation
feature, and not redundantly specify node pointers that the programs can
determine.
However, Texinfo documents are not required to be organized
hierarchically or in fact to contain sectioning commands at all (for
example, if you never intend the document to be printed), so node
pointers may still be specified explicitly, in full generality.

If you are using GNU Emacs, and want explicit pointers, you can use the
update node commands provided by Texinfo mode to insert the names of
the pointers. (See [Updating Nodes and Menus](#Updating-Nodes-and-Menus).)

Alternatively, you can insert the ‘Next’, ‘Previous’, and ‘Up’
pointers yourself. If you do this in Emacs, you may find it helpful
to use the Texinfo mode keyboard command `C-c C-c n`. This command
inserts ‘`@node`’ and a comment line listing the names of the
pointers in their proper order. The comment line helps you keep track
of which arguments are for which pointers.

---

Next: [`@node` Line Requirements](#Node-Line-Requirements), Previous: [Writing a `@node` Line](#Writing-a-Node), Up: [Nodes](#Nodes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 3.2 Choosing Node Names [¶](#Choosing-Node-Names)

The name of a node identifies the node. For all the details of node
names, see [`@node` Line Requirements](#Node-Line-Requirements)).

Here are some suggestions for node names:

* Try to pick node names that are informative but short.

  In the Info file, the file name, node name, and pointer names are all
  inserted on one line, which may run into the right edge of the window.
  (This does not cause a problem with Info, but is ugly.)
* Try to pick node names that differ from each other near the beginnings
  of their names. This way, it is easy to use automatic name completion in
  Info.
* Conventionally, node names are capitalized in the same way as section
  and chapter titles. In this manual, initial and significant words are
  capitalized; others are not. In other manuals, just initial words and
  proper nouns are capitalized. Either way is fine; we recommend just
  being consistent.
* In HTML output, any characters in the node name other than plain ASCII
  letters, numbers or spaces will be changed in the file name.
  (See [HTML Cross-reference Node Name Expansion](#HTML-Xref-Node-Name-Expansion).)
  This can make the URLs for the pages in your manual less user-friendly;
  for example, in this manual the ‘`@dots`’ node is output as
  `__0040dots.html`.

Because node names are used in cross-references, it is not desirable to
casually change them once published. When you delete or rename a node, it is
usually a good idea to define an `@anchor` with the old name.
That way, references from other manuals, from mail archives, and so on
are not invalidated. See [`@anchor`: Defining Arbitrary Cross-reference Targets](#g_t_0040anchor).

The pointers from a given node enable you to reach other nodes and
consist simply of the names of those nodes.

Normally, a node’s ‘Up’ pointer contains the name of the node whose
menu mentions that node. The node’s ‘Next’ pointer contains the name
of the node that follows the present node in that menu and its
‘Previous’ pointer contains the name of the node that precedes it in
that menu. When a node’s ‘Previous’ node is the same as its ‘Up’
node, both pointers name the same node.

Usually, the first node of a Texinfo file is the ‘Top’ node, and its
‘Up’ pointer points to the `dir` file, which contains the main menu
for all of Info.

---

Next: [The First Node](#First-Node), Previous: [Choosing Node Names](#Node-Names), Up: [Nodes](#Nodes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 3.3 `@node` Line Requirements [¶](#g_t_0040node-Line-Requirements)

Names used with `@node` have several requirements:

* All the node names in a single Texinfo file must be unique.

  This means, for example, that if you end every chapter with a summary,
  you must name each summary node differently. You cannot just call
  them all “Summary”. You may, however, duplicate the titles of
  chapters, sections, and the like. Thus you can end each chapter with
  a section called “Summary”, so long as the node names for those
  sections are all different.

  Node names, anchor names (see [`@anchor`: Defining Arbitrary Cross-reference Targets](#g_t_0040anchor)),
  and float labels (see [`@float` [type][,label]: Floating Material](#g_t_0040float)) must all be unique.
* Node names can contain @-commands[3](#FOOT3). For example, using
  `@TeX{}` in a node name results in the TeX logo being
  output, as it would be in normal text. Cross-references should
  use `@TeX{}` just as the node name does.

  Some commands do not make sense in node names; for instance,
  environments (e.g., `@quotation`), commands that read a whole
  line as their argument (e.g., `@sp`), and plenty of others.
  For the complete list of commands that are allowed, and their
  expansion for HTML identifiers and file names, see [HTML Cross-reference Command Expansion](#HTML-Xref-Command-Expansion).
* A node name may not start with a left parenthesis preceding a right
  parenthesis, as in `(not)allowed`, since this syntax is used to
  specify an external manual.
* Unfortunately, you cannot reliably use periods, commas, or colons
  within a node name; these can confuse some Info readers.
  `texi2any` quotes problematic node names and labels by default,
  but some Info readers do not recognize this syntax. Node name and label
  quoting causes `DEL` characters (‘`CTRL-?`’, character number 127,
  often rendered as ‘`^?`’) to appear around the name. To remove
  node names and labels quoting, you can set the customization variable
  `INFO_SPECIAL_CHARS_QUOTE` to ‘`0`’ (see [Other Customization Variables](#Other-Customization-Variables)).

  `texi2any` warns about such problematic usage in node names,
  menu items, and cross-references. If you don’t want to see the
  warnings, you can set the customization variable
  `INFO_SPECIAL_CHARS_WARNING` to ‘`0`’ (see [Other Customization Variables](#Other-Customization-Variables)).

  If you insist on using these characters in node names, in order not
  to confuse the Texinfo processors you must still escape those characters,
  by using either special insertions (see [Inserting ‘,’ with `@comma{}`](#Inserting-a-Comma)) or `@asis`
  (see [`@asis`](#g_t_0040asis)). For example:

  ```
  @node foo@asis{::}bar@comma{} baz
  ```

  As an example of avoiding the special characters, the following is a
  section title in this manual:

  ```
  @section @code{@@unnumbered}, @code{@@appendix}: Chapters with...
  ```

  But the corresponding node name lacks the commas and the subtitle:

  ```
  @node @code{@@unnumbered @@appendix}
  ```
* Case is significant in node names.
* Spaces before and after names on the ‘`@node`’ line are ignored.
  Multiple whitespace characters “inside” a name are collapsed to a
  single space. For example:

  ```
  @node foo bar
  @node  foo bar,
  @node foo bar ,
  @node foo  bar,
  @node  foo  bar ,
  ```

  all define the same node, namely ‘`foo bar`’.
  In menu entries, a single internal space should be used in node
  names or some versions of some Info readers will not find the node.

---

Next: [The `@top` Sectioning Command](#g_t_0040top-Command), Previous: [`@node` Line Requirements](#Node-Line-Requirements), Up: [Nodes](#Nodes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 3.4 The First Node [¶](#The-First-Node)

The first node of a Texinfo file is the *Top* node, except in an
included file (see [Include Files](#Include-Files)). The Top node should contain
a short summary and a master menu. See [The ‘Top’ Node and Master Menu](#The-Top-Node) for more
information on the Top node contents and examples. Straight text
before the Top node outside of any node should be avoided. Such
text, if present, is not output for DocBook.

Here is a description of the node pointers to be used in the Top node:

* The Top node (which must be named ‘`top`’ or ‘`Top`’) should have
  as its ‘Up’ node the name of a node in another file, where there is a
  menu that leads to this file. Specify the file name in parentheses.

  Usually, all Info files are available through a single virtual Info
  tree, constructed from multiple directories. In this case, use
  ‘`(dir)`’ as the parent of the Top node; this specifies the
  top-level node in the `dir` file, which contains the main menu
  for the Info system as a whole. (Each directory with Info files is
  intended to contain a file named `dir`.)

  That’s fine for Info, but for HTML output, one might well want the Up
  link from the Top node to go to some specific place.
  For example, for GNU the natural place would be
  <http://www.gnu.org/manual/> (a web page collecting links to most
  GNU manuals), better specified as just `/manual/` if the manual
  will be installed on `www.gnu.org`. This can be specified with
  the `TOP_NODE_UP_URL` customization variable (see [HTML Customization Variables](#HTML-Customization-Variables)), as in

  ```
  $ texi2any --html -c TOP_NODE_UP_URL=/manual/ ...
  ```
* The ‘Prev’ node of the Top node is usually omitted.
* The ‘Next’ node of the Top node should be the first chapter in your
  document.

See [Installing an Info File](#Installing-an-Info-File), for more information about installing
an Info file in the `info` directory.

It is usually best to leave the pointers off entirely and let the
tools implicitly define them, with this simple result:

```
@node Top
```

---

Next: [Texinfo Document Structure](#Texinfo-Document-Structure), Previous: [The First Node](#First-Node), Up: [Nodes](#Nodes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 3.5 The `@top` Sectioning Command [¶](#The-_0040top-Sectioning-Command)

The `@top` command is a special sectioning command that you
should only use after a ‘`@node Top`’ line at the beginning of a
Texinfo file.

It produces the same sort of output as `@unnumbered`
(see [`@unnumbered`, `@appendix`: Chapters with Other Labeling](#g_t_0040unnumbered-_0040appendix)). In LaTeX `\part*` is used.

`@top` is ignored when raising or lowering sections. That is,
it is never lowered and nothing can be raised to it
(see [Raise/lower Sections: `@raisesections` and `@lowersections`](#Raise_002flower-sections)).

It used to be conventional to wrap the ‘`Top`’ node
in an `@ifnottex` conditional so that it would not appear in
printed output (see [Conditionally Visible Text](#Conditionals)). Thus, a Top node often looked
like this:

```
@ifnottex
@node Top
@top your-manual-title

very-high-level-summary
@end ifnottex
```

This is no longer necessary, as the ‘`Top`’ node is now never output
for printed output. The ‘`Top`’ node is not output for DocBook either.

---

Next: [Node and Menu Illustration](#Node-Menu-Illustration), Previous: [The `@top` Sectioning Command](#g_t_0040top-Command), Up: [Nodes](#Nodes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 3.6 Texinfo Document Structure [¶](#Texinfo-Document-Structure-1)

Nodes can contain *menus*, which contain the names of *child
nodes* within the parent node; for example, a node corresponding to a
chapter would have a menu of the sections in that chapter. The menus
allow the user to move to the child nodes in the Info output.

In addition, nodes contain *node pointers* that name other nodes.
The ‘Next’ and ‘Previous’ pointers link nodes at the same sectioning
level into a chain. As you might imagine, the ‘Next’ pointer links to
the next node, and the ‘Previous’ pointer links to the previous node.
In general, ‘Next’ and ‘Previous’ refer to nodes at the *same
hierarchical level* in the manual, not necessarily to the next node
within the Texinfo file. In the Texinfo file, the subsequent node may
be at a lower level—a section-level node most often follows a chapter-level
node, for example.
Thus, for example, all the nodes that are at the level of sections
within a chapter are linked together, and the order in this chain
is the same as the order of the children in the menu of the parent
chapter. Each child node records the parent node name as its ‘Up’
pointer.

Since the ‘Top’ node is the only node at that level, ‘Next’
refers to the first following node, which is almost always a chapter
or chapter-level node. This is an exception to the rule of
‘Next’ being at the same hierarchical level.

The Info and HTML output for each node includes links to the
‘Next’, ‘Previous’, and ‘Up’ nodes. The HTML also uses
the `accesskey` attribute with the values ‘`n`’, ‘`p`’, and
‘`u`’ respectively. This allows people using web browsers to
follow the navigation using (typically) `M-letter`, e.g.,
`M-n` for the ‘Next’ node, from anywhere within the node.
Node pointers and menus provide structure for Info files just as
chapters, sections, subsections, and the like provide structure for
printed books. The two structures are theoretically distinct; in
practice, however, the tree structure of printed books is essentially
always used for the node and menu structure also, as this leads to a
document which is easy to follow.

Typically, the sectioning structure and the node structure are
completely parallel, with one node for each chapter, section, etc.,
and with the nodes following the same hierarchical arrangement as the
sectioning. Thus, if a node is at the logical level of a chapter, its
child nodes are at the level of sections; similarly, the child nodes
of sections are at the level of subsections.

It is technically possible to create Texinfo documents with
only one structure or the other, or for the two structures not to be
parallel, or for either the sectioning or node structure to be
different from the conventional structure. To the best of our
knowledge, however, all the Texinfo manuals currently in general use do
follow the conventional parallel structure.

---

Next: [Node Descriptions](#Node-Descriptions), Previous: [Texinfo Document Structure](#Texinfo-Document-Structure), Up: [Nodes](#Nodes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 3.7 Node and Menu Illustration [¶](#Node-and-Menu-Illustration)

Here is a diagram that illustrates a Texinfo file with three chapters,
each of which contains two sections.

The “root” is at the top of the diagram and the “leaves” are at
the bottom. This is how such a diagram is drawn conventionally; it
illustrates an upside-down tree. For this reason, the root node is
called the ‘Top’ node, and ‘Up’ node pointers carry you closer to the
root.

```
                         Top
                          |
        -------------------------------------
       |                  |                  |
    Chapter 1          Chapter 2          Chapter 3
       |                  |                  |
    --------           --------           --------
   |        |         |        |         |        |
Section  Section   Section  Section   Section  Section
  1.1      1.2       2.1      2.2       3.1      3.2
```

Using explicit pointers (not recommended, but shown for purposes
of the example), the fully-written command to start Chapter 2
would be this:

```
@node     Chapter 2,  Chapter 3, Chapter 1, Top
@comment  node-name,  next,      previous,  up
```

This `@node` line says that the name of this node is
“Chapter 2”, the name of the ‘Next’ node is “Chapter 3”, the
name of the ‘Previous’ node is “Chapter 1”, and the name of the
‘Up’ node is “Top”. You can (and should) omit writing out these
node names if your document is hierarchically organized, but the
pointer relationships still obtain.

To go to Sections 2.1 and 2.2 using Info, you need a menu inside
Chapter 2. (See [Menus](#Menus).) You would write the menu just before the
beginning of Section 2.1, like this:

```
   @menu
   * Sect. 2.1::    Description of this section.
   * Sect. 2.2::    Description.
   @end menu
```

The automatic pointers for the node for Sect. 2.1 correspond to:

```
@node     Sect. 2.1, Sect. 2.2, ,         Chapter 2
@comment  node-name, next,      previous, up
```

Note that no ‘Prev’ pointer is generated, since there is
no other node at the same hierarchical level before Sect. 2.1.

Using explicit pointers, the node for Sect. 2.1 could be written like this:

```
@node     Sect. 2.1, Sect. 2.2, Chapter 2, Chapter 2
@comment  node-name, next,      previous,  up
```

With automatic pointers, the ‘Next’ and ‘Previous’ pointers of a node
lead to other nodes at the same level—from chapter to chapter or
from section to section. As shown, when using explicit pointers,
the pointers can also lead somewhere else, here, for
example, the ‘Previous’ pointer points up. An ‘Up’ pointer usually
leads to a node at the level above (closer to the ‘Top’ node; and a
‘Menu’ leads to nodes at a level below (closer to ‘leaves’).
(A cross-reference can point to a node at any level; see
[Cross-references](#Cross-References).)

Technically, explicit node pointers can carry you to any node,
regardless of the structure of the document; even to nodes in a
different Info file. However, it would be very confusing for readers
to have the ‘Next’, ‘Previous’ and ‘Up’ pointers lead to nodes that
do not correspond, even loosely, to the next, previous and up node.

A `@node` command and a chapter structuring command are
conventionally used together, in that order, often followed by
indexing commands. (As shown in the example above, you may follow the
`@node` line with a comment line, e.g., to show which pointer is
which if explicit pointers are used.) The Texinfo processors use this
construct to determine the relationships between nodes and sectioning
commands.

Here is the beginning of the chapter in this manual called “Ending a
Texinfo File”. This shows a `@node` line followed by a
`@chapter` line, and then by indexing lines.

```
@node Ending a File
@chapter Ending a Texinfo File
@cindex Ending a Texinfo file
@cindex Texinfo file ending
@cindex File ending
```

---

Next: [Menus](#Menus), Previous: [Node and Menu Illustration](#Node-Menu-Illustration), Up: [Nodes](#Nodes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 3.8 Node Descriptions [¶](#Node-Descriptions-1)

You can provide a short description of the purpose of a node by using
the `@nodedescription` command following the `@node` line.
Such a description might elaborate on or extend the information in the
node name itself.

You can also use a `@nodedescriptionblock` environment to
provide a node description. This may be useful for longer descriptions.

`texi2any` uses the content you provide with these commands
when outputing menus for Info output format (and, optionally,
for HTML). `texi2any` uses the description after a menu
entry for the node if it is generating the menu automatically, or
if no description for the menu entry was provided in an explicit
`@menu` block. (See [Menus](#Menus)).

Here is an example of using these commands:

```
@node Tools
@chapter Tools

This chapter is on different tools you can use.

@node Screwdrivers
@nodedescription Flathead and Phillips.
@section Screwdrivers

This section is about screwdrivers.

@node Drills
@nodedescriptionblock
Making holes in things with power screwdrivers, drill drivers, combi
drills, impact drivers, hammer drills, breakers and demolition drills.
@end nodedescriptionblock
@section Drills

This section is about drills.
```

In Info output, `texi2any` would output the ‘`Tools`’ node with
a menu as follows:

```
* Menu:

* Screwdrivers::          Flathead and Phillips.
* Drills::                Making holes in things with power
                          screwdrivers, drill drivers, combi
                          drills, impact drivers, hammer drills,
                          breakers and demolition drills.
```

---

Previous: [Node Descriptions](#Node-Descriptions), Up: [Nodes](#Nodes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 3.9 Menus [¶](#Menus-1)

*Menus* contain pointers to subordinate nodes. In Info output,
you use menus to go to such nodes. `texi2any` can output menus in
HTML output, but does not do so by default
(see [Other Customization Variables](#Other-Customization-Variables), under `FORMAT_MENU`).
Menus have no role in printed manuals or other output formats.

Menus are automatically generated by `texi2any` when outputting
Info for nodes followed by a sectioning command, without an explicit
`@menu` block, and with automatic pointers.

It is often more convenient to let `texi2any` generate
menus for you, as you do not then have the burden of updating menu
blocks in your Texinfo source when you add, remove, or relocate nodes.
In the usual case of a hierarchically organized manual with sectioning
commands associated with nodes, and with node pointers left out, you
should only write menus if you want exact control over the contents
and formatting of menus in Info.

* [Writing a Menu](#Writing-a-Menu)
* [A Menu Example](#Menu-Example)
* [Menu Location](#Menu-Location)
* [The Parts of a Menu](#Menu-Parts)
* [Less Cluttered Menu Entry](#Less-Cluttered-Menu-Entry)
* [Referring to Other Info Files](#Other-Info-Files)

---

Next: [A Menu Example](#Menu-Example), Up: [Menus](#Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 3.9.1 Writing a Menu [¶](#Writing-a-Menu-1)

A menu consists of a `@menu` command on a line by itself,
followed by menu entry lines or menu comment lines, and then followed
by an `@end menu` command on a line by itself.

A menu looks like this:

```
@menu
Larger Units of Text

* Files::                       All about handling files.
* Multiples: Buffers.           Multiple buffers; editing
                                 several files at once.
@end menu
```

In a menu, every line that begins with an ‘`*`’ is a *menu
entry*. (Note the space after the asterisk.)

A line that does not start with an ‘`*`’ may also appear in a
menu. Such a line is not a menu entry but rather a *menu comment*
line that appears in the Info file. In the example above, the line
‘`Larger Units of Text`’ is such a menu comment line; the two lines
starting with ‘`*`’ are menu entries.

Technically, menus can carry you to any node, regardless of the
structure of the document; even to nodes in a different Info file.
However, the `texi2any` implicit pointer creation feature
(see [Writing a `@node` Line](#Writing-a-Node)) and GNU Emacs Texinfo mode updating commands
work only to create menus of subordinate nodes in a hierarchically
structured document. In a hierarchically structured document, it is
much better to use cross-references to refer to arbitrary nodes.

In Info, a user selects a node with the `m` (`Info-menu`)
command. The menu entry name is what the user types after the `m`
command.
In the HTML output, the `accesskey` attribute is used with the
values ‘`1`’…‘`9`’ for the first nine entries. This
allows people using web browsers to follow the first menu entries
using (typically) `M-digit`, e.g., `M-1` for the first
entry.

---

Next: [Menu Location](#Menu-Location), Previous: [Writing a Menu](#Writing-a-Menu), Up: [Menus](#Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 3.9.2 A Menu Example [¶](#A-Menu-Example)

A menu looks like this in Texinfo:

```
@menu
* menu entry name: Node name.   A short description.
* Node name::                   This form is preferred.
@end menu
```

This produces:

```
* Menu:

* menu entry name: Node name.   A short description.
* Node name::                   This form is preferred.
```

Here is an example as you might see it in a Texinfo file:

```
@menu
Larger Units of Text

* Files::                       All about handling files.
* Multiples: Buffers.           Multiple buffers; editing
                                 several files at once.
@end menu
```

This produces:

```
* Menu:
Larger Units of Text

* Files::                       All about handling files.
* Multiples: Buffers.           Multiple buffers; editing
                                 several files at once.
```

In this example, the menu has two entries. ‘`Files`’ is both a menu
entry name and the name of the node referred to by that name.
‘`Multiples`’ is the menu entry name; it refers to the node named
‘`Buffers`’. The line ‘`Larger Units of Text`’ is a comment; it
appears in the menu, but is not an entry.

Since no file name is specified with either ‘`Files`’ or
‘`Buffers`’, they must be the names of nodes in the same Info file
(see [Referring to Other Info Files](#Other-Info-Files)).

---

Next: [The Parts of a Menu](#Menu-Parts), Previous: [A Menu Example](#Menu-Example), Up: [Menus](#Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 3.9.3 Menu Location [¶](#Menu-Location-1)

There may be at most one menu in a node. A menu is conventionally
located at the end of a node, without any regular text or additional
commands between the `@end menu` and the beginning of the next
node.

This convention is useful, since a reader who uses the menu could
easily miss any such text. Also, any such post-menu text will be
considered part of the menu in Info output (which has no marker for
the end of a menu). Thus, a line beginning with ‘`*` ’ will likely
be incorrectly handled.

It’s usually best if a node with a menu does not contain much text.
If you find yourself with a lot of text before a menu, we generally
recommend moving all but a couple of paragraphs into a new subnode.
Otherwise, it is easy for readers to miss the menu.

---

Next: [Less Cluttered Menu Entry](#Less-Cluttered-Menu-Entry), Previous: [Menu Location](#Menu-Location), Up: [Menus](#Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 3.9.4 The Parts of a Menu [¶](#The-Parts-of-a-Menu)

A menu entry has three parts, only the second of which is required:

1. The menu entry name (optional).
2. The name of the node (required).
3. A description of the item (optional).

The template for a generic menu entry looks like this (but see the
next section for one more possibility):

```
* menu-entry-name: node-name.   description
```

Follow the menu entry name with a single colon, and follow the node
name with tab, comma, newline, or the two characters period and space
(‘`.` ’).

The third part of a menu entry is a descriptive phrase or sentence.
Menu entry names and node names are often short; the description
explains to the reader what the node is about. A useful description
complements the node name rather than repeats it. The description,
which is optional, can spread over multiple lines; if it does, some
authors prefer to indent the second line while others prefer to align
it with the first (and all others). It’s up to you. An empty line,
or the next menu entry, ends a description.

Space characters in a menu are preserved as-is in the Info output; this
allows you to format the menu as you wish.
Unfortunately you must type
node names without any extra spaces or some versions of some Info
readers will not find the node (see [`@node` Line Requirements](#Node-Line-Requirements)).

`texi2any` warns and protect names when the text of a menu item
(and node names and cross-references) contains a problematic construct that
could interfere with its parsing in Info. See [Info Node Names Constraints](#Info-Node-Names-Constraints).

---

Next: [Referring to Other Info Files](#Other-Info-Files), Previous: [The Parts of a Menu](#Menu-Parts), Up: [Menus](#Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 3.9.5 Less Cluttered Menu Entry [¶](#Less-Cluttered-Menu-Entry-1)

When the menu entry name and node name are the same, you can write
the name immediately after the asterisk and space at the beginning of
the line and follow the name with two colons.

For example, write

```
* Name::                        description
```

instead of

```
* Name: Name.                   description
```

We recommend using the node name for the menu entry name whenever
possible, since it reduces visual clutter in the menu.

---

Previous: [Less Cluttered Menu Entry](#Less-Cluttered-Menu-Entry), Up: [Menus](#Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 3.9.6 Referring to Other Info Files [¶](#Referring-to-Other-Info-Files)

You can create a menu entry that enables a reader in Info to go to a
node in another Info file by writing the file name in parentheses just
before the node name. Some examples:

```
@menu
* first-entry-name:(filename)nodename.     description
* (filename)second-node::                  description
@end menu
```

For example, to refer directly to the ‘`Outlining`’ and
‘`Rebinding`’ nodes in the Emacs Manual, you could write a
menu like this:

```
@menu
* Outlining: (emacs)Outline Mode. The major mode for
                                   editing outlines.
* (emacs)Rebinding::              How to redefine the
                                   meaning of a key.
@end menu
```

If you do not list the node name, but only name the file, then Info
presumes that you are referring to the ‘Top’ node. Examples:

```
* Info: (info).         Documentation browsing system.
* (emacs)::             The extensible, self-documenting
                         text editor.
```

The GNU Emacs Texinfo mode menu updating commands only work with nodes
within the current buffer, so you cannot use them to create menus that
refer to other files. You must write such menus by hand.
See [Updating Nodes and Menus](#Updating-Nodes-and-Menus).

---

Next: [Cross-references](#Cross-References), Previous: [Nodes](#Nodes), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 4 Chapter Structuring [¶](#Chapter-Structuring-1)

Texinfo’s *chapter structuring* commands divide a document into a
hierarchy of chapters, sections, subsections, and subsubsections. These
commands generate large headings in the text, like the one above. They
also provide information for generating the table of contents
(see [Generating a Table of Contents](#Contents)).

Normally you put a `@node` command immediately before each
chapter structuring command. See [Nodes](#Nodes).

* [Tree Structure of Sections](#Tree-Structuring)
* [Structuring Command Types](#Structuring-Command-Types)
* [`@chapter`: Chapter Structuring](#g_t_0040chapter)
* [`@unnumbered`, `@appendix`: Chapters with Other Labeling](#g_t_0040unnumbered-_0040appendix)
* [`@majorheading`, `@chapheading`: Chapter-level Headings](#g_t_0040majorheading-_0040chapheading)
* [`@section`: Sections Below Chapters](#g_t_0040section)
* [`@unnumberedsec`, `@appendixsec`, `@heading`](#g_t_0040unnumberedsec-_0040appendixsec-_0040heading)
* [`@subsection`: Subsections Below Sections](#g_t_0040subsection)
* [The `@subsection`-like Commands](#g_t_0040unnumberedsubsec-_0040appendixsubsec-_0040subheading)
* [`@subsubsection` and Other Subsub Commands](#g_t_0040subsubsection)
* [`@part`: Groups of Chapters](#g_t_0040part)
* [Raise/lower Sections: `@raisesections` and `@lowersections`](#Raise_002flower-sections)

---

Next: [Structuring Command Types](#Structuring-Command-Types), Up: [Chapter Structuring](#Chapter-Structuring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 4.1 Tree Structure of Sections [¶](#Tree-Structure-of-Sections)

A Texinfo file is usually structured like a book with chapters,
sections, subsections, and the like. This structure can be visualized
as a tree (or rather as an upside-down tree) with the root at the top
and the levels corresponding to chapters, sections, subsection, and
subsubsections.

Here is a diagram that shows a Texinfo file with three chapters, each
with two sections.

```
                         Top
                          |
        -------------------------------------
       |                  |                  |
    Chapter 1          Chapter 2          Chapter 3
       |                  |                  |
    --------           --------           --------
   |        |         |        |         |        |
Section  Section   Section  Section   Section  Section
  1.1      1.2       2.1      2.2       3.1      3.2
```

In a Texinfo file that has this structure, the beginning of Chapter 2
would be written like this:

```
@node    Chapter 2
@chapter Chapter 2
```

For purposes of example, here is how it would be written with
explicit node pointers:

```
@node    Chapter 2,  Chapter 3, Chapter 1, Top
@chapter Chapter 2
```

The chapter structuring commands are described in the sections that
follow; the `@node` command is described in
the previous chapter (see [Nodes](#Nodes)).

---

Next: [`@chapter`: Chapter Structuring](#g_t_0040chapter), Previous: [Tree Structure of Sections](#Tree-Structuring), Up: [Chapter Structuring](#Chapter-Structuring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 4.2 Structuring Command Types [¶](#Structuring-Command-Types-1)

The chapter structuring commands fall into four groups, each of which
contains structuring commands at the levels of chapters, sections,
subsections, and subsubsections:

* The `@chapter`-like commands and `@appendix`-like commands
  produce numbered or lettered entries both in the body of a document and
  in its table of contents.
* The `@unnumbered`-like commands produce unnumbered entries
  both in the body of a document and in its table of contents. The
  `@top` command, which has a special use, is a member of this
  group (see [The `@top` Sectioning Command](#g_t_0040top-Command)). An `@unnumbered` section
  is a normal part of the document structure.
* The `@heading`-like commands produce simple unnumbered
  headings that do not appear in a table of contents, are not associated
  with nodes, and cannot be cross-referenced. These heading commands
  never start a new page.

In printed output, the chapter structuring commands produce headings
in the document. When a `@setchapternewpage` command says to do so, the
`@chapter`, `@unnumbered`, and `@appendix` commands
start new pages in the printed manual; the `@heading` commands
do not. See [`@setchapternewpage`: Blank Pages Before Chapters](#g_t_0040setchapternewpage).

In Info and plain text output, the command causes the
title to appear on a line by itself, with a line of an ASCII character
(‘`*`’, ‘`=`’, …) inserted underneath.
For example, the “Chapter Structuring” heading could be:

```
5 Chapter Structuring
*********************
```

The underlining character is the same for all the commands at the
same level. For instance, it is the same for the chapter-level commands
`@chapter`, `@apppendix`, `@unnumbered` and
`@chapheading`.

In HTML, the chapter-level commands produce an `<h2>`-level
header by default (controlled by the `CHAPTER_HEADER_LEVEL`
customization variable, see [Other Customization Variables](#Other-Customization-Variables)).
The heading element level is adjusted for the other commands.

In the DocBook output, the appropriate level of element is used.
The produced element includes all following sections up to the next command
at the same or higher level.
For example, a `<chapter>` element is produced for `@chapter`,
and contains any sections or subsections in the chapter.

Here is a summary:

|  |  |  |  |
| --- | --- | --- | --- |
|  |  |  | No new page |
| *Numbered* | *Unnumbered* | *Lettered/numbered* | *Unnumbered* |
| In contents | In contents | In contents | Not in contents |
|  | `@top` |  | `@majorheading` |
| `@chapter` | `@unnumbered` | `@appendix` | `@chapheading` |
| `@section` | `@unnumberedsec` | `@appendixsec` | `@heading` |
| `@subsection` | `@unnumberedsubsec` | `@appendixsubsec` | `@subheading` |
| `@subsubsection` | `@unnumberedsubsubsec` | `@appendixsubsubsec` | `@subsubheading` |

---

Next: [`@unnumbered`, `@appendix`: Chapters with Other Labeling](#g_t_0040unnumbered-_0040appendix), Previous: [Structuring Command Types](#Structuring-Command-Types), Up: [Chapter Structuring](#Chapter-Structuring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 4.3 `@chapter`: Chapter Structuring [¶](#g_t_0040chapter_003a-Chapter-Structuring)

`@chapter` identifies a chapter in the document–the highest
level of the normal document structuring hierarchy. Write the command
at the beginning of a line and follow it on the same line by the title
of the chapter. The chapter is numbered automatically, starting
from 1.

For example, the present chapter in this manual is entitled
“Chapter Structuring”; the `@chapter` line
looks like this:

```
@chapter Chapter Structuring
```

---

Next: [`@majorheading`, `@chapheading`: Chapter-level Headings](#g_t_0040majorheading-_0040chapheading), Previous: [`@chapter`: Chapter Structuring](#g_t_0040chapter), Up: [Chapter Structuring](#Chapter-Structuring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 4.4 `@unnumbered`, `@appendix`: Chapters with Other Labeling [¶](#g_t_0040unnumbered_002c-_0040appendix_003a-Chapters-with-Other-Labeling)

Use the `@unnumbered` command to start a chapter-level element
that appears without chapter numbers of any kind. Use the
`@appendix` command to start an appendix that is labeled by
letter (‘A’, ‘B’, …) instead of by number; appendices are also
at the chapter level of structuring.

Write an `@appendix` or `@unnumbered` command at the
beginning of a line and follow it on the same line by the title,
just as with `@chapter`.

Texinfo also provides a command `@centerchap`, which is analogous
to `@unnumbered`, but centers its argument in the printed and HTML
outputs. This kind of stylistic choice is not usually offered by
Texinfo.
You are recommended not to use this command, as
it may be removed in future releases of Texinfo.

With `@unnumbered`, if the name of the associated node is one of
these English words (case-insensitive):

```
Acknowledgements  Colophon  Dedication  Preface
```

then the DocBook output uses corresponding special tags
(`<preface>`, etc.) instead of the default `<chapter>`.
The argument to `@unnumbered` itself can be anything, and is
output as the element title as usual.

---

Next: [`@section`: Sections Below Chapters](#g_t_0040section), Previous: [`@unnumbered`, `@appendix`: Chapters with Other Labeling](#g_t_0040unnumbered-_0040appendix), Up: [Chapter Structuring](#Chapter-Structuring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 4.5 `@majorheading`, `@chapheading`: Chapter-level Headings [¶](#g_t_0040majorheading_002c-_0040chapheading_003a-Chapter_002dlevel-Headings)

The `@majorheading` and `@chapheading` commands produce
chapter-like headings in the body of a document.

However, neither command produces an entry in the table of contents,
and neither command causes TeX to start a new page in a printed
manual.

In TeX, a `@majorheading` command generates a larger vertical
whitespace before the heading than a `@chapheading` command but
is otherwise the same.

In other output formats, the `@majorheading` and `@chapheading`
commands produce a similar output to `@chapter`. The difference is
the lack of numbering and the lack of any association with nodes.
See [`@chapter`: Chapter Structuring](#g_t_0040chapter).

---

Next: [`@unnumberedsec`, `@appendixsec`, `@heading`](#g_t_0040unnumberedsec-_0040appendixsec-_0040heading), Previous: [`@majorheading`, `@chapheading`: Chapter-level Headings](#g_t_0040majorheading-_0040chapheading), Up: [Chapter Structuring](#Chapter-Structuring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 4.6 `@section`: Sections Below Chapters [¶](#g_t_0040section_003a-Sections-Below-Chapters)

An `@section` command identifies a section within a chapter
unit, whether created with `@chapter`, `@unnumbered`, or
`@appendix`, following the numbering scheme of the chapter-level
command. Thus, within a `@chapter` chapter numbered ‘1’, the
sections are numbered ‘1.1’, ‘1.2’, etc.; within an `@appendix`
“chapter” labeled ‘A’, the sections are numbered ‘A.1’, ‘A.2’, etc.;
within an `@unnumbered` chapter, the section gets no number.

To make a section, write the `@section` command at the
beginning of a line and follow it on the same line by the section
title. For example:

```
@section This is a section
```

Section titles are listed in the table of contents.

---

Next: [`@subsection`: Subsections Below Sections](#g_t_0040subsection), Previous: [`@section`: Sections Below Chapters](#g_t_0040section), Up: [Chapter Structuring](#Chapter-Structuring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 4.7 `@unnumberedsec`, `@appendixsec`, `@heading` [¶](#g_t_0040unnumberedsec_002c-_0040appendixsec_002c-_0040heading)

The `@unnumberedsec`, `@appendixsec`, and `@heading`
commands are, respectively, the unnumbered, appendix-like, and
heading-like equivalents of the `@section` command (see the
previous section).

`@unnumberedsec` and `@appendixsec` do not need to be used
in ordinary circumstances, because `@section` may also be used
within `@unnumbered` and `@appendix` chapters; again, see
the previous section.

`@unnumberedsec`
:   The `@unnumberedsec` command may be used within an unnumbered
    chapter or within a regular chapter or appendix to produce an
    unnumbered section.

`@appendixsec` [¶](#index-appendixsection)

`@appendixsection`
:   `@appendixsection` is a longer spelling of the
    `@appendixsec` command; the two are synonymous.

    Conventionally, the `@appendixsec` or `@appendixsection`
    command is used only within appendices.

`@heading`
:   You may use the `@heading` command (almost) anywhere for a
    section-style heading that will not appear in the table of contents.
    The `@heading`-series commands can appear inside most
    environments, for example, though pathological and useless locations
    such as an argument to another command, etc., are not allowed.

---

Next: [The `@subsection`-like Commands](#g_t_0040unnumberedsubsec-_0040appendixsubsec-_0040subheading), Previous: [`@unnumberedsec`, `@appendixsec`, `@heading`](#g_t_0040unnumberedsec-_0040appendixsec-_0040heading), Up: [Chapter Structuring](#Chapter-Structuring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 4.8 `@subsection`: Subsections Below Sections [¶](#g_t_0040subsection_003a-Subsections-Below-Sections)

Subsections are to sections as sections are to chapters;
see [`@section`: Sections Below Chapters](#g_t_0040section). For example:

```
@subsection This is a subsection
```

Subsection titles are listed in the table of contents.

---

Next: [`@subsubsection` and Other Subsub Commands](#g_t_0040subsubsection), Previous: [`@subsection`: Subsections Below Sections](#g_t_0040subsection), Up: [Chapter Structuring](#Chapter-Structuring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 4.9 The `@subsection`-like Commands [¶](#The-_0040subsection_002dlike-Commands)

The `@unnumberedsubsec`, `@appendixsubsec`, and
`@subheading` commands are, respectively, the unnumbered,
appendix-like, and heading-like equivalents of the `@subsection`
command. (See [`@subsection`: Subsections Below Sections](#g_t_0040subsection).)

`@unnumberedsubsec` and `@appendixsubsec` do not need to
be used in ordinary circumstances, because `@subsection` may
also be used within sections of `@unnumbered` and
`@appendix` chapters (see [`@section`: Sections Below Chapters](#g_t_0040section)).

An `@subheading` command produces a heading like that of a
subsection except that it is not numbered and does not appear in the
table of contents. Similarly, an `@unnumberedsubsec` command
produces an unnumbered heading like that of a subsection and an
`@appendixsubsec` command produces a subsection-like heading
labeled with a letter and numbers; both of these commands produce
headings that appear in the table of contents.

---

Next: [`@part`: Groups of Chapters](#g_t_0040part), Previous: [The `@subsection`-like Commands](#g_t_0040unnumberedsubsec-_0040appendixsubsec-_0040subheading), Up: [Chapter Structuring](#Chapter-Structuring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 4.10 `@subsubsection` and Other Subsub Commands [¶](#g_t_0040subsubsection-and-Other-Subsub-Commands)

The fourth and lowest level sectioning commands in Texinfo are the
‘subsub’ commands. They are:

`@subsubsection`
:   Subsubsections are to subsections as subsections are to sections.
    (See [`@subsection`: Subsections Below Sections](#g_t_0040subsection).) Subsubsection titles appear in the
    table of contents.

`@unnumberedsubsubsec`
:   Unnumbered subsubsection titles appear in the table of contents,
    but lack numbers. Otherwise, unnumbered subsubsections are the same
    as subsubsections.

`@appendixsubsubsec`
:   Conventionally, appendix commands are used only for appendices and are
    lettered and numbered appropriately. They also appear in the table
    of contents.

`@subsubheading`
:   The `@subsubheading` command may be used anywhere that you want
    a small heading that will not appear in the table of contents.

As with subsections, `@unnumberedsubsubsec` and
`@appendixsubsubsec` do not need to be used in ordinary
circumstances, because `@subsubsection` may also be used within
subsections of `@unnumbered` and `@appendix` chapters
(see [`@section`: Sections Below Chapters](#g_t_0040section)).

---

Next: [Raise/lower Sections: `@raisesections` and `@lowersections`](#Raise_002flower-sections), Previous: [`@subsubsection` and Other Subsub Commands](#g_t_0040subsubsection), Up: [Chapter Structuring](#Chapter-Structuring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 4.11 `@part`: Groups of Chapters [¶](#g_t_0040part_003a-Groups-of-Chapters)

The final sectioning command is `@part`, to mark a *part* of
a manual, that is, a group of chapters or (rarely) appendices. This
behaves quite differently from the other sectioning commands, to fit
with the way such “parts” are conventionally used in books.

No `@node` command is associated with `@part`. Just write
the command on a line by itself, including the part title, at the
place in the document you want to mark off as starting that part. For
example:

```
@part Part I:@* The beginning
```

As can be inferred from this example, no automatic numbering or
labeling of the `@part` text is done. The text is taken as-is.

Because parts are not associated with nodes, no general text can
follow the `@part` line. To produce the intended output, it
must be followed by a chapter-level command (including its node).
Thus, to continue the example:

```
@part Part I:@* The beginning

@node Introduction
@chapter Introduction
...
```

In the TeX output, the `@part` text is included in both the
normal and short tables of contents (see [Generating a Table of Contents](#Contents)), without a page
number (since that is the normal convention). In addition, a “part
page” is output in the body of the document, with just the
`@part` text. In the example above, the `@*` causes a
line break on the part page (but is replaced with a space in the
tables of contents). This part page is always forced to be on an odd
(right-hand) page, regardless of the chapter pagination
(see [`@setchapternewpage`: Blank Pages Before Chapters](#g_t_0040setchapternewpage)). In the LaTeX output,
the `@part` is output as `\part`.

In the HTML output, the `@part` text is similarly included in
the tables of contents, and a heading is included in the main document
text, as part of the following chapter or appendix node.

In the DocBook output, the `<part>` element includes all
the following chapters, up to the next `<part>`. A `<part>`
containing chapters is also closed at an appendix.

In the Info and plain text output, `@part` has no effect.

`@part` is ignored when raising or lowering sections (see next
section). That is, it is never lowered and nothing can be raised to it.

---

Previous: [`@part`: Groups of Chapters](#g_t_0040part), Up: [Chapter Structuring](#Chapter-Structuring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 4.12 Raise/lower Sections: `@raisesections` and `@lowersections` [¶](#Raise_002flower-Sections_003a-_0040raisesections-and-_0040lowersections)

The `@raisesections` and `@lowersections` commands
implicitly raise and lower the hierarchical level of following
chapters, sections and the other sectioning commands (excluding parts).

That is, the `@raisesections` command changes sections to
chapters, subsections to sections, and so on. Conversely, the
`@lowersections` command changes chapters to sections, sections
to subsections, and so on. Thus, a `@lowersections` command
cancels a `@raisesections` command, and vice versa.

As a practical matter, you generally only want to raise or lower large
chunks, usually in external files.
You can use `@lowersections` to include text written as an outer
or standalone Texinfo file in another Texinfo file as an inner,
included file (see [Include Files](#Include-Files)). Typical usage looks like this:

```
@lowersections
@include somefile.texi
@raisesections
```

(Without the `@raisesections`, all the subsequent
sections in the main file would also be lowered.)

If the included file being lowered has a `@top` node, you’ll
need to conditionalize its inclusion with a flag (see [`@set` and `@value`](#g_t_0040set-_0040value)).

Any menus in the final result have to take the raising and lowering
into account, so arbitrarily sprinkling `@raisesections` and
`@lowersections` commands throughout the document will likely
lead to errors (unless the menus in your document are all generated
automatically).

Repeated use of the commands continues to raise or lower the
hierarchical level a step at a time. An attempt to raise above
‘chapter’ reproduces chapter commands; an attempt to lower below
‘subsubsection’ reproduces subsubsection commands. Also, lowered
subsubsections and raised chapters will not work with
`texi2any`’s feature of implicitly determining node pointers,
since the menu structure cannot be represented correctly.

Write each `@raisesections` and `@lowersections` command
on a line of its own.

---

Next: [Marking Text, Words and Phrases](#Marking-Text), Previous: [Chapter Structuring](#Chapter-Structuring), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 5 Cross-references [¶](#Cross_002dreferences)

*Cross-references* are used to refer the reader to other parts of the
same or different Texinfo files.

Use cross-references to provide access to information that is too
detailed for the current context, or incidental to it. An online
help system or a reference manual is not like a novel; few read such
documents in sequence from beginning to end. Instead, people look
up what they need. For this reason, such creations should contain
many cross-references to help readers find other information that
they may not have read.

In a printed manual, a cross-reference results in a page reference,
unless it is to another manual altogether, in which case the
cross-reference names that manual. In Info, a cross-reference results
in an entry that you can follow using the Info ‘`f`’ command.
(See [Following cross-references](https://www.gnu.org/software/emacs/manual/html_mono/info.html#Help_002dXref) in Info.) In HTML, a
cross-reference results in an hyperlink. In DocBook, the `<link>`
element is used for cross-references unless it is to another manual,
in which case the cross-reference names that manual.

The various cross-reference commands use nodes, anchors
(see [`@anchor`: Defining Arbitrary Cross-reference Targets](#g_t_0040anchor)) or float labels (see [`@float` [type][,label]: Floating Material](#g_t_0040float)) to define
cross-reference locations. When TeX generates a DVI file, it records each
cross-reference location page number and uses the page numbers in making
references. Thus, even if you are writing a manual that will only be printed,
and not used online, you must nonetheless write `@node` lines (or
`@anchor` anchors) in order to name the places to which you make
cross-references.

* [Different Cross-reference Commands](#Cross-Reference-Commands)
* [Parts of a Cross-reference](#Cross-Reference-Parts)
* [`@xref` with One Argument](#One-Argument)
* [`@xref` with Two Arguments](#Two-Arguments)
* [`@xref` with Three Arguments](#Three-Arguments)
* [`@xref` with Four and Five Arguments](#Four-and-Five-Arguments)
* [Referring to a Manual as a Whole](#Referring-to-a-Manual-as-a-Whole)
* [`@xref`](#g_t_0040xref)
* [`@ref`](#g_t_0040ref)
* [`@pxref`](#g_t_0040pxref)
* [`@anchor`: Defining Arbitrary Cross-reference Targets](#g_t_0040anchor)
* [`@link`: Plain, unadorned hyperlink](#g_t_0040link)
* [`@inforef`: Cross-references to Info-only Material](#g_t_0040inforef)
* [`@url`, `@uref{url[, text][, replacement]}`](#g_t_0040url)
* [`@cite`{reference}](#g_t_0040cite)
* [PDF Colors](#PDF-Colors)

---

Next: [Parts of a Cross-reference](#Cross-Reference-Parts), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.1 Different Cross-reference Commands [¶](#Different-Cross_002dreference-Commands)

There are three different cross-reference commands:

`@xref`
:   Used to start a sentence with an Info cross-reference saying
    ‘`*Note name: node.`’ or with ‘See …’
    in other output formats.

`@ref`
:   Used within or, more often, at the end of a sentence; produces
    an Info cross-reference saying ‘`*note name: node.`’,
    and just the reference in other output formats,
    without the preceding ‘See’.

`@pxref`
:   Used within parentheses, at the end of a sentence, or otherwise before
    punctuation, to make a reference. Its output starts with a lowercase
    ‘`*note`’ in Info, and with a lowercase ‘see’ in the other output
    formats. (‘`p`’ is for ‘parenthesis’.)

Additionally, there are commands to produce references to documents
outside the Texinfo system. The `@cite` command is used
to make references to books and manuals. `@url` produces
a URL, for example a reference to a page on the World
Wide Web.

---

Next: [`@xref` with One Argument](#One-Argument), Previous: [Different Cross-reference Commands](#Cross-Reference-Commands), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]
