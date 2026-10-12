# Add all stable-* branches locally:
for b in $(git branch -r | grep origin/stable- | sort --version-sort); do git checkout ${b#origin/}; done
git checkout -B stable-202501 $S1REV
git checkout -B stable-202507 $S2REV
git remote remove origin # drop some unrelated branches
git gc --prune=now # drop any unrelated commits, not clear this helps
git -c pack.threads=1 repack -adF
git -c 'pack.threads=1' bundle create gnulib.bundle --all
V=$(env TZ=UTC0 git show -s --date=format:%Y%m%d --pretty=%cd master)
mv gnulib.bundle gnulib-$V.bundle
build-aux/gnupload --to ftp.gnu.org:gnulib gnulib-$V.bundle
```

---

Next: [ISO C Keyword Substitutes](#Keyword-Substitutes), Previous: [Miscellaneous Notes](#Miscellaneous-Notes), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 7 Building the ISO C and POSIX Substitutes [¶](#Building-the-ISO-C-and-POSIX-Substitutes)

This section shows a radically different way to use Gnulib.

You can extract the ISO C / POSIX substitutes part of gnulib by running
the command

```
gnulib-tool --create-testdir --source-base=lib \
            --dir=/tmp/posixlib `posix-modules`
```

The command ‘`posix-modules`’ is found in the same directory as
`gnulib-tool`.

The resulting directory can be built on a particular platform,
independently of the program being ported. Then you can configure and
build any program, by setting `CPPFLAGS` and `LDFLAGS` at
configure time accordingly: set `CPPFLAGS="-I.../posixlib/lib"`, plus
any essential type definitions and flags that you find in
`.../posixlib/config.h`, and set
`LDFLAGS=".../posixlib/lib/libgnu.a"`.

This way of using Gnulib is useful when you don’t want to modify the program’s
source code, or when the program uses a mix between C and C++ sources
(requiring separate builds of the `posixlib` for the C compiler and
for the C++ compiler).

---

Next: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes), Previous: [Building the ISO C and POSIX Substitutes](#POSIX-Substitutes-Library), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 8 ISO C Keyword Substitutes [¶](#ISO-C-Keyword-Substitutes)

This chapter describes which keywords specified by ISO C are
substituted by Gnulib.

* [`alignof` and `alignas`](#alignof)
* [`bool`](#bool)
* [`nullptr`](#nullptr)
* [`static_assert`](#static_005fassert)

---

Next: [`bool`](#bool), Up: [ISO C Keyword Substitutes](#Keyword-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 8.1 `alignof` and `alignas` [¶](#alignof-and-alignas)

Gnulib module: alignasof

The `alignasof` module arranges for `alignas` and `alignof`
to be more like standard C.

Portability problems fixed by Gnulib:

* On pre-C23 platforms, `<stdalign.h>` must be included before
  using `alignas` or `alignof`.
  See [`stdalign.h`](#stdalign_002eh).

Portability problems not fixed by Gnulib:

* The set of alignments supported by `alignas` is implementation
  dependent, and implementations need not support alignments greater
  than `alignof (max_align_t)`.
* Some older platforms do not support `alignas`.
  To test this, use the `alignasof` module
  along with the `#if` conditional
  `(defined alignas || defined __alignas_is_defined
  || 202311 <= __STDC_VERSION__ || 201103 <= __cplusplus)`.
  Platforms where `alignas` does not work include:
  IBM XL C for AIX V16.1.
* On some platforms, the argument of `alignas` must be an integer
  constant like ‘`8`’, instead of an integer constant expression
  like ‘`4 + 4`’ that evaluates to 8:
  GCC 4.9.
* On pre-C23 platforms, `alignas` and `alignof` are macros.

---

Next: [`nullptr`](#nullptr), Previous: [`alignof` and `alignas`](#alignof), Up: [ISO C Keyword Substitutes](#Keyword-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 8.2 `bool` [¶](#bool-1)

Gnulib module: bool

Portability problems fixed by Gnulib:

* The keywords `bool`, `true`, and `false` are not available:
  gcc 12 and other compilers predating C23.

Portability problems not fixed by Gnulib:

* On pre-C23 platforms, the keyword substitutes are macros.
* On pre-C23 platforms, the keyword substitutes assume C99 or later.

---

Next: [`static_assert`](#static_005fassert), Previous: [`bool`](#bool), Up: [ISO C Keyword Substitutes](#Keyword-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 8.3 `nullptr` [¶](#nullptr-1)

Gnulib module: nullptr

The `nullptr` module arranges for `nullptr` to act
like standard C and C++.

The `nullptr` keyword yields a null pointer. It differs from
the `NULL` macro, in that `NULL` might be an integer whereas
`nullptr` is of a special `nullptr_t` type with only one
value, namely `nullptr` itself. Using `nullptr` can help
some compilers emit more sensible warnings, can avoid the need to cast
a null pointer passed to a function prototyped with an ellipsis, and
removes the need to include `<stddef.h>` merely to define
`NULL`.

Portability problems fixed by Gnulib:

* Some platforms lack `nullptr`:
  For C: GCC 12, Clang 15, and other pre-2023 C compilers.
  For C++: pre-2011 C++ compilers.
* Some platforms incorrectly warn when `nullptr` is a sentinel argument:
  GCC 13.2 and 14.0.

Portability problems not fixed by Gnulib:

* On older platforms, `nullptr` is a macro instead of a keyword.
* On older platforms, `nullptr` does not have the type `nullptr_t`.
  In C, it has type `void *`; in C++ it has an integer type.
* On older platforms Gnulib cannot easily emulate `nullptr_t`, so
  null pointer type checking is more error prone. In C, `_Generic`
  expressions cannot reliably distinguish the type of `nullptr`
  from integer or `void *` types. C++ overloading has similar
  limitations.
* GCC 14 defines `nullptr_t` even when `<stddef.h>` is not
  included. This bug is fixed in GCC 15.

---

Previous: [`nullptr`](#nullptr), Up: [ISO C Keyword Substitutes](#Keyword-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 8.4 `static_assert` [¶](#static_005fassert-1)

Gnulib module: assert-h

The `assert-h` module arranges for both `static_assert` and
`<assert.h>` to be like standard C. See [`assert.h`](#assert_002eh).

Portability problems fixed by Gnulib:

* Pre-C11 platforms lack `static_assert`.
* On pre-C23 platforms, `<assert.h>` must be included before
  using `static_assert`.

Portability problems not fixed by Gnulib:

* On pre-C23 platforms, `static_assert` is a macro.

---

Next: [ISO C and POSIX Function Substitutes](#Function-Substitutes), Previous: [ISO C Keyword Substitutes](#Keyword-Substitutes), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 9 ISO C and POSIX Header File Substitutes [¶](#ISO-C-and-POSIX-Header-File-Substitutes)

This chapter describes which header files specified by ISO C or POSIX are
substituted by Gnulib, which portability pitfalls are fixed by Gnulib, and
which (known) portability problems are not worked around by Gnulib.

The notation “Gnulib module: —” means that Gnulib does not provide a
module providing a substitute for the header file. When the list
“Portability problems not fixed by Gnulib” is empty, such a module is
not needed: No portability problems are known. Otherwise, it indicates
that such a module would be useful but is not available: No one so far
found this header file important enough to contribute a substitute for it.
If you need this particular header file, you may write to
`<bug-gnulib at gnu dot org>`.

* [`aio.h`](#aio_002eh)
* [`arpa/inet.h`](#arpa_002finet_002eh)
* [`assert.h`](#assert_002eh)
* [`complex.h`](#complex_002eh)
* [`cpio.h`](#cpio_002eh)
* [`ctype.h`](#ctype_002eh)
* [`devctl.h`](#devctl_002eh)
* [`dirent.h`](#dirent_002eh)
* [`dlfcn.h`](#dlfcn_002eh)
* [`endian.h`](#endian_002eh)
* [`errno.h`](#errno_002eh)
* [`fcntl.h`](#fcntl_002eh)
* [`fenv.h`](#fenv_002eh)
* [`float.h`](#float_002eh)
* [`fmtmsg.h`](#fmtmsg_002eh)
* [`fnmatch.h`](#fnmatch_002eh)
* [`ftw.h`](#ftw_002eh)
* [`glob.h`](#glob_002eh)
* [`grp.h`](#grp_002eh)
* [`iconv.h`](#iconv_002eh)
* [`inttypes.h`](#inttypes_002eh)
* [`iso646.h`](#iso646_002eh)
* [`langinfo.h`](#langinfo_002eh)
* [`libgen.h`](#libgen_002eh)
* [`libintl.h`](#libintl_002eh)
* [`limits.h`](#limits_002eh)
* [`locale.h`](#locale_002eh)
* [`math.h`](#math_002eh)
* [`monetary.h`](#monetary_002eh)
* [`mqueue.h`](#mqueue_002eh)
* [`ndbm.h`](#ndbm_002eh)
* [`net/if.h`](#net_002fif_002eh)
* [`netdb.h`](#netdb_002eh)
* [`netinet/in.h`](#netinet_002fin_002eh)
* [`netinet/tcp.h`](#netinet_002ftcp_002eh)
* [`nl_types.h`](#nl_005ftypes_002eh)
* [`poll.h`](#poll_002eh)
* [`pthread.h`](#pthread_002eh)
* [`pwd.h`](#pwd_002eh)
* [`regex.h`](#regex_002eh)
* [`sched.h`](#sched_002eh)
* [`search.h`](#search_002eh)
* [`semaphore.h`](#semaphore_002eh)
* [`setjmp.h`](#setjmp_002eh)
* [`signal.h`](#signal_002eh)
* [`spawn.h`](#spawn_002eh)
* [`stdalign.h`](#stdalign_002eh)
* [`stdarg.h`](#stdarg_002eh)
* [`stdatomic.h`](#stdatomic_002eh)
* [`stdbit.h`](#stdbit_002eh)
* [`stdbool.h`](#stdbool_002eh)
* [`stdckdint.h`](#stdckdint_002eh)
* [`stdcountof.h`](#stdcountof_002eh)
* [`stddef.h`](#stddef_002eh)
* [`stdint.h`](#stdint_002eh)
* [`stdio.h`](#stdio_002eh)
* [`stdlib.h`](#stdlib_002eh)
* [`stdnoreturn.h`](#stdnoreturn_002eh)
* [`string.h`](#string_002eh)
* [`strings.h`](#strings_002eh)
* [`sys/ipc.h`](#sys_002fipc_002eh)
* [`sys/mman.h`](#sys_002fmman_002eh)
* [`sys/msg.h`](#sys_002fmsg_002eh)
* [`sys/resource.h`](#sys_002fresource_002eh)
* [`sys/select.h`](#sys_002fselect_002eh)
* [`sys/sem.h`](#sys_002fsem_002eh)
* [`sys/shm.h`](#sys_002fshm_002eh)
* [`sys/socket.h`](#sys_002fsocket_002eh)
* [`sys/stat.h`](#sys_002fstat_002eh)
* [`sys/statvfs.h`](#sys_002fstatvfs_002eh)
* [`sys/time.h`](#sys_002ftime_002eh)
* [`sys/times.h`](#sys_002ftimes_002eh)
* [`sys/types.h`](#sys_002ftypes_002eh)
* [`sys/uio.h`](#sys_002fuio_002eh)
* [`sys/un.h`](#sys_002fun_002eh)
* [`sys/utsname.h`](#sys_002futsname_002eh)
* [`sys/wait.h`](#sys_002fwait_002eh)
* [`syslog.h`](#syslog_002eh)
* [`tar.h`](#tar_002eh)
* [`termios.h`](#termios_002eh)
* [`tgmath.h`](#tgmath_002eh)
* [`threads.h`](#threads_002eh)
* [`time.h`](#time_002eh)
* [`uchar.h`](#uchar_002eh)
* [`unistd.h`](#unistd_002eh)
* [`utmpx.h`](#utmpx_002eh)
* [`wchar.h`](#wchar_002eh)
* [`wctype.h`](#wctype_002eh)
* [`wordexp.h`](#wordexp_002eh)

---

Next: [`arpa/inet.h`](#arpa_002finet_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.1 `aio.h` [¶](#aio_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/aio.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.10.x, mingw, MSVC 14, Haiku, Android 9.0.
* The type `off64_t` is not defined on some platforms:
  macOS 14, FreeBSD 10.4, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.

---

Next: [`assert.h`](#assert_002eh), Previous: [`aio.h`](#aio_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.2 `arpa/inet.h` [¶](#arpa_002finet_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/arpa_inet.h.html>

Gnulib module: arpa\_inet-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms: mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the headers `<inttypes.h>` and `<netinet/in.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`complex.h`](#complex_002eh), Previous: [`arpa/inet.h`](#arpa_002finet_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.3 `assert.h` [¶](#assert_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/assert.h.html>

Gnulib module: assert-h

See also the Gnulib modules `assert` and `verify`.

Portability problems fixed by Gnulib:

* On older C platforms `<assert.h>` must be included before using
  `static_assert`. For example, GCC versions before 13 do not
  support the `static_assert` keyword that was standardized by C23.
* On older platforms `static_assert` does not allow the second
  string-literal argument to be omitted. For example, GCC versions
  before 9.1 do not support the single-argument `static_assert`
  that was standardized by C23 and C++17.
* Even-older platforms do not support `static_assert` at all.
  For example, GCC versions before 4.6 and G++ versions before 4.3
  do not support the two-argument form, which was standardized
  by C11 and C++11.
* Older C platforms might not support the obsolescent
  `_Static_assert` keyword or macro.
  This portability problem should not matter with code using this
  module, as such code should use `static_assert` instead.

Portability problems not fixed by Gnulib:

* A `static_assert` can also
  be used within a `struct` or `union` specifier, in place of
  an ordinary declaration of a member of the struct or union. The
  Gnulib substitute can be used only as an ordinary declaration
  in code intended to be portable to C99 or earlier.
* In C23 and C++11 and later, `static_assert` is a keyword.
  In C11 and C17 it is a macro. Any Gnulib substitute is also a macro.
* In C99 and later, the argument to `assert` can be of any scalar type.
  In C89, the argument is of type `int`.
* In C23 and C++26 and later, `assert` is a variadic macro that can
  be applied to any scalar expression even if it has an unparenthesized comma.
  In earlier standards, `assert` is a single-argument macro so its
  argument cannot contain an unparenthesized comma.

---

Next: [`cpio.h`](#cpio_002eh), Previous: [`assert.h`](#assert_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.4 `complex.h` [¶](#complex_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/complex.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 1.7.7, mingw, MSVC 9.

---

Next: [`ctype.h`](#ctype_002eh), Previous: [`complex.h`](#complex_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.5 `cpio.h` [¶](#cpio_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/cpio.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  Minix 3.1.8, Cygwin 2.4.x, mingw, MSVC 14.

---

Next: [`devctl.h`](#devctl_002eh), Previous: [`cpio.h`](#cpio_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.6 `ctype.h` [¶](#ctype_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html>

Gnulib module: ctype-h

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`dirent.h`](#dirent_002eh), Previous: [`ctype.h`](#ctype_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.7 `devctl.h` [¶](#devctl_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/devctl.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 9.3, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.6.0, mingw, MSVC 14, Android 9.0.

---

Next: [`dlfcn.h`](#dlfcn_002eh), Previous: [`devctl.h`](#devctl_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.8 `dirent.h` [¶](#dirent_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/dirent.h.html>

Gnulib module: dirent-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  MSVC 14.
* Although many systems define directory entry type macros like `DT_DIR`,
  some do not:
  Minix 3.1.8, AIX 7.2, HP-UX 11, Solaris 11.4, mingw.
* The directory entry type macro `DT_WHT` is missing on many systems:
  All systems where `DT_DIR` is missing, plus OpenBSD 7.9.
* The conversion macros `DTTOIF` and `IFTODT` are missing on
  many systems (however, the Gnulib replacement macros
  may evaluate their arguments multiple times):
  All systems where `DT_DIR` is missing, plus OpenBSD 7.9.
* The type `ino_t` is missing on some platforms:
  glibc 2.23 and others.

Portability problems not fixed by Gnulib:

* Although many systems define a `struct dirent` member named
  `d_type`, some do not:
  Minix 3.1.8, AIX 7.2, HP-UX 11, Solaris 11.4, mingw.
* On systems with `d_type`, not every filesystem supports
  `d_type`, and those lacking support will set it to `DT_UNKNOWN`.
* The POSIX.1-2024 directory entry type macros `DT_MQ`,
  `DT_SEM`, `DT_SHM` and `DT_TMO` are missing on many
  platforms.
* Some systems define a `struct dirent` member named `d_namlen`
  containing the string length of `d_name`, but others do not:
  glibc 2.23 on Linux, Minix 3.1.8, Solaris 11.4, Cygwin.
  All of these, except Cygwin, have a member `d_reclen` instead,
  that has a different semantics.
* Some systems define a `struct dirent` member named `d_off`
  containing a magic cookie suitable as an argument to `seekdir`,
  but others do not:
  glibc 2.23 on Hurd, macOS 11.1, FreeBSD 11.0, NetBSD 10.0, OpenBSD 7.9, AIX 5.1, HP-UX 11, Cygwin, mingw.
* Some systems define a `struct dirent` member named
  `d_reclen` containing the number of bytes in the directory entry
  record, but others do not. This member has limited utility, as it is
  an implementation detail.

---

Next: [`endian.h`](#endian_002eh), Previous: [`dirent.h`](#dirent_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.9 `dlfcn.h` [¶](#dlfcn_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/dlfcn.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`errno.h`](#errno_002eh), Previous: [`dlfcn.h`](#dlfcn_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.10 `endian.h` [¶](#endian_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/endian.h.html>

Describes the platform’s endianness (byte ordering of words stored in memory).
Defines the macros `BYTE_ORDER`, `LITTLE_ENDIAN`, `BIG_ENDIAN`,
`PDP_ENDIAN`.

Gnulib module: endian

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  macOS 14, FreeBSD 13.0, NetBSD 7.1, OpenBSD 3.8, Minix 3.1.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the header `<stdint.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`fcntl.h`](#fcntl_002eh), Previous: [`endian.h`](#endian_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.11 `errno.h` [¶](#errno_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/errno.h.html>

Gnulib module: errno-h

Portability problems fixed by Gnulib:

* The macro `EOVERFLOW` is not defined
  on some platforms:
  OpenBSD 4.0, mingw, MSVC 9.
* The macro `ENOLINK` is not defined
  on some platforms:
  OpenBSD 7.9, mingw, MSVC 9.
* The macro `EMULTIHOP` is not defined
  on some platforms:
  OpenBSD 7.9, mingw, MSVC 14.
* The macro `ECANCELED` is not defined on some platforms:
  OpenBSD 4.0, Cygwin, mingw, MSVC 9.
* The macros `ENOMSG`, `EIDRM`, `EPROTO`, `EBADMSG`,
  `ENOTSUP` are not defined on some platforms:
  OpenBSD 4.0, mingw, MSVC 9.
* The macro `ESTALE` is not defined on some platforms:
  mingw, MSVC 14.
* The macro `EDQUOT` is not defined on some platforms:
  NonStop Kernel, mingw, MSVC 14.
* The macros `ENETRESET`, `ECONNABORTED` are not defined on some
  platforms:
  Minix 3.1.8, mingw, MSVC 9.
* The macros `EWOULDBLOCK`, `ETXTBSY`, `ELOOP`, `ENOTSOCK`,
  `EDESTADDRREQ`, `EMSGSIZE`, `EPROTOTYPE`, `ENOPROTOOPT`,
  `EPROTONOSUPPORT`, `EOPNOTSUPP`, `EAFNOSUPPORT`,
  `EADDRINUSE`, `EADDRNOTAVAIL`, `ENETDOWN`, `ENETUNREACH`,
  `ECONNRESET`, `ENOBUFS`, `EISCONN`, `ENOTCONN`,
  `ETIMEDOUT`, `ECONNREFUSED`, `EHOSTUNREACH`, `EALREADY`,
  `EINPROGRESS` are not defined on some platforms:
  mingw, MSVC 9.
* The macros `EOWNERDEAD`, `ENOTRECOVERABLE` are not defined on
  some platforms:
  glibc/Linux 2.3.6, glibc/Hurd 2.15, glibc/kFreeBSD 2.15,
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 9.3, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin, mingw without pthreads-win32, MSVC 9.
* The macro `ESOCKTNOSUPPORT` is not defined on some platforms:
  Haiku.
* The macro `EILSEQ` is not defined on some platforms:
  LynxOS 178 2.2.2.
* The macros `EEXIST` and `ENOTEMPTY` have the same value on
  some platforms:
  AIX 7.3.

Portability problems not fixed by Gnulib:

* Error numbers are in the range `INT_MIN` through `INT_MIN + 0xffff`,
  instead of being positive as POSIX requires:
  Haiku.

---

Next: [`fenv.h`](#fenv_002eh), Previous: [`errno.h`](#errno_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.12 `fcntl.h` [¶](#fcntl_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fcntl.h.html>

Gnulib module: fcntl-h

Portability problems fixed by Gnulib:

* The type `pid_t` is not defined on some platforms:
  MSVC 14.
* The type `mode_t` is not defined on some platforms:
  MSVC 14.
* The type `off64_t` is not defined on some platforms:
  macOS 14, FreeBSD 10.4, NetBSD 10.0, OpenBSD 7.9, MSVC 14, Cygwin, Haiku, Minix 3.3.
* ‘`O_CLOEXEC`’ is not defined on some platforms:
  Mac OS X 10.6, FreeBSD 8.4, NetBSD 5.1, OpenBSD 4.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 10, Cygwin 1.7.1, mingw, MSVC 14.
* ‘`O_DSYNC`’, ‘`O_NOCTTY`’,
  ‘`O_NOFOLLOW`’, ‘`O_RSYNC`’, ‘`O_SYNC`’,
  and ‘`O_TTY_INIT`’ are not defined on some platforms.
  When not otherwise defined, Gnulib defines these macros to 0,
  which is generally safe.
* ‘`O_NONBLOCK`’ is not defined on some platforms.
  If the ‘`nonblocking`’ module is in use, gnulib guarantees a working
  non-zero value; otherwise, the gnulib replacement is 0.
* ‘`O_EXEC`’ is not defined on some platforms:
  glibc 2.42, macOS 12, FreeBSD 7.4, NetBSD 9.4, OpenBSD 7.9, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android.
  However, glibc does define ‘`O_EXEC`’ on GNU/Hurd.
  When not otherwise defined,
  Gnulib defines this macro to ‘`O_RDONLY`’, which is typically 0.

  Note: ‘`O_EXEC`’ is not suitable for opening a file descriptor
  that you want to use with `fchmod` later, because
  musl libc defines ‘`O_EXEC`’ to `O_PATH` and
  Linux `fchmod` fails with error `EBADF` when given such a file
  descriptor.
* ‘`O_SEARCH`’ is not defined on some platforms:
  glibc 2.42, macOS 12, FreeBSD 12.4, NetBSD 6.1, OpenBSD 7.9, AIX 6.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android.
  When not otherwise defined,
  Gnulib defines this macro to ‘`O_RDONLY`’, which is typically 0.
* ‘`O_SEARCH`’ is defined to `O_PATH` on some platforms:
  musl libc.
* ‘`O_ACCMODE`’ is not defined on some platforms:
  MSVC 14.
* The ‘`O_ACCMODE`’ mask mistakenly omits ‘`O_SEARCH`’ and
  ‘`O_EXEC`’ on some platforms:
  Cygwin.
* ‘`O_BINARY`’, ‘`O_TEXT`’ (not specified by POSIX, but essential for
  portability to native Windows platforms) are defined on some platforms but
  not on others.
  Gnulib defines these macros to 0 on GNU and other platforms that do not
  distinguish between text and binary I/O.
* ‘`O_CLOEXEC`’, ‘`O_NOFOLLOW`’, and ‘`O_TTY_INIT`’
  are defined to values that are too large for an `int` on some platforms:
  AIX 7.1 with XL C 12.1.
* ‘`O_DIRECT`’, ‘`O_IGNORE_CTTY`’, ‘`O_NDELAY`’,
  ‘`O_NOATIME`’, ‘`O_NOLINK`’, ‘`O_NOLINKS`’, and
  ‘`O_NOTRANS`’ (not specified by POSIX) are defined
  on some platforms but not on others.
  When not otherwise defined, Gnulib defines these macros to 0,
  which is generally safe.
* ‘`O_DIRECTORY`’ is not defined on some platforms:
  glibc 2.0, Mac OS X 10.4, FreeBSD 7.4, NetBSD 4.0.1, OpenBSD 4.9, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14.
* ‘`FD_CLOEXEC`’, ‘`F_DUPFD`’, and ‘`F_GETFD`’ are not defined
  on some platforms:
  mingw, MSVC 14.
* ‘`F_DUPFD_CLOEXEC`’ is not defined on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, AIX 5.1, HP-UX 11, Solaris 11 2010-11, Cygwin 1.7.1, mingw, MSVC 14.
* ‘`AT_FDCWD`’, ‘`AT_EACCESS`’, ‘`AT_SYMLINK_NOFOLLOW`’,
  ‘`AT_SYMLINK_FOLLOW`’, and ‘`AT_REMOVEDIR`’
  are not defined on many platforms:
  glibc 2.3.6, Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, AIX 5.1, HP-UX 11, Cygwin 1.5.x, mingw, MSVC 14.
* ‘`AT_FDCWD`’ is defined with a value too large for an `int` on some
  platforms:
  Solaris 11.3.

Portability problems not fixed by Gnulib:

* On some platforms, ‘`O_NOATIME`’ and ‘`O_NOFOLLOW`’
  are defined but do not work.
  To help work around the resulting portability problems,
  the `fcntl-h` module defines ‘`HAVE_WORKING_O_NOATIME`’ to 1
  on platforms where ‘`O_NOATIME`’ is known to work, and to 0 otherwise;
  and similarly for ‘`HAVE_WORKING_O_NOFOLLOW`’ and ‘`O_NOFOLLOW`’:
  Mac OS X 10.3.
* ‘`O_PATH`’ and ‘`AT_EMPTY_PATH`’ are not defined on some platforms:
  glibc 2.13, macOS 26, FreeBSD 13.0, NetBSD 10.1, OpenBSD 7.9, Minix 3.3.0,
  AIX 7.3, HP-UX 11.31, Solaris 11.4, Cygwin 3.0, mingw, MSVC 14.
* Although POSIX.1-2024 standardized `O_CLOFORK`,
  `FD_CLOFORK` and `F_DUPFD_CLOFORK`,
  they are not defined on most platforms:
  glibc 2.42, musl libc, macOS 15.5, FreeBSD 14.2, NetBSD 10.1, OpenBSD 7.7, Minix 3.3.0, AIX 7.3, Solaris 11.3, Cygwin 3.6.2, mingw, MSVC 14.
* ‘`F_SETFD`’, ‘`F_GETFL`’, ‘`F_SETFL`’, ‘`F_GETLK`’,
  ‘`F_SETLK`’, ‘`F_SETLKW`’, ‘`F_GETOWN`’, and ‘`F_SETOWN`’
  are not defined on some platforms:
  mingw, MSVC 14.
* ‘`POSIX_FADV_DONTNEED`’, ‘`POSIX_FADV_NOREUSE`’,
  ‘`POSIX_FADV_NORMAL`’, ‘`POSIX_FADV_RANDOM`’,
  ‘`POSIX_FADV_SEQUENTIAL`’, and ‘`POSIX_FADV_WILLNEED`’ are not
  defined on some platforms.
* POSIX allows ‘`AT_FDCWD`’ to be −1, so it is unsafe to use
  ‘`AT_FDCWD`’ as an invalid directory file descriptor argument to
  functions like ‘`openat`’.
  Instead, you can use ‘`AT_FDCWD == -1 ? -2 : -1`’.
  ‘`AT_FDCWD`’ is −1 on some platforms:
  Haiku R1/Beta4.
* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the headers `<sys/stat.h>` and `<unistd.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`float.h`](#float_002eh), Previous: [`fcntl.h`](#fcntl_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.13 `fenv.h` [¶](#fenv_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fenv.h.html>

Gnulib module: fenv-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, AIX 5.1, Cygwin 1.7.7, MSVC 9.

Portability problems not fixed by Gnulib:

* On MSVC, the values of the rounding direction macros have changed:
  In MSVC 14, `FE_UPWARD` is 0x100 and `FE_DOWNWARD` is 0x200,
  whereas in MSVC 14.30, it’s the opposite.

---

Next: [`fmtmsg.h`](#fmtmsg_002eh), Previous: [`fenv.h`](#fenv_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.14 `float.h` [¶](#float_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/float.h.html>

Gnulib module: float-h

Portability problems fixed by Gnulib:

* The conversion from `int` to `long double` in incorrect on some
  platforms:
  glibc 2.7 on Linux/SPARC64.
* The values of `LDBL_*` macros are incorrect on some platforms:
  On OpenBSD 4.0 and MirBSD 10, they are the same as the values of the
  `DBL_*` macros, although ‘`long double`’ is a larger type than
  ‘`double`’.
  On FreeBSD/x86 6.4, they represent the incorrect 53-bit precision assumptions
  in the compiler, not the real 64-bit precision at runtime.
  On PowerPC with gcc 8.3 and the default `-mabi=ibmlongdouble`,
  they don’t reflect the “double double” representation of `long double`
  correctly; the non-IEEE representation is tricky, as it is a pair of
  IEEE `double` values such that adding the two elements and
  rounding to even yields the high element of the pair, and arithmetic
  overflow has undefined behavior instead of reliably yielding infinity.
* The macros
  `FLT_HAS_SUBNORM`, `FLT_DECIMAL_DIG`, `FLT_TRUE_MIN`,
  `DBL_HAS_SUBNORM`, `DBL_DECIMAL_DIG`, `DBL_TRUE_MIN`,
  `LDBL_HAS_SUBNORM`, `LDBL_DECIMAL_DIG`, `LDBL_TRUE_MIN`,
  that were added in ISO C 11, are missing on some platforms:
  gcc 4.5.4, clang 7, Solaris 11.3 with Sun C.
* The macro `LDBL_DECIMAL_DIG` is missing on some platforms:
  MSVC 14.
* The value of macro `FLT_TRUE_MIN` is not a `float` on some platforms:
  AIX 7.1 with xlc.
* The macros
  `FLT_IS_IEC_60559`, `FLT_NORM_MAX`, `FLT_SNAN`,
  `DBL_IS_IEC_60559`, `DBL_NORM_MAX`, `DBL_SNAN`,
  `LDBL_IS_IEC_60559`, `LDBL_NORM_MAX`, `LDBL_SNAN`,
  that were added in ISO C 23, are missing on some platforms:
  gcc 14 without `-std=c23` or `-std=gnu23`, clang 18, musl libc 1.2.5, AIX 7.3 with xlclang, Solaris 11.3 with Sun C, MSVC 14.

Portability problems not fixed by Gnulib:

* The macro `FLT_ROUNDS` is a constant expression and does not represent
  the current rounding mode on some platforms:
  glibc 2.11, HP-UX 11, mingw.
* The value of `FLT_TRUE_MIN` is not a constant on some platforms:
  FreeBSD/x86.
* The values of `FLT_SNAN`, `DBL_SNAN`, `LDBL_SNAN` are not
  constants on some platforms:
  AIX 7.3 with xlclang, and all platforms which use the gnulib replacements.

---

Next: [`fnmatch.h`](#fnmatch_002eh), Previous: [`float.h`](#float_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.15 `fmtmsg.h` [¶](#fmtmsg_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fmtmsg.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, Cygwin, mingw, MSVC 14, Android 9.0.

---

Next: [`ftw.h`](#ftw_002eh), Previous: [`fmtmsg.h`](#fmtmsg_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.16 `fnmatch.h` [¶](#fnmatch_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fnmatch.h.html>

Gnulib module: fnmatch-h, fnmatch-gnu

Portability problems fixed by Gnulib module `fnmatch-h`:

* This header file is missing on some platforms:
  mingw, MSVC 14.

Portability problems fixed by Gnulib module `fnmatch-gnu`, together with module `fnmatch-h`:

* The macros `FNM_LEADING_DIR` and `FNM_CASEFOLD` are not defined
  on some platforms:
  AIX 7.2, HP-UX 11.31, Solaris 10.
* The macro `FNM_EXTMATCH` is not defined on all non-glibc platforms:
  musl libc, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.2, Minix 3.3, AIX 7.2, HP-UX 11.31, Solaris 11.4, Cygwin 3.4.6, Android 13.
* The macro `FNM_FILE_NAME` is not defined on some platforms:
  NetBSD 10.0, AIX 7.2, HP-UX 11.31, Solaris 10.

Portability problems not fixed by Gnulib:

---

Next: [`glob.h`](#glob_002eh), Previous: [`fnmatch.h`](#fnmatch_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.17 `ftw.h` [¶](#ftw_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ftw.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 3.0, Minix 3.1.8, mingw, MSVC 14.
* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the header `<sys/stat.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`grp.h`](#grp_002eh), Previous: [`ftw.h`](#ftw_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.18 `glob.h` [¶](#glob_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/glob.h.html>

Gnulib module: glob-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`iconv.h`](#iconv_002eh), Previous: [`glob.h`](#glob_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.19 `grp.h` [¶](#grp_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/grp.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`inttypes.h`](#inttypes_002eh), Previous: [`grp.h`](#grp_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.20 `iconv.h` [¶](#iconv_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/iconv.h.html>

Gnulib module: iconv

Portability problems fixed by Gnulib:

* The <iconv.h> from GNU libiconv is not found if installed in
  `$PREFIX/include`.

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  FreeBSD 6.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14,
  when GNU libiconv is not installed.

---

Next: [`iso646.h`](#iso646_002eh), Previous: [`iconv.h`](#iconv_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.21 `inttypes.h` [¶](#inttypes_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/inttypes.h.html>

Gnulib module: inttypes-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  MSVC 9.
* This header file is very incomplete on some platforms.
* The declarations of `imaxabs` and `imaxdiv` are missing on some
  platforms:
  NetBSD 3.0, OpenBSD 3.8, AIX 5.1, HP-UX 11.
* The declarations of `strtoimax` and `strtoumax` are missing on some
  platforms:
  OpenBSD 3.8, AIX 5.1 (missing only `strtoumax`).
* On some hosts that predate C++11, when using C++ one must define
  `__STDC_FORMAT_MACROS` to make visible the declarations of format
  macros such as `PRIdMAX`.

Portability problems not fixed by Gnulib:

* The macros `SCN*8`, `SCN*LEAST8`, `SCN*FAST8`
  are missing on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`.
* The macros `PRIb*`, `PRIB*`, `SCNb*`,
  that were added in ISO C 23,
  are missing on many platforms:
  glibc 2.37 and many others.

---

Next: [`langinfo.h`](#langinfo_002eh), Previous: [`inttypes.h`](#inttypes_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.22 `iso646.h` [¶](#iso646_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/iso646.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, Cygwin, mingw.

---

Next: [`libgen.h`](#libgen_002eh), Previous: [`iso646.h`](#iso646_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.23 `langinfo.h` [¶](#langinfo_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/langinfo.h.html>

Gnulib module: langinfo-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* The constant `CODESET` is not defined on some platforms:
  OpenBSD 3.8.
* The constants `ALTMON_1` to `ALTMON_12` are not defined on some
  platforms:
  glibc 2.26, musl libc, macOS 11.1, NetBSD 8.0, OpenBSD 7.9, AIX 7.2, HP-UX 11.31, Solaris 11.4, Haiku, Cygwin 2.9.
* The constants `ABALTMON_1` to `ABALTMON_12` are not defined on many
  platforms:
  glibc 2.42, musl libc, macOS 14, NetBSD 10.0, OpenBSD 7.6, AIX 7.3, HP-UX 11.31, Solaris 11.4, Haiku, Cygwin 3.5.
* The constants `ERA`, `ERA_D_FMT`, `ERA_D_T_FMT`,
  `ERA_T_FMT`, `ALT_DIGITS` are not defined on some platforms:
  OpenBSD 7.9.

Portability problems not fixed by Gnulib:

* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the header `<nl_types.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`libintl.h`](#libintl_002eh), Previous: [`langinfo.h`](#langinfo_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.24 `libgen.h` [¶](#libgen_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/libgen.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.

The Gnulib module `dirname` provides similar API, with functions
`base_name` and `dir_name` that also work with Windows file names.

---

Next: [`limits.h`](#limits_002eh), Previous: [`libgen.h`](#libgen_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.25 `libintl.h` [¶](#libintl_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/libintl.h.html>

Defines the macros `__USE_GNU_GETTEXT`,
`__GNU_GETTEXT_SUPPORTED_REVISION`, and declares the functions
`gettext`, `dgettext`, `dcgettext`,
`ngettext`, `dngettext`, `dcngettext`,
`textdomain`, `bindtextdomain`, `bind_textdomain_codeset`.

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Message-catalogs-with-gettext.html>,
* <https://www.gnu.org/software/gettext/manual/html_node/gettext.html>.

Gnulib module: gettext

Portability problems fixed by Gnulib, if GNU gettext is installed:

* This header file is missing on some platforms:
  macOS 14, FreeBSD 14.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14, Android 9.0.
* The functions cannot deal with GNU .mo files with system-dependent strings
  (of major version 1 or of minor version 1) on some non-glibc platforms:
  NetBSD 3.0, Solaris 10.

Portability problems not fixed by Gnulib:

---

Next: [`locale.h`](#locale_002eh), Previous: [`libintl.h`](#libintl_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.26 `limits.h` [¶](#limits_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/limits.h.html>

Gnulib module: limits-h or gethostname

Portability problems fixed by Gnulib module `limits-h`:

* The macros `LLONG_MIN`, `LLONG_MAX`, `ULLONG_MAX` are not
  defined on some platforms:
  older glibc systems (e.g. Fedora 1), AIX 5.1, HP-UX 11, OpenVMS.
* The macro `MB_LEN_MAX` is not defined on some platforms:
  pcc 1.2.0.DEVEL 20220331.
* The macros `WORD_BIT`, `LONG_BIT` are not defined on some platforms:
  glibc 2.11 without `-D_GNU_SOURCE`, Cygwin, mingw, MSVC 14.
* Macros like `CHAR_WIDTH` are not defined on some platforms:
  glibc 2.24, NetBSD 10.0, many others.
* The macros `BOOL_MAX` and `BOOL_WIDTH` are not defined on
  some platforms:
  glibc 2.32, many others.
* The macro `BOOL_MAX` is not defined with some compilers:
  clang 15.0.6.
* The macro `SSIZE_MAX` is not defined on some platforms:
  MSVC 14.

Portability problems fixed by Gnulib module `gethostname`:

* The `HOST_NAME_MAX` macro is not defined on some platforms:
  macOS 11.1, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* The macro `SSIZE_MAX` has the wrong type,
  albeit with the correct value:
  32-bit glibc 2.24 (on some architectures), Cygwin 2.5.2.

For `PATH_MAX`, Gnulib provides a module `pathmax` with a header
file `"pathmax.h"`. It defines `PATH_MAX` to a constant on
platforms with a file name length limit.

---

Next: [`math.h`](#math_002eh), Previous: [`limits.h`](#limits_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.27 `locale.h` [¶](#locale_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/locale.h.html>

Gnulib module: locale-h

Portability problems fixed by Gnulib:

* The definition of ‘`LC_MESSAGES`’ is missing on some platforms:
  mingw, MSVC 14.
* The `locale_t` type is not defined on some platforms:
  glibc 2.11, macOS 11.1, FreeBSD 9.0, NetBSD 6.1, OpenBSD 6.1, Solaris 11.3, mingw, MSVC 14, Android 4.4.
* The `struct lconv` type does not contain any members on some platforms:
  Android up to 2014.
* The `struct lconv` type does not contain the members
  `int_p_cs_precedes`, `int_p_sign_posn`, `int_p_sep_by_space`,
  `int_n_cs_precedes`, `int_n_sign_posn`, `int_n_sep_by_space`
  on some platforms:
  glibc, OpenBSD 4.9, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14.
* Some platforms provide a `NULL` macro that cannot be used in arbitrary
  expressions:
  NetBSD 5.0

Portability problems not fixed by Gnulib:

---

Next: [`monetary.h`](#monetary_002eh), Previous: [`locale.h`](#locale_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.28 `math.h` [¶](#math_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/math.h.html>

Gnulib module: math-h

Portability problems fixed by Gnulib:

* The conversion from `int` to `long double` in incorrect on some
  platforms:
  glibc 2.7 on Linux/SPARC64.
* The macro `NAN` is not defined on some platforms:
  OpenBSD 4.0, AIX 5.1.
* The macro `NAN` is not exposed outside of C99 compilation on some
  platforms:
  glibc.
* The macros `NAN` and `HUGE_VAL` expand to a function address
  rather than a floating point constant on some platforms:
  Solaris 10.
* The macros `HUGE_VALF` and `HUGE_VALL` are not defined on some
  platforms:
  glibc/HPPA, glibc/SPARC, AIX 5.1, Solaris 9, MSVC 9.
* The macros `FP_ILOGB0` and `FP_ILOGBNAN` are not defined on some
  platforms:
  NetBSD 5.1, AIX 5.1, Solaris 9, MSVC 9.
* `HUGE_VAL`, `HUGE_VALF`, `HUGE_VALL` are not compile time
  constants on some platforms:
  FreeBSD 5.2, AIX 7.1.
* `INFINITY` and `NAN` are not compile time constants
  on some platforms:
  FreeBSD 5.2, AIX 7.1.
* `INFINITY` and `NAN` are of type `double` instead of `float`
  on some platforms:
  FreeBSD 7.1, mingw 5.0.
* The macros `NAN`, `HUGE_VALL`, and `INFINITY` are not
  defined on some platforms:
  OpenVMS.

Portability problems not fixed by Gnulib:

* `NAN` is not a compile time constant with some compilers:
  OpenVMS.
* The macro or variable `math_errhandling` is not defined on some platforms:
  glibc 2.11, OpenBSD 4.9, NetBSD 5.1, UP-UX 11, Cygwin 1.7.9, mingw, MSVC 9.
* The macros `FP_ILOGB0` and `FP_ILOGBNAN` have wrong values on some
  platforms:
  Haiku before 2023-04-08.

---

Next: [`mqueue.h`](#mqueue_002eh), Previous: [`math.h`](#math_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.29 `monetary.h` [¶](#monetary_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/monetary.h.html>

Gnulib module: monetary-h

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 1.7.1, mingw, MSVC 14, Android 9.0.
* This header file has a syntax error in C++ mode on some platforms:
  NetBSD 8.0.

---

Next: [`ndbm.h`](#ndbm_002eh), Previous: [`monetary.h`](#monetary_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.30 `mqueue.h` [¶](#mqueue_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/mqueue.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`net/if.h`](#net_002fif_002eh), Previous: [`mqueue.h`](#mqueue_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.31 `ndbm.h` [¶](#ndbm_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ndbm.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  Cygwin 3.0.x, mingw, MSVC 14, Android 9.0.

---

Next: [`netdb.h`](#netdb_002eh), Previous: [`ndbm.h`](#ndbm_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.32 `net/if.h` [¶](#net_002fif_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/net_if.h.html>

Gnulib module: net\_if-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.
* This header file is not self-contained on some platforms (needing
  <sys/socket.h> to be included first):
  Mac OS X 10.5, FreeBSD 8.2, OpenBSD 5.2.

Portability problems not fixed by Gnulib:

---

Next: [`netinet/in.h`](#netinet_002fin_002eh), Previous: [`net/if.h`](#net_002fif_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.33 `netdb.h` [¶](#netdb_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/netdb.h.html>

Gnulib module: netdb-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.
* This header file is incomplete on some platforms:
  Cygwin 1.5.x, Haiku.
* This header file does not define `AI_ALL`, `AI_V4MAPPED`
  on some platforms:
  NetBSD 10.0.
* This header file does not define `AI_ADDRCONFIG` on some platforms:
  NetBSD 5.0.
* This header file does not define `AI_NUMERICHOST` on some platforms:
  HP-UX 11.11.
* This header file does not define `AI_NUMERICSERV` on some platforms:
  macOS 10.5, HP-UX 11.11, mingw.
* This header file incorrectly defines `NI_MAXHOST` on some platforms:
  musl libc 1.2.4, OpenBSD 7.9.

Portability problems not fixed by Gnulib:

* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the headers `<inttypes.h>`,
  `<netnet/in.h>`, and `<sys/socket.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`netinet/tcp.h`](#netinet_002ftcp_002eh), Previous: [`netdb.h`](#netdb_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.34 `netinet/in.h` [¶](#netinet_002fin_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/netinet_in.h.html>

Gnulib module: netinet\_in-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.
* This header file is not self-contained on some platforms (it requires
  `<sys/types.h>` to be included first):
  OpenBSD 4.6.

Portability problems not fixed by Gnulib:

* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the headers `<arpa/inet.h>`, `<inttypes.h>`
  and `<sys/socket.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`nl_types.h`](#nl_005ftypes_002eh), Previous: [`netinet/in.h`](#netinet_002fin_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.35 `netinet/tcp.h` [¶](#netinet_002ftcp_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/netinet_tcp.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`poll.h`](#poll_002eh), Previous: [`netinet/tcp.h`](#netinet_002ftcp_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.36 `nl_types.h` [¶](#nl_005ftypes_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/nl_types.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  Minix 3.1.8, Cygwin 2.9.x, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread.h`](#pthread_002eh), Previous: [`nl_types.h`](#nl_005ftypes_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.37 `poll.h` [¶](#poll_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/poll.h.html>

Gnulib module: poll-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the headers `<signal.h>` and `<time.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`pwd.h`](#pwd_002eh), Previous: [`poll.h`](#poll_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.38 `pthread.h` [¶](#pthread_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html>

Gnulib module: pthread-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms.
  Minix 3.1.8, mingw 2.x, MSVC 14.
  But the provided replacement covers only the essential POSIX threads API.
  Furthermore it is just a dummy on some of these platforms:
  Minix 3.1.8.
* This header pollutes the namespace with some broken macro
  implementations for various functions such as `strtok_r` and
  `gmtime_r`:
  mingw 3.0.

Portability problems not fixed by Gnulib:

---

Next: [`regex.h`](#regex_002eh), Previous: [`pthread.h`](#pthread_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.39 `pwd.h` [¶](#pwd_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pwd.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`sched.h`](#sched_002eh), Previous: [`pwd.h`](#pwd_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.40 `regex.h` [¶](#regex_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/regex.h.html>

Gnulib module: regex

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.
* On some systems that have this header file,
  GNU extensions like `re_set_syntax` and `RE_SYNTAX_EMACS`
  are not declared or supported:
  FreeBSD 14.2, OpenBSD 7.6, NetBSD 10.1, macOS 15,
  Minix 3.3.0, AIX 7.3, HP-UX 11, Solaris 11.4.
* On some systems that support GNU extensions, `RE_SYNTAX_EMACS` is 0
  even though it should be `(RE_CHAR_CLASSES | RE_INTERVALS)`
  to be compatible with Emacs 21 (2001) and later:
  glibc 2.42, Cygwin 2.6.x.

Portability problems not fixed by Gnulib:

* This header file is not self-contained on some platforms: it requires
  `<sys/types.h>` to be included first.

---

Next: [`search.h`](#search_002eh), Previous: [`regex.h`](#regex_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.41 `sched.h` [¶](#sched_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sched.h.html>

Gnulib module: sched-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* This header file does not define the type `pid_t` on some platforms:
  glibc 2.11, macOS 11.1.
* `struct sched_param` is not defined on some platforms:
  Haiku.
* `SCHED_FIFO`, `SCHED_RR`, `SCHED_OTHER` are not defined on
  some platforms:
  Haiku.

Portability problems not fixed by Gnulib:

* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the header `<time.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`semaphore.h`](#semaphore_002eh), Previous: [`sched.h`](#sched_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.42 `search.h` [¶](#search_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/search.h.html>

Gnulib module: search-h

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  Minix 3.1.8.

---

Next: [`setjmp.h`](#setjmp_002eh), Previous: [`search.h`](#search_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.43 `semaphore.h` [¶](#semaphore_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/semaphore.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`signal.h`](#signal_002eh), Previous: [`semaphore.h`](#semaphore_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.44 `setjmp.h` [¶](#setjmp_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/setjmp.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`spawn.h`](#spawn_002eh), Previous: [`setjmp.h`](#setjmp_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.45 `signal.h` [¶](#signal_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html>

Gnulib module: signal-h

Portability problems fixed by Gnulib:

* `volatile sig_atomic_t` is rejected by older compilers on some
  platforms:
  AIX.
* `sigset_t` is missing on some platforms:
  MSVC 14.
* `sigset_t` is only declared in <sys/types.h> on some platforms:
  mingw.
* `struct sigaction` and `siginfo_t` are missing on some
  platforms:
  mingw, MSVC 14.
* The type `pid_t` is not defined on some platforms:
  MSVC 14.
* The signal `SIGPIPE` is not defined on some platforms:
  mingw, MSVC 14.
* The macros `SA_RESETHAND` and `SA_RESTART` are not defined
  on some platforms:
  NonStop.
* The type `sighandler_t` (a GNU extension) is not defined on most non-glibc
  platforms:
  macOS 11.1, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* Many signals are not defined on some platforms:
  mingw, MSVC 14.
* The macro `SIGBUS` is set to the same value as `SIGSEGV`,
  rather than being a distinct signal, on some platforms:
  Haiku.
* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the header `<time.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`stdalign.h`](#stdalign_002eh), Previous: [`signal.h`](#signal_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.46 `spawn.h` [¶](#spawn_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/spawn.h.html>

Gnulib module: spawn-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 1.7.25, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`stdarg.h`](#stdarg_002eh), Previous: [`spawn.h`](#spawn_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.47 `stdalign.h` [¶](#stdalign_002eh-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> sections 6.5.3.4, 6.7.5, 7.15.

C++11 specification:  
 <https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2011/n3242.pdf> section 18.10.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdalign.h.html>

Gnulib module: alignasof

Portability problems fixed by Gnulib:

* On older C platforms `<stdalign.h>` must be included before using
  `alignas` or `alignof`. For example, GCC versions before 13 do not
  support these keywords, which were standardized by C23.
  On C23 and later platforms, `<stdalign.h>` has no effect and need
  not be included.
  (Gnulib-using code should not include
  `<stdalign.h>` without also employing Gnulib’s now-deprecated
  `stdalign-h` module.)
* This header file is missing on many platforms:
  FreeBSD 6.4, NetBSD 7.1, OpenBSD 7.9, Minix 3.3.0, AIX 7.1, HP-UX 11.31, Solaris 11.3, mingw, MSVC 14, Android 9.0.
* Clang 3.0’s `<stdalign.h>` does not define `alignof`.
* The `alignof` macro returns too large values for
  the types `double` and `long long` in GCC 4.7.0.
* Older C platforms might not support the obsolescent
  `_Alignas` and `_Alignof` keywords or macros.
  This portability problem should not matter with code using this module,
  as such code should use `alignas` and `alignof` instead.
* In C11 and C17, `<stdalign.h>` defines the macros
  `__alignas_is_defined` and
  `__alignof_is_defined` to 1.
  In C23, these macros are not defined.
  This portability problem should not matter with code using Gnulib’s
  `alignasof` module, as such code should use `alignas` and
  `alignof` without checking these two macros. (Gnulib’s
  now-deprecated `stdalign-h` module defines these two macros.)
* Clang 17 does not support an `alignas` attribute in the
  specifier-qualifier position of a member declaration.

Portability problems not fixed by Gnulib:

* In C11 and later, the operand of `alignof` must be a
  parenthesized type. Recent versions of GCC support an extension in
  which the operand can also be a unary expression, as with
  `sizeof`. The Gnulib substitute does not support this extension.
* On most pre-C11 platforms, the operand of
  `alignof` cannot be a structure type containing a
  flexible array member.
* The `alignas` keyword or macro is not always supported.
  Supported compilers include any compiler supporting C11 or later,
  which includes GCC, IBM C, Sun C 5.9 and later,
  and MSVC 7.0 and later.
* Some compilers do not support alignment via
  `alignas` of `auto` variables (i.e.,
  variables on the stack). They diagnose and ignore the alignment: Sun
  C 5.11.
* Some linkers do not support operands of `alignas`
  that are greater than 8: mingw.
* Some compilers require the operand of `alignas`
  to be a single integer constant, not an expression: MSVC 7.0 through
  at least 10.0.
* The Sun C 5.13 (2014) compiler sometimes mishandles the alignment of multiple
  external variables that are declared close together with
  `alignas`. The bug is fixed in Sun C 5.15, also known as Oracle
  Developer Studio 12.6 (2017).
* You cannot assume that `alignas` and `alignof` are reserved words;
  they might be macros.

---

Next: [`stdatomic.h`](#stdatomic_002eh), Previous: [`stdalign.h`](#stdalign_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.48 `stdarg.h` [¶](#stdarg_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdarg.h.html>

Gnulib module: stdarg-h

Portability problems fixed by Gnulib:

* Some compilers (e.g., AIX 5.3 cc) need to be in c99 mode for the builtin
  `va_copy` to work.

Portability problems not fixed by Gnulib:

---

Next: [`stdbit.h`](#stdbit_002eh), Previous: [`stdarg.h`](#stdarg_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.49 `stdatomic.h` [¶](#stdatomic_002eh-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdatomic.h.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14.
* Linus Torvalds believes that
  “no compiler writer will get the C/C++ memory model right”;
  see <https://lkml.org/lkml/2018/6/7/761>.

---

Next: [`stdbool.h`](#stdbool_002eh), Previous: [`stdatomic.h`](#stdatomic_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.50 `stdbit.h` [¶](#stdbit_002eh-1)

POSIX :  
 Not in POSIX yet, but we expect it will be.
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.

Gnulib module: stdbit

Portability problems fixed by Gnulib:

* This header file is missing on many non-C23 platforms:
  glibc 2.38, macOS 14, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.
* This header file exists but is a no-op:
  G++ 15.2.
* This header file has syntax errors in C++ mode on some platforms:
  FreeBSD 15.1.
* This header file does not define the necessary types
  with the Intel `icpx` C++ compiler.

Portability problems not fixed by Gnulib:

* On older non-C23 platforms lacking `typeof` or equivalent, a call
  to `stdc_bit_floor` and `stdc_bit_ceil` may yield a type
  that is wider than its argument. Although C23 seems to allow this,
  it differs from GNU behavior.
* On non-C23 platforms, type-generic functions apply portably only to
  the standard unsigned integer types specified by C17 or earlier.

---

Next: [`stdckdint.h`](#stdckdint_002eh), Previous: [`stdbit.h`](#stdbit_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.51 `stdbool.h` [¶](#stdbool_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdbool.h.html>

Gnulib module: stdbool-h-c99

The `stdbool-h-c99` module is present only for programs that
formerly used the old `stdbool` module for C99 compatibility,
and that for some reason cannot use the current `bool` module
for C23 compatibility.

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  AIX 5.1, HP-UX 11.
* This header file is not usable in C++ mode with the vendor compiler
  on Solaris 10.
* Some compilers have bugs relating to ‘`bool`’.
* This header file defines `true` incorrectly on some platforms:
  OpenBSD 4.7 with gcc 2.95.

Portability problems not fixed by Gnulib:

* ‘`_Bool`’ cannot be used before `<stdbool.h>` is included, or if
  the program is intended to be compiled by a C++ compiler.
  (With the advent of C23, ‘`_Bool`’ is obsolescent anyway.)
* You cannot assume that `_Bool` is a keyword or a typedef;
  it might be a macro.
  For example, C23 allows `_Bool` to be a macro.
* Bit-fields of type ‘`bool`’ are not supported. Portable code
  should use ‘`unsigned int foo : 1;`’ rather than ‘`bool foo : 1;`’.
* Casts and automatic conversions to ‘`bool`’ don’t test against the
  zero value or the null pointer, as they should. Such casts should only
  be used if the value is known to be equal to 0 or 1.
* You cannot assume that casting a floating point literal to ‘`bool`’ will
  result in a constant expression.

---

Next: [`stdcountof.h`](#stdcountof_002eh), Previous: [`stdbool.h`](#stdbool_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.52 `stdckdint.h` [¶](#stdckdint_002eh-1)

POSIX specification:  
 Not in POSIX yet, but we expect it will be.
ISO draft C23
(<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.20.

Gnulib module: stdckdint-h

Portability problems fixed by Gnulib:

* This header file is missing on many platforms:
  glibc 2.29, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9.0, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

* In draft C23, arguments of `stdckdint.h` macros can have side effects.

---

Next: [`stddef.h`](#stddef_002eh), Previous: [`stdckdint.h`](#stdckdint_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.53 `stdcountof.h` [¶](#stdcountof_002eh-1)

POSIX specification:  
 Not in POSIX yet, but we expect it will be.
ISO draft C2y N3783
(<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
§ 7.21.

Gnulib module: stdcountof-h

Portability problems fixed by Gnulib:

* This header file is missing on many platforms:
  GCC 15, Clang 20, MSVC 14.50, Oracle Developer Studio 12.6,
  IBM Open XL C for AIX 17.1, TCC 0.9.27.

Portability problems not fixed by Gnulib:

* On some platforms, arguments of `countof` are evaluated
  if they contain designated initializers, e.g.,
  `countof ((int[]) {getchar ()})` incorrectly calls `getchar`.
  This is because these platforms have a similar bug with
  `sizeof ((int[]) {getchar ()})`:
  MSVC 14.50, Oracle Developer Studio 12.6, IBM XL C for AIX 16.1.

---

Next: [`stdint.h`](#stdint_002eh), Previous: [`stdcountof.h`](#stdcountof_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.54 `stddef.h` [¶](#stddef_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stddef.h.html>

Gnulib module: stddef-h

Portability problems fixed by Gnulib:

* Some platforms fail to provide `unreachable`, which was added in C23:
  GCC 13, clang 15, AIX with xlc 12.1, Solaris with Sun C 5.15, and others.
* Some platforms fail to provide `max_align_t`, which was added in C11:
  NetBSD 8.0, Solaris 11.0, and others.
* `max_align_t` does not have the expected alignment on some platforms:
  NetBSD 8.0/x86, AIX 7.2 with xlc in 64-bit mode.
* Some platforms provide a `NULL` macro that cannot be used in arbitrary
  expressions:
  NetBSD 5.0
* Some platforms provide a `NULL` macro whose value does not have the size
  of a pointer:
  AIX 7.2 with xlc in 64-bit mode.
* When this header file is provided by TinyCC 0.9.27 on glibc or macOS systems,
  it does not fulfil the expectations of other system header files.

Portability problems not fixed by Gnulib:

* Some platforms fail to provide `nullptr_t`,
  which Gnulib cannot usefully emulate:
  GCC 12, Clang 15, and other pre-2023 C compilers.
* Some platforms define `nullptr_t` even when `<stddef.h>` is
  not included:
  GCC 14
* Some platforms provide an `offsetof` macro that cannot be used in
  arbitrary expressions:
  Solaris 11.4
  This problem can be worked around by parenthesizing the
  `offsetof` expression in the unlikely case you use it with
  `sizeof` or ‘`[]`’.

---

Next: [`stdio.h`](#stdio_002eh), Previous: [`stddef.h`](#stddef_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.55 `stdint.h` [¶](#stdint_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdint.h.html>

Gnulib module: stdint-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  OpenBSD 3.8, AIX 5.1, HP-UX 11.11, MSVC 9.
* This header file is very incomplete on some platforms.
* The values of `SIG_ATOMIC_MIN` and `SIG_ATOMIC_MAX` are incorrect
  on some platforms:
  FreeBSD 6.2/ia64, FreeBSD 13.0/arm64.
* The value of `WINT_MAX` is incorrect on some platforms:
  mingw.
* The values of `INT8_MAX`, `UINT8_MAX` etc. are not usable in
  preprocessor expressions on some platforms:
  HP-UX 11.23.
* The values of `INTPTR_MAX` and `UINTPTR_MAX`, although correctly
  defined in `<stdint.h>`, are replaced by empty values when
  `<limits.h>` or `<inttypes.h>` gets included later on some platforms:
  Solaris 9 with GCC 4.5 or newer.
* The macros `WCHAR_MIN` and `WCHAR_MAX` are not defined in
  `<stdint.h>` (only in `<wchar.h>`) on some platforms:
  Dragonfly.
* On some hosts that predate C++11, when using C++ one must define
  `__STDC_CONSTANT_MACROS` to make visible the definitions of
  constant macros such as `INTMAX_C`, and one must define
  `__STDC_LIMIT_MACROS` to make visible the definitions of limit
  macros such as `INTMAX_MAX`.
* The macro `SIZE_MAX` has the wrong type,
  albeit with the correct value:
  32-bit glibc 2.24 (on s390 architecture), Mac OS X 10.7.
* Macros like `INTMAX_WIDTH` are not defined on some platforms:
  glibc 2.24, NetBSD 10.0, many others.

Portability problems not fixed by Gnulib:

* `{uint,int}_fast{8,16,32,64}_t` may not correspond to the fastest
  types available on the system.
  Other `<stdint.h>` substitutes may define these types differently,
  so public header files should avoid these types.
* Macros are used instead of typedefs.
* Some C preprocessors mishandle constants that do not fit in `long int`.
  For example, as of 2007, Sun C mishandled `#if LLONG_MIN < 0` on
  a platform with 32-bit `long int` and 64-bit `long long int`;
  this bug was fixed on or before Oracle Developer Studio 12.6
  (Sun C 5.15 SunOS\_sparc 2017/05/30).
  Some older preprocessors mishandle constants ending in `LL`.
  To work around these problems, compute the value of expressions like
  `LONG_MAX < LLONG_MAX` at `configure`-time rather than at
  `#if`-time.

The `stdint-h` module uses `#include_next`. If you wish to install
the generated stdint.h file under another name, typically in order to
be able to use some of the types defined by stdint.h in your public
header file, you could use the following Makefile.am-snippet:

```
BUILT_SOURCES += idn-int.h
DISTCLEANFILES += idn-int.h
nodist_include_HEADERS += idn-int.h

idn-int.h:
	if test -n "$(STDINT_H)"; then \
		sed -e s/include_next/include/ gl/stdint.h > idn-int.h; \
	else \
		echo '#include <stdint.h>' > idn-int.h; \
	fi
```

---

Next: [`stdlib.h`](#stdlib_002eh), Previous: [`stdint.h`](#stdint_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.56 `stdio.h` [¶](#stdio_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html>

Gnulib module: stdio-h

Portability problems fixed by Gnulib:

* The type `off_t` is not defined on some platforms:
  glibc 2.8, eglibc 2.11.2 and others.
* The type `ssize_t` is not defined on some platforms:
  glibc 2.8, Mac OS X 10.5, Solaris 10, MSVC 14, and others.
* The type `va_list` is not defined on some platforms:
  glibc 2.8, OpenBSD 4.0, Solaris 11.4, and others.
* The type `off64_t` is not defined on some platforms:
  macOS 14, FreeBSD 10.4, NetBSD 10.0, OpenBSD 7.9, MSVC 14, Cygwin, Haiku, Minix 3.3.
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

Next: [`stdnoreturn.h`](#stdnoreturn_002eh), Previous: [`stdio.h`](#stdio_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 9.57 `stdlib.h` [¶](#stdlib_002eh-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html>

Gnulib module: stdlib-h, system-posix

Portability problems fixed by the Gnulib module `stdlib-h`:

* The macros `EXIT_SUCCESS` and `EXIT_FAILURE` are not defined on
  some platforms.
* Some platforms provide a `NULL` macro that cannot be used in arbitrary
  expressions:
  NetBSD 5.0
* Using the macro `MB_CUR_MAX` causes link errors on some platforms:
  Android NDK r16.
* The value of `MB_CUR_MAX` is too small (3 instead of 4) in UTF-8 locales
  on some platforms:
  Solaris 10.

Portability problems fixed by the Gnulib module `system-posix`:

* The macros `WIFSIGNALED`, `WIFEXITED`, `WIFSTOPPED`,
  `WTERMSIG`, `WEXITSTATUS`, `WNOHANG`, `WUNTRACED`,
  `WSTOPSIG` are not defined in this header file (only in
  `<sys/wait.h>`) on some platforms:
  MirBSD 10.
* The macro `WCOREDUMP` is not defined in this header file (only in
  `<sys/wait.h>`) on some platforms:
  glibc 2.42.

Portability problems not fixed by Gnulib:

* The definition of the type `once_flag`, of the macro
  `ONCE_FLAG_INIT`, and the declaration of the function
  `call_once`, that are required by ISO C 23, are not provided.
  To get them, import Gnulib module `call_once` and include
  `<threads.h>` rather than `<stdlib.h>`.
* System status macros such as `WEXITSTATUS` require an lvalue
  argument on some platforms.
  macOS 11.1.
* POSIX.1-2024 allows, but does not require, this file to make visible
  all symbols from the headers `<fcntl.h>`, `<limits.h>`,
  `<math.h>`, `<stddef.h>`, and `<sys/wait.h>`.
  Programs should not assume that these other symbols are visible merely
  because this file is included, or that they are invisible because only
  this file is included.

---

Next: [`string.h`](#string_002eh), Previous: [`stdlib.h`](#stdlib_002eh), Up: [ISO C and POSIX Header File Substitutes](#Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
