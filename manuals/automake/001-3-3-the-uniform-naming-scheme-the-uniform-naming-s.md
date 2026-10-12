### 3.3 The Uniform Naming Scheme [¶](#The-Uniform-Naming-Scheme)

Automake variables generally follow a *uniform naming scheme* that
makes it easy to decide how programs (and other derived objects) are
built, and how they are installed. This scheme also supports
`configure` time determination of what should be built.

At `make` time, certain variables are used to determine which
objects are to be built. The variable names are made of several pieces
that are concatenated together.

The piece that tells `automake` what is being built is commonly called
the *primary*. For instance, the primary `PROGRAMS` holds a
list of programs that are to be compiled and linked.

A different set of names is used to decide where the built objects
should be installed. These names are prefixes to the primary, and they
indicate which standard directory should be used as the installation
directory. The standard directory names are given in the GNU standards
(see [Directory Variables](https://www.gnu.org/prep/standards/standards.html#Directory-Variables) in The GNU Coding Standards).
Automake extends this list with `pkgdatadir`, `pkgincludedir`,
`pkglibdir`, and `pkglibexecdir`; these are the same as the
non-‘`pkg`’ versions, but with ‘`$(PACKAGE)`’ appended. For instance,
`pkglibdir` is defined as ‘`$(libdir)/$(PACKAGE)`’.

For each primary, there is one additional variable named by prepending
‘`EXTRA_`’ to the primary name. This variable is used to list
objects that may or may not be built, depending on what
`configure` decides. This variable is required because Automake
must statically know the entire list of objects that may be built in
order to generate a `Makefile.in` that will work in all cases.

For instance, `cpio` decides at configure time which programs
should be built. Some of the programs are installed in `bindir`,
and some are installed in `sbindir`:

```
EXTRA_PROGRAMS = mt rmt
bin_PROGRAMS = cpio pax
sbin_PROGRAMS = $(MORE_PROGRAMS)
```

Defining a primary without a prefix as a variable, e.g.,
‘`PROGRAMS`’, is an error.

Note that the common ‘`dir`’ suffix is left off when constructing the
variable names; thus one writes ‘`bin_PROGRAMS`’ and not
‘`bindir_PROGRAMS`’.

Not every sort of object can be installed in every directory.
Automake will flag those attempts it finds in error (but see below how
to override the check if you need to). Automake will also diagnose
obvious misspellings in directory names.

Sometimes the standard directories—even as augmented by
Automake—are not enough. In particular it is sometimes useful, for
clarity, to install objects in a subdirectory of some predefined
directory. To this end, Automake allows you to extend the list of
possible installation directories. A given prefix (e.g., ‘`zar`’)
is valid if a variable of the same name with ‘`dir`’ appended is
defined (e.g., ‘`zardir`’).

For instance, the following snippet will install `file.xml` into
‘`$(datadir)/xml`’.

```
xmldir = $(datadir)/xml
xml_DATA = file.xml
```

This feature can also be used to override the sanity checks Automake
performs to diagnose suspicious directory/primary couples (in the
unlikely case that you need to omit these checks). For example,
Automake would error out on this input:

```
# Forbidden directory combinations, automake will error out on this.
pkglib_PROGRAMS = foo
doc_LIBRARIES = libquux.a
```

but it will succeed with this:

```
# Work around forbidden directory combinations.  Do not use this
# without a very good reason!
my_execbindir = $(pkglibdir)
my_doclibdir = $(docdir)
my_execbin_PROGRAMS = foo
my_doclib_LIBRARIES = libquux.a
```

The ‘`exec`’ substring of the ‘`my_execbindir`’ variable lets
the files be installed at the right time (see [The Two Parts of Install](#The-Two-Parts-of-Install)).

The special prefix ‘`noinst_`’ indicates that the objects in question
should be built but not installed at all. This is usually used for
objects required to build the rest of your package, for instance static
libraries (see [Building a library](#A-Library)), or helper scripts.

The special prefix ‘`check_`’ indicates that the objects in question
should not be built until the ‘`make check`’ command is run. Those
objects are not installed either.

The current primary names are ‘`PROGRAMS`’, ‘`LIBRARIES`’,
‘`LTLIBRARIES`’, ‘`LISP`’, ‘`PYTHON`’, ‘`JAVA`’,
‘`SCRIPTS`’, ‘`DATA`’, ‘`HEADERS`’, ‘`MANS`’, and
‘`TEXINFOS`’.

Some primaries also allow additional prefixes that control other
aspects of `automake`’s behavior. The currently defined prefixes
are ‘`dist_`’, ‘`nodist_`’, ‘`nobase_`’, and ‘`notrans_`’.
These prefixes are explained later (see [Program and Library Variables](#Program-and-Library-Variables))
(see [Man Pages](#Man-Pages)).

---

Next: [How derived variables are named](#Canonicalization), Previous: [The Uniform Naming Scheme](#Uniform), Up: [General ideas](#Generalities)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 3.4 Staying below the command line length limit [¶](#Staying-below-the-command-line-length-limit)

Traditionally, most unix-like systems have a length limitation for the
command line arguments and environment contents when creating new
processes (see for example
<https://www.in-ulm.de/~mascheck/various/argmax/> for an
overview on this issue),
which of course also applies to commands spawned by `make`.
POSIX requires this limit to be at least 4096 bytes, and most modern
systems have quite high limits (or are unlimited).

In order to create portable Makefiles that do not trip over these
limits, it is necessary to keep the length of file lists bounded.
Unfortunately, it is not possible to do so fully transparently within
Automake, so your help may be needed. Typically, you can split long
file lists manually and use different installation directory names for
each list. For example,

```
data_DATA = file1 ... fileN fileN+1 ... file2N
```

may also be written as

```
data_DATA = file1 ... fileN
data2dir = $(datadir)
data2_DATA = fileN+1 ... file2N
```

and will cause Automake to treat the two lists separately during
`make install`. See [The Two Parts of Install](#The-Two-Parts-of-Install) for choosing
directory names that will keep the ordering of the two parts of
installation Note that `make dist` may still only work on a host
with a higher length limit in this example.

Automake itself employs a couple of strategies to avoid long command
lines. For example, when ‘`${srcdir}/`’ is prepended to file
names, as can happen with above `$(data_DATA)` lists, it limits
the amount of arguments passed to external commands.

Unfortunately, some systems’ `make` commands may prepend
`VPATH` prefixes like ‘`${srcdir}/`’ to file names from the
source tree automatically (see [Automatic
Rule Rewriting](https://www.gnu.org/software/autoconf/manual/autoconf.html#Automatic-Rule-Rewriting) in The Autoconf Manual). In this case, the user
may have to switch to use GNU Make, or refrain from using VPATH builds,
in order to stay below the length limit.

For libraries and programs built from many sources, convenience archives
may be used as intermediates in order to limit the object list length
(see [Libtool Convenience Libraries](#Libtool-Convenience-Libraries)).

---

Next: [Variables reserved for the user](#User-Variables), Previous: [Staying below the command line length limit](#Length-Limitations), Up: [General ideas](#Generalities)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 3.5 How derived variables are named [¶](#How-derived-variables-are-named)

Sometimes a Makefile variable name is derived from some text the
maintainer supplies. For instance, a program name listed in
‘`_PROGRAMS`’ is rewritten into the name of a ‘`_SOURCES`’
variable. In cases like this, Automake canonicalizes the text, so that
program names and the like do not have to follow Makefile variable naming
rules. All characters in the name except for letters, numbers, the
strudel (@), and the underscore are turned into underscores when making
variable references.

For example, if your program is named `sniff-glue`, the derived
variable name would be ‘`sniff_glue_SOURCES`’, not
‘`sniff-glue_SOURCES`’. Similarly the sources for a library named
`libmumble++.a` should be listed in the
‘`libmumble___a_SOURCES`’ variable.

The strudel is an addition, to make the use of Autoconf substitutions in
variable names less obfuscating.

---

Next: [Programs automake might require](#Auxiliary-Programs), Previous: [How derived variables are named](#Canonicalization), Up: [General ideas](#Generalities)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 3.6 Variables reserved for the user [¶](#Variables-reserved-for-the-user)

Some `Makefile` variables are reserved by the GNU Coding Standards
for the use of the “user”—the person building the package. For
instance, `CFLAGS` is one such variable.

Sometimes package developers are tempted to set user variables such as
`CFLAGS` because it appears to make their job easier. However,
the package itself should never set a user variable, particularly not
to include switches that are required for proper compilation of the
package. Since these variables are documented as being for the
package builder, that person rightfully expects to be able to override
any of these variables at build time.

To get around this problem, Automake introduces an automake-specific
shadow variable for each user flag variable. (Shadow variables are
not introduced for variables like `CC`, where they would make no
sense.) The shadow variable is named by prepending ‘`AM_`’ to the
user variable’s name. For instance, the shadow variable for
`YFLAGS` is `AM_YFLAGS`. The package maintainer—that is,
the author(s) of the `Makefile.am` and `configure.ac`
files—may adjust these shadow variables however necessary.

See [Flag Variables Ordering](#Flag-Variables-Ordering), for more discussion about these
variables and how they interact with per-target variables.

---

Previous: [Variables reserved for the user](#User-Variables), Up: [General ideas](#Generalities)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 3.7 Programs automake might require [¶](#Programs-automake-might-require)

Automake sometimes requires helper programs so that the generated
`Makefile` can do its work properly. There are a fairly large
number of them, and we list them here.

Although all of these files are distributed and installed with
Automake, a couple of them are maintained separately. The Automake
copies are updated before each release, but we mention the original
source in case you need more recent versions.

`ar-lib` [¶](#index-ar_002dlib)
:   This is a wrapper primarily for the Microsoft lib archiver, to make
    it more POSIX-like.

`compile` [¶](#index-compile)
:   This is a wrapper for compilers that do not accept options `-c`
    and `-o` at the same time. It is only used when absolutely
    required. Such compilers are rare, with the Microsoft C/C++ Compiler
    as the most notable exception. This wrapper also makes the following
    common options available for that compiler, while performing file name
    translation where needed: `-I`, `-L`, `-l`,
    `-Wl,` and `-Xlinker`.

`config.guess` [¶](#index-config_002eguess)

`config.sub`
:   These two programs compute the canonical triplets for the given build,
    host, or target architecture. These programs are updated regularly to
    support new architectures and fix probes broken by changes in new
    kernel versions. Each new release of Automake comes with up-to-date
    copies of these programs. If your copy of Automake is getting old,
    you are encouraged to fetch the latest versions of these files from
    <https://savannah.gnu.org/git/?group=config> before making a
    release.

`depcomp` [¶](#index-depcomp)
:   This program understands how to run a compiler so that it will
    generate not only the desired output but also dependency information
    that is then used by the automatic dependency tracking feature
    (see [Automatic dependency tracking](#Dependencies)).

`install-sh` [¶](#index-install_002dsh)
:   This is a replacement for the `install` program that works on
    platforms where `install` is unavailable or unusable.

`mdate-sh` [¶](#index-mdate_002dsh)
:   This script is used to generate a `version.texi` file. It examines
    a file and prints some date information about it.

`missing` [¶](#index-missing-program)
:   This wraps a number of programs that are typically only required by
    maintainers. If the program in question doesn’t exist, or seems too old,
    `missing` will print an informative warning before failing out,
    to provide the user with more context and information.

`mkinstalldirs` [¶](#index-mkinstalldirs)
:   This script used to be a wrapper around ‘`mkdir -p`’, which is not
    portable. Now we prefer to use ‘`install-sh -d`’ when `configure`
    finds that ‘`mkdir -p`’ does not work; this makes for one less script to
    distribute.

    For backward compatibility `mkinstalldirs` is still used and
    distributed when `automake` finds it in a package. But it is no
    longer installed automatically, and it should be safe to remove it.

`py-compile` [¶](#index-py_002dcompile)
:   This is used to byte-compile Python scripts.

`test-driver` [¶](#index-test_002ddriver)
:   This implements the default test driver offered by the parallel
    testsuite harness.

`texinfo.tex` [¶](#index-texinfo_002etex)
:   When Texinfo sources are in the package, this file is required for
    ‘`make dvi`’, ‘`make ps`’ and ‘`make pdf`’. The latest
    version can be downloaded from
    <https://www.gnu.org/software/texinfo/>. A working TeX
    distribution, or at least a `tex` program, is also required.
    Furthermore, ‘`make dist`’ invokes ‘`make dvi`’, so these become
    requirements for making a distribution with Texinfo sources.

`ylwrap` [¶](#index-ylwrap)
:   This program wraps `lex` and `yacc` to rename their
    output files. It also ensures that, for instance, multiple
    `yacc` instances can be invoked in a single directory in
    parallel.

---

Next: [Creating a `Makefile.in`: Invoking `automake`](#automake-Invocation), Previous: [General ideas](#Generalities), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 4 Some example packages [¶](#Some-example-packages)

This section contains two small examples.

The first example (see [A simple example, start to finish](#Complete)) assumes you have an existing
project already using Autoconf, with handcrafted `Makefile`s, and
that you want to convert it to using Automake. If you are discovering
both tools, it is probably better that you look at the Hello World
example presented earlier (see [A Small Hello World](#Hello-World)).

The second example (see [Building true and false](#true)) shows how two programs can be built
from the same file, using different compilation parameters. It
contains some technical digressions that are probably best skipped on
first read.

* [A simple example, start to finish](#Complete)
* [Building true and false](#true)

---

Next: [Building true and false](#true), Up: [Some example packages](#Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 4.1 A simple example, start to finish [¶](#A-simple-example_002c-start-to-finish)

Let’s suppose you just finished writing `zardoz`, a program to make
your head float from vortex to vortex. You’ve been using Autoconf to
provide a portability framework, but your `Makefile.in`s have been
ad-hoc. You want to make them bulletproof, so you turn to Automake.

The first step is to update your `configure.ac` to include the
commands that `automake` needs. The way to do this is to add an
`AM_INIT_AUTOMAKE` call just after `AC_INIT`:

```
AC_INIT([zardoz], [1.0])
AM_INIT_AUTOMAKE
...
```

Since your program doesn’t have any complicating factors (e.g., it
doesn’t use `gettext`, it doesn’t want to build a shared library),
you’re done with this part. That was easy!

Now you must regenerate `configure`. But to do that, you’ll need
to tell `autoconf` how to find the new macro you’ve used. The
easiest way to do this is to use the `aclocal` program to
generate your `aclocal.m4` for you. But wait… maybe you
already have an `aclocal.m4`, because you had to write some hairy
macros for your program. The `aclocal` program lets you put
your own macros into `acinclude.m4`, so simply rename and then
run:

```
mv aclocal.m4 acinclude.m4
aclocal
autoconf
```

Now it is time to write your `Makefile.am` for `zardoz`.
Since `zardoz` is a user program, you want to install it where the
rest of the user programs go: `bindir`. Additionally,
`zardoz` has some Texinfo documentation. Your `configure.ac`
script uses `AC_REPLACE_FUNCS`, so you need to link against
‘`$(LIBOBJS)`’. So here’s what you’d write:

```
bin_PROGRAMS = zardoz
zardoz_SOURCES = main.c head.c float.c vortex9.c gun.c
zardoz_LDADD = $(LIBOBJS)

info_TEXINFOS = zardoz.texi
```

Now you can run ‘`automake --add-missing`’ to generate your
`Makefile.in` and grab any auxiliary files you might need, and
you’re done!

---

Previous: [A simple example, start to finish](#Complete), Up: [Some example packages](#Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 4.2 Building true and false [¶](#Building-true-and-false)

Here is another, trickier example. It shows how to generate two
programs (`true` and `false`) from the same source file
(`true.c`). The difficult part is that each compilation of
`true.c` requires different `cpp` flags.

```
bin_PROGRAMS = true false
false_SOURCES =
false_LDADD = false.o

true.o: true.c
        $(COMPILE) -DEXIT_CODE=0 -c true.c

false.o: true.c
        $(COMPILE) -DEXIT_CODE=1 -o false.o -c true.c
```

Note that there is no `true_SOURCES` definition. Automake will
implicitly assume that there is a source file named `true.c`
(see [Default `_SOURCES`](#Default-_005fSOURCES)), and
define rules to compile `true.o` and link `true`. The
‘`true.o: true.c`’ rule supplied by the above `Makefile.am`,
will override the Automake generated rule to build `true.o`.

`false_SOURCES` is defined to be empty—that way no implicit value
is substituted. Because we have not listed the source of
`false`, we have to tell Automake how to link the program. This is
the purpose of the `false_LDADD` line. A `false_DEPENDENCIES`
variable, holding the dependencies of the `false` target will be
automatically generated by Automake from the content of
`false_LDADD`.

The above rules won’t work if your compiler doesn’t accept both
`-c` and `-o`. The simplest fix for this is to introduce a
bogus dependency (to avoid problems with a parallel `make`):

```
true.o: true.c false.o
        $(COMPILE) -DEXIT_CODE=0 -c true.c

false.o: true.c
        $(COMPILE) -DEXIT_CODE=1 -c true.c && mv true.o false.o
```

As it turns out, there is also a much easier way to do this same task.
Some of the above technique is useful enough that we’ve kept the
example in the manual. However if you were to build `true` and
`false` in real life, you would probably use per-program
compilation flags, like so:

```
bin_PROGRAMS = false true

false_SOURCES = true.c
false_CPPFLAGS = -DEXIT_CODE=1

true_SOURCES = true.c
true_CPPFLAGS = -DEXIT_CODE=0
```

In this case Automake will cause `true.c` to be compiled twice,
with different flags. In this instance, the names of the object files
would be chosen by automake; they would be `false-true.o` and
`true-true.o`. (The name of the object files rarely matters.)

---

Next: [Scanning `configure.ac`, using `aclocal`](#configure), Previous: [Some example packages](#Examples), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 5 Creating a `Makefile.in`: Invoking `automake` [¶](#Creating-a-Makefile_002ein_003a-Invoking-automake)

To create all the `Makefile.in`s for a package, run the
`automake` program in the top level directory, with no
arguments. `automake` will automatically find each
appropriate `Makefile.am` (by scanning `configure.ac`;
see [Scanning `configure.ac`, using `aclocal`](#configure)) and generate the corresponding `Makefile.in`.
Note that `automake` has a rather simplistic view of what
constitutes a package; it assumes that a package has only one
`configure.ac`, at the top. If your package has multiple
`configure.ac`s, then you must run `automake` in each
directory holding a `configure.ac`. (Alternatively, you may rely
on Autoconf’s `autoreconf`, which is able to recurse your
package tree and run `automake` where appropriate.)

You can optionally give `automake` an argument; `.am` is
appended to the argument and the result is used as the name of the
input file. This feature is generally only used to automatically
rebuild an out-of-date `Makefile.in`. Note that
`automake` must always be run from the topmost directory of a
project, even if being used to regenerate the `Makefile.in` in
some subdirectory. This is necessary because `automake` must
scan `configure.ac`, and because `automake` uses the
knowledge that a `Makefile.in` is in a subdirectory to change its
behavior in some cases.

Automake will run `autoconf` to scan `configure.ac` and
its dependencies (i.e., `aclocal.m4` and any included file),
therefore `autoconf` must be in your `PATH`. If there is
an `AUTOCONF` variable in your environment it will be used
instead of `autoconf`; this allows you to select a particular
version of Autoconf. By the way, don’t misunderstand this paragraph:
`automake` runs `autoconf` to *scan* your
`configure.ac`; this won’t build `configure` and you still
have to run `autoconf` yourself for this purpose.

`automake` accepts the following options:

`-a` [¶](#index-_002da)

`--add-missing`
:   Automake requires certain common files to exist in certain situations;
    for instance, `config.guess` is required if `configure.ac` invokes
    `AC_CANONICAL_HOST`. Automake is distributed with several of these
    files (see [Programs automake might require](#Auxiliary-Programs)); this option will cause the missing
    ones to be automatically added to the package, whenever possible. In
    general if Automake tells you a file is missing, try using this option.
    By default Automake tries to make a symbolic link pointing to its own
    copy of the missing file; this can be changed with `--copy`.

    Many of the potentially-missing files are common scripts whose
    location may be specified via the `AC_CONFIG_AUX_DIR` macro.
    Therefore, `AC_CONFIG_AUX_DIR`’s setting affects whether a
    file is considered missing, and where the missing file is added
    (see [Other things Automake recognizes](#Optional)).

    In some strictness modes, additional files are installed, see [Gnits](#Gnits)
    for more information.

`--libdir=dir` [¶](#index-_002d_002dlibdir)
:   Look for Automake data files in directory dir instead of in the
    installation directory. This is typically used for debugging.

    The environment variable `AUTOMAKE_LIBDIR` provides another way to
    set the directory containing Automake data files. The
    `--libdir` option takes precedence over it.

`--print-libdir` [¶](#index-_002d_002dprint_002dlibdir)
:   Print the path of the installation directory containing Automake-provided
    scripts and data files (e.g., `texinfo.texi` and
    `install-sh`).

`-c` [¶](#index-_002d_002dcopy)

`--copy`
:   When used with `--add-missing`, causes installed files to be
    copied. The default is to make a symbolic link.

`-f` [¶](#index-_002d_002dforce_002dmissing)

`--force-missing`
:   When used with `--add-missing`, causes standard files to be reinstalled
    even if they already exist in the source tree. This involves removing
    the file from the source tree before creating the new symlink (or, with
    `--copy`, copying the new file).

`--foreign` [¶](#index-_002d_002dforeign-1)
:   Set the global strictness to `foreign`. For more information, see
    [Strictness](#Strictness).

`--gnits` [¶](#index-_002d_002dgnits-1)
:   Set the global strictness to `gnits`. For more information, see
    [Strictness](#Strictness).

`--gnu` [¶](#index-_002d_002dgnu-1)
:   Set the global strictness to `gnu`. For more information, see
    [Strictness](#Strictness). This is the default strictness.

`--help` [¶](#index-_002d_002dhelp)
:   Print a summary of the command line options and exit.

`-i` [¶](#index-_002di)

`--ignore-deps`
:   This disables the dependency tracking feature in generated
    `Makefile`s; see [Automatic dependency tracking](#Dependencies).

`--include-deps` [¶](#index-_002d_002dinclude_002ddeps)
:   This enables the dependency tracking feature. This feature is enabled
    by default. This option is provided for historical reasons only and
    probably should not be used.

`--no-force` [¶](#index-_002d_002dno_002dforce)
:   Ordinarily `automake` creates all `Makefile.in`s mentioned in
    `configure.ac`. This option causes it to only update those
    `Makefile.in`s that are out of date with respect to one of their
    dependents.

`-o dir` [¶](#index-_002do)

`--output-dir=dir`
:   Put the generated `Makefile.in` in the directory dir.
    Ordinarily each `Makefile.in` is created in the directory of the
    corresponding `Makefile.am`. This option is deprecated and will be
    removed in a future release.

`-v` [¶](#index-_002dv)

`--verbose`
:   Cause Automake to print information about which files are being read or
    created.

`--version` [¶](#index-_002d_002dversion)
:   Print the version number of Automake and exit.

`-W category[,category...]` [¶](#index-_002dW)

`--warnings=category[,category...]`
:   Output warnings about a category of potential problems with the
    package. category can be any of:

    `cross`
    :   Constructs compromising the ability to cross-compile the package.

    `gnu`
    :   Minor deviations from the GNU Coding Standards
        (see [The GNU Coding Standards](https://www.gnu.org/prep/standards/standards.html#Top)).

    `obsolete`
    :   Obsolete features or constructions.

    `override`
    :   Redefinitions of Automake rules or variables.

    `portability`
    :   Portability issues (e.g., use of `make` features that are
        known to be not portable).

    `portability-recursive`
    :   Recursive, or nested, Make variable expansions (`$(foo$(x))`).
        These are not universally supported, but are more portable than the
        other non-portable constructs diagnosed by `-Wportability`.
        These warnings are turned on by `-Wportability` but can then be
        turned off specifically by `-Wno-portability-recursive`.

    `extra-portability`
    :   Extra portability issues, related to rarely-used tools such as
        the Microsoft `lib` archiver.

    `syntax`
    :   Questionable syntax, unused variables, typos, etc.

    `unsupported`
    :   Unsupported or incomplete features.

    `all`
    :   Turn on all the above categories of warnings.

    `none`
    :   Turn off all the above categories of warnings.

    `error`
    :   Treat warnings as errors.

    A category can be turned off by prefixing its name with ‘`no-`’. For
    instance, `-Wno-syntax` will hide the warnings about unused
    variables.

    Warnings in the ‘`gnu`’, ‘`obsolete`’, ‘`portability`’,
    ‘`syntax`’, and ‘`unsupported`’ categories are turned on by
    default. The ‘`gnu`’ and ‘`portability`’ categories are turned
    off in `--foreign` strictness.

    Turning off ‘`portability`’ will also turn off ‘`extra-portability`’,
    and similarly turning on ‘`extra-portability`’ will also turn on
    ‘`portability`’. However, turning on ‘`portability`’ or turning
    off ‘`extra-portability`’ will not affect the other category.

    Unknown warning categories supplied as an argument to `-W` will
    themselves produce a warning, in the ‘`unsupported`’ category. This
    warning is never treated as an error.

    The environment variable `WARNINGS` can contain a comma separated
    list of categories to enable. `-W` settings on the command line
    take precedence; for instance, `-Wnone` also turns off any
    warning categories enabled by `WARNINGS`.

    Unknown warning categories named in `WARNINGS` are silently ignored.

If the environment variable `AUTOMAKE_JOBS` contains a positive
number, it is taken as the maximum number of Perl threads to use in
`automake` for generating multiple `Makefile.in` files
concurrently. This is an experimental feature.

---

Next: [Directories](#Directories), Previous: [Creating a `Makefile.in`: Invoking `automake`](#automake-Invocation), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 6 Scanning `configure.ac`, using `aclocal` [¶](#Scanning-configure_002eac_002c-using-aclocal)

Automake scans the package’s `configure.ac` to determine certain
information about the package. Some `autoconf` macros are required
and some variables must be defined in `configure.ac`. Automake
will also use information from `configure.ac` to further tailor its
output.

Automake also supplies some Autoconf macros to make the maintenance
easier. These macros can automatically be put into your
`aclocal.m4` using the `aclocal` program.

* [Configuration requirements](#Requirements)
* [Other things Automake recognizes](#Optional)
* [Auto-generating aclocal.m4: Invoking `aclocal`](#aclocal-Invocation)
* [Autoconf macros supplied with Automake](#Macros)

---

Next: [Other things Automake recognizes](#Optional), Up: [Scanning `configure.ac`, using `aclocal`](#configure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 6.1 Configuration requirements [¶](#Configuration-requirements)

The one real requirement of Automake is that your `configure.ac`
call `AM_INIT_AUTOMAKE`. This macro does several things that are
required for proper Automake operation (see [Autoconf macros supplied with Automake](#Macros)).

Here are the other macros that Automake requires but which are not run
by `AM_INIT_AUTOMAKE`:

`AC_CONFIG_FILES` [¶](#index-AC_005fCONFIG_005fFILES)

`AC_OUTPUT`
:   These two macros are usually invoked as follows near the end of
    `configure.ac`.

    ```
    ...
    AC_CONFIG_FILES([
      Makefile
      doc/Makefile
      src/Makefile
      src/lib/Makefile
      ...
    ])
    AC_OUTPUT
    ```

    Automake uses these to determine which files to create (see [Creating Output Files](https://www.gnu.org/software/autoconf/manual/autoconf.html#Output) in The Autoconf Manual). A listed file
    is considered to be an Automake generated `Makefile` if there
    exists a file with the same name and the `.am` extension appended.
    Typically, ‘`AC_CONFIG_FILES([foo/Makefile])`’ will cause Automake to
    generate `foo/Makefile.in` if `foo/Makefile.am` exists.

    When using `AC_CONFIG_FILES` with multiple input files, as in

    ```
    AC_CONFIG_FILES([Makefile:top.in:Makefile.in:bot.in])
    ```

    `automake` will generate the first `.in` input file for
    which a `.am` file exists. If no such file exists the output
    file is not considered to be generated by Automake.

    Files created by `AC_CONFIG_FILES`, be they Automake
    `Makefile`s or not, are all removed by ‘`make distclean`’.
    Their inputs are automatically distributed, unless they
    are the output of prior `AC_CONFIG_FILES` commands.
    Finally, rebuild rules are generated in the Automake `Makefile`
    existing in the subdirectory of the output file, if there is one, or
    in the top-level `Makefile` otherwise.

    The above machinery (cleaning, distributing, and rebuilding) works
    fine if the `AC_CONFIG_FILES` specifications contain only
    literals. If part of the specification uses shell variables,
    `automake` will not be able to fulfill this setup, and you will
    have to complete the missing bits by hand. For instance, on

    ```
    file=input
    ...
    AC_CONFIG_FILES([output:$file],, [file=$file])
    ```

    `automake` will output rules to clean `output`, and
    rebuild it. However the rebuild rule will not depend on `input`,
    and this file will not be distributed either. (You must add
    ‘`EXTRA_DIST = input`’ to your `Makefile.am` if `input` is a
    source file.)

    Similarly

    ```
    file=output
    file2=out:in
    ...
    AC_CONFIG_FILES([$file:input],, [file=$file])
    AC_CONFIG_FILES([$file2],, [file2=$file2])
    ```

    will only cause `input` to be distributed. No file will be
    cleaned automatically (add ‘`DISTCLEANFILES = output out`’
    yourself), and no rebuild rule will be output.

    Obviously `automake` cannot guess what value ‘`$file`’ is
    going to hold later when `configure` is run, and it cannot use
    the shell variable ‘`$file`’ in a `Makefile`. However, if you
    make reference to ‘`$file`’ as ‘`${file}`’ (i.e., in a way
    that is compatible with `make`’s syntax) and furthermore use
    `AC_SUBST` to ensure that ‘`${file}`’ is meaningful in a
    `Makefile`, then `automake` will be able to use
    ‘`${file}`’ to generate all of these rules. For instance, here is
    how the Automake package itself generates versioned scripts for its
    test suite:

    ```
    AC_SUBST([APIVERSION], ...)
    ...
    AC_CONFIG_FILES(
      [tests/aclocal-${APIVERSION}:tests/aclocal.in],
      [chmod +x tests/aclocal-${APIVERSION}],
      [APIVERSION=$APIVERSION])
    AC_CONFIG_FILES(
      [tests/automake-${APIVERSION}:tests/automake.in],
      [chmod +x tests/automake-${APIVERSION}])
    ```

    Here cleaning, distributing, and rebuilding are done automatically,
    because ‘`${APIVERSION}`’ is known at `make`-time.

    Note that you should not use shell variables to declare
    `Makefile` files for which `automake` must create
    `Makefile.in`. Even `AC_SUBST` does not help here, because
    `automake` needs to know the file name when it runs in order
    to check whether `Makefile.am` exists. (In the very hairy case
    that your setup requires such use of variables, you will have to tell
    Automake which `Makefile.in`s to generate on the command-line.)

    It is possible to let `automake` emit conditional rules for
    `AC_CONFIG_FILES` with the help of `AM_COND_IF`
    (see [Other things Automake recognizes](#Optional)).

    To summarize:

    * Use literals for `Makefile`s, and for other files whenever possible.
    * Use ‘`$file`’ (or ‘`${file}`’ without ‘`AC_SUBST([file])`’)
      for files that `automake` should ignore.
    * Use ‘`${file}`’ and ‘`AC_SUBST([file])`’ for files
      that `automake` should not ignore.

---

Next: [Auto-generating aclocal.m4: Invoking `aclocal`](#aclocal-Invocation), Previous: [Configuration requirements](#Requirements), Up: [Scanning `configure.ac`, using `aclocal`](#configure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 6.2 Other things Automake recognizes [¶](#Other-things-Automake-recognizes)

Every time Automake is run it calls Autoconf to trace
`configure.ac`. This way it can recognize the use of certain
macros and tailor the generated `Makefile.in` appropriately.
Currently recognized macros and their effects are:

`AC_CANONICAL_BUILD` [¶](#index-AC_005fCANONICAL_005fBUILD)

`AC_CANONICAL_HOST` [¶](#index-AC_005fCANONICAL_005fHOST)

`AC_CANONICAL_TARGET` [¶](#index-AC_005fCANONICAL_005fTARGET)
:   Automake will ensure that `config.guess` and `config.sub`
    exist. Also, the `Makefile` variables `build_triplet`,
    `host_triplet` and `target_triplet` are introduced. See
    [Getting the Canonical System Type](https://www.gnu.org/software/autoconf/manual/autoconf.html#Canonicalizing) in The Autoconf Manual.

`AC_CONFIG_AUX_DIR` [¶](#index-AC_005fCONFIG_005fAUX_005fDIR-1)
:   Automake will look for various helper scripts, such as
    `install-sh`, in the directory named in this macro invocation.
    (The full list of scripts is:
    `ar-lib`,
    `config.guess`,
    `config.sub`,
    `depcomp`,
    `compile`,
    `install-sh`,
    `ltmain.sh`,
    `mdate-sh`,
    `missing`,
    `mkinstalldirs`,
    `py-compile`,
    `test-driver`,
    `texinfo.tex`,
    `ylwrap`.)
    Not all scripts are always searched for; some scripts
    will only be sought if the generated `Makefile.in` requires them.

    If `AC_CONFIG_AUX_DIR` is used, it must be given before the call
    to `AM_INIT_AUTOMAKE`; Automake will warn about this if it is not
    so. All other `AC_CONFIG_...` macros are conventionally called
    after `AM_INIT_AUTOMAKE`, though they may or may not work in
    other locations, with or without warnings.

    If `AC_CONFIG_AUX_DIR` is not given, the scripts are looked for in
    their standard locations. For `mdate-sh`,
    `texinfo.tex`, and `ylwrap`, the standard location is the
    source directory corresponding to the current `Makefile.am`. For
    the rest, the standard location is the first one of `.`, `..`,
    or `../..` (relative to the top source directory) that provides any
    one of the helper scripts. See [Finding ‘configure’ Input](https://www.gnu.org/software/autoconf/manual/autoconf.html#Input) in The Autoconf Manual.

    Required files from `AC_CONFIG_AUX_DIR` are automatically
    distributed, even if there is no `Makefile.am` in this directory.

`AC_CONFIG_LIBOBJ_DIR` [¶](#index-AC_005fCONFIG_005fLIBOBJ_005fDIR-1)
:   Automake will require the sources file declared with
    `AC_LIBSOURCE` (see below) in the directory specified by this
    macro.

`AC_CONFIG_HEADERS` [¶](#index-AC_005fCONFIG_005fHEADERS)
:   Automake will generate rules to rebuild these headers from the
    corresponding templates (usually, the template for a `foo.h`
    header being `foo.h.in`).

    As with `AC_CONFIG_FILES` (see [Configuration requirements](#Requirements)), parts of the
    specification using shell variables will be ignored as far as
    cleaning, distributing, and rebuilding is concerned.

    Older versions of Automake required the use of
    `AM_CONFIG_HEADER`; this is no longer the case, and that macro
    has indeed been removed.

`AC_CONFIG_LINKS` [¶](#index-AC_005fCONFIG_005fLINKS)
:   Automake will generate rules to remove `configure` generated
    links on ‘`make distclean`’ and to distribute named source files as
    part of ‘`make dist`’.

    As with `AC_CONFIG_FILES` (see [Configuration requirements](#Requirements)), parts of the
    specification using shell variables will be ignored as far as cleaning
    and distributing is concerned. (There are no rebuild rules for links.)

`AC_LIBOBJ` [¶](#index-AC_005fLIBOBJ-2)

`AC_LIBSOURCE` [¶](#index-AC_005fLIBSOURCE-1)

`AC_LIBSOURCES` [¶](#index-AC_005fLIBSOURCES)
:   Automake will automatically distribute any file listed in
    `AC_LIBSOURCE` or `AC_LIBSOURCES`.

    Note that the `AC_LIBOBJ` macro calls `AC_LIBSOURCE`. So if
    an Autoconf macro is documented to call ‘`AC_LIBOBJ([file])`’, then
    `file.c` will be distributed automatically by Automake. This
    encompasses many macros like `AC_FUNC_ALLOCA`,
    `AC_FUNC_MEMCMP`, `AC_REPLACE_FUNCS`, and others.

    By the way, direct assignments to `LIBOBJS` are no longer
    supported. You should always use `AC_LIBOBJ` for this purpose.
    See [`AC_LIBOBJ` vs. `LIBOBJS`](https://www.gnu.org/software/autoconf/manual/autoconf.html#AC_005fLIBOBJ-vs-LIBOBJS) in The Autoconf Manual.

`AC_PROG_RANLIB` [¶](#index-AC_005fPROG_005fRANLIB)
:   This is required if any libraries are built in the package.
    See [Particular Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The
    Autoconf Manual.

`AC_PROG_CXX` [¶](#index-AC_005fPROG_005fCXX)
:   This is required if any C++ source is included. See [Particular Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual.

`AC_PROG_OBJC` [¶](#index-AC_005fPROG_005fOBJC)
:   This is required if any Objective C source is included. See [Particular Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual.

`AC_PROG_OBJCXX` [¶](#index-AC_005fPROG_005fOBJCXX)
:   This is required if any Objective C++ source is included. See [Particular Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual.

`AC_PROG_F77` [¶](#index-AC_005fPROG_005fF77)
:   This is required if any Fortran 77 source is included. See [Particular Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual.

`AC_F77_LIBRARY_LDFLAGS` [¶](#index-AC_005fF77_005fLIBRARY_005fLDFLAGS)
:   This is required for programs and shared libraries that are a mixture of
    languages that include Fortran 77 (see [Mixing Fortran 77 With C and C++](#Mixing-Fortran-77-With-C-and-C_002b_002b)). See [Autoconf macros supplied with Automake](#Macros).

`AC_FC_SRCEXT` [¶](#index-AC_005fFC_005fSRCEXT)
:   Automake will add the flags computed by `AC_FC_SRCEXT` to compilation
    of files with the respective source extension (see [Fortran Compiler Characteristics](https://www.gnu.org/software/autoconf/manual/autoconf.html#Fortran-Compiler) in The Autoconf Manual).

`AC_PROG_FC` [¶](#index-AC_005fPROG_005fFC)
:   This is required if any Fortran 90/95 source is included. This macro is
    distributed with Autoconf version 2.58 and later. See [Particular Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual.

`AC_PROG_LIBTOOL` [¶](#index-AC_005fPROG_005fLIBTOOL)

`LT_INIT` [¶](#index-LT_005fINIT)
:   Automake will turn on processing for `libtool` (see [Introduction](https://www.gnu.org/software/libtool/manual/libtool.html#Top) in The Libtool Manual).

`AC_PROG_YACC` [¶](#index-AC_005fPROG_005fYACC)
:   If a Yacc source file is seen, then you must either use this macro or
    define the variable `YACC` in `configure.ac`. The former is
    preferred (see [Particular Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual).

`AC_PROG_LEX` [¶](#index-AC_005fPROG_005fLEX-1)
:   If a Lex source file is seen, then this macro must be used.
    See [Particular Program Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The
    Autoconf Manual.

`AC_REQUIRE_AUX_FILE` [¶](#index-AC_005fREQUIRE_005fAUX_005fFILE)
:   For each `AC_REQUIRE_AUX_FILE([file])`,
    `automake` will ensure that `file` exists in the
    aux directory, and will complain otherwise. It
    will also automatically distribute the file. This macro should be
    used by third-party Autoconf macros that require some supporting
    files in the aux directory specified with `AC_CONFIG_AUX_DIR`
    above. See [Finding `configure` Input](https://www.gnu.org/software/autoconf/manual/autoconf.html#Input) in The Autoconf Manual.

`AC_SUBST` [¶](#index-AC_005fSUBST)
:   The first argument is automatically defined as a variable in each
    generated `Makefile.in`, unless `AM_SUBST_NOTMAKE` is also
    used for this variable. See [Setting
    Output Variables](https://www.gnu.org/software/autoconf/manual/autoconf.html#Setting-Output-Variables) in The Autoconf Manual.

    For every substituted variable var, `automake` will add
    a line `var = value` to each `Makefile.in` file.
    Many Autoconf macros invoke `AC_SUBST` to set output variables
    this way, e.g., `AC_PATH_XTRA` defines `X_CFLAGS` and
    `X_LIBS`. Thus, you can access these variables as
    `$(X_CFLAGS)` and `$(X_LIBS)` in any `Makefile.am`
    if `AC_PATH_XTRA` is called.

`AM_CONDITIONAL` [¶](#index-AM_005fCONDITIONAL-1)
:   This introduces an Automake conditional (see [Conditionals](#Conditionals)).

`AM_COND_IF` [¶](#index-AM_005fCOND_005fIF-1)
:   This macro allows `automake` to detect subsequent access within
    `configure.ac` to a conditional previously introduced with
    `AM_CONDITIONAL`, thus enabling conditional `AC_CONFIG_FILES`
    (see [Usage of Conditionals](#Usage-of-Conditionals)).

`AM_GNU_GETTEXT` [¶](#index-AM_005fGNU_005fGETTEXT)
:   This macro is required for packages that use GNU gettext
    (see [Gettext](#gettext)). It is distributed with gettext. If Automake sees
    this macro it ensures that the package meets some of gettext’s
    requirements.

`AM_GNU_GETTEXT_INTL_SUBDIR` [¶](#index-AM_005fGNU_005fGETTEXT_005fINTL_005fSUBDIR)
:   This macro specifies that the `intl/` subdirectory is to be built,
    even if the `AM_GNU_GETTEXT` macro was invoked with a first argument
    of ‘`external`’.

`AM_MAINTAINER_MODE([default-mode])` [¶](#index-AM_005fMAINTAINER_005fMODE_0028_005bdefault_002dmode_005d_0029)
:   This macro adds an `--enable-maintainer-mode` option to
    `configure`. If this is used, `automake` will cause
    “maintainer-only” rules to be turned off by default in the
    generated `Makefile.in`s, unless default-mode is
    ‘`enable`’. This macro defines the `MAINTAINER_MODE`
    conditional, which you can use in your own `Makefile.am`.
    See [`missing` and `AM_MAINTAINER_MODE`](#maintainer_002dmode).

`AM_SUBST_NOTMAKE(var)` [¶](#index-AM_005fSUBST_005fNOTMAKE_0028var_0029)
:   Prevent Automake from defining a variable var, even if it is
    substituted by `config.status`. Normally, Automake defines a
    `make` variable for each `configure` substitution,
    i.e., for each `AC_SUBST([var])`. This macro prevents that
    definition from Automake. If `AC_SUBST` has not been called
    for this variable, then `AM_SUBST_NOTMAKE` has no effects.
    Preventing variable definitions may be useful for substitution of
    multi-line values, where `var = @value@` might yield
    unintended results.

`m4_include` [¶](#index-m4_005finclude-1)
:   Files included by `configure.ac` using this macro will be
    detected by Automake and automatically distributed. They will also
    appear as dependencies in `Makefile` rules.

    `m4_include` is seldom used by `configure.ac` authors, but
    can appear in `aclocal.m4` when `aclocal` detects that
    some required macros come from files local to your package (as opposed to
    macros installed in a system-wide directory; see [Auto-generating aclocal.m4: Invoking `aclocal`](#aclocal-Invocation)).

---

Next: [Autoconf macros supplied with Automake](#Macros), Previous: [Other things Automake recognizes](#Optional), Up: [Scanning `configure.ac`, using `aclocal`](#configure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 6.3 Auto-generating aclocal.m4: Invoking `aclocal` [¶](#Auto_002dgenerating-aclocal_002em4_003a-Invoking-aclocal)

Automake includes a number of Autoconf macros that can be used in your
package (see [Autoconf macros supplied with Automake](#Macros)); some of them are required by Automake in
certain situations. These macros must be defined in your
`aclocal.m4`; otherwise they will not be seen by
`autoconf`.

The `aclocal` program will automatically generate
`aclocal.m4` files based on the contents of `configure.ac`.
This provides a convenient way to get Automake-provided macros,
without having to search around. The `aclocal` mechanism
allows other packages to supply their own macros (see [Writing your own aclocal macros](#Extending-aclocal)). You can also use it to maintain your own set of custom
macros (see [Handling Local Macros](#Local-Macros)).

At startup, `aclocal` scans all the `.m4` files it can
find, looking for macro definitions (see [Macro Search Path](#Macro-Search-Path)). Then
it scans `configure.ac`. Any mention of one of the macros found
in the first step causes that macro, and any macros it in turn
requires, to be put into `aclocal.m4`.

*Putting* the file that contains the macro definition into
`aclocal.m4` is usually done by copying the entire text of this
file, including unused macro definitions as well as both ‘`#`’ and
‘`dnl`’ comments. If you want to make a comment that will be
completely ignored by `aclocal`, use ‘`##`’ as the comment
leader.

When a file selected by `aclocal` is located in a subdirectory
specified as a relative search path with `aclocal`’s `-I`
argument, `aclocal` assumes the file belongs to the package
and uses `m4_include` instead of copying it into
`aclocal.m4`. This makes the package smaller, eases dependency
tracking, and causes the file to be distributed automatically.
(See [Handling Local Macros](#Local-Macros), for an example.) Any macro that is found in a
system-wide directory or via an absolute search path will be copied.
So use ‘`` -I `pwd`/reldir ``’ instead of ‘`-I reldir`’ whenever
a relative directory should be considered to be outside the package.

The contents of `acinclude.m4`, if this file exists, are also
automatically included in `aclocal.m4`. We mostly recommend against
using `acinclude.m4` in new packages (see [Handling Local Macros](#Local-Macros)).

While computing `aclocal.m4`, `aclocal` runs
`autom4te` (see [Using `Autom4te`](https://www.gnu.org/software/autoconf/manual/autoconf.html#Using-autom4te) in The Autoconf Manual) in order to trace the macros that are
used, and omit from `aclocal.m4` all macros that are mentioned
but otherwise unexpanded (this can happen when a macro is called
conditionally). `autom4te` is expected to be in the
`PATH`, just like `autoconf`. Its location can be
overridden using the `AUTOM4TE` environment variable.

In practice it is usually simpler to run `autoreconf` instead
of worrying about the required order of the various tools
(see [Using `autoreconf`](https://www.gnu.org/software/autoconf/manual/autoconf.html#autoreconf-Invocation) in The Autoconf Manual).

This section explains the details of `aclocal`.

* [aclocal Options](#aclocal-Options)
* [Macro Search Path](#Macro-Search-Path)
* [Writing your own aclocal macros](#Extending-aclocal)
* [Handling Local Macros](#Local-Macros)
* [Serial Numbers](#Serials)
* [The Future of `aclocal`](#Future-of-aclocal)

---

Next: [Macro Search Path](#Macro-Search-Path), Up: [Auto-generating aclocal.m4: Invoking `aclocal`](#aclocal-Invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 6.3.1 aclocal Options [¶](#aclocal-Options-1)

`aclocal` accepts the following options:

`--automake-acdir=dir` [¶](#index-_002d_002dautomake_002dacdir)
:   Look for Automake-provided macro files in dir instead of
    in the installation directory. This is typically used for debugging.

    If this option is not specified, the value of the environment variable
    `ACLOCAL_AUTOMAKE_DIR`, if set, is used to set the directory
    containing Automake-provided macro files. Otherwise an install-time
    default is used.

`--aclocal-path=path` [¶](#index-_002d_002daclocal_002dpath)
:   Look for local third-party macro files (and the special `dirlist`
    file, see [dirlist](#dirlist)) along path (a colon-separated list of
    directories) instead of in the installation directory. (This can be
    used for building against alternative system roots (“sysroots”) for
    finding headers and libraries.)

    If this option is not specified, the value of the environment variable
    `ACLOCAL_PATH`, if set, is used to set the search path containing
    local third-party macro files. Otherwise an install-time default is
    used.

`--system-acdir=dir` [¶](#index-_002d_002dsystem_002dacdir)
:   Look for the system-wide third-party macro files (and the special
    `dirlist` file) in dir instead of in the installation
    directory. This is typically used for debugging.

`--diff[=command]` [¶](#index-_002d_002ddiff)
:   Run command on the M4 file that would be installed or overwritten
    by `--install`. The default command is ‘`diff -u`’.
    This option implies `--install` and `--dry-run`.

`--dry-run` [¶](#index-_002d_002ddry_002drun)
:   Do not overwrite (or create) `aclocal.m4` and M4 files installed
    by `--install`.

`--help` [¶](#index-_002d_002dhelp-1)
:   Print a summary of the command line options and exit successfully.

`-I dir` [¶](#index-_002dI)
:   Add the directory dir to the list of directories searched for
    `.m4` files.

`--install` [¶](#index-_002d_002dinstall)
:   Install system-wide third-party macros into the first directory
    specified with ‘`-I dir`’ instead of copying them into the
    output file.
    This is also done if dir is an absolute path.

    When this option is used, and only when this option is used,
    `aclocal` will also honor ‘`#serial number`’ lines
    that appear in macros: an M4 file is ignored if there exists another
    M4 file with the same basename and a greater serial number in the
    search path (see [Serial Numbers](#Serials)).

`--force` [¶](#index-_002d_002dforce)
:   Always overwrite the output file. The default is to overwrite the
    output file only when needed, i.e., when its contents change or if one
    of its dependencies is younger.

    This option forces the update of `aclocal.m4` (or the file
    specified with `--output` below) and only this file; it has no
    influence on files that may need to be installed by
    `--install`.

`--output=file` [¶](#index-_002d_002doutput)
:   Cause the output to be put into file instead of `aclocal.m4`.

`--print-ac-dir` [¶](#index-_002d_002dprint_002dac_002ddir)
:   Print the name of the directory that `aclocal` will search to
    find third-party `.m4` files. When this option is given, normal
    processing is suppressed. This option was used *in the past* by
    third-party packages to determine where to install `.m4` macro
    files, but *this usage is today discouraged*, since it causes
    ‘`$(prefix)`’ not to be thoroughly honored (violating the GNU
    Coding Standards). And similar semantics can be better obtained with
    the `ACLOCAL_PATH` environment variable (see [Writing your own aclocal macros](#Extending-aclocal)).

`--verbose` [¶](#index-_002d_002dverbose-1)
:   Print the names of files examined.

`--version` [¶](#index-_002d_002dversion-1)
:   Print the version number of Automake and exit successfully.

`-W CATEGORY`

`--warnings=category` [¶](#index-_002dW-1)
:   Output warnings falling in category, one of:

    `syntax`
    :   dubious syntactic constructs, underquoted macros, unused macros, etc.

    `unsupported`
    :   unknown macros.

    `all`
    :   all the warnings; this is the default.

    `none`
    :   turn off all the warnings.

    `no-category`
    :   turn off warnings in category.

    `error`
    :   treat warnings as errors.

    All warnings are output by default.

    The environment variable `WARNINGS` is honored in the same
    way as it is for `automake` (see [Creating a `Makefile.in`: Invoking `automake`](#automake-Invocation)).

---

Next: [Writing your own aclocal macros](#Extending-aclocal), Previous: [aclocal Options](#aclocal-Options), Up: [Auto-generating aclocal.m4: Invoking `aclocal`](#aclocal-Invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 6.3.2 Macro Search Path [¶](#Macro-Search-Path-1)

By default, `aclocal` searches for `.m4` files in the following
directories, in this order:

`acdir-APIVERSION`
:   This is where the `.m4` macros distributed with Automake itself
    are stored. APIVERSION depends on the Automake release used;
    for example, for Automake 1.11.x, APIVERSION = `1.11`.

`acdir`
:   This directory is intended for third party `.m4` files, and is
    configured when `automake` itself is built. It is
    `@datadir@/aclocal/`, which typically
    expands to `${prefix}/share/aclocal/`. To find the compiled-in
    value of acdir, use the `--print-ac-dir` option
    (see [aclocal Options](#aclocal-Options)).

For example, suppose that Automake 1.11.2 was configured with
`--prefix=/usr/local`. Then, the search path would be:

1. `/usr/local/share/aclocal-1.11.2/`
2. `/usr/local/share/aclocal/`

The paths for the acdir and acdir-APIVERSION directories
can be changed respectively through the `aclocal` options
`--system-acdir` and `--automake-acdir` (see [aclocal Options](#aclocal-Options)). These options are not ordinarily needed by end-users;
they’re mainly intended for use by the Automake test suite, or for
debugging.

As also listed in (see [aclocal Options](#aclocal-Options)), several other options can
be used to change or extend the macro search path.

#### Modifying the Macro Search Path: ‘`-I dir`’ [¶](#Modifying-the-Macro-Search-Path_003a-_002dI-dir)

Any directories specified using `-I` options (see [aclocal Options](#aclocal-Options)) are *prepended* to this search list. Thus,
‘`aclocal -I /foo -I /bar`’ results in the following search path:

1. `/foo`
2. `/bar`
3. acdir-APIVERSION
4. acdir

#### Modifying the Macro Search Path: `dirlist` [¶](#Modifying-the-Macro-Search-Path_003a-dirlist)

There is a third mechanism for customizing the search path: if a
`dirlist` file exists in acdir, then that file is assumed
to contain a list of directory patterns, one per line.
`aclocal` expands these patterns to directory names, and adds
them to the search list *after* all other directories.
`dirlist` entries may use shell wildcards such as ‘`*`’,
‘`?`’, or `[...]`.

For example, suppose
`acdir/dirlist` contains the following:

```
/test1
/test2
/test3*
```

and that `aclocal` was called with the ‘`-I /foo -I /bar`’ options.
Then, the search path would be

1. `/foo`
2. `/bar`
3. acdir-APIVERSION
4. acdir
5. `/test1`
6. `/test2`
7. all directories with path names starting with `/test3`.

If the `--system-acdir=dir` option is used, then
`aclocal` will search for the `dirlist` file in that
dir.

`dirlist` is useful in the following situation: suppose that
Automake 1.11.2 is installed with ‘`--prefix=/usr`’ by the system
vendor. Thus, the default search directories are

1. `/usr/share/aclocal-1.11/`
2. `/usr/share/aclocal/`

However, suppose further that many packages have been manually
installed on the system, with `$prefix=/usr/local`, as is
typical. In that case, many of these “extra” `.m4` files are
in `/usr/local/share/aclocal`. Without `dirlist`, the only
way to force `/usr/bin/aclocal` to find these “extra”
`.m4` files is to always call ‘`aclocal -I
/usr/local/share/aclocal`’. This is inconvenient. With
`dirlist`, one can create a file
`/usr/share/aclocal/dirlist` containing the single line

```
/usr/local/share/aclocal
```

Now, the “default” search path on the affected system is

1. `/usr/share/aclocal-1.11/`
2. `/usr/share/aclocal/`
3. `/usr/local/share/aclocal/`

This way, `-I` options can be reserved for project-specific
needs (`my-source-dir/m4/`), rather than using them to work
around local system-dependent tool installation directories.

Similarly, `dirlist` can be handy if you have installed a local
copy of Automake in your account and want `aclocal` to look for
macros installed at other places on the system.

#### Modifying the Macro Search Path: `--aclocal-path`/`ACLOCAL_PATH` [¶](#Modifying-the-Macro-Search-Path_003a-_002d_002daclocal_002dpath_002fACLOCAL_005fPATH)

The fourth and last mechanism to customize the macro search path: any
directory included in the colon-separated path given to the
`--aclocal-path` command-line option, or (if the option is not
specified) in the `ACLOCAL_PATH` environment variable, is added to
the search path.
These directories take precedence over system directories, including
those found via `dirlist`, with the exception of the versioned
directory acdir-APIVERSION (see [Macro Search Path](#Macro-Search-Path)).
However, directories passed via `-I` will take precedence over
directories in `--aclocal-path`/`ACLOCAL_PATH`.

If the `--install` option is used, any `.m4` file
containing a required macro that is found in a directory listed in
`--aclocal-path`/`ACLOCAL_PATH` will be installed locally.
In this case, serial numbers in `.m4` are honored too
(see [Serial Numbers](#Serials)).

`--aclocal-path`/`ACLOCAL_PATH` is useful if you are using
a global copy of Automake and want `aclocal` to look for
macros somewhere under your home directory. This is more or less the
opposite of `dirlist`.

#### Possible future incompatibility in `aclocal` [¶](#Possible-future-incompatibility-in-aclocal)

The order in which the directories in the macro search path are
currently looked up is confusing and/or suboptimal in various aspects.
In particular, directories in
`--aclocal-path`/`ACLOCAL_PATH` and `acdir`
might end up taking precedence over `acdir-APIVERSION`, and
directories in `acdir/dirlist` might end up taking
precedence over `acdir`. There are no definite plans to
change the current behavior, but if bug reports show the need (so
please report issues), the default might need to be changed, and the
current behavior retained as an option.

---

Next: [Handling Local Macros](#Local-Macros), Previous: [Macro Search Path](#Macro-Search-Path), Up: [Auto-generating aclocal.m4: Invoking `aclocal`](#aclocal-Invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 6.3.3 Writing your own aclocal macros [¶](#Writing-your-own-aclocal-macros)

The `aclocal` program doesn’t have built-in knowledge of any
macros. You can extend it with your own macros.

Libraries, for example, usually want to supply their own Autoconf
macros for use by other programs. For instance, the `gettext`
library supplies a macro `AM_GNU_GETTEXT` that should be used by
any package using `gettext`. When the library is installed,
it installs this macro so that `aclocal` will find it.

A macro file’s name should end in `.m4`. Such files should be
installed in `$(datadir)/aclocal`. This can be done with:

```
aclocaldir = $(datadir)/aclocal
aclocal_DATA = mymacro.m4 myothermacro.m4
```

Please do use `$(datadir)/aclocal`, and not something based on
the result of ‘`aclocal --print-ac-dir`’ (see [Installing to Hard-Coded Locations](#Hard_002dCoded-Install-Paths), for arguments). It might also be helpful to suggest to
the user to add the `$(datadir)/aclocal` directory to their
`ACLOCAL_PATH` variable (see [ACLOCAL\_PATH](#ACLOCAL_005fPATH)) so that
`aclocal` will find the `.m4` files installed by your
package automatically.

A file of macros should be a series of properly quoted
`AC_DEFUN`’s (see [Macro Definitions](https://www.gnu.org/software/autoconf/manual/autoconf.html#Macro-Definitions) in The
Autoconf Manual). The `aclocal` program also understands
`AC_REQUIRE` (see [Prerequisite Macros](https://www.gnu.org/software/autoconf/manual/autoconf.html#Prerequisite-Macros) in The
Autoconf Manual), so it is safe to put each macro in a separate file.
Each file should have no side effects except for defining the
macro(s). In particular, any call to `AC_PREREQ` should be done
inside the defined macro, not at the beginning of the file.

Starting with Automake 1.8, `aclocal` warns about all
underquoted calls to `AC_DEFUN`. We realize this annoys some
people, because `aclocal` was not so strict in the past and
many third party macros are underquoted; and we have to apologize for
this temporary inconvenience. The reason we have to be stricter is
that a possible future implementation of `aclocal`
(see [The Future of `aclocal`](#Future-of-aclocal)) will have to temporarily include all of
these third party `.m4` files, maybe several times, even
including files that end up not being needed. Doing so should
alleviate many problems of the current implementation; however, it
requires a stricter style from macro authors. Hopefully it is easy to
revise the existing macros. For instance,

```
# bad style
AC_PREREQ(2.68)
AC_DEFUN(AX_FOOBAR,
[AC_REQUIRE([AX_SOMETHING])dnl
AX_FOO
AX_BAR
])
```

should be rewritten as

```
AC_DEFUN([AX_FOOBAR],
[AC_PREREQ([2.68])dnl
AC_REQUIRE([AX_SOMETHING])dnl
AX_FOO
AX_BAR
])
```

Wrapping the `AC_PREREQ([2.68])` call inside the macro ensures
that the Autoconf 2.68 will not be a prerequisite if `AX_FOOBAR`
is not used. Most importantly, quoting the first argument of
`AC_DEFUN` allows the macro to be redefined or included twice
(otherwise this first argument would be expanded during the second
definition). For consistency we like to quote even arguments such as
`2.68` that do not require it.

If you have been directed here by the `aclocal` diagnostic but
are not the maintainer of the implicated macro, you will want to
contact the maintainer of that macro. Please try to make sure you
have the latest version of the macro and that the problem hasn’t
already been reported before doing so.

Another situation where `aclocal` is commonly used is to
manage macros that are used locally by the package; [Handling Local Macros](#Local-Macros).

---

Next: [Serial Numbers](#Serials), Previous: [Writing your own aclocal macros](#Extending-aclocal), Up: [Auto-generating aclocal.m4: Invoking `aclocal`](#aclocal-Invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 6.3.4 Handling Local Macros [¶](#Handling-Local-Macros)

Feature tests offered by Autoconf do not cover all needs. People
often have to supplement existing tests with their own macros, or
with third-party macros.

There are two ways to organize custom macros in a package.

The first possibility (the historical practice) is to list all your
macros in `acinclude.m4`. This file will be included in
`aclocal.m4` when you run `aclocal`, and its macro(s) will
henceforth be visible to `autoconf`. However, if it contains
numerous macros, it will rapidly become difficult to maintain, and it
will be almost impossible to share macros between packages.

The second possibility, which we recommend, is to write each macro in
its own file and gather all these files in a directory. This
directory is usually called `m4/`. Then you can update
`configure.ac` by adding a proper call to
`AC_CONFIG_MACRO_DIRS`:

```
AC_CONFIG_MACRO_DIRS([m4])
```

`aclocal` will then take care of automatically adding `m4/`
to its search path for M4 files.

When `aclocal` is run, it will build an `aclocal.m4` that
`m4_include`s any file from `m4/` that defines a required
macro. Macros not found locally will still be searched in system-wide
directories, as explained in [Macro Search Path](#Macro-Search-Path).

Custom macros should be distributed for the same reason that
`configure.ac` is: so that other people have all the sources of
your package if they want to work on it. In fact, this distribution
happens automatically because all `m4_include`d files are
distributed.

There is no consensus on the distribution of third-party macros that
your package may use. Many libraries install their own macro in the
system-wide `aclocal` directory (see [Writing your own aclocal macros](#Extending-aclocal)).
For instance, Guile ships with a file called `guile.m4` that
contains the macro `GUILE_FLAGS` that can be used to define
compiler and linker flags appropriate for using Guile. Using
`GUILE_FLAGS` in `configure.ac` will cause `aclocal`
to copy `guile.m4` into `aclocal.m4`, but as `guile.m4`
is not part of the project, it will not be distributed. Technically,
that means a user who needs to rebuild `aclocal.m4` will have to
install Guile first. This is probably OK, if Guile already is a
requirement to build the package. However, if Guile is only an
optional feature, or if your package might run on architectures where
Guile cannot be installed, this requirement will hinder development.
An easy solution is to copy such third-party macros in your local
`m4/` directory so they get distributed.

Since Automake 1.10, `aclocal` offers the option `--install`
to copy these system-wide third-party macros in your local macro directory,
helping to solve the above problem.

With this setup, system-wide macros will be copied to `m4/`
the first time you run `aclocal`. Then the locally installed
macros will have precedence over the system-wide installed macros
each time `aclocal` is run again.

One reason why you should keep `--install` in the flags even
after the first run is that when you later edit `configure.ac`
and depend on a new macro, this macro will be installed in your
`m4/` automatically. Another one is that serial numbers
(see [Serial Numbers](#Serials)) can be used to update the macros in your source tree
automatically when new system-wide versions are installed. It is good
practice to maintain a serial number for each macro you distribute,
even if you do not use the `--install` option of
`aclocal`: this allows other people to use it.

---

Next: [The Future of `aclocal`](#Future-of-aclocal), Previous: [Handling Local Macros](#Local-Macros), Up: [Auto-generating aclocal.m4: Invoking `aclocal`](#aclocal-Invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 6.3.5 Serial Numbers [¶](#Serial-Numbers)

Because third-party macros defined in `*.m4` files are naturally
shared between multiple projects, people like to version them. This
makes it easier to tell which of two M4 files is newer. Since at
least 1996, the convention is to use a ‘`#serial`’ line for this.

Such a serial number should be a single line of the form

```
# serial version
```

where version is a version number containing only digits and
dots. Usually people use a single integer, and they increment it each
time they change the macro (hence the name of “serial”). Such a
line should appear in the M4 file before any macro definition.

The ‘`#`’ must be the first character on the line,
and it is OK to have extra words after the version, as in

```
#serial version garbage
```

Normally these serial numbers are completely ignored by
`aclocal` and `autoconf`, like any genuine comment.
However when using `aclocal`’s `--install` feature, these
serial numbers will modify the way `aclocal` selects the
macros to install in the package: if two files with the same basename
exist in your search path, and if at least one of them uses a
‘`#serial`’ line, `aclocal` will ignore the file that has
the older ‘`#serial`’ line (or the file that has none).

A serial number applies to a whole M4 file, not to any macro it
contains. A file can contain multiple macros, but only one serial
number.

#### Serial Number Example [¶](#Serial-Number-Example)

Here is a use case that illustrates the use of `--install` and
its interaction with serial numbers. Let’s assume we maintain a
package called MyPackage, the `configure.ac` of which requires a
third-party macro `AX_THIRD_PARTY` defined in
`/usr/share/aclocal/thirdparty.m4` as follows:

```
# serial 1
AC_DEFUN([AX_THIRD_PARTY], [...])
```

MyPackage uses an `m4/` directory to store local macros as
explained in [Handling Local Macros](#Local-Macros), and has

```
AC_CONFIG_MACRO_DIRS([m4])
```

in its `configure.ac`.

Initially the `m4/` directory is empty. The first time we run
`aclocal --install`, it will notice that

* `configure.ac` uses `AX_THIRD_PARTY`
* No local macros define `AX_THIRD_PARTY`
* `/usr/share/aclocal/thirdparty.m4` defines `AX_THIRD_PARTY`
  with serial number 1.

Because `/usr/share/aclocal/thirdparty.m4` is a system-wide macro
and `aclocal` was given the `--install` option, it will
copy this file in `m4/thirdparty.m4`, and output an
`aclocal.m4` that contains ‘`m4_include([m4/thirdparty.m4])`’.

The next time ‘`aclocal --install`’ is run, something different
happens. `aclocal` notices that

* `configure.ac` uses `AX_THIRD_PARTY`
* `m4/thirdparty.m4` defines `AX_THIRD_PARTY`
  with serial number 1.
* `/usr/share/aclocal/thirdparty.m4` defines `AX_THIRD_PARTY`
  with serial number 1.

Because both files have the same serial number, `aclocal` uses
the first it found in its search path order (see [Macro Search Path](#Macro-Search-Path)). `aclocal` therefore ignores
`/usr/share/aclocal/thirdparty.m4` and outputs an
`aclocal.m4` that contains ‘`m4_include([m4/thirdparty.m4])`’.

Local directories specified with `-I` are always searched before
system-wide directories, so a local file will always be preferred to
the system-wide file in case of equal serial numbers.

Now suppose the system-wide third-party macro is changed. This can
happen if the package installing this macro is updated. Let’s suppose
the new macro has serial number 2. The next time ‘`aclocal --install`’
is run the situation is the following:

* `configure.ac` uses `AX_THIRD_PARTY`
* `m4/thirdparty.m4` defines `AX_THIRD_PARTY`
  with serial number 1.
* `/usr/share/aclocal/thirdparty.m4` defines `AX_THIRD_PARTY`
  with serial 2.

When `aclocal` sees a greater serial number, it immediately
forgets anything it knows from files that have the same basename and a
smaller serial number. So after it has found
`/usr/share/aclocal/thirdparty.m4` with serial 2,
`aclocal` will proceed as if it had never seen
`m4/thirdparty.m4`. This brings us back to a situation similar
to that at the beginning of our example, where no local file defined
the macro. `aclocal` will install the new version of the
macro in `m4/thirdparty.m4`, in this case overriding the old
version. MyPackage just had its macro updated as a side effect of
running `aclocal`.

If you are leery of letting `aclocal` update your local
macro, you can run ‘`aclocal --diff`’ to review the changes
‘`aclocal --install`’ would perform on these macros.

Finally, note that the `--force` option of `aclocal`
has no effect on the files installed by `--install`. For
instance, if you have modified your local macros, do not expect
`--install --force` to replace the local macros by their
system-wide versions. If you want to do so, simply erase the local
macros you want to revert, and run ‘`aclocal --install`’.

---

Previous: [Serial Numbers](#Serials), Up: [Auto-generating aclocal.m4: Invoking `aclocal`](#aclocal-Invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 6.3.6 The Future of `aclocal` [¶](#The-Future-of-aclocal)

Ideally, `aclocal` should not be part of Automake. Automake
should focus on generating `Makefile`s; dealing with M4 macros is
more Autoconf’s job. The fact that some people install Automake just
to use `aclocal`, but do not use Automake otherwise, is an
indication of how that feature is misplaced.

A new implementation would probably be done slightly differently. For
instance, it could enforce the `m4/`-style layout discussed in
[Handling Local Macros](#Local-Macros).

We do not know when or whether this will happen. It has been
discussed several times, but someone still has to commit to executing
this non-trivial task.

From the user’s point of view, `aclocal`’s removal might turn
out to be painful. There is a simple precaution that you may take to
make that switch more seamless: never call `aclocal` yourself.
Keep its invocation under the exclusive control of
`autoreconf` and Automake’s rebuild rules. Hopefully you
won’t need to worry about things breaking; when `aclocal`
disappears, because everything will have been taken care of. If
otherwise you used to call `aclocal` directly yourself or from
some script, you will quickly notice the change.

Many packages come with a script called `bootstrap` or
`autogen.sh` that calls `aclocal`, `libtoolize`,
`gettextize` or `autopoint`, `autoconf`,
`autoheader`, and `automake` in the right order. In
fact, this is precisely what `autoreconf` can do for you. If
your package has such a `bootstrap` or `autogen.sh` script,
consider using `autoreconf`. That should simplify its logic
(all to the good), and more to the point you will not call
`aclocal` directly anymore.

For the time being, third-party packages should continue to install
public macros into `/usr/share/aclocal/`. If `aclocal`
is replaced by another tool it might make sense to rename the
directory, but supporting `/usr/share/aclocal/` for backward
compatibility should be feasible, provided all macros are properly
written (see [Writing your own aclocal macros](#Extending-aclocal)).

---

Previous: [Auto-generating aclocal.m4: Invoking `aclocal`](#aclocal-Invocation), Up: [Scanning `configure.ac`, using `aclocal`](#configure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
