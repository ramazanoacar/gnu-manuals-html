### 5.13 Erlang Libraries [¶](#Erlang-Libraries-1)

The following macros check for an installation of Erlang/OTP, and for the
presence of certain Erlang libraries. All those macros require the
configuration of an Erlang interpreter and an Erlang compiler
(see [Erlang Compiler and Interpreter Characteristics](#Erlang-Compiler-and-Interpreter)).

Macro: **AC\_ERLANG\_SUBST\_ERTS\_VER** [¶](#index-AC_005fERLANG_005fSUBST_005fERTS_005fVER-1)
:   Set the output variable `ERLANG_ERTS_VER` to the version of the
    Erlang runtime system (as returned by Erlang’s
    `erlang:system_info(version)` function). The result of this test
    is cached if caching is enabled when running `configure`. The
    `ERLANG_ERTS_VER` variable is not intended to be used for testing
    for features of specific ERTS versions, but to be used for substituting
    the ERTS version in Erlang/OTP release resource files (`.rel`
    files), as shown below.

Macro: **AC\_ERLANG\_SUBST\_ROOT\_DIR** [¶](#index-AC_005fERLANG_005fSUBST_005fROOT_005fDIR-1)
:   Set the output variable `ERLANG_ROOT_DIR` to the path to the base
    directory in which Erlang/OTP is installed (as returned by Erlang’s
    `code:root_dir/0` function). The result of this test is cached if
    caching is enabled when running `configure`.

Macro: **AC\_ERLANG\_SUBST\_LIB\_DIR** [¶](#index-AC_005fERLANG_005fSUBST_005fLIB_005fDIR-1)
:   Set the output variable `ERLANG_LIB_DIR` to the path of the library
    directory of Erlang/OTP (as returned by Erlang’s
    `code:lib_dir/0` function), which subdirectories each contain an installed
    Erlang/OTP library. The result of this test is cached if caching is enabled
    when running `configure`.

Macro: **AC\_ERLANG\_CHECK\_LIB**(library, [action-if-found], [action-if-not-found]) [¶](#index-AC_005fERLANG_005fCHECK_005fLIB-1)
:   Test whether the Erlang/OTP library library is installed by
    calling Erlang’s `code:lib_dir/1` function. The result of this
    test is cached if caching is enabled when running `configure`.
    action-if-found is a list of shell commands to run if the library
    is installed; action-if-not-found is a list of shell commands to
    run if it is not. Additionally, if the library is installed, the output
    variable ‘`ERLANG_LIB_DIR_library`’ is set to the path to the
    library installation directory, and the output variable
    ‘`ERLANG_LIB_VER_library`’ is set to the version number that is
    part of the subdirectory name, if it is in the standard form
    (`library-version`). If the directory name does not
    have a version part, ‘`ERLANG_LIB_VER_library`’ is set to the
    empty string. If the library is not installed,
    ‘`ERLANG_LIB_DIR_library`’ and
    ‘`ERLANG_LIB_VER_library`’ are set to `"not found"`. For
    example, to check if library `stdlib` is installed:

    ```
    AC_ERLANG_CHECK_LIB([stdlib],
      [AS_ECHO(["stdlib version \"$ERLANG_LIB_VER_stdlib\""])
       AS_ECHO(["is installed in \"$ERLANG_LIB_DIR_stdlib\""])],
      [AC_MSG_ERROR([stdlib was not found!])])
    ```

    The ‘`ERLANG_LIB_VER_library`’ variables (set by
    `AC_ERLANG_CHECK_LIB`) and the `ERLANG_ERTS_VER` variable (set
    by `AC_ERLANG_SUBST_ERTS_VER`) are not intended to be used for
    testing for features of specific versions of libraries or of the Erlang
    runtime system. Those variables are intended to be substituted in
    Erlang release resource files (`.rel` files). For instance, to
    generate a `example.rel` file for an application depending on the
    `stdlib` library, `configure.ac` could contain:

    ```
    AC_ERLANG_SUBST_ERTS_VER
    AC_ERLANG_CHECK_LIB([stdlib],
      [],
      [AC_MSG_ERROR([stdlib was not found!])])
    AC_CONFIG_FILES([example.rel])
    ```

    The `example.rel.in` file used to generate `example.rel`
    should contain:

    ```
    {release,
        {"@PACKAGE@", "@VERSION@"},
        {erts, "@ERLANG_ERTS_VER@"},
        [{stdlib, "@ERLANG_LIB_VER_stdlib@"},
         {@PACKAGE@, "@VERSION@"}]}.
    ```

In addition to the above macros, which test installed Erlang libraries, the
following macros determine the paths to the directories into which newly built
Erlang libraries are to be installed:

Macro: **AC\_ERLANG\_SUBST\_INSTALL\_LIB\_DIR** [¶](#index-AC_005fERLANG_005fSUBST_005fINSTALL_005fLIB_005fDIR-2)
:   Set the `ERLANG_INSTALL_LIB_DIR` output variable to the directory into
    which every built Erlang library should be installed in a separate
    subdirectory.
    If this variable is not set in the environment when `configure` runs,
    its default value is `${libdir}/erlang/lib`.

Macro: **AC\_ERLANG\_SUBST\_INSTALL\_LIB\_SUBDIR**(library, version) [¶](#index-AC_005fERLANG_005fSUBST_005fINSTALL_005fLIB_005fSUBDIR-2)
:   Set the ‘`ERLANG_INSTALL_LIB_DIR_library`’ output variable to the
    directory into which the built Erlang library library version
    version should be installed. If this variable is not set in the
    environment when `configure` runs, its default value is
    ‘`$ERLANG_INSTALL_LIB_DIR/library-version`’, the value of the
    `ERLANG_INSTALL_LIB_DIR` variable being set by the
    `AC_ERLANG_SUBST_INSTALL_LIB_DIR` macro.

---

Next: [Results of Tests](#Results), Previous: [Existing Tests](#Existing-Tests), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 6 Writing Tests [¶](#Writing-Tests-1)

If the existing feature tests don’t do something you need, you have to
write new ones. These macros are the building blocks. They provide
ways for other macros to check whether various kinds of features are
available and report the results.

This chapter contains some suggestions and some of the reasons why the
existing tests are written the way they are. You can also learn a lot
about how to write Autoconf tests by looking at the existing ones. If
something goes wrong in one or more of the Autoconf tests, this
information can help you understand the assumptions behind them, which
might help you figure out how to best solve the problem.

These macros check the output of the compiler system of the current
language (see [Language Choice](#Language-Choice)). They do not cache the results of
their tests for future use (see [Caching Results](#Caching-Results)), because they don’t
know enough about the information they are checking for to generate a
cache variable name. They also do not print any messages, for the same
reason. The checks for particular kinds of features call these macros
and do cache their results and print messages about what they’re
checking for.

When you write a feature test that could be applicable to more than one
software package, the best thing to do is encapsulate it in a new macro.
See [Writing Autoconf Macros](#Writing-Autoconf-Macros), for how to do that.

* [Language Choice](#Language-Choice)
* [Writing Test Programs](#Writing-Test-Programs)
* [Running the Preprocessor](#Running-the-Preprocessor)
* [Running the Compiler](#Running-the-Compiler)
* [Running the Linker](#Running-the-Linker)
* [Checking Runtime Behavior](#Runtime)
* [Multiple Cases](#Multiple-Cases)

---

Next: [Writing Test Programs](#Writing-Test-Programs), Up: [Writing Tests](#Writing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 6.1 Language Choice [¶](#Language-Choice-1)

Autoconf-generated `configure` scripts check for the C compiler and
its features by default. Packages that use other programming languages
(maybe more than one, e.g., C and C++) need to test features of the
compilers for the respective languages. The following macros determine
which programming language is used in the subsequent tests in
`configure.ac`.

Macro: **AC\_LANG**(language) [¶](#index-AC_005fLANG-1)
:   Do compilation tests using the compiler, preprocessor, and file
    extensions for the specified language.

    Supported languages are:

    ‘`C`’
    :   Do compilation tests using `CC` and `CPP` and use extension
        `.c` for test programs. Use compilation flags: `CPPFLAGS` with
        `CPP`, and both `CPPFLAGS` and `CFLAGS` with `CC`.

    ‘`C++`’
    :   Do compilation tests using `CXX` and `CXXCPP` and use
        extension `.C` for test programs. Use compilation flags:
        `CPPFLAGS` with `CXXCPP`, and both `CPPFLAGS` and
        `CXXFLAGS` with `CXX`.

    ‘`Fortran 77`’
    :   Do compilation tests using `F77` and use extension `.f` for
        test programs. Use compilation flags: `FFLAGS`.

    ‘`Fortran`’
    :   Do compilation tests using `FC` and use extension `.f` (or
        whatever has been set by `AC_FC_SRCEXT`) for test programs. Use
        compilation flags: `FCFLAGS`.

    ‘`Erlang`’ [¶](#index-ERLC-2)
    :   Compile and execute tests using `ERLC` and `ERL` and use extension
        `.erl` for test Erlang modules. Use compilation flags: `ERLCFLAGS`.

    ‘`Objective C`’
    :   Do compilation tests using `OBJC` and `OBJCPP` and use
        extension `.m` for test programs. Use compilation flags:
        `CPPFLAGS` with `OBJCPP`, and both `CPPFLAGS` and
        `OBJCFLAGS` with `OBJC`.

    ‘`Objective C++`’
    :   Do compilation tests using `OBJCXX` and `OBJCXXCPP` and use
        extension `.mm` for test programs. Use compilation flags:
        `CPPFLAGS` with `OBJCXXCPP`, and both `CPPFLAGS` and
        `OBJCXXFLAGS` with `OBJCXX`.

    ‘`Go`’
    :   Do compilation tests using `GOC` and use extension `.go` for
        test programs. Use compilation flags `GOFLAGS`.

    ‘`Algol 68`’
    :   Do compilation tests using `A68` and use extension `.a68` for
        test programs. Use compilation flags `A68FLAGS`.

Macro: **AC\_LANG\_PUSH**(language) [¶](#index-AC_005fLANG_005fPUSH-1)
:   Remember the current language (as set by `AC_LANG`) on a stack, and
    then select the language. Use this macro and `AC_LANG_POP`
    in macros that need to temporarily switch to a particular language.

Macro: **AC\_LANG\_POP**([language]) [¶](#index-AC_005fLANG_005fPOP-1)
:   Select the language that is saved on the top of the stack, as set by
    `AC_LANG_PUSH`, and remove it from the stack.

    If given, language specifies the language we just *quit*. It
    is a good idea to specify it when it’s known (which should be the
    case…), since Autoconf detects inconsistencies.

    ```
    AC_LANG_PUSH([Fortran 77])
    # Perform some tests on Fortran 77.
    # ...
    AC_LANG_POP([Fortran 77])
    ```

Macro: **AC\_LANG\_ASSERT**(language) [¶](#index-AC_005fLANG_005fASSERT-1)
:   Check statically that the current language is language.
    You should use this in your language specific macros
    to avoid that they be called with an inappropriate language.

    This macro runs only at `autoconf` time, and incurs no cost at
    `configure` time. Sadly enough and because Autoconf is a two
    layer language [2](#FOOT2), the macros
    `AC_LANG_PUSH` and `AC_LANG_POP` cannot be “optimizing”,
    therefore as much as possible you ought to avoid using them to wrap
    your code, rather, require from the user to run the macro with a
    correct current language, and check it with `AC_LANG_ASSERT`.
    And anyway, that may help the user understand she is running a Fortran
    macro while expecting a result about her Fortran 77 compiler...

Macro: **AC\_REQUIRE\_CPP** [¶](#index-AC_005fREQUIRE_005fCPP-1)
:   Ensure that whichever preprocessor would currently be used for tests has
    been found. Calls `AC_REQUIRE` (see [Prerequisite Macros](#Prerequisite-Macros)) with an
    argument of either `AC_PROG_CPP` or `AC_PROG_CXXCPP`,
    depending on which language is current.

---

Next: [Running the Preprocessor](#Running-the-Preprocessor), Previous: [Language Choice](#Language-Choice), Up: [Writing Tests](#Writing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 6.2 Writing Test Programs [¶](#Writing-Test-Programs-1)

Autoconf tests follow a common scheme: feed some program with some
input, and most of the time, feed a compiler with some source file.
This section is dedicated to these source samples.

* [Guidelines for Test Programs](#Guidelines)
* [Test Functions](#Test-Functions)
* [Generating Sources](#Generating-Sources)

---

Next: [Test Functions](#Test-Functions), Up: [Writing Test Programs](#Writing-Test-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 6.2.1 Guidelines for Test Programs [¶](#Guidelines-for-Test-Programs)

The most important rule to follow when writing testing samples is:

*Look for realism.*

This motto means that testing samples must be written with the same
strictness as real programs are written. In particular, you should
avoid “shortcuts” and simplifications.

Don’t just play with the preprocessor if you want to prepare a
compilation. For instance, using `cpp` to check whether a header is
functional might let your `configure` accept a header which
causes some *compiler* error. Do not hesitate to check a header with
other headers included before, especially required headers.

Make sure the symbols you use are properly defined, i.e., refrain from
simply declaring a function yourself instead of including the proper
header.

Test programs should not write to standard output. They
should exit with status 0 if the test succeeds, and with status 1
otherwise, so that success
can be distinguished easily from a core dump or other failure;
segmentation violations and other failures produce a nonzero exit
status. Unless you arrange for `exit` to be declared, test
programs should `return`, not `exit`, from `main`,
because on many systems `exit` is not declared by default.

Test programs can use `#if` or `#ifdef` to check the values of
preprocessor macros defined by tests that have already run. For
example, if you call `AC_HEADER_STDBOOL`, then later on in
`configure.ac` you can have a test program that includes
`stdbool.h` conditionally:

```
#ifdef HAVE_STDBOOL_H
# include <stdbool.h>
#endif
```

Both `#if HAVE_STDBOOL_H` and `#ifdef HAVE_STDBOOL_H` will
work with any standard C compiler. Some developers prefer `#if`
because it is easier to read, while others prefer `#ifdef` because
it avoids diagnostics with picky compilers like GCC with the
`-Wundef` option.

If a test program needs to use or create a data file, give it a name
that starts with `conftest`, such as `conftest.data`. The
`configure` script cleans up by running ‘`rm -f -r conftest*`’
after running test programs and if the script is interrupted.

---

Next: [Generating Sources](#Generating-Sources), Previous: [Guidelines for Test Programs](#Guidelines), Up: [Writing Test Programs](#Writing-Test-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 6.2.2 Test Functions [¶](#Test-Functions-1)

Functions in test code should use function prototypes, introduced in C89
and required in C23.

Functions that test programs declare should also be conditionalized for
C++, which requires ‘`extern "C"`’ prototypes. Make sure to not
include any header files containing clashing prototypes.

```
#ifdef __cplusplus
extern "C"
#endif
void *valloc (size_t);
```

If a test program calls a function with invalid parameters (just to see
whether it exists), organize the program to ensure that it never invokes
that function. You can do this by calling it in another function that is
never invoked. You can’t do it by putting it after a call to
`exit`, because GCC version 2 knows that `exit`
never returns
and optimizes out any code that follows it in the same block.

If you include any header files, be sure to call the functions
relevant to them with the correct number of arguments, even if they are
just 0, to avoid compilation errors due to prototypes. GCC
version 2
has internal prototypes for several functions that it automatically
inlines; for example, `memcpy`. To avoid errors when checking for
them, either pass them the correct number of arguments or redeclare them
with a different return type (such as `char`).

---

Previous: [Test Functions](#Test-Functions), Up: [Writing Test Programs](#Writing-Test-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 6.2.3 Generating Sources [¶](#Generating-Sources-1)

Autoconf provides a set of macros that can be used to generate test
source files. They are written to be language generic, i.e., they
actually depend on the current language (see [Language Choice](#Language-Choice)) to
“format” the output properly.

Macro: **AC\_LANG\_CONFTEST**(source) [¶](#index-AC_005fLANG_005fCONFTEST-1)
:   Save the source text in the current test source file:
    `conftest.extension` where the extension depends on the
    current language. As of Autoconf 2.63b, the source file also contains
    the results of all of the `AC_DEFINE` performed so far.

    Note that the source is evaluated exactly once, like regular
    Autoconf macro arguments, and therefore (i) you may pass a macro
    invocation, (ii) if not, be sure to double quote if needed.

    The source text is expanded as an unquoted here-document, so
    ‘`$`’, ‘`` ` ``’ and some ‘`\`’s should be backslash-escaped.
    See [Here-Documents](#Here_002dDocuments).

    This macro issues a warning during `autoconf` processing if
    source does not include an expansion of the macro
    `AC_LANG_DEFINES_PROVIDED` (note that both `AC_LANG_SOURCE` and
    `AC_LANG_PROGRAM` call this macro, and thus avoid the warning).

    This macro is seldom called directly, but is used under the hood by more
    common macros such as `AC_COMPILE_IFELSE` and `AC_RUN_IFELSE`.

Macro: **AC\_LANG\_DEFINES\_PROVIDED** [¶](#index-AC_005fLANG_005fDEFINES_005fPROVIDED-1)
:   This macro is called as a witness that the file
    `conftest.extension` appropriate for the current language is
    complete, including all previously determined results from
    `AC_DEFINE`. This macro is seldom called directly, but exists if
    you have a compelling reason to write a conftest file without using
    `AC_LANG_SOURCE`, yet still want to avoid a syntax warning from
    `AC_LANG_CONFTEST`.

Macro: **AC\_LANG\_SOURCE**(source) [¶](#index-AC_005fLANG_005fSOURCE-1)
:   Expands into the source, with the definition of
    all the `AC_DEFINE` performed so far. This macro includes an
    expansion of `AC_LANG_DEFINES_PROVIDED`.

    In many cases, you may find it more convenient to use the wrapper
    `AC_LANG_PROGRAM`.

For instance, executing (observe the double quotation!):

```
AC_INIT([Hello], [1.0], [bug-hello@example.org], [],
        [https://www.example.org/])
AC_DEFINE([HELLO_WORLD], ["Hello, World\n"],
  [Greetings string.])
AC_LANG([C])
AC_LANG_CONFTEST(
   [AC_LANG_SOURCE([[const char hw[] = "Hello, World\n";]])])
gcc -E -dD conftest.c
```

on a system with `gcc` installed, results in:

```
...
# 1 "conftest.c"

#define PACKAGE_NAME "Hello"
#define PACKAGE_TARNAME "hello"
#define PACKAGE_VERSION "1.0"
#define PACKAGE_STRING "Hello 1.0"
#define PACKAGE_BUGREPORT "bug-hello@example.org"
#define PACKAGE_URL "https://www.example.org/"
#define HELLO_WORLD "Hello, World\n"

const char hw[] = "Hello, World\n";
```

When the test language is Fortran, Erlang, Go, or Algol 68,
the `AC_DEFINE`
definitions are not automatically translated into constants in the
source code by this macro.

Macro: **AC\_LANG\_PROGRAM**([prologue], [body]) [¶](#index-AC_005fLANG_005fPROGRAM-1)
:   Expands into a source file which consists of the prologue, and
    then body as body of the main function (e.g., `main` in
    C). Since it uses `AC_LANG_SOURCE`, the features of the latter are
    available.

For instance:

```
AC_INIT([Hello], [1.0], [bug-hello@example.org], [],
        [https://www.example.org/])
AC_DEFINE([HELLO_WORLD], ["Hello, World\n"],
  [Greetings string.])
AC_LANG_CONFTEST(
[AC_LANG_PROGRAM([[const char hw[] = "Hello, World\n";]],
                 [[fputs (hw, stdout);]])])
gcc -E -dD conftest.c
```

on a system with `gcc` installed, results in:

```
...
# 1 "conftest.c"

#define PACKAGE_NAME "Hello"
#define PACKAGE_TARNAME "hello"
#define PACKAGE_VERSION "1.0"
#define PACKAGE_STRING "Hello 1.0"
#define PACKAGE_BUGREPORT "bug-hello@example.org"
#define PACKAGE_URL "https://www.example.org/"
#define HELLO_WORLD "Hello, World\n"

const char hw[] = "Hello, World\n";
int
main (void)
{
fputs (hw, stdout);
  ;
  return 0;
}
```

In Erlang tests, the created source file is that of an Erlang module called
`conftest` (`conftest.erl`). This module defines and exports
at least
one `start/0` function, which is called to perform the test. The
prologue is optional code that is inserted between the module header and
the `start/0` function definition. body is the body of the
`start/0` function without the final period (see [Checking Runtime Behavior](#Runtime), about
constraints on this function’s behavior).

For instance:

```
AC_INIT([Hello], [1.0], [bug-hello@example.org])
AC_LANG(Erlang)
AC_LANG_CONFTEST(
[AC_LANG_PROGRAM([[-define(HELLO_WORLD, "Hello, world!").]],
                 [[io:format("~s~n", [?HELLO_WORLD])]])])
cat conftest.erl
```

results in:

```
-module(conftest).
-export([start/0]).
-define(HELLO_WORLD, "Hello, world!").
start() ->
io:format("~s~n", [?HELLO_WORLD])
.
```

Macro: **AC\_LANG\_CALL**(prologue, function) [¶](#index-AC_005fLANG_005fCALL-1)
:   Expands into a source file which consists of the prologue, and
    then a call to the function as body of the main function (e.g.,
    `main` in C). Since it uses `AC_LANG_PROGRAM`, the feature
    of the latter are available.

    This function will probably be replaced in the future by a version
    which would enable specifying the arguments. The use of this macro is
    not encouraged, as it violates strongly the typing system.

    This macro cannot be used for Erlang tests.

Macro: **AC\_LANG\_FUNC\_LINK\_TRY**(function) [¶](#index-AC_005fLANG_005fFUNC_005fLINK_005fTRY-1)
:   Expands into a source file which uses the function in the body of
    the main function (e.g., `main` in C). Since it uses
    `AC_LANG_PROGRAM`, the features of the latter are available.

    As `AC_LANG_CALL`, this macro is documented only for completeness.
    It is considered to be severely broken, and in the future will be
    removed in favor of actual function calls (with properly typed
    arguments).

    This macro cannot be used for Erlang tests.

---

Next: [Running the Compiler](#Running-the-Compiler), Previous: [Writing Test Programs](#Writing-Test-Programs), Up: [Writing Tests](#Writing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 6.3 Running the Preprocessor [¶](#Running-the-Preprocessor-1)

Sometimes one might need to run the preprocessor on some source file.
*Usually it is a bad idea*, as you typically need to *compile*
your project, not merely run the preprocessor on it; therefore you
certainly want to run the compiler, not the preprocessor. Resist the
temptation of following the easiest path.

Nevertheless, if you need to run the preprocessor, then use
`AC_PREPROC_IFELSE`.

The macros described in this section cannot be used for tests in Erlang,
Fortran, Go, or Algol 68 since those languages require no preprocessor.

Macro: **AC\_PREPROC\_IFELSE**(input, [action-if-true], [action-if-false]) [¶](#index-AC_005fPREPROC_005fIFELSE-1)
:   Run the preprocessor of the current language (see [Language Choice](#Language-Choice))
    on the input, run the shell commands action-if-true on
    success, action-if-false otherwise.

    If input is nonempty use the equivalent of
    `AC_LANG_CONFTEST(input)` to generate the current test source
    file; otherwise reuse the already-existing test source file.
    The input can be made by `AC_LANG_PROGRAM` and friends.
    The input text is expanded as an unquoted here-document, so
    ‘`$`’, ‘`` ` ``’ and some ‘`\`’s should be backslash-escaped.
    See [Here-Documents](#Here_002dDocuments).

    This macro uses `CPPFLAGS`, but not `CFLAGS`, because
    `-g`, `-O`, etc. are not valid options to many C
    preprocessors.

    It is customary to report unexpected failures with
    `AC_MSG_FAILURE`. If needed, action-if-true can further access
    the preprocessed output in the file `conftest.i`.

For instance:

```
AC_INIT([Hello], [1.0], [bug-hello@example.org])
AC_DEFINE([HELLO_WORLD], ["Hello, World\n"],
  [Greetings string.])
AC_PREPROC_IFELSE(
   [AC_LANG_PROGRAM([[const char hw[] = "Hello, World\n";]],
                    [[fputs (hw, stdout);]])],
   [AC_MSG_RESULT([OK])],
   [AC_MSG_FAILURE([unexpected preprocessor failure])])
```

might result in:

```
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables...
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether the compiler supports GNU C... yes
checking whether gcc accepts -g... yes
checking for gcc option to enable C23 features... -std=gnu23
checking how to run the C preprocessor... gcc -std=gnu23 -E
OK
```

  

The macro `AC_TRY_CPP` (see [Obsolete Macros](#Obsolete-Macros)) used to play the
role of `AC_PREPROC_IFELSE`, but double quotes its argument, making
it impossible to use it to elaborate sources. You are encouraged to
get rid of your old use of the macro `AC_TRY_CPP` in favor of
`AC_PREPROC_IFELSE`, but, in the first place, are you sure you need
to run the *preprocessor* and not the compiler?

Macro: **AC\_EGREP\_HEADER**(pattern, header-file, action-if-found, [action-if-not-found]) [¶](#index-AC_005fEGREP_005fHEADER-1)
:   pattern, after being expanded as if in a double-quoted shell string,
    is an extended regular expression.
    If the output of running the preprocessor on the system header file
    header-file contains a line matching
    pattern, execute shell commands action-if-found, otherwise
    execute action-if-not-found.

    See below for some problems involving this macro.

Macro: **AC\_EGREP\_CPP**(pattern, program, [action-if-found], [action-if-not-found]) [¶](#index-AC_005fEGREP_005fCPP-1)
:   pattern, after being expanded as if in a double-quoted shell string,
    is an extended regular expression.
    program is the text of a C or C++ program, which is expanded as an
    unquoted here-document (see [Here-Documents](#Here_002dDocuments)). If the
    output of running the preprocessor on program contains a line
    matching pattern, execute shell commands
    action-if-found, otherwise execute action-if-not-found.

    See below for some problems involving this macro.

`AC_EGREP_CPP` and `AC_EGREP_HEADER` should be used with care,
as preprocessors can insert line breaks between output tokens. For
example, the preprocessor might transform this:

```
#define MAJOR 2
#define MINOR 23
Version MAJOR . MINOR
```

into this:

```
Version
       2
                 .
                   23
```

Because preprocessors are allowed to insert white space, change escapes
in string constants, insert backlash-newline pairs, or do any of a number
of things that do not change the meaning of the preprocessed program, it
is better to rely on `AC_PREPROC_IFELSE` than to resort to
`AC_EGREP_CPP` or `AC_EGREP_HEADER`.

For more information about what can appear in portable extended regular
expressions, see [Problematic Expressions](https://www.gnu.org/software/grep/manual/grep.html#Problematic-Expressions) in GNU Grep.

---

Next: [Running the Linker](#Running-the-Linker), Previous: [Running the Preprocessor](#Running-the-Preprocessor), Up: [Writing Tests](#Writing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 6.4 Running the Compiler [¶](#Running-the-Compiler-1)

To check for a syntax feature of the current language’s (see [Language Choice](#Language-Choice)) compiler, such as whether it recognizes a certain keyword, or
simply to try some library feature, use `AC_COMPILE_IFELSE` to try
to compile a small program that uses that feature.

Macro: **AC\_COMPILE\_IFELSE**(input, [action-if-true], [action-if-false]) [¶](#index-AC_005fCOMPILE_005fIFELSE-1)
:   Run the compiler and compilation flags of the current language
    (see [Language Choice](#Language-Choice)) on the input, run the shell commands
    action-if-true on success, action-if-false otherwise.

    If input is nonempty use the equivalent of
    `AC_LANG_CONFTEST(input)` to generate the current test source
    file; otherwise reuse the already-existing test source file.
    The input can be made by `AC_LANG_PROGRAM` and friends.
    The input text is expanded as an unquoted here-document, so
    ‘`$`’, ‘`` ` ``’ and some ‘`\`’s should be backslash-escaped.
    See [Here-Documents](#Here_002dDocuments).

    It is customary to report unexpected failures with
    `AC_MSG_FAILURE`. This macro does not try to link; use
    `AC_LINK_IFELSE` if you need to do that (see [Running the Linker](#Running-the-Linker)). If needed, action-if-true can further access the
    just-compiled object file `conftest.$OBJEXT`.

    This macro uses `AC_REQUIRE` for the compiler associated with the
    current language, which means that if the compiler has not yet been
    determined, the compiler determination will be made prior to the body of
    the outermost `AC_DEFUN` macro that triggered this macro to
    expand (see [Expanded Before Required](#Expanded-Before-Required)).

For tests in Erlang, the input must be the source code of a module named
`conftest`. `AC_COMPILE_IFELSE` generates a `conftest.beam`
file that can be interpreted by the Erlang virtual machine (`ERL`). It is
recommended to use `AC_LANG_PROGRAM` to specify the test program,
to ensure that the Erlang module has the right name.

---

Next: [Checking Runtime Behavior](#Runtime), Previous: [Running the Compiler](#Running-the-Compiler), Up: [Writing Tests](#Writing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 6.5 Running the Linker [¶](#Running-the-Linker-1)

To check for a library, a function, or a global variable, Autoconf
`configure` scripts try to compile and link a small program that
uses it. This is unlike Metaconfig, which by default uses `nm` or
`ar` on the C library to try to figure out which functions are
available. Trying to link with the function is usually a more reliable
approach because it avoids dealing with the variations in the options
and output formats of `nm` and `ar` and in the location of the
standard libraries. It also allows configuring for cross-compilation or
checking a function’s runtime behavior if needed. On the other hand,
it can be slower than scanning the libraries once, but accuracy is more
important than speed.

`AC_LINK_IFELSE` is used to compile test programs to test for
functions and global variables. It is also used by `AC_CHECK_LIB`
to check for libraries (see [Library Files](#Libraries)), by adding the library being
checked for to `LIBS` temporarily and trying to link a small
program.

Macro: **AC\_LINK\_IFELSE**(input, [action-if-true], [action-if-false]) [¶](#index-AC_005fLINK_005fIFELSE-1)
:   Run the compiler (and compilation flags) and the linker of the current
    language (see [Language Choice](#Language-Choice)) on the input, run the shell
    commands action-if-true on success, action-if-false
    otherwise. If needed, action-if-true can further access the
    just-linked program file `conftest$EXEEXT`.

    If input is nonempty use the equivalent of
    `AC_LANG_CONFTEST(input)` to generate the current test source
    file; otherwise reuse the already-existing test source file.
    The input can be made by `AC_LANG_PROGRAM` and friends.
    The input text is expanded as an unquoted here-document, so
    ‘`$`’, ‘`` ` ``’ and some ‘`\`’s should be backslash-escaped.
    See [Here-Documents](#Here_002dDocuments).

    `LDFLAGS` and `LIBS` are used for linking, in addition to the
    current compilation flags.

    It is customary to report unexpected failures with
    `AC_MSG_FAILURE`. This macro does not try to execute the program;
    use `AC_RUN_IFELSE` if you need to do that (see [Checking Runtime Behavior](#Runtime)).

The `AC_LINK_IFELSE` macro cannot be used for Erlang tests, since Erlang
programs are interpreted and do not require linking.

---

Next: [Multiple Cases](#Multiple-Cases), Previous: [Running the Linker](#Running-the-Linker), Up: [Writing Tests](#Writing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 6.6 Checking Runtime Behavior [¶](#Checking-Runtime-Behavior)

Sometimes you need to find out how a system performs at runtime, such
as whether a given function has a certain capability or bug. If you
can, make such checks when your program runs instead of when it is
configured. You can check for things like the machine’s endianness when
your program initializes itself.

If you really need to test for a runtime behavior while configuring,
you can write a test program to determine the result, and compile and
run it using `AC_RUN_IFELSE`. Avoid running test programs if
possible, because this prevents people from configuring your package for
cross-compiling.

Macro: **AC\_RUN\_IFELSE**(input, [action-if-true], [action-if-false], [action-if-cross-compiling = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fRUN_005fIFELSE-1)
:   Run the compiler (and compilation flags) and the linker of the current
    language (see [Language Choice](#Language-Choice)) on the input, then execute the
    resulting program. If the program returns an exit
    status of 0 when executed, run shell commands action-if-true.
    Otherwise, run shell commands action-if-false.

    If input is nonempty use the equivalent of
    `AC_LANG_CONFTEST(input)` to generate the current test source
    file; otherwise reuse the already-existing test source file.
    The input can be made by `AC_LANG_PROGRAM` and friends.
    The input text is expanded as an unquoted here-document, so
    ‘`$`’, ‘`` ` ``’ and some ‘`\`’s should be backslash-escaped.
    See [Here-Documents](#Here_002dDocuments).

    `LDFLAGS` and `LIBS` are used for linking, in addition to the
    compilation flags of the current language (see [Language Choice](#Language-Choice)).
    Additionally, action-if-true can run `./conftest$EXEEXT`
    for further testing.

    In the action-if-false section, the failing exit status is
    available in the shell variable ‘`$?`’. This exit status might be
    that of a failed compilation, or it might be that of a failed program
    execution.

    If cross-compilation mode is enabled (this is the case if either the
    compiler being used does not produce executables that run on the system
    where `configure` is being run, or if the options `--build`
    and `--host` were both specified and their values are different),
    then the test program is
    not run. If the optional shell commands action-if-cross-compiling
    are given, those commands are run instead; typically these commands
    provide pessimistic defaults that allow cross-compilation to work even
    if the guess was wrong. If the fourth argument is empty or omitted, but
    cross-compilation is detected, then `configure` prints an error
    message and exits. If you want your package to be useful in a
    cross-compilation scenario, you *should* provide a non-empty
    action-if-cross-compiling clause, as well as wrap the
    `AC_RUN_IFELSE` compilation inside an `AC_CACHE_CHECK`
    (see [Caching Results](#Caching-Results)) which allows the user to override the
    pessimistic default if needed.

    It is customary to report unexpected failures with
    `AC_MSG_FAILURE`.

`autoconf` prints a warning message when creating
`configure` each time it encounters a call to
`AC_RUN_IFELSE` with no action-if-cross-compiling argument
given. If you are not concerned about users configuring your package
for cross-compilation, you may ignore the warning. A few of the macros
distributed with Autoconf produce this warning message; but if this is a
problem for you, please report it as a bug, along with an appropriate
pessimistic guess to use instead.

To configure for cross-compiling you can also choose a value for those
parameters based on the canonical system name (see [Manual Configuration](#Manual-Configuration)). Alternatively, set up a test results cache file with
the correct values for the host system (see [Caching Results](#Caching-Results)).

To provide a default for calls of `AC_RUN_IFELSE` that are embedded
in other macros, including a few of the ones that come with Autoconf,
you can test whether the shell variable `cross_compiling` is set to
‘`yes`’, and then use an alternate method to get the results instead
of calling the macros.

It is also permissible to temporarily assign to `cross_compiling`
in order to force tests to behave as though they are in a
cross-compilation environment, particularly since this provides a way to
test your action-if-cross-compiling even when you are not using a
cross-compiler.

```
# We temporarily set cross-compile mode to force AC_COMPUTE_INT
# to use the slow link-only method
save_cross_compiling=$cross_compiling
cross_compiling=yes
AC_COMPUTE_INT([...])
cross_compiling=$save_cross_compiling
```

A C or C++ runtime test should be portable.
See [Portable C and C++ Programming](#Portable-C-and-C_002b_002b).

Erlang tests must exit themselves the Erlang VM by calling the `halt/1`
function: the given status code is used to determine the success of the test
(status is `0`) or its failure (status is different than `0`), as
explained above. It must be noted that data output through the standard output
(e.g., using `io:format/2`) may be truncated when halting the VM.
Therefore, if a test must output configuration information, it is recommended
to create and to output data into the temporary file named `conftest.out`,
using the functions of module `file`. The `conftest.out` file is
automatically deleted by the `AC_RUN_IFELSE` macro. For instance, a
simplified implementation of Autoconf’s `AC_ERLANG_SUBST_LIB_DIR`
macro is:

```
AC_INIT([LibdirTest], [1.0], [bug-libdirtest@example.org])
AC_ERLANG_NEED_ERL
AC_LANG(Erlang)
AC_RUN_IFELSE(
  [AC_LANG_PROGRAM([], [dnl
    file:write_file("conftest.out", code:lib_dir()),
    halt(0)])],
  [AS_ECHO(["code:lib_dir() returned: `cat conftest.out`"])],
  [AC_MSG_FAILURE([test Erlang program execution failed])])
```

---

Previous: [Checking Runtime Behavior](#Runtime), Up: [Writing Tests](#Writing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 6.7 Multiple Cases [¶](#Multiple-Cases-1)

Some operations are accomplished in several possible ways, depending on
the OS variant. Checking for them essentially requires a “case
statement”. Autoconf does not directly provide one; however, it is
easy to simulate by using a shell variable to keep track of whether a
way to perform the operation has been found yet.

Here is an example that uses the shell variable `fstype` to keep
track of whether the remaining cases need to be checked. Note that
since the value of `fstype` is under our control, we don’t have to
use the longer ‘`test "x$fstype" = xno`’.

```
AC_MSG_CHECKING([how to get file system type])
fstype=no
# The order of these tests is important.
AC_COMPILE_IFELSE([AC_LANG_PROGRAM([[#include <sys/statvfs.h>
                                     #include <sys/fstyp.h>
                                   ]])],
  [AC_DEFINE([FSTYPE_STATVFS], [1],
     [Define if statvfs exists.])
   fstype=SVR4])
AS_IF([test $fstype = no],
  [AC_COMPILE_IFELSE([AC_LANG_PROGRAM([[#include <sys/statfs.h>
                                        #include <sys/fstyp.h>
                                      ]])],
     [AC_DEFINE([FSTYPE_USG_STATFS], [1],
        [Define if USG statfs.])
      fstype=SVR3])])
AS_IF([test $fstype = no],
  [AC_COMPILE_IFELSE([AC_LANG_PROGRAM([[#include <sys/statfs.h>
                                        #include <sys/vmount.h>
                                      ]])],
     [AC_DEFINE([FSTYPE_AIX_STATFS], [1],
        [Define if AIX statfs.])
      fstype=AIX])])
# (more cases omitted here)
AC_MSG_RESULT([$fstype])
```

---

Next: [Programming in M4](#Programming-in-M4), Previous: [Writing Tests](#Writing-Tests), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 7 Results of Tests [¶](#Results-of-Tests)

Once `configure` has determined whether a feature exists, what can
it do to record that information? There are four sorts of things it can
do: define a C preprocessor symbol, set a variable in the output files,
save the result in a cache file for future `configure` runs, and
print a message letting the user know the result of the test.

* [Defining C Preprocessor Symbols](#Defining-Symbols)
* [Setting Output Variables](#Setting-Output-Variables)
* [Special Characters in Output Variables](#Special-Chars-in-Variables)
* [Caching Results](#Caching-Results)
* [Printing Messages](#Printing-Messages)

---

Next: [Setting Output Variables](#Setting-Output-Variables), Up: [Results of Tests](#Results)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 7.1 Defining C Preprocessor Symbols [¶](#Defining-C-Preprocessor-Symbols)

A common action to take in response to a feature test is to define a C
preprocessor symbol indicating the results of the test. That is done by
calling `AC_DEFINE` or `AC_DEFINE_UNQUOTED`.

By default, `AC_OUTPUT` places the symbols defined by these macros
into the output variable `DEFS`, which contains an option
`-Dsymbol=value` for each symbol defined. Unlike in
Autoconf version 1, there is no variable `DEFS` defined while
`configure` is running. To check whether Autoconf macros have
already defined a certain C preprocessor symbol, test the value of the
appropriate cache variable, as in this example:

```
AC_CHECK_FUNC([vprintf],
  [AC_DEFINE([HAVE_VPRINTF], [1],
     [Define if vprintf exists.])])
AS_IF([test "x$ac_cv_func_vprintf" != xyes],
  [AC_CHECK_FUNC([_doprnt],
     [AC_DEFINE([HAVE_DOPRNT], [1],
        [Define if _doprnt exists.])])])
```

If `AC_CONFIG_HEADERS` has been called, then instead of creating
`DEFS`, `AC_OUTPUT` creates a header file by substituting the
correct values into `#define` statements in a template file.
See [Configuration Header Files](#Configuration-Headers), for more information about this kind of
output.

Macro: **AC\_DEFINE**(variable, value, [description]) [¶](#index-AC_005fDEFINE-1)

Macro: **AC\_DEFINE**(variable) [¶](#index-AC_005fDEFINE-2)
:   Define variable to value (verbatim), by defining a C
    preprocessor macro for variable. variable should be a C
    identifier, optionally suffixed by a parenthesized argument list to
    define a C preprocessor macro with arguments. The macro argument list,
    if present, should be a comma-separated list of C identifiers, possibly
    terminated by an ellipsis ‘`...`’ if C99-or-later syntax is employed.
    variable should not contain comments, white space, trigraphs,
    backslash-newlines, universal character names, or non-ASCII
    characters.

    value may contain backslash-escaped newlines, which will be
    preserved if you use `AC_CONFIG_HEADERS` but flattened if passed
    via `@DEFS@` (with no effect on the compilation, since the
    preprocessor sees only one line in the first place). value should
    not contain raw newlines. If you are not using
    `AC_CONFIG_HEADERS`, value should not contain any ‘`#`’
    characters, as `make` tends to eat them. To use a shell
    variable, use `AC_DEFINE_UNQUOTED` instead.

    description is only useful if you are using
    `AC_CONFIG_HEADERS`. In this case, description is put into
    the generated `config.h.in` as the comment before the macro define.
    The following example defines the C preprocessor variable
    `EQUATION` to be the string constant ‘`"$a > $b"`’:

    ```
    AC_DEFINE([EQUATION], ["$a > $b"],
      [Equation string.])
    ```

    If neither value nor description are given, then
    value defaults to 1 instead of to the empty string. This is for
    backwards compatibility with older versions of Autoconf, but this usage
    is obsolescent and may be withdrawn in future versions of Autoconf.

    If the variable is a literal string, it is passed to
    `m4_pattern_allow` (see [Forbidden Patterns](#Forbidden-Patterns)).

    If multiple `AC_DEFINE` statements are executed for the same
    variable name (not counting any parenthesized argument list),
    the last one wins.

Macro: **AC\_DEFINE\_UNQUOTED**(variable, value, [description]) [¶](#index-AC_005fDEFINE_005fUNQUOTED-1)

Macro: **AC\_DEFINE\_UNQUOTED**(variable) [¶](#index-AC_005fDEFINE_005fUNQUOTED-2)
:   Like `AC_DEFINE`, but three shell expansions are
    performed—once—on variable and value: variable expansion
    (‘`$`’), command substitution (‘`` ` ``’), and backslash escaping
    (‘`\`’), as if in an unquoted here-document. Single and double quote
    characters in the value have no
    special meaning. Use this macro instead of `AC_DEFINE` when
    variable or value is a shell variable. Examples:

    ```
    AC_DEFINE_UNQUOTED([config_machfile], ["$machfile"],
      [Configuration machine file.])
    AC_DEFINE_UNQUOTED([GETGROUPS_T], [$ac_cv_type_getgroups],
      [getgroups return type.])
    AC_DEFINE_UNQUOTED([$ac_tr_hdr], [1],
      [Translated header name.])
    ```

Due to a syntactical oddity of the Bourne shell, do not use
semicolons to separate `AC_DEFINE` or `AC_DEFINE_UNQUOTED`
calls from other macro calls or shell code; that can cause syntax errors
in the resulting `configure` script. Use either blanks or
newlines. That is, do this:

```
AC_CHECK_HEADER([elf.h],
  [AC_DEFINE([SVR4], [1], [System V Release 4]) LIBS="-lelf $LIBS"])
```

or this:

```
AC_CHECK_HEADER([elf.h],
  [AC_DEFINE([SVR4], [1], [System V Release 4])
   LIBS="-lelf $LIBS"])
```

instead of this:

```
AC_CHECK_HEADER([elf.h],
  [AC_DEFINE([SVR4], [1], [System V Release 4]); LIBS="-lelf $LIBS"])
```

---

Next: [Special Characters in Output Variables](#Special-Chars-in-Variables), Previous: [Defining C Preprocessor Symbols](#Defining-Symbols), Up: [Results of Tests](#Results)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 7.2 Setting Output Variables [¶](#Setting-Output-Variables-1)

Another way to record the results of tests is to set *output
variables*, which are shell variables whose values are substituted into
files that `configure` outputs. The two macros below create new
output variables. See [Preset Output Variables](#Preset-Output-Variables), for a list of output
variables that are always available.

Macro: **AC\_SUBST**(variable, [value]) [¶](#index-AC_005fSUBST-1)
:   Create an output variable from a shell variable. Make `AC_OUTPUT`
    substitute the variable variable into output files (typically one
    or more makefiles). This means that `AC_OUTPUT`
    replaces instances of ‘`@variable@`’ in input files with the
    value that the shell variable variable has when `AC_OUTPUT`
    is called. The value can contain any non-`NUL` character, including
    newline. If you are using Automake 1.11 or newer, for newlines in values
    you might want to consider using `AM_SUBST_NOTMAKE` to prevent
    `automake` from adding a line `variable =
    @variable@` to the `Makefile.in` files (see [Automake](https://www.gnu.org/software/automake/manual/automake.html#Optional) in Other things Automake recognizes).

    Variable occurrences should not overlap: e.g., an input file should
    not contain ‘`@var1@var2@`’ if var1 and var2
    are variable names.
    The substituted value is not rescanned for more output variables;
    occurrences of ‘`@variable@`’ in the value are inserted
    literally into the output file. (The algorithm uses the special marker
    `|#_!!_#|` internally, so neither the substituted value nor the
    output file may contain `|#_!!_#|`.)

    If value is given, in addition assign it to variable.

    The string variable is passed to `m4_pattern_allow`
    (see [Forbidden Patterns](#Forbidden-Patterns)). variable is not further expanded,
    even if there is another macro by the same name.

Macro: **AC\_SUBST\_FILE**(variable) [¶](#index-AC_005fSUBST_005fFILE-1)
:   Another way to create an output variable from a shell variable. Make
    `AC_OUTPUT` insert (without substitutions) the contents of the file
    named by shell variable variable into output files. This means
    that `AC_OUTPUT` replaces instances of
    ‘`@variable@`’ in output files (such as `Makefile.in`)
    with the contents of the file that the shell variable variable
    names when `AC_OUTPUT` is called. Set the variable to
    `/dev/null` for cases that do not have a file to insert.
    This substitution occurs only when the ‘`@variable@`’ is on a
    line by itself, optionally surrounded by spaces and tabs. The
    substitution replaces the whole line, including the spaces, tabs, and
    the terminating newline.

    This macro is useful for inserting makefile fragments containing
    special dependencies or other `make` directives for particular host
    or target types into makefiles. For example, `configure.ac`
    could contain:

    ```
    AC_SUBST_FILE([host_frag])
    host_frag=$srcdir/conf/sun4.mh
    ```

    and then a `Makefile.in` could contain:

    ```
    @host_frag@
    ```

    The string variable is passed to `m4_pattern_allow`
    (see [Forbidden Patterns](#Forbidden-Patterns)).

Running `configure` in varying environments can be extremely
dangerous. If for instance the user runs ‘`CC=bizarre-cc
./configure`’, then the cache, `config.h`, and many other output
files depend upon `bizarre-cc` being the C compiler. If
for some reason the user runs `./configure` again, or if it is
run via ‘`./config.status --recheck`’, (See [Automatic Remaking](#Automatic-Remaking),
and see [config.status Invocation](#config_002estatus-Invocation)), then the configuration can be
inconsistent, composed of results depending upon two different
compilers.

Environment variables that affect this situation, such as ‘`CC`’
above, are called *precious variables*, and can be declared as such
by `AC_ARG_VAR`.

Macro: **AC\_ARG\_VAR**(variable, description) [¶](#index-AC_005fARG_005fVAR-1)
:   Declare variable is a precious variable, and include its
    description in the variable section of ‘`./configure --help`’.

    Being precious means that

    * variable is substituted via `AC_SUBST`.
    * The value of variable when `configure` was launched is
      saved in the cache, including if it was not specified on the command
      line but via the environment. Indeed, while `configure` can
      notice the definition of `CC` in ‘`./configure CC=bizarre-cc`’,
      it is impossible to notice it in ‘`CC=bizarre-cc ./configure`’,
      which, unfortunately, is what most users do.

      We emphasize that it is the *initial* value of variable which
      is saved, not that found during the execution of `configure`.
      Indeed, specifying ‘`./configure FOO=foo`’ and letting
      ‘`./configure`’ guess that `FOO` is `foo` can be two
      different things.
    * variable is checked for consistency between two
      `configure` runs. For instance:

      ```
      $ ./configure --silent --config-cache
      $ CC=cc ./configure --silent --config-cache
      configure: error: 'CC' was not set in the previous run
      configure: error: changes in the environment can compromise \
      the build
      configure: error: run 'make distclean' and/or \
      'rm config.cache' and start over
      ```

      and similarly if the variable is unset, or if its content is changed.
      If the content has white space changes only, then the error is degraded
      to a warning only, but the old value is reused.
    * variable is kept during automatic reconfiguration
      (see [config.status Invocation](#config_002estatus-Invocation)) as if it had been passed as a command
      line argument, including when no cache is used:

      ```
      $ CC=/usr/bin/cc ./configure var=raboof --silent
      $ ./config.status --recheck
      running CONFIG_SHELL=/bin/sh /bin/sh ./configure var=raboof \
        CC=/usr/bin/cc  --no-create --no-recursion
      ```

---

Next: [Caching Results](#Caching-Results), Previous: [Setting Output Variables](#Setting-Output-Variables), Up: [Results of Tests](#Results)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 7.3 Special Characters in Output Variables [¶](#Special-Characters-in-Output-Variables)

Many output variables are intended to be evaluated both by
`make` and by the shell. Some characters are expanded
differently in these two contexts, so to avoid confusion these
variables’ values should not contain any of the following characters:

```
" # $ & ' ( ) * ; < > ? [ \ ^ ` |
```

Also, these variables’ values should neither contain newlines, nor start
with ‘`~`’, nor contain white space or ‘`:`’ immediately followed
by ‘`~`’. The values can contain nonempty sequences of white space
characters like tabs and spaces, but each such sequence might
arbitrarily be replaced by a single space during substitution.

These restrictions apply both to the values that `configure`
computes, and to the values set directly by the user. For example, the
following invocations of `configure` are problematic, since they
attempt to use special characters within `CPPFLAGS` and white space
within `$(srcdir)`:

```
CPPFLAGS='-DOUCH="&\"#$*?"' '../My Source/ouch-1.0/configure'

'../My Source/ouch-1.0/configure' CPPFLAGS='-DOUCH="&\"#$*?"'
```

---

Next: [Printing Messages](#Printing-Messages), Previous: [Special Characters in Output Variables](#Special-Chars-in-Variables), Up: [Results of Tests](#Results)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 7.4 Caching Results [¶](#Caching-Results-1)

To avoid checking for the same features repeatedly in various
`configure` scripts (or in repeated runs of one script),
`configure` can optionally save the results of many checks in a
*cache file* (see [Cache Files](#Cache-Files)). If a `configure` script
runs with caching enabled and finds a cache file, it reads the results
of previous runs from the cache and avoids rerunning those checks. As a
result, `configure` can then run much faster than if it had to
perform all of the checks every time.

Macro: **AC\_CACHE\_VAL**(cache-id, commands-to-set-it) [¶](#index-AC_005fCACHE_005fVAL-1)
:   Ensure that the results of the check identified by cache-id are
    available. If the results of the check were in the cache file that was
    read, and `configure` was not given the `--quiet` or
    `--silent` option, print a message saying that the result was
    cached; otherwise, run the shell commands commands-to-set-it. If
    the shell commands are run to determine the value, the value is
    saved in the cache file just before `configure` creates its output
    files. See [Cache Variable Names](#Cache-Variable-Names), for how to choose the name of the
    cache-id variable.

    The commands-to-set-it *must have no side effects* except for
    setting the variable cache-id, see below.

Macro: **AC\_CACHE\_CHECK**(message, cache-id, commands-to-set-it) [¶](#index-AC_005fCACHE_005fCHECK-1)
:   A wrapper for `AC_CACHE_VAL` that takes care of printing the
    messages. This macro provides a convenient shorthand for the most
    common way to use these macros. It calls `AC_MSG_CHECKING` for
    message, then `AC_CACHE_VAL` with the cache-id and
    commands arguments, and `AC_MSG_RESULT` with cache-id.

    The commands-to-set-it *must have no side effects* except for
    setting the variable cache-id, see below.

It is common to find buggy macros using `AC_CACHE_VAL` or
`AC_CACHE_CHECK`, because people are tempted to call
`AC_DEFINE` in the commands-to-set-it. Instead, the code that
*follows* the call to `AC_CACHE_VAL` should call
`AC_DEFINE`, by examining the value of the cache variable. For
instance, the following macro is broken:

```
AC_DEFUN([AC_SHELL_TRUE],
[AC_CACHE_CHECK([whether true(1) works], [my_cv_shell_true_works],
                [my_cv_shell_true_works=no
                 (true) 2>/dev/null && my_cv_shell_true_works=yes
                 if test "x$my_cv_shell_true_works" = xyes; then
                   AC_DEFINE([TRUE_WORKS], [1],
                             [Define if 'true(1)' works properly.])
                 fi])
])
```

This fails if the cache is enabled: the second time this macro is run,
`TRUE_WORKS` *will not be defined*. The proper implementation
is:

```
AC_DEFUN([AC_SHELL_TRUE],
[AC_CACHE_CHECK([whether true(1) works], [my_cv_shell_true_works],
                [my_cv_shell_true_works=no
                 (true) 2>/dev/null && my_cv_shell_true_works=yes])
 if test "x$my_cv_shell_true_works" = xyes; then
   AC_DEFINE([TRUE_WORKS], [1],
             [Define if 'true(1)' works properly.])
 fi
])
```

Also, commands-to-set-it should not print any messages, for
example with `AC_MSG_CHECKING`; do that before calling
`AC_CACHE_VAL`, so the messages are printed regardless of whether
the results of the check are retrieved from the cache or determined by
running the shell commands.

* [Cache Variable Names](#Cache-Variable-Names)
* [Cache Files](#Cache-Files)
* [Cache Checkpointing](#Cache-Checkpointing)

---

Next: [Cache Files](#Cache-Files), Up: [Caching Results](#Caching-Results)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 7.4.1 Cache Variable Names [¶](#Cache-Variable-Names-1)

The names of cache variables should have the following format:

```
package-prefix_cv_value-type_specific-value_[additional-options]
```

for example, ‘`ac_cv_header_stat_broken`’ or
‘`ac_cv_prog_gcc_traditional`’. The parts of the variable name are:

package-prefix
:   An abbreviation for your package or organization; the same prefix you
    begin local Autoconf macros with, except lowercase by convention.
    For cache values used by the distributed Autoconf macros, this value is
    ‘`ac`’.

`_cv_`
:   Indicates that this shell variable is a cache value. This string
    *must* be present in the variable name, including the leading
    underscore.

value-type
:   A convention for classifying cache values, to produce a rational naming
    system. The values used in Autoconf are listed in [Macro Names](#Macro-Names).

specific-value
:   Which member of the class of cache values this test applies to.
    For example, which function (‘`alloca`’), program (‘`gcc`’), or
    output variable (‘`INSTALL`’).

additional-options
:   Any particular behavior of the specific member that this test applies to.
    For example, ‘`broken`’ or ‘`set`’. This part of the name may
    be omitted if it does not apply.

The values assigned to cache variables may not contain newlines.
Usually, their values are Boolean (‘`yes`’ or ‘`no`’) or the
names of files or functions; so this is not an important restriction.
[Cache Variable Index](#Cache-Variable-Index) for an index of cache variables with
documented semantics.

---

Next: [Cache Checkpointing](#Cache-Checkpointing), Previous: [Cache Variable Names](#Cache-Variable-Names), Up: [Caching Results](#Caching-Results)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 7.4.2 Cache Files [¶](#Cache-Files-1)

A cache file is a shell script that caches the results of configure
tests run on one system so they can be shared between configure scripts
and configure runs. It is not useful on other systems. If its contents
are invalid for some reason, the user may delete or edit it, or override
documented cache variables on the `configure` command line.

By default, `configure` uses no cache file,
to avoid problems caused by accidental
use of stale cache files.

To enable caching, `configure` accepts `--config-cache` (or
`-C`) to cache results in the file `config.cache`.
Alternatively, `--cache-file=file` specifies that
file be the cache file. The cache file is created if it does not
exist already. When `configure` calls `configure` scripts in
subdirectories, it uses the `--cache-file` argument so that they
share the same cache. See [Configuring Other Packages in Subdirectories](#Subdirectories), for information on
configuring subdirectories with the `AC_CONFIG_SUBDIRS` macro.

`config.status` only pays attention to the cache file if it is
given the `--recheck` option, which makes it rerun
`configure`.

It is wrong to try to distribute cache files for particular system types.
There is too much room for error in doing that, and too much
administrative overhead in maintaining them. For any features that
can’t be guessed automatically, use the standard method of the canonical
system type and linking files (see [Manual Configuration](#Manual-Configuration)).

The site initialization script can specify a site-wide cache file to
use, instead of the usual per-program cache. In this case, the cache
file gradually accumulates information whenever someone runs a new
`configure` script. (Running `configure` merges the new cache
results with the existing cache file.) This may cause problems,
however, if the system configuration (e.g., the installed libraries or
compilers) changes and the stale cache file is not deleted.

If `configure` is interrupted at the right time when it updates
a cache file outside of the build directory where the `configure`
script is run, it may leave behind a temporary file named after the
cache file with digits following it. You may safely delete such a file.

---

Previous: [Cache Files](#Cache-Files), Up: [Caching Results](#Caching-Results)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 7.4.3 Cache Checkpointing [¶](#Cache-Checkpointing-1)

If your configure script, or a macro called from `configure.ac`, happens
to abort the configure process, it may be useful to checkpoint the cache
a few times at key points using `AC_CACHE_SAVE`. Doing so
reduces the amount of time it takes to rerun the configure script with
(hopefully) the error that caused the previous abort corrected.

Macro: **AC\_CACHE\_LOAD** [¶](#index-AC_005fCACHE_005fLOAD-1)
:   Loads values from existing cache file, or creates a new cache file if a
    cache file is not found. Called automatically from `AC_INIT`.

Macro: **AC\_CACHE\_SAVE** [¶](#index-AC_005fCACHE_005fSAVE-1)
:   Flushes all cached values to the cache file. Called automatically from
    `AC_OUTPUT`, but it can be quite useful to call
    `AC_CACHE_SAVE` at key points in `configure.ac`.

For instance:

```
 ... AC_INIT, etc. ...
```

```
# Checks for programs.
AC_PROG_CC
AC_PROG_AWK
 ... more program checks ...
AC_CACHE_SAVE
```

```

```

```
# Checks for libraries.
AC_CHECK_LIB([nsl], [gethostbyname])
AC_CHECK_LIB([socket], [connect])
 ... more lib checks ...
AC_CACHE_SAVE
```

```

```

```
# Might abort...
AM_PATH_GTK([1.0.2], [], [AC_MSG_ERROR([GTK not in path])])
AM_PATH_GTKMM([0.9.5], [], [AC_MSG_ERROR([GTK not in path])])
```

```
 ... AC_OUTPUT, etc. ...
```

---

Previous: [Caching Results](#Caching-Results), Up: [Results of Tests](#Results)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 7.5 Printing Messages [¶](#Printing-Messages-1)

`configure` scripts need to give users running them several kinds
of information. The following macros print messages in ways appropriate
for each kind. The arguments to all of them get enclosed in shell
double quotes, so the shell performs variable and back-quote
substitution on them.

These macros are all wrappers around the `printf` shell command.
They direct output to the appropriate file descriptor (see [File Descriptor Macros](#File-Descriptor-Macros)).
`configure` scripts should rarely need to run `printf` directly
to print messages for the user. Using these macros makes it easy to
change how and when each kind of message is printed; such changes need
only be made to the macro definitions and all the callers change
automatically.

To diagnose static issues, i.e., when `autoconf` is run, see
[Diagnostic messages from M4sugar](#Diagnostic-Macros).

Macro: **AC\_MSG\_CHECKING**(feature-description) [¶](#index-AC_005fMSG_005fCHECKING-1)
:   Notify the user that `configure` is checking for a particular
    feature. This macro prints a message that starts with ‘`checking` ’
    and ends with ‘`...`’ and no newline. It must be followed by a call
    to `AC_MSG_RESULT` to print the result of the check and the
    newline. The feature-description should be something like
    ‘`whether the Fortran compiler accepts C++ comments`’ or ‘`for
    _Alignof`’.

    This macro prints nothing if `configure` is run with the
    `--quiet` or `--silent` option.

Macro: **AC\_MSG\_RESULT**(result-description) [¶](#index-AC_005fMSG_005fRESULT-1)
:   Notify the user of the results of a check. result-description is
    almost always the value of the cache variable for the check, typically
    ‘`yes`’, ‘`no`’, or a file name. This macro should follow a call
    to `AC_MSG_CHECKING`, and the result-description should be
    the completion of the message printed by the call to
    `AC_MSG_CHECKING`.

    This macro prints nothing if `configure` is run with the
    `--quiet` or `--silent` option.

Macro: **AC\_MSG\_NOTICE**(message) [¶](#index-AC_005fMSG_005fNOTICE-1)
:   Deliver the message to the user. It is useful mainly to print a
    general description of the overall purpose of a group of feature checks,
    e.g.,

    ```
    AC_MSG_NOTICE([checking if stack overflow is detectable])
    ```

    This macro prints nothing if `configure` is run with the
    `--quiet` or `--silent` option.

Macro: **AC\_MSG\_ERROR**(error-description, [exit-status = ‘`$?/1`’]) [¶](#index-AC_005fMSG_005fERROR-1)
:   Notify the user of an error that prevents `configure` from
    completing. This macro prints an error message to the standard error
    output and exits `configure` with exit-status (‘`$?`’
    by default, except that ‘`0`’ is converted to ‘`1`’).
    error-description should be something like ‘`invalid value
    $HOME for \$HOME`’.

    The error-description should start with a lower-case letter, and
    “cannot” is preferred to “can’t”.

Macro: **AC\_MSG\_FAILURE**(error-description, [exit-status]) [¶](#index-AC_005fMSG_005fFAILURE-1)
:   This `AC_MSG_ERROR` wrapper notifies the user of an error that
    prevents `configure` from completing *and* that additional
    details are provided in `config.log`. This is typically used when
    abnormal results are found during a compilation.

Macro: **AC\_MSG\_WARN**(problem-description) [¶](#index-AC_005fMSG_005fWARN-1)
:   Notify the `configure` user of a possible problem. This macro
    prints the message to the standard error output; `configure`
    continues running afterward, so macros that call `AC_MSG_WARN` should
    provide a default (back-up) behavior for the situations they warn about.
    problem-description should be something like ‘`ln -s seems to
    make hard links`’.

---

Next: [Programming in M4sh](#Programming-in-M4sh), Previous: [Results of Tests](#Results), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 8 Programming in M4 [¶](#Programming-in-M4-1)

Autoconf is written on top of two layers: *M4sugar*, which provides
convenient macros for pure M4 programming, and *M4sh*, which
provides macros dedicated to shell script generation.

As of this version of Autoconf, these two layers still contain
experimental macros, whose interface might change in the future. As a
matter of fact, *anything that is not documented must not be used*.

* [M4 Quotation](#M4-Quotation)
* [Using `autom4te`](#Using-autom4te)
* [Programming in M4sugar](#Programming-in-M4sugar)
* [Debugging via autom4te](#Debugging-via-autom4te)

---

Next: [Using `autom4te`](#Using-autom4te), Up: [Programming in M4](#Programming-in-M4)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.1 M4 Quotation [¶](#M4-Quotation-1)

The most common problem with existing macros is an improper quotation.
This section, which users of Autoconf can skip, but which macro writers
*must* read, first justifies the quotation scheme that was chosen
for Autoconf and then ends with a rule of thumb. Understanding the
former helps one to follow the latter.

* [Active Characters](#Active-Characters)
* [One Macro Call](#One-Macro-Call)
* [Quoting and Parameters](#Quoting-and-Parameters)
* [Quotation and Nested Macros](#Quotation-and-Nested-Macros)
* [`changequote` is Evil](#Changequote-is-Evil)
* [Quadrigraphs](#Quadrigraphs)
* [Dealing with unbalanced parentheses](#Balancing-Parentheses)
* [Quotation Rule Of Thumb](#Quotation-Rule-Of-Thumb)

---

Next: [One Macro Call](#One-Macro-Call), Up: [M4 Quotation](#M4-Quotation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.1.1 Active Characters [¶](#Active-Characters-1)

To fully understand where proper quotation is important, you first need
to know what the special characters are in Autoconf: ‘`#`’ introduces
a comment inside which no macro expansion is performed, ‘`,`’
separates arguments, ‘`[`’ and ‘`]`’ are the quotes
themselves[3](#FOOT3),
‘`(`’ and ‘`)`’ (which M4 tries to match by pairs), and finally
‘`$`’ inside a macro definition.

In order to understand the delicate case of macro calls, we first have
to present some obvious failures. Below they are “obvious-ified”,
but when you find them in real life, they are usually in disguise.

Comments, introduced by a hash and running up to the newline, are opaque
tokens to the top level: active characters are turned off, and there is
no macro expansion:

```
