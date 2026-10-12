### 14.81 Glibc Extensions to `<sys/mman.h>` [¶](#Glibc-Extensions-to-_003csys_002fmman_002eh_003e)

* [`madvise`](#madvise)
* [`memfd_create`](#memfd_005fcreate)
* [`mincore`](#mincore)
* [`mlock2`](#mlock2)
* [`mremap`](#mremap)
* [`pkey_alloc`](#pkey_005falloc)
* [`pkey_free`](#pkey_005ffree)
* [`pkey_get`](#pkey_005fget)
* [`pkey_mprotect`](#pkey_005fmprotect)
* [`pkey_set`](#pkey_005fset)
* [`process_madvise`](#process_005fmadvise)
* [`process_mrelease`](#process_005fmrelease)
* [`remap_file_pages`](#remap_005ffile_005fpages)

---

Next: [`memfd_create`](#memfd_005fcreate), Up: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.81.1 `madvise` [¶](#madvise-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Memory_002dmapped-I_002fO.html>,
* [man madvise](https://www.kernel.org/doc/man-pages/online/pages/man2/madvise.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Cygwin 1.7.7, mingw, MSVC 14.
* On Solaris 10 this function accepts `char *` not `void *`,
  unless you define feature test macros like `_POSIX_C_SOURCE`
  or `_XOPEN_SOURCE` that may undesirably hide other symbols.
  To work around this problem, callers can limit themselves to either
  `void *` or `char *`.

---

Next: [`mincore`](#mincore), Previous: [`madvise`](#madvise), Up: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.81.2 `memfd_create` [¶](#memfd_005fcreate-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Memory_002dmapped-I_002fO.html>,
* [man memfd\_create](https://www.kernel.org/doc/man-pages/online/pages/man2/memfd_create.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and FreeBSD and is therefore
  missing on many non-glibc platforms:
  glibc 2.26, macOS 14, FreeBSD 12.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android API level 29.

---

Next: [`mlock2`](#mlock2), Previous: [`memfd_create`](#memfd_005fcreate), Up: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.81.3 `mincore` [¶](#mincore-1)

Documentation:  
 [man mincore](https://www.kernel.org/doc/man-pages/online/pages/man2/mincore.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14.
* On Solaris 10 this function accepts `char *` not `void *`,
  unless you define feature test macros like `_POSIX_C_SOURCE`
  or `_XOPEN_SOURCE` that may undesirably hide other symbols.
  To work around this problem, callers can limit themselves to either
  `void *` or `char *`.

---

Next: [`mremap`](#mremap), Previous: [`mincore`](#mincore), Up: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.81.4 `mlock2` [¶](#mlock2-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Page-Lock-Functions.html>,
* [man mlock2](https://www.kernel.org/doc/man-pages/online/pages/man2/mlock2.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many platforms:
  glibc 2.26, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android API level 29.

---

Next: [`pkey_alloc`](#pkey_005falloc), Previous: [`mlock2`](#mlock2), Up: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.81.5 `mremap` [¶](#mremap-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-mremap.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Memory_002dmapped-I_002fO.html>,
* [man mremap](https://www.kernel.org/doc/man-pages/online/pages/man2/mremap.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  macOS 14, FreeBSD 14.0, NetBSD 3.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [`pkey_free`](#pkey_005ffree), Previous: [`mremap`](#mremap), Up: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.81.6 `pkey_alloc` [¶](#pkey_005falloc-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Memory-Protection.html>,
* [man pkey\_alloc](https://www.kernel.org/doc/man-pages/online/pages/man2/pkey_alloc.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on all non-glibc platforms:
  glibc 2.26, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`pkey_get`](#pkey_005fget), Previous: [`pkey_alloc`](#pkey_005falloc), Up: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.81.7 `pkey_free` [¶](#pkey_005ffree-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Memory-Protection.html>,
* [man pkey\_free](https://www.kernel.org/doc/man-pages/online/pages/man2/pkey_free.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on all non-glibc platforms:
  glibc 2.26, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`pkey_mprotect`](#pkey_005fmprotect), Previous: [`pkey_free`](#pkey_005ffree), Up: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.81.8 `pkey_get` [¶](#pkey_005fget-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Memory-Protection.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on all non-glibc platforms:
  glibc 2.26, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`pkey_set`](#pkey_005fset), Previous: [`pkey_get`](#pkey_005fget), Up: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.81.9 `pkey_mprotect` [¶](#pkey_005fmprotect-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Memory-Protection.html>,
* [man pkey\_mprotect](https://www.kernel.org/doc/man-pages/online/pages/man2/pkey_mprotect.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on all non-glibc platforms:
  glibc 2.26, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`process_madvise`](#process_005fmadvise), Previous: [`pkey_mprotect`](#pkey_005fmprotect), Up: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.81.10 `pkey_set` [¶](#pkey_005fset-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Memory-Protection.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on all non-glibc platforms:
  glibc 2.26, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`process_mrelease`](#process_005fmrelease), Previous: [`pkey_set`](#pkey_005fset), Up: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.81.11 `process_madvise` [¶](#process_005fmadvise-1)

Documentation:  
 [man process\_madvise](https://www.kernel.org/doc/man-pages/online/pages/man2/process_madvise.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.35, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`remap_file_pages`](#remap_005ffile_005fpages), Previous: [`process_madvise`](#process_005fmadvise), Up: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.81.12 `process_mrelease` [¶](#process_005fmrelease-1)

Documentation:  
 <https://lwn.net/Articles/864184/>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.35, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Previous: [`process_mrelease`](#process_005fmrelease), Up: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.81.13 `remap_file_pages` [¶](#remap_005ffile_005fpages-1)

Documentation:  
 [man remap\_file\_pages](https://www.kernel.org/doc/man-pages/online/pages/man2/remap_file_pages.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<sys/personality.h>`](#Glibc-sys_002fpersonality_002eh), Previous: [Glibc Extensions to `<sys/mman.h>`](#Glibc-sys_002fmman_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.82 Glibc `<sys/mount.h>` [¶](#Glibc-_003csys_002fmount_002eh_003e)

* [`fsconfig`](#fsconfig)
* [`fsmount`](#fsmount)
* [`fsopen`](#fsopen)
* [`fspick`](#fspick)
* [`mount`](#mount)
* [`mount_setattr`](#mount_005fsetattr)
* [`move_mount`](#move_005fmount)
* [`open_tree`](#open_005ftree)
* [`umount`](#umount)
* [`umount2`](#umount2)

---

Next: [`fsmount`](#fsmount), Up: [Glibc `<sys/mount.h>`](#Glibc-sys_002fmount_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.82.1 `fsconfig` [¶](#fsconfig-1)

Documentation:  
 <https://lore.kernel.org/all/159827191245.306468.4903071494263813779.stgit@warthog.procyon.org.uk/>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.35, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`fsopen`](#fsopen), Previous: [`fsconfig`](#fsconfig), Up: [Glibc `<sys/mount.h>`](#Glibc-sys_002fmount_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.82.2 `fsmount` [¶](#fsmount-1)

Documentation:  
 <https://lore.kernel.org/all/159827190508.306468.12755090833140558156.stgit@warthog.procyon.org.uk/>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.35, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`fspick`](#fspick), Previous: [`fsmount`](#fsmount), Up: [Glibc `<sys/mount.h>`](#Glibc-sys_002fmount_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.82.3 `fsopen` [¶](#fsopen-1)

Documentation:  
 <https://lore.kernel.org/all/159827190508.306468.12755090833140558156.stgit@warthog.procyon.org.uk/>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.35, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`mount`](#mount), Previous: [`fsopen`](#fsopen), Up: [Glibc `<sys/mount.h>`](#Glibc-sys_002fmount_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.82.4 `fspick` [¶](#fspick-1)

Documentation:  
 <https://lore.kernel.org/all/159827189767.306468.1803062787718957199.stgit@warthog.procyon.org.uk/>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.35, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`mount_setattr`](#mount_005fsetattr), Previous: [`fspick`](#fspick), Up: [Glibc `<sys/mount.h>`](#Glibc-sys_002fmount_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.82.5 `mount` [¶](#mount-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Mount_002dUnmount_002dRemount.html>,
* [man mount](https://www.kernel.org/doc/man-pages/online/pages/man2/mount.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`move_mount`](#move_005fmount), Previous: [`mount`](#mount), Up: [Glibc `<sys/mount.h>`](#Glibc-sys_002fmount_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.82.6 `mount_setattr` [¶](#mount_005fsetattr-1)

Documentation:  
 [man mount\_setattr](https://www.kernel.org/doc/man-pages/online/pages/man2/mount_setattr.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.35, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`open_tree`](#open_005ftree), Previous: [`mount_setattr`](#mount_005fsetattr), Up: [Glibc `<sys/mount.h>`](#Glibc-sys_002fmount_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.82.7 `move_mount` [¶](#move_005fmount-1)

Documentation:  
 <https://lore.kernel.org/all/159827189025.306468.4916341547843731338.stgit@warthog.procyon.org.uk/>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.35, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`umount`](#umount), Previous: [`move_mount`](#move_005fmount), Up: [Glibc `<sys/mount.h>`](#Glibc-sys_002fmount_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.82.8 `open_tree` [¶](#open_005ftree-1)

Documentation:  
 <https://lore.kernel.org/all/159827188271.306468.16962617119460123110.stgit@warthog.procyon.org.uk/>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.35, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`umount2`](#umount2), Previous: [`open_tree`](#open_005ftree), Up: [Glibc `<sys/mount.h>`](#Glibc-sys_002fmount_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.82.9 `umount` [¶](#umount-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Mount_002dUnmount_002dRemount.html>,
* [man umount](https://www.kernel.org/doc/man-pages/online/pages/man2/umount.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 5.1, mingw, MSVC 14.

---

Previous: [`umount`](#umount), Up: [Glibc `<sys/mount.h>`](#Glibc-sys_002fmount_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.82.10 `umount2` [¶](#umount2-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Mount_002dUnmount_002dRemount.html>,
* [man umount2](https://www.kernel.org/doc/man-pages/online/pages/man2/umount2.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11.23, Cygwin 2.9, mingw, MSVC 14.

---

Next: [Glibc `<sys/pidfd.h>`](#Glibc-sys_002fpidfd_002eh), Previous: [Glibc `<sys/mount.h>`](#Glibc-sys_002fmount_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.83 Glibc `<sys/personality.h>` [¶](#Glibc-_003csys_002fpersonality_002eh_003e)

* [`personality`](#personality)

---

Up: [Glibc `<sys/personality.h>`](#Glibc-sys_002fpersonality_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.83.1 `personality` [¶](#personality-1)

Documentation:  
 [man personality](https://www.kernel.org/doc/man-pages/online/pages/man2/personality.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 4.0.2.

---

Next: [Glibc `<sys/prctl.h>`](#Glibc-sys_002fprctl_002eh), Previous: [Glibc `<sys/personality.h>`](#Glibc-sys_002fpersonality_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.84 Glibc `<sys/pidfd.h>` [¶](#Glibc-_003csys_002fpidfd_002eh_003e)

* [`pidfd_getfd`](#pidfd_005fgetfd)
* [`pidfd_getpid`](#pidfd_005fgetpid)
* [`pidfd_open`](#pidfd_005fopen)
* [`pidfd_send_signal`](#pidfd_005fsend_005fsignal)

---

Next: [`pidfd_getpid`](#pidfd_005fgetpid), Up: [Glibc `<sys/pidfd.h>`](#Glibc-sys_002fpidfd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.84.1 `pidfd_getfd` [¶](#pidfd_005fgetfd-1)

Documentation:  
 [man pidfd\_getfd](https://www.kernel.org/doc/man-pages/online/pages/man2/pidfd_getfd.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.35, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`pidfd_open`](#pidfd_005fopen), Previous: [`pidfd_getfd`](#pidfd_005fgetfd), Up: [Glibc `<sys/pidfd.h>`](#Glibc-sys_002fpidfd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.84.2 `pidfd_getpid` [¶](#pidfd_005fgetpid-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Querying-a-Process.html>,
* <https://lwn.net/Articles/943022/>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [`pidfd_send_signal`](#pidfd_005fsend_005fsignal), Previous: [`pidfd_getpid`](#pidfd_005fgetpid), Up: [Glibc `<sys/pidfd.h>`](#Glibc-sys_002fpidfd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.84.3 `pidfd_open` [¶](#pidfd_005fopen-1)

Documentation:  
 [man pidfd\_open](https://www.kernel.org/doc/man-pages/online/pages/man2/pidfd_open.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.35, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Previous: [`pidfd_open`](#pidfd_005fopen), Up: [Glibc `<sys/pidfd.h>`](#Glibc-sys_002fpidfd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.84.4 `pidfd_send_signal` [¶](#pidfd_005fsend_005fsignal-1)

Documentation:  
 [man pidfd\_send\_signal](https://www.kernel.org/doc/man-pages/online/pages/man2/pidfd_send_signal.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.35, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3.0, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 14, Android.

---

Next: [Glibc `<sys/profil.h>`](#Glibc-sys_002fprofil_002eh), Previous: [Glibc `<sys/pidfd.h>`](#Glibc-sys_002fpidfd_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.85 Glibc `<sys/prctl.h>` [¶](#Glibc-_003csys_002fprctl_002eh_003e)

* [`prctl`](#prctl)

---

Up: [Glibc `<sys/prctl.h>`](#Glibc-sys_002fprctl_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.85.1 `prctl` [¶](#prctl-1)

Documentation:  
 [man prctl](https://www.kernel.org/doc/man-pages/online/pages/man2/prctl.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14.

---

Next: [Glibc `<sys/ptrace.h>`](#Glibc-sys_002fptrace_002eh), Previous: [Glibc `<sys/prctl.h>`](#Glibc-sys_002fprctl_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.86 Glibc `<sys/profil.h>` [¶](#Glibc-_003csys_002fprofil_002eh_003e)

* [`sprofil`](#sprofil)

---

Up: [Glibc `<sys/profil.h>`](#Glibc-sys_002fprofil_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.86.1 `sprofil` [¶](#sprofil-1)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11.11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<sys/quota.h>`](#Glibc-sys_002fquota_002eh), Previous: [Glibc `<sys/profil.h>`](#Glibc-sys_002fprofil_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.87 Glibc `<sys/ptrace.h>` [¶](#Glibc-_003csys_002fptrace_002eh_003e)

* [`ptrace`](#ptrace)

---

Up: [Glibc `<sys/ptrace.h>`](#Glibc-sys_002fptrace_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.87.1 `ptrace` [¶](#ptrace-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-ptrace-1.html>

Documentation:  
 [man ptrace](https://www.kernel.org/doc/man-pages/online/pages/man2/ptrace.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, Cygwin 2.9, mingw, MSVC 14.

---

Next: [Glibc `<sys/random.h>`](#Glibc-sys_002frandom_002eh), Previous: [Glibc `<sys/ptrace.h>`](#Glibc-sys_002fptrace_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.88 Glibc `<sys/quota.h>` [¶](#Glibc-_003csys_002fquota_002eh_003e)

* [`quotactl`](#quotactl)

---

Up: [Glibc `<sys/quota.h>`](#Glibc-sys_002fquota_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.88.1 `quotactl` [¶](#quotactl-1)

Documentation:  
 [man quotactl](https://www.kernel.org/doc/man-pages/online/pages/man2/quotactl.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Solaris 11.4, Cygwin 1.7.x, mingw, MSVC 14, Android 7.1.

---

Next: [Glibc `<sys/reboot.h>`](#Glibc-sys_002freboot_002eh), Previous: [Glibc `<sys/quota.h>`](#Glibc-sys_002fquota_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.89 Glibc `<sys/random.h>` [¶](#Glibc-_003csys_002frandom_002eh_003e)

* [`getrandom`](#getrandom)

---

Up: [Glibc `<sys/random.h>`](#Glibc-sys_002frandom_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.89.1 `getrandom` [¶](#getrandom-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Unpredictable-Bytes.html>,
* [man getrandom](https://www.kernel.org/doc/man-pages/online/pages/man2/getrandom.2.html).

Gnulib module: getrandom

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.24, macOS 14, FreeBSD 11.0, NetBSD 9.3, OpenBSD 7.9, Minix 3.3, AIX 7.1, HP-UX 11.31, Solaris 11.0, Cygwin 1.7.x, mingw, MSVC 14, Android 8.1.
* This function has a different return type on some platforms:
  Solaris 11.4.

Portability problems not fixed by Gnulib:

* The `GRND_INSECURE` flag is missing on some platforms:
  glibc 2.34, macOS 10.15, GNU/kFreeBSD, FreeBSD 12.0, OpenBSD 7.9,
  Minix 3.3, Haiku.
* The `GRND_RANDOM` flag has different effects on different platforms.
  Some platforms ignore the flag, or yield data that can fail to be
  random in some cases.

Although this function is intended to produce random data, the data’s
security properties may not be appropriate for your application.
For example, identical “random” data streams might be produced by
rebooted virtual machines. If this is of concern you may need to use
additional techniques such as hedging.[2](#FOOT2)

Related modules include `getentropy`, which has a simpler but
more-limited API, and `crypto/gc-random`, which is likely a
better match for code already using the other `crypto` APIs.

---

Next: [Glibc Extensions to `<sys/resource.h>`](#Glibc-sys_002fresource_002eh), Previous: [Glibc `<sys/random.h>`](#Glibc-sys_002frandom_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.90 Glibc `<sys/reboot.h>` [¶](#Glibc-_003csys_002freboot_002eh_003e)

* [`reboot`](#reboot)

---

Up: [Glibc `<sys/reboot.h>`](#Glibc-sys_002freboot_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.90.1 `reboot` [¶](#reboot-1)

Documentation:  
 [man reboot](https://www.kernel.org/doc/man-pages/online/pages/man2/reboot.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, Cygwin 2.9, mingw, MSVC 14.

---

Next: [Glibc Extensions to `<sys/sem.h>`](#Glibc-sys_002fsem_002eh), Previous: [Glibc `<sys/reboot.h>`](#Glibc-sys_002freboot_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.91 Glibc Extensions to `<sys/resource.h>` [¶](#Glibc-Extensions-to-_003csys_002fresource_002eh_003e)

* [`prlimit`](#prlimit)

---

Up: [Glibc Extensions to `<sys/resource.h>`](#Glibc-sys_002fresource_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.91.1 `prlimit` [¶](#prlimit-1)

Documentation:  
 [man prlimit](https://www.kernel.org/doc/man-pages/online/pages/man2/prlimit.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.12, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 6.0.

---

Next: [Glibc `<sys/sendfile.h>`](#Glibc-sys_002fsendfile_002eh), Previous: [Glibc Extensions to `<sys/resource.h>`](#Glibc-sys_002fresource_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.92 Glibc Extensions to `<sys/sem.h>` [¶](#Glibc-Extensions-to-_003csys_002fsem_002eh_003e)

* [`semtimedop`](#semtimedop)

---

Up: [Glibc Extensions to `<sys/sem.h>`](#Glibc-sys_002fsem_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.92.1 `semtimedop` [¶](#semtimedop-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Semaphores.html>,
* [man semtimedop](https://www.kernel.org/doc/man-pages/online/pages/man2/semtimedop.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11.11, Cygwin 2.9, mingw, MSVC 14, Android 7.1.

---

Next: [Glibc `<sys/signalfd.h>`](#Glibc-sys_002fsignalfd_002eh), Previous: [Glibc Extensions to `<sys/sem.h>`](#Glibc-sys_002fsem_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.93 Glibc `<sys/sendfile.h>` [¶](#Glibc-_003csys_002fsendfile_002eh_003e)

* [`sendfile`](#sendfile)

---

Up: [Glibc `<sys/sendfile.h>`](#Glibc-sys_002fsendfile_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.93.1 `sendfile` [¶](#sendfile-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-sendfile.html>

Documentation:  
 [man sendfile](https://www.kernel.org/doc/man-pages/online/pages/man2/sendfile.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, Solaris 11 2010-11, Cygwin 2.9, mingw, MSVC 14, Android 4.4.
* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files larger than 2 GB. The fix is to use the
  `AC_SYS_LARGEFILE` macro.

---

Next: [Glibc `<sys/single_threaded.h>`](#Glibc-sys_002fsingle_005fthreaded_002eh), Previous: [Glibc `<sys/sendfile.h>`](#Glibc-sys_002fsendfile_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.94 Glibc `<sys/signalfd.h>` [¶](#Glibc-_003csys_002fsignalfd_002eh_003e)

* [`signalfd`](#signalfd)

---

Up: [Glibc `<sys/signalfd.h>`](#Glibc-sys_002fsignalfd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.94.1 `signalfd` [¶](#signalfd-1)

Documentation:  
 [man signalfd](https://www.kernel.org/doc/man-pages/online/pages/man2/signalfd.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux, Cygwin, and illumos and is therefore
  missing on many non-glibc platforms:
  glibc 2.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.11.x, mingw, MSVC 14.

---

Next: [Glibc Extensions to `<sys/socket.h>`](#Glibc-sys_002fsocket_002eh), Previous: [Glibc `<sys/signalfd.h>`](#Glibc-sys_002fsignalfd_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.95 Glibc `<sys/single_threaded.h>` [¶](#Glibc-_003csys_002fsingle_005fthreaded_002eh_003e)

* [`__libc_single_threaded`](#g_t_005f_005flibc_005fsingle_005fthreaded)

---

Up: [Glibc `<sys/single_threaded.h>`](#Glibc-sys_002fsingle_005fthreaded_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.95.1 `__libc_single_threaded` [¶](#g_t_005f_005flibc_005fsingle_005fthreaded-1)

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Single_002dThreaded.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This variable is missing on all non-glibc platforms:
  glibc 2.31, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc Extensions to `<sys/stat.h>`](#Glibc-sys_002fstat_002eh), Previous: [Glibc `<sys/single_threaded.h>`](#Glibc-sys_002fsingle_005fthreaded_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.96 Glibc Extensions to `<sys/socket.h>` [¶](#Glibc-Extensions-to-_003csys_002fsocket_002eh_003e)

* [`isfdtype`](#isfdtype)
* [`recvmmsg`](#recvmmsg)
* [`sendmmsg`](#sendmmsg)

---

Next: [`recvmmsg`](#recvmmsg), Up: [Glibc Extensions to `<sys/socket.h>`](#Glibc-sys_002fsocket_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.96.1 `isfdtype` [¶](#isfdtype-1)

Documentation:  
 [man isfdtype](https://www.kernel.org/doc/man-pages/online/pages/man3/isfdtype.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`sendmmsg`](#sendmmsg), Previous: [`isfdtype`](#isfdtype), Up: [Glibc Extensions to `<sys/socket.h>`](#Glibc-sys_002fsocket_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.96.2 `recvmmsg` [¶](#recvmmsg-1)

Documentation:  
 [man recvmmsg](https://www.kernel.org/doc/man-pages/online/pages/man2/recvmmsg.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.11, macOS 14, FreeBSD 6.4, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 2.9, mingw, MSVC 14, Android 4.4.

---

Previous: [`recvmmsg`](#recvmmsg), Up: [Glibc Extensions to `<sys/socket.h>`](#Glibc-sys_002fsocket_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.96.3 `sendmmsg` [¶](#sendmmsg-1)

Documentation:  
 [man sendmmsg](https://www.kernel.org/doc/man-pages/online/pages/man2/sendmmsg.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  glibc 2.13, macOS 14, FreeBSD 6.4, NetBSD 5.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.3, Cygwin 2.9, mingw, MSVC 14, Android 4.4.

---

Next: [Glibc `<sys/statfs.h>`](#Glibc-sys_002fstatfs_002eh), Previous: [Glibc Extensions to `<sys/socket.h>`](#Glibc-sys_002fsocket_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.97 Glibc Extensions to `<sys/stat.h>` [¶](#Glibc-Extensions-to-_003csys_002fstat_002eh_003e)

* [`getumask`](#getumask)
* [`lchmod`](#lchmod)
* [`statx`](#statx)

---

Next: [`lchmod`](#lchmod), Up: [Glibc Extensions to `<sys/stat.h>`](#Glibc-sys_002fstat_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.97.1 `getumask` [¶](#getumask-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Setting-Permissions.html>,
* [man getumask](https://www.kernel.org/doc/man-pages/online/pages/man3/getumask.3.html).

Gnulib module: getumask

Portability problems fixed by Gnulib:

* This function exists only on Hurd and is therefore
  missing on all non-glibc platforms:
  glibc/Linux, glibc/kFreeBSD, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.3, AIX 7.2, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

Portability problems not fixed by Gnulib:

---

Next: [`statx`](#statx), Previous: [`getumask`](#getumask), Up: [Glibc Extensions to `<sys/stat.h>`](#Glibc-sys_002fstat_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.97.2 `lchmod` [¶](#lchmod-1)

Gnulib module: lchmod

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, AIX 5.1, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android API level 35.
* This function is not declared on some platforms:
  HP-UX 11.31.
* This function always fails with `errno` set to `ENOSYS`,
  even when the file is not a symbolic link:
  GNU/Linux with glibc 2.31.
* This function does not fail when the file name argument ends in a slash
  and (without the slash) names a non-directory, on some platforms:
  AIX 7.2.

Portability problems not fixed by Gnulib:

* Some platforms do not allow changing the access bits on symbolic
  links.
* This function can fail with `errno` set to `EMFILE` or `ENFILE`,
  and it fails with `errno` set to `EOPNOTSUPP` if the
  `/proc` file system is not mounted:
  GNU/Linux with glibc 2.34.

---

Previous: [`lchmod`](#lchmod), Up: [Glibc Extensions to `<sys/stat.h>`](#Glibc-sys_002fstat_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.97.3 `statx` [¶](#statx-1)

Documentation:  
 [man statx](https://www.kernel.org/doc/man-pages/online/pages/man2/statx.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.27, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android API level 29.
* When `AT_EMPTY_PATH` is used,
  this function does not allow the file name to be a null pointer:
  glibc 2.40, Linux kernel 6.10.
* There is an incompatible function of the same name on some platforms:
  AIX 5.2 or newer.

---

Next: [Glibc `<sys/swap.h>`](#Glibc-sys_002fswap_002eh), Previous: [Glibc Extensions to `<sys/stat.h>`](#Glibc-sys_002fstat_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.98 Glibc `<sys/statfs.h>` [¶](#Glibc-_003csys_002fstatfs_002eh_003e)

* [`fstatfs`](#fstatfs)
* [`statfs`](#statfs)

---

Next: [`statfs`](#statfs), Up: [Glibc `<sys/statfs.h>`](#Glibc-sys_002fstatfs_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.98.1 `fstatfs` [¶](#fstatfs-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-fstatfs-2.html>

Documentation:  
 [man fstatfs](https://www.kernel.org/doc/man-pages/online/pages/man2/fstatfs.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  AIX 5.1, mingw, MSVC 14.
* On platforms where `f_blocks` in ‘`struct statfs`’ is a 32-bit
  value, this function may not work correctly on files systems larger than
  4 TiB. The fix is to use the `AC_SYS_LARGEFILE` macro. This affects
  Mac OS X.

---

Previous: [`fstatfs`](#fstatfs), Up: [Glibc `<sys/statfs.h>`](#Glibc-sys_002fstatfs_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.98.2 `statfs` [¶](#statfs-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-statfs-2.html>

Documentation:  
 [man statfs](https://www.kernel.org/doc/man-pages/online/pages/man2/statfs.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, mingw, MSVC 14.
* On platforms where `f_blocks` in ‘`struct statfs`’ is a 32-bit
  value, this function may not work correctly on files systems larger than
  4 TiB. The fix is to use the `AC_SYS_LARGEFILE` macro. This affects
  Mac OS X.

---

Next: [Glibc `<sys/sysctl.h>`](#Glibc-sys_002fsysctl_002eh), Previous: [Glibc `<sys/statfs.h>`](#Glibc-sys_002fstatfs_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.99 Glibc `<sys/swap.h>` [¶](#Glibc-_003csys_002fswap_002eh_003e)

* [`swapoff`](#swapoff)
* [`swapon`](#swapon)

---

Next: [`swapon`](#swapon), Up: [Glibc `<sys/swap.h>`](#Glibc-sys_002fswap_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.99.1 `swapoff` [¶](#swapoff-1)

Documentation:  
 [man swapoff](https://www.kernel.org/doc/man-pages/online/pages/man2/swapoff.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 4.3.

---

Previous: [`swapoff`](#swapoff), Up: [Glibc `<sys/swap.h>`](#Glibc-sys_002fswap_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.99.2 `swapon` [¶](#swapon-1)

Documentation:  
 [man swapon](https://www.kernel.org/doc/man-pages/online/pages/man2/swapon.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, AIX 5.1, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 4.3.

---

Next: [Glibc `<sys/sysinfo.h>`](#Glibc-sys_002fsysinfo_002eh), Previous: [Glibc `<sys/swap.h>`](#Glibc-sys_002fswap_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.100 Glibc `<sys/sysctl.h>` [¶](#Glibc-_003csys_002fsysctl_002eh_003e)

* [`sysctl`](#sysctl)

---

Up: [Glibc `<sys/sysctl.h>`](#Glibc-sys_002fsysctl_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.100.1 `sysctl` [¶](#sysctl-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/System-Parameters.html>,
* [man sysctl](https://www.kernel.org/doc/man-pages/online/pages/man2/sysctl.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.34, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<sys/syslog.h>`](#Glibc-sys_002fsyslog_002eh), Previous: [Glibc `<sys/sysctl.h>`](#Glibc-sys_002fsysctl_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.101 Glibc `<sys/sysinfo.h>` [¶](#Glibc-_003csys_002fsysinfo_002eh_003e)

* [`get_avphys_pages`](#get_005favphys_005fpages)
* [`get_nprocs`](#get_005fnprocs)
* [`get_nprocs_conf`](#get_005fnprocs_005fconf)
* [`get_phys_pages`](#get_005fphys_005fpages)
* [`sysinfo`](#sysinfo)

---

Next: [`get_nprocs`](#get_005fnprocs), Up: [Glibc `<sys/sysinfo.h>`](#Glibc-sys_002fsysinfo_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.101.1 `get_avphys_pages` [¶](#get_005favphys_005fpages-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Query-Memory-Parameters.html>,
* [man get\_avphys\_pages](https://www.kernel.org/doc/man-pages/online/pages/man3/get_avphys_pages.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.1, mingw, MSVC 14, Android 5.1.

---

Next: [`get_nprocs_conf`](#get_005fnprocs_005fconf), Previous: [`get_avphys_pages`](#get_005favphys_005fpages), Up: [Glibc `<sys/sysinfo.h>`](#Glibc-sys_002fsysinfo_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.101.2 `get_nprocs` [¶](#get_005fnprocs-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Processor-Resources.html>,
* [man get\_nprocs](https://www.kernel.org/doc/man-pages/online/pages/man3/get_nprocs.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.7.1, mingw, MSVC 14, Android 5.1.

Gnulib provides the module `nproc` that performs a similar
function but is portable to more systems.

---

Next: [`get_phys_pages`](#get_005fphys_005fpages), Previous: [`get_nprocs`](#get_005fnprocs), Up: [Glibc `<sys/sysinfo.h>`](#Glibc-sys_002fsysinfo_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.101.3 `get_nprocs_conf` [¶](#get_005fnprocs_005fconf-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Processor-Resources.html>,
* [man get\_nprocs\_conf](https://www.kernel.org/doc/man-pages/online/pages/man3/get_nprocs_conf.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.7.1, mingw, MSVC 14, Android 5.1.

---

Next: [`sysinfo`](#sysinfo), Previous: [`get_nprocs_conf`](#get_005fnprocs_005fconf), Up: [Glibc `<sys/sysinfo.h>`](#Glibc-sys_002fsysinfo_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.101.4 `get_phys_pages` [¶](#get_005fphys_005fpages-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Query-Memory-Parameters.html>,
* [man get\_phys\_pages](https://www.kernel.org/doc/man-pages/online/pages/man3/get_phys_pages.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 1.7.1, mingw, MSVC 14, Android 5.1.

---

Previous: [`get_phys_pages`](#get_005fphys_005fpages), Up: [Glibc `<sys/sysinfo.h>`](#Glibc-sys_002fsysinfo_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.101.5 `sysinfo` [¶](#sysinfo-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-sysinfo-1.html>

Documentation:  
 [man sysinfo](https://www.kernel.org/doc/man-pages/online/pages/man2/sysinfo.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, Cygwin 1.7.9, mingw, MSVC 14.
* This function is not declared and thus not part of the Android API
  for Android API levels < 9.

---

Next: [Glibc `<sys/sysmacros.h>`](#Glibc-sys_002fsysmacros_002eh), Previous: [Glibc `<sys/sysinfo.h>`](#Glibc-sys_002fsysinfo_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.102 Glibc `<sys/syslog.h>` [¶](#Glibc-_003csys_002fsyslog_002eh_003e)

* [`vsyslog`](#vsyslog)

---

Up: [Glibc `<sys/syslog.h>`](#Glibc-sys_002fsyslog_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.102.1 `vsyslog` [¶](#vsyslog-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-vsyslog-3.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/syslog_003b-vsyslog.html>,
* [man vsyslog](https://www.kernel.org/doc/man-pages/online/pages/man3/vsyslog.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, mingw, MSVC 14.

---

Next: [Glibc Extensions to `<sys/time.h>`](#Glibc-sys_002ftime_002eh), Previous: [Glibc `<sys/syslog.h>`](#Glibc-sys_002fsyslog_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.103 Glibc `<sys/sysmacros.h>` [¶](#Glibc-_003csys_002fsysmacros_002eh_003e)

* [`gnu_dev_major`](#gnu_005fdev_005fmajor)
* [`gnu_dev_makedev`](#gnu_005fdev_005fmakedev)
* [`gnu_dev_minor`](#gnu_005fdev_005fminor)

---

Next: [`gnu_dev_makedev`](#gnu_005fdev_005fmakedev), Up: [Glibc `<sys/sysmacros.h>`](#Glibc-sys_002fsysmacros_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.103.1 `gnu_dev_major` [¶](#gnu_005fdev_005fmajor-1)

Documentation:  
 [man gnu\_dev\_major](https://www.kernel.org/doc/man-pages/online/pages/man3/gnu_dev_major.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

* The `AC_HEADER_MAJOR` macro in Autoconf 2.69 and earlier fails to
  set `MAJOR_IN_SYSMACROS` when it detects namespace pollution in
  `sys/types.h`; which in turn provokes deprecation warnings in
  glibc 2.25.

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`gnu_dev_minor`](#gnu_005fdev_005fminor), Previous: [`gnu_dev_major`](#gnu_005fdev_005fmajor), Up: [Glibc `<sys/sysmacros.h>`](#Glibc-sys_002fsysmacros_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.103.2 `gnu_dev_makedev` [¶](#gnu_005fdev_005fmakedev-1)

Documentation:  
 [man gnu\_dev\_makedev](https://www.kernel.org/doc/man-pages/online/pages/man3/gnu_dev_makedev.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

* The `AC_HEADER_MAJOR` macro in Autoconf 2.69 and earlier fails to
  set `MAJOR_IN_SYSMACROS` when it detects namespace pollution in
  `sys/types.h`; which in turn provokes deprecation warnings in
  glibc 2.25.

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`gnu_dev_makedev`](#gnu_005fdev_005fmakedev), Up: [Glibc `<sys/sysmacros.h>`](#Glibc-sys_002fsysmacros_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.103.3 `gnu_dev_minor` [¶](#gnu_005fdev_005fminor-1)

Documentation:  
 [man gnu\_dev\_minor](https://www.kernel.org/doc/man-pages/online/pages/man3/gnu_dev_minor.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

* The `AC_HEADER_MAJOR` macro in Autoconf 2.69 and earlier fails to
  set `MAJOR_IN_SYSMACROS` when it detects namespace pollution in
  `sys/types.h`; which in turn provokes deprecation warnings in
  glibc 2.25.

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<sys/timerfd.h>`](#Glibc-sys_002ftimerfd_002eh), Previous: [Glibc `<sys/sysmacros.h>`](#Glibc-sys_002fsysmacros_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.104 Glibc Extensions to `<sys/time.h>` [¶](#Glibc-Extensions-to-_003csys_002ftime_002eh_003e)

* [`adjtime`](#adjtime)
* [`futimes`](#futimes)
* [`futimesat`](#futimesat)
* [`lutimes`](#lutimes)
* [`settimeofday`](#settimeofday)

---

Next: [`futimes`](#futimes), Up: [Glibc Extensions to `<sys/time.h>`](#Glibc-sys_002ftime_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.104.1 `adjtime` [¶](#adjtime-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-adjtime-2.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/High_002dResolution-Calendar.html>,
* [man adjtime](https://www.kernel.org/doc/man-pages/online/pages/man3/adjtime.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`futimesat`](#futimesat), Previous: [`adjtime`](#adjtime), Up: [Glibc Extensions to `<sys/time.h>`](#Glibc-sys_002ftime_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.104.2 `futimes` [¶](#futimes-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-futimes.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/File-Times.html>,
* [man futimes](https://www.kernel.org/doc/man-pages/online/pages/man3/futimes.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 7.1.
* This function cannot set full timestamp resolution. Use
  `futimens(fd,times)` instead.

---

Next: [`lutimes`](#lutimes), Previous: [`futimes`](#futimes), Up: [Glibc Extensions to `<sys/time.h>`](#Glibc-sys_002ftime_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.104.3 `futimesat` [¶](#futimesat-1)

Documentation:  
 [man futimesat](https://www.kernel.org/doc/man-pages/online/pages/man2/futimesat.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 1.5.x, mingw, MSVC 14, Android 7.1.
* On some platforms, this function mis-handles a trailing slash:
  Solaris 9.
* This function cannot set full timestamp resolution. Use

  ```
  file ? utimensat(fd, file, times, 0)
       : futimens(fd, times)
  ```

  or
  the gnulib module `fdutimensat`, instead.

---

Next: [`settimeofday`](#settimeofday), Previous: [`futimesat`](#futimesat), Up: [Glibc Extensions to `<sys/time.h>`](#Glibc-sys_002ftime_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.104.4 `lutimes` [¶](#lutimes-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/File-Times.html>,
* [man lutimes](https://www.kernel.org/doc/man-pages/online/pages/man3/lutimes.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, mingw, MSVC 14, Android 7.1.
* This function cannot set full timestamp resolution.
  Use `utimensat(AT_FDCWD,file,times,AT_SYMLINK_NOFOLLOW)`, or the
  gnulib module `utimens`, instead.
* The mere act of using `lstat` modifies the access time of
  symlinks on some platforms, so `lutimes` can only effectively
  change modification time:
  Cygwin.

---

Previous: [`lutimes`](#lutimes), Up: [Glibc Extensions to `<sys/time.h>`](#Glibc-sys_002ftime_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.104.5 `settimeofday` [¶](#settimeofday-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/High_002dResolution-Calendar.html>,
* [man settimeofday](https://www.kernel.org/doc/man-pages/online/pages/man2/settimeofday.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [Glibc `<sys/timex.h>`](#Glibc-sys_002ftimex_002eh), Previous: [Glibc Extensions to `<sys/time.h>`](#Glibc-sys_002ftime_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.105 Glibc `<sys/timerfd.h>` [¶](#Glibc-_003csys_002ftimerfd_002eh_003e)

* [`timerfd_create`](#timerfd_005fcreate)
* [`timerfd_gettime`](#timerfd_005fgettime)
* [`timerfd_settime`](#timerfd_005fsettime)

---

Next: [`timerfd_gettime`](#timerfd_005fgettime), Up: [Glibc `<sys/timerfd.h>`](#Glibc-sys_002ftimerfd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.105.1 `timerfd_create` [¶](#timerfd_005fcreate-1)

Documentation:  
 [man timerfd\_create](https://www.kernel.org/doc/man-pages/online/pages/man2/timerfd_create.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux, FreeBSD, NetBSD, Cygwin, and illumos and is therefore
  missing on many non-glibc platforms:
  glibc 2.7, macOS 14, FreeBSD 13.2, NetBSD 9.3, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.11.x, mingw, MSVC 14, Android 4.3.

---

Next: [`timerfd_settime`](#timerfd_005fsettime), Previous: [`timerfd_create`](#timerfd_005fcreate), Up: [Glibc `<sys/timerfd.h>`](#Glibc-sys_002ftimerfd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.105.2 `timerfd_gettime` [¶](#timerfd_005fgettime-1)

Documentation:  
 [man timerfd\_gettime](https://www.kernel.org/doc/man-pages/online/pages/man2/timerfd_gettime.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux, FreeBSD, NetBSD, Cygwin, and illumos and is therefore
  missing on many non-glibc platforms:
  glibc 2.7, macOS 14, FreeBSD 13.2, NetBSD 9.3, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.11.x, mingw, MSVC 14, Android 4.3.

---

Previous: [`timerfd_gettime`](#timerfd_005fgettime), Up: [Glibc `<sys/timerfd.h>`](#Glibc-sys_002ftimerfd_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.105.3 `timerfd_settime` [¶](#timerfd_005fsettime-1)

Documentation:  
 [man timerfd\_settime](https://www.kernel.org/doc/man-pages/online/pages/man2/timerfd_settime.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux, FreeBSD, NetBSD, Cygwin, and illumos and is therefore
  missing on many non-glibc platforms:
  glibc 2.7, macOS 14, FreeBSD 13.2, NetBSD 9.3, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.11.x, mingw, MSVC 14, Android 4.3.

---

Next: [Glibc Extensions to `<sys/uio.h>`](#Glibc-sys_002fuio_002eh), Previous: [Glibc `<sys/timerfd.h>`](#Glibc-sys_002ftimerfd_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.106 Glibc `<sys/timex.h>` [¶](#Glibc-_003csys_002ftimex_002eh_003e)

* [`adjtimex`](#adjtimex)
* [`ntp_adjtime`](#ntp_005fadjtime)
* [`ntp_gettime`](#ntp_005fgettime)
* [`ntp_gettimex`](#ntp_005fgettimex)

---

Next: [`ntp_adjtime`](#ntp_005fadjtime), Up: [Glibc `<sys/timex.h>`](#Glibc-sys_002ftimex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.106.1 `adjtimex` [¶](#adjtimex-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/High_002dResolution-Calendar.html>,
* [man adjtimex](https://www.kernel.org/doc/man-pages/online/pages/man2/adjtimex.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 6.0.

---

Next: [`ntp_gettime`](#ntp_005fgettime), Previous: [`adjtimex`](#adjtimex), Up: [Glibc `<sys/timex.h>`](#Glibc-sys_002ftimex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.106.2 `ntp_adjtime` [¶](#ntp_005fadjtime-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/High-Accuracy-Clock.html>,
* [man ntp\_adjtime](https://www.kernel.org/doc/man-pages/online/pages/man3/ntp_adjtime.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`ntp_gettimex`](#ntp_005fgettimex), Previous: [`ntp_adjtime`](#ntp_005fadjtime), Up: [Glibc `<sys/timex.h>`](#Glibc-sys_002ftimex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.106.3 `ntp_gettime` [¶](#ntp_005fgettime-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/High-Accuracy-Clock.html>,
* [man ntp\_gettime](https://www.kernel.org/doc/man-pages/online/pages/man3/ntp_gettime.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Previous: [`ntp_gettime`](#ntp_005fgettime), Up: [Glibc `<sys/timex.h>`](#Glibc-sys_002ftimex_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.106.4 `ntp_gettimex` [¶](#ntp_005fgettimex-1)

Documentation:  
 [man ntp\_gettimex](https://www.kernel.org/doc/man-pages/online/pages/man3/ntp_gettimex.3.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on all non-glibc platforms:
  glibc 2.11, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<sys/ustat.h>`](#Glibc-sys_002fustat_002eh), Previous: [Glibc `<sys/timex.h>`](#Glibc-sys_002ftimex_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.107 Glibc Extensions to `<sys/uio.h>` [¶](#Glibc-Extensions-to-_003csys_002fuio_002eh_003e)

* [`preadv`](#preadv)
* [`preadv2`](#preadv2)
* [`process_vm_readv`](#process_005fvm_005freadv)
* [`process_vm_writev`](#process_005fvm_005fwritev)
* [`pwritev`](#pwritev)
* [`pwritev2`](#pwritev2)

---

Next: [`preadv2`](#preadv2), Up: [Glibc Extensions to `<sys/uio.h>`](#Glibc-sys_002fuio_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.107.1 `preadv` [¶](#preadv-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Scatter_002dGather.html>,
* [man preadv](https://www.kernel.org/doc/man-pages/online/pages/man2/preadv.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.9, macOS 10.15, FreeBSD 5.2.1, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 6.0.
* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files larger than 2 GB. The fix is to use the
  `AC_SYS_LARGEFILE` macro.

---

Next: [`process_vm_readv`](#process_005fvm_005freadv), Previous: [`preadv`](#preadv), Up: [Glibc Extensions to `<sys/uio.h>`](#Glibc-sys_002fuio_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.107.2 `preadv2` [¶](#preadv2-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Scatter_002dGather.html>,
* [man preadv2](https://www.kernel.org/doc/man-pages/online/pages/man2/preadv2.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many platforms:
  glibc 2.25, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android API level 32.

---

Next: [`process_vm_writev`](#process_005fvm_005fwritev), Previous: [`preadv2`](#preadv2), Up: [Glibc Extensions to `<sys/uio.h>`](#Glibc-sys_002fuio_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.107.3 `process_vm_readv` [¶](#process_005fvm_005freadv-1)

Documentation:  
 [man process\_vm\_readv](https://www.kernel.org/doc/man-pages/online/pages/man2/process_vm_readv.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.14, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 5.1.

---

Next: [`pwritev`](#pwritev), Previous: [`process_vm_readv`](#process_005fvm_005freadv), Up: [Glibc Extensions to `<sys/uio.h>`](#Glibc-sys_002fuio_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.107.4 `process_vm_writev` [¶](#process_005fvm_005fwritev-1)

Documentation:  
 [man process\_vm\_writev](https://www.kernel.org/doc/man-pages/online/pages/man2/process_vm_writev.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many non-glibc platforms:
  glibc 2.14, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 5.1.

---

Next: [`pwritev2`](#pwritev2), Previous: [`process_vm_writev`](#process_005fvm_005fwritev), Up: [Glibc Extensions to `<sys/uio.h>`](#Glibc-sys_002fuio_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.107.5 `pwritev` [¶](#pwritev-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Scatter_002dGather.html>,
* [man pwritev](https://www.kernel.org/doc/man-pages/online/pages/man2/pwritev.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.9, macOS 10.15, FreeBSD 5.2.1, Minix 3.1.8, AIX 5.2, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 6.0.
* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly on files larger than 2 GB. The fix is to use the
  `AC_SYS_LARGEFILE` macro.

---

Previous: [`pwritev`](#pwritev), Up: [Glibc Extensions to `<sys/uio.h>`](#Glibc-sys_002fuio_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.107.6 `pwritev2` [¶](#pwritev2-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Scatter_002dGather.html>,
* [man pwritev2](https://www.kernel.org/doc/man-pages/online/pages/man2/pwritev2.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function exists only on Linux and is therefore
  missing on many platforms:
  glibc 2.25, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 7.1, HP-UX 11.31, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android API level 32.

---

Next: [Glibc `<sys/vlimit.h>`](#Glibc-sys_002fvlimit_002eh), Previous: [Glibc Extensions to `<sys/uio.h>`](#Glibc-sys_002fuio_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.108 Glibc `<sys/ustat.h>` [¶](#Glibc-_003csys_002fustat_002eh_003e)

* [`ustat`](#ustat)

---

Up: [Glibc `<sys/ustat.h>`](#Glibc-sys_002fustat_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.108.1 `ustat` [¶](#ustat-1)

Documentation:  
 [man ustat](https://www.kernel.org/doc/man-pages/online/pages/man2/ustat.2.html)

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc Extensions to `<sys/wait.h>`](#Glibc-sys_002fwait_002eh), Previous: [Glibc `<sys/ustat.h>`](#Glibc-sys_002fustat_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.109 Glibc `<sys/vlimit.h>` [¶](#Glibc-_003csys_002fvlimit_002eh_003e)

* [`vlimit`](#vlimit)

---

Up: [Glibc `<sys/vlimit.h>`](#Glibc-sys_002fvlimit_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.109.1 `vlimit` [¶](#vlimit-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Limits-on-Resources.html>,
* [man vlimit](https://www.kernel.org/doc/man-pages/online/pages/man3/vlimit.3.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [Glibc `<sys/xattr.h>`](#Glibc-sys_002fxattr_002eh), Previous: [Glibc `<sys/vlimit.h>`](#Glibc-sys_002fvlimit_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 14.110 Glibc Extensions to `<sys/wait.h>` [¶](#Glibc-Extensions-to-_003csys_002fwait_002eh_003e)

* [`wait3`](#wait3)
* [`wait4`](#wait4)

---

Next: [`wait4`](#wait4), Up: [Glibc Extensions to `<sys/wait.h>`](#Glibc-sys_002fwait_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.110.1 `wait3` [¶](#wait3-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/BSD-Wait-Functions.html>,
* [man wait3](https://www.kernel.org/doc/man-pages/online/pages/man2/wait3.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 9.0.
* This function is not declared on some platforms:
  Android 13.

---

Previous: [`wait3`](#wait3), Up: [Glibc Extensions to `<sys/wait.h>`](#Glibc-sys_002fwait_002eh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 14.110.2 `wait4` [¶](#wait4-1)

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-wait4-2.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Process-Completion.html>,
* [man wait4](https://www.kernel.org/doc/man-pages/online/pages/man2/wait4.2.html).

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11.11, mingw, MSVC 14, Android 4.2.

---

Next: [Glibc Extensions to `<termios.h>`](#Glibc-termios_002eh), Previous: [Glibc Extensions to `<sys/wait.h>`](#Glibc-sys_002fwait_002eh), Up: [Glibc Function Substitutes](#Glibc-Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
