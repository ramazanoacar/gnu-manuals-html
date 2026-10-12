### 10.56 Functions in `<sys/socket.h>` [¶](#Functions-in-_003csys_002fsocket_002eh_003e-1)

* [`accept`](#accept)
* [`accept4`](#accept4)
* [`bind`](#bind)
* [`connect`](#connect)
* [`getpeername`](#getpeername)
* [`getsockname`](#getsockname)
* [`getsockopt`](#getsockopt)
* [`listen`](#listen)
* [`recv`](#recv)
* [`recvfrom`](#recvfrom)
* [`recvmsg`](#recvmsg)
* [`send`](#send)
* [`sendmsg`](#sendmsg)
* [`sendto`](#sendto)
* [`setsockopt`](#setsockopt)
* [`shutdown`](#shutdown)
* [`sockatmark`](#sockatmark)
* [`socket`](#socket)
* [`socketpair`](#socketpair)

---

Next: [`accept4`](#accept4), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.1 `accept` [¶](#accept-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/accept.html>

Gnulib module: accept

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/socket.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), the descriptors returned by
  the `accept` function cannot be used in calls to `read`,
  `write`, and `close`; you have to use `recv`, `send`,
  `closesocket` in these cases instead.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.
* On HP-UX 11, in 64-bit mode, when the macro `_HPUX_ALT_XOPEN_SOCKET_API`
  is not defined, this function behaves incorrectly because it is declared
  to take a pointer to a 64-bit wide `socklen_t` entity but in fact
  considers it as a pointer to a 32-bit wide `unsigned int` entity.

Portability problems not fixed by Gnulib:

* Some platforms don’t have a `socklen_t` type; in this case this function’s
  third argument type is ‘`int *`’.
* On some platforms, this function’s third argument type is ‘`void *`’,
  not ‘`socklen_t *`’:
  Solaris 10, Solaris 11 OmniOS.

---

Next: [`bind`](#bind), Previous: [`accept`](#accept), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.2 `accept4` [¶](#accept4-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/accept4.html>

Documentation:  
 [man accept4](https://www.kernel.org/doc/man-pages/online/pages/man2/accept4.2.html)

Gnulib module: accept4

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 6.0, NetBSD 7.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.1, mingw, MSVC 14, Android 4.4.
  But the replacement function is not atomic; this matters in multi-threaded
  processes that spawn child processes.

Portability problems not fixed by Gnulib:

* SOCK\_CLOEXEC and SOCK\_NONBLOCK may not be defined
  as they’re also significant to the socket() function.

---

Next: [`connect`](#connect), Previous: [`accept4`](#accept4), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.3 `bind` [¶](#bind-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/bind.html>

Gnulib module: bind

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/socket.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.

Portability problems not fixed by Gnulib:

---

Next: [`getpeername`](#getpeername), Previous: [`bind`](#bind), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.4 `connect` [¶](#connect-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/connect.html>

Gnulib module: connect

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/socket.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.

Portability problems not fixed by Gnulib:

---

Next: [`getsockname`](#getsockname), Previous: [`connect`](#connect), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.5 `getpeername` [¶](#getpeername-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getpeername.html>

Gnulib module: getpeername

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/socket.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.
* On HP-UX 11, in 64-bit mode, when the macro `_HPUX_ALT_XOPEN_SOCKET_API`
  is not defined, this function behaves incorrectly because it is declared
  to take a pointer to a 64-bit wide `socklen_t` entity but in fact
  considers it as a pointer to a 32-bit wide `unsigned int` entity.

Portability problems not fixed by Gnulib:

* Some platforms don’t have a `socklen_t` type; in this case this function’s
  third argument type is ‘`int *`’.
* On some platforms, this function’s third argument type is ‘`void *`’,
  not ‘`socklen_t *`’:
  Solaris 11 OmniOS.

---

Next: [`getsockopt`](#getsockopt), Previous: [`getpeername`](#getpeername), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.6 `getsockname` [¶](#getsockname-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getsockname.html>

Gnulib module: getsockname

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/socket.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.
* On HP-UX 11, in 64-bit mode, when the macro `_HPUX_ALT_XOPEN_SOCKET_API`
  is not defined, this function behaves incorrectly because it is declared
  to take a pointer to a 64-bit wide `socklen_t` entity but in fact
  considers it as a pointer to a 32-bit wide `unsigned int` entity.

Portability problems not fixed by Gnulib:

* Some platforms don’t have a `socklen_t` type; in this case this function’s
  third argument type is ‘`int *`’.
* On some platforms, this function’s third argument type is ‘`void *`’,
  not ‘`socklen_t *`’:
  Solaris 11 OmniOS.

---

Next: [`listen`](#listen), Previous: [`getsockname`](#getsockname), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.7 `getsockopt` [¶](#getsockopt-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getsockopt.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getsockopt-1.html>

Gnulib module: getsockopt

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/socket.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.
* On HP-UX 11, in 64-bit mode, when the macro `_HPUX_ALT_XOPEN_SOCKET_API`
  is not defined, this function behaves incorrectly because it is declared
  to take a pointer to a 64-bit wide `socklen_t` entity but in fact
  considers it as a pointer to a 32-bit wide `unsigned int` entity.

Portability problems not fixed by Gnulib:

* Some platforms don’t have a `socklen_t` type; in this case this function’s
  fifth argument type is ‘`int *`’.
* On some platforms, this function’s fifth argument type is ‘`void *`’,
  not ‘`socklen_t *`’:
  Solaris 11 OmniOS.
* Many socket options are not available on all platforms.

---

Next: [`recv`](#recv), Previous: [`getsockopt`](#getsockopt), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.8 `listen` [¶](#listen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/listen.html>

Gnulib module: listen

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/socket.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.

Portability problems not fixed by Gnulib:

---

Next: [`recvfrom`](#recvfrom), Previous: [`listen`](#listen), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.9 `recv` [¶](#recv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/recv.html>

Gnulib module: recv

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/socket.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.

Portability problems not fixed by Gnulib:

---

Next: [`recvmsg`](#recvmsg), Previous: [`recv`](#recv), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.10 `recvfrom` [¶](#recvfrom-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/recvfrom.html>

Gnulib module: recvfrom

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/socket.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.
* On HP-UX 11, in 64-bit mode, when the macro `_HPUX_ALT_XOPEN_SOCKET_API`
  is not defined, this function behaves incorrectly because it is declared
  to take a pointer to a 64-bit wide `socklen_t` entity but in fact
  considers it as a pointer to a 32-bit wide `unsigned int` entity.

Portability problems not fixed by Gnulib:

* Some platforms don’t have a `socklen_t` type; in this case this function’s
  sixth argument type is ‘`int *`’.
* On some platforms, this function’s sixth argument type is ‘`void *`’,
  not ‘`socklen_t *`’:
  Solaris 11 OmniOS.

---

Next: [`send`](#send), Previous: [`recvfrom`](#recvfrom), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.11 `recvmsg` [¶](#recvmsg-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/recvmsg.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`sendmsg`](#sendmsg), Previous: [`recvmsg`](#recvmsg), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.12 `send` [¶](#send-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/send.html>

Gnulib module: send

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/socket.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.

Portability problems not fixed by Gnulib:

---

Next: [`sendto`](#sendto), Previous: [`send`](#send), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.13 `sendmsg` [¶](#sendmsg-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sendmsg.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`setsockopt`](#setsockopt), Previous: [`sendmsg`](#sendmsg), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.14 `sendto` [¶](#sendto-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sendto.html>

Gnulib module: sendto

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/socket.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.

Portability problems not fixed by Gnulib:

---

Next: [`shutdown`](#shutdown), Previous: [`sendto`](#sendto), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.15 `setsockopt` [¶](#setsockopt-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setsockopt.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-setsockopt-2.html>

Gnulib module: setsockopt

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/socket.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.

Portability problems not fixed by Gnulib:

* Many socket options are not available on all platforms.

---

Next: [`sockatmark`](#sockatmark), Previous: [`setsockopt`](#setsockopt), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.16 `shutdown` [¶](#shutdown-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/shutdown.html>

Gnulib module: shutdown

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/socket.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.

Portability problems not fixed by Gnulib:

---

Next: [`socket`](#socket), Previous: [`shutdown`](#shutdown), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.17 `sockatmark` [¶](#sockatmark-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sockatmark.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11.23, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.

---

Next: [`socketpair`](#socketpair), Previous: [`sockatmark`](#sockatmark), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.18 `socket` [¶](#socket-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/socket.html>

Gnulib module: socket

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/socket.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), the descriptors returned by
  the `socket` function cannot be used in calls to `read`,
  `write`, and `close`; you have to use `recv`, `send`,
  `closesocket` in these cases instead.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.

Portability problems not fixed by Gnulib:

---

Previous: [`socket`](#socket), Up: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.56.19 `socketpair` [¶](#socketpair-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/socketpair.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [Functions in `<sys/statvfs.h>`](#Functions-in-_003csys_002fstatvfs_002eh_003e), Previous: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.57 Functions in `<sys/stat.h>` [¶](#Functions-in-_003csys_002fstat_002eh_003e-1)

* [`chmod`](#chmod)
* [`fchmod`](#fchmod)
* [`fchmodat`](#fchmodat)
* [`fstat`](#fstat)
* [`fstatat`](#fstatat)
* [`futimens`](#futimens)
* [`lstat`](#lstat)
* [`mkdir`](#mkdir)
* [`mkdirat`](#mkdirat)
* [`mkfifo`](#mkfifo)
* [`mkfifoat`](#mkfifoat)
* [`mknod`](#mknod)
* [`mknodat`](#mknodat)
* [`stat`](#stat)
* [`umask`](#umask)
* [`utimensat`](#utimensat)

---

Next: [`fchmod`](#fchmod), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.1 `chmod` [¶](#chmod-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/chmod.html>

Gnulib module: chmod

Portability problems fixed by Gnulib:

* This function does not fail when the file name argument ends in a slash
  and (without the slash) names a non-directory, on some platforms:
  AIX 7.2.
* This function fails with a wrong error code (`EINVAL` instead of `ENOTDIR`)
  when the file name argument ends in a slash and (without the slash) names
  a non-directory, on some platforms:
  mingw, MSVC.

Portability problems not fixed by Gnulib:

* This function sometimes fails with `EACCES` when the failure is
  due to lack of appropriate privileges (`EPERM`), not to
  search permission denied on the file name prefix (`EACCES`):
  Linux kernel 5.15 with glibc 2.35 and a CIFS v1 file system
  (see <https://bugs.gnu.org/65599>).

---

Next: [`fchmodat`](#fchmodat), Previous: [`chmod`](#chmod), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.2 `fchmod` [¶](#fchmod-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fchmod.html>

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

Next: [`fstat`](#fstat), Previous: [`fchmod`](#fchmod), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.3 `fchmodat` [¶](#fchmodat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fchmodat.html>

Gnulib module: fchmodat

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8,
  AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14.
  But the replacement function is not safe to be used in libraries and is not
  thread-safe.
* This function does not fail when the file name argument ends in a slash
  and (without the slash) names a non-directory, on some platforms:
  AIX 7.2.
* When given the `AT_SYMLINK_NOFOLLOW` flag,
  this function fails with `errno` set to `ENOTSUP`,
  even when the file is not a symbolic link:
  GNU/Linux with glibc 2.31, Cygwin 2.9.

Portability problems not fixed by Gnulib:

* Some platforms do not allow changing the access bits on symbolic
  links.
* This function does not support `AT_EMPTY_PATH`:
  Linux kernel 2.6.38.
* When `AT_EMPTY_PATH` is used,
  this function does not allow the file name to be a null pointer:
  Linux kernel 6.10, Cygwin 3.6, FreeBSD 15.1.
* If the `AT_SYMLINK_NOFOLLOW` flag is specified,
  this function can fail with `errno` set to `EMFILE` or `ENFILE`,
  and it fails with `errno` set to `EOPNOTSUPP` if the
  `/proc` file system is not mounted:
  GNU/Linux with glibc 2.34.
* This function sometimes fails with `EACCES` when the failure is
  due to lack of appropriate privileges (`EPERM`), not to
  search permission denied on the file name prefix (`EACCES`):
  Linux kernel 5.15 with glibc 2.35 and a CIFS v1 file system
  (see <https://bugs.gnu.org/65599>).

---

Next: [`fstatat`](#fstatat), Previous: [`fchmodat`](#fchmodat), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.4 `fstat` [¶](#fstat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fstat.html>

Gnulib module: fstat

Portability problems fixed by Gnulib:

* This function crashes when invoked with invalid arguments on some platforms:
  MSVC 14.
* On platforms where `off_t` is a 32-bit type, `fstat` may not correctly
  report the size of files or block devices 2 GiB and larger..
  See [Large File Support](#Large-File-Support).
* On Linux/x86 and Linux/x86\_64, applications compiled in 32-bit mode cannot
  access files that happen to have a 64-bit inode number. This can occur with
  file systems such as XFS (typically on large disks) and NFS.
  See [Large File Support](#Large-File-Support).
* On macOS 12.6, when this function yields a timestamp with a
  nonpositive `tv_sec` value, `tv_nsec` might be in the range
  −999999999..−1, representing a negative nanoseconds
  offset from `tv_sec`. Solaris 11.4 is similar, except that
  `tv_sec` might also be −1000000000.
* The `st_atime`, `st_ctime`, `st_mtime` fields are affected by
  the current time zone and by the DST flag of the current time zone on some
  platforms:
  mingw, MSVC 14 (when the environment variable `TZ` is set).

Portability problems not fixed by Gnulib:

* See [`sys/stat.h`](#sys_002fstat_002eh), for general portability problems with `struct stat`.
* This function fails if the file descriptor was opened with `O_PATH`:
  Linux kernel 3.5.
* On Cygwin, `fstat` applied to the file descriptors 0 and 1, returns
  different `st_ino` values, even if standard input and standard output
  are not redirected and refer to the same terminal.

---

Next: [`futimens`](#futimens), Previous: [`fstat`](#fstat), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.5 `fstatat` [¶](#fstatat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fstatat.html>

Gnulib module: fstatat

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8,
  AIX 5.1, HP-UX 11, Cygwin 1.5.x, mingw, MSVC 14.
  But when the function is missing, its Gnulib substitute
  is not safe to be used in libraries and is not
  thread-safe.
* On platforms with `AT_EMPTY_PATH`,
  this function does not work when `AT_EMPTY_PATH`
  is used with a null pointer file name:
  glibc 2.40, Linux kernel 6.10, Cygwin 3.6, FreeBSD 15.1.
* On platforms where `off_t` is a 32-bit type, `fstatat` may
  not correctly report the size of files or block devices larger than 2
  GB. See [Large File Support](#Large-File-Support).
* On some platforms, `fstatat(fd,"file/",buf,flag)` succeeds instead of
  failing with `ENOTDIR`.
  Solaris 9.
* For symlinks, when the argument ends in a slash, some platforms don’t
  dereference the argument:
  Solaris 9.
* On macOS 12.6, when this function yields a timestamp with a
  nonpositive `tv_sec` value, `tv_nsec` might be in the range
  −999999999..−1, representing a negative nanoseconds
  offset from `tv_sec`. Solaris 11.4 is similar, except that
  `tv_sec` might also be −1000000000.

Portability problems not fixed by Gnulib:

* This function does not fail when the second argument is an empty string
  on some platforms, even when `AT_EMPTY_PATH` is not used:
  glibc 2.7, Linux kernel 2.6.38.
* This function sets `st_ino` only to the low-order 32 bits of
  the inode number of a socket or pipe, which thus can disagree
  with the `st_ino` obtained by `fstat`:
  macOS 14.
* See [`sys/stat.h`](#sys_002fstat_002eh), for general portability problems with `struct stat`.

---

Next: [`lstat`](#lstat), Previous: [`fstatat`](#fstatat), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.6 `futimens` [¶](#futimens-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/futimens.html>

Gnulib module: futimens

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.5, Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 4.3.
  However, the replacement function may end up truncating timestamps to
  worse resolution than supported by the file system.
* This function returns a bogus value instead of failing with
  `ENOSYS` on some platforms:
  Linux kernel 2.6.21.
* When using `UTIME_OMIT` or `UTIME_NOW`, some systems require
  the `tv_sec` argument to be 0, and don’t necessarily handle all
  file permissions in the manner required by POSIX:
  Linux kernel 2.6.25.
* When using `UTIME_OMIT` for the modification time, but specifying
  an access time, some systems fail to update the change time:
  Linux kernel 2.6.32, macOS 14, NetBSD 10.0, Solaris 11.1.
* Passing `AT_FDCWD` as the fd argument does not properly fail with
  `EBADF` on some systems:
  glibc 2.11, musl libc, Solaris 11.
* Out-of-range values of `tv_nsec` do not lead to a failure on some
  platforms:
  NetBSD 10.0,
  GNU/Hurd with glibc 2.41.

Portability problems not fixed by Gnulib:

* On file systems mounted with the `noatime` attribute,
  this function might not modify the access time as requested:
  Linux kernel 6.9.
* This function sets a slightly different time stamp than the requested one:
  Haiku.
* Some platforms lack the ability to change the timestamps of a file
  descriptor, so the replacement can fail with `ENOSYS`;
  the gnulib module ‘`utimens`’ provides a more reliable interface
  `fdutimens`.
* The mere act of using `stat` modifies the access time of
  directories on some platforms, so `utimensat` can only
  effectively change directory modification time:
  Cygwin 1.5.x.

---

Next: [`mkdir`](#mkdir), Previous: [`futimens`](#futimens), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.7 `lstat` [¶](#lstat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lstat.html>

Gnulib module: lstat

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, `lstat` may not
  correctly report the size of files or block devices 2 GiB and larger.
  See [Large File Support](#Large-File-Support).
* On Linux/x86 and Linux/x86\_64, applications compiled in 32-bit mode cannot
  access files that happen to have a 64-bit inode number. This can occur with
  file systems such as XFS (typically on large disks) and NFS.
  See [Large File Support](#Large-File-Support).
* For symlinks, when the argument ends in a slash, some platforms don’t
  dereference the argument:
  Solaris 9.
* On some platforms, `lstat("file/",buf)` succeeds instead of
  failing with `ENOTDIR`.
  macOS 14, Solaris 9.
* On macOS 12.6, when this function yields a timestamp with a
  nonpositive `tv_sec` value, `tv_nsec` might be in the range
  −999999999..−1, representing a negative nanoseconds
  offset from `tv_sec`. Solaris 11.4 is similar, except that
  `tv_sec` might also be −1000000000.
* On Windows platforms (excluding Cygwin), a different name has to be used
  for the null device, namely `"NUL"` instead of `"/dev/null"`.
* On Windows platforms (excluding Cygwin), symlinks are not supported, so
  `lstat` does not exist.

Portability problems not fixed by Gnulib:

* See [`sys/stat.h`](#sys_002fstat_002eh), for general portability problems with `struct stat`.

---

Next: [`mkdirat`](#mkdirat), Previous: [`lstat`](#lstat), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.8 `mkdir` [¶](#mkdir-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mkdir.html>

Gnulib module: sys\_stat-h or mkdir

Portability problems fixed by either Gnulib module `sys_stat-h` or `mkdir`:

* This function is declared in different header files (namely, `<io.h>` or
  `<direct.h>`) on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), this function is called `_mkdir`
  and takes only one argument. The fix (without Gnulib) is to define a macro
  like this:

  ```
  #define mkdir ((int (*)()) _mkdir)
  ```

  or

  ```
  #define mkdir(path,mode) _mkdir (path)
  ```

Portability problems fixed by Gnulib module `mkdir`:

* When the argument ends in a slash, the function call fails on some platforms.
* This function mistakenly succeeds on ‘`mkdir("d/./",mode)`’ on
  some platforms:
  Cygwin 1.5.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`mkfifo`](#mkfifo), Previous: [`mkdir`](#mkdir), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.9 `mkdirat` [¶](#mkdirat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mkdirat.html>

Gnulib module: mkdirat

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8,
  AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14.
  But the replacement function is not safe to be used in libraries and
  is not thread-safe.

Portability problems not fixed by Gnulib:

---

Next: [`mkfifoat`](#mkfifoat), Previous: [`mkdirat`](#mkdirat), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.10 `mkfifo` [¶](#mkfifo-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mkfifo.html>

Gnulib module: mkfifo

Portability problems fixed by Gnulib:

* This function mishandles trailing slash on some platforms:
  FreeBSD 7.2, Solaris 9.
* This function is missing on some platforms; however, the replacement
  always fails with `ENOSYS`:
  mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

* If the argument exists and is a directory, this function fails with
  `EISDIR` instead of the correct `EEXIST`:
  HP-UX 11.11.

---

Next: [`mknod`](#mknod), Previous: [`mkfifo`](#mkfifo), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.11 `mkfifoat` [¶](#mkfifoat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mkfifoat.html>

Gnulib module: mkfifoat

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 12, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 5.1.
  But the replacement function is not safe to be used in libraries and
  is not thread-safe.
* This function does not fail when the file name argument ends in a slash
  and (without the slash) names a nonexistent file, on some platforms:
  AIX 7.2.
* This function does not fail when the file name argument ends in a slash
  and (without the slash) names a symbolic link, on some platforms:
  macOS 14.

Portability problems not fixed by Gnulib:

* The gnulib replacement function always fails with `ENOSYS` on
  some platforms: mingw, MSVC 14.
* If the argument exists and is a directory, the gnulib replacement
  function fails with `EISDIR` instead of the correct
  `EEXIST`: HP-UX 11.11.

---

Next: [`mknodat`](#mknodat), Previous: [`mkfifoat`](#mkfifoat), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.12 `mknod` [¶](#mknod-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mknod.html>

Gnulib module: mknod

Portability problems fixed by Gnulib:

* This function requires super-user privileges to create a fifo:
  FreeBSD 7.2, NetBSD 8.0, OpenBSD 7.9.
* This function mishandles trailing slash on some platforms:
  FreeBSD 7.2, Solaris 9.
* This function does not handle the `S_IFIFO` flag on some platforms:
  macOS 14, DragonFly 6.0,
  Haiku.
* This function is missing on some platforms; however, the replacement
  always fails with `ENOSYS`:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* Use of this function for anything except fifos is not portable,
  generally requiring super-user privileges and knowledge of supported
  device numbers.
* If the argument exists and is a directory, this function fails with
  `EISDIR` instead of the correct `EEXIST`:
  HP-UX 11.11.

---

Next: [`stat`](#stat), Previous: [`mknod`](#mknod), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.13 `mknodat` [¶](#mknodat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mknodat.html>

Gnulib module: mkfifoat

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 12, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 4.4.
  But the replacement function is not safe to be used in libraries and
  is not thread-safe.
* This function does not fail when the file name argument ends in a slash
  and (without the slash) names a nonexistent file, on some platforms:
  AIX 7.2.
* This function does not handle the `S_IFIFO` flag on some platforms:
  Haiku.

Portability problems not fixed by Gnulib:

* The gnulib replacement function always fails with `ENOSYS` on
  some platforms:
  mingw, MSVC 14.
* If the argument exists and is a directory, the gnulib replacement
  function fails with `EISDIR` instead of the correct
  `EEXIST`: HP-UX 11.11.

---

Next: [`umask`](#umask), Previous: [`mknodat`](#mknodat), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.14 `stat` [¶](#stat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/stat.html>

Gnulib module: stat

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, `stat` may not correctly
  report the size of files or block devices 2 GiB and larger.
  See [Large File Support](#Large-File-Support).
* On Linux/x86 and Linux/x86\_64, applications compiled in 32-bit mode cannot
  access files that happen to have a 64-bit inode number. This can occur with
  file systems such as XFS (typically on large disks) and NFS.
  See [Large File Support](#Large-File-Support).
* The `st_atime`, `st_ctime`, `st_mtime` fields are affected by
  the current time zone and by the DST flag of the current time zone on some
  platforms:
  mingw, MSVC 14 (when the environment variable `TZ` is set).
* On MSVC 14, this function fails with error `ENOENT`
  on files such as ‘`C:\pagefile.sys`’ and
  on directories such as ‘`C:\System Volume Information`’.
* On some platforms, `stat("link-to-file/",buf)` succeeds instead
  of failing with `ENOTDIR`.
  macOS 14, FreeBSD 7.2, AIX 7.1, Solaris 9, mingw64.
* On some platforms, `stat(".",buf)` and `stat("./",buf)` give
  different results:
  mingw, MSVC 14.
* On mingw up to 14.0.0,
  the `st_mode` field of `stat("NUL",buf)` is bogus,
  suggesting that `NUL` is a regular file.
* On macOS 12.6, when this function yields a timestamp with a
  nonpositive `tv_sec` value, `tv_nsec` might be in the range
  −999999999..−1, representing a negative nanoseconds
  offset from `tv_sec`. Solaris 11.4 is similar, except that
  `tv_sec` might also be −1000000000.
* On Windows platforms (excluding Cygwin), a different name has to be used
  for the null device, namely `"NUL"` instead of `"/dev/null"`.

Portability problems not fixed by Gnulib:

* See [`sys/stat.h`](#sys_002fstat_002eh), for general portability problems with `struct stat`.
* Cygwin’s `stat` function sometimes sets `errno` to `EACCES` when
  `ENOENT` would be more appropriate.
* This function sets `st_ino` only to the low-order 32 bits of
  the inode number of a socket or pipe, which thus can disagree
  with the `st_ino` obtained by `fstat`:
  macOS 14.
* Because of the definition of `struct stat`, it is not possible to
  portably replace `stat` via an object-like macro. Therefore,
  expressions such as `(islnk ? lstat : stat) (name, buf)` are not
  portable, and should instead be written `islnk ? lstat (name,
  buf) : stat (name, buf)`.

---

Next: [`utimensat`](#utimensat), Previous: [`stat`](#stat), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.15 `umask` [¶](#umask-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/umask.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Previous: [`umask`](#umask), Up: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.57.16 `utimensat` [¶](#utimensat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/utimensat.html>

Gnulib module: utimensat

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.5, Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 3.0.
  However, the replacement function may end up truncating timestamps to
  worse resolution than supported by the file system. Furthermore, the
  replacement function is not safe to be used in libraries and is not
  thread-safe.
* This function returns a bogus value instead of failing with
  `ENOSYS` on some platforms:
  Linux kernel 2.6.21.
* This function fails with `ENOSYS` if passed the flag
  `AT_SYMLINK_NOFOLLOW` on a regular file:
  Linux kernel 2.6.22.
* When using `UTIME_OMIT` or `UTIME_NOW`, some systems require
  the `tv_sec` argument to be 0, and don’t necessarily handle all
  file permissions in the manner required by POSIX:
  Linux kernel 2.6.25.
* When using `UTIME_OMIT` for the modification time, but specifying
  an access time, some systems fail to update the change time:
  Linux kernel 2.6.32, macOS 14, NetBSD 10.0, Solaris 11.1.
* Out-of-range values of `tv_nsec` do not lead to a failure on some
  platforms:
  Linux kernel 2.6.22.19 on hppa, NetBSD 10.0,
  GNU/Hurd with glibc 2.41.
* On some platforms, this function mis-handles a trailing slash:
  AIX 7.2.

Portability problems not fixed by Gnulib:

* On file systems mounted with the `noatime` attribute,
  this function might not modify the access time as requested:
  Linux kernel 6.9.
* When `AT_EMPTY_PATH` is used,
  this function does not allow the file name to be a null pointer:
  Linux kernel 6.10, Cygwin 3.6, FreeBSD 15.1.
* This function ignores the flag `AT_SYMLINK_NOFOLLOW`:
  Linux kernel 6.8 on a CIFS file system.
* This function sets a slightly different time stamp than the requested one:
  Haiku.
* On some platforms, timestamps of symbolic links cannot be modified, so
  the replacement fails with `ENOSYS` if passed the flag
  `AT_SYMLINK_NOFOLLOW` on a symlink.
* The mere act of using `lstat` modifies the access time of
  symlinks on some platforms, so `utimensat` with
  `AT_SYMLINK_NOFOLLOW` can only effectively change modification time:
  Cygwin.
* The mere act of using `stat` modifies the access time of
  directories on some platforms, so `utimensat` can only
  effectively change directory modification time:
  Cygwin 1.5.x.

The gnulib module `fdutimensat` provides a similar interface.

---

Next: [Functions in `<sys/time.h>`](#Functions-in-_003csys_002ftime_002eh_003e), Previous: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.58 Functions in `<sys/statvfs.h>` [¶](#Functions-in-_003csys_002fstatvfs_002eh_003e-1)

* [`fstatvfs`](#fstatvfs)
* [`statvfs`](#statvfs)

---

Next: [`statvfs`](#statvfs), Up: [Functions in `<sys/statvfs.h>`](#Functions-in-_003csys_002fstatvfs_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.58.1 `fstatvfs` [¶](#fstatvfs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/fstatvfs.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `f_blocks` in ‘`struct statvfs`’ is a 32-bit
  value, this function may not work correctly on files systems larger than
  4 TiB. See [Large File Support](#Large-File-Support). This affects
  glibc/Hurd, HP-UX 11, Solaris.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, mingw, MSVC 14, Android 4.3.
* This function fails if the file descriptor was opened with `O_PATH`:
  Linux kernel 3.11.

---

Previous: [`fstatvfs`](#fstatvfs), Up: [Functions in `<sys/statvfs.h>`](#Functions-in-_003csys_002fstatvfs_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.58.2 `statvfs` [¶](#statvfs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/statvfs.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `f_blocks` in ‘`struct statvfs`’ is a 32-bit
  value, this function may not work correctly on files systems larger than
  4 TiB. See [Large File Support](#Large-File-Support). This affects
  glibc/Hurd, HP-UX 11, Solaris.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, mingw, MSVC 14, Android 4.3.
* This function can hang if it stats all preceding
  entries in /proc/mounts, and any of those file systems
  are hard-mounted and not available. This affects
  Linux < 2.6.36.

Gnulib provides a module `fsusage` that provides similar information
as `statvfs`.

---

Next: [Functions in `<sys/times.h>`](#Functions-in-_003csys_002ftimes_002eh_003e), Previous: [Functions in `<sys/statvfs.h>`](#Functions-in-_003csys_002fstatvfs_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.59 Functions in `<sys/time.h>` [¶](#Functions-in-_003csys_002ftime_002eh_003e-1)

* [`utimes`](#utimes)

---

Up: [Functions in `<sys/time.h>`](#Functions-in-_003csys_002ftime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.59.1 `utimes` [¶](#utimes-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/utimes.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* On some platforms, this function mis-handles a trailing slash:
  FreeBSD 7.2, Solaris 9.
* This function cannot set full timestamp resolution. In particular,
  some platforms incorrectly round rather than truncate.
  Use `utimensat(AT_FDCWD,file,times,0)`,
  or the gnulib module `utimens`,
  instead.
* On file systems mounted with the `noatime` attribute,
  this function might not modify the access time as requested:
  Linux kernel 6.9.
* On some platforms, `utimes (file, NULL)` fails to set the
  file’s timestamp to the current time:
  glibc 2.3.3.
* On some platforms, `utimes` failed on read-only files when
  `utime` worked fine.
  glibc 2.2.5.
* This function sets a slightly different time stamp than the requested one:
  Haiku.
* On OS/2, this function cannot set the timestamp to earlier than the
  year 1980 in local time.
* On OS/2, this function cannot set the timestamp to an odd number of
  seconds.
* On OS/2, this function does not work on an opened file.

Extension: Gnulib provides a module ‘`utimens`’ that works around these
problems and can set the time with nanosecond resolution (as far as
supported by the file system).

---

Next: [Functions in `<sys/uio.h>`](#Functions-in-_003csys_002fuio_002eh_003e), Previous: [Functions in `<sys/time.h>`](#Functions-in-_003csys_002ftime_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.60 Functions in `<sys/times.h>` [¶](#Functions-in-_003csys_002ftimes_002eh_003e-1)

* [`times`](#times)

---

Up: [Functions in `<sys/times.h>`](#Functions-in-_003csys_002ftimes_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.60.1 `times` [¶](#times-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/times.html>

Gnulib module: times

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* There is no function on Windows to measure consumed process child
  times, thus the `tms_cutime` and `tms_cstime` will always be
  0 when the module is used.

---

Next: [Functions in `<sys/utsname.h>`](#Functions-in-_003csys_002futsname_002eh_003e), Previous: [Functions in `<sys/times.h>`](#Functions-in-_003csys_002ftimes_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.61 Functions in `<sys/uio.h>` [¶](#Functions-in-_003csys_002fuio_002eh_003e-1)

* [`readv`](#readv)
* [`writev`](#writev)

---

Next: [`writev`](#writev), Up: [Functions in `<sys/uio.h>`](#Functions-in-_003csys_002fuio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.61.1 `readv` [¶](#readv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/readv.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Previous: [`readv`](#readv), Up: [Functions in `<sys/uio.h>`](#Functions-in-_003csys_002fuio_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.61.2 `writev` [¶](#writev-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/writev.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [Functions in `<sys/wait.h>`](#Functions-in-_003csys_002fwait_002eh_003e), Previous: [Functions in `<sys/uio.h>`](#Functions-in-_003csys_002fuio_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.62 Functions in `<sys/utsname.h>` [¶](#Functions-in-_003csys_002futsname_002eh_003e-1)

* [`uname`](#uname)

---

Up: [Functions in `<sys/utsname.h>`](#Functions-in-_003csys_002futsname_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.62.1 `uname` [¶](#uname-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/uname.html>

Gnulib module: uname

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [Functions in `<syslog.h>`](#Functions-in-_003csyslog_002eh_003e), Previous: [Functions in `<sys/utsname.h>`](#Functions-in-_003csys_002futsname_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.63 Functions in `<sys/wait.h>` [¶](#Functions-in-_003csys_002fwait_002eh_003e-1)

* [`wait`](#wait)
* [`waitid`](#waitid)
* [`waitpid`](#waitpid)

---

Next: [`waitid`](#waitid), Up: [Functions in `<sys/wait.h>`](#Functions-in-_003csys_002fwait_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.63.1 `wait` [¶](#wait-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/wait.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`waitpid`](#waitpid), Previous: [`wait`](#wait), Up: [Functions in `<sys/wait.h>`](#Functions-in-_003csys_002fwait_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.63.2 `waitid` [¶](#waitid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/waitid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 7.1, OpenBSD 7.2, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14.
* As of 2005, no system is known on which `waitid` with flag `WNOWAIT`
  works correctly.

---

Previous: [`waitid`](#waitid), Up: [Functions in `<sys/wait.h>`](#Functions-in-_003csys_002fwait_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.63.3 `waitpid` [¶](#waitpid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/waitpid.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-waitpid-3.html>

Gnulib module: waitpid

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e), Previous: [Functions in `<sys/wait.h>`](#Functions-in-_003csys_002fwait_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.64 Functions in `<syslog.h>` [¶](#Functions-in-_003csyslog_002eh_003e-1)

* [`closelog`](#closelog)
* [`openlog`](#openlog)
* [`setlogmask`](#setlogmask)
* [`syslog`](#syslog)

---

Next: [`openlog`](#openlog), Up: [Functions in `<syslog.h>`](#Functions-in-_003csyslog_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.64.1 `closelog` [¶](#closelog-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/closelog.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`setlogmask`](#setlogmask), Previous: [`closelog`](#closelog), Up: [Functions in `<syslog.h>`](#Functions-in-_003csyslog_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.64.2 `openlog` [¶](#openlog-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/openlog.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`syslog`](#syslog), Previous: [`openlog`](#openlog), Up: [Functions in `<syslog.h>`](#Functions-in-_003csyslog_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.64.3 `setlogmask` [¶](#setlogmask-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlogmask.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Previous: [`setlogmask`](#setlogmask), Up: [Functions in `<syslog.h>`](#Functions-in-_003csyslog_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.64.4 `syslog` [¶](#syslog-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/syslog.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e), Previous: [Functions in `<syslog.h>`](#Functions-in-_003csyslog_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.65 Functions in `<termios.h>` [¶](#Functions-in-_003ctermios_002eh_003e-1)

* [`cfgetispeed`](#cfgetispeed)
* [`cfgetospeed`](#cfgetospeed)
* [`cfsetispeed`](#cfsetispeed)
* [`cfsetospeed`](#cfsetospeed)
* [`tcdrain`](#tcdrain)
* [`tcflow`](#tcflow)
* [`tcflush`](#tcflush)
* [`tcgetattr`](#tcgetattr)
* [`tcgetsid`](#tcgetsid)
* [`tcgetwinsize`](#tcgetwinsize)
* [`tcsendbreak`](#tcsendbreak)
* [`tcsetattr`](#tcsetattr)
* [`tcsetwinsize`](#tcsetwinsize)

---

Next: [`cfgetospeed`](#cfgetospeed), Up: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.65.1 `cfgetispeed` [¶](#cfgetispeed-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cfgetispeed.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.

---

Next: [`cfsetispeed`](#cfsetispeed), Previous: [`cfgetispeed`](#cfgetispeed), Up: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.65.2 `cfgetospeed` [¶](#cfgetospeed-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cfgetospeed.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.

---

Next: [`cfsetospeed`](#cfsetospeed), Previous: [`cfgetospeed`](#cfgetospeed), Up: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.65.3 `cfsetispeed` [¶](#cfsetispeed-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cfsetispeed.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.

---

Next: [`tcdrain`](#tcdrain), Previous: [`cfsetispeed`](#cfsetispeed), Up: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.65.4 `cfsetospeed` [¶](#cfsetospeed-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cfsetospeed.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.

---

Next: [`tcflow`](#tcflow), Previous: [`cfsetospeed`](#cfsetospeed), Up: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.65.5 `tcdrain` [¶](#tcdrain-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tcdrain.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.
* On some platforms, `tcdrain` on a non-tty fails with `errno` set to
  `EINVAL` or, on Mac OS X, also `EOPNOTSUPP` or `ENODEV`, rather
  than `ENOTTY`.

---

Next: [`tcflush`](#tcflush), Previous: [`tcdrain`](#tcdrain), Up: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.65.6 `tcflow` [¶](#tcflow-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tcflow.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.

---

Next: [`tcgetattr`](#tcgetattr), Previous: [`tcflow`](#tcflow), Up: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.65.7 `tcflush` [¶](#tcflush-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tcflush.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.
* On some platforms, `tcflush` of `TCIFLUSH` on a non-tty fails with
  errno set to `EINVAL` rather than `ENOTTY`.
* On some platforms, `tcflush` of `TCOFLUSH` on a non-tty fails with
  errno set to `EINVAL` or, on Mac OS X,
  also `EOPNOTSUPP` or `ENODEV`, rather than `ENOTTY`.

---

Next: [`tcgetsid`](#tcgetsid), Previous: [`tcflush`](#tcflush), Up: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.65.8 `tcgetattr` [¶](#tcgetattr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tcgetattr.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.

---

Next: [`tcgetwinsize`](#tcgetwinsize), Previous: [`tcgetattr`](#tcgetattr), Up: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.65.9 `tcgetsid` [¶](#tcgetsid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tcgetsid.html>

Gnulib module: tcgetsid

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, OpenBSD 4.5, Minix 3.1.8, Cygwin 1.7.9, mingw, MSVC 14, Android 4.4.
* The declaration of this function in C++ compilation units has C++ linkage, not
  C linkage, on some platforms:
  HP-UX 11.00.

Portability problems not fixed by Gnulib:

* This function always fails on some platforms:
  FreeBSD 6.0, Cygwin 1.7.8, mingw, MSVC 14.
* This function returns int instead of pid\_t on some platforms:
  Cygwin 1.7.11.

---

Next: [`tcsendbreak`](#tcsendbreak), Previous: [`tcgetsid`](#tcgetsid), Up: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.65.10 `tcgetwinsize` [¶](#tcgetwinsize-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tcgetwinsize.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 12.0, NetBSD 8.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

---

Next: [`tcsetattr`](#tcsetattr), Previous: [`tcgetwinsize`](#tcgetwinsize), Up: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.65.11 `tcsendbreak` [¶](#tcsendbreak-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tcsendbreak.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.

---

Next: [`tcsetwinsize`](#tcsetwinsize), Previous: [`tcsendbreak`](#tcsendbreak), Up: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.65.12 `tcsetattr` [¶](#tcsetattr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tcsetattr.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.

---

Previous: [`tcsetattr`](#tcsetattr), Up: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.65.13 `tcsetwinsize` [¶](#tcsetwinsize-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tcsetwinsize.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 12.0, NetBSD 8.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

---

Next: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e), Previous: [Functions in `<termios.h>`](#Functions-in-_003ctermios_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
