### 20.1 Usage of Conditionals [¶](#Usage-of-Conditionals-1)

Before using a conditional, you must define it by using
`AM_CONDITIONAL` in the `configure.ac` file (see [Autoconf macros supplied with Automake](#Macros)).

Macro: **AM\_CONDITIONAL** (conditional, condition) [¶](#index-AM_005fCONDITIONAL-2)
:   The conditional name, conditional, should be a simple string
    starting with a letter and containing only letters, digits, and
    underscores. It must be different from ‘`TRUE`’ and ‘`FALSE`’,
    which are reserved by Automake.

    The shell condition (suitable for use in a shell `if`
    statement) is evaluated when `configure` is run. Note that you
    must arrange for *every* `AM_CONDITIONAL` to be invoked every
    time `configure` is run. If `AM_CONDITIONAL` is run
    conditionally (e.g., in a shell `if` statement), then the result
    will confuse `automake`.

    For portability, it is best to use shell operators `&&` and
    `||` and parentheses, when constructing a compound
    condition using the `test` command, and not the
    `-a` and `-o` options and parentheses as options to
    `test`, all of which have been marked obsolescent by POSIX
    (<https://pubs.opengroup.org/onlinepubs/9699919799/utilities/test.html#tag_20_128_161>).
    The name `test` is also more portable than `[`.
    See [Limitations of Builtins](https://www.gnu.org/software/autoconf/manual/autoconf.html#Limitations-of-Builtins) in The Autoconf Manual.

Conditionals typically depend upon options that the user provides to
the `configure` script. Here is an example of how to write a
conditional that is true if the user uses the `--enable-debug`
option.

```
AC_ARG_ENABLE([debug],
[  --enable-debug    Turn on debugging],
[case "${enableval}" in
  yes) debug=true ;;
  no)  debug=false ;;
  *) AC_MSG_ERROR([bad value ${enableval} for --enable-debug]) ;;
esac],[debug=false])
AM_CONDITIONAL([DEBUG], [test x$debug = xtrue])
```

Here is an example of how to use that conditional in `Makefile.am`:

```
if DEBUG
DBG = debug
else
DBG =
endif
noinst_PROGRAMS = $(DBG)
```

This trivial example could also be handled using `EXTRA_PROGRAMS`
(see [Conditional compilation of programs](#Conditional-Programs)).

You may only test a single variable in an `if` statement, possibly
negated using ‘`!`’. The `else` statement may be omitted.
Conditionals may be nested to any depth. You may specify an argument to
`else` in which case it must be the negation of the condition used
for the current `if`. Similarly you may specify the condition
that is closed on the `endif` line:

```
if DEBUG
DBG = debug
else !DEBUG
DBG =
endif !DEBUG
```

Unbalanced conditions are errors. The `if`, `else`, and
`endif` statements should not be indented, i.e., start on column
one.

The `else` branch of the above two examples could be omitted,
since assigning the empty string to an otherwise undefined variable
makes no difference.

In order to allow access to the condition registered by
`AM_CONDITIONAL` inside `configure.ac`, and to allow
conditional `AC_CONFIG_FILES`, `AM_COND_IF` may be used:

Macro: **AM\_COND\_IF** (conditional, [if-true], [if-false]) [¶](#index-AM_005fCOND_005fIF-2)
:   If conditional is fulfilled, execute if-true, otherwise
    execute if-false. If either branch contains `AC_CONFIG_FILES`,
    it will cause `automake` to output the rules for the respective
    files only for the given condition.

`AM_COND_IF` macros may be nested when M4 quotation is used
properly (see [M4 Quotation](https://www.gnu.org/software/autoconf/manual/autoconf.html#M4-Quotation) in The Autoconf Manual).

Here is an example of how to define a conditional config file:

```
AM_CONDITIONAL([SHELL_WRAPPER], [test "x$with_wrapper" = xtrue])
AM_COND_IF([SHELL_WRAPPER],
           [AC_CONFIG_FILES([wrapper:wrapper.in])])
```

---

Previous: [Usage of Conditionals](#Usage-of-Conditionals), Up: [Conditionals](#Conditionals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 20.2 Limits of Conditionals [¶](#Limits-of-Conditionals-1)

Conditionals should enclose complete statements like variables or
rules definitions. Automake cannot deal with conditionals used inside
a variable definition, for instance, and is not even able to diagnose
this situation. The following example would not work:

```
# This syntax is not understood by Automake
AM_CPPFLAGS = \
  -DFEATURE_A \
if WANT_DEBUG
  -DDEBUG \
endif
  -DFEATURE_B
```

However the intended definition of `AM_CPPFLAGS` can be achieved
with

```
if WANT_DEBUG
  DEBUGFLAGS = -DDEBUG
endif
AM_CPPFLAGS = -DFEATURE_A $(DEBUGFLAGS) -DFEATURE_B
```

or

```
AM_CPPFLAGS = -DFEATURE_A
if WANT_DEBUG
AM_CPPFLAGS += -DDEBUG
endif
AM_CPPFLAGS += -DFEATURE_B
```

More details and examples of conditionals are described alongside
various Automake features in this manual (see [Conditional Subdirectories](#Conditional-Subdirectories), see [Conditional compilation of sources](#Conditional-Sources), see [Conditional compilation of programs](#Conditional-Programs), see [Building Libtool Libraries Conditionally](#Conditional-Libtool-Libraries), see [Libtool Libraries with Conditional Sources](#Conditional-Libtool-Sources)).

---

Next: [When Automake Isn’t Enough](#Not-Enough), Previous: [Conditionals](#Conditionals), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 21 Silencing `make` [¶](#Silencing-make)

* [Make is verbose by default](#Make-verbosity)
* [Standard and generic ways to silence Make](#Tricks-For-Silencing-Make)
* [How Automake can help in silencing Make](#Automake-Silent-Rules)
* [Unsilencing Automake](#Unsilencing-Automake)

---

Next: [Standard and generic ways to silence Make](#Tricks-For-Silencing-Make), Up: [Silencing `make`](#Silencing-Make)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 21.1 Make is verbose by default [¶](#Make-is-verbose-by-default)

Normally, when executing the set of rules associated with a target,
`make` prints each rule before it is executed. This behavior,
despite having been in place since the beginning of `make`,
and being mandated by the POSIX standard, starkly violates the
“silence is golden” UNIX principle[5](#FOOT5):

> When a program has nothing interesting or surprising to say, it should
> say nothing. Well-behaved Unix programs do their jobs unobtrusively,
> with a minimum of fuss and bother. Silence is golden.

The traditional verbosity of `make` is understandable, as it
is useful, often necessary, in order to understand reasons of
failures. However, it can also hide warning and error messages from
`make`-invoked tools, drowning them in uninteresting and
seldom useful messages, and thus allowing them to easily go
undetected.

This can be quite problematic, especially for developers, who usually
know quite well what’s going on behind the scenes, and for whom the
verbose output from `make` ends up being mostly noise that
hampers the easy detection of potentially important warning messages.

So Automake provides some support for silencing `make`.

---

Next: [How Automake can help in silencing Make](#Automake-Silent-Rules), Previous: [Make is verbose by default](#Make-verbosity), Up: [Silencing `make`](#Silencing-Make)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 21.2 Standard and generic ways to silence Make [¶](#Standard-and-generic-ways-to-silence-Make)

Here we describe some common idioms/tricks to obtain a quieter
`make` output, with their relative advantages and drawbacks.
In the next section ([How Automake can help in silencing Make](#Automake-Silent-Rules)) we’ll see how
Automake can help in this respect, providing more elaborate and
flexible idioms.

* `make -s`

  This simply causes `make` not to print *any* rule before
  executing it.

  The `-s` flag is mandated by POSIX, universally supported, and
  its purpose and function are easy to understand.

  But it also has serious limitations. First of all, it embodies an
  “all or nothing” strategy, i.e., either everything is silenced, or
  nothing is; in practice, this lack of granularity makes it unsuitable
  as a general solution. When the `-s` flag is used, the
  `make` output might turn out to be too terse; in case of
  errors, the user won’t be able to easily see what rule or command have
  caused them, or even, in case of tools with poor error reporting, what
  the errors were.
* `make >/dev/null || make`

  Apparently, this perfectly obeys the “silence is golden” rule: warnings
  from stderr are passed through, output reporting is done only in case of
  error, and in that case it should provide a verbose-enough report to allow
  an easy determination of the error location and causes.

  However, calling `make` two times in a row might hide errors
  (especially intermittent ones), or subtly change the expected
  semantics of the `make` calls—these things can clearly make
  debugging and error assessment very difficult.
* `make --no-print-directory`

  This is GNU `make` specific. When called with the
  `--no-print-directory` option, GNU `make` will disable
  printing of the working directory by invoked sub-`make`s (the
  well-known “*Entering/Leaving directory …*” messages). This
  helps to decrease the verbosity of the output, but experience has
  shown that it can also often render debugging considerably harder in
  projects using deeply-nested `make` recursion.

  As an aside, the `--no-print-directory` option is automatically
  activated if the `-s` flag is used.

---

Next: [Unsilencing Automake](#Unsilencing-Automake), Previous: [Standard and generic ways to silence Make](#Tricks-For-Silencing-Make), Up: [Silencing `make`](#Silencing-Make)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 21.3 How Automake can help in silencing Make [¶](#How-Automake-can-help-in-silencing-Make)

The tricks and idioms for silencing `make` described in the
previous section can be useful from time to time, but we’ve seen that
they all have their serious drawbacks and limitations. That’s why
automake provides support for a more advanced and flexible way of
obtaining quieter output from `make` (for most rules at least).

To give the gist of what Automake can do in this respect, here is a simple
comparison between a typical `make` output (where silent rules
are disabled) and one with silent rules enabled:

```
% cat Makefile.am
bin_PROGRAMS = foo
foo_SOURCES = main.c func.c
% cat main.c
int main (void) { return func (); }  /* func used undeclared */
% cat func.c
int func (void) { int i; return i; } /* i used uninitialized */

The make output is by default very verbose.  This causes warnings
from the compiler to be somewhat hidden, and not immediate to spot.
% make CFLAGS=-Wall
gcc -DPACKAGE_NAME=\"foo\" -DPACKAGE_TARNAME=\"foo\" ...
-DPACKAGE_STRING=\"foo\ 1.0\" -DPACKAGE_BUGREPORT=\"\" ...
-DPACKAGE=\"foo\" -DVERSION=\"1.0\" -I. -Wall -MT main.o
-MD -MP -MF .deps/main.Tpo -c -o main.o main.c
main.c: In function ‘main’:
main.c:3:3: warning: implicit declaration of function ‘func’
mv -f .deps/main.Tpo .deps/main.Po
gcc -DPACKAGE_NAME=\"foo\" -DPACKAGE_TARNAME=\"foo\" ...
-DPACKAGE_STRING=\"foo\ 1.0\" -DPACKAGE_BUGREPORT=\"\" ...
-DPACKAGE=\"foo\" -DVERSION=\"1.0\" -I. -Wall -MT func.o
-MD -MP -MF .deps/func.Tpo -c -o func.o func.c
func.c: In function ‘func’:
func.c:4:3: warning: ‘i’ used uninitialized in this function
mv -f .deps/func.Tpo .deps/func.Po
gcc -Wall -o foo main.o func.o

Clean up, so that we can rebuild everything from scratch.
% make clean
test -z "foo" || rm -f foo
rm -f *.o

Silent rules enabled: the output is minimal but informative.
The warnings from the compiler stick out very clearly.
% make V=0 CFLAGS=-Wall
  CC     main.o
main.c: In function ‘main’:
main.c:3:3: warning: implicit declaration of function ‘func’
  CC     func.o
func.c: In function ‘func’:
func.c:4:3: warning: ‘i’ used uninitialized in this function
  CCLD   foo
```

Also, in projects using Libtool, the use of silent rules can
automatically enable `libtool`’s `--silent` option:

```
% cat Makefile.am
lib_LTLIBRARIES = libx.la

% make # Both make and libtool are verbose by default.
...
libtool: compile: gcc -DPACKAGE_NAME=\"foo\" ... -DLT_OBJDIR=\".libs/\"
  -I. -g -O2 -MT libx.lo -MD -MP -MF .deps/libx.Tpo -c libx.c -fPIC
  -DPIC -o .libs/libx.o
mv -f .deps/libx.Tpo .deps/libx.Plo
/bin/sh ./libtool --tag=CC --mode=link gcc -g -O2 -o libx.la -rpath
  /usr/local/lib libx.lo
libtool: link: gcc -shared .libs/libx.o -Wl,-soname -Wl,libx.so.0
  -o .libs/libx.so.0.0.0
libtool: link: cd .libs && rm -f libx.so && ln -s libx.so.0.0.0 libx.so
...

% make V=0
  CC     libx.lo
  CCLD   libx.la
```

For Automake-generated `Makefile`s, the user may influence the
verbosity at `configure` run time as well as at `make`
run time:

* Passing `--enable-silent-rules` to `configure` will cause
  build rules to be less verbose; the option `--disable-silent-rules`
  will cause normal verbose output.
* At `make` run time, the default chosen at `configure`
  time may be overridden: `make V=1` will produce verbose output,
  `make V=0` less verbose output.

  Unfortunately, if `V` is assigned a value other than 0 or 1,
  errors will result. This is problematic when a third-party program or
  library is built in the same tree and also uses the make variable
  `V`, with different values. The best workaround is probably to
  set `AM_V_P=true` (or similar), either on the make command line
  or in the `V`-using project’s `Makefile.am`. (For more
  discussion, see <https://bugs.gnu.org/20077>.)

Silent rules are *disabled* by default; the user must enable them
explicitly at either `configure` run time or at `make`
run time. We think that this is a good policy, since it provides the
casual user with enough information to prepare a good bug report in
case anything breaks.

Notwithstanding those rationales, developers who want to enable silent
rules by default in their own packages can do so by calling
`AM_SILENT_RULES([yes])` in `configure.ac`.

Analogously, users who prefer to have silent rules enabled by default
for everything on their system can edit their `config.site` file
to make the variable `enable_silent_rules` default to ‘`yes`’.
This still allows disabling silent rules at `configure` time
and at `make` time.

To work best, the current implementation of this feature normally uses
nested variable expansion ‘`$(var1$(V))`’, a `Makefile`
feature that is not required by POSIX 2008 but is widely supported in
practice. On the rare `make` implementations that do not
support nested variable expansion, whether rules are silent is always
determined at configure time, and cannot be overridden at make time.
Future versions of POSIX are likely to require nested variable
expansion, so this minor limitation should go away with time.

To extend the silent mode to your own rules, you have a few choices:

* You can use the predefined variable `AM_V_GEN` as a prefix to
  commands that should output a status line in silent mode, and
  `AM_V_at` as a prefix to commands that should not output anything
  in silent mode. When output is to be verbose, both of these variables
  will expand to the empty string.
* You can silence a recipe unconditionally with `@`, and then use
  the predefined variable `AM_V_P` to know whether make is being run
  in silent or verbose mode; adjust the verbose information your recipe
  displays accordingly. For example:

  ```
  generate-headers:
          @set -e; \
          ... [commands defining shell variable '$headers'] ...; \
          if $(AM_V_P); then set -x; else echo " GEN   [headers]"; fi; \
          rm -f $$headers && generate-header --flags $$headers
  ```

  `AM_V_P` is (must be) always set to a simple command, not needing
  shell quoting, typically either `:` or `true` or
  `false`.
* You can add your own variables, so strings of your own choice are shown.
  The following snippet shows how you would define your own equivalent of
  `AM_V_GEN`, say a string ‘`PKG-GEN`’:

  ```
  pkg_verbose = $(pkg_verbose_@AM_V@)
  pkg_verbose_ = $(pkg_verbose_@AM_DEFAULT_V@)
  pkg_verbose_0 = @echo PKG-GEN $@;

  foo: foo.in
          $(pkg_verbose)cp $(srcdir)/foo.in $@
  ```

Even when silent rules are enabled, the `--no-print-directory`
option is still required with GNU `make` if the
“*Entering/Leaving directory …*” messages are to be elided.

---

Previous: [How Automake can help in silencing Make](#Automake-Silent-Rules), Up: [Silencing `make`](#Silencing-Make)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 21.4 Unsilencing Automake [¶](#Unsilencing-Automake-1)

With the `AM_SILENT_RULES` macro described in the previous
section, Automake does a good job reducing `make` output to a
bare minimum. Sometimes you want to see more than that, especially
when debugging. Let’s summarize ways to get more information out of
Automake packages:

* Running `make V=1` will produce generally verbose output.
* Adding `AM_V_GEN= AM_V_at=` will unsilence more rules. Thus, in all:
  `make V=1 AM_V_GEN= AM_V_at=`.
* Even this will not unsilence everything. To see the real truth of what
  gets executed, resort to GNU Make’s debugging feature: `make
  --debug=p ... otherargs ...`. This reports every command being
  run, ignoring the `@` prefix on rules (which is what silences
  them). In the case of Automake, these commands are generally complex
  shell constructs, and you’ll want to track down the source files in
  Automake to actually understand them; but at least you’ll have the
  text to search for. You may wish to include other debugging options.
  See [Options Summary](https://www.gnu.org/software/make/manual/make.html#Options-Summary) in The GNU Make Manual.

---

Next: [Distributing `Makefile.in`s](#Distributing), Previous: [Silencing `make`](#Silencing-Make), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 22 When Automake Isn’t Enough [¶](#When-Automake-Isn_0027t-Enough)

In some situations, where Automake is not up to one task, one has to
resort to handwritten rules or even handwritten `Makefile`s.

* [Extending Automake Rules](#Extending)
* [Third-Party `Makefile`s](#Third_002dParty-Makefiles)

---

Next: [Third-Party `Makefile`s](#Third_002dParty-Makefiles), Up: [When Automake Isn’t Enough](#Not-Enough)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 22.1 Extending Automake Rules [¶](#Extending-Automake-Rules)

With some minor exceptions (for example `_PROGRAMS` variables,
`TESTS`, or `XFAIL_TESTS`) being rewritten to append
‘`$(EXEEXT)`’), the contents of a `Makefile.am` is copied to
`Makefile.in` verbatim.

These copying semantics mean that many problems can be worked around
by simply adding some `make` variables and rules to
`Makefile.am`. Automake will ignore these additions.

Since a `Makefile.in` is built from data gathered from three
different places (`Makefile.am`, `configure.ac`, and
`automake` itself), it is possible to have conflicting
definitions of rules or variables. When building `Makefile.in`
the following priorities are respected by `automake` to ensure
the user always has the last word:

* User defined variables in `Makefile.am` have priority over
  variables `AC_SUBST`ed from `configure.ac`, and
  `AC_SUBST`ed variables have priority over
  `automake`-defined variables.
* As far as rules are concerned, a user-defined rule overrides any
  `automake`-defined rule for the same target.

These overriding semantics make it possible to fine tune some default
settings of Automake, or replace some of its rules. Overriding
Automake rules is often inadvisable, particularly in the topmost
directory of a package with subdirectories. The `-Woverride`
option (see [Creating a `Makefile.in`: Invoking `automake`](#automake-Invocation)) comes in handy to catch overridden
definitions.

Note that Automake does not make any distinction between rules with
commands and rules that only specify dependencies. So it is not
possible to append new dependencies to an `automake`-defined
target without redefining the entire rule.

However, various useful targets have a ‘`-local`’ version you can
specify in your `Makefile.am`. Automake will supplement the
standard target with these user-supplied targets.

The targets that support a local version are `all`, `info`,
`dvi`, `ps`, `pdf`, `html`, `check`,
`install-data`, `install-dvi`, `install-exec`,
`install-html`, `install-info`, `install-pdf`,
`install-ps`, `uninstall`, `installdirs`,
`installcheck` and the various `clean` targets
(`mostlyclean`, `clean`, `distclean`, and
`maintainer-clean`).

Note that there are no `uninstall-exec-local` or
`uninstall-data-local` targets; just use `uninstall-local`.
It doesn’t make sense to uninstall just data or just executables.

For instance, here is one way to erase a subdirectory during
‘`make clean`’ (see [What Gets Cleaned](#Clean)).

```
clean-local:
        -rm -rf testSubDir
```

You may be tempted to use `install-data-local` to install a file
to some hard-coded location, but you should avoid this
(see [Installing to Hard-Coded Locations](#Hard_002dCoded-Install-Paths)).

With the `-local` targets, there is no particular guarantee of
execution order; typically, they are run early, but with parallel
make, there is no way to be sure of that.

In contrast, some rules also have a way to run another rule, called a
*hook*; hooks are always executed after the main rule’s work is done.
The hook is named after the principal target, with ‘`-hook`’ appended.
The targets allowing hooks are `install-data`,
`install-exec`, `uninstall`, `dist`, and
`distcheck`.

For instance, here is how to create a hard link to an installed program:

```
install-exec-hook:
        ln $(DESTDIR)$(bindir)/program$(EXEEXT) \
           $(DESTDIR)$(bindir)/proglink$(EXEEXT)
```

Although cheaper and more portable than symbolic links, hard links
will not work everywhere (for instance, OS/2 does not have
`ln`). Ideally you should fall back to ‘`cp -p`’ when
`ln` does not work. An easy way, if symbolic links are
acceptable to you, is to add `AC_PROG_LN_S` to
`configure.ac` (see [Particular Program
Checks](https://www.gnu.org/software/autoconf/manual/autoconf.html#Particular-Programs) in The Autoconf Manual) and use ‘`$(LN_S)`’ in
`Makefile.am`.

For instance, here is how you could install a versioned copy of a
program using ‘`$(LN_S)`’:

```
install-exec-hook:
        cd $(DESTDIR)$(bindir) && \
          mv -f prog$(EXEEXT) prog-$(VERSION)$(EXEEXT) && \
          $(LN_S) prog-$(VERSION)$(EXEEXT) prog$(EXEEXT)
```

Note that we rename the program so that a new version will erase the
symbolic link, not the real binary. Also we `cd` into the
destination directory in order to create relative links.

When writing `install-exec-hook` or `install-data-hook`,
please bear in mind that the exec/data distinction is based on the
installation directory, not on the primary used (see [The Two Parts of Install](#The-Two-Parts-of-Install)).
So a `foo_SCRIPTS` will be installed by
`install-data`, and a `barexec_SCRIPTS` will be installed by
`install-exec`. You should define your hooks accordingly.

---

Previous: [Extending Automake Rules](#Extending), Up: [When Automake Isn’t Enough](#Not-Enough)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 22.2 Third-Party `Makefile`s [¶](#Third_002dParty-Makefiles-1)

In most projects all `Makefile`s are generated by Automake. In
some cases, however, projects need to embed subdirectories with
handwritten `Makefile`s. For instance, one subdirectory could be
a third-party project with its own build system, not using Automake.

It is possible to list arbitrary directories in `SUBDIRS` or
`DIST_SUBDIRS` provided each of these directories has a
`Makefile` that recognizes all the following recursive targets.

When a user runs one of these targets, that target is run recursively
in all subdirectories. This is why it is important that even
third-party `Makefile`s support them.

`all`
:   Compile the entire package. This is the default target in
    Automake-generated `Makefile`s, but it does not need to be the
    default in third-party `Makefile`s.

`distdir` [¶](#index-distdir)
:   Copy files to distribute into ‘`$(distdir)`’, before a tarball is
    constructed. Of course this target is not required if the
    `no-dist` option (see [Changing Automake’s Behavior](#Options)) is used.

    The variables ‘`$(top_distdir)`’ and ‘`$(distdir)`’
    (see [The dist Hook](#The-dist-Hook)) will be passed from the outer package to the subpackage
    when the `distdir` target is invoked. These two variables have
    been adjusted for the directory that is being recursed into, so they
    are ready to use.

`install`

`install-data`

`install-exec`

`uninstall`
:   Install or uninstall files (see [What Gets Installed](#Install)).

`install-dvi`

`install-html`

`install-info`

`install-ps`

`install-pdf`
:   Install only some specific documentation format (see [Texinfo](#Texinfo)).

`installdirs`
:   Create install directories, but do not install any files.

`check`

`installcheck`
:   Check the package (see [Support for test suites](#Tests)).

`mostlyclean`

`clean`

`distclean`

`maintainer-clean`
:   Cleaning rules (see [What Gets Cleaned](#Clean)).

`dvi`

`pdf`

`ps`

`info`

`html`
:   Build the documentation in various formats (see [Texinfo](#Texinfo)).

`tags`

`ctags`
:   Build `TAGS` and `CTAGS` (see [Interfacing to `etags`](#Tags)).

If you have ever used Gettext in a project, this is a good example of
how third-party `Makefile`s can be used with Automake. The
`Makefile`s that `gettextize` puts in the `po/` and
`intl/` directories are handwritten `Makefile`s that
implement all of these targets. That way they can be added to
`SUBDIRS` in Automake packages.

Directories that are only listed in `DIST_SUBDIRS` but not in
`SUBDIRS` need only the `distclean`,
`maintainer-clean`, and `distdir` rules (see [Conditional Subdirectories](#Conditional-Subdirectories)).

Usually, many of these rules are irrelevant to the third-party
subproject, but they are required for the whole package to work. It’s
OK to have a rule that does nothing, so if you are integrating a
third-party project with no documentation or tag support, you could
augment its `Makefile` as follows:

```
EMPTY_AUTOMAKE_TARGETS = dvi pdf ps info html tags ctags
.PHONY: $(EMPTY_AUTOMAKE_TARGETS)
$(EMPTY_AUTOMAKE_TARGETS):
```

To be clear, there is nothing special about the variable name
`EMPTY_AUTOMAKE_TARGETS`; the name could be anything.

Another aspect of integrating third-party build systems is whether
they support VPATH builds (see [Parallel Build Trees (a.k.a. VPATH Builds)](#VPATH-Builds)). Obviously if the
subpackage does not support VPATH builds the whole package will not
support VPATH builds. This in turns means that ‘`make distcheck`’
will not work, because it relies on VPATH builds. Some people can
live without this (indeed, many Automake users have never heard of
‘`make distcheck`’). Other people may prefer to revamp the
existing `Makefile`s to support VPATH. Doing so does not
necessarily require Automake; only Autoconf is needed (see [Build Directories](https://www.gnu.org/software/autoconf/manual/autoconf.html#Build-Directories) in The Autoconf Manual).
The necessary substitutions: ‘`@srcdir@`’, ‘`@top_srcdir@`’,
and ‘`@top_builddir@`’ are defined by `configure` when it
processes a `Makefile` (see [Preset
Output Variables](https://www.gnu.org/software/autoconf/manual/autoconf.html#Preset-Output-Variables) in The Autoconf Manual); they are not
computed by the `Makefile` like the aforementioned ‘`$(distdir)`’ and
‘`$(top_distdir)`’ variables.

It is sometimes inconvenient to modify a third-party `Makefile`
to introduce the above required targets. For instance, one may want to
keep the third-party sources untouched to ease upgrades to new
versions.

Here are two other ideas. If GNU Make is assumed, one possibility is
to add to that subdirectory a `GNUmakefile` that defines the
required targets and includes the third-party `Makefile`. For
this to work in VPATH builds, `GNUmakefile` must lie in the build
directory; the easiest way to do this is to write a
`GNUmakefile.in` instead, and have it processed with
`AC_CONFIG_FILES` from the outer package. For example, if we
assume `Makefile` defines all targets except the documentation
targets, and that the real `check` target is named `test`,
we could write `GNUmakefile` (or `GNUmakefile.in`) like
this:

```
# First, include the real Makefile
include Makefile
# Then, define the other targets needed by Automake Makefiles.
.PHONY: dvi pdf ps info html check
dvi pdf ps info html:
check: test
```

A similar idea that does not use `include` is to write a proxy
`Makefile` that dispatches rules to the real `Makefile`,
either with ‘`$(MAKE) -f Makefile.real $(AM_MAKEFLAGS) target`’ (if
it’s OK to rename the original `Makefile`) or with ‘`cd
subdir && $(MAKE) $(AM_MAKEFLAGS) target`’ (if it’s OK to store the
subdirectory project one directory deeper). The good news is that
this proxy `Makefile` can be generated with Automake. All we
need are `-local` targets (see [Extending Automake Rules](#Extending)) that perform the
dispatch. Of course the other Automake features are available, so you
could decide to let Automake perform distribution or installation.
Here is a possible `Makefile.am`:

```
all-local:
        cd subdir && $(MAKE) $(AM_MAKEFLAGS) all
check-local:
        cd subdir && $(MAKE) $(AM_MAKEFLAGS) test
clean-local:
        cd subdir && $(MAKE) $(AM_MAKEFLAGS) clean

# Assuming the package knows how to install itself
install-data-local:
        cd subdir && $(MAKE) $(AM_MAKEFLAGS) install-data
install-exec-local:
        cd subdir && $(MAKE) $(AM_MAKEFLAGS) install-exec
uninstall-local:
        cd subdir && $(MAKE) $(AM_MAKEFLAGS) uninstall

# Distribute files from here.
EXTRA_DIST = subdir/Makefile subdir/program.c ...
```

Pushing this idea to the extreme, it is also possible to ignore the
subproject build system and build everything from this proxy
`Makefile.am`. This might well be sensible if you need VPATH
builds but the subproject does not support them.

---

Next: [Automake API Versioning](#API-Versioning), Previous: [When Automake Isn’t Enough](#Not-Enough), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 23 Distributing `Makefile.in`s [¶](#Distributing-Makefile_002eins)

Automake places no restrictions on the distribution of the resulting
`Makefile.in`s. We encourage software authors to distribute
their work under terms like those of the GPL, but doing so is not
required to use Automake.

Some of the files that can be automatically installed via the
`--add-missing` switch do fall under the GPL. However, these also
have a special exception allowing you to distribute them with your
package, regardless of the licensing you choose.

---

Next: [Upgrading a Package to a Newer Automake Version](#Upgrading), Previous: [Distributing `Makefile.in`s](#Distributing), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 24 Automake API Versioning [¶](#Automake-API-Versioning)

New Automake releases usually include bug fixes and new features.
Unfortunately they may also introduce new bugs and incompatibility.
This makes four reasons why a package may require a particular Automake
version.

Things get worse when maintaining a large tree of packages, each one
requiring a different version of Automake. In the past, this meant that
any developer (and sometimes users) had to install several versions of
Automake in different places, and switch ‘`$PATH`’ appropriately for
each package.

Starting with version 1.6, Automake installs versioned binaries. This
means you can install several versions of Automake in the same
‘`$prefix`’, and can select an arbitrary Automake version by running
`automake-1.6` or `automake-1.7` without juggling with
‘`$PATH`’. Furthermore, `Makefile`s generated by Automake 1.6
will use `automake-1.6` explicitly in their rebuild rules.

The number ‘`1.6`’ in `automake-1.6` is Automake’s API version,
not Automake’s version. If a bug fix release is made, for instance
Automake 1.6.1, the API version will remain 1.6. This means that a
package that works with Automake 1.6 should also work with 1.6.1; after
all, this is what people expect from bug fix releases.

If your package relies on a feature or a bug fix introduced in
a release, you can pass this version as an option to Automake to ensure
older releases will not be used. For instance, use this in your
`configure.ac`:

```
  AM_INIT_AUTOMAKE([1.6.1])    dnl Require Automake 1.6.1 or better.
```

or, in a particular `Makefile.am`:

```
  AUTOMAKE_OPTIONS = 1.6.1   # Require Automake 1.6.1 or better.
```

Automake will print an error message if its version is
older than the requested version.

### What is in the API [¶](#What-is-in-the-API)

Automake’s programming interface is not easy to define. It includes
at least all documented variables and targets that a
`Makefile.am` author can use, any behavior associated with them
(e.g., the places where ‘`-hook`’’s are run), the command line
interface of `automake` and `aclocal`, …

### What is not in the API [¶](#What-is-not-in-the-API)

Every undocumented variable, target, or command line option is not part
of the API. You should avoid using them, as they could change from one
version to the other (even in bug fix releases, if this helps to fix a
bug).

If it turns out you need to use such an undocumented feature, contact
[automake@gnu.org](mailto:automake@gnu.org) and try to get it documented and exercised by
the test-suite.

---

Next: [Frequently Asked Questions about Automake](#FAQ), Previous: [Automake API Versioning](#API-Versioning), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 25 Upgrading a Package to a Newer Automake Version [¶](#Upgrading-a-Package-to-a-Newer-Automake-Version)

Automake maintains three kinds of files in a package.

* `aclocal.m4`
* `Makefile.in`s
* auxiliary tools like `install-sh` or `py-compile`

`aclocal.m4` is generated by `aclocal` and contains some
Automake-supplied M4 macros. Auxiliary tools are installed by
‘`automake --add-missing`’ when needed. `Makefile.in`s are
built from `Makefile.am` by `automake`, and rely on the
definitions of the M4 macros put in `aclocal.m4` as well as the
behavior of the auxiliary tools installed.

Because all of these files are closely related, it is important to
regenerate all of them when upgrading to a newer Automake release.
The usual way to do that is

```
aclocal # with any option needed (such as -I m4)
autoconf
automake --add-missing --force-missing
```

or more conveniently:

```
autoreconf -vfi
```

The use of `--force-missing` ensures that auxiliary tools will be
overridden by new versions (see [Creating a `Makefile.in`: Invoking `automake`](#automake-Invocation)).

It is important to regenerate all of these files each time Automake is
upgraded, even between bug fix releases. For instance, it is not
unusual for a bug fix to involve changes to both the rules generated
in `Makefile.in` and the supporting M4 macros copied to
`aclocal.m4`.

Presently `automake` is able to diagnose situations where
`aclocal.m4` has been generated with another version of
`aclocal`, but it never checks whether auxiliary scripts are
up-to-date. In other words, `automake` will tell you when
`aclocal` needs to be rerun, but it will never diagnose a
missing `--force-missing`.

Before upgrading to a new major release, it is a good idea to read the
file `NEWS`. This file lists all changes between releases: new
features, obsolete constructs, known incompatibility, and
workarounds.

---

Next: [Copying This Manual](#Copying-This-Manual), Previous: [Upgrading a Package to a Newer Automake Version](#Upgrading), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 26 Frequently Asked Questions about Automake [¶](#Frequently-Asked-Questions-about-Automake)

This chapter covers some questions that often come up on the mailing
lists.

* [Version control and generated files](#Version-Control)
* [`missing` and `AM_MAINTAINER_MODE`](#maintainer_002dmode)
* [Why doesn’t Automake support wildcards?](#Wildcards)
* [Limitations on File Names](#Limitations-on-File-Names)
* [Flag Variables Ordering](#Flag-Variables-Ordering)
* [Why are object files sometimes renamed?](#Renamed-Objects)
* [Per-Object Flags Emulation](#Per_002dObject-Flags)
* [Handling Tools that Produce Many Outputs](#Multiple-Outputs)
* [Installing to Hard-Coded Locations](#Hard_002dCoded-Install-Paths)
* [Debugging Make Rules](#Debugging-Make-Rules)
* [Reporting Bugs](#Reporting-Bugs)

---

Next: [`missing` and `AM_MAINTAINER_MODE`](#maintainer_002dmode), Up: [Frequently Asked Questions about Automake](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 26.1 Version control and generated files [¶](#Version-control-and-generated-files)

#### Background: distributed generated Files [¶](#Background_003a-distributed-generated-Files)

Packages made with Autoconf and Automake ship with some generated
files like `configure` or `Makefile.in`. These files were
generated on the developer’s machine and are distributed so that
end-users do not have to install the maintainer tools required to
rebuild them. Other generated files like Lex scanners, Yacc parsers,
or Info documentation are usually distributed on similar grounds.

Automake output generates rules in `Makefile`s to rebuild these
files. For instance, `make` will run `autoconf` to
rebuild `configure` whenever `configure.ac` is changed.
This makes development safer by ensuring a `configure` is never
out-of-date with respect to `configure.ac`.

As generated files shipped in packages are up-to-date, and because
`tar` preserves timestamps, these rebuild rules are not
triggered when a user unpacks and builds a package.

#### Background: Version Control and Timestamps [¶](#Background_003a-Version-Control-and-Timestamps)

Typically when you update files with version control commands, working
files will have the timestamp of your update, not the original
timestamp of the commit. This is meant to make sure that
`make` notices that source files have been updated.

This timestamp shift is troublesome when both sources and generated
files are kept under version control. Because version control
commands often process files in lexical order, `configure.ac`
will appear newer than `configure` after a version control
command that updates both files, even if `configure` was newer
than `configure.ac` when it was committed. Calling
`make` will then trigger a spurious rebuild of
`configure`.

#### Living with Version Control in Autoconfiscated Projects [¶](#Living-with-Version-Control-in-Autoconfiscated-Projects)

There are basically two clans among maintainers: those who keep all
distributed files under version control, including generated files, and
those who keep generated files *out* of version control.

#### All Files under Version Control [¶](#All-Files-under-Version-Control)

* The repository contains all distributed files so you know exactly
  what is distributed, and you can check out any prior version entirely.
* Maintainers can see how generated files evolve (for instance, you can
  see what happens to your `Makefile.in`s when you upgrade Automake
  and make sure they look OK).
* Users do not need Autotools to build a check-out of the project; it
  works just like a released tarball.
* If users use version control to update their copy, timestamps will likely be
  inaccurate. Some rebuild rules will be triggered and attempt to
  run developer tools such as `autoconf` or `automake`.

  Calls to such tools are all wrapped into a call to the `missing`
  script discussed later (see [`missing` and `AM_MAINTAINER_MODE`](#maintainer_002dmode)), so that the user will
  see more descriptive warnings about missing or out-of-date tools, and
  possible suggestions about how to obtain them, rather than just some
  “command not found” error, or (worse) some obscure message from some
  older version of the required tool they happen to have installed.

  Maintainers interested in keeping their package buildable from a
  checkout even for those users that lack maintainer-specific tools might
  want to provide a helper script (or to enhance their existing bootstrap
  script) to fix the timestamps after a checkout, to prevent spurious
  rebuilds. In case of a project committing the Autotools-generated
  files, as well as the generated `.info` files, such a script might
  look something like this:

  ```
  #!/bin/sh
  # fix-timestamp.sh: Prevent useless rebuilds after "git pull".
  sleep 1
  # aclocal-generated aclocal.m4 depends on locally-installed
  # '.m4' macro files, as well as on 'configure.ac'.
  touch aclocal.m4
  sleep 1
  # autoconf-generated 'configure' and autoheader-generated
  # config.h.in both depend on aclocal.m4 and on configure.ac.
  touch configure config.h.in
  # Automake-generated Makefile.in files depend on Makefile.am,
  # and makeinfo-generated '.info' files depend on the
  # corresponding '.texi' files.
  touch $(git ls-files '*/Makefile.in' '*.info')
  ```
* In distributed development, developers are likely to have different
  versions of the maintainer tools installed. In this case rebuilds
  triggered by clock skew can lead to spurious changes to generated
  files. There are several solutions to this:
  + All developers should use the same versions, so that the rebuilt files
    are identical to files in the repository. (This becomes difficult
    when different projects on which you are working use different versions.)
  + Or people use a script to fix the timestamp after a checkout (the GCC
    folks have such a script).
  + Or `configure.ac` uses `AM_MAINTAINER_MODE`, which disables
    all of these rebuild rules by default. This is further discussed in
    [`missing` and `AM_MAINTAINER_MODE`](#maintainer_002dmode).
* Although we focused on spurious rebuilds, the converse can also
  happen. Version control timestamp handling can also let you think an
  out-of-date file is up-to-date.

  For instance, suppose a developer has modified `Makefile.am` and
  has rebuilt `Makefile.in`, and then decides to do a last-minute
  change to `Makefile.am` right before checking in both files
  (without rebuilding `Makefile.in` to account for the change).

  This last change to `Makefile.am` makes the copy of
  `Makefile.in` out-of-date. Assuming version control processes
  files alphabetically, when another developer updates their tree,
  `Makefile.in` will happen to be newer than `Makefile.am`.
  This other developer will not see that `Makefile.in` is
  out-of-date.

#### Generated Files out of Version Control [¶](#Generated-Files-out-of-Version-Control)

One way to get version control and `make` working peacefully
is to never store generated files in version control, i.e., do not
version-control files that are `Makefile` targets (also called
*derived* files).

This way developers are not annoyed by changes to generated files. It
does not matter if they all have different versions (assuming they are
compatible, of course). And finally, timestamps are not lost; changes
to source files can’t be missed as in the
`Makefile.am`/`Makefile.in` example discussed earlier.

The drawback is that the repository does not contain some files that
are is distributed, so builders now need to install various
development tools (maybe even specific versions) before they can build
a checkout. But, after all, the job of version control is versioning,
not distribution.

Allowing developers to use different versions of their tools can also
hide bugs during distributed development. Indeed, developers will be
using (hence testing) their own generated files, instead of the
generated files that will be released. The developer who prepares the
tarball might be using a version of the tool that produces bogus
output (for instance a non-portable C file), something other
developers could have noticed if they weren’t using their own versions
of this tool.

#### Third-party Files [¶](#Third_002dparty-Files)

Another class of files not discussed here (because they do not cause
timestamp issues) are files that are shipped with a package, but
maintained elsewhere. For instance, tools like `gettextize`
and `autopoint` (from Gettext) or `libtoolize` (from
Libtool), will install or update files in your package.

These files, whether they are kept under version control or not, raise
similar concerns about version mismatch between developers’ tools.
The Gettext manual has a section about this; see [Integrating with Version Control Systems](https://www.gnu.org/software/gettext/manual/gettext.html#Version-Control-Issues) in GNU
gettext tools.

---

Next: [Why doesn’t Automake support wildcards?](#Wildcards), Previous: [Version control and generated files](#Version-Control), Up: [Frequently Asked Questions about Automake](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 26.2 `missing` and `AM_MAINTAINER_MODE` [¶](#missing-and-AM_005fMAINTAINER_005fMODE)

#### `missing` [¶](#missing)

The `missing` script is a wrapper around several maintainer
tools, designed to warn users if a maintainer tool is required but
missing. Typical maintainer tools are `autoconf`,
`automake`, `bison`, etc. Because files generated by
these tools are shipped with the other sources of a package, these
tools shouldn’t be required during a user build and they are not
checked for in `configure`.

However, if for some reason a rebuild rule is triggered and involves a
missing tool, `missing` will notice it and warn the user, even
suggesting how to obtain such a tool (at least in case it is a well-known
one, like `makeinfo` or `bison`). This is more helpful
and user-friendly than just having the rebuild rules spewing out a terse
error message like ‘`sh: tool: command not found`’. Similarly,
`missing` will warn the user if it detects that a maintainer
tool it attempted to use seems too old (be warned that diagnosing this
correctly is typically more difficult than detecting missing tools, and
requires cooperation from the tool itself, so it won’t always work).

If the required tool is installed, `missing` will run it and
won’t attempt to continue after failures. This is correct behavior during
development: developers love fixing failures. However, users with
missing or too old maintainer tools may get an error when the rebuild
rule is spuriously triggered, halting the build. This failure to let
the build continue is one of the arguments of the
`AM_MAINTAINER_MODE` advocates.

#### `AM_MAINTAINER_MODE` [¶](#AM_005fMAINTAINER_005fMODE)

`AM_MAINTAINER_MODE` allows you to choose whether the so called
"rebuild rules" should be enabled or disabled. With
`AM_MAINTAINER_MODE([enable])`, they are enabled by default;
otherwise they are disabled by default. In the latter case, if you
have `AM_MAINTAINER_MODE` in `configure.ac`, and run
‘`./configure && make`’, then `make` will \*never\* attempt
to rebuild `configure`, `Makefile.in`s, Lex or Yacc outputs,
etc. That is, this disables build rules for files that are usually
distributed and that users should normally not have to update.

The user can override the default setting by passing either
‘`--enable-maintainer-mode`’ or ‘`--disable-maintainer-mode`’
to `configure`.

People use `AM_MAINTAINER_MODE` either because they do not want their
users (or themselves) annoyed by clock skew (see [Version control and generated files](#Version-Control)), or
because they simply can’t stand the rebuild rules and prefer running
maintainer tools explicitly.

`AM_MAINTAINER_MODE` also allows you to disable some custom build
rules conditionally. Some developers use this feature to disable
rules that need exotic tools that users may not have available.

Several years ago François Pinard pointed out several arguments
against this `AM_MAINTAINER_MODE` macro. Most of them relate to
insecurity. By removing dependencies you get non-dependable builds:
changes to source files can have no effect on generated files and this
can be very confusing when unnoticed. He adds that security shouldn’t
be reserved to maintainers (what `--enable-maintainer-mode`
suggests), on the contrary. If one user has to modify a
`Makefile.am`, then either `Makefile.in` should be updated
or a warning should be output (this is what Automake uses
`missing` for) but the last thing you want is that nothing
happens and the user doesn’t notice it (this is what happens when
rebuild rules are disabled by `AM_MAINTAINER_MODE`).

Jim Meyering, the inventor of the `AM_MAINTAINER_MODE` macro, was
swayed by François’ arguments, and got rid of
`AM_MAINTAINER_MODE` in all of his packages.

Still many people continue to use `AM_MAINTAINER_MODE`, because
it helps them working on projects where all files are kept under version
control, and because `missing` isn’t enough if you have the
wrong version of the tools.

---

Next: [Limitations on File Names](#Limitations-on-File-Names), Previous: [`missing` and `AM_MAINTAINER_MODE`](#maintainer_002dmode), Up: [Frequently Asked Questions about Automake](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 26.3 Why doesn’t Automake support wildcards? [¶](#Why-doesn_0027t-Automake-support-wildcards_003f)

Developers are lazy. They would often like to use wildcards in
`Makefile.am`s, so that they would not need to remember to
update `Makefile.am`s every time they add, delete, or rename
a file.

There are several objections to this:

* When using version control, developers need to remember they have to
  add or remove files from version control anyway. Updating
  `Makefile.am` accordingly quickly becomes a reflex.

  Conversely, if your application doesn’t compile
  because you forgot to add a file in `Makefile.am`, it will help
  you remember to add the file to version control.
* Using wildcards makes it easy to distribute files by mistake. For
  instance, some code a developer is experimenting with (a test case,
  say) that should not be part of the distribution.
* Using wildcards it’s easy to omit some files by mistake. For
  instance, one developer creates a new file, uses it in many places,
  but forgets to commit it. Another developer then checks out the
  incomplete project and is able to run ‘`make dist`’ successfully,
  even though a file is missing. By listing files, ‘`make dist`’
  *will* complain.
* Wildcards are not portable to some non-GNU `make` implementations,
  e.g., NetBSD `make` will not expand globs such as ‘`*`’ in
  prerequisites of a target.
* Finally, it’s quite hard to *forget* to add a file to
  `Makefile.am`: files that are not listed in `Makefile.am`
  are not compiled or installed, so you can’t even test them.

Still, these are philosophical objections, and as such you may disagree,
or find enough value in wildcards to dismiss all of them. Before you
start writing a patch against Automake to teach it about wildcards,
let’s see the main technical issue: portability.

Although ‘`$(wildcard ...)`’ works with GNU `make`, it is
not portable to other `make` implementations.

The only way Automake could support `$(wildcard ...)` is by
expanding `$(wildcard ...)` when `automake` is run.
The resulting `Makefile.in`s would be portable since they would
list all files and not use ‘`$(wildcard ...)`’. However that
means developers would need to remember to run `automake` each
time they add, delete, or rename files.

Compared to editing `Makefile.am`, this is a very small gain. Sure,
it’s easier and faster to type ‘`automake; make`’ than to type
‘`emacs Makefile.am; make`’. But nobody bothered enough to write a
patch to add support for this syntax. Some people use scripts to
generate file lists in `Makefile.am` or in separate
`Makefile` fragments.

Even if you don’t care about portability, and are tempted to use
‘`$(wildcard ...)`’ anyway because you target only GNU Make, you
should know there are many places where Automake needs to know exactly
which files should be processed. As Automake doesn’t know how to
expand ‘`$(wildcard ...)`’, you cannot use it in these places.
‘`$(wildcard ...)`’ is a black box comparable to `AC_SUBST`ed
variables as far Automake is concerned.

You can get warnings about ‘`$(wildcard ...`’) constructs using the
`-Wportability` flag.

---

Next: [Flag Variables Ordering](#Flag-Variables-Ordering), Previous: [Why doesn’t Automake support wildcards?](#Wildcards), Up: [Frequently Asked Questions about Automake](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 26.4 Limitations on File Names [¶](#Limitations-on-File-Names-1)

Automake attempts to support all kinds of file names, even those that
contain unusual characters or are unusually long. However, some
limitations are imposed by the underlying operating system and tools.

Most operating systems prohibit the use of the null byte in file
names, and reserve ‘`/`’ as a directory separator. Also, they
require that file names are properly encoded for the user’s locale.
Automake is subject to these limits.

Portable packages should limit themselves to POSIX file
names. These can contain ASCII letters and digits,
‘`_`’, ‘`.`’, and ‘`-`’. File names consist of components
separated by ‘`/`’. File name components cannot begin with
‘`-`’.

Portable POSIX file names cannot contain components that exceed a
14-byte limit, but nowadays it’s normally safe to assume the
more-generous XOPEN limit of 255 bytes. POSIX
limits file names to 255 bytes (XOPEN allows 1023 bytes),
but you may want to limit a source tarball to file names of 99 bytes
to avoid interoperability problems with old versions of `tar`.

If you depart from these rules (e.g., by using non-ASCII
characters in file names, or by using lengthy file names), your
installers may have problems for reasons unrelated to Automake.
However, if this does not concern you, you should know about the
limitations imposed by Automake itself. These limitations are
undesirable, but some of them seem to be inherent to underlying tools
like Autoconf, Make, M4, and the shell. They fall into three
categories: install directories, build directories, and file names.

The following characters:

```
newline " # $ ' `
```

should not appear in the names of install directories. For example,
the operand of `configure`’s `--prefix` option should
not contain these characters.

Build directories suffer the same limitations as install directories,
and in addition should not contain the following characters:

```
& @ \
```

For example, the full name of the directory containing the source
files should not contain these characters.

Source and installation file names like `main.c` are limited even
further: they should conform to the POSIX/XOPEN
rules described above. In addition, if you plan to port to
non-POSIX environments, you should avoid file names that
differ only in case (e.g., `makefile` and `Makefile`).
Nowadays it is no longer worth worrying about the 8.3 limits of
DOS file systems.

---

Next: [Why are object files sometimes renamed?](#Renamed-Objects), Previous: [Limitations on File Names](#Limitations-on-File-Names), Up: [Frequently Asked Questions about Automake](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 26.5 Flag Variables Ordering [¶](#Flag-Variables-Ordering-1)

```
What is the difference between AM_CFLAGS, CFLAGS, and
mumble_CFLAGS?
```

```
Why does automake output CPPFLAGS after
AM_CPPFLAGS on compile lines?  Shouldn’t it be the converse?
```

```
My configure adds some warning flags into CXXFLAGS.  In
one Makefile.am I would like to append a new flag, however if I
put the flag into AM_CXXFLAGS it is prepended to the other
flags, not appended.
```

#### Compile Flag Variables [¶](#Compile-Flag-Variables)

This section attempts to answer all the above questions. We will
mostly discuss `CPPFLAGS` in our examples, but the answer holds
for all the compile flags used in Automake: `CCASFLAGS`,
`CFLAGS`, `CPPFLAGS`, `CXXFLAGS`, `FCFLAGS`,
`FFLAGS`, `GCJFLAGS`, `LDFLAGS`, `LFLAGS`,
`LIBTOOLFLAGS`, `OBJCFLAGS`, `OBJCXXFLAGS`,
`RFLAGS`, `UPCFLAGS`, and `YFLAGS`.

`CPPFLAGS`, `AM_CPPFLAGS`, and `mumble_CPPFLAGS` are
three variables that can be used to pass flags to the C preprocessor (
these variables are also used for other languages like C++ or
preprocessed Fortran). `CPPFLAGS` is the user variable
(see [Variables reserved for the user](#User-Variables)), `AM_CPPFLAGS` is the Automake variable,
and `mumble_CPPFLAGS` is the variable specific to the
`mumble` target (we call this a per-target variable,
see [Program and Library Variables](#Program-and-Library-Variables)).

Automake always uses two of these variables when compiling C sources
files. When compiling an object file for the `mumble` target,
the first variable will be `mumble_CPPFLAGS` if it is defined, or
`AM_CPPFLAGS` otherwise. The second variable is always
`CPPFLAGS`.

In the following example,

```
bin_PROGRAMS = foo bar
foo_SOURCES = xyz.c
bar_SOURCES = main.c
foo_CPPFLAGS = -DFOO
AM_CPPFLAGS = -DBAZ
```

`xyz.o` will be compiled with ‘`$(foo_CPPFLAGS) $(CPPFLAGS)`’,
(because `xyz.o` is part of the `foo` target), while
`main.o` will be compiled with ‘`$(AM_CPPFLAGS) $(CPPFLAGS)`’
(because there is no per-target variable for target `bar`).

The difference between `mumble_CPPFLAGS` and `AM_CPPFLAGS`
being clear enough, let’s focus on `CPPFLAGS`. `CPPFLAGS`
is a user variable, i.e., a variable that users are entitled to modify
in order to compile the package. This variable, like many others,
is documented at the end of the output of ‘`configure --help`’.

For instance, someone who needs to add `/home/my/usr/include` to
the C compiler’s search path would configure a package with

```
./configure CPPFLAGS='-I /home/my/usr/include'
```

and this flag would be propagated to the compile rules of all
`Makefile`s.

It is also not uncommon to override a user variable at
`make`-time. Many installers do this with `prefix`, but
this can be useful with compiler flags too. For instance, while
debugging a C++ project, if you need to disable optimization in one
specific object file, you can run something like

```
rm file.o
make CXXFLAGS=-O0 file.o
make
```

The reason ‘`$(CPPFLAGS)`’ appears after ‘`$(AM_CPPFLAGS)`’ or
‘`$(mumble_CPPFLAGS)`’ in the compile command is that users should
have the last say. In the example above, the desire is for the
‘`CXXFLAGS=-O0`’ to supersede any other switch from
`AM_CXXFLAGS` or `mumble_CXXFLAGS`.

It’s true that not all options to all programs can be overridden. So
in general, users could conceivably want to place options at arbitrary
places in the command line, but Automake does not support this. It
would be difficult to make such generality comprehensible. Being able
to specify the final options commonly suffices.

Thus, you should never redefine a user variable such as
`CPPFLAGS` in `Makefile.am`. Use ‘`automake -Woverride`’
to diagnose such mistakes. Even something like

```
CPPFLAGS = -DDATADIR=\"$(datadir)\" @CPPFLAGS@
```

is erroneous. Although this preserves `configure`’s value of
`CPPFLAGS`, the definition of `DATADIR` will disappear if a
user attempts to override `CPPFLAGS` from the `make`
command line.

```
AM_CPPFLAGS = -DDATADIR=\"$(datadir)\"
```

is all that is needed here if no per-target flags are used.

You should not add options to these user variables within
`configure` either, for the same reason. Occasionally you need
to modify these variables to perform a test, but you should reset
their values afterwards. In contrast, it is OK to modify the
‘`AM_`’ variables within `configure` if you `AC_SUBST`
them, but it is rather rare that you need to do this, unless you want
to change the default definitions of the ‘`AM_`’ variables in all
`Makefile`s.

What we recommend is that you define extra flags in separate
variables. For instance, you may write an Autoconf macro that computes
a set of warning options for the C compiler, and `AC_SUBST` them
in `WARNINGCFLAGS`; you may also have an Autoconf macro that
determines which compiler and which linker flags should be used to
link with library `libfoo`, and `AC_SUBST` these in
`LIBFOOCFLAGS` and `LIBFOOLDFLAGS`. Then, a
`Makefile.am` could use these variables as follows:

```
AM_CFLAGS = $(WARNINGCFLAGS)
bin_PROGRAMS = prog1 prog2
prog1_SOURCES = ...
prog2_SOURCES = ...
prog2_CFLAGS = $(LIBFOOCFLAGS) $(AM_CFLAGS)
prog2_LDFLAGS = $(LIBFOOLDFLAGS)
```

In this example both programs will be compiled with the flags
substituted into ‘`$(WARNINGCFLAGS)`’, and `prog2` will
additionally be compiled with the flags required to link with
`libfoo`.

Note that listing `AM_CFLAGS` in a per-target `CFLAGS`
variable is a common idiom to ensure that `AM_CFLAGS` applies to
every target in a `Makefile.in`.

Using variables like this gives you full control over the ordering of
the flags. For instance, if there is a flag in $(WARNINGCFLAGS) that
you want to negate for a particular target, you can use something like
‘`prog1_CFLAGS = $(AM_CFLAGS) -no-flag`’. If all of these flags had
been forcefully appended to `CFLAGS`, there would be no way to
disable one flag. Yet another reason to leave user variables to
users.

Finally, we have avoided naming the variable of the example
`LIBFOO_LDFLAGS` (with an underscore) because that would cause
Automake to think that this is a per-target variable (like
`mumble_LDFLAGS`) for some non-declared `LIBFOO` target.

#### Other Variables [¶](#Other-Variables)

There are other variables in Automake that follow similar principles
to allow user options. For instance, Texinfo rules (see [Texinfo](#Texinfo))
use `MAKEINFOFLAGS` and `AM_MAKEINFOFLAGS`. Similarly,
DejaGnu tests (see [DejaGnu Tests](#DejaGnu-Tests)) use `RUNTESTFLAGS` and
`AM_RUNTESTFLAGS`. The tags and ctags rules
(see [Interfacing to `etags`](#Tags)) use `ETAGSFLAGS`, `AM_ETAGSFLAGS`,
`CTAGSFLAGS`, and `AM_CTAGSFLAGS`. Java rules
(see [Java bytecode compilation (deprecated)](#Java)) use `JAVACFLAGS` and `AM_JAVACFLAGS`. None
of these rules support per-target flags (yet).

To some extent, even `AM_MAKEFLAGS` (see [Recursing subdirectories](#Subdirectories))
obeys this naming scheme. The slight difference is that
`MAKEFLAGS` is passed to sub-`make`s implicitly by
`make` itself.

`ARFLAGS` (see [Building a library](#A-Library)) is usually defined by Automake and
has neither an `AM_` nor a per-target cousin.

Finally you should not think that the existence of a per-target
variable implies the existence of an `AM_` variable or of a user
variable. For instance, the `mumble_LDADD` per-target variable
overrides the makefile-wide `LDADD` variable (which is not a user
variable), and `mumble_LIBADD` exists only as a per-target
variable. See [Program and Library Variables](#Program-and-Library-Variables).

---

Next: [Per-Object Flags Emulation](#Per_002dObject-Flags), Previous: [Flag Variables Ordering](#Flag-Variables-Ordering), Up: [Frequently Asked Questions about Automake](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 26.6 Why are object files sometimes renamed? [¶](#Why-are-object-files-sometimes-renamed_003f)

This happens when per-target compilation flags are used. Object
files need to be renamed just in case they would clash with object
files compiled from the same sources, but with different flags.
Consider the following example.

```
bin_PROGRAMS = true false
true_SOURCES = generic.c
true_CPPFLAGS = -DEXIT_CODE=0
false_SOURCES = generic.c
false_CPPFLAGS = -DEXIT_CODE=1
```

Obviously the two programs are built from the same source, but it
would be bad if they shared the same object, because `generic.o`
cannot be built with both ‘`-DEXIT_CODE=0`’ *and*
‘`-DEXIT_CODE=1`’. Therefore `automake` outputs rules to
build two different objects: `true-generic.o` and
`false-generic.o`.

Automake doesn’t actually determine whether source files are shared to
decide if it must rename objects. It just renames all objects of a
target as soon as it sees that per-target compilation flags are used.

It’s OK to share object files when per-target compilation flags are not
used. For instance, `true` and `false` will both use
`version.o` in the following example.

```
AM_CPPFLAGS = -DVERSION=1.0
bin_PROGRAMS = true false
true_SOURCES = true.c version.c
false_SOURCES = false.c version.c
```

Note that the renaming of objects is also affected by the
`_SHORTNAME` variable (see [Program and Library Variables](#Program-and-Library-Variables)).

---

Next: [Handling Tools that Produce Many Outputs](#Multiple-Outputs), Previous: [Why are object files sometimes renamed?](#Renamed-Objects), Up: [Frequently Asked Questions about Automake](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 26.7 Per-Object Flags Emulation [¶](#Per_002dObject-Flags-Emulation)

```
One of my source files needs to be compiled with different flags.  How
do I do that?
```

Automake supports per-program and per-library compilation flags (see
[Program and Library Variables](#Program-and-Library-Variables) and [Flag Variables Ordering](#Flag-Variables-Ordering)). With this you can define compilation flags that apply to
all files compiled for a target. For instance, in

```
bin_PROGRAMS = foo
foo_SOURCES = foo.c foo.h bar.c bar.h main.c
foo_CFLAGS = -some -flags
```

`foo-foo.o`, `foo-bar.o`, and `foo-main.o` will all be
compiled with ‘`-some -flags`’. (If you wonder about the names of
these object files, see [Why are object files sometimes renamed?](#Renamed-Objects).) Note that
`foo_CFLAGS` gives the flags to use when compiling all the C
sources of the *program* `foo`; it has nothing to do with
`foo.c` or `foo-foo.o` specifically.

What if `foo.c` needs to be compiled into `foo.o` using some
specific flags, that none of the other files requires? Obviously
per-program flags are not directly applicable here. Something like
per-object flags are expected, i.e., flags that would be used only
when creating `foo-foo.o`. Automake does not support that;
however this is easy to simulate using a library that contains only
that object, and compiling this library with per-library flags.

```
bin_PROGRAMS = foo
foo_SOURCES = bar.c bar.h main.c
foo_CFLAGS = -some -flags
foo_LDADD = libfoo.a
noinst_LIBRARIES = libfoo.a
libfoo_a_SOURCES = foo.c foo.h
libfoo_a_CFLAGS = -some -other -flags
```

Here `foo-bar.o` and `foo-main.o` will all be
compiled with ‘`-some -flags`’, while `libfoo_a-foo.o` will
be compiled using ‘`-some -other -flags`’. Eventually, all
three objects will be linked to form `foo`.

This trick can also be achieved using Libtool convenience libraries,
for instance ‘`noinst_LTLIBRARIES = libfoo.la`’ (see [Libtool Convenience Libraries](#Libtool-Convenience-Libraries)).

Another tempting idea to implement per-object flags is to override the
compile rules `automake` would output for these files.
Automake will not define a rule for a target you have defined, so you
could think about defining the ‘`foo-foo.o: foo.c`’ rule yourself.
We recommend against this, because this is error prone. For instance,
if you add such a rule to the first example, it will break the day you
decide to remove `foo_CFLAGS` (because `foo.c` will then be
compiled as `foo.o` instead of `foo-foo.o`, see [Why are object files sometimes renamed?](#Renamed-Objects)). Also in order to support dependency tracking, the two
`.o`/`.obj` extensions, and all the other flags variables
involved in a compilation, you will end up modifying a copy of the
rule previously output by `automake` for this file. If a new
release of Automake generates a different rule, your copy will need to
be updated by hand.

---

Next: [Installing to Hard-Coded Locations](#Hard_002dCoded-Install-Paths), Previous: [Per-Object Flags Emulation](#Per_002dObject-Flags), Up: [Frequently Asked Questions about Automake](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 26.8 Handling Tools that Produce Many Outputs [¶](#Handling-Tools-that-Produce-Many-Outputs)

This section describes a `make` idiom that can be used when a
tool produces multiple output files. It is not specific to Automake
and can be used in ordinary `Makefile`s.

First, however: GNU `make` is able to express rules with
multiple output files using pattern rules (see [Pattern Rule Examples](https://www.gnu.org/software/make/manual/make.html#Pattern-Examples) in The GNU Make Manual). We do not discuss
pattern rules here because they are not portable, but if you’re able
to assume GNU `make`, they are typically more convenient than
any of the below approaches.

Suppose we have a program called `foo` that will read one file
called `data.foo` and produce two files named `data.c` and
`data.h`. We want to write a `Makefile` rule that captures
this one-to-two dependency.

The naive rule is incorrect:

```
# This is incorrect.
data.c data.h: data.foo
        foo data.foo
```

What the above rule says is that `data.c` and `data.h` each
depend on `data.foo`, and can each be built by running ‘`foo
data.foo`’. In other words it is equivalent to:

```
# We do not want this.
data.c: data.foo
        foo data.foo
data.h: data.foo
        foo data.foo
```

which means that `foo` can be run twice. Usually it will not
be run twice, because `make` implementations are smart enough
to check for the existence of the second file after the first one has
been built; they will therefore detect that it already exists.
However there are a few situations where it can run twice anyway:

* The most worrying case is when running a parallel `make`. If
  `data.c` and `data.h` are built in parallel, two ‘`foo
  data.foo`’ commands will run concurrently. This is harmful.
* Another case is when the dependency (here `data.foo`) is
  (or depends upon) a phony target.

Ideally, we want a scheme that will support any number of output
files, and that works with parallel `make` invocations, and
that does nothing when ‘`make -n`’ is run. It is apparently not
possible to achieve a perfect solution. Even an acceptable solution
for the majority of cases gets complicated, so we will take it step by
step.

One idea is to write the following:

```
# There is still a problem with this one.
data.c: data.foo
        foo data.foo
data.h: data.c
```

The idea is that ‘`foo data.foo`’ is run only when `data.c`
needs to be updated, but we further state that `data.h` depends
upon `data.c`. That way, if `data.h` is required and
`data.foo` is out of date, the dependency on `data.c` will
trigger the build.

This is almost perfect, but suppose we have built `data.h` and
`data.c`, and then we erase `data.h`. Then, running
‘`make data.h`’ will not rebuild `data.h`. The above rules
just state that `data.c` must be up-to-date with respect to
`data.foo`, and this is already the case.

What we need is a rule that forces a rebuild when `data.h` is
missing. Here it is:

```
# More or less works, but not easy to generalize.
data.c: data.foo
        foo data.foo
data.h: data.c
## Recover from the removal of $@
        @test -f $@ || rm -f data.c
        @test -f $@ || $(MAKE) $(AM_MAKEFLAGS) data.c
```

It is tempting to use a single test as follows:

```
# This breaks make -n.
data.h: data.c
## Recover from the removal of $@
        @if test -f $@; then :; else \
          rm -f data.c; \
          $(MAKE) $(AM_MAKEFLAGS) data.c; \
        fi
```

but that would break ‘`make -n`’: at least GNU `make` and
Solaris `make` execute recipes containing the ‘`$(MAKE)`’
string even when they are running in dry mode. So if we didn’t break
the recipe above in two invocations, the file `data.c` would be
removed even upon ‘`make -n`’. Not nice.

The above scheme can be extended to handle more outputs and more
inputs. One of the outputs is selected to serve as a witness to the
successful completion of the command, it depends upon all inputs, and
all other outputs depend upon it. For instance, if `foo`
should additionally read `data.bar` and also produce
`data.w` and `data.x`, we would write:

```
data.c: data.foo data.bar
        foo data.foo data.bar
data.h data.w data.x: data.c
## Recover from the removal of $@
        @test -f $@ || rm -f data.c
        @test -f $@ || $(MAKE) $(AM_MAKEFLAGS) data.c
```

However there are now three problems in this setup. One is related to
the timestamp ordering of `data.h`, `data.w`, `data.x`,
and `data.c`. A second is a race condition if a parallel
`make` attempts to run multiple instances of the recover block
at once. Finally, the recursive rule breaks ‘`make -n`’ when run
with GNU `make` (as well as some other `make`
implementations), as it may remove `data.h` even when it should
not (see [How the `MAKE` Variable Works](https://www.gnu.org/software/make/manual/make.html#MAKE-Variable) in The GNU Make Manual).

Let us deal with the first problem. `foo` outputs four files,
but we do not know in which order these files are created. Suppose
that `data.h` is created before `data.c`. Then we have a
weird situation. The next time `make` is run, `data.h`
will appear older than `data.c`, the second rule will be
triggered, a shell will be started to execute the ‘`if…fi`’
command, but it will just execute the `then` branch, that is:
nothing. In other words, because the witness we selected is not the
first file created by `foo`, `make` will start a shell
to do nothing each time it is run.

A simple riposte is to fix the timestamps when this happens.

```
data.c: data.foo data.bar
        foo data.foo data.bar
data.h data.w data.x: data.c
        @test ! -f $@ || touch $@
## Recover from the removal of $@
        @test -f $@ || rm -f data.c
        @test -f $@ || $(MAKE) $(AM_MAKEFLAGS) data.c
```

Another solution is to use a different and dedicated file as witness,
rather than using any of `foo`’s outputs.

```
data.stamp: data.foo data.bar
        @rm -f data.tmp
        @touch data.tmp
        foo data.foo data.bar
        @mv -f data.tmp $@
data.c data.h data.w data.x: data.stamp
## Recover from the removal of $@
        @test -f $@ || rm -f data.stamp
        @test -f $@ || $(MAKE) $(AM_MAKEFLAGS) data.stamp
```

`data.tmp` is created before `foo` is run, so it has a
timestamp older than output files output by `foo`. It is then
renamed to `data.stamp` after `foo` has run, because we
do not want to update `data.stamp` if `foo` fails.

This solution still suffers from the second problem: the race
condition in the recover rule. If, after a successful build, a user
erases `data.c` and `data.h`, and runs ‘`make -j`’, then
`make` may start both recover rules in parallel. If the two
instances of the rule execute ‘`$(MAKE) $(AM_MAKEFLAGS)
data.stamp`’ concurrently the build is likely to fail (for instance, the
two rules will create `data.tmp`, but only one can rename it).

Admittedly, such a weird situation does not arise during ordinary
builds. It occurs only when the build tree is mutilated. Here
`data.c` and `data.h` have been explicitly removed without
also removing `data.stamp` and the other output files.
`make clean; make` will always recover from these situations even
with parallel makes, so you may decide that the recover rule is solely
to help non-parallel make users and leave things as-is. Fixing this
requires some locking mechanism to ensure only one instance of the
recover rule rebuilds `data.stamp`. One could imagine something
along the following lines.

```
data.c data.h data.w data.x: data.stamp
## Recover from the removal of $@
        @if test -f $@; then :; else \
          trap 'rm -rf data.lock data.stamp' 1 2 13 15; \
## mkdir is a portable test-and-set
          if mkdir data.lock 2>/dev/null; then \
## This code is being executed by the first process.
            rm -f data.stamp; \
            $(MAKE) $(AM_MAKEFLAGS) data.stamp; \
            result=$$?; rm -rf data.lock; exit $$result; \
          else \
## This code is being executed by the follower processes.
## Wait until the first process is done.
            while test -d data.lock; do sleep 1; done; \
## Succeed if and only if the first process succeeded.
            test -f data.stamp; \
          fi; \
        fi
```

Using a dedicated witness, like `data.stamp`, is very handy when
the list of output files is not known beforehand. As an illustration,
consider the following rules to compile many `*.el` files into
`*.elc` files in a single command. It does not matter how
`ELFILES` is defined (as long as it is not empty: empty targets
are not accepted by POSIX).

```
ELFILES = one.el two.el three.el ...
ELCFILES = $(ELFILES:=c)

elc-stamp: $(ELFILES)
        @rm -f elc-temp
        @touch elc-temp
        $(elisp_comp) $(ELFILES)
        @mv -f elc-temp $@

$(ELCFILES): elc-stamp
        @if test -f $@; then :; else \
## Recover from the removal of $@
          trap 'rm -rf elc-lock elc-stamp' 1 2 13 15; \
          if mkdir elc-lock 2>/dev/null; then \
## This code is being executed by the first process.
            rm -f elc-stamp; \
            $(MAKE) $(AM_MAKEFLAGS) elc-stamp; \
            rmdir elc-lock; \
          else \
## This code is being executed by the follower processes.
## Wait until the first process is done.
            while test -d elc-lock; do sleep 1; done; \
## Succeed if and only if the first process succeeded.
            test -f elc-stamp; exit $$?; \
          fi; \
        fi
```

These solutions all still suffer from the third problem, namely that
they break the promise that ‘`make -n`’ should not cause any actual
changes to the tree. For those solutions that do not create lock files,
it is possible to split the recover rules into two separate recipe
commands, one of which does all work but the recursion, and the
other invokes the recursive ‘`$(MAKE)`’. The solutions involving
locking could act upon the contents of the ‘`MAKEFLAGS`’ variable,
but parsing that portably is not easy (see [The Make Macro MAKEFLAGS](https://www.gnu.org/software/autoconf/manual/autoconf.html#The-Make-Macro-MAKEFLAGS) in The Autoconf Manual). Here is an example:

```
ELFILES = one.el two.el three.el ...
ELCFILES = $(ELFILES:=c)

elc-stamp: $(ELFILES)
        @rm -f elc-temp
        @touch elc-temp
        $(elisp_comp) $(ELFILES)
        @mv -f elc-temp $@

$(ELCFILES): elc-stamp
## Recover from the removal of $@
        @dry=; for f in x $$MAKEFLAGS; do \
          case $$f in \
            *=*|--*);; \
            *n*) dry=:;; \
          esac; \
        done; \
        if test -f $@; then :; else \
          $$dry trap 'rm -rf elc-lock elc-stamp' 1 2 13 15; \
          if $$dry mkdir elc-lock 2>/dev/null; then \
## This code is being executed by the first process.
            $$dry rm -f elc-stamp; \
            $(MAKE) $(AM_MAKEFLAGS) elc-stamp; \
            $$dry rmdir elc-lock; \
          else \
## This code is being executed by the follower processes.
## Wait until the first process is done.
            while test -d elc-lock && test -z "$$dry"; do \
              sleep 1; \
            done; \
## Succeed if and only if the first process succeeded.
            $$dry test -f elc-stamp; exit $$?; \
          fi; \
        fi
```

---

Next: [Debugging Make Rules](#Debugging-Make-Rules), Previous: [Handling Tools that Produce Many Outputs](#Multiple-Outputs), Up: [Frequently Asked Questions about Automake](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
