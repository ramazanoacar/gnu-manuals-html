### 10.46 Functions in `<stdlib.h>` [¶](#Functions-in-_003cstdlib_002eh_003e-1)

* [`_Exit`](#g_t_005fExit)
* [`a64l`](#a64l)
* [`abort`](#abort)
* [`abs`](#abs)
* [`aligned_alloc`](#aligned_005falloc)
* [`at_quick_exit`](#at_005fquick_005fexit)
* [`atexit`](#atexit)
* [`atof`](#atof)
* [`atoi`](#atoi)
* [`atol`](#atol)
* [`atoll`](#atoll)
* [`bsearch`](#bsearch)
* [`calloc`](#calloc)
* [`div`](#div)
* [`drand48`](#drand48)
* [`erand48`](#erand48)
* [`exit`](#exit)
* [`free`](#free)
* [`free_aligned_sized`](#free_005faligned_005fsized)
* [`free_sized`](#free_005fsized)
* [`getenv`](#getenv)
* [`getsubopt`](#getsubopt)
* [`grantpt`](#grantpt)
* [`initstate`](#initstate)
* [`jrand48`](#jrand48)
* [`l64a`](#l64a)
* [`labs`](#labs)
* [`lcong48`](#lcong48)
* [`ldiv`](#ldiv)
* [`llabs`](#llabs)
* [`lldiv`](#lldiv)
* [`lrand48`](#lrand48)
* [`malloc`](#malloc)
* [`mblen`](#mblen)
* [`mbstowcs`](#mbstowcs)
* [`mbtowc`](#mbtowc)
* [`memalignment`](#memalignment)
* [`mkdtemp`](#mkdtemp)
* [`mkostemp`](#mkostemp)
* [`mkstemp`](#mkstemp)
* [`mrand48`](#mrand48)
* [`nrand48`](#nrand48)
* [`posix_memalign`](#posix_005fmemalign)
* [`posix_openpt`](#posix_005fopenpt)
* [`ptsname`](#ptsname)
* [`ptsname_r`](#ptsname_005fr)
* [`putenv`](#putenv)
* [`qsort`](#qsort)
* [`qsort_r`](#qsort_005fr)
* [`quick_exit`](#quick_005fexit)
* [`rand`](#rand)
* [`rand_r`](#rand_005fr)
* [`random`](#random)
* [`realloc`](#realloc)
* [`reallocarray`](#reallocarray)
* [`realpath`](#realpath)
* [`secure_getenv`](#secure_005fgetenv)
* [`seed48`](#seed48)
* [`setenv`](#setenv)
* [`setkey`](#setkey)
* [`setstate`](#setstate)
* [`srand`](#srand)
* [`srand48`](#srand48)
* [`srandom`](#srandom)
* [`strfromd`](#strfromd)
* [`strfromf`](#strfromf)
* [`strfroml`](#strfroml)
* [`strtod`](#strtod)
* [`strtof`](#strtof)
* [`strtol`](#strtol)
* [`strtold`](#strtold)
* [`strtoll`](#strtoll)
* [`strtoul`](#strtoul)
* [`strtoull`](#strtoull)
* [`system`](#system)
* [`unlockpt`](#unlockpt)
* [`unsetenv`](#unsetenv)
* [`wcstombs`](#wcstombs)
* [`wctomb`](#wctomb)

---

Next: [`a64l`](#a64l), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.1 `_Exit` [¶](#g_t_005fExit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/_Exit.html>

Gnulib module: \_Exit

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.5.x, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`abort`](#abort), Previous: [`_Exit`](#g_t_005fExit), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.2 `a64l` [¶](#a64l-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/a64l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, Minix 3.1.8, mingw, MSVC 14, Android 9.0.
* This function was not correctly implemented in glibc versions before 2.2.5.

---

Next: [`abs`](#abs), Previous: [`a64l`](#a64l), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.3 `abort` [¶](#abort-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/abort.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* Some platforms mistakenly close all stdio streams prior to raising
  SIGABRT:
  Cygwin 1.5.x.
* Some platforms always print a message to stderr, even if a SIGABRT handler
  uses longjmp to resume execution at a safe point:
  mingw, MSVC 14.

---

Next: [`aligned_alloc`](#aligned_005falloc), Previous: [`abort`](#abort), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.4 `abs` [¶](#abs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/abs.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Android 4.3.

---

Next: [`at_quick_exit`](#at_005fquick_005fexit), Previous: [`abs`](#abs), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.5 `aligned_alloc` [¶](#aligned_005falloc-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.24.3.1.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/aligned_alloc.html>

Documentation:  
 [man aligned\_alloc](https://www.kernel.org/doc/man-pages/online/pages/man3/aligned_alloc.3.html)

Gnulib module: aligned\_alloc

Portability problems fixed by Gnulib:

* This function fails if the alignment argument is smaller than
  `sizeof (void *)` on some platforms:
  macOS 14, AIX 7.3.1.
* This function returns a null pointer if the requested size is
  not a multiple of the alignment:
  Solaris 11.4.
* This function returns a null pointer if the size argument is zero:
  macOS 15, AIX 7.3, Solaris 11.4.
* On some platforms, `aligned_alloc` crashes if the requested size is
  not a multiple of the alignment:
  AddressSanitizer (gcc 11.2 or clang 13).

Portability problems not fixed by Gnulib:

* This function is missing on many older platforms:
  glibc 2.15, macOS 10.14, FreeBSD 6.4, NetBSD 7.1, OpenBSD 6.0, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.
* If the alignment and size are absurdly large, this function crashes:
  glibc 2.42.

Gnulib has partial substitutes for `aligned_alloc`
that do not crash even if the AddressSanitizer bug is present:

* The Gnulib module `alignalloc` provides a portable function
  `alignalloc` that is a near-substitute for glibc
  `aligned_alloc`, except that the result must be freed
  with `alignfree` rather than plain `free`.
* The Gnulib module `aligned-malloc` provides functions for
  allocating and freeing blocks of suitably aligned memory.
* The Gnulib module `pagealign_alloc` provides a similar API for
  allocating and freeing blocks of memory aligned on a system page boundary.

---

Next: [`atexit`](#atexit), Previous: [`aligned_alloc`](#aligned_005falloc), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.6 `at_quick_exit` [¶](#at_005fquick_005fexit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.24.4.3.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/at_quick_exit.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.9, macOS 14, FreeBSD 6.4, NetBSD 5.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.3.

---

Next: [`atof`](#atof), Previous: [`at_quick_exit`](#at_005fquick_005fexit), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.7 `atexit` [¶](#atexit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atexit.html>

Gnulib module: atexit

Portability problems fixed by Gnulib:

* This function is missing on some old platforms.

Portability problems not fixed by Gnulib:

* The exit status can be incorrect for a process terminated by a signal
  during a call to a function registered by `atexit`. This is
  because if a signal terminates a process after `exit` is called
  or `main` returns, the process’s exit status is that specified by
  `exit`’s argument or by `main`’s return value, instead of
  being the exit status corresponding to the signal:
  z/OS 3.1.

---

Next: [`atoi`](#atoi), Previous: [`atexit`](#atexit), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.8 `atof` [¶](#atof-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atof.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Android 4.4.
* This function mis-parses strings with leading ‘`+`’ on some old platforms:
  Old versions of Linux.
* This function returns a positive value for negative underflow on some
  platforms:
  glibc 2.4, Mingw, Cygwin.
* This function fails to do a valid parse of ‘`-0x`’ on some
  platforms:
  glibc 2.4, Cygwin < 1.5.25-11.
* This function fails to parse Infinities and plain NaNs on some platforms:
  Mingw, OpenBSD 4.0.
* This function fails to parse NaN() on some platforms:
  Mingw, OpenBSD 4.0, Cygwin < 1.5.25-11.
* This function fails to parse NaN(n-char-sequence) on some platforms:
  Mingw, OpenBSD 4.0.
* This function fails to parse C99 hexadecimal floating point on some
  platforms:
  Mingw, OpenBSD 4.0.
* This function fails to correctly parse very long strings on some
  platforms:
  Mingw, Cygwin.

---

Next: [`atol`](#atol), Previous: [`atof`](#atof), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.9 `atoi` [¶](#atoi-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atoi.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`atoll`](#atoll), Previous: [`atoi`](#atoi), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.10 `atol` [¶](#atol-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atol.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`bsearch`](#bsearch), Previous: [`atol`](#atol), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.11 `atoll` [¶](#atoll-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atoll.html>

Gnulib module: atoll

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11.23, MSVC 9.

Portability problems not fixed by Gnulib:

---

Next: [`calloc`](#calloc), Previous: [`atoll`](#atoll), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.12 `bsearch` [¶](#bsearch-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/bsearch.html>

Gnulib module: bsearch

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`div`](#div), Previous: [`bsearch`](#bsearch), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.13 `calloc` [¶](#calloc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/calloc.html>

Gnulib module: calloc-posix

Portability problems fixed by Gnulib:

* On some platforms, when the function fails it does not set `errno`:
  mingw, MSVC 14.
* On some platforms, when the function fails it might set `errno`
  to `EAGAIN` instead of to `ENOMEM`. Although POSIX allows
  `EAGAIN`, the `calloc-posix` module insists on `ENOMEM`
  which also conforms to POSIX and is GNU-compatible:
  Solaris 11.4.
* On some platforms, `calloc (n, s)` can succeed even if
  multiplying `n` by `s` would exceed `PTRDIFF_MAX` or
  `SIZE_MAX`. Although failing to check for exceeding
  `PTRDIFF_MAX` is arguably allowed by POSIX it can lead to
  undefined behavior later, so `calloc-posix` does not allow
  going over the limit.

Extension: Gnulib provides a module ‘`calloc-gnu`’ that substitutes a
`calloc` implementation that behaves more like the glibc implementation.
It fixes this portability problem:

* `calloc (0, s)` and `calloc (n, 0)` return `NULL` on success
  on some platforms:
  AIX 7.3.

---

Next: [`drand48`](#drand48), Previous: [`calloc`](#calloc), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.14 `div` [¶](#div-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/div.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`erand48`](#erand48), Previous: [`div`](#div), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.15 `drand48` [¶](#drand48-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/drand48.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`exit`](#exit), Previous: [`drand48`](#drand48), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.16 `erand48` [¶](#erand48-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/erand48.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`free`](#free), Previous: [`erand48`](#erand48), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.17 `exit` [¶](#exit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/exit.html>

Gnulib module: stdlib-h

Portability problems fixed by Gnulib:

* Some problems with the macros `EXIT_SUCCESS` and `EXIT_FAILURE`,
  see [`stdlib.h`](#stdlib_002eh).

Portability problems not fixed by Gnulib:

---

Next: [`free_aligned_sized`](#free_005faligned_005fsized), Previous: [`exit`](#exit), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.18 `free` [¶](#free-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/free.html>

Gnulib module: free-posix

Portability problems fixed by Gnulib:

* This function may clobber `errno` on many platforms:
  glibc 2.32, Mac OS X, FreeBSD, NetBSD, OpenBSD 4.4, Minix, AIX, HP-UX, Cygwin, mingw, MSVC.

Portability problems not fixed by Gnulib:

---

Next: [`free_sized`](#free_005fsized), Previous: [`free`](#free), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.19 `free_aligned_sized` [¶](#free_005faligned_005fsized-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.24.3.5

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most non-C23 platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

---

Next: [`getenv`](#getenv), Previous: [`free_aligned_sized`](#free_005faligned_005fsized), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.20 `free_sized` [¶](#free_005fsized-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.24.3.4

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most non-C23 platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

---

Next: [`getsubopt`](#getsubopt), Previous: [`free_sized`](#free_005fsized), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.21 `getenv` [¶](#getenv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getenv.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`grantpt`](#grantpt), Previous: [`getenv`](#getenv), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.22 `getsubopt` [¶](#getsubopt-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getsubopt.html>

Gnulib module: getsubopt

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 7.1.
* This function is declared in `unistd.h` instead of
  `stdlib.h` on some platforms:
  Cygwin 1.7.1.

Portability problems not fixed by Gnulib:

---

Next: [`initstate`](#initstate), Previous: [`getsubopt`](#getsubopt), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.23 `grantpt` [¶](#grantpt-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/grantpt.html>

Gnulib module: grantpt

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

* This function reports success for invalid file descriptors on some platforms:
  OpenBSD, Cygwin 1.7.9, musl libc.

---

Next: [`jrand48`](#jrand48), Previous: [`grantpt`](#grantpt), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.24 `initstate` [¶](#initstate-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/initstate.html>

Gnulib module: random

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.
* This function is not declared on some platforms:
  Cygwin 1.5.25.

Portability problems not fixed by Gnulib:

* This function crashes if the state buffer is unaligned on some platforms:
  glibc 2.40/sparc.
* The first parameter is `unsigned long` instead of `unsigned int` on
  some platforms:
  MidnightBSD 2.0.
* The third parameter is `long` instead of `size_t` on some platforms:
  MidnightBSD 2.0.
* This function makes out-of-bounds writes on some platforms:
  Haiku.

---

Next: [`l64a`](#l64a), Previous: [`initstate`](#initstate), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.25 `jrand48` [¶](#jrand48-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/jrand48.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`labs`](#labs), Previous: [`jrand48`](#jrand48), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.26 `l64a` [¶](#l64a-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/l64a.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, Minix 3.1.8, mingw, MSVC 14, Android 9.0.
* This function was not correctly implemented in glibc versions before 2.2.5.

---

Next: [`lcong48`](#lcong48), Previous: [`l64a`](#l64a), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.27 `labs` [¶](#labs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/labs.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Android 4.3.

---

Next: [`ldiv`](#ldiv), Previous: [`labs`](#labs), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.28 `lcong48` [¶](#lcong48-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lcong48.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 5.1.

---

Next: [`llabs`](#llabs), Previous: [`lcong48`](#lcong48), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.29 `ldiv` [¶](#ldiv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ldiv.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`lldiv`](#lldiv), Previous: [`ldiv`](#ldiv), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.30 `llabs` [¶](#llabs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/llabs.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.23, MSVC 9, Android 4.3.

---

Next: [`lrand48`](#lrand48), Previous: [`llabs`](#llabs), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.31 `lldiv` [¶](#lldiv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lldiv.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, HP-UX 11.23, MSVC 9.

---

Next: [`malloc`](#malloc), Previous: [`lldiv`](#lldiv), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.32 `lrand48` [¶](#lrand48-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lrand48.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`mblen`](#mblen), Previous: [`lrand48`](#lrand48), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.33 `malloc` [¶](#malloc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/malloc.html>

Gnulib module: malloc-posix

Portability problems fixed by Gnulib:

* On some platforms, when the function fails it does not set `errno`:
  mingw, MSVC 14.
* On some platforms, when the function fails it might set `errno`
  to `EAGAIN` instead of to `ENOMEM`. Although POSIX allows
  `EAGAIN`, the `malloc-posix` module insists on `ENOMEM`
  which also conforms to POSIX and is GNU-compatible:
  Solaris 11.4.
* On some platforms, `malloc (n)` can succeed even if `n`
  exceeds `PTRDIFF_MAX`. Although this behavior is arguably
  allowed by POSIX it can lead to behavior not defined by POSIX later,
  so `malloc-posix` does not allow going over the limit.

Extension: Gnulib provides a module ‘`malloc-gnu`’ that substitutes a
`malloc` implementation that behaves more like the glibc implementation,
by fixing this portability problem:

* `malloc (0)` returns `NULL` on success on some platforms:
  AIX 7.3.

---

Next: [`mbstowcs`](#mbstowcs), Previous: [`malloc`](#malloc), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.34 `mblen` [¶](#mblen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mblen.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Android 7.1.

---

Next: [`mbtowc`](#mbtowc), Previous: [`mblen`](#mblen), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.35 `mbstowcs` [¶](#mbstowcs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mbstowcs.html>

Gnulib module: mbstowcs

Portability problems fixed by Gnulib:

* In the C or POSIX locales, this function can return `(size_t) -1`
  and set `errno` to `EILSEQ`:
  glibc 2.35.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `mbstoc32s`, provided by Gnulib module
  `mbstoc32s`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Next: [`memalignment`](#memalignment), Previous: [`mbstowcs`](#mbstowcs), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.36 `mbtowc` [¶](#mbtowc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mbtowc.html>

Gnulib module: mbtowc

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Android 4.4.

Portability problems not fixed by Gnulib:

* In UTF-8 locales, this function may return wide characters up to 0x7FFFFFFF
  (that is, beyond 0x0010FFFF) on some platforms:
  glibc 2.34.
* This function accumulates hidden state on some platforms:
  glibc 2.8 (see <https://sourceware.org/PR9674>).
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`mkdtemp`](#mkdtemp), Previous: [`mbtowc`](#mbtowc), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.37 `memalignment` [¶](#memalignment-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.24.9.1

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most non-C23 platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

To check whether a pointer P has alignment A,
Gnulib code typically tests `(uintptr_t) P % A`
instead of `memalignment (P) % A`.
See [Other portability assumptions made by Gnulib](#Other-portability-assumptions).

---

Next: [`mkostemp`](#mkostemp), Previous: [`memalignment`](#memalignment), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.38 `mkdtemp` [¶](#mkdtemp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mkdtemp.html>

Gnulib module: mkdtemp

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 10, mingw, MSVC 14.
* This function is declared in `<unistd.h>` instead of `<stdlib.h>`
  on some platforms:
  macOS 26.

Portability problems not fixed by Gnulib:

---

Next: [`mkstemp`](#mkstemp), Previous: [`mkdtemp`](#mkdtemp), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.39 `mkostemp` [¶](#mkostemp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mkostemp.html>

Documentation:  
 [man mkostemp](https://www.kernel.org/doc/man-pages/online/pages/man3/mkostemp.3.html)

Gnulib module: mkostemp

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.6, Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.5, mingw, MSVC 14, Android 5.1.
* This function is declared in `<unistd.h>` instead of `<stdlib.h>`
  on some platforms:
  macOS 14.
* On platforms where `off_t` is a 32-bit type, `mkostemp` may not work
  correctly to create files larger than 2 GB. (Cf. `AC_SYS_LARGEFILE`.)

Portability problems not fixed by Gnulib:

The gnulib module `clean-temp` can create temporary files that will not
be left behind after signals such as SIGINT.

---

Next: [`mrand48`](#mrand48), Previous: [`mkostemp`](#mkostemp), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.40 `mkstemp` [¶](#mkstemp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mkstemp.html>

Gnulib module: mkstemp

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* On platforms where `off_t` is a 32-bit type, `mkstemp` may not work
  correctly to create files 2 GiB and larger. See [Large File Support](#Large-File-Support).
* On some older platforms, `mkstemp` can create a world or group
  writable or readable file, if you haven’t set the process umask to
  077. This is a security risk.

Portability problems not fixed by Gnulib:

The gnulib module `clean-temp` can create temporary files that will not
be left behind after signals such as SIGINT.

---

Next: [`nrand48`](#nrand48), Previous: [`mkstemp`](#mkstemp), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.41 `mrand48` [¶](#mrand48-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mrand48.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`posix_memalign`](#posix_005fmemalign), Previous: [`mrand48`](#mrand48), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.42 `nrand48` [¶](#nrand48-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nrand48.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`posix_openpt`](#posix_005fopenpt), Previous: [`nrand48`](#nrand48), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.43 `posix_memalign` [¶](#posix_005fmemalign-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_memalign.html>

Gnulib module: posix\_memalign

Although this function is superseded by `aligned_alloc`,
it is more portable to older systems that do not support C11.
See [`aligned_alloc`](#aligned_005falloc).

Portability problems fixed by Gnulib:

* This function returns a null pointer if the size argument is zero:
  AIX 7.3.
* This function produces misaligned results on some platforms:
  OpenBSD 6.1.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 4.1.
* If the alignment and size are absurdly large, this function crashes:
  glibc 2.42.

The Gnulib module `aligned-malloc` provides functions for
allocating and freeing blocks of suitably aligned memory.

The Gnulib module `pagealign_alloc` provides a similar API for
allocating and freeing blocks of memory aligned on a system page boundary.

---

Next: [`ptsname`](#ptsname), Previous: [`posix_memalign`](#posix_005fmemalign), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.44 `posix_openpt` [¶](#posix_005fopenpt-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_openpt.html>

Gnulib module: posix\_openpt

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11.23, Solaris 9, Cygwin 1.5.x, mingw, MSVC 14, Android 4.4.
  However, the replacement may fail with `ENOSYS` or `ENOENT` on
  some platforms.

Portability problems not fixed by Gnulib:

Note that when using this function to open the master side of a
pseudo-terminal, you still need platform dependent code to open the
corresponding slave side.
The Gnulib module `openpty` provides
an easy-to-use API that does both at once.

---

Next: [`ptsname_r`](#ptsname_005fr), Previous: [`posix_openpt`](#posix_005fopenpt), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.45 `ptsname` [¶](#ptsname-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ptsname.html>

Gnulib module: ptsname

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, mingw, MSVC 14.
* This function fails to set errno on failure on some platforms:
  FreeBSD 8.2.

Portability problems not fixed by Gnulib:

* On Solaris 11 2010-11, this function fails on all BSD-style `/dev/pty*`
  device files.
* This function is not thread-safe on some platforms:
  Cygwin 1.7.9.
  Likewise, the gnulib replacement is not thread-safe.

Note that the Gnulib module `ptsname_r` is a version of this
function that is more likely to be thread-safe.

---

Next: [`putenv`](#putenv), Previous: [`ptsname`](#ptsname), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.46 `ptsname_r` [¶](#ptsname_005fr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ptsname_r.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Allocation.html>,
* [man ptsname\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/ptsname_r.3.html).

Gnulib module: ptsname\_r

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.12, FreeBSD 12.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8,
  AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.9, mingw,
  MSVC 14.
* When this function fails, it returns -1 instead of the error code
  on some platforms:
  macOS 14, FreeBSD 14.0, Android 4.3.
* When this function fails with `ERANGE`, it stores an empty string as result
  on some platforms:
  NetBSD 10.0, Solaris 11 OmniOS.

Portability problems not fixed by Gnulib:

* When this function fails, it returns 0 and stores an empty string as result
  on some platforms:
  Cygwin 2.9.

Note: Portable programs should expect to find the error code as the
return value of this function, not as the value of `errno`.
This is needed for compatibility with musl libc and with the
forthcoming POSIX Issue 8.

---

Next: [`qsort`](#qsort), Previous: [`ptsname_r`](#ptsname_005fr), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.47 `putenv` [¶](#putenv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/putenv.html>,
amended through <https://www.austingroupbugs.net/view.php?id=1598>.

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Environment-Access.html>.

Note: POSIX `putenv` supports
adding or changing the value of an environment variable,
while glibc also supports removing an environment variable
(as if by `unsetenv`).

Gnulib module: putenv-gnu

Portability problems fixed by Gnulib:

* This function does not support removing an environment variable
  on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, mingw, MSVC 14, Android 11.0.

Portability problems not fixed by Gnulib:

---

Next: [`qsort_r`](#qsort_005fr), Previous: [`putenv`](#putenv), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.48 `qsort` [¶](#qsort-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/qsort.html>

Gnulib module: qsort

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`quick_exit`](#quick_005fexit), Previous: [`qsort`](#qsort), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.49 `qsort_r` [¶](#qsort_005fr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/qsort_r.html>

Documentation:  
 [man qsort\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/qsort_r.3.html)

Gnulib module: qsort\_r

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.7, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android API level 35.
* This function has an incompatible API on some platforms:
  FreeBSD 13.4.

Portability problems not fixed by Gnulib:

---

Next: [`rand`](#rand), Previous: [`qsort_r`](#qsort_005fr), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.50 `quick_exit` [¶](#quick_005fexit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.24.4.7.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/quick_exit.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 6.4, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 9, Android 4.4.

---

Next: [`rand_r`](#rand_005fr), Previous: [`quick_exit`](#quick_005fexit), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.51 `rand` [¶](#rand-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/rand.html>

Gnulib module: rand

Portability problems fixed by Gnulib:

* This function crashes when used in multithreaded processes on some platforms:
  CheriBSD.

Portability problems not fixed by Gnulib:

* This function is only defined as an inline function on some platforms:
  Android 4.4.
* This function is not thread-safe on some platforms:
  musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, AIX 7.1, Solaris 11.4.

---

Next: [`random`](#random), Previous: [`rand`](#rand), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.52 `rand_r` [¶](#rand_005fr-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/rand_r.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.4.
* This function is removed in POSIX.1-2024.
  Use the function `random_r` from Gnulib module `random_r` instead.

---

Next: [`realloc`](#realloc), Previous: [`rand_r`](#rand_005fr), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.53 `random` [¶](#random-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/random.html>

Gnulib module: random

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function is only defined as an inline function on some platforms:
  Android 4.4.
* This function is not thread-safe on some platforms:
  macOS 14, FreeBSD 13.2, Solaris 11.4, Cygwin 3.4.6, Haiku.

Portability problems not fixed by Gnulib:

* This function has a slightly incompatible declaration (the return type being
  ‘`int`’ instead of ‘`long`’) on some platforms:
  Cygwin 1.5.25, Haiku.
* When ‘`srandom`’ was not called, this function returns a non-deterministic
  sequence rather than a deterministic sequence of numbers on some platforms:
  OpenBSD 7.4.

---

Next: [`reallocarray`](#reallocarray), Previous: [`random`](#random), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.54 `realloc` [¶](#realloc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/realloc.html>

Gnulib module: realloc-posix

Portability problems fixed by Gnulib:

* On some platforms, when the function fails it does not set `errno`:
  FreeBSD 15 (for a size > 0x7000000000000000), mingw, MSVC 14.
* On some platforms, when the function fails it might set `errno`
  to `EAGAIN` instead of to `ENOMEM`. Although POSIX allows
  `EAGAIN`, the `realloc-posix` module insists on `ENOMEM`
  which also conforms to POSIX and is GNU-compatible:
  Solaris 11.4.
* On some platforms, `realloc (p, n)` can succeed even if `n`
  exceeds `PTRDIFF_MAX`. Although this behavior is arguably
  allowed by POSIX it is not compatible with GNU and
  can lead to behavior not defined by POSIX later,
  so `realloc-posix` does not allow going over the limit.
* It is not portable to call `realloc` with a size of 0.

  With a null pointer argument, this is the same ambiguity as `malloc (0)`
  as to whether a successful call returns a null pointer or a pointer to a
  new zero-sized memory region. Although both behaviors conform to POSIX,
  `realloc-posix` insists on the latter behavior, which is
  compatible with GNU.

  With non-null `p`, behavior is a real mess for `realloc (p, 0)`.
  C23 says behavior is undefined.
  C89 through C17 say a successful call returns either a null pointer
  or a pointer to a new zero-sized memory region.
  POSIX.1-2017 extends C99 by saying that if a successful call
  returns a null pointer it
  must also set `errno` to an implementation-defined value,
  and POSIX.1-2024 extends C17 a bit further by saying that
  such a call must set `errno` to `EINVAL`.
  It is not known what POSIX.1-2024’s successor will say,
  though presumably it will extend C23.
  In practice, successful `realloc (p, 0)` calls
  have one of the following behaviors:

  1. Free `p`, do not change `errno`, and return a null pointer:
     glibc 2.33–2.41 by default, Android.
  2. Free `p`, possibly set `errno`, and return a null pointer:
     glibc 1–2.1 if specially built with `REALLOC_ZERO_BYTES_FREES=1`,
     glibc 2.1.1–2.32 by default,
     mingw, MSVC 14.
  3. Free `p`, set `errno` to `EINVAL`, and return a null pointer:
     AIX 7.3 without `_LINUX_SOURCE_COMPAT`.
  4. Free `p` and return a pointer to a new region of size zero:
     AIX 7.3 with `_LINUX_SOURCE_COMPAT`, glibc 1–2.1 by default,
     glibc 2.1.1–2.41 if specially built with `REALLOC_ZERO_BYTES_FREES=0`,
     musl libc, macOS, FreeBSD, NetBSD, OpenBSD, Solaris, Cygwin.

  Behaviors (3) and (4) conform to POSIX; behaviors (1) and (2) do not.
  The `realloc-posix` module insists on behavior (4) as it is more
  popular than (3) and is more useful in practice. Behavior (4) is the
  only one of the four in which returning a null pointer means failure,
  which is what non-expert programmers typically expect.

  A program not suspecting these variations in semantics will either:

  + Leak memory (the still-valid `p`) if it unwisely does not check
    for `realloc` failure, when it assumes behavior (1), (2) or (3) but the
    system implements behavior (4).
  + Falsely respond to memory exhaustion (if it wisely checks for `realloc`
    failure), or have double-free bugs (if it unwisely does not check),
    when it assumes behavior (4) but the system implements (1), (2) or (3).
* When `realloc (p, 0)` sets errno to `EINVAL` and returns null,
  a POSIX.1-2024 application cannot tell whether the call has succeeded
  and freed `p`, or has failed without freeing `p`.
  This is only a theoretical problem, though, as the only practical
  implementation with this behavior is AIX without `_LINUX_SOURCE_COMPAT`,
  which behaves this way only when the call succeeds.

Portability problems not fixed by Gnulib:

* When not growing an already-allocated region, i.e.,
  when `p` points to a region of size `psize` and `n <= psize`,
  `realloc (p, n)` can fail and return a null pointer:
  glibc 2.42 and probably other platforms.

---

Next: [`realpath`](#realpath), Previous: [`realloc`](#realloc), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.55 `reallocarray` [¶](#reallocarray-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/reallocarray.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Changing-Block-Size.html>,
* [man reallocarray](https://www.kernel.org/doc/man-pages/online/pages/man3/reallocarray.3.html).

Gnulib module: reallocarray

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.25, macOS 14, FreeBSD 10, NetBSD 7, OpenBSD 5.5, Minix 3.3.0, AIX 7.2, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.
* On some platforms, when the function fails it might set `errno`
  to `EAGAIN` instead of to `ENOMEM`. Although POSIX allows
  `EAGAIN`, the `reallocarray` module insists on `ENOMEM`
  which also conforms to POSIX and is GNU-compatible:
  Solaris 11.4.
* On some platforms, `reallocarray (p, n, s)` can succeed even if
  multiplying `n` by `s` would exceed `PTRDIFF_MAX`,
  which can lead to undefined behavior later:
  FreeBSD 13, NetBSD 9, OpenBSD 6, musl 1.2.
* It is not portable to call
  `reallocarray (p, n, s)` when either `n` or `s` is zero,
  as `reallocarray` has the same issues with zero sizes
  that `realloc` does. See [`realloc`](#realloc).

Portability problems not fixed by Gnulib:

* When not growing an already-allocated region,
  `reallocarray`, like `realloc`, can fail and return a null pointer:
  glibc 2.42 and probably other platforms.

---

Next: [`secure_getenv`](#secure_005fgetenv), Previous: [`reallocarray`](#reallocarray), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.56 `realpath` [¶](#realpath-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/realpath.html>

Gnulib module: canonicalize-lgpl

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function does not allow for a NULL ‘`resolved`’ parameter on
  some platforms:
  Mac OS X 10.5, FreeBSD 6.4, OpenBSD 4.4, Solaris 10.
* This function does not always return an absolute path on some
  platforms:
  Solaris 10.
* This function fails to detect trailing slashes on non-directories on
  some platforms:
  glibc 2.3.5, macOS 14, OpenBSD 6.0.
* This function fails to recognize non-directories or symlinks to non-directories
  followed by ‘`..`’ on some platforms:
  macOS 14, Cygwin 2.9.
* This function misbehaves on consecutive slashes on some platforms:
  musl libc 1.2.2, AIX 7.

Portability problems not fixed by Gnulib:

* This function does not allow to determine the required size of output buffer;
  the use of a non-NULL ‘`resolved`’ buffer is non-portable, since
  PATH\_MAX, if it is defined, is nothing more than a guess.

---

Next: [`seed48`](#seed48), Previous: [`realpath`](#realpath), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.57 `secure_getenv` [¶](#secure_005fgetenv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/secure_getenv.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Environment-Access.html>,
* [man secure\_getenv](https://www.kernel.org/doc/man-pages/online/pages/man3/secure_getenv.3.html).

Gnulib module: secure\_getenv

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.16, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 5.2, Minix 3.2.0, AIX 7.1, HP-UX 11, Solaris 11.3, Cygwin 3.0, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`setenv`](#setenv), Previous: [`secure_getenv`](#secure_005fgetenv), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.58 `seed48` [¶](#seed48-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/seed48.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`setkey`](#setkey), Previous: [`seed48`](#seed48), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.59 `setenv` [¶](#setenv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setenv.html>

Gnulib module: setenv

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.23, Solaris 9, mingw, MSVC 14.
* On some platforms, this function does not fail with `EINVAL` when
  passed an empty string or a string containing ‘`=`’:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 1.6, OpenBSD 7.9, Cygwin 1.5.x.
* On some platforms, this function removes a leading ‘`=`’ from the
  value argument:
  Cygwin 1.5.x.

Portability problems not fixed by Gnulib:

* Older versions of POSIX required that `setenv(NULL,"",0)` gracefully
  fail with `EINVAL`, but not all implementations guarantee this,
  and the requirement was removed.

---

Next: [`setstate`](#setstate), Previous: [`setenv`](#setenv), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.60 `setkey` [¶](#setkey-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setkey.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.34, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function is not declared in `<stdlib.h>`
  (without `-D_GNU_SOURCE`) on some platforms:
  glibc (at least 2.11–2.13).
* POSIX.1-2024 says this function is obsolescent and it is planned to be
  removed in a future version.

---

Next: [`srand`](#srand), Previous: [`setkey`](#setkey), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.61 `setstate` [¶](#setstate-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setstate.html>

Gnulib module: random

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.
* This function is not declared on some platforms:
  Cygwin 1.5.25.

Portability problems not fixed by Gnulib:

* This function has a slightly incompatible declaration (the argument type being
  ‘`const char *`’ instead of ‘`char *`’) on some platforms:
  macOS 14, AIX 7.1, HP-UX 11.31, Solaris 11.4, Solaris 11 OpenIndiana.

---

Next: [`srand48`](#srand48), Previous: [`setstate`](#setstate), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.62 `srand` [¶](#srand-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/srand.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is only defined as an inline function on some platforms:
  Android 4.4.
* This function has no effect on `rand` invocations in other threads
  on some platforms:
  Cygwin 3.4.6.

---

Next: [`srandom`](#srandom), Previous: [`srand`](#srand), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.63 `srand48` [¶](#srand48-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/srand48.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`strfromd`](#strfromd), Previous: [`srand48`](#srand48), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.64 `srandom` [¶](#srandom-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/srandom.html>

Gnulib module: random

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function is only defined as an inline function on some platforms:
  Android 4.4.

Portability problems not fixed by Gnulib:

* This function has a slightly incompatible declaration (the return type being
  ‘`long`’ instead of ‘`void`’) on some platforms:
  Cygwin 1.5.25.
* The parameter is `unsigned long` instead of `unsigned int` on some
  platforms:
  MidnightBSD 2.0.
* This function has no effect on some platforms:
  OpenBSD 7.4.
  This platform has, instead, a function `srandom_deterministic`.

---

Next: [`strfromf`](#strfromf), Previous: [`srandom`](#srandom), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.65 `strfromd` [¶](#strfromd-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Printing-of-Floats.html>,
* [man strfromd](https://www.kernel.org/doc/man-pages/online/pages/man3/strfromd.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`strfroml`](#strfroml), Previous: [`strfromd`](#strfromd), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.66 `strfromf` [¶](#strfromf-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Printing-of-Floats.html>,
* [man strfromf](https://www.kernel.org/doc/man-pages/online/pages/man3/strfromf.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`strtod`](#strtod), Previous: [`strfromf`](#strfromf), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.67 `strfroml` [¶](#strfroml-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Printing-of-Floats.html>,
* [man strfroml](https://www.kernel.org/doc/man-pages/online/pages/man3/strfroml.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`strtof`](#strtof), Previous: [`strfroml`](#strfroml), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.68 `strtod` [¶](#strtod-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtod.html>

Gnulib module: strtod or strtod-obsolete

Portability problems fixed by either Gnulib module `strtod` or `strtod-obsolete`:

* This function mis-parses strings with leading ‘`+`’ on some old platforms:
  Old versions of Linux.
* This function returns a wrong end pointer on some old platforms.
* This function allows whitespace between ‘`e`’ and the exponent on
  some platforms:
  HP-UX 11.11.
* This function returns the wrong end pointer for ‘`-0x`’ on some
  platforms:
  glibc 2.4, Mac OS X 10.5, FreeBSD 6.2, AIX 7.1, Cygwin < 1.5.25-11.
* This function fails to parse Infinities and plain NaNs on some platforms:
  OpenBSD 4.0, HP-UX 11.11, Solaris 9, mingw, MSVC 14.
* This function fails to parse ‘`NaN()`’ on some platforms:
  glibc 2.3.6, Mac OS X 10.5, FreeBSD 6.2, OpenBSD 4.0, AIX 7.1, HP-UX 11.11, Cygwin < 1.5.25-11, mingw, MSVC 14.
* This function fails to parse ‘`NaN(n-char-sequence)`’ on some
  platforms:
  OpenBSD 4.0, HP-UX 11.11, mingw, MSVC 14.
* This function parses ‘`NaN(n-char-sequence)`’, but returns
  the wrong end pointer on some platforms:
  glibc 2.4, AIX 7.1.
* This function misparses ‘`nan(`’ on some platforms:
  macOS 10.6.6.
* This function returns ‘`NaN`’ for ‘`-NaN`’ on some platforms:
  glibc 2.27, musl libc.
* This function fails to parse C99 hexadecimal floating point on some
  platforms:
  NetBSD 5.0, OpenBSD 4.0, AIX 5.1, HP-UX 11.11, Solaris 11.4, mingw, MSVC 14.
* In hexadecimal floats, this function allows whitespace between ‘`p`’
  and the exponent on some platforms:
  HP-UX 11.31/ia64.
* This function returns the wrong end pointer for ‘`0x1p`’ on some
  platforms:
  AIX 7.1.
* This function fails to set `errno` upon gradual underflow (resulting
  in a denormalized number) on some platforms:
  Cygwin 2.9.

Portability problems fixed by Gnulib module `strtod-obsolete`:

* This function is missing on some old platforms.

Portability problems not fixed by Gnulib:

* This function returns +0.0 (not −0.0) for negative underflow on some
  platforms:
  glibc 2.7, Cygwin 1.5.x, mingw, MSVC 14.
* This function cannot distinguish between “nan” and “-nan” on some
  platforms:
  glibc 2.7, mingw, MSVC 14.
* This function fails to correctly parse very long strings on some
  platforms:
  Mac OS X 10.5, FreeBSD 6.2, NetBSD 5.0, OpenBSD 4.0, Cygwin, mingw, MSVC 14.
* This function fails to set `errno` upon gradual underflow (resulting
  in a denormalized number) on some platforms:
  MSVC 14.
* The replacement function does not always return correctly rounded results.
* If no conversion could be performed, this function sets `errno`
  to `EINVAL` on some platforms. Portable programs should check
  for no conversion by comparing `str` to `*endptr`, not by
  inspecting `errno`.

---

Next: [`strtol`](#strtol), Previous: [`strtod`](#strtod), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.69 `strtof` [¶](#strtof-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtof.html>

Gnulib module: strtof

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 9, MSVC 9, Android 4.4.
* This function returns the wrong end pointer for ‘`-0x`’ on some
  platforms:
  glibc 2.4, Mac OS X 10.5, FreeBSD 6.2.
* This function fails to parse ‘`NaN()`’ on some platforms:
  glibc 2.5, FreeBSD 6.2.
* This function misparses ‘`nan(`’ on some platforms:
  macOS 10.6.6.
* This function fails to set `errno` upon overflow on some platforms:
  mingw 9.0.
* This function fails to set `errno` upon underflow on some platforms:
  Cygwin 2.9, mingw 5.0.

Portability problems not fixed by Gnulib:

* This function returns +0.0 (not −0.0) for negative underflow on some
  platforms:
  glibc 2.7, mingw, MSVC 14.
* This function cannot distinguish between “nan” and “-nan” on some
  platforms:
  glibc 2.7, mingw, MSVC 14.
* This function returns ‘`NaN`’ for ‘`-NaN`’ on some platforms:
  glibc 2.27, musl libc.
* This function fails to correctly parse very long strings on some
  platforms:
  Mac OS X 10.5, FreeBSD 6.2, NetBSD 5.0, Cygwin, mingw, MSVC 14.
* This function fails to set `errno` upon gradual underflow (resulting
  in a denormalized number) on some platforms:
  MSVC 14.
* The replacement function does not always return correctly rounded results.
* If no conversion could be performed, this function sets `errno`
  to `EINVAL` on some platforms. Portable programs should check
  for no conversion by comparing `str` to `*endptr`, not by
  inspecting `errno`.

---

Next: [`strtold`](#strtold), Previous: [`strtof`](#strtof), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.70 `strtol` [¶](#strtol-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtol.html>

Gnulib module: strtol

Portability problems fixed by Gnulib:

* This function is missing on some old platforms.
* This function does not parse the leading ‘`0`’ when the input string is
  `"0x"` and the base is 16 or 0 on some platforms:
  Minix 3.3, mingw, MSVC 14.
* This function does not parse binary integers (with a ‘`0b`’ or ‘`0B`’
  prefix) when the base is 2 or 0 on many platforms.

Portability problems not fixed by Gnulib:

* If no conversion could be performed, this function sets `errno`
  to `EINVAL` on some platforms. Portable programs should check
  for no conversion by comparing `str` to `*endptr`, not by
  inspecting `errno`.

---

Next: [`strtoll`](#strtoll), Previous: [`strtol`](#strtol), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.71 `strtold` [¶](#strtold-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtold.html>

Gnulib module: strtold

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, Solaris 9, older Cygwin 1.7.x, MSVC 9, Android 4.4.
* This function returns a struct, not a `long double`, on some platforms:
  HP-UX 11.31/hppa.
* This function always returns a wrong value on some platforms:
  mingw 5.0.
* This function allows whitespace between ‘`e`’ and the exponent on
  some platforms:
  HP-UX 11.31/ia64.
* This function returns the wrong end pointer for ‘`-0x`’ on some
  platforms:
  glibc-2.3.2, Mac OS X 10.5, Haiku.
* This function fails to parse Infinities and plain NaNs on some platforms:
  HP-UX 11.31/ia64.
* This function fails to parse ‘`NaN()`’ on some platforms:
  glibc-2.3.2, mingw, Haiku.
* This function parses ‘`NaN(n-char-sequence)`’, but returns
  the wrong end pointer on some platforms:
  glibc-2.3.2, mingw, Haiku.
* This function returns ‘`NaN`’ for ‘`-NaN`’ on some platforms:
  glibc 2.27, musl libc.
* This function fails to parse C99 hexadecimal floating point on some
  platforms:
  mingw.
* In hexadecimal floats, this function allows whitespace between ‘`p`’
  and the exponent on some platforms:
  HP-UX 11.31/ia64.
* This function fails to set `errno` upon underflow on some platforms:
  Cygwin 2.9.
* This function maps denormalized numbers to zero on some platforms:
  Haiku 2024.
* This function leaks memory on mingw 5.0
  and allocates an unbounded amount of stack on mingw 9.0.

Portability problems not fixed by Gnulib:

* This function fails to set `errno` upon gradual underflow (resulting
  in a denormalized number) on some platforms:
  MSVC 14.
* The replacement function does not always return correctly rounded results.
* If no conversion could be performed, this function sets `errno`
  to `EINVAL` on some platforms. Portable programs should check
  for no conversion by comparing `str` to `*endptr`, not by
  inspecting `errno`.

---

Next: [`strtoul`](#strtoul), Previous: [`strtold`](#strtold), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.72 `strtoll` [¶](#strtoll-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtoll.html>

Gnulib module: strtoll

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.11, MSVC 9.
* This function does not parse the leading ‘`0`’ when the input string is
  `"0x"` and the base is 16 or 0 on some platforms:
  Minix 3.3, mingw, MSVC 14.
* This function does not parse binary integers (with a ‘`0b`’ or ‘`0B`’
  prefix) when the base is 2 or 0 on many platforms.

Portability problems not fixed by Gnulib:

* If no conversion could be performed, this function sets `errno`
  to `EINVAL` on some platforms. Portable programs should check
  for no conversion by comparing `str` to `*endptr`, not by
  inspecting `errno`.

---

Next: [`strtoull`](#strtoull), Previous: [`strtoll`](#strtoll), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.73 `strtoul` [¶](#strtoul-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtoul.html>

Gnulib module: strtoul

Portability problems fixed by Gnulib:

* This function is missing on some old platforms.
* This function does not parse the leading ‘`0`’ when the input string is
  `"0x"` and the base is 16 or 0 on some platforms:
  Minix 3.3, mingw, MSVC 14.
* This function does not parse binary integers (with a ‘`0b`’ or ‘`0B`’
  prefix) when the base is 2 or 0 on many platforms.

Portability problems not fixed by Gnulib:

* If no conversion could be performed, this function sets `errno`
  to `EINVAL` on some platforms. Portable programs should check
  for no conversion by comparing `str` to `*endptr`, not by
  inspecting `errno`.

---

Next: [`system`](#system), Previous: [`strtoul`](#strtoul), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.74 `strtoull` [¶](#strtoull-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtoull.html>

Gnulib module: strtoull

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.11, MSVC 9.
* This function does not parse the leading ‘`0`’ when the input string is
  `"0x"` and the base is 16 or 0 on some platforms:
  Minix 3.3, mingw, MSVC 14.
* This function does not parse binary integers (with a ‘`0b`’ or ‘`0B`’
  prefix) when the base is 2 or 0 on many platforms.

Portability problems not fixed by Gnulib:

* If no conversion could be performed, this function sets `errno`
  to `EINVAL` on some platforms. Portable programs should check
  for no conversion by comparing `str` to `*endptr`, not by
  inspecting `errno`.

---

Next: [`unlockpt`](#unlockpt), Previous: [`strtoull`](#strtoull), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.75 `system` [¶](#system-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/system.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-system-3.html>

Gnulib module: system-posix

Portability problems fixed by Gnulib:

* The macros `WIFSIGNALED`, `WIFEXITED`, `WIFSTOPPED`,
  `WTERMSIG`, `WEXITSTATUS`, `WNOHANG`, `WUNTRACED`,
  `WSTOPSIG` are not defined in `<stdlib.h>` (only in
  `<sys/wait.h>`) on some platforms:
  MirBSD 10.

Portability problems not fixed by Gnulib:

* On Windows platforms (excluding Cygwin), the command processor used by the
  `system` function is `cmd.exe`, not `/bin/sh`. Accordingly,
  the rules for quoting shell arguments containing spaces, quote or other special
  characters are different.

---

Next: [`unsetenv`](#unsetenv), Previous: [`system`](#system), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.76 `unlockpt` [¶](#unlockpt-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/unlockpt.html>

Gnulib module: unlockpt

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* This function reports success for invalid file descriptors on some platforms:
  NetBSD 5.1, Cygwin 1.7.9, Haiku.

---

Next: [`wcstombs`](#wcstombs), Previous: [`unlockpt`](#unlockpt), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.77 `unsetenv` [¶](#unsetenv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/unsetenv.html>

Gnulib module: unsetenv

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11.23, Solaris 9, mingw, MSVC 14.
* This function has the return type ‘`void`’ instead of ‘`int`’ on some
  platforms:
  FreeBSD 6.0, NetBSD 1.6, OpenBSD 3.8.
* On some platforms, this function does not fail with `EINVAL` when
  passed an empty string or a string containing ‘`=`’:
  FreeBSD 6.0, NetBSD 1.6, OpenBSD 4.7.
* This function removes only the first value association for the given
  environment variable, not all of them, on some platforms:
  Solaris 11.4, Haiku.

Portability problems not fixed by Gnulib:

* Older versions of POSIX required that `unsetenv(NULL)` gracefully
  fail with `EINVAL`, but not all implementations guarantee this,
  and the requirement was removed.

---

Next: [`wctomb`](#wctomb), Previous: [`unsetenv`](#unsetenv), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.78 `wcstombs` [¶](#wcstombs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcstombs.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and
  therefore cannot accommodate all Unicode characters.
  However, the Gnulib function `c32stombs`, provided by Gnulib module
  `c32stombs`, operates on 32-bit wide characters and therefore does not
  have this limitation.

---

Previous: [`wcstombs`](#wcstombs), Up: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.46.79 `wctomb` [¶](#wctomb-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wctomb.html>

Gnulib module: wctomb

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [Functions in `<strings.h>`](#Functions-in-_003cstrings_002eh_003e), Previous: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
