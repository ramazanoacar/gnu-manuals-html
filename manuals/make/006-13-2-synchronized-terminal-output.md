### 13.2 Synchronized Terminal Output

Normally GNU `make` will invoke all commands with access to the
same standard and error outputs that `make` itself was started
with. A number of tools will detect whether the output is a terminal
or not-a-terminal, and use this information to change the output
style. For example if the output goes to a terminal the tool may add
control characters that set color, or even change the location of the
cursor. If the output is not going to a terminal then these special
control characters are not emitted so that they don’t corrupt log
files, etc.

The `--output-sync` (see [Output During
Parallel Execution](#Parallel-Output)) option will defeat the terminal detection. When
output synchronization is enabled GNU `make` arranges for all
command output to be written to a file, so that its output can be
written as a block without interference from other commands. This
means that all tools invoked by `make` will believe that their
output is not going to be displayed on a terminal, even when it will
be (because `make` will display it there after the command is
completed).

In order to facilitate tools which would like to determine whether or
not their output will be displayed on a terminal, GNU `make` will
set the `MAKE_TERMOUT` and `MAKE_TERMERR` environment
variables before invoking any commands. Tools which would like to
determine whether standard or error output (respectively) will be
displayed on a terminal can check these environment variables to
determine if they exist and contain a non-empty value. If so the tool
can assume that the output will (eventually) be displayed on a
terminal. If the variables are not set or have an empty value, then
the tool should fall back to its normal methods of detecting whether
output is going to a terminal or not.

The content of the variables can be parsed to determine the type of
terminal which will be used to display the output.

Similarly, environments which invoke `make` and would like to
capture the output and eventually display it on a terminal (or some
display which can interpret terminal control characters) can set these
variables before invoking `make`. GNU `make` will not
modify these environment variables if they already exist when it
starts.

---

Next: [Incompatibilities and Missing Features](#Missing), Previous: [Integrating GNU `make`](#Integrating-make), Up: [GNU `make`](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]## 14 Features of GNU `make`

Here is a summary of the features of GNU `make`, for comparison
with and credit to other versions of `make`. We consider the
features of `make` in 4.2 BSD systems as a baseline. If you are
concerned with writing portable makefiles, you should not use the
features of `make` listed here, nor the ones in [Incompatibilities and Missing Features](#Missing).

Many features come from the version of `make` in System V.

* The `VPATH` variable and its special meaning.
  See [Searching Directories for Prerequisites](#Directory-Search).
  This feature exists in System V `make`, but is undocumented.
  It is documented in 4.3 BSD `make` (which says it mimics System V’s
  `VPATH` feature).
* Included makefiles. See [Including Other Makefiles](#Include).
  Allowing multiple files to be included with a single directive is a GNU
  extension.
* Variables are read from and communicated via the environment.
  See [Variables from the Environment](#Environment).
* Options passed through the variable `MAKEFLAGS` to recursive
  invocations of `make`.
  See [Communicating Options to a Sub-`make`](#Options_002fRecursion).
* The automatic variable `$%` is set to the member name
  in an archive reference. See [Automatic Variables](#Automatic-Variables).
* The automatic variables `$@`, `$*`, `$<`, `$%`,
  and `$?` have corresponding forms like `$(@F)` and
  `$(@D)`. We have generalized this to `$^` as an obvious
  extension. See [Automatic Variables](#Automatic-Variables).
* Substitution variable references.
  See [Basics of Variable References](#Reference).
* The command line options ‘`-b`’ and ‘`-m`’, accepted and
  ignored. In System V `make`, these options actually do something.
* Execution of recursive commands to run `make` via the variable
  `MAKE` even if ‘`-n`’, ‘`-q`’ or ‘`-t`’ is specified.
  See [Recursive Use of `make`](#Recursion).
* Support for suffix ‘`.a`’ in suffix rules. See [Suffix Rules for Archive Files](#Archive-Suffix-Rules). This feature is obsolete in GNU `make`, because the
  general feature of rule chaining (see [Chains of
  Implicit Rules](#Chained-Rules)) allows one pattern rule for installing members in an
  archive (see [Implicit Rule for Archive Member Targets](#Archive-Update)) to be sufficient.
* The arrangement of lines and backslash/newline combinations in
  recipes is retained when the recipes are printed, so they appear as
  they do in the makefile, except for the stripping of initial
  whitespace.

The following features were inspired by various other versions of
`make`. In some cases it is unclear exactly which versions inspired
which others.

* Pattern rules using ‘`%`’.
  This has been implemented in several versions of `make`.
  We’re not sure who invented it first, but it’s been spread around a bit.
  See [Defining and Redefining Pattern Rules](#Pattern-Rules).
* Rule chaining and implicit intermediate files.
  This was implemented by Stu Feldman in his version of `make`
  for AT&T Eighth Edition Research Unix, and later by Andrew Hume of
  AT&T Bell Labs in his `mk` program (where he terms it
  “transitive closure”). We do not really know if
  we got this from either of them or thought it up ourselves at the
  same time. See [Chains of Implicit Rules](#Chained-Rules).
* The automatic variable `$^` containing a list of all prerequisites
  of the current target. We did not invent this, but we have no idea who
  did. See [Automatic Variables](#Automatic-Variables). The automatic variable
  `$+` is a simple extension of `$^`.
* The “what if” flag (‘`-W`’ in GNU `make`) was (as far as we know)
  invented by Andrew Hume in `mk`.
  See [Instead of Executing Recipes](#Instead-of-Execution).
* The concept of doing several things at once (parallelism) exists in
  many incarnations of `make` and similar programs, though not in the
  System V or BSD implementations. See [Recipe Execution](#Execution).
* A number of different build tools that support parallelism also
  support collecting output and displaying as a single block.
  See [Output During Parallel Execution](#Parallel-Output).
* Modified variable references using pattern substitution come from
  SunOS 4. See [Basics of Variable References](#Reference).
  This functionality was provided in GNU `make` by the
  `patsubst` function before the alternate syntax was implemented
  for compatibility with SunOS 4. It is not altogether clear who
  inspired whom, since GNU `make` had `patsubst` before SunOS
  4 was released.
* The special significance of ‘`+`’ characters preceding recipe lines
  (see [Instead of Executing Recipes](#Instead-of-Execution)) is
  mandated by IEEE Standard 1003.2-1992 (POSIX.2).
* The ‘`+=`’ syntax to append to the value of a variable comes from SunOS
  4 `make`. See [Appending More Text to Variables](#Appending).
* The syntax ‘`archive(mem1 mem2…)`’ to list
  multiple members in a single archive file comes from SunOS 4 `make`.
  See [Archive Members as Targets](#Archive-Members).
* The `-include` directive to include makefiles with no error for a
  nonexistent file comes from SunOS 4 `make`. (But note that SunOS 4
  `make` does not allow multiple makefiles to be specified in one
  `-include` directive.) The same feature appears with the name
  `sinclude` in SGI `make` and perhaps others.
* The `!=` shell assignment operator exists in many BSD of
  `make` and is purposefully implemented here to behave identically
  to those implementations.
* Various build management tools are implemented using scripting
  languages such as Perl or Python and thus provide a natural embedded
  scripting language, similar to GNU `make`’s integration of GNU
  Guile.

The remaining features are inventions new in GNU `make`:

* Use the ‘`-v`’ or ‘`--version`’ option to print version and
  copyright information.
* Use the ‘`-h`’ or ‘`--help`’ option to summarize the options to
  `make`.
* Simply-expanded variables. See [The Two Flavors of Variables](#Flavors).
* Pass command line variable assignments automatically through the
  variable `MAKE` to recursive `make` invocations.
  See [Recursive Use of `make`](#Recursion).
* Use the ‘`-C`’ or ‘`--directory`’ command option to change
  directory. See [Summary of Options](#Options-Summary).
* Make verbatim variable definitions with `define`.
  See [Defining Multi-Line Variables](#Multi_002dLine).
* Declare phony targets with the special target `.PHONY`.

  Andrew Hume of AT&T Bell Labs implemented a similar feature with a
  different syntax in his `mk` program. This seems to be a case of
  parallel discovery. See [Phony Targets](#Phony-Targets).
* Manipulate text by calling functions.
  See [Functions for Transforming Text](#Functions).
* Use the ‘`-o`’ or ‘`--old-file`’
  option to pretend a file’s modification-time is old.
  See [Avoiding Recompilation of Some Files](#Avoiding-Compilation).
* Conditional execution.

  This feature has been implemented numerous times in various versions
  of `make`; it seems a natural extension derived from the features
  of the C preprocessor and similar macro languages and is not a
  revolutionary concept. See [Conditional Parts of Makefiles](#Conditionals).
* Specify a search path for included makefiles.
  See [Including Other Makefiles](#Include).
* Specify extra makefiles to read with an environment variable.
  See [The Variable `MAKEFILES`](#MAKEFILES-Variable).
* Strip leading sequences of ‘`./`’ from file names, so that
  `./file` and `file` are considered to be the
  same file.
* Use a special search method for library prerequisites written in the
  form ‘`-lname`’.
  See [Directory Search for Link Libraries](#Libraries_002fSearch).
* Allow suffixes for suffix rules
  (see [Old-Fashioned Suffix Rules](#Suffix-Rules)) to contain any
  characters. In other versions of `make`, they must begin with
  ‘`.`’ and not contain any ‘`/`’ characters.
* Keep track of the current level of `make` recursion using the
  variable `MAKELEVEL`. See [Recursive Use of `make`](#Recursion).
* Provide any goals given on the command line in the variable
  `MAKECMDGOALS`. See [Arguments to Specify the Goals](#Goals).
* Specify static pattern rules. See [Static Pattern Rules](#Static-Pattern).
* Provide selective `vpath` search.
  See [Searching Directories for Prerequisites](#Directory-Search).
* Provide computed variable references.
  See [Basics of Variable References](#Reference).
* Update makefiles. See [How Makefiles Are Remade](#Remaking-Makefiles).
  System V `make` has a very, very limited form of this
  functionality in that it will check out SCCS files for makefiles.
* Various new built-in implicit rules.
  See [Catalogue of Built-In Rules](#Catalogue-of-Rules).
* Load dynamic objects which can modify the behavior of `make`.
  See [Loading Dynamic Objects](#Loading-Objects).

---

Next: [Makefile Conventions](#Makefile-Conventions), Previous: [Features of GNU `make`](#Features), Up: [GNU `make`](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]## 15 Incompatibilities and Missing Features

The `make` programs in various other systems support a few features
that are not implemented in GNU `make`. The POSIX.2 standard
(IEEE Standard 1003.2-1992) which specifies `make` does not
require any of these features.

* A target of the form ‘`file((entry))`’ stands for a member
  of archive file file. The member is chosen, not by name, but by
  being an object file which defines the linker symbol entry.

  This feature was not put into GNU `make` because of the
  non-modularity of putting knowledge into `make` of the internal
  format of archive file symbol tables.
  See [Updating Archive Symbol Directories](#Archive-Symbols).
* Suffixes (used in suffix rules) that end with the character ‘`~`’
  have a special meaning to System V `make`;
  they refer to the SCCS file that corresponds
  to the file one would get without the ‘`~`’. For example, the
  suffix rule ‘`.c~.o`’ would make the file `n.o` from
  the SCCS file `s.n.c`. For complete coverage, a whole
  series of such suffix rules is required.
  See [Old-Fashioned Suffix Rules](#Suffix-Rules).

  In GNU `make`, this entire series of cases is handled by two
  pattern rules for extraction from SCCS, in combination with the
  general feature of rule chaining.
  See [Chains of Implicit Rules](#Chained-Rules).
* In System V and 4.3 BSD `make`, files found by `VPATH`
  search (see [Searching Directories for
  Prerequisites](#Directory-Search)) have their names changed inside recipes. We feel it
  is much cleaner to always use automatic variables and thus make this
  feature unnecessary.
* In some Unix `make`s, the automatic variable `$*` appearing in
  the prerequisites of a rule has the amazingly strange “feature” of
  expanding to the full name of the *target of that rule*. We cannot
  imagine what went on in the minds of Unix `make` developers to do
  this; it is utterly inconsistent with the normal definition of `$*`.
* In some Unix `make`s, implicit rule search (see [Using Implicit Rules](#Implicit-Rules)) is apparently done for *all*
  targets, not just those without recipes. This means you can
  do:

  ```
  foo.o:
          cc -c foo.c
  ```

  and Unix `make` will intuit that `foo.o` depends on
  `foo.c`.

  We feel that such usage is broken. The prerequisite properties of
  `make` are well-defined (for GNU `make`, at least),
  and doing such a thing simply does not fit the model.
* GNU `make` does not include any built-in implicit rules for
  compiling or preprocessing EFL programs. If we hear of anyone who is
  using EFL, we will gladly add them.
* It appears that in SVR4 `make`, a suffix rule can be specified
  with no recipe, and it is treated as if it had an empty recipe
  (see [Using Empty Recipes](#Empty-Recipes)). For example:

  ```
  .c.a:
  ```

  will override the built-in `.c.a` suffix rule.

  We feel that it is cleaner for a rule without a recipe to always simply
  add to the prerequisite list for the target. The above example can be
  easily rewritten to get the desired behavior in GNU `make`:

  ```
  .c.a: ;
  ```
* Some versions of `make` invoke the shell with the ‘`-e`’ flag,
  except under ‘`-k`’ (see [Testing the Compilation of a
  Program](#Testing)). The ‘`-e`’ flag tells the shell to exit as soon as any
  program it runs returns a nonzero status. We feel it is cleaner to
  write each line of the recipe to stand on its own and not require this
  special treatment.

---

Next: [Quick Reference](#Quick-Reference), Previous: [Incompatibilities and Missing Features](#Missing), Up: [GNU `make`](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]## 16 Makefile Conventions

This
describes conventions for writing the Makefiles for GNU programs.
Using Automake will help you write a Makefile that follows these
conventions. For more information on portable Makefiles, see
POSIX and [Portable Make Programming](https://www.gnu.org/software/autoconf/manual/autoconf.html#Portable-Make) in Autoconf.

* [General Conventions for Makefiles](#Makefile-Basics)
* [Utilities in Makefiles](#Utilities-in-Makefiles)
* [Variables for Specifying Commands](#Command-Variables)
* [`DESTDIR`: Support for Staged Installs](#DESTDIR)
* [Variables for Installation Directories](#Directory-Variables)
* [Standard Targets for Users](#Standard-Targets)
* [Install Command Categories](#Install-Command-Categories)

---

Next: [Utilities in Makefiles](#Utilities-in-Makefiles), Up: [Makefile Conventions](#Makefile-Conventions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 16.1 General Conventions for Makefiles

Every Makefile should contain this line:

```
SHELL = /bin/sh
```

to avoid trouble on systems where the `SHELL` variable might be
inherited from the environment. (This is never a problem with GNU
`make`.)

Different `make` programs have incompatible suffix lists and
implicit rules, and this sometimes creates confusion or misbehavior. So
it is a good idea to set the suffix list explicitly using only the
suffixes you need in the particular Makefile, like this:

```
.SUFFIXES:
.SUFFIXES: .c .o
```

The first line clears out the suffix list, the second introduces all
suffixes which may be subject to implicit rules in this Makefile.

Don’t assume that `.` is in the path for command execution. When
you need to run programs that are a part of your package during the
make, please make sure that it uses `./` if the program is built as
part of the make or `$(srcdir)/` if the file is an unchanging part
of the source code. Without one of these prefixes, the current search
path is used.

The distinction between `./` (the *build directory*) and
`$(srcdir)/` (the *source directory*) is important because
users can build in a separate directory using the ‘`--srcdir`’ option
to `configure`. A rule of the form:

```
foo.1 : foo.man sedscript
        sed -f sedscript foo.man > foo.1
```

will fail when the build directory is not the source directory, because
`foo.man` and `sedscript` are in the source directory.

When using GNU `make`, relying on ‘`VPATH`’ to find the source
file will work in the case where there is a single dependency file,
since the `make` automatic variable ‘`$<`’ will represent the
source file wherever it is. (Many versions of `make` set ‘`$<`’
only in implicit rules.) A Makefile target like

```
foo.o : bar.c
        $(CC) -I. -I$(srcdir) $(CFLAGS) -c bar.c -o foo.o
```

should instead be written as

```
foo.o : bar.c
        $(CC) -I. -I$(srcdir) $(CFLAGS) -c $< -o $@
```

in order to allow ‘`VPATH`’ to work correctly. When the target has
multiple dependencies, using an explicit ‘`$(srcdir)`’ is the easiest
way to make the rule work well. For example, the target above for
`foo.1` is best written as:

```
foo.1 : foo.man sedscript
        sed -f $(srcdir)/sedscript $(srcdir)/foo.man > $@
```

GNU distributions usually contain some files which are not source
files—for example, Info files, and the output from Autoconf, Automake,
Bison or Flex. Since these files normally appear in the source
directory, they should always appear in the source directory, not in the
build directory. So Makefile rules to update them should put the
updated files in the source directory.

However, if a file does not appear in the distribution, then the
Makefile should not put it in the source directory, because building a
program in ordinary circumstances should not modify the source directory
in any way.

Try to make the build and installation targets, at least (and all their
subtargets) work correctly with a parallel `make`.

---

Next: [Variables for Specifying Commands](#Command-Variables), Previous: [General Conventions for Makefiles](#Makefile-Basics), Up: [Makefile Conventions](#Makefile-Conventions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 16.2 Utilities in Makefiles

Write the Makefile commands (and any shell scripts, such as
`configure`) to run under `sh` (both the traditional Bourne
shell and the POSIX shell), not `csh`. Don’t use any
special features of `ksh` or `bash`, or POSIX features
not widely supported in traditional Bourne `sh`.

The `configure` script and the Makefile rules for building and
installation should not use any utilities directly except these:

```
awk cat cmp cp diff echo expr false grep install-info ln ls
mkdir mv printf pwd rm rmdir sed sleep sort tar test touch tr true
```

Compression programs such as `gzip` can be used in the
`dist` rule.

Generally, stick to the widely-supported (usually
POSIX-specified) options and features of these programs. For
example, don’t use ‘`mkdir -p`’, convenient as it may be, because a
few systems don’t support it at all and with others, it is not safe
for parallel execution. For a list of known incompatibilities, see
[Portable Shell Programming](https://www.gnu.org/software/autoconf/manual/autoconf.html#Portable-Shell) in Autoconf.

It is a good idea to avoid creating symbolic links in makefiles, since a
few file systems don’t support them.

The Makefile rules for building and installation can also use compilers
and related programs, but should do so via `make` variables so that the
user can substitute alternatives. Here are some of the programs we
mean:

```
ar bison cc flex install ld ldconfig lex
make makeinfo ranlib texi2dvi yacc
```

Use the following `make` variables to run those programs:

```
$(AR) $(BISON) $(CC) $(FLEX) $(INSTALL) $(LD) $(LDCONFIG) $(LEX)
$(MAKE) $(MAKEINFO) $(RANLIB) $(TEXI2DVI) $(YACC)
```

When you use `ranlib` or `ldconfig`, you should make sure
nothing bad happens if the system does not have the program in question.
Arrange to ignore an error from that command, and print a message before
the command to tell the user that failure of this command does not mean
a problem. (The Autoconf ‘`AC_PROG_RANLIB`’ macro can help with
this.)

If you use symbolic links, you should implement a fallback for systems
that don’t have symbolic links.

Additional utilities that can be used via Make variables are:

```
chgrp chmod chown mknod
```

It is ok to use other utilities in Makefile portions (or scripts)
intended only for particular systems where you know those utilities
exist.

---

Next: [`DESTDIR`: Support for Staged Installs](#DESTDIR), Previous: [Utilities in Makefiles](#Utilities-in-Makefiles), Up: [Makefile Conventions](#Makefile-Conventions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 16.3 Variables for Specifying Commands

Makefiles should provide variables for overriding certain commands, options,
and so on.

In particular, you should run most utility programs via variables.
Thus, if you use Bison, have a variable named `BISON` whose default
value is set with ‘`BISON = bison`’, and refer to it with
`$(BISON)` whenever you need to use Bison.

File management utilities such as `ln`, `rm`, `mv`, and
so on, need not be referred to through variables in this way, since users
don’t need to replace them with other programs.

Each program-name variable should come with an options variable that is
used to supply options to the program. Append ‘`FLAGS`’ to the
program-name variable name to get the options variable name—for
example, `BISONFLAGS`. (The names `CFLAGS` for the C
compiler, `YFLAGS` for yacc, and `LFLAGS` for lex, are
exceptions to this rule, but we keep them because they are standard.)
Use `CPPFLAGS` in any compilation command that runs the
preprocessor, and use `LDFLAGS` in any compilation command that
does linking as well as in any direct use of `ld`.

If there are C compiler options that *must* be used for proper
compilation of certain files, do not include them in `CFLAGS`.
Users expect to be able to specify `CFLAGS` freely themselves.
Instead, arrange to pass the necessary options to the C compiler
independently of `CFLAGS`, by writing them explicitly in the
compilation commands or by defining an implicit rule, like this:

```
CFLAGS = -g
ALL_CFLAGS = -I. $(CFLAGS)
.c.o:
        $(CC) -c $(CPPFLAGS) $(ALL_CFLAGS) $<
```

Do include the ‘`-g`’ option in `CFLAGS`, because that is not
*required* for proper compilation. You can consider it a default
that is only recommended. If the package is set up so that it is
compiled with GCC by default, then you might as well include ‘`-O`’
in the default value of `CFLAGS` as well.

Put `CFLAGS` last in the compilation command, after other variables
containing compiler options, so the user can use `CFLAGS` to
override the others.

`CFLAGS` should be used in every invocation of the C compiler,
both those which do compilation and those which do linking.

Every Makefile should define the variable `INSTALL`, which is the
basic command for installing a file into the system.

Every Makefile should also define the variables `INSTALL_PROGRAM`
and `INSTALL_DATA`. (The default for `INSTALL_PROGRAM` should
be `$(INSTALL)`; the default for `INSTALL_DATA` should be
`${INSTALL} -m 644`.) Then it should use those variables as the
commands for actual installation, for executables and non-executables
respectively. Minimal use of these variables is as follows:

```
$(INSTALL_PROGRAM) foo $(bindir)/foo
$(INSTALL_DATA) libfoo.a $(libdir)/libfoo.a
```

However, it is preferable to support a `DESTDIR` prefix on the
target files, as explained in the next section.

It is acceptable, but not required, to install multiple files in one
command, with the final argument being a directory, as in:

```
$(INSTALL_PROGRAM) foo bar baz $(bindir)
```

---

Next: [Variables for Installation Directories](#Directory-Variables), Previous: [Variables for Specifying Commands](#Command-Variables), Up: [Makefile Conventions](#Makefile-Conventions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 16.4 `DESTDIR`: Support for Staged Installs

`DESTDIR` is a variable prepended to each installed target file,
like this:

```
$(INSTALL_PROGRAM) foo $(DESTDIR)$(bindir)/foo
$(INSTALL_DATA) libfoo.a $(DESTDIR)$(libdir)/libfoo.a
```

The `DESTDIR` variable is specified by the user on the `make`
command line as an absolute file name. For example:

```
make DESTDIR=/tmp/stage install
```

`DESTDIR` should be supported only in the `install*` and
`uninstall*` targets, as those are the only targets where it is
useful.

If your installation step would normally install
`/usr/local/bin/foo` and `/usr/local/lib/libfoo.a`, then an
installation invoked as in the example above would install
`/tmp/stage/usr/local/bin/foo` and
`/tmp/stage/usr/local/lib/libfoo.a` instead.

Prepending the variable `DESTDIR` to each target in this way
provides for *staged installs*, where the installed files are not
placed directly into their expected location but are instead copied
into a temporary location (`DESTDIR`). However, installed files
maintain their relative directory structure and any embedded file names
will not be modified.

You should not set the value of `DESTDIR` in your `Makefile`
at all; then the files are installed into their expected locations by
default. Also, specifying `DESTDIR` should not change the
operation of the software in any way, so its value should not be
included in any file contents.

`DESTDIR` support is commonly used in package creation. It is
also helpful to users who want to understand what a given package will
install where, and to allow users who don’t normally have permissions
to install into protected areas to build and install before gaining
those permissions. Finally, it can be useful with tools such as
`stow`, where code is installed in one place but made to appear
to be installed somewhere else using symbolic links or special mount
operations. So, we strongly recommend GNU packages support
`DESTDIR`, though it is not an absolute requirement.

---

Next: [Standard Targets for Users](#Standard-Targets), Previous: [`DESTDIR`: Support for Staged Installs](#DESTDIR), Up: [Makefile Conventions](#Makefile-Conventions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 16.5 Variables for Installation Directories

Installation directories should always be named by variables, so it is
easy to install in a nonstandard place. The standard names for these
variables and the values they should have in GNU packages are
described below. They are based on a standard file system layout;
variants of it are used in GNU/Linux and other modern operating
systems.

Installers are expected to override these values when calling
`make` (e.g., `make prefix=/usr install`) or
`configure` (e.g., `configure --prefix=/usr`). GNU
packages should not try to guess which value should be appropriate for
these variables on the system they are being installed onto: use the
default settings specified here so that all GNU packages behave
identically, allowing the installer to achieve any desired layout.

All installation directories, and their parent directories, should be
created (if necessary) before they are installed into.

These first two variables set the root for the installation. All the
other installation directories should be subdirectories of one of
these two, and nothing should be directly installed into these two
directories.

`prefix` [¶](#index-prefix)
:   A prefix used in constructing the default values of the variables listed
    below. The default value of `prefix` should be `/usr/local`.
    When building the complete GNU system, the prefix will be empty and
    `/usr` will be a symbolic link to `/`.
    (If you are using Autoconf, write it as ‘`@prefix@`’.)

    Running ‘`make install`’ with a different value of `prefix` from
    the one used to build the program should *not* recompile the
    program.

`exec_prefix` [¶](#index-exec_005fprefix)
:   A prefix used in constructing the default values of some of the
    variables listed below. The default value of `exec_prefix` should
    be `$(prefix)`.
    (If you are using Autoconf, write it as ‘`@exec_prefix@`’.)

    Generally, `$(exec_prefix)` is used for directories that contain
    machine-specific files (such as executables and subroutine libraries),
    while `$(prefix)` is used directly for other directories.

    Running ‘`make install`’ with a different value of `exec_prefix`
    from the one used to build the program should *not* recompile the
    program.

Executable programs are installed in one of the following directories.

`bindir` [¶](#index-bindir)
:   The directory for installing executable programs that users can run.
    This should normally be `/usr/local/bin`, but write it as
    `$(exec_prefix)/bin`.
    (If you are using Autoconf, write it as ‘`@bindir@`’.)

`sbindir` [¶](#index-sbindir)
:   The directory for installing executable programs that can be run from
    the shell, but are only generally useful to system administrators. This
    should normally be `/usr/local/sbin`, but write it as
    `$(exec_prefix)/sbin`.
    (If you are using Autoconf, write it as ‘`@sbindir@`’.)

`libexecdir` [¶](#index-libexecdir)
:   The directory for installing executable programs to be run by other
    programs rather than by users. This directory should normally be
    `/usr/local/libexec`, but write it as `$(exec_prefix)/libexec`.
    (If you are using Autoconf, write it as ‘`@libexecdir@`’.)

    The definition of ‘`libexecdir`’ is the same for all packages, so
    you should install your data in a subdirectory thereof. Most packages
    install their data under `$(libexecdir)/package-name/`,
    possibly within additional subdirectories thereof, such as
    `$(libexecdir)/package-name/machine/version`.

Data files used by the program during its execution are divided into
categories in two ways.

* Some files are normally modified by programs; others are never normally
  modified (though users may edit some of these).
* Some files are architecture-independent and can be shared by all
  machines at a site; some are architecture-dependent and can be shared
  only by machines of the same kind and operating system; others may never
  be shared between two machines.

This makes for six different possibilities. However, we want to
discourage the use of architecture-dependent files, aside from object
files and libraries. It is much cleaner to make other data files
architecture-independent, and it is generally not hard.

Here are the variables Makefiles should use to specify directories
to put these various kinds of files in:

‘`datarootdir`’
:   The root of the directory tree for read-only architecture-independent
    data files. This should normally be `/usr/local/share`, but
    write it as `$(prefix)/share`. (If you are using Autoconf, write
    it as ‘`@datarootdir@`’.) ‘`datadir`’’s default value is
    based on this variable; so are ‘`infodir`’, ‘`mandir`’, and
    others.

‘`datadir`’
:   The directory for installing idiosyncratic read-only
    architecture-independent data files for this program. This is usually
    the same place as ‘`datarootdir`’, but we use the two separate
    variables so that you can move these program-specific files without
    altering the location for Info files, man pages, etc.

    This should normally be `/usr/local/share`, but write it as
    `$(datarootdir)`. (If you are using Autoconf, write it as
    ‘`@datadir@`’.)

    The definition of ‘`datadir`’ is the same for all packages, so you
    should install your data in a subdirectory thereof. Most packages
    install their data under `$(datadir)/package-name/`.

‘`sysconfdir`’
:   The directory for installing read-only data files that pertain to a
    single machine–that is to say, files for configuring a host. Mailer
    and network configuration files, `/etc/passwd`, and so forth belong
    here. All the files in this directory should be ordinary ASCII text
    files. This directory should normally be `/usr/local/etc`, but
    write it as `$(prefix)/etc`.
    (If you are using Autoconf, write it as ‘`@sysconfdir@`’.)

    Do not install executables here in this directory (they probably belong
    in `$(libexecdir)` or `$(sbindir)`). Also do not install
    files that are modified in the normal course of their use (programs
    whose purpose is to change the configuration of the system excluded).
    Those probably belong in `$(localstatedir)`.

‘`sharedstatedir`’
:   The directory for installing architecture-independent data files which
    the programs modify while they run. This should normally be
    `/usr/local/com`, but write it as `$(prefix)/com`.
    (If you are using Autoconf, write it as ‘`@sharedstatedir@`’.)

‘`localstatedir`’
:   The directory for installing data files which the programs modify while
    they run, and that pertain to one specific machine. Users should never
    need to modify files in this directory to configure the package’s
    operation; put such configuration information in separate files that go
    in `$(datadir)` or `$(sysconfdir)`. `$(localstatedir)`
    should normally be `/usr/local/var`, but write it as
    `$(prefix)/var`.
    (If you are using Autoconf, write it as ‘`@localstatedir@`’.)

‘`runstatedir`’
:   The directory for installing data files which the programs modify
    while they run, that pertain to one specific machine, and which need
    not persist longer than the execution of the program—which is
    generally long-lived, for example, until the next reboot. PID files
    for system daemons are a typical use. In addition, this directory
    should not be cleaned except perhaps at reboot, while the general
    `/tmp` (`TMPDIR`) may be cleaned arbitrarily. This should
    normally be `/var/run`, but write it as
    `$(localstatedir)/run`. Having it as a separate variable allows
    the use of `/run` if desired, for example. (If you are using
    Autoconf 2.70 or later, write it as ‘`@runstatedir@`’.)

These variables specify the directory for installing certain specific
types of files, if your program has them. Every GNU package should
have Info files, so every program needs ‘`infodir`’, but not all
need ‘`libdir`’ or ‘`lispdir`’.

‘`includedir`’
:   The directory for installing header files to be included by user
    programs with the C ‘`#include`’ preprocessor directive. This
    should normally be `/usr/local/include`, but write it as
    `$(prefix)/include`.
    (If you are using Autoconf, write it as ‘`@includedir@`’.)

    Most compilers other than GCC do not look for header files in directory
    `/usr/local/include`. So installing the header files this way is
    only useful with GCC. Sometimes this is not a problem because some
    libraries are only really intended to work with GCC. But some libraries
    are intended to work with other compilers. They should install their
    header files in two places, one specified by `includedir` and one
    specified by `oldincludedir`.

‘`oldincludedir`’
:   The directory for installing ‘`#include`’ header files for use with
    compilers other than GCC. This should normally be `/usr/include`.
    (If you are using Autoconf, you can write it as ‘`@oldincludedir@`’.)

    The Makefile commands should check whether the value of
    `oldincludedir` is empty. If it is, they should not try to use
    it; they should cancel the second installation of the header files.

    A package should not replace an existing header in this directory unless
    the header came from the same package. Thus, if your Foo package
    provides a header file `foo.h`, then it should install the header
    file in the `oldincludedir` directory if either (1) there is no
    `foo.h` there or (2) the `foo.h` that exists came from the Foo
    package.

    To tell whether `foo.h` came from the Foo package, put a magic
    string in the file—part of a comment—and `grep` for that string.

‘`docdir`’
:   The directory for installing documentation files (other than Info) for
    this package. By default, it should be
    `/usr/local/share/doc/yourpkg`, but it should be written as
    `$(datarootdir)/doc/yourpkg`. (If you are using Autoconf,
    write it as ‘`@docdir@`’.) The yourpkg subdirectory, which
    may include a version number, prevents collisions among files with
    common names, such as `README`.

‘`infodir`’
:   The directory for installing the Info files for this package. By
    default, it should be `/usr/local/share/info`, but it should be
    written as `$(datarootdir)/info`. (If you are using Autoconf,
    write it as ‘`@infodir@`’.) `infodir` is separate from
    `docdir` for compatibility with existing practice.

‘`htmldir`’

‘`dvidir`’

‘`pdfdir`’

‘`psdir`’
:   Directories for installing documentation files in the particular
    format. They should all be set to `$(docdir)` by default. (If
    you are using Autoconf, write them as ‘`@htmldir@`’,
    ‘`@dvidir@`’, etc.) Packages which supply several translations
    of their documentation should install them in
    ‘`$(htmldir)/`’ll, ‘`$(pdfdir)/`’ll, etc. where
    ll is a locale abbreviation such as ‘`en`’ or ‘`pt_BR`’.

‘`libdir`’
:   The directory for object files and libraries of object code. Do not
    install executables here, they probably ought to go in `$(libexecdir)`
    instead. The value of `libdir` should normally be
    `/usr/local/lib`, but write it as `$(exec_prefix)/lib`.
    (If you are using Autoconf, write it as ‘`@libdir@`’.)

‘`lispdir`’
:   The directory for installing any Emacs Lisp files in this package. By
    default, it should be `/usr/local/share/emacs/site-lisp`, but it
    should be written as `$(datarootdir)/emacs/site-lisp`.

    If you are using Autoconf, write the default as ‘`@lispdir@`’.
    In order to make ‘`@lispdir@`’ work, you need the following lines
    in your `configure.ac` file:

    ```
    lispdir='${datarootdir}/emacs/site-lisp'
    AC_SUBST(lispdir)
    ```

‘`localedir`’
:   The directory for installing locale-specific message catalogs for this
    package. By default, it should be `/usr/local/share/locale`, but
    it should be written as `$(datarootdir)/locale`. (If you are
    using Autoconf, write it as ‘`@localedir@`’.) This directory
    usually has a subdirectory per locale.

Unix-style man pages are installed in one of the following:

‘`mandir`’
:   The top-level directory for installing the man pages (if any) for this
    package. It will normally be `/usr/local/share/man`, but you
    should write it as `$(datarootdir)/man`. (If you are using
    Autoconf, write it as ‘`@mandir@`’.)

‘`man1dir`’
:   The directory for installing section 1 man pages. Write it as
    `$(mandir)/man1`.

‘`man2dir`’
:   The directory for installing section 2 man pages. Write it as
    `$(mandir)/man2`

‘`…`’
:   **Don’t make the primary documentation for any GNU software be a
    man page. Write a manual in Texinfo instead. Man pages are just for
    the sake of people running GNU software on Unix, which is a secondary
    application only.**

‘`manext`’
:   The file name extension for the installed man page. This should contain
    a period followed by the appropriate digit; it should normally be ‘`.1`’.

‘`man1ext`’
:   The file name extension for installed section 1 man pages.

‘`man2ext`’
:   The file name extension for installed section 2 man pages.

‘`…`’
:   Use these names instead of ‘`manext`’ if the package needs to install man
    pages in more than one section of the manual.

And finally, you should set the following variable:

‘`srcdir`’
:   The directory for the sources being compiled. The value of this
    variable is normally inserted by the `configure` shell script.
    (If you are using Autoconf, use ‘`srcdir = @srcdir@`’.)

For example:

```
# Common prefix for installation directories.
# NOTE: This directory must exist when you start the install.
prefix = /usr/local
datarootdir = $(prefix)/share
datadir = $(datarootdir)
exec_prefix = $(prefix)
# Where to put the executable for the command 'gcc'.
bindir = $(exec_prefix)/bin
# Where to put the directories used by the compiler.
libexecdir = $(exec_prefix)/libexec
# Where to put the Info files.
infodir = $(datarootdir)/info
```

If your program installs a large number of files into one of the
standard user-specified directories, it might be useful to group them
into a subdirectory particular to that program. If you do this, you
should write the `install` rule to create these subdirectories.

Do not expect the user to include the subdirectory name in the value of
any of the variables listed above. The idea of having a uniform set of
variable names for installation directories is to enable the user to
specify the exact same values for several different GNU packages. In
order for this to be useful, all the packages must be designed so that
they will work sensibly when the user does so.

At times, not all of these variables may be implemented in the current
release of Autoconf and/or Automake; but as of Autoconf 2.60, we
believe all of them are. When any are missing, the descriptions here
serve as specifications for what Autoconf will implement. As a
programmer, you can either use a development version of Autoconf or
avoid using these variables until a stable release is made which
supports them.

---

Next: [Install Command Categories](#Install-Command-Categories), Previous: [Variables for Installation Directories](#Directory-Variables), Up: [Makefile Conventions](#Makefile-Conventions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 16.6 Standard Targets for Users

All GNU programs should have the following targets in their Makefiles:

‘`all`’
:   Compile the entire program. This should be the default target. This
    target need not rebuild any documentation files; Info files should
    normally be included in the distribution, and DVI (and other
    documentation format) files should be made only when explicitly asked
    for.

    By default, the Make rules should compile and link with ‘`-g`’, so
    that executable programs have debugging symbols. Otherwise, you are
    essentially helpless in the face of a crash, and it is often far from
    easy to reproduce with a fresh build.

‘`install`’
:   Compile the program and copy the executables, libraries, and so on to
    the file names where they should reside for actual use. If there is a
    simple test to verify that a program is properly installed, this target
    should run that test.

    Do not strip executables when installing them. This helps eventual
    debugging that may be needed later, and nowadays disk space is cheap
    and dynamic loaders typically ensure debug sections are not loaded during
    normal execution. Users that need stripped binaries may invoke the
    `install-strip` target to do that.

    If possible, write the `install` target rule so that it does not
    modify anything in the directory where the program was built, provided
    ‘`make all`’ has just been done. This is convenient for building the
    program under one user name and installing it under another.

    The commands should create all the directories in which files are to be
    installed, if they don’t already exist. This includes the directories
    specified as the values of the variables `prefix` and
    `exec_prefix`, as well as all subdirectories that are needed.
    One way to do this is by means of an `installdirs` target
    as described below.

    Use ‘`-`’ before any command for installing a man page, so that
    `make` will ignore any errors. This is in case there are systems
    that don’t have the Unix man page documentation system installed.

    The way to install Info files is to copy them into `$(infodir)`
    with `$(INSTALL_DATA)` (see [Variables for Specifying Commands](#Command-Variables)), and then run
    the `install-info` program if it is present. `install-info`
    is a program that edits the Info `dir` file to add or update the
    menu entry for the given Info file; it is part of the Texinfo package.

    Here is a sample rule to install an Info file that also tries to
    handle some additional situations, such as `install-info` not
    being present.

    ```
    do-install-info: foo.info installdirs
            $(NORMAL_INSTALL)
    # Prefer an info file in . to one in srcdir.
            if test -f foo.info; then d=.; \
             else d="$(srcdir)"; fi; \
            $(INSTALL_DATA) $$d/foo.info \
              "$(DESTDIR)$(infodir)/foo.info"
    # Run install-info only if it exists.
    # Use 'if' instead of just prepending '-' to the
    # line so we notice real errors from install-info.
    # Use '$(SHELL) -c' because some shells do not
    # fail gracefully when there is an unknown command.
            $(POST_INSTALL)
            if $(SHELL) -c 'install-info --version' \
               >/dev/null 2>&1; then \
              install-info --dir-file="$(DESTDIR)$(infodir)/dir" \
                           "$(DESTDIR)$(infodir)/foo.info"; \
            else true; fi
    ```

    When writing the `install` target, you must classify all the
    commands into three categories: normal ones, *pre-installation*
    commands and *post-installation* commands. See [Install Command Categories](#Install-Command-Categories).

‘`install-html`’

‘`install-dvi`’

‘`install-pdf`’

‘`install-ps`’
:   These targets install documentation in formats other than Info;
    they’re intended to be called explicitly by the person installing the
    package, if that format is desired. GNU prefers Info files, so these
    must be installed by the `install` target.

    When you have many documentation files to install, we recommend that
    you avoid collisions and clutter by arranging for these targets to
    install in subdirectories of the appropriate installation directory,
    such as `htmldir`. As one example, if your package has multiple
    manuals, and you wish to install HTML documentation with many files
    (such as the “split” mode output by `makeinfo --html`), you’ll
    certainly want to use subdirectories, or two nodes with the same name
    in different manuals will overwrite each other.

    Please make these `install-format` targets invoke the
    commands for the format target, for example, by making
    format a dependency.

‘`uninstall`’
:   Delete all the installed files—the copies that the ‘`install`’
    and ‘`install-*`’ targets create.

    This rule should not modify the directories where compilation is done,
    only the directories where files are installed.

    The uninstallation commands are divided into three categories, just like
    the installation commands. See [Install Command Categories](#Install-Command-Categories).

‘`install-strip`’
:   Like `install`, but strip the executable files while installing
    them. In simple cases, this target can use the `install` target in
    a simple way:

    ```
    install-strip:
            $(MAKE) INSTALL_PROGRAM='$(INSTALL_PROGRAM) -s' \
                    install
    ```

    But if the package installs scripts as well as real executables, the
    `install-strip` target can’t just refer to the `install`
    target; it has to strip the executables but not the scripts.

    `install-strip` should not strip the executables in the build
    directory which are being copied for installation. It should only strip
    the copies that are installed.

    Normally we do not recommend stripping an executable unless you are sure
    the program has no bugs. However, it can be reasonable to install a
    stripped executable for actual execution while saving the unstripped
    executable elsewhere in case there is a bug.

‘`clean`’
:   Delete all files in the current directory that are normally created by
    building the program. Also delete files in other directories if they
    are created by this makefile. However, don’t delete the files that
    record the configuration. Also preserve files that could be made by
    building, but normally aren’t because the distribution comes with
    them. There is no need to delete parent directories that were created
    with ‘`mkdir -p`’, since they could have existed anyway.

    Delete `.dvi` files here if they are not part of the distribution.

‘`distclean`’
:   Delete all files in the current directory (or created by this
    makefile) that are created by configuring or building the program. If
    you have unpacked the source and built the program without creating
    any other files, ‘`make distclean`’ should leave only the files
    that were in the distribution. However, there is no need to delete
    parent directories that were created with ‘`mkdir -p`’, since they
    could have existed anyway.

‘`mostlyclean`’
:   Like ‘`clean`’, but may refrain from deleting a few files that people
    normally don’t want to recompile. For example, the ‘`mostlyclean`’
    target for GCC does not delete `libgcc.a`, because recompiling it
    is rarely necessary and takes a lot of time.

‘`maintainer-clean`’
:   Delete almost everything that can be reconstructed with this Makefile.
    This typically includes everything deleted by `distclean`, plus
    more: C source files produced by Bison, tags tables, Info files, and
    so on.

    The reason we say “almost everything” is that running the command
    ‘`make maintainer-clean`’ should not delete `configure` even
    if `configure` can be remade using a rule in the Makefile. More
    generally, ‘`make maintainer-clean`’ should not delete anything
    that needs to exist in order to run `configure` and then begin to
    build the program. Also, there is no need to delete parent
    directories that were created with ‘`mkdir -p`’, since they could
    have existed anyway. These are the only exceptions;
    `maintainer-clean` should delete everything else that can be
    rebuilt.

    The ‘`maintainer-clean`’ target is intended to be used by a maintainer of
    the package, not by ordinary users. You may need special tools to
    reconstruct some of the files that ‘`make maintainer-clean`’ deletes.
    Since these files are normally included in the distribution, we don’t
    take care to make them easy to reconstruct. If you find you need to
    unpack the full distribution again, don’t blame us.

    To help make users aware of this, the commands for the special
    `maintainer-clean` target should start with these two:

    ```
    @echo 'This command is intended for maintainers to use; it'
    @echo 'deletes files that may need special tools to rebuild.'
    ```

‘`TAGS`’
:   Update a tags table for this program.

‘`info`’
:   Generate any Info files needed. The best way to write the rules is as
    follows:

    ```
    info: foo.info

    foo.info: foo.texi chap1.texi chap2.texi
            $(MAKEINFO) $(srcdir)/foo.texi
    ```

    You must define the variable `MAKEINFO` in the Makefile. It should
    run the `makeinfo` program, which is part of the Texinfo
    distribution.

    Normally a GNU distribution comes with Info files, and that means the
    Info files are present in the source directory. Therefore, the Make
    rule for an info file should update it in the source directory. When
    users build the package, ordinarily Make will not update the Info files
    because they will already be up to date.

‘`dvi`’

‘`html`’

‘`pdf`’

‘`ps`’
:   Generate documentation files in the given format. These targets
    should always exist, but any or all can be a no-op if the given output
    format cannot be generated. These targets should not be dependencies
    of the `all` target; the user must manually invoke them.

    Here’s an example rule for generating DVI files from Texinfo:

    ```
    dvi: foo.dvi

    foo.dvi: foo.texi chap1.texi chap2.texi
            $(TEXI2DVI) $(srcdir)/foo.texi
    ```

    You must define the variable `TEXI2DVI` in the Makefile. It
    should run the program `texi2dvi`, which is part of the Texinfo
    distribution. (`texi2dvi` uses TeX to do the real work of
    formatting. TeX is not distributed with Texinfo.) Alternatively,
    write only the dependencies, and allow GNU `make` to provide the
    command.

    Here’s another example, this one for generating HTML from Texinfo:

    ```
    html: foo.html

    foo.html: foo.texi chap1.texi chap2.texi
            $(TEXI2HTML) $(srcdir)/foo.texi
    ```

    Again, you would define the variable `TEXI2HTML` in the Makefile;
    for example, it might run `makeinfo --no-split --html`
    (`makeinfo` is part of the Texinfo distribution).

‘`dist`’
:   Create a distribution tar file for this program. The tar file should be
    set up so that the file names in the tar file start with a subdirectory
    name which is the name of the package it is a distribution for. This
    name can include the version number.

    For example, the distribution tar file of GCC version 1.40 unpacks into
    a subdirectory named `gcc-1.40`.

    The easiest way to do this is to create a subdirectory appropriately
    named, use `ln` or `cp` to install the proper files in it, and
    then `tar` that subdirectory.

    Compress the tar file with `gzip`. For example, the actual
    distribution file for GCC version 1.40 is called `gcc-1.40.tar.gz`.
    It is ok to support other free compression formats as well.

    The `dist` target should explicitly depend on all non-source files
    that are in the distribution, to make sure they are up to date in the
    distribution.
    See [Making Releases](https://www.gnu.org/prep/standards/standards.html#Releases) in GNU Coding Standards.

‘`check`’
:   Perform self-tests (if any). The user must build the program before
    running the tests, but need not install the program; you should write
    the self-tests so that they work when the program is built but not
    installed.

The following targets are suggested as conventional names, for programs
in which they are useful.

`installcheck`
:   Perform installation tests (if any). The user must build and install
    the program before running the tests. You should not assume that
    `$(bindir)` is in the search path.

`installdirs`
:   It’s useful to add a target named ‘`installdirs`’ to create the
    directories where files are installed, and their parent directories.
    There is a script called `mkinstalldirs` which is convenient for
    this; you can find it in the Gnulib package.
    You can use a rule like this:

    ```
    # Make sure all installation directories (e.g. $(bindir))
    # actually exist by making them if necessary.
    installdirs: mkinstalldirs
            $(srcdir)/mkinstalldirs $(bindir) $(datadir) \
                                    $(libdir) $(infodir) \
                                    $(mandir)
    ```

    or, if you wish to support `DESTDIR` (strongly encouraged),

    ```
    # Make sure all installation directories (e.g. $(bindir))
    # actually exist by making them if necessary.
    installdirs: mkinstalldirs
            $(srcdir)/mkinstalldirs \
                $(DESTDIR)$(bindir) $(DESTDIR)$(datadir) \
                $(DESTDIR)$(libdir) $(DESTDIR)$(infodir) \
                $(DESTDIR)$(mandir)
    ```

    This rule should not modify the directories where compilation is done.
    It should do nothing but create installation directories.

---

Previous: [Standard Targets for Users](#Standard-Targets), Up: [Makefile Conventions](#Makefile-Conventions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]
