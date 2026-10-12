### 5.2 Parts of a Cross-reference [¶](#Parts-of-a-Cross_002dreference)

A cross-reference command requires only one argument, which is
the name of the node to which it refers. A cross-reference command
may contain up to four additional arguments.
The template for a full five argument cross-reference looks like
this:

```
@xref{node-name, online-label, printed-label,
manual-name, printed-manual-title}
```

The five possible arguments for a cross-reference are:

1. The node or anchor name. This is the location to which the cross-reference
   takes you. In a printed document, the location of the node provides the
   page reference only for references within the same document.
   Use `@node` to define the node (see [Writing a `@node` Line](#Writing-a-Node)),
   `@anchor` (see [`@anchor`: Defining Arbitrary Cross-reference Targets](#g_t_0040anchor)), or `@float`
   (see [`@float` [type][,label]: Floating Material](#g_t_0040float)) with a label.
   This argument is required (except for reference to whole manuals).

   Write a node name in a cross-reference in exactly the same way as in
   the `@node` line, including the same capitalization; otherwise, the
   processors may not find the reference.
2. A label for online output. It is usually omitted; then
   the topic description (third argument) is used if it was specified;
   if that was omitted as well, the node name is used.
3. A label for printed output. Often, this is the title or topic of the
   section. This is used as the name of the reference in the printed
   manual. If omitted, the node name is used.
4. The name of the manual to which the reference refers, if it is
   outside the current manual, in a different Texinfo file.
5. The title of the printed manual to which the reference refers,
   from a different Texinfo file.

Whitespace before and after the commas separating these arguments is
ignored. To include a comma in one of the arguments, use
`@comma{}` (see [Inserting ‘,’ with `@comma{}`](#Inserting-a-Comma)).

Cross-references with one, two, three, four, and five arguments are
described separately in following sections.

When processing with TeX, a comma is automatically inserted after the
page number for cross-references to within the same manual, unless the
closing brace of the argument is followed by non-whitespace (such as a
comma or period). This gives you the choice of whether to have a comma
there in other output formats. For example,

```
@xref{Another Section} for more info
```

with TeX produces ‘See Another Section, page ppp, for more info’,
and in the Info output produces
‘`*Note Another Section:: for more info`’.

If an unwanted comma is added, follow the argument
with a command such as ‘`@:`’. For example,
‘`@xref{Hurricanes}@: --- for the details`’ produces

> See Hurricanes, page ppp — for the details

instead of
‘See Hurricanes, page ppp, — for the details’.

`texi2any` warns and protect names when the text of a cross-reference
(and node names and menu items) contains a problematic construct that could
interfere with its parsing in Info. See [Info Node Names Constraints](#Info-Node-Names-Constraints).

---

Next: [`@xref` with Two Arguments](#Two-Arguments), Previous: [Parts of a Cross-reference](#Cross-Reference-Parts), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.3 `@xref` with One Argument [¶](#g_t_0040xref-with-One-Argument)

The simplest form of `@xref` takes one argument, the name of
another node in the same Texinfo file.

For example,

```
@xref{Tropical Storms}.
```

produces

```
*Note Tropical Storms::.
```

in Info and

> See Section 3.1 [Tropical Storms], page 24.

in a printed manual.

---

Next: [`@xref` with Three Arguments](#Three-Arguments), Previous: [`@xref` with One Argument](#One-Argument), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.4 `@xref` with Two Arguments [¶](#g_t_0040xref-with-Two-Arguments)

With two arguments, the second is used as a label for the online output.

The template is like this:

```
@xref{node-name, online-label}.
```

For example,

```
@xref{Electrical Effects, Lightning}.
```

produces:

```
*Note Lightning: Electrical Effects.
```

in Info and

> See Section 5.2 [Electrical Effects], page 57.

in a printed manual, where the node name is printed.

The second argument to cross-references shares constraints
with node names. The potentially problematic character in this
context is the colon. See [Info Node Names Constraints](#Info-Node-Names-Constraints).

---

Next: [`@xref` with Four and Five Arguments](#Four-and-Five-Arguments), Previous: [`@xref` with Two Arguments](#Two-Arguments), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.5 `@xref` with Three Arguments [¶](#g_t_0040xref-with-Three-Arguments)

A third argument replaces the node name in the printed output. The third
argument should be the name of the section in the printed output, or
else state the topic discussed by that section.

The template is like this:

```
@xref{node-name, online-label, printed-label}.
```

For example,

```
@xref{Electrical Effects, Lightning, Thunder and Lightning},
for details.
```

produces

```
*Note Lightning: Electrical Effects, for details.
```

in Info and

> See Section 5.2 [Thunder and Lightning], page 57, for details.

in a printed manual.

If a third argument is given and the second one is empty, then the
third argument serves for both. (Note how two commas, side by side, mark
the empty second argument.)

```
@xref{Electrical Effects, , Thunder and Lightning},
for details.
```

produces

```
*Note Thunder and Lightning: Electrical Effects, for details.
```

in Info and

> See Section 5.2 [Thunder and Lightning], page 57, for details.

in a printed manual.

The third argument to cross-references shares constraints
with node names. The potentially problematic character in this
context is the colon. See [Info Node Names Constraints](#Info-Node-Names-Constraints).

As a practical matter, it is often best to write cross-references with
just the first argument if the node name and the section title are the
same (or nearly so), and with the first and third arguments only if the
node name and title are different.

Texinfo offers a setting to use the section title instead of node
names by default in cross-references (an explicitly specified third
argument still takes precedence):

```
@xrefautomaticsectiontitle on
```

Typically this line would be given near the beginning of the document
and used for the whole manual. But you can turn it off if you want
(`@xrefautomaticsectiontitle off`), for example, if you’re
including some other sub-document that doesn’t have suitable section
names. This setting also applies to node headers in HTML, if
`@xrefautomaticsectiontitle` is on, the sections names are used
in node headers instead of the node names when possible.

---

Next: [Referring to a Manual as a Whole](#Referring-to-a-Manual-as-a-Whole), Previous: [`@xref` with Three Arguments](#Three-Arguments), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.6 `@xref` with Four and Five Arguments [¶](#g_t_0040xref-with-Four-and-Five-Arguments)

In a cross-reference, a fourth argument specifies the name of another
manual, different from the file in which the reference appears, and
a fifth argument specifies its title as a printed manual.

The full template is:

```
@xref{node-name, online-label, printed-label,
manual-name, printed-manual-title}.
```

For example,

```
@xref{Electrical Effects, Lightning, Thunder and Lightning,
weather, An Introduction to Meteorology}.
```

produces this output in Info:

```
*Note Lightning: (weather)Electrical Effects.
```

As you can see, the name of the manual is enclosed in parentheses
and precedes the name of the node. In HTML, the manual name and
the node name are used to construct the hyperlink URL
(see [HTML Cross-references](#HTML-Xref)), while the link text is based on the label.

In a printed manual, the reference looks like this:

> See section “Thunder and Lightning” in An Introduction to
> Meteorology.

The title of the printed manual is typeset like `@cite`; and the
reference lacks a page number since the page a reference refers when
that reference is to another manual cannot be known.

Next case: often, you will leave out the second argument when you use
the long version of `@xref`. In this case, the third argument,
the topic description, will be used as the cross-reference name in
online formats. For example,

```
@xref{Electrical Effects, , Thunder and Lightning,
weather, An Introduction to Meteorology}.
```

produces

```
*Note Thunder and Lightning: (weather)Electrical Effects.
```

in Info and

> See section “Thunder and Lightning” in An Introduction to
> Meteorology.

in a printed manual.

Next case: If the node name and the section title are the same in the
other manual, you may also leave out the section title. In this case,
the node name is used in both instances. For example,

```
@xref{Electrical Effects,,,
weather, An Introduction to Meteorology}.
```

produces

```
*Note (weather)Electrical Effects::.
```

in Info and

> See section “Electrical Effects” in An Introduction to
> Meteorology.

in a printed manual.

In general, there is no reason to have a manual name argument without a
printed manual argument, unless no printed manual is generated.
You may also want to refer to another manual file that
is within a single printed manual—when multiple Texinfo files are
incorporated into the same printed manual but can create separate output files
in other output formats. In this case, you need to specify only the fourth
argument, and not the fifth. If the printed manual title argument is missing, the
manual name will be used instead in printed output.

A printed manual title argument without an online manual argument is of
little use unless only a printed manual is generated from the Texinfo source.
The result in online formats depends on the format, and
can be, for example, an empty manual name or a reference to the printed
manual formatted in a similar way to the printed output.

Finally, it’s also allowed to leave out all the arguments
*except* the fourth and fifth, to refer to another manual as a
whole. See the next section.

---

Next: [`@xref`](#g_t_0040xref), Previous: [`@xref` with Four and Five Arguments](#Four-and-Five-Arguments), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.7 Referring to a Manual as a Whole [¶](#Referring-to-a-Manual-as-a-Whole-1)

Ordinarily, you must always name a node in a cross-reference.
However, it’s not unusual to want to refer to another manual as a
whole, rather than a particular section within it. In this case,
giving any section name is an unnecessary distraction.

So, with cross-references to other manuals (see [`@xref` with Four and Five Arguments](#Four-and-Five-Arguments)), if the first argument is either ‘`Top`’ (capitalized
just that way) or omitted entirely, and the third argument is omitted,
the printed output includes no node or section name. (The Info output
includes ‘`Top`’ if it was given.) For example,

```
@xref{Top,,, make, The GNU Make Manual}.
```

produces

```
*Note (make)Top::.
```

and

> See The GNU Make Manual.

Info readers will go to the Top node of the manual whether
or not the ‘Top’ node is explicitly specified.

It’s also possible (and is historical practice) to refer to a whole
manual by specifying the ‘Top’ node and an appropriate entry for the
third argument to the `@xref` command. Using this idiom, to
make a cross-reference to The GNU Make Manual, you would write:

```
@xref{Top,, Overview, make, The GNU Make Manual}.
```

which produces

```
*Note Overview: (make)Top.
```

in Info and

> See section “Overview” in The GNU Make Manual.

in a printed manual.

In this example, ‘`Top`’ is the name of the first node, and
‘`Overview`’ is the name of the first section of the manual. There
is no widely-used convention for naming the first section in a printed
manual, this is just what the Make manual happens to use. This
arbitrariness of the first name is a principal reason why omitting the
third argument in whole-manual cross-references is preferable.

---

Next: [`@ref`](#g_t_0040ref), Previous: [Referring to a Manual as a Whole](#Referring-to-a-Manual-as-a-Whole), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.8 `@xref` [¶](#g_t_0040xref-1)

The `@xref` command generates a cross-reference for the
beginning of a sentence. Examples of using `@xref` are
in previous sections.

---

Next: [`@pxref`](#g_t_0040pxref), Previous: [`@xref`](#g_t_0040xref), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.9 `@ref` [¶](#g_t_0040ref-1)

`@ref` is nearly the same as `@xref` except that it does
not generate a ‘See’ in the output, just the reference itself.
This makes it useful as the last part of a sentence.

For example,

```
For more information, @pxref{This}, and @ref{That}.
```

produces in Info:

```
For more information, *note This::, and *note That::.
```

and in printed output:

> For more information, see Section 1.1 [This], page 1,
> and Section 1.2 [That], page 2.

The `@ref` command can tempt writers to express themselves in a
manner that is suitable for a printed manual but looks awkward in the
Info format. Bear in mind that your audience could be using both the
printed and other output formats such as Info. For example:

```
Sea surges are described in @ref{Hurricanes}.
```

looks ok in the printed output:

> Sea surges are described in Section 6.7 [Hurricanes], page 72.

but is awkward to read in Info, “note” being a verb:

```
Sea surges are described in *note Hurricanes::.
```

---

Next: [`@anchor`: Defining Arbitrary Cross-reference Targets](#g_t_0040anchor), Previous: [`@ref`](#g_t_0040ref), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.10 `@pxref` [¶](#g_t_0040pxref-1)

The parenthetical reference command, `@pxref`, is nearly the
same as `@xref`, but it is best used within parentheses.
The command differs from `@xref` in that the reference is
typeset with a lowercase ‘see’ rather than an uppercase ‘See’.
In Info, ‘`*note`’ is output.

With one argument, a parenthetical cross-reference looks like this:

```
... storms cause flooding (@pxref{Hurricanes}) ...
```

which produces

```
... storms cause flooding (*note Hurricanes::) ...
```

in Info and

> … storms cause flooding (see Section 6.7 [Hurricanes], page 72) …

in a printed manual.

In past versions of Texinfo, it was not allowed to write punctuation
after a `@pxref`, so it could be used *only* before a
right parenthesis. This is no longer the case.
The effect of ‘`@pxref{node-name}`’ is similar to that of
‘`see @ref{node-name}`’. However, in many circumstances the
latter is preferable, as this makes it clear in the Info output that
the word “see” should be present.

---

Next: [`@link`: Plain, unadorned hyperlink](#g_t_0040link), Previous: [`@pxref`](#g_t_0040pxref), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.11 `@anchor`: Defining Arbitrary Cross-reference Targets [¶](#g_t_0040anchor_003a-Defining-Arbitrary-Cross_002dreference-Targets)

An *anchor* is a position in your document, labelled so that
cross-references can refer to it, just as they can to nodes. You
create an anchor with the `@anchor` command, and give the label
as a normal brace-delimited argument. For example:

```
This marks the @anchor{x-spot}spot.
...
@xref{x-spot,,the spot}.
```

produces:

```
This marks the spot.
...
See [the spot], page 1.
```

As you can see, the `@anchor` command itself produces no output.
This example defines an anchor ‘x-spot’ just before the word ‘spot’.
You can refer to it later with an `@xref` or other cross
reference command, as shown (see [Cross-references](#Cross-References)).

It is best to put `@anchor` commands just before the position you
wish to refer to; that way, the reader’s eye is led on to the correct
text when they jump to the anchor. You can put the `@anchor`
command on a line by itself if that helps readability of the source.
Whitespace (including newlines) is ignored after `@anchor`.

Anchor names, node names and float labels may not conflict. Anchors,
nodes and float labels are
given similar treatment in some ways; for example, the
`goto-node` command takes either an anchor name or a node name as
an argument. (See [Go to node](https://www.gnu.org/software/emacs/manual/html_mono/info.html#Go-to-node) in Info.). Anchors names and float
labels could also appear in menus (see [Menus](#Menus)) and node direction
pointers (see [Writing a `@node` Line](#Writing-a-Node)), although this is not recommended.

Anchor names share the same constraints as nodes on the characters
that can be included (see [Info Node Names Constraints](#Info-Node-Names-Constraints)).

Because of this duality, when you delete or rename a node, it is
usually a good idea to define an `@anchor` with the old name.
That way, any links to the old node, whether from other Texinfo
manuals or general web pages, keep working.

---

Next: [`@inforef`: Cross-references to Info-only Material](#g_t_0040inforef), Previous: [`@anchor`: Defining Arbitrary Cross-reference Targets](#g_t_0040anchor), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.12 `@link`: Plain, unadorned hyperlink [¶](#g_t_0040link_003a-Plain_002c-unadorned-hyperlink)

`@link` produces a plain hyperlink in output formats that
support it, including in HTML, DocBook, LaTeX and online PDF.
The template is:

```
@link{node-name, label, manual-name}
```

node-name is the name of the target node or anchor.
Either or both of label and manual-name can
be omitted. label, if given, is the text to use for the
link. manual-name is the name of the external manual that the
target appears within; if not given, the reference is to the current
manual.

`@link` has similar output to `@ref`, except that it does
produce any extra text around the link label in Info or printed output
that would mark it as a cross-reference.

Be careful about using `@link` to produce links that are
necessary for a user to move around a manual, as these links will do
nothing in these output formats. `@link` is best used to add
convenience links that are nonetheless not essential for a reader
to understand the text of the manual. For example, you might use
`@link` in a code sample to reference documentation of a symbol
in a programming library.

---

Next: [`@url`, `@uref{url[, text][, replacement]}`](#g_t_0040url), Previous: [`@link`: Plain, unadorned hyperlink](#g_t_0040link), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.13 `@inforef`: Cross-references to Info-only Material [¶](#g_t_0040inforef_003a-Cross_002dreferences-to-Info_002donly-Material)

`@inforef` is used for making cross-references to Info
documents—even from a printed manual. This was originally
used for Info files that were not generated from any Texinfo source.
The command is now obsolete and should not be used. In addition
to having little use, similar output can be obtained with
`@xref`, `@ref` or `@pxref` with the Info
file name as the fourth argument and no fifth argument.

The command takes either two or three arguments, in the following
order:

1. The node name.
2. The cross-reference name (optional).
3. The Info file name.

The template is:

```
@inforef{node-name, cross-reference-name, info-file-name}
```

---

Next: [`@cite`{reference}](#g_t_0040cite), Previous: [`@inforef`: Cross-references to Info-only Material](#g_t_0040inforef), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.14 `@url`, `@uref{url[, text][, replacement]}` [¶](#g_t_0040url_002c-_0040uref_007burl_005b_002c-text_005d_005b_002c-replacement_005d_007d)

`@url` produces a reference to a uniform resource locator
(URL). It takes one mandatory argument, the URL, and two optional
arguments which control the text that is displayed. In HTML and PDF
output, `@url` produces a link you can follow. (To merely
indicate a URL without creating a link people can follow, use
`@indicateurl`, see [`@indicateurl`{uniform-resource-locator}](#g_t_0040indicateurl).)

`@uref` is a synonym for `@url`.
(Originally, `@url` had the meaning of `@indicateurl`
and `@uref` was required to produce a working link, but
in practice `@url` was almost always misused. So we’ve changed
the meaning.)

The second argument, if specified, is the text to display (the default
is the URL itself); in output formats other than HTML, the URL is
output in addition to this text.

The third argument, if specified, is the text to display, but in this
case the URL is not output in any format. This is useful when the
text is already sufficiently referential, as in a man page. Also, if
the third argument is given, the second argument is ignored.

* [`@url` Examples](#g_t_0040url-Examples)
* [URL Line Breaking](#URL-Line-Breaking)
* [`@url` PDF Output Format](#g_t_0040url-PDF-Output-Format)

---

Next: [URL Line Breaking](#URL-Line-Breaking), Up: [`@url`, `@uref{url[, text][, replacement]}`](#g_t_0040url)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 5.14.1 `@url` Examples [¶](#g_t_0040url-Examples-1)

First, here is an example of the simplest form of `@url`, with
just one argument. The given URL is both the target and the visible
text of the link:

```
The official GNU ftp site is @url{http://ftp.gnu.org/gnu}.
```

produces:

```
The official GNU ftp site is http://ftp.gnu.org/gnu.
```

#### Two-argument form of `@url` [¶](#Two_002dargument-form-of-_0040url)

Here is an example of the two-argument form:

```
The official @url{http://ftp.gnu.org/gnu, GNU ftp site}
holds programs and texts.
```

which produces:

```
The official GNU ftp site
holds programs and texts.
```

The HTML output is this:

```
The official <a href="http://ftp.gnu.org/gnu">GNU ftp site</a>
holds programs and texts.
```

In other formats, the output is like this:

```
The official GNU ftp site (http://ftp.gnu.org/gnu)
holds programs and texts.
```

#### Three-argument form of `@url` [¶](#Three_002dargument-form-of-_0040url)

Finally, an example of the three-argument form:

```
The @url{/man.cgi/1/ls,,ls} program ...
```

which, except for HTML, produces:

```
The ls program ...
```

but with HTML:

```
The <a href="/man.cgi/1/ls">ls</a> program ...
```

By the way, some people prefer to display URLs in the unambiguous
format:

```
<URL:http://host/path>
```

You can use this form in the input file if you wish. We feel it’s not
necessary to include the ‘`<URL:`’ and ‘`>`’ in the output,
since to be useful any software that tries to detect URLs in text
already has to detect them without the ‘`<URL:`’.

---

Next: [`@url` PDF Output Format](#g_t_0040url-PDF-Output-Format), Previous: [`@url` Examples](#g_t_0040url-Examples), Up: [`@url`, `@uref{url[, text][, replacement]}`](#g_t_0040url)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 5.14.2 URL Line Breaking [¶](#URL-Line-Breaking-1)

TeX allows line breaking within URLs at only a few characters
(which are special in URLs): ‘`&`’, ‘`.`’, ‘`#`’, ‘`?`’,
and ‘`/`’ (but not between two ‘`/`’ characters). A tiny amount
of stretchable space is also inserted around these characters to help
with line breaking.

For HTML output, modern browsers will also do line breaking within
displayed URLs. If you need to allow breaks at other characters you
can insert `@/` as needed (see [`@*` and `@/`: Generate and Allow Line Breaks](#Line-Breaks)).

By default, in TeX any such breaks at special characters will occur
after the character. Some people prefer such breaks to happen before
the special character. This can be controlled with the
`@urefbreakstyle` command (this command has effect only in
TeX):

```
@urefbreakstyle how
```

where the argument how is one of these words:

‘`after`’
:   (the default) Potentially break after the special characters.

‘`before`’
:   Potentially break before the special characters.

‘`none`’
:   Do not consider breaking at the special characters at all; any potential
    breaks must be manually inserted.

---

Previous: [URL Line Breaking](#URL-Line-Breaking), Up: [`@url`, `@uref{url[, text][, replacement]}`](#g_t_0040url)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 5.14.3 `@url` PDF Output Format [¶](#g_t_0040url-PDF-Output-Format-1)

If the ultimate purpose of a PDF is only to be viewed online, perhaps
similar to HTML in some inchoate way, you may not want the URLs to be
included in the visible text (just as URLs are not visible to readers
of web pages). Texinfo provides a PDF-specific option for this, which
must be used inside `@tex`:

```
@tex
\global\urefurlonlylinktrue
@end tex
```

The result is that `@url{http://www.gnu.org, GNU}` has the
visible output of just ‘GNU’, with a link target of
<http://www.gnu.org>. Ordinarily, the visible output would
include both the label and the url: ‘GNU (<http://www.gnu.org>)’.

This option only has effect when the PDF output is produced with the
pdfTeX program, not with other ways of getting from Texinfo to PDF
(e.g., TeX to DVI to PDF). Consequently, it is ok to specify this
option unconditionally within `@tex`, as shown above. It is
ignored when DVI is being produced.

---

Next: [PDF Colors](#PDF-Colors), Previous: [`@url`, `@uref{url[, text][, replacement]}`](#g_t_0040url), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.15 `@cite`{reference} [¶](#g_t_0040cite_007breference_007d)

Use the `@cite` command for the name of a book that lacks a
companion Info file. For example, we could refer to A Book.
The command selects a slanted font in the printed
manual, and generates quotation marks in the Info file.

If a book is written in Texinfo, it is better to use a cross-reference
command since a reader can easily follow such a reference in Info.
See [`@xref`](#g_t_0040xref).

---

Previous: [`@cite`{reference}](#g_t_0040cite), Up: [Cross-references](#Cross-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 5.16 PDF Colors [¶](#PDF-Colors-1)

By default, URLs and cross-reference links are printed in black in PDF
output. Very occasionally, however, you may want to highlight such
“live” links with a different color, as is commonly done on web
pages. Texinfo provides a PDF-specific option for specifying these
colors, which must be used inside `@tex`:

```
@tex
\global\def\linkcolor{1 0 0}  % red
\global\def\urlcolor{0 1 0}   % green
@end tex
```

`\urlcolor` changes the color of `@url` output (both the
actual URL and any textual label), while `\linkcolor` changes the
color for cross-references to nodes, etc. They are independent.

The three given values must be numbers between 0 and 1, specifying the
amount of red, green, and blue respectively.

These definitions only have an effect when the PDF output is produced
with the pdfTeX program, not with other ways of getting from
Texinfo to PDF (e.g., TeX to DVI to PDF). Consequently, it is ok
to specify this option unconditionally within `@tex`, as shown
above. It is ignored when DVI is being produced.

We do not recommend colorizing just for fun; unless you have a
specific reason to use colors, best to skip it.

---

Next: [Quotations and Examples](#Quotations-and-Examples), Previous: [Cross-references](#Cross-References), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 6 Marking Text, Words and Phrases [¶](#Marking-Text_002c-Words-and-Phrases)

In Texinfo, you can mark words and phrases in a variety of ways.
The Texinfo processors use this information to determine how to
highlight the text.
You can specify, for example, whether a word or phrase is a
defining occurrence, a metasyntactic variable, or a symbol used in a
program. Also, you can emphasize text, in several different ways.

* [Indicating Definitions, Commands, etc.](#Indicating)
* [Emphasizing Text](#Emphasis)

---

Next: [Emphasizing Text](#Emphasis), Up: [Marking Text, Words and Phrases](#Marking-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 6.1 Indicating Definitions, Commands, etc. [¶](#Indicating-Definitions_002c-Commands_002c-etc_002e)

Texinfo has commands for indicating just what kind of object a piece
of text refers to. For example, email addresses are marked by
`@email`; that way, the result can be a live link to send email
when the output format supports it. If the email address was simply
marked as “print in a typewriter font”, that would not be possible.

* [Highlighting Commands are Useful](#Useful-Highlighting)
* [`@code`{sample-code}](#g_t_0040code)
* [`@kbd`{keyboard-characters}](#g_t_0040kbd)
* [`@key`{key-name}](#g_t_0040key)
* [`@samp`{text}](#g_t_0040samp)
* [`@verb`{chartextchar}](#g_t_0040verb)
* [`@var`{metasyntactic-variable}](#g_t_0040var)
* [`@env`{environment-variable}](#g_t_0040env)
* [`@file`{file-name}](#g_t_0040file)
* [`@command`{command-name}](#g_t_0040command)
* [`@option`{option-name}](#g_t_0040option)
* [`@dfn`{term}](#g_t_0040dfn)
* [`@abbr`{abbreviation[, meaning]}](#g_t_0040abbr)
* [`@acronym`{acronym[, meaning]}](#g_t_0040acronym)
* [`@indicateurl`{uniform-resource-locator}](#g_t_0040indicateurl)
* [`@email`{email-address[, displayed-text]}](#g_t_0040email)

---

Next: [`@code`{sample-code}](#g_t_0040code), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.1 Highlighting Commands are Useful [¶](#Highlighting-Commands-are-Useful)

The commands serve a variety of purposes:

`@code{sample-code}`
:   Indicate text that is a literal example of a piece of a program.
    See [`@code`{sample-code}](#g_t_0040code).

`@kbd{keyboard-characters}`
:   Indicate keyboard input. See [`@kbd`{keyboard-characters}](#g_t_0040kbd).

`@key{key-name}`
:   Indicate the conventional name for a key on a keyboard.
    See [`@key`{key-name}](#g_t_0040key).

`@samp{text}`
:   Indicate text that is a literal example of a sequence of characters.
    See [`@samp`{text}](#g_t_0040samp).

`@verb{text}`
:   Write a verbatim sequence of characters.
    See [`@verb`{chartextchar}](#g_t_0040verb).

`@var{metasyntactic-variable}`
:   Indicate a metasyntactic variable. See [`@var`{metasyntactic-variable}](#g_t_0040var).

`@env{environment-variable}`
:   Indicate an environment variable. See [`@env`{environment-variable}](#g_t_0040env).

`@file{file-name}`
:   Indicate the name of a file. See [`@file`{file-name}](#g_t_0040file).

`@command{command-name}`
:   Indicate the name of a command.
    See [`@command`{command-name}](#g_t_0040command).

`@option{option}`
:   Indicate a command-line option.
    See [`@option`{option-name}](#g_t_0040option).

`@dfn{term}`
:   Indicate the introductory or defining use of a term.
    See [`@dfn`{term}](#g_t_0040dfn).

`@cite{reference}`
:   Indicate the name of a book. See [`@cite`{reference}](#g_t_0040cite).

`@abbr{abbreviation}`
:   Indicate an abbreviation, such as ‘Comput.’.

`@acronym{acronym}`
:   Indicate an acronym. See [`@acronym`{acronym[, meaning]}](#g_t_0040acronym).

`@indicateurl{uniform-resource-locator}`
:   Indicate an example (that is, nonfunctional) uniform resource locator.
    See [`@indicateurl`{uniform-resource-locator}](#g_t_0040indicateurl). (Use `@url` (see [`@url`, `@uref{url[, text][, replacement]}`](#g_t_0040url)) for
    live URLs.)

`@email{email-address[, displayed-text]}`
:   Indicate an electronic mail address. See [`@email`{email-address[, displayed-text]}](#g_t_0040email).

---

Next: [`@kbd`{keyboard-characters}](#g_t_0040kbd), Previous: [Highlighting Commands are Useful](#Useful-Highlighting), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.2 `@code`{sample-code} [¶](#g_t_0040code_007bsample_002dcode_007d)

Use the `@code` command to indicate text that is a piece of a
program and which consists of entire syntactic tokens. Enclose the
text in braces.

Thus, you should use `@code` for an expression in a program, for
the name of a variable or function used in a program, or for a
keyword in a programming language.

Use `@code` for command names in languages that resemble
programming languages, such as Texinfo. For example, `@code` and
`@samp` are produced by writing ‘`@code{@@code}`’ and
‘`@code{@@samp}`’ in the Texinfo source, respectively.

It is incorrect to alter the case of a word inside a `@code`
command when it appears at the beginning of a sentence. Most computer
languages are case sensitive. In C, for example, `Printf` is
different from the identifier `printf`, and most likely is a
misspelling of it. Even in languages which are not case sensitive, it
is confusing to a human reader to see identifiers spelled in different
ways. Pick one spelling and always use that. If you do not want to
start a sentence with a command name written all in lowercase, you
should rearrange the sentence.

The `@code` argument is typeset in a typewriter (monospace) font.
where the output format allows this. For example,

```
The function returns @code{nil}.
```

produces this:

> The function returns `nil`.

Here are some cases for which it is preferable *not* to use `@code`:

* For shell command names, such as `ls` (use `@command`).
* For environment variables, such as `TEXINPUTS` (use `@env`).
* For shell options, such as ‘`-c`’, when such options stand alone (use
  `@option`).
* An entire shell command often looks better if written using
  `@samp` rather than `@code`. In this case, the rule is to
  choose the more pleasing format.
* For a string of characters shorter than a syntactic token. For example,
  if you are writing about ‘`goto-ch`’, which is just a part of the
  name for the `goto-char` Emacs Lisp function, you should use
  `@samp`.
* In general, when writing about the characters used in a token; for
  example, do not use `@code` when you are explaining what letters
  or printable symbols can be used in the names of functions. (Use
  `@samp`.) Also, you should not use `@code` to mark text
  that is considered input to programs unless the input is written in a
  language that is like a programming language. For example, you should
  not use `@code` for the keystroke commands of GNU Emacs (use
  `@kbd` instead) although you may use `@code` for the names
  of the Emacs Lisp functions that the keystroke commands invoke.

By default, TeX will consider breaking lines at ‘`-`’ and
‘`_`’ characters within `@code` and related commands. This
can be controlled with `@allowcodebreaks`
(see [`@allowcodebreaks`: Control Line Breaks in `@code`](#g_t_0040allowcodebreaks)). In the HTML output breaking
lines is up to the browser’s behavior. For Info, it seems better
never to make such breaks.

For Info and plaintext, quotation characters are usually output around
the output of the `@code` command and related commands
(e.g., `@kbd`, `@command`) except
in typewriter-like contexts such as the `@example` environment
(see [`@example`: Example Text](#g_t_0040example)) and `@code` itself, etc.
To control which quoting characters are inserted by
`texi2any` in the output of ‘`@code`’, etc., see the
`OPEN_QUOTE_SYMBOL` and `CLOSE_QUOTE_SYMBOL` customization
variables (see [Other Customization Variables](#Other-Customization-Variables)).

---

Next: [`@key`{key-name}](#g_t_0040key), Previous: [`@code`{sample-code}](#g_t_0040code), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.3 `@kbd`{keyboard-characters} [¶](#g_t_0040kbd_007bkeyboard_002dcharacters_007d)

Use the `@kbd` command for characters of input to be typed by
users. For example, to refer to the characters `M-a`, write:

```
@kbd{M-a}
```

and to refer to the characters `M-x shell`, write:

```
@kbd{M-x shell}
```

By default, the `@kbd` command produces a different font (slanted
typewriter instead of normal typewriter, where the output format allows),
so users can distinguish the characters that they are supposed
to type from those that the computer outputs.

Since the usage of `@kbd` varies from manual to manual, you can
control the font switching with the `@kbdinputstyle` command.
This command has no effect on Info output. Write this command at the
beginning of a line with a single word as an argument, one of the
following:

‘`code`’
:   Always use the same font for `@kbd` as `@code`.

‘`example`’
:   Use the distinguishing font for `@kbd` only in `@example`
    and similar environments.

‘`distinct`’
:   (the default) Always use the distinguishing font for `@kbd`.

You can embed another @-command inside the braces of a `@kbd`
command. Here, for example, is the way to describe a command that
would be described more verbosely as “press the ‘`r`’ key and then
press the `RETURN` key”:

```
@kbd{r @key{RET}}
```

This produces: `r RET`. (The present manual uses the
default for `@kbdinputstyle`.)

You also use the `@kbd` command if you are spelling out the letters
you type; for example:

```
To give the @code{logout} command,
type the characters @kbd{l o g o u t @key{RET}}.
```

This produces:

> To give the `logout` command,
> type the characters `l o g o u t RET`.

(Also, this example shows that you can add spaces for clarity. If you
explicitly want to mention a space character as one of the characters of
input, write `@key{SPC}` for it.)

---

Next: [`@samp`{text}](#g_t_0040samp), Previous: [`@kbd`{keyboard-characters}](#g_t_0040kbd), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.4 `@key`{key-name} [¶](#g_t_0040key_007bkey_002dname_007d)

Use the `@key` command for the conventional name for a key on a
keyboard, as in:

```
@key{RET}
```

You can use the `@key` command within the argument of an
`@kbd` command when the sequence of characters to be typed
includes one or more keys that are described by name.

For example, to produce `C-x ESC` and `M-TAB` you
would type:

```
@kbd{C-x @key{ESC}}
@kbd{M-@key{TAB}}
```

Here is a list of the recommended names for keys:

> `SPC`
> :   Space
>
> `RET`
> :   Return
>
> `LFD`
> :   Linefeed (however, since most keyboards nowadays do not have a Linefeed key,
>     it might be better to call this character `C-j`)
>
> `TAB`
> :   Tab
>
> `BS`
> :   Backspace
>
> `ESC`
> :   Escape
>
> `DELETE`
> :   Delete
>
> `SHIFT`
> :   Shift
>
> `CTRL`
> :   Control
>
> `META`
> :   Meta

There are subtleties to handling words like ‘meta’ or ‘ctrl’ that are
names of modifier keys. When mentioning a character in which the
modifier key is used, such as `Meta-a`, use the `@kbd` command
alone; do not use the `@key` command; but when you are referring
to the modifier key in isolation, use the `@key` command. For
example, write ‘`@kbd{Meta-a}`’ to produce `Meta-a` and
‘`@key{META}`’ to produce `META`.

---

Next: [`@verb`{chartextchar}](#g_t_0040verb), Previous: [`@key`{key-name}](#g_t_0040key), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.5 `@samp`{text} [¶](#g_t_0040samp_007btext_007d)

Use the `@samp` command to indicate text that is a literal example
or ‘sample’ of a sequence of characters in a file, string, pattern, etc.
Enclose the text in braces. The argument appears within single
quotation marks; in addition, it is printed in a fixed-width font.

```
To match @samp{foo} at the end of the line,
use the regexp @samp{foo$}.
```

produces

> To match ‘`foo`’ at the end of the line, use the regexp
> ‘`foo$`’.

Any time you are referring to single characters, you should use
`@samp` unless `@kbd` or `@key` is more appropriate.
Also, you may use `@samp` for entire statements in C and for entire
shell commands—in this case, `@samp` often looks better than
`@code`. Basically, `@samp` is a catchall for whatever is
not covered by `@code`, `@kbd`, `@key`,
`@command`, etc.

Only include punctuation marks within braces if they are part of the
string you are specifying. Write punctuation marks outside the braces
if those punctuation marks are part of the English text that surrounds
the string. In the following sentence, for example, the commas and
period are outside of the braces:

```
In English, the vowels are @samp{a}, @samp{e},
@samp{i}, @samp{o}, @samp{u}, and sometimes
@samp{y}.
```

This produces:

> In English, the vowels are ‘`a`’, ‘`e`’,
> ‘`i`’, ‘`o`’, ‘`u`’, and sometimes
> ‘`y`’.

---

Next: [`@var`{metasyntactic-variable}](#g_t_0040var), Previous: [`@samp`{text}](#g_t_0040samp), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.6 `@verb`{chartextchar} [¶](#g_t_0040verb_007bchartextchar_007d)

Use the `@verb` command to print a verbatim sequence of
characters.

Like LaTeX’s `\verb` command, the verbatim text can be quoted using
any unique delimiter character. Enclose the verbatim text, including the
delimiters, in braces. Text is printed in a fixed-width font:

```
How many @verb{|@|}-escapes does one need to print this
@verb{.@a @b.@c.} string or @verb{+@'e?`{}!`\+} this?
```

produces

```
How many @-escapes does one need to print this
@a @b.@c string or @'e?`{}!`\ this?
```

This is in contrast to `@samp` (see the previous section),
`@code`, and similar commands; in those cases, the argument is
normal Texinfo text, where the three characters `@{}` are
special, as usual. With `@verb`, nothing is special except the
delimiter character you choose.

The delimiter character itself may appear inside the verbatim text, as
shown above. As another example, ‘`@verb{...}`’ prints a single
(fixed-width) period.

It is not reliable to use `@verb` inside other Texinfo
constructs. In particular, it does not work to use `@verb` in
anything related to cross-referencing, such as section titles or
figure captions.

---

Next: [`@env`{environment-variable}](#g_t_0040env), Previous: [`@verb`{chartextchar}](#g_t_0040verb), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.7 `@var`{metasyntactic-variable} [¶](#g_t_0040var_007bmetasyntactic_002dvariable_007d)

Use the `@var` command to indicate metasyntactic variables. A
*metasyntactic variable* is something that stands for another
piece of text. For example, you should use a metasyntactic variable
in the documentation of a function to describe the arguments that are
passed to that function.

Do not use `@var` for the names of normal variables in computer
programs. These are specific names, so `@code` is correct for
them. For example, the Emacs Lisp variable `texinfo-tex-command`
is not a metasyntactic variable; it is properly formatted using `@code`.

Do not use `@var` for environment variables either; `@env`
is correct for them (see the next section).

The effect of `@var` in the Info file is to change the case of
the argument to all uppercase. In the printed manual, the argument
is output in slanted type.
[4](#FOOT4)

For example,

```
To delete file @var{filename},
type @samp{rm @var{filename}}.
```

produces

> To delete file filename, type ‘`rm filename`’.

(Note that `@var` may appear inside `@code`,
`@samp`, `@file`, etc.)

Write a metasyntactic variable all in lowercase without spaces, and
use hyphens to make it more readable. Thus, the Texinfo source for
the illustration of how to begin a Texinfo manual looks like
this:

```
\input texinfo
@@settitle @var{name-of-manual}
```

This produces:

```
\input texinfo
@settitle name-of-manual
```

In some documentation styles, metasyntactic variables are shown with
angle brackets, for example:

```
..., type rm <filename>
```

However, that is not the style that Texinfo uses.

---

Next: [`@file`{file-name}](#g_t_0040file), Previous: [`@var`{metasyntactic-variable}](#g_t_0040var), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.8 `@env`{environment-variable} [¶](#g_t_0040env_007benvironment_002dvariable_007d)

Use the `@env` command to indicate environment variables, as
used by many operating systems, including GNU. Do not use it for
*meta*syntactic variables; use `@var` for those (see the
previous section).

`@env` is equivalent to `@code` in its effects.
For example:

```
The @env{PATH} environment variable ...
```

produces

> The `PATH` environment variable …

---

Next: [`@command`{command-name}](#g_t_0040command), Previous: [`@env`{environment-variable}](#g_t_0040env), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.9 `@file`{file-name} [¶](#g_t_0040file_007bfile_002dname_007d)

Use the `@file` command to indicate text that is the name of a
file, buffer, or directory, or is the name of a node in Info. You can
also use the command for file name suffixes. Do not use `@file`
for symbols in a programming language; use `@code`.

`@file` is equivalent to `code` in its effects. For
example,

```
The @file{.el} files are in
the @file{/usr/local/emacs/lisp} directory.
```

produces

> The `.el` files are in
> the `/usr/local/emacs/lisp` directory.

---

Next: [`@option`{option-name}](#g_t_0040option), Previous: [`@file`{file-name}](#g_t_0040file), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.10 `@command`{command-name} [¶](#g_t_0040command_007bcommand_002dname_007d)

Use the `@command` command to indicate command names, such as
`ls` or `cc`.

`@command` is equivalent to `@code` in its effects.
For example:

```
The command @command{ls} lists directory contents.
```

produces

> The command `ls` lists directory contents.

You should write the name of a program in the ordinary text font, rather
than using `@command`, if you regard it as a new English word,
such as ‘Emacs’ or ‘Bison’.

When writing an entire shell command invocation, as in ‘`ls -l`’,
you should use either `@samp` or `@code` at your discretion.

---

Next: [`@dfn`{term}](#g_t_0040dfn), Previous: [`@command`{command-name}](#g_t_0040command), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.11 `@option`{option-name} [¶](#g_t_0040option_007boption_002dname_007d)

Use the `@option` command to indicate a command-line option; for
example, `-l` or `--version` or
`--output=filename`.

`@option` is equivalent to `@code` in its effects.
For example:

```
The option @option{-l} produces a long listing.
```

produces

> The option `-l` produces a long listing.

---

Next: [`@abbr`{abbreviation[, meaning]}](#g_t_0040abbr), Previous: [`@option`{option-name}](#g_t_0040option), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.12 `@dfn`{term} [¶](#g_t_0040dfn_007bterm_007d)

Use the `@dfn` command to identify the introductory or defining
use of a technical term. Use the command only in passages whose
purpose is to introduce a term which will be used again or which the
reader ought to know. Mere passing mention of a term for the first
time does not deserve `@dfn`. The command selects a slanted font
in the printed manual, and generates double quotation marks in the Info
file. For example:

```
Getting rid of a file is called @dfn{deleting} it.
```

produces

> Getting rid of a file is called *deleting* it.

As a general rule, a sentence containing the defining occurrence of a
term should be a definition of the term. The sentence does not need
to say explicitly that it is a definition, but it should contain the
information of a definition—it should make the meaning clear.

---

Next: [`@acronym`{acronym[, meaning]}](#g_t_0040acronym), Previous: [`@dfn`{term}](#g_t_0040dfn), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.13 `@abbr`{abbreviation[, meaning]} [¶](#g_t_0040abbr_007babbreviation_005b_002c-meaning_005d_007d)

You can use the `@abbr` command for general abbreviations. The
abbreviation is given as the single argument in braces, as in
‘`@abbr{Comput.}`’. As a matter of style, or for particular
abbreviations, you may prefer to omit periods, as in
‘`@abbr{Mr} Stallman`’.

`@abbr` accepts an optional second argument, intended to be used
for the meaning of the abbreviation.

If the abbreviation ends with a lowercase letter and a period, and is
not at the end of a sentence, and has no second argument, remember to
use the `@.` command (see [Ending a Sentence](#Ending-a-Sentence)) to get the
correct spacing. However, you do not have to use `@.` within
the abbreviation itself; Texinfo automatically assumes periods within
the abbreviation do not end a sentence.

In output formats with an appropriate tag, such as HTML and DocBook, this tag
is used. Otherwise, the first argument is printed as-is; if the second
argument is present, it is printed in parentheses after the
abbreviation. For instance:

```
@abbr{Comput. J., Computer Journal}
```

produces:

```
Comput. J. (Computer Journal)
```

For abbreviations consisting of all capital letters, you may prefer to
use the `@acronym` command instead. See the next section for
more on the usage of these two commands.

---

Next: [`@indicateurl`{uniform-resource-locator}](#g_t_0040indicateurl), Previous: [`@abbr`{abbreviation[, meaning]}](#g_t_0040abbr), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.14 `@acronym`{acronym[, meaning]} [¶](#g_t_0040acronym_007bacronym_005b_002c-meaning_005d_007d)

You can use the `@acronym` command for abbreviations written in
all capital letters, such as ‘NASA’. The abbreviation is
given as the single argument in braces, as in
‘`@acronym{NASA}`’. As a matter of style, or for particular
acronyms, you may prefer to use periods, as in
‘`@acronym{N.A.S.A.}`’.

`@acronym` accepts an optional second argument, intended to be
used for the meaning of the acronym.

If the acronym is at the end of a sentence, and if there is no second
argument, remember to use the `@.` or similar command
(see [Ending a Sentence](#Ending-a-Sentence)) to get the correct spacing.

In TeX, the acronym is printed in slightly smaller font. In the
Info output, the argument is printed as-is. In either format, and
in LaTeX output, if the second argument is present, it is printed in
parentheses after the acronym. In HTML and DocBook the appropriate
tag is used.

For instance (since GNU is a recursive acronym, we use
`@acronym` recursively):

```
@acronym{GNU, @acronym{GNU}'s Not Unix}
```

produces:

```
GNU (GNU’s Not Unix)
```

In some circumstances, it is conventional to print family names in all
capitals. Don’t use `@acronym` for this, since a name is not an
acronym. Use `@sc` instead (see [`@sc`{text}: The Small Caps Font](#Smallcaps)).

`@abbr` and `@acronym` are closely related commands: they
both signal to the reader that a shortened form is being used, and
possibly give a meaning. When choosing whether to use these two
commands, please bear the following in mind.

* In common English usage, acronyms are a subset of abbreviations: they
  include pronounceable words like ‘NATO’, ‘radar’, and
  ‘snafu’; some sources also include syllable acronyms like
  ‘Usenet’, hybrids like ‘SIGGRAPH’, and unpronounceable
  initialisms like ‘FBI’.
* In Texinfo, an acronym (but not an abbreviation) should consist only
  of capital letters and periods, no lowercase.
* In TeX, an acronym (but not an abbreviation) is printed in a
  slightly smaller font.
* It usually turns out to be quite difficult and/or time-consuming to
  consistently use `@acronym` for all sequences of uppercase
  letters. Furthermore, it looks strange for some acronyms to be in the
  normal font size and others to be smaller. Thus, a simpler approach
  you may wish to consider is to avoid `@acronym` and just typeset
  everything as normal text in all capitals: ‘`GNU`’, producing the
  output ‘GNU’.
* In general, it’s not essential to use either of these commands for all
  abbreviations; use your judgment. Text is perfectly readable without
  them.

---

Next: [`@email`{email-address[, displayed-text]}](#g_t_0040email), Previous: [`@acronym`{acronym[, meaning]}](#g_t_0040acronym), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.15 `@indicateurl`{uniform-resource-locator} [¶](#g_t_0040indicateurl_007buniform_002dresource_002dlocator_007d)

Use the `@indicateurl` command to indicate a uniform resource
locator on the World Wide Web. This is purely for markup purposes and
does not produce a link you can follow (use the `@url` or
`@uref` command for that, see [`@url`, `@uref{url[, text][, replacement]}`](#g_t_0040url)).
`@indicateurl` is useful for URLs which do not actually exist.
For example:

```
For example, the URL might be @indicateurl{http://example.org/path}.
```

which produces:

```
For example, the URL might be ‘http://example.org/path’.
```

The output from `@indicateurl` is usually like that of
`@samp` (see [`@samp`{text}](#g_t_0040samp)).

---

Previous: [`@indicateurl`{uniform-resource-locator}](#g_t_0040indicateurl), Up: [Indicating Definitions, Commands, etc.](#Indicating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.1.16 `@email`{email-address[, displayed-text]} [¶](#g_t_0040email_007bemail_002daddress_005b_002c-displayed_002dtext_005d_007d)

Use the `@email` command to indicate an electronic mail address.
It takes one mandatory argument, the address, and one optional argument, the
text to display (the default is the address itself).

In Info, the address is shown in angle brackets, preceded by the text
to display if any. In printed output, the angle brackets are omitted. In
HTML and DocBook output, `@email` produces a ‘`mailto`’ link.
In HTML, a ‘`mailto`’ link usually brings up a mail composition window.
For example:

```
Send bug reports to @email{bug-texinfo@@gnu.org},
suggestions to the @email{bug-texinfo@@gnu.org, same place}.
```

produces

```
Send bug reports to bug-texinfo@gnu.org,
suggestions to the same place.
```

---

Previous: [Indicating Definitions, Commands, etc.](#Indicating), Up: [Marking Text, Words and Phrases](#Marking-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 6.2 Emphasizing Text [¶](#Emphasizing-Text)

Usually, Texinfo changes the font to mark words in the text according
to the category the words belong to; an example is the `@code`
command. Most often, this is the best way to mark words. However,
sometimes you will want to emphasize text without indicating a
category. Texinfo has two commands to do this. Also, Texinfo has
several commands that specify the font in which text will be output.
These commands have no effect in Info and only one of them, the
`@r` command, has any regular use.

* [`@emph`{text} and `@strong`{text}](#g_t_0040emph-_0040strong)
* [`@sc`{text}: The Small Caps Font](#Smallcaps)
* [Fonts for Printing](#Fonts)

---

Next: [`@sc`{text}: The Small Caps Font](#Smallcaps), Up: [Emphasizing Text](#Emphasis)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.2.1 `@emph`{text} and `@strong`{text} [¶](#g_t_0040emph_007btext_007d-and-_0040strong_007btext_007d)

The `@emph` and `@strong` commands are for emphasis;
`@strong` is stronger. In printed output, `@emph` produces
*italics* and `@strong` produces **bold**.
In the Info output, `@emph` surrounds the text with underscores
(‘`_`’), and `@strong` puts asterisks around the text.

For example,

```
@strong{Caution:} @samp{rm * .[^.]*}
removes @emph{all} files in the directory.
```

produces the following:

> **Caution**: ‘`rm * .[^.]*`’
> removes *all* files in the directory.

The `@strong` command is seldom used except to mark what is, in
effect, a typographical element, such as the word ‘Caution’ in the
preceding example.

> **Caution:** Do not use `@strong` with the word ‘`Note`’ followed by a
> space; Info will mistake the combination for a cross-reference. Use a
> phrase such as **Please notice** or **Caution** instead, or
> the optional argument to `@quotation`—‘`Note`’ is allowable
> there.

---

Next: [Fonts for Printing](#Fonts), Previous: [`@emph`{text} and `@strong`{text}](#g_t_0040emph-_0040strong), Up: [Emphasizing Text](#Emphasis)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.2.2 `@sc`{text}: The Small Caps Font [¶](#g_t_0040sc_007btext_007d_003a-The-Small-Caps-Font)

Use the ‘`@sc`’ command to set text in A SMALL CAPS FONT
(where possible). Write the text you want to be in small caps between
braces in lowercase, like this:

```
Richard @sc{Stallman} a commencé le projet GNU.
```

This produces:

```
Richard STALLMAN a commencé le projet GNU.
```

As shown here, we recommend reserving `@sc` for special cases
where you want typographic small caps; family names are one such,
especially in languages other than English, though there are no
hard-and-fast rules about such things.

TeX typesets any uppercase letters between the braces of an
`@sc` command in full-size capitals; only lowercase letters are
printed in the small caps font. In the Info output, the argument to
`@sc` is printed in all uppercase. In HTML, the argument is
uppercased and the output marked with the `<small>` tag to reduce
the font size, since HTML cannot easily represent true small caps.
In LaTeX, a command setting small caps fonts is output.

Overall, we recommend using standard upper- and lowercase letters
wherever possible.

---

Previous: [`@sc`{text}: The Small Caps Font](#Smallcaps), Up: [Emphasizing Text](#Emphasis)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 6.2.3 Fonts for Printing [¶](#Fonts-for-Printing)

Texinfo provides one command to change the size of the main body font
in printed output for a document: `@fonttextsize`. It has no
effect in other output. It takes a single argument on the remainder
of the line, which must be either ‘`10`’ or ‘`11`’. For
example:

```
@fonttextsize 10
```

The effect is to reduce the body font to a 10pt size (the
default is 11pt). Fonts for other elements, such as sections
and chapters, are reduced accordingly. This should only be used in
conjunction with `@smallbook` (see [`@smallbook`: Printing “Small” Books](#g_t_0040smallbook)) or
similar, since 10pt fonts on standard paper (8.5x11 or A4) are
too small. One reason to use this command is to save pages, and hence
printing cost, for physical books.

Texinfo does not at present have commands to switch the font family
to use, or more general size-changing commands.

Texinfo also provides a number of font commands that specify font
changes in the printed manual and (where possible) in the HTML and DocBook
output. They have no effect in Info. All the commands apply to a following
argument surrounded by braces.

`@b` [¶](#index-b-_0028bold-font_0029)
:   selects **bold** face;

`@i` [¶](#index-i-_0028italic-font_0029)
:   selects an *italic* font;

`@r` [¶](#index-r-_0028roman-font_0029)
:   selects a roman font, which is the usual font in which text is
    printed. It may or may not be seriffed.

`@sansserif` [¶](#index-sansserif)
:   selects a sans serif font;

`@slanted` [¶](#index-slanted)
:   selects a *slanted* font;

`@t` [¶](#index-t-_0028typewriter-font_0029)
:   selects the `fixed-width`, typewriter-style font used by `@code`;

The `@r` command can be useful in example-like environments, to
write comments in the standard roman font instead of the fixed-width
font. This looks better in printed output.

For example,

```
@lisp
(+ 2 2)    ; @r{Add two plus two.}
@end lisp
```

produces

```
(+ 2 2)    ; Add two plus two.
```

The `@t` command can occasionally be useful for producing output in
a typewriter font where that is supported, but no distinction with
quotation marks is needed in Info or plain text. (Compare `@t{foo}`
producing `foo` with `@code{foo}` producing `foo`.) Here
are some possible reasons for using `@t` instead of `@code`:

* The argument is a single character
* There are already quotes of some kind enclosing the argument
* It’s evident from context or the argument itself that the argument
  could be computer code (e.g. name of a Usenet newsgroup)

In general, the other font commands are unlikely to be useful; they
exist primarily to make it possible to document the functionality of
specific font effects, such as in TeX and related packages.

---

Next: [Lists and Tables](#Lists-and-Tables), Previous: [Marking Text, Words and Phrases](#Marking-Text), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 7 Quotations and Examples [¶](#Quotations-and-Examples-1)

Quotations and examples are blocks of text consisting of one or more
whole paragraphs that are set off from the bulk of the text and
treated differently. They are usually indented in the output.

In Texinfo, you always begin a quotation or example by writing an
@-command at the beginning of a line by itself, and end it by writing
an `@end` command that is also at the beginning of a line by
itself. For instance, you begin an example by writing
`@example` by itself at the beginning of a line and end the
example by writing `@end example` on a line by itself, at the
beginning of that line, and with only one space between the
`@end` and the `example`.

* [Block Enclosing Commands](#Block-Enclosing-Commands)
* [`@quotation`: Block Quotations](#g_t_0040quotation)
* [`@indentedblock`: Indented text blocks](#g_t_0040indentedblock)
* [`@example`: Example Text](#g_t_0040example)
* [`@verbatim`: Literal Text](#g_t_0040verbatim)
* [`@lisp`: Marking a Lisp Example](#g_t_0040lisp)
* [`@display`: Examples Using the Text Font](#g_t_0040display)
* [`@format`: Examples Using the Full Line Width](#g_t_0040format)
* [`@exdent`: Undoing a Line’s Indentation](#g_t_0040exdent)
* [`@flushleft` and `@flushright`](#g_t_0040flushleft-_0040flushright)
* [`@raggedright`: Ragged Right Text](#g_t_0040raggedright)
* [`@noindent`: Omitting Indentation](#g_t_0040noindent)
* [`@indent`: Forcing Indentation](#g_t_0040indent)
* [`@cartouche`: Rounded Rectangles](#g_t_0040cartouche)
* [`@small…` Block Commands](#small)

---

Next: [`@quotation`: Block Quotations](#g_t_0040quotation), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 7.1 Block Enclosing Commands [¶](#Block-Enclosing-Commands-1)

Here is a summary of commands that enclose blocks of text, also known
as *environments*. They’re explained further in the following
sections.

`@quotation`
:   Indicate text that is quoted. The text is filled, indented (from both
    margins), and printed in a roman font by default.

`@indentedblock`
:   Like `@quotation`, but the text is indented only on the left.

`@example`
:   Illustrate code, commands, and the like. The text is printed
    in a fixed-width font, and indented but not filled.

`@lisp`
:   Like `@example`, but specifically for illustrating Lisp code. The
    text is printed in a fixed-width font, and indented but not filled.

`@verbatim`
:   Mark a piece of text that is to be printed verbatim; no character
    substitutions are made and all commands are ignored, until the next
    `@end verbatim`. The text is printed in a fixed-width font,
    and not indented or filled. Extra spaces and blank lines are
    significant, and tabs are expanded.

`@display`
:   Display illustrative text. The text is indented but not filled, and
    no font is selected (so, by default, the font is roman).

`@format`
:   Like `@display` (the text is not filled and no font is
    selected), but the text is not indented.

`@smallquotation`

`@smallindentedblock`

`@smallexample`

`@smalllisp`

`@smalldisplay`

`@smallformat`
:   These `@small...` commands are just like their non-small
    counterparts, except that they output text in a smaller font size,
    where possible.

`@flushleft`

`@flushright`
:   Text is not filled, but is set flush with the left or right margin,
    respectively.

`@raggedright`
:   Text is filled, but only justified on the left, leaving the right
    margin ragged.

`@cartouche`
:   Highlight text, often an example or quotation, by drawing a box with
    rounded corners around it.

The `@exdent` command is used within the above constructs to
undo the indentation of a line.

The `@noindent` command may be used after one of the above
constructs (or at the beginning of any paragraph) to prevent the
following text from being indented as a new paragraph.

---

Next: [`@indentedblock`: Indented text blocks](#g_t_0040indentedblock), Previous: [Block Enclosing Commands](#Block-Enclosing-Commands), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 7.2 `@quotation`: Block Quotations [¶](#g_t_0040quotation_003a-Block-Quotations)

The text of a quotation is processed like normal text (regular font,
text is filled) except that:

* the left margin is closer to the center of the page, so the whole of the
  quotation is indented; the right margin may also be closer to the center
  of the page
* the first lines of paragraphs are indented no more than other lines; and
* an `@author` command may be given to specify the author of the
  quotation.

> This is an example of text written between a `@quotation`
> command and an `@end quotation` command. A `@quotation`
> command is most often used to indicate text that is excerpted from
> another (real or hypothetical) printed work.

Write a `@quotation` command as text on a line by itself. This
line will disappear from the output. Mark the end of the quotation
with a line beginning with and containing only `@end quotation`.
The `@end quotation` line will likewise disappear from the
output.

`@quotation` takes one optional argument, given on the remainder
of the line. This text, if present, is included at the beginning of
the quotation in bold or otherwise emphasized, and followed with a
‘`:`’. For example:

```
@quotation Note
This is
a foo.
@end quotation
```

produces

> **Note:** This is
> a foo.

If the `@quotation` argument is one of these English words
(case-insensitive):

```
Caution  Important  Note  Tip  Warning
```

then the DocBook output uses corresponding special tags
(`<note>`, etc.) instead of the default `<blockquote>`.

If the author of the quotation is specified in the `@quotation`
block with the `@author` command, a line with the author name is
displayed after the quotation:

```
@quotation
People sometimes ask me if it is a sin in the Church of Emacs to use
vi.  Using a free version of vi is not a sin; it is a penance.  So happy
hacking.

@author Richard Stallman
@end quotation
```

produces

> People sometimes ask me if it is a sin in the Church of Emacs to use
> vi. Using a free version of vi is not a sin; it is a penance. So happy
> hacking.

— *Richard Stallman*

---

Next: [`@example`: Example Text](#g_t_0040example), Previous: [`@quotation`: Block Quotations](#g_t_0040quotation), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 7.3 `@indentedblock`: Indented text blocks [¶](#g_t_0040indentedblock_003a-Indented-text-blocks)

The `@indentedblock` environment is similar to
`@quotation`, except that text is only indented on the left (and
there is no optional argument for an author). Thus, the text font
remains unchanged, and text is gathered and filled as usual, but the
left margin is increased. For example:

> This is an example of text written between an `@indentedblock`
> command and an `@end indentedblock` command. The
> `@indentedblock` environment can contain any text or other
> commands desired.

This is written in the Texinfo source as:

```
@indentedblock
This is an example ...
@end indentedblock
```

---

Next: [`@verbatim`: Literal Text](#g_t_0040verbatim), Previous: [`@indentedblock`: Indented text blocks](#g_t_0040indentedblock), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 7.4 `@example`: Example Text [¶](#g_t_0040example_003a-Example-Text)

The `@example` environment is used to indicate
computer input or output that is not part of the running text. If you
want to embed code fragments within sentences, use the `@code`
command or its relatives instead (see [`@code`{sample-code}](#g_t_0040code)).

Write an `@example` command at the beginning of a line by
itself. Mark the end of the block with `@end example`.
For example,

```
@example
cp foo @var{dest1}; \
 cp foo @var{dest2}
@end example
```

produces

```
cp foo dest1; \
 cp foo dest2
```

The output uses a fixed-width font and is indented. Each line
in the input file is a line in the output; that is, the source
text is not filled. Extra spaces and blank lines are significant.
Texinfo commands *are* expanded; if you want the output to be
the input verbatim, use the `@verbatim` environment instead
(see [`@verbatim`: Literal Text](#g_t_0040verbatim)).

Examples are often, logically speaking, “in the middle” of a
paragraph, and the text that continues afterwards should not be
indented, as in the example above. The `@noindent` command
prevents a piece of text from being indented as if it were a new
paragraph (see [`@noindent`: Omitting Indentation](#g_t_0040noindent)).

If you wish to use the normal roman font for a code comment, you can
use the `@r` command (see [Fonts for Printing](#Fonts)).

You may optionally give arguments to the `@example` command,
separated by commas if there is more than one. In the HTML output,
any such arguments are output as class names, prefixed by the string
‘`user-`’. This may be useful for adding syntax highlighting to
manuals for code samples.

We recommend that when you give multiple arguments to `@example`, you
use the first argument to specify the language of the code (e.g.
‘`C`’, ‘`lisp`’, ‘`Cplusplus`’). You may find uses for other
arguments, such as providing a formatting hint or marking code samples
for extraction and further processing, but for now nothing definitive
is recommended. Perhaps this will change in future Texinfo releases.

> **Caution:** Do not use tabs in the lines of an example! (Or anywhere else in
> Texinfo, except in verbatim environments.) TeX treats tabs as
> single spaces, and that is not what they look like.

---

Next: [`@lisp`: Marking a Lisp Example](#g_t_0040lisp), Previous: [`@example`: Example Text](#g_t_0040example), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 7.5 `@verbatim`: Literal Text [¶](#g_t_0040verbatim_003a-Literal-Text)

Use the `@verbatim` environment for printing of text that may
contain special characters or commands that should not be interpreted,
such as computer input or output (`@example` interprets its text
as regular Texinfo commands). This is especially useful for including automatically
generated files in a Texinfo manual.

In general, the output will be just the same as the input. No
character substitutions are made, e.g., all spaces and blank lines are
significant, including tabs. The text is typeset in a fixed-width font,
and not indented or filled.

Write a `@verbatim` command at the beginning of a line by
itself. This line will disappear from the output. Mark the end of
the verbatim block with an `@end verbatim` command, also written
at the beginning of a line by itself. The `@end verbatim` will
also disappear from the output.

For example:

```
@verbatim
{
<tab>@command with strange characters: @'e
expand<tab>me
}
@end verbatim
```

(where <tab> stands for a literal tab character). This produces:

```
{
	@command with strange characters: @'e
expand	me
}
```

Since the lines containing `@verbatim` and `@end verbatim`
produce no output, typically you should put a blank line before the
`@verbatim` and another blank line after the `@end
verbatim`. Blank lines between the beginning `@verbatim` and
the ending `@end verbatim` will appear in the output.

You can get a “small” verbatim by enclosing the `@verbatim` in
an `@smallformat` environment, as shown here:

```
@smallformat
@verbatim
... still verbatim, but in a smaller font ...
@end verbatim
@end smallformat
```

Finally, a word of warning: it is not reliable to use
`@verbatim` inside other Texinfo constructs.

See also [`@verbatiminclude` file: Include a File Verbatim](#g_t_0040verbatiminclude).

---

Next: [`@display`: Examples Using the Text Font](#g_t_0040display), Previous: [`@verbatim`: Literal Text](#g_t_0040verbatim), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 7.6 `@lisp`: Marking a Lisp Example [¶](#g_t_0040lisp_003a-Marking-a-Lisp-Example)

The `@lisp` command was used for Lisp code:

```
@lisp
Example lisp code
@end lisp
```

This is now synonymous with the following:

```
@example lisp
Example lisp code
@end example
```

Use `@lisp` to preserve information regarding the nature of
the example. This is useful, for example, if you write a function
that evaluates only and all the Lisp code in a Texinfo file. Then you
can use the Texinfo file as a Lisp library.

---

Next: [`@format`: Examples Using the Full Line Width](#g_t_0040format), Previous: [`@lisp`: Marking a Lisp Example](#g_t_0040lisp), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 7.7 `@display`: Examples Using the Text Font [¶](#g_t_0040display_003a-Examples-Using-the-Text-Font)

The `@display` command begins another kind of environment, where
the font is left unchanged, not switched to typewriter as with
`@example`. Each line of input still produces a line of output,
and the output is still indented.

```
This is an example of text written between a @display command
and an @end display command.  The @display command
indents the text, but does not fill it.
```

---

Next: [`@exdent`: Undoing a Line’s Indentation](#g_t_0040exdent), Previous: [`@display`: Examples Using the Text Font](#g_t_0040display), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 7.8 `@format`: Examples Using the Full Line Width [¶](#g_t_0040format_003a-Examples-Using-the-Full-Line-Width)

The `@format` command is similar to `@display`, except it
leaves the text unindented. Like `@display`, it does not select
the fixed-width font. Thus,

```
@format
This is an example of text written between a @code{@@format} command
and an @code{@@end format} command.  As you can see
from this example,
the @code{@@format} command does not fill the text.
@end format
```

produces

```
This is an example of text written between a @format command
and an @end format command.  As you can see
from this example,
the @format command does not fill the text.
```

---

Next: [`@flushleft` and `@flushright`](#g_t_0040flushleft-_0040flushright), Previous: [`@format`: Examples Using the Full Line Width](#g_t_0040format), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 7.9 `@exdent`: Undoing a Line’s Indentation [¶](#g_t_0040exdent_003a-Undoing-a-Line_0027s-Indentation)

The `@exdent` command removes any indentation a line might have.
The command is written at the beginning of a line and applies only to
the text that follows the command that is on the same line. Do not use
braces around the text. The text on an `@exdent` line is also
printed in the roman font where the output format allows this.

`@exdent` is usually used within examples. Thus,

```
@example
This line follows an @@example command.
@exdent This line is exdented.
This line follows the exdented line.
The @@end example comes on the next line.
@end example
```

produces

```
This line follows an @example command.
```

```
This line is exdented.
```

```
This line follows the exdented line.
The @end example comes on the next line.
```

In practice, the `@exdent` command is rarely used. Usually, you
un-indent text by ending the example and returning the page to its
normal width.

`@exdent` does not have an effect in all output formats.

---

Next: [`@raggedright`: Ragged Right Text](#g_t_0040raggedright), Previous: [`@exdent`: Undoing a Line’s Indentation](#g_t_0040exdent), Up: [Quotations and Examples](#Quotations-and-Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]
