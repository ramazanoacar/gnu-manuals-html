### 14.35 Glibc Extensions to `<netinet/in.h>` [¶](#Glibc-Extensions-to-_003cnetinet_002fin_002eh_003e)

* [`bindresvport`](#bindresvport)
* [`getipv4sourcefilter`](#getipv4sourcefilter)
* [`getsourcefilter`](#getsourcefilter)
* [`inet6_option_alloc`](#inet6_005foption_005falloc)
* [`inet6_option_append`](#inet6_005foption_005fappend)
* [`inet6_option_find`](#inet6_005foption_005ffind)
* [`inet6_option_init`](#inet6_005foption_005finit)
* [`inet6_option_next`](#inet6_005foption_005fnext)
* [`inet6_option_space`](#inet6_005foption_005fspace)
* [`inet6_opt_append`](#inet6_005fopt_005fappend)
* [`inet6_opt_find`](#inet6_005fopt_005ffind)
* [`inet6_opt_finish`](#inet6_005fopt_005ffinish)
* [`inet6_opt_get_val`](#inet6_005fopt_005fget_005fval)
* [`inet6_opt_init`](#inet6_005fopt_005finit)
* [`inet6_opt_next`](#inet6_005fopt_005fnext)
* [`inet6_opt_set_val`](#inet6_005fopt_005fset_005fval)
* [`inet6_rth_add`](#inet6_005frth_005fadd)
* [`inet6_rth_getaddr`](#inet6_005frth_005fgetaddr)
* [`inet6_rth_init`](#inet6_005frth_005finit)
* [`inet6_rth_reverse`](#inet6_005frth_005freverse)
* [`inet6_rth_segments`](#inet6_005frth_005fsegments)
* [`inet6_rth_space`](#inet6_005frth_005fspace)
* [`setipv4sourcefilter`](#setipv4sourcefilter)
* [`setsourcefilter`](#setsourcefilter)

---

Next: [`getipv4sourcefilter`](#getipv4sourcefilter), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.1 `bindresvport` [¶](#bindresvport-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-bindresvport-3.html>

Documentation:  
 [man bindresvport](https://www.kernel.org/doc/man-pages/online/pages/man3/bindresvport.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11, Cygwin 1.5.x, mingw, MSVC 14.

---

Next: [`getsourcefilter`](#getsourcefilter), Previous: [`bindresvport`](#bindresvport), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.2 `getipv4sourcefilter` [¶](#getipv4sourcefilter-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11.23, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_option_alloc`](#inet6_005foption_005falloc), Previous: [`getipv4sourcefilter`](#getipv4sourcefilter), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.3 `getsourcefilter` [¶](#getsourcefilter-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11.23, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_option_append`](#inet6_005foption_005fappend), Previous: [`getsourcefilter`](#getsourcefilter), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.4 `inet6_option_alloc` [¶](#inet6_005foption_005falloc-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_option_find`](#inet6_005foption_005ffind), Previous: [`inet6_option_alloc`](#inet6_005foption_005falloc), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.5 `inet6_option_append` [¶](#inet6_005foption_005fappend-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_option_init`](#inet6_005foption_005finit), Previous: [`inet6_option_append`](#inet6_005foption_005fappend), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.6 `inet6_option_find` [¶](#inet6_005foption_005ffind-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_option_next`](#inet6_005foption_005fnext), Previous: [`inet6_option_find`](#inet6_005foption_005ffind), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.7 `inet6_option_init` [¶](#inet6_005foption_005finit-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_option_space`](#inet6_005foption_005fspace), Previous: [`inet6_option_init`](#inet6_005foption_005finit), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.8 `inet6_option_next` [¶](#inet6_005foption_005fnext-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_opt_append`](#inet6_005fopt_005fappend), Previous: [`inet6_option_next`](#inet6_005foption_005fnext), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.9 `inet6_option_space` [¶](#inet6_005foption_005fspace-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_opt_find`](#inet6_005fopt_005ffind), Previous: [`inet6_option_space`](#inet6_005foption_005fspace), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.10 `inet6_opt_append` [¶](#inet6_005fopt_005fappend-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.4, Mac OS X 10.5, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 9, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_opt_finish`](#inet6_005fopt_005ffinish), Previous: [`inet6_opt_append`](#inet6_005fopt_005fappend), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.11 `inet6_opt_find` [¶](#inet6_005fopt_005ffind-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.4, Mac OS X 10.5, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 9, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_opt_get_val`](#inet6_005fopt_005fget_005fval), Previous: [`inet6_opt_find`](#inet6_005fopt_005ffind), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.12 `inet6_opt_finish` [¶](#inet6_005fopt_005ffinish-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.4, Mac OS X 10.5, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 9, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_opt_init`](#inet6_005fopt_005finit), Previous: [`inet6_opt_finish`](#inet6_005fopt_005ffinish), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.13 `inet6_opt_get_val` [¶](#inet6_005fopt_005fget_005fval-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.4, Mac OS X 10.5, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 9, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_opt_next`](#inet6_005fopt_005fnext), Previous: [`inet6_opt_get_val`](#inet6_005fopt_005fget_005fval), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.14 `inet6_opt_init` [¶](#inet6_005fopt_005finit-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.4, Mac OS X 10.5, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 9, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_opt_set_val`](#inet6_005fopt_005fset_005fval), Previous: [`inet6_opt_init`](#inet6_005fopt_005finit), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.15 `inet6_opt_next` [¶](#inet6_005fopt_005fnext-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.4, Mac OS X 10.5, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 9, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_rth_add`](#inet6_005frth_005fadd), Previous: [`inet6_opt_next`](#inet6_005fopt_005fnext), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.16 `inet6_opt_set_val` [¶](#inet6_005fopt_005fset_005fval-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.4, Mac OS X 10.5, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 9, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_rth_getaddr`](#inet6_005frth_005fgetaddr), Previous: [`inet6_opt_set_val`](#inet6_005fopt_005fset_005fval), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.17 `inet6_rth_add` [¶](#inet6_005frth_005fadd-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.4, Mac OS X 10.5, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 9, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_rth_init`](#inet6_005frth_005finit), Previous: [`inet6_rth_add`](#inet6_005frth_005fadd), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.18 `inet6_rth_getaddr` [¶](#inet6_005frth_005fgetaddr-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.4, Mac OS X 10.5, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 9, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_rth_reverse`](#inet6_005frth_005freverse), Previous: [`inet6_rth_getaddr`](#inet6_005frth_005fgetaddr), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.19 `inet6_rth_init` [¶](#inet6_005frth_005finit-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.4, Mac OS X 10.5, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 9, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_rth_segments`](#inet6_005frth_005fsegments), Previous: [`inet6_rth_init`](#inet6_005frth_005finit), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.20 `inet6_rth_reverse` [¶](#inet6_005frth_005freverse-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.4, Mac OS X 10.5, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 9, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`inet6_rth_space`](#inet6_005frth_005fspace), Previous: [`inet6_rth_reverse`](#inet6_005frth_005freverse), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.21 `inet6_rth_segments` [¶](#inet6_005frth_005fsegments-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.4, Mac OS X 10.5, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 9, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`setipv4sourcefilter`](#setipv4sourcefilter), Previous: [`inet6_rth_segments`](#inet6_005frth_005fsegments), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.22 `inet6_rth_space` [¶](#inet6_005frth_005fspace-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.4, Mac OS X 10.5, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 9, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`setsourcefilter`](#setsourcefilter), Previous: [`inet6_rth_space`](#inet6_005frth_005fspace), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.23 `setipv4sourcefilter` [¶](#setipv4sourcefilter-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11.23, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`setipv4sourcefilter`](#setipv4sourcefilter), Up: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.35.24 `setsourcefilter` [¶](#setsourcefilter-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11.23, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<printf.h>`](#Glibc-printf_002eh), Previous: [Glibc Extensions to `<netinet/in.h>`](#Glibc-netinet_002fin_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.36 Glibc `<obstack.h>` [¶](#Glibc-_003cobstack_002eh_003e)

* [`obstack_alloc_failed_handler`](#obstack_005falloc_005ffailed_005fhandler)
* [`obstack_exit_failure`](#obstack_005fexit_005ffailure)
* [`obstack_free`](#obstack_005ffree)
* [`obstack_printf`](#obstack_005fprintf)
* [`obstack_vprintf`](#obstack_005fvprintf)

---

Next: [`obstack_exit_failure`](#obstack_005fexit_005ffailure), Up: [Glibc `<obstack.h>`](#Glibc-obstack_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.36.1 `obstack_alloc_failed_handler` [¶](#obstack_005falloc_005ffailed_005fhandler-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Preparing-for-Obstacks.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`obstack_free`](#obstack_005ffree), Previous: [`obstack_alloc_failed_handler`](#obstack_005falloc_005ffailed_005fhandler), Up: [Glibc `<obstack.h>`](#Glibc-obstack_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.36.2 `obstack_exit_failure` [¶](#obstack_005fexit_005ffailure-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`obstack_printf`](#obstack_005fprintf), Previous: [`obstack_exit_failure`](#obstack_005fexit_005ffailure), Up: [Glibc `<obstack.h>`](#Glibc-obstack_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.36.3 `obstack_free` [¶](#obstack_005ffree-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Freeing-Obstack-Objects.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`obstack_vprintf`](#obstack_005fvprintf), Previous: [`obstack_free`](#obstack_005ffree), Up: [Glibc `<obstack.h>`](#Glibc-obstack_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.36.4 `obstack_printf` [¶](#obstack_005fprintf-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Dynamic-Output.html>.

Gnulib module: obstack-printf or obstack-printf-posix or obstack-printf-gnu

Portability problems fixed by either Gnulib module
`obstack-printf` or `obstack-printf-posix` or `obstack-printf-gnu`:

* This function is missing on all non-glibc platforms:
  musl libc, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

Portability problems fixed by either Gnulib module `obstack-printf-posix` or `obstack-printf-gnu`:

* This function does not support size specifiers as in C99 (`hh`, `ll`,
  `j`, `t`, `z`) on some platforms:
  AIX 5.1, HP-UX 11.23, Solaris 9, Cygwin 1.5.24, old mingw, MSVC 9.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37.
* printf of ‘`long double`’ numbers is unsupported on some platforms:
  mingw, MSVC 14.
* printf `"%f"`, `"%e"`, `"%g"` of Infinity and NaN yields an
  incorrect result on some platforms:
  AIX 7.3, Solaris 11.4, mingw, MSVC 14.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14.
* This function doesn’t support the `'` flag correctly for integers
  (it uses the decimal point instead of the thousands separator!)
  on some platforms:
  mingw with `__USE_MINGW_ANSI_STDIO`.
* This function doesn’t support the `'` flag together with a precision
  on an integer argument correctly on some platforms:
  glibc 2.36, FreeBSD 14.0, NetBSD 10.0, Solaris 11, Cygwin 3.6.0, Haiku.
* This function doesn’t support the `'` flag correctly in locales where
  the thousands separator is a multibyte character on some platforms:
  NetBSD 10.0, Solaris 11 OpenIndiana.
* This function does not support the ‘`a`’ and ‘`A`’ directives on some
  platforms:
  FreeBSD 5.2.1, OpenBSD 4.0, AIX 5.2, HP-UX 11,
  Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`F`’ directive on some platforms:
  NetBSD 3.0, AIX 5.1, HP-UX 11.23, Solaris 9,
  Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`ls`’ directive on some platforms:
  OpenBSD 4.0, Cygwin 1.5.x, Haiku.
* This function does not support precisions in the ‘`ls`’ directive correctly
  on some platforms:
  Solaris 11.4.
* This function does not support format directives that access arguments in an
  arbitrary order, such as `"%2$s"`, on some platforms:
  NetBSD 3.0, mingw, MSVC 14.
* This function doesn’t support the `'` flag on some platforms:
  NetBSD 3.0, Cygwin 1.5.24, mingw, MSVC 14.
* This function does not round the argument of the ‘`a`’ directive correctly
  on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4.
* printf `"%010f"` of NaN and Infinity yields an incorrect result (padded
  with zeroes) on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, AIX 7.3, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC/clang.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* This function does not support precisions larger than 512 or 1024 in integer,
  floating-point and pointer output on some platforms:
  Solaris 10/x86, mingw, MSVC/clang.
* This function produces wrong output for the ‘`lc`’ directive with a NUL
  wide character argument on some platforms:
  musl libc 1.2.4.
* This function can crash in out-of-memory conditions on some platforms:
  FreeBSD 14.0, NetBSD 5.0.

Portability problems fixed by Gnulib module `obstack-printf-gnu`:

* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.

Portability problems fixed by either Gnulib module `obstack-printf-posix` or `obstack-printf-gnu`
and additionally the Gnulib module `printf-with-n-directive`:

* This function does not support the ‘`n`’ directive on some platforms:
  MSVC 14.
* This function does not fully support the ‘`n`’ directive on some platforms:
  HP-UX 11, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.

---

Previous: [`obstack_printf`](#obstack_005fprintf), Up: [Glibc `<obstack.h>`](#Glibc-obstack_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.36.5 `obstack_vprintf` [¶](#obstack_005fvprintf-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Variable-Arguments-Output.html>.

Gnulib module: obstack-printf or obstack-printf-posix or obstack-printf-gnu

Portability problems fixed by either Gnulib module
`obstack-printf` or `obstack-printf-posix` or `obstack-printf-gnu`:

* This function is missing on all non-glibc platforms:
  musl libc, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.

Portability problems fixed by either Gnulib module `obstack-printf-posix` or `obstack-printf-gnu`:

* This function does not support size specifiers as in C99 (`hh`, `ll`,
  `j`, `t`, `z`) on some platforms:
  AIX 5.1, HP-UX 11.23, Solaris 9, Cygwin 1.5.24, old mingw, MSVC 9.
* This function does not support size specifiers as in C23 (`w8`,
  `w16`, `w32`, `w64`, `wf8`, `wf16`, `wf32`,
  `wf64`) on some platforms:
  glibc 2.37.
* printf of ‘`long double`’ numbers is unsupported on some platforms:
  mingw, MSVC 14.
* printf `"%f"`, `"%e"`, `"%g"` of Infinity and NaN yields an
  incorrect result on some platforms:
  AIX 7.3, Solaris 11.4, mingw, MSVC 14.
* This function doesn’t support the `'` flag at all on some platforms:
  mingw without `__USE_MINGW_ANSI_STDIO`, MSVC 14.
* This function doesn’t support the `'` flag correctly for integers
  (it uses the decimal point instead of the thousands separator!)
  on some platforms:
  mingw with `__USE_MINGW_ANSI_STDIO`.
* This function doesn’t support the `'` flag together with a precision
  on an integer argument correctly on some platforms:
  glibc 2.36, FreeBSD 14.0, NetBSD 10.0, Solaris 11, Cygwin 3.6.0, Haiku.
* This function doesn’t support the `'` flag correctly in locales where
  the thousands separator is a multibyte character on some platforms:
  NetBSD 10.0, Solaris 11 OpenIndiana.
* This function does not support the ‘`a`’ and ‘`A`’ directives on some
  platforms:
  FreeBSD 5.2.1, OpenBSD 4.0, AIX 5.2, HP-UX 11,
  Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`b`’ directive, required by ISO C23,
  on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9,
  AIX 7.3, HP-UX 11, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android 9.0.
* This function does not support the ‘`F`’ directive on some platforms:
  NetBSD 3.0, AIX 5.1, HP-UX 11.23, Solaris 9,
  Cygwin 1.5.x, mingw, MSVC 14.
* This function does not support the ‘`ls`’ directive on some platforms:
  OpenBSD 4.0, Cygwin 1.5.x, Haiku.
* This function does not support precisions in the ‘`ls`’ directive correctly
  on some platforms:
  Solaris 11.4.
* This function does not support format directives that access arguments in an
  arbitrary order, such as `"%2$s"`, on some platforms:
  NetBSD 3.0, mingw, MSVC 14.
* This function doesn’t support the `'` flag on some platforms:
  NetBSD 3.0, Cygwin 1.5.24, mingw, MSVC 14.
* This function does not round the argument of the ‘`a`’ directive correctly
  on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4.
* printf `"%010f"` of NaN and Infinity yields an incorrect result (padded
  with zeroes) on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, AIX 7.3, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC/clang.
* printf `"%#.0x"` or `"%#.0X"` with a zero argument yields an
  incorrect result (non-empty) on some platforms:
  Mac OS X 10.6.
* This function does not support precisions larger than 512 or 1024 in integer,
  floating-point and pointer output on some platforms:
  Solaris 10/x86, mingw, MSVC/clang.
* This function produces wrong output for the ‘`lc`’ directive with a NUL
  wide character argument on some platforms:
  musl libc 1.2.4.
* This function can crash in out-of-memory conditions on some platforms:
  FreeBSD 14.0, NetBSD 5.0.

Portability problems fixed by Gnulib module `obstack-printf-gnu`:

* This function does not support the ‘`B`’ directive on some platforms:
  glibc 2.34, musl libc, macOS 14, FreeBSD 13.2, NetBSD 10.0, OpenBSD 7.9, AIX 7.3, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC, Android 9.0.

Portability problems fixed by either Gnulib module `obstack-printf-posix` or `obstack-printf-gnu`
and additionally the Gnulib module `printf-with-n-directive`:

* This function does not support the ‘`n`’ directive on some platforms:
  MSVC 14.
* This function does not fully support the ‘`n`’ directive on some platforms:
  HP-UX 11, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* The `%m` directive is not portable, use `%s` mapped to an
  argument of `strerror(errno)` (or a version of `strerror_r`)
  instead.

---

Next: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh), Previous: [Glibc `<obstack.h>`](#Glibc-obstack_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.37 Glibc `<printf.h>` [¶](#Glibc-_003cprintf_002eh_003e)

* [`parse_printf_format`](#parse_005fprintf_005fformat)
* [`printf_size`](#printf_005fsize)
* [`printf_size_info`](#printf_005fsize_005finfo)
* [`register_printf_function`](#register_005fprintf_005ffunction)
* [`register_printf_modifier`](#register_005fprintf_005fmodifier)
* [`register_printf_specifier`](#register_005fprintf_005fspecifier)
* [`register_printf_type`](#register_005fprintf_005ftype)

---

Next: [`printf_size`](#printf_005fsize), Up: [Glibc `<printf.h>`](#Glibc-printf_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.37.1 `parse_printf_format` [¶](#parse_005fprintf_005fformat-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Parsing-a-Template-String.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`printf_size_info`](#printf_005fsize_005finfo), Previous: [`parse_printf_format`](#parse_005fprintf_005fformat), Up: [Glibc `<printf.h>`](#Glibc-printf_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.37.2 `printf_size` [¶](#printf_005fsize-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Predefined-Printf-Handlers.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`register_printf_function`](#register_005fprintf_005ffunction), Previous: [`printf_size`](#printf_005fsize), Up: [Glibc `<printf.h>`](#Glibc-printf_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.37.3 `printf_size_info` [¶](#printf_005fsize_005finfo-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Predefined-Printf-Handlers.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`register_printf_modifier`](#register_005fprintf_005fmodifier), Previous: [`printf_size_info`](#printf_005fsize_005finfo), Up: [Glibc `<printf.h>`](#Glibc-printf_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.37.4 `register_printf_function` [¶](#register_005fprintf_005ffunction-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Registering-New-Conversions.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`register_printf_specifier`](#register_005fprintf_005fspecifier), Previous: [`register_printf_function`](#register_005fprintf_005ffunction), Up: [Glibc `<printf.h>`](#Glibc-printf_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.37.5 `register_printf_modifier` [¶](#register_005fprintf_005fmodifier-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`register_printf_type`](#register_005fprintf_005ftype), Previous: [`register_printf_modifier`](#register_005fprintf_005fmodifier), Up: [Glibc `<printf.h>`](#Glibc-printf_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.37.6 `register_printf_specifier` [¶](#register_005fprintf_005fspecifier-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`register_printf_specifier`](#register_005fprintf_005fspecifier), Up: [Glibc `<printf.h>`](#Glibc-printf_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.37.7 `register_printf_type` [¶](#register_005fprintf_005ftype-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.9, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<pty.h>`](#Glibc-pty_002eh), Previous: [Glibc `<printf.h>`](#Glibc-printf_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.38 Glibc Extensions to `<pthread.h>` [¶](#Glibc-Extensions-to-_003cpthread_002eh_003e)

* [`pthread_attr_getaffinity_np`](#pthread_005fattr_005fgetaffinity_005fnp)
* [`pthread_attr_setaffinity_np`](#pthread_005fattr_005fsetaffinity_005fnp)
* [`pthread_attr_getsigmask_np`](#pthread_005fattr_005fgetsigmask_005fnp)
* [`pthread_attr_setsigmask_np`](#pthread_005fattr_005fsetsigmask_005fnp)
* [`pthread_clockjoin_np`](#pthread_005fclockjoin_005fnp)
* [`pthread_getaffinity_np`](#pthread_005fgetaffinity_005fnp)
* [`pthread_getattr_default_np`](#pthread_005fgetattr_005fdefault_005fnp)
* [`pthread_getattr_np`](#pthread_005fgetattr_005fnp)
* [`pthread_getname_np`](#pthread_005fgetname_005fnp)
* [`pthread_gettid_np`](#pthread_005fgettid_005fnp)
* [`pthread_kill_other_threads_np`](#pthread_005fkill_005fother_005fthreads_005fnp)
* [`pthread_rwlockattr_getkind_np`](#pthread_005frwlockattr_005fgetkind_005fnp)
* [`pthread_rwlockattr_setkind_np`](#pthread_005frwlockattr_005fsetkind_005fnp)
* [`pthread_setaffinity_np`](#pthread_005fsetaffinity_005fnp)
* [`pthread_setattr_default_np`](#pthread_005fsetattr_005fdefault_005fnp)
* [`pthread_setname_np`](#pthread_005fsetname_005fnp)
* [`pthread_sigqueue`](#pthread_005fsigqueue)
* [`pthread_timedjoin_np`](#pthread_005ftimedjoin_005fnp)
* [`pthread_tryjoin_np`](#pthread_005ftryjoin_005fnp)

---

Next: [`pthread_attr_setaffinity_np`](#pthread_005fattr_005fsetaffinity_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.1 `pthread_attr_getaffinity_np` [¶](#pthread_005fattr_005fgetaffinity_005fnp-1)

Documentation:  
 [man pthread\_attr\_getaffinity\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_attr_getaffinity_np.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.3.2, macOS 14, FreeBSD 6.4, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function has a different signature on some platforms:
  glibc 2.3.3.

---

Next: [`pthread_attr_getsigmask_np`](#pthread_005fattr_005fgetsigmask_005fnp), Previous: [`pthread_attr_getaffinity_np`](#pthread_005fattr_005fgetaffinity_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.2 `pthread_attr_setaffinity_np` [¶](#pthread_005fattr_005fsetaffinity_005fnp-1)

Documentation:  
 [man pthread\_attr\_setaffinity\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_attr_setaffinity_np.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.3.2, macOS 14, FreeBSD 6.4, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.
* This function has a different signature on some platforms:
  glibc 2.3.3.

---

Next: [`pthread_attr_setsigmask_np`](#pthread_005fattr_005fsetsigmask_005fnp), Previous: [`pthread_attr_setaffinity_np`](#pthread_005fattr_005fsetaffinity_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.3 `pthread_attr_getsigmask_np` [¶](#pthread_005fattr_005fgetsigmask_005fnp-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Initial-Thread-Signal-Mask.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.31, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_clockjoin_np`](#pthread_005fclockjoin_005fnp), Previous: [`pthread_attr_getsigmask_np`](#pthread_005fattr_005fgetsigmask_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.4 `pthread_attr_setsigmask_np` [¶](#pthread_005fattr_005fsetsigmask_005fnp-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Initial-Thread-Signal-Mask.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.31, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_getaffinity_np`](#pthread_005fgetaffinity_005fnp), Previous: [`pthread_attr_setsigmask_np`](#pthread_005fattr_005fsetsigmask_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.5 `pthread_clockjoin_np` [¶](#pthread_005fclockjoin_005fnp-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Default-Thread-Attributes.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_getattr_default_np`](#pthread_005fgetattr_005fdefault_005fnp), Previous: [`pthread_clockjoin_np`](#pthread_005fclockjoin_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.6 `pthread_getaffinity_np` [¶](#pthread_005fgetaffinity_005fnp-1)

Documentation:  
 [man pthread\_getaffinity\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_getaffinity_np.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.3.2, macOS 14, FreeBSD 6.4, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.0.x, mingw, MSVC 14, Android API level 35.
* This function has a different signature on some platforms:
  glibc 2.3.3.
* The third parameter has a different type on some platforms:
  FreeBSD 14.0, NetBSD 10.0.

---

Next: [`pthread_getattr_np`](#pthread_005fgetattr_005fnp), Previous: [`pthread_getaffinity_np`](#pthread_005fgetaffinity_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.7 `pthread_getattr_default_np` [¶](#pthread_005fgetattr_005fdefault_005fnp-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Default-Thread-Attributes.html>,
* [man pthread\_getattr\_default\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_getattr_default_np.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.17, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_getname_np`](#pthread_005fgetname_005fnp), Previous: [`pthread_getattr_default_np`](#pthread_005fgetattr_005fdefault_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.8 `pthread_getattr_np` [¶](#pthread_005fgetattr_005fnp-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-pthread-getattr-np-1.html>

Documentation:  
 [man pthread\_getattr\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_getattr_np.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.9, mingw, MSVC 14.

---

Next: [`pthread_gettid_np`](#pthread_005fgettid_005fnp), Previous: [`pthread_getattr_np`](#pthread_005fgetattr_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.9 `pthread_getname_np` [¶](#pthread_005fgetname_005fnp-1)

Documentation:  
 [man pthread\_getname\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_getname_np.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.11, Mac OS X 10.5, FreeBSD 12.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.0, Cygwin 1.7.x, mingw, MSVC 14, Android 7.1.
  See also <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3554.htm>.

---

Next: [`pthread_kill_other_threads_np`](#pthread_005fkill_005fother_005fthreads_005fnp), Previous: [`pthread_getname_np`](#pthread_005fgetname_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.10 `pthread_gettid_np` [¶](#pthread_005fgettid_005fnp-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Process-Identification.html>,
* [man pthread\_gettid\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_gettid_np.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on most non-glibc platforms:
  glibc 2.41, macOS 15, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.7, Minix, AIX 7.3, HP-UX 11.31, Solaris 11.4, Cygwin, mingw, MSVC 14.

---

Next: [`pthread_rwlockattr_getkind_np`](#pthread_005frwlockattr_005fgetkind_005fnp), Previous: [`pthread_gettid_np`](#pthread_005fgettid_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.11 `pthread_kill_other_threads_np` [¶](#pthread_005fkill_005fother_005fthreads_005fnp-1)

Documentation:  
 [man pthread\_kill\_other\_threads\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_kill_other_threads_np.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_rwlockattr_setkind_np`](#pthread_005frwlockattr_005fsetkind_005fnp), Previous: [`pthread_kill_other_threads_np`](#pthread_005fkill_005fother_005fthreads_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.12 `pthread_rwlockattr_getkind_np` [¶](#pthread_005frwlockattr_005fgetkind_005fnp-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-pthread-rwlockattr-getkind-np-1.html>

Documentation:  
 [man pthread\_rwlockattr\_getkind\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_rwlockattr_getkind_np.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 5.1.

---

Next: [`pthread_setaffinity_np`](#pthread_005fsetaffinity_005fnp), Previous: [`pthread_rwlockattr_getkind_np`](#pthread_005frwlockattr_005fgetkind_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.13 `pthread_rwlockattr_setkind_np` [¶](#pthread_005frwlockattr_005fsetkind_005fnp-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-pthread-rwlockattr-getkind-np-1.html>

Documentation:  
 [man pthread\_rwlockattr\_setkind\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_rwlockattr_setkind_np.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 5.1.

---

Next: [`pthread_setattr_default_np`](#pthread_005fsetattr_005fdefault_005fnp), Previous: [`pthread_rwlockattr_setkind_np`](#pthread_005frwlockattr_005fsetkind_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.14 `pthread_setaffinity_np` [¶](#pthread_005fsetaffinity_005fnp-1)

Documentation:  
 [man pthread\_setaffinity\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_setaffinity_np.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.3.2, macOS 14, FreeBSD 6.4, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.0.x, mingw, MSVC 14, Android API level 35.
* This function has a different signature on some platforms:
  glibc 2.3.3.
* The third parameter has a different type on some platforms:
  FreeBSD 14.0, NetBSD 10.0.

---

Next: [`pthread_setname_np`](#pthread_005fsetname_005fnp), Previous: [`pthread_setaffinity_np`](#pthread_005fsetaffinity_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.15 `pthread_setattr_default_np` [¶](#pthread_005fsetattr_005fdefault_005fnp-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Default-Thread-Attributes.html>,
* [man pthread\_setattr\_default\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_setattr_default_np.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.17, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_sigqueue`](#pthread_005fsigqueue), Previous: [`pthread_setattr_default_np`](#pthread_005fsetattr_005fdefault_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.16 `pthread_setname_np` [¶](#pthread_005fsetname_005fnp-1)

Documentation:  
 [man pthread\_setname\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_setname_np.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.11, Mac OS X 10.5, FreeBSD 12.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.0, Cygwin 1.7.x, mingw, MSVC 14.
  See also <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3554.htm>.
* This function has a different signature on some platforms:
  macOS 14, NetBSD 10.0.
  On macOS the function takes only one argument, the name.
  On NetBSD the second argument is interpreted as a `printf` format string,
  with the third argument as parameter.

---

Next: [`pthread_timedjoin_np`](#pthread_005ftimedjoin_005fnp), Previous: [`pthread_setname_np`](#pthread_005fsetname_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.17 `pthread_sigqueue` [¶](#pthread_005fsigqueue-1)

Documentation:  
 [man pthread\_sigqueue](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_sigqueue.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.10, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.0, Cygwin 1.7.9, mingw, MSVC 14, Android 9.0.

---

Next: [`pthread_tryjoin_np`](#pthread_005ftryjoin_005fnp), Previous: [`pthread_sigqueue`](#pthread_005fsigqueue), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.18 `pthread_timedjoin_np` [¶](#pthread_005ftimedjoin_005fnp-1)

Documentation:  
 [man pthread\_timedjoin\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_timedjoin_np.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.3.2, macOS 14, FreeBSD 6.4, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.10.x, mingw, MSVC 14, Android 9.0.
* FreeBSD 6.4 has a function of this name in `libthr` but not in
  `libpthread`, and it also is missing a declaration.

---

Previous: [`pthread_timedjoin_np`](#pthread_005ftimedjoin_005fnp), Up: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.38.19 `pthread_tryjoin_np` [¶](#pthread_005ftryjoin_005fnp-1)

Documentation:  
 [man pthread\_tryjoin\_np](https://www.kernel.org/doc/man-pages/online/pages/man3/pthread_tryjoin_np.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.3.2, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.10.x, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc Extensions to `<pwd.h>`](#Glibc-pwd_002eh), Previous: [Glibc Extensions to `<pthread.h>`](#Glibc-pthread_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.39 Glibc `<pty.h>` [¶](#Glibc-_003cpty_002eh_003e)

* [`forkpty`](#forkpty)
* [`openpty`](#openpty)

---

Next: [`openpty`](#openpty), Up: [Glibc `<pty.h>`](#Glibc-pty_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.39.1 `forkpty` [¶](#forkpty-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Pseudo_002dTerminal-Pairs.html>,
* [man forkpty](https://www.kernel.org/doc/man-pages/online/pages/man3/forkpty.3.html).

Gnulib module: forkpty

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Android 5.1.
* One some systems (at least including Cygwin
  and Mac OS X) linking with `-lutil` is not required.
* On glibc, OpenBSD, NetBSD and FreeBSD linking with `-lutil` is
  required.
* The function is declared in pty.h on glibc and Cygwin.
  It is declared in util.h on Mac OS X, OpenBSD, and NetBSD.
  It is declared in libutil.h on FreeBSD.
  It is declared in termios.h on Solaris.
* Some platforms declare the function without marking the last two
  parameters `const`.
  FreeBSD 14.0, Solaris 11.4, Cygwin 1.7.1.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Previous: [`forkpty`](#forkpty), Up: [Glibc `<pty.h>`](#Glibc-pty_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.39.2 `openpty` [¶](#openpty-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Pseudo_002dTerminal-Pairs.html>,
* [man openpty](https://www.kernel.org/doc/man-pages/online/pages/man3/openpty.3.html).

Gnulib module: openpty

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, HP-UX 11, Solaris 11.3, Android 5.1.
* One some systems (at least including Cygwin
  and Mac OS X) linking with `-lutil` is not required.
* On glibc, OpenBSD, NetBSD and FreeBSD linking with `-lutil` is
  required.
* The function is declared in pty.h on glibc and Cygwin.
  It is declared in util.h on Mac OS X, OpenBSD, and NetBSD.
  It is declared in libutil.h on FreeBSD.
  It is declared in termios.h on Solaris.
* Some platforms declare the function without marking the last two
  parameters `const`.
  FreeBSD 14.0, Solaris 11.4, Cygwin 1.7.1.

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [Glibc Extensions to `<regex.h>`](#Glibc-regex_002eh), Previous: [Glibc `<pty.h>`](#Glibc-pty_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.40 Glibc Extensions to `<pwd.h>` [¶](#Glibc-Extensions-to-_003cpwd_002eh_003e)

* [`fgetpwent`](#fgetpwent)
* [`fgetpwent_r`](#fgetpwent_005fr)
* [`getpw`](#getpw)
* [`getpwent_r`](#getpwent_005fr)
* [`putpwent`](#putpwent)

---

Next: [`fgetpwent_r`](#fgetpwent_005fr), Up: [Glibc Extensions to `<pwd.h>`](#Glibc-pwd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.40.1 `fgetpwent` [¶](#fgetpwent-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Scanning-All-Users.html>,
* [man fgetpwent](https://www.kernel.org/doc/man-pages/online/pages/man3/fgetpwent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getpw`](#getpw), Previous: [`fgetpwent`](#fgetpwent), Up: [Glibc Extensions to `<pwd.h>`](#Glibc-pwd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.40.2 `fgetpwent_r` [¶](#fgetpwent_005fr-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Scanning-All-Users.html>,
* [man fgetpwent\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/fgetpwent_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`getpwent_r`](#getpwent_005fr), Previous: [`fgetpwent_r`](#fgetpwent_005fr), Up: [Glibc Extensions to `<pwd.h>`](#Glibc-pwd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.40.3 `getpw` [¶](#getpw-1)

Documentation:  
 [man getpw](https://www.kernel.org/doc/man-pages/online/pages/man3/getpw.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`putpwent`](#putpwent), Previous: [`getpw`](#getpw), Up: [Glibc Extensions to `<pwd.h>`](#Glibc-pwd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.40.4 `getpwent_r` [¶](#getpwent_005fr-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getpwent-r-1.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Scanning-All-Users.html>,
* [man getpwent\_r](https://www.kernel.org/doc/man-pages/online/pages/man3/getpwent_r.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`getpwent_r`](#getpwent_005fr), Up: [Glibc Extensions to `<pwd.h>`](#Glibc-pwd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.40.5 `putpwent` [¶](#putpwent-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Writing-a-User-Entry.html>,
* [man putpwent](https://www.kernel.org/doc/man-pages/online/pages/man3/putpwent.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<regexp.h>`](#Glibc-regexp_002eh), Previous: [Glibc Extensions to `<pwd.h>`](#Glibc-pwd_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.41 Glibc Extensions to `<regex.h>` [¶](#Glibc-Extensions-to-_003cregex_002eh_003e)

* [`re_comp`](#re_005fcomp)
* [`re_compile_fastmap`](#re_005fcompile_005ffastmap)
* [`re_compile_pattern`](#re_005fcompile_005fpattern)
* [`re_exec`](#re_005fexec)
* [`re_match`](#re_005fmatch)
* [`re_match_2`](#re_005fmatch_005f2)
* [`re_search`](#re_005fsearch)
* [`re_search_2`](#re_005fsearch_005f2)
* [`re_set_registers`](#re_005fset_005fregisters)
* [`re_set_syntax`](#re_005fset_005fsyntax)
* [`re_syntax_options`](#re_005fsyntax_005foptions)

---

Next: [`re_compile_fastmap`](#re_005fcompile_005ffastmap), Up: [Glibc Extensions to `<regex.h>`](#Glibc-regex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.41.1 `re_comp` [¶](#re_005fcomp-1)

Documentation:  
 [man re\_comp](https://www.kernel.org/doc/man-pages/online/pages/man3/re_comp.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`re_compile_pattern`](#re_005fcompile_005fpattern), Previous: [`re_comp`](#re_005fcomp), Up: [Glibc Extensions to `<regex.h>`](#Glibc-regex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.41.2 `re_compile_fastmap` [¶](#re_005fcompile_005ffastmap-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`re_exec`](#re_005fexec), Previous: [`re_compile_fastmap`](#re_005fcompile_005ffastmap), Up: [Glibc Extensions to `<regex.h>`](#Glibc-regex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.41.3 `re_compile_pattern` [¶](#re_005fcompile_005fpattern-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`re_match`](#re_005fmatch), Previous: [`re_compile_pattern`](#re_005fcompile_005fpattern), Up: [Glibc Extensions to `<regex.h>`](#Glibc-regex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.41.4 `re_exec` [¶](#re_005fexec-1)

Documentation:  
 [man re\_exec](https://www.kernel.org/doc/man-pages/online/pages/man3/re_exec.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`re_match_2`](#re_005fmatch_005f2), Previous: [`re_exec`](#re_005fexec), Up: [Glibc Extensions to `<regex.h>`](#Glibc-regex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.41.5 `re_match` [¶](#re_005fmatch-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`re_search`](#re_005fsearch), Previous: [`re_match`](#re_005fmatch), Up: [Glibc Extensions to `<regex.h>`](#Glibc-regex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.41.6 `re_match_2` [¶](#re_005fmatch_005f2-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`re_search_2`](#re_005fsearch_005f2), Previous: [`re_match_2`](#re_005fmatch_005f2), Up: [Glibc Extensions to `<regex.h>`](#Glibc-regex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.41.7 `re_search` [¶](#re_005fsearch-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`re_set_registers`](#re_005fset_005fregisters), Previous: [`re_search`](#re_005fsearch), Up: [Glibc Extensions to `<regex.h>`](#Glibc-regex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.41.8 `re_search_2` [¶](#re_005fsearch_005f2-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`re_set_syntax`](#re_005fset_005fsyntax), Previous: [`re_search_2`](#re_005fsearch_005f2), Up: [Glibc Extensions to `<regex.h>`](#Glibc-regex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.41.9 `re_set_registers` [¶](#re_005fset_005fregisters-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`re_syntax_options`](#re_005fsyntax_005foptions), Previous: [`re_set_registers`](#re_005fset_005fregisters), Up: [Glibc Extensions to `<regex.h>`](#Glibc-regex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.41.10 `re_set_syntax` [¶](#re_005fset_005fsyntax-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`re_set_syntax`](#re_005fset_005fsyntax), Up: [Glibc Extensions to `<regex.h>`](#Glibc-regex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.41.11 `re_syntax_options` [¶](#re_005fsyntax_005foptions-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<resolv.h>`](#Glibc-resolv_002eh), Previous: [Glibc Extensions to `<regex.h>`](#Glibc-regex_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.42 Glibc `<regexp.h>` [¶](#Glibc-_003cregexp_002eh_003e)

* [`advance`](#advance)
* [`loc1`](#loc1)
* [`loc2`](#loc2)
* [`locs`](#locs)
* [`step`](#step)

---

Next: [`loc1`](#loc1), Up: [Glibc `<regexp.h>`](#Glibc-regexp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.42.1 `advance` [¶](#advance-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`loc2`](#loc2), Previous: [`advance`](#advance), Up: [Glibc `<regexp.h>`](#Glibc-regexp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.42.2 `loc1` [¶](#loc1-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`locs`](#locs), Previous: [`loc1`](#loc1), Up: [Glibc `<regexp.h>`](#Glibc-regexp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.42.3 `loc2` [¶](#loc2-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`step`](#step), Previous: [`loc2`](#loc2), Up: [Glibc `<regexp.h>`](#Glibc-regexp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.42.4 `locs` [¶](#locs-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`locs`](#locs), Up: [Glibc `<regexp.h>`](#Glibc-regexp_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.42.5 `step` [¶](#step-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<rpc/auth.h>`](#Glibc-rpc_002fauth_002eh), Previous: [Glibc `<regexp.h>`](#Glibc-regexp_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
