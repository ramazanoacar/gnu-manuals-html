### 10.5 Functions in `<ctype.h>` [¶](#Functions-in-_003cctype_002eh_003e-1)

* [`isalnum`](#isalnum)
* [`isalnum_l`](#isalnum_005fl)
* [`isalpha`](#isalpha)
* [`isalpha_l`](#isalpha_005fl)
* [`isblank`](#isblank)
* [`isblank_l`](#isblank_005fl)
* [`iscntrl`](#iscntrl)
* [`iscntrl_l`](#iscntrl_005fl)
* [`isdigit`](#isdigit)
* [`isdigit_l`](#isdigit_005fl)
* [`isgraph`](#isgraph)
* [`isgraph_l`](#isgraph_005fl)
* [`islower`](#islower)
* [`islower_l`](#islower_005fl)
* [`isprint`](#isprint)
* [`isprint_l`](#isprint_005fl)
* [`ispunct`](#ispunct)
* [`ispunct_l`](#ispunct_005fl)
* [`isspace`](#isspace)
* [`isspace_l`](#isspace_005fl)
* [`isupper`](#isupper)
* [`isupper_l`](#isupper_005fl)
* [`isxdigit`](#isxdigit)
* [`isxdigit_l`](#isxdigit_005fl)
* [`tolower`](#tolower)
* [`tolower_l`](#tolower_005fl)
* [`toupper`](#toupper)
* [`toupper_l`](#toupper_005fl)

---

Next: [`isalnum_l`](#isalnum_005fl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.1 `isalnum` [¶](#isalnum-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalnum.html>

Gnulib module: ctype

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

Note: This function’s behaviour depends on the locale, but does not support
the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are five alternative APIs:

`c_isalnum` [¶](#index-c_002dctype)
:   This function operates in a locale independent way and returns true only for
    ASCII characters. It is provided by the Gnulib module ‘`c-ctype`’.

`iswalnum` [¶](#index-wctype-1)
:   This function operates in a locale dependent way, on wide characters. In
    order to use it, you first have to convert from multibyte to wide characters,
    using the `mbrtowc` function. It is provided by the Gnulib module
    ‘`wctype`’.

`c32isalnum` [¶](#index-c32isalnum)
:   This function operates in a locale dependent way, on 32-bit wide characters.
    In order to use it, you first have to convert from multibyte to 32-bit wide
    characters, using the `mbrtoc32` function. It is provided by the
    Gnulib module ‘`c32isalnum`’.

`mb_isalnum` [¶](#index-mbchar)
:   This function operates in a locale dependent way, on multibyte characters.
    It is provided by the Gnulib module ‘`mbchar`’.

`uc_is_alnum` [¶](#index-unictype_002fctype_002dalnum)
:   This function operates in a locale independent way, on Unicode characters.
    It is provided by the Gnulib module ‘`unictype/ctype-alnum`’.

---

Next: [`isalpha`](#isalpha), Previous: [`isalnum`](#isalnum), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.2 `isalnum_l` [¶](#isalnum_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalnum_l.html>

Gnulib module: isalnum\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`isalpha_l`](#isalpha_005fl), Previous: [`isalnum_l`](#isalnum_005fl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.3 `isalpha` [¶](#isalpha-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalpha.html>

Gnulib module: ctype

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

Note: This function’s behaviour depends on the locale, but does not support
the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are five alternative APIs:

`c_isalpha` [¶](#index-c_002dctype-1)
:   This function operates in a locale independent way and returns true only for
    ASCII characters. It is provided by the Gnulib module ‘`c-ctype`’.

`iswalpha` [¶](#index-wctype-2)
:   This function operates in a locale dependent way, on wide characters. In
    order to use it, you first have to convert from multibyte to wide characters,
    using the `mbrtowc` function. It is provided by the Gnulib module
    ‘`wctype`’.

`c32isalpha` [¶](#index-c32isalpha)
:   This function operates in a locale dependent way, on 32-bit wide characters.
    In order to use it, you first have to convert from multibyte to 32-bit wide
    characters, using the `mbrtoc32` function. It is provided by the
    Gnulib module ‘`c32isalpha`’.

`mb_isalpha` [¶](#index-mbchar-1)
:   This function operates in a locale dependent way, on multibyte characters.
    It is provided by the Gnulib module ‘`mbchar`’.

`uc_is_alpha` [¶](#index-unictype_002fctype_002dalpha)
:   This function operates in a locale independent way, on Unicode characters.
    It is provided by the Gnulib module ‘`unictype/ctype-alpha`’.

---

Next: [`isblank`](#isblank), Previous: [`isalpha`](#isalpha), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.4 `isalpha_l` [¶](#isalpha_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalpha_l.html>

Gnulib module: isalpha\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`isblank_l`](#isblank_005fl), Previous: [`isalpha_l`](#isalpha_005fl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.5 `isblank` [¶](#isblank-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isblank.html>

Gnulib module: isblank

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Solaris 9, mingw, MSVC 9.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

Note: This function’s behaviour depends on the locale, but does not support
the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are five alternative APIs:

`c_isblank` [¶](#index-c_002dctype-2)
:   This function operates in a locale independent way and returns true only for
    ASCII characters. It is provided by the Gnulib module ‘`c-ctype`’.

`iswblank` [¶](#index-wctype-3)
:   This function operates in a locale dependent way, on wide characters. In
    order to use it, you first have to convert from multibyte to wide characters,
    using the `mbrtowc` function. It is provided by the Gnulib module
    ‘`wctype`’.

`c32isblank` [¶](#index-c32isblank)
:   This function operates in a locale dependent way, on 32-bit wide characters.
    In order to use it, you first have to convert from multibyte to 32-bit wide
    characters, using the `mbrtoc32` function. It is provided by the
    Gnulib module ‘`c32isblank`’.

`mb_isblank` [¶](#index-mbchar-2)
:   This function operates in a locale dependent way, on multibyte characters.
    It is provided by the Gnulib module ‘`mbchar`’.

`uc_is_blank` [¶](#index-unictype_002fctype_002dblank)
:   This function operates in a locale independent way, on Unicode characters.
    It is provided by the Gnulib module ‘`unictype/ctype-blank`’.

---

Next: [`iscntrl`](#iscntrl), Previous: [`isblank`](#isblank), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.6 `isblank_l` [¶](#isblank_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isblank_l.html>

Gnulib module: isblank\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`iscntrl_l`](#iscntrl_005fl), Previous: [`isblank_l`](#isblank_005fl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.7 `iscntrl` [¶](#iscntrl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iscntrl.html>

Gnulib module: ctype

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

Note: This function’s behaviour depends on the locale, but does not support
the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are five alternative APIs:

`c_iscntrl` [¶](#index-c_002dctype-3)
:   This function operates in a locale independent way and returns true only for
    ASCII characters. It is provided by the Gnulib module ‘`c-ctype`’.

`iswcntrl` [¶](#index-wctype-4)
:   This function operates in a locale dependent way, on wide characters. In
    order to use it, you first have to convert from multibyte to wide characters,
    using the `mbrtowc` function. It is provided by the Gnulib module
    ‘`wctype`’.

`c32iscntrl` [¶](#index-c32iscntrl)
:   This function operates in a locale dependent way, on 32-bit wide characters.
    In order to use it, you first have to convert from multibyte to 32-bit wide
    characters, using the `mbrtoc32` function. It is provided by the
    Gnulib module ‘`c32iscntrl`’.

`mb_iscntrl` [¶](#index-mbchar-3)
:   This function operates in a locale dependent way, on multibyte characters.
    It is provided by the Gnulib module ‘`mbchar`’.

`uc_is_cntrl` [¶](#index-unictype_002fctype_002dcntrl)
:   This function operates in a locale independent way, on Unicode characters.
    It is provided by the Gnulib module ‘`unictype/ctype-cntrl`’.

---

Next: [`isdigit`](#isdigit), Previous: [`iscntrl`](#iscntrl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.8 `iscntrl_l` [¶](#iscntrl_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iscntrl_l.html>

Gnulib module: iscntrl\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`isdigit_l`](#isdigit_005fl), Previous: [`iscntrl_l`](#iscntrl_005fl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.9 `isdigit` [¶](#isdigit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isdigit.html>

Gnulib module: ctype

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

* This function’s behavior depends on locale, and it can return nonzero on
  arguments other than the usual decimal digits:
  mingw, MSVC.

Note: This function does not support
the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are five alternative APIs:

`c_isdigit` [¶](#index-c_002dctype-4)
:   This function operates in a locale independent way and returns true only for
    ASCII characters. It is provided by the Gnulib module ‘`c-ctype`’.

`iswdigit` [¶](#index-wctype-5)
:   This function operates in a locale dependent way, on wide characters. In
    order to use it, you first have to convert from multibyte to wide characters,
    using the `mbrtowc` function. It is provided by the Gnulib module
    ‘`wctype`’.

`c32isdigit` [¶](#index-c32isdigit)
:   This function operates in a locale dependent way, on 32-bit wide characters.
    In order to use it, you first have to convert from multibyte to 32-bit wide
    characters, using the `mbrtoc32` function. It is provided by the
    Gnulib module ‘`c32isdigit`’.

`mb_isdigit` [¶](#index-mbchar-4)
:   This function operates in a locale dependent way, on multibyte characters.
    It is provided by the Gnulib module ‘`mbchar`’.

`uc_is_digit` [¶](#index-unictype_002fctype_002ddigit)
:   This function operates in a locale independent way, on Unicode characters.
    It is provided by the Gnulib module ‘`unictype/ctype-digit`’.

---

Next: [`isgraph`](#isgraph), Previous: [`isdigit`](#isdigit), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.10 `isdigit_l` [¶](#isdigit_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isdigit_l.html>

Gnulib module: isdigit\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`isgraph_l`](#isgraph_005fl), Previous: [`isdigit_l`](#isdigit_005fl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.11 `isgraph` [¶](#isgraph-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isgraph.html>

Gnulib module: ctype

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

Note: This function’s behaviour depends on the locale, but does not support
the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are five alternative APIs:

`c_isgraph` [¶](#index-c_002dctype-5)
:   This function operates in a locale independent way and returns true only for
    ASCII characters. It is provided by the Gnulib module ‘`c-ctype`’.

`iswgraph` [¶](#index-wctype-6)
:   This function operates in a locale dependent way, on wide characters. In
    order to use it, you first have to convert from multibyte to wide characters,
    using the `mbrtowc` function. It is provided by the Gnulib module
    ‘`wctype`’.

`c32isgraph` [¶](#index-c32isgraph)
:   This function operates in a locale dependent way, on 32-bit wide characters.
    In order to use it, you first have to convert from multibyte to 32-bit wide
    characters, using the `mbrtoc32` function. It is provided by the
    Gnulib module ‘`c32isgraph`’.

`mb_isgraph` [¶](#index-mbchar-5)
:   This function operates in a locale dependent way, on multibyte characters.
    It is provided by the Gnulib module ‘`mbchar`’.

`uc_is_graph` [¶](#index-unictype_002fctype_002dgraph)
:   This function operates in a locale independent way, on Unicode characters.
    It is provided by the Gnulib module ‘`unictype/ctype-graph`’.

---

Next: [`islower`](#islower), Previous: [`isgraph`](#isgraph), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.12 `isgraph_l` [¶](#isgraph_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isgraph_l.html>

Gnulib module: isgraph\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`islower_l`](#islower_005fl), Previous: [`isgraph_l`](#isgraph_005fl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.13 `islower` [¶](#islower-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/islower.html>

Gnulib module: ctype

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

Note: This function’s behaviour depends on the locale, but does not support
the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are five alternative APIs:

`c_islower` [¶](#index-c_002dctype-6)
:   This function operates in a locale independent way and returns true only for
    ASCII characters. It is provided by the Gnulib module ‘`c-ctype`’.

`iswlower` [¶](#index-wctype-7)
:   This function operates in a locale dependent way, on wide characters. In
    order to use it, you first have to convert from multibyte to wide characters,
    using the `mbrtowc` function. It is provided by the Gnulib module
    ‘`wctype`’.

`c32islower` [¶](#index-c32islower)
:   This function operates in a locale dependent way, on 32-bit wide characters.
    In order to use it, you first have to convert from multibyte to 32-bit wide
    characters, using the `mbrtoc32` function. It is provided by the
    Gnulib module ‘`c32islower`’.

`mb_islower` [¶](#index-mbchar-6)
:   This function operates in a locale dependent way, on multibyte characters.
    It is provided by the Gnulib module ‘`mbchar`’.

`uc_is_lower` [¶](#index-unictype_002fctype_002dlower)
:   This function operates in a locale independent way, on Unicode characters.
    It is provided by the Gnulib module ‘`unictype/ctype-lower`’.

---

Next: [`isprint`](#isprint), Previous: [`islower`](#islower), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.14 `islower_l` [¶](#islower_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/islower_l.html>

Gnulib module: islower\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`isprint_l`](#isprint_005fl), Previous: [`islower_l`](#islower_005fl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.15 `isprint` [¶](#isprint-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isprint.html>

Gnulib module: ctype

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

Note: This function’s behaviour depends on the locale, but does not support
the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are five alternative APIs:

`c_isprint` [¶](#index-c_002dctype-7)
:   This function operates in a locale independent way and returns true only for
    ASCII characters. It is provided by the Gnulib module ‘`c-ctype`’.

`iswprint` [¶](#index-wctype-8)
:   This function operates in a locale dependent way, on wide characters. In
    order to use it, you first have to convert from multibyte to wide characters,
    using the `mbrtowc` function. It is provided by the Gnulib module
    ‘`wctype`’.

`c32isprint` [¶](#index-c32isprint)
:   This function operates in a locale dependent way, on 32-bit wide characters.
    In order to use it, you first have to convert from multibyte to 32-bit wide
    characters, using the `mbrtoc32` function. It is provided by the
    Gnulib module ‘`c32isprint`’.

`mb_isprint` [¶](#index-mbchar-7)
:   This function operates in a locale dependent way, on multibyte characters.
    It is provided by the Gnulib module ‘`mbchar`’.

`uc_is_print` [¶](#index-unictype_002fctype_002dprint)
:   This function operates in a locale independent way, on Unicode characters.
    It is provided by the Gnulib module ‘`unictype/ctype-print`’.

---

Next: [`ispunct`](#ispunct), Previous: [`isprint`](#isprint), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.16 `isprint_l` [¶](#isprint_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isprint_l.html>

Gnulib module: isprint\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`ispunct_l`](#ispunct_005fl), Previous: [`isprint_l`](#isprint_005fl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.17 `ispunct` [¶](#ispunct-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ispunct.html>

Gnulib module: ctype

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

Note: This function’s behaviour depends on the locale, but does not support
the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are five alternative APIs:

`c_ispunct` [¶](#index-c_002dctype-8)
:   This function operates in a locale independent way and returns true only for
    ASCII characters. It is provided by the Gnulib module ‘`c-ctype`’.

`iswpunct` [¶](#index-wctype-9)
:   This function operates in a locale dependent way, on wide characters. In
    order to use it, you first have to convert from multibyte to wide characters,
    using the `mbrtowc` function. It is provided by the Gnulib module
    ‘`wctype`’.

`c32ispunct` [¶](#index-c32ispunct)
:   This function operates in a locale dependent way, on 32-bit wide characters.
    In order to use it, you first have to convert from multibyte to 32-bit wide
    characters, using the `mbrtoc32` function. It is provided by the
    Gnulib module ‘`c32ispunct`’.

`mb_ispunct` [¶](#index-mbchar-8)
:   This function operates in a locale dependent way, on multibyte characters.
    It is provided by the Gnulib module ‘`mbchar`’.

`uc_is_punct` [¶](#index-unictype_002fctype_002dpunct)
:   This function operates in a locale independent way, on Unicode characters.
    It is provided by the Gnulib module ‘`unictype/ctype-punct`’.

---

Next: [`isspace`](#isspace), Previous: [`ispunct`](#ispunct), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.18 `ispunct_l` [¶](#ispunct_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ispunct_l.html>

Gnulib module: ispunct\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`isspace_l`](#isspace_005fl), Previous: [`ispunct_l`](#ispunct_005fl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.19 `isspace` [¶](#isspace-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isspace.html>

Gnulib module: ctype

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

Note: This function’s behaviour depends on the locale, but does not support
the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are five alternative APIs:

`c_isspace` [¶](#index-c_002dctype-9)
:   This function operates in a locale independent way and returns true only for
    ASCII characters. It is provided by the Gnulib module ‘`c-ctype`’.

`iswspace` [¶](#index-wctype-10)
:   This function operates in a locale dependent way, on wide characters. In
    order to use it, you first have to convert from multibyte to wide characters,
    using the `mbrtowc` function. It is provided by the Gnulib module
    ‘`wctype`’.

`c32isspace` [¶](#index-c32isspace)
:   This function operates in a locale dependent way, on 32-bit wide characters.
    In order to use it, you first have to convert from multibyte to 32-bit wide
    characters, using the `mbrtoc32` function. It is provided by the
    Gnulib module ‘`c32isspace`’.

`mb_isspace` [¶](#index-mbchar-9)
:   This function operates in a locale dependent way, on multibyte characters.
    It is provided by the Gnulib module ‘`mbchar`’.

`uc_is_space` [¶](#index-unictype_002fctype_002dspace)
:   This function operates in a locale independent way, on Unicode characters.
    It is provided by the Gnulib module ‘`unictype/ctype-space`’.

---

Next: [`isupper`](#isupper), Previous: [`isspace`](#isspace), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.20 `isspace_l` [¶](#isspace_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isspace_l.html>

Gnulib module: isspace\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`isupper_l`](#isupper_005fl), Previous: [`isspace_l`](#isspace_005fl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.21 `isupper` [¶](#isupper-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isupper.html>

Gnulib module: ctype

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

Note: This function’s behaviour depends on the locale, but does not support
the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are five alternative APIs:

`c_isupper` [¶](#index-c_002dctype-10)
:   This function operates in a locale independent way and returns true only for
    ASCII characters. It is provided by the Gnulib module ‘`c-ctype`’.

`iswupper` [¶](#index-wctype-11)
:   This function operates in a locale dependent way, on wide characters. In
    order to use it, you first have to convert from multibyte to wide characters,
    using the `mbrtowc` function. It is provided by the Gnulib module
    ‘`wctype`’.

`c32isupper` [¶](#index-c32isupper)
:   This function operates in a locale dependent way, on 32-bit wide characters.
    In order to use it, you first have to convert from multibyte to 32-bit wide
    characters, using the `mbrtoc32` function. It is provided by the
    Gnulib module ‘`c32isupper`’.

`mb_isupper` [¶](#index-mbchar-10)
:   This function operates in a locale dependent way, on multibyte characters.
    It is provided by the Gnulib module ‘`mbchar`’.

`uc_is_upper` [¶](#index-unictype_002fctype_002dupper)
:   This function operates in a locale independent way, on Unicode characters.
    It is provided by the Gnulib module ‘`unictype/ctype-upper`’.

---

Next: [`isxdigit`](#isxdigit), Previous: [`isupper`](#isupper), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.22 `isupper_l` [¶](#isupper_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isupper_l.html>

Gnulib module: isupper\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`isxdigit_l`](#isxdigit_005fl), Previous: [`isupper_l`](#isupper_005fl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.23 `isxdigit` [¶](#isxdigit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isxdigit.html>

Gnulib module: ctype

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

* This function’s behavior depends on locale, and it can return nonzero on
  arguments other than the usual hexadecimal digits:
  mingw, MSVC.

Note: This function does not support
the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are five alternative APIs:

`c_isxdigit` [¶](#index-c_002dctype-11)
:   This function operates in a locale independent way and returns true only for
    ASCII characters. It is provided by the Gnulib module ‘`c-ctype`’.

`iswxdigit` [¶](#index-wctype-12)
:   This function operates in a locale dependent way, on wide characters. In
    order to use it, you first have to convert from multibyte to wide characters,
    using the `mbrtowc` function. It is provided by the Gnulib module
    ‘`wctype`’.

`c32isxdigit` [¶](#index-c32isxdigit)
:   This function operates in a locale dependent way, on 32-bit wide characters.
    In order to use it, you first have to convert from multibyte to 32-bit wide
    characters, using the `mbrtoc32` function. It is provided by the
    Gnulib module ‘`c32isxdigit`’.

`mb_isxdigit` [¶](#index-mbchar-11)
:   This function operates in a locale dependent way, on multibyte characters.
    It is provided by the Gnulib module ‘`mbchar`’.

`uc_is_xdigit` [¶](#index-unictype_002fctype_002dxdigit)
:   This function operates in a locale independent way, on Unicode characters.
    It is provided by the Gnulib module ‘`unictype/ctype-xdigit`’.

---

Next: [`tolower`](#tolower), Previous: [`isxdigit`](#isxdigit), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.24 `isxdigit_l` [¶](#isxdigit_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isxdigit_l.html>

Gnulib module: isxdigit\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`tolower_l`](#tolower_005fl), Previous: [`isxdigit_l`](#isxdigit_005fl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.25 `tolower` [¶](#tolower-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tolower.html>

Gnulib module: ctype

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

Note: This function’s behaviour depends on the locale, but does not support
the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are four alternative APIs:

`c_tolower` [¶](#index-c_002dctype-12)
:   This function operates in a locale independent way and returns a different
    value than the argument only for uppercase ASCII characters. It is provided
    by the Gnulib module ‘`c-ctype`’.

`towlower` [¶](#index-wctype-13)
:   This function operates in a locale dependent way, on wide characters. In
    order to use it, you first have to convert from multibyte to wide characters,
    using the `mbrtowc` function. It is provided by the Gnulib module
    ‘`wctype`’.

`c32tolower` [¶](#index-c32tolower)
:   This function operates in a locale dependent way, on 32-bit wide characters.
    In order to use it, you first have to convert from multibyte to 32-bit wide
    characters, using the `mbrtoc32` function. It is provided by the
    Gnulib module ‘`c32tolower`’.

`uc_tolower` [¶](#index-unicase_002ftolower)
:   This function operates in a locale independent way, on Unicode characters.
    It is provided by the Gnulib module ‘`unicase/tolower`’.

---

Next: [`toupper`](#toupper), Previous: [`tolower`](#tolower), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.26 `tolower_l` [¶](#tolower_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tolower_l.html>

Gnulib module: tolower\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`toupper_l`](#toupper_005fl), Previous: [`tolower_l`](#tolower_005fl), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.27 `toupper` [¶](#toupper-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/toupper.html>

Gnulib module: ctype

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

Note: This function’s behaviour depends on the locale, but does not support
the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are four alternative APIs:

`c_toupper` [¶](#index-c_002dctype-13)
:   This function operates in a locale independent way and returns a different
    value than the argument only for lowercase ASCII characters. It is provided
    by the Gnulib module ‘`c-ctype`’.

`towupper` [¶](#index-wctype-14)
:   This function operates in a locale dependent way, on wide characters. In
    order to use it, you first have to convert from multibyte to wide characters,
    using the `mbrtowc` function. It is provided by the Gnulib module
    ‘`wctype`’.

`c32toupper` [¶](#index-c32toupper)
:   This function operates in a locale dependent way, on 32-bit wide characters.
    In order to use it, you first have to convert from multibyte to 32-bit wide
    characters, using the `mbrtoc32` function. It is provided by the
    Gnulib module ‘`c32toupper`’.

`uc_toupper` [¶](#index-unicase_002ftoupper)
:   This function operates in a locale independent way, on Unicode characters.
    It is provided by the Gnulib module ‘`unicase/toupper`’.

---

Previous: [`toupper`](#toupper), Up: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.5.28 `toupper_l` [¶](#toupper_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/toupper_l.html>

Gnulib module: toupper\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e), Previous: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.6 Functions in `<devctl.h>` [¶](#Functions-in-_003cdevctl_002eh_003e-1)

* [`posix_devctl`](#posix_005fdevctl)

---

Up: [Functions in `<devctl.h>`](#Functions-in-_003cdevctl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.6.1 `posix_devctl` [¶](#posix_005fdevctl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_devctl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

---

Next: [Functions in `<dlfcn.h>`](#Functions-in-_003cdlfcn_002eh_003e), Previous: [Functions in `<devctl.h>`](#Functions-in-_003cdevctl_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.7 Functions in `<dirent.h>` [¶](#Functions-in-_003cdirent_002eh_003e-1)

* [`alphasort`](#alphasort)
* [`closedir`](#closedir)
* [`dirfd`](#dirfd)
* [`fdopendir`](#fdopendir)
* [`opendir`](#opendir)
* [`posix_getdents`](#posix_005fgetdents)
* [`readdir`](#readdir)
* [`readdir_r`](#readdir_005fr)
* [`rewinddir`](#rewinddir)
* [`scandir`](#scandir)
* [`seekdir`](#seekdir)
* [`telldir`](#telldir)

---

Next: [`closedir`](#closedir), Up: [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.7.1 `alphasort` [¶](#alphasort-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/alphasort.html>

Gnulib module: alphasort

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Solaris 9, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* The parameters of this function are declared as `const void *` on some platforms:
  glibc 2.3.6, macOS 10.7, FreeBSD 6.0, NetBSD 7.1, OpenBSD 7.9.
* The parameters of this function are declared as `void *` on some platforms:
  AIX 5.1.

---

Next: [`dirfd`](#dirfd), Previous: [`alphasort`](#alphasort), Up: [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.7.2 `closedir` [¶](#closedir-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/closedir.html>

Gnulib module: closedir

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`fdopendir`](#fdopendir), Previous: [`closedir`](#closedir), Up: [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.7.3 `dirfd` [¶](#dirfd-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dirfd.html>

Gnulib module: dirfd

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 7.1, HP-UX 11, Solaris 10, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* This function always fails on some platforms:
  mingw.
* There is a `dirfd` macro but no function, and the macro does not
  work with an argument of type `void *`, as a function would:
  NetBSD 9.2.

---

Next: [`opendir`](#opendir), Previous: [`dirfd`](#dirfd), Up: [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.7.4 `fdopendir` [¶](#fdopendir-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fdopendir.html>

Gnulib module: fdopendir

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8,
  AIX 5.1, HP-UX 11, Cygwin 1.5.x, mingw, MSVC 14.
  But the replacement function is not safe to be used in libraries and
  is not thread-safe. Also, the replacement does not guarantee
  that ‘`dirfd(fdopendir(n))==n`’ (dirfd might fail, or return a
  different file descriptor than n).
* This function exists but is not declared on some platforms:
  FreeBSD 7.3.
* This function does not reject non-directory file descriptors on some
  platforms:
  GNU/Hurd.
* This function mistakenly closes non-directory file descriptors on some
  platforms:
  FreeBSD 8.1.

Portability problems not fixed by Gnulib:

---

Next: [`posix_getdents`](#posix_005fgetdents), Previous: [`fdopendir`](#fdopendir), Up: [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.7.5 `opendir` [¶](#opendir-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/opendir.html>

Gnulib module: opendir

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 14.
* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on huge directories 2 GiB and larger. Also, on platforms
  where `ino_t` is a 32-bit type, this function may report inode numbers
  incorrectly. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

---

Next: [`readdir`](#readdir), Previous: [`opendir`](#opendir), Up: [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.7.6 `posix_getdents` [¶](#posix_005fgetdents-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_getdents.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`readdir_r`](#readdir_005fr), Previous: [`posix_getdents`](#posix_005fgetdents), Up: [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.7.7 `readdir` [¶](#readdir-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/readdir.html>

Gnulib module: readdir

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 14.
* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on huge directories 2 GiB and larger. Also, on platforms
  where `ino_t` is a 32-bit type, this function may report inode numbers
  incorrectly. This can occur with file systems such as XFS (typically on
  large disks) and NFS. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* Although POSIX places no restrictions on `d_ino` values, some
  older systems are rumored to return `d_ino` values equal to zero
  for directory entries that do not really exist. Although Gnulib
  formerly attempted to cater to these older systems, this caused
  misbehavior on standard systems and so Gnulib does not attempt to
  cater to them any more. If you know of any problems caused by this,
  please send a bug report.
* When reading a directory that has been removed,
  this function sets `errno` to `ENOENT`
  instead of leaving `errno` alone to indicate EOF:
  glibc 2.2.5.

---

Next: [`rewinddir`](#rewinddir), Previous: [`readdir`](#readdir), Up: [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.7.8 `readdir_r` [¶](#readdir_005fr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/readdir_r.html>

Gnulib module: extensions

Portability problems fixed by Gnulib:

* This function is planned to be removed from POSIX and to be deprecated
  in glibc. Portable applications should use `readdir`.
* This function has an incompatible declaration on some platforms:
  Solaris 11.4 (when `_POSIX_PTHREAD_SEMANTICS` is not defined).
* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on huge directories 2 GiB and larger. Also, on platforms
  where `ino_t` is a 32-bit type, this function may report inode numbers
  incorrectly. This can occur with file systems such as XFS (typically on
  large disks) and NFS. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* When reading a directory that has been removed,
  this function sets `errno` to `ENOENT`
  instead of leaving `errno` alone to indicate EOF:
  glibc 2.2.5.

---

Next: [`scandir`](#scandir), Previous: [`readdir_r`](#readdir_005fr), Up: [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.7.9 `rewinddir` [¶](#rewinddir-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/rewinddir.html>

Gnulib module: rewinddir

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 14.
* On Mac OS X platforms where `long int` is a 32-bit type, this function may
  not work correctly on huge directories 2 GiB and larger.
  See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

---

Next: [`seekdir`](#seekdir), Previous: [`rewinddir`](#rewinddir), Up: [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.7.10 `scandir` [¶](#scandir-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/scandir.html>

Gnulib module: scandir

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Solaris 9, mingw, MSVC 14.
* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on huge directories 2 GiB and larger. Also, on platforms
  where `ino_t` is a 32-bit type, this function may report inode numbers
  incorrectly. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* The fourth parameter of this function is declared as `int (*) (const void *, const void *)` on some platforms:
  glibc 2.3.6, macOS 10.7, FreeBSD 6.0, NetBSD 7.1, OpenBSD 7.9.
* The fourth parameter of this function is declared as `int (*) (void *, void *)` on some platforms:
  AIX 5.1.

---

Next: [`telldir`](#telldir), Previous: [`scandir`](#scandir), Up: [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.7.11 `seekdir` [¶](#seekdir-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/seekdir.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `long int` is a 32-bit type, this function may not
  work correctly on huge directories 2 GiB and larger.
  See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 14, Android 5.1.

---

Previous: [`seekdir`](#seekdir), Up: [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.7.12 `telldir` [¶](#telldir-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/telldir.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `long int` is a 32-bit type, this function may not
  work correctly on huge directories 2 GiB and larger.
  See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 14, Android 5.1.

---

Next: [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e), Previous: [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.8 Functions in `<dlfcn.h>` [¶](#Functions-in-_003cdlfcn_002eh_003e-1)

* [`dladdr`](#dladdr)
* [`dlclose`](#dlclose)
* [`dlerror`](#dlerror)
* [`dlopen`](#dlopen)
* [`dlsym`](#dlsym)

---

Next: [`dlclose`](#dlclose), Up: [Functions in `<dlfcn.h>`](#Functions-in-_003cdlfcn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.8.1 `dladdr` [¶](#dladdr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dladdr.html>

Documentation:  
 [man dladdr](https://www.kernel.org/doc/man-pages/online/pages/man3/dladdr.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Cygwin 1.7.x, mingw, MSVC 14.

---

Next: [`dlerror`](#dlerror), Previous: [`dladdr`](#dladdr), Up: [Functions in `<dlfcn.h>`](#Functions-in-_003cdlfcn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.8.2 `dlclose` [¶](#dlclose-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dlclose.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`dlopen`](#dlopen), Previous: [`dlclose`](#dlclose), Up: [Functions in `<dlfcn.h>`](#Functions-in-_003cdlfcn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.8.3 `dlerror` [¶](#dlerror-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dlerror.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`dlsym`](#dlsym), Previous: [`dlerror`](#dlerror), Up: [Functions in `<dlfcn.h>`](#Functions-in-_003cdlfcn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.8.4 `dlopen` [¶](#dlopen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dlopen.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-dlopen-1.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* If the file name argument is not absolute, the file is searched for. The
  search algorithm is system specific.

---

Previous: [`dlopen`](#dlopen), Up: [Functions in `<dlfcn.h>`](#Functions-in-_003cdlfcn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.8.5 `dlsym` [¶](#dlsym-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dlsym.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-dlsym-1.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* The visibility of symbols loaded in dependent shared libraries or present
  in the main executable is system dependent.

---

Next: [Functions in `<errno.h>`](#Functions-in-_003cerrno_002eh_003e), Previous: [Functions in `<dlfcn.h>`](#Functions-in-_003cdlfcn_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.9 Functions in `<endian.h>` [¶](#Functions-in-_003cendian_002eh_003e-1)

* [`be16toh`](#be16toh)
* [`be32toh`](#be32toh)
* [`be64toh`](#be64toh)
* [`htobe16`](#htobe16)
* [`htobe32`](#htobe32)
* [`htobe64`](#htobe64)
* [`htole16`](#htole16)
* [`htole32`](#htole32)
* [`htole64`](#htole64)
* [`le16toh`](#le16toh)
* [`le32toh`](#le32toh)
* [`le64toh`](#le64toh)

---

Next: [`be32toh`](#be32toh), Up: [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.9.1 `be16toh` [¶](#be16toh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/be16toh.html>

Documentation:  
 [man be16toh](https://www.kernel.org/doc/man-pages/online/pages/man3/be16toh.3.html)

Gnulib module: endian

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.8, macOS 14, FreeBSD 13.0, NetBSD 7.2, OpenBSD 5.5, Minix 3.1.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`be64toh`](#be64toh), Previous: [`be16toh`](#be16toh), Up: [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.9.2 `be32toh` [¶](#be32toh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/be32toh.html>

Documentation:  
 [man be32toh](https://www.kernel.org/doc/man-pages/online/pages/man3/be32toh.3.html)

Gnulib module: endian

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.8, macOS 14, FreeBSD 13.0, NetBSD 7.2, OpenBSD 5.5, Minix 3.1.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`htobe16`](#htobe16), Previous: [`be32toh`](#be32toh), Up: [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.9.3 `be64toh` [¶](#be64toh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/be64toh.html>

Documentation:  
 [man be64toh](https://www.kernel.org/doc/man-pages/online/pages/man3/be64toh.3.html)

Gnulib module: endian

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.8, macOS 14, FreeBSD 13.0, NetBSD 7.2, OpenBSD 5.5, Minix 3.1.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`htobe32`](#htobe32), Previous: [`be64toh`](#be64toh), Up: [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.9.4 `htobe16` [¶](#htobe16-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/htobe16.html>

Documentation:  
 [man htobe16](https://www.kernel.org/doc/man-pages/online/pages/man3/htobe16.3.html)

Gnulib module: endian

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.8, macOS 14, FreeBSD 13.0, NetBSD 7.2, OpenBSD 5.5, Minix 3.1.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`htobe64`](#htobe64), Previous: [`htobe16`](#htobe16), Up: [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.9.5 `htobe32` [¶](#htobe32-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/htobe32.html>

Documentation:  
 [man htobe32](https://www.kernel.org/doc/man-pages/online/pages/man3/htobe32.3.html)

Gnulib module: endian

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.8, macOS 14, FreeBSD 13.0, NetBSD 7.2, OpenBSD 5.5, Minix 3.1.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`htole16`](#htole16), Previous: [`htobe32`](#htobe32), Up: [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.9.6 `htobe64` [¶](#htobe64-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/htobe64.html>

Documentation:  
 [man htobe64](https://www.kernel.org/doc/man-pages/online/pages/man3/htobe64.3.html)

Gnulib module: endian

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.8, macOS 14, FreeBSD 13.0, NetBSD 7.2, OpenBSD 5.5, Minix 3.1.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`htole32`](#htole32), Previous: [`htobe64`](#htobe64), Up: [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.9.7 `htole16` [¶](#htole16-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/htole16.html>

Documentation:  
 [man htole16](https://www.kernel.org/doc/man-pages/online/pages/man3/htole16.3.html)

Gnulib module: endian

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.8, macOS 14, FreeBSD 13.0, NetBSD 7.2, OpenBSD 5.5, Minix 3.1.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`htole64`](#htole64), Previous: [`htole16`](#htole16), Up: [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.9.8 `htole32` [¶](#htole32-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/htole32.html>

Documentation:  
 [man htole32](https://www.kernel.org/doc/man-pages/online/pages/man3/htole32.3.html)

Gnulib module: endian

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.8, macOS 14, FreeBSD 13.0, NetBSD 7.2, OpenBSD 5.5, Minix 3.1.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`le16toh`](#le16toh), Previous: [`htole32`](#htole32), Up: [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.9.9 `htole64` [¶](#htole64-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/htole64.html>

Documentation:  
 [man htole64](https://www.kernel.org/doc/man-pages/online/pages/man3/htole64.3.html)

Gnulib module: endian

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.8, macOS 14, FreeBSD 13.0, NetBSD 7.2, OpenBSD 5.5, Minix 3.1.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`le32toh`](#le32toh), Previous: [`htole64`](#htole64), Up: [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.9.10 `le16toh` [¶](#le16toh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/le16toh.html>

Documentation:  
 [man le16toh](https://www.kernel.org/doc/man-pages/online/pages/man3/le16toh.3.html)

Gnulib module: endian

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.8, macOS 14, FreeBSD 13.0, NetBSD 7.2, OpenBSD 5.5, Minix 3.1.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`le64toh`](#le64toh), Previous: [`le16toh`](#le16toh), Up: [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.9.11 `le32toh` [¶](#le32toh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/le32toh.html>

Documentation:  
 [man le32toh](https://www.kernel.org/doc/man-pages/online/pages/man3/le32toh.3.html)

Gnulib module: endian

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.8, macOS 14, FreeBSD 13.0, NetBSD 7.2, OpenBSD 5.5, Minix 3.1.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Previous: [`le32toh`](#le32toh), Up: [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.9.12 `le64toh` [¶](#le64toh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/le64toh.html>

Documentation:  
 [man le64toh](https://www.kernel.org/doc/man-pages/online/pages/man3/le64toh.3.html)

Gnulib module: endian

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.8, macOS 14, FreeBSD 13.0, NetBSD 7.2, OpenBSD 5.5, Minix 3.1.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [Functions in `<fcntl.h>`](#Functions-in-_003cfcntl_002eh_003e), Previous: [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.10 Functions in `<errno.h>` [¶](#Functions-in-_003cerrno_002eh_003e-1)

* [`errno`](#errno)

---

Up: [Functions in `<errno.h>`](#Functions-in-_003cerrno_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.10.1 `errno` [¶](#errno-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/errno.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On Windows, the socket functions don’t set `errno`; their error code is
  available through `WSAGetLastError()` instead.

---

Next: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e), Previous: [Functions in `<errno.h>`](#Functions-in-_003cerrno_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.11 Functions in `<fcntl.h>` [¶](#Functions-in-_003cfcntl_002eh_003e-1)

* [`creat`](#creat)
* [`fcntl`](#fcntl)
* [`open`](#open)
* [`openat`](#openat)
* [`posix_fadvise`](#posix_005ffadvise)
* [`posix_fallocate`](#posix_005ffallocate)

---

Next: [`fcntl`](#fcntl), Up: [Functions in `<fcntl.h>`](#Functions-in-_003cfcntl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.11.1 `creat` [¶](#creat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/creat.html>

Gnulib module: creat

Portability problems fixed by Gnulib:

* This function does not support modes with execution bits (such as 0700)
  on some platforms:
  MSVC 14.
* On platforms where `off_t` is a 32-bit type, `creat` may not work
  correctly with files 2 GiB and larger. See [Large File Support](#Large-File-Support).
* This function does not fail when the file name argument ends in a slash
  and (without the slash) names a nonexistent file, on some platforms:
  FreeBSD 7.2, AIX 7.1, HP-UX 11.31, Solaris 9.

Portability problems not fixed by Gnulib:

* On Windows, this function returns a file handle in `O_TEXT` mode. If you
  need a file handle in `O_BINARY` mode, you need to use the function
  `open` instead.

---

Next: [`open`](#open), Previous: [`creat`](#creat), Up: [Functions in `<fcntl.h>`](#Functions-in-_003cfcntl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.11.2 `fcntl` [¶](#fcntl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fcntl.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-fcntl-3.html>

Gnulib module: fcntl

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function does not support `F_DUPFD_CLOEXEC` on some
  platforms:
  glibc with Linux kernels before 2.6.24,
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, AIX 7.1, HP-UX 11,
  Solaris 11 2010-11, Cygwin 1.7.1.
  Note that the gnulib replacement code is functional but not atomic.
* The `F_DUPFD_CLOEXEC` action of this function does not set the
  `FD_CLOEXEC` flag on some platforms:
  NetBSD 10.0.
* The `F_DUPFD_CLOEXEC` action of this function sets the
  `FD_CLOEXEC` flag on the wrong file descriptor on some platforms:
  Haiku.
* The `F_DUPFD` action of this function does not reject
  out-of-range targets properly on some platforms:
  AIX 7.1, Cygwin 1.5.x, Haiku.
* The `F_DUPFD` action of this function mistakenly clears
  `FD_CLOEXEC` on the source descriptor on some platforms:
  Haiku.

Portability problems not fixed by Gnulib:

* The replacement function does not support `F_SETFD`,
  `F_GETFL`, `F_SETFL`, `F_GETOWN`, `F_SETOWN`,
  `F_GETLK`, `F_SETLK`, and `F_SETLKW` on some platforms:
  mingw, MSVC 14.
* When a file does not support locking (such as on an NFS file system
  that does not support file locking), calls with `F_SETLK` and
  `F_SETLKW` fail with `errno` set to different values on
  different systems: `EINVAL` on OpenIndiana (as suggested by the
  POSIX 1003.1-2017 `fcntl` specification), `ENOLCK` on
  GNU/Linux, and `EOPNOTSUPP` on FreeBSD.

---

Next: [`openat`](#openat), Previous: [`fcntl`](#fcntl), Up: [Functions in `<fcntl.h>`](#Functions-in-_003cfcntl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.11.3 `open` [¶](#open-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/open.html>

Gnulib module: open, fchdir

Portability problems fixed by the Gnulib module `open`:

* Some platforms do not support `O_CLOEXEC`:
  Mac OS X 10.6, FreeBSD 8.4, NetBSD 5.1, OpenBSD 4.9, Minix 3.1.8, AIX 7.1, HP-UX 11, Solaris 10, Cygwin 1.7.x, mingw, MSVC 14.
* Some platforms do not support `O_DIRECTORY`:
  glibc 2.0, Mac OS X 10.4, FreeBSD 7.4, NetBSD 4.0.1, OpenBSD 4.9, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14.
* On platforms where `off_t` is a 32-bit type, `open` may not work
  correctly with files 2 GiB and larger. See [Large File Support](#Large-File-Support).
* This function does not fail when the file name argument ends in a slash
  and (without the slash) names a nonexistent file or a file that is not a
  directory, on some platforms:
  macOS 14, FreeBSD 7.2, AIX 7.1, HP-UX 11.00, Solaris 9.
* This function does not support the `O_NONBLOCK` flag when it is defined
  by the gnulib module `nonblocking` on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), this function does usually not
  recognize the `/dev/null` filename.

Portability problems fixed by the Gnulib module `fchdir`:

* On Windows platforms (excluding Cygwin), this function fails to open a
  read-only descriptor for directories.

Portability problems not fixed by Gnulib:

* The Gnulib replacement for `O_CLOEXEC` is not atomic, and so is
  not safe in the presence of multiple threads or signal handlers.
* The Gnulib replacement for `O_SEARCH` may require read access
  instead of search/execute access.
* On some platforms, `O_SEARCH` may reject directories
  where you have search access even though `O_SEARCH` is not replaced:
  macOS 12.6.
* When the file does not exist, POSIX says
  the behavior of `O_CREAT | O_DIRECTORY` is unspecified.
  Most platforms fail with `EINVAL` in this case.
  However, in Linux kernels 6.3 and earlier, this function
  creates a regular file and then either returns successfully
  (Linux kernel 5.6 and earlier) or fails with `ENOTDIR`
  (Linux kernels 5.7–6.3).
* `open ("symlink", O_CREAT ...)` fails when the argument points to a
  nonexistent file in an existing directory on some platforms:
  Haiku.
* `open ("symlink", O_NOFOLLOW ...)` fails with `errno` set to
  `EMLINK` instead of the POSIX-required `ELOOP` on some
  platforms:
  FreeBSD 14.2.
* `open ("symlink", O_NOFOLLOW ...)` fails with `errno` set to
  `EFTYPE` instead of the POSIX-required `ELOOP` on some
  platforms:
  NetBSD 10.0.
* `open ("directory", O_CREAT ...)` fails with `errno` set to
  `EINVAL` instead of the POSIX-required `EISDIR` on some
  platforms:
  Solaris 11.4.
* `open ("fifo", O_DIRECTORY ...)` does not fail on some platforms:
  Haiku.
* On Windows, this function returns a file handle in `O_TEXT` mode by
  default; this means that it translates `'\n'` to CR/LF by default. Use the
  `O_BINARY` flag if you need reliable binary I/O.

---

Next: [`posix_fadvise`](#posix_005ffadvise), Previous: [`open`](#open), Up: [Functions in `<fcntl.h>`](#Functions-in-_003cfcntl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.11.4 `openat` [¶](#openat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/openat.html>

Gnulib module: openat

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8,
  AIX 5.1, HP-UX 11, Cygwin 1.5.x, mingw, MSVC 14.
  But the replacement function is not safe to be used in libraries and
  is not thread-safe.
* Some platforms do not support `O_CLOEXEC`:
  AIX 7.1, Solaris 10.
* Some platforms do not support `O_DIRECTORY`:
  glibc 2.0, Mac OS X 10.4, FreeBSD 7.4, NetBSD 4.0.1, OpenBSD 4.9, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14.
* On platforms where `off_t` is a 32-bit type, `open` may not work
  correctly with files 2 GiB and larger. See [Large File Support](#Large-File-Support).
* This function does not fail when the file name argument ends in a slash
  and (without the slash) names a nonexistent file or a file that is not a
  directory, on some platforms:
  Solaris 9.

Portability problems not fixed by Gnulib:

* The Gnulib replacement for `O_CLOEXEC` is not atomic, and so is
  not safe in the presence of multiple threads or signal handlers.
* The Gnulib replacement for `O_SEARCH` may require read access
  instead of search/execute access.
* On some platforms, `O_SEARCH` may reject directories
  where you have search access even though `O_SEARCH` is not replaced:
  macOS 12.6.
* When the file does not exist, POSIX says
  the behavior of `O_CREAT | O_DIRECTORY` is unspecified.
  Most platforms fail with `EINVAL` in this case.
  However, in Linux kernels 6.3 and earlier, this function
  creates a regular file and then either returns successfully
  (Linux kernel 5.6 and earlier) or fails with `ENOTDIR`
  (Linux kernels 5.7–6.3).
* `openat (fd, "symlink", O_NOFOLLOW ...)` fails with `errno`
  set to `EMLINK` instead of the POSIX-required `ELOOP` on
  some platforms:
  FreeBSD 10.1.
* `openat (fd, "symlink", O_NOFOLLOW ...)` fails with `errno`
  set to `EFTYPE` instead of the POSIX-required `ELOOP` on
  some platforms:
  NetBSD 6.1.
* `openat (fd, "directory", O_CREAT ...)` fails with `errno` set
  to `EINVAL` instead of the POSIX-required `EISDIR` on some
  platforms:
  Solaris 11.4.

---

Next: [`posix_fallocate`](#posix_005ffallocate), Previous: [`openat`](#openat), Up: [Functions in `<fcntl.h>`](#Functions-in-_003cfcntl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.11.5 `posix_fadvise` [¶](#posix_005ffadvise-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_fadvise.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly across the entire data range of files 2 GiB and larger.
  See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11.23, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 4.4.

---

Previous: [`posix_fadvise`](#posix_005ffadvise), Up: [Functions in `<fcntl.h>`](#Functions-in-_003cfcntl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.11.6 `posix_fallocate` [¶](#posix_005ffallocate-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_fallocate.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly across the entire data range of files 2 GiB and larger.
  See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 4.4.
* POSIX specifies that `EINVAL` should be returned when the file system doesn’t
  support the allocation operation directly. glibc however emulates the file system
  allocation with writes where unsupported, and apps have depended on
  that long term implementation. This is both inefficient, and as of
  glibc 2.21 at least, buggy on certain NFS setups.

---

Next: [Functions in `<fmtmsg.h>`](#Functions-in-_003cfmtmsg_002eh_003e), Previous: [Functions in `<fcntl.h>`](#Functions-in-_003cfcntl_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
