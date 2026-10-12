### 14.30 Glibc Extensions to `<math.h>` [¶](#Glibc-Extensions-to-_003cmath_002eh_003e)

* [`drem`](#drem)
* [`dremf`](#dremf)
* [`dreml`](#dreml)
* [`finite`](#finite)
* [`finitef`](#finitef)
* [`finitel`](#finitel)
* [`gamma`](#gamma)
* [`gammaf`](#gammaf)
* [`gammal`](#gammal)
* [`isinff`](#isinff)
* [`isinfl`](#isinfl)
* [`isnanf`](#isnanf)
* [`isnanl`](#isnanl)
* [`j0f`](#j0f)
* [`j0l`](#j0l)
* [`j1f`](#j1f)
* [`j1l`](#j1l)
* [`jnf`](#jnf)
* [`jnl`](#jnl)
* [`lgamma_r`](#lgamma_005fr)
* [`lgammaf_r`](#lgammaf_005fr)
* [`lgammal_r`](#lgammal_005fr)
* [`matherr`](#matherr)
* [`pow10`](#pow10)
* [`pow10f`](#pow10f)
* [`pow10l`](#pow10l)
* [`scalbf`](#scalbf)
* [`scalbl`](#scalbl)
* [`significand`](#significand)
* [`significandf`](#significandf)
* [`significandl`](#significandl)
* [`sincos`](#sincos)
* [`sincosf`](#sincosf)
* [`sincosl`](#sincosl)
* [`y0f`](#y0f)
* [`y0l`](#y0l)
* [`y1f`](#y1f)
* [`y1l`](#y1l)
* [`ynf`](#ynf)
* [`ynl`](#ynl)

---

Next: [`dremf`](#dremf), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.1 `drem` [¶](#drem-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-drem.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Remainder-Functions.html>,
* [man drem](https://www.kernel.org/doc/man-pages/online/pages/man3/drem.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11, Solaris 11.4, mingw, MSVC 14.

---

Next: [`dreml`](#dreml), Previous: [`drem`](#drem), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.2 `dremf` [¶](#dremf-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-dremf.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Remainder-Functions.html>,
* [man dremf](https://www.kernel.org/doc/man-pages/online/pages/man3/dremf.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14.

---

Next: [`finite`](#finite), Previous: [`dremf`](#dremf), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.3 `dreml` [¶](#dreml-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-dreml.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Remainder-Functions.html>,
* [man dreml](https://www.kernel.org/doc/man-pages/online/pages/man3/dreml.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.

---

Next: [`finitef`](#finitef), Previous: [`dreml`](#dreml), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.4 `finite` [¶](#finite-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-finite.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Floating-Point-Classes.html>,
* [man finite](https://www.kernel.org/doc/man-pages/online/pages/man3/finite.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11, MSVC 14.

---

Next: [`finitel`](#finitel), Previous: [`finite`](#finite), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.5 `finitef` [¶](#finitef-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-finitef.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Floating-Point-Classes.html>,
* [man finitef](https://www.kernel.org/doc/man-pages/online/pages/man3/finitef.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14.

---

Next: [`gamma`](#gamma), Previous: [`finitef`](#finitef), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.6 `finitel` [¶](#finitel-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-finitel.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Floating-Point-Classes.html>,
* [man finitel](https://www.kernel.org/doc/man-pages/online/pages/man3/finitel.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.

---

Next: [`gammaf`](#gammaf), Previous: [`finitel`](#finitel), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.7 `gamma` [¶](#gamma-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-gamma.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man gamma](https://www.kernel.org/doc/man-pages/online/pages/man3/gamma.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`gammal`](#gammal), Previous: [`gamma`](#gamma), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.8 `gammaf` [¶](#gammaf-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-gammaf.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man gammaf](https://www.kernel.org/doc/man-pages/online/pages/man3/gammaf.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Next: [`isinff`](#isinff), Previous: [`gammaf`](#gammaf), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.9 `gammal` [¶](#gammal-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-gammal.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man gammal](https://www.kernel.org/doc/man-pages/online/pages/man3/gammal.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`isinfl`](#isinfl), Previous: [`gammal`](#gammal), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.10 `isinff` [¶](#isinff-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Floating-Point-Classes.html>,
* [man isinff](https://www.kernel.org/doc/man-pages/online/pages/man3/isinff.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 4.4.
* This function is not declared on some platforms:
  Android 9.0.

---

Next: [`isnanf`](#isnanf), Previous: [`isinff`](#isinff), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.11 `isinfl` [¶](#isinfl-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Floating-Point-Classes.html>,
* [man isinfl](https://www.kernel.org/doc/man-pages/online/pages/man3/isinfl.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* This function is not declared on some platforms:
  Android 9.0.

---

Next: [`isnanl`](#isnanl), Previous: [`isinfl`](#isinfl), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.12 `isnanf` [¶](#isnanf-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Floating-Point-Classes.html>,
* [man isnanf](https://www.kernel.org/doc/man-pages/online/pages/man3/isnanf.3.html).

Gnulib module: isnanf

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, Minix 3.1.8, AIX 5.1, HP-UX 11, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`j0f`](#j0f), Previous: [`isnanf`](#isnanf), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.13 `isnanl` [¶](#isnanl-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Floating-Point-Classes.html>,
* [man isnanl](https://www.kernel.org/doc/man-pages/online/pages/man3/isnanl.3.html).

Gnulib module: isnanl

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 1.7.x, MSVC 14, Android 4.4.
* This function is not declared on some platforms:
  Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`j0l`](#j0l), Previous: [`isnanl`](#isnanl), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.14 `j0f` [¶](#j0f-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-j0f.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man j0f](https://www.kernel.org/doc/man-pages/online/pages/man3/j0f.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Next: [`j1f`](#j1f), Previous: [`j0f`](#j0f), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.15 `j0l` [¶](#j0l-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-j0l.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man j0l](https://www.kernel.org/doc/man-pages/online/pages/man3/j0l.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`j1l`](#j1l), Previous: [`j0l`](#j0l), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.16 `j1f` [¶](#j1f-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-j1f.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man j1f](https://www.kernel.org/doc/man-pages/online/pages/man3/j1f.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Next: [`jnf`](#jnf), Previous: [`j1f`](#j1f), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.17 `j1l` [¶](#j1l-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-j1l.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man j1l](https://www.kernel.org/doc/man-pages/online/pages/man3/j1l.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`jnl`](#jnl), Previous: [`j1l`](#j1l), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.18 `jnf` [¶](#jnf-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-jnf.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man jnf](https://www.kernel.org/doc/man-pages/online/pages/man3/jnf.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Next: [`lgamma_r`](#lgamma_005fr), Previous: [`jnf`](#jnf), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.19 `jnl` [¶](#jnl-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-jnl.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man jnl](https://www.kernel.org/doc/man-pages/online/pages/man3/jnl.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`lgammaf_r`](#lgammaf_005fr), Previous: [`jnl`](#jnl), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.20 `lgamma_r` [¶](#lgamma_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-lgamma-r.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man lgamma\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/lgamma_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5 x86, Minix 3.1.8, mingw, MSVC 14.

---

Next: [`lgammal_r`](#lgammal_005fr), Previous: [`lgamma_r`](#lgamma_005fr), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.21 `lgammaf_r` [¶](#lgammaf_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-lgammaf-r.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man lgammaf\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/lgammaf_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Next: [`matherr`](#matherr), Previous: [`lgammaf_r`](#lgammaf_005fr), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.22 `lgammal_r` [¶](#lgammal_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-lgammal-r.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man lgammal\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/lgammal_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 1.7.x, mingw, MSVC 14, Android 5.1.

---

Next: [`pow10`](#pow10), Previous: [`lgammal_r`](#lgammal_005fr), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.23 `matherr` [¶](#matherr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-matherr-1.html>

Documentation:  
 [man matherr](https://www.kernel.org/doc/man-pages/online/pages/man3/matherr.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14, Android 9.0.

---

Next: [`pow10f`](#pow10f), Previous: [`matherr`](#matherr), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.24 `pow10` [¶](#pow10-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-pow10.html>

Documentation:  
 [man pow10](https://www.kernel.org/doc/man-pages/online/pages/man3/pow10.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`pow10l`](#pow10l), Previous: [`pow10`](#pow10), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.25 `pow10f` [¶](#pow10f-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-pow10f.html>

Documentation:  
 [man pow10f](https://www.kernel.org/doc/man-pages/online/pages/man3/pow10f.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`scalbf`](#scalbf), Previous: [`pow10f`](#pow10f), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.26 `pow10l` [¶](#pow10l-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-pow10l.html>

Documentation:  
 [man pow10l](https://www.kernel.org/doc/man-pages/online/pages/man3/pow10l.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.

---

Next: [`scalbl`](#scalbl), Previous: [`pow10l`](#pow10l), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.27 `scalbf` [¶](#scalbf-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-scalbf.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Normalization-Functions.html>,
* [man scalbf](https://www.kernel.org/doc/man-pages/online/pages/man3/scalbf.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Next: [`significand`](#significand), Previous: [`scalbf`](#scalbf), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.28 `scalbl` [¶](#scalbl-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-scalbl.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Normalization-Functions.html>,
* [man scalbl](https://www.kernel.org/doc/man-pages/online/pages/man3/scalbl.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.

---

Next: [`significandf`](#significandf), Previous: [`scalbl`](#scalbl), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.29 `significand` [¶](#significand-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-significand.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Normalization-Functions.html>,
* [man significand](https://www.kernel.org/doc/man-pages/online/pages/man3/significand.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Next: [`significandl`](#significandl), Previous: [`significand`](#significand), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.30 `significandf` [¶](#significandf-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-significandf.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Normalization-Functions.html>,
* [man significandf](https://www.kernel.org/doc/man-pages/online/pages/man3/significandf.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Next: [`sincos`](#sincos), Previous: [`significandf`](#significandf), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.31 `significandl` [¶](#significandl-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-significandl.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Normalization-Functions.html>,
* [man significandl](https://www.kernel.org/doc/man-pages/online/pages/man3/significandl.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`sincosf`](#sincosf), Previous: [`significandl`](#significandl), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.32 `sincos` [¶](#sincos-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-sincos.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Trig-Functions.html>,
* [man sincos](https://www.kernel.org/doc/man-pages/online/pages/man3/sincos.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 11.0, NetBSD 9.3, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Next: [`sincosl`](#sincosl), Previous: [`sincos`](#sincos), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.33 `sincosf` [¶](#sincosf-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-sincosf.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Trig-Functions.html>,
* [man sincosf](https://www.kernel.org/doc/man-pages/online/pages/man3/sincosf.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 11.0, NetBSD 9.3, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Next: [`y0f`](#y0f), Previous: [`sincosf`](#sincosf), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.34 `sincosl` [¶](#sincosl-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-sincosl.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Trig-Functions.html>,
* [man sincosl](https://www.kernel.org/doc/man-pages/online/pages/man3/sincosl.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 11.0, NetBSD 9.3, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 1.7.x, mingw, MSVC 14.

---

Next: [`y0l`](#y0l), Previous: [`sincosl`](#sincosl), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.35 `y0f` [¶](#y0f-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-y0f.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man y0f](https://www.kernel.org/doc/man-pages/online/pages/man3/y0f.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Next: [`y1f`](#y1f), Previous: [`y0f`](#y0f), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.36 `y0l` [¶](#y0l-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-y0l.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man y0l](https://www.kernel.org/doc/man-pages/online/pages/man3/y0l.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`y1l`](#y1l), Previous: [`y0l`](#y0l), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.37 `y1f` [¶](#y1f-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-y1f.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man y1f](https://www.kernel.org/doc/man-pages/online/pages/man3/y1f.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Next: [`ynf`](#ynf), Previous: [`y1f`](#y1f), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.38 `y1l` [¶](#y1l-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-y1l.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man y1l](https://www.kernel.org/doc/man-pages/online/pages/man3/y1l.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ynl`](#ynl), Previous: [`y1l`](#y1l), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.39 `ynf` [¶](#ynf-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-ynf.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man ynf](https://www.kernel.org/doc/man-pages/online/pages/man3/ynf.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Previous: [`ynf`](#ynf), Up: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.30.40 `ynl` [¶](#ynl-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-ynl.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Special-Functions.html>,
* [man ynl](https://www.kernel.org/doc/man-pages/online/pages/man3/ynl.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<mntent.h>`](#Glibc-mntent_002eh), Previous: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.31 Glibc `<mcheck.h>` [¶](#Glibc-_003cmcheck_002eh_003e)

* [`mcheck`](#mcheck)
* [`mcheck_check_all`](#mcheck_005fcheck_005fall)
* [`mcheck_pedantic`](#mcheck_005fpedantic)
* [`mprobe`](#mprobe)
* [`mtrace`](#mtrace)
* [`muntrace`](#muntrace)

---

Next: [`mcheck_check_all`](#mcheck_005fcheck_005fall), Up: [Glibc `<mcheck.h>`](#Glibc-mcheck_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.31.1 `mcheck` [¶](#mcheck-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Heap-Consistency-Checking.html>,
* [man mcheck](https://www.kernel.org/doc/man-pages/online/pages/man3/mcheck.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`mcheck_pedantic`](#mcheck_005fpedantic), Previous: [`mcheck`](#mcheck), Up: [Glibc `<mcheck.h>`](#Glibc-mcheck_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.31.2 `mcheck_check_all` [¶](#mcheck_005fcheck_005fall-1)

Documentation:  
 [man mcheck\_check\_all](https://www.kernel.org/doc/man-pages/online/pages/man3/mcheck_check_all.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`mprobe`](#mprobe), Previous: [`mcheck_check_all`](#mcheck_005fcheck_005fall), Up: [Glibc `<mcheck.h>`](#Glibc-mcheck_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.31.3 `mcheck_pedantic` [¶](#mcheck_005fpedantic-1)

Documentation:  
 [man mcheck\_pedantic](https://www.kernel.org/doc/man-pages/online/pages/man3/mcheck_pedantic.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`mtrace`](#mtrace), Previous: [`mcheck_pedantic`](#mcheck_005fpedantic), Up: [Glibc `<mcheck.h>`](#Glibc-mcheck_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.31.4 `mprobe` [¶](#mprobe-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Heap-Consistency-Checking.html>,
* [man mprobe](https://www.kernel.org/doc/man-pages/online/pages/man3/mprobe.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`muntrace`](#muntrace), Previous: [`mprobe`](#mprobe), Up: [Glibc `<mcheck.h>`](#Glibc-mcheck_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.31.5 `mtrace` [¶](#mtrace-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Tracing-malloc.html>,
* [man mtrace](https://www.kernel.org/doc/man-pages/online/pages/man3/mtrace.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`mtrace`](#mtrace), Up: [Glibc `<mcheck.h>`](#Glibc-mcheck_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.31.6 `muntrace` [¶](#muntrace-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Tracing-malloc.html>,
* [man muntrace](https://www.kernel.org/doc/man-pages/online/pages/man3/muntrace.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh), Previous: [Glibc `<mcheck.h>`](#Glibc-mcheck_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.32 Glibc `<mntent.h>` [¶](#Glibc-_003cmntent_002eh_003e)

* [`addmntent`](#addmntent)
* [`endmntent`](#endmntent)
* [`getmntent`](#getmntent)
* [`getmntent_r`](#getmntent_005fr)
* [`hasmntopt`](#hasmntopt)
* [`setmntent`](#setmntent)

---

Next: [`endmntent`](#endmntent), Up: [Glibc `<mntent.h>`](#Glibc-mntent_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.32.1 `addmntent` [¶](#addmntent-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/mtab.html>,
* [man addmntent](https://www.kernel.org/doc/man-pages/online/pages/man3/addmntent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getmntent`](#getmntent), Previous: [`addmntent`](#addmntent), Up: [Glibc `<mntent.h>`](#Glibc-mntent_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.32.2 `endmntent` [¶](#endmntent-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/mtab.html>,
* [man endmntent](https://www.kernel.org/doc/man-pages/online/pages/man3/endmntent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Solaris 11.4, mingw, MSVC 14, Android API level 20.

---

Next: [`getmntent_r`](#getmntent_005fr), Previous: [`endmntent`](#endmntent), Up: [Glibc `<mntent.h>`](#Glibc-mntent_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.32.3 `getmntent` [¶](#getmntent-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/mtab.html>,
* [man getmntent](https://www.kernel.org/doc/man-pages/online/pages/man3/getmntent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.

---

Next: [`hasmntopt`](#hasmntopt), Previous: [`getmntent`](#getmntent), Up: [Glibc `<mntent.h>`](#Glibc-mntent_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.32.4 `getmntent_r` [¶](#getmntent_005fr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/mtab.html>,
* [man getmntent\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getmntent_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android API level 20.

---

Next: [`setmntent`](#setmntent), Previous: [`getmntent_r`](#getmntent_005fr), Up: [Glibc `<mntent.h>`](#Glibc-mntent_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.32.5 `hasmntopt` [¶](#hasmntopt-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/mtab.html>,
* [man hasmntopt](https://www.kernel.org/doc/man-pages/online/pages/man3/hasmntopt.3.html).

Gnulib module: hasmntopt

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Cygwin 3.5.3, Android API level 25.
* This function returns wrong results on some platforms:
  musl libc 1.2.5.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.

---

Previous: [`hasmntopt`](#hasmntopt), Up: [Glibc `<mntent.h>`](#Glibc-mntent_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.32.6 `setmntent` [¶](#setmntent-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/mtab.html>,
* [man setmntent](https://www.kernel.org/doc/man-pages/online/pages/man3/setmntent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Solaris 11.4, mingw, MSVC 14, Android API level 20.

---

Next: [Glibc `<netinet/ether.h>`](#Glibc-netinet_002fether_002eh), Previous: [Glibc `<mntent.h>`](#Glibc-mntent_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.33 Glibc Extensions to `<netdb.h>` [¶](#Glibc-Extensions-to-_003cnetdb_002eh_003e)

* [`endnetgrent`](#endnetgrent)
* [`gethostbyaddr_r`](#gethostbyaddr_005fr)
* [`gethostbyname2`](#gethostbyname2)
* [`gethostbyname2_r`](#gethostbyname2_005fr)
* [`gethostbyname_r`](#gethostbyname_005fr)
* [`gethostent_r`](#gethostent_005fr)
* [`getnetbyaddr_r`](#getnetbyaddr_005fr)
* [`getnetbyname_r`](#getnetbyname_005fr)
* [`getnetent_r`](#getnetent_005fr)
* [`getnetgrent`](#getnetgrent)
* [`getnetgrent_r`](#getnetgrent_005fr)
* [`getprotobyname_r`](#getprotobyname_005fr)
* [`getprotobynumber_r`](#getprotobynumber_005fr)
* [`getprotoent_r`](#getprotoent_005fr)
* [`getservbyname_r`](#getservbyname_005fr)
* [`getservbyport_r`](#getservbyport_005fr)
* [`getservent_r`](#getservent_005fr)
* [`herror`](#herror)
* [`hstrerror`](#hstrerror)
* [`innetgr`](#innetgr)
* [`rcmd`](#rcmd)
* [`rcmd_af`](#rcmd_005faf)
* [`rexec`](#rexec)
* [`rexec_af`](#rexec_005faf)
* [`rresvport`](#rresvport)
* [`rresvport_af`](#rresvport_005faf)
* [`ruserok`](#ruserok)
* [`ruserok_af`](#ruserok_005faf)
* [`setnetgrent`](#setnetgrent)

---

Next: [`gethostbyaddr_r`](#gethostbyaddr_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.1 `endnetgrent` [¶](#endnetgrent-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Lookup-Netgroup.html>,
* [man endnetgrent](https://www.kernel.org/doc/man-pages/online/pages/man3/endnetgrent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`gethostbyname2`](#gethostbyname2), Previous: [`endnetgrent`](#endnetgrent), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.2 `gethostbyaddr_r` [¶](#gethostbyaddr_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-gethostbyaddr-r-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Host-Names.html>,
* [man gethostbyaddr\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/gethostbyaddr_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 5.1.

---

Next: [`gethostbyname2_r`](#gethostbyname2_005fr), Previous: [`gethostbyaddr_r`](#gethostbyaddr_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.3 `gethostbyname2` [¶](#gethostbyname2-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-gethostbyname2-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Host-Names.html>,
* [man gethostbyname2](https://www.kernel.org/doc/man-pages/online/pages/man3/gethostbyname2.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14.

---

Next: [`gethostbyname_r`](#gethostbyname_005fr), Previous: [`gethostbyname2`](#gethostbyname2), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.4 `gethostbyname2_r` [¶](#gethostbyname2_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-gethostbyname2-r-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Host-Names.html>,
* [man gethostbyname2\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/gethostbyname2_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 5.1.

---

Next: [`gethostent_r`](#gethostent_005fr), Previous: [`gethostbyname2_r`](#gethostbyname2_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.5 `gethostbyname_r` [¶](#gethostbyname_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-gethostbyname-r-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Host-Names.html>,
* [man gethostbyname\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/gethostbyname_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`getnetbyaddr_r`](#getnetbyaddr_005fr), Previous: [`gethostbyname_r`](#gethostbyname_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.6 `gethostent_r` [¶](#gethostent_005fr-1)

Documentation:  
 [man gethostent\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/gethostent_r.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getnetbyname_r`](#getnetbyname_005fr), Previous: [`gethostent_r`](#gethostent_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.7 `getnetbyaddr_r` [¶](#getnetbyaddr_005fr-1)

Documentation:  
 [man getnetbyaddr\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getnetbyaddr_r.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getnetent_r`](#getnetent_005fr), Previous: [`getnetbyaddr_r`](#getnetbyaddr_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.8 `getnetbyname_r` [¶](#getnetbyname_005fr-1)

Documentation:  
 [man getnetbyname\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getnetbyname_r.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getnetgrent`](#getnetgrent), Previous: [`getnetbyname_r`](#getnetbyname_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.9 `getnetent_r` [¶](#getnetent_005fr-1)

Documentation:  
 [man getnetent\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getnetent_r.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getnetgrent_r`](#getnetgrent_005fr), Previous: [`getnetent_r`](#getnetent_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.10 `getnetgrent` [¶](#getnetgrent-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Lookup-Netgroup.html>,
* [man getnetgrent](https://www.kernel.org/doc/man-pages/online/pages/man3/getnetgrent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getprotobyname_r`](#getprotobyname_005fr), Previous: [`getnetgrent`](#getnetgrent), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.11 `getnetgrent_r` [¶](#getnetgrent_005fr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Lookup-Netgroup.html>,
* [man getnetgrent\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getnetgrent_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getprotobynumber_r`](#getprotobynumber_005fr), Previous: [`getnetgrent_r`](#getnetgrent_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.12 `getprotobyname_r` [¶](#getprotobyname_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getprotobyname-r.html>

Documentation:  
 [man getprotobyname\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getprotobyname_r.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getprotoent_r`](#getprotoent_005fr), Previous: [`getprotobyname_r`](#getprotobyname_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.13 `getprotobynumber_r` [¶](#getprotobynumber_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getprotobynumber-r.html>

Documentation:  
 [man getprotobynumber\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getprotobynumber_r.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getservbyname_r`](#getservbyname_005fr), Previous: [`getprotobynumber_r`](#getprotobynumber_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.14 `getprotoent_r` [¶](#getprotoent_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getprotoent-r.html>

Documentation:  
 [man getprotoent\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getprotoent_r.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getservbyport_r`](#getservbyport_005fr), Previous: [`getprotoent_r`](#getprotoent_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.15 `getservbyname_r` [¶](#getservbyname_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getservbyname-r.html>

Documentation:  
 [man getservbyname\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getservbyname_r.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getservent_r`](#getservent_005fr), Previous: [`getservbyname_r`](#getservbyname_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.16 `getservbyport_r` [¶](#getservbyport_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getservbyport-r.html>

Documentation:  
 [man getservbyport\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getservbyport_r.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`herror`](#herror), Previous: [`getservbyport_r`](#getservbyport_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.17 `getservent_r` [¶](#getservent_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getservent-r.html>

Documentation:  
 [man getservent\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getservent_r.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`hstrerror`](#hstrerror), Previous: [`getservent_r`](#getservent_005fr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.18 `herror` [¶](#herror-1)

Documentation:  
 [man herror](https://www.kernel.org/doc/man-pages/online/pages/man3/herror.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`innetgr`](#innetgr), Previous: [`herror`](#herror), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.19 `hstrerror` [¶](#hstrerror-1)

Documentation:  
 [man hstrerror](https://www.kernel.org/doc/man-pages/online/pages/man3/hstrerror.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11, mingw, MSVC 14.

---

Next: [`rcmd`](#rcmd), Previous: [`hstrerror`](#hstrerror), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.20 `innetgr` [¶](#innetgr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Netgroup-Membership.html>,
* [man innetgr](https://www.kernel.org/doc/man-pages/online/pages/man3/innetgr.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`rcmd_af`](#rcmd_005faf), Previous: [`innetgr`](#innetgr), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.21 `rcmd` [¶](#rcmd-1)

Documentation:  
 [man rcmd](https://www.kernel.org/doc/man-pages/online/pages/man3/rcmd.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 9.0.

---

Next: [`rexec`](#rexec), Previous: [`rcmd`](#rcmd), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.22 `rcmd_af` [¶](#rcmd_005faf-1)

Documentation:  
 [man rcmd\_af](https://www.kernel.org/doc/man-pages/online/pages/man3/rcmd_af.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11.11, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`rexec_af`](#rexec_005faf), Previous: [`rcmd_af`](#rcmd_005faf), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.23 `rexec` [¶](#rexec-1)

Documentation:  
 [man rexec](https://www.kernel.org/doc/man-pages/online/pages/man3/rexec.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`rresvport`](#rresvport), Previous: [`rexec`](#rexec), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.24 `rexec_af` [¶](#rexec_005faf-1)

Documentation:  
 [man rexec\_af](https://www.kernel.org/doc/man-pages/online/pages/man3/rexec_af.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11.11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`rresvport_af`](#rresvport_005faf), Previous: [`rexec_af`](#rexec_005faf), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.25 `rresvport` [¶](#rresvport-1)

Documentation:  
 [man rresvport](https://www.kernel.org/doc/man-pages/online/pages/man3/rresvport.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`ruserok`](#ruserok), Previous: [`rresvport`](#rresvport), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.26 `rresvport_af` [¶](#rresvport_005faf-1)

Documentation:  
 [man rresvport\_af](https://www.kernel.org/doc/man-pages/online/pages/man3/rresvport_af.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11.11, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`ruserok_af`](#ruserok_005faf), Previous: [`rresvport_af`](#rresvport_005faf), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.27 `ruserok` [¶](#ruserok-1)

Documentation:  
 [man ruserok](https://www.kernel.org/doc/man-pages/online/pages/man3/ruserok.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`setnetgrent`](#setnetgrent), Previous: [`ruserok`](#ruserok), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.28 `ruserok_af` [¶](#ruserok_005faf-1)

Documentation:  
 [man ruserok\_af](https://www.kernel.org/doc/man-pages/online/pages/man3/ruserok_af.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`ruserok_af`](#ruserok_005faf), Up: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.33.29 `setnetgrent` [¶](#setnetgrent-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Lookup-Netgroup.html>,
* [man setnetgrent](https://www.kernel.org/doc/man-pages/online/pages/man3/setnetgrent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh), Previous: [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.34 Glibc `<netinet/ether.h>` [¶](#Glibc-_003cnetinet_002fether_002eh_003e)

* [`ether_aton`](#ether_005faton)
* [`ether_aton_r`](#ether_005faton_005fr)
* [`ether_hostton`](#ether_005fhostton)
* [`ether_line`](#ether_005fline)
* [`ether_ntoa`](#ether_005fntoa)
* [`ether_ntoa_r`](#ether_005fntoa_005fr)
* [`ether_ntohost`](#ether_005fntohost)

---

Next: [`ether_aton_r`](#ether_005faton_005fr), Up: [Glibc `<netinet/ether.h>`](#Glibc-netinet_002fether_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.34.1 `ether_aton` [¶](#ether_005faton-1)

Documentation:  
 [man ether\_aton](https://www.kernel.org/doc/man-pages/online/pages/man3/ether_aton.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 2.3.

---

Next: [`ether_hostton`](#ether_005fhostton), Previous: [`ether_aton`](#ether_005faton), Up: [Glibc `<netinet/ether.h>`](#Glibc-netinet_002fether_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.34.2 `ether_aton_r` [¶](#ether_005faton_005fr-1)

Documentation:  
 [man ether\_aton\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/ether_aton_r.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 2.3.

---

Next: [`ether_line`](#ether_005fline), Previous: [`ether_aton_r`](#ether_005faton_005fr), Up: [Glibc `<netinet/ether.h>`](#Glibc-netinet_002fether_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.34.3 `ether_hostton` [¶](#ether_005fhostton-1)

Documentation:  
 [man ether\_hostton](https://www.kernel.org/doc/man-pages/online/pages/man3/ether_hostton.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ether_ntoa`](#ether_005fntoa), Previous: [`ether_hostton`](#ether_005fhostton), Up: [Glibc `<netinet/ether.h>`](#Glibc-netinet_002fether_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.34.4 `ether_line` [¶](#ether_005fline-1)

Documentation:  
 [man ether\_line](https://www.kernel.org/doc/man-pages/online/pages/man3/ether_line.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ether_ntoa_r`](#ether_005fntoa_005fr), Previous: [`ether_line`](#ether_005fline), Up: [Glibc `<netinet/ether.h>`](#Glibc-netinet_002fether_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.34.5 `ether_ntoa` [¶](#ether_005fntoa-1)

Documentation:  
 [man ether\_ntoa](https://www.kernel.org/doc/man-pages/online/pages/man3/ether_ntoa.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 2.3.

---

Next: [`ether_ntohost`](#ether_005fntohost), Previous: [`ether_ntoa`](#ether_005fntoa), Up: [Glibc `<netinet/ether.h>`](#Glibc-netinet_002fether_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.34.6 `ether_ntoa_r` [¶](#ether_005fntoa_005fr-1)

Documentation:  
 [man ether\_ntoa\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/ether_ntoa_r.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 2.3.

---

Previous: [`ether_ntoa_r`](#ether_005fntoa_005fr), Up: [Glibc `<netinet/ether.h>`](#Glibc-netinet_002fether_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.34.7 `ether_ntohost` [¶](#ether_005fntohost-1)

Documentation:  
 [man ether\_ntohost](https://www.kernel.org/doc/man-pages/online/pages/man3/ether_ntohost.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<obstack.h>`](#Glibc-obstack_002eh), Previous: [Glibc `<netinet/ether.h>`](#Glibc-netinet_002fether_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
