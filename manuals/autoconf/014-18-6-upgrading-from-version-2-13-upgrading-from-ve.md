### 18.6 Upgrading From Version 2.13 [¶](#Upgrading-From-Version-2_002e13)

The introduction of the previous section (see [Upgrading From Version 1](#Autoconf-1)) perfectly
suits this section...

> Autoconf version 2.50 is mostly backward compatible with version 2.13.
> However, it introduces better ways to do some things, and doesn’t
> support some of the ugly things in version 2.13. So, depending on how
> sophisticated your `configure.ac` files are, you might have to do
> some manual work in order to upgrade to version 2.50. This chapter
> points out some problems to watch for when upgrading. Also, perhaps
> your `configure` scripts could benefit from some of the new
> features in version 2.50; the changes are summarized in the file
> `NEWS` in the Autoconf distribution.

* [Changed Quotation](#Changed-Quotation)
* [New Macros](#New-Macros)
* [Hosts and Cross-Compilation](#Hosts-and-Cross_002dCompilation)
* [`AC_LIBOBJ` vs. `LIBOBJS`](#AC_005fLIBOBJ-vs-LIBOBJS)
* [`AC_ACT_IFELSE` vs. `AC_TRY_ACT`](#AC_005fACT_005fIFELSE-vs-AC_005fTRY_005fACT)

---

Next: [New Macros](#New-Macros), Up: [Upgrading From Version 2.13](#Autoconf-2_002e13)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 18.6.1 Changed Quotation [¶](#Changed-Quotation-1)

The most important changes are invisible to you: the implementation of
most macros have completely changed. This allowed more factorization of
the code, better error messages, a higher uniformity of the user’s
interface etc. Unfortunately, as a side effect, some construct which
used to (miraculously) work might break starting with Autoconf 2.50.
The most common culprit is bad quotation.

For instance, in the following example, the message is not properly
quoted:

```
AC_INIT
AC_CHECK_HEADERS(foo.h, ,
  AC_MSG_ERROR(cannot find foo.h, bailing out))
AC_OUTPUT
```

Autoconf 2.13 simply ignores it:

```
$ autoconf-2.13; ./configure --silent
creating cache ./config.cache
configure: error: cannot find foo.h
$
```

while Autoconf 2.50 produces a broken `configure`:

```
$ autoconf-2.50; ./configure --silent
configure: error: cannot find foo.h
./configure: exit: bad non-numeric arg `bailing'
./configure: exit: bad non-numeric arg `bailing'
$
```

The message needs to be quoted, and the `AC_MSG_ERROR` invocation
too!

```
AC_INIT([Example], [1.0], [bug-example@example.org])
AC_CHECK_HEADERS([foo.h], [],
  [AC_MSG_ERROR([cannot find foo.h, bailing out])])
AC_OUTPUT
```

Many many (and many more) Autoconf macros were lacking proper quotation,
including no less than… `AC_DEFUN` itself!

```
$ cat configure.in
AC_DEFUN([AC_PROG_INSTALL],
[# My own much better version
])
AC_INIT
AC_PROG_INSTALL
AC_OUTPUT
$ autoconf-2.13
autoconf: Undefined macros:
***BUG in Autoconf--please report*** AC_FD_MSG
***BUG in Autoconf--please report*** AC_EPI
configure.in:1:AC_DEFUN([AC_PROG_INSTALL],
configure.in:5:AC_PROG_INSTALL
$ autoconf-2.50
$
```

---

Next: [Hosts and Cross-Compilation](#Hosts-and-Cross_002dCompilation), Previous: [Changed Quotation](#Changed-Quotation), Up: [Upgrading From Version 2.13](#Autoconf-2_002e13)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 18.6.2 New Macros [¶](#New-Macros-1)

While Autoconf was relatively dormant in the late 1990s, Automake
provided Autoconf-like macros for a while. Starting with Autoconf 2.50
in 2001, Autoconf provided
versions of these macros, integrated in the `AC_` namespace,
instead of `AM_`. But in order to ease the upgrading via
`autoupdate`, bindings to such `AM_` macros are provided.

Unfortunately older versions of Automake (e.g., Automake 1.4)
did not quote the names of these macros.
Therefore, when `m4` finds something like
‘`AC_DEFUN(AM_TYPE_PTRDIFF_T, …)`’ in `aclocal.m4`,
`AM_TYPE_PTRDIFF_T` is
expanded, replaced with its Autoconf definition.

Fortunately Autoconf catches pre-`AC_INIT` expansions, and
complains, in its own words:

```
$ cat configure.ac
AC_INIT([Example], [1.0], [bug-example@example.org])
AM_TYPE_PTRDIFF_T
$ aclocal-1.4
$ autoconf
aclocal.m4:17: error: m4_defn: undefined macro: _m4_divert_diversion
aclocal.m4:17: the top level
autom4te: m4 failed with exit status: 1
$
```

Modern versions of Automake no longer define most of these
macros, and properly quote the names of the remaining macros.
If you must use an old Automake, do not depend upon macros from Automake
as it is simply not its job
to provide macros (but the one it requires itself):

```
$ cat configure.ac
AC_INIT([Example], [1.0], [bug-example@example.org])
AM_TYPE_PTRDIFF_T
$ rm aclocal.m4
$ autoupdate
autoupdate: 'configure.ac' is updated
$ cat configure.ac
AC_INIT([Example], [1.0], [bug-example@example.org])
AC_CHECK_TYPES([ptrdiff_t])
$ aclocal-1.4
$ autoconf
$
```

---

Next: [`AC_LIBOBJ` vs. `LIBOBJS`](#AC_005fLIBOBJ-vs-LIBOBJS), Previous: [New Macros](#New-Macros), Up: [Upgrading From Version 2.13](#Autoconf-2_002e13)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 18.6.3 Hosts and Cross-Compilation [¶](#Hosts-and-Cross_002dCompilation-1)

Based on the experience of compiler writers, and after long public
debates, many aspects of the cross-compilation chain have changed:

* the relationship between the build, host, and target architecture types,
* the command line interface for specifying them to `configure`,
* the variables defined in `configure`,
* the enabling of cross-compilation mode.

  

The relationship between build, host, and target have been cleaned up:
the chain of default is now simply: target defaults to host, host to
build, and build to the result of `config.guess`. Nevertheless,
in order to ease the transition from 2.13 to 2.50, the following
transition scheme is implemented. *Do not rely on it*, as it will
be completely disabled in a couple of releases (we cannot keep it, as it
proves to cause more problems than it cures).

They all default to the result of running `config.guess`, unless
you specify either `--build` or `--host`. In this case,
the default becomes the system type you specified. If you specify both,
and they’re different, `configure` enters cross compilation
mode, so it doesn’t run any tests that require execution.

Hint: if you mean to override the result of `config.guess`,
prefer `--build` over `--host`.

  

For backward compatibility, `configure` accepts a system
type as an option by itself. Such an option overrides the
defaults for build, host, and target system types. The following
configure statement configures a cross toolchain that runs on
NetBSD/aarch64 but generates code for GNU Hurd/riscv64,
which is also the build platform.

```
./configure --host=aarch64-netbsd riscv64-gnu
```

  

In Autoconf 2.13 and before, the variables `build`, `host`,
and `target` had a different semantics before and after the
invocation of `AC_CANONICAL_BUILD` etc. Now, the argument of
`--build` is strictly copied into `build_alias`, and is left
empty otherwise. After the `AC_CANONICAL_BUILD`, `build` is
set to the canonicalized build type. To ease the transition, before,
its contents is the same as that of `build_alias`. Do *not*
rely on this broken feature.

For consistency with the backward compatibility scheme exposed above,
when `--host` is specified but `--build` isn’t, the build
system is assumed to be the same as `--host`, and
‘`build_alias`’ is set to that value. Eventually, this
historically incorrect behavior will go away.

  

The former scheme to enable cross-compilation proved to cause more harm
than good, in particular, it used to be triggered too easily, leaving
regular end users puzzled in front of cryptic error messages.
`configure` could even enter cross-compilation mode only
because the compiler was not functional. This is mainly because
`configure` used to try to detect cross-compilation, instead of
waiting for an explicit flag from the user.

Now, `configure` enters cross-compilation mode if and only if
`--host` is passed.

That’s the short documentation. To ease the transition between 2.13 and
its successors, a more complicated scheme is implemented. *Do not
rely on the following*, as it will be removed in the near future.

If you specify `--host`, but not `--build`, when
`configure` performs the first compiler test it tries to run
an executable produced by the compiler. If the execution fails, it
enters cross-compilation mode. This is fragile. Moreover, by the time
the compiler test is performed, it may be too late to modify the
build-system type: other tests may have already been performed.
Therefore, whenever you specify `--host`, be sure to specify
`--build` too.

```
./configure --build=x86_64-pc-linux-gnu --host=x86_64-w64-mingw64
```

enters cross-compilation mode. The former interface, which
consisted in setting the compiler to a cross-compiler without informing
`configure` is obsolete. For instance, `configure`
fails if it can’t run the code generated by the specified compiler if you
configure as follows:

```
./configure CC=x86_64-w64-mingw64-gcc
```

---

Next: [`AC_ACT_IFELSE` vs. `AC_TRY_ACT`](#AC_005fACT_005fIFELSE-vs-AC_005fTRY_005fACT), Previous: [Hosts and Cross-Compilation](#Hosts-and-Cross_002dCompilation), Up: [Upgrading From Version 2.13](#Autoconf-2_002e13)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 18.6.4 `AC_LIBOBJ` vs. `LIBOBJS` [¶](#AC_005fLIBOBJ-vs_002e-LIBOBJS)

Up to Autoconf 2.13, the replacement of functions was triggered via the
variable `LIBOBJS`. Since Autoconf 2.50, the macro
`AC_LIBOBJ` should be used instead (see [Generic Function Checks](#Generic-Functions)).
Starting at Autoconf 2.53, the use of `LIBOBJS` is an error.

This change is mandated by the unification of the GNU Build System
components. In particular, the various fragile techniques used to parse
a `configure.ac` are all replaced with the use of traces. As a
consequence, any action must be traceable, which obsoletes critical
variable assignments. Fortunately, `LIBOBJS` was the only problem,
and it can even be handled gracefully (read, “without your having to
change something”).

There were two typical uses of `LIBOBJS`: asking for a replacement
function, and adjusting `LIBOBJS` for Automake and/or Libtool.

  

As for function replacement, the fix is immediate: use
`AC_LIBOBJ`. For instance:

```
LIBOBJS="$LIBOBJS fnmatch.o"
LIBOBJS="$LIBOBJS malloc.$ac_objext"
```

should be replaced with:

```
AC_LIBOBJ([fnmatch])
AC_LIBOBJ([malloc])
```

  

When used with Automake 1.10 or newer, a suitable value for
`LIBOBJDIR` is set so that the `LIBOBJS` and `LTLIBOBJS`
can be referenced from any `Makefile.am`. Even without Automake,
arranging for `LIBOBJDIR` to be set correctly enables
referencing `LIBOBJS` and `LTLIBOBJS` in another directory.
The `LIBOBJDIR` feature is experimental.

---

Previous: [`AC_LIBOBJ` vs. `LIBOBJS`](#AC_005fLIBOBJ-vs-LIBOBJS), Up: [Upgrading From Version 2.13](#Autoconf-2_002e13)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 18.6.5 `AC_ACT_IFELSE` vs. `AC_TRY_ACT` [¶](#AC_005fACT_005fIFELSE-vs_002e-AC_005fTRY_005fACT)

Since Autoconf 2.50, internal codes uses `AC_PREPROC_IFELSE`,
`AC_COMPILE_IFELSE`, `AC_LINK_IFELSE`, and
`AC_RUN_IFELSE` on one hand and `AC_LANG_SOURCE`,
and `AC_LANG_PROGRAM` on the other hand instead of the deprecated
`AC_TRY_CPP`, `AC_TRY_COMPILE`, `AC_TRY_LINK`, and
`AC_TRY_RUN`. The motivations where:

* a more consistent interface: `AC_TRY_COMPILE` etc. were double
  quoting their arguments;
* the combinatorial explosion is solved by decomposing on the one hand the
  generation of sources, and on the other hand executing the program;
* this scheme helps supporting more languages than plain C and C++.

In addition to the change of syntax, the philosophy has changed too:
while emphasis was put on speed at the expense of accuracy, today’s
Autoconf promotes accuracy of the testing framework at, ahem…, the
expense of speed.

As a perfect example of what is *not* to be done, here is how to
find out whether a header file contains a particular declaration, such
as a typedef, a structure, a structure member, or a function. Use
`AC_EGREP_HEADER` instead of running `grep` directly on the
header file; on some systems the symbol might be defined in another
header file that the file you are checking includes.

As a (bad) example, here is how you should not check for C preprocessor
symbols, either defined by header files or predefined by the C
preprocessor: using `AC_EGREP_CPP`:

```
AC_EGREP_CPP(yes,
[#ifdef _AIX
  yes
#endif
], is_aix=yes, is_aix=no)
```

The above example, properly written would (i) use
`AC_LANG_PROGRAM`, and (ii) run the compiler:

```
AC_COMPILE_IFELSE([AC_LANG_PROGRAM(
[[#ifndef _AIX
 error: This isn't AIX!
#endif
]])],
                   [is_aix=yes],
                   [is_aix=no])
```

---

Next: [Frequent Autoconf Questions, with answers](#FAQ), Previous: [Obsolete Constructs](#Obsolete-Constructs), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 19 Generating Test Suites with Autotest [¶](#Generating-Test-Suites-with-Autotest)

```
N.B.: This section describes a feature which is still
stabilizing.  Although we believe that Autotest is useful as-is, this
documentation describes an interface which might change in the future:
do not depend upon Autotest without subscribing to the Autoconf mailing
lists.
```

It is paradoxical that portable projects depend on nonportable tools
to run their test suite. Autoconf by itself is the paragon of this
problem: although it aims at perfectly portability, up to 2.13 its
test suite was using DejaGNU, a rich and complex testing
framework, but which is far from being standard on POSIX systems.
Worse yet, it was likely to be missing on the most fragile platforms,
the very platforms that are most likely to torture Autoconf and
exhibit deficiencies.

To circumvent this problem, many package maintainers have developed their
own testing framework, based on simple shell scripts whose sole outputs
are exit status values describing whether the test succeeded. Most of
these tests share common patterns, and this can result in lots of
duplicated code and tedious maintenance.

Following exactly the same reasoning that yielded to the inception of
Autoconf, Autotest provides a test suite generation framework, based on
M4 macros building a portable shell script. The suite itself is
equipped with automatic logging and tracing facilities which greatly
diminish the interaction with bug reporters, and simple timing reports.

Autoconf itself has been using Autotest for years, and we do attest that
it has considerably improved the strength of the test suite and the
quality of bug reports. Other projects are known to use some generation
of Autotest, such as Bison, GNU Wdiff, GNU Tar, each of
them with different needs, and this usage has validated Autotest as a general
testing framework.

Nonetheless, compared to DejaGNU, Autotest is inadequate for
interactive tool testing, which is probably its main limitation.

* [Using an Autotest Test Suite](#Using-an-Autotest-Test-Suite)
* [Writing `testsuite.at`](#Writing-Testsuites)
* [Running `testsuite` Scripts](#testsuite-Invocation)
* [Making `testsuite` Scripts](#Making-testsuite-Scripts)

---

Next: [Writing `testsuite.at`](#Writing-Testsuites), Up: [Generating Test Suites with Autotest](#Using-Autotest)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 19.1 Using an Autotest Test Suite [¶](#Using-an-Autotest-Test-Suite-1)

* [`testsuite` Scripts](#testsuite-Scripts)
* [Autotest Logs](#Autotest-Logs)

---

Next: [Autotest Logs](#Autotest-Logs), Up: [Using an Autotest Test Suite](#Using-an-Autotest-Test-Suite)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 19.1.1 `testsuite` Scripts [¶](#testsuite-Scripts-1)

Generating testing or validation suites using Autotest is rather easy.
The whole validation suite is held in a file to be processed through
`autom4te`, itself using GNU M4 under the hood, to
produce a stand-alone Bourne shell script which then gets distributed.
Neither `autom4te` nor GNU M4 are needed at
the installer’s end.

Each test of the validation suite should be part of some test group. A
*test group* is a sequence of interwoven tests that ought to be
executed together, usually because one test in the group creates data
files that a later test in the same group needs to read. Complex test
groups make later debugging more tedious. It is much better to
keep only a few tests per test group. Ideally there is only one test
per test group.

For all but the simplest packages, some file such as `testsuite.at`
does not fully hold all test sources, as these are often easier to
maintain in separate files. Each of these separate files holds a single
test group, or a sequence of test groups all addressing some common
functionality in the package. In such cases, `testsuite.at`
merely initializes the validation suite, and sometimes does elementary
health checking, before listing include statements for all other test
files. The special file `package.m4`, containing the
identification of the package, is automatically included if found.

A convenient alternative consists in moving all the global issues
(local Autotest macros, elementary health checking, and `AT_INIT`
invocation) into the file `local.at`, and making
`testsuite.at` be a simple list of `m4_include`s of sub test
suites. In such case, generating the whole test suite or pieces of it
is only a matter of choosing the `autom4te` command line
arguments.

The validation scripts that Autotest produces are by convention called
`testsuite`. When run, `testsuite` executes each test
group in turn, producing only one summary line per test to say if that
particular test succeeded or failed. At end of all tests, summarizing
counters get printed. One debugging directory is left for each test
group which failed, if any: such directories are named
`testsuite.dir/nn`, where nn is the sequence number of
the test group, and they include:

* a debugging script named `run` which reruns the test in
  *debug mode* (see [Running `testsuite` Scripts](#testsuite-Invocation)). The automatic generation
  of debugging scripts has the purpose of easing the chase for bugs.
* all the files created with `AT_DATA`
* all the Erlang source code files created with `AT_CHECK_EUNIT`
* a log of the run, named `testsuite.log`

In the ideal situation, none of the tests fail, and consequently no
debugging directory is left behind for validation.

It often happens in practice that individual tests in the validation
suite need to get information coming out of the configuration process.
Some of this information, common for all validation suites, is provided
through the file `atconfig`, automatically created by
`AC_CONFIG_TESTDIR`. For configuration information which your
testing environment specifically needs, you might prepare an optional
file named `atlocal.in`, instantiated by `AC_CONFIG_FILES`.
The configuration process produces `atconfig` and `atlocal`
out of these two input files, and these two produced files are
automatically read by the `testsuite` script.

Here is a diagram showing the relationship between files.

Files used in preparing a software package for distribution:

```
                [package.m4] -->.
                                 \
subfile-1.at ->.  [local.at] ---->+
    ...         \                  \
subfile-i.at ---->-- testsuite.at -->-- autom4te* -->testsuite
    ...         /
subfile-n.at ->'
```

Files used in configuring a software package:

```
                                     .--> atconfig
                                    /
[atlocal.in] -->  config.status* --<
                                    \
                                     `--> [atlocal]
```

Files created during test suite execution:

```
atconfig -->.                    .--> testsuite.log
             \                  /
              >-- testsuite* --<
             /                  \
[atlocal] ->'                    `--> [testsuite.dir]
```

---

Previous: [`testsuite` Scripts](#testsuite-Scripts), Up: [Using an Autotest Test Suite](#Using-an-Autotest-Test-Suite)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 19.1.2 Autotest Logs [¶](#Autotest-Logs-1)

When run, the test suite creates a log file named after itself, e.g., a
test suite named `testsuite` creates `testsuite.log`. It
contains a lot of information, usually more than maintainers actually
need, but therefore most of the time it contains all that is needed:

command line arguments
:   A bad but unfortunately widespread habit consists of
    setting environment variables before the command, such as in
    ‘`CC=my-home-grown-cc ./testsuite`’. The test suite does not
    know this change, hence (i) it cannot report it to you, and (ii)
    it cannot preserve the value of `CC` for subsequent runs.
    Autoconf faced exactly the same problem, and solved it by asking
    users to pass the variable definitions as command line arguments.
    Autotest requires this rule, too, but has no means to enforce it; the log
    then contains a trace of the variables that were changed by the user.

`ChangeLog` excerpts
:   The topmost lines of all the `ChangeLog` files found in the source
    hierarchy. This is especially useful when bugs are reported against
    development versions of the package, since the version string does not
    provide sufficient information to know the exact state of the sources
    the user compiled. Of course, this relies on the use of a
    `ChangeLog`.

build machine
:   Running a test suite in a cross-compile environment is not an easy task,
    since it would mean having the test suite run on a machine build,
    while running programs on a machine host. It is much simpler to
    run both the test suite and the programs on host, but then, from
    the point of view of the test suite, there remains a single environment,
    host = build. The log contains relevant information on the
    state of the build machine, including some important environment
    variables.

tested programs
:   The absolute file name and answers to `--version` of the tested
    programs (see [Writing `testsuite.at`](#Writing-Testsuites), `AT_TESTED`).

configuration log
:   The contents of `config.log`, as created by `configure`,
    are appended. It contains the configuration flags and a detailed report
    on the configuration itself.

---

Next: [Running `testsuite` Scripts](#testsuite-Invocation), Previous: [Using an Autotest Test Suite](#Using-an-Autotest-Test-Suite), Up: [Generating Test Suites with Autotest](#Using-Autotest)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 19.2 Writing `testsuite.at` [¶](#Writing-testsuite_002eat)

The `testsuite.at` is a Bourne shell script making use of special
Autotest M4 macros. It often contains a call to `AT_INIT` near
its beginning followed by one call to `m4_include` per source file
for tests. Each such included file, or the remainder of
`testsuite.at` if include files are not used, contain a sequence of
test groups. Each test group begins with a call to `AT_SETUP`,
then an arbitrary number of shell commands or calls to `AT_CHECK`,
and then completes with a call to `AT_CLEANUP`. Multiple test
groups can be categorized by a call to `AT_BANNER`.

All of the public Autotest macros have all-uppercase names in the
namespace ‘`^AT_`’ to prevent them from accidentally conflicting with
other text; Autoconf also reserves the namespace ‘`^_AT_`’ for
internal macros. All shell variables used in the testsuite for internal
purposes have mostly-lowercase names starting with ‘`at_`’. Autotest
also uses here-document delimiters in the namespace ‘`^_AT[A-Z]`’, and
makes use of the file system namespace ‘`^at-`’.

Since Autoconf is built on top of M4sugar (see [Programming in M4sugar](#Programming-in-M4sugar)) and M4sh (see [Programming in M4sh](#Programming-in-M4sh)), you must also be aware
of those namespaces (‘`^_?\(m4\|AS\)_`’). In general, you
*should not use* the namespace of a package that does not own the
macro or shell code you are writing.

Macro: **AT\_INIT**([name]) [¶](#index-AT_005fINIT-1)
:   Initialize Autotest. Giving a name to the test suite is
    encouraged if your package includes several test suites. Before this
    macro is called, `AT_PACKAGE_STRING` and
    `AT_PACKAGE_BUGREPORT` must be defined, which are used to display
    information about the testsuite to the user. Typically, these macros
    are provided by a file `package.m4` built by `make`
    (see [Making `testsuite` Scripts](#Making-testsuite-Scripts)), in order to inherit the package
    name, version, and bug reporting address from `configure.ac`.

Macro: **AT\_COPYRIGHT**(copyright-notice) [¶](#index-AT_005fCOPYRIGHT-1)
:   State that, in addition to the Free Software Foundation’s copyright on
    the Autotest macros, parts of your test suite are covered by
    copyright-notice.

    The copyright-notice shows up in both the head of
    `testsuite` and in ‘`testsuite --version`’.

Macro: **AT\_ARG\_OPTION**(options, help-text, [action-if-given], [action-if-not-given]) [¶](#index-AT_005fARG_005fOPTION-1)
:   Accept options from the space-separated list options, a list that
    has leading dashes removed from the options. Long options will be
    prefixed with ‘`--`’, single-character options with ‘`-`’. The
    first word in this list is the primary option, any others are
    assumed to be short-hand aliases. The variable associated with it
    is `at_arg_option`, with any dashes in option replaced
    with underscores.

    If the user passes `--option` to the `testsuite`,
    the variable will be set to ‘`:`’. If the user does not pass the
    option, or passes `--no-option`, then the variable will be
    set to ‘`false`’.

    action-if-given is run each time the option is encountered; here,
    the variable `at_optarg` will be set to ‘`:`’ or ‘`false`’ as
    appropriate. `at_optarg` is actually just a copy of
    `at_arg_option`.

    action-if-not-given will be run once after option parsing is
    complete and if no option from options was used.

    help-text is added to the end of the list of options shown in
    `testsuite --help` (see [AS\_HELP\_STRING](#AS_005fHELP_005fSTRING)).

    It is recommended that you use a package-specific prefix to options
    names in order to avoid clashes with future Autotest built-in options.

Macro: **AT\_ARG\_OPTION\_ARG**(options, help-text, [action-if-given], [action-if-not-given]) [¶](#index-AT_005fARG_005fOPTION_005fARG-1)
:   Accept options with arguments from the space-separated list
    options, a list that has leading dashes removed from the options.
    Long options will be prefixed with ‘`--`’, single-character options
    with ‘`-`’. The first word in this list is the primary option,
    any others are assumed to be short-hand aliases. The variable associated
    with it is `at_arg_option`, with any dashes in option
    replaced with underscores.

    If the user passes `--option=arg` or
    `--option arg` to the `testsuite`, the
    variable will be set to ‘`arg`’.

    action-if-given is run each time the option is encountered; here,
    the variable `at_optarg` will be set to ‘`arg`’.
    `at_optarg` is actually just a copy of `at_arg_option`.

    action-if-not-given will be run once after option parsing is
    complete and if no option from options was used.

    help-text is added to the end of the list of options shown in
    `testsuite --help` (see [AS\_HELP\_STRING](#AS_005fHELP_005fSTRING)).

    It is recommended that you use a package-specific prefix to options
    names in order to avoid clashes with future Autotest built-in options.

Macro: **AT\_COLOR\_TESTS** [¶](#index-AT_005fCOLOR_005fTESTS-1)
:   Enable colored test results by default when the output is connected to
    a terminal.

Macro: **AT\_TESTED**(executables) [¶](#index-AT_005fTESTED-1)
:   Log the file name and answer to `--version` of each program in
    space-separated list executables. Several invocations register
    new executables, in other words, don’t fear registering one program
    several times.

    Autotest test suites rely on `PATH` to find the tested program.
    This avoids the need to generate absolute names of the various tools, and
    makes it possible to test installed programs. Therefore, knowing which
    programs are being exercised is crucial to understanding problems in
    the test suite itself, or its occasional misuses. It is a good idea to
    also subscribe foreign programs you depend upon, to avoid incompatible
    diagnostics.

    executables is implicitly wrapped in shell double quotes, but it
    will still use shell variable expansion (‘`$`’), command substitution
    (‘`` ` ``’), and backslash escaping (‘`\`’). In particular, the
    `EXEEXT` variable is available if it is passed to the testsuite
    via `atlocal` or `atconfig`.

Macro: **AT\_PREPARE\_TESTS**(shell-code) [¶](#index-AT_005fPREPARE_005fTESTS-1)
:   Execute shell-code in the main testsuite process,
    after initializing the test suite and processing command-line options,
    but before running any tests. If this macro is used several times,
    all of the shell-codes will be executed,
    in the order they appeared in `testsuite.at`.

    One reason to use `AT_PREPARE_TESTS` is when the programs under
    test are sensitive to environment variables: you can unset all these
    variables or reset them to safe values in shell-code.

    shell-code is only executed if at least one test is going to be
    run. In particular, it will not be executed if any of the `--help`,
    `--version`, `--list`, or `--clean` options are
    given to `testsuite` (see [Running `testsuite` Scripts](#testsuite-Invocation)).

Macro: **AT\_PREPARE\_EACH\_TEST**(shell-code) [¶](#index-AT_005fPREPARE_005fEACH_005fTEST)
:   Execute shell-code in each test group’s subshell, at the point of
    the `AT_SETUP` that starts the test group.

Macro: **AT\_TEST\_HELPER\_FN**(name, args, description, code) [¶](#index-AT_005fTEST_005fHELPER_005fFN)
:   Define a shell function that will be available to the code for each test
    group. Its name will be `ath_fn_name`, and its body will be
    code. (The prefix prevents name conflicts with shell functions
    defined by M4sh and Autotest.)

    args should describe the function’s arguments and description
    what it does; these are used only for documentation comments in the
    generated testsuite script.

  

Macro: **AT\_BANNER**(test-category-name) [¶](#index-AT_005fBANNER-1)
:   This macro identifies the start of a category of related test groups.
    When the resulting `testsuite` is invoked with more than one test
    group to run, its output will include a banner containing
    test-category-name prior to any tests run from that category. The
    banner should be no more than about 40 or 50 characters. A blank banner
    indicates uncategorized tests; an empty line will be inserted after
    tests from an earlier category, effectively ending that category.

Macro: **AT\_SETUP**(test-group-name) [¶](#index-AT_005fSETUP-1)
:   This macro starts a group of related tests, all to be executed in the
    same subshell. It accepts a single argument, which holds a few words
    (no more than about 30 or 40 characters) quickly describing the purpose
    of the test group being started. test-group-name must not expand
    to unbalanced quotes, although quadrigraphs can be used.

Macro: **AT\_KEYWORDS**(keywords) [¶](#index-AT_005fKEYWORDS-1)
:   Associate the space-separated list of keywords to the enclosing
    test group. This makes it possible to run “slices” of the test suite.
    For instance, if some of your test groups exercise some ‘`foo`’
    feature, then using ‘`AT_KEYWORDS(foo)`’ lets you run
    ‘`./testsuite -k foo`’ to run exclusively these test groups. The
    test-group-name of the test group is automatically recorded to
    `AT_KEYWORDS`.

    Several invocations within a test group accumulate new keywords. In
    other words, don’t fear registering the same keyword several times in a
    test group.

Macro: **AT\_CAPTURE\_FILE**(file) [¶](#index-AT_005fCAPTURE_005fFILE-1)
:   If the current test group fails, log the contents of file.
    Several identical calls within one test group have no additional effect.

Macro: **AT\_FAIL\_IF**(shell-condition) [¶](#index-AT_005fFAIL_005fIF-1)
:   Make the test group fail and skip the rest of its execution, if
    shell-condition is true. shell-condition is a shell expression
    such as a `test` command. Tests before `AT_FAIL_IF`
    will be executed and may still cause the test group to be skipped.
    You can instantiate this macro many times from within the same test group.

    You should use this macro only for very simple failure conditions. If the
    shell-condition could emit any kind of output you should instead
    use `AT_CHECK` like

    ```
    AT_CHECK([if shell-condition; then exit 99; fi])
    ```

    so that such output is properly recorded in the `testsuite.log`
    file.

Macro: **AT\_SKIP\_IF**(shell-condition) [¶](#index-AT_005fSKIP_005fIF-1)
:   Determine whether the test should be skipped because it requires
    features that are unsupported on the machine under test.
    shell-condition is a shell expression such as a `test`
    command. Tests before `AT_SKIP_IF` will be executed
    and may still cause the test group to fail. You can instantiate this
    macro many times from within the same test group.

    You should use this macro only for very simple skip conditions. If the
    shell-condition could emit any kind of output you should instead
    use `AT_CHECK` like

    ```
    AT_CHECK([if shell-condition; then exit 77; fi])
    ```

    so that such output is properly recorded in the `testsuite.log`
    file.

Macro: **AT\_XFAIL\_IF**(shell-condition) [¶](#index-AT_005fXFAIL_005fIF-1)
:   Determine whether the test is expected to fail because it is a known
    bug (for unsupported features, you should skip the test).
    shell-condition is a shell expression such as a `test`
    command; you can instantiate this macro many times from within the
    same test group, and one of the conditions is enough to turn
    the test into an expected failure.

Macro: **AT\_CLEANUP** [¶](#index-AT_005fCLEANUP-1)
:   End the current test group.

  

Macro: **AT\_DATA**(file, contents) [¶](#index-AT_005fDATA-1)

Macro: **AT\_DATA\_UNQUOTED**(file, contents) [¶](#index-AT_005fDATA_005fUNQUOTED)
:   Initialize an input data file with given contents. Of
    course, the contents have to be properly quoted between square
    brackets to protect against included commas or spurious M4
    expansion. contents must be empty or end with a newline.
    file must
    be a single shell word that expands into a single file name.

    The difference between `AT_DATA` and `AT_DATA_UNQUOTED` is
    that only the latter performs shell variable expansion (‘`$`’),
    command substitution (‘`` ` ``’), and backslash escaping (‘`\`’)
    on contents.

Macro: **AT\_CHECK**(commands, [status = ‘`0`’], [stdout], [stderr], [run-if-fail], [run-if-pass]) [¶](#index-AT_005fCHECK-1)

Macro: **AT\_CHECK\_UNQUOTED**(commands, [status = ‘`0`’], [stdout], [stderr], [run-if-fail], [run-if-pass]) [¶](#index-AT_005fCHECK_005fUNQUOTED-1)
:   Perform a test, by running the shell commands in a subshell.
    commands is output as-is, so shell expansions are honored.
    These commands are expected to have a final exit status of status,
    and to produce output as described by stdout and stderr
    (see below).

    This macro must be invoked in between `AT_SETUP` and `AT_CLEANUP`.

    If commands exit with unexpected status 77, then the rest of the
    test group is skipped. If commands exit with unexpected status
    99, then the test group is immediately failed; this is called a
    *hard failure*. Otherwise, the test is considered to have
    succeeded if all of the status, stdout, and stderr expectations were
    met.

    If run-if-fail is nonempty, it provides extra shell commands to
    run when the test fails; if run-if-pass is nonempty, it provides
    extra shell commands to run when the test succeeds. These commands are
    *not* run in a subshell, and they are not run when the test group
    is skipped (exit code 77) or hard-failed (exit code 99). They may
    change whether the test group is considered to have succeeded, by
    modifying the shell variable `at_failed`; set it to `:` to
    indicate that the test group has failed, or `false` to indicate
    that it has succeeded.

    The exit status of commands is available to run-if-fail and
    run-if-pass commands in the `at_status` shell variable. The
    output from commands is also available, in the files named by the
    `at_stdout` and `at_stderr` variables.

    If status is the literal ‘`ignore`’, then the exit status of
    commands is not checked, except for the special cases of 77 (skip)
    and 99 (hard failure). The existence of hard failures allows one to
    mark a test as an expected failure with `AT_XFAIL_IF` because a
    feature has not yet been implemented, but to still distinguish between
    gracefully handling the missing feature and dumping core.

    If the value of the stdout or stderr parameter is one of the
    literals in the following table, then the test treats the output
    according to the rules of that literal.

    ‘`ignore`’
    :   The content of the output is ignored, but still captured in the test
        group log (if the testsuite is run with the `-v` option, the test
        group log is displayed as the test is run; if the test group later
        fails, the test group log is also copied into the overall testsuite
        log). This action is valid for both stdout and stderr.

    ‘`ignore-nolog`’
    :   The content of the output is ignored, and nothing is captured in the log
        files. If commands are likely to produce binary output (including
        long lines) or large amounts of output, then logging the output can make
        it harder to locate details related to subsequent tests within the
        group, and could potentially corrupt terminal display of a user running
        `testsuite -v`. This action is valid for both stdout and
        stderr.

    ‘`stdout`’
    :   Only valid as the stdout parameter. Capture the content of
        standard output in both a file named `stdout` and the test group log.
        Subsequent commands in the test group can then post-process the file.
        This action is often used when it is desired to use `grep` to
        look for a substring in the output, or when the output must be
        post-processed to normalize error messages into a common form.

    ‘`stderr`’
    :   Only valid as the stderr parameter. Capture the content of
        standard error in both a file named `stderr` and the test group log.

    ‘`stdout-nolog`’

    ‘`stderr-nolog`’
    :   Like ‘`stdout`’ or ‘`stderr`’, except that the captured output is
        not duplicated into the test group log. This action is particularly
        useful for an intermediate check that produces large amounts of data,
        which will be followed by another check that filters down to the
        relevant data, as it makes it easier to locate details in the log.

    ‘`expout`’
    :   Only valid as the stdout parameter. Compare standard output with
        the previously created file `expout`, and list any differences in
        the testsuite log.

    ‘`experr`’
    :   Only valid as the stderr parameter. Compare standard error with
        the previously created file `experr`, and list any differences in
        the testsuite log.

    Otherwise, the values of the stdout and stderr parameters
    are treated as text that must exactly match the output given by
    commands on standard output and standard error (including an empty
    parameter for no output); any differences are captured in the testsuite
    log and the test is failed (unless an unexpected exit status of 77
    skipped the test instead).

    `AT_CHECK_UNQUOTED` performs shell variable expansion (‘`$`’),
    command substitution (‘`` ` ``’), and backslash escaping (‘`\`’) on
    comparison text given in the stdout and stderr parameters;
    `AT_CHECK` does not. There is no difference in the interpretation
    of commands.

Macro: **AT\_CHECK\_EUNIT**(module, test-spec, [erlflags], [run-if-fail], [run-if-pass]) [¶](#index-AT_005fCHECK_005fEUNIT-1)
:   Initialize and execute an Erlang module named module that performs
    tests following the test-spec EUnit test specification.
    test-spec must be a valid EUnit test specification, as defined in
    the [EUnit
    Reference Manual](https://erlang.org/doc/apps/eunit/index.html). erlflags are optional command-line options
    passed to the Erlang interpreter to execute the test Erlang module.
    Typically, erlflags defines at least the paths to directories
    containing the compiled Erlang modules under test, as ‘`-pa path1
    path2 ...`’.

    For example, the unit tests associated with Erlang module ‘`testme`’,
    which compiled code is in subdirectory `src`, can be performed
    with:

    ```
    AT_CHECK_EUNIT([testme_testsuite], [{module, testme}],
                   [-pa "${abs_top_builddir}/src"])
    ```

    This macro must be invoked in between `AT_SETUP` and `AT_CLEANUP`.

    Variables `ERL`, `ERLC`, and (optionally) `ERLCFLAGS`
    must be defined as the path of the Erlang interpreter, the path of the
    Erlang compiler, and the command-line flags to pass to the compiler,
    respectively. Those variables should be configured in
    `configure.ac` using the `AC_ERLANG_PATH_ERL` and
    `AC_ERLANG_PATH_ERLC` macros, and the configured values of those
    variables are automatically defined in the testsuite. If `ERL` or
    `ERLC` is not defined, the test group is skipped.

    If the EUnit library cannot be found, i.e. if module `eunit` cannot
    be loaded, the test group is skipped. Otherwise, if test-spec is
    an invalid EUnit test specification, the test group fails. Otherwise,
    if the EUnit test passes, shell commands run-if-pass are executed
    or, if the EUnit test fails, shell commands run-if-fail are
    executed and the test group fails.

    Only the generated test Erlang module is automatically compiled and
    executed. If test-spec involves testing other Erlang modules,
    e.g. module ‘`testme`’ in the example above, those modules must be
    already compiled.

    If the testsuite is run in verbose mode and with the `--verbose` option,
    EUnit is also run in verbose mode to output more details about
    individual unit tests.

---

Next: [Making `testsuite` Scripts](#Making-testsuite-Scripts), Previous: [Writing `testsuite.at`](#Writing-Testsuites), Up: [Generating Test Suites with Autotest](#Using-Autotest)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 19.3 Running `testsuite` Scripts [¶](#Running-testsuite-Scripts)

Autotest test suites support the following options:

`--help`

`-h`
:   Display the list of options and exit successfully.

`--version`

`-V`
:   Display the version of the test suite and exit successfully.

`--directory=dir`

`-C dir`
:   Change the current directory to dir before creating any files.
    Useful for running the testsuite in a subdirectory from a top-level
    Makefile.

`--jobs[=n]`

`-j[n]`
:   Run n tests in parallel, if possible. If n is not given,
    run all given tests in parallel. Note that there should be no space
    before the argument to `-j`, as `-j number` denotes
    the separate arguments `-j` and `number`, see below.

    In parallel mode, the standard input device of the testsuite script is
    not available to commands inside a test group. Furthermore, banner
    lines are not printed, and the summary line for each test group is
    output after the test group completes. Summary lines may appear
    unordered. If verbose and trace output are enabled (see below), they
    may appear intermixed from concurrently running tests.

    Parallel mode requires the `mkfifo` command to work, and will be
    silently disabled otherwise.

`--clean`

`-c`
:   Remove all the files the test suite might have created and exit. Meant
    for `clean` Make targets.

`--list`

`-l`
:   List all the tests (or only the selection), including their possible
    keywords.

  

By default all tests are performed (or described with `--list`)
silently in the default environment, but the environment, set of tests,
and verbosity level can be tuned:

‘`variable=value`’
:   Set the environment variable to value. Use this rather
    than ‘`FOO=foo ./testsuite`’ as debugging scripts would then run in a
    different environment.

    The variable `AUTOTEST_PATH` specifies the testing path to prepend
    to `PATH`. Relative directory names (not starting with
    ‘`/`’) are considered to be relative to the top level of the
    package being built. All directories are made absolute, first
    starting from the top level *build* tree, then from the
    *source* tree. For instance ‘`./testsuite
    AUTOTEST_PATH=tests:bin`’ for a `/src/foo-1.0` source package built
    in `/tmp/foo` results in ‘`/tmp/foo/tests:/tmp/foo/bin`’ and
    then ‘`/src/foo-1.0/tests:/src/foo-1.0/bin`’ being prepended to
    `PATH`.

‘`number`’

‘`number-number`’

‘`number-`’

‘`-number`’
:   Add the corresponding test groups, with obvious semantics, to the
    selection.

‘`--keywords=keywords`’

‘`-k keywords`’
:   Add to the selection the test groups with title or keywords (arguments
    to `AT_SETUP` or `AT_KEYWORDS`) that match *all* keywords
    of the comma separated list keywords, case-insensitively. Use
    ‘`!`’ immediately before the keyword to invert the selection for this
    keyword. By default, the keywords match whole words; enclose them in
    ‘`.*`’ to also match parts of words.

    For example, running

    ```
    ./testsuite -k 'autoupdate,.*FUNC.*'
    ```

    selects all tests tagged ‘`autoupdate`’ *and* with tags
    containing ‘`FUNC`’ (as in ‘`AC_CHECK_FUNC`’, ‘`AC_FUNC_ALLOCA`’,
    etc.), while

    ```
    ./testsuite -k '!autoupdate' -k '.*FUNC.*'
    ```

    selects all tests not tagged ‘`autoupdate`’ *or* with tags
    containing ‘`FUNC`’.

‘`--errexit`’

‘`-e`’
:   If any test fails, immediately abort testing. This implies
    `--debug`: post test group clean up, and top-level logging
    are inhibited. This option is meant for the full test
    suite, it is not really useful for generated debugging scripts.
    If the testsuite is run in parallel mode using `--jobs`,
    then concurrently running tests will finish before exiting.

‘`--verbose`’

‘`-v`’
:   Force more verbosity in the detailed output of what is being done. This
    is the default for debugging scripts.

‘`--color`’

‘`--color[=never|auto|always]`’
:   Enable colored test results. Without an argument, or with ‘`always`’,
    test results will be colored. With ‘`never`’, color mode is turned
    off. Otherwise, if either the macro `AT_COLOR_TESTS` is used by
    the testsuite author, or the argument ‘`auto`’ is given, then test
    results are colored if standard output is connected to a terminal.

‘`--debug`’

‘`-d`’
:   Do not remove the files after a test group was performed—but they are
    still removed *before*, therefore using this option is sane when
    running several test groups. Create debugging scripts. Do not
    overwrite the top-level
    log (in order to preserve a supposedly existing full log file). This is
    the default for debugging scripts, but it can also be useful to debug
    the testsuite itself.

‘`--recheck`’
:   Add to the selection all test groups that failed or passed unexpectedly
    during the last non-debugging test run.

‘`--trace`’

‘`-x`’
:   Trigger shell tracing of the test groups.

Besides these options accepted by every Autotest testsuite, the
testsuite author might have added package-specific options
via the `AT_ARG_OPTION` and `AT_ARG_OPTION_ARG` macros
(see [Writing `testsuite.at`](#Writing-Testsuites)); refer to `testsuite --help` and
the package documentation for details.

---

Previous: [Running `testsuite` Scripts](#testsuite-Invocation), Up: [Generating Test Suites with Autotest](#Using-Autotest)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 19.4 Making `testsuite` Scripts [¶](#Making-testsuite-Scripts-1)

For putting Autotest into movement, you need some configuration and
makefile machinery. We recommend, at least if your package uses deep or
shallow hierarchies, that you use `tests/` as the name of the
directory holding all your tests and their makefile. Here is a
check list of things to do, followed by an example, taking into
consideration whether you are also using Automake.

* Make sure to create the file `package.m4`, which defines the
  identity of the package. It must define `AT_PACKAGE_STRING`, the
  full signature of the package, and `AT_PACKAGE_BUGREPORT`, the
  address to which bug reports should be sent. For sake of completeness,
  we suggest that you also define `AT_PACKAGE_NAME`,
  `AT_PACKAGE_TARNAME`, `AT_PACKAGE_VERSION`, and
  `AT_PACKAGE_URL`.
  See [Initializing `configure`](#Initializing-configure), for a description of these variables.
  Be sure to distribute `package.m4` and to put it into the source
  hierarchy: the test suite ought to be shipped! See below for an example.
* Invoke `AC_CONFIG_TESTDIR` in your `configure.ac`.

  Macro: **AC\_CONFIG\_TESTDIR**(directory, [test-path = directory]) [¶](#index-AC_005fCONFIG_005fTESTDIR-1)
  :   An Autotest test suite is to be configured in directory. This
      macro causes `directory/atconfig` to be created by
      `config.status` and sets the default `AUTOTEST_PATH` to
      test-path (see [Running `testsuite` Scripts](#testsuite-Invocation)).
* Still within `configure.ac`, as appropriate, ensure that some
  `AC_CONFIG_FILES` command includes substitution for
  `tests/atlocal`.
* Also within your `configure.ac`, arrange for the `AUTOM4TE`
  variable to be set.
* The appropriate `Makefile` should be modified so the validation in
  your package is triggered by ‘`make check`’.

The following example demonstrates the above checklist, first by
assuming that you are using Automake (see below for tweaks to make to
get the same results without Automake). Begin by adding the following
lines to your `configure.ac`:

```
# Initialize the test suite.
AC_CONFIG_TESTDIR([tests])
AC_CONFIG_FILES([tests/Makefile tests/atlocal])
AM_MISSING_PROG([AUTOM4TE], [autom4te])
```

Next, add the following lines to your `tests/Makefile.am`, in order
to link ‘`make check`’ with a validation suite.

```
$(srcdir)/package.m4: $(top_srcdir)/configure.ac
        printf >'$@' '%s\n' \
          '# Signature of the current package.' \
          'm4_define([AT_PACKAGE_NAME], [$(PACKAGE_NAME)])' \
          'm4_define([AT_PACKAGE_TARNAME], [$(PACKAGE_TARNAME)])' \
          'm4_define([AT_PACKAGE_VERSION], [$(PACKAGE_VERSION)])' \
          'm4_define([AT_PACKAGE_STRING], [$(PACKAGE_STRING)])' \
          'm4_define([AT_PACKAGE_URL], [$(PACKAGE_URL)])' \
          'm4_define([AT_PACKAGE_BUGREPORT], [$(PACKAGE_BUGREPORT)])'

EXTRA_DIST = testsuite.at $(srcdir)/package.m4 $(TESTSUITE) atlocal.in
TESTSUITE = $(srcdir)/testsuite

check-local: atconfig atlocal $(TESTSUITE)
        $(SHELL) '$(TESTSUITE)' $(TESTSUITEFLAGS)

installcheck-local: atconfig atlocal $(TESTSUITE)
        $(SHELL) '$(TESTSUITE)' AUTOTEST_PATH='$(bindir)' \
          $(TESTSUITEFLAGS)

clean-local:
        test ! -f '$(TESTSUITE)' || \
         $(SHELL) '$(TESTSUITE)' --clean

AUTOTEST = $(AUTOM4TE) --language=autotest
$(TESTSUITE): $(srcdir)/testsuite.at $(srcdir)/package.m4
        $(AUTOTEST) -I '$(srcdir)' -o $@.tmp $@.at
        mv $@.tmp $@
```

Note that the built testsuite is distributed; this is necessary because
users might not have Autoconf installed, and thus would not be able to
rebuild it. Likewise, the use of Automake’s `AM_MISSING_PROG` will
arrange for the definition of `$AUTOM4TE` within the Makefile to
provide the user with
a nicer error message if they modify a source file to the testsuite, and
accidentally trigger the rebuild rules.

You might want to list explicitly the dependencies, i.e., the list of
the files `testsuite.at` includes.

If you don’t use Automake, you should make the following tweaks. In
your `configure.ac`, replace the `AM_MISSING_PROG` line above
with `AC_PATH_PROG([AUTOM4TE], [autom4te], [false])`. You are
welcome to also try using the `missing` script from the Automake
project instead of `false`, to try to get a nicer error message
when the user modifies prerequisites but did not have Autoconf
installed, but at that point you may be better off using Automake.
Then, take the code suggested above for `tests/Makefile.am` and
place it in your `tests/Makefile.in` instead. Add code to your
`tests/Makefile.in` to ensure that `$(EXTRA_DIST)` files are
distributed, as well as adding the following additional lines to prepare
the set of needed Makefile variables:

```
subdir = tests
PACKAGE_NAME = @PACKAGE_NAME@
PACKAGE_TARNAME = @PACKAGE_TARNAME@
PACKAGE_VERSION = @PACKAGE_VERSION@
PACKAGE_STRING = @PACKAGE_STRING@
PACKAGE_BUGREPORT = @PACKAGE_BUGREPORT@
PACKAGE_URL = @PACKAGE_URL@
AUTOM4TE = @AUTOM4TE@

atconfig: $(top_builddir)/config.status
        cd $(top_builddir) && \
           $(SHELL) ./config.status $(subdir)/$@

atlocal: $(srcdir)/atlocal.in $(top_builddir)/config.status
        cd $(top_builddir) && \
           $(SHELL) ./config.status $(subdir)/$@
```

Using the above example (with or without Automake), and assuming you
were careful to not initialize ‘`TESTSUITEFLAGS`’ within your
makefile, you can now fine-tune test suite execution at runtime by
altering this variable, for example:

```
make check TESTSUITEFLAGS='-v -d -x 75 -k AC_PROG_CC CFLAGS=-g'
```

---

Next: [History of Autoconf](#History), Previous: [Generating Test Suites with Autotest](#Using-Autotest), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 20 Frequent Autoconf Questions, with answers [¶](#Frequent-Autoconf-Questions_002c-with-answers)

Several questions about Autoconf come up occasionally. Here some of them
are addressed.

* [Distributing `configure` Scripts](#Distributing)
* [Why Require GNU M4?](#Why-GNU-M4)
* [How Can I Bootstrap?](#Bootstrapping)
* [Why Not Imake?](#Why-Not-Imake)
* [How Do I `#define` Installation Directories?](#Defining-Directories)
* [What is `autom4te.cache`?](#Autom4te-Cache)
* [Header Present But Cannot Be Compiled](#Present-But-Cannot-Be-Compiled)
* [Expanded Before Required](#Expanded-Before-Required)
* [Debugging `configure` scripts](#Debugging)

---

Next: [Why Require GNU M4?](#Why-GNU-M4), Up: [Frequent Autoconf Questions, with answers](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 20.1 Distributing `configure` Scripts [¶](#Distributing-configure-Scripts)

```
What are the restrictions on distributing configure
scripts that Autoconf generates?  How does that affect my
programs that use them?
```

There are no restrictions on how the configuration scripts that Autoconf
produces may be distributed or used. In Autoconf version 1, they were
covered by the GNU General Public License. We still encourage
software authors to distribute their work under terms like those of the
GPL, but doing so is not required to use Autoconf.

Of the other files that might be used with `configure`,
`config.h.in` is under whatever copyright you use for your
`configure.ac`. `config.sub` and `config.guess` have an
exception to the GPL when they are used with an Autoconf-generated
`configure` script, which permits you to distribute them under the
same terms as the rest of your package. `install-sh` is from the X
Consortium and is not copyrighted.

---

Next: [How Can I Bootstrap?](#Bootstrapping), Previous: [Distributing `configure` Scripts](#Distributing), Up: [Frequent Autoconf Questions, with answers](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 20.2 Why Require GNU M4? [¶](#Why-Require-GNU-M4_003f)

```
Why does Autoconf require GNU M4?
```

Many M4 implementations have hard-coded limitations on the size and
number of macros that Autoconf exceeds. They also lack several
builtin macros that it would be difficult to get along without in a
sophisticated application like Autoconf, including:

```
m4_builtin
m4_indir
m4_bpatsubst
__file__
__line__
```

Autoconf requires version 1.4.8 or later of GNU M4.
It works better with version 1.4.16 or later.

Since only software maintainers need to use Autoconf, and since GNU
M4 is simple to configure and install, it seems reasonable to require
GNU M4 to be installed also. Many maintainers of GNU and
other free software already have most of the GNU utilities
installed, since they prefer them.

---

Next: [Why Not Imake?](#Why-Not-Imake), Previous: [Why Require GNU M4?](#Why-GNU-M4), Up: [Frequent Autoconf Questions, with answers](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 20.3 How Can I Bootstrap? [¶](#How-Can-I-Bootstrap_003f)

```
If Autoconf requires GNU M4 and GNU M4 has an Autoconf
configure script, how do I bootstrap?  It seems like a chicken
and egg problem!
```

This is a misunderstanding. Although GNU M4 does come with a
`configure` script produced by Autoconf, Autoconf is not required
in order to run the script and install GNU M4. Autoconf is only
required if you want to change the M4 `configure` script, which few
people have to do (mainly its maintainer).

---

Next: [How Do I `#define` Installation Directories?](#Defining-Directories), Previous: [How Can I Bootstrap?](#Bootstrapping), Up: [Frequent Autoconf Questions, with answers](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 20.4 Why Not Imake? [¶](#Why-Not-Imake_003f)

```
Why not use Imake instead of configure scripts?
```

Several people have written addressing this question, so
adaptations of their explanations are included here.

The following answer is based on one written by Richard Pixley:

> Autoconf generated scripts frequently work on machines that it has
> never been set up to handle before. That is, it does a good job of
> inferring a configuration for a new system. Imake cannot do this.
>
> Imake uses a common database of host specific data. For X11, this makes
> sense because the distribution is made as a collection of tools, by one
> central authority who has control over the database.
>
> GNU tools are not released this way. Each GNU tool has a
> maintainer; these maintainers are scattered across the world. Using a
> common database would be a maintenance nightmare. Autoconf may appear
> to be this kind of database, but in fact it is not. Instead of listing
> host dependencies, it lists program requirements.
>
> If you view the GNU suite as a collection of native tools, then the
> problems are similar. But the GNU development tools can be
> configured as cross tools in almost any host+target permutation. All of
> these configurations can be installed concurrently. They can even be
> configured to share host independent files across hosts. Imake doesn’t
> address these issues.
>
> Imake templates are a form of standardization. The GNU coding
> standards address the same issues without necessarily imposing the same
> restrictions.

Here is some further explanation, written by Per Bothner:

> One of the advantages of Imake is that it is easy to generate large
> makefiles using the ‘`#include`’ and macro mechanisms of `cpp`.
> However, `cpp` is not programmable: it has limited conditional
> facilities, and no looping. And `cpp` cannot inspect its
> environment.
>
> All of these problems are solved by using `sh` instead of
> `cpp`. The shell is fully programmable, has macro substitution,
> can execute (or source) other shell scripts, and can inspect its
> environment.

Paul Eggert elaborates more:

> With Autoconf, installers need not assume that Imake itself is already
> installed and working well. This may not seem like much of an advantage
> to people who are accustomed to Imake. But on many hosts Imake is not
> installed or the default installation is not working well, and requiring
> Imake to install a package hinders the acceptance of that package on
> those hosts. For example, the Imake template and configuration files
> might not be installed properly on a host, or the Imake build procedure
> might wrongly assume that all source files are in one big directory
> tree, or the Imake configuration might assume one compiler whereas the
> package or the installer needs to use another, or there might be a
> version mismatch between the Imake expected by the package and the Imake
> supported by the host. These problems are much rarer with Autoconf,
> where each package comes with its own independent configuration
> processor.
>
> Also, Imake often suffers from unexpected interactions between
> `make` and the installer’s C preprocessor. The fundamental problem
> here is that the C preprocessor was designed to preprocess C programs,
> not makefiles. This is much less of a problem with Autoconf,
> which uses the general-purpose preprocessor M4, and where the
> package’s author (rather than the installer) does the preprocessing in a
> standard way.

Finally, Mark Eichin notes:

> Imake isn’t all that extensible, either. In order to add new features to
> Imake, you need to provide your own project template, and duplicate most
> of the features of the existing one. This means that for a sophisticated
> project, using the vendor-provided Imake templates fails to provide any
> leverage—since they don’t cover anything that your own project needs
> (unless it is an X11 program).
>
> On the other side, though:
>
> The one advantage that Imake has over `configure`:
> `Imakefile` files tend to be much shorter (likewise, less redundant)
> than `Makefile.in` files. There is a fix to this, however—at least
> for the Kerberos V5 tree, we’ve modified things to call in common
> `post.in` and `pre.in` makefile fragments for the
> entire tree. This means that a lot of common things don’t have to be
> duplicated, even though they normally are in `configure` setups.

---

Next: [What is `autom4te.cache`?](#Autom4te-Cache), Previous: [Why Not Imake?](#Why-Not-Imake), Up: [Frequent Autoconf Questions, with answers](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 20.5 How Do I `#define` Installation Directories? [¶](#How-Do-I-_0023define-Installation-Directories_003f)

```
My program needs library files, installed in datadir and
similar.  If I use
```

```
AC_DEFINE_UNQUOTED([DATADIR], [$datadir],
  [Define to the read-only architecture-independent
   data directory.])
```

```
I get
```

```
#define DATADIR "${prefix}/share"
```

As already explained, this behavior is on purpose, mandated by the
GNU Coding Standards, see [Installation Directory Variables](#Installation-Directory-Variables). There are several means to achieve a similar goal:

* Do not use `AC_DEFINE` but use your makefile to pass the
  actual value of `datadir` via compilation flags.
  See [Installation Directory Variables](#Installation-Directory-Variables), for the details.
* This solution can be simplified when compiling a program: you may either
  extend the `CPPFLAGS`:

  ```
  CPPFLAGS = -DDATADIR='"$(datadir)"' @CPPFLAGS@
  ```

  If you are using Automake, you should use `AM_CPPFLAGS` instead:

  ```
  AM_CPPFLAGS = -DDATADIR='"$(datadir)"'
  ```

  Alternatively, create a dedicated header file:

  ```
  DISTCLEANFILES = myprog-paths.h
  myprog-paths.h: Makefile
          printf '%s\n' '#define DATADIR "$(datadir)"' >$@
  ```

  The Gnulib module ‘`configmake`’ provides such a header with all the
  standard directory variables defined, see [configmake](https://www.gnu.org/software/gnulib/manual/gnulib.html#configmake) in GNU
  Gnulib.
* Use `AC_DEFINE` but have `configure` compute the literal
  value of `datadir` and others. Many people have wrapped macros to
  automate this task; for an example, see the macro `AC_DEFINE_DIR` from
  the [Autoconf Macro
  Archive](https://www.gnu.org/software/autoconf-archive/).

  This solution does not conform to the GNU Coding Standards.
* Note that all the previous solutions hard wire the absolute name of
  these directories in the executables, which is not a good property. You
  may try to compute the names relative to `prefix`, and try to
  find `prefix` at runtime, this way your package is relocatable.

---

Next: [Header Present But Cannot Be Compiled](#Present-But-Cannot-Be-Compiled), Previous: [How Do I `#define` Installation Directories?](#Defining-Directories), Up: [Frequent Autoconf Questions, with answers](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 20.6 What is `autom4te.cache`? [¶](#What-is-autom4te_002ecache_003f)

```
What is this directory autom4te.cache?  Can I safely remove it?
```

In the GNU Build System, `configure.ac` plays a central
role and is read by many tools: `autoconf` to create
`configure`, `autoheader` to create `config.h.in`,
`automake` to create `Makefile.in`, `autoscan` to
check the completeness of `configure.ac`, `autoreconf` to
check the GNU Build System components that are used. To
“read `configure.ac`” actually means to compile it with M4,
which can be a long process for complex `configure.ac`.

This is why all these tools, instead of running directly M4, invoke
`autom4te` (see [Invoking `autom4te`](#autom4te-Invocation)) which, while answering to
a specific demand, stores additional information in
`autom4te.cache` for future runs. For instance, if you run
`autoconf`, behind the scenes, `autom4te` also
stores information for the other tools, so that when you invoke
`autoheader` or `automake` etc., reprocessing
`configure.ac` is not needed. The speed up is frequently 30%,
and is increasing with the size of `configure.ac`.

But it is and remains being simply a cache: you can safely remove it.

  

```
Can I permanently get rid of it?
```

The creation of this cache can be disabled from
`~/.autom4te.cfg`, see [Customizing `autom4te`](#Customizing-autom4te), for more
details. You should be aware that disabling the cache slows down the
Autoconf test suite by 40%. The more GNU Build System
components are used, the more the cache is useful; for instance
running ‘`autoreconf -f`’ on the Core Utilities is twice slower without
the cache *although `--force` implies that the cache is
not fully exploited*, and eight times slower than without
`--force`.

---

Next: [Expanded Before Required](#Expanded-Before-Required), Previous: [What is `autom4te.cache`?](#Autom4te-Cache), Up: [Frequent Autoconf Questions, with answers](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 20.7 Header Present But Cannot Be Compiled [¶](#Header-Present-But-Cannot-Be-Compiled)

The most important guideline to bear in mind when checking for
features is to mimic as much as possible the intended use.
Unfortunately, old versions of `AC_CHECK_HEADER` and
`AC_CHECK_HEADERS` failed to follow this idea, and called
the preprocessor, instead of the compiler, to check for headers. As a
result, incompatibilities between headers went unnoticed during
configuration, and maintainers finally had to deal with this issue
elsewhere.

The transition began with Autoconf 2.56. As of Autoconf 2.64 both
checks are performed, and `configure` complains loudly if the
compiler and the preprocessor do not agree. However, only the compiler
result is considered. As of Autoconf 2.70, only the compiler check is
performed.

Consider the following example:

```
$ cat number.h
typedef int number;
$ cat pi.h
const number pi = 3;
$ cat configure.ac
AC_INIT([Example], [1.0], [bug-example@example.org])
AC_CHECK_HEADERS([pi.h])
$ autoconf -Wall
$ ./configure CPPFLAGS='-I.'
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables...
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether the compiler supports GNU C... yes
checking whether gcc accepts -g... yes
checking for gcc option to enable C23 features... -std=gnu23
checking for sys/types.h... yes
checking for sys/stat.h... yes
checking for strings.h... yes
checking for inttypes.h... yes
checking for stdint.h... yes
checking for unistd.h... yes
checking for pi.h... no
```

The proper way to handle this case is using the fourth argument
(see [Generic Header Checks](#Generic-Headers)):

```
$ cat configure.ac
AC_INIT([Example], [1.0], [bug-example@example.org])
AC_CHECK_HEADERS([number.h pi.h], [], [],
[[#ifdef HAVE_NUMBER_H
# include <number.h>
#endif
]])
$ autoconf -Wall
$ ./configure CPPFLAGS='-I.'
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables...
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether the compiler supports GNU C... yes
checking whether gcc accepts -g... yes
checking for gcc option to enable C23 features... -std=gnu23
checking for number.h... yes
checking for pi.h... yes
```

See [Particular Header Checks](#Particular-Headers), for a list of headers with their
prerequisites.

---

Next: [Debugging `configure` scripts](#Debugging), Previous: [Header Present But Cannot Be Compiled](#Present-But-Cannot-Be-Compiled), Up: [Frequent Autoconf Questions, with answers](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 20.8 Expanded Before Required [¶](#Expanded-Before-Required-1)

Older versions of Autoconf silently built files with incorrect ordering
between dependent macros if an outer macro first expanded, then later
indirectly required, an inner macro. Starting with Autoconf 2.64, this
situation no longer generates out-of-order code, but results in
duplicate output and a syntax warning:

```
$ cat configure.ac
⇒AC_DEFUN([TESTA], [[echo in A
⇒if test -n "$SEEN_A" ; then echo duplicate ; fi
⇒SEEN_A=:]])
⇒AC_DEFUN([TESTB], [AC_REQUIRE([TESTA])[echo in B
⇒if test -z "$SEEN_A" ; then echo bug ; fi]])
⇒AC_DEFUN([TESTC], [AC_REQUIRE([TESTB])[echo in C]])
⇒AC_DEFUN([OUTER], [[echo in OUTER]
⇒TESTA
⇒TESTC])
⇒AC_INIT
⇒OUTER
⇒AC_OUTPUT
$ autoconf
⇒configure.ac:11: warning: AC_REQUIRE:
⇒ 'TESTA' was expanded before it was required
⇒configure.ac:4: TESTB is expanded from...
⇒configure.ac:6: TESTC is expanded from...
⇒configure.ac:7: OUTER is expanded from...
⇒configure.ac:11: the top level
```

To avoid this warning, decide what purpose the macro in question serves.
If it only needs to be expanded once (for example, if it provides
initialization text used by later macros), then the simplest fix is to
change the macro to be declared with `AC_DEFUN_ONCE`
(see [One-Shot Macros](#One_002dShot-Macros)), although this only works in Autoconf 2.64 and
newer. A more portable fix is to change all
instances of direct calls to instead go through `AC_REQUIRE`
(see [Prerequisite Macros](#Prerequisite-Macros)). If, instead, the macro is parameterized
by arguments or by the current definition of other macros in the m4
environment, then the macro should always be directly expanded instead
of required.

For another case study, consider this example trimmed down from an
actual package. Originally, the package contained shell code and
multiple macro invocations at the top level of `configure.ac`:

```
AC_DEFUN([FOO], [AC_COMPILE_IFELSE([...])])
foobar=
AC_PROG_CC
FOO
```

but that was getting complex, so the author wanted to offload some of
the text into a new macro in another file included via
`aclocal.m4`. The naïve approach merely wraps the text in a new
macro:

```
AC_DEFUN([FOO], [AC_COMPILE_IFELSE([...])])
AC_DEFUN([BAR], [
foobar=
AC_PROG_CC
FOO
])
BAR
```

With older versions of Autoconf, the setting of ‘`foobar=`’ occurs
before the single compiler check, as the author intended. But with
Autoconf 2.64, this issues the “expanded before it was required”
warning for `AC_PROG_CC`, and outputs two copies of the compiler
check, one before ‘`foobar=`’, and one after. To understand why this
is happening, remember that the use of `AC_COMPILE_IFELSE` includes
a call to `AC_REQUIRE([AC_PROG_CC])` under the hood. According to
the documented semantics of `AC_REQUIRE`, this means that
`AC_PROG_CC` *must* occur before the body of the outermost
`AC_DEFUN`, which in this case is `BAR`, thus preceding the
use of ‘`foobar=`’. The older versions of Autoconf were broken with
regards to the rules of `AC_REQUIRE`, which explains why the code
changed from one over to two copies of `AC_PROG_CC` when upgrading
autoconf. In other words, the author was unknowingly relying on a bug
exploit to get the desired results, and that exploit broke once the bug
was fixed.

So, what recourse does the author have, to restore their intended
semantics of setting ‘`foobar=`’ prior to a single compiler check,
regardless of whether Autoconf 2.63 or 2.64 is used? One idea is to
remember that only `AC_DEFUN` is impacted by `AC_REQUIRE`;
there is always the possibility of using the lower-level
`m4_define`:

```
AC_DEFUN([FOO], [AC_COMPILE_IFELSE([...])])
m4_define([BAR], [
foobar=
AC_PROG_CC
FOO
])
BAR
```

This works great if everything is in the same file. However, it does
not help in the case where the author wants to have `aclocal`
find the definition of `BAR` from its own file, since
`aclocal` requires the use of `AC_DEFUN`. In this case, a
better fix is to recognize that if `BAR` also uses
`AC_REQUIRE`, then there will no longer be direct expansion prior
to a subsequent require. Then, by creating yet another helper macro,
the author can once again guarantee a single invocation of
`AC_PROG_CC`, which will still occur after `foobar=`. The
author can also use `AC_BEFORE` to make sure no other macro
appearing before `BAR` has triggered an unwanted expansion of
`AC_PROG_CC`.

```
AC_DEFUN([FOO], [AC_COMPILE_IFELSE([...])])
AC_DEFUN([BEFORE_CC], [
foobar=
])
AC_DEFUN([BAR], [
AC_BEFORE([$0], [AC_PROG_CC])dnl
AC_REQUIRE([BEFORE_CC])dnl
AC_REQUIRE([AC_PROG_CC])dnl
FOO
])
BAR
```

---

Previous: [Expanded Before Required](#Expanded-Before-Required), Up: [Frequent Autoconf Questions, with answers](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
