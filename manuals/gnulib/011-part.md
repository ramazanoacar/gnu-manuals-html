* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 9, Cygwin 1.7.x, Android 4.4.
* This function is only defined as a macro with arguments on some platforms:
  MSVC 14.
* This function returns wrong results on some platforms:
  glibc 2.7 on Linux/SPARC64.
* This function produces results which are accurate to only 16 digits on some
  platforms:
  musl libc 1.2.2/arm64, musl libc 1.2.2/s390x, NetBSD 10.0.

Portability problems not fixed by Gnulib:

---

Next: [`logp1f`](#logp1f), Previous: [`logl`](#logl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.219 `logp1` [¶](#logp1-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.6.14

Gnulib module: logp1

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.39, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

Portability problems not fixed by Gnulib:

---

Next: [`logp1l`](#logp1l), Previous: [`logp1`](#logp1), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.220 `logp1f` [¶](#logp1f-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.6.14

Gnulib module: logp1f

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.39, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

Portability problems not fixed by Gnulib:

---

Next: [`lrint`](#lrint), Previous: [`logp1f`](#logp1f), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.221 `logp1l` [¶](#logp1l-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.6.14

Gnulib module: logp1l

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.39, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

Portability problems not fixed by Gnulib:

---

Next: [`lrintf`](#lrintf), Previous: [`logp1l`](#logp1l), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.222 `lrint` [¶](#lrint-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lrint.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, Solaris 9, MSVC 9.

---

Next: [`lrintl`](#lrintl), Previous: [`lrint`](#lrint), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.223 `lrintf` [¶](#lrintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lrintf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, MSVC 9.

---

Next: [`lround`](#lround), Previous: [`lrintf`](#lrintf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.224 `lrintl` [¶](#lrintl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lrintl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 9.3, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.5.x, MSVC 9, Android 4.4.

---

Next: [`lroundf`](#lroundf), Previous: [`lrintl`](#lrintl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.225 `lround` [¶](#lround-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lround.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, Solaris 9, MSVC 9.

---

Next: [`lroundl`](#lroundl), Previous: [`lround`](#lround), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.226 `lroundf` [¶](#lroundf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lroundf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, MSVC 9.

---

Next: [`math_errhandling`](#math_005ferrhandling), Previous: [`lroundf`](#lroundf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.227 `lroundl` [¶](#lroundl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lroundl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 7.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, MSVC 9.

---

Next: [`modf`](#modf), Previous: [`lroundl`](#lroundl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.228 `math_errhandling` [¶](#math_005ferrhandling-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.1

Documentation:  
 [man math\_error](https://www.kernel.org/doc/man-pages/online/pages/man7/math_error.7.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This macro is missing on some platforms:
  NetBSD 10.0, mingw.
* This macro does not describe the error behaviour
  of elementary arithmetic operations (+, -, \*, /)
  and of mathematical operations for which the compiler emits inline code
  (such as `sqrt` on some CPUs).
* This macro does not describe the error behaviour of functions
  such as `strtod`.
* For mathematical operations in general, it is a safer bet to look
  at the exceptions set in the floating-point environment
  (by calling `feclearexcept (FE_ALL_EXCEPT)` before the operation
  and `fetestexcept` after the operation),
  regardless of `math_errhandling`.

---

Next: [`modff`](#modff), Previous: [`math_errhandling`](#math_005ferrhandling), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.229 `modf` [¶](#modf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/modf.html>

Gnulib module: modf or modf-ieee

Portability problems fixed by either Gnulib module `modf` or `modf-ieee`:

Portability problems fixed by Gnulib module `modf-ieee`:

* This function has problems with a NaN argument on some platforms:
  NetBSD 5.1, Cygwin.
* This function has problems with infinite arguments on some platforms:
  FreeBSD 6.4, OpenBSD 7.3, mingw with MSVCRT.

Portability problems not fixed by Gnulib:

---

Next: [`modfl`](#modfl), Previous: [`modf`](#modf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.230 `modff` [¶](#modff-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/modff.html>

Gnulib module: modff or modff-ieee

Portability problems fixed by either Gnulib module `modff` or `modff-ieee`:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11.
* This function is only defined as a buggy macro with arguments on some platforms:
  MSVC 14.

Portability problems fixed by Gnulib module `modff-ieee`:

* This function has problems with a NaN argument on some platforms:
  NetBSD 5.1, OpenBSD 7.9, Solaris 9, Cygwin.
* This function has problems with infinite arguments on some platforms:
  mingw with MSVCRT.

Portability problems not fixed by Gnulib:

---

Next: [`nan`](#nan), Previous: [`modff`](#modff), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.231 `modfl` [¶](#modfl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/modfl.html>

Gnulib module: modfl or modfl-ieee

Portability problems fixed by either Gnulib module `modfl` or `modfl-ieee`:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 9, Cygwin 1.7.x, Android 4.4.
* This function is only defined as a macro with arguments on some platforms:
  MSVC 14.
* This function produces wrong results on some platforms:
  NetBSD 10.0/arm64.

Portability problems fixed by Gnulib module `modfl-ieee`:

* This function has problems with infinite arguments on some platforms:
  mingw.

Portability problems not fixed by Gnulib:

---

Next: [`nanf`](#nanf), Previous: [`modfl`](#modfl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.232 `nan` [¶](#nan-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nan.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, Solaris 9, MSVC 9.

---

Next: [`nanl`](#nanl), Previous: [`nan`](#nan), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.233 `nanf` [¶](#nanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nanf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, MSVC 9.

---

Next: [`nearbyint`](#nearbyint), Previous: [`nanf`](#nanf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.234 `nanl` [¶](#nanl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nanl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, MSVC 9, Android 3.1.

---

Next: [`nearbyintf`](#nearbyintf), Previous: [`nanl`](#nanl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.235 `nearbyint` [¶](#nearbyint-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nearbyint.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, AIX 5.1, Solaris 9, MSVC 9.

---

Next: [`nearbyintl`](#nearbyintl), Previous: [`nearbyint`](#nearbyint), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.236 `nearbyintf` [¶](#nearbyintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nearbyintf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, MSVC 9.

---

Next: [`nextafter`](#nextafter), Previous: [`nearbyintf`](#nearbyintf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.237 `nearbyintl` [¶](#nearbyintl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nearbyintl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, MSVC 9, Android 4.4.

---

Next: [`nextafterf`](#nextafterf), Previous: [`nearbyintl`](#nearbyintl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.238 `nextafter` [¶](#nextafter-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nextafter.html>

Gnulib module: nextafter

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, MSVC 9.

---

Next: [`nextafterl`](#nextafterl), Previous: [`nextafter`](#nextafter), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.239 `nextafterf` [¶](#nextafterf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nextafterf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Solaris 9, MSVC 9.

---

Next: [`nextdown`](#nextdown), Previous: [`nextafterf`](#nextafterf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.240 `nextafterl` [¶](#nextafterl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nextafterl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 4.4.

---

Next: [`nextdownf`](#nextdownf), Previous: [`nextafterl`](#nextafterl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.241 `nextdown` [¶](#nextdown-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/FP-Bit-Twiddling.html>,
* [man nextdown](https://www.kernel.org/doc/man-pages/online/pages/man3/nextdown.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.23, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`nextdownl`](#nextdownl), Previous: [`nextdown`](#nextdown), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.242 `nextdownf` [¶](#nextdownf-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/FP-Bit-Twiddling.html>,
* [man nextdownf](https://www.kernel.org/doc/man-pages/online/pages/man3/nextdownf.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.23, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`nexttoward`](#nexttoward), Previous: [`nextdownf`](#nextdownf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.243 `nextdownl` [¶](#nextdownl-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/FP-Bit-Twiddling.html>,
* [man nextdownl](https://www.kernel.org/doc/man-pages/online/pages/man3/nextdownl.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.23, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`nexttowardf`](#nexttowardf), Previous: [`nextdownl`](#nextdownl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.244 `nexttoward` [¶](#nexttoward-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nexttoward.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 4.2.

---

Next: [`nexttowardl`](#nexttowardl), Previous: [`nexttoward`](#nexttoward), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.245 `nexttowardf` [¶](#nexttowardf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nexttowardf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9.

---

Next: [`nextup`](#nextup), Previous: [`nexttowardf`](#nexttowardf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.246 `nexttowardl` [¶](#nexttowardl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nexttowardl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 8.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 9, Android 4.2.

---

Next: [`nextupf`](#nextupf), Previous: [`nexttowardl`](#nexttowardl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.247 `nextup` [¶](#nextup-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/FP-Bit-Twiddling.html>,
* [man nextup](https://www.kernel.org/doc/man-pages/online/pages/man3/nextup.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.23, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`nextupl`](#nextupl), Previous: [`nextup`](#nextup), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.248 `nextupf` [¶](#nextupf-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/FP-Bit-Twiddling.html>,
* [man nextupf](https://www.kernel.org/doc/man-pages/online/pages/man3/nextupf.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.23, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`pow`](#pow), Previous: [`nextupf`](#nextupf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.249 `nextupl` [¶](#nextupl-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/FP-Bit-Twiddling.html>,
* [man nextupl](https://www.kernel.org/doc/man-pages/online/pages/man3/nextupl.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.23, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`powf`](#powf), Previous: [`nextupl`](#nextupl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.250 `pow` [¶](#pow-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pow.html>

Gnulib module: pow

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`powl`](#powl), Previous: [`pow`](#pow), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.251 `powf` [¶](#powf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/powf.html>

Gnulib module: powf

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Solaris 9.
* This function is only defined as a macro with arguments on some platforms:
  MSVC 9.

Portability problems not fixed by Gnulib:

---

Next: [`pown`](#pown), Previous: [`powf`](#powf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.252 `powl` [¶](#powl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/powl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 9, Cygwin 1.7.x, Android 4.4.
* This function is only defined as a macro with arguments on some platforms:
  MSVC 14.
* This function produces results which are accurate to only 16 digits on some
  platforms:
  NetBSD 10.0.

---

Next: [`pownf`](#pownf), Previous: [`powl`](#powl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.253 `pown` [¶](#pown-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.7.6

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all platforms:
  glibc 2.41, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`pownl`](#pownl), Previous: [`pown`](#pown), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.254 `pownf` [¶](#pownf-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.7.6

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all platforms:
  glibc 2.41, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`powr`](#powr), Previous: [`pownf`](#pownf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.255 `pownl` [¶](#pownl-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.7.6

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all platforms:
  glibc 2.41, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`powrf`](#powrf), Previous: [`pownl`](#pownl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.256 `powr` [¶](#powr-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.7.7

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all platforms:
  glibc 2.41, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`powrl`](#powrl), Previous: [`powr`](#powr), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.257 `powrf` [¶](#powrf-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.7.7

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all platforms:
  glibc 2.41, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`remainder`](#remainder), Previous: [`powrf`](#powrf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.258 `powrl` [¶](#powrl-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.7.7

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all platforms:
  glibc 2.41, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`remainderf`](#remainderf), Previous: [`powrl`](#powrl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.259 `remainder` [¶](#remainder-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/remainder.html>

Gnulib module: remainder or remainder-ieee

Portability problems fixed by either Gnulib module `remainder` or `remainder-ieee`:

* This function is missing on some platforms:
  MSVC 9.

Portability problems fixed by Gnulib module `remainder-ieee`:

Portability problems not fixed by Gnulib:

---

Next: [`remainderl`](#remainderl), Previous: [`remainder`](#remainder), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.260 `remainderf` [¶](#remainderf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/remainderf.html>

Gnulib module: remainderf or remainderf-ieee

Portability problems fixed by either Gnulib module `remainderf` or `remainderf-ieee`:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, MSVC 9.

Portability problems fixed by Gnulib module `remainderf-ieee`:

Portability problems not fixed by Gnulib:

---

Next: [`remquo`](#remquo), Previous: [`remainderf`](#remainderf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.261 `remainderl` [¶](#remainderl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/remainderl.html>

Gnulib module: remainderl or remainderl-ieee

Portability problems fixed by either Gnulib module `remainderl` or `remainderl-ieee`:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 9.3, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, MSVC 9, Android 4.4.
* This function returns completely wrong values on some platforms:
  OpenBSD 5.1/SPARC.
* This function produces results which are accurate to only 16 digits on some
  platforms:
  musl libc 1.2.2/arm64, musl libc 1.2.2/s390x, NetBSD 10.0.

Portability problems fixed by Gnulib module `remainderl-ieee`:

Portability problems not fixed by Gnulib:

---

Next: [`remquof`](#remquof), Previous: [`remainderl`](#remainderl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.262 `remquo` [¶](#remquo-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/remquo.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, Solaris 9, MSVC 9.

---

Next: [`remquol`](#remquol), Previous: [`remquo`](#remquo), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.263 `remquof` [¶](#remquof-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/remquof.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, MSVC 9.

---

Next: [`rint`](#rint), Previous: [`remquof`](#remquof), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.264 `remquol` [¶](#remquol-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/remquol.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 9.3, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, MSVC 9, Android 4.4.

---

Next: [`rintf`](#rintf), Previous: [`remquol`](#remquol), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.265 `rint` [¶](#rint-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/rint.html>

Gnulib module: rint or rint-ieee

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 9.

Portability problems not fixed by Gnulib:

---

Next: [`rintl`](#rintl), Previous: [`rint`](#rint), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.266 `rintf` [¶](#rintf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/rintf.html>

Gnulib module: rintf or rintf-ieee

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, MSVC 9.

Portability problems not fixed by Gnulib:

---

Next: [`rootn`](#rootn), Previous: [`rintf`](#rintf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.267 `rintl` [¶](#rintl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/rintl.html>

Gnulib module: rintl or rintl-ieee

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.5.x, MSVC 9, Android 4.4.
* This function produces wrong results for negative numbers on some platforms:
  NetBSD 8.0.

Portability problems not fixed by Gnulib:

---

Next: [`rootnf`](#rootnf), Previous: [`rintl`](#rintl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.268 `rootn` [¶](#rootn-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.7.8

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all platforms:
  glibc 2.41, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`rootnl`](#rootnl), Previous: [`rootn`](#rootn), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.269 `rootnf` [¶](#rootnf-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.7.8

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all platforms:
  glibc 2.41, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`round`](#round), Previous: [`rootnf`](#rootnf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.270 `rootnl` [¶](#rootnl-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.7.8

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all platforms:
  glibc 2.41, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`roundeven`](#roundeven), Previous: [`rootnl`](#rootnl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.271 `round` [¶](#round-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/round.html>

Gnulib module: round or round-ieee

Portability problems fixed by either Gnulib module `round` or `round-ieee`:

* This function is missing on some platforms:
  FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, Solaris 9, MSVC 9.
* This function is not declared on some platforms:
  glibc 2.8.
* This functions returns a wrong result for *x = 1/2 - 2^-54*
  on some platforms:
  NetBSD 3.0, AIX 7.1, mingw with MSVCRT.

Portability problems fixed by Gnulib module `round-ieee`:

* This function returns a positive zero for an argument between −0.5 and 0
  on some platforms:
  AIX 7.1.

Portability problems not fixed by Gnulib:

---

Next: [`roundevenf`](#roundevenf), Previous: [`round`](#round), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.272 `roundeven` [¶](#roundeven-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Rounding-Functions.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`roundevenl`](#roundevenl), Previous: [`roundeven`](#roundeven), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.273 `roundevenf` [¶](#roundevenf-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Rounding-Functions.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`roundf`](#roundf), Previous: [`roundevenf`](#roundevenf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.274 `roundevenl` [¶](#roundevenl-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Rounding-Functions.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`roundl`](#roundl), Previous: [`roundevenl`](#roundevenl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.275 `roundf` [¶](#roundf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/roundf.html>

Gnulib module: roundf or roundf-ieee

Portability problems fixed by either Gnulib module `roundf` or `roundf-ieee`:

* This function is missing on some platforms:
  FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, MSVC 9.
* This function is not declared on some platforms:
  glibc 2.8.
* This functions returns a wrong result for *x = 1/2 - 2^-25*
  on some platforms:
  mingw with MSVCRT.

Portability problems fixed by Gnulib module `roundf-ieee`:

* This function returns a positive zero for an argument between −0.5 and 0
  on some platforms:
  AIX 7.1.

Portability problems not fixed by Gnulib:

---

Next: [`rsqrt`](#rsqrt), Previous: [`roundf`](#roundf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.276 `roundl` [¶](#roundl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/roundl.html>

Gnulib module: roundl or roundl-ieee

Portability problems fixed by either Gnulib module `roundl` or `roundl-ieee`:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, MSVC 9.
* This function is not declared on some platforms:
  glibc 2.8.

Portability problems fixed by Gnulib module `roundl-ieee`:

* This function returns a positive zero for an argument between −0.5 and 0
  on some platforms:
  AIX 7.1.

Portability problems not fixed by Gnulib:

---

Next: [`rsqrtf`](#rsqrtf), Previous: [`roundl`](#roundl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.277 `rsqrt` [¶](#rsqrt-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.7.9

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.41, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`rsqrtl`](#rsqrtl), Previous: [`rsqrt`](#rsqrt), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.278 `rsqrtf` [¶](#rsqrtf-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.7.9

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.41, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`scalbln`](#scalbln), Previous: [`rsqrtf`](#rsqrtf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.279 `rsqrtl` [¶](#rsqrtl-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.7.9

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.41, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`scalblnf`](#scalblnf), Previous: [`rsqrtl`](#rsqrtl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.280 `scalbln` [¶](#scalbln-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/scalbln.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, AIX 5.1, HP-UX 11, Solaris 9, MSVC 9, Android 9.0.

---

Next: [`scalblnl`](#scalblnl), Previous: [`scalbln`](#scalbln), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.281 `scalblnf` [¶](#scalblnf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/scalblnf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, AIX 5.1, HP-UX 11, Solaris 9, MSVC 9, Android 9.0.

---

Next: [`scalbn`](#scalbn), Previous: [`scalblnf`](#scalblnf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.282 `scalblnl` [¶](#scalblnl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/scalblnl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, MSVC 9, Android 9.0.

---

Next: [`scalbnf`](#scalbnf), Previous: [`scalblnl`](#scalblnl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.283 `scalbn` [¶](#scalbn-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/scalbn.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, MSVC 9.

---

Next: [`scalbnl`](#scalbnl), Previous: [`scalbn`](#scalbn), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.284 `scalbnf` [¶](#scalbnf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/scalbnf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 9, MSVC 9.

---

Next: [`setpayload`](#setpayload), Previous: [`scalbnf`](#scalbnf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.285 `scalbnl` [¶](#scalbnl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/scalbnl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, MSVC 9.

---

Next: [`setpayloadf`](#setpayloadf), Previous: [`scalbnl`](#scalbnl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.286 `setpayload` [¶](#setpayload-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section F.10.13.2

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/FP-Bit-Twiddling.html>.

Gnulib module: setpayload

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`setpayloadl`](#setpayloadl), Previous: [`setpayload`](#setpayload), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.287 `setpayloadf` [¶](#setpayloadf-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section F.10.13.2

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/FP-Bit-Twiddling.html>.

Gnulib module: setpayloadf

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`setpayloadsig`](#setpayloadsig), Previous: [`setpayloadf`](#setpayloadf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.288 `setpayloadl` [¶](#setpayloadl-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section F.10.13.2

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/FP-Bit-Twiddling.html>.

Gnulib module: setpayloadl

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`setpayloadsigf`](#setpayloadsigf), Previous: [`setpayloadl`](#setpayloadl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.289 `setpayloadsig` [¶](#setpayloadsig-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section F.10.13.3

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/FP-Bit-Twiddling.html>.

Gnulib module: setpayloadsig

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`setpayloadsigl`](#setpayloadsigl), Previous: [`setpayloadsig`](#setpayloadsig), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.290 `setpayloadsigf` [¶](#setpayloadsigf-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section F.10.13.3

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/FP-Bit-Twiddling.html>.

Gnulib module: setpayloadsigf

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`signbit`](#signbit), Previous: [`setpayloadsigf`](#setpayloadsigf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.291 `setpayloadsigl` [¶](#setpayloadsigl-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section F.10.13.3

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/FP-Bit-Twiddling.html>.

Gnulib module: setpayloadsigl

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`signgam`](#signgam), Previous: [`setpayloadsigl`](#setpayloadsigl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.292 `signbit` [¶](#signbit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/signbit.html>

Gnulib module: signbit

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, OpenBSD 7.9, AIX 5.1, Solaris 11.0, MSVC 9, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`sin`](#sin), Previous: [`signbit`](#signbit), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.293 `signgam` [¶](#signgam-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/signgam.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on some platforms:
  macOS 10.13, Minix 3.1.8, AIX 5.1, mingw, MSVC 14.

---

Next: [`sinf`](#sinf), Previous: [`signgam`](#signgam), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.294 `sin` [¶](#sin-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sin.html>

Gnulib module: sin

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`sinh`](#sinh), Previous: [`sin`](#sin), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.295 `sinf` [¶](#sinf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sinf.html>

Gnulib module: sinf

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Solaris 9.
* This function is only defined as a macro with arguments on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`sinhf`](#sinhf), Previous: [`sinf`](#sinf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.296 `sinh` [¶](#sinh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sinh.html>

Gnulib module: sinh

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`sinhl`](#sinhl), Previous: [`sinh`](#sinh), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.297 `sinhf` [¶](#sinhf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sinhf.html>

Gnulib module: sinhf

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Solaris 9.
* This function is only defined as a macro with arguments on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`sinl`](#sinl), Previous: [`sinhf`](#sinhf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.298 `sinhl` [¶](#sinhl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sinhl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 9, Cygwin 1.7.x, Android 4.4.
* This function is only defined as a macro with arguments on some platforms:
  MSVC 14.
* This function produces results which are accurate to only 16 digits on some
  platforms:
  NetBSD 10.0.

---

Next: [`sinpi`](#sinpi), Previous: [`sinhl`](#sinhl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.299 `sinl` [¶](#sinl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sinl.html>

Gnulib module: sinl

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 9, Cygwin 1.7.x, Android 4.4.
* This function is only defined as a macro with arguments on some platforms:
  MSVC 14.
* This function returns a value that is larger than the argument,
  for some positive arguments,
  on some platforms:
  NetBSD 10.0.

Portability problems not fixed by Gnulib:

* This function produces results which are accurate to only 16 digits on some
  platforms:
  NetBSD 10.0.

---

Next: [`sinpif`](#sinpif), Previous: [`sinl`](#sinl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.300 `sinpi` [¶](#sinpi-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.4.13

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-C23 platforms:
  glibc 2.40, macOS 14, FreeBSD 13.4, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`sinpil`](#sinpil), Previous: [`sinpi`](#sinpi), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.301 `sinpif` [¶](#sinpif-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.4.13

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-C23 platforms:
  glibc 2.40, macOS 14, FreeBSD 13.4, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`sqrt`](#sqrt), Previous: [`sinpif`](#sinpif), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.302 `sinpil` [¶](#sinpil-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.4.13

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-C23 platforms:
  glibc 2.40, macOS 14, FreeBSD 13.4, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`sqrtf`](#sqrtf), Previous: [`sinpil`](#sinpil), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.303 `sqrt` [¶](#sqrt-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sqrt.html>

Gnulib module: sqrt or sqrt-ieee

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`sqrtl`](#sqrtl), Previous: [`sqrt`](#sqrt), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.304 `sqrtf` [¶](#sqrtf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sqrtf.html>

Gnulib module: sqrtf or sqrtf-ieee

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Solaris 9.
* This function is only defined as a macro with arguments on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`tan`](#tan), Previous: [`sqrtf`](#sqrtf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.305 `sqrtl` [¶](#sqrtl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sqrtl.html>

Gnulib module: sqrtl or sqrtl-ieee

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 9, Cygwin 1.7.x, Android 4.4.
* This function is only defined as a macro with arguments on some platforms:
  MSVC 14.
* This function produces very imprecise results on some platforms:
  OpenBSD 5.1/SPARC.

Portability problems not fixed by Gnulib:

---

Next: [`tanf`](#tanf), Previous: [`sqrtl`](#sqrtl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.306 `tan` [¶](#tan-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tan.html>

Gnulib module: tan

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`tanh`](#tanh), Previous: [`tan`](#tan), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.307 `tanf` [¶](#tanf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tanf.html>

Gnulib module: tanf

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Solaris 9.
* This function is only defined as a macro with arguments on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`tanhf`](#tanhf), Previous: [`tanf`](#tanf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.308 `tanh` [¶](#tanh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tanh.html>

Gnulib module: tanh

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`tanhl`](#tanhl), Previous: [`tanh`](#tanh), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.309 `tanhf` [¶](#tanhf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tanhf.html>

Gnulib module: tanhf

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Solaris 9.
* This function is only defined as a macro with arguments on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`tanl`](#tanl), Previous: [`tanhf`](#tanhf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.310 `tanhl` [¶](#tanhl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tanhl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 9, Cygwin 1.7.x, Android 4.4.
* This function is only defined as a macro with arguments on some platforms:
  MSVC 14.
* This function produces results which are accurate to only 16 digits on some
  platforms:
  NetBSD 10.0.

---

Next: [`tanpi`](#tanpi), Previous: [`tanhl`](#tanhl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.311 `tanl` [¶](#tanl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tanl.html>

Gnulib module: tanl

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 9, Cygwin 1.7.x, Android 4.4.
* This function is only defined as a macro with arguments on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

* This function produces results which are accurate to only 16 digits on some
  platforms:
  NetBSD 10.0.

---

Next: [`tanpif`](#tanpif), Previous: [`tanl`](#tanl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.312 `tanpi` [¶](#tanpi-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.4.14

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-C23 platforms:
  glibc 2.40, macOS 14, FreeBSD 13.4, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`tanpil`](#tanpil), Previous: [`tanpi`](#tanpi), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.313 `tanpif` [¶](#tanpif-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.4.14

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-C23 platforms:
  glibc 2.40, macOS 14, FreeBSD 13.4, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`tgamma`](#tgamma), Previous: [`tanpif`](#tanpif), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.314 `tanpil` [¶](#tanpil-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.12.4.14

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-C23 platforms:
  glibc 2.40, macOS 14, FreeBSD 13.4, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`tgammaf`](#tgammaf), Previous: [`tanpil`](#tanpil), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.315 `tgamma` [¶](#tgamma-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tgamma.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, MSVC 9.

---

Next: [`tgammal`](#tgammal), Previous: [`tgamma`](#tgamma), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.316 `tgammaf` [¶](#tgammaf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tgammaf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, MSVC 9.

---

Next: [`totalorder`](#totalorder), Previous: [`tgammaf`](#tgammaf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.317 `tgammal` [¶](#tgammal-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tgammal.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 9.3, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, MSVC 9, Android 4.4.

---

Next: [`totalorderf`](#totalorderf), Previous: [`tgammal`](#tgammal), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.318 `totalorder` [¶](#totalorder-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section F.10.12.1

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/FP-Comparison-Functions.html>.
* <https://lists.gnu.org/archive/html/bug-gnulib/2023-10/msg00037.html>

Gnulib module: totalorder

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function has a different signature on some platforms:
  glibc 2.30.

Portability problems not fixed by Gnulib:

* This function treats signalling NaNs incorrectly on some platforms:
  glibc 2.35/sh4.

---

Next: [`totalorderl`](#totalorderl), Previous: [`totalorder`](#totalorder), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.319 `totalorderf` [¶](#totalorderf-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section F.10.12.1

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/FP-Comparison-Functions.html>.
* <https://lists.gnu.org/archive/html/bug-gnulib/2023-10/msg00037.html>

Gnulib module: totalorderf

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function has a different signature on some platforms:
  glibc 2.30.

Portability problems not fixed by Gnulib:

* This function treats signalling NaNs incorrectly on some platforms:
  glibc 2.35/sh4.

---

Next: [`totalordermag`](#totalordermag), Previous: [`totalorderf`](#totalorderf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.320 `totalorderl` [¶](#totalorderl-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section F.10.12.1

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/FP-Comparison-Functions.html>.
* <https://lists.gnu.org/archive/html/bug-gnulib/2023-10/msg00037.html>

Gnulib module: totalorderl

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function has a different signature on some platforms:
  glibc 2.30.

Portability problems not fixed by Gnulib:

* This function treats signalling NaNs incorrectly on some platforms:
  glibc 2.35/sh4.

---

Next: [`totalordermagf`](#totalordermagf), Previous: [`totalorderl`](#totalorderl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.321 `totalordermag` [¶](#totalordermag-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section F.10.12.2

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/FP-Comparison-Functions.html>.

Gnulib module: totalordermag

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function has a different signature on some platforms:
  glibc 2.30.

Portability problems not fixed by Gnulib:

---

Next: [`totalordermagl`](#totalordermagl), Previous: [`totalordermag`](#totalordermag), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.322 `totalordermagf` [¶](#totalordermagf-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section F.10.12.2

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/FP-Comparison-Functions.html>.

Gnulib module: totalordermagf

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function has a different signature on some platforms:
  glibc 2.30.

Portability problems not fixed by Gnulib:

---

Next: [`trunc`](#trunc), Previous: [`totalordermagf`](#totalordermagf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.323 `totalordermagl` [¶](#totalordermagl-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section F.10.12.2

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/FP-Comparison-Functions.html>.

Gnulib module: totalordermagl

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function has a different signature on some platforms:
  glibc 2.30.

Portability problems not fixed by Gnulib:

---

Next: [`truncf`](#truncf), Previous: [`totalordermagl`](#totalordermagl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.324 `trunc` [¶](#trunc-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/trunc.html>

Gnulib module: trunc or trunc-ieee

Portability problems fixed by either Gnulib module `trunc` or `trunc-ieee`:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 3.0, OpenBSD 3.8, Solaris 9, MSVC 9.
* This function is not declared (without `-D_GNU_SOURCE`) on some platforms:
  glibc 2.8.

Portability problems fixed by Gnulib module `trunc-ieee`:

Portability problems not fixed by Gnulib:

---

Next: [`truncl`](#truncl), Previous: [`trunc`](#trunc), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.325 `truncf` [¶](#truncf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/truncf.html>

Gnulib module: truncf or truncf-ieee

Portability problems fixed by either Gnulib module `truncf` or `truncf-ieee`:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, MSVC 9.
* This function is not declared (without `-D_GNU_SOURCE`) on some platforms:
  glibc 2.8.

Portability problems fixed by Gnulib module `truncf-ieee`:

Portability problems not fixed by Gnulib:

---

Next: [`ufromfp`](#ufromfp), Previous: [`truncf`](#truncf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.326 `truncl` [¶](#truncl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/truncl.html>

Gnulib module: truncl or truncl-ieee

Portability problems fixed by either Gnulib module `truncl` or `truncl-ieee`:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, MSVC 9.
* This function is not declared (without `-D_GNU_SOURCE`) on some platforms:
  glibc 2.8.

Portability problems fixed by Gnulib module `truncl-ieee`:

Portability problems not fixed by Gnulib:

---

Next: [`ufromfpf`](#ufromfpf), Previous: [`truncl`](#truncl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.327 `ufromfp` [¶](#ufromfp-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Rounding-Functions.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ufromfpl`](#ufromfpl), Previous: [`ufromfp`](#ufromfp), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.328 `ufromfpf` [¶](#ufromfpf-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Rounding-Functions.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ufromfpx`](#ufromfpx), Previous: [`ufromfpf`](#ufromfpf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.329 `ufromfpl` [¶](#ufromfpl-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Rounding-Functions.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ufromfpxf`](#ufromfpxf), Previous: [`ufromfpl`](#ufromfpl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.330 `ufromfpx` [¶](#ufromfpx-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Rounding-Functions.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ufromfpxl`](#ufromfpxl), Previous: [`ufromfpx`](#ufromfpx), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.331 `ufromfpxf` [¶](#ufromfpxf-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Rounding-Functions.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`y0`](#y0), Previous: [`ufromfpxf`](#ufromfpxf), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.332 `ufromfpxl` [¶](#ufromfpxl-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Rounding-Functions.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`y1`](#y1), Previous: [`ufromfpxl`](#ufromfpxl), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.333 `y0` [¶](#y0-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/y0.html>

Gnulib module: y0

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.

---

Next: [`yn`](#yn), Previous: [`y0`](#y0), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.334 `y1` [¶](#y1-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/y1.html>

Gnulib module: y1

Portability problems fixed by Gnulib:
