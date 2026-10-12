### 10.4 Functions in `<complex.h>` [¶](#Functions-in-_003ccomplex_002eh_003e-1)

* [`CMPLX`](#CMPLX)
* [`CMPLXF`](#CMPLXF)
* [`CMPLXL`](#CMPLXL)
* [`cabs`](#cabs)
* [`cabsf`](#cabsf)
* [`cabsl`](#cabsl)
* [`cacos`](#cacos)
* [`cacosf`](#cacosf)
* [`cacosh`](#cacosh)
* [`cacoshf`](#cacoshf)
* [`cacoshl`](#cacoshl)
* [`cacosl`](#cacosl)
* [`carg`](#carg)
* [`cargf`](#cargf)
* [`cargl`](#cargl)
* [`casin`](#casin)
* [`casinf`](#casinf)
* [`casinh`](#casinh)
* [`casinhf`](#casinhf)
* [`casinhl`](#casinhl)
* [`casinl`](#casinl)
* [`catan`](#catan)
* [`catanf`](#catanf)
* [`catanh`](#catanh)
* [`catanhf`](#catanhf)
* [`catanhl`](#catanhl)
* [`catanl`](#catanl)
* [`ccos`](#ccos)
* [`ccosf`](#ccosf)
* [`ccosh`](#ccosh)
* [`ccoshf`](#ccoshf)
* [`ccoshl`](#ccoshl)
* [`ccosl`](#ccosl)
* [`cexp`](#cexp)
* [`cexpf`](#cexpf)
* [`cexpl`](#cexpl)
* [`cimag`](#cimag)
* [`cimagf`](#cimagf)
* [`cimagl`](#cimagl)
* [`clog`](#clog)
* [`clogf`](#clogf)
* [`clogl`](#clogl)
* [`conj`](#conj)
* [`conjf`](#conjf)
* [`conjl`](#conjl)
* [`cpow`](#cpow)
* [`cpowf`](#cpowf)
* [`cpowl`](#cpowl)
* [`cproj`](#cproj)
* [`cprojf`](#cprojf)
* [`cprojl`](#cprojl)
* [`creal`](#creal)
* [`crealf`](#crealf)
* [`creall`](#creall)
* [`csin`](#csin)
* [`csinf`](#csinf)
* [`csinh`](#csinh)
* [`csinhf`](#csinhf)
* [`csinhl`](#csinhl)
* [`csinl`](#csinl)
* [`csqrt`](#csqrt)
* [`csqrtf`](#csqrtf)
* [`csqrtl`](#csqrtl)
* [`ctan`](#ctan)
* [`ctanf`](#ctanf)
* [`ctanh`](#ctanh)
* [`ctanhf`](#ctanhf)
* [`ctanhl`](#ctanhl)
* [`ctanl`](#ctanl)

---

Next: [`CMPLXF`](#CMPLXF), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.1 `CMPLX` [¶](#CMPLX-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.3.9.3.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/CMPLX.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This macro is missing on many platforms:
  glibc with GCC 4.9.x or clang 18, Mac OS X 10.5, FreeBSD 6.4, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.1, HP-UX 11.11, Solaris 11.0, Solaris 11 OpenIndiana, Cygwin, mingw, MSVC 14.

---

Next: [`CMPLXL`](#CMPLXL), Previous: [`CMPLX`](#CMPLX), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.2 `CMPLXF` [¶](#CMPLXF-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.3.9.3.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/CMPLXF.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This macro is missing on many platforms:
  glibc with GCC 4.9.x or clang 18, Mac OS X 10.5, FreeBSD 6.4, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.1, HP-UX 11.11, Solaris 11.0, Solaris 11 OpenIndiana, Cygwin, mingw, MSVC 14.

---

Next: [`cabs`](#cabs), Previous: [`CMPLXF`](#CMPLXF), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.3 `CMPLXL` [¶](#CMPLXL-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.3.9.3.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/CMPLXL.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This macro is missing on many platforms:
  glibc with GCC 4.9.x or clang 18, Mac OS X 10.5, FreeBSD 6.4, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.1, HP-UX 11.11, Solaris 11.0, Solaris 11 OpenIndiana, Cygwin, mingw, MSVC 14.

---

Next: [`cabsf`](#cabsf), Previous: [`CMPLXL`](#CMPLXL), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.4 `cabs` [¶](#cabs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cabs.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11, Solaris 9.

---

Next: [`cabsl`](#cabsl), Previous: [`cabs`](#cabs), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.5 `cabsf` [¶](#cabsf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cabsf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, mingw, MSVC 9.

---

Next: [`cacos`](#cacos), Previous: [`cabsf`](#cabsf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.6 `cabsl` [¶](#cabsl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cabsl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 10.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 5.1.

---

Next: [`cacosf`](#cacosf), Previous: [`cabsl`](#cabsl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.7 `cacos` [¶](#cacos-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cacos.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 5.1.

---

Next: [`cacosh`](#cacosh), Previous: [`cacos`](#cacos), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.8 `cacosf` [¶](#cacosf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cacosf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 5.1.

---

Next: [`cacoshf`](#cacoshf), Previous: [`cacosf`](#cacosf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.9 `cacosh` [¶](#cacosh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cacosh.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 5.1.

---

Next: [`cacoshl`](#cacoshl), Previous: [`cacosh`](#cacosh), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.10 `cacoshf` [¶](#cacoshf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cacoshf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 5.1.

---

Next: [`cacosl`](#cacosl), Previous: [`cacoshf`](#cacoshf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.11 `cacoshl` [¶](#cacoshl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cacoshl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 11.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Next: [`carg`](#carg), Previous: [`cacoshl`](#cacoshl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.12 `cacosl` [¶](#cacosl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cacosl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 11.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Next: [`cargf`](#cargf), Previous: [`cacosl`](#cacosl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.13 `carg` [¶](#carg-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/carg.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`cargl`](#cargl), Previous: [`carg`](#carg), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.14 `cargf` [¶](#cargf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cargf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`casin`](#casin), Previous: [`cargf`](#cargf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.15 `cargl` [¶](#cargl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cargl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 5.1.

---

Next: [`casinf`](#casinf), Previous: [`cargl`](#cargl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.16 `casin` [¶](#casin-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/casin.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 5.1.

---

Next: [`casinh`](#casinh), Previous: [`casin`](#casin), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.17 `casinf` [¶](#casinf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/casinf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 5.1.

---

Next: [`casinhf`](#casinhf), Previous: [`casinf`](#casinf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.18 `casinh` [¶](#casinh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/casinh.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 5.1.

---

Next: [`casinhl`](#casinhl), Previous: [`casinh`](#casinh), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.19 `casinhf` [¶](#casinhf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/casinhf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 5.1.

---

Next: [`casinl`](#casinl), Previous: [`casinhf`](#casinhf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.20 `casinhl` [¶](#casinhl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/casinhl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 11.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Next: [`catan`](#catan), Previous: [`casinhl`](#casinhl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.21 `casinl` [¶](#casinl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/casinl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 11.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Next: [`catanf`](#catanf), Previous: [`casinl`](#casinl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.22 `catan` [¶](#catan-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/catan.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 5.1.

---

Next: [`catanh`](#catanh), Previous: [`catan`](#catan), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.23 `catanf` [¶](#catanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/catanf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 5.1.

---

Next: [`catanhf`](#catanhf), Previous: [`catanf`](#catanf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.24 `catanh` [¶](#catanh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/catanh.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 5.1.

---

Next: [`catanhl`](#catanhl), Previous: [`catanh`](#catanh), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.25 `catanhf` [¶](#catanhf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/catanhf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 5.1.

---

Next: [`catanl`](#catanl), Previous: [`catanhf`](#catanhf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.26 `catanhl` [¶](#catanhl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/catanhl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 11.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Next: [`ccos`](#ccos), Previous: [`catanhl`](#catanhl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.27 `catanl` [¶](#catanl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/catanl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 11.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Next: [`ccosf`](#ccosf), Previous: [`catanl`](#catanl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.28 `ccos` [¶](#ccos-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ccos.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11,
  Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`ccosh`](#ccosh), Previous: [`ccos`](#ccos), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.29 `ccosf` [¶](#ccosf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ccosf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11,
  Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`ccoshf`](#ccoshf), Previous: [`ccosf`](#ccosf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.30 `ccosh` [¶](#ccosh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ccosh.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`ccoshl`](#ccoshl), Previous: [`ccosh`](#ccosh), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.31 `ccoshf` [¶](#ccoshf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ccoshf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`ccosl`](#ccosl), Previous: [`ccoshf`](#ccoshf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.32 `ccoshl` [¶](#ccoshl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ccoshl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Next: [`cexp`](#cexp), Previous: [`ccoshl`](#ccoshl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.33 `ccosl` [¶](#ccosl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ccosl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Next: [`cexpf`](#cexpf), Previous: [`ccosl`](#ccosl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.34 `cexp` [¶](#cexp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cexp.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11,
  Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`cexpl`](#cexpl), Previous: [`cexp`](#cexp), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.35 `cexpf` [¶](#cexpf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cexpf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11,
  Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`cimag`](#cimag), Previous: [`cexpf`](#cexpf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.36 `cexpl` [¶](#cexpl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cexpl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 13.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Next: [`cimagf`](#cimagf), Previous: [`cexpl`](#cexpl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.37 `cimag` [¶](#cimag-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cimag.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`cimagl`](#cimagl), Previous: [`cimag`](#cimag), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.38 `cimagf` [¶](#cimagf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cimagf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`clog`](#clog), Previous: [`cimagf`](#cimagf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.39 `cimagl` [¶](#cimagl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cimagl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 5.1.

---

Next: [`clogf`](#clogf), Previous: [`cimagl`](#cimagl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.40 `clog` [¶](#clog-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/clog.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 11.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 7.1.

---

Next: [`clogl`](#clogl), Previous: [`clog`](#clog), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.41 `clogf` [¶](#clogf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/clogf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 11.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 7.1.

---

Next: [`conj`](#conj), Previous: [`clogf`](#clogf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.42 `clogl` [¶](#clogl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/clogl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 11.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Next: [`conjf`](#conjf), Previous: [`clogl`](#clogl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.43 `conj` [¶](#conj-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/conj.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`conjl`](#conjl), Previous: [`conj`](#conj), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.44 `conjf` [¶](#conjf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/conjf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`cpow`](#cpow), Previous: [`conjf`](#conjf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.45 `conjl` [¶](#conjl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/conjl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 5.1.

---

Next: [`cpowf`](#cpowf), Previous: [`conjl`](#conjl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.46 `cpow` [¶](#cpow-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cpow.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 11.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 7.1.

---

Next: [`cpowl`](#cpowl), Previous: [`cpow`](#cpow), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.47 `cpowf` [¶](#cpowf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cpowf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 11.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9, Android 7.1.

---

Next: [`cproj`](#cproj), Previous: [`cpowf`](#cpowf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.48 `cpowl` [¶](#cpowl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cpowl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 11.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Next: [`cprojf`](#cprojf), Previous: [`cpowl`](#cpowl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.49 `cproj` [¶](#cproj-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cproj.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.
* The glibc implementation is or was broken.

---

Next: [`cprojl`](#cprojl), Previous: [`cproj`](#cproj), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.50 `cprojf` [¶](#cprojf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cprojf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.
* The glibc implementation is or was broken.

---

Next: [`creal`](#creal), Previous: [`cprojf`](#cprojf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.51 `cprojl` [¶](#cprojl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cprojl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 5.1.
* The glibc implementation is or was broken.

---

Next: [`crealf`](#crealf), Previous: [`cprojl`](#cprojl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.52 `creal` [¶](#creal-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/creal.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`creall`](#creall), Previous: [`creal`](#creal), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.53 `crealf` [¶](#crealf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/crealf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`csin`](#csin), Previous: [`crealf`](#crealf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.54 `creall` [¶](#creall-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/creall.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 5.1.

---

Next: [`csinf`](#csinf), Previous: [`creall`](#creall), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.55 `csin` [¶](#csin-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/csin.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11,
  Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`csinh`](#csinh), Previous: [`csin`](#csin), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.56 `csinf` [¶](#csinf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/csinf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11,
  Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`csinhf`](#csinhf), Previous: [`csinf`](#csinf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.57 `csinh` [¶](#csinh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/csinh.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`csinhl`](#csinhl), Previous: [`csinh`](#csinh), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.58 `csinhf` [¶](#csinhf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/csinhf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`csinl`](#csinl), Previous: [`csinhf`](#csinhf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.59 `csinhl` [¶](#csinhl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/csinhl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Next: [`csqrt`](#csqrt), Previous: [`csinhl`](#csinhl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.60 `csinl` [¶](#csinl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/csinl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Next: [`csqrtf`](#csqrtf), Previous: [`csinl`](#csinl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.61 `csqrt` [¶](#csqrt-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/csqrt.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11,
  Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`csqrtl`](#csqrtl), Previous: [`csqrt`](#csqrt), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.62 `csqrtf` [¶](#csqrtf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/csqrtf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11,
  Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`ctan`](#ctan), Previous: [`csqrtf`](#csqrtf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.63 `csqrtl` [¶](#csqrtl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/csqrtl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 5.1.

---

Next: [`ctanf`](#ctanf), Previous: [`csqrtl`](#csqrtl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.64 `ctan` [¶](#ctan-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ctan.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`ctanh`](#ctanh), Previous: [`ctan`](#ctan), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.65 `ctanf` [¶](#ctanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ctanf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`ctanhf`](#ctanhf), Previous: [`ctanf`](#ctanf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.66 `ctanh` [¶](#ctanh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ctanh.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`ctanhl`](#ctanhl), Previous: [`ctanh`](#ctanh), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.67 `ctanhf` [¶](#ctanhf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ctanhf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`ctanl`](#ctanl), Previous: [`ctanhf`](#ctanhf), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.68 `ctanhl` [¶](#ctanhl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ctanhl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Previous: [`ctanhl`](#ctanhl), Up: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.4.69 `ctanl` [¶](#ctanl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ctanl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 7.1.

---

Next: [Functions in `<devctl.h>`](#Functions-in-_003cdevctl_002eh_003e), Previous: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
