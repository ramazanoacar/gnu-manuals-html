### 10.12 Functions in `<fenv.h>` [¶](#Functions-in-_003cfenv_002eh_003e-1)

* [`feclearexcept`](#feclearexcept)
* [`fegetenv`](#fegetenv)
* [`fegetexceptflag`](#fegetexceptflag)
* [`fegetmode`](#fegetmode)
* [`fegetround`](#fegetround)
* [`feholdexcept`](#feholdexcept)
* [`feraiseexcept`](#feraiseexcept)
* [`fesetenv`](#fesetenv)
* [`fesetexcept`](#fesetexcept)
* [`fesetexceptflag`](#fesetexceptflag)
* [`fesetmode`](#fesetmode)
* [`fesetround`](#fesetround)
* [`fetestexcept`](#fetestexcept)
* [`fetestexceptflag`](#fetestexceptflag)
* [`feupdateenv`](#feupdateenv)

---

Next: [`fegetenv`](#fegetenv), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.1 `feclearexcept` [¶](#feclearexcept-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/feclearexcept.html>

Gnulib module: fenv-exceptions-tracking-c99

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, Solaris 9, Cygwin 1.7.7, MSVC 9, Android 4.4.
* This function is broken on some platforms:
  OpenBSD 7.4/mips64, Minix 3.3/i386.

Portability problems not fixed by Gnulib:

---

Next: [`fegetexceptflag`](#fegetexceptflag), Previous: [`feclearexcept`](#feclearexcept), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.2 `fegetenv` [¶](#fegetenv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fegetenv.html>

Gnulib module: fenv-environment

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, Solaris 9, Cygwin 1.7.7, MSVC 9, Android 4.4.
* This function clears all exception trap bits on some platforms:
  glibc 2.19/x86\_64,
  Mac OS X 10.5/i386, Mac OS X 10.5/x86\_64.
* This function does not save the exception trap bits on some platforms:
  AIX 7.3.

Portability problems not fixed by Gnulib:

---

Next: [`fegetmode`](#fegetmode), Previous: [`fegetenv`](#fegetenv), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.3 `fegetexceptflag` [¶](#fegetexceptflag-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fegetexceptflag.html>

Gnulib module: fenv-exceptions-state-c99

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, Solaris 9, Cygwin 1.7.7, MSVC 9, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`fegetround`](#fegetround), Previous: [`fegetexceptflag`](#fegetexceptflag), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.4 `fegetmode` [¶](#fegetmode-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Control-Functions.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`feholdexcept`](#feholdexcept), Previous: [`fegetmode`](#fegetmode), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.5 `fegetround` [¶](#fegetround-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fegetround.html>

Gnulib module: fenv-rounding

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, AIX 5.1, Solaris 9, Cygwin 1.7.7, MSVC 9, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`feraiseexcept`](#feraiseexcept), Previous: [`fegetround`](#fegetround), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.6 `feholdexcept` [¶](#feholdexcept-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/feholdexcept.html>

Gnulib module: fenv-environment

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, AIX 5.1, Solaris 9, Cygwin 1.7.7, MSVC 9, Android 4.4.
* This function does not work on some platforms:
  glibc 2.5,
  FreeBSD 12.2/arm64.
* This function does not save the exception trap bits on some platforms:
  AIX 7.3.
* This function does not clear the exception trap bits on some platforms:
  musl libc, mingw.
* This function may fail on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`fesetenv`](#fesetenv), Previous: [`feholdexcept`](#feholdexcept), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.7 `feraiseexcept` [¶](#feraiseexcept-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/feraiseexcept.html>

Gnulib module: fenv-exceptions-tracking-c99

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, Solaris 9, Cygwin 1.7.7, MSVC 9, Android 4.4.
* This function does not detect failures on
  glibc 2.19/arm.
* This function does not trigger traps on
  Cygwin 3.4.x/x86\_64.

Portability problems not fixed by Gnulib:

---

Next: [`fesetexcept`](#fesetexcept), Previous: [`feraiseexcept`](#feraiseexcept), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.8 `fesetenv` [¶](#fesetenv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fesetenv.html>

Gnulib module: fenv-environment

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, Solaris 9, Cygwin 1.7.7, MSVC 9, Android 4.4.
* This function does not work on some platforms:
  FreeBSD 12.2/arm64,
  mingw 13.
* This function does not restore the floating-point exception trap bits
  on some platforms:
  musl libc/i386, musl libc/x86\_64, AIX 7.3, Solaris 10 and 11, MSVC 14.
* The macro `FE_DFL_ENV` cannot be used because it leads to a link error
  on some platforms:
  NetBSD 10.0/hppa,
  NetBSD 10.0/sparc,
  Cygwin 2.9.0.
* This function, when called with `FE_DFL_ENV` argument, has no effect
  on the x86 `mxcsr` register and thus on floating-point operations
  performed in the SSE unit on some platforms:
  mingw 10.

Portability problems not fixed by Gnulib:

---

Next: [`fesetexceptflag`](#fesetexceptflag), Previous: [`fesetenv`](#fesetenv), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.9 `fesetexcept` [¶](#fesetexcept-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Status-bit-operations.html>.

Gnulib module: fenv-exceptions-tracking-c23

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function triggers floating-point exception traps although it shouldn’t, on
  glibc 2.37/i386,
  glibc 2.37/powerpc.

Portability problems not fixed by Gnulib:

---

Next: [`fesetmode`](#fesetmode), Previous: [`fesetexcept`](#fesetexcept), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.10 `fesetexceptflag` [¶](#fesetexceptflag-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fesetexceptflag.html>

Gnulib module: fenv-exceptions-state-c99

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, Solaris 9, Cygwin 1.7.7, MSVC 9, Android 4.4.
* This function triggers floating-point exception traps although it shouldn’t, on
  glibc 2.37/i386, glibc 2.37/x86\_64,
  glibc 2.37/powerpc,
  musl libc, Mac OS X 10.5, Minix 3.3, mingw, Haiku.
* This function clears too many floating-point exception flags on
  glibc 2.37/alpha.
* This function loses the `FE_INVALID` flag on
  FreeBSD 14.0/powerpc64,
  NetBSD 9.3/powerpc.
* This function is also buggy on
  AIX 7.3.1.

Portability problems not fixed by Gnulib:

---

Next: [`fesetround`](#fesetround), Previous: [`fesetexceptflag`](#fesetexceptflag), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.11 `fesetmode` [¶](#fesetmode-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Control-Functions.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`fetestexcept`](#fetestexcept), Previous: [`fesetmode`](#fesetmode), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.12 `fesetround` [¶](#fesetround-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fesetround.html>

Gnulib module: fenv-rounding

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, AIX 5.1, Solaris 9, Cygwin 1.7.7, MSVC 9, Android 4.4.
* This function does not work on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

* The rounding mode has no effect on `long double` operations on some platforms:
  Android/arm64, FreeBSD/arm64, NetBSD/sparc64, OpenBSD/arm64, OpenBSD/mips64.

---

Next: [`fetestexceptflag`](#fetestexceptflag), Previous: [`fesetround`](#fesetround), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.13 `fetestexcept` [¶](#fetestexcept-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fetestexcept.html>

Gnulib module: fenv-exceptions-tracking-c99

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, Solaris 9, Cygwin 1.7.7, MSVC 9, Android 4.4.
* This function clears the floating-point exception trap bits on some platforms:
  NetBSD 9.3/x86\_64.
* This function is broken on some platforms:
  Minix 3.3/i386.

Portability problems not fixed by Gnulib:

---

Next: [`feupdateenv`](#feupdateenv), Previous: [`fetestexcept`](#fetestexcept), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.14 `fetestexceptflag` [¶](#fetestexceptflag-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Status-bit-operations.html>.

Gnulib module: fenv-exceptions-state-c23

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.24, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Previous: [`fetestexceptflag`](#fetestexceptflag), Up: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.12.15 `feupdateenv` [¶](#feupdateenv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/feupdateenv.html>

Gnulib module: fenv-environment

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, Solaris 9, Cygwin 1.7.7, MSVC 14, Android 4.4.
* This function crashes when given the argument `FE_DFL_ENV`
  on some platforms:
  glibc 2.37/riscv64.
* This function does not work on some platforms:
  glibc 2.5/ia64,
  FreeBSD 12.2/arm64,
  mingw 13.
* This function forgets about the currently set floating-point exception flags
  on some platforms:
  Mac OS X 10.5/i386, Mac OS X 10.5/x86\_64.
* This function does not restore the floating-point exception trap bits
  on some platforms:
  musl libc/i386, musl libc/x86\_64, AIX 7.3, Solaris 10 and 11, mingw 10.
* This function does not trigger traps on
  glibc 2.37/hppa,
  musl libc/s390x,
  NetBSD 10.0/powerpc.
* This function may fail on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [Functions in `<fnmatch.h>`](#Functions-in-_003cfnmatch_002eh_003e), Previous: [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.13 Functions in `<fmtmsg.h>` [¶](#Functions-in-_003cfmtmsg_002eh_003e-1)

* [`fmtmsg`](#fmtmsg)

---

Up: [Functions in `<fmtmsg.h>`](#Functions-in-_003cfmtmsg_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.13.1 `fmtmsg` [¶](#fmtmsg-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fmtmsg.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Functions in `<ftw.h>`](#Functions-in-_003cftw_002eh_003e), Previous: [Functions in `<fmtmsg.h>`](#Functions-in-_003cfmtmsg_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.14 Functions in `<fnmatch.h>` [¶](#Functions-in-_003cfnmatch_002eh_003e-1)

* [`fnmatch`](#fnmatch)

---

Up: [Functions in `<fnmatch.h>`](#Functions-in-_003cfnmatch_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.14.1 `fnmatch` [¶](#fnmatch-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fnmatch.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-fnmatch-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Wildcard-Matching.html>,
* [man fnmatch](https://www.kernel.org/doc/man-pages/online/pages/man3/fnmatch.3.html).

Gnulib module: fnmatch or fnmatch-gnu

Portability problems fixed by either Gnulib module `fnmatch` or `fnmatch-gnu`:

* This function is missing on some platforms:
  mingw, MSVC 14.
* The `"?"` pattern character fails to match characters outside the
  single-byte range on some platforms:
  NetBSD 10.0,
  Android 13.
* The `"?"` pattern character fails to match characters outside the
  Unicode BMP on some platforms:
  Solaris 10, Cygwin 3.4.6,
  MSVC,
  32-bit AIX.
* In the pattern, negated character ranges (such as `[!a-z]`) are not
  supported on some platforms:
  Solaris 11.4.
* In the pattern, character classes (such as `[:alnum:]`) inside
  bracket expressions are not supported on some platforms:
  FreeBSD 13.2,
  NetBSD 10.0, Solaris 11 OpenIndiana, Cygwin 3.4.6.
* In the pattern, character classes (such as `[:alnum:]`) inside
  bracket expressions fail to match characters outside the single-byte
  range on some platforms:
  Android 13.
* The character class `[:cntrl:]` matches the empty string
  on some platforms:
  Solaris 11.4.
* Ranges that start or end with a backslash don’t work right on some platforms:
  glibc 2.3.3.
* In the pattern, an opening bracket without closing bracket does not match
  a literal `'['` on some platforms:
  glibc 2.12,
  macOS 12.5,
  NetBSD 10.0.

Portability problems fixed by Gnulib module `fnmatch-gnu`:

* This function does not support the flags `FNM_LEADING_DIR` and
  `FNM_CASEFOLD` on some platforms:
  AIX 7.2, HP-UX 11.31, Solaris 10.
* The flag `FNM_CASEFOLD` does not work in many situations on some platforms:
  NetBSD 10.0.
* The flag `FNM_CASEFOLD` does not work for multibyte characters
  consisting of more than one byte on some platforms:
  Android 13.
* This function does not support the flag `FNM_EXTMATCH`
  on all non-glibc platforms:
  musl libc, macOS 12.5, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.2, Minix 3.3, AIX 7.2, HP-UX 11.31, Solaris 11.4, Cygwin 3.4.6, Android 13.
* This function does not support the flag `FNM_FILE_NAME` as an
  alias of `FNM_PATHNAME` on some platforms:
  NetBSD 10.0, AIX 7.2, HP-UX 11.31, Solaris 10.

Portability problems not fixed by Gnulib:

Note: Gnulib’s replacement function has some limitations:

* It does not implement patterns with
  collating elements (such as `"[[.ch.]]"`) or
  equivalence classes (such as `"[[=a=]]"`).

---

Next: [Functions in `<glob.h>`](#Functions-in-_003cglob_002eh_003e), Previous: [Functions in `<fnmatch.h>`](#Functions-in-_003cfnmatch_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.15 Functions in `<ftw.h>` [¶](#Functions-in-_003cftw_002eh_003e-1)

* [`nftw`](#nftw)

---

Up: [Functions in `<ftw.h>`](#Functions-in-_003cftw_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.15.1 `nftw` [¶](#nftw-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nftw.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  correctly report the size of files or block devices 2 GiB and larger.
  See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 3.0, Minix 3.1.8, mingw, MSVC 14, Android 4.1.

---

Next: [Functions in `<grp.h>`](#Functions-in-_003cgrp_002eh_003e), Previous: [Functions in `<ftw.h>`](#Functions-in-_003cftw_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.16 Functions in `<glob.h>` [¶](#Functions-in-_003cglob_002eh_003e-1)

* [`glob`](#glob)
* [`globfree`](#globfree)

---

Next: [`globfree`](#globfree), Up: [Functions in `<glob.h>`](#Functions-in-_003cglob_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.16.1 `glob` [¶](#glob-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/glob.html>

Gnulib module: glob

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 8.1.
* This function does not list symbolic links to nonexistent files among the results,
  on some platforms:
  glibc 2.26, AIX 7.2, HP-UX 11, Solaris 11.4.
* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on huge directories 2 GiB and larger.
  See [Large File Support](#Large-File-Support).
* This function makes a recursive call for every trailing `/` and
  every `/` following a wildcard character on some platforms, which
  can cause the stack to overflow:
  glibc 2.42,
  Cygwin, Android.

Portability problems not fixed by Gnulib:

* Some platforms may store additional flags in the `gl_flags` field.

---

Previous: [`glob`](#glob), Up: [Functions in `<glob.h>`](#Functions-in-_003cglob_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.16.2 `globfree` [¶](#globfree-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/globfree.html>

Gnulib module: glob

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

---

Next: [Functions in `<iconv.h>`](#Functions-in-_003ciconv_002eh_003e), Previous: [Functions in `<glob.h>`](#Functions-in-_003cglob_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.17 Functions in `<grp.h>` [¶](#Functions-in-_003cgrp_002eh_003e-1)

* [`endgrent`](#endgrent)
* [`getgrent`](#getgrent)
* [`getgrgid`](#getgrgid)
* [`getgrgid_r`](#getgrgid_005fr)
* [`getgrnam`](#getgrnam)
* [`getgrnam_r`](#getgrnam_005fr)
* [`setgrent`](#setgrent)

---

Next: [`getgrent`](#getgrent), Up: [Functions in `<grp.h>`](#Functions-in-_003cgrp_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.17.1 `endgrent` [¶](#endgrent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/endgrent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 7.1.

---

Next: [`getgrgid`](#getgrgid), Previous: [`endgrent`](#endgrent), Up: [Functions in `<grp.h>`](#Functions-in-_003cgrp_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.17.2 `getgrent` [¶](#getgrent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getgrent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 7.1.

---

Next: [`getgrgid_r`](#getgrgid_005fr), Previous: [`getgrent`](#getgrent), Up: [Functions in `<grp.h>`](#Functions-in-_003cgrp_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.17.3 `getgrgid` [¶](#getgrgid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getgrgid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`getgrnam`](#getgrnam), Previous: [`getgrgid`](#getgrgid), Up: [Functions in `<grp.h>`](#Functions-in-_003cgrp_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.17.4 `getgrgid_r` [¶](#getgrgid_005fr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getgrgid_r.html>

Gnulib module: extensions

Portability problems fixed by Gnulib:

* This function has an incompatible declaration on some platforms:
  Solaris 11.3 (when `_POSIX_PTHREAD_SEMANTICS` is not defined).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 6.0.

---

Next: [`getgrnam_r`](#getgrnam_005fr), Previous: [`getgrgid_r`](#getgrgid_005fr), Up: [Functions in `<grp.h>`](#Functions-in-_003cgrp_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.17.5 `getgrnam` [¶](#getgrnam-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getgrnam.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`setgrent`](#setgrent), Previous: [`getgrnam`](#getgrnam), Up: [Functions in `<grp.h>`](#Functions-in-_003cgrp_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.17.6 `getgrnam_r` [¶](#getgrnam_005fr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getgrnam_r.html>

Gnulib module: extensions

Portability problems fixed by Gnulib:

* This function has an incompatible declaration on some platforms:
  Solaris 11.3 (when `_POSIX_PTHREAD_SEMANTICS` is not defined).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 6.0.

---

Previous: [`getgrnam_r`](#getgrnam_005fr), Up: [Functions in `<grp.h>`](#Functions-in-_003cgrp_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.17.7 `setgrent` [¶](#setgrent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setgrent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 7.1.

---

Next: [Functions in `<inttypes.h>`](#Functions-in-_003cinttypes_002eh_003e), Previous: [Functions in `<grp.h>`](#Functions-in-_003cgrp_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.18 Functions in `<iconv.h>` [¶](#Functions-in-_003ciconv_002eh_003e-1)

* [`iconv`](#iconv)
* [`iconv_close`](#iconv_005fclose)
* [`iconv_open`](#iconv_005fopen)

---

Next: [`iconv_close`](#iconv_005fclose), Up: [Functions in `<iconv.h>`](#Functions-in-_003ciconv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.18.1 `iconv` [¶](#iconv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iconv.html>

Gnulib module: iconv

Portability problems fixed by Gnulib:

* GNU libiconv is not found if installed in `$PREFIX/lib`.

Portability problems handled by Gnulib
(in the sense that `HAVE_ICONV` does not get defined if the system’s
`iconv` function has this problem):

* Failures are not distinguishable from successful returns on some platforms:
  macOS 14.4, AIX 5.1..7.2, Solaris 10.
* A buffer overrun can occur on some platforms:
  AIX 6.1..7.1.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 8.1,
  when GNU libiconv is not installed.
* This function was not correctly implemented in glibc versions before 2.2.
* When `iconv` encounters an input character that is valid but that
  cannot be converted to the output character set, glibc’s and GNU libiconv’s
  `iconv` stop the conversion. Some other implementations put an
  implementation-defined character into the output buffer.

  Gnulib provides higher-level facilities `striconv` and `striconveh`
  (wrappers around `iconv`) that deal with conversion errors in a platform
  independent way.
* This function returns a positive return value, instead of zero, when
  converting from ISO-8859-1 to UTF-8 on HP-UX 11.

---

Next: [`iconv_open`](#iconv_005fopen), Previous: [`iconv`](#iconv), Up: [Functions in `<iconv.h>`](#Functions-in-_003ciconv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.18.2 `iconv_close` [¶](#iconv_005fclose-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iconv_close.html>

Gnulib module: iconv

Portability problems fixed by Gnulib:

* GNU libiconv is not found if installed in `$PREFIX/lib`.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 8.1,
  when GNU libiconv is not installed.

---

Previous: [`iconv_close`](#iconv_005fclose), Up: [Functions in `<iconv.h>`](#Functions-in-_003ciconv_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.18.3 `iconv_open` [¶](#iconv_005fopen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/iconv_open.html>

Gnulib module: iconv, iconv\_open, iconv\_open-utf

Portability problems fixed by either Gnulib module `iconv` or `iconv_open`:

* GNU libiconv is not found if installed in `$PREFIX/lib`.

Portability problems handled by either Gnulib module `iconv` or `iconv_open`
(in the sense that `HAVE_ICONV` does not get defined if the system’s
`iconv_open` function has this problem):

* No converter from EUC-JP to UTF-8 is provided on some platforms:
  HP-UX 11.

Portability problems fixed by Gnulib module `iconv_open`:

* This function recognizes only non-standard aliases for many encodings (not
  the IANA registered encoding names) on many platforms:
  AIX 5.1, HP-UX 11, Solaris 11 2010-11, z/OS.

Portability problems fixed by Gnulib module `iconv_open-utf`:

* This function does not support the encodings UTF-16BE, UTF-16LE, UTF-32BE,
  UTF-32LE on many platforms:
  AIX 5.1, HP-UX 11.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 8.1,
  when GNU libiconv is not installed.
* For some encodings A and B, this function cannot convert directly from A to B,
  although an indirect conversion from A through UTF-8 to B is possible. This
  occurs on some platforms: Solaris 11 2010-11.
  Gnulib provides a higher-level
  facility `striconveh` (a wrapper around `iconv`) that deals with
  this problem.
* The set of supported encodings and conversions is system dependent.

---

Next: [Functions in `<langinfo.h>`](#Functions-in-_003clanginfo_002eh_003e), Previous: [Functions in `<iconv.h>`](#Functions-in-_003ciconv_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.19 Functions in `<inttypes.h>` [¶](#Functions-in-_003cinttypes_002eh_003e-1)

* [`imaxabs`](#imaxabs)
* [`imaxdiv`](#imaxdiv)
* [`strtoimax`](#strtoimax)
* [`strtoumax`](#strtoumax)
* [`wcstoimax`](#wcstoimax)
* [`wcstoumax`](#wcstoumax)
* [`umaxabs`](#umaxabs)

---

Next: [`imaxdiv`](#imaxdiv), Up: [Functions in `<inttypes.h>`](#Functions-in-_003cinttypes_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.19.1 `imaxabs` [¶](#imaxabs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/imaxabs.html>

Gnulib module: imaxabs

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11.23, Solaris 9, MSVC 9, Android 4.3.

Portability problems not fixed by Gnulib:

---

Next: [`strtoimax`](#strtoimax), Previous: [`imaxabs`](#imaxabs), Up: [Functions in `<inttypes.h>`](#Functions-in-_003cinttypes_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.19.2 `imaxdiv` [¶](#imaxdiv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/imaxdiv.html>

Gnulib module: imaxdiv

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11.23, Solaris 9, MSVC 9, Android 4.3.

Portability problems not fixed by Gnulib:

---

Next: [`strtoumax`](#strtoumax), Previous: [`imaxdiv`](#imaxdiv), Up: [Functions in `<inttypes.h>`](#Functions-in-_003cinttypes_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.19.3 `strtoimax` [¶](#strtoimax-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtoimax.html>

Gnulib module: strtoimax

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, Solaris 9, MSVC 9.
* This function fails on valid input strings on some platforms:
  AIX 5.1.

Portability problems not fixed by Gnulib:

* This function is only defined as a macro on some platforms:
  HP-UX 11.11.
* If no conversion could be performed, this function sets `errno`
  to `EINVAL` on some platforms. Portable programs should check
  for no conversion by comparing `str` to `*endptr`, not by
  inspecting `errno`.

---

Next: [`wcstoimax`](#wcstoimax), Previous: [`strtoimax`](#strtoimax), Up: [Functions in `<inttypes.h>`](#Functions-in-_003cinttypes_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.19.4 `strtoumax` [¶](#strtoumax-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtoumax.html>

Gnulib module: strtoumax

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, AIX 5.1, Solaris 9, MSVC 9.

Portability problems not fixed by Gnulib:

* This function is only defined as a macro on some platforms:
  HP-UX 11.11.
* If no conversion could be performed, this function sets `errno`
  to `EINVAL` on some platforms. Portable programs should check
  for no conversion by comparing `str` to `*endptr`, not by
  inspecting `errno`.

---

Next: [`wcstoumax`](#wcstoumax), Previous: [`strtoumax`](#strtoumax), Up: [Functions in `<inttypes.h>`](#Functions-in-_003cinttypes_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.19.5 `wcstoimax` [¶](#wcstoimax-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcstoimax.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11.11, Solaris 9, Cygwin 1.5.x, MSVC 9, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Next: [`umaxabs`](#umaxabs), Previous: [`wcstoimax`](#wcstoimax), Up: [Functions in `<inttypes.h>`](#Functions-in-_003cinttypes_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.19.6 `wcstoumax` [¶](#wcstoumax-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcstoumax.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11.11, Solaris 9, Cygwin 1.5.x, MSVC 9, Android 4.4.
* On Windows and 32-bit AIX platforms, `wchar_t` is a 16-bit type and therefore cannot
  accommodate all Unicode characters.

---

Previous: [`wcstoumax`](#wcstoumax), Up: [Functions in `<inttypes.h>`](#Functions-in-_003cinttypes_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.19.7 `umaxabs` [¶](#umaxabs-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Absolute-Value.html>,
* [man umaxabs](https://www.kernel.org/doc/man-pages/online/pages/man3/umaxabs.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most platforms:
  glibc 2.42, macOS 15, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.7, Minix, AIX 7.3, HP-UX 11.31, Solaris 11.4, Cygwin, mingw, MSVC 14.

---

Next: [Functions in `<libgen.h>`](#Functions-in-_003clibgen_002eh_003e), Previous: [Functions in `<inttypes.h>`](#Functions-in-_003cinttypes_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.20 Functions in `<langinfo.h>` [¶](#Functions-in-_003clanginfo_002eh_003e-1)

* [`nl_langinfo`](#nl_005flanginfo)
* [`nl_langinfo_l`](#nl_005flanginfo_005fl)

---

Next: [`nl_langinfo_l`](#nl_005flanginfo_005fl), Up: [Functions in `<langinfo.h>`](#Functions-in-_003clanginfo_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.20.1 `nl_langinfo` [¶](#nl_005flanginfo-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nl_langinfo.html>

Gnulib module: nl\_langinfo

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 7.1.
* The constant `CODESET` is not supported on some platforms:
  OpenBSD 3.8.
* The constants `ALTMON_1` to `ALTMON_12` are not supported on some
  platforms:
  glibc 2.26, musl libc, macOS 14, NetBSD 8.0, OpenBSD 7.9, AIX 7.2, HP-UX 11.31, Solaris 11.4, Haiku, Cygwin 2.9.
* The constants `ABALTMON_1` to `ABALTMON_12` are not supported on many
  platforms:
  glibc 2.42, musl libc, macOS 14, NetBSD 10.0, OpenBSD 7.6, AIX 7.3, HP-UX 11.31, Solaris 11.4, Haiku, Cygwin 3.5.
* The constants `ERA`, `ERA_D_FMT`, `ERA_D_T_FMT`,
  `ERA_T_FMT`, `ALT_DIGITS` are not supported on some platforms:
  OpenBSD 7.9.
* This function is not thread-safe on some platforms:
  macOS 26, Solaris 11.3.

Portability problems not fixed by Gnulib:

* On Cygwin 1.5.x, which doesn’t have locales, `nl_langinfo(CODESET)` always
  returns `"US-ASCII"`.
* On Cygwin 1.7.0, only the charset portion of a locale designation is honored.
* On NetBSD 5.0, in some locales, `nl_langinfo(CRNCYSTR)` returns the
  empty string, although the local currency symbol, as returned by
  `localeconv()->currency_symbol`, is non-empty.
* On NetBSD 5.1, in the "C" locale, the results of
  `nl_langinfo(ABMON_1)` ... `nl_langinfo(ABMON_12)` are full month
  names, not abbreviated month names.

---

Previous: [`nl_langinfo`](#nl_005flanginfo), Up: [Functions in `<langinfo.h>`](#Functions-in-_003clanginfo_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.20.2 `nl_langinfo_l` [¶](#nl_005flanginfo_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nl_langinfo_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 7.1.

---

Next: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e), Previous: [Functions in `<langinfo.h>`](#Functions-in-_003clanginfo_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.21 Functions in `<libgen.h>` [¶](#Functions-in-_003clibgen_002eh_003e-1)

* [`basename`](#basename)
* [`dirname`](#dirname)

---

Next: [`dirname`](#dirname), Up: [Functions in `<libgen.h>`](#Functions-in-_003clibgen_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.21.1 `basename` [¶](#basename-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/basename.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-basename-3.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* glibc and Android have two different functions `basename`: the POSIX
  version and the GNU version.
* `basename` assumes file names in POSIX syntax; it does not work with file
  names in Windows syntax.

The Gnulib module `basename-lgpl` provides similar API, with a function
`last_component`, that also works with Windows file names.

---

Previous: [`basename`](#basename), Up: [Functions in `<libgen.h>`](#Functions-in-_003clibgen_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.21.2 `dirname` [¶](#dirname-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dirname.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* `dirname` assumes file names in POSIX syntax; it does not work with file
  names in Windows syntax.

The Gnulib module `dirname` provides similar API, with functions
`dir_name` and `mdir_name`, that also works with Windows
file names.

---

Next: [Functions in `<locale.h>`](#Functions-in-_003clocale_002eh_003e), Previous: [Functions in `<libgen.h>`](#Functions-in-_003clibgen_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.22 Functions in `<libintl.h>` [¶](#Functions-in-_003clibintl_002eh_003e-1)

* [`bind_textdomain_codeset`](#bind_005ftextdomain_005fcodeset)
* [`bindtextdomain`](#bindtextdomain)
* [`dcgettext`](#dcgettext)
* [`dcgettext_l`](#dcgettext_005fl)
* [`dcngettext`](#dcngettext)
* [`dcngettext_l`](#dcngettext_005fl)
* [`dgettext`](#dgettext)
* [`dgettext_l`](#dgettext_005fl)
* [`dngettext`](#dngettext)
* [`dngettext_l`](#dngettext_005fl)
* [`gettext`](#gettext)
* [`gettext_l`](#gettext_005fl)
* [`ngettext`](#ngettext)
* [`ngettext_l`](#ngettext_005fl)
* [`textdomain`](#textdomain)

---

Next: [`bindtextdomain`](#bindtextdomain), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.1 `bind_textdomain_codeset` [¶](#bind_005ftextdomain_005fcodeset-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/bind_textdomain_codeset.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Charset-conversion-in-gettext.html>,
* [man bind\_textdomain\_codeset](https://www.kernel.org/doc/man-pages/online/pages/man3/bind_textdomain_codeset.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`dcgettext`](#dcgettext), Previous: [`bind_textdomain_codeset`](#bind_005ftextdomain_005fcodeset), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.2 `bindtextdomain` [¶](#bindtextdomain-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/bindtextdomain.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Locating-gettext-catalog.html>,
* [man bindtextdomain](https://www.kernel.org/doc/man-pages/online/pages/man3/bindtextdomain.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`dcgettext_l`](#dcgettext_005fl), Previous: [`bindtextdomain`](#bindtextdomain), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.3 `dcgettext` [¶](#dcgettext-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dcgettext.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Translation-with-gettext.html>,
* [man dcgettext](https://www.kernel.org/doc/man-pages/online/pages/man3/dcgettext.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function does not treat a `LANGUAGE` environment variable
  with an empty value like an unset `LANGUAGE` environment variable
  on some platforms:
  NetBSD 10.0.

---

Next: [`dcngettext`](#dcngettext), Previous: [`dcgettext`](#dcgettext), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.4 `dcgettext_l` [¶](#dcgettext_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dcgettext_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

---

Next: [`dcngettext_l`](#dcngettext_005fl), Previous: [`dcgettext_l`](#dcgettext_005fl), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.5 `dcngettext` [¶](#dcngettext-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dcngettext.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Advanced-gettext-functions.html>,
* [man dcngettext](https://www.kernel.org/doc/man-pages/online/pages/man3/dcngettext.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function does not treat a `LANGUAGE` environment variable
  with an empty value like an unset `LANGUAGE` environment variable
  on some platforms:
  NetBSD 10.0.

---

Next: [`dgettext`](#dgettext), Previous: [`dcngettext`](#dcngettext), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.6 `dcngettext_l` [¶](#dcngettext_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dcngettext_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.41, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

---

Next: [`dgettext_l`](#dgettext_005fl), Previous: [`dcngettext_l`](#dcngettext_005fl), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.7 `dgettext` [¶](#dgettext-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dgettext.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Translation-with-gettext.html>,
* [man dgettext](https://www.kernel.org/doc/man-pages/online/pages/man3/dgettext.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function does not treat a `LANGUAGE` environment variable
  with an empty value like an unset `LANGUAGE` environment variable
  on some platforms:
  NetBSD 10.0.

---

Next: [`dngettext`](#dngettext), Previous: [`dgettext`](#dgettext), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.8 `dgettext_l` [¶](#dgettext_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dgettext_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

---

Next: [`dngettext_l`](#dngettext_005fl), Previous: [`dgettext_l`](#dgettext_005fl), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.9 `dngettext` [¶](#dngettext-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dngettext.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Advanced-gettext-functions.html>,
* [man dngettext](https://www.kernel.org/doc/man-pages/online/pages/man3/dngettext.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function does not treat a `LANGUAGE` environment variable
  with an empty value like an unset `LANGUAGE` environment variable
  on some platforms:
  NetBSD 10.0.

---

Next: [`gettext`](#gettext), Previous: [`dngettext`](#dngettext), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.10 `dngettext_l` [¶](#dngettext_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dngettext_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

---

Next: [`gettext_l`](#gettext_005fl), Previous: [`dngettext_l`](#dngettext_005fl), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.11 `gettext` [¶](#gettext-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/gettext.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Translation-with-gettext.html>,
* [man gettext](https://www.kernel.org/doc/man-pages/online/pages/man3/gettext.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function does not treat a `LANGUAGE` environment variable
  with an empty value like an unset `LANGUAGE` environment variable
  on some platforms:
  NetBSD 10.0.

---

Next: [`ngettext`](#ngettext), Previous: [`gettext`](#gettext), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.12 `gettext_l` [¶](#gettext_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/gettext_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

---

Next: [`ngettext_l`](#ngettext_005fl), Previous: [`gettext_l`](#gettext_005fl), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.13 `ngettext` [¶](#ngettext-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ngettext.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Advanced-gettext-functions.html>,
* [man ngettext](https://www.kernel.org/doc/man-pages/online/pages/man3/ngettext.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function does not treat a `LANGUAGE` environment variable
  with an empty value like an unset `LANGUAGE` environment variable
  on some platforms:
  NetBSD 10.0.

---

Next: [`textdomain`](#textdomain), Previous: [`ngettext`](#ngettext), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.14 `ngettext_l` [¶](#ngettext_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ngettext_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

---

Previous: [`ngettext_l`](#ngettext_005fl), Up: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.22.15 `textdomain` [¶](#textdomain-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/textdomain.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Locating-gettext-catalog.html>,
* [man textdomain](https://www.kernel.org/doc/man-pages/online/pages/man3/textdomain.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e), Previous: [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.23 Functions in `<locale.h>` [¶](#Functions-in-_003clocale_002eh_003e-1)

* [`duplocale`](#duplocale)
* [`freelocale`](#freelocale)
* [`getlocalename_l`](#getlocalename_005fl)
* [`localeconv`](#localeconv)
* [`newlocale`](#newlocale)
* [`setlocale`](#setlocale)
* [`uselocale`](#uselocale)

---

Next: [`freelocale`](#freelocale), Up: [Functions in `<locale.h>`](#Functions-in-_003clocale_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.23.1 `duplocale` [¶](#duplocale-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/duplocale.html>

Gnulib module: duplocale

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.
* This function is useless because the `locale_t` type is not defined
  on some platforms:
  z/OS.
* The argument `LC_GLOBAL_LOCALE` is not supported on some platforms:
  glibc 2.11, AIX 7.1.
* With the argument `LC_GLOBAL_LOCALE`, this function returns a wrong result
  on some platforms:
  NetBSD 7.1.

Portability problems not fixed by Gnulib:

* With the argument `LC_GLOBAL_LOCALE`, this function returns a wrong result
  on some platforms:
  Haiku.

---

Next: [`getlocalename_l`](#getlocalename_005fl), Previous: [`duplocale`](#duplocale), Up: [Functions in `<locale.h>`](#Functions-in-_003clocale_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.23.2 `freelocale` [¶](#freelocale-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/freelocale.html>

Gnulib module: freelocale

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.
* This function is useless because the `locale_t` type is not defined
  on some platforms:
  z/OS.

Portability problems not fixed by Gnulib:

* This function’s return type is `int` instead of `void` on macOS.
* This function may cause crashes in subsequent `newlocale` invocations
  on some platforms:
  Haiku.

---

Next: [`localeconv`](#localeconv), Previous: [`freelocale`](#freelocale), Up: [Functions in `<locale.h>`](#Functions-in-_003clocale_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.23.3 `getlocalename_l` [¶](#getlocalename_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getlocalename_l.html>

Gnulib module: getlocalename\_l-simple

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.x, mingw, MSVC 14, Android 9.0.
  But the gnulib replacement supports only a single locale category:
  the argument `LC_ALL` is unsupported.
* This function returns an invalid pointer
  when the second argument is `LC_GLOBAL_LOCALE`,
  on some platforms:
  Cygwin 3.6.0.
* This function returns `"POSIX"` instead of `"C"` on some platforms:
  Haiku.

Portability problems not fixed by Gnulib:

* This function supports only a single locale category,
  i.e. the argument `LC_ALL` is unsupported,
  on some platforms:
  Cygwin 3.6.0.
* This function always returns `"C"`
  when the first argument is `LC_COLLATE`
  on some platforms:
  NetBSD 10.0.

---

Next: [`newlocale`](#newlocale), Previous: [`getlocalename_l`](#getlocalename_005fl), Up: [Functions in `<locale.h>`](#Functions-in-_003clocale_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.23.4 `localeconv` [¶](#localeconv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/localeconv.html>

Gnulib module: localeconv

Portability problems fixed by Gnulib:

* The `struct lconv` type does not contain any members on some platforms:
  Android up to 2014.
* The `struct lconv` type does not contain the members
  `int_p_cs_precedes`, `int_p_sign_posn`, `int_p_sep_by_space`,
  `int_n_cs_precedes`, `int_n_sign_posn`, `int_n_sep_by_space`
  on some platforms:
  glibc, OpenBSD 4.9, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14.
* The values of fields of `struct lconv` of type `char` are -1 instead
  of CHAR\_MAX on some platforms:
  mingw.

Portability problems not fixed by Gnulib:

Note: The return type of the `localeconv` function
is `struct lconv *` in POSIX, with the constraint that
“The application shall not modify the structure to which the return value
points.”
The Gnulib-overridden `localeconv` function
has a return type of `const struct lconv *`.
This helps detecting code that attempts to write into that structure.

---

Next: [`setlocale`](#setlocale), Previous: [`localeconv`](#localeconv), Up: [Functions in `<locale.h>`](#Functions-in-_003clocale_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.23.5 `newlocale` [¶](#newlocale-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/newlocale.html>

Gnulib module: newlocale

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 4.4.
* This function is useless because the `locale_t` type is not defined
  on some platforms:
  z/OS.
* When the third argument is NULL, this function uses locale category data
  from the current locale instead of from the "C" locale on some platforms:
  macOS, NetBSD 10.0, Solaris 11 OpenIndiana.
* When the second argument is an invalid or unsupported locale name,
  this function uses the "C" locale instead of failing on some platforms:
  NetBSD 10.0.

Portability problems not fixed by Gnulib:

* This function is useless because the `locale_t` type contains basically
  no information on some platforms:
  OpenBSD 6.3.

---

Next: [`uselocale`](#uselocale), Previous: [`newlocale`](#newlocale), Up: [Functions in `<locale.h>`](#Functions-in-_003clocale_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.23.6 `setlocale` [¶](#setlocale-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlocale.html>

Gnulib module: setlocale, setlocale-null

Portability problems fixed by Gnulib module `setlocale`:

* On Windows platforms (excluding Cygwin), `setlocale(category,"")`
  and `setlocale(category,NULL)` both ignore the environment variables
  `LC_ALL`, `category`, and `LANG`.
* On Windows platforms (excluding Cygwin) and Cygwin 1.5.x,
  `setlocale(LC_ALL,name)` succeeds and sets the LC\_CTYPE category to
  ‘`C`’ when it does not support the encoding, instead of failing.
* On Windows platforms (excluding Cygwin), `setlocale` understands different
  locale names, that are not based on ISO 639 language names and ISO 3166 country
  names.
* On Android 5.0 and newer, setting any category other than `LC_CTYPE` of
  the global locale affects the `LC_CTYPE` category as well.
* On Android < 5.0, which doesn’t have locales, the `setlocale` function
  always fails. The replacement, however, supports only the locale names
  `"C"` and `"POSIX"`.

Portability problems fixed by Gnulib module `setlocale` or `setlocale-null`:

* Invocations of `setlocale (..., NULL)` are not thread-safe on some
  platforms:
  musl libc, macOS, FreeBSD, NetBSD, OpenBSD, AIX, Haiku, Cygwin.
  To make these invocations thread-safe, you need the Gnulib module
  `setlocale`, or you need to change the code to invoke `setlocale_null`
  or `setlocale_null_r` instead.

Portability problems not fixed by Gnulib:

* On Cygwin 1.5.x, which doesn’t have locales,
  `setlocale(LC_ALL,NULL)` always returns `"C"`.
* On Cygwin 1.7.0, only the charset portion of a locale designation is honored.
* On OpenBSD, `setlocale(LC_ALL,"")` will only update categories that
  are deemed appropriate for the `LC_ALL` environment value, even if
  there are other categories set to different values in the environment.
  In addition any value is accepted for `LC_CTYPE`, and so NULL
  is never returned to indicate a failure to set locale.
  To verify category values, each category must be set individually
  with `setlocale(LC_COLLATE,"")` etc.
* On Android 5.0 and newer, the default locale (i.e. the locale in use when
  `setlocale` was not called) is the `"C.UTF-8"` locale, not the
  `"C"` locale. Additionally, a `setlocale` call that is meant to set
  the `"C"` or `"POSIX"` locale actually sets an equivalent of the
  `"C.UTF-8"` locale.

Note: The return type of the `setlocale` function
is `char *` in POSIX, with the constraint that
“The application shall not modify the string returned.”
The Gnulib-overridden `setlocale` function
has a return type of `const char *`.
This helps detecting code that attempts to write into that string.

Note: The names of locales with UTF-8 encoding are platform dependent:

* On most platforms, they follow the naming scheme ‘`ll_CC.UTF-8`’.
  Here ‘`ll`’ is an ISO 639 two-letter language code,
  and ‘`CC`’ is an ISO 3166 two-letter country code.
  For example, ‘`en_US.UTF-8`’ (this locale exists on nearly all systems),
  ‘`fr_FR.UTF-8`’, and similar.
* On native Windows with mingw, UTF-8 locales are not supported.
* On native Windows with MSVC (version 14 or newer,
  and with the `-MD` option in `CFLAGS`),
  they follow the naming scheme ‘`language_country.65001`’
  or, equivalently, ‘`language_country.utf8`’,
  where ‘`language`’ is a language name in English
  and ‘`country`’ is a country name in English.
  For example ‘`English_United States.65001`’ (yes, with a space in it!)
  or ‘`French_France.65001`’.

---

Previous: [`setlocale`](#setlocale), Up: [Functions in `<locale.h>`](#Functions-in-_003clocale_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.23.7 `uselocale` [¶](#uselocale-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/uselocale.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 10.0, OpenBSD 6.1, Minix 3.1.8, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android API level 20.
* This function is not documented and leads to crashes in subsequent
  `setlocale` invocations on some platforms:
  AIX 7.2.
* This function is useless because the `locale_t` type is not defined
  on some platforms:
  z/OS.
* This function is useless because the `locale_t` type contains basically
  no information on some platforms:
  OpenBSD 7.4.

---

Next: [Functions in `<monetary.h>`](#Functions-in-_003cmonetary_002eh_003e), Previous: [Functions in `<locale.h>`](#Functions-in-_003clocale_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
