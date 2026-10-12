### D.3 Inserting Frequently Used Commands [¶](#Inserting-Frequently-Used-Commands)

Texinfo mode provides commands to insert various frequently used
@-commands into the buffer. You can use these commands to save
keystrokes.

The insert commands are invoked by typing `C-c` twice and then the
first letter of the @-command:

`C-c C-c c` [¶](#index-texinfo_002dinsert_002d_0040code)

`M-x texinfo-insert-@code`
:   Insert `@code{}` and put the
    cursor between the braces.

`C-c C-c d` [¶](#index-texinfo_002dinsert_002d_0040dfn)

`M-x texinfo-insert-@dfn`
:   Insert `@dfn{}` and put the
    cursor between the braces.

`C-c C-c e` [¶](#index-texinfo_002dinsert_002d_0040end)

`M-x texinfo-insert-@end`
:   Insert `@end` and attempt to insert the correct following word,
    such as ‘`example`’ or ‘`table`’. (This command does not handle
    nested lists correctly, but inserts the word appropriate to the
    immediately preceding list.)

`C-c C-c i` [¶](#index-texinfo_002dinsert_002d_0040item)

`M-x texinfo-insert-@item`
:   Insert `@item` and put the
    cursor at the beginning of the next line.

`C-c C-c k` [¶](#index-texinfo_002dinsert_002d_0040kbd)

`M-x texinfo-insert-@kbd`
:   Insert `@kbd{}` and put the
    cursor between the braces.

`C-c C-c n` [¶](#index-texinfo_002dinsert_002d_0040node)

`M-x texinfo-insert-@node`
:   Insert `@node` and a comment line
    listing the sequence for the ‘Next’,
    ‘Previous’, and ‘Up’ nodes.
    Leave point after the `@node`.

`C-c C-c o` [¶](#index-texinfo_002dinsert_002d_0040noindent)

`M-x texinfo-insert-@noindent`
:   Insert `@noindent` and put the
    cursor at the beginning of the next line.

`C-c C-c r` [¶](#index-texinfo_002dinsert_002ddwim_002d_0040ref)

`M-x texinfo-insert-dwim-@ref`
:   This function and binding were added in Emacs 27.1.
    Inserts one of `@pxref{}`, `@xref{}`, or
    `@ref{}` based on the text around point; calling it near an
    unclosed preceding open parenthesis results in `@pxref{}`, at
    the beginning of a sentence or at `(point-min)` yields
    `@xref{}`, any other location (including inside a word), will
    result in `@ref{}`. A numeric argument says how many words
    the braces should surround. Puts the cursor between the braces.

`C-c C-c s` [¶](#index-texinfo_002dinsert_002d_0040samp)

`M-x texinfo-insert-@samp`
:   Insert `@samp{}` and put the
    cursor between the braces.

`C-c C-c t` [¶](#index-texinfo_002dinsert_002d_0040table)

`M-x texinfo-insert-@table`
:   Insert `@table` followed by a `SPC`
    and leave the cursor after the `SPC`.

`C-c C-c v` [¶](#index-texinfo_002dinsert_002d_0040var)

`M-x texinfo-insert-@var`
:   Insert `@var{}` and put the
    cursor between the braces.

`C-c C-c x` [¶](#index-texinfo_002dinsert_002d_0040example)

`M-x texinfo-insert-@example`
:   Insert `@example` and put the
    cursor at the beginning of the next line.

`C-c C-c {` [¶](#index-texinfo_002dinsert_002dbraces)

`M-x texinfo-insert-braces`
:   Insert `{}` and put the cursor between the braces.

`C-c }` [¶](#index-up_002dlist)

`C-c ]`

`M-x up-list`
:   Move from between a pair of braces forward past the closing brace.
    Typing `C-c ]` is easier than typing `C-c }`, which
    is, however, more mnemonic; hence the two keybindings. (Also, you can
    move out from between braces by typing `C-f`.)

To put a command such as `@code{…}` around an
*existing* word, position the cursor in front of the word and type
`C-u 1 C-c C-c c`. This makes it easy to edit existing plain text.
The value of the prefix argument tells Emacs how many words following
point to include between braces—‘`1`’ for one word, ‘`2`’ for
two words, and so on. Use a negative argument to enclose the previous
word or words. If you do not specify a prefix argument, Emacs inserts
the @-command string and positions the cursor between the braces. This
feature works only for those @-commands that operate on a word or words
within one line, such as `@kbd` and `@var`.

This set of insert commands was created after analyzing the frequency
with which different @-commands are used in the GNU Emacs
Manual and the GDB Manual. If you wish to add your own insert
commands, you can bind a keyboard macro to a key, use abbreviations,
or extend the code in `texinfo.el`.

`C-c C-c C-d` (`texinfo-start-menu-description`) is an insert
command that works differently from the other insert commands. It
inserts a node’s section or chapter title in the space for the
description in a menu entry line. (A menu entry has three parts, the
entry name, the node name, and the description. Only the node name is
required, but a description helps explain what the node is about.
See [The Parts of a Menu](#Menu-Parts).)

To use `texinfo-start-menu-description`, position point in a menu
entry line and type `C-c C-c C-d`. The command looks for and copies
the title that goes with the node name, and inserts the title as a
description; it positions point at the beginning of the inserted text so you
can edit it. The function does not insert the title if the menu entry
line already contains a description.

This command is only an aid to writing descriptions; it does not do the
whole job. You must edit the inserted text since a title tends to use
the same words as a node name but a useful description uses different
words.

---

Next: [Updating Nodes and Menus](#Updating-Nodes-and-Menus), Previous: [Inserting Frequently Used Commands](#Inserting), Up: [Using Texinfo Mode](#Texinfo-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### D.4 Showing the Sectioning Structure of a File [¶](#Showing-the-Sectioning-Structure-of-a-File)

You can show the sectioning structure of a Texinfo file by using the
`C-c C-s` command (`texinfo-show-structure`). This command
lists the lines that begin with the @-commands for `@chapter`,
`@section`, and the like. It constructs what amounts to a table
of contents. These lines are displayed in another buffer called the
‘`*Occur*`’ buffer. In that buffer, you can position the cursor
over one of the lines and use the `C-c C-c` command
(`occur-mode-goto-occurrence`), to jump to the corresponding spot
in the Texinfo file.

`C-c C-s` [¶](#index-texinfo_002dshow_002dstructure)

`M-x texinfo-show-structure`
:   Show the `@chapter`, `@section`, and such lines of a
    Texinfo file.

`C-c C-c` [¶](#index-occur_002dmode_002dgoto_002doccurrence)

`M-x occur-mode-goto-occurrence`
:   Go to the line in the Texinfo file corresponding to the line under the
    cursor in the `*Occur*` buffer.

If you call `texinfo-show-structure` with a prefix argument by
typing `C-u C-c C-s`, it will list not only those lines with the
@-commands for `@chapter`, `@section`, and the like, but
also the `@node` lines. You can use `texinfo-show-structure`
with a prefix argument to check whether the ‘Next’, ‘Previous’, and ‘Up’
pointers of an `@node` line are correct.

Often, when you are working on a manual, you will be interested only
in the structure of the current chapter. In this case, you can mark
off the region of the buffer that you are interested in by using the
`C-x n n` (`narrow-to-region`) command and
`texinfo-show-structure` will work on only that region. To see
the whole buffer again, use `C-x n w` (`widen`).
(See [Narrowing](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Narrowing) in The GNU Emacs Manual, for more
information about the narrowing commands.)

In addition to providing the `texinfo-show-structure` command,
Texinfo mode sets the value of the page delimiter variable to match
the chapter-level @-commands. This enables you to use the `C-x
]` (`forward-page`) and `C-x [` (`backward-page`)
commands to move forward and backward by chapter, and to use the
`C-x n p` (`narrow-to-page`) command to narrow to a chapter.
See [Pages](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Pages) in The GNU Emacs Manual, for more information
about the page commands.

* [Using `texinfo-show-structure`](#Using-texinfo_002dshow_002dstructure)
* [Using `occur`](#Using-occur)

---

Next: [Using `occur`](#Using-occur), Up: [Showing the Sectioning Structure of a File](#Showing-the-Structure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.4.1 Using `texinfo-show-structure` [¶](#Using-texinfo_002dshow_002dstructure-1)

It is not always easy to keep track of the nodes, chapters, sections, and
subsections of a Texinfo file. This is especially true if you are revising
or adding to a Texinfo file that someone else has written.

In GNU Emacs, in Texinfo mode, the `texinfo-show-structure`
command lists all the lines that begin with the @-commands that
specify the structure: `@chapter`, `@section`,
`@appendix`, and so on. With an argument (`C-u`
as prefix argument, if interactive),
the command also shows the `@node` lines. The
`texinfo-show-structure` command is bound to `C-c C-s` in
Texinfo mode, by default.

The lines are displayed in a buffer called the ‘`*Occur*`’ buffer,
indented by hierarchical level. For example, here is a part of what was
produced by running `texinfo-show-structure` on this manual:

```
Lines matching "^@\\(chapter \\|sect\\|subs\\|subh\\|
unnum\\|major\\|chapheading \\|heading \\|appendix\\)"
in buffer texinfo.texi.
...
4177:@chapter Nodes
4198:    @heading Two Paths
4231:    @section Node and Menu Illustration
4337:    @section The @code{@@node} Command
4393:        @subheading Choosing Node and Pointer Names
4417:        @subsection How to Write a @code{@@node} Line
4469:        @subsection @code{@@node} Line Tips
...
```

This says that lines 4337, 4393, and 4417 of `texinfo.texi` begin
with the `@section`, `@subheading`, and `@subsection`
commands respectively. If you move your cursor into the ‘`*Occur*`’
window, you can position the cursor over one of the lines and use the
`C-c C-c` command (`occur-mode-goto-occurrence`), to jump to
the corresponding spot in the Texinfo file. See [Using Occur](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Other-Repeating-Search) in The GNU Emacs Manual, for more
information about `occur-mode-goto-occurrence`.

The first line in the ‘`*Occur*`’ window describes the *regular
expression* specified by texinfo-heading-pattern. This regular
expression is the pattern that `texinfo-show-structure` looks for.
See [Using Regular Expressions](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Regexps) in The GNU Emacs Manual,
for more information.

When you invoke the `texinfo-show-structure` command, Emacs will
display the structure of the whole buffer. If you want to see the
structure of just a part of the buffer, of one chapter, for example,
use the `C-x n n` (`narrow-to-region`) command to mark the
region. (See [Narrowing](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Narrowing) in The GNU Emacs Manual.) This is
how the example used above was generated. (To see the whole buffer
again, use `C-x n w` (`widen`).)

If you call `texinfo-show-structure` with a prefix argument by
typing `C-u C-c C-s`, it will list lines beginning with
`@node` as well as the lines beginning with the @-sign commands
for `@chapter`, `@section`, and the like.

You can remind yourself of the structure of a Texinfo file by looking at
the list in the ‘`*Occur*`’ window; and if you have mis-named a node
or left out a section, you can correct the mistake.

---

Previous: [Using `texinfo-show-structure`](#Using-texinfo_002dshow_002dstructure), Up: [Showing the Sectioning Structure of a File](#Showing-the-Structure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.4.2 Using `occur` [¶](#Using-occur-1)

Sometimes the `texinfo-show-structure` command produces too much
information. Perhaps you want to remind yourself of the overall structure
of a Texinfo file, and are overwhelmed by the detailed list produced by
`texinfo-show-structure`. In this case, you can use the `occur`
command directly. To do this, type:

```
M-x occur
```

and then, when prompted, type a *regexp*, a regular expression for
the pattern you want to match. (See [Regular Expressions](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Regexps) in The GNU Emacs Manual.) The `occur` command works from
the current location of the cursor in the buffer to the end of the
buffer. If you want to run `occur` on the whole buffer, place
the cursor at the beginning of the buffer.

For example, to see all the lines that contain the word
‘`@chapter`’ in them, just type ‘`@chapter`’. This will
produce a list of the chapters. It will also list all the sentences
with ‘`@chapter`’ in the middle of the line.

If you want to see only those lines that start with the word
‘`@chapter`’, type ‘`^@chapter`’ when prompted by
`occur`. If you want to see all the lines that end with a word
or phrase, end the last word with a ‘`$`’; for example,
‘`catching mistakes$`’. This can be helpful when you want to see
all the nodes that are part of the same chapter or section and
therefore have the same ‘Up’ pointer.

See [Using Occur](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Other-Repeating-Search) in The GNU Emacs Manual,
for more information.

---

Next: [Formatting for Info](#Info-Formatting), Previous: [Showing the Sectioning Structure of a File](#Showing-the-Structure), Up: [Using Texinfo Mode](#Texinfo-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### D.5 Updating Nodes and Menus [¶](#Updating-Nodes-and-Menus-1)

The `texi2any` command will create an Info file for a hierarchically
organized Texinfo file that lacks ‘Next’, ‘Previous’ and ‘Up’ pointers
(see [Writing a `@node` Line](#Writing-a-Node)). Thus, in general, there is no need for explicit
‘Next’, ‘Previous’, and ‘Up’ pointers. In this setting, menus will be added
automatically for nodes without an explicit menu. (See [`texi2any`: The Translator for Texinfo](#Generic-Translator-texi2any), for more information about `texi2any`.)

If you still want explicit pointers, Texinfo mode provides commands for
automatically creating or updating menus and node pointers. The commands are
called “update” commands because their most frequent use is for updating a
Texinfo file after you have worked on it; but you can use them to insert the
‘Next’, ‘Previous’, and ‘Up’ pointers into an `@node` line that has none
and to create menus in a file that has none.

* [The Updating Commands](#Updating-Commands)
* [Updating Requirements](#Updating-Requirements)
* [Update Outer File and Include Files](#Update-Multiple-Files)
* [Include Files Requirements](#Include-Files-Requirements)
* [Other Updating Commands](#Other-Updating-Commands)

---

Next: [Updating Requirements](#Updating-Requirements), Up: [Updating Nodes and Menus](#Updating-Nodes-and-Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.5.1 The Updating Commands [¶](#The-Updating-Commands)

You can use the updating commands to:

* insert or update the ‘Next’, ‘Previous’, and ‘Up’ pointers of a node,
* insert or update the menu for a section, and
* create a master menu for a Texinfo source file.

You can also use the commands to update all the nodes and menus in a
region or in a whole Texinfo file.

The updating commands work only with conventional Texinfo files, which
are structured hierarchically like books. In such files, a structuring
command line must follow closely after each `@node` line, except
for the ‘Top’ `@node` line. (A *structuring command line* is
a line beginning with `@chapter`, `@section`, or other
similar command.)

You can write the structuring command line on the line that follows
immediately after an `@node` line or else on the line that
follows after a single `@comment` line or a single
`@ifinfo` line. You cannot interpose more than one line between
the `@node` line and the structuring command line; and you may
interpose only a `@comment` line or an `@ifinfo` line.

Commands which work on a whole buffer require that the ‘Top’ node be
followed by a node with a `@chapter` or equivalent-level command.
The menu updating commands will not create a main or master menu for a
Texinfo file that has only `@chapter`-level nodes! The menu
updating commands only create menus *within* nodes for lower level
nodes. To create a menu of chapters, you must provide a ‘Top’
node.

The menu updating commands remove menu entries that refer to other Info
files since they do not refer to nodes within the current buffer. This
is a deficiency. Rather than use menu entries, you can use cross
references to refer to other Info files. None of the updating commands
affect cross-references.

Texinfo mode has five updating commands that are used most often: two
are for updating the node pointers or menu of a single node (or a
region); two are for updating every node pointer and menu in a file;
and one, the `texinfo-master-menu` command, is for creating a
master menu for a complete file, and optionally, for updating every
node and menu in the whole Texinfo file.

The `texinfo-master-menu` command is the primary command:

`C-c C-u m` [¶](#index-texinfo_002dmaster_002dmenu)

`M-x texinfo-master-menu`
:   Create or update a master menu that includes all the other menus
    (incorporating the descriptions from pre-existing menus, if
    any).

    With an argument (prefix argument, `C-u,` if interactive), first create or
    update all the nodes and all the regular menus in the buffer before
    constructing the master menu. (See [The Top Node and
    Master Menu](#The-Top-Node), for more about a master menu.)

    For `texinfo-master-menu` to work, the Texinfo file must have a
    ‘Top’ node and at least one subsequent node.

    After extensively editing a Texinfo file, you can type the following:

    ```
    C-u M-x texinfo-master-menu
    ```

    ```
    or
    ```

    ```
    C-u C-c C-u m
    ```

    This updates all the nodes and menus completely and all at once.

The other major updating commands do smaller jobs and are designed for
the person who updates nodes and menus as he or she writes a Texinfo
file.

The commands are:

`C-c C-u C-n` [¶](#index-texinfo_002dupdate_002dnode)

`M-x texinfo-update-node`
:   Insert the ‘Next’, ‘Previous’, and ‘Up’ pointers for the node that point is
    within (i.e., for the `@node` line preceding point). If the
    `@node` line has pre-existing ‘Next’, ‘Previous’, or ‘Up’
    pointers in it, the old pointers are removed and new ones inserted.
    With an argument (prefix argument, `C-u`, if interactive), this command
    updates all `@node` lines in the region (which is the text
    between point and mark).

`C-c C-u C-m` [¶](#index-texinfo_002dmake_002dmenu)

`M-x texinfo-make-menu`
:   Create or update the menu in the node that point is within.
    With an argument (`C-u` as prefix argument, if
    interactive), the command makes or updates menus for the
    nodes which are either within or a part of the
    region.

    Whenever `texinfo-make-menu` updates an existing menu, the
    descriptions from that menu are incorporated into the new menu. This
    is done by copying descriptions from the existing menu to the entries
    in the new menu that have the same node names. If the node names are
    different, the descriptions are not copied to the new menu.

`C-c C-u C-e` [¶](#index-texinfo_002devery_002dnode_002dupdate)

`M-x texinfo-every-node-update`
:   Insert or update the ‘Next’, ‘Previous’, and ‘Up’ pointers for every
    node in the buffer.

`C-c C-u C-a` [¶](#index-texinfo_002dall_002dmenus_002dupdate)

`M-x texinfo-all-menus-update`
:   Create or update all the menus in the buffer. With an argument
    (`C-u` as prefix argument, if interactive), first insert
    or update all the node
    pointers before working on the menus.

    If a master menu exists, the `texinfo-all-menus-update` command
    updates it; but the command does not create a new master menu if none
    already exists. (Use the `texinfo-master-menu` command for
    that.)

    When working on a document that does not merit a master menu, you can
    type the following:

    ```
    C-u C-c C-u C-a
    ```

    ```
    or
    ```

    ```
    C-u M-x texinfo-all-menus-update
    ```

    This updates all the nodes and menus.

The `texinfo-column-for-description` variable specifies the
column to which menu descriptions are indented. By default, the value
is 32 although it can be useful to reduce it to as low as 24. You
can set the variable via customization (see [Customization](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Customization) in The GNU Emacs Manual) or with the `M-x set-variable`
command (see [Examining and Setting Variables](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Examining) in The GNU Emacs Manual).

Also, the `texinfo-indent-menu-description` command may be used to
indent existing menu descriptions to a specified column. Finally, if
you wish, you can use the `texinfo-insert-node-lines` command to
insert missing `@node` lines into a file. In particular, you can
ignore `@node` lines altogether in your first draft and then
use the `texinfo-insert-node-lines` command to create `@node`
lines for you. However, we do not recommend this practice. It is better
to name the node itself at the same time that you write a segment so you
can easily make cross-references. Useful cross-references are an
especially important feature of a good Texinfo manual.
(See [Other Updating Commands](#Other-Updating-Commands), for more information.)

---

Next: [Update Outer File and Include Files](#Update-Multiple-Files), Previous: [The Updating Commands](#Updating-Commands), Up: [Updating Nodes and Menus](#Updating-Nodes-and-Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.5.2 Updating Requirements [¶](#Updating-Requirements-1)

To use the updating commands, you must organize the Texinfo file
hierarchically with chapters, sections, subsections, and the like.
When you construct the hierarchy of the manual, do not ‘jump down’
more than one level at a time: you can follow the ‘Top’ node with a
chapter, but not with a section; you can follow a chapter with a
section, but not with a subsection. However, you may ‘jump up’ any
number of levels at one time—for example, from a subsection to a
chapter.

Each `@node` line, with the exception of the line for the ‘Top’
node, must be followed by a line with a structuring command such as
`@chapter`, `@section`, or
`@unnumberedsubsec`.

Each `@node` line/structuring-command line combination
must look either like this:

```
@node     Comments,  Minimum, Conventions, Overview
@comment  node-name, next,    previous,    up
@section Comments
```

or like this (without the `@comment` line):

```
@node Comments, Minimum, Conventions, Overview
@section Comments
```

or like this (without the explicit node pointers):

```
@node Comments
@section Comments
```

In this example, ‘Comments’ is the name of both the node and the
section. The next node is called ‘Minimum’ and the previous node is
called ‘Conventions’. The ‘Comments’ section is within the ‘Overview’
node, which is specified by the ‘Up’ pointer.

If a file has a ‘Top’ node, it must be called ‘`top`’ or ‘`Top`’
and be the first node in the file.

The menu updating commands create a menu of sections within a chapter,
a menu of subsections within a section, and so on. This means that
you must have a ‘Top’ node if you want a menu of chapters.

---

Next: [Include Files Requirements](#Include-Files-Requirements), Previous: [Updating Requirements](#Updating-Requirements), Up: [Updating Nodes and Menus](#Updating-Nodes-and-Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.5.3 Update Outer File and Include Files [¶](#Update-Outer-File-and-Include-Files)

GNU Emacs Texinfo mode provides the
`texinfo-multiple-files-update` command. This command creates or
updates ‘Next’, ‘Previous’, and ‘Up’ pointers of included files as
well as those in the outer or overall Texinfo file, and it creates or
updates a main menu in the outer file. Depending on whether you call
it with optional arguments, the command updates only the pointers in
the first `@node` line of the included files or all of them.

With `C-u` as a prefix argument, create and insert a master menu in
the outer file. With a numeric prefix argument, such as `C-u 2`, first
update all the menus and all the ‘Next’, ‘Previous’, and ‘Up’ pointers
of all the included files before creating and inserting a master menu in
the outer file.

In more details:

`M-x texinfo-multiple-files-update`
:   Called without any arguments:

    * Create or update the ‘Next’, ‘Previous’, and ‘Up’ pointers of the
      first `@node` line in each file included in an outer or overall
      Texinfo file.
    * Create or update the ‘Top’ level node pointers of the outer or
      overall file.
    * Create or update a main menu in the outer file.

`C-u M-x texinfo-multiple-files-update`
:   Called with `C-u` as a prefix argument:

    * Create or update pointers in the first `@node` line in each
      included file.
    * Create or update the ‘Top’ level node pointers of the outer file.
    * Create and insert a master menu in the outer file. The master menu
      is made from all the menus in all the included files.

`C-u 8 M-x texinfo-multiple-files-update`
:   Called with a numeric prefix argument, such as `C-u 8`:

    * Create or update *all* the ‘Next’, ‘Previous’, and ‘Up’ pointers
      of all the included files.
    * Create or update *all* the menus of all the included
      files.
    * Create or update the ‘Top’ level node pointers of the outer or
      overall file.
    * And then create a master menu in the outer file. This is similar to
      invoking `texinfo-master-menu` with an argument when you are
      working with just one file.

Note the use of the prefix argument in interactive use: with a regular
prefix argument, just `C-u`, the
`texinfo-multiple-files-update` command inserts a master menu;
with a numeric prefix argument, such as `C-u 8`, the command
updates *every* pointer and menu in *all* the files and
then inserts a master menu.

---

Next: [Other Updating Commands](#Other-Updating-Commands), Previous: [Update Outer File and Include Files](#Update-Multiple-Files), Up: [Updating Nodes and Menus](#Updating-Nodes-and-Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.5.4 Include Files Requirements [¶](#Include-Files-Requirements-1)

If you plan to use the `texinfo-multiple-files-update` command,
the outer Texinfo file that lists included files within it should
contain nothing but the beginning and end parts of a Texinfo file, and
a number of `@include` commands listing the included files. It
should not even include indices, which should be listed in an included
file of their own.

Moreover, each of the included files must contain exactly one highest
level node (conventionally, `@chapter` or equivalent),
and this node must be the first node in the included file.
Furthermore, each of these highest level nodes in each included file
must be at the same hierarchical level in the file structure.
Usually, each is a `@chapter`, an `@appendix`, or an
`@unnumbered` node. Thus, normally, each included file contains
one, and only one, chapter or equivalent-level node.

The outer file should contain only *one* node, the ‘Top’ node. It
should *not* contain any nodes besides the single ‘Top’ node. The
`texinfo-multiple-files-update` command will not process
them.

---

Previous: [Include Files Requirements](#Include-Files-Requirements), Up: [Updating Nodes and Menus](#Updating-Nodes-and-Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.5.5 Other Updating Commands [¶](#Other-Updating-Commands-1)

In addition to the major updating commands, Texinfo mode possesses several
less frequently used updating commands:

`M-x texinfo-insert-node-lines` [¶](#index-texinfo_002dinsert_002dnode_002dlines)
:   Insert `@node` lines before the `@chapter`,
    `@section`, and other sectioning commands wherever they are
    missing throughout a region in a Texinfo file.

    With an argument (`C-u` as prefix argument, if interactive), the
    command `texinfo-insert-node-lines` not only inserts
    `@node` lines but also inserts the chapter or section titles as
    the names of the corresponding nodes. In addition, it inserts the
    titles as node names in pre-existing `@node` lines that lack
    names. Since node names should be more concise than section or
    chapter titles, you must manually edit node names so inserted.

    For example, the following marks a whole buffer as a region and inserts
    `@node` lines and titles throughout:

    ```
    C-x h C-u M-x texinfo-insert-node-lines
    ```

    This command inserts titles as node names in `@node` lines; the
    `texinfo-start-menu-description` command (see [Inserting Frequently Used Commands](#Inserting)) inserts titles as descriptions in
    menu entries, a different action. However, in both cases, you need to
    edit the inserted text.

`M-x texinfo-indent-menu-description` [¶](#index-texinfo_002dindent_002dmenu_002ddescription)
:   Indent every description in the menu following point to the specified
    column. You can use this command to give yourself more space for
    descriptions. With an argument (`C-u` as prefix argument, if
    interactive), the `texinfo-indent-menu-description` command indents
    every description in every menu in the region. However, this command
    does not indent the second and subsequent lines of a multi-line
    description.

`M-x texinfo-sequential-node-update` [¶](#index-texinfo_002dsequential_002dnode_002dupdate)
:   Insert the names of the nodes immediately following and preceding the
    current node as the ‘Next’ or ‘Previous’ pointers regardless of those
    nodes’ hierarchical level. This means that the ‘Next’ node of a
    subsection may well be the next chapter. Sequentially ordered nodes are
    useful for novels and other documents that you read through
    sequentially. (However, in Info, the `g *` command lets
    you look through the file sequentially, so sequentially ordered nodes
    are not strictly necessary.) With an argument (prefix argument, if
    interactive), the `texinfo-sequential-node-update` command
    sequentially updates all the nodes in the region.

---

Next: [Formatting and Printing with Emacs](#Printing-with-Emacs), Previous: [Updating Nodes and Menus](#Updating-Nodes-and-Menus), Up: [Using Texinfo Mode](#Texinfo-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### D.6 Formatting for Info [¶](#Formatting-for-Info)

Texinfo mode provides several commands for formatting part or all of a
Texinfo file for Info.

* [Running `texi2any`/`makeinfo` Within Emacs](#texi2any-in-Emacs)
* [The `texinfo-format…` Commands](#texinfo_002dformat-commands)

---

Next: [The `texinfo-format…` Commands](#texinfo_002dformat-commands), Up: [Formatting for Info](#Info-Formatting)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.6.1 Running `texi2any`/`makeinfo` Within Emacs [¶](#Running-texi2any_002fmakeinfo-Within-Emacs)

The `texi2any` program provides better error messages
than either of the Emacs formatting commands. We recommend it.
The `texi2any` program is independent of Emacs.

You can run `texi2any` (or `makeinfo`) in GNU Emacs
Texinfo mode by using either the `makeinfo-region` or the
`makeinfo-buffer` commands. In Texinfo mode, the commands
are bound to `C-c C-m C-r` and `C-c C-m C-b` by default.

`C-c C-m C-r`

`M-x makeinfo-region`
:   Format the current region for Info.

`C-c C-m C-b`

`M-x makeinfo-buffer`
:   Format the current buffer for Info.

When you invoke `makeinfo-region` the output goes to a temporary
buffer. When you invoke `makeinfo-buffer` output goes to the
file set with `@setfilename` (see [`@setfilename`: Set the Output File Name](#g_t_0040setfilename)).

The Emacs `makeinfo-region` and `makeinfo-buffer` commands
run the `texi2any` program in a temporary shell buffer. If
`texi2any` finds any errors, Emacs displays the error messages in
the temporary buffer.

You can parse the error messages by typing `` C-x ` ``
(`next-error`). This causes Emacs to go to and position the
cursor on the line in the Texinfo source that `texi2any` thinks
caused the error. See [Running `make` or
Compilers Generally](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Compilation) in The GNU Emacs Manual, for more
information about using the `next-error` command.

In addition, you can kill the shell in which the `texi2any`
command is running or make the shell buffer display its most recent
output.

`C-c C-m C-k` [¶](#index-makeinfo_002dkill_002djob)

`M-x makeinfo-kill-job`
:   Kill the current running `texi2any` (or `makeinfo`) job
    (from `makeinfo-region` or `makeinfo-buffer`).

`C-c C-m C-l` [¶](#index-makeinfo_002drecenter_002doutput_002dbuffer)

`M-x makeinfo-recenter-output-buffer`
:   Redisplay the `texi2any` shell buffer to display its most recent
    output.

(Note that the parallel commands for killing and recentering a TeX
job are `C-c C-t C-k` and `C-c C-t C-l`. See [Formatting and Printing in Texinfo Mode](#Texinfo-Mode-Printing).)

You can specify options for `texi2any` by setting the
`makeinfo-options` variable with either the `M-x
customize` or the `M-x set-variable` command, or by setting the
variable in your `.emacs` initialization file.

For example, you could write the following in your `.emacs` file:

```
(setq makeinfo-options
     "--paragraph-indent=0 --no-split
      --fill-column=70 --verbose")
```

For more information, see  
[Easy Customization Interface](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Easy-Customization) in The GNU Emacs Manual,  
[Examining and Setting Variables](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Examining) in The GNU Emacs Manual,  
[Init File](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Init-File) in The GNU Emacs Manual, and  
[`texi2any` Options](#texi2any-Options).

---

Previous: [Running `texi2any`/`makeinfo` Within Emacs](#texi2any-in-Emacs), Up: [Formatting for Info](#Info-Formatting)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.6.2 The `texinfo-format…` Commands [¶](#The-texinfo_002dformat_2026-Commands)

In GNU Emacs in Texinfo mode, you can format part or all of a Texinfo
file with the `texinfo-format-region` command. This formats the
current region and displays the formatted text in a temporary buffer
called ‘`*Info Region*`’.

Similarly, you can format a buffer with the
`texinfo-format-buffer` command. This command creates a new
buffer and generates the Info file in it. Typing `C-x C-s` will
save the Info file under the name specified by the
`@setfilename` line which must be near the beginning of the
Texinfo file.

`C-c C-e C-r` [¶](#index-texinfo_002dformat_002dregion)

`texinfo-format-region`
:   Format the current region for Info.

`C-c C-e C-b` [¶](#index-texinfo_002dformat_002dbuffer)

`texinfo-format-buffer`
:   Format the current buffer for Info.

The `texinfo-format-region` and `texinfo-format-buffer`
commands provide you with some error checking, and other functions can
provide you with further help in finding formatting errors. These
procedures are described in an appendix; see [Catching Mistakes](#Catching-Mistakes).
However, the `texi2any` program provides better error checking
(see [Running `texi2any`/`makeinfo` Within Emacs](#texi2any-in-Emacs)).

A peculiarity of the `texinfo-format-buffer` and
`texinfo-format-region` commands is that they do not indent (nor
fill) paragraphs that contain `@w` or `@*` commands.

---

Next: [Texinfo Mode Summary](#Texinfo-Mode-Summary), Previous: [Formatting for Info](#Info-Formatting), Up: [Using Texinfo Mode](#Texinfo-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### D.7 Formatting and Printing with Emacs [¶](#Formatting-and-Printing-with-Emacs)

GNU Emacs can be used for formatting and printing with TeX,
from an Emacs Shell. Texinfo mode also provides predefined
key commands for formatting and printing.

* [Formatting and Printing in Texinfo Mode](#Texinfo-Mode-Printing)
* [Using the Local Variables List](#Compile_002dCommand)

---

Next: [Using the Local Variables List](#Compile_002dCommand), Up: [Formatting and Printing with Emacs](#Printing-with-Emacs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.7.1 Formatting and Printing in Texinfo Mode [¶](#Formatting-and-Printing-in-Texinfo-Mode)

Texinfo mode provides several predefined key commands for TeX
formatting and printing. These include commands for sorting indices,
looking at the printer queue, killing the formatting job, and
recentering the display of the buffer in which the operations
occur.

Often, when you are writing a document, you want to typeset and print
only part of a file to see what it will look like. You can use the
`texinfo-tex-region` and related commands for this purpose. Use
the `texinfo-tex-buffer` command to format all of a
buffer.

For `texinfo-tex-region` or `texinfo-tex-buffer` to work, the
file *must* start with a ‘`\input texinfo`’ line and must
include a `@settitle` line. The file must end with `@bye`
on a line by itself. (When you use `texinfo-tex-region`, you must
surround the `@settitle` line with start-of-header and
end-of-header lines.)

`C-c C-t C-b` [¶](#index-texinfo_002dtex_002dbuffer)

`M-x texinfo-tex-buffer`
:   Run `texi2dvi` on the buffer. In addition to running TeX on the
    buffer, this command automatically creates or updates indices as
    needed.

`C-c C-t C-r`

`M-x texinfo-tex-region`
:   Run TeX on the current region.

    If @-commands related to printed output are between the
    start-of-header and end-of-header lines, then
    `texinfo-tex-region` will format the
    region accordingly. For example, if you write the `@smallbook` command
    between the start-of-header and end-of-header lines, `texinfo-tex-region`,
    will format the region in “small” book size.

`C-c C-t C-i`

`M-x texinfo-texindex`
:   Run `texindex` to sort the indices of a Texinfo file formatted with
    `texinfo-tex-region`. The `texinfo-tex-region` command does
    not run `texindex` automatically; it only runs the `tex`
    typesetting command. You must run the `texinfo-tex-region` command
    a second time after sorting the raw index files with the `texindex`
    command. (Usually, you do not format an index when you format a region,
    only when you format a buffer. Now that the `texi2dvi` command
    exists, there is little or no need for this command.)

`C-c C-t C-p`

`M-x texinfo-tex-print`
:   Print a DVI file that was made with `texinfo-tex-region` or
    `texinfo-tex-buffer`.

`C-c C-t C-q`

`M-x tex-show-print-queue`
:   Show the print queue.

`C-c C-t C-d`

`M-x texinfo-delete-from-print-queue`
:   Delete a job from the print queue; you will be prompted for the job
    number shown by a preceding `C-c C-t C-q` command
    (`texinfo-show-tex-print-queue`).

`C-c C-t C-k`

`M-x tex-kill-job`
:   Kill the currently running TeX job started by either
    `texinfo-tex-region` or `texinfo-tex-buffer`, or any other
    process running in the Texinfo shell buffer.

`C-c C-t C-x`

`M-x texinfo-quit-job`
:   Quit a TeX formatting job that has stopped because of an error by
    sending an `x` to it. When you do this, TeX preserves a record
    of what it did in a `.log` file.

`C-c C-t C-l`

`M-x tex-recenter-output-buffer`
:   Redisplay the shell buffer in which the TeX printing and formatting
    commands are run to show its most recent output.

Thus, the usual sequence of commands for formatting a buffer is as
follows (with comments to the right):

```
C-c C-t C-b             Run texi2dvi on the buffer.
C-c C-t C-p             Print the DVI file.
C-c C-t C-q             Display the printer queue.
```

The Texinfo mode TeX formatting commands start a subshell in Emacs
called the `*tex-shell*`. The `texinfo-tex-command`,
`texinfo-texindex-command`, and `tex-dvi-print-command`
commands are all run in this shell.

You can watch the commands operate in the ‘`*tex-shell*`’ buffer,
and you can switch to and from and use the ‘`*tex-shell*`’ buffer
as you would any other shell buffer.

The formatting and print commands depend on the values of several variables.
The default values are:

```
    Variable                              Default value

texinfo-texi2dvi-command                  "texi2dvi"
texinfo-tex-command                       "tex"
texinfo-texindex-command                  "texindex"
texinfo-delete-from-print-queue-command   "lprm"
texinfo-tex-trailer                       "@bye"
tex-start-of-header                       "%**start"
tex-end-of-header                         "%**end"
tex-dvi-print-command                     "lpr -d"
tex-show-queue-command                    "lpq"
```

You can change the values of these variables with the `M-x
set-variable` command (see [Examining and Setting
Variables](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Examining) in The GNU Emacs Manual), or with your `.emacs`
initialization file (see [Init File](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Init-File) in The GNU Emacs
Manual).

Beginning with version 20, GNU Emacs offers a user-friendly interface,
called *Customize*, for changing values of user-definable variables.
See [Easy Customization Interface](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Easy-Customization) in The GNU
Emacs Manual, for more details about this. The Texinfo variables can
be found in the ‘`Development/Docs/Texinfo`’ group, once you invoke
the `M-x customize` command.

---

Previous: [Formatting and Printing in Texinfo Mode](#Texinfo-Mode-Printing), Up: [Formatting and Printing with Emacs](#Printing-with-Emacs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.7.2 Using the Local Variables List [¶](#Using-the-Local-Variables-List)

Yet another way to apply the TeX formatting command to a Texinfo file
is to put that command in a *local variables list* at the end of the
Texinfo file. You can then specify the `tex` or `texi2dvi`
commands as a `compile-command` and have Emacs run it by typing
`M-x compile`. This creates a special shell called the
`*compilation*` buffer in which Emacs runs the compile command.
For example, at the end of the `gdb.texi` file, after the
`@bye`, you could put the following:

```
Local Variables:
compile-command: "texi2dvi gdb.texi"
End:
```

This technique is most often used by programmers who also compile programs
this way; see [Compilation](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Compilation) in The GNU Emacs Manual.

---

Next: [Direct Formatting of Info files](#Formatting-Info-files), Previous: [Formatting and Printing with Emacs](#Printing-with-Emacs), Up: [Using Texinfo Mode](#Texinfo-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### D.8 Texinfo Mode Summary [¶](#Texinfo-Mode-Summary-1)

In Texinfo mode, each set of commands has default keybindings that
begin with the same keys. All the commands that are custom-created
for Texinfo mode begin with `C-c`. The keys are somewhat
mnemonic.

#### Insert Commands [¶](#Insert-Commands)

The insert commands are invoked by typing `C-c` twice and then the
first letter of the @-command to be inserted. (It might make more
sense mnemonically to use `C-c C-i`, for ‘custom insert’, but
`C-c C-c` is quick to type.)

```
C-c C-c c       Insert ‘@code’.
C-c C-c d       Insert ‘@dfn’.
C-c C-c e       Insert ‘@end’.
C-c C-c i       Insert ‘@item’.
C-c C-c n       Insert ‘@node’.
C-c C-c s       Insert ‘@samp’.
C-c C-c v       Insert ‘@var’.
C-c {       Insert braces.
C-c ]
C-c }       Move out of enclosing braces.
```

```
C-c C-c C-d     Insert a node’s section title
               in the space for the description
               in a menu entry line.
```

#### Show Structure [¶](#Show-Structure)

The `texinfo-show-structure` command is often used within a
narrowed region.

```
C-c C-s         List all the headings.
```

#### The Master Update Command [¶](#The-Master-Update-Command)

The `texinfo-master-menu` command creates a master menu; and can
be used to update every node and menu in a file as well.

```
C-c C-u m
M-x texinfo-master-menu
               Create or update a master menu.
```

```

```

```
C-u C-c C-u m   With C-u as a prefix argument, first
               create or update all nodes and regular
               menus, and then create a master menu.
```

#### Update Pointers [¶](#Update-Pointers)

The update pointer commands are invoked by typing `C-c C-u` and
then either `C-n` for `texinfo-update-node` or `C-e` for
`texinfo-every-node-update`.

```
C-c C-u C-n     Update a node.
C-c C-u C-e     Update every node in the buffer.
```

#### Update Menus [¶](#Update-Menus)

Invoke the update menu commands by typing `C-c C-u`
and then either `C-m` for `texinfo-make-menu` or
`C-a` for `texinfo-all-menus-update`. To update
both nodes and menus at the same time, precede `C-c C-u
C-a` with `C-u`.

```
C-c C-u C-m     Make or update a menu.
```

```
C-c C-u C-a     Make or update all
               menus in a buffer.
```

```

```

```
C-u C-c C-u C-a With C-u as a prefix argument,
               first create or update all nodes and
               then create or update all menus.
```

#### Format for Info [¶](#Format-for-Info)

The Info formatting commands that are written in Emacs Lisp are
invoked by typing `C-c C-e` and then either `C-r` for a region
or `C-b` for the whole buffer.

The Info formatting commands that are based on the
`texi2any`/`makeinfo` program are invoked by typing
`C-c C-m` and then either `C-r` for a region or `C-b` for
the whole buffer.

Use the `texinfo-format…` commands:

```
C-c C-e C-r     Format the region.
C-c C-e C-b     Format the buffer.
```

Use `texi2any`/`makeinfo`:

```
C-c C-m C-r     Format the region.
C-c C-m C-b     Format the buffer.
C-c C-m C-l     Recenter the texi2any output buffer.
C-c C-m C-k     Kill the texi2any formatting job.
```

#### Typeset and Print [¶](#Typeset-and-Print)

The TeX typesetting and printing commands are invoked by typing
`C-c C-t` and then another control command: `C-r` for
`texinfo-tex-region`, `C-b` for `texinfo-tex-buffer`,
and so on.

```
C-c C-t C-r     Run TeX on the region.
C-c C-t C-b     Run texi2dvi on the buffer.
C-c C-t C-i     Run texindex.
C-c C-t C-p     Print the DVI file.
C-c C-t C-q     Show the print queue.
C-c C-t C-d     Delete a job from the print queue.
C-c C-t C-k     Kill the current TeX formatting job.
C-c C-t C-x     Quit a currently stopped TeX formatting job.
C-c C-t C-l     Recenter the output buffer.
```

#### Other Updating Commands [¶](#Other-Updating-Commands-2)

The remaining updating commands do not have standard keybindings because
they are rarely used.

```
M-x texinfo-insert-node-lines
               Insert missing @node lines in region.
               With C-u as a prefix argument,
               use section titles as node names.
```

```

```

```
M-x texinfo-multiple-files-update
               Update a multi-file document.
               With C-u 2 as a prefix argument,
               create or update all nodes and menus
               in all included files first.
```

```

```

```
M-x texinfo-indent-menu-description
               Indent descriptions.
```

```

```

```
M-x texinfo-sequential-node-update
               Insert node pointers in strict sequence.
```

---

Next: [Catching Mistakes](#Catching-Mistakes), Previous: [Texinfo Mode Summary](#Texinfo-Mode-Summary), Up: [Using Texinfo Mode](#Texinfo-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### D.9 Direct Formatting of Info files [¶](#Direct-Formatting-of-Info-files)

In general, there is no need to edit or process Info files. Most of the
content of Info file is plain text, but the tag tables require computing
offsets for nodes and indirect files (see [Tag Files and Split Files](#Tag-and-Split-Files)), which is
impractical. The Texinfo processors output well-formatted Info from Texinfo
input. Also, lot of information from the Texinfo source is lost in the Info
file. The Texinfo source is therefore more suitable to analyse a manual, be it
for error reporting, for statistics, or to set up translations.

Still, especially with manually written Info files, there are some situations
where it may be interesting to process Info files directly, in particular
to compute Info files node offsets and to compute split Info files file
offsets. Two Emacs commands do that. `Info-tagify` adds a tag table
for a nonsplit file lacking one. This function is also useful for Info
file validation (see [Finding Badly Referenced Nodes](#Running-Info_002dvalidate)). It is
also possible to split a nonsplit Info file with `Info-split`.

* [Tagifying a File](#Tagifying)
* [Splitting a File Manually](#Splitting)

---

Next: [Splitting a File Manually](#Splitting), Up: [Direct Formatting of Info files](#Formatting-Info-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.9.1 Tagifying a File [¶](#Tagifying-a-File)

Texinfo processors create tag tables automatically. The only exception
arise when `texinfo-format-buffer` is called with a prefix, as
`C-u M-x texinfo-format-buffer`, to create an non split Info
file without a tag table (which can be useful in some cases for Info file
validation, see [Creating an Unsplit File and Adding a Tag Table](#Unsplit-and-Tagify)). Adding a tag table is therefore
more generally useful for manually written Info files.

To create a tag table for an nonsplit file, visit the Info file you
wish to tagify and type:

```
M-x Info-tagify
```

(Note the uppercase ‘`I`’ in `Info-tagify`.) This creates an
Info file with a tag table. A tag table is part of a well formed Info
file (see [Tag Files and Split Files](#Tag-and-Split-Files)). A tag table is also needed to validate
or split the Info file.

---

Previous: [Tagifying a File](#Tagifying), Up: [Direct Formatting of Info files](#Formatting-Info-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.9.2 Splitting a File Manually [¶](#Splitting-a-File-Manually)

You should split a large file or else let the
`texinfo-format-buffer` or `makeinfo-buffer` command do it
for you automatically. (Generally you will let one of the formatting
commands do this job for you. See [Formatting for Info](#Info-Formatting).)

The split-off files are called the indirect subfiles.

Tag tables are created automatically by the formatting command;
you only need to create a tag table yourself if you are doing
the job manually. See [Tagifying a File](#Tagifying), for information about
creating a tag table.

Visit the Info file you wish to tagify and split manually and type the
two commands:

```
M-x Info-tagify
M-x Info-split
```

(Note that the ‘`I`’ in ‘`Info`’ is uppercase.)

When you use the `Info-split` command, the buffer is modified into a
(small) Info file which lists the indirect subfiles. This file should be
saved in place of the original visited file. The indirect subfiles are
written in the same directory the original file is in, with names generated
by appending ‘`-`’ and a number to the original file name.

The primary file still functions as an Info file, but it contains just
the tag table and a directory of subfiles. See [Tag Files and Split Files](#Tag-and-Split-Files).

---

Next: [Batch Formatting](#Batch-Formatting), Previous: [Direct Formatting of Info files](#Formatting-Info-files), Up: [Using Texinfo Mode](#Texinfo-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### D.10 Catching Mistakes [¶](#Catching-Mistakes-1)

Besides mistakes in the content of your documentation, there are two
kinds of mistake you can make with Texinfo: you can make mistakes with
@-commands, and you can make mistakes with the structure of the nodes
and chapters.

For finding problems with @-commands, you can run TeX or a region
formatting command on the region that has a problem; indeed, you can
run these commands on each region as you write it.

For finding problems with the structure of nodes and chapters, you can use
`C-c C-s` (`texinfo-show-structure`) and the related `occur`
command and you can use the `M-x Info-validate` command.

* [`texi2any` Preferred](#texi2any-Preferred)
* [Catching Errors with Info Formatting](#Debugging-with-Info)
* [Debugging with TeX](#Debugging-with-TeX)
* [Finding Badly Referenced Nodes](#Running-Info_002dvalidate)

---

Next: [Catching Errors with Info Formatting](#Debugging-with-Info), Up: [Catching Mistakes](#Catching-Mistakes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.10.1 `texi2any` Preferred [¶](#texi2any-Preferred-1)

The `texi2any` program does an excellent job of catching errors
and reporting them—far better than `texinfo-format-region` or
`texinfo-format-buffer`. In addition, the various functions for
automatically creating and updating node pointers and menus remove
many opportunities for human error.

Use `texi2any` (or its Texinfo mode manifestations,
`makeinfo-region` and `makeinfo-buffer`) to format your
file and check for other errors. This is the best way to work with
Texinfo. But if you cannot use `texi2any`, or your problem
is very puzzling, then you may want to use the tools described
in this section.

---

Next: [Debugging with TeX](#Debugging-with-TeX), Previous: [`texi2any` Preferred](#texi2any-Preferred), Up: [Catching Mistakes](#Catching-Mistakes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.10.2 Catching Errors with Info Formatting [¶](#Catching-Errors-with-Info-Formatting)

After you have written part of a Texinfo file, you can use the
`texinfo-format-region` or the `makeinfo-region` command to
see whether the region formats properly.

Most likely, however, you are reading this section because for some
reason you cannot use the `makeinfo-region` command; therefore, the
rest of this section presumes that you are using
`texinfo-format-region`.

If you have made a mistake with an @-command,
`texinfo-format-region` will stop processing at or after the
error and display an error message. To see where in the buffer the
error occurred, switch to the ‘`*Info Region*`’ buffer; the cursor
will be in a position that is after the location of the error. Also,
the text will not be formatted after the place where the error
occurred (or more precisely, where it was detected).

For example, if you accidentally end a menu with the command `@end
menus` with an ‘s’ on the end, instead of with `@end menu`, you
will see an error message that says:

```
@end menus is not handled by texinfo
```

The cursor will stop at the point in the buffer where the error
occurs, or not long after it. The buffer will look like this:

```
---------- Buffer: *Info Region* ----------
* Menu:

* Using texinfo-show-structure::  How to use
                                 `texinfo-show-structure'
                                 to catch mistakes.
* Running Info-validate::         How to check for
                                 unreferenced nodes.
@end menus
∗
---------- Buffer: *Info Region* ----------
```

The `texinfo-format-region` command sometimes provides slightly
odd error messages. For example, the following cross-reference fails
to format:

```
(@xref{Catching Mistakes, for more info.)
```

In this case, `texinfo-format-region` detects the missing closing
brace but displays a message that says ‘`Unbalanced parentheses`’
rather than ‘`Unbalanced braces`’. This is because the formatting
command looks for mismatches between braces as if they were
parentheses.

Sometimes `texinfo-format-region` fails to detect mistakes. For
example, in the following, the closing brace is swapped with the
closing parenthesis:

```
(@xref{Catching Mistakes), for more info.}
```

Formatting produces:

```
(*Note for more info.: Catching Mistakes)
```

The only way for you to detect this error is to realize that the
reference should have looked like this:

```
(*Note Catching Mistakes::, for more info.)
```

Incidentally, if you are reading this node in Info and type `f
RET` (`Info-follow-reference`), you will generate an error
message that says:

```
No such node: "Catching Mistakes) The only way ...
```

This is because Info perceives the example of the error as the first
cross-reference in this node and if you type a `RET` immediately
after typing the Info `f` command, Info will attempt to go to the
referenced node. If you type `f catch TAB RET`, Info
will complete the node name of the correctly written example and take
you to the ‘Catching Mistakes’ node. (If you try this, you can return
from the ‘Catching Mistakes’ node by typing `l`
(`Info-last`).)

---

Next: [Finding Badly Referenced Nodes](#Running-Info_002dvalidate), Previous: [Catching Errors with Info Formatting](#Debugging-with-Info), Up: [Catching Mistakes](#Catching-Mistakes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.10.3 Debugging with TeX [¶](#Debugging-with-TeX-1)

You can also catch mistakes when you format a file with TeX.

Usually, you will want to do this after you have run
`texinfo-format-buffer` (or, better, `makeinfo-buffer`) on
the same file, because `texinfo-format-buffer` sometimes displays
error messages that make more sense than TeX. (See [Catching Errors with Info Formatting](#Debugging-with-Info), for more information.)

For example, TeX was run on a Texinfo file, part of which is shown
here:

```
---------- Buffer: texinfo.texi ----------
name of the Texinfo file as an extension.  The
@samp{??} are `wildcards' that cause the shell to
substitute all the raw index files.  (@xref{sorting
indices, for more information about sorting
indices.)
---------- Buffer: texinfo.texi ----------
```

(The cross-reference lacks a closing brace.)
TeX produced the following output, after which it stopped:

```
---------- Buffer: *tex-shell* ----------
Runaway argument?
{sorting indices, for more information about sorting
indices.) @ETC.
! Paragraph ended before @xref was complete.
<to be read again>
                  @par
l.27

?
---------- Buffer: *tex-shell* ----------
```

In this case, TeX produced an accurate and
understandable error message:

```
Paragraph ended before @xref was complete.
```

‘`@par`’ is an internal TeX command of no relevance to Texinfo.
‘`l.27`’ means that TeX detected the problem on line 27 of the
Texinfo file. The ‘`?`’ is the prompt TeX uses in this
circumstance.

Unfortunately, TeX is not always so helpful, and sometimes you must
truly be a Sherlock Holmes to discover what went wrong.

In any case, if you run into a problem like this, you can do one of three
things.

1. You can tell TeX to continue running and ignore just this error by
   typing `RET` at the ‘`?`’ prompt.
2. You can tell TeX to continue running and to ignore all errors as best
   it can by typing `r RET` at the ‘`?`’ prompt.

   This is often the best thing to do. However, beware: the one error
   may produce a cascade of additional error messages as its consequences
   are felt through the rest of the file. To stop TeX when it is
   producing such an avalanche of error messages, type `C-c` (or
   `C-c C-c`, if you are running a shell inside Emacs).
3. You can tell TeX to stop this run by typing `x RET`
   at the ‘`?`’ prompt.

If you are running TeX inside Emacs, you need to switch to the shell
buffer and line at which TeX offers the ‘`?`’ prompt.

Sometimes TeX will format a file without producing error messages even
though there is a problem. This usually occurs if a command is not ended
but TeX is able to continue processing anyhow. For example, if you fail
to end an itemized list with the `@end itemize` command, TeX will
write a DVI file that you can print out. The only error message that
TeX will give you is the somewhat mysterious comment:

```
(@end occurred inside a group at level 1)
```

However, if you print the DVI file, you will find that the text
of the file that follows the itemized list is entirely indented as if
it were part of the last item in the itemized list. The error message
is the way TeX says that it expected to find an `@end`
command somewhere in the file; but that it could not determine where
it was needed.

Another source of notoriously hard-to-find errors is a missing
`@end group` command. If you ever are stumped by
incomprehensible errors, look for a missing `@end group` command
first.

If the Texinfo file lacks header lines,
TeX may stop in the
beginning of its run and display output that looks like the following.
The ‘`*`’ indicates that TeX is waiting for input.

```
This is TeX, Version 3.14159 (Web2c 7.0)
(test.texinfo [1])
*
```

In this case, simply type `\end RET` after the asterisk. Then
write the header lines in the Texinfo file and run the TeX command
again. (Note the use of the backslash, ‘`\`’. TeX uses ‘`\`’
instead of ‘`@`’; and in this circumstance, you are working
directly with TeX, not with Texinfo.)

---

Previous: [Debugging with TeX](#Debugging-with-TeX), Up: [Catching Mistakes](#Catching-Mistakes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.10.4 Finding Badly Referenced Nodes [¶](#Finding-Badly-Referenced-Nodes)

You can use the `Info-validate` command to check whether any of
the ‘Next’, ‘Previous’, ‘Up’ or other node pointers fail to point to a
node. This command checks that every node pointer points to an
existing node. The `Info-validate` command works only on Info
files, not on Texinfo files.

The `texi2any` program validates pointers automatically, so you
do not need to use the `Info-validate` command if you are using
`texi2any`. With the customization variable
`CHECK_NORMAL_MENU_STRUCTURE` set, `texi2any` will also
warn if the nodes pointers (either explicitly or automatically set)
are not consistent with the order of node menu entries.
`texi2any` does not check that every ‘Next’ pointer is matched
by a ‘Previous’ (in the node where the ‘Next’ points) which points back,
since it may be correct for a non standard document structure.

You only may need to use `Info-validate` if you
are unable to run `texi2any` and instead must create an Info file
using `texinfo-format-region` or `texinfo-format-buffer`, or
if you write an Info file from scratch.

* [Using `Info-validate`](#Using-Info_002dvalidate)
* [Creating an Unsplit File and Adding a Tag Table](#Unsplit-and-Tagify)

---

Next: [Creating an Unsplit File and Adding a Tag Table](#Unsplit-and-Tagify), Up: [Finding Badly Referenced Nodes](#Running-Info_002dvalidate)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.10.4.1 Using `Info-validate` [¶](#Using-Info_002dvalidate-1)

To use `Info-validate`, visit the Info file you wish to check and
type:

```
M-x Info-validate
```

Note that the `Info-validate` command requires an uppercase
‘I’. You may also need to create a tag table before running
`Info-validate`. See [Tagifying a File](#Tagifying).

If your file is valid, you will receive a message that says “File appears
valid”. However, if you have a pointer that does not point to a node,
error messages will be displayed in a buffer called ‘`*problems in
info file*`’.

For example, `Info-validate` was run on a test file that contained
only the first node of this manual. One of the messages said:

```
In node "Overview", invalid Next: Texinfo Mode
```

This meant that the node called ‘`Overview`’ had a ‘Next’ pointer that
did not point to anything (which was true in this case, since the test file
had only one node in it).

Now suppose we add a node named ‘`Texinfo Mode`’ to our test case
but we do not specify a ‘Previous’ for this node. Then we will get
the following error message:

```
In node "Texinfo Mode", should have Previous: Overview
```

This is because, with a standard document structure, every ‘Next’ pointer
should be matched by a ‘Previous’ (in the node where the ‘Next’ points)
which points back.

`Info-validate` also checks that all menu entries and cross-references
point to actual nodes.

`Info-validate` requires a tag table and does not work with files
that have been split. (The `texinfo-format-buffer` command
automatically splits large files.) In order to use `Info-validate`
on a large file, you must run `texinfo-format-buffer` with an
argument so that it does not split the Info file; and you must create a
tag table for the unsplit file.

---

Previous: [Using `Info-validate`](#Using-Info_002dvalidate), Up: [Finding Badly Referenced Nodes](#Running-Info_002dvalidate)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### D.10.4.2 Creating an Unsplit File and Adding a Tag Table [¶](#Creating-an-Unsplit-File-and-Adding-a-Tag-Table)

You can run `Info-validate` only on a single Info file that has a
tag table. The command will not work on the indirect subfiles that
are generated when a master file is split. If you have a large file
(longer than 300,000 bytes or so), you need to run the
`texinfo-format-buffer` or `makeinfo-buffer` command in such
a way that it does not create indirect subfiles. You will also need
to create a tag table for the Info file. After you have done this,
you can run `Info-validate` and look for badly referenced
nodes.

The first step is to create an unsplit Info file. To prevent
`texinfo-format-buffer` from splitting a Texinfo file into
smaller Info files, give a prefix to the `M-x
texinfo-format-buffer` command:

```
C-u M-x texinfo-format-buffer
```

or else

```
C-u C-c C-e C-b
```

When you do this, `texinfo-format-buffer` will not split the file and will
not create a tag table for it.

After creating an unsplit Info file, you must create a tag table for
it (see [Tagifying a File](#Tagifying)). Visit the unsplit Info file created by
`texinfo-format-buffer` and type:

```
M-x Info-tagify
```

The third step is to validate the Info file:

```
M-x Info-validate
```

(Note the uppercase ‘`I`’ in `Info-validate`.)
In brief, the steps are:

```
C-u M-x texinfo-format-buffer
M-x Info-tagify
M-x Info-validate
```

After you have validated the node structure, you can rerun
`texinfo-format-buffer` in the normal way so it will construct a
tag table and split the file automatically, or you can make the tag
table and split the file manually (see [Splitting a File Manually](#Splitting)).

---

Previous: [Catching Mistakes](#Catching-Mistakes), Up: [Using Texinfo Mode](#Texinfo-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### D.11 Batch Formatting [¶](#Batch-Formatting-1)

You can format Texinfo files for Info using `batch-texinfo-format`
and Emacs batch mode. You can run Emacs in batch mode from any shell,
including a shell inside of Emacs. (See [Initial Options](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Initial-Options) in The GNU Emacs Manual.)

Here is a shell command to format all the files that end in
`.texinfo` in the current directory:

```
emacs -batch -funcall batch-texinfo-format *.texinfo
```

Emacs processes all the files listed on the command line, even if an
error occurs while attempting to format some of them.

Run `batch-texinfo-format` only with Emacs in batch mode as shown;
it is not interactive. It kills the batch mode Emacs on completion.

`batch-texinfo-format` is convenient if you lack `texi2any`
and want to format several Texinfo files at once. When you use Batch
mode, you create a new Emacs process. This frees your current Emacs, so
you can continue working in it. (When you run
`texinfo-format-region` or `texinfo-format-buffer`, you cannot
use that Emacs for anything else until the command finishes.)

---

Next: [Info Format Specification](#Info-Format-Specification), Previous: [Using Texinfo Mode](#Texinfo-Mode), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## Appendix E Global Document Commands [¶](#Global-Document-Commands-1)

Here are additional commands which affect the document as a whole. Most
of these commands are for customizing the appearance of the printed output.
They are generally all given before the Top node, if they are given at all.

* [`@setchapternewpage`: Blank Pages Before Chapters](#g_t_0040setchapternewpage)
* [Page Headings](#Headings)
* [`@paragraphindent`: Controlling Paragraph Indentation](#g_t_0040paragraphindent)
* [`@firstparagraphindent`: Indenting After Headings](#g_t_0040firstparagraphindent)
* [`@exampleindent`: Environment Indenting](#g_t_0040exampleindent)
* [`@smallbook`: Printing “Small” Books](#g_t_0040smallbook)
* [Printing on A4 Paper](#A4-Paper)
* [`@pagesizes` [width][, height]: Custom Page Sizes](#g_t_0040pagesizes)
* [Microtypography](#Microtypography)
* [Magnification](#Magnification)

---

Next: [Page Headings](#Headings), Up: [Global Document Commands](#Global-Document-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### E.1 `@setchapternewpage`: Blank Pages Before Chapters [¶](#g_t_0040setchapternewpage_003a-Blank-Pages-Before-Chapters)

In an officially bound book, text is usually printed on both sides of
the paper, chapters start on right-hand pages, and right-hand pages have
odd numbers. But in short reports, text often is printed only on one
side of the paper. Also in short reports, chapters sometimes do not
start on new pages, but are printed on the same page as the end of the
preceding chapter, after a small amount of vertical whitespace.

You can use the `@setchapternewpage` command with various
arguments to specify how chapters should be started in printed output
and whether headers should be formatted for printing on one or both sides of
the paper (single-sided or double-sided printing).

Write the `@setchapternewpage` command at the beginning of a
line followed by its argument. For example, you would write the
following to cause each chapter to start on a fresh odd-numbered page:

```
@setchapternewpage odd
```

You can specify one of three alternatives with the
`@setchapternewpage` command:

`@setchapternewpage off`
:   Typeset a new chapter on the same page as the last chapter, after
    skipping some vertical whitespace. Also, format page headers for
    single-sided printing.

`@setchapternewpage on`
:   Start new chapters on new pages and format page headers for
    single-sided printing. This is the form most often used for short
    reports or personal printing. This is the default.

`@setchapternewpage odd`
:   Start new chapters on new, odd-numbered pages (right-handed pages)
    and typeset for double-sided printing. This is the form most often
    used for books and manuals.

Texinfo does not have a `@setchapternewpage even` command,
because there is no printing tradition of starting chapters or books on
an even-numbered page.

If you don’t like the default headers that `@setchapternewpage`
sets, you can explicit control them with the `@headings` command.
See [The `@headings` Command](#g_t_0040headings).

At the beginning of a manual or book, pages are not numbered—for
example, the title and copyright pages of a book are not numbered. By
convention, table of contents and frontmatter pages are numbered with
roman numerals and not in sequence with the rest of the document.

The `@setchapternewpage` has no effect in output formats that do
not have pages, such as Info and HTML.

We recommend not including any `@setchapternewpage` command in
your document source at all, since such desired pagination is not
intrinsic to the document. For a particular hard copy run, if you
don’t want the default output (no blank pages, same headers on all
pages) use the `--texinfo` option to `texi2dvi` to
specify the output you want.

---

Next: [`@paragraphindent`: Controlling Paragraph Indentation](#g_t_0040paragraphindent), Previous: [`@setchapternewpage`: Blank Pages Before Chapters](#g_t_0040setchapternewpage), Up: [Global Document Commands](#Global-Document-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]
