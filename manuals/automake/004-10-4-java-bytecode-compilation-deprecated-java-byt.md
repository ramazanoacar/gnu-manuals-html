### 10.4 Java bytecode compilation (deprecated) [¶](#Java-bytecode-compilation-_0028deprecated_0029)

Automake provides some minimal support for Java bytecode compilation with
the `JAVA` primary (in addition to the support for compiling Java to
native machine code; see [Compiling Java sources using gcj](#Java-Support-with-gcj)). Note however that
*the interface and most features described here are deprecated*.
Future Automake releases will strive to provide a better and cleaner
interface, which however *won’t be backward-compatible*; the present
interface will probably be removed altogether some time after the
introduction of the new interface (if that ever materializes). In any
case, the current `JAVA` primary features are frozen and will no
longer be developed, not even to take bug fixes.

Any `.java` files listed in a `_JAVA` variable will be
compiled with `JAVAC` at build time. By default, `.java`
files are not included in the distribution; you should use the
`dist_` prefix to distribute them.

Here is a typical setup for distributing `.java` files and
installing the `.class` files resulting from their compilation.

```
javadir = $(datadir)/java
dist_java_JAVA = a.java b.java ...
```

Currently Automake enforces the restriction that only one `_JAVA`
primary can be used in a given `Makefile.am`. The reason for this
restriction is that, in general, it isn’t possible to know which
`.class` files were generated from which `.java` files, so
it would be impossible to know which files to install where. For
instance, a `.java` file can define multiple classes; the resulting
`.class` file names cannot be predicted without parsing the
`.java` file.

There are a few variables that are used when compiling Java sources:

`JAVAC` [¶](#index-JAVAC)
:   The name of the Java compiler. This defaults to ‘`javac`’.

`JAVACFLAGS` [¶](#index-JAVACFLAGS)
:   The flags to pass to the compiler. This is considered to be a user
    variable (see [Variables reserved for the user](#User-Variables)).

`AM_JAVACFLAGS` [¶](#index-AM_005fJAVACFLAGS)
:   More flags to pass to the Java compiler. This, and not
    `JAVACFLAGS`, should be used when it is necessary to put Java
    compiler flags into `Makefile.am`.

`JAVAROOT` [¶](#index-JAVAROOT)
:   The value of this variable is passed to the `-d` option to
    `javac`. It defaults to ‘`$(top_builddir)`’.

`CLASSPATH_ENV` [¶](#index-CLASSPATH_005fENV)
:   This variable is a shell expression that is used to set the
    `CLASSPATH` environment variable on the `javac` command line.
    (In the future we will probably handle class path setting differently.)

---

Previous: [Java bytecode compilation (deprecated)](#Java), Up: [Other GNU Tools](#Other-GNU-Tools)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 10.5 Python [¶](#Python-1)

Automake provides support for Python compilation with the
`PYTHON` primary. A typical setup is to call
`AM_PATH_PYTHON` in `configure.ac` and use a line like this
in `Makefile.am`:

```
python_PYTHON = tree.py leave.py
```

Python source files are included in the distribution by default;
prepend `nodist_` (as in `nodist_python_PYTHON`) to omit
them.

At install time, any files listed in a `_PYTHON` variable will be
byte-compiled with `py-compile`. `py-compile` creates
both standard (`.pyc`) and optimized (`.pyo`) byte-compiled
versions of the source files. Because byte-compilation occurs at
install time, files listed in `noinst_PYTHON` will not be
compiled.

Automake ships with an Autoconf macro named `AM_PATH_PYTHON` that
determines some Python-related directory variables (see below). If
you have called `AM_PATH_PYTHON` from `configure.ac`, then
you may use the variables
`python_PYTHON` and `pkgpython_PYTHON` to list Python source
files in your `Makefile.am`, depending on whether you want your
files installed in `pythondir` or `pkgpythondir`,
respectively.

Macro: **AM\_PATH\_PYTHON** ([version], [action-if-found], [action-if-not-found]) [¶](#index-AM_005fPATH_005fPYTHON)
:   Search for a Python interpreter on the system. This macro takes three
    optional arguments. The first argument, if present, is the minimum
    version of Python required for this package: `AM_PATH_PYTHON`
    will skip any Python interpreter that is older than version.
    If an interpreter is found and satisfies version, then
    action-if-found is run. Otherwise, action-if-not-found is
    run.

    If action-if-not-found is not specified, as in the following
    example, the default is to abort `configure`:

    ```
    AM_PATH_PYTHON([2.5])
    ```

    This is fine when Python is an absolute requirement for the package.
    If Python ≥ 2.5 was only *optional* for the package,
    `AM_PATH_PYTHON` could be called as follows.

    ```
    AM_PATH_PYTHON([2.5],, [:])
    ```

    If the `PYTHON` variable is set when `AM_PATH_PYTHON` is
    called, then that will be the only Python interpreter that is tried.

    `AM_PATH_PYTHON` creates the following output variables based on
    the Python installation found during configuration:

`PYTHON` [¶](#index-PYTHON-1)
:   The name of the Python executable, or ‘`:`’ if no suitable
    interpreter could be found.

    Assuming action-if-not-found is used (otherwise `./configure`
    will abort if Python is absent), the value of `PYTHON` can be used
    to set up a conditional in order to disable the relevant part of a build
    as follows.

    ```
    AM_PATH_PYTHON(,, [:])
    AM_CONDITIONAL([HAVE_PYTHON], [test "$PYTHON" != :])
    ```

`PYTHON_VERSION` [¶](#index-PYTHON_005fVERSION)
:   The Python version number, in the form major.minor
    (e.g., ‘`2.5`’). This is set to be the value of
    ‘`'%u.%u' % sys.version_info[:2]`’.

`PYTHON_PREFIX` [¶](#index-PYTHON_005fPREFIX)

`PYTHON_EXEC_PREFIX` [¶](#index-PYTHON_005fEXEC_005fPREFIX)
:   With no special options given, these have values ‘`${prefix}`’
    and ‘`${exec_prefix}`’, respectively (unexpanded; see below).

    The `configure` options `--with-python_prefix`
    and `--with-python_exec_prefix` set them to an explicit value.

    The `configure` option `--with-python-sys-prefix` set them
    to the values of Python’s ‘`sys.prefix`’ and ‘`sys.exec_prefix`’
    variables. These often differ from ‘`${prefix}`’ and
    ‘`${exec_prefix}`’, e.g., on platforms such as Mac OS x
    (where Python is usually installed as a Framework).

`PYTHON_PLATFORM` [¶](#index-PYTHON_005fPLATFORM)
:   The canonical name used by Python to describe the operating system, as
    given by ‘`sys.platform`’. This value is sometimes needed when
    building Python extensions.

`pythondir` [¶](#index-pythondir)
:   The subdirectory of the Python install tree in which to install Python
    scripts. By default this is, on all systems,
    `$PYTHON_PREFIX/lib/pythonversion/site-packages`, where
    `$PYTHON_PREFIX` is described above, and version is the
    Python version. (For those knowledgeable about Python installation
    details: systems generally have their own Python installation scheme,
    such as `posix_local` on Debian and related (as of
    Python 3.10), which ends up using a directory named
    `dist-packages`; Automake uses the `posix_prefix` scheme and
    `site-packages`.)

`pkgpythondir` [¶](#index-pkgpythondir)
:   This is the directory under `pythondir` that is named after the
    package. That is, it is ‘`$(pythondir)/$(PACKAGE)`’. It is provided
    as a convenience.

`pyexecdir` [¶](#index-pyexecdir)
:   This is the directory where Python extension modules (shared libraries)
    should be installed. An extension module written in C could be declared
    as follows to Automake:

    ```
    pyexec_LTLIBRARIES = quaternion.la
    quaternion_la_SOURCES = quaternion.c support.c support.h
    quaternion_la_LDFLAGS = -avoid-version -module
    ```

`pkgpyexecdir` [¶](#index-pkgpyexecdir)
:   This is a convenience variable that is defined as
    ‘`$(pyexecdir)/$(PACKAGE)`’.

All of these directory variables have values that can start with either
‘`${prefix}`’ or ‘`${exec_prefix}`’, unexpanded. This works
fine in `Makefile`s, but it makes these variables hard to use in
`configure`. This is mandated by the GNU coding standards, so
that the user can run ‘`make prefix=/foo install`’. The Autoconf
manual has a section with more details on this topic
(see [Installation Directory
Variables](https://www.gnu.org/software/autoconf/manual/autoconf.html#Installation-Directory-Variables) in The Autoconf Manual). See also [Installing to Hard-Coded Locations](#Hard_002dCoded-Install-Paths).

* [Supported versions](#Supported-Python-versions)

---

Up: [Python](#Python)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 10.5.1 Supported versions [¶](#Supported-versions)

Automake guarantees releases will support all Python versions that are still
supported by the Python project at the time of the Automake release. Support
for EOL versions of Python are not guaranteed, but will be considered as long
as it is not onerous to do so, and there are large supported distros including
them. If you need to support older Python versions, please use a previous
Automake release.

Here are the current support plans.

| Version | Status |
| --- | --- |
| 0.x | Not supported |
| 1.x | Not supported |
| 2.0–2.6 | Untested, but should work |
| 2.7 | Supported (until Apr 2026) |
| 3.0–3.3 | Untested, but should work |
| 3.4 | Supported (until Apr 2022) |
| 3.5 | Supported (until Apr 2024) |
| 3.6 | Supported (until Apr 2028) |
| 3.7 | Supported (until Apr 2028) |
| 3.8 | Supported (until Apr 2030) |
| 3.9 | Supported (until Apr 2030) |
| 3.10 | Supported (until Apr 2030) |

---

Next: [What Gets Installed](#Install), Previous: [Other GNU Tools](#Other-GNU-Tools), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 11 Building documentation [¶](#Building-documentation)

Currently Automake provides support for Texinfo and man pages.

* [Texinfo](#Texinfo)
* [Man Pages](#Man-Pages)

---

Next: [Man Pages](#Man-Pages), Up: [Building documentation](#Documentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 11.1 Texinfo [¶](#Texinfo-1)

If the current directory contains Texinfo source, you must declare it
with the `TEXINFOS` primary. Generally Texinfo files are converted
into info, and thus the `info_TEXINFOS` variable is most commonly used
here. Any Texinfo source file should have the `.texi` extension.
Automake also accepts `.txi` or `.texinfo` extensions, but their
use is discouraged now, and will elicit runtime warnings.

Automake generates rules to build `.info`, `.dvi`,
`.ps`, `.pdf` and `.html` files from your Texinfo
sources. Following the GNU Coding Standards, only the `.info`
files are built by ‘`make all`’ and installed by ‘`make
install`’ (unless you use `no-installinfo`, see below).
Furthermore, `.info` files are automatically distributed so that
Texinfo is not a prerequisite for installing your package.

It is worth noting that, contrary to what happens with the other formats,
the generated `.info` files are by default placed in `srcdir`
rather than in the `builddir`. This can be changed with the
`info-in-builddir` option.

If the Texinfo sources are in a subdirectory relative to the Makefile, then
`-I` flags for the subdirectory, both in the source directory and in the
build directory, will automatically be added. There is no need to specify
these in ‘`$(MAKEINFO)`’, ‘`$(MAKEINFOFLAGS)`’, etc.

If a Texinfo source file contains an ‘`@setfilename`’ directive,
and its argument has extension ‘`.info`’ (or no extension, but this
is discouraged), that argument should be the same as the basename of
the Texinfo file, extended with ‘`.info`’. The Make rules
generated by Automake assume this, and will not work if the
‘`@setfilename`’ is for some other name.

If a Texinfo source ‘`foo.texi`’ is not present, but
`foo.texi.in` exists, Texinfo will read that `.texi.in` file
for `@setfilename` and `@include version.texi` (described
below).

Texinfo source files need not be present at all, and if present, need
not contain `@setfilename`. Then the file name given in the
Makefile.am will be used.

Other documentation formats can be built on request by ‘`make
dvi`’, ‘`make ps`’, ‘`make pdf`’ and ‘`make html`’, and they
can be installed with ‘`make install-dvi`’, ‘`make install-ps`’,
‘`make install-pdf`’ and ‘`make install-html`’ explicitly.
‘`make uninstall`’ will remove everything: the Texinfo
documentation installed by default as well as all the above optional
formats.

All of these targets can be extended using ‘`-local`’ rules
(see [Extending Automake Rules](#Extending)).

If a `.texi` file `@include`s `version.texi` (actually
any file named `vers...texi`, then that file will be
automatically generated. The file `version.texi` defines four
Texinfo flags you can reference using `@value{EDITION}`,
`@value{VERSION}`, `@value{UPDATED}`, and
`@value{UPDATED-MONTH}`.

`EDITION`

`VERSION`
:   Both of these flags hold the version number of your program. They are
    kept separate for historical reasons.

`UPDATED`
:   This holds the date the primary `.texi` file was last modified.

`UPDATED-MONTH`
:   This holds the name of the month in which the primary `.texi` file
    was last modified.

The `version.texi` support requires the `mdate-sh`
script; this script is supplied with Automake and automatically
included when `automake` is invoked with the
`--add-missing` option.

If you have multiple Texinfo files, and you want to use the
`version.texi` feature, then you have to have a separate version
file for each Texinfo file. Automake will treat any include in a
Texinfo file that matches `vers*.texi` just like an automatically
generated version file.

Often an Info file depends on more than one `.texi` file. For
instance, in GNU Hello, `hello.texi` includes the file
`fdl.texi`. You can tell Automake about these dependencies using
the `texi_TEXINFOS` variable. Here is how GNU Hello does
it:

```
info_TEXINFOS = hello.texi
hello_TEXINFOS = fdl.texi
```

By default, Automake requires the file `texinfo.tex` to appear in
the same directory as the `Makefile.am` file that lists the
`.texi` files. If you used `AC_CONFIG_AUX_DIR` in
`configure.ac` (see [Finding ‘configure’ Input](https://www.gnu.org/software/autoconf/manual/autoconf.html#Input) in The Autoconf Manual), then `texinfo.tex` is looked for
there. In both cases, `automake` then supplies `texinfo.tex` if
`--add-missing` is given, and takes care of its distribution.
However, if you set the `TEXINFO_TEX` variable (see below),
it overrides the location of the file and turns off its installation
into the source as well as its distribution.

The option `no-texinfo.tex` can be used to eliminate the
requirement for the file `texinfo.tex`. Use of the variable
`TEXINFO_TEX` is preferable, however, because that allows the
`dvi`, `ps`, and `pdf` targets to still work.

Automake generates an `install-info` rule; some people apparently
use this. By default, info pages are installed by ‘`make
install`’, so running `make install-info` is pointless. This can
be prevented via the `no-installinfo` option. In this case,
`.info` files are not installed by default, and user must
request this explicitly using ‘`make install-info`’.

By default, `make install-info` and `make uninstall-info`
will try to run the `install-info` program (if available) to
update (or create/remove) the `${infodir}/dir` index.
If this is undesired, it can be prevented by exporting the
`AM_UPDATE_INFO_DIR` variable to "`no`".

The following variables are used by the Texinfo build rules.

`MAKEINFO` [¶](#index-MAKEINFO)
:   The name of the program invoked to build `.info` files. This
    variable is defined by Automake. If the `makeinfo` program is
    found on the system then it will be used by default; otherwise
    `missing` will be used instead.

`MAKEINFOHTML` [¶](#index-MAKEINFOHTML)
:   The command invoked to build `.html` files. Automake
    defines this to ‘`$(MAKEINFO) --html`’.

`MAKEINFOFLAGS` [¶](#index-MAKEINFOFLAGS)
:   User flags passed to each invocation of ‘`$(MAKEINFO)`’ and
    ‘`$(MAKEINFOHTML)`’. This user variable (see [Variables reserved for the user](#User-Variables)) is
    not expected to be defined in any `Makefile`; it can be used by
    users to pass extra flags to suit their needs.

`AM_MAKEINFOFLAGS` [¶](#index-AM_005fMAKEINFOFLAGS)

`AM_MAKEINFOHTMLFLAGS` [¶](#index-AM_005fMAKEINFOHTMLFLAGS)

`AM_TEXI2FLAGS` [¶](#index-AM_005fTEXI2FLAGS)
:   Maintainer flags passed to each `makeinfo` invocation. Unlike
    `MAKEINFOFLAGS`, these variables are meant to be defined by
    maintainers in `Makefile.am`. ‘`$(AM_MAKEINFOFLAGS)`’ is
    passed to `makeinfo` when building `.info` files;
    ‘`$(AM_MAKEINFOHTMLFLAGS)`’ is used when building `.html`
    files; and ‘`$(AM_TEXI2FLAGS)`’ is used when building `.dvi` and
    `.pdf` files.

    For instance, the following setting can be used to obtain one single
    `.html` file per manual, without node separators.

    ```
    AM_MAKEINFOHTMLFLAGS = --no-headers --no-split
    ```

    `AM_MAKEINFOHTMLFLAGS` defaults to ‘`$(AM_MAKEINFOFLAGS)`’.
    This means that defining `AM_MAKEINFOFLAGS` without defining
    `AM_MAKEINFOHTMLFLAGS` will impact builds of both `.info`
    and `.html` files.

`TEXI2DVI` [¶](#index-TEXI2DVI)
:   The name of the command that converts a `.texi` file into a
    `.dvi` file. This defaults to ‘`texi2dvi`’, a script that ships
    with the Texinfo package.

`TEXI2PDF` [¶](#index-TEXI2PDF)
:   The name of the command that translates a `.texi` file into a
    `.pdf` file. This defaults to ‘`$(TEXI2DVI) --pdf --batch`’.

`DVIPS` [¶](#index-DVIPS)
:   The name of the command that builds a `.ps` file out of a
    `.dvi` file. This defaults to ‘`dvips`’.

`TEXINFO_TEX` [¶](#index-TEXINFO_005fTEX)
:   If your package has Texinfo files in many directories, you can use the
    variable `TEXINFO_TEX` to tell Automake where to find the canonical
    `texinfo.tex` for your package. The value of this variable should
    be the relative path from the current `Makefile.am` to
    `texinfo.tex`:

    ```
    TEXINFO_TEX = ../doc/texinfo.tex
    ```

---

Previous: [Texinfo](#Texinfo), Up: [Building documentation](#Documentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 11.2 Man Pages [¶](#Man-Pages-1)

A package can also include man pages (but see the GNU standards on this
matter, [Man Pages](https://www.gnu.org/prep/standards/standards.html#Man-Pages) in The GNU Coding Standards.) Man
pages are declared using the `MANS` primary. Generally the
`man_MANS` variable is used. Man pages are automatically installed in
the correct subdirectory of `mandir`, based on the file extension.

File extensions such as `.1c` are handled by looking for the valid
part of the extension and using that to determine the correct
subdirectory of `mandir`. Valid section names are the digits
‘`0`’ through ‘`9`’, and the letters ‘`l`’ and ‘`n`’.

Sometimes developers prefer to name a man page something like
`foo.man` in the source, and then rename it to have the correct
suffix, for example `foo.1`, when installing the file. Automake
also supports this mode. For a valid section named section,
there is a corresponding directory named ‘`mansectiondir`’,
and a corresponding `_MANS` variable. Files listed in such a
variable are installed in the indicated section. If the file already
has a valid suffix, then it is installed as-is; otherwise the file
suffix is changed to match the section.

For instance, consider this example:

```
man1_MANS = rename.man thesame.1 alsothesame.1c
```

In this case, `rename.man` will be renamed to `rename.1` when
installed, but the other files will keep their names.

By default, man pages are installed by ‘`make install`’. However,
since the GNU project does not require man pages, many maintainers do
not expend effort to keep the man pages up to date. In these cases, the
`no-installman` option will prevent the man pages from being
installed by default. The user can still explicitly install them via
‘`make install-man`’.

For fast installation, with many files it is preferable to use
‘`mansection_MANS`’ over ‘`man_MANS`’ as well as files that
do not need to be renamed.

Man pages are not currently considered to be source, because it is not
uncommon for man pages to be automatically generated. Therefore they
are not automatically included in the distribution. However, this can
be changed by use of the `dist_` prefix. For instance here is
how to distribute and install the two man pages of GNU `cpio`
(which includes both Texinfo documentation and man pages):

```
dist_man_MANS = cpio.1 mt.1
```

The `nobase_` prefix is meaningless for man pages and is
disallowed.

Executables and manpages may be renamed upon installation
(see [Renaming Programs at Install Time](#Renaming)). For manpages this can be avoided by use of the
`notrans_` prefix. For instance, suppose an executable ‘`foo`’
allowing to access a library function ‘`foo`’ from the command line.
The way to avoid renaming of the `foo.3` manpage is:

```
man_MANS = foo.1
notrans_man_MANS = foo.3
```

‘`notrans_`’ must be specified first when used in conjunction with
either ‘`dist_`’ or ‘`nodist_`’ (see [Fine-grained Distribution Control](#Fine_002dgrained-Distribution-Control)). For instance:

```
notrans_dist_man3_MANS = bar.3
```

---

Next: [What Gets Cleaned](#Clean), Previous: [Building documentation](#Documentation), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 12 What Gets Installed [¶](#What-Gets-Installed)

Naturally, Automake handles the details of installing your program
once it has been built. All files named by the various primaries are
automatically installed in the appropriate places when the user runs
‘`make install`’.

* [Basics of Installation](#Basics-of-Installation)
* [The Two Parts of Install](#The-Two-Parts-of-Install)
* [Extending Installation](#Extending-Installation)
* [Staged Installs](#Staged-Installs)
* [Install Rules for the User](#Install-Rules-for-the-User)

---

Next: [The Two Parts of Install](#The-Two-Parts-of-Install), Up: [What Gets Installed](#Install)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 12.1 Basics of Installation [¶](#Basics-of-Installation-1)

A file named in a primary is installed by copying the built file into
the appropriate directory. The base name of the file is used when
installing.

```
bin_PROGRAMS = hello subdir/goodbye
```

In this example, both ‘`hello`’ and ‘`goodbye`’ will be installed
in ‘`$(bindir)`’.

Sometimes it is useful to avoid the basename step at install time. For
instance, you might have a number of header files in subdirectories of
the source tree that are laid out precisely how you want to install
them. In this situation you can use the `nobase_` prefix to
suppress the base name step. For example:

```
nobase_include_HEADERS = stdio.h sys/types.h
```

will install `stdio.h` in ‘`$(includedir)`’ and `types.h`
in ‘`$(includedir)/sys`’.

For most file types, Automake will install multiple files at once, while
avoiding command line length issues (see [Staying below the command line length limit](#Length-Limitations)). Since
some `install` programs will not install the same file twice in
one invocation, you may need to ensure that file lists are unique within
one variable such as ‘`nobase_include_HEADERS`’ above.

You should not rely on the order in which files listed in one variable
are installed. Likewise, to cater for parallel make, you should not
rely on any particular file installation order even among different
file types (library dependencies are an exception here).

---

Next: [Extending Installation](#Extending-Installation), Previous: [Basics of Installation](#Basics-of-Installation), Up: [What Gets Installed](#Install)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 12.2 The Two Parts of Install [¶](#The-Two-Parts-of-Install-1)

Automake generates separate `install-data` and `install-exec`
rules, in case the installer is installing on multiple machines that
share directory structure—these targets allow the machine-independent
parts to be installed only once. `install-exec` installs
platform-dependent files, and `install-data` installs
platform-independent files. The `install` target depends on both
of these targets. While Automake tries to automatically segregate
objects into the correct category, the `Makefile.am` author is, in
the end, responsible for making sure this is done correctly.

Variables using the standard directory prefixes ‘`data`’,
‘`info`’, ‘`man`’, ‘`include`’, ‘`oldinclude`’,
‘`pkgdata`’, or ‘`pkginclude`’ are installed by
`install-data`.

Variables using the standard directory prefixes ‘`bin`’,
‘`sbin`’, ‘`libexec`’, ‘`sysconf`’, ‘`localstate`’,
‘`lib`’, or ‘`pkglib`’ are installed by `install-exec`.

For instance, `data_DATA` files are installed by `install-data`,
while `bin_PROGRAMS` files are installed by `install-exec`.

Any variable using a user-defined directory prefix with
‘`exec`’ in the name (e.g.,
`myexecbin_PROGRAMS`) is installed by `install-exec`. All
other user-defined prefixes are installed by `install-data`.

---

Next: [Staged Installs](#Staged-Installs), Previous: [The Two Parts of Install](#The-Two-Parts-of-Install), Up: [What Gets Installed](#Install)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 12.3 Extending Installation [¶](#Extending-Installation-1)

It is possible to extend this mechanism by defining an
`install-exec-local` or `install-data-local` rule. If these
rules exist, they will be run at ‘`make install`’ time. These
rules can do almost anything; care is required.

Automake also supports two install hooks, `install-exec-hook` and
`install-data-hook`. These hooks are run after all other install
rules of the appropriate type, exec or data, have completed. So, for
instance, it is possible to perform post-installation modifications
using an install hook. See [Extending Automake Rules](#Extending), for some examples.

---

Next: [Install Rules for the User](#Install-Rules-for-the-User), Previous: [Extending Installation](#Extending-Installation), Up: [What Gets Installed](#Install)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 12.4 Staged Installs [¶](#Staged-Installs-1)

Automake generates support for the `DESTDIR` variable in all
install rules. `DESTDIR` is used during the ‘`make install`’
step to relocate install objects into a staging area. Each object and
path is prefixed with the value of `DESTDIR` before being copied
into the install area. Here is an example of typical DESTDIR usage:

```
mkdir /tmp/staging &&
make DESTDIR=/tmp/staging install
```

The `mkdir` command avoids a security problem if the attacker
creates a symbolic link from `/tmp/staging` to a victim area;
then `make` places install objects in a directory tree built under
`/tmp/staging`. If `/gnu/bin/foo` and
`/gnu/share/aclocal/foo.m4` are to be installed, the above command
would install `/tmp/staging/gnu/bin/foo` and
`/tmp/staging/gnu/share/aclocal/foo.m4`.

This feature is commonly used to build install images and packages
(see [Building Binary Packages Using DESTDIR](#DESTDIR)).

Support for `DESTDIR` is implemented by coding it directly into
the install rules. If your `Makefile.am` uses a local install
rule (e.g., `install-exec-local`) or an install hook, then you
must write that code to respect `DESTDIR`.

See [Makefile Conventions](https://www.gnu.org/prep/standards/standards.html#Makefile-Conventions) in The GNU Coding Standards,
for another usage example.

---

Previous: [Staged Installs](#Staged-Installs), Up: [What Gets Installed](#Install)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 12.5 Install Rules for the User [¶](#Install-Rules-for-the-User-1)

Automake also generates rules for targets `uninstall`,
`installdirs`, and `install-strip`.

Automake supports `uninstall-local` and `uninstall-hook`.
There is no notion of separate uninstalls for “exec” and “data”, as
these features would not provide additional functionality.

Note that `uninstall` is not meant as a replacement for a real
packaging tool.

---

Next: [What Goes in a Distribution](#Dist), Previous: [What Gets Installed](#Install), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 13 What Gets Cleaned [¶](#What-Gets-Cleaned)

The GNU Makefile Standards specify a number of different clean rules.
See [Standard Targets for Users](https://www.gnu.org/prep/standards/standards.html#Standard-Targets) in The GNU Coding Standards.

Generally the files that can be cleaned are determined automatically by
Automake. Of course, Automake also recognizes some variables that can
be defined to specify additional files to clean. These variables are
`MOSTLYCLEANFILES`, `CLEANFILES`, `DISTCLEANFILES`, and
`MAINTAINERCLEANFILES`.

When cleaning involves more than deleting some hard-coded list of
files, it is also possible to supplement the cleaning rules with your
own commands. Simply define a rule for any of the
`mostlyclean-local`, `clean-local`, `distclean-local`,
or `maintainer-clean-local` targets (see [Extending Automake Rules](#Extending)). A common
case is deleting a directory, for instance, a directory created by the
test suite:

```
clean-local:
        -rm -rf testSubDir
```

Since `make` allows only one set of rules for a given target,
a more extensible way of writing this is to use a separate target
listed as a dependency:

```
clean-local: clean-local-check
.PHONY: clean-local-check
clean-local-check:
        -rm -rf testSubDir
```

As the GNU Standards aren’t always explicit as to which files should
be removed by which rule, we’ve adopted a heuristic that we believe
was first formulated by François Pinard:

* If `make` built it, and it is commonly something that one would
  want to rebuild (for instance, a `.o` file), then
  `mostlyclean` should delete it.
* Otherwise, if `make` built it, then `clean` should delete it.
* If `configure` built it, then `distclean` should delete it.
* If the maintainer built it (for instance, a `.info` file), then
  `maintainer-clean` should delete it. However
  `maintainer-clean` should not delete anything that needs to exist
  in order to run ‘`./configure && make`’.

We recommend that you follow this same set of heuristics in your
`Makefile.am`.

---

Next: [Support for test suites](#Tests), Previous: [What Gets Cleaned](#Clean), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 14 What Goes in a Distribution [¶](#What-Goes-in-a-Distribution)

* [Basics of Distribution](#Basics-of-Distribution)
* [Fine-grained Distribution Control](#Fine_002dgrained-Distribution-Control)
* [The dist Hook](#The-dist-Hook)
* [Checking the Distribution](#Checking-the-Distribution)
* [The Types of Distributions](#The-Types-of-Distributions)

---

Next: [Fine-grained Distribution Control](#Fine_002dgrained-Distribution-Control), Up: [What Goes in a Distribution](#Dist)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 14.1 Basics of Distribution [¶](#Basics-of-Distribution-1)

The `dist` rule in the generated `Makefile.in` can be used
to generate a gzipped `tar` file and/or other flavors of archives
for distribution. The file is named based on the `PACKAGE` and
`VERSION` variables automatically defined by either the
`AC_INIT` invocation or by a *deprecated* two-arguments
invocation of the `AM_INIT_AUTOMAKE` macro (see [Public Macros](#Public-Macros) for how these variables get their values, from either defaults
or explicit values—it’s slightly trickier than one would expect).
More precisely, the gzipped `tar` file is named
‘`${PACKAGE}-${VERSION}.tar.gz`’.

For compatibility with existing packages, the `VERSION` value may
contain a shell command substitution. For example, a `Makefile.am`
might compute it from a source file using backquotes. In that case,
`$(VERSION)` must not be expanded into targets, prerequisites, or
other syntax interpreted by `make`. Its eventual expansion must
occur in a shell recipe where the command substitution is interpreted.
A literal version is simpler and is recommended for new packages.

You can set the environment (or `Makefile.am`) variable `TAR`
to override the tar program used; it defaults to `tar`.
See [The Types of Distributions](#The-Types-of-Distributions), for how to generate other kinds of
archives.

With GNU tar, you can also set the environment (or `Makefile.am`)
variable `TAR_OPTIONS` to pass options to `tar`. One common
case for using this is to avoid having the local user’s uid and gid in
the tar file, or the uid being larger than is supported by the tar
format. This can be done with, for example:

```
TAR_OPTIONS = --owner=0 --group=0
export TAR_OPTIONS
```

The `export` (a GNU make feature) is necessary to pass the
variable in the environment to the `tar` invocation. Another
possible fix is to use the `tar-pax` option
(see [tar-formats](#tar_002dformats)), though this reduces portability.
(For more discussion, see <https://bugs.gnu.org/19615>
and <https://bugs.gnu.org/73316>.)

For the most part, the files to distribute are automatically
found by Automake:

* All source files are automatically included in a distribution, as are
  all `Makefile.am` and `Makefile.in` files.
* Files that are read by `configure` are automatically
  distributed. These are the source files as specified in various
  Autoconf macros such as `AC_CONFIG_FILES` and siblings.
* Files included in a `Makefile.am` (using `include`) or in
  `configure.ac` (using `m4_include`).
* Automake has a built-in list of commonly used files automatically
  included in the distribution if they are found in the current
  directory (either physically, or as the target of a `Makefile.am`
  rule). Some common examples: `ABOUT-GNU`, `COPYING`,
  `TODO`.

  This list also includes helper scripts installed with ‘`automake
  --add-missing`’. Some common examples: `compile`,
  `config.guess`, `config.rpath`, `config.sub`,
  `texinfo.tex`.
* Automake has another built-in list of files automatically distributed
  if they are found either with the plain name, or with extension
  `.md` (presumably MarkDown, though this not checked). They are
  checked for in that order, so the plain name is preferred. These are:
  `AUTHORS ChangeLog INSTALL NEWS README README-alpha THANKS`.

  For both of these lists, a file that is not currently present on the
  file system is still a part of the distribution if a
  `Makefile.am` rule can build it. When that rule is inside an
  Automake conditional, the file is distributed only when the condition
  is true.
* A final built-in list of files are those distributed only if other
  certain conditions hold. For example,
  the files `config.h.top` and `config.h.bot` are
  automatically distributed only if, e.g.,
  ‘`AC_CONFIG_HEADERS([config.h])`’ is used in `configure.ac`).
  `README-alpha` is another such file, with `README-alpha.md`
  distributed if that is what is available; see [Strictness](#Strictness), for its
  conditions for distribution.

These three lists of files are given in their entirety in the output
from `automake --help`.

Despite all this automatic inclusion, it is still common to have files
to be distributed which are not found by the automatic rules. You
should listed these files in the `EXTRA_DIST` variable. You can
mention files in subdirectories in `EXTRA_DIST`.

You can also mention a directory in `EXTRA_DIST`; in this case
the entire directory will be recursively copied into the distribution.
To emphasize, this copies *everything* in the directory,
including temporary editor files, intermediate build files, version
control files, etc.; thus we recommend against using this feature
as-is. However, you can use the `dist-hook` feature to
ameliorate the problem; see [The dist Hook](#The-dist-Hook).

If you define `SUBDIRS`, Automake will recursively include the
subdirectories in the distribution. If `SUBDIRS` is defined
conditionally (see [Conditionals](#Conditionals)), Automake will normally include
all directories that could possibly appear in `SUBDIRS` in the
distribution. If you need to specify the set of directories
conditionally, you can set the variable `DIST_SUBDIRS` to the
exact list of subdirectories to include in the distribution
(see [Conditional Subdirectories](#Conditional-Subdirectories)).

---

Next: [The dist Hook](#The-dist-Hook), Previous: [Basics of Distribution](#Basics-of-Distribution), Up: [What Goes in a Distribution](#Dist)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 14.2 Fine-grained Distribution Control [¶](#Fine_002dgrained-Distribution-Control-1)

Sometimes you need tighter control over what does *not* go into the
distribution; for instance, you might have source files that are
generated and that you do not want to distribute. In this case
Automake gives fine-grained control using the `dist` and
`nodist` prefixes. Any primary or `_SOURCES` variable can be
prefixed with `dist_` to add the listed files to the distribution.
Similarly, `nodist_` can be used to omit the files from the
distribution.

As an example, here is how you would cause some data to be distributed
while leaving some source code out of the distribution:

```
dist_data_DATA = distribute-this
bin_PROGRAMS = foo
nodist_foo_SOURCES = do-not-distribute.c
```

---

Next: [Checking the Distribution](#Checking-the-Distribution), Previous: [Fine-grained Distribution Control](#Fine_002dgrained-Distribution-Control), Up: [What Goes in a Distribution](#Dist)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 14.3 The dist Hook [¶](#The-dist-Hook-1)

Occasionally it is useful to be able to change the distribution before
it is packaged up. If the `dist-hook` rule exists, it is run
after the distribution directory is filled, but before the actual
distribution archives are created. One way to use this is for
removing unnecessary files that get recursively included by specifying
a directory in `EXTRA_DIST`:

```
EXTRA_DIST = doc
dist-hook:
        chmod -R u+w $(distdir)/doc
        rm -rf `find $(distdir)/doc -type d -name RCS`
```

The `dist-hook` recipe should not assume that the regular files
in the distribution directory are writable; this might not be the case
if one is packaging from a read-only source tree, or when a `make
distcheck` is being done. Similarly, the recipe should not assume
that the subdirectories put into the distribution directory as a
result of being listed in `EXTRA_DIST` are writable. So, if the
`dist-hook` recipe wants to modify the content of an existing
file (or `EXTRA_DIST` subdirectory) in the distribution
directory, it should explicitly to make it writable first:

```
EXTRA_DIST = README doc
dist-hook:
        chmod u+w $(distdir)/README $(distdir)/doc
        echo "Distribution date: `date`" >> $(distdir)/README
        rm -f $(distdir)/doc/HACKING
```

Two variables that come handy when writing `dist-hook` rules are
‘`$(distdir)`’ and ‘`$(top_distdir)`’.

‘`$(distdir)`’ points to the directory where the `dist` rule
will copy files from the current directory before creating the
tarball. If you are at the top-level directory, then ‘`distdir =
$(PACKAGE)-$(VERSION)`’. When used from subdirectory named
`foo/`, then ‘`distdir = ../$(PACKAGE)-$(VERSION)/foo`’.
‘`$(distdir)`’ can be either a relative or absolute path; do not
assume a particular form.

‘`$(top_distdir)`’ always points to the root directory of the
distributed tree. At the top level it’s equal to ‘`$(distdir)`’.
In the `foo/` subdirectory ‘`top_distdir =
../$(PACKAGE)-$(VERSION)`’. ‘`$(top_distdir)`’ can also be either a
relative or absolute path.

When packages are nested using `AC_CONFIG_SUBDIRS`
(see [Nesting Packages](#Subpackages)), then ‘`$(distdir)`’ and
‘`$(top_distdir)`’ are relative to the package where ‘`make
dist`’ was run, not to any sub-packages involved.

---

Next: [The Types of Distributions](#The-Types-of-Distributions), Previous: [The dist Hook](#The-dist-Hook), Up: [What Goes in a Distribution](#Dist)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 14.4 Checking the Distribution [¶](#Checking-the-Distribution-1)

Automake also generates a `distcheck` rule that can be of help
to ensure that a given distribution will actually work. Simplifying
a bit, we can say this rule first makes a distribution, and then,
*operating from it*, takes the following steps (in this order):

* does a `VPATH` build (see [Parallel Build Trees (a.k.a. VPATH Builds)](#VPATH-Builds)), with the
  `srcdir` and all its content made *read-only*;
* makes the printable documentation (with `make dvi`), if any,
* runs the test suite (with `make check`) on this fresh build;
* installs the package in a temporary directory (with `make
  install`), and runs the test suite on the resulting installation
  (with `make installcheck`);
* checks that the package can be correctly uninstalled (by `make
  uninstall`) and cleaned (by `make distclean`);
* finally, makes another tarball to ensure the distribution is
  self-contained.

All of these actions are performed in a temporary directory. The
exact location and the exact structure of such a directory (where the
read-only sources are placed, how the temporary build and install
directories are named and how deeply they are nested, etc.) is to be
considered an implementation detail, which can change at any time, so
please do not rely on it.

* [`DISTCHECK_CONFIGURE_FLAGS`](#DISTCHECK_005fCONFIGURE_005fFLAGS)
* [`distcheck-hook`](#distcheck_002dhook)
* [`dvi` and `distcheck`](#dvi-and-distcheck)
* [`distcleancheck`](#distcleancheck)
* [`distuninstallcheck`](#distuninstallcheck)
* [Errors with `distclean`](#Errors-with-distclean)

---

Next: [`distcheck-hook`](#distcheck_002dhook), Up: [Checking the Distribution](#Checking-the-Distribution)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 14.4.1 `DISTCHECK_CONFIGURE_FLAGS` [¶](#DISTCHECK_005fCONFIGURE_005fFLAGS-1)

Building the package involves running ‘`./configure`’. If you need
to supply additional flags to `configure`, define them in the
`AM_DISTCHECK_CONFIGURE_FLAGS` variable in your top-level
`Makefile.am`. The user can still extend or override the flags
provided there by defining the `DISTCHECK_CONFIGURE_FLAGS` variable,
on the command line when invoking `make`.
It’s worth noting that `make distcheck` needs complete control
over the `configure` options `--srcdir` and
`--prefix`, so those options cannot be overridden by
`AM_DISTCHECK_CONFIGURE_FLAGS` nor by
`DISTCHECK_CONFIGURE_FLAGS`.

Developers are encouraged to strive to make their code buildable
without requiring any special configure option; thus, in general, you
shouldn’t define `AM_DISTCHECK_CONFIGURE_FLAGS`. GNU
`m4` offers an example of when its use is justified, however.
GNU `m4` configures by default with its experimental and
seldom used ‘`changeword`’ feature disabled; so in this case it is
useful to have `make distcheck` run configure with the
`--with-changeword` option, to ensure that the code for
changeword support still compiles correctly. GNU `m4` also
employs the `AM_DISTCHECK_CONFIGURE_FLAGS` variable to
stress-test the use of `--program-prefix=g`, since at one point
the `m4` build system had a bug where `make
installcheck` was wrongly assuming it could blindly test ‘`m4`’,
rather than the just-installed ‘`gm4`’.

---

Next: [`dvi` and `distcheck`](#dvi-and-distcheck), Previous: [`DISTCHECK_CONFIGURE_FLAGS`](#DISTCHECK_005fCONFIGURE_005fFLAGS), Up: [Checking the Distribution](#Checking-the-Distribution)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 14.4.2 `distcheck-hook` [¶](#distcheck_002dhook-1)

If the `distcheck-hook` rule is defined in your top-level
`Makefile.am`, then it will be invoked by `distcheck` after
the new distribution has been unpacked, but before the unpacked copy
is configured and built. Your `distcheck-hook` can do almost
anything, though as always caution is advised. Generally this hook is
used to check for potential distribution errors not caught by the
standard mechanism.

`distcheck-hook`, as well as `AM_DISTCHECK_CONFIGURE_FLAGS`
and `DISTCHECK_CONFIGURE_FLAGS`, are not honored in a subpackage
`Makefile.am`, but the flags from
`AM_DISTCHECK_CONFIGURE_FLAGS` and
`DISTCHECK_CONFIGURE_FLAGS` are passed down to the
`configure` script of the subpackage.

---

Next: [`distcleancheck`](#distcleancheck), Previous: [`distcheck-hook`](#distcheck_002dhook), Up: [Checking the Distribution](#Checking-the-Distribution)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 14.4.3 `dvi` and `distcheck` [¶](#dvi-and-distcheck-1)

Ordinarily, `make distcheck` runs `make dvi`. It does
nothing if the distribution contains no Texinfo sources. If the
distribution does contain a Texinfo manual, by default the `dvi`
target will run TeX to make sure it can be successfully processed
(see [Texinfo](#Texinfo)).

However, you may wish to test the manual by producing `pdf`
(e.g., if your manual uses images in formats other than `eps`),
`html` (if you don’t have TeX at all), some other format, or
just skip the test entirely (not recommended). You can change the
target that is run by setting the variable
`AM_DISTCHECK_DVI_TARGET` in your `Makefile.am`; for
example,

```
AM_DISTCHECK_DVI_TARGET = pdf
```

To make `dvi` into a do-nothing target, see the example for
`EMPTY_AUTOMAKE_TARGETS` in [Third-Party `Makefile`s](#Third_002dParty-Makefiles).

---

Next: [`distuninstallcheck`](#distuninstallcheck), Previous: [`dvi` and `distcheck`](#dvi-and-distcheck), Up: [Checking the Distribution](#Checking-the-Distribution)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 14.4.4 `distcleancheck` [¶](#distcleancheck-1)

`distcheck` ensures that the `distclean` rule actually
removes all built files. This is done by running ‘`make
distcleancheck`’ at the end of the `VPATH` build. By default,
`distcleancheck` will run `distclean` and then make sure the
build tree has been emptied by running the value of the variable
‘`$(distcleancheck_listfiles)`’. Often this check will find
generated files that you forgot to add to the `DISTCLEANFILES`
variable (see [What Gets Cleaned](#Clean)).

The `distcleancheck` behavior should be OK for most packages,
otherwise you have the possibility to override the definition of
either the `distcleancheck` rule, or the
‘`$(distcleancheck_listfiles)`’ variable. For instance, to disable
`distcleancheck` completely (not recommended), add the following
rule to your top-level `Makefile.am`:

```
distcleancheck:
        @:
```

If you want `distcleancheck` to ignore built files that have not
been cleaned because they are also part of the distribution, make the
following definition:

```
distcleancheck_listfiles = \
  find . -type f -exec sh -c 'test -f $(srcdir)/$$1 || echo $$1' \
       sh '{}' ';'
```

The above definition is not the default because it’s usually an error
if your Makefiles cause some distributed files to be rebuilt when the
user builds the package: consider the user missing the tool required
to build the file; or if the required tool is built by your package,
consider the cross-compilation case where it can’t be run.

Please see the (following) section [Errors with `distclean`](#Errors-with-distclean) before
playing with `distcleancheck_listfiles`.

---

Next: [Errors with `distclean`](#Errors-with-distclean), Previous: [`distcleancheck`](#distcleancheck), Up: [Checking the Distribution](#Checking-the-Distribution)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 14.4.5 `distuninstallcheck` [¶](#distuninstallcheck-1)

`distcheck` also checks that the `uninstall` rule works
properly, both for ordinary and `DESTDIR` builds. It does this
by invoking ‘`make uninstall`’, and then it checks the install tree
to see if any files are left over. This check will make sure that you
correctly coded your `uninstall`-related rules.

By default, the checking is done by the `distuninstallcheck`
rule, and the list of files in the install tree is generated by
‘`$(distuninstallcheck_listfiles)`’. The value of the latter
variable is taken to be a shell command to run that prints the list of
files to stdout.

Either of these can be overridden to modify the behavior of
`distcheck`. For instance, to disable this check completely (not
recommended), you would write:

```
distuninstallcheck:
        @:
```

---

Previous: [`distuninstallcheck`](#distuninstallcheck), Up: [Checking the Distribution](#Checking-the-Distribution)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 14.4.6 Errors with `distclean` [¶](#Errors-with-distclean-1)

As explained in the section above (see [Checking the Distribution](#Checking-the-Distribution)),
‘`make distcheck`’ attempts to build and check your package for
errors. One such error you might see is:

```
ERROR: files left in build directory after distclean:
```

The file(s) left in the build directory after ‘`make distclean`’
has run are listed after this error message. This can happen in two
ways:

* files that were forgotten to be distclean-ed;
* distributed files that are erroneously rebuilt.

In the first case of simple left-over files not intended to be
distributed, the fix is to include them for cleaning (see [What Gets Cleaned](#Clean));
this is straightforward and doesn’t need more explanation.

The second case, however, is not always easy to understand and fix, so
let’s proceed with an example. Suppose our package contains a program
for which we want to build a man page using `help2man`. GNU
`help2man` produces simple manual pages from the
`--help` and `--version` output of other commands
(see [The Help2man Manual](https://www.gnu.org/software/help2man/help2man.html#Top)). Because we don’t want to
force our users to install `help2man`, we distribute the
generated man page using the following setup.

```
# This Makefile.am is bogus.
bin_PROGRAMS = foo
foo_SOURCES = foo.c
dist_man_MANS = foo.1

foo.1: foo$(EXEEXT)
        help2man --output=foo.1 ./foo$(EXEEXT)
```

This will effectively distribute the man page. However,
‘`make distcheck`’ will fail with:

```
ERROR: files left in build directory after distclean:
./foo.1
```

Why was `foo.1` rebuilt? Because although distributed,
`foo.1` depends on a non-distributed built file:
`foo$(EXEEXT)`. `foo$(EXEEXT)` is built by the user, so it
will always appear to be newer than the distributed `foo.1`.

In other words, ‘`make distcheck`’ caught an inconsistency in our
package. Our intent was to distribute `foo.1` so users do not
need to install `help2man`, but since this rule causes this
file to be always rebuilt, users *do* need `help2man`.
Either we should ensure that `foo.1` is not rebuilt by users, or
there is no point in distributing `foo.1`.

More generally, the rule is that distributed files should never depend
on non-distributed built files. If you distribute something
generated, distribute all its sources.

One way to fix the above example, while still distributing
`foo.1`, is to not depend on `foo$(EXEEXT)`, but instead on
relevant source files. For instance, assuming `foo --version`
and `foo --help` do not change unless `foo.c` or
`configure.ac` change, we could write the following
`Makefile.am`:

```
bin_PROGRAMS = foo
foo_SOURCES = foo.c
dist_man_MANS = foo.1

foo.1: foo.c $(top_srcdir)/configure.ac
        $(MAKE) $(AM_MAKEFLAGS) foo$(EXEEXT)
        help2man --output=foo.1 ./foo$(EXEEXT)
```

This way, `foo.1` will not get rebuilt every time
`foo$(EXEEXT)` changes. The `make` call makes sure
`foo$(EXEEXT)` is up-to-date before `help2man`.

Another step towards ensuring this would be to use separate
directories for binaries and man pages, and set `SUBDIRS` so that
binaries are built before man pages. Unfortunately, this alone is, in
general, not sufficient. In order to avoid to avoid concurrency bugs,
it may be necessary to include wrappers; this is done by GNU Autoconf,
as mentioned below.

We could also decide not to distribute `foo.1`. In this case
it’s fine to have `foo.1` dependent upon `foo$(EXEEXT)`,
since both will have to be rebuilt. However, it might be impossible
to build the package in a cross-compilation, because building
`foo.1` involves an *execution* of `foo$(EXEEXT)`. The
exception would be if `foo` is a platform-independent script,
such as `help2man`.

Another context where such errors are common is when distributed files
are built by tools that are built by the package. The pattern is
similar:

```
distributed-file: built-tools distributed-sources
        build-command
```

should be changed to

```
distributed-file: distributed-sources
        $(MAKE) $(AM_MAKEFLAGS) built-tools
        build-command
```

or you could choose not to distribute `distributed-file`, if
cross-compilation does not matter.

The points made through these examples are worth summarizing:

|  |
| --- |
| * Distributed files should never depend upon non-distributed built files. * Distributed files should be distributed with all their dependencies. * If a file is *intended* to be rebuilt by users, then there   is no point in distributing it. |

Real-world examples for `help2man`:

* Autoconf takes the approach described above, including man pages in
  its releases. A wrapper for each script is needed to avoid concurrency
  problems. See its source file `autoconf/man/local.mk`, which has
  a good discussion of the necessary additional details.
* Automake itself takes another approach: it does *not* include man
  pages in distributions; thus, every user generates them when building
  from the release tarballs. This is ok (only) because Automake also
  includes a copy of the `help2man` script, which is plausible
  because `help2man` is small, self-contained, and
  platform-independent. See the source file
  `automake/doc/local.mk`.

If you’re desperate, it’s possible to disable this check completely by
setting `distcleancheck_listfiles` (see [`distcleancheck`](#distcleancheck)).
Make sure you understand the reason why ‘`make distcheck`’
complains first. `distcleancheck_listfiles` is a way to
*hide* errors, not to fix them. You can always do better.

---

Previous: [Checking the Distribution](#Checking-the-Distribution), Up: [What Goes in a Distribution](#Dist)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 14.5 The Types of Distributions [¶](#The-Types-of-Distributions-1)

Automake generates rules to provide archives of the project for
distributions in various formats. Their targets are:

`dist-gzip` [¶](#index-dist_002dgzip)
:   Generate a ‘`gzip`’ tar archive of the distribution. This is the
    only format enabled by default. By default, this rule makes
    `gzip` use a compression option of `-9` (more widely
    supported than `--best`). To make
    it use a different one, set the `GZIP_ENV` environment variable.
    For example, ‘`make dist-gzip GZIP_ENV=-7`’. `GZIP_ENV` is not
    used when decompressing.

`dist-bzip2` [¶](#index-dist_002dbzip2-1)
:   Generate a ‘`bzip2`’ tar archive of the distribution. bzip2
    archives are usually smaller than gzipped archives. By default, this
    rule makes ‘`bzip2`’ use a compression option of `-9`. To
    make it use a different one, set the `BZIP2` environment variable.

`dist-bzip3` [¶](#index-dist_002dbzip3-1)
:   Generate a ‘`bzip3`’ tar archive of the distribution. Unlike the
    other compression programs here, `bzip3` does not read any
    environment variables. To build this only when `bzip3` is
    present, see `AM_OPTIONAL_AUTOMAKE` (see [Public Macros](#Public-Macros)).

`dist-lzip` [¶](#index-dist_002dlzip-1)
:   Generate an ‘`lzip`’ tar archive of the distribution.
    `lzip` archives are usually smaller than
    `bzip2`-compressed archives. By default, this rule makes
    ‘`lzip`’ use a compression option of `-9`. To make it use a
    different one, set the `LZIP_OPT` environment variable. To build
    this only when `lzip` is present, see
    `AM_OPTIONAL_AUTOMAKE` (see [Public Macros](#Public-Macros)).

`dist-xz` [¶](#index-dist_002dxz-1)
:   Generate an ‘`xz`’ tar archive of the distribution. `xz`
    archives are usually smaller than `bzip2`-compressed archives.
    By default, this rule runs `xz` with the compression option of
    `-e` (`--extreme`). To use a different option, set the
    `XZ_OPT` environment variable. For example, run this command to
    use the default compression ratio, but with a progress indicator:
    ‘`make dist-xz XZ_OPT=-ve`’. Many packages use `-8e`, since
    that requires far less memory than `-9`, yet the compression
    improvement is tiny.

    Another possibly useful `xz` option is `-TO` to use as
    many threads as available cores for both compression and (more
    importantly) decompression; this is not the default because it changes
    the file format as saved, and would not be reproducible on systems
    with a different number of cores. Using `-T1`, forcing one
    thread for compression, would still speed up decompression and
    eliminate the system dependency, but this whole feature also depends
    on having a new-enough `xz`, which we do not want to require.

`dist-zip` [¶](#index-dist_002dzip-1)
:   Generate a ‘`zip`’ archive of the distribution.

`dist-zstd` [¶](#index-dist_002dzstd-1)
:   Generate a `zstd` tar archive of the distribution. By default,
    this rule makes `zstd` use a compression option of
    `-19`. To use a different setting, set the `ZSTD_OPT`
    environment variable. For example, run this command to use the
    default compression ratio, but with a progress indicator: ‘`make
    dist-zstd ZSTD_OPT=-19v`’. However, note that for compatibility with
    `zstd` itself, you may instead set the `ZSTD_CLEVEL`
    environment variable, in which case, any `ZSTD_OPT` setting is
    ignored. To build this only when `zstd` is present, see
    `AM_OPTIONAL_AUTOMAKE` (see [Public Macros](#Public-Macros)).

`dist-shar` [¶](#index-dist_002dshar-1)
:   Generate a ‘`shar`’ archive of the distribution. This format
    archive is obsolescent, and use of this option is deprecated.
    It and the corresponding functionality will be removed altogether
    in Automake 2.0.

`dist-tarZ` [¶](#index-dist_002dtarZ-1)
:   Generate a tar archive of the distribution, compressed with the
    historical (and obsolescent) program `compress`. This
    option is deprecated, and it and the corresponding functionality
    will be removed altogether in Automake 2.0.

The rule `dist` (and its historical synonym `dist-all`)
will create archives in all the enabled formats (see [List of Automake options](#List-of-Automake-options) for how to change this list). By default, only
the `dist-gzip` target is enabled by `dist`.

All the tar-based formats above compress one and the same temporary
uncompressed tar file, which is created once, however many formats are
requested, and removed once the archives are built. Beware that this
thus requires room for the uncompressed tarball, on top of the
`package-version` directory. Automake used
to pipe `tar` straight into the compressors, which hid the
failures of `tar`, since a shell pipeline reports the exit
status of only its last command.

Some `tar` implementations only warn about the files they
cannot archive, for instance because their names are too long for the
selected format, and still exit successfully. For this reason,
anything `tar` writes to its standard error also makes
‘`make dist`’ fail. If your `tar` is known to write
harmless messages there, set `AM_DIST_TAR_IGNORE_STDERR` to
‘`yes`’, in `Makefile.am` or on the `make` command
line; its messages are then still printed, but no longer treated as
errors.

---

Next: [Rebuilding Makefiles](#Rebuilding), Previous: [What Goes in a Distribution](#Dist), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 15 Support for test suites [¶](#Support-for-test-suites)

Automake can generate code to handle two kinds of test suites. One is
based on integration with the `dejagnu` framework. The other
(and most used) form is based on the use of generic test scripts, and
its activation is triggered by the definition of the special `TESTS`
variable. This second form allows for various degrees of sophistication
and customization; in particular, it allows for concurrent execution
of test scripts, use of established test protocols such as TAP, and
definition of custom test drivers and test runners.

In either case, the testsuite is invoked via ‘`make check`’.

* [Generalities about Testing](#Generalities-about-Testing)
* [Simple Tests](#Simple-Tests)
* [Custom Test Drivers](#Custom-Test-Drivers)
* [Using the TAP test protocol](#Using-the-TAP-test-protocol)
* [DejaGnu Tests](#DejaGnu-Tests)
* [Install Tests](#Install-Tests)

---

Next: [Simple Tests](#Simple-Tests), Up: [Support for test suites](#Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 15.1 Generalities about Testing [¶](#Generalities-about-Testing-1)

The purpose of testing is to determine whether a program or system behaves
as expected (e.g., known inputs produce the expected outputs, error
conditions are correctly handled or reported, and older bugs do not
resurface).

The minimal unit of testing is usually called *test case*, or simply
*test*. How a test case is defined or delimited, and even what
exactly *constitutes* a test case, depends heavily on the testing
paradigm and/or framework in use, so we won’t attempt any more precise
definition. The set of the test cases for a given program or system
constitutes its *testsuite*.

A *test harness* (also *testsuite harness*) is a program or
software component that executes all (or part of) the defined test cases,
analyzes their outcomes, and reports or registers these outcomes
appropriately. Again, the details of how this is accomplished (and how
the developer and user can influence it or interface with it) varies
wildly, and we’ll attempt no precise definition.

A test is said to *pass* when it can determine that the condition or
behavior it means to verify holds, and is said to *fail* when it can
determine that such condition of behavior does *not* hold.

Sometimes, tests can rely on non-portable tools or prerequisites, or
simply make no sense on a given system (for example, a test checking a
Windows-specific feature makes no sense on a GNU/Linux system). In
this case, accordingly to the definition above, the tests can neither
be considered passed nor failed; instead, they are *skipped*,
that is, they are not run, or their result is in any case ignored for
what concerns the count of failures and successes. Skips are usually
explicitly reported though, so that the user will be aware that not
all of the testsuite has been run.

It’s not uncommon, especially during early development stages, that
some tests fail for known reasons, and that the developer doesn’t want
to tackle these failures immediately (this is especially true when the
failing tests deal with corner cases). In this situation, the better
policy is to declare that each of those failures is an *expected
failure* (or *xfail*). When writing such a test, it is highly
desirable to note in the test what has to happen to make the test
succeed. In case a test that is expected to fail ends up passing
instead, many testing environments will flag the result as a special
kind of failure called *unexpected pass* (or *xpass*).

An expected failure result is not related to the contents of the test
itself. For example, a test might check whether a given system call
correctly returns failure given an invalid argument. If so, then the
*test* passes, even though it was testing a failure condition.

Many testing environments and frameworks distinguish between test failures
and hard errors. As we’ve seen, a test failure happens when some invariant
or expected behavior of the software under test is not met. A *hard
error* happens when e.g., the set-up of a test case scenario fails, or when
some other unexpected or highly undesirable condition is encountered (for
example, the program under test experiences a segmentation fault).

---

Next: [Custom Test Drivers](#Custom-Test-Drivers), Previous: [Generalities about Testing](#Generalities-about-Testing), Up: [Support for test suites](#Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 15.2 Simple Tests [¶](#Simple-Tests-1)

* [Scripts-based Testsuites](#Scripts_002dbased-Testsuites)
* [Older (and discouraged) Serial Test Harness](#Serial-Test-Harness)
* [Parallel Test Harness](#Parallel-Test-Harness)

---

Next: [Older (and discouraged) Serial Test Harness](#Serial-Test-Harness), Up: [Simple Tests](#Simple-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.2.1 Scripts-based Testsuites [¶](#Scripts_002dbased-Testsuites-1)

If the special variable `TESTS` is defined, its value is taken to be
a list of programs or scripts to run in order to do the testing. Under
the appropriate circumstances, it’s possible for `TESTS` to list
also data files to be passed to one or more test scripts defined by
different means (the so-called “log compilers”, see [Parallel Test Harness](#Parallel-Test-Harness)).

Test scripts can be executed serially or concurrently. Automake supports
both these kinds of test execution, with the parallel test harness being
the default. The concurrent test harness relies on the concurrence
capabilities (if any) offered by the underlying `make`
implementation, and can thus only be as good as those are.

By default, only the exit statuses of the test scripts are considered when
determining the testsuite outcome. But Automake allows also the use of
more complex test protocols, either standard (see [Using the TAP test protocol](#Using-the-TAP-test-protocol)) or custom (see [Custom Test Drivers](#Custom-Test-Drivers)). You can’t
enable such protocols when the serial harness is used, though.
In the rest of this section we are going to concentrate mostly on
protocol-less tests, since we cover test protocols in a later section
(again, see [Custom Test Drivers](#Custom-Test-Drivers)).

When no test protocol is in use, an exit status of 0 from a test script will
denote a success, an exit status of 77 a skipped test, an exit status of 99
a hard error, and any other exit status will denote a failure.

You may define the variable `XFAIL_TESTS` to a list of tests
(usually a subset of `TESTS`) that are expected to fail; this will
effectively reverse the result of those tests (with the provision that
skips and hard errors remain untouched). You may also instruct the
testsuite harness to treat hard errors like simple failures, by defining
the `DISABLE_HARD_ERRORS` make variable to a nonempty value.

Note however that, for tests based on more complex test protocols,
the exact effects of `XFAIL_TESTS` and `DISABLE_HARD_ERRORS`
might change, or they might even have no effect at all (for example,
in tests using TAP, there is no way to disable hard errors, and the
`DISABLE_HARD_ERRORS` variable has no effect on them).

The result of each test case run by the scripts in `TESTS` will be
printed on standard output, along with the test name. For test protocols
that allow more test cases per test script (such as TAP), a number,
identifier and/or brief description specific for the single test case is
expected to be printed in addition to the name of the test script. The
possible results (whose meanings should be clear from the previous
[Generalities about Testing](#Generalities-about-Testing)) are `PASS`, `FAIL`,
`SKIP`, `XFAIL`, `XPASS` and `ERROR`. Here is an
example of output from a hypothetical testsuite that uses both plain
and TAP tests:

```
PASS: foo.sh
PASS: zardoz.tap 1 - Daemon started
PASS: zardoz.tap 2 - Daemon responding
SKIP: zardoz.tap 3 - Daemon uses /proc # SKIP /proc is not mounted
PASS: zardoz.tap 4 - Daemon stopped
SKIP: bar.sh
PASS: mu.tap 1
XFAIL: mu.tap 2 # TODO frobnication not yet implemented
```

A testsuite summary (expected to report at least the number of run,
skipped and failed tests) will be printed at the end of the testsuite
run. By default, the first line of the summary has the form:

```
Testsuite summary for package-string
```

where package-string is the name and version of the package. If
you have several independent test suites for different parts of the
package, though, it can be misleading for each suite to imply it is
for the whole package. Or, in complex projects, you may wish to add
the current directory or other information to the testsuite header
line. So you can override the ‘ `for package-string`’ suffix
on that line by setting the `AM_TESTSUITE_SUMMARY_HEADER`
variable. The value of this variable is used unquoted in a shell echo
command, so you must include any necessary quotes. For example, the
default value is

```
AM_TESTSUITE_SUMMARY_HEADER = ' for $(PACKAGE_STRING)'
```

including the single quotes (interpreted by the shell) and the leading
space (since the value is output directly after the ‘`Testsuite
summary`’). The `$(PACKAGE_STRING)` is substituted by `make`.

If the standard output is connected to a capable terminal, then the test
results and the summary are colored appropriately. The developer and the
user can disable colored output by setting the `make` variable
‘`AM_COLOR_TESTS=no`’; the user can in addition force colored output
even without a connecting terminal with ‘`AM_COLOR_TESTS=always`’.
It’s also worth noting that some `make` implementations,
when used in parallel mode, have slightly different semantics
(see [Parallel make](https://www.gnu.org/software/autoconf/manual/autoconf.html#Parallel-make) in The Autoconf Manual), which can
break the automatic detection of a connection to a capable terminal.
If this is the case, the user will have to resort to the use of
‘`AM_COLOR_TESTS=always`’ in order to have the testsuite output
colorized.

Test programs that need data files should look for them in `srcdir`
(which is both a make variable and an environment variable made available
to the tests), so that they work when building in a separate directory
(see [Build Directories](https://www.gnu.org/software/autoconf/manual/autoconf.html#Build-Directories) in The Autoconf Manual), and in particular for the `distcheck` rule
(see [Checking the Distribution](#Checking-the-Distribution)).

Automake ensures that each file listed in `TESTS` is built before
it is run; you can list both source and derived programs (or scripts)
in `TESTS`; the generated rule will look both in `srcdir` and
‘`..`’. For instance, you might want to run a C program as a test.
To do this you would list its name in `TESTS` and also in
`check_PROGRAMS`, and then specify it as you would any other
program.

Programs listed in `check_PROGRAMS` (and `check_LIBRARIES`,
`check_LTLIBRARIES`, ...) are only built during `make
check`, not during `make all`. You should list there any program
needed by your tests that does not need to be built by `make
all`. The programs in `check_PROGRAMS` are *not*
automatically added to `TESTS` because `check_PROGRAMS`
usually lists programs used by the tests, not the tests themselves.
If all your programs are in fact test cases, you can set `TESTS =
$(check_PROGRAMS)`.

* [Testsuite Environment Overrides](#Testsuite-Environment-Overrides)

---

Up: [Scripts-based Testsuites](#Scripts_002dbased-Testsuites)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.2.1.1 Testsuite Environment Overrides [¶](#Testsuite-Environment-Overrides-1)

The `AM_TESTS_ENVIRONMENT` and `TESTS_ENVIRONMENT` variables can
be used to run initialization code and set environment variables for the
test scripts. The former variable is developer-reserved, and can be
defined in the `Makefile.am`, while the latter is reserved for the
user, which can employ it to extend or override the settings in the
former; for this to work portably, however, the contents of a non-empty
`AM_TESTS_ENVIRONMENT` *must* be terminated by a semicolon.

The `AM_TESTS_FD_REDIRECT` variable can be used to define file
descriptor redirections for the test scripts. One might think that
`AM_TESTS_ENVIRONMENT` could be used for this purpose, but experience
has shown that doing so portably is practically impossible. The main
hurdle is constituted by Korn shells, which usually set the close-on-exec
flag on file descriptors opened with the `exec` builtin, thus
rendering an idiom like `AM_TESTS_ENVIRONMENT = exec 9>&2;`
ineffectual. This issue also affects some Bourne shells, such as the
HP-UX’s `/bin/sh`.

```
AM_TESTS_ENVIRONMENT = \
## Some environment initializations are kept in a separate shell
## file 'tests-env.sh', which can make it easier to also run tests
## from the command line.
  . $(srcdir)/tests-env.sh; \
## On Solaris, prefer more POSIX-compliant versions of the standard
## tools by default.
  if test -d /usr/xpg4/bin; then \
    PATH=/usr/xpg4/bin:$$PATH; export PATH; \
  fi;

## With this, the test scripts will be able to print diagnostic
## messages to the original standard error stream, even if the test
## driver redirects the stderr of the test scripts to a log file
