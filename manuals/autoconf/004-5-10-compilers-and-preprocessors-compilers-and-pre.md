### 5.10 Compilers and Preprocessors [¶](#Compilers-and-Preprocessors-1)

All the tests for compilers (`AC_PROG_CC`, `AC_PROG_CXX`,
`AC_PROG_F77`) define the output variable `EXEEXT` based on
the output of the compiler, typically to the empty string if
POSIX and ‘`.exe`’ if a DOS variant.

They also define the output variable `OBJEXT` based on the
output of the compiler, after `.c` files have been excluded, typically
to ‘`o`’ if POSIX, ‘`obj`’ if a DOS variant.

If the compiler being used does not produce executables, the tests fail. If
the executables can’t be run, and cross-compilation is not enabled, they
fail too. See [Manual Configuration](#Manual-Configuration), for more on support for cross
compiling.

* [Specific Compiler Characteristics](#Specific-Compiler-Characteristics)
* [Generic Compiler Characteristics](#Generic-Compiler-Characteristics)
* [C Compiler Characteristics](#C-Compiler)
* [C++ Compiler Characteristics](#C_002b_002b-Compiler)
* [Objective C Compiler Characteristics](#Objective-C-Compiler)
* [Objective C++ Compiler Characteristics](#Objective-C_002b_002b-Compiler)
* [Erlang Compiler and Interpreter Characteristics](#Erlang-Compiler-and-Interpreter)
* [Fortran Compiler Characteristics](#Fortran-Compiler)
* [Go Compiler Characteristics](#Go-Compiler)
* [Algol 68 Compiler](#Algol-68-Compiler)

---

Next: [Generic Compiler Characteristics](#Generic-Compiler-Characteristics), Up: [Compilers and Preprocessors](#Compilers-and-Preprocessors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.10.1 Specific Compiler Characteristics [¶](#Specific-Compiler-Characteristics-1)

Some compilers exhibit different behaviors.

Static/Dynamic Expressions
:   Autoconf relies on a trick to extract one bit of information from the C
    compiler: using negative array sizes. For instance the following
    excerpt of a C source demonstrates how to test whether ‘`int`’ objects are 4
    bytes wide:

    ```
    static int test_array[sizeof (int) == 4 ? 1 : -1];
    ```

    To our knowledge, there is a single compiler that does not support this
    trick: the HP C compilers (the real ones, not only the
    “bundled”) on HP-UX 11.00.
    They incorrectly reject the above program with the diagnostic
    “Variable-length arrays cannot have static storage.”
    This bug comes from HP compilers’ mishandling of `sizeof (int)`,
    not from the `? 1 : -1`, and
    Autoconf works around this problem by casting `sizeof (int)` to
    `long int` before comparing it.

---

Next: [C Compiler Characteristics](#C-Compiler), Previous: [Specific Compiler Characteristics](#Specific-Compiler-Characteristics), Up: [Compilers and Preprocessors](#Compilers-and-Preprocessors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.10.2 Generic Compiler Characteristics [¶](#Generic-Compiler-Characteristics-1)

Macro: **AC\_CHECK\_SIZEOF**(type-or-expr, [unused], [includes = ‘`AC_INCLUDES_DEFAULT`’]) [¶](#index-AC_005fCHECK_005fSIZEOF-1)
:   Define `SIZEOF_type-or-expr` (see [Standard Symbols](#Standard-Symbols)) to be
    the size in bytes of type-or-expr, which may be either a type or
    an expression returning a value that has a size. If the expression
    ‘`sizeof (type-or-expr)`’ is invalid, the result is 0.
    includes is a series of include directives, defaulting to
    `AC_INCLUDES_DEFAULT` (see [Default Includes](#Default-Includes)), which are used
    prior to the expression under test.

    This macro now works even when cross-compiling. The unused
    argument was used when cross-compiling.

    For example, the call

    ```
    AC_CHECK_SIZEOF([int *])
    ```

    defines `SIZEOF_INT_P` to be 8 on x86-64 systems.

    This macro caches its result in the `ac_cv_sizeof_type-or-expr`
    variable, with ‘`*`’ mapped to ‘`p`’ and other characters not
    suitable for a variable name mapped to underscores.

Macro: **AC\_CHECK\_ALIGNOF**(type, [includes = ‘`AC_INCLUDES_DEFAULT`’]) [¶](#index-AC_005fCHECK_005fALIGNOF-1)
:   Define `ALIGNOF_type` (see [Standard Symbols](#Standard-Symbols)) to be the
    alignment in bytes of type. ‘`type y;`’ must be valid as
    a structure member declaration. If ‘`type`’ is unknown, the result
    is 0. If no includes are specified, the default includes are used
    (see [Default Includes](#Default-Includes)).

    This macro caches its result in the `ac_cv_alignof_type-or-expr`
    variable, with ‘`*`’ mapped to ‘`p`’ and other characters not
    suitable for a variable name mapped to underscores.

Macro: **AC\_COMPUTE\_INT**(var, expression, [includes = ‘`AC_INCLUDES_DEFAULT`’], [action-if-fails]) [¶](#index-AC_005fCOMPUTE_005fINT-1)
:   Store into the shell variable var the value of the integer
    expression. The
    value should fit in an initializer in a C variable of type `signed
    long`. To support cross compilation, it should be possible to evaluate
    the expression at compile-time. If no includes are specified, the
    default includes are used (see [Default Includes](#Default-Includes)).

    Execute action-if-fails if the value cannot be determined correctly.

Macro: **AC\_LANG\_WERROR** [¶](#index-AC_005fLANG_005fWERROR-1)
:   Normally Autoconf ignores warnings generated by the compiler, linker, and
    preprocessor. If this macro is used, warnings count as fatal
    errors for the current language. This macro is useful when the
    results of configuration are used where warnings are unacceptable; for
    instance, if parts of a program are built with the GCC
    `-Werror`
    option. If the whole program is built using `-Werror` it is
    often simpler to put `-Werror` in the compiler flags (`CFLAGS`,
    etc.).

Macro: **AC\_OPENMP** [¶](#index-AC_005fOPENMP-1)
:   [OpenMP](https://www.openmp.org/) specifies extensions of C, C++,
    and Fortran that simplify optimization of shared memory parallelism,
    which is a common problem on multi-core CPUs.

    If the current language is C, the macro `AC_OPENMP` sets the
    variable `OPENMP_CFLAGS` to the C compiler flags needed for
    supporting OpenMP. `OPENMP_CFLAGS` is set to empty if the
    compiler already supports OpenMP, if it has no way to activate OpenMP
    support, or if the user rejects OpenMP support by invoking
    ‘`configure`’ with the ‘`--disable-openmp`’ option.

    `OPENMP_CFLAGS` needs to be used when compiling programs, when
    preprocessing program source, and when linking programs. Therefore you
    need to add `$(OPENMP_CFLAGS)` to the `CFLAGS` of C programs
    that use OpenMP. If you preprocess OpenMP-specific C code, you also
    need to add `$(OPENMP_CFLAGS)` to `CPPFLAGS`. The presence of
    OpenMP support is revealed at compile time by the preprocessor macro
    `_OPENMP`.

    Linking a program with `OPENMP_CFLAGS` typically adds one more
    shared library to the program’s dependencies, so its use is recommended
    only on programs that actually require OpenMP.

    If the current language is C++, `AC_OPENMP` sets the variable
    `OPENMP_CXXFLAGS`, suitably for the C++ compiler. The same remarks
    hold as for C.

    If the current language is Fortran 77 or Fortran, `AC_OPENMP` sets
    the variable `OPENMP_FFLAGS` or `OPENMP_FCFLAGS`,
    respectively. Similar remarks as for C hold, except that
    `CPPFLAGS` is not used for Fortran, and no preprocessor macro
    signals OpenMP support.

    For portability, it is best to avoid spaces between ‘`#`’ and
    ‘`pragma omp`’. That is, write ‘`#pragma omp`’, not
    ‘`# pragma omp`’. The Sun WorkShop 6.2 C compiler chokes on the
    latter.

    This macro caches its result in the `ac_cv_prog_c_openmp`,
    `ac_cv_prog_cxx_openmp`, `ac_cv_prog_f77_openmp`, or
    `ac_cv_prog_fc_openmp` variable, depending on the current language.

    **Caution:** Some of the compiler options that `AC_OPENMP`
    tests, mean “enable OpenMP” to one compiler, but “write output to a
    file named `mp` or `penmp`” to other compilers. We cannot
    guarantee that the implementation of `AC_OPENMP` will not overwrite
    an existing file with either of these names.

    Therefore, as a defensive measure, a `configure` script that
    uses `AC_OPENMP` will issue an error and stop (before doing any of
    the operations that might overwrite these files) upon encountering
    either of these files in its working directory.
    `autoconf` will also issue an error if it finds either of
    these files in the same directory as a `configure.ac` that
    uses `AC_OPENMP`.

    If you have files with either of these names at the top level of your
    source tree, and you need to use `AC_OPENMP`, we recommend you
    either change their names or move them into a subdirectory.

---

Next: [C++ Compiler Characteristics](#C_002b_002b-Compiler), Previous: [Generic Compiler Characteristics](#Generic-Compiler-Characteristics), Up: [Compilers and Preprocessors](#Compilers-and-Preprocessors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.10.3 C Compiler Characteristics [¶](#C-Compiler-Characteristics)

The following macros provide ways to find and exercise a C Compiler.
There are a few constructs that ought to be avoided, but do not deserve
being checked for, since they can easily be worked around.

Don’t use lines containing solitary backslashes
:   They tickle a bug in the HP-UX C compiler (checked on
    HP-UX 10.20,
    11.00, and 11i). When given the following source:

    ```
    #ifdef __STDC__
    /\
    * A comment with backslash-newlines in it.  %{ %} *\
    \
    /
    char str[] = "\\
    " A string with backslash-newlines in it %{ %} \\
    "";
    char apostrophe = '\\
    \
    '\
    ';
    #endif
    ```

    the compiler incorrectly fails with the diagnostics “Non-terminating
    comment at end of file” and “Missing ‘`#endif`’ at end of file.”
    Removing the lines with solitary backslashes solves the problem.

Don’t compile several files at once if output matters to you
:   Some compilers, such as HP’s, report names of files being
    compiled when given more than one file operand. For instance:

    ```
    $ cc a.c b.c
    a.c:
    b.c:
    ```

    This can cause problems if you observe the output of the compiler to
    detect failures. Invoking ‘`cc -c a.c && cc -c b.c && cc -o c a.o
    b.o`’ solves the issue.

Don’t rely on correct `#line` support
:   On Solaris, `c89` (at least through Oracle Developer Studio 12.6)
    diagnoses `#line` directives whose line
    numbers are greater than 32767. Nothing in POSIX
    makes this invalid. That is why Autoconf stopped issuing
    `#line` directives.

Macro: **AC\_PROG\_CC**([compiler-search-list]) [¶](#index-AC_005fPROG_005fCC-1)
:   Determine a C compiler to use.

    If the environment variable `CC` is set, its value will be taken as
    the name of the C compiler to use. Otherwise, search for a C compiler
    under a series of likely names, trying `gcc` and `cc` first.
    Regardless, the output variable `CC` is set to the chosen compiler.

    If the optional first argument to the macro is used, it must be a
    whitespace-separated list of potential names for a C compiler,
    which overrides the built-in list.

    If no C compiler can be found, `configure` will error out.

    If the selected C compiler is found to be GNU C (regardless of
    its name), the shell variable `GCC` will be set to ‘`yes`’.
    If the shell variable `CFLAGS` was not already set, it is set
    to `-g -O2` for the GNU C compiler (`-O2` on systems
    where GCC does not accept `-g`), or `-g` for other
    compilers. `CFLAGS` is then made an output variable.
    You can override the default for `CFLAGS` by inserting a shell
    default assignment between `AC_INIT` and `AC_PROG_CC`:

    ```
    : ${CFLAGS="options"}
    ```

    where options are the appropriate set of options to use by
    default. (It is important to use this construct rather than a normal
    assignment, so that `CFLAGS` can still be overridden by the
    person building the package. See [Preset Output Variables](#Preset-Output-Variables).)

    If necessary, options are added to `CC` to enable support for
    ISO Standard C features with extensions, preferring the newest edition
    of the C standard for which detection is supported. Currently the
    newest edition Autoconf knows how to detect support for is C23. After calling
    this macro you can check whether the C compiler has been set to accept
    standard C by inspecting the shell variable `ac_prog_cc_stdc`.
    Its value is ‘`c23`’, ‘`c11`’, ‘`c99`’, or ‘`c89`’, respectively,
    if the C compiler has been set to use the 2023, 2011, 1999, or 1990 edition of
    the C standard, and ‘`no`’ if the compiler does not support compiling
    standard C at all.
    (There is no special value for the 2017 edition of the C standard,
    as it is a minor revision that does not introduce new language features.)

    The tests for standard conformance are not comprehensive. They test the
    value of `__STDC_VERSION__`, and a
    representative sample of the language features added in each version of
    the C standard. They do not examine `__STDC__`
    because some compilers by default leave it undefined.
    They do not test for variable-length arrays,
    a C99 feature that was made optional in C11;
    if you need to use this feature when available, use `AC_C_VARARRAYS`.
    They do not test the C standard library, because the C
    compiler might be generating code for a “freestanding environment”
    (in which most of the standard library is optional). If you need to know
    whether a particular C standard header exists, use `AC_CHECK_HEADER`.

    None of the options that may be added to `CC` by this macro
    enable *strict* conformance to the C standard. In particular,
    system-specific extensions are not disabled. (For example, for GNU C,
    the `-std=gnunn` options may be used, but not the
    `-std=cnn` options.)

    Many Autoconf macros use a compiler, and thus call
    ‘`AC_REQUIRE([AC_PROG_CC])`’ to ensure that the compiler has been
    determined before the body of the outermost `AC_DEFUN` macro.
    Although `AC_PROG_CC` is safe to directly expand multiple times, it
    performs certain checks (such as the proper value of `EXEEXT`) only
    on the first invocation. Therefore, care must be used when invoking
    this macro from within another macro rather than at the top level
    (see [Expanded Before Required](#Expanded-Before-Required)).

Macro: **AC\_PROG\_CC\_C\_O** [¶](#index-AC_005fPROG_005fCC_005fC_005fO-1)
:   If the C compiler does not accept the `-c` and `-o` options
    simultaneously, define `NO_MINUS_C_MINUS_O`. This macro actually
    tests both the compiler found by `AC_PROG_CC`, and, if different,
    the first `cc` in the path. The test fails if one fails. This
    macro was created for GNU Make to choose the default C compilation
    rule.

    For the compiler compiler, this macro caches its result in the
    `ac_cv_prog_cc_compiler_c_o` variable.

Macro: **AC\_PROG\_CPP** [¶](#index-AC_005fPROG_005fCPP-1)
:   Set output variable `CPP` to a command that runs the
    C preprocessor. If ‘`$CC -E`’ doesn’t work, tries `cpp` and
    `/lib/cpp`, in that order.

    It is only portable to run `CPP` on files with a `.c`
    extension.

    Some preprocessors don’t indicate missing include files by the error
    status. For such preprocessors an internal variable is set that causes
    other macros to check the standard error from the preprocessor and
    consider the test failed if any warnings have been reported.
    For most preprocessors, though, warnings do not cause include-file
    tests to fail unless `AC_PROG_CPP_WERROR` is also specified.

Macro: **AC\_PROG\_CPP\_WERROR** [¶](#index-AC_005fPROG_005fCPP_005fWERROR-1)
:   This acts like `AC_PROG_CPP`, except it treats warnings from the
    preprocessor as errors even if the preprocessor exit status indicates
    success. This is useful for avoiding headers that generate mandatory
    warnings, such as deprecation notices.

The following macros check for C compiler or machine architecture
features. To check for characteristics not listed here, use
`AC_COMPILE_IFELSE` (see [Running the Compiler](#Running-the-Compiler)) or
`AC_RUN_IFELSE` (see [Checking Runtime Behavior](#Runtime)).

Macro: **AC\_C\_BACKSLASH\_A** [¶](#index-AC_005fC_005fBACKSLASH_005fA-1)
:   Define ‘`HAVE_C_BACKSLASH_A`’ to 1 if the C compiler understands
    ‘`\a`’.

    This macro is obsolescent, as current C compilers understand ‘`\a`’.
    New programs need not use this macro.

Macro: **AC\_C\_BIGENDIAN**([action-if-true], [action-if-false], [action-if-unknown], [action-if-universal]) [¶](#index-AC_005fC_005fBIGENDIAN-1)
:   If words are stored with the most significant byte first (like Motorola
    and SPARC CPUs), execute action-if-true. If words are stored with
    the least significant byte first (like Intel and VAX CPUs), execute
    action-if-false.

    This macro runs a test-case if endianness cannot be determined from the
    system header files. When cross-compiling, the test-case is not run but
    grep’ed for some magic values. action-if-unknown is executed if
    the latter case fails to determine the byte sex of the host system.

    In some cases a single run of a compiler can generate code for multiple
    architectures. This can happen, for example, when generating Mac OS X
    universal binary files, which work on both PowerPC and Intel
    architectures. In this case, the different variants might be for
    architectures with differing endianness. If
    `configure` detects this, it executes action-if-universal
    instead of action-if-unknown.

    The default for action-if-true is to define
    ‘`WORDS_BIGENDIAN`’. The default for action-if-false is to do
    nothing. The default for action-if-unknown is to
    abort configure and tell the installer how to bypass this test.
    And finally, the default for action-if-universal is to ensure that
    ‘`WORDS_BIGENDIAN`’ is defined if and only if a universal build is
    detected and the current code is big-endian; this default works only if
    `autoheader` is used (see [Using `autoheader` to Create `config.h.in`](#autoheader-Invocation)).

    If you use this macro without specifying action-if-universal, you
    should also use `AC_CONFIG_HEADERS`; otherwise
    ‘`WORDS_BIGENDIAN`’ may be set incorrectly for Mac OS X universal
    binary files.

Macro: **AC\_C\_CONST** [¶](#index-AC_005fC_005fCONST-1)
:   If the C compiler does not fully support the `const` keyword,
    define `const` to be empty. Some C compilers that do
    not define `__STDC__` do support `const`; some compilers that
    define `__STDC__` do not completely support `const`. Programs
    can simply use `const` as if every C compiler supported it; for
    those that don’t, the makefile or configuration header file
    defines it as empty.

    Occasionally installers use a C++ compiler to compile C code, typically
    because they lack a C compiler. This causes problems with `const`,
    because C and C++ treat `const` differently. For example:

    ```
    const int foo;
    ```

    is valid in C but not in C++. These differences unfortunately cannot be
    papered over by defining `const` to be empty.

    If `autoconf` detects this situation, it leaves `const` alone,
    as this generally yields better results in practice. However, using a
    C++ compiler to compile C code is not recommended or supported, and
    installers who run into trouble in this area should get a C compiler
    like GCC to compile their C code.

    This macro caches its result in the `ac_cv_c_const` variable.

    This macro is obsolescent, as current C compilers support `const`.
    New programs need not use this macro.

Macro: **AC\_C\_\_GENERIC** [¶](#index-AC_005fC_005f_005fGENERIC-1)
:   If the C compiler supports C11-style generic selection using the
    `_Generic` keyword, define `HAVE_C__GENERIC`.

Macro: **AC\_C\_RESTRICT** [¶](#index-AC_005fC_005fRESTRICT-1)
:   If the C compiler recognizes a variant spelling for the `restrict`
    keyword (`__restrict`, `__restrict__`, or `_Restrict`),
    then define `restrict` to that; this is more likely to do the right
    thing with compilers that support language variants where plain
    `restrict` is not a keyword. Otherwise, if the C compiler
    recognizes the `restrict` keyword, don’t do anything.
    Otherwise, define `restrict` to be empty.
    Thus, programs may simply use `restrict` as if every C compiler
    supported it; for those that do not, the makefile
    or configuration header defines it away.

    Although support in C++ for the `restrict` keyword is not
    required, several C++ compilers do accept the keyword.
    This macro works for them, too.

    This macro caches ‘`no`’ in the `ac_cv_c_restrict` variable
    if `restrict` is not supported, and a supported spelling otherwise.

Macro: **AC\_C\_VOLATILE** [¶](#index-AC_005fC_005fVOLATILE-1)
:   If the C compiler does not understand the keyword `volatile`,
    define `volatile` to be empty. Programs can simply use
    `volatile` as if every C compiler supported it; for those that do
    not, the makefile or configuration header defines it as
    empty.

    If the correctness of your program depends on the semantics of
    `volatile`, simply defining it to be empty does, in a sense, break
    your code. However, given that the compiler does not support
    `volatile`, you are at its mercy anyway. At least your
    program compiles, when it wouldn’t before.
    See [Volatile Objects](#Volatile-Objects), for more about `volatile`.

    This macro is obsolescent, as current C compilers support `volatile`.
    New programs need not use this macro.

Macro: **AC\_C\_INLINE** [¶](#index-AC_005fC_005fINLINE-1)
:   If the C compiler supports the keyword `inline`, do nothing.
    Otherwise define `inline` to `__inline__` or `__inline`
    if it accepts one of those, otherwise define `inline` to be empty.

Macro: **AC\_C\_CHAR\_UNSIGNED** [¶](#index-AC_005fC_005fCHAR_005fUNSIGNED-1)
:   If the C type `char` is unsigned, define `__CHAR_UNSIGNED__`,
    unless the C compiler predefines it.

    These days, using this macro is not necessary. The same information can
    be determined by this portable alternative, thus avoiding the use of
    preprocessor macros in the namespace reserved for the implementation.

    ```
    #include <limits.h>
    #if CHAR_MIN == 0
    # define CHAR_UNSIGNED 1
    #endif
    ```

Macro: **AC\_C\_STRINGIZE** [¶](#index-AC_005fC_005fSTRINGIZE-1)
:   If the C preprocessor supports the stringizing operator, define
    `HAVE_STRINGIZE`. The stringizing operator is ‘`#`’ and is
    found in macros such as this:

    ```
    #define x(y) #y
    ```

    This macro is obsolescent, as current C compilers support the
    stringizing operator. New programs need not use this macro.

Macro: **AC\_C\_FLEXIBLE\_ARRAY\_MEMBER** [¶](#index-AC_005fC_005fFLEXIBLE_005fARRAY_005fMEMBER-1)
:   If the C compiler supports flexible array members, define
    `FLEXIBLE_ARRAY_MEMBER` to nothing; otherwise define it to 1.
    That way, a declaration like this:

    ```
    struct s
      {
        size_t n_vals;
        double val[FLEXIBLE_ARRAY_MEMBER];
      };
    ```

    will let applications use the “struct hack” even with compilers that
    do not support flexible array members. To allocate and use such an
    object, you can use code like this:

    ```
    size_t i;
    size_t n = compute_value_count ();
    struct s *p =
       malloc (offsetof (struct s, val)
               + n * sizeof (double));
    p->n_vals = n;
    for (i = 0; i < n; i++)
      p->val[i] = compute_value (i);
    ```

Macro: **AC\_C\_VARARRAYS** [¶](#index-AC_005fC_005fVARARRAYS-1)
:   If the C compiler does not support variable-length arrays, define the
    macro `__STDC_NO_VLA__` to be 1 if it is not already defined. A
    variable-length array is an array of automatic storage duration whose
    length is determined at run time, when the array is declared. For
    backward compatibility this macro also defines `HAVE_C_VARARRAYS`
    if the C compiler supports variable-length arrays, but this usage is
    obsolescent and new programs should use `__STDC_NO_VLA__`.

Macro: **AC\_C\_TYPEOF** [¶](#index-AC_005fC_005fTYPEOF-1)
:   If the C compiler supports GNU C’s `typeof` syntax either
    directly or
    through a different spelling of the keyword (e.g., `__typeof__`),
    define `HAVE_TYPEOF`. If the support is available only through a
    different spelling, define `typeof` to that spelling.

Macro: **AC\_C\_PROTOTYPES** [¶](#index-AC_005fC_005fPROTOTYPES-1)
:   If function prototypes are understood by the compiler (as determined by
    `AC_PROG_CC`), define `PROTOTYPES` and `__PROTOTYPES`.
    Defining `__PROTOTYPES` is for the benefit of
    header files that cannot use macros that infringe on user name space.

    This macro is obsolescent, as current C compilers support prototypes.
    New programs need not use this macro.

---

Next: [Objective C Compiler Characteristics](#Objective-C-Compiler), Previous: [C Compiler Characteristics](#C-Compiler), Up: [Compilers and Preprocessors](#Compilers-and-Preprocessors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.10.4 C++ Compiler Characteristics [¶](#C_002b_002b-Compiler-Characteristics)

Macro: **AC\_PROG\_CXX**([compiler-search-list]) [¶](#index-AC_005fPROG_005fCXX-1)
:   Determine a C++ compiler to use.

    If either the environment variable `CXX` or the environment
    variable `CCC` is set, its value will be taken as the name of a
    C++ compiler. If both are set, `CXX` is preferred. If neither
    are set, search for a C++ compiler under a series of likely names,
    trying `g++` and `c++` first. Regardless, the output
    variable `CXX` is set to the chosen compiler.

    If the optional first argument to the macro is used, it must be a
    whitespace-separated list of potential names for a C++ compiler,
    which overrides the built-in list.

    If no C++ compiler can be found, as a last resort `CXX` is set to
    `g++` (and subsequent tests will probably fail).

    If the selected C++ compiler is found to be GNU C++ (regardless of
    its name), the shell variable `GXX` will be set to ‘`yes`’.
    If the shell variable `CXXFLAGS` was not already set, it is set
    to `-g -O2` for the GNU C++ compiler (`-O2` on systems
    where G++ does not accept `-g`), or `-g` for other
    compilers. `CXXFLAGS` is then made an output variable.
    You can override the default for `CXXFLAGS` by inserting a shell
    default assignment between `AC_INIT` and `AC_PROG_CXX`:

    ```
    : ${CXXFLAGS="options"}
    ```

    where options are the appropriate set of options to use by
    default. (It is important to use this construct rather than a normal
    assignment, so that `CXXFLAGS` can still be overridden by the
    person building the package. See [Preset Output Variables](#Preset-Output-Variables).)

Macro: **AC\_PROG\_CXXCPP** [¶](#index-AC_005fPROG_005fCXXCPP-1)
:   Set output variable `CXXCPP` to a command that runs the C++
    preprocessor. If ‘`$CXX -E`’ doesn’t work, tries `cpp` and
    `/lib/cpp`, in that order. Because of this fallback, `CXXCPP`
    may or may not set C++-specific predefined macros (such as `__cplusplus`).

    It is portable to run `CXXCPP` only on files with a `.c`,
    `.C`, `.cc`, or `.cpp` extension.

    Some preprocessors don’t indicate missing include files by the error
    status. For such preprocessors an internal variable is set that causes
    other macros to check the standard error from the preprocessor and
    consider the test failed if any warnings have been reported. However,
    it is not known whether such broken preprocessors exist for C++.

Macro: **AC\_PROG\_CXX\_C\_O** [¶](#index-AC_005fPROG_005fCXX_005fC_005fO-1)
:   Test whether the C++ compiler accepts the options `-c` and
    `-o` simultaneously, and define `CXX_NO_MINUS_C_MINUS_O`,
    if it does not.

---

Next: [Objective C++ Compiler Characteristics](#Objective-C_002b_002b-Compiler), Previous: [C++ Compiler Characteristics](#C_002b_002b-Compiler), Up: [Compilers and Preprocessors](#Compilers-and-Preprocessors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.10.5 Objective C Compiler Characteristics [¶](#Objective-C-Compiler-Characteristics)

Macro: **AC\_PROG\_OBJC**([compiler-search-list]) [¶](#index-AC_005fPROG_005fOBJC-1)
:   Determine an Objective C compiler to use. If `OBJC` is not already
    set in the environment, check for Objective C compilers. Set output
    variable `OBJC` to the name of the compiler found.

    This macro may, however, be invoked with an optional first argument
    which, if specified, must be a blank-separated list of Objective C compilers to
    search for. This just gives the user an opportunity to specify an
    alternative search list for the Objective C compiler. For example, if you
    didn’t like the default order, then you could invoke `AC_PROG_OBJC`
    like this:

    ```
    AC_PROG_OBJC([gcc objcc objc])
    ```

    If using a compiler that supports GNU Objective C, set shell variable
    `GOBJC` to ‘`yes`’. If output variable `OBJCFLAGS` was not
    already set, set it to `-g -O2` for a GNU Objective C
    compiler (`-O2` on systems where the compiler does not accept
    `-g`), or `-g` for other compilers.

Macro: **AC\_PROG\_OBJCPP** [¶](#index-AC_005fPROG_005fOBJCPP-1)
:   Set output variable `OBJCPP` to a command that runs the Objective C
    preprocessor. If ‘`$OBJC -E`’ doesn’t work, tries `cpp` and
    `/lib/cpp`, in that order. Because of this fallback, `CXXCPP`
    may or may not set Objective-C-specific predefined macros (such as
    `__OBJC__`).

---

Next: [Erlang Compiler and Interpreter Characteristics](#Erlang-Compiler-and-Interpreter), Previous: [Objective C Compiler Characteristics](#Objective-C-Compiler), Up: [Compilers and Preprocessors](#Compilers-and-Preprocessors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.10.6 Objective C++ Compiler Characteristics [¶](#Objective-C_002b_002b-Compiler-Characteristics)

Macro: **AC\_PROG\_OBJCXX**([compiler-search-list]) [¶](#index-AC_005fPROG_005fOBJCXX-1)
:   Determine an Objective C++ compiler to use. If `OBJCXX` is not already
    set in the environment, check for Objective C++ compilers. Set output
    variable `OBJCXX` to the name of the compiler found.

    This macro may, however, be invoked with an optional first argument
    which, if specified, must be a blank-separated list of Objective C++ compilers
    to search for. This just gives the user an opportunity to specify an
    alternative search list for the Objective C++ compiler. For example, if you
    didn’t like the default order, then you could invoke `AC_PROG_OBJCXX`
    like this:

    ```
    AC_PROG_OBJCXX([gcc g++ objcc++ objcxx])
    ```

    If using a compiler that supports GNU Objective C++, set shell variable
    `GOBJCXX` to ‘`yes`’. If output variable `OBJCXXFLAGS` was not
    already set, set it to `-g -O2` for a GNU Objective C++
    compiler (`-O2` on systems where the compiler does not accept
    `-g`), or `-g` for other compilers.

Macro: **AC\_PROG\_OBJCXXCPP** [¶](#index-AC_005fPROG_005fOBJCXXCPP-1)
:   Set output variable `OBJCXXCPP` to a command that runs the Objective C++
    preprocessor. If ‘`$OBJCXX -E`’ doesn’t work, tries `cpp` and
    `/lib/cpp`, in that order. Because of this fallback, `CXXCPP`
    may or may not set Objective-C++-specific predefined macros (such as
    `__cplusplus` and `__OBJC__`).

---

Next: [Fortran Compiler Characteristics](#Fortran-Compiler), Previous: [Objective C++ Compiler Characteristics](#Objective-C_002b_002b-Compiler), Up: [Compilers and Preprocessors](#Compilers-and-Preprocessors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.10.7 Erlang Compiler and Interpreter Characteristics [¶](#Erlang-Compiler-and-Interpreter-Characteristics)

Autoconf defines the following macros for determining paths to the essential
Erlang/OTP programs:

Macro: **AC\_ERLANG\_PATH\_ERLC**([value-if-not-found], [path = ‘`$PATH`’]) [¶](#index-AC_005fERLANG_005fPATH_005fERLC-1)
:   Determine an Erlang compiler to use. If `ERLC` is not already set in the
    environment, check for `erlc`. Set output variable `ERLC` to the
    complete path of the compiler command found. In addition, if `ERLCFLAGS`
    is not set in the environment, set it to an empty value.

    The two optional arguments have the same meaning as the two last arguments of
    macro `AC_PATH_PROG` for looking for the `erlc` program. For
    example, to look for `erlc` only in the `/usr/lib/erlang/bin`
    directory:

    ```
    AC_ERLANG_PATH_ERLC([not found], [/usr/lib/erlang/bin])
    ```

Macro: **AC\_ERLANG\_NEED\_ERLC**([path = ‘`$PATH`’]) [¶](#index-AC_005fERLANG_005fNEED_005fERLC-1)
:   A simplified variant of the `AC_ERLANG_PATH_ERLC` macro, that prints an
    error message and exits the `configure` script if the `erlc`
    program is not found.

Macro: **AC\_ERLANG\_PATH\_ERL**([value-if-not-found], [path = ‘`$PATH`’]) [¶](#index-AC_005fERLANG_005fPATH_005fERL-1)
:   Determine an Erlang interpreter to use. If `ERL` is not already
    set in the
    environment, check for `erl`. Set output variable `ERL` to the
    complete path of the interpreter command found.

    The two optional arguments have the same meaning as the two last arguments of
    macro `AC_PATH_PROG` for looking for the `erl` program. For
    example, to look for `erl` only in the `/usr/lib/erlang/bin`
    directory:

    ```
    AC_ERLANG_PATH_ERL([not found], [/usr/lib/erlang/bin])
    ```

Macro: **AC\_ERLANG\_NEED\_ERL**([path = ‘`$PATH`’]) [¶](#index-AC_005fERLANG_005fNEED_005fERL-1)
:   A simplified variant of the `AC_ERLANG_PATH_ERL` macro, that prints an
    error message and exits the `configure` script if the `erl`
    program is not found.

---

Next: [Go Compiler Characteristics](#Go-Compiler), Previous: [Erlang Compiler and Interpreter Characteristics](#Erlang-Compiler-and-Interpreter), Up: [Compilers and Preprocessors](#Compilers-and-Preprocessors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.10.8 Fortran Compiler Characteristics [¶](#Fortran-Compiler-Characteristics)

The Autoconf Fortran support is divided into two categories: legacy
Fortran 77 macros (`F77`), and modern Fortran macros (`FC`).
The former are intended for traditional Fortran 77 code, and have output
variables like `F77`, `FFLAGS`, and `FLIBS`. The latter
are for newer programs that can (or must) compile under the newer
Fortran standards, and have output variables like `FC`,
`FCFLAGS`, and `FCLIBS`.

Except for the macros `AC_FC_SRCEXT`, `AC_FC_FREEFORM`,
`AC_FC_FIXEDFORM`, and `AC_FC_LINE_LENGTH` (see below), the
`FC` and `F77` macros behave almost identically, and so they
are documented together in this section.

Macro: **AC\_PROG\_F77**([compiler-search-list]) [¶](#index-AC_005fPROG_005fF77-1)
:   Determine a Fortran 77 compiler to use. If `F77` is not already
    set in the environment, then check for `g77` and `f77`, and
    then some other names. Set the output variable `F77` to the name
    of the compiler found.

    This macro may, however, be invoked with an optional first argument
    which, if specified, must be a blank-separated list of Fortran 77
    compilers to search for. This just gives the user an opportunity to
    specify an alternative search list for the Fortran 77 compiler. For
    example, if you didn’t like the default order, then you could invoke
    `AC_PROG_F77` like this:

    ```
    AC_PROG_F77([fl32 f77 fort77 xlf g77 f90 xlf90])
    ```

    If using a compiler that supports GNU Fortran 77,
    set the shell variable `G77` to ‘`yes`’.
    If the output variable `FFLAGS` was not already set in the
    environment, set it to `-g -02` for `g77` (or `-O2`
    where the GNU Fortran 77 compiler does not accept `-g`), or
    `-g` for other compilers.

    The result of the GNU test is cached in the
    `ac_cv_f77_compiler_gnu` variable, acceptance of `-g` in the
    `ac_cv_prog_f77_g` variable.

Macro: **AC\_PROG\_FC**([compiler-search-list], [dialect]) [¶](#index-AC_005fPROG_005fFC-1)
:   Determine a Fortran compiler to use. If `FC` is not already set in
    the environment, then `dialect` is a hint to indicate what Fortran
    dialect to search for; the default is to search for the newest available
    dialect. Set the output variable `FC` to the name of the compiler
    found.

    By default, newer dialects are preferred over older dialects, but if
    `dialect` is specified then older dialects are preferred starting
    with the specified dialect. `dialect` can currently be one of
    Fortran 77, Fortran 90, or Fortran 95. However, this is only a hint of
    which compiler *name* to prefer (e.g., `f90` or `f95`),
    and no attempt is made to guarantee that a particular language standard
    is actually supported. Thus, it is preferable that you avoid the
    `dialect` option, and use AC\_PROG\_FC only for code compatible with
    the latest Fortran standard.

    This macro may, alternatively, be invoked with an optional first argument
    which, if specified, must be a blank-separated list of Fortran
    compilers to search for, just as in `AC_PROG_F77`.

    If using a compiler that supports GNU Fortran,
    set the shell variable `GFC` to ‘`yes`’.
    If the output variable `FCFLAGS` was not already set in the
    environment, then set it to `-g -02` for a GNU Fortran compiler (or
    `-O2` where the compiler does not accept `-g`), or
    `-g` for other compilers.

    The result of the GNU test is cached in the `ac_cv_fc_compiler_gnu`
    variable, acceptance of `-g` in the `ac_cv_prog_fc_g`
    variable.

Macro: **AC\_PROG\_F77\_C\_O** [¶](#index-AC_005fPROG_005fF77_005fC_005fO-1)

Macro: **AC\_PROG\_FC\_C\_O** [¶](#index-AC_005fPROG_005fFC_005fC_005fO-1)
:   Test whether the Fortran compiler accepts the options `-c` and
    `-o` simultaneously, and define `F77_NO_MINUS_C_MINUS_O` or
    `FC_NO_MINUS_C_MINUS_O`, respectively, if it does not.

    The result of the test is cached in the `ac_cv_prog_f77_c_o` or
    `ac_cv_prog_fc_c_o` variable, respectively.

The following macros check for Fortran compiler characteristics.
To check for characteristics not listed here, use
`AC_COMPILE_IFELSE` (see [Running the Compiler](#Running-the-Compiler)) or
`AC_RUN_IFELSE` (see [Checking Runtime Behavior](#Runtime)), making sure to first set the
current language to Fortran 77 or Fortran via `AC_LANG([Fortran 77])`
or `AC_LANG(Fortran)` (see [Language Choice](#Language-Choice)).

Macro: **AC\_F77\_LIBRARY\_LDFLAGS** [¶](#index-AC_005fF77_005fLIBRARY_005fLDFLAGS-1)

Macro: **AC\_FC\_LIBRARY\_LDFLAGS** [¶](#index-AC_005fFC_005fLIBRARY_005fLDFLAGS-1)
:   Determine the linker flags (e.g., `-L` and `-l`) for the
    *Fortran intrinsic and runtime libraries* that are required to
    successfully link a Fortran program or shared library. The output
    variable `FLIBS` or `FCLIBS` is set to these flags (which
    should be included after `LIBS` when linking).

    This macro is intended to be used in those situations when it is
    necessary to mix, e.g., C++ and Fortran source code in a single
    program or shared library (see [Mixing Fortran 77 With C and C++](https://www.gnu.org/software/automake/manual/automake.html#Mixing-Fortran-77-With-C-and-C_002b_002b) in GNU Automake).

    For example, if object files from a C++ and Fortran compiler must be
    linked together, then the C++ compiler/linker must be used for linking
    (since special C++-ish things need to happen at link time like calling
    global constructors, instantiating templates, enabling exception
    support, etc.).

    However, the Fortran intrinsic and runtime libraries must be linked in
    as well, but the C++ compiler/linker doesn’t know by default how to add
    these Fortran 77 libraries. Hence, this macro was created to determine
    these Fortran libraries.

    The macros `AC_F77_DUMMY_MAIN` and `AC_FC_DUMMY_MAIN` or
    `AC_F77_MAIN` and `AC_FC_MAIN` are probably also necessary to
    link C/C++ with Fortran; see below. Further, it is highly recommended
    that you use `AC_CONFIG_HEADERS` (see [Configuration Header Files](#Configuration-Headers))
    because the complex defines that the function wrapper macros create
    may not work with C/C++ compiler drivers.

    These macros internally compute the flag needed to verbose linking
    output and cache it in `ac_cv_prog_f77_v` or `ac_cv_prog_fc_v`
    variables, respectively. The computed linker flags are cached in
    `ac_cv_f77_libs` or `ac_cv_fc_libs`, respectively.

Macro: **AC\_F77\_DUMMY\_MAIN**([action-if-found], [action-if-not-found = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fF77_005fDUMMY_005fMAIN-1)

Macro: **AC\_FC\_DUMMY\_MAIN**([action-if-found], [action-if-not-found = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fFC_005fDUMMY_005fMAIN-1)
:   With many compilers, the Fortran libraries detected by
    `AC_F77_LIBRARY_LDFLAGS` or `AC_FC_LIBRARY_LDFLAGS` provide
    their own `main` entry function that initializes things like
    Fortran I/O, and which then calls a user-provided entry function named
    (say) `MAIN__` to run the user’s program. The
    `AC_F77_DUMMY_MAIN` and `AC_FC_DUMMY_MAIN` or
    `AC_F77_MAIN` and `AC_FC_MAIN` macros figure out how to deal with
    this interaction.

    When using Fortran for purely numerical functions (no I/O, etc.) often
    one prefers to provide one’s own `main` and skip the Fortran
    library initializations. In this case, however, one may still need to
    provide a dummy `MAIN__` routine in order to prevent linking errors
    on some systems. `AC_F77_DUMMY_MAIN` or `AC_FC_DUMMY_MAIN`
    detects whether any such routine is *required* for linking, and
    what its name is; the shell variable `F77_DUMMY_MAIN` or
    `FC_DUMMY_MAIN` holds this name, `unknown` when no solution
    was found, and `none` when no such dummy main is needed.

    By default, action-if-found defines `F77_DUMMY_MAIN` or
    `FC_DUMMY_MAIN` to the name of this routine (e.g., `MAIN__`)
    *if* it is required. action-if-not-found defaults to
    exiting with an error.

    In order to link with Fortran routines, the user’s C/C++ program should
    then include the following code to define the dummy main if it is
    needed:

    ```
    #ifdef F77_DUMMY_MAIN
    #  ifdef __cplusplus
         extern "C"
    #  endif
       int F77_DUMMY_MAIN (void) { return 1; }
    #endif
    ```

    (Replace `F77` with `FC` for Fortran instead of Fortran 77.)

    Note that this macro is called automatically from `AC_F77_WRAPPERS`
    or `AC_FC_WRAPPERS`; there is generally no need to call it
    explicitly unless one wants to change the default actions.

    The result of this macro is cached in the `ac_cv_f77_dummy_main` or
    `ac_cv_fc_dummy_main` variable, respectively.

Macro: **AC\_F77\_MAIN** [¶](#index-AC_005fF77_005fMAIN-1)

Macro: **AC\_FC\_MAIN** [¶](#index-AC_005fFC_005fMAIN-1)
:   As discussed above, many Fortran libraries allow you to provide an entry
    point called (say) `MAIN__` instead of the usual `main`, which
    is then called by a `main` function in the Fortran libraries that
    initializes things like Fortran I/O. The
    `AC_F77_MAIN` and `AC_FC_MAIN` macros detect whether it is
    *possible* to utilize such an alternate main function, and defines
    `F77_MAIN` and `FC_MAIN` to the name of the function. (If no
    alternate main function name is found, `F77_MAIN` and `FC_MAIN` are
    simply defined to `main`.)

    Thus, when calling Fortran routines from C that perform things like I/O,
    one should use this macro and declare the "main" function like so:

    ```
    #ifdef __cplusplus
      extern "C"
    #endif
    int F77_MAIN (int argc, char *argv[]);
    ```

    (Again, replace `F77` with `FC` for Fortran instead of Fortran 77.)

    The result of this macro is cached in the `ac_cv_f77_main` or
    `ac_cv_fc_main` variable, respectively.

Macro: **AC\_F77\_WRAPPERS** [¶](#index-AC_005fF77_005fWRAPPERS-1)

Macro: **AC\_FC\_WRAPPERS** [¶](#index-AC_005fFC_005fWRAPPERS-1)
:   Defines C macros `F77_FUNC (name, NAME)`, `FC_FUNC (name, NAME)`,
    `F77_FUNC_(name, NAME)`, and `FC_FUNC_(name, NAME)` to properly
    mangle the names of C/C++ identifiers, and identifiers with underscores,
    respectively, so that they match the name-mangling scheme used by the
    Fortran compiler.

    Fortran is case-insensitive, and in order to achieve this the Fortran
    compiler converts all identifiers into a canonical case and format. To
    call a Fortran subroutine from C or to write a C function that is
    callable from Fortran, the C program must explicitly use identifiers in
    the format expected by the Fortran compiler. In order to do this, one
    simply wraps all C identifiers in one of the macros provided by
    `AC_F77_WRAPPERS` or `AC_FC_WRAPPERS`. For example, suppose
    you have the following Fortran 77 subroutine:

    ```
          subroutine foobar (x, y)
          double precision x, y
          y = 3.14159 * x
          return
          end
    ```

    You would then declare its prototype in C or C++ as:

    ```
    #define FOOBAR_F77 F77_FUNC (foobar, FOOBAR)
    #ifdef __cplusplus
    extern "C"  /* prevent C++ name mangling */
    #endif
    void FOOBAR_F77 (double *x, double *y);
    ```

    Note that we pass both the lowercase and uppercase versions of the
    function name to `F77_FUNC` so that it can select the right one.
    Note also that all parameters to Fortran 77 routines are passed as
    pointers (see [Mixing Fortran 77 With C and C++](https://www.gnu.org/software/automake/manual/automake.html#Mixing-Fortran-77-With-C-and-C_002b_002b) in GNU
    Automake).

    (Replace `F77` with `FC` for Fortran instead of Fortran 77.)

    Although Autoconf tries to be intelligent about detecting the
    name-mangling scheme of the Fortran compiler, there may be Fortran
    compilers that it doesn’t support yet. In this case, the above code
    generates a compile-time error, but some other behavior
    (e.g., disabling Fortran-related features) can be induced by checking
    whether `F77_FUNC` or `FC_FUNC` is defined.

    Now, to call that routine from a C program, we would do something like:

    ```
    {
        double x = 2.7183, y;
        FOOBAR_F77 (&x, &y);
    }
    ```

    If the Fortran identifier contains an underscore (e.g., `foo_bar`),
    you should use `F77_FUNC_` or `FC_FUNC_` instead of
    `F77_FUNC` or `FC_FUNC` (with the same arguments). This is
    because some Fortran compilers mangle names differently if they contain
    an underscore.

    The name mangling scheme is encoded in the `ac_cv_f77_mangling` or
    `ac_cv_fc_mangling` cache variable, respectively, and also used for
    the `AC_F77_FUNC` and `AC_FC_FUNC` macros described below.

Macro: **AC\_F77\_FUNC**(name, [shellvar]) [¶](#index-AC_005fF77_005fFUNC-1)

Macro: **AC\_FC\_FUNC**(name, [shellvar]) [¶](#index-AC_005fFC_005fFUNC-1)
:   Given an identifier name, set the shell variable shellvar to
    hold the mangled version name according to the rules of the
    Fortran linker (see also `AC_F77_WRAPPERS` or
    `AC_FC_WRAPPERS`). shellvar is optional; if it is not
    supplied, the shell variable is simply name. The purpose of
    this macro is to give the caller a way to access the name-mangling
    information other than through the C preprocessor as above, for example,
    to call Fortran routines from some language other than C/C++.

Macro: **AC\_FC\_SRCEXT**(ext, [action-if-success], [action-if-failure = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fFC_005fSRCEXT-1)

Macro: **AC\_FC\_PP\_SRCEXT**(ext, [action-if-success], [action-if-failure = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fFC_005fPP_005fSRCEXT-1)
:   By default, the `FC` macros perform their tests using a `.f`
    extension for source-code files. Some compilers, however, only enable
    newer language features for appropriately named files, e.g., Fortran 90
    features only for `.f90` files, or preprocessing only with
    `.F` files or maybe other upper-case extensions. On the other
    hand, some other compilers expect all source files to end in `.f`
    and require special flags to support other file name extensions. The
    `AC_FC_SRCEXT` and `AC_FC_PP_SRCEXT` macros deal with these
    issues.

    The `AC_FC_SRCEXT` macro tries to get the `FC` compiler to
    accept files ending with the extension `.ext` (i.e.,
    ext does *not* contain the dot). If any special compiler
    flags are needed for this, it stores them in the output variable
    `FCFLAGS_ext`. This extension and these flags are then used
    for all subsequent `FC` tests (until `AC_FC_SRCEXT` or
    `AC_FC_PP_SRCEXT` is called another time).

    For example, you would use `AC_FC_SRCEXT(f90)` to employ the
    `.f90` extension in future tests, and it would set the
    `FCFLAGS_f90` output variable with any extra flags that are needed
    to compile such files.

    Similarly, the `AC_FC_PP_SRCEXT` macro tries to get the `FC`
    compiler to preprocess and compile files with the extension
    `.ext`. When both `fpp` and `cpp` style
    preprocessing are provided, the former is preferred, as the latter may
    treat continuation lines, `//` tokens, and white space differently
    from what some Fortran dialects expect. Conversely, if you do not want
    files to be preprocessed, use only lower-case characters in the file
    name extension. Like with `AC_FC_SRCEXT(f90)`, any needed flags
    are stored in the `FCFLAGS_ext` variable.

    The `FCFLAGS_ext` flags can *not* be simply absorbed
    into `FCFLAGS`, for two reasons based on the limitations of some
    compilers. First, only one `FCFLAGS_ext` can be used at a
    time, so files with different extensions must be compiled separately.
    Second, `FCFLAGS_ext` must appear *immediately* before
    the source-code file name when compiling. So, continuing the example
    above, you might compile a `foo.f90` file in your makefile with the
    command:

    ```
    foo.o: foo.f90
           $(FC) -c $(FCFLAGS) $(FCFLAGS_f90) '$(srcdir)/foo.f90'
    ```

    If `AC_FC_SRCEXT` or `AC_FC_PP_SRCEXT` succeeds in compiling
    files with the ext extension, it calls action-if-success
    (defaults to nothing). If it fails, and cannot find a way to make the
    `FC` compiler accept such files, it calls action-if-failure
    (defaults to exiting with an error message).

    The `AC_FC_SRCEXT` and `AC_FC_PP_SRCEXT` macros cache their
    results in `ac_cv_fc_srcext_ext` and
    `ac_cv_fc_pp_srcext_ext` variables, respectively.

Macro: **AC\_FC\_PP\_DEFINE**([action-if-success], [action-if-failure = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fFC_005fPP_005fDEFINE-1)
:   Find a flag to specify defines for preprocessed Fortran. Not all
    Fortran compilers use `-D`. Substitute `FC_DEFINE` with
    the result and call action-if-success (defaults to nothing) if
    successful, and action-if-failure (defaults to failing with an
    error message) if not.

    This macro calls `AC_FC_PP_SRCEXT([F])` in order to learn how to
    preprocess a `conftest.F` file, but restores a previously used
    Fortran source file extension afterwards again.

    The result of this test is cached in the `ac_cv_fc_pp_define`
    variable.

Macro: **AC\_FC\_FREEFORM**([action-if-success], [action-if-failure = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fFC_005fFREEFORM-1)
:   Try to ensure that the Fortran compiler (`$FC`) allows free-format
    source code (as opposed to the older fixed-format style from Fortran
    77). If necessary, it may add some additional flags to `FCFLAGS`.

    This macro is most important if you are using the default `.f`
    extension, since many compilers interpret this extension as indicating
    fixed-format source unless an additional flag is supplied. If you
    specify a different extension with `AC_FC_SRCEXT`, such as
    `.f90`, then `AC_FC_FREEFORM` ordinarily succeeds without
    modifying `FCFLAGS`. For extensions which the compiler does not
    know about, the flag set by the `AC_FC_SRCEXT` macro might let
    the compiler assume Fortran 77 by default, however.

    If `AC_FC_FREEFORM` succeeds in compiling free-form source, it
    calls action-if-success (defaults to nothing). If it fails, it
    calls action-if-failure (defaults to exiting with an error
    message).

    The result of this test, or ‘`none`’ or ‘`unknown`’, is cached in
    the `ac_cv_fc_freeform` variable.

Macro: **AC\_FC\_FIXEDFORM**([action-if-success], [action-if-failure = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fFC_005fFIXEDFORM-1)
:   Try to ensure that the Fortran compiler (`$FC`) allows the old
    fixed-format source code (as opposed to free-format style). If
    necessary, it may add some additional flags to `FCFLAGS`.

    This macro is needed for some compilers alias names like `xlf95`
    which assume free-form source code by default, and in case you want to
    use fixed-form source with an extension like `.f90` which many
    compilers interpret as free-form by default. If you specify a different
    extension with `AC_FC_SRCEXT`, such as `.f`, then
    `AC_FC_FIXEDFORM` ordinarily succeeds without modifying
    `FCFLAGS`.

    If `AC_FC_FIXEDFORM` succeeds in compiling fixed-form source, it
    calls action-if-success (defaults to nothing). If it fails, it
    calls action-if-failure (defaults to exiting with an error
    message).

    The result of this test, or ‘`none`’ or ‘`unknown`’, is cached in
    the `ac_cv_fc_fixedform` variable.

Macro: **AC\_FC\_LINE\_LENGTH**([length], [action-if-success], [action-if-failure = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fFC_005fLINE_005fLENGTH-1)
:   Try to ensure that the Fortran compiler (`$FC`) accepts long source
    code lines. The length argument may be given as 80, 132, or
    unlimited, and defaults to 132. Note that line lengths above 250
    columns are not portable, and some compilers do not accept more than 132
    columns at least for fixed format source. If necessary, it may add some
    additional flags to `FCFLAGS`.

    If `AC_FC_LINE_LENGTH` succeeds in compiling fixed-form source, it
    calls action-if-success (defaults to nothing). If it fails, it
    calls action-if-failure (defaults to exiting with an error
    message).

    The result of this test, or ‘`none`’ or ‘`unknown`’, is cached in
    the `ac_cv_fc_line_length` variable.

Macro: **AC\_FC\_CHECK\_BOUNDS**([action-if-success], [action-if-failure = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fFC_005fCHECK_005fBOUNDS-1)
:   The `AC_FC_CHECK_BOUNDS` macro tries to enable array bounds checking
    in the Fortran compiler. If successful, the action-if-success
    is called and any needed flags are added to `FCFLAGS`. Otherwise,
    action-if-failure is called, which defaults to failing with an error
    message. The macro currently requires Fortran 90 or a newer dialect.

    The result of the macro is cached in the `ac_cv_fc_check_bounds`
    variable.

Macro: **AC\_F77\_IMPLICIT\_NONE**([action-if-success], [action-if-failure = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fF77_005fIMPLICIT_005fNONE-1)

Macro: **AC\_FC\_IMPLICIT\_NONE**([action-if-success], [action-if-failure = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fFC_005fIMPLICIT_005fNONE-1)
:   Try to disallow implicit declarations in the Fortran compiler. If
    successful, action-if-success is called and any needed flags
    are added to `FFLAGS` or `FCFLAGS`, respectively. Otherwise,
    action-if-failure is called, which defaults to failing with an error
    message.

    The result of these macros are cached in the
    `ac_cv_f77_implicit_none` and `ac_cv_fc_implicit_none`
    variables, respectively.

Macro: **AC\_FC\_MODULE\_EXTENSION** [¶](#index-AC_005fFC_005fMODULE_005fEXTENSION-1)
:   Find the Fortran 90 module file name extension. Most Fortran 90
    compilers store module information in files separate from the object
    files. The module files are usually named after the name of the module
    rather than the source file name, with characters possibly turned to
    upper case, plus an extension, often `.mod`.

    Not all compilers use module files at all, or by default. The Cray
    Fortran compiler requires `-e m` in order to store and search
    module information in `.mod` files rather than in object files.
    Likewise, the Fujitsu Fortran compilers uses the `-Am` option to
    indicate how module information is stored.

    The `AC_FC_MODULE_EXTENSION` macro computes the module extension
    without the leading dot, and stores that in the `FC_MODEXT`
    variable. If the compiler does not produce module files, or the
    extension cannot be determined, `FC_MODEXT` is empty. Typically,
    the result of this macro may be used in cleanup `make` rules as
    follows:

    ```
    clean-modules:
            -test -z "$(FC_MODEXT)" || rm -f *.$(FC_MODEXT)
    ```

    The extension, or ‘`unknown`’, is cached in the
    `ac_cv_fc_module_ext` variable.

Macro: **AC\_FC\_MODULE\_FLAG**([action-if-success], [action-if-failure = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fFC_005fMODULE_005fFLAG-1)
:   Find the compiler flag to include Fortran 90 module information from
    another directory, and store that in the `FC_MODINC` variable.
    Call action-if-success (defaults to nothing) if successful, and
    set `FC_MODINC` to empty and call action-if-failure (defaults
    to exiting with an error message) if not.

    Most Fortran 90 compilers provide a way to specify module directories.
    Some have separate flags for the directory to write module files to,
    and directories to search them in, whereas others only allow writing to
    the current directory or to the first directory specified in the include
    path. Further, with some compilers, the module search path and the
    preprocessor search path can only be modified with the same flag. Thus,
    for portability, write module files to the current directory only and
    list that as first directory in the search path.

    There may be no whitespace between `FC_MODINC` and the following
    directory name, but `FC_MODINC` may contain trailing white space.
    For example, if you use Automake and would like to search `../lib`
    for module files, you can use the following:

    ```
    AM_FCFLAGS = $(FC_MODINC). $(FC_MODINC)../lib
    ```

    Inside `configure` tests, you can use:

    ```
    if test -n "$FC_MODINC"; then
      FCFLAGS="$FCFLAGS $FC_MODINC. $FC_MODINC../lib"
    fi
    ```

    The flag is cached in the `ac_cv_fc_module_flag` variable.
    The substituted value of `FC_MODINC` may refer to the
    `ac_empty` dummy placeholder empty variable, to avoid losing
    the significant trailing whitespace in a `Makefile`.

Macro: **AC\_FC\_MODULE\_OUTPUT\_FLAG**([action-if-success], [action-if-failure = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fFC_005fMODULE_005fOUTPUT_005fFLAG-1)
:   Find the compiler flag to write Fortran 90 module information to
    another directory, and store that in the `FC_MODOUT` variable.
    Call action-if-success (defaults to nothing) if successful, and
    set `FC_MODOUT` to empty and call action-if-failure (defaults
    to exiting with an error message) if not.

    Not all Fortran 90 compilers write module files, and of those that do,
    not all allow writing to a directory other than the current one, nor
    do all have separate flags for writing and reading; see the description
    of `AC_FC_MODULE_FLAG` above. If you need to be able to write to
    another directory, for maximum portability use `FC_MODOUT` before
    any `FC_MODINC` and include both the current directory and the one
    you write to in the search path:

    ```
    AM_FCFLAGS = $(FC_MODOUT)../mod $(FC_MODINC)../mod $(FC_MODINC). ...
    ```

    The flag is cached in the `ac_cv_fc_module_output_flag` variable.
    The substituted value of `FC_MODOUT` may refer to the
    `ac_empty` dummy placeholder empty variable, to avoid losing
    the significant trailing whitespace in a `Makefile`.

Macro: **AC\_F77\_CRAY\_POINTERS**([action-if-success], [action-if-failure = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fF77_005fCRAY_005fPOINTERS-1)

Macro: **AC\_FC\_CRAY\_POINTERS**([action-if-success], [action-if-failure = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fFC_005fCRAY_005fPOINTERS-1)
:   Try to ensure that the Fortran compiler (`$F77` or `$FC`)
    accepts Cray pointers. If successful, the action-if-success is
    called and any needed flags are added to `FFLAGS` or
    `FCFLAGS`. Otherwise, action-if-failure is called, which
    defaults to failing with an error message.

    Cray pointers are a non-standard extension supported by many Fortran
    compilers which allow an integer to be declared as C-like pointer to
    a target variable.

    The result of this test, or ‘`none`’ or ‘`unknown`’, is cached in
    the `ac_cv_f77_cray_ptr` or `ac_cv_fc_cray_ptr` variable.

---

Next: [Algol 68 Compiler](#Algol-68-Compiler), Previous: [Fortran Compiler Characteristics](#Fortran-Compiler), Up: [Compilers and Preprocessors](#Compilers-and-Preprocessors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.10.9 Go Compiler Characteristics [¶](#Go-Compiler-Characteristics)

Autoconf provides basic support for the Go programming language when
using the `gccgo` compiler (there is currently no support for the
`6g` and `8g` compilers).

Macro: **AC\_PROG\_GO**([compiler-search-list]) [¶](#index-AC_005fPROG_005fGO)
:   Find the Go compiler to use. Check whether the environment variable
    `GOC` is set; if so, then set output variable `GOC` to its
    value.

    Otherwise, if the macro is invoked without an argument, then search for
    a Go compiler named `gccgo`. If it is not found, then as a last
    resort set `GOC` to `gccgo`.

    This macro may be invoked with an optional first argument which, if
    specified, must be a blank-separated list of Go compilers to search for.

    If output variable `GOFLAGS` was not already set, set it to
    `-g -O2`. If your package does not like this default,
    `GOFLAGS` may be set before `AC_PROG_GO`.

---

Previous: [Go Compiler Characteristics](#Go-Compiler), Up: [Compilers and Preprocessors](#Compilers-and-Preprocessors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.10.10 Algol 68 Compiler [¶](#Algol-68-Compiler-1)

Autoconf provides basic support for the Algol 68 programming language
when using the `ga68` compiler.

Note that the GCC Algol 68 front-end is not yet integrated in the main
compiler, so it is developed and distributed off-tree. See
<https://gcc.gnu.org/wiki/Algol68FrontEnd>. Additional information
about the Algol 68 programming language, and how it is being evolved by
the GNU Algol 68 Working Group can be found at
<https://algol68-lang.org>.

Macro: **AC\_PROG\_A68**([compiler-search-list]) [¶](#index-AC_005fPROG_005fA68)
:   Find the Algol 68 compiler to use. Check whether the environment
    variable `A68` is set; if so, then set output variable `A68`
    to its value.

    Otherwise, if the macro is invoked without an argument, then search for
    an Algol 68 compiler named `ga68`. If it is not found, then as a
    last resort set `A68` to `ga68`.

    This macro may be invoked with an optional first argument which, if
    specified, must be a blank-separated list of Algol 68 compilers to
    search for.

    If output variable `A68FLAGS` was not already set, set it to
    `-g -O2`. If your package does not like this default,
    `A68FLAGS` may be set before `AC_PROG_A68`.

---

Next: [C and POSIX Variants](#C-and-POSIX-Variants), Previous: [Compilers and Preprocessors](#Compilers-and-Preprocessors), Up: [Existing Tests](#Existing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 5.11 System Services [¶](#System-Services-1)

The following macros check for operating system services or capabilities.

Macro: **AC\_PATH\_X** [¶](#index-AC_005fPATH_005fX-1)
:   Try to locate the X Window System include files and libraries. If the
    user gave the command line options `--x-includes=dir` and
    `--x-libraries=dir`, use those directories.

    If either or both were not given, get the missing values by running
    `xmkmf` (or an executable pointed to by the `XMKMF`
    environment variable) on a trivial `Imakefile` and examining the
    makefile that it produces. Setting `XMKMF` to ‘`false`’
    disables this method.

    If this method fails to find the X Window System, `configure`
    looks for the files in several directories where they often reside.
    If either method is successful, set the shell variables
    `x_includes` and `x_libraries` to their locations, unless they
    are in directories the compiler searches by default.

    If both methods fail, or the user gave the command line option
    `--without-x`, set the shell variable `no_x` to ‘`yes`’;
    otherwise set it to the empty string.

Macro: **AC\_PATH\_XTRA** [¶](#index-AC_005fPATH_005fXTRA-1)
:   An enhanced version of `AC_PATH_X`. It adds the C compiler flags
    that X needs to output variable `X_CFLAGS`, and the X linker flags
    to `X_LIBS`. Define `X_DISPLAY_MISSING` if X is not
    available.

    This macro also checks for special libraries that some systems need in
    order to compile X programs. It adds any that the system needs to
    output variable `X_EXTRA_LIBS`. And it checks for special X11R6
    libraries that need to be linked with before `-lX11`, and adds
    any found to the output variable `X_PRE_LIBS`.

Macro: **AC\_SYS\_INTERPRETER** [¶](#index-AC_005fSYS_005fINTERPRETER-1)
:   Check whether the system supports starting scripts with a line of the
    form ‘`#!/bin/sh`’ to select the interpreter to use for the script.
    After running this macro, shell code in `configure.ac` can check
    the shell variable `interpval`; it is set to ‘`yes`’
    if the system supports ‘`#!`’, ‘`no`’ if not.

Macro: **AC\_SYS\_LARGEFILE** [¶](#index-AC_005fSYS_005fLARGEFILE-1)
:   If the default `off_t` type is a 32-bit integer,
    and therefore cannot be used with files 2 GiB or larger,
    make a wider `off_t` available if the system supports it.
    Similarly, widen other types related to sizes of files and file systems
    if possible. These types may include `blkcnt_t`, `dev_t`,
    `ino_t`, `fsblkcnt_t`, `fsfilcnt_t`, and `rlim_t`.

    Also, arrange for a `configure` option `--enable-year2038`
    to request widening the type `time_t` as needed to represent file
    wand other timestamps after mid-January 2038. This widening is possible
    only on 32-bit GNU/Linux x86 and ARM systems with glibc 2.34 or later.
    If year-2038 support is requested but `configure` fails to find a way
    to widen `time_t` and inspection of the system suggests that
    this feature is available somehow, `configure` will error out.
    If you want the default to be `--enable-year2038`, you can use
    `AC_SYS_YEAR2038` or `AC_SYS_YEAR2038_RECOMMENDED`
    instead of `AC_SYS_LARGEFILE`.
    In other words, older packages that have long used `AC_SYS_LARGEFILE`
    can have year-2038 support on 32-bit GNU/Linux x86 and ARM systems either by
    regenerating `configure` with current Autoconf and configuring with
    `--enable-year2038`, or by using `AC_SYS_YEAR2038` or
    `AC_SYS_YEAR2038_RECOMMENDED` and configuring without
    `--disable-year2038`.
    A future version of Autoconf might change the `AC_SYS_LARGEFILE`
    default to `--enable-year2038`; if and when that happens,
    `AC_SYS_LARGEFILE` and `AC_SYS_YEAR2038` will become equivalent.
    See [AC\_SYS\_YEAR2038](#AC_005fSYS_005fYEAR2038).

    Set the shell variable `ac_have_largefile` to ‘`yes`’ or
    `no` depending on whether a wide `off_t` is available,
    regardless of whether arrangements were necessary.
    Similarly, set the shell variable `ac_have_year2038` to `yes`
    or `no` depending on whether a wide-enough `time_t` is available.

    Define preprocessor macros if necessary to make types wider;
    for example, on GNU/Linux systems the macros `_FILE_OFFSET_BITS`
    and `_TIME_BITS` can be defined. Some of these macros work only if
    defined before the first system header is included;
    therefore, when using this macro in concert with
    `AC_CONFIG_HEADERS`, make sure that `config.h` is included
    before any system headers.

    Large-file support can be disabled by configuring with the
    `--disable-largefile` option, and year-2038 support can
    be enabled and disabled via the `--enable-year2038` and
    `--disable-year2038` options. These options have no effect on
    systems where types are wide enough by default.
    Large-file support is required for year-2038 support: if you configure
    with `--disable-largefile` on a platform with 32-bit
    `time_t`, then year-2038 support is not available.

    Disabling large-file or year-2038 support can have surprising effects,
    such as causing functions like `readdir` and `stat` to fail
    even on a small file because its inode number or timestamp is out of range.

    Regardless of whether you use this macro, portable programs should not
    assume that any of the types listed above fit into a `long int`.
    For example, it is not portable to print an arbitrary `off_t` or
    `time_t` value `X` with `printf ("%ld", (long int) X)`.

    The standard C library functions `fseek` and `ftell`
    do not use `off_t`. If you need to use either of these functions,
    you should use `AC_FUNC_FSEEKO` as well as `AC_SYS_LARGEFILE`,
    and then use their POSIX replacements `fseeko` and `ftello`.
    See [AC\_FUNC\_FSEEKO](#AC_005fFUNC_005fFSEEKO).

    When using `AC_SYS_LARGEFILE` in different packages that are linked
    together and that have interfaces that depend on the width of `off_t`,
    `time_t` or related types, the simplest thing is to configure all
    components the same way. For example, if an application uses
    `AC_SYS_LARGEFILE` and is configured with
    `--enable-year2038`, libraries it links to with an `off_t`-
    or `time_t`-dependent interface should be configured equivalently.
    Alternatively, you can modify libraries to support both 32- and 64-bit
    interfaces though this is more work and few libraries other than the C
    library itself are modified in this way.

    Applications and libraries should be configured compatibly.
    If `off_t`, `time_t` or related types appear in a library’s
    public interface, enabling or disabling the library’s large-file or
    year-2038 support may break binary compatibility with applications or
    with other libraries. Similarly, if an application links to a such a
    library, enabling or disabling the application’s large-file support may
    break binary compatibility with that library.

Macro: **AC\_SYS\_LONG\_FILE\_NAMES** [¶](#index-AC_005fSYS_005fLONG_005fFILE_005fNAMES-1)
:   If the system supports file names longer than 14 characters, define
    `HAVE_LONG_FILE_NAMES`.

Macro: **AC\_SYS\_POSIX\_TERMIOS** [¶](#index-AC_005fSYS_005fPOSIX_005fTERMIOS-1)
:   Check to see if the POSIX termios headers and functions are available on the
    system. If so, set the shell variable `ac_cv_sys_posix_termios` to
    ‘`yes`’. If not, set the variable to ‘`no`’.

Macro: **AC\_SYS\_YEAR2038** [¶](#index-AC_005fSYS_005fYEAR2038-1)
:   This is like `AC_SYS_LARGEFILE` except it defaults to enabling
    instead of disabling year-2038 support. Year-2038 support for
    applications and libraries should be configured compatibly.
    See [AC\_SYS\_LARGEFILE](#AC_005fSYS_005fLARGEFILE).

Macro: **AC\_SYS\_YEAR2038\_RECOMMENDED** [¶](#index-AC_005fSYS_005fYEAR2038_005fRECOMMENDED-1)
:   This macro has the same effect as `AC_SYS_YEAR2038`,
    but also declares that the program being configured
    should support timestamps after mid-January 2038.
    If a large `time_t` is unavailable, `configure` will error
    out unless the `--disable-year2038` option is specified.

    Year-2038 support for applications and libraries should be configured
    compatibly. See [AC\_SYS\_YEAR2038](#AC_005fSYS_005fYEAR2038).

---

Next: [Erlang Libraries](#Erlang-Libraries), Previous: [System Services](#System-Services), Up: [Existing Tests](#Existing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 5.12 C and POSIX Variants [¶](#C-and-POSIX-Variants-1)

The following macro makes it possible to use C language and library
extensions defined by the C standards committee, features of POSIX that
are extensions to C, and platform extensions not defined by POSIX.

Macro: **AC\_USE\_SYSTEM\_EXTENSIONS** [¶](#index-AC_005fUSE_005fSYSTEM_005fEXTENSIONS-1)
:   If possible, enable extensions to C or POSIX on hosts that normally
    disable the extensions, typically due to standards-conformance namespace
    issues. This should be called before any macros that run the C
    compiler. Also, when using this macro in concert with
    `AC_CONFIG_HEADERS`, be sure that `config.h` is included
    before any system header.

    Define the following preprocessor macros unconditionally:

    `_ALL_SOURCE` [¶](#index-_005fALL_005fSOURCE)
    :   Enable extensions on AIX and z/OS.

    `_COSMO_SOURCE` [¶](#index-_005fCOSMO_005fSOURCE)
    :   Enable extensions on Cosmopolitan Libc.

    `_DARWIN_C_SOURCE` [¶](#index-_005fDARWIN_005fC_005fSOURCE)
    :   Enable extensions on macOS.

    `_GNU_SOURCE` [¶](#index-_005fGNU_005fSOURCE)
    :   Enable extensions on GNU systems.

    `_NETBSD_SOURCE` [¶](#index-_005fNETBSD_005fSOURCE)
    :   Enable general extensions on NetBSD.
        Enable NetBSD compatibility extensions on Minix.

    `_OPENBSD_SOURCE` [¶](#index-_005fOPENBSD_005fSOURCE)
    :   Enable OpenBSD compatibility extensions on NetBSD.
        Oddly enough, this does nothing on OpenBSD.

    `_POSIX_PTHREAD_SEMANTICS` [¶](#index-_005fPOSIX_005fPTHREAD_005fSEMANTICS)
    :   Enable POSIX-compatible threading on Solaris.

    `__STDC_WANT_IEC_60559_ATTRIBS_EXT__` [¶](#index-_005f_005fSTDC_005fWANT_005fIEC_005f60559_005fATTRIBS_005fEXT_005f_005f)
    :   Enable extensions specified by ISO/IEC TS 18661-5:2014.

    `__STDC_WANT_IEC_60559_BFP_EXT__` [¶](#index-_005f_005fSTDC_005fWANT_005fIEC_005f60559_005fBFP_005fEXT_005f_005f)
    :   Enable extensions specified by ISO/IEC TS 18661-1:2014.

    `__STDC_WANT_IEC_60559_DFP_EXT__` [¶](#index-_005f_005fSTDC_005fWANT_005fIEC_005f60559_005fDFP_005fEXT_005f_005f)
    :   Enable extensions specified by ISO/IEC TS 18661-2:2015.

    `__STDC_WANT_IEC_60559_EXT__` [¶](#index-_005f_005fSTDC_005fWANT_005fIEC_005f60559_005fEXT_005f_005f)
    :   Enable extensions specified by C23 Annex F.

    `__STDC_WANT_IEC_60559_FUNCS_EXT__` [¶](#index-_005f_005fSTDC_005fWANT_005fIEC_005f60559_005fFUNCS_005fEXT_005f_005f)
    :   Enable extensions specified by ISO/IEC TS 18661-4:2015.

    `__STDC_WANT_IEC_60559_TYPES_EXT__` [¶](#index-_005f_005fSTDC_005fWANT_005fIEC_005f60559_005fTYPES_005fEXT_005f_005f)
    :   Enable extensions specified by C23 Annex H and by ISO/IEC TS 18661-3:2015.

    `__STDC_WANT_LIB_EXT2__` [¶](#index-_005f_005fSTDC_005fWANT_005fLIB_005fEXT2_005f_005f)
    :   Enable extensions specified by ISO/IEC TR 24731-2:2010.

    `__STDC_WANT_MATH_SPEC_FUNCS__` [¶](#index-_005f_005fSTDC_005fWANT_005fMATH_005fSPEC_005fFUNCS_005f_005f)
    :   Enable extensions specified by ISO/IEC 24747:2009.

    `_TANDEM_SOURCE` [¶](#index-_005fTANDEM_005fSOURCE)
    :   Enable extensions on HP NonStop systems.

    The following preprocessor macros are defined only when necessary;
    they enable access to extensions on some operating systems but
    *disable* extensions on other operating systems.

    `__EXTENSIONS__` [¶](#index-_005f_005fEXTENSIONS_005f_005f)
    :   Enable general extensions on Solaris. This macro is defined only if
        the headers included by `AC_INCLUDES_DEFAULT`
        (see [Default Includes](#Default-Includes)) work correctly with it defined.

    `_MINIX` [¶](#index-_005fMINIX)

    `_POSIX_SOURCE`

    `_POSIX_1_SOURCE`
    :   Defined only on MINIX. `_POSIX_SOURCE` and `_POSIX_1_SOURCE`
        are needed to enable a number of POSIX features on this OS.
        `_MINIX` does not affect the system headers’ behavior;
        future versions of Autoconf may stop defining it.
        Programs that need to recognize Minix should use `AC_CANONICAL_HOST`.

    `_XOPEN_SOURCE` [¶](#index-_005fXOPEN_005fSOURCE)
    :   Defined (with value 500) only if needed to make `wchar.h` declare
        `mbstate_t`. This is known to be necessary on some versions of HP-UX.

    The C preprocessor macro `__STDC_WANT_DEC_FP__` is not defined.
    ISO/IEC TR 24732:2009 was superseded by ISO/IEC TS 18661-2:2015.

    The C preprocessor macro `__STDC_WANT_LIB_EXT1__` is not defined,
    as the C standard’s Annex K is problematic. See: O’Donell C, Sebor M.
    [Field
    Experience With Annex K—Bounds Checking Interfaces](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1967.htm).

    The Autoconf macro `AC_USE_SYSTEM_EXTENSIONS` was introduced in
    Autoconf 2.60.

---

Previous: [C and POSIX Variants](#C-and-POSIX-Variants), Up: [Existing Tests](#Existing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
