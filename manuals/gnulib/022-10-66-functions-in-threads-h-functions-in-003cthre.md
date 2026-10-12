### 10.66 Functions in `<threads.h>` [¶](#Functions-in-_003cthreads_002eh_003e-1)

* [`call_once`](#call_005fonce)
* [`cnd_broadcast`](#cnd_005fbroadcast)
* [`cnd_destroy`](#cnd_005fdestroy)
* [`cnd_init`](#cnd_005finit)
* [`cnd_signal`](#cnd_005fsignal)
* [`cnd_timedwait`](#cnd_005ftimedwait)
* [`cnd_wait`](#cnd_005fwait)
* [`mtx_destroy`](#mtx_005fdestroy)
* [`mtx_init`](#mtx_005finit)
* [`mtx_lock`](#mtx_005flock)
* [`mtx_timedlock`](#mtx_005ftimedlock)
* [`mtx_trylock`](#mtx_005ftrylock)
* [`mtx_unlock`](#mtx_005funlock)
* [`thrd_create`](#thrd_005fcreate)
* [`thrd_current`](#thrd_005fcurrent)
* [`thrd_detach`](#thrd_005fdetach)
* [`thrd_equal`](#thrd_005fequal)
* [`thrd_exit`](#thrd_005fexit)
* [`thrd_join`](#thrd_005fjoin)
* [`thrd_sleep`](#thrd_005fsleep)
* [`thrd_yield`](#thrd_005fyield)
* [`tss_create`](#tss_005fcreate)
* [`tss_delete`](#tss_005fdelete)
* [`tss_get`](#tss_005fget)
* [`tss_set`](#tss_005fset)

---

Next: [`cnd_broadcast`](#cnd_005fbroadcast), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.1 `call_once` [¶](#call_005fonce-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.2.1.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/call_once.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Call-Once.html>.

Gnulib module: call\_once

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

* This function makes applications hang forever on some platforms:
  Cygwin 3.5.3.
* This function does not work on some platforms:
  Haiku.

---

Next: [`cnd_destroy`](#cnd_005fdestroy), Previous: [`call_once`](#call_005fonce), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.2 `cnd_broadcast` [¶](#cnd_005fbroadcast-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.3.1.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cnd_broadcast.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Condition-Variables.html>.

Gnulib module: cnd

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`cnd_init`](#cnd_005finit), Previous: [`cnd_broadcast`](#cnd_005fbroadcast), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.3 `cnd_destroy` [¶](#cnd_005fdestroy-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.3.2.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cnd_destroy.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Condition-Variables.html>.

Gnulib module: cnd

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`cnd_signal`](#cnd_005fsignal), Previous: [`cnd_destroy`](#cnd_005fdestroy), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.4 `cnd_init` [¶](#cnd_005finit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.3.3.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cnd_init.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Condition-Variables.html>.

Gnulib module: cnd

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`cnd_timedwait`](#cnd_005ftimedwait), Previous: [`cnd_init`](#cnd_005finit), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.5 `cnd_signal` [¶](#cnd_005fsignal-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.3.4.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cnd_signal.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Condition-Variables.html>.

Gnulib module: cnd

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`cnd_wait`](#cnd_005fwait), Previous: [`cnd_signal`](#cnd_005fsignal), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.6 `cnd_timedwait` [¶](#cnd_005ftimedwait-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.3.5.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cnd_timedwait.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Condition-Variables.html>.

Gnulib module: cnd

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

* This function sometimes waits indefinitely
  instead of up to the specified time point,
  on some platforms (glibc/Linux, musl libc) in some hypervisors:
  VirtualBox 6.1.50\_Ubuntu with paravirtualization set to "Default" or "KVM".

---

Next: [`mtx_destroy`](#mtx_005fdestroy), Previous: [`cnd_timedwait`](#cnd_005ftimedwait), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.7 `cnd_wait` [¶](#cnd_005fwait-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.3.6.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/cnd_wait.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Condition-Variables.html>.

Gnulib module: cnd

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`mtx_init`](#mtx_005finit), Previous: [`cnd_wait`](#cnd_005fwait), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.8 `mtx_destroy` [¶](#mtx_005fdestroy-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.4.1.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mtx_destroy.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Mutexes.html>.

Gnulib module: mtx

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`mtx_lock`](#mtx_005flock), Previous: [`mtx_destroy`](#mtx_005fdestroy), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.9 `mtx_init` [¶](#mtx_005finit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.4.2.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mtx_init.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Mutexes.html>.

Gnulib module: mtx

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`mtx_timedlock`](#mtx_005ftimedlock), Previous: [`mtx_init`](#mtx_005finit), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.10 `mtx_lock` [¶](#mtx_005flock-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.4.3.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mtx_lock.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Mutexes.html>.

Gnulib module: mtx

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`mtx_trylock`](#mtx_005ftrylock), Previous: [`mtx_lock`](#mtx_005flock), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.11 `mtx_timedlock` [¶](#mtx_005ftimedlock-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.4.4.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mtx_timedlock.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Mutexes.html>.

Gnulib module: mtx

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`mtx_unlock`](#mtx_005funlock), Previous: [`mtx_timedlock`](#mtx_005ftimedlock), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.12 `mtx_trylock` [¶](#mtx_005ftrylock-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.4.5.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mtx_trylock.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Mutexes.html>.

Gnulib module: mtx

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`thrd_create`](#thrd_005fcreate), Previous: [`mtx_trylock`](#mtx_005ftrylock), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.13 `mtx_unlock` [¶](#mtx_005funlock-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.4.6.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mtx_unlock.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Mutexes.html>.

Gnulib module: mtx

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`thrd_current`](#thrd_005fcurrent), Previous: [`mtx_unlock`](#mtx_005funlock), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.14 `thrd_create` [¶](#thrd_005fcreate-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.5.1.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/thrd_create.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Thread-Management.html>.

Gnulib module: thrd

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.
* This function uses an incorrectly defined `thrd_start_t` on some platforms:
  AIX 7.2.

Portability problems not fixed by Gnulib:

---

Next: [`thrd_detach`](#thrd_005fdetach), Previous: [`thrd_create`](#thrd_005fcreate), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.15 `thrd_current` [¶](#thrd_005fcurrent-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.5.2.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/thrd_current.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Thread-Management.html>.

Gnulib module: thrd

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`thrd_equal`](#thrd_005fequal), Previous: [`thrd_current`](#thrd_005fcurrent), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.16 `thrd_detach` [¶](#thrd_005fdetach-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.5.3.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/thrd_detach.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Thread-Management.html>.

Gnulib module: thrd

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`thrd_exit`](#thrd_005fexit), Previous: [`thrd_detach`](#thrd_005fdetach), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.17 `thrd_equal` [¶](#thrd_005fequal-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.5.4.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/thrd_equal.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Thread-Management.html>.

Gnulib module: thrd

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`thrd_join`](#thrd_005fjoin), Previous: [`thrd_equal`](#thrd_005fequal), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.18 `thrd_exit` [¶](#thrd_005fexit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.5.5.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/thrd_exit.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Thread-Management.html>.

Gnulib module: thrd

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.
* The exit code provided to this function is discarded on some platforms:
  AIX 7.3.1.

Portability problems not fixed by Gnulib:

---

Next: [`thrd_sleep`](#thrd_005fsleep), Previous: [`thrd_exit`](#thrd_005fexit), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.19 `thrd_join` [¶](#thrd_005fjoin-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.5.6.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/thrd_join.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Thread-Management.html>.

Gnulib module: thrd

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.
* This function never stores an exit code on some platforms:
  AIX 7.3.1.
* This function crashes when the second argument is NULL on some platforms:
  Solaris 11.4.

Portability problems not fixed by Gnulib:

---

Next: [`thrd_yield`](#thrd_005fyield), Previous: [`thrd_join`](#thrd_005fjoin), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.20 `thrd_sleep` [¶](#thrd_005fsleep-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.5.7.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/thrd_sleep.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Thread-Management.html>.

Gnulib module: thrd

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`tss_create`](#tss_005fcreate), Previous: [`thrd_sleep`](#thrd_005fsleep), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.21 `thrd_yield` [¶](#thrd_005fyield-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.5.8.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/thrd_yield.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Thread-Management.html>.

Gnulib module: thrd

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`tss_delete`](#tss_005fdelete), Previous: [`thrd_yield`](#thrd_005fyield), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.22 `tss_create` [¶](#tss_005fcreate-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.6.1.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tss_create.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Thread_002dlocal-Storage.html>.

Gnulib module: tss

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`tss_get`](#tss_005fget), Previous: [`tss_create`](#tss_005fcreate), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.23 `tss_delete` [¶](#tss_005fdelete-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.6.2.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tss_delete.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Thread_002dlocal-Storage.html>.

Gnulib module: tss

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [`tss_set`](#tss_005fset), Previous: [`tss_delete`](#tss_005fdelete), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.24 `tss_get` [¶](#tss_005fget-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.6.3.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tss_get.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Thread_002dlocal-Storage.html>.

Gnulib module: tss

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Previous: [`tss_get`](#tss_005fget), Up: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.66.25 `tss_set` [¶](#tss_005fset-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.28.6.4.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tss_set.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/ISO-C-Thread_002dlocal-Storage.html>.

Gnulib module: tss

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 9.3, NetBSD 8.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.1.x, mingw, MSVC 14, Android API level 29.

Portability problems not fixed by Gnulib:

---

Next: [Functions in `<uchar.h>`](#Functions-in-_003cuchar_002eh_003e), Previous: [Functions in `<threads.h>`](#Functions-in-_003cthreads_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.67 Functions in `<time.h>` [¶](#Functions-in-_003ctime_002eh_003e-1)

* [`asctime`](#asctime)
* [`asctime_r`](#asctime_005fr)
* [`clock`](#clock)
* [`clock_getcpuclockid`](#clock_005fgetcpuclockid)
* [`clock_getres`](#clock_005fgetres)
* [`clock_gettime`](#clock_005fgettime)
* [`clock_nanosleep`](#clock_005fnanosleep)
* [`clock_settime`](#clock_005fsettime)
* [`ctime`](#ctime)
* [`ctime_r`](#ctime_005fr)
* [`daylight`](#daylight)
* [`difftime`](#difftime)
* [`getdate`](#getdate)
* [`getdate_err`](#getdate_005ferr)
* [`gmtime`](#gmtime)
* [`gmtime_r`](#gmtime_005fr)
* [`localtime`](#localtime)
* [`localtime_r`](#localtime_005fr)
* [`mktime`](#mktime)
* [`nanosleep`](#nanosleep)
* [`strftime`](#strftime)
* [`strftime_l`](#strftime_005fl)
* [`strptime`](#strptime)
* [`time`](#time)
* [`timegm`](#timegm)
* [`timer_create`](#timer_005fcreate)
* [`timer_delete`](#timer_005fdelete)
* [`timer_getoverrun`](#timer_005fgetoverrun)
* [`timer_gettime`](#timer_005fgettime)
* [`timer_settime`](#timer_005fsettime)
* [`timespec_get`](#timespec_005fget)
* [`timespec_getres`](#timespec_005fgetres)
* [`timezone`](#timezone)
* [`tzname`](#tzname)
* [`tzset`](#tzset)

---

Next: [`asctime_r`](#asctime_005fr), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.1 `asctime` [¶](#asctime-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/asctime.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is deprecated in C23.
  Likewise, POSIX says this function is obsolescent and it is planned to be
  removed in a future version.
  Portable applications can use `strftime` (or even `sprintf`) instead.
  However, `strftime` is locale dependent.
* This function may overflow its internal buffer if its argument
  specifies a year before 1000 or after 9999.
  See [`ctime`](#ctime).
* Although it is tempting to call this function on the value that
  functions like `localtime` return, this is typically a mistake.
  On most current platforms, these functions return a null pointer for
  timestamps out of range, and this function has undefined behavior in
  that case.

---

Next: [`clock`](#clock), Previous: [`asctime`](#asctime), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.2 `asctime_r` [¶](#asctime_005fr-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/asctime_r.html>

Gnulib module: extensions

Portability problems fixed by Gnulib:

* This function has an incompatible declaration on some platforms:
  Solaris 11.4 (when `_POSIX_PTHREAD_SEMANTICS` is not defined).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function is removed in POSIX.1-2024.
  Use the function `strftime` (or even `sprintf`) instead.
  However, `strftime` is locale dependent.
* This function may overflow its output buffer if its argument
  specifies a year before 1000 or after 9999.
  See [`ctime`](#ctime).
* Although it is tempting to call this function on the value that
  functions like `localtime` return, this is typically a mistake.
  On most current platforms, these functions return a null pointer for
  timestamps out of range, and this function has undefined behavior in
  that case.

---

Next: [`clock_getcpuclockid`](#clock_005fgetcpuclockid), Previous: [`asctime_r`](#asctime_005fr), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.3 `clock` [¶](#clock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/clock.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`clock_getres`](#clock_005fgetres), Previous: [`clock`](#clock), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.4 `clock_getcpuclockid` [¶](#clock_005fgetcpuclockid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/clock_getcpuclockid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.11, FreeBSD 6.0, NetBSD 7.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.9, mingw, MSVC 14, Android 5.1.

---

Next: [`clock_gettime`](#clock_005fgettime), Previous: [`clock_getcpuclockid`](#clock_005fgetcpuclockid), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.5 `clock_getres` [¶](#clock_005fgetres-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/clock_getres.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.11, Minix 3.1.8, mingw, MSVC 14.
* On many platforms, this function returns a value other than the clock
  resolution of `clock_gettime`, i.e., the minimum distance between
  differing timestamps. For example, on AIX 7.2 it returns 10
  milliseconds even though the clock resolution is 1 microsecond.
  Conversely, on GNU/Linux it typically returns 1 nanosecond even
  though the clock resolution may be greater.

The Gnulib module `gettime-res` is a partial substitute; it implements
the `CLOCK_REALTIME` functionality of `clock_getres`,
and fixes the too-high resolution bug of platforms like AIX 7.2.

---

Next: [`clock_nanosleep`](#clock_005fnanosleep), Previous: [`clock_getres`](#clock_005fgetres), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.6 `clock_gettime` [¶](#clock_005fgettime-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/clock_gettime.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.11, Minix 3.1.8, mingw, MSVC 14.
* This function leaves the upper 32 bits of the `tv_sec` field of the result
  uninitialized on some platforms:
  mingw in 32-bit mode.

The Gnulib modules `gettime` and `timespec_get` are partial
substitutes; they implement the `CLOCK_REALTIME` functionality of
`clock_gettime`.

---

Next: [`clock_settime`](#clock_005fsettime), Previous: [`clock_gettime`](#clock_005fgettime), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.7 `clock_nanosleep` [¶](#clock_005fnanosleep-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/clock_nanosleep.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.11, FreeBSD 11.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.9, mingw, MSVC 14.

---

Next: [`ctime`](#ctime), Previous: [`clock_nanosleep`](#clock_005fnanosleep), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.8 `clock_settime` [¶](#clock_005fsettime-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/clock_settime.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.11, Minix 3.1.8, Cygwin 1.7.9, mingw, MSVC 14.

---

Next: [`ctime_r`](#ctime_005fr), Previous: [`clock_settime`](#clock_005fsettime), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.9 `ctime` [¶](#ctime-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ctime.html>

Gnulib module: ctime

Portability problems fixed by Gnulib:

* On native Windows platforms (mingw, MSVC), this function works incorrectly
  when the environment variable `TZ` has been set by Cygwin.

Portability problems not fixed by Gnulib:

* This function is deprecated in C23.
  Likewise, POSIX says this function is obsolescent and it is planned to be
  removed in a future version.
  Portable applications can use `localtime_r` and `strftime`
  (or even `sprintf`) instead.
  However, `localtime_r` can fail and `strftime` is locale dependent.
* This function may overflow its internal buffer if its argument
  specifies a time before the year 1000 or after the year 9999.
* This function may dereference an internal null pointer if its argument
  specifies a time before the year `INT_MIN`+1900 or after the
  year `INT_MAX`+1900.
* The `ctime` function need not be reentrant, and consequently is
  not required to be thread safe. Implementations of `ctime`
  typically write the timestamp into static buffer. If two threads
  call `ctime` at roughly the same time, you might end up with the
  wrong date in one of the threads, or some undefined string.
* Native Windows platforms (mingw, MSVC) support only a subset of time
  zones supported by GNU or specified by POSIX. See [`tzset`](#tzset).

Although `ctime` and related functions `asctime`, `asctime_r`
and `ctime_r` formerly were plausible to use,
they are now unsafe in general, and should be avoided.

Decades ago when `time_t` was narrow
and there was no `strftime` or internationalization,
code could call these functions and then select the parts needed.
For example, in Unix 7th Edition `/usr/src/cmd/ls.c` (1979):

```
cp = ctime(&p->lmtime);
if(p->lmtime < year)
        printf(" %-7.7s %-4.4s ", cp+4, cp+20); else
        printf(" %-12.12s ", cp+4);
```

This had well-defined behavior when `time_t` was only 32 bits
and so was OK for circa 1979 platforms.

However, today’s platforms have a `time_t` so wide
that the year might not be in the range [1000, 9999].
In this case the behavior of `ctime` is undefined
and some platforms behave badly, overrunning a buffer
or dereferencing an internal null pointer;
and even on platforms where no undefined behavior occurs,
the 7th Edition code generates wrong output for out-of-range years,
because it incorrectly assumes that every year is represented by
exactly four digits.

---

Next: [`daylight`](#daylight), Previous: [`ctime`](#ctime), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.10 `ctime_r` [¶](#ctime_005fr-1)

Removed in POSIX.1-2024.
POSIX.1-2017 specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ctime_r.html>

Gnulib module: extensions

Portability problems fixed by Gnulib:

* This function has an incompatible declaration on some platforms:
  Solaris 11.4 (when `_POSIX_PTHREAD_SEMANTICS` is not defined).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function is removed in POSIX.1-2024.
  Use the functions `localtime_r` and `strftime`
  (or even `sprintf`) instead.
  However, `localtime_r` can fail and `strftime` is locale dependent.
* This function may overflow its output buffer if its argument
  specifies a time before the year 1000 or after the year 9999.
  See [`ctime`](#ctime).
* This function may dereference an internal null pointer if its argument
  specifies a time before the year `INT_MIN`+1900 or after the
  year `INT_MAX`+1900.

---

Next: [`difftime`](#difftime), Previous: [`ctime_r`](#ctime_005fr), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.11 `daylight` [¶](#daylight-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/daylight.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable’s contents are unreliable if you use a geographical
  `TZ` setting like `TZ="America/Los_Angeles"`.
* This variable is missing on some platforms:
  FreeBSD 14.0, OpenBSD 5.7.
* This variable is called `_daylight` on some platforms:
  mingw with `-DNO_OLDNAMES`, MSVC.
* The address of this variable is not a compile-time constant on some platforms:
  mingw, MSVC.
* Native Windows platforms (mingw, MSVC) support only a subset of time
  zones supported by GNU or specified by POSIX. See [`tzset`](#tzset).

This variable is vestigial: it is not needed and does not suffice in general.
It is planned to be removed from a future version of POSIX.
A more portable way of getting the UTC offset is to use
`strftime` with the `%z` format. See [`strftime`](#strftime).

---

Next: [`getdate`](#getdate), Previous: [`daylight`](#daylight), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.12 `difftime` [¶](#difftime-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/difftime.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`getdate_err`](#getdate_005ferr), Previous: [`difftime`](#difftime), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.13 `getdate` [¶](#getdate-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getdate.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Gnulib provides a module `parse-datetime` that contains a function
`parse_datetime`
that has similar functionality as the `getdate` function.

---

Next: [`gmtime`](#gmtime), Previous: [`getdate`](#getdate), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.14 `getdate_err` [¶](#getdate_005ferr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getdate_err.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on some platforms:
  FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`gmtime_r`](#gmtime_005fr), Previous: [`getdate_err`](#getdate_005ferr), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.15 `gmtime` [¶](#gmtime-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/gmtime.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* On some platforms, this function yields incorrect values for
  timestamps before the year 1:
  MacOS X 10.5, Solaris 11.3.
* On some platforms, this function returns NULL for arguments < -43200, that is,
  for timestamps before 1969-12-31 12:00:00 UTC:
  mingw, MSVC.

---

Next: [`localtime`](#localtime), Previous: [`gmtime`](#gmtime), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.16 `gmtime_r` [¶](#gmtime_005fr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/gmtime_r.html>

Gnulib module: time\_r

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function is not declared unless `_REENTRANT` is defined,
  on some platforms:
  HP-UX 11.

Portability problems not fixed by Gnulib:

* On some platforms, this function yields incorrect values for
  timestamps before the year 1:
  MacOS X 10.5, Solaris 11.3.
* This function sets the field `tm_zone` to `"GMT"`
  instead of `"UTC"` on some platforms:
  glibc, NetBSD, OpenBSD 7.7, Minix, Cygwin, Android.

---

Next: [`localtime_r`](#localtime_005fr), Previous: [`gmtime_r`](#gmtime_005fr), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.17 `localtime` [¶](#localtime-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/localtime.html>

Gnulib module: localtime

Portability problems fixed by Gnulib:

* On native Windows platforms (mingw, MSVC), this function works incorrectly
  when the environment variable `TZ` has been set by Cygwin.

Portability problems not fixed by Gnulib:

* On some platforms, this function loops forever for values
  near extrema (such as the year *-2\*\*31*):
  Mac OS X 10.6.
  You can use the `time_rz` module to work around the problem.
* On some platforms, this function returns nonsense values for
  unsupported arguments (like *2^56*), rather than failing:
  FreeBSD 10.
* On some platforms, this function yields incorrect values for
  timestamps before the year 1:
  MacOS X 10.5, Solaris 11.3.
* On some platforms, this function returns NULL for negative arguments, that is,
  for timestamps before 1970-01-01 00:00:00 local time:
  mingw, MSVC.
* Native Windows platforms (mingw, MSVC) support only a subset of time
  zones supported by GNU or specified by POSIX. See [`tzset`](#tzset).

---

Next: [`mktime`](#mktime), Previous: [`localtime`](#localtime), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.18 `localtime_r` [¶](#localtime_005fr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/localtime_r.html>

Gnulib module: time\_r

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function is not declared unless `_REENTRANT` is defined,
  on some platforms:
  HP-UX 11.

Portability problems not fixed by Gnulib:

* On some platforms, this function loops forever for values
  near extrema (such as the year *-2\*\*31*):
  Mac OS X 10.6.
  You can use the `time_rz` module to work around the problem.
* On some platforms, this function returns nonsense values for
  unsupported arguments (like *2^56*), rather than failing:
  FreeBSD 10.
* On some platforms, this function yields incorrect values for
  timestamps before the year 1:
  MacOS X 10.5, Solaris 11.3.

---

Next: [`nanosleep`](#nanosleep), Previous: [`localtime_r`](#localtime_005fr), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.19 `mktime` [¶](#mktime-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mktime.html>

Gnulib module: mktime

Portability problems fixed by Gnulib:

* On native Windows platforms (mingw, MSVC), this function works incorrectly
  when the environment variable `TZ` has been set by Cygwin.
* `mktime` may go into an endless loop on some platforms.
* `mktime` may occasionally return wrong results on some platforms.

Portability problems not fixed by Gnulib:

* On some platforms, this function yields incorrect values for
  timestamps before the year 1:
  MacOS X 10.5, Solaris 11.3.
* Native Windows platforms (mingw, MSVC) support only a subset of time
  zones supported by GNU or specified by POSIX. See [`tzset`](#tzset).

---

Next: [`strftime`](#strftime), Previous: [`mktime`](#mktime), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.20 `nanosleep` [¶](#nanosleep-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/nanosleep.html>

Gnulib module: nanosleep

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function mishandles large arguments when interrupted by a signal on some
  platforms:
  Linux 64-bit, Solaris 64-bit.
* This function cannot sleep longer than 49.7 days on some platforms:
  Cygwin 1.5.x.
* This function does not fail when passed a negative nanosecond value on some
  platforms:
  newer 32-bit mingw.

Portability problems not fixed by Gnulib:

---

Next: [`strftime_l`](#strftime_005fl), Previous: [`nanosleep`](#nanosleep), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.21 `strftime` [¶](#strftime-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strftime.html>

Gnulib module: strftime-fixes

Portability problems fixed by Gnulib:

* On native Windows platforms (mingw, MSVC), this function works incorrectly
  when the environment variable `TZ` has been set by Cygwin.

Portability problems not fixed by Gnulib:

* The %c specifier does not work with a time retrieved through `gmtime`
  or `gmtime_r`, since it outputs also the current time zone,
  on some platforms:
  glibc 2.30, NetBSD 10.0, Solaris 11.4.
* The %c specifier produces no weekday name and no month name, only a
  potentially ambiguous numerical output, on some platforms:
  mingw, MSVC.
* The %h specifier produces empty output on some platforms:
  mingw, MSVC.
* The %r specifier produces empty output on some platforms:
  mingw, MSVC.
* The %r specifier produces empty output, at least in a French locale,
  on some platforms:
  macOS 12.5, FreeBSD 14.0.
* The %r specifier includes an AM/PM indicator, at least in a French locale,
  on some platforms:
  NetBSD 10.0, Solaris 11.4.
* The %p specifier produces non-empty output, at least in a French locale,
  on some platforms:
  NetBSD 10.0, Solaris 11.4.
* The Windows C runtime library (which is used by MinGW) does not
  support the %e specifier (and possibly the other more recent SUS
  specifiers too, i.e., %C, %D, %h, %n, %r, %R, %t, and %T).
* Native Windows platforms (mingw, MSVC) support only a subset of time
  zones supported by GNU or specified by POSIX. See [`tzset`](#tzset).

Extension: Gnulib offers a module ‘`nstrftime`’ that provides an
`nstrftime` function with various GNU extensions.

---

Next: [`strptime`](#strptime), Previous: [`strftime`](#strftime), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.22 `strftime_l` [¶](#strftime_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strftime_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.

---

Next: [`time`](#time), Previous: [`strftime_l`](#strftime_005fl), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.23 `strptime` [¶](#strptime-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strptime.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-strptime-3.html>

Gnulib module: strptime

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`timegm`](#timegm), Previous: [`strptime`](#strptime), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.24 `time` [¶](#time-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/time.html>

Gnulib module: time

Portability problems fixed by Gnulib:

* This function is not consistent with `gettimeofday` and `timespec_get`
  on some platforms:
  glibc 2.31 or newer on Linux,
  FreeBSD 14.0,
  AIX 7.2, native Windows.
  Namely, in the first 1 to 2.5 milliseconds of every second (or, on AIX and
  Windows, in the first 5 milliseconds of every second), `time` returns
  a value that is one less than the `tv_sec` part of the return value of
  `gettimeofday` or `timespec_get`.

Portability problems not fixed by Gnulib:

---

Next: [`timer_create`](#timer_005fcreate), Previous: [`time`](#time), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.25 `timegm` [¶](#timegm-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Broken_002ddown-Time.html>,
* [man timegm](https://www.kernel.org/doc/man-pages/online/pages/man3/timegm.3.html).

Gnulib module: timegm

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 11.3, mingw, MSVC 14, Android 3.0.

Portability problems not fixed by Gnulib:

---

Next: [`timer_delete`](#timer_005fdelete), Previous: [`timegm`](#timegm), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.26 `timer_create` [¶](#timer_005fcreate-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/timer_create.html>

Gnulib module: timer-time

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.11, FreeBSD 6.4, Minix 3.1.8, mingw, MSVC 14.
* This function fails with `ENOSYS` on some platforms:
  OpenBSD 4.9.

---

Next: [`timer_getoverrun`](#timer_005fgetoverrun), Previous: [`timer_create`](#timer_005fcreate), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.27 `timer_delete` [¶](#timer_005fdelete-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/timer_delete.html>

Gnulib module: timer-time

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.11, FreeBSD 6.4, Minix 3.1.8, mingw, MSVC 14.
* This function fails with `ENOSYS` on some platforms:
  OpenBSD 4.9.

---

Next: [`timer_gettime`](#timer_005fgettime), Previous: [`timer_delete`](#timer_005fdelete), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.28 `timer_getoverrun` [¶](#timer_005fgetoverrun-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/timer_getoverrun.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.11, FreeBSD 6.0, Minix 3.1.8, Cygwin 2.11.x, mingw, MSVC 14.

---

Next: [`timer_settime`](#timer_005fsettime), Previous: [`timer_getoverrun`](#timer_005fgetoverrun), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.29 `timer_gettime` [¶](#timer_005fgettime-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/timer_gettime.html>

Gnulib module: timer-time

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.11, FreeBSD 6.4, Minix 3.1.8, mingw, MSVC 14.
* This function fails with `ENOSYS` on some platforms:
  OpenBSD 4.9.

---

Next: [`timespec_get`](#timespec_005fget), Previous: [`timer_gettime`](#timer_005fgettime), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.30 `timer_settime` [¶](#timer_005fsettime-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/timer_settime.html>

Gnulib module: timer-time

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.11, FreeBSD 6.4, Minix 3.1.8, mingw, MSVC 14.
* This function fails with `ENOSYS` on some platforms:
  OpenBSD 4.9.

---

Next: [`timespec_getres`](#timespec_005fgetres), Previous: [`timer_settime`](#timer_005fsettime), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.31 `timespec_get` [¶](#timespec_005fget-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.29.2.6

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/timespec_get.html>

Gnulib module: timespec\_get

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.15, macOS 10.14, FreeBSD 11.0, NetBSD 7.1, OpenBSD 6.0, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.5.x, mingw with MSVCRT, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`timezone`](#timezone), Previous: [`timespec_get`](#timespec_005fget), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.32 `timespec_getres` [¶](#timespec_005fgetres-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.29.2.7

Gnulib module: timespec\_getres

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.33, macOS 12, FreeBSD 14.0, NetBSD 9.2, OpenBSD 7.0, Minix 3.3.0, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 2.9, mingw, MSVC 14, Android API level 34.

Portability problems not fixed by Gnulib:

* On some platforms, this function returns a value other than the clock
  resolution of `timespec_get`, i.e., the minimum distance between
  differing timestamps. For example, on GNU/Linux it typically returns
  1 nanosecond regardless of the actual clock resolution.

The Gnulib module `gettime-res` is a partial substitute; it implements
the `TIME_UTC` functionality of `timespec_getres`.

---

Next: [`tzname`](#tzname), Previous: [`timespec_getres`](#timespec_005fgetres), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.33 `timezone` [¶](#timezone-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/timezone.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable’s contents are unreliable if you use a geographical
  `TZ` setting like `TZ="America/Los_Angeles"`.
* This variable is missing (and instead, a function named `timezone` exists)
  on some platforms:
  FreeBSD 14.0, OpenBSD 5.7.
* This variable is called `_timezone` on some platforms:
  mingw with `-DNO_OLDNAMES`, MSVC.
* The address of this variable is not a compile-time constant on some platforms:
  mingw, MSVC.
* Native Windows platforms (mingw, MSVC) support only a subset of time
  zones supported by GNU or specified by POSIX. See [`tzset`](#tzset).

This variable is vestigial: it is not needed and does not suffice in general.
It is planned to be removed from a future version of POSIX.
A more portable way of getting the UTC offset is to use
`strftime` with the `%z` format. See [`strftime`](#strftime).

---

Next: [`tzset`](#tzset), Previous: [`timezone`](#timezone), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.34 `tzname` [¶](#tzname-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tzname.html>

Gnulib module: tzname

Portability problems fixed by Gnulib:

* This variable is called `_tzname` on some platforms:
  mingw with `-DNO_OLDNAMES`, MSVC.

Portability problems not fixed by Gnulib:

* This variable’s contents are unreliable if you use a geographical
  `TZ` setting like `TZ="America/Los_Angeles"`.
* The address of this variable is not a compile-time constant on some platforms:
  Cygwin, mingw, MSVC.
* Native Windows platforms (mingw, MSVC) support only a subset of time
  zones supported by GNU or specified by POSIX. See [`tzset`](#tzset).

This variable is vestigial: it is not needed and does not suffice in general.
It is planned to be removed from a future version of POSIX.
A more portable way of getting the time zone abbreviation is to use
`strftime` with the `%Z` format. See [`strftime`](#strftime).

---

Previous: [`tzname`](#tzname), Up: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.67.35 `tzset` [¶](#tzset-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tzset.html>

Gnulib module: tzset

Portability problems fixed by Gnulib:

* On native Windows platforms (mingw, MSVC), this function works incorrectly
  when the environment variable `TZ` has been set by Cygwin.

Portability problems not fixed by Gnulib:

* Native Windows platforms (mingw, MSVC) support only a subset of
  POSIX-specified values for the `TZ` environment variable,
  consisting of a time zone abbreviation containing exactly three ASCII
  letters with no daylight saving time or angle brackets, and with no
  support for `tz` database settings like
  `TZ='America/New_York'`. Even this subset does not work on
  applications built via the Universal Windows Platform, as it does not
  make environment variables like `TZ` available to applications.
* Older POSIX platforms do not support angle brackets in `TZ` values,
  as this feature was added in IEEE Std 1003.1-2001.

---

Next: [Functions in `<unistd.h>`](#Functions-in-_003cunistd_002eh_003e), Previous: [Functions in `<time.h>`](#Functions-in-_003ctime_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.68 Functions in `<uchar.h>` [¶](#Functions-in-_003cuchar_002eh_003e-1)

* [`c8rtomb`](#c8rtomb)
* [`c16rtomb`](#c16rtomb)
* [`c32rtomb`](#c32rtomb)
* [`mbrtoc8`](#mbrtoc8)
* [`mbrtoc16`](#mbrtoc16)
* [`mbrtoc32`](#mbrtoc32)

---

Next: [`c16rtomb`](#c16rtomb), Up: [Functions in `<uchar.h>`](#Functions-in-_003cuchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.68.1 `c8rtomb` [¶](#c8rtomb-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most platforms:
  glibc 2.35, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.1, HP-UX 11.31, Solaris 11.4,
  Cygwin,
  mingw, MSVC 14, Android 9.0.

---

Next: [`c32rtomb`](#c32rtomb), Previous: [`c8rtomb`](#c8rtomb), Up: [Functions in `<uchar.h>`](#Functions-in-_003cuchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.68.2 `c16rtomb` [¶](#c16rtomb-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.30.1.4.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/c16rtomb.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most non-glibc platforms:
  glibc 2.15, macOS 14, FreeBSD 6.4, NetBSD 10.0, OpenBSD 7.3, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.4.x, mingw, MSVC 9, Android 4.4.

---

Next: [`mbrtoc8`](#mbrtoc8), Previous: [`c16rtomb`](#c16rtomb), Up: [Functions in `<uchar.h>`](#Functions-in-_003cuchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.68.3 `c32rtomb` [¶](#c32rtomb-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.30.1.6.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/c32rtomb.html>

Gnulib module: c32rtomb

Portability problems fixed by Gnulib:

* This function is missing on most non-glibc platforms:
  glibc 2.15, macOS 14, FreeBSD 6.4, NetBSD 10.0, OpenBSD 7.3, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.4.x, mingw, MSVC 9, Android 4.4.
* This function returns 0 when the first argument is NULL in some locales on some platforms:
  AIX 7.2.

Portability problems not fixed by Gnulib:

* This function is only defined as an inline function on some platforms:
  Haiku 2020.

---

Next: [`mbrtoc16`](#mbrtoc16), Previous: [`c32rtomb`](#c32rtomb), Up: [Functions in `<uchar.h>`](#Functions-in-_003cuchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.68.4 `mbrtoc8` [¶](#mbrtoc8-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on most platforms:
  glibc 2.35, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.1, HP-UX 11.31, Solaris 11.4,
  Cygwin,
  mingw, MSVC 14, Android 9.0.

---

Next: [`mbrtoc32`](#mbrtoc32), Previous: [`mbrtoc8`](#mbrtoc8), Up: [Functions in `<uchar.h>`](#Functions-in-_003cuchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.68.5 `mbrtoc16` [¶](#mbrtoc16-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.30.1.3.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mbrtoc16.html>

Gnulib module: mbrtoc16

Portability problems fixed by Gnulib:

* This function is missing on most non-glibc platforms:
  glibc 2.15, macOS 14, FreeBSD 6.4, NetBSD 10.0, OpenBSD 7.3, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.4.x, mingw, MSVC 9, Android 4.4.
* This function may crash when the first argument is NULL on some platforms:
  glibc 2.36.
* In the C or POSIX locales, this function can return `(size_t) -1`
  and set `errno` to `EILSEQ`:
  glibc 2.36.
* This function returns 0 instead of `(size_t) -2` when the input
  is empty:
  glibc 2.19,
  Android 11.
* This function returns the total number of bytes that make up the multibyte
  character, not the number of bytes that were needed to complete the multibyte
  character, on some platforms:
  mingw.
* This function returns `(size_t) -3` instead of a byte count when it
  has stored a high surrogate, and returns a byte count instead of
  `(size_t) -3` when it has stored a low surrogate, on some platforms:
  Android.
* This function does not recognize multibyte sequences that `mbrtowc`
  recognizes on some platforms:
  FreeBSD 13.2,
  Solaris 11.4, MSVC 14.

Portability problems not fixed by Gnulib:

* After `mbrtoc16` returns a `char16_t` value, `mbsinit`
  cannot be used to determine whether the function is ready to return
  another `char16_t` value. To do so, instead call `mbrtoc16`
  again, with an appropriately incremented `const char *` argument
  and an appropriately decremented `size_t` argument.

---

Previous: [`mbrtoc16`](#mbrtoc16), Up: [Functions in `<uchar.h>`](#Functions-in-_003cuchar_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.68.6 `mbrtoc32` [¶](#mbrtoc32-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.30.1.5.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mbrtoc32.html>

Gnulib module: mbrtoc32 or mbrtoc32-regular

Portability problems fixed by either Gnulib module `mbrtoc32` or `mbrtoc32-regular`:

* This function is missing on most non-glibc platforms:
  glibc 2.15, macOS 14, FreeBSD 6.4, NetBSD 10.0, OpenBSD 7.3, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 3.4.x, mingw, MSVC 9, Android 4.4.
* In the C or POSIX locales, this function can return `(size_t) -1`
  and set `errno` to `EILSEQ`:
  glibc 2.35.
* This function returns 0 instead of `(size_t) -2` when the input
  is empty:
  glibc 2.19,
  mingw,
  Android 11,
  Haiku.
* This function does not recognize multibyte sequences that `mbrtowc`
  recognizes on some platforms:
  FreeBSD 13.2,
  Solaris 11.4, mingw, MSVC 14.
* This function does not work when it’s fed the input bytes one-by-one
  on some platforms:
  Cygwin 3.5.3.

Portability problems fixed by Gnulib module `mbrtoc32-regular`:

* This function can map some multibyte characters to a sequence of two or more
  Unicode characters, and may thus return `(size_t) -3`.
  No known implementation currently (2023) behaves that way, but it may
  theoretically happen.
  With the `mbrtoc32-regular` module, you have the guarantee that the
  Gnulib-provided `mbrtoc32` function maps each multibyte character to
  exactly one Unicode character and thus never returns `(size_t) -3`.
* This function behaves incorrectly when converting precomposed characters
  from the BIG5-HKSCS encoding:
  glibc 2.36.

Portability problems not fixed by Gnulib:

* This function is only defined as an inline function on some platforms:
  Haiku 2020.

Note: If you want the guarantee that the `char32_t` values returned
by this function are Unicode code points, you also need to request the
`uchar-h-c23` module.

---

Next: [Functions in `<utmpx.h>`](#Functions-in-_003cutmpx_002eh_003e), Previous: [Functions in `<uchar.h>`](#Functions-in-_003cuchar_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
