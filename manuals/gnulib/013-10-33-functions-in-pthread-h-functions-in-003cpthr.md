### 10.33 Functions in `<pthread.h>` [¶](#Functions-in-_003cpthread_002eh_003e-1)

* [`pthread_atfork`](#pthread_005fatfork)
* [`pthread_attr_destroy`](#pthread_005fattr_005fdestroy)
* [`pthread_attr_getdetachstate`](#pthread_005fattr_005fgetdetachstate)
* [`pthread_attr_getguardsize`](#pthread_005fattr_005fgetguardsize)
* [`pthread_attr_getinheritsched`](#pthread_005fattr_005fgetinheritsched)
* [`pthread_attr_getschedparam`](#pthread_005fattr_005fgetschedparam)
* [`pthread_attr_getschedpolicy`](#pthread_005fattr_005fgetschedpolicy)
* [`pthread_attr_getscope`](#pthread_005fattr_005fgetscope)
* [`pthread_attr_getstack`](#pthread_005fattr_005fgetstack)
* [`pthread_attr_getstacksize`](#pthread_005fattr_005fgetstacksize)
* [`pthread_attr_init`](#pthread_005fattr_005finit)
* [`pthread_attr_setdetachstate`](#pthread_005fattr_005fsetdetachstate)
* [`pthread_attr_setguardsize`](#pthread_005fattr_005fsetguardsize)
* [`pthread_attr_setinheritsched`](#pthread_005fattr_005fsetinheritsched)
* [`pthread_attr_setschedparam`](#pthread_005fattr_005fsetschedparam)
* [`pthread_attr_setschedpolicy`](#pthread_005fattr_005fsetschedpolicy)
* [`pthread_attr_setscope`](#pthread_005fattr_005fsetscope)
* [`pthread_attr_setstack`](#pthread_005fattr_005fsetstack)
* [`pthread_attr_setstacksize`](#pthread_005fattr_005fsetstacksize)
* [`pthread_barrier_destroy`](#pthread_005fbarrier_005fdestroy)
* [`pthread_barrier_init`](#pthread_005fbarrier_005finit)
* [`pthread_barrier_wait`](#pthread_005fbarrier_005fwait)
* [`pthread_barrierattr_destroy`](#pthread_005fbarrierattr_005fdestroy)
* [`pthread_barrierattr_getpshared`](#pthread_005fbarrierattr_005fgetpshared)
* [`pthread_barrierattr_init`](#pthread_005fbarrierattr_005finit)
* [`pthread_barrierattr_setpshared`](#pthread_005fbarrierattr_005fsetpshared)
* [`pthread_cancel`](#pthread_005fcancel)
* [`pthread_cleanup_pop`](#pthread_005fcleanup_005fpop)
* [`pthread_cleanup_push`](#pthread_005fcleanup_005fpush)
* [`pthread_cond_broadcast`](#pthread_005fcond_005fbroadcast)
* [`pthread_cond_clockwait`](#pthread_005fcond_005fclockwait)
* [`pthread_cond_destroy`](#pthread_005fcond_005fdestroy)
* [`pthread_cond_init`](#pthread_005fcond_005finit)
* [`pthread_cond_signal`](#pthread_005fcond_005fsignal)
* [`pthread_cond_timedwait`](#pthread_005fcond_005ftimedwait)
* [`pthread_cond_wait`](#pthread_005fcond_005fwait)
* [`pthread_condattr_destroy`](#pthread_005fcondattr_005fdestroy)
* [`pthread_condattr_getclock`](#pthread_005fcondattr_005fgetclock)
* [`pthread_condattr_getpshared`](#pthread_005fcondattr_005fgetpshared)
* [`pthread_condattr_init`](#pthread_005fcondattr_005finit)
* [`pthread_condattr_setclock`](#pthread_005fcondattr_005fsetclock)
* [`pthread_condattr_setpshared`](#pthread_005fcondattr_005fsetpshared)
* [`pthread_create`](#pthread_005fcreate)
* [`pthread_detach`](#pthread_005fdetach)
* [`pthread_equal`](#pthread_005fequal)
* [`pthread_exit`](#pthread_005fexit)
* [`pthread_getcpuclockid`](#pthread_005fgetcpuclockid)
* [`pthread_getschedparam`](#pthread_005fgetschedparam)
* [`pthread_getspecific`](#pthread_005fgetspecific)
* [`pthread_join`](#pthread_005fjoin)
* [`pthread_key_create`](#pthread_005fkey_005fcreate)
* [`pthread_key_delete`](#pthread_005fkey_005fdelete)
* [`pthread_mutex_clocklock`](#pthread_005fmutex_005fclocklock)
* [`pthread_mutex_consistent`](#pthread_005fmutex_005fconsistent)
* [`pthread_mutex_destroy`](#pthread_005fmutex_005fdestroy)
* [`pthread_mutex_getprioceiling`](#pthread_005fmutex_005fgetprioceiling)
* [`pthread_mutex_init`](#pthread_005fmutex_005finit)
* [`pthread_mutex_lock`](#pthread_005fmutex_005flock)
* [`pthread_mutex_setprioceiling`](#pthread_005fmutex_005fsetprioceiling)
* [`pthread_mutex_timedlock`](#pthread_005fmutex_005ftimedlock)
* [`pthread_mutex_trylock`](#pthread_005fmutex_005ftrylock)
* [`pthread_mutex_unlock`](#pthread_005fmutex_005funlock)
* [`pthread_mutexattr_destroy`](#pthread_005fmutexattr_005fdestroy)
* [`pthread_mutexattr_getprioceiling`](#pthread_005fmutexattr_005fgetprioceiling)
* [`pthread_mutexattr_getprotocol`](#pthread_005fmutexattr_005fgetprotocol)
* [`pthread_mutexattr_getpshared`](#pthread_005fmutexattr_005fgetpshared)
* [`pthread_mutexattr_getrobust`](#pthread_005fmutexattr_005fgetrobust)
* [`pthread_mutexattr_gettype`](#pthread_005fmutexattr_005fgettype)
* [`pthread_mutexattr_init`](#pthread_005fmutexattr_005finit)
* [`pthread_mutexattr_setprioceiling`](#pthread_005fmutexattr_005fsetprioceiling)
* [`pthread_mutexattr_setprotocol`](#pthread_005fmutexattr_005fsetprotocol)
* [`pthread_mutexattr_setpshared`](#pthread_005fmutexattr_005fsetpshared)
* [`pthread_mutexattr_setrobust`](#pthread_005fmutexattr_005fsetrobust)
* [`pthread_mutexattr_settype`](#pthread_005fmutexattr_005fsettype)
* [`pthread_once`](#pthread_005fonce)
* [`pthread_rwlock_clockrdlock`](#pthread_005frwlock_005fclockrdlock)
* [`pthread_rwlock_clockwrlock`](#pthread_005frwlock_005fclockwrlock)
* [`pthread_rwlock_destroy`](#pthread_005frwlock_005fdestroy)
* [`pthread_rwlock_init`](#pthread_005frwlock_005finit)
* [`pthread_rwlock_rdlock`](#pthread_005frwlock_005frdlock)
* [`pthread_rwlock_timedrdlock`](#pthread_005frwlock_005ftimedrdlock)
* [`pthread_rwlock_timedwrlock`](#pthread_005frwlock_005ftimedwrlock)
* [`pthread_rwlock_tryrdlock`](#pthread_005frwlock_005ftryrdlock)
* [`pthread_rwlock_trywrlock`](#pthread_005frwlock_005ftrywrlock)
* [`pthread_rwlock_unlock`](#pthread_005frwlock_005funlock)
* [`pthread_rwlock_wrlock`](#pthread_005frwlock_005fwrlock)
* [`pthread_rwlockattr_destroy`](#pthread_005frwlockattr_005fdestroy)
* [`pthread_rwlockattr_getpshared`](#pthread_005frwlockattr_005fgetpshared)
* [`pthread_rwlockattr_init`](#pthread_005frwlockattr_005finit)
* [`pthread_rwlockattr_setpshared`](#pthread_005frwlockattr_005fsetpshared)
* [`pthread_self`](#pthread_005fself)
* [`pthread_setcancelstate`](#pthread_005fsetcancelstate)
* [`pthread_setcanceltype`](#pthread_005fsetcanceltype)
* [`pthread_setschedparam`](#pthread_005fsetschedparam)
* [`pthread_setschedprio`](#pthread_005fsetschedprio)
* [`pthread_setspecific`](#pthread_005fsetspecific)
* [`pthread_spin_destroy`](#pthread_005fspin_005fdestroy)
* [`pthread_spin_init`](#pthread_005fspin_005finit)
* [`pthread_spin_lock`](#pthread_005fspin_005flock)
* [`pthread_spin_trylock`](#pthread_005fspin_005ftrylock)
* [`pthread_spin_unlock`](#pthread_005fspin_005funlock)
* [`pthread_testcancel`](#pthread_005ftestcancel)

---

Next: [`pthread_attr_destroy`](#pthread_005fattr_005fdestroy), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.1 `pthread_atfork` [¶](#pthread_005fatfork-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_atfork.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, mingw, MSVC 14, Android 3.0.

---

Next: [`pthread_attr_getdetachstate`](#pthread_005fattr_005fgetdetachstate), Previous: [`pthread_atfork`](#pthread_005fatfork), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.2 `pthread_attr_destroy` [¶](#pthread_005fattr_005fdestroy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_destroy.html>

Gnulib module: pthread-thread

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_attr_getguardsize`](#pthread_005fattr_005fgetguardsize), Previous: [`pthread_attr_destroy`](#pthread_005fattr_005fdestroy), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.3 `pthread_attr_getdetachstate` [¶](#pthread_005fattr_005fgetdetachstate-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getdetachstate.html>

Gnulib module: pthread-thread

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_attr_getinheritsched`](#pthread_005fattr_005fgetinheritsched), Previous: [`pthread_attr_getdetachstate`](#pthread_005fattr_005fgetdetachstate), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.4 `pthread_attr_getguardsize` [¶](#pthread_005fattr_005fgetguardsize-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getguardsize.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, Cygwin 1.7.9, mingw, MSVC 14.

---

Next: [`pthread_attr_getschedparam`](#pthread_005fattr_005fgetschedparam), Previous: [`pthread_attr_getguardsize`](#pthread_005fattr_005fgetguardsize), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.5 `pthread_attr_getinheritsched` [¶](#pthread_005fattr_005fgetinheritsched-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getinheritsched.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 8.1.

---

Next: [`pthread_attr_getschedpolicy`](#pthread_005fattr_005fgetschedpolicy), Previous: [`pthread_attr_getinheritsched`](#pthread_005fattr_005fgetinheritsched), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.6 `pthread_attr_getschedparam` [¶](#pthread_005fattr_005fgetschedparam-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getschedparam.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_attr_getscope`](#pthread_005fattr_005fgetscope), Previous: [`pthread_attr_getschedparam`](#pthread_005fattr_005fgetschedparam), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.7 `pthread_attr_getschedpolicy` [¶](#pthread_005fattr_005fgetschedpolicy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getschedpolicy.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_attr_getstack`](#pthread_005fattr_005fgetstack), Previous: [`pthread_attr_getschedpolicy`](#pthread_005fattr_005fgetschedpolicy), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.8 `pthread_attr_getscope` [¶](#pthread_005fattr_005fgetscope-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getscope.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_attr_getstacksize`](#pthread_005fattr_005fgetstacksize), Previous: [`pthread_attr_getscope`](#pthread_005fattr_005fgetscope), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.9 `pthread_attr_getstack` [¶](#pthread_005fattr_005fgetstack-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getstack.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11.11, Solaris 9, Cygwin 1.7.9, mingw, MSVC 14.

---

Next: [`pthread_attr_init`](#pthread_005fattr_005finit), Previous: [`pthread_attr_getstack`](#pthread_005fattr_005fgetstack), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.10 `pthread_attr_getstacksize` [¶](#pthread_005fattr_005fgetstacksize-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getstacksize.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_attr_setdetachstate`](#pthread_005fattr_005fsetdetachstate), Previous: [`pthread_attr_getstacksize`](#pthread_005fattr_005fgetstacksize), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.11 `pthread_attr_init` [¶](#pthread_005fattr_005finit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_init.html>

Gnulib module: pthread-thread

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.
* This function is only defined as an inline function on some platforms:
  HP-UX 11.11.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_attr_setguardsize`](#pthread_005fattr_005fsetguardsize), Previous: [`pthread_attr_init`](#pthread_005fattr_005finit), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.12 `pthread_attr_setdetachstate` [¶](#pthread_005fattr_005fsetdetachstate-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_setdetachstate.html>

Gnulib module: pthread-thread

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_attr_setinheritsched`](#pthread_005fattr_005fsetinheritsched), Previous: [`pthread_attr_setdetachstate`](#pthread_005fattr_005fsetdetachstate), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.13 `pthread_attr_setguardsize` [¶](#pthread_005fattr_005fsetguardsize-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_setguardsize.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, Cygwin 1.7.9, mingw, MSVC 14.

---

Next: [`pthread_attr_setschedparam`](#pthread_005fattr_005fsetschedparam), Previous: [`pthread_attr_setguardsize`](#pthread_005fattr_005fsetguardsize), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.14 `pthread_attr_setinheritsched` [¶](#pthread_005fattr_005fsetinheritsched-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_setinheritsched.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 8.1.

---

Next: [`pthread_attr_setschedpolicy`](#pthread_005fattr_005fsetschedpolicy), Previous: [`pthread_attr_setinheritsched`](#pthread_005fattr_005fsetinheritsched), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.15 `pthread_attr_setschedparam` [¶](#pthread_005fattr_005fsetschedparam-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_setschedparam.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_attr_setscope`](#pthread_005fattr_005fsetscope), Previous: [`pthread_attr_setschedparam`](#pthread_005fattr_005fsetschedparam), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.16 `pthread_attr_setschedpolicy` [¶](#pthread_005fattr_005fsetschedpolicy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_setschedpolicy.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_attr_setstack`](#pthread_005fattr_005fsetstack), Previous: [`pthread_attr_setschedpolicy`](#pthread_005fattr_005fsetschedpolicy), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.17 `pthread_attr_setscope` [¶](#pthread_005fattr_005fsetscope-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_setscope.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_attr_setstacksize`](#pthread_005fattr_005fsetstacksize), Previous: [`pthread_attr_setscope`](#pthread_005fattr_005fsetscope), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.18 `pthread_attr_setstack` [¶](#pthread_005fattr_005fsetstack-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_setstack.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11.11, Solaris 9, Cygwin 1.7.9, mingw, MSVC 14.

---

Next: [`pthread_barrier_destroy`](#pthread_005fbarrier_005fdestroy), Previous: [`pthread_attr_setstack`](#pthread_005fattr_005fsetstack), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.19 `pthread_attr_setstacksize` [¶](#pthread_005fattr_005fsetstacksize-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_setstacksize.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_barrier_init`](#pthread_005fbarrier_005finit), Previous: [`pthread_attr_setstacksize`](#pthread_005fattr_005fsetstacksize), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.20 `pthread_barrier_destroy` [¶](#pthread_005fbarrier_005fdestroy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_barrier_destroy.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 6.0.

---

Next: [`pthread_barrier_wait`](#pthread_005fbarrier_005fwait), Previous: [`pthread_barrier_destroy`](#pthread_005fbarrier_005fdestroy), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.21 `pthread_barrier_init` [¶](#pthread_005fbarrier_005finit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_barrier_init.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 6.0.

---

Next: [`pthread_barrierattr_destroy`](#pthread_005fbarrierattr_005fdestroy), Previous: [`pthread_barrier_init`](#pthread_005fbarrier_005finit), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.22 `pthread_barrier_wait` [¶](#pthread_005fbarrier_005fwait-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_barrier_wait.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 6.0.

---

Next: [`pthread_barrierattr_getpshared`](#pthread_005fbarrierattr_005fgetpshared), Previous: [`pthread_barrier_wait`](#pthread_005fbarrier_005fwait), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.23 `pthread_barrierattr_destroy` [¶](#pthread_005fbarrierattr_005fdestroy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_barrierattr_destroy.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 6.0.

---

Next: [`pthread_barrierattr_init`](#pthread_005fbarrierattr_005finit), Previous: [`pthread_barrierattr_destroy`](#pthread_005fbarrierattr_005fdestroy), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.24 `pthread_barrierattr_getpshared` [¶](#pthread_005fbarrierattr_005fgetpshared-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_barrierattr_getpshared.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.2, macOS 14, FreeBSD 5.2.1, NetBSD 10.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 6.0.

---

Next: [`pthread_barrierattr_setpshared`](#pthread_005fbarrierattr_005fsetpshared), Previous: [`pthread_barrierattr_getpshared`](#pthread_005fbarrierattr_005fgetpshared), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.25 `pthread_barrierattr_init` [¶](#pthread_005fbarrierattr_005finit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_barrierattr_init.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 6.0.

---

Next: [`pthread_cancel`](#pthread_005fcancel), Previous: [`pthread_barrierattr_init`](#pthread_005fbarrierattr_005finit), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.26 `pthread_barrierattr_setpshared` [¶](#pthread_005fbarrierattr_005fsetpshared-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_barrierattr_setpshared.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, NetBSD 10.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 6.0.

---

Next: [`pthread_cleanup_pop`](#pthread_005fcleanup_005fpop), Previous: [`pthread_barrierattr_setpshared`](#pthread_005fbarrierattr_005fsetpshared), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.27 `pthread_cancel` [¶](#pthread_005fcancel-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cancel.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_cleanup_push`](#pthread_005fcleanup_005fpush), Previous: [`pthread_cancel`](#pthread_005fcancel), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.28 `pthread_cleanup_pop` [¶](#pthread_005fcleanup_005fpop-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cleanup_pop.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, NetBSD 10.0, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_cond_broadcast`](#pthread_005fcond_005fbroadcast), Previous: [`pthread_cleanup_pop`](#pthread_005fcleanup_005fpop), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.29 `pthread_cleanup_push` [¶](#pthread_005fcleanup_005fpush-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cleanup_push.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, NetBSD 10.0, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_cond_clockwait`](#pthread_005fcond_005fclockwait), Previous: [`pthread_cleanup_push`](#pthread_005fcleanup_005fpush), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.30 `pthread_cond_broadcast` [¶](#pthread_005fcond_005fbroadcast-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_broadcast.html>

Gnulib module: pthread-cond

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_cond_destroy`](#pthread_005fcond_005fdestroy), Previous: [`pthread_cond_broadcast`](#pthread_005fcond_005fbroadcast), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.31 `pthread_cond_clockwait` [¶](#pthread_005fcond_005fclockwait-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_clockwait.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Non_002dPOSIX-Extensions.html>,

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 3.2.x, mingw, MSVC 14, Android API level 29.

---

Next: [`pthread_cond_init`](#pthread_005fcond_005finit), Previous: [`pthread_cond_clockwait`](#pthread_005fcond_005fclockwait), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.32 `pthread_cond_destroy` [¶](#pthread_005fcond_005fdestroy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_destroy.html>

Gnulib module: pthread-cond

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_cond_signal`](#pthread_005fcond_005fsignal), Previous: [`pthread_cond_destroy`](#pthread_005fcond_005fdestroy), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.33 `pthread_cond_init` [¶](#pthread_005fcond_005finit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_init.html>

Gnulib module: pthread-cond

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_cond_timedwait`](#pthread_005fcond_005ftimedwait), Previous: [`pthread_cond_init`](#pthread_005fcond_005finit), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.34 `pthread_cond_signal` [¶](#pthread_005fcond_005fsignal-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_signal.html>

Gnulib module: pthread-cond

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_cond_wait`](#pthread_005fcond_005fwait), Previous: [`pthread_cond_signal`](#pthread_005fcond_005fsignal), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.35 `pthread_cond_timedwait` [¶](#pthread_005fcond_005ftimedwait-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_timedwait.html>

Gnulib module: pthread-cond

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

* This function sometimes waits indefinitely
  instead of up to the specified time point,
  on some platforms (glibc/Linux, musl libc) in some hypervisors:
  VirtualBox 6.1.50\_Ubuntu with paravirtualization set to "Default" or "KVM".

---

Next: [`pthread_condattr_destroy`](#pthread_005fcondattr_005fdestroy), Previous: [`pthread_cond_timedwait`](#pthread_005fcond_005ftimedwait), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.36 `pthread_cond_wait` [¶](#pthread_005fcond_005fwait-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_wait.html>

Gnulib module: pthread-cond

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_condattr_getclock`](#pthread_005fcondattr_005fgetclock), Previous: [`pthread_cond_wait`](#pthread_005fcond_005fwait), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.37 `pthread_condattr_destroy` [¶](#pthread_005fcondattr_005fdestroy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_condattr_destroy.html>

Gnulib module: pthread-cond

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_condattr_getpshared`](#pthread_005fcondattr_005fgetpshared), Previous: [`pthread_condattr_destroy`](#pthread_005fcondattr_005fdestroy), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.38 `pthread_condattr_getclock` [¶](#pthread_005fcondattr_005fgetclock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_condattr_getclock.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.2, macOS 14, FreeBSD 5.2.1, NetBSD 7.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.9, mingw, MSVC 14, Android 4.4.

---

Next: [`pthread_condattr_init`](#pthread_005fcondattr_005finit), Previous: [`pthread_condattr_getclock`](#pthread_005fcondattr_005fgetclock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.39 `pthread_condattr_getpshared` [¶](#pthread_005fcondattr_005fgetpshared-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_condattr_getpshared.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_condattr_setclock`](#pthread_005fcondattr_005fsetclock), Previous: [`pthread_condattr_getpshared`](#pthread_005fcondattr_005fgetpshared), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.40 `pthread_condattr_init` [¶](#pthread_005fcondattr_005finit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_condattr_init.html>

Gnulib module: pthread-cond

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_condattr_setpshared`](#pthread_005fcondattr_005fsetpshared), Previous: [`pthread_condattr_init`](#pthread_005fcondattr_005finit), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.41 `pthread_condattr_setclock` [¶](#pthread_005fcondattr_005fsetclock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_condattr_setclock.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.2, macOS 14, FreeBSD 5.2.1, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.9, mingw, MSVC 14, Android 4.4.

---

Next: [`pthread_create`](#pthread_005fcreate), Previous: [`pthread_condattr_setclock`](#pthread_005fcondattr_005fsetclock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.42 `pthread_condattr_setpshared` [¶](#pthread_005fcondattr_005fsetpshared-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_condattr_setpshared.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_detach`](#pthread_005fdetach), Previous: [`pthread_condattr_setpshared`](#pthread_005fcondattr_005fsetpshared), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.43 `pthread_create` [¶](#pthread_005fcreate-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html>

Gnulib module: pthread-thread

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.
* This function is only defined as an inline function on some platforms:
  HP-UX 11.11.

Portability problems not fixed by Gnulib:

* On Linux/glibc platforms before the advent of NPTL in 2003, signals could only
  be sent to one particular thread. In POSIX, signals are sent to the entire
  process and executed by any thread of the process that happens to have the
  particular signal currently unblocked.

---

Next: [`pthread_equal`](#pthread_005fequal), Previous: [`pthread_create`](#pthread_005fcreate), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.44 `pthread_detach` [¶](#pthread_005fdetach-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_detach.html>

Gnulib module: pthread-thread

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_exit`](#pthread_005fexit), Previous: [`pthread_detach`](#pthread_005fdetach), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.45 `pthread_equal` [¶](#pthread_005fequal-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_equal.html>

Gnulib module: pthread-thread

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_getcpuclockid`](#pthread_005fgetcpuclockid), Previous: [`pthread_equal`](#pthread_005fequal), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.46 `pthread_exit` [¶](#pthread_005fexit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_exit.html>

Gnulib module: pthread-thread

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_getschedparam`](#pthread_005fgetschedparam), Previous: [`pthread_exit`](#pthread_005fexit), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.47 `pthread_getcpuclockid` [¶](#pthread_005fgetcpuclockid-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_getcpuclockid.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 7.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.9, mingw, MSVC 14.

---

Next: [`pthread_getspecific`](#pthread_005fgetspecific), Previous: [`pthread_getcpuclockid`](#pthread_005fgetcpuclockid), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.48 `pthread_getschedparam` [¶](#pthread_005fgetschedparam-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_getschedparam.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_join`](#pthread_005fjoin), Previous: [`pthread_getschedparam`](#pthread_005fgetschedparam), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.49 `pthread_getspecific` [¶](#pthread_005fgetspecific-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_getspecific.html>

Gnulib module: pthread-tss

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_key_create`](#pthread_005fkey_005fcreate), Previous: [`pthread_getspecific`](#pthread_005fgetspecific), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.50 `pthread_join` [¶](#pthread_005fjoin-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_join.html>

Gnulib module: pthread-thread

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_key_delete`](#pthread_005fkey_005fdelete), Previous: [`pthread_join`](#pthread_005fjoin), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.51 `pthread_key_create` [¶](#pthread_005fkey_005fcreate-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_key_create.html>

Gnulib module: pthread-tss

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_mutex_clocklock`](#pthread_005fmutex_005fclocklock), Previous: [`pthread_key_create`](#pthread_005fkey_005fcreate), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.52 `pthread_key_delete` [¶](#pthread_005fkey_005fdelete-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_key_delete.html>

Gnulib module: pthread-tss

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_mutex_consistent`](#pthread_005fmutex_005fconsistent), Previous: [`pthread_key_delete`](#pthread_005fkey_005fdelete), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.53 `pthread_mutex_clocklock` [¶](#pthread_005fmutex_005fclocklock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_clocklock.html>

Documentation: —

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 3.2.x, mingw, MSVC 14, Android API level 29.

---

Next: [`pthread_mutex_destroy`](#pthread_005fmutex_005fdestroy), Previous: [`pthread_mutex_clocklock`](#pthread_005fmutex_005fclocklock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.54 `pthread_mutex_consistent` [¶](#pthread_005fmutex_005fconsistent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_consistent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.11, macOS 14, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_mutex_getprioceiling`](#pthread_005fmutex_005fgetprioceiling), Previous: [`pthread_mutex_consistent`](#pthread_005fmutex_005fconsistent), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.55 `pthread_mutex_destroy` [¶](#pthread_005fmutex_005fdestroy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_destroy.html>

Gnulib module: pthread-mutex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_mutex_init`](#pthread_005fmutex_005finit), Previous: [`pthread_mutex_destroy`](#pthread_005fmutex_005fdestroy), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.56 `pthread_mutex_getprioceiling` [¶](#pthread_005fmutex_005fgetprioceiling-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_getprioceiling.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, NetBSD 7.1, Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_mutex_lock`](#pthread_005fmutex_005flock), Previous: [`pthread_mutex_getprioceiling`](#pthread_005fmutex_005fgetprioceiling), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.57 `pthread_mutex_init` [¶](#pthread_005fmutex_005finit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_init.html>

Gnulib module: pthread-mutex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.3.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_mutex_setprioceiling`](#pthread_005fmutex_005fsetprioceiling), Previous: [`pthread_mutex_init`](#pthread_005fmutex_005finit), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.58 `pthread_mutex_lock` [¶](#pthread_005fmutex_005flock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_lock.html>

Gnulib module: pthread-mutex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

* This function does not behave as required by POSIX for mutexes of type
  NORMAL and ERRORCHECK, by allowing RECURSIVE locking,
  in programs linked without `-lpthread` on some platforms:
  FreeBSD 14.0, NetBSD 10.0.

---

Next: [`pthread_mutex_timedlock`](#pthread_005fmutex_005ftimedlock), Previous: [`pthread_mutex_lock`](#pthread_005fmutex_005flock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.59 `pthread_mutex_setprioceiling` [¶](#pthread_005fmutex_005fsetprioceiling-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_setprioceiling.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, NetBSD 7.1, Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_mutex_trylock`](#pthread_005fmutex_005ftrylock), Previous: [`pthread_mutex_setprioceiling`](#pthread_005fmutex_005fsetprioceiling), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.60 `pthread_mutex_timedlock` [¶](#pthread_005fmutex_005ftimedlock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_timedlock.html>

Gnulib module: pthread\_mutex\_timedlock

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, NetBSD 7.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_mutex_unlock`](#pthread_005fmutex_005funlock), Previous: [`pthread_mutex_timedlock`](#pthread_005fmutex_005ftimedlock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.61 `pthread_mutex_trylock` [¶](#pthread_005fmutex_005ftrylock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_trylock.html>

Gnulib module: pthread-mutex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

* This function does not behave as required by POSIX for mutexes of type
  NORMAL and ERRORCHECK, by allowing RECURSIVE locking,
  in programs linked without `-lpthread` on some platforms:
  FreeBSD 14.0, NetBSD 10.0.

---

Next: [`pthread_mutexattr_destroy`](#pthread_005fmutexattr_005fdestroy), Previous: [`pthread_mutex_trylock`](#pthread_005fmutex_005ftrylock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.62 `pthread_mutex_unlock` [¶](#pthread_005fmutex_005funlock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_unlock.html>

Gnulib module: pthread-mutex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_mutexattr_getprioceiling`](#pthread_005fmutexattr_005fgetprioceiling), Previous: [`pthread_mutex_unlock`](#pthread_005fmutex_005funlock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.63 `pthread_mutexattr_destroy` [¶](#pthread_005fmutexattr_005fdestroy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_destroy.html>

Gnulib module: pthread-mutex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_mutexattr_getprotocol`](#pthread_005fmutexattr_005fgetprotocol), Previous: [`pthread_mutexattr_destroy`](#pthread_005fmutexattr_005fdestroy), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.64 `pthread_mutexattr_getprioceiling` [¶](#pthread_005fmutexattr_005fgetprioceiling-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_getprioceiling.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, NetBSD 7.1, Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_mutexattr_getpshared`](#pthread_005fmutexattr_005fgetpshared), Previous: [`pthread_mutexattr_getprioceiling`](#pthread_005fmutexattr_005fgetprioceiling), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.65 `pthread_mutexattr_getprotocol` [¶](#pthread_005fmutexattr_005fgetprotocol-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_getprotocol.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, NetBSD 7.1, Minix 3.1.8, mingw, MSVC 14, Android 8.1.

---

Next: [`pthread_mutexattr_getrobust`](#pthread_005fmutexattr_005fgetrobust), Previous: [`pthread_mutexattr_getprotocol`](#pthread_005fmutexattr_005fgetprotocol), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.66 `pthread_mutexattr_getpshared` [¶](#pthread_005fmutexattr_005fgetpshared-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_getpshared.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_mutexattr_gettype`](#pthread_005fmutexattr_005fgettype), Previous: [`pthread_mutexattr_getpshared`](#pthread_005fmutexattr_005fgetpshared), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.67 `pthread_mutexattr_getrobust` [¶](#pthread_005fmutexattr_005fgetrobust-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_getrobust.html>

Gnulib module: pthread-mutex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.11, macOS 14, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
  But the provided replacement is just a dummy.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_mutexattr_init`](#pthread_005fmutexattr_005finit), Previous: [`pthread_mutexattr_getrobust`](#pthread_005fmutexattr_005fgetrobust), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.68 `pthread_mutexattr_gettype` [¶](#pthread_005fmutexattr_005fgettype-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_gettype.html>

Gnulib module: pthread-mutex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_mutexattr_setprioceiling`](#pthread_005fmutexattr_005fsetprioceiling), Previous: [`pthread_mutexattr_gettype`](#pthread_005fmutexattr_005fgettype), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.69 `pthread_mutexattr_init` [¶](#pthread_005fmutexattr_005finit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_init.html>

Gnulib module: pthread-mutex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_mutexattr_setprotocol`](#pthread_005fmutexattr_005fsetprotocol), Previous: [`pthread_mutexattr_init`](#pthread_005fmutexattr_005finit), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.70 `pthread_mutexattr_setprioceiling` [¶](#pthread_005fmutexattr_005fsetprioceiling-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_setprioceiling.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, NetBSD 7.1, Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_mutexattr_setpshared`](#pthread_005fmutexattr_005fsetpshared), Previous: [`pthread_mutexattr_setprioceiling`](#pthread_005fmutexattr_005fsetprioceiling), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.71 `pthread_mutexattr_setprotocol` [¶](#pthread_005fmutexattr_005fsetprotocol-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_setprotocol.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, NetBSD 7.1, Minix 3.1.8, mingw, MSVC 14, Android 8.1.

---

Next: [`pthread_mutexattr_setrobust`](#pthread_005fmutexattr_005fsetrobust), Previous: [`pthread_mutexattr_setprotocol`](#pthread_005fmutexattr_005fsetprotocol), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.72 `pthread_mutexattr_setpshared` [¶](#pthread_005fmutexattr_005fsetpshared-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_setpshared.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_mutexattr_settype`](#pthread_005fmutexattr_005fsettype), Previous: [`pthread_mutexattr_setpshared`](#pthread_005fmutexattr_005fsetpshared), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.73 `pthread_mutexattr_setrobust` [¶](#pthread_005fmutexattr_005fsetrobust-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_setrobust.html>

Gnulib module: pthread-mutex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.11, macOS 14, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
  But the provided replacement is just a dummy.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_once`](#pthread_005fonce), Previous: [`pthread_mutexattr_setrobust`](#pthread_005fmutexattr_005fsetrobust), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.74 `pthread_mutexattr_settype` [¶](#pthread_005fmutexattr_005fsettype-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_settype.html>

Gnulib module: pthread-mutex

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_rwlock_clockrdlock`](#pthread_005frwlock_005fclockrdlock), Previous: [`pthread_mutexattr_settype`](#pthread_005fmutexattr_005fsettype), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.75 `pthread_once` [¶](#pthread_005fonce-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_once.html>

Gnulib module: pthread-once

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.
* This function makes applications hang forever on some platforms:
  Cygwin 3.5.3.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_rwlock_clockwrlock`](#pthread_005frwlock_005fclockwrlock), Previous: [`pthread_once`](#pthread_005fonce), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.76 `pthread_rwlock_clockrdlock` [¶](#pthread_005frwlock_005fclockrdlock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlock_clockrdlock.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Non_002dPOSIX-Extensions.html>,

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 3.2.x, mingw, MSVC 14, Android API level 29.

---

Next: [`pthread_rwlock_destroy`](#pthread_005frwlock_005fdestroy), Previous: [`pthread_rwlock_clockrdlock`](#pthread_005frwlock_005fclockrdlock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.77 `pthread_rwlock_clockwrlock` [¶](#pthread_005frwlock_005fclockwrlock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlock_clockwrlock.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Non_002dPOSIX-Extensions.html>,

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 3.2.x, mingw, MSVC 14, Android API level 29.

---

Next: [`pthread_rwlock_init`](#pthread_005frwlock_005finit), Previous: [`pthread_rwlock_clockwrlock`](#pthread_005frwlock_005fclockwrlock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.78 `pthread_rwlock_destroy` [¶](#pthread_005frwlock_005fdestroy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlock_destroy.html>

Gnulib module: pthread-rwlock

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.3.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_rwlock_rdlock`](#pthread_005frwlock_005frdlock), Previous: [`pthread_rwlock_destroy`](#pthread_005frwlock_005fdestroy), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.79 `pthread_rwlock_init` [¶](#pthread_005frwlock_005finit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlock_init.html>

Gnulib module: pthread-rwlock

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.3.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_rwlock_timedrdlock`](#pthread_005frwlock_005ftimedrdlock), Previous: [`pthread_rwlock_init`](#pthread_005frwlock_005finit), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.80 `pthread_rwlock_rdlock` [¶](#pthread_005frwlock_005frdlock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlock_rdlock.html>

Gnulib module: pthread-rwlock

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.3.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.
* This function prefers readers to writers (meaning, when this function is
  called on an rwlock that is already taken by one or more readers, and
  another writer is already waiting to take it, this function may return
  successfully immediately) – a behaviour that may lead to writer starvation –
  on some platforms:
  glibc 2.42.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_rwlock_timedwrlock`](#pthread_005frwlock_005ftimedwrlock), Previous: [`pthread_rwlock_rdlock`](#pthread_005frwlock_005frdlock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.81 `pthread_rwlock_timedrdlock` [¶](#pthread_005frwlock_005ftimedrdlock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlock_timedrdlock.html>

Gnulib module: pthread-rwlock

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 4.3.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.
* This function prefers readers to writers (meaning, when this function is
  called on an rwlock that is already taken by one or more readers, and
  another writer is already waiting to take it, this function may return
  successfully immediately) – a behaviour that may lead to writer starvation –
  on some platforms:
  glibc 2.42.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_rwlock_tryrdlock`](#pthread_005frwlock_005ftryrdlock), Previous: [`pthread_rwlock_timedrdlock`](#pthread_005frwlock_005ftimedrdlock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.82 `pthread_rwlock_timedwrlock` [¶](#pthread_005frwlock_005ftimedwrlock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlock_timedwrlock.html>

Gnulib module: pthread-rwlock

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 5.2.1, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 4.3.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_rwlock_trywrlock`](#pthread_005frwlock_005ftrywrlock), Previous: [`pthread_rwlock_timedwrlock`](#pthread_005frwlock_005ftimedwrlock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.83 `pthread_rwlock_tryrdlock` [¶](#pthread_005frwlock_005ftryrdlock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlock_tryrdlock.html>

Gnulib module: pthread-rwlock

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.3.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.
* This function prefers readers to writers (meaning, when this function is
  called on an rwlock that is already taken by one or more readers, and
  another writer is already waiting to take it, this function may return
  successfully immediately) – a behaviour that may lead to writer starvation –
  on some platforms:
  glibc 2.42.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_rwlock_unlock`](#pthread_005frwlock_005funlock), Previous: [`pthread_rwlock_tryrdlock`](#pthread_005frwlock_005ftryrdlock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.84 `pthread_rwlock_trywrlock` [¶](#pthread_005frwlock_005ftrywrlock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlock_trywrlock.html>

Gnulib module: pthread-rwlock

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.3.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

* This function returns `EDEADLK` instead of `EBUSY`
  when the current thread already owns the lock for writing
  on some platforms:
  macOS 12.

---

Next: [`pthread_rwlock_wrlock`](#pthread_005frwlock_005fwrlock), Previous: [`pthread_rwlock_trywrlock`](#pthread_005frwlock_005ftrywrlock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.85 `pthread_rwlock_unlock` [¶](#pthread_005frwlock_005funlock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlock_unlock.html>

Gnulib module: pthread-rwlock

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.3.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_rwlockattr_destroy`](#pthread_005frwlockattr_005fdestroy), Previous: [`pthread_rwlock_unlock`](#pthread_005frwlock_005funlock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.86 `pthread_rwlock_wrlock` [¶](#pthread_005frwlock_005fwrlock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlock_wrlock.html>

Gnulib module: pthread-rwlock

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 4.3.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_rwlockattr_getpshared`](#pthread_005frwlockattr_005fgetpshared), Previous: [`pthread_rwlock_wrlock`](#pthread_005frwlock_005fwrlock), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.87 `pthread_rwlockattr_destroy` [¶](#pthread_005frwlockattr_005fdestroy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlockattr_destroy.html>

Gnulib module: pthread-rwlock

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_rwlockattr_init`](#pthread_005frwlockattr_005finit), Previous: [`pthread_rwlockattr_destroy`](#pthread_005frwlockattr_005fdestroy), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.88 `pthread_rwlockattr_getpshared` [¶](#pthread_005frwlockattr_005fgetpshared-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlockattr_getpshared.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 10.0, Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_rwlockattr_setpshared`](#pthread_005frwlockattr_005fsetpshared), Previous: [`pthread_rwlockattr_getpshared`](#pthread_005frwlockattr_005fgetpshared), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.89 `pthread_rwlockattr_init` [¶](#pthread_005frwlockattr_005finit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlockattr_init.html>

Gnulib module: pthread-rwlock

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_self`](#pthread_005fself), Previous: [`pthread_rwlockattr_init`](#pthread_005frwlockattr_005finit), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.90 `pthread_rwlockattr_setpshared` [¶](#pthread_005frwlockattr_005fsetpshared-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_rwlockattr_setpshared.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 10.0, Minix 3.1.8, mingw, MSVC 14.

---

Next: [`pthread_setcancelstate`](#pthread_005fsetcancelstate), Previous: [`pthread_rwlockattr_setpshared`](#pthread_005frwlockattr_005fsetpshared), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.91 `pthread_self` [¶](#pthread_005fself-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_self.html>

Gnulib module: pthread-thread

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
  But the provided replacement is just a dummy on some of these platforms:
  Minix 3.1.8.

Portability problems not fixed by Gnulib:

---

Next: [`pthread_setcanceltype`](#pthread_005fsetcanceltype), Previous: [`pthread_self`](#pthread_005fself), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.92 `pthread_setcancelstate` [¶](#pthread_005fsetcancelstate-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_setcancelstate.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_setschedparam`](#pthread_005fsetschedparam), Previous: [`pthread_setcancelstate`](#pthread_005fsetcancelstate), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.93 `pthread_setcanceltype` [¶](#pthread_005fsetcanceltype-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_setcanceltype.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_setschedprio`](#pthread_005fsetschedprio), Previous: [`pthread_setcanceltype`](#pthread_005fsetcanceltype), Up: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.33.94 `pthread_setschedparam` [¶](#pthread_005fsetschedparam-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_setschedparam.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---
