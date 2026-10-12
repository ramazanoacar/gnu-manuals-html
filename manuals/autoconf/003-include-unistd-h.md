# include <unistd.h>
#endif

#ifdef _POSIX_VERSION
/* Code for POSIX systems.  */
#endif
```

Macro: **AC\_HEADER\_TIOCGWINSZ** [¶](#index-AC_005fHEADER_005fTIOCGWINSZ-1)
:   If the use of `TIOCGWINSZ` requires `<sys/ioctl.h>`, then
    define `GWINSZ_IN_SYS_IOCTL`. Otherwise `TIOCGWINSZ` can be
    found in `<termios.h>`.

    Use:

    ```
    #ifdef HAVE_TERMIOS_H
    # include <termios.h>
    #endif

    #ifdef GWINSZ_IN_SYS_IOCTL
    # include <sys/ioctl.h>
    #endif
    ```

---

Previous: [Particular Header Checks](#Particular-Headers), Up: [Header Files](#Header-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.6.3 Generic Header Checks [¶](#Generic-Header-Checks)

These macros are used to find system header files not covered by the
“particular” test macros. If you need to check the contents of a header
as well as find out whether it is present, you have to write your own
test for it (see [Writing Tests](#Writing-Tests)).

Macro: **AC\_CHECK\_HEADER**(header-file, [action-if-found], [action-if-not-found], [includes]) [¶](#index-AC_005fCHECK_005fHEADER-1)
:   If the system header file header-file is compilable, execute shell
    commands action-if-found, otherwise execute
    action-if-not-found. If you just want to define a symbol if the
    header file is available, consider using `AC_CHECK_HEADERS`
    instead.

    includes should be the appropriate *prerequisite* code, i.e.
    whatever might be required to appear above
    ‘`#include <header-file>`’ for it to compile without error.
    This can be anything, but will normally be additional ‘`#include`’
    directives. If includes is omitted or empty, `configure` will
    use the contents of the macro `AC_INCLUDES_DEFAULT`.
    See [Default Includes](#Default-Includes).

    This macro used to check only for the *presence* of a header, not
    whether its contents were acceptable to the compiler. Some older
    `configure` scripts rely on this behavior, so it is still
    available by specifying ‘`-`’ as includes. This mechanism is
    deprecated as of Autoconf 2.70; situations where a preprocessor-only
    check is required should use `AC_PREPROC_IFELSE`.
    See [Running the Preprocessor](#Running-the-Preprocessor).

    This macro caches its result in the `ac_cv_header_header-file`
    variable, with characters not suitable for a variable name mapped to
    underscores.

Macro: **AC\_CHECK\_HEADERS**(header-file…, [action-if-found], [action-if-not-found], [includes]) [¶](#index-AC_005fCHECK_005fHEADERS-1)
:   For each given system header file header-file in the
    blank-separated argument list that exists, define
    `HAVE_header-file` (in all capitals). If action-if-found
    is given, it is additional shell code to execute when one of the header
    files is found. You can give it a value of ‘`break`’ to break out of
    the loop on the first match. If action-if-not-found is given, it
    is executed when one of the header files is not found.

    includes is interpreted as in `AC_CHECK_HEADER`, in order to
    choose the set of preprocessor directives supplied before the header
    under test.

    This macro caches its result in the `ac_cv_header_header-file`
    variable, with characters not suitable for a variable name mapped to
    underscores.

Macro: **AC\_CHECK\_HEADERS\_ONCE**(header-file…) [¶](#index-AC_005fCHECK_005fHEADERS_005fONCE-1)
:   For each given system header file header-file in the
    blank-separated argument list that exists, define
    `HAVE_header-file` (in all capitals).

    If you do not need the full power of `AC_CHECK_HEADERS`, this
    variant generates smaller, faster `configure` files. All
    headers passed to `AC_CHECK_HEADERS_ONCE` are checked for in one
    pass, early during the `configure` run. The checks cannot be
    conditionalized, you cannot specify an action-if-found or
    action-if-not-found, and `AC_INCLUDES_DEFAULT` is always used
    for the prerequisites.

In previous versions of Autoconf, these macros merely checked whether
the header was accepted by the preprocessor. This was changed because
the old test was inappropriate for typical uses. Headers are typically
used to compile, not merely to preprocess, and the old behavior
sometimes accepted headers that clashed at compile-time
(see [Header Present But Cannot Be Compiled](#Present-But-Cannot-Be-Compiled)). If for some reason it is
inappropriate to check whether a header is compilable, you should use
`AC_PREPROC_IFELSE` (see [Running the Preprocessor](#Running-the-Preprocessor)) instead of
these macros.

Requiring each header to compile improves the robustness of the test,
but it also requires you to make sure that the includes are
correct. Most system headers nowadays make sure to `#include`
whatever they require, or else have their dependencies satisfied by
`AC_INCLUDES_DEFAULT` (see [Default Includes](#Default-Includes)), but
see [Portability of Headers](#Header-Portability), for known exceptions. In general, if you
are looking for `bar.h`, which requires that `foo.h` be
included first if it exists, you should do something like this:

```
AC_CHECK_HEADERS([foo.h])
AC_CHECK_HEADERS([bar.h], [], [],
[#ifdef HAVE_FOO_H
# include <foo.h>
#endif
])
```

---

Next: [Structures](#Structures), Previous: [Header Files](#Header-Files), Up: [Existing Tests](#Existing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 5.7 Declarations [¶](#Declarations-1)

The following macros check for the declaration of variables and
functions. If there is no macro specifically defined to check for a
symbol you need, then you can use the general macros (see [Generic Declaration Checks](#Generic-Declarations)) or, for more complex tests, you may use
`AC_COMPILE_IFELSE` (see [Running the Compiler](#Running-the-Compiler)).

* [Particular Declaration Checks](#Particular-Declarations)
* [Generic Declaration Checks](#Generic-Declarations)

---

Next: [Generic Declaration Checks](#Generic-Declarations), Up: [Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.7.1 Particular Declaration Checks [¶](#Particular-Declaration-Checks)

There are no specific macros for declarations.

---

Previous: [Particular Declaration Checks](#Particular-Declarations), Up: [Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.7.2 Generic Declaration Checks [¶](#Generic-Declaration-Checks)

These macros are used to find declarations not covered by the “particular”
test macros.

Macro: **AC\_CHECK\_DECL**(symbol, [action-if-found], [action-if-not-found], [includes = ‘`AC_INCLUDES_DEFAULT`’]) [¶](#index-AC_005fCHECK_005fDECL-1)
:   If symbol (a function, variable, or constant) is not declared in
    includes and a declaration is needed, run the shell commands
    action-if-not-found, otherwise action-if-found.
    includes is a series of include directives, defaulting to
    `AC_INCLUDES_DEFAULT` (see [Default Includes](#Default-Includes)), which are used
    prior to the declaration under test.

    This macro actually tests whether symbol is defined as a macro or
    can be used as an r-value, not whether it is really declared, because it
    is much safer to avoid introducing extra declarations when they are not
    needed. In order to facilitate use of C++ and overloaded function
    declarations, it is possible to specify function argument types in
    parentheses for types which can be zero-initialized:

    ```
    AC_CHECK_DECL([basename(char *)])
    ```

    This macro caches its result in the `ac_cv_have_decl_symbol`
    variable, with characters not suitable for a variable name mapped to
    underscores.

Macro: **AC\_CHECK\_DECLS**(symbols, [action-if-found], [action-if-not-found], [includes = ‘`AC_INCLUDES_DEFAULT`’]) [¶](#index-AC_005fCHECK_005fDECLS-1)
:   For each of the symbols (*comma*-separated list with optional
    function argument types for C++ overloads), define
    `HAVE_DECL_symbol` (in all capitals) to ‘`1`’ if
    symbol is declared, otherwise to ‘`0`’. If
    action-if-not-found is given, it is additional shell code to
    execute when one of the function declarations is needed, otherwise
    action-if-found is executed.

    includes is a series of include directives, defaulting to
    `AC_INCLUDES_DEFAULT` (see [Default Includes](#Default-Includes)), which are used
    prior to the declarations under test.

    This macro uses an M4 list as first argument:

    ```
    AC_CHECK_DECLS([strdup])
    AC_CHECK_DECLS([strlen])
    AC_CHECK_DECLS([malloc, realloc, calloc, free])
    AC_CHECK_DECLS([j0], [], [], [[#include <math.h>]])
    AC_CHECK_DECLS([[basename(char *)], [dirname(char *)]])
    ```

    Unlike the other ‘`AC_CHECK_*S`’ macros, when a symbol is not
    declared, `HAVE_DECL_symbol` is defined to ‘`0`’ instead
    of leaving `HAVE_DECL_symbol` undeclared. When you are
    *sure* that the check was performed, use
    `HAVE_DECL_symbol` in `#if`:

    ```
    #if !HAVE_DECL_SYMBOL
    extern char *symbol;
    #endif
    ```

    If the test may have not been performed, however, because it is safer
    *not* to declare a symbol than to use a declaration that conflicts
    with the system’s one, you should use:

    ```
    #if defined HAVE_DECL_MALLOC && !HAVE_DECL_MALLOC
    void *malloc (size_t *s);
    #endif
    ```

    You fall into the second category only in extreme situations: either
    your files may be used without being configured, or they are used during
    the configuration. In most cases the traditional approach is enough.

    This macro caches its results in `ac_cv_have_decl_symbol`
    variables, with characters not suitable for a variable name mapped to
    underscores.

Macro: **AC\_CHECK\_DECLS\_ONCE**(symbols) [¶](#index-AC_005fCHECK_005fDECLS_005fONCE-1)
:   For each of the symbols (*comma*-separated list), define
    `HAVE_DECL_symbol` (in all capitals) to ‘`1`’ if
    symbol is declared in the default include files, otherwise to
    ‘`0`’. This is a once-only variant of `AC_CHECK_DECLS`. It
    generates the checking code at most once, so that `configure` is
    smaller and faster; but the checks cannot be conditionalized and are
    always done once, early during the `configure` run.

---

Next: [Types](#Types), Previous: [Declarations](#Declarations), Up: [Existing Tests](#Existing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 5.8 Structures [¶](#Structures-1)

The following macros check for the presence of certain members in C
structures. If there is no macro specifically defined to check for a
member you need, then you can use the general structure-member macros
(see [Generic Structure Checks](#Generic-Structures)) or, for more complex tests, you may use
`AC_COMPILE_IFELSE` (see [Running the Compiler](#Running-the-Compiler)).

* [Particular Structure Checks](#Particular-Structures)
* [Generic Structure Checks](#Generic-Structures)

---

Next: [Generic Structure Checks](#Generic-Structures), Up: [Structures](#Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.8.1 Particular Structure Checks [¶](#Particular-Structure-Checks)

The following macros check for certain structures or structure members.

Macro: **AC\_STRUCT\_DIRENT\_D\_INO** [¶](#index-AC_005fSTRUCT_005fDIRENT_005fD_005fINO-1)
:   Perform all the actions of `AC_HEADER_DIRENT` (see [Particular Header Checks](#Particular-Headers)). Then, if `struct dirent` contains a `d_ino`
    member, define `HAVE_STRUCT_DIRENT_D_INO`.

    `HAVE_STRUCT_DIRENT_D_INO` indicates only the presence of
    `d_ino`, not whether its contents are always reliable.
    Traditionally, a zero `d_ino` indicated a deleted directory entry,
    though current systems hide this detail from the user and never return
    zero `d_ino` values.
    Many current systems report an incorrect `d_ino` for a directory
    entry that is a mount point.

Macro: **AC\_STRUCT\_DIRENT\_D\_TYPE** [¶](#index-AC_005fSTRUCT_005fDIRENT_005fD_005fTYPE-1)
:   Perform all the actions of `AC_HEADER_DIRENT` (see [Particular Header Checks](#Particular-Headers)). Then, if `struct dirent` contains a `d_type`
    member, define `HAVE_STRUCT_DIRENT_D_TYPE`.

Macro: **AC\_STRUCT\_ST\_BLOCKS** [¶](#index-AC_005fSTRUCT_005fST_005fBLOCKS-1)
:   If `struct stat` contains an `st_blocks` member, define
    `HAVE_STRUCT_STAT_ST_BLOCKS`. Otherwise, require an
    `AC_LIBOBJ` replacement of ‘`fileblocks`’. The former name,
    `HAVE_ST_BLOCKS` is to be avoided, as its support will cease in the
    future.

    This macro caches its result in the `ac_cv_member_struct_stat_st_blocks`
    variable.

Macro: **AC\_STRUCT\_TM** [¶](#index-AC_005fSTRUCT_005fTM-1)
:   If `time.h` does not define `struct tm`, define
    `TM_IN_SYS_TIME`, which means that including `sys/time.h`
    had better define `struct tm`.

    This macro is obsolescent, as `time.h` defines `struct tm` in
    current systems. New programs need not use this macro.

Macro: **AC\_STRUCT\_TIMEZONE** [¶](#index-AC_005fSTRUCT_005fTIMEZONE-1)
:   Figure out how to get the current timezone. If `struct tm` has a
    `tm_zone` member, define `HAVE_STRUCT_TM_TM_ZONE` (and the
    obsoleted `HAVE_TM_ZONE`). Otherwise, if the external array
    `tzname` is found, define `HAVE_TZNAME`; if it is declared,
    define `HAVE_DECL_TZNAME`.

---

Previous: [Particular Structure Checks](#Particular-Structures), Up: [Structures](#Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.8.2 Generic Structure Checks [¶](#Generic-Structure-Checks)

These macros are used to find structure members not covered by the
“particular” test macros.

Macro: **AC\_CHECK\_MEMBER**(aggregate.member, [action-if-found], [action-if-not-found], [includes = ‘`AC_INCLUDES_DEFAULT`’]) [¶](#index-AC_005fCHECK_005fMEMBER-1)
:   Check whether member is a member of the aggregate aggregate.
    If no includes are specified, the default includes are used
    (see [Default Includes](#Default-Includes)).

    ```
    AC_CHECK_MEMBER([struct passwd.pw_gecos], [],
                    [AC_MSG_ERROR([we need 'passwd.pw_gecos'])],
                    [[#include <pwd.h>]])
    ```

    You can use this macro for submembers:

    ```
    AC_CHECK_MEMBER(struct top.middle.bot)
    ```

    This macro caches its result in the
    `ac_cv_member_aggregate_member` variable, with
    characters not suitable for a variable name mapped to underscores.

Macro: **AC\_CHECK\_MEMBERS**(members, [action-if-found], [action-if-not-found], [includes = ‘`AC_INCLUDES_DEFAULT`’]) [¶](#index-AC_005fCHECK_005fMEMBERS-1)
:   Check for the existence of each ‘`aggregate.member`’ of
    members using the previous macro. When member belongs to
    aggregate, define `HAVE_aggregate_member` (in all
    capitals, with spaces and dots replaced by underscores). If
    action-if-found is given, it is executed for each of the found
    members. If action-if-not-found is given, it is executed for each
    of the members that could not be found.

    includes is a series of include directives, defaulting to
    `AC_INCLUDES_DEFAULT` (see [Default Includes](#Default-Includes)), which are used
    prior to the members under test.

    This macro uses M4 lists:

    ```
    AC_CHECK_MEMBERS([struct stat.st_rdev, struct stat.st_blksize])
    ```

---

Next: [Compilers and Preprocessors](#Compilers-and-Preprocessors), Previous: [Structures](#Structures), Up: [Existing Tests](#Existing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 5.9 Types [¶](#Types-1)

The following macros check for C types, either builtin or typedefs. If
there is no macro specifically defined to check for a type you need, and
you don’t need to check for any special properties of it, then you can
use a general type-check macro.

* [Particular Type Checks](#Particular-Types)
* [Generic Type Checks](#Generic-Types)

---

Next: [Generic Type Checks](#Generic-Types), Up: [Types](#Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.9.1 Particular Type Checks [¶](#Particular-Type-Checks)

These macros check for particular C types in `sys/types.h`,
`stdlib.h`, `stdint.h`, `inttypes.h` and others, if they
exist.

The Gnulib `stdint` module is an alternate way to define many of
these symbols; it is useful if you prefer your code to assume a
C99-or-better environment. See [Gnulib](#Gnulib).

Macro: **AC\_TYPE\_GETGROUPS** [¶](#index-AC_005fTYPE_005fGETGROUPS-1)
:   Define `GETGROUPS_T` to be whichever of `gid_t` or `int`
    is the base type of the array argument to `getgroups`.

    This macro caches the base type in the `ac_cv_type_getgroups`
    variable.

Macro: **AC\_TYPE\_INT8\_T** [¶](#index-AC_005fTYPE_005fINT8_005fT-1)
:   If `stdint.h` or `inttypes.h` does not define the type
    `int8_t`, define `int8_t` to a signed
    integer type that is exactly 8 bits wide and that uses two’s complement
    representation, if such a type exists.
    If you are worried about porting to hosts that lack such a type, you can
    use the results of this macro as follows:

    ```
    #if HAVE_STDINT_H
    # include <stdint.h>
    #endif
    #if defined INT8_MAX || defined int8_t
     code using int8_t
    #else
     complicated alternative using >8-bit 'signed char'
    #endif
    ```

    This macro caches the type in the `ac_cv_c_int8_t` variable.

Macro: **AC\_TYPE\_INT16\_T** [¶](#index-AC_005fTYPE_005fINT16_005fT-1)
:   This is like `AC_TYPE_INT8_T`, except for 16-bit integers.

Macro: **AC\_TYPE\_INT32\_T** [¶](#index-AC_005fTYPE_005fINT32_005fT-1)
:   This is like `AC_TYPE_INT8_T`, except for 32-bit integers.

Macro: **AC\_TYPE\_INT64\_T** [¶](#index-AC_005fTYPE_005fINT64_005fT-1)
:   This is like `AC_TYPE_INT8_T`, except for 64-bit integers.

Macro: **AC\_TYPE\_INTMAX\_T** [¶](#index-AC_005fTYPE_005fINTMAX_005fT-1)
:   If `stdint.h` or `inttypes.h` defines the type `intmax_t`,
    define `HAVE_INTMAX_T`. Otherwise, define `intmax_t` to the
    widest signed integer type.

Macro: **AC\_TYPE\_INTPTR\_T** [¶](#index-AC_005fTYPE_005fINTPTR_005fT-1)
:   If `stdint.h` or `inttypes.h` defines the type `intptr_t`,
    define `HAVE_INTPTR_T`. Otherwise, define `intptr_t` to a
    signed integer type wide enough to hold a pointer, if such a type
    exists.

Macro: **AC\_TYPE\_LONG\_DOUBLE** [¶](#index-AC_005fTYPE_005fLONG_005fDOUBLE-1)
:   If the C compiler supports a working `long double` type, define
    `HAVE_LONG_DOUBLE`. The `long double` type might have the
    same range and precision as `double`.

    This macro caches its result in the `ac_cv_type_long_double`
    variable.

    This macro is obsolescent, as current C compilers support `long
    double`. New programs need not use this macro.

Macro: **AC\_TYPE\_LONG\_DOUBLE\_WIDER** [¶](#index-AC_005fTYPE_005fLONG_005fDOUBLE_005fWIDER-1)
:   If the C compiler supports a working `long double` type with more
    range or precision than the `double` type, define
    `HAVE_LONG_DOUBLE_WIDER`.

    This macro caches its result in the `ac_cv_type_long_double_wider`
    variable.

Macro: **AC\_TYPE\_LONG\_LONG\_INT** [¶](#index-AC_005fTYPE_005fLONG_005fLONG_005fINT-1)
:   If the C compiler supports a working `long long int` type, define
    `HAVE_LONG_LONG_INT`. However, this test does not test
    `long long int` values in preprocessor `#if` expressions,
    because too many compilers mishandle such expressions.
    See [Preprocessor Arithmetic](#Preprocessor-Arithmetic).

    This macro caches its result in the `ac_cv_type_long_long_int`
    variable.

Macro: **AC\_TYPE\_MBSTATE\_T** [¶](#index-AC_005fTYPE_005fMBSTATE_005fT-1)
:   Define `HAVE_MBSTATE_T` if `<wchar.h>` declares the
    `mbstate_t` type. Also, define `mbstate_t` to be a type if
    `<wchar.h>` does not declare it.

    This macro caches its result in the `ac_cv_type_mbstate_t`
    variable.

Macro: **AC\_TYPE\_MODE\_T** [¶](#index-AC_005fTYPE_005fMODE_005fT-1)
:   Define `mode_t` to a suitable type, if standard headers do not
    define it.

    This macro caches its result in the `ac_cv_type_mode_t` variable.

Macro: **AC\_TYPE\_OFF\_T** [¶](#index-AC_005fTYPE_005fOFF_005fT-1)
:   Define `off_t` to a suitable type, if standard headers do not
    define it.

    This macro caches its result in the `ac_cv_type_off_t` variable.

Macro: **AC\_TYPE\_PID\_T** [¶](#index-AC_005fTYPE_005fPID_005fT-1)
:   Define `pid_t` to a suitable type, if standard headers do not
    define it.

    This macro caches its result in the `ac_cv_type_pid_t` variable.

Macro: **AC\_TYPE\_SIZE\_T** [¶](#index-AC_005fTYPE_005fSIZE_005fT-1)
:   Define `size_t` to a suitable type, if standard headers do not
    define it.

    This macro caches its result in the `ac_cv_type_size_t` variable.

Macro: **AC\_TYPE\_SSIZE\_T** [¶](#index-AC_005fTYPE_005fSSIZE_005fT-1)
:   Define `ssize_t` to a suitable type, if standard headers do not
    define it.

    This macro caches its result in the `ac_cv_type_ssize_t` variable.

Macro: **AC\_TYPE\_UID\_T** [¶](#index-AC_005fTYPE_005fUID_005fT-1)
:   Define `uid_t` and `gid_t` to suitable types, if standard
    headers do not define them.

    This macro caches its result in the `ac_cv_type_uid_t` variable.

Macro: **AC\_TYPE\_UINT8\_T** [¶](#index-AC_005fTYPE_005fUINT8_005fT-1)
:   If `stdint.h` or `inttypes.h` does not define the type
    `uint8_t`, define `uint8_t` to an
    unsigned integer type that is exactly 8 bits wide, if such a type
    exists.
    This is like `AC_TYPE_INT8_T`, except for unsigned integers.

Macro: **AC\_TYPE\_UINT16\_T** [¶](#index-AC_005fTYPE_005fUINT16_005fT-1)
:   This is like `AC_TYPE_UINT8_T`, except for 16-bit integers.

Macro: **AC\_TYPE\_UINT32\_T** [¶](#index-AC_005fTYPE_005fUINT32_005fT-1)
:   This is like `AC_TYPE_UINT8_T`, except for 32-bit integers.

Macro: **AC\_TYPE\_UINT64\_T** [¶](#index-AC_005fTYPE_005fUINT64_005fT-1)
:   This is like `AC_TYPE_UINT8_T`, except for 64-bit integers.

Macro: **AC\_TYPE\_UINTMAX\_T** [¶](#index-AC_005fTYPE_005fUINTMAX_005fT-1)
:   If `stdint.h` or `inttypes.h` defines the type `uintmax_t`,
    define `HAVE_UINTMAX_T`. Otherwise, define `uintmax_t` to the
    widest unsigned integer type.

Macro: **AC\_TYPE\_UINTPTR\_T** [¶](#index-AC_005fTYPE_005fUINTPTR_005fT-1)
:   If `stdint.h` or `inttypes.h` defines the type `uintptr_t`,
    define `HAVE_UINTPTR_T`. Otherwise, define `uintptr_t` to an
    unsigned integer type wide enough to hold a pointer, if such a type
    exists.

Macro: **AC\_TYPE\_UNSIGNED\_LONG\_LONG\_INT** [¶](#index-AC_005fTYPE_005fUNSIGNED_005fLONG_005fLONG_005fINT-1)
:   If the C compiler supports a working `unsigned long long int` type,
    define `HAVE_UNSIGNED_LONG_LONG_INT`. However, this test does not test
    `unsigned long long int` values in preprocessor `#if` expressions,
    because too many compilers mishandle such expressions.
    See [Preprocessor Arithmetic](#Preprocessor-Arithmetic).

    This macro caches its result in the `ac_cv_type_unsigned_long_long_int`
    variable.

---

Previous: [Particular Type Checks](#Particular-Types), Up: [Types](#Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.9.2 Generic Type Checks [¶](#Generic-Type-Checks)

These macros are used to check for types not covered by the “particular”
test macros.

Macro: **AC\_CHECK\_TYPE**(type, [action-if-found], [action-if-not-found], [includes = ‘`AC_INCLUDES_DEFAULT`’]) [¶](#index-AC_005fCHECK_005fTYPE-2)
:   Check whether type is defined. It may be a compiler builtin type
    or defined by the includes. includes is a series of include
    directives, defaulting to `AC_INCLUDES_DEFAULT` (see [Default Includes](#Default-Includes)), which are used prior to the type under test.

    In C, type must be a type-name, so that the expression ‘`sizeof
    (type)`’ is valid (but ‘`sizeof ((type))`’ is not). The
    same test is applied when compiling for C++, which means that in C++
    type should be a type-id and should not be an anonymous
    ‘`struct`’ or ‘`union`’.

    This macro caches its result in the `ac_cv_type_type`
    variable, with ‘`*`’ mapped to ‘`p`’ and other characters not
    suitable for a variable name mapped to underscores.

Macro: **AC\_CHECK\_TYPES**(types, [action-if-found], [action-if-not-found], [includes = ‘`AC_INCLUDES_DEFAULT`’]) [¶](#index-AC_005fCHECK_005fTYPES-1)
:   For each type of the types that is defined, define
    `HAVE_type` (in all capitals). Each type must follow
    the rules of `AC_CHECK_TYPE`. If no includes are
    specified, the default includes are used (see [Default Includes](#Default-Includes)). If
    action-if-found is given, it is additional shell code to execute
    when one of the types is found. If action-if-not-found is given,
    it is executed when one of the types is not found.

    This macro uses M4 lists:

    ```
    AC_CHECK_TYPES([ptrdiff_t])
    AC_CHECK_TYPES([unsigned long long int, uintmax_t])
    AC_CHECK_TYPES([float_t], [], [], [[#include <math.h>]])
    ```

Autoconf, up to 2.13, used to provide to another version of
`AC_CHECK_TYPE`, broken by design. In order to keep backward
compatibility, a simple heuristic, quite safe but not totally, is
implemented. In case of doubt, read the documentation of the former
`AC_CHECK_TYPE`, see [Obsolete Macros](#Obsolete-Macros).

---

Next: [System Services](#System-Services), Previous: [Types](#Types), Up: [Existing Tests](#Existing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
