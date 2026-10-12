Next: [`symlink`](#symlink), Previous: [`sleep`](#sleep), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.83 `swab` [¶](#swab-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/swab.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Android 8.1.

---

Next: [`symlinkat`](#symlinkat), Previous: [`swab`](#swab), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.84 `symlink` [¶](#symlink-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/symlink.html>

Gnulib module: symlink

Portability problems fixed by Gnulib:

* On some systems, `symlink(value,"name/")` mistakenly creates a
  symlink:
  macOS 14, FreeBSD 7.2, AIX 7.1, Solaris 9.
* This function is missing on some platforms; however, the replacement
  always fails with `EPERM`:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* Some file systems do not support symbolic links.
* If the target is the empty string, this function can create an empty
  symbolic link instead of failing with `ENOENT` as GNU/Linux does:
  FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.8, Solaris 11.4.
  On Solaris, the empty symbolic link `""` resolves like `"."` does;
  on the other operating systems, resolution fails with `ENOENT`.

---

Next: [`sync`](#sync), Previous: [`symlink`](#symlink), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.85 `symlinkat` [¶](#symlinkat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/symlinkat.html>

Gnulib module: symlinkat

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 4.4.
  But the replacement function is not safe to be used in libraries and
  is not thread-safe.
* Some platforms declare this function in `stdio.h` instead of
  `unistd.h`:
  Cygwin 1.7.1.
* On some systems, `symlinkat(value, fd, "name/")` mistakenly creates a
  symlink:
  macOS 14, AIX 7.1.

Portability problems not fixed by Gnulib:

* This function always fails with `ENOSYS` on platforms that don’t
  support symlinks:
  mingw, MSVC 14.
* If the target is the empty string, this function can create an empty
  symbolic link instead of failing with `ENOENT` as GNU/Linux does:
  FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.8, Solaris 11.4.
  On Solaris, the empty symbolic link `""` resolves like `"."` does;
  on the BSDs, resolution fails with `ENOENT`.

---

Next: [`sysconf`](#sysconf), Previous: [`symlinkat`](#symlinkat), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.86 `sync` [¶](#sync-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sync.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`tcgetpgrp`](#tcgetpgrp), Previous: [`sync`](#sync), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.87 `sysconf` [¶](#sysconf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sysconf.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-sysconf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`tcsetpgrp`](#tcsetpgrp), Previous: [`sysconf`](#sysconf), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.88 `tcgetpgrp` [¶](#tcgetpgrp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tcgetpgrp.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`truncate`](#truncate), Previous: [`tcgetpgrp`](#tcgetpgrp), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.89 `tcsetpgrp` [¶](#tcsetpgrp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tcsetpgrp.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`ttyname`](#ttyname), Previous: [`tcsetpgrp`](#tcsetpgrp), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.90 `truncate` [¶](#truncate-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/truncate.html>

Gnulib module: truncate

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4 with `AC_SYS_LARGEFILE` in effect.
* On platforms where `off_t` is a 32-bit type, this function is not
  applicable to arbitrary lengths for files 2 GiB and larger.
  See [Large File Support](#Large-File-Support).
* This function does not fail when the file name argument ends in a slash
  and (without the slash) names a non-directory, on some platforms:
  AIX 7.2.

Portability problems not fixed by Gnulib:

---

Next: [`ttyname_r`](#ttyname_005fr), Previous: [`truncate`](#truncate), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.91 `ttyname` [¶](#ttyname-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ttyname.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function is just a stub that produces an error message on standard error
  on some platforms:
  Android 4.3.

---

Next: [`unlink`](#unlink), Previous: [`ttyname`](#ttyname), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.92 `ttyname_r` [¶](#ttyname_005fr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ttyname_r.html>

Gnulib module: ttyname\_r

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, Minix 3.1.8, mingw, MSVC 14.
* This function is not declared unless `_REENTRANT` is defined,
  on some platforms:
  HP-UX 11.
* This function has an incompatible declaration on some platforms:
  Solaris 11.4 (when `_POSIX_PTHREAD_SEMANTICS` is not defined).
* This function refuses to do anything when the output buffer is less than 128
  bytes large, on some platforms:
  Solaris 11 2010-11.
* This function is just a stub that produces an error message on standard error
  on some platforms:
  Android 4.3.

Portability problems not fixed by Gnulib:

---

Next: [`unlinkat`](#unlinkat), Previous: [`ttyname_r`](#ttyname_005fr), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.93 `unlink` [¶](#unlink-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/unlink.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-unlink-3.html>

Gnulib module: unlink

Portability problems fixed by Gnulib:

* This function is declared in a different header file (namely, `<stdio.h>`)
  on some platforms:
  MSVC 14.
* Some systems mistakenly succeed on `unlink("link-to-file/")`:
  GNU/Hurd, FreeBSD 7.2, AIX 7.1, Solaris 9.
* On Mac OS X 10.10, in a writable HFS mount, `unlink("..")` succeeds
  without doing anything.

Portability problems not fixed by Gnulib:

* Some systems allow a superuser to unlink directories, even though this
  can cause file system corruption. The error given if a process is not
  permitted to unlink directories varies across implementations; it is
  not always the POSIX value of `EPERM`. Meanwhile, if a process
  has the ability to unlink directories, POSIX requires that
  `unlink("symlink-to-dir/")` remove `dir` and leave
  `symlink-to-dir` dangling; this behavior is counter-intuitive.
  The gnulib module `unlinkdir` can help determine whether code must be
  cautious of unlinking directories.
* Removing an open file is non-portable: On Unix this allows the programs that
  have the file already open to continue working with it; the file’s storage
  is only freed when the no process has the file open any more. On Windows,
  the attempt to remove an open file fails.

---

Next: [`write`](#write), Previous: [`unlink`](#unlink), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.94 `unlinkat` [¶](#unlinkat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/unlinkat.html>

Gnulib module: unlinkat

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8,
  AIX 5.1, HP-UX 11, Cygwin 1.5.x, mingw, MSVC 14.
  But the replacement function is not safe to be used in libraries and
  is not thread-safe.
* This function is declared in `<fcntl.h>`, not in `<unistd.h>`,
  on some platforms:
  Cygwin 1.7.1, Android 4.3.
* On Mac OS X 10.10, in a writable HFS mount, `unlinkat(fd, "..", 0)` succeeds
  without doing anything.
* Some systems mistakenly succeed on `unlinkat(fd,"file/",flag)`:
  GNU/Hurd, Solaris 9.

Portability problems not fixed by Gnulib:

* When `unlinkat(fd,name,AT_REMOVEDIR)` fails because the specified
  directory is not empty, the `errno` value is system dependent.
* POSIX requires that `unlinkdir(fd,"link-to-empty/",AT_REMOVEDIR)`
  remove `empty` and leave `link-to-empty` as a dangling
  symlink. This is counter-intuitive, so some systems fail with
  `ENOTDIR` instead:
  glibc
* Some systems allow a superuser to unlink directories, even though this
  can cause file system corruption. The error given if a process is not
  permitted to unlink directories varies across implementations; it is
  not always the POSIX value of `EPERM`. Meanwhile, if a process
  has the ability to unlink directories, POSIX requires that
  `unlinkat(fd,"symlink-to-dir/",0)` remove `dir` and leave
  `symlink-to-dir` dangling; this behavior is counter-intuitive.
  The gnulib module `unlinkdir` can help determine whether code must be
  cautious of unlinking directories.
* Removing an open file is non-portable: On Unix this allows the programs that
  have the file already open to continue working with it; the file’s storage
  is only freed when the no process has the file open any more. On Windows,
  the attempt to remove an open file fails.

---

Previous: [`unlinkat`](#unlinkat), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.95 `write` [¶](#write-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/write.html>

Gnulib module: write, nonblocking, sigpipe

Portability problems fixed by Gnulib module `write`:

* This function is declared in a different header file (namely, `<io.h>`)
  on some platforms:
  mingw, MSVC 14.
* This function crashes when invoked with invalid arguments on some platforms:
  MSVC 14.

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When writing to a non-blocking pipe whose buffer is full, this function fails
  with `errno` being set to `ENOSPC` instead of `EAGAIN` on some
  platforms:
  mingw, MSVC 14.
* When writing to a non-blocking pipe on which no reader is currently waiting
  an amount of bytes that exceeds the pipe buffer’s size, then—even if the
  pipe’s buffer is empty—this function fails, instead of performing a partial
  write into the pipe buffer, on some platforms:
  mingw, MSVC 14.

Portability problems fixed by Gnulib module `stdio-h`, together with module `sigpipe`:

* When writing to a pipe with no readers, this function fails with error
  `EINVAL`, instead of obeying the current `SIGPIPE` handler, on
  some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* This function may fail with error `EINTR`, even in programs that don’t
  install any signal handlers, on some platforms:
  macOS 14.

For handling `EINTR`, Gnulib provides a module ‘`safe-write`’ with a
function `safe_write`.

---

Next: [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e), Previous: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.70 Functions in `<utmpx.h>` [¶](#Functions-in-_003cutmpx_002eh_003e-1)

* [`endutxent`](#endutxent)
* [`getutxent`](#getutxent)
* [`getutxid`](#getutxid)
* [`getutxline`](#getutxline)
* [`pututxline`](#pututxline)
* [`setutxent`](#setutxent)

---

Next: [`getutxent`](#getutxent), Up: [Functions in `<utmpx.h>`](#Functions-in-_003cutmpx_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.70.1 `endutxent` [¶](#endutxent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/endutxent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`getutxid`](#getutxid), Previous: [`endutxent`](#endutxent), Up: [Functions in `<utmpx.h>`](#Functions-in-_003cutmpx_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.70.2 `getutxent` [¶](#getutxent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getutxent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`getutxline`](#getutxline), Previous: [`getutxent`](#getutxent), Up: [Functions in `<utmpx.h>`](#Functions-in-_003cutmpx_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.70.3 `getutxid` [¶](#getutxid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getutxid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`pututxline`](#pututxline), Previous: [`getutxid`](#getutxid), Up: [Functions in `<utmpx.h>`](#Functions-in-_003cutmpx_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.70.4 `getutxline` [¶](#getutxline-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getutxline.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`setutxent`](#setutxent), Previous: [`getutxline`](#getutxline), Up: [Functions in `<utmpx.h>`](#Functions-in-_003cutmpx_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.70.5 `pututxline` [¶](#pututxline-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pututxline.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Previous: [`pututxline`](#pututxline), Up: [Functions in `<utmpx.h>`](#Functions-in-_003cutmpx_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.70.6 `setutxent` [¶](#setutxent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setutxent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e), Previous: [Functions in `<utmpx.h>`](#Functions-in-_003cutmpx_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
