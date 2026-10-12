### 13.29 `shadow.h` [¶](#shadow_002eh-1)

Defines the type `struct spwd` and declares the functions
`setspent`, `endspent`,
`getspent`, `getspent_r`,
`getspnam`, `getspnam_r`,
`sgetspent`, `sgetspent_r`,
`fgetspent`, `fgetspent_r`,
`putspent`,
`lckpwdf`, `ulckpwdf`.

Documentation:

* [man setspent](https://www.kernel.org/doc/man-pages/online/pages/man3/setspent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, Cygwin, mingw, MSVC 14, Android 9.0.
* The functions `getspent_r`, `getspnam_r`, `sgetspent_r`,
  `fgetspent`, `fgetspent_r`, `putspent` are missing on some
  platforms:
  HP-UX 11.
* The functions `sgetspent`, `sgetspent_r` are missing on some
  platforms:
  HP-UX 11, Solaris 11.4.

---

Next: [`sys/ioctl.h`](#sys_002fioctl_002eh), Previous: [`shadow.h`](#shadow_002eh), Up: [Glibc Header File Substitutes](#Glibc-Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 13.30 `sys/file.h` [¶](#sys_002ffile_002eh-1)

Declares the function `flock`.

Gnulib module: sys\_file-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

* This header file does not declare the `flock` function on some platforms:
  AIX 7.1, HP-UX 11.23, Solaris 11.4.

---

Next: [`sys/random.h`](#sys_002frandom_002eh), Previous: [`sys/file.h`](#sys_002ffile_002eh), Up: [Glibc Header File Substitutes](#Glibc-Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 13.31 `sys/ioctl.h` [¶](#sys_002fioctl_002eh-1)

Declares the function `ioctl`.

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/IOCTLs.html>,
* [man ioctl](https://www.kernel.org/doc/man-pages/online/pages/man2/ioctl.2.html).

Gnulib module: sys\_ioctl-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.
* This header file does not declare the `ioctl` function on some platforms:
  AIX 5.1, Solaris 11.4, Haiku 2017.

Portability problems not fixed by Gnulib:

---

Next: [`sysexits.h`](#sysexits_002eh), Previous: [`sys/ioctl.h`](#sys_002fioctl_002eh), Up: [Glibc Header File Substitutes](#Glibc-Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 13.32 `sys/random.h` [¶](#sys_002frandom_002eh-1)

Declares the function `getrandom` and the flags for it.

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Unpredictable-Bytes.html>,
* [man getrandom](https://www.kernel.org/doc/man-pages/online/pages/man2/getrandom.2.html).

Gnulib module: sys\_random-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  glibc 2.24, NetBSD 9.3, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.11, Cygwin 2.6.x, mingw, MSVC 14.
* This header file is not self-contained on some platforms:
  uClibc, macOS 11.1.

Portability problems not fixed by Gnulib:

---

Next: [`ttyent.h`](#ttyent_002eh), Previous: [`sys/random.h`](#sys_002frandom_002eh), Up: [Glibc Header File Substitutes](#Glibc-Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 13.33 `sysexits.h` [¶](#sysexits_002eh-1)

Defines the `EX_*` macros, including `EX_OK`.

Gnulib module: sysexits-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  mingw, MSVC 14.
* This header file defines no `EX_*` macros if `_BSD_SOURCE` is not
  defined on some platforms:
  Haiku.
* The macro `EX_CONFIG` is missing on some platforms:
  HP-UX 11.

Portability problems not fixed by Gnulib:

---

Next: [`utmp.h`](#utmp_002eh), Previous: [`sysexits.h`](#sysexits_002eh), Up: [Glibc Header File Substitutes](#Glibc-Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 13.34 `ttyent.h` [¶](#ttyent_002eh-1)

Defines the type `struct ttyent` and declares the functions
`setttyent`, `endttyent`, `getttyent`, `getttynam`.

Documentation:

* [man setttyent](https://www.kernel.org/doc/man-pages/online/pages/man3/setttyent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This header file is missing on some platforms:
  HP-UX 11, Solaris 11.4, Cygwin, mingw, MSVC 14, Android 9.0.

---

Previous: [`ttyent.h`](#ttyent_002eh), Up: [Glibc Header File Substitutes](#Glibc-Header-File-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 13.35 `utmp.h` [¶](#utmp_002eh-1)

Defines functions for login and logout (to a tty session) and for examining the
history of logins and logouts.

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Manipulating-the-Database.html>,
* [man utmp](https://www.kernel.org/doc/man-pages/online/pages/man5/utmp.5.html).

Gnulib module: utmp-h

Portability problems fixed by Gnulib:

* This header file is missing on some platforms:
  FreeBSD 14.0, mingw, MSVC 14.
* `<sys/types.h>` is a prerequisite of `<utmp.h>` on some platforms:
  FreeBSD 8.0, OpenBSD 7.2.

Portability problems not fixed by Gnulib:

* While some platforms have the `struct utmp` field `ut_user`,
  older platforms have the field `ut_name`.
* The `struct utmp` fields `ut_id`, `ut_pid`, `ut_type`
  do not exist on some platforms:
  macOS, old FreeBSD, NetBSD, OpenBSD, Minix.
* The `struct utmp` field `ut_host` does not exist on some platforms:
  Solaris.
* The `struct utmp` field `ut_exit` does not exist on some platforms:
  macOS, old FreeBSD, NetBSD, OpenBSD, Minix, Cygwin.
* The `struct utmp` field `ut_session` does not exist on some platforms:
  macOS, old FreeBSD, NetBSD, OpenBSD, Minix, AIX, HP-UX, Solaris, Cygwin.
* The `struct utmp` field `ut_addr` or `ut_addr_v6` does not exist
  on some platforms:
  macOS, old FreeBSD, NetBSD, OpenBSD, Minix, AIX, Solaris.
* On some platforms, this API does not support timestamps past the
  year 2038:
  glibc 2.38 on 32-bit platforms like x86 and ARM where `time_t`
  was historically 32 bits.
* On some platforms, this header misbehaves if the `year2038` or
  `year2038-recommended` modules are used and the program is
  configured without the `--disable-year2038` option.
  The `readutmp` module works around this problem:
  glibc 2.38 on 32-bit platforms like x86 and ARM where `time_t`
  was historically 32 bits.
  See [Avoiding the year 2038 problem](#Avoiding-the-year-2038-problem).

---

Next: [Native Windows Support](#Native-Windows-Support), Previous: [Glibc Header File Substitutes](#Glibc-Header-File-Substitutes), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 14 Glibc Function Substitutes [¶](#Glibc-Function-Substitutes-1)

This chapter describes which functions and function-like macros
provided as extensions by at least GNU libc are also supported by Gnulib,
which portability pitfalls are fixed by Gnulib, and which (known)
portability problems are not worked around by Gnulib.

The notation “Gnulib module: —” means that Gnulib does not provide a
module providing a substitute for the function. When the list
“Portability problems not fixed by Gnulib” is empty, such a module is
not needed: No portability problems are known. Otherwise, it indicates
that such a module would be useful but is not available: No one so far
found this function important enough to contribute a substitute for it.
If you need this particular function, you may write to
`<bug-gnulib at gnu dot org>`.

This list of functions is sorted according to the header that declares them.

* [Glibc Extensions to `<aio.h>`](#Glibc-aio_002eh)
* [Glibc `<aliases.h>`](#Glibc-aliases_002eh)
* [Glibc `<argp.h>`](#Glibc-argp_002eh)
* [Glibc `<argz.h>`](#Glibc-argz_002eh)
* [Glibc Extensions to `<arpa/inet.h>`](#Glibc-arpa_002finet_002eh)
* [Glibc Extensions to `<arpa/nameser.h>`](#Glibc-arpa_002fnameser_002eh)
* [Glibc `<byteswap.h>`](#Glibc-byteswap_002eh)
* [Glibc Extensions to `<complex.h>`](#Glibc-complex_002eh)
* [Glibc Extensions to `<ctype.h>`](#Glibc-ctype_002eh)
* [Glibc Extensions to `<dirent.h>`](#Glibc-dirent_002eh)
* [Glibc Extensions to `<dlfcn.h>`](#Glibc-dlfcn_002eh)
* [Glibc `<envz.h>`](#Glibc-envz_002eh)
* [Glibc `<err.h>`](#Glibc-err_002eh)
* [Glibc Extensions to `<errno.h>`](#Glibc-errno_002eh)
* [Glibc `<error.h>`](#Glibc-error_002eh)
* [Glibc `<execinfo.h>`](#Glibc-execinfo_002eh)
* [Glibc Extensions to `<fcntl.h>`](#Glibc-fcntl_002eh)
* [Glibc Extensions to `<fenv.h>`](#Glibc-fenv_002eh)
* [Glibc Extensions to `<fmtmsg.h>`](#Glibc-fmtmsg_002eh)
* [Glibc `<fstab.h>`](#Glibc-fstab_002eh)
* [Glibc `<fts.h>`](#Glibc-fts_002eh)
* [Glibc `<getopt.h>`](#Glibc-getopt_002eh)
* [Glibc Extensions to `<glob.h>`](#Glibc-glob_002eh)
* [Glibc Extensions to `<gnu/libc-version.h>`](#Glibc-gnu_002flibc_002dversion_002eh)
* [Glibc Extensions to `<grp.h>`](#Glibc-grp_002eh)
* [Glibc `<gshadow.h>`](#Glibc-gshadow_002eh)
* [Glibc `<ifaddrs.h>`](#Glibc-ifaddrs_002eh)
* [Glibc `<link.h>`](#Glibc-link_002eh)
* [Glibc `<malloc.h>`](#Glibc-malloc_002eh)
* [Glibc Extensions to `<math.h>`](#Glibc-math_002eh)
* [Glibc `<mcheck.h>`](#Glibc-mcheck_002eh)
* [Glibc `<mntent.h>`](#Glibc-mntent_002eh)
* [Glibc Extensions to `<netdb.h>`](#Glibc-netdb_002eh)
* [Glibc `<netinet/ether.h>`](#Glibc-netinet_002fether_002eh)
* [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)
* [Glibc `<obstack.h>`](#Glibc-obstack_002eh)
* [Glibc `<printf.h>`](#Glibc-printf_002eh)
* [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)
* [Glibc `<pty.h>`](#Glibc-pty_002eh)
* [Glibc Extensions to `<pwd.h>`](#Glibc-pwd_002eh)
* [Glibc Extensions to `<regex.h>`](#Glibc-regex_002eh)
* [Glibc `<regexp.h>`](#Glibc-regexp_002eh)
* [Glibc `<resolv.h>`](#Glibc-resolv_002eh)
* [Glibc `<rpc/auth.h>`](#Glibc-rpc_002fauth_002eh)
* [Glibc `<rpc/auth_des.h>`](#Glibc-rpc_002fauth_005fdes_002eh)
* [Glibc `<rpc/auth_unix.h>`](#Glibc-rpc_002fauth_005funix_002eh)
* [Glibc `<rpc/clnt.h>`](#Glibc-rpc_002fclnt_002eh)
* [Glibc `<rpc/key_prot.h>`](#Glibc-rpc_002fkey_005fprot_002eh)
* [Glibc `<rpc/netdb.h>`](#Glibc-rpc_002fnetdb_002eh)
* [Glibc `<rpc/pmap_clnt.h>`](#Glibc-rpc_002fpmap_005fclnt_002eh)
* [Glibc `<rpc/pmap_prot.h>`](#Glibc-rpc_002fpmap_005fprot_002eh)
* [Glibc `<rpc/pmap_rmt.h>`](#Glibc-rpc_002fpmap_005frmt_002eh)
* [Glibc `<rpc/rpc_msg.h>`](#Glibc-rpc_002frpc_005fmsg_002eh)
* [Glibc `<rpc/svc.h>`](#Glibc-rpc_002fsvc_002eh)
* [Glibc `<rpc/xdr.h>`](#Glibc-rpc_002fxdr_002eh)
* [Glibc `<rpcsvc/nislib.h>`](#Glibc-rpcsvc_002fnislib_002eh)
* [Glibc `<rpcsvc/nis_callback.h>`](#Glibc-rpcsvc_002fnis_005fcallback_002eh)
* [Glibc `<rpcsvc/yp.h>`](#Glibc-rpcsvc_002fyp_002eh)
* [Glibc `<rpcsvc/ypclnt.h>`](#Glibc-rpcsvc_002fypclnt_002eh)
* [Glibc Extensions to `<sched.h>`](#Glibc-sched_002eh)
* [Glibc Extensions to `<search.h>`](#Glibc-search_002eh)
* [Glibc Extensions to `<selinux/selinux.h>`](#Glibc-selinux_002fselinux_002eh)
* [Glibc `<shadow.h>`](#Glibc-shadow_002eh)
* [Glibc Extensions to `<signal.h>`](#Glibc-signal_002eh)
* [Glibc Extensions to `<spawn.h>`](#Glibc-spawn_002eh)
* [Glibc Extensions to `<stdio.h>`](#Glibc-stdio_002eh)
* [Glibc Extensions to `<stdlib.h>`](#Glibc-stdlib_002eh)
* [Glibc Extensions to `<string.h>`](#Glibc-string_002eh)
* [Glibc `<sys/auxv.h>`](#Glibc-sys_002fauxv_002eh)
* [Glibc `<sys/capability.h>`](#Glibc-sys_002fcapability_002eh)
* [Glibc `<sys/epoll.h>`](#Glibc-sys_002fepoll_002eh)
* [Glibc `<sys/eventfd.h>`](#Glibc-sys_002feventfd_002eh)
* [Glibc `<sys/fanotify.h>`](#Glibc-sys_002ffanotify_002eh)
* [Glibc `<sys/file.h>`](#Glibc-sys_002ffile_002eh)
* [Glibc `<sys/fsuid.h>`](#Glibc-sys_002ffsuid_002eh)
* [Glibc `<sys/gmon.h>`](#Glibc-sys_002fgmon_002eh)
* [Glibc `<sys/inotify.h>`](#Glibc-sys_002finotify_002eh)
* [Glibc `<sys/io.h>`, `<sys/perm.h>`](#Glibc-sys_002fio_002eh-and-sys_002fperm_002eh)
* [Glibc `<sys/kdaemon.h>`](#Glibc-sys_002fkdaemon_002eh)
* [Glibc `<sys/klog.h>`](#Glibc-sys_002fklog_002eh)
* [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh)
* [Glibc `<sys/mount.h>`](#Glibc-sys_002fmount_002eh)
* [Glibc `<sys/personality.h>`](#Glibc-sys_002fpersonality_002eh)
* [Glibc `<sys/pidfd.h>`](#Glibc-sys_002fpidfd_002eh)
* [Glibc `<sys/prctl.h>`](#Glibc-sys_002fprctl_002eh)
* [Glibc `<sys/profil.h>`](#Glibc-sys_002fprofil_002eh)
* [Glibc `<sys/ptrace.h>`](#Glibc-sys_002fptrace_002eh)
* [Glibc `<sys/quota.h>`](#Glibc-sys_002fquota_002eh)
* [Glibc `<sys/random.h>`](#Glibc-sys_002frandom_002eh)
* [Glibc `<sys/reboot.h>`](#Glibc-sys_002freboot_002eh)
* [Glibc Extensions to `<sys/resource.h>`](#Glibc-sys_002fresource_002eh)
* [Glibc Extensions to `<sys/sem.h>`](#Glibc-sys_002fsem_002eh)
* [Glibc `<sys/sendfile.h>`](#Glibc-sys_002fsendfile_002eh)
* [Glibc `<sys/signalfd.h>`](#Glibc-sys_002fsignalfd_002eh)
* [Glibc `<sys/single_threaded.h>`](#Glibc-sys_002fsingle_005fthreaded_002eh)
* [Glibc Extensions to `<sys/socket.h>`](#Glibc-sys_002fsocket_002eh)
* [Glibc Extensions to `<sys/stat.h>`](#Glibc-sys_002fstat_002eh)
* [Glibc `<sys/statfs.h>`](#Glibc-sys_002fstatfs_002eh)
* [Glibc `<sys/swap.h>`](#Glibc-sys_002fswap_002eh)
* [Glibc `<sys/sysctl.h>`](#Glibc-sys_002fsysctl_002eh)
* [Glibc `<sys/sysinfo.h>`](#Glibc-sys_002fsysinfo_002eh)
* [Glibc `<sys/syslog.h>`](#Glibc-sys_002fsyslog_002eh)
* [Glibc `<sys/sysmacros.h>`](#Glibc-sys_002fsysmacros_002eh)
* [Glibc Extensions to `<sys/time.h>`](#Glibc-sys_002ftime_002eh)
* [Glibc `<sys/timerfd.h>`](#Glibc-sys_002ftimerfd_002eh)
* [Glibc `<sys/timex.h>`](#Glibc-sys_002ftimex_002eh)
* [Glibc Extensions to `<sys/uio.h>`](#Glibc-sys_002fuio_002eh)
* [Glibc `<sys/ustat.h>`](#Glibc-sys_002fustat_002eh)
* [Glibc `<sys/vlimit.h>`](#Glibc-sys_002fvlimit_002eh)
* [Glibc Extensions to `<sys/wait.h>`](#Glibc-sys_002fwait_002eh)
* [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh)
* [Glibc Extensions to `<termios.h>`](#Glibc-termios_002eh)
* [Glibc Extensions to `<time.h>`](#Glibc-time_002eh)
* [Glibc `<ttyent.h>`](#Glibc-ttyent_002eh)
* [Glibc Extensions to `<unistd.h>`](#Glibc-unistd_002eh)
* [Glibc `<utmp.h>`](#Glibc-utmp_002eh)
* [Glibc Extensions to `<utmpx.h>`](#Glibc-utmpx_002eh)
* [Glibc Extensions to `<wchar.h>`](#Glibc-wchar_002eh)

---

Next: [Glibc `<aliases.h>`](#Glibc-aliases_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.1 Glibc Extensions to `<aio.h>` [¶](#Glibc-Extensions-to-_003caio_002eh_003e)

* [`aio_init`](#aio_005finit)

---

Up: [Glibc Extensions to `<aio.h>`](#Glibc-aio_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.1.1 `aio_init` [¶](#aio_005finit-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Configuration-of-AIO.html>,
* [man aio\_init](https://www.kernel.org/doc/man-pages/online/pages/man3/aio_init.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<argp.h>`](#Glibc-argp_002eh), Previous: [Glibc Extensions to `<aio.h>`](#Glibc-aio_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.2 Glibc `<aliases.h>` [¶](#Glibc-_003caliases_002eh_003e)

* [`endaliasent`](#endaliasent)
* [`getaliasbyname`](#getaliasbyname)
* [`getaliasbyname_r`](#getaliasbyname_005fr)
* [`getaliasent`](#getaliasent)
* [`getaliasent_r`](#getaliasent_005fr)
* [`setaliasent`](#setaliasent)

---

Next: [`getaliasbyname`](#getaliasbyname), Up: [Glibc `<aliases.h>`](#Glibc-aliases_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.2.1 `endaliasent` [¶](#endaliasent-1)

Documentation:  
 [man endaliasent](https://www.kernel.org/doc/man-pages/online/pages/man3/endaliasent.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getaliasbyname_r`](#getaliasbyname_005fr), Previous: [`endaliasent`](#endaliasent), Up: [Glibc `<aliases.h>`](#Glibc-aliases_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.2.2 `getaliasbyname` [¶](#getaliasbyname-1)

Documentation:  
 [man getaliasbyname](https://www.kernel.org/doc/man-pages/online/pages/man3/getaliasbyname.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getaliasent`](#getaliasent), Previous: [`getaliasbyname`](#getaliasbyname), Up: [Glibc `<aliases.h>`](#Glibc-aliases_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.2.3 `getaliasbyname_r` [¶](#getaliasbyname_005fr-1)

Documentation:  
 [man getaliasbyname\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getaliasbyname_r.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getaliasent_r`](#getaliasent_005fr), Previous: [`getaliasbyname_r`](#getaliasbyname_005fr), Up: [Glibc `<aliases.h>`](#Glibc-aliases_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.2.4 `getaliasent` [¶](#getaliasent-1)

Documentation:  
 [man getaliasent](https://www.kernel.org/doc/man-pages/online/pages/man3/getaliasent.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`setaliasent`](#setaliasent), Previous: [`getaliasent`](#getaliasent), Up: [Glibc `<aliases.h>`](#Glibc-aliases_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.2.5 `getaliasent_r` [¶](#getaliasent_005fr-1)

Documentation:  
 [man getaliasent\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getaliasent_r.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`getaliasent_r`](#getaliasent_005fr), Up: [Glibc `<aliases.h>`](#Glibc-aliases_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.2.6 `setaliasent` [¶](#setaliasent-1)

Documentation:  
 [man setaliasent](https://www.kernel.org/doc/man-pages/online/pages/man3/setaliasent.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<argz.h>`](#Glibc-argz_002eh), Previous: [Glibc `<aliases.h>`](#Glibc-aliases_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.3 Glibc `<argp.h>` [¶](#Glibc-_003cargp_002eh_003e)

* [`argp_err_exit_status`](#argp_005ferr_005fexit_005fstatus)
* [`argp_error`](#argp_005ferror)
* [`argp_failure`](#argp_005ffailure)
* [`argp_help`](#argp_005fhelp)
* [`argp_parse`](#argp_005fparse)
* [`argp_program_bug_address`](#argp_005fprogram_005fbug_005faddress)
* [`argp_program_version`](#argp_005fprogram_005fversion)
* [`argp_program_version_hook`](#argp_005fprogram_005fversion_005fhook)
* [`argp_state_help`](#argp_005fstate_005fhelp)
* [`argp_usage`](#argp_005fusage)

---

Next: [`argp_error`](#argp_005ferror), Up: [Glibc `<argp.h>`](#Glibc-argp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.3.1 `argp_err_exit_status` [¶](#argp_005ferr_005fexit_005fstatus-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Argp-Global-Variables.html>.

Gnulib module: argp

Portability problems fixed by Gnulib:

* This variable is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`argp_failure`](#argp_005ffailure), Previous: [`argp_err_exit_status`](#argp_005ferr_005fexit_005fstatus), Up: [Glibc `<argp.h>`](#Glibc-argp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.3.2 `argp_error` [¶](#argp_005ferror-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Argp-Helper-Functions.html>.

Gnulib module: argp

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`argp_help`](#argp_005fhelp), Previous: [`argp_error`](#argp_005ferror), Up: [Glibc `<argp.h>`](#Glibc-argp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.3.3 `argp_failure` [¶](#argp_005ffailure-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Argp-Helper-Functions.html>.

Gnulib module: argp

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`argp_parse`](#argp_005fparse), Previous: [`argp_failure`](#argp_005ffailure), Up: [Glibc `<argp.h>`](#Glibc-argp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.3.4 `argp_help` [¶](#argp_005fhelp-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Argp-Help.html>.

Gnulib module: argp

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`argp_program_bug_address`](#argp_005fprogram_005fbug_005faddress), Previous: [`argp_help`](#argp_005fhelp), Up: [Glibc `<argp.h>`](#Glibc-argp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.3.5 `argp_parse` [¶](#argp_005fparse-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Argp.html>.

Gnulib module: argp

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`argp_program_version`](#argp_005fprogram_005fversion), Previous: [`argp_parse`](#argp_005fparse), Up: [Glibc `<argp.h>`](#Glibc-argp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.3.6 `argp_program_bug_address` [¶](#argp_005fprogram_005fbug_005faddress-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Argp-Global-Variables.html>.

Gnulib module: argp

Portability problems fixed by Gnulib:

* This variable is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`argp_program_version_hook`](#argp_005fprogram_005fversion_005fhook), Previous: [`argp_program_bug_address`](#argp_005fprogram_005fbug_005faddress), Up: [Glibc `<argp.h>`](#Glibc-argp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.3.7 `argp_program_version` [¶](#argp_005fprogram_005fversion-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Argp-Global-Variables.html>.

Gnulib module: argp

Portability problems fixed by Gnulib:

* This variable is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`argp_state_help`](#argp_005fstate_005fhelp), Previous: [`argp_program_version`](#argp_005fprogram_005fversion), Up: [Glibc `<argp.h>`](#Glibc-argp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.3.8 `argp_program_version_hook` [¶](#argp_005fprogram_005fversion_005fhook-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Argp-Global-Variables.html>.

Gnulib module: argp

Portability problems fixed by Gnulib:

* This variable is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`argp_usage`](#argp_005fusage), Previous: [`argp_program_version_hook`](#argp_005fprogram_005fversion_005fhook), Up: [Glibc `<argp.h>`](#Glibc-argp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.3.9 `argp_state_help` [¶](#argp_005fstate_005fhelp-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Argp-Helper-Functions.html>.

Gnulib module: argp

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Previous: [`argp_state_help`](#argp_005fstate_005fhelp), Up: [Glibc `<argp.h>`](#Glibc-argp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.3.10 `argp_usage` [¶](#argp_005fusage-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Argp-Helper-Functions.html>.

Gnulib module: argp

Portability problems fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [Glibc Extensions to `<arpa/inet.h>`](#Glibc-arpa_002finet_002eh), Previous: [Glibc `<argp.h>`](#Glibc-argp_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.4 Glibc `<argz.h>` [¶](#Glibc-_003cargz_002eh_003e)

* [`argz_add`](#argz_005fadd)
* [`argz_add_sep`](#argz_005fadd_005fsep)
* [`argz_append`](#argz_005fappend)
* [`argz_count`](#argz_005fcount)
* [`argz_create`](#argz_005fcreate)
* [`argz_create_sep`](#argz_005fcreate_005fsep)
* [`argz_delete`](#argz_005fdelete)
* [`argz_extract`](#argz_005fextract)
* [`argz_insert`](#argz_005finsert)
* [`argz_next`](#argz_005fnext)
* [`argz_replace`](#argz_005freplace)
* [`argz_stringify`](#argz_005fstringify)

---

Next: [`argz_add_sep`](#argz_005fadd_005fsep), Up: [Glibc `<argz.h>`](#Glibc-argz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.4.1 `argz_add` [¶](#argz_005fadd-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-argz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Argz-Functions.html>,
* [man argz\_add](https://www.kernel.org/doc/man-pages/online/pages/man3/argz_add.3.html).

Gnulib module: argz

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.
* This function is broken on some platforms:
  Cygwin 1.5.24.

Portability problems not fixed by Gnulib:

---

Next: [`argz_append`](#argz_005fappend), Previous: [`argz_add`](#argz_005fadd), Up: [Glibc `<argz.h>`](#Glibc-argz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.4.2 `argz_add_sep` [¶](#argz_005fadd_005fsep-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-argz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Argz-Functions.html>,
* [man argz\_add\_sep](https://www.kernel.org/doc/man-pages/online/pages/man3/argz_add_sep.3.html).

Gnulib module: argz

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.
* This function is broken on some platforms:
  Cygwin 1.5.24.

Portability problems not fixed by Gnulib:

---

Next: [`argz_count`](#argz_005fcount), Previous: [`argz_add_sep`](#argz_005fadd_005fsep), Up: [Glibc `<argz.h>`](#Glibc-argz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.4.3 `argz_append` [¶](#argz_005fappend-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-argz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Argz-Functions.html>,
* [man argz\_append](https://www.kernel.org/doc/man-pages/online/pages/man3/argz_append.3.html).

Gnulib module: argz

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.
* This function is broken on some platforms:
  Cygwin 1.5.24.

Portability problems not fixed by Gnulib:

---

Next: [`argz_create`](#argz_005fcreate), Previous: [`argz_append`](#argz_005fappend), Up: [Glibc `<argz.h>`](#Glibc-argz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.4.4 `argz_count` [¶](#argz_005fcount-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-argz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Argz-Functions.html>,
* [man argz\_count](https://www.kernel.org/doc/man-pages/online/pages/man3/argz_count.3.html).

Gnulib module: argz

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.
* This function is broken on some platforms:
  Cygwin 1.5.24.

Portability problems not fixed by Gnulib:

---

Next: [`argz_create_sep`](#argz_005fcreate_005fsep), Previous: [`argz_count`](#argz_005fcount), Up: [Glibc `<argz.h>`](#Glibc-argz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.4.5 `argz_create` [¶](#argz_005fcreate-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-argz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Argz-Functions.html>,
* [man argz\_create](https://www.kernel.org/doc/man-pages/online/pages/man3/argz_create.3.html).

Gnulib module: argz

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.
* This function is broken on some platforms:
  Cygwin 1.5.24.

Portability problems not fixed by Gnulib:

---

Next: [`argz_delete`](#argz_005fdelete), Previous: [`argz_create`](#argz_005fcreate), Up: [Glibc `<argz.h>`](#Glibc-argz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.4.6 `argz_create_sep` [¶](#argz_005fcreate_005fsep-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-argz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Argz-Functions.html>,
* [man argz\_create\_sep](https://www.kernel.org/doc/man-pages/online/pages/man3/argz_create_sep.3.html).

Gnulib module: argz

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.
* This function is broken on some platforms:
  Cygwin 1.5.24.

Portability problems not fixed by Gnulib:

---

Next: [`argz_extract`](#argz_005fextract), Previous: [`argz_create_sep`](#argz_005fcreate_005fsep), Up: [Glibc `<argz.h>`](#Glibc-argz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.4.7 `argz_delete` [¶](#argz_005fdelete-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-argz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Argz-Functions.html>,
* [man argz\_delete](https://www.kernel.org/doc/man-pages/online/pages/man3/argz_delete.3.html).

Gnulib module: argz

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.
* This function is broken on some platforms:
  Cygwin 1.5.24.

Portability problems not fixed by Gnulib:

---

Next: [`argz_insert`](#argz_005finsert), Previous: [`argz_delete`](#argz_005fdelete), Up: [Glibc `<argz.h>`](#Glibc-argz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.4.8 `argz_extract` [¶](#argz_005fextract-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-argz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Argz-Functions.html>,
* [man argz\_extract](https://www.kernel.org/doc/man-pages/online/pages/man3/argz_extract.3.html).

Gnulib module: argz

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.
* This function is broken on some platforms:
  Cygwin 1.5.24.

Portability problems not fixed by Gnulib:

---

Next: [`argz_next`](#argz_005fnext), Previous: [`argz_extract`](#argz_005fextract), Up: [Glibc `<argz.h>`](#Glibc-argz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.4.9 `argz_insert` [¶](#argz_005finsert-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-argz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Argz-Functions.html>,
* [man argz\_insert](https://www.kernel.org/doc/man-pages/online/pages/man3/argz_insert.3.html).

Gnulib module: argz

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.
* This function is broken on some platforms:
  Cygwin 1.5.24.

Portability problems not fixed by Gnulib:

---

Next: [`argz_replace`](#argz_005freplace), Previous: [`argz_insert`](#argz_005finsert), Up: [Glibc `<argz.h>`](#Glibc-argz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.4.10 `argz_next` [¶](#argz_005fnext-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-argz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Argz-Functions.html>,
* [man argz\_next](https://www.kernel.org/doc/man-pages/online/pages/man3/argz_next.3.html).

Gnulib module: argz

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.
* This function is broken on some platforms:
  Cygwin 1.5.24.

Portability problems not fixed by Gnulib:

---

Next: [`argz_stringify`](#argz_005fstringify), Previous: [`argz_next`](#argz_005fnext), Up: [Glibc `<argz.h>`](#Glibc-argz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.4.11 `argz_replace` [¶](#argz_005freplace-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-argz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Argz-Functions.html>,
* [man argz\_replace](https://www.kernel.org/doc/man-pages/online/pages/man3/argz_replace.3.html).

Gnulib module: argz

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.
* This function is broken on some platforms:
  Cygwin 1.5.24.

Portability problems not fixed by Gnulib:

---

Previous: [`argz_replace`](#argz_005freplace), Up: [Glibc `<argz.h>`](#Glibc-argz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.4.12 `argz_stringify` [¶](#argz_005fstringify-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-argz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Argz-Functions.html>,
* [man argz\_stringify](https://www.kernel.org/doc/man-pages/online/pages/man3/argz_stringify.3.html).

Gnulib module: argz

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.
* This function is broken on some platforms:
  Cygwin 1.5.24.

Portability problems not fixed by Gnulib:

---

Next: [Glibc Extensions to `<arpa/nameser.h>`](#Glibc-arpa_002fnameser_002eh), Previous: [Glibc `<argz.h>`](#Glibc-argz_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.5 Glibc Extensions to `<arpa/inet.h>` [¶](#Glibc-Extensions-to-_003carpa_002finet_002eh_003e)

* [`inet_aton`](#inet_005faton)
* [`inet_lnaof`](#inet_005flnaof)
* [`inet_makeaddr`](#inet_005fmakeaddr)
* [`inet_net_ntop`](#inet_005fnet_005fntop)
* [`inet_net_pton`](#inet_005fnet_005fpton)
* [`inet_netof`](#inet_005fnetof)
* [`inet_network`](#inet_005fnetwork)
* [`inet_nsap_addr`](#inet_005fnsap_005faddr)
* [`inet_nsap_ntoa`](#inet_005fnsap_005fntoa)

---

Next: [`inet_lnaof`](#inet_005flnaof), Up: [Glibc Extensions to `<arpa/inet.h>`](#Glibc-arpa_002finet_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.5.1 `inet_aton` [¶](#inet_005faton-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-inet-aton-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Host-Address-Functions.html>,
* [man inet\_aton](https://www.kernel.org/doc/man-pages/online/pages/man3/inet_aton.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`inet_makeaddr`](#inet_005fmakeaddr), Previous: [`inet_aton`](#inet_005faton), Up: [Glibc Extensions to `<arpa/inet.h>`](#Glibc-arpa_002finet_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.5.2 `inet_lnaof` [¶](#inet_005flnaof-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Host-Address-Functions.html>,
* [man inet\_lnaof](https://www.kernel.org/doc/man-pages/online/pages/man3/inet_lnaof.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 4.4.

---

Next: [`inet_net_ntop`](#inet_005fnet_005fntop), Previous: [`inet_lnaof`](#inet_005flnaof), Up: [Glibc Extensions to `<arpa/inet.h>`](#Glibc-arpa_002finet_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.5.3 `inet_makeaddr` [¶](#inet_005fmakeaddr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Host-Address-Functions.html>,
* [man inet\_makeaddr](https://www.kernel.org/doc/man-pages/online/pages/man3/inet_makeaddr.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.4.

---

Next: [`inet_net_pton`](#inet_005fnet_005fpton), Previous: [`inet_makeaddr`](#inet_005fmakeaddr), Up: [Glibc Extensions to `<arpa/inet.h>`](#Glibc-arpa_002finet_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.5.4 `inet_net_ntop` [¶](#inet_005fnet_005fntop-1)

Documentation:  
 [man inet\_net\_ntop](https://www.kernel.org/doc/man-pages/online/pages/man3/inet_net_ntop.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet_netof`](#inet_005fnetof), Previous: [`inet_net_ntop`](#inet_005fnet_005fntop), Up: [Glibc Extensions to `<arpa/inet.h>`](#Glibc-arpa_002finet_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.5.5 `inet_net_pton` [¶](#inet_005fnet_005fpton-1)

Documentation:  
 [man inet\_net\_pton](https://www.kernel.org/doc/man-pages/online/pages/man3/inet_net_pton.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet_network`](#inet_005fnetwork), Previous: [`inet_net_pton`](#inet_005fnet_005fpton), Up: [Glibc Extensions to `<arpa/inet.h>`](#Glibc-arpa_002finet_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.5.6 `inet_netof` [¶](#inet_005fnetof-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Host-Address-Functions.html>,
* [man inet\_netof](https://www.kernel.org/doc/man-pages/online/pages/man3/inet_netof.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.4.

---

Next: [`inet_nsap_addr`](#inet_005fnsap_005faddr), Previous: [`inet_netof`](#inet_005fnetof), Up: [Glibc Extensions to `<arpa/inet.h>`](#Glibc-arpa_002finet_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.5.7 `inet_network` [¶](#inet_005fnetwork-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Host-Address-Functions.html>,
* [man inet\_network](https://www.kernel.org/doc/man-pages/online/pages/man3/inet_network.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.

---

Next: [`inet_nsap_ntoa`](#inet_005fnsap_005fntoa), Previous: [`inet_network`](#inet_005fnetwork), Up: [Glibc Extensions to `<arpa/inet.h>`](#Glibc-arpa_002finet_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.5.8 `inet_nsap_addr` [¶](#inet_005fnsap_005faddr-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, HP-UX 11.00, Cygwin 2.9, mingw, MSVC 14.

---

Previous: [`inet_nsap_addr`](#inet_005fnsap_005faddr), Up: [Glibc Extensions to `<arpa/inet.h>`](#Glibc-arpa_002finet_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.5.9 `inet_nsap_ntoa` [¶](#inet_005fnsap_005fntoa-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, HP-UX 11.00, Cygwin 2.9, mingw, MSVC 14.

---

Next: [Glibc `<byteswap.h>`](#Glibc-byteswap_002eh), Previous: [Glibc Extensions to `<arpa/inet.h>`](#Glibc-arpa_002finet_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.6 Glibc Extensions to `<arpa/nameser.h>` [¶](#Glibc-Extensions-to-_003carpa_002fnameser_002eh_003e)

* [`ns_name_compress`](#ns_005fname_005fcompress)
* [`ns_name_ntop`](#ns_005fname_005fntop)
* [`ns_name_pack`](#ns_005fname_005fpack)
* [`ns_name_pton`](#ns_005fname_005fpton)
* [`ns_name_skip`](#ns_005fname_005fskip)
* [`ns_name_uncompress`](#ns_005fname_005funcompress)
* [`ns_name_unpack`](#ns_005fname_005funpack)

---

Next: [`ns_name_ntop`](#ns_005fname_005fntop), Up: [Glibc Extensions to `<arpa/nameser.h>`](#Glibc-arpa_002fnameser_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.6.1 `ns_name_compress` [¶](#ns_005fname_005fcompress-1)

Documentation:  
 <https://web.deu.edu.tr/doc/oreily/networking/dnsbind/ch14_02.htm>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.33, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, HP-UX 11.11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ns_name_pack`](#ns_005fname_005fpack), Previous: [`ns_name_compress`](#ns_005fname_005fcompress), Up: [Glibc Extensions to `<arpa/nameser.h>`](#Glibc-arpa_002fnameser_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.6.2 `ns_name_ntop` [¶](#ns_005fname_005fntop-1)

Documentation: —

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.33, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, HP-UX 11.11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ns_name_pton`](#ns_005fname_005fpton), Previous: [`ns_name_ntop`](#ns_005fname_005fntop), Up: [Glibc Extensions to `<arpa/nameser.h>`](#Glibc-arpa_002fnameser_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.6.3 `ns_name_pack` [¶](#ns_005fname_005fpack-1)

Documentation: —

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.33, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, HP-UX 11.11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ns_name_skip`](#ns_005fname_005fskip), Previous: [`ns_name_pack`](#ns_005fname_005fpack), Up: [Glibc Extensions to `<arpa/nameser.h>`](#Glibc-arpa_002fnameser_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.6.4 `ns_name_pton` [¶](#ns_005fname_005fpton-1)

Documentation: —

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.33, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, HP-UX 11.11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ns_name_uncompress`](#ns_005fname_005funcompress), Previous: [`ns_name_pton`](#ns_005fname_005fpton), Up: [Glibc Extensions to `<arpa/nameser.h>`](#Glibc-arpa_002fnameser_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.6.5 `ns_name_skip` [¶](#ns_005fname_005fskip-1)

Documentation:  
 <https://web.deu.edu.tr/doc/oreily/networking/dnsbind/ch14_02.htm>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.33, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, HP-UX 11.11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ns_name_unpack`](#ns_005fname_005funpack), Previous: [`ns_name_skip`](#ns_005fname_005fskip), Up: [Glibc Extensions to `<arpa/nameser.h>`](#Glibc-arpa_002fnameser_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.6.6 `ns_name_uncompress` [¶](#ns_005fname_005funcompress-1)

Documentation:  
 <https://web.deu.edu.tr/doc/oreily/networking/dnsbind/ch14_02.htm>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.33, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, HP-UX 11.11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`ns_name_uncompress`](#ns_005fname_005funcompress), Up: [Glibc Extensions to `<arpa/nameser.h>`](#Glibc-arpa_002fnameser_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.6.7 `ns_name_unpack` [¶](#ns_005fname_005funpack-1)

Documentation: —

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.33, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, HP-UX 11.11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

https://man7.org/linux/man-pages/man3/libsres.3.html
https://www.kernel.org/doc/man-pages/online/pages/man3/

---

Next: [Glibc Extensions to `<complex.h>`](#Glibc-complex_002eh), Previous: [Glibc Extensions to `<arpa/nameser.h>`](#Glibc-arpa_002fnameser_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.7 Glibc `<byteswap.h>` [¶](#Glibc-_003cbyteswap_002eh_003e)

* [`bswap_16`](#bswap_005f16)
* [`bswap_32`](#bswap_005f32)
* [`bswap_64`](#bswap_005f64)

---

Next: [`bswap_32`](#bswap_005f32), Up: [Glibc `<byteswap.h>`](#Glibc-byteswap_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.7.1 `bswap_16` [¶](#bswap_005f16-1)

Documentation:  
 [man bswap\_16](https://www.kernel.org/doc/man-pages/online/pages/man3/bswap_16.3.html)

Gnulib module: byteswap

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`bswap_64`](#bswap_005f64), Previous: [`bswap_16`](#bswap_005f16), Up: [Glibc `<byteswap.h>`](#Glibc-byteswap_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.7.2 `bswap_32` [¶](#bswap_005f32-1)

Documentation:  
 [man bswap\_32](https://www.kernel.org/doc/man-pages/online/pages/man3/bswap_32.3.html)

Gnulib module: byteswap

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`bswap_32`](#bswap_005f32), Up: [Glibc `<byteswap.h>`](#Glibc-byteswap_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.7.3 `bswap_64` [¶](#bswap_005f64-1)

Documentation:  
 [man bswap\_64](https://www.kernel.org/doc/man-pages/online/pages/man3/bswap_64.3.html)

Gnulib module: byteswap

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc Extensions to `<ctype.h>`](#Glibc-ctype_002eh), Previous: [Glibc `<byteswap.h>`](#Glibc-byteswap_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.8 Glibc Extensions to `<complex.h>` [¶](#Glibc-Extensions-to-_003ccomplex_002eh_003e)

* [`clog10`](#clog10)
* [`clog10f`](#clog10f)
* [`clog10l`](#clog10l)

---

Next: [`clog10f`](#clog10f), Up: [Glibc Extensions to `<complex.h>`](#Glibc-complex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.8.1 `clog10` [¶](#clog10-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib---clog10.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Exponents-and-Logarithms.html>,
* [man clog10](https://www.kernel.org/doc/man-pages/online/pages/man3/clog10.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 9, Android 9.0.

---

Next: [`clog10l`](#clog10l), Previous: [`clog10`](#clog10), Up: [Glibc Extensions to `<complex.h>`](#Glibc-complex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.8.2 `clog10f` [¶](#clog10f-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib---clog10f.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Exponents-and-Logarithms.html>,
* [man clog10f](https://www.kernel.org/doc/man-pages/online/pages/man3/clog10f.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 9, Android 9.0.

---

Previous: [`clog10f`](#clog10f), Up: [Glibc Extensions to `<complex.h>`](#Glibc-complex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.8.3 `clog10l` [¶](#clog10l-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib---clog10l.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Exponents-and-Logarithms.html>,
* [man clog10l](https://www.kernel.org/doc/man-pages/online/pages/man3/clog10l.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 9, Android 9.0.

---

Next: [Glibc Extensions to `<dirent.h>`](#Glibc-dirent_002eh), Previous: [Glibc Extensions to `<complex.h>`](#Glibc-complex_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.9 Glibc Extensions to `<ctype.h>` [¶](#Glibc-Extensions-to-_003cctype_002eh_003e)

* [`isctype`](#isctype)

---

Up: [Glibc Extensions to `<ctype.h>`](#Glibc-ctype_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.9.1 `isctype` [¶](#isctype-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc Extensions to `<dlfcn.h>`](#Glibc-dlfcn_002eh), Previous: [Glibc Extensions to `<ctype.h>`](#Glibc-ctype_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.10 Glibc Extensions to `<dirent.h>` [¶](#Glibc-Extensions-to-_003cdirent_002eh_003e)

* [`getdirentries`](#getdirentries)
* [`scandirat`](#scandirat)
* [`versionsort`](#versionsort)

---

Next: [`scandirat`](#scandirat), Up: [Glibc Extensions to `<dirent.h>`](#Glibc-dirent_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.10.1 `getdirentries` [¶](#getdirentries-1)

Documentation:  
 [man getdirentries](https://www.kernel.org/doc/man-pages/online/pages/man3/getdirentries.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on huge directories larger than 2 GB. The fix is to use
  the `AC_SYS_LARGEFILE` macro.

---

Next: [`versionsort`](#versionsort), Previous: [`getdirentries`](#getdirentries), Up: [Glibc Extensions to `<dirent.h>`](#Glibc-dirent_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.10.2 `scandirat` [¶](#scandirat-1)

Documentation:  
 [man scandirat](https://www.kernel.org/doc/man-pages/online/pages/man3/scandirat.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.14, macOS 14, FreeBSD 13.0, NetBSD 10.0, OpenBSD 7.5, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 1.7.10, mingw, MSVC 14, Android 6.0.

---

Previous: [`scandirat`](#scandirat), Up: [Glibc Extensions to `<dirent.h>`](#Glibc-dirent_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.10.3 `versionsort` [¶](#versionsort-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Scanning-Directory-Content.html>,
* [man versionsort](https://www.kernel.org/doc/man-pages/online/pages/man3/versionsort.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 13.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<envz.h>`](#Glibc-envz_002eh), Previous: [Glibc Extensions to `<dirent.h>`](#Glibc-dirent_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.11 Glibc Extensions to `<dlfcn.h>` [¶](#Glibc-Extensions-to-_003cdlfcn_002eh_003e)

* [`dladdr1`](#dladdr1)
* [`dlinfo`](#dlinfo)
* [`dlmopen`](#dlmopen)
* [`dlvsym`](#dlvsym)
* [`_dl_find_object`](#g_t_005fdl_005ffind_005fobject)

---

Next: [`dlinfo`](#dlinfo), Up: [Glibc Extensions to `<dlfcn.h>`](#Glibc-dlfcn_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.11.1 `dladdr1` [¶](#dladdr1-1)

Documentation:  
 [man dladdr1](https://www.kernel.org/doc/man-pages/online/pages/man3/dladdr1.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`dlmopen`](#dlmopen), Previous: [`dladdr1`](#dladdr1), Up: [Glibc Extensions to `<dlfcn.h>`](#Glibc-dlfcn_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.11.2 `dlinfo` [¶](#dlinfo-1)

Documentation:  
 [man dlinfo](https://www.kernel.org/doc/man-pages/online/pages/man3/dlinfo.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`dlvsym`](#dlvsym), Previous: [`dlinfo`](#dlinfo), Up: [Glibc Extensions to `<dlfcn.h>`](#Glibc-dlfcn_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.11.3 `dlmopen` [¶](#dlmopen-1)

Documentation:  
 [man dlmopen](https://www.kernel.org/doc/man-pages/online/pages/man3/dlmopen.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`_dl_find_object`](#g_t_005fdl_005ffind_005fobject), Previous: [`dlmopen`](#dlmopen), Up: [Glibc Extensions to `<dlfcn.h>`](#Glibc-dlfcn_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.11.4 `dlvsym` [¶](#dlvsym-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-dlvsym-1.html>

Documentation:  
 [man dlvsym](https://www.kernel.org/doc/man-pages/online/pages/man3/dlvsym.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 6.0.

---

Previous: [`dlvsym`](#dlvsym), Up: [Glibc Extensions to `<dlfcn.h>`](#Glibc-dlfcn_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.11.5 `_dl_find_object` [¶](#g_t_005fdl_005ffind_005fobject-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Dynamic-Linker-Introspection.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.34, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [Glibc `<err.h>`](#Glibc-err_002eh), Previous: [Glibc Extensions to `<dlfcn.h>`](#Glibc-dlfcn_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.12 Glibc `<envz.h>` [¶](#Glibc-_003cenvz_002eh_003e)

* [`envz_add`](#envz_005fadd)
* [`envz_entry`](#envz_005fentry)
* [`envz_get`](#envz_005fget)
* [`envz_merge`](#envz_005fmerge)
* [`envz_remove`](#envz_005fremove)
* [`envz_strip`](#envz_005fstrip)

---

Next: [`envz_entry`](#envz_005fentry), Up: [Glibc `<envz.h>`](#Glibc-envz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.12.1 `envz_add` [¶](#envz_005fadd-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-envz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Envz-Functions.html>,
* [man envz\_add](https://www.kernel.org/doc/man-pages/online/pages/man3/envz_add.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.

---

Next: [`envz_get`](#envz_005fget), Previous: [`envz_add`](#envz_005fadd), Up: [Glibc `<envz.h>`](#Glibc-envz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.12.2 `envz_entry` [¶](#envz_005fentry-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-envz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Envz-Functions.html>,
* [man envz\_entry](https://www.kernel.org/doc/man-pages/online/pages/man3/envz_entry.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.

---

Next: [`envz_merge`](#envz_005fmerge), Previous: [`envz_entry`](#envz_005fentry), Up: [Glibc `<envz.h>`](#Glibc-envz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.12.3 `envz_get` [¶](#envz_005fget-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-envz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Envz-Functions.html>,
* [man envz\_get](https://www.kernel.org/doc/man-pages/online/pages/man3/envz_get.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.

---

Next: [`envz_remove`](#envz_005fremove), Previous: [`envz_get`](#envz_005fget), Up: [Glibc `<envz.h>`](#Glibc-envz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.12.4 `envz_merge` [¶](#envz_005fmerge-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-envz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Envz-Functions.html>,
* [man envz\_merge](https://www.kernel.org/doc/man-pages/online/pages/man3/envz_merge.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.

---

Next: [`envz_strip`](#envz_005fstrip), Previous: [`envz_merge`](#envz_005fmerge), Up: [Glibc `<envz.h>`](#Glibc-envz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.12.5 `envz_remove` [¶](#envz_005fremove-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-envz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Envz-Functions.html>,
* [man envz\_remove](https://www.kernel.org/doc/man-pages/online/pages/man3/envz_remove.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.

---

Previous: [`envz_remove`](#envz_005fremove), Up: [Glibc `<envz.h>`](#Glibc-envz_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.12.6 `envz_strip` [¶](#envz_005fstrip-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-envz-add.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Envz-Functions.html>,
* [man envz\_strip](https://www.kernel.org/doc/man-pages/online/pages/man3/envz_strip.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc Extensions to `<errno.h>`](#Glibc-errno_002eh), Previous: [Glibc `<envz.h>`](#Glibc-envz_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.13 Glibc `<err.h>` [¶](#Glibc-_003cerr_002eh_003e)

* [`err`](#err)
* [`errx`](#errx)
* [`verr`](#verr)
* [`verrx`](#verrx)
* [`vwarn`](#vwarn)
* [`vwarnx`](#vwarnx)
* [`warn`](#warn)
* [`warnx`](#warnx)

---

Next: [`errx`](#errx), Up: [Glibc `<err.h>`](#Glibc-err_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.13.1 `err` [¶](#err-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-err-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>,
* [man err](https://www.kernel.org/doc/man-pages/online/pages/man3/err.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 10, mingw, MSVC 14.

---

Next: [`verr`](#verr), Previous: [`err`](#err), Up: [Glibc `<err.h>`](#Glibc-err_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.13.2 `errx` [¶](#errx-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-errx-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>,
* [man errx](https://www.kernel.org/doc/man-pages/online/pages/man3/errx.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 10, mingw, MSVC 14.

---

Next: [`verrx`](#verrx), Previous: [`errx`](#errx), Up: [Glibc `<err.h>`](#Glibc-err_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.13.3 `verr` [¶](#verr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>,
* [man verr](https://www.kernel.org/doc/man-pages/online/pages/man3/verr.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 10, mingw, MSVC 14.

---

Next: [`vwarn`](#vwarn), Previous: [`verr`](#verr), Up: [Glibc `<err.h>`](#Glibc-err_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.13.4 `verrx` [¶](#verrx-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-verrx-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>,
* [man verrx](https://www.kernel.org/doc/man-pages/online/pages/man3/verrx.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 10, mingw, MSVC 14.

---

Next: [`vwarnx`](#vwarnx), Previous: [`verrx`](#verrx), Up: [Glibc `<err.h>`](#Glibc-err_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.13.5 `vwarn` [¶](#vwarn-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>,
* [man vwarn](https://www.kernel.org/doc/man-pages/online/pages/man3/vwarn.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 10, mingw, MSVC 14.

---

Next: [`warn`](#warn), Previous: [`vwarn`](#vwarn), Up: [Glibc `<err.h>`](#Glibc-err_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.13.6 `vwarnx` [¶](#vwarnx-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>,
* [man vwarnx](https://www.kernel.org/doc/man-pages/online/pages/man3/vwarnx.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 10, mingw, MSVC 14.

---

Next: [`warnx`](#warnx), Previous: [`vwarnx`](#vwarnx), Up: [Glibc `<err.h>`](#Glibc-err_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.13.7 `warn` [¶](#warn-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-warn-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>,
* [man warn](https://www.kernel.org/doc/man-pages/online/pages/man3/warn.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 10, mingw, MSVC 14.

---

Previous: [`warn`](#warn), Up: [Glibc `<err.h>`](#Glibc-err_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.13.8 `warnx` [¶](#warnx-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-warnx-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>,
* [man warnx](https://www.kernel.org/doc/man-pages/online/pages/man3/warnx.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 10, mingw, MSVC 14.

---

Next: [Glibc `<error.h>`](#Glibc-error_002eh), Previous: [Glibc `<err.h>`](#Glibc-err_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.14 Glibc Extensions to `<errno.h>` [¶](#Glibc-Extensions-to-_003cerrno_002eh_003e)

* [`program_invocation_name`](#program_005finvocation_005fname)
* [`program_invocation_short_name`](#program_005finvocation_005fshort_005fname)

---

Next: [`program_invocation_short_name`](#program_005finvocation_005fshort_005fname), Up: [Glibc Extensions to `<errno.h>`](#Glibc-errno_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.14.1 `program_invocation_name` [¶](#program_005finvocation_005fname-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>,
* [man program\_invocation\_name](https://www.kernel.org/doc/man-pages/online/pages/man3/program_invocation_name.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.7, mingw, MSVC 14, Android 9.0.

---

Previous: [`program_invocation_name`](#program_005finvocation_005fname), Up: [Glibc Extensions to `<errno.h>`](#Glibc-errno_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.14.2 `program_invocation_short_name` [¶](#program_005finvocation_005fshort_005fname-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>,
* [man program\_invocation\_short\_name](https://www.kernel.org/doc/man-pages/online/pages/man3/program_invocation_short_name.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.7, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<execinfo.h>`](#Glibc-execinfo_002eh), Previous: [Glibc Extensions to `<errno.h>`](#Glibc-errno_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
