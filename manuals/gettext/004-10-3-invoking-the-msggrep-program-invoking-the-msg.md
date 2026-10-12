### 10.3 Invoking the `msggrep` Program [¶](#Invoking-the-msggrep-Program)

```
msggrep [option] [inputfile]
```

The `msggrep` program extracts all messages of a translation catalog
that match a given pattern or belong to some given source files.

* [Input file location](#Input-file-location-6)
* [Output file location](#Output-file-location-6)
* [Message selection](#Message-selection-2)
* [Input file syntax](#Input-file-syntax-5)
* [Output details](#Output-details-6)
* [Informative output](#Informative-output-7)
* [Examples](#Examples-2)

#### 10.3.1 Input file location [¶](#Input-file-location-6)

‘`inputfile`’
:   Input PO file.

‘`-D directory`’ [¶](#index-_002dD_002c-msggrep-option)

‘`--directory=directory`’
:   Add directory to the list of directories. Source files are
    searched relative to this list of directories. The resulting `.po`
    file will be written relative to the current directory, though.

If no inputfile is given or if it is ‘`-`’, standard input is read.

#### 10.3.2 Output file location [¶](#Output-file-location-6)

‘`-o file`’ [¶](#index-_002do_002c-msggrep-option)

‘`--output-file=file`’
:   Write output to specified file.

The results are written to standard output if no output file is specified
or if it is ‘`-`’.

#### 10.3.3 Message selection [¶](#Message-selection-2)

```
  [-N sourcefile]... [-M domainname]...
  [-J msgctxt-pattern] [-K msgid-pattern] [-T msgstr-pattern]
  [-C comment-pattern] [-X extracted-comment-pattern]
  [-W workflow-flag] [-S sticky-flag]
```

A message is selected if

* it comes from one of the specified source files,
* or if it comes from one of the specified domains,
* or if ‘`-J`’ is given and its context (msgctxt) matches
  msgctxt-pattern,
* or if ‘`-K`’ is given and its key (msgid or msgid\_plural) matches
  msgid-pattern,
* or if ‘`-T`’ is given and its translation (msgstr) matches
  msgstr-pattern,
* or if ‘`-C`’ is given and the translator’s comment matches
  comment-pattern,
* or if ‘`-X`’ is given and the extracted comment matches
  extracted-comment-pattern,
* or if ‘`-W`’ is given and its flags contain workflow-flag,
* or if ‘`-S`’ is given and its flags contain sticky-flag.

When more than one selection criterion is specified, the set of selected
messages is the union of the selected messages of each criterion.

msgctxt-pattern or msgid-pattern or msgstr-pattern syntax:

```
  [-E | -F] [-e pattern | -f file]...
```

patterns are basic regular expressions by default, or extended regular
expressions if -E is given, or fixed strings if -F is given.

‘`-N sourcefile`’ [¶](#index-_002dN_002c-msggrep-option)

‘`--location=sourcefile`’
:   Select messages extracted from sourcefile. sourcefile can be
    either a literal file name or a wildcard pattern.

‘`-M domainname`’ [¶](#index-_002dM_002c-msggrep-option)

‘`--domain=domainname`’
:   Select messages belonging to domain domainname.

‘`-J`’ [¶](#index-_002dJ_002c-msggrep-option)

‘`--msgctxt`’
:   Start of patterns for the msgctxt.

‘`-K`’ [¶](#index-_002dK_002c-msggrep-option)

‘`--msgid`’
:   Start of patterns for the msgid.

‘`-T`’ [¶](#index-_002dT_002c-msggrep-option)

‘`--msgstr`’
:   Start of patterns for the msgstr.

‘`-C`’ [¶](#index-_002dC_002c-msggrep-option)

‘`--comment`’
:   Start of patterns for the translator’s comment.

‘`-X`’ [¶](#index-_002dX_002c-msggrep-option)

‘`--extracted-comment`’
:   Start of patterns for the extracted comments.

‘`-E`’ [¶](#index-_002dE_002c-msggrep-option)

‘`--extended-regexp`’
:   Specify that pattern is an extended regular expression.

‘`-F`’ [¶](#index-_002dF_002c-msggrep-option)

‘`--fixed-strings`’
:   Specify that pattern is a set of newline-separated strings.

‘`-e pattern`’ [¶](#index-_002de_002c-msggrep-option)

‘`--regexp=pattern`’
:   Use pattern as a regular expression.

‘`-f file`’ [¶](#index-_002df_002c-msggrep-option)

‘`--file=file`’
:   Obtain pattern from file.

‘`-i`’ [¶](#index-_002di_002c-msggrep-option)

‘`--ignore-case`’
:   Ignore case distinctions.

‘`-W workflow-flag`’ [¶](#index-_002dW_002c-msggrep-option)

‘`--workflow-flag=workflow-flag`’
:   Select messages with workflow-flag.
    Workflow flags are defined in [Workflow flags](#Workflow-flags).

‘`-S sticky-flag`’ [¶](#index-_002dS_002c-msggrep-option)

‘`--sticky-flag=sticky-flag`’
:   Select messages with sticky-flag.
    Sticky flags are defined in [Sticky flags](#Sticky-flags).

‘`-v`’ [¶](#index-_002dv_002c-msggrep-option)

‘`--invert-match`’
:   Output only the messages that do not match any selection criterion, instead
    of the messages that match a selection criterion.

#### 10.3.4 Input file syntax [¶](#Input-file-syntax-5)

‘`-P`’ [¶](#index-_002dP_002c-msggrep-option)

‘`--properties-input`’
:   Assume the input file is a Java ResourceBundle in Java `.properties`
    syntax, not in PO file syntax.

‘`--stringtable-input`’ [¶](#index-_002d_002dstringtable_002dinput_002c-msggrep-option)
:   Assume the input file is a NeXTstep/GNUstep localized resource file in
    `.strings` syntax, not in PO file syntax.

#### 10.3.5 Output details [¶](#Output-details-6)

‘`--color`’ [¶](#index-_002d_002dcolor_002c-msggrep-option)

‘`--color=when`’
:   Specify whether or when to use colors and other text attributes.
    See [The `--color` option](#The-_002d_002dcolor-option) for details.

‘`--style=style_file`’ [¶](#index-_002d_002dstyle_002c-msggrep-option)
:   Specify the CSS style rule file to use for `--color`.
    See [The `--style` option](#The-_002d_002dstyle-option) for details.

‘`--force-po`’ [¶](#index-_002d_002dforce_002dpo_002c-msggrep-option)
:   Always write an output file even if it contains no message.

‘`--indent`’ [¶](#index-_002d_002dindent_002c-msggrep-option)
:   Write the .po file using indented style.

‘`--no-location`’ [¶](#index-_002d_002dno_002dlocation_002c-msggrep-option)
:   Do not write ‘`#: filename:line`’ lines.

‘`-n`’ [¶](#index-_002d_002dadd_002dlocation_002c-msggrep-option)

‘`--add-location=type`’
:   Generate ‘`#: filename:line`’ lines (default).

    The optional type can be either ‘`full`’, ‘`file`’, or
    ‘`never`’. If it is not given or ‘`full`’, it generates the
    lines with both file name and line number. If it is ‘`file`’, the
    line number part is omitted. If it is ‘`never`’, it completely
    suppresses the lines (same as `--no-location`).

‘`--strict`’ [¶](#index-_002d_002dstrict_002c-msggrep-option)
:   Write out a strict Uniforum conforming PO file. Note that this
    Uniforum format should be avoided because it doesn’t support the
    GNU extensions.

‘`-p`’ [¶](#index-_002dp_002c-msggrep-option)

‘`--properties-output`’
:   Write out a Java ResourceBundle in Java `.properties` syntax. Note
    that this file format doesn’t support plural forms and silently drops
    obsolete messages.

‘`--stringtable-output`’ [¶](#index-_002d_002dstringtable_002doutput_002c-msggrep-option)
:   Write out a NeXTstep/GNUstep localized resource file in `.strings` syntax.
    Note that this file format doesn’t support plural forms.

‘`-w number`’ [¶](#index-_002dw_002c-msggrep-option)

‘`--width=number`’
:   Set the output page width. Long strings in the output files will be
    split across multiple lines in order to ensure that each line’s width
    (= number of screen columns) is less or equal to the given number.

‘`--no-wrap`’ [¶](#index-_002d_002dno_002dwrap_002c-msggrep-option)
:   Do not break long message lines. Message lines whose width exceeds the
    output page width will not be split into several lines. Only file reference
    lines which are wider than the output page width will be split.

‘`--sort-output`’ [¶](#index-_002d_002dsort_002doutput_002c-msggrep-option)
:   Generate sorted output. Note that using this option makes it much harder
    for the translator to understand each message’s context.

‘`--sort-by-file`’ [¶](#index-_002d_002dsort_002dby_002dfile_002c-msggrep-option)
:   Sort output by file location.

#### 10.3.6 Informative output [¶](#Informative-output-7)

‘`-h`’ [¶](#index-_002dh_002c-msggrep-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-msggrep-option)

‘`--version`’
:   Output version information and exit.

#### 10.3.7 Examples [¶](#Examples-2)

To extract the messages that come from the source files
`gnulib-lib/error.c` and `gnulib-lib/getopt.c`:

```
msggrep -N gnulib-lib/error.c -N gnulib-lib/getopt.c input.po
```

To extract the messages that contain the string “Please specify” in the
original string:

```
msggrep --msgid -F -e 'Please specify' input.po
```

To extract the messages that have a context specifier of either “Menu>File”
or “Menu>Edit” or a submenu of them:

```
msggrep --msgctxt -E -e '^Menu>(File|Edit)' input.po
```

To extract the messages whose translation contains one of the strings in the
file `wordlist.txt`:

```
msggrep --msgstr -F -f wordlist.txt input.po
```

---

Next: [Invoking the `msguniq` Program](#msguniq-Invocation), Previous: [Invoking the `msggrep` Program](#msggrep-Invocation), Up: [Manipulating PO Files](#Manipulating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 10.4 Invoking the `msgfilter` Program [¶](#Invoking-the-msgfilter-Program)

```
msgfilter [option] filter [filter-option]
```

The `msgfilter` program applies a filter to all translations of a
translation catalog.

During each filter invocation, the environment variable
`MSGFILTER_MSGID` is bound to the message’s msgid, and the environment
variable `MSGFILTER_LOCATION` is bound to the location in the PO file
of the message. If the message has a context, the environment variable
`MSGFILTER_MSGCTXT` is bound to the message’s msgctxt, otherwise it is
unbound. If the message has a plural form, environment variable
`MSGFILTER_MSGID_PLURAL` is bound to the message’s msgid\_plural and
`MSGFILTER_PLURAL_FORM` is bound to the order number of the plural
actually processed (starting with 0), otherwise both are unbound.
If the message has a previous msgid (added by `msgmerge`),
environment variable `MSGFILTER_PREV_MSGCTXT` is bound to the
message’s previous msgctxt, `MSGFILTER_PREV_MSGID` is bound to
the previous msgid, and `MSGFILTER_PREV_MSGID_PLURAL` is bound to
the previous msgid\_plural.

* [Input file location](#Input-file-location-7)
* [Output file location](#Output-file-location-7)
* [The filter](#The-filter)
* [Useful filter-options when the filter is ‘`sed`’](#Useful-filter_002doptions-when-the-filter-is-sed)
* [Built-in filters](#Built_002din-filters)
* [Input file syntax](#Input-file-syntax-6)
* [Output details](#Output-details-7)
* [Informative output](#Informative-output-8)
* [Examples](#Examples-3)

#### 10.4.1 Input file location [¶](#Input-file-location-7)

‘`-i inputfile`’ [¶](#index-_002di_002c-msgfilter-option)

‘`--input=inputfile`’
:   Input PO file.

‘`-D directory`’ [¶](#index-_002dD_002c-msgfilter-option)

‘`--directory=directory`’
:   Add directory to the list of directories. Source files are
    searched relative to this list of directories. The resulting `.po`
    file will be written relative to the current directory, though.

If no inputfile is given or if it is ‘`-`’, standard input is read.

#### 10.4.2 Output file location [¶](#Output-file-location-7)

‘`-o file`’ [¶](#index-_002do_002c-msgfilter-option)

‘`--output-file=file`’
:   Write output to specified file.

The results are written to standard output if no output file is specified
or if it is ‘`-`’.

#### 10.4.3 The filter [¶](#The-filter)

The filter can be any program that reads a translation from standard
input and writes a modified translation to standard output. A frequently
used filter is ‘`sed`’. A few particular built-in filters are also
recognized.

‘`--newline`’ [¶](#index-_002d_002dnewline_002c-msgfilter-option)
:   Add newline at the end of each input line and also strip the ending
    newline from the output line.

Note: If the filter is not a built-in filter, you have to care about encodings:
It is your responsibility to ensure that the filter can cope
with input encoded in the translation catalog’s encoding. If the
filter wants input in a particular encoding, you can in a first step
convert the translation catalog to that encoding using the ‘`msgconv`’
program, before invoking ‘`msgfilter`’. If the filter wants input
in the locale’s encoding, but you want to avoid the locale’s encoding, then
you can first convert the translation catalog to UTF-8 using the
‘`msgconv`’ program and then make ‘`msgfilter`’ work in an UTF-8
locale, by using the `LC_ALL` environment variable.

Note: Most translations in a translation catalog don’t end with a
newline character. For this reason, unless the `--newline`
option is used, it is important that the filter recognizes its
last input line even if it ends without a newline, and that it doesn’t
add an undesired trailing newline at the end. The ‘`sed`’ program on
some platforms is known to ignore the last line of input if it is not
terminated with a newline. You can use GNU `sed` instead; it does
not have this limitation.

#### 10.4.4 Useful filter-options when the filter is ‘`sed`’ [¶](#Useful-filter_002doptions-when-the-filter-is-sed)

‘`-e script`’ [¶](#index-_002de_002c-msgfilter-option)

‘`--expression=script`’
:   Add script to the commands to be executed.

‘`-f scriptfile`’ [¶](#index-_002df_002c-msgfilter-option)

‘`--file=scriptfile`’
:   Add the contents of scriptfile to the commands to be executed.

‘`-n`’ [¶](#index-_002dn_002c-msgfilter-option)

‘`--quiet`’

‘`--silent`’
:   Suppress automatic printing of pattern space.

#### 10.4.5 Built-in filters [¶](#Built_002din-filters)

The filter ‘`recode-sr-latin`’ is recognized as a built-in filter.
The command ‘`recode-sr-latin`’ converts Serbian text, written in the
Cyrillic script, to the Latin script.
The command ‘`msgfilter recode-sr-latin`’ applies this conversion to the
translations of a PO file. Thus, it can be used to convert an `sr.po`
file to an `sr@latin.po` file.

The filter ‘`quot`’ is recognized as a built-in filter.
The command ‘`msgfilter quot`’ converts any quotations surrounded
by a pair of ‘`"`’, ‘`'`’, and ‘`` ` ``’.

The filter ‘`boldquot`’ is recognized as a built-in filter.
The command ‘`msgfilter boldquot`’ converts any quotations
surrounded by a pair of ‘`"`’, ‘`'`’, and ‘`` ` ``’, also adding the
VT100 escape sequences to the text to decorate it as bold.

The use of built-in filters is not sensitive to the current locale’s encoding.
Moreover, when used with a built-in filter, ‘`msgfilter`’ can automatically
convert the message catalog to the UTF-8 encoding when needed.

#### 10.4.6 Input file syntax [¶](#Input-file-syntax-6)

‘`-P`’ [¶](#index-_002dP_002c-msgfilter-option)

‘`--properties-input`’
:   Assume the input file is a Java ResourceBundle in Java `.properties`
    syntax, not in PO file syntax.

‘`--stringtable-input`’ [¶](#index-_002d_002dstringtable_002dinput_002c-msgfilter-option)
:   Assume the input file is a NeXTstep/GNUstep localized resource file in
    `.strings` syntax, not in PO file syntax.

#### 10.4.7 Output details [¶](#Output-details-7)

‘`--color`’ [¶](#index-_002d_002dcolor_002c-msgfilter-option)

‘`--color=when`’
:   Specify whether or when to use colors and other text attributes.
    See [The `--color` option](#The-_002d_002dcolor-option) for details.

‘`--style=style_file`’ [¶](#index-_002d_002dstyle_002c-msgfilter-option)
:   Specify the CSS style rule file to use for `--color`.
    See [The `--style` option](#The-_002d_002dstyle-option) for details.

‘`--force-po`’ [¶](#index-_002d_002dforce_002dpo_002c-msgfilter-option)
:   Always write an output file even if it contains no message.

‘`--indent`’ [¶](#index-_002d_002dindent_002c-msgfilter-option)
:   Write the .po file using indented style.

‘`--keep-header`’ [¶](#index-_002d_002dkeep_002dheader_002c-msgfilter-option)
:   Keep the header entry, i.e. the message with ‘`msgid ""`’, unmodified,
    instead of filtering it. By default, the header entry is subject to
    filtering like any other message.

‘`--no-location`’ [¶](#index-_002d_002dno_002dlocation_002c-msgfilter-option)
:   Do not write ‘`#: filename:line`’ lines.

‘`-n`’ [¶](#index-_002d_002dadd_002dlocation_002c-msgfilter-option)

‘`--add-location=type`’
:   Generate ‘`#: filename:line`’ lines (default).

    The optional type can be either ‘`full`’, ‘`file`’, or
    ‘`never`’. If it is not given or ‘`full`’, it generates the
    lines with both file name and line number. If it is ‘`file`’, the
    line number part is omitted. If it is ‘`never`’, it completely
    suppresses the lines (same as `--no-location`).

‘`--strict`’ [¶](#index-_002d_002dstrict_002c-msgfilter-option)
:   Write out a strict Uniforum conforming PO file. Note that this
    Uniforum format should be avoided because it doesn’t support the
    GNU extensions.

‘`-p`’ [¶](#index-_002dp_002c-msgfilter-option)

‘`--properties-output`’
:   Write out a Java ResourceBundle in Java `.properties` syntax. Note
    that this file format doesn’t support plural forms and silently drops
    obsolete messages.

‘`--stringtable-output`’ [¶](#index-_002d_002dstringtable_002doutput_002c-msgfilter-option)
:   Write out a NeXTstep/GNUstep localized resource file in `.strings` syntax.
    Note that this file format doesn’t support plural forms.

‘`-w number`’ [¶](#index-_002dw_002c-msgfilter-option)

‘`--width=number`’
:   Set the output page width. Long strings in the output files will be
    split across multiple lines in order to ensure that each line’s width
    (= number of screen columns) is less or equal to the given number.

‘`--no-wrap`’ [¶](#index-_002d_002dno_002dwrap_002c-msgfilter-option)
:   Do not break long message lines. Message lines whose width exceeds the
    output page width will not be split into several lines. Only file reference
    lines which are wider than the output page width will be split.

‘`-s`’ [¶](#index-_002ds_002c-msgfilter-option)

‘`--sort-output`’
:   Generate sorted output. Note that using this option makes it much harder
    for the translator to understand each message’s context.

‘`-F`’ [¶](#index-_002dF_002c-msgfilter-option)

‘`--sort-by-file`’
:   Sort output by file location.

#### 10.4.8 Informative output [¶](#Informative-output-8)

‘`-h`’ [¶](#index-_002dh_002c-msgfilter-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-msgfilter-option)

‘`--version`’
:   Output version information and exit.

#### 10.4.9 Examples [¶](#Examples-3)

To convert German translations to Swiss orthography (in an UTF-8 locale):

```
msgconv -t UTF-8 de.po | msgfilter sed -e 's/ß/ss/g'
```

To convert Serbian translations in Cyrillic script to Latin script:

```
msgfilter recode-sr-latin < sr.po
```

---

Next: [Invoking the `msgcomm` Program](#msgcomm-Invocation), Previous: [Invoking the `msgfilter` Program](#msgfilter-Invocation), Up: [Manipulating PO Files](#Manipulating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 10.5 Invoking the `msguniq` Program [¶](#Invoking-the-msguniq-Program)

```
msguniq [option] [inputfile]
```

The `msguniq` program unifies duplicate translations in a translation
catalog. It finds duplicate translations of the same message ID. Such
duplicates are invalid input for other programs like `msgfmt`,
`msgmerge` or `msgcat`. By default, duplicates are merged
together. When using the ‘`--repeated`’ option, only duplicates are
output, and all other messages are discarded. Comments and extracted
comments will be cumulated, except that if ‘`--use-first`’ is
specified, they will be taken from the first translation. File positions
will be cumulated. When using the ‘`--unique`’ option, duplicates are
discarded.

* [Input file location](#Input-file-location-8)
* [Output file location](#Output-file-location-8)
* [Message selection](#Message-selection-3)
* [Input file syntax](#Input-file-syntax-7)
* [Output details](#Output-details-8)
* [Informative output](#Informative-output-9)

#### 10.5.1 Input file location [¶](#Input-file-location-8)

‘`inputfile`’
:   Input PO file.

‘`-D directory`’ [¶](#index-_002dD_002c-msguniq-option)

‘`--directory=directory`’
:   Add directory to the list of directories. Source files are
    searched relative to this list of directories. The resulting `.po`
    file will be written relative to the current directory, though.

If no inputfile is given or if it is ‘`-`’, standard input is read.

#### 10.5.2 Output file location [¶](#Output-file-location-8)

‘`-o file`’ [¶](#index-_002do_002c-msguniq-option)

‘`--output-file=file`’
:   Write output to specified file.

The results are written to standard output if no output file is specified
or if it is ‘`-`’.

#### 10.5.3 Message selection [¶](#Message-selection-3)

‘`-d`’ [¶](#index-_002dd_002c-msguniq-option)

‘`--repeated`’
:   Print only duplicates.

‘`-u`’ [¶](#index-_002du_002c-msguniq-option)

‘`--unique`’
:   Print only unique messages, discard duplicates.

#### 10.5.4 Input file syntax [¶](#Input-file-syntax-7)

‘`-P`’ [¶](#index-_002dP_002c-msguniq-option)

‘`--properties-input`’
:   Assume the input file is a Java ResourceBundle in Java `.properties`
    syntax, not in PO file syntax.

‘`--stringtable-input`’ [¶](#index-_002d_002dstringtable_002dinput_002c-msguniq-option)
:   Assume the input file is a NeXTstep/GNUstep localized resource file in
    `.strings` syntax, not in PO file syntax.

#### 10.5.5 Output details [¶](#Output-details-8)

‘`-t`’ [¶](#index-_002dt_002c-msguniq-option)

‘`--to-code=name`’
:   Specify encoding for output.

‘`--use-first`’ [¶](#index-_002d_002duse_002dfirst_002c-msguniq-option)
:   Use first available translation for each message. Don’t merge several
    translations into one.

‘`--color`’ [¶](#index-_002d_002dcolor_002c-msguniq-option)

‘`--color=when`’
:   Specify whether or when to use colors and other text attributes.
    See [The `--color` option](#The-_002d_002dcolor-option) for details.

‘`--style=style_file`’ [¶](#index-_002d_002dstyle_002c-msguniq-option)
:   Specify the CSS style rule file to use for `--color`.
    See [The `--style` option](#The-_002d_002dstyle-option) for details.

‘`--force-po`’ [¶](#index-_002d_002dforce_002dpo_002c-msguniq-option)
:   Always write an output file even if it contains no message.

‘`-i`’ [¶](#index-_002di_002c-msguniq-option)

‘`--indent`’
:   Write the .po file using indented style.

‘`--no-location`’ [¶](#index-_002d_002dno_002dlocation_002c-msguniq-option)
:   Do not write ‘`#: filename:line`’ lines.

‘`-n`’ [¶](#index-_002dn_002c-msguniq-option)

‘`--add-location=type`’
:   Generate ‘`#: filename:line`’ lines (default).

    The optional type can be either ‘`full`’, ‘`file`’, or
    ‘`never`’. If it is not given or ‘`full`’, it generates the
    lines with both file name and line number. If it is ‘`file`’, the
    line number part is omitted. If it is ‘`never`’, it completely
    suppresses the lines (same as `--no-location`).

‘`--strict`’ [¶](#index-_002d_002dstrict_002c-msguniq-option)
:   Write out a strict Uniforum conforming PO file. Note that this
    Uniforum format should be avoided because it doesn’t support the
    GNU extensions.

‘`-p`’ [¶](#index-_002dp_002c-msguniq-option)

‘`--properties-output`’
:   Write out a Java ResourceBundle in Java `.properties` syntax. Note
    that this file format doesn’t support plural forms and silently drops
    obsolete messages.

‘`--stringtable-output`’ [¶](#index-_002d_002dstringtable_002doutput_002c-msguniq-option)
:   Write out a NeXTstep/GNUstep localized resource file in `.strings` syntax.
    Note that this file format doesn’t support plural forms.

‘`-w number`’ [¶](#index-_002dw_002c-msguniq-option)

‘`--width=number`’
:   Set the output page width. Long strings in the output files will be
    split across multiple lines in order to ensure that each line’s width
    (= number of screen columns) is less or equal to the given number.

‘`--no-wrap`’ [¶](#index-_002d_002dno_002dwrap_002c-msguniq-option)
:   Do not break long message lines. Message lines whose width exceeds the
    output page width will not be split into several lines. Only file reference
    lines which are wider than the output page width will be split.

‘`-s`’ [¶](#index-_002ds_002c-msguniq-option)

‘`--sort-output`’
:   Generate sorted output. Note that using this option makes it much harder
    for the translator to understand each message’s context.

‘`-F`’ [¶](#index-_002dF_002c-msguniq-option)

‘`--sort-by-file`’
:   Sort output by file location.

#### 10.5.6 Informative output [¶](#Informative-output-9)

‘`-h`’ [¶](#index-_002dh_002c-msguniq-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-msguniq-option)

‘`--version`’
:   Output version information and exit.

---

Next: [Invoking the `msgcmp` Program](#msgcmp-Invocation), Previous: [Invoking the `msguniq` Program](#msguniq-Invocation), Up: [Manipulating PO Files](#Manipulating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 10.6 Invoking the `msgcomm` Program [¶](#Invoking-the-msgcomm-Program)

```
msgcomm [option] [inputfile]...
```

The `msgcomm` program finds messages which are common to two or more
of the specified PO files.
By using the `--more-than` option, greater commonality may be requested
before messages are printed. Conversely, the `--less-than` option may be
used to specify less commonality before messages are printed (i.e.
‘`--less-than=2`’ will only print the unique messages). Translations,
comments and extracted comments will be preserved, but only from the first
PO file to define them. File positions from all PO files will be
cumulated.

* [Input file location](#Input-file-location-9)
* [Output file location](#Output-file-location-9)
* [Message selection](#Message-selection-4)
* [Input file syntax](#Input-file-syntax-8)
* [Output details](#Output-details-9)
* [Informative output](#Informative-output-10)

#### 10.6.1 Input file location [¶](#Input-file-location-9)

‘`inputfile …`’
:   Input files.

‘`-f file`’ [¶](#index-_002df_002c-msgcomm-option)

‘`--files-from=file`’
:   Read the names of the input files from file instead of getting
    them from the command line.

‘`-D directory`’ [¶](#index-_002dD_002c-msgcomm-option)

‘`--directory=directory`’
:   Add directory to the list of directories. Source files are
    searched relative to this list of directories. The resulting `.po`
    file will be written relative to the current directory, though.

If inputfile is ‘`-`’, standard input is read.

#### 10.6.2 Output file location [¶](#Output-file-location-9)

‘`-o file`’ [¶](#index-_002do_002c-msgcomm-option)

‘`--output-file=file`’
:   Write output to specified file.

The results are written to standard output if no output file is specified
or if it is ‘`-`’.

#### 10.6.3 Message selection [¶](#Message-selection-4)

‘`-< number`’ [¶](#index-_002d_003c_002c-msgcomm-option)

‘`--less-than=number`’
:   Print messages with less than number definitions, defaults to infinite
    if not set.

‘`-> number`’ [¶](#index-_002d_003e_002c-msgcomm-option)

‘`--more-than=number`’
:   Print messages with more than number definitions, defaults to 1 if not
    set.

‘`-u`’ [¶](#index-_002du_002c-msgcomm-option)

‘`--unique`’
:   Shorthand for ‘`--less-than=2`’. Requests that only unique messages be
    printed.

#### 10.6.4 Input file syntax [¶](#Input-file-syntax-8)

‘`-P`’ [¶](#index-_002dP_002c-msgcomm-option)

‘`--properties-input`’
:   Assume the input files are Java ResourceBundles in Java `.properties`
    syntax, not in PO file syntax.

‘`--stringtable-input`’ [¶](#index-_002d_002dstringtable_002dinput_002c-msgcomm-option)
:   Assume the input files are NeXTstep/GNUstep localized resource files in
    `.strings` syntax, not in PO file syntax.

#### 10.6.5 Output details [¶](#Output-details-9)

‘`--color`’ [¶](#index-_002d_002dcolor_002c-msgcomm-option)

‘`--color=when`’
:   Specify whether or when to use colors and other text attributes.
    See [The `--color` option](#The-_002d_002dcolor-option) for details.

‘`--style=style_file`’ [¶](#index-_002d_002dstyle_002c-msgcomm-option)
:   Specify the CSS style rule file to use for `--color`.
    See [The `--style` option](#The-_002d_002dstyle-option) for details.

‘`--force-po`’ [¶](#index-_002d_002dforce_002dpo_002c-msgcomm-option)
:   Always write an output file even if it contains no message.

‘`-i`’ [¶](#index-_002di_002c-msgcomm-option)

‘`--indent`’
:   Write the .po file using indented style.

‘`--no-location`’ [¶](#index-_002d_002dno_002dlocation_002c-msgcomm-option)
:   Do not write ‘`#: filename:line`’ lines.

‘`-n`’ [¶](#index-_002dn_002c-msgcomm-option)

‘`--add-location=type`’
:   Generate ‘`#: filename:line`’ lines (default).

    The optional type can be either ‘`full`’, ‘`file`’, or
    ‘`never`’. If it is not given or ‘`full`’, it generates the
    lines with both file name and line number. If it is ‘`file`’, the
    line number part is omitted. If it is ‘`never`’, it completely
    suppresses the lines (same as `--no-location`).

‘`--strict`’ [¶](#index-_002d_002dstrict_002c-msgcomm-option)
:   Write out a strict Uniforum conforming PO file. Note that this
    Uniforum format should be avoided because it doesn’t support the
    GNU extensions.

‘`-p`’ [¶](#index-_002dp_002c-msgcomm-option)

‘`--properties-output`’
:   Write out a Java ResourceBundle in Java `.properties` syntax. Note
    that this file format doesn’t support plural forms and silently drops
    obsolete messages.

‘`--stringtable-output`’ [¶](#index-_002d_002dstringtable_002doutput_002c-msgcomm-option)
:   Write out a NeXTstep/GNUstep localized resource file in `.strings` syntax.
    Note that this file format doesn’t support plural forms.

‘`-w number`’ [¶](#index-_002dw_002c-msgcomm-option)

‘`--width=number`’
:   Set the output page width. Long strings in the output files will be
    split across multiple lines in order to ensure that each line’s width
    (= number of screen columns) is less or equal to the given number.

‘`--no-wrap`’ [¶](#index-_002d_002dno_002dwrap_002c-msgcomm-option)
:   Do not break long message lines. Message lines whose width exceeds the
    output page width will not be split into several lines. Only file reference
    lines which are wider than the output page width will be split.

‘`-s`’ [¶](#index-_002ds_002c-msgcomm-option)

‘`--sort-output`’
:   Generate sorted output. Note that using this option makes it much harder
    for the translator to understand each message’s context.

‘`-F`’ [¶](#index-_002dF_002c-msgcomm-option)

‘`--sort-by-file`’
:   Sort output by file location.

‘`--omit-header`’ [¶](#index-_002d_002domit_002dheader_002c-msgcomm-option)
:   Don’t write header with ‘`msgid ""`’ entry.
    Note: Using this option may lead to an error in subsequent operations
    if the output contains non-ASCII characters.

#### 10.6.6 Informative output [¶](#Informative-output-10)

‘`-h`’ [¶](#index-_002dh_002c-msgcomm-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-msgcomm-option)

‘`--version`’
:   Output version information and exit.

---

Next: [Invoking the `msgattrib` Program](#msgattrib-Invocation), Previous: [Invoking the `msgcomm` Program](#msgcomm-Invocation), Up: [Manipulating PO Files](#Manipulating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 10.7 Invoking the `msgcmp` Program [¶](#Invoking-the-msgcmp-Program)

```
msgcmp [option] def.po ref.pot
```

The `msgcmp` program compares two Uniforum style .po files to check that
both contain the same set of msgid strings. The def.po file is an
existing PO file with the translations. The ref.pot file is the last
created PO file, or a PO Template file (generally created by `xgettext`).
This is useful for checking that you have translated each and every message
in your program. Where an exact match cannot be found, fuzzy matching is
used to produce better diagnostics.

* [Input file location](#Input-file-location-10)
* [Operation modifiers](#Operation-modifiers-1)
* [Input file syntax](#Input-file-syntax-9)
* [Informative output](#Informative-output-11)

#### 10.7.1 Input file location [¶](#Input-file-location-10)

‘`def.po`’
:   Translations.

‘`ref.pot`’
:   References to the sources.

‘`-D directory`’ [¶](#index-_002dD_002c-msgcmp-option)

‘`--directory=directory`’
:   Add directory to the list of directories. Source files are
    searched relative to this list of directories.

#### 10.7.2 Operation modifiers [¶](#Operation-modifiers-1)

‘`-m`’ [¶](#index-_002dm_002c-msgcmp-option)

‘`--multi-domain`’
:   Apply ref.pot to each of the domains in def.po.

‘`-N`’ [¶](#index-_002dN_002c-msgcmp-option)

‘`--no-fuzzy-matching`’
:   Do not use fuzzy matching when an exact match is not found. This may speed
    up the operation considerably.

‘`--use-fuzzy`’ [¶](#index-_002d_002duse_002dfuzzy_002c-msgcmp-option)
:   Consider fuzzy messages in the def.po file like translated messages.
    Note that using this option is usually wrong, because fuzzy messages are
    exactly those which have not been validated by a human translator.

‘`--use-untranslated`’ [¶](#index-_002d_002duse_002duntranslated_002c-msgcmp-option)
:   Consider untranslated messages in the def.po file like translated
    messages. Note that using this option is usually wrong.

#### 10.7.3 Input file syntax [¶](#Input-file-syntax-9)

‘`-P`’ [¶](#index-_002dP_002c-msgcmp-option)

‘`--properties-input`’
:   Assume the input files are Java ResourceBundles in Java `.properties`
    syntax, not in PO file syntax.

‘`--stringtable-input`’ [¶](#index-_002d_002dstringtable_002dinput_002c-msgcmp-option)
:   Assume the input files are NeXTstep/GNUstep localized resource files in
    `.strings` syntax, not in PO file syntax.

#### 10.7.4 Informative output [¶](#Informative-output-11)

‘`-h`’ [¶](#index-_002dh_002c-msgcmp-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-msgcmp-option)

‘`--version`’
:   Output version information and exit.

---

Next: [Invoking the `msgen` Program](#msgen-Invocation), Previous: [Invoking the `msgcmp` Program](#msgcmp-Invocation), Up: [Manipulating PO Files](#Manipulating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 10.8 Invoking the `msgattrib` Program [¶](#Invoking-the-msgattrib-Program)

```
msgattrib [option] [inputfile]
```

The `msgattrib` program filters the messages of a translation catalog
according to their attributes, and manipulates the attributes.

* [Input file location](#Input-file-location-11)
* [Output file location](#Output-file-location-10)
* [Message selection](#Message-selection-5)
* [Attribute manipulation](#Attribute-manipulation)
* [Input file syntax](#Input-file-syntax-10)
* [Output details](#Output-details-10)
* [Informative output](#Informative-output-12)

#### 10.8.1 Input file location [¶](#Input-file-location-11)

‘`inputfile`’
:   Input PO file.

‘`-D directory`’ [¶](#index-_002dD_002c-msgattrib-option)

‘`--directory=directory`’
:   Add directory to the list of directories. Source files are
    searched relative to this list of directories. The resulting `.po`
    file will be written relative to the current directory, though.

If no inputfile is given or if it is ‘`-`’, standard input is read.

#### 10.8.2 Output file location [¶](#Output-file-location-10)

‘`-o file`’ [¶](#index-_002do_002c-msgattrib-option)

‘`--output-file=file`’
:   Write output to specified file.

The results are written to standard output if no output file is specified
or if it is ‘`-`’.

#### 10.8.3 Message selection [¶](#Message-selection-5)

‘`--translated`’ [¶](#index-_002d_002dtranslated_002c-msgattrib-option)
:   Keep translated messages, remove untranslated messages.

‘`--untranslated`’ [¶](#index-_002d_002duntranslated_002c-msgattrib-option)
:   Keep untranslated messages, remove translated messages.

‘`--no-fuzzy`’ [¶](#index-_002d_002dno_002dfuzzy_002c-msgattrib-option)
:   Remove
    ‘fuzzy’
    marked messages.

‘`--only-fuzzy`’ [¶](#index-_002d_002donly_002dfuzzy_002c-msgattrib-option)
:   Keep
    ‘fuzzy’
    marked messages, remove all other messages.

‘`--no-obsolete`’ [¶](#index-_002d_002dno_002dobsolete_002c-msgattrib-option)
:   Remove obsolete #~ messages.

‘`--only-obsolete`’ [¶](#index-_002d_002donly_002dobsolete_002c-msgattrib-option)
:   Keep obsolete #~ messages, remove all other messages.

#### 10.8.4 Attribute manipulation [¶](#Attribute-manipulation)

Attributes are modified after the message selection/removal has been
performed. If the ‘`--only-file`’ or ‘`--ignore-file`’ option is
specified, the attribute modification is applied only to those messages
that are listed in the only-file and not listed in the
ignore-file.

‘`--set-fuzzy`’ [¶](#index-_002d_002dset_002dfuzzy_002c-msgattrib-option)
:   Set all messages
    ‘fuzzy’.

‘`--clear-fuzzy`’ [¶](#index-_002d_002dclear_002dfuzzy_002c-msgattrib-option)
:   Set all messages
    non-‘fuzzy’.

‘`--set-obsolete`’ [¶](#index-_002d_002dset_002dobsolete_002c-msgattrib-option)
:   Set all messages obsolete.

‘`--clear-obsolete`’ [¶](#index-_002d_002dclear_002dobsolete_002c-msgattrib-option)
:   Set all messages non-obsolete.

‘`--previous`’ [¶](#index-_002d_002dprevious_002c-msgattrib-option)
:   When setting
    ‘fuzzy’
    mark, keep “previous msgid” of translated messages.

‘`--clear-previous`’ [¶](#index-_002d_002dclear_002dprevious_002c-msgattrib-option)
:   Remove the “previous msgid” (‘`#|`’) comments from all messages.

‘`--empty`’ [¶](#index-_002d_002dempty_002c-msgattrib-option)
:   When removing
    ‘fuzzy’
    mark, also set msgstr empty.

‘`--only-file=file`’ [¶](#index-_002d_002donly_002dfile_002c-msgattrib-option)
:   Limit the attribute changes to entries that are listed in file.
    file should be a PO or POT file.

‘`--ignore-file=file`’ [¶](#index-_002d_002dignore_002dfile_002c-msgattrib-option)
:   Limit the attribute changes to entries that are not listed in file.
    file should be a PO or POT file.

‘`--fuzzy`’ [¶](#index-_002d_002dfuzzy_002c-msgattrib-option)
:   Synonym for ‘`--only-fuzzy --clear-fuzzy`’: It keeps only the fuzzy
    messages and removes their
    ‘fuzzy’
    mark.

‘`--obsolete`’ [¶](#index-_002d_002dobsolete_002c-msgattrib-option)
:   Synonym for ‘`--only-obsolete --clear-obsolete`’: It keeps only the
    obsolete messages and makes them non-obsolete.

#### 10.8.5 Input file syntax [¶](#Input-file-syntax-10)

‘`-P`’ [¶](#index-_002dP_002c-msgattrib-option)

‘`--properties-input`’
:   Assume the input file is a Java ResourceBundle in Java `.properties`
    syntax, not in PO file syntax.

‘`--stringtable-input`’ [¶](#index-_002d_002dstringtable_002dinput_002c-msgattrib-option)
:   Assume the input file is a NeXTstep/GNUstep localized resource file in
    `.strings` syntax, not in PO file syntax.

#### 10.8.6 Output details [¶](#Output-details-10)

‘`--color`’ [¶](#index-_002d_002dcolor_002c-msgattrib-option)

‘`--color=when`’
:   Specify whether or when to use colors and other text attributes.
    See [The `--color` option](#The-_002d_002dcolor-option) for details.

‘`--style=style_file`’ [¶](#index-_002d_002dstyle_002c-msgattrib-option)
:   Specify the CSS style rule file to use for `--color`.
    See [The `--style` option](#The-_002d_002dstyle-option) for details.

‘`--force-po`’ [¶](#index-_002d_002dforce_002dpo_002c-msgattrib-option)
:   Always write an output file even if it contains no message.

‘`-i`’ [¶](#index-_002di_002c-msgattrib-option)

‘`--indent`’
:   Write the .po file using indented style.

‘`--no-location`’ [¶](#index-_002d_002dno_002dlocation_002c-msgattrib-option)
:   Do not write ‘`#: filename:line`’ lines.

‘`-n`’ [¶](#index-_002dn_002c-msgattrib-option)

‘`--add-location=type`’
:   Generate ‘`#: filename:line`’ lines (default).

    The optional type can be either ‘`full`’, ‘`file`’, or
    ‘`never`’. If it is not given or ‘`full`’, it generates the
    lines with both file name and line number. If it is ‘`file`’, the
    line number part is omitted. If it is ‘`never`’, it completely
    suppresses the lines (same as `--no-location`).

‘`--strict`’ [¶](#index-_002d_002dstrict_002c-msgattrib-option)
:   Write out a strict Uniforum conforming PO file. Note that this
    Uniforum format should be avoided because it doesn’t support the
    GNU extensions.

‘`-p`’ [¶](#index-_002dp_002c-msgattrib-option)

‘`--properties-output`’
:   Write out a Java ResourceBundle in Java `.properties` syntax. Note
    that this file format doesn’t support plural forms and silently drops
    obsolete messages.

‘`--stringtable-output`’ [¶](#index-_002d_002dstringtable_002doutput_002c-msgattrib-option)
:   Write out a NeXTstep/GNUstep localized resource file in `.strings` syntax.
    Note that this file format doesn’t support plural forms.

‘`-w number`’ [¶](#index-_002dw_002c-msgattrib-option)

‘`--width=number`’
:   Set the output page width. Long strings in the output files will be
    split across multiple lines in order to ensure that each line’s width
    (= number of screen columns) is less or equal to the given number.

‘`--no-wrap`’ [¶](#index-_002d_002dno_002dwrap_002c-msgattrib-option)
:   Do not break long message lines. Message lines whose width exceeds the
    output page width will not be split into several lines. Only file reference
    lines which are wider than the output page width will be split.

‘`-s`’ [¶](#index-_002ds_002c-msgattrib-option)

‘`--sort-output`’
:   Generate sorted output. Note that using this option makes it much harder
    for the translator to understand each message’s context.

‘`-F`’ [¶](#index-_002dF_002c-msgattrib-option)

‘`--sort-by-file`’
:   Sort output by file location.

#### 10.8.7 Informative output [¶](#Informative-output-12)

‘`-h`’ [¶](#index-_002dh_002c-msgattrib-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-msgattrib-option)

‘`--version`’
:   Output version information and exit.

---

Next: [Invoking the `msgexec` Program](#msgexec-Invocation), Previous: [Invoking the `msgattrib` Program](#msgattrib-Invocation), Up: [Manipulating PO Files](#Manipulating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 10.9 Invoking the `msgen` Program [¶](#Invoking-the-msgen-Program)

```
msgen [option] inputfile
```

The `msgen` program creates an English translation catalog. The
input file is the last created English PO file, or a PO Template file
(generally created by xgettext). Untranslated entries are assigned a
translation that is identical to the msgid.

Note: ‘`msginit --no-translator --locale=en`’ performs a very similar
task. The main difference is that `msginit` cares specially about
the header entry, whereas `msgen` doesn’t.

* [Input file location](#Input-file-location-12)
* [Output file location](#Output-file-location-11)
* [Input file syntax](#Input-file-syntax-11)
* [Output details](#Output-details-11)
* [Informative output](#Informative-output-13)

#### 10.9.1 Input file location [¶](#Input-file-location-12)

‘`inputfile`’
:   Input PO or POT file.

‘`-D directory`’ [¶](#index-_002dD_002c-msgen-option)

‘`--directory=directory`’
:   Add directory to the list of directories. Source files are
    searched relative to this list of directories. The resulting `.po`
    file will be written relative to the current directory, though.

If inputfile is ‘`-`’, standard input is read.

#### 10.9.2 Output file location [¶](#Output-file-location-11)

‘`-o file`’ [¶](#index-_002do_002c-msgen-option)

‘`--output-file=file`’
:   Write output to specified file.

The results are written to standard output if no output file is specified
or if it is ‘`-`’.

#### 10.9.3 Input file syntax [¶](#Input-file-syntax-11)

‘`-P`’ [¶](#index-_002dP_002c-msgen-option)

‘`--properties-input`’
:   Assume the input file is a Java ResourceBundle in Java `.properties`
    syntax, not in PO file syntax.

‘`--stringtable-input`’ [¶](#index-_002d_002dstringtable_002dinput_002c-msgen-option)
:   Assume the input file is a NeXTstep/GNUstep localized resource file in
    `.strings` syntax, not in PO file syntax.

#### 10.9.4 Output details [¶](#Output-details-11)

‘`--lang=catalogname`’ [¶](#index-_002d_002dlang_002c-msgen-option)
:   Specify the ‘`Language`’ field to be used in the header entry. See
    [Filling in the Header Entry](#Header-Entry) for the meaning of this field. Note: The
    ‘`Language-Team`’ and ‘`Plural-Forms`’ fields are not set by this
    option.

‘`--color`’ [¶](#index-_002d_002dcolor_002c-msgen-option)

‘`--color=when`’
:   Specify whether or when to use colors and other text attributes.
    See [The `--color` option](#The-_002d_002dcolor-option) for details.

‘`--style=style_file`’ [¶](#index-_002d_002dstyle_002c-msgen-option)
:   Specify the CSS style rule file to use for `--color`.
    See [The `--style` option](#The-_002d_002dstyle-option) for details.

‘`--force-po`’ [¶](#index-_002d_002dforce_002dpo_002c-msgen-option)
:   Always write an output file even if it contains no message.

‘`-i`’ [¶](#index-_002di_002c-msgen-option)

‘`--indent`’
:   Write the .po file using indented style.

‘`--no-location`’ [¶](#index-_002d_002dno_002dlocation_002c-msgen-option)
:   Do not write ‘`#: filename:line`’ lines.

‘`-n`’ [¶](#index-_002d_002dadd_002dlocation_002c-msgen-option)

‘`--add-location=type`’
:   Generate ‘`#: filename:line`’ lines (default).

    The optional type can be either ‘`full`’, ‘`file`’, or
    ‘`never`’. If it is not given or ‘`full`’, it generates the
    lines with both file name and line number. If it is ‘`file`’, the
    line number part is omitted. If it is ‘`never`’, it completely
    suppresses the lines (same as `--no-location`).

‘`--strict`’ [¶](#index-_002d_002dstrict_002c-msgen-option)
:   Write out a strict Uniforum conforming PO file. Note that this
    Uniforum format should be avoided because it doesn’t support the
    GNU extensions.

‘`-p`’ [¶](#index-_002dp_002c-msgen-option)

‘`--properties-output`’
:   Write out a Java ResourceBundle in Java `.properties` syntax. Note
    that this file format doesn’t support plural forms and silently drops
    obsolete messages.

‘`--stringtable-output`’ [¶](#index-_002d_002dstringtable_002doutput_002c-msgen-option)
:   Write out a NeXTstep/GNUstep localized resource file in `.strings` syntax.
    Note that this file format doesn’t support plural forms.

‘`-w number`’ [¶](#index-_002dw_002c-msgen-option)

‘`--width=number`’
:   Set the output page width. Long strings in the output files will be
    split across multiple lines in order to ensure that each line’s width
    (= number of screen columns) is less or equal to the given number.

‘`--no-wrap`’ [¶](#index-_002d_002dno_002dwrap_002c-msgen-option)
:   Do not break long message lines. Message lines whose width exceeds the
    output page width will not be split into several lines. Only file reference
    lines which are wider than the output page width will be split.

‘`-s`’ [¶](#index-_002ds_002c-msgen-option)

‘`--sort-output`’
:   Generate sorted output. Note that using this option makes it much harder
    for the translator to understand each message’s context.

‘`-F`’ [¶](#index-_002dF_002c-msgen-option)

‘`--sort-by-file`’
:   Sort output by file location.

#### 10.9.5 Informative output [¶](#Informative-output-13)

‘`-h`’ [¶](#index-_002dh_002c-msgen-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-msgen-option)

‘`--version`’
:   Output version information and exit.

---

Next: [Highlighting parts of PO files](#Colorizing), Previous: [Invoking the `msgen` Program](#msgen-Invocation), Up: [Manipulating PO Files](#Manipulating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 10.10 Invoking the `msgexec` Program [¶](#Invoking-the-msgexec-Program)

```
msgexec [option] command [command-option]
```

The `msgexec` program applies a command to all translations of a
translation catalog.
The command can be any program that reads a translation from standard
input. It is invoked once for each translation. Its output becomes
msgexec’s output. `msgexec`’s return code is the maximum return code
across all invocations.

A special builtin command called ‘`0`’ outputs the translation, followed
by a null byte. The output of ‘`msgexec 0`’ is suitable as input for
‘`xargs -0`’.

‘`--newline`’ [¶](#index-_002d_002dnewline_002c-msgfilter-option-1)
:   Add newline at the end of each input line.

During each command invocation, the environment variable
`MSGEXEC_MSGID` is bound to the message’s msgid, and the environment
variable `MSGEXEC_LOCATION` is bound to the location in the PO file
of the message. If the message has a context, the environment variable
`MSGEXEC_MSGCTXT` is bound to the message’s msgctxt, otherwise it is
unbound. If the message has a plural form, environment variable
`MSGEXEC_MSGID_PLURAL` is bound to the message’s msgid\_plural and
`MSGEXEC_PLURAL_FORM` is bound to the order number of the plural
actually processed (starting with 0), otherwise both are unbound.
If the message has a previous msgid (added by `msgmerge`),
environment variable `MSGEXEC_PREV_MSGCTXT` is bound to the
message’s previous msgctxt, `MSGEXEC_PREV_MSGID` is bound to
the previous msgid, and `MSGEXEC_PREV_MSGID_PLURAL` is bound to
the previous msgid\_plural.

Note: It is your responsibility to ensure that the command can cope
with input encoded in the translation catalog’s encoding. If the
command wants input in a particular encoding, you can in a first step
convert the translation catalog to that encoding using the ‘`msgconv`’
program, before invoking ‘`msgexec`’. If the command wants input
in the locale’s encoding, but you want to avoid the locale’s encoding, then
you can first convert the translation catalog to UTF-8 using the
‘`msgconv`’ program and then make ‘`msgexec`’ work in an UTF-8
locale, by using the `LC_ALL` environment variable.

* [Input file location](#Input-file-location-13)
* [Input file syntax](#Input-file-syntax-12)
* [Informative output](#Informative-output-14)

#### 10.10.1 Input file location [¶](#Input-file-location-13)

‘`-i inputfile`’ [¶](#index-_002di_002c-msgexec-option)

‘`--input=inputfile`’
:   Input PO file.

‘`-D directory`’ [¶](#index-_002dD_002c-msgexec-option)

‘`--directory=directory`’
:   Add directory to the list of directories. Source files are
    searched relative to this list of directories. The resulting `.po`
    file will be written relative to the current directory, though.

If no inputfile is given or if it is ‘`-`’, standard input is read.

#### 10.10.2 Input file syntax [¶](#Input-file-syntax-12)

‘`-P`’ [¶](#index-_002dP_002c-msgexec-option)

‘`--properties-input`’
:   Assume the input file is a Java ResourceBundle in Java `.properties`
    syntax, not in PO file syntax.

‘`--stringtable-input`’ [¶](#index-_002d_002dstringtable_002dinput_002c-msgexec-option)
:   Assume the input file is a NeXTstep/GNUstep localized resource file in
    `.strings` syntax, not in PO file syntax.

#### 10.10.3 Informative output [¶](#Informative-output-14)

‘`-h`’ [¶](#index-_002dh_002c-msgexec-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-msgexec-option)

‘`--version`’
:   Output version information and exit.

---

Next: [Other tools for manipulating PO files](#Other-tools), Previous: [Invoking the `msgexec` Program](#msgexec-Invocation), Up: [Manipulating PO Files](#Manipulating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 10.11 Highlighting parts of PO files [¶](#Highlighting-parts-of-PO-files)

Translators are usually only interested in seeing the untranslated and
fuzzy messages of a PO file. Also, when a message is set fuzzy because
the msgid changed, they want to see the differences between the previous
msgid and the current one (especially if the msgid is long and only few
words in it have changed). Finally, it’s always welcome to highlight the
different sections of a message in a PO file (comments, msgid, msgstr, etc.).

Such highlighting is possible through the options ‘`--color`’ and
‘`--style`’. They are supported by all the programs that produce
a PO file on standard output, such as `msgcat`, `msgmerge`,
and `msgunfmt`.

* [The `--color` option](#The-_002d_002dcolor-option)
* [The environment variable `TERM`](#The-TERM-variable)
* [The `--style` option](#The-_002d_002dstyle-option)
* [Style rules for PO files](#Style-rules)
* [Customizing `less` for viewing PO files](#Customizing-less)

---

Next: [The environment variable `TERM`](#The-TERM-variable), Up: [Highlighting parts of PO files](#Colorizing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 10.11.1 The `--color` option [¶](#The-_002d_002dcolor-option-1)

The ‘`--color=when`’ option specifies under which conditions
colorized output should be generated. The when part can be one of
the following:

`always`

`yes`
:   The output will be colorized.

`never`

`no`
:   The output will not be colorized.

`auto`

`tty`
:   The output will be colorized if the output device is a tty, i.e. when the
    output goes directly to a text screen or terminal emulator window.

`html`
:   The output will be colorized and be in HTML format.

`test`
:   This is a special value, understood only by the `msgcat` program. It
    is explained in the next section ([The environment variable `TERM`](#The-TERM-variable)).

‘`--color`’ is equivalent to ‘`--color=yes`’. The default is
‘`--color=auto`’.

Thus, a command like ‘`msgcat vi.po`’ will produce colorized output
when called by itself in a command window. Whereas in a pipe, such as
‘`msgcat vi.po | less -R`’, it will not produce colorized output. To
get colorized output in this situation nevertheless, use the command
‘`msgcat --color vi.po | less -R`’.

The ‘`--color=html`’ option will produce output that can be viewed in
a browser. This can be useful, for example, for Indic languages,
because the renderic of Indic scripts in browsers is usually better than
in terminal emulators.

Note that the output produced with the `--color` option is *not*
a valid PO file in itself. It contains additional terminal-specific escape
sequences or HTML tags. A PO file reader will give a syntax error when
confronted with such content. Except for the ‘`--color=html`’ case,
you therefore normally don’t need to save output produced with the
`--color` option in a file.

---

Next: [The `--style` option](#The-_002d_002dstyle-option), Previous: [The `--color` option](#The-_002d_002dcolor-option), Up: [Highlighting parts of PO files](#Colorizing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 10.11.2 The environment variable `TERM` [¶](#The-environment-variable-TERM)

The environment variable `TERM` contains a identifier for the text
window’s capabilities. You can get a detailed list of these capabilities
by using the ‘`infocmp`’ command, using ‘`man 5 terminfo`’ as a
reference.

When producing text with embedded color directives, `msgcat` looks
at the `TERM` variable. Text windows today typically support at least
8 colors. Often, however, the text window supports 16 or more colors,
even though the `TERM` variable is set to a identifier denoting only
8 supported colors. It can be worth setting the `TERM` variable to
a different value in these cases:

`xterm`
:   `xterm` is in most cases built with support for 16 colors. It can also
    be built with support for 88 or 256 colors (but not both). You can try to
    set `TERM` to either `xterm-16color`, `xterm-88color`, or
    `xterm-256color`.

`rxvt`
:   `rxvt` is often built with support for 16 colors. You can try to set
    `TERM` to `rxvt-16color`.

`konsole`
:   `konsole` too is often built with support for 16 colors. You can try to
    set `TERM` to `konsole-16color` or `xterm-16color`.

After setting `TERM`, you can verify it by invoking
‘`msgcat --color=test`’ and seeing whether the output looks like a
reasonable color map.

---

Next: [Style rules for PO files](#Style-rules), Previous: [The environment variable `TERM`](#The-TERM-variable), Up: [Highlighting parts of PO files](#Colorizing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 10.11.3 The `--style` option [¶](#The-_002d_002dstyle-option-1)

The ‘`--style=style_file`’ option specifies the style file to use
when colorizing. It has an effect only when the `--color` option is
effective.

If the `--style` option is not specified, the environment variable
`PO_STYLE` is considered. It is meant to point to the user’s
preferred style for PO files.

The default style file is `$prefix/share/gettext/styles/po-default.css`,
where `$prefix` is the installation location.

A few style files are predefined:

`po-vim.css`
:   This style imitates the look used by vim 7.

`po-emacs-x.css`
:   This style imitates the look used by GNU Emacs 21 and 22 in an X11 window.

`po-emacs-xterm.css`

`po-emacs-xterm16.css`

`po-emacs-xterm256.css`
:   This style imitates the look used by GNU Emacs 22 in a terminal of type
    ‘`xterm`’ (8 colors) or ‘`xterm-16color`’ (16 colors) or
    ‘`xterm-256color`’ (256 colors), respectively.

You can use these styles without specifying a directory. They are actually
located in `$prefix/share/gettext/styles/`, where `$prefix` is the
installation location.

You can also design your own styles. This is described in the next section.

---

Next: [Customizing `less` for viewing PO files](#Customizing-less), Previous: [The `--style` option](#The-_002d_002dstyle-option), Up: [Highlighting parts of PO files](#Colorizing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 10.11.4 Style rules for PO files [¶](#Style-rules-for-PO-files)

The same style file can be used for styling of a PO file, for terminal
output and for HTML output. It is written in CSS (Cascading Style Sheet)
syntax. See <https://www.w3.org/TR/css2/cover.html> for a formal
definition of CSS. Many HTML authoring tutorials also contain explanations
of CSS.

In the case of HTML output, the style file is embedded in the HTML output.
In the case of text output, the style file is interpreted by the
`msgcat` program. This means, in particular, that when
`@import` is used with relative file names, the file names are

* relative to the resulting HTML file, in the case of HTML output,
* relative to the style sheet containing the `@import`, in the case of
  text output. (Actually, `@import`s are not yet supported in this case,
  due to a limitation in `libcroco`.)

CSS rules are built up from selectors and declarations. The declarations
specify graphical properties; the selectors specify when they apply.

In PO files, the following simple selectors (based on "CSS classes", see
the CSS2 spec, section 5.8.3) are supported.

* Selectors that apply to entire messages:

  `.header`
  :   This matches the header entry of a PO file.

  `.translated`
  :   This matches a translated message.

  `.untranslated`
  :   This matches an untranslated message (i.e. a message with empty translation).

  `.fuzzy`
  :   This matches a fuzzy message (i.e. a message which has a translation that
      needs review by the translator).

  `.obsolete`
  :   This matches an obsolete message (i.e. a message that was translated but is
      not needed by the current POT file any more).
* Selectors that apply to parts of a message in PO syntax. Recall the general
  structure of a message in PO syntax:

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

  `.comment`
  :   This matches all comments (translator comments, extracted comments,
      source file reference comments, flag comments, previous message comments,
      as well as the entire obsolete messages).

  `.translator-comment`
  :   This matches the translator comments.

  `.extracted-comment`
  :   This matches the extracted comments, i.e. the comments placed by the
      programmer at the attention of the translator.

  `.reference-comment`
  :   This matches the source file reference comments (entire lines).

  `.reference`
  :   This matches the individual source file references inside the source file
      reference comment lines.

  `.flag-comment`
  :   This matches the flag comment lines (entire lines).

  `.flag`
  :   This matches the individual flags inside flag comment lines.

  `.fuzzy-flag`
  :   This matches the ‘fuzzy’ flag inside flag comment lines.

  `.previous-comment`
  :   This matches the comments containing the previous untranslated string (entire
      lines).

  `.previous`
  :   This matches the previous untranslated string including the string delimiters,
      the associated keywords (`msgid` etc.) and the spaces between them.

  `.msgid`
  :   This matches the untranslated string including the string delimiters,
      the associated keywords (`msgid` etc.) and the spaces between them.

  `.msgstr`
  :   This matches the translated string including the string delimiters,
      the associated keywords (`msgstr` etc.) and the spaces between them.

  `.keyword`
  :   This matches the keywords (`msgid`, `msgstr`, etc.).

  `.string`
  :   This matches strings, including the string delimiters (double quotes).
* Selectors that apply to parts of strings:

  `.text`
  :   This matches the entire contents of a string (excluding the string delimiters,
      i.e. the double quotes).

  `.escape-sequence`
  :   This matches an escape sequence (starting with a backslash).

  `.format-directive`
  :   This matches a format string directive (starting with a ‘`%`’ sign in the
      case of most programming languages, with a ‘`{`’ in the case of
      `java-format` and `csharp-format`, with a ‘`~`’ in the case of
      `lisp-format` and `scheme-format`, or with ‘`$`’ in the case of
      `sh-format`).

  `.invalid-format-directive`
  :   This matches an invalid format string directive.

  `.added`
  :   In an untranslated string, this matches a part of the string that was not
      present in the previous untranslated string. (Not yet implemented in this
      release.)

  `.changed`
  :   In an untranslated string or in a previous untranslated string, this matches
      a part of the string that is changed or replaced. (Not yet implemented in
      this release.)

  `.removed`
  :   In a previous untranslated string, this matches a part of the string that
      is not present in the current untranslated string. (Not yet implemented in
      this release.)

These selectors can be combined to hierarchical selectors. For example,

```
.msgstr .invalid-format-directive { color: red; }
```

will highlight the invalid format directives in the translated strings.

In text mode, pseudo-classes (CSS2 spec, section 5.11) and pseudo-elements
(CSS2 spec, section 5.12) are not supported.

The declarations in HTML mode are not limited; any graphical attribute
supported by the browsers can be used.

The declarations in text mode are limited to the following properties. Other
properties will be silently ignored.

`color` (CSS2 spec, section 14.1)

`background-color` (CSS2 spec, section 14.2.1)
:   These properties is supported. Colors will be adjusted to match the terminal’s
    capabilities. Note that many terminals support only 8 colors.

`font-weight` (CSS2 spec, section 15.2.3)
:   This property is supported, but most terminals can only render two different
    weights: `normal` and `bold`. Values >= 600 are rendered as
    `bold`.

`font-style` (CSS2 spec, section 15.2.3)
:   This property is supported. The values `italic` and `oblique` are
    rendered the same way.

`text-decoration` (CSS2 spec, section 16.3.1)
:   This property is supported, limited to the values `none` and
    `underline`.

---

Previous: [Style rules for PO files](#Style-rules), Up: [Highlighting parts of PO files](#Colorizing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 10.11.5 Customizing `less` for viewing PO files [¶](#Customizing-less-for-viewing-PO-files)

The ‘`less`’ program is a popular text file browser for use in a text
screen or terminal emulator. It also supports text with embedded escape
sequences for colors and text decorations.

You can use `less` to view a PO file like this (assuming an UTF-8
environment):

```
msgcat --to-code=UTF-8 --color xyz.po | less -R
```

You can simplify this to this simple command:

```
less xyz.po
```

after these three preparations:

1. Add the options ‘`-R`’ and ‘`-f`’ to the `LESS` environment
   variable. In sh shells:

   ```
   $ LESS="$LESS -R -f"
   $ export LESS
   ```
2. If your system does not already have the `lessopen.sh` and
   `lessclose.sh` scripts, create them and set the `LESSOPEN` and
   `LESSCLOSE` environment variables, as indicated in the manual page
   (‘`man less`’).
3. Add to `lessopen.sh` a piece of script that recognizes PO files
   through their file extension and invokes `msgcat` on them, producing
   a temporary file. Like this:

   ```
   case "$1" in
     *.po)
       tmpfile=`mktemp "${TMPDIR-/tmp}/less.XXXXXX"`
       msgcat --to-code=UTF-8 --color "$1" > "$tmpfile"
       echo "$tmpfile"
       exit 0
       ;;
   esac
   ```

---

Next: [Writing your own programs that process PO files](#libgettextpo), Previous: [Highlighting parts of PO files](#Colorizing), Up: [Manipulating PO Files](#Manipulating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 10.12 Other tools for manipulating PO files [¶](#Other-tools-for-manipulating-PO-files)

#### Pology [¶](#Pology)

The “Pology” package is a Free Software package for manipulating PO files.
It features, in particular:

* Examination and in-place modification of collections of PO files.
* Format-aware diffing and patching of PO files.
* Handling of version-control branches.
* Fine-grained asynchronous review workflow.
* Custom translation validation.
* Language and project specific support.

Its home page is at <http://pology.nedohodnik.net/>.

#### Translate Toolkit [¶](#Translate-Toolkit)

The “Translate Toolkit” is a Free Software package.
It contains a set of programs to
convert between PO files and other file formats,
merge translations,
and perform various checks.

Its home page is at <https://toolkit.translatehouse.org/>.
The code is at <https://github.com/translate/translate>.

---

Previous: [Other tools for manipulating PO files](#Other-tools), Up: [Manipulating PO Files](#Manipulating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]
