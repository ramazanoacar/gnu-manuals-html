### 10.47 Functions in `<string.h>` [¶](#Functions-in-_003cstring_002eh_003e-1)

* [`memccpy`](#memccpy)
* [`memchr`](#memchr)
* [`memcmp`](#memcmp)
* [`memcpy`](#memcpy)
* [`memmem`](#memmem)
* [`memmove`](#memmove)
* [`memset`](#memset)
* [`memset_explicit`](#memset_005fexplicit)
* [`stpcpy`](#stpcpy)
* [`stpncpy`](#stpncpy)
* [`strcat`](#strcat)
* [`strchr`](#strchr)
* [`strcmp`](#strcmp)
* [`strcoll`](#strcoll)
* [`strcoll_l`](#strcoll_005fl)
* [`strcpy`](#strcpy)
* [`strcspn`](#strcspn)
* [`strdup`](#strdup)
* [`strerror`](#strerror)
* [`strerror_l`](#strerror_005fl)
* [`strerror_r`](#strerror_005fr)
* [`strlcat`](#strlcat)
* [`strlcpy`](#strlcpy)
* [`strlen`](#strlen)
* [`strncat`](#strncat)
* [`strncmp`](#strncmp)
* [`strncpy`](#strncpy)
* [`strndup`](#strndup)
* [`strnlen`](#strnlen)
* [`strpbrk`](#strpbrk)
* [`strrchr`](#strrchr)
* [`strsignal`](#strsignal)
* [`strspn`](#strspn)
* [`strstr`](#strstr)
* [`strtok`](#strtok)
* [`strtok_r`](#strtok_005fr)
* [`strxfrm`](#strxfrm)
* [`strxfrm_l`](#strxfrm_005fl)

---

Next: [`memchr`](#memchr), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.1 `memccpy` [¶](#memccpy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/memccpy.html>

Gnulib module: memccpy

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`memcmp`](#memcmp), Previous: [`memccpy`](#memccpy), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.2 `memchr` [¶](#memchr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/memchr.html>

Gnulib module: memchr

Portability problems fixed by Gnulib:

* This function dereferences too much memory on some platforms:
  glibc 2.10 on x86\_64, IA-64; glibc 2.11 on Alpha, AIX 7.2.
* This function returns NULL if the character argument is not in the range
  of an `unsigned char` on some platforms:
  Android 5.0.

Portability problems not fixed by Gnulib:

---

Next: [`memcpy`](#memcpy), Previous: [`memchr`](#memchr), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.3 `memcmp` [¶](#memcmp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/memcmp.html>

Gnulib module: memcmp

Portability problems fixed by Gnulib:

* This function fails when comparing 16 bytes or more and with at least one
  buffer not starting on a 4-byte boundary on some older platforms:
  NeXTstep/x86.

Portability problems not fixed by Gnulib:

* This function produces wrong results when the arguments are constant strings
  and the compiler is clang 6.0.1.

---

Next: [`memmem`](#memmem), Previous: [`memcmp`](#memcmp), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.4 `memcpy` [¶](#memcpy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/memcpy.html>

Gnulib module: memcpy

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

---

Next: [`memmove`](#memmove), Previous: [`memcpy`](#memcpy), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.5 `memmem` [¶](#memmem-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/memmem.html>

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Search-Functions.html>,
* [man memmem](https://www.kernel.org/doc/man-pages/online/pages/man3/memmem.3.html).

Gnulib module: memmem or memmem-simple

Both modules implement the same replacement for the `memmem` function
with the `memmem` module providing a replacement on more platforms where
the existing `memmem` function has a quadratic worst-case complexity.

Portability problems fixed by either Gnulib module `memmem-simple`
or `memmem`:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 5.2.1, OpenBSD 4.0, Minix 3.1.8, HP-UX 11, Solaris 10, mingw, MSVC 14.
* This function has reversed arguments on some older platforms:
  Linux libc 5.0.9
* This function can trigger false positives for long periodic needles on
  some platforms:
  glibc 2.12, Cygwin 1.7.7.
* This function returns incorrect values in some cases, such as when
  given an empty needle:
  glibc <= 2.0, macOS 14, AIX 7.2, Solaris 11.3, Cygwin 1.5.x.

Performance problems fixed by Gnulib module `memmem`:

* This function has quadratic instead of linear worst-case complexity on some
  platforms:
  glibc 2.8, macOS 14, FreeBSD 11.4, NetBSD 8.2, OpenBSD 6.6, AIX 7.2, Solaris 11.4, Cygwin 1.5.x.
  Note for small needles the replacement may be slower.

Portability problems not fixed by Gnulib:

---

Next: [`memset`](#memset), Previous: [`memmem`](#memmem), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.6 `memmove` [¶](#memmove-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/memmove.html>

Gnulib module: memmove

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

---

Next: [`memset_explicit`](#memset_005fexplicit), Previous: [`memmove`](#memmove), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.7 `memset` [¶](#memset-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/memset.html>

Gnulib module: memset

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

---

Next: [`stpcpy`](#stpcpy), Previous: [`memset`](#memset), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.8 `memset_explicit` [¶](#memset_005fexplicit-1)

Documentation:

* <https://www.gnu.org/software/libc/manual/html_node/Erasing-Sensitive-Data.html>,

Gnulib module: memset\_explicit

The `memset_explicit` function is an approximation to what is
needed, and does not suffice in general to erase information.
Although calling `memset_explicit` should clear the memory in
question, the information that was in memory may still be available
elsewhere on the machine. Proper implementation of information
erasure requires support from levels below C code.

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.36, FreeBSD 13.1, NetBSD 10.0, OpenBSD 7.2, macOS 13, Solaris 11.4, Android API level 33,
  and many other systems.

Portability problems not fixed by Gnulib:

* Although the module’s implementation should set the memory on
  platforms compatible with GCC and on platforms using traditional
  linkers, it may not set the memory on non-GCC platforms that use
  whole-program optimization.

---

Next: [`stpncpy`](#stpncpy), Previous: [`memset_explicit`](#memset_005fexplicit), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.9 `stpcpy` [¶](#stpcpy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/stpcpy.html>

Gnulib module: stpcpy

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 4.4.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

---

Next: [`strcat`](#strcat), Previous: [`stpcpy`](#stpcpy), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.10 `stpncpy` [¶](#stpncpy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/stpncpy.html>

Gnulib module: stpncpy

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 4.4.
* This function has an incompatible return value on some platforms:
  AIX 5.1.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.
* This function looks at more bytes of the source string than allowed
  on some platforms:
  FreeBSD 15.0/x86\_64.

Portability problems not fixed by Gnulib:

---

Next: [`strchr`](#strchr), Previous: [`stpncpy`](#stpncpy), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.11 `strcat` [¶](#strcat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcat.html>

Gnulib module: string-h

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

---

Next: [`strcmp`](#strcmp), Previous: [`strcat`](#strcat), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.12 `strchr` [¶](#strchr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strchr.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function cannot work correctly on character strings in some multibyte
  locales.
  Gnulib provides an alternative function `mbschr` that works on
  character strings in all locales.

---

Next: [`strcoll`](#strcoll), Previous: [`strchr`](#strchr), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.13 `strcmp` [¶](#strcmp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcmp.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`strcoll_l`](#strcoll_005fl), Previous: [`strcmp`](#strcmp), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.14 `strcoll` [¶](#strcoll-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcoll.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function fails, setting `errno` to `EILSEQ`, on Solaris 10, in
  UTF-8 locales, when at least one of the strings contains a Unicode character
  in a block that was not assigned in Unicode 4.0.

---

Next: [`strcpy`](#strcpy), Previous: [`strcoll`](#strcoll), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.15 `strcoll_l` [¶](#strcoll_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcoll_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.

---

Next: [`strcspn`](#strcspn), Previous: [`strcoll_l`](#strcoll_005fl), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.16 `strcpy` [¶](#strcpy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcpy.html>

Gnulib module: string-h

Portability problems fixed by Gnulib:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

Note: `strcpy (dst, src)` is only safe to use when you can guarantee that
there are at least `strlen (src) + 1` bytes allocated at `dst`.

---

Next: [`strdup`](#strdup), Previous: [`strcpy`](#strcpy), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.17 `strcspn` [¶](#strcspn-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcspn.html>

Gnulib module: strcspn

Portability problems fixed by Gnulib:

* This function is missing on some old platforms.

Portability problems not fixed by Gnulib:

* This function cannot work correctly on character strings in multibyte locales.
  Gnulib provides an alternative function `mbscspn` that works on character
  strings in all locales.

---

Next: [`strerror`](#strerror), Previous: [`strcspn`](#strcspn), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.18 `strdup` [¶](#strdup-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strdup.html>

Gnulib module: strdup or strdup-posix

Portability problems fixed by either Gnulib module `strdup` or `strdup-posix`:

* This function has no prototype in `<string.h>` on some old platforms.

Portability problems fixed by Gnulib module `strdup-posix`:

* On some platforms, when the function fails it does not set `errno`:
  mingw, MSVC 14.
* On some platforms, when the function fails it might set `errno`
  to `EAGAIN` instead of to `ENOMEM`. Although POSIX allows
  `EAGAIN`, the `strdup-posix` module insists on `ENOMEM`
  which also conforms to POSIX and is GNU-compatible:
  Solaris 11.4.

Portability problems not fixed by Gnulib:

---

Next: [`strerror_l`](#strerror_005fl), Previous: [`strdup`](#strdup), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.19 `strerror` [¶](#strerror-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strerror.html>

Gnulib module: strerror

Portability problems fixed by Gnulib:

* This function does not support the error values that are specified by POSIX
  but not defined by the system, on some platforms:
  OpenBSD 4.0, NonStop Kernel, Cygwin 1.5.x, mingw, MSVC 14.
* This function reports failure for `strerror(0)` (by setting
  `errno` or using a string similar to out-of-range values),
  although POSIX requires this to leave `errno` unchanged and
  report success, on some platforms:
  FreeBSD 8.2, NetBSD 10.0, OpenBSD 4.7, macOS 14.
* This function returns an empty string for out-of-range integers on
  some platforms:
  HP-UX 11.

Portability problems not fixed by Gnulib:

Note: The return type of the `strerror` function
is `char *` in POSIX, with the constraint that
“The application shall not modify the string returned.”
The Gnulib-overridden `strerror` function
has a return type of `const char *`.
This helps detecting code that attempts to write into that string.

---

Next: [`strerror_r`](#strerror_005fr), Previous: [`strerror`](#strerror), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.20 `strerror_l` [¶](#strerror_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strerror_l.html>

Gnulib module: strerror\_l

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.5, macOS 14, FreeBSD 12.4, NetBSD 6.1, OpenBSD 6.1, Minix 3.1.8, AIX 7.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 5.1.

Portability problems not fixed by Gnulib:

---

Next: [`strlcat`](#strlcat), Previous: [`strerror_l`](#strerror_005fl), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.21 `strerror_r` [¶](#strerror_005fr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strerror_r.html>

LSB specification:

* <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-strerror-r.html>
* <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib---xpg-strerror-r.html>

Gnulib module: strerror\_r-posix

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, Minix 3.1.8, HP-UX 11.23, Solaris 9, mingw, MSVC 14.
* glibc, Cygwin, Android, and AIX+`_LINUX_SOURCE_COMPAT`
  have an incompatible version of this function.
  The POSIX compliant code

  ```
  char *s = (strerror_r (err, buf, buflen) == 0 ? buf : NULL);
  ```

  is essentially equivalent to this code using the glibc function:

  ```
  char *s = strerror_r (err, buf, buflen);
  ```
* This function clobbers the `strerror` buffer on some platforms:
  Cygwin 1.7.9.
* This function is sometimes not declared in `<string.h>` on some platforms:
  glibc 2.8.
* The third argument is of type `int` instead of `size_t` on some
  platforms:
  AIX 5.1.
* When this function fails, it returns −1 and sets `errno`, instead of
  returning the error number, on some platforms:
  glibc 2.12 with `-D_POSIX_C_SOURCE=200112L`, AIX 6.1.
* When this function fails, it corrupts `errno`, on some platforms:
  Solaris 10.
* This function does not support many error values defined in `<errno.h>` on
  some platforms:
  MSVC 14.
* This function does not support the error values that are specified by POSIX
  but not defined by the system, on some platforms:
  OpenBSD 4.0, NonStop Kernel, Cygwin 1.5.x.
* This function reports failure for `strerror_r(0, buf, len)`,
  although POSIX requires this to succeed, on some platforms:
  FreeBSD 8.2.
* This function produces a different string for `0` than
  `strerror` on some platforms:
  macOS 14.
* This function always fails when the third argument is less than 80 on some
  platforms:
  HP-UX 11.31.
* When the buffer is too small and the value is in range, this function
  does not fail, but instead truncates the result and returns 0 on some
  platforms:
  AIX 6.1, Haiku 2017.
* When the value is not in range or the buffer is too small, this
  function fails to leave a NUL-terminated string in the buffer on some
  platforms:
  glibc 2.13, FreeBSD 8.2, Solaris 10.
* When the value is out of range but the buffer is too small, this
  function does not always return the longest possible string on some
  platforms:
  OpenBSD 4.7.

Portability problems not fixed by Gnulib:

Note: Gnulib has a module `xstrerror`, with the property that
`xstrerror (NULL, errnum)` returns the value of `strerror_r`
as a freshly allocated string.
(Recall that the expression `strerror (errnum)` is not thread-safe.)

---

Next: [`strlcpy`](#strlcpy), Previous: [`strerror_r`](#strerror_005fr), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.22 `strlcat` [¶](#strlcat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strlcat.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Truncating-Strings.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.37, AIX 7.3.1, HP-UX 11.31, mingw, MSVC 14.

---

Next: [`strlen`](#strlen), Previous: [`strlcat`](#strlcat), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.23 `strlcpy` [¶](#strlcpy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strlcpy.html>

Documentation:  
<https://www.gnu.org/software/libc/manual/html_node/Truncating-Strings.html>.

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.37, AIX 7.3.1, HP-UX 11.31, mingw, MSVC 14.

---

Next: [`strncat`](#strncat), Previous: [`strlcpy`](#strlcpy), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.24 `strlen` [¶](#strlen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strlen.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`strncmp`](#strncmp), Previous: [`strlen`](#strlen), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.25 `strncat` [¶](#strncat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strncat.html>

Gnulib module: strncat

Portability problems fixed by Gnulib:

* This function dereferences too much memory on some platforms:
  Solaris 11.4 on SPARC, Solaris 11.0 on x86.
* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems not fixed by Gnulib:

---

Next: [`strncpy`](#strncpy), Previous: [`strncat`](#strncat), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.26 `strncmp` [¶](#strncmp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strncmp.html>

Gnulib module: strncmp

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`strndup`](#strndup), Previous: [`strncmp`](#strncmp), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.27 `strncpy` [¶](#strncpy-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strncpy.html>

Gnulib module: string-h or strncpy

Portability problems fixed by Gnulib module `string-h` or Gnulib module `strncpy`:

* This function cannot be called from plain inline or extern inline functions
  on some platforms:
  macOS 10.8.

Portability problems fixed by Gnulib module `strncpy`:

* This function looks at more bytes of the source string than allowed
  on some platforms:
  FreeBSD 15.0/x86\_64.

Portability problems not fixed by Gnulib:

Note: This function was designed for the use-case of filling a fixed-size
record with a string, before writing it to a file. This function is
**not** appropriate for copying a string into a bounded memory area,
because you have no guarantee that the result will be NUL-terminated.
Even if you add the NUL byte at the end yourself, this function is
inefficient (as it spends time clearing unused memory) and will allow
silent truncation to occur, which is not a good behavior for GNU programs.
For more details, see <https://meyering.net/crusade-to-eliminate-strncpy/>.

---

Next: [`strnlen`](#strnlen), Previous: [`strncpy`](#strncpy), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.28 `strndup` [¶](#strndup-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strndup.html>

Gnulib module: strndup

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, HP-UX 11, Solaris 10, mingw, MSVC 14.
* This function does not NUL-terminate the result on some platforms:
  AIX 5.1.
* This function does not support zero-length operations on NULL pointers
  on some platforms:
  AIX 7.3.

Portability problems not fixed by Gnulib:

---

Next: [`strpbrk`](#strpbrk), Previous: [`strndup`](#strndup), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.29 `strnlen` [¶](#strnlen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strnlen.html>

Gnulib module: strnlen

Portability problems fixed by Gnulib:

* On some platforms, calls like `strnlen (s, maxlen)` can crash if
  s is null-terminated but address arithmetic overflows
  (i.e., `s + maxlen < s`):
  Android 5.0.
* This function is missing on some platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 5.0, OpenBSD 3.8, HP-UX 11, Solaris 10, mingw.

Portability problems not fixed by Gnulib:

---

Next: [`strrchr`](#strrchr), Previous: [`strnlen`](#strnlen), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.30 `strpbrk` [¶](#strpbrk-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strpbrk.html>

Gnulib module: strpbrk

Portability problems fixed by Gnulib:

* This function is missing on some old platforms.

Portability problems not fixed by Gnulib:

* This function cannot work correctly on character strings in multibyte locales.
  Gnulib provides an alternative function `mbspbrk` that works on character
  strings in all locales.

---

Next: [`strsignal`](#strsignal), Previous: [`strpbrk`](#strpbrk), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.31 `strrchr` [¶](#strrchr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strrchr.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function cannot work correctly on character strings in some multibyte
  locales.
  Gnulib provides an alternative function `mbsrchr` that works
  on character strings in all locales.

---

Next: [`strspn`](#strspn), Previous: [`strrchr`](#strrchr), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.32 `strsignal` [¶](#strsignal-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strsignal.html>

Gnulib module: strsignal

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, HP-UX 11, mingw, MSVC 14.
* This function does not return a string for out-of-range numbers on
  some platforms:
  Solaris, AIX 5.1.
* This function is declared in `unistd.h` instead of
  `string.h` on some platforms:
  NetBSD 5.0.

Portability problems not fixed by Gnulib:

* This function returns `const char *` instead of `char *` on
  some platforms:
  cygwin 1.5.25, Haiku.

---

Next: [`strstr`](#strstr), Previous: [`strsignal`](#strsignal), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.33 `strspn` [¶](#strspn-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strspn.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function cannot work correctly on character strings in multibyte locales.
  Gnulib provides an alternative function `mbsspn` that works on character
  strings in all locales.

---

Next: [`strtok`](#strtok), Previous: [`strspn`](#strspn), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.34 `strstr` [¶](#strstr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strstr.html>

Gnulib module: strstr or strstr-simple

Portability problems fixed by either Gnulib module `strstr-simple`
or `strstr`:

* This function can trigger memchr bugs on some platforms:
  glibc 2.10.
* This function can trigger false positives for long periodic needles on
  some platforms:
  glibc 2.12, Cygwin 1.7.7.
* This function may fail to find matches on some platforms:
  glibc 2.28.

Portability problems fixed by Gnulib module `strstr`:

* This function has quadratic instead of linear worst-case complexity on some
  platforms:
  glibc 2.8, macOS 14, FreeBSD 11.4, NetBSD 10.0, OpenBSD 6.1, AIX 7.2, HP-UX 11, Solaris 11.4, Cygwin 1.5.x, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* This function cannot work correctly on character strings in most multibyte
  locales.
  Gnulib provides an alternative function `mbsstr` that works
  on character strings in all locales.

---

Next: [`strtok_r`](#strtok_005fr), Previous: [`strstr`](#strstr), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.35 `strtok` [¶](#strtok-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtok.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`strxfrm`](#strxfrm), Previous: [`strtok`](#strtok), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.36 `strtok_r` [¶](#strtok_005fr-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtok_r.html>

Gnulib module: strtok\_r

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* This function crashes when invoked from code compiled with optimization enabled
  on some platforms:
  glibc 2.7.

Portability problems not fixed by Gnulib:

* This function cannot work correctly on character strings in multibyte locales.
  Gnulib provides an alternative function `mbstok_r` that works on character
  strings in all locales.

---

Next: [`strxfrm_l`](#strxfrm_005fl), Previous: [`strtok_r`](#strtok_005fr), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.37 `strxfrm` [¶](#strxfrm-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strxfrm.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function has a wrong return value on some platforms:
  Haiku.

---

Previous: [`strxfrm`](#strxfrm), Up: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.47.38 `strxfrm_l` [¶](#strxfrm_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strxfrm_l.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 6.0, NetBSD 5.0, OpenBSD 6.0, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.3, Cygwin 1.7.x, mingw, MSVC 14, Android 4.4.

---

Next: [Functions in `<sys/ipc.h>`](#Functions-in-_003csys_002fipc_002eh_003e), Previous: [Functions in `<string.h>`](#Functions-in-_003cstring_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.48 Functions in `<strings.h>` [¶](#Functions-in-_003cstrings_002eh_003e-1)

* [`ffs`](#ffs)
* [`ffsl`](#ffsl)
* [`ffsll`](#ffsll)
* [`strcasecmp`](#strcasecmp)
* [`strcasecmp_l`](#strcasecmp_005fl)
* [`strncasecmp`](#strncasecmp)
* [`strncasecmp_l`](#strncasecmp_005fl)

---

Next: [`ffsl`](#ffsl), Up: [Functions in `<strings.h>`](#Functions-in-_003cstrings_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.48.1 `ffs` [¶](#ffs-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ffs.html>

Gnulib module: ffs

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* This function is only defined as an inline function on some platforms:
  Android 13.

---

Next: [`ffsll`](#ffsll), Previous: [`ffs`](#ffs), Up: [Functions in `<strings.h>`](#Functions-in-_003cstrings_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.48.2 `ffsl` [¶](#ffsl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ffsl.html>

Documentation:  
 [man ffsl](https://www.kernel.org/doc/man-pages/online/pages/man3/ffsl.3.html)

Gnulib module: ffsl

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  FreeBSD 5.2.1, NetBSD 10.0, OpenBSD 7.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.
* This function is declared in `<strings.h>` instead of `<string.h>`
  on some platforms:
  AIX 7.2, Android 13.

Portability problems not fixed by Gnulib:

* This function is only defined as an inline function on some platforms:
  Android 13.

---

Next: [`strcasecmp`](#strcasecmp), Previous: [`ffsl`](#ffsl), Up: [Functions in `<strings.h>`](#Functions-in-_003cstrings_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.48.3 `ffsll` [¶](#ffsll-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ffsll.html>

Documentation:  
 [man ffsll](https://www.kernel.org/doc/man-pages/online/pages/man3/ffsll.3.html)

Gnulib module: ffsll

Portability problems fixed by Gnulib:

* This function is missing on many non-glibc platforms:
  Mac OS X 10.5, FreeBSD 6.0, NetBSD 10.0, OpenBSD 7.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.7.x, mingw, MSVC 14, Android 9.0.
* This function is declared in `<strings.h>` instead of `<string.h>`
  on some platforms:
  AIX 7.2, Android 13.
* This function returns completely wrong values on some platforms:
  AIX 7.2 in 32-bit mode.

Portability problems not fixed by Gnulib:

* This function is only defined as an inline function on some platforms:
  Android 13.

---

Next: [`strcasecmp_l`](#strcasecmp_005fl), Previous: [`ffsll`](#ffsll), Up: [Functions in `<strings.h>`](#Functions-in-_003cstrings_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.48.4 `strcasecmp` [¶](#strcasecmp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcasecmp.html>

Gnulib module: strcasecmp

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 14.
* This function uses the case mappings of a wrong locale on some platforms:
  Solaris 11.4,
  Cygwin 3.5.6.

Portability problems not fixed by Gnulib:

* This function cannot work correctly on character strings in multibyte locales.
  Gnulib provides an alternative function `mbscasecmp` that does a case
  insensitive comparison of character strings and that works in all locales.

---

Next: [`strncasecmp`](#strncasecmp), Previous: [`strcasecmp`](#strcasecmp), Up: [Functions in `<strings.h>`](#Functions-in-_003cstrings_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.48.5 `strcasecmp_l` [¶](#strcasecmp_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcasecmp_l.html>

Gnulib module: strcasecmp\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 10.0, OpenBSD 6.1, Minix 3.3.0, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 5.1.
* This function is declared in `<string.h>` instead of `<strings.h>`
  on some platforms:
  glibc 2.9, macOS 15.
* This function uses the case mappings of a wrong locale on some platforms:
  Solaris 11.4, Cygwin 3.5.6.

Portability problems not fixed by Gnulib:

---

Next: [`strncasecmp_l`](#strncasecmp_005fl), Previous: [`strcasecmp_l`](#strcasecmp_005fl), Up: [Functions in `<strings.h>`](#Functions-in-_003cstrings_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.48.6 `strncasecmp` [¶](#strncasecmp-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strncasecmp.html>

Gnulib module: strncasecmp

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  MSVC 14.
* This function uses the case mappings of a wrong locale on some platforms:
  Solaris 11.4,
  Cygwin 3.5.6.

Portability problems not fixed by Gnulib:

* This function cannot work correctly on character strings in multibyte locales.

  Gnulib provides alternative functions `mbsncasecmp` and `mbspcasecmp`
  that do a case insensitive comparison of character strings and that work in all
  locales.

---

Previous: [`strncasecmp`](#strncasecmp), Up: [Functions in `<strings.h>`](#Functions-in-_003cstrings_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.48.7 `strncasecmp_l` [¶](#strncasecmp_005fl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/strncasecmp_l.html>

Gnulib module: strncasecmp\_l

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  FreeBSD 9.0, NetBSD 10.0, OpenBSD 6.1, Minix 3.3.0, AIX 6.1, HP-UX 11, Solaris 11.3, Cygwin 2.5.x, mingw, MSVC 14, Android 5.1.
* This function is declared in `<string.h>` instead of `<strings.h>`
  on some platforms:
  glibc 2.9, macOS 15.
* This function uses the case mappings of a wrong locale on some platforms:
  Solaris 11.4, Cygwin 3.5.6.

Portability problems not fixed by Gnulib:

---

Next: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e), Previous: [Functions in `<strings.h>`](#Functions-in-_003cstrings_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.49 Functions in `<sys/ipc.h>` [¶](#Functions-in-_003csys_002fipc_002eh_003e-1)

* [`ftok`](#ftok)

---

Up: [Functions in `<sys/ipc.h>`](#Functions-in-_003csys_002fipc_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.49.1 `ftok` [¶](#ftok-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/ftok.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [Functions in `<sys/msg.h>`](#Functions-in-_003csys_002fmsg_002eh_003e), Previous: [Functions in `<sys/ipc.h>`](#Functions-in-_003csys_002fipc_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.50 Functions in `<sys/mman.h>` [¶](#Functions-in-_003csys_002fmman_002eh_003e-1)

* [`mlock`](#mlock)
* [`mlockall`](#mlockall)
* [`mmap`](#mmap)
* [`mprotect`](#mprotect)
* [`msync`](#msync)
* [`munlock`](#munlock)
* [`munlockall`](#munlockall)
* [`munmap`](#munmap)
* [`posix_madvise`](#posix_005fmadvise)
* [`posix_mem_offset`](#posix_005fmem_005foffset)
* [`posix_typed_mem_get_info`](#posix_005ftyped_005fmem_005fget_005finfo)
* [`posix_typed_mem_open`](#posix_005ftyped_005fmem_005fopen)
* [`shm_open`](#shm_005fopen)
* [`shm_unlink`](#shm_005funlink)

---

Next: [`mlockall`](#mlockall), Up: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.50.1 `mlock` [¶](#mlock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mlock.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* On Solaris 10 this function accepts `char *` not `void const *`,
  unless you define feature test macros like `_POSIX_C_SOURCE`
  or `_XOPEN_SOURCE` that may undesirably hide other symbols.
  To work around this problem, callers can limit themselves to either
  `void const *` or `char const *`.

---

Next: [`mmap`](#mmap), Previous: [`mlock`](#mlock), Up: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.50.2 `mlockall` [¶](#mlockall-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mlockall.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 4.1.

---

Next: [`mprotect`](#mprotect), Previous: [`mlockall`](#mlockall), Up: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.50.3 `mmap` [¶](#mmap-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mmap.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `off_t` is a 32-bit type, this function may not
  work correctly across the entire data range of files 2 GiB and larger.
  See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* To get anonymous memory, on some platforms, you can use the flags
  `MAP_ANONYMOUS | MAP_PRIVATE` and `-1` instead of a file descriptor;
  on others you have to use a read-only file descriptor of `/dev/zero`.
* On Solaris 10 this function accepts and returns `char *` not `void *`,
  unless you define feature test macros like `_POSIX_C_SOURCE`
  or `_XOPEN_SOURCE` that may undesirably hide other symbols.
  To work around this problem, callers can limit themselves to either
  `void *` or `char *` for both arguments and results.
* On HP-UX, passing a non-NULL first argument, as a hint for the address (even
  without `MAP_FIXED`), often causes `mmap` to fail. Better pass NULL
  in this case.
* On HP-UX, `MAP_FIXED` basically never works. On other platforms, it depends
  on the circumstances whether memory can be returned at a given address.

---

Next: [`msync`](#msync), Previous: [`mmap`](#mmap), Up: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.50.4 `mprotect` [¶](#mprotect-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/mprotect.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* This function does not set `errno` on some platforms:
  mingw.
* On AIX, it is not possible to use `mprotect` on memory regions allocated
  with `malloc`.
* On Solaris 10 this function accepts `char *` not `void *`,
  unless you define feature test macros like `_POSIX_C_SOURCE`
  or `_XOPEN_SOURCE` that may undesirably hide other symbols.
  To work around this problem, callers can limit themselves to either
  `void *` or `char *`.

---

Next: [`munlock`](#munlock), Previous: [`mprotect`](#mprotect), Up: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.50.5 `msync` [¶](#msync-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/msync.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* On NetBSD, `msync` takes only two arguments.
* On Solaris 10 this function accepts `char *` not `void *`,
  unless you define feature test macros like `_POSIX_C_SOURCE`
  or `_XOPEN_SOURCE` that may undesirably hide other symbols.
  To work around this problem, callers can limit themselves to either
  `void *` or `char *`.

---

Next: [`munlockall`](#munlockall), Previous: [`msync`](#msync), Up: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.50.6 `munlock` [¶](#munlock-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/munlock.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.
* On Solaris 10 this function accepts `char *` not `void const *`,
  unless you define feature test macros like `_POSIX_C_SOURCE`
  or `_XOPEN_SOURCE` that may undesirably hide other symbols.
  To work around this problem, callers can limit themselves to either
  `void const *` or `char const *`.

---

Next: [`munmap`](#munmap), Previous: [`munlock`](#munlock), Up: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.50.7 `munlockall` [¶](#munlockall-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/munlockall.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, Cygwin 2.9, mingw, MSVC 14, Android 4.1.

---

Next: [`posix_madvise`](#posix_005fmadvise), Previous: [`munlockall`](#munlockall), Up: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.50.8 `munmap` [¶](#munmap-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/munmap.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.
* On Solaris 10 this function accepts `char *` not `void *`,
  unless you define feature test macros like `_POSIX_C_SOURCE`
  or `_XOPEN_SOURCE` that may undesirably hide other symbols.
  To work around this problem, callers can limit themselves to either
  `void *` or `char *`.

---

Next: [`posix_mem_offset`](#posix_005fmem_005foffset), Previous: [`munmap`](#munmap), Up: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.50.9 `posix_madvise` [¶](#posix_005fmadvise-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_madvise.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 3.0, OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 10, Cygwin 1.5.x, mingw, MSVC 14, Android 5.1.

---

Next: [`posix_typed_mem_get_info`](#posix_005ftyped_005fmem_005fget_005finfo), Previous: [`posix_madvise`](#posix_005fmadvise), Up: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.50.10 `posix_mem_offset` [¶](#posix_005fmem_005foffset-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_mem_offset.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`posix_typed_mem_open`](#posix_005ftyped_005fmem_005fopen), Previous: [`posix_mem_offset`](#posix_005fmem_005foffset), Up: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.50.11 `posix_typed_mem_get_info` [¶](#posix_005ftyped_005fmem_005fget_005finfo-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_typed_mem_get_info.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`shm_open`](#shm_005fopen), Previous: [`posix_typed_mem_get_info`](#posix_005ftyped_005fmem_005fget_005finfo), Up: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.50.12 `posix_typed_mem_open` [¶](#posix_005ftyped_005fmem_005fopen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_typed_mem_open.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  glibc 2.3.6, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, Minix 3.1.8, AIX 5.1, HP-UX 11, Solaris 11.4, Cygwin 2.9, mingw, MSVC 14, Android 9.0.

---

Next: [`shm_unlink`](#shm_005funlink), Previous: [`posix_typed_mem_open`](#posix_005ftyped_005fmem_005fopen), Up: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.50.13 `shm_open` [¶](#shm_005fopen-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/shm_open.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Previous: [`shm_open`](#shm_005fopen), Up: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.50.14 `shm_unlink` [¶](#shm_005funlink-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/shm_unlink.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  NetBSD 5.0, OpenBSD 3.8, Minix 3.1.8, Cygwin 1.5.x, mingw, MSVC 14, Android 9.0.

---

Next: [Functions in `<sys/resource.h>`](#Functions-in-_003csys_002fresource_002eh_003e), Previous: [Functions in `<sys/mman.h>`](#Functions-in-_003csys_002fmman_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.51 Functions in `<sys/msg.h>` [¶](#Functions-in-_003csys_002fmsg_002eh_003e-1)

* [`msgctl`](#msgctl)
* [`msgget`](#msgget)
* [`msgrcv`](#msgrcv)
* [`msgsnd`](#msgsnd)

---

Next: [`msgget`](#msgget), Up: [Functions in `<sys/msg.h>`](#Functions-in-_003csys_002fmsg_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.51.1 `msgctl` [¶](#msgctl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/msgctl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 7.1.

---

Next: [`msgrcv`](#msgrcv), Previous: [`msgctl`](#msgctl), Up: [Functions in `<sys/msg.h>`](#Functions-in-_003csys_002fmsg_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.51.2 `msgget` [¶](#msgget-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/msgget.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 7.1.

---

Next: [`msgsnd`](#msgsnd), Previous: [`msgget`](#msgget), Up: [Functions in `<sys/msg.h>`](#Functions-in-_003csys_002fmsg_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.51.3 `msgrcv` [¶](#msgrcv-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/msgrcv.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 7.1.

---

Previous: [`msgrcv`](#msgrcv), Up: [Functions in `<sys/msg.h>`](#Functions-in-_003csys_002fmsg_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.51.4 `msgsnd` [¶](#msgsnd-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/msgsnd.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14, Android 7.1.

---

Next: [Functions in `<sys/select.h>`](#Functions-in-_003csys_002fselect_002eh_003e), Previous: [Functions in `<sys/msg.h>`](#Functions-in-_003csys_002fmsg_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.52 Functions in `<sys/resource.h>` [¶](#Functions-in-_003csys_002fresource_002eh_003e-1)

* [`getpriority`](#getpriority)
* [`getrlimit`](#getrlimit)
* [`getrusage`](#getrusage)
* [`setpriority`](#setpriority)
* [`setrlimit`](#setrlimit)

---

Next: [`getrlimit`](#getrlimit), Up: [Functions in `<sys/resource.h>`](#Functions-in-_003csys_002fresource_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.52.1 `getpriority` [¶](#getpriority-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getpriority.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`getrusage`](#getrusage), Previous: [`getpriority`](#getpriority), Up: [Functions in `<sys/resource.h>`](#Functions-in-_003csys_002fresource_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.52.2 `getrlimit` [¶](#getrlimit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getrlimit.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getrlimit-1.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `rlim_t` is a 32-bit type, this function does not
  allow to retrieve limits larger than 4 GiB and larger,
  such as for RLIMIT\_FSIZE. See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Next: [`setpriority`](#setpriority), Previous: [`getrlimit`](#getrlimit), Up: [Functions in `<sys/resource.h>`](#Functions-in-_003csys_002fresource_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.52.3 `getrusage` [¶](#getrusage-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/getrusage.html>

Gnulib module: getrusage

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

Portability problems not fixed by Gnulib:

* Many platforms don’t fill in all the fields of `struct rusage` with
  meaningful values.
* Some platforms fill the `ru_maxrss` field of `struct rusage`
  with the maximum resident set size in bytes instead of kilobytes:
  macOS 26.

---

Next: [`setrlimit`](#setrlimit), Previous: [`getrusage`](#getrusage), Up: [Functions in `<sys/resource.h>`](#Functions-in-_003csys_002fresource_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.52.4 `setpriority` [¶](#setpriority-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setpriority.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14.

---

Previous: [`setpriority`](#setpriority), Up: [Functions in `<sys/resource.h>`](#Functions-in-_003csys_002fresource_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.52.5 `setrlimit` [¶](#setrlimit-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/setrlimit.html>

LSB specification:  
 <https://refspecs.linuxbase.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/baselib-getrlimit-1.html>

Gnulib module: —

Portability problems fixed by Gnulib:

* On platforms where `rlim_t` is a 32-bit type, this function does not
  allow to set limits 4 GiB and larger, such as for RLIMIT\_FSIZE.
  See [Large File Support](#Large-File-Support).

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  Minix 3.1.8, mingw, MSVC 14.

---

Next: [Functions in `<sys/sem.h>`](#Functions-in-_003csys_002fsem_002eh_003e), Previous: [Functions in `<sys/resource.h>`](#Functions-in-_003csys_002fresource_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.53 Functions in `<sys/select.h>` [¶](#Functions-in-_003csys_002fselect_002eh_003e-1)

* [`FD_CLR`](#FD_005fCLR)
* [`FD_ISSET`](#FD_005fISSET)
* [`FD_SET`](#FD_005fSET)
* [`FD_ZERO`](#FD_005fZERO)
* [`pselect`](#pselect)
* [`select`](#select)

---

Next: [`FD_ISSET`](#FD_005fISSET), Up: [Functions in `<sys/select.h>`](#Functions-in-_003csys_002fselect_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.53.1 `FD_CLR` [¶](#FD_005fCLR-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/FD_CLR.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`FD_SET`](#FD_005fSET), Previous: [`FD_CLR`](#FD_005fCLR), Up: [Functions in `<sys/select.h>`](#Functions-in-_003csys_002fselect_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.53.2 `FD_ISSET` [¶](#FD_005fISSET-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/FD_ISSET.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`FD_ZERO`](#FD_005fZERO), Previous: [`FD_ISSET`](#FD_005fISSET), Up: [Functions in `<sys/select.h>`](#Functions-in-_003csys_002fselect_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.53.3 `FD_SET` [¶](#FD_005fSET-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/FD_SET.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`pselect`](#pselect), Previous: [`FD_SET`](#FD_005fSET), Up: [Functions in `<sys/select.h>`](#Functions-in-_003csys_002fselect_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.53.4 `FD_ZERO` [¶](#FD_005fZERO-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/FD_ZERO.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

---

Next: [`select`](#select), Previous: [`FD_ZERO`](#FD_005fZERO), Up: [Functions in `<sys/select.h>`](#Functions-in-_003csys_002fselect_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.53.5 `pselect` [¶](#pselect-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/pselect.html>

Gnulib module: pselect

Portability problems fixed by Gnulib:

* This function is missing on some platforms:
  OpenBSD 3.8, Minix 3.1.8, AIX 5.1, HP-UX 11.23, Solaris 9, mingw, MSVC 14.
* On some platforms, this function fails to detect invalid fds with
  EBADF, but only if they lie beyond the current maximum open fd:
  FreeBSD 8.2.

Portability problems not fixed by Gnulib:

* This function has a slightly incompatible declaration on some platforms:
  AIX 7.1 (when `_ALL_SOURCE` is defined).
* When the sigmask argument is nonnull on platforms that do not
  natively support this function, race conditions are possible when its
  gnulib implementation temporarily modifies the signal mask, and the
  behavior is unspecified in a multi-threaded process.

---

Previous: [`pselect`](#pselect), Up: [Functions in `<sys/select.h>`](#Functions-in-_003csys_002fselect_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.53.6 `select` [¶](#select-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/select.html>

Gnulib module: select

Portability problems fixed by Gnulib:

* This function is declared in `<winsock2.h>`
  instead of `<sys/select.h>`
  on some platforms:
  mingw, MSVC 14.
* On Windows platforms (excluding Cygwin), `select` can only be
  called on descriptors created by the `socket` function, not on regular
  file descriptors.
* On Windows platforms (excluding Cygwin), error codes from this function
  are not placed in `errno`, and `WSAGetLastError` must be used
  instead.
* On some platforms, this function fails to detect invalid fds with
  EBADF, but only if they lie beyond the current maximum open fd:
  FreeBSD 8.2.

Portability problems not fixed by Gnulib:

* When you call `select` with a timeout, some implementations modify the
  timeout parameter so that upon return from the function, it contains the
  amount of time not slept. Other implementations leave the timeout parameter
  unmodified.
* Under Windows, when passing a pipe, Gnulib’s `select` replacement might
  return 0 even before the timeout has passed. Programs using it with pipes can
  thus busy wait.
* On Linux, when some file descriptor refers to a regular file, `select`
  may fail, setting `errno` to `EBADF`.
* On Haiku,
  the maximum allowed value for the first argument is not `FD_SETSIZE`
  but `FD_SETSIZE / 2`.

---

Next: [Functions in `<sys/shm.h>`](#Functions-in-_003csys_002fshm_002eh_003e), Previous: [Functions in `<sys/select.h>`](#Functions-in-_003csys_002fselect_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.54 Functions in `<sys/sem.h>` [¶](#Functions-in-_003csys_002fsem_002eh_003e-1)

* [`semctl`](#semctl)
* [`semget`](#semget)
* [`semop`](#semop)

---

Next: [`semget`](#semget), Up: [Functions in `<sys/sem.h>`](#Functions-in-_003csys_002fsem_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.54.1 `semctl` [¶](#semctl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/semctl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 7.1.

---

Next: [`semop`](#semop), Previous: [`semctl`](#semctl), Up: [Functions in `<sys/sem.h>`](#Functions-in-_003csys_002fsem_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.54.2 `semget` [¶](#semget-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/semget.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 7.1.

---

Previous: [`semget`](#semget), Up: [Functions in `<sys/sem.h>`](#Functions-in-_003csys_002fsem_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.54.3 `semop` [¶](#semop-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/semop.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 7.1.

---

Next: [Functions in `<sys/socket.h>`](#Functions-in-_003csys_002fsocket_002eh_003e), Previous: [Functions in `<sys/sem.h>`](#Functions-in-_003csys_002fsem_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 10.55 Functions in `<sys/shm.h>` [¶](#Functions-in-_003csys_002fshm_002eh_003e-1)

* [`shmat`](#shmat)
* [`shmctl`](#shmctl)
* [`shmdt`](#shmdt)
* [`shmget`](#shmget)

---

Next: [`shmctl`](#shmctl), Up: [Functions in `<sys/shm.h>`](#Functions-in-_003csys_002fshm_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.55.1 `shmat` [¶](#shmat-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/shmat.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 7.1.
* On Linux, the flag `SHM_REMAP` is needed in order to force `shmat`
  to replace existing memory mappings in the specify address range. On other
  platforms, it is not needed.

---

Next: [`shmdt`](#shmdt), Previous: [`shmat`](#shmat), Up: [Functions in `<sys/shm.h>`](#Functions-in-_003csys_002fshm_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.55.2 `shmctl` [¶](#shmctl-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/shmctl.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 7.1.

---

Next: [`shmget`](#shmget), Previous: [`shmctl`](#shmctl), Up: [Functions in `<sys/shm.h>`](#Functions-in-_003csys_002fshm_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.55.3 `shmdt` [¶](#shmdt-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/shmdt.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 7.1.
* On Solaris 10 this function accepts `char *` not `void const *`,
  unless you define feature test macros like `_POSIX_C_SOURCE`
  or `_XOPEN_SOURCE` that may undesirably hide other symbols.
  To work around this problem, callers can limit themselves to either
  `void *` or `char *`.

---

Previous: [`shmdt`](#shmdt), Up: [Functions in `<sys/shm.h>`](#Functions-in-_003csys_002fshm_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.55.4 `shmget` [¶](#shmget-1)

POSIX specification:  
 <https://pubs.opengroup.org/onlinepubs/9799919799/functions/shmget.html>

Gnulib module: —

Portability problems fixed by Gnulib:

Portability problems not fixed by Gnulib:

* This function is missing on some platforms:
  mingw, MSVC 14, Android 7.1.
* On many platforms (not Linux), SHMMAX is so small that it is unusable for
  reasonable applications, and/or `shmget` requires superuser privileges.

---

Next: [Functions in `<sys/stat.h>`](#Functions-in-_003csys_002fstat_002eh_003e), Previous: [Functions in `<sys/shm.h>`](#Functions-in-_003csys_002fshm_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
