### 10.41 Functions in `<spawn.h>` [¶](#Functions-in-_003cspawn_002eh_003e-1)

* [`posix_spawn`](#posix_005fspawn)
* [`posix_spawn_file_actions_addchdir`](#posix_005fspawn_005ffile_005factions_005faddchdir)
* [`posix_spawn_file_actions_addclose`](#posix_005fspawn_005ffile_005factions_005faddclose)
* [`posix_spawn_file_actions_adddup2`](#posix_005fspawn_005ffile_005factions_005fadddup2)
* [`posix_spawn_file_actions_addfchdir`](#posix_005fspawn_005ffile_005factions_005faddfchdir)
* [`posix_spawn_file_actions_addopen`](#posix_005fspawn_005ffile_005factions_005faddopen)
* [`posix_spawn_file_actions_destroy`](#posix_005fspawn_005ffile_005factions_005fdestroy)
* [`posix_spawn_file_actions_init`](#posix_005fspawn_005ffile_005factions_005finit)
* [`posix_spawnattr_destroy`](#posix_005fspawnattr_005fdestroy)
* [`posix_spawnattr_getflags`](#posix_005fspawnattr_005fgetflags)
* [`posix_spawnattr_getpgroup`](#posix_005fspawnattr_005fgetpgroup)
* [`posix_spawnattr_getschedparam`](#posix_005fspawnattr_005fgetschedparam)
* [`posix_spawnattr_getschedpolicy`](#posix_005fspawnattr_005fgetschedpolicy)
* [`posix_spawnattr_getsigdefault`](#posix_005fspawnattr_005fgetsigdefault)
* [`posix_spawnattr_getsigmask`](#posix_005fspawnattr_005fgetsigmask)
* [`posix_spawnattr_init`](#posix_005fspawnattr_005finit)
* [`posix_spawnattr_setflags`](#posix_005fspawnattr_005fsetflags)
* [`posix_spawnattr_setpgroup`](#posix_005fspawnattr_005fsetpgroup)
* [`posix_spawnattr_setschedparam`](#posix_005fspawnattr_005fsetschedparam)
* [`posix_spawnattr_setschedpolicy`](#posix_005fspawnattr_005fsetschedpolicy)
* [`posix_spawnattr_setsigdefault`](#posix_005fspawnattr_005fsetsigdefault)
* [`posix_spawnattr_setsigmask`](#posix_005fspawnattr_005fsetsigmask)
* [`posix_spawnp`](#posix_005fspawnp)

---

Next: [`posix_spawn_file_actions_addchdir`](#posix_005fspawn_005ffile_005factions_005faddchdir), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.1 `posix_spawn` [¶](#posix_005fspawn-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawn.html>

Gnulib module: posix\_spawn

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.3, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.
* When this function fails, it causes the stdio buffer contents to be output
  twice on some platforms:
  AIX 6.1.
* When the program to be invoked is an executable script without a ‘`#!`’
  marker in the first line, this function executes the script as if it were
  a shell script, on some platforms:
  GNU/Hurd.

Portability problems not fixed by Gnulib:

* This function does not work on some platforms:
  AIX 6.1 (under particular circumstances).

The Gnulib modules `posix_spawn_file_actions_addchdir` and
`posix_spawn_file_actions_addfchdir` provide additional actions,
that consist in changing the current directory of the child process
before starting the specified program.

---

Next: [`posix_spawn_file_actions_addclose`](#posix_005fspawn_005ffile_005factions_005faddclose), Previous: [`posix_spawn`](#posix_005fspawn), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.2 `posix_spawn_file_actions_addchdir` [¶](#posix_005fspawn_005ffile_005factions_005faddchdir-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawn_file_actions_addchdir.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 9.3, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

---

Next: [`posix_spawn_file_actions_adddup2`](#posix_005fspawn_005ffile_005factions_005fadddup2), Previous: [`posix_spawn_file_actions_addchdir`](#posix_005fspawn_005ffile_005factions_005faddchdir), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.3 `posix_spawn_file_actions_addclose` [¶](#posix_005fspawn_005ffile_005factions_005faddclose-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawn_file_actions_addclose.html>

Gnulib module: posix\_spawn\_file\_actions\_addclose

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.
* This function does not reject a negative file descriptor on some platforms:
  musl libc.

Portability problems not fixed by Gnulib:

* This function does not reject an out-of-range file descriptor on some platforms:
  musl libc, NetBSD, Cygwin.

---

Next: [`posix_spawn_file_actions_addfchdir`](#posix_005fspawn_005ffile_005factions_005faddfchdir), Previous: [`posix_spawn_file_actions_addclose`](#posix_005fspawn_005ffile_005factions_005faddclose), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.4 `posix_spawn_file_actions_adddup2` [¶](#posix_005fspawn_005ffile_005factions_005fadddup2-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawn_file_actions_adddup2.html>

Gnulib module: posix\_spawn\_file\_actions\_adddup2

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.
* This function does not reject a too large file descriptor on some platforms:
  musl libc, Solaris 11.4.

Portability problems not fixed by Gnulib:

---

Next: [`posix_spawn_file_actions_addopen`](#posix_005fspawn_005ffile_005factions_005faddopen), Previous: [`posix_spawn_file_actions_adddup2`](#posix_005fspawn_005ffile_005factions_005fadddup2), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.5 `posix_spawn_file_actions_addfchdir` [¶](#posix_005fspawn_005ffile_005factions_005faddfchdir-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawn_file_actions_addfchdir.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.42, macOS 14, FreeBSD 14.0, NetBSD 9.3, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

---

Next: [`posix_spawn_file_actions_destroy`](#posix_005fspawn_005ffile_005factions_005fdestroy), Previous: [`posix_spawn_file_actions_addfchdir`](#posix_005fspawn_005ffile_005factions_005faddfchdir), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.6 `posix_spawn_file_actions_addopen` [¶](#posix_005fspawn_005ffile_005factions_005faddopen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawn_file_actions_addopen.html>

Gnulib module: posix\_spawn\_file\_actions\_addopen

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.
* This function does not reject a too large file descriptor on some platforms:
  musl libc, Solaris 11.4.

Portability problems not fixed by Gnulib:

---

Next: [`posix_spawn_file_actions_init`](#posix_005fspawn_005ffile_005factions_005finit), Previous: [`posix_spawn_file_actions_addopen`](#posix_005fspawn_005ffile_005factions_005faddopen), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.7 `posix_spawn_file_actions_destroy` [¶](#posix_005fspawn_005ffile_005factions_005fdestroy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawn_file_actions_destroy.html>

Gnulib module: posix\_spawn\_file\_actions\_destroy

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

---

Next: [`posix_spawnattr_destroy`](#posix_005fspawnattr_005fdestroy), Previous: [`posix_spawn_file_actions_destroy`](#posix_005fspawn_005ffile_005factions_005fdestroy), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.8 `posix_spawn_file_actions_init` [¶](#posix_005fspawn_005ffile_005factions_005finit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawn_file_actions_init.html>

Gnulib module: posix\_spawn\_file\_actions\_init

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

---

Next: [`posix_spawnattr_getflags`](#posix_005fspawnattr_005fgetflags), Previous: [`posix_spawn_file_actions_init`](#posix_005fspawn_005ffile_005factions_005finit), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.9 `posix_spawnattr_destroy` [¶](#posix_005fspawnattr_005fdestroy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnattr_destroy.html>

Gnulib module: posix\_spawnattr\_destroy

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

---

Next: [`posix_spawnattr_getpgroup`](#posix_005fspawnattr_005fgetpgroup), Previous: [`posix_spawnattr_destroy`](#posix_005fspawnattr_005fdestroy), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.10 `posix_spawnattr_getflags` [¶](#posix_005fspawnattr_005fgetflags-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnattr_getflags.html>

Gnulib module: posix\_spawnattr\_getflags

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

---

Next: [`posix_spawnattr_getschedparam`](#posix_005fspawnattr_005fgetschedparam), Previous: [`posix_spawnattr_getflags`](#posix_005fspawnattr_005fgetflags), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.11 `posix_spawnattr_getpgroup` [¶](#posix_005fspawnattr_005fgetpgroup-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnattr_getpgroup.html>

Gnulib module: posix\_spawnattr\_getpgroup

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

---

Next: [`posix_spawnattr_getschedpolicy`](#posix_005fspawnattr_005fgetschedpolicy), Previous: [`posix_spawnattr_getpgroup`](#posix_005fspawnattr_005fgetpgroup), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.12 `posix_spawnattr_getschedparam` [¶](#posix_005fspawnattr_005fgetschedparam-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnattr_getschedparam.html>

Gnulib module: posix\_spawnattr\_getschedparam

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

* This function always fails with error `ENOSYS` on some platforms:
  musl libc.

---

Next: [`posix_spawnattr_getsigdefault`](#posix_005fspawnattr_005fgetsigdefault), Previous: [`posix_spawnattr_getschedparam`](#posix_005fspawnattr_005fgetschedparam), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.13 `posix_spawnattr_getschedpolicy` [¶](#posix_005fspawnattr_005fgetschedpolicy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnattr_getschedpolicy.html>

Gnulib module: posix\_spawnattr\_getschedpolicy

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

* This function always fails with error `ENOSYS` on some platforms:
  musl libc.

---

Next: [`posix_spawnattr_getsigmask`](#posix_005fspawnattr_005fgetsigmask), Previous: [`posix_spawnattr_getschedpolicy`](#posix_005fspawnattr_005fgetschedpolicy), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.14 `posix_spawnattr_getsigdefault` [¶](#posix_005fspawnattr_005fgetsigdefault-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnattr_getsigdefault.html>

Gnulib module: posix\_spawnattr\_getsigdefault

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

---

Next: [`posix_spawnattr_init`](#posix_005fspawnattr_005finit), Previous: [`posix_spawnattr_getsigdefault`](#posix_005fspawnattr_005fgetsigdefault), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.15 `posix_spawnattr_getsigmask` [¶](#posix_005fspawnattr_005fgetsigmask-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnattr_getsigmask.html>

Gnulib module: posix\_spawnattr\_getsigmask

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

---

Next: [`posix_spawnattr_setflags`](#posix_005fspawnattr_005fsetflags), Previous: [`posix_spawnattr_getsigmask`](#posix_005fspawnattr_005fgetsigmask), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.16 `posix_spawnattr_init` [¶](#posix_005fspawnattr_005finit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnattr_init.html>

Gnulib module: posix\_spawnattr\_init

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

---

Next: [`posix_spawnattr_setpgroup`](#posix_005fspawnattr_005fsetpgroup), Previous: [`posix_spawnattr_init`](#posix_005fspawnattr_005finit), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.17 `posix_spawnattr_setflags` [¶](#posix_005fspawnattr_005fsetflags-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnattr_setflags.html>

Gnulib module: posix\_spawnattr\_setflags

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

---

Next: [`posix_spawnattr_setschedparam`](#posix_005fspawnattr_005fsetschedparam), Previous: [`posix_spawnattr_setflags`](#posix_005fspawnattr_005fsetflags), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.18 `posix_spawnattr_setpgroup` [¶](#posix_005fspawnattr_005fsetpgroup-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnattr_setpgroup.html>

Gnulib module: posix\_spawnattr\_setpgroup

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

---

Next: [`posix_spawnattr_setschedpolicy`](#posix_005fspawnattr_005fsetschedpolicy), Previous: [`posix_spawnattr_setpgroup`](#posix_005fspawnattr_005fsetpgroup), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.19 `posix_spawnattr_setschedparam` [¶](#posix_005fspawnattr_005fsetschedparam-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnattr_setschedparam.html>

Gnulib module: posix\_spawnattr\_setschedparam

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

* This function always fails with error `ENOSYS` on some platforms:
  musl libc.

---

Next: [`posix_spawnattr_setsigdefault`](#posix_005fspawnattr_005fsetsigdefault), Previous: [`posix_spawnattr_setschedparam`](#posix_005fspawnattr_005fsetschedparam), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.20 `posix_spawnattr_setschedpolicy` [¶](#posix_005fspawnattr_005fsetschedpolicy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnattr_setschedpolicy.html>

Gnulib module: posix\_spawnattr\_setschedpolicy

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

* This function always fails with error `ENOSYS` on some platforms:
  musl libc.

---

Next: [`posix_spawnattr_setsigmask`](#posix_005fspawnattr_005fsetsigmask), Previous: [`posix_spawnattr_setschedpolicy`](#posix_005fspawnattr_005fsetschedpolicy), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.21 `posix_spawnattr_setsigdefault` [¶](#posix_005fspawnattr_005fsetsigdefault-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnattr_setsigdefault.html>

Gnulib module: posix\_spawnattr\_setsigdefault

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

---

Next: [`posix_spawnp`](#posix_005fspawnp), Previous: [`posix_spawnattr_setsigdefault`](#posix_005fspawnattr_005fsetsigdefault), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.22 `posix_spawnattr_setsigmask` [¶](#posix_005fspawnattr_005fsetsigmask-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnattr_setsigmask.html>

Gnulib module: posix\_spawnattr\_setsigmask

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.

Portability problems not fixed by Gnulib:

---

Previous: [`posix_spawnattr_setsigmask`](#posix_005fspawnattr_005fsetsigmask), Up: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.41.23 `posix_spawnp` [¶](#posix_005fspawnp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawnp.html>

Gnulib module: posix\_spawnp

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.3, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.
* When this function fails, it causes the stdio buffer contents to be output
  twice on some platforms:
  AIX 6.1.
* When the program to be invoked is an executable script without a ‘`#!`’
  marker in the first line, this function executes the script as if it were
  a shell script, on some platforms:
  glibc 2.14/Linux, glibc 2.32/Hurd, macOS 14, FreeBSD 14.0, OpenBSD 7.2, AIX 7.2, Solaris 11.4, Cygwin 3.4.6.

Portability problems not fixed by Gnulib:

* This function does not work on some platforms:
  AIX 6.1 (under particular circumstances).

The Gnulib modules `posix_spawn_file_actions_addchdir` and
`posix_spawn_file_actions_addfchdir` provide additional actions,
that consist in changing the current directory of the child process
before starting the specified program.

---

Next: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e), Previous: [Functions in `<spawn.h>`](#Functions-in-_003cspawn_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.42 Functions in `<stdarg.h>` [¶](#Functions-in-_003cstdarg_002eh_003e-1)

* [`va_arg`](#va_005farg)
* [`va_copy`](#va_005fcopy)
* [`va_end`](#va_005fend)
* [`va_start`](#va_005fstart)

---

Next: [`va_copy`](#va_005fcopy), Up: [Functions in `<stdarg.h>`](#Functions-in-_003cstdarg_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.42.1 `va_arg` [¶](#va_005farg-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/va_arg.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* The second argument of `va_arg` must be a type that is invariant under
  the “default argument promotions” (ISO C 99 6.5.2.2 paragraph 6). This
  means that the following are not valid here:

  ‘`float`’
  :   Use ‘`double`’ instead.

  ‘`bool`’
  :   Use ‘`int`’ instead.

  Integer types smaller than ‘`int`’.
  :   Use ‘`int`’ or ‘`unsigned int`’ instead.

  This is a portability problem because you don’t know the width of some
  abstract types like `uid_t`, `gid_t`, `mode_t`. So, instead of

  ```
  mode = va_arg (ap, mode_t);
  ```

  you have to write

  ```
  mode = (sizeof (mode_t) < sizeof (int)
          ? va_arg (ap, int)
          : va_arg (ap, mode_t));
  ```

---

Next: [`va_end`](#va_005fend), Previous: [`va_arg`](#va_005farg), Up: [Functions in `<stdarg.h>`](#Functions-in-_003cstdarg_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.42.2 `va_copy` [¶](#va_005fcopy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/va_copy.html>

Gnulib module: stdarg-h

Portability problems fixed by Gnulib:

* This macro is missing on some platforms:
  AIX 5.1 with cc or xlc, HP-UX 11 with cc.

Portability problems not fixed by Gnulib:

---

Next: [`va_start`](#va_005fstart), Previous: [`va_copy`](#va_005fcopy), Up: [Functions in `<stdarg.h>`](#Functions-in-_003cstdarg_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.42.3 `va_end` [¶](#va_005fend-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/va_end.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Previous: [`va_end`](#va_005fend), Up: [Functions in `<stdarg.h>`](#Functions-in-_003cstdarg_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.42.4 `va_start` [¶](#va_005fstart-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/va_start.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e), Previous: [Functions in `<stdarg.h>`](#Functions-in-_003cstdarg_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.43 Functions in `<stdatomic.h>` [¶](#Functions-in-_003cstdatomic_002eh_003e-1)

* [`atomic_compare_exchange_strong`](#atomic_005fcompare_005fexchange_005fstrong)
* [`atomic_compare_exchange_strong_explicit`](#atomic_005fcompare_005fexchange_005fstrong_005fexplicit)
* [`atomic_compare_exchange_weak`](#atomic_005fcompare_005fexchange_005fweak)
* [`atomic_compare_exchange_weak_explicit`](#atomic_005fcompare_005fexchange_005fweak_005fexplicit)
* [`atomic_exchange`](#atomic_005fexchange)
* [`atomic_exchange_explicit`](#atomic_005fexchange_005fexplicit)
* [`atomic_fetch_add`](#atomic_005ffetch_005fadd)
* [`atomic_fetch_add_explicit`](#atomic_005ffetch_005fadd_005fexplicit)
* [`atomic_fetch_and`](#atomic_005ffetch_005fand)
* [`atomic_fetch_and_explicit`](#atomic_005ffetch_005fand_005fexplicit)
* [`atomic_fetch_or`](#atomic_005ffetch_005for)
* [`atomic_fetch_or_explicit`](#atomic_005ffetch_005for_005fexplicit)
* [`atomic_fetch_sub`](#atomic_005ffetch_005fsub)
* [`atomic_fetch_sub_explicit`](#atomic_005ffetch_005fsub_005fexplicit)
* [`atomic_fetch_xor`](#atomic_005ffetch_005fxor)
* [`atomic_fetch_xor_explicit`](#atomic_005ffetch_005fxor_005fexplicit)
* [`atomic_flag_clear`](#atomic_005fflag_005fclear)
* [`atomic_flag_clear_explicit`](#atomic_005fflag_005fclear_005fexplicit)
* [`atomic_flag_test_and_set`](#atomic_005fflag_005ftest_005fand_005fset)
* [`atomic_flag_test_and_set_explicit`](#atomic_005fflag_005ftest_005fand_005fset_005fexplicit)
* [`atomic_init`](#atomic_005finit)
* [`atomic_is_lock_free`](#atomic_005fis_005flock_005ffree)
* [`atomic_load`](#atomic_005fload)
* [`atomic_load_explicit`](#atomic_005fload_005fexplicit)
* [`atomic_signal_fence`](#atomic_005fsignal_005ffence)
* [`atomic_store`](#atomic_005fstore)
* [`atomic_store_explicit`](#atomic_005fstore_005fexplicit)
* [`atomic_thread_fence`](#atomic_005fthread_005ffence)
* [`kill_dependency`](#kill_005fdependency)

---

Next: [`atomic_compare_exchange_strong_explicit`](#atomic_005fcompare_005fexchange_005fstrong_005fexplicit), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.1 `atomic_compare_exchange_strong` [¶](#atomic_005fcompare_005fexchange_005fstrong-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.4.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_compare_exchange_strong.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_compare_exchange_weak`](#atomic_005fcompare_005fexchange_005fweak), Previous: [`atomic_compare_exchange_strong`](#atomic_005fcompare_005fexchange_005fstrong), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.2 `atomic_compare_exchange_strong_explicit` [¶](#atomic_005fcompare_005fexchange_005fstrong_005fexplicit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.4.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_compare_exchange_strong_explicit.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_compare_exchange_weak_explicit`](#atomic_005fcompare_005fexchange_005fweak_005fexplicit), Previous: [`atomic_compare_exchange_strong_explicit`](#atomic_005fcompare_005fexchange_005fstrong_005fexplicit), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.3 `atomic_compare_exchange_weak` [¶](#atomic_005fcompare_005fexchange_005fweak-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.4.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_compare_exchange_weak.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_exchange`](#atomic_005fexchange), Previous: [`atomic_compare_exchange_weak`](#atomic_005fcompare_005fexchange_005fweak), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.4 `atomic_compare_exchange_weak_explicit` [¶](#atomic_005fcompare_005fexchange_005fweak_005fexplicit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.4.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_compare_exchange_weak_explicit.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_exchange_explicit`](#atomic_005fexchange_005fexplicit), Previous: [`atomic_compare_exchange_weak_explicit`](#atomic_005fcompare_005fexchange_005fweak_005fexplicit), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.5 `atomic_exchange` [¶](#atomic_005fexchange-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.3.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_exchange.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_fetch_add`](#atomic_005ffetch_005fadd), Previous: [`atomic_exchange`](#atomic_005fexchange), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.6 `atomic_exchange_explicit` [¶](#atomic_005fexchange_005fexplicit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.3.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_exchange_explicit.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_fetch_add_explicit`](#atomic_005ffetch_005fadd_005fexplicit), Previous: [`atomic_exchange_explicit`](#atomic_005fexchange_005fexplicit), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.7 `atomic_fetch_add` [¶](#atomic_005ffetch_005fadd-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.5.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_fetch_add.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_fetch_and`](#atomic_005ffetch_005fand), Previous: [`atomic_fetch_add`](#atomic_005ffetch_005fadd), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.8 `atomic_fetch_add_explicit` [¶](#atomic_005ffetch_005fadd_005fexplicit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.5.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_fetch_add_explicit.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_fetch_and_explicit`](#atomic_005ffetch_005fand_005fexplicit), Previous: [`atomic_fetch_add_explicit`](#atomic_005ffetch_005fadd_005fexplicit), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.9 `atomic_fetch_and` [¶](#atomic_005ffetch_005fand-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.5.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_fetch_and.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_fetch_or`](#atomic_005ffetch_005for), Previous: [`atomic_fetch_and`](#atomic_005ffetch_005fand), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.10 `atomic_fetch_and_explicit` [¶](#atomic_005ffetch_005fand_005fexplicit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.5.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_fetch_and_explicit.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_fetch_or_explicit`](#atomic_005ffetch_005for_005fexplicit), Previous: [`atomic_fetch_and_explicit`](#atomic_005ffetch_005fand_005fexplicit), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.11 `atomic_fetch_or` [¶](#atomic_005ffetch_005for-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.5.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_fetch_or.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_fetch_sub`](#atomic_005ffetch_005fsub), Previous: [`atomic_fetch_or`](#atomic_005ffetch_005for), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.12 `atomic_fetch_or_explicit` [¶](#atomic_005ffetch_005for_005fexplicit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.5.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_fetch_or_explicit.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_fetch_sub_explicit`](#atomic_005ffetch_005fsub_005fexplicit), Previous: [`atomic_fetch_or_explicit`](#atomic_005ffetch_005for_005fexplicit), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.13 `atomic_fetch_sub` [¶](#atomic_005ffetch_005fsub-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.5.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_fetch_sub.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_fetch_xor`](#atomic_005ffetch_005fxor), Previous: [`atomic_fetch_sub`](#atomic_005ffetch_005fsub), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.14 `atomic_fetch_sub_explicit` [¶](#atomic_005ffetch_005fsub_005fexplicit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.5.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_fetch_sub_explicit.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_fetch_xor_explicit`](#atomic_005ffetch_005fxor_005fexplicit), Previous: [`atomic_fetch_sub_explicit`](#atomic_005ffetch_005fsub_005fexplicit), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.15 `atomic_fetch_xor` [¶](#atomic_005ffetch_005fxor-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.5.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_fetch_xor.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_flag_clear`](#atomic_005fflag_005fclear), Previous: [`atomic_fetch_xor`](#atomic_005ffetch_005fxor), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.16 `atomic_fetch_xor_explicit` [¶](#atomic_005ffetch_005fxor_005fexplicit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.5.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_fetch_xor_explicit.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_flag_clear_explicit`](#atomic_005fflag_005fclear_005fexplicit), Previous: [`atomic_fetch_xor_explicit`](#atomic_005ffetch_005fxor_005fexplicit), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.17 `atomic_flag_clear` [¶](#atomic_005fflag_005fclear-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.8.2.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_flag_clear.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_flag_test_and_set`](#atomic_005fflag_005ftest_005fand_005fset), Previous: [`atomic_flag_clear`](#atomic_005fflag_005fclear), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.18 `atomic_flag_clear_explicit` [¶](#atomic_005fflag_005fclear_005fexplicit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.8.2.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_flag_clear_explicit.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_flag_test_and_set_explicit`](#atomic_005fflag_005ftest_005fand_005fset_005fexplicit), Previous: [`atomic_flag_clear_explicit`](#atomic_005fflag_005fclear_005fexplicit), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.19 `atomic_flag_test_and_set` [¶](#atomic_005fflag_005ftest_005fand_005fset-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.8.1.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_flag_test_and_set.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_init`](#atomic_005finit), Previous: [`atomic_flag_test_and_set`](#atomic_005fflag_005ftest_005fand_005fset), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.20 `atomic_flag_test_and_set_explicit` [¶](#atomic_005fflag_005ftest_005fand_005fset_005fexplicit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.8.1.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_flag_test_and_set_explicit.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_is_lock_free`](#atomic_005fis_005flock_005ffree), Previous: [`atomic_flag_test_and_set_explicit`](#atomic_005fflag_005ftest_005fand_005fset_005fexplicit), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.21 `atomic_init` [¶](#atomic_005finit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.2.1.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_init.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_load`](#atomic_005fload), Previous: [`atomic_init`](#atomic_005finit), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.22 `atomic_is_lock_free` [¶](#atomic_005fis_005flock_005ffree-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.5.1.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_is_lock_free.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_load_explicit`](#atomic_005fload_005fexplicit), Previous: [`atomic_is_lock_free`](#atomic_005fis_005flock_005ffree), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.23 `atomic_load` [¶](#atomic_005fload-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.2.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_load.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_signal_fence`](#atomic_005fsignal_005ffence), Previous: [`atomic_load`](#atomic_005fload), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.24 `atomic_load_explicit` [¶](#atomic_005fload_005fexplicit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.2.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_load_explicit.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_store`](#atomic_005fstore), Previous: [`atomic_load_explicit`](#atomic_005fload_005fexplicit), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.25 `atomic_signal_fence` [¶](#atomic_005fsignal_005ffence-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.4.2.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_signal_fence.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_store_explicit`](#atomic_005fstore_005fexplicit), Previous: [`atomic_signal_fence`](#atomic_005fsignal_005ffence), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.26 `atomic_store` [¶](#atomic_005fstore-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.1.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_store.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`atomic_thread_fence`](#atomic_005fthread_005ffence), Previous: [`atomic_store`](#atomic_005fstore), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.27 `atomic_store_explicit` [¶](#atomic_005fstore_005fexplicit-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.7.1.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_store_explicit.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [`kill_dependency`](#kill_005fdependency), Previous: [`atomic_store_explicit`](#atomic_005fstore_005fexplicit), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.28 `atomic_thread_fence` [¶](#atomic_005fthread_005ffence-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.4.1.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/atomic_thread_fence.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Previous: [`atomic_thread_fence`](#atomic_005fthread_005ffence), Up: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.43.29 `kill_dependency` [¶](#kill_005fdependency-1)

ISO C23 specification:  
 <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf> section 7.17.3.1.

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/kill_dependency.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc with GCC 4.8.x or clang 2.8, FreeBSD 6.4, NetBSD 7.0, OpenBSD 6.5, OpenBSD 6.9 with gcc, Minix 3.3.0, AIX 7.2 with xlc or xlclang, HP-UX 11.31, Solaris 11.3 with Oracle Studio 12.4 (Sun C 5.13), Cygwin 1.5.19, mingw, MSVC 14..

---

Next: [Functions in `<stdio.h>`](#Functions-in-_003cstdio_002eh_003e), Previous: [Functions in `<stdatomic.h>`](#Functions-in-_003cstdatomic_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
