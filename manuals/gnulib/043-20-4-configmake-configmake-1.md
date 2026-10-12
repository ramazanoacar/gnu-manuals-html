### 20.4 configmake [¶](#configmake-1)

The `configmake` module builds a C include file named
`configmake.h` containing the usual installation directory
values; for example, those specified by `--prefix` or
`--libdir` to configure. Each variable is given a `#define`
with an all-uppercase macro name, such as `PREFIX` and
`LIBDIR`. (Automake cannot create this file directly because the
user might override directory values at `make` time.)

Specifically, the module retrieves values of the variables through
`configure` followed by `make`, not directly through
`configure`, so that a user who sets some of these variables
consistently on the `make` command line gets correct results.

One advantage of this approach, compared to the classical approach of
adding `-DLIBDIR=\"$(libdir)\"` etc. to `AM_CPPFLAGS`, is
that it protects against the use of undefined variables. That is, if,
say, `$(libdir)` is not set in the Makefile, `LIBDIR` is not
defined by this module, and code using `LIBDIR` gives a
compilation error.

Another advantage is that `make` output is shorter.

For the complete list of variables which are `#define`d this way,
see the file `gnulib/modules/configmake`, or inspect your
resulting gnulib Makefile.

---

Next: [manywarnings](#manywarnings), Previous: [configmake](#configmake), Up: [Build Infrastructure Modules](#Build-Infrastructure-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 20.5 warnings [¶](#warnings-1)

The `warnings` module allows regularly building a package with more
GCC warnings than the default warnings emitted by GCC. It is often used
indirectly through the `manywarnings` module
(see [manywarnings](#manywarnings)).

It provides the following functionality:

* You can select some warning options, such as ‘`-Wall`’, to be enabled
  whenever building with a GCC version that supports these options. The
  user can choose to override these warning options by providing the
  opposite options in the `CFLAGS` variable at configuration time.
* You can make these warnings apply to selected directories only. In
  projects where subprojects are maintained by different people, or where
  parts of the source code are imported from external sources (for example
  from gnulib), it is useful to apply different warning options to
  different directories.
* It lets you use ‘`-Werror`’ at ‘`make distcheck`’ time, to verify
  that on the maintainer’s system, no warnings remain. (Note that use of
  ‘`-Werror`’ in `CFLAGS` does not work in general, because it may
  break autoconfiguration.)
* Similarly, it lets you use ‘`-Werror`’ when the builder runs
  `configure` with an option such as
  `--enable-gcc-warnings`.

To use this module, you need the following:

1. In `configure.ac`, use for example

   ```
   gl_WARN_ADD([-Wall], [WARN_CFLAGS])
   gl_WARN_ADD([-Wpointer-arith], [WARN_CFLAGS])
   ```
2. In the directories which shall use `WARN_CFLAGS`, use it in the
   definition of `AM_CFLAGS`, like this:

   ```
   AM_CFLAGS = $(WARN_CFLAGS)
   ```

   Note that the `AM_CFLAGS` is used in combination with `CFLAGS`
   and before `CFLAGS` in build rules emitted by Automake. This allows
   the user to provide `CFLAGS` that override the `WARN_CFLAGS`.

‘`gl_WARN_ADD([-Werror])`’ is intended for developers, and should be
avoided in contexts where it would affect ordinary installation builds. The
warnings emitted by GCC depend, to some extent, on the contents of the
system header files, on the size and signedness of built-in types, etc.
Use of ‘`-Werror`’ would cause frustration to all users on platforms
that the maintainer has not tested before the release. It is better if
‘`-Werror`’ is off by default, and is enabled only by developers.
For example, ‘`-Werror`’ could affect ‘`make distcheck`’ or
‘`configure --enable-gcc-warnings`’ as mentioned above.

---

Next: [Running self-tests under valgrind](#Running-self_002dtests-under-valgrind), Previous: [warnings](#warnings), Up: [Build Infrastructure Modules](#Build-Infrastructure-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 20.6 manywarnings [¶](#manywarnings-1)

The `manywarnings` module enables many GCC warnings for your
package. Here is an example use:

```
AC_ARG_ENABLE([gcc-warnings],
  [AS_HELP_STRING([[--enable-gcc-warnings[=TYPE]]],
    [control generation of GCC warnings.  The TYPE 'no' disables
     warnings; 'yes' (default) generates cheap warnings;
     'expensive' in addition generates expensive warnings.])])

AS_IF([test "$enable_gcc_warnings" != no],
  [
   # Set up the list of unwanted warning options.
   nw=
   if test "$enable_gcc_warnings" != expensive; then
     nw="$nw -fanalyzer"
   fi
   nw="$nw -Wbad-function-cast" # Casting a function's result is not more
                                # dangerous than casting any other value.
   nw="$nw -Winline"            # It's OK to not inline.
   nw="$nw -Wsign-compare"      # Too many false alarms.
   nw="$nw -Wsystem-headers"    # Don't warn in system headers.

   # Setup the list of meaningful warning options for the C compiler.
   # The list comes from manywarnings.m4. Warning options that are not
   # generally meaningful have already been filtered out (cf.
   # build-aux/gcc-warning.spec).
   gl_MANYWARN_ALL_GCC([possible_warning_options])

   # Compute the list of warning options that are desired.
   gl_MANYWARN_COMPLEMENT([desired_warning_options],
                          [$possible_warning_options], [$nw])
   # Compute the list of remaining undesired warning options.
   # Namely those, that were not in manywarnings.m4 because they were
   # already listed in build-aux/gcc-warning.spec; this includes those
   # that are implied by -Wall.
   gl_MANYWARN_COMPLEMENT([remaining_undesired_warning_options],
                          [$nw], [$possible_warning_options])

   # Add the desired warning options to WARN_CFLAGS.
   for w in $desired_warning_options; do
     gl_WARN_ADD([$w])
   done

   # Add the opposites of the remaining undesired warning options to
   # WARN_CFLAGS.
   for w in `echo "$remaining_undesired_warning_options" | sed -e 's/-W/-Wno-/g'`; do
     gl_WARN_ADD([$w])
   done
])
```

This module sets up many GCC warning options.

When you use it for the first time, it is common practice to do it as
follows:

* Start with the newest major release of GCC.
  This will save you time, because some warning options produce many false
  alarms with older versions of GCC.
* Consider the platforms commonly used when enabling GCC warnings.
  This includes not only target architectures and operating systems, but also
  optimization options, which can greatly affect the warnings generated.
  Makefiles generated by `configure` default to `-O2` optimization.
  If you also commonly build with `-O0` or other optimization options,
  you can compile again with those options.
  Using more optimizations catches more bugs, because the compiler does
  a better static analysis of the program when optimizing more.
  Also, some warning options that diagnose suboptimal code generation,
  such as `-Winline`, are not effective when not optimizing.
  On the other hand, if it’s frequent to build the package with warnings but
  without optimizations, for debugging purposes, then you don’t want to see
  undesired warnings in these phases of development either.
* Compile the package with an empty `nw` value, that is, with all
  possible warnings enabled.
* Then you will go through the list of warnings.
  Since there are likely many warnings, the first time, it’s a good idea
  to sort them by warning option first:

  ```
  $ grep warning: make-output.log \
    | sed -e 's/^\(.*\) \[\(-W.*\)\]$/\2  \1/' | sort -k1
  ```
* You will likely deactivate warnings that occur often and don’t point to
  mistakes in the code, by adding them to the ‘`nw`’ variable, then
  reconfiguring and recompiling.
  When warnings point to real mistakes and bugs in the code, you will of
  course not disable them but fix your code to silence the warning
  instead.

  Many GCC warning options usually don’t point to mistakes in the code;
  these warnings enforce a certain programming style.
  It is a project management decision whether you want your code to follow
  any of these styles.
  Note that some of these programming styles are conflicting.
  You cannot have them all; you have to choose among them.

  When a warning option pinpoints real bugs occasionally, but it also
  whines about a few code locations which are fine, we recommend to leave
  the warning option enabled.
  Whether you then live with the remaining few warnings, or choose to
  disable them one-by-one through
  `#pragma GCC diagnostic ignored "option"`
  (see [Diagnostic Pragmas](https://gcc.gnu.org/onlinedocs/gcc/Diagnostic-Pragmas.html#Diagnostic-Pragmas) in Using the GNU Compiler Collection,
  <https://gcc.gnu.org/onlinedocs/gcc/Diagnostic-Pragmas.html>),
  is again a project management decision.

When a new major version of GCC is released, the Gnulib maintainers add
the newly available warning options into the `gl_MANYWARN_ALL_GCC`
macro.
You will then enjoy the benefits of the new warnings, simply by updating
to the newest Gnulib.
If some of the new warnings are undesired, you can add them to the
‘`nw`’ variable, as described above.

Comments on particular warning flags:

‘`-fanalyzer`’
:   The `manywarnings` module by default uses GCC’s
    `-fanalyzer` option, as this issues some useful warnings.
    (It can also help GCC generate better code.)
    However, `-fanalyzer` can greatly slow down compilation,
    and in programs with large modules it can be so slow as to be unusable,
    so it is common for `configure` to disable it unless
    `configure` is given an option like
    `--enable-gcc-warnings=expensive`.

‘`-fstrict-aliasing`’
:   Although the `manywarnings` module does not enable GCC’s
    `-fstrict-aliasing` option, it is enabled by default if you
    compile with `-O2` or higher optimization, and can help GCC
    generate better warnings.

‘`-Wanalyzer-malloc-leak`’
:   The `-fanalyzer` option generates many false alarms about
    `malloc` leaks, which `manywarnings` suppresses by also
    using `-Wno-analyzer-malloc-leak`.

‘`-fstrict-flex-arrays`’
:   The `manywarnings` module by default uses GCC’s
    `-fstrict-flex-arrays` option if available, so that GCC can
    warn about nonportable usage of flexible array members.
    In a few cases this can help GCC generate better code,
    so it is not strictly a warning option.

‘`-Wsign-compare`’
:   GCC and Clang generate too many false alarms with `-Wsign-compare`,
    and we don’t recommend that warning. You can disable it by using
    `gl_WARN_ADD([-Wno-sign-compare])` as illustrated above.
    Programs using Gnulib generally don’t enable
    that warning when compiling Gnulib code. If you happen to find a real
    bug with that warning we’d like to know it.

---

Next: [Package version management](#Package-version-management), Previous: [manywarnings](#manywarnings), Up: [Build Infrastructure Modules](#Build-Infrastructure-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 20.7 Running self-tests under valgrind [¶](#Running-self_002dtests-under-valgrind-1)

For projects written in C or similar languages, running the self-tests
under Valgrind can reveal hard to find memory issues. Gnulib supports
two ways to make use of Valgrind: one that enables use of Valgrind at
configure time, when `configure` found it to be present; and one
at the discretion of the developer.

* [Using valgrind without developer intervention](#Using-valgrind-automatically)
* [Valgrind options](#Valgrind-options)
* [Using valgrind at the developer’s discretion](#Using-valgrind-manually)
* [How to use Valgrind with shell scripts](#Valgrind-and-shell-scripts)

---

Next: [Valgrind options](#Valgrind-options), Up: [Running self-tests under valgrind](#Running-self_002dtests-under-valgrind)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 20.7.1 Using valgrind without developer intervention [¶](#Using-valgrind-without-developer-intervention)

The `valgrind-tests` module searches for Valgrind at configure time
and declares the `LOG_VALGRIND` automake variable for use with
automake’s `LOG_COMPILER`.

After importing the `valgrind-tests` module to your project, you
use it by adding the following to the `Makefile.am` that runs the
self-tests:

```
LOG_COMPILER = $(LOG_VALGRIND)
```

This will run all self-checks under valgrind.

Replace `LOG_COMPILER` with `TESTS_ENVIRONMENT` if you are
using the old serial test harness. The parallel test harness has been
the default in automake since version 1.11.3, but if you are using an
older automake, or put ‘`serial-tests`’ in
‘`AM_INIT_AUTOMAKE`’/‘`AUTOMAKE_OPTIONS`’ you would still be using
the serial test harness.

If you desire a project-wide decision that valgrind is not enabled by
default, but still allow users to enable it with
`--enable-valgrind-tests` you may put the following in configure.ac
before gl\_INIT.

```
gl_VALGRIND_TESTS_DEFAULT_NO
```

---

Next: [Using valgrind at the developer’s discretion](#Using-valgrind-manually), Previous: [Using valgrind without developer intervention](#Using-valgrind-automatically), Up: [Running self-tests under valgrind](#Running-self_002dtests-under-valgrind)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 20.7.2 Valgrind options [¶](#Valgrind-options-1)

The `VALGRIND` variable holds the name of the valgrind binary and
some options passed to valgrind. You may provide additional options
that are passed to valgrind using the ‘`VALGRINDFLAGS`’ variable, for
example:

```
./configure VALGRINDFLAGS="--suppressions=~/local.supp"
```

Alternatively during build phase:

```
make check VALGRINDFLAGS="--suppressions=~/local.supp"
```

This is useful if you have a valgrind suppression files that are needed
to avoid triggering errors for known errors, typically in system
libraries.

The `VALGRIND` variable include options that are useful when
valgrind is run non-interactively through the test harness. The default
parameters are `-q` to silence the output,
`--error-exitcode=1` to cause valgrind errors to be treated as
fatal errors, and `--leak-check=full` to check for memory leaks.

These options can be controlled through the `DEFAULT_VALGRINDFLAGS`
variable. For example, when configuring the package:

```
./configure DEFAULT_VALGRINDFLAGS="--quiet"
```

Alternatively, during the build phase:

```
make check DEFAULT_VALGRINDFLAGS="--quiet"
```

That would have the effect of removing `--error-exitcode=1` and
`--leak-check=full` from the default options, thus causing any
valgrind errors to be silently ignored, instead of causing fatal test
failures.

As a developer you may use the variables in `configure.ac` before
calling `gl_INIT`, like this if your program has deeply-nested call
chains:

```
gl_EARLY
...
VALGRINDFLAGS="$VALGRINDFLAGS --num-callers=42"
...
gl_INIT
```

Note that any user-supplied `VALGRINDFLAGS` value is preserved,
which is usually what you want.

Finally, as a developer you may want to provide additional per-directory
options to valgrind and the `AM_VALGRINDFLAGS` variable can be used
for this. For example:

```
AM_VALGRINDFLAGS = --suppressions=$(srcdir)/local-valgrind.supp
LOG_COMPILER = $(LOG_VALGRIND)
```

---

Next: [How to use Valgrind with shell scripts](#Valgrind-and-shell-scripts), Previous: [Valgrind options](#Valgrind-options), Up: [Running self-tests under valgrind](#Running-self_002dtests-under-valgrind)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 20.7.3 Using valgrind at the developer’s discretion [¶](#Using-valgrind-at-the-developer_0027s-discretion)

In this approach, you define a `Makefile.am` variable ‘`VALGRIND`’
(or, more abstractly, ‘`CHECKER`’), that is usually set to empty.
When you have configured and built the package and you decide that you want
to run the tests with valgrind, you do so by modifying the definition of
‘`VALGRIND`’ in the Makefile.

---

Previous: [Using valgrind at the developer’s discretion](#Using-valgrind-manually), Up: [Running self-tests under valgrind](#Running-self_002dtests-under-valgrind)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 20.7.4 How to use Valgrind with shell scripts [¶](#How-to-use-Valgrind-with-shell-scripts)

It is not desirable to apply valgrind to shell scripts or other non-binaries,
because

* It is wasteful, and you usually don’t want to look for memory leaks in bash.
* On a bi-arch system, you may get an error message such as
  "valgrind: wrong executable class (eg. 32-bit instead of 64-bit)".

There are two ways to avoid this:

* Using the Automake parallel-tests feature, you can use the following instead:

  ```
  TEST_EXTENSIONS = .pl .sh
  LOG_COMPILER = $(LOG_VALGRIND)
  ```

  Then valgrind will only be used for the non-.sh and non-.pl tests.

  For old automake (before 1.11.3), you will need `AUTOMAKE_OPTIONS =
  parallel-tests` to enable the parallel test harness.
* You can make use of the `build-aux/run-test` script from Gnulib.
  Add these lines to your `Makefile.am`:

  ```
  LOG_COMPILER += $(SHELL) $(top_srcdir)/build-aux/run-test '$(LOG_VALGRIND)'
  ```

  Replace `LOG_COMPILER` with `TESTS_ENVIRONMENT` if you use the
  old serial test harness.

However, with this measure in place, binaries invoked through scripts will
not be invoked under valgrind. This can be solved by defining environment
variables in the `TESTS_ENVIRONMENT` variable that are then used by the
shell scripts. For example, add the following:

```
TESTS_ENVIRONMENT = VALGRIND='$(LOG_VALGRIND)'
```

And then modify the shell scripts to invoke the binary prefixed with
`$VALGRIND`.

---

Next: [VCS To ChangeLog](#VCS-To-ChangeLog), Previous: [Running self-tests under valgrind](#Running-self_002dtests-under-valgrind), Up: [Build Infrastructure Modules](#Build-Infrastructure-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 20.8 Package version management [¶](#Package-version-management-1)

The goals of a package maintainer,
when dealing with the version number of a package, are:

* Programs should identify themselves with that version number
  when invoked as `program --version`.
* Every package tarball should contain the version number, in up to four ways:
  + The tarball name should be `package-version.tar.gz`.
  + The tarball should unpack into a directory
    named `package-version`.
  + Programs built from that tarball should identify themselves
    when invoked as `program --version`.
  + Documentation (especially in PDF format) and man pages
    may contain the version number, as an important bit of meta information.
* The package maintainer should be able to
  easily set the version number before making a release,
  and doing so should be quick.
* When a contributor is
  building from the git repository (as opposed to a tarball),
  the version number should, by default, reflect the git commit,
  in order to avoid confusion w.r.t. official releases.

Here’s how the GNU Build System and Gnulib achieve these goals.

* [Setting the package version](#Setting-the-package-version)
* [Propagating the package version](#Propagating-the-package-version)
* [Using the package version during the build](#Using-the-package-version)

---

Next: [Propagating the package version](#Propagating-the-package-version), Up: [Package version management](#Package-version-management)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 20.8.1 Setting the package version [¶](#Setting-the-package-version-1)

When the maintainer has decided about the version number
for the next build (and likely also the next tarball),
they implement that decision by storing this version number
in a file named `.tarball-version` at the top level of the package.
This is a simple one-line text file.

The maintainer can, alternatively, decide to
let the git version be the version number.
To implement this choice,
they remove the file `.tarball-version` at the top level of the package.

The actual version number comes from an invocation of
the program `git-version-gen`,
part of the Gnulib module `git-version-gen`.
It looks at the file `.tarball-version`, if that exists,
and at the current checkout, otherwise.

The file `.tarball-version` should not be put under version control.
Therefore you may want to list it
in the package’s top-level `.gitignore` file.

But you need a `Makefile.am` rule that will
make sure that `.tarball-version` will exist in distribution tarballs:

```
dist-hook: dist-tarball-version
.PHONY: dist-tarball-version
dist-tarball-version:
	echo '$(VERSION)' > $(distdir)/.tarball-version
```

Typically, the maintainer will set the version number in a clean directory
(i.e. after “make distclean”).
In order to adapt to alternative workflows,
the file `GNUmakefile`, part of the Gnulib module `gnumakefile`,
contains a rule that will print a warning or possibly call `autoreconf`
if it finds that the version has changed but has not yet been propagated.
Reminder: Not every package supports `autoreconf`.

---

Next: [Using the package version during the build](#Using-the-package-version), Previous: [Setting the package version](#Setting-the-package-version), Up: [Package version management](#Package-version-management)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 20.8.2 Propagating the package version [¶](#Propagating-the-package-version-1)

At the end of a `configure` run,
`config.status` is run,
that creates various files with embedded pieces of information.
It thus propagates the values of various variables
to various files in the build tree
(most notably, `Makefile`s and `config.h`).

One such propagated value is
the value of the Automake-defined variable `$(VERSION)`.
(There is also the Autoconf-defined variable `$(PACKAGE_VERSION)`,
but nothing except Automake ought to use it.)

For `$(VERSION)` to have a sensible value,
a few lines are needed in `configure.ac`.

The recommended code pattern is

```
AC_INIT([package], [package])
AC_CONFIG_SRCDIR([unique-file-in-source-dir])
AC_CONFIG_AUX_DIR([build-aux])
VERSION_NUMBER=`cd $srcdir \
                && build-aux/git-version-gen .tarball-version`
gl_INIT_PACKAGE_VERSION([$VERSION_NUMBER])
AM_INIT_AUTOMAKE([options])
```

With this code pattern,
the contents of the file `.tarball-version` and the git status
are considered when `configure` is run.

Two older code patterns are deprecated, because they read
the contents of the file `.tarball-version` and the git status
when `autoconf` is run, not when `configure` is run.
These older code patterns thus require a longer turnaround cycle
when the maintainer has changed the version number.

The first such old code pattern
is to set the version number directly in `configure.ac`:

```
AC_INIT([package, version])
AC_CONFIG_SRCDIR([unique-file-in-source-dir])
AC_CONFIG_AUX_DIR([build-aux])
AM_INIT_AUTOMAKE([options])
```

The second such old code pattern
is to invoke `git-version-gen` at `autoconf` time:

```
AC_INIT([package],
        m4_esyscmd([build-aux/git-version-gen .tarball-version])])
AC_CONFIG_SRCDIR([unique-file-in-source-dir])
AC_CONFIG_AUX_DIR([build-aux])
AM_INIT_AUTOMAKE([options])
```

The macro `gl_INIT_PACKAGE_VERSION`
is defined in the Gnulib module `package-version`.

---

Previous: [Propagating the package version](#Propagating-the-package-version), Up: [Package version management](#Package-version-management)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 20.8.3 Using the package version during the build [¶](#Using-the-package-version-during-the-build)

#### The unoptimized way [¶](#The-unoptimized-way)

Once propagated through `config.status`,
the value of `$(VERSION)` can appear anywhere in built files.
For built files that are
distributed (that is, not erased by “make distclean”),
it is important to add a dependency in the `Makefile.am`,
so that the file gets rebuilt:

```
built-file: $(top_builddir)/config.status
```

For files that are erased by “make distclean”, this is not really necessary,
because

* In the main workflow, where the developer
  changes the version only between “make distclean” and “./configure”,
  the built files have been remade.
* Most `.o` files depend on `config.h`,
  which is rebuilt when `configure` runs.

Thus, only in packages that don’t use Gnulib would a dependency such as

```
hello-hello.$(OBJEXT): $(top_builddir)/config.status
```

be needed.

#### The optimized way [¶](#The-optimized-way)

Notice that
`config.status` usually changes much more often than the version number.
Therefore, for built files which depend
*only* on `$(VERSION)` and not on other variables,
the following optimized technique can be used.

The technique consists of keeping a file named `$(top_srcdir)/.version`,
which is a timestamp file.
Its modification time represents the last time
the value of `$(VERSION)` was changed.
Its contents is an undocumented implementation detail.

With such a file, the dependency in `Makefile.am` becomes:

```
built-file: $(top_srcdir)/.version
```

In order to prepare for using `$(top_srcdir)/.version`,
three modifications in the package are needed:

* In the top-level `configure.ac` file,
  add an invocation of the macro `gl_CONFIG_VERSION_STAMP`,
  near the end (before `AC_OUTPUT`).
  This macro is defined in file `m4/version-stamp.m4`,
  part of the Gnulib module `version-stamp`.
* In the top-level `Makefile.am`, add:

  ```
  EXTRA_DIST += $(top_srcdir)/.version
  BUILT_SOURCES += $(top_srcdir)/.version
  ```

  so that the file `.version` will be present in tarballs
  and so that “make maintainer-clean” will erase it.
* Add `.version` to the package’s top-level `.gitignore` file.

---

Next: [gitlog-to-changelog](#gitlog_002dto_002dchangelog), Previous: [Package version management](#Package-version-management), Up: [Build Infrastructure Modules](#Build-Infrastructure-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 20.9 VCS To ChangeLog [¶](#VCS-To-ChangeLog-1)

Gnulib provides the ‘`vcs-to-changelog`’ module to generate an output
similar to the GNU ChangeLog format from metadata of source control software
such as git. Here’s an example of using ‘`vcs-to-changelog`’:

```
build-aux/vcs-to-changelog.py <from_ref> <to_ref>
```

where `<from_ref>` and `<to_ref>` refer to the range of commits to
generate the output.

VCS To ChangeLog currently recognises changes in C source code and can traverse
commits in git. Additional source frontends and source control backends may be
added to the module. ‘`vcs-to-changelog`’ takes the following optional
arguments:

* `-d`: Run the parser debugger, used for debugging
  ‘`vcs-to-changelog`’
* `-q filename`: Load filename as the quirks file for the
  project.

The quirks file is a python module that must minimally implement a
`get_project_quirks` function that returns an object of type
`ProjectQuirks` or its subclass. The subclass may override the following
members of `ProjectQuirks`:

* `repo`: Specify the project repo source control. The default value
  is `git`.
* `IGNORE_LIST`: A list of files to ignore in the changesets, either
  because they are not needed (such as the ChangeLog) or because they are
  not parseable. For example, the GNU C Library has a header file that is only
  assembly code, which breaks the C parser.
* `MACRO_QUIRKS`: A list of dictionary entries with indexes as
  `orig` and `sub` where `orig` is a Python regular expression
  pattern to match and `sub` is the substitution. These substitutions are
  used to work around C/C++ macros that are known to break parsing of C programs.
* `C_MACROS`: This is a list of C preprocessor macro definitions that
  are extensively used and are known to break parsing due to some characteristic,
  mainly the lack of a semicolon at the end.

---

Next: [maintainer-makefile](#maintainer_002dmakefile), Previous: [VCS To ChangeLog](#VCS-To-ChangeLog), Up: [Build Infrastructure Modules](#Build-Infrastructure-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 20.10 gitlog-to-changelog [¶](#gitlog_002dto_002dchangelog-1)

Gnulib has a module `gitlog-to-changelog` to parse `git log`
output and generate `ChangeLog` files; see
<https://www.gnu.org/prep/standards/html_node/Change-Logs.html>.

You can use it by extending the `dist-hook` rule in the
top-level `Makefile.am` like this:

```
dist-hook: gen-ChangeLog
.PHONY: gen-ChangeLog
gen-ChangeLog:
        $(AM_V_GEN)if test -e $(srcdir)/.git; then             \
          LC_ALL=en_US.UTF-8 TZ=UTC0                           \
            $(top_srcdir)/build-aux/gitlog-to-changelog        \
              --srcdir=$(srcdir) > $(distdir)/ChangeLog        \
          || exit 1;                                           \
        fi
```

See `gitlog-to-changelog --help` for complete documentation.

The `LC_ALL=en_US.UTF-8 TZ=UTC0` line in this recipe assumes that
you want to generate reproducible `ChangeLog` files that do not
depend on the developer’s locale and time zone. Omit this line if you
prefer `ChangeLog` files that depend on these developer settings.

If you wish to output the `ChangeLog` with dates respecting the
time zone each individual commit was made in you can use the
`--commit-timezone` option. For example:

```
dist-hook: gen-ChangeLog
.PHONY: gen-ChangeLog
gen-ChangeLog:
        $(AM_V_GEN)if test -e $(srcdir)/.git; then             \
          $(top_srcdir)/build-aux/gitlog-to-changelog          \
              --srcdir=$(srcdir) --commit-timezone             \
              > $(distdir)/ChangeLog                           \
          || exit 1;                                           \
        fi
```

The use of `--commit-timezone` means that `ChangeLog` dates
correctly represent when a committer pushed a change according to their
time zone. However, as a consequence `ChangeLog` dates will no
longer be monotonically increasing, if the developers are spread across
different time zones.
For example, the following three commits were made in a short period of
time across two different time zones.
This behavior may be undesired.

```
2024-06-19  Bruno Haible  <bruno@clisp.org>

	...

2024-06-18  Collin Funk  <collin.funk1@gmail.com>

	...

2024-06-19  Bruno Haible  <bruno@clisp.org>

	...
```

If you wish to limit the `ChangeLog` entries (perhaps for size
issues) to contain only entries since a particular git tag, you can
use a `gen-ChangeLog` rule like the following:

```
gen_start_ver = 8.31
gen-ChangeLog:
        $(AM_V_GEN)if test -e $(srcdir)/.git; then                 \
          log_fix='$(srcdir)/build-aux/git-log-fix';               \
          test -e "$$log_fix"                                      \
            && amend_git_log=--amend=$$log_fix                     \
            || amend_git_log=;                                     \
          { LC_ALL=en_US.UTF-8 TZ=UTC0                             \
              $(top_srcdir)/build-aux/gitlog-to-changelog          \
                --srcdir=$(srcdir)                                 \
                "$$amend_git_log" -- 'v$(gen_start_ver)~..' &&     \
            printf '\n\nSee the source repo for older entries.\n'; \
          } > $(distdir)/ChangeLog                                 \
          || exit 1;                                               \
        fi
```

---

Previous: [gitlog-to-changelog](#gitlog_002dto_002dchangelog), Up: [Build Infrastructure Modules](#Build-Infrastructure-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 20.11 maintainer-makefile [¶](#maintainer_002dmakefile-1)

Software projects are generally expected to adhere to specific coding
and maintenance conventions. GNU projects in particular follow the
conventions laid out in the GNU Coding Standards and Information for GNU
Maintainers manuals. The Gnulib module `maintainer-makefile` helps
automate many of these processes by providing a common framework with
overridable defaults.

The module includes the files `GNUmakefile` (from the
`gnumakefile` module) and `maint.mk`, which define several
convenient targets. Each target usually comes with a description of its
workings and the variables that control it. You can override variables
and define rules in a file named `cfg.mk`, which `GNUmakefile`
will look for. Some targets may not be relevant to your project, or may
invoke programs that you do not have installed; you are not required to
set all of them up.

Below is a summary of the available targets.

`coverage`
:   Generate code coverage reports.

`refresh-gnulib-patches`

`refresh-po`
:   Check for updates to external files.

`release-commit`

`release`

`upload`

`web-manual-update`
:   Automate the process of making, announcing, and distributing a new
    project release.

    These rely on several other modules, including
    `do-release-commit-and-tag` and `announce-gen`, and expect a
    GNU-like project structure by default. For example, your project should
    maintain a file `NEWS` or `NEWS.md` which highlights important
    changes between releases, from new to old. The section for each release
    is introduced by a heading typically indicating its version, date, and
    type. Here are some examples:

    ```
    * Noteworthy changes in release 1.2 (2025-11-17) [stable]
    # Noteworthy changes in release 1.2 (2025-11-17)
    ## Major changes in 1.2.3 (2025-11-17) [beta]
    Changes in 1.2.3 (2025-11-17)
    ```

    The first such heading in the file stands as a placeholder for
    as-yet-unreleased changes. It typically looks like a template for
    subsequent headings, with details replaced by question marks. For
    example:

    ```
    # Noteworthy changes in release ?.? (????-??-??) [?]
    ```

    Though this file was traditionally named `NEWS`, more recent
    versions of Automake and Gnulib also accept `NEWS.md`. If the
    tooling infrastructure you use supports only `NEWS` but you would
    prefer to maintain a `NEWS.md`, you can try defining the former as
    a symlink to the latter. In any case, maintaining both names as
    distinct files can be confusing and is therefore discouraged.

    See also the module `readme-release`, which outlines a typical GNU
    release process.

`syntax-check`
:   Search the project for many suspicious constructs, including unportable
    syntax, stale copyright notices, and misspellings.

`update-copyright` [¶](#index-update_002dcopyright)
:   Update all copyright year lists.

---

Next: [Release Management Files](#Release-Management-Files), Previous: [Build Infrastructure Modules](#Build-Infrastructure-Modules), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 21 Build Infrastructure Files [¶](#Build-Infrastructure-Files-1)

Gnulib contains also a small number of files that are not part of
modules. They are meant to be imported into packages by means of
‘`gnulib-tool --copy-file`’, not ‘`gnulib-tool --import`’. For
example, the commands to import the files `config.guess` and
`config.sub` are

```
for file in config.guess config.sub; do
  $GNULIB_TOOL --copy-file build-aux/$file \
    && chmod a+x build-aux/$file \
    || exit $?
done
```

Packages that don’t use Gnulib can get hold of these files through
direct download from Gnulib’s git repository. The commands to do this
look as follows:

```
for file in config.guess config.sub; do
  echo "$0: getting $file..."
  wget -q --timeout=5 -O build-aux/$file.tmp "https://git.savannah.gnu.org/gitweb/?p=gnulib.git;a=blob_plain;f=build-aux/${file};hb=HEAD" \
    && mv build-aux/$file.tmp build-aux/$file \
    && chmod a+x build-aux/$file
  retval=$?
  rm -f build-aux/$file.tmp
  test $retval -eq 0 || exit $retval
done
```

* [Recognizing platforms](#Recognizing-platforms)
* [Utilities for Makefiles](#Utilities-for-Makefiles)
* [Programs for developing in Git checkouts](#Developer-tools)
* [Utilities for building documentation](#For-building-documentation)
* [Utilities for building libraries](#For-building-libraries)
* [Utilities for running test suites](#For-running-tests)

---

Next: [Utilities for Makefiles](#Utilities-for-Makefiles), Up: [Build Infrastructure Files](#Build-Infrastructure-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 21.1 Recognizing platforms [¶](#Recognizing-platforms-1)

`build-aux/config.guess`

`build-aux/config.sub`
:   These files are helper scripts, invoked by the ‘`configure`’ script.
    `config.guess` recognizes the platform on which the script is
    running, and produces a triplet of the form
    `cpu-type-vendor-operating_system`.
    `config.sub` receives a possibly abbreviated triplet and produces a
    canonical triplet for a platform. For more information, see
    <https://www.gnu.org/prep/standards/html_node/Configuration.html>.

It is important that you always include the newest versions of these two
files in your tarball, because people who work on emerging platforms
otherwise have a hard time building your package.

---

Next: [Programs for developing in Git checkouts](#Developer-tools), Previous: [Recognizing platforms](#Recognizing-platforms), Up: [Build Infrastructure Files](#Build-Infrastructure-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 21.2 Utilities for Makefiles [¶](#Utilities-for-Makefiles-1)

These programs can be used in Makefiles. Some
of them are also described in
<https://www.gnu.org/software/automake/manual/html_node/Auxiliary-Programs.html>.

`build-aux/ar-lib`

`build-aux/compile`
:   These two scripts are necessary for supporting portability to native
    Windows with the MSVC compiler. `compile` is a wrapper script that
    invokes the compiler and provides a command-line interface compatible
    with Unix compilers. Similarly, `ar-lib` is a wrapper script that
    provides a command-line interface compatible with Unix `ar`.

`build-aux/depcomp`
:   This is a helper script, used by Makefile rules generated by GNU
    Automake. It generates Makefile dependencies while compiling a file.

`build-aux/install-sh`
:   This is a helper script, used by Makefile rules generated by GNU
    Automake. It installs files during the `make install` phase. In
    the Makefile, don’t use this file directly; always use
    `$(INSTALL_PROGRAM)` or `$(INSTALL_DATA)` instead.

`build-aux/mdate-sh`
:   This script determines the modification time of a file and pretty-prints
    it. The typical use is to add a “Last modified” line to the
    documentation.

`build-aux/mkinstalldirs`
:   This is a helper script, used by Makefile rules generated by GNU
    Automake. It creates directories during the `make install` phase.
    It is roughly equivalent to ‘`mkdir -p`’ (except that the latter is
    not portable). In the Makefile, don’t use this file directly; always
    use `$(MKDIR_P)` instead.

`build-aux/mktempd`
:   This script creates a temporary directory. It is roughly equivalent to
    ‘`mktemp -d`’ (except that the latter is not portable).

`build-aux/move-if-change`
:   This script moves a freshly generated file to a destination file, with a
    special optimization for the case that both files are identical. In
    this case the freshly generated file is deleted, and the time stamp of
    the destination file is *not* changed. This is useful when
    updating a file that rarely actually changes and which many Makefile
    targets depend upon.

---

Next: [Utilities for building documentation](#For-building-documentation), Previous: [Utilities for Makefiles](#Utilities-for-Makefiles), Up: [Build Infrastructure Files](#Build-Infrastructure-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 21.3 Programs for developing in Git checkouts [¶](#Programs-for-developing-in-Git-checkouts)

These programs can help when developing in a Git
checkout. The maintainer of the package copies these programs into the
version control of the package, so that co-developers can use these
tools right away.

`top/gitsub.sh`
:   This program manages the subdirectories of a Git checkout that come from
    other packages, including Gnulib.

`top/bootstrap`

`top/autopull.sh`

`top/autogen.sh`

`top/bootstrap-funclib.sh`
:   This is a set of three programs and a function library, that manage
    the source directory of a package, preparing for the state where
    ‘`./configure`’ can be used.

    `autopull.sh` is a program for fetching dependencies that may
    require network accesses. It manages the Git submodules, including
    Gnulib – assuming that Gnulib is a Git submodule. It also can fetch
    the PO files for internationalized packages.

    `autogen.sh` is a program that is meant to be run after
    `autopull.sh`. It generates all autogeneratable files that are
    omitted from version control. Usually this means that it invokes
    `gnulib-tool` and `automake`, that generate files from other
    files.

    `bootstrap` is a wrapper around both:
    `./bootstrap --pull` is equivalent to `./autopull.sh`,
    `./bootstrap --gen` is equivalent to `./autogen.sh`.
    Plain `./bootstrap` is equivalent to `./autopull.sh`
    immediately followed by `./autogen.sh`; however, because plain
    `./bootstrap` mixes version control management and
    generation of files in non-obvious ways, it has a number of usability
    issues for the advanced developer.

    `bootstrap-funclib.sh` is a function library for these three
    programs. It is not meant to be used directly.

    All three programs make use of a configuration file, called
    `bootstrap.conf`.

`build-aux/bootstrap`
:   This acts like `top/bootstrap`, except it does not
    need the companion files `autogen.sh`,
    `autopull.sh`, and `bootstrap-funclib.sh`
    so it avoids some clutter in your project’s top level directory.
    With this approach, you update via `./bootstrap --pull` and
    `./bootstrap --gen` instead of via `./autopull.sh` and
    `./autogen.sh`. Otherwise this approach acts similarly, and
    uses the same `bootstrap.conf` file.

`build-aux/bootstrap.conf`
:   This is the template configuration file. After copying it into the
    top-level directory of your package, you need to customize it.

`build-aux/po/Makefile.in.in`

`build-aux/po/remove-potcdate.sed`
:   These are auxiliary files used by `bootstrap`. You don’t have to
    copy them yourself; `bootstrap` will do that.

---

Next: [Utilities for building libraries](#For-building-libraries), Previous: [Programs for developing in Git checkouts](#Developer-tools), Up: [Build Infrastructure Files](#Build-Infrastructure-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 21.4 Utilities for building documentation [¶](#Utilities-for-building-documentation)

These are auxiliary files for building documentation.

`build-aux/texinfo.tex`
:   This file is needed for the conversion of Texinfo-format documentation
    to PDF, PostScript, or DVI formats. It implements the GNU Texinfo
    commands on top of plain TeX.

`build-aux/x-to-1.in`
:   This file, once processed, gives a program `x-to-1`, that produces
    a manual page for a program, by combining a skeleton with the program’s
    `--help` output.

---

Next: [Utilities for running test suites](#For-running-tests), Previous: [Utilities for building documentation](#For-building-documentation), Up: [Build Infrastructure Files](#Build-Infrastructure-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 21.5 Utilities for building libraries [¶](#Utilities-for-building-libraries)

`build-aux/declared.sh`
:   This program extracts the declared global symbols of a C header file.
    It is useful when you want to control the set of symbols exported by a
    library. See [Controlling the Exported Symbols of Shared Libraries](#Exported-Symbols-of-Shared-Libraries).

---

Previous: [Utilities for building libraries](#For-building-libraries), Up: [Build Infrastructure Files](#Build-Infrastructure-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 21.6 Utilities for running test suites [¶](#Utilities-for-running-test-suites)

`build-aux/run-test`
:   This file is a test driver that supports running a test under
    `valgrind`.

`build-aux/test-driver.diff`
:   This is a patch, against Automake’s test driver, that supports running a
    test suite on Android.

---

Next: [GNU Free Documentation License](#GNU-Free-Documentation-License), Previous: [Build Infrastructure Files](#Build-Infrastructure-Files), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 22 Release Management Files [¶](#Release-Management-Files-1)

Gnulib also contains a few scripts that are useful for the release
management of a package. They can be used directly off the Gnulib
checkout; they don’t need to be copied first.

* [Tools for releasing packages with shared libraries](#For-building-shared-libraries)
* [Tools for uploading release tarballs](#For-uploading-releases)

---

Next: [Tools for uploading release tarballs](#For-uploading-releases), Up: [Release Management Files](#Release-Management-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 22.1 Tools for releasing packages with shared libraries [¶](#Tools-for-releasing-packages-with-shared-libraries)

`build-aux/libtool-next-version`
:   This program is a wizard that helps a maintainer update the libtool
    version of a shared library, without making mistakes in this process.
    For background documentation, see
    <https://www.gnu.org/software/libtool/manual/html_node/Updating-version-info.html>.

---

Previous: [Tools for releasing packages with shared libraries](#For-building-shared-libraries), Up: [Release Management Files](#Release-Management-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 22.2 Tools for uploading release tarballs [¶](#Tools-for-uploading-release-tarballs)

`build-aux/gnupload` [¶](#index-gnupload)
:   This program is a user-friendly way to upload a release tarball to one of
    the GNU servers (`ftp.gnu.org` or `alpha.gnu.org`). It
    implements the interface described in
    <https://www.gnu.org/prep/maintain/html_node/Automated-FTP-Uploads.html>.

`build-aux/ncftpput-ftp`
:   This is a helper program that mimics the `ncftpput` program used by
    `gnupload`. If you want to use `gnupload` but don’t have
    `ncftp` installed, copy this file into your $PATH, renaming it to
    `ncftpput`.

---

Next: [Gnulib history](#Gnulib-history), Previous: [Release Management Files](#Release-Management-Files), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## Appendix A GNU Free Documentation License [¶](#GNU-Free-Documentation-License-1)

Version 1.3, 3 November 2008

```
Copyright © 2000, 2001, 2002, 2007, 2008 Free Software Foundation, Inc.
https://fsf.org/

Everyone is permitted to copy and distribute verbatim copies
of this license document, but changing it is not allowed.
```

0. PREAMBLE

   The purpose of this License is to make a manual, textbook, or other
   functional and useful document *free* in the sense of freedom: to
   assure everyone the effective freedom to copy and redistribute it,
   with or without modifying it, either commercially or noncommercially.
   Secondarily, this License preserves for the author and publisher a way
   to get credit for their work, while not being considered responsible
   for modifications made by others.

   This License is a kind of “copyleft”, which means that derivative
   works of the document must themselves be free in the same sense. It
   complements the GNU General Public License, which is a copyleft
   license designed for free software.

   We have designed this License in order to use it for manuals for free
   software, because free software needs free documentation: a free
   program should come with manuals providing the same freedoms that the
   software does. But this License is not limited to software manuals;
   it can be used for any textual work, regardless of subject matter or
   whether it is published as a printed book. We recommend this License
   principally for works whose purpose is instruction or reference.
1. APPLICABILITY AND DEFINITIONS

   This License applies to any manual or other work, in any medium, that
   contains a notice placed by the copyright holder saying it can be
   distributed under the terms of this License. Such a notice grants a
   world-wide, royalty-free license, unlimited in duration, to use that
   work under the conditions stated herein. The “Document”, below,
   refers to any such manual or work. Any member of the public is a
   licensee, and is addressed as “you”. You accept the license if you
   copy, modify or distribute the work in a way requiring permission
   under copyright law.

   A “Modified Version” of the Document means any work containing the
   Document or a portion of it, either copied verbatim, or with
   modifications and/or translated into another language.

   A “Secondary Section” is a named appendix or a front-matter section
   of the Document that deals exclusively with the relationship of the
   publishers or authors of the Document to the Document’s overall
   subject (or to related matters) and contains nothing that could fall
   directly within that overall subject. (Thus, if the Document is in
   part a textbook of mathematics, a Secondary Section may not explain
   any mathematics.) The relationship could be a matter of historical
   connection with the subject or with related matters, or of legal,
   commercial, philosophical, ethical or political position regarding
   them.

   The “Invariant Sections” are certain Secondary Sections whose titles
   are designated, as being those of Invariant Sections, in the notice
   that says that the Document is released under this License. If a
   section does not fit the above definition of Secondary then it is not
   allowed to be designated as Invariant. The Document may contain zero
   Invariant Sections. If the Document does not identify any Invariant
   Sections then there are none.

   The “Cover Texts” are certain short passages of text that are listed,
   as Front-Cover Texts or Back-Cover Texts, in the notice that says that
   the Document is released under this License. A Front-Cover Text may
   be at most 5 words, and a Back-Cover Text may be at most 25 words.

   A “Transparent” copy of the Document means a machine-readable copy,
   represented in a format whose specification is available to the
   general public, that is suitable for revising the document
   straightforwardly with generic text editors or (for images composed of
   pixels) generic paint programs or (for drawings) some widely available
   drawing editor, and that is suitable for input to text formatters or
   for automatic translation to a variety of formats suitable for input
   to text formatters. A copy made in an otherwise Transparent file
   format whose markup, or absence of markup, has been arranged to thwart
   or discourage subsequent modification by readers is not Transparent.
   An image format is not Transparent if used for any substantial amount
   of text. A copy that is not “Transparent” is called “Opaque”.

   Examples of suitable formats for Transparent copies include plain
   ASCII without markup, Texinfo input format, LaTeX input
   format, SGML or XML using a publicly available
   DTD, and standard-conforming simple HTML,
   PostScript or PDF designed for human modification. Examples
   of transparent image formats include PNG, XCF and
   JPG. Opaque formats include proprietary formats that can be
   read and edited only by proprietary word processors, SGML or
   XML for which the DTD and/or processing tools are
   not generally available, and the machine-generated HTML,
   PostScript or PDF produced by some word processors for
   output purposes only.

   The “Title Page” means, for a printed book, the title page itself,
   plus such following pages as are needed to hold, legibly, the material
   this License requires to appear in the title page. For works in
   formats which do not have any title page as such, “Title Page” means
   the text near the most prominent appearance of the work’s title,
   preceding the beginning of the body of the text.

   The “publisher” means any person or entity that distributes copies
   of the Document to the public.

   A section “Entitled XYZ” means a named subunit of the Document whose
   title either is precisely XYZ or contains XYZ in parentheses following
   text that translates XYZ in another language. (Here XYZ stands for a
   specific section name mentioned below, such as “Acknowledgements”,
   “Dedications”, “Endorsements”, or “History”.) To “Preserve the Title”
   of such a section when you modify the Document means that it remains a
   section “Entitled XYZ” according to this definition.

   The Document may include Warranty Disclaimers next to the notice which
   states that this License applies to the Document. These Warranty
   Disclaimers are considered to be included by reference in this
   License, but only as regards disclaiming warranties: any other
   implication that these Warranty Disclaimers may have is void and has
   no effect on the meaning of this License.
2. VERBATIM COPYING

   You may copy and distribute the Document in any medium, either
   commercially or noncommercially, provided that this License, the
   copyright notices, and the license notice saying this License applies
   to the Document are reproduced in all copies, and that you add no other
   conditions whatsoever to those of this License. You may not use
   technical measures to obstruct or control the reading or further
   copying of the copies you make or distribute. However, you may accept
   compensation in exchange for copies. If you distribute a large enough
   number of copies you must also follow the conditions in section 3.

   You may also lend copies, under the same conditions stated above, and
   you may publicly display copies.
3. COPYING IN QUANTITY

   If you publish printed copies (or copies in media that commonly have
   printed covers) of the Document, numbering more than 100, and the
   Document’s license notice requires Cover Texts, you must enclose the
   copies in covers that carry, clearly and legibly, all these Cover
   Texts: Front-Cover Texts on the front cover, and Back-Cover Texts on
   the back cover. Both covers must also clearly and legibly identify
   you as the publisher of these copies. The front cover must present
   the full title with all words of the title equally prominent and
   visible. You may add other material on the covers in addition.
   Copying with changes limited to the covers, as long as they preserve
   the title of the Document and satisfy these conditions, can be treated
   as verbatim copying in other respects.

   If the required texts for either cover are too voluminous to fit
   legibly, you should put the first ones listed (as many as fit
   reasonably) on the actual cover, and continue the rest onto adjacent
   pages.

   If you publish or distribute Opaque copies of the Document numbering
   more than 100, you must either include a machine-readable Transparent
   copy along with each Opaque copy, or state in or with each Opaque copy
   a computer-network location from which the general network-using
   public has access to download using public-standard network protocols
   a complete Transparent copy of the Document, free of added material.
   If you use the latter option, you must take reasonably prudent steps,
   when you begin distribution of Opaque copies in quantity, to ensure
   that this Transparent copy will remain thus accessible at the stated
   location until at least one year after the last time you distribute an
   Opaque copy (directly or through your agents or retailers) of that
   edition to the public.

   It is requested, but not required, that you contact the authors of the
   Document well before redistributing any large number of copies, to give
   them a chance to provide you with an updated version of the Document.
4. MODIFICATIONS

   You may copy and distribute a Modified Version of the Document under
   the conditions of sections 2 and 3 above, provided that you release
   the Modified Version under precisely this License, with the Modified
   Version filling the role of the Document, thus licensing distribution
   and modification of the Modified Version to whoever possesses a copy
   of it. In addition, you must do these things in the Modified Version:

   1. Use in the Title Page (and on the covers, if any) a title distinct
      from that of the Document, and from those of previous versions
      (which should, if there were any, be listed in the History section
      of the Document). You may use the same title as a previous version
      if the original publisher of that version gives permission.
   2. List on the Title Page, as authors, one or more persons or entities
      responsible for authorship of the modifications in the Modified
      Version, together with at least five of the principal authors of the
      Document (all of its principal authors, if it has fewer than five),
      unless they release you from this requirement.
   3. State on the Title page the name of the publisher of the
      Modified Version, as the publisher.
   4. Preserve all the copyright notices of the Document.
   5. Add an appropriate copyright notice for your modifications
      adjacent to the other copyright notices.
   6. Include, immediately after the copyright notices, a license notice
      giving the public permission to use the Modified Version under the
      terms of this License, in the form shown in the Addendum below.
   7. Preserve in that license notice the full lists of Invariant Sections
      and required Cover Texts given in the Document’s license notice.
   8. Include an unaltered copy of this License.
   9. Preserve the section Entitled “History”, Preserve its Title, and add
      to it an item stating at least the title, year, new authors, and
      publisher of the Modified Version as given on the Title Page. If
      there is no section Entitled “History” in the Document, create one
      stating the title, year, authors, and publisher of the Document as
      given on its Title Page, then add an item describing the Modified
      Version as stated in the previous sentence.
   10. Preserve the network location, if any, given in the Document for
       public access to a Transparent copy of the Document, and likewise
       the network locations given in the Document for previous versions
       it was based on. These may be placed in the “History” section.
       You may omit a network location for a work that was published at
       least four years before the Document itself, or if the original
       publisher of the version it refers to gives permission.
   11. For any section Entitled “Acknowledgements” or “Dedications”, Preserve
       the Title of the section, and preserve in the section all the
       substance and tone of each of the contributor acknowledgements and/or
       dedications given therein.
   12. Preserve all the Invariant Sections of the Document,
       unaltered in their text and in their titles. Section numbers
       or the equivalent are not considered part of the section titles.
   13. Delete any section Entitled “Endorsements”. Such a section
       may not be included in the Modified Version.
   14. Do not retitle any existing section to be Entitled “Endorsements” or
       to conflict in title with any Invariant Section.
   15. Preserve any Warranty Disclaimers.

   If the Modified Version includes new front-matter sections or
   appendices that qualify as Secondary Sections and contain no material
   copied from the Document, you may at your option designate some or all
   of these sections as invariant. To do this, add their titles to the
   list of Invariant Sections in the Modified Version’s license notice.
   These titles must be distinct from any other section titles.

   You may add a section Entitled “Endorsements”, provided it contains
   nothing but endorsements of your Modified Version by various
   parties—for example, statements of peer review or that the text has
   been approved by an organization as the authoritative definition of a
   standard.

   You may add a passage of up to five words as a Front-Cover Text, and a
   passage of up to 25 words as a Back-Cover Text, to the end of the list
   of Cover Texts in the Modified Version. Only one passage of
   Front-Cover Text and one of Back-Cover Text may be added by (or
   through arrangements made by) any one entity. If the Document already
   includes a cover text for the same cover, previously added by you or
   by arrangement made by the same entity you are acting on behalf of,
   you may not add another; but you may replace the old one, on explicit
   permission from the previous publisher that added the old one.

   The author(s) and publisher(s) of the Document do not by this License
   give permission to use their names for publicity for or to assert or
   imply endorsement of any Modified Version.
5. COMBINING DOCUMENTS

   You may combine the Document with other documents released under this
   License, under the terms defined in section 4 above for modified
   versions, provided that you include in the combination all of the
   Invariant Sections of all of the original documents, unmodified, and
   list them all as Invariant Sections of your combined work in its
   license notice, and that you preserve all their Warranty Disclaimers.

   The combined work need only contain one copy of this License, and
   multiple identical Invariant Sections may be replaced with a single
   copy. If there are multiple Invariant Sections with the same name but
   different contents, make the title of each such section unique by
   adding at the end of it, in parentheses, the name of the original
   author or publisher of that section if known, or else a unique number.
   Make the same adjustment to the section titles in the list of
   Invariant Sections in the license notice of the combined work.

   In the combination, you must combine any sections Entitled “History”
   in the various original documents, forming one section Entitled
   “History”; likewise combine any sections Entitled “Acknowledgements”,
   and any sections Entitled “Dedications”. You must delete all
   sections Entitled “Endorsements.”
6. COLLECTIONS OF DOCUMENTS

   You may make a collection consisting of the Document and other documents
   released under this License, and replace the individual copies of this
   License in the various documents with a single copy that is included in
   the collection, provided that you follow the rules of this License for
   verbatim copying of each of the documents in all other respects.

   You may extract a single document from such a collection, and distribute
   it individually under this License, provided you insert a copy of this
   License into the extracted document, and follow this License in all
   other respects regarding verbatim copying of that document.
7. AGGREGATION WITH INDEPENDENT WORKS

   A compilation of the Document or its derivatives with other separate
   and independent documents or works, in or on a volume of a storage or
   distribution medium, is called an “aggregate” if the copyright
   resulting from the compilation is not used to limit the legal rights
   of the compilation’s users beyond what the individual works permit.
   When the Document is included in an aggregate, this License does not
   apply to the other works in the aggregate which are not themselves
   derivative works of the Document.

   If the Cover Text requirement of section 3 is applicable to these
   copies of the Document, then if the Document is less than one half of
   the entire aggregate, the Document’s Cover Texts may be placed on
   covers that bracket the Document within the aggregate, or the
   electronic equivalent of covers if the Document is in electronic form.
   Otherwise they must appear on printed covers that bracket the whole
   aggregate.
8. TRANSLATION

   Translation is considered a kind of modification, so you may
   distribute translations of the Document under the terms of section 4.
   Replacing Invariant Sections with translations requires special
   permission from their copyright holders, but you may include
   translations of some or all Invariant Sections in addition to the
   original versions of these Invariant Sections. You may include a
   translation of this License, and all the license notices in the
   Document, and any Warranty Disclaimers, provided that you also include
   the original English version of this License and the original versions
   of those notices and disclaimers. In case of a disagreement between
   the translation and the original version of this License or a notice
   or disclaimer, the original version will prevail.

   If a section in the Document is Entitled “Acknowledgements”,
   “Dedications”, or “History”, the requirement (section 4) to Preserve
   its Title (section 1) will typically require changing the actual
   title.
9. TERMINATION

   You may not copy, modify, sublicense, or distribute the Document
   except as expressly provided under this License. Any attempt
   otherwise to copy, modify, sublicense, or distribute it is void, and
   will automatically terminate your rights under this License.

   However, if you cease all violation of this License, then your license
   from a particular copyright holder is reinstated (a) provisionally,
   unless and until the copyright holder explicitly and finally
   terminates your license, and (b) permanently, if the copyright holder
   fails to notify you of the violation by some reasonable means prior to
   60 days after the cessation.

   Moreover, your license from a particular copyright holder is
   reinstated permanently if the copyright holder notifies you of the
   violation by some reasonable means, this is the first time you have
   received notice of violation of this License (for any work) from that
   copyright holder, and you cure the violation prior to 30 days after
   your receipt of the notice.

   Termination of your rights under this section does not terminate the
   licenses of parties who have received copies or rights from you under
   this License. If your rights have been terminated and not permanently
   reinstated, receipt of a copy of some or all of the same material does
   not give you any rights to use it.
10. FUTURE REVISIONS OF THIS LICENSE

    The Free Software Foundation may publish new, revised versions
    of the GNU Free Documentation License from time to time. Such new
    versions will be similar in spirit to the present version, but may
    differ in detail to address new problems or concerns. See
    <https://www.gnu.org/licenses/>.

    Each version of the License is given a distinguishing version number.
    If the Document specifies that a particular numbered version of this
    License “or any later version” applies to it, you have the option of
    following the terms and conditions either of that specified version or
    of any later version that has been published (not as a draft) by the
    Free Software Foundation. If the Document does not specify a version
    number of this License, you may choose any version ever published (not
    as a draft) by the Free Software Foundation. If the Document
    specifies that a proxy can decide which future versions of this
    License can be used, that proxy’s public statement of acceptance of a
    version permanently authorizes you to choose that version for the
    Document.
11. RELICENSING

    “Massive Multiauthor Collaboration Site” (or “MMC Site”) means any
    World Wide Web server that publishes copyrightable works and also
    provides prominent facilities for anybody to edit those works. A
    public wiki that anybody can edit is an example of such a server. A
    “Massive Multiauthor Collaboration” (or “MMC”) contained in the
    site means any set of copyrightable works thus published on the MMC
    site.

    “CC-BY-SA” means the Creative Commons Attribution-Share Alike 3.0
    license published by Creative Commons Corporation, a not-for-profit
    corporation with a principal place of business in San Francisco,
    California, as well as future copyleft versions of that license
    published by that same organization.

    “Incorporate” means to publish or republish a Document, in whole or
    in part, as part of another Document.

    An MMC is “eligible for relicensing” if it is licensed under this
    License, and if all works that were first published under this License
    somewhere other than this MMC, and subsequently incorporated in whole
    or in part into the MMC, (1) had no cover texts or invariant sections,
    and (2) were thus incorporated prior to November 1, 2008.

    The operator of an MMC Site may republish an MMC contained in the site
    under CC-BY-SA on the same site at any time before August 1, 2009,
    provided the MMC is eligible for relicensing.
