### 6.4 Autoconf macros supplied with Automake [¶](#Autoconf-macros-supplied-with-Automake)

Automake ships with several Autoconf macros that you can use from your
`configure.ac`. When you use one of them it will be included by
`aclocal` in `aclocal.m4`.

* [Public Macros](#Public-Macros)
* [Obsolete Macros](#Obsolete-Macros)
* [Private Macros](#Private-Macros)

---

Next: [Obsolete Macros](#Obsolete-Macros), Up: [Autoconf macros supplied with Automake](#Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 6.4.1 Public Macros [¶](#Public-Macros-1)

`AM_INIT_AUTOMAKE([options])` [¶](#index-AM_005fINIT_005fAUTOMAKE-1)
:   Runs many macros required for proper operation of the generated Makefiles.

    Today, `AM_INIT_AUTOMAKE` is called with a single argument: a
    space-separated list of Automake options that should be applied to
    every `Makefile.am` in the tree. The effect is as if
    each option were listed in `AUTOMAKE_OPTIONS` (see [Changing Automake’s Behavior](#Options)).

    This macro can also be called in another, *deprecated* form:
    `AM_INIT_AUTOMAKE(PACKAGE, VERSION, [NO-DEFINE])`. In this form,
    there are two required arguments: the package and the version number.
    This usage is mostly obsolete because the package and version
    can be obtained from Autoconf’s `AC_INIT` macro. However,
    differently from what happens for `AC_INIT` invocations, this
    `AM_INIT_AUTOMAKE` invocation supports shell variables’ expansions
    in the `PACKAGE` and `VERSION` arguments (which otherwise
    defaults, respectively, to the `PACKAGE_TARNAME` and
    `PACKAGE_VERSION` defined via the `AC_INIT` invocation;
    see [The `AC_INIT` macro](https://www.gnu.org/software/autoconf/manual/autoconf.html#AC_005fINIT) in The Autoconf Manual);
    and this can still be useful in some selected situations.
    Our hope is that future Autoconf versions will improve their support
    for package versions defined dynamically at configure runtime; when
    (and if) this happens, support for the two-args `AM_INIT_AUTOMAKE`
    invocation will likely be removed from Automake.

    If your `configure.ac` has:

    ```
    AC_INIT([src/foo.c])
    AM_INIT_AUTOMAKE([mumble], [1.5])
    ```

    you should modernize it as follows:

    ```
    AC_INIT([mumble], [1.5])
    AC_CONFIG_SRCDIR([src/foo.c])
    AM_INIT_AUTOMAKE
    ```

    Note that if you’re upgrading your `configure.ac` from an earlier
    version of Automake, it is not always correct to simply move the
    package and version arguments from `AM_INIT_AUTOMAKE` directly to
    `AC_INIT`, as in the example above. The first argument to
    `AC_INIT` should be the name of your package (e.g., ‘`GNU
    Automake`’), not the tarball name (e.g., ‘`automake`’) that you used
    to pass to `AM_INIT_AUTOMAKE`. Autoconf tries to derive a
    tarball name from the package name, which should work for most but not
    all package names. (If it doesn’t work for yours, you can use the
    four-argument form of `AC_INIT` to provide the tarball name
    explicitly).

    By default this macro `AC_DEFINE`’s `PACKAGE` and
    `VERSION`. This can be avoided by passing the `no-define`
    option (see [List of Automake options](#List-of-Automake-options)):

    ```
    AM_INIT_AUTOMAKE([no-define ...])
    ```

`AM_OPTIONAL_AUTOMAKE([options])` [¶](#index-AM_005fOPTIONAL_005fAUTOMAKE)
:   options is a whitespace-separated list of `dist-` option
    names, using the same spellings `AM_INIT_AUTOMAKE` accepts:
    `dist-bzip2`, `dist-bzip3`, `dist-xz`,
    `dist-lzip`, `dist-zstd`, `dist-zip`,
    `dist-shar`, and `dist-tarZ`. For each, ‘`make
    dist`’ builds the matching archive when its compressor or archiver is
    available and skips it otherwise, including when a tool present at
    `configure` time is gone by ‘`make dist`’ time; a missing
    listed tool does not make ‘`make dist`’ fail. An unknown
    option is reported as a `syntax` warning and ignored.

    This is meant for systems that intentionally lack some compression
    tools, such as minimal container or CI images, where installing
    `bzip3`, `lzip`, `zstd`, etc. only so that
    ‘`make distcheck`’ succeeds is undesirable. It avoids overriding
    `DIST_TARGETS` or `DIST_ARCHIVES` by hand.

    The macro does not pass the options to `automake`; it uses
    only the `distdir` target and `$(am__tar)`, present since
    Automake 1.10, so an older `automake` that does not recognize
    an option (such as `dist-bzip3`) is not a problem.

    ```
    AM_INIT_AUTOMAKE([-Wall foreign dist-bzip2 dist-xz dist-zip])
    AM_OPTIONAL_AUTOMAKE([dist-bzip3])
    ```

    Here `.tar.gz`, `.tar.bz2`, `.tar.xz`, and `.zip`
    are always built (and ‘`make dist`’ fails if their tools are
    missing), while `.tar.bz3` is built only if `bzip3` is
    installed.

    A given `dist-` option should appear in `AM_INIT_AUTOMAKE`
    or `AM_OPTIONAL_AUTOMAKE`, not both; listing it in
    `AM_INIT_AUTOMAKE` already makes that format mandatory. The
    macro may be called more than once; repeating an option is a no-op.

`AM_PATH_LISPDIR` [¶](#index-AM_005fPATH_005fLISPDIR)
:   Searches for the program `emacs`, and, if found, sets the
    output variable `lispdir` to the full path to Emacs’ site-lisp
    directory.

    Note that this test assumes the `emacs` found to be a version
    that supports Emacs Lisp (such as GNU Emacs or XEmacs). Other
    emacsen can cause this test to hang (some, like old versions of
    MicroEmacs, start up in interactive mode, requiring `C-x C-c` to
    exit, which is hardly obvious for a non-emacs user). In most cases,
    however, you should be able to use `C-c` to kill the test. In
    order to avoid problems, you can set `EMACS` to “no” in the
    environment, or use the `--with-lispdir` option to
    `configure` to explicitly set the correct path (if you’re sure
    you have an `emacs` that supports Emacs Lisp).

`AM_PROG_AR([act-if-fail])` [¶](#index-AM_005fPROG_005fAR)
:   You must use this macro when you use the archiver in your project, if
    you want support for unusual archivers such as Microsoft `lib`.
    The content of the optional argument is executed if the archiver
    interface is not recognized; the default action is to abort configure
    with an error message.

`AM_PROG_AS` [¶](#index-AM_005fPROG_005fAS)
:   Use this macro when you have assembly code in your project. This will
    choose the assembler for you (by default the C compiler) and set
    `CCAS`, and will also set `CCASFLAGS` if required.

`AM_PROG_CC_C_O` [¶](#index-AM_005fPROG_005fCC_005fC_005fO)
:   This is an obsolescent macro that checks that the C compiler supports
    the `-c` and `-o` options together. Note that, since
    Automake 1.14, the `AC_PROG_CC` is rewritten to implement such
    checks itself, and thus the explicit use of `AM_PROG_CC_C_O`
    should no longer be required.

`AM_PROG_LEX([options])` [¶](#index-AM_005fPROG_005fLEX)
:   Like `AC_PROG_LEX` (see [Particular
    Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual), but uses the
    `missing` script on systems that do not have `lex`.
    HP-UX 10 is one such system. options are passed directly to
    `AC_PROG_LEX`.

`AM_PROG_GCJ` [¶](#index-AM_005fPROG_005fGCJ)
:   This macro finds the `gcj` program or causes an error. It sets
    `GCJ` and `GCJFLAGS`. `gcj` is the Java front-end to the
    GNU Compiler Collection.

`AM_PROG_UPC([compiler-search-list])` [¶](#index-AM_005fPROG_005fUPC)
:   Find a compiler for Unified Parallel C and define the `UPC`
    variable. The default compiler-search-list is ‘`upcc upc`’.
    This macro will abort `configure` if no Unified Parallel C
    compiler is found.

`AM_MISSING_PROG(name, program)` [¶](#index-AM_005fMISSING_005fPROG)
:   Find a maintainer tool program and define the name
    environment variable with its location. If program is not
    detected, then name will instead invoke the `missing`
    script, in order to give useful advice to the user about the missing
    maintainer tool. See [`missing` and `AM_MAINTAINER_MODE`](#maintainer_002dmode), for more information on when
    the `missing` script is appropriate.

`AM_SILENT_RULES` [¶](#index-AM_005fSILENT_005fRULES)
:   Control the machinery for less verbose build output
    (see [How Automake can help in silencing Make](#Automake-Silent-Rules)).

`AM_WITH_DMALLOC` [¶](#index-AM_005fWITH_005fDMALLOC)
:   Add support for the [Dmalloc package](https://dmalloc.com/). If
    the user runs `configure` with `--with-dmalloc`, then
    define `WITH_DMALLOC` and add `-ldmalloc` to `LIBS`.

---

Next: [Private Macros](#Private-Macros), Previous: [Public Macros](#Public-Macros), Up: [Autoconf macros supplied with Automake](#Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 6.4.2 Obsolete Macros [¶](#Obsolete-Macros-1)

Although using some of the following macros was required in past
releases, you should not use any of them in new code. *All
these macros will be removed in the next major Automake version*;
if you are still using them, running `autoupdate` should
adjust your `configure.ac` automatically (see [Using `autoupdate` to Modernize
`configure.ac`](https://www.gnu.org/software/autoconf/manual/autoconf.html#autoupdate-Invocation) in The Autoconf Manual).
*Do it NOW!*

`AM_PROG_MKDIR_P` [¶](#index-AM_005fPROG_005fMKDIR_005fP)
:   From Automake 1.8 to 1.9.6 this macro used to define the output
    variable `mkdir_p` to one of `mkdir -p`, `install-sh
    -d`, or `mkinstalldirs`.

    Nowadays Autoconf provides a similar functionality with
    `AC_PROG_MKDIR_P` (see [Particular
    Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual), however this defines
    the output variable `MKDIR_P` instead. In case you are still
    using the `AM_PROG_MKDIR_P` macro in your `configure.ac`,
    or its provided variable `$(mkdir_p)` in your `Makefile.am`,
    you are advised to switch ASAP to the more modern Autoconf-provided
    interface instead; both the macro and the variable might be removed
    in a future major Automake release.

---

Previous: [Obsolete Macros](#Obsolete-Macros), Up: [Autoconf macros supplied with Automake](#Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 6.4.3 Private Macros [¶](#Private-Macros-1)

The following macros are private macros you should not call directly.
They are called by the other public macros when appropriate. Do not
rely on them, as they might be changed in a future version. Consider
them as implementation details; or better, do not consider them at all:
skip this section!

`_AM_DEPENDENCIES` [¶](#index-_005fAM_005fDEPENDENCIES)

`AM_SET_DEPDIR` [¶](#index-AM_005fSET_005fDEPDIR)

`AM_DEP_TRACK` [¶](#index-AM_005fDEP_005fTRACK)

`AM_OUTPUT_DEPENDENCY_COMMANDS` [¶](#index-AM_005fOUTPUT_005fDEPENDENCY_005fCOMMANDS)
:   These macros are used to implement Automake’s automatic dependency
    tracking scheme. They are called automatically by Automake when
    required, and there should be no need to invoke them manually.

`AM_MAKE_INCLUDE` [¶](#index-AM_005fMAKE_005fINCLUDE)
:   This macro is used to discover how the user’s `make` handles
    `include` statements. This macro is automatically invoked when
    needed; there should be no need to invoke it manually.

`AM_PROG_INSTALL_STRIP` [¶](#index-AM_005fPROG_005fINSTALL_005fSTRIP)
:   This is used to find a version of `install` that can be used to
    strip a program at installation time. This macro is automatically
    included when required.

`AM_SANITY_CHECK` [¶](#index-AM_005fSANITY_005fCHECK)
:   This checks to make sure that a file created in the build directory is
    newer than a file in the source directory. This can fail on systems
    where the clock is set incorrectly. This macro is automatically run
    from `AM_INIT_AUTOMAKE`.

---

Next: [Building Programs and Libraries](#Programs), Previous: [Scanning `configure.ac`, using `aclocal`](#configure), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 7 Directories [¶](#Directories-1)

For simple projects that distribute all files in the same directory
it is enough to have a single `Makefile.am` that builds
everything in place.

In larger projects, it is common to organize files in different
directories, in a tree. For example, there could be a directory
for the program’s source, one for the testsuite, and one for the
documentation; or, for very large projects, there could be one
directory per program, per library or per module.

The traditional approach is to build these subdirectories recursively,
employing *make recursion*: each directory contains its
own `Makefile`, and when `make` is run from the top-level
directory, it enters each subdirectory in turn, and invokes there a
new `make` instance to build the directory’s contents.

Because this approach is very widespread, Automake offers built-in
support for it. However, it is worth noting that the use of make
recursion has its own serious issues and drawbacks, and that it’s
well possible to have packages with a multi directory layout that
make little or no use of such recursion (examples of such packages
are GNU Bison and GNU Automake itself); see also the [An Alternative Approach to Subdirectories](#Alternative)
section below.

* [Recursing subdirectories](#Subdirectories)
* [Conditional Subdirectories](#Conditional-Subdirectories)
* [An Alternative Approach to Subdirectories](#Alternative)
* [Nesting Packages](#Subpackages)

---

Next: [Conditional Subdirectories](#Conditional-Subdirectories), Up: [Directories](#Directories)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 7.1 Recursing subdirectories [¶](#Recursing-subdirectories)

In packages using make recursion, the top level `Makefile.am` must
tell Automake which subdirectories are to be built. This is done via
the `SUBDIRS` variable.

The `SUBDIRS` variable holds a list of subdirectories in which
building of various sorts can occur. The rules for many targets
(e.g., `all`) in the generated `Makefile` will run commands
both locally and in all specified subdirectories. Note that the
directories listed in `SUBDIRS` are not required to contain
`Makefile.am`s; only `Makefile`s (after configuration).
This allows inclusion of libraries from packages that do not use
Automake (such as `gettext`; see also [Third-Party `Makefile`s](#Third_002dParty-Makefiles)).

In packages that use subdirectories, the top-level `Makefile.am` is
often very short. For instance, here is the `Makefile.am` from the
GNU Hello distribution:

```
EXTRA_DIST = BUGS ChangeLog.O README-alpha
SUBDIRS = doc intl po src tests
```

When Automake invokes `make` in a subdirectory, it uses the value
of the `MAKE` variable. It passes the value of the variable
`AM_MAKEFLAGS` to the `make` invocation; this can be set in
`Makefile.am` if there are flags you must always pass to
`make`.

The directories mentioned in `SUBDIRS` are usually direct
children of the current directory, each subdirectory containing its
own `Makefile.am` with a `SUBDIRS` pointing to deeper
subdirectories. Automake can be used to construct packages of
arbitrary depth this way.

By default, Automake generates `Makefiles` that work depth-first
in postfix order: the subdirectories are built before the current
directory. However, it is possible to change this ordering. You can
do this by putting ‘`.`’ into `SUBDIRS`. For instance,
putting ‘`.`’ first will cause a prefix ordering of
directories.

Using

```
SUBDIRS = lib src . test
```

will cause `lib/` to be built before `src/`, then the
current directory will be built, finally the `test/` directory
will be built. It is customary to arrange test directories to be
built after everything else since they are meant to test what has
been constructed.

In addition to the built-in recursive targets defined by Automake
(`all`, `check`, etc.), the developer can also define his
own recursive targets. That is done by passing the names of such
targets as arguments to the M4 macro `AM_EXTRA_RECURSIVE_TARGETS`
in `configure.ac`. Automake generates rules to handle the
recursion for such targets; and the developer can define real actions
for them by defining corresponding `-local` targets.

```
% cat configure.ac
AC_INIT([pkg-name], [1.0])
AM_INIT_AUTOMAKE
AM_EXTRA_RECURSIVE_TARGETS([foo])
AC_CONFIG_FILES([Makefile sub/Makefile sub/src/Makefile])
AC_OUTPUT
% cat Makefile.am
SUBDIRS = sub
foo-local:
        @echo This will be run by "make foo".
% cat sub/Makefile.am
SUBDIRS = src
% cat sub/src/Makefile.am
foo-local:
        @echo This too will be run by a "make foo" issued either in
        @echo the 'sub/src/' directory, the 'sub/' directory, or the
        @echo top-level directory.
```

---

Next: [An Alternative Approach to Subdirectories](#Alternative), Previous: [Recursing subdirectories](#Subdirectories), Up: [Directories](#Directories)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 7.2 Conditional Subdirectories [¶](#Conditional-Subdirectories-1)

It is possible to define the `SUBDIRS` variable conditionally if,
like in the case of GNU Inetutils, you want to only build a subset of
the entire package.

To illustrate how this works, let’s assume we have two directories,
`src/` and `opt/`. `src/` should always be built, but we
want to decide in `configure` whether `opt/` will be built
or not. (For this example we will assume that `opt/` should be
built when the variable ‘`$want_opt`’ was set to ‘`yes`’.)

Running `make` should thus recurse into `src/` always, and
then maybe in `opt/`.

However ‘`make dist`’ should always recurse into both `src/`
and `opt/`, because `opt/` should be distributed even if it
is not needed in the current configuration. This means
`opt/Makefile` should be created *unconditionally*.

There are two ways to set up a project like this. You can use Automake
conditionals (see [Conditionals](#Conditionals)) or use Autoconf `AC_SUBST`
variables (see [Setting Output
Variables](https://www.gnu.org/software/autoconf/manual/autoconf.html#Setting-Output-Variables) in The Autoconf Manual). Using Automake
conditionals is the preferred solution. Before we illustrate these
two possibilities, let’s introduce `DIST_SUBDIRS`.

* [`SUBDIRS` vs. `DIST_SUBDIRS`](#SUBDIRS-vs-DIST_005fSUBDIRS)
* [Subdirectories with `AM_CONDITIONAL`](#Subdirectories-with-AM_005fCONDITIONAL)
* [Subdirectories with `AC_SUBST`](#Subdirectories-with-AC_005fSUBST)
* [Unconfigured Subdirectories](#Unconfigured-Subdirectories)

---

Next: [Subdirectories with `AM_CONDITIONAL`](#Subdirectories-with-AM_005fCONDITIONAL), Up: [Conditional Subdirectories](#Conditional-Subdirectories)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 7.2.1 `SUBDIRS` vs. `DIST_SUBDIRS` [¶](#SUBDIRS-vs_002e-DIST_005fSUBDIRS)

Automake considers two sets of directories, defined by the variables
`SUBDIRS` and `DIST_SUBDIRS`.

`SUBDIRS` contains the subdirectories of the current directory
that must be built (see [Recursing subdirectories](#Subdirectories)). It must be defined
manually; Automake will never guess a directory is to be built. As we
will see in the next two sections, it is possible to define it
conditionally so that some directory will be omitted from the build.

`DIST_SUBDIRS` is used in rules that need to recurse in all
directories, even those that have been conditionally left out of the
build. Recall our example where we may not want to build subdirectory
`opt/`, but yet we want to distribute it? This is where
`DIST_SUBDIRS` comes into play: ‘`opt`’ may not appear in
`SUBDIRS`, but it must appear in `DIST_SUBDIRS`.

Precisely, `DIST_SUBDIRS` is used by ‘`make
maintainer-clean`’, ‘`make distclean`’ and ‘`make dist`’. All
other recursive rules use `SUBDIRS`.

If `SUBDIRS` is defined conditionally using Automake
conditionals, Automake will define `DIST_SUBDIRS` automatically
from the possible values of `SUBDIRS` in all conditions.

If `SUBDIRS` contains `AC_SUBST` variables,
`DIST_SUBDIRS` will not be defined correctly because Automake
does not know the possible values of these variables. In this case
`DIST_SUBDIRS` needs to be defined manually.

---

Next: [Subdirectories with `AC_SUBST`](#Subdirectories-with-AC_005fSUBST), Previous: [`SUBDIRS` vs. `DIST_SUBDIRS`](#SUBDIRS-vs-DIST_005fSUBDIRS), Up: [Conditional Subdirectories](#Conditional-Subdirectories)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 7.2.2 Subdirectories with `AM_CONDITIONAL` [¶](#Subdirectories-with-AM_005fCONDITIONAL-1)

`configure` should output the `Makefile` for each directory
and define a condition into which `opt/` should be built.

```
...
AM_CONDITIONAL([COND_OPT], [test "$want_opt" = yes])
AC_CONFIG_FILES([Makefile src/Makefile opt/Makefile])
...
```

Then `SUBDIRS` can be defined in the top-level `Makefile.am`
as follows.

```
if COND_OPT
  MAYBE_OPT = opt
endif
SUBDIRS = src $(MAYBE_OPT)
```

As you can see, running `make` will rightly recurse into
`src/` and maybe `opt/`.

As you can’t see, running ‘`make dist`’ will recurse into both
`src/` and `opt/` directories because ‘`make dist`’, unlike
‘`make all`’, doesn’t use the `SUBDIRS` variable. It uses the
`DIST_SUBDIRS` variable.

In this case Automake will define ‘`DIST_SUBDIRS = src opt`’
automatically because it knows that `MAYBE_OPT` can contain
‘`opt`’ in some condition.

---

Next: [Unconfigured Subdirectories](#Unconfigured-Subdirectories), Previous: [Subdirectories with `AM_CONDITIONAL`](#Subdirectories-with-AM_005fCONDITIONAL), Up: [Conditional Subdirectories](#Conditional-Subdirectories)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 7.2.3 Subdirectories with `AC_SUBST` [¶](#Subdirectories-with-AC_005fSUBST-1)

Another possibility is to define `MAYBE_OPT` from
`./configure` using `AC_SUBST`:

```
...
if test "$want_opt" = yes; then
  MAYBE_OPT=opt
else
  MAYBE_OPT=
fi
AC_SUBST([MAYBE_OPT])
AC_CONFIG_FILES([Makefile src/Makefile opt/Makefile])
...
```

In this case the top-level `Makefile.am` should look as follows.

```
SUBDIRS = src $(MAYBE_OPT)
DIST_SUBDIRS = src opt
```

The drawback is that since Automake cannot guess what the possible
values of `MAYBE_OPT` are, it is necessary to define
`DIST_SUBDIRS`.

---

Previous: [Subdirectories with `AC_SUBST`](#Subdirectories-with-AC_005fSUBST), Up: [Conditional Subdirectories](#Conditional-Subdirectories)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 7.2.4 Unconfigured Subdirectories [¶](#Unconfigured-Subdirectories-1)

The semantics of `DIST_SUBDIRS` are often misunderstood by some
users that try to *configure and build* subdirectories
conditionally. Here by configuring we mean creating the
`Makefile` (it might also involve running a nested
`configure` script: this is a costly operation that explains
why people want to do it conditionally, but only the `Makefile`
is relevant to the discussion).

The above examples all assume that every `Makefile` is created,
even in directories that are not going to be built. The simple reason
is that we want ‘`make dist`’ to distribute even the directories
that are not being built (e.g., platform-dependent code), hence
`make dist` must recurse into the subdirectory, hence this
directory must be configured and appear in `DIST_SUBDIRS`.

Building packages that do not configure every subdirectory is a tricky
business, and we do not recommend it to the novice as it is easy to
produce an incomplete tarball by mistake. We will not discuss this
topic in depth here, yet for the adventurous here are a few rules to
remember.

|  |
| --- |
| * `SUBDIRS` should always be a subset of `DIST_SUBDIRS`. It makes little sense to have a directory in `SUBDIRS` that   is not in `DIST_SUBDIRS`. Think of the former as a way to tell   which directories listed in the latter should be built. * Any directory listed in `DIST_SUBDIRS` and `SUBDIRS`   must be configured. That is, the `Makefile` must exist or the recursive `make`   rules will not be able to process the directory. * Any configured directory must be listed in `DIST_SUBDIRS`. This is so the cleaning rules remove the generated `Makefile`s.   It would be correct to see `DIST_SUBDIRS` as a variable that   lists all the directories that have been configured. |

In order to prevent recursion in some unconfigured directory you
must therefore ensure that this directory does not appear in
`DIST_SUBDIRS` (and `SUBDIRS`). For instance, if you define
`SUBDIRS` conditionally using `AC_SUBST` and do not define
`DIST_SUBDIRS` explicitly, it will be default to
‘`$(SUBDIRS)`’; another possibility is to force `DIST_SUBDIRS
= $(SUBDIRS)`.

Of course, directories that are omitted from `DIST_SUBDIRS` will
not be distributed unless you make other arrangements for this to
happen (for instance, always running ‘`make dist`’ in a
configuration where all directories are known to appear in
`DIST_SUBDIRS`; or writing a `dist-hook` target to
distribute these directories).

In a few packages, unconfigured directories are not even expected to
be distributed. Although these packages do not require the
aforementioned extra arrangements, there is another pitfall. If the
name of a directory appears in `SUBDIRS` or `DIST_SUBDIRS`,
`automake` will make sure the directory exists. Consequently
`automake` cannot be run on such a distribution when one
directory has been omitted. One way to avoid this check is to use the
`AC_SUBST` method to declare conditional directories; since
`automake` does not know the values of `AC_SUBST`
variables it cannot ensure the corresponding directory exists.

---

Next: [Nesting Packages](#Subpackages), Previous: [Conditional Subdirectories](#Conditional-Subdirectories), Up: [Directories](#Directories)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 7.3 An Alternative Approach to Subdirectories [¶](#An-Alternative-Approach-to-Subdirectories)

If you’ve ever read Peter Miller’s excellent paper, Recursive
Make Considered Harmful, the preceding sections on the use of make
recursion will probably come as unwelcome advice. For those who
haven’t read the paper, Miller’s main thesis is that recursive
`make` invocations are both slow and error-prone.

Automake is intended to have sufficient cross-directory support to
enable you to write a single `Makefile.am` for a complex
multi-directory package. (If it seems to be lacking, please report
the issue as usual.)

By default an installable file specified in a subdirectory will have its
directory name stripped before installation. For instance, in this
example, the header file will be installed as
`$(includedir)/stdio.h`:

```
include_HEADERS = inc/stdio.h
```

However, the ‘`nobase_`’ prefix can be used to circumvent this path
stripping. In this example, the header file will be installed as
`$(includedir)/sys/types.h`:

```
nobase_include_HEADERS = sys/types.h
```

‘`nobase_`’ should be specified first when used in conjunction with
either ‘`dist_`’ or ‘`nodist_`’ (see [Fine-grained Distribution Control](#Fine_002dgrained-Distribution-Control)). For instance:

```
nobase_dist_pkgdata_DATA = images/vortex.pgm sounds/whirl.ogg
```

Finally, note that a variable using the ‘`nobase_`’ prefix can
often be replaced by several variables, one for each destination
directory (see [The Uniform Naming Scheme](#Uniform)). For instance, the last example could be
rewritten as follows:

```
imagesdir = $(pkgdatadir)/images
soundsdir = $(pkgdatadir)/sounds
dist_images_DATA = images/vortex.pgm
dist_sounds_DATA = sounds/whirl.ogg
```

This latter syntax makes it possible to change one destination
directory without changing the layout of the source tree.

Currently, ‘`nobase_*_LTLIBRARIES`’ are the only exception to this
rule, in that there is no particular installation order guarantee for
an otherwise equivalent set of variables without ‘`nobase_`’ prefix.

---

Previous: [An Alternative Approach to Subdirectories](#Alternative), Up: [Directories](#Directories)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 7.4 Nesting Packages [¶](#Nesting-Packages)

In the GNU Build System, packages can be nested to arbitrary depth.
This means that a package can embed other packages with their own
`configure`, `Makefile`s, etc.

These other packages should just appear as subdirectories of their
parent package. They must be listed in `SUBDIRS` like other
ordinary directories. However the subpackage’s `Makefile`s
should be output by its own `configure` script, not by the
parent’s `configure`. This is achieved using the
`AC_CONFIG_SUBDIRS` Autoconf macro (see [Configuring Other Packages in Subdirectories](https://www.gnu.org/software/autoconf/manual/autoconf.html#Subdirectories) in The Autoconf Manual).

Here is an example package for an `arm` program that links with
a `hand` library that is a nested package in subdirectory
`hand/`.

`arm`’s `configure.ac`:

```
AC_INIT([arm], [1.0])
AC_CONFIG_AUX_DIR([.])
AM_INIT_AUTOMAKE
AC_PROG_CC
AC_CONFIG_FILES([Makefile])
# Call hand's ./configure script recursively.
AC_CONFIG_SUBDIRS([hand])
AC_OUTPUT
```

`arm`’s `Makefile.am`:

```
# Build the library in the hand subdirectory first.
SUBDIRS = hand

# Include hand's header when compiling this directory.
AM_CPPFLAGS = -I$(srcdir)/hand

bin_PROGRAMS = arm
arm_SOURCES = arm.c
# link with the hand library.
arm_LDADD = hand/libhand.a
```

Now here is `hand`’s `hand/configure.ac`:

```
AC_INIT([hand], [1.2])
AC_CONFIG_AUX_DIR([.])
AM_INIT_AUTOMAKE
AC_PROG_CC
AM_PROG_AR
AC_PROG_RANLIB
AC_CONFIG_FILES([Makefile])
AC_OUTPUT
```

and its `hand/Makefile.am`:

```
lib_LIBRARIES = libhand.a
libhand_a_SOURCES = hand.c
```

When ‘`make dist`’ is run from the top-level directory it will
create an archive `arm-1.0.tar.gz` that contains the `arm`
code as well as the `hand` subdirectory. This package can be
built and installed like any ordinary package, with the usual
‘`./configure && make && make install`’ sequence (the `hand`
subpackage will be built and installed by the process).

When ‘`make dist`’ is run from the hand directory, it will create a
self-contained `hand-1.2.tar.gz` archive. So although it appears
to be embedded in another package, it can still be used separately.

The purpose of the ‘`AC_CONFIG_AUX_DIR([.])`’ instruction is to
force Automake and Autoconf to search for auxiliary scripts in the
current directory. For instance, this means that there will be two
copies of `install-sh`: one in the top-level of the `arm`
package, and another one in the `hand/` subdirectory for the
`hand` package.

The historical default is to search for these auxiliary scripts in the
parent directory and the grandparent directory. So if the
‘`AC_CONFIG_AUX_DIR([.])`’ line was removed from
`hand/configure.ac`, that subpackage would share the auxiliary
script of the `arm` package. This may look like a gain in size
(a few kilobytes), but more importantly, it is a loss of modularity as
the `hand` subpackage is no longer self-contained (‘`make
dist`’ in the subdirectory will not work anymore).

Packages that do not use Automake need more work to be integrated this
way. See [Third-Party `Makefile`s](#Third_002dParty-Makefiles).

---

Next: [Other Derived Objects](#Other-Objects), Previous: [Directories](#Directories), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 8 Building Programs and Libraries [¶](#Building-Programs-and-Libraries)

A large part of Automake’s functionality is dedicated to making it easy
to build programs and libraries.

* [Building a program](#A-Program)
* [Building a library](#A-Library)
* [Building a Shared Library](#A-Shared-Library)
* [Program and Library Variables](#Program-and-Library-Variables)
* [Default `_SOURCES`](#Default-_005fSOURCES)
* [Special handling for `LIBOBJS` and `ALLOCA`](#LIBOBJS)
* [Variables used when building a program](#Program-Variables)
* [Yacc and Lex support](#Yacc-and-Lex)
* [C++ Support](#C_002b_002b-Support)
* [Objective C Support](#Objective-C-Support)
* [Objective C++ Support](#Objective-C_002b_002b-Support)
* [Unified Parallel C Support](#Unified-Parallel-C-Support)
* [Assembly Support](#Assembly-Support)
* [Fortran 77 Support](#Fortran-77-Support)
* [Fortran 9x Support](#Fortran-9x-Support)
* [Compiling Java sources using gcj](#Java-Support-with-gcj)
* [Vala Support](#Vala-Support)
* [Algol 68 Support](#Algol-68-Support)
* [Support for Other Languages](#Support-for-Other-Languages)
* [Automatic dependency tracking](#Dependencies)
* [Support for executable extensions](#EXEEXT)

---

Next: [Building a library](#A-Library), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.1 Building a program [¶](#Building-a-program)

In order to build a program, you need to tell Automake which sources
are part of it, and which libraries it should be linked with.

This section also covers conditional compilation of sources or
programs. Most of the comments about these also apply to libraries
(see [Building a library](#A-Library)) and libtool libraries (see [Building a Shared Library](#A-Shared-Library)).

* [Defining program sources](#Program-Sources)
* [Linking the program](#Linking)
* [Conditional compilation of sources](#Conditional-Sources)
* [Conditional compilation of programs](#Conditional-Programs)

---

Next: [Linking the program](#Linking), Up: [Building a program](#A-Program)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.1.1 Defining program sources [¶](#Defining-program-sources)

In a directory containing source that gets built into a program (as
opposed to a library or a script), the `PROGRAMS` primary is used.
Programs can be installed in `bindir`, `sbindir`,
`libexecdir`, `pkglibexecdir`, or not at all
(`noinst_`). They can also be built only for ‘`make check`’, in
which case the prefix is ‘`check_`’.

For instance:

```
bin_PROGRAMS = hello
```

In this simple case, the resulting `Makefile.in` will contain code
to generate a program named `hello`.

Associated with each program are several assisting variables that are
named after the program. These variables are all optional, and have
reasonable defaults. Each variable, its use, and default is spelled out
below; we use the “hello” example throughout.

The variable `hello_SOURCES` is used to specify which source files
get built into an executable:

```
hello_SOURCES = hello.c version.c getopt.c getopt1.c getopt.h system.h
```

This causes each mentioned `.c` file to be compiled into the
corresponding `.o`. Then all are linked to produce `hello`.

If `hello_SOURCES` is not specified, then it defaults to the single
file `hello.c` (see [Default `_SOURCES`](#Default-_005fSOURCES)).

Multiple programs can be built in a single directory. Multiple programs
can share a single source file, which must be listed in each
`_SOURCES` definition.

Header files listed in a `_SOURCES` definition will be included in
the distribution but otherwise ignored. In case it isn’t obvious, you
should not include the header file generated by `configure` in a
`_SOURCES` variable; this file should not be distributed. Lex
(`.l`) and Yacc (`.y`) files can also be listed; see [Yacc and Lex support](#Yacc-and-Lex).

---

Next: [Conditional compilation of sources](#Conditional-Sources), Previous: [Defining program sources](#Program-Sources), Up: [Building a program](#A-Program)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.1.2 Linking the program [¶](#Linking-the-program)

If you need to link against libraries that are not found by
`configure`, you can use `LDADD` to do so. This variable is
used to specify additional objects or libraries to link with.
Libraries to link with can be indicated either as file names or
through `-L` and `-l` options.

For other linker options (not `-L` or `-l`), `LDADD`
is inappropriate. You should use `AM_LDFLAGS` for this purpose.

Sometimes, multiple programs are built in one directory but do not share
the same link-time requirements. In this case, you can use the
`prog_LDADD` variable (where prog is the name of the
program as it appears in some `_PROGRAMS` variable, and usually
written in lowercase) to override `LDADD`. If this variable exists
for a given program, then that program is not linked using `LDADD`.

For instance, in GNU cpio, `pax`, `cpio` and `mt` are
linked against the library `libcpio.a`. However, `rmt` is
built in the same directory, and has no such link requirement. Also,
`mt` and `rmt` are only built on certain architectures. Here
is what cpio’s `src/Makefile.am` looks like (abridged):

```
bin_PROGRAMS = cpio pax $(MT)
libexec_PROGRAMS = $(RMT)
EXTRA_PROGRAMS = mt rmt

LDADD = ../lib/libcpio.a $(INTLLIBS)
rmt_LDADD =

cpio_SOURCES = ...
pax_SOURCES = ...
mt_SOURCES = ...
rmt_SOURCES = ...
```

`prog_LDADD` is inappropriate for passing program-specific
linker flags (except for `-l`, `-L`, `-dlopen` and
`-dlpreopen`). So, use the `prog_LDFLAGS` variable for
this purpose.

It is also occasionally useful to have a program depend on some other
target that is not in fact part of that program. This can be done
using either the `prog_DEPENDENCIES` or the
`EXTRA_prog_DEPENDENCIES` variable. Each program depends on
the contents both variables, but no further interpretation is done.

Since these dependencies are associated to the link rule used to
create the programs they should normally list files used by the link
command. That is `*.$(OBJEXT)`, `*.a`, or `*.la`
files. In rare cases you may need to add other kinds of files such as
linker scripts, but *listing a source file in
`_DEPENDENCIES` is wrong*. If some source file needs to be built
before all the components of a program are built, consider using the
`BUILT_SOURCES` variable instead (see [Built Sources](#Sources)).

If `prog_DEPENDENCIES` is not supplied, it is computed by
Automake. The automatically-assigned value is the contents of
`prog_LDADD`, with most configure substitutions, `-l`,
`-L`, `-dlopen` and `-dlpreopen` options removed. The
configure substitutions that are left in are only ‘`$(LIBOBJS)`’ and
‘`$(ALLOCA)`’; these are left because it is known that they will not
cause an invalid value for `prog_DEPENDENCIES` to be
generated.

[Conditional compilation of sources](#Conditional-Sources) shows a situation where `_DEPENDENCIES`
may be used.

The `EXTRA_prog_DEPENDENCIES` may be useful for cases where
you merely want to augment the `automake`-generated
`prog_DEPENDENCIES` rather than replacing it.

We recommend that you avoid using `-l` options in `LDADD`
or `prog_LDADD` when referring to libraries built by your
package. Instead, write the file name of the library explicitly as in
the above `cpio` example. Use `-l` only to list
third-party libraries. If you follow this rule, the default value of
`prog_DEPENDENCIES` will list all your local libraries and
omit the other ones.

---

Next: [Conditional compilation of programs](#Conditional-Programs), Previous: [Linking the program](#Linking), Up: [Building a program](#A-Program)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.1.3 Conditional compilation of sources [¶](#Conditional-compilation-of-sources)

You can’t put a configure substitution (e.g., ‘`@FOO@`’ or
‘`$(FOO)`’ where `FOO` is defined via `AC_SUBST`) into a
`_SOURCES` variable. The reason for this is a bit hard to
explain, but suffice to say that it simply won’t work. Automake will
give an error if you try to do this.

Fortunately there are two other ways to achieve the same result. One is
to use configure substitutions in `_LDADD` variables, the other is
to use an Automake conditional.

#### Conditional Compilation using `_LDADD` Substitutions [¶](#Conditional-Compilation-using-_005fLDADD-Substitutions)

Automake must know all the source files that could possibly go into a
program, even if not all the files are built in every circumstance. Any
files that are only conditionally built should be listed in the
appropriate `EXTRA_` variable. For instance, if
`hello-linux.c` or `hello-generic.c` were conditionally included
in `hello`, the `Makefile.am` would contain:

```
bin_PROGRAMS = hello
hello_SOURCES = hello-common.c
EXTRA_hello_SOURCES = hello-linux.c hello-generic.c
hello_LDADD = $(HELLO_SYSTEM)
hello_DEPENDENCIES = $(HELLO_SYSTEM)
```

You can then set up the ‘`$(HELLO_SYSTEM)`’ substitution from
`configure.ac`:

```
...
case $host in
  *linux*) HELLO_SYSTEM='hello-linux.$(OBJEXT)' ;;
  *)       HELLO_SYSTEM='hello-generic.$(OBJEXT)' ;;
esac
AC_SUBST([HELLO_SYSTEM])
...
```

In this case, the variable `HELLO_SYSTEM` should be replaced by
either `hello-linux.o` or `hello-generic.o`, and added to
both `hello_DEPENDENCIES` and `hello_LDADD` in order to be
built and linked in.

#### Conditional Compilation using Automake Conditionals [¶](#Conditional-Compilation-using-Automake-Conditionals)

An often simpler way to compile source files conditionally is to use
Automake conditionals. For instance, you could use this
`Makefile.am` construct to build the same `hello` example:

```
bin_PROGRAMS = hello
if LINUX
hello_SOURCES = hello-linux.c hello-common.c
else
hello_SOURCES = hello-generic.c hello-common.c
endif
```

In this case, `configure.ac` should set up the `LINUX`
conditional using `AM_CONDITIONAL` (see [Conditionals](#Conditionals)).

When using conditionals like this you don’t need to use the
`EXTRA_` variable, because Automake will examine the contents of
each variable to construct the complete list of source files.

If your program uses a lot of files, you will probably prefer a
conditional ‘`+=`’.

```
bin_PROGRAMS = hello
hello_SOURCES = hello-common.c
if LINUX
hello_SOURCES += hello-linux.c
else
hello_SOURCES += hello-generic.c
endif
```

---

Previous: [Conditional compilation of sources](#Conditional-Sources), Up: [Building a program](#A-Program)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.1.4 Conditional compilation of programs [¶](#Conditional-compilation-of-programs)

Sometimes it is useful to determine the programs that are to be built
at configure time. For instance, GNU `cpio` only builds
`mt` and `rmt` under special circumstances. The means to
achieve conditional compilation of programs are the same you can use
to compile source files conditionally: substitutions or conditionals.

#### Conditional Programs using `configure` Substitutions [¶](#Conditional-Programs-using-configure-Substitutions)

In this case, you must notify Automake of all the programs that can
possibly be built, but at the same time cause the generated
`Makefile.in` to use the programs specified by `configure`.
This is done by having `configure` substitute values into each
`_PROGRAMS` definition, while listing all optionally built programs
in `EXTRA_PROGRAMS`.

```
bin_PROGRAMS = cpio pax $(MT)
libexec_PROGRAMS = $(RMT)
EXTRA_PROGRAMS = mt rmt
```

As explained in [Support for executable extensions](#EXEEXT), Automake will rewrite
`bin_PROGRAMS`, `libexec_PROGRAMS`, and
`EXTRA_PROGRAMS`, appending ‘`$(EXEEXT)`’ to each binary.
Obviously it cannot rewrite values obtained at run-time through
`configure` substitutions, therefore you should take care of
appending ‘`$(EXEEXT)`’ yourself, as in ‘`AC_SUBST([MT],
['mt${EXEEXT}'])`’.

#### Conditional Programs using Automake Conditionals [¶](#Conditional-Programs-using-Automake-Conditionals)

You can also use Automake conditionals (see [Conditionals](#Conditionals)) to
select programs to be built. In this case you don’t have to worry
about ‘`$(EXEEXT)`’ or `EXTRA_PROGRAMS`.

```
bin_PROGRAMS = cpio pax
if WANT_MT
  bin_PROGRAMS += mt
endif
if WANT_RMT
  libexec_PROGRAMS = rmt
endif
```

---

Next: [Building a Shared Library](#A-Shared-Library), Previous: [Building a program](#A-Program), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.2 Building a library [¶](#Building-a-library)

Building a library is much like building a program. In this case, the
name of the primary is `LIBRARIES`. Libraries can be installed in
`libdir` or `pkglibdir`.

See [Building a Shared Library](#A-Shared-Library), for information on how to build shared
libraries using Libtool and the `LTLIBRARIES` primary.

Each `_LIBRARIES` variable is a list of the libraries to be built.
For instance, to create a library named `libcpio.a`, but not install
it, you would write:

```
noinst_LIBRARIES = libcpio.a
libcpio_a_SOURCES = ...
```

The sources that go into a library are determined exactly as they are
for programs, via the `_SOURCES` variables. Note that the library
name is canonicalized (see [How derived variables are named](#Canonicalization)), so the `_SOURCES`
variable corresponding to `libcpio.a` is ‘`libcpio_a_SOURCES`’,
not ‘`libcpio.a_SOURCES`’.

Extra objects can be added to a library using the
`library_LIBADD` variable. This should be used for objects
determined by `configure`. Again from `cpio`:

```
libcpio_a_LIBADD = $(LIBOBJS) $(ALLOCA)
```

In addition, sources for extra objects that will not exist until
configure-time must be added to the `BUILT_SOURCES` variable
(see [Built Sources](#Sources)).

Building a static library is done by compiling all object files, then
by invoking ‘`$(AR) $(ARFLAGS)`’ followed by the name of the
library and the list of objects, and finally by calling
‘`$(RANLIB)`’ on that library. You should call
`AC_PROG_RANLIB` from your `configure.ac` to define
`RANLIB` (Automake will complain otherwise). You should also
call `AM_PROG_AR` to define `AR`, in order to support unusual
archivers such as Microsoft lib. `ARFLAGS` will default to
`cr`; you can override this variable by setting it in your
`Makefile.am` or by `AC_SUBST`ing it from your
`configure.ac`. You can override the `AR` variable by
defining a per-library `maude_AR` variable (see [Program and Library Variables](#Program-and-Library-Variables)).

Be careful when selecting library components conditionally. Because
building an empty library is not portable, you should ensure that any
library always contains at least one object.

To use a static library when building a program, add it to
`LDADD` for this program. In the following example, the program
`cpio` is statically linked with the library `libcpio.a`.

```
noinst_LIBRARIES = libcpio.a
libcpio_a_SOURCES = ...

bin_PROGRAMS = cpio
cpio_SOURCES = cpio.c ...
cpio_LDADD = libcpio.a
```

---

Next: [Program and Library Variables](#Program-and-Library-Variables), Previous: [Building a library](#A-Library), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.3 Building a Shared Library [¶](#Building-a-Shared-Library)

Building shared libraries portably is a relatively complex matter.
For this reason, GNU Libtool (see [Introduction](https://www.gnu.org/software/libtool/manual/libtool.html#Top) in The
Libtool Manual) was created to help build shared libraries in a
platform-independent way.

* [The Libtool Concept](#Libtool-Concept)
* [Building Libtool Libraries](#Libtool-Libraries)
* [Building Libtool Libraries Conditionally](#Conditional-Libtool-Libraries)
* [Libtool Libraries with Conditional Sources](#Conditional-Libtool-Sources)
* [Libtool Convenience Libraries](#Libtool-Convenience-Libraries)
* [Libtool Modules](#Libtool-Modules)
* [`_LIBADD`, `_LDFLAGS`, and `_LIBTOOLFLAGS`](#Libtool-Flags)
* [`LTLIBOBJS` and `LTALLOCA`](#LTLIBOBJS)
* [Common Issues Related to Libtool’s Use](#Libtool-Issues)

---

Next: [Building Libtool Libraries](#Libtool-Libraries), Up: [Building a Shared Library](#A-Shared-Library)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.1 The Libtool Concept [¶](#The-Libtool-Concept)

Libtool abstracts shared and static libraries into a unified concept
henceforth called *libtool libraries*. Libtool libraries are
files using the `.la` suffix, and can designate a static library,
a shared library, or maybe both. Their exact nature cannot be
determined until `./configure` is run: not all platforms support
all kinds of libraries, and users can explicitly select which
libraries should be built. (However the package’s maintainers can
tune the default; see [The `LT_INIT`
macro](https://www.gnu.org/software/libtool/manual/libtool.html#LT_005fINIT) in The Libtool Manual.)

Because object files for shared and static libraries must be compiled
differently, Libtool is also used during compilation. Object files
built by Libtool are called *libtool objects*: these are files
using the `.lo` suffix. Libtool libraries are built from these
libtool objects.

You should not assume anything about the structure of `.la` or
`.lo` files and how Libtool constructs them; this is Libtool’s
concern. However, the existence of these files matters, because they
are used as targets and dependencies in `Makefile` rules when
building Libtool libraries. There are situations where you may have
to refer to these, for instance when expressing dependencies for
building source files conditionally (see [Libtool Libraries with Conditional Sources](#Conditional-Libtool-Sources)).

People considering writing a plug-in system, with dynamically loaded
modules, should look into `libltdl`: Libtool’s dlopening library
(see [Using libltdl](https://www.gnu.org/software/libtool/manual/libtool.html#Using-libltdl) in The Libtool Manual).
This offers a portable dlopening facility to load Libtool libraries
dynamically, and can also achieve static linking where unavoidable.

Before we discuss how to use Libtool with Automake in detail, it
should be noted that the Libtool manual also has a section about how
to use Automake with Libtool (see [Using Automake
with Libtool](https://www.gnu.org/software/libtool/manual/libtool.html#Using-Automake) in The Libtool Manual).

---

Next: [Building Libtool Libraries Conditionally](#Conditional-Libtool-Libraries), Previous: [The Libtool Concept](#Libtool-Concept), Up: [Building a Shared Library](#A-Shared-Library)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.2 Building Libtool Libraries [¶](#Building-Libtool-Libraries)

Automake uses Libtool to build libraries declared with the
`LTLIBRARIES` primary. Each `_LTLIBRARIES` variable is a
list of Libtool libraries to build. For instance, to create a libtool
library named `libgettext.la`, and install it in `libdir`,
write:

```
lib_LTLIBRARIES = libgettext.la
libgettext_la_SOURCES = gettext.c gettext.h ...
```

Automake predefines the variable `pkglibdir`, so you can use
`pkglib_LTLIBRARIES` to install libraries in
‘`$(libdir)/@PACKAGE@/`’.

If `gettext.h` is a public header file that needs to be installed
in order for people to use the library, it should be declared using a
`_HEADERS` variable, not in `libgettext_la_SOURCES`.
Headers listed in the latter should be internal headers that are not
part of the public interface.

```
lib_LTLIBRARIES = libgettext.la
libgettext_la_SOURCES = gettext.c ...
include_HEADERS = gettext.h ...
```

A package can build and install such a library along with other
programs that use it. This dependency should be specified using
`LDADD`. The following example builds a program named
`hello` that is linked with `libgettext.la`.

```
lib_LTLIBRARIES = libgettext.la
libgettext_la_SOURCES = gettext.c ...

bin_PROGRAMS = hello
hello_SOURCES = hello.c ...
hello_LDADD = libgettext.la
```

Whether `hello` is statically or dynamically linked with
`libgettext.la` is not yet known: this will depend on the
configuration of Libtool and the capabilities of the host.

---

Next: [Libtool Libraries with Conditional Sources](#Conditional-Libtool-Sources), Previous: [Building Libtool Libraries](#Libtool-Libraries), Up: [Building a Shared Library](#A-Shared-Library)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.3 Building Libtool Libraries Conditionally [¶](#Building-Libtool-Libraries-Conditionally)

Like conditional programs (see [Conditional compilation of programs](#Conditional-Programs)), there are
two main ways to build conditional libraries: using Automake
conditionals or using Autoconf `AC_SUBST`itutions.

The important implementation detail you have to be aware of is that
the place where a library will be installed matters to Libtool: it
needs to be indicated *at link-time* using the `-rpath`
option.

For libraries whose destination directory is known when Automake runs,
Automake will automatically supply the appropriate `-rpath`
option to `libtool`. This is the case for libraries listed
explicitly in some installable `_LTLIBRARIES` variables such as
`lib_LTLIBRARIES`.

However, for libraries determined at configure time (and thus
mentioned in `EXTRA_LTLIBRARIES`), Automake does not know the
final installation directory. For such libraries you must add the
`-rpath` option to the appropriate `_LDFLAGS` variable by
hand.

The examples below illustrate the differences between these two methods.

Here is an example where `WANTEDLIBS` is an `AC_SUBST`ed
variable set at `./configure`-time to either `libfoo.la`,
`libbar.la`, both, or none. Although ‘`$(WANTEDLIBS)`’
appears in the `lib_LTLIBRARIES`, Automake cannot guess it
relates to `libfoo.la` or `libbar.la` at the time it creates
the link rule for these two libraries. Therefore the `-rpath`
argument must be explicitly supplied.

```
EXTRA_LTLIBRARIES = libfoo.la libbar.la
lib_LTLIBRARIES = $(WANTEDLIBS)
libfoo_la_SOURCES = foo.c ...
libfoo_la_LDFLAGS = -rpath '$(libdir)'
libbar_la_SOURCES = bar.c ...
libbar_la_LDFLAGS = -rpath '$(libdir)'
```

Here is how the same `Makefile.am` would look using Automake
conditionals named `WANT_LIBFOO` and `WANT_LIBBAR`. Now
Automake is able to compute the `-rpath` setting itself, because
it’s clear that both libraries will end up in ‘`$(libdir)`’ if they
are installed.

```
lib_LTLIBRARIES =
if WANT_LIBFOO
lib_LTLIBRARIES += libfoo.la
endif
if WANT_LIBBAR
lib_LTLIBRARIES += libbar.la
endif
libfoo_la_SOURCES = foo.c ...
libbar_la_SOURCES = bar.c ...
```

---

Next: [Libtool Convenience Libraries](#Libtool-Convenience-Libraries), Previous: [Building Libtool Libraries Conditionally](#Conditional-Libtool-Libraries), Up: [Building a Shared Library](#A-Shared-Library)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.4 Libtool Libraries with Conditional Sources [¶](#Libtool-Libraries-with-Conditional-Sources)

Conditional compilation of sources in a library can be achieved in the
same way as conditional compilation of sources in a program
(see [Conditional compilation of sources](#Conditional-Sources)). The only difference is that
`_LIBADD` should be used instead of `_LDADD` and that it
should mention libtool objects (`.lo` files).

So, to mimic the `hello` example from [Conditional compilation of sources](#Conditional-Sources),
we could build a `libhello.la` library using either
`hello-linux.c` or `hello-generic.c` with the following
`Makefile.am`.

```
lib_LTLIBRARIES = libhello.la
libhello_la_SOURCES = hello-common.c
EXTRA_libhello_la_SOURCES = hello-linux.c hello-generic.c
libhello_la_LIBADD = $(HELLO_SYSTEM)
libhello_la_DEPENDENCIES = $(HELLO_SYSTEM)
```

And make sure `configure` defines `HELLO_SYSTEM` as
either `hello-linux.lo` or `hello-generic.lo`.

Or we could simply use an Automake conditional as follows.

```
lib_LTLIBRARIES = libhello.la
libhello_la_SOURCES = hello-common.c
if LINUX
libhello_la_SOURCES += hello-linux.c
else
libhello_la_SOURCES += hello-generic.c
endif
```

---

Next: [Libtool Modules](#Libtool-Modules), Previous: [Libtool Libraries with Conditional Sources](#Conditional-Libtool-Sources), Up: [Building a Shared Library](#A-Shared-Library)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.5 Libtool Convenience Libraries [¶](#Libtool-Convenience-Libraries-1)

Sometimes you want to build Libtool libraries that should not be
installed. These are called *libtool convenience libraries* and
are typically used to encapsulate many sublibraries, later gathered
into one big installed library.

Libtool convenience libraries are declared by directory-less variables
such as `noinst_LTLIBRARIES`, `check_LTLIBRARIES`, or even
`EXTRA_LTLIBRARIES`. Unlike installed Libtool libraries they do
not need an `-rpath` flag at link time (this is in fact the only
difference).

Convenience libraries listed in `noinst_LTLIBRARIES` are always
built. Those listed in `check_LTLIBRARIES` are built only upon
‘`make check`’. Finally, libraries listed in
`EXTRA_LTLIBRARIES` are never built explicitly: Automake outputs
rules to build them, but if the library does not appear as a `Makefile`
dependency anywhere it won’t be built (this is why
`EXTRA_LTLIBRARIES` is used for conditional compilation).

Here is a sample setup merging Libtool convenience libraries from
subdirectories into one main `libtop.la` library.

```
# -- Top-level Makefile.am --
SUBDIRS = sub1 sub2 ...
lib_LTLIBRARIES = libtop.la
libtop_la_SOURCES =
libtop_la_LIBADD = \
  sub1/libsub1.la \
  sub2/libsub2.la \
  ...

# -- sub1/Makefile.am --
noinst_LTLIBRARIES = libsub1.la
libsub1_la_SOURCES = ...

# -- sub2/Makefile.am --
# showing nested convenience libraries
SUBDIRS = sub2.1 sub2.2 ...
noinst_LTLIBRARIES = libsub2.la
libsub2_la_SOURCES =
libsub2_la_LIBADD = \
  sub21/libsub21.la \
  sub22/libsub22.la \
  ...
```

When using such a setup, beware that `automake` will assume
`libtop.la` is to be linked with the C linker. This is because
`libtop_la_SOURCES` is empty, so `automake` picks C as
default language. If `libtop_la_SOURCES` was not empty,
`automake` would select the linker as explained in [How the Linker is Chosen](#How-the-Linker-is-Chosen).

If one of the sublibraries contains non-C source, it is important that
the appropriate linker be chosen. One way to achieve this is to
pretend that there is such a non-C file among the sources of the
library, thus forcing `automake` to select the appropriate
linker. Here is the top-level `Makefile` of our example updated
to force C++ linking.

```
SUBDIRS = sub1 sub2 ...
lib_LTLIBRARIES = libtop.la
libtop_la_SOURCES =
# Dummy C++ source to cause C++ linking.
nodist_EXTRA_libtop_la_SOURCES = dummy.cxx
libtop_la_LIBADD = \
  sub1/libsub1.la \
  sub2/libsub2.la \
  ...
```

‘`EXTRA_*_SOURCES`’ variables are used to keep track of source
files that might be compiled (this is mostly useful when doing
conditional compilation using `AC_SUBST`; see [Libtool Libraries with Conditional Sources](#Conditional-Libtool-Sources)), and the `nodist_` prefix means the listed
sources are not to be distributed (see [Program and Library Variables](#Program-and-Library-Variables)). In effect the file `dummy.cxx` does not need to
exist in the source tree. Of course if you have some real source file
to list in `libtop_la_SOURCES` there is no point in cheating with
`nodist_EXTRA_libtop_la_SOURCES`.

---

Next: [`_LIBADD`, `_LDFLAGS`, and `_LIBTOOLFLAGS`](#Libtool-Flags), Previous: [Libtool Convenience Libraries](#Libtool-Convenience-Libraries), Up: [Building a Shared Library](#A-Shared-Library)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.6 Libtool Modules [¶](#Libtool-Modules-1)

These are Libtool libraries meant to be dlopened. They are
indicated to `libtool` by passing `-module` at link-time.

```
pkglib_LTLIBRARIES = mymodule.la
mymodule_la_SOURCES = doit.c
mymodule_la_LDFLAGS = -module
```

Ordinarily, Automake requires that a library’s name start with
`lib`. However, when building a dynamically loadable module you
might wish to use a “nonstandard” name. Automake will not complain
about such nonstandard names if it knows the library being built is a
libtool module, i.e., if `-module` explicitly appears in the
library’s `_LDFLAGS` variable (or in the common `AM_LDFLAGS`
variable when no per-library `_LDFLAGS` variable is defined).

As always, `AC_SUBST` variables are black boxes to Automake since
their values are not yet known when `automake` is run.
Therefore if `-module` is set via such a variable, Automake
cannot notice it and will proceed as if the library was an ordinary
libtool library, with strict naming.

If `mymodule_la_SOURCES` is not specified, then it defaults to
the single file `mymodule.c` (see [Default `_SOURCES`](#Default-_005fSOURCES)).

---

Next: [`LTLIBOBJS` and `LTALLOCA`](#LTLIBOBJS), Previous: [Libtool Modules](#Libtool-Modules), Up: [Building a Shared Library](#A-Shared-Library)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.7 `_LIBADD`, `_LDFLAGS`, and `_LIBTOOLFLAGS` [¶](#g_t_005fLIBADD_002c-_005fLDFLAGS_002c-and-_005fLIBTOOLFLAGS)

As shown in previous sections, the ‘`library_LIBADD`’
variable should be used to list extra libtool objects (`.lo`
files) or libtool libraries (`.la`) to add to library.

The ‘`library_LDFLAGS`’ variable is the place to list
additional `libtool` linking flags, such as
`-version-info`, `-static`, and a lot more. See [Link
mode](https://www.gnu.org/software/libtool/manual/libtool.html#Link-mode) in The Libtool Manual.

The `libtool` command has two kinds of options: mode-specific
options and generic options. Mode-specific options such as the
aforementioned linking flags should be lumped with the other flags
passed to the tool invoked by `libtool` (hence the use of
‘`library_LDFLAGS`’ for libtool linking flags). Generic
options include `--tag=tag` and `--silent`
(see [Invoking `libtool`](https://www.gnu.org/software/libtool/manual/libtool.html#Invoking-libtool) in The
Libtool Manual for more options). They should appear before the mode
selection on the command line; in `Makefile.am`s they should
be listed in the ‘`library_LIBTOOLFLAGS`’ variable.

If ‘`library_LIBTOOLFLAGS`’ is not defined, then the variable
`AM_LIBTOOLFLAGS` is used instead.

These flags are passed to `libtool` after the
`--tag=tag` option computed by Automake (if any), so
‘`library_LIBTOOLFLAGS`’ (or `AM_LIBTOOLFLAGS`) is a
good place to override or supplement the `--tag=tag`
setting.

The libtool rules also use a `LIBTOOLFLAGS` variable that should
not be set in `Makefile.am`: this is a user variable (see [Flag Variables Ordering](#Flag-Variables-Ordering)). It allows users to run ‘`make
LIBTOOLFLAGS=--silent`’, for instance. Note that the verbosity of
`libtool` can also be influenced by the Automake support
for silent rules (see [How Automake can help in silencing Make](#Automake-Silent-Rules)).

---

Next: [Common Issues Related to Libtool’s Use](#Libtool-Issues), Previous: [`_LIBADD`, `_LDFLAGS`, and `_LIBTOOLFLAGS`](#Libtool-Flags), Up: [Building a Shared Library](#A-Shared-Library)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.8 `LTLIBOBJS` and `LTALLOCA` [¶](#LTLIBOBJS-and-LTALLOCA)

Where an ordinary library might include ‘`$(LIBOBJS)`’ or
‘`$(ALLOCA)`’ (see [Special handling for `LIBOBJS` and `ALLOCA`](#LIBOBJS)), a libtool library must use
‘`$(LTLIBOBJS)`’ or ‘`$(LTALLOCA)`’. This is required because
the object files that libtool operates on do not necessarily end in
`.o`.

Nowadays, the computation of `LTLIBOBJS` from `LIBOBJS` is
performed automatically by Autoconf (see [`AC_LIBOBJ` vs. `LIBOBJS`](https://www.gnu.org/software/autoconf/manual/autoconf.html#AC_005fLIBOBJ-vs-LIBOBJS) in The Autoconf Manual).

---

Previous: [`LTLIBOBJS` and `LTALLOCA`](#LTLIBOBJS), Up: [Building a Shared Library](#A-Shared-Library)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.9 Common Issues Related to Libtool’s Use [¶](#Common-Issues-Related-to-Libtool_0027s-Use)

* [Error: ‘`required file './ltmain.sh' not found`’—`libtoolize` needed](#Error-required-file-ltmain_002esh-not-found)
* [Error: ‘`Libtool library used but 'LIBTOOL' is undefined`’—`libtoolize` redux](#Libtool-library-used-but-LIBTOOL-is-undefined)
* [Objects ‘`created with both libtool and without`’](#Objects-created-both-with-libtool-and-without)

---

Next: [Error: ‘`Libtool library used but 'LIBTOOL' is undefined`’—`libtoolize` redux](#Libtool-library-used-but-LIBTOOL-is-undefined), Up: [Common Issues Related to Libtool’s Use](#Libtool-Issues)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.9.1 Error: ‘`required file './ltmain.sh' not found`’—`libtoolize` needed [¶](#Error_003a-required-file-_0027_002e_002fltmain_002esh_0027-not-found_002d_002d_002dlibtoolize-needed)

Libtool comes with a tool called `libtoolize` that will
install Libtool’s supporting files into a package. Running this
command will install `ltmain.sh`. You should execute it before
`aclocal` and `automake`.

People upgrading old packages to newer autotools are likely to face
this issue because older Automake versions used to call
`libtoolize`. Therefore old build scripts do not call
`libtoolize`.

Since Automake 1.6, it has been decided that running
`libtoolize` was not Automake’s business. Instead, that
functionality has been moved into the `autoreconf` command
(see [Using `autoreconf`](https://www.gnu.org/software/autoconf/manual/autoconf.html#autoreconf-Invocation) in The Autoconf Manual). The `autoreconf` command saves you
from having to figure out which infrastructure scripts to run in what
order. Augmenting existing `bootstrap` or `autogen.sh`
scripts with a call to `autoreconf` should also free you from
any similar incompatible change in the future.

---

Next: [Objects ‘`created with both libtool and without`’](#Objects-created-both-with-libtool-and-without), Previous: [Error: ‘`required file './ltmain.sh' not found`’—`libtoolize` needed](#Error-required-file-ltmain_002esh-not-found), Up: [Common Issues Related to Libtool’s Use](#Libtool-Issues)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.9.2 Error: ‘`Libtool library used but 'LIBTOOL' is undefined`’—`libtoolize` redux [¶](#Error_003a-Libtool-library-used-but-_0027LIBTOOL_0027-is-undefined_002d_002d_002dlibtoolize-redux)

This error message results from Automake seeing Libtool being used in
`Makefile.am`, but the `LIBTOOL` variable is not defined.
There are two common reasons for this. First, `LT_INIT` is
missing from `configure.ac`; it can simply be added.

The second reason is more complicated: if you’ve installed Automake
under a separate prefix, Automake cannot know where to find the
`LT_INIT` macro. Automake looks in the `aclocal.m4` file,
which is generated by the `aclocal` command (see [Auto-generating aclocal.m4: Invoking `aclocal`](#aclocal-Invocation)). But since the default search paths for `aclocal`
are based on where it is installed (see [Macro Search Path](#Macro-Search-Path)), if
Automake is installed in one place (e.g., because it’s for a test
release), and Libtool in another (e.g., for general system use),
`libtool.m4` won’t be found by `aclocal` and thus
`LT_INIT` won’t be defined.

The best solution is to run `libtoolize` in the tree that is
using the separately-installed Automake. That will copy
`libtool.m4` and the other Libtool M4 files and scripts into your
package, including `ltmain.sh` (see previous section). If you
have subpackages that use Libtool independently, you’ll need to run
`libtoolize` in each of them. See [Invoking libtoolize](https://www.gnu.org/software/libtool/manual/libtool.html#Invoking-libtoolize) in The Libtool Manual.

The `libtoolize` output suggests using an `m4` subdirectory:

```
libtoolize: Consider adding 'AC_CONFIG_MACRO_DIRS([m4])' to configure.ac,
libtoolize: and rerunning libtoolize and aclocal.
libtoolize: Consider adding '-I m4' to ACLOCAL_AMFLAGS in Makefile.am.
```

For purposes of Libtool and Automake, however, an m4 subdirectory is
required, not just something to consider. Without having the Libtool
M4 files in the local package, `aclocal` will (frustratingly)
report the same ‘`Libtool library used but ...`’ error. (The name
of the subdirectory for the M4 files can be anything, but ‘`m4`’ is
by far the most common convention.)

Here is a concrete example. Suppose you’ve installed a test
release of Automake (thank you) with `--prefix=/tmp/amtest`, and
thus put `/tmp/amtest/bin` first in your `PATH`. You already
have Libtool installed under `/usr/local/gnu`. Then, your
Libtool-using package will fail to bootstrap, with the error:

```
Makefile.am:161: error: Libtool library used but 'LIBTOOL' is undefined
...
```

You can resolve this by ensuring your `configure.ac` includes
these lines (after the `AM_INIT_AUTOMAKE`):

```
AC_CONFIG_MACRO_DIRS([m4])
AM_PROG_AR
LT_INIT
```

and then running:

```
libtoolize
autoreconf
```

To summarize, the `libtoolize` (from the system directory)
copies `libtool.m4` and the other Libtool support files into your
package. The subsequent `autoreconf` then runs
`aclocal` (under the test prefix) which can now find
`LT_INIT`.

(For more discussion of this, see <https://bugs.gnu.org/71847>.

---

Previous: [Error: ‘`Libtool library used but 'LIBTOOL' is undefined`’—`libtoolize` redux](#Libtool-library-used-but-LIBTOOL-is-undefined), Up: [Common Issues Related to Libtool’s Use](#Libtool-Issues)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.9.3 Objects ‘`created with both libtool and without`’ [¶](#Objects-created-with-both-libtool-and-without)

Sometimes, the same source file is used both to build a Libtool
library and to build another non-libtool target (be it a program or
another library).

Let’s consider the following `Makefile.am`.

```
bin_PROGRAMS = prog
prog_SOURCES = prog.c foo.c ...

lib_LTLIBRARIES = libfoo.la
libfoo_la_SOURCES = foo.c ...
```

(In this trivial case the issue could be avoided by linking
`libfoo.la` with `prog` instead of listing `foo.c` in
`prog_SOURCES`. But let’s assume we want to keep `prog` and
`libfoo.la` separate.)

Technically, it means that we should build `foo.$(OBJEXT)` for
`prog`, and `foo.lo` for `libfoo.la`. The problem is
that in the course of creating `foo.lo`, Libtool may erase (or
replace) `foo.$(OBJEXT)`, and this cannot be avoided.

Therefore, when Automake detects this situation it will complain
with a message such as

```
object 'foo.$(OBJEXT)' created both with libtool and without
```

A workaround for this issue is to ensure that these two objects get
different basenames. As explained in [Why are object files sometimes renamed?](#Renamed-Objects), this
happens automatically when per-target flags are used.

```
bin_PROGRAMS = prog
prog_SOURCES = prog.c foo.c ...
prog_CFLAGS = $(AM_CFLAGS)

lib_LTLIBRARIES = libfoo.la
libfoo_la_SOURCES = foo.c ...
```

Adding ‘`prog_CFLAGS = $(AM_CFLAGS)`’ is almost a no-op, because
when the `prog_CFLAGS` is defined, it is used instead of
`AM_CFLAGS`. However as a side effect it will cause
`prog.c` and `foo.c` to be compiled as
`prog-prog.$(OBJEXT)` and `prog-foo.$(OBJEXT)`, which solves
the issue.

---

Next: [Default `_SOURCES`](#Default-_005fSOURCES), Previous: [Building a Shared Library](#A-Shared-Library), Up: [Building Programs and Libraries](#Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
