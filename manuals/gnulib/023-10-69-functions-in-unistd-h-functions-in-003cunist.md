### 10.69 Functions in `<unistd.h>` [¶](#Functions-in-_003cunistd_002eh_003e-1)

* [`_exit`](#g_t_005fexit)
* [`_Fork`](#g_t_005fFork)
* [`access`](#access)
* [`alarm`](#alarm)
* [`chdir`](#chdir)
* [`chown`](#chown)
* [`close`](#close)
* [`confstr`](#confstr)
* [`crypt`](#crypt)
* [`dup`](#dup)
* [`dup2`](#dup2)
* [`dup3`](#dup3)
* [`encrypt`](#encrypt)
* [`environ`](#environ)
* [`execl`](#execl)
* [`execle`](#execle)
* [`execlp`](#execlp)
* [`execv`](#execv)
* [`execve`](#execve)
* [`execvp`](#execvp)
* [`faccessat`](#faccessat)
* [`fchdir`](#fchdir)
* [`fchown`](#fchown)
* [`fchownat`](#fchownat)
* [`fdatasync`](#fdatasync)
* [`fexecve`](#fexecve)
* [`fork`](#fork)
* [`fpathconf`](#fpathconf)
* [`fsync`](#fsync)
* [`ftruncate`](#ftruncate)
* [`getcwd`](#getcwd)
* [`getegid`](#getegid)
* [`getentropy`](#getentropy)
* [`geteuid`](#geteuid)
* [`getgid`](#getgid)
* [`getgroups`](#getgroups)
* [`gethostid`](#gethostid)
* [`gethostname`](#gethostname)
* [`getlogin`](#getlogin)
* [`getlogin_r`](#getlogin_005fr)
* [`getopt`](#getopt)
* [`getpgid`](#getpgid)
* [`getpgrp`](#getpgrp)
* [`getpid`](#getpid)
* [`getppid`](#getppid)
* [`getresgid`](#getresgid)
* [`getresuid`](#getresuid)
* [`getsid`](#getsid)
* [`getuid`](#getuid)
* [`isatty`](#isatty)
* [`lchown`](#lchown)
* [`link`](#link)
* [`linkat`](#linkat)
* [`lockf`](#lockf)
* [`lseek`](#lseek)
* [`nice`](#nice)
* [`optarg`](#optarg)
* [`opterr`](#opterr)
* [`optind`](#optind)
* [`optopt`](#optopt)
* [`pathconf`](#pathconf)
* [`pause`](#pause)
* [`pipe`](#pipe)
* [`pipe2`](#pipe2)
* [`posix_close`](#posix_005fclose)
* [`pread`](#pread)
* [`pwrite`](#pwrite)
* [`read`](#read)
* [`readlink`](#readlink)
* [`readlinkat`](#readlinkat)
* [`rmdir`](#rmdir)
* [`setegid`](#setegid)
* [`seteuid`](#seteuid)
* [`setgid`](#setgid)
* [`setpgid`](#setpgid)
* [`setregid`](#setregid)
* [`setresgid`](#setresgid)
* [`setresuid`](#setresuid)
* [`setreuid`](#setreuid)
* [`setsid`](#setsid)
* [`setuid`](#setuid)
* [`sleep`](#sleep)
* [`swab`](#swab)
* [`symlink`](#symlink)
* [`symlinkat`](#symlinkat)
* [`sync`](#sync)
* [`sysconf`](#sysconf)
* [`tcgetpgrp`](#tcgetpgrp)
* [`tcsetpgrp`](#tcsetpgrp)
* [`truncate`](#truncate)
* [`ttyname`](#ttyname)
* [`ttyname_r`](#ttyname_005fr)
* [`unlink`](#unlink)
* [`unlinkat`](#unlinkat)
* [`write`](#write)

---

Next: [`_Fork`](#g_t_005fFork), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.1 `_exit` [¶](#g_t_005fexit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/_exit.html>

Gnulib module: unistd-h

Portability problems fixed by Gnulib:

* This function is declared in a different header file (namely, `<stdlib.h>`)
  on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`access`](#access), Previous: [`_exit`](#g_t_005fexit), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.2 `_Fork` [¶](#g_t_005fFork-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/_Fork.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Creating-a-Process.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.33, macOS 14, FreeBSD 13.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android API level 34.

---

Next: [`alarm`](#alarm), Previous: [`_Fork`](#g_t_005fFork), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.3 `access` [¶](#access-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/access.html>

Gnulib module: access

Portability problems fixed by Gnulib:

* This function does not support the `X_OK` mode on some platforms:
  MSVC 14.
* This function does not reject trailing slashes on symlinks to non-directories
  on some platforms:
  Mac OS X 10.5.

Portability problems not fixed by Gnulib:

* This function uses the effective id instead of the real id on some
  platforms:
  Cygwin 1.5.x.

Other problems of this function:

* There is an inherent race between calling this function and performing
  some action based on the results; you should think twice before trusting
  this function, especially in a set-uid or set-gid program.
* This function does not have an option for not following symbolic links
  (like `stat` versus `lstat`). If you need this option, use
  the Gnulib module `faccessat` with the `AT_EACCESS` flag.
* On Solaris, for the root user, any file is `X_OK` even if the file
  does not have the `x` permission bit set.
* On native Windows, files whose basename does not contain a ‘`.`’
  cannot be executed through `execlp` or `execvp`. Nevertheless,
  this function may return true for such files.
* On Windows, different facilities for executing a program have different
  ways of finding an executable file, by trying various suffixes. For
  example, `execlp` and `execvp` search for files with the
  suffixes `.com`, `.exe`, `.bat`, `.cmd`, when the
  file with the given file name does not exist. Whereas `cmd.exe`
  searches according to the `PATHEXT` environment variable. This
  function does not perform any search; it merely looks at the file with
  the given file name.

---

Next: [`chdir`](#chdir), Previous: [`access`](#access), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.4 `alarm` [¶](#alarm-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/alarm.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function has no impact if `SIGALRM` is inherited as
  ignored; programs should use `signal (SIGALRM, SIG_DFL)` if
  it is important to ensure the alarm will fire.
* Use of this function in multi-threaded programs is not advised.
* This function is missing on some platforms:
  mingw (2011), MSVC 14.
* This function is conditionally declared in the non-standard
  `<io.h>` header on some platforms:
  mingw (2012 or newer).

---

Next: [`chown`](#chown), Previous: [`alarm`](#alarm), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.5 `chdir` [¶](#chdir-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/chdir.html>

Gnulib module: chdir

Portability problems fixed by Gnulib:

* This function is declared in different header files (namely, `<io.h>` or
  `<direct.h>`) on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`close`](#close), Previous: [`chdir`](#chdir), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.6 `chown` [¶](#chown-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/chown.html>

Gnulib module: chown

Portability problems fixed by Gnulib:

* Some platforms fail to detect trailing slash on non-directories, as in
  `chown("link-to-file/",uid,gid)`:
  macOS 14, FreeBSD 7.2, AIX 7.3.1, Solaris 9.
* Some platforms fail to update the change time when at least one
  argument was not −1, but no ownership changes resulted:
  OpenBSD 7.7, Cygwin 2.9.0.
* When passed an argument of −1, some implementations really set the owner
  user/group id of the file to this value, rather than leaving that id of the
  file alone:
  some very old platforms.
* When applied to a symbolic link, some implementations don’t dereference
  the symlink, i.e. they behave like `lchown`:
  some very old platforms.
* This function is missing on some platforms; however, the replacement
  always fails with `ENOSYS`:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* This function sometimes fails with `EACCES` when the failure is
  due to lack of appropriate privileges (`EPERM`), not to
  search permission denied on the file name prefix (`EACCES`):
  Linux kernel 5.15 with glibc 2.35 and a CIFS v1 file system
  (see <https://bugs.gnu.org/65599>).

---

Next: [`confstr`](#confstr), Previous: [`chown`](#chown), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.7 `close` [¶](#close-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/close.html>

Gnulib module: close

Portability problems fixed by Gnulib:

* This function is declared in a different header file (namely, `<io.h>`)
  on some platforms:
  MSVC 14.
* This function crashes when invoked with invalid arguments on some platforms:
  MSVC 14.
* On Windows platforms (excluding Cygwin), `socket` and `accept`
  do not return file descriptors that can be closed by `close`.
  Instead, `closesocket` must be used.

Portability problems not fixed by Gnulib:

---

Next: [`crypt`](#crypt), Previous: [`close`](#close), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.8 `confstr` [¶](#confstr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/confstr.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`dup`](#dup), Previous: [`confstr`](#confstr), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.9 `crypt` [¶](#crypt-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/crypt.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.34, FreeBSD 6.0, NetBSD 5.0, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function is not declared in `<unistd.h>`
  (without `-D_GNU_SOURCE`) on some platforms:
  glibc (at least 2.11–2.13).

---

Next: [`dup2`](#dup2), Previous: [`crypt`](#crypt), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.10 `dup` [¶](#dup-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dup.html>

Gnulib module: dup

Portability problems fixed by Gnulib:

* This function is declared in a different header file (namely, `<io.h>`)
  on some platforms:
  MSVC 14.
* This function crashes when invoked with invalid arguments on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`dup3`](#dup3), Previous: [`dup`](#dup), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.11 `dup2` [¶](#dup2-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dup2.html>

Gnulib module: dup2

Portability problems fixed by Gnulib:

* This function is declared in a different header file (namely, `<io.h>`)
  on some platforms:
  MSVC 14.
* This function always returns 0 for success on some platforms:
  mingw, MSVC 14.
* This function can hang when duplicating an fd to itself on some platforms:
  mingw, MSVC 14.
* This function crashes when invoked with invalid arguments on some platforms:
  Cygwin 1.7.17, MSVC 14.
* This function crashes when invoked with valid arguments on some platforms:
  Cygwin 1.7.25.
* This function fails with `EINVAL` when duplicating an fd to itself:
  Android.
* This function resets the `FD_CLOEXEC` flag when duplicating an fd
  to itself on some platforms:
  Haiku.
* This function returns 0 for `dup2 (1, 1)` on some platforms:
  Cygwin 1.5.x.
* This function may return `-EBADF` instead of `-1` on some platforms:
  Linux releases between July 2008 and May 2009 (versions 2.6.27 to 2.6.29).
* This function returns `EMFILE` instead of `EBADF` for
  large targets, which interferes with using
  `dup2(fd,fd)==fd)` as the minimal `EBADF` filter:
  AIX 7.1, FreeBSD 6.1, Cygwin 1.5.

Portability problems not fixed by Gnulib:

---

Next: [`encrypt`](#encrypt), Previous: [`dup2`](#dup2), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.12 `dup3` [¶](#dup3-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dup3.html>

Documentation:  
 [man dup3](https://www.kernel.org/doc/man-pages/online/pages/man2/dup3.2.html)

Gnulib module: dup3

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.8, macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.1, mingw, MSVC 14, Android 4.4.
* This function can crash on some platforms:
  Cygwin 1.7.25.
* This function mistakenly succeeds when given two equal file descriptors on some platforms:
  NetBSD 10.0, Haiku.

Portability problems not fixed by Gnulib:

---

Next: [`environ`](#environ), Previous: [`dup3`](#dup3), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.13 `encrypt` [¶](#encrypt-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/encrypt.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function is not declared in `<unistd.h>`
  (without `-D_GNU_SOURCE`) on some platforms:
  glibc (at least 2.11–2.13).
* POSIX.1-2024 says this function is obsolescent and it is planned to be
  removed in a future version.

---

Next: [`execl`](#execl), Previous: [`encrypt`](#encrypt), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.14 `environ` [¶](#environ-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/environ.html>

Gnulib module: environ

Portability problems fixed by Gnulib:

* POSIX does not require this variable to be declared, and it is indeed not
  declared on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 5.0, OpenBSD 3.8, Solaris 11.4.
* On macOS, this variable is not declared. Up to Mac OS X 10.4, one can use

  ```
  extern char **environ;
  ```

  to get the variable declared. This does not work any more, however, in
  shared libraries on macOS 10.5. Here is a workaround: Instead, one can use

  ```
  #include <crt_externs.h>
  #define environ (*_NSGetEnviron())
  ```

  This works at all versions of macOS.
* On Cygwin in 64-bit mode, references to this variable cause a link error when
  the option `-Wl,--disable-auto-import` is in use.

Portability problems not fixed by Gnulib:

* The address of this variable is not a compile-time constant on some platforms:
  mingw.
* Assigning NULL to `environ` to clear all variables is not
  portable; better is to assign `environ` to one-element array
  containing a NULL pointer. That said, an empty environment is not
  portable either, as some systems may require particular environment
  variables (such as `PATH`) to be present in order to operate
  consistently.

---

Next: [`execle`](#execle), Previous: [`environ`](#environ), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.15 `execl` [¶](#execl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/execl.html>

Gnulib module: execl

Portability problems fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not pass
  command-line arguments correctly if they contain space, tab, backslash,
  or double-quote characters.
* On Windows platforms (excluding Cygwin), this function spawns an asynchronous
  child process and then exits the current process immediately. As a
  consequence, the parent of the current process 1. may incorrectly proceed
  as if its child had exited, and 2. will never see the child’s exit status.
* On Windows platforms (excluding Cygwin), the return type of this function is
  `intptr_t`, not `int`.

Note: The Gnulib replacement for this function is not async-safe, that is,
it must not be invoked from a signal handler.

Portability problems not fixed by Gnulib:

* On some platforms, a script without executable permission is still run:
  Cygwin 1.5.x.

---

Next: [`execlp`](#execlp), Previous: [`execl`](#execl), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.16 `execle` [¶](#execle-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/execle.html>

Gnulib module: execle

Portability problems fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not pass
  command-line arguments correctly if they contain space, tab, backslash,
  or double-quote characters.
* On Windows platforms (excluding Cygwin), this function spawns an asynchronous
  child process and then exits the current process immediately. As a
  consequence, the parent of the current process 1. may incorrectly proceed
  as if its child had exited, and 2. will never see the child’s exit status.
* On Windows platforms (excluding Cygwin), the return type of this function is
  `intptr_t`, not `int`.

Note: The Gnulib replacement for this function is not async-safe, that is,
it must not be invoked from a signal handler.

Portability problems not fixed by Gnulib:

* On some platforms, a script without executable permission is still run:
  Cygwin 1.5.x.

---

Next: [`execv`](#execv), Previous: [`execle`](#execle), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.17 `execlp` [¶](#execlp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/execlp.html>

Gnulib module: execlp

Portability problems fixed by Gnulib:

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

* On some platforms, a script without executable permission is still run:
  Cygwin 1.5.x.

---

Next: [`execve`](#execve), Previous: [`execlp`](#execlp), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.18 `execv` [¶](#execv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/execv.html>

Gnulib module: execv

Portability problems fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not pass
  command-line arguments correctly if they contain space, tab, backslash,
  or double-quote characters.
* On Windows platforms (excluding Cygwin), this function spawns an asynchronous
  child process and then exits the current process immediately. As a
  consequence, the parent of the current process 1. may incorrectly proceed
  as if its child had exited, and 2. will never see the child’s exit status.
* On Windows platforms (excluding Cygwin), the return type of this function is
  `intptr_t`, not `int`.

Note: The Gnulib replacement for this function is not async-safe, that is,
it must not be invoked from a signal handler.

Portability problems not fixed by Gnulib:

* On some platforms, a script without executable permission is still run:
  Cygwin 1.5.x.

---

Next: [`execvp`](#execvp), Previous: [`execv`](#execv), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.19 `execve` [¶](#execve-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/execve.html>

Gnulib module: execve

Portability problems fixed by Gnulib:

* On Windows platforms (excluding Cygwin), this function does not pass
  command-line arguments correctly if they contain space, tab, backslash,
  or double-quote characters.
* On Windows platforms (excluding Cygwin), this function spawns an asynchronous
  child process and then exits the current process immediately. As a
  consequence, the parent of the current process 1. may incorrectly proceed
  as if its child had exited, and 2. will never see the child’s exit status.
* On Windows platforms (excluding Cygwin), the return type of this function is
  `intptr_t`, not `int`.

Note: The Gnulib replacement for this function is not async-safe, that is,
it must not be invoked from a signal handler.

Portability problems not fixed by Gnulib:

* On some platforms, a script without executable permission is still run:
  Cygwin 1.5.x.

---

Next: [`faccessat`](#faccessat), Previous: [`execve`](#execve), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.20 `execvp` [¶](#execvp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/execvp.html>

Gnulib module: execvp

Portability problems fixed by Gnulib:

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

* On some platforms, a script without executable permission is still run:
  Cygwin 1.5.x.

---

Next: [`fchdir`](#fchdir), Previous: [`execvp`](#execvp), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.21 `faccessat` [¶](#faccessat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/faccessat.html>

Gnulib module: faccessat

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 10.9, FreeBSD 7.4, NetBSD 6.1.5, OpenBSD 4.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 4.0.4.
* On some platforms, `faccessat (dfd, "file/", amode, flag)`
  succeeds instead of failing when `file` is not a directory.
  macOS 14.

Portability problems not fixed by Gnulib:

* The replacement does not always take ACLs into account.
* The replacement is not safe to be used in libraries.
* The replacement is not thread-safe.
* The replacement does not support the `AT_SYMLINK_NOFOLLOW` flag,
  which is supported by GNU `faccessat`.
* On some platforms, `faccessat` can mishandle `AT_EACCESS`
  after a process starts as root and then becomes non-root:
  GNU/Linux with glibc 2.32.
* This function does not support `AT_EMPTY_PATH`:
  Linux kernel 2.6.38.
* When `AT_EMPTY_PATH` is used,
  this function does not allow the file name to be a null pointer:
  glibc 2.41, Linux kernel 6.10, Cygwin 3.6, FreeBSD 15.1.

Other problems of this function:

* There is an inherent race between calling this function and performing
  some action based on the results; you should think twice before trusting
  this function, especially in a set-uid or set-gid program.

---

Next: [`fchown`](#fchown), Previous: [`faccessat`](#faccessat), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.22 `fchdir` [¶](#fchdir-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fchdir.html>

Gnulib module: fchdir

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
  But the replacement function is not safe to be used in libraries and is not
  thread-safe.

Portability problems not fixed by Gnulib:

* This function fails if the file descriptor was opened with `O_PATH`:
  Linux kernel 3.4.

---

Next: [`fchownat`](#fchownat), Previous: [`fchdir`](#fchdir), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.23 `fchown` [¶](#fchown-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fchown.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function sometimes fails with `EACCES` when the failure is
  due to lack of appropriate privileges (`EPERM`), not to
  search permission denied on the file name prefix (`EACCES`):
  Linux kernel 5.15 with glibc 2.35 and a CIFS v1 file system
  (see <https://bugs.gnu.org/65599>).

---

Next: [`fdatasync`](#fdatasync), Previous: [`fchown`](#fchown), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.24 `fchownat` [¶](#fchownat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fchownat.html>

Gnulib module: fchownat

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8,
  AIX 5.1, HP-UX 11, Cygwin 1.5.x, mingw, MSVC 14.
  But the replacement function is not safe to be used in libraries and
  is not thread-safe. Also, the replacement may fail to change
  symlinks if `lchown` is unsupported, or fail altogether if
  `chown` is unsupported.
* This function is declared in `<sys/stat.h>`, not in `<unistd.h>`,
  on some platforms:
  Android 4.3.
* Some platforms fail to detect trailing slash on non-directories, as in
  `fchown(dir,"link-to-file/",uid,gid,flag)`:
  Solaris 9.
* Some platforms fail to update the change time when at least one
  argument was not −1, but no ownership changes resulted:
  OpenBSD 7.7.
* Some platforms mistakenly dereference symlinks when using
  `AT_SYMLINK_NOFOLLOW`:
  Linux kernel 2.6.17.
* This function does not fail for an empty filename on some platforms:
  Linux with glibc < 2.11.

Portability problems not fixed by Gnulib:

* When `AT_EMPTY_PATH` is used,
  this function does not allow the file name to be a null pointer:
  Linux kernel 6.10, Cygwin 3.6, FreeBSD 15.1.
* This function sometimes fails with `EACCES` when the failure is
  due to lack of appropriate privileges (`EPERM`), not to
  search permission denied on the file name prefix (`EACCES`):
  Linux kernel 5.15 with glibc 2.35 and a CIFS v1 file system
  (see <https://bugs.gnu.org/65599>).

---

Next: [`fexecve`](#fexecve), Previous: [`fchownat`](#fchownat), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.25 `fdatasync` [¶](#fdatasync-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fdatasync.html>

Gnulib module: fdatasync

Portability problems fixed by Gnulib:

* This function is present but not declared on some platforms:
  Mac OS X 10.7.
* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 11.0, OpenBSD 3.8, Minix 3.1.8, mingw, MSVC 14, Android 2.2.

Portability problems not fixed by Gnulib:

* If the argument is a file descriptor that lacks write access,
  such as a directory file descriptor, this function fails
  with `EBADF` on some platforms:
  AIX 7.3, Cygwin 3.6.x.

---

Next: [`fork`](#fork), Previous: [`fdatasync`](#fdatasync), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.26 `fexecve` [¶](#fexecve-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fexecve.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11 2010-11, Cygwin 1.5.x, mingw, MSVC 14, Android 8.1.

---

Next: [`fpathconf`](#fpathconf), Previous: [`fexecve`](#fexecve), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.27 `fork` [¶](#fork-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fork.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* On some platforms, `fork` followed by a call of the `exec` family
  (`execl`, `execlp`, `execle`, `execv`, `execvp`,
  or `execve`) is less efficient than `vfork` followed by the same
  call. `vfork` is a variant of `fork` that has been introduced to
  optimize the `fork`/`exec` pattern.
* On Windows platforms (excluding Cygwin), this function is not implemented; use
  `_spawnvp` instead.

---

Next: [`fsync`](#fsync), Previous: [`fork`](#fork), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.28 `fpathconf` [¶](#fpathconf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fpathconf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`ftruncate`](#ftruncate), Previous: [`fpathconf`](#fpathconf), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.29 `fsync` [¶](#fsync-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fsync.html>

Gnulib module: fsync

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* If the argument is a file descriptor that lacks write access,
  such as a directory file descriptor, this function fails
  with `EBADF` on some platforms:
  AIX 7.3, Cygwin 3.6.x.

---

Next: [`getcwd`](#getcwd), Previous: [`fsync`](#fsync), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.30 `ftruncate` [¶](#ftruncate-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ftruncate.html>

Gnulib module: ftruncate

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 14.
* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

---

Next: [`getegid`](#getegid), Previous: [`ftruncate`](#ftruncate), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.31 `getcwd` [¶](#getcwd-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getcwd.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getcwd.html>

Gnulib module: getcwd or getcwd-lgpl

Portability problems fixed by either Gnulib module `getcwd` or
`getcwd-lgpl`:

* This function is declared in different header files (namely, `<io.h>` or
  `<direct.h>`) on some platforms:
  mingw, MSVC 14.
* On glibc and Solaris 11.4 platforms,
  `getcwd (NULL, n)` allocates memory for the result.
  On some other platforms, this call is not allowed.
* On some platforms, the prototype for `getcwd` uses `int`
  instead of `size_t` for the size argument when using non-standard
  headers, and the declaration is missing from `<unistd.h>`:
  mingw, MSVC 14.
* On some platforms, `getcwd (buf, 0)` crashes:
  MSVC 14.
* On some platforms, `getcwd (buf, 0)` fails with `ERANGE`
  instead of the required `EINVAL`:
  mingw.

Portability problems fixed by Gnulib module `getcwd`:

* This function does not handle long file names (greater than `PATH_MAX`)
  correctly on some platforms:
  glibc on Linux 2.4.20, musl libc 1.2.2/powerpc64le, macOS 13, FreeBSD 6.4, NetBSD 10.0, OpenBSD 7.9, AIX 7.1.

Portability problems not fixed by Gnulib:

* When using `getcwd(NULL, nonzero)`, some platforms, such as glibc
  or cygwin, allocate exactly `nonzero` bytes and fail with
  `ERANGE` if it was not big enough, while other platforms, such as
  FreeBSD, mingw, or MSVC 14, ignore the size argument and allocate whatever size
  is necessary. If this call succeeds, an application cannot portably
  access beyond the string length of the result.

---

Next: [`getentropy`](#getentropy), Previous: [`getcwd`](#getcwd), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.32 `getegid` [¶](#getegid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getegid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`geteuid`](#geteuid), Previous: [`getegid`](#getegid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.33 `getentropy` [¶](#getentropy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getentropy.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Unpredictable-Bytes.html>,
* [man getentropy](https://www.kernel.org/doc/man-pages/online/pages/man3/getentropy.3.html).

Gnulib module: getentropy

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.24, Mac OS X 10.11, FreeBSD 11.0, NetBSD 9.3, OpenBSD 5.5, Minix 3.3, AIX 7.1, HP-UX 11.31, Solaris 11.0, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.
* This function is declared in `<sys/random.h>`, not in `<unistd.h>`,
  on some platforms:
  macOS 14, Solaris 11.4, Android 9.0.

Portability problems not fixed by Gnulib:

Although this function is intended to produce random data, the data’s
security properties may not be appropriate for your application.
For example, identical “random” data streams might be produced by
rebooted virtual machines. If this is of concern you may need to use
additional techniques such as hedging.[1](#FOOT1)

Related modules include `getrandom`, which has a more-flexible
but more-complex API, and `crypto/gc-random`, which is likely a
better match for code already using the other `crypto` APIs.

---

Next: [`getgid`](#getgid), Previous: [`getentropy`](#getentropy), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.34 `geteuid` [¶](#geteuid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/geteuid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`getgroups`](#getgroups), Previous: [`geteuid`](#geteuid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.35 `getgid` [¶](#getgid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getgid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`gethostid`](#gethostid), Previous: [`getgid`](#getgid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.36 `getgroups` [¶](#getgroups-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getgroups.html>

Gnulib module: getgroups

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* On some platforms, this function fails to reject a negative count,
  even though that is less than the size that would be returned:
  macOS 14, FreeBSD 13.0.
* On NeXTstep 3.2, `getgroups (0, NULL)` always fails. See macro
  ‘`AC_FUNC_GETGROUPS`’.
* On very old systems, this function operated on an array of ‘`int`’,
  even though that was a different size than an array of ‘`gid_t`’.

Portability problems not fixed by Gnulib:

* This function is unsafe to call between `fork` and `exec` if
  the parent process is multi-threaded.
* It is unspecified whether the effective group id will be included in
  the returned list, nor whether the list will be sorted in any
  particular order. For that matter, some platforms include the
  effective group id twice, if it is also a member of the current
  supplemental group ids.

The Gnulib module `mgetgroups` provides a similar API.

---

Next: [`gethostname`](#gethostname), Previous: [`getgroups`](#getgroups), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.37 `gethostid` [¶](#gethostid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/gethostid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`getlogin`](#getlogin), Previous: [`gethostid`](#gethostid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.38 `gethostname` [¶](#gethostname-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/gethostname.html>

Gnulib module: gethostname

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>` instead of `<unistd.h>`
  on some platforms:
  MSVC 14.
* On mingw and MSVC 14, this function has a prototype that differs from that
  specified by POSIX, and it is defined only in the ws2\_32 library.

Portability problems not fixed by Gnulib:

* This function’s second argument type is `int` instead of `size_t`
  on some platforms:
  Solaris 10, Solaris 11 OmniOS.
* If the given buffer is too small for the host name, some implementations
  fail with `EINVAL`, instead of returning a truncated host name.

---

Next: [`getlogin_r`](#getlogin_005fr), Previous: [`gethostname`](#gethostname), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.39 `getlogin` [¶](#getlogin-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getlogin.html>

Gnulib module: getlogin

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  older mingw, MSVC 14.
* This function is not declared unless `_POSIX` is defined on some platforms:
  mingw.
* This function returns the value of the `LOGNAME` environment variable
  and is therefore arbitrarily fakeable:
  musl libc 1.2.5.

Portability problems not fixed by Gnulib:

* This function returns an empty string even when standard input is a tty
  on some platforms:
  HP-UX 11.11.
* When there are several user names with the same user ID,
  this function may return, instead of the user name that logged in,
  another user name that has the same user ID, on some platforms:
  Linux and others.

---

Next: [`getopt`](#getopt), Previous: [`getlogin`](#getlogin), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.40 `getlogin_r` [¶](#getlogin_005fr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getlogin_r.html>

Gnulib module: getlogin\_r

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, Minix 3.1.8, mingw, MSVC 14, Android 8.1.
* This function is not declared unless `_REENTRANT` is defined,
  on some platforms:
  HP-UX 11.
* This function returns a truncated result, instead of failing with error code
  `ERANGE`, when the buffer is not large enough, on some platforms:
  macOS 14.
* This function returns the value of the `LOGNAME` environment variable
  and is therefore arbitrarily fakeable:
  musl libc 1.2.5.

Portability problems not fixed by Gnulib:

* This function has an incompatible declaration on some platforms:
  FreeBSD 12.0, MidnightBSD 2.0, Solaris 11.4 (when `_POSIX_PTHREAD_SEMANTICS` is not defined).
* This function fails even when standard input is a tty on some platforms:
  HP-UX 11.11.
* This function fails with error code `EINVAL` instead of `ERANGE` when
  the second argument is zero on some platforms:
  HP-UX 11.31.
* This function fails with error code `ENOMEM` instead of `ERANGE` on
  some platforms:
  Haiku.
* When there are several user names with the same user ID,
  this function may return, instead of the user name that logged in,
  another user name that has the same user ID, on some platforms:
  Linux and others.

---

Next: [`getpgid`](#getpgid), Previous: [`getlogin_r`](#getlogin_005fr), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.41 `getopt` [¶](#getopt-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getopt.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/libutil-getopt-3.html>

Gnulib module: getopt-posix or getopt-gnu

The module `getopt-gnu` has support for “long options” and for
“options that take optional arguments”. Compared to the API defined by POSIX,
it adds a header file `<getopt.h>` and a function `getopt_long`.

Portability problems fixed by either Gnulib module `getopt-posix` or `getopt-gnu`:

* This function is missing on some platforms:
  MSVC 14.
* The value of `optind` after a missing required argument is wrong
  on some platforms:
  macOS 14, AIX 7.1, mingw.

Portability problems fixed by Gnulib module `getopt-gnu`:

* The function `getopt` does not support the ‘`+`’ flag in the options
  string on some platforms:
  macOS 14, AIX 5.2, HP-UX 11, Solaris 11 2010-11.
* The function `getopt` does not obey the combination of ‘`+`’
  and ‘`:`’ flags in the options string on some platforms:
  glibc 2.11.
* The function `getopt` does not obey the ‘`-`’ flag in the options
  string when `POSIXLY_CORRECT` is set on some platforms:
  Cygwin 1.7.0.
* The function `getopt` does not support options with optional arguments
  on some platforms:
  macOS 14, OpenBSD 4.0, AIX 5.2, HP-UX 11, Solaris 11 2010-11, Cygwin 1.5.x.
* The function `getopt_long` is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 9, MSVC 14.
* The function `getopt_long` does not support abbreviated long options
  where all disambiguations are equivalent on some platforms:
  OpenBSD 5.0.
* The function `getopt_long_only` is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 10.0, AIX 5.1, HP-UX 11, Solaris 9, mingw, MSVC 14.
* This function crashes if the option string includes `W;` on some
  platforms:
  glibc 2.14.

Portability problems not fixed by Gnulib:

* The default behavior of the glibc implementation of `getopt` allows
  mixing option and non-option arguments on the command line in any order.
  Other implementations, such as the one in Cygwin, enforce strict POSIX
  compliance: they require that the option arguments precede the non-option
  arguments. This is something to watch out in your program’s
  testsuite.
* The glibc implementation allows a complete reset of the environment,
  including re-checking for `POSIXLY_CORRECT`, by setting
  `optind` to 0. Several BSD implementations provide `optreset`,
  causing a reset by setting it non-zero, although it does not
  necessarily re-read `POSIXLY_CORRECT`. Solaris `getopt` does
  not support either reset method, but does not maintain state that
  needs the extra level of reset.
* On some platforms, this function does not set the stream error
  indicator on attempts to write to a read-only stream:
  glibc 2.13, Cygwin 1.7.9.

---

Next: [`getpgrp`](#getpgrp), Previous: [`getopt`](#getopt), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.42 `getpgid` [¶](#getpgid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getpgid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`getpid`](#getpid), Previous: [`getpgid`](#getpgid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.43 `getpgrp` [¶](#getpgrp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getpgrp.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`getppid`](#getppid), Previous: [`getpgrp`](#getpgrp), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.44 `getpid` [¶](#getpid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getpid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`getresgid`](#getresgid), Previous: [`getpid`](#getpid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.45 `getppid` [¶](#getppid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getppid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`getresuid`](#getresuid), Previous: [`getppid`](#getppid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.46 `getresgid` [¶](#getresgid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getresgid.html>

Documentation:  
 [man getresgid](https://www.kernel.org/doc/man-pages/online/pages/man2/getresgid.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, NetBSD 10.0, Minix 3.1.8, AIX 5.1, HP-UX 11.00, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`getsid`](#getsid), Previous: [`getresgid`](#getresgid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.47 `getresuid` [¶](#getresuid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getresuid.html>

Documentation:  
 [man getresuid](https://www.kernel.org/doc/man-pages/online/pages/man2/getresuid.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, NetBSD 10.0, Minix 3.1.8, AIX 5.1, HP-UX 11.00, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`getuid`](#getuid), Previous: [`getresuid`](#getresuid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.48 `getsid` [¶](#getsid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getsid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.1.

---

Next: [`isatty`](#isatty), Previous: [`getsid`](#getsid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.49 `getuid` [¶](#getuid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getuid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`lchown`](#lchown), Previous: [`getuid`](#getuid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.50 `isatty` [¶](#isatty-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/isatty.html>

Gnulib module: isatty

Portability problems fixed by Gnulib:

* This function is declared in a different header file (namely, `<io.h>`)
  on some platforms:
  MSVC 14.
* On native Windows, this function also returns true for character devices such
  as `NUL`.
* On native Windows, this function returns false for Cygwin consoles.
* This function crashes when invoked with invalid arguments on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`link`](#link), Previous: [`isatty`](#isatty), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.51 `lchown` [¶](#lchown-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lchown.html>

Gnulib module: lchown

Portability problems fixed by Gnulib:

* This function is missing on some platforms; however, the replacement
  fails with `ENOSYS`:
  mingw, MSVC 14.
* This function is missing on some platforms; however, the replacement
  fails on symlinks:
  Minix 3.2.1.
* Some platforms fail to detect trailing slash on non-directories, as in
  `lchown("link-to-file/",uid,gid)`:
  FreeBSD 7.2, Solaris 9.
* Some platforms fail to update the change time when at least one
  argument was not −1, but no ownership changes resulted:
  OpenBSD 7.7.

Portability problems not fixed by Gnulib:

* This function sometimes fails with `EACCES` when the failure is
  due to lack of appropriate privileges (`EPERM`), not to
  search permission denied on the file name prefix (`EACCES`):
  Linux kernel 5.15 with glibc 2.35 and a CIFS v1 file system
  (see <https://bugs.gnu.org/65599>).

---

Next: [`linkat`](#linkat), Previous: [`lchown`](#lchown), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.52 `link` [¶](#link-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/link.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-link-1.html>

Gnulib module: link

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function fails to reject trailing slashes on non-directories on
  some platforms:
  FreeBSD 7.2, Solaris 11.3, Cygwin 1.5.x.

Portability problems not fixed by Gnulib:

* When the first argument is a symlink, some platforms create a
  hard-link to what the symlink referenced, rather than to the symlink
  itself. Use ‘`linkat`’ to force a particular behavior.

---

Next: [`lockf`](#lockf), Previous: [`link`](#link), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.53 `linkat` [¶](#linkat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/linkat.html>

Gnulib module: linkat

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Mac OS X 10.9, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 4.4.
  But the replacement function is not safe to be used in libraries and
  is not thread-safe.
* This function fails to directly hardlink symlinks on some platforms:
  Mac OS X 10.10.
* This function fails to reject trailing slashes on non-directories on
  some platforms:
  macOS 14, AIX 7.1, Solaris 11.3.
* This functions does not support `AT_SYMLINK_FOLLOW` on some platforms:
  Linux kernel 2.6.17.

Portability problems not fixed by Gnulib:

* `linkat (fd, "directory", fd, "symlink/" ...)` sets `errno`
  to `EINVAL` instead of `EEXIST` or `ENOTDIR` on some
  platforms:
  GNU/Hurd with glibc 2.41.

---

Next: [`lseek`](#lseek), Previous: [`linkat`](#linkat), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.54 `lockf` [¶](#lockf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lockf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly across the entire data range of files 2 GiB and larger.
  See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 6.0.

---

Next: [`nice`](#nice), Previous: [`lockf`](#lockf), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.55 `lseek` [¶](#lseek-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lseek.html>

Gnulib module: lseek

Portability problems fixed by Gnulib:

* On some platforms, `lseek (fd, offset, SEEK_DATA)` returns a value
  greater than `offset` even when `offset` addresses data:
  macOS 14.
* This function is declared in a different header file (namely, `<io.h>`)
  on some platforms:
  MSVC 14.
* On platforms where `off_t` is a 32-bit type, `lseek` does not work
  correctly with files 2 GiB and larger. See [Large File Support](#Large-File-Support).
* This function mistakenly succeeds on pipes on some platforms: mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* POSIX does not specify which file descriptors support seeking and which don’t.
  In practice, regular files and block devices support seeking, and ttys, pipes,
  and most character devices don’t support it.
* Some systems do not support `SEEK_DATA` and `SEEK_HOLE`:
  AIX, HP-UX, Microsoft Windows, NetBSD, OpenBSD, Cygwin 3.4.6.
* Some systems have a buggy `SEEK_DATA` and `SEEK_HOLE`,
  and Gnulib works around the problem via `#undef SEEK_DATA`
  and `#undef SEEK_HOLE`:
  FreeBSD 13, macOS 12.

---

Next: [`optarg`](#optarg), Previous: [`lseek`](#lseek), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.56 `nice` [¶](#nice-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nice.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* In glibc before glibc 2.2.4, `nice` returned 0 upon success.

---

Next: [`opterr`](#opterr), Previous: [`nice`](#nice), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.57 `optarg` [¶](#optarg-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/optarg.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on some platforms:
  Mac OS X 10.5, MSVC 14.

---

Next: [`optind`](#optind), Previous: [`optarg`](#optarg), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.58 `opterr` [¶](#opterr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/opterr.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on some platforms:
  MSVC 14.

---

Next: [`optopt`](#optopt), Previous: [`opterr`](#opterr), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.59 `optind` [¶](#optind-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/optind.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on some platforms:
  MSVC 14.

---

Next: [`pathconf`](#pathconf), Previous: [`optind`](#optind), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.60 `optopt` [¶](#optopt-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/optopt.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on some platforms:
  Mac OS X 10.5, MSVC 14.

---

Next: [`pause`](#pause), Previous: [`optopt`](#optopt), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.61 `pathconf` [¶](#pathconf-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pathconf.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`pipe`](#pipe), Previous: [`pathconf`](#pathconf), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.62 `pause` [¶](#pause-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pause.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`pipe2`](#pipe2), Previous: [`pause`](#pause), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.63 `pipe` [¶](#pipe-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pipe.html>

Gnulib module: pipe-posix

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* This function crashes rather than failing with `EMFILE` if no
  resources are left on some platforms:
  Cygwin 1.7.9.

---

Next: [`posix_close`](#posix_005fclose), Previous: [`pipe`](#pipe), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.64 `pipe2` [¶](#pipe2-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pipe2.html>

Documentation:  
 [man pipe2](https://www.kernel.org/doc/man-pages/online/pages/man2/pipe2.2.html)

Gnulib module: pipe2

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.8, macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.1, mingw, MSVC 14, Android 2.2.
  But the replacement function is not atomic; this matters in multi-threaded
  processes that spawn child processes.

Portability problems not fixed by Gnulib:

* This function crashes rather than failing with `EMFILE` if no
  resources are left on some platforms:
  Cygwin 1.7.9.

Note: This function portably supports the `O_NONBLOCK` flag only if the
gnulib module `nonblocking` is also used.

---

Next: [`pread`](#pread), Previous: [`pipe2`](#pipe2), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.65 `posix_close` [¶](#posix_005fclose-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_close.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

---

Next: [`pwrite`](#pwrite), Previous: [`posix_close`](#posix_005fclose), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.66 `pread` [¶](#pread-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pread.html>

Gnulib module: pread

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files 2 GiB and larger. See [Large File Support](#Large-File-Support).
* This function returns zero instead of positive values when large file support
  is enabled on some platforms:
  HP-UX 11.11.
* This function does not fail on pipes on some platforms:
  HP-UX 11.31.

Portability problems not fixed by Gnulib:

---

Next: [`read`](#read), Previous: [`pread`](#pread), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.67 `pwrite` [¶](#pwrite-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pwrite.html>

Gnulib module: pwrite

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files 2 GiB and larger. See [Large File Support](#Large-File-Support).
* This function does not fail when an invalid (negative) offset is passed when
  large file support is enabled on some platforms:
  HP-UX 11.11.
* This function uses an arbitrary offset instead of the `off_t` argument
  when large file support is enabled on some platforms:
  HP-UX 11.11.

Portability problems not fixed by Gnulib:

---

Next: [`readlink`](#readlink), Previous: [`pwrite`](#pwrite), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.68 `read` [¶](#read-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/read.html>

Gnulib module: read, stdio-h, nonblocking

Portability problems fixed by Gnulib module `read`:

* This function is declared in a different header file (namely, `<io.h>`)
  on some platforms:
  mingw, MSVC 14.
* This function crashes when invoked with invalid arguments on some platforms:
  MSVC 14.

Portability problems fixed by Gnulib module `stdio-h`, together with module `nonblocking`:

* When reading from a non-blocking pipe whose buffer is empty, this function
  fails with `errno` being set to `EINVAL` instead of `EAGAIN` on
  some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* This function may fail with error `EINTR`, even in programs that don’t
  install any signal handlers, on some platforms:
  macOS 14.

For handling `EINTR`, Gnulib provides a module ‘`safe-read`’ with a
function `safe_read`.

---

Next: [`readlinkat`](#readlinkat), Previous: [`read`](#read), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.69 `readlink` [¶](#readlink-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/readlink.html>

Gnulib module: readlink

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* Some platforms mistakenly succeed on file names ending in `/`:
  FreeBSD 7.2, Solaris 9, macOS 14.
* On some platforms, this function returns `int` instead of
  `ssize_t`:
  glibc 2.4, FreeBSD 6.0, OpenBSD 7.9, Cygwin 1.5.x, AIX 7.1.
* For the file name `/dev/null`, this function returns `\Device\Null`,
  which is unusable, on some platforms:
  Cygwin 3.3.6.

Portability problems mostly fixed by Gnulib:

* On some platforms, this function fails and sets `errno` to
  `ERANGE` rather than returning truncated contents:
  AIX 7.2, HP-UX 11.
  The Gnulib replacement normally works as POSIX requires by returning
  the truncated contents. However, if the full link contents are
  unreasonably large (more than 4000 bytes) the replacement clears the
  entire buffer and returns the buffer size; although this is not a
  complete fix, it suffices for typical callers, which ignore the buffer
  contents anyway.

Portability problems not fixed by Gnulib:

* This function always fails on platforms that don’t support symlinks:
  mingw, MSVC 14.
* When this function is called on a directory: In the case of NFS mounted
  directories, Cygwin sets `errno` to `ENOENT` or `EIO` instead of
  `EINVAL`. To avoid this problem, check for a directory before calling
  this function.
* When this function is called on a file that is not a symbolic link:
  Cygwin may set errno to `EACCES` instead of `EINVAL`.
* When this function fails because it is called on an existing
  non-directory’s name concatenated to `/`,
  it sets `errno` to `EINVAL`:
  AIX 7.2.
* Symlink contents do not always have a trailing null byte, and there is
  no indication if symlink contents were truncated if the return value
  matches the length. Furthermore,
  Linux sets `errno` to `EINVAL` if the
  requested length is zero.
  Use the gnulib module `areadlink` for
  improved ability to read symlink contents.

---

Next: [`rmdir`](#rmdir), Previous: [`readlink`](#readlink), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.70 `readlinkat` [¶](#readlinkat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/readlinkat.html>

Gnulib module: readlinkat

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Mac OS X 10.9, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 4.4.
  But the replacement function is not safe to be used in libraries and
  is not thread-safe.
* Some platforms mistakenly succeed on file names ending in `/`:
  macOS 14.
* On some platforms, this function returns `int` instead of
  `ssize_t`:
  AIX 7.1.
* For the file name `/dev/null`, this function returns `\Device\Null`,
  which is unusable, on some platforms:
  Cygwin 3.3.6.

Portability problems mostly fixed by Gnulib:

* On some platforms, this function fails and sets `errno` to
  `ERANGE` rather than returning truncated contents:
  AIX 7.2.
  The Gnulib replacement normally works as POSIX requires by returning
  the truncated contents. However, if the full link contents are
  unreasonably large (more than 4000 bytes) the replacement clears the
  entire buffer and returns the buffer size; although this is not a
  complete fix, it suffices for typical callers, which ignore the buffer
  contents anyway.

Portability problems not fixed by Gnulib:

* Behaviors differ when this function is called on an empty file name.
  On most platforms, the function fails with error `ENOENT`.
  On Cygnus 3.4.6, the function fails with error `EBADF` instead.
  On GNU/Linux (kernel version 2.6.39 or later), when dirfd is
  nonnegative the function instead reads directly from dirfd,
  and this succeeds if dirfd corresponds to a symbolic link that
  was opened with `O_PATH | O_NOFOLLOW`.
* This function always fails on platforms that don’t support symlinks:
  mingw, MSVC 14.
* When this function is called on a directory: In the case of NFS mounted
  directories, Cygwin sets `errno` to `ENOENT` or `EIO` instead of
  `EINVAL`. To avoid this problem, check for a directory before calling
  this function.
* When this function is called on a file that is not a symbolic link:
  Cygwin may set errno to `EACCES` instead of `EINVAL`.
* When this function fails because it is called on an existing
  non-directory’s name concatenated to `/`,
  it sets `errno` to `EINVAL`:
  AIX 7.2.
* Symlink contents do not always have a trailing null byte, and there is
  no indication if symlink contents were truncated if the return value
  matches the length. Furthermore,
  Linux sets `errno` to `EINVAL` if the
  requested length is zero.
  Use the gnulib module `areadlink` for
  improved ability to read symlink contents.

---

Next: [`setegid`](#setegid), Previous: [`readlinkat`](#readlinkat), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.71 `rmdir` [¶](#rmdir-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/rmdir.html>

Gnulib module: rmdir

Portability problems fixed by Gnulib:

* This function is declared in different header files (namely, `<io.h>` or
  `<direct.h>`) on some platforms:
  mingw, MSVC 14.
* This function mistakenly removes a directory with
  `rmdir("dir/./")` on some platforms:
  Cygwin 1.5.x.
* This function fails with `EINVAL` instead of the expected
  `ENOTDIR` for `rmdir("file/")` on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* When `rmdir` fails because the specified directory is not empty, the
  `errno` value is system dependent.
* POSIX requires that `rmdir("link-to-empty/")` remove `empty`
  and leave `link-to-empty` as a dangling symlink. This is
  counter-intuitive, so some systems fail with `ENOTDIR` instead:
  glibc

---

Next: [`seteuid`](#seteuid), Previous: [`rmdir`](#rmdir), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.72 `setegid` [¶](#setegid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setegid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.23, mingw, MSVC 14.

---

Next: [`setgid`](#setgid), Previous: [`setegid`](#setegid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.73 `seteuid` [¶](#seteuid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/seteuid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.23, mingw, MSVC 14.

---

Next: [`setpgid`](#setpgid), Previous: [`seteuid`](#seteuid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.74 `setgid` [¶](#setgid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setgid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`setregid`](#setregid), Previous: [`setgid`](#setgid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.75 `setpgid` [¶](#setpgid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setpgid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`setresgid`](#setresgid), Previous: [`setpgid`](#setpgid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.76 `setregid` [¶](#setregid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setregid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`setresuid`](#setresuid), Previous: [`setregid`](#setregid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.77 `setresgid` [¶](#setresgid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setresgid.html>

Documentation:  
 [man setresgid](https://www.kernel.org/doc/man-pages/online/pages/man2/setresgid.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, NetBSD 10.0, Minix 3.1.8, AIX 5.1, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`setreuid`](#setreuid), Previous: [`setresgid`](#setresgid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.78 `setresuid` [¶](#setresuid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setresuid.html>

Documentation:  
 [man setresuid](https://www.kernel.org/doc/man-pages/online/pages/man2/setresuid.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, NetBSD 10.0, Minix 3.1.8, AIX 5.1, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`setsid`](#setsid), Previous: [`setresuid`](#setresuid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.79 `setreuid` [¶](#setreuid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setreuid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`setuid`](#setuid), Previous: [`setreuid`](#setreuid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.80 `setsid` [¶](#setsid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setsid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`sleep`](#sleep), Previous: [`setsid`](#setsid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.81 `setuid` [¶](#setuid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setuid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`swab`](#swab), Previous: [`setuid`](#setuid), Up: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.69.82 `sleep` [¶](#sleep-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sleep.html>

Gnulib module: sleep

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw (2005 or newer), MSVC 14.
* This function takes milliseconds as argument and returns `void` on some
  platforms:
  mingw (2005 and older).
* This function cannot sleep longer than 49.7 days on some platforms:
  Cygwin 1.5.x.

Portability problems not fixed by Gnulib:

* According to POSIX, the `sleep` function may interfere with the program’s
  use of the `SIGALRM` signal. On Linux, it doesn’t; on other platforms,
  it may.

---
