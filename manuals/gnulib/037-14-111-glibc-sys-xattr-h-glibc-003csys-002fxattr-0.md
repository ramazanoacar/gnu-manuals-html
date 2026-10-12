### 14.111 Glibc `<sys/xattr.h>` [¶](#Glibc-_003csys_002fxattr_002eh_003e)

* [`fgetxattr`](#fgetxattr)
* [`flistxattr`](#flistxattr)
* [`fremovexattr`](#fremovexattr)
* [`fsetxattr`](#fsetxattr)
* [`getxattr`](#getxattr)
* [`lgetxattr`](#lgetxattr)
* [`listxattr`](#listxattr)
* [`llistxattr`](#llistxattr)
* [`lremovexattr`](#lremovexattr)
* [`lsetxattr`](#lsetxattr)
* [`removexattr`](#removexattr)
* [`setxattr`](#setxattr)

---

Next: [`flistxattr`](#flistxattr), Up: [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.111.1 `fgetxattr` [¶](#fgetxattr-1)

Documentation:  
 [man fgetxattr](https://www.kernel.org/doc/man-pages/online/pages/man2/fgetxattr.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 4.0.4.
* This function has extra `offset` and `options` parameters
  on some platforms:
  macOS 14.

---

Next: [`fremovexattr`](#fremovexattr), Previous: [`fgetxattr`](#fgetxattr), Up: [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.111.2 `flistxattr` [¶](#flistxattr-1)

Documentation:  
 [man flistxattr](https://www.kernel.org/doc/man-pages/online/pages/man2/flistxattr.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 4.0.4.
* This function has an extra `options` parameter on some platforms:
  macOS 14.
* This function fails with `EACCES` on unreadable files accessed via NFS:
  Linux kernel 6.12.
* This function does not report synthetic `security.*` extended attributes
  on `tmpfs` and `kernfs`-based file systems:
  Linux kernel 6.14.
* This function can fail with `ERANGE` even if the buffer is large enough:
  Linux kernel 6.15.

---

Next: [`fsetxattr`](#fsetxattr), Previous: [`flistxattr`](#flistxattr), Up: [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.111.3 `fremovexattr` [¶](#fremovexattr-1)

Documentation:  
 [man fremovexattr](https://www.kernel.org/doc/man-pages/online/pages/man2/fremovexattr.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 4.0.4.
* This function has an extra `options` parameter on some platforms:
  macOS 14.

---

Next: [`getxattr`](#getxattr), Previous: [`fremovexattr`](#fremovexattr), Up: [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.111.4 `fsetxattr` [¶](#fsetxattr-1)

Documentation:  
 [man fsetxattr](https://www.kernel.org/doc/man-pages/online/pages/man2/fsetxattr.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 4.0.4.
* This function has extra `offset` and `options` parameters
  and no `flags` parameter on some platforms:
  macOS 14.

---

Next: [`lgetxattr`](#lgetxattr), Previous: [`fsetxattr`](#fsetxattr), Up: [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.111.5 `getxattr` [¶](#getxattr-1)

Documentation:  
 [man getxattr](https://www.kernel.org/doc/man-pages/online/pages/man2/getxattr.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 4.0.4.
* This function has extra `offset` and `options` parameters
  on some platforms:
  macOS 14.

---

Next: [`listxattr`](#listxattr), Previous: [`getxattr`](#getxattr), Up: [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.111.6 `lgetxattr` [¶](#lgetxattr-1)

Documentation:  
 [man lgetxattr](https://www.kernel.org/doc/man-pages/online/pages/man2/lgetxattr.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 4.0.4.

---

Next: [`llistxattr`](#llistxattr), Previous: [`lgetxattr`](#lgetxattr), Up: [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.111.7 `listxattr` [¶](#listxattr-1)

Documentation:  
 [man listxattr](https://www.kernel.org/doc/man-pages/online/pages/man2/listxattr.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 4.0.4.
* This function has an extra `options` parameter on some platforms:
  macOS 14.
* This function fails with `EACCES` on unreadable files accessed via NFS:
  Linux kernel 6.12.
* This function does not report synthetic `security.*` extended attributes
  on `tmpfs` and `kernfs`-based file systems:
  Linux kernel 6.14.
* This function can fail with `ERANGE` even if the buffer is large enough:
  Linux kernel 6.15.

---

Next: [`lremovexattr`](#lremovexattr), Previous: [`listxattr`](#listxattr), Up: [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.111.8 `llistxattr` [¶](#llistxattr-1)

Documentation:  
 [man llistxattr](https://www.kernel.org/doc/man-pages/online/pages/man2/llistxattr.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 4.0.4.
* This function fails with `EACCES` on unreadable files accessed via NFS:
  Linux kernel 6.12.
* This function does not report synthetic `security.*` extended attributes
  on `tmpfs` and `kernfs`-based file systems:
  Linux kernel 6.14.
* This function can fail with `ERANGE` even if the buffer is large enough:
  Linux kernel 6.15.

---

Next: [`lsetxattr`](#lsetxattr), Previous: [`llistxattr`](#llistxattr), Up: [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.111.9 `lremovexattr` [¶](#lremovexattr-1)

Documentation:  
 [man lremovexattr](https://www.kernel.org/doc/man-pages/online/pages/man2/lremovexattr.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 4.0.4.

---

Next: [`removexattr`](#removexattr), Previous: [`lremovexattr`](#lremovexattr), Up: [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.111.10 `lsetxattr` [¶](#lsetxattr-1)

Documentation:  
 [man lsetxattr](https://www.kernel.org/doc/man-pages/online/pages/man2/lsetxattr.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 4.0.4.

---

Next: [`setxattr`](#setxattr), Previous: [`lsetxattr`](#lsetxattr), Up: [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.111.11 `removexattr` [¶](#removexattr-1)

Documentation:  
 [man removexattr](https://www.kernel.org/doc/man-pages/online/pages/man2/removexattr.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 4.0.4.
* This function has an extra `options` parameter on some platforms:
  macOS 14.

---

Previous: [`removexattr`](#removexattr), Up: [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.111.12 `setxattr` [¶](#setxattr-1)

Documentation:  
 [man setxattr](https://www.kernel.org/doc/man-pages/online/pages/man2/setxattr.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 4.0.4.
* This function has extra `offset` and `options` parameters
  and no `flags` parameter on some platforms:
  macOS 14.

---

Next: [Glibc Extensions to `<time.h>`](#Glibc-time_002eh), Previous: [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.112 Glibc Extensions to `<termios.h>` [¶](#Glibc-Extensions-to-_003ctermios_002eh_003e)

* [`cfmakeraw`](#cfmakeraw)
* [`cfsetspeed`](#cfsetspeed)

---

Next: [`cfsetspeed`](#cfsetspeed), Up: [Glibc Extensions to `<termios.h>`](#Glibc-termios_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.112.1 `cfmakeraw` [¶](#cfmakeraw-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-cfmakeraw-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Noncanonical-Input.html>,
* [man cfmakeraw](https://www.kernel.org/doc/man-pages/online/pages/man3/cfmakeraw.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 4.4.

---

Previous: [`cfmakeraw`](#cfmakeraw), Up: [Glibc Extensions to `<termios.h>`](#Glibc-termios_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.112.2 `cfsetspeed` [¶](#cfsetspeed-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-cfsetspeed-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Line-Speed.html>,
* [man cfsetspeed](https://www.kernel.org/doc/man-pages/online/pages/man3/cfsetspeed.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.

---

Next: [Glibc `<ttyent.h>`](#Glibc-ttyent_002eh), Previous: [Glibc Extensions to `<termios.h>`](#Glibc-termios_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.113 Glibc Extensions to `<time.h>` [¶](#Glibc-Extensions-to-_003ctime_002eh_003e)

* [`clock_adjtime`](#clock_005fadjtime)
* [`dysize`](#dysize)
* [`getdate_r`](#getdate_005fr)
* [`stime`](#stime)
* [`strptime_l`](#strptime_005fl)
* [`timelocal`](#timelocal)

---

Next: [`dysize`](#dysize), Up: [Glibc Extensions to `<time.h>`](#Glibc-time_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.113.1 `clock_adjtime` [¶](#clock_005fadjtime-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.13, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 6.0.

---

Next: [`getdate_r`](#getdate_005fr), Previous: [`clock_adjtime`](#clock_005fadjtime), Up: [Glibc Extensions to `<time.h>`](#Glibc-time_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.113.2 `dysize` [¶](#dysize-1)

Documentation:  
 [man dysize](https://www.kernel.org/doc/man-pages/online/pages/man3/dysize.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`stime`](#stime), Previous: [`dysize`](#dysize), Up: [Glibc Extensions to `<time.h>`](#Glibc-time_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.113.3 `getdate_r` [¶](#getdate_005fr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/General-Time-String-Parsing.html>,
* [man getdate\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getdate_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`strptime_l`](#strptime_005fl), Previous: [`getdate_r`](#getdate_005fr), Up: [Glibc Extensions to `<time.h>`](#Glibc-time_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.113.4 `stime` [¶](#stime-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-stime-2.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Simple-Calendar-Time.html>,
* [man stime](https://www.kernel.org/doc/man-pages/online/pages/man2/stime.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.

---

Next: [`timelocal`](#timelocal), Previous: [`stime`](#stime), Up: [Glibc Extensions to `<time.h>`](#Glibc-time_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.113.5 `strptime_l` [¶](#strptime_005fl-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

---

Previous: [`strptime_l`](#strptime_005fl), Up: [Glibc Extensions to `<time.h>`](#Glibc-time_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.113.6 `timelocal` [¶](#timelocal-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Broken_002ddown-Time.html>,
* [man timelocal](https://www.kernel.org/doc/man-pages/online/pages/man3/timelocal.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 11.3, mingw, MSVC 14, Android 3.0.

---

Next: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh), Previous: [Glibc Extensions to `<time.h>`](#Glibc-time_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.114 Glibc `<ttyent.h>` [¶](#Glibc-_003cttyent_002eh_003e)

* [`endttyent`](#endttyent)
* [`getttyent`](#getttyent)
* [`getttynam`](#getttynam)
* [`setttyent`](#setttyent)

---

Next: [`getttyent`](#getttyent), Up: [Glibc `<ttyent.h>`](#Glibc-ttyent_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.114.1 `endttyent` [¶](#endttyent-1)

Documentation:  
 [man endttyent](https://www.kernel.org/doc/man-pages/online/pages/man3/endttyent.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getttynam`](#getttynam), Previous: [`endttyent`](#endttyent), Up: [Glibc `<ttyent.h>`](#Glibc-ttyent_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.114.2 `getttyent` [¶](#getttyent-1)

Documentation:  
 [man getttyent](https://www.kernel.org/doc/man-pages/online/pages/man3/getttyent.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`setttyent`](#setttyent), Previous: [`getttyent`](#getttyent), Up: [Glibc `<ttyent.h>`](#Glibc-ttyent_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.114.3 `getttynam` [¶](#getttynam-1)

Documentation:  
 [man getttynam](https://www.kernel.org/doc/man-pages/online/pages/man3/getttynam.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`getttynam`](#getttynam), Up: [Glibc `<ttyent.h>`](#Glibc-ttyent_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.114.4 `setttyent` [¶](#setttyent-1)

Documentation:  
 [man setttyent](https://www.kernel.org/doc/man-pages/online/pages/man3/setttyent.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<utmp.h>`](#Glibc-utmp_002eh), Previous: [Glibc `<ttyent.h>`](#Glibc-ttyent_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.115 Glibc Extensions to `<unistd.h>` [¶](#Glibc-Extensions-to-_003cunistd_002eh_003e)

* [`acct`](#acct)
* [`brk`](#brk)
* [`chroot`](#chroot)
* [`closefrom`](#closefrom)
* [`close_range`](#close_005frange)
* [`copy_file_range`](#copy_005ffile_005frange)
* [`daemon`](#daemon)
* [`eaccess`](#eaccess)
* [`endusershell`](#endusershell)
* [`euidaccess`](#euidaccess)
* [`execveat`](#execveat)
* [`execvpe`](#execvpe)
* [`get_current_dir_name`](#get_005fcurrent_005fdir_005fname)
* [`getdomainname`](#getdomainname)
* [`getdtablesize`](#getdtablesize)
* [`getpagesize`](#getpagesize)
* [`getpass`](#getpass)
* [`gettid`](#gettid)
* [`getusershell`](#getusershell)
* [`group_member`](#group_005fmember)
* [`profil`](#profil)
* [`revoke`](#revoke)
* [`sbrk`](#sbrk)
* [`setlogin`](#setlogin)
* [`setdomainname`](#setdomainname)
* [`sethostid`](#sethostid)
* [`sethostname`](#sethostname)
* [`setusershell`](#setusershell)
* [`syncfs`](#syncfs)
* [`syscall`](#syscall)
* [`ttyslot`](#ttyslot)
* [`vhangup`](#vhangup)

---

Next: [`brk`](#brk), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.1 `acct` [¶](#acct-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-acct-3.html>

Documentation:  
 [man acct](https://www.kernel.org/doc/man-pages/online/pages/man2/acct.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`chroot`](#chroot), Previous: [`acct`](#acct), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.2 `brk` [¶](#brk-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Resizing-the-Data-Segment.html>,
* [man brk](https://www.kernel.org/doc/man-pages/online/pages/man2/brk.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 12.2/arm64, AIX 5.1, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`closefrom`](#closefrom), Previous: [`brk`](#brk), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.3 `chroot` [¶](#chroot-1)

Documentation:  
 [man chroot](https://www.kernel.org/doc/man-pages/online/pages/man2/chroot.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, mingw, MSVC 14.

---

Next: [`close_range`](#close_005frange), Previous: [`chroot`](#chroot), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.4 `closefrom` [¶](#closefrom-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Opening-and-Closing-Files.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.33, macOS 14, FreeBSD 7.2, Minix 3.3.0, AIX 7.1, HP-UX 11.31, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Note (quoted from
[man close](https://www.kernel.org/doc/man-pages/online/pages/man3/close.3p.html)):
“The [POSIX] standard developers rejected a proposal to add
`closefrom()` to the [POSIX] standard. Because the standard
permits implementations to use inherited file descriptors as a means of
providing a conforming environment for the child process, it is not
possible to standardize an interface that closes arbitrary file
descriptors above a certain value while still guaranteeing a conforming
environment.”

---

Next: [`copy_file_range`](#copy_005ffile_005frange), Previous: [`closefrom`](#closefrom), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.5 `close_range` [¶](#close_005frange-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Opening-and-Closing-Files.html>,
* [man close\_range](https://www.kernel.org/doc/man-pages/online/pages/man2/close_range.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.33, macOS 14, FreeBSD 12.4, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.4.x, mingw, MSVC 14, Android API level 33.

---

Next: [`daemon`](#daemon), Previous: [`close_range`](#close_005frange), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.6 `copy_file_range` [¶](#copy_005ffile_005frange-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Copying-File-Data.html>,
* [man copy\_file\_range](https://www.kernel.org/doc/man-pages/online/pages/man2/copy_file_range.2.html).

Gnulib module: copy-file-range

Portability problems fixed by Gnulib:

* This function exists only on Linux and FreeBSD and is therefore
  missing on many platforms:
  glibc 2.26, macOS 14, FreeBSD 12.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android API level 33.
  But the replacement function is only a stub: It always fails with error `ENOSYS`.
* This function has many problems on Linux kernel versions before 5.3.
  See [“Fixes for major
  copy\_file\_range() issues”](https://lwn.net/Articles/789527/).
  On these kernel versions, the replacement function always fails with
  error `ENOSYS`.
* This function returns an incorrect value when the number of bytes copied
  exceeds `INT_MAX` on systems using glibc version 2.41 or 2.42.
  See [glibc bug 33245](https://sourceware.org/PR33245).
* This function is provided on GNU/Hurd but it is only a stub: It always
  fails with error `ENOSYS`.

Portability problems not fixed by Gnulib:

* If built with include files for Linux kernel 5.3 or later,
  the Gnulib substitute may misbehave when run on Linux kernel 5.2 or earlier.
  A similar problem exists if built with include files for glibc 2.43 or later,
  and run on glibc 2.41 or 2.42.

---

Next: [`eaccess`](#eaccess), Previous: [`copy_file_range`](#copy_005ffile_005frange), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.7 `daemon` [¶](#daemon-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-daemon-3.html>

Documentation:  
 [man daemon](https://www.kernel.org/doc/man-pages/online/pages/man3/daemon.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Next: [`endusershell`](#endusershell), Previous: [`daemon`](#daemon), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.8 `eaccess` [¶](#eaccess-1)

Documentation:  
 [man eaccess](https://www.kernel.org/doc/man-pages/online/pages/man3/eaccess.3.html)

This function is an alias of `euidaccess`. See [`euidaccess`](#euidaccess).

---

Next: [`euidaccess`](#euidaccess), Previous: [`eaccess`](#eaccess), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.9 `endusershell` [¶](#endusershell-1)

Documentation:  
 [man endusershell](https://www.kernel.org/doc/man-pages/online/pages/man3/endusershell.3.html)

Gnulib module: getusershell

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 9.0.
* This function is missing a declaration on some platforms:
  Solaris 9.

Portability problems not fixed by Gnulib:

---

Next: [`execveat`](#execveat), Previous: [`endusershell`](#endusershell), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.10 `euidaccess` [¶](#euidaccess-1)

Documentation:  
 [man euidaccess](https://www.kernel.org/doc/man-pages/online/pages/man3/euidaccess.3.html)

Gnulib module: euidaccess

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

Other problems of this function:

* There is an inherent race between calling this function and performing
  some action based on the results; you should think twice before trusting
  this function, especially in a set-uid or set-gid program.
* This function does not have an option for not following symbolic links
  (like `stat` versus `lstat`). If you need this option, use
  the Gnulib module `faccessat` with the `AT_EACCESS` flag.
* On Solaris, for the root user, any file is `X_OK` even if the file
  does not have the `x` permission bit set.

---

Next: [`execvpe`](#execvpe), Previous: [`euidaccess`](#euidaccess), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.11 `execveat` [¶](#execveat-1)

Documentation:
[man execveat](https://www.kernel.org/doc/man-pages/online/pages/man2/execveat.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.33/Linux, glibc 2.34/Hurd, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`get_current_dir_name`](#get_005fcurrent_005fdir_005fname), Previous: [`execveat`](#execveat), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.12 `execvpe` [¶](#execvpe-1)

Documentation:  
 [man execvpe](https://www.kernel.org/doc/man-pages/online/pages/man3/execvpe.3.html)

Gnulib module: execvpe

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.10, macOS 14, FreeBSD 14.0, NetBSD 7.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.5.x, mingw, Android 4.4.
* This function is not declared on some platforms:
  AIX 7.1.
* On Windows platforms (excluding Cygwin), this function does not pass
  command-line arguments correctly if they contain space, tab, backslash,
  or double-quote characters.
* On Windows platforms (excluding Cygwin), this function spawns an asynchronous
  child process and then exits the current process immediately. As a
  consequence, the parent of the current process 1. may incorrectly proceed
  as if its child had exited, and 2. will never see the child’s exit status.
* On Windows platforms (excluding Cygwin), the return type of this function is
  `intptr_t`, not `int`.

Portability problems not fixed by Gnulib:

---

Next: [`getdomainname`](#getdomainname), Previous: [`execvpe`](#execvpe), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.13 `get_current_dir_name` [¶](#get_005fcurrent_005fdir_005fname-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Working-Directory.html>,
* [man get\_current\_dir\_name](https://www.kernel.org/doc/man-pages/online/pages/man3/get_current_dir_name.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 1.7.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getdtablesize`](#getdtablesize), Previous: [`get_current_dir_name`](#get_005fcurrent_005fdir_005fname), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.14 `getdomainname` [¶](#getdomainname-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getdomainname.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Host-Identification.html>,
* [man getdomainname](https://www.kernel.org/doc/man-pages/online/pages/man2/getdomainname.2.html).

Gnulib module: getdomainname

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Solaris 11.3, mingw, MSVC 14, Android 7.1.
* This function is declared in `netdb.h`, not in `unistd.h`, on
  some platforms:
  AIX 7.1.
* The second argument is of type `int`, not `size_t`, on some platforms:
  macOS 14, FreeBSD 14.0, AIX 7.1.

Portability problems not fixed by Gnulib:

---

Next: [`getpagesize`](#getpagesize), Previous: [`getdomainname`](#getdomainname), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.15 `getdtablesize` [¶](#getdtablesize-1)

SUSv2 specification: <https://pubs.opengroup.org/onlinepubs/7908799/xsh/getdtablesize.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getdtablesize.html>

Documentation:  
 [man getdtablesize](https://www.kernel.org/doc/man-pages/online/pages/man3/getdtablesize.3.html)

Gnulib module: getdtablesize

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Android LP64, mingw, MSVC 14, Android 9.0.
* This function is not declared on some platforms:
  Android 13 (LP32?).
* This function does not represent the true `RLIMIT_NOFILE` soft
  limit on some platforms:
  Android 13 (LP32?), Cygwin 1.7.25.

Portability problems not fixed by Gnulib:

* On OpenVMS, this function returns the maximum number of open file descriptors
  in a process. The possible values of file descriptors are not constrained by
  this function.

---

Next: [`getpass`](#getpass), Previous: [`getdtablesize`](#getdtablesize), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.16 `getpagesize` [¶](#getpagesize-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getpagesize.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Query-Memory-Parameters.html>,
* [man getpagesize](https://www.kernel.org/doc/man-pages/online/pages/man2/getpagesize.2.html).

Gnulib module: getpagesize

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 14, Android 4.4.
* This function is not declared on some platforms:
  HP-UX 11 with compiler option `-D_XOPEN_SOURCE=600`.
* This function is broken on some platforms:
  mingw.

Portability problems not fixed by Gnulib:

* The function has never been in POSIX and was removed from the Single
  UNIX Specification in SUSv3 (2001).
  Although portable programs are supposed to use `sysconf (_SC_PAGESIZE)`
  instead, in practice `getpagesize` can be a little faster.
* On typical platforms this function’s API limits page sizes to at most 1 GiB,
  even though `sysconf (_SC_PAGESIZE)` does not have this limitation.
  However, this has not been a practical problem yet,
  and so many programs use `getpagesize` that it’s unlikely that
  new platforms would introduce larger page sizes any time soon.

---

Next: [`gettid`](#gettid), Previous: [`getpagesize`](#getpagesize), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.17 `getpass` [¶](#getpass-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/getpass.html>,
* [man getpass](https://www.kernel.org/doc/man-pages/online/pages/man3/getpass.3.html).

Gnulib module: getpass or getpass-gnu

Portability problems fixed by either Gnulib module `getpass` or `getpass-gnu`:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 9.0.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  Android 13.

Portability problems fixed by Gnulib module `getpass-gnu`:

* The returned password is truncated to PASS\_MAX characters on some platforms:
  uClibc (256), musl (128), macOS 14 (128), FreeBSD 14.0 (128), NetBSD 3.0 (128), OpenBSD 4.0 (128), AIX 5.1 (32), HP-UX 11 (8), Solaris 11 2010-11 (8, even less than PASS\_MAX), Cygwin (128).
  The gnulib implementation returns the password untruncated.

Portability problems not fixed by Gnulib:

---

Next: [`getusershell`](#getusershell), Previous: [`getpass`](#getpass), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.18 `gettid` [¶](#gettid-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Process-Identification.html>,
* [man gettid](https://www.kernel.org/doc/man-pages/online/pages/man2/gettid.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on most non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`group_member`](#group_005fmember), Previous: [`gettid`](#gettid), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.19 `getusershell` [¶](#getusershell-1)

Documentation:  
 [man getusershell](https://www.kernel.org/doc/man-pages/online/pages/man3/getusershell.3.html)

Gnulib module: getusershell

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 9.0.
* This function is missing a declaration on some platforms:
  Solaris 9.
* This function mistakenly returns comments and empty lines on some platforms:
  musl libc 1.2.4

Portability problems not fixed by Gnulib:

---

Next: [`profil`](#profil), Previous: [`getusershell`](#getusershell), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.20 `group_member` [¶](#group_005fmember-1)

Documentation:  
 [man group\_member](https://www.kernel.org/doc/man-pages/online/pages/man3/group_member.3.html)

Gnulib module: group-member

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`revoke`](#revoke), Previous: [`group_member`](#group_005fmember), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.21 `profil` [¶](#profil-1)

Documentation:  
 [man profil](https://www.kernel.org/doc/man-pages/online/pages/man3/profil.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`sbrk`](#sbrk), Previous: [`profil`](#profil), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.22 `revoke` [¶](#revoke-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.

---

Next: [`setlogin`](#setlogin), Previous: [`revoke`](#revoke), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.23 `sbrk` [¶](#sbrk-1)

Documentation:  
 [man sbrk](https://www.kernel.org/doc/man-pages/online/pages/man2/sbrk.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 12.2/arm64, AIX 5.1, mingw, MSVC 14.

---

Next: [`setdomainname`](#setdomainname), Previous: [`sbrk`](#sbrk), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.24 `setlogin` [¶](#setlogin-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`sethostid`](#sethostid), Previous: [`setlogin`](#setlogin), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.25 `setdomainname` [¶](#setdomainname-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Host-Identification.html>,
* [man setdomainname](https://www.kernel.org/doc/man-pages/online/pages/man2/setdomainname.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Cygwin 2.9, mingw, MSVC 14, Android 7.1.

---

Next: [`sethostname`](#sethostname), Previous: [`setdomainname`](#setdomainname), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.26 `sethostid` [¶](#sethostid-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Host-Identification.html>,
* [man sethostid](https://www.kernel.org/doc/man-pages/online/pages/man3/sethostid.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`setusershell`](#setusershell), Previous: [`sethostid`](#sethostid), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.27 `sethostname` [¶](#sethostname-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-sethostname-2.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Host-Identification.html>,
* [man sethostname](https://www.kernel.org/doc/man-pages/online/pages/man2/sethostname.2.html).

Gnulib module: sethostname

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.7.x, mingw, MSVC 14, Android 5.1.
  Note that the Gnulib replacement may fail with `ENOSYS` on some platforms.
* This function is not declared on some platforms:
  AIX 7.1, Solaris 10.

Portability problems not fixed by Gnulib:

* The first parameter is `char *` instead of `const char *`
  on some platforms:
  Solaris 11 2010-11.
* The second parameter is `int` instead of `size_t`
  on some platforms:
  macOS 14, FreeBSD 14.0, MidnightBSD 3.0, Solaris 11 2010-11, Solaris 11 OpenIndiana, Solaris 11 OmniOS.
* This function does not reject a too long host name on some platforms:
  Cygwin 3.5.3, Haiku.

---

Next: [`syncfs`](#syncfs), Previous: [`sethostname`](#sethostname), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.28 `setusershell` [¶](#setusershell-1)

Documentation:  
 [man setusershell](https://www.kernel.org/doc/man-pages/online/pages/man3/setusershell.3.html)

Gnulib module: getusershell

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 9.0.
* This function is missing a declaration on some platforms:
  Solaris 9.

Portability problems not fixed by Gnulib:

---

Next: [`syscall`](#syscall), Previous: [`setusershell`](#setusershell), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.29 `syncfs` [¶](#syncfs-1)

Documentation:  
 [man syncfs](https://www.kernel.org/doc/man-pages/online/pages/man2/syncfs.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.13, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 8.1.

---

Next: [`ttyslot`](#ttyslot), Previous: [`syncfs`](#syncfs), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.30 `syscall` [¶](#syscall-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/System-Calls.html>,
* [man syscall](https://www.kernel.org/doc/man-pages/online/pages/man2/syscall.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`vhangup`](#vhangup), Previous: [`syscall`](#syscall), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.31 `ttyslot` [¶](#ttyslot-1)

Documentation:  
 [man ttyslot](https://www.kernel.org/doc/man-pages/online/pages/man3/ttyslot.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 9.0.

---

Previous: [`ttyslot`](#ttyslot), Up: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.115.32 `vhangup` [¶](#vhangup-1)

Documentation:  
 [man vhangup](https://www.kernel.org/doc/man-pages/online/pages/man2/vhangup.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc Extensions to `<utmpx.h>`](#Glibc-utmpx_002eh), Previous: [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.116 Glibc `<utmp.h>` [¶](#Glibc-_003cutmp_002eh_003e)

* [`endutent`](#endutent)
* [`getutent`](#getutent)
* [`getutent_r`](#getutent_005fr)
* [`getutid`](#getutid)
* [`getutid_r`](#getutid_005fr)
* [`getutline`](#getutline)
* [`getutline_r`](#getutline_005fr)
* [`pututline`](#pututline)
* [`setutent`](#setutent)
* [`updwtmp`](#updwtmp)
* [`utmpname`](#utmpname)
* [`login`](#login)
* [`login_tty`](#login_005ftty)

---

Next: [`getutent`](#getutent), Up: [Glibc `<utmp.h>`](#Glibc-utmp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.116.1 `endutent` [¶](#endutent-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-endutent-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Manipulating-the-Database.html>,
* [man endutent](https://www.kernel.org/doc/man-pages/online/pages/man3/endutent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.
* This function is not declared on some platforms:
  Android before ca. 2015.

---

Next: [`getutent_r`](#getutent_005fr), Previous: [`endutent`](#endutent), Up: [Glibc `<utmp.h>`](#Glibc-utmp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.116.2 `getutent` [¶](#getutent-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getutent-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Manipulating-the-Database.html>,
* [man getutent](https://www.kernel.org/doc/man-pages/online/pages/man3/getutent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.

---

Next: [`getutid`](#getutid), Previous: [`getutent`](#getutent), Up: [Glibc `<utmp.h>`](#Glibc-utmp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.116.3 `getutent_r` [¶](#getutent_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getutent-r-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Manipulating-the-Database.html>,
* [man getutent\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getutent_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getutid_r`](#getutid_005fr), Previous: [`getutent_r`](#getutent_005fr), Up: [Glibc `<utmp.h>`](#Glibc-utmp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.116.4 `getutid` [¶](#getutid-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Manipulating-the-Database.html>,
* [man getutid](https://www.kernel.org/doc/man-pages/online/pages/man3/getutid.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`getutline`](#getutline), Previous: [`getutid`](#getutid), Up: [Glibc `<utmp.h>`](#Glibc-utmp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.116.5 `getutid_r` [¶](#getutid_005fr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Manipulating-the-Database.html>,
* [man getutid\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getutid_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getutline_r`](#getutline_005fr), Previous: [`getutid_r`](#getutid_005fr), Up: [Glibc `<utmp.h>`](#Glibc-utmp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.116.6 `getutline` [¶](#getutline-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Manipulating-the-Database.html>,
* [man getutline](https://www.kernel.org/doc/man-pages/online/pages/man3/getutline.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`pututline`](#pututline), Previous: [`getutline`](#getutline), Up: [Glibc `<utmp.h>`](#Glibc-utmp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.116.7 `getutline_r` [¶](#getutline_005fr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Manipulating-the-Database.html>,
* [man getutline\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getutline_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`setutent`](#setutent), Previous: [`getutline_r`](#getutline_005fr), Up: [Glibc `<utmp.h>`](#Glibc-utmp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.116.8 `pututline` [¶](#pututline-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Manipulating-the-Database.html>,
* [man pututline](https://www.kernel.org/doc/man-pages/online/pages/man3/pututline.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.

---

Next: [`updwtmp`](#updwtmp), Previous: [`pututline`](#pututline), Up: [Glibc `<utmp.h>`](#Glibc-utmp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.116.9 `setutent` [¶](#setutent-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-setutent-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Manipulating-the-Database.html>,
* [man setutent](https://www.kernel.org/doc/man-pages/online/pages/man3/setutent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.

---

Next: [`utmpname`](#utmpname), Previous: [`setutent`](#setutent), Up: [Glibc `<utmp.h>`](#Glibc-utmp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.116.10 `updwtmp` [¶](#updwtmp-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Manipulating-the-Database.html>,
* [man updwtmp](https://www.kernel.org/doc/man-pages/online/pages/man3/updwtmp.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, mingw, MSVC 14, Android 9.0.

---

Next: [`login`](#login), Previous: [`updwtmp`](#updwtmp), Up: [Glibc `<utmp.h>`](#Glibc-utmp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.116.11 `utmpname` [¶](#utmpname-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-utmpname-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Manipulating-the-Database.html>,
* [man utmpname](https://www.kernel.org/doc/man-pages/online/pages/man3/utmpname.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.

---

Next: [`login_tty`](#login_005ftty), Previous: [`utmpname`](#utmpname), Up: [Glibc `<utmp.h>`](#Glibc-utmp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.116.12 `login` [¶](#login-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Logging-In-and-Out.html>,
* [man login](https://www.kernel.org/doc/man-pages/online/pages/man3/login.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, mingw, MSVC 14, Android 9.0.

---

Previous: [`login`](#login), Up: [Glibc `<utmp.h>`](#Glibc-utmp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.116.13 `login_tty` [¶](#login_005ftty-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Logging-In-and-Out.html>,
* [man login\_tty](https://www.kernel.org/doc/man-pages/online/pages/man3/login_tty.3.html).

Gnulib module: login\_tty

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Android 5.1.
* This function is declared in `<utmp.h>` on glibc, Cygwin, Android,
  in `<util.h>` on macOS 14, NetBSD 5.0, OpenBSD 3.8,
  in `<libutil.h>` on FreeBSD 14.0, Haiku,
  and in `<termios.h>` on Solaris 11.4.
* This function requires linking with `-lutil` on some platforms:
  glibc 2.3.6, FreeBSD 14.0, NetBSD 5.0, OpenBSD 3.8.
  It is available without link options on other platforms:
  macOS 14, Cygwin.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh), Previous: [Glibc `<utmp.h>`](#Glibc-utmp_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.117 Glibc Extensions to `<utmpx.h>` [¶](#Glibc-Extensions-to-_003cutmpx_002eh_003e)

* [`getutmp`](#getutmp)
* [`getutmpx`](#getutmpx)
* [`updwtmpx`](#updwtmpx)
* [`utmpxname`](#utmpxname)

---

Next: [`getutmpx`](#getutmpx), Up: [Glibc Extensions to `<utmpx.h>`](#Glibc-utmpx_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.117.1 `getutmp` [¶](#getutmp-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/XPG-Functions.html>,
* [man getutmp](https://www.kernel.org/doc/man-pages/online/pages/man3/getutmp.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* On some platforms, this function does not support timestamps past the
  year 2038:
  glibc 2.38 on 32-bit platforms like x86 and ARM where `time_t`
  was historically 32 bits.
* On some platforms, this function misbehaves if the `year2038` or
  `year2038-recommended` modules are used and the program is
  configured without the `--disable-year2038` option.
  The `readutmp` module works around this problem:
  glibc 2.38 on 32-bit platforms like x86 and ARM where `time_t`
  was historically 32 bits.
  See [Avoiding the year 2038 problem](#Avoiding-the-year-2038-problem).

---

Next: [`updwtmpx`](#updwtmpx), Previous: [`getutmp`](#getutmp), Up: [Glibc Extensions to `<utmpx.h>`](#Glibc-utmpx_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.117.2 `getutmpx` [¶](#getutmpx-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/XPG-Functions.html>,
* [man getutmpx](https://www.kernel.org/doc/man-pages/online/pages/man3/getutmpx.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* On some platforms, this function does not support timestamps past the
  year 2038:
  glibc 2.38 on 32-bit platforms like x86 and ARM where `time_t`
  was historically 32 bits.
* On some platforms, this function misbehaves if the `year2038` or
  `year2038-recommended` modules are used and the program is
  configured without the `--disable-year2038` option.
  The `readutmp` module works around this problem:
  glibc 2.38 on 32-bit platforms like x86 and ARM where `time_t`
  was historically 32 bits.
  See [Avoiding the year 2038 problem](#Avoiding-the-year-2038-problem).

---

Next: [`utmpxname`](#utmpxname), Previous: [`getutmpx`](#getutmpx), Up: [Glibc Extensions to `<utmpx.h>`](#Glibc-utmpx_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.117.3 `updwtmpx` [¶](#updwtmpx-1)

Documentation:  
 [man updwtmpx](https://www.kernel.org/doc/man-pages/online/pages/man3/updwtmpx.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Previous: [`updwtmpx`](#updwtmpx), Up: [Glibc Extensions to `<utmpx.h>`](#Glibc-utmpx_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.117.4 `utmpxname` [¶](#utmpxname-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/XPG-Functions.html>,
* [man utmpxname](https://www.kernel.org/doc/man-pages/online/pages/man3/utmpxname.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14, Android 9.0.

---

Previous: [Glibc Extensions to `<utmpx.h>`](#Glibc-utmpx_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
