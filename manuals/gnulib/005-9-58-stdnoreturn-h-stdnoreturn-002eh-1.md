### 9.58 `stdnoreturn.h` [¶](#stdnoreturn_002eh-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.25.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdnoreturn.h.html>

Gnulib module: stdnoreturn-h

Portability problems fixed by Gnulib:

* This header file is missing on many platforms:
  FreeBSD 6.4, NetBSD 7.1, OpenBSD 7.9, Minix 3.3.0, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 2.9.0, mingw, MSVC 14, Android 9.0.
* This file conflicts with some system header files, such as `<stdlib.h>` and
  `<process.h>`, on some platforms:
  MSVC/clang.

Portability problems not fixed by Gnulib:

* `<stdnoreturn.h>` and the `noreturn` macro are obsolescent in C23.
* `<stdnoreturn.h>` cannot be #included in C++ mode on some platforms:
  FreeBSD 13.1.
* `<stdnoreturn.h>` should be #included before ‘`_Noreturn`’ is used.
* You cannot assume that `_Noreturn` is a reserved word;
  it might be a macro.
* When the macro `lint` is defined, standard headers define
  `_Noreturn` (and therefore `noreturn`) to be a macro that
  expands to the empty token sequence on some platforms:
  Cygwin 2.5.1, FreeBSD 10.3.
* On Cygwin 1.7.30 and MSVC 14, `noreturn` expands to the empty token
  sequence, to avoid problems with standard headers that use `noreturn`
  in combination with `__attribute__` or `__declspec`. Although
  the resulting code operates correctly, the compiler is not informed whether
  `noreturn` functions do not return, so it may generate incorrect
  warnings at compile-time, or code that is slightly less optimized. This
  problem does not occur with `_Noreturn`.
* Circa 2012 bleeding-edge GCC with `-Werror=old-style-declaration`
  requires `_Noreturn` or `noreturn` before the returned type
  in a declaration, and therefore rejects valid but unusually-worded
  declarations such as `void _Noreturn foo (void);`.

---

Next: [`strings.h`](#strings_002eh), Previous: [`stdnoreturn.h`](#stdnoreturn_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.59 `string.h` [¶](#string_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html>

Gnulib module: string-h

Portability problems fixed by Gnulib:

* Some platforms provide a `NULL` macro that cannot be used in arbitrary
  expressions:
  NetBSD 5.0

Portability problems not fixed by Gnulib:

* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the header `<stddef.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`sys/ipc.h`](#sys_002fipc_002eh), Previous: [`string.h`](#string_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.60 `strings.h` [¶](#strings_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/strings.h.html>

Gnulib module: strings-h

Portability problems fixed by Gnulib:

* This header file is not self-contained on some platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  MSVC 14.
* This header file defines symbols, such as ‘`index`’, often used for
  variables, making debugging harder.

---

Next: [`sys/mman.h`](#sys_002fmman_002eh), Previous: [`strings.h`](#strings_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.61 `sys/ipc.h` [¶](#sys_002fipc_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_ipc.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`sys/msg.h`](#sys_002fmsg_002eh), Previous: [`sys/ipc.h`](#sys_002fipc_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.62 `sys/mman.h` [¶](#sys_002fmman_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_mman.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.
* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the header `<fcntl.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`sys/resource.h`](#sys_002fresource_002eh), Previous: [`sys/mman.h`](#sys_002fmman_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.63 `sys/msg.h` [¶](#sys_002fmsg_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_msg.h.html>

Gnulib module: sys\_msg-h

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`sys/select.h`](#sys_002fselect_002eh), Previous: [`sys/msg.h`](#sys_002fmsg_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.64 `sys/resource.h` [¶](#sys_002fresource_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_resource.h.html>

Gnulib module: sys\_resource-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.
* On some platforms, this header file requires that <sys/types.h> and
  <sys/time.h> already be included:
  FreeBSD 5.0.
* On some platforms, this header file does not define the `RUSAGE_SELF` and
  `RUSAGE_CHILDREN` constants:
  OpenVMS.

Portability problems not fixed by Gnulib:

* On some platforms, this header does not define some or all of the
  symbolic constants required by POSIX. For example, OpenVMS and Android do
  not define `RLIM_SAVED_CUR` or `RLIM_SAVED_MAX`.
* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the header `<sys/time.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`sys/sem.h`](#sys_002fsem_002eh), Previous: [`sys/resource.h`](#sys_002fresource_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.65 `sys/select.h` [¶](#sys_002fselect_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_select.h.html>

Gnulib module: sys\_select-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  HP-UX 11.11, NonStop Kernel, mingw, MSVC 14.
* This header file is not self-contained on some platforms: it requires
  `<sys/types.h>` to be included first.
* This header file is not self-contained—it requires `<string.h>`
  before `FD_ZERO` can be used—on some platforms:
  AIX 7.1, Solaris 11.4.

Portability problems not fixed by Gnulib:

* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the headers `<signal.h>` and `<time.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`sys/shm.h`](#sys_002fshm_002eh), Previous: [`sys/select.h`](#sys_002fselect_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.66 `sys/sem.h` [¶](#sys_002fsem_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_sem.h.html>

Gnulib module: sys\_sem-h

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`sys/socket.h`](#sys_002fsocket_002eh), Previous: [`sys/sem.h`](#sys_002fsem_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.67 `sys/shm.h` [¶](#sys_002fshm_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_shm.h.html>

Gnulib module: sys\_shm-h

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`sys/stat.h`](#sys_002fstat_002eh), Previous: [`sys/shm.h`](#sys_002fshm_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.68 `sys/socket.h` [¶](#sys_002fsocket_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_socket.h.html>

Gnulib module: sys\_socket-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.
* This header file is not self-contained on some platforms: it requires
  `<sys/types.h>` to be included first.
* This header file does not define the type `struct iovec` on some platforms:
  OpenBSD 4.4.
* This header file is lacking the `SHUT_RD`, `SHUT_WR`,
  `SHUT_RDWR` macros on some platforms, despite having the `shutdown`
  functions:
  emx+gcc.
* The `struct sockaddr_storage` type does not have a member `ss_family`
  on some platforms:
  AIX 7.1.
* The `CMSG_SPACE` and `CMSG_LEN` macros are not provided on some
  platforms:
  OpenVMS.
* This header file does not define the `SO_REUSEPORT` macro on some
  platforms:
  Minix 3.1.8, Solaris 10, Cygwin, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* This header file does not declare the `msg_control` and
  `msg_controllen` members of `struct msghdr` on some
  platforms. This can be detected by the absence of the
  `CMSG_FIRSTHDR` macro:
  gnulib replacement header, old BSD
* This header file does not define the macro `SOCK_NONBLOCK`
  on some platforms:
  macOS 15, AIX 7.3, Solaris 10, mingw, MSVC 14.
* This header file does not define the macros `SOCK_CLOEXEC` and
  `MSG_CMSG_CLOEXEC` on some platforms:
  macOS 15, AIX 7.3, Solaris 10, mingw, MSVC 14.
* This header file does not define the macro `MSG_CMSG_CLOEXEC`
  on some platforms:
  Solaris 11 OmniOS.
* Although POSIX.1-2024 standardized `SOCK_CLOFORK` and
  `MSG_CMSG_CLOFORK`, no known platform supports them.
* This header file does not define the macro `SO_DOMAIN` on some platforms:
  macOS 15, NetBSD 10.0, AIX 7.3, Cygwin 3.5, mingw, MSVC 14.
* This header file does not define the macro `SO_PROTOCOL` on some platforms:
  macOS 15, NetBSD 10.0, AIX 7.3, Solaris 11.4, Cygwin 3.5, mingw, MSVC 14.
* This header file does not define the macro `MSG_NOSIGNAL`
  on some platforms:
  Solaris 10, mingw, MSVC 14.
* This header file does not define the macro `MSG_EOR` on some platforms:
  mingw, MSVC 14.
* This header file does not define the macros `MSG_CTRUNC` and
  `MSG_TRUNC` on some platforms:
  mingw.
* This header file does not define the macro `MSG_WAITALL` on some platforms:
  MSVC 14.
* This header file does not define the type `struct cmsghdr` and
  the constant `SCM_RIGHTS` on some platforms:
  mingw, MSVC 14.
* This header file does not define the constant `SOCK_CLOFORK`
  on some platforms:
  glibc 2.42, musl libc, macOS, FreeBSD, NetBSD, OpenBSD, AIX, Solaris 11.3, Cygwin, mingw, MSVC, Android.
* This header file does not define the constant `MSG_CMSG_CLOFORK`
  on some platforms:
  glibc 2.42, musl libc, macOS, FreeBSD, NetBSD, OpenBSD, AIX, Solaris 11.4, Cygwin, mingw, MSVC, Android.
* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the header `<sys/uio.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`sys/statvfs.h`](#sys_002fstatvfs_002eh), Previous: [`sys/socket.h`](#sys_002fsocket_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.69 `sys/stat.h` [¶](#sys_002fstat_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_stat.h.html>

Gnulib module: sys\_stat-h

Portability problems fixed by Gnulib module `sys_stat-h`:

* The type `mode_t` is not defined on some platforms:
  MSVC 14.
* The type `blksize_t` is not defined on some platforms:
  FreeBSD 5.5, OpenBSD 5.6, HP-UX 11.23, mingw, MSVC 14.
* The type `blkcnt_t` is not defined on some platforms:
  FreeBSD 5.5, OpenBSD 5.6, mingw, MSVC 14.
* Some macros, such as `S_IFMT` or `S_IFIFO`, are missing on some
  platforms.
* The macros `S_ISBLK`, `S_ISCHR`, `S_ISDIR`, `S_ISFIFO`,
  `S_ISLNK`, `S_ISREG`, `S_ISSOCK` are broken on some platforms.
* Some platforms define macros, such as `S_ISDOOR`, that are not defined
  on other platforms.
* The functions `lstat` and `mkdir` are not declared on some platforms:
  mingw, MSVC 14.
* The macros `UTIME_NOW` and `UTIME_OMIT` are missing on some
  platforms.
* On some platforms, `struct stat` does not include `st_atim`,
  `st_mtim`, or `st_ctim` members.
  Use the gnulib module
  ‘`stat-time`’ for accessors to portably get at subsecond resolution.

Portability problems fixed by Gnulib module `sys_stat-h`, together with module `windows-stat-inodes`:

* On Windows platforms (excluding Cygwin), `st_ino` is always 0.

On platforms where `off_t` is a 32-bit type, `struct stat`
cannot represent the size of files or block devices 2 GiB or lager
and may not work correctly on directories 2 GiB or larger.
Also, on platforms where `ino_t` is a 32-bit type,
`struct stat` cannot represent larger inode numbers.
See [Large File Support](#Large-File-Support), for how to address these problems.

See [Avoiding the year 2038 problem](#Avoiding-the-year-2038-problem), for portability issues with the
`time_t` components of `struct stat`.

Portability problems not fixed by Gnulib:

* On some platforms the types `blksize_t` and `blkcnt_t` are unsigned:
  QNX, Android, glibc/alpha.
* The macro `S_IFBLK` is missing on some platforms:
  MSVC 14.
* On OpenVMS, `st_ino` is an array of three `ino_t` values,
  not a single value.
  `b` are known to represent the same file.
* On some platforms, two different files may have the same `st_dev`
  and `st_ino` values, even when `st_ino` is nonzero:
  + GNU/Linux NFS servers that export all local file systems as a single
    NFS file system, if a local `st_dev` exceeds 255, or if a local
    `st_ino` exceeds 16777215.
  + Network Appliance NFS servers in snapshot directories; see Network
    Appliance bug #195.
  + ClearCase MVFS; see bug id ATRia04618.
* On some file systems, `st_size` contains bogus information for
  symlinks;
  use the Gnulib module `areadlink-with-size` for a
  better way to get symlink contents.
* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the header `<time.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

To partially work around porting problems with Microsoft Windows and OpenVMS,
you can use the Gnulib `same-inode` module to test whether two
`struct stat` objects are known to represent the same file.
For example, `psame_inode (&a, &b)` is true if the `struct stat`
objects `a` and `b` are known to represent the same file.

Another partial workaround is to compare other file metadata such as
`st_mode` and `st_mtime` on platforms where `st_dev`
and `st_ino` not uniquely identify a file. However, this does
not work reliably on files whose metadata are being changed by other
programs, or where the metadata happen to be equal.

---

Next: [`sys/time.h`](#sys_002ftime_002eh), Previous: [`sys/stat.h`](#sys_002fstat_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.70 `sys/statvfs.h` [¶](#sys_002fstatvfs_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_statvfs.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  OpenBSD 3.8, mingw, MSVC 14.

---

Next: [`sys/times.h`](#sys_002ftimes_002eh), Previous: [`sys/statvfs.h`](#sys_002fstatvfs_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.71 `sys/time.h` [¶](#sys_002ftime_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_time.h.html>

Gnulib module: sys\_time-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  MSVC 14.
* ‘`struct timeval`’ is not defined on some platforms.
* ‘`struct timeval`’ is defined with a `tv_sec` type that is
  narrower than `time_t`
  on some native Windows platforms:
  mingw64 in 64-bit mode,
  mingw64 in 32-bit mode when `__MINGW_USE_VC2005_COMPAT` is defined,
  MSVC 14 in 64-bit mode,
  MSVC 14 in 32-bit mode when `_USE_32BIT_TIME_T` is not defined.

See [Avoiding the year 2038 problem](#Avoiding-the-year-2038-problem), for portability issues with
`time_t` and the `time_t` component of `struct timeval`.

Portability problems not fixed by Gnulib:

* ‘`struct timeval`’ is defined with a `tv_sec` type that is
  wider than `time_t`:
  OpenBSD 5.1 in 64-bit mode.
* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the header `<sys/select.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`sys/types.h`](#sys_002ftypes_002eh), Previous: [`sys/time.h`](#sys_002ftime_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.72 `sys/times.h` [¶](#sys_002ftimes_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_times.h.html>

Gnulib module: sys\_times-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`sys/uio.h`](#sys_002fuio_002eh), Previous: [`sys/times.h`](#sys_002ftimes_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.73 `sys/types.h` [¶](#sys_002ftypes_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_types.h.html>

Gnulib module: sys\_types-h

Portability problems fixed by Gnulib:

* The type `pid_t` is not defined on some platforms:
  MSVC 14.
* The type `size_t` is not defined in this file on some platforms:
  MSVC 14.
* The type `ssize_t` is not defined on some platforms:
  MSVC 14.
* The type `mode_t` is not defined on some platforms:
  MSVC 14.
* The type `off64_t` is not defined on some platforms:
  macOS 14, FreeBSD 10.4, NetBSD 10.0, OpenBSD 7.9, MSVC 14, Cygwin, Haiku, Minix 3.3.
* The type `blksize_t` is not defined on some platforms:
  FreeBSD 5.5, OpenBSD 5.6, HP-UX 11.23, mingw, MSVC 14.
* The type `blkcnt_t` is not defined on some platforms:
  FreeBSD 5.5, OpenBSD 5.6, mingw, MSVC 14.
* Some systems leak definitions of `major`, `minor`, and
  `makedev` through this header; however, when
  `sys/sysmacros.h` exists, that file should also be included to
  avoid deprecation warnings from the versions in this header:
  glibc 2.25.

See [Avoiding the year 2038 problem](#Avoiding-the-year-2038-problem), for portability issues with
`time_t`.

Portability problems not fixed by Gnulib:

* On some platforms the types `blksize_t` and `blkcnt_t` are unsigned:
  QNX, Android, glibc/alpha.
* On some platforms the types `blksize_t` and `suseconds_t`
  are signed integer types that are wider than `long`:
  glibc x32

This module, together with the module `largefile`, also defines the type
`off_t` to a 64-bit integer type on some platforms:
mingw, MSVC 14.

---

Next: [`sys/un.h`](#sys_002fun_002eh), Previous: [`sys/types.h`](#sys_002ftypes_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.74 `sys/uio.h` [¶](#sys_002fuio_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_uio.h.html>

Gnulib module: sys\_uio-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.
* This header file is not self-contained (it requires `<sys/types.h>` to be
  included first) on some platforms:
  OpenBSD 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`sys/utsname.h`](#sys_002futsname_002eh), Previous: [`sys/uio.h`](#sys_002fuio_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.75 `sys/un.h` [¶](#sys_002fun_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_un.h.html>

Gnulib module: sys\_un-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.
* This header file does not define the `sa_family_t` type on some platforms:
  Cygwin 3.5.5, mingw, MSVC 14.
* This header file causes compilation errors in C++ mode on some platforms:
  glibc 2.42.

Portability problems not fixed by Gnulib:

* This header requires `sys/socket.h` to be included first on
  some platforms:
  Cygwin 1.7.18.

---

Next: [`sys/wait.h`](#sys_002fwait_002eh), Previous: [`sys/un.h`](#sys_002fun_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.76 `sys/utsname.h` [¶](#sys_002futsname_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_utsname.h.html>

Gnulib module: sys\_utsname-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.
* This header file is not self-contained on some platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`syslog.h`](#syslog_002eh), Previous: [`sys/utsname.h`](#sys_002futsname_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.77 `sys/wait.h` [¶](#sys_002fwait_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_wait.h.html>

Gnulib module: sys\_wait-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.
* This header file does not define the `WCOREDUMP` macro on some platforms:
  AIX 7.3, Haiku.

Portability problems not fixed by Gnulib:

* System status macros such as `WEXITSTATUS` require an lvalue
  argument on some platforms:
  macOS 11.1.
* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the header `<signal.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`tar.h`](#tar_002eh), Previous: [`sys/wait.h`](#sys_002fwait_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.78 `syslog.h` [¶](#syslog_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/syslog.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`termios.h`](#termios_002eh), Previous: [`syslog.h`](#syslog_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.79 `tar.h` [¶](#tar_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/tar.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  Cygwin 1.5.x, mingw, MSVC 14.

---

Next: [`tgmath.h`](#tgmath_002eh), Previous: [`tar.h`](#tar_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.80 `termios.h` [¶](#termios_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/termios.h.html>

Gnulib module: termios-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.
* This header does not declare `pid_t` on all platforms:
  glibc on some architectures, FreeBSD 6.4, OpenBSD 4.9, Cygwin 1.7.11.

Portability problems not fixed by Gnulib:

* The types `struct termios`, `struct winsize`, `cc_t`,
  `speed_t`, `tcflag_t` are not defined on some platforms:
  mingw, MSVC 14.
* `struct winsize` is defined in `sys/ioctl.h` on some platforms:
  glibc, AIX.

---

Next: [`threads.h`](#threads_002eh), Previous: [`termios.h`](#termios_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.81 `tgmath.h` [¶](#tgmath_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/tgmath.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  Mac OS X 10.5, FreeBSD 5.2.1, NetBSD 10.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 1.7.9, mingw, MSVC 14, Android 9.0.

---

Next: [`time.h`](#time_002eh), Previous: [`tgmath.h`](#tgmath_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.82 `threads.h` [¶](#threads_002eh-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/threads.h.html>

Defines the multithreading facility of ISO C11.

Gnulib module: threads-h

Portability problems fixed by Gnulib:

* This header file is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 2.2.x, mingw, MSVC 14, Android 9.0.
* This header file defines `thrd_start_t` incorrectly on some platforms:
  AIX 7.2.
* This header file does not define `TSS_DTOR_ITERATIONS` on some platforms:
  AIX 7.2.

Portability problems not fixed by Gnulib:

* There is no way to define a working `thread_local` macro on some platforms:
  + Mac OS X 10.5,
  + OpenBSD 6.5,
  + AIX 7.1 with gcc (but it works with ‘`xlc -qthreaded -qtls`’),
  + HP-UX 11.31 with cc (but it works with gcc),
  + Android 4.3.

---

Next: [`uchar.h`](#uchar_002eh), Previous: [`threads.h`](#threads_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.83 `time.h` [¶](#time_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/time.h.html>

Gnulib module: time-h

Portability problems fixed by Gnulib:

* ‘`struct timespec`’ is not defined on some platforms.
* The macro `TIME_UTC` is not defined on many platforms:
  glibc 2.15, macOS 10.13, FreeBSD 11.0, NetBSD 7.1, OpenBSD 6.0, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* Some platforms provide a `NULL` macro that cannot be used in arbitrary
  expressions:
  NetBSD 5.0.

Portability problems fixed by the Gnulib module `year2038`:

* On some platforms where `time_t` defaults to 32-bit but can be
  changed to 64-bit, functions like `stat` can fail with
  `errno == EOVERFLOW` when a 32-bit timestamp is out of range,
  such as with a file timestamp in the far future or past:
  glibc 2.34+ atop 32-bit x86 or ARM Linux.
  See [Avoiding the year 2038 problem](#Avoiding-the-year-2038-problem).

Portability problems not fixed by Gnulib:

* On some platforms `struct tm` lacks the the `tm_gmtoff` and
  `tm_zone` members:
  AIX 7.3, HP-UX 11, Solaris 11.4, mingw, MSVC 14.
* On some platforms the global state variables `daylight`,
  `timezone` and `tzname` are not available. Even on
  platforms where they are available, their contents are often unreliable,
  even in single-threaded processes.
  Portable code can instead use `struct tm`’s `tm_gmtoff` and
  `tm_zone` members when available, and the `strftime` function
  with `%z` or `%Z` conversion specifiers otherwise.
* On platforms where `time_t` is always 32-bit, functions like
  `stat` can fail with `errno == EOVERFLOW` when a timestamp
  is out of range, such as with a file timestamp in the far future or
  past; on other such platforms,
  the functions silently return the low-order 32 bits of the correct
  timestamp. These platforms will be obsolete when 32-bit `time_t`
  rolls around, which will occur in 2038 for the typical case when
  `time_t` is signed.
  See [Avoiding the year 2038 problem](#Avoiding-the-year-2038-problem).
* On some platforms the `tv_nsec` member of `struct timespec`
  is not of type `long`, but is of type `long long` instead:
  glibc x32
* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the header `<signal.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`unistd.h`](#unistd_002eh), Previous: [`time.h`](#time_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.84 `uchar.h` [¶](#uchar_002eh-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.30.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/uchar.h.html>

Defines the types `char16_t`, `char32_t`,
`mbstate_t` and `size_t`, and declares the
functions `mbrtoc16`, `c16rtomb`, `mbrtoc32`,
`c32rtomb`.

Gnulib module: uchar-h or uchar-h-c23

Portability problems fixed by either Gnulib module `uchar-h` or `uchar-h-c23`:

* This header file is missing on many non-glibc platforms:
  glibc 2.15, macOS 14, FreeBSD 6.4, NetBSD 10.0, OpenBSD 7.3, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.4.x, mingw, MSVC 9.
* This file is not self-contained on some platforms:
  Haiku.
* This file produces compilation errors in C++ mode on some platforms:
  AIX 7.2 with xlclang++.

Portability problems fixed by Gnulib module `uchar-h-c23`:

* `char32_t` values may not be Unicode code points.
  This is the case in ISO C 11 compliant but not ISO C 23 compliant
  implementations.

Portability problems fixed by Gnulib modules
`btoc32`,
`c32_apply_mapping`, `c32_apply_type_test`,
`c32_get_mapping`, `c32_get_type_test`,
`c32isalnum`, `c32isalpha`, `c32isblank`,
`c32iscntrl`, `c32isdigit`, `c32isgraph`,
`c32islower`, `c32isprint`, `c32ispunct`,
`c32isspace`, `c32isupper`, `c32isxdigit`,
`c32rtomb`, `c32snrtombs`, `c32srtombs`,
`c32stombs`, `c32swidth`, `c32tob`,
`c32tolower`, `c32toupper`, `c32width`:

* The `char32_t` type of ISO C 23 and POSIX `<uchar.h>` lacks
  counterparts of several standard `wchar_t` functions declared in
  `<wchar.h>` and `<wctype.h>`.
  This shortcoming is ameliorated by these Gnulib modules,
  each of which arranges for `<uchar.h>` to declare a
  Gnulib-specific function and to make visible the symbols `wint_t`
  and `WEOF` if the symbols are part of that function’s API.

Portability problems not fixed by Gnulib:

* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the headers `<stddef.h>`, `<stdint.h>`, and
  `<wchar.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`utmpx.h`](#utmpx_002eh), Previous: [`uchar.h`](#uchar_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.85 `unistd.h` [¶](#unistd_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/unistd.h.html>

Gnulib module: unistd-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  MSVC 14.
* The type `off64_t` is not defined on some platforms:
  macOS 14, FreeBSD 10.4, NetBSD 10.0, OpenBSD 7.9, MSVC 14, Cygwin, Haiku, Minix 3.3.
* The SEEK\_\* macros are not defined in this file on some platforms:
  mingw.
* The \*\_FILENO macros are not defined in this file on some platforms:
  OS/2 EMX, mingw.
* The `_exit` function is not declared in this file on some platforms:
  mingw.
* This header file does not define `O_CLOEXEC` on some platforms:
  glibc 2.42, macOS 12, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5, mingw.
* Some platforms provide a `NULL` macro that cannot be used in arbitrary
  expressions:
  NetBSD 5.0

Portability problems not fixed by Gnulib:

* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the headers `<fcntl.h>`, `<stddef.h>`,
  `<stdint.h>`, and `<stdio.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`wchar.h`](#wchar_002eh), Previous: [`unistd.h`](#unistd_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.86 `utmpx.h` [¶](#utmpx_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/utmpx.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  FreeBSD 8.4, OpenBSD 7.2, Minix 3.1.8, mingw, MSVC 14, Android 13.0.
* While some platforms have the `struct utmpx` field `ut_user`,
  older platforms have the field `ut_name`.
* The `struct utmpx` field `ut_exit` does not exist on some platforms:
  macOS, FreeBSD, AIX, Cygwin.
* The `struct utmpx` field `ut_session` does not exist
  on some platforms:
  macOS, FreeBSD, AIX, HP-UX, Cygwin.
* The `struct utmpx` field `ut_addr` or `ut_addr_v6` or
  `ut_ss` does not exist on some platforms:
  macOS, FreeBSD, AIX, Solaris.
* On some platforms, the `struct utmpx` field `ut_tv` is not
  of type `struct timeval`. Instead, it is a different
  struct with `tv_sec` and `tv_usec` members that may
  have different types than the members of `struct timeval`:
  glibc 2.42 on platforms where `time_t` was historically 32 bits
  and where log file formats were not changed when 64-bit `time_t`
  was introduced.
* On some platforms, this API does not support timestamps past the
  year 2038:
  glibc 2.39 on 32-bit platforms like x86 and ARM where `time_t`
  was historically 32 bits; later glibc versions support
  timestamps up to the year 2106, by changing `ut_tv.tv_sec`’s type
  to be a 32-bit unsigned integer.
* On some platforms, this header misbehaves if the `year2038` or
  `year2038-recommended` modules are used and the program is
  configured without the `--disable-year2038` option.
  The `readutmp` module works around this problem:
  glibc 2.38 on 32-bit platforms like x86 and ARM where `time_t`
  was historically 32 bits.
  See [Avoiding the year 2038 problem](#Avoiding-the-year-2038-problem).
* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the header `<sys/time.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`wctype.h`](#wctype_002eh), Previous: [`utmpx.h`](#utmpx_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.87 `wchar.h` [¶](#wchar_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/wchar.h.html>

Gnulib module: wchar-h

Portability problems fixed by Gnulib:

* This header file cannot be included on some platforms:
  Linux uClibc built without wide character support.
* The type `wint_t` is incorrect on some platforms:
  mingw, MSVC 14.
* Some platforms provide a `NULL` macro that cannot be used in arbitrary
  expressions:
  NetBSD 5.0

Portability problems not fixed by Gnulib:

* In multibyte locales, `wchar_t` values need not be Unicode code points:
  FreeBSD 15, NetBSD 10, AIX, Solaris.
* In single-byte locales, `wchar_t` values need not be Unicode code points:
  FreeBSD 15, NetBSD 10, OpenBSD 7, macOS 26, Solaris, mingw, MSVC.
* `wchar_t` is only 16 bits, and so can represent only a
  UTF-16 subset of Unicode when `wchar_t` values are Unicode code points:
  32-bit AIX, mingw, MSVC.
* This header file leads to link errors and endless recursions or endless loops
  on some platforms:
  glibc version 2.5 or older, together with gcc version 4.3 or newer and the
  option ‘`-std=c99`’ or ‘`-std=gnu99`’.
* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the headers `<ctype.h>`, `<stdarg.h>`,
  `<stddef.h>`, `<stdlib.h>`, `<stdio.h>`,
  `<string.h>`, and `<time.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`wordexp.h`](#wordexp_002eh), Previous: [`wchar.h`](#wchar_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.88 `wctype.h` [¶](#wctype_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/wctype.h.html>

Gnulib module: wctype-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  HP-UX 11.00.
* The type `wint_t` is incorrect on some platforms:
  mingw, MSVC 14.
* The functions `isw*` are missing on some platforms:
  FreeBSD 4.11.

Portability problems not fixed by Gnulib:

* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the headers `<ctype.h>`, `<stdarg.h>`,
  `<stddef.h>`, `<stdlib.h>`, `<stdio.h>`,
  `<string.h>`, `<time.h>`, and `<wchar.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Previous: [`wctype.h`](#wctype_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.89 `wordexp.h` [¶](#wordexp_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/wordexp.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [Past POSIX Header File Substitutes](#Legacy-Header-File-Substitutes), Previous: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 10 ISO C and POSIX Function Substitutes [¶](#ISO-C-and-POSIX-Function-Substitutes)

This chapter describes which functions and function-like macros specified by
ISO C (including ISO TS 18661-1) or POSIX are substituted by Gnulib, which
portability pitfalls are fixed by Gnulib, and which (known) portability
problems are not worked around by Gnulib.

The notation “Gnulib module: —” means that Gnulib does not provide a
module providing a substitute for the function. When the list
“Portability problems not fixed by Gnulib” is empty, such a module is
not needed: No portability problems are known. Otherwise, it indicates
that such a module would be useful but is not available: No one so far
found this function important enough to contribute a substitute for it.
If you need this particular function, you may write to
`<bug-gnulib at gnu dot org>`.

This list of functions is sorted according to the header that declares them.

* [Functions in `<aio.h>`](#Functions-in-_003caio_002eh_003e)
* [Functions in `<arpa/inet.h>`](#Functions-in-_003carpa_002finet_002eh_003e)
* [Functions in `<assert.h>`](#Functions-in-_003cassert_002eh_003e)
* [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e)
* [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e)
* [Functions in `<devctl.h>`](#Functions-in-_003cdevctl_002eh_003e)
* [Functions in `<dirent.h>`](#Functions-in-_003cdirent_002eh_003e)
* [Functions in `<dlfcn.h>`](#Functions-in-_003cdlfcn_002eh_003e)
* [Functions in `<endian.h>`](#Functions-in-_003cendian_002eh_003e)
* [Functions in `<errno.h>`](#Functions-in-_003cerrno_002eh_003e)
* [Functions in `<fcntl.h>`](#Functions-in-_003cfcntl_002eh_003e)
* [Functions in `<fenv.h>`](#Functions-in-_003cfenv_002eh_003e)
* [Functions in `<fmtmsg.h>`](#Functions-in-_003cfmtmsg_002eh_003e)
* [Functions in `<fnmatch.h>`](#Functions-in-_003cfnmatch_002eh_003e)
* [Functions in `<ftw.h>`](#Functions-in-_003cftw_002eh_003e)
* [Functions in `<glob.h>`](#Functions-in-_003cglob_002eh_003e)
* [Functions in `<grp.h>`](#Functions-in-_003cgrp_002eh_003e)
* [Functions in `<iconv.h>`](#Functions-in-_003ciconv_002eh_003e)
* [Functions in `<inttypes.h>`](#Functions-in-_003cinttypes_002eh_003e)
* [Functions in `<langinfo.h>`](#Functions-in-_003clanginfo_002eh_003e)
* [Functions in `<libgen.h>`](#Functions-in-_003clibgen_002eh_003e)
* [Functions in `<libintl.h>`](#Functions-in-_003clibintl_002eh_003e)
* [Functions in `<locale.h>`](#Functions-in-_003clocale_002eh_003e)
* [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)
* [Functions in `<monetary.h>`](#Functions-in-_003cmonetary_002eh_003e)
* [Functions in `<mqueue.h>`](#Functions-in-_003cmqueue_002eh_003e)
* [Functions in `<ndbm.h>`](#Functions-in-_003cndbm_002eh_003e)
* [Functions in `<net/if.h>`](#Functions-in-_003cnet_002fif_002eh_003e)
* [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)
* [Functions in `<netinet/in.h>`](#Functions-in-_003cnetinet_002fin_002eh_003e)
* [Functions in `<nl_types.h>`](#Functions-in-_003cnl_005ftypes_002eh_003e)
* [Functions in `<poll.h>`](#Functions-in-_003cpoll_002eh_003e)
* [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)
* [Functions in `<pwd.h>`](#Functions-in-_003cpwd_002eh_003e)
* [Functions in `<regex.h>`](#Functions-in-_003cregex_002eh_003e)
* [Functions in `<sched.h>`](#Functions-in-_003csched_002eh_003e)
* [Functions in `<search.h>`](#Functions-in-_003csearch_002eh_003e)
* [Functions in `<semaphore.h>`](#Functions-in-_003csemaphore_002eh_003e)
* [Functions in `<setjmp.h>`](#Functions-in-_003csetjmp_002eh_003e)
* [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)
* [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)
* [Functions in `<stdarg.h>`](#Functions-in-_003cstdarg_002eh_003e)
* [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)
* [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)
* [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e)
* [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e)
* [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)
* [Functions in `<strings.h>`](#Functions-in-_003cstrings_002eh_003e)
* [Functions in `<sys/ipc.h>`](#Functions-in-_003csys_002fipc_002eh_003e)
* [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)
* [Functions in `<sys/msg.h>`](#Functions-in-_003csys_002fmsg_002eh_003e)
* [Functions in `<sys/resource.h>`](#Functions-in-_003csys_002fresource_002eh_003e)
* [Functions in `<sys/select.h>`](#Functions-in-_003csys_002fselect_002eh_003e)
* [Functions in `<sys/sem.h>`](#Functions-in-_003csys_002fsem_002eh_003e)
* [Functions in `<sys/shm.h>`](#Functions-in-_003csys_002fshm_002eh_003e)
* [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)
* [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)
* [Functions in `<sys/statvfs.h>`](#Functions-in-_003csys_002fstatvfs_002eh_003e)
* [Functions in `<sys/time.h>`](#Functions-in-_003csys_002ftime_002eh_003e)
* [Functions in `<sys/times.h>`](#Functions-in-_003csys_002ftimes_002eh_003e)
* [Functions in `<sys/uio.h>`](#Functions-in-_003csys_002fuio_002eh_003e)
* [Functions in `<sys/utsname.h>`](#Functions-in-_003csys_002futsname_002eh_003e)
* [Functions in `<sys/wait.h>`](#Functions-in-_003csys_002fwait_002eh_003e)
* [Functions in `<syslog.h>`](#Functions-in-_003csyslog_002eh_003e)
* [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e)
* [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)
* [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)
* [Functions in `<uchar.h>`](#Functions-in-_003cuchar_002eh_003e)
* [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e)
* [Functions in `<utmpx.h>`](#Functions-in-_003cutmpx_002eh_003e)
* [Functions in `<wchar.h>`](#Functions-in-_003cwchar_002eh_003e)
* [Functions in `<wctype.h>`](#Functions-in-_003cwctype_002eh_003e)
* [Functions in `<wordexp.h>`](#Functions-in-_003cwordexp_002eh_003e)

---

Next: [Functions in `<arpa/inet.h>`](#Functions-in-_003carpa_002finet_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.1 Functions in `<aio.h>` [¶](#Functions-in-_003caio_002eh_003e-1)

* [`aio_cancel`](#aio_005fcancel)
* [`aio_error`](#aio_005ferror)
* [`aio_fsync`](#aio_005ffsync)
* [`aio_read`](#aio_005fread)
* [`aio_return`](#aio_005freturn)
* [`aio_suspend`](#aio_005fsuspend)
* [`aio_write`](#aio_005fwrite)
* [`lio_listio`](#lio_005flistio)

---

Next: [`aio_error`](#aio_005ferror), Up: [Functions in `<aio.h>`](#Functions-in-_003caio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.1.1 `aio_cancel` [¶](#aio_005fcancel-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/aio_cancel.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.10.x, mingw, MSVC 14, Android 9.0.

---

Next: [`aio_fsync`](#aio_005ffsync), Previous: [`aio_cancel`](#aio_005fcancel), Up: [Functions in `<aio.h>`](#Functions-in-_003caio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.1.2 `aio_error` [¶](#aio_005ferror-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/aio_error.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, Cygwin 2.10.x, mingw, MSVC 14, Android 9.0.

---

Next: [`aio_read`](#aio_005fread), Previous: [`aio_error`](#aio_005ferror), Up: [Functions in `<aio.h>`](#Functions-in-_003caio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.1.3 `aio_fsync` [¶](#aio_005ffsync-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/aio_fsync.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, Cygwin 2.10.x, mingw, MSVC 14, Android 9.0.

---

Next: [`aio_return`](#aio_005freturn), Previous: [`aio_fsync`](#aio_005ffsync), Up: [Functions in `<aio.h>`](#Functions-in-_003caio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.1.4 `aio_read` [¶](#aio_005fread-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/aio_read.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.10.x, mingw, MSVC 14, Android 9.0.

---

Next: [`aio_suspend`](#aio_005fsuspend), Previous: [`aio_read`](#aio_005fread), Up: [Functions in `<aio.h>`](#Functions-in-_003caio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.1.5 `aio_return` [¶](#aio_005freturn-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/aio_return.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, Cygwin 2.10.x, mingw, MSVC 14, Android 9.0.

---

Next: [`aio_write`](#aio_005fwrite), Previous: [`aio_return`](#aio_005freturn), Up: [Functions in `<aio.h>`](#Functions-in-_003caio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.1.6 `aio_suspend` [¶](#aio_005fsuspend-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/aio_suspend.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.10.x, mingw, MSVC 14, Android 9.0.

---

Next: [`lio_listio`](#lio_005flistio), Previous: [`aio_suspend`](#aio_005fsuspend), Up: [Functions in `<aio.h>`](#Functions-in-_003caio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.1.7 `aio_write` [¶](#aio_005fwrite-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/aio_write.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.10.x, mingw, MSVC 14, Android 9.0.

---

Previous: [`aio_write`](#aio_005fwrite), Up: [Functions in `<aio.h>`](#Functions-in-_003caio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.1.8 `lio_listio` [¶](#lio_005flistio-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lio_listio.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files 2 GiB and larger. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.10.x, mingw, MSVC 14, Android 9.0.

---

Next: [Functions in `<assert.h>`](#Functions-in-_003cassert_002eh_003e), Previous: [Functions in `<aio.h>`](#Functions-in-_003caio_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.2 Functions in `<arpa/inet.h>` [¶](#Functions-in-_003carpa_002finet_002eh_003e-1)

* [`htonl`](#htonl)
* [`htons`](#htons)
* [`inet_addr`](#inet_005faddr)
* [`inet_ntoa`](#inet_005fntoa)
* [`inet_ntop`](#inet_005fntop)
* [`inet_pton`](#inet_005fpton)
* [`ntohl`](#ntohl)
* [`ntohs`](#ntohs)

---

Next: [`htons`](#htons), Up: [Functions in `<arpa/inet.h>`](#Functions-in-_003carpa_002finet_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.2.1 `htonl` [¶](#htonl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/htonl.html>

Gnulib module: htonl

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11, Android API level 20.
* This function is declared in `<winsock2.h>` instead of `<arpa/inet.h>`
  on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`inet_addr`](#inet_005faddr), Previous: [`htonl`](#htonl), Up: [Functions in `<arpa/inet.h>`](#Functions-in-_003carpa_002finet_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.2.2 `htons` [¶](#htons-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/htons.html>

Gnulib module: htonl

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11, Android API level 20.
* This function is declared in `<winsock2.h>` instead of `<arpa/inet.h>`
  on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`inet_ntoa`](#inet_005fntoa), Previous: [`htons`](#htons), Up: [Functions in `<arpa/inet.h>`](#Functions-in-_003carpa_002finet_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.2.3 `inet_addr` [¶](#inet_005faddr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/inet_addr.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is declared in `<winsock2.h>` instead of `<arpa/inet.h>`
  on some platforms:
  mingw, MSVC 14.
* POSIX.1-2024 says this function is obsolescent and it is planned to be
  removed in a future version.
  Use the function `inet_pton` instead.
* On some old platforms, this function returns a ‘`struct in_addr`’ rather
  than a scalar type such as ‘`unsigned int`’ or ‘`unsigned long`’.

---

Next: [`inet_ntop`](#inet_005fntop), Previous: [`inet_addr`](#inet_005faddr), Up: [Functions in `<arpa/inet.h>`](#Functions-in-_003carpa_002finet_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.2.4 `inet_ntoa` [¶](#inet_005fntoa-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/inet_ntoa.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is declared in `<winsock2.h>` instead of `<arpa/inet.h>`
  on some platforms:
  mingw, MSVC 14.
* POSIX.1-2024 says this function is obsolescent and it is planned to be
  removed in a future version.
  Use the function `inet_ntop` instead.
* The `inet_ntoa` function need not be reentrant, and consequently
  is not required to be thread safe. Implementations of
  `inet_ntoa` typically write the timestamp into static buffer.
  If two threads call `inet_ntoa` at roughly the same time, you
  might end up with the wrong date in one of the threads, or some
  undefined string.

Note: `inet_ntoa` is specific for IPv4 addresses.
A protocol independent function is `inet_ntop`.

---

Next: [`inet_pton`](#inet_005fpton), Previous: [`inet_ntoa`](#inet_005fntoa), Up: [Functions in `<arpa/inet.h>`](#Functions-in-_003carpa_002finet_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.2.5 `inet_ntop` [¶](#inet_005fntop-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/inet_ntop.html>

Gnulib module: inet\_ntop

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.00, mingw, MSVC 14.
* This function is declared in `<netdb.h>` instead of `<arpa/inet.h>`
  on some platforms:
  NonStop Kernel.
* This function is declared in `<ws2tcpip.h>`, with a POSIX incompatible
  declaration, on some platforms:
  MSVC 14 on Windows >= Vista.

Portability problems not fixed by Gnulib:

* This function’s fourth argument type is `size_t` instead of
  `socklen_t` on some platforms:
  Solaris 10.

---

Next: [`ntohl`](#ntohl), Previous: [`inet_ntop`](#inet_005fntop), Up: [Functions in `<arpa/inet.h>`](#Functions-in-_003carpa_002finet_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.2.6 `inet_pton` [¶](#inet_005fpton-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/inet_pton.html>

Gnulib module: inet\_pton

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.00, mingw, MSVC 14.
* This function is declared in `<netdb.h>` instead of `<arpa/inet.h>`
  on some platforms:
  NonStop Kernel.
* This function is declared in `<ws2tcpip.h>`, with a POSIX incompatible
  declaration, on some platforms:
  MSVC 14 on Windows >= Vista.

Portability problems not fixed by Gnulib:

---

Next: [`ntohs`](#ntohs), Previous: [`inet_pton`](#inet_005fpton), Up: [Functions in `<arpa/inet.h>`](#Functions-in-_003carpa_002finet_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.2.7 `ntohl` [¶](#ntohl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ntohl.html>

Gnulib module: htonl

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11, Android API level 20.
* This function is declared in `<winsock2.h>` instead of `<arpa/inet.h>`
  on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Previous: [`ntohl`](#ntohl), Up: [Functions in `<arpa/inet.h>`](#Functions-in-_003carpa_002finet_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.2.8 `ntohs` [¶](#ntohs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ntohs.html>

Gnulib module: htonl

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11, Android API level 20.
* This function is declared in `<winsock2.h>` instead of `<arpa/inet.h>`
  on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [Functions in `<complex.h>`](#Functions-in-_003ccomplex_002eh_003e), Previous: [Functions in `<arpa/inet.h>`](#Functions-in-_003carpa_002finet_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.3 Functions in `<assert.h>` [¶](#Functions-in-_003cassert_002eh_003e-1)

* [`assert`](#assert)

---

Up: [Functions in `<assert.h>`](#Functions-in-_003cassert_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.3.1 `assert` [¶](#assert-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/assert.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

Extension: Gnulib offers a module ‘`assert`’ that allows the installer to
disable assertions through a ‘`configure`’ option: ‘`--disable-assert`’.

---

Next: [Functions in `<ctype.h>`](#Functions-in-_003cctype_002eh_003e), Previous: [Functions in `<assert.h>`](#Functions-in-_003cassert_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
