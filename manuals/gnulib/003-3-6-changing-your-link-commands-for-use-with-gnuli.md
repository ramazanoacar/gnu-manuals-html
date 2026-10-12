### 3.6 Changing your link commands for use with Gnulib [¶](#Changing-your-link-commands-for-use-with-Gnulib)

When you use Gnulib, you need to augment the set of libraries against which
your programs and libraries are linked. This is done by augmenting the
Automake variable `LDADD` (for all programs) or
`prog_LDADD` (for a single program `prog`) or
`library_la_LIBADD` (for a single library `library.la`).

What do you need to add to this Automake variable?

1. The reference to the Gnulib library. In the example of section
   [Initial import](#Initial-import), this would be `lib/libgnu.a` for source in the
   top-level directory, or `../lib/libgnu.a` for source in a sibling
   directory of `lib/`.
2. References to additional libraries, brought in by some of the Gnulib
   modules that you use (directly or indirectly). The complete list of such
   libraries is printed when you invoke `gnulib-tool`. Alternatively,
   you can retrieve the set of additional libraries required by a specific
   Gnulib module by running

   ```
   ./gnulib-tool --extract-recursive-link-directive module
   ```

   Beware: By looking into the module description file `modules/module`
   or by running

   ```
   ./gnulib-tool --extract-link-directive module
   ```

   you would miss the link dependencies of indirectly used modules.

---

Next: [Modifying the build rules of a Gnulib import directory](#Modified-build-rules), Previous: [Changing your link commands for use with Gnulib](#Link_002dtime-requirements), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 3.7 Finding recommended ISO C and POSIX function substitutes [¶](#Finding-recommended-ISO-C-and-POSIX-function-substitutes)

Gnulib contains a wealth of portability workarounds for ISO C and POSIX
functions. They are listed in detail in the chapter [ISO C and POSIX Function Substitutes](#Function-Substitutes).
If you want to know which function substitutes are recommended for your
package, you can search your source code for ISO C and POSIX functions
that it uses and read the corresponding sections of said documentation
chapter. But this is a tedious task. Here is an alternative approach
that makes this task easier.

1. Add the Gnulib module ‘`posixcheck`’ to the Gnulib imports of your package,
   as described earlier in this chapter.
2. Do a `make distclean` if you previously built in the top-level directory.
   Then regenerate the Autotools-generated parts of the package.
3. On a glibc system, build your package. Pay attention to the compiler warnings.
   Warnings are generated for uses of ISO C and POSIX functions that have
   portability problems or other important pitfalls and for which you have not yet
   imported the corresponding Gnulib module. If you get, say, a warning
   “warning: call to ’close’ declared with attribute warning: close does not
   portably work on sockets - use gnulib module close for portability”,
   put ‘`close`’ on your list of modules to import.
4. Add the modules you noted to the Gnulib imports of your package.
5. Optionally, you can do the same steps again, and make sure that there are no
   warnings left except those that you want to intentionally ignore.
6. Finally, remove the Gnulib module ‘`posixcheck`’ from the Gnulib imports,
   and run `make distclean`.

---

Next: [Building directly from the top-level directory](#Non_002drecursive-make), Previous: [Finding recommended ISO C and POSIX function substitutes](#Finding-POSIX-substitutes), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 3.8 Modifying the build rules of a Gnulib import directory [¶](#Modifying-the-build-rules-of-a-Gnulib-import-directory)

In some cases, you may want to set additional compiler options for
use within the Gnulib import directory. For example, the
‘`relocatable`’ module operates better if you define the C macros
`ENABLE_COSTLY_RELOCATABLE` and `INSTALLDIR` during its
compilation.

There are two ways to do so: Use of the `gnulib-tool` option
`--makefile-name`, and a kitchen-sink module.

With the `gnulib-tool` option `--makefile-name`, you are
telling `gnulib-tool` to generate an includable `Makefile.am`
portion, rather than a self-contained `Makefile.am`. For example,
when you use `--makefile-name=Makefile.gnulib`, `gnulib-tool`
will generate `Makefile.gnulib`, and you will provide a
hand-written `Makefile.am` that includes `Makefile.gnulib`
through a line such as

```
include Makefile.gnulib
```

Before this include, you need to initialize a set of `Makefile.am`
variables.

For the directory into which `gnulib-tool` copies the gnulib source files,
the set of variables to initialize is:

* `AUTOMAKE_OPTIONS`
* `SUBDIRS`
* `noinst_HEADERS`
* `noinst_LIBRARIES`
* `noinst_LTLIBRARIES`
* `pkgdata_DATA` (only with Automake ≥ 1.11.4)
* `EXTRA_DIST`
* `BUILT_SOURCES`
* `SUFFIXES`
* `MOSTLYCLEANFILES`
* `MOSTLYCLEANDIRS`
* `CLEANFILES`
* `DISTCLEANFILES`
* `MAINTAINERCLEANFILES`
* `AM_CPPFLAGS`
* `AM_CFLAGS`

`AUTOMAKE_OPTIONS` should be initialized as described in
[Changing Automake’s Behavior](https://www.gnu.org/software/automake/manual/automake.html#Options) in GNU Automake.
The other variables can be initialized to empty. However, you will most
likely want to initialize some of them with non-empty values, in order
to achieve the desired customization.

For the directory into which `gnulib-tool` copies
the gnulib unit test files
(when option ‘`--with-tests`’ is used),
the set of variables to initialize is:

* `AM_CFLAGS`
* `AM_CXXFLAGS` (when this directory contains some C++ code)

The other approach, the kitchen-sink module, is more advanced. See
chapter [Extending Gnulib](#Extending-Gnulib).

---

Next: [Using Gnulib for both a library and a program](#Multiple-instances), Previous: [Modifying the build rules of a Gnulib import directory](#Modified-build-rules), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 3.9 Building directly from the top-level directory [¶](#Building-directly-from-the-top_002dlevel-directory)

By default, the Gnulib import directory will contain a generated
`Makefile.am` file. After configuring, this produces a generated
`Makefile` in this directory. As a consequence, the build from the
top-level directory will use a recursive `make` invocation for this
directory.

Some people prefer a build system where the `Makefile` in the
top-level directory directly builds the artifacts in the subdirectories,
without an intermediate `make` invocation. This is called
“non-recursive make” and is supported by Automake. For more details,
see <https://autotools.io/automake/nonrecursive.html>.

Gnulib supports this flavour of build system too. To use it, pass two
options to `gnulib-tool`: ‘`--makefile-name`’ and
‘`--automake-subdir`’.

With the `gnulib-tool` option ‘`--makefile-name`’, you are
telling `gnulib-tool` to generate an includable `Makefile.am`
portion in the Gnulib import directory, rather than a self-contained
`Makefile.am`. For example, when you use
‘`--makefile-name=Makefile.gnulib`’, `gnulib-tool` will generate
`Makefile.gnulib`.

With the option ‘`--automake-subdir`’, you are telling
`gnulib-tool` that you will include the generated file from the
`Makefile.am` in the top-level directory, rather than from a
`Makefile.am` in the same directory. For example, the top-level
`Makefile.am` might contain this directive:

```
include lib/Makefile.gnulib
```

The option ‘`--automake-subdir`’ is also supported in combination
with ‘`--with-tests`’ (see [Bundling the unit tests of the Gnulib modules](#Unit-tests)). Note that in this case,
however, the generated unit tests directory will contains a
`Makefile.am` and thus use a recursive `make` invocation.
This is not a problem, since the built artifacts of your package have
no dependencies towards the Gnulib unit tests, nor vice versa.

---

Next: [Caveat: `gettextize` and `autopoint` users](#gettextize-and-autopoint), Previous: [Building directly from the top-level directory](#Non_002drecursive-make), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 3.10 Using Gnulib for both a library and a program [¶](#Using-Gnulib-for-both-a-library-and-a-program)

Your project might build both a library and some accompanying programs
in the same source tree. In that case you might want to use different
modules for the library than for the programs. Typically the programs
might want to make use of `getopt-posix` or `version-etc`,
while the library wants to stay clear of these modules for technical
or licensing reasons.

Let’s assume that your project contains a `lib` directory where
the source of the library resides and a `src` directory for the
sources of the programs as follows.

```
.
|-- configure.ac
|-- lib
|   |-- foo.c
|   `-- Makefile.am
|-- Makefile.am
`-- src
    |-- bar.c
    `-- Makefile.am
```

You can now add two instances of Gnulib to your project in separate
source trees:

```
~/src/libfoo$ gnulib-tool --import --lib=libgnu --source-base=gnulib \
              --m4-base=gnulib/m4 --macro-prefix=gl strndup
~/src/libfoo$ gnulib-tool --import --lib=libgnutools \
              --source-base=src/gnulib --m4-base=src/gnulib/m4 \
              --macro-prefix=gl_tools getopt-gnu
```

The first one will import the module `strndup` in `gnulib`
and the second one will import `getopt-gnu` in `src/gnulib`
and you will end up with the following source tree (many files omitted
in the interest of brevity):

```
.
|-- configure.ac
|-- gnulib
|   |-- m4
|   |-- strndup.c
|-- lib
|   |-- foo.c
|   `-- Makefile.am
|-- Makefile.am
`-- src
    |-- bar.c
    |-- gnulib
    |   |-- getopt.c
    |   |-- getopt.in.h
    |   |-- m4
    `-- Makefile.am
```

As discussed in [Bundling the unit tests of the Gnulib modules](#Unit-tests), you may not use ‘`--with-tests`’
for this project since the `configure.ac` is shared.

Integration with your code is basically the same as outlined in
[Initial import](#Initial-import) with the one exception that you have to add both
the macro `gl_EARLY` and the macro `gl_tools_EARLY` to your
`configure.ac` (and of course also both macros `gl_INIT` and
`gl_tools_INIT`). Obviously the name of the second macro is
dependent on the value of the `--macro-prefix` option in your
`gnulib-tool` invocation.

```
...
AC_PROG_CC
gl_EARLY
gl_tools_EARLY
...
# For gnulib.
gl_INIT
gl_tools_INIT
...
```

Also as outlined in [Initial import](#Initial-import) you will have to add compiler
and linker flags. For the library you might have to add something
along the line of the following to your `Makefile.am`:

```
...
AM_CPPFLAGS = -I$(top_srcdir)/gnulib -I$(top_builddir)/gnulib
...
libfoo_la_LIBADD = $(top_builddir)/gnulib/libgnu.la
...
```

Correspondingly for the programs you will have to add something like
this:

```
...
AM_CPPFLAGS = -I$(top_srcdir)/src/gnulib -I$(top_builddir)/src/gnulib
...
LDADD = $(top_builddir)/src/gnulib/libgnutools.la
...
```

The name of the library that you have pass in the linker option
depends on the `--lib` option in `gnulib-tool`
invocation.

---

Next: [Handling Gnulib’s own message translations](#Localization), Previous: [Using Gnulib for both a library and a program](#Multiple-instances), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 3.11 Caveat: `gettextize` and `autopoint` users [¶](#Caveat_003a-gettextize-and-autopoint-users)

The programs `gettextize` and `autopoint`, part of
GNU `gettext`, import or update the internationalization infrastructure.
Some of this infrastructure, namely ca. 20 Autoconf macro files and the
`config.rpath` file, is also contained in Gnulib and may be imported
by `gnulib-tool`. The use of `gettextize` or `autopoint`
will therefore overwrite some of the files that `gnulib-tool` has
imported, and vice versa.

Avoiding to use `gettextize` (manually, as package maintainer) or
`autopoint` (as part of a script like `autoreconf` or
`autogen.sh`) is not the solution: These programs also import the
infrastructure in the `po/` and optionally in the `intl/` directory.

The copies of the conflicting files in Gnulib are more up-to-date than
the copies brought in by `gettextize` and `autopoint`. When a
new `gettext` release is made, the copies of the files in Gnulib will
be updated immediately.

The choice of which version of gettext to require depends on the needs
of your package. For a package that wants to comply to GNU Coding
Standards, the steps are:

1. When you run `gettextize`, always use the `gettextize` from the
   matching GNU gettext release. For the most recent Gnulib checkout, this is
   the newest release found on <https://ftp.gnu.org/gnu/gettext/>. For an
   older Gnulib snapshot, it is the release that was the most recent release
   at the time the Gnulib snapshot was taken.
2. After running `gettextize`, invoke `gnulib-tool` and import
   the `gettext` module. Also, copy the latest version of gnulib’s
   `build-aux/po/Makefile.in.in` to your `po/` directory (this
   is done for you if you use gnulib’s `autogen.sh` script).
3. If you get an error message like
   `*** error: gettext infrastructure mismatch:
   using a Makefile.in.in from gettext version ...
   but the Autoconf macros are from gettext version ...`,
   it means that a new GNU gettext release was made, and its Autoconf macros
   were integrated into Gnulib and now mismatch the `po/` infrastructure.
   In this case, fetch and install the new GNU gettext release and run
   `gettextize` followed by `gnulib-tool`.

On the other hand, if your package is not as concerned with compliance
to the latest standards, but instead favors development on stable
environments, the steps are:

1. Determine the oldest version of `gettext` that you intend to
   support during development (at this time, gnulib recommends going no
   older than version 0.17). Run `autopoint` (not
   `gettextize`) to copy infrastructure into place (newer versions
   of gettext will install the older infrastructure that you requested).
2. Invoke `gnulib-tool`, and import the `gettext-h` module.

Regardless of which approach you used to get the infrastructure in
place, the following steps must then be used to preserve that
infrastructure (gnulib’s `autogen.sh` script follows these rules):

1. When a script of yours run `autopoint`, invoke `gnulib-tool`
   afterwards.
2. When you invoke `autoreconf` after `gnulib-tool`, make sure to
   not invoke `autopoint` a second time, by setting the `AUTOPOINT`
   environment variable, like this:

   ```
   $ env AUTOPOINT=true autoreconf --install
   ```

---

Next: [Integration with Version Control Systems](#VCS-Issues), Previous: [Caveat: `gettextize` and `autopoint` users](#gettextize-and-autopoint), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 3.12 Handling Gnulib’s own message translations [¶](#Handling-Gnulib_0027s-own-message-translations)

Gnulib provides some functions that emit translatable messages using GNU
`gettext`. The ‘`gnulib`’ domain at the
[Translation Project](https://translationproject.org/) collects
translations of these messages, which you should incorporate into your
own programs.

The recommended way to achieve this is that
your package depends on the package ‘`gnulib-l10n`’.
This package contains the Gnulib localizations as `.mo` files.

In order to use this method, you must
– in each program that might use Gnulib code –
add an extra line to the part of the program
that initializes locale-dependent behavior.
Where you would normally write something like:

```
  setlocale (LC_ALL, "");
  bindtextdomain (PACKAGE, LOCALEDIR);
  textdomain (PACKAGE);
```

you should add an additional `bindtextdomain` call to inform gettext of
where the MO files for the extra message domain may be found:

```
  bindtextdomain ("gnulib", GNULIB_LOCALEDIR);
```

Since you do not change the `textdomain` call,
the default message domain for your program remains the same
and your own use of `gettext` functions will not be affected.

If your package has a `DEPENDENCIES` file,
for the ease of installers and distributors,
you should add the ‘`gnulib-l10n`’ dependency in this file.

If your package does not support localization,
you can simplify its build process by using
`gnulib-tool`’s `--avoid=gnulib-i18n` option.
The package need not call `bindtextdomain` or `textdomain`,
and all Gnulib-generated messages will be in American English.

Two older methods to use Gnulib localizations are deprecated:

* The first, and older, method
  is to list all the source files you use from Gnulib in your own
  `po/POTFILES.in` file. This will cause all the relevant
  translatable strings to be included in your POT file. When you send
  this POT file to the Translation Project, translators will normally fill
  in the translations of the Gnulib strings from their “translation
  memory”, and send you back updated PO files.
  For this to work, your program should define the macro
  `GNULIB_TEXT_DOMAIN` to its own textdomain name early,
  before including `gettext.h`;
  you can do this by putting `AC_DEFINE([GNULIB_TEXT_DOMAIN], [PACKAGE])`
  in your `configure.ac`.

  However, this process is error-prone: you might forget to list some
  source files, or the translator might not be using a translation memory
  and provide a different translation than another translator, or the
  translation might not be kept in sync between Gnulib and your package.
  It is also slow and causes substantial extra work, because a human
  translator must be in the loop for each language and you will need to
  incorporate their work on request.
* The second method is that
  you pass the `--po-base=directory` and `--po-domain=domain`
  options to `gnulib-tool`, then `gnulib-tool` creates a
  separate directory with its own `POTFILES.in`, and fetches current
  translations directly from the Translation Project (using `wget`).
  The POT file in this directory will be called
  `domain-gnulib.pot`, depending on the domain you gave to the
  `--po-domain` option (typically the same as the package name).
  This causes these translations to reside in a separate message domain,
  so that they do not clash either with the translations for the main part
  of your package nor with those of other packages on the system that use
  possibly different versions of Gnulib.
  When you use these options, the functions in Gnulib are built
  in such a way that they will always use this domain regardless of the
  default domain set by `textdomain`.

  This method is deprecated because it adds complexity to your package
  and, additionally, mixes the “autopull” and “autogen” phases of
  package source preparation.

---

Next: [Bundling the unit tests of the Gnulib modules](#Unit-tests), Previous: [Handling Gnulib’s own message translations](#Localization), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 3.13 Integration with Version Control Systems [¶](#Integration-with-Version-Control-Systems)

If a project stores its source files in a version control system (VCS),
such as CVS, Subversion, or Git, one needs to decide which files to commit.

In principle, all files created by `gnulib-tool`, except
`gnulib-cache.m4`, can be treated like generated source files,
like for example a `parser.c` file generated from
`parser.y`. Alternatively, they can be considered source files
and updated manually.

Here are the three different approaches in common use. Each has its
place, and you should use whichever best suits your particular project
and development methods.

1. In projects which commit all source files, whether generated or not,
   into their VCS, the `gnulib-tool` generated files should all be
   committed. In this case, you should pass the option
   ‘`--no-vc-files`’ to `gnulib-tool`, which avoids alteration of
   VCS-related files such as `.gitignore`.

   Gnulib also contains files generated by `make` (and removed by
   `make clean`), using information determined by
   `configure`. For a Gnulib source file of the form
   `lib/foo.in.h`, the corresponding `lib/foo.h` is such a
   `make`-generated file. These should *not* be checked
   into the VCS, but instead added to `.gitignore` or equivalent.
2. In projects which customarily omit from their VCS all files that are
   generated from other source files, none of these files and directories
   are added into the VCS. As described in [Modified imports](#Modified-imports), there
   are two ways to keep track of options and module names that are passed
   to `gnulib-tool`. The command for restoring the omitted files
   depends on it:
   * If they are stored in a file other than `gnulib-cache.m4`, such as
     `autogen.sh`, `bootstrap`, `bootstrap.conf`, or similar,
     the restoration command is the entire `gnulib-tool ... --import ...`
     invocation with all options and module names.
   * If the project relies on `gnulib-tool`’s memory of the last used
     options and module names, then the file `gnulib-cache.m4` in the M4
     macros directory must be added to the VCS, and the restoration command
     is:

     ```
     $ gnulib-tool --update
     ```

     The ‘`--update`’ option operates much like the ‘`--add-import`’
     option, but it does not offer the possibility to change the way Gnulib is
     used. Also it does not report in the ChangeLogs the files that it had to
     add because they were missing.

   Most packages nowadays use the first among these two approaches. Over
   time, three ways of handling version control have evolved.

   In the cases (A) and (B), a “git submodule” is used to reference
   the precise commit of the gnulib repository, so that each developer
   running ‘`./bootstrap --pull`’ or `autopull.sh`
   will get the same version of all gnulib-provided
   files.

   The alternative is to always follow the newest Gnulib automatically.
   Note that this can cause breakages at unexpected moments, namely
   when a broken commit is pushed in Gnulib. It does not happen often,
   but it does happen.

   * (A)
     In this approach, the developers use a git submodule manually.

     The location of the submodule can be chosen to fit the package’s needs;
     here’s how to initially create the submodule in the directory `gnulib`:

     ```
     $ git submodule add -- https://git.savannah.gnu.org/git/gnulib.git gnulib
     ```

     Thereafter, the developer will run this command to update the
     submodule to the recorded checkout level:

     ```
     $ git submodule update --init gnulib
     ```

     Use this sequence to update to a newer version of gnulib:

     ```
     $ git submodule update --remote gnulib
     $ git add gnulib
     $ ./bootstrap --bootstrap-sync
     ```

     If multiple submodules are used, the following may be useful:

     ```
     $ git config alias.syncsub "submodule foreach git pull origin master"
     $ git syncsub
     ```
   * (B)
     In this approach, the `build-aux/bootstrap` or `autopull.sh`
     program (see [Programs for developing in Git checkouts](#Developer-tools)) is used to aid a developer in
     using this setup. You copy this program (and if it’s
     `autopull.sh`, its companion files) into your package and place
     the copy or copies under version control. The program can be
     customized using `bootstrap.conf` which you also put under
     version control.
   * (C)
     In this approach, you write the `autopull.sh` and `autogen.sh`
     files by hand.

     `autopull.sh` is most easily written as a script that
     invokes

     ```
     ./gitsub.sh pull || exit 1
     ```

     where `gitsub.sh` is described in [Programs for developing in Git checkouts](#Developer-tools).

     `autogen.sh` typically contains an explicit `gnulib-tool`
     invocation, followed by

     ```
     aclocal -I m4 \
       && autoconf \
       && autoheader && touch config.h.in \
       && automake --add-missing --copy \
       && rm -rf autom4te.cache \
       || exit $?
     ```
3. Some projects take a “middle road”: they do commit Gnulib source
   files as in the first approach, but they do not commit other derived
   files, such as a `Makefile.in` generated by Automake. This
   increases the size and complexity of the repository, but can help
   occasional contributors by not requiring them to have a full Gnulib
   checkout to do a build, and all developers by ensuring that all
   developers are working with the same version of Gnulib in the
   repository. It also supports multiple Gnulib instances within a
   project. It remains important not to commit the
   `make`-generated files, as described above.

---

Next: [Avoiding unnecessary checks and compilations](#Conditional-dependencies), Previous: [Integration with Version Control Systems](#VCS-Issues), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 3.14 Bundling the unit tests of the Gnulib modules [¶](#Bundling-the-unit-tests-of-the-Gnulib-modules)

You can bundle the unit tests of the Gnulib modules together with your
package, through the ‘`--with-tests`’ option. Together with
‘`--with-tests`’, you also specify the directory for these tests
through the ‘`--tests-base`’ option. Of course, you need to add this
directory to the `SUBDIRS` variable in the `Makefile.am` of
the parent directory.

The advantage of having the unit tests bundled is that when your program
has a problem on a particular platform, running the unit tests may help
determine quickly if the problem is on Gnulib’s side or on your package’s
side. Also, it helps verifying Gnulib’s portability, of course.

The unit tests will be compiled and run when the user runs ‘`make check`’.
When the user runs only ‘`make`’, the unit tests will not be compiled.

In the `SUBDIRS` variable, it is useful to put the Gnulib tests directory
after the directory containing the other tests, not before:

```
SUBDIRS = gnulib-lib src man tests gnulib-tests
```

This will ensure that on platforms where there are test failures in either
directory, users will see and report the failures from the tests of your
program.

Note: In packages which use more than one invocation of `gnulib-tool`
in the scope of the same `configure.ac`, you cannot use
‘`--with-tests`’. You will have to use a separate `configure.ac`
in this case.

---

Previous: [Bundling the unit tests of the Gnulib modules](#Unit-tests), Up: [Invoking gnulib-tool](#Invoking-gnulib_002dtool)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 3.15 Avoiding unnecessary checks and compilations [¶](#Avoiding-unnecessary-checks-and-compilations)

In some cases, a module is needed by another module only on specific
platforms. But when a module is present, its Autoconf checks are always
executed, and its `Makefile.am` additions are always enabled. So
it can happen that some Autoconf checks are executed and some source files
are compiled, although no other module needs them on this particular
platform, just *in case* some other module would need them.

The option ‘`--conditional-dependencies`’ enables an optimization of
configure checks and `Makefile.am` snippets that avoids this. With
this option, whether a module is considered “present” is no longer decided
when `gnulib-tool` is invoked, but later, when `configure` is run.
This applies to modules that were added as dependencies while
`gnulib-tool` was run; modules that were passed on the command line
explicitly are always “present”.

For example, the `timegm` module needs, on platforms
where the system’s `timegm` function is missing or buggy, a replacement
that is based on a function `mktime_internal`. The module
`mktime-internal` that provides this function provides it on all
platforms. So, by default, the file `mktime-internal.c` will be
compiled on all platforms, even on glibc and BSD systems which have a
working `timegm` function. When the option
‘`--conditional-dependencies`’ is given, on the other hand, and if
`mktime-internal` was not explicitly required on the command line,
the file `mktime-internal.c` will only be compiled on the platforms
where the `timegm` needs them.

Conditional dependencies are specified in the module description by putting
the condition on the same line as the dependent module, enclosed in brackets.
The condition is a boolean shell expression that can assume that the
`configure.ac` snippet from the module description has already been
executed. In the example above, the dependency from `timegm` to
`mktime-internal` is written like this:

```
Depends-on:
...
mktime-internal [test $HAVE_TIMEGM = 0 || test $REPLACE_TIMEGM = 1]
...
```

Note: The option ‘`--conditional-dependencies`’ cannot be used together
with the option ‘`--with-tests`’. It also cannot be used when a package
uses `gnulib-tool` for several subdirectories, with different values
of ‘`--source-base`’, in the scope of a single `configure.ac` file.

---

Next: [Extending Gnulib](#Extending-Gnulib), Previous: [Invoking gnulib-tool](#Invoking-gnulib_002dtool), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 4 Writing modules [¶](#Writing-modules-1)

This chapter explains how to write modules of your own, either to
extend Gnulib for your own package (see [Extending Gnulib](#Extending-Gnulib)), or for
inclusion in gnulib proper.

The guidelines in this chapter do not necessarily need to be followed for
using `gnulib-tool`. They merely represent a set of good practices.
Following them will result in a good structure of your modules and in
consistency with gnulib.

* [Source code files](#Source-code-files)
* [Header files](#Header-files)
* [Implementation files](#Implementation-files)
* [Specification](#Specification)
* [Module description](#Module-description)
* [Autoconf macros](#Autoconf-macros)
* [Making proper use of `AC_LIBOBJ`](#Using-AC_005fLIBOBJ)
* [Unit test modules](#Unit-test-modules)
* [Incompatible changes](#Incompatible-changes)

---

Next: [Header files](#Header-files), Up: [Writing modules](#Writing-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 4.1 Source code files [¶](#Source-code-files-1)

Every API (C functions or variables) provided should be declared in a header
file (.h file) and implemented in one or more implementation files (.c files).
The separation has the effect that users of your module need to read only
the contents of the .h file and the module description in order to understand
what the module is about and how to use it—not the entire implementation.
Furthermore, users of your module don’t need to repeat the declarations of
the functions in their code, and are likely to receive notification through
compiler errors if you make incompatible changes to the API (like, adding a
parameter or changing the return type of a function).

---

Next: [Implementation files](#Implementation-files), Previous: [Source code files](#Source-code-files), Up: [Writing modules](#Writing-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 4.2 Header files [¶](#Header-files-1)

The .h file should declare the C functions and variables that the module
provides.

The .h file should be stand-alone. That is, it does not require other .h files
to be included before. Rather, it includes all necessary .h files by itself.

It is a tradition to use CPP tricks to avoid parsing the same header
file more than once, which might cause warnings. The trick is to wrap
the content of the header file (say, `foo.h`) in a block, as in:

```
#ifndef FOO_H
# define FOO_H
...
body of header file goes here
...
#endif /* FOO_H */
```

Whether to use `FOO_H` or `_FOO_H` is a matter of taste and
style. The C99 and C11 standards reserve all identifiers that begin with an
underscore and either an uppercase letter or another underscore, for
any use. Thus, in theory, an application might not safely assume that
`_FOO_H` has not already been defined by a library. On the other
hand, using `FOO_H` will likely lead the higher risk of
collisions with other symbols (e.g., `KEY_H`, `XK_H`, `BPF_H`,
which are CPP macro constants, or `COFF_LONG_H`, which is a CPP
macro function). Your preference may depend on whether you consider
the header file under discussion as part of the application (which has
its own namespace for CPP symbols) or a supporting library (that
shouldn’t interfere with the application’s CPP symbol namespace).

Adapting C header files for use in C++ applications can use another
CPP trick, as in:

```
# ifdef __cplusplus
extern "C"
{
# endif
...
body of header file goes here
...
# ifdef __cplusplus
}
# endif
```

The idea here is that `__cplusplus` is defined only by C++
implementations, which will wrap the header file in an ‘`extern "C"`’
block. Again, whether to use this trick is a matter of taste and
style. While the above can be seen as harmless, it could be argued
that the header file is written in C, and any C++ application using it
should explicitly use the ‘`extern "C"`’ block itself. Your
preference might depend on whether you consider the API exported by
your header file as something available for C programs only, or for C
and C++ programs alike.

Note that putting a `#include` in an `extern "C" { ... }`
block yields a syntax error in C++ mode on some platforms (e.g., glibc
systems with g++ v3.3 to v4.2, AIX). For this reason, it
is recommended to place the `#include` before the `extern
"C"` block.

---

Next: [Specification](#Specification), Previous: [Header files](#Header-files), Up: [Writing modules](#Writing-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 4.3 Implementation files [¶](#Implementation-files-1)

The .c file or files implement the functions and variables declared in the
.h file.

#### Include ordering [¶](#Include-ordering)

Every implementation file must start with ‘`#include <config.h>`’.
This is necessary for activating the preprocessor macros that are defined
on behalf of the Autoconf macros. Some of these preprocessor macros,
such as `_GNU_SOURCE`, would have no effect if defined after a system
header file has already been included.

Then comes the ‘`#include "..."`’ specifying the header file that is
being implemented. Putting this right after ‘`#include <config.h>`’
has the effect that it verifies that the header file is self-contained.

Then come the system and application headers. It is customary to put all the
system headers before all application headers, so as to minimize the risk
that a preprocessor macro defined in an application header confuses the system
headers on some platforms.

In summary:

* First comes #include <config.h>.
* Second comes the #include "..." specifying the module being implemented.
* Then come all the #include <...> of system or system-replacement headers,
  in arbitrary order.
* Then come all the #include "..." of gnulib and application headers, in
  arbitrary order.

---

Next: [Module description](#Module-description), Previous: [Implementation files](#Implementation-files), Up: [Writing modules](#Writing-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 4.4 Specification [¶](#Specification-1)

The specification of a function should answer at least the following
questions:

* What is the purpose of the function?
* What are the arguments?
* What is the return value?
* What happens in case of failure? (Exit? A specific return value? Errno set?)
* Memory allocation policy: If pointers to memory are returned, are they freshly
  allocated and supposed to be freed by the caller?

Where to put the specification describing exported functions? Three practices
are used in gnulib:

* The specification can be as comments in the header file, just above the
  function declaration.
* The specification can be as comments in the implementation file, just above
  the function definition.
* The specification can be in texinfo format, so that it gets included in the
  gnulib manual.

In any case, the specification should appear in just one place, unless you can
ensure that the multiple copies will always remain identical.

The advantage of putting it in the header file is that the user only has to
read the include file normally never needs to peek into the implementation
file(s).

The advantage of putting it in the implementation file is that when reviewing
or changing the implementation, you have both elements side by side.

The advantage of texinfo formatted documentation is that it is easily
published in HTML or Info format.

Currently (as of 2020), 70% of gnulib uses the first practice, 25% of
gnulib uses the second practice, and a small minority uses the texinfo
practice.

---

Next: [Autoconf macros](#Autoconf-macros), Previous: [Specification](#Specification), Up: [Writing modules](#Writing-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 4.5 Module description [¶](#Module-description-1)

For the module description, you can start from an existing module’s
description, or from a blank one: `module/TEMPLATE` for a normal module,
or `module/TEMPLATE-TESTS` for a unit test module. Some more fields
are possible but rarely used. Use `module/TEMPLATE-EXTENDED` if you
want to use one of them.

Module descriptions have the following fields. Absent fields are equivalent
to fields with empty contents.

Description
:   This field should contain a concise description of the module’s functionality.
    One sentence is enough. For example, if it defines a single function
    ‘`frob`’, the description can be ‘`frob() function: frobnication.`’
    Gnulib’s documentation generator will automatically convert the first part
    to a hyperlink when it has this form.

Status
:   This field is either empty/absent, or contains the word ‘`obsolete`’. In
    the latter case, `gnulib-tool` will, unless the option
    `--with-obsolete` is given, omit it when it used as a dependency. It is
    good practice to also notify the user about an obsolete module. This is done
    by putting into the ‘`Notice`’ section (see below) text like
    ‘`This module is obsolete.`’

Notice
:   This field contains text that `gnulib-tool` will show to the user
    when the module is used. This can be a status indicator like
    ‘`This module is obsolete.`’ or additional advice. Do not abuse this
    field.

Applicability
:   This field is either empty/absent, or contains the word ‘`all`’. It
    describes to which `Makefile.am` the module is applied. By default,
    a normal module is applied to `source_base/Makefile.am`
    (normally `lib/Makefile.am`), whereas a module ending in `-tests`
    is applied to `tests_base/Makefile.am` (normally
    `tests/Makefile.am`). If this field is ‘`all`’, it is applied to
    both `Makefile.am`s. This is useful for modules which provide
    Makefile.am macros rather than compiled source code.

Usable-in-testdir
:   This field should contain ‘`no`’ if the module cannot be used in a
    test directory created with `gnulib-tool --create-testdir`.

Files
:   This field contains a newline separated list of the files that are part of
    the module. `gnulib-tool` copies these files into the package that
    uses the module.

    This list is typically ordered by importance: First comes the header file,
    then the implementation files, then other files.

    It is possible to have the same file mentioned in multiple modules. That is,
    if the maintainers of that module agree on the purpose and future of said
    file.

Depends-on
:   This field contains a newline separated list of the modules that are required
    for the proper working of this module. `gnulib-tool` includes each
    required module automatically, unless it is specified with option
    `--avoid` or it is marked as obsolete and the option
    `--with-obsolete` is not given.

    A test modules `foo-tests` implicitly depends on the corresponding non-test
    module `foo`. `foo` implicitly depends on `foo-tests` if the
    latter exists and if the option `--with-tests` has been given.

    Tests modules can depend on non-tests modules. Non-tests modules should not
    depend on tests modules. (Recall that tests modules are built in a separate
    directory.)

    Each listed required module may be declared a conditional dependency. This
    is indicated by placing the condition for the dependency on the same line,
    enclosed in brackets, after the name of the required module. The condition
    is a shell expression that is run after the module’s `configure.ac`
    statements. For example:

    ```
    strtoull   [test $ac_cv_func_strtoumax = no]
    ```

    Lines starting with `#` are recognized as comments and are ignored.

configure.ac-early
:   This field contains `configure.ac` stuff (Autoconf macro invocations and
    shell statements) that are logically placed early in the `configure.ac`
    file: right after the `AC_PROG_CC` invocation. This section is adequate
    for statements that modify `CPPFLAGS`, as these can affect the results of
    other Autoconf macros.

configure.ac
:   This field contains `configure.ac` stuff (Autoconf macro invocations and
    shell statements).

    It is forbidden to add items to the `CPPFLAGS` variable here, other than
    temporarily, as these could affect the results of other Autoconf macros.

    We avoid adding items to the `LIBS` variable, other than temporarily.
    Instead, the module can export an Autoconf-substituted variable that contains
    link options. The user of the module can then decide to which executables
    to apply which link options. Recall that a package can build executables of
    different kinds and purposes; having all executables link against all
    libraries is inappropriate.

    If the statements in this section grow larger than a couple of lines, we
    recommend moving them to a `.m4` file of their own.

Makefile.am
:   This field contains `Makefile.am` statements. Variables like
    `lib_SOURCES` are transformed to match the name of the library
    being built in that directory. For example, `lib_SOURCES` may become
    `libgnu_a_SOURCES` (for a plain library) or `libgnu_la_SOURCES`
    (for a libtool library). Therefore, the normal way of having an
    implementation file `lib/foo.c` compiled unconditionally is to write

    ```
    lib_SOURCES += foo.c
    ```

Include
:   This field contains the preprocessor statements that users of the module
    need to add to their source code files. Typically it’s a single include
    statement. A shorthand is allowed: You don’t need to write the word
    “#include”, just the name of the include file in the way it will appear
    in an include statement. Example:

    ```
    "foo.h"
    ```

Link
:   This field contains the set of libraries that are needed when linking
    libraries or executables that use this module. Often this will be
    written as a reference to a Makefile variable. Please write them
    one per line, so that `gnulib-tool` can remove duplicates
    when presenting a summary to the user.
    Example:

    ```
    $(POW_LIBM)
    $(LTLIBICONV) when linking with libtool, $(LIBICONV) otherwise
    ```

    When this field is omitted, it defaults to the union of the `Link`
    field of the dependencies.

License
:   This field specifies the license that governs the source code parts of
    this module. See [Copyright](#Copyright) for details. Be sure to place, in every
    source code file, a copyright notice and the appropriate license notice,
    taken from the `etc/license-notices/` directory.

Maintainer
:   This field specifies the persons who have a definitive say about proposed
    changes to this module. You don’t need to mention email addresses here:
    they can be inferred from the `ChangeLog` file.

    Please put at least one person here. We don’t like unmaintained modules.

---

Next: [Making proper use of `AC_LIBOBJ`](#Using-AC_005fLIBOBJ), Previous: [Module description](#Module-description), Up: [Writing modules](#Writing-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 4.6 Autoconf macros [¶](#Autoconf-macros-1)

For a module `foo`, an Autoconf macro file `m4/foo.m4` is typically
created when the Autoconf macro invocations for the module are longer than
one or two lines.

The name of the main entry point into this Autoconf macro file is typically
`gl_FOO`. For modules outside Gnulib that are not likely to be moved
into Gnulib, please use a prefix specific to your package: `gt_` for
GNU gettext, `cu_` for GNU coreutils, etc.

For modules that define a function `foo`, the entry point is called
`gl_FUNC_FOO` instead of `gl_FOO`. For modules that provide a
header file with multiple functions, say `foo.h`, the entry point is
called `gl_FOO_H` or `gl_HEADER_FOO_H`. This convention is useful
because sometimes a header and a function name coincide (for example,
`fcntl` and `fcntl.h`).

For modules that provide a replacement, it is useful to split the Autoconf
macro into two macro definitions: one that detects whether the replacement
is needed and requests the replacement by setting a `HAVE_FOO`
variable to 0 or a `REPLACE_FOO` variable to 1 (this is the
entry point, say `gl_FUNC_FOO`), and one that arranges for the macros
needed by the replacement code `lib/foo.c` (typically called
`gl_PREREQ_FOO`). The reason of this separation is

1. to make it easy to update the Autoconf macros when you have modified the
   source code file: after changing `lib/foo.c`, all you have to review
   is the `Depends-on` section of the module description and the
   `gl_PREREQ_FOO` macro in the Autoconf macro file.
2. The Autoconf macros are often large enough that splitting them eases
   maintenance.

Autoconf macro files in Gnulib all start with a header consisting of

1. A comment line with the file name.
   This is useful because in some cases the generated `aclocal.m4`
   file does not contain a reference to the `.m4` file but its entire
   contents.
   The comment makes it clear which `.m4` file is where in the
   `aclocal.m4` file.
2. A comment line with a serial number.
   This is useful when people use the ‘`aclocal --install`’ command.
   `aclocal` will then refrain from copying a file with a smaller
   serial number onto a file with a larger serial number.
   The serial number should be a positive integer on the main branch,
   or a positive fractional number on a stable branch.
3. The copyright and license header, as usual.

---

Next: [Unit test modules](#Unit-test-modules), Previous: [Autoconf macros](#Autoconf-macros), Up: [Writing modules](#Writing-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 4.7 Making proper use of `AC_LIBOBJ` [¶](#Making-proper-use-of-AC_005fLIBOBJ)

Source files that provide a replacement should be only compiled on the
platforms that need this replacement. While it is actually possible
to compile a `.c` file whose contents is entirely `#ifdef`’ed
out on the platforms that don’t need the replacement, this practice is
discouraged because

* It makes the build time longer than needed, by invoking the compiler for
  nothing.
* It produces a `.o` file that suggests that a replacement was needed.
* Empty object files produce a linker warning on some platforms: MSVC.

The typical idiom for invoking `AC_LIBOBJ` is thus the following,
in the module description:

```
if test $HAVE_FOO = 0 || test $REPLACE_FOO = 1; then
  AC_LIBOBJ([foo])
  gl_PREREQ_FOO
fi
```

Important: Do not place `AC_LIBOBJ` invocations in the Autoconf
macros in the `m4/` directory. The purpose of the Autoconf macros
is to determine what features or bugs the platform has, and to make
decisions about which replacements are needed. The purpose of the
`configure.ac` and `Makefile.am` sections of the module
descriptions is to arrange for the replacements to be compiled.
**Source file names do not belong in the `m4/` directory.**

When an `AC_LIBOBJ` invocation is unconditional, it is simpler
to just have the source file compiled through an Automake variable
augmentation: In the `Makefile.am` section write

```
lib_SOURCES += foo.c
```

When a module description contains an `AC_LIBOBJ([foo])`
invocation, you **must** list the source file `lib/foo.c`
in the `Files` section. This is needed even if the module
depends on another module that already lists `lib/foo.c` in its
`Files` section – because your module might be used among
the test modules (in the directory specified through ‘`--tests-base`’)
and the other module among the main modules (in the directory specified
through ‘`--source-base`’), and in this situation, the
`AC_LIBOBJ([foo])` of your module can only be satisfied by having
`foo.c` be present in the tests source directory as well.

---

Next: [Incompatible changes](#Incompatible-changes), Previous: [Making proper use of `AC_LIBOBJ`](#Using-AC_005fLIBOBJ), Up: [Writing modules](#Writing-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 4.8 Unit test modules [¶](#Unit-test-modules-1)

A unit test that is a simple C program usually has a module description as
simple as this:

```
Files:
tests/test-foo.c
tests/macros.h

Depends-on:

configure.ac:

Makefile.am:
TESTS += test-foo
check_PROGRAMS += test-foo
```

The test program `tests/test-foo.c` often has the following structure:

* First comes the obligatory ‘`#include <config.h>`’.
* Second comes the include of the header file that declares the API being tested.
  Including it here verifies that said header file is self-contained.
* Then come other includes. In particular, the file `macros.h` is often
  used here. It contains a convenient `ASSERT` macro.

The body of the test, then, contains many `ASSERT` invocations. When
a test fails, the `ASSERT` macro prints the line number of the failing
statement, thus giving you, the developer, an idea of which part of the test
failed, even when you don’t have access to the machine where the test failed
and the reporting user cannot run a debugger.

Sometimes it is convenient to write part of the test as a shell script.
(For example, in areas related to process control or interprocess
communication, or when different locales should be tried.) In these cases,
the typical module description is like this:

```
Files:
tests/test-foo.sh
tests/test-foo.c
tests/macros.h

Depends-on:

configure.ac:

Makefile.am:
TESTS += test-foo.sh
TESTS_ENVIRONMENT += FOO_BAR='@FOO_BAR@'
check_PROGRAMS += test-foo
```

Here, the `TESTS_ENVIRONMENT` variable can be used to pass values
determined by `configure` or by the `Makefile` to the shell
script, as environment variables. The Autoconf values `EXEEXT`
and `srcdir` are already provided as environment variables,
through an initial value of `TESTS_ENVIRONMENT` that
`gnulib-tool` puts in place.

Regardless of the specific form of the unit test, the following guidelines
should be respected:

* A test indicates success by exiting with exit code 0. It should normally
  not produce output in this case. (Output to temporary files that are
  cleaned up at the end of the test are possible, of course.)
* A test indicates failure by exiting with an exit code different from 0 and 77,
  typically 1. It is useful to print a message about the failure in this case.
  The `ASSERT` macro already does so.
* A test indicates "skip", that is, that most of its interesting functionality
  could not be performed, through a return code of 77. A test should also
  print a message to stdout or stderr about the reason for skipping.
  For example:

  ```
    fputs ("Skipping test: multithreading not enabled\n", stderr);
    return 77;
  ```

  Such a message helps detecting bugs in the autoconf macros: A simple message
  ‘`SKIP: test-foo`’ does not sufficiently catch the attention of the user.

---

Previous: [Unit test modules](#Unit-test-modules), Up: [Writing modules](#Writing-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 4.9 Incompatible changes [¶](#Incompatible-changes-1)

Incompatible changes to Gnulib modules should be mentioned in Gnulib’s
`NEWS` file. Incompatible changes here mean that existing source code
may not compile or work any more.

We don’t mean changes in the binary interface (ABI), since

1. Gnulib code is used in source-code form.
2. The user who distributes libraries that contain Gnulib code is supposed to
   bump the version number in the way described in the Libtool documentation
   before every release.

---

Next: [Miscellaneous Notes](#Miscellaneous-Notes), Previous: [Writing modules](#Writing-modules), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 5 Extending Gnulib [¶](#Extending-Gnulib-1)

Gnulib modules are intended to be suitable for widespread use. Most
problems with Gnulib can and should be fixed in a generic way, so that
all of Gnulib’s users can benefit from the change. But occasionally a
problem arises that is difficult or undesirable to fix generically, or
a project that uses Gnulib may need to work around an issue before the
Gnulib maintainers commit a final fix. Maintainers may also want to
add their own pools of modules to projects as Gnulib “staging
areas.”

The obvious way to make local changes to Gnulib modules is to use
`gnulib-tool` to check out pristine modules, then to modify
the results in-place. This works well enough for short-lived
experiments. It is harder to keep modified versions of Gnulib modules
for a long time, even though Git (or another distributed version
control systems) can help out a lot with this during the development
process.

Git, however, doesn’t address the distribution issue. When a package
“foobar” needs a modified version of, say, `stdint.in.h`, it
either has to put a comment into `foobar/autogen.sh` saying
“Attention! This doesn’t work with a pristine Gnulib, you need this
and that patch after checking out Gnulib,” or it has to use the
‘`--avoid=stdint-h`’ option and provide the modified `stdint-h`
module in a different directory.

The `--local-dir` option to `gnulib-tool` solves this
problem. It allows the package to override or augment Gnulib. This
means:

* You can store files that are to override Gnulib files or modules.
* You can store context diffs to be applied to Gnulib files.
* You can add modules of your own, that are not (yet) in Gnulib.
* You can also add unstructured amounts of code to the library, by
  grouping the non-Gnulib files of the library in a single kitchen-sink
  “module.” (This kind of kitchen-sink module is not needed when you
  use the `gnulib-tool` option `--makefile-name`.)

In a release tarball, you can distribute the contents of this
`--local-dir` directory that will be combinable with newer
versions of Gnulib, barring incompatible changes to Gnulib.

If the `--local-dir=directory` option is specified, then
`gnulib-tool` looks in `directory` whenever it
reads a file from the Gnulib directory. Suppose `gnulib-tool`
is looking for file. Then:

* If `directory/file` exists, then `gnulib-tool` uses
  it instead of the file included in Gnulib.
* Otherwise, if `directory/file.diff` exists, then
  `gnulib-tool` uses the file from Gnulib after applying the diff
  using the `patch` program.
* Otherwise, `gnulib-tool` uses the file included in Gnulib.

You can specify the `--local-dir` multiple times. In this case,
the first specified directory has the highest precedence. That is, a
`file` found in one directory will shadow any `file`
and `file.diff` in the later directories and in the Gnulib
directory. And a file `file.diff` found in one directory will
be applied on top of the combination of `file` and
`file.diff` files found in the later directories and in the
Gnulib directory.

Please make wise use of this option. It also allows you to easily
hold back modifications you make to Gnulib macros in cases it may be
better to share them.

---

Next: [Building the ISO C and POSIX Substitutes](#POSIX-Substitutes-Library), Previous: [Extending Gnulib](#Extending-Gnulib), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 6 Miscellaneous Notes [¶](#Miscellaneous-Notes-1)

* [Out of memory handling](#Out-of-memory-handling)
* [Obsolete modules](#Obsolete-modules)
* [Extra tests modules](#Extra-tests-modules)
* [Modules that modify the way other modules work](#Modules-that-modify-the-way-other-modules-work)
* [A C++ namespace for gnulib](#A-C_002b_002b-namespace-for-gnulib)
* [License Texinfo sources](#License-Texinfo-sources)
* [Building gnulib](#Building-gnulib)
* [Gnulib Git Bundle](#Gnulib-Git-Bundle)

---

Next: [Obsolete modules](#Obsolete-modules), Up: [Miscellaneous Notes](#Miscellaneous-Notes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 6.1 Out of memory handling [¶](#Out-of-memory-handling-1)

The gnulib API does not have a standard error code for the out of memory
error condition. Instead of adding a non-standard error code, gnulib
has chosen to adopt a different strategy. Out of memory handling
happens in rare situations, but performing the out of memory error
handling after almost all API function invocations pollute your source
code and might make it harder to spot more serious problems. The
strategy chosen improves code readability and robustness.

For most applications, aborting the application with an error message
when the out of memory situation occurs is the best that can be wished
for. This is how the library behaves by default (using
the ‘`xalloc-die`’ module).

However, we realize that some applications may not want to abort
execution in any situation. Gnulib supports a hook to let the
application regain control and perform its own cleanups when an out of
memory situation has occurred. The application can define a function
(having a `void` prototype, i.e., no return value and no
parameters) and set the library variable
`xalloc_die` to that function. The variable should be
declared as follows.

```
extern void (*xalloc_die) (void);
```

Gnulib will invoke this function if an out of memory error occurs. Note
that the function should not return. Of course, care must be taken to
not allocate more memory, as that will likely also fail.

---

Next: [Extra tests modules](#Extra-tests-modules), Previous: [Out of memory handling](#Out-of-memory-handling), Up: [Miscellaneous Notes](#Miscellaneous-Notes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 6.2 Obsolete modules [¶](#Obsolete-modules-1)

Modules can be marked obsolete. This means that the problems they fix
don’t occur any more on the platforms that are reasonable porting targets
now. `gnulib-tool` warns when obsolete modules are mentioned on the
command line, and by default ignores dependencies from modules to obsolete
modules. When you pass the option `--with-obsolete` to
`gnulib-tool`, dependencies to obsolete modules will be included,
however, unless blocked through an `--avoid` option. This option
is useful if your package should be portable even to very old platforms.

In order to mark a module obsolete, you need to add this to the module
description:

```
Status:
obsolete

Notice:
This module is obsolete.
```

---

Next: [Modules that modify the way other modules work](#Modules-that-modify-the-way-other-modules-work), Previous: [Obsolete modules](#Obsolete-modules), Up: [Miscellaneous Notes](#Miscellaneous-Notes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 6.3 Extra tests modules [¶](#Extra-tests-modules-1)

Test modules can be marked with some special status attributes. When a
test module has such an attribute, `gnulib-tool --import` will not
include it by default.

The supported status attributes are:

`c++-test`
:   Indicates that the test is testing C++ interoperability. Such a test is
    useful in a C++ or mixed C/C++ package, but is useless in a C package.

`longrunning-test`
:   Indicates that the test takes a long time to compile or execute (more
    than five minutes or so). Such a test is better avoided in a release
    that is made for the general public.

`privileged-test`
:   Indicates that the test will request special privileges, for example,
    ask for the superuser password. Such a test may hang when run
    non-interactively and is therefore better avoided in a release that is
    made for the general public.

`unportable-test`
:   Indicates that the test is known to fail on some systems, and that
    there is no workaround about it. Such a test is better avoided in a
    release that is made for the general public.

`gnulib-tool --import --with-tests` will not include tests marked with
these attributes by default. When `gnulib-tool` is invoked with one
of the options `--with-c++-tests`, `--with-longrunning-tests`,
`--with-privileged-tests`, `--with-unportable-tests`, it
will include tests despite the corresponding special status attribute.
When `gnulib-tool` receives the option `--with-all-tests`,
it will include all tests regardless of their status attributes.

`gnulib-tool --create-testdir --with-tests` and
`gnulib-tool --create-megatestdir --with-tests` by default include all
tests of modules specified on the command line, regardless of their status
attributes. Tests of modules occurring as dependencies are not included
by default if they have one of these status attributes. The options
`--with-c++-tests`, `--with-longrunning-tests`,
`--with-privileged-tests`, `--with-unportable-tests` are
recognized here as well. Additionally, `gnulib-tool` also
understands the options `--without-c++-tests`,
`--without-longrunning-tests`, `--without-privileged-tests`,
`--without-unportable-tests`.

In order to mark a module with a status attribute, you need to add it
to the module description, like this:

```
Status:
longrunning-test
```

If only a part of a test deserves a particular status attribute, you
can split the module into a primary and a secondary test module,
say `foo-tests` and `foo-extra-tests`. Then add a dependency
from `foo-tests` to `foo-extra-tests`, and mark the
`foo-extra-tests` with the particular status attribute.

---

Next: [A C++ namespace for gnulib](#A-C_002b_002b-namespace-for-gnulib), Previous: [Extra tests modules](#Extra-tests-modules), Up: [Miscellaneous Notes](#Miscellaneous-Notes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 6.4 Modules that modify the way other modules work [¶](#Modules-that-modify-the-way-other-modules-work-1)

The normal way to design modules is that each module has its own code,
and the module dependencies provide the facilities on which this code
can rely. But sometimes it is necessary to use more advanced
techniques. For example:

* You may want to have optional module dependencies: Let module A use
  facilities provided by module B, if module B is present, but without
  requiring that module B is present.
* A module can indicate support for particular behaviours. For example,
  Gnulib has a module ‘`sigpipe`’ that requests POSIX compatible
  SIGPIPE behaviour from all other modules – something that is not
  enabled by default. Or consider the ‘`nonblocking`’ module, that is
  an indicator that all I/O functions should handle non-blocking file
  descriptors – something that, equally, is not enabled by default.
* A module can indicate to other modules that they can rely on certain
  guarantees, and thus omit specific code. For example, when Gnulib’s
  ‘`malloc-gnu`’ module is present, you can omit code that test
  `n` against zero when you call `malloc (n)`.

Be aware that these advanced techniques likely cause breakage in the
situation of multiple `gnulib-tool` invocations in the scope of a
single `configure` file. This is because the question “is module
B present?” does not have a unique answer in such situations.
`gnulib-tool` has support for these techniques in the situation of
`--create-testdir` (without option `--two-configures`),
which basically has two
`gnulib-tool` invocations, one for a set of modules that end up in
`gllib`, and one for the set of modules that end up in
`gltests`. But you should be aware that this does not cover the
general situation.

Which technique to use, depends on the answer to the question: “If my
module occurs among the modules of `gltests`, should it have an
effect on the modules in `gllib`?”

If the answer is “no”, your module description should invoke the
Autoconf macro `gl_MODULE_INDICATOR`. This Autoconf macro takes
one argument: the name of your module. The effect of
`gl_MODULE_INDICATOR([my-module])` is to define, in
`config.h`, a C macro `GNULIB_MY_MODULE` that indicates
whether your macro is considered to be present. This works even when
your macro is used in `gltests`: `GNULIB_MY_MODULE`
will then evaluate to 1 in `gltests` but to 0 in `gllib`.

If the answer is “yes”, you have two techniques available. The first
one is to invoke a similar Autoconf macro, named
`gl_MODULE_INDICATOR_FOR_TESTS`. It works similarly. However,
when your macro is used in `gltests`, `GNULIB_MY_MODULE`
will evaluate to 1 both in `gltests` and in `gllib`.

The second one is to define a shell variable in the `configure`
file that tells whether your module is present, through use of
`m4_divert_text`. The Autoconf macros of a dependency module will
initialize this shell variable, through
‘`m4_divert_text([DEFAULTS], [my_shell_var=no])`’. The
Autoconf macros of your module will override this value, through
‘`m4_divert_text([INIT_PREPARE], [my_shell_var=yes])`’. Then
you can use `my_shell_var` in the Autoconf macros of both
modules. You can find more details about this technique in the Gnulib
module `getopt-gnu`.

Reminder: These techniques are advanced. They have the potential to
cause lots of headaches if you apply them incorrectly.

---

Next: [License Texinfo sources](#License-Texinfo-sources), Previous: [Modules that modify the way other modules work](#Modules-that-modify-the-way-other-modules-work), Up: [Miscellaneous Notes](#Miscellaneous-Notes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 6.5 A C++ namespace for gnulib [¶](#A-C_002b_002b-namespace-for-gnulib-1)

The function definitions provided by Gnulib (`.c` code) are meant
to be compiled by a C compiler. The header files (`.h` files),
on the other hand, can be used in either C or C++.

By default, when used in a C++ compilation unit, the `.h` files
declare the same symbols and overrides as in C mode, except that functions
defined by Gnulib or by the system are declared as ‘`extern "C"`’.

It is also possible to indicate to Gnulib to provide many of its symbols
in a dedicated C++ namespace. If you define the macro
`GNULIB_NAMESPACE` to an identifier, many functions will be defined
in the namespace specified by the identifier instead of the global
namespace. For example, after you have defined

```
#define GNULIB_NAMESPACE gnulib
```

at the beginning of a compilation unit, Gnulib’s `<fcntl.h>` header
file will make available the `open` function as `gnulib::open`.
The symbol `open` will still refer to the system’s `open` function,
with its platform specific bugs and limitations.

The symbols provided in the Gnulib namespace are those for which the
corresponding header file contains a `_GL_CXXALIAS_RPL` or
`_GL_CXXALIAS_SYS` macro invocation.

The benefits of this namespace mode are:

* Gnulib defines fewer symbols as preprocessor macros. For example, on a
  platform where `open` has to be overridden, Gnulib normally does
  `#define open rpl_open`. If your package has a class with a member
  `open`, for example a class `foo` with a method `foo::open`,
  then if you define this member in a compilation unit that includes
  `<fcntl.h>` and use it in a compilation unit that does not include
  `<fcntl.h>`, or vice versa, you will get a link error. Worse: You
  will not notice this problem on the platform where the system’s `open`
  function works fine. This problem goes away in namespace mode.
* It provides a safety check whether the set of modules your package requests
  from Gnulib is sufficient. For example, if you use the function
  `gnulib::open` in your code, and you forgot to request the module
  ‘`open`’ from Gnulib, you will get a compilation error (regardless of
  the platform).

The drawback of this namespace mode is that the system provided symbols in
the global namespace are still present, even when they contain bugs that
Gnulib fixes. For example, if you call `open (...)` in your code,
it will invoke the possibly buggy system function, even if you have
requested the module ‘`open`’ from gnulib-tool.

You can turn on the namespace mode in some compilation units and keep it
turned off in others. This can be useful if your package consists of
an application layer that does not need to invoke POSIX functions and
an operating system interface layer that contains all the OS function
calls. In such a situation, you will want to turn on the namespace mode
for the application layer—to avoid many preprocessor macro
definitions—and turn it off for the OS interface layer—to avoid
the drawback of the namespace mode, mentioned above.

---

Next: [Building gnulib](#Building-gnulib), Previous: [A C++ namespace for gnulib](#A-C_002b_002b-namespace-for-gnulib), Up: [Miscellaneous Notes](#Miscellaneous-Notes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 6.6 License Texinfo sources [¶](#License-Texinfo-sources-1)

Gnulib provides copies of the GNU GPL, GNU LGPL, GNU Affero GPL, and
GNU FDL licenses in Texinfo form. (The master location is
<https://www.gnu.org/licenses/>). These Texinfo documents do not
have any node names and structures built into them; for your manual,
you should `@include` them in an appropriate `@node`.

The conventional name for the GPL node is ‘`Copying`’ and for the FDL
‘`GNU Free Documentation License`’. The LGPL doesn’t seem to have a
conventional node name.

Of course the license texts themselves should not be changed at all.

The recommended way to make use of these license files, consistently
with current practice, is as follows:

* The code license (GNU GPL, GNU LGPL, or GNU Affero GPL) is usually
  present as a file in the top-level directory. This is true not only for
  the release tarballs, but also in the VCS repository. The file is
  typically named ‘`COPYING`’ for the GNU GPL, or ‘`COPYING.LIB`’ or
  ‘`COPYING.LESSER`’ for the GNU LGPL. The presence of this file
  fulfills a legal obligation; see
  <https://www.gnu.org/licenses/gpl-faq.html#WhyMustIInclude>.

  To make use of the code license in your documentation, you may request
  one of the modules `gpl-3.0`, `gpl-2.0`, `lgpl-3.0`,
  `lgpl-2.1`, `agpl-3.0`, through a `gnulib-tool` invocation.
  Or you may copy the relevant Texinfo file directly into your VCS repository.
  Both approaches are equally good. The Texinfo file changes very rarely.
* The documentation license file (GNU FDL) is usually not present as a
  file in the top-level directory, because that would be ambiguous: When
  you use the GNU FDL, you need to specify the Invariant Sections, the
  Front-Cover Texts, and the Back-Cover Texts.

  To make use of this documentation license, copy the relevant Texinfo
  file (`doc/fdl-1.3.texi`) into your VCS repository. This makes
  sure that anyone who receives a copy of your VCS repository has also
  received a copy of the documentation license. In the documentation,
  also state what are the Invariant Sections, the Front-Cover Texts, and
  the Back-Cover Texts.

We recommend to place the licenses as appendices at the end of the
manual, right before any indices. For the FDL, we suggest the following
`@menu` entry:

```
* GNU Free Documentation License::  License for copying this manual
```

For any `@detailmenu` entries, we suggest the following:

```
Copying This Manual

* GNU Free Documentation License::     Copying and sharing this manual
```

And for actual inclusion of the FDL itself, we suggest the following:

```
@node GNU Free Documentation License
@appendix GNU Free Documentation License

@include fdl.texi
```

---

Next: [Gnulib Git Bundle](#Gnulib-Git-Bundle), Previous: [License Texinfo sources](#License-Texinfo-sources), Up: [Miscellaneous Notes](#Miscellaneous-Notes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 6.7 Building gnulib [¶](#Building-gnulib-1)

If you wish to help the gnulib development effort with build logs for
your favorite platform, you may perform these steps:

1. Prerequisites tools

   Install the proper development tools. To build and test all of
   Gnulib, you will need development tools for the programming languages
   C, C++, Python, and Perl, along with standard POSIX utilities such
   as `awk`, `make`, and `sh`. You will also need
   development tools that include Autoconf, Automake, Bison, Gettext,
   Git, GNU M4, Gperf, Libtool, and Texinfo. Some of these tools are
   needed only by some modules. More details can be found in Gnulib’s
   `DEPENDENCIES` file.
2. Obtain Gnulib

   See <https://www.gnu.org/software/gnulib/> for how to get the
   current Gnulib sources via Git.
3. Create gnulib directory

   On a machine with GNU development tools installed and with a gnulib
   git checkout, use

   ```
   gnulib-tool --create-testdir --with-tests --dir=...
   ```

   Alternatively, pick a small set of modules and run

   ```
   gnulib-tool --create-megatestdir --with-tests --dir=... autobuild MODULES
   ```

   (You can’t do this with all of Gnulib at once: `gnulib-tool` would run
   for a week and produce a directory that takes more than 100 GB, maybe even 1 TB,
   of disk space.)
4. Transfer gnulib directory

   Transfer this directory to a build machine (HP-UX, Cygwin, or
   whatever). Often it is easier to transfer one file, and this can be
   achieved by running, inside the directory the following commands:

   ```
   ./configure
   make dist
   ```

   And then transferring the `dummy-0.tar.gz` file.
5. Build modules

   On the build machine, run ./do-autobuild (or "nohup ./do-autobuild").
   It creates a directory `logs/` with a log file for each module.

---

Previous: [Building gnulib](#Building-gnulib), Up: [Miscellaneous Notes](#Miscellaneous-Notes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 6.8 Gnulib Git Bundle [¶](#Gnulib-Git-Bundle-1)

To provide a serialized archival copy of the Gnulib Git repository we
publish Git Bundles (<https://git-scm.com/docs/git-bundle>) of
Gnulib at <https://ftp.gnu.org/gnu/gnulib/>. These may be useful
if Savannah happens to be offline or if you want to have a GnuPG
signed confirmation of the Gnulib content.

The files are named like `gnulib-YYYYMMDD.bundle`, for example
`gnulib-20260109.bundle`, where `YYYYMMDD` corresponds to
the Git commit date (in UTC0) of the last commit on the `master`
branch in the bundle.

After downloading the Git bundle you may use it to create a local
gnulib clone using normal Git commands:

```
wget -nv https://ftp.gnu.org/gnu/gnulib/gnulib-20260109.bundle
git clone gnulib-20260109.bundle gnulib
cd gnulib
```

Below are SHA-256 and SHA3-256 checksums of known releases:

```
9dae009ef9dd7cff17b74c0cda5d7a423e2ed98b4f5b7aa29a970565b0591c06  gnulib-20250303.bundle
f01e423a7ef6b48e947fabd24bb11744204f4549342416e15dc64f427caa32e2  gnulib-20250729.bundle
ae7f76abdf11d70f87170363b8f5807ec377a3ebf231499ab8278270c7a19f1a  gnulib-20260109.bundle

XMv72pyPrDiGrukOrQ9UwgLh+bbekQhQWuyaEmEf3Co=  gnulib-20250303.bundle
c3X/89WHMIRVqGpOHHQPZfw2bcxnZEIkgOam7WwRUyw=  gnulib-20250729.bundle
6kYv60oHv7kXpkJM2vUlADWNmh62nus1xA80bJJiJEs=  gnulib-20260109.bundle
```

Verify the SHA256 checksum
with either `sha256sum`, `sha256`, or `shasum -a 256`.

Verify the base64 SHA3-256 checksum
with `cksum -a sha3 -l 256 --base64` from coreutils 9.8 or newer,
or with `cksum -a sha3 --check` from coreutils 9.9 or newer.

Next to the Git Bundle is a GnuPG signature on the file, named
`gnulib-YYYYMMDD.bundle.sig`, which can be verified using GnuPG
as usual:

```
gpg --verify gnulib-20260109.bundle.sig
```

Or using the simpler `gpgv` tool like this:

```
gpgv gnulib-20260109.bundle.sig gnulib-20260109.bundle
```

The following GnuPG keys have signed releases:

```
sec>  ed25519 2019-03-20 [SC] https://josefsson.org/key-20190320.txt
      B1D2BD1375BECB784CF4F8C4D73CF638C53C06BE
uid           [ultimate] Simon Josefsson <simon@josefsson.org>
```

We desire that the Gnulib Git bundles will be forever bit-by-bit
reproducible for others from the official git repository. Currently
gnulib maintainers invoke the following commands to prepare and upload
the latest Gnulib git bundle. We appreciate ideas on how to improve
these set of commands (or the upstream Git tool) to make further
supply-chain security related improvements.

```
cd $(mktemp -d)
REV=2961ed7a687c11aebdf5951d5267dec85d9e59cb   # master branch commit to package
S1REV=e93789db7e86c51d6cb9683ea508e676a55cdefa # stable-202601 branch commit
S2REV=b6d56475ad6d8bc59e85cdf7fa8dd60fbddc6ffc # stable-202507 branch commit
git clone https://git.savannah.gnu.org/git/gnulib.git
cd gnulib
git fsck # attempt to validate input
# Manually inspect that the new tree matches a trusted previous copy
git checkout -B master $REV # put $REV at master
