### 1.4 Contributing to Gnulib [¶](#Contributing-to-Gnulib-1)

All software here is copyrighted by the Free Software Foundation—you need
to have filled out an assignment form for a project that uses the
module for that contribution to be accepted here.

If you have a piece of code that you would like to contribute, please
email [bug-gnulib@gnu.org](mailto:bug-gnulib@gnu.org).

Generally we are looking for files that fulfill at least one of the
following requirements:

* If your `.c` and `.h` files define functions that are broken or
  missing on some other system, we should be able to include it.
* If your functions remove arbitrary limits from existing
  functions (either under the same name, or as a slightly different
  name), we should be able to include it.

If your functions define completely new but rarely used functionality,
you should probably consider packaging it as a separate library.

* [Gnulib licensing](#Gnulib-licensing)
* [Indent with spaces not TABs](#Indent-with-spaces-not-TABs)
* [How to add a new module](#How-to-add-a-new-module)

---

Next: [Indent with spaces not TABs](#Indent-with-spaces-not-TABs), Up: [Contributing to Gnulib](#Contributing-to-Gnulib)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 1.4.1 Gnulib licensing [¶](#Gnulib-licensing-1)

Gnulib contains code both under GPL and LGPL. Because several packages
that use Gnulib are GPL, the files state they are licensed under GPL.
However, to support LGPL projects as well, you may use some of the
files under LGPL. The “License:” information in the files under
modules/ clarifies the real license that applies to the module source.

Keep in mind that if you submit patches to files in Gnulib, you should
license them under a compatible license, which means that sometimes
the contribution will have to be LGPL, if the original file is
available under LGPL via a “License: LGPL” information in the
projects’ modules/ file.

---

Next: [How to add a new module](#How-to-add-a-new-module), Previous: [Gnulib licensing](#Gnulib-licensing), Up: [Contributing to Gnulib](#Contributing-to-Gnulib)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 1.4.2 Indent with spaces not TABs [¶](#Indent-with-spaces-not-TABs-1)

We use space-only indentation in nearly all files. This includes all
`*.h`, `*.c`, `*.y` files, except for the `regex`
module. Makefile and ChangeLog files are excluded, since TAB
characters are part of their format.

In order to tell your editor to produce space-only indentation, you
can use these instructions.

* For Emacs: Add these lines to your Emacs initialization file
  (`$HOME/.emacs` or similar):

  ```
  ;; In Gnulib, indent with spaces everywhere (not TABs).
  ;; Exceptions: Makefile and ChangeLog modes.
  (add-hook 'find-file-hook '(lambda ()
    (if (and buffer-file-name
             (string-match "/gnulib\\>" (buffer-file-name))
             (not (string-equal mode-name "Change Log"))
             (not (string-equal mode-name "Makefile")))
        (setq indent-tabs-mode nil))))
  ```
* For vi (vim): Add these lines to your `$HOME/.vimrc` file:

  ```
  " Don't use tabs for indentation. Spaces are nicer to work with.
  set expandtab
  ```

  For Makefile and ChangeLog files, compensate for this by adding this
  to your `$HOME/.vim/after/indent/make.vim` file, and similarly
  for your `$HOME/.vim/after/indent/changelog.vim` file:

  ```
  " Use tabs for indentation, regardless of the global setting.
  set noexpandtab
  ```
* For Eclipse: In the “Window|Preferences” dialog (or “Eclipse|Preferences”
  dialog on Mac OS),
  1. Under “General|Editors|Text Editors”, select the “Insert spaces for tabs”
     checkbox.
  2. Under “C/C++|Code Style”, select a code style profile that has the
     “Indentation|Tab policy” combobox set to “Spaces only”, such as the
     “GNU [built-in]” policy.

  If you use the GNU indent program, pass it the option `--no-tabs`.

---

Previous: [Indent with spaces not TABs](#Indent-with-spaces-not-TABs), Up: [Contributing to Gnulib](#Contributing-to-Gnulib)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 1.4.3 How to add a new module [¶](#How-to-add-a-new-module-1)

* Add the header files and source files to `lib/`.
* If the module needs configure-time checks, write an Autoconf
  macro for it in `m4/module.m4`. See `m4/README` for details.
* Write a module description `modules/module`, based on
  `modules/TEMPLATE`.
* If the module contributes a section to the end-user documentation,
  put this documentation in `doc/module.texi` and add it to the “Files”
  section of `modules/module`. Most modules don’t do this; they have only
  documentation for the programmer (= Gnulib user). Such documentation
  usually goes into the `lib/` source files. It may also go into `doc/`;
  but don’t add it to the module description in this case.
* Add the module to the list in `MODULES.html.sh`.

You can test that a module builds correctly with:

```
$ ./gnulib-tool --create-testdir --dir=/tmp/testdir module1 ... moduleN
$ cd /tmp/testdir
$ ./configure && make
```

Other things:

* Check the license and copyright year of headers.
* Check that the source code follows the GNU coding standards;
  see <https://www.gnu.org/prep/standards>.
* Add source files to `config/srclist*` if they are identical to upstream
  and should be upgraded in Gnulib whenever the upstream source changes.
* Include header files in source files to verify the function prototypes.
* Make sure a replacement function doesn’t cause warnings or clashes on
  systems that have the function.
* Autoconf functions can use ‘`gl_*`’ prefix. The ‘`AC_*`’ prefix is for
  autoconf internal functions.
* Build files only if they are needed on a platform. Look at the
  `alloca` and `fnmatch` modules for how to achieve this. If
  for some reason you cannot do this, and you have a `.c` file that
  leads to an empty `.o` file on some platforms (through some big
  `#if` around all the code), then ensure that the compilation unit
  is not empty after preprocessing. One way to do this is to
  `#include <stddef.h>` or `<stdio.h>` before the big
  `#if`.

---

Next: [High Quality](#High-Quality), Previous: [Contributing to Gnulib](#Contributing-to-Gnulib), Up: [Brief Overview](#Brief-Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 1.5 Portability guidelines [¶](#Portability-guidelines-1)

Gnulib code is intended to be portable to a wide variety of platforms,
not just GNU platforms. Gnulib typically attempts to support a
platform as long as it is still supported by its provider, even if the
platform is not the latest version. See [Target Platforms](#Target-Platforms).

Many Gnulib modules exist so that applications need not worry about
undesirable variability in implementations. For example, an
application that uses the `malloc` module need not worry about
`malloc (0)` returning a null pointer on some Standard C
platforms; and `glob` users need not worry about `glob`
silently omitting symbolic links to nonexistent files on some
platforms that do not conform to POSIX.

Gnulib code is intended to port without problem to new hosts, e.g.,
hosts conforming to recent C and POSIX standards. Hence Gnulib code
should avoid using constructs that these newer standards no longer
require, without first testing for the presence of these constructs.
For example, because C11 made variable length arrays optional, Gnulib
code should avoid them unless it first uses the `vararrays`
module to check whether they are supported.

The following subsections discuss some exceptions and caveats to the
general Gnulib portability guidelines.

* [C language versions](#C-language-versions)
* [C99 features assumed by Gnulib](#C99-features-assumed)
* [C99 features avoided by Gnulib](#C99-features-avoided)
* [Other portability assumptions made by Gnulib](#Other-portability-assumptions)

---

Next: [C99 features assumed by Gnulib](#C99-features-assumed), Up: [Portability guidelines](#Portability-guidelines)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 1.5.1 C language versions [¶](#C-language-versions-1)

Currently Gnulib assumes at least a freestanding C99 compiler,
possibly operating with a C library that predates C99; with time this
assumption will likely be strengthened to later versions of the C
standard. Old platforms currently supported include AIX 7.3
and Solaris 10, though these platforms are rarely tested.
Gnulib itself is so old that it contains many fixes for obsolete
platforms, fixes that may be removed in the future.

Because of the freestanding C99 assumption, Gnulib code can include
`<float.h>`, `<limits.h>`, `<stdarg.h>`,
`<stddef.h>`, and `<stdint.h>`
unconditionally; `<stdbool.h>` is also in the C99 freestanding
list but is obsolescent as of C23. Gnulib code can also assume the existence
of `<ctype.h>`, `<errno.h>`, `<fcntl.h>`,
`<locale.h>`, `<signal.h>`, `<stdio.h>`,
`<stdlib.h>`, `<string.h>`, and `<time.h>`. Similarly,
many modules include `<sys/types.h>` even though it’s not
in the C standard; that’s OK since `<sys/types.h>` has been around
since Seventh Edition Unix (1979).

Even if the include files exist, they may not conform to the C standard.
However, GCC has a `fixincludes` script that attempts to fix most
conformance problems. Gnulib currently assumes include files
largely conform to C99 or better. People still using ancient hosts
should use fixincludes or fix their include files manually.

Even if the include files conform, the library itself may not.
For example, `strtod` and `mktime` have some bugs on some platforms.
You can work around some of these problems by requiring the relevant
modules, e.g., the Gnulib `mktime` module supplies a working and
conforming `mktime`.

---

Next: [C99 features avoided by Gnulib](#C99-features-avoided), Previous: [C language versions](#C-language-versions), Up: [Portability guidelines](#Portability-guidelines)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 1.5.2 C99 features assumed by Gnulib [¶](#C99-features-assumed-by-Gnulib)

Although the C99 standard specifies many features, Gnulib code
is conservative about using them, partly because Gnulib predates
the widespread adoption of C99, and partly because many C99
features are not well-supported in practice. C99 features that
are reasonably portable nowadays include:

* A declaration after a statement, or as the first clause in a
  `for` statement.
* `long long int`.
* `<stdbool.h>`,
  although Gnulib code no longer uses
  it directly, preferring plain `bool` via the
  `bool` module instead.
  See [`stdbool.h`](#stdbool_002eh).
* `<stdint.h>`,
  assuming the `stdint-h` module is used.
  See [`stdint.h`](#stdint_002eh).
* Compound literals and designated initializers.
* Empty arguments in macro calls.
* Variadic macros.  

  Note: The handling of `__VA_ARGS__` in MSVC differs from the one
  in ISO C 99, see <https://stackoverflow.com/questions/5134523/>.
  But usually this matters only for macros that decompose `__VA_ARGS__`.
* `static inline` functions.
* `__func__`, assuming the `func` module is used. See [func](#func).
* The `restrict` qualifier, assuming
  `AC_REQUIRE([AC_C_RESTRICT])` is used.
  This qualifier is sometimes implemented via a macro, so C++ code that
  uses Gnulib should avoid using `restrict` as an identifier.
* Flexible array members (however, see the `flexmember` module).

---

Next: [Other portability assumptions made by Gnulib](#Other-portability-assumptions), Previous: [C99 features assumed by Gnulib](#C99-features-assumed), Up: [Portability guidelines](#Portability-guidelines)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 1.5.3 C99 features avoided by Gnulib [¶](#C99-features-avoided-by-Gnulib)

Gnulib avoids some features even though they are standardized by C99,
as they have portability problems in practice. Here is a partial list
of avoided C99 features. Many other C99 features are portable only if
their corresponding modules are used; Gnulib code that uses such a
feature should require the corresponding module.

* Variable length arrays (VLAs) or variably modified types,
  without checking whether `__STDC_NO_VLA__` is defined.
  See the `vararrays` and `vla` modules.
* Block-scope variable length arrays, without checking whether either
  `GNULIB_NO_VLA` or `__STDC_NO_VLA__` is defined.
  This lets you define `GNULIB_NO_VLA` to pacify GCC when
  using its `-Wvla-larger-than warnings` option,
  and to avoid large stack usage that may have security implications.
  `GNULIB_NO_VLA` does not affect Gnulib’s other uses of VLAs and
  variably modified types, such as array declarations in function
  prototype scope.
* Converting to pointers via integer types other than `intptr_t` or
  `uintptr_t`. Although the C standard says that values of these
  integer types, if they exist, should be convertible to and from
  `intmax_t` and `uintmax_t` without loss of information, on
  CHERI platforms such conversions result in integers that, if converted
  back to a pointer, cannot be dereferenced.
* `extern inline` functions, without checking whether they are
  supported. See [Extern inline functions](#extern-inline).
* Type-generic math functions.
* Universal character names in source code.
* `<iso646.h>`, since GNU programs need not worry about deficient
  source-code encodings.
* Comments beginning with ‘`//`’. This is mostly for style reasons.

---

Previous: [C99 features avoided by Gnulib](#C99-features-avoided), Up: [Portability guidelines](#Portability-guidelines)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 1.5.4 Other portability assumptions made by Gnulib [¶](#Other-portability-assumptions-made-by-Gnulib)

Gnulib code makes the following assumptions
that go beyond what C and POSIX require:

* Standard internal types like `ptrdiff_t` and `size_t` are no
  wider than `long`. The GNU coding standards allow code to make
  this assumption, POSIX requires implementations to support at
  least one programming environment where this is true, and such
  environments are recommended for Gnulib-using applications. When it
  is easy to port to non-POSIX platforms like MinGW where these types
  are wider than `long`, new Gnulib code should do so, e.g., by
  using `ptrdiff_t` instead of `long`. However, it is not
  always that easy, and no effort has been made to check that all Gnulib
  modules work on MinGW-like environments.
* `int` and `unsigned int` are at least 32 bits wide. POSIX
  and the GNU coding standards both require this.
* Signed integer arithmetic is two’s complement.

  Although some Gnulib modules contain explicit support for
  ones’ complement and signed magnitude integer representations,
  which are allowed by C17 and earlier,
  these modules are the exception rather than the rule.
  All practical Gnulib targets use two’s complement, which is required by C23.
* When an out-of-range integer value is copied to a signed integer,
  low-order bits are copied and high-order bits are silently discarded.

  Although the C standard says the that the resulting signed value is
  implementation-defined or an implementation-defined signal is raised,
  all known platforms simply copy the low-order bits,
  just as the C standard requires when copying to an unsigned integer.

  This assumption is merely about copying data, not about arithmetic.
  Previously, Gnulib code sometimes also assumed that signed integer
  arithmetic wraps around, but modern compiler optimizations
  sometimes do not guarantee this, and Gnulib code with this
  assumption is now considered to be questionable.
  See [Integer Properties](#Integer-Properties).
* There are no “holes” in integer values: all the bits of an integer
  contribute to its value in the usual way.
  In particular, an unsigned type and its signed counterpart have the
  same number of bits when you count the latter’s sign bit.
  (As an exception, Gnulib code is portable to CHERI platforms
  even though this assumption is false for CHERI.)
* Objects with all bits zero are treated as zero or as null pointers.
  For example, `memset (A, 0, sizeof A)` initializes an array
  `A` of pointers to null pointers.
* The types `intptr_t` and `uintptr_t` exist, and pointers
  can be converted to and from these types without loss of information.
* Addresses and sizes behave as if objects reside in a flat address space.
  In particular:
  + If two nonoverlapping objects have sizes S and T represented as
    `ptrdiff_t` or `size_t` values, then `S + T`
    cannot overflow.
  + A pointer P points within an object O if and only if
    `(char *) &O <= (char *) P && (char *) P <
    (char *) (&O + 1)`.
  + Arithmetic on a valid pointer is equivalent to the same arithmetic on
    the pointer converted to `uintptr_t`, except that offsets are
    multiplied by the size of the pointed-to objects.
    For example, if `P + I` is a valid expression
    involving a pointer P and an integer I, then
    `(uintptr_t) (P + I) == (uintptr_t) ((uintptr_t)
    P + I * sizeof *P)`.
    Similar arithmetic can be done with `intptr_t`, although more
    care must be taken in case of integer overflow or negative integers.
  + A pointer P has alignment A if and only if
    `(uintptr_t) P % A` is zero, and similarly for `intptr_t`.
    Although C23 says that portable code should instead test whether
    `memalignment (P) % A` is zero, `memalignment` is
    missing in many non-C23 systems, is not specified to work portably on
    pointers to atomic or volatile memory, and is likely a bit slower than
    the cast.
  + If an existing object has size S, and if T is sufficiently
    small (e.g., 8 KiB), then `S + T` cannot overflow.
    Overflow in this case would mean that the rest of your program fits
    into T bytes, which can’t happen in realistic flat-address-space
    hosts.
  + Adding zero to a null pointer does not change the pointer.
    For example, `0 + (char *) NULL == (char *) NULL`.
    Similarly, subtracting zero does not change a null pointer,
    and subtracting two null pointers results in zero.
    C23 was the first C standard to require this behavior; see
    “[Allow zero length operations on null pointers](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3322.pdf)”, WG14 N3322 (2024-08-28);
    which was adopted retroactively for C23.

Some system platforms violate these assumptions and are therefore not
Gnulib porting targets. See [Unsupported Platforms](#Unsupported-Platforms).

---

Next: [Join the GNU Project](#Joining-GNU), Previous: [Portability guidelines](#Portability-guidelines), Up: [Brief Overview](#Brief-Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 1.6 High Quality [¶](#High-Quality-1)

We develop and maintain a testsuite for Gnulib. The goal is to have a
100% firm interface so that maintainers can feel free to update to the
code in git at *any* time and know that their application will not
break. This means that before any change can be committed to the
repository, a test suite program must be produced that exposes the bug
for regression testing.

* [Stable Branches](#Stable-Branches)
* [Writing reliable code](#Writing-reliable-code)

---

Next: [Writing reliable code](#Writing-reliable-code), Up: [High Quality](#High-Quality)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 1.6.1 Stable Branches [¶](#Stable-Branches-1)

In Gnulib, we don’t use topic branches for experimental work.
Therefore, occasionally a broken commit may be pushed in Gnulib.
It does not happen often, but it does happen.

To compensate for this, Gnulib offers “stable branches”. These
are branches of the Gnulib code that are maintained over some
longer period (a year, for example) and include

* bug fixes,
* portability enhancements (to existing as well as to new platforms),
* updates to `config.guess` and `config.sub`.

Not included in the stable branches are:

* new features, such as new modules,
* optimizations,
* refactorings,
* complex or risky changes in general,
* updates to `texinfo.tex`,
* documentation updates.

So far, we have the following stable branches:

`stable-202607`
:   A stable branch that starts at the beginning of July 2026.

`stable-202601`
:   A stable branch that starts at the beginning of January 2026.

`stable-202507`
:   A stable branch that starts at the beginning of July 2025.
    It is no longer updated.

`stable-202501`
:   A stable branch that starts at the beginning of January 2025.
    It is no longer updated.

`stable-202407`
:   A stable branch that starts at the beginning of July 2024.
    It is no longer updated.

`stable-202401`
:   A stable branch that starts at the beginning of January 2024.
    It is no longer updated.

`stable-202307`
:   A stable branch that starts at the beginning of July 2023.
    It is no longer updated.

`stable-202301`
:   A stable branch that starts at the beginning of January 2023.
    It is no longer updated.

`stable-202207`
:   A stable branch that starts at the beginning of July 2022.
    It is no longer updated.

`stable-202201`
:   A stable branch that starts at the beginning of January 2022.
    It is no longer updated.

The two use-cases of stable branches are thus:

* You want to protect yourself from occasional breakage in Gnulib.
* When making a bug-fix release of your code, you can incorporate
  bug fixes in Gnulib, by pulling in the newest commits from the
  same stable branch that you were already using for the previous
  release.

---

Previous: [Stable Branches](#Stable-Branches), Up: [High Quality](#High-Quality)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 1.6.2 Writing reliable code [¶](#Writing-reliable-code-1)

When compiling and testing Gnulib and Gnulib-using programs, certain
compiler options can help improve reliability. First of all, make it
a habit to use ‘`-Wall`’ in all compilation commands. Beyond that,
the `manywarnings` module enables several forms of static checking in
GCC and related compilers (see [manywarnings](#manywarnings)).

For dynamic checking, you can run `configure` with `CFLAGS`
options appropriate for your compiler. For example:

```
./configure \
 CPPFLAGS='-Wall'\
 CFLAGS='-g3 -O2'\
' -D_FORTIFY_SOURCE=3'\
' -D_GLIBCXX_ASSERTIONS'\
' -fsanitize=undefined'\
' -fsanitize-trap=all'
```

Here:

* `-D_FORTIFY_SOURCE=3` and `-D_GLIBCXX_ASSERTIONS`
  enable extra security hardening checks in
  the GNU C library and GNU C++ library, respectively.
* `-fsanitize=undefined` enables GCC’s undefined behavior sanitizer
  (`ubsan`), and
* `-fsanitize-trap=all` causes `ubsan` to
  abort the program (through an “illegal instruction” signal). This
  measure stops exploit attempts and also allows you to debug the issue.

Without the `-fsanitize-trap=all` option,
`-fsanitize=undefined` causes messages to be printed, and
execution continues after an undefined behavior situation.
The message printing causes GCC-like compilers to arrange for the
program to dynamically link to libraries it might not otherwise need.
With GCC, instead of `-fsanitize-trap=all` you can
use the `-static-libubsan` option to arrange for two of the extra
libraries (`libstdc++` and `libubsan`) to be linked
statically rather than dynamically, though this typically bloats the
executable and the remaining extra libraries are still linked
dynamically.

It is also good to occasionally run the programs under `valgrind`
(see [Running self-tests under valgrind](#Running-self_002dtests-under-valgrind)).

GCC’s `-fhardened` option can also be used, but with caution
because it is designed for production more than testing, and therefore
enables `-ftrivial-auto-var-init=zero` which can mask program
errors.

---

Previous: [High Quality](#High-Quality), Up: [Brief Overview](#Brief-Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 1.7 Join the GNU Project [¶](#Join-the-GNU-Project)

GNU Gnulib is part of the GNU Operating System, developed by the GNU Project.

If you are the author of an awesome program and want to join us in
writing Free (libre) Software, please consider making it an official
GNU program and become a GNU Maintainer. Instructions on how to do
this are [here](https://www.gnu.org/help/evaluation).
We are looking forward to hacking with you!

Don’t have a program to contribute? Look at all the
[other ways to help](https://www.gnu.org/help/help.html).

And to learn more about Free (libre) Software in general, please read
and share [this page](https://gnu.org/philosophy/free-sw.html).

---

Next: [Invoking gnulib-tool](#Invoking-gnulib_002dtool), Previous: [Brief Overview](#Brief-Overview), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 2 Philosophy [¶](#Philosophy-1)

Gnulib’s design and development philosophy is organized around steady,
collaborative, and open development of reusable modules that are
suitable for a reasonably wide variety of platforms.

* [Benefits of using Gnulib](#Benefits)
* [Library vs. Reusable Code](#Library-vs-Reusable-Code)
* [Portability and Application Code](#Portability-and-Application-Code)
* [Target Platforms](#Target-Platforms)
* [Modules](#Modules)
* [Various Kinds of Modules](#Various-Kinds-of-Modules)
* [Collaborative Development](#Collaborative-Development)
* [Copyright](#Copyright)
* [Steady Development](#Steady-Development)
* [Openness](#Openness)

---

Next: [Library vs. Reusable Code](#Library-vs-Reusable-Code), Up: [Philosophy](#Philosophy)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 2.1 Benefits of using Gnulib [¶](#Benefits-of-using-Gnulib)

Gnulib is useful to enhance various aspects of a package:

* Portability: With Gnulib, a package maintainer can program against the
  POSIX and GNU libc APIs and nevertheless expect good portability to
  platforms that don’t implement POSIX.
* Maintainability: When a package uses modules from Gnulib instead of code
  written specifically for that package, the maintainer has less code to
  maintain.
* Security: Gnulib provides functions that are immune against vulnerabilities
  that plague the uses of the corresponding commonplace functions. For
  example, `asprintf`, `canonicalize_file_name` are not affected
  by buffer sizing problems that affect `sprintf`, `realpath`.
  `openat` does not have the race conditions that `open` has. Etc.
* Reliability: Gnulib provides functions that combine a call to a system
  function with a check of the result. Examples are `xalloc`,
  `xprintf`, `xstrtod`, `xgetcwd`.
* Structure: Gnulib offers a way to structure code into modules, typically
  one include file, one source code file, and one autoconf macro for each
  functionality. Modularity helps maintainability.

---

Next: [Portability and Application Code](#Portability-and-Application-Code), Previous: [Benefits of using Gnulib](#Benefits), Up: [Philosophy](#Philosophy)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 2.2 Library vs. Reusable Code [¶](#Library-vs_002e-Reusable-Code)

Classical libraries are installed as binary object code. Gnulib is
different: It is used as a source code library. Each package that uses
Gnulib thus ships with part of the Gnulib source code. The used portion
of Gnulib is tailored to the package: A build tool, called
`gnulib-tool`, is provided that copies a tailored subset of Gnulib
into the package.

* [Comparison with other source code libraries](#Other-source-code-libraries)

---

Up: [Library vs. Reusable Code](#Library-vs-Reusable-Code)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 2.2.1 Comparison with other source code libraries [¶](#Comparison-with-other-source-code-libraries)

Many programming languages nowadays have

* an ecosystem of reusable source code packages, available through a central site,
* together with a tool that downloads dependencies from this central site.

Here is a table of such source code libraries.

| Language | Central site | Download tool |
| --- | --- | --- |
| Python | [pypi.org](https://pypi.org) | ‘`pip`’ |
| Java | [mvnrepository.com](https://mvnrepository.com) | ‘`maven`’, ‘`gradle`’ |
| C# | [nuget.org](https://nuget.org) | ‘`nuget`’ |
| JavaScript, TypeScript | [npmjs.com](https://npmjs.com) | ‘`npm`’, ‘`yarn`’ |
| Common Lisp | [quicklisp.org](https://quicklisp.org) | ‘`ql:quickload`’ |
| Emacs Lisp | [elpa.gnu.org](https://elpa.gnu.org) | ‘`M-x package-install`’ |
| Rust | [crates.io](https://crates.io) | ‘`cargo`’ |
| Go | [pkg.go.dev](https://pkg.go.dev) | ‘`go mod`’ |
| Ruby | [rubygems.org](https://rubygems.org) | ‘`gem`’ |
| Lua | [luarocks.org](https://luarocks.org) | ‘`luarocks`’ |
| OCaml | [ocaml.org/packages](https://ocaml.org/packages) | ‘`opam`’ |
| D | [code.dlang.org](https://code.dlang.org) | ‘`dub`’ |
| Perl | [cpan.org](https://cpan.org) | ‘`cpan`’ |
| PHP | [packagist.org](https://packagist.org) | ‘`composer`’ |
| R | [cran.r-project.org](https://cran.r-project.org) | ‘`R CMD INSTALL`’ |

Most of them are vulnerable to *supply chain attacks*.
These are cyber-attacks to the central site (library)
or to one of the packages in the library,
with the goal of attacking the packages that use that specific package
as a (direct or indirect) dependency.

Examples of such supply chain attacks:

* A disenchanted package maintainer may replace the code of their package
  with something that does not work any more.
* An attacker may plant a backdoor in a package.
* An attacker may register packages whose names only slightly differ
  from well-known and heavily used packages (“typo-squatting”).

While some mitigations exist, they are often cumbersome to put in place.
For example, a developer could set up a partial mirror of the library,
where adding a package to the mirror is a manual operation,
together with a configuration or modification of the tool that
redirects download or installation requests to the mirror.

Gnulib is not vulnerable to such attacks, because
all of its code is managed in a single repository,
with a limited set of committers
and with established code review practices.

---

Next: [Target Platforms](#Target-Platforms), Previous: [Library vs. Reusable Code](#Library-vs-Reusable-Code), Up: [Philosophy](#Philosophy)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 2.3 Portability and Application Code [¶](#Portability-and-Application-Code-1)

One of the goals of Gnulib is to make portable programming easy, on
the basis of the standards relevant for GNU (and Unix). The objective
behind that is to avoid a fragmentation of the user community into
disjoint user communities according to the operating system, and
instead allow synergies between users on different operating systems.

Another goal of Gnulib is to provide application code that can be shared
between several applications. Some people wonder: "What? glibc doesn’t
have a function to copy a file?" Indeed, the scope of a system’s libc is
to implement the relevant standards (ISO C, POSIX) and to provide
access functions to the kernel’s system calls, and little more.

There is no clear borderline between both areas.

For example, Gnulib has a facility for generating the name of backup
files. While this task is entirely at the application level—no
standard specifies an API for it—the naïve code has
some portability problems because on some platforms the length of file
name components is limited to 30 characters or so. Gnulib handles
that.

Similarly, Gnulib has a facility for executing a command in a
subprocess. It is at the same time a portability enhancement (it
works on GNU, Unix, and Windows, compared to the classical
`fork`/`exec` idiom which is not portable to Windows), as well
as an application aid: it takes care of redirecting stdin and/or
stdout if desired, and emits an error message if the subprocess
failed.

---

Next: [Modules](#Modules), Previous: [Portability and Application Code](#Portability-and-Application-Code), Up: [Philosophy](#Philosophy)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 2.4 Target Platforms [¶](#Target-Platforms-1)

Gnulib supports a number of platforms that we call the “reasonable
portability targets”. This class consists of widespread operating systems,
for three years after their last availability, or—for proprietary
operating systems—as long as the vendor provides commercial support for
it. Already existing Gnulib code for older operating systems is usually
left in place for longer than these three years. So it comes that programs
that use Gnulib run pretty well also on these older operating systems.

Some operating systems are not very widespread, but are Free Software and
are actively developed. Such platforms are also supported by Gnulib, if
that OS’s developers community keeps in touch with the Gnulib developers,
by providing bug reports, analyses, or patches. For such platforms, Gnulib
supports only the versions of the last year or the last few months,
depending on the maturity of said OS project, the number of its users, and
how often these users upgrade.

Niche operating systems are generally unsupported by Gnulib, unless some
of their developers or users contribute support to Gnulib.

The degree of support Gnulib guarantees for a platform depends on the
amount of testing it gets from volunteers. Platforms on which Gnulib
is frequently tested are the best supported. Then come platforms with
occasional testing, then platforms which are rarely tested. Usually,
we fix bugs when they are reported. Except that some rarely tested
platforms are also low priority; bug fixes for these platforms can
take longer.

* [Supported Platforms](#Supported-Platforms)
* [Formerly Supported Platforms](#Formerly-Supported-Platforms)
* [Unsupported Platforms](#Unsupported-Platforms)

---

Next: [Formerly Supported Platforms](#Formerly-Supported-Platforms), Up: [Target Platforms](#Target-Platforms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 2.4.1 Supported Platforms [¶](#Supported-Platforms-1)

As of 2024, the list of supported platforms is the following:

* glibc systems. With glibc 2.19 or newer, they are frequently tested.
  About the kernels:
  + glibc on Linux is frequently tested.
  + glibc on kFreeBSD is rarely tested.
  + musl libc on Linux is occasionally tested.
* macOS. In versions 12.5, it’s occasionally tested. In version
  10.5, it’s rarely tested.
* FreeBSD 14.0 or newer is occasionally tested.
* OpenBSD 7.0 or newer is occasionally tested.
* NetBSD 10.0 or newer is occasionally tested.
* CheriBSD 25.03 or newer is occasionally tested.
* AIX 7.1 and 7.3 are occasionally tested.
* Solaris 10 and 11.4 are occasionally tested. Solaris 9 is rarely
  tested and low priority.
* Android is occasionally tested, through the Termux app on Android 11.
* Cygwin 2.9 and 3.4 are occasionally tested. Cygwin 1.7.x no longer tested.
* Native Windows:
  + mingw is occasionally tested. Only the latest version of mingw is
    tested; older versions are not supported.
  + MSVC 14 (Microsoft Visual Studio 2015 14.0) is occasionally tested.
    Only “release” builds (compiler option ‘`-MD`’) are supported,
    not “debug” builds (compiler option ‘`-MDd`’).

  Note that some modules are currently unsupported on native Windows:
  `mgetgroups`, `getugroups`, `idcache`,
  `userspec`, `openpty`, `login_tty`, `forkpty`,
  `pt_chown`, `grantpt`, `pty-h`, `savewd`,
  `mkancesdirs`, `mkdir-p`, `euidaccess`, `faccessat`.
  The versions of Windows that are supported are Windows 10 and newer.
* GNU Hurd 0.9 is rarely tested.
* Minix 3.3.0 is no longer tested.
* Haiku is no longer tested.
* uClibc on Linux is no longer tested.
* QNX is no longer tested.

---

Next: [Unsupported Platforms](#Unsupported-Platforms), Previous: [Supported Platforms](#Supported-Platforms), Up: [Target Platforms](#Target-Platforms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 2.4.2 Formerly Supported Platforms [¶](#Formerly-Supported-Platforms-1)

The following platforms were supported in the past, but are no longer
supported:

* glibc versions 2.1.x and older.
* Mac OS X 10.4 and older.
* AIX 6 and older.
* IBM XL C/C++ 13.1 and older.
* HP-UX 11.31.
* IRIX 6.5 and older.
* OSF/1 5.1.
* Solaris 8 and older.
* Interix.
* BeOS.

Gnulib supports these operating systems only in an unvirtualized environment.
When you run an OS inside a virtual machine, you have to be aware that the
virtual machine can bring in bugs of its own. For example, floating-point
operations on Solaris can behave slightly differently in QEMU than on real
hardware. And Haiku’s `bash` program misbehaves in VirtualBox 3,
whereas it behaves fine in VirtualBox 4.

Similarly, running native Windows binaries on GNU/Linux under WINE is
rarely tested and low priority: WINE has a set of behaviours and bugs that
is slightly different from native Windows.

---

Previous: [Formerly Supported Platforms](#Formerly-Supported-Platforms), Up: [Target Platforms](#Target-Platforms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 2.4.3 Unsupported Platforms [¶](#Unsupported-Platforms-1)

Some platforms with C compilers are not supported by Gnulib because
the platforms violate Gnulib’s C portability assumptions. See [Other portability assumptions made by Gnulib](#Other-portability-assumptions).

These assumptions are not required by the C or POSIX standards but
hold on almost all practical porting targets. If you need to port
Gnulib code to a platform where these assumptions are not true, we
would appreciate hearing of any fixes. We need fixes that do not
increase runtime overhead on standard hosts and that are relatively
easy to maintain.

These platforms are listed below to illustrate problems that Gnulib
and Gnulib-using code would have if it were intended to be portable to
all practical POSIX or C platforms.

* In Clang 19 and earlier, the `-fsanitize=undefined` option
  causes the program to crash if it adds zero to a null pointer –
  behavior that is undefined in strict C17 and earlier, but which yields
  a null pointer in C23 and later and on all practical porting targets,
  and which the Gnulib portability guidelines allow.

  If you use Clang 19 or earlier with `-fsanitize=undefined`, you can work
  around the problem by also using ‘`-fno-sanitize=pointer-overflow`’,
  although this may also disable some unrelated and useful pointer checks.
  Perhaps someday the Clang developers will fix the infelicity.
* The IBM i’s pointers are 128 bits wide and it lacks the two types
  `intptr_t` and `uintptr_t`, which are optional in the C and
  POSIX standards. However, these two types are required for the XSI
  extension to POSIX, and many Gnulib modules use them. To work around
  this compatibility problem, Gnulib-using applications can be run on
  the IBM i’s PASE emulation environment. The IBM i’s architecture
  descends from the System/38 (1978).
* The Unisys ClearPath Dorado’s machine word is 36 bits. Its signed
  integers use a ones’-complement representation that conforms to
  C17 and earlier but not to C23 and later. On these machines,
  `CHAR_BIT == 9` and `INT_MIN == -INT_MAX`. By default
  `UINT_MAX` is *2^{36} - 2*, which does not conform to the C
  requirement that it be one less than a power of two. Although
  compiler options can raise `UINT_MAX` to be *2^{36} - 1*,
  this can break system code that uses *-0* as a flag value.
  This platform’s architecture descends from the UNIVAC 1103 (1953).
* The Unisys ClearPath Libra’s machine word is 48 bits
  with a 4-bit tag and a 4-bit data extension. Its
  `unsigned int` uses the low-order 39 bits of the word, and
  `int` uses the low-order 40 bits of the word with a
  signed-magnitude representation that conforms to C17 and earlier
  but not to C23 and later. On these machines, `INT_MAX ==
  UINT_MAX`, `INT_MIN == -INT_MAX`, and `sizeof (int) == 6`.
  This platform’s architecture descends from the Burroughs B5000 (1961).

The following platforms are not supported by Gnulib. The cost of
supporting them would exceed the benefit because they are rarely used, or
poorly documented, or have been supplanted by other platforms, or diverge
too much from POSIX, or some combination of these and other factors.
Please don’t bother sending us patches for them.

* Windows 95/98/ME.
* DJGPP and EMX (the 32-bit operating systems running in DOS).
* MSDOS (the 16-bit operating system).
* Windows Mobile, Symbian OS, iOS.

---

Next: [Various Kinds of Modules](#Various-Kinds-of-Modules), Previous: [Target Platforms](#Target-Platforms), Up: [Philosophy](#Philosophy)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 2.5 Modules [¶](#Modules-1)

Gnulib is divided into modules. Every module implements a single
facility. Modules can depend on other modules.

A module consists of a number of files and a module description. The
files are copied by `gnulib-tool` into the package that will use it,
usually verbatim, without changes. Source code files (.h, .c files)
reside in the `lib/` subdirectory. Autoconf macro files reside in
the `m4/` subdirectory. Build scripts reside in the
`build-aux/` subdirectory.

The module description contains the list of files; `gnulib-tool`
copies these files. It contains the module’s
dependencies; `gnulib-tool` installs them as well. It also
contains the autoconf macro invocation (usually a single line or
nothing at all); `gnulib-tool` ensures this is invoked from the
package’s `configure.ac` file. And also a `Makefile.am`
snippet; `gnulib-tool` collects these into a `Makefile.am`
for the tailored Gnulib part. The module description and include file
specification are for documentation purposes; they are combined into
`MODULES.html`.

The module system serves two purposes:

1. It ensures consistency of the used autoconf macros and `Makefile.am`
   rules with the source code. For example, source code which uses the
   `getopt_long` function—this is a common way to implement parsing
   of command line options in a way that complies with the GNU standards—needs
   the source code (`lib/getopt.c` and others), the autoconf macro
   which detects whether the system’s libc already has this function (in
   `m4/getopt.m4`), and a few `Makefile.am` lines that create the
   substitute `getopt.h` if not. These three pieces belong together.
   They cannot be used without each other. The module description and
   `gnulib-tool` ensure that they are copied altogether into the
   destination package.
2. It allows for scalability. It is well-known since the inception of the
   MODULA-2 language around 1978 that dissection into modules with
   dependencies allows for building large sets of code in a maintainable way.
   The maintainability comes from the facts that:
   * Every module has a single purpose; you don’t worry about other parts of
     the program while creating, reading or modifying the code of a module.
   * The code you have to read in order to understand a module is limited to
     the source of the module and the .h files of the modules listed as
     dependencies. It is for this reason also that we recommend to put the
     comments describing the functions exported by a module into its .h file.

   In other words, the module is the elementary unit of code in Gnulib,
   comparable to a class in object-oriented languages like Java or C#.

The module system is the basis of `gnulib-tool`. When
`gnulib-tool` copies a part of Gnulib into a package, it first
compiles a module list, starting with the requested modules and adding all
the dependencies, and then collects the files, `configure.ac`
snippets and `Makefile.am` snippets.

---

Next: [Collaborative Development](#Collaborative-Development), Previous: [Modules](#Modules), Up: [Philosophy](#Philosophy)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 2.6 Various Kinds of Modules [¶](#Various-Kinds-of-Modules-1)

There are modules of various kinds in Gnulib. For a complete list of the
modules, see in `MODULES.html`.

* [Support for ISO C or POSIX functions.](#Support-for-ISO-C-or-POSIX-functions_002e)
* [Enhancements of ISO C or POSIX functions](#Enhancements-of-ISO-C-or-POSIX-functions)
* [Portable general use facilities](#Portable-general-use-facilities)
* [Reusable application code](#Reusable-application-code)
* [Object oriented classes](#Object-oriented-classes)
* [Interfaces to external libraries](#Interfaces-to-external-libraries)
* [Build / maintenance infrastructure](#Build-_002f-maintenance-infrastructure)

#### 2.6.1 Support for ISO C or POSIX functions. [¶](#Support-for-ISO-C-or-POSIX-functions_002e)

When a function is not implemented by a system, the Gnulib module provides
an implementation under the same name. Examples are the ‘`snprintf`’
and ‘`readlink`’ modules.

Similarly, when a function is not correctly implemented by a system,
Gnulib provides a replacement. For functions, we use the pattern

```
#if !HAVE_WORKING_FOO
# define foo rpl_foo
#endif
```

and implement the `foo` function under the name `rpl_foo`. This
renaming is needed to avoid conflicts at compile time (in case the system
header files declare `foo`) and at link/run time (because the code
making use of `foo` could end up residing in a shared library, and
the executable program using this library could be defining `foo`
itself).

For header files, such as `stdint.h`, we provide
the substitute only if the system doesn’t provide a correct one. The
template of this replacement is distributed in a slightly different name,
with ‘`.in`’ inserted before the ‘`.h`’ extension, so that on
systems which do provide a correct
header file the system’s one is used.

The modules in this category are supported in C++ mode as well. This
means, while the autoconfiguration uses the C compiler, the resulting
header files and function substitutes can be used with a matching C++
compiler as well.

#### 2.6.2 Enhancements of ISO C or POSIX functions [¶](#Enhancements-of-ISO-C-or-POSIX-functions)

These are sometimes POSIX functions with GNU extensions also found in
glibc—examples: ‘`getopt`’, ‘`fnmatch`’—and often new
APIs—for example, for all functions that allocate memory in one way
or the other, we have variants which also include the error checking
against the out-of-memory condition.

#### 2.6.3 Portable general use facilities [¶](#Portable-general-use-facilities)

Examples are a module for copying a file—the portability problems
relate to the copying of the file’s modification time, access rights,
and extended attributes—or a module for extracting the tail
component of a file name—here the portability to native Windows
requires a different API than the classical POSIX `basename` function.

#### 2.6.4 Reusable application code [¶](#Reusable-application-code)

Examples are an error reporting function, a module that allows output of
numbers with K/M/G suffixes, or cryptographic facilities.

#### 2.6.5 Object oriented classes [¶](#Object-oriented-classes)

Examples are data structures like ‘`list`’, or abstract output stream
classes that work around the fact that an application cannot implement an
stdio `FILE` with its logic. Here, while staying in C, we use
implementation techniques like tables of function pointers, known from the
C++ language or from the Linux kernel.

#### 2.6.6 Interfaces to external libraries [¶](#Interfaces-to-external-libraries)

Examples are the ‘`iconv`’ module, which interfaces to the
`iconv` facility, regardless whether it is contained in libc or in
an external `libiconv`.
Or the ‘`readline`’ module, which interfaces to the GNU readline library.

#### 2.6.7 Build / maintenance infrastructure [¶](#Build-_002f-maintenance-infrastructure)

An example is the ‘`maintainer-makefile`’ module, which provides extra
Makefile tags for maintaining a package.

---

Next: [Copyright](#Copyright), Previous: [Various Kinds of Modules](#Various-Kinds-of-Modules), Up: [Philosophy](#Philosophy)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 2.7 Collaborative Development [¶](#Collaborative-Development-1)

Gnulib is maintained collaboratively. The mailing list is
`<bug-gnulib at gnu dot org>`. Be warned that some people on the
list may be very active at some times and unresponsive at other times.

Every module has one or more maintainers. While issues are discussed
collaboratively on the list, the maintainer of a module nevertheless has
a veto right regarding changes in his module.

All patches should be posted to the list, regardless whether they are
proposed patches or whether they are committed immediately by the
maintainer of the particular module. The purpose is not only to inform
the other users of the module, but mainly to allow peer review. It is not
uncommon that several people contribute comments or spot bugs after a
patch was proposed.

Conversely, if you are using Gnulib, and a patch is posted that affects
one of the modules that your package uses, you have an interest in
proofreading the patch.

---

Next: [Steady Development](#Steady-Development), Previous: [Collaborative Development](#Collaborative-Development), Up: [Philosophy](#Philosophy)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 2.8 Copyright [¶](#Copyright-1)

Most modules are under the GPL. Some, mostly modules which can
reasonably be used in libraries, are under LGPL. Few modules are
under other licenses, such as LGPLv2+, unlimited, or public domain.

If the module description file says "GPL", it means "GPLv3+" (GPLv3
or newer, at the licensee’s choice); if it says "LGPL", it means
"LGPLv3+" (LGPLv3 or newer, at the licensee’s choice).

The source files, more precisely the files in `lib/` and
`build-aux/`, are under a license compatible with the module’s
license. Most often, they are under the same license. But files can be
shared among several modules, and in these cases it can happen that a
source file is under a weaker license than noted in the module
description – namely under the weakest license among the licenses of
the modules that contain the file.

Different licenses apply to files in special directories:

`modules/`
:   Module description files are under this copyright:

    > Copyright © 20XX–20YY Free Software Foundation, Inc.  
    > Copying and distribution of this file, with or without modification,
    > in any medium, are permitted without royalty provided the copyright
    > notice and this notice are preserved. This file is offered as-is,
    > without any warranty.

`m4/`
:   Autoconf macro files are under this copyright:

    > Copyright © 20XX–20YY Free Software Foundation, Inc.  
    > This file is free software; the Free Software Foundation
    > gives unlimited permission to copy and/or distribute it,
    > with or without modifications, as long as this notice is preserved.
    > This file is offered as-is, without any warranty.

    or under this copyright:

    > Copyright © 20XX–20YY Free Software Foundation, Inc.  
    > This file is free software, distributed under the terms of the GNU
    > General Public License as published by the Free Software Foundation;
    > either version 2 of the License, or (at your option) any later version.
    > As a special exception to the GNU General Public License, this file
    > may be distributed as part of a program that contains a configuration
    > script generated by Autoconf, under the same distribution terms as
    > the rest of that program.

    We denote either of these license statements as “unlimited”.

`tests/`
:   If a license statement is not present in a test module, the test files are
    under GPL. Even if the corresponding source module is under LGPL, this is
    not a problem, since compiled tests are not installed by “make install”.

`doc/`
:   Documentation files are under this copyright:

    > Copyright © 2004–20YY Free Software Foundation, Inc.  
    > Permission is granted to copy, distribute and/or modify this document
    > under the terms of the GNU Free Documentation License, Version 1.3 or
    > any later version published by the Free Software Foundation; with no
    > Invariant Sections, no Front-Cover Texts, and no Back-Cover Texts. A
    > copy of the license is at <https://www.gnu.org/licenses/fdl-1.3.en.html>.

If you want to use some Gnulib modules under LGPL, you can do so by
passing the option ‘`--lgpl`’ to `gnulib-tool`. This will
ensure that all imported modules can be used under the LGPL license.
Similarly, if you want some Gnulib modules
under LGPLv2+ (Lesser GPL version 2.1 or newer), you can do so by
passing the option ‘`--lgpl=2`’ to `gnulib-tool`.

Keep in mind that when you submit patches to files in Gnulib, you should
license them under a compatible license. This means that sometimes the
contribution will have to be LGPL, if the original file is available
under LGPL. You can find out about it by looking at the license header
of the file.

---

Next: [Openness](#Openness), Previous: [Copyright](#Copyright), Up: [Philosophy](#Philosophy)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 2.9 Steady Development [¶](#Steady-Development-1)

Gnulib modules are continually adapted, to match new practices, to be
consistent with newly added modules, or simply as a response to build
failure reports.

If you are willing to report an occasional regression, we recommend to
use the newest version from git always, except in periods of major
changes. Most Gnulib users do this.

---

Previous: [Steady Development](#Steady-Development), Up: [Philosophy](#Philosophy)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 2.10 Openness [¶](#Openness-1)

Gnulib is open in the sense that we gladly accept contributions if they
are generally useful, well engineered, and if the contributors have signed
the obligatory papers with the FSF.

The module system is open in the sense that a package using Gnulib can

1. locally patch or override files in Gnulib,
2. locally add modules that are treated like Gnulib modules by
   `gnulib-tool`.

This is achieved by the ‘`--local-dir`’ option of `gnulib-tool`
(see [Extending Gnulib](#Extending-Gnulib)).

---

Next: [Writing modules](#Writing-modules), Previous: [Philosophy](#Philosophy), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 3 Invoking gnulib-tool [¶](#Invoking-gnulib_002dtool-1)

The `gnulib-tool` command is the recommended way to import
Gnulib modules. It is possible to borrow Gnulib modules in a package
without using `gnulib-tool`, relying only on the
meta-information stored in the `modules/*` files, but with a
growing number of modules this becomes tedious. `gnulib-tool`
simplifies the management of source files, `Makefile.am`s and
`configure.ac` in packages incorporating Gnulib modules.

`gnulib-tool` is not installed in a standard directory that is
contained in the `PATH` variable. It needs to be run directly in
the directory that contains the Gnulib source code. You can do this
either by specifying the absolute filename of `gnulib-tool`, or
you can also use a symbolic link from a place inside your `PATH`
to the `gnulib-tool` file of your preferred and most up-to-date
Gnulib checkout, like this:

```
$ ln -s $HOME/gnu/src/gnulib.git/gnulib-tool $HOME/bin/gnulib-tool
```

Run ‘`gnulib-tool --help`’ for information. To get familiar with
`gnulib-tool` without affecting your sources, you can also try
some commands with the option ‘`--dry-run`’; then
`gnulib-tool` will only report which actions it would perform in
a real run without changing anything.

* [Finding modules](#Which-modules_003f)
* [Initial import](#Initial-import)
* [Modified imports](#Modified-imports)
* [Simple update](#Simple-update)
* [Changing your sources for use with Gnulib](#Source-changes)
* [Changing your link commands for use with Gnulib](#Link_002dtime-requirements)
* [Finding recommended ISO C and POSIX function substitutes](#Finding-POSIX-substitutes)
* [Modifying the build rules of a Gnulib import directory](#Modified-build-rules)
* [Building directly from the top-level directory](#Non_002drecursive-make)
* [Using Gnulib for both a library and a program](#Multiple-instances)
* [Caveat: `gettextize` and `autopoint` users](#gettextize-and-autopoint)
* [Handling Gnulib’s own message translations](#Localization)
* [Integration with Version Control Systems](#VCS-Issues)
* [Bundling the unit tests of the Gnulib modules](#Unit-tests)
* [Avoiding unnecessary checks and compilations](#Conditional-dependencies)

---

Next: [Initial import](#Initial-import), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 3.1 Finding modules [¶](#Finding-modules)

There are four ways of finding the names of Gnulib modules that you can use
in your package:

* You have the complete module list, sorted according to categories, in
  <https://www.gnu.org/software/gnulib/MODULES.html>.
* If you are looking for POSIX function replacements that you don’t know about
  yet, follow the procedure described in section [Finding recommended ISO C and POSIX function substitutes](#Finding-POSIX-substitutes).
* If you are looking for a particular POSIX header or function replacement,
  look in the chapters [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes) and
  [ISO C and POSIX Function Substitutes](#Function-Substitutes). For headers and functions that are provided by
  Glibc but not standardized by POSIX, look in the chapters
  [Glibc Header File Substitutes](#Glibc-Header-File-Substitutes) and [Glibc Function Substitutes](#Glibc-Function-Substitutes).
* If you have already found the source file in Gnulib and are looking for the
  module that contains this source file, you can use the command
  ‘`gnulib-tool --find filename`’.

---

Next: [Modified imports](#Modified-imports), Previous: [Finding modules](#Which-modules_003f), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 3.2 Initial import [¶](#Initial-import-1)

Gnulib assumes that your project uses Autoconf. When using Gnulib, you
will need to have Autoconf among your build tools.

Gnulib also assumes that your project’s `configure.ac` contains the
line

```
AC_CONFIG_HEADERS([config.h])
```

The `config.h` file gets generated with platform dependent C macro
definitions, and the source files include it (see [Changing your sources for use with Gnulib](#Source-changes)).

Unless you use `gnulib-tool`’s `--gnu-make` option,
Gnulib also assumes that your project uses Automake at least in a
subdirectory of your project. While the use of Automake in your
project’s top level directory is an easy way to fulfil the Makefile
conventions of the GNU coding standards, Gnulib does not require it.

Invoking ‘`gnulib-tool --import`’ will copy source files, create a
`Makefile.am` to build them, generate a file `gnulib-comp.m4` with
Autoconf M4 macro declarations used by `configure.ac`, and generate
a file `gnulib-cache.m4` containing the cached specification of how
Gnulib is used.

Our example will be a library that uses Autoconf, Automake and
Libtool. It calls `strdup`, and you wish to use gnulib to make
the package portable to C99 and C11 (which don’t have `strdup`).

```
~/src/libfoo$ gnulib-tool --import strdup
Module list with included dependencies:
  absolute-header
  extensions
  strdup
  string
File list:
  lib/dummy.c
  lib/strdup.c
  lib/string.in.h
  m4/absolute-header.m4
  m4/extensions.m4
  m4/gnulib-common.m4
  m4/strdup.m4
  m4/string_h.m4
Creating directory ./lib
Creating directory ./m4
Copying file lib/dummy.c
Copying file lib/strdup.c
Copying file lib/string.in.h
Copying file m4/absolute-header.m4
Copying file m4/extensions.m4
Copying file m4/gnulib-common.m4
Copying file m4/gnulib-tool.m4
Copying file m4/strdup.m4
Copying file m4/string_h.m4
Creating lib/Makefile.am
Creating m4/gnulib-cache.m4
Creating m4/gnulib-comp.m4
Finished.

You may need to add #include directives for the following .h files.
  #include <string.h>

Don't forget to
  - add "lib/Makefile" to AC_CONFIG_FILES in ./configure.ac,
  - mention "lib" in SUBDIRS in Makefile.am,
  - mention "-I m4" in ACLOCAL_AMFLAGS in Makefile.am,
    or add an AC_CONFIG_MACRO_DIRS([m4]) invocation in ./configure.ac,
  - invoke gl_EARLY in ./configure.ac, right after AC_PROG_CC,
  - invoke gl_INIT in ./configure.ac.
~/src/libfoo$
```

By default, the source code is copied into `lib/` and the M4
macros in `m4/`. You can override these paths by using
`--source-base=DIRECTORY` and `--m4-base=DIRECTORY`. Some
modules also provide other files necessary for building. These files
are copied into the directory specified by ‘`AC_CONFIG_AUX_DIR`’ in
`configure.ac` or by the `--aux-dir=DIRECTORY` option. If
neither is specified, the current directory is assumed.

`gnulib-tool` can make symbolic links instead of copying the
source files. The option to specify for this is ‘`--symlink`’, or
‘`-s`’ for short. This can be useful to save a few kilobytes of disk
space. But it is likely to introduce bugs when `gnulib` is updated;
it is more reliable to use ‘`gnulib-tool --update`’ (see below)
to update to newer versions of `gnulib`. Furthermore it requires
extra effort to create self-contained tarballs, and it may disturb some
mechanism the maintainer applies to the sources. For these reasons,
this option is generally discouraged.

`gnulib-tool` will overwrite any preexisting files, in
particular `Makefile.am`. It is also possible to separate the
generated `Makefile.am` content (for building the gnulib library)
into a separate file, say `gnulib.mk`, that can be included by your
handwritten `Makefile.am`, but this is a more advanced use of
`gnulib-tool`.

Consequently, it is a good idea to choose directories that are not
already used by your projects, to separate gnulib imported files from
your own files. This approach is also useful if you want to avoid
conflicts between other tools (e.g., `gettextize` that also copy
M4 files into your package. Simon Josefsson successfully uses a source
base of `gl/`, and a M4 base of `gl/m4/`, in several
packages.

After the ‘`--import`’ option on the command line comes the list of
Gnulib modules that you want to incorporate in your package. The names
of the modules coincide with the filenames in Gnulib’s `modules/`
directory.

Some Gnulib modules depend on other Gnulib modules. `gnulib-tool`
will automatically add the needed modules as well; you need not list
them explicitly. `gnulib-tool` will also memorize which dependent
modules it has added, so that when someday a dependency is dropped, the
implicitly added module is dropped as well (unless you have explicitly
requested that module).

If you want to cut a dependency, i.e., not add a module although one of
your requested modules depends on it, you may use the option
‘`--avoid=module`’ to do so. Multiple uses of this option are
possible. Of course, you will then need to implement the same interface
as the removed module.

A few manual steps are required to finish the initial import.
`gnulib-tool` printed a summary of these steps.

First, you must ensure Autoconf can find the macro definitions in
`gnulib-comp.m4`. Use the `ACLOCAL_AMFLAGS` specifier in
your top-level `Makefile.am` file, as in:

```
ACLOCAL_AMFLAGS = -I m4
```

Alternatively, add an `AC_CONFIG_MACRO_DIRS` invocation in your
`configure.ac` file, as in:

```
AC_CONFIG_MACRO_DIRS([m4])
```

You are now ready to call the M4 macros in `gnulib-comp.m4` from
`configure.ac`. The macro `gl_EARLY` must be called as soon
as possible after verifying that the C compiler is working.
Typically, this is immediately after `AC_PROG_CC`, as in:

```
...
AC_PROG_CC
gl_EARLY
...
```

The core part of the gnulib checks are done by the macro
`gl_INIT`. Place it further down in the file, typically where
you normally check for header files or functions. It must come after
other checks which may affect the compiler invocation, such as
`AC_MINIX`. For example:

```
...
# For gnulib.
gl_INIT
...
```

`gl_INIT` will in turn call the macros related with the
gnulib functions, be it specific gnulib macros, like `gl_FUNC_ALLOCA`
or Autoconf or Automake macros like `AC_FUNC_ALLOCA` or
`AM_FUNC_GETLINE`. So there is no need to call those macros yourself
when you use the corresponding gnulib modules.

You must also make sure that the gnulib library is built. Add the
`Makefile` in the gnulib source base directory to
`AC_CONFIG_FILES`, as in:

```
AC_CONFIG_FILES(... lib/Makefile ...)
```

You must also make sure that `make` will recurse into the gnulib
directory. To achieve this, add the gnulib source base directory to a
`SUBDIRS` Makefile.am statement, as in:

```
SUBDIRS = lib
```

or if you, more likely, already have a few entries in `SUBDIRS`,
you can add something like:

```
SUBDIRS += lib
```

Finally, you have to add compiler and linker flags in the appropriate
source directories, so that you can make use of the gnulib library.
Since some modules (‘`getopt`’, for example) may copy files into
the build directory, `top_builddir/lib` is needed as well
as `top_srcdir/lib`. For example:

```
...
AM_CPPFLAGS = -I$(top_builddir)/lib -I$(top_srcdir)/lib
...
LDADD = lib/libgnu.a
...
```

Don’t forget to `#include` the various header files. In this
example, you would need to make sure that ‘`#include <string.h>`’
is evaluated when compiling all source code files, that want to make
use of `strdup`.

In the usual case where Autoconf is creating a `config.h` file,
you should include `config.h` first, before any other include
file. That way, for example, if `config.h` defines
‘`restrict`’ to be the empty string on a non-C99 host, or a macro
like ‘`_FILE_OFFSET_BITS`’ that affects the layout of data
structures, the definition is consistent for all include files.
Also, on some platforms macros like ‘`_FILE_OFFSET_BITS`’ and
‘`_GNU_SOURCE`’ may be ineffective, or may have only a limited
effect, if defined after the first system header file is included.

Finally, note that you cannot use `AC_LIBOBJ` or
`AC_REPLACE_FUNCS` in your `configure.ac` and expect the
resulting object files to be automatically added to `lib/libgnu.a`.
This is because your `AC_LIBOBJ` and `AC_REPLACE_FUNCS` invocations
from `configure.ac` augment a variable `@LIBOBJS@` (and/or
`@LTLIBOBJS@` if using Libtool), whereas `lib/libgnu.a`
is built from the contents of a different variable, usually
`@gl_LIBOBJS@` (or `@gl_LTLIBOBJS@` if using Libtool).

---

Next: [Simple update](#Simple-update), Previous: [Initial import](#Initial-import), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 3.3 Modified imports [¶](#Modified-imports-1)

You can at any moment decide to use Gnulib differently than the last time.

There are two ways to change how Gnulib is used. Which one you’ll use,
depends on where you keep track of options and module names that you pass
to `gnulib-tool`.

* If you store the options and module names in a file under your own
  control, such as `autogen.sh`, `bootstrap`,
  `bootstrap.conf`, or similar, simply invoke `gnulib-tool`
  again, with modified options and more or fewer module names.
* `gnulib-tool` remembers which modules were used last time. If you
  want to rely on `gnulib-tool`’s own memory of the last used
  options and module names, you can use the commands
  `gnulib-tool --add-import` and
  `gnulib-tool --remove-import`.

  So, if you only want to use more Gnulib modules, simply invoke
  `gnulib-tool --add-import new-modules`. The list of
  modules that you pass after ‘`--add-import`’ is *added* to the
  previous list of modules.

  Similarly, if you want to use fewer Gnulib modules, simply invoke
  `gnulib-tool --remove-import unneeded-modules`. The list
  of modules that you pass after ‘`--remove-import`’ is *removed*
  from the previous list of modules. Note that if a module is then still
  needed as dependency of other modules, it will be used nevertheless.
  If you want to *really* not use a module any more, regardless of
  whether other modules may need it, you need to use the ‘`--avoid`’
  option.

  For other changes, such as different choices of ‘`--lib`’,
  ‘`--source-base`’ or ‘`--aux-dir`’, the normal way is to
  modify manually the file `gnulib-cache.m4` in the M4 macros
  directory, then launch ‘`gnulib-tool --add-import`’.

  The only change for which this doesn’t work is a change of the
  ‘`--m4-base`’ directory. Because, when you pass a different value of
  ‘`--m4-base`’, `gnulib-tool` will not find the previous
  `gnulib-cache.m4` file any more. A possible solution is to
  manually copy the `gnulib-cache.m4` into the new M4 macro directory.

  In the `gnulib-cache.m4` file, the macros have the following meaning:

  `gl_LOCAL_DIR`
  :   The argument is a colon separated list of local directories where
      `gnulib-tool` will search before looking at gnulib’s directory.
      Corresponds to the ‘`--local-dir`’ command line argument.

  `gl_MODULES`
  :   The argument is a space separated list of the requested modules, not including
      dependencies.

  `gl_WITH_OBSOLETE`
  :   The presence of this macro corresponds to the ‘`--with-obsolete`’ command
      line argument. It takes no arguments.

  `gl_WITH_CXX_TESTS`
  :   The presence of this macro corresponds to the ‘`--with-c++-tests`’ command
      line argument and to the absence of the ‘`--without-c++-tests`’ command line
      argument. It takes no arguments.

  `gl_WITH_LONGRUNNING_TESTS`
  :   The presence of this macro corresponds to the ‘`--with-longrunning-tests`’
      command line argument and to the absence of the
      ‘`--without-longrunning-tests`’ command line argument. It takes no
      arguments.

  `gl_WITH_PRIVILEGED_TESTS`
  :   The presence of this macro corresponds to the ‘`--with-longrunning-tests`’
      command line argument and to the absence of the
      ‘`--without-longrunning-tests`’ command line argument. It takes no
      arguments.

  `gl_WITH_UNPORTABLE_TESTS`
  :   The presence of this macro corresponds to the ‘`--with-unportable-tests`’
      command line argument and to the absence of the
      ‘`--without-unportable-tests`’ command line argument. It takes no arguments.

  `gl_WITH_ALL_TESTS`
  :   The presence of this macro corresponds to the ‘`--with-all-tests`’ command
      line argument. It takes no arguments.

  `gl_AVOID`
  :   The argument is a space separated list of modules that should not be used,
      even if they occur as dependencies. Corresponds to the ‘`--avoid`’
      command line argument.

  `gl_SOURCE_BASE`
  :   The argument is the relative file name of the directory containing the gnulib
      source files (mostly \*.c and \*.h files). Corresponds to the
      ‘`--source-base`’ command line argument.

  `gl_M4_BASE`
  :   The argument is the relative file name of the directory containing the gnulib
      M4 macros (\*.m4 files). Corresponds to the ‘`--m4-base`’ command line
      argument.

  `gl_PO_BASE`
  :   The argument is the relative file name of the directory containing \*.po files.
      Corresponds to the ‘`--po-base`’ command line argument.

  `gl_DOC_BASE`
  :   The argument is the relative file name of the directory containing documentation
      files. Corresponds to the ‘`--doc-base`’ command line argument.

  `gl_TESTS_BASE`
  :   The argument is the relative file name of the directory containing the gnulib
      unit test files. Corresponds to the ‘`--tests-base`’ command line argument.

  `gl_WITH_TESTS`
  :   The presence of this macro corresponds to the ‘`--with-tests`’ command line
      argument. It takes no arguments.

  `gl_LIB`
  :   The argument is the name of the library to be created. Corresponds to the
      ‘`--lib`’ command line argument.

  `gl_LGPL`
  :   The presence of this macro without arguments corresponds to the ‘`--lgpl`’
      command line argument. The presence of this macro with an argument (whose
      value must be 2 or 3) corresponds to the ‘`--lgpl=arg`’ command line
      argument.

  `gl_GPL`
  :   The presence of this macro with an argument (whose value must be 2 or 3)
      corresponds to the ‘`--gpl=arg`’ command line argument.

  `gl_MAKEFILE_NAME`
  :   The argument is the name of the makefile in the source-base and tests-base
      directories. Corresponds to the ‘`--makefile-name`’ command line argument.

  `gl_TESTS_MAKEFILE_NAME`
  :   The argument is the name of the makefile in the tests-base directory.
      Corresponds to the ‘`--tests-makefile-name`’ command line argument.

  `gl_AUTOMAKE_SUBDIR`
  :   The presence of this macro corresponds to the ‘`--automake-subdir`’ command
      line argument. It takes no arguments.

  `gl_CONDITIONAL_DEPENDENCIES`
  :   The presence of this macro corresponds to the ‘`--conditional-dependencies`’
      command line argument and to the absence of the
      ‘`--no-conditional-dependencies`’ command line argument. It takes no
      arguments.

  `gl_LIBTOOL`
  :   The presence of this macro corresponds to the ‘`--libtool`’ command line
      argument and to the absence of the ‘`--no-libtool`’ command line argument.
      It takes no arguments.

  `gl_MACRO_PREFIX`
  :   The argument is the prefix to use for macros in the `gnulib-comp.m4`
      file. Corresponds to the ‘`--macro-prefix`’ command line argument.

  `gl_PO_DOMAIN`
  :   The argument is the prefix of the i18n domain, typically matching the package
      name. Corresponds to the ‘`--po-domain`’ command line argument.

  `gl_WITNESS_C_MACRO`
  :   The argument is the C macro that is defined when the sources in this directory
      are compiled or used. Corresponds to the ‘`--witness-c-macro`’ command line
      argument.

  `gl_VC_FILES`
  :   The argument to this macro is `true` or `false`. The former
      corresponds to the ‘`--vc-files`’ command line argument and the latter
      corresponds to the ‘`--no-vc-files`’ command line argument.

---

Next: [Changing your sources for use with Gnulib](#Source-changes), Previous: [Modified imports](#Modified-imports), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 3.4 Simple update [¶](#Simple-update-1)

When you want to update to a more recent version of Gnulib, without
changing the list of modules or other parameters, a simple call
does it:

```
$ gnulib-tool --add-import
```

This will create, update or remove files, as needed.

Note: From time to time, changes are made in Gnulib that are not backward
compatible. When updating to a more recent Gnulib, you should consult
Gnulib’s `NEWS` file to check whether the incompatible changes affect
your project.

---

Next: [Changing your link commands for use with Gnulib](#Link_002dtime-requirements), Previous: [Simple update](#Simple-update), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 3.5 Changing your sources for use with Gnulib [¶](#Changing-your-sources-for-use-with-Gnulib)

When you use Gnulib, you need to make some small changes to your source code
base.

* [-I options](#g_t_002dI-options)
* [Include <config.h>](#Include-_003cconfig_002eh_003e)
* [Style of #include statements](#Style-of-_0023include-statements)

---

Next: [Include <config.h>](#Include-_003cconfig_002eh_003e), Up: [Changing your sources for use with Gnulib](#Source-changes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 3.5.1 -I options [¶](#g_t_002dI-options-1)

Gnulib contains some header file overrides. This means that when building
on systems with deficient header files in `/usr/include/`, it may create
files named `string.h`, `stdlib.h`, `stdint.h` or similar in
the build directory. In the other source directories of your package you
will usually pass ‘`-I`’ options to the compiler, so that these Gnulib
substitutes are visible and take precedence over the files in
`/usr/include/`.

---

Next: [Style of #include statements](#Style-of-_0023include-statements), Previous: [-I options](#g_t_002dI-options), Up: [Changing your sources for use with Gnulib](#Source-changes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 3.5.2 Include <config.h> [¶](#Include-_003cconfig_002eh_003e-1)

These Gnulib substitute header files rely on `<config.h>` being
already included. Furthermore `<config.h>` must be the first include
in every compilation unit. This means that to *all your source files*
and likely also to *all your tests source files* you need to add an
‘`#include <config.h>`’ at the top. Which source files are affected?
Exactly those whose compilation includes a ‘`-I`’ option that refers to
the Gnulib library directory.

This is annoying, but inevitable: On many systems, `<config.h>` is
used to set system dependent flags (such as `_GNU_SOURCE` on GNU systems),
and these flags have no effect after any system header file has been included.

---

Previous: [Include <config.h>](#Include-_003cconfig_002eh_003e), Up: [Changing your sources for use with Gnulib](#Source-changes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 3.5.3 Style of #include statements [¶](#Style-of-_0023include-statements-1)

When including including specific header files, you need to use the
‘`#include <...>`’ syntax, not the ‘`#include "..."`’ syntax.
This is true for the following POSIX or ISO C standardized header files:

* `arpa/inet.h`
* `assert.h`
* `ctype.h`
* `dirent.h`
* `endian.h`
* `errno.h`
* `fcntl.h`
* `fenv.h`
* `float.h`
* `fnmatch.h`
* `glob.h`
* `iconv.h`
* `inttypes.h`
* `langinfo.h`
* `limits.h`
* `locale.h`
* `math.h`
* `monetary.h`
* `net/if.h`
* `netdb.h`
* `netinet/in.h`
* `poll.h`
* `pthread.h`
* `sched.h`
* `search.h`
* `signal.h`
* `spawn.h`
* `stdalign.h`
* `stdarg.h`
* `stdcountof.h`
* `stddef.h`
* `stdint.h`
* `stdio.h`
* `stdlib.h`
* `string.h`
* `strings.h`
* `sys/msg.h`
* `sys/resource.h`
* `sys/select.h`
* `sys/sem.h`
* `sys/shm.h`
* `sys/socket.h`
* `sys/stat.h`
* `sys/time.h`
* `sys/times.h`
* `sys/types.h`
* `sys/uio.h`
* `sys/un.h`
* `sys/utsname.h`
* `sys/wait.h`
* `termios.h`
* `threads.h`
* `time.h`
* `uchar.h`
* `unistd.h`
* `utime.h`
* `wchar.h`
* `wctype.h`

as well as for the following header files that exist in the GNU C library
but are not standardized:

* `alloca.h`
* `byteswap.h`
* `error.h`
* `getopt.h`
* `malloc.h`
* `mntent.h`
* `omp.h`
* `pty.h`
* `selinux/selinux.h`
* `sys/file.h`
* `sys/ioctl.h`
* `sys/random.h`
* `sysexits.h`
* `utmp.h`

The reason for this requirement is that for these header files, the Gnulib
override uses `#include_next` to include the system-provided header
of the same name, and `#include_next` may not work right with the
‘`#include "..."`’ syntax.

---

Next: [Finding recommended ISO C and POSIX function substitutes](#Finding-POSIX-substitutes), Previous: [Changing your sources for use with Gnulib](#Source-changes), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
