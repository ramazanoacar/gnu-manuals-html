Next: [Introduction](#Introduction), Up: [(dir)](/manual)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]# GNU `gettext` utilities [¶](#GNU-gettext-utilities)

This manual documents the GNU gettext tools and the GNU libintl library,
version 1.0.## Table of Contents

* [1 Introduction](#Introduction)
  + [1.1 The Purpose of GNU `gettext`](#Why)
  + [1.2 I18n, L10n, and Such](#Concepts)
  + [1.3 Aspects in Native Language Support](#Aspects)
  + [1.4 Files Conveying Translations](#Files)
  + [1.5 Overview of GNU `gettext`](#Overview)
* [2 The User’s View](#Users)
  + [2.1 Operating System Installation](#System-Installation)
  + [2.2 Setting the Locale Used by GUI Programs](#Setting-the-GUI-Locale)
  + [2.3 Setting the Locale through Environment Variables](#Setting-the-POSIX-Locale)
    - [2.3.1 Locale Names](#Locale-Names)
    - [2.3.2 Locale Environment Variables](#Locale-Environment-Variables)
    - [2.3.3 Specifying a Priority List of Languages](#The-LANGUAGE-variable)
  + [2.4 Obtaining good output in a Windows console](#Working-in-a-Windows-console)
  + [2.5 Installing Translations for Particular Programs](#Installing-Localizations)
* [3 The Format of PO Files](#PO-Files)
  + [3.1 What an entry looks like](#PO-File-Entries)
  + [3.2 Workflow flags](#Workflow-flags)
  + [3.3 Sticky flags](#Sticky-flags)
  + [3.4 Entries with Context](#Entries-with-Context)
  + [3.5 Entries with Plural Forms](#Entries-with-Plural-Forms)
  + [3.6 Further details on the PO file format](#More-Details)
  + [3.7 Evolution of the PO File Format](#PO-File-Format-Evolution)
* [4 Preparing Program Sources](#Sources)
  + [4.1 Importing the `gettext` declaration](#Importing)
  + [4.2 Triggering `gettext` Operations](#Triggering)
  + [4.3 Preparing Translatable Strings](#Preparing-Strings)
    - [4.3.1 Decent English style](#Decent-English-style)
    - [4.3.2 Entire sentences](#Entire-sentences)
    - [4.3.3 Split at paragraphs](#Split-at-paragraphs)
    - [4.3.4 No string concatenation](#No-string-concatenation)
    - [4.3.5 No embedded URLs](#No-embedded-URLs)
    - [4.3.6 No programmer-defined format string directives](#No-custom-format-directives)
    - [4.3.7 No unusual markup](#No-unusual-markup)
  + [4.4 How Marks Appear in Sources](#Mark-Keywords)
  + [4.5 Marking Translatable Strings](#Marking)
  + [4.6 Adding advice for translators](#Translator-advice)
  + [4.7 Special Comments preceding Keywords](#c_002dformat-Flag)
  + [4.8 Special Cases of Translatable Strings](#Special-cases)
  + [4.9 Letting Users Report Translation Bugs](#Bug-Report-Address)
  + [4.10 Marking Proper Names for Translation](#Names)
  + [4.11 Preparing Library Sources](#Libraries)
* [5 Making the PO Template File](#Template)
  + [5.1 Invoking the `xgettext` Program](#xgettext-Invocation)
    - [5.1.1 Input file location](#Input-file-location)
    - [5.1.2 Output file location](#Output-file-location)
    - [5.1.3 Choice of input file language](#Choice-of-input-file-language)
    - [5.1.4 Input file interpretation](#Input-file-interpretation)
    - [5.1.5 Operation mode](#Operation-mode)
    - [5.1.6 Language specific options](#Language-specific-options)
    - [5.1.7 Options for XML input files](#Options-for-XML-input-files)
    - [5.1.8 Output details](#Output-details)
    - [5.1.9 Informative output](#Informative-output)
    - [5.1.10 Example](#Example)
  + [5.2 Combining PO Template Files](#Combining-POTs)
* [6 Creating or preparing for translating a PO file](#Creating)
  + [6.1 Invoking the `msginit` Program](#msginit-Invocation)
    - [6.1.1 Input file location](#Input-file-location-1)
    - [6.1.2 Output file location](#Output-file-location-1)
    - [6.1.3 Input file syntax](#Input-file-syntax)
    - [6.1.4 Output details](#Output-details-1)
    - [6.1.5 Informative output](#Informative-output-1)
  + [6.2 Filling in the Header Entry](#Header-Entry)
* [7 Updating Existing PO Files](#Updating)
  + [7.1 Invoking the `msgmerge` Program](#msgmerge-Invocation)
    - [7.1.1 Input file location](#Input-file-location-2)
    - [7.1.2 Operation mode](#Operation-mode-1)
    - [7.1.3 Output file location](#Output-file-location-2)
    - [7.1.4 Output file location in update mode](#Output-file-location-in-update-mode)
    - [7.1.5 Operation modifiers](#Operation-modifiers)
    - [7.1.6 Input file syntax](#Input-file-syntax-1)
    - [7.1.7 Output details](#Output-details-2)
    - [7.1.8 Informative output](#Informative-output-2)
* [8 Pretranslating PO Files](#Pretranslating)
  + [8.1 Installing a Large Language Model](#Installing-an-LLM)
  + [8.2 Invoking the `msgpre` Program](#msgpre-Invocation)
    - [8.2.1 Input file location](#Input-file-location-3)
    - [8.2.2 Output file location](#Output-file-location-3)
    - [8.2.3 Message selection](#Message-selection)
    - [8.2.4 Large Language Model (LLM) options](#Large-Language-Model-_0028LLM_0029-options)
    - [8.2.5 Input file syntax](#Input-file-syntax-2)
    - [8.2.6 Output details](#Output-details-3)
    - [8.2.7 Informative output](#Informative-output-3)
    - [8.2.8 Examples](#Examples)
  + [8.3 Invoking the `spit` Program](#spit-Invocation)
    - [8.3.1 Large Language Model (LLM) options](#Large-Language-Model-_0028LLM_0029-options-1)
    - [8.3.2 Informative output](#Informative-output-4)
    - [8.3.3 Examples](#Examples-1)
* [9 Editing PO Files](#Editing)
  + [9.1 Web-based PO editing](#Web-based-localization)
  + [9.2 KDE’s PO File Editor](#Lokalize)
  + [9.3 GNOME’s PO File Editor](#Gtranslator)
  + [9.4 Poedit](#Poedit)
  + [9.5 OmegaT](#OmegaT)
  + [9.6 The Virtaal Translation Editor](#Virtaal)
  + [9.7 Emacs’s PO File Editor](#PO-Mode)
    - [9.7.1 Completing GNU `gettext` Installation](#Installation)
    - [9.7.2 Main PO mode Commands](#Main-PO-Commands)
    - [9.7.3 Entry Positioning](#Entry-Positioning)
    - [9.7.4 Normalizing Strings in Entries](#Normalizing)
    - [9.7.5 Translated Entries](#Translated-Entries)
    - [9.7.6 Fuzzy Entries](#Fuzzy-Entries)
    - [9.7.7 Untranslated Entries](#Untranslated-Entries)
    - [9.7.8 Obsolete Entries](#Obsolete-Entries)
    - [9.7.9 Modifying Translations](#Modifying-Translations)
    - [9.7.10 Modifying Comments](#Modifying-Comments)
    - [9.7.11 Details of Sub Edition](#Subedit)
    - [9.7.12 C Sources Context](#C-Sources-Context)
    - [9.7.13 Consulting Auxiliary PO Files](#Auxiliary)
  + [9.8 Editing PO Files in vim](#Vim)
  + [9.9 Using Translation Compendia](#Compendium)
    - [9.9.1 Creating Compendia](#Creating-Compendia)
      * [9.9.1.1 Concatenate PO Files](#Concatenate-PO-Files)
      * [9.9.1.2 Extract a Message Subset from a PO File](#Extract-a-Message-Subset-from-a-PO-File)
    - [9.9.2 Using Compendia](#Using-Compendia)
      * [9.9.2.1 Initialize a New Translation File](#Initialize-a-New-Translation-File)
      * [9.9.2.2 Update an Existing Translation File](#Update-an-Existing-Translation-File)
* [10 Manipulating PO Files](#Manipulating)
  + [10.1 Invoking the `msgcat` Program](#msgcat-Invocation)
    - [10.1.1 Input file location](#Input-file-location-4)
    - [10.1.2 Output file location](#Output-file-location-4)
    - [10.1.3 Message selection](#Message-selection-1)
    - [10.1.4 Input file syntax](#Input-file-syntax-3)
    - [10.1.5 Output details](#Output-details-4)
    - [10.1.6 Informative output](#Informative-output-5)
  + [10.2 Invoking the `msgconv` Program](#msgconv-Invocation)
    - [10.2.1 Input file location](#Input-file-location-5)
    - [10.2.2 Output file location](#Output-file-location-5)
    - [10.2.3 Conversion target](#Conversion-target)
    - [10.2.4 Input file syntax](#Input-file-syntax-4)
    - [10.2.5 Output details](#Output-details-5)
    - [10.2.6 Informative output](#Informative-output-6)
  + [10.3 Invoking the `msggrep` Program](#msggrep-Invocation)
    - [10.3.1 Input file location](#Input-file-location-6)
    - [10.3.2 Output file location](#Output-file-location-6)
    - [10.3.3 Message selection](#Message-selection-2)
    - [10.3.4 Input file syntax](#Input-file-syntax-5)
    - [10.3.5 Output details](#Output-details-6)
    - [10.3.6 Informative output](#Informative-output-7)
    - [10.3.7 Examples](#Examples-2)
  + [10.4 Invoking the `msgfilter` Program](#msgfilter-Invocation)
    - [10.4.1 Input file location](#Input-file-location-7)
    - [10.4.2 Output file location](#Output-file-location-7)
    - [10.4.3 The filter](#The-filter)
    - [10.4.4 Useful filter-options when the filter is ‘`sed`’](#Useful-filter_002doptions-when-the-filter-is-sed)
    - [10.4.5 Built-in filters](#Built_002din-filters)
    - [10.4.6 Input file syntax](#Input-file-syntax-6)
    - [10.4.7 Output details](#Output-details-7)
    - [10.4.8 Informative output](#Informative-output-8)
    - [10.4.9 Examples](#Examples-3)
  + [10.5 Invoking the `msguniq` Program](#msguniq-Invocation)
    - [10.5.1 Input file location](#Input-file-location-8)
    - [10.5.2 Output file location](#Output-file-location-8)
    - [10.5.3 Message selection](#Message-selection-3)
    - [10.5.4 Input file syntax](#Input-file-syntax-7)
    - [10.5.5 Output details](#Output-details-8)
    - [10.5.6 Informative output](#Informative-output-9)
  + [10.6 Invoking the `msgcomm` Program](#msgcomm-Invocation)
    - [10.6.1 Input file location](#Input-file-location-9)
    - [10.6.2 Output file location](#Output-file-location-9)
    - [10.6.3 Message selection](#Message-selection-4)
    - [10.6.4 Input file syntax](#Input-file-syntax-8)
    - [10.6.5 Output details](#Output-details-9)
    - [10.6.6 Informative output](#Informative-output-10)
  + [10.7 Invoking the `msgcmp` Program](#msgcmp-Invocation)
    - [10.7.1 Input file location](#Input-file-location-10)
    - [10.7.2 Operation modifiers](#Operation-modifiers-1)
    - [10.7.3 Input file syntax](#Input-file-syntax-9)
    - [10.7.4 Informative output](#Informative-output-11)
  + [10.8 Invoking the `msgattrib` Program](#msgattrib-Invocation)
    - [10.8.1 Input file location](#Input-file-location-11)
    - [10.8.2 Output file location](#Output-file-location-10)
    - [10.8.3 Message selection](#Message-selection-5)
    - [10.8.4 Attribute manipulation](#Attribute-manipulation)
    - [10.8.5 Input file syntax](#Input-file-syntax-10)
    - [10.8.6 Output details](#Output-details-10)
    - [10.8.7 Informative output](#Informative-output-12)
  + [10.9 Invoking the `msgen` Program](#msgen-Invocation)
    - [10.9.1 Input file location](#Input-file-location-12)
    - [10.9.2 Output file location](#Output-file-location-11)
    - [10.9.3 Input file syntax](#Input-file-syntax-11)
    - [10.9.4 Output details](#Output-details-11)
    - [10.9.5 Informative output](#Informative-output-13)
  + [10.10 Invoking the `msgexec` Program](#msgexec-Invocation)
    - [10.10.1 Input file location](#Input-file-location-13)
    - [10.10.2 Input file syntax](#Input-file-syntax-12)
    - [10.10.3 Informative output](#Informative-output-14)
  + [10.11 Highlighting parts of PO files](#Colorizing)
    - [10.11.1 The `--color` option](#The-_002d_002dcolor-option)
    - [10.11.2 The environment variable `TERM`](#The-TERM-variable)
    - [10.11.3 The `--style` option](#The-_002d_002dstyle-option)
    - [10.11.4 Style rules for PO files](#Style-rules)
    - [10.11.5 Customizing `less` for viewing PO files](#Customizing-less)
  + [10.12 Other tools for manipulating PO files](#Other-tools)
  + [10.13 Writing your own programs that process PO files](#libgettextpo)
    - [10.13.1 Error Handling](#Error-Handling)
    - [10.13.2 po\_file\_t API](#po_005ffile_005ft-API)
    - [10.13.3 po\_message\_iterator\_t API](#po_005fmessage_005fiterator_005ft-API)
    - [10.13.4 po\_message\_t API](#po_005fmessage_005ft-API)
    - [10.13.5 PO Header Entry API](#PO-Header-Entry-API)
    - [10.13.6 po\_filepos\_t API](#po_005ffilepos_005ft-API)
    - [10.13.7 Format Type API](#Format-Type-API)
    - [10.13.8 po\_flag\_iterator\_t API](#po_005fflag_005fiterator_005ft-API)
    - [10.13.9 Checking API](#Checking-API)
* [11 Producing Binary MO Files](#Binaries)
  + [11.1 Invoking the `msgfmt` Program](#msgfmt-Invocation)
    - [11.1.1 Input file location](#Input-file-location-14)
    - [11.1.2 Operation mode](#Operation-mode-2)
    - [11.1.3 Output file location](#Output-file-location-12)
    - [11.1.4 Output file location in Java mode](#Output-file-location-in-Java-mode)
    - [11.1.5 Output file location in C# mode](#Output-file-location-in-C_0023-mode)
    - [11.1.6 Output file location in Tcl mode](#Output-file-location-in-Tcl-mode)
    - [11.1.7 Desktop Entry mode options](#Desktop-Entry-mode-options)
    - [11.1.8 XML mode options](#XML-mode-options)
    - [11.1.9 Input file syntax](#Input-file-syntax-13)
    - [11.1.10 Input file interpretation](#Input-file-interpretation-1)
    - [11.1.11 Output details](#Output-details-12)
    - [11.1.12 Informative output](#Informative-output-15)
  + [11.2 Invoking the `msgunfmt` Program](#msgunfmt-Invocation)
    - [11.2.1 Operation mode](#Operation-mode-3)
    - [11.2.2 Input file location](#Input-file-location-15)
    - [11.2.3 Input file location in Java mode](#Input-file-location-in-Java-mode)
    - [11.2.4 Input file location in C# mode](#Input-file-location-in-C_0023-mode)
    - [11.2.5 Input file location in Tcl mode](#Input-file-location-in-Tcl-mode)
    - [11.2.6 Output file location](#Output-file-location-13)
    - [11.2.7 Output details](#Output-details-13)
    - [11.2.8 Informative output](#Informative-output-16)
  + [11.3 The Format of GNU MO Files](#MO-Files)
* [12 The Programmer’s View](#Programmers)
  + [12.1 About `catgets`](#catgets)
    - [12.1.1 The Interface](#Interface-to-catgets)
    - [12.1.2 Problems with the `catgets` Interface?!](#Problems-with-catgets)
  + [12.2 About `gettext`](#gettext)
    - [12.2.1 The Interface](#Interface-to-gettext)
    - [12.2.2 Solving Ambiguities](#Ambiguities)
    - [12.2.3 Locating Message Catalog Files](#Locating-Catalogs)
    - [12.2.4 How to specify the output character set `gettext` uses](#Charset-conversion)
    - [12.2.5 Using contexts for solving ambiguities](#Contexts)
    - [12.2.6 Additional functions for plural forms](#Plural-forms)
    - [12.2.7 Optimization of the \*gettext functions](#Optimized-gettext)
  + [12.3 Comparing the Two Interfaces](#Comparison)
  + [12.4 Using libintl.a in own programs](#Using-libintl_002ea)
  + [12.5 Being a `gettext` grok](#gettext-grok)
  + [12.6 Temporary Notes for the Programmers Chapter](#Temp-Programmers)
    - [12.6.1 Temporary - Two Possible Implementations](#Temp-Implementations)
    - [12.6.2 Temporary - About `catgets`](#Temp-catgets)
    - [12.6.3 Temporary - Why a single implementation](#Temp-WSI)
    - [12.6.4 Temporary - Notes](#Temp-Notes)
* [13 The Translator’s View](#Translators)
  + [13.1 Organization](#Organization)
  + [13.2 Responsibilities in the Translation Project](#Responsibilities)
  + [13.3 Language dialects](#Dialects)
  + [13.4 Translating plural forms](#Translating-plural-forms)
  + [13.5 Prioritizing messages: How to determine which messages to translate first](#Prioritizing-messages)
* [14 The Maintainer’s View](#Maintainers)
  + [14.1 Flat or Non-Flat Directory Structures](#Flat-and-Non_002dFlat)
  + [14.2 Prerequisite Works](#Prerequisites)
  + [14.3 Invoking the `gettextize` Program](#gettextize-Invocation)
  + [14.4 Files You Must Create or Alter](#Adjusting-Files)
    - [14.4.1 `POTFILES.in` in `po/`](#po_002fPOTFILES_002ein)
    - [14.4.2 `fetch-po` in `po/`](#po_002ffetch_002dpo)
    - [14.4.3 `LINGUAS` in `po/`](#po_002fLINGUAS)
    - [14.4.4 `Makevars` in `po/`](#po_002fMakevars)
    - [14.4.5 Extending `Makefile` in `po/`](#po_002fRules_002d_002a)
    - [14.4.6 `configure.ac` at top level](#configure_002eac)
    - [14.4.7 `config.guess`, `config.sub` at top level](#config_002eguess)
    - [14.4.8 `mkinstalldirs` at top level](#mkinstalldirs)
    - [14.4.9 `aclocal.m4` at top level](#aclocal)
    - [14.4.10 `config.h.in` at top level](#config_002eh_002ein)
    - [14.4.11 `Makefile.in` at top level](#Makefile)
    - [14.4.12 `Makefile.in` in `src/`](#src_002fMakefile)
    - [14.4.13 `gettext.h` in `lib/`](#lib_002fgettext_002eh)
  + [14.5 Autoconf macros for use in `configure.ac`](#autoconf-macros)
    - [14.5.1 AM\_GNU\_GETTEXT in `gettext.m4`](#AM_005fGNU_005fGETTEXT)
    - [14.5.2 AM\_GNU\_GETTEXT\_VERSION in `gettext.m4`](#AM_005fGNU_005fGETTEXT_005fVERSION)
    - [14.5.3 AM\_GNU\_GETTEXT\_NEED in `gettext.m4`](#AM_005fGNU_005fGETTEXT_005fNEED)
    - [14.5.4 AM\_PO\_SUBDIRS in `po.m4`](#AM_005fPO_005fSUBDIRS)
    - [14.5.5 AM\_XGETTEXT\_OPTION in `po.m4`](#AM_005fXGETTEXT_005fOPTION)
    - [14.5.6 AM\_ICONV in `iconv.m4`](#AM_005fICONV)
  + [14.6 Interfacing with the Translators](#Interfacing-with-the-Translators)
  + [14.7 Integrating with Version Control Systems](#Version-Control-Issues)
    - [14.7.1 Avoiding version mismatch in distributed development](#Distributed-Development)
    - [14.7.2 Files to put under version control](#Files-under-Version-Control)
    - [14.7.3 Put the PO Template File under Version Control or not?](#PO-Template-File-under-Version-Control_003f)
    - [14.7.4 Put the PO Files under Version Control or not?](#Translations-under-Version-Control_003f)
    - [14.7.5 Invoking the `autopoint` Program](#autopoint-Invocation)
      * [14.7.5.1 Options](#Options)
      * [14.7.5.2 Informative output](#Informative-output-17)
    - [14.7.6 Invoking the `po-fetch` Program](#po_002dfetch-Invocation)
      * [14.7.6.1 Options](#Options-1)
      * [14.7.6.2 Output files location](#Output-files-location)
      * [14.7.6.3 Informative output](#Informative-output-18)
  + [14.8 Creating a Distribution Tarball](#Release-Management)
* [15 The Installer’s and Distributor’s View](#Installers)
* [16 Other Programming Languages](#Programming-Languages)
  + [16.1 The Language Implementor’s View](#Language-Implementors)
  + [16.2 The Programmer’s View](#Programmers-for-other-Languages)
  + [16.3 The Translator’s View](#Translators-for-other-Languages)
    - [16.3.1 C Format Strings](#c_002dformat)
    - [16.3.2 Objective C Format Strings](#objc_002dformat)
    - [16.3.3 C++ Format Strings](#c_002b_002b_002dformat)
    - [16.3.4 Python Format Strings](#python_002dformat)
    - [16.3.5 Java Format Strings](#java_002dformat)
    - [16.3.6 C# Format Strings](#csharp_002dformat)
    - [16.3.7 JavaScript Format Strings](#javascript_002dformat)
    - [16.3.8 Scheme Format Strings](#scheme_002dformat)
    - [16.3.9 Lisp Format Strings](#lisp_002dformat)
    - [16.3.10 Emacs Lisp Format Strings](#elisp_002dformat)
    - [16.3.11 librep Format Strings](#librep_002dformat)
    - [16.3.12 Rust Format Strings](#rust_002dformat)
    - [16.3.13 Go Format Strings](#go_002dformat)
    - [16.3.14 Ruby Format Strings](#ruby_002dformat)
    - [16.3.15 Shell Format Strings](#sh_002dformat)
    - [16.3.16 awk Format Strings](#awk_002dformat)
    - [16.3.17 Lua Format Strings](#lua_002dformat)
    - [16.3.18 Object Pascal Format Strings](#object_002dpascal_002dformat)
    - [16.3.19 Modula-2 Format Strings](#modula2_002dformat)
    - [16.3.20 D Format Strings](#d_002dformat)
    - [16.3.21 OCaml Format Strings](#ocaml_002dformat)
    - [16.3.22 Smalltalk Format Strings](#smalltalk_002dformat)
    - [16.3.23 Qt Format Strings](#qt_002dformat)
    - [16.3.24 Qt Format Strings](#qt_002dplural_002dformat)
    - [16.3.25 KDE Format Strings](#kde_002dformat)
    - [16.3.26 KUIT Format Strings](#kde_002dkuit_002dformat)
    - [16.3.27 Boost Format Strings](#boost_002dformat)
    - [16.3.28 Tcl Format Strings](#tcl_002dformat)
    - [16.3.29 Perl Format Strings](#perl_002dformat)
    - [16.3.30 PHP Format Strings](#php_002dformat)
    - [16.3.31 GCC internal Format Strings](#gcc_002dinternal_002dformat)
    - [16.3.32 GFC internal Format Strings](#gfc_002dinternal_002dformat)
    - [16.3.33 YCP Format Strings](#ycp_002dformat)
  + [16.4 The Maintainer’s View](#Maintainers-for-other-Languages)
  + [16.5 Individual Programming Languages](#List-of-Programming-Languages)
    - [16.5.1 C, C++, Objective C](#C)
    - [16.5.2 Python](#Python)
    - [16.5.3 Java](#Java)
    - [16.5.4 C#](#C_0023)
    - [16.5.5 JavaScript](#JavaScript)
    - [16.5.6 TypeScript and TSX](#TypeScript)
    - [16.5.7 GNU guile - Scheme](#Scheme)
    - [16.5.8 GNU clisp - Common Lisp](#Common-Lisp)
    - [16.5.9 GNU clisp C sources](#clisp-C)
    - [16.5.10 Emacs Lisp](#Emacs-Lisp)
    - [16.5.11 librep](#librep)
    - [16.5.12 Rust](#Rust)
    - [16.5.13 Go](#Go)
    - [16.5.14 Ruby](#Ruby)
    - [16.5.15 sh - Shell Script](#sh)
      * [16.5.15.1 Three approaches for Shell Scripts](#sh-_002d-Three-approaches)
      * [16.5.15.2 The `gettext.sh` approach](#The-gettext_002esh-approach)
      * [16.5.15.3 The `printf` approach](#The-printf-approach)
      * [16.5.15.4 The `printf_gettext` approach](#The-printf_005fgettext-approach)
      * [16.5.15.5 Preparing Shell Scripts for the `gettext.sh` approach](#Preparing-for-gettext_002esh)
      * [16.5.15.6 Preparing Shell Scripts for the `printf` approach](#Preparing-for-printf)
      * [16.5.15.7 Preparing Shell Scripts for the `printf_gettext` approach](#Preparing-for-printf_005fgettext)
      * [16.5.15.8 Invoking the `gettext` program](#gettext-Invocation)
      * [16.5.15.9 Invoking the `ngettext` program](#ngettext-Invocation)
      * [16.5.15.10 Invoking the `printf_gettext` program](#printf_005fgettext-Invocation)
      * [16.5.15.11 Invoking the `printf_ngettext` program](#printf_005fngettext-Invocation)
      * [16.5.15.12 Invoking the `envsubst` program](#envsubst-Invocation)
      * [16.5.15.13 Invoking the `eval_gettext` function](#eval_005fgettext-Invocation)
      * [16.5.15.14 Invoking the `eval_ngettext` function](#eval_005fngettext-Invocation)
      * [16.5.15.15 Invoking the `eval_pgettext` function](#eval_005fpgettext-Invocation)
      * [16.5.15.16 Invoking the `eval_npgettext` function](#eval_005fnpgettext-Invocation)
    - [16.5.16 bash - Bourne-Again Shell Script](#bash)
    - [16.5.17 GNU awk](#gawk)
    - [16.5.18 Lua](#Lua)
    - [16.5.19 Pascal - Free Pascal Compiler](#Pascal)
    - [16.5.20 Modula-2](#Modula_002d2)
    - [16.5.21 D](#D)
    - [16.5.22 OCaml](#OCaml)
    - [16.5.23 GNU Smalltalk](#Smalltalk)
    - [16.5.24 Vala](#Vala)
    - [16.5.25 wxWidgets library](#wxWidgets)
    - [16.5.26 Tcl - Tk’s scripting language](#Tcl)
    - [16.5.27 Perl](#Perl)
      * [16.5.27.1 General Problems Parsing Perl Code](#General-Problems)
      * [16.5.27.2 Which keywords will xgettext look for?](#Default-Keywords)
      * [16.5.27.3 How to Extract Hash Keys](#Special-Keywords)
      * [16.5.27.4 What are Strings And Quote-like Expressions?](#Quote_002dlike-Expressions)
      * [16.5.27.5 Unsupported Uses Of String Interpolation](#Interpolation-I)
      * [16.5.27.6 Valid Uses Of String Interpolation](#Interpolation-II)
      * [16.5.27.7 When To Use Parentheses](#Parentheses)
      * [16.5.27.8 How To Grok with Long Lines](#Long-Lines)
      * [16.5.27.9 Bugs, Pitfalls, And Things That Do Not Work](#Perl-Pitfalls)
    - [16.5.28 PHP Hypertext Preprocessor](#PHP)
    - [16.5.29 Pike](#Pike)
    - [16.5.30 GNU Compiler Collection sources](#GCC_002dsource)
    - [16.5.31 YCP - YaST2 scripting language](#YCP)
* [17 Other Data Formats](#Data-Formats)
  + [17.1 Internationalizable Data Formats](#Internationalizable-Data)
    - [17.1.1 POT - Portable Object Template](#POT)
    - [17.1.2 Resource String Table](#RST)
    - [17.1.3 Glade - GNOME user interface description](#Glade)
    - [17.1.4 GSettings - GNOME user configuration schema](#GSettings)
    - [17.1.5 AppData - freedesktop.org application description](#AppData)
    - [17.1.6 Preparing Rules for XML Internationalization](#Preparing-ITS-Rules)
      * [17.1.6.1 Specifying ITS Rules](#ITS-Rules)
      * [17.1.6.2 Specifying where to find the ITS Rules](#Locating-Rules)
      * [17.1.6.3 Two Use-cases of Translated Strings in XML](#Two-Use_002dcases-of-Translated-Strings-in-XML)
  + [17.2 Localized Data Formats](#Localized-Data)
    - [17.2.1 Editable Message Catalogs](#Editable-Message-Catalogs)
      * [17.2.1.1 PO - Portable Object](#PO)
      * [17.2.1.2 Java .properties](#Java-_002eproperties)
      * [17.2.1.3 NeXTstep/GNUstep .strings](#GNUstep-_002estrings)
    - [17.2.2 Compiled Message Catalogs](#Compiled-Message-Catalogs)
      * [17.2.2.1 MO - Machine Object](#MO)
      * [17.2.2.2 Java ResourceBundle](#Java-ResourceBundle)
      * [17.2.2.3 C# Satellite Assembly](#C_0023-Satellite-Assembly)
      * [17.2.2.4 C# Resource](#C_0023-Resource)
      * [17.2.2.5 Tcl message catalog](#Tcl-message-catalog)
      * [17.2.2.6 Qt message catalog](#Qt-message-catalog)
    - [17.2.3 Desktop Entry files](#Desktop-Entry)
      * [17.2.3.1 How to handle icons in Desktop Entry files](#Icons)
    - [17.2.4 XML files](#XML)
* [18 Concluding Remarks](#Conclusion)
  + [18.1 History of GNU `gettext`](#History)
  + [18.2 Notes on the Free Translation Project](#The-original-ABOUT_002dNLS)
    - [18.2.1 INSTALL Matters](#INSTALL-Matters)
    - [18.2.2 Using This Package](#Using-This-Package)
    - [18.2.3 Translating Teams](#Translating-Teams)
    - [18.2.4 Available Packages](#Available-Packages)
    - [18.2.5 Using `gettext` in new packages](#Using-gettext-in-own-code)
  + [18.3 Related Readings](#References)
* [Appendix A Language Codes](#Language-Codes)
  + [A.1 Usual Language Codes](#Usual-Language-Codes)
  + [A.2 Rare Language Codes](#Rare-Language-Codes)
* [Appendix B Country Codes](#Country-Codes)
* [Appendix C Licenses](#Licenses)
  + [C.1 GNU GENERAL PUBLIC LICENSE](#GNU-GPL)
  + [C.2 GNU LESSER GENERAL PUBLIC LICENSE](#GNU-LGPL)
  + [C.3 GNU Free Documentation License](#GNU-FDL)
* [Program Index](#Program-Index)
* [Option Index](#Option-Index)
* [Variable Index](#Variable-Index)
* [PO Mode Index](#PO-Mode-Index)
* [Autoconf Macro Index](#Autoconf-Macro-Index)
* [General Index](#Index)

---

Next: [The User’s View](#Users), Previous: [GNU `gettext` utilities](#Top), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 1 Introduction [¶](#Introduction-1)

This chapter explains the goals sought in the creation
of GNU `gettext` and the free Translation Project.
Then, it explains a few broad concepts around
Native Language Support, and positions message translation with regard
to other aspects of national and cultural variance, as they apply
to programs. It also surveys those files used to convey the
translations. It explains how the various tools interact in the
initial generation of these files, and later, how the maintenance
cycle should usually operate.

In this manual, we use *he* when speaking of the programmer or
maintainer, *she* when speaking of the translator, and *they*
when speaking of the installers or end users of the translated program.
This is only a convenience for clarifying the documentation. It is
*absolutely* not meant to imply that some roles are more appropriate
to males or females. Besides, as you might guess, GNU `gettext`
is meant to be useful for people using computers, whatever their sex,
race, religion or nationality!

Please submit suggestions and corrections

* either in the bug tracker at <https://savannah.gnu.org/projects/gettext>
* or by email to `bug-gettext@gnu.org`.

Please include the manual’s edition number and update date in your messages.

* [The Purpose of GNU `gettext`](#Why)
* [I18n, L10n, and Such](#Concepts)
* [Aspects in Native Language Support](#Aspects)
* [Files Conveying Translations](#Files)
* [Overview of GNU `gettext`](#Overview)

---

Next: [I18n, L10n, and Such](#Concepts), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 1.1 The Purpose of GNU `gettext` [¶](#The-Purpose-of-GNU-gettext)

Usually, programs are written and documented in English, and use
English at execution time to interact with users. This is true
not only of GNU software, but also of a great deal of proprietary
and free software. Using a common language is quite handy for
communication between developers, maintainers and users from all
countries. On the other hand, most people are less comfortable with
English than with their own native language, and would prefer to
use their mother tongue for day to day’s work, as far as possible.
Many would simply *love* to see their computer screen showing
a lot less of English, and far more of their own language.

However, to many people, this dream might appear so far fetched that
they may believe it is not even worth spending time thinking about
it. They have no confidence at all that the dream might ever
become true. Yet some have not lost hope, and have organized themselves.
The Translation Project is a formalization of this hope into a
workable structure, which has a good chance to get all of us nearer
the achievement of a truly multi-lingual set of programs.

GNU `gettext` is an important step for the Translation Project,
as it is an asset on which we may build many other steps. This package
offers to programmers, translators and even users, a well integrated
set of tools and documentation. Specifically, the GNU `gettext`
utilities are a set of tools that provides a framework within which
other free packages may produce multi-lingual messages. These tools
include

* A set of conventions about how programs should be written to support
  message catalogs.
* A directory and file naming organization for the message catalogs
  themselves.
* A runtime library supporting the retrieval of translated messages.
* A few stand-alone programs to massage in various ways the sets of
  translatable strings, or already translated strings.
* A library supporting the parsing and creation of files containing
  translated messages.
* A special mode for Emacs[1](#FOOT1) which helps preparing these sets
  and bringing them up to date.

GNU `gettext` is designed to minimize the impact of
internationalization on program sources, keeping this impact as small
and hardly noticeable as possible. Internationalization has better
chances of succeeding if it is very light weighted, or at least,
appear to be so, when looking at program sources.

The Translation Project also uses the GNU `gettext` distribution
as a vehicle for documenting its structure and methods. This goes
beyond the strict technicalities of documenting the GNU `gettext`
proper. By so doing, translators will find in a single place, as
far as possible, all they need to know for properly doing their
translating work. Also, this supplemental documentation might also
help programmers, and even curious users, in understanding how GNU
`gettext` is related to the remainder of the Translation
Project, and consequently, have a glimpse at the *big picture*.

---

Next: [Aspects in Native Language Support](#Aspects), Previous: [The Purpose of GNU `gettext`](#Why), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 1.2 I18n, L10n, and Such [¶](#I18n_002c-L10n_002c-and-Such)

Two long words appear all the time when we discuss support of native
language in programs, and these words have a precise meaning, worth
being explained here, once and for all in this document. The words are
*internationalization* and *localization*. Many people,
tired of writing these long words over and over again, took the
habit of writing *i18n* and *l10n* instead, quoting the first
and last letter of each word, and replacing the run of intermediate
letters by a number merely telling how many such letters there are.
But in this manual, in the sake of clarity, we will patiently write
the names in full, each time…

By *internationalization*, one refers to the operation by which a
program, or a set of programs turned into a package, is made aware of and
able to support multiple languages. This is a generalization process,
by which the programs are untied from calling only English strings or
other English specific habits, and connected to generic ways of doing
the same, instead. Program developers may use various techniques to
internationalize their programs. Some of these have been standardized.
GNU `gettext` offers one of these standards. See [The Programmer’s View](#Programmers).

By *localization*, one means the operation by which, in a set
of programs already internationalized, one gives the program all
needed information so that it can adapt itself to handle its input
and output in a fashion which is correct for some native language and
cultural habits. This is a particularisation process, by which generic
methods already implemented in an internationalized program are used
in specific ways. The programming environment puts several functions
to the programmers disposal which allow this runtime configuration.
The formal description of specific set of cultural habits for some
country, together with all associated translations targeted to the
same native language, is called the *locale* for this language
or country. Users achieve localization of programs by setting proper
values to special environment variables, prior to executing those
programs, identifying which locale should be used.

In fact, locale message support is only one component of the cultural
data that makes up a particular locale. There are a whole host of
routines and functions provided to aid programmers in developing
internationalized software and which allow them to access the data
stored in a particular locale. When someone presently refers to a
particular locale, they are obviously referring to the data stored
within that particular locale. Similarly, if a programmer is referring
to “accessing the locale routines”, they are referring to the
complete suite of routines that access all of the locale’s information.

One uses the expression *Native Language Support*, or merely NLS,
for speaking of the overall activity or feature encompassing both
internationalization and localization, allowing for multi-lingual
interactions in a program. In a nutshell, one could say that
internationalization is the operation by which further localizations
are made possible.

Also, very roughly said, when it comes to multi-lingual messages,
internationalization is usually taken care of by programmers, and
localization is usually taken care of by translators.

---

Next: [Files Conveying Translations](#Files), Previous: [I18n, L10n, and Such](#Concepts), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 1.3 Aspects in Native Language Support [¶](#Aspects-in-Native-Language-Support)

For a totally multi-lingual distribution, there are many things to
translate beyond output messages.

* As of today, GNU `gettext` offers a complete toolset for
  translating messages output by C programs. Perl scripts and shell
  scripts will also need to be translated. Even if there are today some hooks
  by which this can be done, these hooks are not integrated as well as they
  should be.
* Some programs, like `autoconf` or `bison`, are able
  to produce other programs (or scripts). Even if the generating
  programs themselves are internationalized, the generated programs they
  produce may need internationalization on their own, and this indirect
  internationalization could be automated right from the generating
  program. In fact, quite usually, generating and generated programs
  could be internationalized independently, as the effort needed is
  fairly orthogonal.
* A few programs include textual tables which might need translation
  themselves, independently of the strings contained in the program
  itself. For example, RFC 1345 gives an English description for each
  character which the `recode` program is able to reconstruct at execution.
  Since these descriptions are extracted from the RFC by mechanical means,
  translating them properly would require a prior translation of the RFC
  itself.
* Almost all programs accept options, which are often worded out so to
  be descriptive for the English readers; one might want to consider
  offering translated versions for program options as well.
* Many programs read, interpret, compile, or are somewhat driven by
  input files which are texts containing keywords, identifiers, or
  replies which are inherently translatable. For example, one may want
  `gcc` to allow diacriticized characters in identifiers or use
  translated keywords; ‘`rm -i`’ might accept something else than
  ‘`y`’ or ‘`n`’ for replies, etc. Even if the program will
  eventually make most of its output in the foreign languages, one has
  to decide whether the input syntax, option values, etc., are to be
  localized or not.
* The manual accompanying a package, as well as all documentation files
  in the distribution, could surely be translated, too. Translating a
  manual, with the intent of later keeping up with updates, is a major
  undertaking in itself, generally.

As we already stressed, translation is only one aspect of locales.
Other internationalization aspects are system services and are handled
in GNU `libc`. There
are many attributes that are needed to define a country’s cultural
conventions. These attributes include beside the country’s native
language, the formatting of the date and time, the representation of
numbers, the symbols for currency, etc. These local *rules* are
termed the country’s locale. The locale represents the knowledge
needed to support the country’s native attributes.

There are a few major areas which may vary between countries and
hence, define what a locale must describe. The following list helps
putting multi-lingual messages into the proper context of other tasks
related to locales. See the GNU `libc` manual for details.

*Characters and Codesets* [¶](#index-codeset)
:   The codeset most commonly used through out the USA and most English
    speaking parts of the world is the ASCII codeset. However, there are
    many characters needed by various locales that are not found within
    this codeset. The 8-bit ISO 8859-1 code set has most of the special
    characters needed to handle the major European languages. However, in
    many cases, choosing ISO 8859-1 is nevertheless not adequate: it
    doesn’t even handle the major European currency. Hence each locale
    will need to specify which codeset they need to use and will need
    to have the appropriate character handling routines to cope with
    the codeset.

*Currency* [¶](#index-currency-symbols)
:   The symbols used vary from country to country as does the position
    used by the symbol. Software needs to be able to transparently
    display currency figures in the native mode for each locale.

*Dates* [¶](#index-date-format)
:   The format of date varies between locales. For example, Christmas day
    in 1994 is written as 12/25/94 in the USA and as 25/12/94 in Australia.
    Other countries might use ISO 8601 dates, etc.

    Time of the day may be noted as hh:mm, hh.mm,
    or otherwise. Some locales require time to be specified in 24-hour
    mode rather than as AM or PM. Further, the nature and yearly extent
    of the Daylight Saving correction vary widely between countries.

*Numbers* [¶](#index-number-format)
:   Numbers can be represented differently in different locales.
    For example, the following numbers are all written correctly for
    their respective locales:

    ```
    12,345.67       English
    12.345,67       German
     12345,67       French
    1,2345.67       Asia
    ```

    Some programs could go further and use different unit systems, like
    English units or Metric units, or even take into account variants
    about how numbers are spelled in full.

*Messages* [¶](#index-messages)
:   The most obvious area is the language support within a locale. This is
    where GNU `gettext` provides the means for developers and users to
    easily change the language that the software uses to communicate to
    the user.

These areas of cultural conventions are called *locale categories*.
It is an unfortunate term; *locale aspects* or *locale feature
categories* would be a better term, because each “locale category”
describes an area or task that requires localization. The concrete data
that describes the cultural conventions for such an area and for a particular
culture is also called a *locale category*. In this sense, a locale
is composed of several locale categories: the locale category describing
the codeset, the locale category describing the formatting of numbers,
the locale category containing the translated messages, and so on.

Components of locale outside of message handling are standardized in
the ISO C standard and the POSIX:2001 standard (also known as the SUSV3
specification). GNU `libc`
fully implements this, and most other modern systems provide a more
or less reasonable support for at least some of the missing components.

---

Next: [Overview of GNU `gettext`](#Overview), Previous: [Aspects in Native Language Support](#Aspects), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 1.4 Files Conveying Translations [¶](#Files-Conveying-Translations)

The letters PO in `.po` files means Portable Object, to
distinguish it from `.mo` files, where MO stands for Machine
Object. This paradigm, as well as the PO file format, is inspired
by the NLS standard developed by Uniforum, and first implemented by
Sun in their Solaris system.

PO files are meant to be read and edited by humans, and associate each
original, translatable string of a given package with its translation
in a particular target language. A single PO file is dedicated to
a single target language. If a package supports many languages,
there is one such PO file per language supported, and each package
has its own set of PO files. These PO files are best created by
the `xgettext` program, and later updated or refreshed through
the `msgmerge` program. Program `xgettext` extracts all
marked messages from a set of C files and initializes a PO file with
empty translations. Program `msgmerge` takes care of adjusting
PO files between releases of the corresponding sources, commenting
obsolete entries, initializing new ones, and updating all source
line references. Files ending with `.pot` are kind of base
translation files found in distributions, in PO file format.

MO files are meant to be read by programs, and are binary in nature.
A few systems already offer tools for creating and handling MO files
as part of the Native Language Support coming with the system, but the
format of these MO files is often different from system to system,
and non-portable. The tools already provided with these systems don’t
support all the features of GNU `gettext`. Therefore GNU
`gettext` uses its own format for MO files. Files ending with
`.gmo` are really MO files, when it is known that these files use
the GNU format.

---

Previous: [Files Conveying Translations](#Files), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 1.5 Overview of GNU `gettext` [¶](#Overview-of-GNU-gettext)

The following diagram summarizes the relation between the files
handled by GNU `gettext` and the tools acting on these files.
It is followed by somewhat detailed explanations, which you should
read while keeping an eye on the diagram. Having a clear understanding
of these interrelations will surely help programmers, translators
and maintainers.

```
Original C Sources ───> Preparation ───> Marked C Sources ───╮
                                                             │
              ╭─────────<─── GNU gettext Library             │
╭─── make <───┤                                              │
│             ╰─────────<────────────────────┬───────────────╯
│                                            │
│   ╭─────<─── PACKAGE.pot <─── xgettext <───╯   ╭───<─── PO Compendium
│   │                                            │              ↑
│   │                                            ╰───╮          │
│   ╰───╮                                            ├───> PO editor ───╮
│       ├────> msgmerge ──────> LANG.po ────>────────╯                  │
│   ╭───╯                                                               │
│   │                                                                   │
│   ╰─────────────<───────────────╮                                     │
│                                 ├─── New LANG.po <────────────────────╯
│   ╭─── LANG.gmo <─── msgfmt <───╯
│   │
│   ╰───> install ───> /.../LANG/PACKAGE.mo ───╮
│                                              ├───> "Hello world!"
╰───────> install ───> /.../bin/PROGRAM ───────╯
```

As a programmer, the first step to bringing GNU `gettext`
into your package is identifying, right in the C sources, those strings
which are meant to be translatable, and those which are untranslatable.
This tedious job can be done a little more comfortably using emacs PO
mode, but you can use any means familiar to you for modifying your
C sources. Beside this some other simple, standard changes are needed to
properly initialize the translation library. See [Preparing Program Sources](#Sources), for
more information about all this.

For newly written software the strings of course can and should be
marked while writing it. The `gettext` approach makes this
very easy. Simply put the following lines at the beginning of each file
or in a central header file:

```
#define _(String) (String)
#define N_(String) String
#define textdomain(Domain)
#define bindtextdomain(Package, Directory)
```

Doing this allows you to prepare the sources for internationalization.
Later when you feel ready for the step to use the `gettext` library
simply replace these definitions by the following:

```
#include <libintl.h>
#define _(String) gettext (String)
#define gettext_noop(String) String
#define N_(String) gettext_noop (String)
```

and link against `libintl.a` or `libintl.so`. Note that on
GNU systems, you don’t need to link with `libintl` because the
`gettext` library functions are already contained in GNU libc.
That is all you have to change.

Once the C sources have been modified, the `xgettext` program
is used to find and extract all translatable strings, and create a
PO template file out of all these. This `package.pot` file
contains all original program strings. It has sets of pointers to
exactly where in C sources each string is used. All translations
are set to empty. The letter `t` in `.pot` marks this as
a Template PO file, not yet oriented towards any particular language.
See [Invoking the `xgettext` Program](#xgettext-Invocation), for more details about how one calls the
`xgettext` program. If you are *really* lazy, you might
be interested at working a lot more right away, and preparing the
whole distribution setup (see [The Maintainer’s View](#Maintainers)). By doing so, you
spare yourself typing the `xgettext` command, as `make`
should now generate the proper things automatically for you!

The first time through, there is no `lang.po` yet, so the
`msgmerge` step may be skipped and replaced by a mere copy of
`package.pot` to `lang.po`, where lang
represents the target language. See [Creating or preparing for translating a PO file](#Creating) for details.

Then comes the initial translation of messages. Translation in
itself is a whole matter, still exclusively meant for humans,
and whose complexity far overwhelms the level of this manual.
Nevertheless, a few hints are given in some other chapter of this
manual (see [The Translator’s View](#Translators)). You will also find there indications
about how to contact translating teams, or becoming part of them,
for sharing your translating concerns with others who target the same
native language.

While adding the translated messages into the `lang.po`
PO file, if you are not using one of the dedicated PO file editors
(see [Editing PO Files](#Editing)), you are on your own
for ensuring that your efforts fully respect the PO file format, and quoting
conventions (see [The Format of PO Files](#PO-Files)). This is surely not an impossible task,
as this is the way many people have handled PO files around 1995.
On the other hand, by using a PO file editor, most details
of PO file format are taken care of for you, but you have to acquire
some familiarity with PO file editor itself.

If some common translations have already been saved into a compendium
PO file, translators may use PO mode for initializing untranslated
entries from the compendium, and also save selected translations into
the compendium, updating it (see [Using Translation Compendia](#Compendium)). Compendium files
are meant to be exchanged between members of a given translation team.

Programs, or packages of programs, are dynamic in nature: users write
bug reports and suggestion for improvements, maintainers react by
modifying programs in various ways. The fact that a package has
already been internationalized should not make maintainers shy
of adding new strings, or modifying strings already translated.
They just do their job the best they can. For the Translation
Project to work smoothly, it is important that maintainers do not
carry translation concerns on their already loaded shoulders, and that
translators be kept as free as possible of programming concerns.

The only concern maintainers should have is carefully marking new
strings as translatable, when they should be, and do not otherwise
worry about them being translated, as this will come in proper time.
Consequently, when programs and their strings are adjusted in various
ways by maintainers, and for matters usually unrelated to translation,
`xgettext` would construct `package.pot` files which are
evolving over time, so the translations carried by `lang.po`
are slowly fading out of date.

It is important for translators (and even maintainers) to understand
that package translation is a continuous process in the lifetime of a
package, and not something which is done once and for all at the start.
After an initial burst of translation activity for a given package,
interventions are needed once in a while, because here and there,
translated entries become obsolete, and new untranslated entries
appear, needing translation.

The `msgmerge` program has the purpose of refreshing an already
existing `lang.po` file, by comparing it with a newer
`package.pot` template file, extracted by `xgettext`
out of recent C sources. The refreshing operation adjusts all
references to C source locations for strings, since these strings
move as programs are modified. Also, `msgmerge` comments out as
obsolete, in `lang.po`, those already translated entries
which are no longer used in the program sources (see [Obsolete Entries](#Obsolete-Entries)). It finally discovers new strings and inserts them in
the resulting PO file as untranslated entries (see [Untranslated Entries](#Untranslated-Entries)). See [Invoking the `msgmerge` Program](#msgmerge-Invocation), for more information about what
`msgmerge` really does.

Whatever route or means taken, the goal is to obtain an updated
`lang.po` file offering translations for all strings.

The temporal mobility, or fluidity of PO files, is an integral part of
the translation game, and should be well understood, and accepted.
People resisting it will have a hard time participating in the
Translation Project, or will give a hard time to other participants! In
particular, maintainers should relax and include all available official
PO files in their distributions, even if these have not recently been
updated, without exerting pressure on the translator teams to get the
job done. The pressure should rather come
from the community of users speaking a particular language, and
maintainers should consider themselves fairly relieved of any concern
about the adequacy of translation files. On the other hand, translators
should reasonably try updating the PO files they are responsible for,
while the package is undergoing pretest, prior to an official
distribution.

Once the PO file is complete and dependable, the `msgfmt` program
is used for turning the PO file into a machine-oriented format, which
may yield efficient retrieval of translations by the programs of the
package, whenever needed at runtime (see [The Format of GNU MO Files](#MO-Files)). See [Invoking the `msgfmt` Program](#msgfmt-Invocation), for more information about all modes of execution
for the `msgfmt` program.

Finally, the modified and marked C sources are compiled and linked
with the GNU `gettext` library, usually through the operation of
`make`, given a suitable `Makefile` exists for the project,
and the resulting executable is installed somewhere users will find it.
The MO files themselves should also be properly installed. Given the
appropriate environment variables are set (see [Setting the Locale through Environment Variables](#Setting-the-POSIX-Locale)),
the program should localize itself automatically, whenever it executes.

Shipping the MO files as separate files,
as opposed to embedding them in the executable,
has three advantages:

* For the users:
  It allows users to prepare and install new translations,
  without needing to rebuild the package (which may require developer skills).
* For the distributors:
  It allows distributions to
  ship translations that were produced after the release of the package.
* For the vendors of complex packages:
  When lengthy quality assurance steps are required before making a release,
  this quality assurance can start
  before the translators have produced the translations,
  shortening the critical path of the release schedule by a week or two.

Embedding the translations in the executable,
whether by the ISO C `#embed` directive or through other means,
would deprive users without developer skills
of the ability to fix translation mistakes and add new translations.

The remainder of this manual has the purpose of explaining in depth the various
steps outlined above.

---

Next: [The Format of PO Files](#PO-Files), Previous: [Introduction](#Introduction), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 2 The User’s View [¶](#The-User_0027s-View)

Nowadays, when users log into a computer, they usually find that all
their programs show messages in their native language – at least for
users of languages with an active free software community, like French or
German; to a lesser extent for languages with a smaller participation in
free software and the GNU project, like Hindi and Filipino.

How does this work? How can the user influence the language that is used
by the programs? This chapter will answer it.

* [Operating System Installation](#System-Installation)
* [Setting the Locale Used by GUI Programs](#Setting-the-GUI-Locale)
* [Setting the Locale through Environment Variables](#Setting-the-POSIX-Locale)
* [Obtaining good output in a Windows console](#Working-in-a-Windows-console)
* [Installing Translations for Particular Programs](#Installing-Localizations)

---

Next: [Setting the Locale Used by GUI Programs](#Setting-the-GUI-Locale), Up: [The User’s View](#Users)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 2.1 Operating System Installation [¶](#Operating-System-Installation)

The default language is often already specified during operating system
installation. When the operating system is installed, the installer
typically asks for the language used for the installation process and,
separately, for the language to use in the installed system. Some OS
installers only ask for the language once.

This determines the system-wide default language for all users. But the
installers often give the possibility to install extra localizations for
additional languages. For example, the localizations of KDE (the K
Desktop Environment) and LibreOffice are often bundled separately, as one
installable package per language.

At this point it is good to consider the intended use of the machine: If
it is a machine designated for personal use, additional localizations are
probably not necessary. If, however, the machine is in use in an
organization or company that has international relationships, one can
consider the needs of guest users. If you have a guest from abroad, for
a week, what could be his preferred locales? It may be worth installing
these additional localizations ahead of time, since they cost only a bit
of disk space at this point.

The system-wide default language is the locale configuration that is used
when a new user account is created. But the user can have his own locale
configuration that is different from the one of the other users of the
same machine. He can specify it, typically after the first login, as
described in the next section.

---

Next: [Setting the Locale through Environment Variables](#Setting-the-POSIX-Locale), Previous: [Operating System Installation](#System-Installation), Up: [The User’s View](#Users)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 2.2 Setting the Locale Used by GUI Programs [¶](#Setting-the-Locale-Used-by-GUI-Programs)

The immediately available programs in a user’s desktop come from a group
of programs called a “desktop environment”; it usually includes the window
manager, a web browser, a text editor, and more. The most common free
desktop environments are KDE, GNOME, and Xfce.

The locale used by GUI programs of the desktop environment can be specified
in a configuration screen called “control center”, “language settings”
or “country settings”.

Individual GUI programs that are not part of the desktop environment can
have their locale specified either in a settings panel, or through environment
variables.

For some programs, it is possible to specify the locale through environment
variables, possibly even to a different locale than the desktop’s locale.
This means, instead of starting a program through a menu or from the file
system, you can start it from the command-line, after having set some
environment variables. The environment variables can be those specified
in the next section ([Setting the Locale through Environment Variables](#Setting-the-POSIX-Locale)); for some versions of
KDE, however, the locale is specified through a variable `KDE_LANG`,
rather than `LANG` or `LC_ALL`.

---

Next: [Obtaining good output in a Windows console](#Working-in-a-Windows-console), Previous: [Setting the Locale Used by GUI Programs](#Setting-the-GUI-Locale), Up: [The User’s View](#Users)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 2.3 Setting the Locale through Environment Variables [¶](#Setting-the-Locale-through-Environment-Variables)

As a user, if your language has been installed for this package, in the
simplest case, you only have to set the `LANG` environment variable
to the appropriate ‘`ll_CC`’ combination. For example,
let’s suppose that you speak German and live in Germany. At the shell
prompt, merely execute
‘`setenv LANG de_DE`’ (in `csh`),
‘`export LANG; LANG=de_DE`’ (in `sh`) or
‘`export LANG=de_DE`’ (in `bash`). This can be done from your
`.login` or `.profile` file, once and for all.

* [Locale Names](#Locale-Names)
* [Locale Environment Variables](#Locale-Environment-Variables)
* [Specifying a Priority List of Languages](#The-LANGUAGE-variable)

---

Next: [Locale Environment Variables](#Locale-Environment-Variables), Up: [Setting the Locale through Environment Variables](#Setting-the-POSIX-Locale)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 2.3.1 Locale Names [¶](#Locale-Names-1)

A locale name usually has the form ‘`ll_CC`’. Here

* ‘`ll`’ is an ISO 639 two-letter language code.
  For some languages,
  a two-letter code does not exist, and a three-letter code is used instead.
* ‘`CC`’ is an ISO 3166 two-letter code of a country or territory.

For example,
for German in Germany, ll is `de`, and CC is `DE`.
You find a list of the language codes in appendix [Language Codes](#Language-Codes) and
a list of the country codes in appendix [Country Codes](#Country-Codes).

You might think that the country code specification is redundant. But in
fact, some languages have dialects in different countries. For example,
‘`de_AT`’ is used for Austria, and ‘`pt_BR`’ for Brazil. The country
code serves to distinguish the dialects.

Many locale names have an extended syntax
‘`ll_CC.encoding`’ that also specifies the character
encoding. These are in use because between 2000 and 2005, most users have
switched to locales in UTF-8 encoding. For example, the German locale on
glibc systems is nowadays ‘`de_DE.UTF-8`’. The older name ‘`de_DE`’
still refers to the German locale as of 2000 that stores characters in
ISO-8859-1 encoding – a text encoding that cannot even accommodate the Euro
currency sign.

Some locale names use ‘`ll_CC@variant`’ instead of
‘`ll_CC`’. The ‘`@variant`’ can denote any kind of
characteristics that is not already implied by the language ll and
the country CC. It can denote a particular monetary unit. For example,
on glibc systems, ‘`de_DE@euro`’ denotes the locale that uses the Euro
currency, in contrast to the older locale ‘`de_DE`’ which implies the use
of the currency before 2002. It can also denote a dialect of the language,
or the script used to write text (for example, ‘`sr_RS@latin`’ uses the
Latin script, whereas ‘`sr_RS`’ uses the Cyrillic script to write Serbian),
or the orthography rules, or similar.

On other systems, some variations of this scheme are used, such as
‘`ll`’. You can get the list of locales supported by your system
for your language by running the command ‘`locale -a | grep '^ll'`’.

There are also two special locales:

* The locale called ‘`C`’.  
  When it is used, it disables all localization: in this locale, all programs
  standardized by POSIX use English messages and an unspecified character
  encoding (often US-ASCII, but sometimes also ISO-8859-1 or UTF-8, depending on
  the operating system).
* The locale called ‘`C.UTF-8`’.  
  This locale exists on all modern GNU and Unix systems,
  but not on all operating systems.
  When it is used, it disables all localization as well.
  It uses UTF-8 as character encoding.

---

Next: [Specifying a Priority List of Languages](#The-LANGUAGE-variable), Previous: [Locale Names](#Locale-Names), Up: [Setting the Locale through Environment Variables](#Setting-the-POSIX-Locale)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 2.3.2 Locale Environment Variables [¶](#Locale-Environment-Variables-1)

A locale is composed of several *locale categories*, see [Aspects in Native Language Support](#Aspects).
When a program looks up locale dependent values, it does this according to
the following environment variables, in priority order:

1. `LANGUAGE`
2. `LC_ALL`
3. `LC_xxx`, according to selected locale category:
   `LC_CTYPE`, `LC_NUMERIC`, `LC_TIME`, `LC_COLLATE`,
   `LC_MONETARY`, `LC_MESSAGES`, ...
4. `LANG`

Variables whose value is set but is empty are ignored in this lookup.

`LANG` is the normal environment variable for specifying a locale.
As a user, you normally set this variable (unless some of the other variables
have already been set by the system, in `/etc/profile` or similar
initialization files).

`LC_CTYPE`, `LC_NUMERIC`, `LC_TIME`, `LC_COLLATE`,
`LC_MONETARY`, `LC_MESSAGES`, and so on, are the environment
variables meant to override `LANG` and affecting a single locale
category only. For example, assume you are a Swedish user in Spain, and you
want your programs to handle numbers and dates according to Spanish
conventions, and only the messages should be in Swedish. Then you could
create a locale named ‘`sv_ES`’ or ‘`sv_ES.UTF-8`’ by use of the
`localedef` program. But it is simpler, and achieves the same effect,
to set the `LANG` variable to `es_ES.UTF-8` and the
`LC_MESSAGES` variable to `sv_SE.UTF-8`; these two locales come
already preinstalled with the operating system.

`LC_ALL` is an environment variable that overrides all of these.
It is typically used in scripts that run particular programs. For example,
`configure` scripts generated by GNU autoconf use `LC_ALL` to make
sure that the configuration tests don’t operate in locale dependent ways.

Some systems, unfortunately, set `LC_ALL` in `/etc/profile` or in
similar initialization files. As a user, you therefore have to unset this
variable if you want to set `LANG` and optionally some of the other
`LC_xxx` variables.

The `LANGUAGE` variable is described in the next subsection.

---

Previous: [Locale Environment Variables](#Locale-Environment-Variables), Up: [Setting the Locale through Environment Variables](#Setting-the-POSIX-Locale)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 2.3.3 Specifying a Priority List of Languages [¶](#Specifying-a-Priority-List-of-Languages)

Not all programs have translations for all languages. By default, an
English message is shown in place of a nonexistent translation. If you
understand other languages, you can set up a priority list of languages.
This is done through a different environment variable, called
`LANGUAGE`. GNU `gettext` gives preference to `LANGUAGE`
over `LC_ALL` and `LANG` for the purpose of message handling,
but you still need to have `LANG` (or `LC_ALL`) set to the primary
language; this is required by other parts of the system libraries.
For example, some Swedish users who would rather read translations in
German than English for when Swedish is not available, set `LANGUAGE`
to ‘`sv:de`’ while leaving `LANG` to ‘`sv_SE`’.

Special advice for Norwegian users: The language code for Norwegian
bokmål changed from ‘`no`’ to ‘`nb`’ back in 2003.
Most of the message catalogs for this language are installed under ‘`nb`’.
But in order to also use the older ones installed under ‘`no`’, it is
recommended for Norwegian users to set `LANGUAGE` to ‘`nb:no`’.

In the `LANGUAGE` environment variable, but not in the other
environment variables, ‘`ll_CC`’ combinations can be
abbreviated as ‘`ll`’ to denote the language’s main dialect.
For example, ‘`de`’ is equivalent to ‘`de_DE`’ (German as spoken in
Germany), and ‘`pt`’ to ‘`pt_PT`’ (Portuguese as spoken in Portugal)
in this context.

Special advice for Chinese users:
Users who want to see translations with Simplified Chinese characters
should set `LANGUAGE` to `zh_CN`,
whereas users who want to see translations with Traditional Chinese characters
should set `LANGUAGE` to `zh_TW`.
Chinese users in Singapore will want to set it to `zh_SG:zh_CN`,
Chinese users in Hong Kong will want to set it to `zh_HK:zh_TW`,
and Chinese users in Macao will want to set it to `zh_MO:zh_TW`.
Here `zh_CN` or `zh_TW`, respectively, acts as fallback,
since only few packages have translations
for `zh_SG`, `zh_HK`, or `zh_MO`.

Note: The variable `LANGUAGE` is ignored if the locale is set to
‘`C`’. In other words, you have to first enable localization, by setting
`LANG` (or `LC_ALL`) to a value other than ‘`C`’, before you can
use a language priority list through the `LANGUAGE` variable.

---

Next: [Installing Translations for Particular Programs](#Installing-Localizations), Previous: [Setting the Locale through Environment Variables](#Setting-the-POSIX-Locale), Up: [The User’s View](#Users)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 2.4 Obtaining good output in a Windows console [¶](#Obtaining-good-output-in-a-Windows-console)

On Windows, consoles such as the one started by the `cmd.exe`
program do input and output in an encoding, called “OEM code page”,
that is different from the encoding that text-mode programs usually use,
called “ANSI code page”. (Note: This problem does not exist for
Cygwin consoles; these consoles do input and output in the UTF-8
encoding.) As a workaround, you may request that the programs produce
output in this “OEM” encoding. To do so, set the environment variable
`OUTPUT_CHARSET` to the “OEM” encoding, through a command such as

```
set OUTPUT_CHARSET=CP850
```

Note: This has an effect only on strings looked up in message catalogs;
other categories of text are usually not affected by this setting.
Note also that this environment variable also affects output sent to a
file or to a pipe; output to a file is most often expected to be in the
“ANSI” or in the UTF-8 encoding.

Here are examples of the “ANSI” and “OEM” code pages:

| Territories | ANSI encoding | OEM encoding |
| --- | --- | --- |
| Western Europe | CP1252 | CP850 |
| Slavic countries (Latin 2) | CP1250 | CP852 |
| Baltic countries | CP1257 | CP775 |
| Russia | CP1251 | CP866 |

---

Previous: [Obtaining good output in a Windows console](#Working-in-a-Windows-console), Up: [The User’s View](#Users)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 2.5 Installing Translations for Particular Programs [¶](#Installing-Translations-for-Particular-Programs)

Languages are not equally well supported in all packages using GNU
`gettext`, and more translations are added over time. Usually, you
use the translations that are shipped with the operating system
or with particular packages that you install afterwards. But you can also
install newer localizations directly. For doing this, you will need an
understanding where each localization file is stored on the file system.

For programs that participate in the Translation Project, you can start
looking for translations here:
<https://translationproject.org/team/index.html>.

For programs that are part of the KDE project, the starting point is:
<https://l10n.kde.org/>.

For programs that are part of the GNOME project, the starting point is:
<https://wiki.gnome.org/TranslationProject>.

For other programs, you may check whether the program’s source code package
contains some `ll.po` files; often they are kept together in a
directory called `po/`. Each `ll.po` file contains the
message translations for the language whose abbreviation of ll.

---

Next: [Preparing Program Sources](#Sources), Previous: [The User’s View](#Users), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 3 The Format of PO Files [¶](#The-Format-of-PO-Files)

The GNU `gettext` toolset helps programmers and translators
at producing, updating and using translation files, mainly those
PO files which are textual, editable files. This chapter explains
the format of PO files.

* [What an entry looks like](#PO-File-Entries)
* [Workflow flags](#Workflow-flags)
* [Sticky flags](#Sticky-flags)
* [Entries with Context](#Entries-with-Context)
* [Entries with Plural Forms](#Entries-with-Plural-Forms)
* [Further details on the PO file format](#More-Details)
* [Evolution of the PO File Format](#PO-File-Format-Evolution)

---

Next: [Workflow flags](#Workflow-flags), Up: [The Format of PO Files](#PO-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 3.1 What an entry looks like [¶](#What-an-entry-looks-like)

A PO file is made up of many entries, each entry holding the relation
between an original untranslated string and its corresponding
translation. All entries in a given PO file usually pertain
to a single project, and all translations are expressed in a single
target language. One PO file *entry* has the following schematic
structure:

```
white-space
#  translator-comments
#. extracted-comments
#: reference...
#, flag...
#| msgid previous-untranslated-string
msgid untranslated-string
msgstr translated-string
```

The general structure of a PO file should be well understood by
the translator. When using PO mode, very little has to be known
about the format details, as PO mode takes care of them for her.

A simple entry can look like this:

```
#: lib/error.c:116
msgid "Unknown system error"
msgstr "Error desconegut del sistema"
```

Entries begin with some optional white space. Usually, when generated
through GNU `gettext` tools, there is exactly one blank line
between entries. Then comments follow, on lines all starting with the
character `#`. There are two kinds of comments: those which have
some white space immediately following the `#` - the translator
comments -, which comments are created and maintained exclusively by the
translator, and those which have some non-white character just after the
`#` - the automatic comments -, which comments are created and
maintained automatically by GNU `gettext` tools. Comment lines
starting with `#.` contain comments given by the programmer, directed
at the translator; these comments are called extracted comments
because the `xgettext` program extracts them from the program’s
source code. Comment lines starting with `#:` contain references to
the program’s source code. Comment lines starting with `#,` contain
flags; more about these below. Comment lines starting with `#|`
contain the previous untranslated string for which the translator gave
a translation.

All comments, of either kind, are optional.

References to the program’s source code, in lines that start with `#:`,
are of the form `file_name:line_number` or just
file\_name. If the file\_name contains spaces. it is enclosed
within Unicode characters U+2068 and U+2069.

After white space and comments, entries show two strings, namely
first the untranslated string as it appears in the original program
sources, and then, the translation of this string. The original
string is introduced by the keyword `msgid`, and the translation,
by `msgstr`. The two strings, untranslated and translated,
are quoted in various ways in the PO file, using `"`
delimiters and `\` escapes, but the translator does not really
have to pay attention to the precise quoting format, as PO mode fully
takes care of quoting for her.

The `msgid` strings, as well as automatic comments, are produced
and managed by other GNU `gettext` tools, and PO mode does not
provide means for the translator to alter these. The most she can
do is merely deleting them, and only by deleting the whole entry.
On the other hand, the `msgstr` string, as well as translator
comments, are really meant for the translator, and PO mode gives her
the full control she needs.

The previous-untranslated-string is optionally inserted by the
`msgmerge` program, at the same time when it marks a message fuzzy.
It helps the translator to see which changes were done by the developers
on the untranslated-string.

The comment lines beginning with `#,` are special because they are
not completely ignored by the programs as comments generally are. The
comma separated list of flags is used by the `msgfmt`
program to give the user some better diagnostic messages. Currently
there are two forms of flags defined: workflow flags and sticky flags.

---

Next: [Sticky flags](#Sticky-flags), Previous: [What an entry looks like](#PO-File-Entries), Up: [The Format of PO Files](#PO-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 3.2 Workflow flags [¶](#Workflow-flags-1)

Workflow flags are flags that can be added or removed
by the translator,
by the PO file editor that the translator uses,
or by tools that are part of the workflow.
These flags represent the state of the entry in the workflow.

Currently, only one workflow flag is defined:

`fuzzy` [¶](#index-fuzzy-flag)
:   This flag can be generated by the `msgmerge` program or it can be
    inserted by the translator herself. It shows that the `msgstr`
    string might not be a correct translation (anymore). Only the translator
    can judge if the translation requires further modification, or is
    acceptable as is. Once satisfied with the translation, she then removes
    this `fuzzy` attribute. The `msgmerge` program inserts this
    when it combined the `msgid` and `msgstr` entries after fuzzy
    search only. See [Fuzzy Entries](#Fuzzy-Entries).

---

Next: [Entries with Context](#Entries-with-Context), Previous: [Workflow flags](#Workflow-flags), Up: [The Format of PO Files](#PO-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]
