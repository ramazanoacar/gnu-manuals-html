### 5.1 Invoking the `xgettext` Program [¶](#Invoking-the-xgettext-Program)

```
xgettext [option] [inputfile] ...
```

The `xgettext` program extracts translatable strings from given
input files.

* [Input file location](#Input-file-location)
* [Output file location](#Output-file-location)
* [Choice of input file language](#Choice-of-input-file-language)
* [Input file interpretation](#Input-file-interpretation)
* [Operation mode](#Operation-mode)
* [Language specific options](#Language-specific-options)
* [Options for XML input files](#Options-for-XML-input-files)
* [Output details](#Output-details)
* [Informative output](#Informative-output)
* [Example](#Example)

#### 5.1.1 Input file location [¶](#Input-file-location)

‘`inputfile …`’
:   Input files.

‘`-f file`’ [¶](#index-_002df_002c-xgettext-option)

‘`--files-from=file`’
:   Read the names of the input files from file instead of getting
    them from the command line.

    Often file is a temporary file, generated during the build process.
    In this case, you should also pass the `--generated=file` option.

‘`-D directory`’ [¶](#index-_002dD_002c-xgettext-option)

‘`--directory=directory`’
:   Add directory to the list of directories. Source files are
    searched relative to this list of directories. The resulting `.po`
    file will be written relative to the current directory, though.

If inputfile is ‘`-`’, standard input is read.

#### 5.1.2 Output file location [¶](#Output-file-location)

‘`-d name`’ [¶](#index-_002dd_002c-xgettext-option)

‘`--default-domain=name`’
:   Use `name.po` for output (instead of `messages.po`).

‘`-o file`’ [¶](#index-_002do_002c-xgettext-option)

‘`--output=file`’
:   Write output to specified file (instead of `name.po` or
    `messages.po`).

‘`-p dir`’ [¶](#index-_002dp_002c-xgettext-option)

‘`--output-dir=dir`’
:   Output files will be placed in directory dir.

If the output file is ‘`-`’ or ‘`/dev/stdout`’, the output
is written to standard output.

#### 5.1.3 Choice of input file language [¶](#Choice-of-input-file-language)

‘`-L name`’ [¶](#index-_002dL_002c-xgettext-option)

‘`--language=name`’
:   Specifies the language of the input files. The supported languages are
    `C`, `C++`, `ObjectiveC`,
    `PO`,
    `Python`,
    `Java`, `JavaProperties`,
    `C#`,
    `JavaScript`,
    `TypeScript`, `TSX`,
    `Scheme`, `Guile`,
    `Lisp`,
    `EmacsLisp`,
    `librep`,
    `Rust`,
    `Go`,
    `Ruby`,
    `Shell`,
    `awk`,
    `Lua`,
    `Modula-2`,
    `D`,
    `OCaml`,
    `Smalltalk`,
    `Vala`,
    `Tcl`,
    `Perl`,
    `PHP`,
    `GCC-source`,
    `YCP`,
    `NXStringTable`,
    `RST`, `RSJ`,
    `Glade`,
    `GSettings`,
    `Desktop`.

‘`-C`’ [¶](#index-_002dC_002c-xgettext-option)

‘`--c++`’
:   This is a shorthand for `--language=C++`.

By default the language is guessed depending on the input file name
extension.

#### 5.1.4 Input file interpretation [¶](#Input-file-interpretation)

‘`--from-code=name`’ [¶](#index-_002d_002dfrom_002dcode_002c-xgettext-option)
:   Specifies the encoding of the input files. This option is needed only
    if some untranslated message strings or their corresponding comments
    contain non-ASCII characters. Note that Tcl and Glade input files are
    always assumed to be in UTF-8, regardless of this option.

By default the input files are assumed to be in ASCII.

#### 5.1.5 Operation mode [¶](#Operation-mode)

‘`-j`’ [¶](#index-_002dj_002c-xgettext-option)

‘`--join-existing`’
:   Join messages with existing file.

‘`-x file`’ [¶](#index-_002dx_002c-xgettext-option)

‘`--exclude-file=file`’
:   Entries from file are not extracted. file should be a PO or
    POT file.

‘`-c[tag]`’ [¶](#index-_002dc_002c-xgettext-option)

‘`--add-comments[=tag]`’
:   Place comment blocks starting with tag and preceding keyword lines
    in the output file. Without a tag, the option means to put *all*
    comment blocks preceding keyword lines in the output file.

    Note that comment blocks are only extracted if there is no program code
    between the comment and the string that gets extracted.
    For example, in the following C source code:

    ```
    /* This is the first comment.  */
    gettext ("foo");

    /* This is the second comment: not extracted  */
    gettext (
      "bar");

    gettext (
      /* This is the third comment.  */
      "baz");

    /* This is the fourth comment.  */

    gettext ("I love blank lines in my programs");
    ```

    the second comment line will not be extracted, because there is a line
    with some tokens between the comment line and the line that contains
    the string. But the fourth comment is extracted, because between it and
    the line with the string there is merely a blank line.

‘`--check[=CHECK]`’ [¶](#index-_002d_002dcheck_002c-xgettext-option)
:   Perform a syntax check on msgid and msgid\_plural.
    The supported checks that are disabled by default and that can be enabled are:

    ‘`ellipsis-unicode`’
    :   Prefer Unicode ellipsis character over ASCII `...`

    ‘`space-ellipsis`’
    :   Prohibit whitespace before an ellipsis character

    ‘`quote-unicode`’
    :   Prefer Unicode quotation marks over ASCII `` "'` ``

    ‘`bullet-unicode`’
    :   Prefer Unicode bullet character over ASCII `*` or `-`

    The option has an effect on all input files. To enable or disable
    checks for a certain string, you can mark it with an `xgettext:`
    special comment in the source file. For example, if you specify the
    `--check=space-ellipsis` option, but want to suppress the check on
    a particular string, add the following comment:

    ```
    /* xgettext: no-space-ellipsis-check */
    gettext ("We really want a space before ellipsis here ...");
    ```

    The `xgettext:` comment can be followed by flags separated with a
    comma. The possible flags are of the form ‘`[no-]name-check`’,
    where name is the name of a valid syntax check. If a flag is
    prefixed by `no-`, the meaning is negated.

    Some tests apply the checks to each sentence within the msgid, rather
    than the whole string. `xgettext` detects the end of sentence by
    performing a pattern match, which usually looks for a period followed by
    a certain number of spaces. The number is specified with the
    `--sentence-end` option.

‘`--no-check[=CHECK]`’ [¶](#index-_002d_002dno_002dcheck_002c-xgettext-option)
:   Don’t perform a syntax check on msgid and msgid\_plural
    that is enabled by default.
    The supported checks that are enabled by default are:

    ‘`url`’
    :   Prohibit a URL inside a string.

    ‘`email`’
    :   Prohibit an email address inside a string.

    The option has an effect on all input files.
    To disable a check for a certain string,
    you can mark it with an `xgettext:` special comment in the source file.
    For example, if you want to suppress the check on a particular string,
    add the following comment:

    ```
    /* xgettext: no-email-check */
    gettext ("Specify your@email-address.com here.");
    ```

‘`--sentence-end[=TYPE]`’ [¶](#index-_002d_002dsentence_002dend_002c-xgettext-option)
:   The supported values are:

    ‘`single-space`’
    :   Expect at least one whitespace after a period

    ‘`double-space`’
    :   Expect at least two whitespaces after a period

#### 5.1.6 Language specific options [¶](#Language-specific-options)

‘`-a`’ [¶](#index-_002da_002c-xgettext-option)

‘`--extract-all`’
:   Extract all strings.

    This option has an effect with most languages, namely
    C, C++, ObjectiveC,
    Python,
    Java,
    C#,
    JavaScript,
    Scheme, Guile,
    Lisp,
    EmacsLisp,
    librep,
    Rust,
    Go,
    Shell,
    awk,
    Lua,
    Modula-2,
    D,
    OCaml,
    Vala,
    Tcl,
    Perl,
    PHP,
    GCC-source,
    Glade,
    GSettings.

‘`-k[keywordspec]`’ [¶](#index-_002dk_002c-xgettext-option)

‘`--keyword[=keywordspec]`’
:   Specify keywordspec as an additional keyword to be looked for.
    Without a keywordspec, the option means to not use default keywords.

    If keywordspec is a C identifier id, `xgettext` looks
    for strings in the first argument of each call to the function or macro
    id. If keywordspec is of the form
    ‘`id:argnum`’, `xgettext` looks for strings in the
    argnumth argument of the call. If keywordspec is of the form
    ‘`id:argnum1,argnum2`’, `xgettext` looks for
    strings in the argnum1st argument and in the argnum2nd argument
    of the call, and treats them as singular/plural variants for a message
    with plural handling. Also, if keywordspec is of the form
    ‘`id:contextargnumc,argnum`’ or
    ‘`id:argnum,contextargnumc`’, `xgettext` treats
    strings in the contextargnumth argument as a context specifier.
    And, as a special-purpose support for GNOME, if keywordspec is of the
    form ‘`id:argnumg`’, `xgettext` recognizes the
    argnumth argument as a string with context, using the GNOME `glib`
    syntax ‘`"msgctxt|msgid"`’.
      
    Furthermore, if keywordspec is of the form
    ‘`id:…,totalnumargst`’, `xgettext` recognizes this
    argument specification only if the number of actual arguments is equal to
    totalnumargs. This is useful for disambiguating overloaded function
    calls in C++.
      
    Finally, if keywordspec is of the form
    ‘`id:argnum...,"xcomment"`’, `xgettext`, when
    extracting a message from the specified argument strings, adds an extracted
    comment xcomment to the message. Note that when used through a normal
    shell command line, the double-quotes around the xcomment need to be
    escaped.

    This option has an effect with most languages, namely
    C, C++, ObjectiveC,
    Python,
    Java,
    C#,
    JavaScript,
    TypeScript, TSX,
    Scheme, Guile,
    Lisp,
    EmacsLisp,
    librep,
    Rust,
    Go,
    Shell,
    awk,
    Lua,
    Modula-2,
    D,
    OCaml,
    Vala,
    Tcl,
    Perl,
    PHP,
    GCC-source,
    Glade,
    GSettings,
    Desktop.

    The default keyword specifications, which are always looked for if not
    explicitly disabled, are language dependent. They are:

    * For C, C++, and GCC-source: `gettext`, `dgettext:2`,
      `dcgettext:2`, `ngettext:1,2`, `dngettext:2,3`,
      `dcngettext:2,3`, `gettext_noop`, and `pgettext:1c,2`,
      `dpgettext:2c,3`, `dcpgettext:2c,3`, `npgettext:1c,2,3`,
      `dnpgettext:2c,3,4`, `dcnpgettext:2c,3,4`.
    * For Objective C: Like for C, and also `NSLocalizedString`, `_`,
      `NSLocalizedStaticString`, `__`.
    * For Shell scripts:
      `gettext`, `ngettext:1,2`,
      `printf_gettext`, `printf_ngettext:1,2`,
      `eval_gettext`, `eval_ngettext:1,2`,
      `eval_pgettext:1c,2`, `eval_npgettext:1c,2,3`.
    * For Python: `gettext`, `ugettext`, `dgettext:2`,
      `ngettext:1,2`, `ungettext:1,2`, `dngettext:2,3`, `_`.
    * For Lisp: `gettext`, `ngettext:1,2`, `gettext-noop`.
    * For EmacsLisp: `_`.
    * For librep: `_`.
    * For Scheme and Guile: `gettext`, `ngettext:1,2`, `gettext-noop`.
    * For Java: `GettextResource.gettext:2`,
      `GettextResource.ngettext:2,3`, `GettextResource.pgettext:2c,3`,
      `GettextResource.npgettext:2c,3,4`, `gettext`, `ngettext:1,2`,
      `pgettext:1c,2`, `npgettext:1c,2,3`, `getString`.
    * For C#: `GetString`, `GetPluralString:1,2`,
      `GetParticularString:1c,2`, `GetParticularPluralString:1c,2,3`.
    * For awk: `dcgettext`, `dcngettext:1,2`.
    * For Tcl: `::msgcat::mc`.
    * For Perl: `gettext`, `%gettext`, `$gettext`, `dgettext:2`,
      `dcgettext:2`, `ngettext:1,2`, `dngettext:2,3`,
      `dcngettext:2,3`, `gettext_noop`.
    * For PHP: `_`, `gettext`, `dgettext:2`, `dcgettext:2`,
      `ngettext:1,2`, `dngettext:2,3`, `dcngettext:2,3`.
    * For Glade 1: `label`, `title`, `text`, `format`,
      `copyright`, `comments`, `preview_text`, `tooltip`.
    * For Lua: `_`, `gettext.gettext`, `gettext.dgettext:2`,
      `gettext.dcgettext:2`, `gettext.ngettext:1,2`,
      `gettext.dngettext:2,3`, `gettext.dcngettext:2,3`.
    * For D: `gettext`, `dgettext:2`, `dcgettext:2`,
      `ngettext:1,2`, `dngettext:2,3`, `dcngettext:2,3`.
    * For JavaScript, TypeScript, TSX:
      `_`, `gettext`, `dgettext:2`,
      `dcgettext:2`, `ngettext:1,2`, `dngettext:2,3`,
      `pgettext:1c,2`, `dpgettext:2c,3`.
    * For Vala: `_`, `Q_`, `N_`, `NC_`, `dgettext:2`,
      `dcgettext:2`, `ngettext:1,2`, `dngettext:2,3`,
      `dpgettext:2c,3`, `dpgettext2:2c,3`.
    * For Desktop: `Name`, `GenericName`, `Comment`,
      `Keywords`.

    To disable the default keyword specifications, the option ‘`-k`’ or
    ‘`--keyword`’ or ‘`--keyword=`’, without a keywordspec, can be
    used.

‘`--flag=word:arg:flag`’ [¶](#index-_002d_002dflag_002c-xgettext-option)
:   Specifies additional flags for strings occurring as part of the argth
    argument of the function word. The possible flags are the possible
    format string indicators, such as ‘`c-format`’, and their negations,
    such as ‘`no-c-format`’, possibly prefixed with ‘`pass-`’.
      

    The meaning of `--flag=function:arg:lang-format`
    is that in language lang, the specified function expects as
    argth argument a format string. (For those of you familiar with
    GCC function attributes, `--flag=function:arg:c-format` is
    roughly equivalent to the declaration
    ‘`__attribute__ ((__format__ (__printf__, arg, ...)))`’ attached
    to function in a C source file.)
    For example, if you use the ‘`error`’ function from GNU libc, you can
    specify its behaviour through `--flag=error:3:c-format`. The effect of
    this specification is that `xgettext` will mark as format strings all
    `gettext` invocations that occur as argth argument of
    function.
    This is useful when such strings contain no format string directives:
    together with the checks done by ‘`msgfmt -c`’ it will ensure that
    translators cannot accidentally use format string directives that would
    lead to a crash at runtime.
      

    The meaning of `--flag=function:arg:pass-lang-format`
    is that in language lang, if the function call occurs in a
    position that must yield a format string, then its argth argument
    must yield a format string of the same type as well. (If you know GCC
    function attributes, the `--flag=function:arg:pass-c-format`
    option is roughly equivalent to the declaration
    ‘`__attribute__ ((__format_arg__ (arg)))`’ attached to function
    in a C source file.)
    For example, if you use the ‘`_`’ shortcut for the `gettext` function,
    you should use `--flag=_:1:pass-c-format`. The effect of this
    specification is that `xgettext` will propagate a format string
    requirement for a `_("string")` call to its first argument, the literal
    `"string"`, and thus mark it as a format string.
    This is useful when such strings contain no format string directives:
    together with the checks done by ‘`msgfmt -c`’ it will ensure that
    translators cannot accidentally use format string directives that would
    lead to a crash at runtime.
      
    This option has an effect with most languages, namely
    C, C++, ObjectiveC,
    Python,
    Java,
    C#,
    JavaScript,
    TypeScript, TSX,
    Scheme, Guile,
    Lisp,
    EmacsLisp,
    librep,
    Rust,
    Go,
    Shell,
    awk,
    Lua,
    Modula-2,
    D,
    OCaml,
    Vala,
    Tcl,
    Perl,
    PHP,
    GCC-source,
    YCP.

‘`--tag=word:format`’ [¶](#index-_002d_002dtag_002c-xgettext-option)
:   Defines the behaviour of tagged template literals with tag word.
    This option has an effect only with language JavaScript.
      
    format is a symbolic description
    of the first step of the JavaScript function named word,
    namely how this function constructs a format string
    based on the parts of the template literal.
    Currently only one value is supported: `javascript-gnome-format`,
    which describes the construction of a format string with numbered placeholders
    `{0}`, `{1}`, `{2}`, etc.
    For example, `javascript-gnome-format` transforms the template literal
    `` word`My name is ${id.name} and I am ${id.age} years old.` ``
    into the format string `"My name is {0} and I am {1} years old."`.

‘`-T`’ [¶](#index-_002dT_002c-xgettext-option)

‘`--trigraphs`’
:   Understand ANSI C trigraphs for input
    (deprecated, since trigraphs have been removed from ISO C 23).
      
    This option has an effect only with the languages C, C++, ObjectiveC.

‘`--qt`’ [¶](#index-_002d_002dqt_002c-xgettext-option)
:   Recognize Qt format strings.
      
    This option has an effect only with the language C++.

‘`--kde`’ [¶](#index-_002d_002dkde_002c-xgettext-option)
:   Recognize KDE 4 format strings.
      
    This option has an effect only with the language C++.

‘`--boost`’ [¶](#index-_002d_002dboost_002c-xgettext-option)
:   Recognize Boost format strings.
      
    This option has an effect only with the language C++.

‘`--debug`’ [¶](#index-_002d_002ddebug_002c-xgettext-option)
:   Use the flags `c-format` and `possible-c-format` to show who was
    responsible for marking a message as a format string. The latter form is
    used if the `xgettext` program decided, the former form is used if
    the programmer prescribed it.

    By default only the `c-format` form is used. The translator should
    not have to care about these details.

This implementation of `xgettext` is able to process a few awkward
cases, like strings in preprocessor macros, ANSI concatenation of
adjacent strings, and escaped end of lines for continued strings.

#### 5.1.7 Options for XML input files [¶](#Options-for-XML-input-files)

When some of the input files are XML files
and they are not of one of the types covered
by the system-wide installed \*.its files,
a \*.its file is needed for each such file type,
so that `xgettext` can handle them.
There are two ways to specify such a file:

* ‘`--its=file`’ [¶](#index-_002d_002dits_002c-xgettext-option)
  :   Use the ITS rules defined in file.
* The environment variable `GETTEXTDATADIRS`.
  Together with the \*.its file, you need a corresponding \*.loc file
  (see [Preparing Rules for XML Internationalization](#Preparing-ITS-Rules)).
  Furthermore you need to store these files
  in a directory `parent_dir/its/`

  and set the environment variable `GETTEXTDATADIRS` to include
  `parent_dir`.
  More generally, the value of `GETTEXTDATADIRS` should be
  a colon-separated list of directory names.

Note that when the option `--its` is specified,
the system-wide installed \*.its files are ignored
and the environment variable `GETTEXTDATADIRS` has no effect either.

#### 5.1.8 Output details [¶](#Output-details)

‘`--color`’ [¶](#index-_002d_002dcolor_002c-xgettext-option)

‘`--color=when`’
:   Specify whether or when to use colors and other text attributes.
    See [The `--color` option](#The-_002d_002dcolor-option) for details.

‘`--style=style_file`’ [¶](#index-_002d_002dstyle_002c-xgettext-option)
:   Specify the CSS style rule file to use for `--color`.
    See [The `--style` option](#The-_002d_002dstyle-option) for details.

‘`--force-po`’ [¶](#index-_002d_002dforce_002dpo_002c-xgettext-option)
:   Always write an output file even if no message is defined.

‘`-i`’ [¶](#index-_002di_002c-xgettext-option)

‘`--indent`’
:   Write the .po file using indented style.

‘`--no-location`’ [¶](#index-_002d_002dno_002dlocation_002c-xgettext-option)
:   Do not write ‘`#: filename:line`’ lines. Note that using
    this option makes it harder for technically skilled translators to understand
    each message’s context.

‘`-n`’ [¶](#index-_002dn_002c-xgettext-option)

‘`--add-location=type`’
:   Generate ‘`#: filename:line`’ lines (default).

    The optional type can be either ‘`full`’, ‘`file`’, or
    ‘`never`’. If it is not given or ‘`full`’, it generates the
    lines with both file name and line number. If it is ‘`file`’, the
    line number part is omitted. If it is ‘`never`’, it completely
    suppresses the lines (same as `--no-location`).

‘`--strict`’ [¶](#index-_002d_002dstrict_002c-xgettext-option)
:   Write out a strict Uniforum conforming PO file. Note that this
    Uniforum format should be avoided because it doesn’t support the
    GNU extensions.

‘`--properties-output`’ [¶](#index-_002d_002dproperties_002doutput_002c-xgettext-option)
:   Write out a Java ResourceBundle in Java `.properties` syntax. Note
    that this file format doesn’t support plural forms and silently drops
    obsolete messages.

‘`--stringtable-output`’ [¶](#index-_002d_002dstringtable_002doutput_002c-xgettext-option)
:   Write out a NeXTstep/GNUstep localized resource file in `.strings` syntax.
    Note that this file format doesn’t support plural forms.

‘`--itstool`’ [¶](#index-_002d_002ditstool_002c-xgettext-option)
:   Write out comments recognized by itstool (<http://itstool.org>).
    Note that this is only effective with XML files.

‘`-w number`’ [¶](#index-_002dw_002c-xgettext-option)

‘`--width=number`’
:   Set the output page width. Long strings in the output files will be
    split across multiple lines in order to ensure that each line’s width
    (= number of screen columns) is less or equal to the given number.

‘`--no-wrap`’ [¶](#index-_002d_002dno_002dwrap_002c-xgettext-option)
:   Do not break long message lines. Message lines whose width exceeds the
    output page width will not be split into several lines. Only file reference
    lines which are wider than the output page width will be split.

‘`-s`’ [¶](#index-_002ds_002c-xgettext-option)

‘`--sort-output`’
:   Generate sorted output (deprecated). Note that using this option makes it
    much harder for the translator to understand each message’s context.

‘`-F`’ [¶](#index-_002dF_002c-xgettext-option)

‘`--sort-by-file`’
:   Sort output by file location.

‘`--omit-header`’ [¶](#index-_002d_002domit_002dheader_002c-xgettext-option)
:   Don’t write header with ‘`msgid ""`’ entry.
    Note: Using this option may lead to an error in subsequent operations
    if the output contains non-ASCII characters.

    This is useful for testing purposes because it eliminates a source
    of variance for generated `.gmo` files. With `--omit-header`,
    two invocations of `xgettext` on the same files with the same
    options at different times are guaranteed to produce the same results.

    Note that using this option will lead to an error if the resulting file
    would not entirely be in ASCII.

‘`--no-git`’ [¶](#index-_002d_002dno_002dgit_002c-xgettext-option)
:   Don’t use the `git` program
    to produce a reproducible ‘`POT-Creation-Date`’ field in the output.

    Use this option, for speed, if
    your project has a very long `Git` history
    (hundreds of thousands of commits)
    or you are specifying thousands of input files.

    By default, `xgettext` determines the ‘`POT-Creation-Date`’ as
    the maximum version-controlled modification time
    among all the given input files.
    With this option, you can specify that it should instead use
    the maximum modification time (time stamp on disk)
    among all the given input files.

    By “version control”, here we mean the `Git` version control system.

‘`--copyright-holder=string`’ [¶](#index-_002d_002dcopyright_002dholder_002c-xgettext-option)
:   Set the copyright holder in the output. string should be the
    copyright holder of the surrounding package. (Note that the `msgid`
    strings, extracted from the package’s sources, belong to the copyright
    holder of the package.) Translators are expected to transfer or disclaim
    the copyright for their translations, so that package maintainers can
    distribute them without legal risk. If string is empty, the output
    files are marked as being in the public domain; in this case, the translators
    are expected to disclaim their copyright, again so that package maintainers
    can distribute them without legal risk.

    The default value for string is the Free Software Foundation, Inc.,
    simply because `xgettext` was first used in the GNU project.

‘`--foreign-user`’ [¶](#index-_002d_002dforeign_002duser_002c-xgettext-option)
:   Omit FSF copyright in output. This option is equivalent to
    ‘`--copyright-holder=''`’. It can be useful for packages outside the GNU
    project that want their translations to be in the public domain.

‘`--package-name=package`’ [¶](#index-_002d_002dpackage_002dname_002c-xgettext-option)
:   Set the package name in the header of the output.

‘`--package-version=version`’ [¶](#index-_002d_002dpackage_002dversion_002c-xgettext-option)
:   Set the package version in the header of the output. This option has an
    effect only if the ‘`--package-name`’ option is also used.

‘`--msgid-bugs-address=email@address`’ [¶](#index-_002d_002dmsgid_002dbugs_002daddress_002c-xgettext-option)
:   Set the reporting address for msgid bugs. This is the email address or URL
    to which the translators shall report bugs in the untranslated strings:

    * Strings which are not entire sentences; see the maintainer guidelines
      in [Preparing Translatable Strings](#Preparing-Strings).
    * Strings which use unclear terms or require additional context to be
      understood.
    * Strings which make invalid assumptions about notation of date, time or
      money.
    * Pluralisation problems.
    * Incorrect English spelling.
    * Incorrect formatting.

    It can be your email address, or a mailing list address where translators
    can write to without being subscribed, or the URL of a web page through
    which the translators can contact you.

    The default value is empty, which means that translators will be clueless!
    Don’t forget to specify this option.

‘`--generated=file`’ [¶](#index-_002d_002dgenerated_002c-xgettext-option)
:   Declares that the given file is generated
    and therefore should not have
    an influence on the ‘`POT-Creation-Date`’ field in the output.

    When you specify this option,
    you should also specify one or more `--reference` options,
    to indicate the files from which the given file was generated.

‘`--reference=file`’ [¶](#index-_002d_002dreference_002c-xgettext-option)
:   Declares that the output depends on the contents of the given file.
    This has an influence on the ‘`POT-Creation-Date`’ field in the output.

    By default, `xgettext` determines the ‘`POT-Creation-Date`’ as
    the maximum version-controlled modification time
    among all the given input files.
    With this option, you can specify that
    the output depends also on some other files.
    For example, use this option when
    some of the input files is not under version control
    but instead is generated from one or more files that are under version control.

    By “version control”, here we mean the `Git` version control system.

‘`-m[string]`’ [¶](#index-_002dm_002c-xgettext-option)

‘`--msgstr-prefix[=string]`’
:   Use string (or "" if not specified) as prefix for msgstr values.

‘`-M[string]`’ [¶](#index-_002dM_002c-xgettext-option)

‘`--msgstr-suffix[=string]`’
:   Use string (or "" if not specified) as suffix for msgstr values.

#### 5.1.9 Informative output [¶](#Informative-output)

‘`-h`’ [¶](#index-_002dh_002c-xgettext-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-xgettext-option)

‘`--version`’
:   Output version information and exit.

‘`-v`’ [¶](#index-_002dv_002c-xgettext-option)

‘`--verbose`’
:   Increase verbosity level.

#### 5.1.10 Example [¶](#Example)

A sample invocation of `xgettext`, in a project
that has a single source file `src/hello.c`
that uses ‘`_`’ as shorthand for the `gettext` function,
could be:

```
xgettext -o hello.pot \
         --add-comments=TRANSLATORS: \
         --keyword=_ --flag=_:1:pass-c-format \
         --directory=.. \
         src/hello.c
```

---

Previous: [Invoking the `xgettext` Program](#xgettext-Invocation), Up: [Making the PO Template File](#Template)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 5.2 Combining PO Template Files [¶](#Combining-PO-Template-Files)

When a package contains sources in different programming languages and
different, incompatible `xgettext` command line options are required
for these different parts of the package, the solution is to create
intermediate PO template files for each of the parts and then combine (merge)
them together.

For example, assume you have two source files `a.c` and `b.py`,
and want to extract their translatable strings in separate steps.

Each of the following command sequences does this. The output is the same.

* This command sequence creates intermediate POT files and then combines them.

  ```
  xgettext -o part-c.pot a.c
  xgettext -o part-py.pot b.py
  xgettext -o all.pot part-c.pot part-py.pot
  ```
* This command sequence does several `xgettext` invocations, with a
  single POT file that accumulates the translatable strings.

  ```
  xgettext -o all.pot a.c
  xgettext -o all.pot --join-existing b.py
  ```
* Likewise here, but a ‘`--default-domain`’ option is used to denote
  the output file rather than a ‘`-o`’ option.

  ```
  xgettext --default-domain=all a.c
  xgettext --default-domain=all --join-existing b.py
  mv all.po all.pot
  ```

One might be tempted to think that ‘`msgcat`’ can do the same thing,
through a command sequence such as:

```
xgettext -o part-c.pot a.c
xgettext -o part-py.pot b.py
msgcat -o all.pot part-c.pot part-py.pot
```

But no, this does not work reliably, because sometimes `part-c.pot`
and `part-py.pot` will contain different `POT-Creation-Date`
values, and `msgcat` then produces an `all.pot` file that has
conflict markers in the header entry.
This is because `msgcat` generally is meant to produce PO files that
are to be reviewed and edited by a translator; this is not desired here.

---

Next: [Updating Existing PO Files](#Updating), Previous: [Making the PO Template File](#Template), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 6 Creating or preparing for translating a PO file [¶](#Creating-or-preparing-for-translating-a-PO-file)

When starting a new translation, the translator creates a file called
`LANG.po`, as a copy of the `package.pot` template
file with modifications in the initial comments (at the beginning of the file)
and in the header entry (the first entry, near the beginning of the file).

Before continuing an existing translation,
after a new release of the package package was made,
the translator updates the file called `LANG.po`,
with respect to the new `package.pot` template file,
and adds her name in the `Last-Translator` field of the header entry.

The easiest way to do so, in either case,
is by use of the ‘`msginit`’ program.
For example:

```
$ cd PACKAGE-VERSION
$ cd po
$ msginit
```

The alternative way, without `msginit`, is as follows:

* When starting a new translation,
  the translator does the copy and modifications by hand.
  She copies `package.pot` to `LANG.po`.
  Then she modifies the initial comments and the header entry of this file.
* When continuing an existing translation,
  the translator invokes
  `msgmerge --previous -o lang.new.po lang.po package.pot`
  and `mv lang.new.po lang.po`.

* [Invoking the `msginit` Program](#msginit-Invocation)
* [Filling in the Header Entry](#Header-Entry)

---

Next: [Filling in the Header Entry](#Header-Entry), Up: [Creating or preparing for translating a PO file](#Creating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 6.1 Invoking the `msginit` Program [¶](#Invoking-the-msginit-Program)

```
msginit [option]
```

The `msginit` program creates a new PO file, initializing the meta
information with values from the user’s environment.

Here are more details. The following header fields of a PO file are
automatically filled, when possible.

‘`Project-Id-Version`’
:   The value is guessed from the `configure` script or any other files
    in the current directory.

‘`PO-Revision-Date`’
:   The value is taken from the `PO-Creation-Data` in the input POT
    file, or the current date is used.

‘`Last-Translator`’
:   The value is taken from user’s password file entry and the mailer
    configuration files.

‘`Language-Team, Language`’
:   These values are set according to the current locale and the predefined
    list of translation teams.

‘`MIME-Version, Content-Type, Content-Transfer-Encoding`’
:   These values are set according to the content of the POT file and the
    current locale. If the POT file contains charset=UTF-8, it means that
    the POT file contains non-ASCII characters, and we keep the UTF-8
    encoding. Otherwise, when the POT file is plain ASCII, we use the
    locale’s encoding.

‘`Plural-Forms`’
:   The value is first looked up from the embedded table.

    As an experimental feature, you can instruct `msginit` to use the
    information from Unicode CLDR, by setting the `GETTEXTCLDRDIR`
    environment variable. The program will look for a file named
    `common/supplemental/plurals.xml` under that directory. You can
    get the CLDR data from <http://cldr.unicode.org/>.

* [Input file location](#Input-file-location-1)
* [Output file location](#Output-file-location-1)
* [Input file syntax](#Input-file-syntax)
* [Output details](#Output-details-1)
* [Informative output](#Informative-output-1)

#### 6.1.1 Input file location [¶](#Input-file-location-1)

‘`-i inputfile`’ [¶](#index-_002di_002c-msginit-option)

‘`--input=inputfile`’
:   Input POT file.

If no inputfile is given, the current directory is searched for the
POT file. If it is ‘`-`’, standard input is read.

#### 6.1.2 Output file location [¶](#Output-file-location-1)

‘`-o file`’ [¶](#index-_002do_002c-msginit-option)

‘`--output-file=file`’
:   Write output to specified PO file.

If no output file is given, it depends on the ‘`--locale`’ option or the
user’s locale setting.

If the output file already exists, it is merged with the input file,
as if through `msgmerge`.

If it is ‘`-`’, the results are written to standard output.

#### 6.1.3 Input file syntax [¶](#Input-file-syntax)

‘`-P`’ [¶](#index-_002dP_002c-msginit-option)

‘`--properties-input`’
:   Assume the input file is a Java ResourceBundle in Java `.properties`
    syntax, not in PO file syntax.

‘`--stringtable-input`’ [¶](#index-_002d_002dstringtable_002dinput_002c-msginit-option)
:   Assume the input file is a NeXTstep/GNUstep localized resource file in
    `.strings` syntax, not in PO file syntax.

#### 6.1.4 Output details [¶](#Output-details-1)

‘`-l ll_CC[.encoding]`’ [¶](#index-_002dl_002c-msginit-option)

‘`--locale=ll_CC[.encoding]`’
:   Set target locale. ll should be a language code, and CC should
    be a country code. The optional part .encoding specifies the encoding
    of the locale; most often this part is `.UTF-8`.
    The command ‘`locale -a`’ can be used to output a list
    of all installed locales. The default is the user’s locale setting.

‘`--no-translator`’ [¶](#index-_002d_002dno_002dtranslator_002c-msginit-option)
:   Declares that the PO file will not have a human translator and is instead
    automatically generated.

‘`--color`’ [¶](#index-_002d_002dcolor_002c-msginit-option)

‘`--color=when`’
:   Specify whether or when to use colors and other text attributes.
    See [The `--color` option](#The-_002d_002dcolor-option) for details.

‘`--style=style_file`’ [¶](#index-_002d_002dstyle_002c-msginit-option)
:   Specify the CSS style rule file to use for `--color`.
    See [The `--style` option](#The-_002d_002dstyle-option) for details.

‘`-p`’ [¶](#index-_002dp_002c-msginit-option)

‘`--properties-output`’
:   Write out a Java ResourceBundle in Java `.properties` syntax. Note
    that this file format doesn’t support plural forms and silently drops
    obsolete messages.

‘`--stringtable-output`’ [¶](#index-_002d_002dstringtable_002doutput_002c-msginit-option)
:   Write out a NeXTstep/GNUstep localized resource file in `.strings` syntax.
    Note that this file format doesn’t support plural forms.

‘`-w number`’ [¶](#index-_002dw_002c-msginit-option)

‘`--width=number`’
:   Set the output page width. Long strings in the output files will be
    split across multiple lines in order to ensure that each line’s width
    (= number of screen columns) is less or equal to the given number.

‘`--no-wrap`’ [¶](#index-_002d_002dno_002dwrap_002c-msginit-option)
:   Do not break long message lines. Message lines whose width exceeds the
    output page width will not be split into several lines. Only file reference
    lines which are wider than the output page width will be split.

#### 6.1.5 Informative output [¶](#Informative-output-1)

‘`-h`’ [¶](#index-_002dh_002c-msginit-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-msginit-option)

‘`--version`’
:   Output version information and exit.

---

Previous: [Invoking the `msginit` Program](#msginit-Invocation), Up: [Creating or preparing for translating a PO file](#Creating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 6.2 Filling in the Header Entry [¶](#Filling-in-the-Header-Entry)

The initial comments "SOME DESCRIPTIVE TITLE", "YEAR" and
"FIRST AUTHOR <EMAIL@ADDRESS>, YEAR" ought to be replaced by sensible
information. This can be done in any text editor; if Emacs is used
and it switched to PO mode automatically (because it has recognized
the file’s suffix), you can disable it by typing `M-x fundamental-mode`.

Modifying the header entry can already be done using PO mode: in Emacs,
type `M-x po-mode RET` and then `RET` again to start editing the
entry. You should fill in the following fields.

Project-Id-Version
:   This is the name and version of the package. Fill it in if it has not
    already been filled in by `xgettext`.

Report-Msgid-Bugs-To
:   This has already been filled in by `xgettext`. It contains an email
    address or URL where you can report bugs in the untranslated strings:

    * Strings which are not entire sentences, see the maintainer guidelines
      in [Preparing Translatable Strings](#Preparing-Strings).
    * Strings which use unclear terms or require additional context to be
      understood.
    * Strings which make invalid assumptions about notation of date, time or
      money.
    * Pluralisation problems.
    * Incorrect English spelling.
    * Incorrect formatting.

POT-Creation-Date
:   This has already been filled in by `xgettext`.

PO-Revision-Date
:   You don’t need to fill this in. It will be filled by the PO file editor
    when you save the file.

Last-Translator
:   Fill in your name and email address (without double quotes).

Language-Team
:   Fill in the English name of the language, and the email address or
    homepage URL of the language team you are part of.

    Before starting a translation, it is a good idea to get in touch with
    your translation team, not only to make sure you don’t do duplicated work,
    but also to coordinate difficult linguistic issues.

    In the Free Translation Project, each translation team has its own mailing
    list. The up-to-date list of teams can be found at the Free Translation
    Project’s homepage, <https://translationproject.org/>, in the "Teams"
    area.

Language
:   Fill in the language code of the language. This can be in one of three
    forms:

    * ‘`ll`’, an ISO 639 two-letter language code (lowercase).
      For some languages,
      a two-letter code does not exist, and a three-letter code is used instead.
      See [Language Codes](#Language-Codes) for the list of codes.
    * ‘`ll_CC`’, where ‘`ll`’ is an ISO 639 two-letter
      or three-letter
      language code (lowercase) and ‘`CC`’ is an ISO 3166 two-letter
      country code (uppercase). The country code specification is not redundant:
      Some languages have dialects in different countries. For example,
      ‘`de_AT`’ is used for Austria, and ‘`pt_BR`’ for Brazil. The country
      code serves to distinguish the dialects. See [Language Codes](#Language-Codes) and
      [Country Codes](#Country-Codes) for the lists of codes.
    * ‘`ll_CC@variant`’, where ‘`ll`’ is an
      ISO 639 two-letter
      or three-letter
      language code (lowercase), ‘`CC`’ is an
      ISO 3166 two-letter country code (uppercase), and ‘`variant`’ is
      a variant designator. The variant designator (lowercase) can be a script
      designator, such as ‘`latin`’ or ‘`cyrillic`’.

    The naming convention ‘`ll_CC`’ is also the way locales are
    named on systems based on GNU libc. But there are three important differences:

    * In this PO file field, but not in locale names, ‘`ll_CC`’
      combinations denoting a language’s main dialect are abbreviated as
      ‘`ll`’. For example, ‘`de`’ is equivalent to ‘`de_DE`’
      (German as spoken in Germany), and ‘`pt`’ to ‘`pt_PT`’ (Portuguese as
      spoken in Portugal) in this context.
    * In this PO file field, suffixes like ‘`.encoding`’ are not used.
    * In this PO file field, variant designators that are not relevant to message
      translation, such as ‘`@euro`’, are not used.

    So, if your locale name is ‘`de_DE.UTF-8`’, the language specification in
    PO files is just ‘`de`’.

Content-Type [¶](#index-encoding-of-PO-files)
:   Replace ‘`CHARSET`’ with the character encoding used for your language,
    in your locale, or UTF-8. This field is needed for correct operation of the
    `msgmerge` and `msgfmt` programs, as well as for users whose
    locale’s character encoding differs from yours (see [How to specify the output character set `gettext` uses](#Charset-conversion)).

    You get the character encoding of your locale by running the shell command
    ‘`locale charmap`’. If the result is ‘`C`’ or ‘`ANSI_X3.4-1968`’,
    which is equivalent to ‘`ASCII`’ (= ‘`US-ASCII`’), it means that your
    locale is not correctly configured. In this case, ask your translation
    team which charset to use. ‘`ASCII`’ is not usable for any language
    except Latin.

    Because the PO files must be portable to operating systems with less advanced
    internationalization facilities, the character encodings that can be used
    are limited to those supported by both GNU `libc` and GNU
    `libiconv`. These are:
    `ASCII`, `ISO-8859-1`, `ISO-8859-2`, `ISO-8859-3`,
    `ISO-8859-4`, `ISO-8859-5`, `ISO-8859-6`, `ISO-8859-7`,
    `ISO-8859-8`, `ISO-8859-9`, `ISO-8859-13`, `ISO-8859-14`,
    `ISO-8859-15`,
    `KOI8-R`, `KOI8-U`, `KOI8-T`,
    `CP850`, `CP866`, `CP874`,
    `CP932`, `CP949`, `CP950`, `CP1250`, `CP1251`,
    `CP1252`, `CP1253`, `CP1254`, `CP1255`, `CP1256`,
    `CP1257`, `GB2312`, `EUC-JP`, `EUC-KR`, `EUC-TW`,
    `BIG5`, `BIG5-HKSCS`, `GBK`, `GB18030`, `SHIFT_JIS`,
    `JOHAB`, `TIS-620`, `VISCII`, `GEORGIAN-PS`, `UTF-8`.

    In the GNU system, the following encodings are frequently used for the
    corresponding languages.

    * `ISO-8859-1` for
      Afrikaans, Albanian, Basque, Breton, Catalan, Cornish, Danish, Dutch,
      English, Estonian, Faroese, Finnish, French, Galician, German,
      Greenlandic, Icelandic, Indonesian, Irish, Italian, Malay, Manx,
      Norwegian, Occitan, Portuguese, Spanish, Swedish, Tagalog, Uzbek,
      Walloon,
    * `ISO-8859-2` for
      Bosnian, Croatian, Czech, Hungarian, Polish, Romanian, Serbian, Slovak,
      Slovenian,
    * `ISO-8859-3` for Maltese,
    * `ISO-8859-5` for Macedonian, Serbian,
    * `ISO-8859-6` for Arabic,
    * `ISO-8859-7` for Greek,
    * `ISO-8859-8` for Hebrew,
    * `ISO-8859-9` for Turkish,
    * `ISO-8859-13` for Latvian, Lithuanian, Maori,
    * `ISO-8859-14` for Welsh,
    * `ISO-8859-15` for
      Basque, Catalan, Dutch, English, Finnish, French, Galician, German, Irish,
      Italian, Portuguese, Spanish, Swedish, Walloon,
    * `KOI8-R` for Russian,
    * `KOI8-U` for Ukrainian,
    * `KOI8-T` for Tajik,
    * `CP1251` for Bulgarian, Belarusian,
    * `GB2312`, `GBK`, `GB18030`
      for simplified writing of Chinese,
    * `BIG5`, `BIG5-HKSCS`
      for traditional writing of Chinese,
    * `EUC-JP` for Japanese,
    * `EUC-KR` for Korean,
    * `TIS-620` for Thai,
    * `GEORGIAN-PS` for Georgian,
    * `UTF-8` for any language, including those listed above.

    When single quote characters or double quote characters are used in
    translations for your language, and your locale’s encoding is one of the
    ISO-8859-\* charsets, it is best if you create your PO files in UTF-8
    encoding, instead of your locale’s encoding. This is because in UTF-8
    the real quote characters can be represented (single quote characters:
    U+2018, U+2019, double quote characters: U+201C, U+201D), whereas none of
    ISO-8859-\* charsets has them all. Users in UTF-8 locales will see the
    real quote characters, whereas users in ISO-8859-\* locales will see the
    vertical apostrophe and the vertical double quote instead (because that’s
    what the character set conversion will transliterate them to).

    To enter such quote characters under X11, you can change your keyboard
    mapping using the `xmodmap` program. The X11 names of the quote
    characters are "leftsinglequotemark", "rightsinglequotemark",
    "leftdoublequotemark", "rightdoublequotemark", "singlelowquotemark",
    "doublelowquotemark".

    The character encoding name can be written in either upper or lower case.
    Usually upper case is preferred.

Content-Transfer-Encoding
:   Set this to `8bit`.

Plural-Forms
:   This field is optional. It is only needed if the PO file has plural forms.
    You can find them by searching for the ‘`msgid_plural`’ keyword. The
    format of the plural forms field is described in [Additional functions for plural forms](#Plural-forms) and
    [Translating plural forms](#Translating-plural-forms).

---

Next: [Pretranslating PO Files](#Pretranslating), Previous: [Creating or preparing for translating a PO file](#Creating), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 7 Updating Existing PO Files [¶](#Updating-Existing-PO-Files)

* [Invoking the `msgmerge` Program](#msgmerge-Invocation)

---

Up: [Updating Existing PO Files](#Updating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 7.1 Invoking the `msgmerge` Program [¶](#Invoking-the-msgmerge-Program)

```
msgmerge [option] def.po ref.pot
```

The `msgmerge` program merges two Uniforum style .po files together.
The def.po file is an existing PO file with translations which will
be taken over to the newly created file as long as they still match;
comments will be preserved, but extracted comments and file positions will
be discarded. The ref.pot file is the last created PO file with
up-to-date source references but old translations, or a PO Template file
(generally created by `xgettext`); any translations or comments
in the file will be discarded, however dot comments and file positions
will be preserved. Where an exact match cannot be found, fuzzy matching
is used to produce better results.

* [Input file location](#Input-file-location-2)
* [Operation mode](#Operation-mode-1)
* [Output file location](#Output-file-location-2)
* [Output file location in update mode](#Output-file-location-in-update-mode)
* [Operation modifiers](#Operation-modifiers)
* [Input file syntax](#Input-file-syntax-1)
* [Output details](#Output-details-2)
* [Informative output](#Informative-output-2)

#### 7.1.1 Input file location [¶](#Input-file-location-2)

‘`def.po`’
:   Translations referring to old sources.

‘`ref.pot`’
:   References to the new sources.

‘`-D directory`’ [¶](#index-_002dD_002c-msgmerge-option)

‘`--directory=directory`’
:   Add directory to the list of directories. Source files are
    searched relative to this list of directories. The resulting `.po`
    file will be written relative to the current directory, though.

‘`-C file`’ [¶](#index-_002dC_002c-msgmerge-option)

‘`--compendium=file`’
:   Specify an additional library of message translations. See [Using Translation Compendia](#Compendium).
    This option may be specified more than once.

#### 7.1.2 Operation mode [¶](#Operation-mode-1)

‘`-U`’ [¶](#index-_002dU_002c-msgmerge-option)

‘`--update`’
:   Update def.po. Do nothing if def.po is already up to date.

#### 7.1.3 Output file location [¶](#Output-file-location-2)

‘`-o file`’ [¶](#index-_002do_002c-msgmerge-option)

‘`--output-file=file`’
:   Write output to specified file.

The results are written to standard output if no output file is specified
or if it is ‘`-`’.

#### 7.1.4 Output file location in update mode [¶](#Output-file-location-in-update-mode)

The result is written back to def.po.

‘`--backup=control`’ [¶](#index-_002d_002dbackup_002c-msgmerge-option)
:   Make a backup of def.po

‘`--suffix=suffix`’ [¶](#index-_002d_002dsuffix_002c-msgmerge-option)
:   Override the usual backup suffix.

The version control method may be selected via the `--backup` option
or through the `VERSION_CONTROL` environment variable. Here are the
values:

‘`none`’

‘`off`’
:   Never make backups (even if `--backup` is given).

‘`numbered`’

‘`t`’
:   Make numbered backups.

‘`existing`’

‘`nil`’
:   Make numbered backups if numbered backups for this file already exist,
    otherwise make simple backups.

‘`simple`’

‘`never`’
:   Always make simple backups.

The backup suffix is ‘`~`’, unless set with `--suffix` or the
`SIMPLE_BACKUP_SUFFIX` environment variable.

#### 7.1.5 Operation modifiers [¶](#Operation-modifiers)

‘`-m`’ [¶](#index-_002dm_002c-msgmerge-option)

‘`--multi-domain`’
:   Apply ref.pot to each of the domains in def.po.

‘`--for-msgfmt`’ [¶](#index-_002d_002dfor_002dmsgfmt_002c-msgmerge-option)
:   Produce a PO file meant for `msgfmt` only, not for a translator.
    This option omits untranslated messages, fuzzy messages (except the header
    entry), and obsolete messages from the output. Also, it omits translator
    comments and ‘`#: filename:line`’ lines from the output.
    In particular, this option implies ‘`--no-fuzzy-matching`’.

‘`-N`’ [¶](#index-_002dN_002c-msgmerge-option)

‘`--no-fuzzy-matching`’
:   Do not use fuzzy matching when an exact match is not found. This may speed
    up the operation considerably.

‘`--previous`’ [¶](#index-_002d_002dprevious_002c-msgmerge-option)
:   Keep the previous msgids of translated messages, marked with ‘`#|`’, when
    adding the fuzzy marker to such messages.

#### 7.1.6 Input file syntax [¶](#Input-file-syntax-1)

‘`-P`’ [¶](#index-_002dP_002c-msgmerge-option)

‘`--properties-input`’
:   Assume the input files are Java ResourceBundles in Java `.properties`
    syntax, not in PO file syntax.

‘`--stringtable-input`’ [¶](#index-_002d_002dstringtable_002dinput_002c-msgmerge-option)
:   Assume the input files are NeXTstep/GNUstep localized resource files in
    `.strings` syntax, not in PO file syntax.

#### 7.1.7 Output details [¶](#Output-details-2)

‘`--lang=catalogname`’ [¶](#index-_002d_002dlang_002c-msgmerge-option)
:   Specify the ‘`Language`’ field to be used in the header entry. See
    [Filling in the Header Entry](#Header-Entry) for the meaning of this field. Note: The
    ‘`Language-Team`’ and ‘`Plural-Forms`’ fields are left unchanged.
    If this option is not specified, the ‘`Language`’ field is inferred, as
    best as possible, from the ‘`Language-Team`’ field.

‘`--color`’ [¶](#index-_002d_002dcolor_002c-msgmerge-option)

‘`--color=when`’
:   Specify whether or when to use colors and other text attributes.
    See [The `--color` option](#The-_002d_002dcolor-option) for details.

‘`--style=style_file`’ [¶](#index-_002d_002dstyle_002c-msgmerge-option)
:   Specify the CSS style rule file to use for `--color`.
    See [The `--style` option](#The-_002d_002dstyle-option) for details.

‘`--force-po`’ [¶](#index-_002d_002dforce_002dpo_002c-msgmerge-option)
:   Always write an output file even if it contains no message.

‘`-i`’ [¶](#index-_002di_002c-msgmerge-option)

‘`--indent`’
:   Write the .po file using indented style.

‘`--no-location`’ [¶](#index-_002d_002dno_002dlocation_002c-msgmerge-option)
:   Do not write ‘`#: filename:line`’ lines.

‘`-n`’ [¶](#index-_002d_002dadd_002dlocation_002c-msgmerge-option)

‘`--add-location=type`’
:   Generate ‘`#: filename:line`’ lines (default).

    The optional type can be either ‘`full`’, ‘`file`’, or
    ‘`never`’. If it is not given or ‘`full`’, it generates the
    lines with both file name and line number. If it is ‘`file`’, the
    line number part is omitted. If it is ‘`never`’, it completely
    suppresses the lines (same as `--no-location`).

‘`--strict`’ [¶](#index-_002d_002dstrict_002c-msgmerge-option)
:   Write out a strict Uniforum conforming PO file. Note that this
    Uniforum format should be avoided because it doesn’t support the
    GNU extensions.

‘`-p`’ [¶](#index-_002dp_002c-msgmerge-option)

‘`--properties-output`’
:   Write out a Java ResourceBundle in Java `.properties` syntax. Note
    that this file format doesn’t support plural forms and silently drops
    obsolete messages.

‘`--stringtable-output`’ [¶](#index-_002d_002dstringtable_002doutput_002c-msgmerge-option)
:   Write out a NeXTstep/GNUstep localized resource file in `.strings` syntax.
    Note that this file format doesn’t support plural forms.

‘`-w number`’ [¶](#index-_002dw_002c-msgmerge-option)

‘`--width=number`’
:   Set the output page width. Long strings in the output files will be
    split across multiple lines in order to ensure that each line’s width
    (= number of screen columns) is less or equal to the given number.

‘`--no-wrap`’ [¶](#index-_002d_002dno_002dwrap_002c-msgmerge-option)
:   Do not break long message lines. Message lines whose width exceeds the
    output page width will not be split into several lines. Only file reference
    lines which are wider than the output page width will be split.

‘`-s`’ [¶](#index-_002ds_002c-msgmerge-option)

‘`--sort-output`’
:   Generate sorted output (deprecated). Note that using this option makes it
    much harder for the translator to understand each message’s context.

‘`-F`’ [¶](#index-_002dF_002c-msgmerge-option)

‘`--sort-by-file`’
:   Sort output by file location.

#### 7.1.8 Informative output [¶](#Informative-output-2)

‘`-h`’ [¶](#index-_002dh_002c-msgmerge-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-msgmerge-option)

‘`--version`’
:   Output version information and exit.

‘`-v`’ [¶](#index-_002dv_002c-msgmerge-option)

‘`--verbose`’
:   Increase verbosity level.

‘`-q`’ [¶](#index-_002dq_002c-msgmerge-option)

‘`--quiet`’

‘`--silent`’
:   Suppress progress indicators.

---

Next: [Editing PO Files](#Editing), Previous: [Updating Existing PO Files](#Updating), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 8 Pretranslating PO Files [¶](#Pretranslating-PO-Files)

As a translator,
you may save yourself some work
by starting from a reasonably good translation produced by a machine,
and modify that translation, to make it perfect.

Thus, before you start working on a translation,
you might have the PO file *pretranslated*.

This process, also called *machine translation*,
is nowadays best performed through a Large Language Model (LLM).
(See <https://en.wikipedia.org/wiki/Machine_translation#Neural_MT>,
<https://en.wikipedia.org/wiki/Neural_machine_translation#Generative_LLMs>.)

* [Installing a Large Language Model](#Installing-an-LLM)
* [Invoking the `msgpre` Program](#msgpre-Invocation)
* [Invoking the `spit` Program](#spit-Invocation)

---

Next: [Invoking the `msgpre` Program](#msgpre-Invocation), Up: [Pretranslating PO Files](#Pretranslating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 8.1 Installing a Large Language Model [¶](#Installing-a-Large-Language-Model)

We don’t recommend to use machine translation
through a web service in the cloud, controlled by someone else than yourself.
Such a machine translation service would be have major drawbacks
(it could go away any time,
it could be used to spy on you or manipulate you,
or the costs could go up beyond your control);
see <https://www.gnu.org/philosophy/who-does-that-server-really-serve.en.html>.
Additionally, such a service typically has some cost
(between $10 and $25 per megabyte, as of 2025).

Instead, we recommend a Large Language Model execution engine
that runs on hardware under your control.
This can be a desktop computer,
or for instance a single-board computer in your local network.

At this point (in 2025),
a Large Language Model execution engine that is Free Software is
‘`ollama`’, that can be downloaded from <https://ollama.com/>.

Next, you will need to pick a Large Language Model.
There are two properties to watch out for:

* The license under which the LLM is released.
  While some LLMs are under a Free Software license
  (most often the Apache 2.0 license)
  and are thus “open weights” models
  (see <https://opensource.org/ai/open-weights>),
  others have licenses with usage restrictions.
* The size of the LLM.
  Generally, it should be smaller than the available RAM size
  of your machine on which you will run the LLM execution engine.

Together with an LLM of reasonable quality,
such as the model `ministral-3:14b`,
the system requirements are as follows:

* RAM: 16 GB.
* Disk space: 10 GB (1 GB for `ollama`, 9 GB for the model).
* GPU or TPU: A GPU (Graphics Processing Unit) or TPU (Tensor Processing Unit)
  is not needed.
  As of 2025, a high-end GPU from certain vendors
  can be used by `ollama` to provide an optional speedup.

Additional configuration:

* If you are running `ollama` on your computer directly,
  no further configuration is needed.
* If you are running `ollama` on a separate machine,
  and want to make it accessible from all machines in the LAN:
  Edit the file `/etc/systemd/system/ollama.services`,
  adding a line: `Environment="OLLAMA_HOST=0.0.0.0"`.
  See <https://github.com/ollama/ollama/issues/703>.
* If you are running `ollama` in a virtual machine,
  make the port 11434 accessible through port forwarding.

---

Next: [Invoking the `spit` Program](#spit-Invocation), Previous: [Installing a Large Language Model](#Installing-an-LLM), Up: [Pretranslating PO Files](#Pretranslating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 8.2 Invoking the `msgpre` Program [¶](#Invoking-the-msgpre-Program)

```
msgpre [option...]
```

The `msgpre` program pretranslates a translation catalog.

**Warning:** The pretranslations might not be what you expect.
They might be of the wrong form, be of poor quality, or reflect some biases.

* [Input file location](#Input-file-location-3)
* [Output file location](#Output-file-location-3)
* [Message selection](#Message-selection)
* [Large Language Model (LLM) options](#Large-Language-Model-_0028LLM_0029-options)
* [Input file syntax](#Input-file-syntax-2)
* [Output details](#Output-details-3)
* [Informative output](#Informative-output-3)
* [Examples](#Examples)

#### 8.2.1 Input file location [¶](#Input-file-location-3)

‘`-i inputfile`’ [¶](#index-_002di_002c-msgpre-option)

‘`--input=inputfile`’
:   Input PO file.

‘`-D directory`’ [¶](#index-_002dD_002c-msgpre-option)

‘`--directory=directory`’
:   Add directory to the list of directories. Source files are
    searched relative to this list of directories. The resulting `.po`
    file will be written relative to the current directory, though.

If no inputfile is given or if it is ‘`-`’, standard input is read.

#### 8.2.2 Output file location [¶](#Output-file-location-3)

‘`-o file`’ [¶](#index-_002do_002c-msgpre-option)

‘`--output-file=file`’
:   Write output to specified file.

The results are written to standard output if no output file is specified
or if it is ‘`-`’.

#### 8.2.3 Message selection [¶](#Message-selection)

‘`--keep-fuzzy`’ [¶](#index-_002d_002dkeep_002dfuzzy_002c-msgpre-option)
:   Keep fuzzy messages unmodified.
    Pretranslate only untranslated messages.

#### 8.2.4 Large Language Model (LLM) options [¶](#Large-Language-Model-_0028LLM_0029-options)

‘`--species=type`’ [¶](#index-_002d_002dspecies_002c-msgpre-option)
:   Specifies the type of Large Language Model execution engine.
    The default and only valid value is `ollama`.

‘`--url=url`’ [¶](#index-_002d_002durl_002c-msgpre-option)
:   Specifies the URL of the server that runs Large Language Model execution engine.
    For `ollama`, the default is `http://localhost:11434`.

‘`-m model`’ [¶](#index-_002dm_002c-msgpre-option)

‘`--model=model`’
:   Specifies the model to use.
    This option is mandatory; no default exists.
    The specified model must
    already be installed in the Large Language Model execution engine.

‘`--prompt=text`’ [¶](#index-_002d_002dprompt_002c-msgpre-option)
:   Specifies the prompt to use before each `msgid` from the PO file.
    It allows you to specify extra instructions for the LLM.
    The prompt should include an instruction like
    "Translate into target language.".
    Some hints for good prompts are described in the article
    “How to write AI prompts for translation”
    <https://poeditor.com/blog/ai-prompts-for-translation/>.

‘`--postprocess=command`’ [¶](#index-_002d_002dpostprocess_002c-msgpre-option)
:   Specifies a command to post-process the output from the LLM.
    This should be a Bourne shell command
    that reads from standard input and writes to standard output.

    For instance, the `ministral-3:14b` model
    often emphasizes part of the output with ‘`**`’ characters.
    To eliminate these markers,
    you could use the command ‘`sed -e 's/[*][*]//g'`’.

#### 8.2.5 Input file syntax [¶](#Input-file-syntax-2)

‘`-P`’ [¶](#index-_002dP_002c-msgpre-option)

‘`--properties-input`’
:   Assume the input file is a Java ResourceBundle in Java `.properties`
    syntax, not in PO file syntax.

‘`--stringtable-input`’ [¶](#index-_002d_002dstringtable_002dinput_002c-msgpre-option)
:   Assume the input file is a NeXTstep/GNUstep localized resource file in
    `.strings` syntax, not in PO file syntax.

#### 8.2.6 Output details [¶](#Output-details-3)

‘`--color`’ [¶](#index-_002d_002dcolor_002c-msgpre-option)

‘`--color=when`’
:   Specify whether or when to use colors and other text attributes.
    See [The `--color` option](#The-_002d_002dcolor-option) for details.

‘`--style=style_file`’ [¶](#index-_002d_002dstyle_002c-msgpre-option)
:   Specify the CSS style rule file to use for `--color`.
    See [The `--style` option](#The-_002d_002dstyle-option) for details.

‘`--force-po`’ [¶](#index-_002d_002dforce_002dpo_002c-msgpre-option)
:   Always write an output file even if it contains no message.

‘`--indent`’ [¶](#index-_002d_002dindent_002c-msgpre-option)
:   Write the .po file using indented style.

‘`--no-location`’ [¶](#index-_002d_002dno_002dlocation_002c-msgpre-option)
:   Do not write ‘`#: filename:line`’ lines.

‘`-n`’ [¶](#index-_002d_002dadd_002dlocation_002c-msgpre-option)

‘`--add-location=type`’
:   Generate ‘`#: filename:line`’ lines (default).

    The optional type can be either ‘`full`’, ‘`file`’, or
    ‘`never`’. If it is not given or ‘`full`’, it generates the
    lines with both file name and line number. If it is ‘`file`’, the
    line number part is omitted. If it is ‘`never`’, it completely
    suppresses the lines (same as `--no-location`).

‘`--strict`’ [¶](#index-_002d_002dstrict_002c-msgpre-option)
:   Write out a strict Uniforum conforming PO file. Note that this
    Uniforum format should be avoided because it doesn’t support the
    GNU extensions.

‘`-p`’ [¶](#index-_002dp_002c-msgpre-option)

‘`--properties-output`’
:   Write out a Java ResourceBundle in Java `.properties` syntax. Note
    that this file format doesn’t support plural forms and silently drops
    obsolete messages.

‘`--stringtable-output`’ [¶](#index-_002d_002dstringtable_002doutput_002c-msgpre-option)
:   Write out a NeXTstep/GNUstep localized resource file in `.strings` syntax.
    Note that this file format doesn’t support plural forms.

‘`-w number`’ [¶](#index-_002dw_002c-msgpre-option)

‘`--width=number`’
:   Set the output page width. Long strings in the output files will be
    split across multiple lines in order to ensure that each line’s width
    (= number of screen columns) is less or equal to the given number.

‘`--no-wrap`’ [¶](#index-_002d_002dno_002dwrap_002c-msgpre-option)
:   Do not break long message lines. Message lines whose width exceeds the
    output page width will not be split into several lines. Only file reference
    lines which are wider than the output page width will be split.

‘`-s`’ [¶](#index-_002ds_002c-msgpre-option)

‘`--sort-output`’
:   Generate sorted output. Note that using this option makes it much harder
    for the translator to understand each message’s context.

‘`-F`’ [¶](#index-_002dF_002c-msgpre-option)

‘`--sort-by-file`’
:   Sort output by file location.

#### 8.2.7 Informative output [¶](#Informative-output-3)

‘`-h`’ [¶](#index-_002dh_002c-msgpre-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-msgpre-option)

‘`--version`’
:   Output version information and exit.

‘`-q`’ [¶](#index-_002dq_002c-msgpre-option)

‘`--quiet`’

‘`--silent`’
:   Suppress progress indicators.

#### 8.2.8 Examples [¶](#Examples)

To pretranslate the file `foo.po`:

```
msgpre --model=ministral-3:14b < foo.po > foo-pretranslated.po
```

Note that this command can take a long time,
depending on the model and the available hardware.

---

Previous: [Invoking the `msgpre` Program](#msgpre-Invocation), Up: [Pretranslating PO Files](#Pretranslating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 8.3 Invoking the `spit` Program [¶](#Invoking-the-spit-Program)

```
spit [option...]
```

The `spit` program
passes its input to a Large Language Model (LLM) instance
and prints the response.
With the `--to` option,
it translates its input to the specified language
through a Large Language Model (LLM) and prints the translation.

**Warning:** The output might not be what you expect.
It might be of the wrong form, be of poor quality, or reflect some biases.

* [Large Language Model (LLM) options](#Large-Language-Model-_0028LLM_0029-options-1)
* [Informative output](#Informative-output-4)
* [Examples](#Examples-1)

#### 8.3.1 Large Language Model (LLM) options [¶](#Large-Language-Model-_0028LLM_0029-options-1)

‘`--species=type`’ [¶](#index-_002d_002dspecies_002c-spit-option)
:   Specifies the type of Large Language Model execution engine.
    The default and only valid value is `ollama`.

‘`--url=url`’ [¶](#index-_002d_002durl_002c-spit-option)
:   Specifies the URL of the server that runs Large Language Model execution engine.
    For `ollama`, the default is `http://localhost:11434`.

‘`-m model`’ [¶](#index-_002dm_002c-spit-option)

‘`--model=model`’
:   Specifies the model to use.
    This option is mandatory; no default exists.
    The specified model must
    already be installed in the Large Language Model execution engine.

‘`--to=language`’ [¶](#index-_002d_002dto_002c-spit-option)
:   Specifies the target language.
    language may be specified
    as an ISO 639 language code (such as `fr` for French),
    as a combination of an ISO 639 language code and an ISO 3166 country code
    (such as `fr_CA` for French in Canada,
    or `zh_TW` for traditional Chinese),
    or as the English name of a language (such as `French`).

    The effect of this option is to
    add a prompt similar to "Translate to language:".

‘`--prompt=text`’ [¶](#index-_002d_002dprompt_002c-spit-option)
:   Specifies the prompt to use before the input that comes from standard input.
    It allows you to specify extra instructions for the LLM.

    This option overrides the `--to` option.

‘`--postprocess=command`’ [¶](#index-_002d_002dpostprocess_002c-spit-option)
:   Specifies a command to post-process the output.
    This should be a Bourne shell command
    that reads from standard input and writes to standard output.

    For instance, the `ministral-3:14b` model
    often emphasizes part of the output with ‘`**`’ characters.
    To eliminate these markers,
    you could use the command ‘`sed -e 's/[*][*]//g'`’.

#### 8.3.2 Informative output [¶](#Informative-output-4)

‘`-h`’ [¶](#index-_002dh_002c-spit-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-spit-option)

‘`--version`’
:   Output version information and exit.

#### 8.3.3 Examples [¶](#Examples-1)

Machine translation of a single sentence:

```
$ echo 'Translate into German: "Welcome to the GNU project!"' \
    | spit --model=ministral-3:14b \
           --postprocess="sed -e 's/[*][*]//g'"
"Willkommen zum GNU-Projekt!"
```

The perfect translation would be `"Willkommen beim GNU-Projekt!"`.
You can see: some manual adjustment after the machine translation is needed.

---

Next: [Manipulating PO Files](#Manipulating), Previous: [Pretranslating PO Files](#Pretranslating), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 9 Editing PO Files [¶](#Editing-PO-Files)

As a translator, you will typically edit a PO file
in an editor that has built-in knowledge about the PO file format.
You most probably won’t want to edit a PO file
in a text editor for plain-text files,
because that would be cumbersome regarding cursor navigation
and would also easily lead to syntax mistakes.

* [Web-based PO editing](#Web-based-localization)
* [KDE’s PO File Editor](#Lokalize)
* [GNOME’s PO File Editor](#Gtranslator)
* [Poedit](#Poedit)
* [OmegaT](#OmegaT)
* [The Virtaal Translation Editor](#Virtaal)
* [Emacs’s PO File Editor](#PO-Mode)
* [Editing PO Files in vim](#Vim)
* [Using Translation Compendia](#Compendium)

---

Next: [KDE’s PO File Editor](#Lokalize), Up: [Editing PO Files](#Editing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 9.1 Web-based PO editing [¶](#Web_002dbased-PO-editing)

There are two ways to edit a PO file:
either through a web-based PO editor, in a browser,
or through a PO editor that you can install on your computer.
Which one you choose, depends on your habits.

Typically, the software project for which you want to provide translations
has set up a workflow that you, as a translator, have to follow.

* There are projects which use the [Weblate](https://weblate.org/)
  localization suite.
  In this case, you have the choice between a
  [web-based PO editor](https://docs.weblate.org/en/latest/user/translating.html)
  and a [workflow](https://docs.weblate.org/en/latest/user/files.html)
  where you download the previous translation,
  update it with your preferred PO editor,
  and then upload it back.
* There are projects which offer you only a web-based PO editor, as only choice.
  Since web-based tools restrict your freedom as a user – you cannot make
  modifications on your own to that tool –,
  you should complain to that project and claim the choice of using a locally
  installed PO editor, because that is the only way that can guarantee
  your freedom of choice and freedom to modify the software.
* There are projects which do not support web-based localizations.
  In this case, pick a PO file editor as listed in the next few sections.
  Examples for such projects are
  the [Free Translation Project](https://translationproject.org/),
  as well as projects where you directly interact with the version control
  system of the project.

---

Next: [GNOME’s PO File Editor](#Gtranslator), Previous: [Web-based PO editing](#Web-based-localization), Up: [Editing PO Files](#Editing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 9.2 KDE’s PO File Editor [¶](#KDE_0027s-PO-File-Editor)

Lokalize (<https://apps.kde.org/lokalize/>)
is the PO file editor made by the KDE project.
It is present in [many](https://repology.org/project/lokalize/versions)
GNU/Linux distributions.

---

Next: [Poedit](#Poedit), Previous: [KDE’s PO File Editor](#Lokalize), Up: [Editing PO Files](#Editing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 9.3 GNOME’s PO File Editor [¶](#GNOME_0027s-PO-File-Editor)

Gtranslator (<https://wiki.gnome.org/Apps/Gtranslator>)
is the PO file editor made by the GNOME project.
It is present in [many](https://repology.org/project/gtranslator/versions)
GNU/Linux distributions.

---

Next: [OmegaT](#OmegaT), Previous: [GNOME’s PO File Editor](#Gtranslator), Up: [Editing PO Files](#Editing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 9.4 Poedit [¶](#Poedit-1)

Poedit (<https://github.com/vslavik/poedit>)
is another decent PO file editor.
It works on all major desktop OSes and
is present in [many](https://repology.org/project/poedit/versions)
GNU/Linux distributions.

---

Next: [The Virtaal Translation Editor](#Virtaal), Previous: [Poedit](#Poedit), Up: [Editing PO Files](#Editing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 9.5 OmegaT [¶](#OmegaT-1)

OmegaT
(Wikipedia: <https://en.wikipedia.org/wiki/OmegaT>,
home page: <https://omegat.org/>,
code: <https://github.com/omegat-org/omegat>)
is a translation editor
that focuses on speeding up the translator’s work through advanced features
like translation memory, spell-checking, glossaries, dictionaries.
It supports not only PO files, but also
direct translation of various file formats
(such as plain text, web pages, OpenDocument files, DocBook XML, etc.)
without using intermediate files.
It is present in [many](https://repology.org/omegat/poedit/versions)
GNU/Linux distributions.

---

Next: [Emacs’s PO File Editor](#PO-Mode), Previous: [OmegaT](#OmegaT), Up: [Editing PO Files](#Editing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 9.6 The Virtaal Translation Editor [¶](#The-Virtaal-Translation-Editor)

Virtaal
(Wikipedia: <https://en.wikipedia.org/wiki/Virtaal>,
home page: <https://virtaal.translatehouse.org/>,
code: <https://github.com/translate/virtaal>)
is a translation editor
that supports not only PO files but also XLIFF files.
It is present in [some](https://repology.org/project/virtaal/versions)
GNU/Linux distributions,
such as Debian up to Debian 11.

---

Next: [Editing PO Files in vim](#Vim), Previous: [The Virtaal Translation Editor](#Virtaal), Up: [Editing PO Files](#Editing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]
