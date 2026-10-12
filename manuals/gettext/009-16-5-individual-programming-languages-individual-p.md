### 16.5 Individual Programming Languages [¶](#Individual-Programming-Languages)

* [C, C++, Objective C](#C)
* [Python](#Python)
* [Java](#Java)
* [C#](#C_0023)
* [JavaScript](#JavaScript)
* [TypeScript and TSX](#TypeScript)
* [GNU guile - Scheme](#Scheme)
* [GNU clisp - Common Lisp](#Common-Lisp)
* [GNU clisp C sources](#clisp-C)
* [Emacs Lisp](#Emacs-Lisp)
* [librep](#librep)
* [Rust](#Rust)
* [Go](#Go)
* [Ruby](#Ruby)
* [sh - Shell Script](#sh)
* [bash - Bourne-Again Shell Script](#bash)
* [GNU awk](#gawk)
* [Lua](#Lua)
* [Pascal - Free Pascal Compiler](#Pascal)
* [Modula-2](#Modula_002d2)
* [D](#D)
* [OCaml](#OCaml)
* [GNU Smalltalk](#Smalltalk)
* [Vala](#Vala)
* [wxWidgets library](#wxWidgets)
* [Tcl - Tk’s scripting language](#Tcl)
* [Perl](#Perl)
* [PHP Hypertext Preprocessor](#PHP)
* [Pike](#Pike)
* [GNU Compiler Collection sources](#GCC_002dsource)
* [YCP - YaST2 scripting language](#YCP)

---

Next: [Python](#Python), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.1 C, C++, Objective C [¶](#C_002c-C_002b_002b_002c-Objective-C)

RPMs
:   gcc, gpp, gobjc, glibc, gettext

Ubuntu packages
:   gcc, g++, gobjc, libc6-dev, libasprintf-dev

File extension
:   For C: `c`, `h`.
      
    For C++: `C`, `c++`, `cc`, `cxx`, `cpp`, `hpp`.
      
    For Objective C: `m`.

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
:   `bindtextdomain` and `wbindtextdomain` functions

setlocale
:   Programmer must call `setlocale (LC_ALL, "")`

Prerequisite
:   `#include <libintl.h>`
      
    `#include <locale.h>`
      
    `#define _(string) gettext (string)`

Use or emulate GNU gettext
:   Use

Extractor
:   `xgettext -k_`
      
    For C++ with the Boost library: `xgettext --boost -k_`

Formatting with positions
:   `fprintf "%2$d %1$d"`
      
    In C++: `autosprintf "%2$d %1$d"`
    (see [Introduction](autosprintf.html#Top) in GNU autosprintf)
      
    In C++ 20 or newer: `std::vformat "{1} {0}"`
      
    In C++ with the Boost library: `boost::format "%2% %1%"`

Portability
:   autoconf (gettext.m4) and #if ENABLE\_NLS

po-mode marking
:   yes

The following examples are available in the `examples` directory:
`hello-c`, `hello-c-gnome2`, `hello-c-gnome3`, `hello-c-http`,
`hello-c++`, `hello-c++20`,
`hello-c++-qt`, `hello-c++-kde`,
`hello-c++-gnome2`, `hello-c++-gnome3`, `hello-c++-wxwidgets`,
`hello-objc`, `hello-objc-gnustep`, `hello-objc-gnome2`.

---

Next: [Java](#Java), Previous: [C, C++, Objective C](#C), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.2 Python [¶](#Python-1)

RPMs
:   python

Ubuntu packages
:   python

File extension
:   `py`

String syntax
:   `'abc'`, `u'abc'`, `r'abc'`, `ur'abc'`,
      
    `"abc"`, `u"abc"`, `r"abc"`, `ur"abc"`,
      
    `'''abc'''`, `u'''abc'''`, `r'''abc'''`, `ur'''abc'''`,
      
    `"""abc"""`, `u"""abc"""`, `r"""abc"""`, `ur"""abc"""`

gettext shorthand
:   `_('abc')` etc.

gettext/ngettext functions
:   `gettext.gettext`, `gettext.dgettext`,
    `gettext.ngettext`, `gettext.dngettext`,
    also `ugettext`, `ungettext`

textdomain
:   `gettext.textdomain` function, or
    `gettext.install(domain)` function

bindtextdomain
:   `gettext.bindtextdomain` function, or
    `gettext.install(domain,localedir)` function

setlocale
:   not used by the gettext emulation

Prerequisite
:   `import gettext`

Use or emulate GNU gettext
:   emulate

Extractor
:   `xgettext`

Formatting with positions
:   `'...%(ident)d...' % { 'ident': value }`
      
    `'...{ident}...'.format(ident=value)`
      
    `'...{0}...{1}...'.format(value0,value1)`
      
    `'...{}...{}...'.format(value0,value1)`

Portability
:   fully portable

po-mode marking
:   —

An example is available in the `examples` directory: `hello-python`.

A note about format strings: Python supports format strings with unnamed
arguments, such as `'...%d...'`, and format strings with named arguments,
such as `'...%(ident)d...'`. The latter are preferable for
internationalized programs, for two reasons:

* When a format string takes more than one argument, the translator can provide
  a translation that uses the arguments in a different order, if the format
  string uses named arguments. For example, the translator can reformulate

  ```
  "'%(volume)s' has only %(freespace)d bytes free."
  ```

  to

  ```
  "Only %(freespace)d bytes free on '%(volume)s'."
  ```

  Additionally, the identifiers also provide some context to the translator.
* In the context of plural forms, the format string used for the singular form
  does not use the numeric argument in many languages. Even in English, one
  prefers to write `"one hour"` instead of `"1 hour"`. Omitting
  individual arguments from format strings like this is only possible with
  the named argument syntax. (With unnamed arguments, Python – unlike C –
  verifies that the format string uses all supplied arguments.)

A note about f-strings (PEP 498): `xgettext`

* syntactically recognizes f-strings,
* is able to extract f-strings that contain no sub-expressions.

However, `xgettext` does not extract f-strings marked for translation
that contain sub-expressions. This will not work as expected:

```
_(f"The file {file[i]} does not exist.")
```

because the translator is generally not a programmer and should thus not be
confronted with expressions from the programming language.

#### Related software [¶](#Related-software)

An internationalization system based on GNU gettext and PO files is
[Babel](https://babel.pocoo.org/).

---

Next: [C#](#C_0023), Previous: [Python](#Python), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.3 Java [¶](#Java-1)

RPMs
:   java, java2

Ubuntu packages
:   default-jdk

File extension
:   `java`

String syntax
:   "abc", """text block"""

gettext shorthand
:   i18n("abc")

gettext/ngettext functions
:   `GettextResource.gettext`, `GettextResource.ngettext`,
    `GettextResource.pgettext`, `GettextResource.npgettext`

textdomain
:   —, use `ResourceBundle.getResource` instead

bindtextdomain
:   —, use CLASSPATH instead

setlocale
:   automatic

Prerequisite
:   —

Use or emulate GNU gettext
:   —, uses a Java specific message catalog format

Extractor
:   `xgettext -ki18n`

Formatting with positions
:   `MessageFormat.format "{1,number} {0,number}"`
    or `String.format "%2$d %1$d"`

Portability
:   fully portable

po-mode marking
:   —

Before marking strings as internationalizable, uses of the string
concatenation operator need to be converted to `MessageFormat`
applications. For example, `"file "+filename+" not found"` becomes
`MessageFormat.format("file {0} not found", new Object[] { filename })`.
Only after this is done, can the strings be marked and extracted.

GNU gettext uses the native Java internationalization mechanism, namely
`ResourceBundle`s. There are two formats of `ResourceBundle`s:
`.properties` files and `.class` files. The `.properties`
format is a text file which the translators can directly edit, like PO
files, but which doesn’t support plural forms. Whereas the `.class`
format is compiled from `.java` source code and can support plural
forms (provided it is accessed through an appropriate API, see below).

To convert a PO file to a `.properties` file, the `msgcat`
program can be used with the option `--properties-output`. To convert
a `.properties` file back to a PO file, the `msgcat` program
can be used with the option `--properties-input`. All the tools
that manipulate PO files can work with `.properties` files as well,
if given the `--properties-input` and/or `--properties-output`
option.

To convert a PO file to a ResourceBundle class, the `msgfmt` program
can be used with the option `--java` or `--java2`. To convert a
ResourceBundle back to a PO file, the `msgunfmt` program can be used
with the option `--java`.

Two different programmatic APIs can be used to access ResourceBundles.
Note that both APIs work with all kinds of ResourceBundles, whether
GNU gettext generated classes, or other `.class` or `.properties`
files.

1. The `java.util.ResourceBundle` API.

   In particular, its `getString` function returns a string translation.
   Note that a missing translation yields a `MissingResourceException`.

   This has the advantage of being the standard API. And it does not require
   any additional libraries, only the `msgcat` generated `.properties`
   files or the `msgfmt` generated `.class` files. But it cannot do
   plural handling, even if the resource was generated by `msgfmt` from
   a PO file with plural handling.
2. The `gnu.gettext.GettextResource` API.

   Reference documentation in Javadoc 1.1 style format is in the
   [javadoc2 directory](javadoc2/index.html).

   Its `gettext` function returns a string translation. Note that when
   a translation is missing, the msgid argument is returned unchanged.

   This has the advantage of having the `ngettext` function for plural
   handling and the `pgettext` and `npgettext` for strings constraint
   to a particular context.

   To use this API, one needs the `libintl.jar` file which is part of
   the GNU gettext package and distributed under the LGPL.

Four examples, using the second API, are available in the `examples`
directory: `hello-java`, `hello-java-awt`, `hello-java-swing`,
`hello-java-qtjambi`.

Now, to make use of the API and define a shorthand for ‘`getString`’,
there are three idioms that you can choose from:

* (This one assumes Java 1.5 or newer.)
  In a unique class of your project, say ‘`Util`’, define a static variable
  holding the `ResourceBundle` instance and the shorthand:

  ```
  private static ResourceBundle myResources =
    ResourceBundle.getBundle("domain-name");
  public static String i18n(String s) {
    return myResources.getString(s);
  }
  ```

  All classes containing internationalized strings then contain

  ```
  import static Util.i18n;
  ```

  and the shorthand is used like this:

  ```
  System.out.println(i18n("Operation completed."));
  ```
* In a unique class of your project, say ‘`Util`’, define a static variable
  holding the `ResourceBundle` instance:

  ```
  public static ResourceBundle myResources =
    ResourceBundle.getBundle("domain-name");
  ```

  All classes containing internationalized strings then contain

  ```
  private static ResourceBundle res = Util.myResources;
  private static String i18n(String s) { return res.getString(s); }
  ```

  and the shorthand is used like this:

  ```
  System.out.println(i18n("Operation completed."));
  ```
* You add a class with a very short name, say ‘`S`’, containing just the
  definition of the resource bundle and of the shorthand:

  ```
  public class S {
    public static ResourceBundle myResources =
      ResourceBundle.getBundle("domain-name");
    public static String i18n(String s) {
      return myResources.getString(s);
    }
  }
  ```

  and the shorthand is used like this:

  ```
  System.out.println(S.i18n("Operation completed."));
  ```

Which of the three idioms you choose, will depend on whether your project
requires portability to Java versions prior to Java 1.5 and, if so, whether
copying two lines of codes into every class is more acceptable in your project
than a class with a single-letter name.

---

Next: [JavaScript](#JavaScript), Previous: [Java](#Java), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.4 C# [¶](#C_0023-1)

RPMs
:   mono or dotnet8.0

Ubuntu packages
:   mono-mcs or dotnet8

File extension
:   `cs`

String syntax
:   `"abc"`, `@"abc"`

gettext shorthand
:   \_("abc")

gettext/ngettext functions
:   `GettextResourceManager.GetString`,
    `GettextResourceManager.GetPluralString`
    `GettextResourceManager.GetParticularString`
    `GettextResourceManager.GetParticularPluralString`

textdomain
:   `new GettextResourceManager(domain)`

bindtextdomain
:   —, compiled message catalogs are located in subdirectories of the directory
    containing the executable

setlocale
:   automatic

Prerequisite
:   —

Use or emulate GNU gettext
:   —, uses a C# specific message catalog format

Extractor
:   `xgettext -k_`

Formatting with positions
:   `String.Format "{1} {0}"`

Portability
:   fully portable

po-mode marking
:   —

Before marking strings as internationalizable, uses of the string
concatenation operator need to be converted to `String.Format`
invocations. For example, `"file "+filename+" not found"` becomes
`String.Format("file {0} not found", filename)`.
Only after this is done, can the strings be marked and extracted.

GNU gettext uses the native C#/.NET internationalization mechanism, namely
the classes `ResourceManager` and `ResourceSet`. Applications
use the `ResourceManager` methods to retrieve the native language
translation of strings. An instance of `ResourceSet` is the in-memory
representation of a message catalog file. The `ResourceManager` loads
and accesses `ResourceSet` instances as needed to look up the
translations.

There are two formats of `ResourceSet`s that can be directly loaded by
the C# runtime: `.resources` files and `.dll` files.

* The `.resources` format is a binary file usually generated through the
  `resgen` or `monoresgen` utility, but which doesn’t support plural
  forms. `.resources` files can also be embedded in .NET `.exe` files.
  This only affects whether a file system access is performed to load the message
  catalog; it doesn’t affect the contents of the message catalog.
* On the other hand, the `.dll` format is a binary file that is compiled
  from `.cs` source code and can support plural forms (provided it is
  accessed through the GNU gettext API, see below).

Note that these .NET `.dll` and `.exe` files are not tied to a
particular platform; their file format and GNU gettext for C# can be used
on any platform.

To convert a PO file to a `.resources` file, the `msgfmt` program
can be used with the option ‘`--csharp-resources`’. To convert a
`.resources` file back to a PO file, the `msgunfmt` program can be
used with the option ‘`--csharp-resources`’. You can also, in some cases,
use the `monoresgen` program (from the `mono`/`mcs` package).
This program can also convert a `.resources` file back to a PO file. But
beware: as of this writing (January 2004), the `monoresgen` converter is
quite buggy.

To convert a PO file to a `.dll` file, the `msgfmt` program can be
used with the option `--csharp`. The result will be a `.dll` file
containing a subclass of `GettextResourceSet`, which itself is a subclass
of `ResourceSet`. To convert a `.dll` file containing a
`GettextResourceSet` subclass back to a PO file, the `msgunfmt`
program can be used with the option `--csharp`.

The advantages of the `.dll` format over the `.resources` format
are:

1. Freedom to localize: Users can add their own translations to an application
   after it has been built and distributed. Whereas when the programmer uses
   a `ResourceManager` constructor provided by the system, the set of
   `.resources` files for an application must be specified when the
   application is built and cannot be extended afterwards.
2. Plural handling: A message catalog in `.dll` format supports the plural
   handling function `GetPluralString`. Whereas `.resources` files can
   only contain data and only support lookups that depend on a single string.
3. Context handling: A message catalog in `.dll` format supports the
   query-with-context functions `GetParticularString` and
   `GetParticularPluralString`. Whereas `.resources` files can
   only contain data and only support lookups that depend on a single string.
4. The `GettextResourceManager` that loads the message catalogs in
   `.dll` format also provides for inheritance on a per-message basis.
   For example, in Austrian (`de_AT`) locale, translations from the German
   (`de`) message catalog will be used for messages not found in the
   Austrian message catalog. This has the consequence that the Austrian
   translators need only translate those few messages for which the translation
   into Austrian differs from the German one. Whereas when working with
   `.resources` files, each message catalog must provide the translations
   of all messages by itself.
5. The `GettextResourceManager` that loads the message catalogs in
   `.dll` format also provides for a fallback: The English msgid is
   returned when no translation can be found. Whereas when working with
   `.resources` files, a language-neutral `.resources` file must
   explicitly be provided as a fallback.

On the side of the programmatic APIs, the programmer can use either the
standard `ResourceManager` API and the GNU `GettextResourceManager`
API. The latter is an extension of the former, because
`GettextResourceManager` is a subclass of `ResourceManager`.

1. The `System.Resources.ResourceManager` API.

   This API works with resources in `.resources` format.

   The creation of the `ResourceManager` is done through

   ```
     new ResourceManager(domainname, Assembly.GetExecutingAssembly())
   ```

   The `GetString` function returns a string’s translation. Note that this
   function returns null when a translation is missing (i.e. not even found in
   the fallback resource file).
2. The `GNU.Gettext.GettextResourceManager` API.

   This API works with resources in `.dll` format.

   Reference documentation is in the
   [csharpdoc directory](csharpdoc/index.html).

   The creation of the `ResourceManager` is done through

   ```
     new GettextResourceManager(domainname)
   ```

   The `GetString` function returns a string’s translation. Note that when
   a translation is missing, the msgid argument is returned unchanged.

   The `GetPluralString` function returns a string translation with plural
   handling, like the `ngettext` function in C.

   The `GetParticularString` function returns a string’s translation,
   specific to a particular context, like the `pgettext` function in C.
   Note that when a translation is missing, the msgid argument is returned
   unchanged.

   The `GetParticularPluralString` function returns a string translation,
   specific to a particular context, with plural handling, like the
   `npgettext` function in C.

   To use this API, one needs the `GNU.Gettext.dll` file which is part of
   the GNU gettext package and distributed under the LGPL.

You can also mix both approaches: use the
`GNU.Gettext.GettextResourceManager` constructor, but otherwise use
only the `ResourceManager` type and only the `GetString` method.
This is appropriate when you want to profit from the tools for PO files,
but don’t want to change an existing source code that uses
`ResourceManager` and don’t (yet) need the `GetPluralString` method.

Two examples, using the second API, are available in the `examples`
directory: `hello-csharp`, `hello-csharp-forms`.

Now, to make use of the API and define a shorthand for ‘`GetString`’,
there are two idioms that you can choose from:

* In a unique class of your project, say ‘`Util`’, define a static variable
  holding the `ResourceManager` instance:

  ```
  public static GettextResourceManager MyResourceManager =
    new GettextResourceManager("domain-name");
  ```

  All classes containing internationalized strings then contain

  ```
  private static GettextResourceManager Res = Util.MyResourceManager;
  private static String _(String s) { return Res.GetString(s); }
  ```

  and the shorthand is used like this:

  ```
  Console.WriteLine(_("Operation completed."));
  ```
* You add a class with a very short name, say ‘`S`’, containing just the
  definition of the resource manager and of the shorthand:

  ```
  public class S {
    public static GettextResourceManager MyResourceManager =
      new GettextResourceManager("domain-name");
    public static String _(String s) {
       return MyResourceManager.GetString(s);
    }
  }
  ```

  and the shorthand is used like this:

  ```
  Console.WriteLine(S._("Operation completed."));
  ```

Which of the two idioms you choose, will depend on whether copying two lines
of codes into every class is more acceptable in your project than a class
with a single-letter name.

---

Next: [TypeScript and TSX](#TypeScript), Previous: [C#](#C_0023), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.5 JavaScript [¶](#JavaScript-1)

RPMs
:   js

Ubuntu packages
:   gjs

File extension
:   `js`

String syntax
:   * `"abc"`
    * `'abc'`
    * `` `abc` ``
    * `` tag`abc${expression}def{expression}...` ``,
      see the description of ‘`--tag`’ in [Invoking the `xgettext` Program](#xgettext-Invocation).

gettext shorthand
:   `_("abc")`

gettext/ngettext functions
:   `gettext`, `dgettext`, `dcgettext`, `ngettext`,
    `dngettext`

textdomain
:   `textdomain` function

bindtextdomain
:   `bindtextdomain` function

setlocale
:   automatic

Prerequisite
:   —

Use or emulate GNU gettext
:   use, or emulate

Extractor
:   `xgettext`

Formatting with positions
:   A `format` method on strings can be used.
    But since it is not standard in JavaScript,
    you have to enable it yourself, through

    ```
    const Format = imports.format;
    String.prototype.format = Format.format;
    ```

Portability
:   On platforms without gettext, the functions are not available.

po-mode marking
:   —

---

Next: [GNU guile - Scheme](#Scheme), Previous: [JavaScript](#JavaScript), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.6 TypeScript and TSX [¶](#TypeScript-and-TSX)

RPMs
:   js

Ubuntu packages
:   gjs

File extension
:   `ts` for TypeScript, `tsx` for TSX (TypeScript with JSX)

String syntax
:   * `"abc"`
    * `'abc'`
    * `` `abc` ``

gettext shorthand
:   `_("abc")`

gettext/ngettext functions
:   `gettext`, `dgettext`, `dcgettext`, `ngettext`,
    `dngettext`

textdomain
:   `textdomain` function

bindtextdomain
:   `bindtextdomain` function

setlocale
:   automatic

Prerequisite
:   unknown

Use or emulate GNU gettext
:   use, or emulate

Extractor
:   `xgettext`

Formatting with positions
:   A `format` method on strings can be used.
    But since it is not standard in TypeScript,
    you have to enable it yourself.

Portability
:   On platforms without gettext, the functions are not available.

po-mode marking
:   —

---

Next: [GNU clisp - Common Lisp](#Common-Lisp), Previous: [TypeScript and TSX](#TypeScript), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.7 GNU guile - Scheme [¶](#GNU-guile-_002d-Scheme)

RPMs
:   guile

Ubuntu packages
:   guile-2.0

File extension
:   `scm`

String syntax
:   `"abc"`

gettext shorthand
:   `(_ "abc")`, `_"abc"` (GIMP script-fu extension)

gettext/ngettext functions
:   `gettext`, `ngettext`

textdomain
:   `textdomain`

bindtextdomain
:   `bindtextdomain`

setlocale
:   `(catch #t (lambda () (setlocale LC_ALL "")) (lambda args #f))`

Prerequisite
:   `(use-modules (ice-9 format))`

Use or emulate GNU gettext
:   use

Extractor
:   `xgettext -L Guile -k_`

    ‘`xgettext -L Scheme`’ and ‘`xgettext -L Guile`’ are nearly equivalent.
    They differ in the interpretation of escape sequences in string literals:
    While ‘`xgettext -L Scheme`’ assumes the
    [R6RS](https://www.r6rs.org/) and
    [R7RS](https://standards.scheme.org/corrected-r7rs/r7rs.html)
    syntax of string literals,
    ‘`xgettext -L Guile`’ assumes the syntax of string literals
    understood by Guile 2.x and 3.0
    (without command-line option `--r6rs` or `--r7rs`,
    and before a `#!r6rs` directive is seen).
    After a `#!r6rs` directive,
    there is no difference any more
    between ‘`xgettext -L Scheme`’ and ‘`xgettext -L Guile`’
    for the rest of the file.

Formatting with positions
:   —

Portability
:   On platforms without gettext, no translation.

po-mode marking
:   —

An example is available in the `examples` directory: `hello-guile`.

---

Next: [GNU clisp C sources](#clisp-C), Previous: [GNU guile - Scheme](#Scheme), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.8 GNU clisp - Common Lisp [¶](#GNU-clisp-_002d-Common-Lisp)

RPMs
:   clisp 2.28 or newer

Ubuntu packages
:   clisp

File extension
:   `lisp`

String syntax
:   `"abc"`

gettext shorthand
:   `(_ "abc")`, `(ENGLISH "abc")`

gettext/ngettext functions
:   `i18n:gettext`, `i18n:ngettext`

textdomain
:   `i18n:textdomain`

bindtextdomain
:   `i18n:textdomaindir`

setlocale
:   automatic

Prerequisite
:   —

Use or emulate GNU gettext
:   use

Extractor
:   `xgettext -k_ -kENGLISH`

Formatting with positions
:   `format "~1@*~D ~0@*~D"`

Portability
:   On platforms without gettext, no translation.

po-mode marking
:   —

An example is available in the `examples` directory: `hello-clisp`.

---

Next: [Emacs Lisp](#Emacs-Lisp), Previous: [GNU clisp - Common Lisp](#Common-Lisp), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.9 GNU clisp C sources [¶](#GNU-clisp-C-sources)

RPMs
:   clisp

Ubuntu packages
:   clisp

File extension
:   `d`

String syntax
:   `"abc"`

gettext shorthand
:   `ENGLISH ? "abc" : ""`
      
    `GETTEXT("abc")`
      
    `GETTEXTL("abc")`

gettext/ngettext functions
:   `clgettext`, `clgettextl`

textdomain
:   —

bindtextdomain
:   —

setlocale
:   automatic

Prerequisite
:   `#include "lispbibl.c"`

Use or emulate GNU gettext
:   use

Extractor
:   `clisp-xgettext`

Formatting with positions
:   `fprintf "%2$d %1$d"`

Portability
:   On platforms without gettext, no translation.

po-mode marking
:   —

---

Next: [librep](#librep), Previous: [GNU clisp C sources](#clisp-C), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.10 Emacs Lisp [¶](#Emacs-Lisp-1)

RPMs
:   emacs, xemacs

Ubuntu packages
:   emacs, xemacs21

File extension
:   `el`

String syntax
:   `"abc"`

gettext shorthand
:   `(_"abc")`

gettext/ngettext functions
:   `gettext`, `dgettext` (xemacs only)

textdomain
:   `domain` special form (xemacs only)

bindtextdomain
:   `bind-text-domain` function (xemacs only)

setlocale
:   automatic

Prerequisite
:   —

Use or emulate GNU gettext
:   use

Extractor
:   `xgettext`

Formatting with positions
:   `format "%2$d %1$d"`

Portability
:   Only XEmacs. Without `I18N3` defined at build time, no translation.

po-mode marking
:   —

---

Next: [Rust](#Rust), Previous: [Emacs Lisp](#Emacs-Lisp), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.11 librep [¶](#librep-1)

RPMs
:   librep 0.15.3 or newer

Ubuntu packages
:   librep16

File extension
:   `jl`

String syntax
:   `"abc"`

gettext shorthand
:   `(_"abc")`

gettext/ngettext functions
:   `gettext`

textdomain
:   `textdomain` function

bindtextdomain
:   `bindtextdomain` function

setlocale
:   —

Prerequisite
:   `(require 'rep.i18n.gettext)`

Use or emulate GNU gettext
:   use

Extractor
:   `xgettext`

Formatting with positions
:   `format "%2$d %1$d"`

Portability
:   On platforms without gettext, no translation.

po-mode marking
:   —

An example is available in the `examples` directory: `hello-librep`.

---

Next: [Go](#Go), Previous: [librep](#librep), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.12 Rust [¶](#Rust-1)

RPMs
:   rust, rust-cargo

Ubuntu packages
:   rustc, cargo

File extension
:   `rs`

String syntax
:   `"abc"`, `r"abc"`, `r#"abc"#` etc.

gettext shorthand
:   —

gettext/ngettext functions
:   `gettext`, `ngettext`

textdomain
:   `textdomain` function

bindtextdomain
:   `bindtextdomain` function

setlocale
:   `setlocale` function

Prerequisite
:   `$ cargo add gettext-rs`

    `use gettextrs::*;`

    Note: We recommend the ‘`gettext-rs`’ crate.
    We do not recommend the ‘`gettext`’ crate, because
    (as of 2025) it does not handle
    catalog fallback (e.g. from `de_AT` to `de`)
    nor the `LANGUAGE` environment variable.

Use or emulate GNU gettext
:   use

Extractor
:   `xgettext`

Formatting with positions
:   There are three common ways of doing string formatting in Rust:

    * Using the built-ins `format!`, `println!`, etc.
      This facility supports only constant strings, known at compile-time.
      Thus it cannot be used with translated format strings.
      You would get an error such as
      “error: format argument must be a string literal”.
    * Using the `strfmt` library.
      The facility cannot be recommended,
      because it does not support the case where
      some of the values are strings and some of the values are numbers
      (without an excessive amount of contortions).
    * Using the `formatx` library.
      This is the one we recommend.

    So, you have to convert the `format!`, `println!`, etc.
    invocations to use `formatx`.
    For example,

    ```
    println!("Hello {}, you got {} coins.", name, left);
    ```

    becomes

    ```
    println!("{}", formatx!(gettext("Hello {}, you got {} coins."),
                            name, left)
                   .unwrap());
    ```

    For swapped positions, a translator may translate
    `"Hello {}, you got {} coins."`
    with
    `"Hello, {1} coins are left for you, {0}."`

Portability
:   fully portable

po-mode marking
:   —

An example is available in the `examples` directory: `hello-rust`.

---

Next: [Ruby](#Ruby), Previous: [Rust](#Rust), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.13 Go [¶](#Go-1)

Three packages are available,
that can be used for message localization with PO files:

* The `github.com/leonelquinteros/gotext` package.

  Documentation: <https://pkg.go.dev/github.com/leonelquinteros/gotext>

  Source code: <https://github.com/leonelquinteros/gotext>
* The `github.com/gosexy/gettext` package.

  Documentation: <https://pkg.go.dev/github.com/gosexy/gettext>

  Source code: <https://github.com/gosexy/gettext>
* The `github.com/snapcore/go-gettext` package.

  Documentation: <https://pkg.go.dev/github.com/snapcore/go-gettext>

  Source code: <https://github.com/canonical/go-gettext>

Go programs can be classified as one of:

* *Single-locale* programs,
  that use the same locale across all threads of the program.
  Example: Most command-line programs.
* *Multi-locale* programs,
  that use one locale per thread.
  Example: Web servers.

The three different packages support these two classes of programs differently:

* `github.com/leonelquinteros/gotext` package:
  It has two different APIs,
  one for the single-locale case and one for the multi-locale case.
* `github.com/gosexy/gettext` package:
  Its API supports only the single-locale case.
* `github.com/snapcore/go-gettext` package:
  Its API supports the single-locale case and the multi-locale case in the same way.

#### Gettext support characteristics: [¶](#Gettext-support-characteristics_003a)

RPMs
:   golang

Ubuntu packages
:   golang-go (which provides the `go` program),
    or gccgo (which provides a `go-version` command).  
    gccgo has better portability; for example it works on SPARC CPUs.

File extension
:   `go`

String syntax
:   `"abc"`, `` `abc` ``

gettext shorthand
:   —

gettext/ngettext functions
:   This depends on the API:

    * `github.com/leonelquinteros/gotext` API:
      `Get`, `GetD`, `GetN`, `GetND`
    * `github.com/gosexy/gettext` API:
      `Gettext`, `DGettext`, `DCGettext`, `NGettext`,
      `DNGettext`, `DCNGettext`
    * `github.com/snapcore/go-gettext` API:
      `Gettext`, `NGettext`

    Note that the `ngettext`-like functions need to take
    two argument strings that consume the same number of arguments.
    For example, you cannot write
    `fmt.Sprintf(gotext.GetN("a piece", "%d pieces", n), n)`
    because in the singular case,
    `fmt.Sprintf` would treat the unused argument as an error and
    produce `"a piece%!(EXTRA int=1)"` instead of the desired `"a piece"`.
    As a workaround, you need to convert `n` to a string and
    format that string with precision zero:
    `fmt.Sprintf(gotext.GetN("%.0sa piece", "%s pieces", n), strconv.Itoa(n))`

textdomain
:   This depends on the API:

    * `github.com/leonelquinteros/gotext` API:
      `Locale.AddDomain` method or `gotext.Configure` function
    * `github.com/gosexy/gettext` API:
      `Textdomain` function
    * `github.com/snapcore/go-gettext` API:
      `TextDomain` constructor

bindtextdomain
:   This depends on the API:

    * `github.com/leonelquinteros/gotext` API:
      `gotext.NewLocale` function or `gotext.Configure` function
    * `github.com/gosexy/gettext` API:
      `BindTextdomain` function
    * `github.com/snapcore/go-gettext` API:
      `TextDomain` constructor

setlocale
:   This depends on the API:

    * `github.com/leonelquinteros/gotext` API:
      Programmer must determine the appropriate locale and pass it to the
      `gotext.NewLocale` function or `gotext.Configure` function.
    * `github.com/gosexy/gettext` API:
      Programmer must call `gettext.SetLocale(gettext.LcAll, "")`.
    * `github.com/snapcore/go-gettext` API:
      Programmer must determine the appropriate locale and pass it to the
      `TextDomain.Locale` method.

Prerequisite
:   This depends on the API:

    * `github.com/leonelquinteros/gotext` API:
      `import ("github.com/leonelquinteros/gotext")`
    * `github.com/gosexy/gettext` API:
      `import ("github.com/gosexy/gettext")`
    * `github.com/snapcore/go-gettext` API:
      `import ("github.com/snapcore/go-gettext")`

Use or emulate GNU gettext
:   This depends on the API:

    * `github.com/leonelquinteros/gotext` API:
      Emulate
    * `github.com/gosexy/gettext` API:
      Use
    * `github.com/snapcore/go-gettext` API:
      Emulate

Extractor
:   `xgettext`

Formatting with positions
:   `fmt.Sprintf("%[2]d %[1]d", ...)`

Portability
:   fully portable

po-mode marking
:   —

Two examples are available in the `examples` directory:
`hello-go` and `hello-go-http`.

---

Next: [sh - Shell Script](#sh), Previous: [Go](#Go), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.14 Ruby [¶](#Ruby-1)

RPMs
:   ruby, ruby-gettext

Ubuntu packages
:   ruby, ruby-gettext

File extension
:   `rb`

String syntax
:   `"abc"`, `'abc'`, `%q/abc/` etc.,
    `%q(abc)`, `%q[abc]`, `%q{abc}`

gettext shorthand
:   `_("abc")`

gettext/ngettext functions
:   `gettext`, `ngettext`

textdomain
:   —

bindtextdomain
:   `bindtextdomain` function

setlocale
:   —

Prerequisite
:   `require 'gettext'`
    `include GetText`

Use or emulate GNU gettext
:   emulate

Extractor
:   `xgettext`

Formatting with positions
:   `sprintf("%2$d %1$d", x, y)`
      
    `"%{new} replaces %{old}" % {:old => oldvalue, :new => newvalue}`

Portability
:   fully portable

po-mode marking
:   —

An example is available in the `examples` directory: `hello-ruby`.

---

Next: [bash - Bourne-Again Shell Script](#bash), Previous: [Ruby](#Ruby), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15 sh - Shell Script [¶](#sh-_002d-Shell-Script)

RPMs
:   bash, gettext

Ubuntu packages
:   bash, gettext-base

File extension
:   `sh`

String syntax
:   `"abc"`, `'abc'`, `abc`

gettext shorthand
:   `` "`gettext \"abc\"`" ``

gettext/ngettext functions [¶](#index-gettext)
:   `gettext`, `ngettext` programs
      
    `eval_gettext`, `eval_ngettext`, `eval_pgettext`,
    `eval_npgettext` shell functions
      
    `printf_gettext`, `printf_ngettext` commands

textdomain [¶](#index-TEXTDOMAIN_002c-environment-variable)
:   environment variable `TEXTDOMAIN`

bindtextdomain [¶](#index-TEXTDOMAINDIR_002c-environment-variable)
:   environment variable `TEXTDOMAINDIR`

setlocale
:   automatic

Prerequisite
:   For the shell functions approach: `. gettext.sh`

Use or emulate GNU gettext
:   use

Extractor
:   `xgettext`

Formatting with positions
:   There are three approaches; see below for details.

Portability
:   fully portable

po-mode marking
:   —

An example is available in the `examples` directory: `hello-sh`.

* [Three approaches for Shell Scripts](#sh-_002d-Three-approaches)
* [The `gettext.sh` approach](#The-gettext_002esh-approach)
* [The `printf` approach](#The-printf-approach)
* [The `printf_gettext` approach](#The-printf_005fgettext-approach)
* [Preparing Shell Scripts for the `gettext.sh` approach](#Preparing-for-gettext_002esh)
* [Preparing Shell Scripts for the `printf` approach](#Preparing-for-printf)
* [Preparing Shell Scripts for the `printf_gettext` approach](#Preparing-for-printf_005fgettext)
* [Invoking the `gettext` program](#gettext-Invocation)
* [Invoking the `ngettext` program](#ngettext-Invocation)
* [Invoking the `printf_gettext` program](#printf_005fgettext-Invocation)
* [Invoking the `printf_ngettext` program](#printf_005fngettext-Invocation)
* [Invoking the `envsubst` program](#envsubst-Invocation)
* [Invoking the `eval_gettext` function](#eval_005fgettext-Invocation)
* [Invoking the `eval_ngettext` function](#eval_005fngettext-Invocation)
* [Invoking the `eval_pgettext` function](#eval_005fpgettext-Invocation)
* [Invoking the `eval_npgettext` function](#eval_005fnpgettext-Invocation)

---

Next: [The `gettext.sh` approach](#The-gettext_002esh-approach), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.1 Three approaches for Shell Scripts [¶](#Three-approaches-for-Shell-Scripts)

There are three approaches for internationalizing shell scripts.

The common part is how to internationalize a message without parameters:
This is done through the invocation of the `gettext` program,
see [Invoking the `gettext` program](#gettext-Invocation).

The approaches differ regarding how to internationalize a message with parameters, that is, what is called “formatted output” in other programming languages.

---

Next: [The `printf` approach](#The-printf-approach), Previous: [Three approaches for Shell Scripts](#sh-_002d-Three-approaches), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.2 The `gettext.sh` approach [¶](#The-gettext_002esh-approach-1)

This approach uses shell functions defined in the `gettext.sh` file.

Here’s an example that references a shell variable `pid`:

```
eval_gettext "Running as process number \$pid."; echo
```

A ready-to-use example is available in the `hello-1.sh` file
in the `examples/hello-sh` directory.

#### Contents of `gettext.sh` [¶](#Contents-of-gettext_002esh)

`gettext.sh`, contained in the run-time package of GNU gettext,
provides the following:

`$echo`
:   The variable `echo` is set to a command that outputs its first argument
    and a newline, without interpreting backslashes in the argument string.

`eval_gettext`
:   See [Invoking the `eval_gettext` function](#eval_005fgettext-Invocation).

`eval_ngettext`
:   See [Invoking the `eval_ngettext` function](#eval_005fngettext-Invocation).

`eval_pgettext`
:   See [Invoking the `eval_pgettext` function](#eval_005fpgettext-Invocation).

`eval_npgettext`
:   See [Invoking the `eval_npgettext` function](#eval_005fnpgettext-Invocation).

#### Advantages and Drawbacks [¶](#Advantages-and-Drawbacks)

Advantages:

* Portability: This is fully portable.

Drawbacks:

* The strings passed to the `eval_gettext` et al. functions
  must not start nor end with a newline.
  This means, in order to add a newline after a message,
  you need to add an explicit invocation of `echo`.
* References to shell variables must be introduced with ‘`\$`’,
  not just ‘`$`’.
* Speed:
  Each invocation of the `eval_gettext` et al. functions
  uses a pipe and a subshell.
  This is slower than the `printf_gettext` approach.

---

Next: [The `printf_gettext` approach](#The-printf_005fgettext-approach), Previous: [The `gettext.sh` approach](#The-gettext_002esh-approach), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.3 The `printf` approach [¶](#The-printf-approach-1)

This approach uses a POSIX:2024 compliant `printf` command.

Here’s an example that references a shell variable `pid`:

```
env printf "`gettext \"Running as process number %u.\"`"'\n' $pid
```

An example is available in the `hello-2.sh` file
in the `examples/hello-sh` directory.

#### Advantages and Drawbacks [¶](#Advantages-and-Drawbacks-1)

Drawbacks:

* Portability:
  A POSIX compliant
   [`printf`](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/printf.html)
  command is required, such as the one
  from GNU coreutils 9.6 or newer,
  from FreeBSD 11 or newer,
  or from Solaris 11 OpenIndiana or Solaris 11 OmniOS. At the time of this writing (2025),
  the only known shells whose `printf` built-in is POSIX compliant
  are `ksh93` and `zsh`;
  `bash` and `dash` are not.
  For this reason, the code needs to use `env printf`, not `printf`,
  so as to avoid invoking the shell’s `printf` built-in.
* The strings passed to the `gettext` et al. program inside a subshell
  must not start nor end with a newline.
  This means, in order to add a newline after a message, you either need
  to append a newline to the format string argument of `printf`, or
  to add an explicit invocation of `echo`.
* Some amount of backslashing is involved, see [Preparing Shell Scripts for the `printf` approach](#Preparing-for-printf).
  It is easy to make editing mistakes during this process.
* Speed:
  Each invocation of `env printf` uses a subshell.
  This is slower than the `printf_gettext` approach.

---

Next: [Preparing Shell Scripts for the `gettext.sh` approach](#Preparing-for-gettext_002esh), Previous: [The `printf` approach](#The-printf-approach), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.4 The `printf_gettext` approach [¶](#The-printf_005fgettext-approach-1)

This approach uses
the `printf_gettext` and `printf_ngettext` programs,
contained in the run-time package of GNU gettext 0.26 or newer.

Here’s an example that references a shell variable `pid`:

```
printf_gettext 'Running as process number %u.' $pid; echo
```

An example is available in the `hello-3.sh` file
in the `examples/hello-sh` directory.

#### Advantages and Drawbacks [¶](#Advantages-and-Drawbacks-2)

Advantages:

* Portability: The only requirement is GNU gettext 0.26 or newer.
* Speed:
  Since no subshell needs to be created,
  this approach is about 30% faster than the two other approaches.

An example is available in the `hello-3.sh` file
in the `examples/hello-sh` directory.

---

Next: [Preparing Shell Scripts for the `printf` approach](#Preparing-for-printf), Previous: [The `printf_gettext` approach](#The-printf_005fgettext-approach), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.5 Preparing Shell Scripts for the `gettext.sh` approach [¶](#Preparing-Shell-Scripts-for-the-gettext_002esh-approach)

Preparing a shell script for internationalization is conceptually similar
to the steps described in [Preparing Program Sources](#Sources).
The concrete steps for shell scripts with this approach are as follows.

1. Insert the line

   ```
   . gettext.sh
   ```

   near the top of the script.
   `gettext.sh` is a shell function library that provides the functions
   `eval_gettext` (see [Invoking the `eval_gettext` function](#eval_005fgettext-Invocation)),
   `eval_ngettext` (see [Invoking the `eval_ngettext` function](#eval_005fngettext-Invocation)),
   `eval_pgettext` (see [Invoking the `eval_pgettext` function](#eval_005fpgettext-Invocation)), and
   `eval_npgettext` (see [Invoking the `eval_npgettext` function](#eval_005fnpgettext-Invocation)).
   You have to ensure that `gettext.sh` can be found in the `PATH`.
2. Set and export the `TEXTDOMAIN` and `TEXTDOMAINDIR` environment
   variables.
   Usually `TEXTDOMAIN` is the package or program name, and
   `TEXTDOMAINDIR` is the absolute pathname corresponding to
   `$prefix/share/locale`, where `$prefix` is the installation location.

   ```
   TEXTDOMAIN=@PACKAGE@
   export TEXTDOMAIN
   TEXTDOMAINDIR=@LOCALEDIR@
   export TEXTDOMAINDIR
   ```
3. Prepare the strings for translation, as described in [Preparing Translatable Strings](#Preparing-Strings).
4. Simplify translatable strings so that they don’t contain command substitution
   (`` "`...`" `` or `"$(...)"`), variable access with defaulting (like
   `${variable-default}`), access to positional arguments
   (like `$0`, `$1`, ...) or highly volatile shell variables (like
   `$?`).
   This can always be done through simple local code restructuring.
   For example,

   ```
   echo "Usage: $0 [OPTION] FILE..."
   ```

   becomes

   ```
   program_name=$0
   echo "Usage: $program_name [OPTION] FILE..."
   ```

   Similarly,

   ```
   echo "Remaining files: `ls | wc -l`"
   ```

   becomes

   ```
   filecount="`ls | wc -l`"
   echo "Remaining files: $filecount"
   ```
5. For each translatable string, change the output command ‘`echo`’ or
   ‘`$echo`’ to ‘`gettext`’ (if the string contains no references to
   shell variables) or to ‘`eval_gettext`’ (if it refers to shell variables),
   followed by a no-argument ‘`echo`’ command (to account for the terminating
   newline).
   Similarly, for cases with plural handling, replace a conditional
   ‘`echo`’ command with an invocation of ‘`ngettext`’ or
   ‘`eval_ngettext`’, followed by a no-argument ‘`echo`’ command.

   When doing this, you also need to add an extra backslash before the dollar
   sign in references to shell variables, so that the ‘`eval_gettext`’
   function receives the translatable string before the variable values are
   substituted into it.
   For example,

   ```
   echo "Remaining files: $filecount"
   ```

   becomes

   ```
   eval_gettext "Remaining files: \$filecount"; echo
   ```

   If the output command is not ‘`echo`’, you can make it use ‘`echo`’
   nevertheless, through the use of backquotes.
   However, note that inside backquotes,
   backslashes must be doubled to be effective
   (because the backquoting eats one level of backslashes).
   For example, assuming that
   ‘`error`’ is a shell function that signals an error,

   ```
   error "file not found: $filename"
   ```

   is first transformed into

   ```
   error "`echo \"file not found: \$filename\"`"
   ```

   which then becomes

   ```
   error "`eval_gettext \"file not found: \\\$filename\"`"
   ```

---

Next: [Preparing Shell Scripts for the `printf_gettext` approach](#Preparing-for-printf_005fgettext), Previous: [Preparing Shell Scripts for the `gettext.sh` approach](#Preparing-for-gettext_002esh), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.6 Preparing Shell Scripts for the `printf` approach [¶](#Preparing-Shell-Scripts-for-the-printf-approach)

Preparing a shell script for internationalization is conceptually similar
to the steps described in [Preparing Program Sources](#Sources).
The concrete steps for shell scripts with this approach are as follows.

1. Set and export the `TEXTDOMAIN` and `TEXTDOMAINDIR` environment
   variables.
   Usually `TEXTDOMAIN` is the package or program name, and
   `TEXTDOMAINDIR` is the absolute pathname corresponding to
   `$prefix/share/locale`, where `$prefix` is the installation location.

   ```
   TEXTDOMAIN=@PACKAGE@
   export TEXTDOMAIN
   TEXTDOMAINDIR=@LOCALEDIR@
   export TEXTDOMAINDIR
   ```
2. Prepare the strings for translation, as described in [Preparing Translatable Strings](#Preparing-Strings).
3. Simplify translatable strings so that they don’t contain command substitution
   (`` "`...`" `` or `"$(...)"`), variable access with defaulting (like
   `${variable-default}`), access to positional arguments
   (like `$0`, `$1`, ...) or highly volatile shell variables (like
   `$?`).
   This can always be done through simple local code restructuring.
   For example,

   ```
   echo "Usage: $0 [OPTION] FILE..."
   ```

   becomes

   ```
   program_name=$0
   echo "Usage: $program_name [OPTION] FILE..."
   ```

   Similarly,

   ```
   echo "Remaining files: `ls | wc -l`"
   ```

   becomes

   ```
   filecount="`ls | wc -l`"
   echo "Remaining files: $filecount"
   ```
4. For each translatable string, change the output command ‘`echo`’ or
   ‘`$echo`’ to ‘`gettext`’ (if the string contains no references to
   shell variables)
   or to ‘`` env printf "`gettext ... `" ``’ (if it refers to shell variables),
   followed by a no-argument ‘`echo`’ command (to account for the terminating
   newline).
   Similarly, for cases with plural handling, replace a conditional
   ‘`echo`’ command with an invocation of ‘`ngettext`’ or
   ‘`` env printf "`ngettext ... `" ``’,
   followed by a no-argument ‘`echo`’ command.

   When doing this, you also need to replace the references to shell variables
   with format string directives (see [Shell Format Strings](#sh_002dformat)),
   so that the ‘`eval_gettext`’ function receives the translatable string
   before the variable values are substituted into it.
   For example,

   ```
   echo "Remaining files: $filecount"
   ```

   becomes

   ```
   env printf "`gettext \"Remaining files: %u.\"`"'\n' "$filecount"
   ```

   If the output command is not ‘`echo`’, you can make it use ‘`echo`’
   nevertheless, through the use of backquotes.
   However, note that inside backquotes,
   backslashes must be doubled to be effective
   (because the backquoting eats one level of backslashes).
   For example, assuming that
   ‘`error`’ is a shell function that signals an error,

   ```
   error "file not found: $filename"
   ```

   is first transformed into

   ```
   error "`echo \"file not found: \$filename\"`"
   ```

   which then becomes

   ```
   error "`env printf \"\`gettext \\\"file not found: %s\\\"\`\" \"\$filename\"`"
   ```

---

Next: [Invoking the `gettext` program](#gettext-Invocation), Previous: [Preparing Shell Scripts for the `printf` approach](#Preparing-for-printf), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.7 Preparing Shell Scripts for the `printf_gettext` approach [¶](#Preparing-Shell-Scripts-for-the-printf_005fgettext-approach)

Preparing a shell script for internationalization is conceptually similar
to the steps described in [Preparing Program Sources](#Sources).
The concrete steps for shell scripts with this approach are as follows.

1. Set and export the `TEXTDOMAIN` and `TEXTDOMAINDIR` environment
   variables.
   Usually `TEXTDOMAIN` is the package or program name, and
   `TEXTDOMAINDIR` is the absolute pathname corresponding to
   `$prefix/share/locale`, where `$prefix` is the installation location.

   ```
   TEXTDOMAIN=@PACKAGE@
   export TEXTDOMAIN
   TEXTDOMAINDIR=@LOCALEDIR@
   export TEXTDOMAINDIR
   ```
2. Prepare the strings for translation, as described in [Preparing Translatable Strings](#Preparing-Strings).
3. Simplify translatable strings so that they don’t contain command substitution
   (`` "`...`" `` or `"$(...)"`), variable access with defaulting (like
   `${variable-default}`), access to positional arguments
   (like `$0`, `$1`, ...) or highly volatile shell variables (like
   `$?`).
   This can always be done through simple local code restructuring.
   For example,

   ```
   echo "Usage: $0 [OPTION] FILE..."
   ```

   becomes

   ```
   program_name=$0
   echo "Usage: $program_name [OPTION] FILE..."
   ```

   Similarly,

   ```
   echo "Remaining files: `ls | wc -l`"
   ```

   becomes

   ```
   filecount="`ls | wc -l`"
   echo "Remaining files: $filecount"
   ```
4. For each translatable string, change the output command ‘`echo`’ or
   ‘`$echo`’ to ‘`gettext`’ (if the string contains no references to
   shell variables)
   or to ‘`printf_gettext`’ (if it refers to shell variables),
   followed by a no-argument ‘`echo`’ command (to account for the terminating
   newline).
   Similarly, for cases with plural handling, replace a conditional
   ‘`echo`’ command with an invocation of ‘`ngettext`’ or
   ‘`printf_ngettext`’,
   followed by a no-argument ‘`echo`’ command.

   When doing this, you also need to replace the references to shell variables
   with format string directives (see [Shell Format Strings](#sh_002dformat)),
   so that the ‘`eval_gettext`’ function receives the translatable string
   before the variable values are substituted into it.
   For example,

   ```
   echo "Remaining files: $filecount"
   ```

   becomes

   ```
   printf_gettext "Remaining files: %u." "$filecount"; echo
   ```

   If the output command is not ‘`echo`’, you can make it use ‘`echo`’
   nevertheless, through the use of backquotes.
   However, note that inside backquotes,
   backslashes must be doubled to be effective
   (because the backquoting eats one level of backslashes).
   For example, assuming that
   ‘`error`’ is a shell function that signals an error,

   ```
   error "file not found: $filename"
   ```

   is first transformed into

   ```
   error "`echo \"file not found: \$filename\"`"
   ```

   which then becomes

   ```
   error "`printf_gettext \"file not found: %s\" \"\$filename\"`"
   ```

---

Next: [Invoking the `ngettext` program](#ngettext-Invocation), Previous: [Preparing Shell Scripts for the `printf_gettext` approach](#Preparing-for-printf_005fgettext), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.8 Invoking the `gettext` program [¶](#Invoking-the-gettext-program)

```
gettext [option] [[textdomain] msgid]
gettext [option] -s [msgid]...
```

The `gettext` program displays the native language translation of a
textual message.

**Options and arguments**

‘`-c context`’ [¶](#index-_002dc_002c-gettext-option)

‘`--context=context`’
:   Specify the context for the messages to be translated.
    See [Using contexts for solving ambiguities](#Contexts) for details.

‘`-d textdomain`’ [¶](#index-_002dd_002c-gettext-option)

‘`--domain=textdomain`’
:   Retrieve translated messages from textdomain. Usually a textdomain
    corresponds to a package, a program, or a module of a program.

‘`-e`’ [¶](#index-_002de_002c-gettext-option)
:   Enable expansion of some escape sequences. This option is for compatibility
    with the ‘`echo`’ program or shell built-in. The escape sequences
    ‘`\a`’, ‘`\b`’, ‘`\c`’, ‘`\f`’, ‘`\n`’, ‘`\r`’, ‘`\t`’,
    ‘`\v`’, ‘`\\`’, and ‘`\`’ followed by one to three octal digits, are
    interpreted like the System V ‘`echo`’ program did.

‘`-E`’ [¶](#index-_002dE_002c-gettext-option)
:   This option is only for compatibility with the ‘`echo`’ program or shell
    built-in. It has no effect.

‘`-n`’ [¶](#index-_002dn_002c-gettext-option)
:   This option has only an effect if the `-s` option is given. It
    suppresses the additional newline at the end.

‘`[textdomain] msgid`’
:   Retrieve translated message corresponding to msgid from textdomain.

**Informative output**

‘`-h`’ [¶](#index-_002dh_002c-gettext-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-gettext-option)

‘`--version`’
:   Output version information and exit.

If the textdomain parameter is not given, the domain is determined from
the environment variable `TEXTDOMAIN`. If the message catalog is not
found in the regular directory, another location can be specified with the
environment variable `TEXTDOMAINDIR`.

When used with the `-s` option the program behaves like the ‘`echo`’
command. But it does not simply copy its arguments to stdout. Instead those
messages found in the selected catalog are translated. Also, a newline is
added at the end, unless either the option `-n` is specified or the
option `-e` is specified and some of the argument strings contains a
‘`\c`’ escape sequence.

Note: `xgettext` supports only the one-argument form of the
`gettext` invocation, where no options are present and the
textdomain is implicit, from the environment.

---

Next: [Invoking the `printf_gettext` program](#printf_005fgettext-Invocation), Previous: [Invoking the `gettext` program](#gettext-Invocation), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.9 Invoking the `ngettext` program [¶](#Invoking-the-ngettext-program)

```
ngettext [option] [textdomain] msgid msgid-plural count
```

The `ngettext` program displays the native language translation of a
textual message whose grammatical form depends on a number.

**Options and arguments**

‘`-c context`’ [¶](#index-_002dc_002c-ngettext-option)

‘`--context=context`’
:   Specify the context for the messages to be translated.
    See [Using contexts for solving ambiguities](#Contexts) for details.

‘`-d textdomain`’ [¶](#index-_002dd_002c-ngettext-option)

‘`--domain=textdomain`’
:   Retrieve translated messages from textdomain. Usually a textdomain
    corresponds to a package, a program, or a module of a program.

‘`-e`’ [¶](#index-_002de_002c-ngettext-option)
:   Enable expansion of some escape sequences. This option is for compatibility
    with the ‘`gettext`’ program. The escape sequences
    ‘`\a`’, ‘`\b`’, ‘`\f`’, ‘`\n`’, ‘`\r`’, ‘`\t`’,
    ‘`\v`’, ‘`\\`’, and ‘`\`’ followed by one to three octal digits, are
    interpreted like the System V ‘`echo`’ program did.

‘`-E`’ [¶](#index-_002dE_002c-ngettext-option)
:   This option is only for compatibility with the ‘`gettext`’ program. It has
    no effect.

‘`textdomain`’
:   Retrieve translated message from textdomain.

‘`msgid msgid-plural`’
:   Translate msgid (English singular) / msgid-plural (English plural).

‘`count`’
:   Choose singular/plural form based on this value.

**Informative output**

‘`-h`’ [¶](#index-_002dh_002c-ngettext-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-ngettext-option)

‘`--version`’
:   Output version information and exit.

If the textdomain parameter is not given, the domain is determined from
the environment variable `TEXTDOMAIN`. If the message catalog is not
found in the regular directory, another location can be specified with the
environment variable `TEXTDOMAINDIR`.

Note: `xgettext` supports only the three-arguments form of the
`ngettext` invocation, where no options are present and the
textdomain is implicit, from the environment.

---

Next: [Invoking the `printf_ngettext` program](#printf_005fngettext-Invocation), Previous: [Invoking the `ngettext` program](#ngettext-Invocation), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.10 Invoking the `printf_gettext` program [¶](#Invoking-the-printf_005fgettext-program)

```
printf_gettext [option] format [argument]...
```

The `printf_gettext` program produces formatted output,
applying the native language translation of format
to the arguments.

**Options and arguments**

‘`-c context`’ [¶](#index-_002dc_002c-printf_005fgettext-option)

‘`--context=context`’
:   Specify the context for the format string to be translated.
    See [Using contexts for solving ambiguities](#Contexts) for details.

‘`format`’
:   The format string.

‘`argument`’
:   A string or numeric argument.

**Informative output**

‘`-h`’ [¶](#index-_002dh_002c-printf_005fgettext-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-printf_005fgettext-option)

‘`--version`’
:   Output version information and exit.

The format string consists of

* plain text,
* directives, that start with ‘`%`’,
* escape sequences, that start with a backslash.

A directive that consumes an argument

* starts with ‘`%`’ or ‘`%m$`’ where m is a positive integer,
* is optionally followed by any of the characters
  ‘`#`’, ‘`0`’, ‘`-`’, ‘’, ‘`+`’,
  each of which acts as a flag,
* is optionally followed by a width specification (a nonnegative integer),
* is optionally followed by ‘`.`’ and a precision specification
  (an optional nonnegative integer),
* is finished by a specifier
  + ‘`c`’, that prints a character,
  + ‘`s`’, that prints a string,
  + ‘`i`’, ‘`d`’, that print an integer,
  + ‘`u`’, ‘`o`’, ‘`x`’, ‘`X`’,
    that print an unsigned (nonnegative) integer,
  + ‘`e`’, ‘`E`’, that print a floating-point number in scientific notation,
  + ‘`f`’, ‘`F`’, that print a floating-point number without an exponent,
  + ‘`g`’, ‘`G`’, that print a floating-point number in general notation,
  + ‘`a`’, ‘`A`’, that print a floating-point number in hexadecimal notation.

Some flag+specifier combinations are invalid:

* The ‘`#`’ flag with the specifiers
  ‘`c`’, ‘`s`’, ‘`i`’, ‘`d`’, ‘`u`’.
* The ‘`0`’ flag with the specifiers
  ‘`c`’, ‘`s`’.

Additionally there is the directive ‘`%%`’, that prints a single `%`.

If a directive specifies the argument by its number (‘`%m$`’ notation),
all directives that consume an argument must do so.

The escape sequences are:

`\\`
:   backslash

`\a`
:   alert (BEL)

`\b`
:   backspace (BS)

`\f`
:   form feed (FF)

`\n`
:   new line (LF)

`\r`
:   carriage return (CR)

`\t`
:   horizontal tab (HT)

`\v`
:   vertical tab (VT)

`\nnn`
:   octal number with 1 to 3 octal digits

**Environment Variables**

The translation of the format string is looked up in the translation domain
given by the environment variable `TEXTDOMAIN`.

It is looked up in the catalogs directory given by the environment variable
`TEXTDOMAINDIR` or, if not present, in the default catalogs directory.

---

Next: [Invoking the `envsubst` program](#envsubst-Invocation), Previous: [Invoking the `printf_gettext` program](#printf_005fgettext-Invocation), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.11 Invoking the `printf_ngettext` program [¶](#Invoking-the-printf_005fngettext-program)

```
printf_ngettext [option] format format-plural count [argument]...
```

The `printf_ngettext` program produces formatted output,
applying the native language translation of
format and format-plural, depending on count,
to the arguments.

**Options and arguments**

‘`-c context`’ [¶](#index-_002dc_002c-printf_005fngettext-option)

‘`--context=context`’
:   Specify the context for the format string to be translated.
    See [Using contexts for solving ambiguities](#Contexts) for details.

‘`format`’
:   English singular form of format string.

‘`format-plural`’
:   English plural form of format string.

‘`count`’
:   A cardinal number.
    The singular/plural form is chosen based on this value.

‘`argument`’
:   A string or numeric argument.

**Informative output**

‘`-h`’ [¶](#index-_002dh_002c-printf_005fngettext-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-printf_005fngettext-option)

‘`--version`’
:   Output version information and exit.

Each format string consists of

* plain text,
* directives, that start with ‘`%`’,
* escape sequences, that start with a backslash.

A directive that consumes an argument

* starts with ‘`%`’ or ‘`%m$`’ where m is a positive integer,
* is optionally followed by any of the characters
  ‘`#`’, ‘`0`’, ‘`-`’, ‘’, ‘`+`’,
  each of which acts as a flag,
* is optionally followed by a width specification (a nonnegative integer),
* is optionally followed by ‘`.`’ and a precision specification
  (an optional nonnegative integer),
* is finished by a specifier
  + ‘`c`’, that prints a character,
  + ‘`s`’, that prints a string,
  + ‘`i`’, ‘`d`’, that print an integer,
  + ‘`u`’, ‘`o`’, ‘`x`’, ‘`X`’,
    that print an unsigned (nonnegative) integer,
  + ‘`e`’, ‘`E`’, that print a floating-point number in scientific notation,
  + ‘`f`’, ‘`F`’, that print a floating-point number without an exponent,
  + ‘`g`’, ‘`G`’, that print a floating-point number in general notation,
  + ‘`a`’, ‘`A`’, that print a floating-point number in hexadecimal notation.

Some flag+specifier combinations are invalid:

* The ‘`#`’ flag with the specifiers
  ‘`c`’, ‘`s`’, ‘`i`’, ‘`d`’, ‘`u`’.
* The ‘`0`’ flag with the specifiers
  ‘`c`’, ‘`s`’.

Additionally there is the directive ‘`%%`’, that prints a single `%`.

If a directive specifies the argument by its number (‘`%m$`’ notation),
all directives that consume an argument must do so.

The escape sequences are:

`\\`
:   backslash

`\a`
:   alert (BEL)

`\b`
:   backspace (BS)

`\f`
:   form feed (FF)

`\n`
:   new line (LF)

`\r`
:   carriage return (CR)

`\t`
:   horizontal tab (HT)

`\v`
:   vertical tab (VT)

`\nnn`
:   octal number with 1 to 3 octal digits

**Environment Variables**

The translation of the format string is looked up in the translation domain
given by the environment variable `TEXTDOMAIN`.

It is looked up in the catalogs directory given by the environment variable
`TEXTDOMAINDIR` or, if not present, in the default catalogs directory.

---

Next: [Invoking the `eval_gettext` function](#eval_005fgettext-Invocation), Previous: [Invoking the `printf_ngettext` program](#printf_005fngettext-Invocation), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.12 Invoking the `envsubst` program [¶](#Invoking-the-envsubst-program)

```
envsubst [option] [shell-format]
```

The `envsubst` program substitutes the values of environment variables.

**Operation mode**

‘`-v`’ [¶](#index-_002dv_002c-envsubst-option)

‘`--variables`’
:   Output the variables occurring in shell-format.

**Informative output**

‘`-h`’ [¶](#index-_002dh_002c-envsubst-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-envsubst-option)

‘`--version`’
:   Output version information and exit.

In normal operation mode, standard input is copied to standard output,
with references to environment variables of the form `$VARIABLE` or
`${VARIABLE}` being replaced with the corresponding values. If a
shell-format is given, only those environment variables that are
referenced in shell-format are substituted; otherwise all environment
variables references occurring in standard input are substituted.

These substitutions are a subset of the substitutions that a shell performs
on unquoted and double-quoted strings. Other kinds of substitutions done
by a shell, such as `${variable-default}` or
`$(command-list)` or `` `command-list` ``, are not performed
by the `envsubst` program, due to security reasons.

When `--variables` is used, standard input is ignored, and the output
consists of the environment variables that are referenced in
shell-format, one per line.

---

Next: [Invoking the `eval_ngettext` function](#eval_005fngettext-Invocation), Previous: [Invoking the `envsubst` program](#envsubst-Invocation), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.13 Invoking the `eval_gettext` function [¶](#Invoking-the-eval_005fgettext-function)

```
eval_gettext msgid
```

This function outputs the native language translation of a textual message,
performing dollar-substitution on the result. Note that only shell variables
mentioned in msgid will be dollar-substituted in the result.

---

Next: [Invoking the `eval_pgettext` function](#eval_005fpgettext-Invocation), Previous: [Invoking the `eval_gettext` function](#eval_005fgettext-Invocation), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.14 Invoking the `eval_ngettext` function [¶](#Invoking-the-eval_005fngettext-function)

```
eval_ngettext msgid msgid-plural count
```

This function outputs the native language translation of a textual message
whose grammatical form depends on a number, performing dollar-substitution
on the result. Note that only shell variables mentioned in msgid or
msgid-plural will be dollar-substituted in the result.

---

Next: [Invoking the `eval_npgettext` function](#eval_005fnpgettext-Invocation), Previous: [Invoking the `eval_ngettext` function](#eval_005fngettext-Invocation), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.15 Invoking the `eval_pgettext` function [¶](#Invoking-the-eval_005fpgettext-function)

```
eval_pgettext msgctxt msgid
```

This function outputs the native language translation of a textual message
in the given context msgctxt (see [Using contexts for solving ambiguities](#Contexts)), performing
dollar-substitution on the result. Note that only shell variables mentioned
in msgid will be dollar-substituted in the result.

---

Previous: [Invoking the `eval_pgettext` function](#eval_005fpgettext-Invocation), Up: [sh - Shell Script](#sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.15.16 Invoking the `eval_npgettext` function [¶](#Invoking-the-eval_005fnpgettext-function)

```
eval_npgettext msgctxt msgid msgid-plural count
```

This function outputs the native language translation of a textual message
whose grammatical form depends on a number in the given context msgctxt
(see [Using contexts for solving ambiguities](#Contexts)), performing dollar-substitution on the result. Note
that only shell variables mentioned in msgid or msgid-plural
will be dollar-substituted in the result.

---

Next: [GNU awk](#gawk), Previous: [sh - Shell Script](#sh), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.16 bash - Bourne-Again Shell Script [¶](#bash-_002d-Bourne_002dAgain-Shell-Script)

GNU `bash` 2.0 or newer has a special shorthand for translating a
string and substituting variable values in it: `$"msgid"`. But
the use of this construct is **discouraged**, due to the security
holes it opens and due to its portability problems.

The security holes of `$"..."` come from the fact that after looking up
the translation of the string, `bash` processes it like it processes
any double-quoted string: dollar and backquote processing, like ‘`eval`’
does.

1. In a locale whose encoding is one of BIG5, BIG5-HKSCS, GBK, GB18030, SHIFT\_JIS,
   JOHAB, some double-byte characters have a second byte whose value is
   `0x60`. For example, the byte sequence `\xe0\x60` is a single
   character in these locales. Many versions of `bash` (all versions
   up to bash-2.05, and newer versions on platforms without `mbsrtowcs()`
   function) don’t know about character boundaries and see a backquote character
   where there is only a particular Chinese character. Thus it can start
   executing part of the translation as a command list. This situation can occur
   even without the translator being aware of it: if the translator provides
   translations in the UTF-8 encoding, it is the `gettext()` function which
   will, during its conversion from the translator’s encoding to the user’s
   locale’s encoding, produce the dangerous `\x60` bytes.
2. A translator could - voluntarily or inadvertently - use backquotes
   `` "`...`" `` or dollar-parentheses `"$(...)"` in her translations.
   The enclosed strings would be executed as command lists by the shell.

The portability problem is that `bash` must be built with
internationalization support; this is normally not the case on systems
that don’t have the `gettext()` function in libc.

---

Next: [Lua](#Lua), Previous: [bash - Bourne-Again Shell Script](#bash), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.17 GNU awk [¶](#GNU-awk)

RPMs
:   gawk 3.1 or newer

Ubuntu packages
:   gawk

File extension
:   `awk`, `gawk`, `twjr`.
    The file extension `twjr` is used by TexiWeb Jr
    (<https://github.com/arnoldrobbins/texiwebjr>).

String syntax
:   `"abc"`

gettext shorthand
:   `_"abc"`

gettext/ngettext functions
:   `dcgettext`, missing `dcngettext` in gawk-3.1.0

textdomain
:   `TEXTDOMAIN` variable

bindtextdomain
:   `bindtextdomain` function

setlocale
:   automatic, but missing `setlocale (LC_MESSAGES, "")` in gawk-3.1.0

Prerequisite
:   —

Use or emulate GNU gettext
:   use

Extractor
:   `xgettext`

Formatting with positions
:   `printf "%2$d %1$d"` (GNU awk only)

Portability
:   On platforms without gettext, no translation. On non-GNU awks, you must
    define `dcgettext`, `dcngettext` and `bindtextdomain`
    yourself.

po-mode marking
:   —

An example is available in the `examples` directory: `hello-gawk`.

---

Next: [Pascal - Free Pascal Compiler](#Pascal), Previous: [GNU awk](#gawk), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.18 Lua [¶](#Lua-1)

RPMs
:   lua

Ubuntu packages
:   lua, lua-gettext
      
    You need to install the `lua-gettext` package from
    <https://gitlab.com/sukhichev/lua-gettext/blob/master/README.us.md>.
    Debian and Ubuntu packages of it are available. Download the
    appropriate one, and install it through
    ‘`sudo dpkg -i lua-gettext_0.0+nmu1_amd64.deb`’.

File extension
:   `lua`

String syntax
:   * `"abc"`
    * `'abc'`
    * `[[abc]]`
    * `[=[abc]=]`
    * `[==[abc]==]`
    * ...

gettext shorthand
:   `_("abc")`

gettext/ngettext functions
:   `gettext.gettext`, `gettext.dgettext`, `gettext.dcgettext`,
    `gettext.ngettext`, `gettext.dngettext`, `gettext.dcngettext`

textdomain
:   `textdomain` function

bindtextdomain
:   `bindtextdomain` function

setlocale
:   automatic

Prerequisite
:   `require 'gettext'` or running lua interpreter with `-l gettext` option

Use or emulate GNU gettext
:   use

Extractor
:   `xgettext`

Formatting with positions
:   —

Portability
:   On platforms without gettext, the functions are not available.

po-mode marking
:   —

---

Next: [Modula-2](#Modula_002d2), Previous: [Lua](#Lua), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.19 Pascal - Free Pascal Compiler [¶](#Pascal-_002d-Free-Pascal-Compiler)

RPMs
:   fpc

Ubuntu packages
:   fp-compiler, fp-units-fcl

File extension
:   `pp`, `pas`

String syntax
:   `'abc'`

gettext shorthand
:   automatic

gettext/ngettext functions
:   —, use `ResourceString` data type instead

textdomain
:   —, use `TranslateResourceStrings` function instead

bindtextdomain
:   —, use `TranslateResourceStrings` function instead

setlocale
:   automatic, but uses only LANG, not LC\_MESSAGES or LC\_ALL

Prerequisite
:   `{$mode delphi}` or `{$mode objfpc}`  
    `uses gettext;`

Use or emulate GNU gettext
:   emulate partially

Extractor
:   `ppc386` followed by `xgettext` or `rstconv`

Formatting with positions
:   `uses sysutils;`  
    `format "%1:d %0:d"`

Portability
:   ?

po-mode marking
:   —

The Pascal compiler has special support for the `ResourceString` data
type. It generates a `.rst` file. This is then converted to a
`.pot` file by use of `xgettext` or `rstconv`. At runtime,
a `.mo` file corresponding to translations of this `.pot` file
can be loaded using the `TranslateResourceStrings` function in the
`gettext` unit.

An example is available in the `examples` directory: `hello-pascal`.

---

Next: [D](#D), Previous: [Pascal - Free Pascal Compiler](#Pascal), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.20 Modula-2 [¶](#Modula_002d2-1)

RPMs
:   gcc-gm2, libgm2

Ubuntu packages
:   gm2

File extension
:   `mod`, `def`

String syntax
:   `'abc'`, `"abc"`

gettext shorthand
:   —

gettext/ngettext functions
:   `Gettext`, `DGettext`, `DCGettext`,
    `NGettext`, `DNGettext`, `DCNGettext`

textdomain
:   `TextDomain` function

bindtextdomain
:   `BindTextDomain` function

setlocale
:   Programmer must call `SetLocale (LC_ALL, "")`

Prerequisite
:   `FROM Libintl IMPORT Gettext ...;`

Use or emulate GNU gettext
:   Use

Extractor
:   `xgettext`

Formatting with positions
:   —

Portability
:   fully portable to all platforms supported by GNU Modula-2

po-mode marking
:   —

An example is available in the `examples` directory: `hello-modula2`.

---

Next: [OCaml](#OCaml), Previous: [Modula-2](#Modula_002d2), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.21 D [¶](#D-1)

RPMs
:   gcc-gdc or ldc

Ubuntu packages
:   gdc or ldc

File extension
:   `d`

String syntax
:   `r"abc"`, `` `abc` ``, `"abc"`,
    `q"[abc]"`, `q"(abc)"`, `q"<abc>"`, `q"{abc}"`,
    `q{abc}`, `x"6A 6B 6C"`

gettext shorthand
:   `_("abc")`

gettext/ngettext functions
:   `gettext`, `dgettext`, `dcgettext`,
    `ngettext`, `dngettext`, `dcngettext`

    Note that the `ngettext`-like functions need to take
    two argument strings that consume the same number of arguments.
    For example, you cannot write
    `format(ngettext("a piece", "%d pieces", n), n)`
    because in the singular case,
    `format` would treat the unused argument as an error and
    throw an exception.
    As a workaround, you need to convert `n` to a string and
    format that string with precision zero:
    `format(ngettext("%.0sa piece", "%s pieces", n), to!string(n))`
    or
    `format(ngettext("%.0sa piece", "%s pieces", n), text(n))`

textdomain
:   `textdomain` function

bindtextdomain
:   `bindtextdomain` function

setlocale
:   Programmer must call `setlocale (LC_ALL, "")`

Prerequisite
:   `import gnu.libintl;`
      
    `alias _ = gettext;`

Use or emulate GNU gettext
:   Use

Extractor
:   `xgettext -k_ --flag=_:1:pass-c-format --flag=_:1:pass-d-format`

Formatting with positions
:   `fprintf "%2$d %1$d"`, `format "%2$d %1$d"`

Portability
:   fully portable

po-mode marking
:   —

An example is available in the `examples` directory: `hello-d`.

---

Next: [GNU Smalltalk](#Smalltalk), Previous: [D](#D), Up: [Individual Programming Languages](#List-of-Programming-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 16.5.22 OCaml [¶](#OCaml-1)

RPMs
:   ocaml, ocaml-gettext-devel

Ubuntu packages
:   ocaml, opam
      
    `opam install gettext-stub`
