### 18.18 Recognizing Option Arguments [¶](#Recognizing-Option-Arguments-1)

The module ‘`argmatch`’ provides a simple textual user interface to a
finite choice. It is for example well suited to recognize arguments of
options or values of environment variables that accept a fixed set of valid
choices.

These choices may be denoted by synonyms, such as ‘none’ and ‘off’ below.

```
$ my_cp --backup=none foo bar
$ my_cp --backup=non foo bar
$ my_cp --backup=no foo bar
$ my_cp --backup=n foo bar
my_cp: ambiguous argument 'n' for 'backup type'
Valid arguments are:
  - 'no', 'none', 'off'
  - 'numbered', 't', 'newstyle'
  - 'existing', 'nil', 'numbered-existing'
  - 'simple', 'never', 'single'
Try 'my_cp --help' for more information.
$ my_cp --backup=num foo bar
$ my_cp --backup=true foo bar
my_cp: invalid argument 'true' for 'backup type'
Valid arguments are:
  - 'no', 'none', 'off'
  - 'numbered', 't', 'newstyle'
  - 'existing', 'nil', 'numbered-existing'
  - 'simple', 'never', 'single'
Try 'my_cp --help' for more information.
```

To set up `argmatch`, first call ‘`ARGMATCH_DEFINE_GROUP
(name, type)`’ with the name of the argmatch group name, and the
value type. For instance:

```
enum backup_type
{
  no_backups,
  simple_backups,
  numbered_existing_backups,
  numbered_backups
};

ARGMATCH_DEFINE_GROUP (backup, enum backup_type);
```

This defines a few types and functions named `argmatch_name_*`.
Introduce the array that defines the mapping from user-input to actual
value, with a terminator:

```
static const argmatch_backup_arg argmatch_backup_args[] =
{
  { "no",                no_backups },
  { "none",              no_backups },
  { "off",               no_backups },
  { "simple",            simple_backups },
  { "never",             simple_backups },
  { "single",            simple_backups },
  { "existing",          numbered_existing_backups },
  { "nil",               numbered_existing_backups },
  { "numbered-existing", numbered_existing_backups },
  { "numbered",          numbered_backups },
  { "t",                 numbered_backups },
  { "newstyle",          numbered_backups },
  { NULL, no_backups }
};
```

Then introduce the array that defines the values, also with a terminator.
Document only once per group of synonyms:

```
static const argmatch_backup_doc argmatch_backup_docs[] =
{
  { "no",       N_("never make backups (even if --backup is given)") },
  { "numbered", N_("make numbered backups") },
  { "existing", N_("numbered if numbered backups exist, simple otherwise") },
  { "simple",   N_("always make simple backups") },
  { NULL, NULL }
};
```

Finally, define the argmatch group:

```
const argmatch_backup_group_type argmatch_backup_group =
{
  argmatch_backup_args,
  argmatch_backup_docs,
  N_("\
The backup suffix is '~', unless set with --suffix or SIMPLE_BACKUP_SUFFIX.\n\
The version control method may be selected via the --backup option or through\n\
the VERSION_CONTROL environment variable.  Here are the values:\n"),
  NULL
};
```

  

To use the argmatch group:

```
ptrdiff_t i = argmatch_backup_choice ("--backup", "none");
// argmatch_backup_group.args[i].arg is "none", so its value
// is argmatch_backup_group.args[i].val.
// Return -1 on invalid argument, and -2 on ambiguity.

enum backup_type val = *argmatch_backup_value ("--backup", "none");
// Returns a pointer to the value, and exit on errors.
// So argmatch_backup_group.args[i].val == val.

const char *arg = argmatch_backup_argument (&no_backups);
// arg is "no".

// Print the documentation on stdout.
argmatch_backup_usage (stdout);
// Gives:
//
// The backup suffix is '~', unless set with --suffix or SIMPLE_BACKUP_SUFFIX.
// The version control method may be selected via the --backup option or through
// the VERSION_CONTROL environment variable.  Here are the values:
//
//   no, none, off  never make backups (even if --backup is given)
//   numbered, t, newstyle
//                  make numbered backups
//   existing, nil, numbered-existing
//                  numbered if numbered backups exist, simple otherwise
//   simple, never, single
//                  always make simple backups
```

---

Next: [progname and getprogname](#progname-and-getprogname), Previous: [Recognizing Option Arguments](#Recognizing-Option-Arguments), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.19 Quoting [¶](#Quoting-1)

Gnulib provides ‘`quote`’ and ‘`quotearg`’ modules to help with
quoting text, such as file names, in messages to the user. Here’s an
example of using ‘`quote`’:

```
#include <quote.h>
 ...
  error (0, errno, _("cannot change owner of %s"), quote (fname));
```

This differs from

```
  error (0, errno, _("cannot change owner of '%s'"), fname);
```

in that `quote` escapes unusual characters in
`fname`, e.g., ‘`'`’ and control characters like ‘`\n`’.

However, a caveat: `quote` reuses the storage that it returns.
Hence if you need more than one thing quoted at the same time, you
need to use `quote_n`.

Also, the `quote` module is not suited for multithreaded processes.
In that case, you have to use `quotearg_alloc`, defined in the
‘`quotearg`’ module, which is decidedly less convenient.

---

Next: [gcd: greatest common divisor](#gcd), Previous: [Quoting](#Quoting), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.20 progname and getprogname [¶](#progname-and-getprogname-1)

Gnulib has two modules for retrieving the name of the currently executing
program: `progname` and `getprogname`.

The `progname` module defines a variable `program_name`.
It contains the name of the currently executing program, on all platforms.
But it cannot be used implicitly: It requires that every `main`
function be modified to invoke `set_program_name (argv[0])` as one
of its first actions.

The `getprogname` module defines a function `getprogname()`.
It returns the name of the currently executing program, on most platforms.
The advantage of this module is that it can be used without prior
initializations. But it has limitations: In some rare situations, it
cannot determine the name; then it returns `"?"` instead. And on
some platforms, it returns a truncated program name.

The `error` function uses the `getprogname` module.

---

Next: [Profiling of program phases](#Profiling-of-program-phases), Previous: [progname and getprogname](#progname-and-getprogname), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.21 gcd: greatest common divisor [¶](#gcd_003a-greatest-common-divisor)

The `gcd` function returns the greatest common divisor of two numbers
`a > 0` and `b > 0`. It is the caller’s responsibility to ensure
that the arguments are non-zero.

If you need a gcd function for an integer type larger than
‘`unsigned long`’, you can include the `gcd.c` implementation file
with parametrization. The parameters are:

* WORD\_T
  Define this to the unsigned integer type that you need this function for.
* GCD
  Define this to the name of the function to be created.

The created function has the prototype

```
WORD_T GCD (WORD_T a, WORD_T b);
```

If you need the least common multiple of two numbers, it can be computed
like this: `lcm(a,b) = (a / gcd(a,b)) * b` or
`lcm(a,b) = a * (b / gcd(a,b))`.
Avoid the formula `lcm(a,b) = (a * b) / gcd(a,b)` because—although
mathematically correct—it can yield a wrong result, due to integer overflow.

In some applications it is useful to have a function taking the gcd of two
signed numbers. In this case, the gcd function result is usually normalized
to be non-negative (so that two gcd results can be compared in magnitude
or compared against 1, etc.). Note that in this case the prototype of the
function has to be

```
unsigned long gcd (long a, long b);
```

and not

```
long gcd (long a, long b);
```

because `gcd(LONG_MIN,LONG_MIN) = -LONG_MIN = LONG_MAX + 1` does not
fit into a signed ‘`long`’.

---

Next: [Library version handling](#Library-version-handling), Previous: [gcd: greatest common divisor](#gcd), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.22 Profiling of program phases [¶](#Profiling-of-program-phases-1)

The module ‘`timevar`’ provides a simple self-profiling facility,
based on timers.

```
Execution times (seconds)
read                  :   0.09 (19%) usr   0.08 (80%) sys   0.09 (18%) wall
read: scan            :   0.04 ( 9%) usr   0.08 (80%) sys   0.12 (26%) wall
read: parse           :   0.05 (10%) usr   0.00 ( 0%) sys   0.05 (10%) wall
work                  :   0.33 (70%) usr   0.00 ( 0%) sys   0.35 (71%) wall
work: phase 1         :   0.30 (64%) usr   0.00 ( 0%) sys   0.30 (64%) wall
work: phase 2         :   0.13 (28%) usr   0.00 ( 0%) sys   0.14 (29%) wall
output                :   0.04 ( 9%) usr   0.02 (20%) sys   0.04 ( 8%) wall
total time            :   0.47             0.10             0.49
```

To set up `timevar`, copy the stub file
`gnulib/lib/timevar.def` next to where `timevar.h` and
`timevar.c` were imported in your project, and define your timers
there. For instance:

```
/* The total execution time.  Mandatory.  */
DEFTIMEVAR (tv_total,      "total time")

/* Examples.  */
DEFTIMEVAR (tv_read,       "read")
DEFTIMEVAR (tv_work,       "work")
DEFTIMEVAR (tv_work_1,     "work: phase 1")
DEFTIMEVAR (tv_work_2,     "work: phase 2")
DEFTIMEVAR (tv_output,     "output")
```

Do not remove `tv_total`, it is mandatory. You may change its
associated string.

  

Use `timevar_push`/`timevar_pop` to start/stop timers, as in
the following example.

```
#include <config.h>
#include "timevar.h"

#include <stdio.h>
#include "read.h"
#include "work.h"
#include "output.h"

int
main (void)
{
  timevar_enabled = true;
  timevar_init ();
  timevar_start (tv_total);

  timevar_push (tv_read);
  reader ();
  timevar_pop (tv_read);

  timevar_push (tv_work);
  work ();
  timevar_pop (tv_work);

  timevar_push (tv_output);
  output ();
  timevar_pop (tv_output);

  timevar_stop (tv_total);
  timevar_print (stderr);
}
```

with, for instance, in `work.c`

```
#include <config.h>
#include "work.h"

void
work (void)
{
  timevar_push (tv_work_phase1);
  work1 ();
  timevar_pop (tv_work_phase1);

  timevar_push (tv_work_phase2);
  work2 ();
  timevar_pop (tv_work_phase2);
}
```

---

Next: [Supporting Relocation](#Supporting-Relocation), Previous: [Profiling of program phases](#Profiling-of-program-phases), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.23 Library version handling [¶](#Library-version-handling-1)

The module ‘`check-version`’ can be useful when your gnulib
application is a system library. You will typically wrap the call to
the `check_version` function through a library API, your library
header file may contain:

```
#define STRINGPREP_VERSION "0.5.18"
...
  extern const char *stringprep_check_version (const char *req_version);
```

To avoid ELF symbol collisions with other libraries that use the
‘`check-version`’ module, add to `config.h` through a
AC\_DEFINE something like:

```
AC_DEFINE(check_version, stringprep_check_version,
          [Rename check_version.])
```

The `stringprep_check_version` function will thus be implemented
by the `check_version` module.

There are two uses of the interface. The first is a way to provide
for applications to find out the version number of the library it
uses. The application may contain diagnostic code such as:

```
  printf ("Stringprep version: header %s library %s",
          STRINGPREP_VERSION,
          stringprep_check_version (NULL));
```

Separating the library and header file version can be useful when
searching for version mismatch related problems.

The second uses is as a rudimentary test of proper library version, by
making sure the application get a library version that is the same, or
newer, than the header file used when building the application. This
doesn’t catch all problems, libraries may change backwards incompatibly
in later versions, but enable applications to require a certain
minimum version before it may proceed.

Typical uses look like:

```
       /* Check version of libgcrypt. */
       if (!gcry_check_version (GCRYPT_VERSION))
         die ("version mismatch\n");
```

---

Next: [func](#func), Previous: [Library version handling](#Library-version-handling), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.24 Supporting Relocation [¶](#Supporting-Relocation-1)

It has been a pain for many users of GNU packages for a long time that
packages are not relocatable. It means a user cannot copy a program,
installed by another user on the same machine, to his home directory,
and have it work correctly (including i18n). So many users need to go
through `configure; make; make install` with all its
dependencies, options, and hurdles.

Red Hat, Debian, and other binary distributions solve the “ease of
installation” problem, but they hardwire path names, usually to
`/usr` or `/usr/local`. This means that users need root
privileges to install a binary package, and prevents installing two
different versions of the same binary package.

A relocatable program can be moved or copied to a different location
on the file system. It is possible to make symlinks to the installed
and moved programs, and invoke them through the symlink. It is
possible to do the same thing with a hard link *only* if the hard
link file is in the same directory as the real program.

The `relocatable-prog` module aims to ease the process of making a
GNU program relocatable. It helps overcome two obstacles. First, it aids
with relocating the hard-coded references to absolute file names that
GNU programs often contain. These references must be fixed up at
runtime if a program is to be successfully relocated. The
`relocatable-prog` module provides a function `relocate` that
does this job.

Second, the loader must be able to find shared libraries linked to
relocatable executables or referenced by other shared libraries linked
to relocatable executables. The `relocatable-prog` module helps out
here in a platform-specific way:

* On most operating systems, it adds a linker option (`-rpath`) that
  causes the dynamic linker to search for libraries in a directory relative
  to the location of the invoked executable. This works on GNU/Linux and
  modern versions of GNU/Hurd, GNU/kFreeBSD, macOS, FreeBSD, NetBSD, OpenBSD,
  Solaris, Haiku.
* On other Unix systems, it installs a trampoline executable. The trampoline
  sets the environment variable that controls shared library searching
  (usually `LD_LIBRARY_PATH`) and then invokes the real executable.
  This applies to operating systems such as AIX, HP-UX, or Minix.
* On Windows, the executable’s own directory is searched for libraries,
  so installing shared libraries into the executable’s directory is
  sufficient.

You can make your program relocatable by following these steps:

1. Import the `relocatable-prog` module. For libraries, use the
   `relocatable-lib` or `relocatable-lib-lgpl` module, if
   the libraries are independent. For installing multiple libraries,
   at least one of which depends on another one, use the `relocatable-prog`
   module.
   If you need more than one module, or you need to use them with different
   settings, you will need multiple copies of gnulib (see [Using Gnulib for both a library and a program](#Multiple-instances)).
2. In every program, add to `main` as the first statement (even
   before setting the locale or doing anything related to libintl):

   ```
   set_program_name (argv[0]);
   ```

   The prototype for this function is in `progname.h`.
3. If you want your code to be portable to platforms that do not support
   automatic initialization, call `set_relocation_prefix`.
4. Everywhere where you use a constant pathname from installation-time,
   wrap it in `relocate` so it gets translated to the run-time situation.
   Example:

   ```
   bindtextdomain (PACKAGE, LOCALEDIR);
   ```

   becomes:

   ```
   bindtextdomain (PACKAGE, relocate (LOCALEDIR));
   ```

   The prototype for this function is in `relocatable.h`.

   There is also a variant of this function, named `relocate2`, that
   makes it easy to reclaim the memory allocated by the call.
5. The `set_program_name` function can also configure some
   additional libraries to relocate files that they access, by defining
   corresponding C preprocessor symbols to 1. The libraries for which
   this is supported and the corresponding preprocessor symbols are:

   libcharset
   :   `DEPENDS_ON_LIBCHARSET`

   libiconv
   :   `DEPENDS_ON_LIBICONV`

   libintl
   :   `DEPENDS_ON_LIBINTL`

   Defining the symbol for a library makes every program in the package
   depend on that library, whether the program really uses the library or
   not, so this feature should be used with some caution.
6. If your package installs shell scripts, also import the
   `relocatable-script` module. Then, near the beginning of each
   shell script that your package installs, add the following:

   ```
   @relocatable_sh@

   prefix="@prefix@"
   exec_prefix="@exec_prefix@"   # usually needs $prefix.
   datarootdir="@datarootdir@"   # usually needs $prefix.

   if test "@RELOCATABLE@" = yes; then
     bindir="@bindir@"
     orig_installdir="$bindir" # see Makefile.am's *_SCRIPTS variables
     func_find_curr_installdir # determine curr_installdir
     func_find_prefixes
     relocate () {
       echo "$1/" \
       | sed -e "s%^${orig_installprefix}/%${curr_installprefix}/%" \
       | sed -e 's,/$,,'
     }
   else
     relocate () {
       echo "$1"
     }
   fi

   # Get some relocated directory names.
   sysconfdir=`relocate "@sysconfdir@"`          # usually needs $prefix.
   some_datadir=`relocate "@datadir@/something"` # usually needs $datarootdir.
   bindir=`relocate "@bindir@"`       # usually needs $exec_prefix, hence $prefix.
   ```

   You must adapt the definition of `orig_installdir`, depending on
   where the script gets installed. Also, at the end, instead of
   `sysconfdir` and `some_datadir`, transform those variables
   that you need.
7. If your package installs Perl scripts, also import the
   `relocatable-perl` module. Then, near the beginning of each
   Perl script that your package installs, add the following:

   ```
   @relocatable_pl@
   if ("@RELOCATABLE@" eq "yes") {
     my $exec_prefix = "@exec_prefix@";
     my $orig_installdir = "@bindir@"; # see Makefile.am's *_SCRIPTS variables
     my ($orig_installprefix, $curr_installprefix) =
       find_prefixes($orig_installdir, find_curr_installdir());

     # the subroutine is defined whether or not the enclosing block is executed
     sub relocate {
       my ($dir) = @_;
       if ("@RELOCATABLE@" eq "yes") {
         $dir =~ s%^$orig_installprefix/%$curr_installprefix/%;
         $dir =~ s,/$,,;
       }
       return $dir;
     }
   }

   # Get some relocated directory names.
   # (The gnulib module 'configmake' can help with this.)
   $sysconfdir = relocate("@sysconfdir@");
   $some_datadir = relocate(@datadir@/something");
   ```

   You must adapt the definition of `$orig_installdir`, depending on
   where the script gets installed. Also, at the end, instead of
   `sysconfdir` and `some_datadir`, transform those variables
   that you need.
8. In your `Makefile.am`, for every program `foo` that gets
   installed in, say, `$(bindir)`, you add:

   ```
   foo_CPPFLAGS = -DINSTALLDIR=$(bindir_c_make)
   if RELOCATABLE_VIA_LD
   foo_LDFLAGS = `$(RELOCATABLE_LDFLAGS) $(bindir)`
   endif
   ```

   Similarly, if a program `foo` gets installed in `$(pkglibdir)`,
   you add:

   ```
   foo_CPPFLAGS = -DINSTALLDIR=$(pkglibdir_c_make)
   if RELOCATABLE_VIA_LD
   foo_LDFLAGS = `$(RELOCATABLE_LDFLAGS) $(pkglibdir)`
   endif
   ```

   The Makefile variables `bindir_c_make` or `pkglibdir_c_make`
   get defined by the Autoconf macros
   `gl_BUILD_TO_HOST_BINDIR` or `gl_BUILD_TO_HOST_PKGLIBDIR`,
   respectively.
   These Autoconf macros are defined in the file `m4/build-to-host.m4`.
   You need to

   * Import this file `m4/build-to-host.m4` into your package, for example
     by using of a command like ‘`gnulib-tool --copy m4/build-to-host.m4`’.
   * Invoke the corresponding macro(s) from your package’s `configure.ac`.
9. When building gnulib to use with a relocatable library, you need to
   define the preprocessor symbol `IN_LIBRARY`.
   You may also want to build with `ENABLE_COSTLY_RELOCATABLE`, in which case
   you will also need to define `INSTALLDIR`.
   The following fragment can be added to an override `Makefile.am` used
   to build gnulib (see [Modifying the build rules of a Gnulib import directory](#Modified-build-rules)).

   ```
   AM_CPPFLAGS += -DIN_LIBRARY -DENABLE_COSTLY_RELOCATABLE

   if SHLIBS_IN_BINDIR
   AM_CPPFLAGS += -DINSTALLDIR=$(bindir_c_make)
   else
   AM_CPPFLAGS += -DINSTALLDIR=$(libdir_c_make)
   endif
   ```

   `SHLIBS_IN_BINDIR` is defined in `configure.ac` as follows:

   ```
   AM_CONDITIONAL([SHLIBS_IN_BINDIR],
                  [case "$host_os" in mingw* | cygwin*) true;; *) false;; esac])
   ```
10. In your `Makefile.am`, for every library `libfoo` that gets
    installed in, say, `$(libdir)`, you add:

    ```
    if RELOCATABLE_VIA_LD
    libfoo_la_LDFLAGS = `$(RELOCATABLE_LDFLAGS) $(libdir)`
    endif
    ```
11. Add a couple of variable assignments to your `Makefile.am`.

    If your package (or any package you rely on, e.g. gettext-runtime)
    will be relocated together with a set of installed shared libraries,
    then set `RELOCATABLE_LIBRARY_PATH` to a colon-separated list
    of those libraries’ directories, e.g.

    ```
    RELOCATABLE_LIBRARY_PATH = $(libdir)
    ```

    If your `config.h` is not in `$(top_builddir)`, then set
    `RELOCATABLE_CONFIG_H_DIR` to its directory, e.g.

    ```
    RELOCATABLE_CONFIG_H_DIR = $(top_builddir)/src
    ```

---

Next: [stat-size](#stat_002dsize), Previous: [Supporting Relocation](#Supporting-Relocation), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.25 func [¶](#func-1)

The `func` module makes sure that you can use the predefined
identifier `__func__` as defined by C99 in your code.

A small example is:

```
#include <config.h>
#include <stdio.h> /* for printf */

int main (void)
{
    printf ("%s: hello world\n", __func__);
}
```

Note that `sizeof` cannot be applied to `__func__`: On SunPRO C
compiler, `sizeof __func__` evaluates to 0.

---

Previous: [func](#func), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.26 stat-size [¶](#stat_002dsize-1)

The `stat-size` module provides a small number of macros
intended for interpreting the file size information in an instance of
`struct stat`.

On POSIX systems, the `st_blocks` member of `struct stat`
contains the number of disk blocks occupied by a file. The
`ST_NBLOCKS` and `STP_NBLOCKS` macros
estimate this quantity on systems
which don’t actually have `st_blocks`. Each of these blocks
contains `ST_NBLOCKSIZE` bytes.

The value of `ST_NBLOCKSIZE` is often quite small, small enough
that performing I/O in chunks that size would be inefficient.
The `ST_BLKSIZE` and `STP_BLKSIZE` macros
give the I/O block size recommended for I/O to this
file. This is not guaranteed to give optimum performance, but it
should be reasonably efficient.

---

Next: [Build Infrastructure Modules](#Build-Infrastructure-Modules), Previous: [Particular Modules](#Particular-Modules), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 19 Regular expressions [¶](#Regular-expressions-1)

* [Overview](#Overview)
* [Regular Expression Syntax](#Regular-Expression-Syntax)
* [Common Operators](#Common-Operators)
* [GNU Operators](#GNU-Operators)
* [What Gets Matched?](#What-Gets-Matched_003f)
* [Programming with Regex](#Programming-with-Regex)
* [Regular expression syntaxes](#Regular-expression-syntaxes)

---

Next: [Regular Expression Syntax](#Regular-Expression-Syntax), Up: [Regular expressions](#Regular-expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 19.1 Overview [¶](#Overview-1)

A *regular expression* (or *regexp*, or *pattern*) is a text
string that describes some (mathematical) set of strings. A regexp
r *matches* a string s if s is in the set of
strings described by r.

Using the Regex library, you can:

* see if a string matches a specified pattern as a whole, and
* search within a string for a substring matching a specified pattern.

Some regular expressions match only one string, i.e., the set they
describe has only one member. For example, the regular expression
‘`foo`’ matches the string ‘`foo`’ and no others. Other regular
expressions match more than one string, i.e., the set they describe has
more than one member. For example, the regular expression ‘`f*`’
matches the set of strings made up of any number (including zero) of
‘`f`’s. As you can see, some characters in regular expressions match
themselves (such as ‘`f`’) and some don’t (such as ‘`*`’); the
ones that don’t match themselves instead let you specify patterns that
describe many different strings.

To either match or search for a regular expression with the Regex
library functions, you must first compile it with a Regex pattern
compiling function. A *compiled pattern* is a regular expression
converted to the internal format used by the library functions. Once
you’ve compiled a pattern, you can use it for matching or searching any
number of times.

The Regex library is used by including `regex.h`.
Regex provides three groups of functions with which you can operate on
regular expressions. One group—the GNU group—is more
powerful but not completely compatible with the other two, namely the
POSIX and Berkeley Unix groups; its interface was designed
specifically for GNU.

We wrote this chapter with programmers in mind, not users of
programs—such as Emacs—that use Regex. We describe the Regex
library in its entirety, not how to write regular expressions that a
particular program understands.

---

Next: [Common Operators](#Common-Operators), Previous: [Overview](#Overview), Up: [Regular expressions](#Regular-expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 19.2 Regular Expression Syntax [¶](#Regular-Expression-Syntax-1)

*Characters* are things you can type. *Operators* are things in
a regular expression that match one or more characters. You compose
regular expressions from operators, which in turn you specify using one
or more characters.

Most characters represent what we call the match-self operator, i.e.,
they match themselves; we call these characters *ordinary*. Other
characters represent either all or parts of fancier operators; e.g.,
‘`.`’ represents what we call the match-any-character operator
(which, no surprise, matches (almost) any character); we call these
characters *special*. Two different things determine what
characters represent what operators:

1. the regular expression syntax your program has told the Regex library to
   recognize, and
2. the context of the character in the regular expression.

In the following sections, we describe these things in more detail.

* [Syntax Bits](#Syntax-Bits)
* [Predefined Syntaxes](#Predefined-Syntaxes)
* [Collating Elements vs. Characters](#Collating-Elements-vs_002e-Characters)
* [The Backslash Character](#The-Backslash-Character)

---

Next: [Predefined Syntaxes](#Predefined-Syntaxes), Up: [Regular Expression Syntax](#Regular-Expression-Syntax)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.2.1 Syntax Bits [¶](#Syntax-Bits-1)

In any particular syntax for regular expressions, some characters are
always special, others are sometimes special, and others are never
special. The particular syntax that Regex recognizes for a given
regular expression depends on the current syntax (as set by
`re_set_syntax`) when the pattern buffer of that regular expression
was compiled.

You get a pattern buffer by compiling a regular expression. See [GNU Pattern Buffers](#GNU-Pattern-Buffers), for more information on pattern buffers. See [GNU Regular Expression Compiling](#GNU-Regular-Expression-Compiling), and [BSD Regular Expression Compiling](#BSD-Regular-Expression-Compiling), for more information on compiling.

Regex considers the current syntax to be a collection of bits; we refer
to these bits as *syntax bits*. In most cases, they affect what
characters represent what operators. We describe the meanings of the
operators to which we refer in [Common Operators](#Common-Operators) and [GNU Operators](#GNU-Operators).

For reference, here is the complete list of syntax bits, in alphabetical
order:

`RE_BACKSLASH_ESCAPE_IN_LISTS` [¶](#index-RE_005fBACKSLASH_005fESCAPE_005fIN_005fLIST)
:   If this bit is set, then ‘`\`’ inside a list (see [List Operators (`[` … `]` and `[^` … `]`)](#List-Operators))
    quotes (makes ordinary, if it’s special) the following character; if
    this bit isn’t set, then ‘`\`’ is an ordinary character inside lists.
    (See [The Backslash Character](#The-Backslash-Character), for what ‘`\`’ does outside of lists.)

`RE_BK_PLUS_QM` [¶](#index-RE_005fBK_005fPLUS_005fQM)
:   If this bit is set, then ‘`\+`’ represents the match-one-or-more
    operator and ‘`\?`’ represents the match-zero-or-more operator; if
    this bit isn’t set, then ‘`+`’ represents the match-one-or-more
    operator and ‘`?`’ represents the match-zero-or-one operator. This
    bit is irrelevant if `RE_LIMITED_OPS` is set.

`RE_CHAR_CLASSES` [¶](#index-RE_005fCHAR_005fCLASSES)
:   If this bit is set, then you can use character classes in lists; if this
    bit isn’t set, then you can’t.

`RE_CONTEXT_INDEP_ANCHORS` [¶](#index-RE_005fCONTEXT_005fINDEP_005fANCHORS)
:   If this bit is set, then ‘`^`’ and ‘`$`’ are special anywhere outside
    a list; if this bit isn’t set, then these characters are special only in
    certain contexts. See [The Match-beginning-of-line Operator (`^`)](#Match_002dbeginning_002dof_002dline-Operator), and
    [The Match-end-of-line Operator (`$`)](#Match_002dend_002dof_002dline-Operator).

`RE_CONTEXT_INDEP_OPS` [¶](#index-RE_005fCONTEXT_005fINDEP_005fOPS)
:   If this bit is set, then certain characters are special anywhere outside
    a list; if this bit isn’t set, then those characters are special only in
    some contexts and are ordinary elsewhere. Specifically, if this bit
    isn’t set then ‘`*`’, and (if the syntax bit `RE_LIMITED_OPS`
    isn’t set) ‘`+`’ and ‘`?`’ (or ‘`\+`’ and ‘`\?`’, depending
    on the syntax bit `RE_BK_PLUS_QM`) represent repetition operators
    only if they’re not first in a regular expression or just after an
    open-group or alternation operator. The same holds for ‘`{`’ (or
    ‘`\{`’, depending on the syntax bit `RE_NO_BK_BRACES`) if
    it is the beginning of a valid interval and the syntax bit
    `RE_INTERVALS` is set.

`RE_CONTEXT_INVALID_DUP` [¶](#index-RE_005fCONTEXT_005fINVALID_005fDUP)
:   If this bit is set, then an open-interval operator cannot occur at the
    start of a regular expression, or immediately after an alternation,
    open-group or close-interval operator.

`RE_CONTEXT_INVALID_OPS` [¶](#index-RE_005fCONTEXT_005fINVALID_005fOPS)
:   If this bit is set, then repetition and alternation operators can’t be
    in certain positions within a regular expression. Specifically, the
    regular expression is invalid if it has:

    * a repetition operator first in the regular expression or just after a
      match-beginning-of-line, open-group, or alternation operator; or
    * an alternation operator first or last in the regular expression, just
      before a match-end-of-line operator, or just after an alternation or
      open-group operator.

    If this bit isn’t set, then you can put the characters representing the
    repetition and alternation characters anywhere in a regular expression.
    Whether or not they will in fact be operators in certain positions
    depends on other syntax bits.

`RE_DEBUG` [¶](#index-RE_005fDEBUG)
:   If this bit is set, and the regex library was compiled with
    `-DDEBUG`, then internal debugging is turned on; if unset, then
    it is turned off.

`RE_DOT_NEWLINE` [¶](#index-RE_005fDOT_005fNEWLINE)
:   If this bit is set, then the match-any-character operator matches
    a newline; if this bit isn’t set, then it doesn’t.

`RE_DOT_NOT_NULL` [¶](#index-RE_005fDOT_005fNOT_005fNULL)
:   If this bit is set, then the match-any-character operator doesn’t match
    a null character; if this bit isn’t set, then it does.

`RE_HAT_LISTS_NOT_NEWLINE` [¶](#index-RE_005fHAT_005fLISTS_005fNOT_005fNEWLINE)
:   If this bit is set, nonmatching lists ‘`[^...]`’ do not match
    newline; if not set, they do.

`RE_ICASE` [¶](#index-RE_005fICASE)
:   If this bit is set, then ignore case when matching; otherwise, case is
    significant.

`RE_INTERVALS` [¶](#index-RE_005fINTERVALS)
:   If this bit is set, then Regex recognizes interval operators; if this bit
    isn’t set, then it doesn’t.

`RE_INVALID_INTERVAL_ORD` [¶](#index-RE_005fINVALID_005fINTERVAL_005fORD)
:   If this bit is set, a syntactically invalid interval is treated as a
    string of ordinary characters. For example, the extended regular
    expression ‘`a{1`’ is treated as ‘`a\{1`’.

`RE_LIMITED_OPS` [¶](#index-RE_005fLIMITED_005fOPS)
:   If this bit is set, then Regex doesn’t recognize the match-one-or-more,
    match-zero-or-one or alternation operators; if this bit isn’t set, then
    it does.

`RE_NEWLINE_ALT` [¶](#index-RE_005fNEWLINE_005fALT)
:   If this bit is set, then newline represents the alternation operator; if
    this bit isn’t set, then newline is ordinary.

`RE_NO_BK_BRACES` [¶](#index-RE_005fNO_005fBK_005fBRACES)
:   If this bit is set, then ‘`{`’ represents the open-interval operator
    and ‘`}`’ represents the close-interval operator; if this bit isn’t
    set, then ‘`\{`’ represents the open-interval operator and
    ‘`\}`’ represents the close-interval operator. This bit is relevant
    only if `RE_INTERVALS` is set.

`RE_NO_BK_PARENS` [¶](#index-RE_005fNO_005fBK_005fPARENS)
:   If this bit is set, then ‘`(`’ represents the open-group operator and
    ‘`)`’ represents the close-group operator; if this bit isn’t set, then
    ‘`\(`’ represents the open-group operator and ‘`\)`’ represents
    the close-group operator.

`RE_NO_BK_REFS` [¶](#index-RE_005fNO_005fBK_005fREFS)
:   If this bit is set, then Regex doesn’t recognize ‘`\`’digit as
    the back-reference operator; if this bit isn’t set, then it does.

`RE_NO_BK_VBAR` [¶](#index-RE_005fNO_005fBK_005fVBAR)
:   If this bit is set, then ‘`|`’ represents the alternation operator;
    if this bit isn’t set, then ‘`\|`’ represents the alternation
    operator. This bit is irrelevant if `RE_LIMITED_OPS` is set.

`RE_NO_EMPTY_RANGES` [¶](#index-RE_005fNO_005fEMPTY_005fRANGES)
:   If this bit is set, then a regular expression with a range whose ending
    point collates lower than its starting point is invalid; if this bit
    isn’t set, then Regex considers such a range to be empty.

`RE_NO_GNU_OPS` [¶](#index-RE_005fNO_005fGNU_005fOPS)
:   If this bit is set, GNU regex operators are not recognized; otherwise,
    they are.

`RE_NO_POSIX_BACKTRACKING` [¶](#index-RE_005fNO_005fPOSIX_005fBACKTRACKING)
:   If this bit is set, succeed as soon as we match the whole pattern,
    without further backtracking. This means that a match may not be
    the leftmost longest; see [What Gets Matched?](#What-Gets-Matched_003f) for what this means.

`RE_NO_SUB` [¶](#index-RE_005fNO_005fSUB)
:   If this bit is set, then `no_sub` will be set to one during
    `re_compile_pattern`. This causes matching and searching routines
    not to record substring match information.

`RE_UNMATCHED_RIGHT_PAREN_ORD` [¶](#index-RE_005fUNMATCHED_005fRIGHT_005fPAREN_005fORD)
:   If this bit is set and the regular expression has no matching open-group
    operator, then Regex considers what would otherwise be a close-group
    operator (based on how `RE_NO_BK_PARENS` is set) to match ‘`)`’.

---

Next: [Collating Elements vs. Characters](#Collating-Elements-vs_002e-Characters), Previous: [Syntax Bits](#Syntax-Bits), Up: [Regular Expression Syntax](#Regular-Expression-Syntax)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.2.2 Predefined Syntaxes [¶](#Predefined-Syntaxes-1)

If you’re programming with Regex, you can set a pattern buffer’s
(see [GNU Pattern Buffers](#GNU-Pattern-Buffers))
syntax either to an arbitrary combination of syntax bits
(see [Syntax Bits](#Syntax-Bits)) or else to the configurations defined by Regex.
These configurations define the syntaxes used by certain
programs—GNU Emacs,
POSIX Awk,
traditional Awk,
Grep,

Egrep—in addition to syntaxes for POSIX basic and extended
regular expressions.

The predefined syntaxes—taken directly from `regex.h`—are:

```
# define RE_SYNTAX_EMACS                                                \
  (RE_CHAR_CLASSES | RE_INTERVALS)

#define RE_SYNTAX_AWK                                                   \
  (RE_BACKSLASH_ESCAPE_IN_LISTS | RE_DOT_NOT_NULL                       \
   | RE_NO_BK_PARENS            | RE_NO_BK_REFS                         \
   | RE_NO_BK_VBAR               | RE_NO_EMPTY_RANGES                   \
   | RE_UNMATCHED_RIGHT_PAREN_ORD)

#define RE_SYNTAX_POSIX_AWK                                             \
  (RE_SYNTAX_POSIX_EXTENDED | RE_BACKSLASH_ESCAPE_IN_LISTS)

#define RE_SYNTAX_GREP                                                  \
  (RE_BK_PLUS_QM              | RE_CHAR_CLASSES                         \
   | RE_HAT_LISTS_NOT_NEWLINE | RE_INTERVALS                            \
   | RE_NEWLINE_ALT)

#define RE_SYNTAX_EGREP                                                 \
  (RE_CHAR_CLASSES        | RE_CONTEXT_INDEP_ANCHORS                    \
   | RE_CONTEXT_INDEP_OPS | RE_HAT_LISTS_NOT_NEWLINE                    \
   | RE_NEWLINE_ALT       | RE_NO_BK_PARENS                             \
   | RE_NO_BK_VBAR)

#define RE_SYNTAX_POSIX_EGREP                                           \
  (RE_SYNTAX_EGREP | RE_INTERVALS | RE_NO_BK_BRACES)

/* P1003.2/D11.2, section 4.20.7.1, lines 5078ff.  */
#define RE_SYNTAX_ED RE_SYNTAX_POSIX_BASIC

#define RE_SYNTAX_SED RE_SYNTAX_POSIX_BASIC

/* Syntax bits common to both basic and extended POSIX regex syntax.  */
#define _RE_SYNTAX_POSIX_COMMON                                         \
  (RE_CHAR_CLASSES | RE_DOT_NEWLINE      | RE_DOT_NOT_NULL              \
   | RE_INTERVALS  | RE_NO_EMPTY_RANGES)

#define RE_SYNTAX_POSIX_BASIC                                           \
  (_RE_SYNTAX_POSIX_COMMON | RE_BK_PLUS_QM)

/* Differs from ..._POSIX_BASIC only in that RE_BK_PLUS_QM becomes
   RE_LIMITED_OPS, i.e., \? \+ \| are not recognized.  Actually, this
   isn't minimal, since other operators, such as \`, aren't disabled.  */
#define RE_SYNTAX_POSIX_MINIMAL_BASIC                                   \
  (_RE_SYNTAX_POSIX_COMMON | RE_LIMITED_OPS)

#define RE_SYNTAX_POSIX_EXTENDED                                        \
  (_RE_SYNTAX_POSIX_COMMON | RE_CONTEXT_INDEP_ANCHORS                   \
   | RE_CONTEXT_INDEP_OPS  | RE_NO_BK_BRACES                            \
   | RE_NO_BK_PARENS       | RE_NO_BK_VBAR                              \
   | RE_UNMATCHED_RIGHT_PAREN_ORD)

/* Differs from ..._POSIX_EXTENDED in that RE_CONTEXT_INVALID_OPS
   replaces RE_CONTEXT_INDEP_OPS and RE_NO_BK_REFS is added.  */
#define RE_SYNTAX_POSIX_MINIMAL_EXTENDED                                \
  (_RE_SYNTAX_POSIX_COMMON  | RE_CONTEXT_INDEP_ANCHORS                  \
   | RE_CONTEXT_INVALID_OPS | RE_NO_BK_BRACES                           \
   | RE_NO_BK_PARENS        | RE_NO_BK_REFS                             \
   | RE_NO_BK_VBAR          | RE_UNMATCHED_RIGHT_PAREN_ORD)
```

---

Next: [The Backslash Character](#The-Backslash-Character), Previous: [Predefined Syntaxes](#Predefined-Syntaxes), Up: [Regular Expression Syntax](#Regular-Expression-Syntax)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.2.3 Collating Elements vs. Characters [¶](#Collating-Elements-vs_002e-Characters-1)

POSIX generalizes the notion of a character to that of a
collating element. It defines a *collating element* to be “a
sequence of one or more bytes defined in the current collating sequence
as a unit of collation.”

This generalizes the notion of a character in
two ways. First, a single character can map into two or more collating
elements. For example, the German “ß”
collates as the collating element ‘`s`’ followed by another collating
element ‘`s`’. Second, two or more characters can map into one
collating element. For example, the Czech ‘`ch`’ collates after
‘`h`’ and before ‘`i`’.

Since POSIX’s “collating element” preserves the essential idea of
a “character,” we use the latter, more familiar, term in this document.

---

Previous: [Collating Elements vs. Characters](#Collating-Elements-vs_002e-Characters), Up: [Regular Expression Syntax](#Regular-Expression-Syntax)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.2.4 The Backslash Character [¶](#The-Backslash-Character-1)

The ‘`\`’ character has one of four different meanings, depending on
the context in which you use it and what syntax bits are set
(see [Syntax Bits](#Syntax-Bits)). It can: 1) stand for itself, 2) quote the next
character, 3) introduce an operator, or 4) do nothing.

1. It stands for itself inside a list
   (see [List Operators (`[` … `]` and `[^` … `]`)](#List-Operators)) if the syntax bit
   `RE_BACKSLASH_ESCAPE_IN_LISTS` is not set. For example, ‘`[\]`’
   would match ‘`\`’.
2. It quotes (makes ordinary, if it’s special) the next character when you
   use it either:
   * outside a list,[3](#FOOT3) or
   * inside a list and the syntax bit `RE_BACKSLASH_ESCAPE_IN_LISTS` is set.
3. It introduces an operator when followed by certain ordinary
   characters—sometimes only when certain syntax bits are set. See the
   cases `RE_BK_PLUS_QM`, `RE_NO_BK_BRACES`, `RE_NO_BK_VAR`,
   `RE_NO_BK_PARENS`, `RE_NO_BK_REF` in [Syntax Bits](#Syntax-Bits). Also:
   * ‘`\b`’ represents the match-word-boundary operator
     (see [The Match-word-boundary Operator (`\b`)](#Match_002dword_002dboundary-Operator)).
   * ‘`\B`’ represents the match-within-word operator
     (see [The Match-within-word Operator (`\B`)](#Match_002dwithin_002dword-Operator)).
   * ‘`\<`’ represents the match-beginning-of-word operator   
     (see [The Match-beginning-of-word Operator (`\<`)](#Match_002dbeginning_002dof_002dword-Operator)).
   * ‘`\>`’ represents the match-end-of-word operator
     (see [The Match-end-of-word Operator (`\>`)](#Match_002dend_002dof_002dword-Operator)).
   * ‘`\w`’ represents the match-word-constituent operator
     (see [The Match-word-constituent Operator (`\w`)](#Match_002dword_002dconstituent-Operator)).
   * ‘`\W`’ represents the match-non-word-constituent operator
     (see [The Match-non-word-constituent Operator (`\W`)](#Match_002dnon_002dword_002dconstituent-Operator)).
   * ‘`\sclass`’ is equivalent to `[[:space:]]`
     (see [The Match-space Operator (`\s`)](#Match_002dspace-Operator)).
   * ‘`\Sclass`’ is equivalent to `[^[:space]]`
     (see [The Match-non-space Operator (`\S`)](#Match_002dnon_002dspace-Operator)).
   * ‘`` \` ``’ represents the match-beginning-of-string
     operator and ‘`\'`’ represents the match-end-of-string operator
     (see [Whole-string Operators](#Whole_002dstring-Operators)).
4. In all other cases, Regex ignores ‘`\`’. For example,
   ‘`\n`’ matches ‘`n`’.

---

Next: [GNU Operators](#GNU-Operators), Previous: [Regular Expression Syntax](#Regular-Expression-Syntax), Up: [Regular expressions](#Regular-expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 19.3 Common Operators [¶](#Common-Operators-1)

You compose regular expressions from operators. In the following
sections, we describe the regular expression operators specified by
POSIX; GNU also uses these. Most operators have more than one
representation as characters. See [Regular Expression Syntax](#Regular-Expression-Syntax), for
what characters represent what operators under what circumstances.

For most operators that can be represented in two ways, one
representation is a single character and the other is that character
preceded by ‘`\`’. For example, either ‘`(`’ or ‘`\(`’
represents the open-group operator. Which one does depends on the
setting of a syntax bit, in this case `RE_NO_BK_PARENS`. Why is
this so? Historical reasons dictate some of the varying
representations, while POSIX dictates others.

Finally, almost all characters lose any special meaning inside a list
(see [List Operators (`[` … `]` and `[^` … `]`)](#List-Operators)).

* [The Match-self Operator (ordinary character)](#Match_002dself-Operator)
* [The Match-any-character Operator (`.`)](#Match_002dany_002dcharacter-Operator)
* [The Concatenation Operator](#Concatenation-Operator)
* [Repetition Operators](#Repetition-Operators)
* [The Alternation Operator (`|` or `\|`)](#Alternation-Operator)
* [List Operators (`[` … `]` and `[^` … `]`)](#List-Operators)
* [Grouping Operators (`(` … `)` or `\(` … `\)`)](#Grouping-Operators)
* [The Back-reference Operator (*\*digit)](#Back_002dreference-Operator)
* [Anchoring Operators](#Anchoring-Operators)

---

Next: [The Match-any-character Operator (`.`)](#Match_002dany_002dcharacter-Operator), Up: [Common Operators](#Common-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.1 The Match-self Operator (ordinary character) [¶](#The-Match_002dself-Operator-_0028ordinary-character_0029)

This operator matches the character itself. All ordinary characters
(see [Regular Expression Syntax](#Regular-Expression-Syntax)) represent this operator. For
example, ‘`f`’ is always an ordinary character, so the regular
expression ‘`f`’ matches only the string ‘`f`’. In
particular, it does *not* match the string ‘`ff`’.

---

Next: [The Concatenation Operator](#Concatenation-Operator), Previous: [The Match-self Operator (ordinary character)](#Match_002dself-Operator), Up: [Common Operators](#Common-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.2 The Match-any-character Operator (`.`) [¶](#The-Match_002dany_002dcharacter-Operator-_0028_002e_0029)

This operator matches any single printing or nonprinting character
except it won’t match a:

newline
:   if the syntax bit `RE_DOT_NEWLINE` isn’t set.

null
:   if the syntax bit `RE_DOT_NOT_NULL` is set.

The ‘`.`’ (period) character represents this operator. For example,
‘`a.b`’ matches any three-character string beginning with ‘`a`’
and ending with ‘`b`’.

---

Next: [Repetition Operators](#Repetition-Operators), Previous: [The Match-any-character Operator (`.`)](#Match_002dany_002dcharacter-Operator), Up: [Common Operators](#Common-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.3 The Concatenation Operator [¶](#The-Concatenation-Operator)

This operator concatenates two regular expressions a and b.
No character represents this operator; you simply put b after
a. The result is a regular expression that will match a string if
a matches its first part and b matches the rest. For
example, ‘`xy`’ (two match-self operators) matches ‘`xy`’.

---

Next: [The Alternation Operator (`|` or `\|`)](#Alternation-Operator), Previous: [The Concatenation Operator](#Concatenation-Operator), Up: [Common Operators](#Common-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.4 Repetition Operators [¶](#Repetition-Operators-1)

Repetition operators repeat the preceding regular expression a specified
number of times.

* [The Match-zero-or-more Operator (`*`)](#Match_002dzero_002dor_002dmore-Operator)
* [The Match-one-or-more Operator (`+` or `\+`)](#Match_002done_002dor_002dmore-Operator)
* [The Match-zero-or-one Operator (`?` or `\?`)](#Match_002dzero_002dor_002done-Operator)
* [Interval Operators (`{` … `}` or `\{` … `\}`)](#Interval-Operators)

---

Next: [The Match-one-or-more Operator (`+` or `\+`)](#Match_002done_002dor_002dmore-Operator), Up: [Repetition Operators](#Repetition-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.4.1 The Match-zero-or-more Operator (`*`) [¶](#The-Match_002dzero_002dor_002dmore-Operator-_0028_002a_0029)

This operator repeats the smallest possible preceding regular expression
as many times as necessary (including zero) to match the pattern.
‘`*`’ represents this operator. For example, ‘`o*`’
matches any string made up of zero or more ‘`o`’s. Since this
operator operates on the smallest preceding regular expression,
‘`fo*`’ has a repeating ‘`o`’, not a repeating ‘`fo`’. So,
‘`fo*`’ matches ‘`f`’, ‘`fo`’, ‘`foo`’, and so on.

Since the match-zero-or-more operator is a suffix operator, it may be
useless as such when no regular expression precedes it. This is the
case when it:

* is first in a regular expression, or
* follows a match-beginning-of-line, open-group, or alternation
  operator.

Three different things can happen in these cases:

1. If the syntax bit `RE_CONTEXT_INVALID_OPS` is set, then the
   regular expression is invalid.
2. If `RE_CONTEXT_INVALID_OPS` isn’t set, but
   `RE_CONTEXT_INDEP_OPS` is, then ‘`*`’ represents the
   match-zero-or-more operator (which then operates on the empty string).
3. Otherwise, ‘`*`’ is ordinary.

The matcher processes a match-zero-or-more operator by first matching as
many repetitions of the smallest preceding regular expression as it can.
Then it continues to match the rest of the pattern.

If it can’t match the rest of the pattern, it backtracks (as many times
as necessary), each time discarding one of the matches until it can
either match the entire pattern or be certain that it cannot get a
match. For example, when matching ‘`ca*ar`’ against ‘`caaar`’,
the matcher first matches all three ‘`a`’s of the string with the
‘`a*`’ of the regular expression. However, it cannot then match the
final ‘`ar`’ of the regular expression against the final ‘`r`’ of
the string. So it backtracks, discarding the match of the last ‘`a`’
in the string. It can then match the remaining ‘`ar`’.

---

Next: [The Match-zero-or-one Operator (`?` or `\?`)](#Match_002dzero_002dor_002done-Operator), Previous: [The Match-zero-or-more Operator (`*`)](#Match_002dzero_002dor_002dmore-Operator), Up: [Repetition Operators](#Repetition-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.4.2 The Match-one-or-more Operator (`+` or `\+`) [¶](#The-Match_002done_002dor_002dmore-Operator-_0028_002b-or-_005c_002b_0029)

If the syntax bit `RE_LIMITED_OPS` is set, then Regex doesn’t recognize
this operator. Otherwise, if the syntax bit `RE_BK_PLUS_QM` isn’t
set, then ‘`+`’ represents this operator; if it is, then ‘`\+`’
does.

This operator is similar to the match-zero-or-more operator except that
it repeats the preceding regular expression at least once;
see [The Match-zero-or-more Operator (`*`)](#Match_002dzero_002dor_002dmore-Operator), for what it operates on, how some
syntax bits affect it, and how Regex backtracks to match it.

For example, supposing that ‘`+`’ represents the match-one-or-more
operator; then ‘`ca+r`’ matches, e.g., ‘`car`’ and
‘`caaaar`’, but not ‘`cr`’.

---

Next: [Interval Operators (`{` … `}` or `\{` … `\}`)](#Interval-Operators), Previous: [The Match-one-or-more Operator (`+` or `\+`)](#Match_002done_002dor_002dmore-Operator), Up: [Repetition Operators](#Repetition-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.4.3 The Match-zero-or-one Operator (`?` or `\?`) [¶](#The-Match_002dzero_002dor_002done-Operator-_0028_003f-or-_005c_003f_0029)

If the syntax bit `RE_LIMITED_OPS` is set, then Regex doesn’t
recognize this operator. Otherwise, if the syntax bit
`RE_BK_PLUS_QM` isn’t set, then ‘`?`’ represents this operator;
if it is, then ‘`\?`’ does.

This operator is similar to the match-zero-or-more operator except that
it repeats the preceding regular expression once or not at all;
see [The Match-zero-or-more Operator (`*`)](#Match_002dzero_002dor_002dmore-Operator), to see what it operates on, how
some syntax bits affect it, and how Regex backtracks to match it.

For example, supposing that ‘`?`’ represents the match-zero-or-one
operator; then ‘`ca?r`’ matches both ‘`car`’ and ‘`cr`’, but
nothing else.

---

Previous: [The Match-zero-or-one Operator (`?` or `\?`)](#Match_002dzero_002dor_002done-Operator), Up: [Repetition Operators](#Repetition-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.4.4 Interval Operators (`{` … `}` or `\{` … `\}`) [¶](#Interval-Operators-_0028_007b-_002e_002e_002e-_007d-or-_005c_007b-_002e_002e_002e-_005c_007d_0029)

If the syntax bit `RE_INTERVALS` is set, then Regex recognizes
*interval expressions*. They repeat the smallest possible preceding
regular expression a specified number of times.

If the syntax bit `RE_NO_BK_BRACES` is set, ‘`{`’ represents
the *open-interval operator* and ‘`}`’ represents the
*close-interval operator* ; otherwise, ‘`\{`’ and ‘`\}`’ do.

Specifically, supposing that ‘`{`’ and ‘`}`’ represent the
open-interval and close-interval operators; then:

`{count}`
:   matches exactly count occurrences of the preceding regular
    expression.

`{min,}`
:   matches min or more occurrences of the preceding regular
    expression.

`{min, max}`
:   matches at least min but no more than max occurrences of
    the preceding regular expression.

The interval expression (but not necessarily the regular expression that
contains it) is invalid if:

* min is greater than max, or
* any of count, min, or max are outside the range
  zero to `RE_DUP_MAX` (which symbol `regex.h`
  defines).

If the interval expression is invalid and the syntax bit
`RE_NO_BK_BRACES` is set, then Regex considers all the
characters in the would-be interval to be ordinary. If that bit
isn’t set, then the regular expression is invalid.

If the interval expression is valid but there is no preceding regular
expression on which to operate, then if the syntax bit
`RE_CONTEXT_INVALID_OPS` is set, the regular expression is invalid.
If that bit isn’t set, then Regex considers all the characters—other
than backslashes, which it ignores—in the would-be interval to be
ordinary.

---

Next: [List Operators (`[` … `]` and `[^` … `]`)](#List-Operators), Previous: [Repetition Operators](#Repetition-Operators), Up: [Common Operators](#Common-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.5 The Alternation Operator (`|` or `\|`) [¶](#The-Alternation-Operator-_0028_007c-or-_005c_007c_0029)

If the syntax bit `RE_LIMITED_OPS` is set, then Regex doesn’t
recognize this operator. Otherwise, if the syntax bit
`RE_NO_BK_VBAR` is set, then ‘`|`’ represents this operator;
otherwise, ‘`\|`’ does.

Alternatives match one of a choice of regular expressions:
if you put the character(s) representing the alternation operator between
any two regular expressions a and b, the result matches
the union of the strings that a and b match. For
example, supposing that ‘`|`’ is the alternation operator, then
‘`foo|bar|quux`’ would match any of ‘`foo`’, ‘`bar`’ or
‘`quux`’.

The alternation operator operates on the *largest* possible
surrounding regular expressions. (Put another way, it has the lowest
precedence of any regular expression operator.)
Thus, the only way you can
delimit its arguments is to use grouping. For example, if ‘`(`’ and
‘`)`’ are the open and close-group operators, then ‘`fo(o|b)ar`’
would match either ‘`fooar`’ or ‘`fobar`’. (‘`foo|bar`’ would
match ‘`foo`’ or ‘`bar`’.)

The matcher usually tries all combinations of alternatives so as to
match the longest possible string. For example, when matching
‘`(fooq|foo)*(qbarquux|bar)`’ against ‘`fooqbarquux`’, it cannot
take, say, the first (“depth-first”) combination it could match, since
then it would be content to match just ‘`fooqbar`’.

Note that since the default behavior is to return the leftmost longest
match, when more than one of a series of alternatives matches the actual
match will be the longest matching alternative, not necessarily the
first in the list.

---

Next: [Grouping Operators (`(` … `)` or `\(` … `\)`)](#Grouping-Operators), Previous: [The Alternation Operator (`|` or `\|`)](#Alternation-Operator), Up: [Common Operators](#Common-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.6 List Operators (`[` … `]` and `[^` … `]`) [¶](#List-Operators-_0028_005b-_002e_002e_002e-_005d-and-_005b_005e-_002e_002e_002e-_005d_0029)

*Lists*, also called *bracket expressions*, are a set of one or
more items. An *item* is a character,
a collating symbol, an equivalence class expression,
a character class expression, or a range expression. The syntax bits
affect which kinds of items you can put in a list. We explain the last
four items in subsections below. Empty lists are invalid.

A *matching list* matches a single character represented by one of
the list items. You form a matching list by enclosing one or more items
within an *open-matching-list operator* (represented by ‘`[`’)
and a *close-list operator* (represented by ‘`]`’).

For example, ‘`[ab]`’ matches either ‘`a`’ or ‘`b`’.
‘`[ad]*`’ matches the empty string and any string composed of just
‘`a`’s and ‘`d`’s in any order. Regex considers invalid a regular
expression with a ‘`[`’ but no matching
‘`]`’.

*Nonmatching lists* are similar to matching lists except that they
match a single character *not* represented by one of the list
items. You use an *open-nonmatching-list operator* (represented by
‘`[^`’[4](#FOOT4)) instead of an open-matching-list operator to start a
nonmatching list.

For example, ‘`[^ab]`’ matches any character except ‘`a`’ or
‘`b`’.

If the syntax bit `RE_HAT_LISTS_NOT_NEWLINE` is set, then
nonmatching lists do not match a newline.

Most characters lose any special meaning inside a list. The special
characters inside a list follow.

‘`]`’
:   ends the list if it’s not the first list item. So, if you want to make
    the ‘`]`’ character a list item, you must put it first.

‘`\`’
:   quotes the next character if the syntax bit `RE_BACKSLASH_ESCAPE_IN_LISTS` is
    set.

‘`[.`’
:   represents the open-collating-symbol operator (see [Collating Symbol Operators (`[.` … `.]`)](#Collating-Symbol-Operators)).

‘`.]`’
:   represents the close-collating-symbol operator.

‘`[=`’
:   represents the open-equivalence-class operator (see [Equivalence Class Operators (`[=` … `=]`)](#Equivalence-Class-Operators)).

‘`=]`’
:   represents the close-equivalence-class operator.

‘`[:`’
:   represents the open-character-class operator (see [Character Class Operators (`[:` … `:]`)](#Character-Class-Operators)) if the syntax bit `RE_CHAR_CLASSES` is set and what
    follows is a valid character class expression.

‘`:]`’
:   represents the close-character-class operator if the syntax bit
    `RE_CHAR_CLASSES` is set and what precedes it is an
    open-character-class operator followed by a valid character class name.

‘`-`’
:   represents the range operator (see [The Range Operator (`-`)](#Range-Operator)) if it’s
    not first or last in a list or the ending point of a range.

All other characters are ordinary. For example, ‘`[.*]`’ matches
‘`.`’ and ‘`*`’.

* [Collating Symbol Operators (`[.` … `.]`)](#Collating-Symbol-Operators)
* [Equivalence Class Operators (`[=` … `=]`)](#Equivalence-Class-Operators)
* [Character Class Operators (`[:` … `:]`)](#Character-Class-Operators)
* [The Range Operator (`-`)](#Range-Operator)

---

Next: [Equivalence Class Operators (`[=` … `=]`)](#Equivalence-Class-Operators), Up: [List Operators (`[` … `]` and `[^` … `]`)](#List-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.6.1 Collating Symbol Operators (`[.` … `.]`) [¶](#Collating-Symbol-Operators-_0028_005b_002e-_002e_002e_002e-_002e_005d_0029)

Collating symbols can be represented inside lists.
You form a *collating symbol* by
putting a collating element between an *open-collating-symbol
operator* and a *close-collating-symbol operator*. ‘`[.`’
represents the open-collating-symbol operator and ‘`.]`’ represents
the close-collating-symbol operator. For example, if ‘`ll`’ is a
collating element, then ‘`[[.ll.]]`’ would match ‘`ll`’.

---

Next: [Character Class Operators (`[:` … `:]`)](#Character-Class-Operators), Previous: [Collating Symbol Operators (`[.` … `.]`)](#Collating-Symbol-Operators), Up: [List Operators (`[` … `]` and `[^` … `]`)](#List-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.6.2 Equivalence Class Operators (`[=` … `=]`) [¶](#Equivalence-Class-Operators-_0028_005b_003d-_002e_002e_002e-_003d_005d_0029)

Regex recognizes equivalence class
expressions inside lists. A *equivalence class expression* is a set
of collating elements which all belong to the same equivalence class.
You form an equivalence class expression by putting a collating
element between an *open-equivalence-class operator* and a
*close-equivalence-class operator*. ‘`[=`’ represents the
open-equivalence-class operator and ‘`=]`’ represents the
close-equivalence-class operator. For example, if ‘`a`’ and ‘`A`’
were an equivalence class, then both ‘`[[=a=]]`’ and ‘`[[=A=]]`’
would match both ‘`a`’ and ‘`A`’. If the collating element in an
equivalence class expression isn’t part of an equivalence class, then
the matcher considers the equivalence class expression to be a collating
symbol.

---

Next: [The Range Operator (`-`)](#Range-Operator), Previous: [Equivalence Class Operators (`[=` … `=]`)](#Equivalence-Class-Operators), Up: [List Operators (`[` … `]` and `[^` … `]`)](#List-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.6.3 Character Class Operators (`[:` … `:]`) [¶](#Character-Class-Operators-_0028_005b_003a-_002e_002e_002e-_003a_005d_0029)

If the syntax bit `RE_CHAR_CLASSES` is set, then Regex recognizes
character class expressions inside lists. A *character class
expression* matches one character from a given class. You form a
character class expression by putting a character class name between
an *open-character-class operator* (represented by ‘`[:`’) and
a *close-character-class operator* (represented by ‘`:]`’).
The character class names and their meanings are:

`alnum`
:   letters and digits

`alpha`
:   letters

`blank`
:   system-dependent; for GNU, a space or tab

`cntrl`
:   control characters (in the ASCII encoding, code 0177 and codes
    less than 040)

`digit`
:   digits

`graph`
:   same as `print` except omits space

`lower`
:   lowercase letters

`print`
:   printable characters (in the ASCII encoding, space
    tilde—codes 040 through 0176)

`punct`
:   neither control nor alphanumeric characters

`space`
:   space, carriage return, newline, vertical tab, and form feed

`upper`
:   uppercase letters

`xdigit`
:   hexadecimal digits: `0`–`9`, `a`–`f`, `A`–`F`

These correspond to the definitions in the C library’s `<ctype.h>`
facility. For example, ‘`[:alpha:]`’ corresponds to the standard
facility `isalpha`. Regex recognizes character class expressions
only inside of lists; so ‘`[[:alpha:]]`’ matches any letter, but
‘`[:alpha:]`’ outside of a bracket expression and not followed by a
repetition operator matches just itself.

---

Previous: [Character Class Operators (`[:` … `:]`)](#Character-Class-Operators), Up: [List Operators (`[` … `]` and `[^` … `]`)](#List-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.6.4 The Range Operator (`-`) [¶](#The-Range-Operator-_0028_002d_0029)

Regex recognizes *range expressions* inside a list. They represent
those characters
that fall between two elements in the current collating sequence. You
form a range expression by putting a *range operator* between two
of any of the following: characters, collating elements, collating symbols,
and equivalence class expressions. The starting point of the range and
the ending point of the range don’t have to be the same kind of item,
e.g., the starting point could be a collating element and the ending
point could be an equivalence class expression. If a range’s ending
point is an equivalence class, then all the collating elements in that
class will be in the range.[5](#FOOT5) ‘`-`’ represents the range operator. For example,
‘`a-f`’ within a list represents all the characters from ‘`a`’
through ‘`f`’
inclusively.

If the syntax bit `RE_NO_EMPTY_RANGES` is set, then if the range’s
ending point collates less than its starting point, the range (and the
regular expression containing it) is invalid. For example, the regular
expression ‘`[z-a]`’ would be invalid. If this bit isn’t set, then
Regex considers such a range to be empty.

Since ‘`-`’ represents the range operator, if you want to make a
‘`-`’ character itself
a list item, you must do one of the following:

* Put the ‘`-`’ either first or last in the list.
* Include a range whose starting point collates strictly lower than
  ‘`-`’ and whose ending point collates equal or higher. Unless a
  range is the first item in a list, a ‘`-`’ can’t be its starting
  point, but *can* be its ending point. That is because Regex
  considers ‘`-`’ to be the range operator unless it is preceded by
  another ‘`-`’. For example, in the ASCII encoding, ‘`)`’,
  ‘`*`’, ‘`+`’, ‘`,`’, ‘`-`’, ‘`.`’, and ‘`/`’ are
  contiguous characters in the collating sequence. You might think that
  ‘`[)-+--/]`’ has two ranges: ‘`)-+`’ and ‘`--/`’. Rather, it
  has the ranges ‘`)-+`’ and ‘`+--`’, plus the character ‘`/`’, so
  it matches, e.g., ‘`,`’, not ‘`.`’.
* Put a range whose starting point is ‘`-`’ first in the list.

For example, ‘`[-a-z]`’ matches a lowercase letter or a hyphen (in
English, in ASCII).

---

Next: [The Back-reference Operator (*\*digit)](#Back_002dreference-Operator), Previous: [List Operators (`[` … `]` and `[^` … `]`)](#List-Operators), Up: [Common Operators](#Common-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.7 Grouping Operators (`(` … `)` or `\(` … `\)`) [¶](#Grouping-Operators-_0028_0028-_002e_002e_002e-_0029-or-_005c_0028-_002e_002e_002e-_005c_0029_0029)

A *group*, also known as a *subexpression*, consists of an
*open-group operator*, any number of other operators, and a
*close-group operator*. Regex treats this sequence as a unit, just
as mathematics and programming languages treat a parenthesized
expression as a unit.

Therefore, using *groups*, you can:

* delimit the argument(s) to an alternation operator (see [The Alternation Operator (`|` or `\|`)](#Alternation-Operator)) or a repetition operator (see [Repetition Operators](#Repetition-Operators)).
* keep track of the indices of the substring that matched a given group.
  See [Using Registers](#Using-Registers), for a precise explanation.
  This lets you:
  + use the back-reference operator (see [The Back-reference Operator (*\*digit)](#Back_002dreference-Operator)).
  + use registers (see [Using Registers](#Using-Registers)).

If the syntax bit `RE_NO_BK_PARENS` is set, then ‘`(`’ represents
the open-group operator and ‘`)`’ represents the
close-group operator; otherwise, ‘`\(`’ and ‘`\)`’ do.

If the syntax bit `RE_UNMATCHED_RIGHT_PAREN_ORD` is set and a
close-group operator has no matching open-group operator, then Regex
considers it to match ‘`)`’.

---

Next: [Anchoring Operators](#Anchoring-Operators), Previous: [Grouping Operators (`(` … `)` or `\(` … `\)`)](#Grouping-Operators), Up: [Common Operators](#Common-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.8 The Back-reference Operator (*\*digit) [¶](#The-Back_002dreference-Operator-_0028_005cdigit_0029)

If the syntax bit `RE_NO_BK_REF` isn’t set, then Regex recognizes
back-references. A back-reference matches a specified preceding group.
The back-reference operator is represented by ‘`\digit`’
anywhere after the end of a regular expression’s digit-th
group (see [Grouping Operators (`(` … `)` or `\(` … `\)`)](#Grouping-Operators)).

digit must be between ‘`1`’ and ‘`9`’. The matcher assigns
numbers 1 through 9 to the first nine groups it encounters. By using
one of ‘`\1`’ through ‘`\9`’ after the corresponding group’s
close-group operator, you can match a substring identical to the
one that the group does.

Back-references match according to the following (in all examples below,
‘`(`’ represents the open-group, ‘`)`’ the close-group, ‘`{`’
the open-interval and ‘`}`’ the close-interval operator):

* If the group matches a substring, the back-reference matches an
  identical substring. For example, ‘`(a)\1`’ matches ‘`aa`’ and
  ‘`(bana)na\1bo\1`’ matches ‘`bananabanabobana`’. Likewise,
  ‘`(.*)\1`’ matches any (newline-free if the syntax bit
  `RE_DOT_NEWLINE` isn’t set) string that is composed of two
  identical halves; the ‘`(.*)`’ matches the first half and the
  ‘`\1`’ matches the second half.
* If the group matches more than once (as it might if followed
  by, e.g., a repetition operator), then the back-reference matches the
  substring the group *last* matched. For example,
  ‘`((a*)b)*\1\2`’ matches ‘`aabababa`’; first group 1 (the
  outer one) matches ‘`aab`’ and group 2 (the inner one) matches
  ‘`aa`’. Then group 1 matches ‘`ab`’ and group 2 matches
  ‘`a`’. So, ‘`\1`’ matches ‘`ab`’ and ‘`\2`’ matches
  ‘`a`’.
* If the group doesn’t participate in a match, i.e., it is part of an
  alternative not taken or a repetition operator allows zero repetitions
  of it, then the back-reference makes the whole match fail. For example,
  ‘`(one()|two())-and-(three\2|four\3)`’ matches ‘`one-and-three`’
  and ‘`two-and-four`’, but not ‘`one-and-four`’ or
  ‘`two-and-three`’. For example, if the pattern matches
  ‘`one-and-`’, then its group 2 matches the empty string and its
  group 3 doesn’t participate in the match. So, if it then matches
  ‘`four`’, then when it tries to back-reference group 3—which it
  will attempt to do because ‘`\3`’ follows the ‘`four`’—the match
  will fail because group 3 didn’t participate in the match.

You can use a back-reference as an argument to a repetition operator. For
example, ‘`(a(b))\2*`’ matches ‘`a`’ followed by two or more
‘`b`’s. Similarly, ‘`(a(b))\2{3}`’ matches ‘`abbbb`’.

If there is no preceding digit-th subexpression, the regular
expression is invalid.

Back-references can greatly slow down matching, as they can generate
exponentially many matching possibilities that can consume both time
and memory to explore. Also, the POSIX specification for
back-references is at times unclear. Furthermore, many regular
expression implementations have back-reference bugs that can cause
programs to return incorrect answers or even crash, and fixing these
bugs has often been low-priority: for example, as of 2020 the
[GNU C library bug database](https://sourceware.org/bugzilla/)
contained back-reference bugs
[52](https://sourceware.org/PR52),
[10844](https://sourceware.org/PR10844),
[11053](https://sourceware.org/PR11053),
[24269](https://sourceware.org/PR24269)
and [25322](https://sourceware.org/PR25322),
with little sign of forthcoming fixes. Luckily,
back-references are rarely useful and it should be little trouble to
avoid them in practical applications.

---

Previous: [The Back-reference Operator (*\*digit)](#Back_002dreference-Operator), Up: [Common Operators](#Common-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.9 Anchoring Operators [¶](#Anchoring-Operators-1)

These operators can constrain a pattern to match only at the beginning or
end of the entire string or at the beginning or end of a line.

* [The Match-beginning-of-line Operator (`^`)](#Match_002dbeginning_002dof_002dline-Operator)
* [The Match-end-of-line Operator (`$`)](#Match_002dend_002dof_002dline-Operator)

---

Next: [The Match-end-of-line Operator (`$`)](#Match_002dend_002dof_002dline-Operator), Up: [Anchoring Operators](#Anchoring-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.9.1 The Match-beginning-of-line Operator (`^`) [¶](#The-Match_002dbeginning_002dof_002dline-Operator-_0028_005e_0029)

This operator can match the empty string either at the beginning of the
string or after a newline character. Thus, it is said to *anchor*
the pattern to the beginning of a line.

In the cases following, ‘`^`’ represents this operator. (Otherwise,
‘`^`’ is ordinary.)

* It (the ‘`^`’) is first in the pattern, as in ‘`^foo`’.
* The syntax bit `RE_CONTEXT_INDEP_ANCHORS` is set, and it is outside
  a bracket expression.
* It follows an open-group or alternation operator, as in ‘`a\(^b\)`’
  and ‘`a\|^b`’. See [Grouping Operators (`(` … `)` or `\(` … `\)`)](#Grouping-Operators), and [The Alternation Operator (`|` or `\|`)](#Alternation-Operator).

These rules imply that some valid patterns containing ‘`^`’ cannot be
matched; for example, ‘`foo^bar`’ if `RE_CONTEXT_INDEP_ANCHORS`
is set.

If the `not_bol` field is set in the pattern buffer (see [GNU Pattern Buffers](#GNU-Pattern-Buffers)), then ‘`^`’ fails to match at the beginning of the
string. This lets you match against pieces of a line, as you would need to if,
say, searching for repeated instances of a given pattern in a line; it
would work correctly for patterns both with and without
match-beginning-of-line operators.

---

Previous: [The Match-beginning-of-line Operator (`^`)](#Match_002dbeginning_002dof_002dline-Operator), Up: [Anchoring Operators](#Anchoring-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.3.9.2 The Match-end-of-line Operator (`$`) [¶](#The-Match_002dend_002dof_002dline-Operator-_0028_0024_0029)

This operator can match the empty string either at the end of
the string or before a newline character in the string. Thus, it is
said to *anchor* the pattern to the end of a line.

It is always represented by ‘`$`’. For example, ‘`foo$`’ usually
matches, e.g., ‘`foo`’ and, e.g., the first three characters of
‘`foo\nbar`’.

Its interaction with the syntax bits and pattern buffer fields is
exactly the dual of ‘`^`’’s; see the previous section. (That is,
“‘`^`’” becomes “‘`$`’”, “beginning” becomes “end”,
“next” becomes “previous”, “after” becomes “before”, and
“`not_bol`” becomes “`not_eol`”.)

---

Next: [What Gets Matched?](#What-Gets-Matched_003f), Previous: [Common Operators](#Common-Operators), Up: [Regular expressions](#Regular-expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
