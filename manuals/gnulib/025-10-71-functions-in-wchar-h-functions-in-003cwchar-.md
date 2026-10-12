### 10.71 Functions in `<wchar.h>` [¶](#Functions-in-_003cwchar_002eh_003e-1)

* [`btowc`](#btowc)
* [`fgetwc`](#fgetwc)
* [`fgetws`](#fgetws)
* [`fputwc`](#fputwc)
* [`fputws`](#fputws)
* [`fwide`](#fwide)
* [`fwprintf`](#fwprintf)
* [`fwscanf`](#fwscanf)
* [`getwc`](#getwc)
* [`getwchar`](#getwchar)
* [`mbrlen`](#mbrlen)
* [`mbrtowc`](#mbrtowc)
* [`mbsinit`](#mbsinit)
* [`mbsnrtowcs`](#mbsnrtowcs)
* [`mbsrtowcs`](#mbsrtowcs)
* [`open_wmemstream`](#open_005fwmemstream)
* [`putwc`](#putwc)
* [`putwchar`](#putwchar)
* [`swprintf`](#swprintf)
* [`swscanf`](#swscanf)
* [`ungetwc`](#ungetwc)
* [`vfwprintf`](#vfwprintf)
* [`vfwscanf`](#vfwscanf)
* [`vswprintf`](#vswprintf)
* [`vswscanf`](#vswscanf)
* [`vwprintf`](#vwprintf)
* [`vwscanf`](#vwscanf)
* [`wcpcpy`](#wcpcpy)
* [`wcpncpy`](#wcpncpy)
* [`wcrtomb`](#wcrtomb)
* [`wcscasecmp`](#wcscasecmp)
* [`wcscasecmp_l`](#wcscasecmp_005fl)
* [`wcscat`](#wcscat)
* [`wcschr`](#wcschr)
* [`wcscmp`](#wcscmp)
* [`wcscoll`](#wcscoll)
* [`wcscoll_l`](#wcscoll_005fl)
* [`wcscpy`](#wcscpy)
* [`wcscspn`](#wcscspn)
* [`wcsdup`](#wcsdup)
* [`wcsftime`](#wcsftime)
* [`wcslcat`](#wcslcat)
* [`wcslcpy`](#wcslcpy)
* [`wcslen`](#wcslen)
* [`wcsncasecmp`](#wcsncasecmp)
* [`wcsncasecmp_l`](#wcsncasecmp_005fl)
* [`wcsncat`](#wcsncat)
* [`wcsncmp`](#wcsncmp)
* [`wcsncpy`](#wcsncpy)
* [`wcsnlen`](#wcsnlen)
* [`wcsnrtombs`](#wcsnrtombs)
* [`wcspbrk`](#wcspbrk)
* [`wcsrchr`](#wcsrchr)
* [`wcsrtombs`](#wcsrtombs)
* [`wcsspn`](#wcsspn)
* [`wcsstr`](#wcsstr)
* [`wcstod`](#wcstod)
* [`wcstof`](#wcstof)
* [`wcstok`](#wcstok)
* [`wcstol`](#wcstol)
* [`wcstold`](#wcstold)
* [`wcstoll`](#wcstoll)
* [`wcstoul`](#wcstoul)
* [`wcstoull`](#wcstoull)
* [`wcswidth`](#wcswidth)
* [`wcsxfrm`](#wcsxfrm)
* [`wcsxfrm_l`](#wcsxfrm_005fl)
* [`wctob`](#wctob)
* [`wcwidth`](#wcwidth)
* [`wmemchr`](#wmemchr)
* [`wmemcmp`](#wmemcmp)
* [`wmemcpy`](#wmemcpy)
* [`wmemmove`](#wmemmove)
* [`wmemset`](#wmemset)
* [`wprintf`](#wprintf)
* [`wscanf`](#wscanf)

---

Next: [`fgetwc`](#fgetwc), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.1 `btowc` [¶](#btowc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/btowc.html>

Gnulib module: btowc

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, mingw.
* This function returns WEOF for a NUL argument on some platforms:
  Cygwin 1.7.2.
* In the C or POSIX locales, this function is not consistent with
  Gnulib’s `mbrtowc` and can return `WEOF`:
  glibc 2.35, MirOS BSD #10.
* In the C or POSIX locales, this function is not consistent with `mbrtowc`
  on some platforms:
  Cygwin 3.4.x, mingw with MSVCRT.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `btoc32`, provided by Gnulib module
  `btoc32`, operates on 32-bit wide characters and therefore does not have
  this limitation.

---

Next: [`fgetws`](#fgetws), Previous: [`btowc`](#btowc), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.2 `fgetwc` [¶](#fgetwc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgetwc.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`fputwc`](#fputwc), Previous: [`fgetwc`](#fgetwc), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.3 `fgetws` [¶](#fgetws-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgetws.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`fputws`](#fputws), Previous: [`fgetws`](#fgetws), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.4 `fputwc` [¶](#fputwc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fputwc.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`fwide`](#fwide), Previous: [`fputwc`](#fputwc), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.5 `fputws` [¶](#fputws-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fputws.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Next: [`fwprintf`](#fwprintf), Previous: [`fputws`](#fputws), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.6 `fwide` [¶](#fwide-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fwide.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, Cygwin 1.5.x, MSVC 14.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
* `fwide` is not guaranteed to be able to change a file stream’s mode
  to a different mode than the current one.

---

Next: [`fwscanf`](#fwscanf), Previous: [`fwide`](#fwide), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.7 `fwprintf` [¶](#fwprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fwprintf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11.00, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14,
  Haiku.
* This function doesn’t support the `'` flag correctly for integers
  (it uses the decimal point instead of the thousands separator!)
  on some platforms:
  mingw with `__USE_MINGW_ANSI_STDIO`.
* This function doesn’t support the `'` flag together with a precision
  on an integer argument correctly on some platforms:
  glibc 2.36, FreeBSD 14.0, NetBSD 10.0, Solaris 11, Cygwin 3.6.0, Haiku.
* This function doesn’t support the `'` flag correctly in locales where
  the thousands separator is a multibyte character on some platforms:
  NetBSD 10.0, Solaris 11 OpenIndiana.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.
* In the C or POSIX locales, the `%c` and `%s` conversions may fail
  on some platforms:
  glibc 2.35.
* When formatting an integer with grouping flag, this function inserts thousands
  separators even in the "C" locale on some platforms:
  NetBSD 5.1.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Next: [`getwc`](#getwc), Previous: [`fwprintf`](#fwprintf), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.8 `fwscanf` [¶](#fwscanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fwscanf.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-fwscanf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

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

Next: [`getwchar`](#getwchar), Previous: [`fwscanf`](#fwscanf), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.9 `getwc` [¶](#getwc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getwc.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`mbrlen`](#mbrlen), Previous: [`getwc`](#getwc), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.10 `getwchar` [¶](#getwchar-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getwchar.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`mbrtowc`](#mbrtowc), Previous: [`getwchar`](#getwchar), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.11 `mbrlen` [¶](#mbrlen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mbrlen.html>

Gnulib module: mbrlen

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, mingw.
* In the C or POSIX locales, this function can return `(size_t) -1`
  and set `errno` to `EILSEQ`:
  glibc 2.35.
* This function returns 0 instead of `(size_t) -2` when the input
  is empty:
  glibc 2.19.
* This function returns `(size_t) -1` instead of `(size_t) -2`
  when the input is empty:
  AIX 5.1.
* This function does not put the state into non-initial state when parsing an
  incomplete multibyte character on some platforms:
  AIX 5.1.
* This function returns the total number of bytes that make up the multibyte
  character, not the number of bytes that were needed to complete the multibyte
  character, on some platforms:
  HP-UX 11.11, Solaris 11 2010-11.
* This function returns `(size_t) -2` instead of `(size_t) -1`
  for some invalid byte sequences on some platforms:
  NetBSD 10.
* This function may not return 0 when parsing the NUL character on some platforms:
  Solaris 9.

Portability problems not fixed by Gnulib:

---

Next: [`mbsinit`](#mbsinit), Previous: [`mbrlen`](#mbrlen), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.12 `mbrtowc` [¶](#mbrtowc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mbrtowc.html>

Gnulib module: mbrtowc

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, mingw.
* In the C or POSIX locales, this function can return `(size_t) -1`
  and set `errno` to `EILSEQ`:
  glibc 2.35.
* This function returns 0 instead of `(size_t) -2` when the input
  is empty:
  glibc 2.19,
  mingw with UCRT, MSVC 14, Android 11.
* This function returns `(size_t) -1` instead of `(size_t) -2`
  when the input is empty:
  AIX 7.2.
* This function does not put the state into non-initial state when parsing an
  incomplete multibyte character on some platforms:
  AIX 7.2.
* This function stores a wide character when parsing an incomplete multibyte
  character on some platforms:
  mingw with UCRT, MSVC 14.
* This function returns the total number of bytes that make up the multibyte
  character, not the number of bytes that were needed to complete the multibyte
  character, on some platforms:
  HP-UX 11.11, Solaris 11 2010-11, mingw, MSVC 14.
* This function returns `(size_t) -2` instead of `(size_t) -1`
  for some invalid byte sequences on some platforms:
  NetBSD 10.
* This function may not return 0 when parsing the NUL character on some platforms:
  Solaris 9.

Portability problems not fixed by Gnulib:

* In UTF-8 locales, this function may return wide characters up to 0x7FFFFFFF
  (that is, beyond 0x0010FFFF) on some platforms:
  glibc 2.34.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the ISO C11 function `mbrtoc32`, provided by Gnulib module
  `mbrtoc32`, operates on 32-bit wide characters and therefore does not have
  this limitation.
* This function does not work with a copied `mbstate_t` on some platforms:
  Haiku.

---

Next: [`mbsnrtowcs`](#mbsnrtowcs), Previous: [`mbrtowc`](#mbrtowc), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.13 `mbsinit` [¶](#mbsinit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mbsinit.html>

Gnulib module: mbsinit

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, mingw with UCRT, MSVC 14.
* This function always returns 1, even in multibyte locales, on some platforms:
  mingw.

Portability problems not fixed by Gnulib:

* This function is not useful after calls to `mbrtoc16` or `mbrtoc8`.

---

Next: [`mbsrtowcs`](#mbsrtowcs), Previous: [`mbsinit`](#mbsinit), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.14 `mbsnrtowcs` [¶](#mbsnrtowcs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mbsnrtowcs.html>

Gnulib module: mbsnrtowcs

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 11.3, Cygwin 1.5.x, mingw, MSVC 14, Android 4.4.
* This function produces invalid wide characters on some platforms:
  Solaris 11.4.
* In the C or POSIX locales, this function can return `(size_t) -1`
  and set `errno` to `EILSEQ`:
  glibc 2.35.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `mbsnrtoc32s`, provided by Gnulib module
  `mbsnrtoc32s`, operates on 32-bit wide characters and therefore does not
  have this limitation.
* The specification is not clear about whether this function should update the
  conversion state when the first argument (the destination pointer) is NULL.
  The glibc, Mac OS X, FreeBSD implementations do update the state in this case.
  For portability, when passing a NULL destination argument, it is best to pass
  a pointer to a temporary copy of the conversion state.

---

Next: [`open_wmemstream`](#open_005fwmemstream), Previous: [`mbsnrtowcs`](#mbsnrtowcs), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.15 `mbsrtowcs` [¶](#mbsrtowcs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mbsrtowcs.html>

Gnulib module: mbsrtowcs

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, mingw.
* This function does not work on some platforms:
  HP-UX 11, Solaris 11 2010-11.
* In the C or POSIX locales, this function can return `(size_t) -1`
  and set `errno` to `EILSEQ`:
  glibc 2.35.
* This function does not work when the first argument is NULL on some platforms:
  mingw.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `mbsrtoc32s`, provided by Gnulib module
  `mbsrtoc32s`, operates on 32-bit wide characters and therefore does not
  have this limitation.
* The specification is not clear about whether this function should update the
  conversion state when the first argument (the destination pointer) is NULL.
  The glibc implementation does not update the state in this case; the macOS
  and FreeBSD implementations do.
  For portability, when passing a NULL destination argument, it is best to pass
  a pointer to a temporary copy of the conversion state.

---

Next: [`putwc`](#putwc), Previous: [`mbsrtowcs`](#mbsrtowcs), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.16 `open_wmemstream` [¶](#open_005fwmemstream-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/open_wmemstream.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Mac OS X 10.5, FreeBSD 6.0, NetBSD 7.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.5.x, mingw, MSVC 14, Android 5.1.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`putwchar`](#putwchar), Previous: [`open_wmemstream`](#open_005fwmemstream), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.17 `putwc` [¶](#putwc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/putwc.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Next: [`swprintf`](#swprintf), Previous: [`putwc`](#putwc), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.18 `putwchar` [¶](#putwchar-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/putwchar.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Next: [`swscanf`](#swscanf), Previous: [`putwchar`](#putwchar), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.19 `swprintf` [¶](#swprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/swprintf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11.00, Cygwin 1.5.x, mingw with UCRT.
* This function has a different signature on some platforms:
  MSVC 9.
* This function is only defined as a macro on some platforms:
  MSVC 14.
* This function does not support the ‘`n`’ directive on some platforms:
  glibc when used with `_FORTIFY_SOURCE >= 2` (set by default on Ubuntu),
  macOS 14, OpenBSD, MSVC 14, Android.
* This function sometimes returns a wrong value through the ‘`n`’ directive
  on some platforms:
  musl libc 1.2.3.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
* On Windows, this function does not take a buffer size as second argument.
* This function produces wrong values for the ‘`La`’ directive
  on some platforms:
  glibc 2.15,
  Haiku,
  NetBSD 10.0.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* This function ignores the minimum field width in the ‘`lc`’ directive
  on some platforms:
  musl libc 1.2.3.
* This function is broken when it produces output that contains null wide
  characters on some platforms:
  musl libc 1.2.3,
  macOS 14, FreeBSD 13.1, NetBSD 10.0, OpenBSD 7.2, AIX 7.2, mingw.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14,
  Haiku.
* This function doesn’t support the `'` flag correctly for integers
  (it uses the decimal point instead of the thousands separator!)
  on some platforms:
  mingw with `__USE_MINGW_ANSI_STDIO`.
* This function doesn’t support the `'` flag together with a precision
  on an integer argument correctly on some platforms:
  glibc 2.36, FreeBSD 14.0, NetBSD 10.0, Solaris 11, Cygwin 3.6.0, Haiku.
* This function doesn’t support the `'` flag correctly in locales where
  the thousands separator is a multibyte character on some platforms:
  NetBSD 10.0, Solaris 11 OpenIndiana.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.
* In the C or POSIX locales, the `%c` and `%s` conversions may fail
  on some platforms:
  glibc 2.35, musl libc 1.2.4, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.2, Cygwin 2.9.0.
* The `%lc` directive may fail on some platforms:
  musl libc 1.2.4, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.2.
* When formatting an integer with grouping flag, this function inserts thousands
  separators even in the "C" locale on some platforms:
  NetBSD 5.1.

---

Next: [`ungetwc`](#ungetwc), Previous: [`swprintf`](#swprintf), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.20 `swscanf` [¶](#swscanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/swscanf.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-swscanf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

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

Next: [`vfwprintf`](#vfwprintf), Previous: [`swscanf`](#swscanf), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.21 `ungetwc` [¶](#ungetwc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ungetwc.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`vfwscanf`](#vfwscanf), Previous: [`ungetwc`](#ungetwc), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.22 `vfwprintf` [¶](#vfwprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vfwprintf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11.00, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14,
  Haiku.
* This function doesn’t support the `'` flag correctly for integers
  (it uses the decimal point instead of the thousands separator!)
  on some platforms:
  mingw with `__USE_MINGW_ANSI_STDIO`.
* This function doesn’t support the `'` flag together with a precision
  on an integer argument correctly on some platforms:
  glibc 2.36, FreeBSD 14.0, NetBSD 10.0, Solaris 11, Cygwin 3.6.0, Haiku.
* This function doesn’t support the `'` flag correctly in locales where
  the thousands separator is a multibyte character on some platforms:
  NetBSD 10.0, Solaris 11 OpenIndiana.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.
* In the C or POSIX locales, the `%c` and `%s` conversions may fail
  on some platforms:
  glibc 2.35.
* When formatting an integer with grouping flag, this function inserts thousands
  separators even in the "C" locale on some platforms:
  NetBSD 5.1.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Next: [`vswprintf`](#vswprintf), Previous: [`vfwprintf`](#vfwprintf), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.23 `vfwscanf` [¶](#vfwscanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vfwscanf.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-vfwscanf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11.23, Cygwin 1.5.x, Android 4.4.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.38, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.2,
  AIX 7.2, HP-UX 11, Solaris 11.4, Cygwin 2.9.0, mingw, MSVC 14.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`vswscanf`](#vswscanf), Previous: [`vfwscanf`](#vfwscanf), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.24 `vswprintf` [¶](#vswprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vswprintf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11.00, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
* On Windows, this function does not take a buffer size as second argument.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14,
  Haiku.
* This function doesn’t support the `'` flag correctly for integers
  (it uses the decimal point instead of the thousands separator!)
  on some platforms:
  mingw with `__USE_MINGW_ANSI_STDIO`.
* This function doesn’t support the `'` flag together with a precision
  on an integer argument correctly on some platforms:
  glibc 2.36, FreeBSD 14.0, NetBSD 10.0, Solaris 11, Cygwin 3.6.0, Haiku.
* This function doesn’t support the `'` flag correctly in locales where
  the thousands separator is a multibyte character on some platforms:
  NetBSD 10.0, Solaris 11 OpenIndiana.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.
* In the C or POSIX locales, the `%c` and `%s` conversions may fail
  on some platforms:
  glibc 2.35.
* When formatting an integer with grouping flag, this function inserts thousands
  separators even in the "C" locale on some platforms:
  NetBSD 5.1.

---

Next: [`vwprintf`](#vwprintf), Previous: [`vswprintf`](#vswprintf), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.25 `vswscanf` [¶](#vswscanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vswscanf.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-vswscanf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11.23, Cygwin 1.5.x, MSVC 14, Android 4.4.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.38, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.2,
  AIX 7.2, HP-UX 11, Solaris 11.4, Cygwin 2.9.0, mingw, MSVC 14.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`vwscanf`](#vwscanf), Previous: [`vswscanf`](#vswscanf), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.26 `vwprintf` [¶](#vwprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vwprintf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11.00, Cygwin 1.7.
* This function exists, but does not support wide arguments on some
  platforms: Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14,
  Haiku.
* This function doesn’t support the `'` flag correctly for integers
  (it uses the decimal point instead of the thousands separator!)
  on some platforms:
  mingw with `__USE_MINGW_ANSI_STDIO`.
* This function doesn’t support the `'` flag together with a precision
  on an integer argument correctly on some platforms:
  glibc 2.36, FreeBSD 14.0, NetBSD 10.0, Solaris 11, Cygwin 3.6.0, Haiku.
* This function doesn’t support the `'` flag correctly in locales where
  the thousands separator is a multibyte character on some platforms:
  NetBSD 10.0, Solaris 11 OpenIndiana.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.
* In the C or POSIX locales, the `%c` and `%s` conversions may fail
  on some platforms:
  glibc 2.35.
* When formatting an integer with grouping flag, this function inserts thousands
  separators even in the "C" locale on some platforms:
  NetBSD 5.1.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Next: [`wcpcpy`](#wcpcpy), Previous: [`vwprintf`](#vwprintf), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.27 `vwscanf` [¶](#vwscanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vwscanf.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-vwscanf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11.23, Cygwin 1.5.x, Android 4.4.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.38, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.2,
  AIX 7.2, HP-UX 11, Solaris 11.4, Cygwin 2.9.0, mingw, MSVC 14.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcpncpy`](#wcpncpy), Previous: [`vwscanf`](#vwscanf), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.28 `wcpcpy` [¶](#wcpcpy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcpcpy.html>

Gnulib module: wcpcpy

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14.
* This function is not declared (without `-D_GNU_SOURCE`) on some platforms:
  glibc 2.13.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcrtomb`](#wcrtomb), Previous: [`wcpcpy`](#wcpcpy), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.29 `wcpncpy` [¶](#wcpncpy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcpncpy.html>

Gnulib module: wcpncpy

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14.
* This function is not declared (without `-D_GNU_SOURCE`) on some platforms:
  glibc 2.13.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcscasecmp`](#wcscasecmp), Previous: [`wcpncpy`](#wcpncpy), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.30 `wcrtomb` [¶](#wcrtomb-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcrtomb.html>

Gnulib module: wcrtomb

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, mingw.
* This function produces wrong characters in the C locale on some platforms:
  Android 4.3.
* This function returns 0 when the first argument is NULL in some locales on some platforms:
  Solaris 11.3.
* This function does not ignore the second argument when the first argument is NULL on some platforms:
  mingw with UCRT, MSVC 14.
* This function fails to handle wide characters produced by `mbrtowc` on some platforms:
  Haiku.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the ISO C11 function `c32rtomb`, provided by Gnulib module
  `c32rtomb`, operates on 32-bit wide characters and therefore does not have
  this limitation.

---

Next: [`wcscasecmp_l`](#wcscasecmp_005fl), Previous: [`wcrtomb`](#wcrtomb), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.31 `wcscasecmp` [¶](#wcscasecmp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcscasecmp.html>

Gnulib module: wcscasecmp

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcscat`](#wcscat), Previous: [`wcscasecmp`](#wcscasecmp), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.32 `wcscasecmp_l` [¶](#wcscasecmp_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcscasecmp_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  Mac OS X 10.5, FreeBSD 14.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 5.1.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcschr`](#wcschr), Previous: [`wcscasecmp_l`](#wcscasecmp_005fl), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.33 `wcscat` [¶](#wcscat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcscat.html>

Gnulib module: wcscat

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcscmp`](#wcscmp), Previous: [`wcscat`](#wcscat), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.34 `wcschr` [¶](#wcschr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcschr.html>

Gnulib module: wcschr

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcscoll`](#wcscoll), Previous: [`wcschr`](#wcschr), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.35 `wcscmp` [¶](#wcscmp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcscmp.html>

Gnulib module: wcscmp

Portability problems fixed by Gnulib:

* This function compares the wide characters as if they were unsigned, although
  `wchar_t` is signed, on some platforms:
  glibc 2.14.1 on x86 or x86\_64, musl libc 1.2.3, macOS 14,
  FreeBSD 13.2,
  NetBSD 10.0, OpenBSD 7.2, Solaris 11.4.
* This function may return a wrong result if the two arguments are of different
  length, on some platforms:
  glibc 2.37 on arm and arm64 CPUs,
  AIX 7.2 in 64-bit mode.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcscoll_l`](#wcscoll_005fl), Previous: [`wcscmp`](#wcscmp), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.36 `wcscoll` [¶](#wcscoll-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcscoll.html>

Gnulib module: wcscoll

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcscpy`](#wcscpy), Previous: [`wcscoll`](#wcscoll), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.37 `wcscoll_l` [¶](#wcscoll_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcscoll_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcscspn`](#wcscspn), Previous: [`wcscoll_l`](#wcscoll_005fl), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.38 `wcscpy` [¶](#wcscpy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcscpy.html>

Gnulib module: wcscpy

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

Note: `wcscpy (dst, src)` is only safe to use when you can guarantee that
there are at least `wcslen (src) + 1` wide characters allocated at
`dst`.

---

Next: [`wcsdup`](#wcsdup), Previous: [`wcscpy`](#wcscpy), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.39 `wcscspn` [¶](#wcscspn-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcscspn.html>

Gnulib module: wcscspn

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcsftime`](#wcsftime), Previous: [`wcscspn`](#wcscspn), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.40 `wcsdup` [¶](#wcsdup-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsdup.html>

Gnulib module: wcsdup

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, HP-UX 11, Solaris 10, Cygwin 1.5.x.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcslcat`](#wcslcat), Previous: [`wcsdup`](#wcsdup), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.41 `wcsftime` [¶](#wcsftime-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsftime.html>

Gnulib module: wcsftime

Portability problems fixed by Gnulib:

* On native Windows platforms (mingw, MSVC), this function works incorrectly
  when the environment variable `TZ` has been set by Cygwin.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
* Native Windows platforms (mingw, MSVC) support only a subset of time
  zones specified by POSIX. See [`tzset`](#tzset).

---

Next: [`wcslcpy`](#wcslcpy), Previous: [`wcsftime`](#wcsftime), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.42 `wcslcat` [¶](#wcslcat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcslcat.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Truncating-Strings.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.37, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, mingw, MSVC 14.

---

Next: [`wcslen`](#wcslen), Previous: [`wcslcat`](#wcslcat), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.43 `wcslcpy` [¶](#wcslcpy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcslcpy.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Truncating-Strings.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.37, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, mingw, MSVC 14.

---

Next: [`wcsncasecmp`](#wcsncasecmp), Previous: [`wcslcpy`](#wcslcpy), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.44 `wcslen` [¶](#wcslen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcslen.html>

Gnulib module: wcslen

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcsncasecmp_l`](#wcsncasecmp_005fl), Previous: [`wcslen`](#wcslen), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.45 `wcsncasecmp` [¶](#wcsncasecmp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsncasecmp.html>

Gnulib module: wcsncasecmp

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcsncat`](#wcsncat), Previous: [`wcsncasecmp`](#wcsncasecmp), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.46 `wcsncasecmp_l` [¶](#wcsncasecmp_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsncasecmp_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  Mac OS X 10.5, FreeBSD 14.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 5.1.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcsncmp`](#wcsncmp), Previous: [`wcsncasecmp_l`](#wcsncasecmp_005fl), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.47 `wcsncat` [¶](#wcsncat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsncat.html>

Gnulib module: wcsncat

Portability problems fixed by Gnulib:

* This function does not support zero-length operations on NULL pointers
  on some platforms:
  macOS 15, NetBSD 10.0, OpenBSD 7.6, Solaris 11.4, Cygwin 3.5.4.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcsncpy`](#wcsncpy), Previous: [`wcsncat`](#wcsncat), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.48 `wcsncmp` [¶](#wcsncmp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsncmp.html>

Gnulib module: wcsncmp

Portability problems fixed by Gnulib:

* This function compares the wide characters as if they were unsigned, although
  `wchar_t` is signed, on some platforms:
  glibc 2.14.1 on x86 or x86\_64, musl libc 1.2.3, macOS 14,
  FreeBSD 13.2,
  NetBSD 10.0, OpenBSD 7.2, Solaris 11.4.
* This function may return a wrong result if the two arguments are of different
  length, on some platforms:
  AIX 7.2 in 64-bit mode.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcsnlen`](#wcsnlen), Previous: [`wcsncmp`](#wcsncmp), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.49 `wcsncpy` [¶](#wcsncpy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsncpy.html>

Gnulib module: wcsncpy

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore
  cannot accommodate all Unicode characters.

Note: This function has no real use: It cannot be used for filling a fixed-size
record with a wide string, before writing it to a file, because the wide string
encoding is platform dependent and, on some platforms, also locale dependent.
And this function is **not** appropriate for copying a wide string into a
bounded memory area, because you have no guarantee that the result will be
null-terminated. Even if you add the null character at the end yourself, this
function is inefficient (as it spends time clearing unused memory) and will
allow silent truncation to occur, which is not a good behavior for GNU programs.

---

Next: [`wcsnrtombs`](#wcsnrtombs), Previous: [`wcsncpy`](#wcsncpy), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.50 `wcsnlen` [¶](#wcsnlen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsnlen.html>

Gnulib module: wcsnlen

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 7.1, OpenBSD 7.5, Minix 3.1.8, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcspbrk`](#wcspbrk), Previous: [`wcsnlen`](#wcsnlen), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.51 `wcsnrtombs` [¶](#wcsnrtombs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsnrtombs.html>

Gnulib module: wcsnrtombs

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 11.3, Cygwin 1.5.x, mingw, MSVC 14, Android 4.4.
* This function cannot consume valid sequences of wide characters
  on some platforms:
  Solaris 11.4.
* In C++ mode, the system’s `<wchar.h>` defines `std::wcsnrtombs` but
  not `::wcsnrtombs` on some platforms:
  Solaris 11 OpenIndiana.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32snrtombs`, provided by Gnulib module
  `c32snrtombs`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`wcsrchr`](#wcsrchr), Previous: [`wcsnrtombs`](#wcsnrtombs), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.52 `wcspbrk` [¶](#wcspbrk-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcspbrk.html>

Gnulib module: wcspbrk

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcsrtombs`](#wcsrtombs), Previous: [`wcspbrk`](#wcspbrk), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.53 `wcsrchr` [¶](#wcsrchr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsrchr.html>

Gnulib module: wcsrchr

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcsspn`](#wcsspn), Previous: [`wcsrchr`](#wcsrchr), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.54 `wcsrtombs` [¶](#wcsrtombs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsrtombs.html>

Gnulib module: wcsrtombs

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, mingw.
* This function does not ignore the length argument if the destination argument is NULL on some platforms:
  mingw.
* This function updates the source pointer also if the destination argument is NULL on some platforms:
  HP-UX 11.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32srtombs`, provided by Gnulib module
  `c32srtombs`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`wcsstr`](#wcsstr), Previous: [`wcsrtombs`](#wcsrtombs), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.55 `wcsspn` [¶](#wcsspn-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsspn.html>

Gnulib module: wcsspn

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstod`](#wcstod), Previous: [`wcsspn`](#wcsspn), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.56 `wcsstr` [¶](#wcsstr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsstr.html>

Gnulib module: wcsstr or wcsstr-simple

Portability problems fixed by either Gnulib module `wcsstr-simple` or `wcsstr`:

* This function is missing on some platforms:
  HP-UX 11.00.

Portability problems fixed by Gnulib module `wcsstr`:

* This function has quadratic instead of linear worst-case complexity on some
  platforms:
  glibc 2.39, macOS 14, FreeBSD 13.1, NetBSD 10.0, OpenBSD 7.2, AIX 7.2, HP-UX 11, Solaris 11.4, Cygwin 2.9.0, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstof`](#wcstof), Previous: [`wcsstr`](#wcsstr), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.57 `wcstod` [¶](#wcstod-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcstod.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstok`](#wcstok), Previous: [`wcstod`](#wcstod), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.58 `wcstof` [¶](#wcstof-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcstof.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 9, Cygwin 1.5.x, MSVC 9, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstol`](#wcstol), Previous: [`wcstof`](#wcstof), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.59 `wcstok` [¶](#wcstok-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcstok.html>

Gnulib module: wcstok

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Cygwin 1.5.x.
* This function takes only two arguments on some platforms:
  mingw, older MSVC.
* This function may use hidden state, ignoring the third argument, and thus
  exhibit a bug when two or more `wcstok` iteration loops are being performed
  in the same thread, on some platforms:
  HP-UX 11.31.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstold`](#wcstold), Previous: [`wcstok`](#wcstok), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.60 `wcstol` [¶](#wcstol-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcstol.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstoll`](#wcstoll), Previous: [`wcstol`](#wcstol), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.61 `wcstold` [¶](#wcstold-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcstold.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 9, Cygwin 1.7.x, MSVC 9, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstoul`](#wcstoul), Previous: [`wcstold`](#wcstold), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.62 `wcstoll` [¶](#wcstoll-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcstoll.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.11, Solaris 9, Cygwin 1.5.x, MSVC 9, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstoull`](#wcstoull), Previous: [`wcstoll`](#wcstoll), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.63 `wcstoul` [¶](#wcstoul-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcstoul.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcswidth`](#wcswidth), Previous: [`wcstoul`](#wcstoul), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.64 `wcstoull` [¶](#wcstoull-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcstoull.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.11, Solaris 9, Cygwin 1.5.x, MSVC 9, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcsxfrm`](#wcsxfrm), Previous: [`wcstoull`](#wcstoull), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.65 `wcswidth` [¶](#wcswidth-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcswidth.html>

Gnulib module: wcswidth

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
  However, the Gnulib function `c32swidth`, provided by Gnulib module
  `c32swidth`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`wcsxfrm_l`](#wcsxfrm_005fl), Previous: [`wcswidth`](#wcswidth), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.66 `wcsxfrm` [¶](#wcsxfrm-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsxfrm.html>

Gnulib module: wcsxfrm

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wctob`](#wctob), Previous: [`wcsxfrm`](#wcsxfrm), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.67 `wcsxfrm_l` [¶](#wcsxfrm_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcsxfrm_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcwidth`](#wcwidth), Previous: [`wcsxfrm_l`](#wcsxfrm_005fl), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.68 `wctob` [¶](#wctob-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wctob.html>

Gnulib module: wctob

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, mingw.
* This function clobbers caller-owned registers on some platforms:
  Cygwin 1.7.5.
* This function does not work on some platforms:
  Solaris 9.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32tob`, provided by Gnulib module
  `c32tob`, operates on 32-bit wide characters and therefore does not have
  this limitation.

---

Next: [`wmemchr`](#wmemchr), Previous: [`wctob`](#wctob), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.69 `wcwidth` [¶](#wcwidth-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcwidth.html>

Gnulib module: wcwidth

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* This function is not declared (without `-D_GNU_SOURCE`) on some platforms:
  glibc 2.8.
* This function handles combining characters in UTF-8 locales incorrectly on some
  platforms:
  macOS 14, NetBSD 9.3, OpenBSD 5.8, MidnightBSD 1.1.
* This function returns 2 for characters with ambiguous east asian width, even in
  Western locales, on some platforms:
  Solaris 11.4.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
  However, the Gnulib function `c32width`, provided by Gnulib module
  `c32width`, operates on 32-bit wide characters and therefore does not
  have this limitation.
* This function treats zero-width spaces like control characters on some
  platforms:
  AIX 7.2.

---

Next: [`wmemcmp`](#wmemcmp), Previous: [`wcwidth`](#wcwidth), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.70 `wmemchr` [¶](#wmemchr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wmemchr.html>

Gnulib module: wmemchr

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.00, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wmemcpy`](#wmemcpy), Previous: [`wmemchr`](#wmemchr), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.71 `wmemcmp` [¶](#wmemcmp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wmemcmp.html>

Gnulib module: wmemcmp

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.00, MSVC 14.
* This function compares the wide characters as if they were unsigned, although
  `wchar_t` is signed, on some platforms:
  glibc 2.14.1 on x86 or x86\_64, musl libc 1.2.3, NetBSD 10.0, OpenBSD 7.2, Solaris 11.4.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wmemmove`](#wmemmove), Previous: [`wmemcmp`](#wmemcmp), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.72 `wmemcpy` [¶](#wmemcpy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wmemcpy.html>

Gnulib module: wmemcpy

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.00, MSVC 9.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wmemset`](#wmemset), Previous: [`wmemcpy`](#wmemcpy), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.73 `wmemmove` [¶](#wmemmove-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wmemmove.html>

Gnulib module: wmemmove

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.00, MSVC 9.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wprintf`](#wprintf), Previous: [`wmemmove`](#wmemmove), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.74 `wmemset` [¶](#wmemset-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wmemset.html>

Gnulib module: wmemset

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.00, MSVC 9.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wscanf`](#wscanf), Previous: [`wmemset`](#wmemset), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.75 `wprintf` [¶](#wprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wprintf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11.00.
* This function exists, but does not support wide arguments on some
  platforms: Cygwin 1.5.x.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14,
  Haiku.
* This function doesn’t support the `'` flag correctly for integers
  (it uses the decimal point instead of the thousands separator!)
  on some platforms:
  mingw with `__USE_MINGW_ANSI_STDIO`.
* This function doesn’t support the `'` flag together with a precision
  on an integer argument correctly on some platforms:
  glibc 2.36, FreeBSD 14.0, NetBSD 10.0, Solaris 11, Cygwin 3.6.0, Haiku.
* This function doesn’t support the `'` flag correctly in locales where
  the thousands separator is a multibyte character on some platforms:
  NetBSD 10.0, Solaris 11 OpenIndiana.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.
* In the C or POSIX locales, the `%c` and `%s` conversions may fail
  on some platforms:
  glibc 2.35.
* When formatting an integer with grouping flag, this function inserts thousands
  separators even in the "C" locale on some platforms:
  NetBSD 5.1.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Previous: [`wprintf`](#wprintf), Up: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.71.76 `wscanf` [¶](#wscanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wscanf.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-wscanf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:
