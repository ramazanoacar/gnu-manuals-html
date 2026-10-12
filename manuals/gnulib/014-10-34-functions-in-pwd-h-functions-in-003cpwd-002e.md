Next: [`pthread_setspecific`](#pthread_005fsetspecific), Previous: [`pthread_setschedparam`](#pthread_005fsetschedparam), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.95 `pthread_setschedprio` [¶](#pthread_005fsetschedprio-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_setschedprio.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.3, macOS 14, FreeBSD 14.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.9, mingw, MSVC 14, Android 8.1.

---

Next: [`pthread_spin_destroy`](#pthread_005fspin_005fdestroy), Previous: [`pthread_setschedprio`](#pthread_005fsetschedprio), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.96 `pthread_setspecific` [¶](#pthread_005fsetspecific-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_setspecific.html>

Gnulib module: pthread-tss

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_spin_init`](#pthread_005fspin_005finit), Previous: [`pthread_setspecific`](#pthread_005fsetspecific), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.97 `pthread_spin_destroy` [¶](#pthread_005fspin_005fdestroy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_spin_destroy.html>

Gnulib module: pthread-spin

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.9, mingw, MSVC 14, Android 6.0.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_spin_lock`](#pthread_005fspin_005flock), Previous: [`pthread_spin_destroy`](#pthread_005fspin_005fdestroy), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.98 `pthread_spin_init` [¶](#pthread_005fspin_005finit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_spin_init.html>

Gnulib module: pthread-spin

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.9, mingw, MSVC 14, Android 6.0.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_spin_trylock`](#pthread_005fspin_005ftrylock), Previous: [`pthread_spin_init`](#pthread_005fspin_005finit), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.99 `pthread_spin_lock` [¶](#pthread_005fspin_005flock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_spin_lock.html>

Gnulib module: pthread-spin

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.9, mingw, MSVC 14, Android 6.0.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_spin_unlock`](#pthread_005fspin_005funlock), Previous: [`pthread_spin_lock`](#pthread_005fspin_005flock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.100 `pthread_spin_trylock` [¶](#pthread_005fspin_005ftrylock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_spin_trylock.html>

Gnulib module: pthread-spin

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.9, mingw, MSVC 14, Android 6.0.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_testcancel`](#pthread_005ftestcancel), Previous: [`pthread_spin_trylock`](#pthread_005fspin_005ftrylock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.101 `pthread_spin_unlock` [¶](#pthread_005fspin_005funlock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_spin_unlock.html>

Gnulib module: pthread-spin

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.9, mingw, MSVC 14, Android 6.0.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Previous: [`pthread_spin_unlock`](#pthread_005fspin_005funlock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.102 `pthread_testcancel` [¶](#pthread_005ftestcancel-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_testcancel.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [Functions in `<regex.h>`](#Functions-in-_003cregex_002eh_003e), Previous: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.34 Functions in `<pwd.h>` [¶](#Functions-in-_003cpwd_002eh_003e-1)

* [`endpwent`](#endpwent)
* [`getpwent`](#getpwent)
* [`getpwnam`](#getpwnam)
* [`getpwnam_r`](#getpwnam_005fr)
* [`getpwuid`](#getpwuid)
* [`getpwuid_r`](#getpwuid_005fr)
* [`setpwent`](#setpwent)

---

Next: [`getpwent`](#getpwent), Up: [Functions in `<pwd.h>`](#Functions-in-_003cpwd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.34.1 `endpwent` [¶](#endpwent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/endpwent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`getpwnam`](#getpwnam), Previous: [`endpwent`](#endpwent), Up: [Functions in `<pwd.h>`](#Functions-in-_003cpwd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.34.2 `getpwent` [¶](#getpwent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getpwent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 7.1.

---

Next: [`getpwnam_r`](#getpwnam_005fr), Previous: [`getpwent`](#getpwent), Up: [Functions in `<pwd.h>`](#Functions-in-_003cpwd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.34.3 `getpwnam` [¶](#getpwnam-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getpwnam.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`getpwuid`](#getpwuid), Previous: [`getpwnam`](#getpwnam), Up: [Functions in `<pwd.h>`](#Functions-in-_003cpwd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.34.4 `getpwnam_r` [¶](#getpwnam_005fr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getpwnam_r.html>

Gnulib module: extensions

Portability problems fixed by Gnulib:

* This function has an incompatible declaration on some platforms:
  Solaris 11.3 (when `_POSIX_PTHREAD_SEMANTICS` is not defined).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, mingw, MSVC 14, Android 3.0.

---

Next: [`getpwuid_r`](#getpwuid_005fr), Previous: [`getpwnam_r`](#getpwnam_005fr), Up: [Functions in `<pwd.h>`](#Functions-in-_003cpwd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.34.5 `getpwuid` [¶](#getpwuid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getpwuid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is unsafe to call between `fork` and `exec` if
  the parent process is multi-threaded. Instead, use `getpwuid_r`
  prior to forking.
* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`setpwent`](#setpwent), Previous: [`getpwuid`](#getpwuid), Up: [Functions in `<pwd.h>`](#Functions-in-_003cpwd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.34.6 `getpwuid_r` [¶](#getpwuid_005fr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getpwuid_r.html>

Gnulib module: extensions

Portability problems fixed by Gnulib:

* This function has an incompatible declaration on some platforms:
  Solaris 11.3 (when `_POSIX_PTHREAD_SEMANTICS` is not defined).

Portability problems not fixed by Gnulib:

* This function is unsafe to call between `fork` and `exec` if
  the parent process is multi-threaded. Use it prior to forking.
* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, mingw, MSVC 14, Android 3.0.

---

Previous: [`getpwuid_r`](#getpwuid_005fr), Up: [Functions in `<pwd.h>`](#Functions-in-_003cpwd_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.34.7 `setpwent` [¶](#setpwent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setpwent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 7.1.

---

Next: [Functions in `<sched.h>`](#Functions-in-_003csched_002eh_003e), Previous: [Functions in `<pwd.h>`](#Functions-in-_003cpwd_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.35 Functions in `<regex.h>` [¶](#Functions-in-_003cregex_002eh_003e-1)

* [`regcomp`](#regcomp)
* [`regerror`](#regerror)
* [`regexec`](#regexec)
* [`regfree`](#regfree)

---

Next: [`regerror`](#regerror), Up: [Functions in `<regex.h>`](#Functions-in-_003cregex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.35.1 `regcomp` [¶](#regcomp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/regcomp.html>

Gnulib module: regex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* Many regular expression implementations have bugs.

Portability problems not fixed by Gnulib:

---

Next: [`regexec`](#regexec), Previous: [`regcomp`](#regcomp), Up: [Functions in `<regex.h>`](#Functions-in-_003cregex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.35.2 `regerror` [¶](#regerror-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/regerror.html>

Gnulib module: regex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`regfree`](#regfree), Previous: [`regerror`](#regerror), Up: [Functions in `<regex.h>`](#Functions-in-_003cregex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.35.3 `regexec` [¶](#regexec-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/regexec.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-regexec-2.html>

Gnulib module: regex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* Many regular expression implementations have bugs.

Portability problems not fixed by Gnulib:

---

Previous: [`regexec`](#regexec), Up: [Functions in `<regex.h>`](#Functions-in-_003cregex_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.35.4 `regfree` [¶](#regfree-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/regfree.html>

Gnulib module: regex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [Functions in `<search.h>`](#Functions-in-_003csearch_002eh_003e), Previous: [Functions in `<regex.h>`](#Functions-in-_003cregex_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.36 Functions in `<sched.h>` [¶](#Functions-in-_003csched_002eh_003e-1)

* [`sched_get_priority_max`](#sched_005fget_005fpriority_005fmax)
* [`sched_get_priority_min`](#sched_005fget_005fpriority_005fmin)
* [`sched_getparam`](#sched_005fgetparam)
* [`sched_getscheduler`](#sched_005fgetscheduler)
* [`sched_rr_get_interval`](#sched_005frr_005fget_005finterval)
* [`sched_setparam`](#sched_005fsetparam)
* [`sched_setscheduler`](#sched_005fsetscheduler)
* [`sched_yield`](#sched_005fyield)

---

Next: [`sched_get_priority_min`](#sched_005fget_005fpriority_005fmin), Up: [Functions in `<sched.h>`](#Functions-in-_003csched_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.36.1 `sched_get_priority_max` [¶](#sched_005fget_005fpriority_005fmax-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_get_priority_max.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, HP-UX 11, mingw, MSVC 14.

---

Next: [`sched_getparam`](#sched_005fgetparam), Previous: [`sched_get_priority_max`](#sched_005fget_005fpriority_005fmax), Up: [Functions in `<sched.h>`](#Functions-in-_003csched_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.36.2 `sched_get_priority_min` [¶](#sched_005fget_005fpriority_005fmin-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_get_priority_min.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, HP-UX 11, mingw, MSVC 14.

---

Next: [`sched_getscheduler`](#sched_005fgetscheduler), Previous: [`sched_get_priority_min`](#sched_005fget_005fpriority_005fmin), Up: [Functions in `<sched.h>`](#Functions-in-_003csched_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.36.3 `sched_getparam` [¶](#sched_005fgetparam-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_getparam.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.
* This function always fails with error `ENOSYS` on some platforms:
  musl libc.

---

Next: [`sched_rr_get_interval`](#sched_005frr_005fget_005finterval), Previous: [`sched_getparam`](#sched_005fgetparam), Up: [Functions in `<sched.h>`](#Functions-in-_003csched_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.36.4 `sched_getscheduler` [¶](#sched_005fgetscheduler-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_getscheduler.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.
* This function always fails with error `ENOSYS` on some platforms:
  musl libc.

---

Next: [`sched_setparam`](#sched_005fsetparam), Previous: [`sched_getscheduler`](#sched_005fgetscheduler), Up: [Functions in `<sched.h>`](#Functions-in-_003csched_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.36.5 `sched_rr_get_interval` [¶](#sched_005frr_005fget_005finterval-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_rr_get_interval.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, mingw, MSVC 14.

---

Next: [`sched_setscheduler`](#sched_005fsetscheduler), Previous: [`sched_rr_get_interval`](#sched_005frr_005fget_005finterval), Up: [Functions in `<sched.h>`](#Functions-in-_003csched_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.36.6 `sched_setparam` [¶](#sched_005fsetparam-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_setparam.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.
* This function always fails with error `ENOSYS` on some platforms:
  musl libc.

---

Next: [`sched_yield`](#sched_005fyield), Previous: [`sched_setparam`](#sched_005fsetparam), Up: [Functions in `<sched.h>`](#Functions-in-_003csched_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.36.7 `sched_setscheduler` [¶](#sched_005fsetscheduler-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_setscheduler.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-sched-setscheduler.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.
* This function always fails with error `ENOSYS` on some platforms:
  musl libc.

---

Previous: [`sched_setscheduler`](#sched_005fsetscheduler), Up: [Functions in `<sched.h>`](#Functions-in-_003csched_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.36.8 `sched_yield` [¶](#sched_005fyield-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_yield.html>

Gnulib module: sched\_yield

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [Functions in `<semaphore.h>`](#Functions-in-_003csemaphore_002eh_003e), Previous: [Functions in `<sched.h>`](#Functions-in-_003csched_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.37 Functions in `<search.h>` [¶](#Functions-in-_003csearch_002eh_003e-1)

* [`hcreate`](#hcreate)
* [`hdestroy`](#hdestroy)
* [`hsearch`](#hsearch)
* [`insque`](#insque)
* [`lfind`](#lfind)
* [`lsearch`](#lsearch)
* [`remque`](#remque)
* [`tdelete`](#tdelete)
* [`tfind`](#tfind)
* [`tsearch`](#tsearch)
* [`twalk`](#twalk)

---

Next: [`hdestroy`](#hdestroy), Up: [Functions in `<search.h>`](#Functions-in-_003csearch_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.37.1 `hcreate` [¶](#hcreate-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/hcreate.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 8.1.

---

Next: [`hsearch`](#hsearch), Previous: [`hcreate`](#hcreate), Up: [Functions in `<search.h>`](#Functions-in-_003csearch_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.37.2 `hdestroy` [¶](#hdestroy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/hdestroy.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 8.1.

---

Next: [`insque`](#insque), Previous: [`hdestroy`](#hdestroy), Up: [Functions in `<search.h>`](#Functions-in-_003csearch_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.37.3 `hsearch` [¶](#hsearch-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/hsearch.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 8.1.

---

Next: [`lfind`](#lfind), Previous: [`hsearch`](#hsearch), Up: [Functions in `<search.h>`](#Functions-in-_003csearch_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.37.4 `insque` [¶](#insque-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/insque.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 4.4.

---

Next: [`lsearch`](#lsearch), Previous: [`insque`](#insque), Up: [Functions in `<search.h>`](#Functions-in-_003csearch_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.37.5 `lfind` [¶](#lfind-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lfind.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Android 4.4.

---

Next: [`remque`](#remque), Previous: [`lfind`](#lfind), Up: [Functions in `<search.h>`](#Functions-in-_003csearch_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.37.6 `lsearch` [¶](#lsearch-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/lsearch.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Android 4.4.

---

Next: [`tdelete`](#tdelete), Previous: [`lsearch`](#lsearch), Up: [Functions in `<search.h>`](#Functions-in-_003csearch_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.37.7 `remque` [¶](#remque-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/remque.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 4.4.

---

Next: [`tfind`](#tfind), Previous: [`remque`](#remque), Up: [Functions in `<search.h>`](#Functions-in-_003csearch_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.37.8 `tdelete` [¶](#tdelete-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tdelete.html>

Gnulib module: tsearch

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.0.4.
* `tdelete` returns `NULL` when removing the last element of a tree
  on some platforms:
  OpenBSD 4.0.

Portability problems not fixed by Gnulib:

---

Next: [`tsearch`](#tsearch), Previous: [`tdelete`](#tdelete), Up: [Functions in `<search.h>`](#Functions-in-_003csearch_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.37.9 `tfind` [¶](#tfind-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tfind.html>

Gnulib module: tsearch

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.0.4.

Portability problems not fixed by Gnulib:

* This function uses `void **` instead of `void *const *` for
  the second parameter on some platforms:
  Cygwin.

---

Next: [`twalk`](#twalk), Previous: [`tfind`](#tfind), Up: [Functions in `<search.h>`](#Functions-in-_003csearch_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.37.10 `tsearch` [¶](#tsearch-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/tsearch.html>

Gnulib module: tsearch

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.0.4.

Portability problems not fixed by Gnulib:

---

Previous: [`tsearch`](#tsearch), Up: [Functions in `<search.h>`](#Functions-in-_003csearch_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.37.11 `twalk` [¶](#twalk-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/twalk.html>

Gnulib module: tsearch

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.4.

Portability problems not fixed by Gnulib:

---

Next: [Functions in `<setjmp.h>`](#Functions-in-_003csetjmp_002eh_003e), Previous: [Functions in `<search.h>`](#Functions-in-_003csearch_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.38 Functions in `<semaphore.h>` [¶](#Functions-in-_003csemaphore_002eh_003e-1)

* [`sem_clockwait`](#sem_005fclockwait)
* [`sem_close`](#sem_005fclose)
* [`sem_destroy`](#sem_005fdestroy)
* [`sem_getvalue`](#sem_005fgetvalue)
* [`sem_init`](#sem_005finit)
* [`sem_open`](#sem_005fopen)
* [`sem_post`](#sem_005fpost)
* [`sem_timedwait`](#sem_005ftimedwait)
* [`sem_trywait`](#sem_005ftrywait)
* [`sem_unlink`](#sem_005funlink)
* [`sem_wait`](#sem_005fwait)

---

Next: [`sem_close`](#sem_005fclose), Up: [Functions in `<semaphore.h>`](#Functions-in-_003csemaphore_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.38.1 `sem_clockwait` [¶](#sem_005fclockwait-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_clockwait.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Non_002dPOSIX-Extensions.html>,

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 3.2.x, mingw, MSVC 14, Android API level 29.

---

Next: [`sem_destroy`](#sem_005fdestroy), Previous: [`sem_clockwait`](#sem_005fclockwait), Up: [Functions in `<semaphore.h>`](#Functions-in-_003csemaphore_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.38.2 `sem_close` [¶](#sem_005fclose-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_close.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`sem_getvalue`](#sem_005fgetvalue), Previous: [`sem_close`](#sem_005fclose), Up: [Functions in `<semaphore.h>`](#Functions-in-_003csemaphore_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.38.3 `sem_destroy` [¶](#sem_005fdestroy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_destroy.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* This function is not implemented on some platforms:
  Mac OS X 10.11.

---

Next: [`sem_init`](#sem_005finit), Previous: [`sem_destroy`](#sem_005fdestroy), Up: [Functions in `<semaphore.h>`](#Functions-in-_003csemaphore_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.38.4 `sem_getvalue` [¶](#sem_005fgetvalue-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_getvalue.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* This function is not implemented on some platforms:
  Mac OS X 10.11.

---

Next: [`sem_open`](#sem_005fopen), Previous: [`sem_getvalue`](#sem_005fgetvalue), Up: [Functions in `<semaphore.h>`](#Functions-in-_003csemaphore_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.38.5 `sem_init` [¶](#sem_005finit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_init.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* This function is not implemented on some platforms:
  Mac OS X 10.11.

---

Next: [`sem_post`](#sem_005fpost), Previous: [`sem_init`](#sem_005finit), Up: [Functions in `<semaphore.h>`](#Functions-in-_003csemaphore_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.38.6 `sem_open` [¶](#sem_005fopen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_open.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`sem_timedwait`](#sem_005ftimedwait), Previous: [`sem_open`](#sem_005fopen), Up: [Functions in `<semaphore.h>`](#Functions-in-_003csemaphore_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.38.7 `sem_post` [¶](#sem_005fpost-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_post.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`sem_trywait`](#sem_005ftrywait), Previous: [`sem_post`](#sem_005fpost), Up: [Functions in `<semaphore.h>`](#Functions-in-_003csemaphore_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.38.8 `sem_timedwait` [¶](#sem_005ftimedwait-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_timedwait.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, mingw, MSVC 14.

---

Next: [`sem_unlink`](#sem_005funlink), Previous: [`sem_timedwait`](#sem_005ftimedwait), Up: [Functions in `<semaphore.h>`](#Functions-in-_003csemaphore_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.38.9 `sem_trywait` [¶](#sem_005ftrywait-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_trywait.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`sem_wait`](#sem_005fwait), Previous: [`sem_trywait`](#sem_005ftrywait), Up: [Functions in `<semaphore.h>`](#Functions-in-_003csemaphore_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.38.10 `sem_unlink` [¶](#sem_005funlink-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_unlink.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14.

---

Previous: [`sem_unlink`](#sem_005funlink), Up: [Functions in `<semaphore.h>`](#Functions-in-_003csemaphore_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.38.11 `sem_wait` [¶](#sem_005fwait-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_wait.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e), Previous: [Functions in `<semaphore.h>`](#Functions-in-_003csemaphore_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.39 Functions in `<setjmp.h>` [¶](#Functions-in-_003csetjmp_002eh_003e-1)

* [`longjmp`](#longjmp)
* [`setjmp`](#setjmp)
* [`siglongjmp`](#siglongjmp)
* [`sigsetjmp`](#sigsetjmp)

---

Next: [`setjmp`](#setjmp), Up: [Functions in `<setjmp.h>`](#Functions-in-_003csetjmp_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.39.1 `longjmp` [¶](#longjmp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/longjmp.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* The effects of this call are system and compiler optimization dependent,
  since it restores the contents of register-allocated variables but not
  the contents of stack-allocated variables.
* When longjumping out of a signal handler that was being executed on an
  alternate stack (installed through `sigaltstack`),
  on Hurd, FreeBSD, NetBSD, OpenBSD,
  you need to clear the `SS_ONSTACK` flag in the `stack_t`
  structure managed by the kernel.
* This function may crash when the invoker code was compiled with option
  `_FORTIFY_SOURCE >= 2`
  on Hurd.

---

Next: [`siglongjmp`](#siglongjmp), Previous: [`longjmp`](#longjmp), Up: [Functions in `<setjmp.h>`](#Functions-in-_003csetjmp_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.39.2 `setjmp` [¶](#setjmp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setjmp.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* POSIX does not specify whether `setjmp` saves the signal mask in the
  `jmp_buf`. It does on BSD platforms, and on glibc platforms when
  `_BSD_SOURCE` (and/or `_DEFAULT_SOURCE` on glibc >= 2.19) is defined;
  in this case `setjmp` behaves like `sigsetjmp(.,1)`, and functions
  `_setjmp` and `_longjmp` are available that don’t save or restore
  the signal mask. On System V platforms (excluding HP-UX), and on glibc
  platforms by default, `setjmp` doesn’t save the signal mask.

---

Next: [`sigsetjmp`](#sigsetjmp), Previous: [`setjmp`](#setjmp), Up: [Functions in `<setjmp.h>`](#Functions-in-_003csetjmp_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.39.3 `siglongjmp` [¶](#siglongjmp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/siglongjmp.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* This is only provided as a macro on some platforms:
  Cygwin.

---

Previous: [`siglongjmp`](#siglongjmp), Up: [Functions in `<setjmp.h>`](#Functions-in-_003csetjmp_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.39.4 `sigsetjmp` [¶](#sigsetjmp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigsetjmp.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e), Previous: [Functions in `<setjmp.h>`](#Functions-in-_003csetjmp_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.40 Functions in `<signal.h>` [¶](#Functions-in-_003csignal_002eh_003e-1)

* [`kill`](#kill)
* [`killpg`](#killpg)
* [`psiginfo`](#psiginfo)
* [`psignal`](#psignal)
* [`pthread_kill`](#pthread_005fkill)
* [`pthread_sigmask`](#pthread_005fsigmask)
* [`raise`](#raise)
* [`sig2str`](#sig2str)
* [`sigaction`](#sigaction)
* [`sigaddset`](#sigaddset)
* [`sigaltstack`](#sigaltstack)
* [`sigdelset`](#sigdelset)
* [`sigemptyset`](#sigemptyset)
* [`sigfillset`](#sigfillset)
* [`sigismember`](#sigismember)
* [`signal`](#signal)
* [`sigpending`](#sigpending)
* [`sigprocmask`](#sigprocmask)
* [`sigqueue`](#sigqueue)
* [`sigsuspend`](#sigsuspend)
* [`sigtimedwait`](#sigtimedwait)
* [`sigwait`](#sigwait)
* [`sigwaitinfo`](#sigwaitinfo)
* [`str2sig`](#str2sig)

---

Next: [`killpg`](#killpg), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.1 `kill` [¶](#kill-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/kill.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-kill-3.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`psiginfo`](#psiginfo), Previous: [`kill`](#kill), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.2 `killpg` [¶](#killpg-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/killpg.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`psignal`](#psignal), Previous: [`killpg`](#killpg), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.3 `psiginfo` [¶](#psiginfo-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/psiginfo.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 1.7.9, mingw, MSVC 14, Android 4.1.

---

Next: [`pthread_kill`](#pthread_005fkill), Previous: [`psiginfo`](#psiginfo), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.4 `psignal` [¶](#psignal-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/psignal.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11, Cygwin 1.7.9, mingw, MSVC 14, Android API level 16.
* This function is declared in `<siginfo.h>`, not `<signal.h>`,
  on some platforms:
  Solaris 11.3.
* The first parameter is of type `unsigned int`, not `int`,
  on some platforms:
  macOS 13, FreeBSD 10.4, NetBSD 5.1, OpenBSD 7.9.

---

Next: [`pthread_sigmask`](#pthread_005fsigmask), Previous: [`psignal`](#psignal), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.5 `pthread_kill` [¶](#pthread_005fkill-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_kill.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`raise`](#raise), Previous: [`pthread_kill`](#pthread_005fkill), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.6 `pthread_sigmask` [¶](#pthread_005fsigmask-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_sigmask.html>

Gnulib module: pthread\_sigmask

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* This function is declared in `<pthread.h>` instead of `<signal.h>`
  on some platforms:
  FreeBSD 6.4, OpenBSD 3.8.
* This function requires compiling with `-pthread` and/or
  linking with `-lpthread` on some platforms:
  glibc 2.31, NetBSD, OpenBSD 5.8.
* This function does nothing and always returns 0 in programs that are not
  linked with `-lpthread` on some platforms:
  FreeBSD 14.0, MidnightBSD 1.1, HP-UX 11.31, Solaris 9.
* When it fails, this function returns −1 instead of the error number on
  some platforms:
  Cygwin 1.7.5,
  NetBSD 9.3 when libpthread is not in use.

Portability problems not fixed by Gnulib:

* On platforms that do not natively support this function,
  it has unspecified behavior in a multi-threaded process.
  For example, the Gnulib replacement on native MS-Windows can affect
  the signal masks of other threads, which can lead to race conditions.
* The Gnulib replacement on native MS-Windows is not async-signal-safe
  (see
  [POSIX section “Signal Actions”](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_04_03)).

---

Next: [`sig2str`](#sig2str), Previous: [`pthread_sigmask`](#pthread_005fsigmask), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.7 `raise` [¶](#raise-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/raise.html>

Gnulib module: raise

Portability problems fixed by Gnulib:

* This function is missing on some old platforms.
* This function crashes when invoked with invalid arguments on some platforms:
  MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`sigaction`](#sigaction), Previous: [`raise`](#raise), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.8 `sig2str` [¶](#sig2str-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sig2str.html>

Gnulib module: sig2str

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Cygwin 3.2.x, mingw, MSVC 14, Android API level 35.

Portability problems not fixed by Gnulib:

* This function does not map the argument 0 to `"EXIT"` on some platforms:
  FreeBSD 15.0.

---

Next: [`sigaddset`](#sigaddset), Previous: [`sig2str`](#sig2str), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.9 `sigaction` [¶](#sigaction-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html>

Gnulib module: sigaction

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* POSIX recommends that when specifying SA\_RESETHAND, SA\_NODEFER must
  also be specified.
* Support for SA\_ONSTACK is missing on some platforms:
  mingw, MSVC 14, cygwin.
* Support for SA\_SIGINFO is missing on some platforms:
  mingw, MSVC 14.
* Support for SIGCHLD, and thus for SA\_NOCLDSTOP and SA\_NOCLDWAIT, is
  missing on some platforms:
  mingw, MSVC 14.
* Support for SA\_RESETHAND is missing on some platforms:
  NonStop.
* Support for SA\_RESTART is missing on some platforms:
  mingw, MSVC 14, NonStop.
* The symbolic value `SIG_IGN` for the `SIGCHLD` signal is equivalent
  to a signal handler

  ```
  void handle_child (int sigchld)
  {
    while (waitpid (-1, NULL, WNOHANG) > 0)
      ;
  }
  ```

  except that `SIG_IGN` for `SIGCHLD` has the effect that the children
  execution times are not accounted in the `times` function.
  On some platforms (BSD? SystemV? Linux?), you need to use the `sigaction`
  flag `SA_NOCLDWAIT` in order to obtain this behavior.
* The Gnulib replacement on native MS-Windows can affect the signal
  masks of other threads, either when `sigaction` is called
  or when a signal handler is called, and this can lead to race conditions.
* The Gnulib replacement on native MS-Windows is not async-signal-safe
  (see
  [POSIX section “Signal Actions”](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_04_03)).

---

Next: [`sigaltstack`](#sigaltstack), Previous: [`sigaction`](#sigaction), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.10 `sigaddset` [¶](#sigaddset-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaddset.html>

Gnulib module: sigprocmask

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

---

Next: [`sigdelset`](#sigdelset), Previous: [`sigaddset`](#sigaddset), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.11 `sigaltstack` [¶](#sigaltstack-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaltstack.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 1.7.x, mingw, MSVC 14.
* `sigaltstack` doesn’t work on HP-UX 11/IA-64 and
  OpenBSD 3.6/Sparc64.

---

Next: [`sigemptyset`](#sigemptyset), Previous: [`sigaltstack`](#sigaltstack), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.12 `sigdelset` [¶](#sigdelset-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigdelset.html>

Gnulib module: sigprocmask

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

---

Next: [`sigfillset`](#sigfillset), Previous: [`sigdelset`](#sigdelset), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.13 `sigemptyset` [¶](#sigemptyset-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigemptyset.html>

Gnulib module: sigprocmask

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

---

Next: [`sigismember`](#sigismember), Previous: [`sigemptyset`](#sigemptyset), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.14 `sigfillset` [¶](#sigfillset-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigfillset.html>

Gnulib module: sigprocmask

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

---

Next: [`signal`](#signal), Previous: [`sigfillset`](#sigfillset), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.15 `sigismember` [¶](#sigismember-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigismember.html>

Gnulib module: sigprocmask

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 4.4.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

---

Next: [`sigpending`](#sigpending), Previous: [`sigismember`](#sigismember), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.16 `signal` [¶](#signal-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/signal.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function crashes when invoked with invalid arguments on some platforms:
  MSVC 14.
* On System V platforms, when the signal is triggered, the kernel uninstalls the
  handler (i.e. resets the signal’s action to SIG\_DFL) before invoking the
  handler. This opens the door to race conditions: undesired things happen
  if the signal is triggered twice and the signal handler was not quick enough
  reinstalling itself as a handler. On BSD platforms and glibc platforms, on the
  other hand, when the signal is triggered, the kernel blocks the signal
  before invoking the handler. This is saner, but POSIX still allows either
  behavior. To avoid this problem, use `sigaction` instead of
  `signal`.

---

Next: [`sigprocmask`](#sigprocmask), Previous: [`signal`](#signal), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.17 `sigpending` [¶](#sigpending-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigpending.html>

Gnulib module: sigprocmask

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`sigqueue`](#sigqueue), Previous: [`sigpending`](#sigpending), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.18 `sigprocmask` [¶](#sigprocmask-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigprocmask.html>

Gnulib module: sigprocmask

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* In case of failure, the return value is wrong on some platforms:
  NetBSD 10.0 when libpthread is in use.
* POSIX says that in multi-threaded processes `sigprocmask` has
  unspecified behavior so `pthread_sigmask` should be used instead.
  On most platforms, `sigprocmask` is essentially equivalent to
  `pthread_sigmask`, with only a difference regarding the error
  return convention.
  However, on macOS and on the Gnulib replacement on native MS-Windows,
  `sigprocmask` can affect the signal masks of other threads,
  which can lead to race conditions.
* The Gnulib replacement on native MS-Windows is not async-signal-safe
  (see
  [POSIX section “Signal Actions”](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_04_03)).

Note: In single-threaded programs it’s simpler to use
`sigprocmask`, since it does not require compiling with
`-pthread` and/or linking with `-lpthread` on some platforms:
AIX.

---

Next: [`sigsuspend`](#sigsuspend), Previous: [`sigprocmask`](#sigprocmask), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.19 `sigqueue` [¶](#sigqueue-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigqueue.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14, Android 5.1.

---

Next: [`sigtimedwait`](#sigtimedwait), Previous: [`sigqueue`](#sigqueue), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.20 `sigsuspend` [¶](#sigsuspend-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigsuspend.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`sigwait`](#sigwait), Previous: [`sigsuspend`](#sigsuspend), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.21 `sigtimedwait` [¶](#sigtimedwait-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigtimedwait.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9.x, mingw, MSVC 14, Android 5.1.
* Linux implements the meaning of NULL timeout by doing what `sigwaitinfo`
  does; other platforms may not do the same.

---

Next: [`sigwaitinfo`](#sigwaitinfo), Previous: [`sigtimedwait`](#sigtimedwait), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.22 `sigwait` [¶](#sigwait-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigwait.html>

Gnulib module: extensions

Portability problems fixed by Gnulib:

* This function has an incompatible declaration on some platforms:
  Solaris 11.4 (when `_POSIX_PTHREAD_SEMANTICS` is not defined).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* On Linux/glibc platforms before the advent of NPTL, signals could only be
  sent to one particular thread. In POSIX, signals are sent to the entire
  process and executed by any thread of the process that happens to have the
  particular signal currently unblocked.

---

Next: [`str2sig`](#str2sig), Previous: [`sigwait`](#sigwait), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.23 `sigwaitinfo` [¶](#sigwaitinfo-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigwaitinfo.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14, Android 5.1.

---

Previous: [`sigwaitinfo`](#sigwaitinfo), Up: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.40.24 `str2sig` [¶](#str2sig-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/str2sig.html>

Gnulib module: sig2str

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Cygwin 3.2.x, mingw, MSVC 14, Android API level 35.

Portability problems not fixed by Gnulib:

---

Next: [Functions in `<stdarg.h>`](#Functions-in-_003cstdarg_002eh_003e), Previous: [Functions in `<signal.h>`](#Functions-in-_003csignal_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
