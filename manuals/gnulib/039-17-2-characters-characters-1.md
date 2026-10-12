### 17.2 Characters [¶](#Characters-1)

A *character* is the elementary unit that strings are made of.

What is a character? “A character is an element of a character set”
is sort of a circular definition, but it highlights the fact that it is
not merely a number. Although many characters are visually represented
by a single glyph, there are characters that, for example, have a
different glyph when used at the end of a word than when used inside a
word. A character is also not the minimal rendered text processing
unit; that is a grapheme cluster and in general consists of one or more
characters. If you want to know more about the concept of character and
various concepts associated with characters, refer to the Unicode
standard.

For the representation in memory of a character, various types have been
in use, and some of them were failures: `char` and `wchar_t`
were invented for this purpose, but are not the right types.
`char32_t` is the right type (successor of `wchar_t`); and
`mbchar_t` (defined by Gnulib) is an alternative for specific kinds
of processing.

* [The `char` type](#The-char-type)
* [The `wchar_t` type](#The-wchar_005ft-type)
* [The `char32_t` type](#The-char32_005ft-type)
* [The `mbchar_t` type](#The-mbchar_005ft-type)
* [Comparison of character APIs](#Comparison-of-character-APIs)

---

Next: [The `wchar_t` type](#The-wchar_005ft-type), Up: [Characters](#Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.2.1 The `char` type [¶](#The-char-type-1)

The `char` type is in the C language since the beginning in the
1970ies, but – due to its limitation of 256 possible values – is no
longer the adequate type for storing a character.

Technically, it is still adequate in unibyte locales. But since most
locales nowadays are multibyte locales, it makes no sense to write a
program that runs only in unibyte locales.

ISO C and POSIX standardized an API for characters of type `char`,
in `<ctype.h>`. This API is nowadays useless and obsolete, when it
comes to general text processing.

The important lessons to remember are:

|  |
| --- |
| *A ‘`char`’ is just the elementary storage unit for a string, not a character.* |

|  |
| --- |
| *Never use `<ctype.h>`!* |

---

Next: [The `char32_t` type](#The-char32_005ft-type), Previous: [The `char` type](#The-char-type), Up: [Characters](#Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.2.2 The `wchar_t` type [¶](#The-wchar_005ft-type-1)

The ISO C and POSIX standard creators made an attempt to overcome the
dead end regarding the `char` type. They introduced

* a type ‘`wchar_t`’, designed to encapsulate a character,
* a “wide string” type ‘`wchar_t *`’, with some API functions
  declared in [`<wchar.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/wchar.h.html), and
* functions declared in [`<wctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/wctype.h.html) that were meant to supplant
  the ones in [`<ctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html).

Unfortunately, this API and its implementation has numerous problems:

* On Windows platforms and on AIX in 32-bit mode, `wchar_t` is a
  16-bit type. This means that it can never accommodate an entire Unicode
  character. Either the `wchar_t *` strings are limited to
  characters in UCS-2 (the “Basic Multilingual Plane” of Unicode), or
  – if `wchar_t *` strings are encoded in UTF-16 – a
  `wchar_t` represents only half of a character in the worst case,
  making the [`<wctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/wctype.h.html) functions pointless.
* On Solaris and FreeBSD, the `wchar_t` encoding is locale dependent
  and undocumented. This means, if you want to know any property of a
  `wchar_t` character, other than the properties defined by
  [`<wctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/wctype.h.html) – such as whether it’s a dash, currency symbol,
  paragraph separator, or similar –, you have to convert it to
  `char *` encoding first, by use of the function [`wctomb`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/wctomb.html).
* When you read a stream of wide characters, through the functions
  [`fgetwc`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgetwc.html) and [`fgetws`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgetws.html), and when the input
  stream/file is not in the expected encoding, you have no way to
  determine the invalid byte sequence and do some corrective action. If
  you use these functions, your program becomes “garbage in - more
  garbage out” or “garbage in - abort”.

As a consequence, it is better to use multibyte strings. Such multibyte
strings can bypass limitations of the `wchar_t` type, if you use
functions defined in Gnulib and GNU libunistring for text processing.
They can also faithfully transport malformed characters that were
present in the input, without requiring the program to produce garbage
or abort.

---

Next: [The `mbchar_t` type](#The-mbchar_005ft-type), Previous: [The `wchar_t` type](#The-wchar_005ft-type), Up: [Characters](#Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.2.3 The `char32_t` type [¶](#The-char32_005ft-type-1)

The ISO C and POSIX standard creators then introduced the
`char32_t` type. In ISO C 11, it was conceptually a “32-bit wide
character” type. In ISO C 23, its semantics has been further
specified: A `char32_t` value is a Unicode code point.

Thus, the `char32_t` type is not affected the problems that plague
the `wchar_t` type.

The `char32_t` type and its API are defined in the `<uchar.h>`
header file.

ISO C and POSIX specify only the basic functions for the `char32_t`
type, namely conversion of a single character (`mbrtoc32` and
`c32rtomb`). For convenience, Gnulib adds API for classification
and case conversion of characters.

GNU libunistring can also be used on `char32_t` values. Since
`char32_t` is the same as `uint32_t`, all `u32_*`
functions of GNU libunistring are applicable to arrays of
`char32_t` values.

On glibc systems, use of the 32-bit wide strings (`char32_t[]`) is
exactly as efficient as the use of the older wide strings
(`wchar_t[]`). This is possible because on glibc, `wchar_t`
values already always were 32-bit and Unicode code points.
`mbrtoc32` is just an alias of `mbrtowc`. The Gnulib
`*c32*` functions are optimized so that on glibc systems they
immediately redirect to the corresponding `*wc*` functions.

Gnulib implements the ISO C 23 semantics of `char32_t` when you
import the ‘`uchar-h-c23`’ module. Without this module, it implements
only the ISO C 11 semantics; the effect is that on some platforms
(macOS, FreeBSD, NetBSD, Solaris) a `char32_t` value is the same
as a `wchar_t` value, not a Unicode code point. Thus, when you
want to pass `char32_t` values to GNU libunistring or to some Unicode
centric Gnulib functions, you need the ‘`uchar-h-c23`’ module in order
to do so without portability problems.

---

Next: [Comparison of character APIs](#Comparison-of-character-APIs), Previous: [The `char32_t` type](#The-char32_005ft-type), Up: [Characters](#Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.2.4 The `mbchar_t` type [¶](#The-mbchar_005ft-type-1)

Gnulib defines an alternate way to encode a multibyte character:
`mbchar_t`. Its main feature is the ability to process a string or
stream with some malformed characters without reporting an error.

The type `mbchar_t`, defined in `"mbchar.h"`, holds a
character in both the multibyte and the 32-bit wide character
representation. In case of a malformed character only the multibyte
representation is used.

* [Reading multibyte strings](#Reading-multibyte-strings)

---

Up: [The `mbchar_t` type](#The-mbchar_005ft-type)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.2.4.1 Reading multibyte strings [¶](#Reading-multibyte-strings-1)

If you want to process (possibly multibyte) characters while reading
them from a `FILE *` stream, without reading them into a string
first, the `mbfile` module is made for this purpose.

---

Previous: [The `mbchar_t` type](#The-mbchar_005ft-type), Up: [Characters](#Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.2.5 Comparison of character APIs [¶](#Comparison-of-character-APIs-1)

This table summarizes the API functions available for characters, in
POSIX and in Gnulib.

| unibyte character | assume C locale | wide character | 32-bit wide character | mbchar\_t character |
| --- | --- | --- | --- | --- |
| `== '\0'` | `== '\0'` | `== L'\0'` | `== 0` | `mb_isnul` |
| `==` | `==` | `==` | `==` | `mb_equal` |
| `isalnum` | `c_isalnum` | `iswalnum` | `c32isalnum` | `mb_isalnum` |
| `isalpha` | `c_isalpha` | `iswalpha` | `c32isalpha` | `mb_isalpha` |
| `isblank` | `c_isblank` | `iswblank` | `c32isblank` | `mb_isblank` |
| `iscntrl` | `c_iscntrl` | `iswcntrl` | `c32iscntrl` | `mb_iscntrl` |
| `isdigit` | `c_isdigit` | `iswdigit` | `c32isdigit` | `mb_isdigit` |
| `isgraph` | `c_isgraph` | `iswgraph` | `c32isgraph` | `mb_isgraph` |
| `islower` | `c_islower` | `iswlower` | `c32islower` | `mb_islower` |
| `isprint` | `c_isprint` | `iswprint` | `c32isprint` | `mb_isprint` |
| `ispunct` | `c_ispunct` | `iswpunct` | `c32ispunct` | `mb_ispunct` |
| `isspace` | `c_isspace` | `iswspace` | `c32isspace` | `mb_isspace` |
| `isupper` | `c_isupper` | `iswupper` | `c32isupper` | `mb_isupper` |
| `isxdigit` | `c_isxdigit` | `iswxdigit` | `c32isxdigit` | `mb_isxdigit` |
| – | – | `wctype` | `c32_get_type_test` | – |
| – | – | `iswctype` | `c32_apply_type_test` | – |
| `tolower` | `c_tolower` | `towlower` | `c32tolower` | – |
| `toupper` | `c_toupper` | `towupper` | `c32toupper` | – |
| – | – | `wctrans` | `c32_get_mapping` | – |
| – | – | `towctrans` | `c32_apply_mapping` | – |
| – | – | `wcwidth` | `c32width` | `mb_width` |

---

Next: [Regular expressions](#Regular-expressions), Previous: [Strings and Characters](#Strings-and-Characters), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 18 Particular Modules [¶](#Particular-Modules-1)

* [alloca](#alloca)
* [alloca-opt](#alloca_002dopt)
* [Safe Allocation Macros](#Safe-Allocation-Macros)
* [Attributes](#Attributes)
* [Compile-time Assertions](#Compile_002dtime-Assertions)
* [Non-returning Functions](#Non_002dreturning-Functions)
* [Integer Properties](#Integer-Properties)
* [Static inline functions](#static-inline)
* [Extern inline functions](#extern-inline)
* [Handling closed standard file descriptors](#Closed-standard-fds)
* [Handling strings with NUL characters](#Handling-strings-with-NUL-characters)
* [Container data types](#Container-data-types)
* [Modernized printf](#Modernized-printf)
* [Advanced functions on FILE streams](#Advanced-stdio-functions)
* [libunistring](#libunistring)
* [Stack traces](#Stack-traces)
* [Printing –version and bug-reporting information](#Printing-_002d_002dversion-and-bug_002dreporting-information)
* [Recognizing Option Arguments](#Recognizing-Option-Arguments)
* [Quoting](#Quoting)
* [progname and getprogname](#progname-and-getprogname)
* [gcd: greatest common divisor](#gcd)
* [Profiling of program phases](#Profiling-of-program-phases)
* [Library version handling](#Library-version-handling)
* [Supporting Relocation](#Supporting-Relocation)
* [func](#func)
* [stat-size](#stat_002dsize)

---

Next: [alloca-opt](#alloca_002dopt), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.1 alloca [¶](#alloca-1)

The `alloca` module provides for a function `alloca` which allocates
memory on the stack, where the system allows it. A memory block allocated with
`alloca` exists only until the function that calls `alloca` returns
or exits abruptly.

There are a few systems where this is not possible: HP-UX systems, and some
other platforms when the C++ compiler is used. On these platforms the alloca
module provides a `malloc` based emulation. This emulation will not free a
memory block immediately when the calling function returns, but rather will
wait until the next `alloca` call from a function with the same or a
shorter stack length. Thus, in some cases, a few memory blocks will be kept
although they are not needed any more.

The user can `#include <alloca.h>` and use `alloca` on all platforms.
Note that the `#include <alloca.h>` must be the first one after the
autoconf-generated `config.h`, for AIX 3 compatibility. Thanks to IBM for
this nice restriction!

Note that GCC 3.1 and 3.2 can *inline* functions that call `alloca`.
When this happens, the memory blocks allocated with `alloca` will not be
freed until *the end of the calling function*. If this calling function
runs a loop calling the function that uses `alloca`, the program easily
gets a stack overflow and crashes. To protect against this compiler behaviour,
you can mark the function that uses `alloca` with the following attribute:

```
#ifdef __GNUC__
__attribute__ ((__noinline__))
#endif
```

An alternative to this module is the ‘`alloca-opt`’ module.

---

Next: [Safe Allocation Macros](#Safe-Allocation-Macros), Previous: [alloca](#alloca), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.2 alloca-opt [¶](#alloca_002dopt-1)

The `alloca-opt` module provides for a function `alloca` which allocates
memory on the stack, where the system allows it. A memory block allocated with
`alloca` exists only until the function that calls `alloca` returns
or exits abruptly.

There are a few systems where this is not possible: HP-UX systems, and some
other platforms when the C++ compiler is used. On these platforms the
`alloca-opt` module provides no replacement, just a preprocessor macro
HAVE\_ALLOCA.

The user can `#include <alloca.h>` on all platforms, and use
`alloca` on those platforms where the preprocessor macro HAVE\_ALLOCA
evaluates to true. If HAVE\_ALLOCA is false, the code should use a heap-based
memory allocation based on `malloc` or (in C++) `new`. Note that
the `#include <alloca.h>` must be the first one after the
autoconf-generated `config.h`, for AIX 3 compatibility. Thanks to IBM for
this nice restriction!

Note that GCC 3.1 and 3.2 can *inline* functions that call `alloca`.
When this happens, the memory blocks allocated with `alloca` will not be
freed until *the end of the calling function*. If this calling function
runs a loop calling the function that uses `alloca`, the program easily
gets a stack overflow and crashes. To protect against this compiler behaviour,
you can mark the function that uses `alloca` with the following attribute:

```
#ifdef __GNUC__
__attribute__ ((__noinline__))
#endif
```

---

Next: [Attributes](#Attributes), Previous: [alloca-opt](#alloca_002dopt), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.3 Safe Allocation Macros [¶](#Safe-Allocation-Macros-1)

The standard C library malloc/realloc/calloc/free APIs are prone to a
number of common coding errors. The `safe-alloc` module provides
macros that make it easier to avoid many of them. It still uses the
standard C allocation functions behind the scenes.

This module is obsolete, as it does not seem to have caught on in
practice and some of its features could not be ported to unusual
platforms.

Some of the memory allocation mistakes that are commonly made are

* passing the incorrect number of bytes to `malloc`, especially
  when allocating an array,
* unchecked integer overflow when calculating array sizes,
* fail to check the return value of `malloc` and `realloc` for
  errors,
* forget to fully initialize memory just allocated with `malloc`,
* duplicate calls to `free` by forgetting to set the pointer
  variable to `NULL`,
* leaking memory in calls to `realloc` when that call fails.

The `safe-alloc` module addresses these problems in the following way:

* It defines macros that wrap around the standard C allocation
  functions. That makes it possible to use the compiler’s knowledge of
  the size of objects for allocation; it also allows setting pointers
  passed in as arguments when appropriate.
* It uses return values only for a success/failure error condition flag,
  and annotates them with GCC’s `__warn_unused_result__` attribute.
* When allocating a fresh array, it uses `calloc` instead of
  `malloc` so that the array’s contents are zeroed.
  However, memory added to an already-existing array is uninitialized.

Macro: **int** ALLOC (ptr) [¶](#index-int)
:   Allocate `sizeof *ptr` bytes of memory and store the address of
    allocated memory in `ptr`. Fill the newly allocated memory with
    zeros.

    Returns −1 on failure, 0 on success.

Macro: **int** ALLOC\_N (ptr, count) [¶](#index-int-1)
:   Allocate an array of `count` elements, each `sizeof *ptr`
    bytes long, and store the address of allocated memory in
    `ptr`. Fill the newly allocated memory with zeros.

    Returns −1 on failure, 0 on success.

Macro: **int** ALLOC\_N\_UNINITIALIZED (ptr, count) [¶](#index-int-2)
:   Allocate an array of `count` elements, each `sizeof *ptr`
    bytes long, and store the address of allocated memory in
    `ptr`. The allocated memory is not initialized.

    Returns −1 on failure, 0 on success.

Macro: **void** FREE (ptr) [¶](#index-void)
:   Free the memory stored in `ptr` and set `ptr` to
    `NULL`.

---

Next: [Compile-time Assertions](#Compile_002dtime-Assertions), Previous: [Safe Allocation Macros](#Safe-Allocation-Macros), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.4 Attributes [¶](#Attributes-1)

The module ‘`attribute`’ provides a header file `attribute.h` that
defines macros related to C and C++ attributes and the GCC
`__attribute__` keyword.

Here is an example of its use:

```
#include <attribute.h>

NODISCARD
extern char *crypt (char const *, char const *)
  ATTRIBUTE_NOTHROW ATTRIBUTE_LEAF ATTRIBUTE_NONNULL ((1, 2));
```

`NODISCARD` expands to `[[nodiscard]]` if the compiler
supports this C23 syntax, otherwise to
`__attribute__ ((__warn_unused_result__))` if the compiler
is a recent-enough GCC or GCC-like compiler, otherwise to nothing.
`ATTRIBUTE_NOTHROW` expands to `__attribute__
((__nothrow__))` if the compiler is a recent-enough GCC or GCC-like
compiler, and to nothing otherwise. Similarly for
`ATTRIBUTE_LEAF`. `ATTRIBUTE_NONNULL ((1, 2))` expands to
`__attribute__ ((__nonnull__ (1, 2)))` if the compiler is
recent-enough GCC, and to nothing otherwise.

Most of these attribute names begin with `ATTRIBUTE_`.
A few do not, because they are part of C23 and their
names are not likely to clash with other macro names.
These macros are `DEPRECATED`, `FALLTHROUGH`,
`MAYBE_UNUSED`, and `NODISCARD`, which can
be defined to `[[deprecated]]` etc. on C23 platforms.
Also, these exceptional macros should be placed at the start of
function declarations, whereas the `ATTRIBUTE_*` macros can be
placed at the end.

The module also defines the macro `UNNAMED`,
designed for function parameters that are never used,
whereas the `MAYBE_UNUSED` attribute is better suited for parameters
that might or might not be unused depending on preprocessor macro settings.
For example:

```
int
f (int a, MAYBE_UNUSED int b, int UNNAMED (c))
{
  int r = a;
#ifdef B_FEATURE
  r |= b;
#endif
  return r;
}
```

---

Next: [Non-returning Functions](#Non_002dreturning-Functions), Previous: [Attributes](#Attributes), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.5 Compile-time Assertions [¶](#Compile_002dtime-Assertions-1)

The module ‘`verify`’ provides a header file `verify.h` that defines
macros related to compile-time verification.

Two of these macros are `verify (V)` and `verify_expr
(V, EXPR)`. Both accept an integer constant expression
argument V and verify that it is nonzero. If not, a compile-time error
results.

These two macros implement compile-time tests, as opposed to
the standard `assert` macro which supports only runtime tests.
Since the tests occur at compile-time, they are more reliable, and
they require no runtime overhead.

`verify (V);` is a declaration; it can occur outside of
functions. In contrast, `verify_expr (V, EXPR)` is
an expression that returns the value of EXPR; it can be used in
macros that expand to expressions. If EXPR is an integer
constant expression, then `verify_expr (V, EXPR)` is
also an integer constant expression. Although EXPR and
`verify_expr (V, EXPR)` are guaranteed to have the
same side effects and value and type (after integer promotion), they
need not have the same type if EXPR’s type is an integer that is
narrower than `int` or `unsigned int`.

V should be an integer constant expression in the sense
of the C standard. Its leaf operands should be integer, enumeration,
or character constants; or `sizeof` expressions that return
constants; or floating constants that are the immediate operands of
casts. Outside a `sizeof` subexpression, V should
not contain any assignments, function calls, comma operators, casts to
non-integer types, or subexpressions whose values are outside the
representable ranges for their types. If V is not an
integer constant expression, then a compiler might reject a usage like
‘`verify (V);`’ even when V is
nonzero.

Although the standard `assert` macro is a runtime test, C23 and C++17
specify a builtin `static_assert (V)`, which differs
from `verify` in two major ways. First, it can also be used
within a `struct` or `union` specifier, in place of an
ordinary member declaration. Second, it allows the programmer to
specify, as an optional second argument, a compile-time diagnostic as
a string literal. If your program is not intended to be portable to
compilers that lack C23 or C++17 `static_assert`, the only
advantage of `verify` is that its name is a bit shorter.

The `verify.h` header defines one more macro, `assume
(E)`, which expands to an expression of type `void`
that causes the compiler to assume that E yields a nonzero
value. E should be a scalar expression, and should not
have side effects; it may or may not be evaluated. The behavior is
undefined if E would yield zero. The main use of `assume`
is optimization, as the compiler may be able to generate better code
if it assumes E. For best results, E should be simple
enough that a compiler can determine that it has no side effects: if
E calls an external function or accesses volatile storage the
compiler may not be able to optimize E away and `assume
(E)` may therefore slow down the program.

Here are some example uses of these macros.

```
#include <verify.h>

#include <limits.h>
#include <time.h>
```

```

```

```
/* Verify that time_t is an integer type.  */
verify ((time_t) 1.5 == 1);
```

```

```

```
/* Verify that time_t is no smaller than int.  */
verify (sizeof (int) <= sizeof (time_t));
```

```

```

```
/* Verify that time_t is signed.  */
verify ((time_t) -1 < 0);
```

```

```

```
/* Verify that time_t uses two's complement representation.  */
verify (~ (time_t) -1 == 0);
```

```

```

```
/* Return the maximum value of the integer type T,
   verifying that T is an unsigned integer type.
   The cast to (T) is outside the call to verify_expr
   so that the result is of type T
   even when T is narrower than unsigned int.  */
#define MAX_UNSIGNED_VAL(t) \
   ((t) verify_expr (0 < (t) -1, -1))
```

```

```

```
/* Return T divided by CHAR_MAX + 1, where behavior is
   undefined if T < 0.  In the common case where CHAR_MAX
   is 127 the compiler can therefore implement the division
   by shifting T right 7 bits, an optimization that would
   not be valid if T were negative.  */
time_t
time_index (time_t t)
{
  assume (0 <= t);
  return t / (CHAR_MAX + 1);
}
```

---

Next: [Integer Properties](#Integer-Properties), Previous: [Compile-time Assertions](#Compile_002dtime-Assertions), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.6 Non-returning Functions [¶](#Non_002dreturning-Functions-1)

A "non-returning" function is a function which cannot return normally.
Instead of returning, it can loop forever, or it can transfer control via
`abort`, `execvp`, `exit`, `longjmp`, `throw`
(in C++), or similar mechanisms. Non-returning functions are
declared with a `void` return type.

It helps the compiler’s ability to emit sensible warnings, following
data-flow analysis, to declare which functions are non-returning.
It can also help generate more-efficient code, as there is no need
to save a return address when calling a non-returning function.

Gnulib has multiple ways to support such a declaration:

* The `_Noreturn` keyword. No modules are needed, as Gnulib
  arranges for `<config.h>` to define `_Noreturn` to an
  appropriate replacement on platforms lacking it.
  Unfortunately, although this approach works for all current C versions,
  the `_Noreturn` keyword is obsolescent in C23.
* The ‘`noreturn`’ module. It provides a way to put this declaration
  at function declarations, at function definitions, and in function
  pointer types. The identifiers to use are:
  + `_GL_NORETURN_FUNC` for use in function declarations and function
    definitions.
  + `_GL_NORETURN_FUNCPTR` for use on function pointers.

  The include file is `<noreturn.h>`.

Which of the approaches to use? If the non-returning functions you
have to declare are unlikely to be accessed through function pointers,
you should use `_Noreturn`; otherwise the module
`noreturn` provides for better data-flow analysis and thus for
better warnings.

There is also an obsolete `stdnoreturn-h` module, but its use is no
longer recommended.

---

Next: [Static inline functions](#static-inline), Previous: [Non-returning Functions](#Non_002dreturning-Functions), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.7 Integer Properties [¶](#Integer-Properties-1)

The `intprops` module consists of an include file `<intprops.h>`
that defines several macros useful for testing properties of integer
types.

Integer overflow is a common source of problems in programs written in
C and other languages. In some cases, such as signed integer
arithmetic in C programs, the resulting behavior is undefined, and
practical platforms do not always behave as if integers wrap around
reliably. In other cases, such as unsigned integer arithmetic in C,
the resulting behavior is well-defined, but programs may still
misbehave badly after overflow occurs.

Many techniques have been proposed to attack these problems. These
include precondition testing, wraparound behavior where signed integer
arithmetic is guaranteed to be modular, saturation semantics where
overflow reliably yields an extreme value, undefined behavior
sanitizers where overflow is guaranteed to trap, and various static
analysis techniques.

Gnulib supports wraparound arithmetic and precondition testing, as
these are relatively easy to support portably and efficiently. There
are two families of precondition tests: the first, for integer types,
is easier to use, while the second, for integer ranges, has a simple
and straightforward portable implementation.

Like other Gnulib modules, the implementation of the `intprops`
module assumes that integers use a two’s complement representation,
but it does not assume that signed integer arithmetic wraps around.
See [Other portability assumptions made by Gnulib](#Other-portability-assumptions).

* [Arithmetic Type Properties](#Arithmetic-Type-Properties)
* [Arithmetic Type Conversion](#Arithmetic-Type-Conversion)
* [Integer Bounds](#Integer-Bounds)
* [Checking Integer Overflow](#Checking-Integer-Overflow)
* [Wraparound Arithmetic with Integers](#Wraparound-Arithmetic)
* [Integer Type Overflow](#Integer-Type-Overflow)
* [Integer Range Overflow](#Integer-Range-Overflow)

---

Next: [Arithmetic Type Conversion](#Arithmetic-Type-Conversion), Up: [Integer Properties](#Integer-Properties)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.7.1 Arithmetic Type Properties [¶](#Arithmetic-Type-Properties-1)

`TYPE_IS_INTEGER (t)` is an arithmetic constant
expression that yields 1 if the arithmetic type t is an integer type,
0 otherwise.
`bool` counts as an integer type.

`TYPE_SIGNED (t)` is an arithmetic constant expression
that yields 1 if the real type t is a signed integer type or a
floating type, 0 otherwise.
If t is an integer type, `TYPE_SIGNED (t)`
is an integer constant expression.

`EXPR_SIGNED (e)` yields 1 if the real expression e
has a signed integer type or a floating type, 0 otherwise. If e is an
integer constant expression or an arithmetic constant expression,
`EXPR_SIGNED (e)` is likewise. The expression
e is not evaluated, and `EXPR_SIGNED
(e)` is typically optimized to a constant.

Example usage:

```
#include <intprops.h>
#include <sys/types.h>

enum
{
  clock_t_is_integer = TYPE_IS_INTEGER (clock_t),
  uid_t_is_signed = TYPE_SIGNED (uid_t)
};

int
CLOCKS_PER_SEC_is_signed (void)
{
  return EXPR_SIGNED (CLOCKS_PER_SEC);
}
```

---

Next: [Integer Bounds](#Integer-Bounds), Previous: [Arithmetic Type Properties](#Arithmetic-Type-Properties), Up: [Integer Properties](#Integer-Properties)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.7.2 Arithmetic Type Conversion [¶](#Arithmetic-Type-Conversion-1)

Here are some ways in C to convert an arithmetic expression e to
a possibly different arithmetic type t.

* An explicit conversion like `((t) e)` is powerful and
  can therefore be dangerous, because the conversion can succeed even if
  neither e nor t happens to have an arithmetic type. For
  example, if e is a pointer, `((long int) e)` will do
  a conversion even if that was not the intent.
* An implicit conversion like `t v = e;` is less
  powerful, as it does not convert from pointers. However, it can lose
  information and sign, as in for example `int v = -0.9;` which
  sets `v` to zero.
* A no-op arithmetic expression like `+e` is even less
  powerful, as it preserves value (including sign) because it does only
  integer promotions. That is, it converts to `int` if that can
  represent all values of `e`’s underlying type, otherwise to
  `unsigned int` if that can represent all values, and
  otherwise it does no conversion.

`INT_PROMOTE (e)` is an expression with the same value as
the arithmetic expression e but with e’s type after
any integer promotion. It behaves like `+e`.

In the following example, using `INT_PROMOTE` pacifies GCC’s
`-Wswitch-enum` option, and may help human readers see what is
going on even if they are not expert in C’s integer promotion rules
and might be confused by the simpler `switch (+v)`.

```
enum { A = 1, B, C, D, E } v = ...;
switch (INT_PROMOTE (v))
  {
    case A: case C:
      return true;
    default:
      /* Handle all other cases,
         even cases like v == 0.  */
      return false;
  }
```

---

Next: [Checking Integer Overflow](#Checking-Integer-Overflow), Previous: [Arithmetic Type Conversion](#Arithmetic-Type-Conversion), Up: [Integer Properties](#Integer-Properties)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.7.3 Integer Bounds [¶](#Integer-Bounds-1)

`INT_BUFSIZE_BOUND (t)` is an integer constant
expression that is a bound on the size of the string representing an
integer type or expression t in decimal notation, including the
terminating null character and any leading `-` character. For
example, if `INT_BUFSIZE_BOUND (int)` is 12, any value of type
`int` can be represented in 12 bytes or less, including the
terminating null. The bound is not necessarily tight.

Example usage:

```
#include <intprops.h>
#include <stdio.h>
int
int_strlen (int i)
{
  char buf[INT_BUFSIZE_BOUND (int)];
  return sprintf (buf, "%d", i);
}
```

`INT_STRLEN_BOUND (t)` is an integer constant
expression that is a bound on the length of the string representing an
integer type or expression t in decimal notation, including any
leading `-` character. This is one less than
`INT_BUFSIZE_BOUND (t)`.

`TYPE_MINIMUM (t)` and `TYPE_MAXIMUM (t)` are
integer constant expressions equal to the minimum and maximum
values of the integer type t. These expressions are of the type
t.

Example usage:

```
#include <sys/types.h>
#include <intprops.h>
bool
in_off_t_range (long long int a)
{
  return TYPE_MINIMUM (off_t) <= a && a <= TYPE_MAXIMUM (off_t);
}
```

---

Next: [Wraparound Arithmetic with Integers](#Wraparound-Arithmetic), Previous: [Integer Bounds](#Integer-Bounds), Up: [Integer Properties](#Integer-Properties)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.7.4 Checking Integer Overflow [¶](#Checking-Integer-Overflow-1)

Signed integer arithmetic has undefined behavior on overflow in C.
Although almost all modern computers use two’s complement signed
arithmetic that is well-defined to wrap around, C compilers routinely
optimize assuming that signed integer overflow cannot occur, which
means that a C program cannot easily get at the underlying machine
arithmetic. For example:

```
if ((a + b < b) == (a < 0))
  a += b;
else
  printf ("overflow\n");
```

might not work as expected if `a` and `b` are signed,
because a compiler can assume that signed overflow cannot occur and
treat the entire `if` expression as if it were true. And even if
`a` is unsigned, the expression might not work as expected if
`b` is negative or is wider than `a`.

The following macros work around this problem by yielding an overflow
indication while computing the sum, difference, or product of two
integers. For example, if `i` is of type `int`,
`INT_ADD_OK (INT_MAX - 1, 1, &i)` sets `i` to
`INT_MAX` and yields 1, whereas `INT_ADD_OK (INT_MAX, 1,
&i)` yields 0.

Example usage:

```
#include <intprops.h>
#include <stdio.h>

/* Compute A * B, reporting whether overflow occurred.  */
void
print_product (long int a, long int b)
{
  long int r;
  if (INT_MULTIPLY_OK (a, b, &r))
    printf ("result is %ld\n", r);
  else
    printf ("overflow\n");
}
```

These macros work for both signed and unsigned integers, so they can
be used with integer types like `time_t` that may or may not be
signed, depending on the platform.

These macros have the following restrictions:

* Their first two arguments must be integer expressions.
* Their last argument must be a non-null pointer to an integer.
* They may evaluate their arguments zero or multiple times, so the
  arguments should not have side effects.
* They are not necessarily constant expressions, even if all their
  arguments are constant expressions.

`INT_ADD_OK (a, b, r)` [¶](#index-INT_005fADD_005fOK)
:   Compute the sum of a and b. If it fits into
    `*r`, store it there and yield 1. Otherwise yield
    0, possibly modifying `*r` to an unspecified value.
    See above for restrictions.

`INT_SUBTRACT_OK (a, b, r)` [¶](#index-INT_005fSUBTRACT_005fOK)
:   Compute the difference between a and b. If it fits into
    `*r`, store it there and yield 1. Otherwise yield
    0, possibly modifying `*r` to an unspecified value.
    See above for restrictions.

`INT_MULTIPLY_OK (a, b, r)` [¶](#index-INT_005fMULTIPLY_005fOK)
:   Compute the product of a and b. If it fits into
    `*r`, store it there and yield 1. Otherwise yield
    0, possibly modifying `*r` to an unspecified value.
    See above for restrictions.

Other macros are available if you need wrapped-around results when
overflow occurs (see [Wraparound Arithmetic with Integers](#Wraparound-Arithmetic)), or if you need to
check for overflow in operations other than addition, subtraction, and
multiplication (see [Integer Type Overflow](#Integer-Type-Overflow)).

---

Next: [Integer Type Overflow](#Integer-Type-Overflow), Previous: [Checking Integer Overflow](#Checking-Integer-Overflow), Up: [Integer Properties](#Integer-Properties)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.7.5 Wraparound Arithmetic with Integers [¶](#Wraparound-Arithmetic-with-Integers)

Signed integer arithmetic has undefined behavior on overflow in C.
Although almost all modern computers use two’s complement signed
arithmetic that is well-defined to wrap around, C compilers routinely
optimize assuming that signed integer overflow cannot occur, which
means that a C program cannot easily get at the underlying machine
arithmetic. For example, on a typical machine with 32-bit two’s
complement `int` the expression `INT_MAX + 1` does not
necessarily yield `INT_MIN`, because the compiler may do
calculations with a 64-bit register, or may generate code that
traps on signed integer overflow.

The following macros work around this problem by storing the
wraparound value, i.e., the low-order bits of the correct answer, and
by yielding an overflow indication. For example, if `i` is of
type `int`, `INT_ADD_WRAPV (INT_MAX, 1, &i)` sets `i`
to `INT_MIN` and yields 1 on a two’s complement machine.
See [Integer Type Overflow](#Integer-Type-Overflow).

Example usage:

```
#include <intprops.h>
#include <stdio.h>

/* Print the low order bits of A * B,
   reporting whether overflow occurred.  */
void
print_product (long int a, long int b)
{
  long int r;
  int overflow = INT_MULTIPLY_WRAPV (a, b, &r);
  printf ("result is %ld (%s)\n", r,
          (overflow
           ? "after overflow"
           : "no overflow"));
}
```

These macros work for both signed and unsigned integers, so they can
be used with integer types like `time_t` that may or may not be
signed, depending on the platform.

These macros have the following restrictions:

* Their first two arguments must be integer expressions.
* Their last argument must be a non-null pointer to an integer.
* They may evaluate their arguments zero or multiple times, so the
  arguments should not have side effects.
* They are not necessarily constant expressions, even if all their
  arguments are constant expressions.

`INT_ADD_WRAPV (a, b, r)` [¶](#index-INT_005fADD_005fWRAPV)
:   Store the low-order bits of the sum of a and b into
    `*r`. Yield 1 if overflow occurred, 0 if the
    low-order bits are the mathematically-correct sum. See above for
    restrictions.

`INT_SUBTRACT_WRAPV (a, b, r)` [¶](#index-INT_005fSUBTRACT_005fWRAPV)
:   Store the low-order bits of the difference between a and b
    into `*r`. Yield 1 if overflow occurred, 0 if the
    low-order bits are the mathematically-correct difference. See above
    for restrictions.

`INT_MULTIPLY_WRAPV (a, b, r)` [¶](#index-INT_005fMULTIPLY_005fWRAPV)
:   Store the low-order bits of the product of a and b into
    `*r`. Yield 1 if overflow occurred, 0 if the
    low-order bits are the mathematically-correct product. See above for
    restrictions.

If your code includes `<intprops.h>` only for these `_WRAPV`
macros, you may prefer to use Gnulib’s `stdckdint-h` module
instead, as it supports similar macros that were standardized in C23
and are therefore independent of Gnulib if your code can assume C23 or
later. See [`stdckdint.h`](#stdckdint_002eh).

Other macros are available if you do not need wrapped-around results
when overflow occurs (see [Checking Integer Overflow](#Checking-Integer-Overflow)), or if you
need to check for overflow in operations other than addition,
subtraction, and multiplication (see [Integer Type Overflow](#Integer-Type-Overflow)).

---

Next: [Integer Range Overflow](#Integer-Range-Overflow), Previous: [Wraparound Arithmetic with Integers](#Wraparound-Arithmetic), Up: [Integer Properties](#Integer-Properties)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.7.6 Integer Type Overflow [¶](#Integer-Type-Overflow-1)

Although unsigned integer arithmetic wraps around modulo a power of
two, signed integer arithmetic has undefined behavior on overflow in
C. Almost all modern computers use two’s complement signed
arithmetic that is well-defined to wrap around, but C compilers
routinely optimize based on the assumption that signed integer
overflow cannot occur, which means that a C program cannot easily get
at the underlying machine behavior. For example, the signed integer
expression `(a + b < b) != (a < 0)` is not a reliable test for
whether `a + b` overflows, because a compiler can assume that
signed overflow cannot occur and treat the entire expression as if it
were false.

These macros yield 1 if the corresponding C operators overflow, 0 otherwise.
They work correctly on all known practical hosts, and do not
rely on undefined behavior due to signed arithmetic overflow. They
are integer constant expressions if their arguments are. They
are typically easier to use than the integer range overflow macros
(see [Integer Range Overflow](#Integer-Range-Overflow)), and they support more operations and
evaluation contexts than the integer overflow checking macros
(see [Checking Integer Overflow](#Checking-Integer-Overflow)) or the wraparound macros
(see [Wraparound Arithmetic with Integers](#Wraparound-Arithmetic)).

These macros can be tricky to use with arguments narrower than
`int`. For example, in the common case with 16-bit `short
int` and 32-bit `int`, if `a` and `b` are of type
`short int` then `INT_MULTIPLY_OVERFLOW (a, b)` always
yields 0, as `a * b` cannot overflow due to C’s rule that
`a` and `b` are widened to `int` before multiplying.
For this reason, often it is better to use the integer overflow
checking macros (see [Checking Integer Overflow](#Checking-Integer-Overflow)) or the wraparound
macros (see [Wraparound Arithmetic with Integers](#Wraparound-Arithmetic)) when checking for overflow in
addition, subtraction, or multiplication.

Example usage:

```
#include <intprops.h>
#include <limits.h>
#include <stdio.h>

/* Print A * B if in range, an overflow
   indicator otherwise.  */
void
print_product (long int a, long int b)
{
  if (INT_MULTIPLY_OVERFLOW (a, b))
    printf ("multiply would overflow");
  else
    printf ("product is %ld", a * b);
}

/* Does the product of two ints always fit
   in a long int?  */
enum {
  INT_PRODUCTS_FIT_IN_LONG
    = ! (INT_MULTIPLY_OVERFLOW
         ((long int) INT_MIN, INT_MIN))
};
```

These macros have the following restrictions:

* Their arguments must be integer expressions.
* They may evaluate their arguments zero or multiple times, so the
  arguments should not have side effects.

These macros are tuned for their last argument being a constant.

`INT_ADD_OVERFLOW (a, b)` [¶](#index-INT_005fADD_005fOVERFLOW)
:   Yield 1 if `a + b` would overflow, 0 otherwise. See above for
    restrictions.

`INT_SUBTRACT_OVERFLOW (a, b)` [¶](#index-INT_005fSUBTRACT_005fOVERFLOW)
:   Yield 1 if `a - b` would overflow, 0 otherwise. See above for
    restrictions.

`INT_NEGATE_OVERFLOW (a)` [¶](#index-INT_005fNEGATE_005fOVERFLOW)
:   Yields 1 if `-a` would overflow, 0 otherwise.
    See above for restrictions.

`INT_MULTIPLY_OVERFLOW (a, b)` [¶](#index-INT_005fMULTIPLY_005fOVERFLOW)
:   Yield 1 if `a * b` would overflow, 0 otherwise. See above for
    restrictions.

`INT_DIVIDE_OVERFLOW (a, b)` [¶](#index-INT_005fDIVIDE_005fOVERFLOW)
:   Yield 1 if `a / b` would overflow, 0 otherwise. See above for
    restrictions. Division overflow can happen on two’s complement hosts
    when dividing the most negative integer by −1. This macro does
    not check for division by zero.

`INT_REMAINDER_OVERFLOW (a, b)` [¶](#index-INT_005fREMAINDER_005fOVERFLOW)
:   Yield 1 if `a % b` would overflow, 0 otherwise. See above for
    restrictions. Remainder overflow can happen on two’s complement hosts
    when dividing the most negative integer by −1; although the
    mathematical result is always 0, in practice some implementations
    trap, so this counts as an overflow. This macro does not check for
    division by zero.

`INT_LEFT_SHIFT_OVERFLOW (a, b)` [¶](#index-INT_005fLEFT_005fSHIFT_005fOVERFLOW)
:   Yield 1 if `a << b` would overflow, 0 otherwise. See above for
    restrictions. The C standard says that behavior is undefined for
    shifts unless 0≤b<w where w is a’s word
    width, and that when a is negative then `a <<
    b` has undefined behavior, but this macro does not check these
    other restrictions.

---

Previous: [Integer Type Overflow](#Integer-Type-Overflow), Up: [Integer Properties](#Integer-Properties)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.7.7 Integer Range Overflow [¶](#Integer-Range-Overflow-1)

These macros yield 1 if the corresponding C operators might not yield
numerically correct answers due to arithmetic overflow, and 0 if if
the operators do not overflow. They do not
rely on undefined or implementation-defined behavior. They are
integer constant expressions if their arguments are. Their
implementations are simple and straightforward, but they are typically
harder to use than the integer type overflow macros. See [Integer Type Overflow](#Integer-Type-Overflow).

Although the implementation of these macros is similar to that
suggested in the SEI CERT C Secure Coding Standard,
in its two sections
“[INT30-C. Ensure that unsigned integer operations do not wrap](https://www.securecoding.cert.org/confluence/display/c/INT30-C.+Ensure+that+unsigned+integer+operations+do+not+wrap)” and
“[INT32-C. Ensure that operations on signed integers do not result in
overflow](https://www.securecoding.cert.org/confluence/display/c/INT32-C.+Ensure+that+operations+on+signed+integers+do+not+result+in+overflow)”, Gnulib’s implementation was derived independently of
CERT’s suggestions.

Example usage:

```
#include <intprops.h>
#include <limits.h>
#include <stdio.h>

void
print_product (long int a, long int b)
{
  if (INT_MULTIPLY_RANGE_OVERFLOW (a, b, LONG_MIN, LONG_MAX))
    printf ("multiply would overflow");
  else
    printf ("product is %ld", a * b);
}

/* Does the product of two ints always fit
   in a long int?  */
enum {
  INT_PRODUCTS_FIT_IN_LONG
    = ! (INT_MULTIPLY_RANGE_OVERFLOW
         ((long int) INT_MIN, (long int) INT_MIN,
          LONG_MIN, LONG_MAX))
};
```

These macros have the following restrictions:

* Their arguments must be integer expressions.
* They may evaluate their arguments zero or multiple times, so
  the arguments should not have side effects.
* The arithmetic arguments (including the min and max
  arguments) must be of the same integer type after the usual arithmetic
  conversions, and the type must have minimum value min and
  maximum max. Unsigned values should use a zero min of the
  proper type, for example, `(unsigned int) 0`.

These macros are tuned for constant min and max. For
commutative operations such as `a + b`, they are also
tuned for constant b.

`INT_ADD_RANGE_OVERFLOW (a, b, min, max)` [¶](#index-INT_005fADD_005fRANGE_005fOVERFLOW)
:   Yield 1 if `a + b` would overflow in
    [min,max] integer arithmetic, 0 otherwise.
    See above for restrictions.

`INT_SUBTRACT_RANGE_OVERFLOW (a, b, min, max)` [¶](#index-INT_005fSUBTRACT_005fRANGE_005fOVERFLOW)
:   Yield 1 if `a - b` would overflow in
    [min,max] integer arithmetic, 0 otherwise.
    See above for restrictions.

`INT_NEGATE_RANGE_OVERFLOW (a, min, max)` [¶](#index-INT_005fNEGATE_005fRANGE_005fOVERFLOW)
:   Yield 1 if `-a` would overflow in [min,max]
    integer arithmetic, 0 otherwise. See above for restrictions.

`INT_MULTIPLY_RANGE_OVERFLOW (a, b, min, max)` [¶](#index-INT_005fMULTIPLY_005fRANGE_005fOVERFLOW)
:   Yield 1 if `a * b` would overflow in
    [min,max] integer arithmetic, 0 otherwise.
    See above for restrictions.

`INT_DIVIDE_RANGE_OVERFLOW (a, b, min, max)` [¶](#index-INT_005fDIVIDE_005fRANGE_005fOVERFLOW)
:   Yield 1 if `a / b` would overflow in
    [min,max] integer arithmetic, 0 otherwise.
    See above for restrictions.
    Division overflow can happen on two’s complement hosts when dividing
    the most negative integer by −1. This macro does not check for
    division by zero.

`INT_REMAINDER_RANGE_OVERFLOW (a, b, min, max)` [¶](#index-INT_005fREMAINDER_005fRANGE_005fOVERFLOW)
:   Yield 1 if `a % b` would overflow in
    [min,max] integer arithmetic, 0 otherwise.
    See above for restrictions.
    Remainder overflow can happen on two’s complement hosts when dividing
    the most negative integer by −1; although the mathematical
    result is always 0, in practice some implementations trap, so this
    counts as an overflow. This macro does not check for division by
    zero.

`INT_LEFT_SHIFT_RANGE_OVERFLOW (a, b, min, max)` [¶](#index-INT_005fLEFT_005fSHIFT_005fRANGE_005fOVERFLOW)
:   Yield 1 if `a << b` would overflow in
    [min,max] integer arithmetic, 0 otherwise.
    See above for restrictions.
    Here, min and max are for a only, and b need
    not be of the same type as the other arguments. The C standard says
    that behavior is undefined for shifts unless 0≤b<w
    where w is a’s word width, and that when a is negative
    then `a << b` has undefined behavior, but this macro
    does not check these other restrictions.

---

Next: [Extern inline functions](#extern-inline), Previous: [Integer Properties](#Integer-Properties), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.8 Static inline functions [¶](#Static-inline-functions)

In order to mark functions as `static inline` in `.c` files,
the only prerequisite you need is an `AC_REQUIRE([AC_C_INLINE])`.
No Gnulib module is needed.

When marking functions as `static inline` in `.h` files,
on the other hand,
it is useful to avoid the following problem:
When gcc or clang is not optimizing, it generates the same object code
for every `static inline` function
that is actually used in the current compilation unit,
thus bloating the size of the resulting binary.
(Code of `static inline` functions
that are unused in the current compilation unit gets dropped;
that is not an issue.)
To avoid this problem, use the Gnulib module ‘`inline`’
and make the `static inline` function definition conditional
on the value of the C macro `HAVE_INLINE`.

---

Next: [Handling closed standard file descriptors](#Closed-standard-fds), Previous: [Static inline functions](#static-inline), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.9 Extern inline functions [¶](#Extern-inline-functions)

The `extern-inline` module supports the use of C99-style
`extern inline` functions so that the code still runs on
compilers that do not support this feature correctly.

C code ordinarily should not use `inline`. Typically it is
better to let the compiler figure out whether to inline, as compilers
are pretty good about optimization nowadays. In this sense,
`inline` is like `register`, another keyword that is
typically no longer needed.

Functions defined (not merely declared) in headers are an exception,
as avoiding `inline` would commonly cause problems for these
functions. Suppose `aaa.h` defines the function `aaa_fun`,
and `aaa.c`, `bbb.c` and `ccc.c` all include
`aaa.h`. If code is intended to portable to non-C99 compilers,
`aaa_fun` cannot be declared with the C99 `inline` keyword.
This problem cannot be worked around by making `aaa_fun` an
ordinary function, as it would be defined three times with external
linkage and the definitions would clash. Although `aaa_fun`
could be a static function, with separate compilation if
`aaa_fun` is not inlined its code will appear in the executable
three times.

To avoid this code bloat, `aaa.h` can do this:

```
/* aaa.h */
/* #include any other headers here */
#ifndef _GL_INLINE_HEADER_BEGIN
 #error "Please include config.h first."
#endif
_GL_INLINE_HEADER_BEGIN
#ifndef AAA_INLINE
# define AAA_INLINE _GL_INLINE
#endif
...
AAA_INLINE int
aaa_fun (int i)
{
  return i + 1;
}
...
_GL_INLINE_HEADER_END
```

and `aaa.c` can do this:

```
/* aaa.c */
#define AAA_INLINE _GL_EXTERN_INLINE
#include <config.h>
#include <aaa.h>
```

whereas `bbb.c` and `ccc.c` can include `aaa.h` in the
usual way. C99 compilers expand `AAA_INLINE` to C99-style
`inline` usage, where `aaa_fun` is declared `extern
inline` in `aaa.c` and plain `inline` in other modules.
Non-C99 compilers that are compatible with GCC use GCC-specific syntax
to accomplish the same ends. Other non-C99 compilers use `static
inline` so they suffer from code bloat, but they are not mainline
platforms and will die out eventually.

In this coding idiom,
the compilation unit should define `AAA_INLINE` before
including the `aaa.h` header that conditionally defines it.
In the unusual and not-recommended case where `config.h`
itself includes `aaa.h`, the compilation unit should
define `AAA_INLINE` before including `config.h`,
not merely before including `aaa.h`. Also,
you need one `AAA_INLINE`-like macro per compilation unit,
not one per header file.
In other words, if the header file `aaa.h` defines functions
defined in `aaa-foo.c` and `aaa-bar.c`,
you need different macros `AAA_FOO_INLINE` and `AAA_BAR_INLINE`.
Use `AAA_FOO_INLINE` for the functions defined in `aaa-foo.c`,
and use `AAA_BAR_INLINE` for the functions defined in `aaa-bar.c`.

Functions defined in this way should not refer to identifiers with
static linkage, as C99 through C23 prohibit this.
Although a future C standard will likely relax this restriction,
it is still an issue on many practical platforms.

`_GL_INLINE` is a portable alternative to C99 plain `inline`.

`_GL_EXTERN_INLINE` is a portable alternative to C99 `extern inline`.

Invoke `_GL_INLINE_HEADER_BEGIN` before all uses of
`_GL_INLINE` in an include file. This suppresses some
bogus warnings in GCC versions before 5.1. If an include file includes
other files, it is better to invoke this macro after including the
other files.

Invoke `_GL_INLINE_HEADER_END` after all uses of
`_GL_INLINE` in an include file.

---

Next: [Handling strings with NUL characters](#Handling-strings-with-NUL-characters), Previous: [Extern inline functions](#extern-inline), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.10 Handling closed standard file descriptors [¶](#Handling-closed-standard-file-descriptors)

Usually, when a program gets invoked, its file descriptors
0 (for standard input), 1 (for standard output), and 2 (for standard error)
are open. But there are situations when some of these file descriptors are
closed. These situations can arise when

* The invoking process invokes `close()` on the file descriptor before
  `exec`, or
* The invoking process invokes `posix_spawn_file_actions_addclose()` for
  the file descriptor before `posix_spawn` or `posix_spawnp`, or
* The invoking process is a Bourne shell, and the shell script uses the
  POSIX syntax for closing the file descriptor:
  `<&-` for closing standard input,
  `>&-` for closing standard output, or
  `2>&-` for closing standard error.

When a closed file descriptor is accessed through a system call, such as
`fcntl()`, `fstat()`, `read()`, or `write()`, the
system calls fails with error `EBADF` ("Bad file descriptor").

When a new file descriptor is allocated, the operating system chooses the
smallest non-negative integer that does not yet correspond to an open file
descriptor. So, when a given fd (0, 1, or 2) is closed, opening a new file
descriptor may assign the new file descriptor to this fd. This can have
unintended effects, because now standard input/output/error of your process
is referring to a file that was not meant to be used in that role.

This situation is a security risk because the behaviour of the program
in this situation was surely never tested, therefore anything can happen
then – from overwriting precious files of the user to endless loops.

To deal with this situation, you first need to determine whether your
program is affected by the problem.

* Does your program invoke functions that allocate new file descriptors?
  These are the system calls
  + `open()`, `openat()`, `creat()`
  + `dup()`
  + `fopen()`, `freopen()`
  + `pipe()`, `pipe2()`, `popen()`
  + `opendir()`
  + `tmpfile()`, `mkstemp()`, `mkstemps()`, `mkostemp()`,
    `mkostemps()`

  Note that you also have to consider the libraries that your program uses.
* If your program may open two or more file descriptors or FILE streams for
  reading at the same time, and some of them may reference standard input,
  your program *is affected*.
* If your program may open two or more file descriptors or FILE streams for
  writing at the same time, and some of them may reference standard output
  or standard error, your program *is affected*.
* If your program does not open new file descriptors or FILE streams, it is
  *not affected*.
* If your program opens only one new file descriptor or FILE stream at a time,
  it is *not affected*. This is often the case for programs that are
  structured in simple phases: first a phase where input is read from a file
  into memory, then a phase of processing in memory, finally a phase where
  the result is written to a file.
* If your program opens only two new file descriptors or FILE streams at a
  time, out of which one is for reading and the one is for writing, it is
  *not affected*. This is because if the first file descriptor is
  allocated and the second file descriptor is picked as 0, 1, or 2, and
  both happen to be the same, writing to the one opened in `O_RDONLY`
  mode will produce an error `EBADF`, as desired.

If your program is affected, what is the mitigation?

Some operating systems install open file descriptors in place of the
closed ones, either in the `exec` system call or during program
startup. When such a file descriptor is accessed through a system call,
it behaves like an open file descriptor opened for the “wrong” direction:
the system calls `fcntl()` and `fstat()` succeed, whereas
`read()` from fd 0 and `write()` to fd 1 or 2 fail with error
`EBADF` ("Bad file descriptor"). The important point here is that
when your program allocates a new file descriptor, it will have a value
greater than 2.

This mitigation is enabled on HP-UX, for all programs, and on glibc,
FreeBSD, NetBSD, OpenBSD, but only for setuid or setgid programs. Since
it is operating system dependent, it is not a complete mitigation.

For a complete mitigation, Gnulib provides two alternative sets of modules:

* The `xstdopen` module.
* The `*-safer` modules:
  `fcntl-safer`,
  `openat-safer`,
  `unistd-safer`,
  `fopen-safer`,
  `freopen-safer`,
  `pipe2-safer`,
  `popen-safer`,
  `dirent-safer`,
  `tmpfile-safer`,
  `stdlib-safer`.

The approach with the `xstdopen` module is simple, but it adds three
system calls to program startup. Whereas the approach with the `*-safer`
modules is more complicated and error-prone,
and does not fix the problem if system library functions call one
of the affected functions,
but adds no overhead (no additional system calls)
in the normal case.

To use the approach with the `xstdopen` module:

1. Import the module `xstdopen` from Gnulib.
2. In the compilation unit that contains the `main` function, include
   `"xstdopen.h"`.
3. In the `main` function, near the beginning, namely right after
   the i18n related initializations (`setlocale`, `bindtextdomain`,
   `textdomain` invocations, if any) and
   the `closeout` initialization (if any), insert the invocation:

   ```
   /* Ensure that stdin, stdout, stderr are open.  */
   xstdopen ();
   ```

To use the approach with the `*-safer` modules:

1. Import the relevant modules from Gnulib.
2. In the compilation units that contain these function calls, include the
   replacement header file.

Do so according to this table:

| Function | Module | Header file |
| --- | --- | --- |
| `open()` | `fcntl-safer` | `"fcntl--.h"` |
| `openat()` | `openat-safer` | `"fcntl--.h"` |
| `creat()` | `fcntl-safer` | `"fcntl--.h"` |
| `dup()` | `unistd-safer` | `"unistd--.h"` |
| `fopen()` | `fopen-safer` | `"stdio--.h"` |
| `freopen()` | `freopen-safer` | `"stdio--.h"` |
| `pipe()` | `unistd-safer` | `"unistd--.h"` |
| `pipe2()` | `pipe2-safer` | `"unistd--.h"` |
| `popen()` | `popen-safer` | `"stdio--.h"` |
| `opendir()` | `dirent-safer` | `"dirent--.h"` |
| `tmpfile()` | `tmpfile-safer` | `"stdio--.h"` |
| `mkstemp()` | `stdlib-safer` | `"stdlib--.h"` |
| `mkstemps()` | `stdlib-safer` | `"stdlib--.h"` |
| `mkostemp()` | `stdlib-safer` | `"stdlib--.h"` |
| `mkostemps()` | `stdlib-safer` | `"stdlib--.h"` |

---

Next: [Container data types](#Container-data-types), Previous: [Handling closed standard file descriptors](#Closed-standard-fds), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.11 Handling strings with NUL characters [¶](#Handling-strings-with-NUL-characters-1)

Strings in C are usually represented by a character sequence with a
terminating NUL character. A ‘`char *`’, pointer to the first byte
of this character sequence, is what gets passed around as function
argument or return value.

The major restriction of this string representation is that it cannot
handle strings that contain NUL characters: such strings will appear
shorter than they were meant to be. In most application areas, this is
not a problem, and the `char *` type is well usable.

A second problem of this string representation is that
taking a substring is not cheap:
it either requires a memory allocation
or a destructive modification of the string.
The former has a runtime cost;
the latter complicates the logic of the program.
This matters for application areas that analyze text, such as parsers.

In areas where strings with embedded NUL characters need to be handled
or where taking substrings is a recurrent operation,
the common approach is to use a `char *ptr` pointer variable
together with a `size_t nbytes` variable (or an `idx_t nbytes`
variable, if you want to avoid problems due to integer overflow). This
works fine in code that constructs or manipulates strings with embedded
NUL characters. But when it comes to *storing* them, for example
in an array or as key or value of a hash table, one needs a type that
combines these two fields.

The Gnulib modules `string-desc`, `xstring-desc`, and
`string-desc-quotearg` provide such a type. We call it a
“string descriptor” and name it `string_desc_t`.

The type `string_desc_t` is a struct that contains a pointer to the
first byte and the number of bytes of the memory region that make up the
string. An additional terminating NUL byte, that may be present in
memory, is not included in this byte count. This type implements the
same concept as `std::string_view` in C++, or the `String`
type in Java.

`string_desc_t` is a string descriptor to a string that cannot
be written to. There is also a type `rw_string_desc_t`, that is
a descriptor for a writable string.
`rw_string_desc_t` compares to `string_desc_t`, like the
pointer type ‘`char *`’ compares to the pointer type
‘`const char *`’.

A `string_desc_t` or `rw_string_desc_t`
can be passed to a function as an argument, or
can be the return value of a function. This is type-safe: If, by
mistake, a programmer passes a `string_desc_t` to a function that
expects a `char *` argument, or vice versa, or assigns a
`string_desc_t` value to a variable of type `char *`, or
vice versa, the compiler will report an error.

Unfortunately, `string_desc_t` and `rw_string_desc_t`
being different types, there is no implicit conversion from
`rw_string_desc_t` to `string_desc_t`. In places
where such a conversion is desired, the (inline) function
`sd_readonly` needs to be called.

Functions related to string descriptors are provided:

* Side-effect-free operations in `"string-desc.h"`,
* Memory-allocating operations in `"string-desc.h"`,
* Memory-allocating operations with out-of-memory checking in
  `"xstring-desc.h"`,
* Operations with side effects in `"string-desc.h"`.

For outputting a string descriptor, the `*printf` family of
functions cannot be used directly. A format string directive such as
`"%.*s"` would not work:

* it would stop the output at the first encountered NUL character,
* it would require to cast the number of bytes to `int`, and thus
  would not work for strings longer than `INT_MAX` bytes.

Therefore Gnulib offers

* a function `sd_fwrite` that outputs a string descriptor to
  a `FILE` stream,
* a function `sd_write` that outputs a string descriptor to
  a file descriptor,
* and for those applications where the NUL characters should become
  visible as ‘`\0`’, a family of `quotearg` based functions, that
  allow to specify the escaping rules in detail.

The functionality is thus split across three modules as follows:

* The module `string-desc`, under LGPL, defines the type and
  elementary functions.
* The module `xstring-desc`, under GPL, defines the memory-allocating
  functions with out-of-memory checking.
* The module `string-desc-quotearg`, under GPL, defines the
  `quotearg` based functions.

---

Next: [Modernized printf](#Modernized-printf), Previous: [Handling strings with NUL characters](#Handling-strings-with-NUL-characters), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
