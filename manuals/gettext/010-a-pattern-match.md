File extension
:   `ml`

String syntax
:   `"abc"`

gettext shorthand
:   `(s_ "abc")` or, for format strings, `(f_ "abc")`

gettext/ngettext functions
:   `s_`, `f_`, `sn_`, `fn_`, and also
    `gettext`, `fgettext`,
    `dgettext`, `fdgettext`,
    `dcgettext`, `fdcgettext`,
    `ngettext`, `fngettext`,
    `dngettext`, `fdngettext`,
    `dcngettext`, `fdcngettext`.

textdomain
:   `textdomain` field in first parameter of `Gettext.Program`

bindtextdomain
:   `dir` field in first parameter of `Gettext.Program`

setlocale
:   —

Prerequisite
:   Declare the libraries `gettext.base` and `gettext-stub`
    in the ‘`dune`’ file.

Use or emulate GNU gettext
:   Use (assuming that you pass `GettextStub.Native`
    as second parameter of `Gettext.Program`)

Extractor
:   `xgettext`

Formatting with positions
:   In the source code: `Printf.fprintf "%d %d"`
      
    In the msgid: `"%d %d"`
      
    In the msgstr: `"%2$d %1$d"`
      
    Note: This does not work yet; it depends on the completion of
    <https://github.com/gildor478/ocaml-gettext/issues/39>.

Portability
:   fully portable

po-mode marking
:   —

An example is available in the `examples` directory: `hello-ocaml`.

---

Next: [Vala](#Vala), Previous: [OCaml](#OCaml), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.23 GNU Smalltalk [¶](#GNU-Smalltalk)

RPMs
:   smalltalk

Ubuntu packages
:   gnu-smalltalk

File extension
:   `st`

String syntax
:   `'abc'`

gettext shorthand
:   `NLS ? 'abc'`

gettext/ngettext functions
:   `LcMessagesDomain>>#at:`, `LcMessagesDomain>>#at:plural:with:`

textdomain
:   `LcMessages>>#domain:localeDirectory:` (returns a `LcMessagesDomain`
    object).  
    Example: `I18N Locale default messages domain: 'gettext' localeDirectory: /usr/local/share/locale'`

bindtextdomain
:   `LcMessages>>#domain:localeDirectory:`, see above.

setlocale
:   Automatic if you use `I18N Locale default`.

Prerequisite
:   `PackageLoader fileInPackage: 'I18N'!`

Use or emulate GNU gettext
:   emulate

Extractor
:   `xgettext`

Formatting with positions
:   `'%1 %2' bindWith: 'Hello' with: 'world'`

Portability
:   fully portable

po-mode marking
:   —

An example is available in the `examples` directory:
`hello-smalltalk`.

---

Next: [wxWidgets library](#wxWidgets), Previous: [GNU Smalltalk](#Smalltalk), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.24 Vala [¶](#Vala-1)

RPMs
:   vala

Ubuntu packages
:   valac

File extension
:   `vala`

String syntax
:   * `"abc"`
    * `"""abc"""`

gettext shorthand
:   `_("abc")`

gettext/ngettext functions
:   `gettext`, `dgettext`, `dcgettext`, `ngettext`,
    `dngettext`, `dpgettext`, `dpgettext2`

textdomain
:   `textdomain` function, defined under the `Intl` namespace

bindtextdomain
:   `bindtextdomain` function, defined under the `Intl` namespace

setlocale
:   Programmer must call `Intl.setlocale (LocaleCategory.ALL, "")`

Prerequisite
:   —

Use or emulate GNU gettext
:   Use

Extractor
:   `xgettext`

Formatting with positions
:   Same as for the C language.

Portability
:   autoconf (gettext.m4) and #if ENABLE\_NLS

po-mode marking
:   yes

---

Next: [Tcl - Tk’s scripting language](#Tcl), Previous: [Vala](#Vala), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.25 wxWidgets library [¶](#wxWidgets-library)

RPMs
:   wxGTK, gettext

Ubuntu packages
:   libwxgtk3.0-dev or libwxgtk3.2-dev

File extension
:   `cpp`

String syntax
:   `"abc"`

gettext shorthand
:   `_("abc")`

gettext/ngettext functions
:   `wxLocale::GetString`, `wxGetTranslation`

textdomain
:   `wxLocale::AddCatalog`

bindtextdomain
:   `wxLocale::AddCatalogLookupPathPrefix`

setlocale
:   `wxLocale::Init`, `wxSetLocale`

Prerequisite
:   `#include <wx/intl.h>`

Use or emulate GNU gettext
:   emulate, see `include/wx/intl.h` and `src/common/intl.cpp`

Extractor
:   `xgettext`

Formatting with positions
:   wxString::Format supports positions if and only if the system has
    `wprintf()`, `vswprintf()` functions and they support positions
    according to POSIX.

Portability
:   fully portable

po-mode marking
:   yes

---

Next: [Perl](#Perl), Previous: [wxWidgets library](#wxWidgets), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.26 Tcl - Tk’s scripting language [¶](#Tcl-_002d-Tk_0027s-scripting-language)

RPMs
:   tcl

Ubuntu packages
:   tcl

File extension
:   `tcl`

String syntax
:   `"abc"`

gettext shorthand
:   `[_ "abc"]`

gettext/ngettext functions
:   `::msgcat::mc`

textdomain
:   —

bindtextdomain
:   —, use `::msgcat::mcload` instead

setlocale
:   automatic, uses LANG, but ignores LC\_MESSAGES and LC\_ALL

Prerequisite
:   `package require msgcat`
      
    `proc _ {s} {return [::msgcat::mc $s]}`

Use or emulate GNU gettext
:   —, uses a Tcl specific message catalog format

Extractor
:   `xgettext -k_`

Formatting with positions
:   `format "%2\$d %1\$d"`

Portability
:   fully portable

po-mode marking
:   —

Two examples are available in the `examples` directory:
`hello-tcl`, `hello-tcl-tk`.

Before marking strings as internationalizable, substitutions of variables
into the string need to be converted to `format` applications. For
example, `"file $filename not found"` becomes
`[format "file %s not found" $filename]`.
Only after this is done, can the strings be marked and extracted.
After marking, this example becomes
`[format [_ "file %s not found"] $filename]` or
`[msgcat::mc "file %s not found" $filename]`. Note that the
`msgcat::mc` function implicitly calls `format` when more than one
argument is given.

---

Next: [PHP Hypertext Preprocessor](#PHP), Previous: [Tcl - Tk’s scripting language](#Tcl), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.27 Perl [¶](#Perl-1)

RPMs
:   perl

Ubuntu packages
:   perl, libintl-perl

File extension
:   `pl`, `PL`, `pm`, `perl`, `cgi`

String syntax
:   * `"abc"`
    * `'abc'`
    * `qq (abc)`
    * `q (abc)`
    * `qr /abc/`
    * `qx (/bin/date)`
    * `/pattern match/`
    * `?pattern match?`
    * `s/substitution/operators/`
    * `$tied_hash{"message"}`
    * `$tied_hash_reference->{"message"}`
    * etc., issue the command ‘`man perlsyn`’ for details

gettext shorthand
:   `__` (double underscore)

gettext/ngettext functions
:   `gettext`, `dgettext`, `dcgettext`, `ngettext`,
    `dngettext`, `dcngettext`, `pgettext`, `dpgettext`,
    `dcpgettext`, `npgettext`, `dnpgettext`,
    `dcnpgettext`

textdomain
:   `textdomain` function

bindtextdomain
:   `bindtextdomain` function

bind\_textdomain\_codeset
:   `bind_textdomain_codeset` function

setlocale
:   Use `setlocale (LC_ALL, "");`

Prerequisite
:   `use POSIX;`
      
    `use Locale::TextDomain;` (included in the package libintl-perl
    which is available on the Comprehensive Perl Archive Network CPAN,
    https://www.cpan.org/).

Use or emulate GNU gettext
:   platform dependent: gettext\_pp emulates, gettext\_xs uses GNU gettext

Extractor
:   `xgettext -k__ -k\$__ -k%__ -k__x -k__n:1,2 -k__nx:1,2 -k__xn:1,2
    -kN__ -kN__n:1,2 -k__p:1c,2 -k__np:1c,2,3 -kN__p:1c,2 -kN__np:1c,2,3`

Formatting with positions
:   Both kinds of format strings support formatting with positions.
      
    `printf "%2\$d %1\$d", ...` (requires Perl 5.8.0 or newer)
      
    `__expand("[new] replaces [old]", old => $oldvalue, new => $newvalue)`

Portability
:   The `libintl-perl` package is platform independent but is not
    part of the Perl core. The programmer is responsible for
    providing a dummy implementation of the required functions if the
    package is not installed on the target system.

po-mode marking
:   —

Documentation
:   Included in `libintl-perl`, available on CPAN
    (https://www.cpan.org/).

An example is available in the `examples` directory: `hello-perl`.

The `xgettext` parser backend for Perl differs significantly from
the parser backends for other programming languages, just as Perl
itself differs significantly from other programming languages. The
Perl parser backend offers many more string marking facilities than
the other backends but it also has some Perl specific limitations, the
worst probably being its imperfectness.

* [General Problems Parsing Perl Code](#General-Problems)
* [Which keywords will xgettext look for?](#Default-Keywords)
* [How to Extract Hash Keys](#Special-Keywords)
* [What are Strings And Quote-like Expressions?](#Quote_002dlike-Expressions)
* [Unsupported Uses Of String Interpolation](#Interpolation-I)
* [Valid Uses Of String Interpolation](#Interpolation-II)
* [When To Use Parentheses](#Parentheses)
* [How To Grok with Long Lines](#Long-Lines)
* [Bugs, Pitfalls, And Things That Do Not Work](#Perl-Pitfalls)

---

Next: [Which keywords will xgettext look for?](#Default-Keywords), Up: [Perl](#Perl)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.27.1 General Problems Parsing Perl Code [¶](#General-Problems-Parsing-Perl-Code)

It is often heard that only Perl can parse Perl. This is not true.
Perl cannot be *parsed* at all, it can only be *executed*.
Perl has various built-in ambiguities that can only be resolved at runtime.

The following example may illustrate one common problem:

```
print gettext "Hello World!";
```

Although this example looks like a bullet-proof case of a function
invocation, it is not:

```
open gettext, ">testfile" or die;
print gettext "Hello world!"
```

In this context, the string `gettext` looks more like a
file handle. But not necessarily:

```
use Locale::Messages qw (:libintl_h);
open gettext ">testfile" or die;
print gettext "Hello world!";
```

Now, the file is probably syntactically incorrect, provided that the module
`Locale::Messages` found first in the Perl include path exports a
function `gettext`. But what if the module
`Locale::Messages` really looks like this?

```
use vars qw (*gettext);

1;
```

In this case, the string `gettext` will be interpreted as a file
handle again, and the above example will create a file `testfile`
and write the string “Hello world!” into it. Even advanced
control flow analysis will not really help:

```
if (0.5 < rand) {
   eval "use Sane";
} else {
   eval "use InSane";
}
print gettext "Hello world!";
```

If the module `Sane` exports a function `gettext` that does
what we expect, and the module `InSane` opens a file for writing
and associates the *handle* `gettext` with this output
stream, we are clueless again about what will happen at runtime. It is
completely unpredictable. The truth is that Perl has so many ways to
fill its symbol table at runtime that it is impossible to interpret a
particular piece of code without executing it.

Of course, `xgettext` will not execute your Perl sources while
scanning for translatable strings, but rather use heuristics in order
to guess what you meant.

Another problem is the ambiguity of the slash and the question mark.
Their interpretation depends on the context:

```
# A pattern match.
print "OK\n" if /foobar/;

# A division.
print 1 / 2;

# Another pattern match.
print "OK\n" if ?foobar?;

# Conditional.
print $x ? "foo" : "bar";
```

The slash may either act as the division operator or introduce a
pattern match, whereas the question mark may act as the ternary
conditional operator or as a pattern match, too. Other programming
languages like `awk` present similar problems, but the consequences of a
misinterpretation are particularly nasty with Perl sources. In `awk`
for instance, a statement can never exceed one line and the parser
can recover from a parsing error at the next newline and interpret
the rest of the input stream correctly. Perl is different, as a
pattern match is terminated by the next appearance of the delimiter
(the slash or the question mark) in the input stream, regardless of
the semantic context. If a slash is really a division sign but
mis-interpreted as a pattern match, the rest of the input file is most
probably parsed incorrectly.

There are certain cases, where the ambiguity cannot be resolved at all:

```
$x = wantarray ? 1 : 0;
```

The Perl built-in function `wantarray` does not accept any arguments.
The Perl parser therefore knows that the question mark does not start
a regular expression but is the ternary conditional operator.

```
sub wantarrays {}
$x = wantarrays ? 1 : 0;
```

Now the situation is different. The function `wantarrays` takes
a variable number of arguments (like any non-prototyped Perl function).
The question mark is now the delimiter of a pattern match, and hence
the piece of code does not compile.

```
sub wantarrays() {}
$x = wantarrays ? 1 : 0;
```

Now the function is prototyped, Perl knows that it does not accept any
arguments, and the question mark is therefore interpreted as the
ternaray operator again. But that unfortunately outsmarts `xgettext`.

The Perl parser in `xgettext` cannot know whether a function has
a prototype and what that prototype would look like. It therefore makes
an educated guess. If a function is known to be a Perl built-in and
this function does not accept any arguments, a following question mark
or slash is treated as an operator, otherwise as the delimiter of a
following regular expression. The Perl built-ins that do not accept
arguments are `wantarray`, `fork`, `time`, `times`,
`getlogin`, `getppid`, `getpwent`, `getgrent`,
`gethostent`, `getnetent`, `getprotoent`, `getservent`,
`setpwent`, `setgrent`, `endpwent`, `endgrent`,
`endhostent`, `endnetent`, `endprotoent`, and
`endservent`.

If you find that `xgettext` fails to extract strings from
portions of your sources, you should therefore look out for slashes
and/or question marks preceding these sections. You may have come
across a bug in `xgettext`’s Perl parser (and of course you
should report that bug). In the meantime you should consider to
reformulate your code in a manner less challenging to `xgettext`.

In particular, if the parser is too dumb to see that a function
does not accept arguments, use parentheses:

```
$x = somefunc() ? 1 : 0;
$y = (somefunc) ? 1 : 0;
```

In fact the Perl parser itself has similar problems and warns you
about such constructs.

---

Next: [How to Extract Hash Keys](#Special-Keywords), Previous: [General Problems Parsing Perl Code](#General-Problems), Up: [Perl](#Perl)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.27.2 Which keywords will xgettext look for? [¶](#Which-keywords-will-xgettext-look-for_003f)

Unless you instruct `xgettext` otherwise by invoking it with one
of the options `--keyword` or `-k`, it will recognize the
following keywords in your Perl sources:

* `gettext`
* `dgettext:2`

  The second argument will be extracted.
* `dcgettext:2`

  The second argument will be extracted.
* `ngettext:1,2`

  The first (singular) and the second (plural) argument will be
  extracted.
* `dngettext:2,3`

  The second (singular) and the third (plural) argument will be
  extracted.
* `dcngettext:2,3`

  The second (singular) and the third (plural) argument will be
  extracted.
* `pgettext:1c,2`

  The first (message context) and the second argument will be extracted.
* `dpgettext:2c,3`

  The second (message context) and the third argument will be extracted.
* `dcpgettext:2c,3`

  The second (message context) and the third argument will be extracted.
* `npgettext:1c,2,3`

  The first (message context), second (singular), and third (plural)
  argument will be extracted.
* `dnpgettext:2c,3,4`

  The second (message context), third (singular), and fourth (plural)
  argument will be extracted.
* `dcnpgettext:2c,3,4`

  The second (message context), third (singular), and fourth (plural)
  argument will be extracted.
* `gettext_noop`
* `%gettext`

  The keys of lookups into the hash `%gettext` will be extracted.
* `$gettext`

  The keys of lookups into the hash reference `$gettext` will be extracted.

---

Next: [What are Strings And Quote-like Expressions?](#Quote_002dlike-Expressions), Previous: [Which keywords will xgettext look for?](#Default-Keywords), Up: [Perl](#Perl)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.27.3 How to Extract Hash Keys [¶](#How-to-Extract-Hash-Keys)

Translating messages at runtime is normally performed by looking up the
original string in the translation database and returning the
translated version. The “natural” Perl implementation is a hash
lookup, and, of course, `xgettext` supports such practice.

```
print __"Hello world!";
print $__{"Hello world!"};
print $__->{"Hello world!"};
print $$__{"Hello world!"};
```

The above four lines all do the same thing. The Perl module
`Locale::TextDomain` exports by default a hash `%__` that
is tied to the function `__()`. It also exports a reference
`$__` to `%__`.

If an argument to the `xgettext` option `--keyword`,
resp. `-k` starts with a percent sign, the rest of the keyword is
interpreted as the name of a hash. If it starts with a dollar
sign, the rest of the keyword is interpreted as a reference to a
hash.

Note that you can omit the quotation marks (single or double) around
the hash key (almost) whenever Perl itself allows it:

```
print $gettext{Error};
```

The exact rule is: You can omit the surrounding quotes, when the hash
key is a valid C (!) identifier, i.e. when it starts with an
underscore or an ASCII letter and is followed by an arbitrary number
of underscores, ASCII letters or digits. Other Unicode characters
are *not* allowed, regardless of the `use utf8` pragma.

---

Next: [Unsupported Uses Of String Interpolation](#Interpolation-I), Previous: [How to Extract Hash Keys](#Special-Keywords), Up: [Perl](#Perl)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.27.4 What are Strings And Quote-like Expressions? [¶](#What-are-Strings-And-Quote_002dlike-Expressions_003f)

Perl offers a plethora of different string constructs. Those that can
be used either as arguments to functions or inside braces for hash
lookups are generally supported by `xgettext`.

* **double-quoted strings**
    

  ```
  print gettext "Hello World!";
  ```
* **single-quoted strings**
    

  ```
  print gettext 'Hello World!';
  ```
* **the operator qq**
    

  ```
  print gettext qq |Hello World!|;
  print gettext qq <E-mail: <guido\@imperia.net>>;
  ```

  The operator `qq` is fully supported. You can use arbitrary
  delimiters, including the four bracketing delimiters (round, angle,
  square, curly) that nest.
* **the operator q**
    

  ```
  print gettext q |Hello World!|;
  print gettext q <E-mail: <guido@imperia.net>>;
  ```

  The operator `q` is fully supported. You can use arbitrary
  delimiters, including the four bracketing delimiters (round, angle,
  square, curly) that nest.
* **the operator qx**
    

  ```
  print gettext qx ;LANGUAGE=C /bin/date;
  print gettext qx [/usr/bin/ls | grep '^[A-Z]*'];
  ```

  The operator `qx` is fully supported. You can use arbitrary
  delimiters, including the four bracketing delimiters (round, angle,
  square, curly) that nest.

  The example is actually a useless use of `gettext`. It will
  invoke the `gettext` function on the output of the command
  specified with the `qx` operator. The feature was included
  in order to make the interface consistent (the parser will extract
  all strings and quote-like expressions).
* **here documents**
    

  ```
  print gettext <<'EOF';
  program not found in $PATH
  EOF

  print ngettext <<EOF, <<"EOF";
  one file deleted
  EOF
  several files deleted
  EOF
  ```

  Here-documents are recognized. If the delimiter is enclosed in single
  quotes, the string is not interpolated. If it is enclosed in double
  quotes or has no quotes at all, the string is interpolated.

  Delimiters that start with a digit are not supported!

---

Next: [Valid Uses Of String Interpolation](#Interpolation-II), Previous: [What are Strings And Quote-like Expressions?](#Quote_002dlike-Expressions), Up: [Perl](#Perl)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.27.5 Unsupported Uses Of String Interpolation [¶](#Unsupported-Uses-Of-String-Interpolation)

Perl is capable of interpolating variables into strings. This offers
some nice features in localized programs but can also lead to
problems.

A common error is a construct like the following:

```
print gettext "This is the program $0!\n";
```

Perl will interpolate at runtime the value of the variable `$0`
into the argument of the `gettext()` function. Hence, this
argument is not a string constant but a variable argument (`$0`
is a global variable that holds the name of the Perl script being
executed). The interpolation is performed by Perl before the string
argument is passed to `gettext()` and will therefore depend on
the name of the script which can only be determined at runtime.
Consequently, it is almost impossible that a translation can be looked
up at runtime (except if, by accident, the interpolated string is found
in the message catalog).

The `xgettext` program will therefore produce a warning
if it encounters a variable inside of a string to be extracted,
and not extract that string.
In general, this will happen for all kinds of string interpolations that
cannot be safely performed at compile time. If you absolutely know
what you are doing, you can always circumvent this behavior:

```
my $know_what_i_am_doing = "This is program $0!\n";
print gettext $know_what_i_am_doing;
```

Since the parser only recognizes strings and quote-like expressions,
but not variables or other terms, the above construct will be
accepted. You will have to find another way, however, to let your
original string make it into your message catalog.

If invoked with the option `--extract-all`, resp. `-a`,
variable interpolation will be accepted. Rationale: You will
generally use this option in order to prepare your sources for
internationalization.

Please see the manual page ‘`man perlop`’ for details of strings and
quote-like expressions that are subject to interpolation and those
that are not. Safe interpolations (that will not lead to a warning)
are:

* the escape sequences `\t` (tab, HT, TAB), `\n`
  (newline, NL), `\r` (return, CR), `\f` (form feed, FF),
  `\b` (backspace, BS), `\a` (alarm, bell, BEL), and `\e`
  (escape, ESC).
* octal chars, like `\033`
    
  Note that octal escapes in the range of 400-777 are translated into a
  UTF-8 representation, regardless of the presence of the `use utf8` pragma.
* hex chars, like `\x1b`
* wide hex chars, like `\x{263a}`
    
  Note that this escape is translated into a UTF-8 representation,
  regardless of the presence of the `use utf8` pragma.
* control chars, like `\c[` (CTRL-[)
* named Unicode chars, like `\N{LATIN CAPITAL LETTER C WITH CEDILLA}`
    
  Note that this escape is translated into a UTF-8 representation,
  regardless of the presence of the `use utf8` pragma.

The following escapes are considered partially safe:

* `\l` lowercase next char
* `\u` uppercase next char
* `\L` lowercase till \E
* `\U` uppercase till \E
* `\E` end case modification
* `\Q` quote non-word characters till \E

These escapes are only considered safe if the string consists of
ASCII characters only. Translation of characters outside the range
defined by ASCII is locale-dependent and can actually only be performed
at runtime; `xgettext` doesn’t do these locale-dependent translations
at extraction time.

Except for the modifier `\Q`, these translations, albeit valid,
are generally useless and only obfuscate your sources. If a
translation can be safely performed at compile time you can just as
well write what you mean.

---

Next: [When To Use Parentheses](#Parentheses), Previous: [Unsupported Uses Of String Interpolation](#Interpolation-I), Up: [Perl](#Perl)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.27.6 Valid Uses Of String Interpolation [¶](#Valid-Uses-Of-String-Interpolation)

Perl is often used to generate sources for other programming languages
or arbitrary file formats. Web applications that output HTML code
make a prominent example for such usage.

You will often come across situations where you want to intersperse
code written in the target (programming) language with translatable
messages, like in the following HTML example:

```
print gettext <<EOF;
<h1>My Homepage</h1>
<script language="JavaScript"><!--
for (i = 0; i < 100; ++i) {
    alert ("Thank you so much for visiting my homepage!");
}
//--></script>
EOF
```

The parser will extract the entire here document, and it will appear
entirely in the resulting PO file, including the JavaScript snippet
embedded in the HTML code. If you exaggerate with constructs like
the above, you will run the risk that the translators of your package
will look out for a less challenging project. You should consider an
alternative expression here:

```
print <<EOF;
<h1>$gettext{"My Homepage"}</h1>
<script language="JavaScript"><!--
for (i = 0; i < 100; ++i) {
    alert ("$gettext{'Thank you so much for visiting my homepage!'}");
}
//--></script>
EOF
```

Only the translatable portions of the code will be extracted here, and
the resulting PO file will begrudgingly improve in terms of readability.

You can interpolate hash lookups in all strings or quote-like
expressions that are subject to interpolation (see the manual page
‘`man perlop`’ for details). Double interpolation is unsupported, however:

```
# TRANSLATORS: Replace "the earth" with the name of your planet.
print gettext qq{Welcome to $gettext->{"the earth"}};
```

The `qq`-quoted string is recognized as an argument to `xgettext` in
the first place, and checked for unsupported variable interpolation. The
dollar sign of hash-dereferencing will therefore terminate the parser
with an “unsupported interpolation” warning.

It is valid to interpolate hash lookups in regular expressions:

```
if ($var =~ /$gettext{"the earth"}/) {
   print gettext "Match!\n";
}
s/$gettext{"U. S. A."}/$gettext{"U. S. A."} $gettext{"(dial +0)"}/g;
```

---

Next: [How To Grok with Long Lines](#Long-Lines), Previous: [Valid Uses Of String Interpolation](#Interpolation-II), Up: [Perl](#Perl)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.27.7 When To Use Parentheses [¶](#When-To-Use-Parentheses)

In Perl, parentheses around function arguments are mostly optional.
`xgettext` will always assume that all
recognized keywords (except for hashes and hash references) are names
of properly prototyped functions, and will (hopefully) only require
parentheses where Perl itself requires them. All constructs in the
following example are therefore ok to use:

```
print gettext ("Hello World!\n");
print gettext "Hello World!\n";
print dgettext ($package => "Hello World!\n");
print dgettext $package, "Hello World!\n";

# The "fat comma" => turns the left-hand side argument into a
# single-quoted string!
print dgettext smellovision => "Hello World!\n";

# The following assignment only works with prototyped functions.
# Otherwise, the functions will act as "greedy" list operators and
# eat up all following arguments.
my $anonymous_hash = {
   planet => gettext "earth",
   cakes => ngettext "one cake", "several cakes", $n,
   still => $works,
};
# The same without fat comma:
my $other_hash = {
   'planet', gettext "earth",
   'cakes', ngettext "one cake", "several cakes", $n,
   'still', $works,
};

# Parentheses are only significant for the first argument.
print dngettext 'package', ("one cake", "several cakes", $n), $discarded;
```

---

Next: [Bugs, Pitfalls, And Things That Do Not Work](#Perl-Pitfalls), Previous: [When To Use Parentheses](#Parentheses), Up: [Perl](#Perl)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.27.8 How To Grok with Long Lines [¶](#How-To-Grok-with-Long-Lines)

The necessity of long messages can often lead to a cumbersome or
unreadable coding style. Perl has several options that may prevent
you from writing unreadable code, and
`xgettext` does its best to do likewise. This is where the dot
operator (the string concatenation operator) may come in handy:

```
print gettext ("This is a very long"
               . " message that is still"
               . " readable, because"
               . " it is split into"
               . " multiple lines.\n");
```

Perl is smart enough to concatenate these constant string fragments
into one long string at compile time, and so is
`xgettext`. You will only find one long message in the resulting
POT file.

Note that the future Perl 6 will probably use the underscore
(‘`_`’) as the string concatenation operator, and the dot
(‘`.`’) for dereferencing. This new syntax is not yet supported by
`xgettext`.

If embedded newline characters are not an issue, or even desired, you
may also insert newline characters inside quoted strings wherever you
feel like it:

```
print gettext ("<em>In HTML output
embedded newlines are generally no
problem, since adjacent whitespace
is always rendered into a single
space character.</em>");
```

You may also consider to use here documents:

```
print gettext <<EOF;
<em>In HTML output
embedded newlines are generally no
problem, since adjacent whitespace
is always rendered into a single
space character.</em>
EOF
```

Please do not forget that the line breaks are real, i.e. they
translate into newline characters that will consequently show up in
the resulting POT file.

---

Previous: [How To Grok with Long Lines](#Long-Lines), Up: [Perl](#Perl)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.27.9 Bugs, Pitfalls, And Things That Do Not Work [¶](#Bugs_002c-Pitfalls_002c-And-Things-That-Do-Not-Work)

The foregoing sections should have proven that
`xgettext` is quite smart in extracting translatable strings from
Perl sources. Yet, some more or less exotic constructs that could be
expected to work, actually do not work.

One of the more relevant limitations can be found in the
implementation of variable interpolation inside quoted strings. Only
simple hash lookups can be used there:

```
print <<EOF;
$gettext{"The dot operator"
          . " does not work"
          . "here!"}
Likewise, you cannot @{[ gettext ("interpolate function calls") ]}
inside quoted strings or quote-like expressions.
EOF
```

This is valid Perl code and will actually trigger invocations of the
`gettext` function at runtime. Yet, the Perl parser in
`xgettext` will fail to recognize the strings. A less obvious
example can be found in the interpolation of regular expressions:

```
s/<!--START_OF_WEEK-->/gettext ("Sunday")/e;
```

The modifier `e` will cause the substitution to be interpreted as
an evaluable statement. Consequently, at runtime the function
`gettext()` is called, but again, the parser fails to extract the
string “Sunday”. Use a temporary variable as a simple workaround if
you really happen to need this feature:

```
my $sunday = gettext "Sunday";
s/<!--START_OF_WEEK-->/$sunday/;
```

Hash slices would also be handy but are not recognized:

```
my @weekdays = @gettext{'Sunday', 'Monday', 'Tuesday', 'Wednesday',
                        'Thursday', 'Friday', 'Saturday'};
# Or even:
@weekdays = @gettext{qw (Sunday Monday Tuesday Wednesday Thursday
                         Friday Saturday) };
```

This is perfectly valid usage of the tied hash `%gettext` but the
strings are not recognized and therefore will not be extracted.

Another caveat of the current version is its rudimentary support for
non-ASCII characters in identifiers. You may encounter serious
problems if you use identifiers with characters outside the range of
’A’-’Z’, ’a’-’z’, ’0’-’9’ and the underscore ’\_’.

Maybe some of these missing features will be implemented in future
versions, but since you can always make do without them at minimal effort,
these todos have very low priority.

A nasty problem are brace format strings that already contain braces
as part of the normal text, for example the usage strings typically
encountered in programs:

```
die "usage: $0 {OPTIONS} FILENAME...\n";
```

If you want to internationalize this code with Perl brace format strings,
you will run into a problem:

```
die __x ("usage: {program} {OPTIONS} FILENAME...\n", program => $0);
```

Whereas ‘`{program}`’ is a placeholder, ‘`{OPTIONS}`’
is not and should probably be translated. Yet, there is no way to teach
the Perl parser in `xgettext` to recognize the first one, and leave
the other one alone.

There are two possible work-arounds for this problem. If you are
sure that your program will run under Perl 5.8.0 or newer (these
Perl versions handle positional parameters in `printf()`) or
if you are sure that the translator will not have to reorder the arguments
in her translation – for example if you have only one brace placeholder
in your string, or if it describes a syntax, like in this one –, you can
mark the string as `no-perl-brace-format` and use `printf()`:

```
# xgettext: no-perl-brace-format
die sprintf ("usage: %s {OPTIONS} FILENAME...\n", $0);
```

If you want to use the more portable Perl brace format, you will have to do
put placeholders in place of the literal braces:

```
die __x ("usage: {program} {[}OPTIONS{]} FILENAME...\n",
         program => $0, '[' => '{', ']' => '}');
```

Perl brace format strings know no escaping mechanism. No matter how this
escaping mechanism looked like, it would either give the programmer a
hard time, make translating Perl brace format strings heavy-going, or
result in a performance penalty at runtime, when the format directives
get executed. Most of the time you will happily get along with
`printf()` for this special case.

---

Next: [Pike](#Pike), Previous: [Perl](#Perl), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.28 PHP Hypertext Preprocessor [¶](#PHP-Hypertext-Preprocessor)

RPMs
:   php

Ubuntu packages
:   php

File extension
:   `php`, `php3`, `php4`

String syntax
:   `"abc"`, `'abc'`,
    `<<<EOT`, `<<<"EOT"`, `<<<'EOT'`

gettext shorthand
:   `_("abc")`

gettext/ngettext functions
:   `gettext`, `dgettext`, `dcgettext`,
    `ngettext`, `dngettext`, `dcngettext`

textdomain
:   `textdomain` function

bindtextdomain
:   `bindtextdomain` function

setlocale
:   Programmer must call `setlocale (LC_ALL, "")`

Prerequisite
:   —

Use or emulate GNU gettext
:   use

Extractor
:   `xgettext`

Formatting with positions
:   `printf "%2\$d %1\$d"`

Portability
:   On platforms without gettext, the functions are not available.

po-mode marking
:   —

An example is available in the `examples` directory: `hello-php`.

---

Next: [GNU Compiler Collection sources](#GCC_002dsource), Previous: [PHP Hypertext Preprocessor](#PHP), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.29 Pike [¶](#Pike-1)

RPMs
:   roxen

Ubuntu packages
:   pike8.0 or pike7.8

File extension
:   `pike`

String syntax
:   `"abc"`

gettext shorthand
:   —

gettext/ngettext functions
:   `gettext`, `dgettext`, `dcgettext`

textdomain
:   `textdomain` function

bindtextdomain
:   `bindtextdomain` function

setlocale
:   `setlocale` function

Prerequisite
:   `import Locale.Gettext;`

Use or emulate GNU gettext
:   use

Extractor
:   —

Formatting with positions
:   —

Portability
:   On platforms without gettext, the functions are not available.

po-mode marking
:   —

---

Next: [YCP - YaST2 scripting language](#YCP), Previous: [Pike](#Pike), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.30 GNU Compiler Collection sources [¶](#GNU-Compiler-Collection-sources)

RPMs
:   gcc

Ubuntu packages
:   gcc

File extension
:   `c`, `h`.

String syntax
:   `"abc"`

gettext shorthand
:   `_("abc")`

gettext/ngettext functions
:   `gettext`, `dgettext`, `dcgettext`, `ngettext`,
    `dngettext`, `dcngettext`

textdomain
:   `textdomain` function

bindtextdomain
:   `bindtextdomain` function

setlocale
:   Programmer must call `setlocale (LC_ALL, "")`

Prerequisite
:   `#include "intl.h"`

Use or emulate GNU gettext
:   Use

Extractor
:   `xgettext -k_`

Formatting with positions
:   —

Portability
:   Uses autoconf macros

po-mode marking
:   yes

---

Previous: [GNU Compiler Collection sources](#GCC_002dsource), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.31 YCP - YaST2 scripting language [¶](#YCP-_002d-YaST2-scripting-language)

RPMs
:   libycp, libycp-devel, yast2-core, yast2-core-devel

Ubuntu packages
:   —

File extension
:   `ycp`

String syntax
:   `"abc"`

gettext shorthand
:   `_("abc")`

gettext/ngettext functions
:   `_()` with 1 or 3 arguments

textdomain
:   `textdomain` statement

bindtextdomain
:   —

setlocale
:   —

Prerequisite
:   —

Use or emulate GNU gettext
:   use

Extractor
:   `xgettext`

Formatting with positions
:   `sformat "%2 %1"`

Portability
:   fully portable

po-mode marking
:   —

An example is available in the `examples` directory: `hello-ycp`.

---

Next: [Concluding Remarks](#Conclusion), Previous: [Other Programming Languages](#Programming-Languages), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 17 Other Data Formats [¶](#Other-Data-Formats)

While the GNU gettext tools deal mainly with POT and PO files, they can
also manipulate a couple of other data formats.

For support for many document formats, such as
plain text, MarkDown, manual pages,
XML, DocBook, and others,
you can use the `po4a` tools (<https://www.po4a.org>).

For LibreOffice document formats, such as `.odt` files,
there are no PO file based workflows.
Instead, you can use

* Lokalize (<https://apps.kde.org/lokalize/>) with the menu actions
  *Project > Create OpenDocument translation project* and
  *File > Merge translation into OpenDocument*, or
* OmegaT
  (Wikipedia: <https://en.wikipedia.org/wiki/OmegaT>,
  home page: <https://omegat.org/>,
  code: <https://github.com/omegat-org/omegat>).

With these tools, a translator can translate LibreOffice document
directly, without going through the workflow with POT and PO files.
(Internally, they use XLIFF or TMX files for storage of the translations.)
Alternatively, you can have a workflow with an XLIFF file instead of a PO file,
through the Translate Toolkit <https://toolkit.translatehouse.org/>
(see [its documentation](https://docs.translatehouse.org/projects/translate-toolkit/en/latest/formats/odf.html)).

* [Internationalizable Data Formats](#Internationalizable-Data)
* [Localized Data Formats](#Localized-Data)

---

Next: [Localized Data Formats](#Localized-Data), Up: [Other Data Formats](#Data-Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 17.1 Internationalizable Data Formats [¶](#Internationalizable-Data-Formats)

Here is a list of other data formats which can be internationalized
using GNU gettext.

* [POT - Portable Object Template](#POT)
* [Resource String Table](#RST)
* [Glade - GNOME user interface description](#Glade)
* [GSettings - GNOME user configuration schema](#GSettings)
* [AppData - freedesktop.org application description](#AppData)
* [Preparing Rules for XML Internationalization](#Preparing-ITS-Rules)

---

Next: [Resource String Table](#RST), Up: [Internationalizable Data Formats](#Internationalizable-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.1.1 POT - Portable Object Template [¶](#POT-_002d-Portable-Object-Template)

RPMs
:   gettext

Ubuntu packages
:   gettext

File extension
:   `pot`, `po`

Extractor
:   `xgettext`

---

Next: [Glade - GNOME user interface description](#Glade), Previous: [POT - Portable Object Template](#POT), Up: [Internationalizable Data Formats](#Internationalizable-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.1.2 Resource String Table [¶](#Resource-String-Table)

RST is the format of resource string table files of the Free Pascal compiler
versions older than 3.0.0. RSJ is the new format of resource string table
files, created by the Free Pascal compiler version 3.0.0 or newer.

RPMs
:   fpk

Ubuntu packages
:   fp-compiler

File extension
:   `rst`, `rsj`

Extractor
:   `xgettext`, `rstconv`

---

Next: [GSettings - GNOME user configuration schema](#GSettings), Previous: [Resource String Table](#RST), Up: [Internationalizable Data Formats](#Internationalizable-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.1.3 Glade - GNOME user interface description [¶](#Glade-_002d-GNOME-user-interface-description)

RPMs
:   glade, libglade, glade2, libglade2, intltool

Ubuntu packages
:   glade, libglade2-dev, intltool

File extension
:   `glade`, `glade2`, `ui`

Extractor
:   `xgettext`, `libglade-xgettext`, `xml-i18n-extract`, `intltool-extract`

---

Next: [AppData - freedesktop.org application description](#AppData), Previous: [Glade - GNOME user interface description](#Glade), Up: [Internationalizable Data Formats](#Internationalizable-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.1.4 GSettings - GNOME user configuration schema [¶](#GSettings-_002d-GNOME-user-configuration-schema)

RPMs
:   glib2

Ubuntu packages
:   libglib2.0-dev

File extension
:   `gschema.xml`

Extractor
:   `xgettext`, `intltool-extract`

---

Next: [Preparing Rules for XML Internationalization](#Preparing-ITS-Rules), Previous: [GSettings - GNOME user configuration schema](#GSettings), Up: [Internationalizable Data Formats](#Internationalizable-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.1.5 AppData - freedesktop.org application description [¶](#AppData-_002d-freedesktop_002eorg-application-description)

This file format is specified in
<https://www.freedesktop.org/software/appstream/docs/>.

RPMs
:   appdata-tools, appstream, libappstream-glib, libappstream-glib-builder

Ubuntu packages
:   appdata-tools, appstream, libappstream-glib-dev

File extension
:   `appdata.xml`, `metainfo.xml`

Extractor
:   `xgettext`, `intltool-extract`, `itstool`

---

Previous: [AppData - freedesktop.org application description](#AppData), Up: [Internationalizable Data Formats](#Internationalizable-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.1.6 Preparing Rules for XML Internationalization [¶](#Preparing-Rules-for-XML-Internationalization)

* [Specifying ITS Rules](#ITS-Rules)
* [Specifying where to find the ITS Rules](#Locating-Rules)
* [Two Use-cases of Translated Strings in XML](#Two-Use_002dcases-of-Translated-Strings-in-XML)

---

Next: [Specifying where to find the ITS Rules](#Locating-Rules), Up: [Preparing Rules for XML Internationalization](#Preparing-ITS-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.1.6.1 Specifying ITS Rules [¶](#Specifying-ITS-Rules)

Marking translatable strings in an XML file is done through a separate
"rule" file, making use of the Internationalization Tag Set standard
(ITS, <https://www.w3.org/TR/its20/>). The currently supported ITS
data categories are: ‘`Translate`’, ‘`Localization Note`’,
‘`Elements Within Text`’, and ‘`Preserve Space`’. In addition to
them, `xgettext` also recognizes the following extended data
categories:

‘`Context`’
:   This data category associates `msgctxt` to the extracted text. In
    the global rule, the `contextRule` element contains the following:

    * A required `selector` attribute. It contains an absolute selector
      that selects the nodes to which this rule applies.
    * A required `contextPointer` attribute that contains a relative
      selector pointing to a node that holds the `msgctxt` value.
    * An optional `textPointer` attribute that contains a relative
      selector pointing to a node that holds the `msgid` value.

‘`Extended Preserve Space`’
:   This data category extends the standard ‘`Preserve Space`’ data
    category with the additional values ‘`trim`’ and ‘`paragraph`’.
    ‘`trim`’ means to remove the leading and trailing whitespaces of the
    content, but not to normalize whitespaces in the middle.
    ‘`paragraph`’ means to normalize the content but keep the paragraph
    boundaries. In the global
    rule, the `preserveSpaceRule` element contains the following:

    * A required `selector` attribute. It contains an absolute selector
      that selects the nodes to which this rule applies.
    * A required `space` attribute with the value `default`,
      `preserve`, `trim`, or `paragraph`.

‘`Escape Special Characters`’
:   This data category indicates whether the special XML characters
    (`<`, `>`, `&`, `"`) are escaped with entity
    references. In the global rule, the `escapeRule` element contains
    the following:

    * A required `selector` attribute. It contains an absolute selector
      that selects the nodes to which this rule applies.
    * A required `escape` attribute with the value `yes` or `no`.
    * An optional `unescape-if` attribute with the value
      `xml`, `xhtml`, `html`, or `no`.

    The default values, `escape="no"` and `unescape-if="no"`,
    should be good for most XML file types.
    A rule with `escape="no"`,
    that was necessary with GNU gettext versions before 0.23,
    is now redundant.

    The `unescape-if` attribute is useful for XML file types
    which present messages with embedded XML elements to the translator.
    Such file types are for example DocBook or XHTML.
    If `unescape-if="xml"` is specified and the translation
    of a message looks like valid XML, the usual escaping of `<`,
    `>`, and character references is omitted.
    The resulting XML document then is likely what the translator intended.
    However, if the translator did not merely copy the XML markup from the
    message to the translation, but added or removed markup,
    the resulting XML document may be invalid.
    It is therefore useful if, after invoking `msgfmt`, you check
    the resulting XML document against the appropriate XML schema or DTD.

    Similarly, if `unescape-if="xhtml"` is specified and the translation
    looks like valid XHTML, the usual escaping is omitted.
    And likewise for `unescape-if="html"`.

All those extended data categories can only be expressed with global
rules, and the rule elements have to have the
`https://www.gnu.org/s/gettext/ns/its/extensions/1.0` namespace.

Given the following XML document in a file `messages.xml`:

```
<?xml version="1.0"?>
<messages>
  <message>
    <p>A translatable string</p>
  </message>
  <message>
    <p translatable="no">A non-translatable string</p>
  </message>
</messages>
```

To extract the first text content ("A translatable string"), but not the
second ("A non-translatable string"), the following ITS rules can be used:

```
<?xml version="1.0"?>
<its:rules xmlns:its="http://www.w3.org/2005/11/its" version="1.0">
  <its:translateRule selector="/messages" translate="no"/>
  <its:translateRule selector="//message/p" translate="yes"/>

  <!-- If 'p' has an attribute 'translatable' with the value 'no', then
       the content is not translatable.  -->
  <its:translateRule selector="//message/p[@translatable = 'no']"
    translate="no"/>
</its:rules>
```

ITS rules files must have the `.its` file extension and obey
the XML schema version 1.0 encoded by `its.xsd10` or
the XML schema version 1.1 encoded by `its.xsd11`
and its auxiliary schema `its-extensions.xsd`.

---

Previous: [Specifying ITS Rules](#ITS-Rules), Up: [Preparing Rules for XML Internationalization](#Preparing-ITS-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.1.6.2 Specifying where to find the ITS Rules [¶](#Specifying-where-to-find-the-ITS-Rules)

‘`xgettext`’ needs another file called "locating rules" to associate
an ITS rule with an XML file. If the above ITS file is saved as
`messages.its`, the locating rules file would look like:

```
<?xml version="1.0"?>
<locatingRules>
  <locatingRule name="Messages" pattern="*.xml">
    <documentRule localName="messages" target="messages.its"/>
  </locatingRule>
  <locatingRule name="Messages" pattern="*.msg" target="messages.its"/>
</locatingRules>
```

The `locatingRule` element must have a `pattern` attribute,
which denotes either a literal file name or a wildcard pattern of the
XML file[6](#FOOT6). The `locatingRule` element can have child
`documentRule` element, which adds checks on the content of the XML
file.

The first rule matches any file with the `.xml` file extension, but
it only applies to XML files whose root element is ‘`<messages>`’.

The second rule indicates that the same ITS rules file are also
applicable to any file with the `.msg` file extension. The
optional `name` attribute of `locatingRule` allows to choose
rules by name, typically with `xgettext`’s `-L` option.

The associated ITS rules file is indicated by the `target` attribute
of `locatingRule` or `documentRule`. If it is specified in a
`documentRule` element, the parent `locatingRule` shouldn’t
have the `target` attribute.

Locating rules files must have the `.loc` file extension and obey
the XML schema version 1.0 encoded by `locating-rules.xsd10` or
the XML schema version 1.1 encoded by `locating-rules.xsd11`.
Both ITS rules files and locating rules files must be installed in the
`$prefix/share/gettext/its` directory. Once those files are
properly installed, `xgettext` can extract translatable strings
from the matching XML files.

#### 17.1.6.3 Two Use-cases of Translated Strings in XML [¶](#Two-Use_002dcases-of-Translated-Strings-in-XML)

After strings have been extracted from an XML file to a POT file
through `xgettext`
and the translator has produced a PO file with translations,
it can be used in two ways:

* The PO file (or the MO file generated from it) can be directly consumed
  by a program.
* Or the translated strings can be merged back to the original XML document.
  To do this use the `msgfmt` program with the option `--xml`.
  See [Invoking the `msgfmt` Program](#msgfmt-Invocation), for more details about how one calls
  the ‘`msgfmt`’ program.

  During this merge from a PO file into an XML file, it may happen that
  more escaping of special characters for XML is needed
  than what `msgfmt` does by default.
  In this case, you can enforce more escaping
  either throuch an `<escapeRule>` ITS rule,
  or through an attribute `gt:escape="yes"` on the particular XML element.

---

Previous: [Internationalizable Data Formats](#Internationalizable-Data), Up: [Other Data Formats](#Data-Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 17.2 Localized Data Formats [¶](#Localized-Data-Formats)

Here is a list of file formats that contain localized data and that the
GNU gettext tools can manipulate.

* [Editable Message Catalogs](#Editable-Message-Catalogs)
* [Compiled Message Catalogs](#Compiled-Message-Catalogs)
* [Desktop Entry files](#Desktop-Entry)
* [XML files](#XML)

---

Next: [Compiled Message Catalogs](#Compiled-Message-Catalogs), Up: [Localized Data Formats](#Localized-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.2.1 Editable Message Catalogs [¶](#Editable-Message-Catalogs-1)

These file formats can be used with all of the `msg*` tools and with
the `xgettext` program.

If you just want to convert among these formats, you can use the
`msgcat` program (with the appropriate option) or the `xgettext`
program.

* [PO - Portable Object](#PO)
* [Java .properties](#Java-_002eproperties)
* [NeXTstep/GNUstep .strings](#GNUstep-_002estrings)

---

Next: [Java .properties](#Java-_002eproperties), Up: [Editable Message Catalogs](#Editable-Message-Catalogs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.2.1.1 PO - Portable Object [¶](#PO-_002d-Portable-Object)

File extension
:   `po`

---

Next: [NeXTstep/GNUstep .strings](#GNUstep-_002estrings), Previous: [PO - Portable Object](#PO), Up: [Editable Message Catalogs](#Editable-Message-Catalogs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.2.1.2 Java .properties [¶](#Java-_002eproperties-1)

File extension
:   `properties`

---

Previous: [Java .properties](#Java-_002eproperties), Up: [Editable Message Catalogs](#Editable-Message-Catalogs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.2.1.3 NeXTstep/GNUstep .strings [¶](#NeXTstep_002fGNUstep-_002estrings)

File extension
:   `strings`

---

Next: [Desktop Entry files](#Desktop-Entry), Previous: [Editable Message Catalogs](#Editable-Message-Catalogs), Up: [Localized Data Formats](#Localized-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.2.2 Compiled Message Catalogs [¶](#Compiled-Message-Catalogs-1)

These file formats can be created through `msgfmt` and converted back
to PO format through `msgunfmt`.

* [MO - Machine Object](#MO)
* [Java ResourceBundle](#Java-ResourceBundle)
* [C# Satellite Assembly](#C_0023-Satellite-Assembly)
* [C# Resource](#C_0023-Resource)
* [Tcl message catalog](#Tcl-message-catalog)
* [Qt message catalog](#Qt-message-catalog)

---

Next: [Java ResourceBundle](#Java-ResourceBundle), Up: [Compiled Message Catalogs](#Compiled-Message-Catalogs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.2.2.1 MO - Machine Object [¶](#MO-_002d-Machine-Object)

File extension
:   `mo`

See section [The Format of GNU MO Files](#MO-Files) for details.

---

Next: [C# Satellite Assembly](#C_0023-Satellite-Assembly), Previous: [MO - Machine Object](#MO), Up: [Compiled Message Catalogs](#Compiled-Message-Catalogs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.2.2.2 Java ResourceBundle [¶](#Java-ResourceBundle-1)

File extension
:   `class`

For more information, see the section [Java](#Java) and the examples
`hello-java`, `hello-java-awt`, `hello-java-swing`.

---

Next: [C# Resource](#C_0023-Resource), Previous: [Java ResourceBundle](#Java-ResourceBundle), Up: [Compiled Message Catalogs](#Compiled-Message-Catalogs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.2.2.3 C# Satellite Assembly [¶](#C_0023-Satellite-Assembly-1)

File extension
:   `dll`

For more information, see the section [C#](#C_0023).

---

Next: [Tcl message catalog](#Tcl-message-catalog), Previous: [C# Satellite Assembly](#C_0023-Satellite-Assembly), Up: [Compiled Message Catalogs](#Compiled-Message-Catalogs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.2.2.4 C# Resource [¶](#C_0023-Resource-1)

File extension
:   `resources`

For more information, see the section [C#](#C_0023).

---

Next: [Qt message catalog](#Qt-message-catalog), Previous: [C# Resource](#C_0023-Resource), Up: [Compiled Message Catalogs](#Compiled-Message-Catalogs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.2.2.5 Tcl message catalog [¶](#Tcl-message-catalog-1)

File extension
:   `msg`

For more information, see the section [Tcl - Tk’s scripting language](#Tcl) and the examples
`hello-tcl`, `hello-tcl-tk`.

---

Previous: [Tcl message catalog](#Tcl-message-catalog), Up: [Compiled Message Catalogs](#Compiled-Message-Catalogs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.2.2.6 Qt message catalog [¶](#Qt-message-catalog-1)

File extension
:   `qm`

For more information, see the examples `hello-c++-qt` and
`hello-c++-kde`.

---

Next: [XML files](#XML), Previous: [Compiled Message Catalogs](#Compiled-Message-Catalogs), Up: [Localized Data Formats](#Localized-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.2.3 Desktop Entry files [¶](#Desktop-Entry-files)

The programmer produces a desktop entry file template with only the
English strings. These strings get included in the POT file, by way of
`xgettext` (usually by listing the template in `po/POTFILES.in`).
The translators produce PO files, one for each language. Finally, an
`msgfmt --desktop` invocation collects all the translations in the
desktop entry file.

For more information, see the example `hello-c-gnome3`.

* [How to handle icons in Desktop Entry files](#Icons)

---

Up: [Desktop Entry files](#Desktop-Entry)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.2.3.1 How to handle icons in Desktop Entry files [¶](#How-to-handle-icons-in-Desktop-Entry-files)

Icons are generally locale dependent, for the following reasons:

* Icons may contain signs that are considered rude in some cultures. For
  example, the high-five sign, in some cultures, is perceived as an
  unfriendly “stop” sign.
* Icons may contain metaphors that are culture specific. For example, a
  mailbox in the U.S. looks different than mailboxes all around the world.
* Icons may need to be mirrored for right-to-left locales.
* Icons may contain text strings (a bad practice, but anyway).

However, icons are not covered by GNU gettext localization, because

* Icons cannot be easily embedded in PO files,
* The need to localize an icon is rare, and the ability to do so in a PO
  file would introduce translator mistakes.

Desktop Entry files may contain an ‘`Icon`’ property, and this
property is localizable. If a translator wishes to localize an icon,
she should do so by bypassing the normal workflow with PO files:

1. The translator contacts the package developers directly, sending them
   the icon appropriate for her locale, with a request to change the
   template file.
2. The package developers add the icon file to their repository, and a
   line

   ```
   Icon[locale]=icon_file_name
   ```

   to the template file.

This line remains in place when this template file is merged with the
translators’ PO files, through `msgfmt`.

---

Previous: [Desktop Entry files](#Desktop-Entry), Up: [Localized Data Formats](#Localized-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 17.2.4 XML files [¶](#XML-files)

See the section [Preparing Rules for XML Internationalization](#Preparing-ITS-Rules) and
[Invoking the `msgfmt` Program](#msgfmt-Invocation), subsection “XML mode operations”.

---

Next: [Language Codes](#Language-Codes), Previous: [Other Data Formats](#Data-Formats), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 18 Concluding Remarks [¶](#Concluding-Remarks)

We would like to conclude this GNU `gettext` manual by presenting
an history of the Translation Project so far. We finally give
a few pointers for those who want to do further research or readings
about Native Language Support matters.

* [History of GNU `gettext`](#History)
* [Notes on the Free Translation Project](#The-original-ABOUT_002dNLS)
* [Related Readings](#References)

---

Next: [Notes on the Free Translation Project](#The-original-ABOUT_002dNLS), Up: [Concluding Remarks](#Conclusion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 18.1 History of GNU `gettext` [¶](#History-of-GNU-gettext)

Internationalization concerns and algorithms have been informally
and casually discussed for years in GNU, sometimes around GNU
`libc`, maybe around the incoming `Hurd`, or otherwise
(nobody clearly remembers). And even then, when the work started for
real, this was somewhat independently of these previous discussions.

This all began in July 1994, when Patrick D’Cruze had the idea and
initiative of internationalizing version 3.9.2 of GNU `fileutils`.
He then asked Jim Meyering, the maintainer, how to get those changes
folded into an official release. That first draft was full of
`#ifdef`s and somewhat disconcerting, and Jim wanted to find
nicer ways. Patrick and Jim shared some tries and experimentations
in this area. Then, feeling that this might eventually have a deeper
impact on GNU, Jim wanted to know what standards were, and contacted
Richard Stallman, who very quickly and verbally described an overall
design for what was meant to become `glocale`, at that time.

Jim implemented `glocale` and got a lot of exhausting feedback
from Patrick and Richard, of course, but also from Mitchum DSouza
(who wrote a `catgets`-like package), Roland McGrath, maybe David
MacKenzie, François Pinard, and Paul Eggert, all pushing and
pulling in various directions, not always compatible, to the extent
that after a couple of test releases, `glocale` was torn apart.
In particular, Paul Eggert – always keeping an eye on developments
in Solaris – advocated the use of the `gettext` API over
`glocale`’s `catgets`-based API.

While Jim took some distance and time and became dad for a second
time, Roland wanted to get GNU `libc` internationalized, and
got Ulrich Drepper involved in that project. Instead of starting
from `glocale`, Ulrich rewrote something from scratch, but
more conforming to the set of guidelines who emerged out of the
`glocale` effort. Then, Ulrich got people from the previous
forum to involve themselves into this new project, and the switch
from `glocale` to what was first named `msgutils`, renamed
`nlsutils`, and later `gettext`, became officially accepted
by Richard in May 1995 or so.

Let’s summarize by saying that Ulrich Drepper wrote GNU `gettext`
in April 1995. The first official release of the package, including
PO mode, occurred in July 1995, and was numbered 0.7. Other people
contributed to the effort by providing a discussion forum around
Ulrich, writing little pieces of code, or testing. These are quoted
in the `THANKS` file which comes with the GNU `gettext`
distribution.

While this was being done, François adapted half a dozen of
GNU packages to `glocale` first, then later to `gettext`,
putting them in pretest, so providing along the way an effective
user environment for fine tuning the evolving tools. He also took
the responsibility of organizing and coordinating the Translation
Project. After nearly a year of informal exchanges between people from
many countries, translator teams started to exist in May 1995, through
the creation and support by Patrick D’Cruze of twenty unmoderated
mailing lists for that many native languages, and two moderated
lists: one for reaching all teams at once, the other for reaching
all willing maintainers of internationalized free software packages.

François also wrote PO mode in June 1995 with the collaboration
of Greg McGary, as a kind of contribution to Ulrich’s package.
He also gave a hand with the GNU `gettext` Texinfo manual.

In 1997, Ulrich Drepper released the GNU libc 2.0, which included the
`gettext`, `textdomain` and `bindtextdomain` functions.

In 2000, Ulrich Drepper added plural form handling (the `ngettext`
function) to GNU libc. Later, in 2001, he released GNU libc 2.2.x,
which is the first free C library with full internationalization support.

Ulrich being quite busy in his role of General Maintainer of GNU libc,
he handed over the GNU `gettext` maintenance to Bruno Haible in
2000. Bruno added the plural form handling to the tools as well, added
support for UTF-8 and CJK locales, and wrote a few new tools for
manipulating PO files.

---

Next: [Related Readings](#References), Previous: [History of GNU `gettext`](#History), Up: [Concluding Remarks](#Conclusion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]
