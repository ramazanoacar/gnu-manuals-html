### 14.67 Glibc Extensions to `<stdlib.h>` [¶](#Glibc-Extensions-to-_003cstdlib_002eh_003e)

* [`arc4random`](#arc4random)
* [`arc4random_buf`](#arc4random_005fbuf)
* [`arc4random_uniform`](#arc4random_005funiform)
* [`canonicalize_file_name`](#canonicalize_005ffile_005fname)
* [`cfree`](#cfree)
* [`clearenv`](#clearenv)
* [`drand48_r`](#drand48_005fr)
* [`ecvt_r`](#ecvt_005fr)
* [`erand48_r`](#erand48_005fr)
* [`fcvt_r`](#fcvt_005fr)
* [`getloadavg`](#getloadavg)
* [`getpt`](#getpt)
* [`initstate_r`](#initstate_005fr)
* [`jrand48_r`](#jrand48_005fr)
* [`lcong48_r`](#lcong48_005fr)
* [`lrand48_r`](#lrand48_005fr)
* [`mkostemps`](#mkostemps)
* [`mkstemps`](#mkstemps)
* [`mrand48_r`](#mrand48_005fr)
* [`nrand48_r`](#nrand48_005fr)
* [`on_exit`](#on_005fexit)
* [`qecvt`](#qecvt)
* [`qecvt_r`](#qecvt_005fr)
* [`qfcvt`](#qfcvt)
* [`qfcvt_r`](#qfcvt_005fr)
* [`qgcvt`](#qgcvt)
* [`random_r`](#random_005fr)
* [`rpmatch`](#rpmatch)
* [`seed48_r`](#seed48_005fr)
* [`setstate_r`](#setstate_005fr)
* [`srand48_r`](#srand48_005fr)
* [`srandom_r`](#srandom_005fr)
* [`strtod_l`](#strtod_005fl)
* [`strtof_l`](#strtof_005fl)
* [`strtol_l`](#strtol_005fl)
* [`strtold_l`](#strtold_005fl)
* [`strtoll_l`](#strtoll_005fl)
* [`strtoq`](#strtoq)
* [`strtoul_l`](#strtoul_005fl)
* [`strtoull_l`](#strtoull_005fl)
* [`strtouq`](#strtouq)
* [`uabs`](#uabs)
* [`ulabs`](#ulabs)
* [`ullabs`](#ullabs)
* [`valloc`](#valloc)

---

Next: [`arc4random_buf`](#arc4random_005fbuf), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.1 `arc4random` [¶](#arc4random-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/High-Quality-Random.html>,
* [man arc4random](https://www.kernel.org/doc/man-pages/online/pages/man3/arc4random.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.35, AIX 7.3.1, HP-UX 11.31, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14.

---

Next: [`arc4random_uniform`](#arc4random_005funiform), Previous: [`arc4random`](#arc4random), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.2 `arc4random_buf` [¶](#arc4random_005fbuf-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/High-Quality-Random.html>,
* [man arc4random](https://www.kernel.org/doc/man-pages/online/pages/man3/arc4random.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.35, AIX 7.3.1, HP-UX 11.31, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14.

---

Next: [`canonicalize_file_name`](#canonicalize_005ffile_005fname), Previous: [`arc4random_buf`](#arc4random_005fbuf), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.3 `arc4random_uniform` [¶](#arc4random_005funiform-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/High-Quality-Random.html>,
* [man arc4random](https://www.kernel.org/doc/man-pages/online/pages/man3/arc4random.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.35, AIX 7.3.1, HP-UX 11.31, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14.

---

Next: [`cfree`](#cfree), Previous: [`arc4random_uniform`](#arc4random_005funiform), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.4 `canonicalize_file_name` [¶](#canonicalize_005ffile_005fname-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Symbolic-Links.html>,
* [man canonicalize\_file\_name](https://www.kernel.org/doc/man-pages/online/pages/man3/canonicalize_file_name.3.html).

Gnulib module: canonicalize-lgpl

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.
* This function fails to detect trailing slashes on non-directories on
  some platforms:
  glibc 2.3.5.

Portability problems not fixed by Gnulib:

---

Next: [`clearenv`](#clearenv), Previous: [`canonicalize_file_name`](#canonicalize_005ffile_005fname), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.5 `cfree` [¶](#cfree-1)

Documentation:  
 [man cfree](https://www.kernel.org/doc/man-pages/online/pages/man3/cfree.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`drand48_r`](#drand48_005fr), Previous: [`cfree`](#cfree), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.6 `clearenv` [¶](#clearenv-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Environment-Access.html>,
* [man clearenv](https://www.kernel.org/doc/man-pages/online/pages/man3/clearenv.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Solaris 10, Cygwin 2.10.x, mingw, MSVC 14.

---

Next: [`ecvt_r`](#ecvt_005fr), Previous: [`clearenv`](#clearenv), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.7 `drand48_r` [¶](#drand48_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-drand48-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/SVID-Random.html>,
* [man drand48\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/drand48_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.3, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`erand48_r`](#erand48_005fr), Previous: [`drand48_r`](#drand48_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.8 `ecvt_r` [¶](#ecvt_005fr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/System-V-Number-Conversion.html>,
* [man ecvt\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/ecvt_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`fcvt_r`](#fcvt_005fr), Previous: [`ecvt_r`](#ecvt_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.9 `erand48_r` [¶](#erand48_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-erand48-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/SVID-Random.html>,
* [man erand48\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/erand48_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.3, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getloadavg`](#getloadavg), Previous: [`erand48_r`](#erand48_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.10 `fcvt_r` [¶](#fcvt_005fr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/System-V-Number-Conversion.html>,
* [man fcvt\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/fcvt_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getpt`](#getpt), Previous: [`fcvt_r`](#fcvt_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.11 `getloadavg` [¶](#getloadavg-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getloadavg-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Processor-Resources.html>,
* [man getloadavg](https://www.kernel.org/doc/man-pages/online/pages/man3/getloadavg.3.html).

Gnulib module: getloadavg

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.
* This function is declared in `<sys/loadavg.h>`, not `<stdlib.h>`,
  on some platforms:
  Solaris 11.4.

Portability problems not fixed by Gnulib:

---

Next: [`initstate_r`](#initstate_005fr), Previous: [`getloadavg`](#getloadavg), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.12 `getpt` [¶](#getpt-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Allocation.html>,
* [man getpt](https://www.kernel.org/doc/man-pages/online/pages/man3/getpt.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.9, mingw, MSVC 14.

---

Next: [`jrand48_r`](#jrand48_005fr), Previous: [`getpt`](#getpt), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.13 `initstate_r` [¶](#initstate_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-initstate-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/BSD-Random.html>,
* [man initstate\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/initstate_r.3.html).

Gnulib module: random\_r

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function has an incompatible declaration on some platforms:
  AIX 7.1.

Portability problems not fixed by Gnulib:

* This function crashes if the state buffer is unaligned on some platforms:
  glibc 2.40/sparc.
* This function has a slightly different (but compatible) declaration on some
  platforms:
  Haiku.

---

Next: [`lcong48_r`](#lcong48_005fr), Previous: [`initstate_r`](#initstate_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.14 `jrand48_r` [¶](#jrand48_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-jrand48-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/SVID-Random.html>,
* [man jrand48\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/jrand48_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.3, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`lrand48_r`](#lrand48_005fr), Previous: [`jrand48_r`](#jrand48_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.15 `lcong48_r` [¶](#lcong48_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-lcong48-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/SVID-Random.html>,
* [man lcong48\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/lcong48_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.3, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`mkostemps`](#mkostemps), Previous: [`lcong48_r`](#lcong48_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.16 `lrand48_r` [¶](#lrand48_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-lrand48-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/SVID-Random.html>,
* [man lrand48\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/lrand48_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.3, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`mkstemps`](#mkstemps), Previous: [`lrand48_r`](#lrand48_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.17 `mkostemps` [¶](#mkostemps-1)

Documentation:  
 [man mkostemps](https://www.kernel.org/doc/man-pages/online/pages/man3/mkostemps.3.html)

Gnulib module: mkostemps

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.10, Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.5, mingw, MSVC 14, Android 5.1.
* This function is declared in `<unistd.h>` instead of `<stdlib.h>`
  on some platforms:
  macOS 14.
* On platforms where `off_t` is a 32-bit type, `mkostemps` may not work
  correctly to create files larger than 2 GB. (Cf. `AC_SYS_LARGEFILE`.)

Portability problems not fixed by Gnulib:

The gnulib module `clean-temp` can create temporary files that will not
be left behind after signals such as SIGINT.

---

Next: [`mrand48_r`](#mrand48_005fr), Previous: [`mkostemps`](#mkostemps), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.18 `mkstemps` [¶](#mkstemps-1)

Documentation:  
 [man mkstemps](https://www.kernel.org/doc/man-pages/online/pages/man3/mkstemps.3.html)

Gnulib module: mkstemps

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.10, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10,
  Cygwin 1.5.x, mingw, MSVC 14.
* This function is declared in `<unistd.h>` instead of `<stdlib.h>`
  on some platforms:
  macOS 14.
* On platforms where `off_t` is a 32-bit type, `mkstemps` may not work
  correctly to create files larger than 2 GB. (Cf. `AC_SYS_LARGEFILE`.)

Portability problems not fixed by Gnulib:

The gnulib module `clean-temp` can create temporary files that will not
be left behind after signals such as SIGINT.

---

Next: [`nrand48_r`](#nrand48_005fr), Previous: [`mkstemps`](#mkstemps), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.19 `mrand48_r` [¶](#mrand48_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-mrand48-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/SVID-Random.html>,
* [man mrand48\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/mrand48_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.3, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`on_exit`](#on_005fexit), Previous: [`mrand48_r`](#mrand48_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.20 `nrand48_r` [¶](#nrand48_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-nrand48-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/SVID-Random.html>,
* [man nrand48\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/nrand48_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.3, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`qecvt`](#qecvt), Previous: [`nrand48_r`](#nrand48_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.21 `on_exit` [¶](#on_005fexit-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Cleanups-on-Exit.html>,
* [man on\_exit](https://www.kernel.org/doc/man-pages/online/pages/man3/on_exit.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.

---

Next: [`qecvt_r`](#qecvt_005fr), Previous: [`on_exit`](#on_005fexit), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.22 `qecvt` [¶](#qecvt-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/System-V-Number-Conversion.html>,
* [man qecvt](https://www.kernel.org/doc/man-pages/online/pages/man3/qecvt.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`qfcvt`](#qfcvt), Previous: [`qecvt`](#qecvt), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.23 `qecvt_r` [¶](#qecvt_005fr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/System-V-Number-Conversion.html>,
* [man qecvt\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/qecvt_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`qfcvt_r`](#qfcvt_005fr), Previous: [`qecvt_r`](#qecvt_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.24 `qfcvt` [¶](#qfcvt-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/System-V-Number-Conversion.html>,
* [man qfcvt](https://www.kernel.org/doc/man-pages/online/pages/man3/qfcvt.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`qgcvt`](#qgcvt), Previous: [`qfcvt`](#qfcvt), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.25 `qfcvt_r` [¶](#qfcvt_005fr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/System-V-Number-Conversion.html>,
* [man qfcvt\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/qfcvt_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`random_r`](#random_005fr), Previous: [`qfcvt_r`](#qfcvt_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.26 `qgcvt` [¶](#qgcvt-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/System-V-Number-Conversion.html>,
* [man qgcvt](https://www.kernel.org/doc/man-pages/online/pages/man3/qgcvt.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`rpmatch`](#rpmatch), Previous: [`qgcvt`](#qgcvt), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.27 `random_r` [¶](#random_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-random-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/BSD-Random.html>,
* [man random\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/random_r.3.html).

Gnulib module: random\_r

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function has an incompatible declaration on some platforms:
  AIX 7.1.

Portability problems not fixed by Gnulib:

* This function has a slightly different (but compatible) declaration on some
  platforms:
  Haiku 2017.

---

Next: [`seed48_r`](#seed48_005fr), Previous: [`random_r`](#random_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.28 `rpmatch` [¶](#rpmatch-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Yes_002dor_002dNo-Questions.html>,
* [man rpmatch](https://www.kernel.org/doc/man-pages/online/pages/man3/rpmatch.3.html).

Gnulib module: rpmatch

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 10.14, FreeBSD 5.2.1, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`setstate_r`](#setstate_005fr), Previous: [`rpmatch`](#rpmatch), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.29 `seed48_r` [¶](#seed48_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-seed48-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/SVID-Random.html>,
* [man seed48\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/seed48_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.3, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`srand48_r`](#srand48_005fr), Previous: [`seed48_r`](#seed48_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.30 `setstate_r` [¶](#setstate_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-setstate-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/BSD-Random.html>,
* [man setstate\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/setstate_r.3.html).

Gnulib module: random\_r

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function has an incompatible declaration on some platforms:
  AIX 7.1.

Portability problems not fixed by Gnulib:

* This function has a slightly different (but compatible) declaration on some
  platforms:
  Haiku.

---

Next: [`srandom_r`](#srandom_005fr), Previous: [`setstate_r`](#setstate_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.31 `srand48_r` [¶](#srand48_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-srand48-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/SVID-Random.html>,
* [man srand48\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/srand48_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.3, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`strtod_l`](#strtod_005fl), Previous: [`srand48_r`](#srand48_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.32 `srandom_r` [¶](#srandom_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-srandom-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/BSD-Random.html>,
* [man srandom\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/srandom_r.3.html).

Gnulib module: random\_r

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`strtof_l`](#strtof_005fl), Previous: [`srandom_r`](#srandom_005fr), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.33 `strtod_l` [¶](#strtod_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 7.1.

---

Next: [`strtol_l`](#strtol_005fl), Previous: [`strtod_l`](#strtod_005fl), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.34 `strtof_l` [¶](#strtof_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 7.1.

---

Next: [`strtold_l`](#strtold_005fl), Previous: [`strtof_l`](#strtof_005fl), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.35 `strtol_l` [¶](#strtol_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 7.1.

---

Next: [`strtoll_l`](#strtoll_005fl), Previous: [`strtol_l`](#strtol_005fl), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.36 `strtold_l` [¶](#strtold_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.

---

Next: [`strtoq`](#strtoq), Previous: [`strtold_l`](#strtold_005fl), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.37 `strtoll_l` [¶](#strtoll_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.

---

Next: [`strtoul_l`](#strtoul_005fl), Previous: [`strtoll_l`](#strtoll_005fl), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.38 `strtoq` [¶](#strtoq-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-strtoq-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Parsing-of-Integers.html>,
* [man strtoq](https://www.kernel.org/doc/man-pages/online/pages/man3/strtoq.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.
* This function is not declared on some platforms:
  Android 9.0.

---

Next: [`strtoull_l`](#strtoull_005fl), Previous: [`strtoq`](#strtoq), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.39 `strtoul_l` [¶](#strtoul_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 7.1.

---

Next: [`strtouq`](#strtouq), Previous: [`strtoul_l`](#strtoul_005fl), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.40 `strtoull_l` [¶](#strtoull_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.

---

Next: [`uabs`](#uabs), Previous: [`strtoull_l`](#strtoull_005fl), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.41 `strtouq` [¶](#strtouq-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-strtouq-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Parsing-of-Integers.html>,
* [man strtouq](https://www.kernel.org/doc/man-pages/online/pages/man3/strtouq.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.
* This function is not declared on some platforms:
  Android 9.0.

---

Next: [`ulabs`](#ulabs), Previous: [`strtouq`](#strtouq), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.42 `uabs` [¶](#uabs-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Absolute-Value.html>,
* [man uabs](https://www.kernel.org/doc/man-pages/online/pages/man3/uabs.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most platforms:
  glibc 2.41, macOS 15, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.7, Minix, AIX 7.3, HP-UX 11.31, Solaris 11.4, Cygwin, mingw, MSVC 14.

---

Next: [`ullabs`](#ullabs), Previous: [`uabs`](#uabs), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.43 `ulabs` [¶](#ulabs-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Absolute-Value.html>,
* [man ulabs](https://www.kernel.org/doc/man-pages/online/pages/man3/ulabs.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most platforms:
  glibc 2.41, macOS 15, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.7, Minix, AIX 7.3, HP-UX 11.31, Solaris 11.4, Cygwin, mingw, MSVC 14.

---

Next: [`valloc`](#valloc), Previous: [`ulabs`](#ulabs), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.44 `ullabs` [¶](#ullabs-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Absolute-Value.html>,
* [man ullabs](https://www.kernel.org/doc/man-pages/online/pages/man3/ullabs.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most platforms:
  glibc 2.41, macOS 15, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.7, Minix, AIX 7.3, HP-UX 11.31, Solaris 11.4, Cygwin, mingw, MSVC 14.

---

Previous: [`ullabs`](#ullabs), Up: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.67.45 `valloc` [¶](#valloc-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Aligned-Memory-Blocks.html>,
* [man valloc](https://www.kernel.org/doc/man-pages/online/pages/man3/valloc.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [Glibc `<sys/auxv.h>`](#Glibc-sys_002fauxv_002eh), Previous: [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.68 Glibc Extensions to `<string.h>` [¶](#Glibc-Extensions-to-_003cstring_002eh_003e)

* [`explicit_bzero`](#explicit_005fbzero)
* [`memfrob`](#memfrob)
* [`mempcpy`](#mempcpy)
* [`memrchr`](#memrchr)
* [`rawmemchr`](#rawmemchr)
* [`sigabbrev_np`](#sigabbrev_005fnp)
* [`sigdescr_np`](#sigdescr_005fnp)
* [`strcasestr`](#strcasestr)
* [`strchrnul`](#strchrnul)
* [`strerrordesc_np`](#strerrordesc_005fnp)
* [`strerrorname_np`](#strerrorname_005fnp)
* [`strfry`](#strfry)
* [`strsep`](#strsep)
* [`strverscmp`](#strverscmp)

---

Next: [`memfrob`](#memfrob), Up: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.68.1 `explicit_bzero` [¶](#explicit_005fbzero-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Erasing-Sensitive-Data.html>,
* [man explicit\_bzero](https://www.kernel.org/doc/man-pages/online/pages/man3/explicit_bzero.3.html).

Gnulib module: explicit\_bzero

The `explicit_bzero` function is an approximation to what is
needed, and does not suffice in general to erase information.
Although calling `explicit_bzero` should clear the memory in
question, the information that was in memory may still be available
elsewhere on the machine. Proper implementation of information
erasure requires support from levels below C code.

C23 specifies the function `memset_explicit`, which should be
preferred to `explicit_bzero` in new code.
See [`memset_explicit`](#memset_005fexplicit).

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.24, FreeBSD 10, NetBSD 7.1, OpenBSD 5.4, macOS 10.12, Solaris 11.4, Android 9.0,
  and many other systems.

Portability problems not fixed by Gnulib:

* Although the module’s implementation should clear the memory on
  platforms compatible with GCC and on platforms using traditional
  linkers, it may not clear the memory on non-GCC platforms that use
  whole-program optimization.

---

Next: [`mempcpy`](#mempcpy), Previous: [`explicit_bzero`](#explicit_005fbzero), Up: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.68.2 `memfrob` [¶](#memfrob-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Obfuscating-Data.html>,
* [man memfrob](https://www.kernel.org/doc/man-pages/online/pages/man3/memfrob.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`memrchr`](#memrchr), Previous: [`memfrob`](#memfrob), Up: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.68.3 `mempcpy` [¶](#mempcpy-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Copying-Strings-and-Arrays.html>,
* [man mempcpy](https://www.kernel.org/doc/man-pages/online/pages/man3/mempcpy.3.html).

Gnulib module: mempcpy

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 13.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 5.1.

Portability problems not fixed by Gnulib:

---

Next: [`rawmemchr`](#rawmemchr), Previous: [`mempcpy`](#mempcpy), Up: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.68.4 `memrchr` [¶](#memrchr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-memrchr.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Search-Functions.html>,
* [man memrchr](https://www.kernel.org/doc/man-pages/online/pages/man3/memrchr.3.html).

Gnulib module: memrchr

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`sigabbrev_np`](#sigabbrev_005fnp), Previous: [`memrchr`](#memrchr), Up: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.68.5 `rawmemchr` [¶](#rawmemchr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Search-Functions.html>,
* [man rawmemchr](https://www.kernel.org/doc/man-pages/online/pages/man3/rawmemchr.3.html).

Gnulib module: rawmemchr

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`sigdescr_np`](#sigdescr_005fnp), Previous: [`rawmemchr`](#rawmemchr), Up: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.68.6 `sigabbrev_np` [¶](#sigabbrev_005fnp-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Signal-Messages.html>.

Gnulib module: sigabbrev\_np

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.31, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

Note: Gnulib has a module `sig2str` that contains an equivalent function
and also one that does the opposite conversion, from an abbreviated signal name
to a signal number.

---

Next: [`strcasestr`](#strcasestr), Previous: [`sigabbrev_np`](#sigabbrev_005fnp), Up: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.68.7 `sigdescr_np` [¶](#sigdescr_005fnp-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Signal-Messages.html>.

Gnulib module: sigdescr\_np

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.31, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

* The description of SIGCHLD is wrong on some platforms:
  glibc.
* The description of SIGEMT is completely unintelligible on some platforms:
  glibc.

Note: This function is hardly useful, because it returns English strings, not
internationalized strings. Better use the function `strsignal`, which
returns internationalized strings.

---

Next: [`strchrnul`](#strchrnul), Previous: [`sigdescr_np`](#sigdescr_005fnp), Up: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.68.8 `strcasestr` [¶](#strcasestr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-strcasestr.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Search-Functions.html>,
* [man strcasestr](https://www.kernel.org/doc/man-pages/online/pages/man3/strcasestr.3.html).

Gnulib module: strcasestr or strcasestr-simple

Portability problems fixed by either Gnulib module `strcasestr-simple`
or `strcasestr`:

* This function is missing on some platforms:
  AIX 7.2, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14.
* This function can trigger memchr bugs on some platforms:
  glibc 2.10.
* This function can trigger false positives for long periodic needles on
  some platforms:
  glibc 2.12, Cygwin 1.7.7.
* This function may fail to find matches on some platforms:
  glibc 2.28.

Portability problems fixed by Gnulib module `strcasestr`:

* This function has quadratic instead of linear worst-case complexity on some
  platforms:
  glibc 2.8, musl libc 1.2.3, macOS 14, FreeBSD 13.1, NetBSD 10.0, OpenBSD 7.2, Solaris 11.4.

Portability problems not fixed by Gnulib:

---

Next: [`strerrordesc_np`](#strerrordesc_005fnp), Previous: [`strcasestr`](#strcasestr), Up: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.68.9 `strchrnul` [¶](#strchrnul-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Search-Functions.html>,
* [man strchrnul](https://www.kernel.org/doc/man-pages/online/pages/man3/strchrnul.3.html).

Gnulib module: strchrnul

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 15.3, FreeBSD 6.0, NetBSD 7.1, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.7.8, mingw, MSVC 14, Android 6.0.
* This function crashes when no occurrence is found on some platforms:
  Cygwin 1.7.9.

Portability problems not fixed by Gnulib:

---

Next: [`strerrorname_np`](#strerrorname_005fnp), Previous: [`strchrnul`](#strchrnul), Up: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.68.10 `strerrordesc_np` [¶](#strerrordesc_005fnp-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.31, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`strfry`](#strfry), Previous: [`strerrordesc_np`](#strerrordesc_005fnp), Up: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.68.11 `strerrorname_np` [¶](#strerrorname_005fnp-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>.

Gnulib module: strerrorname\_np

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.31, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android API level 34.
* This function returns wrong values on some platforms:
  glibc 2.37, Solaris 11 OmniOS.

Portability problems not fixed by Gnulib:

---

Next: [`strsep`](#strsep), Previous: [`strerrorname_np`](#strerrorname_005fnp), Up: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.68.12 `strfry` [¶](#strfry-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Shuffling-Bytes.html>,
* [man strfry](https://www.kernel.org/doc/man-pages/online/pages/man3/strfry.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`strverscmp`](#strverscmp), Previous: [`strfry`](#strfry), Up: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.68.13 `strsep` [¶](#strsep-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-strsep-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Finding-Tokens-in-a-String.html>,
* [man strsep](https://www.kernel.org/doc/man-pages/online/pages/man3/strsep.3.html).

Gnulib module: strsep

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11, Solaris 10, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Previous: [`strsep`](#strsep), Up: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.68.14 `strverscmp` [¶](#strverscmp-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/String_002fArray-Comparison.html>,
* [man strverscmp](https://www.kernel.org/doc/man-pages/online/pages/man3/strverscmp.3.html).

Gnulib module: strverscmp

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  macOS 14, FreeBSD 13.1, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.
* This function treats ASCII letters as smaller than a digit sequence
  on some platforms:
  musl libc 1.2.3, Cygwin 3.4.x.

Portability problems not fixed by Gnulib:

* On older glibc platforms, this function does not always properly treat
  digit strings with leading zeros as fractions (for example,
  `strverscmp ("B0075022800016.gbp.corp.com",
  "B007502357019.GBP.CORP.COM")` does not yield a negative number as it
  should):
  glibc 2.9

---

Next: [Glibc `<sys/capability.h>`](#Glibc-sys_002fcapability_002eh), Previous: [Glibc Extensions to `<string.h>`](#Glibc-string_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.69 Glibc `<sys/auxv.h>` [¶](#Glibc-_003csys_002fauxv_002eh_003e)

* [`getauxval`](#getauxval)

---

Up: [Glibc `<sys/auxv.h>`](#Glibc-sys_002fauxv_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.69.1 `getauxval` [¶](#getauxval-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Auxiliary-Vector.html>,
* [man getauxval](https://www.kernel.org/doc/man-pages/online/pages/man3/getauxval.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.15, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [Glibc `<sys/epoll.h>`](#Glibc-sys_002fepoll_002eh), Previous: [Glibc `<sys/auxv.h>`](#Glibc-sys_002fauxv_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.70 Glibc `<sys/capability.h>` [¶](#Glibc-_003csys_002fcapability_002eh_003e)

* [`capget`](#capget)
* [`capset`](#capset)

---

Next: [`capset`](#capset), Up: [Glibc `<sys/capability.h>`](#Glibc-sys_002fcapability_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.70.1 `capget` [¶](#capget-1)

Documentation:  
 [man capget](https://www.kernel.org/doc/man-pages/online/pages/man2/capget.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Previous: [`capget`](#capget), Up: [Glibc `<sys/capability.h>`](#Glibc-sys_002fcapability_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.70.2 `capset` [¶](#capset-1)

Documentation:  
 [man capset](https://www.kernel.org/doc/man-pages/online/pages/man2/capset.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [Glibc `<sys/eventfd.h>`](#Glibc-sys_002feventfd_002eh), Previous: [Glibc `<sys/capability.h>`](#Glibc-sys_002fcapability_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.71 Glibc `<sys/epoll.h>` [¶](#Glibc-_003csys_002fepoll_002eh_003e)

* [`epoll_create`](#epoll_005fcreate)
* [`epoll_create1`](#epoll_005fcreate1)
* [`epoll_ctl`](#epoll_005fctl)
* [`epoll_pwait`](#epoll_005fpwait)
* [`epoll_pwait2`](#epoll_005fpwait2)
* [`epoll_wait`](#epoll_005fwait)

---

Next: [`epoll_create1`](#epoll_005fcreate1), Up: [Glibc `<sys/epoll.h>`](#Glibc-sys_002fepoll_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.71.1 `epoll_create` [¶](#epoll_005fcreate-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-epoll-create-1.html>

Documentation:  
 [man epoll\_create](https://www.kernel.org/doc/man-pages/online/pages/man2/epoll_create.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and illumos and is therefore
  missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`epoll_ctl`](#epoll_005fctl), Previous: [`epoll_create`](#epoll_005fcreate), Up: [Glibc `<sys/epoll.h>`](#Glibc-sys_002fepoll_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.71.2 `epoll_create1` [¶](#epoll_005fcreate1-1)

Documentation:  
 [man epoll\_create1](https://www.kernel.org/doc/man-pages/online/pages/man2/epoll_create1.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and illumos and is therefore
  missing on many non-glibc platforms:
  glibc 2.8, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 4.3.

---

Next: [`epoll_pwait`](#epoll_005fpwait), Previous: [`epoll_create1`](#epoll_005fcreate1), Up: [Glibc `<sys/epoll.h>`](#Glibc-sys_002fepoll_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.71.3 `epoll_ctl` [¶](#epoll_005fctl-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-epoll-ctl-1.html>

Documentation:  
 [man epoll\_ctl](https://www.kernel.org/doc/man-pages/online/pages/man2/epoll_ctl.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and illumos and is therefore
  missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`epoll_pwait2`](#epoll_005fpwait2), Previous: [`epoll_ctl`](#epoll_005fctl), Up: [Glibc `<sys/epoll.h>`](#Glibc-sys_002fepoll_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.71.4 `epoll_pwait` [¶](#epoll_005fpwait-1)

Documentation:  
 [man epoll\_pwait](https://www.kernel.org/doc/man-pages/online/pages/man2/epoll_pwait.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and illumos and is therefore
  missing on many non-glibc platforms:
  glibc 2.5, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 4.3.

---

Next: [`epoll_wait`](#epoll_005fwait), Previous: [`epoll_pwait`](#epoll_005fpwait), Up: [Glibc `<sys/epoll.h>`](#Glibc-sys_002fepoll_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.71.5 `epoll_pwait2` [¶](#epoll_005fpwait2-1)

Documentation:  
 [man epoll\_pwait2](https://www.kernel.org/doc/man-pages/online/pages/man2/epoll_pwait2.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.34, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android API level 34.

---

Previous: [`epoll_pwait2`](#epoll_005fpwait2), Up: [Glibc `<sys/epoll.h>`](#Glibc-sys_002fepoll_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.71.6 `epoll_wait` [¶](#epoll_005fwait-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-epoll-wait-1.html>

Documentation:  
 [man epoll\_wait](https://www.kernel.org/doc/man-pages/online/pages/man2/epoll_wait.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and illumos and is therefore
  missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [Glibc `<sys/fanotify.h>`](#Glibc-sys_002ffanotify_002eh), Previous: [Glibc `<sys/epoll.h>`](#Glibc-sys_002fepoll_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.72 Glibc `<sys/eventfd.h>` [¶](#Glibc-_003csys_002feventfd_002eh_003e)

* [`eventfd`](#eventfd)
* [`eventfd_read`](#eventfd_005fread)
* [`eventfd_write`](#eventfd_005fwrite)

---

Next: [`eventfd_read`](#eventfd_005fread), Up: [Glibc `<sys/eventfd.h>`](#Glibc-sys_002feventfd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.72.1 `eventfd` [¶](#eventfd-1)

Documentation:  
 [man eventfd](https://www.kernel.org/doc/man-pages/online/pages/man2/eventfd.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux, FreeBSD, NetBSD, and illumos and is therefore
  missing on many non-glibc platforms:
  glibc 2.6, macOS 14, FreeBSD 12.0, NetBSD 9.3, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`eventfd_write`](#eventfd_005fwrite), Previous: [`eventfd`](#eventfd), Up: [Glibc `<sys/eventfd.h>`](#Glibc-sys_002feventfd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.72.2 `eventfd_read` [¶](#eventfd_005fread-1)

Documentation:  
 [man eventfd\_read](https://www.kernel.org/doc/man-pages/online/pages/man3/eventfd_read.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux, FreeBSD, NetBSD, and illumos and is therefore
  missing on many non-glibc platforms:
  glibc 2.6, macOS 14, FreeBSD 12.0, NetBSD 9.3, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Previous: [`eventfd_read`](#eventfd_005fread), Up: [Glibc `<sys/eventfd.h>`](#Glibc-sys_002feventfd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.72.3 `eventfd_write` [¶](#eventfd_005fwrite-1)

Documentation:  
 [man eventfd\_write](https://www.kernel.org/doc/man-pages/online/pages/man3/eventfd_write.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux, FreeBSD, NetBSD, and illumos and is therefore
  missing on many non-glibc platforms:
  glibc 2.6, macOS 14, FreeBSD 12.0, NetBSD 9.3, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [Glibc `<sys/file.h>`](#Glibc-sys_002ffile_002eh), Previous: [Glibc `<sys/eventfd.h>`](#Glibc-sys_002feventfd_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.73 Glibc `<sys/fanotify.h>` [¶](#Glibc-_003csys_002ffanotify_002eh_003e)

* [`fanotify_init`](#fanotify_005finit)
* [`fanotify_mark`](#fanotify_005fmark)

---

Next: [`fanotify_mark`](#fanotify_005fmark), Up: [Glibc `<sys/fanotify.h>`](#Glibc-sys_002ffanotify_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.73.1 `fanotify_init` [¶](#fanotify_005finit-1)

Documentation:  
 [man fanotify\_init](https://www.kernel.org/doc/man-pages/online/pages/man2/fanotify_init.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on all non-glibc platforms:
  glibc 2.12, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`fanotify_init`](#fanotify_005finit), Up: [Glibc `<sys/fanotify.h>`](#Glibc-sys_002ffanotify_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.73.2 `fanotify_mark` [¶](#fanotify_005fmark-1)

Documentation:  
 [man fanotify\_mark](https://www.kernel.org/doc/man-pages/online/pages/man2/fanotify_mark.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on all non-glibc platforms:
  glibc 2.12, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<sys/fsuid.h>`](#Glibc-sys_002ffsuid_002eh), Previous: [Glibc `<sys/fanotify.h>`](#Glibc-sys_002ffanotify_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.74 Glibc `<sys/file.h>` [¶](#Glibc-_003csys_002ffile_002eh_003e)

* [`flock`](#flock)

---

Up: [Glibc `<sys/file.h>`](#Glibc-sys_002ffile_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.74.1 `flock` [¶](#flock-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-flock-2.html>

Documentation:  
 [man flock](https://www.kernel.org/doc/man-pages/online/pages/man2/flock.2.html)

Gnulib module: flock

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14
* This function is missing on some platforms:
  AIX 7.1, HP-UX 11.23, Solaris 11.4.
  But the replacement function does not really work; it exhibits test failures.

Portability problems not fixed by Gnulib:

---

Next: [Glibc `<sys/gmon.h>`](#Glibc-sys_002fgmon_002eh), Previous: [Glibc `<sys/file.h>`](#Glibc-sys_002ffile_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.75 Glibc `<sys/fsuid.h>` [¶](#Glibc-_003csys_002ffsuid_002eh_003e)

* [`setfsgid`](#setfsgid)
* [`setfsuid`](#setfsuid)

---

Next: [`setfsuid`](#setfsuid), Up: [Glibc `<sys/fsuid.h>`](#Glibc-sys_002ffsuid_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.75.1 `setfsgid` [¶](#setfsgid-1)

Documentation:  
 [man setfsgid](https://www.kernel.org/doc/man-pages/online/pages/man2/setfsgid.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 4.4.

---

Previous: [`setfsgid`](#setfsgid), Up: [Glibc `<sys/fsuid.h>`](#Glibc-sys_002ffsuid_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.75.2 `setfsuid` [¶](#setfsuid-1)

Documentation:  
 [man setfsuid](https://www.kernel.org/doc/man-pages/online/pages/man2/setfsuid.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 4.4.

---

Next: [Glibc `<sys/inotify.h>`](#Glibc-sys_002finotify_002eh), Previous: [Glibc `<sys/fsuid.h>`](#Glibc-sys_002ffsuid_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.76 Glibc `<sys/gmon.h>` [¶](#Glibc-_003csys_002fgmon_002eh_003e)

* [`monstartup`](#monstartup)

---

Up: [Glibc `<sys/gmon.h>`](#Glibc-sys_002fgmon_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.76.1 `monstartup` [¶](#monstartup-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<sys/io.h>`, `<sys/perm.h>`](#Glibc-sys_002fio_002eh-and-sys_002fperm_002eh), Previous: [Glibc `<sys/gmon.h>`](#Glibc-sys_002fgmon_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.77 Glibc `<sys/inotify.h>` [¶](#Glibc-_003csys_002finotify_002eh_003e)

* [`inotify_add_watch`](#inotify_005fadd_005fwatch)
* [`inotify_init`](#inotify_005finit)
* [`inotify_init1`](#inotify_005finit1)
* [`inotify_rm_watch`](#inotify_005frm_005fwatch)

---

Next: [`inotify_init`](#inotify_005finit), Up: [Glibc `<sys/inotify.h>`](#Glibc-sys_002finotify_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.77.1 `inotify_add_watch` [¶](#inotify_005fadd_005fwatch-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-inotify-add-watch.html>

Documentation:  
 [man inotify\_add\_watch](https://www.kernel.org/doc/man-pages/online/pages/man2/inotify_add_watch.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most non-glibc platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`inotify_init1`](#inotify_005finit1), Previous: [`inotify_add_watch`](#inotify_005fadd_005fwatch), Up: [Glibc `<sys/inotify.h>`](#Glibc-sys_002finotify_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.77.2 `inotify_init` [¶](#inotify_005finit-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-inotify-init.html>

Documentation:  
 [man inotify\_init](https://www.kernel.org/doc/man-pages/online/pages/man2/inotify_init.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most non-glibc platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`inotify_rm_watch`](#inotify_005frm_005fwatch), Previous: [`inotify_init`](#inotify_005finit), Up: [Glibc `<sys/inotify.h>`](#Glibc-sys_002finotify_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.77.3 `inotify_init1` [¶](#inotify_005finit1-1)

Documentation:  
 [man inotify\_init1](https://www.kernel.org/doc/man-pages/online/pages/man2/inotify_init1.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most non-glibc platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 4.3.

---

Previous: [`inotify_init1`](#inotify_005finit1), Up: [Glibc `<sys/inotify.h>`](#Glibc-sys_002finotify_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.77.4 `inotify_rm_watch` [¶](#inotify_005frm_005fwatch-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-inotify-rm-watch.html>

Documentation:  
 [man inotify\_rm\_watch](https://www.kernel.org/doc/man-pages/online/pages/man2/inotify_rm_watch.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most non-glibc platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [Glibc `<sys/kdaemon.h>`](#Glibc-sys_002fkdaemon_002eh), Previous: [Glibc `<sys/inotify.h>`](#Glibc-sys_002finotify_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.78 Glibc `<sys/io.h>`, `<sys/perm.h>` [¶](#Glibc-_003csys_002fio_002eh_003e_002c-_003csys_002fperm_002eh_003e)

* [`ioperm`](#ioperm)
* [`iopl`](#iopl)

---

Next: [`iopl`](#iopl), Up: [Glibc `<sys/io.h>`, `<sys/perm.h>`](#Glibc-sys_002fio_002eh-and-sys_002fperm_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.78.1 `ioperm` [¶](#ioperm-1)

Documentation:  
 [man ioperm](https://www.kernel.org/doc/man-pages/online/pages/man2/ioperm.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`ioperm`](#ioperm), Up: [Glibc `<sys/io.h>`, `<sys/perm.h>`](#Glibc-sys_002fio_002eh-and-sys_002fperm_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.78.2 `iopl` [¶](#iopl-1)

Documentation:  
 [man iopl](https://www.kernel.org/doc/man-pages/online/pages/man2/iopl.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<sys/klog.h>`](#Glibc-sys_002fklog_002eh), Previous: [Glibc `<sys/io.h>`, `<sys/perm.h>`](#Glibc-sys_002fio_002eh-and-sys_002fperm_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.79 Glibc `<sys/kdaemon.h>` [¶](#Glibc-_003csys_002fkdaemon_002eh_003e)

* [`bdflush`](#bdflush)

---

Up: [Glibc `<sys/kdaemon.h>`](#Glibc-sys_002fkdaemon_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.79.1 `bdflush` [¶](#bdflush-1)

Documentation:  
 [man bdflush](https://www.kernel.org/doc/man-pages/online/pages/man2/bdflush.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh), Previous: [Glibc `<sys/kdaemon.h>`](#Glibc-sys_002fkdaemon_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.80 Glibc `<sys/klog.h>` [¶](#Glibc-_003csys_002fklog_002eh_003e)

* [`klogctl`](#klogctl)

---

Up: [Glibc `<sys/klog.h>`](#Glibc-sys_002fklog_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.80.1 `klogctl` [¶](#klogctl-1)

Documentation:  
 [man klogctl](https://www.kernel.org/doc/man-pages/online/pages/man3/klogctl.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [Glibc `<sys/mount.h>`](#Glibc-sys_002fmount_002eh), Previous: [Glibc `<sys/klog.h>`](#Glibc-sys_002fklog_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
