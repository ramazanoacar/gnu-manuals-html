# the directory /usr/local/lib64 over /usr/local/lib.
test "$libdir" = '${exec_prefix}/lib' && libdir='${exec_prefix}/lib64'
```

---

Next: [config.status Invocation](#config_002estatus-Invocation), Previous: [Site Configuration](#Site-Configuration), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 16 Running `configure` Scripts [¶](#Running-configure-Scripts-1)

Below are instructions on how to configure a package that uses a
`configure` script, suitable for inclusion as an `INSTALL`
file in the package. A plain-text version of `INSTALL` which you
may use comes with Autoconf.

* [Basic Installation](#Basic-Installation)
* [Installation Prerequisites](#Installation-Prerequisites)
* [Compilers and Options](#Compilers-and-Options)
* [Compiling For Multiple Architectures](#Multiple-Architectures)
* [Installation Names](#Installation-Names)
* [Optional Features](#Optional-Features)
* [Specifying a System Type](#System-Types)
* [Sharing Defaults](#Sharing-Defaults)
* [Defining Variables](#Defining-Variables)
* [`configure` Invocation](#configure-Invocation)

---

Next: [Installation Prerequisites](#Installation-Prerequisites), Up: [Running `configure` Scripts](#Running-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 16.1 Basic Installation [¶](#Basic-Installation-1)

The following shell commands:

```
test -f configure || ./bootstrap
./configure
make
make install
```

should configure, build, and install this package.
The first line, which bootstraps, is intended for developers;
when building from distribution tarballs it does nothing and can be skipped.
A package might name the bootstrapping script differently;
if the name is `autogen.sh`, for example, the first line should say
`./autogen.sh` instead of `./bootstrap`.

The following
more-detailed instructions are generic; see the `README` file for
instructions specific to this package.
More recommendations for GNU packages can be found in
[Makefile Conventions](https://www.gnu.org/prep/standards/standards.html#Makefile-Conventions) in GNU Coding Standards.

Many packages have scripts meant for developers instead of ordinary
builders, as they may use developer tools that are less commonly installed,
or they may access the network, which has privacy implications.
These scripts attempt to bootstrap by building the
`configure` script and related files, possibly
using developer tools or the network. Because the output of
bootstrapping is system-independent, it is normally run by a
package developer so that its output can be put into the distribution
tarball and ordinary builders and users need not bootstrap.
Some packages have commands like `./autopull.sh` and
`./autogen.sh` that you can run instead of `./bootstrap`,
for more fine-grained control over bootstrapping.

The `configure` script attempts to guess correct values
for various system-dependent variables used during compilation. It uses
those values to create a `Makefile` in each directory of the
package. It may also create one or more `.h` files containing
system-dependent definitions. Finally, it creates a script
`config.status` that you can run in the future to recreate the
current configuration, and a file `config.log` containing
output useful for debugging `configure`.

It can also use an optional file (typically called `config.cache`
and enabled with `--cache-file=config.cache` or simply
`-C`) that saves the results of its tests to speed up
reconfiguring. Caching is disabled by default to prevent problems with
accidental use of stale cache files.

If you need to do unusual things to compile the package, please try to
figure out how `configure` could check whether to do them, and
mail diffs or instructions to the address given in the `README` so
they can be considered for the next release. If you are using the
cache, and at some point `config.cache` contains results you don’t
want to keep, you may remove or edit it.

The `autoconf` program generates `configure` from the file
`configure.ac`. Normally you should edit `configure.ac`
instead of editing `configure` directly.

The simplest way to compile this package is:

1. `cd` to the directory containing the package’s source code.
2. If this is a developer checkout and file `configure` does not yet exist,
   run the bootstrapping script (typically `./bootstrap` or
   `./autogen.sh`) to bootstrap and create the file.
   You may need special developer tools and network access to bootstrap,
   and the network access may have privacy implications.
3. Type ‘`./configure`’ to configure the package for your system.
   This might take a while. While running, `configure` prints
   messages telling which features it is checking for.
4. Type ‘`make`’ to compile the package.
5. Optionally, type ‘`make check`’ to run any self-tests that come with
   the package, generally using the just-built uninstalled binaries.
6. Type ‘`make install`’ to install the programs and any data files and
   documentation. When installing into a prefix owned by root, it is
   recommended that the package be configured and built as a regular user,
   and only the ‘`make install`’ phase executed with root privileges.
7. Optionally, type ‘`make installcheck`’ to repeat any self-tests, but
   this time using the binaries in their final installed location. This
   target does not install anything. Running this target as a regular
   user, particularly if the prior ‘`make install`’ required root
   privileges, verifies that the installation completed correctly.
8. You can remove the program binaries and object files from the source
   code directory by typing ‘`make clean`’. To also remove the files
   that `configure` created (so you can compile the package for a
   different kind of computer), type ‘`make distclean`’. There is also
   a ‘`make maintainer-clean`’ target, but that is intended mainly for
   the package’s developers. If you use it, you may have to bootstrap again.
9. If the package follows the GNU Coding Standards,
   you can type ‘`make uninstall`’ to remove the installed files.

---

Next: [Compilers and Options](#Compilers-and-Options), Previous: [Basic Installation](#Basic-Installation), Up: [Running `configure` Scripts](#Running-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 16.2 Installation Prerequisites [¶](#Installation-Prerequisites-1)

Installation requires a POSIX-like environment
with a shell and at least the following standard utilities:

> `awk cat cp diff echo expr false
> ls mkdir mv printf pwd
> rm rmdir sed sort test tr`

This package’s installation may need other standard utilities such as
`grep`, `make`, `sleep` and `touch`,
along with compilers like `gcc`.

---

Next: [Compiling For Multiple Architectures](#Multiple-Architectures), Previous: [Installation Prerequisites](#Installation-Prerequisites), Up: [Running `configure` Scripts](#Running-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 16.3 Compilers and Options [¶](#Compilers-and-Options-1)

Some systems require unusual options for compilation or linking that the
`configure` script does not know about. Run ‘`./configure
--help`’ for details on some of the pertinent environment variables.

You can give `configure` initial values for configuration
parameters by setting variables in the command line or in the environment.
Here is an example:

```
./configure CC=gcc CFLAGS=-g LIBS=-lposix
```

[Defining Variables](#Defining-Variables)
and [Preset Output Variables](#Preset-Output-Variables)
for more details.

---

Next: [Installation Names](#Installation-Names), Previous: [Compilers and Options](#Compilers-and-Options), Up: [Running `configure` Scripts](#Running-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 16.4 Compiling For Multiple Architectures [¶](#Compiling-For-Multiple-Architectures)

You can compile the package for more than one kind of computer at the
same time, by placing the object files for each system in their
own directory. To do this, you can use GNU `make`.
`cd` to the directory where you want the object files and
executables to go and run the `configure` script.
`configure` automatically checks for the source code in the
directory that `configure` is in and in `..`. This is
known as a *VPATH* build.

With a non-GNU `make`,
it is safer to compile the package for one
system at a time in the source code directory. After you have
installed the package for one system, use ‘`make distclean`’
before reconfiguring for another system.

Some platforms, notably macOS, support “fat” or “universal” binaries,
where a single binary can execute on different architectures.
On these platforms you can configure and compile just once,
with options specific to that platform.

---

Next: [Optional Features](#Optional-Features), Previous: [Compiling For Multiple Architectures](#Multiple-Architectures), Up: [Running `configure` Scripts](#Running-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 16.5 Installation Names [¶](#Installation-Names-1)

By default, ‘`make install`’ installs the package’s commands under
`/usr/local/bin`, include files under `/usr/local/include`, etc.
You can specify an
installation prefix other than `/usr/local` by giving
`configure` the option `--prefix=prefix`, where
prefix must be an absolute file name.

You can specify separate installation prefixes for architecture-specific
files and architecture-independent files. If you pass the option
`--exec-prefix=prefix` to `configure`, the
package uses prefix as the prefix for installing programs and
libraries. Documentation and other data files still use the
regular prefix.

In addition, if you use an unusual directory layout you can give options
like `--bindir=dir` to specify different values for
particular kinds of files. Run ‘`configure --help`’ for a list of
the directories you can set and what kinds of files go in them. In
general, the default for these options is expressed in terms of
‘`${prefix}`’, so that specifying just `--prefix` will
affect all of the other directory specifications that were not
explicitly provided.

The most portable way to affect installation locations is to pass the
correct locations to `configure`; however, many packages provide
one or both of the following shortcuts of passing variable assignments
to the ‘`make install`’ command line to change installation locations
without having to reconfigure or recompile.

The first method involves providing an override variable for each
affected directory. For example, ‘`make install
prefix=/alternate/directory`’ will choose an alternate location for all
directory configuration variables that were expressed in terms of
‘`${prefix}`’. Any directories that were specified during
`configure`, but not in terms of ‘`${prefix}`’, must each be
overridden at install time for the entire
installation to be relocated. The approach of makefile variable
overrides for each directory variable is required by the GNU
Coding Standards, and ideally causes no recompilation. However, some
platforms have known limitations with the semantics of shared libraries
that end up requiring recompilation when using this method, particularly
noticeable in packages that use GNU Libtool.

The second method involves providing the ‘`DESTDIR`’ variable. For
example, ‘`make install DESTDIR=/alternate/directory`’ will prepend
‘`/alternate/directory`’ before all installation names. The approach
of ‘`DESTDIR`’ overrides is not required by the GNU Coding
Standards, and does not work on platforms that have drive letters. On
the other hand, it does better at avoiding recompilation issues, and
works well even when some directory options were not specified in terms
of ‘`${prefix}`’ at `configure` time.

---

Next: [Specifying a System Type](#System-Types), Previous: [Installation Names](#Installation-Names), Up: [Running `configure` Scripts](#Running-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 16.6 Optional Features [¶](#Optional-Features-1)

If the package supports it, you can cause programs to be installed with
an extra prefix or suffix on their names by giving `configure`
the option `--program-prefix=PREFIX` or
`--program-suffix=SUFFIX`.

Some packages pay attention to `--enable-feature`
and `--disable-feature` options
to `configure`, where feature indicates an optional part
of the package. They may also pay attention to
`--with-package` and `--without-package` options,
where package is something like ‘`gnu-ld`’.
‘`./configure --help`’ should mention the
`--enable-...` and `--with-...`
options that the package recognizes.

Some packages offer the ability to configure how verbose the execution
of `make` will be. For these packages, running
‘`./configure --enable-silent-rules`’ sets the default to minimal
output, which can be overridden with `make V=1`; while running
‘`./configure --disable-silent-rules`’ sets the default to verbose,
which can be overridden with `make V=0`.

---

Next: [Sharing Defaults](#Sharing-Defaults), Previous: [Optional Features](#Optional-Features), Up: [Running `configure` Scripts](#Running-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 16.7 Specifying a System Type [¶](#Specifying-a-System-Type)

By default `configure` builds for the current system.
To create binaries that can run on a different system type,
specify a `--host=type` option along with compiler
variables that specify how to generate object code for type.
For example, to create binaries intended to run on a 64-bit ARM
processor:

```
./configure --host=aarch64-linux-gnu \
   CC=aarch64-linux-gnu-gcc \
   CXX=aarch64-linux-gnu-g++
```

If done on a machine that can execute these binaries
(e.g., via `qemu-aarch64`, `$QEMU_LD_PREFIX`, and Linux’s
`binfmt_misc` capability), the build behaves like a native build.
Otherwise it is a cross-build: `configure`
will make cross-compilation guesses instead of running test programs,
and `make check` will not work.

A system type can either be a short name like ‘`mingw64`’,
or a canonical name like ‘`x86_64-pc-linux-gnu`’.
Canonical names have the form cpu-company-system
where system is either os or kernel-os.
To canonicalize and validate a system type,
you can run the command `config.sub`,
which is often squirreled away in a subdirectory like `build-aux`.
For example:

```
$ build-aux/config.sub arm64-linux
aarch64-unknown-linux-gnu
$ build-aux/config.sub riscv-lnx
Invalid configuration 'riscv-lnx': OS 'lnx' not recognized
```

You can look at the `config.sub` file to see which types are recognized.
If the file is absent, this package does not need the system type.

If `configure` fails with the diagnostic “cannot guess build type”.
`config.sub` did not recognize your system’s type.
In this case, first fetch the newest versions of these files
from the [GNU config package](https://savannah.gnu.org/projects/config).
If that fixes things, please report it to the
maintainers of the package containing `configure`.
Otherwise, you can try the configure option
`--build=type` where type comes close to your
system type; also, please report the problem to
[config-patches@gnu.org](mailto:config-patches@gnu.org).

For more details about configuring system types, see
[Manual Configuration](#Manual-Configuration).

---

Next: [Defining Variables](#Defining-Variables), Previous: [Specifying a System Type](#System-Types), Up: [Running `configure` Scripts](#Running-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 16.8 Sharing Defaults [¶](#Sharing-Defaults-1)

If you want to set default values for `configure` scripts to
share, you can create a site shell script called `config.site` that
gives default values for variables like `CC`, `cache_file`,
and `prefix`. `configure` looks for
`prefix/share/config.site` if it exists, then
`prefix/etc/config.site` if it exists. Or, you can set the
`CONFIG_SITE` environment variable to the location of the site
script. A warning: not all `configure` scripts look for a site
script.

---

Next: [`configure` Invocation](#configure-Invocation), Previous: [Sharing Defaults](#Sharing-Defaults), Up: [Running `configure` Scripts](#Running-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 16.9 Defining Variables [¶](#Defining-Variables-1)

Variables not defined in a site shell script can be set in the
environment passed to `configure`. However, some packages may
run configure again during the build, and the customized values of these
variables may be lost. In order to avoid this problem, you should set
them in the `configure` command line, using ‘`VAR=value`’.
For example:

```
./configure CC=/usr/local2/bin/gcc
```

causes the specified `gcc` to be used as the C compiler (unless it is
overridden in the site shell script).

Unfortunately, this technique does not work for `CONFIG_SHELL` due
to an Autoconf limitation. Until the limitation is lifted, you can use
this workaround:

```
CONFIG_SHELL=/bin/bash ./configure CONFIG_SHELL=/bin/bash
```

---

Previous: [Defining Variables](#Defining-Variables), Up: [Running `configure` Scripts](#Running-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 16.10 `configure` Invocation [¶](#configure-Invocation-1)

`configure` recognizes the following options to control how it
operates.

`--help`

`-h`
:   Print a summary of all of the options to `configure`, and exit.

`--help=short`

`--help=recursive`
:   Print a summary of the options unique to this package’s
    `configure`, and exit. The `short` variant lists options
    used only in the top level, while the `recursive` variant lists
    options also present in any nested packages.

`--version`

`-V`
:   Print the version of Autoconf used to generate the `configure`
    script, and exit.

`--cache-file=file` [¶](#index-Cache_002c-enabling)
:   Enable the cache: use and save the results of the tests in file,
    traditionally `config.cache`. file defaults to
    `/dev/null` to disable caching.

`--config-cache`

`-C`
:   Alias for `--cache-file=config.cache`.

`--srcdir=dir`
:   Look for the package’s source code in directory dir. Usually
    `configure` can determine that directory automatically.

`--prefix=dir`
:   Use dir as the installation prefix.
    [Installation Names](#Installation-Names)
    for more details, including other options available for fine-tuning
    the installation locations.

`--host=type`
:   Build binaries for system type.
    See [Specifying a System Type](#System-Types).

`--enable-feature`

`--disable-feature`
:   Enable or disable the optional feature.
    See [Optional Features](#Optional-Features).

`--with-package`

`--without-package`
:   Use or omit package when building.
    See [Optional Features](#Optional-Features).

`--quiet`

`--silent`

`-q`
:   Do not print messages saying which checks are being made. To suppress
    all normal output, redirect it to `/dev/null` (any error messages
    will still be shown).

`--no-create`

`-n`
:   Run the configure checks, but stop before creating any output files.

`configure` also recognizes several environment variables,
and accepts some other, less widely useful, options.
Run ‘`configure --help`’ for more details.

---

Next: [Obsolete Constructs](#Obsolete-Constructs), Previous: [Running `configure` Scripts](#Running-configure-Scripts), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 17 config.status Invocation [¶](#config_002estatus-Invocation-1)

The `configure` script creates a file named `config.status`,
which actually configures, *instantiates*, the template files. It
also records the configuration options that were specified when the
package was last configured in case reconfiguring is needed.

Synopsis:

```
./config.status [option]... [tag]...
```

It configures each tag; if none are specified, all the templates
are instantiated. A tag refers to a file or other tag associated
with a configuration action, as specified by an `AC_CONFIG_ITEMS`
macro (see [Performing Configuration Actions](#Configuration-Actions)). The files must be specified
without their dependencies, as in

```
./config.status foobar
```

not

```
./config.status foobar:foo.in:bar.in
```

The supported options are:

`--help`

`-h`
:   Print a summary of the command line options, the list of the template
    files, and exit.

`--version`

`-V`
:   Print the version number of Autoconf and the configuration settings,
    and exit.

`--config`
:   Print the configuration settings in reusable way, quoted for the shell,
    and exit. For example, for a debugging build that otherwise reuses the
    configuration from a different build directory build-dir of a
    package in src-dir, you could use the following:

    ```
    args=`build-dir/config.status --config`
    eval src-dir/configure "$args" CFLAGS=-g --srcdir=src-dir
    ```

    Note that it may be necessary to override a `--srcdir` setting
    that was saved in the configuration, if the arguments are used in a
    different build directory.

`--silent`

`--quiet`

`-q`
:   Do not print progress messages.

`--debug`

`-d`
:   Don’t remove the temporary files.

`--file=file[:template]`
:   Require that file be instantiated as if
    ‘`AC_CONFIG_FILES(file:template)`’ was used. Both
    file and template may be ‘`-`’ in which case the standard
    output and/or standard input, respectively, is used. If a
    template file name is relative, it is first looked for in the build
    tree, and then in the source tree. See [Performing Configuration Actions](#Configuration-Actions), for
    more details.

    This option and the following ones provide one way for separately
    distributed packages to share the values computed by `configure`.
    Doing so can be useful if some of the packages need a superset of the
    features that one of them, perhaps a common library, does. These
    options allow a `config.status` file to create files other than the
    ones that its `configure.ac` specifies, so it can be used for a
    different package, or for extracting a subset of values. For example,

    ```
    echo '@CC@' | ./config.status --file=-
    ```

    provides the value of `@CC@` on standard output.

`--header=file[:template]`
:   Same as `--file` above, but with ‘`AC_CONFIG_HEADERS`’.

`--recheck`
:   Ask `config.status` to update itself and exit (no instantiation).
    This option is useful if you change `configure`, so that the
    results of some tests might be different from the previous run. The
    `--recheck` option reruns `configure` with the same arguments
    you used before, plus the `--no-create` option, which prevents
    `configure` from running `config.status` and creating
    `Makefile` and other files, and the `--no-recursion` option,
    which prevents `configure` from running other `configure`
    scripts in subdirectories. (This is so other Make rules can
    run `config.status` when it changes; see [Automatic Remaking](#Automatic-Remaking),
    for an example).

`config.status` checks several optional environment variables that
can alter its behavior:

Variable: **CONFIG\_SHELL** [¶](#index-CONFIG_005fSHELL-1)
:   The shell with which to run `configure`. It must be
    Bourne-compatible, and the absolute name of the shell should be passed.
    The default is a shell that supports `LINENO` if available, and
    `/bin/sh` otherwise.

Variable: **CONFIG\_STATUS** [¶](#index-CONFIG_005fSTATUS-1)
:   The file name to use for the shell script that records the
    configuration. The default is `./config.status`. This variable is
    useful when one package uses parts of another and the `configure`
    scripts shouldn’t be merged because they are maintained separately.

You can use `./config.status` in your makefiles. For example, in
the dependencies given above (see [Automatic Remaking](#Automatic-Remaking)),
`config.status` is run twice when `configure.ac` has changed.
If that bothers you, you can make each run only regenerate the files for
that rule:

```
config.h: stamp-h
stamp-h: config.h.in config.status
        ./config.status config.h
        echo > stamp-h

Makefile: Makefile.in config.status
        ./config.status Makefile
```

The calling convention of `config.status` has changed; see
[Obsolete `config.status` Invocation](#Obsolete-config_002estatus-Use), for details.

---

Next: [Generating Test Suites with Autotest](#Using-Autotest), Previous: [config.status Invocation](#config_002estatus-Invocation), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 18 Obsolete Constructs [¶](#Obsolete-Constructs-1)

Autoconf changes, and throughout the years some constructs have been
obsoleted. Most of the changes involve the macros, but in some cases
the tools themselves, or even some concepts, are now considered
obsolete.

You may completely skip this chapter if you are new to Autoconf. Its
intention is mainly to help maintainers updating their packages by
understanding how to move to more modern constructs.

* [Obsolete `config.status` Invocation](#Obsolete-config_002estatus-Use)
* [`acconfig.h`](#acconfig-Header)
* [Using `autoupdate` to Modernize `configure.ac`](#autoupdate-Invocation)
* [Obsolete Macros](#Obsolete-Macros)
* [Upgrading From Version 1](#Autoconf-1)
* [Upgrading From Version 2.13](#Autoconf-2_002e13)

---

Next: [`acconfig.h`](#acconfig-Header), Up: [Obsolete Constructs](#Obsolete-Constructs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 18.1 Obsolete `config.status` Invocation [¶](#Obsolete-config_002estatus-Invocation)

`config.status` now supports arguments to specify the files to
instantiate; see [config.status Invocation](#config_002estatus-Invocation), for more details.
Before, environment variables had to be used.

Variable: **CONFIG\_COMMANDS** [¶](#index-CONFIG_005fCOMMANDS-1)
:   The tags of the commands to execute. The default is the arguments given
    to `AC_OUTPUT` and `AC_CONFIG_COMMANDS` in
    `configure.ac`.

Variable: **CONFIG\_FILES** [¶](#index-CONFIG_005fFILES-1)
:   The files in which to perform ‘`@variable@`’ substitutions.
    The default is the arguments given to `AC_OUTPUT` and
    `AC_CONFIG_FILES` in `configure.ac`.

Variable: **CONFIG\_HEADERS** [¶](#index-CONFIG_005fHEADERS-1)
:   The files in which to substitute C `#define` statements. The
    default is the arguments given to `AC_CONFIG_HEADERS`; if that
    macro was not called, `config.status` ignores this variable.

Variable: **CONFIG\_LINKS** [¶](#index-CONFIG_005fLINKS-1)
:   The symbolic links to establish. The default is the arguments given to
    `AC_CONFIG_LINKS`; if that macro was not called,
    `config.status` ignores this variable.

In [config.status Invocation](#config_002estatus-Invocation), using this old interface, the example
would be:

```
config.h: stamp-h
stamp-h: config.h.in config.status
        CONFIG_COMMANDS= CONFIG_LINKS= CONFIG_FILES= \
          CONFIG_HEADERS=config.h ./config.status
        echo > stamp-h

Makefile: Makefile.in config.status
        CONFIG_COMMANDS= CONFIG_LINKS= CONFIG_HEADERS= \
          CONFIG_FILES=Makefile ./config.status
```

(If `configure.ac` does not call `AC_CONFIG_HEADERS`, there is
no need to set `CONFIG_HEADERS` in the `make` rules. Equally
for `CONFIG_COMMANDS`, etc.)

---

Next: [Using `autoupdate` to Modernize `configure.ac`](#autoupdate-Invocation), Previous: [Obsolete `config.status` Invocation](#Obsolete-config_002estatus-Use), Up: [Obsolete Constructs](#Obsolete-Constructs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 18.2 `acconfig.h` [¶](#acconfig_002eh)

In order to produce `config.h.in`, `autoheader` needs to
build or to find templates for each symbol. Modern releases of Autoconf
use `AH_VERBATIM` and `AH_TEMPLATE` (see [Autoheader Macros](#Autoheader-Macros)), but in older releases a file, `acconfig.h`, contained the
list of needed templates. `autoheader` copied comments and
`#define` and `#undef` statements from `acconfig.h` in
the current directory, if present. This file used to be mandatory if
you `AC_DEFINE` any additional symbols.

Modern releases of Autoconf also provide `AH_TOP` and
`AH_BOTTOM` if you need to prepend/append some information to
`config.h.in`. Ancient versions of Autoconf had a similar feature:
if `./acconfig.h` contains the string ‘`@TOP@`’,
`autoheader` copies the lines before the line containing
‘`@TOP@`’ into the top of the file that it generates. Similarly,
if `./acconfig.h` contains the string ‘`@BOTTOM@`’,
`autoheader` copies the lines after that line to the end of the
file it generates. Either or both of those strings may be omitted. An
even older alternate way to produce the same effect in ancient versions
of Autoconf is to create the files `file.top` (typically
`config.h.top`) and/or `file.bot` in the current
directory. If they exist, `autoheader` copies them to the
beginning and end, respectively, of its output.

In former versions of Autoconf, the files used in preparing a software
package for distribution were:

```
configure.ac --.   .------> autoconf* -----> configure
               +---+
[aclocal.m4] --+   `---.
[acsite.m4] ---'       |
                       +--> [autoheader*] -> [config.h.in]
[acconfig.h] ----.     |
                 +-----'
[config.h.top] --+
[config.h.bot] --'
```

Using only the `AH_` macros, `configure.ac` should be
self-contained, and should not depend upon `acconfig.h` etc.

---

Next: [Obsolete Macros](#Obsolete-Macros), Previous: [`acconfig.h`](#acconfig-Header), Up: [Obsolete Constructs](#Obsolete-Constructs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 18.3 Using `autoupdate` to Modernize `configure.ac` [¶](#Using-autoupdate-to-Modernize-configure_002eac)

The `autoupdate` program updates a `configure.ac` file that
calls Autoconf macros by their old names to use the current macro names.
In version 2 of Autoconf, most of the macros were renamed to use a more
uniform and descriptive naming scheme. See [Macro Names](#Macro-Names), for a
description of the new scheme. Although the old names still work
(see [Obsolete Macros](#Obsolete-Macros), for a list of the old macros and the corresponding
new names), you can make your `configure.ac` files more readable
and make it easier to use the current Autoconf documentation if you
update them to use the new macro names.

If given no arguments, `autoupdate` updates `configure.ac`,
backing up the original version with the suffix `~` (or the value
of the environment variable `SIMPLE_BACKUP_SUFFIX`, if that is
set). If you give `autoupdate` an argument, it reads that file
instead of `configure.ac` and writes the updated file to the
standard output.

`autoupdate` accepts the following options:

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
:   Force the update even if the file has not changed. Disregard the cache.

`--include=dir`

`-I dir`
:   Also look for input files in dir. Multiple invocations accumulate.
    Directories are browsed from last to first.

`--prepend-include=dir`

`-B dir`
:   Prepend directory dir to the search path. This is used to include
    the language-specific files before any third-party macros.

---

Next: [Upgrading From Version 1](#Autoconf-1), Previous: [Using `autoupdate` to Modernize `configure.ac`](#autoupdate-Invocation), Up: [Obsolete Constructs](#Obsolete-Constructs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 18.4 Obsolete Macros [¶](#Obsolete-Macros-1)

Several macros are obsoleted in Autoconf, for various reasons (typically
they failed to quote properly, couldn’t be extended for more recent
issues, etc.). They are still supported, but deprecated: their use
should be avoided.

During the jump from Autoconf version 1 to version 2, most of the
macros were renamed to use a more uniform and descriptive naming scheme,
but their signature did not change. See [Macro Names](#Macro-Names), for a
description of the new naming scheme. Below, if there is just the mapping
from old names to new names for these macros, the reader is invited to
refer to the definition of the new macro for the signature and the
description.

Macro: **AC\_AIX** [¶](#index-AC_005fAIX-1)
:   This macro is a platform-specific subset of
    `AC_USE_SYSTEM_EXTENSIONS` (see [AC\_USE\_SYSTEM\_EXTENSIONS](#AC_005fUSE_005fSYSTEM_005fEXTENSIONS)).

Macro: **AC\_ALLOCA** [¶](#index-AC_005fALLOCA-1)
:   Replaced by `AC_FUNC_ALLOCA` (see [AC\_FUNC\_ALLOCA](#AC_005fFUNC_005fALLOCA)).

Macro: **AC\_ARG\_ARRAY** [¶](#index-AC_005fARG_005fARRAY-1)
:   Removed because of limited usefulness.

Macro: **AC\_C\_CROSS** [¶](#index-AC_005fC_005fCROSS-1)
:   This macro is obsolete; it does nothing.

Macro: **AC\_C\_LONG\_DOUBLE** [¶](#index-AC_005fC_005fLONG_005fDOUBLE-1)
:   If the C compiler supports a working `long double` type with more
    range or precision than the `double` type, define
    `HAVE_LONG_DOUBLE`.

    You should use `AC_TYPE_LONG_DOUBLE` or
    `AC_TYPE_LONG_DOUBLE_WIDER` instead. See [Particular Type Checks](#Particular-Types).

Macro: **AC\_CANONICAL\_SYSTEM** [¶](#index-AC_005fCANONICAL_005fSYSTEM-1)
:   Determine the system type and set output variables to the names of the
    canonical system types. See [Getting the Canonical System Type](#Canonicalizing), for details about the
    variables this macro sets.

    The user is encouraged to use either `AC_CANONICAL_BUILD`, or
    `AC_CANONICAL_HOST`, or `AC_CANONICAL_TARGET`, depending on
    the needs. Using `AC_CANONICAL_TARGET` is enough to run the two
    other macros (see [Getting the Canonical System Type](#Canonicalizing)).

Macro: **AC\_CHAR\_UNSIGNED** [¶](#index-AC_005fCHAR_005fUNSIGNED-1)
:   Replaced by `AC_C_CHAR_UNSIGNED` (see [AC\_C\_CHAR\_UNSIGNED](#AC_005fC_005fCHAR_005fUNSIGNED)).

Macro: **AC\_CHECK\_TYPE**(type, default) [¶](#index-AC_005fCHECK_005fTYPE-3)
:   Autoconf, up to 2.13, used to provide this version of
    `AC_CHECK_TYPE`, deprecated because of its flaws. First, although
    it is a member of the `CHECK` clan, it does
    more than just checking. Secondly, missing types are defined
    using `#define`, not `typedef`, and this can lead to
    problems in the case of pointer types.

    This use of `AC_CHECK_TYPE` is obsolete and discouraged; see
    [Generic Type Checks](#Generic-Types), for the description of the current macro.

    If the type type is not defined, define it to be the C (or C++)
    builtin type default, e.g., ‘`short int`’ or ‘`unsigned int`’.

    This macro is equivalent to:

    ```
    AC_CHECK_TYPE([type], [],
      [AC_DEFINE_UNQUOTED([type], [default],
         [Define to 'default'
          if <sys/types.h> does not define.])])
    ```

    In order to keep backward compatibility, the two versions of
    `AC_CHECK_TYPE` are implemented, selected using these heuristics:

    1. If there are three or four arguments, the modern version is used.
    2. If the second argument appears to be a C or C++ type, then the
       obsolete version is used. This happens if the argument is a C or C++
       *builtin* type or a C identifier ending in ‘`_t`’, optionally
       followed by one of ‘`[(*` ’ and then by a string of zero or more
       characters taken from the set ‘`[]()* _a-zA-Z0-9`’.
    3. If the second argument is spelled with the alphabet of valid C and C++
       types, the user is warned and the modern version is used.
    4. Otherwise, the modern version is used.

    You are encouraged either to use a valid builtin type, or to use the
    equivalent modern code (see above), or better yet, to use
    `AC_CHECK_TYPES` together with

    ```
    #ifndef HAVE_LOFF_T
    typedef loff_t off_t;
    #endif
    ```

Macro: **AC\_CHECKING**(feature-description) [¶](#index-AC_005fCHECKING-1)
:   Same as

    ```
    AC_MSG_NOTICE([checking feature-description...]
    ```

    See [AC\_MSG\_NOTICE](#AC_005fMSG_005fNOTICE).

Macro: **AC\_COMPILE\_CHECK**(echo-text, includes, function-body, action-if-true, [action-if-false]) [¶](#index-AC_005fCOMPILE_005fCHECK-1)
:   This is an obsolete version of `AC_TRY_COMPILE` itself replaced by
    `AC_COMPILE_IFELSE` (see [Running the Compiler](#Running-the-Compiler)), with the
    addition that it prints ‘`checking for echo-text`’ to the
    standard output first, if echo-text is non-empty. Use
    `AC_MSG_CHECKING` and `AC_MSG_RESULT` instead to print
    messages (see [Printing Messages](#Printing-Messages)).

Macro: **AC\_CONST** [¶](#index-AC_005fCONST-1)
:   Replaced by `AC_C_CONST` (see [AC\_C\_CONST](#AC_005fC_005fCONST)).

Macro: **AC\_CROSS\_CHECK** [¶](#index-AC_005fCROSS_005fCHECK-1)
:   Same as `AC_C_CROSS`, which is obsolete too, and does nothing
    `:-)`.

Macro: **AC\_CYGWIN** [¶](#index-AC_005fCYGWIN-1)
:   Check for the Cygwin environment in which case the shell variable
    `CYGWIN` is set to ‘`yes`’. Don’t use this macro, the dignified
    means to check the nature of the host is using `AC_CANONICAL_HOST`
    (see [Getting the Canonical System Type](#Canonicalizing)). As a matter of fact this macro is defined as:

    ```
    AC_REQUIRE([AC_CANONICAL_HOST])[]dnl
    case $host_os in
      *cygwin* ) CYGWIN=yes;;
             * ) CYGWIN=no;;
    esac
    ```

    Beware that the variable `CYGWIN` has a special meaning when
    running Cygwin, and should not be changed. That’s yet another reason
    not to use this macro.

Macro: **AC\_DECL\_SYS\_SIGLIST** [¶](#index-AC_005fDECL_005fSYS_005fSIGLIST-1)
:   Same as:

    ```
    AC_CHECK_DECLS([sys_siglist], [], [],
    [#include <signal.h>
    /* NetBSD declares sys_siglist in unistd.h.  */
    #ifdef HAVE_UNISTD_H
    # include <unistd.h>
    #endif
    ])
    ```

    See [AC\_CHECK\_DECLS](#AC_005fCHECK_005fDECLS).

Macro: **AC\_DECL\_YYTEXT** [¶](#index-AC_005fDECL_005fYYTEXT-1)
:   Does nothing, now integrated in `AC_PROG_LEX` (see [AC\_PROG\_LEX](#AC_005fPROG_005fLEX)).

Macro: **AC\_DIAGNOSE**(category, message) [¶](#index-AC_005fDIAGNOSE-1)
:   Replaced by `m4_warn` (see [m4\_warn](#m4_005fwarn)).

Macro: **AC\_DIR\_HEADER** [¶](#index-AC_005fDIR_005fHEADER-1)
:   Like calling `AC_FUNC_CLOSEDIR_VOID`
    (see [AC\_FUNC\_CLOSEDIR\_VOID](#AC_005fFUNC_005fCLOSEDIR_005fVOID)) and `AC_HEADER_DIRENT`
    (see [AC\_HEADER\_DIRENT](#AC_005fHEADER_005fDIRENT)),
    but defines a different set of C preprocessor macros to indicate which
    header file is found:

    |  |  |  |
    | --- | --- | --- |
    | Header | Old Symbol | New Symbol |
    | `dirent.h` | `DIRENT` | `HAVE_DIRENT_H` |
    | `sys/ndir.h` | `SYSNDIR` | `HAVE_SYS_NDIR_H` |
    | `sys/dir.h` | `SYSDIR` | `HAVE_SYS_DIR_H` |
    | `ndir.h` | `NDIR` | `HAVE_NDIR_H` |

Macro: **AC\_DYNIX\_SEQ** [¶](#index-AC_005fDYNIX_005fSEQ-1)
:   If on DYNIX/ptx, add `-lseq` to output variable
    `LIBS`. This macro used to be defined as

    ```
    AC_CHECK_LIB([seq], [getmntent], [LIBS="-lseq $LIBS"])
    ```

    now it is just `AC_FUNC_GETMNTENT` (see [AC\_FUNC\_GETMNTENT](#AC_005fFUNC_005fGETMNTENT)).

Macro: **AC\_EXEEXT** [¶](#index-AC_005fEXEEXT-1)
:   Defined the output variable `EXEEXT` based on the output of the
    compiler, which is now done automatically. Typically set to empty
    string if POSIX and ‘`.exe`’ if a DOS variant.

Macro: **AC\_EMXOS2** [¶](#index-AC_005fEMXOS2-1)
:   Similar to `AC_CYGWIN` but checks for the EMX environment on OS/2
    and sets `EMXOS2`. Don’t use this macro, the dignified means to
    check the nature of the host is using `AC_CANONICAL_HOST`
    (see [Getting the Canonical System Type](#Canonicalizing)).

Macro: **AC\_ENABLE**(feature, action-if-given, [action-if-not-given]) [¶](#index-AC_005fENABLE-1)
:   This is an obsolete version of `AC_ARG_ENABLE` that does not
    support providing a help string (see [AC\_ARG\_ENABLE](#AC_005fARG_005fENABLE)).

Macro: **AC\_ERROR** [¶](#index-AC_005fERROR-1)
:   Replaced by `AC_MSG_ERROR` (see [AC\_MSG\_ERROR](#AC_005fMSG_005fERROR)).

Macro: **AC\_FATAL**(message) [¶](#index-AC_005fFATAL-1)
:   Replaced by `m4_fatal` (see [m4\_fatal](#m4_005ffatal)).

Macro: **AC\_FIND\_X** [¶](#index-AC_005fFIND_005fX-1)
:   Replaced by `AC_PATH_X` (see [AC\_PATH\_X](#AC_005fPATH_005fX)).

Macro: **AC\_FIND\_XTRA** [¶](#index-AC_005fFIND_005fXTRA-1)
:   Replaced by `AC_PATH_XTRA` (see [AC\_PATH\_XTRA](#AC_005fPATH_005fXTRA)).

Macro: **AC\_FOREACH** [¶](#index-AC_005fFOREACH-1)
:   Replaced by `m4_foreach_w` (see [m4\_foreach\_w](#m4_005fforeach_005fw)).

Macro: **AC\_FUNC\_CHECK** [¶](#index-AC_005fFUNC_005fCHECK-1)
:   Replaced by `AC_CHECK_FUNC` (see [AC\_CHECK\_FUNC](#AC_005fCHECK_005fFUNC)).

Macro: **AC\_FUNC\_SETVBUF\_REVERSED** [¶](#index-AC_005fFUNC_005fSETVBUF_005fREVERSED-1)
:   Do nothing. Formerly, this macro checked whether `setvbuf` takes
    the buffering type as its second argument and the buffer pointer as the
    third, instead of the other way around, and defined
    `SETVBUF_REVERSED`. However, the last systems to have the problem
    were those based on SVR2, which became obsolete in 1987, and the macro
    is no longer needed.

Macro: **AC\_FUNC\_WAIT3** [¶](#index-AC_005fFUNC_005fWAIT3-1)
:   If `wait3` is found and fills in the contents of its third argument
    (a ‘`struct rusage *`’), which HP-UX does not do, define
    `HAVE_WAIT3`.

    These days portable programs should use `waitpid`, not
    `wait3`, as `wait3` has been removed from POSIX.

Macro: **AC\_GCC\_TRADITIONAL** [¶](#index-AC_005fGCC_005fTRADITIONAL-1)
:   Replaced by `AC_PROG_GCC_TRADITIONAL` (see [AC\_PROG\_GCC\_TRADITIONAL](#AC_005fPROG_005fGCC_005fTRADITIONAL)),
    which is itself obsolete.

Macro: **AC\_GETGROUPS\_T** [¶](#index-AC_005fGETGROUPS_005fT-1)
:   Replaced by `AC_TYPE_GETGROUPS` (see [AC\_TYPE\_GETGROUPS](#AC_005fTYPE_005fGETGROUPS)).

Macro: **AC\_GETLOADAVG** [¶](#index-AC_005fGETLOADAVG-1)
:   Replaced by `AC_FUNC_GETLOADAVG` (see [AC\_FUNC\_GETLOADAVG](#AC_005fFUNC_005fGETLOADAVG)).

Macro: **AC\_GNU\_SOURCE** [¶](#index-AC_005fGNU_005fSOURCE-1)
:   This macro is a platform-specific subset of
    `AC_USE_SYSTEM_EXTENSIONS` (see [AC\_USE\_SYSTEM\_EXTENSIONS](#AC_005fUSE_005fSYSTEM_005fEXTENSIONS)).

Macro: **AC\_HAVE\_FUNCS** [¶](#index-AC_005fHAVE_005fFUNCS-1)
:   Replaced by `AC_CHECK_FUNCS` (see [AC\_CHECK\_FUNCS](#AC_005fCHECK_005fFUNCS)).

Macro: **AC\_HAVE\_HEADERS** [¶](#index-AC_005fHAVE_005fHEADERS-1)
:   Replaced by `AC_CHECK_HEADERS` (see [AC\_CHECK\_HEADERS](#AC_005fCHECK_005fHEADERS)).

Macro: **AC\_HAVE\_LIBRARY**(library, [action-if-found], [action-if-not-found], [other-libraries]) [¶](#index-AC_005fHAVE_005fLIBRARY-1)
:   This macro is equivalent to calling `AC_CHECK_LIB` with a
    function argument of `main`. In addition, library can
    be written as any of ‘`foo`’, `-lfoo`, or ‘`libfoo.a`’. In
    all of those cases, the compiler is passed `-lfoo`. However,
    library cannot be a shell variable; it must be a literal name.
    See [AC\_CHECK\_LIB](#AC_005fCHECK_005fLIB).

Macro: **AC\_HAVE\_POUNDBANG** [¶](#index-AC_005fHAVE_005fPOUNDBANG-1)
:   Replaced by `AC_SYS_INTERPRETER` (see [AC\_SYS\_INTERPRETER](#AC_005fSYS_005fINTERPRETER)).

Macro: **AC\_HEADER\_CHECK** [¶](#index-AC_005fHEADER_005fCHECK-1)
:   Replaced by `AC_CHECK_HEADER` (see [AC\_CHECK\_HEADER](#AC_005fCHECK_005fHEADER)).

Macro: **AC\_HEADER\_EGREP** [¶](#index-AC_005fHEADER_005fEGREP-1)
:   Replaced by `AC_EGREP_HEADER` (see [AC\_EGREP\_HEADER](#AC_005fEGREP_005fHEADER)).

Macro: **AC\_HEADER\_TIME** [¶](#index-AC_005fHEADER_005fTIME-1)
:   This macro used to check whether it was possible to include
    `time.h` and `sys/time.h` in the same source file,
    defining `TIME_WITH_SYS_TIME` if so.

    Nowadays, it is equivalent to ‘`AC_CHECK_HEADERS([sys/time.h])`’,
    although it does still define `TIME_WITH_SYS_TIME` for
    compatibility’s sake. `time.h` is universally present, and the
    systems on which `sys/time.h` conflicted with `time.h` are
    obsolete.

Macro: **AC\_HELP\_STRING** [¶](#index-AC_005fHELP_005fSTRING-1)
:   Replaced by `AS_HELP_STRING` (see [AS\_HELP\_STRING](#AS_005fHELP_005fSTRING)).

Macro: **AC\_INIT**(unique-file-in-source-dir) [¶](#index-AC_005fINIT-3)
:   Formerly `AC_INIT` used to have a single argument, and was
    equivalent to:

    ```
    AC_INIT
    AC_CONFIG_SRCDIR(unique-file-in-source-dir)
    ```

    See [AC\_INIT](#AC_005fINIT) and [AC\_CONFIG\_SRCDIR](#AC_005fCONFIG_005fSRCDIR).

Macro: **AC\_INLINE** [¶](#index-AC_005fINLINE-1)
:   Replaced by `AC_C_INLINE` (see [AC\_C\_INLINE](#AC_005fC_005fINLINE)).

Macro: **AC\_INT\_16\_BITS** [¶](#index-AC_005fINT_005f16_005fBITS-1)
:   If the C type `int` is 16 bits wide, define `INT_16_BITS`.
    Use ‘`AC_CHECK_SIZEOF(int)`’ instead (see [AC\_CHECK\_SIZEOF](#AC_005fCHECK_005fSIZEOF)).

Macro: **AC\_IRIX\_SUN** [¶](#index-AC_005fIRIX_005fSUN-1)
:   If on IRIX (Silicon Graphics Unix), add `-lsun` to output
    `LIBS`. If you were using it to get `getmntent`, use
    `AC_FUNC_GETMNTENT` instead. If you used it for the NIS versions
    of the password and group functions, use ‘`AC_CHECK_LIB(sun,
    getpwnam)`’. Up to Autoconf 2.13, it used to be

    ```
    AC_CHECK_LIB([sun], [getmntent], [LIBS="-lsun $LIBS"])
    ```

    now it is defined as

    ```
    AC_FUNC_GETMNTENT
    AC_CHECK_LIB([sun], [getpwnam])
    ```

    See [AC\_FUNC\_GETMNTENT](#AC_005fFUNC_005fGETMNTENT) and [AC\_CHECK\_LIB](#AC_005fCHECK_005fLIB).

Macro: **AC\_ISC\_POSIX** [¶](#index-AC_005fISC_005fPOSIX-1)
:   This macro adds `-lcposix` to output variable `LIBS` if
    necessary for POSIX facilities. Sun dropped support for the obsolete
    INTERACTIVE Systems Corporation Unix on 2006-07-23. New programs
    need not use this macro. It is implemented as
    `AC_SEARCH_LIBS([strerror], [cposix])` (see [AC\_SEARCH\_LIBS](#AC_005fSEARCH_005fLIBS)).

Macro: **AC\_LANG\_C** [¶](#index-AC_005fLANG_005fC-1)
:   Same as ‘`AC_LANG([C])`’ (see [AC\_LANG](#AC_005fLANG)).

Macro: **AC\_LANG\_CPLUSPLUS** [¶](#index-AC_005fLANG_005fCPLUSPLUS-1)
:   Same as ‘`AC_LANG([C++])`’ (see [AC\_LANG](#AC_005fLANG)).

Macro: **AC\_LANG\_FORTRAN77** [¶](#index-AC_005fLANG_005fFORTRAN77-1)
:   Same as ‘`AC_LANG([Fortran 77])`’ (see [AC\_LANG](#AC_005fLANG)).

Macro: **AC\_LANG\_RESTORE** [¶](#index-AC_005fLANG_005fRESTORE-1)
:   Select the language that is saved on the top of the stack, as set
    by `AC_LANG_SAVE`, remove it from the stack, and call
    `AC_LANG(language)`. See [Language Choice](#Language-Choice), for the
    preferred way to change languages.

Macro: **AC\_LANG\_SAVE** [¶](#index-AC_005fLANG_005fSAVE-1)
:   Remember the current language (as set by `AC_LANG`) on a stack.
    The current language does not change. `AC_LANG_PUSH` is preferred
    (see [AC\_LANG\_PUSH](#AC_005fLANG_005fPUSH)).

Macro: **AC\_LINK\_FILES**(source…, dest…) [¶](#index-AC_005fLINK_005fFILES-1)
:   This is an obsolete version of `AC_CONFIG_LINKS`
    (see [AC\_CONFIG\_LINKS](#AC_005fCONFIG_005fLINKS). An updated version of:

    ```
    AC_LINK_FILES(config/$machine.h config/$obj_format.h,
                  host.h            object.h)
    ```

    is:

    ```
    AC_CONFIG_LINKS([host.h:config/$machine.h
                    object.h:config/$obj_format.h])
    ```

Macro: **AC\_LN\_S** [¶](#index-AC_005fLN_005fS-1)
:   Replaced by `AC_PROG_LN_S` (see [AC\_PROG\_LN\_S](#AC_005fPROG_005fLN_005fS)).

Macro: **AC\_LONG\_64\_BITS** [¶](#index-AC_005fLONG_005f64_005fBITS-1)
:   Define `LONG_64_BITS` if the C type `long int` is 64 bits wide.
    Use the generic macro ‘`AC_CHECK_SIZEOF([long int])`’ instead
    (see [AC\_CHECK\_SIZEOF](#AC_005fCHECK_005fSIZEOF)).

Macro: **AC\_LONG\_DOUBLE** [¶](#index-AC_005fLONG_005fDOUBLE-1)
:   If the C compiler supports a working `long double` type with more
    range or precision than the `double` type, define
    `HAVE_LONG_DOUBLE`.

    You should use `AC_TYPE_LONG_DOUBLE` or
    `AC_TYPE_LONG_DOUBLE_WIDER` instead. See [Particular Type Checks](#Particular-Types).

Macro: **AC\_LONG\_FILE\_NAMES** [¶](#index-AC_005fLONG_005fFILE_005fNAMES-1)
:   Replaced by

    ```
    AC_SYS_LONG_FILE_NAMES
    ```

    See [AC\_SYS\_LONG\_FILE\_NAMES](#AC_005fSYS_005fLONG_005fFILE_005fNAMES).

Macro: **AC\_MAJOR\_HEADER** [¶](#index-AC_005fMAJOR_005fHEADER-1)
:   Replaced by `AC_HEADER_MAJOR` (see [AC\_HEADER\_MAJOR](#AC_005fHEADER_005fMAJOR)).

Macro: **AC\_MEMORY\_H** [¶](#index-AC_005fMEMORY_005fH-1)
:   Used to define `NEED_MEMORY_H` if the `mem` functions were
    defined in `memory.h`. Today it is equivalent to
    ‘`AC_CHECK_HEADERS([memory.h])`’ (see [AC\_CHECK\_HEADERS](#AC_005fCHECK_005fHEADERS)). Adjust
    your code to get the `mem` functions from `string.h` instead.

Macro: **AC\_MINGW32** [¶](#index-AC_005fMINGW32-1)
:   Similar to `AC_CYGWIN` but checks for the MinGW compiler
    environment and sets `MINGW32`. Don’t use this macro, the
    dignified means to check the nature of the host is using
    `AC_CANONICAL_HOST` (see [Getting the Canonical System Type](#Canonicalizing)).

Macro: **AC\_MINIX** [¶](#index-AC_005fMINIX-1)
:   This macro is a platform-specific subset of
    `AC_USE_SYSTEM_EXTENSIONS` (see [AC\_USE\_SYSTEM\_EXTENSIONS](#AC_005fUSE_005fSYSTEM_005fEXTENSIONS)).

Macro: **AC\_MINUS\_C\_MINUS\_O** [¶](#index-AC_005fMINUS_005fC_005fMINUS_005fO-1)
:   Replaced by `AC_PROG_CC_C_O` (see [AC\_PROG\_CC\_C\_O](#AC_005fPROG_005fCC_005fC_005fO)).

Macro: **AC\_MMAP** [¶](#index-AC_005fMMAP-1)
:   Replaced by `AC_FUNC_MMAP` (see [AC\_FUNC\_MMAP](#AC_005fFUNC_005fMMAP)).

Macro: **AC\_MODE\_T** [¶](#index-AC_005fMODE_005fT-1)
:   Replaced by `AC_TYPE_MODE_T` (see [AC\_TYPE\_MODE\_T](#AC_005fTYPE_005fMODE_005fT)).

Macro: **AC\_OBJEXT** [¶](#index-AC_005fOBJEXT-1)
:   Defined the output variable `OBJEXT` based on the output of the
    compiler, after .c files have been excluded. Typically set to ‘`o`’
    if POSIX, ‘`obj`’ if a DOS variant.
    Now the compiler checking macros handle
    this automatically.

Macro: **AC\_OBSOLETE**(this-macro-name, [suggestion]) [¶](#index-AC_005fOBSOLETE-1)
:   Make M4 print a message to the standard error output warning that
    this-macro-name is obsolete, and giving the file and line number
    where it was called. this-macro-name should be the name of the
    macro that is calling `AC_OBSOLETE`. If suggestion is given,
    it is printed at the end of the warning message; for example, it can be
    a suggestion for what to use instead of this-macro-name.

    For instance

    ```
    AC_OBSOLETE([$0], [; use AC_CHECK_HEADERS(unistd.h) instead])dnl
    ```

    You are encouraged to use `AU_DEFUN` instead, since it gives better
    services to the user (see [AU\_DEFUN](#AU_005fDEFUN)).

Macro: **AC\_OFF\_T** [¶](#index-AC_005fOFF_005fT-1)
:   Replaced by `AC_TYPE_OFF_T` (see [AC\_TYPE\_OFF\_T](#AC_005fTYPE_005fOFF_005fT)).

Macro: **AC\_OUTPUT**([file]…, [extra-cmds], [init-cmds]) [¶](#index-AC_005fOUTPUT-3)
:   The use of `AC_OUTPUT` with arguments is deprecated. This obsoleted
    interface is equivalent to:

    ```
    AC_CONFIG_FILES(file...)
    AC_CONFIG_COMMANDS([default],
                       extra-cmds, init-cmds)
    AC_OUTPUT
    ```

    See [AC\_CONFIG\_FILES](#AC_005fCONFIG_005fFILES), [AC\_CONFIG\_COMMANDS](#AC_005fCONFIG_005fCOMMANDS), and [AC\_OUTPUT](#AC_005fOUTPUT).

Macro: **AC\_OUTPUT\_COMMANDS**(extra-cmds, [init-cmds]) [¶](#index-AC_005fOUTPUT_005fCOMMANDS-1)
:   Specify additional shell commands to run at the end of
    `config.status`, and shell commands to initialize any variables
    from `configure`. This macro may be called multiple times. It is
    obsolete, replaced by `AC_CONFIG_COMMANDS` (see [AC\_CONFIG\_COMMANDS](#AC_005fCONFIG_005fCOMMANDS)).

    Here is an unrealistic example:

    ```
    fubar=27
    AC_OUTPUT_COMMANDS(
      [AS_ECHO(["this is extra $fubar, and so on."])],
      [fubar=$fubar])
    AC_OUTPUT_COMMANDS(
      [AS_ECHO(["this is another, extra, bit"])],
      [AS_ECHO(["init bit"])])
    ```

    Aside from the fact that `AC_CONFIG_COMMANDS` requires an
    additional key, an important difference is that
    `AC_OUTPUT_COMMANDS` is quoting its arguments twice, unlike
    `AC_CONFIG_COMMANDS`. This means that `AC_CONFIG_COMMANDS`
    can safely be given macro calls as arguments:

    ```
    AC_CONFIG_COMMANDS(foo, [my_FOO()])
    ```

    Conversely, where one level of quoting was enough for literal strings
    with `AC_OUTPUT_COMMANDS`, you need two with
    `AC_CONFIG_COMMANDS`. The following lines are equivalent:

    ```
    AC_OUTPUT_COMMANDS([echo "Square brackets: []"])
    AC_CONFIG_COMMANDS([default], [[echo "Square brackets: []"]])
    ```

Macro: **AC\_PID\_T** [¶](#index-AC_005fPID_005fT-1)
:   Replaced by `AC_TYPE_PID_T` (see [AC\_TYPE\_PID\_T](#AC_005fTYPE_005fPID_005fT)).

Macro: **AC\_PREFIX** [¶](#index-AC_005fPREFIX-1)
:   Replaced by `AC_PREFIX_PROGRAM` (see [AC\_PREFIX\_PROGRAM](#AC_005fPREFIX_005fPROGRAM)).

Macro: **AC\_PROG\_CC\_C89** [¶](#index-AC_005fPROG_005fCC_005fC89-1)
:   Now done by `AC_PROG_CC` (see [AC\_PROG\_CC](#AC_005fPROG_005fCC)).

Macro: **AC\_PROG\_CC\_C99** [¶](#index-AC_005fPROG_005fCC_005fC99-1)
:   Now done by `AC_PROG_CC` (see [AC\_PROG\_CC](#AC_005fPROG_005fCC)).

Macro: **AC\_PROG\_CC\_STDC** [¶](#index-AC_005fPROG_005fCC_005fSTDC-1)
:   Now done by `AC_PROG_CC` (see [AC\_PROG\_CC](#AC_005fPROG_005fCC)).

Macro: **AC\_PROG\_GCC\_TRADITIONAL** [¶](#index-AC_005fPROG_005fGCC_005fTRADITIONAL-1)
:   Used to put GCC into “traditional” (pre-ISO C) compilation mode,
    on systems with headers that did not work correctly with a
    standard-compliant compiler. GCC has not supported traditional
    compilation in many years, and all of the systems that required this are
    long obsolete themselves. This macro is now a compatibility synonym for
    `AC_PROG_CC` (see [AC\_PROG\_CC](#AC_005fPROG_005fCC)).

Macro: **AC\_PROGRAMS\_CHECK** [¶](#index-AC_005fPROGRAMS_005fCHECK-1)
:   Replaced by `AC_CHECK_PROGS` (see [AC\_CHECK\_PROGS](#AC_005fCHECK_005fPROGS)).

Macro: **AC\_PROGRAMS\_PATH** [¶](#index-AC_005fPROGRAMS_005fPATH-1)
:   Replaced by `AC_PATH_PROGS` (see [AC\_PATH\_PROGS](#AC_005fPATH_005fPROGS)).

Macro: **AC\_PROGRAM\_CHECK** [¶](#index-AC_005fPROGRAM_005fCHECK-1)
:   Replaced by `AC_CHECK_PROG` (see [AC\_CHECK\_PROG](#AC_005fCHECK_005fPROG)).

Macro: **AC\_PROGRAM\_EGREP** [¶](#index-AC_005fPROGRAM_005fEGREP-1)
:   Replaced by `AC_EGREP_CPP` (see [AC\_EGREP\_CPP](#AC_005fEGREP_005fCPP)).

Macro: **AC\_PROGRAM\_PATH** [¶](#index-AC_005fPROGRAM_005fPATH-1)
:   Replaced by `AC_PATH_PROG` (see [AC\_PATH\_PROG](#AC_005fPATH_005fPROG)).

Macro: **AC\_REMOTE\_TAPE** [¶](#index-AC_005fREMOTE_005fTAPE-1)
:   Removed because of limited usefulness.

Macro: **AC\_RESTARTABLE\_SYSCALLS** [¶](#index-AC_005fRESTARTABLE_005fSYSCALLS-1)
:   This macro was renamed `AC_SYS_RESTARTABLE_SYSCALLS`. However,
    these days portable programs should use `sigaction` with
    `SA_RESTART` if they want restartable system calls. They should
    not rely on `HAVE_RESTARTABLE_SYSCALLS`, since nowadays whether a
    system call is restartable is a dynamic issue, not a configuration-time
    issue.

Macro: **AC\_RETSIGTYPE** [¶](#index-AC_005fRETSIGTYPE-1)
:   Replaced by `AC_TYPE_SIGNAL` (see [AC\_TYPE\_SIGNAL](#AC_005fTYPE_005fSIGNAL)), which itself
    is obsolete.

Macro: **AC\_RSH** [¶](#index-AC_005fRSH-1)
:   Removed because of limited usefulness.

Macro: **AC\_SCO\_INTL** [¶](#index-AC_005fSCO_005fINTL-1)
:   Equivalent to the obsolescent macro `AC_FUNC_STRFTIME`.
    See [AC\_FUNC\_STRFTIME](#AC_005fFUNC_005fSTRFTIME).

Macro: **AC\_SETVBUF\_REVERSED** [¶](#index-AC_005fSETVBUF_005fREVERSED-1)
:   Replaced by

    ```
    AC_FUNC_SETVBUF_REVERSED
    ```

    See [AC\_FUNC\_SETVBUF\_REVERSED](#AC_005fFUNC_005fSETVBUF_005fREVERSED).

Macro: **AC\_SET\_MAKE** [¶](#index-AC_005fSET_005fMAKE-1)
:   Replaced by `AC_PROG_MAKE_SET` (see [AC\_PROG\_MAKE\_SET](#AC_005fPROG_005fMAKE_005fSET)).

Macro: **AC\_SIZEOF\_TYPE** [¶](#index-AC_005fSIZEOF_005fTYPE-1)
:   Replaced by `AC_CHECK_SIZEOF` (see [AC\_CHECK\_SIZEOF](#AC_005fCHECK_005fSIZEOF)).

Macro: **AC\_SIZE\_T** [¶](#index-AC_005fSIZE_005fT-1)
:   Replaced by `AC_TYPE_SIZE_T` (see [AC\_TYPE\_SIZE\_T](#AC_005fTYPE_005fSIZE_005fT)).

Macro: **AC\_STAT\_MACROS\_BROKEN** [¶](#index-AC_005fSTAT_005fMACROS_005fBROKEN-1)
:   Replaced by `AC_HEADER_STAT` (see [AC\_HEADER\_STAT](#AC_005fHEADER_005fSTAT)).

Macro: **AC\_STDC\_HEADERS** [¶](#index-AC_005fSTDC_005fHEADERS-1)
:   Replaced by `AC_HEADER_STDC` (see [AC\_HEADER\_STDC](#AC_005fHEADER_005fSTDC)), which
    is itself obsolete. Nowadays it is safe to assume the facilities of C89
    exist.

Macro: **AC\_STRCOLL** [¶](#index-AC_005fSTRCOLL-1)
:   Replaced by `AC_FUNC_STRCOLL` (see [AC\_FUNC\_STRCOLL](#AC_005fFUNC_005fSTRCOLL)).

Macro: **AC\_STRUCT\_ST\_BLKSIZE** [¶](#index-AC_005fSTRUCT_005fST_005fBLKSIZE-1)
:   If `struct stat` contains an `st_blksize` member, define
    `HAVE_STRUCT_STAT_ST_BLKSIZE`. The former name,
    `HAVE_ST_BLKSIZE` is to be avoided, as its support will cease in
    the future. This macro is obsoleted, and should be replaced by

    ```
    AC_CHECK_MEMBERS([struct stat.st_blksize])
    ```

    See [AC\_CHECK\_MEMBERS](#AC_005fCHECK_005fMEMBERS).

Macro: **AC\_STRUCT\_ST\_RDEV** [¶](#index-AC_005fSTRUCT_005fST_005fRDEV-1)
:   If `struct stat` contains an `st_rdev` member, define
    `HAVE_STRUCT_STAT_ST_RDEV`. The former name for this macro,
    `HAVE_ST_RDEV`, is to be avoided as it will cease to be supported
    in the future. Actually, even the new macro is obsolete and should be
    replaced by:

    ```
    AC_CHECK_MEMBERS([struct stat.st_rdev])
    ```

    See [AC\_CHECK\_MEMBERS](#AC_005fCHECK_005fMEMBERS).

Macro: **AC\_ST\_BLKSIZE** [¶](#index-AC_005fST_005fBLKSIZE-1)
:   Replaced by `AC_CHECK_MEMBERS` (see [AC\_CHECK\_MEMBERS](#AC_005fCHECK_005fMEMBERS)).

Macro: **AC\_ST\_BLOCKS** [¶](#index-AC_005fST_005fBLOCKS-1)
:   Replaced by `AC_STRUCT_ST_BLOCKS` (see [AC\_STRUCT\_ST\_BLOCKS](#AC_005fSTRUCT_005fST_005fBLOCKS)).

Macro: **AC\_ST\_RDEV** [¶](#index-AC_005fST_005fRDEV-1)
:   Replaced by `AC_CHECK_MEMBERS` (see [AC\_CHECK\_MEMBERS](#AC_005fCHECK_005fMEMBERS)).

Macro: **AC\_SYS\_RESTARTABLE\_SYSCALLS** [¶](#index-AC_005fSYS_005fRESTARTABLE_005fSYSCALLS-1)
:   If the system automatically restarts a system call that is interrupted
    by a signal, define `HAVE_RESTARTABLE_SYSCALLS`. This macro does
    not check whether system calls are restarted in general—it checks whether a
    signal handler installed with `signal` (but not `sigaction`)
    causes system calls to be restarted. It does not check whether system calls
    can be restarted when interrupted by signals that have no handler.

    These days portable programs should use `sigaction` with
    `SA_RESTART` if they want restartable system calls. They should
    not rely on `HAVE_RESTARTABLE_SYSCALLS`, since nowadays whether a
    system call is restartable is a dynamic issue, not a configuration-time
    issue.

Macro: **AC\_SYS\_SIGLIST\_DECLARED** [¶](#index-AC_005fSYS_005fSIGLIST_005fDECLARED-1)
:   This macro was renamed `AC_DECL_SYS_SIGLIST`. However, even that
    name is obsolete, as the same functionality is now achieved via
    `AC_CHECK_DECLS` (see [AC\_CHECK\_DECLS](#AC_005fCHECK_005fDECLS)).

Macro: **AC\_TEST\_CPP** [¶](#index-AC_005fTEST_005fCPP-1)
:   This macro was renamed `AC_TRY_CPP`, which in turn was replaced by
    `AC_PREPROC_IFELSE` (see [AC\_PREPROC\_IFELSE](#AC_005fPREPROC_005fIFELSE)).

Macro: **AC\_TEST\_PROGRAM** [¶](#index-AC_005fTEST_005fPROGRAM-1)
:   This macro was renamed `AC_TRY_RUN`, which in turn was replaced by
    `AC_RUN_IFELSE` (see [AC\_RUN\_IFELSE](#AC_005fRUN_005fIFELSE)).

Macro: **AC\_TIMEZONE** [¶](#index-AC_005fTIMEZONE-1)
:   Replaced by `AC_STRUCT_TIMEZONE` (see [AC\_STRUCT\_TIMEZONE](#AC_005fSTRUCT_005fTIMEZONE)).

Macro: **AC\_TIME\_WITH\_SYS\_TIME** [¶](#index-AC_005fTIME_005fWITH_005fSYS_005fTIME-1)
:   Replaced by `AC_HEADER_TIME` (see [AC\_HEADER\_TIME](#AC_005fHEADER_005fTIME)), which is
    itself obsolete; nowadays one need only do
    ‘`AC_CHECK_HEADERS([sys/time.h])`’.

Macro: **AC\_TRY\_COMPILE**(includes, function-body, [action-if-true], [action-if-false]) [¶](#index-AC_005fTRY_005fCOMPILE-1)
:   Same as:

    ```
    AC_COMPILE_IFELSE(
      [AC_LANG_PROGRAM([[includes]],
         [[function-body]])],
      [action-if-true],
      [action-if-false])
    ```

    See [Running the Compiler](#Running-the-Compiler).

    This macro double quotes both includes and function-body.

    For C and C++, includes is any `#include` statements needed
    by the code in function-body (includes is ignored if
    the currently selected language is Fortran or Fortran 77). The compiler
    and compilation flags are determined by the current language
    (see [Language Choice](#Language-Choice)).

Macro: **AC\_TRY\_CPP**(input, [action-if-true], [action-if-false]) [¶](#index-AC_005fTRY_005fCPP-1)
:   Same as:

    ```
    AC_PREPROC_IFELSE(
      [AC_LANG_SOURCE([[input]])],
      [action-if-true],
      [action-if-false])
    ```

    See [Running the Preprocessor](#Running-the-Preprocessor).

    This macro double quotes the input.

Macro: **AC\_TRY\_LINK**(includes, function-body, [action-if-true], [action-if-false]) [¶](#index-AC_005fTRY_005fLINK-1)
:   Same as:

    ```
    AC_LINK_IFELSE(
      [AC_LANG_PROGRAM([[includes]],
         [[function-body]])],
      [action-if-true],
      [action-if-false])
    ```

    See [Running the Linker](#Running-the-Linker).

    This macro double quotes both includes and function-body.

    Depending on the current language (see [Language Choice](#Language-Choice)), create a
    test program to see whether a function whose body consists of
    function-body can be compiled and linked. If the file compiles
    and links successfully, run shell commands action-if-found,
    otherwise run action-if-not-found.

    This macro double quotes both includes and function-body.

    For C and C++, includes is any `#include` statements needed
    by the code in function-body (includes is ignored if
    the currently selected language is Fortran or Fortran 77). The compiler
    and compilation flags are determined by the current language
    (see [Language Choice](#Language-Choice)), and in addition `LDFLAGS` and
    `LIBS` are used for linking.

Macro: **AC\_TRY\_LINK\_FUNC**(function, [action-if-found], [action-if-not-found]) [¶](#index-AC_005fTRY_005fLINK_005fFUNC-1)
:   This macro is equivalent to

    ```
    AC_LINK_IFELSE([AC_LANG_CALL([], [function])],
      [action-if-found], [action-if-not-found])
    ```

    See [Running the Linker](#Running-the-Linker).

Macro: **AC\_TRY\_RUN**(program, [action-if-true], [action-if-false], [action-if-cross-compiling = ‘`AC_MSG_FAILURE`’]) [¶](#index-AC_005fTRY_005fRUN-1)
:   Same as:

    ```
    AC_RUN_IFELSE(
      [AC_LANG_SOURCE([[program]])],
      [action-if-true],
      [action-if-false],
      [action-if-cross-compiling])
    ```

    See [Checking Runtime Behavior](#Runtime).

Macro: **AC\_TYPE\_SIGNAL** [¶](#index-AC_005fTYPE_005fSIGNAL-1)
:   If `signal.h` declares `signal` as returning a pointer to a
    function returning `void`, define `RETSIGTYPE` to be
    `void`; otherwise, define it to be `int`. These days, it is
    portable to assume C89, and that signal handlers return `void`,
    without needing to use this macro or `RETSIGTYPE`.

Macro: **AC\_UID\_T** [¶](#index-AC_005fUID_005fT-1)
:   Replaced by `AC_TYPE_UID_T` (see [AC\_TYPE\_UID\_T](#AC_005fTYPE_005fUID_005fT)).

Macro: **AC\_UNISTD\_H** [¶](#index-AC_005fUNISTD_005fH-1)
:   Same as ‘`AC_CHECK_HEADERS([unistd.h])`’ (see [AC\_CHECK\_HEADERS](#AC_005fCHECK_005fHEADERS)),
    which is one of the tests done as a side effect by
    `AC_INCLUDES_DEFAULT` (see [Default Includes](#Default-Includes)), so usually
    unnecessary to write explicitly.

Macro: **AC\_USG** [¶](#index-AC_005fUSG-1)
:   Define `USG` if the BSD string functions (`bcopy`,
    `bzero`, `index`, `rindex`, etc) are *not* defined
    in `strings.h`. Modern code should assume `string.h` exists
    and should use the standard C string functions (`memmove`, `memset`,
    `strchr`, `strrchr`, etc) unconditionally.

    `strings.h` may be the only header that declares `strcasecmp`,
    `strncasecmp`, and `ffs`. `AC_INCLUDES_DEFAULT` checks
    for it (see [Default Includes](#Default-Includes)); test `HAVE_STRINGS_H`.

Macro: **AC\_UTIME\_NULL** [¶](#index-AC_005fUTIME_005fNULL-1)
:   Replaced by `AC_FUNC_UTIME_NULL` (see [AC\_FUNC\_UTIME\_NULL](#AC_005fFUNC_005fUTIME_005fNULL)).

Macro: **AC\_VALIDATE\_CACHED\_SYSTEM\_TUPLE**([cmd]) [¶](#index-AC_005fVALIDATE_005fCACHED_005fSYSTEM_005fTUPLE-1)
:   If the cache file is inconsistent with the current host, target and
    build system types, it used to execute cmd or print a default
    error message. This is now handled by default.

Macro: **AC\_VERBOSE**(result-description) [¶](#index-AC_005fVERBOSE-1)
:   Replaced by `AC_MSG_RESULT` (see [AC\_MSG\_RESULT](#AC_005fMSG_005fRESULT)).

Macro: **AC\_VFORK** [¶](#index-AC_005fVFORK-1)
:   Replaced by `AC_FUNC_FORK` (see [AC\_FUNC\_FORK](#AC_005fFUNC_005fFORK)).

Macro: **AC\_VPRINTF** [¶](#index-AC_005fVPRINTF-1)
:   Replaced by `AC_FUNC_VPRINTF` (see [AC\_FUNC\_VPRINTF](#AC_005fFUNC_005fVPRINTF)).

Macro: **AC\_WAIT3** [¶](#index-AC_005fWAIT3-1)
:   This macro was renamed `AC_FUNC_WAIT3`. However, these days
    portable programs should use `waitpid`, not `wait3`, as
    `wait3` has been removed from POSIX.

Macro: **AC\_WARN** [¶](#index-AC_005fWARN-1)
:   Replaced by `AC_MSG_WARN` (see [AC\_MSG\_WARN](#AC_005fMSG_005fWARN)).

Macro: **AC\_WARNING**(message) [¶](#index-AC_005fWARNING-1)
:   Replaced by `m4_warn` (see [m4\_warn](#m4_005fwarn)).

Macro: **AC\_WITH**(package, action-if-given, [action-if-not-given]) [¶](#index-AC_005fWITH-1)
:   This is an obsolete version of `AC_ARG_WITH` that does not
    support providing a help string (see [AC\_ARG\_WITH](#AC_005fARG_005fWITH)).

Macro: **AC\_WORDS\_BIGENDIAN** [¶](#index-AC_005fWORDS_005fBIGENDIAN-1)
:   Replaced by `AC_C_BIGENDIAN` (see [AC\_C\_BIGENDIAN](#AC_005fC_005fBIGENDIAN)).

Macro: **AC\_XENIX\_DIR** [¶](#index-AC_005fXENIX_005fDIR-1)
:   This macro is equivalent to the obsolescent `AC_HEADER_DIRENT`
    macro, plus it also sets the shell variable `XENIX`.
    Don’t use this macro, the dignified means to check the nature of the
    host is using `AC_CANONICAL_HOST` (see [Getting the Canonical System Type](#Canonicalizing)).

Macro: **AC\_YYTEXT\_POINTER** [¶](#index-AC_005fYYTEXT_005fPOINTER-1)
:   This macro was renamed `AC_DECL_YYTEXT`, which in turn was
    integrated into `AC_PROG_LEX` (see [AC\_PROG\_LEX](#AC_005fPROG_005fLEX)).

---

Next: [Upgrading From Version 2.13](#Autoconf-2_002e13), Previous: [Obsolete Macros](#Obsolete-Macros), Up: [Obsolete Constructs](#Obsolete-Constructs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 18.5 Upgrading From Version 1 [¶](#Upgrading-From-Version-1)

Autoconf version 2 is mostly backward compatible with version 1.
However, it introduces better ways to do some things, and doesn’t
support some of the ugly things in version 1. So, depending on how
sophisticated your `configure.ac` files are, you might have to do
some manual work in order to upgrade to version 2. This chapter points
out some problems to watch for when upgrading. Also, perhaps your
`configure` scripts could benefit from some of the new features in
version 2; the changes are summarized in the file `NEWS` in the
Autoconf distribution.

* [Changed File Names](#Changed-File-Names)
* [Changed Makefiles](#Changed-Makefiles)
* [Changed Macros](#Changed-Macros)
* [Changed Results](#Changed-Results)
* [Changed Macro Writing](#Changed-Macro-Writing)

---

Next: [Changed Makefiles](#Changed-Makefiles), Up: [Upgrading From Version 1](#Autoconf-1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 18.5.1 Changed File Names [¶](#Changed-File-Names-1)

If you have an `aclocal.m4` installed with Autoconf (as opposed to
in a particular package’s source directory), you must rename it to
`acsite.m4`. See [Using `autoconf` to Create `configure`](#autoconf-Invocation).

If you distribute `install.sh` with your package, rename it to
`install-sh` so `make` builtin rules don’t inadvertently
create a file called `install` from it. `AC_PROG_INSTALL`
looks for the script under both names, but it is best to use the new name.

If you were using `config.h.top`, `config.h.bot`, or
`acconfig.h`, you still can, but you have less clutter if you
use the `AH_` macros. See [Autoheader Macros](#Autoheader-Macros).

---

Next: [Changed Macros](#Changed-Macros), Previous: [Changed File Names](#Changed-File-Names), Up: [Upgrading From Version 1](#Autoconf-1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 18.5.2 Changed Makefiles [¶](#Changed-Makefiles-1)

Add ‘`@CFLAGS@`’, ‘`@CPPFLAGS@`’, and ‘`@LDFLAGS@`’ in
your `Makefile.in` files, so they can take advantage of the values
of those variables in the environment when `configure` is run.
Doing this isn’t necessary, but it’s a convenience for users.

Also add ‘`@configure_input@`’ in a comment to each input file for
`AC_OUTPUT`, so that the output files contain a comment saying
they were produced by `configure`. Automatically selecting the
right comment syntax for all the kinds of files that people call
`AC_OUTPUT` on became too much work.

Add `config.log` and `config.cache` to the list of files you
remove in `distclean` targets.

If you have the following in `Makefile.in`:

```
prefix = /usr/local
exec_prefix = $(prefix)
```

you must change it to:

```
prefix = @prefix@
exec_prefix = @exec_prefix@
```

The old behavior of replacing those variables without ‘`@`’
characters around them has been removed.

---

Next: [Changed Results](#Changed-Results), Previous: [Changed Makefiles](#Changed-Makefiles), Up: [Upgrading From Version 1](#Autoconf-1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 18.5.3 Changed Macros [¶](#Changed-Macros-1)

Many of the macros were renamed in Autoconf version 2. You can still
use the old names, but the new ones are clearer, and it’s easier to find
the documentation for them. See [Obsolete Macros](#Obsolete-Macros), for a table showing the
new names for the old macros. Use the `autoupdate` program to
convert your `configure.ac` to using the new macro names.
See [Using `autoupdate` to Modernize `configure.ac`](#autoupdate-Invocation).

Some macros have been superseded by similar ones that do the job better,
but are not call-compatible. If you get warnings about calling obsolete
macros while running `autoconf`, you may safely ignore them, but
your `configure` script generally works better if you follow
the advice that is printed about what to replace the obsolete macros with. In
particular, the mechanism for reporting the results of tests has
changed. If you were using `echo` or `AC_VERBOSE` (perhaps
via `AC_COMPILE_CHECK`), your `configure` script’s output
looks better if you switch to `AC_MSG_CHECKING` and
`AC_MSG_RESULT`. See [Printing Messages](#Printing-Messages). Those macros work best
in conjunction with cache variables. See [Caching Results](#Caching-Results).

---

Next: [Changed Macro Writing](#Changed-Macro-Writing), Previous: [Changed Macros](#Changed-Macros), Up: [Upgrading From Version 1](#Autoconf-1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 18.5.4 Changed Results [¶](#Changed-Results-1)

If you were checking the results of previous tests by examining the
shell variable `DEFS`, you need to switch to checking the values of
the cache variables for those tests. `DEFS` no longer exists while
`configure` is running; it is only created when generating output
files. This difference from version 1 is because properly quoting the
contents of that variable turned out to be too cumbersome and
inefficient to do every time `AC_DEFINE` is called. See [Cache Variable Names](#Cache-Variable-Names).

For example, here is a `configure.ac` fragment written for Autoconf
version 1:

```
AC_HAVE_FUNCS(syslog)
case "$DEFS" in
*-DHAVE_SYSLOG*) ;;
*) # syslog is not in the default libraries.  See if it's in some other.
  saved_LIBS="$LIBS"
  for lib in bsd socket inet; do
    AC_CHECKING(for syslog in -l$lib)
    LIBS="-l$lib $saved_LIBS"
    AC_HAVE_FUNCS(syslog)
    case "$DEFS" in
    *-DHAVE_SYSLOG*) break ;;
    *) ;;
    esac
    LIBS="$saved_LIBS"
  done ;;
esac
```

Here is a way to write it for version 2:

```
AC_CHECK_FUNCS([syslog])
AS_IF([test "x$ac_cv_func_syslog" = xno],
  [# syslog is not in the default libraries.  See if it's in some other.
   for lib in bsd socket inet; do
     AC_CHECK_LIB([$lib], [syslog],
       [AC_DEFINE([HAVE_SYSLOG])
        LIBS="-l$lib $LIBS"; break])
   done])
```

If you were working around bugs in `AC_DEFINE_UNQUOTED` by adding
backslashes before quotes, you need to remove them. It now works
predictably, and does not treat quotes (except back quotes) specially.
See [Setting Output Variables](#Setting-Output-Variables).

All of the Boolean shell variables set by Autoconf macros now use
‘`yes`’ for the true value. Most of them use ‘`no`’ for false,
though for backward compatibility some use the empty string instead. If
you were relying on a shell variable being set to something like 1 or
‘`t`’ for true, you need to change your tests.

---

Previous: [Changed Results](#Changed-Results), Up: [Upgrading From Version 1](#Autoconf-1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 18.5.5 Changed Macro Writing [¶](#Changed-Macro-Writing-1)

When defining your own macros, you should now use `AC_DEFUN`
instead of `define`. `AC_DEFUN` automatically calls
`AC_PROVIDE` and ensures that macros called via `AC_REQUIRE`
do not interrupt other macros, to prevent nested ‘`checking…`’
messages on the screen. There’s no actual harm in continuing to use the
older way, but it’s less convenient and attractive. See [Macro Definitions](#Macro-Definitions).

You probably looked at the macros that came with Autoconf as a guide for
how to do things. It would be a good idea to take a look at the new
versions of them, as the style is somewhat improved and they take
advantage of some new features.

If you were doing tricky things with undocumented Autoconf internals
(macros, variables, diversions), check whether you need to change
anything to account for changes that have been made. Perhaps you can
even use an officially supported technique in version 2 instead of
kludging. Or perhaps not.

To speed up your locally written feature tests, add caching to them.
See whether any of your tests are of general enough usefulness to
encapsulate them into macros that you can share.

---

Previous: [Upgrading From Version 1](#Autoconf-1), Up: [Obsolete Constructs](#Obsolete-Constructs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
