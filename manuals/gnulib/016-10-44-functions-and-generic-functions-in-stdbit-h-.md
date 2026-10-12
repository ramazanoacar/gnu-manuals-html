### 10.44 Functions and generic functions in `<stdbit.h>` [¶](#Functions-and-generic-functions-in-_003cstdbit_002eh_003e)

* [`stdc_bit_ceil`](#stdc_005fbit_005fceil)
* [`stdc_bit_floor`](#stdc_005fbit_005ffloor)
* [`stdc_bit_width`](#stdc_005fbit_005fwidth)
* [`stdc_count_ones`](#stdc_005fcount_005fones)
* [`stdc_count_zeros`](#stdc_005fcount_005fzeros)
* [`stdc_first_leading_one`](#stdc_005ffirst_005fleading_005fone)
* [`stdc_first_leading_zero`](#stdc_005ffirst_005fleading_005fzero)
* [`stdc_first_trailing_one`](#stdc_005ffirst_005ftrailing_005fone)
* [`stdc_first_trailing_zero`](#stdc_005ffirst_005ftrailing_005fzero)
* [`stdc_has_single_bit`](#stdc_005fhas_005fsingle_005fbit)
* [`stdc_leading_ones`](#stdc_005fleading_005fones)
* [`stdc_leading_zeros`](#stdc_005fleading_005fzeros)
* [`stdc_load8_aligned_bes8`](#stdc_005fload8_005faligned_005fbes8)
* [`stdc_load8_aligned_bes16`](#stdc_005fload8_005faligned_005fbes16)
* [`stdc_load8_aligned_bes32`](#stdc_005fload8_005faligned_005fbes32)
* [`stdc_load8_aligned_bes64`](#stdc_005fload8_005faligned_005fbes64)
* [`stdc_load8_aligned_beu8`](#stdc_005fload8_005faligned_005fbeu8)
* [`stdc_load8_aligned_beu16`](#stdc_005fload8_005faligned_005fbeu16)
* [`stdc_load8_aligned_beu32`](#stdc_005fload8_005faligned_005fbeu32)
* [`stdc_load8_aligned_beu64`](#stdc_005fload8_005faligned_005fbeu64)
* [`stdc_load8_aligned_les8`](#stdc_005fload8_005faligned_005fles8)
* [`stdc_load8_aligned_les16`](#stdc_005fload8_005faligned_005fles16)
* [`stdc_load8_aligned_les32`](#stdc_005fload8_005faligned_005fles32)
* [`stdc_load8_aligned_les64`](#stdc_005fload8_005faligned_005fles64)
* [`stdc_load8_aligned_leu8`](#stdc_005fload8_005faligned_005fleu8)
* [`stdc_load8_aligned_leu16`](#stdc_005fload8_005faligned_005fleu16)
* [`stdc_load8_aligned_leu32`](#stdc_005fload8_005faligned_005fleu32)
* [`stdc_load8_aligned_leu64`](#stdc_005fload8_005faligned_005fleu64)
* [`stdc_load8_bes8`](#stdc_005fload8_005fbes8)
* [`stdc_load8_bes16`](#stdc_005fload8_005fbes16)
* [`stdc_load8_bes32`](#stdc_005fload8_005fbes32)
* [`stdc_load8_bes64`](#stdc_005fload8_005fbes64)
* [`stdc_load8_beu8`](#stdc_005fload8_005fbeu8)
* [`stdc_load8_beu16`](#stdc_005fload8_005fbeu16)
* [`stdc_load8_beu32`](#stdc_005fload8_005fbeu32)
* [`stdc_load8_beu64`](#stdc_005fload8_005fbeu64)
* [`stdc_load8_les8`](#stdc_005fload8_005fles8)
* [`stdc_load8_les16`](#stdc_005fload8_005fles16)
* [`stdc_load8_les32`](#stdc_005fload8_005fles32)
* [`stdc_load8_les64`](#stdc_005fload8_005fles64)
* [`stdc_load8_leu8`](#stdc_005fload8_005fleu8)
* [`stdc_load8_leu16`](#stdc_005fload8_005fleu16)
* [`stdc_load8_leu32`](#stdc_005fload8_005fleu32)
* [`stdc_load8_leu64`](#stdc_005fload8_005fleu64)
* [`stdc_memreverse8`](#stdc_005fmemreverse8)
* [`stdc_memreverse8u8`](#stdc_005fmemreverse8u8)
* [`stdc_memreverse8u16`](#stdc_005fmemreverse8u16)
* [`stdc_memreverse8u32`](#stdc_005fmemreverse8u32)
* [`stdc_memreverse8u64`](#stdc_005fmemreverse8u64)
* [`stdc_rotate_left`](#stdc_005frotate_005fleft)
* [`stdc_rotate_right`](#stdc_005frotate_005fright)
* [`stdc_store8_aligned_bes8`](#stdc_005fstore8_005faligned_005fbes8)
* [`stdc_store8_aligned_bes16`](#stdc_005fstore8_005faligned_005fbes16)
* [`stdc_store8_aligned_bes32`](#stdc_005fstore8_005faligned_005fbes32)
* [`stdc_store8_aligned_bes64`](#stdc_005fstore8_005faligned_005fbes64)
* [`stdc_store8_aligned_beu8`](#stdc_005fstore8_005faligned_005fbeu8)
* [`stdc_store8_aligned_beu16`](#stdc_005fstore8_005faligned_005fbeu16)
* [`stdc_store8_aligned_beu32`](#stdc_005fstore8_005faligned_005fbeu32)
* [`stdc_store8_aligned_beu64`](#stdc_005fstore8_005faligned_005fbeu64)
* [`stdc_store8_aligned_les8`](#stdc_005fstore8_005faligned_005fles8)
* [`stdc_store8_aligned_les16`](#stdc_005fstore8_005faligned_005fles16)
* [`stdc_store8_aligned_les32`](#stdc_005fstore8_005faligned_005fles32)
* [`stdc_store8_aligned_les64`](#stdc_005fstore8_005faligned_005fles64)
* [`stdc_store8_aligned_leu8`](#stdc_005fstore8_005faligned_005fleu8)
* [`stdc_store8_aligned_leu16`](#stdc_005fstore8_005faligned_005fleu16)
* [`stdc_store8_aligned_leu32`](#stdc_005fstore8_005faligned_005fleu32)
* [`stdc_store8_aligned_leu64`](#stdc_005fstore8_005faligned_005fleu64)
* [`stdc_store8_bes8`](#stdc_005fstore8_005fbes8)
* [`stdc_store8_bes16`](#stdc_005fstore8_005fbes16)
* [`stdc_store8_bes32`](#stdc_005fstore8_005fbes32)
* [`stdc_store8_bes64`](#stdc_005fstore8_005fbes64)
* [`stdc_store8_beu8`](#stdc_005fstore8_005fbeu8)
* [`stdc_store8_beu16`](#stdc_005fstore8_005fbeu16)
* [`stdc_store8_beu32`](#stdc_005fstore8_005fbeu32)
* [`stdc_store8_beu64`](#stdc_005fstore8_005fbeu64)
* [`stdc_store8_les8`](#stdc_005fstore8_005fles8)
* [`stdc_store8_les16`](#stdc_005fstore8_005fles16)
* [`stdc_store8_les32`](#stdc_005fstore8_005fles32)
* [`stdc_store8_les64`](#stdc_005fstore8_005fles64)
* [`stdc_store8_leu8`](#stdc_005fstore8_005fleu8)
* [`stdc_store8_leu16`](#stdc_005fstore8_005fleu16)
* [`stdc_store8_leu32`](#stdc_005fstore8_005fleu32)
* [`stdc_store8_leu64`](#stdc_005fstore8_005fleu64)
* [`stdc_trailing_ones`](#stdc_005ftrailing_005fones)
* [`stdc_trailing_zeros`](#stdc_005ftrailing_005fzeros)

---

Next: [`stdc_bit_floor`](#stdc_005fbit_005ffloor), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.1 `stdc_bit_ceil` [¶](#stdc_005fbit_005fceil-1)

Documentation:  
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.16.

Gnulib module: stdc\_bit\_ceil

Portability problems fixed by Gnulib:

* This macro is missing on many platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.
* This macro returns a value of the wrong type
  (e.g. `unsigned long long` instead of `unsigned long`)
  on some platforms:
  Solaris 11 OpenIndiana.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_bit_width`](#stdc_005fbit_005fwidth), Previous: [`stdc_bit_ceil`](#stdc_005fbit_005fceil), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.2 `stdc_bit_floor` [¶](#stdc_005fbit_005ffloor-1)

Documentation:  
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.15.

Gnulib module: stdc\_bit\_floor

Portability problems fixed by Gnulib:

* This macro is missing on many platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.
* This macro returns a value of the wrong type
  (e.g. `unsigned long long` instead of `unsigned long`)
  on some platforms:
  Solaris 11 OpenIndiana.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_count_ones`](#stdc_005fcount_005fones), Previous: [`stdc_bit_floor`](#stdc_005fbit_005ffloor), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.3 `stdc_bit_width` [¶](#stdc_005fbit_005fwidth-1)

Documentation:  
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.14.

Gnulib module: stdc\_bit\_width

Portability problems fixed by Gnulib:

* This macro is missing on many platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_count_zeros`](#stdc_005fcount_005fzeros), Previous: [`stdc_bit_width`](#stdc_005fbit_005fwidth), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.4 `stdc_count_ones` [¶](#stdc_005fcount_005fones-1)

Documentation:  
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.12.

Gnulib module: stdc\_count\_ones

Portability problems fixed by Gnulib:

* This macro is missing on many platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_first_leading_one`](#stdc_005ffirst_005fleading_005fone), Previous: [`stdc_count_ones`](#stdc_005fcount_005fones), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.5 `stdc_count_zeros` [¶](#stdc_005fcount_005fzeros-1)

Documentation:  
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.11.

Gnulib module: stdc\_count\_zeros

Portability problems fixed by Gnulib:

* This macro is missing on many platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_first_leading_zero`](#stdc_005ffirst_005fleading_005fzero), Previous: [`stdc_count_zeros`](#stdc_005fcount_005fzeros), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.6 `stdc_first_leading_one` [¶](#stdc_005ffirst_005fleading_005fone-1)

Documentation:  
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.8.

Gnulib module: stdc\_first\_leading\_one

Portability problems fixed by Gnulib:

* This macro is missing on many platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_first_trailing_one`](#stdc_005ffirst_005ftrailing_005fone), Previous: [`stdc_first_leading_one`](#stdc_005ffirst_005fleading_005fone), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.7 `stdc_first_leading_zero` [¶](#stdc_005ffirst_005fleading_005fzero-1)

Documentation:  
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.7.

Gnulib module: stdc\_first\_leading\_zero

Portability problems fixed by Gnulib:

* This macro is missing on many platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_first_trailing_zero`](#stdc_005ffirst_005ftrailing_005fzero), Previous: [`stdc_first_leading_zero`](#stdc_005ffirst_005fleading_005fzero), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.8 `stdc_first_trailing_one` [¶](#stdc_005ffirst_005ftrailing_005fone-1)

Documentation:  
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.10.

Gnulib module: stdc\_first\_trailing\_one

Portability problems fixed by Gnulib:

* This macro is missing on many platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_has_single_bit`](#stdc_005fhas_005fsingle_005fbit), Previous: [`stdc_first_trailing_one`](#stdc_005ffirst_005ftrailing_005fone), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.9 `stdc_first_trailing_zero` [¶](#stdc_005ffirst_005ftrailing_005fzero-1)

Documentation:  
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.9.

Gnulib module: stdc\_first\_trailing\_zero

Portability problems fixed by Gnulib:

* This macro is missing on many platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_leading_ones`](#stdc_005fleading_005fones), Previous: [`stdc_first_trailing_zero`](#stdc_005ffirst_005ftrailing_005fzero), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.10 `stdc_has_single_bit` [¶](#stdc_005fhas_005fsingle_005fbit-1)

Documentation:  
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.13.

Gnulib module: stdc\_has\_single\_bit

Portability problems fixed by Gnulib:

* This macro is missing on many platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_leading_zeros`](#stdc_005fleading_005fzeros), Previous: [`stdc_has_single_bit`](#stdc_005fhas_005fsingle_005fbit), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.11 `stdc_leading_ones` [¶](#stdc_005fleading_005fones-1)

Documentation:  
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.4.

Gnulib module: stdc\_leading\_ones

Portability problems fixed by Gnulib:

* This macro is missing on many platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_bes8`](#stdc_005fload8_005faligned_005fbes8), Previous: [`stdc_leading_ones`](#stdc_005fleading_005fones), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.12 `stdc_leading_zeros` [¶](#stdc_005fleading_005fzeros-1)

Documentation:  
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.3.

Gnulib module: stdc\_leading\_zeros

Portability problems fixed by Gnulib:

* This macro is missing on many platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_bes16`](#stdc_005fload8_005faligned_005fbes16), Previous: [`stdc_leading_zeros`](#stdc_005fleading_005fzeros), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.13 `stdc_load8_aligned_bes8` [¶](#stdc_005fload8_005faligned_005fbes8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_bes32`](#stdc_005fload8_005faligned_005fbes32), Previous: [`stdc_load8_aligned_bes8`](#stdc_005fload8_005faligned_005fbes8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.14 `stdc_load8_aligned_bes16` [¶](#stdc_005fload8_005faligned_005fbes16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_bes64`](#stdc_005fload8_005faligned_005fbes64), Previous: [`stdc_load8_aligned_bes16`](#stdc_005fload8_005faligned_005fbes16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.15 `stdc_load8_aligned_bes32` [¶](#stdc_005fload8_005faligned_005fbes32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_beu8`](#stdc_005fload8_005faligned_005fbeu8), Previous: [`stdc_load8_aligned_bes32`](#stdc_005fload8_005faligned_005fbes32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.16 `stdc_load8_aligned_bes64` [¶](#stdc_005fload8_005faligned_005fbes64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_beu16`](#stdc_005fload8_005faligned_005fbeu16), Previous: [`stdc_load8_aligned_bes64`](#stdc_005fload8_005faligned_005fbes64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.17 `stdc_load8_aligned_beu8` [¶](#stdc_005fload8_005faligned_005fbeu8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_beu32`](#stdc_005fload8_005faligned_005fbeu32), Previous: [`stdc_load8_aligned_beu8`](#stdc_005fload8_005faligned_005fbeu8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.18 `stdc_load8_aligned_beu16` [¶](#stdc_005fload8_005faligned_005fbeu16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_beu64`](#stdc_005fload8_005faligned_005fbeu64), Previous: [`stdc_load8_aligned_beu16`](#stdc_005fload8_005faligned_005fbeu16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.19 `stdc_load8_aligned_beu32` [¶](#stdc_005fload8_005faligned_005fbeu32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_les8`](#stdc_005fload8_005faligned_005fles8), Previous: [`stdc_load8_aligned_beu32`](#stdc_005fload8_005faligned_005fbeu32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.20 `stdc_load8_aligned_beu64` [¶](#stdc_005fload8_005faligned_005fbeu64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_les16`](#stdc_005fload8_005faligned_005fles16), Previous: [`stdc_load8_aligned_beu64`](#stdc_005fload8_005faligned_005fbeu64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.21 `stdc_load8_aligned_les8` [¶](#stdc_005fload8_005faligned_005fles8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_les32`](#stdc_005fload8_005faligned_005fles32), Previous: [`stdc_load8_aligned_les8`](#stdc_005fload8_005faligned_005fles8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.22 `stdc_load8_aligned_les16` [¶](#stdc_005fload8_005faligned_005fles16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_les64`](#stdc_005fload8_005faligned_005fles64), Previous: [`stdc_load8_aligned_les16`](#stdc_005fload8_005faligned_005fles16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.23 `stdc_load8_aligned_les32` [¶](#stdc_005fload8_005faligned_005fles32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_leu8`](#stdc_005fload8_005faligned_005fleu8), Previous: [`stdc_load8_aligned_les32`](#stdc_005fload8_005faligned_005fles32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.24 `stdc_load8_aligned_les64` [¶](#stdc_005fload8_005faligned_005fles64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_leu16`](#stdc_005fload8_005faligned_005fleu16), Previous: [`stdc_load8_aligned_les64`](#stdc_005fload8_005faligned_005fles64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.25 `stdc_load8_aligned_leu8` [¶](#stdc_005fload8_005faligned_005fleu8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_leu32`](#stdc_005fload8_005faligned_005fleu32), Previous: [`stdc_load8_aligned_leu8`](#stdc_005fload8_005faligned_005fleu8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.26 `stdc_load8_aligned_leu16` [¶](#stdc_005fload8_005faligned_005fleu16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_aligned_leu64`](#stdc_005fload8_005faligned_005fleu64), Previous: [`stdc_load8_aligned_leu16`](#stdc_005fload8_005faligned_005fleu16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.27 `stdc_load8_aligned_leu32` [¶](#stdc_005fload8_005faligned_005fleu32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_bes8`](#stdc_005fload8_005fbes8), Previous: [`stdc_load8_aligned_leu32`](#stdc_005fload8_005faligned_005fleu32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.28 `stdc_load8_aligned_leu64` [¶](#stdc_005fload8_005faligned_005fleu64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_bes16`](#stdc_005fload8_005fbes16), Previous: [`stdc_load8_aligned_leu64`](#stdc_005fload8_005faligned_005fleu64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.29 `stdc_load8_bes8` [¶](#stdc_005fload8_005fbes8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_bes32`](#stdc_005fload8_005fbes32), Previous: [`stdc_load8_bes8`](#stdc_005fload8_005fbes8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.30 `stdc_load8_bes16` [¶](#stdc_005fload8_005fbes16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_bes64`](#stdc_005fload8_005fbes64), Previous: [`stdc_load8_bes16`](#stdc_005fload8_005fbes16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.31 `stdc_load8_bes32` [¶](#stdc_005fload8_005fbes32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_beu8`](#stdc_005fload8_005fbeu8), Previous: [`stdc_load8_bes32`](#stdc_005fload8_005fbes32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.32 `stdc_load8_bes64` [¶](#stdc_005fload8_005fbes64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_beu16`](#stdc_005fload8_005fbeu16), Previous: [`stdc_load8_bes64`](#stdc_005fload8_005fbes64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.33 `stdc_load8_beu8` [¶](#stdc_005fload8_005fbeu8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_beu32`](#stdc_005fload8_005fbeu32), Previous: [`stdc_load8_beu8`](#stdc_005fload8_005fbeu8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.34 `stdc_load8_beu16` [¶](#stdc_005fload8_005fbeu16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_beu64`](#stdc_005fload8_005fbeu64), Previous: [`stdc_load8_beu16`](#stdc_005fload8_005fbeu16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.35 `stdc_load8_beu32` [¶](#stdc_005fload8_005fbeu32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_les8`](#stdc_005fload8_005fles8), Previous: [`stdc_load8_beu32`](#stdc_005fload8_005fbeu32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.36 `stdc_load8_beu64` [¶](#stdc_005fload8_005fbeu64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_les16`](#stdc_005fload8_005fles16), Previous: [`stdc_load8_beu64`](#stdc_005fload8_005fbeu64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.37 `stdc_load8_les8` [¶](#stdc_005fload8_005fles8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_les32`](#stdc_005fload8_005fles32), Previous: [`stdc_load8_les8`](#stdc_005fload8_005fles8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.38 `stdc_load8_les16` [¶](#stdc_005fload8_005fles16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_les64`](#stdc_005fload8_005fles64), Previous: [`stdc_load8_les16`](#stdc_005fload8_005fles16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.39 `stdc_load8_les32` [¶](#stdc_005fload8_005fles32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_leu8`](#stdc_005fload8_005fleu8), Previous: [`stdc_load8_les32`](#stdc_005fload8_005fles32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.40 `stdc_load8_les64` [¶](#stdc_005fload8_005fles64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_leu16`](#stdc_005fload8_005fleu16), Previous: [`stdc_load8_les64`](#stdc_005fload8_005fles64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.41 `stdc_load8_leu8` [¶](#stdc_005fload8_005fleu8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_leu32`](#stdc_005fload8_005fleu32), Previous: [`stdc_load8_leu8`](#stdc_005fload8_005fleu8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.42 `stdc_load8_leu16` [¶](#stdc_005fload8_005fleu16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_load8_leu64`](#stdc_005fload8_005fleu64), Previous: [`stdc_load8_leu16`](#stdc_005fload8_005fleu16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.43 `stdc_load8_leu32` [¶](#stdc_005fload8_005fleu32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_memreverse8`](#stdc_005fmemreverse8), Previous: [`stdc_load8_leu32`](#stdc_005fload8_005fleu32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.44 `stdc_load8_leu64` [¶](#stdc_005fload8_005fleu64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.21.

Gnulib module: stdc\_load8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_memreverse8u8`](#stdc_005fmemreverse8u8), Previous: [`stdc_load8_leu64`](#stdc_005fload8_005fleu64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.45 `stdc_memreverse8` [¶](#stdc_005fmemreverse8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.19.

Gnulib module: stdc\_memreverse8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_memreverse8u16`](#stdc_005fmemreverse8u16), Previous: [`stdc_memreverse8`](#stdc_005fmemreverse8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.46 `stdc_memreverse8u8` [¶](#stdc_005fmemreverse8u8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.20.

Gnulib module: stdc\_memreverse8u

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_memreverse8u32`](#stdc_005fmemreverse8u32), Previous: [`stdc_memreverse8u8`](#stdc_005fmemreverse8u8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.47 `stdc_memreverse8u16` [¶](#stdc_005fmemreverse8u16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.20.

Gnulib module: stdc\_memreverse8u

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_memreverse8u64`](#stdc_005fmemreverse8u64), Previous: [`stdc_memreverse8u16`](#stdc_005fmemreverse8u16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.48 `stdc_memreverse8u32` [¶](#stdc_005fmemreverse8u32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.20.

Gnulib module: stdc\_memreverse8u

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_rotate_left`](#stdc_005frotate_005fleft), Previous: [`stdc_memreverse8u32`](#stdc_005fmemreverse8u32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.49 `stdc_memreverse8u64` [¶](#stdc_005fmemreverse8u64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.20.

Gnulib module: stdc\_memreverse8u

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_rotate_right`](#stdc_005frotate_005fright), Previous: [`stdc_memreverse8u64`](#stdc_005fmemreverse8u64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.50 `stdc_rotate_left` [¶](#stdc_005frotate_005fleft-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.17.

Gnulib module: stdc\_rotate\_left

Portability problems fixed by Gnulib:

* This macro and its associated functions is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_bes8`](#stdc_005fstore8_005faligned_005fbes8), Previous: [`stdc_rotate_left`](#stdc_005frotate_005fleft), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.51 `stdc_rotate_right` [¶](#stdc_005frotate_005fright-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.18.

Gnulib module: stdc\_rotate\_right

Portability problems fixed by Gnulib:

* This macro and its associated functions is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_bes16`](#stdc_005fstore8_005faligned_005fbes16), Previous: [`stdc_rotate_right`](#stdc_005frotate_005fright), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.52 `stdc_store8_aligned_bes8` [¶](#stdc_005fstore8_005faligned_005fbes8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_bes32`](#stdc_005fstore8_005faligned_005fbes32), Previous: [`stdc_store8_aligned_bes8`](#stdc_005fstore8_005faligned_005fbes8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.53 `stdc_store8_aligned_bes16` [¶](#stdc_005fstore8_005faligned_005fbes16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_bes64`](#stdc_005fstore8_005faligned_005fbes64), Previous: [`stdc_store8_aligned_bes16`](#stdc_005fstore8_005faligned_005fbes16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.54 `stdc_store8_aligned_bes32` [¶](#stdc_005fstore8_005faligned_005fbes32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_beu8`](#stdc_005fstore8_005faligned_005fbeu8), Previous: [`stdc_store8_aligned_bes32`](#stdc_005fstore8_005faligned_005fbes32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.55 `stdc_store8_aligned_bes64` [¶](#stdc_005fstore8_005faligned_005fbes64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_beu16`](#stdc_005fstore8_005faligned_005fbeu16), Previous: [`stdc_store8_aligned_bes64`](#stdc_005fstore8_005faligned_005fbes64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.56 `stdc_store8_aligned_beu8` [¶](#stdc_005fstore8_005faligned_005fbeu8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_beu32`](#stdc_005fstore8_005faligned_005fbeu32), Previous: [`stdc_store8_aligned_beu8`](#stdc_005fstore8_005faligned_005fbeu8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.57 `stdc_store8_aligned_beu16` [¶](#stdc_005fstore8_005faligned_005fbeu16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_beu64`](#stdc_005fstore8_005faligned_005fbeu64), Previous: [`stdc_store8_aligned_beu16`](#stdc_005fstore8_005faligned_005fbeu16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.58 `stdc_store8_aligned_beu32` [¶](#stdc_005fstore8_005faligned_005fbeu32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_les8`](#stdc_005fstore8_005faligned_005fles8), Previous: [`stdc_store8_aligned_beu32`](#stdc_005fstore8_005faligned_005fbeu32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.59 `stdc_store8_aligned_beu64` [¶](#stdc_005fstore8_005faligned_005fbeu64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_les16`](#stdc_005fstore8_005faligned_005fles16), Previous: [`stdc_store8_aligned_beu64`](#stdc_005fstore8_005faligned_005fbeu64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.60 `stdc_store8_aligned_les8` [¶](#stdc_005fstore8_005faligned_005fles8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_les32`](#stdc_005fstore8_005faligned_005fles32), Previous: [`stdc_store8_aligned_les8`](#stdc_005fstore8_005faligned_005fles8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.61 `stdc_store8_aligned_les16` [¶](#stdc_005fstore8_005faligned_005fles16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_les64`](#stdc_005fstore8_005faligned_005fles64), Previous: [`stdc_store8_aligned_les16`](#stdc_005fstore8_005faligned_005fles16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.62 `stdc_store8_aligned_les32` [¶](#stdc_005fstore8_005faligned_005fles32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_leu8`](#stdc_005fstore8_005faligned_005fleu8), Previous: [`stdc_store8_aligned_les32`](#stdc_005fstore8_005faligned_005fles32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.63 `stdc_store8_aligned_les64` [¶](#stdc_005fstore8_005faligned_005fles64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_leu16`](#stdc_005fstore8_005faligned_005fleu16), Previous: [`stdc_store8_aligned_les64`](#stdc_005fstore8_005faligned_005fles64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.64 `stdc_store8_aligned_leu8` [¶](#stdc_005fstore8_005faligned_005fleu8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_leu32`](#stdc_005fstore8_005faligned_005fleu32), Previous: [`stdc_store8_aligned_leu8`](#stdc_005fstore8_005faligned_005fleu8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.65 `stdc_store8_aligned_leu16` [¶](#stdc_005fstore8_005faligned_005fleu16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_aligned_leu64`](#stdc_005fstore8_005faligned_005fleu64), Previous: [`stdc_store8_aligned_leu16`](#stdc_005fstore8_005faligned_005fleu16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.66 `stdc_store8_aligned_leu32` [¶](#stdc_005fstore8_005faligned_005fleu32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_bes8`](#stdc_005fstore8_005fbes8), Previous: [`stdc_store8_aligned_leu32`](#stdc_005fstore8_005faligned_005fleu32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.67 `stdc_store8_aligned_leu64` [¶](#stdc_005fstore8_005faligned_005fleu64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8\_aligned

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_bes16`](#stdc_005fstore8_005fbes16), Previous: [`stdc_store8_aligned_leu64`](#stdc_005fstore8_005faligned_005fleu64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.68 `stdc_store8_bes8` [¶](#stdc_005fstore8_005fbes8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_bes32`](#stdc_005fstore8_005fbes32), Previous: [`stdc_store8_bes8`](#stdc_005fstore8_005fbes8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.69 `stdc_store8_bes16` [¶](#stdc_005fstore8_005fbes16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_bes64`](#stdc_005fstore8_005fbes64), Previous: [`stdc_store8_bes16`](#stdc_005fstore8_005fbes16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.70 `stdc_store8_bes32` [¶](#stdc_005fstore8_005fbes32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_beu8`](#stdc_005fstore8_005fbeu8), Previous: [`stdc_store8_bes32`](#stdc_005fstore8_005fbes32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.71 `stdc_store8_bes64` [¶](#stdc_005fstore8_005fbes64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_beu16`](#stdc_005fstore8_005fbeu16), Previous: [`stdc_store8_bes64`](#stdc_005fstore8_005fbes64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.72 `stdc_store8_beu8` [¶](#stdc_005fstore8_005fbeu8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_beu32`](#stdc_005fstore8_005fbeu32), Previous: [`stdc_store8_beu8`](#stdc_005fstore8_005fbeu8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.73 `stdc_store8_beu16` [¶](#stdc_005fstore8_005fbeu16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_beu64`](#stdc_005fstore8_005fbeu64), Previous: [`stdc_store8_beu16`](#stdc_005fstore8_005fbeu16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.74 `stdc_store8_beu32` [¶](#stdc_005fstore8_005fbeu32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_les8`](#stdc_005fstore8_005fles8), Previous: [`stdc_store8_beu32`](#stdc_005fstore8_005fbeu32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.75 `stdc_store8_beu64` [¶](#stdc_005fstore8_005fbeu64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_les16`](#stdc_005fstore8_005fles16), Previous: [`stdc_store8_beu64`](#stdc_005fstore8_005fbeu64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.76 `stdc_store8_les8` [¶](#stdc_005fstore8_005fles8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_les32`](#stdc_005fstore8_005fles32), Previous: [`stdc_store8_les8`](#stdc_005fstore8_005fles8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.77 `stdc_store8_les16` [¶](#stdc_005fstore8_005fles16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_les64`](#stdc_005fstore8_005fles64), Previous: [`stdc_store8_les16`](#stdc_005fstore8_005fles16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.78 `stdc_store8_les32` [¶](#stdc_005fstore8_005fles32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_leu8`](#stdc_005fstore8_005fleu8), Previous: [`stdc_store8_les32`](#stdc_005fstore8_005fles32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.79 `stdc_store8_les64` [¶](#stdc_005fstore8_005fles64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_leu16`](#stdc_005fstore8_005fleu16), Previous: [`stdc_store8_les64`](#stdc_005fstore8_005fles64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.80 `stdc_store8_leu8` [¶](#stdc_005fstore8_005fleu8-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_leu32`](#stdc_005fstore8_005fleu32), Previous: [`stdc_store8_leu8`](#stdc_005fstore8_005fleu8), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.81 `stdc_store8_leu16` [¶](#stdc_005fstore8_005fleu16-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_store8_leu64`](#stdc_005fstore8_005fleu64), Previous: [`stdc_store8_leu16`](#stdc_005fstore8_005fleu16), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.82 `stdc_store8_leu32` [¶](#stdc_005fstore8_005fleu32-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_trailing_ones`](#stdc_005ftrailing_005fones), Previous: [`stdc_store8_leu32`](#stdc_005fstore8_005fleu32), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.83 `stdc_store8_leu64` [¶](#stdc_005fstore8_005fleu64-1)

Documentation:  
ISO C2y (draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3783.pdf>)
section 7.18.22.

Gnulib module: stdc\_store8

Portability problems fixed by Gnulib:

* This function is missing on many platforms:
  glibc 2.43, macOS 26, FreeBSD 15.0, NetBSD 10.0, OpenBSD 7.8, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [`stdc_trailing_zeros`](#stdc_005ftrailing_005fzeros), Previous: [`stdc_store8_leu64`](#stdc_005fstore8_005fleu64), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.84 `stdc_trailing_ones` [¶](#stdc_005ftrailing_005fones-1)

Documentation:  
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.6.

Gnulib module: stdc\_trailing\_ones

Portability problems fixed by Gnulib:

* This macro is missing on many platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Previous: [`stdc_trailing_ones`](#stdc_005ftrailing_005fones), Up: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 10.44.85 `stdc_trailing_zeros` [¶](#stdc_005ftrailing_005fzeros-1)

Documentation:  
ISO C23 (latest free draft
<https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf>)
section 7.18.5.

Gnulib module: stdc\_trailing\_zeros

Portability problems fixed by Gnulib:

* This macro is missing on many platforms:
  glibc 2.38, macOS 14, FreeBSD 14.0, NetBSD 10.0, OpenBSD 7.9, AIX 7.3.1, HP-UX 11.31, Solaris 11.4, Cygwin 3.5.3, mingw, MSVC 17, Android 15.

Portability problems not fixed by Gnulib:

---

Next: [Functions in `<stdlib.h>`](#Functions-in-_003cstdlib_002eh_003e), Previous: [Functions and generic functions in `<stdbit.h>`](#Functions-in-_003cstdbit_002eh_003e), Up: [ISO C and POSIX Function Substitutes](#Function-Substitutes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
