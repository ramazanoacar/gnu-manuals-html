Next: [Texinfo Copying Conditions](#Copying-Conditions), Up: [(dir)](/manual/)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]# Texinfo [¶](#Texinfo)

This manual is for GNU Texinfo (version 7.1, 18 October 2023),
a documentation system that can produce both online information and a
printed manual from a single source using semantic markup.## Short Table of Contents

* [Texinfo Copying Conditions](#toc-Texinfo-Copying-Conditions)
* [1 Overview of Texinfo](#toc-Overview-of-Texinfo)
* [2 Writing a Texinfo File](#toc-Writing-a-Texinfo-File-1)
* [3 Nodes](#toc-Nodes-1)
* [4 Chapter Structuring](#toc-Chapter-Structuring-1)
* [5 Cross-references](#toc-Cross_002dreferences)
* [6 Marking Text, Words and Phrases](#toc-Marking-Text_002c-Words-and-Phrases)
* [7 Quotations and Examples](#toc-Quotations-and-Examples-1)
* [8 Lists and Tables](#toc-Lists-and-Tables-1)
* [9 Special Displays](#toc-Special-Displays-1)
* [10 Indices](#toc-Indices-1)
* [11 Special Insertions](#toc-Special-Insertions)
* [12 Forcing and Preventing Breaks](#toc-Forcing-and-Preventing-Breaks)
* [13 Definition Commands](#toc-Definition-Commands-1)
* [14 Internationalization](#toc-Internationalization-1)
* [15 Conditionally Visible Text](#toc-Conditionally-Visible-Text)
* [16 Defining New Texinfo Commands](#toc-Defining-New-Texinfo-Commands-1)
* [17 Include Files](#toc-Include-Files-1)
* [18 Formatting and Printing with TeX](#toc-Formatting-and-Printing-with-TeX)
* [19 `texi2any`: The Translator for Texinfo](#toc-texi2any_003a-The-Translator-for-Texinfo)
* [20 Creating and Installing Info Files](#toc-Creating-and-Installing-Info-Files-1)
* [21 Generating HTML](#toc-Generating-HTML-1)
* [Appendix A @-Command Details](#toc-_0040_002dCommand-Details-1)
* [Appendix B Tips and Hints](#toc-Tips-and-Hints)
* [Appendix C Sample Texinfo Files](#toc-Sample-Texinfo-Files-1)
* [Appendix D Using Texinfo Mode](#toc-Using-Texinfo-Mode)
* [Appendix E Global Document Commands](#toc-Global-Document-Commands-1)
* [Appendix F Info Format Specification](#toc-Info-Format-Specification-1)
* [Appendix G GNU Free Documentation License](#toc-GNU-Free-Documentation-License-1)
* [Command and Variable Index](#toc-Command-and-Variable-Index-1)
* [General Index](#toc-General-Index-1)## Table of Contents

* [Texinfo Copying Conditions](#Copying-Conditions)
* [1 Overview of Texinfo](#Overview)
  + [1.1 Reporting Bugs](#Reporting-Bugs)
  + [1.2 Output Formats](#Output-Formats)
  + [1.3 Info Files](#Info-Files)
  + [1.4 Printed Books](#Printed-Books)
  + [1.5 Adding Output Formats](#Adding-Output-Formats)
  + [1.6 History](#History)
* [2 Writing a Texinfo File](#Writing-a-Texinfo-File)
  + [2.1 General Syntactic Conventions](#Conventions)
  + [2.2 Comments](#Comments)
  + [2.3 What a Texinfo File Must Have](#Minimum)
  + [2.4 Short Sample](#Short-Sample-Texinfo-File)
  + [2.5 Texinfo File Header](#Texinfo-File-Header)
    - [2.5.1 The First Line of a Texinfo File](#First-Line)
    - [2.5.2 `@setfilename`: Set the Output File Name](#g_t_0040setfilename)
    - [2.5.3 `@settitle`: Set the Document Title](#g_t_0040settitle)
    - [2.5.4 Preamble](#Preamble)
    - [2.5.5 Start and End of Header for Emacs](#Start-and-End-of-Header)
  + [2.6 Directory Category](#Directory-Category)
  + [2.7 Document Permissions](#Document-Permissions)
    - [2.7.1 `@copying`: Declare Copying Permissions](#g_t_0040copying)
    - [2.7.2 `@insertcopying`: Include Permissions Text](#g_t_0040insertcopying)
  + [2.8 Title and Copyright Pages](#Titlepage-_0026-Copyright-Page)
    - [2.8.1 `@titlepage`](#g_t_0040titlepage)
    - [2.8.2 `@title`, `@subtitle`, and `@author`](#g_t_0040title-_0040subtitle-_0040author)
    - [2.8.3 `@titlefont`, `@center`, and `@sp`](#g_t_0040titlefont-_0040center-_0040sp)
    - [2.8.4 Copyright Page](#Copyright)
    - [2.8.5 Heading Generation](#Heading-Generation)
  + [2.9 Generating a Table of Contents](#Contents)
  + [2.10 The ‘Top’ Node and Master Menu](#The-Top-Node)
    - [2.10.1 Parts of a Master Menu](#Master-Menu-Parts)
  + [2.11 The Body of the Document](#The-Body-of-the-Document)
  + [2.12 Ending a Texinfo File](#Ending-a-File)
* [3 Nodes](#Nodes)
  + [3.1 Writing a `@node` Line](#Writing-a-Node)
  + [3.2 Choosing Node Names](#Node-Names)
  + [3.3 `@node` Line Requirements](#Node-Line-Requirements)
  + [3.4 The First Node](#First-Node)
  + [3.5 The `@top` Sectioning Command](#g_t_0040top-Command)
  + [3.6 Texinfo Document Structure](#Texinfo-Document-Structure)
  + [3.7 Node and Menu Illustration](#Node-Menu-Illustration)
  + [3.8 Node Descriptions](#Node-Descriptions)
  + [3.9 Menus](#Menus)
    - [3.9.1 Writing a Menu](#Writing-a-Menu)
    - [3.9.2 A Menu Example](#Menu-Example)
    - [3.9.3 Menu Location](#Menu-Location)
    - [3.9.4 The Parts of a Menu](#Menu-Parts)
    - [3.9.5 Less Cluttered Menu Entry](#Less-Cluttered-Menu-Entry)
    - [3.9.6 Referring to Other Info Files](#Other-Info-Files)
* [4 Chapter Structuring](#Chapter-Structuring)
  + [4.1 Tree Structure of Sections](#Tree-Structuring)
  + [4.2 Structuring Command Types](#Structuring-Command-Types)
  + [4.3 `@chapter`: Chapter Structuring](#g_t_0040chapter)
  + [4.4 `@unnumbered`, `@appendix`: Chapters with Other Labeling](#g_t_0040unnumbered-_0040appendix)
  + [4.5 `@majorheading`, `@chapheading`: Chapter-level Headings](#g_t_0040majorheading-_0040chapheading)
  + [4.6 `@section`: Sections Below Chapters](#g_t_0040section)
  + [4.7 `@unnumberedsec`, `@appendixsec`, `@heading`](#g_t_0040unnumberedsec-_0040appendixsec-_0040heading)
  + [4.8 `@subsection`: Subsections Below Sections](#g_t_0040subsection)
  + [4.9 The `@subsection`-like Commands](#g_t_0040unnumberedsubsec-_0040appendixsubsec-_0040subheading)
  + [4.10 `@subsubsection` and Other Subsub Commands](#g_t_0040subsubsection)
  + [4.11 `@part`: Groups of Chapters](#g_t_0040part)
  + [4.12 Raise/lower Sections: `@raisesections` and `@lowersections`](#Raise_002flower-sections)
* [5 Cross-references](#Cross-References)
  + [5.1 Different Cross-reference Commands](#Cross-Reference-Commands)
  + [5.2 Parts of a Cross-reference](#Cross-Reference-Parts)
  + [5.3 `@xref` with One Argument](#One-Argument)
  + [5.4 `@xref` with Two Arguments](#Two-Arguments)
  + [5.5 `@xref` with Three Arguments](#Three-Arguments)
  + [5.6 `@xref` with Four and Five Arguments](#Four-and-Five-Arguments)
  + [5.7 Referring to a Manual as a Whole](#Referring-to-a-Manual-as-a-Whole)
  + [5.8 `@xref`](#g_t_0040xref)
  + [5.9 `@ref`](#g_t_0040ref)
  + [5.10 `@pxref`](#g_t_0040pxref)
  + [5.11 `@anchor`: Defining Arbitrary Cross-reference Targets](#g_t_0040anchor)
  + [5.12 `@link`: Plain, unadorned hyperlink](#g_t_0040link)
  + [5.13 `@inforef`: Cross-references to Info-only Material](#g_t_0040inforef)
  + [5.14 `@url`, `@uref{url[, text][, replacement]}`](#g_t_0040url)
    - [5.14.1 `@url` Examples](#g_t_0040url-Examples)
    - [5.14.2 URL Line Breaking](#URL-Line-Breaking)
    - [5.14.3 `@url` PDF Output Format](#g_t_0040url-PDF-Output-Format)
  + [5.15 `@cite`{reference}](#g_t_0040cite)
  + [5.16 PDF Colors](#PDF-Colors)
* [6 Marking Text, Words and Phrases](#Marking-Text)
  + [6.1 Indicating Definitions, Commands, etc.](#Indicating)
    - [6.1.1 Highlighting Commands are Useful](#Useful-Highlighting)
    - [6.1.2 `@code`{sample-code}](#g_t_0040code)
    - [6.1.3 `@kbd`{keyboard-characters}](#g_t_0040kbd)
    - [6.1.4 `@key`{key-name}](#g_t_0040key)
    - [6.1.5 `@samp`{text}](#g_t_0040samp)
    - [6.1.6 `@verb`{chartextchar}](#g_t_0040verb)
    - [6.1.7 `@var`{metasyntactic-variable}](#g_t_0040var)
    - [6.1.8 `@env`{environment-variable}](#g_t_0040env)
    - [6.1.9 `@file`{file-name}](#g_t_0040file)
    - [6.1.10 `@command`{command-name}](#g_t_0040command)
    - [6.1.11 `@option`{option-name}](#g_t_0040option)
    - [6.1.12 `@dfn`{term}](#g_t_0040dfn)
    - [6.1.13 `@abbr`{abbreviation[, meaning]}](#g_t_0040abbr)
    - [6.1.14 `@acronym`{acronym[, meaning]}](#g_t_0040acronym)
    - [6.1.15 `@indicateurl`{uniform-resource-locator}](#g_t_0040indicateurl)
    - [6.1.16 `@email`{email-address[, displayed-text]}](#g_t_0040email)
  + [6.2 Emphasizing Text](#Emphasis)
    - [6.2.1 `@emph`{text} and `@strong`{text}](#g_t_0040emph-_0040strong)
    - [6.2.2 `@sc`{text}: The Small Caps Font](#Smallcaps)
    - [6.2.3 Fonts for Printing](#Fonts)
* [7 Quotations and Examples](#Quotations-and-Examples)
  + [7.1 Block Enclosing Commands](#Block-Enclosing-Commands)
  + [7.2 `@quotation`: Block Quotations](#g_t_0040quotation)
  + [7.3 `@indentedblock`: Indented text blocks](#g_t_0040indentedblock)
  + [7.4 `@example`: Example Text](#g_t_0040example)
  + [7.5 `@verbatim`: Literal Text](#g_t_0040verbatim)
  + [7.6 `@lisp`: Marking a Lisp Example](#g_t_0040lisp)
  + [7.7 `@display`: Examples Using the Text Font](#g_t_0040display)
  + [7.8 `@format`: Examples Using the Full Line Width](#g_t_0040format)
  + [7.9 `@exdent`: Undoing a Line’s Indentation](#g_t_0040exdent)
  + [7.10 `@flushleft` and `@flushright`](#g_t_0040flushleft-_0040flushright)
  + [7.11 `@raggedright`: Ragged Right Text](#g_t_0040raggedright)
  + [7.12 `@noindent`: Omitting Indentation](#g_t_0040noindent)
  + [7.13 `@indent`: Forcing Indentation](#g_t_0040indent)
  + [7.14 `@cartouche`: Rounded Rectangles](#g_t_0040cartouche)
  + [7.15 `@small…` Block Commands](#small)
* [8 Lists and Tables](#Lists-and-Tables)
  + [8.1 Introducing Lists](#Introducing-Lists)
  + [8.2 `@itemize`: Making an Itemized List](#g_t_0040itemize)
  + [8.3 `@enumerate`: Making a Numbered or Lettered List](#g_t_0040enumerate)
  + [8.4 Making a Two-column Table](#Two_002dcolumn-Tables)
    - [8.4.1 Using the `@table` Command](#g_t_0040table)
    - [8.4.2 `@ftable` and `@vtable`](#g_t_0040ftable-_0040vtable)
    - [8.4.3 `@itemx`: Second and Subsequent Items](#g_t_0040itemx)
  + [8.5 `@multitable`: Multi-column Tables](#Multi_002dcolumn-Tables)
    - [8.5.1 Multitable Column Widths](#Multitable-Column-Widths)
    - [8.5.2 Multitable Rows](#Multitable-Rows)
* [9 Special Displays](#Special-Displays)
  + [9.1 Floats](#Floats)
    - [9.1.1 `@float` [type][,label]: Floating Material](#g_t_0040float)
    - [9.1.2 `@caption` & `@shortcaption`](#g_t_0040caption-_0040shortcaption)
    - [9.1.3 `@listoffloats`: Tables of Contents for Floats](#g_t_0040listoffloats)
  + [9.2 Inserting Images](#Images)
    - [9.2.1 Image Syntax](#Image-Syntax)
    - [9.2.2 Image Scaling](#Image-Scaling)
  + [9.3 Footnotes](#Footnotes)
    - [9.3.1 Footnote Commands](#Footnote-Commands)
    - [9.3.2 Footnote Styles](#Footnote-Styles)
* [10 Indices](#Indices)
  + [10.1 Predefined Indices](#Predefined-Indices)
  + [10.2 Defining the Entries of an Index](#Indexing-Commands)
  + [10.3 Advanced Indexing Commands](#Advanced-Indexing)
  + [10.4 Making Index Entries](#Index-Entries)
  + [10.5 Printing Indices and Menus](#Printing-Indices-_0026-Menus)
  + [10.6 Combining Indices](#Combining-Indices)
    - [10.6.1 `@syncodeindex`: Combining Indices Using `@code`](#g_t_0040syncodeindex)
    - [10.6.2 `@synindex`: Combining Indices](#g_t_0040synindex)
  + [10.7 Defining New Indices](#New-Indices)
* [11 Special Insertions](#Insertions)
  + [11.1 Special Characters: Inserting @ {} , \ # &](#Special-Characters)
    - [11.1.1 Inserting ‘@’ with `@@` and `@atchar{}`](#Inserting-an-Atsign)
    - [11.1.2 Inserting ‘{ ‘}’ with `@{ @}` and `@l rbracechar{}`](#Inserting-Braces)
    - [11.1.3 Inserting ‘,’ with `@comma{}`](#Inserting-a-Comma)
    - [11.1.4 Inserting ‘\’ with `@backslashchar{}`](#Inserting-a-Backslash)
    - [11.1.5 Inserting ‘#’ with `@hashchar{}`](#Inserting-a-Hashsign)
    - [11.1.6 Inserting ‘&’ with `@&` and `@ampchar{}`](#Inserting-an-Ampersand)
  + [11.2 Inserting Quote Characters](#Inserting-Quote-Characters)
  + [11.3 Inserting Space](#Inserting-Space)
    - [11.3.1 Multiple Spaces](#Multiple-Spaces)
    - [11.3.2 Not Ending a Sentence](#Not-Ending-a-Sentence)
    - [11.3.3 Ending a Sentence](#Ending-a-Sentence)
    - [11.3.4 `@frenchspacing` val: Control Sentence Spacing](#g_t_0040frenchspacing)
    - [11.3.5 `@dmn`{dimension}: Format a Dimension](#g_t_0040dmn)
  + [11.4 Inserting Accents](#Inserting-Accents)
  + [11.5 Inserting Quotation Marks](#Inserting-Quotation-Marks)
  + [11.6 `@sub` and `@sup`: Inserting Subscripts and Superscripts](#Inserting-Subscripts-and-Superscripts)
  + [11.7 `@math` and `@displaymath`: Formatting Mathematics](#Inserting-Math)
  + [11.8 Glyphs for Text](#Glyphs-for-Text)
    - [11.8.1 `@TeX`{} (TeX) and `@LaTeX`{} (LaTeX)](#g_t_0040TeX-_0040LaTeX)
    - [11.8.2 `@copyright{}` (©)](#g_t_0040copyright)
    - [11.8.3 `@registeredsymbol{}` (®)](#g_t_0040registeredsymbol)
    - [11.8.4 `@dots` (…) and `@enddots` (...)](#g_t_0040dots)
    - [11.8.5 `@bullet` (•)](#g_t_0040bullet)
    - [11.8.6 `@euro` (€): Euro Currency Symbol](#g_t_0040euro)
    - [11.8.7 `@pounds` (£): Pounds Sterling](#g_t_0040pounds)
    - [11.8.8 `@textdegree` (°): Degrees Symbol](#g_t_0040textdegree)
    - [11.8.9 `@minus` (−): Inserting a Minus Sign](#g_t_0040minus)
    - [11.8.10 `@geq` (≥) and `@leq` (≤): Inserting Relations](#g_t_0040geq-_0040leq)
  + [11.9 Glyphs for Programming](#Glyphs-for-Programming)
    - [11.9.1 Glyphs Summary](#Glyphs-Summary)
    - [11.9.2 `@result{}` (⇒): Result of an Expression](#g_t_0040result)
    - [11.9.3 `@expansion{}` (→): Indicating an Expansion](#g_t_0040expansion)
    - [11.9.4 `@print{}` (-|): Indicating Generated Output](#g_t_0040print)
    - [11.9.5 `@error{}` (error→): Indicating an Error Message](#g_t_0040error)
    - [11.9.6 `@equiv{}` (≡): Indicating Equivalence](#g_t_0040equiv)
    - [11.9.7 `@point{}` (∗): Indicating Point in a Buffer](#g_t_0040point)
    - [11.9.8 Click Sequences](#Click-Sequences)
  + [11.10 Inserting Unicode: `@U`](#Inserting-Unicode)
* [12 Forcing and Preventing Breaks](#Breaks)
  + [12.1 Break Commands](#Break-Commands)
  + [12.2 `@*` and `@/`: Generate and Allow Line Breaks](#Line-Breaks)
  + [12.3 `@-` and `@hyphenation`: Hyphenation in Printed Output](#g_t_0040_002d-_0040hyphenation)
  + [12.4 `@allowcodebreaks`: Control Line Breaks in `@code`](#g_t_0040allowcodebreaks)
  + [12.5 `@w`{text}: Prevent Line Breaks](#g_t_0040w)
  + [12.6 `@tie{}`: Inserting an Unbreakable Space](#g_t_0040tie)
  + [12.7 `@sp` n: Insert Blank Lines](#g_t_0040sp)
  + [12.8 `@page`: Start a New Page](#g_t_0040page)
  + [12.9 `@group`: Prevent Page Breaks](#g_t_0040group)
  + [12.10 `@need mils`: Prevent Page Breaks](#g_t_0040need)
* [13 Definition Commands](#Definition-Commands)
  + [13.1 The Template for a Definition](#Def-Cmd-Template)
  + [13.2 Definition Command Continuation Lines](#Def-Cmd-Continuation-Lines)
  + [13.3 Optional and Repeated Arguments](#Optional-Arguments)
  + [13.4 Omitting the Space After a Definition Name](#No-Space-After-Definition-Name)
  + [13.5 `@deffnx`, et al.: Two or More ‘First’ Lines](#g_t_0040deffnx)
  + [13.6 The Definition Commands](#Def-Cmds-in-Detail)
    - [13.6.1 Functions and Similar Entities](#Functions-Commands)
    - [13.6.2 Functions in Typed Languages](#Typed-Functions)
    - [13.6.3 Variables and Similar Entities](#Variables-Commands)
    - [13.6.4 Variables in Typed Languages](#Typed-Variables)
    - [13.6.5 Data Types](#Data-Types)
    - [13.6.6 Object-Oriented Programming](#Abstract-Objects)
      * [13.6.6.1 Object-Oriented Variables](#Object_002dOriented-Variables)
      * [13.6.6.2 Object-Oriented Methods](#Object_002dOriented-Methods)
  + [13.7 Generic Definition Commands](#Generic-Definition-Commands)
  + [13.8 Conventions for Writing Definitions](#Def-Cmd-Conventions)
  + [13.9 A Sample Function Definition](#Sample-Function-Definition)
* [14 Internationalization](#Internationalization)
  + [14.1 `@documentlanguage ll[_cc]`: Set the Document Language](#g_t_0040documentlanguage)
  + [14.2 `@documentencoding enc`: Set Input Encoding](#g_t_0040documentencoding)
* [15 Conditionally Visible Text](#Conditionals)
  + [15.1 Conditional Commands](#Conditional-Commands)
  + [15.2 Conditional Not Commands](#Conditional-Not-Commands)
  + [15.3 Raw Formatter Commands](#Raw-Formatter-Commands)
  + [15.4 Inline Conditionals: `@inline`, `@inlineifelse`, `@inlineraw`](#Inline-Conditionals)
  + [15.5 Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value)
    - [15.5.1 `@set` and `@value`](#g_t_0040set-_0040value)
    - [15.5.2 `@ifset` and `@ifclear`](#g_t_0040ifset-_0040ifclear)
    - [15.5.3 `@inlineifset` and `@inlineifclear`](#g_t_0040inlineifset-_0040inlineifclear)
    - [15.5.4 `@value` Example](#g_t_0040value-Example)
  + [15.6 Testing for Texinfo Commands: `@ifcommanddefined`, `@ifcommandnotdefined`](#Testing-for-Texinfo-Commands)
  + [15.7 Conditional Nesting](#Conditional-Nesting)
* [16 Defining New Texinfo Commands](#Defining-New-Texinfo-Commands)
  + [16.1 Defining Macros](#Defining-Macros)
  + [16.2 Invoking Macros](#Invoking-Macros)
  + [16.3 Macro Details and Caveats](#Macro-Details)
  + [16.4 ‘`@alias new=existing`’](#g_t_0040alias)
  + [16.5 Line Macros](#Line-Macros)
  + [16.6 `@definfoenclose`: Customized Highlighting](#g_t_0040definfoenclose)
  + [16.7 External Macro Processors: Line Directives](#External-Macro-Processors)
    - [16.7.1 ‘`#line`’ Directive](#g_t_0023line-Directive)
    - [16.7.2 ‘`#line`’ and TeX](#g_t_0023line-and-TeX)
    - [16.7.3 ‘`#line`’ Syntax Details](#g_t_0023line-Syntax-Details)
* [17 Include Files](#Include-Files)
  + [17.1 How to Use Include Files](#Using-Include-Files)
  + [17.2 Sample File with `@include`](#Sample-Include-File)
  + [17.3 `@verbatiminclude` file: Include a File Verbatim](#g_t_0040verbatiminclude)
* [18 Formatting and Printing with TeX](#Hardcopy-with-TeX)
  + [18.1 Use TeX](#Use-TeX)
  + [18.2 Format with `texi2dvi` or `texi2pdf`](#Format-with-texi2dvi-or-texi2pdf)
  + [18.3 Format with `tex`/`texindex`](#Format-with-tex_002ftexindex)
    - [18.3.1 Formatting Partial Documents](#Formatting-Partial-Documents)
    - [18.3.2 Details of `texindex`](#Details-of-texindex)
  + [18.4 Print with `lpr` from Shell](#Print-with-lpr)
  + [18.5 Preparing for TeX](#Preparing-for-TeX)
  + [18.6 Overfull “hboxes”](#Overfull-hboxes)
* [19 `texi2any`: The Translator for Texinfo](#Generic-Translator-texi2any)
  + [19.1 Invoking `texi2any` from a Shell](#Invoking-texi2any)
  + [19.2 Environment Variables Recognized by `texi2any`](#texi2any-Environment-Variables)
  + [19.3 `texi2any` Printed Output](#texi2any-Printed-Output)
  + [19.4 Customization Variables](#Customization-Variables)
    - [19.4.1 Customization Variables for @-Commands](#Customization-Variables-for-_0040_002dCommands)
    - [19.4.2 Customization Variables and Options](#Customization-Variables-and-Options)
    - [19.4.3 HTML Customization Variables](#HTML-Customization-Variables)
    - [19.4.4 MathJax Customization Variables](#MathJax-Customization-Variables)
    - [19.4.5 `latex2html` Customization Variables](#latex2html-Customization-Variables)
    - [19.4.6 `tex4ht` Customization Variables](#tex4ht-Customization-Variables)
    - [19.4.7 LaTeX Customization Variables](#LaTeX-Customization-Variables)
    - [19.4.8 Other Customization Variables](#Other-Customization-Variables)
  + [19.5 Internationalization of Document Strings](#Internationalization-of-Document-Strings)
  + [19.6 Invoking `pod2texi`: Convert Pod to Texinfo](#Invoking-pod2texi)
    - [19.6.1 pod2texi](#pod2texi-manual-page)
  + [19.7 `texi2html`: Ancestor of `texi2any`](#texi2html)
* [20 Creating and Installing Info Files](#Creating-and-Installing-Info-Files)
  + [20.1 Installing an Info File](#Installing-an-Info-File)
    - [20.1.1 The Directory File `dir`](#Directory-File)
    - [20.1.2 Listing a New Info File](#New-Info-File)
    - [20.1.3 Info Files in Other Directories](#Other-Info-Directories)
    - [20.1.4 Installing Info Directory Files](#Installing-Dir-Entries)
    - [20.1.5 Invoking `install-info`](#Invoking-install_002dinfo)
  + [20.2 Tag Files and Split Files](#Tag-and-Split-Files)
  + [20.3 Info Format FAQ](#Info-Format-FAQ)
* [21 Generating HTML](#Generating-HTML)
  + [21.1 HTML Translation](#HTML-Translation)
  + [21.2 HTML Splitting](#HTML-Splitting)
  + [21.3 HTML CSS](#HTML-CSS)
  + [21.4 `@documentdescription`: Summary Text](#g_t_0040documentdescription)
  + [21.5 Generating EPUB](#Generating-EPUB)
    - [21.5.1 Container Directory and Output Files](#EPUB-Output-File-and-Directory)
    - [21.5.2 EPUB Cross-References](#EPUB-Cross_002dReferences)
    - [21.5.3 HTML Generated for EPUB](#EPUB-HTML)
  + [21.6 Code Examples Syntax Highlighting in HTML](#Syntax-Highlighting)
  + [21.7 HTML Cross-references](#HTML-Xref)
    - [21.7.1 HTML Cross-reference Link Basics](#HTML-Xref-Link-Basics)
    - [21.7.2 HTML Cross-reference Node Name Expansion](#HTML-Xref-Node-Name-Expansion)
    - [21.7.3 HTML Cross-reference Command Expansion](#HTML-Xref-Command-Expansion)
    - [21.7.4 HTML Cross-reference 8-bit Character Expansion](#HTML-Xref-8_002dbit-Character-Expansion)
    - [21.7.5 HTML Cross-reference Mismatch](#HTML-Xref-Mismatch)
    - [21.7.6 HTML Cross-reference Configuration: `htmlxref.cnf`](#HTML-Xref-Configuration)
* [Appendix A @-Command Details](#g_t_0040_002dCommand-Details)
  + [A.1 @-Command Syntax](#Command-Syntax)
  + [A.2 @-Command List](#Command-List)
  + [A.3 @-Command Contexts](#Command-Contexts)
  + [A.4 Obsolete @-Commands](#Obsolete-_0040_002dCommands)
* [Appendix B Tips and Hints](#Tips)
* [Appendix C Sample Texinfo Files](#Sample-Texinfo-Files)
  + [C.1 GNU Sample Texts](#GNU-Sample-Texts)
  + [C.2 Verbatim Copying License](#Verbatim-Copying-License)
  + [C.3 All-permissive Copying License](#All_002dpermissive-Copying-License)
* [Appendix D Using Texinfo Mode](#Texinfo-Mode)
  + [D.1 Texinfo Mode Overview](#Texinfo-Mode-Overview)
  + [D.2 The Usual GNU Emacs Editing Commands](#Emacs-Editing)
  + [D.3 Inserting Frequently Used Commands](#Inserting)
  + [D.4 Showing the Sectioning Structure of a File](#Showing-the-Structure)
    - [D.4.1 Using `texinfo-show-structure`](#Using-texinfo_002dshow_002dstructure)
    - [D.4.2 Using `occur`](#Using-occur)
  + [D.5 Updating Nodes and Menus](#Updating-Nodes-and-Menus)
    - [D.5.1 The Updating Commands](#Updating-Commands)
    - [D.5.2 Updating Requirements](#Updating-Requirements)
    - [D.5.3 Update Outer File and Include Files](#Update-Multiple-Files)
    - [D.5.4 Include Files Requirements](#Include-Files-Requirements)
    - [D.5.5 Other Updating Commands](#Other-Updating-Commands)
  + [D.6 Formatting for Info](#Info-Formatting)
    - [D.6.1 Running `texi2any`/`makeinfo` Within Emacs](#texi2any-in-Emacs)
    - [D.6.2 The `texinfo-format…` Commands](#texinfo_002dformat-commands)
  + [D.7 Formatting and Printing with Emacs](#Printing-with-Emacs)
    - [D.7.1 Formatting and Printing in Texinfo Mode](#Texinfo-Mode-Printing)
    - [D.7.2 Using the Local Variables List](#Compile_002dCommand)
  + [D.8 Texinfo Mode Summary](#Texinfo-Mode-Summary)
  + [D.9 Direct Formatting of Info files](#Formatting-Info-files)
    - [D.9.1 Tagifying a File](#Tagifying)
    - [D.9.2 Splitting a File Manually](#Splitting)
  + [D.10 Catching Mistakes](#Catching-Mistakes)
    - [D.10.1 `texi2any` Preferred](#texi2any-Preferred)
    - [D.10.2 Catching Errors with Info Formatting](#Debugging-with-Info)
    - [D.10.3 Debugging with TeX](#Debugging-with-TeX)
    - [D.10.4 Finding Badly Referenced Nodes](#Running-Info_002dvalidate)
      * [D.10.4.1 Using `Info-validate`](#Using-Info_002dvalidate)
      * [D.10.4.2 Creating an Unsplit File and Adding a Tag Table](#Unsplit-and-Tagify)
  + [D.11 Batch Formatting](#Batch-Formatting)
* [Appendix E Global Document Commands](#Global-Document-Commands)
  + [E.1 `@setchapternewpage`: Blank Pages Before Chapters](#g_t_0040setchapternewpage)
  + [E.2 Page Headings](#Headings)
    - [E.2.1 The `@headings` Command](#g_t_0040headings)
    - [E.2.2 Standard Heading Formats](#Heading-Format)
    - [E.2.3 How to Make Your Own Headings](#Custom-Headings)
  + [E.3 `@paragraphindent`: Controlling Paragraph Indentation](#g_t_0040paragraphindent)
  + [E.4 `@firstparagraphindent`: Indenting After Headings](#g_t_0040firstparagraphindent)
  + [E.5 `@exampleindent`: Environment Indenting](#g_t_0040exampleindent)
  + [E.6 `@smallbook`: Printing “Small” Books](#g_t_0040smallbook)
  + [E.7 Printing on A4 Paper](#A4-Paper)
  + [E.8 `@pagesizes` [width][, height]: Custom Page Sizes](#g_t_0040pagesizes)
  + [E.9 Microtypography](#Microtypography)
  + [E.10 Magnification](#Magnification)
* [Appendix F Info Format Specification](#Info-Format-Specification)
  + [F.1 Info Format: A Whole Manual](#Info-Format-Whole-Manual)
  + [F.2 Info Format: Preamble](#Info-Format-Preamble)
  + [F.3 Info Format: Indirect Table](#Info-Format-Indirect-Table)
  + [F.4 Info Format: Tag Table](#Info-Format-Tag-Table)
  + [F.5 Info Format: Local Variables](#Info-Format-Local-Variables)
  + [F.6 Info Format: Regular Nodes](#Info-Format-Regular-Nodes)
  + [F.7 Info Format: Menu](#Info-Format-Menu)
  + [F.8 Info Format: Image](#Info-Format-Image)
  + [F.9 Info Format: Printindex](#Info-Format-Printindex)
  + [F.10 Info Format: Cross-reference](#Info-Format-Cross-Reference)
* [Appendix G GNU Free Documentation License](#GNU-Free-Documentation-License)
* [Command and Variable Index](#Command-and-Variable-Index)
* [General Index](#General-Index)

---

Next: [Overview of Texinfo](#Overview), Previous: [Texinfo](#Top), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## Texinfo Copying Conditions [¶](#Texinfo-Copying-Conditions)

GNU Texinfo is *free software*; this means that everyone is free
to use it and free to redistribute it on certain conditions. Texinfo
is not in the public domain; it is copyrighted and there are
restrictions on its distribution, but these restrictions are designed
to permit everything that a good cooperating citizen would want to do.
What is not allowed is to try to prevent others from further sharing
any version of Texinfo that they might get from you.

Specifically, we want to make sure that you have the right to give away
copies of the programs that relate to Texinfo, that you receive source
code or else can get it if you want it, that you can change these
programs or use pieces of them in new free programs, and that you know
you can do these things.

To make sure that everyone has such rights, we have to forbid you to
deprive anyone else of these rights. For example, if you distribute
copies of the Texinfo related programs, you must give the recipients all
the rights that you have. You must make sure that they, too, receive or
can get the source code. And you must tell them their rights.

Also, for our own protection, we must make certain that everyone finds
out that there is no warranty for the programs that relate to Texinfo.
If these programs are modified by someone else and passed on, we want
their recipients to know that what they have is not what we distributed,
so that any problems introduced by others will not reflect on our
reputation.

The precise conditions of the licenses for the programs currently
being distributed that relate to Texinfo are found in the General
Public Licenses that accompany them. This manual is covered by the
GNU Free Documentation License (see [GNU Free Documentation License](#GNU-Free-Documentation-License)).

---

Next: [Writing a Texinfo File](#Writing-a-Texinfo-File), Previous: [Texinfo Copying Conditions](#Copying-Conditions), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 1 Overview of Texinfo [¶](#Overview-of-Texinfo)

*Texinfo* is a documentation system that uses a single source file
to produce both online information and printed output. This means
that instead of writing several different documents, one for each output
format, you need only write one document.

Using Texinfo, you can create a printed document (via the TeX
typesetting system) in PDF format, including chapters,
sections, cross-references, and indices. From the same Texinfo source
file, you can create HTML output suitable for use with a web
browser, you can create an Info file for use in GNU Emacs or other
Info-reading programs, and also create DocBook, EPUB 3, or LaTeX files.

A Texinfo source file is a plain text file containing text interspersed
with *@-commands* (words preceded by an ‘`@`’) that tell the
Texinfo processors what to do. Texinfo’s markup commands are almost
entirely *semantic*; that is, they specify the intended meaning
of text in the document, rather than physical formatting instructions.

GNU Emacs has a special mode, called Texinfo mode, that provides various
Texinfo-related features. (See [Using Texinfo Mode](#Texinfo-Mode).)

Texinfo was devised specifically for the purpose of writing software
documentation and manuals. If you want to write a good manual for
your program, Texinfo has many features which we hope will make your
job easier. However, Texinfo is not intended to be a general-purpose
formatting program. It provides almost no commands for controlling
the final formatting, so may be inappropriate for your needs if you
want to lay out a newspaper, devise a glossy magazine ad, or follow
the exact formatting requirements of a publishing house.

Spell “Texinfo” with a capital “T” and the other letters in
lowercase. The first syllable of “Texinfo” is pronounced like
“speck”, not “hex”. This odd pronunciation is derived from the
pronunciation of TeX. Pronounce TeX as if the ‘`X`’ were
the last sound in the name ‘Bach’. In the word TeX, the ‘`X`’
is, rather than the English letter “ex”, actually the Greek letter
“chi”.

Texinfo is the official documentation format of the GNU project.
More information, including manuals for GNU packages, is available
at the [GNU documentation web page](http://www.gnu.org/doc/).

* [Reporting Bugs](#Reporting-Bugs)
* [Output Formats](#Output-Formats)
* [Info Files](#Info-Files)
* [Printed Books](#Printed-Books)
* [Adding Output Formats](#Adding-Output-Formats)
* [History](#History)

---

Next: [Output Formats](#Output-Formats), Up: [Overview of Texinfo](#Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 1.1 Reporting Bugs [¶](#Reporting-Bugs-1)

We welcome bug reports and suggestions for any aspect of the Texinfo
system: programs, documentation, installation, etc. Please email them
to [bug-texinfo@gnu.org](mailto:bug-texinfo@gnu.org). You can get the latest version of
Texinfo via its home page, <http://www.gnu.org/software/texinfo>.

For bug reports, please include enough information for the maintainers
to reproduce the problem. Generally speaking, that means:

* The version number of Texinfo and the program(s) or manual(s) involved.
* The contents of any input files necessary to reproduce the bug.
* Precisely how you ran any program(s) involved.
* A description of the problem and samples of any erroneous output.
* Hardware and operating system names and versions.
* Anything else that you think would be helpful.

When in doubt whether something is needed or not, include it. It’s
better to include too much than to leave out something important.

It is critical to send an actual input file that reproduces the
problem.

Any problems with the Info reader in Emacs should be reported to
the Emacs developers: see [Bugs](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Bugs) in The GNU Emacs Manual.

Patches are welcome; if possible, please make them with ‘`diff -c`’,
‘`diff -u`’ (see [Comparing and Merging Files](https://www.gnu.org/software/diffutils/manual/diffutils.html#Top)),
or ‘`git diff`’ and include `ChangeLog` entries (see [Change Log](https://www.gnu.org/prep/standards/standards.html#Change-Log) in GNU Coding Standards), and follow the existing coding style.

---

Next: [Info Files](#Info-Files), Previous: [Reporting Bugs](#Reporting-Bugs), Up: [Overview of Texinfo](#Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 1.2 Output Formats [¶](#Output-Formats-1)

Here is an overview of the output formats currently supported by Texinfo.

Info [¶](#index-Info-output_002c-overview)
:   (Generated via `texi2any`.) Info format is mostly a plain
    text transliteration of the Texinfo source. It adds a few control
    characters to provide navigational information for cross-references,
    indices, and so on. The Emacs Info subsystem (see [Info](https://www.gnu.org/software/emacs/manual/html_mono/info.html#Top)), and the standalone `info` program (see [GNU Info](https://www.gnu.org/software/texinfo/manual/info-stnd/info-stnd.html#Top)), among others, can read these files. See [Info Files](#Info-Files), and [Creating and Installing Info Files](#Creating-and-Installing-Info-Files).

Plain text [¶](#index-Plain-text-output_002c-overview)
:   (Generated via `texi2any --plaintext`.) This is almost the
    same as Info output with the navigational control characters are
    omitted.

HTML [¶](#index-HTML-output_002c-overview)
:   (Generated via `texi2any --html`.) HTML, standing for Hyper
    Text Markup Language, is the language for writing documents on the World
    Wide Web. Web browsers
    can render this language online. There
    are many versions of HTML, both different standards and
    browser-specific variations. `texi2any` uses a subset
    of the language that can be interpreted by any common browser,
    intentionally not using many newer or less widely-supported tags.
    Although the native output is thus rather plain, it can be customized
    at various levels, if desired. See [Generating HTML](#Generating-HTML).

EPUB 3 [¶](#index-EPUB-3-output_002c-overview)
:   (Generated via `texi2any --epub3`.)
    EPUB is a format designed for reading electronic books on
    portable devices. It is a derivative of HTML. The format was
    developed by the International Digital Publishing Forum (IDPF),
    which is now part of the World Wide Web Consortium (W3C). The
    latest major revision, EPUB 3, dates from 2011.

DVI [¶](#index-DVI-output_002c-overview)
:   (Generated via `texi2dvi`.) The DeVIce Independent binary
    format is output by the TeX typesetting program
    (<http://tug.org>). This is then read by a DVI ‘driver’, which
    knows the actual device-specific commands that can be viewed or
    printed, notably Dvips for translation to PostScript (see [Dvips](https://tug.org/texinfohtml/dvips.html#Top)) and Xdvi for viewing on an X display
    (<http://sourceforge.net/projects/xdvi/>). See [Formatting and Printing with TeX](#Hardcopy-with-TeX). (Be aware that the Texinfo language is very different
    from TeX’s usual languages: plain TeX, LaTeX, ConTeXt,
    etc.)

PostScript [¶](#index-PostScript-output_002c-overview)
:   (Generated via `texi2dvi --ps`.) PostScript is a page
    description language that became widely used around 1985 and is still
    used today. <https://en.wikipedia.org/wiki/PostScript> gives a
    basic description and more preferences. By default, Texinfo uses the
    `dvips` program to convert TeX’s DVI output to PostScript.
    See [Dvips](https://tug.org/texinfohtml/dvips.html#Top).

PDF [¶](#index-PDF-output_002c-overview)
:   (Generated via `texi2dvi --pdf` or `texi2pdf`.) This
    format was developed by Adobe Systems for portable document
    interchange, based on their previous PostScript language. It can
    represent the exact appearance of a document, including fonts and
    graphics, and supporting arbitrary scaling. It is intended to be
    platform-independent and easily viewable, among other design goals;
    <https://en.wikipedia.org/wiki/Portable_Document_Format> and
    <http://tug.org/TUGboat/tb22-3/tb72beebe-pdf.pdf> have some
    background. By default, Texinfo uses the `pdftex` program, an
    extension of TeX, to output PDF; see
    <http://tug.org/applications/pdftex>. See [PDF Output](#PDF-Output).

LaTeX [¶](#index-LaTeX-output_002c-overview)
:   (Generated via `texi2any --latex`.) This is a typesetting
    system built on top of TeX. It was originally released by
    Leslie Lamport in 1984. LaTeX adds more definitions to those
    of TeX and has a wide range of packages built on it. LaTeX is
    ubiquitous in academic literature. The current version of LaTeX
    is under active development; more information is available online
    at <https://www.latex-project.org/>.

    The LaTeX output can be further processed into DVI, PostScript,
    or PDF. In theory, the LaTeX output should allow for much more
    customizability of the output than would be possible with the plain
    TeX implementation of Texinfo.

DocBook [¶](#index-DocBook-output_002c-overview)
:   (Generated via `texi2any --docbook`.) This is an XML-based
    format, primarily for technical
    documentation. It therefore bears some resemblance, in broad
    outline, to Texinfo. See <http://www.docbook.org>. Various
    converters from DocBook *to* Texinfo have also been developed;
    see the Texinfo web pages.

XML [¶](#index-XML-Texinfo-output_002c-overview)
:   (Generated via `texi2any --xml`.)
    The `texi2any` XML output, unlike all the other output
    formats, is a transliteration of the Texinfo source, rather than
    finished output. Texinfo XML files cannot be viewed in web browsers
    or other programs.

    XML is a generic syntax specification usable for any sort of content.
    (A reference is at <http://www.w3.org/XML>.)
    The purpose of the Texinfo XML output is to allow further processing by
    XML tools. The output syntax is defined in an XML DTD,
    which is contained in a file `texinfo.dtd` included in the
    Texinfo source distribution.

    The Texinfo source distribution includes a
    utility script `txixml2texi` to do a backward transformation
    to recreate the original Texinfo content (except for Texinfo macros
    and conditionals).

---

Next: [Printed Books](#Printed-Books), Previous: [Output Formats](#Output-Formats), Up: [Overview of Texinfo](#Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 1.3 Info Files [¶](#Info-Files-1)

As mentioned above, Info format is mostly a plain text transliteration
of the Texinfo source, with the addition of a few control characters
to separate nodes and provide navigational information, so that
Info-reading programs can operate on it.

Info files are nearly always created by processing a Texinfo source
document. `texi2any`, also known as `makeinfo`, is
the principal command that converts a Texinfo file into an Info file;
see [`texi2any`: The Translator for Texinfo](#Generic-Translator-texi2any).

Generally, you enter an Info file through a node that by convention is
named ‘Top’. This node normally contains just a brief summary of the
file’s purpose, and a large menu through which the rest of the file is
reached. From this node, you can either traverse the file
systematically by going from node to node, or you can go to a specific
node listed in the main menu, or you can search the index menus and then
go directly to the node that has the information you want. Alternatively,
with the standalone Info program, you can specify specific menu items on
the command line (see [Info](https://www.gnu.org/software/emacs/manual/html_mono/info.html#Top)).

If you want to read through an Info file in sequence, as if it were a
printed manual, you can hit `SPC` repeatedly, or you get the whole
file with the advanced Info command `g *`. (See [Advanced Info commands](https://www.gnu.org/software/emacs/manual/html_mono/info.html#Advanced) in Info.)

The `dir` file in the `info` directory serves as the
departure point for the whole Info system. From it, you can reach the
‘Top’ nodes of each of the documents in a complete Info system.

If you wish to refer to an Info file via a URI, you can use the
(unofficial) syntax exemplified by the following. This works with
Emacs/W3, for example:

```
info:emacs#Dissociated%20Press
info:///usr/info/emacs#Dissociated%20Press
info://localhost/usr/info/emacs#Dissociated%20Press
```

The `info` program itself does not follow URIs of any kind.

---

Next: [Adding Output Formats](#Adding-Output-Formats), Previous: [Info Files](#Info-Files), Up: [Overview of Texinfo](#Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 1.4 Printed Books [¶](#Printed-Books-1)

A Texinfo file can be formatted and typeset as a printed book or
manual. To do this, you need TeX, a sophisticated typesetting
program written by Donald Knuth of Stanford University. It is not part
of the Texinfo distribution.

Texinfo provides a file `texinfo.tex` that contains the definitions
that TeX uses when it typesets a Texinfo file.
You can get the latest version of `texinfo.tex` from
the Texinfo home page, <http://www.gnu.org/software/texinfo/>.

A Texinfo-based book is similar to any other typeset, printed work: it
can have a title page, copyright page, table of contents, and preface,
as well as chapters, numbered or unnumbered sections and subsections,
page headers, cross-references, footnotes, and indices.

TeX is very powerful and has a great many features. However,
because a Texinfo file must be able to present information both on a
character-only terminal in Info form and in a typeset book, the
formatting commands that Texinfo supports are necessarily limited.

See [Formatting and Printing with TeX](#Hardcopy-with-TeX), for more information on processing a
manual with TeX.

---

Next: [History](#History), Previous: [Printed Books](#Printed-Books), Up: [Overview of Texinfo](#Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 1.5 Adding Output Formats [¶](#Adding-Output-Formats-1)

The output formats in the previous sections handle a wide variety of
usage, but of course there is always room for more.

If you are a programmer and would like to contribute to the GNU
project by implementing additional output formats for Texinfo, that
would be excellent. The way to do this that would be most useful is
to write a new back-end for `texi2any`, our reference
implementation of a Texinfo parser; it creates a tree representation
of the Texinfo input that you can use for the conversion. The
documentation in the source file
`tp/Texinfo/Convert/Converter.pm` is a good place to start
(see [Texinfo::Convert::Converter](https://www.gnu.org/software/texinfo/manual/texi2any_internals/texi2any_internals.html#Texinfo_003a_003aConvert_003a_003aConverter) in Texinfo modules documentation).
See [`texi2any`: The Translator for Texinfo](#Generic-Translator-texi2any).

Another viable approach is use the Texinfo XML output from
`texi2any` as your input. This XML is an essentially complete
representation of the input, but without the Texinfo syntax and option
peculiarities, as described above.

If you still cannot resist the temptation of writing a new program
that reads Texinfo source directly, let us give some more caveats:
please do not underestimate the amount of work required. Texinfo is
by no means a simple language to parse correctly, and remains under
development, so you would be committing to an ongoing task. You
are advised to check that the tests of the language that come with
`texi2any` give correct results with your new program.

From time to time, proposals are made to generate traditional Unix man
pages from Texinfo source. However, because man pages have a strict
conventional format, creating a good man page requires a completely
different source from that needed for the typical Texinfo applications
of writing a good user tutorial and/or a good reference manual. This
makes generating man pages incompatible with the Texinfo design
goal of not having to document the same information in different
ways for different output formats. You might as well write the man
page directly.

As an alternative way to support man pages, you may find the program
`help2man` to be useful. It generates a traditional man page
from the ‘`--help`’ output of a program. In fact, the man pages
for the programs in the Texinfo distribution are generated with this.
It is GNU software written by Brendan O’Dea, available from
<http://www.gnu.org/software/help2man>.

---

Previous: [Adding Output Formats](#Adding-Output-Formats), Up: [Overview of Texinfo](#Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 1.6 History [¶](#History-1)

Richard M. Stallman invented the Texinfo format, wrote the initial
processors, and created Edition 1.0 of this manual. Robert J.
Chassell greatly revised and extended the manual, starting with
Edition 1.1. Brian Fox was responsible for the standalone Texinfo
distribution until version 3.8. Karl Berry continued maintenance
from Texinfo 3.8 (manual edition 2.22), and Gavin Smith has continued
maintenance since Texinfo 6.0.

#### Beginnings [¶](#Beginnings)

Richard Stallman included an on-line, hypertext help system called
Info in the original implementation of Emacs (in 1975/6).
Stallman had been inspired after seeing a presentation a couple of years
earlier on the “NLS” hypertext system of Douglas Engelbart.

In another development, in the 1970’s at CMU, Brian Reid developed
a program and format named Scribe to mark up documents for printing.
It used the `@` character to introduce commands, as Texinfo does.
Much more consequentially, it strove to describe document contents
rather than formatting, an idea wholeheartedly adopted by Texinfo.

Meanwhile, people at MIT developed another format called Bolio.
Richard Stallman (RMS) worked on converting Bolio to use TeX as
its typesetting language, resulting in BoTeX. The earliest BoTeX
version seems to have been 0.02 on October 31, 1984.

BoTeX could only be used as a markup language for documents to be
printed, not for online documents. RMS combined BoTeX and Info
to create Texinfo, a mark-up language for text that is intended to
be read both online and as printed hard copy.

The original translator to create Info was written
(primarily by RMS and Bob Chassell) in Emacs Lisp, namely the
`texinfo-format-buffer` and other functions. In the early 1990s,
Brian Fox reimplemented the conversion program in C, now called
`makeinfo`, as well as the standalone `info` program.

#### Reimplementing in Perl [¶](#Reimplementing-in-Perl)

In 2012, the C `makeinfo` was itself replaced by a Perl
implementation generically called `texi2any`. This version
supports the same level of output customization as
`texi2html`, an independent program originally written by
Lionel Cons, later with substantial work by many others. The many
additional features needed to make `texi2html` a replacement
for `makeinfo` were implemented by Patrice Dumas. The first,
never-released version of `texi2any` was based on the
`texi2html` code.

That implementation, however, was abandoned in favor of the current
program (also written by Patrice Dumas), which parses the Texinfo
input into a tree for processing. It inherited the design of
customization and other features from `texi2html` (for more
on `texi2html` compatibility, see [`texi2html`: Ancestor of `texi2any`](#texi2html)).
However, `texi2any` is a full reimplementation: it constructs
a tree-based representation of the input document for all back-ends
to work from.

The new Perl program is much slower than the old C program. The speed
gap has partially closed since first release, but it may not ever be
entirely comparable. So why did we switch? In short, we intend and
hope that the present program will be much easier than the previous C
implementation of `makeinfo` to extend to different output
styles, back-end output formats, and all other customizations.
In more detail:

* HTML customization. Many GNU and other free software packages
  had been happily using the HTML customization features in
  `texi2html` for years. Thus, in effect two independent
  implementations of the Texinfo language had developed, and keeping
  them in sync was not simple. Adding the HTML customization possible
  in `texi2html` to a C program would have been an
  enormous effort.
* Unicode, and multilingual support generally, especially of east
  Asian languages. At that time, doing it in C would have been tantamount
  to rewriting the entire program. Since then, the parser and bits of
  converter back-ends have been rewritten in C, but the converter
  back-ends are still mostly in Perl which has good multilingual
  support built in.
* Additional back-ends. The `makeinfo` code had become
  convoluted to the point where adding a new back-end was quite complex,
  requiring complex interactions with existing back-ends. In contrast,
  the new implementation provides a clean tree-based representation for
  all back-ends to work from. People have requested numerous different
  back-ends (LaTeX, the latest (X)HTML, …); this change made them
  much more feasible to implement. Which leads to the last item:
* Making contributions easier. In general, due to the cleaner
  structure, the separate parser/back-ends implementation
  should be considerably easier than the former C `makeinfo`
  implementation for anyone to read and contribute to, with the resulting
  obvious benefits. After ten years, contributed back-ends were yet to
  happen, but it is still believed that this structure could in theory
  lend better to contributions.

`texi2any` is intended to be a reference implementation
that defines parts of the language not fully specified by the manual.
Without such a reference, alternative implementations would be very
likely to have subtle, or not-so-subtle, differences in behavior,
and thus Texinfo documents would become dependent on the processor.
It is also important to have consistent command-line options for
all processors. Extensive tests of the language and processor were
developed at the same time as `texi2any`; we encourage anyone
thinking of writing a program to parse Texinfo input to make use of
these tests.

With the release of `texi2any` as the reference
implementation, development of both the C implementation of
`makeinfo` and `texi2html` has been halted. Going
forward, we ask authors of Texinfo documents to use only
`texi2any`.

---

Next: [Nodes](#Nodes), Previous: [Overview of Texinfo](#Overview), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 2 Writing a Texinfo File [¶](#Writing-a-Texinfo-File-1)

This chapter describes Texinfo syntax and what is required in a Texinfo
file, and gives a short sample file.

* [General Syntactic Conventions](#Conventions)
* [Comments](#Comments)
* [What a Texinfo File Must Have](#Minimum)
* [Short Sample](#Short-Sample-Texinfo-File)
* [Texinfo File Header](#Texinfo-File-Header)
* [Directory Category](#Directory-Category)
* [Document Permissions](#Document-Permissions)
* [Title and Copyright Pages](#Titlepage-_0026-Copyright-Page)
* [Generating a Table of Contents](#Contents)
* [The ‘Top’ Node and Master Menu](#The-Top-Node)
* [The Body of the Document](#The-Body-of-the-Document)
* [Ending a Texinfo File](#Ending-a-File)

---

Next: [Comments](#Comments), Up: [Writing a Texinfo File](#Writing-a-Texinfo-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 2.1 General Syntactic Conventions [¶](#General-Syntactic-Conventions)

This section describes the general conventions used in all Texinfo documents.

* All printable ASCII characters except ‘`@`’, ‘`{`’ and
  ‘`}`’ can appear in a Texinfo file and stand for themselves.
  ‘`@`’ is the escape character which introduces commands, while
  ‘`{`’ and ‘`}`’ are used to surround arguments to certain
  commands. To put one of these special characters into the document, put
  an ‘`@`’ character in front of it, like this: ‘`@@`’,
  ‘`@{`’, and ‘`@}`’.
* In a Texinfo file, the commands you write to describe the contents of
  the manual are preceded by an ‘`@`’ character; they are called
  *@-commands*. (The ‘`@`’ in Texinfo has the same meaning that
  ‘`\`’ has in plain TeX.)

  Depending on what they do or what arguments they take, you
  need to write @-commands on lines of their own, or as part of
  sentences. As a general rule, a command requires braces if it mingles
  among other text; but it does not need braces if it is on a line of its
  own. For more details of Texinfo command syntax, see [@-Command Syntax](#Command-Syntax).
* Whitespace following an @-command name is optional and (usually)
  ignored if present. The exceptions are contexts when whitespace is
  significant, e.g., an `@example` environment.
* Texinfo supports the usual quotation marks used in English and in
  other languages; see [Inserting Quotation Marks](#Inserting-Quotation-Marks).
* Use three hyphens in a row, ‘`---`’, to produce a long dash—like
  this (called an *em dash*), used for punctuation in sentences.
  Use two hyphens, ‘`--`’, to produce a medium dash (called an
  *en dash*), used primarily for numeric ranges, as in “June
  25–26”. Use a single hyphen, ‘`-`’, to produce a standard hyphen
  used in compound words. For display on the screen, Info reduces three
  hyphens to two and two hyphens to one (not transitively!). Of course,
  any number of hyphens in the source remain as they are in literal
  contexts, such as `@code` and `@example`.
* *Whitespace*. Texinfo files are plain text files composed of lines
  terminated by the usual newline character (line feed). Texinfo processors
  read input one line at a time. Paragraphs are terminated by an empty
  line or a line containing only spaces. A sequence of several spaces in
  text is usually treated the same as a single space (except in
  verbatim modes).

  Form feed (`CTRL-l`) characters in normal text end any open paragraph.
  Other ASCII whitespace (tab, carriage return) may be treated
  the same as space characters, although the results may differ depending
  on output format. Hence, there is not much point in using these
  in documents. Non-ASCII spaces, such as Unicode “em space”, are
  not recognized as whitespace at all and will be treated as regular,
  non-whitespace characters.

  However, in verbatim modes, for example in code samples, tab characters
  may produce the correct formatting in the output.

---

Next: [What a Texinfo File Must Have](#Minimum), Previous: [General Syntactic Conventions](#Conventions), Up: [Writing a Texinfo File](#Writing-a-Texinfo-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 2.2 Comments [¶](#Comments-1)

You can write comments in a Texinfo file by using the `@comment`
command, which may be abbreviated to `@c`. Such comments are
for a person looking at the Texinfo source file. All the text on a
line that follows either `@comment` or `@c` is a comment;
the rest of the line does not appear in the visible output. (To be
precise, the character after the `@c` or `@comment` must
be something other than a dash or alphanumeric, or it will be taken as
part of the command.)

Often, you can write the `@comment` or `@c` in the middle
of a line, and only the text that follows after the `@comment`
or `@c` command does not appear; but some commands, such as
`@settitle`, work on a whole line. You cannot use `@comment`
or `@c` within a line beginning with such a command.

In cases of nested command invocations, complicated macro definitions,
etc., `@c` and `@comment` may provoke an error when
processing with TeX. Therefore, you can also use the `DEL`
character (ASCII 127 decimal, 0x7f hex, 0177 octal) as a true TeX
comment character (catcode 14, in TeX internals). Everything on
the line after the `DEL` will be ignored and the next line will
be merged.

You can also have long stretches of text ignored by the Texinfo
processors with the `@ignore` and `@end ignore` commands.
Write each of these commands on a line of its own, starting each
command at the beginning of the line. Text between these two commands
does not appear in the processed output. You can use `@ignore`
and `@end ignore` for writing comments. (For some caveats
regarding nesting of such commands, see [Conditional Nesting](#Conditional-Nesting).)

---

Next: [Short Sample](#Short-Sample-Texinfo-File), Previous: [Comments](#Comments), Up: [Writing a Texinfo File](#Writing-a-Texinfo-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 2.3 What a Texinfo File Must Have [¶](#What-a-Texinfo-File-Must-Have)

By convention, the name of a Texinfo file ends with one of the
extensions `.texi`, `.texinfo`, `.txi`, or
`.tex`. Using `.tex` is discouraged as this extension
is already used by TeX and LaTeX input files. The most common
and recommended extension is `.texi`. The name of a Texinfo
file should only contain ASCII characters.

The output name is based on the input file name, in the default case.
First, any of the extensions `.texi`, `.tex`, `.txi`,
or `.texinfo` is removed from the input file name; then,
the output format specific extension is added—`.html` when
generating HTML, `.info` when generating Info, etc.
The output name should only contain ASCII
characters[1](#FOOT1).

In order to be made into a printed manual, a Texinfo file must begin
with a line like this:

```
\input texinfo
```

The contents of the file follow this beginning, and then you
must end the Texinfo source with a line like this:

```
@bye
```

The `@bye` line at the end of the file on a line of its own tells
TeX that the file is ended and to stop formatting. If you
leave this out, you’ll be dumped at TeX’s prompt at the end of the
run.

Furthermore, you will usually provide a Texinfo file with a title,
a title page, indices, and the like, all of which are explained in
this manual. But the minimum, which can be useful for short
documents, is just the one line at the beginning and the one line
at the end.

Without additional information, the input and output encodings are
assumed to be UTF-8, an universal codeset compatible with 7-bit
ASCII.

---

Next: [Texinfo File Header](#Texinfo-File-Header), Previous: [What a Texinfo File Must Have](#Minimum), Up: [Writing a Texinfo File](#Writing-a-Texinfo-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 2.4 Short Sample [¶](#Short-Sample-1)

Here is a short sample Texinfo file.

```
\input texinfo
@settitle Sample Manual 1.0

@copying
This is a short example of a complete Texinfo file.

Copyright @copyright{} 2016 Free Software Foundation, Inc.
@end copying

@titlepage
@title Sample Title
@page
@vskip 0pt plus 1filll
@insertcopying
@end titlepage

@contents

@node Top
@top GNU Sample

This manual is for GNU Sample
(version @value{VERSION}, @value{UPDATED}).

@menu
* First Chapter::    The first chapter is the
                      only chapter in this sample.
* Index::            Complete index.
@end menu

@node First Chapter
@chapter First Chapter

@cindex chapter, first
This is the first chapter.
@cindex index entry, another
```

```
Here is a numbered list.

@enumerate
@item
This is the first item.

@item
This is the second item.
@end enumerate
```

```
@node First Section
@section First Section

First section of first chapter.

@node Second Section
@section Second Section

Second section of first chapter.

@node Index
@unnumbered Index

@printindex cp

@bye
```

---

Next: [Directory Category](#Directory-Category), Previous: [Short Sample](#Short-Sample-Texinfo-File), Up: [Writing a Texinfo File](#Writing-a-Texinfo-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 2.5 Texinfo File Header [¶](#Texinfo-File-Header-1)

Texinfo files start with the line:

```
\input texinfo
```

The ‘`\input texinfo`’ line tells TeX to use the
`texinfo.tex` file, which tells TeX how to translate the Texinfo
@-commands into TeX typesetting commands. (Note the use of the
backslash, ‘`\`’; this is correct for TeX.)

It makes sense to include any command that affects document formatting
as a whole in the header. The `@settitle` line is usually
present at the beginning of the header:

```
@settitle Sample Manual 1.0
```

The `@settitle` line specifies a title for the page headers
(or footers) of the printed manual, and the default title and document
description for the ‘`<head>`’ in HTML. `@synindex`
(see [`@synindex`: Combining Indices](#g_t_0040synindex)), for instance, is another command often
included in the header.

The start of the Texinfo file up to the first content that is output
as part of the main body of the document is the *preamble*.
It includes the header, [Document Permissions](#Document-Permissions) and [Title and Copyright Pages](#Titlepage-_0026-Copyright-Page)
specification. It is important for the LaTeX output format as the
end of preamble is where the `\begin{document}` line is output. In
other output formats it may be used to determine how some special output is
formatted, for example [`@copying`: Declare Copying Permissions](#g_t_0040copying) output as a comment
at the beginning of output files, or the language used in file headers.

* [The First Line of a Texinfo File](#First-Line)
* [`@setfilename`: Set the Output File Name](#g_t_0040setfilename)
* [`@settitle`: Set the Document Title](#g_t_0040settitle)
* [Preamble](#Preamble)
* [Start and End of Header for Emacs](#Start-and-End-of-Header)

---

Next: [`@setfilename`: Set the Output File Name](#g_t_0040setfilename), Up: [Texinfo File Header](#Texinfo-File-Header)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 2.5.1 The First Line of a Texinfo File [¶](#The-First-Line-of-a-Texinfo-File)

Every Texinfo file that is to be the top-level input to TeX must begin
with a line that looks like this:

```
\input texinfo
```

When the file is processed by TeX, the ‘`\input texinfo`’ command
tells TeX to load the macros needed for processing a Texinfo file.
These are in a file called `texinfo.tex`, which should have been
installed on your system along with either the TeX or Texinfo
software. TeX uses the backslash, ‘`\`’, to mark the beginning of
a command, exactly as Texinfo uses ‘`@`’. The `texinfo.tex`
file causes the switch from ‘`\`’ to ‘`@`’; before the switch
occurs, TeX requires ‘`\`’, which is why it appears at the
beginning of the file.

You may optionally follow this line with a comment to tell GNU Emacs
to use Texinfo mode when the file is edited:

```
\input texinfo   @c -*-texinfo-*-
```

This may be useful when Emacs doesn’t detect the file type
from the file extension automatically.

---

Next: [`@settitle`: Set the Document Title](#g_t_0040settitle), Previous: [The First Line of a Texinfo File](#First-Line), Up: [Texinfo File Header](#Texinfo-File-Header)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 2.5.2 `@setfilename`: Set the Output File Name [¶](#g_t_0040setfilename_003a-Set-the-Output-File-Name)

The `@setfilename` line specifies the name of the output file to
be generated by `texi2any`. This command is ignored for
TeX formatting. When present, it should be the first Texinfo
command (that is, after ‘`\input texinfo`’). Write the
`@setfilename` command at the beginning of a line and follow it
on the same line by the Info file name.

```
@setfilename info-file-name
```

The name must be different from the name of the Texinfo file.
There are two conventions for choosing the name: you
can either remove the extension (such as ‘`.texi`’) entirely from
the input file name, or (recommended) replace it with the ‘`.info`’
extension. It is not advised to base the `@setfilename` name
on a entirely different name than the input file name.

When a `@setfilename` line is present, the Texinfo processors
ignore everything written before the `@setfilename` line. This
is why the very first line of the file (the `\input` line) does
not show up in the output.

If there is no `@setfilename` line, `texi2any` uses the
input file name to determine the output name (see [What a Texinfo File Must Have](#Minimum)). The
`\input` line is still ignored in this processing, as well
as leading blank lines.

When producing another output format, `texi2any` will replace any
final extension with the output format-specific extension (‘`html`’
when generating HTML, for example), or add a dot followed by the
extension (‘`.html`’ for HTML) if the given name has no extension.

@-commands are not allowed in `@setfilename`, except for
`@@`, `@{`, `@}` and associated @-commands
such as `@atchar{}`.

`@setfilename` used to be required by the Texinfo processors
and some other programs. This should not be the case any more;
`@setfilename` can be omitted. If the Texinfo input is
processed from standard input, without an input file name to deduce the
base file name from, `@setfilename` could still be relevant.
This is not the only way, however: `--output` option specifies
the output file name on the `texi2any` command-line
(see [Invoking `texi2any` from a Shell](#Invoking-texi2any)).

Although an explicit ‘`.info`’ extension is preferable, some
operating systems cannot handle long file names. You can run into a
problem even when the file name you specify is itself short enough.
This occurs because the Info formatters split a long Info file into
short indirect subfiles, and name them by appending ‘`-1`’,
‘`-2`’, …, ‘`-10`’, ‘`-11`’, and so on, to the original
file name. (See [Tag Files and Split Files](#Tag-and-Split-Files).) The subfile name
`texinfo.info-10`, for example, is too long for old systems with
a 14-character limit on filenames; so the Info file name for this
document could be `texinfo` rather than `texinfo.info` on
such a system. `@setfilename` is a way to specify an
alternative name.

---

Next: [Preamble](#Preamble), Previous: [`@setfilename`: Set the Output File Name](#g_t_0040setfilename), Up: [Texinfo File Header](#Texinfo-File-Header)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 2.5.3 `@settitle`: Set the Document Title [¶](#g_t_0040settitle_003a-Set-the-Document-Title)

A Texinfo file should contain a line that looks like this:

```
@settitle title
```

Write the `@settitle` command at the beginning of a line and
follow it on the same line by the title. Do not write anything else
on the line. The `@settitle` command should precede everything
that generates actual output. The best place for it is right after
the `@setfilename` command, if present (described in the previous
section).

This command gives the title to use in a header or footer
for double-sided printed output, in case such headings are output. For
more on headings for printed output, see [Heading Generation](#Heading-Generation).

In HTML, title serves as the document ‘`<title>`’ and it becomes
the default document description in the ‘`<head>`’ part.

When the title page is used in the output, as is generally the case for printed
output, the title in the `@settitle` command does not affect the title as
it appears on the title page. Thus, the two do not need not to match exactly.
A practice we recommend is to include the version or edition number of the
manual in the `@settitle` title; on the title page, the version number
generally appears as a `@subtitle` so it would be omitted from the
`@title`. See [`@titlepage`](#g_t_0040titlepage).

---

Next: [Start and End of Header for Emacs](#Start-and-End-of-Header), Previous: [`@settitle`: Set the Document Title](#g_t_0040settitle), Up: [Texinfo File Header](#Texinfo-File-Header)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 2.5.4 Preamble [¶](#Preamble-1)

The *preamble* starts at the beginning of the Texinfo file and continues
until the first directly output material. It typically includes the
file header (see [Texinfo File Header](#Texinfo-File-Header)), the `@copying` block
specifying the document permissions (see [`@copying`: Declare Copying Permissions](#g_t_0040copying)) and the
`@titlepage` specification (see [Title and Copyright Pages](#Titlepage-_0026-Copyright-Page)).

The preamble may contain commands that affect document formatting
as a whole but which do not produce output, or do not produce output
straight away, such as `@settitle` (see [`@settitle`: Set the Document Title](#g_t_0040settitle)),
`@documentlanguage`, (see [`@documentlanguage ll[_cc]`: Set the Document Language](#g_t_0040documentlanguage)),
commands setting the headings, controlling indentation or hyphenation,
or the table of contents (see [Generating a Table of Contents](#Contents)).

Any text that starts a paragraph, @-commands that are formatted
as quotations, tables, lists and so on, and `@node` (see [Nodes](#Nodes))
and chapter structuring commands (see [Chapter Structuring](#Chapter-Structuring)) end the
preamble.

The concept of the preamble is significant for LaTeX output, as the
`\begin{document}` line is output at the end of the preamble.

In plaintext, the preamble is simply output as usual at the beginning of the
document; for example, a `@contents` in the preamble is output as
the table of contents (see [Generating a Table of Contents](#Contents)).

There is not much special treatment of the preamble for HTML and Info
output either. However, some settings current at the very end of
the preamble may be used for the document as a whole, regardless of
what follows. This may apply to commands specifying the indentation,
or the language (see [`@documentlanguage ll[_cc]`: Set the Document Language](#g_t_0040documentlanguage)).

For example, for the following document, the HTML and Info copying
comments are formatted with `@documentlanguage` set to ‘`pt`’,
as it is the last `@documentlanguage` before the end of the
preamble.

```
\input texinfo
@documentlanguage fr

@copying
The copying information @error{} some text
@end copying

@documentlanguage pt

Text ending the preamble

@documentlanguage de

@node Top
```

---

Previous: [Preamble](#Preamble), Up: [Texinfo File Header](#Texinfo-File-Header)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 2.5.5 Start and End of Header for Emacs [¶](#Start-and-End-of-Header-for-Emacs)

In Emacs, start- and end-of-header lines can be used to enclose commands that
globally affect the document in the Texinfo preambule. This allows you to
format only part of a Texinfo file for Info or printing.
See [The `texinfo-format…` Commands](#texinfo_002dformat-commands).

A start-of-header line is a Texinfo comment that looks like this:

```
@c %**start of header
```

Write the start-of-header line on the second line of a Texinfo file.
Follow the start-of-header commands that globally affect the document
formatting, such as `@settitle`, `@synindex` or
`@footnotestyle`; and then by an end-of-header line.

A end-of-header line is a Texinfo comment that looks like this:

```
@c %**end of header
```

The odd string of characters, ‘`%**`’, is to ensure that no other
comment is accidentally taken for a start-of-header line. You can
change it if you wish by setting the `tex-start-of-header` and/or
`tex-end-of-header` Emacs variables. See [Formatting and Printing in Texinfo Mode](#Texinfo-Mode-Printing).

The start- and end-of-header lines are not part of the Texinfo format
specification, which is why they are implemented with comments.

---

Next: [Document Permissions](#Document-Permissions), Previous: [Texinfo File Header](#Texinfo-File-Header), Up: [Writing a Texinfo File](#Writing-a-Texinfo-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 2.6 Directory Category [¶](#Directory-Category-1)

Use the `@dircategory` command to specify a category
for the manual. Here are a few examples of category names:

```
Basics
Text creation and manipulation
Archiving
Compression
Database
Editors
Emacs
Email
Graphics
Localization
Network applications
Printing
Science
Software development
Software libraries
Version control
```

`@dircategory` commands are usually followed by a
`@direntry` blocks, which are used by `install-info`.
See [Installing Info Directory Files](#Installing-Dir-Entries), for details.

The first `@dircategory` command in a manual is the category for
the entire manual. Subsequent uses of `@dircategory` set the category
for following `@direntry` blocks only.

---

Next: [Title and Copyright Pages](#Titlepage-_0026-Copyright-Page), Previous: [Directory Category](#Directory-Category), Up: [Writing a Texinfo File](#Writing-a-Texinfo-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 2.7 Document Permissions [¶](#Document-Permissions-1)

This segment describes the document and contains the copyright notice
and copying permissions. This is done with the `@copying` command.
A real manual includes more text here, according to the license under
which it is distributed.

```
@copying
This is a short example of a complete Texinfo file, version 1.0.

Copyright @copyright{} 2016 Free Software Foundation, Inc.
@end copying
```

The copyright notice and copying permissions for a document need to
appear in several places in the various Texinfo output formats.
Therefore, Texinfo provides a command (`@copying`) to declare
this text once, and another command (`@insertcopying`) to
insert the text at appropriate points.

If the document is a software manual, the software is typically under a
different license—for GNU and many other free software packages,
software is usually released under the GNU GPL, and manuals are
released under the GNU FDL. It is helpful to state the license of
the software of the manual, but giving the complete text of the
software license is not necessarily required.

* [`@copying`: Declare Copying Permissions](#g_t_0040copying)
* [`@insertcopying`: Include Permissions Text](#g_t_0040insertcopying)

---

Next: [`@insertcopying`: Include Permissions Text](#g_t_0040insertcopying), Up: [Document Permissions](#Document-Permissions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 2.7.1 `@copying`: Declare Copying Permissions [¶](#g_t_0040copying_003a-Declare-Copying-Permissions)

The `@copying` command should be given very early in the document;
the recommended location is right after the header material
(see [Texinfo File Header](#Texinfo-File-Header)). It conventionally consists of a sentence
or two about what the program is, identification of the documentation
itself, the legal copyright line, and the copying permissions. Here is
a skeletal example:

```
@copying
This manual is for program (version version, updated
date), which ...

Copyright @copyright{} years copyright-owner.

@quotation
Permission is granted to ...
@end quotation
@end copying
```

The `@quotation` has no legal significance; it’s there to improve
readability in some contexts.

The text of `@copying` appears as a comment at the beginning of Info
and HTML output files. This information is also output at the beginning
of the DocBook output files using appropriate markup. It is *not*
output implicitly in plain text or printed output; it’s up to you to use
`@insertcopying` to emit the copying information. See the next section
for details.

The `@copyright{}` command generates a ‘`c`’ inside a
circle when the output format supports this glyph (print and HTML
always do, for instance). When the glyph is not supported in the
output, it generates the three-character sequence ‘`(C)`’.

The copyright notice itself has the following legally-prescribed
form:

```
Copyright © years copyright-owner.
```

The word ‘Copyright’ must always be written in English, even if the
document is otherwise written in another language. This is due to
international law.

The list of years should include all years in which a version was
completed (even if it was released in a subsequent year). It is
simplest for each year to be written out individually and in full,
separated by commas.

The copyright owner (or owners) is whoever holds legal copyright on the
work. In the case of works assigned to the FSF, the owner is ‘Free
Software Foundation, Inc.’.

The copyright ‘line’ may actually be split across multiple lines, both
in the source document and in the output. This often happens for
documents with a long history, having many different years of
publication. If you do use several lines, do not indent any of them
(or anything else in the `@copying` block) in the source file.

See [Copyright Notices](https://www.gnu.org/prep/maintain/maintain.html#Copyright-Notices) in GNU Maintainer Information, for
additional information. See [GNU Sample Texts](#GNU-Sample-Texts), for the full text to
be used in GNU manuals. See [GNU Free Documentation License](#GNU-Free-Documentation-License), for
the license itself under which GNU and other free manuals are
distributed.

---

Previous: [`@copying`: Declare Copying Permissions](#g_t_0040copying), Up: [Document Permissions](#Document-Permissions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 2.7.2 `@insertcopying`: Include Permissions Text [¶](#g_t_0040insertcopying_003a-Include-Permissions-Text)

The `@insertcopying` command is simply written on a line by
itself, like this:

```
@insertcopying
```

This inserts the text previously defined by `@copying`. To meet
legal requirements, it must be used on the copyright page in the printed
manual (see [Copyright Page](#Copyright)).

The `@copying` command itself causes the permissions text to
appear in an Info file *before* the first node. The text is also
copied into the beginning of each split Info output file, as is legally
necessary. This location implies a human reading the manual using Info
does *not* see this text (except when using the advanced Info
command `g *`). This does not matter for legal purposes,
because the text is present. But to get a visible text in the output,
`@insertcopying` should be used.

Similarly, the `@copying` text is automatically included at the
beginning of each HTML output file, as an HTML comment. Again, this
text is not visible without `@insertcopying` (unless the reader views the
HTML source).

The permissions text defined by `@copying` also appears
automatically at the beginning of the DocBook output files
using appropriate markup. `@insertcopying` can be used
to output the permission text within normal text.

---

Next: [Generating a Table of Contents](#Contents), Previous: [Document Permissions](#Document-Permissions), Up: [Writing a Texinfo File](#Writing-a-Texinfo-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]
