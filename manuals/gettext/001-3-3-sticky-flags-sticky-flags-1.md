### 3.3 Sticky flags [¶](#Sticky-flags-1)

Sticky flags are flags that are
set on an entry when the PO template file is created,
and that remain attached to it throughout the workflow.

There are two kinds of sticky flags:

* The `*-format` flags,
  that are inferred from the `msgid`
  and from the source code that surrounds the `msgid`.
* Other flags, assigned by the programmer.

The following `*-format` flags exist.
More can be added in future GNU gettext releases.

`c-format` [¶](#index-no_002dc_002dformat-flag)

`no-c-format`
:   These flags should not be added by a human. Instead only the
    `xgettext` program adds them. In an automated PO file processing
    system as proposed here, the user’s changes would be thrown away again as
    soon as the `xgettext` program generates a new template file.

    The `c-format` flag indicates that the untranslated string and the
    translation are supposed to be C format strings. The `no-c-format`
    flag indicates that they are not C format strings, even though the untranslated
    string happens to look like a C format string (with ‘`%`’ directives).

    When the `c-format` flag is given for a string the `msgfmt`
    program does some more tests to check the validity of the translation.
    See [Invoking the `msgfmt` Program](#msgfmt-Invocation), [Special Comments preceding Keywords](#c_002dformat-Flag) and [C Format Strings](#c_002dformat).

`objc-format` [¶](#index-no_002dobjc_002dformat-flag)

`no-objc-format`
:   Likewise for Objective C, see [Objective C Format Strings](#objc_002dformat).

`c++-format` [¶](#index-no_002dc_002b_002b_002dformat-flag)

`no-c++-format`
:   Likewise for C++, see [C++ Format Strings](#c_002b_002b_002dformat).

`python-format` [¶](#index-no_002dpython_002dformat-flag)

`no-python-format`
:   Likewise for Python, see [Python Format Strings](#python_002dformat).

`python-brace-format` [¶](#index-no_002dpython_002dbrace_002dformat-flag)

`no-python-brace-format`
:   Likewise for Python brace, see [Python Format Strings](#python_002dformat).

`java-format` [¶](#index-no_002djava_002dformat-flag)

`no-java-format`
:   Likewise for Java `MessageFormat` format strings, see [Java Format Strings](#java_002dformat).

`java-printf-format` [¶](#index-no_002djava_002dprintf_002dformat-flag)

`no-java-printf-format`
:   Likewise for Java `printf` format strings, see [Java Format Strings](#java_002dformat).

`csharp-format` [¶](#index-no_002dcsharp_002dformat-flag)

`no-csharp-format`
:   Likewise for C#, see [C# Format Strings](#csharp_002dformat).

`javascript-format` [¶](#index-no_002djavascript_002dformat-flag)

`no-javascript-format`
:   Likewise for JavaScript, see [JavaScript Format Strings](#javascript_002dformat).

`scheme-format` [¶](#index-no_002dscheme_002dformat-flag)

`no-scheme-format`
:   Likewise for Scheme, see [Scheme Format Strings](#scheme_002dformat).

`lisp-format` [¶](#index-no_002dlisp_002dformat-flag)

`no-lisp-format`
:   Likewise for Lisp, see [Lisp Format Strings](#lisp_002dformat).

`elisp-format` [¶](#index-no_002delisp_002dformat-flag)

`no-elisp-format`
:   Likewise for Emacs Lisp, see [Emacs Lisp Format Strings](#elisp_002dformat).

`librep-format` [¶](#index-no_002dlibrep_002dformat-flag)

`no-librep-format`
:   Likewise for librep, see [librep Format Strings](#librep_002dformat).

`rust-format` [¶](#index-no_002drust_002dformat-flag)

`no-rust-format`
:   Likewise for Rust, see [Rust Format Strings](#rust_002dformat).

`go-format` [¶](#index-no_002dgo_002dformat-flag)

`no-go-format`
:   Likewise for Go, see [Go Format Strings](#go_002dformat).

`ruby-format` [¶](#index-no_002druby_002dformat-flag)

`no-ruby-format`
:   Likewise for Ruby, see [Ruby Format Strings](#ruby_002dformat).

`sh-format` [¶](#index-no_002dsh_002dformat-flag)

`no-sh-format`
:   Likewise for Shell format strings, see [Shell Format Strings](#sh_002dformat).

`sh-printf-format` [¶](#index-no_002dsh_002dprintf_002dformat-flag)

`no-sh-printf-format`
:   Likewise for Shell `printf` format strings, see [Shell Format Strings](#sh_002dformat).

`awk-format` [¶](#index-no_002dawk_002dformat-flag)

`no-awk-format`
:   Likewise for awk, see [awk Format Strings](#awk_002dformat).

`lua-format` [¶](#index-no_002dlua_002dformat-flag)

`no-lua-format`
:   Likewise for Lua, see [Lua Format Strings](#lua_002dformat).

`object-pascal-format` [¶](#index-no_002dobject_002dpascal_002dformat-flag)

`no-object-pascal-format`
:   Likewise for Object Pascal, see [Object Pascal Format Strings](#object_002dpascal_002dformat).

`modula2-format` [¶](#index-no_002dmodula2_002dformat-flag)

`no-modula2-format`
:   Likewise for Modula-2, see [Modula-2 Format Strings](#modula2_002dformat).

`d-format` [¶](#index-no_002dd_002dformat-flag)

`no-d-format`
:   Likewise for D, see [D Format Strings](#d_002dformat).

`ocaml-format` [¶](#index-no_002docaml_002dformat-flag)

`no-ocaml-format`
:   Likewise for OCaml, see [OCaml Format Strings](#ocaml_002dformat).

`smalltalk-format` [¶](#index-no_002dsmalltalk_002dformat-flag)

`no-smalltalk-format`
:   Likewise for Smalltalk, see [Smalltalk Format Strings](#smalltalk_002dformat).

`qt-format` [¶](#index-no_002dqt_002dformat-flag)

`no-qt-format`
:   Likewise for Qt, see [Qt Format Strings](#qt_002dformat).

`qt-plural-format` [¶](#index-no_002dqt_002dplural_002dformat-flag)

`no-qt-plural-format`
:   Likewise for Qt plural forms, see [Qt Format Strings](#qt_002dplural_002dformat).

`kde-format` [¶](#index-no_002dkde_002dformat-flag)

`no-kde-format`
:   Likewise for KDE, see [KDE Format Strings](#kde_002dformat).

`boost-format` [¶](#index-no_002dboost_002dformat-flag)

`no-boost-format`
:   Likewise for Boost, see [Boost Format Strings](#boost_002dformat).

`tcl-format` [¶](#index-no_002dtcl_002dformat-flag)

`no-tcl-format`
:   Likewise for Tcl, see [Tcl Format Strings](#tcl_002dformat).

`perl-format` [¶](#index-no_002dperl_002dformat-flag)

`no-perl-format`
:   Likewise for Perl, see [Perl Format Strings](#perl_002dformat).

`perl-brace-format` [¶](#index-no_002dperl_002dbrace_002dformat-flag)

`no-perl-brace-format`
:   Likewise for Perl brace, see [Perl Format Strings](#perl_002dformat).

`php-format` [¶](#index-no_002dphp_002dformat-flag)

`no-php-format`
:   Likewise for PHP, see [PHP Format Strings](#php_002dformat).

`gcc-internal-format` [¶](#index-no_002dgcc_002dinternal_002dformat-flag)

`no-gcc-internal-format`
:   Likewise for the GCC sources, see [GCC internal Format Strings](#gcc_002dinternal_002dformat).

`gfc-internal-format` [¶](#index-no_002dgfc_002dinternal_002dformat-flag)

`no-gfc-internal-format`
:   Likewise for the GNU Fortran Compiler sources up to GCC 14.x,
    see [GFC internal Format Strings](#gfc_002dinternal_002dformat).
    These flags are deprecated.

`ycp-format` [¶](#index-no_002dycp_002dformat-flag)

`no-ycp-format`
:   Likewise for YCP, see [YCP Format Strings](#ycp_002dformat).

Other flags, assigned by the programmer, are:

`no-wrap` [¶](#index-no_002dwrap-flag)
:   This flag influences the presentation of the entry in the PO file.
    By default, when a PO file is output by a GNU gettext program
    and the option ‘`--no-wrap`’ is not specified,
    message lines that exceed the output page width are split into several lines.
    This flag inhibits this line breaking for the entry.
    This is useful for entries whose lines are close to 80 columns wide.

---

Next: [Entries with Plural Forms](#Entries-with-Plural-Forms), Previous: [Sticky flags](#Sticky-flags), Up: [The Format of PO Files](#PO-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 3.4 Entries with Context [¶](#Entries-with-Context-1)

It is also possible to have entries with a context specifier. They look like
this:

```
white-space
#  translator-comments
#. extracted-comments
#: reference...
#, flag...
#| msgctxt previous-context
#| msgid previous-untranslated-string
msgctxt context
msgid untranslated-string
msgstr translated-string
```

The context serves to disambiguate messages with the same
untranslated-string. It is possible to have several entries with
the same untranslated-string in a PO file, provided that they each
have a different context. Note that an empty context string
and an absent `msgctxt` line do not mean the same thing.

---

Next: [Further details on the PO file format](#More-Details), Previous: [Entries with Context](#Entries-with-Context), Up: [The Format of PO Files](#PO-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 3.5 Entries with Plural Forms [¶](#Entries-with-Plural-Forms-1)

A different kind of entries is used for translations which involve
plural forms.

```
white-space
#  translator-comments
#. extracted-comments
#: reference...
#, flag...
#| msgid previous-untranslated-string-singular
#| msgid_plural previous-untranslated-string-plural
msgid untranslated-string-singular
msgid_plural untranslated-string-plural
msgstr[0] translated-string-case-0
...
msgstr[N] translated-string-case-n
```

Such an entry can look like this:

```
#: src/msgcmp.c:338 src/po-lex.c:699
#, c-format
msgid "found %d fatal error"
msgid_plural "found %d fatal errors"
msgstr[0] "s'ha trobat %d error fatal"
msgstr[1] "s'han trobat %d errors fatals"
```

Here also, a `msgctxt` context can be specified before `msgid`,
like above.

Here, additional kinds of sticky flags can be used:

`range:` [¶](#index-range_003a-flag)
:   This flag is followed by a range of non-negative numbers, using the syntax
    `range: minimum-value..maximum-value`. It designates the
    possible values that the numeric parameter of the message can take. In some
    languages, translators may produce slightly better translations if they know
    that the value can only take on values between 0 and 10, for example.

---

Next: [Evolution of the PO File Format](#PO-File-Format-Evolution), Previous: [Entries with Plural Forms](#Entries-with-Plural-Forms), Up: [The Format of PO Files](#PO-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 3.6 Further details on the PO file format [¶](#Further-details-on-the-PO-file-format)

It happens that some lines, usually whitespace or comments, follow the
very last entry of a PO file. Such lines are not part of any entry,
and will be dropped when the PO file is processed by the tools, or may
disturb some PO file editors.

The remainder of this section may be safely skipped by those using
a PO file editor, yet it may be interesting for everybody to have a better
idea of the precise format of a PO file. On the other hand, those
wishing to modify PO files by hand should carefully continue reading on.

An empty untranslated-string is reserved to contain the header
entry with the meta information (see [Filling in the Header Entry](#Header-Entry)). This header
entry should be the first entry of the file. The empty
untranslated-string is reserved for this purpose and must
not be used anywhere else.

Each of untranslated-string and translated-string respects
the C syntax for a character string, including the surrounding quotes
and embedded backslashed escape sequences, except that universal character
escape sequences (`\u` and `\U`) are not allowed. When the time
comes to write multi-line strings, one should not use escaped newlines.
Instead, a closing quote should follow the last character on the
line to be continued, and an opening quote should resume the string
at the beginning of the following PO file line. For example:

```
msgid ""
"Here is an example of how one might continue a very long string\n"
"for the common case the string represents multi-line output.\n"
```

In this example, the empty string is used on the first line, to
allow better alignment of the `H` from the word ‘`Here`’
over the `f` from the word ‘`for`’. In this example, the
`msgid` keyword is followed by three strings, which are meant
to be concatenated. Concatenating the empty string does not change
the resulting overall string, but it is a way for us to comply with
the necessity of `msgid` to be followed by a string on the same
line, while keeping the multi-line presentation left-justified, as
we find this to be a cleaner disposition. The empty string could have
been omitted, but only if the string starting with ‘`Here`’ was
promoted on the first line, right after `msgid`.[2](#FOOT2) It was not really necessary
either to switch between the two last quoted strings immediately after
the newline ‘`\n`’, the switch could have occurred after *any*
other character, we just did it this way because it is neater.

One should carefully distinguish between end of lines marked as
‘`\n`’ *inside* quotes, which are part of the represented
string, and end of lines in the PO file itself, outside string quotes,
which have no incidence on the represented string.

Outside strings, white lines and comments may be used freely.
Comments start at the beginning of a line with ‘`#`’ and extend
until the end of the PO file line. Comments written by translators
should have the initial ‘`#`’ immediately followed by some white
space. If the ‘`#`’ is not immediately followed by white space,
this comment is most likely generated and managed by specialized GNU
tools, and might disappear or be replaced unexpectedly when the PO
file is given to `msgmerge`.

For a PO file to be valid, no two entries without `msgctxt` may have
the same untranslated-string or untranslated-string-singular.
Similarly, no two entries may have the same `msgctxt` and the same
untranslated-string or untranslated-string-singular.

---

Previous: [Further details on the PO file format](#More-Details), Up: [The Format of PO Files](#PO-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 3.7 Evolution of the PO File Format [¶](#Evolution-of-the-PO-File-Format)

It is planned that after 2027-01-01, entries in PO files support
additional workflow flags and also custom sticky flags.

Additional workflow flags are useful to support
workflows with other steps than translation proper,
such as pretranslation (based on translation memory or done through AI)
or review or approval by a different person than the translator.

Custom sticky flags are useful for projects
which have extra capabilities in their tooling,
such as project specific checks on the translations.

In order to support both reliably, the PO file format is being extended
in two steps.

Effective **now (June 2025)**,
the character sequence ‘`#=`’ at the beginning of a line
introduces a line of flags,
like the characters ‘`#,`’ already do.

All applications that *read* (consume) PO files are encouraged to
support ‘`#=`’ as an alternative to ‘`#,`’.

All applications that *modify* (read, modify, then write) PO files should

* when not changing the flags:
  + either keep the ‘`#,`’ line unchanged and the ‘`#=`’ line unchanged,
  + or emit all flags in a ‘`#,`’ line and no ‘`#=`’ line at all.
* when adding flags:
  add these flags in the ‘`#,`’ line (**not** in the ‘`#=`’ line),
* when removing flags that were specified in the ‘`#=`’ line:
  + either remove the flag from the ‘`#=`’ line, emitting a ‘`#=`’ line with fewer flags,
  + or emit all flags in a ‘`#,`’ line and no ‘`#=`’ line at all.

**On 2027-01-01**, the PO file format will be revised again:
it will be decided that either

```
‘#,’ should be followed by workflow flags,
and ‘#=’ should be followed by sticky flags,
```

or the other way around:

```
‘#=’ should be followed by workflow flags,
and ‘#,’ should be followed by sticky flags.
```

From that moment on, applications that *write* (produce) PO files are

* allowed to emit ‘`#=`’ lines,
* allowed to emit custom sticky flags.

And applications that *read and write* PO files are

* required to keep unknown sticky flags in place, i.e. not drop them.

---

Next: [Making the PO Template File](#Template), Previous: [The Format of PO Files](#PO-Files), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 4 Preparing Program Sources [¶](#Preparing-Program-Sources)

For the programmer, changes to the C source code fall into three
categories. First, you have to make the localization functions
known to all modules needing message translation. Second, you should
properly trigger the operation of GNU `gettext` when the program
initializes, usually from the `main` function. Last, you should
identify, adjust and mark all constant strings in your program
needing translation.

* [Importing the `gettext` declaration](#Importing)
* [Triggering `gettext` Operations](#Triggering)
* [Preparing Translatable Strings](#Preparing-Strings)
* [How Marks Appear in Sources](#Mark-Keywords)
* [Marking Translatable Strings](#Marking)
* [Adding advice for translators](#Translator-advice)
* [Special Comments preceding Keywords](#c_002dformat-Flag)
* [Special Cases of Translatable Strings](#Special-cases)
* [Letting Users Report Translation Bugs](#Bug-Report-Address)
* [Marking Proper Names for Translation](#Names)
* [Preparing Library Sources](#Libraries)

---

Next: [Triggering `gettext` Operations](#Triggering), Up: [Preparing Program Sources](#Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 4.1 Importing the `gettext` declaration [¶](#Importing-the-gettext-declaration)

Presuming that your set of programs, or package, has been adjusted
so all needed GNU `gettext` files are available, and your
`Makefile` files are adjusted (see [The Maintainer’s View](#Maintainers)), each C module
having translated C strings should contain the line:

```
#include <libintl.h>
```

Similarly, each C module containing `printf()`/`fprintf()`/...
calls with a format string that could be a translated C string (even if
the C string comes from a different C module) should contain the line:

```
#include <libintl.h>
```

---

Next: [Preparing Translatable Strings](#Preparing-Strings), Previous: [Importing the `gettext` declaration](#Importing), Up: [Preparing Program Sources](#Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 4.2 Triggering `gettext` Operations [¶](#Triggering-gettext-Operations)

The initialization of locale data should be done with more or less
the same code in every program, as demonstrated below:

```
int
main (int argc, char *argv[])
{
  ...
  setlocale (LC_ALL, "");
  bindtextdomain (PACKAGE, LOCALEDIR);
  textdomain (PACKAGE);
  ...
}
```

PACKAGE and LOCALEDIR should be provided either by
`config.h` or by the Makefile. For now consult the `gettext`
or `hello` sources for more information.

The use of `LC_ALL` might not be appropriate for you.
`LC_ALL` includes all locale categories and especially
`LC_CTYPE`. This latter category is responsible for determining
character classes with the `isalnum` etc. functions from
`ctype.h` which could especially for programs, which process some
kind of input language, be wrong. For example this would mean that a
source code using the ç (c-cedilla character) is runnable in
France but not in the U.S.

Some systems also have problems with parsing numbers using the
`scanf` functions if an other but the `LC_ALL` locale category is
used. The standards say that additional formats but the one known in the
`"C"` locale might be recognized. But some systems seem to reject
numbers in the `"C"` locale format. In some situation, it might
also be a problem with the notation itself which makes it impossible to
recognize whether the number is in the `"C"` locale or the local
format. This can happen if thousands separator characters are used.
Some locales define this character according to the national
conventions to `'.'` which is the same character used in the
`"C"` locale to denote the decimal point.

So it is sometimes necessary to replace the `LC_ALL` line in the
code above by a sequence of `setlocale` lines

```
{
  ...
  setlocale (LC_CTYPE, "");
  setlocale (LC_MESSAGES, "");
  ...
}
```

On all POSIX conformant systems the locale categories `LC_CTYPE`,
`LC_MESSAGES`, `LC_COLLATE`, `LC_MONETARY`,
`LC_NUMERIC`, and `LC_TIME` are available. On some systems
which are only ISO C compliant, `LC_MESSAGES` is missing, but
a substitute for it is defined in GNU gettext’s `<libintl.h>` and
in GNU gnulib’s `<locale.h>`.

Note that changing the `LC_CTYPE` also affects the functions
declared in the `<ctype.h>` standard header and some functions
declared in the `<string.h>` and `<stdlib.h>` standard headers.
If this is not
desirable in your application (for example in a compiler’s parser),
you can use a set of substitute functions which hardwire the C locale,
such as found in the modules
‘`c-ctype`’,
‘`c-strcase`’,
‘`c-strcasestr`’,
‘`c-snprintf`’,
‘`c-strtod`’, ‘`c-strtold`’,
‘`c-dtoastr`’, ‘`c-ldtoastr`’
in the GNU gnulib source distribution.

It is also possible to switch the locale forth and back between the
environment dependent locale and the C locale, but this approach is
normally avoided because a `setlocale` call is expensive,
because it is tedious to determine the places where a locale switch
is needed in a large program’s source, and because switching a locale
is not multithread-safe.

---

Next: [How Marks Appear in Sources](#Mark-Keywords), Previous: [Triggering `gettext` Operations](#Triggering), Up: [Preparing Program Sources](#Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 4.3 Preparing Translatable Strings [¶](#Preparing-Translatable-Strings)

Before strings can be marked for translations, they sometimes need to
be adjusted. Usually preparing a string for translation is done right
before marking it, during the marking phase which is described in the
next sections. What you have to keep in mind while doing that is the
following.

* Decent English style.
* Entire sentences.
* Split at paragraphs.
* Use format strings instead of string concatenation.
* Use placeholders in format strings instead of embedded URLs.
* Use placeholders in format strings instead of programmer-defined format
  string directives.
* Avoid unusual markup and unusual control characters.

Let’s look at some examples of these guidelines.

* [Decent English style](#Decent-English-style)
* [Entire sentences](#Entire-sentences)
* [Split at paragraphs](#Split-at-paragraphs)
* [No string concatenation](#No-string-concatenation)
* [No embedded URLs](#No-embedded-URLs)
* [No programmer-defined format string directives](#No-custom-format-directives)
* [No unusual markup](#No-unusual-markup)

---

Next: [Entire sentences](#Entire-sentences), Up: [Preparing Translatable Strings](#Preparing-Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 4.3.1 Decent English style [¶](#Decent-English-style-1)

Translatable strings should be in good English style. If slang language
with abbreviations and shortcuts is used, often translators will not
understand the message and will produce very inappropriate translations.

```
"%s: is parameter\n"
```

This is nearly untranslatable: Is the displayed item *a* parameter or
*the* parameter?

```
"No match"
```

The ambiguity in this message makes it unintelligible: Is the program
attempting to set something on fire? Does it mean "The given object does
not match the template"? Does it mean "The template does not fit for any
of the objects"?

In both cases, adding more words to the message will help both the
translator and the English speaking user.

---

Next: [Split at paragraphs](#Split-at-paragraphs), Previous: [Decent English style](#Decent-English-style), Up: [Preparing Translatable Strings](#Preparing-Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 4.3.2 Entire sentences [¶](#Entire-sentences-1)

Translatable strings should be entire sentences. It is often not possible
to translate single verbs or adjectives in a substitutable way.

```
printf ("File %s is %s protected", filename, rw ? "write" : "read");
```

Most translators will not look at the source and will thus only see the
string `"File %s is %s protected"`, which is unintelligible. Change
this to

```
printf (rw ? "File %s is write protected" : "File %s is read protected",
        filename);
```

This way the translator will not only understand the message, she will
also be able to find the appropriate grammatical construction. A French
translator for example translates "write protected" like "protected
against writing".

Entire sentences are also important because in many languages, the
declination of some word in a sentence depends on the gender or the
number (singular/plural) of another part of the sentence. There are
usually more interdependencies between words than in English. The
consequence is that asking a translator to translate two half-sentences
and then combining these two half-sentences through dumb string concatenation
will not work, for many languages, even though it would work for English.
That’s why translators need to handle entire sentences.

Often sentences don’t fit into a single line. If a sentence is output
using two subsequent `printf` statements, like this

```
printf ("Locale charset \"%s\" is different from\n", lcharset);
printf ("input file charset \"%s\".\n", fcharset);
```

the translator would have to translate two half sentences, but nothing
in the POT file would tell her that the two half sentences belong together.
It is necessary to merge the two `printf` statements so that the
translator can handle the entire sentence at once and decide at which
place to insert a line break in the translation (if at all):

```
printf ("Locale charset \"%s\" is different from\n\
input file charset \"%s\".\n", lcharset, fcharset);
```

You may now ask: how about two or more adjacent sentences? Like in this case:

```
puts ("Apollo 13 scenario: Stack overflow handling failed.");
puts ("On the next stack overflow we will crash!!!");
```

Should these two statements merged into a single one? I would recommend to
merge them if the two sentences are related to each other, because then it
makes it easier for the translator to understand and translate both. On
the other hand, if one of the two messages is a stereotypic one, occurring
in other places as well, you will do a favour to the translator by not
merging the two. (Identical messages occurring in several places are
combined by `xgettext`, so the translator has to handle them once only.)

---

Next: [No string concatenation](#No-string-concatenation), Previous: [Entire sentences](#Entire-sentences), Up: [Preparing Translatable Strings](#Preparing-Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 4.3.3 Split at paragraphs [¶](#Split-at-paragraphs-1)

Translatable strings should be limited to one paragraph; don’t let a
single message be longer than ten lines. The reason is that when the
translatable string changes, the translator is faced with the task of
updating the entire translated string. Maybe only a single word will
have changed in the English string, but the translator doesn’t see that
(with the current translation tools), therefore she has to proofread
the entire message.

Many GNU programs have a ‘`--help`’ output that extends over several
screen pages. It is a courtesy towards the translators to split such a
message into several ones of five to ten lines each. While doing that,
you can also attempt to split the documented options into groups,
such as the input options, the output options, and the informative
output options. This will help every user to find the option he is
looking for.

---

Next: [No embedded URLs](#No-embedded-URLs), Previous: [Split at paragraphs](#Split-at-paragraphs), Up: [Preparing Translatable Strings](#Preparing-Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 4.3.4 No string concatenation [¶](#No-string-concatenation-1)

Hardcoded string concatenation is sometimes used to construct English
strings:

```
strcpy (s, "Replace ");
strcat (s, object1);
strcat (s, " with ");
strcat (s, object2);
strcat (s, "?");
```

In order to present to the translator only entire sentences, and also
because in some languages the translator might want to swap the order
of `object1` and `object2`, it is necessary to change this
to use a format string:

```
sprintf (s, "Replace %s with %s?", object1, object2);
```

#### String concatenation operator [¶](#String-concatenation-operator)

In many programming languages,
a particular operator denotes string concatenation
at runtime (or possibly at compile time, if the compiler supports that).

* In C++, string concatenation of `std::string` objects
  is denoted by the ‘`+`’ operator.
* In Python, string concatenation is denoted by the ‘`+`’ operator.
* In Java, string concatenation is denoted by the ‘`+`’ operator.
* In C#, string concatenation is denoted by the ‘`+`’ operator.
* In JavaScript and TypeScript,
  string concatenation is denoted by the ‘`+`’ operator.
* In Go, string concatenation is denoted by the ‘`+`’ operator.
* In Ruby, string concatenation is denoted by the ‘`+`’ operator.
* In Shell, string concatenation is denoted by mere juxtaposition of strings.
* In awk, string concatenation is denoted by mere juxtaposition of strings.
* In Lua, string concatenation is denoted by the ‘`..`’ operator.
* In Modula-2, string concatenation is denoted by the ‘`+`’ operator.
* In D, string concatenation is denoted by the ‘`~`’ operator.
* In OCaml, string concatenation is denoted by the ‘`^`’ operator.
* In Smalltalk, string concatenation is denoted by the ‘`,`’ operator.
* In Vala, string concatenation is denoted by the ‘`+`’ operator.
* In Perl, string concatenation is denoted by the ‘`.`’ operator.
* In PHP, string concatenation is denoted by the ‘`.`’ operator.

So, for example, in Java, you would change

```
System.out.println("Replace "+object1+" with "+object2+"?");
```

into a statement involving a format string:

```
System.out.println(
    MessageFormat.format("Replace {0} with {1}?",
                         new Object[] { object1, object2 }));
```

Similarly, in C#, you would change

```
Console.WriteLine("Replace "+object1+" with "+object2+"?");
```

into a statement involving a format string:

```
Console.WriteLine(
    String.Format("Replace {0} with {1}?", object1, object2));
```

#### Strings with embedded expressions [¶](#Strings-with-embedded-expressions)

In some programming languages,
it is possible to have strings with embedded expressions.
The expressions can refer to variables of the program.
The value of such an expression is converted to a string
and inserted in place of the expression;
but no formatting function is called.

* In Python, *f-strings* can contain expressions.
  Such as `f"Hello, {name}!"`.
* In C#, since C# 6.0, *interpolated strings* can contain expressions.
  Such as `$"Hello, {name}!"`.
* In JavaScript, since ES6, and in TypeScript,
  *template literals* can contain expressions.
  Such as `` `Hello, ${name}!` ``.
* In Ruby, *interpolated strings* can contain expressions.
  Such as `"Hello, #{name}!"`.
* In Shell language, double-quoted strings can contain
  references to variables, along with default values and string operations.
  Such as `"Hello, $name!"` or `"Hello, ${name}!"`.
* In D, *interpolation expression sequences* can contain expressions.
  Such as `i"Hello, $(name)!"`.
* In Tcl, strings are subject to *variable substitution*.
  Such as `"Hello, $name!"`.
* In Perl, *interpolated strings* can contain expressions.
  Such as `"Hello, $name!"`.
* In PHP, string literals are subject to *variable parsing*.
  Such as `"Hello, $name!"`.

These cases are effectively string concatenation as well,
just with a different syntax.

So, for example, in Python, you would change

```
print (f'Replace {object1.name} with {object2.name}?')
```

into a statement involving a format string:

```
print ('Replace %(name1)s with %(name2)s?'
       % { 'name1': object1.name, 'name2': object2.name })
```

or equivalently

```
print ('Replace {name1} with {name2}?'
       .format(name1 = object1.name, name2 = object2.name))
```

And in JavaScript, you would change

```
print (`Replace ${object1.name} with ${object2.name}?`)
```

into a statement involving a format string:

```
print ('Replace %s with %s?'.format(object1.name, object2.name))
```

Specifically in JavaScript,
an alternative is to use a *tagged* template literal:

```
print (tag`Replace ${object1.name} with ${object2.name}?`)
```

and pass an option ‘`--tag=tag:format`’ to `xgettext`.

#### Format strings with embedded named references [¶](#Format-strings-with-embedded-named-references)

Format strings with embedded named references are different:
They are suitable for internationalization, because it is possible
to insert a call to the `gettext` function (that will return a
translated format string) *before* the argument values are
inserted in place of the placeholders.

The format string types that allow embedded named references are:

* [Shell format strings](#sh_002dformat).
* In Python, those [Python format strings](#python_002dformat)
  that take a dictionary as argument,
  and the [Python brace format strings](#python_002dformat).
* In Ruby, those [Ruby format strings](#ruby_002dformat)
  that take a hash table as argument.
* In Perl, the [Perl brace format strings](#perl_002dformat).

#### The `<inttypes.h>` macros [¶](#The-_003cinttypes_002eh_003e-macros)

A similar case is compile time concatenation of strings. The ISO C 99
include file `<inttypes.h>` contains a macro `PRId64` that
can be used as a formatting directive for outputting an ‘`int64_t`’
integer through `printf`. It expands to a constant string, usually
"d" or "ld" or "lld" or something like this, depending on the platform.
Assume you have code like

```
printf ("The amount is %0" PRId64 "\n", number);
```

The `gettext` tools and library have special support for these
`<inttypes.h>` macros. You can therefore simply write

```
printf (gettext ("The amount is %0" PRId64 "\n"), number);
```

The PO file will contain the string "The amount is %0<PRId64>\n".
The translators will provide a translation containing "%0<PRId64>"
as well, and at runtime the `gettext` function’s result will
contain the appropriate constant string, "d" or "ld" or "lld".

This works only for the predefined `<inttypes.h>` macros. If
you have defined your own similar macros, let’s say ‘`MYPRId64`’,
that are not known to `xgettext`, the solution for this problem
is to change the code like this:

```
char buf1[100];
sprintf (buf1, "%0" MYPRId64, number);
printf (gettext ("The amount is %s\n"), buf1);
```

This means, you put the platform dependent code in one statement, and the
internationalization code in a different statement. Note that a buffer length
of 100 is safe, because all available hardware integer types are limited to
128 bits, and to print a 128 bit integer one needs at most 54 characters,
regardless whether in decimal, octal or hexadecimal.

---

Next: [No programmer-defined format string directives](#No-custom-format-directives), Previous: [No string concatenation](#No-string-concatenation), Up: [Preparing Translatable Strings](#Preparing-Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 4.3.5 No embedded URLs [¶](#No-embedded-URLs-1)

It is good to not embed URLs in translatable strings, for several reasons:

* It avoids possible mistakes during copy and paste.
* Translators cannot translate the URLs or, by mistake, use the URLs from
  other packages that are present in their compendium.
* When the URLs change, translators don’t need to revisit the translation
  of the string.

The same holds for email addresses.

So, you would change

```
fputs (_("GNU GPL version 3 <https://gnu.org/licenses/gpl.html>\n"),
       stream);
```

to

```
fprintf (stream, _("GNU GPL version 3 <%s>\n"),
         "https://gnu.org/licenses/gpl.html");
```

---

Next: [No unusual markup](#No-unusual-markup), Previous: [No embedded URLs](#No-embedded-URLs), Up: [Preparing Translatable Strings](#Preparing-Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 4.3.6 No programmer-defined format string directives [¶](#No-programmer_002ddefined-format-string-directives)

The GNU C Library’s `<printf.h>` facility and the C++ standard library’s `<format>` header file make it possible for the programmer to define their own format string directives. However, such format directives cannot be used in translatable strings, for two reasons:

* There is no reference documentation for format strings with such directives, that the translators could consult. They would therefore have to guess where the directive starts and where it ends.
* An ‘`msgfmt -c`’ invocation cannot check whether the translator has produced a compatible translation of the format string. As a consequence, when a format string contains a programmer-defined directive, the program may crash at runtime when it uses the translated format string.

To avoid this situation, you need to move the formatting with the custom directive into a format string that does not get translated.

For example, assuming code that makes use of a `%r` directive:

```
fprintf (stream, _("The contents is: %r"), data);
```

you would rewrite it to:

```
char *tmp;
if (asprintf (&tmp, "%r", data) < 0)
  error (...);
fprintf (stream, _("The contents is: %s"), tmp);
free (tmp);
```

Similarly, in C++, assuming you have defined a custom `formatter` for the type of `data`, the code

```
cout << format (_("The contents is: {:#$#}"), data);
```

should be rewritten to:

```
string tmp = format ("{:#$#}", data);
cout << format (_("The contents is: {}"), tmp);
```

---

Previous: [No programmer-defined format string directives](#No-custom-format-directives), Up: [Preparing Translatable Strings](#Preparing-Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 4.3.7 No unusual markup [¶](#No-unusual-markup-1)

Unusual markup or control characters should not be used in translatable
strings. Translators will likely not understand the particular meaning
of the markup or control characters.

For example, if you have a convention that ‘`|`’ delimits the
left-hand and right-hand part of some GUI elements, translators will
often not understand it without specific comments. It might be
better to have the translator translate the left-hand and right-hand
part separately.

Another example is the ‘`argp`’ convention to use a single ‘`\v`’
(vertical tab) control character to delimit two sections inside a
string. This is flawed. Some translators may convert it to a simple
newline, some to blank lines. With some PO file editors it may not be
easy to even enter a vertical tab control character. So, you cannot
be sure that the translation will contain a ‘`\v`’ character, at the
corresponding position. The solution is, again, to let the translator
translate two separate strings and combine at run-time the two translated
strings with the ‘`\v`’ required by the convention.

HTML markup, however, is common enough that it’s probably ok to use in
translatable strings. But please bear in mind that the GNU gettext tools
don’t verify that the translations are well-formed HTML.

---

Next: [Marking Translatable Strings](#Marking), Previous: [Preparing Translatable Strings](#Preparing-Strings), Up: [Preparing Program Sources](#Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 4.4 How Marks Appear in Sources [¶](#How-Marks-Appear-in-Sources)

All strings requiring translation should be marked in the C sources. Marking
is done in such a way that each translatable string appears to be
the sole argument of some function or preprocessor macro. There are
only a few such possible functions or macros meant for translation,
and their names are said to be marking keywords. The marking is
attached to strings themselves, rather than to what we do with them.
This approach has more uses. A blatant example is an error message
produced by formatting. The format string needs translation, as
well as some strings inserted through some ‘`%s`’ specification
in the format, while the result from `sprintf` may have so many
different instances that it is impractical to list them all in some
‘`error_string_out()`’ routine, say.

This marking operation has two goals. The first goal of marking
is for triggering the retrieval of the translation, at run time.
The keyword is possibly resolved into a routine able to dynamically
return the proper translation, as far as possible or wanted, for the
argument string. Most localizable strings are found in executable
positions, that is, attached to variables or given as parameters to
functions. But this is not universal usage, and some translatable
strings appear in structured initializations. See [Special Cases of Translatable Strings](#Special-cases).

The second goal of the marking operation is to help `xgettext`
at properly extracting all translatable strings when it scans a set
of program sources and produces PO file templates.

The canonical keyword for marking translatable strings is
‘`gettext`’, it gave its name to the whole GNU `gettext`
package. For packages making only light use of the ‘`gettext`’
keyword, macro or function, it is easily used *as is*. However,
for packages using the `gettext` interface more heavily, it
is usually more convenient to give the main keyword a shorter, less
obtrusive name. Indeed, the keyword might appear on a lot of strings
all over the package, and programmers usually do not want nor need
their program sources to remind them forcefully, all the time, that they
are internationalized. Further, a long keyword has the disadvantage
of using more horizontal space, forcing more indentation work on
sources for those trying to keep them within 79 or 80 columns.

Many packages use ‘`_`’ (a simple underline) as a keyword,
and write ‘`_("Translatable string")`’ instead of ‘`gettext
("Translatable string")`’. Further, the coding rule, from GNU standards,
wanting that there is a space between the keyword and the opening
parenthesis is relaxed, in practice, for this particular usage.
So, the textual overhead per translatable string is reduced to
only three characters: the underline and the two parentheses.
However, even if GNU `gettext` uses this convention internally,
it does not offer it officially. The real, genuine keyword is truly
‘`gettext`’ indeed. It is fairly easy for those wanting to use
‘`_`’ instead of ‘`gettext`’ to declare:

```
#include <libintl.h>
#define _(String) gettext (String)
```

instead of merely using ‘`#include <libintl.h>`’.

The marking keywords ‘`gettext`’ and ‘`_`’ take the translatable
string as sole argument. It is also possible to define marking functions
that take it at another argument position. It is even possible to make
the marked argument position depend on the total number of arguments of
the function call; this is useful in C++. All this is achieved using
`xgettext`’s ‘`--keyword`’ option. How to pass such an option
to `xgettext`, assuming that `gettextize` is used, is described
in [`Makevars` in `po/`](#po_002fMakevars) and [AM\_XGETTEXT\_OPTION in `po.m4`](#AM_005fXGETTEXT_005fOPTION).

Note also that long strings can be split across lines, into multiple
adjacent string tokens. Automatic string concatenation is performed
at compile time according to ISO C and ISO C++; `xgettext` also
supports this syntax.

In C++, marking a C++ format string requires a small code change,
because the first argument to `std::format` must be a constant
expression.
For example,

```
std::format ("{} {}!", "Hello", "world")
```

needs to be changed to

```
std::vformat (gettext ("{} {}!"), std::make_format_args("Hello", "world"))
```

Later on, the maintenance is relatively easy. If, as a programmer,
you add or modify a string, you will have to ask yourself if the
new or altered string requires translation, and include it within
‘`_()`’ if you think it should be translated. For example, ‘`"%s"`’
is an example of string *not* requiring translation. But
‘`"%s: %d"`’ *does* require translation, because in French, unlike
in English, it’s customary to put a space before a colon.

---

Next: [Adding advice for translators](#Translator-advice), Previous: [How Marks Appear in Sources](#Mark-Keywords), Up: [Preparing Program Sources](#Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 4.5 Marking Translatable Strings [¶](#Marking-Translatable-Strings)

In PO mode, one set of features is meant more for the programmer than
for the translator, and allows him to interactively mark which strings,
in a set of program sources, are translatable, and which are not.
Even if it is a fairly easy job for a programmer to find and mark
such strings by other means, using any editor of his choice, PO mode
makes this work more comfortable. Further, this gives translators
who feel a little like programmers, or programmers who feel a little
like translators, a tool letting them work at marking translatable
strings in the program sources, while simultaneously producing a set of
translation in some language, for the package being internationalized.

The set of program sources, targeted by the PO mode commands describe
here, should have an Emacs tags table constructed for your project,
prior to using these PO file commands. This is easy to do. In any
shell window, change the directory to the root of your project, then
execute a command resembling:

```
etags src/*.[hc] lib/*.[hc]
```

presuming here you want to process all `.h` and `.c` files
from the `src/` and `lib/` directories. This command will
explore all said files and create a `TAGS` file in your root
directory, somewhat summarizing the contents using a special file
format Emacs can understand.

For packages following the GNU coding standards, there is
a make goal `tags` or `TAGS` which constructs the tag files in
all directories and for all files containing source code.

Once your `TAGS` file is ready, the following commands assist
the programmer at marking translatable strings in his set of sources.
But these commands are necessarily driven from within a PO file
window, and it is likely that you do not even have such a PO file yet.
This is not a problem at all, as you may safely open a new, empty PO
file, mainly for using these commands. This empty PO file will slowly
fill in while you mark strings as translatable in your program sources.

`,` [¶](#index-_002c_002c-PO-Mode-command)
:   Search through program sources for a string which looks like a
    candidate for translation (`po-tags-search`).

`M-,` [¶](#index-M_002d_002c_002c-PO-Mode-command)
:   Mark the last string found with ‘`_()`’ (`po-mark-translatable`).

`M-.` [¶](#index-M_002d_002e_002c-PO-Mode-command)
:   Mark the last string found with a keyword taken from a set of possible
    keywords. This command with a prefix allows some management of these
    keywords (`po-select-mark-and-mark`).

The `,` (`po-tags-search`) command searches for the next
occurrence of a string which looks like a possible candidate for
translation, and displays the program source in another Emacs window,
positioned in such a way that the string is near the top of this other
window. If the string is too big to fit whole in this window, it is
positioned so only its end is shown. In any case, the cursor
is left in the PO file window. If the shown string would be better
presented differently in different native languages, you may mark it
using `M-,` or `M-.`. Otherwise, you might rather ignore it
and skip to the next string by merely repeating the `,` command.

A string is a good candidate for translation if it contains a sequence
of three or more letters. A string containing at most two letters in
a row will be considered as a candidate if it has more letters than
non-letters. The command disregards strings containing no letters,
or isolated letters only. It also disregards strings within comments,
or strings already marked with some keyword PO mode knows (see below).

If you have never told Emacs about some `TAGS` file to use, the
command will request that you specify one from the minibuffer, the
first time you use the command. You may later change your `TAGS`
file by using the regular Emacs command `M-x visit-tags-table`,
which will ask you to name the precise `TAGS` file you want
to use. See [Tag Tables](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Tags) in The Emacs Editor.

Each time you use the `,` command, the search resumes from where it was
left by the previous search, and goes through all program sources,
obeying the `TAGS` file, until all sources have been processed.
However, by giving a prefix argument to the command (`C-u ,`), you may request that the search be restarted all over again
from the first program source; but in this case, strings that you
recently marked as translatable will be automatically skipped.

Using this `,` command does not prevent using of other regular
Emacs tags commands. For example, regular `tags-search` or
`tags-query-replace` commands may be used without disrupting the
independent `,` search sequence. However, as implemented, the
*initial* `,` command (or the `,` command is used with a
prefix) might also reinitialize the regular Emacs tags searching to the
first tags file, this reinitialization might be considered spurious.

The `M-,` (`po-mark-translatable`) command will mark the
recently found string with the ‘`_`’ keyword. The `M-.`
(`po-select-mark-and-mark`) command will request that you type
one keyword from the minibuffer and use that keyword for marking
the string. Both commands will automatically create a new PO file
untranslated entry for the string being marked, and make it the
current entry (making it easy for you to immediately proceed to its
translation, if you feel like doing it right away). It is possible
that the modifications made to the program source by `M-,` or
`M-.` render some source line longer than 80 columns, forcing you
to break and re-indent this line differently. You may use the `O`
command from PO mode, or any other window changing command from
Emacs, to break out into the program source window, and do any
needed adjustments. You will have to use some regular Emacs command
to return the cursor to the PO file window, if you want command
`,` for the next string, say.

The `M-.` command has a few built-in speedups, so you do not
have to explicitly type all keywords all the time. The first such
speedup is that you are presented with a *preferred* keyword,
which you may accept by merely typing `RET` at the prompt.
The second speedup is that you may type any non-ambiguous prefix of the
keyword you really mean, and the command will complete it automatically
for you. This also means that PO mode has to *know* all
your possible keywords, and that it will not accept mistyped keywords.

If you reply `?` to the keyword request, the command gives a
list of all known keywords, from which you may choose. When the
command is prefixed by an argument (`C-u M-.`), it inhibits
updating any program source or PO file buffer, and does some simple
keyword management instead. In this case, the command asks for a
keyword, written in full, which becomes a new allowed keyword for
later `M-.` commands. Moreover, this new keyword automatically
becomes the *preferred* keyword for later commands. By typing
an already known keyword in response to `C-u M-.`, one merely
changes the *preferred* keyword and does nothing more.

All keywords known for `M-.` are recognized by the `,` command
when scanning for strings, and strings already marked by any of those
known keywords are automatically skipped. If many PO files are opened
simultaneously, each one has its own independent set of known keywords.
There is no provision in PO mode, currently, for deleting a known
keyword, you have to quit the file (maybe using `q`) and reopen
it afresh. When a PO file is newly brought up in an Emacs window, only
‘`gettext`’ and ‘`_`’ are known as keywords, and ‘`gettext`’
is preferred for the `M-.` command. In fact, this is not useful to
prefer ‘`_`’, as this one is already built in the `M-,` command.

---

Next: [Special Comments preceding Keywords](#c_002dformat-Flag), Previous: [Marking Translatable Strings](#Marking), Up: [Preparing Program Sources](#Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 4.6 Adding advice for translators [¶](#Adding-advice-for-translators)

Sometimes you might want to add advice for the translators to a
particular message.
For example:

* The translatable string might be decent English but nevertheless ambiguous.
* The translatable string refers to something in English culture (such as
  a film’s name) that is different in other cultures.
* The translator should make an adjustment that is specific to her locale.

The way to do this is to add comments,
before the `gettext` invocation or inside the `gettext` invocation
but before the string, that start with the substring ‘`TRANSLATORS:`’.
These comments will be extracted into the POT file, so that translators
can see them.
For example, when you write

```
/* TRANSLATORS: This is an English idiom,
   meaning not to reveal a secret. */
puts (gettext ("Don't spill the beans!"));
```

the POT file will contain:

```
#. TRANSLATORS: This is an English idiom,
#. meaning not to reveal a secret.
#: source.c:213
msgid "Don't spill the beans!"
msgstr ""
```

and the translators will be shown the advice
in a particular place in their translation tool.

Only comments that immediately precede the `gettext` invocation or
the translatable string are considered.
Intervening blank lines are OK,
but if there is other code between the comment and the translatable string,
the comment no longer applies.

Note: The string `TRANSLATORS:` is a convention, enabled by the
`Makefile.in.in` file that is part of a package’s build system.
It is not enabled by default in `xgettext`.
If you are using `xgettext`
without the `Makefile.in.in` infrastructure,
you will need to pass the option `--add-comments=TRANSLATORS:` yourself.

---

Next: [Special Cases of Translatable Strings](#Special-cases), Previous: [Adding advice for translators](#Translator-advice), Up: [Preparing Program Sources](#Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 4.7 Special Comments preceding Keywords [¶](#Special-Comments-preceding-Keywords)

In C programs strings are often used within calls of functions from the
`printf` family. The special thing about these format strings is
that they can contain format specifiers introduced with `%`. Assume
we have the code

```
printf (gettext ("String `%s' has %d characters\n"), s, strlen (s));
```

A possible German translation for the above string might be:

```
"%d Zeichen lang ist die Zeichenkette `%s'"
```

A C programmer, even if he cannot speak German, will recognize that
there is something wrong here. The order of the two format specifiers
is changed but of course the arguments in the `printf` don’t have.
This will most probably lead to problems because now the length of the
string is regarded as the address.

To prevent errors at runtime caused by translations, the `msgfmt`
tool can check statically whether the arguments in the original and the
translation string match in type and number. If this is not the case
and the ‘`-c`’ option has been passed to `msgfmt`, `msgfmt`
will give an error and refuse to produce a MO file. Thus consistent
use of ‘`msgfmt -c`’ will catch the error, so that it cannot cause
problems at runtime.

If the word order in the above German translation would be correct one
would have to write

```
"%2$d Zeichen lang ist die Zeichenkette `%1$s'"
```

The routines in `msgfmt` know about this special notation.

Because not all strings in a program will be format strings, it is not
useful for `msgfmt` to test all the strings in the `.po` file.
This might cause problems because the string might contain what looks
like a format specifier, but the string is not used in `printf`.

Therefore `xgettext` adds a special tag to those messages it
thinks might be a format string. There is no absolute rule for this,
only a heuristic. In the `.po` file the entry is marked using the
`c-format` flag in the `#,` comment line (see [The Format of PO Files](#PO-Files)).

The careful reader now might say that this again can cause problems.
The heuristic might guess it wrong. This is true and therefore
`xgettext` knows about a special kind of comment which lets
the programmer take over the decision. If in the same line as or
the immediately preceding line to the `gettext` keyword
the `xgettext` program finds a comment containing the words
`xgettext:c-format`, it will mark the string in any case with
the `c-format` flag. This kind of comment should be used when
`xgettext` does not recognize the string as a format string but
it really is one and it should be tested. Please note that when the
comment is in the same line as the `gettext` keyword, it must be
before the string to be translated. Also note that a comment such as
`xgettext:c-format` applies only to the first string in the same
or the next line, not to multiple strings.

This situation happens quite often. The `printf` function is often
called with strings which do not contain a format specifier. Of course
one would normally use `fputs` but it does happen. In this case
`xgettext` does not recognize this as a format string but what
happens if the translation introduces a valid format specifier? The
`printf` function will try to access one of the parameters but none
exists because the original code does not pass any parameters.

`xgettext` of course could make a wrong decision the other way
round, i.e. a string marked as a format string actually is not a format
string. In this case the `msgfmt` might give too many warnings and
would prevent translating the `.po` file. The method to prevent
this wrong decision is similar to the one used above, only the comment
to use must contain the string `xgettext:no-c-format`.

If a string is marked with `c-format` and this is not correct the
user can find out who is responsible for the decision. See
[Invoking the `xgettext` Program](#xgettext-Invocation) to see how the `--debug` option can be
used for solving this problem.

---

Next: [Letting Users Report Translation Bugs](#Bug-Report-Address), Previous: [Special Comments preceding Keywords](#c_002dformat-Flag), Up: [Preparing Program Sources](#Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 4.8 Special Cases of Translatable Strings [¶](#Special-Cases-of-Translatable-Strings)

The attentive reader might now point out that it is not always possible
to mark translatable string with `gettext` or something like this.
Consider the following case:

```
{
  static const char *messages[] = {
    "some very meaningful message",
    "and another one"
  };
  const char *string;
  ...
  string
    = index > 1 ? "a default message" : messages[index];

  fputs (string);
  ...
}
```

While it is no problem to mark the string `"a default message"` it
is not possible to mark the string initializers for `messages`.
What is to be done? We have to fulfill two tasks. First we have to mark the
strings so that the `xgettext` program (see [Invoking the `xgettext` Program](#xgettext-Invocation))
can find them, and second we have to translate the string at runtime
before printing them.

The first task can be fulfilled by creating a new keyword, which names a
no-op. For the second we have to mark all access points to a string
from the array. So one solution can look like this:

```
#define gettext_noop(String) String

{
  static const char *messages[] = {
    gettext_noop ("some very meaningful message"),
    gettext_noop ("and another one")
  };
  const char *string;
  ...
  string
    = index > 1 ? gettext ("a default message") : gettext (messages[index]);

  fputs (string);
  ...
}
```

Please convince yourself that the string which is written by
`fputs` is translated in any case. How to get `xgettext` know
the additional keyword `gettext_noop` is explained in [Invoking the `xgettext` Program](#xgettext-Invocation).

The above is of course not the only solution. You could also come along
with the following one:

```
#define gettext_noop(String) String

{
  static const char *messages[] = {
    gettext_noop ("some very meaningful message"),
    gettext_noop ("and another one")
  };
  const char *string;
  ...
  string
    = index > 1 ? gettext_noop ("a default message") : messages[index];

  fputs (gettext (string));
  ...
}
```

But this has a drawback. The programmer has to take care that
he uses `gettext_noop` for the string `"a default message"`.
A use of `gettext` could have in rare cases unpredictable results.

One advantage is that you need not make control flow analysis to make
sure the output is really translated in any case. But this analysis is
generally not very difficult. If it should be in any situation you can
use this second method in this situation.

---

Next: [Marking Proper Names for Translation](#Names), Previous: [Special Cases of Translatable Strings](#Special-cases), Up: [Preparing Program Sources](#Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 4.9 Letting Users Report Translation Bugs [¶](#Letting-Users-Report-Translation-Bugs)

Code sometimes has bugs, but translations sometimes have bugs too. The
users need to be able to report them. Reporting translation bugs to the
programmer or maintainer of a package is not very useful, since the
maintainer must never change a translation, except on behalf of the
translator. Hence the translation bugs must be reported to the
translators.

Here is a way to organize this so that the maintainer does not need to
forward translation bug reports, nor even keep a list of the addresses of
the translators or their translation teams.

Every program has a place where is shows the bug report address. For
GNU programs, it is the code which handles the “–help” option,
typically in a function called “usage”. In this place, instruct the
translator to add her own bug reporting address. For example, if that
code has a statement

```
printf (_("Report bugs to <%s>.\n"), PACKAGE_BUGREPORT);
```

you can add some translator instructions like this:

```
/* TRANSLATORS: The placeholder indicates the bug-reporting address
   for this package.  Please add _another line_ saying
   "Report translation bugs to <...>\n" with the address for translation
   bugs (typically your translation team's web or email address).  */
printf (_("Report bugs to <%s>.\n"), PACKAGE_BUGREPORT);
```

These will be extracted by ‘`xgettext`’, leading to a .pot file that
contains this:

```
#. TRANSLATORS: The placeholder indicates the bug-reporting address
#. for this package.  Please add _another line_ saying
#. "Report translation bugs to <...>\n" with the address for translation
#. bugs (typically your translation team's web or email address).
#: src/hello.c:178
#, c-format
msgid "Report bugs to <%s>.\n"
msgstr ""
```

---

Next: [Preparing Library Sources](#Libraries), Previous: [Letting Users Report Translation Bugs](#Bug-Report-Address), Up: [Preparing Program Sources](#Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 4.10 Marking Proper Names for Translation [¶](#Marking-Proper-Names-for-Translation)

Should names of persons, cities, locations etc. be marked for translation
or not? People who only know languages that can be written with Latin
letters (English, Spanish, French, German, etc.) are tempted to say “no”,
because names usually do not change when transported between these languages.
However, in general when translating from one script to another, names
are translated too, usually phonetically or by transliteration. For
example, Russian or Greek names are converted to the Latin alphabet when
being translated to English, and English or French names are converted
to the Katakana script when being translated to Japanese. This is
necessary because the speakers of the target language in general cannot
read the script the name is originally written in.

As a programmer, you should therefore make sure that names are marked
for translation, with a special comment telling the translators that it
is a proper name and how to pronounce it. In its simple form, it looks
like this:

```
printf (_("Written by %s.\n"),
        /* TRANSLATORS: This is a proper name.  See the gettext
           manual, section Names.  Note this is actually a non-ASCII
           name: The first name is (with Unicode escapes)
           "Fran\u00e7ois" or (with HTML entities) "Fran&ccedil;ois".
           Pronunciation is like "fraa-swa pee-nar".  */
        _("Francois Pinard"));
```

The GNU gnulib library offers a module ‘`propername`’
(<https://www.gnu.org/software/gnulib/MODULES.html#module=propername>)
which takes care to automatically append the original name, in parentheses,
to the translated name. For names that cannot be written in ASCII, it
also frees the translator from the task of entering the appropriate non-ASCII
characters if no script change is needed. In this more comfortable form,
it looks like this:

```
printf (_("Written by %s and %s.\n"),
        proper_name ("Ulrich Drepper"),
        /* TRANSLATORS: This is a proper name.  See the gettext
           manual, section Names.  Note this is actually a non-ASCII
           name: The first name is (with Unicode escapes)
           "Fran\u00e7ois" or (with HTML entities) "Fran&ccedil;ois".
           Pronunciation is like "fraa-swa pee-nar".  */
        proper_name_utf8 ("Francois Pinard", "Fran\303\247ois Pinard"));
```

You can also write the original name directly in Unicode (rather than with
Unicode escapes or HTML entities) and denote the pronunciation using the
International Phonetic Alphabet (see
<https://en.wikipedia.org/wiki/International_Phonetic_Alphabet>).

As a translator, you should use some care when translating names, because
it is frustrating if people see their names mutilated or distorted.

If your language uses the Latin script, all you need to do is to reproduce
the name as perfectly as you can within the usual character set of your
language. In this particular case, this means to provide a translation
containing the c-cedilla character. If your language uses a different
script and the people speaking it don’t usually read Latin words, it means
transliteration. If the programmer used the simple case, you should still
give, in parentheses, the original writing of the name – for the sake of
the people that do read the Latin script. If the programmer used the
‘`propername`’ module mentioned above, you don’t need to give the original
writing of the name in parentheses, because the program will already do so.
Here is an example, using Greek as the target script:

```
#. This is a proper name.  See the gettext
#. manual, section Names.  Note this is actually a non-ASCII
#. name: The first name is (with Unicode escapes)
#. "Fran\u00e7ois" or (with HTML entities) "Fran&ccedil;ois".
#. Pronunciation is like "fraa-swa pee-nar".
msgid "Francois Pinard"
msgstr "\phi\rho\alpha\sigma\omicron\alpha \pi\iota\nu\alpha\rho"
       " (Francois Pinard)"
```

Because translation of names is such a sensitive domain, it is a good
idea to test your translation before submitting it.

---

Previous: [Marking Proper Names for Translation](#Names), Up: [Preparing Program Sources](#Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 4.11 Preparing Library Sources [¶](#Preparing-Library-Sources)

When you are preparing a library, not a program, for the use of
`gettext`, only a few details are different. Here we assume that
the library has a translation domain and a POT file of its own. (If
it uses the translation domain and POT file of the main program, then
the previous sections apply without changes.)

1. The library code doesn’t call `setlocale (LC_ALL, "")`. It’s the
   responsibility of the main program to set the locale. The library’s
   documentation should mention this fact, so that developers of programs
   using the library are aware of it.
2. The library code doesn’t call `textdomain (PACKAGE)`, because it
   would interfere with the text domain set by the main program.
3. The initialization code for a program was

   ```
     setlocale (LC_ALL, "");
     bindtextdomain (PACKAGE, LOCALEDIR);
     textdomain (PACKAGE);
   ```

   For a library it is reduced to

   ```
     bindtextdomain (PACKAGE, LOCALEDIR);
   ```

   If your library’s API doesn’t already have an initialization function,
   you need to create one, containing at least the `bindtextdomain`
   invocation. However, you usually don’t need to export and document this
   initialization function: It is sufficient that all entry points of the
   library call the initialization function if it hasn’t been called before.
   The typical idiom used to achieve this is a static boolean variable that
   indicates whether the initialization function has been called. If the
   library is meant to be used in multithreaded applications, this variable
   needs to be marked `volatile`, so that its value get propagated
   between threads. Like this:

   ```
   static volatile bool libfoo_initialized;

   static void
   libfoo_initialize (void)
   {
     bindtextdomain (PACKAGE, LOCALEDIR);
     libfoo_initialized = true;
   }

   /* This function is part of the exported API.  */
   struct foo *
   create_foo (...)
   {
     /* Must ensure the initialization is performed.  */
     if (!libfoo_initialized)
       libfoo_initialize ();
     ...
   }

   /* This function is part of the exported API.  The argument must be
      non-NULL and have been created through create_foo().  */
   int
   foo_refcount (struct foo *argument)
   {
     /* No need to invoke the initialization function here, because
        create_foo() must already have been called before.  */
     ...
   }
   ```

   The more general solution for initialization functions, POSIX
   `pthread_once`, is not needed in this case.
4. The usual declaration of the ‘`_`’ macro in each source file was

   ```
   #include <libintl.h>
   #define _(String) gettext (String)
   ```

   for a program. For a library, which has its own translation domain,
   it reads like this:

   ```
   #include <libintl.h>
   #define _(String) dgettext (PACKAGE, String)
   ```

   In other words, `dgettext` is used instead of `gettext`.
   Similarly, the `dngettext` function should be used in place of the
   `ngettext` function.

---

Next: [Creating or preparing for translating a PO file](#Creating), Previous: [Preparing Program Sources](#Sources), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 5 Making the PO Template File [¶](#Making-the-PO-Template-File)

After preparing the sources, the programmer creates a PO template file.
This section explains how to use `xgettext` for this purpose.

`xgettext` creates a file named `domainname.po`. You
should then rename it to `domainname.pot`. (Why doesn’t
`xgettext` create it under the name `domainname.pot`
right away? The answer is: for historical reasons. When `xgettext`
was specified, the distinction between a PO file and PO file template
was fuzzy, and the suffix ‘`.pot`’ wasn’t in use at that time.)

* [Invoking the `xgettext` Program](#xgettext-Invocation)
* [Combining PO Template Files](#Combining-POTs)

---

Next: [Combining PO Template Files](#Combining-POTs), Up: [Making the PO Template File](#Template)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]
