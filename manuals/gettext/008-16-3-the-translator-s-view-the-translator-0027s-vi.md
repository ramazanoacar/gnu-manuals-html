### 16.3 The Translator’s View [¶](#The-Translator_0027s-View-1)

The translator works exactly as in the C language case. The only
difference is that when translating format strings, she has to be aware
of the language’s particular syntax for positional arguments in format
strings.

* [C Format Strings](#c_002dformat)
* [Objective C Format Strings](#objc_002dformat)
* [C++ Format Strings](#c_002b_002b_002dformat)
* [Python Format Strings](#python_002dformat)
* [Java Format Strings](#java_002dformat)
* [C# Format Strings](#csharp_002dformat)
* [JavaScript Format Strings](#javascript_002dformat)
* [Scheme Format Strings](#scheme_002dformat)
* [Lisp Format Strings](#lisp_002dformat)
* [Emacs Lisp Format Strings](#elisp_002dformat)
* [librep Format Strings](#librep_002dformat)
* [Rust Format Strings](#rust_002dformat)
* [Go Format Strings](#go_002dformat)
* [Ruby Format Strings](#ruby_002dformat)
* [Shell Format Strings](#sh_002dformat)
* [awk Format Strings](#awk_002dformat)
* [Lua Format Strings](#lua_002dformat)
* [Object Pascal Format Strings](#object_002dpascal_002dformat)
* [Modula-2 Format Strings](#modula2_002dformat)
* [D Format Strings](#d_002dformat)
* [OCaml Format Strings](#ocaml_002dformat)
* [Smalltalk Format Strings](#smalltalk_002dformat)
* [Qt Format Strings](#qt_002dformat)
* [Qt Format Strings](#qt_002dplural_002dformat)
* [KDE Format Strings](#kde_002dformat)
* [KUIT Format Strings](#kde_002dkuit_002dformat)
* [Boost Format Strings](#boost_002dformat)
* [Tcl Format Strings](#tcl_002dformat)
* [Perl Format Strings](#perl_002dformat)
* [PHP Format Strings](#php_002dformat)
* [GCC internal Format Strings](#gcc_002dinternal_002dformat)
* [GFC internal Format Strings](#gfc_002dinternal_002dformat)
* [YCP Format Strings](#ycp_002dformat)

---

Next: [Objective C Format Strings](#objc_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.1 C Format Strings [¶](#C-Format-Strings)

C format strings are described in POSIX (IEEE P1003.1 2001), section
XSH 3 fprintf(),
<https://pubs.opengroup.org/onlinepubs/9799919799/functions/fprintf.html>.
See also the fprintf() manual page
[man fprintf](https://www.kernel.org/doc/man-pages/online/pages/man3/fprintf.3.html).

Although format strings with positions that reorder arguments, such as

```
"Only %2$d bytes free on '%1$s'."
```

which is semantically equivalent to

```
"'%s' has only %d bytes free."
```

are a POSIX/XSI feature and not specified by ISO C 99, translators can rely
on this reordering ability: On the few platforms where `printf()`,
`fprintf()` etc. don’t support this feature natively, `libintl.a`
or `libintl.so` provides replacement functions, and GNU `<libintl.h>`
activates these replacement functions automatically.

C format strings can contain placeholders
that reference macros defined in ISO C 99 `<inttypes.h>`.
For example, `<PRId64>` references the macro `PRId64`.
The value of such a macro is system-dependent,
but programmers and translators do not need to know this value.
ISO C 23 specifies system-independent format string elements,
for example, `"%w64d"` instead of `"%" PRId64`;
however, as of 2024, these are not implemented across systems
and therefore cannot be used portably.

As a special feature for Farsi (Persian) and maybe Arabic, translators can
insert an ‘`I`’ flag into numeric format directives. For example, the
translation of `"%d"` can be `"%Id"`. The effect of this flag,
on systems with GNU `libc`, is that in the output, the ASCII digits are
replaced with the ‘`outdigits`’ defined in the `LC_CTYPE` locale
category. On other systems, the `gettext` function removes this flag,
so that it has no effect.

Note that the programmer should *not* put this flag into the
untranslated string. (Putting the ‘`I`’ format directive flag into an
msgid string would lead to undefined behaviour on platforms without
glibc when NLS is disabled.)

---

Next: [C++ Format Strings](#c_002b_002b_002dformat), Previous: [C Format Strings](#c_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.2 Objective C Format Strings [¶](#Objective-C-Format-Strings)

Objective C format strings are like C format strings. They support an
additional format directive: "%@", which when executed consumes an argument
of type `Object *`.

Objective C format strings, like C format strings, can contain placeholders
that reference macros defined in ISO C 99 `<inttypes.h>`.

---

Next: [Python Format Strings](#python_002dformat), Previous: [Objective C Format Strings](#objc_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.3 C++ Format Strings [¶](#C_002b_002b-Format-Strings)

C++ format strings are described in ISO C++ 20, namely in
<https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2020/n4861.pdf>,
section 20.20.2 Format string [format.string].

An easier-to-read description is found at
<https://en.cppreference.com/w/cpp/utility/format/format#Parameters> and
<https://en.cppreference.com/w/cpp/utility/format/formatter#Standard_format_specification>.

---

Next: [Java Format Strings](#java_002dformat), Previous: [C++ Format Strings](#c_002b_002b_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.4 Python Format Strings [¶](#Python-Format-Strings)

There are two kinds of format strings in Python: those acceptable to
the Python built-in format operator `%`, labelled as
‘`python-format`’, and those acceptable to the `format` method
of the ‘`str`’ object.

Python `%` format strings are described in
Python Library reference /
5. Built-in Types /
5.6. Sequence Types /
5.6.2. String Formatting Operations.
<https://docs.python.org/2/library/stdtypes.html#string-formatting-operations>.

Python brace format strings are described in PEP 3101 – Advanced String Formatting, <https://www.python.org/dev/peps/pep-3101/>.

---

Next: [C# Format Strings](#csharp_002dformat), Previous: [Python Format Strings](#python_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.5 Java Format Strings [¶](#Java-Format-Strings)

There are two kinds of format strings in Java: those acceptable to the
`MessageFormat.format` function, labelled as ‘`java-format`’,
and those acceptable to the `String.format` and
`PrintStream.printf` functions, labelled as ‘`java-printf-format`’.

Java format strings are described in the JDK documentation for class
`java.text.MessageFormat`,
<https://docs.oracle.com/javase/7/docs/api/java/text/MessageFormat.html>.
See also the ICU documentation
<http://icu-project.org/apiref/icu4j/com/ibm/icu/text/MessageFormat.html>.

Java `printf` format strings are described in the JDK documentation
for class `java.util.Formatter`,
<https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html>.

---

Next: [JavaScript Format Strings](#javascript_002dformat), Previous: [Java Format Strings](#java_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.6 C# Format Strings [¶](#C_0023-Format-Strings)

C# format strings are described in the .NET documentation for class
`System.String` and in
<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/cpguide/html/cpConFormattingOverview.asp>.

---

Next: [Scheme Format Strings](#scheme_002dformat), Previous: [C# Format Strings](#csharp_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.7 JavaScript Format Strings [¶](#JavaScript-Format-Strings)

Although JavaScript specification itself does not define any format
strings, many JavaScript implementations provide printf-like
functions. `xgettext` understands a set of common format strings
used in popular JavaScript implementations including Gjs, Seed, and
Node.JS. In such a format string, a directive starts with ‘`%`’
and is finished by a specifier: ‘`%`’ denotes a literal percent
sign, ‘`c`’ denotes a character, ‘`s`’ denotes a string,
‘`b`’, ‘`d`’, ‘`o`’, ‘`x`’, ‘`X`’ denote an integer,
‘`f`’ denotes floating-point number, ‘`j`’ denotes a JSON
object.

---

Next: [Lisp Format Strings](#lisp_002dformat), Previous: [JavaScript Format Strings](#javascript_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.8 Scheme Format Strings [¶](#Scheme-Format-Strings)

Scheme format strings are documented in the SLIB manual, section
Format Specification.

---

Next: [Emacs Lisp Format Strings](#elisp_002dformat), Previous: [Scheme Format Strings](#scheme_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.9 Lisp Format Strings [¶](#Lisp-Format-Strings)

Lisp format strings are described in the Common Lisp HyperSpec,
chapter 22.3 Formatted Output,
<http://www.ai.mit.edu/projects/iiip/doc/CommonLISP/HyperSpec/Body/sec_22-3.html>.

---

Next: [librep Format Strings](#librep_002dformat), Previous: [Lisp Format Strings](#lisp_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.10 Emacs Lisp Format Strings [¶](#Emacs-Lisp-Format-Strings)

Emacs Lisp format strings are documented in the Emacs Lisp reference,
section Formatting Strings,
<https://www.gnu.org/manual/elisp-manual-21-2.8/html_chapter/elisp_4.html#SEC75>.
Note that as of version 21, XEmacs supports numbered argument specifications
in format strings while FSF Emacs doesn’t.

---

Next: [Rust Format Strings](#rust_002dformat), Previous: [Emacs Lisp Format Strings](#elisp_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.11 librep Format Strings [¶](#librep-Format-Strings)

librep format strings are documented in the librep manual, section
Formatted Output,
<http://librep.sourceforge.net/librep-manual.html#Formatted%20Output>,
<http://www.gwinnup.org/research/docs/librep.html#SEC122>.

---

Next: [Go Format Strings](#go_002dformat), Previous: [librep Format Strings](#librep_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.12 Rust Format Strings [¶](#Rust-Format-Strings)

Rust format strings are those supported by the `formatx` library
<https://crates.io/crates/formatx>.
These are those supported by the `format!` built-in
<https://doc.rust-lang.org/std/fmt/>
with the restrictions listed in
<https://crates.io/crates/formatx>, section "Limitations".

A Rust format string consists of

* an opening brace ‘`{`’,
* an optional non-empty sequence of digits or an optional identifier,
* optionally, a ‘`:`’ and a format specifier,
  where a format specifier is of the form
  `[[fill]align][sign][#][0][minimumwidth][.precision][type]`
  where
  + the fill character is any character,
  + the align flag is one of ‘`<`’, ‘`>`’, ‘`^`’,
  + the sign is one of ‘`+`’, ‘`-`’,
  + the # flag is ‘`#`’,
  + the 0 flag is ‘`0`’,
  + minimumwidth is a non-empty sequence of digits,
  + precision is a non-empty sequence of digits,
  + type is ‘`?`’,
* optional white-space,
* a closing brace ‘`}`’.

Brace characters ‘`{`’ and ‘`}`’ can be escaped by doubling them:
‘`{{`’ and ‘`}}`’.

---

Next: [Ruby Format Strings](#ruby_002dformat), Previous: [Rust Format Strings](#rust_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.13 Go Format Strings [¶](#Go-Format-Strings)

Go format strings are documented
on the Go packages site, for package `fmt`,
at <https://pkg.go.dev/fmt>.

---

Next: [Shell Format Strings](#sh_002dformat), Previous: [Go Format Strings](#go_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.14 Ruby Format Strings [¶](#Ruby-Format-Strings)

Ruby format strings are described in the documentation of the Ruby
functions `format` and `sprintf`, in
<https://ruby-doc.org/core-2.7.1/Kernel.html#method-i-sprintf>.

There are two kinds of format strings in Ruby:

* Those that take a list of arguments without names. They support
  argument reordering by use of the `%n$` syntax. Note
  that if one argument uses this syntax, all must use this syntax.
* Those that take a hash table, containing named arguments. The
  syntax is `%<name>`. Note that `%{name}` is
  equivalent to `%<name>s`.

---

Next: [awk Format Strings](#awk_002dformat), Previous: [Ruby Format Strings](#ruby_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.15 Shell Format Strings [¶](#Shell-Format-Strings)

There are two kinds of format strings in shell scripts:
those with dollar notation for placeholders,
called *Shell format strings*
and labelled as ‘`sh-format`’,
and those acceptable to the ‘`printf`’ command (or shell built-in command),
called *Shell `printf` format strings*
and labelled as ‘`sh-printf-format`’.

Shell format strings, as supported by GNU gettext and the ‘`envsubst`’
program, are strings with references to shell variables in the form
`$variable` or `${variable}`. References of the form
`${variable-default}`,
`${variable:-default}`,
`${variable=default}`,
`${variable:=default}`,
`${variable+replacement}`,
`${variable:+replacement}`,
`${variable?ignored}`,
`${variable:?ignored}`,
that would be valid inside shell scripts, are not supported. The
variable names must consist solely of alphanumeric or underscore
ASCII characters, not start with a digit and be nonempty; otherwise such
a variable reference is ignored.

Shell `printf` format strings are the format strings supported
by the POSIX ‘`printf`’ command
(<https://pubs.opengroup.org/onlinepubs/9799919799/utilities/printf.html>),
including the floating-point conversion specifiers
`a`, `A`, `e`, `E`, `f`, `F`, `g`, `G`,
but without the obsolescent `b` conversion specifier.
Extensions by the GNU coreutils ‘`printf`’ command
(<https://www.gnu.org/software/coreutils/manual/html_node/printf-invocation.html>)
are not supported:
use of the ‘`'`’ flag in the
`%i`, `%d`, `%u`, `%f`, `%F`, `%g`, `%G`
directives;
use of ‘`*`’ or ‘`*m$`’ as width or precision;
use of size specifiers `h`, `l`, `j`, `z`, `t` (ignored);
and the escape sequences `\c`,
`\xnn`, `\unnnn`, `\Unnnnnnnn`.
Extensions by the GNU bash ‘`printf`’ built-in
(<https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html#index-printf>)
are not supported either:
use of ‘`*`’ as width or precision;
use of size specifiers `h`, `l`, `j`, `z`, `t` (ignored);
the `%b`, `%q`, `%Q`, `%T`, `%n` directives;
and the escape sequences
`\xnn`, `\unnnn`, `\Unnnnnnnn`.

---

Next: [Lua Format Strings](#lua_002dformat), Previous: [Shell Format Strings](#sh_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.16 awk Format Strings [¶](#awk-Format-Strings)

awk format strings are described in the gawk documentation, section
Printf,
<https://www.gnu.org/manual/gawk/html_node/Printf.html#Printf>.

---

Next: [Object Pascal Format Strings](#object_002dpascal_002dformat), Previous: [awk Format Strings](#awk_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.17 Lua Format Strings [¶](#Lua-Format-Strings)

Lua format strings are described in the Lua reference manual, section String Manipulation,
<https://www.lua.org/manual/5.1/manual.html#pdf-string.format>.

---

Next: [Modula-2 Format Strings](#modula2_002dformat), Previous: [Lua Format Strings](#lua_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.18 Object Pascal Format Strings [¶](#Object-Pascal-Format-Strings)

Object Pascal format strings are described in the documentation of the
Free Pascal runtime library, section Format,
<https://www.freepascal.org/docs-html/rtl/sysutils/format.html>.

---

Next: [D Format Strings](#d_002dformat), Previous: [Object Pascal Format Strings](#object_002dpascal_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.19 Modula-2 Format Strings [¶](#Modula_002d2-Format-Strings)

Modula-2 format strings are defined as follows:

1. Escape sequences are processed.
   These escape sequences are understood:
   ‘`\a`’, ‘`\b`’, ‘`\e`’, ‘`\f`’, ‘`\n`’, ‘`\r`’,
   ‘`\xhex-digits`’, ‘`\octal-digits`’.
   Other than that, a backslash is ignored.
2. A directive consists of
   * a ‘`%`’ character,
   * optionally a flag character ‘`-`’,
   * optionally a flag character ‘`0`’,
   * optionally a width specification (a nonnegative integer),
   * and finally a specifier:
     ‘`s`’ that formats a string, ‘`c`’ that formats a character,
     ‘`d`’ and ‘`u`’, that format a (signed/unsigned) integer in decimal,
     or ‘`x`’, that formats an unsigned integer in hexadecimal.

   There is also the directive ‘`%%`’, that produces a single percent character.

---

Next: [OCaml Format Strings](#ocaml_002dformat), Previous: [Modula-2 Format Strings](#modula2_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.20 D Format Strings [¶](#D-Format-Strings)

D format strings are described
in the documentation of the D module `std.format`,
at <https://dlang.org/library/std/format.html>.

---

Next: [Smalltalk Format Strings](#smalltalk_002dformat), Previous: [D Format Strings](#d_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.21 OCaml Format Strings [¶](#OCaml-Format-Strings)

OCaml format strings are described
in the documentation of the `Printf` module,
at <https://ocaml.org/manual/5.3/api/Printf.html#VALfprintf>.
In translated strings (`msgstr` values),
arguments can be specified through a number,
using the syntax ‘`%m$`’ instead of ‘`%`’
and ‘`*m$`’ instead of ‘`*`’
to designate the argument number m.

---

Next: [Qt Format Strings](#qt_002dformat), Previous: [OCaml Format Strings](#ocaml_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.22 Smalltalk Format Strings [¶](#Smalltalk-Format-Strings)

Smalltalk format strings are described in the GNU Smalltalk documentation,
class `CharArray`, methods ‘`bindWith:`’ and
‘`bindWithArguments:`’.
<https://www.gnu.org/software/smalltalk/gst-manual/gst_68.html#SEC238>.
In summary, a directive starts with ‘`%`’ and is followed by ‘`%`’
or a nonzero digit (‘`1`’ to ‘`9`’).

---

Next: [Qt Format Strings](#qt_002dplural_002dformat), Previous: [Smalltalk Format Strings](#smalltalk_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.23 Qt Format Strings [¶](#Qt-Format-Strings)

Qt format strings are described in the documentation of the QString class
<file:/usr/lib/qt-4.3.0/doc/html/qstring.html>.
In summary, a directive consists of a ‘`%`’ followed by a digit. The same
directive cannot occur more than once in a format string.

---

Next: [KDE Format Strings](#kde_002dformat), Previous: [Qt Format Strings](#qt_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.24 Qt Format Strings [¶](#Qt-Format-Strings-1)

Qt format strings are described in the documentation of the QObject::tr method
<file:/usr/lib/qt-4.3.0/doc/html/qobject.html>.
In summary, the only allowed directive is ‘`%n`’.

---

Next: [KUIT Format Strings](#kde_002dkuit_002dformat), Previous: [Qt Format Strings](#qt_002dplural_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.25 KDE Format Strings [¶](#KDE-Format-Strings)

KDE 4 format strings are defined as follows:
A directive consists of a ‘`%`’ followed by a non-zero decimal number.
If a ‘`%n`’ occurs in a format strings, all of ‘`%1`’, ..., ‘`%(n-1)`’
must occur as well, except possibly one of them.

---

Next: [Boost Format Strings](#boost_002dformat), Previous: [KDE Format Strings](#kde_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.26 KUIT Format Strings [¶](#KUIT-Format-Strings)

KUIT (KDE User Interface Text) is compatible with KDE 4 format strings,
while it also allows programmers to add semantic information to a format
string, through XML markup tags. For example, if the first format
directive in a string is a filename, programmers could indicate that
with a ‘`filename`’ tag, like ‘`<filename>%1</filename>`’.

KUIT format strings are described in
<https://api.kde.org/frameworks/ki18n/html/prg_guide.html#kuit_markup>.

---

Next: [Tcl Format Strings](#tcl_002dformat), Previous: [KUIT Format Strings](#kde_002dkuit_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.27 Boost Format Strings [¶](#Boost-Format-Strings)

Boost format strings are described in the documentation of the
`boost::format` class, at
<https://www.boost.org/libs/format/doc/format.html>.
In summary, a directive has either the same syntax as in a C format string,
such as ‘`%1$+5d`’, or may be surrounded by vertical bars, such as
‘`%|1$+5d|`’ or ‘`%|1$+5|`’, or consists of just an argument number
between percent signs, such as ‘`%1%`’.

---

Next: [Perl Format Strings](#perl_002dformat), Previous: [Boost Format Strings](#boost_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.28 Tcl Format Strings [¶](#Tcl-Format-Strings)

Tcl format strings are described in the `format.n` manual page,
<http://www.scriptics.com/man/tcl8.3/TclCmd/format.htm>.

---

Next: [PHP Format Strings](#php_002dformat), Previous: [Tcl Format Strings](#tcl_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.29 Perl Format Strings [¶](#Perl-Format-Strings)

There are two kinds of format strings in Perl: those acceptable to the
Perl built-in function `printf`, labelled as ‘`perl-format`’,
and those acceptable to the `libintl-perl` function `__x`,
labelled as ‘`perl-brace-format`’.

Perl `printf` format strings are described in the `sprintf`
section of ‘`man perlfunc`’.

Perl brace format strings are described in the
`Locale::TextDomain(3pm)` manual page of the CPAN package
libintl-perl. In brief, Perl format uses placeholders put between
braces (‘`{`’ and ‘`}`’). The placeholder must have the syntax
of simple identifiers.

---

Next: [GCC internal Format Strings](#gcc_002dinternal_002dformat), Previous: [Perl Format Strings](#perl_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.30 PHP Format Strings [¶](#PHP-Format-Strings)

PHP format strings are described in the documentation of the PHP function
`sprintf`, in `phpdoc/manual/function.sprintf.html` or
<http://www.php.net/manual/en/function.sprintf.php>.

---

Next: [GFC internal Format Strings](#gfc_002dinternal_002dformat), Previous: [PHP Format Strings](#php_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.31 GCC internal Format Strings [¶](#GCC-internal-Format-Strings)

These format strings are used inside the GCC sources.
In such a format string,
a directive starts with ‘`%`’,
is optionally followed by a size specifier ‘`l`’ or ‘`ll`’
or ‘`w`’ or ‘`z`’ or ‘`t`’,
an optional flag ‘`q`’ (indicates quotes),
another optional flag ‘`+`’,
another optional flag ‘`#`’,
and is finished by a specifier:

* ‘`%`’ denotes a literal percent sign,
* ‘`<`’ denotes an opening quote,
* ‘`>`’ or ‘`'`’ denotes a closing quote,
* ‘`r`’ denotes the beginning of a colored text segment,
* ‘`R`’ denotes the end of a colored text segment,
* ‘`{`’ denotes the beginning of a clickable text segment,
* ‘`}`’ denotes the end of a clickable text segment,
* ‘`c`’ denotes a character,
* ‘`s`’ denotes a string,
* ‘`i`’ and ‘`d`’ denote an integer,
* ‘`o`’, ‘`u`’, ‘`x`’ denote an unsigned integer,
* ‘`f`’ denotes a floating-point number,
* ‘`.*s`’ denotes a string preceded by a width specification,
* ‘`D`’ denotes a general declaration,
* ‘`E`’ denotes an expression,
* ‘`F`’ denotes a function declaration,
* ‘`T`’ denotes a type,
* ‘`A`’ denotes a function argument,
* ‘`H`’ denotes the first type in a type difference,
* ‘`I`’ denotes the second type in a type difference,
* ‘`O`’ denotes a binary operator,
* ‘`P`’ denotes a function parameter index,
* ‘`Q`’ denotes an assignment operator,
* ‘`S`’ denotes a substitution,
* ‘`X`’ denotes an exception,
* ‘`V`’ or ‘`v`’ denotes a list of const/volatile qualifiers,
* ‘`C`’ denotes a tree code or the current locus,
* ‘`L`’ denotes a programming language or a locus,
* ‘`p`’, ‘`@`’, ‘`e`’ denote
  various other types of compiler-internal objects,
* ‘`Z`’ denotes an array of integers.

---

Next: [YCP Format Strings](#ycp_002dformat), Previous: [GCC internal Format Strings](#gcc_002dinternal_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.32 GFC internal Format Strings [¶](#GFC-internal-Format-Strings)

These format strings are used inside the GNU Fortran Compiler sources,
that is, the Fortran frontend in the GCC sources, up to GCC 14.x.
In such a format
string, a directive starts with ‘`%`’ and is finished by a
specifier: ‘`%`’ denotes a literal percent sign, ‘`C`’ denotes the
current source location, ‘`L`’ denotes a source location, ‘`c`’
denotes a character, ‘`s`’ denotes a string, ‘`i`’ and ‘`d`’
denote an integer, ‘`u`’ denotes an unsigned integer. ‘`i`’,
‘`d`’, and ‘`u`’ may be preceded by a size specifier ‘`l`’.

---

Previous: [GFC internal Format Strings](#gfc_002dinternal_002dformat), Up: [The Translator’s View](#Translators-for-other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.3.33 YCP Format Strings [¶](#YCP-Format-Strings)

YCP sformat strings are described in the libycp documentation
<file:/usr/share/doc/packages/libycp/YCP-builtins.html>.
In summary, a directive starts with ‘`%`’ and is followed by ‘`%`’
or a nonzero digit (‘`1`’ to ‘`9`’).

---

Next: [Individual Programming Languages](#List-of-Programming-Languages), Previous: [The Translator’s View](#Translators-for-other-Languages), Up: [Other Programming Languages](#Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 16.4 The Maintainer’s View [¶](#The-Maintainer_0027s-View-1)

For the maintainer, the general procedure differs from the C language
case:

* If only a single programming language is used, the `XGETTEXT_OPTIONS`
  variable in `po/Makevars` (see [`Makevars` in `po/`](#po_002fMakevars)) should be adjusted to
  match the `xgettext` options for that particular programming language.
  If the package uses more than one programming language with `gettext`
  support, it becomes necessary to change the POT file construction rule
  in `po/Makefile.in.in`. It is recommended to make one `xgettext`
  invocation per programming language, each with the options appropriate for
  that language, and to combine the resulting files using `msgcat`.

---

Previous: [The Maintainer’s View](#Maintainers-for-other-Languages), Up: [Other Programming Languages](#Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]
