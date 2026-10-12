* This function does not support size specifiers as in C99 (`hh`, `ll`,
  `j`, `t`, `z`) on some platforms:
  AIX 5.1, HP-UX 11.23, Solaris 9, Cygwin 1.5.24, old mingw, MSVC 9.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* printf of ‘`long double`’ numbers is unsupported on some platforms:
  mingw, MSVC 14.
* printf `"%f"`, `"%e"`, `"%g"` of Infinity and NaN yields an
  incorrect result on some platforms:
  AIX 7.3, Solaris 11.4, mingw, MSVC 14.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14.
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
* This function does not support the ‘`a`’ and ‘`A`’ directives on some
  platforms:
  FreeBSD 5.2.1, OpenBSD 4.0, AIX 5.2, HP-UX 11,
  Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`F`’ directive on some platforms:
  NetBSD 3.0, AIX 5.1, HP-UX 11.23, Solaris 9,
  Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`ls`’ directive on some platforms:
  OpenBSD 4.0, Cygwin 1.5.x, Haiku.
* This function does not support precisions in the ‘`ls`’ directive correctly
  on some platforms:
  Solaris 11.4.
* This function doesn’t support the `'` flag on some platforms:
  NetBSD 3.0, Cygwin 1.5.24, mingw, MSVC 14.
* This function does not round the argument of the ‘`a`’ directive correctly
  on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4.
* printf `"%010f"` of NaN and Infinity yields an incorrect result (padded
  with zeroes, or wrong capitalization) on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, AIX 7.3, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC/clang.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* This function does not support precisions larger than 512 or 1024 in integer,
  floating-point and pointer output on some platforms:
  AIX 7.1, Solaris 10/x86, mingw, MSVC/clang.
* This function mishandles large floating point precisions
  (for example, formatting 1.0 with ‘`"%.511f"`’)
  on some platforms:
  Solaris 10.
* This function produces wrong output for the ‘`lc`’ directive with a NUL
  wide character argument on some platforms:
  musl libc 1.2.4.
* This function can crash in out-of-memory conditions on some platforms:
  FreeBSD 14.0, NetBSD 5.0.
* This function does not truncate the result as specified in C99 on some platforms:
  mingw, MSVC 14.
* This function does not fully support the ‘`n`’ directive on some platforms:
  HP-UX 11, mingw.

Portability problems fixed by Gnulib module `snprintf-gnu`:

* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.

Portability problems fixed by either Gnulib module `snprintf-posix` or `snprintf-gnu`
and additionally the Gnulib module `printf-with-n-directive`:

* This function does not support the ‘`n`’ directive on some platforms:
  glibc when used with `_FORTIFY_SOURCE >= 2` (set by default on Ubuntu),
  macOS 14, OpenBSD, MSVC 14, Android.

Portability problems not fixed by Gnulib:

* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.
* Formatting noncanonical ‘`long double`’ numbers produces
  nonmeaningful results on some platforms:
  glibc and others, on x86, x86\_64, IA-64 CPUs.
* When formatting an integer with grouping flag, this function inserts thousands
  separators even in the "C" locale on some platforms:
  NetBSD 5.1.

---

Next: [`sscanf`](#sscanf), Previous: [`snprintf`](#snprintf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.55 `sprintf` [¶](#sprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sprintf.html>

Gnulib module: sprintf-posix or sprintf-gnu

Portability problems fixed by either Gnulib module `sprintf-posix` or `sprintf-gnu`:

* This function does not support size specifiers as in C99 (`hh`, `ll`,
  `j`, `t`, `z`) on some platforms:
  AIX 5.1, HP-UX 11.23, Solaris 9, Cygwin 1.5.24, old mingw, MSVC 9.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* printf of ‘`long double`’ numbers is unsupported on some platforms:
  mingw, MSVC 14.
* printf `"%f"`, `"%e"`, `"%g"` of Infinity and NaN yields an
  incorrect result on some platforms:
  AIX 7.3, Solaris 11.4, mingw, MSVC 14.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14.
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
* This function does not support the ‘`a`’ and ‘`A`’ directives on some
  platforms:
  FreeBSD 5.2.1, OpenBSD 4.0, AIX 5.2, HP-UX 11,
  Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`F`’ directive on some platforms:
  NetBSD 3.0, AIX 5.1, HP-UX 11.23, Solaris 9,
  Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`ls`’ directive on some platforms:
  OpenBSD 4.0, Cygwin 1.5.x, Haiku.
* This function does not support precisions in the ‘`ls`’ directive correctly
  on some platforms:
  Solaris 11.4.
* This function does not support format directives that access arguments in an
  arbitrary order, such as `"%2$s"`, on some platforms:
  NetBSD 3.0, mingw, MSVC 14.
* This function doesn’t support the `'` flag on some platforms:
  NetBSD 3.0, Cygwin 1.5.24, mingw, MSVC 14.
* This function does not round the argument of the ‘`a`’ directive correctly
  on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4.
* printf `"%010f"` of NaN and Infinity yields an incorrect result (padded
  with zeroes, or wrong capitalization) on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, AIX 7.3, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC/clang.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* This function does not support precisions larger than 512 or 1024 in integer,
  floating-point and pointer output on some platforms:
  AIX 7.1, Solaris 10/x86, mingw, MSVC/clang.
* This function mishandles large floating point precisions
  (for example, formatting 1.0 with ‘`"%.511f"`’)
  on some platforms:
  Solaris 10.
* This function produces wrong output for the ‘`lc`’ directive with a NUL
  wide character argument on some platforms:
  musl libc 1.2.4.
* This function can crash in out-of-memory conditions on some platforms:
  FreeBSD 14.0, NetBSD 5.0.
* The compiler warns that this function is deprecated:
  macOS 14.

Portability problems fixed by Gnulib module `sprintf-gnu`:

* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.

Portability problems fixed by either Gnulib module `sprintf-posix` or `sprintf-gnu`
and additionally the Gnulib module `printf-with-n-directive`:

* This function does not support the ‘`n`’ directive on some platforms:
  glibc when used with `_FORTIFY_SOURCE >= 2` (set by default on Ubuntu),
  macOS 14, OpenBSD, MSVC 14, Android.

Portability problems not fixed by Gnulib:

* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.
* Formatting noncanonical ‘`long double`’ numbers produces
  nonmeaningful results on some platforms:
  glibc and others, on x86, x86\_64, IA-64 CPUs.
* When formatting an integer with grouping flag, this function inserts thousands
  separators even in the "C" locale on some platforms:
  NetBSD 5.1.

---

Next: [`stderr`](#stderr), Previous: [`sprintf`](#sprintf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.56 `sscanf` [¶](#sscanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sscanf.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-sscanf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.38, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.2,
  AIX 7.2, HP-UX 11, Solaris 11.4, Cygwin 2.9.0, mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* On Windows, this function doesn’t support the `hh`, `ll`, `j`,
  `t`, `z` size specifiers.

---

Next: [`stdin`](#stdin), Previous: [`sscanf`](#sscanf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.57 `stderr` [¶](#stderr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/stderr.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* stderr is created in 32-bit mode instead of 64-bit mode: Cygwin 1.5.x.
  One workaround is to use `freopen(NULL, "r+", stderr)` on Cygwin 1.5.21
  or newer.
  Another is to use the gnulib `ftello` module and do
  `ftello(stderr)`.
* POSIX states that a setuid application can guarantee that fd 2 is
  open, but some systems guarantee this even for non-setuid programs.
  If an application is executed with fd 2 closed, use of `stderr`
  can affect an unrelated file that happened to be assigned to fd 2.
  The gnulib `xstdopen` or the \*-safer modules may be used to
  guarantee that fd 2 stays
  reserved for `stderr`.

---

Next: [`stdout`](#stdout), Previous: [`stderr`](#stderr), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.58 `stdin` [¶](#stdin-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/stdin.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* stdin is created in 32-bit mode instead of 64-bit mode: Cygwin 1.5.x.
  One workaround is to use freopen(NULL, “r”, stdin) on Cygwin 1.5.21
  or newer. Another is to use the gnulib ftello module and do
  ftello(stdin).
* POSIX states that a setuid application can guarantee that fd 0 is
  open, but some systems guarantee this even for non-setuid programs.
  If an application is executed with fd 0 closed, use of `stdin`
  can affect an unrelated file that happened to be assigned to fd 0.
  The gnulib `xstdopen` or the \*-safer modules may be used to
  guarantee that fd 0 stays
  reserved for `stdin`.

---

Next: [`tmpfile`](#tmpfile), Previous: [`stdin`](#stdin), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.59 `stdout` [¶](#stdout-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/stdout.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* stdout is created in 32-bit mode instead of 64-bit mode: Cygwin 1.5.x.
  One workaround is to use `freopen(NULL, "w", stdout)` on Cygwin 1.5.21
  or newer.
  Another is to use the gnulib `ftello` module and do
  `ftello(stdout)`.
* POSIX states that a setuid application can guarantee that fd 1 is
  open, but some systems guarantee this even for non-setuid programs.
  If an application is executed with fd 1 closed, use of `stdout`
  can affect an unrelated file that happened to be assigned to fd 1.
  The gnulib `xstdopen` or the \*-safer modules may be used to
  guarantee that fd 1 stays
  reserved for `stdout`.

---

Next: [`tmpnam`](#tmpnam), Previous: [`stdout`](#stdout), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.60 `tmpfile` [¶](#tmpfile-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tmpfile.html>

Gnulib module: tmpfile

Portability problems fixed by Gnulib:

* This function always fails on some platforms:
  Android 4.3.
* This function often fails for trivial reasons on some platforms:
  mingw, MSVC 14.
* On platforms where `off_t` is a 32-bit type, `tmpfile` may not work
  correctly to create files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

---

Next: [`ungetc`](#ungetc), Previous: [`tmpfile`](#tmpfile), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.61 `tmpnam` [¶](#tmpnam-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tmpnam.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is not appropriate for creating temporary files. (It has
  security risks.) Better use `mkstemp` instead.

---

Next: [`vasprintf`](#vasprintf), Previous: [`tmpnam`](#tmpnam), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.62 `ungetc` [¶](#ungetc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ungetc.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.

---

Next: [`vdprintf`](#vdprintf), Previous: [`ungetc`](#ungetc), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.63 `vasprintf` [¶](#vasprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vasprintf.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Variable-Arguments-Output.html>,
* [man vasprintf](https://www.kernel.org/doc/man-pages/online/pages/man3/vasprintf.3.html).

Gnulib module: vasprintf or vasprintf-posix or vasprintf-gnu

Portability problems fixed by either Gnulib module `vasprintf` or `vasprintf-posix` or `vasprintf-gnu`:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 10, mingw, MSVC 14.

Portability problems fixed by either Gnulib module `vasprintf-posix` or `vasprintf-gnu`:

* This function does not support size specifiers as in C99 (`hh`, `ll`,
  `j`, `t`, `z`) on some platforms:
  Cygwin 1.5.24.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* printf `"%f"`, `"%e"`, `"%g"` of Infinity and NaN yields an
  incorrect result on some platforms:
  AIX 7.3, Solaris 11.4.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14.
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
* This function does not support the ‘`a`’ and ‘`A`’ directives on some
  platforms:
  FreeBSD 5.2.1, OpenBSD 4.0, Solaris 11.4, Cygwin 1.5.x.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`F`’ directive on some platforms:
  NetBSD 3.0, Cygwin 1.5.x.
* This function does not support the ‘`ls`’ directive on some platforms:
  OpenBSD 4.0, Cygwin 1.5.x, Haiku.
* This function does not support precisions in the ‘`ls`’ directive correctly
  on some platforms:
  Solaris 11.4.
* This function does not support format directives that access arguments in an
  arbitrary order, such as `"%2$s"`, on some platforms:
  NetBSD 3.0.
* This function doesn’t support the `'` flag on some platforms:
  NetBSD 3.0, Cygwin 1.5.24.
* This function does not round the argument of the ‘`a`’ directive correctly
  on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4.
* printf `"%010f"` of NaN and Infinity yields an incorrect result (padded
  with zeroes) on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, AIX 7.3, Solaris 11.4, Cygwin 1.5.x.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* This function produces wrong output for the ‘`lc`’ directive with a NUL
  wide character argument on some platforms:
  musl libc 1.2.4.
* This function can crash in out-of-memory conditions on some platforms:
  FreeBSD 14.0, NetBSD 5.0.

Portability problems fixed by Gnulib module `vasprintf-gnu`:

* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.

Portability problems not fixed by Gnulib:

* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.

---

Next: [`vfprintf`](#vfprintf), Previous: [`vasprintf`](#vasprintf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.64 `vdprintf` [¶](#vdprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vdprintf.html>

Gnulib module: vdprintf or vdprintf-posix or vdprintf-gnu

Portability problems fixed by either Gnulib module `vdprintf` or `vdprintf-posix` or `vdprintf-gnu`:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.5.x, mingw, MSVC 14.

Portability problems fixed by either Gnulib module `vdprintf-posix` or `vdprintf-gnu`:

* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* printf `"%f"`, `"%e"`, `"%g"` of Infinity and NaN yields an
  incorrect result on some platforms:
  AIX 7.3, Solaris 11.4.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14.
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
* This function does not support the ‘`a`’ and ‘`A`’ directives on some
  platforms:
  Solaris 11.4.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support precisions in the ‘`ls`’ directive correctly
  on some platforms:
  Solaris 11.4.
* This function does not round the argument of the ‘`a`’ directive correctly
  on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4.
* printf `"%010f"` of NaN and Infinity yields an incorrect result (padded
  with zeroes, or wrong capitalization) on some platforms:
  AIX 7.3, Solaris 11.4.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* This function does not support precisions larger than 512 or 1024 in integer,
  floating-point and pointer output on some platforms:
  AIX 7.1.
* This function produces wrong output for the ‘`lc`’ directive with a NUL
  wide character argument on some platforms:
  musl libc 1.2.4.

Portability problems fixed by Gnulib module `vdprintf-gnu`:

* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.

Portability problems fixed by either Gnulib module `vdprintf-posix` or `vdprintf-gnu`
and additionally the Gnulib module `printf-with-n-directive`:

* This function does not support the ‘`n`’ directive on some platforms:
  glibc when used with `_FORTIFY_SOURCE >= 2` (set by default on Ubuntu),
  macOS 14, OpenBSD, Android.

Portability problems not fixed by Gnulib:

* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.
* Formatting noncanonical ‘`long double`’ numbers produces
  nonmeaningful results on some platforms:
  glibc and others, on x86, x86\_64, IA-64 CPUs.
* When formatting an integer with grouping flag, this function inserts thousands
  separators even in the "C" locale on some platforms:
  NetBSD 5.1.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Next: [`vfscanf`](#vfscanf), Previous: [`vdprintf`](#vdprintf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.65 `vfprintf` [¶](#vfprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vfprintf.html>

Gnulib module: vfprintf-posix or vfprintf-gnu or stdio-h, nonblocking, sigpipe

Portability problems fixed by either Gnulib module `stdio-h` or `fprintf-posix` or `fprintf-gnu`:

* This function does not support non-ASCII characters in double-byte encoding,
  corresponding to the locale, on some platforms:
  mingw with `__USE_MINGW_ANSI_STDIO` in combination with msvcrt,
  when the output goes to a Windows console.

Portability problems fixed by either Gnulib module `vfprintf-posix` or `vfprintf-gnu`:

* This function does not support size specifiers as in C99 (`hh`, `ll`,
  `j`, `t`, `z`) on some platforms:
  AIX 5.1, HP-UX 11.23, Solaris 9, Cygwin 1.5.24, old mingw, MSVC 9.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* printf of ‘`long double`’ numbers is unsupported on some platforms:
  mingw, MSVC 14.
* printf `"%f"`, `"%e"`, `"%g"` of Infinity and NaN yields an
  incorrect result on some platforms:
  AIX 7.3, Solaris 11.4, mingw, MSVC 14.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14.
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
* This function does not support the ‘`a`’ and ‘`A`’ directives on some
  platforms:
  FreeBSD 5.2.1, OpenBSD 4.0, AIX 5.2, HP-UX 11,
  Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`F`’ directive on some platforms:
  NetBSD 3.0, AIX 5.1, HP-UX 11.23, Solaris 9,
  Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`ls`’ directive on some platforms:
  OpenBSD 4.0, Cygwin 1.5.x, Haiku.
* This function does not support precisions in the ‘`ls`’ directive correctly
  on some platforms:
  Solaris 11.4.
* This function does not support format directives that access arguments in an
  arbitrary order, such as `"%2$s"`, on some platforms:
  NetBSD 3.0, mingw, MSVC 14.
* This function doesn’t support the `'` flag on some platforms:
  NetBSD 3.0, Cygwin 1.5.24, mingw, MSVC 14.
* This function does not round the argument of the ‘`a`’ directive correctly
  on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4.
* printf `"%010f"` of NaN and Infinity yields an incorrect result (padded
  with zeroes, or wrong capitalization) on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, AIX 7.3, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC/clang.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* This function does not support precisions larger than 512 or 1024 in integer,
  floating-point and pointer output on some platforms:
  AIX 7.1, Solaris 10/x86, mingw, MSVC/clang.
* This function mishandles large floating point precisions
  (for example, formatting 1.0 with ‘`"%.511f"`’)
  on some platforms:
  Solaris 10.
* This function produces wrong output for the ‘`lc`’ directive with a NUL
  wide character argument on some platforms:
  musl libc 1.2.4.
* This function can crash in out-of-memory conditions on some platforms:
  FreeBSD 14.0, NetBSD 5.0.

Portability problems fixed by Gnulib module `vfprintf-gnu`:

* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.

Portability problems fixed by either Gnulib module `vfprintf-posix` or `vfprintf-gnu`
and additionally the Gnulib module `printf-with-n-directive`:

* This function does not support the ‘`n`’ directive on some platforms:
  glibc when used with `_FORTIFY_SOURCE >= 2` (set by default on Ubuntu),
  macOS 14, OpenBSD, MSVC 14, Android.

Portability problems fixed by Gnulib module `stdio-h` or `vfprintf-posix` or `vfprintf-gnu`, together with module `nonblocking`:

* When writing to a non-blocking pipe whose buffer is full, this function fails
  with `errno` being set to `ENOSPC` instead of `EAGAIN` on some
  platforms:
  mingw, MSVC 14.

Portability problems fixed by Gnulib module `stdio-h` or `vfprintf-posix` or `vfprintf-gnu`, together with module `sigpipe`:

* When writing to a pipe with no readers, this function fails, instead of
  obeying the current `SIGPIPE` handler, on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.
* Formatting noncanonical ‘`long double`’ numbers produces
  nonmeaningful results on some platforms:
  glibc and others, on x86, x86\_64, IA-64 CPUs.
* When formatting an integer with grouping flag, this function inserts thousands
  separators even in the "C" locale on some platforms:
  NetBSD 5.1.
* Attempting to write to a read-only stream fails with `EOF` but
  does not set the error flag for `ferror` on some platforms:
  glibc 2.13, cygwin 1.7.9.

---

Next: [`vprintf`](#vprintf), Previous: [`vfprintf`](#vfprintf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.66 `vfscanf` [¶](#vfscanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vfscanf.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-vfscanf.html>

Gnulib module: vfscanf, nonblocking

Portability problems fixed by Gnulib module `vfscanf`, together with module `nonblocking`:

* When reading from a non-blocking pipe whose buffer is empty, this function
  fails with `errno` being set to `EINVAL` instead of `EAGAIN` on
  some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.38, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.2,
  AIX 7.2, HP-UX 11, Solaris 11.4, Cygwin 2.9.0, mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* On Windows, this function doesn’t support the `hh`, `ll`, `j`,
  `t`, `z` size specifiers.

---

Next: [`vscanf`](#vscanf), Previous: [`vfscanf`](#vfscanf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.67 `vprintf` [¶](#vprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vprintf.html>

Gnulib module: vprintf-posix or vprintf-gnu or stdio-h, nonblocking, sigpipe

Portability problems fixed by either Gnulib module `stdio-h` or `fprintf-posix` or `fprintf-gnu`:

* This function does not support non-ASCII characters in double-byte encoding,
  corresponding to the locale, on some platforms:
  mingw with `__USE_MINGW_ANSI_STDIO` in combination with msvcrt,
  when the output goes to a Windows console.

Portability problems fixed by either Gnulib module `vprintf-posix` or `vprintf-gnu`:

* This function does not support size specifiers as in C99 (`hh`, `ll`,
  `j`, `t`, `z`) on some platforms:
  AIX 5.1, HP-UX 11.23, Solaris 9, Cygwin 1.5.24, old mingw, MSVC 9.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* printf of ‘`long double`’ numbers is unsupported on some platforms:
  mingw, MSVC 14.
* printf `"%f"`, `"%e"`, `"%g"` of Infinity and NaN yields an
  incorrect result on some platforms:
  AIX 7.3, Solaris 11.4, mingw, MSVC 14.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14.
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
* This function does not support the ‘`a`’ and ‘`A`’ directives on some
  platforms:
  FreeBSD 5.2.1, OpenBSD 4.0, AIX 5.2, HP-UX 11,
  Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`F`’ directive on some platforms:
  NetBSD 3.0, AIX 5.1, HP-UX 11.23, Solaris 9,
  Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`ls`’ directive on some platforms:
  OpenBSD 4.0, Cygwin 1.5.x, Haiku.
* This function does not support precisions in the ‘`ls`’ directive correctly
  on some platforms:
  Solaris 11.4.
* This function does not support format directives that access arguments in an
  arbitrary order, such as `"%2$s"`, on some platforms:
  NetBSD 3.0, mingw, MSVC 14.
* This function doesn’t support the `'` flag on some platforms:
  NetBSD 3.0, Cygwin 1.5.24, mingw, MSVC 14.
* This function does not round the argument of the ‘`a`’ directive correctly
  on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4.
* printf `"%010f"` of NaN and Infinity yields an incorrect result (padded
  with zeroes, or wrong capitalization) on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, AIX 7.3, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC/clang.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* This function does not support precisions larger than 512 or 1024 in integer,
  floating-point and pointer output on some platforms:
  AIX 7.1, Solaris 10/x86, mingw, MSVC/clang.
* This function mishandles large floating point precisions
  (for example, formatting 1.0 with ‘`"%.511f"`’)
  on some platforms:
  Solaris 10.
* This function produces wrong output for the ‘`lc`’ directive with a NUL
  wide character argument on some platforms:
  musl libc 1.2.4.
* This function can crash in out-of-memory conditions on some platforms:
  FreeBSD 14.0, NetBSD 5.0.

Portability problems fixed by Gnulib module `vprintf-gnu`:

* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.

Portability problems fixed by either Gnulib module `vprintf-posix` or `vprintf-gnu`
and additionally the Gnulib module `printf-with-n-directive`:

* This function does not support the ‘`n`’ directive on some platforms:
  glibc when used with `_FORTIFY_SOURCE >= 2` (set by default on Ubuntu),
  macOS 14, OpenBSD, MSVC 14, Android.

Portability problems fixed by Gnulib module `stdio-h` or `vprintf-posix` or `vprintf-gnu`, together with module `nonblocking`:

* When writing to a non-blocking pipe whose buffer is full, this function fails
  with `errno` being set to `ENOSPC` instead of `EAGAIN` on some
  platforms:
  mingw, MSVC 14.

Portability problems fixed by Gnulib module `stdio-h` or `vprintf-posix` or `vprintf-gnu`, together with module `sigpipe`:

* When writing to a pipe with no readers, this function fails, instead of
  obeying the current `SIGPIPE` handler, on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.
* Formatting noncanonical ‘`long double`’ numbers produces
  nonmeaningful results on some platforms:
  glibc and others, on x86, x86\_64, IA-64 CPUs.
* When formatting an integer with grouping flag, this function inserts thousands
  separators even in the "C" locale on some platforms:
  NetBSD 5.1.
* Attempting to write to a read-only stream fails with `EOF` but
  does not set the error flag for `ferror` on some platforms:
  glibc 2.13, cygwin 1.7.9.

---

Next: [`vsnprintf`](#vsnprintf), Previous: [`vprintf`](#vprintf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.68 `vscanf` [¶](#vscanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vscanf.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-vscanf.html>

Gnulib module: vscanf, nonblocking

Portability problems fixed by Gnulib module `vscanf`, together with module `nonblocking`:

* When reading from a non-blocking pipe whose buffer is empty, this function
  fails with `errno` being set to `EINVAL` instead of `EAGAIN` on
  some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.38, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.2,
  AIX 7.2, HP-UX 11, Solaris 11.4, Cygwin 2.9.0, mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* On Windows, these functions don’t support the `hh`, `ll`, `j`,
  `t`, `z` size specifiers.

---

Next: [`vsprintf`](#vsprintf), Previous: [`vscanf`](#vscanf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.69 `vsnprintf` [¶](#vsnprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vsnprintf.html>

Gnulib module: vsnprintf or vsnprintf-posix or vsnprintf-gnu

Portability problems fixed by either Gnulib module `vsnprintf` or `vsnprintf-posix` or `vsnprintf-gnu`:

* This function does not support format directives that access arguments in an
  arbitrary order, such as `"%2$s"`, on some platforms:
  NetBSD 3.0, mingw, MSVC 14.
* This function does not return a byte count as specified in C99 on some
  platforms:
  HP-UX 11, Solaris 9, mingw, MSVC 14.

Portability problems fixed by either Gnulib module `vsnprintf-posix` or `vsnprintf-gnu`:

* This function does not support size specifiers as in C99 (`hh`, `ll`,
  `j`, `t`, `z`) on some platforms:
  AIX 5.1, HP-UX 11.23, Solaris 9, Cygwin 1.5.24, old mingw, MSVC 9.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* printf of ‘`long double`’ numbers is unsupported on some platforms:
  mingw, MSVC 14.
* printf `"%f"`, `"%e"`, `"%g"` of Infinity and NaN yields an
  incorrect result on some platforms:
  AIX 7.3, Solaris 11.4, mingw, MSVC 14.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14.
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
* This function does not support the ‘`a`’ and ‘`A`’ directives on some
  platforms:
  FreeBSD 5.2.1, OpenBSD 4.0, AIX 5.2, HP-UX 11,
  Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`F`’ directive on some platforms:
  NetBSD 3.0, AIX 5.1, HP-UX 11.23, Solaris 9,
  Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`ls`’ directive on some platforms:
  OpenBSD 4.0, Cygwin 1.5.x, Haiku.
* This function does not support precisions in the ‘`ls`’ directive correctly
  on some platforms:
  Solaris 11.4.
* This function doesn’t support the `'` flag on some platforms:
  NetBSD 3.0, Cygwin 1.5.24, mingw, MSVC 14.
* This function does not round the argument of the ‘`a`’ directive correctly
  on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4.
* printf `"%010f"` of NaN and Infinity yields an incorrect result (padded
  with zeroes, or wrong capitalization) on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, AIX 7.3, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC/clang.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* This function does not support precisions larger than 512 or 1024 in integer,
  floating-point and pointer output on some platforms:
  AIX 7.1, Solaris 10/x86, mingw, MSVC/clang.
* This function mishandles large floating point precisions
  (for example, formatting 1.0 with ‘`"%.511f"`’)
  on some platforms:
  Solaris 10.
* This function produces wrong output for the ‘`lc`’ directive with a NUL
  wide character argument on some platforms:
  musl libc 1.2.4.
* This function can crash in out-of-memory conditions on some platforms:
  FreeBSD 14.0, NetBSD 5.0.
* This function does not truncate the result as specified in C99 on some platforms:
  mingw, MSVC 14.
* This function overwrites memory even when a zero size argument is passed on some
  platforms:
  HP-UX 11.

Portability problems fixed by Gnulib module `vsnprintf-gnu`:

* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.

Portability problems fixed by either Gnulib module `vsnprintf-posix` or `vsnprintf-gnu`
and additionally the Gnulib module `printf-with-n-directive`:

* This function does not fully support the ‘`n`’ directive on some platforms:
  glibc when used with `_FORTIFY_SOURCE >= 2` (set by default on Ubuntu),
  macOS 14, OpenBSD, HP-UX 11, mingw, MSVC 14, Android.

Portability problems not fixed by Gnulib:

* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.
* Formatting noncanonical ‘`long double`’ numbers produces
  nonmeaningful results on some platforms:
  glibc and others, on x86, x86\_64, IA-64 CPUs.
* When formatting an integer with grouping flag, this function inserts thousands
  separators even in the "C" locale on some platforms:
  NetBSD 5.1.

---

Next: [`vsscanf`](#vsscanf), Previous: [`vsnprintf`](#vsnprintf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.70 `vsprintf` [¶](#vsprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vsprintf.html>

Gnulib module: vsprintf-posix or vsprintf-gnu

Portability problems fixed by either Gnulib module `vsprintf-posix` or `vsprintf-gnu`:

* This function does not support size specifiers as in C99 (`hh`, `ll`,
  `j`, `t`, `z`) on some platforms:
  AIX 5.1, HP-UX 11.23, Solaris 9, Cygwin 1.5.24, old mingw, MSVC 9.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.
* printf of ‘`long double`’ numbers is unsupported on some platforms:
  mingw, MSVC 14.
* printf `"%f"`, `"%e"`, `"%g"` of Infinity and NaN yields an
  incorrect result on some platforms:
  AIX 7.3, Solaris 11.4, mingw, MSVC 14.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14.
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
* This function does not support the ‘`a`’ and ‘`A`’ directives on some
  platforms:
  FreeBSD 5.2.1, OpenBSD 4.0, AIX 5.2, HP-UX 11,
  Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`F`’ directive on some platforms:
  NetBSD 3.0, AIX 5.1, HP-UX 11.23, Solaris 9,
  Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`ls`’ directive on some platforms:
  OpenBSD 4.0, Cygwin 1.5.x, Haiku.
* This function does not support precisions in the ‘`ls`’ directive correctly
  on some platforms:
  Solaris 11.4.
* This function does not support format directives that access arguments in an
  arbitrary order, such as `"%2$s"`, on some platforms:
  NetBSD 3.0, mingw, MSVC 14.
* This function doesn’t support the `'` flag on some platforms:
  NetBSD 3.0, Cygwin 1.5.24, mingw, MSVC 14.
* This function does not round the argument of the ‘`a`’ directive correctly
  on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4.
* printf `"%010f"` of NaN and Infinity yields an incorrect result (padded
  with zeroes, or wrong capitalization) on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, AIX 7.3, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC/clang.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* This function does not support precisions larger than 512 or 1024 in integer,
  floating-point and pointer output on some platforms:
  AIX 7.1, Solaris 10/x86, mingw, MSVC/clang.
* This function mishandles large floating point precisions
  (for example, formatting 1.0 with ‘`"%.511f"`’)
  on some platforms:
  Solaris 10.
* This function produces wrong output for the ‘`lc`’ directive with a NUL
  wide character argument on some platforms:
  musl libc 1.2.4.
* This function can crash in out-of-memory conditions on some platforms:
  FreeBSD 14.0, NetBSD 5.0.
* The compiler warns that this function is deprecated:
  macOS 14.

Portability problems fixed by Gnulib module `vsprintf-gnu`:

* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.

Portability problems fixed by either Gnulib module `vsprintf-posix` or `vsprintf-gnu`
and additionally the Gnulib module `printf-with-n-directive`:

* This function does not support the ‘`n`’ directive on some platforms:
  glibc when used with `_FORTIFY_SOURCE >= 2` (set by default on Ubuntu),
  macOS 14, OpenBSD, MSVC 14, Android.

Portability problems not fixed by Gnulib:

* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.
* Formatting noncanonical ‘`long double`’ numbers produces
  nonmeaningful results on some platforms:
  glibc and others, on x86, x86\_64, IA-64 CPUs.
* When formatting an integer with grouping flag, this function inserts thousands
  separators even in the "C" locale on some platforms:
  NetBSD 5.1.

---

Previous: [`vsprintf`](#vsprintf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.71 `vsscanf` [¶](#vsscanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/vsscanf.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-vsscanf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 14.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.38, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.2,
  AIX 7.2, HP-UX 11, Solaris 11.4, Cygwin 2.9.0, mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* On Windows, these functions don’t support the `hh`, `ll`, `j`,
  `t`, `z` size specifiers.

---

Next: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e), Previous: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
