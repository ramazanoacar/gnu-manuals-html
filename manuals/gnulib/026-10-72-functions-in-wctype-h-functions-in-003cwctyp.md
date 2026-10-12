* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11.00, Cygwin 1.5.x.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.38, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.2,
  AIX 7.2, HP-UX 11, Solaris 11.4, Cygwin 2.9.0, mingw, MSVC 14.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [Functions in `<wordexp.h>`](#Functions-in-_003cwordexp_002eh_003e), Previous: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.72 Functions in `<wctype.h>` [¶](#Functions-in-_003cwctype_002eh_003e-1)

* [`iswalnum`](#iswalnum)
* [`iswalnum_l`](#iswalnum_005fl)
* [`iswalpha`](#iswalpha)
* [`iswalpha_l`](#iswalpha_005fl)
* [`iswblank`](#iswblank)
* [`iswblank_l`](#iswblank_005fl)
* [`iswcntrl`](#iswcntrl)
* [`iswcntrl_l`](#iswcntrl_005fl)
* [`iswctype`](#iswctype)
* [`iswctype_l`](#iswctype_005fl)
* [`iswdigit`](#iswdigit)
* [`iswdigit_l`](#iswdigit_005fl)
* [`iswgraph`](#iswgraph)
* [`iswgraph_l`](#iswgraph_005fl)
* [`iswlower`](#iswlower)
* [`iswlower_l`](#iswlower_005fl)
* [`iswprint`](#iswprint)
* [`iswprint_l`](#iswprint_005fl)
* [`iswpunct`](#iswpunct)
* [`iswpunct_l`](#iswpunct_005fl)
* [`iswspace`](#iswspace)
* [`iswspace_l`](#iswspace_005fl)
* [`iswupper`](#iswupper)
* [`iswupper_l`](#iswupper_005fl)
* [`iswxdigit`](#iswxdigit)
* [`iswxdigit_l`](#iswxdigit_005fl)
* [`towctrans`](#towctrans)
* [`towctrans_l`](#towctrans_005fl)
* [`towlower`](#towlower)
* [`towlower_l`](#towlower_005fl)
* [`towupper`](#towupper)
* [`towupper_l`](#towupper_005fl)
* [`wctrans`](#wctrans)
* [`wctrans_l`](#wctrans_005fl)
* [`wctype`](#wctype)
* [`wctype_l`](#wctype_005fl)

---

Next: [`iswalnum_l`](#iswalnum_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.1 `iswalnum` [¶](#iswalnum-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswalnum.html>

Gnulib module: wctype-h

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32isalnum`, provided by Gnulib module
  `c32isalnum`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`iswalpha`](#iswalpha), Previous: [`iswalnum`](#iswalnum), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.2 `iswalnum_l` [¶](#iswalnum_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswalnum_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`iswalpha_l`](#iswalpha_005fl), Previous: [`iswalnum_l`](#iswalnum_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.3 `iswalpha` [¶](#iswalpha-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswalpha.html>

Gnulib module: wctype-h

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32isalpha`, provided by Gnulib module
  `c32isalpha`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`iswblank`](#iswblank), Previous: [`iswalpha`](#iswalpha), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.4 `iswalpha_l` [¶](#iswalpha_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswalpha_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`iswblank_l`](#iswblank_005fl), Previous: [`iswalpha_l`](#iswalpha_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.5 `iswblank` [¶](#iswblank-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswblank.html>

Gnulib module: iswblank

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Solaris 9, mingw, MSVC 9, Android 4.4.
* This function is not declared (without `-D_GNU_SOURCE`) on some platforms:
  glibc 2.8.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32isblank`, provided by Gnulib module
  `c32isblank`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`iswcntrl`](#iswcntrl), Previous: [`iswblank`](#iswblank), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.6 `iswblank_l` [¶](#iswblank_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswblank_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`iswcntrl_l`](#iswcntrl_005fl), Previous: [`iswblank_l`](#iswblank_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.7 `iswcntrl` [¶](#iswcntrl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswcntrl.html>

Gnulib module: wctype-h

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32iscntrl`, provided by Gnulib module
  `c32iscntrl`, operates on 32-bit wide characters and therefore does not
  have this limitation.
* This function returns 0 for U+2028 (LINE SEPARATOR) and
  U+2029 (PARAGRAPH SEPARATOR) on some platforms:
  macOS 14.

---

Next: [`iswctype`](#iswctype), Previous: [`iswcntrl`](#iswcntrl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.8 `iswcntrl_l` [¶](#iswcntrl_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswcntrl_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`iswctype_l`](#iswctype_005fl), Previous: [`iswcntrl_l`](#iswcntrl_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.9 `iswctype` [¶](#iswctype-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswctype.html>

Gnulib module: iswctype

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.
* This function is declared in `<wchar.h>`, not in `<wctype.h>`, on
  some platforms:
  HP-UX 11.00.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
  However, the Gnulib function `c32_apply_type_test`, provided by Gnulib
  module `c32_apply_type_test`, operates on 32-bit wide characters and
  therefore does not have this limitation.

---

Next: [`iswdigit`](#iswdigit), Previous: [`iswctype`](#iswctype), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.10 `iswctype_l` [¶](#iswctype_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswctype_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`iswdigit_l`](#iswdigit_005fl), Previous: [`iswctype_l`](#iswctype_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.11 `iswdigit` [¶](#iswdigit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswdigit.html>

Gnulib module: iswdigit

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.
* This function is not ISO C 99 compliant on some platforms:
  FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Solaris 11.4, mingw, MSVC 14, Haiku, Android.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32isdigit`, provided by Gnulib module
  `c32isdigit`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`iswgraph`](#iswgraph), Previous: [`iswdigit`](#iswdigit), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.12 `iswdigit_l` [¶](#iswdigit_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswdigit_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`iswgraph_l`](#iswgraph_005fl), Previous: [`iswdigit_l`](#iswdigit_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.13 `iswgraph` [¶](#iswgraph-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswgraph.html>

Gnulib module: wctype-h

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32isgraph`, provided by Gnulib module
  `c32isgraph`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`iswlower`](#iswlower), Previous: [`iswgraph`](#iswgraph), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.14 `iswgraph_l` [¶](#iswgraph_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswgraph_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`iswlower_l`](#iswlower_005fl), Previous: [`iswgraph_l`](#iswgraph_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.15 `iswlower` [¶](#iswlower-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswlower.html>

Gnulib module: wctype-h

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32islower`, provided by Gnulib module
  `c32islower`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`iswprint`](#iswprint), Previous: [`iswlower`](#iswlower), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.16 `iswlower_l` [¶](#iswlower_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswlower_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`iswprint_l`](#iswprint_005fl), Previous: [`iswlower_l`](#iswlower_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.17 `iswprint` [¶](#iswprint-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswprint.html>

Gnulib module: wctype-h

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.
* This function returns true for the tab (`'\t'`) character
  on some platforms:
  mingw.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32isprint`, provided by Gnulib module
  `c32isprint`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`iswpunct`](#iswpunct), Previous: [`iswprint`](#iswprint), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.18 `iswprint_l` [¶](#iswprint_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswprint_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`iswpunct_l`](#iswpunct_005fl), Previous: [`iswprint_l`](#iswprint_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.19 `iswpunct` [¶](#iswpunct-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswpunct.html>

Gnulib module: iswpunct

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.
* This function is inconsistent with the `ispunct` function, because it
  returns false for the characters `'$'`, `'+'`, `'<'`,
  `'='`, `'>'`, `'^'`, `` '`' `` , `'|'`, `'~'`
  on some platforms:
  Android 11.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32ispunct`, provided by Gnulib module
  `c32ispunct`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`iswspace`](#iswspace), Previous: [`iswpunct`](#iswpunct), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.20 `iswpunct_l` [¶](#iswpunct_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswpunct_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`iswspace_l`](#iswspace_005fl), Previous: [`iswpunct_l`](#iswpunct_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.21 `iswspace` [¶](#iswspace-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswspace.html>

Gnulib module: wctype-h

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32isspace`, provided by Gnulib module
  `c32isspace`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`iswupper`](#iswupper), Previous: [`iswspace`](#iswspace), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.22 `iswspace_l` [¶](#iswspace_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswspace_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`iswupper_l`](#iswupper_005fl), Previous: [`iswspace_l`](#iswspace_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.23 `iswupper` [¶](#iswupper-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswupper.html>

Gnulib module: wctype-h

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32isupper`, provided by Gnulib module
  `c32isupper`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`iswxdigit`](#iswxdigit), Previous: [`iswupper`](#iswupper), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.24 `iswupper_l` [¶](#iswupper_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswupper_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`iswxdigit_l`](#iswxdigit_005fl), Previous: [`iswupper_l`](#iswupper_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.25 `iswxdigit` [¶](#iswxdigit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswxdigit.html>

Gnulib module: iswxdigit

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.
* This function is not ISO C 99 compliant on some platforms:
  FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Solaris 11.4, MSVC 14, Haiku, Android.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32isxdigit`, provided by Gnulib module
  `c32isxdigit`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`towctrans`](#towctrans), Previous: [`iswxdigit`](#iswxdigit), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.26 `iswxdigit_l` [¶](#iswxdigit_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iswxdigit_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`towctrans_l`](#towctrans_005fl), Previous: [`iswxdigit_l`](#iswxdigit_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.27 `towctrans` [¶](#towctrans-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/towctrans.html>

Gnulib module: towctrans

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, mingw, MSVC 9, Android 7.1.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
  However, the Gnulib function `c32_apply_mapping`, provided by Gnulib
  module `c32_apply_mapping`, operates on 32-bit wide characters and
  therefore does not have this limitation.

---

Next: [`towlower`](#towlower), Previous: [`towctrans`](#towctrans), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.28 `towctrans_l` [¶](#towctrans_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/towctrans_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 7.1.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`towlower_l`](#towlower_005fl), Previous: [`towctrans_l`](#towctrans_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.29 `towlower` [¶](#towlower-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/towlower.html>

Gnulib module: wctype-h

Portability problems fixed by Gnulib:

* This function returns values of which the upper 16 bits are incorrect
  on some platforms:
  mingw.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
  However, the Gnulib function `c32tolower`, provided by Gnulib module
  `c32tolower`, operates on 32-bit wide characters and therefore does not
  have this limitation.
* On native Windows, in an UTF-8 locale, this function does not even do
  the simple expected mappings, such as from 0x00C9 to 0x00E9.
* This function returns wrong values even for the ASCII characters
  in a zh\_CN.GB18030 locale on some platforms:
  NetBSD 10.0.

---

Next: [`towupper`](#towupper), Previous: [`towlower`](#towlower), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.30 `towlower_l` [¶](#towlower_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/towlower_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`towupper_l`](#towupper_005fl), Previous: [`towlower_l`](#towlower_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.31 `towupper` [¶](#towupper-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/towupper.html>

Gnulib module: wctype-h

Portability problems fixed by Gnulib:

* This function returns values of which the upper 16 bits are incorrect
  on some platforms:
  mingw.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
  However, the Gnulib function `c32toupper`, provided by Gnulib module
  `c32toupper`, operates on 32-bit wide characters and therefore does not
  have this limitation.
* On native Windows, in an UTF-8 locale, this function does not even do
  the simple expected mappings, such as from 0x00E9 to 0x00C9.
* This function returns wrong values even for the ASCII characters
  in a zh\_CN.GB18030 locale on some platforms:
  NetBSD 10.0.

---

Next: [`wctrans`](#wctrans), Previous: [`towupper`](#towupper), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.32 `towupper_l` [¶](#towupper_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/towupper_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wctrans_l`](#wctrans_005fl), Previous: [`towupper_l`](#towupper_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.33 `wctrans` [¶](#wctrans-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wctrans.html>

Gnulib module: wctrans

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, mingw, MSVC 9, Android 7.1.
* This function does not support the `"tolower"` and `"toupper"`
  mappings on some platforms:
  NetBSD 10.0.
  (It returns non-null values for the arguments `"towlower"` and
  `"towupper"`, but with these values, the function `towctrans`
  always crashes.)

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
  However, the Gnulib function `c32_get_mapping`, provided by Gnulib module
  `c32_get_mapping`, operates on 32-bit wide characters and therefore does
  not have this limitation.

---

Next: [`wctype`](#wctype), Previous: [`wctrans`](#wctrans), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.34 `wctrans_l` [¶](#wctrans_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wctrans_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 7.1.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wctype_l`](#wctype_005fl), Previous: [`wctrans_l`](#wctrans_005fl), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.35 `wctype` [¶](#wctype-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wctype.html>

Gnulib module: wctype

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, MSVC 9.
* This function is declared in `<wchar.h>`, not in `<wctype.h>`, on
  some platforms:
  HP-UX 11.00.
* This function does not support the `"blank"` character class
  on some platforms:
  mingw.
* The object returned by this function for the `"blank"` character class
  is inconsistent with the `iswblank` and `isblank` functions
  on some platforms:
  MSVC 14.
* The object returned by this function for the `"punct"` character class
  is inconsistent with the `ispunct` function on some platforms:
  Android 11.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
  However, the Gnulib function `c32_get_type_test`, provided by Gnulib
  module `c32_get_type_test`, operates on 32-bit wide characters and
  therefore does not have this limitation.

---

Previous: [`wctype`](#wctype), Up: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.72.36 `wctype_l` [¶](#wctype_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wctype_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Previous: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.73 Functions in `<wordexp.h>` [¶](#Functions-in-_003cwordexp_002eh_003e-1)

* [`wordexp`](#wordexp)
* [`wordfree`](#wordfree)

---

Next: [`wordfree`](#wordfree), Up: [Functions in `<wordexp.h>`](#Functions-in-_003cwordexp_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.73.1 `wordexp` [¶](#wordexp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wordexp.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Previous: [`wordexp`](#wordexp), Up: [Functions in `<wordexp.h>`](#Functions-in-_003cwordexp_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.73.2 `wordfree` [¶](#wordfree-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wordfree.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes), Previous: [ISO C and POSIX Function Substitutes](#Function-Substitutes), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 11 Past POSIX Header File Substitutes [¶](#Past-POSIX-Header-File-Substitutes)

This chapter describes which header files specified by older versions of
POSIX are substituted by Gnulib, which portability pitfalls are fixed by
Gnulib, and which (known) portability problems are not worked around by
Gnulib.

The notation “Gnulib module: —” means that Gnulib does not provide a
module providing a substitute for the header file. When the list
“Portability problems not fixed by Gnulib” is empty, such a module is
not needed: No portability problems are known. Otherwise, it indicates
that such a module would be useful but is not available: No one so far
found this header file important enough to contribute a substitute for it.
If you need this particular header file, you may write to
`<bug-gnulib at gnu dot org>`.

* [`stropts.h`](#stropts_002eh)
* [`sys/timeb.h`](#sys_002ftimeb_002eh)
* [`trace.h`](#trace_002eh)
* [`ucontext.h`](#ucontext_002eh)
* [`ulimit.h`](#ulimit_002eh)
* [`utime.h`](#utime_002eh)

---

Next: [`sys/timeb.h`](#sys_002ftimeb_002eh), Up: [Past POSIX Header File Substitutes](#Legacy-Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 11.1 `stropts.h` [¶](#stropts_002eh-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/stropts.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Cygwin, mingw, MSVC 14, Android 9.0.
* This header is removed in POSIX.1-2024.

---

Next: [`trace.h`](#trace_002eh), Previous: [`stropts.h`](#stropts_002eh), Up: [Past POSIX Header File Substitutes](#Legacy-Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 11.2 `sys/timeb.h` [¶](#sys_002ftimeb_002eh-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification:  
 <https://pubs.opengroup.org/onlinepubs/009695399/basedefs/sys/timeb.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  OpenBSD 7.9, Android 9.0.

---

Next: [`ucontext.h`](#ucontext_002eh), Previous: [`sys/timeb.h`](#sys_002ftimeb_002eh), Up: [Past POSIX Header File Substitutes](#Legacy-Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 11.3 `trace.h` [¶](#trace_002eh-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/trace.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin, mingw, MSVC 14, Android 9.0.
* This header is removed in POSIX.1-2024.

---

Next: [`ulimit.h`](#ulimit_002eh), Previous: [`trace.h`](#trace_002eh), Up: [Past POSIX Header File Substitutes](#Legacy-Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 11.4 `ucontext.h` [¶](#ucontext_002eh-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification:  
 <https://pubs.opengroup.org/onlinepubs/009695399/basedefs/ucontext.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  OpenBSD 7.9, Cygwin 1.7.35, mingw, MSVC 14.

---

Next: [`utime.h`](#utime_002eh), Previous: [`ucontext.h`](#ucontext_002eh), Up: [Past POSIX Header File Substitutes](#Legacy-Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 11.5 `ulimit.h` [¶](#ulimit_002eh-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/ulimit.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, Cygwin, mingw, MSVC 14, Android 9.0.
* This header is removed in POSIX.1-2024.

---

Previous: [`ulimit.h`](#ulimit_002eh), Up: [Past POSIX Header File Substitutes](#Legacy-Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 11.6 `utime.h` [¶](#utime_002eh-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/utime.h.html>

Gnulib module: utime-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms: MSVC 14.

Portability problems not fixed by Gnulib:

* This header is removed in POSIX.1-2024.

---

Next: [Glibc Header File Substitutes](#Glibc-Header-File-Substitutes), Previous: [Past POSIX Header File Substitutes](#Legacy-Header-File-Substitutes), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 12 Past POSIX Function Substitutes [¶](#Past-POSIX-Function-Substitutes)

This chapter describes which functions and function-like macros specified by
older versions of POSIX are substituted by Gnulib, which
portability pitfalls are fixed by Gnulib, and which (known) portability
problems are not worked around by Gnulib.

The notation “Gnulib module: —” means that Gnulib does not provide a
module providing a substitute for the function. When the list
“Portability problems not fixed by Gnulib” is empty, such a module is
not needed: No portability problems are known. Otherwise, it indicates
that such a module would be useful but is not available: No one so far
found this function important enough to contribute a substitute for it.
If you need this particular function, you may write to
`<bug-gnulib at gnu dot org>`.

* [`_longjmp`](#g_t_005flongjmp)
* [`_setjmp`](#g_t_005fsetjmp)
* [`_tolower`](#g_t_005ftolower)
* [`_toupper`](#g_t_005ftoupper)
* [`bcmp`](#bcmp)
* [`bcopy`](#bcopy)
* [`bsd_signal`](#bsd_005fsignal)
* [`bzero`](#bzero)
* [`ecvt`](#ecvt)
* [`fattach`](#fattach)
* [`fcvt`](#fcvt)
* [`fdetach`](#fdetach)
* [`ftime`](#ftime)
* [`ftw`](#ftw)
* [`gcvt`](#gcvt)
* [`getcontext`](#getcontext)
* [`gethostbyaddr`](#gethostbyaddr)
* [`gethostbyname`](#gethostbyname)
* [`getitimer`](#getitimer)
* [`getmsg`](#getmsg)
* [`getpmsg`](#getpmsg)
* [`gets`](#gets)
* [`gettimeofday`](#gettimeofday)
* [`getwd`](#getwd)
* [`h_errno`](#h_005ferrno)
* [`index`](#g_t_005findex)
* [`ioctl`](#ioctl)
* [`isascii`](#isascii)
* [`isastream`](#isastream)
* [`makecontext`](#makecontext)
* [`mktemp`](#mktemp)
* [`posix_trace_attr_destroy`](#posix_005ftrace_005fattr_005fdestroy)
* [`posix_trace_attr_getclockres`](#posix_005ftrace_005fattr_005fgetclockres)
* [`posix_trace_attr_getcreatetime`](#posix_005ftrace_005fattr_005fgetcreatetime)
* [`posix_trace_attr_getgenversion`](#posix_005ftrace_005fattr_005fgetgenversion)
* [`posix_trace_attr_getinherited`](#posix_005ftrace_005fattr_005fgetinherited)
* [`posix_trace_attr_getlogfullpolicy`](#posix_005ftrace_005fattr_005fgetlogfullpolicy)
* [`posix_trace_attr_getlogsize`](#posix_005ftrace_005fattr_005fgetlogsize)
* [`posix_trace_attr_getmaxdatasize`](#posix_005ftrace_005fattr_005fgetmaxdatasize)
* [`posix_trace_attr_getmaxsystemeventsize`](#posix_005ftrace_005fattr_005fgetmaxsystemeventsize)
* [`posix_trace_attr_getmaxusereventsize`](#posix_005ftrace_005fattr_005fgetmaxusereventsize)
* [`posix_trace_attr_getname`](#posix_005ftrace_005fattr_005fgetname)
* [`posix_trace_attr_getstreamfullpolicy`](#posix_005ftrace_005fattr_005fgetstreamfullpolicy)
* [`posix_trace_attr_getstreamsize`](#posix_005ftrace_005fattr_005fgetstreamsize)
* [`posix_trace_attr_init`](#posix_005ftrace_005fattr_005finit)
* [`posix_trace_attr_setinherited`](#posix_005ftrace_005fattr_005fsetinherited)
* [`posix_trace_attr_setlogfullpolicy`](#posix_005ftrace_005fattr_005fsetlogfullpolicy)
* [`posix_trace_attr_setlogsize`](#posix_005ftrace_005fattr_005fsetlogsize)
* [`posix_trace_attr_setmaxdatasize`](#posix_005ftrace_005fattr_005fsetmaxdatasize)
* [`posix_trace_attr_setname`](#posix_005ftrace_005fattr_005fsetname)
* [`posix_trace_attr_setstreamfullpolicy`](#posix_005ftrace_005fattr_005fsetstreamfullpolicy)
* [`posix_trace_attr_setstreamsize`](#posix_005ftrace_005fattr_005fsetstreamsize)
* [`posix_trace_clear`](#posix_005ftrace_005fclear)
* [`posix_trace_close`](#posix_005ftrace_005fclose)
* [`posix_trace_create`](#posix_005ftrace_005fcreate)
* [`posix_trace_create_withlog`](#posix_005ftrace_005fcreate_005fwithlog)
* [`posix_trace_event`](#posix_005ftrace_005fevent)
* [`posix_trace_eventid_equal`](#posix_005ftrace_005feventid_005fequal)
* [`posix_trace_eventid_get_name`](#posix_005ftrace_005feventid_005fget_005fname)
* [`posix_trace_eventid_open`](#posix_005ftrace_005feventid_005fopen)
* [`posix_trace_eventset_add`](#posix_005ftrace_005feventset_005fadd)
* [`posix_trace_eventset_del`](#posix_005ftrace_005feventset_005fdel)
* [`posix_trace_eventset_empty`](#posix_005ftrace_005feventset_005fempty)
* [`posix_trace_eventset_fill`](#posix_005ftrace_005feventset_005ffill)
* [`posix_trace_eventset_ismember`](#posix_005ftrace_005feventset_005fismember)
* [`posix_trace_eventtypelist_getnext_id`](#posix_005ftrace_005feventtypelist_005fgetnext_005fid)
* [`posix_trace_eventtypelist_rewind`](#posix_005ftrace_005feventtypelist_005frewind)
* [`posix_trace_flush`](#posix_005ftrace_005fflush)
* [`posix_trace_get_attr`](#posix_005ftrace_005fget_005fattr)
* [`posix_trace_get_filter`](#posix_005ftrace_005fget_005ffilter)
* [`posix_trace_get_status`](#posix_005ftrace_005fget_005fstatus)
* [`posix_trace_getnext_event`](#posix_005ftrace_005fgetnext_005fevent)
* [`posix_trace_open`](#posix_005ftrace_005fopen)
* [`posix_trace_rewind`](#posix_005ftrace_005frewind)
* [`posix_trace_set_filter`](#posix_005ftrace_005fset_005ffilter)
* [`posix_trace_shutdown`](#posix_005ftrace_005fshutdown)
* [`posix_trace_start`](#posix_005ftrace_005fstart)
* [`posix_trace_stop`](#posix_005ftrace_005fstop)
* [`posix_trace_timedgetnext_event`](#posix_005ftrace_005ftimedgetnext_005fevent)
* [`posix_trace_trid_eventid_open`](#posix_005ftrace_005ftrid_005feventid_005fopen)
* [`posix_trace_trygetnext_event`](#posix_005ftrace_005ftrygetnext_005fevent)
* [`pthread_attr_getstackaddr`](#pthread_005fattr_005fgetstackaddr)
* [`pthread_attr_setstackaddr`](#pthread_005fattr_005fsetstackaddr)
* [`pthread_getconcurrency`](#pthread_005fgetconcurrency)
* [`pthread_setconcurrency`](#pthread_005fsetconcurrency)
* [`putmsg`](#putmsg)
* [`putpmsg`](#putpmsg)
* [`rindex`](#rindex)
* [`scalb`](#scalb)
* [`setcontext`](#setcontext)
* [`setitimer`](#setitimer)
* [`setpgrp`](#setpgrp)
* [`sighold`](#sighold)
* [`sigignore`](#sigignore)
* [`siginterrupt`](#siginterrupt)
* [`sigpause`](#sigpause)
* [`sigrelse`](#sigrelse)
* [`sigset`](#sigset)
* [`swapcontext`](#swapcontext)
* [`tempnam`](#tempnam)
* [`toascii`](#toascii)
* [`ualarm`](#ualarm)
* [`ulimit`](#ulimit)
* [`usleep`](#usleep)
* [`utime`](#utime)
* [`vfork`](#vfork)
* [`wcswcs`](#wcswcs)

---

Next: [`_setjmp`](#g_t_005fsetjmp), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.1 `_longjmp` [¶](#g_t_005flongjmp-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/_longjmp.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function is removed in POSIX.1-2024.

Note: Despite its being removed from POSIX, in 2024 on all
systems which have `_setjmp`, it is the fastest way to save the
registers but not the signal mask (up to 30 times faster than `setjmp`
on some systems).

---

Next: [`_tolower`](#g_t_005ftolower), Previous: [`_longjmp`](#g_t_005flongjmp), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.2 `_setjmp` [¶](#g_t_005fsetjmp-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/_setjmp.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is removed in POSIX.1-2024.

Note: Despite its being removed from POSIX, in 2024 on all
systems which have `_setjmp`, it is the fastest way to save the
registers but not the signal mask (up to 30 times faster than `setjmp`
on some systems).

---

Next: [`_toupper`](#g_t_005ftoupper), Previous: [`_setjmp`](#g_t_005fsetjmp), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.3 `_tolower` [¶](#g_t_005ftolower-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/_tolower.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, Minix 3.1.8, Android 4.4.
* This function is removed in POSIX.1-2024.
  Use the function `tolower` instead.

---

Next: [`bcmp`](#bcmp), Previous: [`_tolower`](#g_t_005ftolower), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.4 `_toupper` [¶](#g_t_005ftoupper-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/_toupper.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, Minix 3.1.8, Android 4.4.
* This function is removed in POSIX.1-2024.
  Use the function `toupper` instead.

---

Next: [`bcopy`](#bcopy), Previous: [`_toupper`](#g_t_005ftoupper), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.5 `bcmp` [¶](#bcmp-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/bcmp.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 9.0.
* This function is removed in POSIX.1-2008. Better use `memcmp`
  instead.

---

Next: [`bsd_signal`](#bsd_005fsignal), Previous: [`bcmp`](#bcmp), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.6 `bcopy` [¶](#bcopy-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/bcopy.html>

Gnulib module: bcopy

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

* This function is removed in POSIX.1-2008. Better use `memcpy`
  or `memmove` instead.
* This module does not arrange to declare this function.

---

Next: [`bzero`](#bzero), Previous: [`bcopy`](#bcopy), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.7 `bsd_signal` [¶](#bsd_005fsignal-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/bsd_signal.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.3, FreeBSD 14.0, NetBSD 7.1, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`ecvt`](#ecvt), Previous: [`bsd_signal`](#bsd_005fsignal), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.8 `bzero` [¶](#bzero-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/bzero.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 9.0.
* This function is removed in POSIX.1-2008. Better use `memset`
  instead.

---

Next: [`fattach`](#fattach), Previous: [`bzero`](#bzero), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.9 `ecvt` [¶](#ecvt-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/ecvt.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 14.0, NetBSD 10.0, Android 9.0.
* This function is not declared on some platforms:
  Cygwin 2.9.
* This function is removed in POSIX.1-2008. Better use `sprintf`
  instead.

---

Next: [`fcvt`](#fcvt), Previous: [`ecvt`](#ecvt), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.10 `fattach` [¶](#fattach-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/fattach.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`fdetach`](#fdetach), Previous: [`fattach`](#fattach), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.11 `fcvt` [¶](#fcvt-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/fcvt.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 14.0, NetBSD 10.0, Android 9.0.
* This function is not declared on some platforms:
  Cygwin 2.9.
* This function is removed in POSIX.1-2008. Better use `sprintf`
  instead.

---

Next: [`ftime`](#ftime), Previous: [`fcvt`](#fcvt), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.12 `fdetach` [¶](#fdetach-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/fdetach.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ftw`](#ftw), Previous: [`fdetach`](#fdetach), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.13 `ftime` [¶](#ftime-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/ftime.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.3, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Android 9.0.
* This function is not declared on some platforms:
  Android 13.
* On native Windows platforms (mingw, MSVC), this function works incorrectly
  when the environment variable `TZ` has been set by Cygwin.
* This function is removed in POSIX.1-2008. Better use
  `clock_gettime` instead, and use `ftime` only as a fallback for
  portability to Windows platforms.

---

Next: [`gcvt`](#gcvt), Previous: [`ftime`](#ftime), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.14 `ftw` [¶](#ftw-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/ftw.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 3.0, Minix 3.1.8, mingw, MSVC 14, Android 4.1.
* This function is removed in POSIX.1-2024.
  Use the Gnulib module `fts` instead.

---

Next: [`getcontext`](#getcontext), Previous: [`ftw`](#ftw), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.15 `gcvt` [¶](#gcvt-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/gcvt.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.3, FreeBSD 14.0, NetBSD 10.0, Android 9.0.
* This function is not declared on some platforms:
  Cygwin 2.9.
* This function is removed in POSIX.1-2008. Better use `sprintf`
  instead.

---

Next: [`gethostbyaddr`](#gethostbyaddr), Previous: [`gcvt`](#gcvt), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.16 `getcontext` [¶](#getcontext-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/getcontext.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.

Note: A third-party implementation is available at
<https://github.com/kaniini/libucontext/>.

---

Next: [`gethostbyname`](#gethostbyname), Previous: [`getcontext`](#getcontext), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.17 `gethostbyaddr` [¶](#gethostbyaddr-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/gethostbyaddr.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is declared in `<winsock2.h>` instead of `<netdb.h>`
  on some platforms:
  mingw, MSVC 14.

---

Next: [`getitimer`](#getitimer), Previous: [`gethostbyaddr`](#gethostbyaddr), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.18 `gethostbyname` [¶](#gethostbyname-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/gethostbyname.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is declared in `<winsock2.h>` instead of `<netdb.h>`
  on some platforms:
  mingw, MSVC 14.

---

Next: [`getmsg`](#getmsg), Previous: [`gethostbyname`](#gethostbyname), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.19 `getitimer` [¶](#getitimer-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/getitimer.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function is removed in POSIX.1-2024.
  Use the function `timer_gettime` instead.

---

Next: [`getpmsg`](#getpmsg), Previous: [`getitimer`](#getitimer), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.20 `getmsg` [¶](#getmsg-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/getmsg.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`gets`](#gets), Previous: [`getmsg`](#getmsg), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.21 `getpmsg` [¶](#getpmsg-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/getpmsg.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`gettimeofday`](#gettimeofday), Previous: [`getpmsg`](#getpmsg), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.22 `gets` [¶](#gets-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/gets.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is removed in POSIX.1-2024.
  Use the function `fgets` instead.
* This function should never be used, because it can overflow any given buffer.
* This function is missing on some platforms:
  OpenBSD 7.9, Solaris 11.4.
* When reading from a non-blocking pipe whose buffer is empty, this function
  fails with `errno` being set to `EINVAL` instead of `EAGAIN` on
  some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.

---

Next: [`getwd`](#getwd), Previous: [`gets`](#gets), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.23 `gettimeofday` [¶](#gettimeofday-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/gettimeofday.html>

Gnulib module: gettimeofday

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 14.
* This function is declared with a nonstandard function prototype (only one
  argument, or “...” after the first argument) on some platforms.
* On some platforms, the second argument has type `struct
  timezone*` rather than `void *`, making it an error to redeclare
  the function with the POSIX signature:
  glibc.
  However, rather than penalize these systems with a replacement
  function, gnulib defines `GETTIMEOFDAY_TIMEZONE` to the
  appropriate type for use in avoiding a compiler warning if assigning
  `gettimeofday` to a function pointer.
* This function has only a precision of 15.6 milliseconds on some platforms:
  mingw.

Portability problems not fixed by Gnulib:

* Behavior is non-portable if the second argument to `gettimeofday`
  is not `NULL`.
* This function is removed in POSIX.1-2024.

  Use the Gnulib module `gettime` or `timespec_get` instead.
  (POSIX recommends to use the function `clock_gettime`, but there is
  no corresponding Gnulib module for it yet.)

---

Next: [`h_errno`](#h_005ferrno), Previous: [`gettimeofday`](#gettimeofday), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.24 `getwd` [¶](#getwd-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/getwd.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 9.0.
* The size of the buffer required for this function is not a compile-time
  constant. Also, the function truncates a result that would be larger than
  the minimum buffer size. For these reasons, this function is removed
  in POSIX.1-2008. Better use the `getcwd` function instead.

---

Next: [`index`](#g_t_005findex), Previous: [`getwd`](#getwd), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.25 `h_errno` [¶](#h_005ferrno-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/h_errno.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.34, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ioctl`](#ioctl), Previous: [`h_errno`](#h_005ferrno), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.26 `index` [¶](#index)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/index.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 9.0.
* This function is not declared on some platforms:
  Android 13.
* This function is removed in POSIX.1-2008. Better use `strchr`
  instead.

---

Next: [`isascii`](#isascii), Previous: [`index`](#g_t_005findex), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.27 `ioctl` [¶](#ioctl-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/ioctl.html>

LSB specification:

* <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-ioctl-2.html>
* <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-ttyio-2.html>
* <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-sockio-2.html>

Gnulib module: ioctl

Portability problems fixed by Gnulib:

* On Windows platforms (excluding Cygwin), `ioctl` is called
  `ioctlsocket`, and error codes from this function are not placed in
  `errno`, and `WSAGetLastError` must be used instead.
* The second parameter is of type `unsigned long` rather than `int`
  on some platforms:
  glibc 2.26, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Haiku 2017.

Portability problems not fixed by Gnulib:

* Most `ioctl` requests are platform and hardware specific.

---

Next: [`isastream`](#isastream), Previous: [`ioctl`](#ioctl), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.28 `isascii` [¶](#isascii-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/isascii.html>

Gnulib module: —

Portability problems not fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.
* This function is removed in POSIX.1-2024.

Note: This function’s behaviour depends on the locale, but requires special
handling for the multibyte characters that occur in strings in locales with
`MB_CUR_MAX > 1` (this includes all the common UTF-8 locales).
There are two alternative APIs:

`c_isascii` [¶](#index-c_002dctype-14)
:   This function operates in a locale independent way and returns true only for
    ASCII characters. It is provided by the Gnulib module ‘`c-ctype`’.

`mb_isascii` [¶](#index-mbchar-12)
:   This function operates in a locale dependent way, on multibyte characters.
    It is provided by the Gnulib module ‘`mbchar`’.

---

Next: [`makecontext`](#makecontext), Previous: [`isascii`](#isascii), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.29 `isastream` [¶](#isastream-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/isastream.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`mktemp`](#mktemp), Previous: [`isastream`](#isastream), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.30 `makecontext` [¶](#makecontext-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/makecontext.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.

Note: A third-party implementation is available at
<https://github.com/kaniini/libucontext/>.

---

Next: [`posix_trace_attr_destroy`](#posix_005ftrace_005fattr_005fdestroy), Previous: [`makecontext`](#makecontext), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.31 `mktemp` [¶](#mktemp-1)

Removed in POSIX.1-2008.
POSIX.1-2004 specification: <https://pubs.opengroup.org/onlinepubs/009695399/functions/mktemp.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is not appropriate for creating temporary files. (It has
  security risks.) Therefore it is removed in POSIX.1-2008. Better use
  `mkstemp` instead.

---

Next: [`posix_trace_attr_getclockres`](#posix_005ftrace_005fattr_005fgetclockres), Previous: [`mktemp`](#mktemp), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.32 `posix_trace_attr_destroy` [¶](#posix_005ftrace_005fattr_005fdestroy-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_destroy.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`posix_trace_attr_getcreatetime`](#posix_005ftrace_005fattr_005fgetcreatetime), Previous: [`posix_trace_attr_destroy`](#posix_005ftrace_005fattr_005fdestroy), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.33 `posix_trace_attr_getclockres` [¶](#posix_005ftrace_005fattr_005fgetclockres-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getclockres.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`posix_trace_attr_getgenversion`](#posix_005ftrace_005fattr_005fgetgenversion), Previous: [`posix_trace_attr_getclockres`](#posix_005ftrace_005fattr_005fgetclockres), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.34 `posix_trace_attr_getcreatetime` [¶](#posix_005ftrace_005fattr_005fgetcreatetime-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getcreatetime.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`posix_trace_attr_getinherited`](#posix_005ftrace_005fattr_005fgetinherited), Previous: [`posix_trace_attr_getcreatetime`](#posix_005ftrace_005fattr_005fgetcreatetime), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.35 `posix_trace_attr_getgenversion` [¶](#posix_005ftrace_005fattr_005fgetgenversion-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getgenversion.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`posix_trace_attr_getlogfullpolicy`](#posix_005ftrace_005fattr_005fgetlogfullpolicy), Previous: [`posix_trace_attr_getgenversion`](#posix_005ftrace_005fattr_005fgetgenversion), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.36 `posix_trace_attr_getinherited` [¶](#posix_005ftrace_005fattr_005fgetinherited-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getinherited.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`posix_trace_attr_getlogsize`](#posix_005ftrace_005fattr_005fgetlogsize), Previous: [`posix_trace_attr_getinherited`](#posix_005ftrace_005fattr_005fgetinherited), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.37 `posix_trace_attr_getlogfullpolicy` [¶](#posix_005ftrace_005fattr_005fgetlogfullpolicy-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getlogfullpolicy.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`posix_trace_attr_getmaxdatasize`](#posix_005ftrace_005fattr_005fgetmaxdatasize), Previous: [`posix_trace_attr_getlogfullpolicy`](#posix_005ftrace_005fattr_005fgetlogfullpolicy), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.38 `posix_trace_attr_getlogsize` [¶](#posix_005ftrace_005fattr_005fgetlogsize-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getlogsize.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`posix_trace_attr_getmaxsystemeventsize`](#posix_005ftrace_005fattr_005fgetmaxsystemeventsize), Previous: [`posix_trace_attr_getlogsize`](#posix_005ftrace_005fattr_005fgetlogsize), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 12.39 `posix_trace_attr_getmaxdatasize` [¶](#posix_005ftrace_005fattr_005fgetmaxdatasize-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getmaxdatasize.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`posix_trace_attr_getmaxusereventsize`](#posix_005ftrace_005fattr_005fgetmaxusereventsize), Previous: [`posix_trace_attr_getmaxdatasize`](#posix_005ftrace_005fattr_005fgetmaxdatasize), Up: [Past POSIX Function Substitutes](#Legacy-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
