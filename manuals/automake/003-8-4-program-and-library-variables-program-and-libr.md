### 8.4 Program and Library Variables [¶](#Program-and-Library-Variables-1)

Associated with each program is a collection of variables that can be
used to modify how that program is built. There is a similar list of
such variables for each library. The canonical name of the program (or
library) is used as a base for naming these variables.

In the list below, we use the name “maude” to refer to the program or
library. In your `Makefile.am` you would replace this with the
canonical name of your program. This list also refers to “maude” as a
program, but in general the same rules apply for both static and dynamic
libraries; the documentation below notes situations where programs and
libraries differ.

`maude_SOURCES` [¶](#index-maude_005fSOURCES)
:   This variable, if it exists, lists all the source files that are
    compiled to build the program. These files are added to the
    distribution by default. When building the program, Automake will cause
    each source file to be compiled to a single `.o` file (or
    `.lo` when using Libtool). Normally these object files are named
    after the source file, but other factors can change this. If a file in
    the `_SOURCES` variable has an unrecognized extension, Automake
    will do one of two things with it. If a suffix rule exists for turning
    files with the unrecognized extension into `.o` files, then
    `automake` will treat this file as it will any other source file
    (see [Support for Other Languages](#Support-for-Other-Languages)). Otherwise, the file will be
    ignored as though it were a header file.

    The prefixes `dist_` and `nodist_` can be used to control
    whether files listed in a `_SOURCES` variable are distributed.
    `dist_` is redundant, as sources are distributed by default, but it
    can be specified for clarity if desired.

    It is possible to have both `dist_` and `nodist_` variants of
    a given `_SOURCES` variable at once; this lets you easily
    distribute some files and not others, for instance:

    ```
    nodist_maude_SOURCES = nodist.c
    dist_maude_SOURCES = dist-me.c
    ```

    By default the output file (on Unix systems, the `.o` file) will
    be put into the current build directory. However, if the option
    `subdir-objects` is in effect in the current directory then the
    `.o` file will be put into the subdirectory named after the
    source file. For instance, with `subdir-objects` enabled,
    `sub/dir/file.c` will be compiled to `sub/dir/file.o`. Some
    projects prefer or require this mode of operation. You can specify
    `subdir-objects` in `AUTOMAKE_OPTIONS` (see [Changing Automake’s Behavior](#Options)).

    When `subdir-objects` is specified, and source files which lie
    outside the current directory tree are nevertheless specified, as in
    `foo_SOURCES = ../lib/other.c`, Automake will still remove
    `../lib/other.o`, in fact, `../lib/*.o` (e.g., at `make
    clean`, even though it is arguably wrong for one subdirectory to
    clean in a sibling. This may or may not be changed in the future.

`EXTRA_maude_SOURCES` [¶](#index-EXTRA_005fmaude_005fSOURCES)
:   Automake needs to know the list of files you intend to compile
    *statically*. For one thing, this is the only way Automake has of
    knowing what sort of language support a given `Makefile.in`
    requires. (There are other, more obscure reasons for
    this limitation as well.) This means that, for example, you can’t put a
    configure substitution like ‘`@my_sources@`’ into a ‘`_SOURCES`’
    variable. If you intend to conditionally compile source files and use
    `configure` to substitute the appropriate object names into, e.g.,
    `_LDADD` (see below), then you should list the corresponding source
    files in the `EXTRA_` variable.

    This variable also supports `dist_` and `nodist_` prefixes.
    For instance, `nodist_EXTRA_maude_SOURCES` would list extra
    sources that may need to be built, but should not be distributed.

`maude_AR` [¶](#index-maude_005fAR)
:   A static library is created by default by invoking ‘`$(AR)
    $(ARFLAGS)`’ followed by the name of the library and then the objects
    being put into the library. You can override this by setting the
    `_AR` variable. This is usually used with C++; some C++
    compilers require a special invocation in order to instantiate all the
    templates that should go into a library. For instance, the SGI C++
    compiler likes this variable set like so:

    ```
    libmaude_a_AR = $(CXX) -ar -o
    ```

`maude_RANLIB` [¶](#index-maude_005fRANLIB)
:   A static library’s index is updated by default by invoking ‘`$(RANLIB)`’
    followed by the name of the library. You can override this by setting the
    `_RANLIB` variable.

`maude_LIBADD` [¶](#index-maude_005fLIBADD-1)
:   Extra objects can be added to a *library* using the `_LIBADD`
    variable. For instance, this should be used for objects determined by
    `configure` (see [Building a library](#A-Library)).

    In the case of Libtool libraries, `maude_LIBADD` can also refer
    to other Libtool libraries.

`maude_LDADD` [¶](#index-maude_005fLDADD-1)
:   Extra objects (`*.$(OBJEXT)`) and libraries (`*.a`,
    `*.la`) can be added to a *program* by listing them in the
    `_LDADD` variable. For instance, this should be used for objects
    determined by `configure` (see [Linking the program](#Linking)).

    `_LDADD` and `_LIBADD` are inappropriate for passing
    program-specific linker flags (except for `-l`, `-L`,
    `-dlopen` and `-dlpreopen`). Use the `_LDFLAGS` variable
    for this purpose.

    For instance, if your `configure.ac` uses `AC_PATH_XTRA`, you
    could link your program against the X libraries like so:

    ```
    maude_LDADD = $(X_PRE_LIBS) $(X_LIBS) $(X_EXTRA_LIBS)
    ```

    We recommend that you use `-l` and `-L` only when
    referring to third-party libraries, and give the explicit file names
    of any library built by your package. Doing so will ensure that
    `maude_DEPENDENCIES` (see below) is correctly defined by default.

`maude_LDFLAGS` [¶](#index-maude_005fLDFLAGS-1)
:   This variable is used to pass extra flags to the link step of a program
    or a shared library. It overrides the `AM_LDFLAGS` variable,
    even if it is defined only in a false branch of a conditional; in
    other words, if `prog_LDFLAGS` is defined at all,
    `AM_LDFLAGS` will not be used.

`maude_LIBTOOLFLAGS` [¶](#index-maude_005fLIBTOOLFLAGS-1)
:   This variable is used to pass extra options to `libtool`.
    It overrides the `AM_LIBTOOLFLAGS` variable.
    These options are output before `libtool`’s `--mode=mode`
    option, so they should not be mode-specific options (those belong to
    the compiler or linker flags). See [`_LIBADD`, `_LDFLAGS`, and `_LIBTOOLFLAGS`](#Libtool-Flags).

`maude_DEPENDENCIES` [¶](#index-maude_005fDEPENDENCIES-1)

`EXTRA_maude_DEPENDENCIES` [¶](#index-EXTRA_005fmaude_005fDEPENDENCIES-1)
:   It is also occasionally useful to have a target (program or library)
    depend on some other file that is not in fact part of that target.
    This can be done using the `_DEPENDENCIES` variable. Each
    target depends on the contents of such a variable, but no further
    interpretation is done.

    Since these dependencies are associated with the link rule used to
    create the programs they should normally list files used by the link
    command. That is `*.$(OBJEXT)`, `*.a`, or `*.la` files
    for programs; `*.lo` and `*.la` files for Libtool libraries;
    and `*.$(OBJEXT)` files for static libraries. In rare cases you
    may need to add other kinds of files such as linker scripts, but
    *listing a source file in `_DEPENDENCIES` is wrong*. If
    some source file needs to be built before all the components of a
    program are built, consider using the `BUILT_SOURCES` variable
    (see [Built Sources](#Sources)).

    If `_DEPENDENCIES` is not supplied, it is computed by Automake.
    The automatically-assigned value is the contents of `_LDADD` or
    `_LIBADD`, with most configure substitutions, `-l`, `-L`,
    `-dlopen` and `-dlpreopen` options removed. The configure
    substitutions that are left in are only ‘`$(LIBOBJS)`’ and
    ‘`$(ALLOCA)`’; these are left because it is known that they will not
    cause an invalid value for `_DEPENDENCIES` to be generated.

    `_DEPENDENCIES` is more likely used to perform conditional
    compilation using an `AC_SUBST` variable that contains a list of
    objects. See [Conditional compilation of sources](#Conditional-Sources), and [Libtool Libraries with Conditional Sources](#Conditional-Libtool-Sources).

    The `EXTRA_*_DEPENDENCIES` variable may be useful for cases where
    you merely want to augment the `automake`-generated
    `_DEPENDENCIES` variable rather than replacing it.

`maude_LINK` [¶](#index-maude_005fLINK)
:   You can override the linker on a per-program basis. By default the
    linker is chosen according to the languages used by the program. For
    instance, a program that includes C++ source code would use the C++
    compiler to link. The `_LINK` variable must hold the name of a
    command that can be passed all the `.o` file names and libraries
    to link against as arguments. Note that the name of the underlying
    program is *not* passed to `_LINK`; typically one uses
    ‘`$@`’:

    ```
    maude_LINK = $(CCLD) -magic -o $@
    ```

    If a `_LINK` variable is not supplied, it may still be generated
    and used by Automake due to the use of per-target link flags such as
    `_CFLAGS`, `_LDFLAGS` or `_LIBTOOLFLAGS`, in cases where
    they apply.

    If the variable `AM_V_*_LINK` exists, it is used to output a
    status line in silent mode; otherwise, `AM_V_GEN` is used.

`maude_CCASFLAGS` [¶](#index-maude_005fCCASFLAGS)

`maude_CFLAGS` [¶](#index-maude_005fCFLAGS)

`maude_CPPFLAGS` [¶](#index-maude_005fCPPFLAGS)

`maude_CXXFLAGS` [¶](#index-maude_005fCXXFLAGS)

`maude_FFLAGS` [¶](#index-maude_005fFFLAGS)

`maude_GCJFLAGS` [¶](#index-maude_005fGCJFLAGS)

`maude_LFLAGS` [¶](#index-maude_005fLFLAGS)

`maude_OBJCFLAGS` [¶](#index-maude_005fOBJCFLAGS)

`maude_OBJCXXFLAGS` [¶](#index-maude_005fOBJCXXFLAGS)

`maude_RFLAGS` [¶](#index-maude_005fRFLAGS)

`maude_UPCFLAGS` [¶](#index-maude_005fUPCFLAGS)

`maude_YFLAGS` [¶](#index-maude_005fYFLAGS)
:   Automake allows you to set compilation flags on a per-program (or
    per-library) basis. A single source file can be included in several
    programs, and it will potentially be compiled with different flags for
    each program. This works for any language directly supported by
    Automake. These *per-target compilation flags* are
    ‘`_CCASFLAGS`’,
    ‘`_CFLAGS`’,
    ‘`_CPPFLAGS`’,
    ‘`_CXXFLAGS`’,
    ‘`_FFLAGS`’,
    ‘`_GCJFLAGS`’,
    ‘`_LFLAGS`’,
    ‘`_OBJCFLAGS`’,
    ‘`_OBJCXXFLAGS`’,
    ‘`_RFLAGS`’,
    ‘`_UPCFLAGS`’, and
    ‘`_YFLAGS`’.

    When using a per-target compilation flag, Automake will choose a
    different name for the intermediate object files. Ordinarily a file
    like `sample.c` will be compiled to produce `sample.o`.
    However, if the program’s `_CFLAGS` variable is set, then the
    object file will be named, for instance, `maude-sample.o`. (See
    also [Why are object files sometimes renamed?](#Renamed-Objects).)

    In compilations with per-target flags, the ordinary ‘`AM_`’ form of
    the flags variable is *not* automatically included in the
    compilation (however, the user form of the variable *is* included).
    So for instance, if you want the hypothetical `maude` compilations
    to also use the value of `AM_CFLAGS`, you would need to write:

    ```
    maude_CFLAGS = ... your flags ... $(AM_CFLAGS)
    ```

    See [Flag Variables Ordering](#Flag-Variables-Ordering), for more discussion about the
    interaction between user variables, ‘`AM_`’ shadow variables, and
    per-target variables.

`maude_SHORTNAME` [¶](#index-maude_005fSHORTNAME)
:   On some platforms the allowable file names are very short. In order to
    support these systems and per-target compilation flags at the same
    time, Automake allows you to set a “short name” that will influence
    how intermediate object files are named. For instance, in the following
    example,

    ```
    bin_PROGRAMS = maude
    maude_CPPFLAGS = -DSOMEFLAG
    maude_SHORTNAME = m
    maude_SOURCES = sample.c ...
    ```

    the object file would be named `m-sample.o` rather than
    `maude-sample.o`.

    This facility is rarely needed in practice,
    and we recommend avoiding it until you find it is required.

---

Next: [Special handling for `LIBOBJS` and `ALLOCA`](#LIBOBJS), Previous: [Program and Library Variables](#Program-and-Library-Variables), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.5 Default `_SOURCES` [¶](#Default-_005fSOURCES-1)

`_SOURCES` variables are used to specify source files of programs
(see [Building a program](#A-Program)), libraries (see [Building a library](#A-Library)), and Libtool
libraries (see [Building a Shared Library](#A-Shared-Library)).

When no such variable is specified for a target, Automake will define
one itself. The default is to compile a single C file whose base name
is the name of the target itself, with any extension replaced by
`AM_DEFAULT_SOURCE_EXT`, which defaults to `.c`.

For example if you have the following somewhere in your
`Makefile.am` with no corresponding `libfoo_a_SOURCES`:

```
lib_LIBRARIES = libfoo.a sub/libc++.a
```

`libfoo.a` will be built using a default source file named
`libfoo.c`, and `sub/libc++.a` will be built from
`sub/libc++.c`. (In older versions `sub/libc++.a`
would be built from `sub_libc___a.c`, i.e., the default source
was the canonicalized name of the target, with `.c` appended.
We believe the new behavior is more sensible, but for backward
compatibility `automake` will use the old name if a file or a rule
with that name exists and `AM_DEFAULT_SOURCE_EXT` is not used.)

Default sources are mainly useful in test suites, when building many
test programs each from a single source. For instance, in

```
check_PROGRAMS = test1 test2 test3
AM_DEFAULT_SOURCE_EXT = .cpp
```

`test1`, `test2`, and `test3` will be built
from `test1.cpp`, `test2.cpp`, and `test3.cpp`.
Without the last line, they will be built from `test1.c`,
`test2.c`, and `test3.c`.

Another case where this is convenient is building many Libtool modules
(`modulen.la`), each defined in its own file
(`modulen.c`).

```
AM_LDFLAGS = -module
lib_LTLIBRARIES = module1.la module2.la module3.la
```

Finally, there is one situation where this default source computation
needs to be avoided: when a target should not be built from sources.
We already saw such an example in [Building true and false](#true); this happens when all
the constituents of a target have already been compiled and just need
to be combined using a `_LDADD` variable. Then it is necessary
to define an empty `_SOURCES` variable, so that `automake`
does not compute a default.

```
bin_PROGRAMS = target
target_SOURCES =
target_LDADD = libmain.a libmisc.a
```

---

Next: [Variables used when building a program](#Program-Variables), Previous: [Default `_SOURCES`](#Default-_005fSOURCES), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.6 Special handling for `LIBOBJS` and `ALLOCA` [¶](#Special-handling-for-LIBOBJS-and-ALLOCA)

The ‘`$(LIBOBJS)`’ and ‘`$(ALLOCA)`’ variables list object
files that should be compiled into the project to provide an
implementation for functions that are missing or broken on the host
system. They are substituted by `configure`.

These variables are defined by Autoconf macros such as
`AC_LIBOBJ`, `AC_REPLACE_FUNCS` (see [Generic Function Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Generic-Functions) in The Autoconf Manual), or
`AC_FUNC_ALLOCA` (see [Particular
Function Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Functions) in The Autoconf Manual). Many other Autoconf
macros call `AC_LIBOBJ` or `AC_REPLACE_FUNCS` to
populate ‘`$(LIBOBJS)`’.

Using these variables is very similar to doing conditional compilation
using `AC_SUBST` variables, as described in [Conditional compilation of sources](#Conditional-Sources). That is, when building a program, ‘`$(LIBOBJS)`’ and
‘`$(ALLOCA)`’ should be added to the associated ‘`*_LDADD`’
variable, or to the ‘`*_LIBADD`’ variable when building a library.
However there is no need to list the corresponding sources in
‘`EXTRA_*_SOURCES`’ nor to define ‘`*_DEPENDENCIES`’. Automake
automatically adds ‘`$(LIBOBJS)`’ and ‘`$(ALLOCA)`’ to the
dependencies, and it will discover the list of corresponding source
files automatically (by tracing the invocations of the
`AC_LIBSOURCE` Autoconf macros). If you have already defined
‘`*_DEPENDENCIES`’ explicitly for an unrelated reason, then you
either need to add these variables manually, or use
‘`EXTRA_*_DEPENDENCIES`’ instead of ‘`*_DEPENDENCIES`’.

These variables are usually used to build a portability library that
is linked with all the programs of the project. We now review a
sample setup. First, `configure.ac` contains some checks that
affect either `LIBOBJS` or `ALLOCA`.

```
# configure.ac
...
AC_CONFIG_LIBOBJ_DIR([lib])
...
AC_FUNC_MALLOC             dnl May add malloc.$(OBJEXT) to LIBOBJS
AC_FUNC_MEMCMP             dnl May add memcmp.$(OBJEXT) to LIBOBJS
AC_REPLACE_FUNCS([strdup]) dnl May add strdup.$(OBJEXT) to LIBOBJS
AC_FUNC_ALLOCA             dnl May add alloca.$(OBJEXT) to ALLOCA
...
AC_CONFIG_FILES([
  lib/Makefile
  src/Makefile
])
AC_OUTPUT
```

The `AC_CONFIG_LIBOBJ_DIR` tells Autoconf that the source files
of these object files are to be found in the `lib/` directory.
Automake can also use this information, otherwise it expects the
source files are to be in the directory where the ‘`$(LIBOBJS)`’
and ‘`$(ALLOCA)`’ variables are used.

The `lib/` directory should therefore contain `malloc.c`,
`memcmp.c`, `strdup.c`, `alloca.c`. Here is its
`Makefile.am`:

```
# lib/Makefile.am

noinst_LIBRARIES = libcompat.a
libcompat_a_SOURCES =
libcompat_a_LIBADD = $(LIBOBJS) $(ALLOCA)
```

The library can have any name, of course, and anyway it is not going
to be installed: it just holds the replacement versions of the missing
or broken functions so we can later link them in. Many projects
also include extra functions, specific to the project, in that
library: they are simply added on the `_SOURCES` line.

There is a small trap here, though: ‘`$(LIBOBJS)`’ and
‘`$(ALLOCA)`’ might be empty, and building an empty library is not
portable. You should ensure that there is always something to put in
`libcompat.a`. Most projects will also add some utility
functions in that directory, and list them in
`libcompat_a_SOURCES`, so in practice `libcompat.a` cannot
be empty.

Finally here is how this library could be used from the `src/`
directory.

```
# src/Makefile.am

# Link all programs in this directory with libcompat.a
LDADD = ../lib/libcompat.a

bin_PROGRAMS = tool1 tool2 ...
tool1_SOURCES = ...
tool2_SOURCES = ...
```

When option `subdir-objects` is not used, as in the above
example, the variables ‘`$(LIBOBJS)`’ or ‘`$(ALLOCA)`’ can only
be used in the directory where their sources lie. E.g., here it would
be wrong to use ‘`$(LIBOBJS)`’ or ‘`$(ALLOCA)`’ in
`src/Makefile.am`. However if both `subdir-objects` and
`AC_CONFIG_LIBOBJ_DIR` are used, it is OK to use these variables
in other directories. For instance `src/Makefile.am` could be
changed as follows.

```
# src/Makefile.am

AUTOMAKE_OPTIONS = subdir-objects
LDADD = $(LIBOBJS) $(ALLOCA)

bin_PROGRAMS = tool1 tool2 ...
tool1_SOURCES = ...
tool2_SOURCES = ...
```

Because ‘`$(LIBOBJS)`’ and ‘`$(ALLOCA)`’ contain object
file names that end with ‘`.$(OBJEXT)`’, they are not suitable for
Libtool libraries (where the expected object extension is `.lo`):
`LTLIBOBJS` and `LTALLOCA` should be used instead.

`LTLIBOBJS` is defined automatically by Autoconf and should not
be defined by hand (as in the past), however at the time of writing
`LTALLOCA` still needs to be defined from `ALLOCA` manually.
See [`AC_LIBOBJ` vs. `LIBOBJS`](https://www.gnu.org/software/autoconf/manual/autoconf.html#AC_005fLIBOBJ-vs-LIBOBJS) in The Autoconf Manual.

---

Next: [Yacc and Lex support](#Yacc-and-Lex), Previous: [Special handling for `LIBOBJS` and `ALLOCA`](#LIBOBJS), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.7 Variables used when building a program [¶](#Variables-used-when-building-a-program)

Occasionally it is useful to know which `Makefile` variables
Automake uses for compilations, and in which order (see [Flag Variables Ordering](#Flag-Variables-Ordering)); for instance, you might need to do your own
compilation in some special cases.

Some variables are inherited from Autoconf; these are `CC`,
`CFLAGS`, `CPPFLAGS`, `DEFS`, `LDFLAGS`, and
`LIBS`.

There are some additional variables that Automake defines on its own:

`AM_CPPFLAGS` [¶](#index-AM_005fCPPFLAGS)
:   The contents of this variable are passed to every compilation that invokes
    the C preprocessor; it is a list of arguments to the preprocessor. For
    instance, `-I` and `-D` options should be listed here.

    Automake already provides some `-I` options automatically, in a
    separate variable that is also passed to every compilation that invokes
    the C preprocessor. In particular it generates ‘`-I.`’,
    ‘`-I$(srcdir)`’, and a `-I` pointing to the directory holding
    `config.h` (if you’ve used `AC_CONFIG_HEADERS`). You can
    disable the default `-I` options using the `nostdinc`
    option.

    When a file to be included is generated during the build and not part
    of a distribution tarball, its location is under `$(builddir)`,
    not under `$(srcdir)`. This matters especially for packages that
    use header files placed in sub-directories and want to allow builds
    outside the source tree (see [Parallel Build Trees (a.k.a. VPATH Builds)](#VPATH-Builds)). In that case we
    recommend using a pair of `-I` options, such as, e.g.,
    ‘`-Isome/subdir -I$(srcdir)/some/subdir`’ or
    ‘`-I$(top_builddir)/some/subdir -I$(top_srcdir)/some/subdir`’.
    Note that the reference to the build tree should come before the
    reference to the source tree, so that accidentally leftover generated
    files in the source directory are ignored.

    `AM_CPPFLAGS` is ignored in preference to a per-executable (or
    per-library) `_CPPFLAGS` variable if it is defined.

`INCLUDES` [¶](#index-INCLUDES)
:   This does the same job as `AM_CPPFLAGS` (or any per-target
    `_CPPFLAGS` variable if it is used). It is an older name for the
    same functionality. This variable is deprecated; we suggest using
    `AM_CPPFLAGS` and per-target `_CPPFLAGS` instead.

`AM_CFLAGS` [¶](#index-AM_005fCFLAGS)
:   This is the variable the `Makefile.am` author can use to pass
    in additional C compiler flags. In some situations, this is
    not used, in preference to the per-executable (or per-library)
    `_CFLAGS`.

`COMPILE` [¶](#index-COMPILE)
:   This is the command used to compile a C source file. The file name is
    appended to form the complete command line.

`AM_LDFLAGS` [¶](#index-AM_005fLDFLAGS-1)
:   This is the variable the `Makefile.am` author can use to pass
    in additional linker flags. In some situations, this is not used, in
    preference to the per-executable (or per-library) `_LDFLAGS`.

`LINK` [¶](#index-LINK)
:   This is the command used to link a C program. It already includes
    ‘`-o $@`’ and the usual variable references (for instance,
    `CFLAGS`); it takes as “arguments” the names of the object
    files and libraries to link in. This variable is not used when the
    linker is overridden with a per-target `_LINK` variable or
    per-target flags cause Automake to define such a `_LINK`
    variable.

---

Next: [C++ Support](#C_002b_002b-Support), Previous: [Variables used when building a program](#Program-Variables), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.8 Yacc and Lex support [¶](#Yacc-and-Lex-support)

Automake has somewhat idiosyncratic support for Yacc and Lex.

Automake assumes that the `.c` file generated by `yacc`
or `lex` should be named using the basename of the input
file. That is, for a Yacc source file `foo.y`, Automake will
cause the intermediate file to be named `foo.c` (as opposed to
`y.tab.c`, which is more traditional).

The extension of a Yacc source file is used to determine the extension
of the resulting C or C++ source and header files. Be aware that header
files are generated only when the option `-d` is given to Yacc; see
below for more information about this flag, and how to specify it.
Files with the extension `.y` will thus be turned into `.c`
sources and `.h` headers; likewise, `.yy` will become
`.cc` and `.hh`, `.y++` will become `c++` and
`h++`, `.yxx` will become `.cxx` and `.hxx`,
and `.ypp` will become `.cpp` and `.hpp`.

Similarly, Lex source files can be used to generate C or C++; the
extensions `.l`, `.ll`, `.l++`, `.lxx`, and
`.lpp` are recognized.

You should never explicitly mention the intermediate (C or C++) file
in any `SOURCES` variable (except `BUILT_SOURCES`, see below);
only list the source file.

The intermediate files generated by `yacc` (or `lex`)
will be included in any distribution that is made. That way the user
doesn’t need to have `yacc` or `lex`.

If a Yacc source file is seen, then your `configure.ac` must
define the variable `YACC`. This is most easily done by invoking
the macro `AC_PROG_YACC` (see [Particular Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual).

When `yacc` is invoked, it is passed `AM_YFLAGS` and
`YFLAGS`. The latter is a user variable and the former is
intended for the `Makefile.am` author.

`AM_YFLAGS` is usually used to pass the `-d` option to
`yacc`. Automake knows what this means and will automatically
adjust its rules to update and distribute the header file built by
‘`yacc -d`’. Caveat: `automake` recognizes `-d` in
`AM_YFLAGS` only if it is not clustered with other options; for
example, it won’t be recognized if `AM_YFLAGS` is `-dt`,
but it will be if `AM_YFLAGS` is `-d -t` or `-t
-d`.

What Automake cannot guess, though, is where this header will be used:
it is up to you to ensure the header gets built before it is first
used. Typically this is necessary in order for dependency tracking to
work when the header is included by another file. The common solution
is listing the header file, and the corresponding C file, in
`BUILT_SOURCES` (see [Built Sources](#Sources)) as follows.

```
BUILT_SOURCES = parser.h parser.c
AM_YFLAGS = -d
bin_PROGRAMS = foo
foo_SOURCES = ... parser.y ...
```

If a Lex source file is seen, then your `configure.ac` must
define the variable `LEX`. You can use `AC_PROG_LEX` to do
this (see [Particular Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual), but using the `AM_PROG_LEX`
macro (see [Autoconf macros supplied with Automake](#Macros)) is recommended.

When `lex` is invoked, it is passed `AM_LFLAGS` and
`LFLAGS`. The latter is a user variable and the former is
intended for the `Makefile.am` author.

When `AM_MAINTAINER_MODE` (see [`missing` and `AM_MAINTAINER_MODE`](#maintainer_002dmode)) is in effect,
the rebuild rules for distributed Yacc and Lex sources are only used
when `maintainer-mode` is enabled, or when the files have been
erased.

When Yacc or Lex sources are used, `automake -a` automatically
installs an auxiliary program called `ylwrap` in your package
(see [Programs automake might require](#Auxiliary-Programs)). This program is used by the build rules
to rename the output of these tools, and makes it possible to include
multiple `yacc` (or `lex`) source files in a single
directory. This is necessary because Yacc’s output file name is
fixed, and a parallel make could invoke more than one instance of
`yacc` simultaneously.

* [Linking Multiple Yacc Parsers](#Linking-Multiple-Yacc-Parsers)

---

Up: [Yacc and Lex support](#Yacc-and-Lex)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.8.1 Linking Multiple Yacc Parsers [¶](#Linking-Multiple-Yacc-Parsers-1)

For `yacc`, simply managing locking as with `ylwrap` is
insufficient. The output of `yacc` always uses the same
symbol names internally, so it isn’t possible to link two
`yacc` parsers into the same executable.

We recommend using the following renaming hack used in `gdb`:

```
#define yymaxdepth c_maxdepth
#define yyparse c_parse
#define yylex   c_lex
#define yyerror c_error
#define yylval  c_lval
#define yychar  c_char
#define yydebug c_debug
#define yypact  c_pact
#define yyr1    c_r1
#define yyr2    c_r2
#define yydef   c_def
#define yychk   c_chk
#define yypgo   c_pgo
#define yyact   c_act
#define yyexca  c_exca
#define yyerrflag c_errflag
#define yynerrs c_nerrs
#define yyps    c_ps
#define yypv    c_pv
#define yys     c_s
#define yy_yys  c_yys
#define yystate c_state
#define yytmp   c_tmp
#define yyv     c_v
#define yy_yyv  c_yyv
#define yyval   c_val
#define yylloc  c_lloc
#define yyreds  c_reds
#define yytoks  c_toks
#define yylhs   c_yylhs
#define yylen   c_yylen
#define yydefred c_yydefred
#define yydgoto  c_yydgoto
#define yysindex c_yysindex
#define yyrindex c_yyrindex
#define yygindex c_yygindex
#define yytable  c_yytable
#define yycheck  c_yycheck
#define yyname   c_yyname
#define yyrule   c_yyrule
```

For each define, replace the ‘`c_`’ prefix with whatever you like.
These defines work for `bison`, `byacc`, and
traditional `yacc`s. If you find a parser generator that uses a
symbol not covered here, please report the new name so it can be added
to the list.

---

Next: [Objective C Support](#Objective-C-Support), Previous: [Yacc and Lex support](#Yacc-and-Lex), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.9 C++ Support [¶](#C_002b_002b-Support-1)

Automake includes full support for C++.

Any package including C++ code must define the output variable
`CXX` in `configure.ac`; the simplest way to do this is to use
the `AC_PROG_CXX` macro (see [Particular
Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual).

A few additional variables are defined when a C++ source file is seen:

`CXX` [¶](#index-CXX)
:   The name of the C++ compiler.

`CXXFLAGS` [¶](#index-CXXFLAGS)
:   Any flags to pass to the C++ compiler.

`AM_CXXFLAGS` [¶](#index-AM_005fCXXFLAGS)
:   The maintainer’s variant of `CXXFLAGS`.

`CXXCOMPILE` [¶](#index-CXXCOMPILE)
:   The command used to compile a C++ source file. The file name is
    appended to form the complete command line.

`CXXLINK` [¶](#index-CXXLINK)
:   The command used to link a C++ program.

---

Next: [Objective C++ Support](#Objective-C_002b_002b-Support), Previous: [C++ Support](#C_002b_002b-Support), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.10 Objective C Support [¶](#Objective-C-Support-1)

Automake includes some support for Objective C.

Any package including Objective C code must define the output variable
`OBJC` in `configure.ac`; the simplest way to do this is to use
the `AC_PROG_OBJC` macro (see [Particular
Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual).

A few additional variables are defined when an Objective C source file
is seen:

`OBJC` [¶](#index-OBJC)
:   The name of the Objective C compiler.

`OBJCFLAGS` [¶](#index-OBJCFLAGS)
:   Any flags to pass to the Objective C compiler.

`AM_OBJCFLAGS` [¶](#index-AM_005fOBJCFLAGS)
:   The maintainer’s variant of `OBJCFLAGS`.

`OBJCCOMPILE` [¶](#index-OBJCCOMPILE)
:   The command used to compile an Objective C source file. The file name
    is appended to form the complete command line.

`OBJCLINK` [¶](#index-OBJCLINK)
:   The command used to link an Objective C program.

---

Next: [Unified Parallel C Support](#Unified-Parallel-C-Support), Previous: [Objective C Support](#Objective-C-Support), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.11 Objective C++ Support [¶](#Objective-C_002b_002b-Support-1)

Automake includes some support for Objective C++.

Any package including Objective C++ code must define the output variable
`OBJCXX` in `configure.ac`; the simplest way to do this is to use
the `AC_PROG_OBJCXX` macro (see [Particular
Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual).

A few additional variables are defined when an Objective C++ source file
is seen:

`OBJCXX` [¶](#index-OBJCXX)
:   The name of the Objective C++ compiler.

`OBJCXXFLAGS` [¶](#index-OBJCXXFLAGS)
:   Any flags to pass to the Objective C++ compiler.

`AM_OBJCXXFLAGS` [¶](#index-AM_005fOBJCXXFLAGS)
:   The maintainer’s variant of `OBJCXXFLAGS`.

`OBJCXXCOMPILE` [¶](#index-OBJCXXCOMPILE)
:   The command used to compile an Objective C++ source file. The file
    name is appended to form the complete command line.

`OBJCXXLINK` [¶](#index-OBJCXXLINK)
:   The command used to link an Objective C++ program.

---

Next: [Assembly Support](#Assembly-Support), Previous: [Objective C++ Support](#Objective-C_002b_002b-Support), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.12 Unified Parallel C Support [¶](#Unified-Parallel-C-Support-1)

Automake includes some support for Unified Parallel C.

Any package including Unified Parallel C code must define the output
variable `UPC` in `configure.ac`; the simplest way to do
this is to use the `AM_PROG_UPC` macro (see [Public Macros](#Public-Macros)).

A few additional variables are defined when a Unified Parallel C
source file is seen:

`UPC` [¶](#index-UPC-1)
:   The name of the Unified Parallel C compiler.

`UPCFLAGS` [¶](#index-UPCFLAGS)
:   Any flags to pass to the Unified Parallel C compiler.

`AM_UPCFLAGS` [¶](#index-AM_005fUPCFLAGS)
:   The maintainer’s variant of `UPCFLAGS`.

`UPCCOMPILE` [¶](#index-UPCCOMPILE)
:   The command used to compile a Unified Parallel C source file. The
    file name is appended to form the complete command line.

`UPCLINK` [¶](#index-UPCLINK)
:   The command used to link a Unified Parallel C program.

---

Next: [Fortran 77 Support](#Fortran-77-Support), Previous: [Unified Parallel C Support](#Unified-Parallel-C-Support), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.13 Assembly Support [¶](#Assembly-Support-1)

Automake includes some support for assembly code. There are two forms
of assembler files: normal (`*.s`) and preprocessed by `CPP`
(`*.S` or `*.sx`).

The variable `CCAS` holds the name of the compiler used to build
assembly code. This compiler must work a bit like a C compiler; in
particular it must accept `-c` and `-o`. The values of
`CCASFLAGS` and `AM_CCASFLAGS` (or its per-target
definition) is passed to the compilation. For preprocessed files,
`DEFS`, `DEFAULT_INCLUDES`, `INCLUDES`, `CPPFLAGS`
and `AM_CPPFLAGS` are also used.

The autoconf macro `AM_PROG_AS` will define `CCAS` and
`CCASFLAGS` for you (unless they are already set, it simply sets
`CCAS` to the C compiler and `CCASFLAGS` to the C compiler
flags), but you are free to define these variables by other means.

Only the suffixes `.s`, `.S`, and `.sx` are recognized by
`automake` as being files containing assembly code.

---

Next: [Fortran 9x Support](#Fortran-9x-Support), Previous: [Assembly Support](#Assembly-Support), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.14 Fortran 77 Support [¶](#Fortran-77-Support-1)

Automake includes full support for Fortran 77.

Any package including Fortran 77 code must define the output variable
`F77` in `configure.ac`; the simplest way to do this is to use
the `AC_PROG_F77` macro (see [Particular
Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual).

A few additional variables are defined when a Fortran 77 source file is
seen:

`F77` [¶](#index-F77)
:   The name of the Fortran 77 compiler.

`FFLAGS` [¶](#index-FFLAGS)
:   Any flags to pass to the Fortran 77 compiler.

`AM_FFLAGS` [¶](#index-AM_005fFFLAGS)
:   The maintainer’s variant of `FFLAGS`.

`RFLAGS` [¶](#index-RFLAGS)
:   Any flags to pass to the Ratfor compiler.

`AM_RFLAGS` [¶](#index-AM_005fRFLAGS)
:   The maintainer’s variant of `RFLAGS`.

`F77COMPILE` [¶](#index-F77COMPILE)
:   The command used to compile a Fortran 77 source file. The file name
    is appended to form the complete command line.

`FLINK` [¶](#index-FLINK)
:   The command used to link a pure Fortran 77 program or shared library.

Automake can handle preprocessing Fortran 77 and Ratfor source files in
addition to compiling them[2](#FOOT2). Automake
also contains some support for creating programs and shared libraries
that are a mixture of Fortran 77 and other languages (see [Mixing Fortran 77 With C and C++](#Mixing-Fortran-77-With-C-and-C_002b_002b)).

These issues are covered in the following sections.

* [Preprocessing Fortran 77](#Preprocessing-Fortran-77)
* [Compiling Fortran 77 Files](#Compiling-Fortran-77-Files)
* [Mixing Fortran 77 With C and C++](#Mixing-Fortran-77-With-C-and-C_002b_002b)

---

Next: [Compiling Fortran 77 Files](#Compiling-Fortran-77-Files), Up: [Fortran 77 Support](#Fortran-77-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.14.1 Preprocessing Fortran 77 [¶](#Preprocessing-Fortran-77-1)

`N.f` is made automatically from `N.F` or `N.r`. This
rule runs just the preprocessor to convert a preprocessable Fortran 77
or Ratfor source file into a strict Fortran 77 source file. The precise
command used is as follows:

`.F`
:   `$(F77) -F $(DEFS) $(INCLUDES) $(AM_CPPFLAGS) $(CPPFLAGS)  
    $(AM_FFLAGS) $(FFLAGS)`

`.r`
:   `$(F77) -F $(AM_FFLAGS) $(FFLAGS) $(AM_RFLAGS) $(RFLAGS)`

---

Next: [Mixing Fortran 77 With C and C++](#Mixing-Fortran-77-With-C-and-C_002b_002b), Previous: [Preprocessing Fortran 77](#Preprocessing-Fortran-77), Up: [Fortran 77 Support](#Fortran-77-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.14.2 Compiling Fortran 77 Files [¶](#Compiling-Fortran-77-Files-1)

`N.o` is made automatically from `N.f`, `N.F` or
`N.r` by running the Fortran 77 compiler. The precise command used
is as follows:

`.f`
:   `$(F77) -c $(AM_FFLAGS) $(FFLAGS)`

`.F`
:   `$(F77) -c $(DEFS) $(INCLUDES) $(AM_CPPFLAGS) $(CPPFLAGS)  
    $(AM_FFLAGS) $(FFLAGS)`

`.r`
:   `$(F77) -c $(AM_FFLAGS) $(FFLAGS) $(AM_RFLAGS) $(RFLAGS)`

---

Previous: [Compiling Fortran 77 Files](#Compiling-Fortran-77-Files), Up: [Fortran 77 Support](#Fortran-77-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.14.3 Mixing Fortran 77 With C and C++ [¶](#Mixing-Fortran-77-With-C-and-C_002b_002b-1)

Automake currently provides *limited* support for creating programs
and shared libraries that are a mixture of Fortran 77 and C and/or C++.
However, there are many other issues related to mixing Fortran 77 with
other languages that are *not* (currently) handled by Automake, but
that are handled by other packages[3](#FOOT3).

Automake can help in two ways:

1. Automatic selection of the linker depending on which combinations of
   source code.
2. Automatic selection of the appropriate linker flags (e.g., `-L` and
   `-l`) to pass to the automatically selected linker in order to link
   in the appropriate Fortran 77 intrinsic and run-time libraries.

   These extra Fortran 77 linker flags are supplied in the output variable
   `FLIBS` by the `AC_F77_LIBRARY_LDFLAGS` Autoconf macro.
   See [Fortran Compiler Characteristics](https://www.gnu.org/software/autoconf/manual/autoconf.html#Fortran-Compiler) in The Autoconf Manual.

If Automake detects that a program or shared library (as mentioned in
some `_PROGRAMS` or `_LTLIBRARIES` primary) contains source
code that is a mixture of Fortran 77 and C and/or C++, then it requires
that the macro `AC_F77_LIBRARY_LDFLAGS` be called in
`configure.ac`, and that either `$(FLIBS)`
appear in the appropriate `_LDADD` (for programs) or `_LIBADD`
(for shared libraries) variables. It is the responsibility of the
person writing the `Makefile.am` to make sure that ‘`$(FLIBS)`’
appears in the appropriate `_LDADD` or
`_LIBADD` variable.

For example, consider the following `Makefile.am`:

```
bin_PROGRAMS = foo
foo_SOURCES  = main.cc foo.f
foo_LDADD    = libfoo.la $(FLIBS)

pkglib_LTLIBRARIES = libfoo.la
libfoo_la_SOURCES  = bar.f baz.c zardoz.cc
libfoo_la_LIBADD   = $(FLIBS)
```

In this case, Automake will insist that `AC_F77_LIBRARY_LDFLAGS`
is mentioned in `configure.ac`. Also, if ‘`$(FLIBS)`’ hadn’t
been mentioned in `foo_LDADD` and `libfoo_la_LIBADD`, then
Automake would have issued a warning.

* [How the Linker is Chosen](#How-the-Linker-is-Chosen)

---

Up: [Mixing Fortran 77 With C and C++](#Mixing-Fortran-77-With-C-and-C_002b_002b)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.14.3.1 How the Linker is Chosen [¶](#How-the-Linker-is-Chosen-1)

When a program or library mixes several languages, Automake chooses the
linker according to the following priorities. (The names in
parentheses are the variables containing the link command.)

1. Native Java (`GCJLINK`)
2. Objective C++ (`OBJCXXLINK`)
3. C++ (`CXXLINK`)
4. Fortran 77 (`F77LINK`)
5. Fortran (`FCLINK`)
6. Objective C (`OBJCLINK`)
7. Unified Parallel C (`UPCLINK`)
8. C (`LINK`)

For example, if Fortran 77, C and C++ source code is compiled
into a program, then the C++ linker will be used. In this case, if the
C or Fortran 77 linkers required any special libraries that weren’t
included by the C++ linker, then they must be manually added to an
`_LDADD` or `_LIBADD` variable by the user writing the
`Makefile.am`.

Automake only looks at the file names listed in `_SOURCES`
variables to choose the linker, and defaults to the C linker.
Sometimes this is inconvenient because you are linking against a
library written in another language and would like to set the linker
more appropriately. See [Libtool Convenience Libraries](#Libtool-Convenience-Libraries), for a
trick with `nodist_EXTRA_…_SOURCES`.

A per-target `_LINK` variable will override the above selection.
Per-target link flags will cause Automake to write a per-target
`_LINK` variable according to the language chosen as above.

---

Next: [Compiling Java sources using gcj](#Java-Support-with-gcj), Previous: [Fortran 77 Support](#Fortran-77-Support), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.15 Fortran 9x Support [¶](#Fortran-9x-Support-1)

Automake includes support for Fortran 9x.

Any package including Fortran 9x code must define the output variable
`FC` in `configure.ac`; the simplest way to do this is to use
the `AC_PROG_FC` macro (see [Particular
Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual).

A few additional variables are defined when a Fortran 9x source file is
seen:

`FC` [¶](#index-FC)
:   The name of the Fortran 9x compiler.

`FCFLAGS` [¶](#index-FCFLAGS)
:   Any flags to pass to the Fortran 9x compiler.

`AM_FCFLAGS` [¶](#index-AM_005fFCFLAGS)
:   The maintainer’s variant of `FCFLAGS`.

`FCCOMPILE` [¶](#index-FCCOMPILE)
:   The command used to compile a Fortran 9x source file. The file name
    is appended to form the complete command line.

`FCLINK` [¶](#index-FCLINK-1)
:   The command used to link a pure Fortran 9x program or shared library.

* [Compiling Fortran 9x Files](#Compiling-Fortran-9x-Files)

---

Up: [Fortran 9x Support](#Fortran-9x-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.15.1 Compiling Fortran 9x Files [¶](#Compiling-Fortran-9x-Files-1)

`file.o` is made automatically from `file.f90`,
`file.f95`, `file.f03`, or `file.f08`
by running the Fortran 9x compiler. The precise command used
is as follows:

`.f90`
:   `$(FC) $(AM_FCFLAGS) $(FCFLAGS) -c $(FCFLAGS_f90) $<`

`.f95`
:   `$(FC) $(AM_FCFLAGS) $(FCFLAGS) -c $(FCFLAGS_f95) $<`

`.f03`
:   `$(FC) $(AM_FCFLAGS) $(FCFLAGS) -c $(FCFLAGS_f03) $<`

`.f08`
:   `$(FC) $(AM_FCFLAGS) $(FCFLAGS) -c $(FCFLAGS_f08) $<`

---

Next: [Vala Support](#Vala-Support), Previous: [Fortran 9x Support](#Fortran-9x-Support), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.16 Compiling Java sources using gcj [¶](#Compiling-Java-sources-using-gcj)

Automake includes support for natively compiled Java, using `gcj`,
the Java front end to the GNU Compiler Collection (rudimentary support
for compiling Java to bytecode using the `javac` compiler is
also present, *albeit deprecated*; see [Java bytecode compilation (deprecated)](#Java)).

Any package including Java code to be compiled must define the output
variable `GCJ` in `configure.ac`; the variable `GCJFLAGS`
must also be defined somehow (either in `configure.ac` or
`Makefile.am`). The simplest way to do this is to use the
`AM_PROG_GCJ` macro.

By default, programs including Java source files are linked with
`gcj`.

As always, the contents of `AM_GCJFLAGS` are passed to every
compilation invoking `gcj` (in its role as an ahead-of-time
compiler, when invoking it to create `.class` files,
`AM_JAVACFLAGS` is used instead). If it is necessary to pass
options to `gcj` from `Makefile.am`, this variable, and not
the user variable `GCJFLAGS`, should be used.

`gcj` can be used to compile `.java`, `.class`,
`.zip`, or `.jar` files.

When linking, `gcj` requires that the main class be specified
using the `--main=` option. The easiest way to do this is to use
the `_LDFLAGS` variable for the program.

---

Next: [Algol 68 Support](#Algol-68-Support), Previous: [Compiling Java sources using gcj](#Java-Support-with-gcj), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.17 Vala Support [¶](#Vala-Support-1)

Automake supports Vala (<https://vala.dev/>).
Vala support requires the user to use GNU `make`.

```
foo_SOURCES = foo.vala bar.vala zardoz.c
```

Any `.vala` file listed in a `_SOURCES` variable will be
compiled into C code by the Vala compiler. The generated `.c`
files are distributed. The end user does not need to have a Vala
compiler installed.

Because all C files must be generated, and the Vala compiler compiles
all the `.vala` files for a target at once, it is not possible to
add files to a `_SOURCES` variable that cannot be compiled
together; for example, alternative platform-specific definitions of
the same methods.

Automake ships with an Autoconf macro called `AM_PROG_VALAC`
that will locate the Vala compiler and optionally check its version
number.

Macro: **AM\_PROG\_VALAC** ([minimum-version], [action-if-found], [action-if-not-found]) [¶](#index-AM_005fPROG_005fVALAC)
:   Search for a Vala compiler in `PATH`. If it is found, the variable
    `VALAC` is set to point to it (see below for more details). This
    macro takes three optional arguments. The first argument, if present,
    is the minimum version of the Vala API required to compile this package.
    For Vala releases, this is the same as the major and minor release
    number; e.g., when `valac --version` reports `0.48.7`,
    `valac --api-version` reports `0.48`. If a compiler is found
    and satisfies minimum-version, then action-if-found is run
    (this defaults to do nothing). Otherwise, action-if-not-found is
    run. If action-if-not-found is not specified, the default value
    is to print a warning in case no compiler is found, or if a too-old
    version of the compiler is found.

There are a few variables that are used when compiling Vala sources:

`VALAC` [¶](#index-VALAC)
:   Absolute path to the Vala compiler, or simply ‘`valac`’ if no
    suitable Vala compiler could be found at configure runtime.

`VALAFLAGS` [¶](#index-VALAFLAGS)
:   Additional arguments for the Vala compiler.

`AM_VALAFLAGS` [¶](#index-AM_005fVALAFLAGS)
:   The maintainer’s variant of `VALAFLAGS`.

    ```
    lib_LTLIBRARIES = libfoo.la
    libfoo_la_SOURCES = foo.vala
    ```

Note that currently, you cannot use per-target `*_VALAFLAGS`
(see [Why are object files sometimes renamed?](#Renamed-Objects)) to produce different C files from one Vala
source file.

---

Next: [Support for Other Languages](#Support-for-Other-Languages), Previous: [Vala Support](#Vala-Support), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.18 Algol 68 Support [¶](#Algol-68-Support-1)

Automake includes support for Algol 68, using GCC.

Any package including Algol 68 code must define the output variable
`A68` in `configure.ac`; the simplest way to do this is to use
the `AC_PROG_A68` macro (see [Particular
Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual).

A few additional variables are defined when an Algol 68 source file is
seen:

`A68` [¶](#index-A68)
:   The name of the Algol 68 compiler.

`A68FLAGS` [¶](#index-A68FLAGS)
:   Any flags to pass to the Algol 68 compiler.

`AM_A68FLAGS` [¶](#index-AM_005fA68FLAGS)
:   The maintainer’s variant of `A68FLAGS`.

`A68COMPILE` [¶](#index-A68COMPILE)
:   The command used to compile an Algol 68 source file. The file name is
    appended to form the complete command line.

`A68LINK` [¶](#index-A68LINK)
:   The command used to link a pure Algol 68 program or shared library.

---

Next: [Automatic dependency tracking](#Dependencies), Previous: [Algol 68 Support](#Algol-68-Support), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.19 Support for Other Languages [¶](#Support-for-Other-Languages-1)

Automake currently only includes full support for C, C++ (see [C++ Support](#C_002b_002b-Support)), Objective C (see [Objective C Support](#Objective-C-Support)),
Objective C++ (see [Objective C++ Support](#Objective-C_002b_002b-Support)),
Fortran 77
(see [Fortran 77 Support](#Fortran-77-Support)), Fortran 9x (see [Fortran 9x Support](#Fortran-9x-Support)),
and Java (see [Compiling Java sources using gcj](#Java-Support-with-gcj)). There is only rudimentary
support for other languages, support for which will be improved based
on user demand.

Some limited support for adding your own languages is available via the
suffix rule handling (see [Handling new file extensions](#Suffixes)).

---

Next: [Support for executable extensions](#EXEEXT), Previous: [Support for Other Languages](#Support-for-Other-Languages), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.20 Automatic dependency tracking [¶](#Automatic-dependency-tracking)

As a developer it is painful to continually update the
`Makefile.am` whenever the include-file dependencies change in a
project. Automake supplies a way to automatically track dependency
changes (see [Automatic Dependency Tracking](#Dependency-Tracking)).

Automake always uses complete dependencies for a compilation,
including system headers. Automake’s model is that dependency
computation should be a side effect of the build. To this end,
dependencies are computed by running all compilations through a
special wrapper program called `depcomp`. `depcomp`
understands how to coax many different C and C++ compilers into
generating dependency information in the format it requires.
‘`automake -a`’ will install `depcomp` into your source
tree for you. If `depcomp` can’t figure out how to properly
invoke your compiler, dependency tracking will simply be disabled for
your build.

Experience with earlier versions of Automake (see [Dependency Tracking Evolution](https://www.gnu.org/software/automake/history/automake-history.html#Dependency-Tracking-Evolution) in Brief History
of Automake) taught us that it is not reliable to generate dependencies
only on the maintainer’s system, as configurations vary too much. So
instead Automake implements dependency tracking at build time.

This automatic dependency tracking can be suppressed by putting
`no-dependencies` in the variable `AUTOMAKE_OPTIONS`, or
passing `no-dependencies` as an argument to `AM_INIT_AUTOMAKE`
(this should be the preferred way). Or, you can invoke `automake`
with the `-i` option. Dependency tracking is enabled by default.

The person building your package also can choose to disable dependency
tracking by configuring with `--disable-dependency-tracking`.

If, as the package maintainer, you wish to conditionalize your
`Makefile.am` according to whether dependency tracking is
enabled, the best way is to define your own conditional in
`configure.ac` according to the shell variable
`$enable_dependency_tracking` (all
`--enable`/`--disable` options are available as shell
variables; see [Package Options](https://www.gnu.org/software/autoconf/manual/autoconf.html#Package-Options) in The Autoconf Manual):

```
AM_CONDITIONAL([NO_DEP_TRACKING],
               [test x"$enable_dependency_tracking" = x"no"])
```

And then in your `Makefile.am`:

```
if NO_DEP_TRACKING
# stuff to do when dependency tracking is disabled
else
# stuff to do when it's enabled
endif
```

Automake does not have support for extending the dependency tracking
at runtime, e.g., if you want to have dependencies for a new language.
However, Nick Bowler sketched how to do this in a message to the
`automake` mailing list:
<https://lists.gnu.org/archive/html/automake/2025-07/msg00014.html>.
Although this necessarily uses various implementation details of
Automake, and thus can’t be considered future-proof, we don’t have any
plans or expectations for changing the dependency implementation in
any substantive way.

---

Previous: [Automatic dependency tracking](#Dependencies), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.21 Support for executable extensions [¶](#Support-for-executable-extensions)

On some platforms, such as Windows, executables are expected to have an
extension such as `.exe`. On these platforms, some compilers (GCC
among them) will automatically generate `foo.exe` when asked to
generate `foo`.

Automake provides mostly-transparent support for this. Unfortunately
*mostly* doesn’t yet mean *fully*. Until the English
dictionary is revised, you will have to assist Automake if your package
must support those platforms.

One thing you must be aware of is that, internally, Automake rewrites
something like this:

```
bin_PROGRAMS = liver
```

to this:

```
bin_PROGRAMS = liver$(EXEEXT)
```

The targets Automake generates are likewise given the ‘`$(EXEEXT)`’
extension.

The variables `TESTS` and `XFAIL_TESTS` (see [Simple Tests](#Simple-Tests))
are also rewritten if they contain filenames that have been declared as
programs in the same `Makefile`. (This is mostly useful when some
programs from `check_PROGRAMS` are listed in `TESTS`.)

However, Automake cannot apply this rewriting to `configure`
substitutions. This means that if you are conditionally building a
program using such a substitution, then your `configure.ac` must
take care to add ‘`$(EXEEXT)`’ when constructing the output variable.

Sometimes maintainers like to write an explicit link rule for their
program. Without executable extension support, you can simply write a
rule whose target is the name of the program. However, when
executable extension support is enabled, you must instead add the
‘`$(EXEEXT)`’ suffix.

This might be a nuisance for maintainers who know their package will
never run on a platform that has
executable extensions. For those maintainers, the `no-exeext`
option (see [Changing Automake’s Behavior](#Options)) will disable this feature. This works in a
fairly ugly way; if `no-exeext` is seen, then the presence of a
rule for a target named `foo` in `Makefile.am` will override
an `automake`-generated rule for ‘`foo$(EXEEXT)`’. Without
the `no-exeext` option, this use will give a diagnostic.

---

Next: [Other GNU Tools](#Other-GNU-Tools), Previous: [Building Programs and Libraries](#Programs), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 9 Other Derived Objects [¶](#Other-Derived-Objects)

Automake can handle derived objects that are not C programs.
Sometimes the support for building such objects must be explicitly
supplied, but Automake can still automatically handle installation and
distribution.

* [Executable Scripts](#Scripts)
* [Header files](#Headers)
* [Architecture-independent data files](#Data)
* [Built Sources](#Sources)

---

Next: [Header files](#Headers), Up: [Other Derived Objects](#Other-Objects)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 9.1 Executable Scripts [¶](#Executable-Scripts)

It is possible to define and install programs that are scripts. Such
programs are listed using the `SCRIPTS` primary name. When the
script is distributed in its final, installable form, the
`Makefile` usually looks as follows:

```
# Install my_script in $(bindir) and distribute it.
dist_bin_SCRIPTS = my_script
```

Scripts are not distributed by default; as we have just seen, those
that should be distributed can be specified using a `dist_`
prefix as with other primaries.

Scripts can be installed in `bindir`, `sbindir`,
`libexecdir`, `pkglibexecdir`, or `pkgdatadir`.

Scripts that need not be installed can be listed in
`noinst_SCRIPTS`, and among them, those which are needed only by
‘`make check`’ should go in `check_SCRIPTS`.

When a script needs to be built, the `Makefile.am` should include
the appropriate rules. For instance the `automake` program
itself is a Perl script that is generated from `automake.in`.
Here is how this is handled:

```
bin_SCRIPTS = automake
CLEANFILES = $(bin_SCRIPTS)
EXTRA_DIST = automake.in

do_subst = sed -e 's,[@]datadir[@],$(datadir),g' \
            -e 's,[@]PERL[@],$(PERL),g' \
            -e 's,[@]PACKAGE[@],$(PACKAGE),g' \
            -e 's,[@]VERSION[@],$(VERSION),g' \
            ...

automake: automake.in Makefile
        $(do_subst) < $(srcdir)/automake.in > automake
        chmod +x automake
```

Such scripts for which a build rule has been supplied need to be
deleted explicitly using `CLEANFILES` (see [What Gets Cleaned](#Clean)), and their
sources have to be distributed, usually with `EXTRA_DIST`
(see [Basics of Distribution](#Basics-of-Distribution)).

Another common way to build scripts is to process them from
`configure` with `AC_CONFIG_FILES`. In this situation
Automake knows which files should be cleaned and distributed, and what
the rebuild rules should look like.

For instance if `configure.ac` contains

```
AC_CONFIG_FILES([src/my_script], [chmod +x src/my_script])
```

to build `src/my_script` from `src/my_script.in`, then a
`src/Makefile.am` to install this script in `$(bindir)` can
be as simple as

```
bin_SCRIPTS = my_script
CLEANFILES = $(bin_SCRIPTS)
```

There is no need for `EXTRA_DIST` or any build rule: Automake
infers them from `AC_CONFIG_FILES` (see [Configuration requirements](#Requirements)).
`CLEANFILES` is still useful, because by default Automake will
clean targets of `AC_CONFIG_FILES` in `distclean`, not
`clean`.

Although this looks simpler, building scripts this way has one
drawback: directory variables such as `$(datadir)` are not fully
expanded and may refer to other directory variables.

---

Next: [Architecture-independent data files](#Data), Previous: [Executable Scripts](#Scripts), Up: [Other Derived Objects](#Other-Objects)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 9.2 Header files [¶](#Header-files)

Header files that must be installed are specified by the
`HEADERS` family of variables. Headers can be installed in
`includedir`, `oldincludedir`, `pkgincludedir` or any
other directory you may have defined (see [The Uniform Naming Scheme](#Uniform)). For instance,

```
include_HEADERS = foo.h bar/bar.h
```

will install the two files as `$(includedir)/foo.h` and
`$(includedir)/bar.h`.

The `nobase_` prefix is also supported:

```
nobase_include_HEADERS = foo.h bar/bar.h
```

will install the two files as `$(includedir)/foo.h` and
`$(includedir)/bar/bar.h` (see [An Alternative Approach to Subdirectories](#Alternative)).

Usually, only header files that accompany installed libraries need to
be installed. Headers used by programs or convenience libraries are
not installed. The `noinst_HEADERS` variable can be used for
such headers. However, when the header belongs to a single
convenience library or program, we recommend listing it in the
program’s or library’s `_SOURCES` variable (see [Defining program sources](#Program-Sources)) instead of in `noinst_HEADERS`. This is clearer for
the `Makefile.am` reader. `noinst_HEADERS` would be the
right variable to use in a directory containing only headers and no
associated library or program.

All header files must be listed somewhere; in a `_SOURCES`
variable or in a `_HEADERS` variable. Missing ones will not
appear in the distribution.

For header files that are built and must not be distributed, use the
`nodist_` prefix as in `nodist_include_HEADERS` or
`nodist_prog_SOURCES`. If these generated headers are needed
during the build, you must also ensure they exist before they are
used (see [Built Sources](#Sources)).

---

Next: [Built Sources](#Sources), Previous: [Header files](#Headers), Up: [Other Derived Objects](#Other-Objects)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 9.3 Architecture-independent data files [¶](#Architecture_002dindependent-data-files)

Automake supports the installation of miscellaneous data files using the
`DATA` family of variables.

Such data can be installed in the directories `datadir`,
`docdir`, `lispdir`, `sysconfdir`, `sharedstatedir`,
`localstatedir`, or `pkgdatadir`.

By default, data files are *not* included in a distribution. Of
course, you can use the `dist_` prefix to change this on a
per-variable basis.

Here is how Automake declares its auxiliary data files:

```
dist_pkgdata_DATA = clean-kr.am clean.am ...
```

---

Previous: [Architecture-independent data files](#Data), Up: [Other Derived Objects](#Other-Objects)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 9.4 Built Sources [¶](#Built-Sources)

Because Automake’s automatic dependency tracking works as a side-effect
of compilation (see [Automatic dependency tracking](#Dependencies)) there is a bootstrap issue: a
target should not be compiled before its dependencies are made, but
these dependencies are unknown until the target is first compiled.

Ordinarily this is not a problem, because dependencies are distributed
sources: they preexist and do not need to be built. Suppose that
`foo.c` includes `foo.h`. When it first compiles
`foo.o`, `make` only knows that `foo.o` depends on
`foo.c`. As a side-effect of this compilation `depcomp`
records the `foo.h` dependency so that following invocations of
`make` will honor it. In these conditions, it’s clear there is
no problem: either `foo.o` doesn’t exist and has to be built
(regardless of the dependencies), or accurate dependencies exist and
they can be used to decide whether `foo.o` should be rebuilt.

It’s a different story if `foo.h` doesn’t exist by the first
`make` run. For instance, there might be a rule to build
`foo.h`. This time `file.o`’s build will fail because the
compiler can’t find `foo.h`. `make` failed to trigger the
rule to build `foo.h` first by lack of dependency information.

The `BUILT_SOURCES` variable is a workaround for this problem. A
source file listed in `BUILT_SOURCES` is made when ‘`make
all`’, ‘`make check`’, ‘`make install`’, ‘`make install-exec`’
(or `make dist`) is run, before other targets are processed.
However, such a source file is not *compiled* unless explicitly
requested by mentioning it in some other `_SOURCES` variable.

So, to conclude our introductory example, we could use
‘`BUILT_SOURCES = foo.h`’ to ensure `foo.h` gets built before
any other target (including `foo.o`) during ‘`make all`’ or
‘`make check`’.

`BUILT_SOURCES` is a bit of a misnomer, as any file which must be
created early in the build process can be listed in this variable.
Moreover, all built sources do not necessarily have to be listed in
`BUILT_SOURCES`. For instance, a generated `.c` file
doesn’t need to appear in `BUILT_SOURCES` (unless it is included
by another source), because it’s a known dependency of the associated
object.

To emphasize, `BUILT_SOURCES` is honored only by ‘`make all`’,
‘`make check`’, ‘`make install`’, and `make install-exec`
(and ‘`make dist`’). This means you cannot build an arbitrary
target (e.g., ‘`make foo`’) in a clean tree if it depends on a
built source (unless you explicitly create a dependency for `foo`
on `$(BUILT_SOURCES)`). It will succeed if you have run
‘`make all`’ earlier, though, because accurate dependencies are
already available.

The next section illustrates and discusses the handling of built sources
on a toy example.

* [Built Sources Example](#Built-Sources-Example)

---

Up: [Built Sources](#Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 9.4.1 Built Sources Example [¶](#Built-Sources-Example-1)

Suppose that `foo.c` includes `bindir.h`, which is
installation-dependent and not distributed: it needs to be built. Here
`bindir.h` defines the preprocessor macro `bindir` to the
value of the `make` variable `bindir` (inherited from
`configure`).

We suggest several implementations below. It’s not meant to be an
exhaustive listing of all ways to handle built sources, but to give
you a few ideas if you encounter this issue.

#### First Try [¶](#First-Try)

This first implementation will illustrate the bootstrap issue mentioned
in the previous section (see [Built Sources](#Sources)).

Here is a tentative `Makefile.am`.

```
# This won't work.
bin_PROGRAMS = foo
foo_SOURCES = foo.c
nodist_foo_SOURCES = bindir.h
CLEANFILES = bindir.h
bindir.h: Makefile
        echo '#define bindir "$(bindir)"' >$@
```

This setup doesn’t work, because Automake doesn’t know that `foo.c`
includes `bindir.h`. Remember, automatic dependency tracking works
as a side-effect of compilation, so the dependencies of `foo.o` will
be known only after `foo.o` has been compiled (see [Automatic dependency tracking](#Dependencies)).
The symptom is as follows.

```
% make
source='foo.c' object='foo.o' libtool=no \
depfile='.deps/foo.Po' tmpdepfile='.deps/foo.TPo' \
depmode=gcc /bin/sh ./depcomp \
gcc -I. -I. -g -O2 -c `test -f 'foo.c' || echo './'`foo.c
foo.c:2: bindir.h: No such file or directory
make: *** [foo.o] Error 1
```

In this example `bindir.h` is not distributed nor installed, and
it is not even being built on-time.

One may wonder if the ‘`nodist_foo_SOURCES = bindir.h`’ line has
any use at all. This line merely declares that `bindir.h` is a
source of `foo`, so for instance, it should be inspected while
generating tags (see [Interfacing to `etags`](#Tags)). In other words, it does not help our
present problem, and the build would fail identically without it.

#### Using `BUILT_SOURCES` [¶](#Using-BUILT_005fSOURCES)

One solution is to require `bindir.h` to be built before anything
else. This is what `BUILT_SOURCES` is meant for (see [Built Sources](#Sources)).

```
bin_PROGRAMS = foo
foo_SOURCES = foo.c
nodist_foo_SOURCES = bindir.h
BUILT_SOURCES = bindir.h
CLEANFILES = bindir.h
bindir.h: Makefile
        echo '#define bindir "$(bindir)"' >$@
```

See how `bindir.h` gets built first:

```
% make
echo '#define bindir "/usr/local/bin"' >bindir.h
make  all-am
make[1]: Entering directory `/home/adl/tmp'
source='foo.c' object='foo.o' libtool=no \
depfile='.deps/foo.Po' tmpdepfile='.deps/foo.TPo' \
depmode=gcc /bin/sh ./depcomp \
gcc -I. -I. -g -O2 -c `test -f 'foo.c' || echo './'`foo.c
gcc  -g -O2   -o foo  foo.o
make[1]: Leaving directory `/home/adl/tmp'
```

However, as said earlier, `BUILT_SOURCES` applies only to the
`all`, `check`, and `install` targets. It still fails
if you try to run ‘`make foo`’ explicitly:

```
% make clean
test -z "bindir.h" || rm -f bindir.h
test -z "foo" || rm -f foo
rm -f *.o
% : > .deps/foo.Po # Suppress previously recorded dependencies
% make foo
source='foo.c' object='foo.o' libtool=no \
depfile='.deps/foo.Po' tmpdepfile='.deps/foo.TPo' \
depmode=gcc /bin/sh ./depcomp \
gcc -I. -I. -g -O2 -c `test -f 'foo.c' || echo './'`foo.c
foo.c:2: bindir.h: No such file or directory
make: *** [foo.o] Error 1
```

#### Recording Dependencies manually [¶](#Recording-Dependencies-manually)

Usually people are happy enough with `BUILT_SOURCES` because they
never build targets such as ‘`make foo`’ before ‘`make all`’, as
in the previous example. However if this matters to you, you can
avoid `BUILT_SOURCES` and record such dependencies explicitly in
the `Makefile.am`.

```
bin_PROGRAMS = foo
foo_SOURCES = foo.c
nodist_foo_SOURCES = bindir.h
foo.$(OBJEXT): bindir.h
CLEANFILES = bindir.h
bindir.h: Makefile
        echo '#define bindir "$(bindir)"' >$@
```

You don’t have to list *all* the dependencies of `foo.o`
explicitly, only those that might need to be built. If a dependency
already exists, it will not hinder the first compilation and will be
recorded by the normal dependency tracking code. After this first
compilation, the dependency tracking code will also have recorded the
dependency between `foo.o` and `bindir.h`, so our explicit
dependency is useful only to the first build, but does no harm.

Adding explicit dependencies like this can be dangerous, because
Automake does not overwrite your own rules (it assumes you know better
than it). Thus, ‘`foo.$(OBJEXT): bindir.h`’ supersedes any rule
Automake may want to output to build ‘`foo.$(OBJEXT)`’. It works
in this case because Automake doesn’t output any ‘`foo.$(OBJEXT):`’
target: it relies on a suffix rule instead (i.e.,
‘`.c.$(OBJEXT):`’). So, in this approach, you should always check
the generated `Makefile.in` for Automake-generated rules with the
same target.

#### Build `bindir.h` from `configure` [¶](#Build-bindir_002eh-from-configure)

It’s possible to define this preprocessor macro from `configure`,
either in `config.h` (see [Defining
Directories](https://www.gnu.org/software/autoconf/manual/autoconf.html#Defining-Directories) in The Autoconf Manual), or by processing a
`bindir.h.in` file using `AC_CONFIG_FILES`
(see [Configuration Actions](https://www.gnu.org/software/autoconf/manual/autoconf.html#Configuration-Actions) in The
Autoconf Manual).

At this point it should be clear that building `bindir.h` from
`configure` works well for this example. `bindir.h` will exist
before you build any target, hence will not cause any dependency issue.

The Makefile can be shrunk as follows. We do not even have to mention
`bindir.h`.

```
bin_PROGRAMS = foo
foo_SOURCES = foo.c
```

However, it’s not always possible to build sources from
`configure`, especially when these sources are generated by a tool
that needs to be built first.

#### Build `bindir.c`, not `bindir.h`. [¶](#Build-bindir_002ec_002c-not-bindir_002eh_002e)

Another attractive idea is to define `bindir` as a variable or
function exported from `bindir.o`, and build `bindir.c`
instead of `bindir.h`.

```
noinst_PROGRAMS = foo
foo_SOURCES = foo.c bindir.h
nodist_foo_SOURCES = bindir.c
CLEANFILES = bindir.c
bindir.c: Makefile
        echo 'const char bindir[] = "$(bindir)";' >$@
```

`bindir.h` contains just the variable’s declaration and doesn’t
need to be built, so it won’t cause any trouble. `bindir.o` is
always dependent on `bindir.c`, so `bindir.c` will get built
first.

#### Which is best? [¶](#Which-is-best_003f)

There is no panacea. Each solution has its merits and drawbacks.

You cannot use `BUILT_SOURCES` if the ability to run ‘`make
foo`’ for any `foo` on a clean tree is important to you.

You won’t want to define explicit dependencies if you are leery of
overriding an Automake rule by mistake.

Building files from `./configure` is not always possible; neither
is converting `.h` files into `.c` files.

---

Next: [Building documentation](#Documentation), Previous: [Other Derived Objects](#Other-Objects), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 10 Other GNU Tools [¶](#Other-GNU-Tools-1)

Since Automake is primarily intended to generate `Makefile.in`s for
use in GNU programs, it tries hard to interoperate with other GNU tools.

* [Emacs Lisp](#Emacs-Lisp)
* [Gettext](#gettext)
* [Libtool](#Libtool)
* [Java bytecode compilation (deprecated)](#Java)
* [Python](#Python)

---

Next: [Gettext](#gettext), Up: [Other GNU Tools](#Other-GNU-Tools)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 10.1 Emacs Lisp [¶](#Emacs-Lisp-1)

Automake provides some support for Emacs Lisp. The `LISP` primary
is used to hold a list of `.el` files. Possible prefixes for this
primary are `lisp_` and `noinst_`. Note that if
`lisp_LISP` is defined, then `configure.ac` must run
`AM_PATH_LISPDIR` (see [Autoconf macros supplied with Automake](#Macros)).

Lisp sources are not distributed by default. You can prefix the
`LISP` primary with `dist_`, as in `dist_lisp_LISP` or
`dist_noinst_LISP`, to indicate that these files should be
distributed.

Automake will byte-compile all Emacs Lisp source files using the Emacs
found by `AM_PATH_LISPDIR`, if any was found. When performing such
byte-compilation, the flags specified in the (developer-reserved)
`AM_ELCFLAGS` and (user-reserved) `ELCFLAGS` make variables
will be passed to the Emacs invocation.

Byte-compiled Emacs Lisp files are not portable among all versions of
Emacs, so it makes sense to turn this off if you expect sites to have
more than one version of Emacs installed. Furthermore, many packages
do not actually benefit from byte-compilation. Still, we recommend
that you byte-compile your Emacs Lisp sources. It is probably better
for sites with strange setups to cope for themselves than to make the
installation less nice for everybody else.

There are two ways to avoid byte-compiling. Historically, we have
recommended the following construct.

```
lisp_LISP = file1.el file2.el
ELCFILES =
```

`ELCFILES` is an internal Automake variable that normally lists
all `.elc` files that must be byte-compiled. Automake defines
`ELCFILES` automatically from `lisp_LISP`. Emptying this
variable explicitly prevents byte-compilation.

Since Automake 1.8, we now recommend using `lisp_DATA` instead:

```
lisp_DATA = file1.el file2.el
```

Note that these two constructs are not equivalent. `_LISP` will
not install a file if Emacs is not installed, while `_DATA` will
always install its files.

---

Next: [Libtool](#Libtool), Previous: [Emacs Lisp](#Emacs-Lisp), Up: [Other GNU Tools](#Other-GNU-Tools)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 10.2 Gettext [¶](#Gettext)

If `AM_GNU_GETTEXT` is seen in `configure.ac`, then Automake
turns on support for GNU gettext, a message catalog system for
internationalization
(see [Introduction](https://www.gnu.org/software/gettext/manual/gettext.html#Top) in GNU gettext utilities).

The `gettext` support in Automake requires the addition of one or
two subdirectories to the package: `po` and possibly also `intl`.
The latter is needed if `AM_GNU_GETTEXT` is not invoked with the
‘`external`’ argument, or if `AM_GNU_GETTEXT_INTL_SUBDIR` is used.
Automake ensures that these directories exist and are mentioned in
`SUBDIRS`.

---

Next: [Java bytecode compilation (deprecated)](#Java), Previous: [Gettext](#gettext), Up: [Other GNU Tools](#Other-GNU-Tools)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 10.3 Libtool [¶](#Libtool-1)

Automake provides support for GNU Libtool (see [Introduction](https://www.gnu.org/software/libtool/manual/libtool.html#Top) in The Libtool Manual) with the `LTLIBRARIES` primary.
See [Building a Shared Library](#A-Shared-Library).

---

Next: [Python](#Python), Previous: [Libtool](#Libtool), Up: [Other GNU Tools](#Other-GNU-Tools)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
