### ADDENDUM: How to use this License for your documents [¶](#ADDENDUM_003a-How-to-use-this-License-for-your-documents)

To use this License in a document you have written, include a copy of
the License in the document and put the following copyright and
license notices just after the title page:

```
  Copyright (C)  year  your name.
  Permission is granted to copy, distribute and/or modify this document
  under the terms of the GNU Free Documentation License, Version 1.3
  or any later version published by the Free Software Foundation;
  with no Invariant Sections, no Front-Cover Texts, and no Back-Cover
  Texts.  A copy of the license is included in the section entitled ``GNU
  Free Documentation License''.
```

If you have Invariant Sections, Front-Cover Texts and Back-Cover Texts,
replace the “with…Texts.” line with this:

```
    with the Invariant Sections being list their titles, with
    the Front-Cover Texts being list, and with the Back-Cover Texts
    being list.
```

If you have Invariant Sections without Cover Texts, or some other
combination of the three, merge those two alternatives to suit the
situation.

If your document contains nontrivial examples of program code, we
recommend releasing these examples in parallel under your choice of
free software license, such as the GNU General Public License,
to permit their use in free software.

---

Next: [Module Index](#Module-Index), Previous: [GNU Free Documentation License](#GNU-Free-Documentation-License), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## Appendix B Gnulib history [¶](#Gnulib-history-1)

In the beginning, Richard Stallman maintained the regular expression engine
of GNU and made it available to other GNU packages, such as
`sed`, `awk`, `grep`, so that they could use the same engine.
Recall that there was no GNU C library at the time.

A couple of years later, Jim Meyering, who was maintaining
the shell utilities, the file utilities, and the text processing utilities,
collected the common parts of these three packages in the same repository.
Paul Eggert joined in, with code coming from a few GNU packages that he
maintained,
and so did Bruno Haible, with reusable code from the GNU gettext package.

As they cared for portability, many changes in the C code were accompanied
by changes in the build infrastructure.
Copying these changes into packages — it was all done manually — became
cumbersome. So they wrote a program, called ‘`gnulib-tool`’, that does
this job of copying the requested shared code into a particular package.
This was in 2002.

Providing a substitute / override for a system function was relatively easy.
Providing a substitute / override for a system header file was significantly
harder, but was done successively:
for `stdint.h` in 2004,
for `stdarg.h`, `sys/socket.h`, `sys/stat.h` in 2006,
for `sys/time.h`, `wchar.h` in 2007,
and the development of the corresponding idioms took until 2010.

Unicode string modules (that make up GNU libunistring) were added in 2007–2009.

Modules for numeric functions (`<math.h>`) were added in 2010–2011.

Modules for container data structures were added between 2006 and 2018.

Versatile bit-set modules were added in 2018.

POSIX threads on non-POSIX platforms, as well as ISO C threads on all
platforms, were added in 2019.

The `posix_spawn` facility was brought to completion on native Windows
in 2022, providing the world’s first `posix_spawn` implementation for
this platform.

Support for Android was added in 2023 and immediately used by GNU Emacs
for Android.

Functions for working with Unicode characters in multibyte representation,
based on `mbrtoc32`, were added in 2023.

Modules for manipulating the floating-point environment (`fenv.h`) were
added in 2023.

The ‘`gnulib-tool`’ rewrite in Python, that was started by Dmitry Selyutin
in 2012 but lay unfinished for many years, was completed by Collin Funk and
Bruno Haible in 2024.

---

Next: [Undocumented Module Index](#Undocumented-Module-Index), Previous: [Gnulib history](#Gnulib-history), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## Module Index [¶](#Module-Index-1)

|  |  |
| --- | --- |
| Jump to: | [**\_**](#Module-Index_m_symbol-1)     [**A**](#Module-Index_m_letter-A)   [**B**](#Module-Index_m_letter-B)   [**C**](#Module-Index_m_letter-C)   [**D**](#Module-Index_m_letter-D)   [**E**](#Module-Index_m_letter-E)   [**F**](#Module-Index_m_letter-F)   [**G**](#Module-Index_m_letter-G)   [**H**](#Module-Index_m_letter-H)   [**I**](#Module-Index_m_letter-I)   [**J**](#Module-Index_m_letter-J)   [**L**](#Module-Index_m_letter-L)   [**M**](#Module-Index_m_letter-M)   [**N**](#Module-Index_m_letter-N)   [**O**](#Module-Index_m_letter-O)   [**P**](#Module-Index_m_letter-P)   [**Q**](#Module-Index_m_letter-Q)   [**R**](#Module-Index_m_letter-R)   [**S**](#Module-Index_m_letter-S)   [**T**](#Module-Index_m_letter-T)   [**U**](#Module-Index_m_letter-U)   [**V**](#Module-Index_m_letter-V)   [**W**](#Module-Index_m_letter-W)   [**X**](#Module-Index_m_letter-X)   [**Y**](#Module-Index_m_letter-Y)   [**Z**](#Module-Index_m_letter-Z) |

|  |  |  |
| --- | --- | --- |
|  | Index Entry | Section |
| --- | | |
| \_ |
|  | [`_Exit`](#index-_005fExit-1) | [\_Exit](#g_t_005fExit) |
| --- | | |
| A |
|  | [`abort-debug`](#index-abort_002ddebug) | [Stack traces](#Stack-traces) |
|  | [`accept`](#index-accept-1) | [accept](#accept) |
|  | [`accept4`](#index-accept4-1) | [accept4](#accept4) |
|  | [`access`](#index-access-1) | [access](#access) |
|  | [`acos`](#index-acos-1) | [acos](#acos) |
|  | [`acosf`](#index-acosf-1) | [acosf](#acosf) |
|  | [`acosl`](#index-acosl-1) | [acosl](#acosl) |
|  | [`agpl-3.0`](#index-agpl_002d3_002e0) | [License Texinfo sources](#License-Texinfo-sources) |
|  | [`alignalloc`](#index-alignalloc) | [aligned\_alloc](#aligned_005falloc) |
|  | [`alignasof`](#index-alignasof) | [alignof](#alignof) |
|  | [`alignasof`](#index-alignasof-1) | [stdalign.h](#stdalign_002eh) |
|  | [`aligned_alloc`](#index-aligned_005falloc-1) | [aligned\_alloc](#aligned_005falloc) |
|  | [`aligned-malloc`](#index-aligned_002dmalloc) | [aligned\_alloc](#aligned_005falloc) |
|  | [`aligned-malloc`](#index-aligned_002dmalloc-1) | [posix\_memalign](#posix_005fmemalign) |
|  | [`aligned-malloc`](#index-aligned_002dmalloc-2) | [memalign](#memalign) |
|  | [`alloca`](#index-alloca-2) | [alloca.h](#alloca_002eh) |
|  | [`alloca`](#index-alloca-3) | [alloca](#alloca) |
|  | [`alloca-opt`](#index-alloca_002dopt) | [alloca](#alloca) |
|  | [`alloca-opt`](#index-alloca_002dopt-1) | [alloca-opt](#alloca_002dopt) |
|  | [`alphasort`](#index-alphasort-1) | [alphasort](#alphasort) |
|  | [`announce-gen`](#index-announce_002dgen) | [maintainer-makefile](#maintainer_002dmakefile) |
|  | [`areadlink`](#index-areadlink) | [readlink](#readlink) |
|  | [`areadlink`](#index-areadlink-1) | [readlinkat](#readlinkat) |
|  | [`areadlink-with-size`](#index-areadlink_002dwith_002dsize) | [sys/stat.h](#sys_002fstat_002eh) |
|  | [`argmatch`](#index-argmatch) | [Recognizing Option Arguments](#Recognizing-Option-Arguments) |
|  | [`argp`](#index-argp) | [argp.h](#argp_002eh) |
|  | [`argp`](#index-argp-1) | [argp\_err\_exit\_status](#argp_005ferr_005fexit_005fstatus) |
|  | [`argp`](#index-argp-2) | [argp\_error](#argp_005ferror) |
|  | [`argp`](#index-argp-3) | [argp\_failure](#argp_005ffailure) |
|  | [`argp`](#index-argp-4) | [argp\_help](#argp_005fhelp) |
|  | [`argp`](#index-argp-5) | [argp\_parse](#argp_005fparse) |
|  | [`argp`](#index-argp-6) | [argp\_program\_bug\_address](#argp_005fprogram_005fbug_005faddress) |
|  | [`argp`](#index-argp-7) | [argp\_program\_version](#argp_005fprogram_005fversion) |
|  | [`argp`](#index-argp-8) | [argp\_program\_version\_hook](#argp_005fprogram_005fversion_005fhook) |
|  | [`argp`](#index-argp-9) | [argp\_state\_help](#argp_005fstate_005fhelp) |
|  | [`argp`](#index-argp-10) | [argp\_usage](#argp_005fusage) |
|  | [`argp-version-etc`](#index-argp_002dversion_002detc) | [Printing --version and bug-reporting information](#Printing-_002d_002dversion-and-bug_002dreporting-information) |
|  | [`argz`](#index-argz) | [argz.h](#argz_002eh) |
|  | [`argz`](#index-argz-1) | [argz\_add](#argz_005fadd) |
|  | [`argz`](#index-argz-2) | [argz\_add\_sep](#argz_005fadd_005fsep) |
|  | [`argz`](#index-argz-3) | [argz\_append](#argz_005fappend) |
|  | [`argz`](#index-argz-4) | [argz\_count](#argz_005fcount) |
|  | [`argz`](#index-argz-5) | [argz\_create](#argz_005fcreate) |
|  | [`argz`](#index-argz-6) | [argz\_create\_sep](#argz_005fcreate_005fsep) |
|  | [`argz`](#index-argz-7) | [argz\_delete](#argz_005fdelete) |
|  | [`argz`](#index-argz-8) | [argz\_extract](#argz_005fextract) |
|  | [`argz`](#index-argz-9) | [argz\_insert](#argz_005finsert) |
|  | [`argz`](#index-argz-10) | [argz\_next](#argz_005fnext) |
|  | [`argz`](#index-argz-11) | [argz\_replace](#argz_005freplace) |
|  | [`argz`](#index-argz-12) | [argz\_stringify](#argz_005fstringify) |
|  | [`arpa_inet-h`](#index-arpa_005finet_002dh) | [arpa/inet.h](#arpa_002finet_002eh) |
|  | [`array-list`](#index-array_002dlist) | [Ordinary containers](#Ordinary-containers) |
|  | [`array-map`](#index-array_002dmap) | [Ordinary containers](#Ordinary-containers) |
|  | [`array-omap`](#index-array_002domap) | [Ordinary containers](#Ordinary-containers) |
|  | [`array-oset`](#index-array_002doset) | [Ordinary containers](#Ordinary-containers) |
|  | [`array-set`](#index-array_002dset) | [Ordinary containers](#Ordinary-containers) |
|  | [`asin`](#index-asin-1) | [asin](#asin) |
|  | [`asinf`](#index-asinf-1) | [asinf](#asinf) |
|  | [`asinl`](#index-asinl-1) | [asinl](#asinl) |
|  | [`assert`](#index-assert-1) | [assert.h](#assert_002eh) |
|  | [`assert`](#index-assert-2) | [assert](#assert) |
|  | [`assert-h`](#index-assert_002dh) | [static\_assert](#static_005fassert) |
|  | [`assert-h`](#index-assert_002dh-1) | [assert.h](#assert_002eh) |
|  | [`atan`](#index-atan-1) | [atan](#atan) |
|  | [`atan2`](#index-atan2-1) | [atan2](#atan2) |
|  | [`atan2f`](#index-atan2f-1) | [atan2f](#atan2f) |
|  | [`atanf`](#index-atanf-1) | [atanf](#atanf) |
|  | [`atanl`](#index-atanl-1) | [atanl](#atanl) |
|  | [`atexit`](#index-atexit-1) | [atexit](#atexit) |
|  | [`atoll`](#index-atoll-1) | [atoll](#atoll) |
|  | [`attribute`](#index-attribute) | [Attributes](#Attributes) |
|  | [`avltree-list`](#index-avltree_002dlist) | [Ordinary containers](#Ordinary-containers) |
|  | [`avltree-omap`](#index-avltree_002domap) | [Ordinary containers](#Ordinary-containers) |
|  | [`avltree-oset`](#index-avltree_002doset) | [Ordinary containers](#Ordinary-containers) |
|  | [`avltreehash-list`](#index-avltreehash_002dlist) | [Ordinary containers](#Ordinary-containers) |
| --- | | |
| B |
|  | [`basename-lgpl`](#index-basename_002dlgpl) | [basename](#basename) |
|  | [`bcopy`](#index-bcopy-1) | [bcopy](#bcopy) |
|  | [`bind`](#index-bind-1) | [bind](#bind) |
|  | [`bool`](#index-bool) | [C99 features assumed](#C99-features-assumed) |
|  | [`bool`](#index-bool-1) | [bool](#bool) |
|  | [`bool`](#index-bool-2) | [stdbool.h](#stdbool_002eh) |
|  | [`bsearch`](#index-bsearch-1) | [bsearch](#bsearch) |
|  | [`btoc32`](#index-btoc32) | [btowc](#btowc) |
|  | [`btowc`](#index-btowc-1) | [btowc](#btowc) |
|  | [`byteswap`](#index-byteswap) | [byteswap.h](#byteswap_002eh) |
|  | [`byteswap`](#index-byteswap-1) | [bswap\_16](#bswap_005f16) |
|  | [`byteswap`](#index-byteswap-2) | [bswap\_32](#bswap_005f32) |
|  | [`byteswap`](#index-byteswap-3) | [bswap\_64](#bswap_005f64) |
| --- | | |
| C |
|  | [`c-ctype`](#index-c_002dctype) | [isalnum](#isalnum) |
|  | [`c-ctype`](#index-c_002dctype-1) | [isalpha](#isalpha) |
|  | [`c-ctype`](#index-c_002dctype-2) | [isblank](#isblank) |
|  | [`c-ctype`](#index-c_002dctype-3) | [iscntrl](#iscntrl) |
|  | [`c-ctype`](#index-c_002dctype-4) | [isdigit](#isdigit) |
|  | [`c-ctype`](#index-c_002dctype-5) | [isgraph](#isgraph) |
|  | [`c-ctype`](#index-c_002dctype-6) | [islower](#islower) |
|  | [`c-ctype`](#index-c_002dctype-7) | [isprint](#isprint) |
|  | [`c-ctype`](#index-c_002dctype-8) | [ispunct](#ispunct) |
|  | [`c-ctype`](#index-c_002dctype-9) | [isspace](#isspace) |
|  | [`c-ctype`](#index-c_002dctype-10) | [isupper](#isupper) |
|  | [`c-ctype`](#index-c_002dctype-11) | [isxdigit](#isxdigit) |
|  | [`c-ctype`](#index-c_002dctype-12) | [tolower](#tolower) |
|  | [`c-ctype`](#index-c_002dctype-13) | [toupper](#toupper) |
|  | [`c-ctype`](#index-c_002dctype-14) | [isascii](#isascii) |
|  | [`c-ctype`](#index-c_002dctype-15) | [c-ctype](#c_002dctype) |
|  | [`c-ctype`](#index-c_002dctype-16) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c-snzprintf`](#index-c_002dsnzprintf) | [Modernized printf](#Modernized-printf) |
|  | [`c-snzprintf-gnu`](#index-c_002dsnzprintf_002dgnu) | [Modernized printf](#Modernized-printf) |
|  | [`c-strcasecmp`](#index-c_002dstrcasecmp) | [c-strcasecmp](#c_002dstrcasecmp) |
|  | [`c-strcaseeq`](#index-c_002dstrcaseeq) | [c-strcaseeq](#c_002dstrcaseeq) |
|  | [`c-strcasestr`](#index-c_002dstrcasestr) | [c-strcasestr](#c_002dstrcasestr) |
|  | [`c-strncasecmp`](#index-c_002dstrncasecmp) | [c-strncasecmp](#c_002dstrncasecmp) |
|  | [`c-strstr`](#index-c_002dstrstr) | [c-strstr](#c_002dstrstr) |
|  | [`c-strtod`](#index-c_002dstrtod) | [c-strtod](#c_002dstrtod) |
|  | [`c-strtod`](#index-c_002dstrtod-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`c-strtold`](#index-c_002dstrtold) | [c-strtold](#c_002dstrtold) |
|  | [`c-strtold`](#index-c_002dstrtold-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`c-vaszprintf`](#index-c_002dvaszprintf) | [Modernized printf](#Modernized-printf) |
|  | [`c-vaszprintf-gnu`](#index-c_002dvaszprintf_002dgnu) | [Modernized printf](#Modernized-printf) |
|  | [`c-vsnzprintf`](#index-c_002dvsnzprintf) | [Modernized printf](#Modernized-printf) |
|  | [`c-vsnzprintf-gnu`](#index-c_002dvsnzprintf_002dgnu) | [Modernized printf](#Modernized-printf) |
|  | [`c-xvasprintf`](#index-c_002dxvasprintf) | [Modernized printf](#Modernized-printf) |
|  | [`c32_apply_mapping`](#index-c32_005fapply_005fmapping) | [towctrans](#towctrans) |
|  | [`c32_apply_mapping`](#index-c32_005fapply_005fmapping-1) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32_apply_type_test`](#index-c32_005fapply_005ftype_005ftest) | [iswctype](#iswctype) |
|  | [`c32_apply_type_test`](#index-c32_005fapply_005ftype_005ftest-1) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32_get_mapping`](#index-c32_005fget_005fmapping) | [wctrans](#wctrans) |
|  | [`c32_get_mapping`](#index-c32_005fget_005fmapping-1) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32_get_type_test`](#index-c32_005fget_005ftype_005ftest) | [wctype](#wctype) |
|  | [`c32_get_type_test`](#index-c32_005fget_005ftype_005ftest-1) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32isalnum`](#index-c32isalnum) | [isalnum](#isalnum) |
|  | [`c32isalnum`](#index-c32isalnum-1) | [iswalnum](#iswalnum) |
|  | [`c32isalnum`](#index-c32isalnum-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32isalpha`](#index-c32isalpha) | [isalpha](#isalpha) |
|  | [`c32isalpha`](#index-c32isalpha-1) | [iswalpha](#iswalpha) |
|  | [`c32isalpha`](#index-c32isalpha-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32isblank`](#index-c32isblank) | [isblank](#isblank) |
|  | [`c32isblank`](#index-c32isblank-1) | [iswblank](#iswblank) |
|  | [`c32isblank`](#index-c32isblank-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32iscntrl`](#index-c32iscntrl) | [iscntrl](#iscntrl) |
|  | [`c32iscntrl`](#index-c32iscntrl-1) | [iswcntrl](#iswcntrl) |
|  | [`c32iscntrl`](#index-c32iscntrl-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32isdigit`](#index-c32isdigit) | [isdigit](#isdigit) |
|  | [`c32isdigit`](#index-c32isdigit-1) | [iswdigit](#iswdigit) |
|  | [`c32isdigit`](#index-c32isdigit-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32isgraph`](#index-c32isgraph) | [isgraph](#isgraph) |
|  | [`c32isgraph`](#index-c32isgraph-1) | [iswgraph](#iswgraph) |
|  | [`c32isgraph`](#index-c32isgraph-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32islower`](#index-c32islower) | [islower](#islower) |
|  | [`c32islower`](#index-c32islower-1) | [iswlower](#iswlower) |
|  | [`c32islower`](#index-c32islower-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32isprint`](#index-c32isprint) | [isprint](#isprint) |
|  | [`c32isprint`](#index-c32isprint-1) | [iswprint](#iswprint) |
|  | [`c32isprint`](#index-c32isprint-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32ispunct`](#index-c32ispunct) | [ispunct](#ispunct) |
|  | [`c32ispunct`](#index-c32ispunct-1) | [iswpunct](#iswpunct) |
|  | [`c32ispunct`](#index-c32ispunct-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32isspace`](#index-c32isspace) | [isspace](#isspace) |
|  | [`c32isspace`](#index-c32isspace-1) | [iswspace](#iswspace) |
|  | [`c32isspace`](#index-c32isspace-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32isupper`](#index-c32isupper) | [isupper](#isupper) |
|  | [`c32isupper`](#index-c32isupper-1) | [iswupper](#iswupper) |
|  | [`c32isupper`](#index-c32isupper-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32isxdigit`](#index-c32isxdigit) | [isxdigit](#isxdigit) |
|  | [`c32isxdigit`](#index-c32isxdigit-1) | [iswxdigit](#iswxdigit) |
|  | [`c32isxdigit`](#index-c32isxdigit-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32rtomb`](#index-c32rtomb-1) | [c32rtomb](#c32rtomb) |
|  | [`c32rtomb`](#index-c32rtomb-2) | [wcrtomb](#wcrtomb) |
|  | [`c32snrtombs`](#index-c32snrtombs) | [wcsnrtombs](#wcsnrtombs) |
|  | [`c32snrtombs`](#index-c32snrtombs-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`c32srtombs`](#index-c32srtombs) | [wcsrtombs](#wcsrtombs) |
|  | [`c32srtombs`](#index-c32srtombs-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`c32stombs`](#index-c32stombs) | [wcstombs](#wcstombs) |
|  | [`c32stombs`](#index-c32stombs-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`c32swidth`](#index-c32swidth) | [wcswidth](#wcswidth) |
|  | [`c32swidth`](#index-c32swidth-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`c32tob`](#index-c32tob) | [wctob](#wctob) |
|  | [`c32tolower`](#index-c32tolower) | [tolower](#tolower) |
|  | [`c32tolower`](#index-c32tolower-1) | [towlower](#towlower) |
|  | [`c32tolower`](#index-c32tolower-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32tolower`](#index-c32tolower-3) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32toupper`](#index-c32toupper) | [toupper](#toupper) |
|  | [`c32toupper`](#index-c32toupper-1) | [towupper](#towupper) |
|  | [`c32toupper`](#index-c32toupper-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32toupper`](#index-c32toupper-3) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`c32width`](#index-c32width) | [wcwidth](#wcwidth) |
|  | [`c32width`](#index-c32width-1) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`call_once`](#index-call_005fonce-1) | [stdlib.h](#stdlib_002eh) |
|  | [`call_once`](#index-call_005fonce-2) | [call\_once](#call_005fonce) |
|  | [`calloc-gnu`](#index-calloc_002dgnu) | [calloc](#calloc) |
|  | [`calloc-posix`](#index-calloc_002dposix) | [calloc](#calloc) |
|  | [`canonicalize-lgpl`](#index-canonicalize_002dlgpl) | [realpath](#realpath) |
|  | [`canonicalize-lgpl`](#index-canonicalize_002dlgpl-1) | [canonicalize\_file\_name](#canonicalize_005ffile_005fname) |
|  | [`carray-list`](#index-carray_002dlist) | [Ordinary containers](#Ordinary-containers) |
|  | [`cbrt`](#index-cbrt-1) | [cbrt](#cbrt) |
|  | [`cbrt-ieee`](#index-cbrt_002dieee) | [cbrt](#cbrt) |
|  | [`cbrtf`](#index-cbrtf-1) | [cbrtf](#cbrtf) |
|  | [`cbrtf-ieee`](#index-cbrtf_002dieee) | [cbrtf](#cbrtf) |
|  | [`cbrtl`](#index-cbrtl-1) | [cbrtl](#cbrtl) |
|  | [`cbrtl-ieee`](#index-cbrtl_002dieee) | [cbrtl](#cbrtl) |
|  | [`ceil`](#index-ceil-1) | [ceil](#ceil) |
|  | [`ceil-ieee`](#index-ceil_002dieee) | [ceil](#ceil) |
|  | [`ceilf`](#index-ceilf-1) | [ceilf](#ceilf) |
|  | [`ceilf-ieee`](#index-ceilf_002dieee) | [ceilf](#ceilf) |
|  | [`ceill`](#index-ceill-1) | [ceill](#ceill) |
|  | [`ceill-ieee`](#index-ceill_002dieee) | [ceill](#ceill) |
|  | [`chdir`](#index-chdir-1) | [chdir](#chdir) |
|  | [`check-version`](#index-check_002dversion) | [Library version handling](#Library-version-handling) |
|  | [`chmod`](#index-chmod-1) | [chmod](#chmod) |
|  | [`chown`](#index-chown-1) | [chown](#chown) |
|  | [`clean-temp`](#index-clean_002dtemp) | [mkostemp](#mkostemp) |
|  | [`clean-temp`](#index-clean_002dtemp-1) | [mkstemp](#mkstemp) |
|  | [`clean-temp`](#index-clean_002dtemp-2) | [mkostemps](#mkostemps) |
|  | [`clean-temp`](#index-clean_002dtemp-3) | [mkstemps](#mkstemps) |
|  | [`close`](#index-close-1) | [close](#close) |
|  | [`closedir`](#index-closedir-1) | [closedir](#closedir) |
|  | [`cnd`](#index-cnd) | [cnd\_broadcast](#cnd_005fbroadcast) |
|  | [`cnd`](#index-cnd-1) | [cnd\_destroy](#cnd_005fdestroy) |
|  | [`cnd`](#index-cnd-2) | [cnd\_init](#cnd_005finit) |
|  | [`cnd`](#index-cnd-3) | [cnd\_signal](#cnd_005fsignal) |
|  | [`cnd`](#index-cnd-4) | [cnd\_timedwait](#cnd_005ftimedwait) |
|  | [`cnd`](#index-cnd-5) | [cnd\_wait](#cnd_005fwait) |
|  | [`cnd`](#index-cnd-6) | [ISO C multithreading](#ISO-C-multithreading) |
|  | [`cond`](#index-cond) | [Gnulib multithreading](#Gnulib-multithreading) |
|  | [`configmake`](#index-configmake) | [configmake](#configmake) |
|  | [`connect`](#index-connect-1) | [connect](#connect) |
|  | [`copy-file-range`](#index-copy_002dfile_002drange) | [copy\_file\_range](#copy_005ffile_005frange) |
|  | [`copysign`](#index-copysign-1) | [copysign](#copysign) |
|  | [`copysignf`](#index-copysignf-1) | [copysignf](#copysignf) |
|  | [`copysignl`](#index-copysignl-1) | [copysignl](#copysignl) |
|  | [`cos`](#index-cos-1) | [cos](#cos) |
|  | [`cosf`](#index-cosf-1) | [cosf](#cosf) |
|  | [`cosh`](#index-cosh-1) | [cosh](#cosh) |
|  | [`coshf`](#index-coshf-1) | [coshf](#coshf) |
|  | [`cosl`](#index-cosl-1) | [cosl](#cosl) |
|  | [`creat`](#index-creat-1) | [creat](#creat) |
|  | [`crypto/gc-random`](#index-crypto_002fgc_002drandom) | [getentropy](#getentropy) |
|  | [`crypto/gc-random`](#index-crypto_002fgc_002drandom-1) | [getrandom](#getrandom) |
|  | [`ctime`](#index-ctime-1) | [ctime](#ctime) |
|  | [`ctype`](#index-ctype) | [isalnum](#isalnum) |
|  | [`ctype`](#index-ctype-1) | [isalpha](#isalpha) |
|  | [`ctype`](#index-ctype-2) | [iscntrl](#iscntrl) |
|  | [`ctype`](#index-ctype-3) | [isdigit](#isdigit) |
|  | [`ctype`](#index-ctype-4) | [isgraph](#isgraph) |
|  | [`ctype`](#index-ctype-5) | [islower](#islower) |
|  | [`ctype`](#index-ctype-6) | [isprint](#isprint) |
|  | [`ctype`](#index-ctype-7) | [ispunct](#ispunct) |
|  | [`ctype`](#index-ctype-8) | [isspace](#isspace) |
|  | [`ctype`](#index-ctype-9) | [isupper](#isupper) |
|  | [`ctype`](#index-ctype-10) | [isxdigit](#isxdigit) |
|  | [`ctype`](#index-ctype-11) | [tolower](#tolower) |
|  | [`ctype`](#index-ctype-12) | [toupper](#toupper) |
|  | [`ctype`](#index-ctype-13) | [toascii](#toascii) |
|  | [`ctype-h`](#index-ctype_002dh) | [ctype.h](#ctype_002eh) |
| --- | | |
| D |
|  | [`dirent-h`](#index-dirent_002dh) | [dirent.h](#dirent_002eh) |
|  | [`dirent-safer`](#index-dirent_002dsafer) | [Closed standard fds](#Closed-standard-fds) |
|  | [`dirfd`](#index-dirfd-1) | [dirfd](#dirfd) |
|  | [`dirname`](#index-dirname-1) | [libgen.h](#libgen_002eh) |
|  | [`dirname`](#index-dirname-2) | [dirname](#dirname) |
|  | [`do-release-commit-and-tag`](#index-do_002drelease_002dcommit_002dand_002dtag) | [maintainer-makefile](#maintainer_002dmakefile) |
|  | [`dprintf`](#index-dprintf-1) | [dprintf](#dprintf) |
|  | [`dprintf-gnu`](#index-dprintf_002dgnu) | [dprintf](#dprintf) |
|  | [`dprintf-posix`](#index-dprintf_002dposix) | [dprintf](#dprintf) |
|  | [`dup`](#index-dup-1) | [dup](#dup) |
|  | [`dup2`](#index-dup2-1) | [dup2](#dup2) |
|  | [`dup3`](#index-dup3-1) | [dup3](#dup3) |
|  | [`duplocale`](#index-duplocale-1) | [duplocale](#duplocale) |
|  | [`dzprintf`](#index-dzprintf) | [Modernized printf](#Modernized-printf) |
|  | [`dzprintf-gnu`](#index-dzprintf_002dgnu) | [Modernized printf](#Modernized-printf) |
|  | [`dzprintf-posix`](#index-dzprintf_002dposix) | [Modernized printf](#Modernized-printf) |
| --- | | |
| E |
|  | [`endian`](#index-endian) | [endian.h](#endian_002eh) |
|  | [`endian`](#index-endian-1) | [be16toh](#be16toh) |
|  | [`endian`](#index-endian-2) | [be32toh](#be32toh) |
|  | [`endian`](#index-endian-3) | [be64toh](#be64toh) |
|  | [`endian`](#index-endian-4) | [htobe16](#htobe16) |
|  | [`endian`](#index-endian-5) | [htobe32](#htobe32) |
|  | [`endian`](#index-endian-6) | [htobe64](#htobe64) |
|  | [`endian`](#index-endian-7) | [htole16](#htole16) |
|  | [`endian`](#index-endian-8) | [htole32](#htole32) |
|  | [`endian`](#index-endian-9) | [htole64](#htole64) |
|  | [`endian`](#index-endian-10) | [le16toh](#le16toh) |
|  | [`endian`](#index-endian-11) | [le32toh](#le32toh) |
|  | [`endian`](#index-endian-12) | [le64toh](#le64toh) |
|  | [`environ`](#index-environ-1) | [environ](#environ) |
|  | [`erf`](#index-erf-1) | [erf](#erf) |
|  | [`erfc`](#index-erfc-1) | [erfc](#erfc) |
|  | [`errno-h`](#index-errno_002dh) | [errno.h](#errno_002eh) |
|  | [`error`](#index-error-2) | [error](#error) |
|  | [`error`](#index-error-3) | [error\_at\_line](#error_005fat_005fline) |
|  | [`error`](#index-error-4) | [error\_message\_count](#error_005fmessage_005fcount) |
|  | [`error`](#index-error-5) | [error\_one\_per\_line](#error_005fone_005fper_005fline) |
|  | [`error`](#index-error-6) | [error\_print\_progname](#error_005fprint_005fprogname) |
|  | [`error-h`](#index-error_002dh) | [error.h](#error_002eh) |
|  | [`euidaccess`](#index-euidaccess-1) | [euidaccess](#euidaccess) |
|  | [`execinfo`](#index-execinfo) | [execinfo.h](#execinfo_002eh) |
|  | [`execinfo`](#index-execinfo-1) | [backtrace](#backtrace) |
|  | [`execinfo`](#index-execinfo-2) | [backtrace\_symbols](#backtrace_005fsymbols) |
|  | [`execinfo`](#index-execinfo-3) | [backtrace\_symbols\_fd](#backtrace_005fsymbols_005ffd) |
|  | [`execl`](#index-execl-1) | [execl](#execl) |
|  | [`execle`](#index-execle-1) | [execle](#execle) |
|  | [`execlp`](#index-execlp-1) | [execlp](#execlp) |
|  | [`execv`](#index-execv-1) | [execv](#execv) |
|  | [`execve`](#index-execve-1) | [execve](#execve) |
|  | [`execvp`](#index-execvp-1) | [execvp](#execvp) |
|  | [`execvpe`](#index-execvpe-1) | [execvpe](#execvpe) |
|  | [`exp`](#index-exp-1) | [exp](#exp) |
|  | [`exp-ieee`](#index-exp_002dieee) | [exp](#exp) |
|  | [`exp2`](#index-exp2-1) | [exp2](#exp2) |
|  | [`exp2-ieee`](#index-exp2_002dieee) | [exp2](#exp2) |
|  | [`exp2f`](#index-exp2f-1) | [exp2f](#exp2f) |
|  | [`exp2f-ieee`](#index-exp2f_002dieee) | [exp2f](#exp2f) |
|  | [`exp2l`](#index-exp2l-1) | [exp2l](#exp2l) |
|  | [`exp2l-ieee`](#index-exp2l_002dieee) | [exp2l](#exp2l) |
|  | [`expf`](#index-expf-1) | [expf](#expf) |
|  | [`expf-ieee`](#index-expf_002dieee) | [expf](#expf) |
|  | [`expl`](#index-expl-1) | [expl](#expl) |
|  | [`expl-ieee`](#index-expl_002dieee) | [expl](#expl) |
|  | [`explicit_bzero`](#index-explicit_005fbzero-1) | [explicit\_bzero](#explicit_005fbzero) |
|  | [`expm1`](#index-expm1-1) | [expm1](#expm1) |
|  | [`expm1-ieee`](#index-expm1_002dieee) | [expm1](#expm1) |
|  | [`expm1f`](#index-expm1f-1) | [expm1f](#expm1f) |
|  | [`expm1f-ieee`](#index-expm1f_002dieee) | [expm1f](#expm1f) |
|  | [`expm1l`](#index-expm1l-1) | [expm1l](#expm1l) |
|  | [`expm1l-ieee`](#index-expm1l_002dieee) | [expm1l](#expm1l) |
|  | [`extensions`](#index-extensions) | [readdir\_r](#readdir_005fr) |
|  | [`extensions`](#index-extensions-1) | [getgrgid\_r](#getgrgid_005fr) |
|  | [`extensions`](#index-extensions-2) | [getgrnam\_r](#getgrnam_005fr) |
|  | [`extensions`](#index-extensions-3) | [getpwnam\_r](#getpwnam_005fr) |
|  | [`extensions`](#index-extensions-4) | [getpwuid\_r](#getpwuid_005fr) |
|  | [`extensions`](#index-extensions-5) | [sigwait](#sigwait) |
|  | [`extensions`](#index-extensions-6) | [asctime\_r](#asctime_005fr) |
|  | [`extensions`](#index-extensions-7) | [ctime\_r](#ctime_005fr) |
|  | [`extern-inline`](#index-extern_002dinline) | [extern inline](#extern-inline) |
| --- | | |
| F |
|  | [`fabs`](#index-fabs-1) | [fabs](#fabs) |
|  | [`fabs-ieee`](#index-fabs_002dieee) | [fabs](#fabs) |
|  | [`fabsf`](#index-fabsf-1) | [fabsf](#fabsf) |
|  | [`fabsf-ieee`](#index-fabsf_002dieee) | [fabsf](#fabsf) |
|  | [`fabsl`](#index-fabsl-1) | [fabsl](#fabsl) |
|  | [`fabsl-ieee`](#index-fabsl_002dieee) | [fabsl](#fabsl) |
|  | [`faccessat`](#index-faccessat-1) | [faccessat](#faccessat) |
|  | [`fbufmode`](#index-fbufmode) | [Accessing the write buffer](#Accessing-the-write-buffer) |
|  | [`fchdir`](#index-fchdir-1) | [open](#open) |
|  | [`fchdir`](#index-fchdir-2) | [fchdir](#fchdir) |
|  | [`fchmodat`](#index-fchmodat-1) | [fchmodat](#fchmodat) |
|  | [`fchownat`](#index-fchownat-1) | [fchownat](#fchownat) |
|  | [`fclose`](#index-fclose-1) | [fclose](#fclose) |
|  | [`fcntl`](#index-fcntl-1) | [fcntl](#fcntl) |
|  | [`fcntl-h`](#index-fcntl_002dh) | [fcntl.h](#fcntl_002eh) |
|  | [`fcntl-safer`](#index-fcntl_002dsafer) | [Closed standard fds](#Closed-standard-fds) |
|  | [`fdatasync`](#index-fdatasync-1) | [fdatasync](#fdatasync) |
|  | [`fdopen`](#index-fdopen-1) | [fdopen](#fdopen) |
|  | [`fdopendir`](#index-fdopendir-1) | [fdopendir](#fdopendir) |
|  | [`fdutimensat`](#index-fdutimensat) | [utimensat](#utimensat) |
|  | [`fdutimensat`](#index-fdutimensat-1) | [futimesat](#futimesat) |
|  | [`fenv-environment`](#index-fenv_002denvironment) | [fegetenv](#fegetenv) |
|  | [`fenv-environment`](#index-fenv_002denvironment-1) | [feholdexcept](#feholdexcept) |
|  | [`fenv-environment`](#index-fenv_002denvironment-2) | [fesetenv](#fesetenv) |
|  | [`fenv-environment`](#index-fenv_002denvironment-3) | [feupdateenv](#feupdateenv) |
|  | [`fenv-exceptions-state-c23`](#index-fenv_002dexceptions_002dstate_002dc23) | [fetestexceptflag](#fetestexceptflag) |
|  | [`fenv-exceptions-state-c99`](#index-fenv_002dexceptions_002dstate_002dc99) | [fegetexceptflag](#fegetexceptflag) |
|  | [`fenv-exceptions-state-c99`](#index-fenv_002dexceptions_002dstate_002dc99-1) | [fesetexceptflag](#fesetexceptflag) |
|  | [`fenv-exceptions-tracking-c23`](#index-fenv_002dexceptions_002dtracking_002dc23) | [fesetexcept](#fesetexcept) |
|  | [`fenv-exceptions-tracking-c99`](#index-fenv_002dexceptions_002dtracking_002dc99) | [feclearexcept](#feclearexcept) |
|  | [`fenv-exceptions-tracking-c99`](#index-fenv_002dexceptions_002dtracking_002dc99-1) | [feraiseexcept](#feraiseexcept) |
|  | [`fenv-exceptions-tracking-c99`](#index-fenv_002dexceptions_002dtracking_002dc99-2) | [fetestexcept](#fetestexcept) |
|  | [`fenv-exceptions-trapping`](#index-fenv_002dexceptions_002dtrapping) | [fedisableexcept](#fedisableexcept) |
|  | [`fenv-exceptions-trapping`](#index-fenv_002dexceptions_002dtrapping-1) | [feenableexcept](#feenableexcept) |
|  | [`fenv-exceptions-trapping`](#index-fenv_002dexceptions_002dtrapping-2) | [fegetexcept](#fegetexcept) |
|  | [`fenv-h`](#index-fenv_002dh) | [fenv.h](#fenv_002eh) |
|  | [`fenv-rounding`](#index-fenv_002drounding) | [fegetround](#fegetround) |
|  | [`fenv-rounding`](#index-fenv_002drounding-1) | [fesetround](#fesetround) |
|  | [`fflush`](#index-fflush-1) | [fflush](#fflush) |
|  | [`ffs`](#index-ffs-1) | [ffs](#ffs) |
|  | [`ffsl`](#index-ffsl-1) | [ffsl](#ffsl) |
|  | [`ffsll`](#index-ffsll-1) | [ffsll](#ffsll) |
|  | [`flexmember`](#index-flexmember) | [C99 features assumed](#C99-features-assumed) |
|  | [`float-h`](#index-float_002dh) | [float.h](#float_002eh) |
|  | [`flock`](#index-flock-1) | [flock](#flock) |
|  | [`floor`](#index-floor-1) | [floor](#floor) |
|  | [`floor-ieee`](#index-floor_002dieee) | [floor](#floor) |
|  | [`floorf`](#index-floorf-1) | [floorf](#floorf) |
|  | [`floorf-ieee`](#index-floorf_002dieee) | [floorf](#floorf) |
|  | [`floorl`](#index-floorl-1) | [floorl](#floorl) |
|  | [`floorl-ieee`](#index-floorl_002dieee) | [floorl](#floorl) |
|  | [`fma`](#index-fma-1) | [fma](#fma) |
|  | [`fma-ieee`](#index-fma_002dieee) | [fma](#fma) |
|  | [`fmaf`](#index-fmaf-1) | [fmaf](#fmaf) |
|  | [`fmaf-ieee`](#index-fmaf_002dieee) | [fmaf](#fmaf) |
|  | [`fmal`](#index-fmal-1) | [fmal](#fmal) |
|  | [`fmal-ieee`](#index-fmal_002dieee) | [fmal](#fmal) |
|  | [`fmod`](#index-fmod-1) | [fmod](#fmod) |
|  | [`fmod-ieee`](#index-fmod_002dieee) | [fmod](#fmod) |
|  | [`fmodf`](#index-fmodf-1) | [fmodf](#fmodf) |
|  | [`fmodf-ieee`](#index-fmodf_002dieee) | [fmodf](#fmodf) |
|  | [`fmodl`](#index-fmodl-1) | [fmodl](#fmodl) |
|  | [`fmodl-ieee`](#index-fmodl_002dieee) | [fmodl](#fmodl) |
|  | [`fnmatch`](#index-fnmatch-1) | [fnmatch](#fnmatch) |
|  | [`fnmatch-gnu`](#index-fnmatch_002dgnu) | [fnmatch.h](#fnmatch_002eh) |
|  | [`fnmatch-gnu`](#index-fnmatch_002dgnu-1) | [fnmatch](#fnmatch) |
|  | [`fnmatch-h`](#index-fnmatch_002dh) | [fnmatch.h](#fnmatch_002eh) |
|  | [`fopen`](#index-fopen-1) | [fopen](#fopen) |
|  | [`fopen-gnu`](#index-fopen_002dgnu) | [fopen](#fopen) |
|  | [`fopen-safer`](#index-fopen_002dsafer) | [Closed standard fds](#Closed-standard-fds) |
|  | [`forkpty`](#index-forkpty-1) | [forkpty](#forkpty) |
|  | [`fpending`](#index-fpending) | [Accessing the write buffer](#Accessing-the-write-buffer) |
|  | [`fprintf-gnu`](#index-fprintf_002dgnu) | [fprintf](#fprintf) |
|  | [`fprintf-posix`](#index-fprintf_002dposix) | [fprintf](#fprintf) |
|  | [`fpurge`](#index-fpurge) | [Dropping the buffer contents](#Dropping-the-buffer-contents) |
|  | [`freadable`](#index-freadable) | [State of a stream](#State-of-a-stream) |
|  | [`freadahead`](#index-freadahead) | [Accessing the read buffer](#Accessing-the-read-buffer) |
|  | [`freading`](#index-freading) | [State of a stream](#State-of-a-stream) |
|  | [`freadptr`](#index-freadptr) | [Accessing the read buffer](#Accessing-the-read-buffer) |
|  | [`freadseek`](#index-freadseek) | [Accessing the read buffer](#Accessing-the-read-buffer) |
|  | [`free-posix`](#index-free_002dposix) | [free](#free) |
|  | [`freelocale`](#index-freelocale-1) | [freelocale](#freelocale) |
|  | [`freopen`](#index-freopen-1) | [freopen](#freopen) |
|  | [`freopen-safer`](#index-freopen_002dsafer) | [freopen](#freopen) |
|  | [`freopen-safer`](#index-freopen_002dsafer-1) | [Closed standard fds](#Closed-standard-fds) |
|  | [`frexp`](#index-frexp-1) | [frexp](#frexp) |
|  | [`frexp-ieee`](#index-frexp_002dieee) | [frexp](#frexp) |
|  | [`frexpf`](#index-frexpf-1) | [frexpf](#frexpf) |
|  | [`frexpf-ieee`](#index-frexpf_002dieee) | [frexpf](#frexpf) |
|  | [`frexpl`](#index-frexpl-1) | [frexpl](#frexpl) |
|  | [`frexpl-ieee`](#index-frexpl_002dieee) | [frexpl](#frexpl) |
|  | [`fseek`](#index-fseek-1) | [fseek](#fseek) |
|  | [`fseeko`](#index-fseeko-1) | [fseeko](#fseeko) |
|  | [`fseterr`](#index-fseterr) | [Error in a stream](#Error-in-a-stream) |
|  | [`fstat`](#index-fstat-1) | [fstat](#fstat) |
|  | [`fstatat`](#index-fstatat-1) | [fstatat](#fstatat) |
|  | [`fsusage`](#index-fsusage) | [statvfs](#statvfs) |
|  | [`fsync`](#index-fsync-1) | [fsync](#fsync) |
|  | [`ftell`](#index-ftell-1) | [ftell](#ftell) |
|  | [`ftello`](#index-ftello-1) | [ftello](#ftello) |
|  | [`ftello`](#index-ftello-2) | [stderr](#stderr) |
|  | [`ftello`](#index-ftello-3) | [stdout](#stdout) |
|  | [`ftruncate`](#index-ftruncate-1) | [ftruncate](#ftruncate) |
|  | [`fts`](#index-fts) | [ftw](#ftw) |
|  | [`fts`](#index-fts-1) | [fts.h](#fts_002eh) |
|  | [`fts`](#index-fts-2) | [fts\_children](#fts_005fchildren) |
|  | [`fts`](#index-fts-3) | [fts\_close](#fts_005fclose) |
|  | [`fts`](#index-fts-4) | [fts\_open](#fts_005fopen) |
|  | [`fts`](#index-fts-5) | [fts\_read](#fts_005fread) |
|  | [`fts`](#index-fts-6) | [fts\_set](#fts_005fset) |
|  | [`func`](#index-func) | [C99 features assumed](#C99-features-assumed) |
|  | [`func`](#index-func-1) | [func](#func) |
|  | [`futimens`](#index-futimens-1) | [futimens](#futimens) |
|  | [`fwritable`](#index-fwritable) | [State of a stream](#State-of-a-stream) |
|  | [`fwriting`](#index-fwriting) | [State of a stream](#State-of-a-stream) |
|  | [`fzprintf`](#index-fzprintf) | [Modernized printf](#Modernized-printf) |
|  | [`fzprintf-gnu`](#index-fzprintf_002dgnu) | [Modernized printf](#Modernized-printf) |
|  | [`fzprintf-posix`](#index-fzprintf_002dposix) | [Modernized printf](#Modernized-printf) |
| --- | | |
| G |
|  | [`gcd`](#index-gcd-1) | [gcd](#gcd) |
|  | [`getaddrinfo`](#index-getaddrinfo-1) | [freeaddrinfo](#freeaddrinfo) |
|  | [`getaddrinfo`](#index-getaddrinfo-2) | [gai\_strerror](#gai_005fstrerror) |
|  | [`getaddrinfo`](#index-getaddrinfo-3) | [getaddrinfo](#getaddrinfo) |
|  | [`getaddrinfo`](#index-getaddrinfo-4) | [getnameinfo](#getnameinfo) |
|  | [`getaddrinfo`](#index-getaddrinfo-5) | [Windows sockets](#Windows-sockets) |
|  | [`getcwd`](#index-getcwd-1) | [getcwd](#getcwd) |
|  | [`getcwd-lgpl`](#index-getcwd_002dlgpl) | [getcwd](#getcwd) |
|  | [`getdelim`](#index-getdelim-1) | [getdelim](#getdelim) |
|  | [`getdomainname`](#index-getdomainname-1) | [getdomainname](#getdomainname) |
|  | [`getdtablesize`](#index-getdtablesize-1) | [getdtablesize](#getdtablesize) |
|  | [`getentropy`](#index-getentropy-1) | [getentropy](#getentropy) |
|  | [`getentropy`](#index-getentropy-2) | [getrandom](#getrandom) |
|  | [`getgroups`](#index-getgroups-1) | [getgroups](#getgroups) |
|  | [`gethostname`](#index-gethostname-1) | [limits.h](#limits_002eh) |
|  | [`gethostname`](#index-gethostname-2) | [gethostname](#gethostname) |
|  | [`getline`](#index-getline-1) | [getline](#getline) |
|  | [`getloadavg`](#index-getloadavg-1) | [getloadavg](#getloadavg) |
|  | [`getlocalename_l-simple`](#index-getlocalename_005fl_002dsimple) | [getlocalename\_l](#getlocalename_005fl) |
|  | [`getlogin`](#index-getlogin-1) | [getlogin](#getlogin) |
|  | [`getlogin_r`](#index-getlogin_005fr-1) | [getlogin\_r](#getlogin_005fr) |
|  | [`getopt-gnu`](#index-getopt_002dgnu) | [getopt](#getopt) |
|  | [`getopt-gnu`](#index-getopt_002dgnu-1) | [getopt.h](#getopt_002eh) |
|  | [`getopt-gnu`](#index-getopt_002dgnu-2) | [getopt\_long](#getopt_005flong) |
|  | [`getopt-gnu`](#index-getopt_002dgnu-3) | [getopt\_long\_only](#getopt_005flong_005fonly) |
|  | [`getopt-posix`](#index-getopt_002dposix) | [getopt](#getopt) |
|  | [`getpagesize`](#index-getpagesize-1) | [getpagesize](#getpagesize) |
|  | [`getpass`](#index-getpass-1) | [getpass](#getpass) |
|  | [`getpass-gnu`](#index-getpass_002dgnu) | [getpass](#getpass) |
|  | [`getpayload`](#index-getpayload-1) | [getpayload](#getpayload) |
|  | [`getpayloadf`](#index-getpayloadf-1) | [getpayloadf](#getpayloadf) |
|  | [`getpayloadl`](#index-getpayloadl-1) | [getpayloadl](#getpayloadl) |
|  | [`getpeername`](#index-getpeername-1) | [getpeername](#getpeername) |
|  | [`getprogname`](#index-getprogname-1) | [progname and getprogname](#progname-and-getprogname) |
|  | [`getrandom`](#index-getrandom-1) | [getentropy](#getentropy) |
|  | [`getrandom`](#index-getrandom-2) | [getrandom](#getrandom) |
|  | [`getrusage`](#index-getrusage-1) | [getrusage](#getrusage) |
|  | [`getsockname`](#index-getsockname-1) | [getsockname](#getsockname) |
|  | [`getsockopt`](#index-getsockopt-1) | [getsockopt](#getsockopt) |
|  | [`getsubopt`](#index-getsubopt-1) | [getsubopt](#getsubopt) |
|  | [`gettext`](#index-gettext-1) | [gettextize and autopoint](#gettextize-and-autopoint) |
|  | [`gettext`](#index-gettext-2) | [libintl.h](#libintl_002eh) |
|  | [`gettext-h`](#index-gettext_002dh) | [gettextize and autopoint](#gettextize-and-autopoint) |
|  | [`gettime`](#index-gettime) | [clock\_gettime](#clock_005fgettime) |
|  | [`gettime`](#index-gettime-1) | [gettimeofday](#gettimeofday) |
|  | [`gettime-res`](#index-gettime_002dres) | [clock\_getres](#clock_005fgetres) |
|  | [`gettime-res`](#index-gettime_002dres-1) | [timespec\_getres](#timespec_005fgetres) |
|  | [`gettimeofday`](#index-gettimeofday-1) | [gettimeofday](#gettimeofday) |
|  | [`getugroups`](#index-getugroups) | [getgrouplist](#getgrouplist) |
|  | [`getumask`](#index-getumask-1) | [getumask](#getumask) |
|  | [`getusershell`](#index-getusershell-1) | [endusershell](#endusershell) |
|  | [`getusershell`](#index-getusershell-2) | [getusershell](#getusershell) |
|  | [`getusershell`](#index-getusershell-3) | [setusershell](#setusershell) |
|  | [`git-version-gen`](#index-git_002dversion_002dgen) | [Setting the package version](#Setting-the-package-version) |
|  | [`gitlog-to-changelog`](#index-gitlog_002dto_002dchangelog) | [gitlog-to-changelog](#gitlog_002dto_002dchangelog) |
|  | [`glob`](#index-glob-1) | [glob](#glob) |
|  | [`glob`](#index-glob-2) | [globfree](#globfree) |
|  | [`glob`](#index-glob-3) | [glob\_pattern\_p](#glob_005fpattern_005fp) |
|  | [`glob-h`](#index-glob_002dh) | [glob.h](#glob_002eh) |
|  | [`gnumakefile`](#index-gnumakefile) | [Setting the package version](#Setting-the-package-version) |
|  | [`gnumakefile`](#index-gnumakefile-1) | [maintainer-makefile](#maintainer_002dmakefile) |
|  | [`gnupload`](#index-gnupload) | [For uploading releases](#For-uploading-releases) |
|  | [`gpl-2.0`](#index-gpl_002d2_002e0) | [License Texinfo sources](#License-Texinfo-sources) |
|  | [`gpl-3.0`](#index-gpl_002d3_002e0) | [License Texinfo sources](#License-Texinfo-sources) |
|  | [`grantpt`](#index-grantpt-1) | [grantpt](#grantpt) |
|  | [`group-member`](#index-group_002dmember) | [group\_member](#group_005fmember) |
| --- | | |
| H |
|  | [`hamt`](#index-hamt) | [Specialized containers](#Specialized-containers) |
|  | [`hash-map`](#index-hash_002dmap) | [Ordinary containers](#Ordinary-containers) |
|  | [`hash-set`](#index-hash_002dset) | [Ordinary containers](#Ordinary-containers) |
|  | [`hasmntopt`](#index-hasmntopt-1) | [hasmntopt](#hasmntopt) |
|  | [`havelib`](#index-havelib) | [Searching for Libraries](#Searching-for-Libraries) |
|  | [`htonl`](#index-htonl-1) | [htonl](#htonl) |
|  | [`htonl`](#index-htonl-2) | [htons](#htons) |
|  | [`htonl`](#index-htonl-3) | [ntohl](#ntohl) |
|  | [`htonl`](#index-htonl-4) | [ntohs](#ntohs) |
|  | [`hypot`](#index-hypot-1) | [hypot](#hypot) |
|  | [`hypot-ieee`](#index-hypot_002dieee) | [hypot](#hypot) |
|  | [`hypotf`](#index-hypotf-1) | [hypotf](#hypotf) |
|  | [`hypotf-ieee`](#index-hypotf_002dieee) | [hypotf](#hypotf) |
|  | [`hypotl`](#index-hypotl-1) | [hypotl](#hypotl) |
|  | [`hypotl-ieee`](#index-hypotl_002dieee) | [hypotl](#hypotl) |
| --- | | |
| I |
|  | [`iconv`](#index-iconv-1) | [Various Kinds of Modules](#Various-Kinds-of-Modules) |
|  | [`iconv`](#index-iconv-2) | [iconv.h](#iconv_002eh) |
|  | [`iconv`](#index-iconv-3) | [iconv](#iconv) |
|  | [`iconv`](#index-iconv-4) | [iconv\_close](#iconv_005fclose) |
|  | [`iconv`](#index-iconv-5) | [iconv\_open](#iconv_005fopen) |
|  | [`iconv_open`](#index-iconv_005fopen-1) | [iconv\_open](#iconv_005fopen) |
|  | [`iconv_open-utf`](#index-iconv_005fopen_002dutf) | [iconv\_open](#iconv_005fopen) |
|  | [`ieee754-h`](#index-ieee754_002dh) | [ieee754.h](#ieee754_002eh) |
|  | [`ilogb`](#index-ilogb-1) | [ilogb](#ilogb) |
|  | [`ilogbf`](#index-ilogbf-1) | [ilogbf](#ilogbf) |
|  | [`ilogbl`](#index-ilogbl-1) | [ilogbl](#ilogbl) |
|  | [`imaxabs`](#index-imaxabs-1) | [imaxabs](#imaxabs) |
|  | [`imaxdiv`](#index-imaxdiv-1) | [imaxdiv](#imaxdiv) |
|  | [`inet_ntop`](#index-inet_005fntop-1) | [inet\_ntop](#inet_005fntop) |
|  | [`inet_pton`](#index-inet_005fpton-1) | [inet\_pton](#inet_005fpton) |
|  | [`inline`](#index-inline-2) | [static inline](#static-inline) |
|  | [`intprops`](#index-intprops) | [Integer Properties](#Integer-Properties) |
|  | [`inttypes-h`](#index-inttypes_002dh) | [inttypes.h](#inttypes_002eh) |
|  | [`ioctl`](#index-ioctl-1) | [ioctl](#ioctl) |
|  | [`isalnum_l`](#index-isalnum_005fl-1) | [isalnum\_l](#isalnum_005fl) |
|  | [`isalpha_l`](#index-isalpha_005fl-1) | [isalpha\_l](#isalpha_005fl) |
|  | [`isatty`](#index-isatty-1) | [isatty](#isatty) |
|  | [`isblank`](#index-isblank-1) | [isblank](#isblank) |
|  | [`isblank`](#index-isblank-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`isblank_l`](#index-isblank_005fl-1) | [isblank\_l](#isblank_005fl) |
|  | [`iscntrl_l`](#index-iscntrl_005fl-1) | [iscntrl\_l](#iscntrl_005fl) |
|  | [`isdigit_l`](#index-isdigit_005fl-1) | [isdigit\_l](#isdigit_005fl) |
|  | [`isfinite`](#index-isfinite-1) | [isfinite](#isfinite) |
|  | [`isgraph_l`](#index-isgraph_005fl-1) | [isgraph\_l](#isgraph_005fl) |
|  | [`isinf`](#index-isinf-1) | [isinf](#isinf) |
|  | [`islower_l`](#index-islower_005fl-1) | [islower\_l](#islower_005fl) |
|  | [`isnan`](#index-isnan-1) | [isnan](#isnan) |
|  | [`isnanf`](#index-isnanf-1) | [isnanf](#isnanf) |
|  | [`isnanl`](#index-isnanl-1) | [isnanl](#isnanl) |
|  | [`isprint_l`](#index-isprint_005fl-1) | [isprint\_l](#isprint_005fl) |
|  | [`ispunct_l`](#index-ispunct_005fl-1) | [ispunct\_l](#ispunct_005fl) |
|  | [`isspace_l`](#index-isspace_005fl-1) | [isspace\_l](#isspace_005fl) |
|  | [`isupper_l`](#index-isupper_005fl-1) | [isupper\_l](#isupper_005fl) |
|  | [`iswblank`](#index-iswblank-1) | [iswblank](#iswblank) |
|  | [`iswblank`](#index-iswblank-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`iswctype`](#index-iswctype-1) | [iswctype](#iswctype) |
|  | [`iswctype`](#index-iswctype-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`iswdigit`](#index-iswdigit-1) | [iswdigit](#iswdigit) |
|  | [`iswdigit`](#index-iswdigit-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`iswpunct`](#index-iswpunct-1) | [iswpunct](#iswpunct) |
|  | [`iswpunct`](#index-iswpunct-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`iswxdigit`](#index-iswxdigit-1) | [iswxdigit](#iswxdigit) |
|  | [`iswxdigit`](#index-iswxdigit-2) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`isxdigit_l`](#index-isxdigit_005fl-1) | [isxdigit\_l](#isxdigit_005fl) |
| --- | | |
| J |
|  | [`j0`](#index-j0-1) | [j0](#j0) |
|  | [`j1`](#index-j1-1) | [j1](#j1) |
|  | [`jn`](#index-jn-1) | [jn](#jn) |
| --- | | |
| L |
|  | [`langinfo-h`](#index-langinfo_002dh) | [langinfo.h](#langinfo_002eh) |
|  | [`largefile`](#index-largefile) | [sys/types.h](#sys_002ftypes_002eh) |
|  | [`largefile`](#index-largefile-1) | [Avoiding the year 2038 problem](#Avoiding-the-year-2038-problem) |
|  | [`lchmod`](#index-lchmod-1) | [lchmod](#lchmod) |
|  | [`lchown`](#index-lchown-1) | [lchown](#lchown) |
|  | [`ldexp`](#index-ldexp-1) | [ldexp](#ldexp) |
|  | [`ldexp-ieee`](#index-ldexp_002dieee) | [ldexp](#ldexp) |
|  | [`ldexpf`](#index-ldexpf-1) | [ldexpf](#ldexpf) |
|  | [`ldexpf-ieee`](#index-ldexpf_002dieee) | [ldexpf](#ldexpf) |
|  | [`ldexpl`](#index-ldexpl-1) | [ldexpl](#ldexpl) |
|  | [`ldexpl-ieee`](#index-ldexpl_002dieee) | [ldexpl](#ldexpl) |
|  | [`lgamma`](#index-lgamma-1) | [lgamma](#lgamma) |
|  | [`lgpl-2.1`](#index-lgpl_002d2_002e1) | [License Texinfo sources](#License-Texinfo-sources) |
|  | [`lgpl-3.0`](#index-lgpl_002d3_002e0) | [License Texinfo sources](#License-Texinfo-sources) |
|  | [`lib-msvc-compat`](#index-lib_002dmsvc_002dcompat) | [Visual Studio Compatibility](#Visual-Studio-Compatibility) |
|  | [`lib-symbol-versions`](#index-lib_002dsymbol_002dversions) | [LD Version Scripts](#LD-Version-Scripts) |
|  | [`lib-symbol-visibility`](#index-lib_002dsymbol_002dvisibility) | [Exported Symbols of Shared Libraries](#Exported-Symbols-of-Shared-Libraries) |
|  | [`libunistring`](#index-libunistring) | [How to use libunistring](#How-to-use-libunistring) |
|  | [`libunistring-optional`](#index-libunistring_002doptional) | [How to use libunistring](#How-to-use-libunistring) |
|  | [`limits-h`](#index-limits_002dh) | [limits.h](#limits_002eh) |
|  | [`link`](#index-link-1) | [link](#link) |
|  | [`linkat`](#index-linkat-1) | [linkat](#linkat) |
|  | [`linked-list`](#index-linked_002dlist) | [Ordinary containers](#Ordinary-containers) |
|  | [`linkedhash-list`](#index-linkedhash_002dlist) | [Ordinary containers](#Ordinary-containers) |
|  | [`linkedhash-map`](#index-linkedhash_002dmap) | [Ordinary containers](#Ordinary-containers) |
|  | [`linkedhash-set`](#index-linkedhash_002dset) | [Ordinary containers](#Ordinary-containers) |
|  | [`list`](#index-list) | [Ordinary containers](#Ordinary-containers) |
|  | [`list`](#index-list-1) | [Ordinary containers](#Ordinary-containers) |
|  | [`list-c++`](#index-list_002dc_002b_002b) | [Ordinary containers](#Ordinary-containers) |
|  | [`listen`](#index-listen-1) | [listen](#listen) |
|  | [`localcharset`](#index-localcharset) | [<uniconv.h> modules](#g_t_003cuniconv_002eh_003e-modules) |
|  | [`locale-h`](#index-locale_002dh) | [locale.h](#locale_002eh) |
|  | [`localeconv`](#index-localeconv-1) | [localeconv](#localeconv) |
|  | [`localtime`](#index-localtime-1) | [localtime](#localtime) |
|  | [`lock`](#index-lock) | [Gnulib multithreading](#Gnulib-multithreading) |
|  | [`log`](#index-log-1) | [log](#log) |
|  | [`log-ieee`](#index-log_002dieee) | [log](#log) |
|  | [`log10`](#index-log10-1) | [log10](#log10) |
|  | [`log10-ieee`](#index-log10_002dieee) | [log10](#log10) |
|  | [`log10f`](#index-log10f-1) | [log10f](#log10f) |
|  | [`log10f-ieee`](#index-log10f_002dieee) | [log10f](#log10f) |
|  | [`log10l`](#index-log10l-1) | [log10l](#log10l) |
|  | [`log10l-ieee`](#index-log10l_002dieee) | [log10l](#log10l) |
|  | [`log1p`](#index-log1p-1) | [log1p](#log1p) |
|  | [`log1p-ieee`](#index-log1p_002dieee) | [log1p](#log1p) |
|  | [`log1pf`](#index-log1pf-1) | [log1pf](#log1pf) |
|  | [`log1pf-ieee`](#index-log1pf_002dieee) | [log1pf](#log1pf) |
|  | [`log1pl`](#index-log1pl-1) | [log1pl](#log1pl) |
|  | [`log1pl-ieee`](#index-log1pl_002dieee) | [log1pl](#log1pl) |
|  | [`log2`](#index-log2-1) | [log2](#log2) |
|  | [`log2-ieee`](#index-log2_002dieee) | [log2](#log2) |
|  | [`log2f`](#index-log2f-1) | [log2f](#log2f) |
|  | [`log2f-ieee`](#index-log2f_002dieee) | [log2f](#log2f) |
|  | [`log2l`](#index-log2l-1) | [log2l](#log2l) |
|  | [`log2l-ieee`](#index-log2l_002dieee) | [log2l](#log2l) |
|  | [`logb`](#index-logb-1) | [logb](#logb) |
|  | [`logb-ieee`](#index-logb_002dieee) | [logb](#logb) |
|  | [`logbf`](#index-logbf-1) | [logbf](#logbf) |
|  | [`logbf-ieee`](#index-logbf_002dieee) | [logbf](#logbf) |
|  | [`logbl`](#index-logbl-1) | [logbl](#logbl) |
|  | [`logbl-ieee`](#index-logbl_002dieee) | [logbl](#logbl) |
|  | [`logf`](#index-logf-1) | [logf](#logf) |
|  | [`logf-ieee`](#index-logf_002dieee) | [logf](#logf) |
|  | [`login_tty`](#index-login_005ftty-1) | [login\_tty](#login_005ftty) |
|  | [`logl`](#index-logl-1) | [logl](#logl) |
|  | [`logl-ieee`](#index-logl_002dieee) | [logl](#logl) |
|  | [`logp1`](#index-logp1-1) | [logp1](#logp1) |
|  | [`logp1f`](#index-logp1f-1) | [logp1f](#logp1f) |
|  | [`logp1l`](#index-logp1l-1) | [logp1l](#logp1l) |
|  | [`lseek`](#index-lseek-1) | [lseek](#lseek) |
|  | [`lstat`](#index-lstat-1) | [lstat](#lstat) |
| --- | | |
| M |
|  | [`maintainer-makefile`](#index-maintainer_002dmakefile) | [Various Kinds of Modules](#Various-Kinds-of-Modules) |
|  | [`maintainer-makefile`](#index-maintainer_002dmakefile-1) | [maintainer-makefile](#maintainer_002dmakefile) |
|  | [`malloc-gnu`](#index-malloc_002dgnu) | [malloc](#malloc) |
|  | [`malloc-h`](#index-malloc_002dh) | [malloc.h](#malloc_002eh) |
|  | [`malloc-posix`](#index-malloc_002dposix) | [malloc](#malloc) |
|  | [`manywarnings`](#index-manywarnings) | [warnings](#warnings) |
|  | [`manywarnings`](#index-manywarnings-1) | [manywarnings](#manywarnings) |
|  | [`map`](#index-map) | [Ordinary containers](#Ordinary-containers) |
|  | [`map`](#index-map-1) | [Ordinary containers](#Ordinary-containers) |
|  | [`map-c++`](#index-map_002dc_002b_002b) | [Ordinary containers](#Ordinary-containers) |
|  | [`math-h`](#index-math_002dh) | [math.h](#math_002eh) |
|  | [`mbchar`](#index-mbchar) | [isalnum](#isalnum) |
|  | [`mbchar`](#index-mbchar-1) | [isalpha](#isalpha) |
|  | [`mbchar`](#index-mbchar-2) | [isblank](#isblank) |
|  | [`mbchar`](#index-mbchar-3) | [iscntrl](#iscntrl) |
|  | [`mbchar`](#index-mbchar-4) | [isdigit](#isdigit) |
|  | [`mbchar`](#index-mbchar-5) | [isgraph](#isgraph) |
|  | [`mbchar`](#index-mbchar-6) | [islower](#islower) |
|  | [`mbchar`](#index-mbchar-7) | [isprint](#isprint) |
|  | [`mbchar`](#index-mbchar-8) | [ispunct](#ispunct) |
|  | [`mbchar`](#index-mbchar-9) | [isspace](#isspace) |
|  | [`mbchar`](#index-mbchar-10) | [isupper](#isupper) |
|  | [`mbchar`](#index-mbchar-11) | [isxdigit](#isxdigit) |
|  | [`mbchar`](#index-mbchar-12) | [isascii](#isascii) |
|  | [`mbchar`](#index-mbchar-13) | [Comparison of character APIs](#Comparison-of-character-APIs) |
|  | [`mbfile`](#index-mbfile) | [Reading multibyte strings](#Reading-multibyte-strings) |
|  | [`mbiter`](#index-mbiter) | [Iterating through strings](#Iterating-through-strings) |
|  | [`mbiterf`](#index-mbiterf) | [Iterating through strings](#Iterating-through-strings) |
|  | [`mbrlen`](#index-mbrlen-1) | [mbrlen](#mbrlen) |
|  | [`mbrtoc16`](#index-mbrtoc16-1) | [mbrtoc16](#mbrtoc16) |
|  | [`mbrtoc32`](#index-mbrtoc32-1) | [mbrtoc32](#mbrtoc32) |
|  | [`mbrtoc32`](#index-mbrtoc32-2) | [mbrtowc](#mbrtowc) |
|  | [`mbrtoc32-regular`](#index-mbrtoc32_002dregular) | [mbrtoc32](#mbrtoc32) |
|  | [`mbrtowc`](#index-mbrtowc-1) | [mbrtowc](#mbrtowc) |
|  | [`mbs_endswith`](#index-mbs_005fendswith) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbs_startswith`](#index-mbs_005fstartswith) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbscasecmp`](#index-mbscasecmp) | [strcasecmp](#strcasecmp) |
|  | [`mbscasecmp`](#index-mbscasecmp-1) | [c-strcasecmp](#c_002dstrcasecmp) |
|  | [`mbscasecmp`](#index-mbscasecmp-2) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbscasestr`](#index-mbscasestr) | [c-strcasestr](#c_002dstrcasestr) |
|  | [`mbscasestr`](#index-mbscasestr-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbschr`](#index-mbschr) | [strchr](#strchr) |
|  | [`mbschr`](#index-mbschr-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbscspn`](#index-mbscspn) | [strcspn](#strcspn) |
|  | [`mbscspn`](#index-mbscspn-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbsinit`](#index-mbsinit-1) | [mbsinit](#mbsinit) |
|  | [`mbslen`](#index-mbslen) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbsncasecmp`](#index-mbsncasecmp) | [strncasecmp](#strncasecmp) |
|  | [`mbsncasecmp`](#index-mbsncasecmp-1) | [c-strncasecmp](#c_002dstrncasecmp) |
|  | [`mbsncasecmp`](#index-mbsncasecmp-2) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbsnlen`](#index-mbsnlen) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbsnrtoc32s`](#index-mbsnrtoc32s) | [mbsnrtowcs](#mbsnrtowcs) |
|  | [`mbsnrtoc32s`](#index-mbsnrtoc32s-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbsnrtowcs`](#index-mbsnrtowcs-1) | [mbsnrtowcs](#mbsnrtowcs) |
|  | [`mbsnrtowcs`](#index-mbsnrtowcs-2) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbspbrk`](#index-mbspbrk) | [strpbrk](#strpbrk) |
|  | [`mbspbrk`](#index-mbspbrk-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbspcasecmp`](#index-mbspcasecmp) | [strncasecmp](#strncasecmp) |
|  | [`mbspcasecmp`](#index-mbspcasecmp-1) | [c-strncasecmp](#c_002dstrncasecmp) |
|  | [`mbspcasecmp`](#index-mbspcasecmp-2) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbsrchr`](#index-mbsrchr) | [strrchr](#strrchr) |
|  | [`mbsrchr`](#index-mbsrchr-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbsrtoc32s`](#index-mbsrtoc32s) | [mbsrtowcs](#mbsrtowcs) |
|  | [`mbsrtoc32s`](#index-mbsrtoc32s-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbsrtowcs`](#index-mbsrtowcs-1) | [mbsrtowcs](#mbsrtowcs) |
|  | [`mbsrtowcs`](#index-mbsrtowcs-2) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbssep`](#index-mbssep) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbsspn`](#index-mbsspn) | [strspn](#strspn) |
|  | [`mbsspn`](#index-mbsspn-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbsstr`](#index-mbsstr) | [strstr](#strstr) |
|  | [`mbsstr`](#index-mbsstr-1) | [c-strstr](#c_002dstrstr) |
|  | [`mbsstr`](#index-mbsstr-2) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbstoc32s`](#index-mbstoc32s) | [mbstowcs](#mbstowcs) |
|  | [`mbstoc32s`](#index-mbstoc32s-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbstok_r`](#index-mbstok_005fr) | [strtok\_r](#strtok_005fr) |
|  | [`mbstok_r`](#index-mbstok_005fr-1) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbstowcs`](#index-mbstowcs-1) | [mbstowcs](#mbstowcs) |
|  | [`mbstowcs`](#index-mbstowcs-2) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbswidth`](#index-mbswidth) | [Comparison of string APIs](#Comparison-of-string-APIs) |
|  | [`mbtowc`](#index-mbtowc-1) | [mbtowc](#mbtowc) |
|  | [`mbuiter`](#index-mbuiter) | [Iterating through strings](#Iterating-through-strings) |
|  | [`mbuiterf`](#index-mbuiterf) | [Iterating through strings](#Iterating-through-strings) |
|  | [`mcel`](#index-mcel) | [Iterating through strings](#Iterating-through-strings) |
|  | [`mcel-prefer`](#index-mcel_002dprefer) | [Iterating through strings](#Iterating-through-strings) |
|  | [`memalign`](#index-memalign-1) | [memalign](#memalign) |
|  | [`memccpy`](#index-memccpy-1) | [memccpy](#memccpy) |
|  | [`memchr`](#index-memchr-1) | [memchr](#memchr) |
|  | [`memcmp`](#index-memcmp-1) | [memcmp](#memcmp) |
|  | [`memcpy`](#index-memcpy-1) | [memcpy](#memcpy) |
|  | [`memmem`](#index-memmem-1) | [memmem](#memmem) |
|  | [`memmem-simple`](#index-memmem_002dsimple) | [memmem](#memmem) |
|  | [`memmove`](#index-memmove-1) | [memmove](#memmove) |
|  | [`mempcpy`](#index-mempcpy-1) | [mempcpy](#mempcpy) |
|  | [`memrchr`](#index-memrchr-1) | [memrchr](#memrchr) |
|  | [`memset`](#index-memset-1) | [memset](#memset) |
|  | [`memset_explicit`](#index-memset_005fexplicit-1) | [memset\_explicit](#memset_005fexplicit) |
|  | [`mgetgroups`](#index-mgetgroups) | [getgroups](#getgroups) |
|  | [`mgetgroups`](#index-mgetgroups-1) | [initgroups](#initgroups) |
|  | [`mkdir`](#index-mkdir-1) | [mkdir](#mkdir) |
|  | [`mkdirat`](#index-mkdirat-1) | [mkdirat](#mkdirat) |
|  | [`mkdtemp`](#index-mkdtemp-1) | [mkdtemp](#mkdtemp) |
|  | [`mkfifo`](#index-mkfifo-1) | [mkfifo](#mkfifo) |
|  | [`mkfifoat`](#index-mkfifoat-1) | [mkfifoat](#mkfifoat) |
|  | [`mkfifoat`](#index-mkfifoat-2) | [mknodat](#mknodat) |
|  | [`mknod`](#index-mknod-1) | [mknod](#mknod) |
|  | [`mkostemp`](#index-mkostemp-1) | [mkostemp](#mkostemp) |
|  | [`mkostemps`](#index-mkostemps-1) | [mkostemps](#mkostemps) |
|  | [`mkstemp`](#index-mkstemp-1) | [mkstemp](#mkstemp) |
|  | [`mkstemps`](#index-mkstemps-1) | [mkstemps](#mkstemps) |
|  | [`mktime`](#index-mktime-1) | [mktime](#mktime) |
|  | [`mntent-h`](#index-mntent_002dh) | [mntent.h](#mntent_002eh) |
|  | [`modf`](#index-modf-1) | [modf](#modf) |
|  | [`modf-ieee`](#index-modf_002dieee) | [modf](#modf) |
|  | [`modff`](#index-modff-1) | [modff](#modff) |
|  | [`modff-ieee`](#index-modff_002dieee) | [modff](#modff) |
|  | [`modfl`](#index-modfl-1) | [modfl](#modfl) |
|  | [`modfl-ieee`](#index-modfl_002dieee) | [modfl](#modfl) |
|  | [`monetary-h`](#index-monetary_002dh) | [monetary.h](#monetary_002eh) |
|  | [`mountlist`](#index-mountlist) | [mntent.h](#mntent_002eh) |
|  | [`mtx`](#index-mtx) | [mtx\_destroy](#mtx_005fdestroy) |
|  | [`mtx`](#index-mtx-1) | [mtx\_init](#mtx_005finit) |
|  | [`mtx`](#index-mtx-2) | [mtx\_lock](#mtx_005flock) |
|  | [`mtx`](#index-mtx-3) | [mtx\_timedlock](#mtx_005ftimedlock) |
|  | [`mtx`](#index-mtx-4) | [mtx\_trylock](#mtx_005ftrylock) |
|  | [`mtx`](#index-mtx-5) | [mtx\_unlock](#mtx_005funlock) |
|  | [`mtx`](#index-mtx-6) | [ISO C multithreading](#ISO-C-multithreading) |
| --- | | |
| N |
|  | [`nanosleep`](#index-nanosleep-1) | [nanosleep](#nanosleep) |
|  | [`net_if-h`](#index-net_005fif_002dh) | [net/if.h](#net_002fif_002eh) |
|  | [`netdb-h`](#index-netdb_002dh) | [netdb.h](#netdb_002eh) |
|  | [`netinet_in-h`](#index-netinet_005fin_002dh) | [netinet/in.h](#netinet_002fin_002eh) |
|  | [`newlocale`](#index-newlocale-1) | [newlocale](#newlocale) |
|  | [`nextafter`](#index-nextafter-1) | [nextafter](#nextafter) |
|  | [`nl_langinfo`](#index-nl_005flanginfo-1) | [nl\_langinfo](#nl_005flanginfo) |
|  | [`nonblocking`](#index-nonblocking) | [fcntl.h](#fcntl_002eh) |
|  | [`nonblocking`](#index-nonblocking-1) | [open](#open) |
|  | [`nonblocking`](#index-nonblocking-2) | [fgetc](#fgetc) |
|  | [`nonblocking`](#index-nonblocking-3) | [fgets](#fgets) |
|  | [`nonblocking`](#index-nonblocking-4) | [fprintf](#fprintf) |
|  | [`nonblocking`](#index-nonblocking-5) | [fputc](#fputc) |
|  | [`nonblocking`](#index-nonblocking-6) | [fputs](#fputs) |
|  | [`nonblocking`](#index-nonblocking-7) | [fread](#fread) |
|  | [`nonblocking`](#index-nonblocking-8) | [fscanf](#fscanf) |
|  | [`nonblocking`](#index-nonblocking-9) | [fwrite](#fwrite) |
|  | [`nonblocking`](#index-nonblocking-10) | [getc](#getc) |
|  | [`nonblocking`](#index-nonblocking-11) | [getchar](#getchar) |
|  | [`nonblocking`](#index-nonblocking-12) | [printf](#printf) |
|  | [`nonblocking`](#index-nonblocking-13) | [putc](#putc) |
|  | [`nonblocking`](#index-nonblocking-14) | [putchar](#putchar) |
|  | [`nonblocking`](#index-nonblocking-15) | [puts](#puts) |
|  | [`nonblocking`](#index-nonblocking-16) | [scanf](#scanf) |
|  | [`nonblocking`](#index-nonblocking-17) | [vfprintf](#vfprintf) |
|  | [`nonblocking`](#index-nonblocking-18) | [vfscanf](#vfscanf) |
|  | [`nonblocking`](#index-nonblocking-19) | [vprintf](#vprintf) |
|  | [`nonblocking`](#index-nonblocking-20) | [vscanf](#vscanf) |
|  | [`nonblocking`](#index-nonblocking-21) | [pipe2](#pipe2) |
|  | [`nonblocking`](#index-nonblocking-22) | [read](#read) |
|  | [`nonblocking`](#index-nonblocking-23) | [write](#write) |
|  | [`noreturn`](#index-noreturn) | [Non-returning Functions](#Non_002dreturning-Functions) |
|  | [`nproc`](#index-nproc) | [get\_nprocs](#get_005fnprocs) |
|  | [`nstrftime`](#index-nstrftime) | [strftime](#strftime) |
|  | [`nullptr`](#index-nullptr) | [nullptr](#nullptr) |
| --- | | |
| O |
|  | [`obstack`](#index-obstack) | [obstack.h](#obstack_002eh) |
|  | [`obstack-printf`](#index-obstack_002dprintf) | [obstack\_printf](#obstack_005fprintf) |
|  | [`obstack-printf`](#index-obstack_002dprintf-1) | [obstack\_vprintf](#obstack_005fvprintf) |
|  | [`obstack-printf-gnu`](#index-obstack_002dprintf_002dgnu) | [obstack\_printf](#obstack_005fprintf) |
|  | [`obstack-printf-gnu`](#index-obstack_002dprintf_002dgnu-1) | [obstack\_vprintf](#obstack_005fvprintf) |
|  | [`obstack-printf-posix`](#index-obstack_002dprintf_002dposix) | [obstack\_printf](#obstack_005fprintf) |
|  | [`obstack-printf-posix`](#index-obstack_002dprintf_002dposix-1) | [obstack\_vprintf](#obstack_005fvprintf) |
|  | [`obstack-zprintf`](#index-obstack_002dzprintf) | [Modernized printf](#Modernized-printf) |
|  | [`obstack-zprintf-gnu`](#index-obstack_002dzprintf_002dgnu) | [Modernized printf](#Modernized-printf) |
|  | [`obstack-zprintf-posix`](#index-obstack_002dzprintf_002dposix) | [Modernized printf](#Modernized-printf) |
|  | [`omap`](#index-omap) | [Ordinary containers](#Ordinary-containers) |
|  | [`omap`](#index-omap-1) | [Ordinary containers](#Ordinary-containers) |
|  | [`omap-c++`](#index-omap_002dc_002b_002b) | [Ordinary containers](#Ordinary-containers) |
|  | [`once`](#index-once) | [Gnulib multithreading](#Gnulib-multithreading) |
|  | [`open`](#index-open-1) | [open](#open) |
|  | [`openat`](#index-openat-1) | [openat](#openat) |
|  | [`openat-safer`](#index-openat_002dsafer) | [Closed standard fds](#Closed-standard-fds) |
|  | [`openat2`](#index-openat2-1) | [openat2](#openat2) |
|  | [`opendir`](#index-opendir-1) | [opendir](#opendir) |
|  | [`openmp`](#index-openmp) | [Multithreading](#Multithreading) |
|  | [`openpty`](#index-openpty-1) | [posix\_openpt](#posix_005fopenpt) |
|  | [`openpty`](#index-openpty-2) | [openpty](#openpty) |
|  | [`options`](#index-options) | [getopt\_long](#getopt_005flong) |
|  | [`oset`](#index-oset) | [Ordinary containers](#Ordinary-containers) |
|  | [`oset`](#index-oset-1) | [Ordinary containers](#Ordinary-containers) |
|  | [`oset-c++`](#index-oset_002dc_002b_002b) | [Ordinary containers](#Ordinary-containers) |
| --- | | |
| P |
|  | [`package-version`](#index-package_002dversion) | [Propagating the package version](#Propagating-the-package-version) |
|  | [`pagealign_alloc`](#index-pagealign_005falloc) | [aligned\_alloc](#aligned_005falloc) |
|  | [`pagealign_alloc`](#index-pagealign_005falloc-1) | [posix\_memalign](#posix_005fmemalign) |
|  | [`parse-datetime`](#index-parse_002ddatetime) | [getdate](#getdate) |
|  | [`pathmax`](#index-pathmax) | [limits.h](#limits_002eh) |
|  | [`pclose`](#index-pclose-1) | [pclose](#pclose) |
|  | [`perror`](#index-perror-1) | [perror](#perror) |
|  | [`pipe-posix`](#index-pipe_002dposix) | [pipe](#pipe) |
|  | [`pipe2`](#index-pipe2-1) | [pipe2](#pipe2) |
|  | [`pipe2-safer`](#index-pipe2_002dsafer) | [Closed standard fds](#Closed-standard-fds) |
|  | [`poll`](#index-poll-1) | [poll](#poll) |
|  | [`poll-h`](#index-poll_002dh) | [poll.h](#poll_002eh) |
|  | [`popen`](#index-popen-1) | [popen](#popen) |
|  | [`popen-safer`](#index-popen_002dsafer) | [Closed standard fds](#Closed-standard-fds) |
|  | [`posix_memalign`](#index-posix_005fmemalign-1) | [posix\_memalign](#posix_005fmemalign) |
|  | [`posix_openpt`](#index-posix_005fopenpt-1) | [posix\_openpt](#posix_005fopenpt) |
|  | [`posix_spawn`](#index-posix_005fspawn-1) | [posix\_spawn](#posix_005fspawn) |
|  | [`posix_spawn_file_actions_addchdir`](#index-posix_005fspawn_005ffile_005factions_005faddchdir-1) | [posix\_spawn](#posix_005fspawn) |
|  | [`posix_spawn_file_actions_addchdir`](#index-posix_005fspawn_005ffile_005factions_005faddchdir-2) | [posix\_spawnp](#posix_005fspawnp) |
|  | [`posix_spawn_file_actions_addchdir`](#index-posix_005fspawn_005ffile_005factions_005faddchdir-3) | [posix\_spawn\_file\_actions\_addchdir\_np](#posix_005fspawn_005ffile_005factions_005faddchdir_005fnp) |
|  | [`posix_spawn_file_actions_addclose`](#index-posix_005fspawn_005ffile_005factions_005faddclose-1) | [posix\_spawn\_file\_actions\_addclose](#posix_005fspawn_005ffile_005factions_005faddclose) |
|  | [`posix_spawn_file_actions_adddup2`](#index-posix_005fspawn_005ffile_005factions_005fadddup2-1) | [posix\_spawn\_file\_actions\_adddup2](#posix_005fspawn_005ffile_005factions_005fadddup2) |
|  | [`posix_spawn_file_actions_addfchdir`](#index-posix_005fspawn_005ffile_005factions_005faddfchdir-1) | [posix\_spawn](#posix_005fspawn) |
|  | [`posix_spawn_file_actions_addfchdir`](#index-posix_005fspawn_005ffile_005factions_005faddfchdir-2) | [posix\_spawnp](#posix_005fspawnp) |
|  | [`posix_spawn_file_actions_addfchdir`](#index-posix_005fspawn_005ffile_005factions_005faddfchdir-3) | [posix\_spawn\_file\_actions\_addfchdir\_np](#posix_005fspawn_005ffile_005factions_005faddfchdir_005fnp) |
|  | [`posix_spawn_file_actions_addopen`](#index-posix_005fspawn_005ffile_005factions_005faddopen-1) | [posix\_spawn\_file\_actions\_addopen](#posix_005fspawn_005ffile_005factions_005faddopen) |
|  | [`posix_spawn_file_actions_destroy`](#index-posix_005fspawn_005ffile_005factions_005fdestroy-1) | [posix\_spawn\_file\_actions\_destroy](#posix_005fspawn_005ffile_005factions_005fdestroy) |
|  | [`posix_spawn_file_actions_init`](#index-posix_005fspawn_005ffile_005factions_005finit-1) | [posix\_spawn\_file\_actions\_init](#posix_005fspawn_005ffile_005factions_005finit) |
|  | [`posix_spawnattr_destroy`](#index-posix_005fspawnattr_005fdestroy-1) | [posix\_spawnattr\_destroy](#posix_005fspawnattr_005fdestroy) |
|  | [`posix_spawnattr_getflags`](#index-posix_005fspawnattr_005fgetflags-1) | [posix\_spawnattr\_getflags](#posix_005fspawnattr_005fgetflags) |
|  | [`posix_spawnattr_getpgroup`](#index-posix_005fspawnattr_005fgetpgroup-1) | [posix\_spawnattr\_getpgroup](#posix_005fspawnattr_005fgetpgroup) |
|  | [`posix_spawnattr_getschedparam`](#index-posix_005fspawnattr_005fgetschedparam-1) | [posix\_spawnattr\_getschedparam](#posix_005fspawnattr_005fgetschedparam) |
|  | [`posix_spawnattr_getschedpolicy`](#index-posix_005fspawnattr_005fgetschedpolicy-1) | [posix\_spawnattr\_getschedpolicy](#posix_005fspawnattr_005fgetschedpolicy) |
|  | [`posix_spawnattr_getsigdefault`](#index-posix_005fspawnattr_005fgetsigdefault-1) | [posix\_spawnattr\_getsigdefault](#posix_005fspawnattr_005fgetsigdefault) |
|  | [`posix_spawnattr_getsigmask`](#index-posix_005fspawnattr_005fgetsigmask-1) | [posix\_spawnattr\_getsigmask](#posix_005fspawnattr_005fgetsigmask) |
|  | [`posix_spawnattr_init`](#index-posix_005fspawnattr_005finit-1) | [posix\_spawnattr\_init](#posix_005fspawnattr_005finit) |
|  | [`posix_spawnattr_setflags`](#index-posix_005fspawnattr_005fsetflags-1) | [posix\_spawnattr\_setflags](#posix_005fspawnattr_005fsetflags) |
|  | [`posix_spawnattr_setpgroup`](#index-posix_005fspawnattr_005fsetpgroup-1) | [posix\_spawnattr\_setpgroup](#posix_005fspawnattr_005fsetpgroup) |
|  | [`posix_spawnattr_setschedparam`](#index-posix_005fspawnattr_005fsetschedparam-1) | [posix\_spawnattr\_setschedparam](#posix_005fspawnattr_005fsetschedparam) |
|  | [`posix_spawnattr_setschedpolicy`](#index-posix_005fspawnattr_005fsetschedpolicy-1) | [posix\_spawnattr\_setschedpolicy](#posix_005fspawnattr_005fsetschedpolicy) |
|  | [`posix_spawnattr_setsigdefault`](#index-posix_005fspawnattr_005fsetsigdefault-1) | [posix\_spawnattr\_setsigdefault](#posix_005fspawnattr_005fsetsigdefault) |
|  | [`posix_spawnattr_setsigmask`](#index-posix_005fspawnattr_005fsetsigmask-1) | [posix\_spawnattr\_setsigmask](#posix_005fspawnattr_005fsetsigmask) |
|  | [`posix_spawnp`](#index-posix_005fspawnp-1) | [posix\_spawnp](#posix_005fspawnp) |
|  | [`posixcheck`](#index-posixcheck) | [Finding POSIX substitutes](#Finding-POSIX-substitutes) |
|  | [`pow`](#index-pow-1) | [pow](#pow) |
|  | [`powf`](#index-powf-1) | [powf](#powf) |
|  | [`pread`](#index-pread-1) | [pread](#pread) |
|  | [`printf-gnu`](#index-printf_002dgnu) | [printf](#printf) |
|  | [`printf-posix`](#index-printf_002dposix) | [printf](#printf) |
|  | [`printf-with-n-directive`](#index-printf_002dwith_002dn_002ddirective) | [dprintf](#dprintf) |
|  | [`printf-with-n-directive`](#index-printf_002dwith_002dn_002ddirective-1) | [fprintf](#fprintf) |
|  | [`printf-with-n-directive`](#index-printf_002dwith_002dn_002ddirective-2) | [printf](#printf) |
|  | [`printf-with-n-directive`](#index-printf_002dwith_002dn_002ddirective-3) | [snprintf](#snprintf) |
|  | [`printf-with-n-directive`](#index-printf_002dwith_002dn_002ddirective-4) | [sprintf](#sprintf) |
|  | [`printf-with-n-directive`](#index-printf_002dwith_002dn_002ddirective-5) | [vdprintf](#vdprintf) |
|  | [`printf-with-n-directive`](#index-printf_002dwith_002dn_002ddirective-6) | [vfprintf](#vfprintf) |
|  | [`printf-with-n-directive`](#index-printf_002dwith_002dn_002ddirective-7) | [vprintf](#vprintf) |
|  | [`printf-with-n-directive`](#index-printf_002dwith_002dn_002ddirective-8) | [vsnprintf](#vsnprintf) |
|  | [`printf-with-n-directive`](#index-printf_002dwith_002dn_002ddirective-9) | [vsprintf](#vsprintf) |
|  | [`printf-with-n-directive`](#index-printf_002dwith_002dn_002ddirective-10) | [obstack\_printf](#obstack_005fprintf) |
|  | [`printf-with-n-directive`](#index-printf_002dwith_002dn_002ddirective-11) | [obstack\_vprintf](#obstack_005fvprintf) |
|  | [`progname`](#index-progname) | [progname and getprogname](#progname-and-getprogname) |
|  | [`pselect`](#index-pselect-1) | [pselect](#pselect) |
|  | [`pthread`](#index-pthread) | [POSIX multithreading](#POSIX-multithreading) |
|  | [`pthread_mutex_timedlock`](#index-pthread_005fmutex_005ftimedlock-1) | [pthread\_mutex\_timedlock](#pthread_005fmutex_005ftimedlock) |
|  | [`pthread_sigmask`](#index-pthread_005fsigmask-1) | [pthread\_sigmask](#pthread_005fsigmask) |
|  | [`pthread-cond`](#index-pthread_002dcond) | [pthread\_cond\_broadcast](#pthread_005fcond_005fbroadcast) |
|  | [`pthread-cond`](#index-pthread_002dcond-1) | [pthread\_cond\_destroy](#pthread_005fcond_005fdestroy) |
|  | [`pthread-cond`](#index-pthread_002dcond-2) | [pthread\_cond\_init](#pthread_005fcond_005finit) |
|  | [`pthread-cond`](#index-pthread_002dcond-3) | [pthread\_cond\_signal](#pthread_005fcond_005fsignal) |
|  | [`pthread-cond`](#index-pthread_002dcond-4) | [pthread\_cond\_timedwait](#pthread_005fcond_005ftimedwait) |
|  | [`pthread-cond`](#index-pthread_002dcond-5) | [pthread\_cond\_wait](#pthread_005fcond_005fwait) |
|  | [`pthread-cond`](#index-pthread_002dcond-6) | [pthread\_condattr\_destroy](#pthread_005fcondattr_005fdestroy) |
|  | [`pthread-cond`](#index-pthread_002dcond-7) | [pthread\_condattr\_init](#pthread_005fcondattr_005finit) |
|  | [`pthread-cond`](#index-pthread_002dcond-8) | [POSIX multithreading](#POSIX-multithreading) |
|  | [`pthread-h`](#index-pthread_002dh) | [pthread.h](#pthread_002eh) |
|  | [`pthread-mutex`](#index-pthread_002dmutex) | [pthread\_mutex\_destroy](#pthread_005fmutex_005fdestroy) |
|  | [`pthread-mutex`](#index-pthread_002dmutex-1) | [pthread\_mutex\_init](#pthread_005fmutex_005finit) |
|  | [`pthread-mutex`](#index-pthread_002dmutex-2) | [pthread\_mutex\_lock](#pthread_005fmutex_005flock) |
|  | [`pthread-mutex`](#index-pthread_002dmutex-3) | [pthread\_mutex\_trylock](#pthread_005fmutex_005ftrylock) |
|  | [`pthread-mutex`](#index-pthread_002dmutex-4) | [pthread\_mutex\_unlock](#pthread_005fmutex_005funlock) |
|  | [`pthread-mutex`](#index-pthread_002dmutex-5) | [pthread\_mutexattr\_destroy](#pthread_005fmutexattr_005fdestroy) |
|  | [`pthread-mutex`](#index-pthread_002dmutex-6) | [pthread\_mutexattr\_getrobust](#pthread_005fmutexattr_005fgetrobust) |
|  | [`pthread-mutex`](#index-pthread_002dmutex-7) | [pthread\_mutexattr\_gettype](#pthread_005fmutexattr_005fgettype) |
|  | [`pthread-mutex`](#index-pthread_002dmutex-8) | [pthread\_mutexattr\_init](#pthread_005fmutexattr_005finit) |
|  | [`pthread-mutex`](#index-pthread_002dmutex-9) | [pthread\_mutexattr\_setrobust](#pthread_005fmutexattr_005fsetrobust) |
|  | [`pthread-mutex`](#index-pthread_002dmutex-10) | [pthread\_mutexattr\_settype](#pthread_005fmutexattr_005fsettype) |
|  | [`pthread-mutex`](#index-pthread_002dmutex-11) | [POSIX multithreading](#POSIX-multithreading) |
|  | [`pthread-once`](#index-pthread_002donce) | [pthread\_once](#pthread_005fonce) |
|  | [`pthread-once`](#index-pthread_002donce-1) | [POSIX multithreading](#POSIX-multithreading) |
|  | [`pthread-rwlock`](#index-pthread_002drwlock) | [pthread\_rwlock\_destroy](#pthread_005frwlock_005fdestroy) |
|  | [`pthread-rwlock`](#index-pthread_002drwlock-1) | [pthread\_rwlock\_init](#pthread_005frwlock_005finit) |
|  | [`pthread-rwlock`](#index-pthread_002drwlock-2) | [pthread\_rwlock\_rdlock](#pthread_005frwlock_005frdlock) |
|  | [`pthread-rwlock`](#index-pthread_002drwlock-3) | [pthread\_rwlock\_timedrdlock](#pthread_005frwlock_005ftimedrdlock) |
|  | [`pthread-rwlock`](#index-pthread_002drwlock-4) | [pthread\_rwlock\_timedwrlock](#pthread_005frwlock_005ftimedwrlock) |
|  | [`pthread-rwlock`](#index-pthread_002drwlock-5) | [pthread\_rwlock\_tryrdlock](#pthread_005frwlock_005ftryrdlock) |
|  | [`pthread-rwlock`](#index-pthread_002drwlock-6) | [pthread\_rwlock\_trywrlock](#pthread_005frwlock_005ftrywrlock) |
|  | [`pthread-rwlock`](#index-pthread_002drwlock-7) | [pthread\_rwlock\_unlock](#pthread_005frwlock_005funlock) |
|  | [`pthread-rwlock`](#index-pthread_002drwlock-8) | [pthread\_rwlock\_wrlock](#pthread_005frwlock_005fwrlock) |
|  | [`pthread-rwlock`](#index-pthread_002drwlock-9) | [pthread\_rwlockattr\_destroy](#pthread_005frwlockattr_005fdestroy) |
|  | [`pthread-rwlock`](#index-pthread_002drwlock-10) | [pthread\_rwlockattr\_init](#pthread_005frwlockattr_005finit) |
|  | [`pthread-rwlock`](#index-pthread_002drwlock-11) | [POSIX multithreading](#POSIX-multithreading) |
|  | [`pthread-spin`](#index-pthread_002dspin) | [pthread\_spin\_destroy](#pthread_005fspin_005fdestroy) |
|  | [`pthread-spin`](#index-pthread_002dspin-1) | [pthread\_spin\_init](#pthread_005fspin_005finit) |
|  | [`pthread-spin`](#index-pthread_002dspin-2) | [pthread\_spin\_lock](#pthread_005fspin_005flock) |
|  | [`pthread-spin`](#index-pthread_002dspin-3) | [pthread\_spin\_trylock](#pthread_005fspin_005ftrylock) |
|  | [`pthread-spin`](#index-pthread_002dspin-4) | [pthread\_spin\_unlock](#pthread_005fspin_005funlock) |
|  | [`pthread-spin`](#index-pthread_002dspin-5) | [POSIX multithreading](#POSIX-multithreading) |
|  | [`pthread-thread`](#index-pthread_002dthread) | [pthread\_attr\_destroy](#pthread_005fattr_005fdestroy) |
|  | [`pthread-thread`](#index-pthread_002dthread-1) | [pthread\_attr\_getdetachstate](#pthread_005fattr_005fgetdetachstate) |
|  | [`pthread-thread`](#index-pthread_002dthread-2) | [pthread\_attr\_init](#pthread_005fattr_005finit) |
|  | [`pthread-thread`](#index-pthread_002dthread-3) | [pthread\_attr\_setdetachstate](#pthread_005fattr_005fsetdetachstate) |
|  | [`pthread-thread`](#index-pthread_002dthread-4) | [pthread\_create](#pthread_005fcreate) |
|  | [`pthread-thread`](#index-pthread_002dthread-5) | [pthread\_detach](#pthread_005fdetach) |
|  | [`pthread-thread`](#index-pthread_002dthread-6) | [pthread\_equal](#pthread_005fequal) |
|  | [`pthread-thread`](#index-pthread_002dthread-7) | [pthread\_exit](#pthread_005fexit) |
|  | [`pthread-thread`](#index-pthread_002dthread-8) | [pthread\_join](#pthread_005fjoin) |
|  | [`pthread-thread`](#index-pthread_002dthread-9) | [pthread\_self](#pthread_005fself) |
|  | [`pthread-thread`](#index-pthread_002dthread-10) | [POSIX multithreading](#POSIX-multithreading) |
|  | [`pthread-tss`](#index-pthread_002dtss) | [pthread\_getspecific](#pthread_005fgetspecific) |
|  | [`pthread-tss`](#index-pthread_002dtss-1) | [pthread\_key\_create](#pthread_005fkey_005fcreate) |
|  | [`pthread-tss`](#index-pthread_002dtss-2) | [pthread\_key\_delete](#pthread_005fkey_005fdelete) |
|  | [`pthread-tss`](#index-pthread_002dtss-3) | [pthread\_setspecific](#pthread_005fsetspecific) |
|  | [`pthread-tss`](#index-pthread_002dtss-4) | [POSIX multithreading](#POSIX-multithreading) |
|  | [`ptsname`](#index-ptsname-1) | [ptsname](#ptsname) |
|  | [`ptsname_r`](#index-ptsname_005fr-1) | [ptsname](#ptsname) |
|  | [`ptsname_r`](#index-ptsname_005fr-2) | [ptsname\_r](#ptsname_005fr) |
|  | [`pty-h`](#index-pty_002dh) | [pty.h](#pty_002eh) |
|  | [`putenv-gnu`](#index-putenv_002dgnu) | [putenv](#putenv) |
|  | [`pwrite`](#index-pwrite-1) | [pwrite](#pwrite) |
| --- | | |
| Q |
|  | [`
