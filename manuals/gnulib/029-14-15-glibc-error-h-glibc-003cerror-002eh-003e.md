### 14.15 Glibc `<error.h>` [¶](#Glibc-_003cerror_002eh_003e)

* [`error`](#error)
* [`error_at_line`](#error_005fat_005fline)
* [`error_message_count`](#error_005fmessage_005fcount)
* [`error_one_per_line`](#error_005fone_005fper_005fline)
* [`error_print_progname`](#error_005fprint_005fprogname)

---

Next: [`error_at_line`](#error_005fat_005fline), Up: [Glibc `<error.h>`](#Glibc-error_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.15.1 `error` [¶](#error-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-error-n.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>,
* [man error](https://www.kernel.org/doc/man-pages/online/pages/man3/error.3.html).

Gnulib module: error

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.9, mingw, MSVC 14, Android 5.1.
* This function outputs an extra space if `error_print_progname` is set,
  on some platforms:
  Android 11.

Portability problems not fixed by Gnulib:

---

Next: [`error_message_count`](#error_005fmessage_005fcount), Previous: [`error`](#error), Up: [Glibc `<error.h>`](#Glibc-error_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.15.2 `error_at_line` [¶](#error_005fat_005fline-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>,
* [man error\_at\_line](https://www.kernel.org/doc/man-pages/online/pages/man3/error_at_line.3.html).

Gnulib module: error

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.9, mingw, MSVC 14, Android 5.1.

Portability problems not fixed by Gnulib:

---

Next: [`error_one_per_line`](#error_005fone_005fper_005fline), Previous: [`error_at_line`](#error_005fat_005fline), Up: [Glibc `<error.h>`](#Glibc-error_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.15.3 `error_message_count` [¶](#error_005fmessage_005fcount-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>,
* [man error\_message\_count](https://www.kernel.org/doc/man-pages/online/pages/man3/error_message_count.3.html).

Gnulib module: error

Portability problems fixed by Gnulib:

* This variable is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.9, mingw, MSVC 14, Android 5.1.

Portability problems not fixed by Gnulib:

---

Next: [`error_print_progname`](#error_005fprint_005fprogname), Previous: [`error_message_count`](#error_005fmessage_005fcount), Up: [Glibc `<error.h>`](#Glibc-error_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.15.4 `error_one_per_line` [¶](#error_005fone_005fper_005fline-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Error-Messages.html>,
* [man error\_one\_per\_line](https://www.kernel.org/doc/man-pages/online/pages/man3/error_one_per_line.3.html).

Gnulib module: error

Portability problems fixed by Gnulib:

* This variable is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.9, mingw, MSVC 14, Android 5.1.

Portability problems not fixed by Gnulib:

---

Previous: [`error_one_per_line`](#error_005fone_005fper_005fline), Up: [Glibc `<error.h>`](#Glibc-error_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.15.5 `error_print_progname` [¶](#error_005fprint_005fprogname-1)

Documentation:  
 [man error\_print\_progname](https://www.kernel.org/doc/man-pages/online/pages/man3/error_print_progname.3.html)

Gnulib module: error

Portability problems fixed by Gnulib:

* This variable is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.9, mingw, MSVC 14, Android 5.1.

Portability problems not fixed by Gnulib:

---

Next: [Glibc Extensions to `<fcntl.h>`](#Glibc-fcntl_002eh), Previous: [Glibc `<error.h>`](#Glibc-error_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.16 Glibc `<execinfo.h>` [¶](#Glibc-_003cexecinfo_002eh_003e)

* [`backtrace`](#backtrace)
* [`backtrace_symbols`](#backtrace_005fsymbols)
* [`backtrace_symbols_fd`](#backtrace_005fsymbols_005ffd)

---

Next: [`backtrace_symbols`](#backtrace_005fsymbols), Up: [Glibc `<execinfo.h>`](#Glibc-execinfo_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.16.1 `backtrace` [¶](#backtrace-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-backtrace-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Backtraces.html>,
* [man backtrace](https://www.kernel.org/doc/man-pages/online/pages/man3/backtrace.3.html).

Gnulib module: execinfo

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  musl libc, FreeBSD 9.3, NetBSD 6.1, OpenBSD 6.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android API level 32.

Portability problems not fixed by Gnulib:

* The second argument is of type `size_t`, not `int`, on some platforms:
  FreeBSD, NetBSD, OpenBSD.
* The return value may be `(size_t)(-1)` instead of 0 on some platforms:
  FreeBSD 12.4/i386.
* The returned stack trace is empty if it would not entirely fit into the
  provided buffer on some platforms:
  FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9.
* The returned stack trace is always empty on some platforms:
  FreeBSD 11.0/i386, FreeBSD 12.2/sparc64.
* On platforms where the function is missing,
  the Gnulib substitute implementation is just a stub, and does nothing.

---

Next: [`backtrace_symbols_fd`](#backtrace_005fsymbols_005ffd), Previous: [`backtrace`](#backtrace), Up: [Glibc `<execinfo.h>`](#Glibc-execinfo_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.16.2 `backtrace_symbols` [¶](#backtrace_005fsymbols-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-backtrace-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Backtraces.html>,
* [man backtrace\_symbols](https://www.kernel.org/doc/man-pages/online/pages/man3/backtrace_symbols.3.html).

Gnulib module: execinfo

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  musl libc, FreeBSD 9.3, NetBSD 6.1, OpenBSD 6.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android API level 32.

Portability problems not fixed by Gnulib:

* The second argument is of type `size_t`, not `int`, on some platforms:
  FreeBSD, NetBSD, OpenBSD.
* On platforms where the function is missing,
  the Gnulib substitute implementation is just a stub, and does nothing.

---

Previous: [`backtrace_symbols`](#backtrace_005fsymbols), Up: [Glibc `<execinfo.h>`](#Glibc-execinfo_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.16.3 `backtrace_symbols_fd` [¶](#backtrace_005fsymbols_005ffd-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-backtrace-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Backtraces.html>,
* [man backtrace\_symbols\_fd](https://www.kernel.org/doc/man-pages/online/pages/man3/backtrace_symbols_fd.3.html).

Gnulib module: execinfo

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  musl libc, FreeBSD 9.3, NetBSD 6.1, OpenBSD 6.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android API level 32.

Portability problems not fixed by Gnulib:

* The second argument is of type `size_t`, not `int`, on some platforms:
  FreeBSD, NetBSD, OpenBSD.
* The strings printed by this function not the same as the strings returned by
  `backtrace_symbols` on some platforms:
  glibc 2.39.
* On platforms where the function is missing,
  the Gnulib substitute implementation is just a stub, and does nothing.

---

Next: [Glibc Extensions to `<fenv.h>`](#Glibc-fenv_002eh), Previous: [Glibc `<execinfo.h>`](#Glibc-execinfo_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.17 Glibc Extensions to `<fcntl.h>` [¶](#Glibc-Extensions-to-_003cfcntl_002eh_003e)

* [`fallocate`](#fallocate)
* [`name_to_handle_at`](#name_005fto_005fhandle_005fat)
* [`readahead`](#readahead)
* [`open_by_handle_at`](#open_005fby_005fhandle_005fat)
* [`openat2`](#openat2)
* [`sync_file_range`](#sync_005ffile_005frange)

---

Next: [`name_to_handle_at`](#name_005fto_005fhandle_005fat), Up: [Glibc Extensions to `<fcntl.h>`](#Glibc-fcntl_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.17.1 `fallocate` [¶](#fallocate-1)

Documentation:  
 [man fallocate](https://www.kernel.org/doc/man-pages/online/pages/man2/fallocate.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 3.4.x, mingw, MSVC 14, Android 4.4.
* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly across the entire data range of files larger than 2 GB.
  The fix is to use the `AC_SYS_LARGEFILE` macro.

---

Next: [`readahead`](#readahead), Previous: [`fallocate`](#fallocate), Up: [Glibc Extensions to `<fcntl.h>`](#Glibc-fcntl_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.17.2 `name_to_handle_at` [¶](#name_005fto_005fhandle_005fat-1)

Documentation:  
 [man name\_to\_handle\_at](https://www.kernel.org/doc/man-pages/online/pages/man2/name_to_handle_at.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on all non-glibc platforms:
  glibc 2.13, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* When `AT_EMPTY_PATH` is used,
  this function does not allow the file name to be a null pointer:
  Linux kernel 6.10.

---

Next: [`open_by_handle_at`](#open_005fby_005fhandle_005fat), Previous: [`name_to_handle_at`](#name_005fto_005fhandle_005fat), Up: [Glibc Extensions to `<fcntl.h>`](#Glibc-fcntl_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.17.3 `readahead` [¶](#readahead-1)

Documentation:  
 [man readahead](https://www.kernel.org/doc/man-pages/online/pages/man2/readahead.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 4.0.4.

---

Next: [`openat2`](#openat2), Previous: [`readahead`](#readahead), Up: [Glibc Extensions to `<fcntl.h>`](#Glibc-fcntl_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.17.4 `open_by_handle_at` [¶](#open_005fby_005fhandle_005fat-1)

Documentation:  
 [man open\_by\_handle\_at](https://www.kernel.org/doc/man-pages/online/pages/man2/open_by_handle_at.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on all non-glibc platforms:
  glibc 2.13, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`sync_file_range`](#sync_005ffile_005frange), Previous: [`open_by_handle_at`](#open_005fby_005fhandle_005fat), Up: [Glibc Extensions to `<fcntl.h>`](#Glibc-fcntl_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.17.5 `openat2` [¶](#openat2-1)

Documentation:  
 [man openat2](https://www.kernel.org/doc/man-pages/online/pages/man2/openat2.2.html)

Gnulib module: openat2

Portability problems fixed by Gnulib:

* This function is missing on most platforms:
  glibc 2.42 on Linux, glibc on non-Linux, all non-glibc platforms.

Portability problems not fixed by Gnulib:

* On platforms that do not natively support `openat2` functionality,
  the Gnulib emulation might not act atomically.

---

Previous: [`openat2`](#openat2), Up: [Glibc Extensions to `<fcntl.h>`](#Glibc-fcntl_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.17.6 `sync_file_range` [¶](#sync_005ffile_005frange-1)

Documentation:  
 [man sync\_file\_range](https://www.kernel.org/doc/man-pages/online/pages/man2/sync_file_range.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.5, GNU/Hurd, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 4.3.

---

Next: [Glibc Extensions to `<fmtmsg.h>`](#Glibc-fmtmsg_002eh), Previous: [Glibc Extensions to `<fcntl.h>`](#Glibc-fcntl_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.18 Glibc Extensions to `<fenv.h>` [¶](#Glibc-Extensions-to-_003cfenv_002eh_003e)

* [`fedisableexcept`](#fedisableexcept)
* [`feenableexcept`](#feenableexcept)
* [`fegetexcept`](#fegetexcept)

---

Next: [`feenableexcept`](#feenableexcept), Up: [Glibc Extensions to `<fenv.h>`](#Glibc-fenv_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.18.1 `fedisableexcept` [¶](#fedisableexcept-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-fedisableexcept.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Control-Functions.html>,
* [man fedisableexcept](https://www.kernel.org/doc/man-pages/online/pages/man3/fedisableexcept.3.html).

Gnulib module: fenv-exceptions-trapping

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, musl libc, FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.7, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [`fegetexcept`](#fegetexcept), Previous: [`fedisableexcept`](#fedisableexcept), Up: [Glibc Extensions to `<fenv.h>`](#Glibc-fenv_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.18.2 `feenableexcept` [¶](#feenableexcept-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-feenableexcept.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Control-Functions.html>,
* [man feenableexcept](https://www.kernel.org/doc/man-pages/online/pages/man3/feenableexcept.3.html).

Gnulib module: fenv-exceptions-trapping

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, musl libc, FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.7, mingw, MSVC 14, Android 4.4.
* This function does not detect failures on
  glibc 2.19/aarch64, FreeBSD 12.2/arm, FreeBSD 12.2/arm64,
  NetBSD 10.0/arm64.
* This function is broken on mingw 14.0.0.

Portability problems not fixed by Gnulib:

* The resulting signal is `SIGILL` instead of `SIGFPE`
  on some platforms:
  Linux/hppa, macOS 14/arm64.

---

Previous: [`feenableexcept`](#feenableexcept), Up: [Glibc Extensions to `<fenv.h>`](#Glibc-fenv_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.18.3 `fegetexcept` [¶](#fegetexcept-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-fegetexcept.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Control-Functions.html>,
* [man fegetexcept](https://www.kernel.org/doc/man-pages/online/pages/man3/fegetexcept.3.html).

Gnulib module: fenv-exceptions-trapping

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  Mac OS X 10.4, musl libc, FreeBSD 14.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.7, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [Glibc `<fstab.h>`](#Glibc-fstab_002eh), Previous: [Glibc Extensions to `<fenv.h>`](#Glibc-fenv_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.19 Glibc Extensions to `<fmtmsg.h>` [¶](#Glibc-Extensions-to-_003cfmtmsg_002eh_003e)

* [`addseverity`](#addseverity)

---

Up: [Glibc Extensions to `<fmtmsg.h>`](#Glibc-fmtmsg_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.19.1 `addseverity` [¶](#addseverity-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Adding-Severity-Classes.html>,
* [man addseverity](https://www.kernel.org/doc/man-pages/online/pages/man3/addseverity.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<fts.h>`](#Glibc-fts_002eh), Previous: [Glibc Extensions to `<fmtmsg.h>`](#Glibc-fmtmsg_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.20 Glibc `<fstab.h>` [¶](#Glibc-_003cfstab_002eh_003e)

* [`endfsent`](#endfsent)
* [`getfsent`](#getfsent)
* [`getfsfile`](#getfsfile)
* [`getfsspec`](#getfsspec)
* [`setfsent`](#setfsent)

---

Next: [`getfsent`](#getfsent), Up: [Glibc `<fstab.h>`](#Glibc-fstab_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.20.1 `endfsent` [¶](#endfsent-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/fstab.html>,
* [man endfsent](https://www.kernel.org/doc/man-pages/online/pages/man3/endfsent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getfsfile`](#getfsfile), Previous: [`endfsent`](#endfsent), Up: [Glibc `<fstab.h>`](#Glibc-fstab_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.20.2 `getfsent` [¶](#getfsent-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/fstab.html>,
* [man getfsent](https://www.kernel.org/doc/man-pages/online/pages/man3/getfsent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getfsspec`](#getfsspec), Previous: [`getfsent`](#getfsent), Up: [Glibc `<fstab.h>`](#Glibc-fstab_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.20.3 `getfsfile` [¶](#getfsfile-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/fstab.html>,
* [man getfsfile](https://www.kernel.org/doc/man-pages/online/pages/man3/getfsfile.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`setfsent`](#setfsent), Previous: [`getfsfile`](#getfsfile), Up: [Glibc `<fstab.h>`](#Glibc-fstab_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.20.4 `getfsspec` [¶](#getfsspec-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/fstab.html>,
* [man getfsspec](https://www.kernel.org/doc/man-pages/online/pages/man3/getfsspec.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`getfsspec`](#getfsspec), Up: [Glibc `<fstab.h>`](#Glibc-fstab_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.20.5 `setfsent` [¶](#setfsent-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/fstab.html>,
* [man setfsent](https://www.kernel.org/doc/man-pages/online/pages/man3/setfsent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<getopt.h>`](#Glibc-getopt_002eh), Previous: [Glibc `<fstab.h>`](#Glibc-fstab_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.21 Glibc `<fts.h>` [¶](#Glibc-_003cfts_002eh_003e)

* [`fts_children`](#fts_005fchildren)
* [`fts_close`](#fts_005fclose)
* [`fts_open`](#fts_005fopen)
* [`fts_read`](#fts_005fread)
* [`fts_set`](#fts_005fset)

---

Next: [`fts_close`](#fts_005fclose), Up: [Glibc `<fts.h>`](#Glibc-fts_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.21.1 `fts_children` [¶](#fts_005fchildren-1)

Documentation:  
 [man fts\_children](https://www.kernel.org/doc/man-pages/online/pages/man3/fts_children.3.html)

Gnulib module: fts

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 11.3, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  correctly report the size of files or block devices larger than 2 GB and
  may not work correctly on huge directories larger than 2 GB. Also, on
  platforms where `ino_t` is a 32-bit type, this function may report
  inode numbers incorrectly. The fix is to use the `AC_SYS_LARGEFILE`
  macro (only on Mac OS X systems).

---

Next: [`fts_open`](#fts_005fopen), Previous: [`fts_children`](#fts_005fchildren), Up: [Glibc `<fts.h>`](#Glibc-fts_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.21.2 `fts_close` [¶](#fts_005fclose-1)

Documentation:  
 [man fts\_close](https://www.kernel.org/doc/man-pages/online/pages/man3/fts_close.3.html)

Gnulib module: fts

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 11.3, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`fts_read`](#fts_005fread), Previous: [`fts_close`](#fts_005fclose), Up: [Glibc `<fts.h>`](#Glibc-fts_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.21.3 `fts_open` [¶](#fts_005fopen-1)

Documentation:  
 [man fts\_open](https://www.kernel.org/doc/man-pages/online/pages/man3/fts_open.3.html)

Gnulib module: fts

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 11.3, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`fts_set`](#fts_005fset), Previous: [`fts_open`](#fts_005fopen), Up: [Glibc `<fts.h>`](#Glibc-fts_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.21.4 `fts_read` [¶](#fts_005fread-1)

Documentation:  
 [man fts\_read](https://www.kernel.org/doc/man-pages/online/pages/man3/fts_read.3.html)

Gnulib module: fts

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 11.3, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  correctly report the size of files or block devices larger than 2 GB and
  may not work correctly on huge directories larger than 2 GB. Also, on
  platforms where `ino_t` is a 32-bit type, this function may report
  inode numbers incorrectly. The fix is to use the `AC_SYS_LARGEFILE`
  macro (only on Mac OS X systems).

---

Previous: [`fts_read`](#fts_005fread), Up: [Glibc `<fts.h>`](#Glibc-fts_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.21.5 `fts_set` [¶](#fts_005fset-1)

Documentation:  
 [man fts\_set](https://www.kernel.org/doc/man-pages/online/pages/man3/fts_set.3.html)

Gnulib module: fts

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 11.3, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [Glibc Extensions to `<glob.h>`](#Glibc-glob_002eh), Previous: [Glibc `<fts.h>`](#Glibc-fts_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.22 Glibc `<getopt.h>` [¶](#Glibc-_003cgetopt_002eh_003e)

* [`getopt_long`](#getopt_005flong)
* [`getopt_long_only`](#getopt_005flong_005fonly)

---

Next: [`getopt_long_only`](#getopt_005flong_005fonly), Up: [Glibc `<getopt.h>`](#Glibc-getopt_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.22.1 `getopt_long` [¶](#getopt_005flong-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getopt-long-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Getopt-Long-Options.html>,
* [man getopt\_long](https://www.kernel.org/doc/man-pages/online/pages/man3/getopt_long.3.html).

Gnulib module: getopt-gnu

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, MSVC 14.
* The function `getopt_long` does not obey the combination of
  ‘`+`’ and ‘`:`’ flags in the options string on some platforms:
  glibc 2.11.
* The use of ‘`W;`’ in the optstring argument to does not always
  allow `-W foo` to behave synonymously with `--foo`:
  glibc 2.11.
* The function `getopt_long` does not support the ‘`+`’ flag in
  the options string on some platforms:
  macOS 14, AIX 5.2, Solaris 10.
* The value of `optind` after a missing required argument is wrong
  on some platforms:
  macOS 14.
* The function `getopt_long` does not obey the ‘`-`’ flag in the
  options string when `POSIXLY_CORRECT` is set on some platforms:
  Cygwin 1.7.0.
* Some implementations fail to reset state, including re-checking
  `POSIXLY_CORRECT`, when `optind` is set to ‘`0`’:
  NetBSD, Cygwin 1.7.0.
* The function `getopt_long` does not support options with optional
  arguments on some platforms:
  macOS 14, OpenBSD 4.0, AIX 5.2, Solaris 11 2010-11, Cygwin 1.5.x.
* This function crashes if the option string includes `W;` but
  there are no long options, on some platforms:
  glibc 2.14.

Portability problems not fixed by Gnulib:

Gnulib provides also a module `options`, that
fixes the following shortcomings of the `getopt_long` API.

These shortcomings are best illustrated with an example:

```
static struct option const long_options[] =
{
  { "width", required_argument, NULL, 'w' },
  { "help", no_argument, &show_help, 1 },
  { "version", no_argument, &show_version, 1 },
  { NULL, 0, NULL, 0 }
};

while ((optchar = getopt_long (argc, argv, "w:xhV", long_options, NULL))
       != -1)
  switch (optchar)
    {
    case '\0':           /* Long option with flag != NULL.  */
      break;
    case 'w':
      set_width (optarg);
      break;
    case 'x':
      do_x = true;
      break;
    case 'h':
      show_help = 1;     /* Action code duplication!  */
      break;
    case 'V':
      show_version = 1;  /* Action code duplication!  */
      break;
    default:
      usage (EXIT_FAILURE);
    }
```

* The information whether an option takes a required vs. optional argument
  needs to be specified twice:
  in the `option[]` array for the long option
  and in the string argument for the short option.
  It is too easy to forget to
  add the `":"` or `"::"` part in the string argument
  and thus get inconsistent behaviour
  between the long option and the short option.
* This information needs to be specified twice, but in different ways:

  | In the array | In the string |
  | --- | --- |
  | `no_argument` | `""` |
  | `required_argument` | `":"` |
  | `optional_argument` | `"::"` |
* For an action that merely sets an `int`-typed variable to a value,
  you can specify this action in the `options[]` array,
  and thus omit the handling in the `switch` statement.
  But this works only for options that are
  long options without a corresponding short option.
  As soon as the option has a corresponding short option,
  you do need to handle it in the `switch` statement.
  Here again, there is the opportunity for
  inconsistent behaviour between the long option and the short option.
* The `val` field in a `struct option` has different meanings,
  depending on another field:
  If the `flag` field is non-NULL,
  `val` is a value to be stored in a variable.
  If the `flag` field is NULL,
  `val` is a key to be returned from `getopt_long`
  and subject to the `switch` statement.
* The handling of non-option arguments is specified by
  prepending a certain character (‘`+`’ or ‘`-`’) to the string argument.
  This is not one of the usual ways to specify things in an API.
  The conventional way in an API is
  an argument of `enum` type, or a flags word.
* The handling of errors consists of two independent flags,
  and each of the flags has to be specified in a different way:
  one flag is specified by
  prepending a certain character (‘`:`’) to the string argument;
  the other flag is specified through the global variable `opterr`.
* The `struct option` is a misnomer:
  It cannot encode short options.
  Therefore, it would have better been called `struct long_option`.
* The `getopt_long` function is expected to
  receive the same arguments in each call, in the `while` loop.
  The effects are undefined if you don’t follow this (unwritten!) constraint.
* The fifth argument to `getopt_long`, indexptr, is redundant, because
  when the `flag` is non-NULL,
  the `switch` statement does not need to handle the option,
  and when the `flag` is NULL,
  `getopt_long` returns the value of `val`,
  as a way to identify which option was seen.

---

Previous: [`getopt_long`](#getopt_005flong), Up: [Glibc `<getopt.h>`](#Glibc-getopt_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.22.2 `getopt_long_only` [¶](#getopt_005flong_005fonly-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getopt-long-only-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Getopt-Long-Options.html>,
* [man getopt\_long\_only](https://www.kernel.org/doc/man-pages/online/pages/man3/getopt_long_only.3.html).

Gnulib module: getopt-gnu

Portability problems fixed by Gnulib:

* The function `getopt_long_only` does not obey the combination of
  ‘`+`’ and ‘`:`’ flags in the options string on some platforms:
  glibc 2.11.
* The use of ‘`W;`’ in the optstring argument to does not always
  allow `-W foo` to behave synonymously with `--foo`:
  glibc 2.11.
* The function `getopt_long_only` does not support the ‘`+`’
  flag in the options string on some platforms:
  macOS 14, AIX 5.2, Solaris 10.
* The value of `optind` after a missing required argument is wrong
  on some platforms:
  macOS 14.
* The function `getopt_long_only` does not obey the ‘`-`’ flag
  in the options string when `POSIXLY_CORRECT` is set on some platforms:
  Cygwin 1.7.0.
* Some implementations fail to reset state, including re-checking
  `POSIXLY_CORRECT`, when `optind` is set to ‘`0`’:
  NetBSD, Cygwin 1.7.0.
* The function `getopt_long_only` does not support options with
  optional arguments on some platforms:
  macOS 14, OpenBSD 4.0, AIX 5.2, Solaris 11 2010-11, Cygwin 1.5.x.
* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 10.0, Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.
* This function crashes if the option string includes `W;` but
  there are no long options, on some platforms:
  glibc 2.14.

Portability problems not fixed by Gnulib:

* Some implementations return success instead of reporting an ambiguity
  if user’s option is a prefix of two long options with the same outcome:
  FreeBSD.
* The GNU Coding Standards discourage the use of `getopt_long_only`
  in new programs.

---

Next: [Glibc Extensions to `<gnu/libc-version.h>`](#Glibc-gnu_002flibc_002dversion_002eh), Previous: [Glibc `<getopt.h>`](#Glibc-getopt_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.23 Glibc Extensions to `<glob.h>` [¶](#Glibc-Extensions-to-_003cglob_002eh_003e)

* [`glob_pattern_p`](#glob_005fpattern_005fp)

---

Up: [Glibc Extensions to `<glob.h>`](#Glibc-glob_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.23.1 `glob_pattern_p` [¶](#glob_005fpattern_005fp-1)

Gnulib module: glob

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11,
  Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc Extensions to `<grp.h>`](#Glibc-grp_002eh), Previous: [Glibc Extensions to `<glob.h>`](#Glibc-glob_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.24 Glibc Extensions to `<gnu/libc-version.h>` [¶](#Glibc-Extensions-to-_003cgnu_002flibc_002dversion_002eh_003e)

* [`gnu_get_libc_release`](#gnu_005fget_005flibc_005frelease)
* [`gnu_get_libc_version`](#gnu_005fget_005flibc_005fversion)

---

Next: [`gnu_get_libc_version`](#gnu_005fget_005flibc_005fversion), Up: [Glibc Extensions to `<gnu/libc-version.h>`](#Glibc-gnu_002flibc_002dversion_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.24.1 `gnu_get_libc_release` [¶](#gnu_005fget_005flibc_005frelease-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-gnu-get-libc-version-1.html>

Documentation:  
 [man gnu\_get\_libc\_release](https://www.kernel.org/doc/man-pages/online/pages/man3/gnu_get_libc_release.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`gnu_get_libc_release`](#gnu_005fget_005flibc_005frelease), Up: [Glibc Extensions to `<gnu/libc-version.h>`](#Glibc-gnu_002flibc_002dversion_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.24.2 `gnu_get_libc_version` [¶](#gnu_005fget_005flibc_005fversion-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-gnu-get-libc-version-1.html>

Documentation:  
 [man gnu\_get\_libc\_version](https://www.kernel.org/doc/man-pages/online/pages/man3/gnu_get_libc_version.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<gshadow.h>`](#Glibc-gshadow_002eh), Previous: [Glibc Extensions to `<gnu/libc-version.h>`](#Glibc-gnu_002flibc_002dversion_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.25 Glibc Extensions to `<grp.h>` [¶](#Glibc-Extensions-to-_003cgrp_002eh_003e)

* [`fgetgrent`](#fgetgrent)
* [`fgetgrent_r`](#fgetgrent_005fr)
* [`getgrent_r`](#getgrent_005fr)
* [`getgrouplist`](#getgrouplist)
* [`initgroups`](#initgroups)
* [`putgrent`](#putgrent)
* [`setgroups`](#setgroups)

---

Next: [`fgetgrent_r`](#fgetgrent_005fr), Up: [Glibc Extensions to `<grp.h>`](#Glibc-grp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.25.1 `fgetgrent` [¶](#fgetgrent-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Scanning-All-Groups.html>,
* [man fgetgrent](https://www.kernel.org/doc/man-pages/online/pages/man3/fgetgrent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getgrent_r`](#getgrent_005fr), Previous: [`fgetgrent`](#fgetgrent), Up: [Glibc Extensions to `<grp.h>`](#Glibc-grp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.25.2 `fgetgrent_r` [¶](#fgetgrent_005fr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Scanning-All-Groups.html>,
* [man fgetgrent\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/fgetgrent_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getgrouplist`](#getgrouplist), Previous: [`fgetgrent_r`](#fgetgrent_005fr), Up: [Glibc Extensions to `<grp.h>`](#Glibc-grp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.25.3 `getgrent_r` [¶](#getgrent_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getgrent-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Scanning-All-Groups.html>,
* [man getgrent\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getgrent_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`initgroups`](#initgroups), Previous: [`getgrent_r`](#getgrent_005fr), Up: [Glibc Extensions to `<grp.h>`](#Glibc-grp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.25.4 `getgrouplist` [¶](#getgrouplist-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getgrouplist-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Setting-Groups.html>,
* [man getgrouplist](https://www.kernel.org/doc/man-pages/online/pages/man3/getgrouplist.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function takes `int` instead of `gid_t` parameters
  on some platforms: OS X 10.11.
* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.0, Cygwin 1.7.9, mingw, MSVC 14.

The Gnulib module `getugroups` provides a similar API.

---

Next: [`putgrent`](#putgrent), Previous: [`getgrouplist`](#getgrouplist), Up: [Glibc Extensions to `<grp.h>`](#Glibc-grp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.25.5 `initgroups` [¶](#initgroups-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-initgroups-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Setting-Groups.html>,
* [man initgroups](https://www.kernel.org/doc/man-pages/online/pages/man3/initgroups.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is unsafe to call between `fork` and `exec` if
  the parent process is multi-threaded. Instead, use `getgroups` or
  `getgrouplist`

  (or use the gnulib module `mgetgroups`)
  before forking, and `setgroups` in the child.
* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`setgroups`](#setgroups), Previous: [`initgroups`](#initgroups), Up: [Glibc Extensions to `<grp.h>`](#Glibc-grp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.25.6 `putgrent` [¶](#putgrent-1)

Documentation:  
 [man putgrent](https://www.kernel.org/doc/man-pages/online/pages/man3/putgrent.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`putgrent`](#putgrent), Up: [Glibc Extensions to `<grp.h>`](#Glibc-grp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.25.7 `setgroups` [¶](#setgroups-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-setgroups-2.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Setting-Groups.html>,
* [man setgroups](https://www.kernel.org/doc/man-pages/online/pages/man2/setgroups.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, mingw, MSVC 14.
* On very old systems, this function operated on an array of ‘`int`’,
  even though that was a different size than an array of ‘`gid_t`’;
  you can use autoconf’s AC\_TYPE\_GETGROUPS to set `GETGROUPS_T` to
  the appropriate size (since `getgroups` and `setgroups`
  share the same bug).

---

Next: [Glibc `<ifaddrs.h>`](#Glibc-ifaddrs_002eh), Previous: [Glibc Extensions to `<grp.h>`](#Glibc-grp_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.26 Glibc `<gshadow.h>` [¶](#Glibc-_003cgshadow_002eh_003e)

* [`endsgent`](#endsgent)
* [`fgetsgent`](#fgetsgent)
* [`fgetsgent_r`](#fgetsgent_005fr)
* [`getsgent`](#getsgent)
* [`getsgent_r`](#getsgent_005fr)
* [`getsgnam`](#getsgnam)
* [`getsgnam_r`](#getsgnam_005fr)
* [`putsgent`](#putsgent)
* [`setsgent`](#setsgent)
* [`sgetsgent`](#sgetsgent)
* [`sgetsgent_r`](#sgetsgent_005fr)

---

Next: [`fgetsgent`](#fgetsgent), Up: [Glibc `<gshadow.h>`](#Glibc-gshadow_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.26.1 `endsgent` [¶](#endsgent-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`fgetsgent_r`](#fgetsgent_005fr), Previous: [`endsgent`](#endsgent), Up: [Glibc `<gshadow.h>`](#Glibc-gshadow_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.26.2 `fgetsgent` [¶](#fgetsgent-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getsgent`](#getsgent), Previous: [`fgetsgent`](#fgetsgent), Up: [Glibc `<gshadow.h>`](#Glibc-gshadow_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.26.3 `fgetsgent_r` [¶](#fgetsgent_005fr-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getsgent_r`](#getsgent_005fr), Previous: [`fgetsgent_r`](#fgetsgent_005fr), Up: [Glibc `<gshadow.h>`](#Glibc-gshadow_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.26.4 `getsgent` [¶](#getsgent-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getsgnam`](#getsgnam), Previous: [`getsgent`](#getsgent), Up: [Glibc `<gshadow.h>`](#Glibc-gshadow_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.26.5 `getsgent_r` [¶](#getsgent_005fr-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getsgnam_r`](#getsgnam_005fr), Previous: [`getsgent_r`](#getsgent_005fr), Up: [Glibc `<gshadow.h>`](#Glibc-gshadow_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.26.6 `getsgnam` [¶](#getsgnam-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`putsgent`](#putsgent), Previous: [`getsgnam`](#getsgnam), Up: [Glibc `<gshadow.h>`](#Glibc-gshadow_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.26.7 `getsgnam_r` [¶](#getsgnam_005fr-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`setsgent`](#setsgent), Previous: [`getsgnam_r`](#getsgnam_005fr), Up: [Glibc `<gshadow.h>`](#Glibc-gshadow_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.26.8 `putsgent` [¶](#putsgent-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`sgetsgent`](#sgetsgent), Previous: [`putsgent`](#putsgent), Up: [Glibc `<gshadow.h>`](#Glibc-gshadow_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.26.9 `setsgent` [¶](#setsgent-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`sgetsgent_r`](#sgetsgent_005fr), Previous: [`setsgent`](#setsgent), Up: [Glibc `<gshadow.h>`](#Glibc-gshadow_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.26.10 `sgetsgent` [¶](#sgetsgent-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`sgetsgent`](#sgetsgent), Up: [Glibc `<gshadow.h>`](#Glibc-gshadow_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.26.11 `sgetsgent_r` [¶](#sgetsgent_005fr-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<link.h>`](#Glibc-link_002eh), Previous: [Glibc `<gshadow.h>`](#Glibc-gshadow_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.27 Glibc `<ifaddrs.h>` [¶](#Glibc-_003cifaddrs_002eh_003e)

* [`getifaddrs`](#getifaddrs)
* [`freeifaddrs`](#freeifaddrs)

---

Next: [`freeifaddrs`](#freeifaddrs), Up: [Glibc `<ifaddrs.h>`](#Glibc-ifaddrs_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.27.1 `getifaddrs` [¶](#getifaddrs-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getifaddrs.html>

Documentation:  
 [man getifaddrs](https://www.kernel.org/doc/man-pages/online/pages/man3/getifaddrs.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 6.0.

---

Previous: [`getifaddrs`](#getifaddrs), Up: [Glibc `<ifaddrs.h>`](#Glibc-ifaddrs_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.27.2 `freeifaddrs` [¶](#freeifaddrs-1)

Documentation:  
 [man freeifaddrs](https://www.kernel.org/doc/man-pages/online/pages/man3/freeifaddrs.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 6.0.

---

Next: [Glibc `<malloc.h>`](#Glibc-malloc_002eh), Previous: [Glibc `<ifaddrs.h>`](#Glibc-ifaddrs_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.28 Glibc `<link.h>` [¶](#Glibc-_003clink_002eh_003e)

* [`dl_iterate_phdr`](#dl_005fiterate_005fphdr)

---

Up: [Glibc `<link.h>`](#Glibc-link_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.28.1 `dl_iterate_phdr` [¶](#dl_005fiterate_005fphdr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-dl-iterate-phdr-1.html>

Documentation:  
 [man dl\_iterate\_phdr](https://www.kernel.org/doc/man-pages/online/pages/man3/dl_iterate_phdr.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 5.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android 4.4.

---

Next: [Glibc Extensions to `<math.h>`](#Glibc-math_002eh), Previous: [Glibc `<link.h>`](#Glibc-link_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.29 Glibc `<malloc.h>` [¶](#Glibc-_003cmalloc_002eh_003e)

* [`mallinfo`](#mallinfo)
* [`mallinfo2`](#mallinfo2)
* [`malloc_info`](#malloc_005finfo)
* [`malloc_stats`](#malloc_005fstats)
* [`malloc_trim`](#malloc_005ftrim)
* [`malloc_usable_size`](#malloc_005fusable_005fsize)
* [`mallopt`](#mallopt)
* [`memalign`](#memalign)
* [`pvalloc`](#pvalloc)

---

Next: [`mallinfo2`](#mallinfo2), Up: [Glibc `<malloc.h>`](#Glibc-malloc_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.29.1 `mallinfo` [¶](#mallinfo-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Statistics-of-Malloc.html>,
* [man mallinfo](https://www.kernel.org/doc/man-pages/online/pages/man3/mallinfo.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Solaris 11 2010-11, mingw, MSVC 14.

---

Next: [`malloc_info`](#malloc_005finfo), Previous: [`mallinfo`](#mallinfo), Up: [Glibc `<malloc.h>`](#Glibc-malloc_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.29.2 `mallinfo2` [¶](#mallinfo2-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Statistics-of-Malloc.html>,
* [man mallinfo2](https://www.kernel.org/doc/man-pages/online/pages/man3/mallinfo2.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.32, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`malloc_stats`](#malloc_005fstats), Previous: [`mallinfo2`](#mallinfo2), Up: [Glibc `<malloc.h>`](#Glibc-malloc_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.29.3 `malloc_info` [¶](#malloc_005finfo-1)

Documentation:  
 [man malloc\_info](https://www.kernel.org/doc/man-pages/online/pages/man3/malloc_info.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 5.1.

---

Next: [`malloc_trim`](#malloc_005ftrim), Previous: [`malloc_info`](#malloc_005finfo), Up: [Glibc `<malloc.h>`](#Glibc-malloc_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.29.4 `malloc_stats` [¶](#malloc_005fstats-1)

Documentation:  
 [man malloc\_stats](https://www.kernel.org/doc/man-pages/online/pages/man3/malloc_stats.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.

---

Next: [`malloc_usable_size`](#malloc_005fusable_005fsize), Previous: [`malloc_stats`](#malloc_005fstats), Up: [Glibc `<malloc.h>`](#Glibc-malloc_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.29.5 `malloc_trim` [¶](#malloc_005ftrim-1)

Documentation:  
 [man malloc\_trim](https://www.kernel.org/doc/man-pages/online/pages/man3/malloc_trim.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 9.0.

---

Next: [`mallopt`](#mallopt), Previous: [`malloc_trim`](#malloc_005ftrim), Up: [Glibc `<malloc.h>`](#Glibc-malloc_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.29.6 `malloc_usable_size` [¶](#malloc_005fusable_005fsize-1)

Documentation:  
 [man malloc\_usable\_size](https://www.kernel.org/doc/man-pages/online/pages/man3/malloc_usable_size.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11.00, Solaris 11.4, mingw, MSVC 14, Android 4.1.

---

Next: [`memalign`](#memalign), Previous: [`malloc_usable_size`](#malloc_005fusable_005fsize), Up: [Glibc `<malloc.h>`](#Glibc-malloc_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.29.7 `mallopt` [¶](#mallopt-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Malloc-Tunable-Parameters.html>,
* [man mallopt](https://www.kernel.org/doc/man-pages/online/pages/man3/mallopt.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Solaris 11 2010-11, mingw, MSVC 14, Android 7.1.

---

Next: [`pvalloc`](#pvalloc), Previous: [`mallopt`](#mallopt), Up: [Glibc `<malloc.h>`](#Glibc-malloc_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.29.8 `memalign` [¶](#memalign-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Aligned-Memory-Blocks.html>,
* [man memalign](https://www.kernel.org/doc/man-pages/online/pages/man3/memalign.3.html).

Gnulib module: memalign

Portability problems fixed by Gnulib:

* This function is declared in `<stdlib.h>` instead of `<malloc.h>`
  on some platforms:
  Solaris 11.
* This function doesn’t accept an alignment of 1 or 2 on some platforms:
  Solaris 11.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 12.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11.00, mingw, MSVC 14.

The Gnulib module `aligned-malloc` provides functions for
allocating and freeing blocks of suitably aligned memory.

---

Previous: [`memalign`](#memalign), Up: [Glibc `<malloc.h>`](#Glibc-malloc_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.29.9 `pvalloc` [¶](#pvalloc-1)

Documentation:  
 [man pvalloc](https://www.kernel.org/doc/man-pages/online/pages/man3/pvalloc.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [Glibc `<mcheck.h>`](#Glibc-mcheck_002eh), Previous: [Glibc `<malloc.h>`](#Glibc-malloc_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
