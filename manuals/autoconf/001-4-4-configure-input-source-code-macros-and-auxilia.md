### 4.4 Configure Input: Source Code, Macros, and Auxiliary Files [¶](#Configure-Input_003a-Source-Code_002c-Macros_002c-and-Auxiliary-Files)

The following macros help you manage the contents of your source tree.

Macro: **AC\_CONFIG\_SRCDIR**(unique-file-in-source-dir) [¶](#index-AC_005fCONFIG_005fSRCDIR-1)
:   Distinguish this package’s source directory from other source
    directories that might happen to exist in the file system.
    unique-file-in-source-dir should name a file that is unique to
    this package. `configure` will verify that this file exists in
    `srcdir`, before it runs any other checks.

    Use of this macro is strongly recommended. It protects against people
    accidentally specifying the wrong directory with `--srcdir`.
    See [`configure` Invocation](#configure-Invocation), for more information.

Packages that use `aclocal` to generate `aclocal.m4`
should declare where local macros can be found using
`AC_CONFIG_MACRO_DIRS`.

Macro: **AC\_CONFIG\_MACRO\_DIRS**(dir1 [dir2 ... dirN]) [¶](#index-AC_005fCONFIG_005fMACRO_005fDIRS-1)

Macro: **AC\_CONFIG\_MACRO\_DIR**(dir) [¶](#index-AC_005fCONFIG_005fMACRO_005fDIR-1)
:   Specify the given directories as the location of additional local Autoconf
    macros. These macros are intended for use by commands like
    `autoreconf` or `aclocal` that trace macro calls; they should
    be called directly from `configure.ac` so that tools that install
    macros for `aclocal` can find the macros’ declarations. Tools
    that want to learn which directories have been selected should trace
    `AC_CONFIG_MACRO_DIR_TRACE`, which will be called once per directory.

    AC\_CONFIG\_MACRO\_DIRS is the preferred form, and can be called multiple
    times and with multiple arguments; in such cases, directories in earlier
    calls are expected to be searched before directories in later calls, and
    directories appearing in the same call are expected to be searched in
    the order in which they appear in the call. For historical reasons, the
    macro AC\_CONFIG\_MACRO\_DIR can also be used once, if it appears first,
    for tools such as older `libtool` that weren’t prepared to
    handle multiple directories. For example, a usage like

    ```
    AC_CONFIG_MACRO_DIR([dir1])
    AC_CONFIG_MACRO_DIRS([dir2])
    AC_CONFIG_MACRO_DIRS([dir3 dir4])
    ```

    will cause the trace of AC\_CONFIG\_MACRO\_DIR\_TRACE to appear four times,
    and should cause the directories to be searched in this order:
    ‘`dir1 dir2 dir3 dir4`’.

    Note that if you use `aclocal` from an Automake release prior to
    1.13 to generate `aclocal.m4`, you must also set
    `ACLOCAL_AMFLAGS = -I dir1 [-I dir2 ... -I dirN]`
    in your top-level `Makefile.am`. Due to a limitation in
    the Autoconf implementation of `autoreconf`, these include
    directives currently must be set on a single line in `Makefile.am`,
    without any backslash-newlines or makefile comments.

Some Autoconf macros require auxiliary scripts. `AC_PROG_INSTALL`
(see [Particular Program Checks](#Particular-Programs)) requires a
fallback implementation of `install` called `install-sh`,
and the `AC_CANONICAL` macros (see [Manual Configuration](#Manual-Configuration))
require the system-identification scripts `config.sub` and
`config.guess`. Third-party tools, such as Automake and Libtool,
may require additional auxiliary scripts.

By default, `configure` looks for these scripts next to itself,
in `srcdir`. For convenience when working with subdirectories
with their own configure scripts (see [Configuring Other Packages in Subdirectories](#Subdirectories)), if the
scripts are not in `srcdir` it will also look in
`srcdir/..` and `srcdir/../..`. All of the
scripts must be found in the same directory.

If these default locations are not adequate, or simply to reduce clutter
at the top level of the source tree, packages can use
`AC_CONFIG_AUX_DIR` to declare where to look for auxiliary scripts.

Macro: **AC\_CONFIG\_AUX\_DIR**(dir) [¶](#index-AC_005fCONFIG_005fAUX_005fDIR-1)
:   Look for auxiliary scripts in dir. Normally, dir should be a
    relative path, which is taken as relative to `srcdir`.
    If dir is an absolute path or contains shell variables, however,
    it is used as-is.

    When the goal of using `AC_CONFIG_AUX_DIR` is to reduce clutter at
    the top level of the source tree, the conventional name for dir is
    `build-aux`. If you need portability to DOS variants, do not name
    the auxiliary directory `aux`. See [File System Conventions](#File-System-Conventions).

Macro: **AC\_REQUIRE\_AUX\_FILE**(file) [¶](#index-AC_005fREQUIRE_005fAUX_005fFILE-1)
:   Declare that file is an auxiliary script needed by this configure
    script, and set the shell variable `ac_aux_dir` to the directory
    where it can be found. The value of `ac_aux_dir` is guaranteed to
    end with a ‘`/`’.

    Macros that need auxiliary scripts must use this macro to register each
    script they need.

`configure` checks for all the auxiliary scripts it needs on
startup, and exits with an error if any are missing.

`autoreconf` also detects missing auxiliary scripts. When used
with the `--install` option, `autoreconf` will try to add
missing scripts to the directory specified by `AC_CONFIG_AUX_DIR`,
or to the top level of the source tree if `AC_CONFIG_AUX_DIR` was
not used. It can always do this for the scripts needed by Autoconf core
macros: `install-sh`, `config.sub`, and `config.guess`.
Many other commonly-needed scripts are installed by the third-party
tools that `autoreconf` knows how to run, such as `missing`
for Automake and `ltmain.sh` for Libtool.

If you are using Automake, auxiliary scripts will automatically be
included in the tarball created by `make dist`. If you are
not using Automake you will need to arrange for auxiliary scripts to
be included in tarballs yourself. Auxiliary scripts should normally
*not* be checked into a version control system, for the same
reasons that `configure` shouldn’t be.

The scripts needed by Autoconf core macros can be found in
`$(datadir)/autoconf/build-aux` of the Autoconf installation
(see [Installation Directory Variables](#Installation-Directory-Variables)).
`install-sh` can be downloaded from
<https://git.savannah.gnu.org/cgit/automake.git/plain/lib/install-sh>.
`config.sub` and `config.guess` can be downloaded from
<https://git.savannah.gnu.org/cgit/config.git/tree/>.

---

Next: [Performing Configuration Actions](#Configuration-Actions), Previous: [Configure Input: Source Code, Macros, and Auxiliary Files](#Input), Up: [Initialization and Output Files](#Setup)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 4.5 Outputting Files [¶](#Outputting-Files)

Every Autoconf script, e.g., `configure.ac`, should finish by
calling `AC_OUTPUT`. That is the macro that generates and runs
`config.status`, which in turn creates the makefiles and any
other files resulting from configuration. This is the only required
macro besides `AC_INIT` (see [Configure Input: Source Code, Macros, and Auxiliary Files](#Input)).

Macro: **AC\_OUTPUT** [¶](#index-AC_005fOUTPUT-2)
:   Generate `config.status` and launch it. Call this macro once, at
    the end of `configure.ac`.

    `config.status` performs all the configuration actions: all the
    output files (see [Creating Configuration Files](#Configuration-Files), macro
    `AC_CONFIG_FILES`), header files (see [Configuration Header Files](#Configuration-Headers),
    macro `AC_CONFIG_HEADERS`), commands (see [Running Arbitrary Configuration Commands](#Configuration-Commands), macro `AC_CONFIG_COMMANDS`), links (see
    [Creating Configuration Links](#Configuration-Links), macro `AC_CONFIG_LINKS`), subdirectories
    to configure (see [Configuring Other Packages in Subdirectories](#Subdirectories), macro `AC_CONFIG_SUBDIRS`)
    are honored.

    The location of your `AC_OUTPUT` invocation is the exact point
    where configuration actions are taken: any code afterwards is
    executed by `configure` once `config.status` was run. If
    you want to bind actions to `config.status` itself
    (independently of whether `configure` is being run), see
    [Running Arbitrary Configuration
    Commands](#Configuration-Commands).

Historically, the usage of `AC_OUTPUT` was somewhat different.
See [Obsolete Macros](#Obsolete-Macros), for a description of the arguments that
`AC_OUTPUT` used to support.

If you run `make` in subdirectories, you should run it using the
`make` variable `MAKE`. Most versions of `make` set
`MAKE` to the name of the `make` program plus any options it
was given. (But many do not include in it the values of any variables
set on the command line, so those are not passed on automatically.)
Some old versions of `make` do not set this variable. The
following macro allows you to use it even with those versions.

Macro: **AC\_PROG\_MAKE\_SET** [¶](#index-AC_005fPROG_005fMAKE_005fSET-1)
:   If the Make command, `$MAKE` if set or else ‘`make`’, predefines
    `$(MAKE)`, define output variable `SET_MAKE` to be empty.
    Otherwise, define `SET_MAKE` to a macro definition that sets
    `$(MAKE)`, such as ‘`MAKE=make`’. Calls `AC_SUBST` for
    `SET_MAKE`.

If you use this macro, place a line like this in each `Makefile.in`
that runs `MAKE` on other directories:

```
@SET_MAKE@
```

---

Next: [Creating Configuration Files](#Configuration-Files), Previous: [Outputting Files](#Output), Up: [Initialization and Output Files](#Setup)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 4.6 Performing Configuration Actions [¶](#Performing-Configuration-Actions)

`configure` is designed so that it appears to do everything itself,
but there is actually a hidden slave: `config.status`.
`configure` is in charge of examining your system, but it is
`config.status` that actually takes the proper actions based on the
results of `configure`. The most typical task of
`config.status` is to *instantiate* files.

This section describes the common behavior of the four standard
instantiating macros: `AC_CONFIG_FILES`, `AC_CONFIG_HEADERS`,
`AC_CONFIG_COMMANDS` and `AC_CONFIG_LINKS`. They all
have this prototype:

```
AC_CONFIG_ITEMS(tag..., [commands], [init-cmds])
```

where the arguments are:

tag…
:   A blank-or-newline-separated list of tags, which are typically the names of
    the files to instantiate.

    You are encouraged to use literals as tags. In particular, you
    should avoid

    ```
    AS_IF([...], [my_foos="$my_foos fooo"])
    AS_IF([...], [my_foos="$my_foos foooo"])
    AC_CONFIG_ITEMS([$my_foos])
    ```

    and use this instead:

    ```
    AS_IF([...], [AC_CONFIG_ITEMS([fooo])])
    AS_IF([...], [AC_CONFIG_ITEMS([foooo])])
    ```

    The macros `AC_CONFIG_FILES` and `AC_CONFIG_HEADERS` use
    special tag values: they may have the form ‘`output`’ or
    ‘`output:inputs`’. The file output is instantiated
    from its templates, inputs (defaulting to ‘`output.in`’).

    ‘`AC_CONFIG_FILES([Makefile:boiler/top.mk:boiler/bot.mk])`’,
    for example, asks for
    the creation of the file `Makefile` that contains the expansion of the
    output variables in the concatenation of `boiler/top.mk` and
    `boiler/bot.mk`.

    The special value ‘`-`’ might be used to denote the standard output
    when used in output, or the standard input when used in the
    inputs. You most probably don’t need to use this in
    `configure.ac`, but it is convenient when using the command line
    interface of `./config.status`, see [config.status Invocation](#config_002estatus-Invocation),
    for more details.

    The inputs may be absolute or relative file names. In the latter
    case they are first looked for in the build tree, and then in the source
    tree. Input files should be text files, and a line length below 2000
    bytes should be safe.

commands
:   Shell commands output literally into `config.status`, and
    associated with a tag that the user can use to tell `config.status`
    which commands to run. The commands are run each time a tag
    request is given to `config.status`, typically each time the file
    `tag` is created.

    The variables set during the execution of `configure` are
    *not* available here: you first need to set them via the
    init-cmds. Nonetheless the following variables are pre-computed:

    `srcdir` [¶](#index-srcdir-1)
    :   The name of the top source directory, assuming that the working
        directory is the top build directory. This
        is what `configure`’s `--srcdir` option sets.

    `ac_top_srcdir` [¶](#index-ac_005ftop_005fsrcdir)
    :   The name of the top source directory, assuming that the working
        directory is the current build directory.

    `ac_top_build_prefix` [¶](#index-ac_005ftop_005fbuild_005fprefix)
    :   The name of the top build directory, assuming that the working
        directory is the current build directory.
        It can be empty, or else ends with a slash, so that you may concatenate
        it.

    `ac_srcdir` [¶](#index-ac_005fsrcdir)
    :   The name of the corresponding source directory, assuming that the
        working directory is the current build directory.

    `tmp` [¶](#index-tmp-1)
    :   The name of a temporary directory within the build tree, which you
        can use if you need to create additional temporary files. The
        directory is cleaned up when `config.status` is done or
        interrupted. Please use package-specific file name prefixes to
        avoid clashing with files that `config.status` may use
        internally.

    The *current* directory refers to the directory (or
    pseudo-directory) containing the input part of tags. For
    instance, running

    ```
    AC_CONFIG_COMMANDS([deep/dir/out:in/in.in], [...], [...])
    ```

    with `--srcdir=../package` produces the following values:

    ```
    # Argument of --srcdir
    srcdir='../package'
    # Reversing deep/dir
    ac_top_build_prefix='../../'
    # Concatenation of $ac_top_build_prefix and srcdir
    ac_top_srcdir='../../../package'
    # Concatenation of $ac_top_srcdir and deep/dir
    ac_srcdir='../../../package/deep/dir'
    ```

    independently of ‘`in/in.in`’.

init-cmds
:   Shell commands output *unquoted* near the beginning of
    `config.status`, and executed each time `config.status` runs
    (regardless of the tag). Because they are unquoted, for example,
    ‘`$var`’ is output as the value of `var`. init-cmds
    is typically used by `configure` to give `config.status` some
    variables it needs to run the commands.

    You should be extremely cautious in your variable names: all the
    init-cmds share the same name space and may overwrite each other
    in unpredictable ways. Sorry...

All these macros can be called multiple times, with different
tag values, of course!

---

Next: [Substitutions in Makefiles](#Makefile-Substitutions), Previous: [Performing Configuration Actions](#Configuration-Actions), Up: [Initialization and Output Files](#Setup)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 4.7 Creating Configuration Files [¶](#Creating-Configuration-Files)

Be sure to read the previous section, [Performing Configuration Actions](#Configuration-Actions).

Macro: **AC\_CONFIG\_FILES**(file…, [cmds], [init-cmds]) [¶](#index-AC_005fCONFIG_005fFILES-1)
:   Make `AC_OUTPUT` create each `file` by copying an input
    file (by default `file.in`), substituting the output variable
    values.
    This macro is one of the instantiating macros; see [Performing Configuration Actions](#Configuration-Actions). See [Substitutions in Makefiles](#Makefile-Substitutions), for more information on using
    output variables. See [Setting Output Variables](#Setting-Output-Variables), for more information
    on creating them. This macro creates the directory that the file is in
    if it doesn’t exist. Usually, makefiles are created this way,
    but other files, such as `.gdbinit`, can be specified as well.

    Typical calls to `AC_CONFIG_FILES` look like this:

    ```
    AC_CONFIG_FILES([Makefile src/Makefile man/Makefile X/Imakefile])
    AC_CONFIG_FILES([autoconf], [chmod +x autoconf])
    ```

    You can override an input file name by appending to file a
    colon-separated list of input files. Examples:

    ```
    AC_CONFIG_FILES([Makefile:boiler/top.mk:boiler/bot.mk]
                    [lib/Makefile:boiler/lib.mk])
    ```

    Doing this allows you to keep your file names acceptable to
    DOS variants, or
    to prepend and/or append boilerplate to the file.

    The file names should not contain shell metacharacters.
    See [Special Characters in Output Variables](#Special-Chars-in-Variables).

---

Next: [Configuration Header Files](#Configuration-Headers), Previous: [Creating Configuration Files](#Configuration-Files), Up: [Initialization and Output Files](#Setup)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 4.8 Substitutions in Makefiles [¶](#Substitutions-in-Makefiles)

Each subdirectory in a distribution that contains something to be
compiled or installed should come with a file `Makefile.in`, from
which `configure` creates a file `Makefile` in that directory.
To create `Makefile`, `configure` performs a simple variable
substitution, replacing occurrences of ‘`@variable@`’ in
`Makefile.in` with the value that `configure` has determined
for that variable. Variables that are substituted into output files in
this way are called *output variables*. They are ordinary shell
variables that are set in `configure`. To make `configure`
substitute a particular variable into the output files, the macro
`AC_SUBST` must be called with that variable name as an argument.
Any occurrences of ‘`@variable@`’ for other variables are
left unchanged. See [Setting Output Variables](#Setting-Output-Variables), for more information
on creating output variables with `AC_SUBST`.

A software package that uses a `configure` script should be
distributed with a file `Makefile.in`, but no makefile; that
way, the user has to properly configure the package for the local system
before compiling it.

See [Makefile Conventions](https://www.gnu.org/prep/standards/standards.html#Makefile-Conventions) in The
GNU Coding Standards, for more information on what to put in
makefiles.

* [Preset Output Variables](#Preset-Output-Variables)
* [Installation Directory Variables](#Installation-Directory-Variables)
* [Changed Directory Variables](#Changed-Directory-Variables)
* [Build Directories](#Build-Directories)
* [Automatic Remaking](#Automatic-Remaking)

---

Next: [Installation Directory Variables](#Installation-Directory-Variables), Up: [Substitutions in Makefiles](#Makefile-Substitutions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 4.8.1 Preset Output Variables [¶](#Preset-Output-Variables-1)

Some output variables are preset by the Autoconf macros. Some of the
Autoconf macros set additional output variables, which are mentioned in
the descriptions for those macros. See [Output Variable Index](#Output-Variable-Index), for a
complete list of output variables. See [Installation Directory Variables](#Installation-Directory-Variables), for the list of the preset ones related to installation
directories. Below are listed the other preset ones, many of which are
precious variables (see [Setting Output Variables](#Setting-Output-Variables),
`AC_ARG_VAR`).

The preset variables which are available during `config.status`
(see [Performing Configuration Actions](#Configuration-Actions)) may also be used during
`configure` tests. For example, it is permissible to reference
‘`$srcdir`’ when constructing a list of directories to pass via
the `-I` option during a compiler feature check. When used in this
manner, coupled with the fact that `configure` is always run
from the top build directory, it is sufficient to use just
‘`$srcdir`’ instead of ‘`$top_srcdir`’.

Variable: **CFLAGS** [¶](#index-CFLAGS-4)
:   Debugging and optimization options for the C compiler. If it is not set
    in the environment when `configure` runs, the default value is set
    when you call `AC_PROG_CC` (or empty if you don’t). `configure`
    uses this variable when compiling or linking programs to test for C features.

    If a compiler option affects only the behavior of the preprocessor
    (e.g., `-Dname`), it should be put into `CPPFLAGS`
    instead. If it affects only the linker (e.g., `-Ldirectory`),
    it should be put into `LDFLAGS` instead. If it
    affects only the compiler proper, `CFLAGS` is the natural home for
    it. If an option affects multiple phases of the compiler, though,
    matters get tricky:

    * If an option selects a 32-bit or 64-bit build on a bi-arch system, it
      must be put direcly into `CC`, e.g., `CC='gcc -m64'`. This is
      necessary for `config.guess` to work right.
    * Otherwise one approach is to put the option into `CC`. Another is
      to put it into both `CPPFLAGS` and `LDFLAGS`, but not into
      `CFLAGS`.

    However, remember that some `Makefile` variables are reserved by
    the GNU Coding Standards for the use of the “user”—the person
    building the package. For instance, `CFLAGS` is one such variable.

    Sometimes package developers are tempted to set user variables such as
    `CFLAGS` because it appears to make their job easier. However, the
    package itself should never set a user variable, particularly not to
    include switches that are required for proper compilation of the
    package. Since these variables are documented as being for the package
    builder, that person rightfully expects to be able to override any of
    these variables at build time. If the package developer needs to add
    switches without interfering with the user, the proper way to do that is
    to introduce an additional variable. Automake makes this easy by
    introducing `AM_CFLAGS` (see [Flag Variables Ordering](https://www.gnu.org/software/automake/manual/automake.html#Flag-Variables-Ordering) in GNU Automake), but the concept is the same even if
    Automake is not used.

Variable: **configure\_input** [¶](#index-configure_005finput-1)
:   A comment saying that the file was generated automatically by
    `configure` and giving the name of the input file.
    `AC_OUTPUT` adds a comment line containing this variable to the top
    of every makefile it creates. For other files, you should
    reference this variable in a comment at the top of each input file. For
    example, an input shell script should begin like this:

    ```
    #!/bin/sh
    # @configure_input@
    ```

    The presence of that line also reminds people editing the file that it
    needs to be processed by `configure` in order to be used.

Variable: **CPPFLAGS** [¶](#index-CPPFLAGS-2)
:   Preprocessor options for the C, C++, Objective C, and Objective C++
    preprocessors and compilers. If
    it is not set in the environment when `configure` runs, the default
    value is empty. `configure` uses this variable when preprocessing
    or compiling programs to test for C, C++, Objective C, and Objective C++
    features.

    This variable’s contents should contain options like `-I`,
    `-D`, and `-U` that affect only the behavior of the
    preprocessor. Please see the explanation of `CFLAGS` for what you
    can do if an option affects other phases of the compiler as well.

    Currently, `configure` always links as part of a single
    invocation of the compiler that also preprocesses and compiles, so it
    uses this variable also when linking programs. However, it is unwise to
    depend on this behavior because the GNU Coding Standards do
    not require it and many packages do not use `CPPFLAGS` when linking
    programs.

    See [Special Characters in Output Variables](#Special-Chars-in-Variables), for limitations that `CPPFLAGS`
    might run into.

Variable: **CXXFLAGS** [¶](#index-CXXFLAGS-4)
:   Debugging and optimization options for the C++ compiler. It acts like
    `CFLAGS`, but for C++ instead of C.

Variable: **DEFS** [¶](#index-DEFS-1)
:   `-D` options to pass to the C compiler. If `AC_CONFIG_HEADERS`
    is called, `configure` replaces ‘`@DEFS@`’ with
    `-DHAVE_CONFIG_H` instead (see [Configuration Header Files](#Configuration-Headers)). This
    variable is not defined while `configure` is performing its tests,
    only when creating the output files. See [Setting Output Variables](#Setting-Output-Variables), for
    how to check the results of previous tests.

Variable: **ECHO\_C** [¶](#index-ECHO_005fC-1)

Variable: **ECHO\_N** [¶](#index-ECHO_005fN-1)

Variable: **ECHO\_T** [¶](#index-ECHO_005fT-1)
:   These obsolescent variables let you suppress the trailing newline from
    `echo` for question-answer message pairs.
    Nowadays it is better to use `AS_ECHO_N`.

Variable: **ERLCFLAGS** [¶](#index-ERLCFLAGS-5)
:   Debugging and optimization options for the Erlang compiler. If it is not set
    in the environment when `configure` runs, the default value is empty.
    `configure` uses this variable when compiling
    programs to test for Erlang features.

Variable: **FCFLAGS** [¶](#index-FCFLAGS-4)
:   Debugging and optimization options for the Fortran compiler. If it
    is not set in the environment when `configure` runs, the default
    value is set when you call `AC_PROG_FC` (or empty if you don’t).
    `configure` uses this variable when compiling or linking
    programs to test for Fortran features.

Variable: **FFLAGS** [¶](#index-FFLAGS-4)
:   Debugging and optimization options for the Fortran 77 compiler. If it
    is not set in the environment when `configure` runs, the default
    value is set when you call `AC_PROG_F77` (or empty if you don’t).
    `configure` uses this variable when compiling or linking
    programs to test for Fortran 77 features.

Variable: **LDFLAGS** [¶](#index-LDFLAGS-2)
:   Options for the linker. If it is not set
    in the environment when `configure` runs, the default value is empty.
    `configure` uses this variable when linking programs to test for
    features in C, C++, Objective C, Objective C++, Fortran, Go, and Algol 68.

    This variable’s contents should contain options like `-s` and
    `-L` that affect only the behavior of the linker. Please see the
    explanation of `CFLAGS` for what you can do if an option also
    affects other phases of the compiler.

    Don’t use this variable to pass library names
    (`-l`) to the linker; use `LIBS` instead.

Variable: **LIBS** [¶](#index-LIBS-5)
:   `-l` options to pass to the linker. The default value is empty,
    but some Autoconf macros may prepend extra libraries to this variable if
    those libraries are found and provide necessary functions, see
    [Library Files](#Libraries). `configure` uses this variable when linking
    programs to test for C, C++, Objective C, Objective C++, Fortran, and Go
    features.

Variable: **OBJCFLAGS** [¶](#index-OBJCFLAGS-4)
:   Debugging and optimization options for the Objective C compiler. It
    acts like `CFLAGS`, but for Objective C instead of C.

Variable: **OBJCXXFLAGS** [¶](#index-OBJCXXFLAGS-4)
:   Debugging and optimization options for the Objective C++ compiler. It
    acts like `CXXFLAGS`, but for Objective C++ instead of C++.

Variable: **GOFLAGS** [¶](#index-GOFLAGS-2)
:   Debugging and optimization options for the Go compiler. It acts like
    `CFLAGS`, but for Go instead of C.

Variable: **A68FLAGS** [¶](#index-A68FLAGS-2)
:   Debugging and optimization options for the Algol 68 compiler. It acts
    like `CFLAGS`, but for Algol 68 instead of C.

Variable: **builddir** [¶](#index-builddir-1)
:   Rigorously equal to ‘`.`’. Added for symmetry only.

Variable: **abs\_builddir** [¶](#index-abs_005fbuilddir-1)
:   Absolute name of `builddir`.

Variable: **top\_builddir** [¶](#index-top_005fbuilddir-1)
:   The relative name of the top level of the current build tree. In the
    top-level directory, this is the same as `builddir`.

Variable: **top\_build\_prefix** [¶](#index-top_005fbuild_005fprefix-1)
:   The relative name of the top level of the current build tree with final
    slash if nonempty. This is the same as `top_builddir`, except that
    it contains zero or more runs of `../`, so it should not be
    appended with a slash for concatenation. This helps for `make`
    implementations that otherwise do not treat `./file` and `file`
    as equal in the top-level build directory.

Variable: **abs\_top\_builddir** [¶](#index-abs_005ftop_005fbuilddir-1)
:   Absolute name of `top_builddir`.

Variable: **srcdir** [¶](#index-srcdir-2)
:   The name of the directory that contains the source code for
    that makefile.

Variable: **abs\_srcdir** [¶](#index-abs_005fsrcdir-1)
:   Absolute name of `srcdir`.

Variable: **top\_srcdir** [¶](#index-top_005fsrcdir-1)
:   The name of the top-level source code directory for the
    package. In the top-level directory, this is the same as `srcdir`.

Variable: **abs\_top\_srcdir** [¶](#index-abs_005ftop_005fsrcdir-1)
:   Absolute name of `top_srcdir`.

---

Next: [Changed Directory Variables](#Changed-Directory-Variables), Previous: [Preset Output Variables](#Preset-Output-Variables), Up: [Substitutions in Makefiles](#Makefile-Substitutions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 4.8.2 Installation Directory Variables [¶](#Installation-Directory-Variables-1)

The following variables specify the directories for
package installation, see [Variables for
Installation Directories](https://www.gnu.org/prep/standards/standards.html#Directory-Variables) in The GNU Coding
Standards, for more information. Each variable corresponds to an
argument of `configure`; trailing slashes are stripped so that
expressions such as ‘`${prefix}/lib`’ expand with only one slash
between directory names. See the end of this section for
details on when and how to use these variables.

Variable: **bindir** [¶](#index-bindir-1)
:   The directory for installing executables that users run.

Variable: **datadir** [¶](#index-datadir-1)
:   The directory for installing idiosyncratic read-only
    architecture-independent data.

Variable: **datarootdir** [¶](#index-datarootdir-2)
:   The root of the directory tree for read-only architecture-independent
    data files.

Variable: **docdir** [¶](#index-docdir-1)
:   The directory for installing documentation files (other than Info and
    man).

Variable: **dvidir** [¶](#index-dvidir-1)
:   The directory for installing documentation files in DVI format.

Variable: **exec\_prefix** [¶](#index-exec_005fprefix-1)
:   The installation prefix for architecture-dependent files. By default
    it’s the same as `prefix`. You should avoid installing anything
    directly to `exec_prefix`. However, the default value for
    directories containing architecture-dependent files should be relative
    to `exec_prefix`.

Variable: **htmldir** [¶](#index-htmldir-1)
:   The directory for installing HTML documentation.

Variable: **includedir** [¶](#index-includedir-1)
:   The directory for installing C header files.

Variable: **infodir** [¶](#index-infodir-1)
:   The directory for installing documentation in Info format.

Variable: **libdir** [¶](#index-libdir-1)
:   The directory for installing object code libraries.

Variable: **libexecdir** [¶](#index-libexecdir-1)
:   The directory for installing executables that other programs run.

Variable: **localedir** [¶](#index-localedir-1)
:   The directory for installing locale-dependent but
    architecture-independent data, such as message catalogs. This directory
    usually has a subdirectory per locale.

Variable: **localstatedir** [¶](#index-localstatedir-1)
:   The directory for installing modifiable single-machine data. Content in
    this directory typically survives a reboot.

Variable: **runstatedir** [¶](#index-runstatedir-1)
:   The directory for installing temporary modifiable single-machine data.
    Content in this directory survives as long as the process is running
    (such as pid files), as contrasted with `/tmp` that may be
    periodically cleaned. Conversely, this directory is typically cleaned
    on a reboot. By default, this is a subdirectory of
    `localstatedir`.

Variable: **mandir** [¶](#index-mandir-1)
:   The top-level directory for installing documentation in man format.

Variable: **oldincludedir** [¶](#index-oldincludedir-1)
:   The directory for installing C header files for non-GCC compilers.

Variable: **pdfdir** [¶](#index-pdfdir-1)
:   The directory for installing PDF documentation.

Variable: **prefix** [¶](#index-prefix-1)
:   The common installation prefix for all files. If `exec_prefix`
    is defined to a different value, `prefix` is used only for
    architecture-independent files.

Variable: **psdir** [¶](#index-psdir-1)
:   The directory for installing PostScript documentation.

Variable: **sbindir** [¶](#index-sbindir-1)
:   The directory for installing executables that system
    administrators run.

Variable: **sharedstatedir** [¶](#index-sharedstatedir-1)
:   The directory for installing modifiable architecture-independent data.

Variable: **sysconfdir** [¶](#index-sysconfdir-1)
:   The directory for installing read-only single-machine data.

Most of these variables have values that rely on `prefix` or
`exec_prefix`. It is deliberate that the directory output
variables keep them unexpanded: typically ‘`@datarootdir@`’ is
replaced by ‘`${prefix}/share`’, not ‘`/usr/local/share`’, and
‘`@datadir@`’ is replaced by ‘`${datarootdir}`’.

This behavior is mandated by the GNU Coding Standards, so that when
the user runs:

‘`make`’
:   she can still specify a different prefix from the one specified to
    `configure`, in which case, if needed, the package should hard
    code dependencies corresponding to the make-specified prefix.

‘`make install`’
:   she can specify a different installation location, in which case the
    package *must* still depend on the location which was compiled in
    (i.e., never recompile when ‘`make install`’ is run). This is an
    extremely important feature, as many people may decide to install all
    the files of a package grouped together, and then install links from
    the final locations to there.

In order to support these features, it is essential that
`datarootdir` remains defined as ‘`${prefix}/share`’,
so that its value can be expanded based
on the current value of `prefix`.

A corollary is that you should not use these variables except in
makefiles. For instance, instead of trying to evaluate `datadir`
in `configure` and hard-coding it in makefiles using
e.g., ‘`AC_DEFINE_UNQUOTED([DATADIR], ["$datadir"], [Data directory.])`’,
you should add
`-DDATADIR='$(datadir)'` to your makefile’s definition of
`CPPFLAGS` (`AM_CPPFLAGS` if you are also using Automake).

Similarly, you should not rely on `AC_CONFIG_FILES` to replace
`bindir` and friends in your shell scripts and other files; instead,
let `make` manage their replacement. For instance Autoconf
ships templates of its shell scripts ending with ‘`.in`’, and uses a
makefile snippet similar to the following to build scripts like
`autoheader` and `autom4te`:

```
edit = sed \
        -e 's|@bindir[@]|$(bindir)|g' \
        -e 's|@pkgdatadir[@]|$(pkgdatadir)|g' \
        -e 's|@prefix[@]|$(prefix)|g'
```

```

```

```
autoheader autom4te: Makefile
        rm -f $@ $@.tmp
        srcdir=''; \
          test -f ./$@.in || srcdir=$(srcdir)/; \
          $(edit) $${srcdir}$@.in >$@.tmp
        chmod +x $@.tmp
        chmod a-w $@.tmp
        mv $@.tmp $@
```

```

```

```
autoheader: $(srcdir)/autoheader.in
autom4te: $(srcdir)/autom4te.in
```

Some details are noteworthy:

‘`@bindir[@]`’
:   The brackets prevent `configure` from replacing
    ‘`@bindir@`’ in the Sed expression itself.
    Brackets are preferable to a backslash here, since
    POSIX says ‘`\@`’ is not portable.

‘`$(bindir)`’
:   Don’t use ‘`@bindir@`’! Use the matching makefile variable
    instead.

‘`$(pkgdatadir)`’
:   The example takes advantage of the variable ‘`$(pkgdatadir)`’
    provided by Automake; it is equivalent to ‘`$(datadir)/$(PACKAGE)`’.

‘`/`’
:   Don’t use ‘`/`’ in the Sed expressions that replace file names since
    most likely the
    variables you use, such as ‘`$(bindir)`’, contain ‘`/`’.
    Use a shell metacharacter instead, such as ‘`|`’.

special characters
:   File names, file name components, and the value of `VPATH` should
    not contain shell metacharacters or white
    space. See [Special Characters in Output Variables](#Special-Chars-in-Variables).

dependency on `Makefile`
:   Since `edit` uses values that depend on the configuration specific
    values (`prefix`, etc.) and not only on `VERSION` and so forth,
    the output depends on `Makefile`, not `configure.ac`.

‘`$@`’
:   The main rule is generic, and uses ‘`$@`’ extensively to
    avoid the need for multiple copies of the rule.

Separated dependencies and single suffix rules
:   You can’t use them! The above snippet cannot be (portably) rewritten
    as:

    ```
    autoconf autoheader: Makefile
    ```

    ```
    .in:
            rm -f $@ $@.tmp
            $(edit) $< >$@.tmp
            chmod +x $@.tmp
            mv $@.tmp $@
    ```

    See [Single Suffix Rules and Separated Dependencies](#Single-Suffix-Rules), for details.

‘`$(srcdir)`’
:   Be sure to specify the name of the source directory,
    otherwise the package won’t support separated builds.

For the more specific installation of Erlang libraries, the following variables
are defined:

Variable: **ERLANG\_INSTALL\_LIB\_DIR** [¶](#index-ERLANG_005fINSTALL_005fLIB_005fDIR-2)
:   The common parent directory of Erlang library installation directories.
    This variable is set by calling the `AC_ERLANG_SUBST_INSTALL_LIB_DIR`
    macro in `configure.ac`.

Variable: **ERLANG\_INSTALL\_LIB\_DIR\_library** [¶](#index-ERLANG_005fINSTALL_005fLIB_005fDIR_005flibrary-2)
:   The installation directory for Erlang library library.
    This variable is set by using the
    ‘`AC_ERLANG_SUBST_INSTALL_LIB_SUBDIR`’
    macro in `configure.ac`.

See [Erlang Libraries](#Erlang-Libraries), for details.

---

Next: [Build Directories](#Build-Directories), Previous: [Installation Directory Variables](#Installation-Directory-Variables), Up: [Substitutions in Makefiles](#Makefile-Substitutions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 4.8.3 Changed Directory Variables [¶](#Changed-Directory-Variables-1)

In Autoconf 2.60, the set of directory variables has changed, and the
defaults of some variables have been adjusted
(see [Installation Directory Variables](#Installation-Directory-Variables)) to changes in the
GNU Coding Standards. Notably, `datadir`, `infodir`, and
`mandir` are now expressed in terms of `datarootdir`. If you are
upgrading from an earlier Autoconf version, you may need to adjust your files
to ensure that the directory variables are substituted correctly
(see [How Do I `#define` Installation Directories?](#Defining-Directories)), and that a definition of `datarootdir` is
in place. For example, in a `Makefile.in`, adding

```
datarootdir = @datarootdir@
```

is usually sufficient. If you use Automake to create `Makefile.in`,
it will add this for you.

To help with the transition, Autoconf warns about files that seem to use
`datarootdir` without defining it. In some cases, it then expands
the value of `$datarootdir` in substitutions of the directory
variables. The following example shows such a warning:

```
$ cat configure.ac
AC_INIT
AC_CONFIG_FILES([Makefile])
AC_OUTPUT
$ cat Makefile.in
prefix = @prefix@
datadir = @datadir@
$ autoconf
$ configure
configure: creating ./config.status
config.status: creating Makefile
config.status: WARNING:
               Makefile.in seems to ignore the --datarootdir setting
$ cat Makefile
prefix = /usr/local
datadir = ${prefix}/share
```

Usually one can easily change the file to accommodate both older and newer
Autoconf releases:

```
$ cat Makefile.in
prefix = @prefix@
datarootdir = @datarootdir@
datadir = @datadir@
$ configure
configure: creating ./config.status
config.status: creating Makefile
$ cat Makefile
prefix = /usr/local
datarootdir = ${prefix}/share
datadir = ${datarootdir}
```

In some cases, however, the checks may not be able to detect that a suitable
initialization of `datarootdir` is in place, or they may fail to detect
that such an initialization is necessary in the output file. If, after
auditing your package, there are still spurious `configure` warnings about
`datarootdir`, you may add the line

```
AC_DEFUN([AC_DATAROOTDIR_CHECKED])
```

to your `configure.ac` to disable the warnings. This is an exception
to the usual rule that you should not define a macro whose name begins with
`AC_` (see [Macro Names](#Macro-Names)).

---

Next: [Automatic Remaking](#Automatic-Remaking), Previous: [Changed Directory Variables](#Changed-Directory-Variables), Up: [Substitutions in Makefiles](#Makefile-Substitutions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 4.8.4 Build Directories [¶](#Build-Directories-1)

You can support compiling a software package for several architectures
simultaneously from the same copy of the source code. The object files
for each architecture are kept in their own directory.

To support doing this, `make` uses the `VPATH` variable to
find the files that are in the source directory. GNU Make
can do this. Most other recent `make` programs can do this as
well, though they may have difficulties and it is often simpler to
recommend GNU `make` (see [`VPATH` and Make](#VPATH-and-Make)). Older
`make` programs do not support `VPATH`; when using them, the
source code must be in the same directory as the object files.

If you are using GNU Automake, the remaining details in this
section are already covered for you, based on the contents of your
`Makefile.am`. But if you are using Autoconf in isolation, then
supporting `VPATH` requires the following in your
`Makefile.in`:

```
srcdir = @srcdir@
VPATH = @srcdir@
```

Do not set `VPATH` to the value of another variable (see [Variables listed in `VPATH`](#Variables-listed-in-VPATH).

`configure` substitutes the correct value for `srcdir` when
it produces `Makefile`.

Do not use the `make` variable `$<`, which expands to the
file name of the file in the source directory (found with `VPATH`),
except in implicit rules. (An implicit rule is one such as ‘`.c.o`’,
which tells how to create a `.o` file from a `.c` file.) Some
versions of `make` do not set `$<` in explicit rules; they
expand it to an empty value.

Instead, Make command lines should always refer to source
files by prefixing them with ‘`$(srcdir)/`’. It’s safer
to quote the source directory name, in case it contains characters that
are special to the shell. Because ‘`$(srcdir)`’ is expanded by Make,
single-quoting works and is safer than double-quoting. For example:

```
time.info: time.texinfo
        $(MAKEINFO) '$(srcdir)/time.texinfo'
```

---

Previous: [Build Directories](#Build-Directories), Up: [Substitutions in Makefiles](#Makefile-Substitutions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 4.8.5 Automatic Remaking [¶](#Automatic-Remaking-1)

You can put rules like the following in the top-level `Makefile.in`
for a package to automatically update the configuration information when
you change the configuration files. This example includes all of the
optional files, such as `aclocal.m4` and those related to
configuration header files. Omit from the `Makefile.in` rules for
any of these files that your package does not use.

The ‘`$(srcdir)/`’ prefix is included because of limitations in the
`VPATH` mechanism.

The `stamp-` files are necessary because the timestamps of
`config.h.in` and `config.h` are not changed if remaking
them does not change their contents. This feature avoids unnecessary
recompilation. You should include the file `stamp-h.in` in your
package’s distribution, so that `make` considers
`config.h.in` up to date. Don’t use `touch`
(see [Limitations of Usual Tools](#touch)); instead, use
`echo` (using
`date` would cause needless output differences).

```
$(srcdir)/configure: configure.ac aclocal.m4
        cd '$(srcdir)' && autoconf

# autoheader might not change config.h.in, so touch a stamp file.
$(srcdir)/config.h.in: stamp-h.in ;
$(srcdir)/stamp-h.in: configure.ac aclocal.m4
        cd '$(srcdir)' && autoheader
        echo timestamp > '$(srcdir)/stamp-h.in'

config.h: stamp-h ;
stamp-h: config.h.in config.status
        ./config.status

Makefile: Makefile.in config.status
        ./config.status

config.status: configure
        ./config.status --recheck
```

(Be careful if you copy these lines directly into your makefile, as you
need to convert the indented lines to start with the tab character.)

In addition, you should use

```
AC_CONFIG_FILES([stamp-h], [echo timestamp > stamp-h])
```

so `config.status` ensures that `config.h` is considered up to
date. See [Outputting Files](#Output), for more information about `AC_OUTPUT`.

See [config.status Invocation](#config_002estatus-Invocation), for more examples of handling
configuration-related dependencies.

---

Next: [Running Arbitrary Configuration Commands](#Configuration-Commands), Previous: [Substitutions in Makefiles](#Makefile-Substitutions), Up: [Initialization and Output Files](#Setup)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 4.9 Configuration Header Files [¶](#Configuration-Header-Files)

When a package contains more than a few tests that define C preprocessor
symbols, the command lines to pass `-D` options to the compiler
can get quite long. This causes two problems. One is that the
`make` output is hard to visually scan for errors. More
seriously, the command lines can exceed the length limits of some
operating systems. As an alternative to passing `-D` options to
the compiler, `configure` scripts can create a C header file
containing ‘`#define`’ directives. The `AC_CONFIG_HEADERS`
macro selects this kind of output. Though it can be called anywhere
between `AC_INIT` and `AC_OUTPUT`, it is customary to call
it right after `AC_INIT`.

The package should ‘`#include`’ the configuration header file before
any other header files, to prevent inconsistencies in declarations (for
example, if it redefines `const`, or if it defines a macro like
`_FILE_OFFSET_BITS` that affects the behavior of system
headers). Note that it is okay to only include `config.h` from
`.c` files; the project’s `.h` files can rely on
`config.h` already being included first by the corresponding
`.c` file.

To provide for VPATH builds, remember to pass the C compiler a `-I.`
option (or `-I..`; whichever directory contains `config.h`).
Even if you use ‘`#include "config.h"`’, the preprocessor searches only
the directory of the currently read file, i.e., the source directory, not
the build directory.

With the appropriate `-I` option, you can use
‘`#include <config.h>`’. Actually, it’s a good habit to use it,
because in the rare case when the source directory contains another
`config.h`, the build directory should be searched first.

Macro: **AC\_CONFIG\_HEADERS**(header …, [cmds], [init-cmds]) [¶](#index-AC_005fCONFIG_005fHEADERS-1)
:   This macro is one of the instantiating macros; see [Performing Configuration Actions](#Configuration-Actions). Make `AC_OUTPUT` create the file(s) in the
    blank-or-newline-separated list header containing C preprocessor
    `#define` statements, and replace ‘`@DEFS@`’ in generated
    files with `-DHAVE_CONFIG_H` instead of the value of `DEFS`.
    The usual name for header is `config.h`;
    header should not contain shell metacharacters.
    See [Special Characters in Output Variables](#Special-Chars-in-Variables).

    If header already exists and its contents are identical to what
    `AC_OUTPUT` would put in it, it is left alone. Doing this allows
    making some changes in the configuration without needlessly causing
    object files that depend on the header file to be recompiled.

    Usually the input file is named `header.in`; however, you can
    override the input file name by appending to header a
    colon-separated list of input files. For example, you might need to make
    the input file name acceptable to DOS variants:

    ```
    AC_CONFIG_HEADERS([config.h:config.hin])
    ```

Macro: **AH\_HEADER** [¶](#index-AH_005fHEADER-1)
:   This macro is defined as the name of the first declared config header
    and undefined if no config headers have been declared up to this point.
    A third-party macro may, for example, require use of a config header
    without invoking AC\_CONFIG\_HEADERS twice, like this:

    ```
    AC_CONFIG_COMMANDS_PRE(
            [m4_ifndef([AH_HEADER], [AC_CONFIG_HEADERS([config.h])])])
    ```

See [Performing Configuration Actions](#Configuration-Actions), for more details on header.

* [Configuration Header Templates](#Header-Templates)
* [Using `autoheader` to Create `config.h.in`](#autoheader-Invocation)
* [Autoheader Macros](#Autoheader-Macros)

---

Next: [Using `autoheader` to Create `config.h.in`](#autoheader-Invocation), Up: [Configuration Header Files](#Configuration-Headers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 4.9.1 Configuration Header Templates [¶](#Configuration-Header-Templates)

Your distribution should contain a template file that looks as you want
the final header file to look, including comments, with `#undef`
statements which are used as hooks. For example, suppose your
`configure.ac` makes these calls:

```
AC_CONFIG_HEADERS([conf.h])
AC_CHECK_HEADERS([unistd.h])
```

Then you could have code like the following in `conf.h.in`.
The `conf.h` created by `configure` defines ‘`HAVE_UNISTD_H`’
to 1, if and only if the system has `unistd.h`.

```
/* Define as 1 if you have unistd.h.  */
#undef HAVE_UNISTD_H
```

The format of the template file is stricter than what the C preprocessor
is required to accept. A directive line should contain only whitespace,
‘`#undef`’, and ‘`HAVE_UNISTD_H`’. The use of ‘`#define`’
instead of ‘`#undef`’, or of comments on the same line as
‘`#undef`’, is strongly discouraged. Each hook should only be listed
once. Other preprocessor lines, such as ‘`#ifdef`’ or
‘`#include`’, are copied verbatim from the template into the
generated header.

Since it is a tedious task to keep a template header up to date, you may
use `autoheader` to generate it, see [Using `autoheader` to Create `config.h.in`](#autoheader-Invocation).

During the instantiation of the header, each ‘`#undef`’ line in the
template file for each symbol defined by ‘`AC_DEFINE`’ is changed to an
appropriate ‘`#define`’. If the corresponding ‘`AC_DEFINE`’ has not
been executed during the `configure` run, the ‘`#undef`’ line is
commented out. (This is important, e.g., for ‘`_POSIX_SOURCE`’:
on many systems, it can be implicitly defined by the compiler, and
undefining it in the header would then break compilation of subsequent
headers.)

Currently, *all* remaining ‘`#undef`’ lines in the header
template are commented out, whether or not there was a corresponding
‘`AC_DEFINE`’ for the macro name; but this behavior is not guaranteed
for future releases of Autoconf.

Generally speaking, since you should not use ‘`#define`’, and you
cannot guarantee whether a ‘`#undef`’ directive in the header
template will be converted to a ‘`#define`’ or commented out in the
generated header file, the template file cannot be used for conditional
definition effects. Consequently, if you need to use the construct

```
#ifdef THIS
# define THAT
#endif
```

you must place it outside of the template.
If you absolutely need to hook it to the config header itself, please put
the directives to a separate file, and ‘`#include`’ that file from the
config header template. If you are using `autoheader`, you would
probably use ‘`AH_BOTTOM`’ to append the ‘`#include`’ directive.

---

Next: [Autoheader Macros](#Autoheader-Macros), Previous: [Configuration Header Templates](#Header-Templates), Up: [Configuration Header Files](#Configuration-Headers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 4.9.2 Using `autoheader` to Create `config.h.in` [¶](#Using-autoheader-to-Create-config_002eh_002ein)

The `autoheader` program can create a template file of C
‘`#define`’ statements for `configure` to use.
It searches for the first invocation of `AC_CONFIG_HEADERS` in
`configure` sources to determine the name of the template.
(If the first call of `AC_CONFIG_HEADERS` specifies more than one
input file name, `autoheader` uses the first one.)

It is recommended that only one input file is used. If you want to append
a boilerplate code, it is preferable to use
‘`AH_BOTTOM([#include <conf_post.h>])`’.
File `conf_post.h` is not processed during the configuration then,
which make things clearer. Analogically, `AH_TOP` can be used to
prepend a boilerplate code.

In order to do its job, `autoheader` needs you to document all
of the symbols that you might use. Typically this is done via an
`AC_DEFINE` or `AC_DEFINE_UNQUOTED` call whose first argument
is a literal symbol and whose third argument describes the symbol
(see [Defining C Preprocessor Symbols](#Defining-Symbols)). Alternatively, you can use
`AH_TEMPLATE` (see [Autoheader Macros](#Autoheader-Macros)), or you can supply a
suitable input file for a subsequent configuration header file.
Symbols defined by Autoconf’s builtin tests are already documented properly;
you need to document only those that you
define yourself.

You might wonder why `autoheader` is needed: after all, why
would `configure` need to “patch” a `config.h.in` to
produce a `config.h` instead of just creating `config.h` from
scratch? Well, when everything rocks, the answer is just that we are
wasting our time maintaining `autoheader`: generating
`config.h` directly is all that is needed. When things go wrong,
however, you’ll be thankful for the existence of `autoheader`.

The fact that the symbols are documented is important in order to
*check* that `config.h` makes sense. The fact that there is a
well-defined list of symbols that should be defined (or not) is
also important for people who are porting packages to environments where
`configure` cannot be run: they just have to *fill in the
blanks*.

But let’s come back to the point: the invocation of `autoheader`…

If you give `autoheader` an argument, it uses that file instead
of `configure.ac` and writes the header file to the standard output
instead of to `config.h.in`. If you give `autoheader` an
argument of `-`, it reads the standard input instead of
`configure.ac` and writes the header file to the standard output.

`autoheader` accepts the following options:

`--help`

`-h`
:   Print a summary of the command line options and exit.

`--version`

`-V`
:   Print the version number of Autoconf and exit.

`--verbose`

`-v`
:   Report processing steps.

`--debug`

`-d`
:   Don’t remove the temporary files.

`--force`

`-f`
:   Remake the template file even if newer than its input files.

`--replace-handwritten`

`-R`
:   Remake the template file even if it appears not to have been generated
    by `autoheader`.

`--include=dir`

`-I dir`
:   Append dir to the include path. Multiple invocations accumulate.

`--prepend-include=dir`

`-B dir`
:   Prepend dir to the include path. Multiple invocations accumulate.

`--warnings=category[,category...]` [¶](#index-WARNINGS-2)

`-Wcategory[,category...]`
:   Enable or disable warnings related to each category.
    See [m4\_warn](#m4_005fwarn), for a comprehensive list of categories.
    Special values include:

    ‘`all`’
    :   Enable all categories of warnings.

    ‘`none`’
    :   Disable all categories of warnings.

    ‘`error`’
    :   Treat all warnings as errors.

    ‘`no-category`’
    :   Disable warnings falling into category.

    The environment variable `WARNINGS` may also be set to a
    comma-separated list of warning categories to enable or disable.
    It is interpreted exactly the same way as the argument of
    `--warnings`, but unknown categories are silently ignored.
    The command line takes precedence; for instance, if `WARNINGS`
    is set to `obsolete`, but `-Wnone` is given on the
    command line, no warnings will be issued.

    Some categories of warnings are on by default.
    Again, for details see [m4\_warn](#m4_005fwarn).

---

Previous: [Using `autoheader` to Create `config.h.in`](#autoheader-Invocation), Up: [Configuration Header Files](#Configuration-Headers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 4.9.3 Autoheader Macros [¶](#Autoheader-Macros-1)

`autoheader` scans `configure.ac` and figures out which C
preprocessor symbols it might define. It knows how to generate
templates for symbols defined by `AC_CHECK_HEADERS`,
`AC_CHECK_FUNCS` etc., but if you `AC_DEFINE` any additional
symbol, you must define a template for it. If there are missing
templates, `autoheader` fails with an error message.

The template for a symbol is created
by `autoheader` from
the description argument to an `AC_DEFINE`;
see [Defining C Preprocessor Symbols](#Defining-Symbols).

For special needs, you can use the following macros.

Macro: **AH\_TEMPLATE**(key, description) [¶](#index-AH_005fTEMPLATE-1)
:   Tell `autoheader` to generate a template for key. This macro
    generates standard templates just like `AC_DEFINE` when a
    description is given.

    For example:

    ```
    AH_TEMPLATE([NULL_DEVICE],
      [Name of the file to open to get
       a null file, or a data sink.])
    ```

    generates the following template, with the description properly
    justified.

    ```
    /* Name of the file to open to get a null file, or a data sink. */
    #undef NULL_DEVICE
    ```

Macro: **AH\_VERBATIM**(key, template) [¶](#index-AH_005fVERBATIM-1)
:   Tell `autoheader` to include the template as-is in the header
    template file. This template is associated with the key,
    which is used to sort all the different templates and guarantee their
    uniqueness. It should be a symbol that can be defined via `AC_DEFINE`.

Macro: **AH\_TOP**(text) [¶](#index-AH_005fTOP-1)
:   Include text at the top of the header template file.

Macro: **AH\_BOTTOM**(text) [¶](#index-AH_005fBOTTOM-1)
:   Include text at the bottom of the header template file.

Please note that text gets included “verbatim” to the template file,
not to the resulting config header, so it can easily get mangled when the
template is processed. There is rarely a need for something other than

```
AH_BOTTOM([#include <custom.h>])
```

---

Next: [Creating Configuration Links](#Configuration-Links), Previous: [Configuration Header Files](#Configuration-Headers), Up: [Initialization and Output Files](#Setup)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 4.10 Running Arbitrary Configuration Commands [¶](#Running-Arbitrary-Configuration-Commands)

You can execute arbitrary commands before, during, and after
`config.status` is run. The three following macros accumulate the
commands to run when they are called multiple times.
`AC_CONFIG_COMMANDS` replaces the obsolete macro
`AC_OUTPUT_COMMANDS`; see [Obsolete Macros](#Obsolete-Macros), for details.

Macro: **AC\_CONFIG\_COMMANDS**(tag…, [cmds], [init-cmds]) [¶](#index-AC_005fCONFIG_005fCOMMANDS-1)
:   Specify additional shell commands to run at the end of
    `config.status`, and shell commands to initialize any variables
    from `configure`. Associate the commands with tag.
    Since typically the cmds create a file, tag should
    naturally be the name of that file. If needed, the directory hosting
    tag is created. The tag should not contain shell
    metacharacters. See [Special Characters in Output Variables](#Special-Chars-in-Variables).
    This macro is one of the instantiating macros;
    see [Performing Configuration Actions](#Configuration-Actions).

    Here is an unrealistic example:

    ```
    fubar=42
    AC_CONFIG_COMMANDS([fubar],
      [AS_ECHO(["this is extra $fubar, and so on."])],
      [fubar=$fubar])
    ```

    Here is a better one:

    ```
    AC_CONFIG_COMMANDS([timestamp], [echo >timestamp])
    ```

The following two macros look similar, but in fact they are not of the same
breed: they are executed directly by `configure`, so you cannot use
`config.status` to rerun them.

Macro: **AC\_CONFIG\_COMMANDS\_PRE**(cmds) [¶](#index-AC_005fCONFIG_005fCOMMANDS_005fPRE-1)
:   Execute the cmds right before creating `config.status`.

    This macro presents the last opportunity to call `AC_SUBST`,
    `AC_DEFINE`, or `AC_CONFIG_ITEMS` macros.

Macro: **AC\_CONFIG\_COMMANDS\_POST**(cmds) [¶](#index-AC_005fCONFIG_005fCOMMANDS_005fPOST-1)
:   Execute the cmds right after creating `config.status`.

---

Next: [Configuring Other Packages in Subdirectories](#Subdirectories), Previous: [Running Arbitrary Configuration Commands](#Configuration-Commands), Up: [Initialization and Output Files](#Setup)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 4.11 Creating Configuration Links [¶](#Creating-Configuration-Links)

You may find it convenient to create links whose destinations depend upon
results of tests. One can use `AC_CONFIG_COMMANDS` but the
creation of relative symbolic links can be delicate when the package is
built in a directory different from the source directory.

Macro: **AC\_CONFIG\_LINKS**(dest:source…, [cmds], [init-cmds]) [¶](#index-AC_005fCONFIG_005fLINKS-1)
:   Make `AC_OUTPUT` link each of the existing files source to
    the corresponding link name dest. Makes a symbolic link if
    possible, otherwise a hard link if possible, otherwise a copy. The
    dest and source names should be relative to the top level
    source or build directory, and should not contain shell metacharacters.
    See [Special Characters in Output Variables](#Special-Chars-in-Variables).

    This macro is one of the instantiating
    macros; see [Performing Configuration Actions](#Configuration-Actions).

    For example, this call:

    ```
    AC_CONFIG_LINKS([host.h:config/$machine.h
                    object.h:config/$obj_format.h])
    ```

    creates in the current directory `host.h` as a link to
    `srcdir/config/$machine.h`, and `object.h` as a
    link to `srcdir/config/$obj_format.h`.

    The tempting value ‘`.`’ for dest is invalid: it makes it
    impossible for ‘`config.status`’ to guess the links to establish.

    One can then run:

    ```
    ./config.status host.h object.h
    ```

    to create the links.

---

Next: [Default Prefix](#Default-Prefix), Previous: [Creating Configuration Links](#Configuration-Links), Up: [Initialization and Output Files](#Setup)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 4.12 Configuring Other Packages in Subdirectories [¶](#Configuring-Other-Packages-in-Subdirectories)

In most situations, calling `AC_OUTPUT` is sufficient to produce
makefiles in subdirectories. However, `configure` scripts
that control more than one independent package can use
`AC_CONFIG_SUBDIRS` to run `configure` scripts for other
packages in subdirectories.

Macro: **AC\_CONFIG\_SUBDIRS**(dir …) [¶](#index-AC_005fCONFIG_005fSUBDIRS-1)
:   Make `AC_OUTPUT` run `configure` in each subdirectory
    dir in the given blank-or-newline-separated list. Each dir should
    be a literal, i.e., please do not use:

    ```
    if test "x$package_foo_enabled" = xyes; then
      my_subdirs="$my_subdirs foo"
    fi
    AC_CONFIG_SUBDIRS([$my_subdirs])
    ```

    because this prevents ‘`./configure --help=recursive`’ from
    displaying the options of the package `foo`. Instead, you should
    write:

    ```
    AS_IF([test "x$package_foo_enabled" = xyes],
      [AC_CONFIG_SUBDIRS([foo])])
    ```

    If a given dir is not found at `configure` run time, a
    warning is reported; if the subdirectory is optional, write:

    ```
    AS_IF([test -d "$srcdir/foo"],
      [AC_CONFIG_SUBDIRS([foo])])
    ```

    These examples use `AS_IF` instead of ordinary shell `if` to
    avoid problems that Autoconf has with macro calls in shell conditionals
    outside macro definitions. See [Common Shell Constructs](#Common-Shell-Constructs).

    If a given dir contains `configure.gnu`, it is run instead
    of `configure`. This is for packages that might use a
    non-Autoconf script `Configure`, which can’t be called through a
    wrapper `configure` since it would be the same file on
    case-insensitive file systems.

    The subdirectory `configure` scripts are given the same command
    line options that were given to this `configure` script, with minor
    changes if needed, which include:

    * adjusting a relative name for the cache file;
    * adjusting a relative name for the source directory;
    * propagating the current value of `$prefix`, including if it was
      defaulted, and if the default values of the top level and of the subdirectory
      `configure` differ.

    This macro also sets the output variable `subdirs` to the list of
    directories ‘`dir …`’. Make rules can use
    this variable to determine which subdirectories to recurse into.

    This macro may be called multiple times.

---

Previous: [Configuring Other Packages in Subdirectories](#Subdirectories), Up: [Initialization and Output Files](#Setup)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 4.13 Default Prefix [¶](#Default-Prefix-1)

By default, `configure` sets the prefix for files it installs to
`/usr/local`. The user of `configure` can select a different
prefix using the `--prefix` and `--exec-prefix` options.
There are two ways to change the default: when creating
`configure`, and when running it.

Some software packages might want to install in a directory other than
`/usr/local` by default. To accomplish that, use the
`AC_PREFIX_DEFAULT` macro.

Macro: **AC\_PREFIX\_DEFAULT**(prefix) [¶](#index-AC_005fPREFIX_005fDEFAULT-1)
:   Set the default installation prefix to prefix instead of
    `/usr/local`.

It may be convenient for users to have `configure` guess the
installation prefix from the location of a related program that they
have already installed. If you wish to do that, you can call
`AC_PREFIX_PROGRAM`.

Macro: **AC\_PREFIX\_PROGRAM**(program) [¶](#index-AC_005fPREFIX_005fPROGRAM-1)
:   If the user did not specify an installation prefix (using the
    `--prefix` option), guess a value for it by looking for
    program in `PATH`, the way the shell does. If program
    is found, set the prefix to the parent of the directory containing
    program, else default the prefix as described above
    (`/usr/local` or `AC_PREFIX_DEFAULT`). For example, if
    program is `gcc` and the `PATH` contains
    `/usr/local/gnu/bin/gcc`, set the prefix to `/usr/local/gnu`.

---

Next: [Writing Tests](#Writing-Tests), Previous: [Initialization and Output Files](#Setup), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 5 Existing Tests [¶](#Existing-Tests-1)

These macros test for particular system features that packages might
need or want to use. If you need to test for a kind of feature that
none of these macros check for, you can probably do it by calling
primitive test macros with appropriate arguments (see [Writing Tests](#Writing-Tests)).

These tests print messages telling the user which feature they’re
checking for, and what they find. They cache their results for future
`configure` runs (see [Caching Results](#Caching-Results)).

Some of these macros set output variables. See [Substitutions in Makefiles](#Makefile-Substitutions), for how to get their values. The phrase “define
name” is used below as a shorthand to mean “define the C
preprocessor symbol name to the value 1”. See [Defining C Preprocessor Symbols](#Defining-Symbols), for how to get those symbol definitions into your program.

* [Common Behavior](#Common-Behavior)
* [Alternative Programs](#Alternative-Programs)
* [Files](#Files)
* [Library Files](#Libraries)
* [Library Functions](#Library-Functions)
* [Header Files](#Header-Files)
* [Declarations](#Declarations)
* [Structures](#Structures)
* [Types](#Types)
* [Compilers and Preprocessors](#Compilers-and-Preprocessors)
* [System Services](#System-Services)
* [C and POSIX Variants](#C-and-POSIX-Variants)
* [Erlang Libraries](#Erlang-Libraries)

---

Next: [Alternative Programs](#Alternative-Programs), Up: [Existing Tests](#Existing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 5.1 Common Behavior [¶](#Common-Behavior-1)

Much effort has been expended to make Autoconf easy to learn. The most
obvious way to reach this goal is simply to enforce standard interfaces
and behaviors, avoiding exceptions as much as possible. Because of
history and inertia, unfortunately, there are still too many exceptions
in Autoconf; nevertheless, this section describes some of the common
rules.

* [Standard Symbols](#Standard-Symbols)
* [Default Includes](#Default-Includes)

---

Next: [Default Includes](#Default-Includes), Up: [Common Behavior](#Common-Behavior)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.1.1 Standard Symbols [¶](#Standard-Symbols-1)

All the generic macros that `AC_DEFINE` a symbol as a result of
their test transform their argument values to a standard alphabet.
First, argument is converted to upper case and any asterisks
(‘`*`’) are each converted to ‘`P`’. Any remaining characters
that are not alphanumeric are converted to underscores.

For instance,

```
AC_CHECK_TYPES([struct $Expensive*])
```

defines the symbol ‘`HAVE_STRUCT__EXPENSIVEP`’ if the check
succeeds.

---

Previous: [Standard Symbols](#Standard-Symbols), Up: [Common Behavior](#Common-Behavior)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 5.1.2 Default Includes [¶](#Default-Includes-1)

Test programs frequently need to include headers that may or may not be
available on the system whose features are being tested. Each test can
use all the preprocessor macros that have been `AC_DEFINE`d by
previous tests, so for example one may write

```
#include <time.h>
#ifdef HAVE_SYS_TIME_H
# include <sys/time.h>
#endif
```

if `sys/time.h` has already been tested for.

All hosted environments that are still of interest for portable code
provide all of the headers specified in C89 (as amended in 1995):
`assert.h`, `ctype.h`, `errno.h`, `float.h`,
`iso646.h`, `limits.h`, `locale.h`, `math.h`,
`setjmp.h`, `signal.h`, `stdarg.h`, `stddef.h`,
`stdio.h`, `stdlib.h`, `string.h`, `time.h`,
`wchar.h`, and `wctype.h`. Most programs can safely include
these headers unconditionally. A program not intended to be portable to
C89 can also safely include the C99-specified header `stdbool.h`.
Other headers, including headers from C99 and later revisions of the C
standard, might need to be tested for (see [Header Files](#Header-Files)) or their
bugs may need to be worked around (see [Gnulib](#Gnulib)).

If your program needs to be portable to a *freestanding*
environment, such as an embedded OS that doesn’t provide all of the
facilities of the C89 standard library, you may need to test for some of
the above headers as well. Note that many Autoconf macros internally
assume that the complete set of C89 headers are available.

Most generic macros use the following macro to provide a default set
of includes:

Macro: **AC\_INCLUDES\_DEFAULT**([include-directives]) [¶](#index-AC_005fINCLUDES_005fDEFAULT-1)
:   Expand to include-directives if present and nonempty, otherwise to:

    ```
    #include <stddef.h>
    #ifdef HAVE_STDIO_H
    # include <stdio.h>
    #endif
    #ifdef HAVE_STDLIB_H
    # include <stdlib.h>
    #endif
    #ifdef HAVE_STRING_H
    # include <string.h>
    #endif
    #ifdef HAVE_INTTYPES_H
    # include <inttypes.h>
    #endif
    #ifdef HAVE_STDINT_H
    # include <stdint.h>
    #endif
    #ifdef HAVE_STRINGS_H
    # include <strings.h>
    #endif
    #ifdef HAVE_SYS_TYPES_H
    # include <sys/types.h>
    #endif
    #ifdef HAVE_SYS_STAT_H
    # include <sys/stat.h>
    #endif
    #ifdef HAVE_UNISTD_H
    # include <unistd.h>
    #endif
    ```

    Using this macro without include-directives has the side effect of
    checking for `stdio.h`, `stdlib.h`, `string.h`,
    `inttypes.h`, `stdint.h`, `strings.h`,
    `sys/types.h`, `sys/stat.h`, and `unistd.h`, as if by
    `AC_CHECK_HEADERS_ONCE`. For backward compatibility, the macro
    `STDC_HEADERS` will be defined when both `stdlib.h` and
    `string.h` are available.

    **Portability Note:** It is safe for most programs to assume the
    presence of all of the headers required by the original 1990 C standard.
    `AC_INCLUDES_DEFAULT` checks for `stdio.h`, `stdlib.h`,
    and `string.h`, even though they are in that list, because they
    might not be available when compiling for a “freestanding environment”
    (in which most of the features of the C library are optional). You
    probably do not need to write ‘`#ifdef HAVE_STDIO_H`’ in your own
    code.

    `inttypes.h` and `stdint.h` were added to C in the 1999
    revision of the standard, and `strings.h`, `sys/types.h`,
    `sys/stat.h`, and `unistd.h` are POSIX extensions. You
    *should* guard uses of these headers with appropriate conditionals.

Macro: **AC\_CHECK\_INCLUDES\_DEFAULT** [¶](#index-AC_005fCHECK_005fINCLUDES_005fDEFAULT-1)
:   Check for all the headers that `AC_INCLUDES_DEFAULT` would check
    for as a side-effect, if this has not already happened.

    This macro mainly exists so that `autoupdate` can replace certain
    obsolete constructs with it. You should not need to use it yourself; in
    fact, it is likely to be safe to delete it from any script in which it
    appears. (`autoupdate` does not know whether preprocessor macros
    such as `HAVE_STDINT_H` are used in the program, nor whether they
    would get defined as a side-effect of other checks.)

---

Next: [Files](#Files), Previous: [Common Behavior](#Common-Behavior), Up: [Existing Tests](#Existing-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
