### 20.1 Installing an Info File [¶](#Installing-an-Info-File-1)

Info files are usually kept in the `info` directory. You can
read Info files using the standalone Info program or the Info reader
built into Emacs. (See [Info](https://www.gnu.org/software/emacs/manual/html_mono/info.html#Top), for an introduction to
Info.)

* [The Directory File `dir`](#Directory-File)
* [Listing a New Info File](#New-Info-File)
* [Info Files in Other Directories](#Other-Info-Directories)
* [Installing Info Directory Files](#Installing-Dir-Entries)
* [Invoking `install-info`](#Invoking-install_002dinfo)

---

Next: [Listing a New Info File](#New-Info-File), Up: [Installing an Info File](#Installing-an-Info-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 20.1.1 The Directory File `dir` [¶](#The-Directory-File-dir)

For Info to work, the `info` directory must contain a file that
serves as a top-level directory for the Info system. By convention,
this file is called `dir`. (You can find the location of this file
within Emacs by typing `C-h i` to enter Info and then typing
`C-x C-f` to see the location of the `info` directory.)

The `dir` file is itself an Info file. It contains the top-level
menu for all the Info files in the system. The menu looks like
this:

```
* Menu:
* Info:    (info).     Documentation browsing system.
* Emacs:   (emacs).    The extensible, self-documenting
                      text editor.
* Texinfo: (texinfo).  With one source file, make
                      either a printed manual using
                      @TeX{} or an Info file.
...
```

Each of these menu entries points to the ‘Top’ node of the Info file
that is named in parentheses. (The menu entry does not need to
specify the ‘Top’ node, since Info goes to the ‘Top’ node if no node
name is mentioned. See [Nodes in Other Info
Files](#Other-Info-Files).)

Thus, the ‘`Info`’ entry points to the ‘Top’ node of the
`info` file and the ‘`Emacs`’ entry points to the ‘Top’ node
of the `emacs` file.

In each of the Info files, the ‘Up’ pointer of the ‘Top’ node refers
back to the `dir` file. For example, the line for the ‘Top’
node of the Emacs manual looks like this in Info:

```
File: emacs  Node: Top, Up: (DIR), Next: Distrib
```

In this case, the `dir` file name is written in uppercase
letters—it can be written in either upper- or lowercase. This is not
true in general, it is a special case for `dir`.

See the `util/dir-example` file in the Texinfo distribution for
a large sample `dir` file.

---

Next: [Info Files in Other Directories](#Other-Info-Directories), Previous: [The Directory File `dir`](#Directory-File), Up: [Installing an Info File](#Installing-an-Info-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 20.1.2 Listing a New Info File [¶](#Listing-a-New-Info-File)

To add a new Info file to your system, you must write a menu entry to
add to the menu in the `dir` file in the `info` directory.
For example, if you were adding documentation for GDB, you would write
the following new entry:

```
* GDB: (gdb).           The source-level C debugger.
```

The first part of the menu entry is the menu entry name, followed by a
colon. The second part is the name of the Info file, in parentheses,
followed by a period. The third part is the description.

The name of an Info file often has a `.info` extension. Thus, the
Info file for GDB might be called either `gdb` or `gdb.info`.
The Info reader programs automatically try the file name both with and
without `.info`[10](#FOOT10); so it is better to avoid
clutter and not to write ‘`.info`’ explicitly in the menu entry. For
example, the GDB menu entry should use just ‘`gdb`’ for the file
name, not ‘`gdb.info`’.

---

Next: [Installing Info Directory Files](#Installing-Dir-Entries), Previous: [Listing a New Info File](#New-Info-File), Up: [Installing an Info File](#Installing-an-Info-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 20.1.3 Info Files in Other Directories [¶](#Info-Files-in-Other-Directories)

If an Info file is not in the `info` directory, there are three
ways to specify its location:

1. Write the file name in the `dir` file as the second part of the menu.
2. Specify the Info directory name in the `INFOPATH` environment
   variable in your `.profile` or `.cshrc` initialization file.
   (Only you and others who set this environment variable will be able to
   find Info files whose location is specified this way.)
3. If you are using Emacs, list the name of the file in a second `dir`
   file, in its directory; and then add the name of that directory to the
   `Info-directory-list` variable in your personal or site
   initialization file.

   This variable tells Emacs where to look for `dir` files (the files
   must be named `dir`). Emacs merges the files named `dir` from
   each of the listed directories. (In Emacs version 18, you can set the
   `Info-directory` variable to the name of only one
   directory.)

For example, to reach a test file in the `/home/bob/info`
directory, you could add an entry like this to the menu in the
standard `dir` file:

```
* Test: (/home/bob/info/info-test).  Bob's own test file.
```

In this case, the absolute file name of the `info-test` file is
written as the second part of the menu entry.

If you don’t want to edit the system `dir` file, you can tell
Info where to look by setting the `INFOPATH` environment variable
in your shell startup file. This works with both the Emacs and
standalone Info readers.

Emacs uses the `INFOPATH` environment variable to initialize the value of
Emacs’s own `Info-directory-list` variable. The standalone Info reader
merges any files named `dir` in any directory listed in the
`INFOPATH` variable into a single menu presented to you in the node
called ‘`(dir)Top`’.

However you set `INFOPATH`, if its last character is a colon (on
MS-DOS/MS-Windows systems, use a semicolon instead), this is replaced
by the default (compiled-in) path. This gives you a way to augment
the default path with new directories without having to list all the
standard places. For example (using `sh` syntax):

```
INFOPATH=/home/bob/info:
export INFOPATH
```

will search `/home/bob/info` first, then the standard directories.
Leading or doubled colons are not treated specially.

When you create your own `dir` file for use with
`Info-directory-list` or `INFOPATH`, it’s easiest to start by
copying an existing `dir` file and replace all the text after the
‘`* Menu:`’ with your desired entries. That way, the punctuation
and special `CTRL-_` characters that Info needs will be present.

As one final alternative, which works only with Emacs Info, you can
change the `Info-directory-list` variable. For example:

```
(add-hook 'Info-mode-hook '(lambda ()
	     (add-to-list 'Info-directory-list
			  (expand-file-name "~/info"))))
```

---

Next: [Invoking `install-info`](#Invoking-install_002dinfo), Previous: [Info Files in Other Directories](#Other-Info-Directories), Up: [Installing an Info File](#Installing-an-Info-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 20.1.4 Installing Info Directory Files [¶](#Installing-Info-Directory-Files)

When you install an Info file onto your system, you can use the program
`install-info` to update the Info directory file `dir`.
Normally the makefile for the package runs `install-info`, just
after copying the Info file into its proper installed location.

In order for the Info file to work with `install-info`, you include
the commands `@dircategory` and
`@direntry`…`@end direntry` in the Texinfo source
file. Use `@direntry` to specify the menu entries to add to the
Info directory file. Use `@dircategory` to specify a category
for the manual, which determines which part of the Info directory to
put it in. See [Directory Category](#Directory-Category).

Here is how these commands are used in this manual:

```
@dircategory Texinfo documentation system
@direntry
* Texinfo: (texinfo).           The GNU documentation format.
* install-info: (texinfo)Invoking install-info. ...
...
@end direntry
```

Here’s what this produces in the Info file:

```
INFO-DIR-SECTION Texinfo documentation system
START-INFO-DIR-ENTRY
* Texinfo: (texinfo).           The GNU documentation format.
* install-info: (texinfo)Invoking install-info. ...
...
END-INFO-DIR-ENTRY
```

The `install-info` program sees these lines in the Info file, and
that is how it knows what to do.

Always use the `@direntry` and `@dircategory` commands near
the beginning of the Texinfo input, before the first `@node`
command. If you use them later on in the input, `install-info`
will not notice them.

`install-info` will automatically reformat the description of the
menu entries it is adding. As a matter of convention, the description
of the main entry (above, ‘`The GNU documentation format`’) should
start at column 32, starting at zero (as in
`what-cursor-position` in Emacs). This will make it align with
most others. Description for individual utilities best start in
column 48, where possible. For more information about formatting see
the ‘`--calign`’, ‘`--align`’, and ‘`--max-width`’ options in
[Invoking `install-info`](#Invoking-install_002dinfo).

If you use `@dircategory` more than once in the Texinfo source,
each usage specifies the ‘current’ category; any subsequent
`@direntry` commands will add to that category.

Each ‘Invoking’ node for every program installed should have a
corresponding `@direntry`. This lets users easily find the
documentation for the different programs they can run, as with the
traditional `man` system.

---

Previous: [Installing Info Directory Files](#Installing-Dir-Entries), Up: [Installing an Info File](#Installing-an-Info-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 20.1.5 Invoking `install-info` [¶](#Invoking-install_002dinfo-1)

`install-info` inserts menu entries from an Info file into the
top-level `dir` file in the Info system (see the previous sections
for an explanation of how the `dir` file works). `install-info`
also removes menu entries from the `dir` file. It’s most often
run as part of software installation, or when constructing a `dir` file
for all manuals on a system. Synopsis:

```
install-info [option...] [info-file [dir-file]]
```

If info-file or dir-file are not specified, the options
(described below) that define them must be. There are no compile-time
defaults, and standard input is never used. `install-info` can
read only one Info file and write only one `dir` file per invocation.

If dir-file (however specified) does not exist,
`install-info` creates it if possible (with no entries).

If any input file is compressed with `gzip` (see [Gzip](https://www.gnu.org/software/gzip/manual/gzip.html#Top)), `install-info` automatically uncompresses it for reading.
And if dir-file is compressed, `install-info` also
automatically leaves it compressed after writing any changes. If
dir-file itself does not exist, `install-info` tries to
open `dir-file.gz`, `dir-file.xz`,
`dir-file.bz2`, `dir-file.lz`, and
`dir-file.lzma`, in that order.

Options:

`--add-once` [¶](#index-_002d_002dadd_002donce_002c-for-install_002dinfo)
:   Specifies that the entry or entries will only be put into a single section.

`--align=column` [¶](#index-_002d_002dalign_003dcolumn_002c-for-install_002dinfo)
:   Specifies the column that the second and subsequent lines of menu entry’s
    description will be formatted to begin at. The default for this option is
    ‘`35`’. It is used in conjunction with the ‘`--max-width`’ option.
    column starts counting at 1.

`--append-new-sections` [¶](#index-_002d_002dappend_002dnew_002dsections_002c-for-install_002dinfo)
:   Instead of alphabetizing new sections, place them at the end of the DIR file.

`--calign=column` [¶](#index-_002d_002dcalign_003dcolumn_002c-for-install_002dinfo)
:   Specifies the column that the first line of menu entry’s description will
    be formatted to begin at. The default for this option is ‘`33`’. It is
    used in conjunction with the ‘`--max-width`’ option.
    When the name of the menu entry exceeds this column, entry’s description
    will start on the following line.
    column starts counting at 1.

`--debug` [¶](#index-_002d_002ddebug_002c-for-install_002dinfo)
:   Report what is being done.

`--delete` [¶](#index-_002d_002ddelete_002c-for-install_002dinfo)
:   Delete the entries in info-file from dir-file. The file
    name in the entry in dir-file must be info-file (except for
    an optional ‘`.info`’ in either one). Don’t insert any new entries.
    Any empty sections that result from the removal are also removed.

`--description=text` [¶](#index-_002d_002ddescription_003dtext_002c-for-install_002dinfo)
:   Specify the explanatory portion of the menu entry. If you don’t specify
    a description (either via ‘`--entry`’, ‘`--item`’ or this option),
    the description is taken from the Info file itself.

`--dir-file=name` [¶](#index-_002d_002ddir_002dfile_003dname_002c-for-install_002dinfo)
:   Specify file name of the Info directory file. This is equivalent to
    using the dir-file argument.

`--dry-run` [¶](#index-_002d_002ddry_002drun_002c-for-install_002dinfo)
:   Same as ‘`--test`’.

`--entry=text` [¶](#index-_002d_002dentry_003dtext_002c-for-install_002dinfo)
:   Insert text as an Info directory entry; text should have the
    form of an Info menu item line plus zero or more extra lines starting
    with whitespace. If you specify more than one entry, they are all
    added. If you don’t specify any entries, they are determined from
    information in the Info file itself.

`--help` [¶](#index-_002d_002dhelp_002c-for-texindex)
:   Display a usage message with basic usage and all available options,
    then exit successfully.

`--info-file=file` [¶](#index-_002d_002dinfo_002dfile_003dfile_002c-for-install_002dinfo)
:   Specify Info file to install in the directory. This is
    equivalent to using the info-file argument.

`--info-dir=dir` [¶](#index-_002d_002dinfo_002ddir_003ddir_002c-for-install_002dinfo)
:   Specify the directory where the directory file `dir` resides.
    Equivalent to ‘`--dir-file=dir/dir`’.

`--infodir=dir` [¶](#index-_002d_002dinfodir_003ddir_002c-for-install_002dinfo)
:   Same as ‘`--info-dir`’.

`--item=text` [¶](#index-_002d_002ditem_003dtext_002c-for-install_002dinfo)
:   Same as ‘`--entry=text`’. An Info directory entry is actually
    a menu item.

`--keep-old` [¶](#index-_002d_002dkeep_002dold_002c-for-install_002dinfo)
:   Do not replace pre-existing menu entries. When ‘`--remove`’ is specified,
    this option means that empty sections are not removed.

`--max-width=column` [¶](#index-_002d_002dmax_002dwidth_003dcolumn_002c-for-install_002dinfo)
:   Specifies the column that the menu entry’s description will be word-wrapped
    at. column starts counting at 1.

`--maxwidth=column` [¶](#index-_002d_002dmaxwidth_003dcolumn_002c-for-install_002dinfo)
:   Same as ‘`--max-width`’.

`--menuentry=text` [¶](#index-_002d_002dmenuentry_003dtext_002c-for-install_002dinfo)
:   Same as ‘`--name`’.

`--name=text` [¶](#index-_002d_002dname_003dtext_002c-for-install_002dinfo)
:   Specify the name portion of the menu entry. If the text does
    not start with an asterisk ‘`*`’, it is presumed to be the text
    after the ‘`*`’ and before the parentheses that specify the Info
    file. Otherwise text is taken verbatim, and is taken as
    defining the text up to and including the first period (a space is
    appended if necessary). If you don’t specify the name (either via
    ‘`--entry`’, ‘`--item`’ or this option), it is taken from the
    Info file itself. If the Info does not contain the name, the basename
    of the Info file is used.

`--no-indent` [¶](#index-_002d_002dno_002dindent_002c-for-install_002dinfo)
:   Suppress formatting of new entries into the `dir` file.

`--quiet` [¶](#index-_002d_002dquiet_002c-for-install_002dinfo)

`--silent`
:   Suppress warnings, etc., for silent operation.

`--remove` [¶](#index-_002d_002dremove_002c-for-install_002dinfo)
:   Same as ‘`--delete`’.

`--remove-exactly` [¶](#index-_002d_002dremove_002dexactly_002c-for-install_002dinfo)
:   Also like ‘`--delete`’, but only entries if the Info file name
    matches exactly; `.info` and/or `.gz` suffixes are
    *not* ignored.

`--section=sec` [¶](#index-_002d_002dsection_003dsec_002c-for-install_002dinfo)
:   Put this file’s entries in section sec of the directory. If you
    specify more than one section, all the entries are added in each of the
    sections. If you don’t specify any sections, they are determined from
    information in the Info file itself. If the Info file doesn’t specify
    a section, the menu entries are put into the Miscellaneous section.

`--section regex sec` [¶](#index-_002d_002dsection-regex-sec_002c-for-install_002dinfo)
:   Same as ‘`--regex=regex --section=sec --add-once`’.

    `install-info` tries to detect when this alternate syntax is used,
    but does not always guess correctly. Here is the heuristic that
    `install-info` uses:

    1. If the second argument to `--section` starts with a hyphen, the
       original syntax is presumed.
    2. If the second argument to `--section` is a file that can be
       opened, the original syntax is presumed.
    3. Otherwise the alternate syntax is used.

    When the heuristic fails because your section title starts with a
    hyphen, or it happens to be a file that can be opened, the syntax
    should be changed to ‘`--regex=regex --section=sec
    --add-once`’.

`--regex=regex` [¶](#index-_002d_002dregex_003dregex_002c-for-install_002dinfo)
:   Put this file’s entries into any section that matches regex. If
    more than one section matches, all of the entries are added in each of the
    sections. Specify regex using basic regular expression syntax, more
    or less as used with `grep`, for example.

`--test` [¶](#index-_002d_002dtest_002c-for-install_002dinfo)
:   Suppress updating of the directory file.

`--version` [¶](#index-_002d_002dversion_002c-for-install_002dinfo)
:   Display version information and exit successfully.

---

Next: [Info Format FAQ](#Info-Format-FAQ), Previous: [Installing an Info File](#Installing-an-Info-File), Up: [Creating and Installing Info Files](#Creating-and-Installing-Info-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 20.2 Tag Files and Split Files [¶](#Tag-Files-and-Split-Files)

Info files always contain a *tag table*, to be able to jump to
nodes quickly. Info files can be *nonsplit* (also called
*unsplit*) or *split*.

If the Info file contains less than about 300,000 characters the
file should be nonsplit. In that case, the tag table should
appear at the end of the Info file. If the Texinfo file contains
more than about 300,000 characters, Texinfo processors split the
large Info file into shorter *indirect* subfiles of about
300,000 characters each. With `texi2any`, splitting
may be prevented by `--no-split`, and the default size
of 300,000 characters may be modified with `--split-size`
(see [Invoking `texi2any` from a Shell](#Invoking-texi2any)).

When a file is split, Info itself makes use of a shortened version of
the original file that contains just the tag table and references to
the files that were split off. The split-off files are called
*indirect* files.

The split-off files have names that are created by appending ‘`-1`’,
‘`-2`’, ‘`-3`’ and so on to the output file name, specified
by the `@setfilename` command or the input file name. The shortened
version of the original file continues to have the name specified by
`@setfilename` or the input file name.

At one stage in writing this document, for example, the Info file was saved
as the file `test-texinfo` and that file looked like this:

```
Info file: test-texinfo,    -*-Text-*-
produced by texinfo-format-buffer
from file: new-texinfo-manual.texinfo

^_
Indirect:
test-texinfo-1: 102
test-texinfo-2: 50422
```

```
test-texinfo-3: 101300
^_^L
Tag table:
(Indirect)
Node: overview^?104
Node: info file^?1271
```

```
Node: printed manual^?4853
Node: conventions^?6855
...
```

(But `test-texinfo` had far more nodes than are shown here.) Each of
the split-off, indirect files, `test-texinfo-1`,
`test-texinfo-2`, and `test-texinfo-3`, is listed in this file
after the line that says ‘`Indirect:`’. The tag table is listed after
the line that says ‘`Tag table:`’.

In the list of indirect files, the number following the file name
records the cumulative number of bytes in the preceding indirect
files, not counting the file list itself, the tag table, or any
permissions text in the first file. In the tag table, the number
following the node name records the location of the beginning of the
node, in bytes from the beginning of the (unsplit) output.

If you are using `texinfo-format-buffer` to create Info files,
you may want to run the `Info-validate` command. (The
`texi2any` command does such a good job on its own, you do not
need `Info-validate`.) However, you cannot run the `M-x
Info-validate` node-checking command on indirect files. For
information on how to prevent files from being split with
`texinfo-format-buffer` and how to validate the structure of the nodes,
see [Using `Info-validate`](#Using-Info_002dvalidate).

---

Previous: [Tag Files and Split Files](#Tag-and-Split-Files), Up: [Creating and Installing Info Files](#Creating-and-Installing-Info-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 20.3 Info Format FAQ [¶](#Info-Format-FAQ-1)

Here are some questions that have been frequently asked on
the project mailing lists and elsewhere.

Why when I run ‘`info foo`’ do I get the same output as ‘`man foo`’?
:   Check that the Info manuals are installed. Not all GNU/Linux
    distributions install all the Info manuals by default. This is
    regrettable, as often the Info manual provides more information than
    the provided man page.

Why not use HTML instead of Info?
:   Manuals are rarely written in the Info format itself, but are
    generated from Texinfo source by the `texi2any` program.
    `texi2any` can generate HTML as well as Info from Texinfo
    source. You can also access and download HTML manuals from the GNU
    website (<https://www.gnu.org/manual/manual.html>). Additionally,
    some GNU/Linux distributions provide packages for the installation
    of HTML manuals.

    Info still has some advantages over HTML for locally installed
    documentation. The Info browsers support index lookup and support
    for links between locally installed manuals in multiple locations.
    It’s important to have documentation installed locally on your machine
    rather than relying on the Internet; this makes accessing documentation
    more reliable, and ensures it matches installed versions of packages.
    It’s hoped that support for browsing locally installed Texinfo
    documentation in some form of HTML can be improved in the future.

Why provide the command-line `info` program when the Emacs Info reader is better?
:   The Emacs Info reader can display images, and fully supports using
    a mouse. There are not many differences among the Info readers
    besides that. Command-line `info` can be configured
    to change the default key bindings, as well as change the color
    of cross-references and search results, and to enable mouse
    scrolling support. You should not need to be experienced with
    the Emacs editor to use `info`. (Some recommend another
    program called `pinfo`, but this lacks in important
    features like index lookup.)

    Some prefer to be able to scroll through the entire manual at once (as
    happens with man pages), rather than being limited to a single ‘node’
    of content at once. Of course, there is no accounting for taste,
    but a single, unstructured block of text becomes more awkward as a
    manual becomes longer and more complicated. (However, if you really
    want to, you can view an info manual all at once in the `less`
    pager with ‘`info foo | less`’.)

Why do I have ‘see’ appearing in front of all of my links?
:   By default, Emacs Info mode either changes the marker ‘`*note`’ for
    cross-references to ‘see’, or hides it completely. Command-line
    `info` does the same if `hide-note-references` is set.
    Unfortunately, there is no way to do this reliably, as both the `@pxref`
    and `@ref` commands in Texinfo output this marker in the Info
    output, and the ‘see’ text is only appropriate for `@pxref`.

Yes, but how do I get a plain link, with no extra markup?
:   You can’t. Info is a plain text format that is displayed mostly as-is
    in the viewers, and without the ‘`*note`’ text there would be nothing
    to mark text as a link.

    For output formats such as HTML, you can use the `@link` command
    to produce a plain link. See [`@link`: Plain, unadorned hyperlink](#g_t_0040link). This does not produce
    a working cross-reference in Info output or in a printed copy of the
    manual, though.

Why do lines containing links appear mysteriously short?
:   This due to Emacs (or `info` with `hide-note-references`
    set to ‘`On`’) hiding ‘`*note`’ strings, as mentioned above.

Can the Info format be extended to support fonts, colors or reflowable text?
:   Any extension would be incompatible with existing Info-viewing programs.
    Extra markup added to Info files would be displayed to the user, making
    files ugly and unreadable.

    When Texinfo files are processed into Info files, information about
    which Texinfo commands were used to markup text is lost. Moreover,
    it is not possible to reliably detect whether text can be reflowed
    (e.g. a paragraph of prose), or whether line breaks should be kept
    (e.g. a code sample, or poem).

    Info’s core purpose is to display documentation on text terminals.
    If you want more, you are recommended to use the HTML output from
    `texi2any` instead.

---

Next: [@-Command Details](#g_t_0040_002dCommand-Details), Previous: [Creating and Installing Info Files](#Creating-and-Installing-Info-Files), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 21 Generating HTML [¶](#Generating-HTML-1)

`texi2any` generates Info output by default, but given the
`--html` option, it will generate HTML, for web browsers and
other programs. This chapter gives some details on such HTML output.

`texi2any` has many user-definable customization variables
with which you can influence the HTML output. See [Customization Variables](#Customization-Variables). In particular, there is support for syntax highlighting in
`@example` (see [Code Examples Syntax Highlighting in HTML](#Syntax-Highlighting)). You can also write so-called
*initialization files*, or *init files* for short, to modify almost
every aspect of HTML output formatting. Initialization files contain code and
are loaded by `--init-file` (see [Invoking `texi2any` from a Shell](#Invoking-texi2any)).

Some initialization files are maintained with Texinfo and installed
in the default case. For example, `chm.pm` produces the intermediate
compressed HTML Help format files that can be subsequently converted to
the CHM format.

The documentation of `texi2any` HTML output adaptation using
customization files is in a separate manual. See [GNU
Texinfo `texi2any` Output Customization](https://www.gnu.org/software/texinfo/manual/texi2any_api/texi2any_api.html#Top).

* [HTML Translation](#HTML-Translation)
* [HTML Splitting](#HTML-Splitting)
* [HTML CSS](#HTML-CSS)
* [`@documentdescription`: Summary Text](#g_t_0040documentdescription)
* [Generating EPUB](#Generating-EPUB)
* [Code Examples Syntax Highlighting in HTML](#Syntax-Highlighting)
* [HTML Cross-references](#HTML-Xref)

---

Next: [HTML Splitting](#HTML-Splitting), Up: [Generating HTML](#Generating-HTML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 21.1 HTML Translation [¶](#HTML-Translation-1)

The HTML generated by `texi2any` generates standard HTML
output. The output is intentionally quite plain for maximum portability
and accessibility.

You can customize the output via CSS (see [HTML CSS](#HTML-CSS)) or other
means (see [Customization Variables](#Customization-Variables)). If you cannot accomplish
a reasonable customization, feel free to report that.

**Navigation bar:** By default, a navigation bar is inserted at the
start of each node, analogous to Info output. If the
‘`--no-headers`’ option is used, the navigation bar is only
inserted at the beginning of split files. Header `<link>` elements
in split output support Info-like navigation with browsers which implement
this feature.

**Raw HTML**: `texi2any` will include segments of Texinfo
source between `@ifhtml` and `@end ifhtml` in the HTML
output (but not any of the other conditionals, by default). Source
between `@html` and `@end html` is passed without change
to the output (i.e., suppressing the normal escaping of input
‘`<`’, ‘`>`’ and ‘`&`’ characters which have special
significance in HTML). See [Conditional Commands](#Conditional-Commands).

**Standards**:
It is intentionally not our goal, and not even always possible, to pass
through every conceivable validation test without any diagnostics.
Different validation tests have different goals, often about pedantic
enforcement of some standard or another. Our overriding goal is to
help users, not blindly comply with standards.

Please report output from an
error-free run of `texi2any` which has *practical* browser
or EPUB reader portability problems as a bug (see [Reporting Bugs](#Reporting-Bugs)).

In practice, the HTML produced by `texi2any` is slowly adjusted
over time towards the latest HTML standard, while also trying to keep
compatibility with earlier produced HTML. We use transitional markup
and try to be slow enough to give time for the diverse HTML readers
to adjust (and for standards to reincorporate useful features that were
dropped…).

---

Next: [HTML CSS](#HTML-CSS), Previous: [HTML Translation](#HTML-Translation), Up: [Generating HTML](#Generating-HTML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 21.2 HTML Splitting [¶](#HTML-Splitting-1)

When splitting output at nodes (which is the default),
`texi2any` writes HTML output into (basically) one output file
per Texinfo source `@node`.

Each output file name is the node name with spaces replaced by
‘`-`’’s and special characters changed to ‘`_`’ followed by
their code point in hex (see [HTML Cross-references](#HTML-Xref)). This is to make it
portable and easy to use as a file name. In the unusual case of two
different nodes having the same name after this treatment, they are
written consecutively to the same file, with HTML anchors so each can
be referred to independently.

If `texi2any` is run on a system which does not distinguish
case in file names, nodes which are the same except for case (e.g.,
‘`index`’ and ‘`Index`’) will also be folded into the same
output file with anchors. You can also pretend to be on a case
insensitive filesystem by setting the customization variable
`CASE_INSENSITIVE_FILENAMES`.

It is also possible to split at chapters or sections with
`--split` (see [Invoking `texi2any` from a Shell](#Invoking-texi2any)). In that case,
the file names are constructed after the name of the node associated
with the relevant sectioning command. Also, unless
`--no-node-files` is specified, a redirection file is output
for every node in order to more reliably support cross-references to
that manual (see [HTML Cross-references](#HTML-Xref)).

When splitting, the HTML output files are written into a subdirectory. The
subdirectory name is derived from the base name (that
is, any extension is removed), with `_html` postpended. For example, HTML
output for `gcc.texi` would be written into a subdirectory
named ‘`gcc_html/`’. The subdirectory name is based on `@setfilename`
or on the input file name (see [Setting the Output File Name](#Setting-the-Output-File-Name)).

In any case, the top-level output file within the directory
is always named ‘`index.html`’.

Monolithic output (`--no-split`) is named according to
`@setfilename`, if present (with any ‘`.info`’ extension replaced
with ‘`.html`’), `--output` (the argument is used literally), or
based on the input file name as a last resort
(see [Setting the Output File Name](#Setting-the-Output-File-Name)).

---

Next: [`@documentdescription`: Summary Text](#g_t_0040documentdescription), Previous: [HTML Splitting](#HTML-Splitting), Up: [Generating HTML](#Generating-HTML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 21.3 HTML CSS [¶](#HTML-CSS-1)

Cascading Style Sheets (CSS) is an Internet standard for
influencing the display of HTML documents: see
<http://www.w3.org/Style/CSS/>.

By default, some CSS code is included to better implement the
appearance of some Texinfo environments. For example:

```
pre.display { font-family:inherit }
```

The above tells the web browser to use the same font as the main
document inside ‘`<pre>`’ elements used for `@display` environments.
By default, the HTML ‘`<pre>`’ command uses a monospaced font.

Please see the reference above for a full explanation of CSS.

You can influence the CSS in the HTML output with two
`texi2any` options: `--css-include=file` and
`--css-ref=url`.

The option `--css-ref=url` adds to each output HTML file
a ‘`<link>`’ tag referencing a CSS at the given url. This
allows using external style sheets.

The option `--css-include=file` includes the contents
file in the HTML output, as you might expect. However, the
details are somewhat tricky, as described in the following, to provide
maximum flexibility.

The CSS file first line may be a ‘`@charset`’ directive. If present,
this directive is used to determine the encoding of the CSS file. The
line is not copied into the output.

The CSS file may begin with so-called ‘`@import`’ directives,
which link to external CSS specifications for browsers to use when
interpreting the document. Again, a full description is beyond our
scope here, but we’ll describe how they work syntactically, so we can
explain how they are handled.

There can be more than one ‘`@import`’, but they have to come
first in the file, with only whitespace and comments interspersed, no normal
definitions. Comments in CSS files are delimited by ‘`/* ... */`’, as in
C. An ‘`@import`’ directive must be in one of these two forms:

```
@import url(http://example.org/foo.css);
@import "http://example.net/bar.css";
```

The crucial characters are the ‘`@`’ at the beginning and
the semicolon terminating the directive. When reading the CSS
file, any such ‘`@`’-directive is simply copied into the
output, as follows:

* If file contains only normal CSS declarations, it is
  included after the default CSS, thus overriding it.
* If file begins with ‘`@import`’ specifications (see
  below), then the ‘`import`’’s are included first (they have to come
  first, according to the standard), and then the default CSS is
  included. If you need to override the default CSS from an
  ‘`@import`’, you can do so with the ‘`! important`’ CSS
  construct, as in:

  ```
  pre.example { font-size: inherit ! important }
  ```
* If file contains both ‘`@import`’ and inline CSS
  specifications, the ‘`@import`’’s are included first, then
  default CSS, and lastly the inline CSS from file.
* Any @-directive other than ‘`@import`’ and ‘`@charset`’
  is treated as a CSS declaration, meaning the default CSS is included
  and then the rest of the file is prepended.

If the CSS file is malformed or erroneous, the output
is unspecified. The meaning of the CSS file is not interpreted in
any way; the special ‘`@`’ and ‘`;`’ characters are looked for
the text is blindly copied into the output. Comments in the CSS
file may or may not be included in the output.

In addition to the possibilities offered by CSS, `texi2any`
has many user-definable customization variables with which you can
influence the HTML output. See [Customization Variables](#Customization-Variables).

---

Next: [Generating EPUB](#Generating-EPUB), Previous: [HTML CSS](#HTML-CSS), Up: [Generating HTML](#Generating-HTML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 21.4 `@documentdescription`: Summary Text [¶](#g_t_0040documentdescription_003a-Summary-Text)

When producing HTML output for a document, a ‘`<meta>`’ element
is written in the ‘`<head>`’ to give some idea of the
content of the document. By default, this *description* is the
title of the document, taken from the `@settitle` command
(see [`@settitle`: Set the Document Title](#g_t_0040settitle)). To change this, use the
`@documentdescription` environment, as in:

```
@documentdescription
descriptive text.
@end documentdescription
```

This will produce the following output in the ‘`<head>`’ of the HTML:

```
<meta name=description content="descriptive text.">
```

---

Next: [Code Examples Syntax Highlighting in HTML](#Syntax-Highlighting), Previous: [`@documentdescription`: Summary Text](#g_t_0040documentdescription), Up: [Generating HTML](#Generating-HTML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 21.5 Generating EPUB [¶](#Generating-EPUB-1)

EPUB is a format designed for reading electronic books on portable
devices. `texi2any` generated EPUB 3.2 in 2022. An EPUB
file is a ZIP archive container, holding informative files as
well as the manual rendered in HTML.

The generation of the EPUB file depends on the `Archive::Zip`
Perl module being installed. This dependency is checked at runtime.
In the default case, trying to output EPUB without this dependency raises an
error. However, if the EPUB output file is not generated, with the
customization variable `EPUB_CREATE_CONTAINER_FILE` set to 0, it is
not an error if `Archive::Zip` is not installed.

The `texi2any` tests related to EPUB generation do not require the
installation of `Archive::Zip`, as they set
`EPUB_CREATE_CONTAINER_FILE` to 0 and keep the directory containing
the files and directories needed for the EPUB file by setting the
`EPUB_KEEP_CONTAINER_FOLDER` customization variable to 1.

* [Container Directory and Output Files](#EPUB-Output-File-and-Directory)
* [EPUB Cross-References](#EPUB-Cross_002dReferences)
* [HTML Generated for EPUB](#EPUB-HTML)

---

Next: [EPUB Cross-References](#EPUB-Cross_002dReferences), Up: [Generating EPUB](#Generating-EPUB)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 21.5.1 Container Directory and Output Files [¶](#Container-Directory-and-Output-Files)

A directory containing the files and directories needed for the
EPUB format is created when outputting EPUB. The name of this
container directory is derived from the base name of the input file (that
is, any extension is removed), with `_epub_package` postpended.
If an output directory is specified, with `--output`,
or with the `SUBDIR` customization variable,
the container directory is created in that directory instead of
the current directory. At the beginning of a new run, the container
directory and all its contents are removed. The container directory
is also removed after the final EPUB file has been generated in the
default case.

The HTML files produced from the Texinfo manual are output in subdirectories
of the container directory. Image files referred to from the Texinfo manual,
if found, are copied to subdirectories of the container directory.

The EPUB output file is a ZIP archive of the container directory.
The file name is derived from the base name, with the `.epub`
extension postpended. If an output file is specified, with
`--output`, or with the `OUTFILE` customization function,
this file name is used instead. The output EPUB file is never placed
in the directory specified by `--output` or `SUBDIR`;
only the container directory is placed there, as explained just above.

The EPUB output file is not generated if the customization variable
`EPUB_CREATE_CONTAINER_FILE` is set to 0. The container directory
is left after the final EPUB file has been generated if
`EPUB_KEEP_CONTAINER_FOLDER` is set.

See [Invoking `texi2any` from a Shell](#Invoking-texi2any).

---

Next: [HTML Generated for EPUB](#EPUB-HTML), Previous: [Container Directory and Output Files](#EPUB-Output-File-and-Directory), Up: [Generating EPUB](#Generating-EPUB)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 21.5.2 EPUB Cross-References [¶](#EPUB-Cross_002dReferences-1)

The EPUB format does not support references from an EPUB file to
another EPUB file. Therefore any references to “external” Texinfo
manuals should resolve to an external URL. `texi2any`
produces these links with HTML cross-reference configuration
(see [HTML Cross-reference Configuration: `htmlxref.cnf`](#HTML-Xref-Configuration)). Since the links in the
resulting EPUB are incorrect if no information is found for the
cross-references, `texi2any` issues a warning by default for
missing cross-references information. If these warnings are unwanted,
set `CHECK_HTMLXREF` to 0.

---

Previous: [EPUB Cross-References](#EPUB-Cross_002dReferences), Up: [Generating EPUB](#Generating-EPUB)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 21.5.3 HTML Generated for EPUB [¶](#HTML-Generated-for-EPUB)

The HTML generated for EPUB is XHTML conformant, UTF-8 encoded, and
formatted without the usual HTML navigation headers and footers.
Most of these features are enabled with customization variables, such as
`USE_XML_SYNTAX` or `OUTPUT_FILE_NAME_ENCODING`. Some
features of printed output are used in EPUB. In particular, the Top
node does not appear in the EPUB output, while a title page is
generated. This is obtained by setting `NO_TOP_NODE_OUTPUT`.

The `OUTFILE` and `SUBDIR` customization variables values
correspond initially to the EPUB directory container and/or the
EPUB output file (see [Container Directory and Output Files](#EPUB-Output-File-and-Directory)). These
customization variables values are undefined or reset to the
locations in the container directory where the XHTML files are output
for the HTML generation. It is mentioned here because resetting
customization variables is unusual; however, the variables reset are
used internally for the conversion, and should not interact with any
customization set by the user.

See [HTML Customization Variables](#HTML-Customization-Variables).

---

Next: [HTML Cross-references](#HTML-Xref), Previous: [Generating EPUB](#Generating-EPUB), Up: [Generating HTML](#Generating-HTML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 21.6 Code Examples Syntax Highlighting in HTML [¶](#Code-Examples-Syntax-Highlighting-in-HTML)

|  |
| --- |
| **warning:** Source highlighting is experimental, feedback is welcomed. |

Support for source code syntax highlighting is available in
`texi2any` for the HTML output, with the help of external software.
This feature is turned on by setting `HIGHLIGHT_SYNTAX`. Source code
highlighting is set up for `@example` blocks. The language
specified for syntax highlighting is the first argument on the `@example` line
(see [`@example`: Example Text](#g_t_0040example)), or `HIGHLIGHT_SYNTAX_DEFAULT_LANGUAGE` if set
and there is no first argument.

The `HIGHLIGHT_SYNTAX` value determines the command used for highlighting:

`highlight`
:   Use `highlight` from
    <http://www.andre-simon.de/doku/highlight/en/highlight.php>;

`pygments`
:   Use `pygmentize` from <https://pygments.org/>;

`anything else`
:   Use `source-highlight` (see [GNU Source-highlight](source-highlight.html#Top)).

See [Other Customization Variables](#Other-Customization-Variables).

---

Previous: [Code Examples Syntax Highlighting in HTML](#Syntax-Highlighting), Up: [Generating HTML](#Generating-HTML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 21.7 HTML Cross-references [¶](#HTML-Cross_002dreferences)

Cross-references between Texinfo manuals in HTML format become standard
HTML `<a>` links. This section describes the algorithm used,
so that Texinfo can cooperate with other programs, such as
`texi2html`, by writing mutually compatible HTML files.

This algorithm may or may not be used for links *within* HTML
output for a Texinfo file. Since no issues of compatibility arise in
such cases, we do not need to specify this.

We try to support references to such “external” manuals in both
monolithic and split forms. A *monolithic* (mono) manual is
entirely contained in one file, and a *split* manual has a file
for each node. (See [HTML Splitting](#HTML-Splitting).)

The algorithm was primarily devised by Patrice Dumas in 2003–04.

* [HTML Cross-reference Link Basics](#HTML-Xref-Link-Basics)
* [HTML Cross-reference Node Name Expansion](#HTML-Xref-Node-Name-Expansion)
* [HTML Cross-reference Command Expansion](#HTML-Xref-Command-Expansion)
* [HTML Cross-reference 8-bit Character Expansion](#HTML-Xref-8_002dbit-Character-Expansion)
* [HTML Cross-reference Mismatch](#HTML-Xref-Mismatch)
* [HTML Cross-reference Configuration: `htmlxref.cnf`](#HTML-Xref-Configuration)

---

Next: [HTML Cross-reference Node Name Expansion](#HTML-Xref-Node-Name-Expansion), Up: [HTML Cross-references](#HTML-Xref)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 21.7.1 HTML Cross-reference Link Basics [¶](#HTML-Cross_002dreference-Link-Basics)

For our purposes, an HTML link consists of four components: a host
name, a directory part, a file part, and a target part. We
always assume the `http` protocol. For example:

```
http://host/dir/file.html#target
```

The information to construct a link comes from the node name and
manual name in the cross-reference command in the Texinfo source
(see [Cross-references](#Cross-References)), and from *external information*
(see [HTML Cross-reference Configuration: `htmlxref.cnf`](#HTML-Xref-Configuration)).

We now consider each part in turn.

The host is hardwired to be the local host. This could either
be the literal string ‘`localhost`’, or, according to the rules for
HTML links, the ‘`http://localhost/`’ could be omitted entirely.

The dir and file parts are more complicated, and depend on
the relative split/mono nature of both the manual being processed and
the manual that the cross-reference refers to. The underlying idea is
that there is one directory for Texinfo manuals in HTML, and a given
manual is either available as a monolithic file
`manual.html`, or a split subdirectory
`manual_html/*.html`. Here are the cases:

* If the present manual is split, and the referent manual is also split,
  the directory is ‘`../referent_html/`’ and the file is the
  expanded node name (described later).
* If the present manual is split, and the referent manual is mono, the
  directory is ‘`../`’ and the file is `referent.html`.
* If the present manual is mono, and the referent manual is split, the
  directory is `referent_html/` and the file is the expanded node
  name.
* If the present manual is mono, and the referent manual is also mono,
  the directory is `./` (or just the empty string), and the file is
  `referent.html`.

Another rule, that only holds for file names, is that base file names
are truncated to 245 characters, to allow for an extension to be
appended and still comply with the 255-character limit which is common
to many filesystems. Although technically this can be changed with
the `BASEFILENAME_LENGTH` customization variable (see [Other Customization Variables](#Other-Customization-Variables)), doing so would make cross-manual references
to such nodes invalid.

Any directory part in the file name argument of the source cross
reference command is ignored. Thus, `@xref{,,,../foo}` and
`@xref{,,,foo}` both use ‘`foo`’ as the manual name. This
is because any such attempted hardwiring of the directory is very
unlikely to be useful for all the output formats that use the manual
name.

Finally, the target part is always the expanded node name.

Whether the present manual is split or mono is determined by user
option; `texi2any` defaults to split, with the
`--no-split` option overriding this.

Whether the referent manual is split or mono, however, is another bit
of the external information (see [HTML Cross-reference Configuration: `htmlxref.cnf`](#HTML-Xref-Configuration)). By
default, `texi2any` uses the same form of the referent manual
as the present manual.

Thus, there can be a mismatch between the format of the referent
manual that the generating software assumes, and the format it’s
actually present in. See [HTML Cross-reference Mismatch](#HTML-Xref-Mismatch).

---

Next: [HTML Cross-reference Command Expansion](#HTML-Xref-Command-Expansion), Previous: [HTML Cross-reference Link Basics](#HTML-Xref-Link-Basics), Up: [HTML Cross-references](#HTML-Xref)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 21.7.2 HTML Cross-reference Node Name Expansion [¶](#HTML-Cross_002dreference-Node-Name-Expansion)

As mentioned in the previous section, the key part of the HTML cross
reference algorithm is the conversion of node names in the Texinfo
source into strings suitable for XHTML identifiers and file names. The
restrictions are similar for each: plain ASCII letters, numbers, and
the ‘`-`’ and ‘`_`’ characters are all that can be used.
(Although HTML anchors can contain most characters, XHTML is more
restrictive.)

Cross-references in Texinfo can refer either to nodes, anchors
(see [`@anchor`: Defining Arbitrary Cross-reference Targets](#g_t_0040anchor)) or float labels (see [`@float` [type][,label]: Floating Material](#g_t_0040float)).
However, anchors and float labels are treated identically
to nodes in this context, so we’ll continue to say “node” names for
simplicity.

A special exception: the Top node (see [The ‘Top’ Node and Master Menu](#The-Top-Node)) is always
mapped to the file `index.html`, to match web server software.
However, the HTML *target* is ‘`Top`’. Thus (in the split case):

```
@xref{Top,,, emacs, The GNU Emacs Manual}.
⇒ <a href="../emacs_html/index.html#Top">
```

1. The standard ASCII letters (a-z and A-Z) are not modified. All other
   characters may be changed as specified below.
2. The standard ASCII numbers (0-9) are not modified except when a number
   is the first character of the node name. In that case, see below.
3. Multiple consecutive space, tab and newline characters are transformed
   into just one space.
4. Leading and trailing spaces are removed.
5. After the above has been applied, each remaining space character is
   converted into a ‘`-`’ character.
6. Other ASCII 7-bit characters are transformed into ‘`_00xx`’,
   where xx is the ASCII character code in (lowercase) hexadecimal.
   This includes ‘`_`’, which is mapped to ‘`_005f`’.
7. If the node name does not begin with a letter, the literal string
   ‘`g_t`’ is prefixed to the result. (Due to the rules above, that
   string can never occur otherwise; it is an arbitrary choice, standing
   for “GNU Texinfo”.) This is necessary because XHTML requires that
   identifiers begin with a letter.

For example:

```
@node A  node --- with _'%
⇒ A-node-_002d_002d_002d-with-_005f_0027_0025
```

Example translations of common characters:

* ‘`_`’ ⇒ ‘`_005f`’
* ‘`-`’ ⇒ ‘`_002d`’
* ‘`A node`’ ⇒ ‘`A-node`’

On case-folding computer systems, nodes differing only by case will be
mapped to the same file. In particular, as mentioned above, Top
always maps to the file `index.html`. Thus, on a case-folding
system, Top and a node named ‘Index’ will both be written to
`index.html`. Fortunately, the targets serve to distinguish
these cases, since HTML target names are always case-sensitive,
independent of operating system.

---

Next: [HTML Cross-reference 8-bit Character Expansion](#HTML-Xref-8_002dbit-Character-Expansion), Previous: [HTML Cross-reference Node Name Expansion](#HTML-Xref-Node-Name-Expansion), Up: [HTML Cross-references](#HTML-Xref)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 21.7.3 HTML Cross-reference Command Expansion [¶](#HTML-Cross_002dreference-Command-Expansion)

Node names may contain @-commands (see [`@node` Line Requirements](#Node-Line-Requirements)).
This section describes how they are handled.

First, comments are removed.

Next, any `@value` commands (see [`@set` and `@value`](#g_t_0040set-_0040value)) and
macro invocations (see [Invoking Macros](#Invoking-Macros)) are fully expanded.

Then, for the following commands, the command name and braces are removed,
and the text of the argument is recursively transformed:

```
@asis @b @cite @code @command @dfn @dmn @dotless
@emph @env @file @i @indicateurl @kbd @key
@samp @sansserif @sc @slanted @strong @sub @sup
@t @U @var @verb @w
```

In addition, the following commands are replaced by constant text, as
shown below. If any of these commands have non-empty arguments, as in
`@TeX{bad}`, it is an error, and the result is unspecified.
In this table, ‘(space)’ means a space character and ‘(nothing)’ means
the empty string. The notation ‘U+hhhh’ means Unicode code
point hhhh (in hex, as usual).

There are further transformations of many of these expansions to yield
the final file or other target name, such as space characters to
‘`-`’, etc., according to the other rules.

|  |  |
| --- | --- |
| `@(newline)` | (space) |
| `@(space)` | (space) |
| `@(tab)` | (space) |
| `@!` | ‘`!`’ |
| `@*` | (space) |
| `@-` | (nothing) |
| `@.` | ‘`.`’ |
| `@:` | (nothing) |
| `@?` | ‘`?`’ |
| `@@` | ‘`@`’ |
| `@{` | ‘`{`’ |
| `@}` | ‘`}`’ |
| `@LaTeX` | ‘`LaTeX`’ |
| `@TeX` | ‘`TeX`’ |
| `@arrow` | U+2192 |
| `@bullet` | U+2022 |
| `@comma` | ‘`,`’ |
| `@copyright` | U+00A9 |
| `@dots` | U+2026 |
| `@enddots` | ‘`...`’ |
| `@equiv` | U+2261 |
| `@error` | ‘`error-->`’ |
| `@euro` | U+20AC |
| `@exclamdown` | U+00A1 |
| `@expansion` | U+21A6 |
| `@geq` | U+2265 |
| `@leq` | U+2264 |
| `@minus` | U+2212 |
| `@ordf` | U+00AA |
| `@ordm` | U+00BA |
| `@point` | U+22C6 |
| `@pounds` | U+00A3 |
| `@print` | U+22A3 |
| `@questiondown` | U+00BF |
| `@registeredsymbol` | U+00AE |
| `@result` | U+21D2 |
| `@textdegree` | U+00B0 |
| `@tie` | (space) |

Quotation mark @-commands (`@quotedblright{}` and the like),
are likewise replaced by their Unicode values. Normal quotation
*characters* (e.g., ASCII ‘ and ’) are not altered.
See [Inserting Quotation Marks](#Inserting-Quotation-Marks).

Any `@acronym`, `@abbr`, `@email`, and
`@image` commands are replaced by their first argument. (For
these commands, all subsequent arguments are optional, and ignored
here.) See [`@acronym`{acronym[, meaning]}](#g_t_0040acronym), and [`@email`{email-address[, displayed-text]}](#g_t_0040email), and [Inserting Images](#Images).

Accents are handled according to the next section.

Any other command is an error, and the result is unspecified.

---

Next: [HTML Cross-reference Mismatch](#HTML-Xref-Mismatch), Previous: [HTML Cross-reference Command Expansion](#HTML-Xref-Command-Expansion), Up: [HTML Cross-references](#HTML-Xref)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 21.7.4 HTML Cross-reference 8-bit Character Expansion [¶](#HTML-Cross_002dreference-8_002dbit-Character-Expansion)

Usually, characters other than plain 7-bit ASCII are transformed into
the corresponding Unicode code point(s) in Normalization Form C,
which uses precomposed characters where available. (This is the
normalization form recommended by the W3C and other bodies.) This
holds when that code point is `0xffff` or less, as it almost
always is.

These will then be further transformed by the rules above into the
string ‘`_hhhh`’, where hhhh is the code point in hex.

For example, combining this rule and the previous section:

```
@node @b{A} @TeX{} @u{B} @point{}@enddots{}
⇒ A-TeX-B_0306-_22C6_002e_002e_002e
```

Notice: 1) `@enddots` expands to three periods which in
turn expands to three ‘`_002e`’’s; 2) `@u{B}` is a ‘B’
with a breve accent, which does not exist as a pre-accented Unicode
character, therefore expands to ‘`B_0306`’ (B with combining
breve).

When the Unicode code point is above `0xffff`, the transformation
is ‘`__xxxxxx`’, that is, two leading underscores followed by
six hex digits. Since Unicode has declared that their highest code
point is `0x10ffff`, this is sufficient. (We felt it was better
to define this extra escape than to always use six hex digits, since
the first two would nearly always be zeros.)

This method works fine if the node name consists mostly of ASCII
characters and contains only few 8-bit ones. But if the document is
written in a language whose script is not based on the Latin alphabet
(for example, Ukrainian), it will create file names consisting almost
entirely of ‘`_xxxx`’ notations, which is inconvenient and
all but unreadable. To handle such cases, `texi2any` offers
the `--transliterate-file-names` command line option. This
option enables *transliteration* of node names into ASCII
characters for the purposes of file name creation and referencing.
The transliteration is based on phonetic principles, which makes the
generated file names more easily understandable.

For the definition of Unicode Normalization Form C, see Unicode
report UAX#15, <http://www.unicode.org/reports/tr15/>. Many
related documents and implementations are available elsewhere on the
web.

---

Next: [HTML Cross-reference Configuration: `htmlxref.cnf`](#HTML-Xref-Configuration), Previous: [HTML Cross-reference 8-bit Character Expansion](#HTML-Xref-8_002dbit-Character-Expansion), Up: [HTML Cross-references](#HTML-Xref)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 21.7.5 HTML Cross-reference Mismatch [¶](#HTML-Cross_002dreference-Mismatch)

As mentioned earlier (see [HTML Cross-reference Link Basics](#HTML-Xref-Link-Basics)), the generating
software may need to guess whether a given manual being cross-referenced is available in split or monolithic form—and, inevitably,
it might guess wrong. However, when the *referent* manual is
generated, it is possible to handle at least some mismatches.

In the case where we assume the referent is split, but it is actually
available in mono, the only recourse would be to generate a
`manual_html/` subdirectory full of HTML files which redirect back to
the monolithic `manual.html`. Since this is essentially the same
as a split manual in the first place, it’s not very appealing.

On the other hand, in the case where we assume the referent is mono,
but it is actually available in split, it is possible to use
JavaScript to redirect from the putatively monolithic
`manual.html` to the different `manual_html/node.html` files.
Here’s an example:

```
function redirect() {
  switch (location.hash) {
    case "#Node1":
      location.replace("manual_html/Node1.html#Node1"); break;
    case "#Node2" :
      location.replace("manual_html/Node2.html#Node2"); break;
    ...
    default:;
  }
}
```

Then, in the `<body>` tag of `manual.html`:

```
<body onLoad="redirect();">
```

Once again, this is something the software which generated the
*referent* manual has to do in advance, it’s not something the
software generating the cross-reference in the present manual can
control.

---

Previous: [HTML Cross-reference Mismatch](#HTML-Xref-Mismatch), Up: [HTML Cross-references](#HTML-Xref)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 21.7.6 HTML Cross-reference Configuration: `htmlxref.cnf` [¶](#HTML-Cross_002dreference-Configuration_003a-htmlxref_002ecnf)

`texi2any` reads a file named `htmlxref.cnf` to gather
information for cross-references to other manuals in HTML output. It
is looked for in the following directories:

`./`
:   (the current directory)

`./.texinfo/`
:   (under the current directory)

`~/.texinfo/`
:   (where `~` is the current user’s home directory)

`sysconfdir/texinfo/`
:   (where sysconfdir is the system configuration directory
    specified at compile-time, e.g., `/usr/local/etc`)

`datadir/texinfo/`
:   (likewise specified at compile time, e.g., `/usr/local/share`)

All files found are used, with earlier entries overriding later ones.
The Texinfo distribution includes a default file which handles many
GNU manuals; it is installed in the last of the above directories,
i.e., `datadir/texinfo/htmlxref.cnf`.

The `HTMLXREF_MODE` customization variable can be set to modify how the
files are found. For instance, if set to ‘`none`’, no external information
is used. `HTMLXREF_FILE` sets the file name to something else than
`htmlxref.cnf`. see [HTML Customization Variables](#HTML-Customization-Variables).

The file is line-oriented. Lines consisting only of whitespace are
ignored. Comments are indicated with a ‘`#`’ at the beginning of a
line, optionally preceded by whitespace. Since ‘`#`’ can occur in
URLs (like almost any character), it does not otherwise start a
comment.

Each non-blank non-comment line must be either a *variable
assignment* or *manual information*.

A variable assignment line looks like this:

```
varname = varvalue
```

Whitespace around the ‘`=`’ is optional and ignored. The
varname should consist of letters; case is significant. The
varvalue is an arbitrary string, continuing to the end of the
line. Variables are then referenced with ‘`${varname}`’;
variable references can occur in the varvalue.

A manual information line looks like this:

```
manual keyword urlprefix
```

with manual the short identifier for a manual, keyword
being one of: `mono`, `node`, `section`,
`chapter`, and urlprefix described below. Variable
references can occur only in the urlprefix. For example (used
in the canonical `htmlxref.cnf`):

```
G = http://www.gnu.org
GS = ${G}/software
hello mono    ${GS}/hello/manual/hello.html
hello chapter ${GS}/hello/manual/html_chapter/
hello section ${GS}/hello/manual/html_section/
hello node    ${GS}/hello/manual/html_node/
```

If the keyword is `mono`, urlprefix gives the host,
directory, and file name for manual as one monolithic file.

If the keyword is `node`, `section`, or `chapter`,
urlprefix gives the host and directory for manual split
into nodes, sections, or chapters, respectively.

When available, `texi2any` will use the “corresponding”
value for cross-references between manuals. That is, when generating
monolithic output (`--no-split`), the `mono` URL will be
used, when generating output that is split by node, the `node`
URL will be used, etc. However, if a manual is not available in that
form, anything that is available can be used. Here is the search
order for each style:

```
node    ⇒ node,    section, chapter, mono
section ⇒ section, chapter, node,    mono
chapter ⇒ chapter, section, node,    mono
mono    ⇒ mono,    chapter, section, node
```

These section- and chapter-level cross-manual references can succeed
only when the target manual was created using `--node-files`;
this is the default for split output.

If you have additions or corrections to the `htmlxref.cnf`
distributed with Texinfo, please email [bug-texinfo@gnu.org](mailto:bug-texinfo@gnu.org) as
usual. You can get the latest version from
<http://ftpmirror.gnu.org/texinfo/htmlxref.cnf>.

---

Next: [Tips and Hints](#Tips), Previous: [Generating HTML](#Generating-HTML), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## Appendix A @-Command Details [¶](#g_t_0040_002dCommand-Details-1)

Here are the details of @-commands: information about their syntax, a
list of commands, and information about where commands can appear.

* [@-Command Syntax](#Command-Syntax)
* [@-Command List](#Command-List)
* [@-Command Contexts](#Command-Contexts)
* [Obsolete @-Commands](#Obsolete-_0040_002dCommands)

---

Next: [@-Command List](#Command-List), Up: [@-Command Details](#g_t_0040_002dCommand-Details)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### A.1 @-Command Syntax [¶](#g_t_0040_002dCommand-Syntax)

Texinfo has the following types of @-command:

1. Brace commands
:   These commands start with @ followed by a letter or a word, followed by an
    argument within braces. For example, the command `@dfn` indicates
    the introductory or defining use of a term; it is used as follows: ‘`In
    Texinfo, @@-commands are @dfn{mark-up} commands.`’

2. Line commands
:   These commands occupy an entire line. The line starts with @,
    followed by the name of the command (a word); for example, `@center`
    or `@cindex`. If no argument is needed, the word is followed by
    the end of the line. If there is an argument, it is separated from
    the command name by a space. Braces are not used.

3. Block commands [¶](#index-end)
:   These commands are written at the start of a line, with general text on
    following lines, terminated by a matching `@end` command on a
    line of its own. For example, `@example`, then the lines of a
    coding example, then `@end example`. Some of these block commands
    take arguments as line commands do; for example, `@enumerate A`
    opening an environment terminated by `@end enumerate`. Here
    ‘`A`’ is the argument.

4. Symbol insertion commands with no arguments
:   These commands start with @ followed by a word followed by a
    left and right- brace. These commands insert special symbols in
    the document; they do not take arguments. Some examples:
    `@dots{}` ⇒ ‘`…`’, `@equiv{}`
    ⇒ ‘`≡`’, `@TeX{}` ⇒ ‘TeX’, and
    `@bullet{}` ⇒ ‘`•`’.

5. Non-alphabetic commands
:   The names of commands in all of the above categories consist of
    alphabetic characters, almost entirely in lower-case. Unlike those, the
    non-alphabetic commands consist of an @ followed by a
    punctuation mark or other character that is not part of the Latin
    alphabet. Non-alphabetic commands are almost always part of text
    within a paragraph. The non-alphabetic commands include `@@`,
    `@{`, `@}`, `@.`, `@SPACE`, and most of
    the accent commands.

6. Miscellaneous commands
:   There are a handful of commands that don’t fit into any of the above
    categories; for example, the obsolete command `@refill`, which is
    always used at the end of a paragraph immediately following the final
    period or other punctuation character. `@refill` takes no
    argument and does not require braces. Likewise, `@tab` used in a
    `@multitable` block does not take arguments, and is not followed
    by braces.

Thus, the alphabetic commands fall into classes that have
different argument syntaxes. You cannot tell to which class a command
belongs by the appearance of its name, but you can tell by the
command’s meaning: if the command stands for a glyph, it is in
class 4 and does not require an argument; if it makes sense to use the
command among other text as part of a paragraph, the command
is in class 1 and must be followed by an argument in braces. The
non-alphabetic commands, such as `@:`, are exceptions to the
rule; they do not need braces.

The purpose of having different syntax for commands is to make Texinfo
files easier to read, and also to help the GNU Emacs paragraph and
filling commands work properly.

---

Next: [@-Command Contexts](#Command-Contexts), Previous: [@-Command Syntax](#Command-Syntax), Up: [@-Command Details](#g_t_0040_002dCommand-Details)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]
