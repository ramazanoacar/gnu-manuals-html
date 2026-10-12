### 19.4 Customization Variables [¶](#Customization-Variables-1)

> **Warning:** These customization variable names and meanings may change in any
> Texinfo release. We always try to avoid incompatible changes, but we
> cannot absolutely promise, since needs change over time.

Many aspects of the behavior and output of `texi2any` may be
modified by modifying so-called *customization variables*. These
fall into a few general categories:

* Those associated with @-commands; for example,
  `@documentlanguage`.
* Those associated with command-line options; for example, the
  customization variable `SPLIT` is associated with the
  `--split` command-line option, and `TEXINFO_OUTPUT_FORMAT`
  allows specifying the output format.
* Those associated with customizing the HTML output.
* Other ad hoc variables.

Customization variables may set on the command line using
`--set-customization-variable 'var value'` (quoting
the variable/value pair to the shell) or
`--set-customization-variable var=value` (using
`=`). A special value is ‘`undef`’, which sets the
variable to this special “undefined” Perl value.

The sections below give the details for each of these.

* [Customization Variables for @-Commands](#Customization-Variables-for-_0040_002dCommands)
* [Customization Variables and Options](#Customization-Variables-and-Options)
* [HTML Customization Variables](#HTML-Customization-Variables)
* [MathJax Customization Variables](#MathJax-Customization-Variables)
* [`latex2html` Customization Variables](#latex2html-Customization-Variables)
* [`tex4ht` Customization Variables](#tex4ht-Customization-Variables)
* [LaTeX Customization Variables](#LaTeX-Customization-Variables)
* [Other Customization Variables](#Other-Customization-Variables)

---

Next: [Customization Variables and Options](#Customization-Variables-and-Options), Up: [Customization Variables](#Customization-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 19.4.1 Customization Variables for @-Commands [¶](#Customization-Variables-for-_0040_002dCommands-1)

Each of the following @-commands has an associated customization
variable with the same name (minus the leading `@`):

```
@afivepaper            @afourpaper        @afourlatex
@afourwide             @allowcodebreaks   @bsixpaper
@contents              @clickstyle        @codequotebacktick
@codequoteundirected   @deftypefnnewline  @documentdescription
@documentencoding      @documentlanguage  @evenfooting
@evenfootingmarks      @evenheading       @evenheadingmarks
@everyfooting          @everyfootingmarks @everyheading
@everyheadingmarks     @exampleindent     @firstparagraphindent
@fonttextsize          @footnotestyle     @frenchspacing
@headings              @kbdinputstyle     @microtype
@novalidate            @oddfooting        @oddfootingmarks
@oddheading            @oddheadingmarks   @pagesizes
@paragraphindent       @setfilename       @setchapternewpage
@shortcontents         @smallbook         @summarycontents
@urefbreakstyle        @xrefautomaticsectiontitle
```

Setting such a customization variable to a value ‘`foo`’ is similar
to executing `@cmd foo`. It is not exactly the same,
though, since any side effects of parsing the Texinfo source are not
redone. Also, some variables do not take Texinfo code when generating
particular formats, but an argument that is already formatted. This
is the case, for example, for HTML for `documentdescription`.

Note that if `texi2any` is invoked to process the file with
TeX (e.g., with the `--pdf` option), then these customization
variables may not be passed on to TeX.

---

Next: [HTML Customization Variables](#HTML-Customization-Variables), Previous: [Customization Variables for @-Commands](#Customization-Variables-for-_0040_002dCommands), Up: [Customization Variables](#Customization-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 19.4.2 Customization Variables and Options [¶](#Customization-Variables-and-Options-1)

The following table gives the customization variables associated with
some command line options. See [Invoking `texi2any` from a Shell](#Invoking-texi2any), for the
meaning of the options.

| Option | Variable |
| --- | --- |
| `--enable-encoding` | `ENABLE_ENCODING` |
| `--document-language` | `documentlanguage` |
| `--error-limit` | `ERROR_LIMIT` |
| `--fill-column` | `FILLCOLUMN` |
| `--footnote-style` | `footnotestyle` |
| `--force` | `FORCE` |
| `--internal-links` | `INTERNAL_LINKS` |
| `--macro-expand` | `MACRO_EXPAND` |
| `--headers` | `HEADERS`, `FORMAT_MENU` |
| `--no-warn` | `NO_WARN` |
| `--no-validate` | `novalidate` |
| `--number-footnotes` | `NUMBER_FOOTNOTES` |
| `--number-sections` | `NUMBER_SECTIONS` |
| `--node-files` | `NODE_FILES` |
| `--output` | `OUTFILE`, `SUBDIR` |
| `--paragraph-indent` | `paragraphindent` |
| `--silent` | `SILENT` |
| `--split` | `SPLIT` |
| `--split-size` | `SPLIT_SIZE` |
| `--transliterate-file-names` | `TRANSLITERATE_FILE_NAMES` |
| `--verbose` | `VERBOSE` |

Setting such a customization variable to a value ‘`foo`’ is
essentially the same as specifying the `--opt=foo` if the
option takes an argument, or `--opt` if not.

In addition, the customization variable `TEXINFO_OUTPUT_FORMAT`
allows specifying what `texi2any` outputs, either one of the usual
output formats that can be specified with options, or various other
forms:

‘`docbook`’

‘`dvi`’

‘`dvipdf`’

‘`epub3`’

‘`html`’

‘`info`’

‘`pdf`’

‘`plaintext`’

‘`ps`’

‘`xml`’
:   These correspond to the command-line options (and
    `TEXINFO_OUTPUT_FORMAT` environment variable values) of the same
    name. See [Invoking `texi2any` from a Shell](#Invoking-texi2any).

‘`debugtree`’ [¶](#index-tree-representation_002c-for-debugging)
:   Instead of generating a regular output format, output a text representation
    of the tree obtained by parsing the input texinfo document.

‘`parse`’
:   Do only Texinfo source parsing; there is no output.

‘`plaintexinfo`’
:   Output the Texinfo source with all the macros, `@include` and
    `@value{}` expanded. This is similar to setting
    `--macro-expand`, but instead of being output in addition to
    the normal conversion, output of Texinfo is the main output.

‘`rawtext`’ [¶](#index-raw-text-output)
:   Output raw text, with minimal formatting. For example, footnotes are
    ignored and there is no paragraph filling. This is used by the parser
    for file names and copyright text in HTML comments, for example.

‘`structure`’
:   Do only Texinfo source parsing and determination of the document
    structure; there is no output.

‘`texinfosxml`’ [¶](#index-SXML-output)
:   Output the document in TexinfoSXML representation, a syntax for
    writing XML data using Lisp S-expressions.

‘`textcontent`’ [¶](#index-spell-checking)
:   Output the text content only, stripped of commands; this is useful for
    spell checking or word counting, for example. The trivial
    `detexinfo` script setting this is in the `util` directory
    of the Texinfo source as an example. It’s one line:

    ```
    exec texi2any -c TEXINPUT_OUTPUT_FORMAT=textcontent "$@"
    ```

---

Next: [MathJax Customization Variables](#MathJax-Customization-Variables), Previous: [Customization Variables and Options](#Customization-Variables-and-Options), Up: [Customization Variables](#Customization-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 19.4.3 HTML Customization Variables [¶](#HTML-Customization-Variables-1)

This table gives the customization variables which apply to HTML
output only. A few other customization variables apply to both HTML
and other output formats; see [Other Customization Variables](#Other-Customization-Variables).

`AVOID_MENU_REDUNDANCY` [¶](#index-AVOID_005fMENU_005fREDUNDANCY)
:   If set, and the menu entry and menu description are the
    same, then do not print the menu description; default false.

`AFTER_BODY_OPEN` [¶](#index-AFTER_005fBODY_005fOPEN)
:   If set, the corresponding text will appear at the
    beginning of each HTML file; default unset.

`AFTER_SHORT_TOC_LINES` [¶](#index-AFTER_005fSHORT_005fTOC_005fLINES)

`AFTER_TOC_LINES` [¶](#index-AFTER_005fTOC_005fLINES)
:   If set, the corresponding text is output after the short
    table of contents for `AFTER_SHORT_TOC_LINES` and after the table of
    contents for `AFTER_TOC_LINES`; otherwise, a default string is
    used. At the time of writing, a `</div>` element is closed.

    In general, you should set `BEFORE_SHORT_TOC_LINES` if
    `AFTER_SHORT_TOC_LINES` is set, and you should set
    `BEFORE_TOC_LINES` if `AFTER_TOC_LINES` is set.

`BASEFILENAME_LENGTH` [¶](#index-BASEFILENAME_005fLENGTH)
:   The maximum length of a base file name; default 245.
    Changing this would make cross-manual references to such long node
    names invalid (see [HTML Cross-reference Link Basics](#HTML-Xref-Link-Basics)).

`BEFORE_SHORT_TOC_LINES` [¶](#index-BEFORE_005fSHORT_005fTOC_005fLINES)

`BEFORE_TOC_LINES` [¶](#index-BEFORE_005fTOC_005fLINES)
:   If set, the corresponding text is output before the short
    table of contents for `BEFORE_SHORT_TOC_LINES` and before the table of
    contents for `BEFORE_TOC_LINES`, otherwise a default string is
    used. At the time of writing, a `<div ...>` element is opened.

    In general you should set `AFTER_SHORT_TOC_LINES` if
    `BEFORE_SHORT_TOC_LINES` is set, and you should set
    `AFTER_TOC_LINES` if `BEFORE_TOC_LINES` is set.

`BIG_RULE` [¶](#index-BIG_005fRULE)
:   Rule used after and before the top element and before
    special elements, but not for footers and headers; default
    `<hr>`.

`BODYTEXT` [¶](#index-BODYTEXT)
:   The text appearing in `<body>`. By default, sets the
    HTML `lang` attribute to the document language
    (see [`@documentlanguage ll[_cc]`: Set the Document Language](#g_t_0040documentlanguage)).

`CASE_INSENSITIVE_FILENAMES` [¶](#index-CASE_005fINSENSITIVE_005fFILENAMES)
:   Construct output file names as if the filesystem were case
    insensitive (see [HTML Splitting](#HTML-Splitting)); default false.

`CHAPTER_HEADER_LEVEL` [¶](#index-CHAPTER_005fHEADER_005fLEVEL)
:   Header formatting level used for chapter level sectioning
    commands; default ‘`2`’.

`CHECK_HTMLXREF` [¶](#index-CHECK_005fHTMLXREF)
:   Check that manuals which are the target of external
    cross-references (see [`@xref` with Four and Five Arguments](#Four-and-Five-Arguments)) are present in
    `htmlxref.cnf` (see [HTML Cross-reference Configuration: `htmlxref.cnf`](#HTML-Xref-Configuration)); default false.

`COMPLEX_FORMAT_IN_TABLE` [¶](#index-COMPLEX_005fFORMAT_005fIN_005fTABLE)
:   If set, use tables for indentation of complex formats; default
    false.

`CONTENTS_OUTPUT_LOCATION` [¶](#index-CONTENTS_005fOUTPUT_005fLOCATION)
:   If set to ‘`after_top`’, output the contents at the end of the `@top`
    section. If set to ‘`inline`’, output the contents where the
    `@contents` and similar @-commands are located. If set to
    ‘`separate_element`’ output the contents in separate elements, either at the
    end of the document if not split, or in a separate file. If set to
    ‘`after_title`’ the tables of contents are output after the title; default
    ‘`after_top`’.

`CONVERT_TO_LATEX_IN_MATH` [¶](#index-CONVERT_005fTO_005fLATEX_005fIN_005fMATH)
:   If set, try to convert any Texinfo @-commands inside `@math` and
    `@displaymath` to LaTeX, before converting the `@math` or
    `@displaymath` to HTML. Default `undef`. If undefined,
    set if `HTML_MATH` is set.

`COPIABLE_LINKS` [¶](#index-COPIABLE_005fLINKS)
:   If set, output copiable links for the definition commands
    (see [Definition Commands](#Definition-Commands)) and table commands (see [Making a Two-column Table](#Two_002dcolumn-Tables)) where an index entry is defined. A link appears as a ‘¶’
    sign that appears when you hover the mouse pointer over the heading text.

`DATE_IN_HEADER` [¶](#index-DATE_005fIN_005fHEADER)
:   Put the document generation date in the header; off by default.

`DEF_TABLE` [¶](#index-DEF_005fTABLE)
:   If set, a `<table>` construction for `@deffn`
    and similar @-commands is used (looking more like the TeX output),
    instead of definition lists; default false.

`DEFAULT_RULE` [¶](#index-DEFAULT_005fRULE)
:   Rule used between element, except before and after the
    top element, and before special elements, and for footers and headers;
    default `<hr>`.

`DO_ABOUT` [¶](#index-DO_005fABOUT)
:   If set to 0 never do an About special element;
    if set to 1 always do an About special element;
    default 0.

`EXTERNAL_CROSSREF_SPLIT` [¶](#index-EXTERNAL_005fCROSSREF_005fSPLIT)
:   For cross-references to other manuals, this determines if the other
    manual is considered to be split or monolithic. By default, it is set
    based on the value of `SPLIT`. See [HTML Cross-references](#HTML-Xref), and see [HTML Cross-reference Configuration: `htmlxref.cnf`](#HTML-Xref-Configuration).

`EXTERNAL_DIR` [¶](#index-EXTERNAL_005fDIR)
:   Base directory for external manuals; default none. It is
    better to use the general external cross-reference mechanism
    (see [HTML Cross-reference Configuration: `htmlxref.cnf`](#HTML-Xref-Configuration)) than this variable.

`EXTERNAL_CROSSREF_EXTENSION` [¶](#index-EXTERNAL_005fCROSSREF_005fEXTENSION)
:   File extension for cross-references to other manuals. If unset,
    based on `EXTENSION`.

`EXTRA_HEAD` [¶](#index-EXTRA_005fHEAD)
:   Additional text appearing within `<head>`; default unset.

`FOOTNOTE_END_HEADER_LEVEL` [¶](#index-FOOTNOTE_005fEND_005fHEADER_005fLEVEL)
:   Header formatting level used for the footnotes header with
    the ‘end’ footnotestyle; default ‘`4`’. See [Footnote Styles](#Footnote-Styles).

`FOOTNOTE_SEPARATE_HEADER_LEVEL` [¶](#index-FOOTNOTE_005fSEPARATE_005fHEADER_005fLEVEL)
:   Header formatting level used for the footnotes header with
    the ‘separate’ footnotestyle; default ‘`4`’. See [Footnote Styles](#Footnote-Styles).

`FRAMES` [¶](#index-FRAMES)
:   If set, a file describing the frame layout is generated,
    together with a file with the short table of contents; default false.

`FRAMESET_DOCTYPE` [¶](#index-FRAMESET_005fDOCTYPE)
:   Same as DOCTYPE, but for the file containing the frame
    description.

`HEADER_IN_TABLE` [¶](#index-HEADER_005fIN_005fTABLE)
:   Use tables for header formatting rather than a simple
    `<div>` element; default false.

`HTML_MATH` [¶](#index-HTML_005fMATH)
:   Method to use to render `@math`. This can be unset, set to
    ‘`mathjax`’ (see [MathJax Customization Variables](#MathJax-Customization-Variables)),
    set to ‘`l2h`’, which uses `latex2html`
    (see [`latex2html` Customization Variables](#latex2html-Customization-Variables)), or set to
    ‘`t4h`’, which uses `tex4ht`
    (see [`tex4ht` Customization Variables](#tex4ht-Customization-Variables)). In the default case,
    setting `HTML_MATH` also sets `CONVERT_TO_LATEX_IN_MATH`.

`HTML_ROOT_ELEMENT_ATTRIBUTES` [¶](#index-HTML_005fROOT_005fELEMENT_005fATTRIBUTES)
:   Use that string for the `<html>` HTML document root element.
    Default undefined.

`HTMLXREF_FILE` [¶](#index-HTMLXREF_005fFILE)
:   Set the file name used for cross-references to other manuals. If
    not defined, `htmlxref.cnf` is used (see [HTML Cross-reference Configuration: `htmlxref.cnf`](#HTML-Xref-Configuration)).
    Not defined in the default case. If `TEST` is set, `HTMLXREF_MODE`
    is set to the default and `HTMLXREF_FILE` is not defined, information on
    cross-references to other manuals is not used.

    If `HTMLXREF_MODE` is set to ‘`file`’ the file name is directly used
    as the source of information, otherwise the file name is searched for in
    directories, and all the files found are used (see [HTML Cross-reference Configuration: `htmlxref.cnf`](#HTML-Xref-Configuration)).

`HTMLXREF_MODE` [¶](#index-HTMLXREF_005fMODE)
:   How cross-references to other manuals information is determined.
    If set to ‘`none`’, no information is used. If set to ‘`file`’,
    the information is determined from a file path, `htmlxref.cnf`
    in the default case, or the value of `HTMLXREF_FILE`. If not defined
    (the default) or set to any other value, search in
    directories and use all the files (see [HTML Cross-reference Configuration: `htmlxref.cnf`](#HTML-Xref-Configuration)).

`ICONS` [¶](#index-ICONS)
:   Use icons for the navigation panel; default false.

`IMAGE_LINK_PREFIX` [¶](#index-IMAGE_005fLINK_005fPREFIX)
:   If set, the associated value is prepended to the image file
    links; default unset.

`INDEX_ENTRY_COLON` [¶](#index-INDEX_005fENTRY_005fCOLON)
:   Symbol used between the index entry and the associated node or section;
    default ‘`:`’.

`INFO_JS_DIR` [¶](#index-INFO_005fJS_005fDIR)
:   (Experimental.) Add a JavaScript browsing interface to the manual.
    The value of the variable is the directory to place the code for this
    interface, so you would run the program as e.g. ‘`texi2any --html
    -c INFO_JS_DIR=js manual.texi`’ to place files in a ‘`js`’
    directory under the output. This provides some of the functionality
    of the Info browsers in a web browser, such as keyboard navigation
    and index lookup. This only works with non-split HTML output.

    The interface should provide an acceptable fallback in functionality
    if JavaScript or web browser features are not available. However,
    please be cautious when using this option, in case you do make your
    documentation harder to access for some of your users.

`IGNORE_REF_TO_TOP_NODE_UP` [¶](#index-IGNORE_005fREF_005fTO_005fTOP_005fNODE_005fUP)
:   Ignore references to `TOP_NODE_UP`, the up node for the Top node.

`INLINE_CSS_STYLE` [¶](#index-INLINE_005fCSS_005fSTYLE)
:   Put CSS directly in HTML elements rather than at the
    beginning of the output; default false.

`JS_WEBLABELS` [¶](#index-JS_005fWEBLABELS)

`JS_WEBLABELS_FILE` [¶](#index-JS_005fWEBLABELS_005fFILE)
:   Specify how to use a *JavaScript license web labels* page to
    give licensing information and source code for any JavaScript used
    in the HTML files for the manual.
    (See <https://www.gnu.org/licenses/javascript-labels.html>).

    With the value ‘`generate`’ (the default), generate a labels page
    at `JS_WEBLABELS_FILE`, and link to it in the HTML output files.
    Only do this if actually referencing JavaScript files (either with
    `HTML_MATH` set to ‘`mathjax`’, or when using the experimental
    JS browsing interface when `INFO_JS_DIR` is set). With this
    setting, `JS_WEBLABELS_FILE` must be a relative file name.

    With the value ‘`reference`’, link to the labels
    file given by `JS_WEBLABELS_FILE` in the output, and do not
    generate a labels file. This setting is useful if you separately
    maintain a single labels file for a larger website that includes
    your manual.

    With ‘`omit`’, neither generate nor link to a labels file.

`MAX_HEADER_LEVEL` [¶](#index-MAX_005fHEADER_005fLEVEL)
:   Maximum header formatting level used (higher header
    formatting level numbers correspond to lower sectioning levels);
    default ‘`4`’.

`MENU_ENTRY_COLON` [¶](#index-MENU_005fENTRY_005fCOLON)
:   Symbol used between the menu entry and the description; default
    ‘`:`’.

`MENU_SYMBOL` [¶](#index-MENU_005fSYMBOL)
:   Symbol used in front of menu entries when node names are used
    for menu entries formatting; default is undefined and set to
    `&bull;` if `USE_NUMERIC_ENTITY` is not set, and to
    `&#8217;` if set.

`MONOLITHIC` [¶](#index-MONOLITHIC)
:   Output only one file including the table of contents. Set
    by default, but only relevant when the output is not split.

`NO_CSS` [¶](#index-NO_005fCSS)
:   Do not use CSS; default false. See [HTML CSS](#HTML-CSS).

`NO_CUSTOM_HTML_ATTRIBUTE` [¶](#index-NO_005fCUSTOM_005fHTML_005fATTRIBUTE)
:   Do not output HTML with custom attributes in elements; default false.

`NO_NUMBER_FOOTNOTE_SYMBOL` [¶](#index-NO_005fNUMBER_005fFOOTNOTE_005fSYMBOL)
:   Symbol used for footnotes if `NUMBER_FOOTNOTES` is false.
    Default is `*`.

`NODE_NAME_IN_INDEX` [¶](#index-NODE_005fNAME_005fIN_005fINDEX)
:   If true, use node names in index entries, otherwise prefer section names.
    If undefined, use `USE_NODES` value in HTML. Default is undefined.

`PRE_BODY_CLOSE` [¶](#index-PRE_005fBODY_005fCLOSE)
:   If set, the given text will appear at the footer of each
    HTML file; default unset.

`PROGRAM_NAME_IN_ABOUT` [¶](#index-PROGRAM_005fNAME_005fIN_005fABOUT)
:   Used when an About element is output. If set, output the program
    name and miscellaneous related information in About special element;
    default false.

`PROGRAM_NAME_IN_FOOTER` [¶](#index-PROGRAM_005fNAME_005fIN_005fFOOTER)
:   If set, output the program name and miscellaneous related
    information in the page footers; default false.

`SECTION_NAME_IN_TITLE` [¶](#index-SECTION_005fNAME_005fIN_005fTITLE)
:   If set, when output is split, use the argument of the chapter
    structuring command (e.g., `@chapter` or `@section`)
    in the `<title>` instead of the argument to `@node`.

`SHORT_TOC_LINK_TO_TOC` [¶](#index-SHORT_005fTOC_005fLINK_005fTO_005fTOC)
:   If set, the cross-references in the Short table of contents links to the
    corresponding Table of Contents entries, if a Table of Contents is output;
    default true.

`SHOW_BUILTIN_CSS_RULES` [¶](#index-SHOW_005fBUILTIN_005fCSS_005fRULES)
:   Output the built-in default CSS rules on the standard output and exit.

`SHOW_TITLE` [¶](#index-SHOW_005fTITLE)
:   If set, output the title at the beginning of the document;
    default ‘`undef`’. If set to ‘`undef`’, setting
    `NO_TOP_NODE_OUTPUT` also sets `SHOW_TITLE` for HTML.

`SIMPLE_MENU` [¶](#index-SIMPLE_005fMENU)
:   If set, use a simple preformatted style for the menu,
    instead of breaking down the different parts of the menu; default false.
    See [The Parts of a Menu](#Menu-Parts).

`TOC_LINKS` [¶](#index-TOC_005fLINKS)
:   If set, links from headings to toc entries are created;
    default false.

`TOP_FILE` [¶](#index-TOP_005fFILE)
:   This file name may be used for the top-level file. The extension is
    set appropriately, if necessary. This is used to override the default,
    and is, in general, only taken into account when output is split, and
    for HTML.

`TOP_NODE_FILE_TARGET` [¶](#index-TOP_005fNODE_005fFILE_005fTARGET)
:   File name used for the Top node in cross-references;
    default is `index.html`.

`TOP_NODE_UP_URL` [¶](#index-TOP_005fNODE_005fUP_005fURL)
:   A URL used for Top node up references; the default is
    `undef`, in that case no Top node Up reference is generated.
    For more about the Top node pointers, see [The First Node](#First-Node). For
    overriding the Up pointer name in case `TOP_NODE_UP_URL` is set
    and for other formats, see `TOP_NODE_UP` in
    [Other Customization Variables](#Other-Customization-Variables).

`USE_ACCESSKEY` [¶](#index-USE_005fACCESSKEY)
:   Use `accesskey` in cross-references; default true.

`USE_ISO` [¶](#index-USE_005fISO)
:   Use entities for doubled single-quote characters
    (see [Inserting Quotation Marks](#Inserting-Quotation-Marks)), and ‘`---`’ and ‘`--`’
    (see [General Syntactic Conventions](#Conventions)); default true.

`USE_LINKS` [¶](#index-USE_005fLINKS)
:   Generate `<link>` elements in the HTML `<head>`
    output; default true.

`USE_NEXT_HEADING_FOR_LONE_NODE` [¶](#index-USE_005fNEXT_005fHEADING_005fFOR_005fLONE_005fNODE)
:   If set, a node not associated to a sectioning command but
    followed by a heading command not usually associated to node
    such as `@heading` before other formatted contents
    do not have its name output as a heading, under the assumption
    that the command found provides the heading. Default true.

`USE_NODE_DIRECTIONS` [¶](#index-USE_005fNODE_005fDIRECTIONS)
:   If true, use nodes to determine where next, up and prev
    link to in node headers. If false, use sections. If undefined, use
    `USE_NODES` value. Default is undefined. Note that this setting does not
    determine the link string only where the links points to, see [xrefautomaticsectiontitle](#Three-Arguments) for the link string customization. If
    nodes and sections are systematically associated, this customization has no
    practical effect.

`USE_REL_REV` [¶](#index-USE_005fREL_005fREV)
:   Use `rel` in cross-references; default true.

`USE_TITLEPAGE_FOR_TITLE` [¶](#index-USE_005fTITLEPAGE_005fFOR_005fTITLE)
:   Use the full `@titlepage` as the title, not a simple title string;
    default true. Only relevant if `SHOW_TITLE` is set.

`USE_XML_SYNTAX` [¶](#index-USE_005fXML_005fSYNTAX)
:   Use XML/XHTML compatible syntax.

`VERTICAL_HEAD_NAVIGATION` [¶](#index-VERTICAL_005fHEAD_005fNAVIGATION)
:   If set, a vertical navigation panel is used; default false.

`WORDS_IN_PAGE` [¶](#index-WORDS_005fIN_005fPAGE)
:   When output is split by nodes, specifies the approximate
    minimum page length at which a navigation panel is placed at the
    bottom of a page. To avoid ever having the navigation buttons at the
    bottom of a page, set this to a sufficiently large number. The
    default is 300.

`XREF_USE_FLOAT_LABEL` [¶](#index-XREF_005fUSE_005fFLOAT_005fLABEL)
:   If set, for the float name in cross-references, use the
    float label instead of the type followed by the float number
    (see [`@float` [type][,label]: Floating Material](#g_t_0040float)). The default is off.

`XREF_USE_NODE_NAME_ARG` [¶](#index-XREF_005fUSE_005fNODE_005fNAME_005fARG)
:   Only relevant for cross-reference commands with no cross
    reference name (second argument). If set to 1, use the node name
    (first) argument in cross-reference @-commands for the text displayed
    as the hyperlink. If set to 0, use the node name if
    `USE_NODES` is set, otherwise the section name. If set to
    ‘`undef`’, use the first argument in preformatted environments,
    otherwise use the node name or section name depending on
    `USE_NODES`. The default is ‘`undef`’.

---

Next: [`latex2html` Customization Variables](#latex2html-Customization-Variables), Previous: [HTML Customization Variables](#HTML-Customization-Variables), Up: [Customization Variables](#Customization-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 19.4.4 MathJax Customization Variables [¶](#MathJax-Customization-Variables-1)

This table lists the customization variables which can be used when
MathJax is being used, which will be the case when `HTML_MATH`
is set to ‘`mathjax`’.

`MATHJAX_SCRIPT` [¶](#index-MATHJAX_005fSCRIPT)
:   URL of the MathJax component file (e.g. `tex-svg.js`) you are using.
    `texi2any` provides a default value for this variable, but you
    are encouraged to host this file yourself on your website so that you are
    not dependent on others’ hosting.

`MATHJAX_SOURCE` [¶](#index-MATHJAX_005fSOURCE)
:   A URL of the full source code in its preferred form for modification,
    or instructions for obtaining such source code, for the component file
    named by `MATHJAX_SCRIPT`. ‘Preferred form for modification’
    means that this should not be in a ‘minified’ form. Used in the
    license labels page (see [HTML Customization Variables](#HTML-Customization-Variables), under
    `JS_WEBLABELS`).

    Again, `texi2any` provides a default value for this variable,
    but you are encouraged to host the source code for MathJax and its
    dependencies yourself. This is in order to make the source code
    available reliably, and to reduce you and your users’ dependence on
    others’ distribution systems.

---

Next: [`tex4ht` Customization Variables](#tex4ht-Customization-Variables), Previous: [MathJax Customization Variables](#MathJax-Customization-Variables), Up: [Customization Variables](#Customization-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 19.4.5 `latex2html` Customization Variables [¶](#latex2html-Customization-Variables-1)

This table lists the customization variables which can be used when
`latex2html` is being used to convert `@math`,
`@displaymath`, `@latex` and `@tex` sections for HTML.
These customization variables are relevant only if `HTML_MATH` is set to
‘`l2h`’.

To actually convert `@tex` sections, `--iftex` should be used,
and to actually convert `@latex` sections, `--iflatex` should be
used.

`L2H_CLEAN` [¶](#index-L2H_005fCLEAN)
:   If set, the intermediate files generated in relation with `latex2html`
    are removed; default true.

`L2H_FILE` [¶](#index-L2H_005fFILE)
:   If set, the given file is used as `latex2html`’s init file; default
    unset.

`L2H_HTML_VERSION` [¶](#index-L2H_005fHTML_005fVERSION)
:   The HTML version used in the `latex2html` call; default unset.

`L2H_L2H` [¶](#index-L2H_005fL2H)
:   The program invoked as `latex2html`; default is `latex2html`.

`L2H_SKIP` [¶](#index-L2H_005fSKIP)
:   If set to a true value, the actual call to `latex2html` is skipped;
    previously generated content is reused instead. If set to 0, the cache is not
    used at all. If set to ‘`undef`’, the cache is used for as many TeX
    fragments as possible and for any remaining the command is run. The default is
    ‘`undef`’.

`L2H_TMP` [¶](#index-L2H_005fTMP)
:   Set the directory used for temporary files. None of the file name components
    in this directory name may start with ‘`.`’; otherwise, `latex2html`
    will fail (because of `dvips`). The default is the empty string, which
    means the current directory.

---

Next: [LaTeX Customization Variables](#LaTeX-Customization-Variables), Previous: [`latex2html` Customization Variables](#latex2html-Customization-Variables), Up: [Customization Variables](#Customization-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 19.4.6 `tex4ht` Customization Variables [¶](#tex4ht-Customization-Variables-1)

This table lists the customization variables which can be used when
`tex4ht` is being used to convert `@math`, `@displaymath`,
`@tex` and `@latex` sections for HTML. These customization
variables are relevant only if `HTML_MATH` is set to ‘`t4h`’.

To actually convert `@tex` sections, `--iftex` should be used,
and to actually convert `@latex` sections, `--iflatex` should be
used.

`T4H_LATEX_CONVERSION` [¶](#index-T4H_005fLATEX_005fCONVERSION)
:   If set, the conversion type used for `@latex` sections. Possibilities
    are ‘`latex`’, ‘`tex`’ and ‘`texi`’. Set to ‘`latex`’ if not
    defined.

`T4H_MATH_CONVERSION` [¶](#index-T4H_005fMATH_005fCONVERSION)
:   If set, the conversion type used for `@math` and `@displymath`.
    Possibilities are ‘`latex`’, ‘`tex`’ and ‘`texi`’. Set to ‘`tex`’
    if not defined.

`T4H_TEX_CONVERSION` [¶](#index-T4H_005fTEX_005fCONVERSION)
:   If set, the conversion type used for `@tex` sections. Possibilities
    are ‘`latex`’, ‘`tex`’ and ‘`texi`’. Set to ‘`tex`’ if not
    defined.

---

Next: [Other Customization Variables](#Other-Customization-Variables), Previous: [`tex4ht` Customization Variables](#tex4ht-Customization-Variables), Up: [Customization Variables](#Customization-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 19.4.7 LaTeX Customization Variables [¶](#LaTeX-Customization-Variables-1)

|  |
| --- |
| **warning:** LaTeX output customization is experimental. Nothing is decided, everything can change, and we would welcome any feedback. |

This table gives the customization variables which apply to LaTeX output
only.

`CLASS_BEGIN_USEPACKAGE` [¶](#index-CLASS_005fBEGIN_005fUSEPACKAGE)
:   If set, the corresponding text will replace the LaTeX `\documentclass`,
    package imports that are always output and are output right after
    `\documentclass`, and package imports that depend on the document encoding
    setting the input and font encoding (`inputenc` and `fontenc`).

    The text replaced is along:

    ```
    \documentclass{book}
    \usepackage{amsfonts}
    \usepackage{amsmath}
    \usepackage[gen]{eurosym}
    \usepackage{textcomp}
    \usepackage{graphicx}
    \usepackage{etoolbox}
    \usepackage{titleps}
    \usepackage[utf8]{inputenc}
    \usepackage[T1]{fontenc}
    ```

`END_USEPACKAGE` [¶](#index-END_005fUSEPACKAGE)
:   If set, the corresponding text will replace the package imports that depend
    on the Texinfo commands used, and the last packages imports that are always
    output and output after all the other packages imports. The last package
    imports corresponds to ‘`\usepackage[hidelinks]{hyperref}`’.

    Here is an example of the corresponding text for a document with indices,
    `@need`, `@multitable`, definition commands, `@cartouche`,
    lists, and `@float`:

    ```
    \usepackage{imakeidx}
    \usepackage{needspace}
    \usepackage{array}
    \usepackage{embrac}
    \usepackage{expl3}
    \usepackage{tabularx}
    \usepackage[framemethod=tikz]{mdframed}
    \usepackage{enumitem}
    \usepackage{float}
    \usepackage[hidelinks]{hyperref}
    ```

---

Previous: [LaTeX Customization Variables](#LaTeX-Customization-Variables), Up: [Customization Variables](#Customization-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 19.4.8 Other Customization Variables [¶](#Other-Customization-Variables-1)

This table gives the remaining customization variables, which apply to
multiple formats, or affect global behavior, or otherwise don’t fit
into the categories of the previous sections.

`ASCII_DASHES_AND_QUOTES` [¶](#index-ASCII_005fDASHES_005fAND_005fQUOTES)
:   For Info output, when set, use plain ASCII characters to represent
    quotation marks, hyphens and dashes when these are given in the Texinfo
    source as ‘`-`’, ‘`--`’, ‘`---`’, ‘`` ` ``’, ‘``` `` ```’,
    ‘`'`’, and ‘`''`’, rather than UTF-8 directional quotation marks,
    en dashes, vel sim. On by default.

`ASCII_GLYPH` [¶](#index-ASCII_005fGLYPH)
:   For Info output, use ASCII output for glyph commands such as the copyright
    sign (`@copyright{}`, becoming ‘`(C)`’),
    and the bullet symbol (`@bullet{}`, becoming ‘`*`’), rather
    than other Unicode sequences. Off by default.

`ASCII_PUNCTUATION` [¶](#index-ASCII_005fPUNCTUATION)
:   Avoid any unncessary or gratuitious non-ASCII, UTF-8 sequences in the
    output. Implies both `ASCII_DASHES_AND_QUOTES` and `ASCII_GLYPH`
    and additionally affects the output of commands such as `@samp` which
    output quotation marks.

`AUTO_MENU_DESCRIPTION_ALIGN_COLUMN` [¶](#index-AUTO_005fMENU_005fDESCRIPTION_005fALIGN_005fCOLUMN)
:   For Info output, column at which to start a menu entry description
    provided by `@nodedescription` or `@nodedescriptionblock`.
    Undefined by default, in which case 45% of the fill column value is used
    (see [Invoking `texi2any` from a Shell](#Invoking-texi2any)).

`AUTO_MENU_MAX_WIDTH` [¶](#index-AUTO_005fMENU_005fMAX_005fWIDTH)
:   Maximum number of columns in a menu entry line in Info when adding a
    description from `@nodedescription` or `@nodedescriptionblock`.
    Undefined by default, in which case 10% more than the fill column value
    is used (see [Invoking `texi2any` from a Shell](#Invoking-texi2any)).

`CHECK_MISSING_MENU_ENTRY` [¶](#index-CHECK_005fMISSING_005fMENU_005fENTRY)
:   When a `@menu` block occurs in a node, check if there is a menu
    entry for all subordinate nodes in the document sectioning structure. On
    by default.

`CHECK_NORMAL_MENU_STRUCTURE` [¶](#index-CHECK_005fNORMAL_005fMENU_005fSTRUCTURE)
:   Warn if the node pointers (either explicitly or automatically set)
    are not consistent with the order of node menu entries. This is a more
    thorough structure check than that provided by
    `CHECK_MISSING_MENU_ENTRY`. Off by default.

`CLOSE_QUOTE_SYMBOL` [¶](#index-CLOSE_005fQUOTE_005fSYMBOL)
:   When a closing quote is needed, e.g. for `@samp` output, use
    this character; default `&#8217;` in DocBook.
    Undefined in the default case in HTML and set to `&rsquo;`
    if `USE_NUMERIC_ENTITY` is not set, to `&#8217;` if set, and
    to a quote character if `OUTPUT_CHARACTERS` is set and the output
    encoding includes that character.

    The default for Info is set the same as for `OPEN_QUOTE_SYMBOL`,
    except that the Unicode code is a closing quote (see below).

`CLOSE_DOUBLE_QUOTE_SYMBOL` [¶](#index-CLOSE_005fDOUBLE_005fQUOTE_005fSYMBOL)
:   When a closing double quote is needed, for ‘`@dfn`’ in Info, use this
    character. The default for Info is set the same as for
    `OPEN_DOUBLE_QUOTE_SYMBOL`, except that the Unicode code is a closing
    double quote (see below).

`COMMAND_LINE_ENCODING` [¶](#index-COMMAND_005fLINE_005fENCODING)
:   Encoding used to decode command-line arguments. Default is based on the locale
    encoding. This may affect file names inserted into output files or error
    messages printed by the program.

    Note that some file and directory names from the command line may
    not be decoded immediately, and may not be decoded at all.

`CPP_LINE_DIRECTIVES` [¶](#index-CPP_005fLINE_005fDIRECTIVES)
:   Recognize `#line` directives in a “preprocessing” pass
    (see [External Macro Processors: Line Directives](#External-Macro-Processors)); on by default.

`DEBUG` [¶](#index-DEBUG)
:   If set, debugging output is generated; default is off (zero).

`DOC_ENCODING_FOR_INPUT_FILE_NAME` [¶](#index-DOC_005fENCODING_005fFOR_005fINPUT_005fFILE_005fNAME)
:   If set, use the input Texinfo document encoding information for
    the encoding of input file names, such as file names specified as
    `@include` or `@verbatiminclude` arguments. If unset, use
    the locale encoding instead. Default is set, except on MS-Windows where
    the locale encoding is used by default.

    Note that this is for file names only; the default encoding or
    `@documentencoding` is always used for the encoding of file
    content (see [`@documentencoding enc`: Set Input Encoding](#g_t_0040documentencoding)).

    The `INPUT_FILE_NAME_ENCODING` variable overrides this variable.

`DOC_ENCODING_FOR_OUTPUT_FILE_NAME` [¶](#index-DOC_005fENCODING_005fFOR_005fOUTPUT_005fFILE_005fNAME)
:   If set, use the input Texinfo document encoding information for the
    encoding of output file names, such as files specified with `--output`.
    If unset, use the locale encoding instead. Default is unset, so files
    names are encoded using the current locale.

    Note that this is for file names only; `OUTPUT_ENCODING_NAME`
    is used for the encoding of file content.

    The `OUTPUT_FILE_NAME_ENCODING` variable overrides this variable.

`DOCTYPE` [¶](#index-DOCTYPE)
:   For DocBook, HTML, XML. Specifies the `SystemLiteral`, the
    entity’s system identifier. This is a URI which may be used to
    retrieve the entity, and identifies the canonical DTD for the
    document. The default value is different for each of HTML, DocBook
    and XML.

`DUMP_TEXI` [¶](#index-DUMP_005fTEXI)
:   For debugging. If set, no conversion is done, only parsing and macro
    expansion. If the option `--macro-expand` is set, the Texinfo
    source is also expanded to the corresponding file. Default false.

`DUMP_TREE` [¶](#index-DUMP_005fTREE)
:   For debugging. If set, the tree constructed upon parsing a Texinfo
    document is output to standard error; default false.

`EPUB_CREATE_CONTAINER_FILE` [¶](#index-EPUB_005fCREATE_005fCONTAINER_005fFILE)
:   If set to 0, do not generate the EPUB output file. Default is set
    to 1.

`EPUB_KEEP_CONTAINER_FOLDER` [¶](#index-EPUB_005fKEEP_005fCONTAINER_005fFOLDER)
:   If set, keep the directory containing the directories and files
    needed for EPUB. The EPUB output file is a ZIP archive of this
    directory. Default is not defined. Set if not defined and `TEST` or
    `DEBUG` is set. See [Container Directory and Output Files](#EPUB-Output-File-and-Directory).

`EXTENSION` [¶](#index-EXTENSION)
:   The extension added to the output file name. The default is different
    for each output format.

`FORMAT_MENU` [¶](#index-FORMAT_005fMENU)
:   If set to ‘`menu`’, output Texinfo menus. This is the default for
    Info. ‘`sectiontoc`’ is the default setting for HTML, where instead
    of the contents of `@menu` blocks being output, a list of subordinate
    sections is output in each node. If set to ‘`nomenu`’, do not output
    menus.

    This variable is set to ‘`nomenu`’ when generating DocBook, or when
    `--no-headers` is specified.

`HIGHLIGHT_SYNTAX` [¶](#index-HIGHLIGHT_005fSYNTAX)
:   If set, `@example` blocks with language information as first
    argument are highlighted in the HTML output. It is also possible to specify a
    default for the language with `HIGHLIGHT_SYNTAX_DEFAULT_LANGUAGE`. Syntax
    highlighting requires an external program to generate the highlighted HTML.
    The `HIGHLIGHT_SYNTAX` value allows to select a specific program. The
    possibilities are `highlight`, `pygments`, any other value standing
    for `source-highlight` (see [Code Examples Syntax Highlighting in HTML](#Syntax-Highlighting)).

`HIGHLIGHT_SYNTAX_DEFAULT_LANGUAGE` [¶](#index-HIGHLIGHT_005fSYNTAX_005fDEFAULT_005fLANGUAGE)
:   The default language used for syntax highlighting when there is no
    language information.

`IGNORE_SPACE_AFTER_BRACED_COMMAND_NAME` [¶](#index-IGNORE_005fSPACE_005fAFTER_005fBRACED_005fCOMMAND_005fNAME)
:   If set, spaces are ignored after an @-command that takes braces.
    Default true, matching the TeX behavior.

`INDEX_SPECIAL_CHARS_WARNING` [¶](#index-INDEX_005fSPECIAL_005fCHARS_005fWARNING)
:   If set, warn about ‘`:`’ in index entry, as not all Info readers may
    be able to process these. For Info and plaintext only. Default false,
    because parsing problems there don’t prevent navigation; readers can still
    relatively easily find their way to the node in question.

`INFO_SPECIAL_CHARS_QUOTE` [¶](#index-INFO_005fSPECIAL_005fCHARS_005fQUOTE)
:   If set, whenever there are problematic characters for Info output in
    places such as node names or menu items, surround the part of the
    construct where they appear with quoting characters, as described in
    [Info Format Specification](#Info-Format-Specification). Default is set for Info and unset
    for plaintext. See [`@node` Line Requirements](#Node-Line-Requirements).

`INFO_SPECIAL_CHARS_WARNING` [¶](#index-INFO_005fSPECIAL_005fCHARS_005fWARNING)
:   If set, warn about problematic constructs for Info output (such as the
    string ‘`::`’) in node names, menu items, and cross-references.
    If not defined, set unless `INFO_SPECIAL_CHARS_QUOTE` is set.
    Default is set for Info and not defined for plaintext. Similar warnings in
    index entries are covered by `INDEX_SPECIAL_CHARS_WARNING`.

`INPUT_FILE_NAME_ENCODING` [¶](#index-INPUT_005fFILE_005fNAME_005fENCODING)
:   Encoding used for input file names. This variable overrides
    any encoding from the document or current locale. Normally, you do
    not need to set this variable, but it can be used if file names are
    in a certain character encoding on a filesystem. An alternative is to
    set `DOC_ENCODING_FOR_INPUT_FILE_NAME` to ‘`0`’ to use the locale
    encoding. See also `OUTPUT_FILE_NAME_ENCODING`.

`LOCALE_ENCODING` [¶](#index-LOCALE_005fENCODING)
:   Locale encoding obtained from the system. You should not need to
    explicitly set this variable.

`MAX_MACRO_CALL_NESTING` [¶](#index-MAX_005fMACRO_005fCALL_005fNESTING)
:   The maximal number of recursive calls of @-commands defined through
    `@rmacro`; default 100000. The purpose of this variable is to
    avoid infinite recursions.

`MESSAGE_ENCODING` [¶](#index-MESSAGE_005fENCODING)
:   Encoding used to encode messages output by `texi2any`. Default is
    based on the locale encoding.

    It is also used for command-line argument passed to commands called from
    `texi2any`. For example, `latex2html` will be called from
    `texi2any` if `HTML_MATH` is set to ‘`l2h`’.

`NO_TOP_NODE_OUTPUT` [¶](#index-NO_005fTOP_005fNODE_005fOUTPUT)
:   If set do not output the Top node content. The Top node is still
    parsed, but the content is discarded. Not set in the default case
    for HTML. Set in the default case for EPUB. If `SHOW_TITLE`
    is ‘`undef`’, setting `NO_TOP_NODE_OUTPUT` also sets
    `SHOW_TITLE` for HTML.

    Setting `NO_TOP_NODE_OUTPUT`, which removes the Top node and adds
    a title page corresponds more to the formatting of a book. Setting
    `NO_TOP_NODE_OUTPUT` to false, with `SHOW_TITLE` remaining
    ‘`undef`’, and false, corresponds more to a document setup for browsing,
    with a direct access to the information at the Top node.

    For DocBook, `NO_TOP_NODE_OUTPUT` is set to true. Setting
    `NO_TOP_NODE_OUTPUT` to false causes the Top node content to be
    output. It is not recommended to output the Top node in DocBook as
    the title and copying informations are always output. This option
    is kept for DocBook for compatibility, as before 2022 the Top node was output
    in DocBook. It could be removed in the future.

`NO_USE_SETFILENAME` [¶](#index-NO_005fUSE_005fSETFILENAME)
:   If set, do not use `@setfilename` to set the document name;
    instead, base the output document name only on the input file name.
    The default is false.

`NODE_NAME_IN_MENU` [¶](#index-NODE_005fNAME_005fIN_005fMENU)
:   If set, use node names in menu entries, otherwise prefer section names;
    default true.

`OPEN_QUOTE_SYMBOL` [¶](#index-OPEN_005fQUOTE_005fSYMBOL)
:   When an opening quote is needed, e.g., for ‘`@samp`’ output, use
    the specified character; default `&#8216;` for DocBook.
    Undefined in the default case in HTML and set to `&lsquo;`
    if `USE_NUMERIC_ENTITY` is not set, to `&#8217;` if set, and
    to a quote character if `OUTPUT_CHARACTERS` is set and the output
    encoding includes that character.

    For Info, the default depends on the enabled document encoding. If
    `--disable-encoding` is set or the document encoding is not UTF-8,
    ‘`'`’ is used. This character usually appears
    as an undirected single quote on modern systems. Otherwise, the Info
    output uses a Unicode left quote.

`OPEN_DOUBLE_QUOTE_SYMBOL` [¶](#index-OPEN_005fDOUBLE_005fQUOTE_005fSYMBOL)
:   When an opening double quote is needed, for ‘`@dfn`’ output in Info, use
    the specified character. If `--disable-encoding` is set or the document
    encoding is not UTF-8, ‘`"`’ is used. Otherwise, the Info output uses a
    Unicode left double quote.

`OUTPUT_CHARACTERS` [¶](#index-OUTPUT_005fCHARACTERS)
:   If not set, the default, output accented and special characters in HTML, XML
    and DocBook using XML entities, and in LaTeX using macros. If set, output
    accented characters in HTML, XML, DocBook and LaTeX output and special
    characters in HTML and LaTeX output based on the document encoding.
    See [`@documentencoding enc`: Set Input Encoding](#g_t_0040documentencoding), and [Inserting Accents](#Inserting-Accents).

`OUTPUT_ENCODING_NAME` [¶](#index-OUTPUT_005fENCODING_005fNAME)
:   Normalized encoding name used for output files. Should be a usable
    charset name in HTML, typically one of the preferred IANA encoding
    names. By default, if an input encoding is set (typically through
    `@documentencoding`), this information is used to set the output
    encoding name, otherwise the output encoding is based on the
    default encoding. See [`@documentencoding enc`: Set Input Encoding](#g_t_0040documentencoding).

`OUTPUT_FILE_NAME_ENCODING` [¶](#index-OUTPUT_005fFILE_005fNAME_005fENCODING)
:   Encoding used for output file names. This variable overrides
    any encoding from the document or current locale.

    Normally, you do not need to set this variable, but it can be used if file
    names should be created in a certain character encoding on a filesystem.
    See also `INPUT_FILE_NAME_ENCODING`.

`PACKAGE` [¶](#index-PACKAGE)

`PACKAGE_VERSION` [¶](#index-PACKAGE_005fVERSION)

`PACKAGE_AND_VERSION` [¶](#index-PACKAGE_005fAND_005fVERSION)

`PACKAGE_URL` [¶](#index-PACKAGE_005fURL)

`PACKAGE_NAME` [¶](#index-PACKAGE_005fNAME)
:   The implementation’s short package name, package version, package name
    and version concatenated, package URL, and full package name,
    respectively. By default, these variables are all set through
    Autoconf, Automake, and `configure`.

`PREFIX` [¶](#index-PREFIX)
:   The output file prefix, which is prepended to some output file names.
    By default it is set by `@setfilename` or from the input file
    (see [Setting the Output File Name](#Setting-the-Output-File-Name)). How this value is used depends on the
    value of other customization variables or command line options, such
    as whether the output is split. The default is unset.

`PROGRAM` [¶](#index-PROGRAM)
:   Name of the program used. By default, it is set to the name of the
    program launched, with a trailing ‘`.pl`’ removed.

`SORT_ELEMENT_COUNT` [¶](#index-SORT_005fELEMENT_005fCOUNT)
:   If set, the name of a file to which a list of elements (nodes or
    sections, depending on the output format) is dumped, sorted by the
    number of lines they contain after removal of @-commands; default
    unset. This is used by the program `texi-elements-by-size` in
    the `util/` directory of the Texinfo source distribution
    (see [texi-elements-by-size](#texi_002delements_002dby_002dsize)).

`SORT_ELEMENT_COUNT_WORDS` [¶](#index-SORT_005fELEMENT_005fCOUNT_005fWORDS)
:   When dumping the elements-by-size file (see preceding item), use word
    counts instead of line counts; default false.

`TEST` [¶](#index-TEST)
:   If set to true, some variables which are normally dynamically
    generated anew for each run (date, program name, version) are set to
    fixed and given values. This is useful to compare the output to a
    reference file, as is done for the tests. The default is false.

`TEXI2DVI` [¶](#index-TEXI2DVI)
:   Name of the command used to produce PostScript, PDF, and DVI; default
    ‘`texi2dvi`’. See [`texi2any` Printed Output](#texi2any-Printed-Output).

`TEXI2HTML` [¶](#index-TEXI2HTML)
:   Generate HTML and try to be as compatible as possible with
    `texi2html`; default false.

`TEXINFO_DTD_VERSION` [¶](#index-TEXINFO_005fDTD_005fVERSION)
:   For XML. Version of the DTD used in the XML output preamble. The
    default is set based on a variable in `configure.ac`.

`TEXTCONTENT_COMMENT` [¶](#index-TEXTCONTENT_005fCOMMENT)
:   For stripped text content output (i.e., when
    `TEXINFO_OUTPUT_FORMAT` is set to `textcontent`). If set,
    also output comments. Default false.

`TOP_NODE_UP` [¶](#index-TOP_005fNODE_005fUP)
:   Up node for the Top node; default ‘`(dir)`’. This node name is
    supposed to be already formatted for the output format. In HTML
    can be used in attribute, so should not contain any element. Used for
    HTML output only if `TOP_NODE_UP_URL` is set to override the URL,
    see `TOP_NODE_UP_URL` in [HTML Customization Variables](#HTML-Customization-Variables).

`TREE_TRANSFORMATIONS` [¶](#index-TREE_005fTRANSFORMATIONS)
:   The associated value is a comma separated list of transformations that
    can be applied to the Texinfo tree prior to outputting the result. If
    more than one is specified, the ordering is irrelevant; each is always
    applied at the necessary point during processing.

    By default, the tree transformations ‘`move_index_entries_after_items`’
    and ‘`relate_index_entries_to_table_entries`’ are executed
    for HTML and DocBook output.
    Here’s an example of updating the master menu in a document:

    ```
    texi2any \
      -c TREE_TRANSFORMATIONS=regenerate_master_menu \
      -c TEXINFO_OUTPUT_FORMAT=plaintexinfo \
      mydoc.texi \
      -o /tmp/out
    ```

    (Caveat: Since ‘`plaintexinfo`’ output expands
    Texinfo macros and conditionals, it’s necessary to remove any such
    differences before installing the updates in the original document.
    This may be remedied in a future release.)

    The following transformations are currently supported (many are used
    in the `pod2texi` utility distributed with Texinfo;
    see [Invoking `pod2texi`: Convert Pod to Texinfo](#Invoking-pod2texi)):

    ‘`complete_tree_nodes_menus`’ [¶](#index-complete_005ftree_005fnodes_005fmenus)
    :   Add menu entries or whole menus for nodes associated with sections of
        any level, based on the sectioning tree.

    ‘`complete_tree_nodes_missing_menu`’ [¶](#index-complete_005ftree_005fnodes_005fmissing_005fmenu)
    :   Add whole menus for nodes associated with sections without menu. The
        menus are based on the sectioning tree.

    ‘`fill_gaps_in_sectioning`’ [¶](#index-fill_005fgaps_005fin_005fsectioning)
    :   Adds empty `@unnumbered...` sections in a tree to fill gaps in
        sectioning. For example, an `@unnumberedsec` will be inserted
        if a `@chapter` is followed by a `@subsection`.

    ‘`insert_nodes_for_sectioning_commands`’ [¶](#index-insert_005fnodes_005ffor_005fsectioning_005fcommands)
    :   Insert nodes for sectioning commands lacking a corresponding node.

    ‘`move_index_entries_after_items`’ [¶](#index-move_005findex_005fentries_005fafter_005fitems)
    :   In `@enumerate` and `@itemize`, move index entries
        appearing just before an `@item` to just after the
        `@item`. Comment lines between index entries are moved too. As
        mentioned, this is always done for HTML and DocBook output.

    ‘`regenerate_master_menu`’ [¶](#index-regenerate_005fmaster_005fmenu)
    :   Update the Top node master menu, either replacing the (first)
        `@detailmenu` in the Top node menu, or creating it at the end of
        the Top node menu.

    ‘`relate_index_entries_to_table_entries`’ [¶](#index-relate_005findex_005fentries_005fto_005ftable_005fentries)
    :   In `@table`, `@vtable` and `@ftable`,
        reassociate the index entry information from an index @-command
        appearing right after an `@item` line with the first element of
        the `@item`. Remove the index @-command from the tree.

    ‘`simple_menu`’ [¶](#index-simple_005fmenu)
    :   Mostly the same as `SIMPLE_MENU`: use a simple preformatted style
        for the menu. It differs from setting `SIMPLE_MENU` in that
        `SIMPLE_MENU` only has an effect in HTML output.

`USE_NODES` [¶](#index-USE_005fNODES)
:   Preferentially use nodes to decide where elements are separated. If
    set to false, preferentially use sectioning to decide where elements
    are separated. The default is true.

`USE_NUMERIC_ENTITY` [¶](#index-USE_005fNUMERIC_005fENTITY)
:   For HTML, XML and DocBook. If set, use numeric entities instead of named
    entities. By default, set to true for DocBook output.

`USE_UP_NODE_FOR_ELEMENT_UP` [¶](#index-USE_005fUP_005fNODE_005fFOR_005fELEMENT_005fUP)
:   Fill in up sectioning direction with node direction when there is no
    sectioning up direction. In practice this can only happen when there
    is no @top section. Not set by default.

`USE_SETFILENAME_EXTENSION` [¶](#index-USE_005fSETFILENAME_005fEXTENSION)
:   Default is on for Info, off for other output. If set, use exactly
    what `@setfilename` gives for the output file name, including
    the extension. You should not need to explicitly set this variable.

`USE_UNIDECODE` [¶](#index-USE_005fUNIDECODE)
:   If set to false, do not use the `Text::Unidecode` Perl module to
    transliterate more characters; default true.

---

Next: [Invoking `pod2texi`: Convert Pod to Texinfo](#Invoking-pod2texi), Previous: [Customization Variables](#Customization-Variables), Up: [`texi2any`: The Translator for Texinfo](#Generic-Translator-texi2any)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 19.5 Internationalization of Document Strings [¶](#Internationalization-of-Document-Strings-1)

`texi2any` writes fixed strings into the output document at
various places: cross-references, page footers, the help page,
alternate text for images, and so on. The string chosen depends on
the value of the `documentlanguage` at the time of the string
being output (see [`@documentlanguage ll[_cc]`: Set the Document Language](#g_t_0040documentlanguage), for the Texinfo
command interface).

The Gettext framework is used for those strings (see [Gettext](https://www.gnu.org/software/gettext/manual/gettext.html#Top)). The `libintl-perl` package is used as the
`gettext` implementation; more specifically, the pure Perl
implementation is used, so Texinfo can support consistent behavior
across all platforms and installations, which would not otherwise be
possible. `libintl-perl` is included in the Texinfo distribution
and always installed, to ensure that it is available if needed. It is
also possible to use the system `gettext` (the choice can be made
at build-time).

The Gettext domain ‘`texinfo_document`’ is used for the strings.
Translated strings are written as Texinfo, and may include
@-commands. In translated strings, the varying parts of the string
are not usually denoted by `%s` and the like, but by
‘`{arg_name}`’. (This convention is common for `gettext` in
Perl and is fully supported in GNU Gettext; see [Perl
Format Strings](https://www.gnu.org/software/gettext/manual/gettext.html#perl_002dformat) in GNU Gettext.) For example, in the
following, ‘`{section}`’ will be replaced by the section name:

```
see {section}
```

These Perl-style brace format strings are used for two reasons: first,
changing the order of `printf` arguments is only available since
Perl 5.8.0; second, and more importantly, the order of arguments
is unpredictable, since @-command expansion may lead to different
orders depending on the output format.

The expansion of a translation string is done like this:

1. First, the string is translated. The locale
   is documentlanguage`.`documentencoding.

   If the documentlanguage has the form ‘`ll_CC`’, that is
   tried first, and then just ‘`ll`’.

   To cope with the possibility of having multiple encodings, a
   special use of the `us-ascii` locale encoding is also possible.
   If the ‘`ll`’ locale in the current encoding does not exist, and the
   encoding is not `us-ascii`, then `us-ascii` is tried.

   The idea is that if there is a `us-ascii` encoding, it means that
   all the characters in the charset may be expressed as @-commands.
   For example, there is a `fr.us-ascii` locale that can accommodate
   any encoding, since all the Latin 1 characters have associated
   @-commands. On the other hand, Japanese has only a translation
   `ja.utf-8`, since there are no @-commands for Japanese
   characters.

   The `us-ascii` locales are not needed much now that
   UTF-8 is used for most documents. Note that accented characters
   are required to be expressed as @-commands in the `us-ascii` locales,
   which may be inconvenient for translators.
2. Next, the string is expanded as Texinfo, and converted.
   The arguments are substituted; for example, ‘`{arg_name}`’ is
   replaced by the corresponding actual argument.

In the following example, ‘`{date}`’, ‘`{program_homepage}`’
and ‘`{program}`’ are the arguments of the string. Since they
are used in `@uref`, their order is not predictable.
‘`{date}`’, ‘`{program_homepage}`’ and ‘`{program}`’ are
substituted after the expansion:

```
Generated on @emph{{date}} using
@uref{{program_homepage}, @emph{{program}}}.
```

This approach is admittedly a bit complicated. Its usefulness is that
it supports having translations available in different encodings for
encodings which can be covered by @-commands, and also specifying how
the formatting for some commands is done, independently of the output
format—yet still be language-dependent. For example, the
‘`@pxref`’ translation string can be like this:

```
see {node_file_href} section `{section}' in @cite{{book}}
```

which allows for specifying a string independently of the output
format, while nevertheless with rich formatting it may be translated
appropriately in many languages.

---

Next: [`texi2html`: Ancestor of `texi2any`](#texi2html), Previous: [Internationalization of Document Strings](#Internationalization-of-Document-Strings), Up: [`texi2any`: The Translator for Texinfo](#Generic-Translator-texi2any)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 19.6 Invoking `pod2texi`: Convert Pod to Texinfo [¶](#Invoking-pod2texi_003a-Convert-Pod-to-Texinfo)

The `pod2texi` program translates Perl Pod documentation file(s)
to Texinfo. There are two basic modes of operation: generating a
standalone manual from each input Pod, or (if `--base-level=1` or
higher is given) generating Texinfo subfiles suitable for use
with `@include`.

The `pod2texi` program may be useful outside of the rest of Texinfo;
thus, the invocation of `pod2texi` is documented in the Pod language
using the man page format to follow the convention used in Perl standalone
programs, with a version on the web
<http://www.gnu.org/software/texinfo/manual/pod2texi.html> and a version
included below. The version included in the manual is also an example of
`pod2texi` use, as it is converted from Pod using `pod2texi`.

* [pod2texi](#pod2texi-manual-page)

---

Up: [Invoking `pod2texi`: Convert Pod to Texinfo](#Invoking-pod2texi)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 19.6.1 pod2texi [¶](#pod2texi-1)

#### pod2texi NAME [¶](#pod2texi-NAME-1)

pod2texi - convert Pod to Texinfo

#### pod2texi SYNOPSIS [¶](#pod2texi-SYNOPSIS-1)

```
  pod2texi [OPTION]... POD...
```

#### pod2texi DESCRIPTION [¶](#pod2texi-DESCRIPTION-1)

Translate Pod file(s) to Texinfo. There are two basic modes of
operation. First, by default, each Pod is translated to a standalone
Texinfo manual.

Second, if `--base-level` is set higher than 0, each Pod is translated
to a file suitable for `@include`, and one more file with a main menu
and all the `@include` is generated.

#### pod2texi OPTIONS [¶](#pod2texi-OPTIONS-1)

**--appendix-sections**
:   Use appendix sectioning commands (`@appendix`, ...) instead of the
    default numbered sectioning Texinfo @-commands (`@chapter`,
    `@section`, ...).

**--base-level**=*NUM|NAME*
:   Sets the level of the `head1` commands. It may be an integer or a
    Texinfo sectioning command (without the `@`): 1 corresponds to the
    `@chapter`/`@unnumbered` level, 2 to the `@section` level, and so on.
    The default is 0, meaning that `head1` commands are still output as
    chapters, but the output is arranged as a standalone manual.

    If the level is not 0, the Pod file is rendered as a fragment of a
    Texinfo manual suitable for `@include`. In this case, each Pod file
    has an additional sectioning command covering the entire file, one level
    above the `--base-level` value. Therefore, to make each Pod file a
    chapter in a large manual, you should use `section` as the base level.

    For an example of making Texinfo out of the Perl documentation itself,
    see `contrib/perldoc-all` in the Texinfo source distribution.

**--debug**=*NUM*
:   Set debugging level to *NUM*.

**--headings-as-sections**
:   Use headings commands (`@heading`, ...) instead of the
    default numbered sectioning Texinfo @-commands (`@chapter`,
    `@section`, ...). The sectioning command covering the entire
    file output for each Pod file if **--base-level** is not 0 is a
    numbered command.

**--help**
:   Display help and exit.

**--menus**
:   Output node menus. If there is a main manual, its Top node menu
    is always output, since a master menu is generated. Other nodes
    menus are not output in the default case.

**--output**=*NAME*
:   Name for the first manual, or the main manual if there is a main manual.
    Default is to write to standard output.

**--no-section-nodes**
:   Use anchors for sections instead of nodes.

**--no-fill-section-gaps**
:   Do not fill sectioning gaps with empty `@unnumbered` files.
    Ordinarily, it’s good to keep the sectioning hierarchy intact.

**--preamble**=*STR*
:   Insert *STR* as top boilerplate before menu and includes. If *STR* is
    set to `-`, read the top boilerplate from the standard input. The default top
    boilerplate is a minimal beginning for a Texinfo document.

**--setfilename**=*STR*
:   Use *STR* in top boilerplate before menu and includes for `@setfilename`.
    No `@setfilename` is output in the default case.

**--subdir**=*NAME*
:   If there is a main manual with include files (each corresponding to
    an input Pod file), then those include files are put in directory *NAME*.

**--unnumbered-sections**
:   Use unnumbered sectioning commands (`@unnumbered`, ...) instead of the
    default numbered sectioning Texinfo @-commands (`@chapter`,
    `@section`, ...).

**--top**=*TOP*
:   Name of the `@top` element for the main manual. May contain Texinfo code.

**--version**
:   Display version information and exit.

#### pod2texi SEE ALSO [¶](#pod2texi-SEE-ALSO-1)

[(Pod-Simple-Texinfo)](Pod-Simple-Texinfo.html#Top). [(perlpod)](perlpod.html#Top). The Texinfo manual.
Texinfo home page: <http://www.gnu.org/software/texinfo/>

#### pod2texi COPYRIGHT AND LICENSE [¶](#pod2texi-COPYRIGHT-AND-LICENSE-1)

Copyright 2012-2023 Free Software Foundation, Inc.

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 3 of the License,
or (at your option) any later version.

There is NO WARRANTY, to the extent permitted by law.

#### pod2texi AUTHOR [¶](#pod2texi-AUTHOR-1)

Patrice Dumas <bug-texinfo@gnu.org>.

---

Previous: [Invoking `pod2texi`: Convert Pod to Texinfo](#Invoking-pod2texi), Up: [`texi2any`: The Translator for Texinfo](#Generic-Translator-texi2any)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 19.7 `texi2html`: Ancestor of `texi2any` [¶](#texi2html_003a-Ancestor-of-texi2any)

Conceptually, the `texi2html` program is the parent of today’s
`texi2any` program. `texi2html` was developed
independently, originally by Lionel Cons in 1998; at the time,
`makeinfo` could not generate HTML. Many other people
contributed to `texi2html` over the years.

The present `texi2any` uses little of the actual code of
`texi2html`, and has quite a different basic approach to the
implementation (namely, parsing the Texinfo document into a tree), but
still, there is a family resemblance.

By design, `texi2any` supports nearly all the features of
`texi2html` in some way. However, we did not attempt to
maintain strict compatibility, so no `texi2html` executable is
installed by the Texinfo package. An approximation can be run with an
invocation like this:

```
texi2any --set-customization-variable TEXI2HTML=1 ...
```

but, to emphasize, this is *not* a drop-in replacement
for the previous `texi2html`. Here are the biggest differences:

* Most blatantly, the command line options of `texi2html`
  are now customization variables, for the most part. A table of
  approximate equivalents is given below.
* The program-level customization API is very different in
  `texi2any`.
* Indices cannot be split.

We do not intend to reimplement these
differences. Therefore, the route forward for authors is alter
manuals and build processes as necessary to use the new features and
methods of `texi2any`. The `texi2html` maintainers
(one of whom is the principal author of `texi2any`) do not
intend to make further releases.

Here is the table showing `texi2html` options and
corresponding `texi2any` customization variables.

|  |  |
| --- | --- |
| `--toc-links` | `TOC_LINKS` |
| `--short-ext` | `EXTENSION` set to ‘`htm`’ |
| `--prefix` | `PREFIX` |
| `--def-table` | `DEF_TABLE` |
| `--html-xref-prefix` | `EXTERNAL_DIR` |
| `--l2h` | `HTML_MATH` set to ‘`l2h`’ |
| `--l2h-l2h` | `L2H_L2H` |
| `--l2h-skip` | `L2H_SKIP` |
| `--l2h-tmp` | `L2H_TMP` |
| `--l2h-file` | `L2H_FILE` |
| `--l2h-clean` | `L2H_CLEAN` |
| `--use-nodes` | `USE_NODES` |
| `--monolithic` | `MONOLITHIC` |
| `--top-file` | `TOP_FILE` |
| `--frames` | `FRAMES` |
| `--menu` | `FORMAT_MENU` |
| `--debug` | `DEBUG` |
| `--doctype` | `DOCTYPE` |
| `--frameset-doctype` | `FRAMESET_DOCTYPE` |
| `--test` | `TEST` |

Finally, any `texi2html` users seeking more detailed information can
check the first part of the archived file `doc/texi2oldapi.texi` in the
Texinfo source repository.

---

Next: [Generating HTML](#Generating-HTML), Previous: [`texi2any`: The Translator for Texinfo](#Generic-Translator-texi2any), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 20 Creating and Installing Info Files [¶](#Creating-and-Installing-Info-Files-1)

This chapter gives some information on the Info output and describes how to
install Info files. For the creation of Info files with `texi2any`, see
[`texi2any`: The Translator for Texinfo](#Generic-Translator-texi2any), and with Emacs, [Formatting for Info](#Info-Formatting). See [Info Files](#Info-Files), for general information about the file format.
See [Info Format Specification](#Info-Format-Specification), for a detailed technical specification of the
Info format.

* [Installing an Info File](#Installing-an-Info-File)
* [Tag Files and Split Files](#Tag-and-Split-Files)
* [Info Format FAQ](#Info-Format-FAQ)

---

Next: [Tag Files and Split Files](#Tag-and-Split-Files), Up: [Creating and Installing Info Files](#Creating-and-Installing-Info-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]
