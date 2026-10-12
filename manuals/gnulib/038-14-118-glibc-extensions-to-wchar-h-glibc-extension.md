### 14.118 Glibc Extensions to `<wchar.h>` [¶](#Glibc-Extensions-to-_003cwchar_002eh_003e)

* [`fgetwc_unlocked`](#fgetwc_005funlocked)
* [`fgetws_unlocked`](#fgetws_005funlocked)
* [`fputwc_unlocked`](#fputwc_005funlocked)
* [`fputws_unlocked`](#fputws_005funlocked)
* [`getwc_unlocked`](#getwc_005funlocked)
* [`getwchar_unlocked`](#getwchar_005funlocked)
* [`putwc_unlocked`](#putwc_005funlocked)
* [`putwchar_unlocked`](#putwchar_005funlocked)
* [`wcschrnul`](#wcschrnul)
* [`wcsftime_l`](#wcsftime_005fl)
* [`wcstod_l`](#wcstod_005fl)
* [`wcstof_l`](#wcstof_005fl)
* [`wcstol_l`](#wcstol_005fl)
* [`wcstold_l`](#wcstold_005fl)
* [`wcstoll_l`](#wcstoll_005fl)
* [`wcstoq`](#wcstoq)
* [`wcstoul_l`](#wcstoul_005fl)
* [`wcstoull_l`](#wcstoull_005fl)
* [`wcstouq`](#wcstouq)
* [`wmempcpy`](#wmempcpy)

---

Next: [`fgetws_unlocked`](#fgetws_005funlocked), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.1 `fgetwc_unlocked` [¶](#fgetwc_005funlocked-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-fgetwc-unlocked-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Character-Input.html>,
* [man fgetwc\_unlocked](https://www.kernel.org/doc/man-pages/online/pages/man3/fgetwc_unlocked.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`fputwc_unlocked`](#fputwc_005funlocked), Previous: [`fgetwc_unlocked`](#fgetwc_005funlocked), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.2 `fgetws_unlocked` [¶](#fgetws_005funlocked-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-fgetws-unlocked-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Line-Input.html>,
* [man fgetws\_unlocked](https://www.kernel.org/doc/man-pages/online/pages/man3/fgetws_unlocked.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`fputws_unlocked`](#fputws_005funlocked), Previous: [`fgetws_unlocked`](#fgetws_005funlocked), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.3 `fputwc_unlocked` [¶](#fputwc_005funlocked-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-fputwc-unlocked-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Simple-Output.html>,
* [man fputwc\_unlocked](https://www.kernel.org/doc/man-pages/online/pages/man3/fputwc_unlocked.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`getwc_unlocked`](#getwc_005funlocked), Previous: [`fputwc_unlocked`](#fputwc_005funlocked), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.4 `fputws_unlocked` [¶](#fputws_005funlocked-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-fputws-unlocked-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Simple-Output.html>,
* [man fputws\_unlocked](https://www.kernel.org/doc/man-pages/online/pages/man3/fputws_unlocked.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`getwchar_unlocked`](#getwchar_005funlocked), Previous: [`fputws_unlocked`](#fputws_005funlocked), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.5 `getwc_unlocked` [¶](#getwc_005funlocked-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getwc-unlocked-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Character-Input.html>,
* [man getwc\_unlocked](https://www.kernel.org/doc/man-pages/online/pages/man3/getwc_unlocked.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`putwc_unlocked`](#putwc_005funlocked), Previous: [`getwc_unlocked`](#getwc_005funlocked), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.6 `getwchar_unlocked` [¶](#getwchar_005funlocked-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getwchar-unlocked-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Character-Input.html>,
* [man getwchar\_unlocked](https://www.kernel.org/doc/man-pages/online/pages/man3/getwchar_unlocked.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`putwchar_unlocked`](#putwchar_005funlocked), Previous: [`getwchar_unlocked`](#getwchar_005funlocked), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.7 `putwc_unlocked` [¶](#putwc_005funlocked-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-putwc-unlocked-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Simple-Output.html>,
* [man putwc\_unlocked](https://www.kernel.org/doc/man-pages/online/pages/man3/putwc_unlocked.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcschrnul`](#wcschrnul), Previous: [`putwc_unlocked`](#putwc_005funlocked), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.8 `putwchar_unlocked` [¶](#putwchar_005funlocked-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-putwchar-unlocked-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Simple-Output.html>,
* [man putwchar\_unlocked](https://www.kernel.org/doc/man-pages/online/pages/man3/putwchar_unlocked.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcsftime_l`](#wcsftime_005fl), Previous: [`putwchar_unlocked`](#putwchar_005funlocked), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.9 `wcschrnul` [¶](#wcschrnul-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Search-Functions.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstod_l`](#wcstod_005fl), Previous: [`wcschrnul`](#wcschrnul), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.10 `wcsftime_l` [¶](#wcsftime_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstof_l`](#wcstof_005fl), Previous: [`wcsftime_l`](#wcsftime_005fl), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.11 `wcstod_l` [¶](#wcstod_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 7.1, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstol_l`](#wcstol_005fl), Previous: [`wcstod_l`](#wcstod_005fl), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.12 `wcstof_l` [¶](#wcstof_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 7.1, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstold_l`](#wcstold_005fl), Previous: [`wcstof_l`](#wcstof_005fl), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.13 `wcstol_l` [¶](#wcstol_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstoll_l`](#wcstoll_005fl), Previous: [`wcstol_l`](#wcstol_005fl), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.14 `wcstold_l` [¶](#wcstold_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 7.1, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstoq`](#wcstoq), Previous: [`wcstold_l`](#wcstold_005fl), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.15 `wcstoll_l` [¶](#wcstoll_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstoul_l`](#wcstoul_005fl), Previous: [`wcstoll_l`](#wcstoll_005fl), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.16 `wcstoq` [¶](#wcstoq-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-wcstoq.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Parsing-of-Integers.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstoull_l`](#wcstoull_005fl), Previous: [`wcstoq`](#wcstoq), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.17 `wcstoul_l` [¶](#wcstoul_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wcstouq`](#wcstouq), Previous: [`wcstoul_l`](#wcstoul_005fl), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.18 `wcstoull_l` [¶](#wcstoull_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`wmempcpy`](#wmempcpy), Previous: [`wcstoull_l`](#wcstoull_005fl), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.19 `wcstouq` [¶](#wcstouq-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-wcstouq.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Parsing-of-Integers.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Previous: [`wcstouq`](#wcstouq), Up: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.118.20 `wmempcpy` [¶](#wmempcpy-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Copying-Strings-and-Arrays.html>,
* [man wmempcpy](https://www.kernel.org/doc/man-pages/online/pages/man3/wmempcpy.3.html).

Gnulib module: wmempcpy

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 13.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9.x, mingw, MSVC 14, Android 5.1.

Portability problems not fixed by Gnulib:

* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [Multithreading](#Multithreading), Previous: [Glibc Function Substitutes](#Glibc-Function-Substitutes), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 15 Native Windows Support [¶](#Native-Windows-Support-1)

There are three ways to create binaries that run on Microsoft Windows:

* Native binaries, built using the MinGW tool chain.
* Native binaries, built using the MSVC (MS Visual C/C++) tool chain.
* Binaries for the Cygwin environment.

This chapter deals with the MinGW and MSVC platforms, commonly called
“native Windows” platforms. Cygwin, on the other hand, is close enough
to POSIX that it can be treated like any other Unix-like platform.

* [Libtool and Windows](#Libtool-and-Windows)
* [Large File Support](#Large-File-Support)
* [Inode numbers on Windows](#Inode-numbers-on-Windows)
* [Precise file timestamps on Windows](#Precise-file-timestamps-on-Windows)
* [Avoiding the year 2038 problem](#Avoiding-the-year-2038-problem)
* [Windows sockets](#Windows-sockets)
* [Native Windows Support without MSVC Support](#Native-Windows-Support-without-MSVC-Support)
* [Visual Studio Compatibility](#Visual-Studio-Compatibility)

---

Next: [Large File Support](#Large-File-Support), Up: [Native Windows Support](#Native-Windows-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 15.1 Libtool and Windows [¶](#Libtool-and-Windows-1)

If you want it to be possible to compile your program for a native Windows
platform and you use Libtool, you need to use the `win32-dll` option of
`LT_INIT`. In other words, put:

```
LT_INIT([win32-dll])
```

in your `configure.ac`. This sets the correct names for the
`OBJDUMP`, `DLLTOOL`, and `AS` tools for the build.

If you are building a library, you will also need to pass
`-no-undefined` to make sure Libtool produces a DLL for your
library. From a `Makefile.am`:

```
libgsasl_la_LDFLAGS += -no-undefined
```

---

Next: [Inode numbers on Windows](#Inode-numbers-on-Windows), Previous: [Libtool and Windows](#Libtool-and-Windows), Up: [Native Windows Support](#Native-Windows-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 15.2 Large File Support [¶](#Large-File-Support-1)

This module provides support for files 2 GiB and larger, or with
device or inode numbers exceeding 32 bits.
To this effect, it attempts to ensure that types like `off_t` and
`ino_t` are 64-bit,
at least on the following platforms:
glibc, Mac OS X, FreeBSD, NetBSD, OpenBSD, AIX, HP-UX, Solaris,
Cygwin, mingw, MSVC.

If the types cannot be made 64-bit, `configure` issues a
warning and proceeds.

This module gives
`configure` an option ‘`--disable-largefile`’ that
suppresses support for large files. This may be useful if the package
links to other libraries whose user-facing ABIs still require
`off_t` or most other file-related types to be 32-bit on your
platform.

This module also adds to `configure` an option
`--enable-year2038`, needed on some platforms to access files
with timestamps past the year 2038. See [Avoiding the year 2038 problem](#Avoiding-the-year-2038-problem).

---

Next: [Precise file timestamps on Windows](#Precise-file-timestamps-on-Windows), Previous: [Large File Support](#Large-File-Support), Up: [Native Windows Support](#Native-Windows-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 15.3 Inode numbers on Windows [¶](#Inode-numbers-on-Windows-1)

The module ‘`windows-stat-inodes`’ ensures that,
on native Windows platforms, `struct stat` contains
`st_dev`, `st_ino` fields that are able to distinguish
different inodes.

Note: Such values can only be provided for most files on the
file system. For a few files (such as inaccessible files),
`st_dev` and `st_ino` are set to 0. Therefore,
you should test whether `st_dev != 0 && st_ino != 0`,
before going to make inferences based on the file identity
based on `st_dev` and `st_ino`.

---

Next: [Avoiding the year 2038 problem](#Avoiding-the-year-2038-problem), Previous: [Inode numbers on Windows](#Inode-numbers-on-Windows), Up: [Native Windows Support](#Native-Windows-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 15.4 Precise file timestamps on Windows [¶](#Precise-file-timestamps-on-Windows-1)

The module ‘`windows-stat-timespec`’ ensures that,
on native Windows platforms, `struct stat` contains
`st_atim`, `st_mtim`, `st_ctim` fields of type
`struct timespec`, providing 100 ns resolution for the timestamps
of files.

Note: On some types of file systems, the timestamp resolution is limited
by the file system. For example, on FAT file systems, `st_mtim`
only has a resolution of 2 seconds. For more details, see
<https://docs.microsoft.com/en-us/windows/desktop/SysInfo/file-times>.

---

Next: [Windows sockets](#Windows-sockets), Previous: [Precise file timestamps on Windows](#Precise-file-timestamps-on-Windows), Up: [Native Windows Support](#Native-Windows-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 15.5 Avoiding the year 2038 problem [¶](#Avoiding-the-year-2038-problem-1)

The *year 2038 problem* denotes unpredictable behaviour that will
likely occur in the year 2038, for programs that use a 32-bit signed
integer ‘`time_t`’ type that cannot represent timestamps on or
after 2038-01-19 03:14:08 UTC. See
[Year 2038
problem](https://en.wikipedia.org/wiki/Year_2038_problem) for details.

The Gnulib module ‘`year2038`’ fixes this problem on some
platforms, by making `time_t` wide enough to represent timestamps
after 2038. This has no effect on most current platforms, which have
timestamps that are already wide enough. However, ‘`year2038`’ by
default arranges for builds on legacy 32-bit Linux kernels running
glibc 2.34 and later to compile with ‘`_TIME_BITS=64`’ to get wider
timestamps. On older platforms that do not support timestamps after
the year 2038, ‘`year2038`’ causes `configure` to issue a
warning but still proceed. On platforms that appear to support
post-2038 timestamps but where something prevents this from working,
`configure` fails.

The default behavior of ‘`year2038`’ can be overridden by using the
`configure` option `--disable-year2038`, which
suppresses support for post-2038 timestamps. This may be useful if
the package links to other libraries whose user-facing ABIs still
require `time_t` to be 32-bit on your platform.

The Gnulib module ‘`year2038-recommended`’ is like ‘`year2038`’,
except it by default rejects platforms where `time_t` cannot represent
timestamps after 2038. If this module is used and a 32-platform cannot support
64-bit `time_t`, one can still fix the year-2038 problem by using
a 64-bit instead of a 32-bit build, as noted in the architecture list
below. If all else fails one can configure with
`--disable-year2038`; however, the resulting programs will
mishandle timestamps after 2038.

The Gnulib module ‘`year2038-recommended`’ is designed
for packages intended for use on 32-bit platforms
after the year 2038. If your package is commonly built on
32-bit platforms that will not be used after the year 2038,
you can use the ‘`year2038`’ module instead, to save builders
the trouble of configuring with `--disable-year2038`.

If the Gnulib module ‘`largefile`’ is used but neither
‘`year2038`’ nor ‘`year2038-recommended`’ is used,
`configure` will have an option `--enable-year2038`
that causes `configure` to behave as if ‘`year2038`’ was used.
This is for packages that have long used ‘`largefile`’ but have not
gotten around to upgrading their Gnulib module list to include
‘`year2038`’ or ‘`year2038-recommended`’.
See [Large File Support](#Large-File-Support).

With the ‘`year2038-recommended`’ module, `configure` by
default should work on the following 32-bit platforms (or 32-bit ABIs
in bi-arch systems):

* Linux kernel 5.1 (2019) and later with glibc 2.34 (2021) and later on
  x86, arm, mips (o32 or n32 ABI), powerpc32, sparc32, s390, hppa, m68k, sh, csky, microblaze, nios2,
* Linux kernel 5.1 (2019) and later with musl libc 1.2 (2020) and later on x86,
* Linux on arc, loong32, or1k, riscv32 and x86\_64-x32,
* NetBSD 6.0 (2012) and later on x86 and sparc,
* OpenBSD 5.5 (2014) and later on x86,
* FreeBSD/arm,
* Minix 3.3 (2014).

Whereas with ‘`year2038-recommended`’, `configure` should
by default fail on earlier versions of the abovementioned platforms if
a version is listed, and it should also by default fail on all
versions of the following older 32-bit platforms or ABIs:

* Android on ARMv7 or x86,
* Mac OS X 10.6 (2009) and earlier on x86 and powerpc,
* GNU/Hurd/x86,
* GNU/kFreeBSD/x86,
* FreeBSD/x86 (this port demoted to Tier 2 in FreeBSD 13 [2021]
  and planned to never have 64-bit `time_t`),
* MidnightBSD/x86,
* AIX/powerpc (to fix, configure with ‘`CC='gcc -maix64' AR='ar -X64'`’),
* Solaris 11.4 (2018) and earlier on x86 and sparc
  (to fix, configure with ‘`CC='gcc -m64'`’),
* Cygwin 3.3.6 (2022) and earlier on x86,
* Haiku/x86.

If you use the ‘`year2038`’ or ‘`year2038-recommended`’ modules,
and configure to support timestamps after the year 2038,
your code should not include ‘`<utmp.h>`’ or ‘`<utmpx.h>`’
directly, because these include files do not work with 64-bit timestamps
if the platform’s `time_t` was traditionally 32 bits.
Your code can instead use the ‘`readutmp`’ module,
which works around this problem.

---

Next: [Native Windows Support without MSVC Support](#Native-Windows-Support-without-MSVC-Support), Previous: [Avoiding the year 2038 problem](#Avoiding-the-year-2038-problem), Up: [Native Windows Support](#Native-Windows-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 15.6 Windows sockets [¶](#Windows-sockets-1)

One of the portability problems for native Windows are sockets and
networking functions.

* [Getaddrinfo and WINVER](#Getaddrinfo-and-WINVER)

#### 15.6.1 Getaddrinfo and WINVER [¶](#Getaddrinfo-and-WINVER)

This was written for the `getaddrinfo` module, but may be applicable to
other functions too.

The `getaddrinfo` function exists in `ws2tcpip.h` and `-lws2_32`
on Windows XP. The function declaration is present if `WINVER >= 0x0501`.
Windows 2000 does not have `getaddrinfo` in its `WS2_32.DLL`.

Thus, if you want to assume Windows XP or later, you can add
`AC_DEFINE([WINVER], [0x0501])` to avoid compiling the (partial)
`getaddrinfo` implementation.

If you want to support Windows 2000, don’t do anything. The
replacement function will open `WS2_32.DLL` during run-time to
see if there is a `getaddrinfo` function available, and use it when
available.

---

Next: [Visual Studio Compatibility](#Visual-Studio-Compatibility), Previous: [Windows sockets](#Windows-sockets), Up: [Native Windows Support](#Native-Windows-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 15.7 Native Windows Support without MSVC Support [¶](#Native-Windows-Support-without-MSVC-Support-1)

If your package does not desire to have MSVC support, that is, if MinGW
shall be the only native Windows platform that you wish to get support
for from Gnulib, and you wish to minimize the number of files imported
from Gnulib accordingly, you can do so by passing the options
‘`--avoid=msvc-inval --avoid=msvc-nothrow`’ to `gnulib-tool`.

---

Previous: [Native Windows Support without MSVC Support](#Native-Windows-Support-without-MSVC-Support), Up: [Native Windows Support](#Native-Windows-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 15.8 Visual Studio Compatibility [¶](#Visual-Studio-Compatibility-1)

The `lib-msvc-compat` module detects whether the linker supports
`--output-def` when building a library. That parameter is used
to generate a DEF file for a shared library (DLL). DEF files are
useful for developers that use Visual Studio to develop programs that
links to your library. See the GNU LD manual for more information.

There are other ways to create a DEF file, but we believe they are all
sub-optimal to using `--output-def` during the build process.
The variants we have considered include:

* Use DUMPBIN /EXPORTS.
  This is explained in
  <https://docs.microsoft.com/en-us/cpp/build/reference/dash-exports>.
  The tool does not generate DEF files directly, so its output needs to
  be post processed manually:

  ```
  $ { echo EXPORTS; \
      dumpbin /EXPORTS libfoo-0.dll | tail -n+20 | awk '{ print $4 }'; \
    } > libfoo-0.def
  $ lib /def:libfoo-0.def
  ```
* Use IMPDEF.
  There is a tool called IMPDEF
  that can generate DEF files. However, it is not part of a standard
  Visual Studio installation. Further, it is documented as being an
  unreliable process.
* Use DLLTOOL.
  The dlltool is part of the MinGW suite, and thus not part of a
  standard Visual Studio installation. The documentation for the IMPDEF
  tool claims that DLLTOOL is the wrong tool for this job. Finally,
  DLLTOOL does not generate DEF files directly, so it requires
  post-processing of the output.

If you are using libtool to build your shared library, here is how to
use this module. Import `lib-msvc-compat` to your project, and
then add the following lines to the `Makefile.am` that builds the
library:

```
if HAVE_LD_OUTPUT_DEF
libfoo_la_LDFLAGS += -Wl,--output-def,libfoo-$(DLL_VERSION).def
libfoo-$(DLL_VERSION).def: libfoo.la
defexecdir = $(libdir)
defexec_DATA = libfoo-$(DLL_VERSION).def
DISTCLEANFILES += $(defexec_DATA)
endif
```

The `DLL_VERSION` variable needs to be defined. It should be the
shared library version number used in the DLL filename. For Windows
targets you compute this value from the values you pass to Libtool’s
`-version-info`. Assuming you have variables `LT_CURRENT`
and `LT_AGE` defined for the `CURRENT` and `AGE`
libtool version integers, you compute `DLL_VERSION` as follows:

```
DLL_VERSION=`expr ${LT_CURRENT} - ${LT_AGE}`
AC_SUBST(DLL_VERSION)
```

---

Next: [Strings and Characters](#Strings-and-Characters), Previous: [Native Windows Support](#Native-Windows-Support), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 16 Multithreading [¶](#Multithreading-1)

Multithreading is a programming paradigm. In a *multithreaded
process*, multiple threads execute concurrently (or quasi
concurrently) at different places in the program, and a
*multithreaded program* is one that contains primitives that can
create threads. In contrast, a *single-threaded process* has just
one thread, and a *single-threaded program* contains no
multithreading primitives and so executes entirely in its process’s
main thread.

There are three motivations for using multithreading in a program:

* Exploiting CPU hardware with multiple execution units. Nowadays, many CPUs
  have 2 to 8 execution cores in a single chip. Additionally, often multiple
  CPU chips are combined in a single package. Thus, some CPU packages support
  64 or 96 simultaneous threads of execution.
* Simplifying program architecture. When a program has to read from different
  file descriptors, network sockets, or event channels at the same time, the
  classical single-threaded architecture is to have a main loop which uses
  `select` or `poll` on all the descriptors and then dispatches
  according to from which descriptor input arrived. In a multi-threaded
  program, you allocate one thread for each descriptor, and these threads can
  be programmed and managed independently.
* Offloading work from signal handlers. A signal handler is not allowed to
  call `malloc`; therefore you are very limited in what you can do in
  a signal handler. But a signal handler can notify a thread, and the thread
  can then do the appropriate processing, as complex as it needs to be.

A multithreading API offers

* Primitives for creating threads, for waiting until threads are terminated,
  and for reaping their results.
* Primitives through which different threads can operate on the same data or
  use some data structures for communicating between the threads. These are
  called “mutexes” or “locks”.
* Primitives for executing a certain (initialization) code at most once.
* Primitives for notifying one or more other threads. These are called wait
  queues or “condition variables”.
* Primitives for allowing different threads to have different values for a
  variable. Such a variable is said to reside in “thread-local storage” or
  “thread-specific storage”.
* Primitives for relinquishing control for some time and letting other threads
  go.

Note: Programs that achieve multithreading through OpenMP (cf. the gnulib
module ‘`openmp`’) don’t create and manage their threads themselves.
Nevertheless, they need to use mutexes/locks in many cases.

* [The three multithreading APIs](#Multithreading-APIs)
* [Choosing the right multithreading API](#Choosing-a-multithreading-API)
* [The POSIX multithreading API](#POSIX-multithreading)
* [The ISO C multithreading API](#ISO-C-multithreading)
* [The Gnulib multithreading API](#Gnulib-multithreading)
* [Optimizations of multithreaded code](#Multithreading-Optimizations)

---

Next: [Choosing the right multithreading API](#Choosing-a-multithreading-API), Up: [Multithreading](#Multithreading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 16.1 The three multithreading APIs [¶](#The-three-multithreading-APIs)

Three multithreading APIs are available to Gnulib users:

* POSIX multithreading,
* ISO C multithreading,
* Gnulib multithreading.

They are supported on all platforms that have multithreading in one form or
the other. Currently, these are all platforms supported by Gnulib, except
for Minix.

The main differences are:

* The exit code of a thread is a pointer in the POSIX and Gnulib APIs, but
  only an `int` in the ISO C API.
* The POSIX API has additional facilities for detaching threads, setting the
  priority of a thread, assigning a thread to a certain set of processors,
  and much more.
* In the POSIX and ISO C APIs, most functions have a return code, and you
  are supposed to check the return code; even locking and unlocking a lock
  can fail. In the Gnulib API, many functions don’t have a return code; if
  they cannot complete, the program aborts. This sounds harsh, but such
  aborts have not been reported in 12 years.
* In the ISO C API, the initialization of a statically allocated lock is
  clumsy: You have to initialize it through a once-only function.

---

Next: [The POSIX multithreading API](#POSIX-multithreading), Previous: [The three multithreading APIs](#Multithreading-APIs), Up: [Multithreading](#Multithreading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 16.2 Choosing the right multithreading API [¶](#Choosing-the-right-multithreading-API)

Here are guidelines for determining which multithreading API is best for
your code.

In programs that use advanced POSIX APIs, such as
detached threads (`pthread_detach`),
signal blocking (`pthread_sigmask`),
priorities (`pthread_setschedparam`),
processor affinity (`pthread_setaffinity_np`), it is best to use
the POSIX API. This is because you cannot convert an ISO C `thrd_t`
or a Gnulib `gl_thread_t` to a POSIX `pthread_t`.

In code that is shared with glibc, it is best to use the POSIX API as well.

In libraries, it is best to use the Gnulib API. This is because it gives
the person who builds the library an option
‘`--enable-threads={isoc,posix,windows}`’, that determines on which
native multithreading API of the platform to rely. In other words, with
this choice, you can minimize the amount of glue code that your library
needs to contain.

In the other cases, the POSIX API and the Gnulib API are equally well suited.

The ISO C API is never the best choice, as of this writing (2020).

---

Next: [The ISO C multithreading API](#ISO-C-multithreading), Previous: [Choosing the right multithreading API](#Choosing-a-multithreading-API), Up: [Multithreading](#Multithreading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 16.3 The POSIX multithreading API [¶](#The-POSIX-multithreading-API)

The POSIX multithreading API is documented in POSIX
<https://pubs.opengroup.org/onlinepubs/9799919799/>.

To make use of POSIX multithreading, even on platforms that don’t support it
natively (most prominently, native Windows), use the following Gnulib modules:

| Purpose | Module |
| --- | --- |
| For thread creation and management: | `pthread-thread` |
| For simple and recursive locks: | `pthread-mutex` |
| For read-write locks: | `pthread-rwlock` |
| For once-only execution: | `pthread-once` |
| For “condition variables” (wait queues): | `pthread-cond` |
| For thread-local storage: | `pthread-tss` |
| For relinquishing control: | `sched_yield` |
| For spin locks: | `pthread-spin` |

There is also a convenience module named `pthread` which depends on all
of these (except `sched_yield`); so you don’t need to enumerate these
modules one by one.

---

Next: [The Gnulib multithreading API](#Gnulib-multithreading), Previous: [The POSIX multithreading API](#POSIX-multithreading), Up: [Multithreading](#Multithreading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 16.4 The ISO C multithreading API [¶](#The-ISO-C-multithreading-API)

The ISO C multithreading API is documented in ISO C 11
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf>.

To make use of ISO C multithreading, even on platforms that don’t support it
or have severe bugs, use the following Gnulib modules:

| Purpose | Module |
| --- | --- |
| For thread creation and management: | `thrd` |
| For simple locks, recursive locks, and read-write locks: | `mtx` |
| For once-only execution: | `mtx` |
| For “condition variables” (wait queues): | `cnd` |
| For thread-local storage: | `tss` |

There is also a convenience module named `threads` which depends on all
of these; so you don’t need to enumerate these modules one by one.

---

Next: [Optimizations of multithreaded code](#Multithreading-Optimizations), Previous: [The ISO C multithreading API](#ISO-C-multithreading), Up: [Multithreading](#Multithreading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 16.5 The Gnulib multithreading API [¶](#The-Gnulib-multithreading-API)

The Gnulib multithreading API is documented in the respective include files:

* `<glthread/thread.h>`
* `<glthread/lock.h>`
* `<glthread/once.h>`
* `<glthread/cond.h>`
* `<glthread/tls.h>`
* `<glthread/yield.h>`
* `<glthread/spin.h>`

To make use of Gnulib multithreading, use the following Gnulib modules:

| Purpose | Module |
| --- | --- |
| For thread creation and management: | `thread` |
| For simple locks, recursive locks, and read-write locks: | `lock` |
| For once-only execution: | `once` |
| For “condition variables” (wait queues): | `cond` |
| For thread-local storage: | `tls` |
| For relinquishing control: | `yield` |
| For spin locks: | `spin` |

The Gnulib multithreading supports a configure option
‘`--enable-threads={isoc,posix,windows}`’, that chooses the underlying
thread implementation. Currently (2020):

* `--enable-threads=posix` is supported and is the best choice on all
  platforms except for native Windows. It may also work, to a limited extent,
  on mingw with the `winpthreads` library, but is not recommended there.
* `--enable-threads=windows` is supported and is the best choice on
  native Windows platforms (mingw and MSVC).
  Except that on mingw, in C++ mode, use of the standard C++ library
  (`<iostream>` etc.) is incompatible with this choice.
* `--enable-threads=isoc` is supported on all platforms that have the
  ISO C multithreading API. However, `--enable-threads=posix` is always
  a better choice.

---

Previous: [The Gnulib multithreading API](#Gnulib-multithreading), Up: [Multithreading](#Multithreading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 16.6 Optimizations of multithreaded code [¶](#Optimizations-of-multithreaded-code)

Despite all the optimizations of multithreading primitives that have been
implemented over the years – from
[atomic operations in hardware](https://en.wikipedia.org/wiki/Compare-and-swap),
over [futexes](https://en.wikipedia.org/wiki/Futex) and
[restartable sequences](https://www.efficios.com/blog/2019/02/08/linux-restartable-sequences/)
in the Linux kernel, to lock elision
[[1]](https://lwn.net/Articles/534758/)
[[2]](https://www.gnu.org/software/libc/manual/html_node/Elision-Tunables.html))
– single-threaded programs can still profit performance-wise from the
assertion that they are single-threaded.

Gnulib defines four facilities that help optimizing for the single-threaded
case.

* The Gnulib multithreading API, when used on glibc ≤ 2.32 and \*BSD systems,
  uses weak symbols to detect whether the program is linked with
  `libpthread`. If not, the program has no way to create additional
  threads and must therefore be single-threaded. This optimization applies
  to all the Gnulib multithreading API (locks, thread-local storage, and more).
* The `thread-optim` module,
  on ELF systems (glibc, musl libc, FreeBSD, NetBSD, OpenBSD, Solaris, Android),
  allows your code
  to skip locking between threads (regardless which of the three multithreading
  APIs you use). You need extra code for this: include the
  `"thread-optim.h"` header file, and use the macro `gl_multithreaded`
  like this:

  ```
  bool mt = gl_multithreaded ();
  if (mt) gl_lock_lock (some_lock);
  ...
  if (mt) gl_lock_unlock (some_lock);
  ```
* You may use the `unlocked-io` module if you want the `FILE` stream
  functions `getc`, `putc`, etc. to use unlocked I/O if available,
  throughout the package. Unlocked I/O can improve performance, sometimes
  dramatically. But unlocked I/O is safe only in processes in which
  two threads never simultaneously access the same `FILE` stream.

  You need extra code for this optimization to be effective: include the
  `"unlocked-io.h"` header file. Some Gnulib modules that do operations
  on `FILE` streams have these preparations already included.
* You may define the C macro `GNULIB_REGEX_SINGLE_THREAD`, if all the
  programs in your package invoke the functions of the `regex` module
  only from a single thread.
* You may define the C macro `GNULIB_MBRTOWC_SINGLE_THREAD`, if all the
  programs in your package invoke the functions `mbrtowc`, `mbrtoc32`,
  and the functions of the `regex` module only from a single thread. (The
  `regex` module uses `mbrtowc` under the hood.)
* You may define the C macro `GNULIB_WCHAR_SINGLE_LOCALE`, if all the
  programs in your package set the locale early and
  + don’t change the locale after it has been initialized, and
  + don’t call locale sensitive functions (`mbrtowc`, `wcwidth`, etc.)
    before the locale has been initialized.

  This macro optimizes the functions `mbrtowc`, `mbrtoc32`, and
  `wcwidth`.
  You can get this macro defined by including the Gnulib module
  `wchar-single`.
* You may define the C macro `GNULIB_SIGPROCMASK_SINGLE_THREAD`, if all the
  package’s programs call the following functions only from a single thread:
  `pthread_sigmask`, `sigaction`, `signal`, `sigpending`.
  Regardless of whether this macro is defined, only single-threaded processes
  should call the `sigprocmask` function itself (see [`sigprocmask`](#sigprocmask)).
  The macro is named after `sigprocmask` because it affects
  Gnulib’s implementation of `sigprocmask` on native Windows,
  the only platform where the macro has any effect,
  and the other functions’ Gnulib implementations use Gnulib
  `sigprocmask` on native Windows.
* You may define the C macro `GNULIB_EXCLUDE_SINGLE_THREAD`, if all the
  programs in your package invoke the functions of the `exclude` module
  only from a single thread.

---

Next: [Particular Modules](#Particular-Modules), Previous: [Multithreading](#Multithreading), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 17 Strings and Characters [¶](#Strings-and-Characters-1)

This chapter describes the APIs for strings and characters, provided by Gnulib.

* [Strings](#Strings)
* [Characters](#Characters)

---

Next: [Characters](#Characters), Up: [Strings and Characters](#Strings-and-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 17.1 Strings [¶](#Strings-1)

Several possible representations exist for the representation of strings
in memory of a running C program.

* [The C string representation](#C-strings)
* [Iterating through strings](#Iterating-through-strings)
* [Strings with NUL characters](#Strings-with-NUL-characters)
* [Character and String Functions in C Locale](#String-Functions-in-C-Locale)
* [Comparison of string APIs](#Comparison-of-string-APIs)

---

Next: [Iterating through strings](#Iterating-through-strings), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.1.1 The C string representation [¶](#The-C-string-representation)

The classical representation of a string in C is a sequence of
characters, where each character takes up one or more bytes, followed by
a terminating NUL byte. This representation is used for strings that
are passed by the operating system (in the `argv` argument of
`main`, for example) and for strings that are passed to the
operating system (in system calls such as `open`). The C type to
hold such strings is ‘`char *`’ or, in places where the string shall
not be modified, ‘`const char *`’. There are many C library
functions, standardized by ISO C and POSIX, that assume this
representation of strings.

A *character encoding*, or *encoding* for short, describes
how the elements of a character set are represented as a sequence of
bytes. For example, in the `ASCII` encoding, the UNDERSCORE
character is represented by a single byte, with value 0x5F. As another
example, the COPYRIGHT SIGN character is represented:

* in the `ISO-8859-1` encoding, by the single byte 0xA9,
* in the `UTF-8` encoding, by the two bytes 0xC2 0xA9,
* in the `GB18030` encoding, by the four bytes 0x81 0x30 0x84 0x38.

Note: The ‘`char`’ type may be signed or unsigned, depending on the
platform. When we talk about the "byte 0xA9" we actually mean the
`char` object whose value is `(char) 0xA9`; we omit the cast
to `char` in this documentation, for brevity.

In POSIX, the character encoding is determined by the locale. The
locale is some environmental attribute that the user can choose.

Depending on the encoding, in general, every character is represented by
one or more bytes (up to 4 bytes in practice – but
use `MB_LEN_MAX` instead of the number 4 in the code).

When every character is represented by only 1 byte, we speak of an
“unibyte locale”, otherwise of a “multibyte locale”.

It is important to realize that the majority of Unix installations
nowadays use UTF-8 as locale encoding; therefore, the
majority of users are using multibyte locales.

Three important facts to remember are:

|  |
| --- |
| *A ‘`char`’ is a byte, not a character.* |

As a consequence:

* The [`<ctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html) API, that was designed only with unibyte
  encodings in mind, is useless nowadays for general text processing; it
  does not work in multibyte locales.
* The [`strlen`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strlen.html) function does not return the number of characters
  in a string. Nor does it return the number of screen columns occupied
  by a string after it is output. It merely returns the number of
  *bytes* occupied by a string.
* Truncating a string, for example, with [`strncpy`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strncpy.html), can have the
  effect of truncating it in the middle of a multibyte character. Such
  a string will, when output, have a garbled character at its end, often
  represented by a hollow box.

|  |
| --- |
| *Multibyte does not imply UTF-8 encoding.* |

While UTF-8 is the most common multibyte encoding, GB18030 is also a
supported locale encoding on GNU systems (mostly because it is a Chinese
government standard, last revised in 2022).

|  |
| --- |
| *Searching for a character in a string is not the same as searching for a byte in the string.* |

Take the above example of COPYRIGHT SIGN in the `GB18030` encoding:
A byte search will find the bytes `'0'` and `'8'` in this
string. But a search for the *character* "0" or "8" in the string
"©" must, of course, report “not found”.

As a consequence:

* [`strchr`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strchr.html) and [`strrchr`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strrchr.html) do not work with multibyte
  strings if the locale encoding is GB18030 and the character to be
  searched is a digit.
* [`strstr`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strstr.html) does not work with multibyte strings if the locale
  encoding is different from UTF-8.
* [`strcspn`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcspn.html), [`strpbrk`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strpbrk.html), [`strspn`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strspn.html) cannot work
  correctly in multibyte locales: they assume the second argument is a
  list of single-byte characters. Even in this simple case, they do not
  work with multibyte strings if the locale encoding is GB18030 and one of
  the characters to be searched is a digit.
* [`strsep`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strsep.html) and [`strtok_r`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtok_r.html) do not work with multibyte
  strings unless all of the delimiter characters are ASCII characters
  < 0x30.
* The [`strcasecmp`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcasecmp.html), [`strncasecmp`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strncasecmp.html), and
  [`strcasestr`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcasestr.html) functions do not work with multibyte strings.

Workarounds can be found in Gnulib, in the form of `mbs*` API
functions:

* Gnulib has functions `mbslen` and `mbswidth` that can be used
  instead of [`strlen`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strlen.html) when the number of characters or the
  number of screen columns of a string is requested.
* Gnulib has functions `mbschr` and `mbsrrchr` that are like
  [`strchr`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strchr.html) and [`strrchr`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strrchr.html), but work in multibyte
  locales.
* Gnulib has a function `mbsstr` that is like [`strstr`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strstr.html), but
  works in multibyte locales.
* Gnulib has functions `mbscspn`, `mbspbrk`, `mbsspn` that
  are like [`strcspn`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcspn.html), [`strpbrk`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strpbrk.html), [`strspn`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strspn.html),
  but work in multibyte locales.
* Gnulib has functions `mbssep` and `mbstok_r` that are like
  [`strsep`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strsep.html) and [`strtok_r`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtok_r.html) but work in multibyte
  locales.
* Gnulib has functions `mbscasecmp`, `mbsncasecmp`,
  `mbspcasecmp`, and `mbscasestr` that are like
  [`strcasecmp`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcasecmp.html), [`strncasecmp`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strncasecmp.html), and
  [`strcasestr`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcasestr.html), but work in multibyte locales. Still, the
  function `ulc_casecmp` is preferable to these functions.

|  |
| --- |
| *A C string can contain encoding errors.* |

Not every NUL-terminated byte sequence represents a valid multibyte
string. Byte sequences can contain encoding errors, that is, bytes or
byte sequences that are invalid and do not represent characters.

String functions like `mbscasecmp` and `strcoll` whose
behavior depends on encoding have unspecified behavior on strings
containing encoding errors, unless the behavior is specifically
documented. If an application needs a particular behavior on these
strings it can iterate through them itself, as described in the next
subsection.

---

Next: [Strings with NUL characters](#Strings-with-NUL-characters), Previous: [The C string representation](#C-strings), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.1.2 Iterating through strings [¶](#Iterating-through-strings-1)

For complex string processing, string functions may not be enough, and
you need to iterate through a string while processing each (possibly
multibyte) character or encoding error in turn. Gnulib has several
modules for iterating forward through a string in this way. Backward
iteration, that is, from the string’s end to start, is not provided,
as it is too hairy in general.

* The `mbiter` module iterates through a string whose length
  is already known. The string can contain NULs and encoding errors.
* The `mbiterf` module is like `mbiter`
  except it is more complex and typically faster.
* The `mbuiter` module iterates through a C string whose length
  is not a-priori known. The string can contain encoding errors and is
  terminated by the first NUL.
* The `mbuiterf` module is like `mbuiter`
  except it is more complex and typically faster.
* The `mcel` module is simpler than `mbiter` and `mbuiter`
  and can be faster than even `mbiterf` and `mbuiterf`.
  It can iterate through either strings whose length is known, or
  C strings, or strings terminated by other ASCII characters < 0x30.
* The `mcel-prefer` module is like `mcel` except that it
  causes some other modules to be based on `mcel` instead of
  on the `mbiter` family.

The choice of modules depends on the application’s needs. The
`mbiter` module family is more suitable for applications that
treat some sequences of two or more bytes as a single encoding error,
and for applications that need to support obsolescent encodings on
non-GNU platforms, such as CP864, EBCDIC, Johab, and Shift JIS.
In this module family, `mbuiter` and `mbuiterf` are more
suitable than `mbiter` and `mbiterf` when arguments are C strings,
lengths are not already known, and it is highly likely that only the
first few multibyte characters need to be inspected.

The `mcel` module is simpler and can be faster than the
`mbiter` family, and is more suitable for applications that do
not need the `mbiter` family’s special features.

The `mcel-prefer` module is like `mcel` except that it also
causes some other modules, such as `mbscasecmp`, to use
`mcel` rather than the `mbiter` family. This can be simpler
and faster. However, it does not support the obsolescent encodings,
and it may behave differently on data containing encoding errors where
behavior is unspecified or undefined, because in `mcel` each
encoding error is a single byte whereas in the `mbiter` family a
single encoding error can contain two or more bytes.

If a package uses `mcel-prefer`, it may also want to give
`gnulib-tool` one or more of the options
`--avoid=mbiter`, `--avoid=mbiterf`,
`--avoid=mbuiter` and `--avoid=mbuiterf`,
to avoid packaging modules that are not needed.

---

Next: [Character and String Functions in C Locale](#String-Functions-in-C-Locale), Previous: [Iterating through strings](#Iterating-through-strings), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.1.3 Strings with NUL characters [¶](#Strings-with-NUL-characters-1)

The GNU Coding Standards, section
<https://www.gnu.org/prep/standards/html_node/Semantics.html>,
specifies:

|  |
| --- |
| Utilities reading files should not drop NUL characters, or any other nonprinting characters. |

When it is a requirement to store NUL characters in strings, a variant
of the C strings is needed. Gnulib offers a “string descriptor” type
for this purpose. See [Handling strings with NUL characters](#Handling-strings-with-NUL-characters).

All remarks regarding encodings and multibyte characters in the previous
section apply to string descriptors as well.

---

Next: [Comparison of string APIs](#Comparison-of-string-APIs), Previous: [Strings with NUL characters](#Strings-with-NUL-characters), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.1.4 Character and String Functions in C Locale [¶](#Character-and-String-Functions-in-C-Locale)

The functions in this section are similar to the generic string functions
from the standard C library, except that

* They behave as if the locale was set to the "C" locale, even when the
  locale is different, and/or
* They are specially optimized for the case where all characters are plain
  ASCII characters.

The functions are provided by the following modules.

* [c-ctype](#c_002dctype)
* [c-strcaseeq](#c_002dstrcaseeq)
* [c-strcasecmp](#c_002dstrcasecmp)
* [c-strncasecmp](#c_002dstrncasecmp)
* [c-strcasestr](#c_002dstrcasestr)
* [c-strstr](#c_002dstrstr)
* [c-strtod](#c_002dstrtod)
* [c-strtold](#c_002dstrtold)

---

Next: [c-strcaseeq](#c_002dstrcaseeq), Up: [Character and String Functions in C Locale](#String-Functions-in-C-Locale)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.1.4.1 c-ctype [¶](#c_002dctype-1)

The `c-ctype` module contains functions operating on single-byte
characters, like the functions in `<ctype.h>`, that operate as if the
locale encoding was ASCII. (The "C" locale on many systems has the locale
encoding "ASCII".)

The functions are:

```
extern bool c_isascii (int c);

extern bool c_isalnum (int c);
extern bool c_isalpha (int c);
extern bool c_isblank (int c);
extern bool c_iscntrl (int c);
extern bool c_isdigit (int c);
extern bool c_islower (int c);
extern bool c_isgraph (int c);
extern bool c_isprint (int c);
extern bool c_ispunct (int c);
extern bool c_isspace (int c);
extern bool c_isupper (int c);
extern bool c_isxdigit (int c);

extern int c_tolower (int c);
extern int c_toupper (int c);
```

These functions assign properties only to ASCII characters.

The c argument can be a `char` or `unsigned char` value,
whereas the corresponding functions in `<ctype.h>` take an argument
that is actually an `unsigned char` value.

The `c_is*` functions return ‘`bool`’, where the corresponding
functions in `<ctype.h>` return ‘`int`’ for historical reasons.

Note: The `<ctype.h>` functions support only unibyte locales.

---

Next: [c-strcasecmp](#c_002dstrcasecmp), Previous: [c-ctype](#c_002dctype), Up: [Character and String Functions in C Locale](#String-Functions-in-C-Locale)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.1.4.2 c-strcaseeq [¶](#c_002dstrcaseeq-1)

The `c-strcaseeq` module contains an optimized case-insensitive
string comparison function operating on single-byte character strings, that
operate as if the locale encoding was ASCII.
(The "C" locale on many systems has the locale encoding "ASCII".)

The functions is actually implemented as a macro:

```
extern int STRCASEEQ (const char *s1, const char *s2,
                      int s20, int s21, int s22, int s23, int s24, int s25,
                      int s26, int s27, int s28);
```

s2 should be a short literal ASCII string, and s20, s21, ...
the individual characters of s2.

For case conversion here, only ASCII characters are considered to be
upper case or lower case.

---

Next: [c-strncasecmp](#c_002dstrncasecmp), Previous: [c-strcaseeq](#c_002dstrcaseeq), Up: [Character and String Functions in C Locale](#String-Functions-in-C-Locale)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.1.4.3 c-strcasecmp [¶](#c_002dstrcasecmp-1)

The `c-strcasecmp` module contains a case-insensitive string comparison
function operating on single-byte character strings, like the functions in
`<strings.h>`, that operate as if the locale encoding was ASCII.
(The "C" locale on many systems has the locale encoding "ASCII".)

The function is:

```
extern int c_strcasecmp (const char *s1, const char *s2);
```

For case conversion here, only ASCII characters are considered to be
upper case or lower case.

Note: The function `strcasecmp` from `<strings.h>`
supports only unibyte locales;
for multibyte locales,
you need the function `mbscasecmp`.

---

Next: [c-strcasestr](#c_002dstrcasestr), Previous: [c-strcasecmp](#c_002dstrcasecmp), Up: [Character and String Functions in C Locale](#String-Functions-in-C-Locale)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.1.4.4 c-strncasecmp [¶](#c_002dstrncasecmp-1)

The `c-strncasecmp` module contains a case-insensitive string comparison
function operating on single-byte character strings, like the functions in
`<strings.h>`, that operate as if the locale encoding was ASCII.
(The "C" locale on many systems has the locale encoding "ASCII".)

The function is:

```
extern int c_strncasecmp (const char *s1, const char *s2, size_t n);
```

For case conversion here, only ASCII characters are considered to be
upper case or lower case.

Note: The function `strncasecmp` from `<strings.h>`
supports only unibyte locales;

for multibyte locales,
you need the function `mbsncasecmp` or `mbspcasecmp`.

---

Next: [c-strstr](#c_002dstrstr), Previous: [c-strncasecmp](#c_002dstrncasecmp), Up: [Character and String Functions in C Locale](#String-Functions-in-C-Locale)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.1.4.5 c-strcasestr [¶](#c_002dstrcasestr-1)

The `c-strcasestr` module contains a case-insensitive string search
function operating on single-byte character strings, that operate as if the
locale encoding was ASCII.
(The "C" locale on many systems has the locale encoding "ASCII".)

The function is:

```
extern char *c_strcasestr (const char *haystack, const char *needle);
```

For case conversion here, only ASCII characters are considered to be
upper case or lower case.

Note: The function `strcasestr` from `<string.h>` supports only
unibyte locales;
for multibyte locales, you need the function `mbscasestr`.

---

Next: [c-strtod](#c_002dstrtod), Previous: [c-strcasestr](#c_002dstrcasestr), Up: [Character and String Functions in C Locale](#String-Functions-in-C-Locale)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.1.4.6 c-strstr [¶](#c_002dstrstr-1)

The `c-strstr` module contains a substring search function operating
on single-byte character strings, that operate as if the locale encoding
was ASCII.
(The "C" locale on many systems has the locale encoding "ASCII".)

The function is:

```
extern char *c_strstr (const char *haystack, const char *needle);
```

Note: The function `strstr` from `<string.h>` supports only
unibyte locales;
for multibyte locales, you need the function `mbsstr`.

---

Next: [c-strtold](#c_002dstrtold), Previous: [c-strstr](#c_002dstrstr), Up: [Character and String Functions in C Locale](#String-Functions-in-C-Locale)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.1.4.7 c-strtod [¶](#c_002dstrtod-1)

The `c-strtod` module contains a string to number (‘`double`’)
conversion function operating on single-byte character strings, that operates
as if the locale encoding was ASCII.
(The "C" locale on many systems has the locale encoding "ASCII".)

The function is:

```
extern double c_strtod (const char *string, char **endp);
```

In particular, only a period ‘`.`’ is accepted as decimal point, even
when the current locale’s notion of decimal point is a comma ‘`,`’,
and no characters outside the basic character set are accepted.

On platforms without `strtod_l`, this function is not safe for use in
multi-threaded processes since it calls `setlocale`.

---

Previous: [c-strtod](#c_002dstrtod), Up: [Character and String Functions in C Locale](#String-Functions-in-C-Locale)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.1.4.8 c-strtold [¶](#c_002dstrtold-1)

The `c-strtold` module contains a string to number (‘`long double`’)
conversion function operating on single-byte character strings, that operates
as if the locale encoding was ASCII.
(The "C" locale on many systems has the locale encoding "ASCII".)

The function is:

```
extern long double c_strtold (const char *string, char **endp);
```

In particular, only a period ‘`.`’ is accepted as decimal point, even
when the current locale’s notion of decimal point is a comma ‘`,`’.

---

Previous: [Character and String Functions in C Locale](#String-Functions-in-C-Locale), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 17.1.5 Comparison of string APIs [¶](#Comparison-of-string-APIs-1)

This table summarizes the API functions available for strings, in POSIX
and in Gnulib.

| unibyte strings only | assume C locale | multibyte strings | multibyte strings with NULs | wide character strings | 32-bit wide character strings |
| --- | --- | --- | --- | --- | --- |
| `strlen` | `strlen` | `mbslen` | `sd_length` | `wcslen` | `u32_strlen` |
| `strnlen` | `strnlen` | `mbsnlen` | – | `wcsnlen` | `u32_strnlen`, `u32_mbsnlen` |
| `strcmp` | `strcmp` | `strcmp` | `sd_cmp` | `wcscmp` | `u32_strcmp` |
| `strncmp` | `strncmp` | `strncmp` | – | `wcsncmp` | `u32_strncmp` |
| `strcasecmp` | `strcasecmp` | `mbscasecmp` | – | `wcscasecmp` | `u32_casecmp` |
| `strncasecmp` | `strncasecmp` | `mbsncasecmp`, `mbspcasecmp` | – | `wcsncasecmp` | `u32_casecmp` |
| `strcoll` | `strcmp` | `strcoll` | – | `wcscoll` | `u32_strcoll` |
| `strxfrm` | – | `strxfrm` | – | `wcsxfrm` | – |
| `strchr` | `strchr` | `mbschr` | `sd_index` | `wcschr` | `u32_strchr` |
| `strrchr` | `strrchr` | `mbsrchr` | `sd_last_index` | `wcsrchr` | `u32_strrchr` |
| `strstr` | `strstr` | `mbsstr` | `sd_contains` | `wcsstr` | `u32_strstr` |
| `strcasestr` | `strcasestr` | `mbscasestr` | – | – | – |
| `str_startswith` | `str_startswith` | `mbs_startswith` | `sd_startswith` | – | `u32_startswith` |
| `str_endswith` | `str_endswith` | `mbs_endswith` | `sd_endswith` | – | `u32_endswith` |
| `strspn` | `strspn` | `mbsspn` | – | `wcsspn` | `u32_strspn` |
| `strcspn` | `strcspn` | `mbscspn` | – | `wcscspn` | `u32_strcspn` |
| `strpbrk` | `strpbrk` | `mbspbrk` | – | `wcspbrk` | `u32_strpbrk` |
| `strtok_r` | `strtok_r` | `mbstok_r` | – | `wcstok` | `u32_strtok` |
| `strsep` | `strsep` | `mbssep` | – | – | – |
| `strcpy` | `strcpy` | `strcpy` | `sd_copy` | `wcscpy` | `u32_strcpy` |
| `stpcpy` | `stpcpy` | `stpcpy` | – | `wcpcpy` | `u32_stpcpy` |
| `strncpy` | `strncpy` | `strncpy` | – | `wcsncpy` | `u32_strncpy` |
| `stpncpy` | `stpncpy` | `stpncpy` | – | `wcpncpy` | `u32_stpncpy` |
| `strcat` | `strcat` | `strcat` | `sd_concat` | `wcscat` | `u32_strcat` |
| `strncat` | `strncat` | `strncat` | – | `wcsncat` | `u32_strncat` |
| `free` | `free` | `free` | `sd_free` | `free` | `free` |
| `strdup` | `strdup` | `strdup` | `sd_copy` | `wcsdup` | `u32_strdup` |
| `strndup` | `strndup` | `strndup` | – | – | – |
| `mbswidth` | `mbswidth` | `mbswidth` | – | `wcswidth` | `c32swidth`, `u32_strwidth` |
| `strtol` | `strtol` | `strtol` | – | – | – |
| `strtoul` | `strtoul` | `strtoul` | – | – | – |
| `strtoll` | `strtoll` | `strtoll` | – | – | – |
| `strtoull` | `strtoull` | `strtoull` | – | – | – |
| `strtoimax` | `strtoimax` | `strtoimax` | – | `wcstoimax` | – |
| `strtoumax` | `strtoumax` | `strtoumax` | – | `wcstoumax` | – |
| `strtof` | – | `strtof` | – | – | – |
| `strtod` | `c_strtod` | `strtod` | – | – | – |
| `strtold` | `c_strtold` | `strtold` | – | – | – |
| `strfromf` | – | `strfromf` | – | – | – |
| `strfromd` | – | `strfromd` | – | – | – |
| `strfroml` | – | `strfroml` | – | – | – |
| – | – | – | – | `mbstowcs` | `mbstoc32s` |
| – | – | – | – | `mbsrtowcs` | `mbsrtoc32s` |
| – | – | – | – | `mbsnrtowcs` | `mbsnrtoc32s` |
| – | – | – | – | `wcstombs` | `c32stombs` |
| – | – | – | – | `wcsrtombs` | `c32srtombs` |
| – | – | – | – | `wcsnrtombs` | `c32snrtombs` |

---

Previous: [Strings](#Strings), Up: [Strings and Characters](#Strings-and-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
