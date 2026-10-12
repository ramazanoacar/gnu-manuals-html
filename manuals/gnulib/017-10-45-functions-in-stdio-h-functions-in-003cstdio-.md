### 10.45 Functions in `<stdio.h>` [¶](#Functions-in-_003cstdio_002eh_003e-1)

* [`asprintf`](#asprintf)
* [`clearerr`](#clearerr)
* [`ctermid`](#ctermid)
* [`dprintf`](#dprintf)
* [`fclose`](#fclose)
* [`fdopen`](#fdopen)
* [`feof`](#feof)
* [`ferror`](#ferror)
* [`fflush`](#fflush)
* [`fgetc`](#fgetc)
* [`fgetpos`](#fgetpos)
* [`fgets`](#fgets)
* [`fileno`](#fileno)
* [`flockfile`](#flockfile)
* [`fmemopen`](#fmemopen)
* [`fopen`](#fopen)
* [`fprintf`](#fprintf)
* [`fputc`](#fputc)
* [`fputs`](#fputs)
* [`fread`](#fread)
* [`freopen`](#freopen)
* [`fscanf`](#fscanf)
* [`fseek`](#fseek)
* [`fseeko`](#fseeko)
* [`fsetpos`](#fsetpos)
* [`ftell`](#ftell)
* [`ftello`](#ftello)
* [`ftrylockfile`](#ftrylockfile)
* [`funlockfile`](#funlockfile)
* [`fwrite`](#fwrite)
* [`getc`](#getc)
* [`getc_unlocked`](#getc_005funlocked)
* [`getchar`](#getchar)
* [`getchar_unlocked`](#getchar_005funlocked)
* [`getdelim`](#getdelim)
* [`getline`](#getline)
* [`open_memstream`](#open_005fmemstream)
* [`pclose`](#pclose)
* [`perror`](#perror)
* [`popen`](#popen)
* [`printf`](#printf)
* [`putc`](#putc)
* [`putc_unlocked`](#putc_005funlocked)
* [`putchar`](#putchar)
* [`putchar_unlocked`](#putchar_005funlocked)
* [`puts`](#puts)
* [`remove`](#remove)
* [`rename`](#rename)
* [`renameat`](#renameat)
* [`rewind`](#rewind)
* [`scanf`](#scanf)
* [`setbuf`](#setbuf)
* [`setvbuf`](#setvbuf)
* [`snprintf`](#snprintf)
* [`sprintf`](#sprintf)
* [`sscanf`](#sscanf)
* [`stderr`](#stderr)
* [`stdin`](#stdin)
* [`stdout`](#stdout)
* [`tmpfile`](#tmpfile)
* [`tmpnam`](#tmpnam)
* [`ungetc`](#ungetc)
* [`vasprintf`](#vasprintf)
* [`vdprintf`](#vdprintf)
* [`vfprintf`](#vfprintf)
* [`vfscanf`](#vfscanf)
* [`vprintf`](#vprintf)
* [`vscanf`](#vscanf)
* [`vsnprintf`](#vsnprintf)
* [`vsprintf`](#vsprintf)
* [`vsscanf`](#vsscanf)

---

Next: [`clearerr`](#clearerr), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.1 `asprintf` [¶](#asprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/asprintf.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Dynamic-Output.html>,
* [man asprintf](https://www.kernel.org/doc/man-pages/online/pages/man3/asprintf.3.html).

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
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14.
* This function doesn’t support the `'` flag correctly for integers
  (it uses the decimal point instead of the thousands separator!)
  on some platforms:
  mingw with `__USE_MINGW_ANSI_STDIO`.
* This function doesn’t support the `'` flag on some platforms:
  NetBSD 3.0, Cygwin 1.5.24.
* This function doesn’t support the `'` flag together with a precision
  on an integer argument correctly on some platforms:
  glibc 2.36, FreeBSD 14.0, NetBSD 10.0, Solaris 11, Cygwin 3.6.0, Haiku.
* This function doesn’t support the `'` flag correctly in locales where
  the thousands separator is a multibyte character on some platforms:
  NetBSD 10.0, Solaris 11 OpenIndiana.
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

Next: [`ctermid`](#ctermid), Previous: [`asprintf`](#asprintf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.2 `clearerr` [¶](#clearerr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/clearerr.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`dprintf`](#dprintf), Previous: [`clearerr`](#clearerr), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.3 `ctermid` [¶](#ctermid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ctermid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 7.1.

---

Next: [`fclose`](#fclose), Previous: [`ctermid`](#ctermid), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.4 `dprintf` [¶](#dprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dprintf.html>

Gnulib module: dprintf or dprintf-posix or dprintf-gnu

Portability problems fixed by either Gnulib module `dprintf` or `dprintf-posix` or `dprintf-gnu`:

* This function is missing on many non-glibc platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.5.x, mingw, MSVC 14.

Portability problems fixed by either Gnulib module `dprintf-posix` or `dprintf-gnu`:

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

Portability problems fixed by Gnulib module `dprintf-gnu`:

* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.

Portability problems fixed by either Gnulib module `dprintf-posix` or `dprintf-gnu`
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

Next: [`fdopen`](#fdopen), Previous: [`dprintf`](#dprintf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.5 `fclose` [¶](#fclose-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fclose.html>

Gnulib module: fclose

Portability problems fixed by Gnulib:

* On some platforms, this function fails to set the file position of a
  seekable input stream to the byte after the last one actually read:
  glibc 2.34, FreeBSD, AIX 7.2.
* This function crashes if the stream’s file descriptor has already been
  closed on some platforms:
  MSVC 14.
* On Windows platforms (excluding Cygwin), `socket` and `accept`
  followed by `fdopen` do not return streams that can be closed by
  `fclose`.

Portability problems not fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.

---

Next: [`feof`](#feof), Previous: [`fclose`](#fclose), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.6 `fdopen` [¶](#fdopen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fdopen.html>

Gnulib module: fdopen

Portability problems fixed by Gnulib:

* This function crashes when invoked with invalid arguments on some platforms:
  MSVC 14.
* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.

Portability problems not fixed by Gnulib:

---

Next: [`ferror`](#ferror), Previous: [`fdopen`](#fdopen), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.7 `feof` [¶](#feof-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/feof.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`fflush`](#fflush), Previous: [`feof`](#feof), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.8 `ferror` [¶](#ferror-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ferror.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`fgetc`](#fgetc), Previous: [`ferror`](#ferror), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.9 `fflush` [¶](#fflush-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fflush.html>

Gnulib module: fflush

Portability problems fixed by Gnulib:

* `fflush` followed by `fseek` or `fseeko`, applied to an input
  stream, should have the effect of positioning the underlying file descriptor.
  It doesn’t do this on some platforms:
  glibc 2.34, FreeBSD 14.0, and others.
* `fflush` on an input stream changes the position of the stream to the
  end of the previous buffer, on some platforms: mingw, MSVC 14.
* `fflush` on an input stream right after `ungetc` does not discard
  the `ungetc` buffer, on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Cygwin 1.5.25-10.
* `fflush` fails with `EBADF` if the stream is not open for writing:
  NetBSD 10.0, OpenBSD 7.9.

Portability problems not fixed by Gnulib:

* `fflush`, `ftell`, `ftello`, `fgetpos` behave incorrectly
  on input streams that are opened in `O_TEXT` mode and whose contents
  contains Unix line terminators (LF), on some platforms: mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* This function crashes if the stream’s file descriptor has already been
  closed, if `MSVC_INVALID_PARAMETER_HANDLING` is
  `HAIRY_LIBRARY_HANDLING` or `SANE_LIBRARY_HANDLING`,
  on some platforms:
  MSVC 14.
* `fflush` on an input stream right after `ungetc` does not discard
  the `ungetc` buffer, on some platforms:
  AIX 5.1, HP-UX 11, Solaris 11 2010-11, mingw, MSVC 14.

---

Next: [`fgetpos`](#fgetpos), Previous: [`fflush`](#fflush), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.10 `fgetc` [¶](#fgetc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgetc.html>

Gnulib module: stdio-h, nonblocking

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When reading from a non-blocking pipe whose buffer is empty, this function
  fails with `errno` being set to `EINVAL` instead of `EAGAIN` on
  some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* C99 and POSIX.1-2001 and later require end-of-file to be sticky, that
  is, they require this function to act as if it reads end-of-file if
  `feof` would return nonzero. However, on some systems this
  function attempts to read from the underlying file descriptor even if
  the stream’s end-of-file indicator is set. These systems include
  glibc and default Solaris.
* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* This function crashes if the stream’s file descriptor has already been
  closed, if `MSVC_INVALID_PARAMETER_HANDLING` is
  `HAIRY_LIBRARY_HANDLING` or `SANE_LIBRARY_HANDLING`,
  on some platforms:
  MSVC 14.

---

Next: [`fgets`](#fgets), Previous: [`fgetc`](#fgetc), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.11 `fgetpos` [¶](#fgetpos-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgetpos.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* `fflush`, `ftell`, `ftello`, `fgetpos` behave incorrectly
  on input streams that are opened in `O_TEXT` mode and whose contents
  contains Unix line terminators (LF), on some platforms: mingw, MSVC 14.
* This function mistakenly succeeds on pipes on some platforms:
  mingw 10.

---

Next: [`fileno`](#fileno), Previous: [`fgetpos`](#fgetpos), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.12 `fgets` [¶](#fgets-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgets.html>

Gnulib module: stdio-h, nonblocking

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When reading from a non-blocking pipe whose buffer is empty, this function
  fails with `errno` being set to `EINVAL` instead of `EAGAIN` on
  some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* C99 and POSIX.1-2001 and later require end-of-file to be sticky, that
  is, they require this function to act as if it reads end-of-file if
  `feof` would return nonzero. However, on some systems this
  function attempts to read from the underlying file descriptor even if
  the stream’s end-of-file indicator is set. These systems include
  glibc and default Solaris.
* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.

---

Next: [`flockfile`](#flockfile), Previous: [`fgets`](#fgets), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.13 `fileno` [¶](#fileno-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fileno.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`fmemopen`](#fmemopen), Previous: [`fileno`](#fileno), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.14 `flockfile` [¶](#flockfile-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/flockfile.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`fopen`](#fopen), Previous: [`flockfile`](#flockfile), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.15 `fmemopen` [¶](#fmemopen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fmemopen.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.5.x, mingw, MSVC 14, Android 5.1.

An alternative to the `fmemopen` function is the Gnulib module
`sf-istream`.

---

Next: [`fprintf`](#fprintf), Previous: [`fmemopen`](#fmemopen), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.16 `fopen` [¶](#fopen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fopen.html>

Gnulib module: fopen or fopen-gnu

Portability problems fixed by either Gnulib module `fopen` or `fopen-gnu`:

* This function does not fail when the file name argument ends in a slash
  and (without the slash) names a nonexistent file or a file that is not a
  directory, on some platforms:
  HP-UX 11.00, AIX 7.1, Solaris 9.
* On platforms where `off_t` is a 32-bit type, `fopen` may not work
  correctly with files 2 GiB and larger. See [Large File Support](#Large-File-Support).
* On Windows platforms (excluding Cygwin), this function does usually not
  recognize the `/dev/null` filename.

Portability problems fixed by Gnulib module `fopen-gnu`:

* This function does not support the mode character
  ‘`x`’ (corresponding to `O_EXCL`), introduced in ISO C11,
  on some platforms:
  FreeBSD 8.2, NetBSD 6.1, OpenBSD 5.6, Minix 3.2, AIX 6.1, HP-UX 11.31, Solaris 11.3, Cygwin 1.7.16 (2012), mingw with MSVCRT, MSVC 14.
* This function does not support the mode character
  ‘`e`’ (corresponding to `O_CLOEXEC`), introduced in
  POSIX.1-2024, on some platforms:
  glibc 2.6, macOS 14, FreeBSD 9.0, NetBSD 5.1, OpenBSD 5.6, Minix 3.2, AIX 7.2, HP-UX 11.31, Solaris 11.3, Cygwin 1.7.16 (2012), mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* On Windows, this function returns a file stream in “text” mode by default;
  this means that it translates `'\n'` to CR/LF by default. Use the
  `"b"` flag if you need reliable binary I/O.
* On Windows platforms (excluding Cygwin), this function fails to open
  directories for reading. Such streams have implementation-defined
  semantics on other platforms. To avoid directory streams with a
  consistent error message, use `fstat` after `open` and
  `fdopen`, rather than `fopen` and `fileno`.

---

Next: [`fputc`](#fputc), Previous: [`fopen`](#fopen), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.17 `fprintf` [¶](#fprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fprintf.html>

Gnulib module: fprintf-posix or fprintf-gnu or stdio-h, nonblocking, sigpipe

Portability problems fixed by either Gnulib module `stdio-h` or `fprintf-posix` or `fprintf-gnu`:

* This function does not support non-ASCII characters in double-byte encoding,
  corresponding to the locale, on some platforms:
  mingw with `__USE_MINGW_ANSI_STDIO` in combination with msvcrt,
  when the output goes to a Windows console.

Portability problems fixed by either Gnulib module `fprintf-posix` or `fprintf-gnu`:

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
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14.
* This function doesn’t support the `'` flag correctly for integers
  (it uses the decimal point instead of the thousands separator!)
  on some platforms:
  mingw with `__USE_MINGW_ANSI_STDIO`.
* This function doesn’t support the `'` flag on some platforms:
  NetBSD 3.0, Cygwin 1.5.24, mingw, MSVC 14.
* This function doesn’t support the `'` flag together with a precision
  on an integer argument correctly on some platforms:
  glibc 2.36, FreeBSD 14.0, NetBSD 10.0, Solaris 11, Cygwin 3.6.0, Haiku.
* This function doesn’t support the `'` flag correctly in locales where
  the thousands separator is a multibyte character on some platforms:
  NetBSD 10.0, Solaris 11 OpenIndiana.
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

Portability problems fixed by Gnulib module `fprintf-gnu`:

* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.

Portability problems fixed by either Gnulib module `fprintf-posix` or `fprintf-gnu`
and additionally the Gnulib module `printf-with-n-directive`:

* This function does not support the ‘`n`’ directive on some platforms:
  glibc when used with `_FORTIFY_SOURCE >= 2` (set by default on Ubuntu),
  macOS 14, OpenBSD, MSVC 14, Android.

Portability problems fixed by Gnulib module `stdio-h` or `fprintf-posix` or `fprintf-gnu`, together with module `nonblocking`:

* When writing to a non-blocking pipe whose buffer is full, this function fails
  with `errno` being set to `ENOSPC` instead of `EAGAIN` on some
  platforms:
  mingw, MSVC 14.

Portability problems fixed by Gnulib module `stdio-h` or `fprintf-posix` or `fprintf-gnu`, together with module `sigpipe`:

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

Next: [`fputs`](#fputs), Previous: [`fprintf`](#fprintf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.18 `fputc` [¶](#fputc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fputc.html>

Gnulib module: stdio-h, nonblocking, sigpipe

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When writing to a non-blocking pipe whose buffer is full, this function fails
  with `errno` being set to `ENOSPC` instead of `EAGAIN` on some
  platforms:
  mingw, MSVC 14.

Portability problems fixed by Gnulib module `stdio-h`, together with module `sigpipe`:

* When writing to a pipe with no readers, this function fails, instead of
  obeying the current `SIGPIPE` handler, on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* This function fails and produces garbled output
  when invoked twice, for outputting a non-ASCII character in double-byte encoding,
  corresponding to the locale, on some platforms:
  mingw in combination with msvcrt,
  when the output goes to a Windows console.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.
* This function crashes if the stream’s file descriptor has already been
  closed, if `MSVC_INVALID_PARAMETER_HANDLING` is
  `HAIRY_LIBRARY_HANDLING` or `SANE_LIBRARY_HANDLING`,
  on some platforms:
  MSVC 14.

---

Next: [`fread`](#fread), Previous: [`fputc`](#fputc), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.19 `fputs` [¶](#fputs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fputs.html>

Gnulib module: stdio-h, nonblocking, sigpipe

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When writing to a non-blocking pipe whose buffer is full, this function fails
  with `errno` being set to `ENOSPC` instead of `EAGAIN` on some
  platforms:
  mingw, MSVC 14.

Portability problems fixed by Gnulib module `stdio-h`, together with module `sigpipe`:

* When writing to a pipe with no readers, this function fails, instead of
  obeying the current `SIGPIPE` handler, on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Next: [`freopen`](#freopen), Previous: [`fputs`](#fputs), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.20 `fread` [¶](#fread-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fread.html>

Gnulib module: stdio-h, nonblocking

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When reading from a non-blocking pipe whose buffer is empty, this function
  fails with `errno` being set to `EINVAL` instead of `EAGAIN` on
  some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* C99 and POSIX.1-2001 and later require end-of-file to be sticky, that
  is, they require this function to act as if it reads end-of-file if
  `feof` would return nonzero. However, on some systems this
  function attempts to read from the underlying file descriptor even if
  the stream’s end-of-file indicator is set. These systems include
  glibc and default Solaris.
* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* This function crashes if the stream’s file descriptor has already been
  closed, if `MSVC_INVALID_PARAMETER_HANDLING` is
  `HAIRY_LIBRARY_HANDLING` or `SANE_LIBRARY_HANDLING`,
  on some platforms:
  MSVC 14.

---

Next: [`fscanf`](#fscanf), Previous: [`fread`](#fread), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.21 `freopen` [¶](#freopen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/freopen.html>

Gnulib module: freopen

Portability problems fixed by Gnulib:

* On some platforms, if `stream` does not already have an open
  file descriptor, `freopen` returns the stream without opening
  the file: glibc 2.24.
* On platforms where `off_t` is a 32-bit type, `freopen` may not work
  correctly with files 2 GiB and larger. See [Large File Support](#Large-File-Support).
* On Windows platforms (excluding Cygwin), this function does usually not
  recognize the `/dev/null` filename.

Portability problems not fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* This function does not support a `NULL` file name argument on some
  platforms:
  OpenBSD 4.9, AIX 7.1, HP-UX 11.23, Solaris 10, mingw, MSVC 14.
* This function does not fail when the file name argument ends in a slash
  and (without the slash) names a nonexistent file or a file that is not a
  directory, on some platforms:
  HP-UX 11.00, Solaris 9.
* Applications should not assume that `fileno(f)` will be the same
  before and after a call to `freopen(name,mode,f)`.
  However, the
  module `freopen-safer` can at least protect `stdin`, `stdout`,
  and `stderr`.

---

Next: [`fseek`](#fseek), Previous: [`freopen`](#freopen), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.22 `fscanf` [¶](#fscanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fscanf.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-fscanf.html>

Gnulib module: stdio-h, nonblocking

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When reading from a non-blocking pipe whose buffer is empty, this function
  fails with `errno` being set to `EINVAL` instead of `EAGAIN` on
  some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* C99 and POSIX.1-2001 and later require end-of-file to be sticky, that
  is, they require this function to act as if it reads end-of-file if
  `feof` would return nonzero. However, on some systems this
  function attempts to read from the underlying file descriptor even if
  the stream’s end-of-file indicator is set. These systems include
  glibc and default Solaris.
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

Next: [`fseeko`](#fseeko), Previous: [`fscanf`](#fscanf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.23 `fseek` [¶](#fseek-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fseek.html>

Gnulib module: fseek

Portability problems fixed by Gnulib:

* This function mistakenly succeeds on pipes on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* On platforms where `long` is a 32-bit type, `fseek` does not work
  correctly with files 2 GiB and larger, even when the `AC_SYS_LARGEFILE`
  macro is used. The fix is to use `fseeko` instead.

---

Next: [`fsetpos`](#fsetpos), Previous: [`fseek`](#fseek), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.24 `fseeko` [¶](#fseeko-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fseeko.html>

Gnulib module: fseeko

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* The declaration of `fseeko` in `<stdio.h>` is not enabled by default
  on some platforms:
  glibc 2.3.6.
* This function fails on seekable stdin, stdout, and stderr: cygwin <= 1.5.24.
* On platforms where `off_t` is a 32-bit type, `fseeko` does not work
  correctly with files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

---

Next: [`ftell`](#ftell), Previous: [`fseeko`](#fseeko), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.25 `fsetpos` [¶](#fsetpos-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fsetpos.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

---

Next: [`ftello`](#ftello), Previous: [`fsetpos`](#fsetpos), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.26 `ftell` [¶](#ftell-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ftell.html>

Gnulib module: ftell

Portability problems fixed by Gnulib:

* This function mistakenly succeeds on pipes on some platforms:
  mingw, MSVC 14.
* This function produces incorrect results after `putc` that followed a
  `getc` call that reached EOF on some platforms:
  Solaris 11 2010-11.
* This function, when invoked after `ungetc`, throws away the `ungetc`
  buffer, changes the stream’s file position, and returns the wrong position on
  some platforms:
  macOS 10.15, 11, 12.
* This function mistakenly succeeds on pipes on some platforms:
  mingw 10.

Portability problems not fixed by Gnulib:

* This function produces incorrect results immediately after `fseek` on some
  platforms:
  HP-UX 11.
* `fflush`, `ftell`, `ftello`, `fgetpos` behave incorrectly
  on input streams that are opened in `O_TEXT` mode and whose contents
  contains Unix line terminators (LF), on some platforms: mingw, MSVC 14.
* On platforms where `long` is a 32-bit type, `ftell` does not work
  correctly with files 2 GiB and larger, even when the `AC_SYS_LARGEFILE`
  macro is used. The fix is to use `ftello` instead.

---

Next: [`ftrylockfile`](#ftrylockfile), Previous: [`ftell`](#ftell), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.27 `ftello` [¶](#ftello-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ftello.html>

Gnulib module: ftello

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* The declaration of `ftello` in `<stdio.h>` is not enabled by default
  on some platforms:
  glibc 2.3.6.
* This function produces incorrect results after `putc` that followed a
  `getc` call that reached EOF on some platforms:
  Solaris 11 2010-11.
* This function, when invoked after `ungetc`, throws away the `ungetc`
  buffer, changes the stream’s file position, and returns the wrong position on
  some platforms:
  macOS 10.15, 11, 12.
* This function fails on seekable stdin, stdout, and stderr: cygwin <= 1.5.24.
* This function mistakenly succeeds on pipes on some platforms:
  mingw 10.
* On platforms where `off_t` is a 32-bit type, `ftello` does not work
  correctly with files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function produces incorrect results immediately after `fseek` on some
  platforms:
  HP-UX 11.
* `fflush`, `ftell`, `ftello`, `fgetpos` behave incorrectly
  on input streams that are opened in `O_TEXT` mode and whose contents
  contains Unix line terminators (LF), on some platforms: mingw, MSVC 14.

---

Next: [`funlockfile`](#funlockfile), Previous: [`ftello`](#ftello), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.28 `ftrylockfile` [¶](#ftrylockfile-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ftrylockfile.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`fwrite`](#fwrite), Previous: [`ftrylockfile`](#ftrylockfile), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.29 `funlockfile` [¶](#funlockfile-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/funlockfile.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`getc`](#getc), Previous: [`funlockfile`](#funlockfile), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.30 `fwrite` [¶](#fwrite-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fwrite.html>

Gnulib module: stdio-h, nonblocking, sigpipe

Portability problems fixed by Gnulib module `stdio-h`:

* This function fails and produces no output
  when the argument string starts with a non-ASCII character in double-byte encoding,
  corresponding to the locale, on some platforms:
  mingw in combination with msvcrt,
  when the output goes to a Windows console.

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When writing to a non-blocking pipe whose buffer is full, this function fails
  with `errno` being set to `ENOSPC` instead of `EAGAIN` on some
  platforms:
  mingw, MSVC 14.

Portability problems fixed by Gnulib module `stdio-h`, together with module `sigpipe`:

* When writing to a pipe with no readers, this function fails, instead of
  obeying the current `SIGPIPE` handler, on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.
* This function crashes if the stream’s file descriptor has already been
  closed, if `MSVC_INVALID_PARAMETER_HANDLING` is
  `HAIRY_LIBRARY_HANDLING` or `SANE_LIBRARY_HANDLING`,
  on some platforms:
  MSVC 14.

---

Next: [`getc_unlocked`](#getc_005funlocked), Previous: [`fwrite`](#fwrite), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.31 `getc` [¶](#getc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getc.html>

Gnulib module: stdio-h, nonblocking

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When reading from a non-blocking pipe whose buffer is empty, this function
  fails with `errno` being set to `EINVAL` instead of `EAGAIN` on
  some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* C99 and POSIX.1-2001 and later require end-of-file to be sticky, that
  is, they require this function to act as if it reads end-of-file if
  `feof` would return nonzero. However, on some systems this
  function attempts to read from the underlying file descriptor even if
  the stream’s end-of-file indicator is set. These systems include
  glibc and default Solaris.
* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.

---

Next: [`getchar`](#getchar), Previous: [`getc`](#getc), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.32 `getc_unlocked` [¶](#getc_005funlocked-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getc_unlocked.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`getchar_unlocked`](#getchar_005funlocked), Previous: [`getc_unlocked`](#getc_005funlocked), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.33 `getchar` [¶](#getchar-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getchar.html>

Gnulib module: stdio-h, nonblocking

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When reading from a non-blocking pipe whose buffer is empty, this function
  fails with `errno` being set to `EINVAL` instead of `EAGAIN` on
  some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* C99 and POSIX.1-2001 and later require end-of-file to be sticky, that
  is, they require this function to act as if it reads end-of-file if
  `feof` would return nonzero. However, on some systems this
  function attempts to read from the underlying file descriptor even if
  the stream’s end-of-file indicator is set. These systems include
  glibc and default Solaris.
* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.

---

Next: [`getdelim`](#getdelim), Previous: [`getchar`](#getchar), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.34 `getchar_unlocked` [¶](#getchar_005funlocked-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getchar_unlocked.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`getline`](#getline), Previous: [`getchar_unlocked`](#getchar_005funlocked), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.35 `getdelim` [¶](#getdelim-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getdelim.html>

Gnulib module: getdelim

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, mingw, MSVC 14, Android 4.2.
* This function makes out-of-bounds reads on some platforms:
  macOS 10.13.
* This function crashes when passed a pointer to a NULL buffer together with a
  pointer to a non-zero buffer size on some platforms:
  FreeBSD 8.0.

Portability problems not fixed by Gnulib:

* This function has quadratic running time for long lines on some platforms:
  uClibc 0.9.31.
* This function does not NUL terminate the buffer when the first
  character read is EOF on some platforms:
  glibc 2.42.

---

Next: [`open_memstream`](#open_005fmemstream), Previous: [`getdelim`](#getdelim), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.36 `getline` [¶](#getline-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getline.html>

Gnulib module: getline

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, mingw, MSVC 14, Android 4.2.
* This function is missing a declaration on some platforms:
  AIX 7.1.
* Some platforms provide a function by this name but with the wrong
  signature, for example in -linet.
* This function crashes when passed a pointer to a NULL buffer together with a
  pointer to a non-zero buffer size on some platforms:
  FreeBSD 8.0.

Portability problems not fixed by Gnulib:

* This function has quadratic running time for long lines on some platforms:
  uClibc 0.9.31.
* This function does not NUL terminate the buffer when the first
  character read is EOF on some platforms:
  glibc 2.42.

---

Next: [`pclose`](#pclose), Previous: [`getline`](#getline), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.37 `open_memstream` [¶](#open_005fmemstream-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/open_memstream.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 7.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.5.x, mingw, MSVC 14, Android 5.1.

An alternative to the `open_memstream` function is the Gnulib module
`string-buffer`.

---

Next: [`perror`](#perror), Previous: [`open_memstream`](#open_005fmemstream), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.38 `pclose` [¶](#pclose-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pclose.html>

Gnulib module: pclose

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`popen`](#popen), Previous: [`pclose`](#pclose), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.39 `perror` [¶](#perror-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/perror.html>

Gnulib module: perror

Portability problems fixed by Gnulib:

* This function does not support the error values that are specified by POSIX
  but not defined by the system, on some platforms:
  OpenBSD 4.0, Cygwin 1.5.x, mingw, MSVC 14.
* This function treats `errno` of 0 like failure, although POSIX
  requires that the message declare it as a success, on some platforms:
  FreeBSD 8.2, OpenBSD 4.7, macOS 14.
* This function clobbers the `strerror` buffer on some platforms:
  Cygwin 1.7.9, Android 11.
* This function fails to print a useful a string for out-of-range integers on
  some platforms:
  HP-UX 11.

Portability problems not fixed by Gnulib:

* POSIX requires that this function set the stream error bit (detected
  by `ferror`) on write failure, but not all platforms do this:
  glibc 2.13, cygwin 1.7.9.
* POSIX requires that this function not alter stream orientation, but
  the gnulib replacement locks in byte orientation and fails on wide
  character streams.

---

Next: [`printf`](#printf), Previous: [`perror`](#perror), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.40 `popen` [¶](#popen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/popen.html>

Gnulib module: popen

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 14.
* Some platforms start the child with closed stdin or stdout if the
  standard descriptors were closed in the parent:
  Cygwin 1.5.x.
* This function hangs if called with closed stdin or stdout on some platforms:
  OS/2 kLIBC.

Portability problems not fixed by Gnulib:

* On native Windows platforms, this functions terminates the current process
  with exit code 127 if the environment variable `COMSPEC` is not set.
* Some platforms mistakenly set the close-on-exec bit, then if it is
  cleared by the application, the platform then leaks file descriptors
  from earlier `popen` calls into subsequent `popen` children:
  Cygwin 1.5.x.

---

Next: [`putc`](#putc), Previous: [`popen`](#popen), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.41 `printf` [¶](#printf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/printf.html>

Gnulib module: printf-posix or printf-gnu or stdio-h, nonblocking, sigpipe

Portability problems fixed by either Gnulib module `stdio-h` or `fprintf-posix` or `fprintf-gnu`:

* This function does not support non-ASCII characters in double-byte encoding,
  corresponding to the locale, on some platforms:
  mingw with `__USE_MINGW_ANSI_STDIO` in combination with msvcrt,
  when the output goes to a Windows console.

Portability problems fixed by either Gnulib module `printf-posix` or `printf-gnu`:

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
* This function returns zero instead of a negative number after running
  out of memory:
  OpenBSD 7.8.

Portability problems fixed by Gnulib module `printf-gnu`:

* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.

Portability problems fixed by either Gnulib module `printf-posix` or `printf-gnu`
and additionally the Gnulib module `printf-with-n-directive`:

* This function does not support the ‘`n`’ directive on some platforms:
  glibc when used with `_FORTIFY_SOURCE >= 2` (set by default on Ubuntu),
  macOS 14, OpenBSD, MSVC 14, Android.

Portability problems fixed by Gnulib module `stdio-h` or `printf-posix` or `printf-gnu`, together with module `nonblocking`:

* When writing to a non-blocking pipe whose buffer is full, this function fails
  with `errno` being set to `ENOSPC` instead of `EAGAIN` on some
  platforms:
  mingw, MSVC 14.

Portability problems fixed by Gnulib module `stdio-h` or `printf-posix` or `printf-gnu`, together with module `sigpipe`:

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

Next: [`putc_unlocked`](#putc_005funlocked), Previous: [`printf`](#printf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.42 `putc` [¶](#putc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/putc.html>

Gnulib module: stdio-h, nonblocking, sigpipe

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When writing to a non-blocking pipe whose buffer is full, this function fails
  with `errno` being set to `ENOSPC` instead of `EAGAIN` on some
  platforms:
  mingw, MSVC 14.

Portability problems fixed by Gnulib module `stdio-h`, together with module `sigpipe`:

* When writing to a pipe with no readers, this function fails, instead of
  obeying the current `SIGPIPE` handler, on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* This function fails and produces garbled output
  when invoked twice, for outputting a non-ASCII character in double-byte encoding,
  corresponding to the locale, on some platforms:
  mingw in combination with msvcrt,
  when the output goes to a Windows console.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Next: [`putchar`](#putchar), Previous: [`putc`](#putc), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.43 `putc_unlocked` [¶](#putc_005funlocked-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/putc_unlocked.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Next: [`putchar_unlocked`](#putchar_005funlocked), Previous: [`putc_unlocked`](#putc_005funlocked), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.44 `putchar` [¶](#putchar-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/putchar.html>

Gnulib module: stdio-h, nonblocking, sigpipe

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When writing to a non-blocking pipe whose buffer is full, this function fails
  with `errno` being set to `ENOSPC` instead of `EAGAIN` on some
  platforms:
  mingw, MSVC 14.

Portability problems fixed by Gnulib module `stdio-h`, together with module `sigpipe`:

* When writing to a pipe with no readers, this function fails, instead of
  obeying the current `SIGPIPE` handler, on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Next: [`puts`](#puts), Previous: [`putchar`](#putchar), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.45 `putchar_unlocked` [¶](#putchar_005funlocked-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/putchar_unlocked.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline
  functions on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Next: [`remove`](#remove), Previous: [`putchar_unlocked`](#putchar_005funlocked), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.46 `puts` [¶](#puts-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/puts.html>

Gnulib module: stdio-h, nonblocking, sigpipe

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When writing to a non-blocking pipe whose buffer is full, this function fails
  with `errno` being set to `ENOSPC` instead of `EAGAIN` on some
  platforms:
  mingw, MSVC 14.

Portability problems fixed by Gnulib module `stdio-h`, together with module `sigpipe`:

* When writing to a pipe with no readers, this function fails, instead of
  obeying the current `SIGPIPE` handler, on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* On some platforms, this function does not set `errno` or the
  stream error indicator on attempts to write to a read-only stream:
  Cygwin 1.7.9.

---

Next: [`rename`](#rename), Previous: [`puts`](#puts), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.47 `remove` [¶](#remove-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/remove.html>

Gnulib module: remove

Portability problems fixed by Gnulib:

* This function fails to reject trailing slashes on non-directories on
  some platforms:
  FreeBSD 7.2, Solaris 9.
* This function mistakenly removes a directory with
  `remove("dir/./")` on some platforms:
  Cygwin 1.5.x.
* This function does not remove empty directories on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`renameat`](#renameat), Previous: [`remove`](#remove), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.48 `rename` [¶](#rename-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/rename.html>

Gnulib module: rename

Portability problems fixed by Gnulib:

* This function does not allow trailing slashes when creating a
  destination directory, as in `rename("dir","new/")`:
  NetBSD 1.6.
* This function does not reject trailing slashes on the destination for
  non-directories on some platforms, as in `rename("file","new/")`:
  AIX 7.1, Solaris 11.3, Cygwin 1.5.x, mingw, MSVC 14.
* This function does not reject trailing slashes on symlinks to
  non-directories on some platforms, as in
  `rename("link-to-file/","f")`:
  FreeBSD 7.2.
* This function ignores trailing slashes on symlinks on some platforms,
  such that `rename("link/","new")` corrupts `link`:
  Solaris 9.
* This function incorrectly reduces the link count when comparing two
  spellings of a hard link on some platforms:
  NetBSD 1.6, Cygwin 1.5.x.
* This function will not always replace an existing destination on some
  platforms:
  Cygwin 1.5.x, mingw, MSVC 14.
  However, the replacement is not atomic for directories, and may end up
  losing the empty destination if the source could not be renamed.
* This function mistakenly allows names ending in ‘`.`’ or ‘`..`’
  on some platforms:
  Cygwin 1.5.x, mingw, MSVC 14.
* This function does not reject attempts to rename existing directories
  and non-directories onto one another on some platforms:
  Cygwin 1.5.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* POSIX requires that `rename("symlink-to-dir/","dir2")` rename
  `dir` and leave `symlink-to-dir` dangling; likewise, it
  requires that `rename("dir","dangling/")` rename `dir` so
  that `dangling` is no longer a dangling symlink. This behavior
  is counter-intuitive, so on some systems, `rename` fails with
  `ENOTDIR` if either argument is a symlink with a trailing slash:
  glibc, OpenBSD, Cygwin 1.7.
* POSIX requires that `rename` do nothing and return 0 if the
  source and destination are hard links to the same file. This behavior
  is counterintuitive, and on some systems `renameat` is a no-op in
  this way only if the source and destination identify the same
  directory entry. On these systems, for example, although renaming
  `./f` to `f` is a no-op, renaming `f` to `g`
  deletes `f` when `f` and `g` are hard links to the same
  file:
  NetBSD 7.0.
* After renaming a non-empty directory over an existing empty directory,
  the old directory name is still visible through the `stat` function
  for 30 seconds after the rename, on NFS file systems, on some platforms:
  Linux 2.6.18.
* This function will not rename a source that is currently opened
  by any process:
  mingw, MSVC 14.

---

Next: [`rewind`](#rewind), Previous: [`rename`](#rename), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.49 `renameat` [¶](#renameat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/renameat.html>

Gnulib module: renameat

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 1.5.x, mingw, MSVC 14.
  But the replacement function is not safe to be used in libraries and is not
  thread-safe.
* This function is declared in `<unistd.h>`, not in `<stdio.h>`,
  on some platforms:
  NetBSD 7.0, Solaris 11.4.
* This function is declared in `<sys/stat.h>`, not in `<stdio.h>`,
  on some platforms:
  Android 4.3.
* This function does not reject trailing slashes on non-directories on
  some platforms, as in `renameat(fd,"file",fd,"new/")`:
  Solaris 11.4.
* This function ignores trailing slashes on symlinks on some platforms,
  such that `renameat(fd,"link/",fd,"new")` corrupts `link`:
  Solaris 9.
* `renameat (AT_FDCWD, "file", AT_FDCWD, "new/")` succeeds instead of
  failing and setting `errno` to `ENOTDIR`, on some platforms:
  GNU/Hurd with glibc 2.41.
* `renameat (ATFDCWD, "dir", AT_FDCWD, "new/")` fails with
  `errno` set to `ENOENT`, on some platforms:
  GNU/Hurd with glibc 2.42.
* `renameat (ATFDCWD, "dir/", AT_FDCWD, "new")` fails with
  `errno` set to `ENOTDIR`, on some platforms:
  GNU/Hurd with glibc 2.42.

Portability problems not fixed by Gnulib:

* POSIX requires that `renameat(fd,"symlink-to-dir/",fd,"dir2")` rename
  `dir` and leave `symlink-to-dir` dangling; likewise, it
  requires that `renameat(fd,"dir",fd,"dangling/")` rename `dir` so
  that `dangling` is no longer a dangling symlink. This behavior
  is counter-intuitive, so on some systems, `renameat` fails with
  `ENOTDIR` if either argument is a symlink with a trailing slash:
  glibc, OpenBSD, Cygwin 1.7.
* POSIX requires that `renameat` do nothing and return 0 if the
  source and destination are hard links to the same file. This behavior
  is counterintuitive, and on some systems `renameat` is a no-op in
  this way only if the source and destination identify the same
  directory entry. On these systems, for example, although renaming
  `./f` to `f` is a no-op, renaming `f` to `g`
  deletes `f` when `f` and `g` are hard links to the same
  file:
  NetBSD 7.0.
* After renaming a non-empty directory over an existing empty directory,
  the old directory name is still visible through the `stat` function
  for 30 seconds after the rename, on NFS file systems, on some platforms:
  Linux 2.6.18.
* This function will not rename a source that is currently opened
  by any process:
  mingw, MSVC 14.

---

Next: [`scanf`](#scanf), Previous: [`renameat`](#renameat), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.50 `rewind` [¶](#rewind-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/rewind.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.

---

Next: [`setbuf`](#setbuf), Previous: [`rewind`](#rewind), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.51 `scanf` [¶](#scanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/scanf.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-scanf.html>

Gnulib module: stdio-h, nonblocking

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When reading from a non-blocking pipe whose buffer is empty, this function
  fails with `errno` being set to `EINVAL` instead of `EAGAIN` on
  some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* C99 and POSIX.1-2001 and later require end-of-file to be sticky, that
  is, they require this function to act as if it reads end-of-file if
  `feof` would return nonzero. However, on some systems this
  function attempts to read from the underlying file descriptor even if
  the stream’s end-of-file indicator is set. These systems include
  glibc and default Solaris.
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

Next: [`setvbuf`](#setvbuf), Previous: [`scanf`](#scanf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.52 `setbuf` [¶](#setbuf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setbuf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`snprintf`](#snprintf), Previous: [`setbuf`](#setbuf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.53 `setvbuf` [¶](#setvbuf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setvbuf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not set `errno`
  upon failure.
* On Windows platforms (excluding Cygwin), `_IOLBF` (line-buffering)
  is treated as if it were `_IOFBF` (full buffering),
  possibly with a warning generated.

---

Next: [`sprintf`](#sprintf), Previous: [`setvbuf`](#setvbuf), Up: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.45.54 `snprintf` [¶](#snprintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/snprintf.html>

Gnulib module: snprintf or snprintf-posix or snprintf-gnu

Portability problems fixed by either Gnulib module `snprintf` or `snprintf-posix` or `snprintf-gnu`:

* This function is missing on some platforms:
  MSVC 14.
* This function does not support format directives that access arguments in an
  arbitrary order, such as `"%2$s"`, on some platforms:
  NetBSD 3.0, mingw.
* This function does not return a byte count as specified in C99 on some
  platforms:
  HP-UX 11, Solaris 9, mingw.

Portability problems fixed by either Gnulib module `snprintf-posix` or `snprintf-gnu`:
