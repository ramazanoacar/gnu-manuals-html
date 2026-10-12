Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.

---

Previous: [`y1`](#y1), Up: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.24.335 `yn` [¶](#yn-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/yn.html>

Gnulib module: yn

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8.

---

Next: [Functions in `<mqueue.h>`](#Functions-in-_003cmqueue_002eh_003e), Previous: [Functions in `<math.h>`](#Functions-in-_003cmath_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.25 Functions in `<monetary.h>` [¶](#Functions-in-_003cmonetary_002eh_003e-1)

* [`strfmon`](#strfmon)
* [`strfmon_l`](#strfmon_005fl)

---

Next: [`strfmon_l`](#strfmon_005fl), Up: [Functions in `<monetary.h>`](#Functions-in-_003cmonetary_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.25.1 `strfmon` [¶](#strfmon-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strfmon.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 1.7.1, mingw, MSVC 14, Android 9.0.

---

Previous: [`strfmon`](#strfmon), Up: [Functions in `<monetary.h>`](#Functions-in-_003cmonetary_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.25.2 `strfmon_l` [¶](#strfmon_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strfmon_l.html>

Gnulib module: strfmon\_l

Portability problems fixed by Gnulib:

* This function ignores the locale argument on some platforms:
  FreeBSD 13.0, Cygwin 2.9.
* This function uses a wrong locale for the numbers on some platforms:
  glibc 2.23.

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 7.1, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.

---

Next: [Functions in `<ndbm.h>`](#Functions-in-_003cndbm_002eh_003e), Previous: [Functions in `<monetary.h>`](#Functions-in-_003cmonetary_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.26 Functions in `<mqueue.h>` [¶](#Functions-in-_003cmqueue_002eh_003e-1)

* [`mq_close`](#mq_005fclose)
* [`mq_getattr`](#mq_005fgetattr)
* [`mq_notify`](#mq_005fnotify)
* [`mq_open`](#mq_005fopen)
* [`mq_receive`](#mq_005freceive)
* [`mq_send`](#mq_005fsend)
* [`mq_setattr`](#mq_005fsetattr)
* [`mq_timedreceive`](#mq_005ftimedreceive)
* [`mq_timedsend`](#mq_005ftimedsend)
* [`mq_unlink`](#mq_005funlink)

---

Next: [`mq_getattr`](#mq_005fgetattr), Up: [Functions in `<mqueue.h>`](#Functions-in-_003cmqueue_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.26.1 `mq_close` [¶](#mq_005fclose-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mq_close.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`mq_notify`](#mq_005fnotify), Previous: [`mq_close`](#mq_005fclose), Up: [Functions in `<mqueue.h>`](#Functions-in-_003cmqueue_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.26.2 `mq_getattr` [¶](#mq_005fgetattr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mq_getattr.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`mq_open`](#mq_005fopen), Previous: [`mq_getattr`](#mq_005fgetattr), Up: [Functions in `<mqueue.h>`](#Functions-in-_003cmqueue_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.26.3 `mq_notify` [¶](#mq_005fnotify-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mq_notify.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`mq_receive`](#mq_005freceive), Previous: [`mq_notify`](#mq_005fnotify), Up: [Functions in `<mqueue.h>`](#Functions-in-_003cmqueue_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.26.4 `mq_open` [¶](#mq_005fopen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mq_open.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`mq_send`](#mq_005fsend), Previous: [`mq_open`](#mq_005fopen), Up: [Functions in `<mqueue.h>`](#Functions-in-_003cmqueue_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.26.5 `mq_receive` [¶](#mq_005freceive-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mq_receive.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`mq_setattr`](#mq_005fsetattr), Previous: [`mq_receive`](#mq_005freceive), Up: [Functions in `<mqueue.h>`](#Functions-in-_003cmqueue_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.26.6 `mq_send` [¶](#mq_005fsend-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mq_send.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`mq_timedreceive`](#mq_005ftimedreceive), Previous: [`mq_send`](#mq_005fsend), Up: [Functions in `<mqueue.h>`](#Functions-in-_003cmqueue_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.26.7 `mq_setattr` [¶](#mq_005fsetattr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mq_setattr.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`mq_timedsend`](#mq_005ftimedsend), Previous: [`mq_setattr`](#mq_005fsetattr), Up: [Functions in `<mqueue.h>`](#Functions-in-_003cmqueue_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.26.8 `mq_timedreceive` [¶](#mq_005ftimedreceive-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mq_timedreceive.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [`mq_unlink`](#mq_005funlink), Previous: [`mq_timedreceive`](#mq_005ftimedreceive), Up: [Functions in `<mqueue.h>`](#Functions-in-_003cmqueue_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.26.9 `mq_timedsend` [¶](#mq_005ftimedsend-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mq_timedsend.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 9, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Previous: [`mq_timedsend`](#mq_005ftimedsend), Up: [Functions in `<mqueue.h>`](#Functions-in-_003cmqueue_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.26.10 `mq_unlink` [¶](#mq_005funlink-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mq_unlink.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 6.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [Functions in `<net/if.h>`](#Functions-in-_003cnet_002fif_002eh_003e), Previous: [Functions in `<mqueue.h>`](#Functions-in-_003cmqueue_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.27 Functions in `<ndbm.h>` [¶](#Functions-in-_003cndbm_002eh_003e-1)

* [`dbm_clearerr`](#dbm_005fclearerr)
* [`dbm_close`](#dbm_005fclose)
* [`dbm_delete`](#dbm_005fdelete)
* [`dbm_error`](#dbm_005ferror)
* [`dbm_fetch`](#dbm_005ffetch)
* [`dbm_firstkey`](#dbm_005ffirstkey)
* [`dbm_nextkey`](#dbm_005fnextkey)
* [`dbm_open`](#dbm_005fopen)
* [`dbm_store`](#dbm_005fstore)

---

Next: [`dbm_close`](#dbm_005fclose), Up: [Functions in `<ndbm.h>`](#Functions-in-_003cndbm_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.27.1 `dbm_clearerr` [¶](#dbm_005fclearerr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dbm_clearerr.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, HP-UX 11.11, Cygwin 3.0.x, mingw, MSVC 14, Android 9.0.

---

Next: [`dbm_delete`](#dbm_005fdelete), Previous: [`dbm_clearerr`](#dbm_005fclearerr), Up: [Functions in `<ndbm.h>`](#Functions-in-_003cndbm_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.27.2 `dbm_close` [¶](#dbm_005fclose-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dbm_close.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Cygwin 3.0.x, mingw, MSVC 14, Android 9.0.

---

Next: [`dbm_error`](#dbm_005ferror), Previous: [`dbm_close`](#dbm_005fclose), Up: [Functions in `<ndbm.h>`](#Functions-in-_003cndbm_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.27.3 `dbm_delete` [¶](#dbm_005fdelete-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dbm_delete.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Cygwin 3.0.x, mingw, MSVC 14, Android 9.0.

---

Next: [`dbm_fetch`](#dbm_005ffetch), Previous: [`dbm_delete`](#dbm_005fdelete), Up: [Functions in `<ndbm.h>`](#Functions-in-_003cndbm_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.27.4 `dbm_error` [¶](#dbm_005ferror-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dbm_error.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, HP-UX 11.11, Cygwin 3.0.x, mingw, MSVC 14, Android 9.0.

---

Next: [`dbm_firstkey`](#dbm_005ffirstkey), Previous: [`dbm_error`](#dbm_005ferror), Up: [Functions in `<ndbm.h>`](#Functions-in-_003cndbm_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.27.5 `dbm_fetch` [¶](#dbm_005ffetch-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dbm_fetch.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Cygwin 3.0.x, mingw, MSVC 14, Android 9.0.

---

Next: [`dbm_nextkey`](#dbm_005fnextkey), Previous: [`dbm_fetch`](#dbm_005ffetch), Up: [Functions in `<ndbm.h>`](#Functions-in-_003cndbm_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.27.6 `dbm_firstkey` [¶](#dbm_005ffirstkey-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dbm_firstkey.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Cygwin 3.0.x, mingw, MSVC 14, Android 9.0.

---

Next: [`dbm_open`](#dbm_005fopen), Previous: [`dbm_firstkey`](#dbm_005ffirstkey), Up: [Functions in `<ndbm.h>`](#Functions-in-_003cndbm_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.27.7 `dbm_nextkey` [¶](#dbm_005fnextkey-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dbm_nextkey.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Cygwin 3.0.x, mingw, MSVC 14, Android 9.0.

---

Next: [`dbm_store`](#dbm_005fstore), Previous: [`dbm_nextkey`](#dbm_005fnextkey), Up: [Functions in `<ndbm.h>`](#Functions-in-_003cndbm_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.27.8 `dbm_open` [¶](#dbm_005fopen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dbm_open.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Cygwin 3.0.x, mingw, MSVC 14, Android 9.0.

---

Previous: [`dbm_open`](#dbm_005fopen), Up: [Functions in `<ndbm.h>`](#Functions-in-_003cndbm_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.27.9 `dbm_store` [¶](#dbm_005fstore-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/dbm_store.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, Cygwin 3.0.x, mingw, MSVC 14, Android 9.0.

---

Next: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e), Previous: [Functions in `<ndbm.h>`](#Functions-in-_003cndbm_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.28 Functions in `<net/if.h>` [¶](#Functions-in-_003cnet_002fif_002eh_003e-1)

* [`if_freenameindex`](#if_005ffreenameindex)
* [`if_indextoname`](#if_005findextoname)
* [`if_nameindex`](#if_005fnameindex)
* [`if_nametoindex`](#if_005fnametoindex)

---

Next: [`if_indextoname`](#if_005findextoname), Up: [Functions in `<net/if.h>`](#Functions-in-_003cnet_002fif_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.28.1 `if_freenameindex` [¶](#if_005ffreenameindex-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/if_freenameindex.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, HP-UX 11.23, Cygwin 1.5.x, mingw, MSVC 14, Android 6.0.
* This is available only as a macro, rather than a function, on some
  platforms:
  OpenBSD 4.6, MirBSD 10.

---

Next: [`if_nameindex`](#if_005fnameindex), Previous: [`if_freenameindex`](#if_005ffreenameindex), Up: [Functions in `<net/if.h>`](#Functions-in-_003cnet_002fif_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.28.2 `if_indextoname` [¶](#if_005findextoname-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/if_indextoname.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.23, Cygwin 1.5.x, mingw, MSVC 14.

---

Next: [`if_nametoindex`](#if_005fnametoindex), Previous: [`if_indextoname`](#if_005findextoname), Up: [Functions in `<net/if.h>`](#Functions-in-_003cnet_002fif_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.28.3 `if_nameindex` [¶](#if_005fnameindex-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/if_nameindex.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.23, Cygwin 1.5.x, mingw, MSVC 14, Android 6.0.

---

Previous: [`if_nameindex`](#if_005fnameindex), Up: [Functions in `<net/if.h>`](#Functions-in-_003cnet_002fif_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.28.4 `if_nametoindex` [¶](#if_005fnametoindex-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/if_nametoindex.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.23, Cygwin 1.5.x, mingw, MSVC 14.

---

Next: [Functions in `<netinet/in.h>`](#Functions-in-_003cnetinet_002fin_002eh_003e), Previous: [Functions in `<net/if.h>`](#Functions-in-_003cnet_002fif_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.29 Functions in `<netdb.h>` [¶](#Functions-in-_003cnetdb_002eh_003e-1)

* [`endhostent`](#endhostent)
* [`endnetent`](#endnetent)
* [`endprotoent`](#endprotoent)
* [`endservent`](#endservent)
* [`freeaddrinfo`](#freeaddrinfo)
* [`gai_strerror`](#gai_005fstrerror)
* [`getaddrinfo`](#getaddrinfo)
* [`gethostent`](#gethostent)
* [`getnameinfo`](#getnameinfo)
* [`getnetbyaddr`](#getnetbyaddr)
* [`getnetbyname`](#getnetbyname)
* [`getnetent`](#getnetent)
* [`getprotobyname`](#getprotobyname)
* [`getprotobynumber`](#getprotobynumber)
* [`getprotoent`](#getprotoent)
* [`getservbyname`](#getservbyname)
* [`getservbyport`](#getservbyport)
* [`getservent`](#getservent)
* [`sethostent`](#sethostent)
* [`setnetent`](#setnetent)
* [`setprotoent`](#setprotoent)
* [`setservent`](#setservent)

---

Next: [`endnetent`](#endnetent), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.1 `endhostent` [¶](#endhostent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/endhostent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 8.1.

---

Next: [`endprotoent`](#endprotoent), Previous: [`endhostent`](#endhostent), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.2 `endnetent` [¶](#endnetent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/endnetent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Cygwin 2.9, mingw, MSVC 14, Android 8.1.

---

Next: [`endservent`](#endservent), Previous: [`endnetent`](#endnetent), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.3 `endprotoent` [¶](#endprotoent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/endprotoent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 8.1.

---

Next: [`freeaddrinfo`](#freeaddrinfo), Previous: [`endprotoent`](#endprotoent), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.4 `endservent` [¶](#endservent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/endservent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`gai_strerror`](#gai_005fstrerror), Previous: [`endservent`](#endservent), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.5 `freeaddrinfo` [¶](#freeaddrinfo-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/freeaddrinfo.html>

Gnulib module: getaddrinfo

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.11, Cygwin 1.5.x, mingw, MSVC 14.
* On Windows, this function is declared in `<ws2tcpip.h>` rather than in
  `<netdb.h>`.
* On Windows, in 32-bit mode, this function is defined with a calling convention
  that is different from `cdecl`.

Portability problems not fixed by Gnulib:

---

Next: [`getaddrinfo`](#getaddrinfo), Previous: [`freeaddrinfo`](#freeaddrinfo), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.6 `gai_strerror` [¶](#gai_005fstrerror-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/gai_strerror.html>

Gnulib module: getaddrinfo

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.11, Cygwin 1.5.x.
* This function is only available in `<ws2tcpip.h>` on some
  platforms:
  mingw, MSVC 14.
* This function’s return type is `char *` instead of `const char *`
  on some platforms:
  AIX 7.1, HP-UX 11, Solaris 9, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`gethostent`](#gethostent), Previous: [`gai_strerror`](#gai_005fstrerror), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.7 `getaddrinfo` [¶](#getaddrinfo-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getaddrinfo.html>

Gnulib module: getaddrinfo

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.11, Cygwin 1.5.x, mingw, MSVC 14.
* On native Windows, this function is declared in `<ws2tcpip.h>`
  rather than in `<netdb.h>`.
* On native Windows, this function does not support
  the `AI_NUMERICSERV` hints flag.
* On native Windows, in 32-bit mode, this function is defined
  with a calling convention that is different from `cdecl`.
* On Solaris 10, this function does not support
  the `AI_NUMERICHOST` hints flag.

Portability problems not fixed by Gnulib:

* Unlike glibc’s implementation, gnulib’s replacement does not support
  internationalized domain names (IDN) encoding.

---

Next: [`getnameinfo`](#getnameinfo), Previous: [`getaddrinfo`](#getaddrinfo), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.8 `gethostent` [¶](#gethostent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/gethostent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`getnetbyaddr`](#getnetbyaddr), Previous: [`gethostent`](#gethostent), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.9 `getnameinfo` [¶](#getnameinfo-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getnameinfo.html>

Gnulib module: getaddrinfo

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  HP-UX 11.11, Cygwin 1.5.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

---

Next: [`getnetbyname`](#getnetbyname), Previous: [`getnameinfo`](#getnameinfo), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.10 `getnetbyaddr` [¶](#getnetbyaddr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getnetbyaddr.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Cygwin 2.9, mingw, MSVC 14.

---

Next: [`getnetent`](#getnetent), Previous: [`getnetbyaddr`](#getnetbyaddr), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.11 `getnetbyname` [¶](#getnetbyname-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getnetbyname.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Cygwin 2.9, mingw, MSVC 14.

---

Next: [`getprotobyname`](#getprotobyname), Previous: [`getnetbyname`](#getnetbyname), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.12 `getnetent` [¶](#getnetent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getnetent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Cygwin 2.9, mingw, MSVC 14, Android 8.1.

---

Next: [`getprotobynumber`](#getprotobynumber), Previous: [`getnetent`](#getnetent), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.13 `getprotobyname` [¶](#getprotobyname-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getprotobyname.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is declared in `<winsock2.h>` instead of `<netdb.h>`
  on some platforms:
  mingw, MSVC 14.

---

Next: [`getprotoent`](#getprotoent), Previous: [`getprotobyname`](#getprotobyname), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.14 `getprotobynumber` [¶](#getprotobynumber-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getprotobynumber.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is declared in `<winsock2.h>` instead of `<netdb.h>`
  on some platforms:
  mingw, MSVC 14.

---

Next: [`getservbyname`](#getservbyname), Previous: [`getprotobynumber`](#getprotobynumber), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.15 `getprotoent` [¶](#getprotoent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getprotoent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 8.1.

---

Next: [`getservbyport`](#getservbyport), Previous: [`getprotoent`](#getprotoent), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.16 `getservbyname` [¶](#getservbyname-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getservbyname.html>

Gnulib module: servent

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>` instead of `<netdb.h>`
  on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows, in 32-bit mode, this function is defined with a calling
  convention that is different from `cdecl`.

---

Next: [`getservent`](#getservent), Previous: [`getservbyname`](#getservbyname), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.17 `getservbyport` [¶](#getservbyport-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getservbyport.html>

Gnulib module: servent

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>` instead of `<netdb.h>`
  on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* On Windows, in 32-bit mode, this function is defined with a calling
  convention that is different from `cdecl`.

---

Next: [`sethostent`](#sethostent), Previous: [`getservbyport`](#getservbyport), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.18 `getservent` [¶](#getservent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getservent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`setnetent`](#setnetent), Previous: [`getservent`](#getservent), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.19 `sethostent` [¶](#sethostent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/sethostent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 8.1.

---

Next: [`setprotoent`](#setprotoent), Previous: [`sethostent`](#sethostent), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.20 `setnetent` [¶](#setnetent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setnetent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Cygwin 2.9, mingw, MSVC 14, Android 8.1.

---

Next: [`setservent`](#setservent), Previous: [`setnetent`](#setnetent), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.21 `setprotoent` [¶](#setprotoent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setprotoent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 8.1.

---

Previous: [`setprotoent`](#setprotoent), Up: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.29.22 `setservent` [¶](#setservent-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setservent.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [Functions in `<nl_types.h>`](#Functions-in-_003cnl_005ftypes_002eh_003e), Previous: [Functions in `<netdb.h>`](#Functions-in-_003cnetdb_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.30 Functions in `<netinet/in.h>` [¶](#Functions-in-_003cnetinet_002fin_002eh_003e-1)

* [`in6addr_any`](#in6addr_005fany)
* [`in6addr_loopback`](#in6addr_005floopback)

---

Next: [`in6addr_loopback`](#in6addr_005floopback), Up: [Functions in `<netinet/in.h>`](#Functions-in-_003cnetinet_002fin_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.30.1 `in6addr_any` [¶](#in6addr_005fany-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/in6addr_any.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Host-Address-Data-Type.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This constant is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, Cygwin 1.5.x, mingw, MSVC 14, Android 6.0.

---

Previous: [`in6addr_any`](#in6addr_005fany), Up: [Functions in `<netinet/in.h>`](#Functions-in-_003cnetinet_002fin_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.30.2 `in6addr_loopback` [¶](#in6addr_005floopback-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/in6addr_loopback.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Host-Address-Data-Type.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This constant is missing on some platforms:
  Minix 3.1.8, HP-UX 11.00, Cygwin 1.5.x, mingw, MSVC 14, Android 6.0.

---

Next: [Functions in `<poll.h>`](#Functions-in-_003cpoll_002eh_003e), Previous: [Functions in `<netinet/in.h>`](#Functions-in-_003cnetinet_002fin_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.31 Functions in `<nl_types.h>` [¶](#Functions-in-_003cnl_005ftypes_002eh_003e-1)

* [`catclose`](#catclose)
* [`catgets`](#catgets)
* [`catopen`](#catopen)

---

Next: [`catgets`](#catgets), Up: [Functions in `<nl_types.h>`](#Functions-in-_003cnl_005ftypes_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.31.1 `catclose` [¶](#catclose-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/catclose.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 2.9.x, mingw, MSVC 14, Android 7.1.

---

Next: [`catopen`](#catopen), Previous: [`catclose`](#catclose), Up: [Functions in `<nl_types.h>`](#Functions-in-_003cnl_005ftypes_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.31.2 `catgets` [¶](#catgets-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/catgets.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 2.9.x, mingw, MSVC 14, Android 7.1.

---

Previous: [`catgets`](#catgets), Up: [Functions in `<nl_types.h>`](#Functions-in-_003cnl_005ftypes_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.31.3 `catopen` [¶](#catopen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/catopen.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 2.9.x, mingw, MSVC 14, Android 7.1.

---

Next: [Functions in `<pthread.h>`](#Functions-in-_003cpthread_002eh_003e), Previous: [Functions in `<nl_types.h>`](#Functions-in-_003cnl_005ftypes_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.32 Functions in `<poll.h>` [¶](#Functions-in-_003cpoll_002eh_003e-1)

* [`poll`](#poll)
* [`ppoll`](#ppoll)

---

Next: [`ppoll`](#ppoll), Up: [Functions in `<poll.h>`](#Functions-in-_003cpoll_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.32.1 `poll` [¶](#poll-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/poll.html>

Gnulib module: poll

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, HP NonStop.
* This function doesn’t work on special files like `/dev/null` and ttys like
  `/dev/tty` on some platforms:
  macOS 14, AIX 5.3.

Portability problems not fixed by Gnulib:

* Under Windows, when passing a pipe, Gnulib’s `poll` replacement might
  return 0 even before the timeout has passed. Programs using it with pipes can
  thus busy wait.
* On some platforms, file descriptors other than sockets do not support
  POLLHUP; they will return a "readable" or "writable" status instead:
  AIX 7.2, Cygwin 3.4.6, HP NonStop, mingw, MSVC.

---

Previous: [`poll`](#poll), Up: [Functions in `<poll.h>`](#Functions-in-_003cpoll_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.32.2 `ppoll` [¶](#ppoll-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ppoll.html>

Documentation:  
 [man ppoll](https://www.kernel.org/doc/man-pages/online/pages/man2/ppoll.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, FreeBSD 6.4, NetBSD 9.3, OpenBSD 3.8, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 10, Cygwin 1.7.x, mingw, MSVC 14, Android 4.3.

---

Next: [Functions in `<pwd.h>`](#Functions-in-_003cpwd_002eh_003e), Previous: [Functions in `<poll.h>`](#Functions-in-_003cpoll_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
