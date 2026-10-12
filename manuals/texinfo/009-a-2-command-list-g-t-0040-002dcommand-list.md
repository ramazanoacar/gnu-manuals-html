### A.2 @-Command List [¶](#g_t_0040_002dCommand-List)

Here is an alphabetical list of the @-commands in Texinfo. Square
brackets, `[` `]`, indicate optional arguments; an ellipsis,
‘`…`’, indicates repeated text.

`@whitespace`
:   An `@` followed by a space, tab, or newline produces a normal,
    stretchable, interword space. See [Multiple Spaces](#Multiple-Spaces).

`@!`
:   Produce an exclamation point that ends a sentence (usually after an
    end-of-sentence capital letter). See [Ending a Sentence](#Ending-a-Sentence).

`@"`

`@'`
:   Generate an umlaut or acute accent, respectively, over the next
    character, as in ö and ó. See [Inserting Accents](#Inserting-Accents).

`@&`

`@ampchar{}`
:   Generate an ampersand. See [Inserting ‘&’ with `@&` and `@ampchar{}`](#Inserting-an-Ampersand).

`@*`
:   Force a line break. See [`@*` and `@/`: Generate and Allow Line Breaks](#Line-Breaks).

`@,{c}`
:   Generate a cedilla accent under c, as in ç. See [Inserting Accents](#Inserting-Accents).

`@-`
:   Insert a discretionary hyphenation point. See [`@-` and `@hyphenation`: Hyphenation in Printed Output](#g_t_0040_002d-_0040hyphenation).

`@.`
:   Produce a period that ends a sentence (usually after an
    end-of-sentence capital letter). See [Ending a Sentence](#Ending-a-Sentence).

`@/`
:   Produces no output, but allows a line break. See [`@*` and `@/`: Generate and Allow Line Breaks](#Line-Breaks).

`@:`
:   Tell printed output processors to refrain from inserting extra
    whitespace after an immediately preceding period, question mark,
    exclamation mark, or colon. See [Not Ending a Sentence](#Not-Ending-a-Sentence).

`@=`
:   Generate a macron (bar) accent over the next character, as in ō.
    See [Inserting Accents](#Inserting-Accents).

`@?`
:   Produce a question mark that ends a sentence (usually after an
    end-of-sentence capital letter). See [Ending a Sentence](#Ending-a-Sentence).

`@@`

`@atchar{}`
:   Insert an at sign, ‘`@`’. See [Inserting ‘@’ with `@@` and `@atchar{}`](#Inserting-an-Atsign).

`@\`

`@backslashchar{}`
:   Insert a backslash, ‘`\`’; `@backslashchar{}` works
    anywhere, while `@\` works only inside `@math`.
    See [Inserting ‘\’ with `@backslashchar{}`](#Inserting-a-Backslash), and [`@math` and `@displaymath`: Formatting Mathematics](#Inserting-Math).

`@^`

`` @` ``
:   Generate a circumflex (hat) or grave accent, respectively, over the next
    character, as in ô and è.
    See [Inserting Accents](#Inserting-Accents).

`@{`

`@lbracechar{}`
:   Insert a left brace, ‘`{`’. See [Inserting ‘{ ‘}’ with `@{ @}` and `@l rbracechar{}`](#Inserting-Braces).

`@}`

`@rbracechar{}`
:   Insert a right brace, ‘`}`’. See [Inserting ‘{ ‘}’ with `@{ @}` and `@l rbracechar{}`](#Inserting-Braces).

`@~`
:   Generate a tilde accent over the next character, as in Ñ.
    See [Inserting Accents](#Inserting-Accents).

`@AA{}`

`@aa{}`
:   Generate the uppercase and lowercase Scandinavian A-ring letters,
    respectively: Å, å. See [Inserting Accents](#Inserting-Accents).

`@abbr{abbreviation}`
:   Indicate a general abbreviation, such as ‘Comput.’.
    See [`@abbr`{abbreviation[, meaning]}](#g_t_0040abbr).

`@acronym{acronym}`
:   Indicate an acronym in all capital letters, such as ‘NASA’.
    See [`@acronym`{acronym[, meaning]}](#g_t_0040acronym).

`@AE{}`

`@ae{}`
:   Generate the uppercase and lowercase AE ligatures, respectively:
    Æ, æ. See [Inserting Accents](#Inserting-Accents).

`@afivepaper`
:   Change page dimensions for the A5 paper size. See [Printing on A4 Paper](#A4-Paper).

`@afourlatex`

`@afourpaper`

`@afourwide`
:   Change page dimensions for the A4 paper size. See [Printing on A4 Paper](#A4-Paper).

`@alias new=existing`
:   Make the command ‘`@new`’ a synonym for the existing command
    ‘`@existing`’. See [‘`@alias new=existing`’](#g_t_0040alias).

`@allowcodebreaks true-false`
:   Control breaking at ‘`-`’ and ‘`_`’ in printed output.
    See [`@allowcodebreaks`: Control Line Breaks in `@code`](#g_t_0040allowcodebreaks).

`@anchor{name}`
:   Define name as the current location for use as a cross-reference
    target. See [`@anchor`: Defining Arbitrary Cross-reference Targets](#g_t_0040anchor).

`@appendix title`
:   Begin an appendix. The title appears in the table of contents.
    See [`@unnumbered`, `@appendix`: Chapters with Other Labeling](#g_t_0040unnumbered-_0040appendix).

`@appendixsec title`

`@appendixsection title`
:   Begin an appendix section within an appendix. The section title
    appears in the table of contents. `@appendixsection` is
    a longer spelling of the `@appendixsec` command.
    See [`@unnumberedsec`, `@appendixsec`, `@heading`](#g_t_0040unnumberedsec-_0040appendixsec-_0040heading).

`@appendixsubsec title`
:   Begin an appendix subsection. The title appears in the table of
    contents. See [The `@subsection`-like Commands](#g_t_0040unnumberedsubsec-_0040appendixsubsec-_0040subheading).

`@appendixsubsubsec title`
:   Begin an appendix subsubsection. The title appears in the table of
    contents. See [`@subsubsection` and Other Subsub Commands](#g_t_0040subsubsection).

`@arrow{}`
:   Generate a right arrow glyph: ‘`→`’. Used by default
    for `@click`. See [Click Sequences](#Click-Sequences).

`@asis`
:   Keep the argument as is.
    Used following `@table`, `@ftable`, and `@vtable` to
    print the table’s first column without highlighting (“as is”).
    See [`@asis`](#g_t_0040asis).

`@author author`
:   Set a manual author in the title page. See [`@title`, `@subtitle`, and `@author`](#g_t_0040title-_0040subtitle-_0040author).
    Set a quotation author in `@quotation`. See [`@quotation`: Block Quotations](#g_t_0040quotation).

`@b{text}`
:   Set text in a **bold** font, if possible. See [Fonts for Printing](#Fonts).

`@bullet{}`
:   Generate a large round dot, •, or the closest possible thing to one.
    Often used with `@table`. See [`@bullet` (•)](#g_t_0040bullet).

`@bsixpaper`
:   Change page dimensions for the B6 paper size. See [Printing on A4 Paper](#A4-Paper).

`@bye`
:   Stop processing a file. The processors do not see anything in the
    input file following `@bye`. See [Ending a Texinfo File](#Ending-a-File).

`@c comment`
:   Begin a comment in Texinfo. The rest of the line does not appear in
    any output. A synonym for `@comment`. `DEL` also
    starts a comment. See [Comments](#Comments).

`@caption`
:   Define the full caption for a `@float`. See [`@caption` & `@shortcaption`](#g_t_0040caption-_0040shortcaption).

`@cartouche`
:   Highlight an example or quotation by drawing a box with rounded
    corners around it, if possible. Pair with `@end cartouche`.
    See [`@cartouche`: Rounded Rectangles](#g_t_0040cartouche).

`@center line-of-text`
:   Center the line of text following the command.
    See [`@titlefont`, `@center`, and `@sp`](#g_t_0040titlefont-_0040center-_0040sp).

`@centerchap line-of-text`
:   Like `@chapter`, but centers the chapter title. See [`@chapter`: Chapter Structuring](#g_t_0040chapter).

`@chapheading title`
:   Print an unnumbered chapter-like heading, but omit from the table of
    contents. See [`@majorheading`, `@chapheading`: Chapter-level Headings](#g_t_0040majorheading-_0040chapheading).

`@chapter title`
:   Begin a numbered chapter. The chapter title appears in the table of
    contents. See [`@chapter`: Chapter Structuring](#g_t_0040chapter).

`@cindex entry`
:   Add entry to the index of concepts. See [Defining the Entries of an Index](#Index-Entries).

`@cite{reference}`
:   Highlight the name of a book or other reference that has no companion
    Info file. See [`@cite`{reference}](#g_t_0040cite).

`@clear flag`
:   Unset flag, preventing the Texinfo formatting commands from
    formatting text between subsequent pairs of `@ifset flag`
    and `@end ifset` commands, and preventing
    `@value{flag}` from expanding to the value to which
    flag is set. See [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value).

`@click{}`
:   Represent a single “click” in a GUI. Used within
    `@clicksequence`. See [Click Sequences](#Click-Sequences).

`@clicksequence{action @click{} action}`
:   Represent a sequence of clicks in a GUI. See [Click Sequences](#Click-Sequences).

`@clickstyle @cmd`
:   Execute @cmd for each `@click`; the default is
    `@arrow`. The usual following empty braces on @cmd are
    omitted. See [Click Sequences](#Click-Sequences).

`@code{sample-code}`
:   Indicate an expression, a syntactically complete token of a program,
    or a program name. See [`@code`{sample-code}](#g_t_0040code).

`@codequotebacktick on-off`

`@codequoteundirected on-off`
:   Control output of `` ` `` and `'` in code examples.
    See [Inserting Quote Characters](#Inserting-Quote-Characters).

`@comma{}`
:   Insert a comma ‘,’ character; only needed when a literal comma would
    be taken as an argument separator. See [Inserting ‘,’ with `@comma{}`](#Inserting-a-Comma).

`@command{command-name}`
:   Indicate a command name, such as `ls`. See [`@command`{command-name}](#g_t_0040command).

`@comment comment`
:   Begin a comment in Texinfo. The rest of the line does not appear in
    any output. A synonym for `@c`.
    See [Comments](#Comments).

`@contents`
:   Print a complete table of contents or specify that a table of content
    should be output, for formats that may output a table of contents.
    See [Generating a Table of Contents](#Contents).

`@copying`
:   Specify copyright holders and copying conditions for the document. Pair
    with `@end copying`. See [`@copying`: Declare Copying Permissions](#g_t_0040copying).

`@copyright{}`
:   Generate the copyright symbol ©.
    See [`@copyright{}` (©)](#g_t_0040copyright).

`@defblock`
:   Start a block containing definitions. Pair with ‘`@end defblock`’.
    See [Generic Definition Commands](#Generic-Definition-Commands).

`@defcodeindex index-name`
:   Define a new index and its indexing command. Print entries in an
    `@code` font. See [Defining New Indices](#New-Indices).

`@defcv category class name`

`@defcvx category class name`
:   Format a description for a variable associated with a class in
    object-oriented programming. Takes three arguments: the category of
    thing being defined, the class to which it belongs, and its name.
    See [Definition Commands](#Definition-Commands).

`@deffn category name arguments…`

`@deffnx category name arguments…`
:   Format a description for a function, interactive command, or similar
    entity that may take arguments. `@deffn` takes as arguments the
    category of entity being described, the name of this particular
    entity, and its arguments, if any. See [Definition Commands](#Definition-Commands).

`@defindex index-name`
:   Define a new index and its indexing command. Print entries in a roman
    font. See [Defining New Indices](#New-Indices).

`@definfoenclose newcmd, before, after`
:   Create a new command `@newcmd` for online formats
    that marks text by enclosing it in strings that precede and
    follow the text.
    See [`@definfoenclose`: Customized Highlighting](#g_t_0040definfoenclose).

`@defivar class instance-variable-name`

`@defivarx class instance-variable-name`
:   Format a description for an instance variable in object-oriented
    programming. The command is equivalent to ‘`@defcv {Instance
    Variable} …`’. See [Definition Commands](#Definition-Commands).

`@defline category name arguments…`
:   Use within a `@defblock` environment to give the heading prototype
    line for a symbol being defined. This command does not
    create any index entries. See [Generic Definition Commands](#Generic-Definition-Commands).

`@defmac macroname arguments…`

`@defmacx macroname arguments…`
:   Format a description for a macro; equivalent to ‘`@deffn Macro
    …`’. See [Definition Commands](#Definition-Commands).

`@defmethod class method-name arguments…`

`@defmethodx class method-name arguments…`
:   Format a description for a method in object-oriented programming;
    equivalent to ‘`@defop Method …`’. See [Definition Commands](#Definition-Commands).

`@defop category class name arguments…`

`@defopx category class name arguments…`
:   Format a description for an operation in object-oriented programming.
    `@defop` takes as arguments the name of the category of
    operation, the name of the operation’s class, the name of the
    operation, and its arguments, if any. See [Definition Commands](#Definition-Commands), and
    [Object-Oriented Programming](#Abstract-Objects).

`@defopt option-name`

`@defoptx option-name`
:   Format a description for a user option; equivalent to ‘`@defvr
    {User Option} …`’. See [Definition Commands](#Definition-Commands).

`@defspec special-form-name arguments…`

`@defspecx special-form-name arguments…`
:   Format a description for a special form; equivalent to ‘`@deffn
    {Special Form} …`’. See [Definition Commands](#Definition-Commands).

`@deftp category name-of-type attributes…`

`@deftpx category name-of-type attributes…`
:   Format a description for a data type; its arguments are the category,
    the name of the type (e.g., ‘`int`’) , and then the names of
    attributes of objects of that type. See [Definition Commands](#Definition-Commands), and
    [Data Types](#Data-Types).

`@deftypecv category class data-type name`

`@deftypecvx category class data-type name`
:   Format a description for a typed class variable in object-oriented programming.
    See [Definition Commands](#Definition-Commands), and [Object-Oriented Programming](#Abstract-Objects).

`@deftypefn category data-type name arguments…`

`@deftypefnx category data-type name arguments…`
:   Format a description for a function or similar entity that may take
    arguments and that is typed. `@deftypefn` takes as arguments the
    category of entity being described, the type, the name of the
    entity, and its arguments, if any. See [Definition Commands](#Definition-Commands).

`@deftypefnnewline on-off`
:   Specifies whether return types for `@deftypefn` and similar are
    printed on lines by themselves; default is off. See [Functions in Typed Languages](#Typed-Functions).

`@deftypefun data-type function-name arguments…`

`@deftypefunx data-type function-name arguments…`
:   Format a description for a function in a typed language.
    The command is equivalent to ‘`@deftypefn Function …`’.
    See [Definition Commands](#Definition-Commands).

`@deftypeivar class data-type variable-name`

`@deftypeivarx class data-type variable-name`
:   Format a description for a typed instance variable in object-oriented
    programming. See [Definition Commands](#Definition-Commands), and [Object-Oriented Programming](#Abstract-Objects).

`@deftypeline category data-type name arguments…`
:   Use within a `@defblock` environment to give the heading prototype
    line for a symbol being defined, with data types. This command does not
    create any index entries. See [Generic Definition Commands](#Generic-Definition-Commands).

`@deftypemethod class data-type method-name arguments…`

`@deftypemethodx class data-type method-name arguments…`
:   Format a description for a typed method in object-oriented programming.
    See [Definition Commands](#Definition-Commands).

`@deftypeop category class data-type name arguments…`

`@deftypeopx category class data-type name arguments…`
:   Format a description for a typed operation in object-oriented programming.
    See [Definition Commands](#Definition-Commands), and [Object-Oriented Programming](#Abstract-Objects).

`@deftypevar data-type variable-name`

`@deftypevarx data-type variable-name`
:   Format a description for a variable in a typed language. The command is
    equivalent to ‘`@deftypevr Variable …`’. See [Definition Commands](#Definition-Commands).

`@deftypevr category data-type name`

`@deftypevrx category data-type name`
:   Format a description for something like a variable in a typed
    language—an entity that records a value. Takes as arguments the
    category of entity being described, the type, and the name of the
    entity. See [Definition Commands](#Definition-Commands).

`@defun function-name arguments…`

`@defunx function-name arguments…`
:   Format a description for a function; equivalent to
    ‘`@deffn Function …`’. See [Definition Commands](#Definition-Commands).

`@defvar variable-name`

`@defvarx variable-name`
:   Format a description for a variable; equivalent to ‘`@defvr
    Variable …`’. See [Definition Commands](#Definition-Commands).

`@defvr category name`

`@defvrx category name`
:   Format a description for any kind of variable. `@defvr` takes
    as arguments the category of the entity and the name of the entity.
    See [Definition Commands](#Definition-Commands).

`@detailmenu`
:   Mark the (optional) detailed node listing in a master menu.
    See [Parts of a Master Menu](#Master-Menu-Parts).

`@dfn{term}`
:   Indicate the introductory or defining use of a term. See [`@dfn`{term}](#g_t_0040dfn).

`@DH{}`

`@dh{}`
:   Generate the uppercase and lowercase Icelandic letter eth, respectively:
    Ð, ð. See [Inserting Accents](#Inserting-Accents).

`@dircategory dirpart`
:   Specify a category for the manual. See [Directory Category](#Directory-Category).

`@direntry`
:   Begin the Info directory menu entry for this file. Pair with
    `@end direntry`. See [Installing Info Directory Files](#Installing-Dir-Entries).

`@display`
:   Begin a kind of example. Like `@example` (indent text, do not
    fill), but do not select a new font. Pair with `@end display`.
    See [`@display`: Examples Using the Text Font](#g_t_0040display).

`@displaymath`
:   Format a block of math in “display” format. See [`@math` and `@displaymath`: Formatting Mathematics](#Inserting-Math).

`@dmn{dimension}`
:   Format a unit of measure, as in 12pt. See [`@dmn`{dimension}: Format a Dimension](#g_t_0040dmn).

`@docbook`
:   Enter DocBook completely. Pair with `@end docbook`. See [Raw Formatter Commands](#Raw-Formatter-Commands).

`@documentdescription`
:   Set the document description text, included in the HTML output. Pair
    with `@end documentdescription`. See [`@documentdescription`: Summary Text](#g_t_0040documentdescription).

`@documentencoding enc`
:   Declare the input encoding to be enc.
    See [`@documentencoding enc`: Set Input Encoding](#g_t_0040documentencoding).

`@documentlanguage CC`
:   Declare the document language as the two-character ISO-639 abbreviation
    CC. See [`@documentlanguage ll[_cc]`: Set the Document Language](#g_t_0040documentlanguage).

`@dotaccent{c}`
:   Generate a dot accent over the character c, as in ȯ.
    See [Inserting Accents](#Inserting-Accents).

`@dotless{i-or-j}`
:   Generate dotless i (‘ı’) and dotless j (‘ȷ’).
    See [Inserting Accents](#Inserting-Accents).

`@dots{}`
:   Generate an ellipsis, ‘`…`’.
    See [`@dots` (…) and `@enddots` (...)](#g_t_0040dots).

`@email{address[, displayed-text]}`
:   Indicate an electronic mail address. See [`@email`{email-address[, displayed-text]}](#g_t_0040email).

`@emph{text}`
:   Emphasize text. See [Emphasizing Text](#Emphasis).

`@end environment`
:   Ends environment, as in ‘`@end example`’. See [@-commands](#Formatting-Commands).

`@enddots{}`
:   Generate an end-of-sentence ellipsis, like this: ...
    See [`@dots` (…) and `@enddots` (...)](#g_t_0040dots).

`@enumerate [number-or-letter]`
:   Begin a numbered list, using `@item` for each entry.
    Optionally, start list with number-or-letter. Pair with
    `@end enumerate`. See [`@enumerate`: Making a Numbered or Lettered List](#g_t_0040enumerate).

`@env{environment-variable}`
:   Indicate an environment variable name, such as `PATH`.
    See [`@env`{environment-variable}](#g_t_0040env).

`@equiv{}`
:   Indicate to the reader the exact equivalence of two forms with a
    glyph: ‘`≡`’. See [`@equiv{}` (≡): Indicating Equivalence](#g_t_0040equiv).

`@error{}`
:   Indicate to the reader with a glyph that the following text is
    an error message: ‘`error→`’. See [`@error{}` (error→): Indicating an Error Message](#g_t_0040error).

`@errormsg{msg}`
:   Report msg as an error to standard error, and exit unsuccessfully.
    Texinfo commands within msg are expanded to plain text.
    See [Conditionally Visible Text](#Conditionals), and [External Macro Processors: Line Directives](#External-Macro-Processors).

`@euro{}`
:   Generate the Euro currency sign. See [`@euro` (€): Euro Currency Symbol](#g_t_0040euro).

`@evenfooting [left] @| [center] @| [right]`

`@evenheading [left] @| [center] @| [right]`
:   Specify page footings resp. headings for even-numbered (left-hand)
    pages. See [How to Make Your Own Headings](#Custom-Headings).

`@everyfooting [left] @| [center] @| [right]`

`@everyheading [left] @| [center] @| [right]`
:   Specify page footings resp. headings for every page. Not relevant to
    Info. See [How to Make Your Own Headings](#Custom-Headings).

`@example`
:   Begin an example. Indent text, do not fill, and select fixed-width
    font. Pair with `@end example`. `@example` accepts optional
    arguments, separated by commas. It is recommended to set the first argument to
    the language of the example code. See [`@example`: Example Text](#g_t_0040example).

`@exampleindent indent`
:   Indent example-like environments by indent number of spaces
    (perhaps 0). See [`@exampleindent`: Environment Indenting](#g_t_0040exampleindent).

`@exclamdown{}`
:   Generate an upside-down exclamation point. See [Inserting Accents](#Inserting-Accents).

`@exdent line-of-text`
:   Remove any indentation a line might have. See [`@exdent`: Undoing a Line’s Indentation](#g_t_0040exdent).

`@expansion{}`
:   Indicate the result of a macro expansion to the reader with a special
    glyph: ‘`→`’. See [`@expansion{}` (→): Indicating an Expansion](#g_t_0040expansion).

`@file{filename}`
:   Highlight the name of a file, buffer, node, directory, etc.
    See [`@file`{file-name}](#g_t_0040file).

`@finalout`
:   Prevent TeX from printing large black warning rectangles beside
    over-wide lines. See [Overfull “hboxes”](#Overfull-hboxes).

`@findex entry`
:   Add entry to the index of functions. See [Defining the Entries of an Index](#Index-Entries).

`@firstparagraphindent word`
:   Control indentation of the first paragraph after section headers
    according to word, one of ‘none’ or ‘insert’.
    See [`@firstparagraphindent`: Indenting After Headings](#g_t_0040firstparagraphindent).

`@float`
:   Environment to define floating material. Pair with `@end float`.
    See [Floats](#Floats).

`@flushleft`

`@flushright`
:   Do not fill text; left (right) justify every line while leaving the
    right (left) end ragged. Leave font as is. Pair with `@end
    flushleft` (`@end flushright`). See [`@flushleft` and `@flushright`](#g_t_0040flushleft-_0040flushright).

`@fonttextsize 10-11`
:   Change the size of the main body font in the printed output.
    See [Fonts for Printing](#Fonts).

`@footnote{text-of-footnote}`
:   Enter a footnote, for a reference that documents or elucidates the
    primary text. Footnote text is printed at the bottom of the page
    in printed output. In other formats, footnote text can be output
    in the same node, in a separate node, or simply be marked as
    being footnote text. See [Footnotes](#Footnotes).

`@footnotestyle style`
:   Specify a footnote style, either ‘`end`’ for the end
    node style or ‘`separate`’ for the separate style. In the
    separate style, footnotes are put in a separate node or file.
    See [Footnote Styles](#Footnote-Styles).

`@format`
:   Begin a kind of example. Like `@display`, but do not indent.
    Pair with `@end format`. See [`@example`: Example Text](#g_t_0040example).

`@frenchspacing on-off`
:   Control spacing after punctuation. See [`@frenchspacing` val: Control Sentence Spacing](#g_t_0040frenchspacing).

`@ftable formatting-command`
:   Begin a two-column table, using `@item` for each entry.
    Automatically enter each of the items in the first column into the
    index of functions. Pair with `@end ftable`. The same as
    `@table`, except for indexing. See [`@ftable` and `@vtable`](#g_t_0040ftable-_0040vtable).

`@geq{}`
:   Generate a greater-than-or-equal sign, ‘≥’. See [`@geq` (≥) and `@leq` (≤): Inserting Relations](#g_t_0040geq-_0040leq).

`@group`
:   Disallow page breaks within following text. Pair with `@end
    group`. Ignored in Info. See [`@group`: Prevent Page Breaks](#g_t_0040group).

`@guillemetleft{}`

`@guillemetright{}`

`@guillemotleft{}`

`@guillemotright{}`

`@guilsinglleft{}`

`@guilsinglright{}`
:   Double and single angle quotation marks: «
    » ‹ ›.
    `@guillemotleft` and `@guillemotright` are synonyms for
    `@guillemetleft` and `@guillemetright`. See [Inserting Quotation Marks](#Inserting-Quotation-Marks).

`@H{c}`
:   Generate the long Hungarian umlaut accent over c, as in ő.

`@hashchar{}`
:   Insert a hash ‘#’ character; only needed when a literal hash would
    introduce `#line` directive. See [Inserting ‘#’ with `@hashchar{}`](#Inserting-a-Hashsign), and
    [External Macro Processors: Line Directives](#External-Macro-Processors).

`@heading title`
:   Print an unnumbered section-like heading, but omit from the table of
    contents. See [`@unnumberedsec`, `@appendixsec`, `@heading`](#g_t_0040unnumberedsec-_0040appendixsec-_0040heading).

`@headings on-off-single-double`
:   Turn page headings on or off, and/or specify single-sided or double-sided
    page headings for printing. See [The `@headings` Command](#g_t_0040headings).

`@headitem`
:   Begin a heading row in a multitable. See [Multitable Rows](#Multitable-Rows).

`@headitemfont{text}`
:   Set text in the font used for multitable heading rows; mostly
    useful in multitable templates. See [Multitable Rows](#Multitable-Rows).

`@html`
:   Enter HTML completely. Pair with `@end html`. See [Raw Formatter Commands](#Raw-Formatter-Commands).

`@hyphenation{hy-phen-a-ted words}`
:   Explicitly define hyphenation points. See [`@-` and `@hyphenation`: Hyphenation in Printed Output](#g_t_0040_002d-_0040hyphenation).

`@i{text}`
:   Set text in an *italic* font, when possible. See [Fonts for Printing](#Fonts).

`@ifclear txivar`
:   If the Texinfo variable txivar is not set, format the following
    text. Pair with `@end ifclear`. See [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value).

`@ifcommanddefined txicmd`

`@ifcommandnotdefined txicmd`
:   If the Texinfo code ‘`@txicmd`’ is (not) defined, format the
    follow text. Pair with the corresponding `@end ifcommand...`.
    See [Testing for Texinfo Commands: `@ifcommanddefined`, `@ifcommandnotdefined`](#Testing-for-Texinfo-Commands).

`@ifdocbook`

`@ifhtml`

`@ifinfo`

`@iflatex`

`@ifplaintext`

`@ifxml`
:   Begin text that will appear only in the given output format.
    `@ifinfo` output appears in both Info and (for historical
    compatibility) plain text output. Pair with `@end ifdocbook`
    resp. `@end ifhtml`...
    See [Conditionally Visible Text](#Conditionals).

`@ifnotdocbook`

`@ifnothtml`

`@ifnotlatex`

`@ifnotplaintext`

`@ifnottex`

`@ifnotxml`
:   Begin text to be ignored in one output format but not the others.
    `@ifnothtml` text is omitted from HTML output, etc. Pair with
    the corresponding `@end ifnotformat`.
    See [Conditionally Visible Text](#Conditionals).

`@ifnotinfo`
:   Begin text to appear in output other than Info and (for historical
    compatibility) plain text. Pair with `@end ifnotinfo`.
    See [Conditionally Visible Text](#Conditionals).

`@ifset txivar`
:   If the Texinfo variable txivar is set, format the following
    text. Pair with `@end ifset`. See [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value).

`@iftex`
:   Begin text to appear only in the TeX output. Pair with `@end
    iftex`. See [Conditionally Visible Text](#Conditionals).

`@ignore`
:   Begin text that will not appear in any output. Pair with `@end
    ignore`. See [Comments and Ignored Text](#Comments).

`@image{filename, [width], [height], [alt], [ext]}`
:   Include graphics image in external filename scaled to the given
    width and/or height, using alt text and looking for
    ‘`filename.ext`’ in HTML. See [Inserting Images](#Images).

`@include filename`
:   Read the contents of Texinfo source file filename. See [Include Files](#Include-Files).

`@indent`
:   Insert paragraph indentation. See [`@indent`: Forcing Indentation](#g_t_0040indent).

`@indentedblock`
:   Indent a block of arbitrary text on the left. Pair with `@end
    indentedblock`. See [`@indentedblock`: Indented text blocks](#g_t_0040indentedblock).

`@indicateurl{indicateurl}`
:   Indicate text that is a uniform resource locator for the World Wide
    Web. See [`@indicateurl`{uniform-resource-locator}](#g_t_0040indicateurl).

`@inforef{node-name, [entry-name], info-file-name}`
:   Make a cross-reference to an Info file for which there is no printed
    manual. See [`@inforef`: Cross-references to Info-only Material](#g_t_0040inforef).

`@inlinefmt{fmt, text}`
:   Insert text only if the output format is fmt.
    See [Inline Conditionals: `@inline`, `@inlineifelse`, `@inlineraw`](#Inline-Conditionals).

`@inlinefmtifelse{fmt, text, else-text}`
:   Insert text if the output format is fmt, else else-text.

`@inlineifclear{var, text}`

`@inlineifset{var, text}`
:   Insert text only if the Texinfo variable var is (not) set.

`@inlineraw{fmt, raw-text}`
:   Insert text as in a raw conditional, only if the output format
    is fmt.

`\input macro-definitions-file`
:   Use the specified macro definitions file. This command is used only
    in the first line of a Texinfo file to cause TeX to make use of the
    `texinfo` macro definitions file. The `\` in `\input`
    is used instead of an `@` because TeX does not recognize
    `@` until after it has read the definitions file. See [Texinfo File Header](#Texinfo-File-Header).

`@insertcopying`
:   Insert the text previously defined with the `@copying`
    environment. See [`@insertcopying`: Include Permissions Text](#g_t_0040insertcopying).

`@item`
:   Indicate the beginning of a marked paragraph for `@itemize` and
    `@enumerate`; indicate the beginning of the text of a first column
    entry for `@table`, `@ftable`, and `@vtable`.
    See [Lists and Tables](#Lists-and-Tables).

`@itemize mark-generating-character-or-command`
:   Begin an unordered list: indented paragraphs with a mark, such as
    `@bullet`, inside the left margin at the beginning of each item.
    Pair with `@end itemize`. See [`@itemize`: Making an Itemized List](#g_t_0040itemize).

`@itemx`
:   Like `@item` in `@table`, `@ftable`, and `@vtable`,
    but do not generate extra vertical space above the
    item text. Thus, when several items have the same description, use
    `@item` for the first and `@itemx` for the others.
    See [`@itemx`: Second and Subsequent Items](#g_t_0040itemx).

`@kbd{keyboard-characters}`
:   Indicate characters of input to be typed by users. See [`@kbd`{keyboard-characters}](#g_t_0040kbd).

`@kbdinputstyle style`
:   Specify when `@kbd` should use a font distinct from
    `@code` according to style: `code`, `distinct`,
    `example`. See [`@kbd`{keyboard-characters}](#g_t_0040kbd).

`@key{key-name}`
:   Indicate the name of a key on a keyboard. See [`@key`{key-name}](#g_t_0040key).

`@kindex entry`
:   Add entry to the index of keys.
    See [Defining the Entries of an Index](#Index-Entries).

`@L{}`

`@l{}`
:   Generate the uppercase and lowercase Polish suppressed-L letters,
    respectively: Ł, ł.

`@LaTeX{}`
:   Generate the LaTeX logo. See [`@TeX`{} (TeX) and `@LaTeX`{} (LaTeX)](#g_t_0040TeX-_0040LaTeX).

`@latex`
:   Enter LaTeX completely. Pair with `@end latex`. See [Raw Formatter Commands](#Raw-Formatter-Commands).

`@leq{}`
:   Generate a less-than-or-equal sign, ‘≤’. See [`@geq` (≥) and `@leq` (≤): Inserting Relations](#g_t_0040geq-_0040leq).

`@linemacro macroname {params}`
:   Define a new macro which takes rest of the line as an argument, and
    expands to a whole number of complete lines. See [Line Macros](#Line-Macros).

`@link{nodename, label, manual-name}`
:   Create a plain link with no visible markup or page reference.
    See [`@link`: Plain, unadorned hyperlink](#g_t_0040link).

`@lisp`
:   Begin an example of Lisp code. Indent text, do not fill, and select
    fixed-width font. Pair with `@end lisp`. See [`@lisp`: Marking a Lisp Example](#g_t_0040lisp).

`@listoffloats`
:   Produce a table-of-contents-like listing of `@float`s.
    See [`@listoffloats`: Tables of Contents for Floats](#g_t_0040listoffloats).

`@lowersections`
:   Change subsequent chapters to sections, sections to subsections, and so
    on. See [`@raisesections` and
    `@lowersections`](#Raise_002flower-sections).

`@macro macroname {params}`
:   Define a new Texinfo command `@macroname{params}`.
    Pair with `@end macro`. See [Defining Macros](#Defining-Macros).

`@majorheading title`
:   Print an unnumbered chapter-like heading, but omit from the table of
    contents. This generates more vertical whitespace before the heading
    than the `@chapheading` command. See [`@majorheading`, `@chapheading`: Chapter-level Headings](#g_t_0040majorheading-_0040chapheading).

`@math{mathematical-expression}`
:   Format a mathematical expression. See [`@math` and `@displaymath`: Formatting Mathematics](#Inserting-Math).

`@menu`
:   Mark the beginning of a menu of nodes. No effect in a printed manual.
    Pair with `@end menu`. See [Menus](#Menus).

`@microtype on-off`
:   Turn microtype on or off. See [Microtypography](#Microtypography).

`@minus{}`
:   Generate a minus sign, ‘−’. See [`@minus` (−): Inserting a Minus Sign](#g_t_0040minus).

`@multitable column-width-spec`
:   Begin a multi-column table. Begin each row with `@item` or
    `@headitem`, and separate columns with `@tab`. Pair with
    `@end multitable`. See [Multitable Column Widths](#Multitable-Column-Widths).

`@need n`
:   Start a new page in a printed manual if fewer than n mils
    (thousandths of an inch) remain on the current page.
    See [`@need mils`: Prevent Page Breaks](#g_t_0040need).

`@node name, [next], [previous], [up]`
:   Begin a new node. Only the first argument is mandatory.
    See [Writing a `@node` Line](#Writing-a-Node).

`@nodedescription node-description`
:   Provide a short elaboration of the purpose of a node.
    See [Node Descriptions](#Node-Descriptions).

`@nodedescriptionblock`
:   Used to start a longer node description. Pair with
    ‘`@end nodedescriptionblock`’. See [Node Descriptions](#Node-Descriptions).

`@noindent`
:   Prevent text from being indented as if it were a new paragraph.
    See [`@noindent`: Omitting Indentation](#g_t_0040noindent).

`@novalidate`
:   Suppress validation of node references and omit creation of auxiliary
    files with TeX. Use before any sectioning or cross-reference
    commands. See [Pointer Validation](#Pointer-Validation).

`@O{}`

`@o{}`
:   Generate the uppercase and lowercase O-with-slash letters, respectively:
    Ø, ø.

`@oddfooting [left] @| [center] @| [right]`

`@oddheading [left] @| [center] @| [right]`
:   Specify page footings resp. headings for odd-numbered (right-hand)
    pages. See [How to Make Your Own Headings](#Custom-Headings).

`@OE{}`

`@oe{}`
:   Generate the uppercase and lowercase OE ligatures, respectively:
    Œ, œ. See [Inserting Accents](#Inserting-Accents).

`@ogonek{c}`
:   Generate an ogonek diacritic under the next character, as in
    ą. See [Inserting Accents](#Inserting-Accents).

`@option{option-name}`
:   Indicate a command-line option, such as `-l` or
    `--help`. See [`@option`{option-name}](#g_t_0040option).

`@ordf{}`

`@ordm{}`
:   Generate the feminine and masculine Spanish ordinals, respectively:
    ª, º. See [Inserting Accents](#Inserting-Accents).

`@page`
:   Start a new page in a printed manual. See [`@page`: Start a New Page](#g_t_0040page).

`@pagesizes [width][, height]`
:   Change page dimensions. See [pagesizes](#pagesizes).

`@paragraphindent indent`
:   Indent paragraphs by indent number of spaces (perhaps 0); preserve
    source file indentation if indent is `asis`.
    See [`@paragraphindent`: Controlling Paragraph Indentation](#g_t_0040paragraphindent).

`@part title`
:   Begin a group of chapters or appendixes; included in the tables of
    contents.
    See [`@part`: Groups of Chapters](#g_t_0040part).

`@pindex entry`
:   Add entry to the index of programs. See [Defining
    the Entries of an Index](#Index-Entries).

`@point{}`
:   Indicate the position of point in a buffer to the reader with a glyph:
    ‘`∗`’. See [`@point{}` (∗): Indicating Point in a Buffer](#g_t_0040point).

`@pounds{}`
:   Generate the pounds sterling currency sign, ‘`£`’.
    See [`@pounds` (£): Pounds Sterling](#g_t_0040pounds).

`@print{}`
:   Indicate printed output to the reader with a glyph: ‘`-|`’.
    See [`@print{}` (-|): Indicating Generated Output](#g_t_0040print).

`@printindex index-name`
:   Generate the index for index-name. See [Printing Indices and Menus](#Printing-Indices-_0026-Menus).

`@pxref{node, [entry], [node-title], [info-file], [printed-manual]}`
:   Make a reference to be used within parentheses. Starts with a lowercase
    ‘see’ in a printed manual. The first argument is mandatory, except for
    references to whole manuals. To refer to another manual as a whole,
    the printed-manual and/or the info-file are the only
    required arguments. See [`@pxref`](#g_t_0040pxref).

`@questiondown{}`
:   Generate an upside-down question mark. See [Inserting Accents](#Inserting-Accents).

`@quotation`
:   Narrow the margins to indicate text that is quoted from another work.
    Takes optional argument specifying prefix text. Pair with `@end
    quotation`. See [`@quotation`: Block Quotations](#g_t_0040quotation).

`@quotedblleft{}`

`@quotedblright{}`

`@quoteleft{}`

`@quoteright{}`

`@quotedblbase{}`

`@quotesinglbase{}`
:   Produce various quotation marks: “ ”
    ‘ ’ „ ‚.
    See [Inserting Quotation Marks](#Inserting-Quotation-Marks).

`@r{text}`
:   Set text in the regular roman font, if possible.
    See [Fonts for Printing](#Fonts).

`@raggedright`
:   Fill text; left justify every line while leaving the right end ragged.
    Leave font as is. Pair with `@end raggedright`.
    See [`@raggedright`: Ragged Right Text](#g_t_0040raggedright).

`@raisesections`
:   Change subsequent sections to chapters, subsections to sections, and so
    on. See [Raise/lower Sections: `@raisesections` and `@lowersections`](#Raise_002flower-sections).

`@ref{node, [entry], [node-title], [info-file], [printed-manual]}`
:   Make a plain reference that does not start with any special text.
    Follow command with a punctuation mark. The first argument is mandatory, except for
    references to whole manuals. To refer to another manual as a whole,
    the printed-manual and/or the info-file are the only
    required arguments. See [`@ref`](#g_t_0040ref).

`@registeredsymbol{}`
:   Generate the legal symbol ®.
    See [`@registeredsymbol{}` (®)](#g_t_0040registeredsymbol).

`@result{}`
:   Indicate the result of an expression to the reader with a special
    glyph: ‘`⇒`’. See [`@result{}` (⇒): Result of an Expression](#g_t_0040result).

`@ringaccent{c}`
:   Generate a ring accent over the next character, as in o̊.
    See [Inserting Accents](#Inserting-Accents).

`@samp{text}`
:   Indicate a literal example of a sequence of characters, in general.
    See [`@samp`{text}](#g_t_0040samp).

`@sansserif{text}`
:   Set text in a sans serif font if possible.
    See [Fonts for Printing](#Fonts).

`@sc{text}`
:   Set text in a small caps font if possible, and uppercase
    in Info. See [`@sc`{text}: The Small Caps Font](#Smallcaps).

`@section title`
:   Begin a section within a chapter. The section title appears in the
    table of contents. Within `@chapter` and `@appendix`, the section
    title is numbered; within `@unnumbered`, the section is unnumbered.
    See [`@section`: Sections Below Chapters](#g_t_0040section).

`@seealso{index-text}`
:   Use in an index entry to refer the reader to another relevant index entry.
    See [Advanced Indexing Commands](#Advanced-Indexing).

`@seeentry{index-text}`
:   Use in an index entry to redirect the reader to another index entry.
    See [Advanced Indexing Commands](#Advanced-Indexing).

`@set txivar [string]`
:   Define the Texinfo variable txivar, optionally to the value
    string. See [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value).

`@setchapternewpage on-off-odd`
:   Specify whether chapters start on new pages, and if so, whether on
    odd-numbered (right-hand) new pages. See [`@setchapternewpage`: Blank Pages Before Chapters](#g_t_0040setchapternewpage).

`@setfilename info-file-name`
:   Provide a name to be used for the output files. This command is ignored
    for TeX formatting. See [`@setfilename`: Set the Output File Name](#g_t_0040setfilename).

`@settitle title`
:   Specify the title for page headers in a printed manual, and the
    default document title for HTML.
    See [`@settitle`: Set the Document Title](#g_t_0040settitle).

`@shortcaption`
:   Define the short caption for a `@float`. See [`@caption` & `@shortcaption`](#g_t_0040caption-_0040shortcaption).

`@shortcontents`
:   Print a short table of contents, with chapter-level entries only,
    or specify that a short table of contents should be output.
    For formats that may output a short table of contents.
    See [Generating a Table of Contents](#Contents).

`@shorttitlepage title`
:   Generate a minimal title page. See [`@titlepage`](#g_t_0040titlepage).

`@slanted{text}`
:   Set text in a *slanted* font if possible. See [Fonts for Printing](#Fonts).

`@smallbook`
:   In printed output, use a 7 by 9.25 inch format rather than the
    regular 8.5 by 11 inch format. See [`@smallbook`: Printing “Small” Books](#g_t_0040smallbook).

`@smalldisplay`
:   Begin a kind of example. Like `@display`, but use a smaller
    font size where possible. Pair with `@end smalldisplay`.
    See [`@small…` Block Commands](#small).

`@smallexample`
:   Begin an example. Like `@example`, but use a smaller font size
    where possible. Pair with `@end smallexample`.
    See [`@small…` Block Commands](#small).

`@smallformat`
:   Begin a kind of example. Like `@format`, but use a smaller font
    size where possible. Pair with `@end smallformat`.
    See [`@small…` Block Commands](#small).

`@smallindentedblock`
:   Like `@indentedblock`, but use a smaller font size where
    possible. Pair with `@end smallindentedblock`.
    See [`@small…` Block Commands](#small).

`@smalllisp`
:   Begin an example of Lisp code. Same as `@smallexample`. Pair
    with `@end smalllisp`. See [`@small…` Block Commands](#small).

`@smallquotation`
:   Like `@quotation`, but use a smaller font size where possible.
    Pair with `@end smallquotation`. See [`@small…` Block Commands](#small).

`@sortas {key}`
:   Used in the arguments to index commands to give a string by which the
    index entry should be sorted. See [Defining the Entries of an Index](#Indexing-Commands).

`@sp n`
:   Skip n blank lines. See [`@sp` n: Insert Blank Lines](#g_t_0040sp).

`@ss{}`
:   Generate the German sharp-S es-zet letter, ß. See [Inserting Accents](#Inserting-Accents).

`@strong {text}`
:   Emphasize text more strongly than `@emph`.
    See [Emphasizing Text](#emph-_0026-strong).

`@sub {text}`
:   Set text as a subscript. See [`@sub` and `@sup`: Inserting Subscripts and Superscripts](#Inserting-Subscripts-and-Superscripts).

`@subentry`
:   Use in an index entry to separate parts of a multi-level entry.
    See [Advanced Indexing Commands](#Advanced-Indexing).

`@subheading title`
:   Print an unnumbered subsection-like heading, but omit from the table
    of contents of a printed manual.
    See [The `@subsection`-like Commands](#g_t_0040unnumberedsubsec-_0040appendixsubsec-_0040subheading).

`@subsection title`
:   Begin a subsection within a section. The subsection title appears in
    the table of contents.
    Same context-dependent numbering as `@section`.
    See [`@subsection`: Subsections Below Sections](#g_t_0040subsection).

`@subsubheading title`
:   Print an unnumbered subsubsection-like heading, but omit from the
    table of contents of a printed manual. See [`@subsubsection` and Other Subsub Commands](#g_t_0040subsubsection).

`@subsubsection title`
:   Begin a subsubsection within a subsection. The subsubsection title
    appears in the table of contents. Same context-dependent numbering as
    `@section`. See [`@subsubsection` and Other Subsub Commands](#g_t_0040subsubsection).

`@subtitle title`
:   Set a subtitle for the title page. See [`@title`, `@subtitle`, and `@author`](#g_t_0040title-_0040subtitle-_0040author).

`@summarycontents`
:   Print or specify a short table of contents. Synonym for `@shortcontents`.
    See [Generating a Table of Contents](#Contents).

`@sup {text}`
:   Set text as a superscript. See [`@sub` and `@sup`: Inserting Subscripts and Superscripts](#Inserting-Subscripts-and-Superscripts).

`@syncodeindex from-index to-index`
:   Merge the index named in the first argument into the index named in
    the second argument, formatting the entries from the first index with
    `@code`. See [Combining Indices](#Combining-Indices).

`@synindex from-index to-index`
:   Merge the index named in the first argument into the index named in
    the second argument. Do not change the font of from-index
    entries. See [Combining Indices](#Combining-Indices).

`@t{text}`
:   Set text in a `fixed-width`, typewriter-like font, if possible.
    See [Fonts for Printing](#Fonts).

`@tab`
:   Separate columns in a row of a multitable. See [Multitable Rows](#Multitable-Rows).

`@table formatting-command`
:   Begin a two-column table (description list), using `@item` for
    each entry. Write each first column entry on the same line as
    `@item`. First column entries are printed in the font resulting
    from formatting-command. Pair with `@end table`.
    See [Making a Two-column Table](#Two_002dcolumn-Tables). Also see
    [`@ftable` and `@vtable`](#g_t_0040ftable-_0040vtable), and [`@itemx`: Second and Subsequent Items](#g_t_0040itemx).

`@TeX{}`
:   Generate the TeX logo. See [`@TeX`{} (TeX) and `@LaTeX`{} (LaTeX)](#g_t_0040TeX-_0040LaTeX).

`@tex`
:   Enter TeX completely. Pair with `@end tex`. See [Raw Formatter Commands](#Raw-Formatter-Commands).

`@textdegree{}`
:   Generate the degree symbol. See [`@textdegree` (°): Degrees Symbol](#g_t_0040textdegree).

`@thischapter`

`@thischaptername`

`@thischapternum`

`@thissection`

`@thissectionname`

`@thissectionnum`

`@thisfile`

`@thispage`

`@thistitle`
:   Only allowed in a heading or footing. Stands for, respectively, the
    number and name of the current chapter (in the format ‘Chapter 1:
    Title’), the current chapter name only, the current chapter number
    only, the number and name of the current section, the current section
    name only, the current section number only, the file name, the current
    page number, and the title of the document. See [How to Make Your Own Headings](#Custom-Headings).

`@TH{}`

`@th{}`
:   Generate the uppercase and lowercase Icelandic letter thorn, respectively:
    Þ, þ. See [Inserting Accents](#Inserting-Accents).

`@tie{}`
:   Generate a normal interword space at which a line break is not
    allowed. See [`@tie{}`: Inserting an Unbreakable Space](#g_t_0040tie).

`@tieaccent{cc}`
:   Generate a tie-after accent over the next two characters cc, as in
    ‘o͡o’. See [Inserting Accents](#Inserting-Accents).

`@tindex entry`
:   Add entry to the index of data types. See [Defining the Entries of an Index](#Index-Entries).

`@title title`
:   Set the title for the title page. See [`@title`, `@subtitle`, and `@author`](#g_t_0040title-_0040subtitle-_0040author).

`@titlefont{text}`
:   Print text in a larger than normal font, if possible.
    See [`@titlefont`, `@center`, and `@sp`](#g_t_0040titlefont-_0040center-_0040sp).

`@titlepage`
:   Begin the title page. Write the command on a line of its own, paired
    with `@end titlepage`. The title page is not output, in the
    default case, in online formats. See [`@titlepage`](#g_t_0040titlepage).

`@today{}`
:   Insert the current date, in ‘1 Jan 1900’ style. See [How to Make Your Own Headings](#Custom-Headings).

`@top title`
:   Mark the topmost `@node` in the file, which must be defined on
    the line immediately preceding the `@top` command. The title is
    formatted as a chapter-level heading. In TeX the `@top`
    command is merely a synonym for `@unnumbered`.

`@U{hex}`
:   Output a representation of Unicode character U+hex.
    See [Inserting Unicode: `@U`](#Inserting-Unicode).

`@u{c}`

`@ubaraccent{c}`

`@udotaccent{c}`
:   Generate a breve, underbar, or underdot accent, respectively, over or
    under the character c, as in ŏ, o̲,
    ọ. See [Inserting Accents](#Inserting-Accents).

`@unmacro macroname`
:   Undefine the macro `@macroname` if it has been defined.
    See [Defining Macros](#Defining-Macros).

`@unnumbered title`
:   Begin a chapter that appears without chapter numbers of any kind. The
    title appears in the table of contents. See [`@unnumbered`, `@appendix`: Chapters with Other Labeling](#g_t_0040unnumbered-_0040appendix).

`@unnumberedsec title`
:   Begin a section that appears without section numbers of any kind. The
    title appears in the table of contents. See [`@unnumberedsec`, `@appendixsec`, `@heading`](#g_t_0040unnumberedsec-_0040appendixsec-_0040heading).

`@unnumberedsubsec title`
:   Begin an unnumbered subsection. The title appears in the table of
    contents. See [The `@subsection`-like Commands](#g_t_0040unnumberedsubsec-_0040appendixsubsec-_0040subheading).

`@unnumberedsubsubsec title`
:   Begin an unnumbered subsubsection. The title appears in the table of
    contents. See [`@subsubsection` and Other Subsub Commands](#g_t_0040subsubsection).

`@uref{url[, displayed-text][, replacement}`

`@url{url[, displayed-text][, replacement}`
:   Define a cross-reference to an external uniform resource locator,
    e.g., for the World Wide Web. See [`@url`, `@uref{url[, text][, replacement]}`](#g_t_0040url).

`@urefbreakstyle style`
:   Specify how `@uref`/`@url` should break at special
    characters: `after`, `before`, `none`.
    See [`@url`, `@uref{url[, text][, replacement]}`](#g_t_0040url).

`@v{c}`
:   Generate check accent over the character c, as in ǒ.
    See [Inserting Accents](#Inserting-Accents).

`@value{txivar}`
:   Insert the value, if any, of the Texinfo variable txivar,
    previously defined by `@set`. See [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value).

`@var{metasyntactic-variable}`
:   Highlight a metasyntactic variable, which is something that stands for
    another piece of text. See [`@var`{metasyntactic-variable}](#g_t_0040var).

`@verb{delim literal delim}`
:   Output literal, delimited by the single character delim,
    exactly as is (in the fixed-width font), including any whitespace or
    Texinfo special characters. See [`@verb`{chartextchar}](#g_t_0040verb).

`@verbatim`
:   Output the text of the environment exactly as is (in the fixed-width
    font). Pair with `@end verbatim`. See [`@verbatim`: Literal Text](#g_t_0040verbatim).

`@verbatiminclude filename`
:   Output the contents of filename exactly as is (in the
    fixed-width font). See [`@verbatiminclude` file: Include a File Verbatim](#g_t_0040verbatiminclude).

`@vindex entry`
:   Add entry to the index of variables. See [Defining the Entries of an Index](#Index-Entries).

`@vskip amount`
:   In a printed manual, insert whitespace so as to push text on the
    remainder of the page towards the bottom of the page. Used in
    formatting the copyright page with the argument ‘`0pt plus
    1filll`’. (Note spelling of ‘`filll`’.) See [Copyright Page](#Copyright).

`@vtable formatting-command`
:   Begin a two-column table, using `@item` for each entry.
    Automatically enter each of the items in the first column into the
    index of variables. Pair with `@end vtable`. The same as
    `@table`, except for indexing. See [`@ftable` and `@vtable`](#g_t_0040ftable-_0040vtable).

`@w{text}`
:   Disallow line breaks within text. See [`@w`{text}: Prevent Line Breaks](#g_t_0040w).

`@xml`
:   Enter XML completely. Pair with `@end xml`. See [Raw Formatter Commands](#Raw-Formatter-Commands).

`@xref{node, [entry], [node-title], [info-file], [printed-manual]}`
:   Make a reference that starts with ‘See’ in a printed manual. Follow
    command with a punctuation mark. The first argument is mandatory, except for
    references to whole manuals. To refer to another manual as a whole,
    the printed-manual and/or the info-file are the only
    required arguments. See [`@xref`](#g_t_0040xref).

`@xrefautomaticsectiontitle on-off`
:   By default, use the section title instead of the node name in cross
    references, including in node headers in HTML. See [`@xref` with Three Arguments](#Three-Arguments).

---

Next: [Obsolete @-Commands](#Obsolete-_0040_002dCommands), Previous: [@-Command List](#Command-List), Up: [@-Command Details](#g_t_0040_002dCommand-Details)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### A.3 @-Command Contexts [¶](#g_t_0040_002dCommand-Contexts)

Here we describe approximately which @-commands can be used in which
contexts. It is not exhaustive or meant to be a complete reference.
Discrepancies between the information here and the Texinfo processors
implementations are most likely to be resolved in favor of the
implementations.

By *general text* below, we mean anything except sectioning and
other such outer-level document commands, such as `@section`,
`@node`, and `@setfilename`.

`@c`, `@comment` and `@if ... @end if` conditional
commands may appear anywhere (except the conditionals must still be on lines by
themselves). `@caption` and `@shortcaption` may only appear in
`@float` but may contain general text. `@footnote` content
likewise.

@-commands with braces marking text (such as `@strong`,
`@sc`, `@asis`) may contain raw formatter commands such as
`@html` but no other block commands (other commands terminated
by `@end`) and may not be split across paragraphs, but may
otherwise contain general text.

In addition to the block command restriction, on `@center`,
`@exdent` and `@item` in `@table` lines, @-commands
that makes only sense in a paragraph are not accepted, such as
`@indent`.

In addition to the above, sectioning commands cannot contain
`@anchor`, `@footnote` or `@verb`.

In addition to the above, remaining commands (`@node`,
`@anchor`, `@printindex`, `@ref`, `@math`,
`@cindex`, `@url`, `@image`, and so on) cannot
contain cross-reference commands (`@ref`, `@xref`,
`@pxref` and `@inforef`).

For precise and complete information, we suggest looking into the
test suite in the sources, which exhaustively tries combinations.

---

Previous: [@-Command Contexts](#Command-Contexts), Up: [@-Command Details](#g_t_0040_002dCommand-Details)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### A.4 Obsolete @-Commands [¶](#Obsolete-_0040_002dCommands-1)

Here are Texinfo @-commands which are obsolete or have been removed
completely. This section is for historical purposes.

`@refill` [¶](#index-_0040refill)
:   This command used to refill and indent the paragraph after all the
    other processing has been done. It is no longer needed, since all
    formatters now automatically refill as needed, but you may still see
    it in the source to some manuals, as it does no harm.

`@setcontentsaftertitlepage` [¶](#index-_0040setcontentsaftertitlepage)
:   In the past, the contents commands were sometimes placed at the end of
    the file, after any indices and just before the `@bye`, but we
    no longer recommend this.
    This command could be used by a user printing a manual, to force the
    contents to be printed after the title page
    (after the ‘`@end titlepage`’ line)
    even if the `@contents` command was at the end of the manual.

`@setshortcontentsaftertitlepage` [¶](#index-_0040setshortcontentsaftertitlepage)
:   This placed the short table of contents after the ‘`@end titlepage`’
    command even if the `@shortcontents` command was at the end.

---

Next: [Sample Texinfo Files](#Sample-Texinfo-Files), Previous: [@-Command Details](#g_t_0040_002dCommand-Details), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## Appendix B Tips and Hints [¶](#Tips-and-Hints)

Here are some tips for writing Texinfo documentation:

* Write in the present tense, not in the past or the future.
* Write actively! For example, write “We recommend that …” rather
  than “It is recommended that …”.
* Use 70 or 72 as your fill column. Longer lines are hard to read.
* Include a copyright notice and copying permissions.
* Design your manual so that it can be read sequentially, as far as
  possible. People tire of flipping back and forth to find information
  that should be presented to them as they need it.

#### Index, Index, Index! [¶](#Index_002c-Index_002c-Index_0021)

Write many index entries, in different ways.
Readers like indices; they are helpful and convenient.

Although it is easiest to write index entries as you write the body of
the text, some people prefer to write entries afterwards. In either
case, write an entry before the paragraph to which it applies. This
way, an index entry points to the first page of a paragraph that is
split across pages.

Here are more index-related hints we have found valuable:

* Write each index entry differently, so each entry refers to a different
  place in the document.
* Write index entries only where a topic is discussed significantly. For
  example, it is not useful to index “debugging information” in a
  chapter on reporting bugs. Someone who wants to know about debugging
  information will certainly not find it in that chapter.
* Consistently capitalize the first word of every concept index entry,
  or else consistently use lowercase. Terse entries often call for
  lowercase; longer entries for capitalization. Whichever case
  convention you use, please use one or the other consistently! Mixing
  the two styles looks bad.
* Always capitalize or use uppercase for those words in an index for
  which this is proper, such as names of countries or acronyms. Always
  use the appropriate case for case-sensitive names, such as those in C or
  Lisp.
* Write the indexing commands that refer to a whole section immediately
  after the section command, and write the indexing commands that refer to
  a paragraph before that paragraph.

  In the example that follows, a blank line comes after the index
  entry for “Leaping”:

  ```
  @section The Dog and the Fox
  @cindex Jumping, in general
  @cindex Leaping

  @cindex Dog, lazy, jumped over
  @cindex Lazy dog jumped over
  @cindex Fox, jumps over dog
  @cindex Quick fox jumps over dog
  The quick brown fox jumps over the lazy dog.
  ```

  (Note that the example shows entries for the same concept that are
  written in different ways—‘`Lazy dog`’, and ‘`Dog, lazy`’—so
  readers can look up the concept in different ways.)

#### Blank Lines [¶](#Blank-Lines)

* Insert a blank line between a sectioning command and the first following
  sentence or paragraph, or between the indexing commands associated with
  the sectioning command and the first following sentence or paragraph, as
  shown in the tip on indexing. It makes the source easier to read.
* Always insert a blank line before a `@table` command and after an
  `@end table` command; but never insert a blank line after an
  `@table` command.

  For example,

  ```
  Types of fox:

  @table @samp
  @item Quick
  Jump over lazy dogs.
  ```

  ```

  ```

  ```
  @item Brown
  Also jump over lazy dogs.
  @end table
  ```

  ```
  @noindent
  On the other hand, ...
  ```

  Insert blank lines before and after `@itemize` … `@end
  itemize` and `@enumerate` … `@end enumerate` in the
  same way.

#### Complete Phrases [¶](#Complete-Phrases)

Complete phrases are easier to read than …

* Write entries in an itemized list as complete sentences; or at least, as
  complete phrases. Incomplete expressions … awkward … like
  this.
* Write the prefatory sentence or phrase for a multi-item list or table as
  a complete expression. Do not write “You can set:”; instead, write
  “You can set these variables:”. The former expression sounds cut off.

#### Editions, Dates and Versions [¶](#Editions_002c-Dates-and-Versions)

Include edition numbers, version numbers, and dates in the
`@copying` text (for people reading the Texinfo file, and for the
legal copyright in the output files). Then use `@insertcopying`
in the `@titlepage` section for people reading the printed
output (see [Short Sample](#Short-Sample)).

It is easiest to handle such version information using `@set`
and `@value`. See [`@value` Example](#g_t_0040value-Example), and [GNU Sample Texts](#GNU-Sample-Texts).

#### Definition Commands [¶](#Definition-Commands-2)

Definition commands are `@deffn`, `@defun`,
`@defmac`, and the like, and enable you to write descriptions in
a uniform format.

* Write just one definition command for each entity you define with a
  definition command. The automatic indexing feature creates an index
  entry that leads the reader to the definition.
* Use `@table` … `@end table` in an appendix that
  contains a summary of functions, not `@deffn` or other definition
  commands.

#### Capitalization [¶](#Capitalization)

* Capitalize “Texinfo”; it is a name. Do not write the ‘`x`’ or
  ‘`i`’ in uppercase.
* Capitalize “Info”; it is a name.
* Write TeX using the `@TeX{}` command. Note the uppercase
  ‘`T`’ and ‘`X`’. This command causes the formatters to
  typeset the name according to the wishes of Donald Knuth, who wrote
  TeX. (Likewise `@LaTeX{}` for LaTeX.)

#### Spaces [¶](#Spaces)

Do not use spaces to format a Texinfo file, except inside of
`@example` … `@end example` and other literal
environments and commands.

For example, TeX fills the following:

```
   @kbd{C-x v}
   @kbd{M-x vc-next-action}
      Perform the next logical operation
      on the version-controlled file
      corresponding to the current buffer.
```

so it looks like this:

> ‘C-x v’ ‘M-x vc-next-action’ Perform the next logical operation on the
> version-controlled file corresponding to the current buffer.

In this case, the text should be formatted with
`@table`, `@item`, and `@itemx`, to create a table.

#### @code, @samp, @var, and ‘`---`’ [¶](#g_t_0040code_002c-_0040samp_002c-_0040var_002c-and-_002d_002d_002d)

* Use `@code` around Lisp symbols, including command names.
  For example,

  ```
  The main function is @code{vc-next-action}, ...
  ```
* Avoid putting letters such as ‘`s`’ immediately after an
  ‘`@code`’. Such letters look bad.
* Use `@var` around meta-variables. Do not write angle brackets
  around them.
* Use three hyphens in a row, ‘`---`’, to indicate a long dash.
  The Info formatter reduces three hyphens to two; a long dash is
  typeset in other output formats.

#### Periods Outside of Quotes [¶](#Periods-Outside-of-Quotes)

Place periods and other punctuation marks *outside* of quotations,
unless the punctuation is part of the quotation. This practice goes
against some publishing conventions in the United States, but enables the
reader to distinguish between the contents of the quotation and the
whole passage.

For example, you should write the following sentence with the period
outside the end quotation marks:

```
Evidently, ‘au’ is an abbreviation for ``author''.
```

since ‘`au`’ does *not* serve as an abbreviation for
‘`author.`’ (with a period following the word).

#### Introducing New Terms [¶](#Introducing-New-Terms)

* Introduce new terms so that a reader who does not know them can
  understand them from context; or write a definition for the term.

  For example, in the following, the terms “check in”, “register” and
  “delta” are all appearing for the first time; the example sentence should be
  rewritten so they are understandable.

  > The major function assists you in checking in a file to your
  > version control system and registering successive sets of changes to
  > it as deltas.
* Use the `@dfn` command around a word being introduced, to indicate
  that the reader should not expect to know the meaning already, and
  should expect to learn the meaning from this passage.

#### Program Invocation Nodes [¶](#Program-Invocation-Nodes)

You can invoke programs such as Emacs, GCC, and `gawk` from a
shell. The documentation for each program should contain a section that
describes this. Unfortunately, if the node names and titles for these
sections are all different, they are difficult for users to find.

So, there is a convention to name such sections with a phrase beginning
with the word ‘Invoking’, as in ‘Invoking Emacs’; this way, users can
find the section easily.

#### ANSI C Syntax [¶](#ANSI-C-Syntax)

When you use `@example` to describe a C function’s calling
conventions, use the ANSI C syntax, like this:

```
void dld_init (char *@var{path});
```

And in the subsequent discussion, refer to the argument values by
writing the same argument names, again highlighted with
`@var`.

Avoid the obsolete style that looks like this:

```
#include <dld.h>

dld_init (path)
  char *path;
```

Also, it is best to avoid writing `#include` above the
declaration just to indicate that the function is declared in a
header file. The practice may give the misimpression that the
`#include` belongs near the declaration of the function. Either
state explicitly which header file holds the declaration or, better
yet, name the header file used for a group of functions at the
beginning of the section that describes the functions.

#### Node Length [¶](#Node-Length)

Keep nodes (sections) to a reasonable length, whatever reasonable
might be in the given context. Don’t hesitate to break up long nodes
into subnodes and have an extensive tree structure; that’s what it’s
there for. Many times, readers will probably try to find a single
specific point in the manual, using search, indexing, or just plain
guessing, rather than reading the whole thing from beginning to end.

You can use the `texi-elements-by-size` utility to see a list
of all nodes (or sections) in the document, sorted by size (either
lines or words), to find candidates for splitting. It’s in the
`util/` subdirectory of the Texinfo sources.

#### Bad Examples [¶](#Bad-Examples)

Here are several examples of bad writing to avoid:

In this example, say, “ … you must `@dfn`{check
in} the new version.” That flows better.

> When you are done editing the file, you must perform a
> `@dfn`{check in}.

In the following example, say, “… makes a unified interface such as VC
mode possible.”

> SCCS, RCS and other version-control systems all perform similar
> functions in broadly similar ways (it is this resemblance which makes
> a unified control mode like this possible).

And in this example, you should specify what ‘it’ refers to:

> If you are working with other people, it assists in coordinating
> everyone’s changes so they do not step on each other.

#### And Finally … [¶](#And-Finally-_2026)

* Pronounce TeX as if the ‘`X`’ were a Greek ‘chi’, as the last
  sound in the name ‘Bach’. But pronounce Texinfo as in ‘speck’:
  “teckinfo”.
* Write notes for yourself at the very end of a Texinfo file after the
  `@bye`. None of the processors process text after the
  `@bye`; it is as if the text were within `@ignore` …
  `@end ignore`.

---

Next: [Using Texinfo Mode](#Texinfo-Mode), Previous: [Tips and Hints](#Tips), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## Appendix C Sample Texinfo Files [¶](#Sample-Texinfo-Files-1)

This appendix includes texts to be used in GNU manuals.

* [GNU Sample Texts](#GNU-Sample-Texts)
* [Verbatim Copying License](#Verbatim-Copying-License)
* [All-permissive Copying License](#All_002dpermissive-Copying-License)

---

Next: [Verbatim Copying License](#Verbatim-Copying-License), Up: [Sample Texinfo Files](#Sample-Texinfo-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### C.1 GNU Sample Texts [¶](#GNU-Sample-Texts-1)

Following is a sample Texinfo document with the full texts that should
be used (adapted as necessary) in GNU manuals.

As well as the legal texts, it also serves as a practical example of how
many elements in a GNU system can affect the manual. If you’re not
familiar with all these different elements, don’t worry. They’re not
required and a perfectly good manual can be written without them.
They’re included here nonetheless because many manuals do (or could)
benefit from them.

See [Short Sample](#Short-Sample), for a minimal example of a Texinfo file.

Here are some notes on the example:

* The `version.texi` in the `@include` command is maintained
  automatically by Automake (see [Texinfo](https://www.gnu.org/software/automake/manual/automake.html#Texinfo) in GNU Automake).
  It sets the ‘`VERSION`’, ‘`UPDATED`’ and ‘`UPDATED-MONTH`’
  values used elsewhere. If your distribution doesn’t use Automake, but
  you do use Emacs, you may find the `time-stamp.el` package helpful
  (see [Time Stamps](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Time-Stamps) in The GNU Emacs Manual).
* The `@syncodeindex` command reflects the recommendation to use
  only one index where possible, to make it easier for readers to look up
  index entries.
* The `@dircategory` specify a category for the manual. It is
  used for constructing the Info directory. See [Directory Category](#Directory-Category),
  which includes a variety of recommended category names.
  See [Installing Info Directory Files](#Installing-Dir-Entries).
* The ‘Invoking’ node is a GNU standard to help users find the basic
  information about command-line usage of a given program. See [Manual
  Structure Details](https://www.gnu.org/prep/standards/standards.html#Manual-Structure-Details) in GNU Coding Standards.
* This sample shows how to includes the FDL in a manual using the
  `@include` command. The `fdl.texi` file
  is available in the Texinfo and other GNU source distributions. It is
  also available on the GNU website (at
  <https://www.gnu.org/licenses/fdl-1.3.html>) along with guidance
  for using it.
* If the FSF is not the copyright holder, then use the appropriate name.
* For documents that express your personal views, feelings or
  experiences, it is more appropriate to use a license permitting only
  verbatim copying, rather than the FDL. See [Verbatim Copying License](#Verbatim-Copying-License).

Here is the sample document:

```
\input texinfo   @c -*-texinfo-*-
@comment %**start of header
@include version.texi
@settitle GNU Sample @value{VERSION}
@syncodeindex pg cp
@comment %**end of header
@copying
This manual is for GNU Sample (version @value{VERSION}, @value{UPDATED}),
which is an example in the Texinfo documentation.

Copyright @copyright{} 2016 Free Software Foundation, Inc.

@quotation
Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with no
Invariant Sections, with no Front-Cover Texts, and with no Back-Cover
Texts.  A copy of the license is included in the section entitled
``GNU Free Documentation License''.
@end quotation
@end copying

@dircategory Texinfo documentation system
@direntry
* sample: (sample)Invoking sample.
@end direntry

@titlepage
@title GNU Sample
@subtitle for version @value{VERSION}, @value{UPDATED}
@author A.U. Thor (@email{bug-sample@@gnu.org})
@page
@vskip 0pt plus 1filll
@insertcopying
@end titlepage

@contents

@node Top
@top GNU Sample

This manual is for GNU Sample (version @value{VERSION}, @value{UPDATED}).

@menu
* Invoking sample::
* GNU Free Documentation License::
* Index::
@end menu

@node Invoking sample
@chapter Invoking sample

@pindex sample
@cindex invoking @command{sample}

This is a sample manual.  There is no sample program to
invoke, but if there were, you could see its basic usage
and command line options here.

@node GNU Free Documentation License
@appendix GNU Free Documentation License

@include fdl.texi

@node Index
@unnumbered Index

@printindex cp

@bye
```

---

Next: [All-permissive Copying License](#All_002dpermissive-Copying-License), Previous: [GNU Sample Texts](#GNU-Sample-Texts), Up: [Sample Texinfo Files](#Sample-Texinfo-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### C.2 Verbatim Copying License [¶](#Verbatim-Copying-License-1)

For software manuals and other documentation, it is critical to use a
license permitting free redistribution and updating, so that when a free
program is changed, the documentation can be updated as well.

On the other hand, for documents that express your personal views,
feelings or experiences, it is more appropriate to use a license
permitting only verbatim copying.

Here is a sample text for such a license permitting verbatim copying only.
This is just the license text itself. For a complete sample document,
see the previous sections.

```
@copying
This document is a sample for allowing verbatim copying only.

Copyright @copyright{} 2016 Free Software Foundation, Inc.

@quotation
Permission is granted to make and distribute verbatim copies
of this entire document without royalty provided the
copyright notice and this permission notice are preserved.
@end quotation
@end copying
```

---

Previous: [Verbatim Copying License](#Verbatim-Copying-License), Up: [Sample Texinfo Files](#Sample-Texinfo-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### C.3 All-permissive Copying License [¶](#All_002dpermissive-Copying-License-1)

For software manuals and other documentation, it is important to use a
license permitting free redistribution and updating, so that when a free
program is changed, the documentation can be updated as well.

On the other hand, for small supporting files, short manuals (under 300
lines long) and rough documentation (README files, INSTALL files, etc.),
the full FDL would be overkill. They can use a simple all-permissive
license.

Here is a sample text for such an all-permissive license. This is just
the license text itself. For a complete sample document, see the
previous sections.

```
Copyright @copyright{} 2016 Free Software Foundation, Inc.

Copying and distribution of this file, with or without modification,
are permitted in any medium without royalty provided the copyright
notice and this notice are preserved.
```

---

Next: [Global Document Commands](#Global-Document-Commands), Previous: [Sample Texinfo Files](#Sample-Texinfo-Files), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## Appendix D Using Texinfo Mode [¶](#Using-Texinfo-Mode)

You may edit a Texinfo file with any text editor you choose. A Texinfo
file is no different from any other ASCII file. However, GNU Emacs
comes with a special mode, called Texinfo mode, that provides Emacs
commands and tools to help ease your work.

* [Texinfo Mode Overview](#Texinfo-Mode-Overview)
* [The Usual GNU Emacs Editing Commands](#Emacs-Editing)
* [Inserting Frequently Used Commands](#Inserting)
* [Showing the Sectioning Structure of a File](#Showing-the-Structure)
* [Updating Nodes and Menus](#Updating-Nodes-and-Menus)
* [Formatting for Info](#Info-Formatting)
* [Formatting and Printing with Emacs](#Printing-with-Emacs)
* [Texinfo Mode Summary](#Texinfo-Mode-Summary)
* [Direct Formatting of Info files](#Formatting-Info-files)
* [Catching Mistakes](#Catching-Mistakes)
* [Batch Formatting](#Batch-Formatting)

---

Next: [The Usual GNU Emacs Editing Commands](#Emacs-Editing), Up: [Using Texinfo Mode](#Texinfo-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### D.1 Texinfo Mode Overview [¶](#Texinfo-Mode-Overview-1)

Texinfo mode provides special features for working with Texinfo files.
You can:

* Insert frequently used @-commands.
* Automatically create `@node` lines.
* Show the structure of a Texinfo source file.
* Automatically create or update the ‘Next’,
  ‘Previous’, and ‘Up’ pointers of a node.
* Automatically create or update menus.
* Automatically create a master menu.
* Format a part or all of a file for Info.
* Typeset and print part or all of a file.

Perhaps the two most helpful features are those for inserting frequently
used @-commands and for creating node pointers and menus.

---

Next: [Inserting Frequently Used Commands](#Inserting), Previous: [Texinfo Mode Overview](#Texinfo-Mode-Overview), Up: [Using Texinfo Mode](#Texinfo-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### D.2 The Usual GNU Emacs Editing Commands [¶](#The-Usual-GNU-Emacs-Editing-Commands)

In most cases, the usual Text mode commands work the same in Texinfo
mode as they do in Text mode. Texinfo mode adds new editing commands
and tools to GNU Emacs’ general purpose editing features. The major
difference concerns filling. In Texinfo mode, the paragraph
separation variable and syntax table are redefined so that Texinfo
commands that should be on lines of their own are not inadvertently
included in paragraphs. Thus, the `M-q` (`fill-paragraph`)
command will refill a paragraph but not mix an indexing command on a
line adjacent to it into the paragraph.

In addition, Texinfo mode sets the `page-delimiter` variable to
the value of `texinfo-chapter-level-regexp`; by default, this is
a regular expression matching the commands for chapters and their
equivalents, such as appendices. With this value for the page
delimiter, you can jump from chapter title to chapter title with the
`C-x ]` (`forward-page`) and `C-x [`
(`backward-page`) commands and narrow to a chapter with the
`C-x n p` (`narrow-to-page`) command. (See [Pages](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Pages) in The GNU Emacs Manual, for details about the page commands.)

GNU Emacs automatically enters Texinfo mode when you visit a
file with a `.texinfo`, `.texi` or `.txi`
extension. Also, Emacs switches to Texinfo mode
when you visit a
file that has ‘`-*-texinfo-*-`’ in its first line. If ever you are
in another mode and wish to switch to Texinfo mode, type `M-x
texinfo-mode`.

Like all other Emacs features, you can customize or enhance Texinfo
mode as you wish. In particular, the keybindings are very easy to
change. The keybindings described here are the default or standard
ones.

---

Next: [Showing the Sectioning Structure of a File](#Showing-the-Structure), Previous: [The Usual GNU Emacs Editing Commands](#Emacs-Editing), Up: [Using Texinfo Mode](#Texinfo-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]
