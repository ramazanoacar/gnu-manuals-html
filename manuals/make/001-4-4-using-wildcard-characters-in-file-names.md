### 4.4 Using Wildcard Characters in File Names

A single file name can specify many files using *wildcard characters*.
The wildcard characters in `make` are ‘`*`’, ‘`?`’ and
‘`[…]`’, the same as in the Bourne shell. For example, `*.c`
specifies a list of all the files (in the working directory) whose names
end in ‘`.c`’.

If an expression matches multiple files then the results will be
sorted.[2](#FOOT2) However multiple expressions will not be
globally sorted. For example, `*.c *.h` will list all the files whose
names end in ‘`.c`’, sorted, followed by all the files whose names end in
‘`.h`’, sorted.

The character ‘`~`’ at the beginning of a file name also has special
significance. If alone, or followed by a slash, it represents your home
directory. For example `~/bin` expands to `/home/you/bin`.
If the ‘`~`’ is followed by a word, the string represents the home
directory of the user named by that word. For example `~john/bin`
expands to `/home/john/bin`. On systems which don’t have a home
directory for each user (such as MS-DOS or MS-Windows), this
functionality can be simulated by setting the environment variable
HOME.

Wildcard expansion is performed by `make` automatically in
targets and in prerequisites. In recipes, the shell is responsible
for wildcard expansion. In other contexts, wildcard expansion happens
only if you request it explicitly with the `wildcard` function.

The special significance of a wildcard character can be turned off by
preceding it with a backslash. Thus, `foo\*bar` would refer to a
specific file whose name consists of ‘`foo`’, an asterisk, and
‘`bar`’.

* [Wildcard Examples](#Wildcard-Examples)
* [Pitfalls of Using Wildcards](#Wildcard-Pitfall)
* [The Function `wildcard`](#Wildcard-Function)

---

Next: [Pitfalls of Using Wildcards](#Wildcard-Pitfall), Previous: [Using Wildcard Characters in File Names](#Wildcards), Up: [Using Wildcard Characters in File Names](#Wildcards)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 4.4.1 Wildcard Examples

Wildcards can be used in the recipe of a rule, where they are expanded
by the shell. For example, here is a rule to delete all the object files:

```
clean:
        rm -f *.o
```

Wildcards are also useful in the prerequisites of a rule. With the
following rule in the makefile, ‘`make print`’ will print all the
‘`.c`’ files that have changed since the last time you printed them:

```
print: *.c
        lpr -p $?
        touch print
```

This rule uses `print` as an empty target file; see [Empty Target Files to Record Events](#Empty-Targets). (The automatic variable
‘`$?`’ is used to print only those files that have changed; see
[Automatic Variables](#Automatic-Variables).)

Wildcard expansion does not happen when you define a variable. Thus, if
you write this:

```
objects = *.o
```

then the value of the variable `objects` is the actual string
‘`*.o`’. However, if you use the value of `objects` in a
target or prerequisite, wildcard expansion will take place there. If
you use the value of `objects` in a recipe, the shell may perform
wildcard expansion when the recipe runs. To set `objects` to the
expansion, instead use:

```
objects := $(wildcard *.o)
```

See [The Function `wildcard`](#Wildcard-Function).

---

Next: [The Function `wildcard`](#Wildcard-Function), Previous: [Wildcard Examples](#Wildcard-Examples), Up: [Using Wildcard Characters in File Names](#Wildcards)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 4.4.2 Pitfalls of Using Wildcards

Now here is an example of a naive way of using wildcard expansion, that
does not do what you would intend. Suppose you would like to say that the
executable file `foo` is made from all the object files in the
directory, and you write this:

```
objects = *.o

foo : $(objects)
        cc -o foo $(CFLAGS) $(objects)
```

The value of `objects` is the actual string ‘`*.o`’. Wildcard
expansion happens in the rule for `foo`, so that each *existing*
‘`.o`’ file becomes a prerequisite of `foo` and will be recompiled if
necessary.

But what if you delete all the ‘`.o`’ files? When a wildcard matches
no files, it is left as it is, so then `foo` will depend on the
oddly-named file `*.o`. Since no such file is likely to exist,
`make` will give you an error saying it cannot figure out how to
make `*.o`. This is not what you want!

Actually it is possible to obtain the desired result with wildcard
expansion, but you need more sophisticated techniques, including the
`wildcard` function and string substitution.
See [The Function `wildcard`](#Wildcard-Function).

Microsoft operating systems (MS-DOS and MS-Windows) use backslashes to
separate directories in pathnames, like so:

```
  c:\foo\bar\baz.c
```

This is equivalent to the Unix-style `c:/foo/bar/baz.c` (the
`c:` part is the so-called drive letter). When `make` runs on
these systems, it supports backslashes as well as the Unix-style forward
slashes in pathnames. However, this support does *not* include the
wildcard expansion, where backslash is a quote character. Therefore,
you *must* use Unix-style slashes in these cases.

---

Previous: [Pitfalls of Using Wildcards](#Wildcard-Pitfall), Up: [Using Wildcard Characters in File Names](#Wildcards)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 4.4.3 The Function `wildcard`

Wildcard expansion happens automatically in rules. But wildcard expansion
does not normally take place when a variable is set, or inside the
arguments of a function. If you want to do wildcard expansion in such
places, you need to use the `wildcard` function, like this:

```
$(wildcard pattern…)
```

This string, used anywhere in a makefile, is replaced by a
space-separated list of names of existing files that match one of the
given file name patterns. If no existing file name matches a pattern,
then that pattern is omitted from the output of the `wildcard`
function. Note that this is different from how unmatched wildcards
behave in rules, where they are used verbatim rather than ignored
(see [Pitfalls of Using Wildcards](#Wildcard-Pitfall)).

As with wildcard expansion in rules, the results of the `wildcard`
function are sorted. But again, each individual expression is sorted
separately, so ‘`$(wildcard *.c *.h)`’ will expand to all files matching
‘`.c`’, sorted, followed by all files matching ‘`.h`’, sorted.

One use of the `wildcard` function is to get a list of all the C source
files in a directory, like this:

```
$(wildcard *.c)
```

We can change the list of C source files into a list of object files by
replacing the ‘`.c`’ suffix with ‘`.o`’ in the result, like this:

```
$(patsubst %.c,%.o,$(wildcard *.c))
```

(Here we have used another function, `patsubst`.
See [Functions for String Substitution and Analysis](#Text-Functions).)

Thus, a makefile to compile all C source files in the directory and then
link them together could be written as follows:

```
objects := $(patsubst %.c,%.o,$(wildcard *.c))

foo : $(objects)
        cc -o foo $(objects)
```

(This takes advantage of the implicit rule for compiling C programs, so
there is no need to write explicit rules for compiling the files.
See [The Two Flavors of Variables](#Flavors), for an explanation of
‘`:=`’, which is a variant of ‘`=`’.)

---

Next: [Phony Targets](#Phony-Targets), Previous: [Using Wildcard Characters in File Names](#Wildcards), Up: [Writing Rules](#Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 4.5 Searching Directories for Prerequisites

For large systems, it is often desirable to put sources in a separate
directory from the binaries. The *directory search* features of
`make` facilitate this by searching several directories
automatically to find a prerequisite. When you redistribute the files
among directories, you do not need to change the individual rules,
just the search paths.

* [`VPATH`: Search Path for All Prerequisites](#General-Search)
* [The `vpath` Directive](#Selective-Search)
* [How Directory Searches are Performed](#Search-Algorithm)
* [Writing Recipes with Directory Search](#Recipes_002fSearch)
* [Directory Search and Implicit Rules](#Implicit_002fSearch)
* [Directory Search for Link Libraries](#Libraries_002fSearch)

---

Next: [The `vpath` Directive](#Selective-Search), Previous: [Searching Directories for Prerequisites](#Directory-Search), Up: [Searching Directories for Prerequisites](#Directory-Search)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 4.5.1 `VPATH`: Search Path for All Prerequisites

The value of the `make` variable `VPATH` specifies a list of
directories that `make` should search. Most often, the
directories are expected to contain prerequisite files that are not in the
current directory; however, `make` uses `VPATH` as a search
list for both prerequisites and targets of rules.

Thus, if a file that is listed as a target or prerequisite does not exist
in the current directory, `make` searches the directories listed in
`VPATH` for a file with that name. If a file is found in one of
them, that file may become the prerequisite (see below). Rules may then
specify the names of files in the prerequisite list as if they all
existed in the current directory. See [Writing Recipes with Directory Search](#Recipes_002fSearch).

In the `VPATH` variable, directory names are separated by colons or
blanks. The order in which directories are listed is the order followed
by `make` in its search. (On MS-DOS and MS-Windows, semi-colons
are used as separators of directory names in `VPATH`, since the
colon can be used in the pathname itself, after the drive letter.)

For example,

```
VPATH = src:../headers
```

specifies a path containing two directories, `src` and
`../headers`, which `make` searches in that order.

With this value of `VPATH`, the following rule,

```
foo.o : foo.c
```

is interpreted as if it were written like this:

```
foo.o : src/foo.c
```

assuming the file `foo.c` does not exist in the current directory but
is found in the directory `src`.

---

Next: [How Directory Searches are Performed](#Search-Algorithm), Previous: [`VPATH`: Search Path for All Prerequisites](#General-Search), Up: [Searching Directories for Prerequisites](#Directory-Search)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 4.5.2 The `vpath` Directive

Similar to the `VPATH` variable, but more selective, is the
`vpath` directive (note lower case), which allows you to specify a
search path for a particular class of file names: those that match a
particular pattern. Thus you can supply certain search directories for
one class of file names and other directories (or none) for other file
names.

There are three forms of the `vpath` directive:

`vpath pattern directories`
:   Specify the search path directories for file names that match
    pattern.

    The search path, directories, is a list of directories to be
    searched, separated by colons (semi-colons on MS-DOS and MS-Windows) or
    blanks, just like the search path used in the `VPATH` variable.

`vpath pattern`
:   Clear out the search path associated with pattern.

`vpath`
:   Clear all search paths previously specified with `vpath` directives.

A `vpath` pattern is a string containing a ‘`%`’ character. The
string must match the file name of a prerequisite that is being searched
for, the ‘`%`’ character matching any sequence of zero or more
characters (as in pattern rules; see [Defining and
Redefining Pattern Rules](#Pattern-Rules)). For example, `%.h` matches files that
end in `.h`. (If there is no ‘`%`’, the pattern must match the
prerequisite exactly, which is not useful very often.)

‘`%`’ characters in a `vpath` directive’s pattern can be quoted
with preceding backslashes (‘`\`’). Backslashes that would otherwise
quote ‘`%`’ characters can be quoted with more backslashes.
Backslashes that quote ‘`%`’ characters or other backslashes are
removed from the pattern before it is compared to file names. Backslashes
that are not in danger of quoting ‘`%`’ characters go unmolested.

When a prerequisite fails to exist in the current directory, if the
pattern in a `vpath` directive matches the name of the
prerequisite file, then the directories in that directive are searched
just like (and before) the directories in the `VPATH` variable.

For example,

```
vpath %.h ../headers
```

tells `make` to look for any prerequisite whose name ends in `.h`
in the directory `../headers` if the file is not found in the current
directory.

If several `vpath` patterns match the prerequisite file’s name, then
`make` processes each matching `vpath` directive one by one,
searching all the directories mentioned in each directive. `make`
handles multiple `vpath` directives in the order in which they
appear in the makefile; multiple directives with the same pattern are
independent of each other.

Thus,

```
vpath %.c foo
vpath %   blish
vpath %.c bar
```

will look for a file ending in ‘`.c`’ in `foo`, then
`blish`, then `bar`, while

```
vpath %.c foo:bar
vpath %   blish
```

will look for a file ending in ‘`.c`’ in `foo`, then
`bar`, then `blish`.

---

Next: [Writing Recipes with Directory Search](#Recipes_002fSearch), Previous: [The `vpath` Directive](#Selective-Search), Up: [Searching Directories for Prerequisites](#Directory-Search)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 4.5.3 How Directory Searches are Performed

When a prerequisite is found through directory search, regardless of type
(general or selective), the pathname located may not be the one that
`make` actually provides you in the prerequisite list. Sometimes
the path discovered through directory search is thrown away.

The algorithm `make` uses to decide whether to keep or abandon a
path found via directory search is as follows:

1. If a target file does not exist at the path specified in the makefile,
   directory search is performed.
2. If the directory search is successful, that path is kept and this file
   is tentatively stored as the target.
3. All prerequisites of this target are examined using this same method.
4. After processing the prerequisites, the target may or may not need to be
   rebuilt:
   1. If the target does *not* need to be rebuilt, the path to the file
      found during directory search is used for any prerequisite lists which
      contain this target. In short, if `make` doesn’t need to rebuild
      the target then you use the path found via directory search.
   2. If the target *does* need to be rebuilt (is out-of-date), the
      pathname found during directory search is *thrown away*, and the
      target is rebuilt using the file name specified in the makefile. In
      short, if `make` must rebuild, then the target is rebuilt locally,
      not in the directory found via directory search.

This algorithm may seem complex, but in practice it is quite often
exactly what you want.

Other versions of `make` use a simpler algorithm: if the file does
not exist, and it is found via directory search, then that pathname is
always used whether or not the target needs to be built. Thus, if the
target is rebuilt it is created at the pathname discovered during
directory search.

If, in fact, this is the behavior you want for some or all of your
directories, you can use the `GPATH` variable to indicate this to
`make`.

`GPATH` has the same syntax and format as `VPATH` (that is, a
space- or colon-delimited list of pathnames). If an out-of-date target
is found by directory search in a directory that also appears in
`GPATH`, then that pathname is not thrown away. The target is
rebuilt using the expanded path.

---

Next: [Directory Search and Implicit Rules](#Implicit_002fSearch), Previous: [How Directory Searches are Performed](#Search-Algorithm), Up: [Searching Directories for Prerequisites](#Directory-Search)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 4.5.4 Writing Recipes with Directory Search

When a prerequisite is found in another directory through directory search,
this cannot change the recipe of the rule; they will execute as written.
Therefore, you must write the recipe with care so that it will look for
the prerequisite in the directory where `make` finds it.

This is done with the *automatic variables* such as ‘`$^`’
(see [Automatic Variables](#Automatic-Variables)).
For instance, the value of ‘`$^`’ is a
list of all the prerequisites of the rule, including the names of
the directories in which they were found, and the value of
‘`$@`’ is the target. Thus:

```
foo.o : foo.c
        cc -c $(CFLAGS) $^ -o $@
```

(The variable `CFLAGS` exists so you can specify flags for C
compilation by implicit rules; we use it here for consistency so it will
affect all C compilations uniformly;
see [Variables Used by Implicit Rules](#Implicit-Variables).)

Often the prerequisites include header files as well, which you do not
want to mention in the recipe. The automatic variable ‘`$<`’ is
just the first prerequisite:

```
VPATH = src:../headers
foo.o : foo.c defs.h hack.h
        cc -c $(CFLAGS) $< -o $@
```

---

Next: [Directory Search for Link Libraries](#Libraries_002fSearch), Previous: [Writing Recipes with Directory Search](#Recipes_002fSearch), Up: [Searching Directories for Prerequisites](#Directory-Search)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 4.5.5 Directory Search and Implicit Rules

The search through the directories specified in `VPATH` or with
`vpath` also happens during consideration of implicit rules
(see [Using Implicit Rules](#Implicit-Rules)).

For example, when a file `foo.o` has no explicit rule, `make`
considers implicit rules, such as the built-in rule to compile
`foo.c` if that file exists. If such a file is lacking in the
current directory, the appropriate directories are searched for it. If
`foo.c` exists (or is mentioned in the makefile) in any of the
directories, the implicit rule for C compilation is applied.

The recipes of implicit rules normally use automatic variables as a
matter of necessity; consequently they will use the file names found by
directory search with no extra effort.

---

Previous: [Directory Search and Implicit Rules](#Implicit_002fSearch), Up: [Searching Directories for Prerequisites](#Directory-Search)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 4.5.6 Directory Search for Link Libraries

Directory search applies in a special way to libraries used with the
linker. This special feature comes into play when you write a prerequisite
whose name is of the form ‘`-lname`’. (You can tell something
strange is going on here because the prerequisite is normally the name of a
file, and the *file name* of a library generally looks like
`libname.a`, not like ‘`-lname`’.)

When a prerequisite’s name has the form ‘`-lname`’, `make`
handles it specially by searching for the file `libname.so`,
and, if it is not found, for the file `libname.a` in the current
directory, in directories specified by matching `vpath`
search paths and the `VPATH` search path, and then in the
directories `/lib`, `/usr/lib`, and `prefix/lib`
(normally `/usr/local/lib`, but MS-DOS/MS-Windows versions of
`make` behave as if prefix is defined to be the root of the
DJGPP installation tree).

For example, if there is a `/usr/lib/libcurses.a` library on your
system (and no `/usr/lib/libcurses.so` file), then

```
foo : foo.c -lcurses
        cc $^ -o $@
```

would cause the command ‘`cc foo.c /usr/lib/libcurses.a -o foo`’ to
be executed when `foo` is older than `foo.c` or than
`/usr/lib/libcurses.a`.

Although the default set of files to be searched for is
`libname.so` and `libname.a`, this is customizable
via the `.LIBPATTERNS` variable. Each word in the value of this
variable is a pattern string. When a prerequisite like
‘`-lname`’ is seen, `make` will replace the percent in
each pattern in the list with name and perform the above directory
searches using each library file name.

The default value for `.LIBPATTERNS` is ‘`lib%.so lib%.a`’,
which provides the default behavior described above.

You can turn off link library expansion completely by setting this
variable to an empty value.

---

Next: [Rules without Recipes or Prerequisites](#Force-Targets), Previous: [Searching Directories for Prerequisites](#Directory-Search), Up: [Writing Rules](#Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 4.6 Phony Targets

A phony target is one that is not really the name of a file; rather it
is just a name for a recipe to be executed when you make an explicit
request. There are two reasons to use a phony target: to avoid a
conflict with a file of the same name, and to improve performance.

If you write a rule whose recipe will not create the target file, the
recipe will be executed every time the target comes up for remaking.
Here is an example:

```
clean:
        rm *.o temp
```

Because the `rm` command does not create a file named `clean`,
probably no such file will ever exist. Therefore, the `rm` command
will be executed every time you say ‘`make clean`’.

In this example, the `clean` target will not work properly if a
file named `clean` is ever created in this directory. Since it
has no prerequisites, `clean` would always be considered up to
date and its recipe would not be executed. To avoid this problem you
can explicitly declare the target to be phony by making it a
prerequisite of the special target `.PHONY`
(see [Special Built-in Target Names](#Special-Targets)) as follows:

```
.PHONY: clean
clean:
        rm *.o temp
```

Once this is done, ‘`make clean`’ will run the recipe regardless of
whether there is a file named `clean`.

Prerequisites of `.PHONY` are always interpreted as literal
target names, never as patterns (even if they contain ‘`%`’
characters). To always rebuild a pattern rule consider using a
“force target” (see [Rules without Recipes or
Prerequisites](#Force-Targets)).

Phony targets are also useful in conjunction with recursive
invocations of `make` (see [Recursive Use of `make`](#Recursion)).
In this situation the makefile will often contain a variable which
lists a number of sub-directories to be built. A simplistic way to
handle this is to define one rule with a recipe that loops over the
sub-directories, like this:

```
SUBDIRS = foo bar baz

subdirs:
        for dir in $(SUBDIRS); do \
          $(MAKE) -C $$dir; \
        done
```

There are problems with this method, however. First, any error detected in a
sub-make is ignored by this rule, so it will continue to build the rest of the
directories even when one fails. This can be overcome by adding shell
commands to note the error and exit, but then it will do so even if
`make` is invoked with the `-k` option, which is unfortunate.
Second, and perhaps more importantly, you cannot take full advantage of
`make`’s ability to build targets in parallel (see [Parallel
Execution](#Parallel)), since there is only one rule. Each individual makefile’s targets
will be built in parallel, but only one sub-directory will be built at a time.

By declaring the sub-directories as `.PHONY` targets (you must do
this as the sub-directory obviously always exists; otherwise it won’t
be built) you can remove these problems:

```
SUBDIRS = foo bar baz

.PHONY: subdirs $(SUBDIRS)

subdirs: $(SUBDIRS)

$(SUBDIRS):
        $(MAKE) -C $@

foo: baz
```

Here we’ve also declared that the `foo` sub-directory cannot be
built until after the `baz` sub-directory is complete; this kind of
relationship declaration is particularly important when attempting
parallel builds.

The implicit rule search (see [Using Implicit Rules](#Implicit-Rules)) is skipped for
`.PHONY` targets. This is why declaring a target as
`.PHONY` is good for performance, even if you are not worried
about the actual file existing.

A phony target should not be a prerequisite of a real target file; if it is,
its recipe will be run every time `make` considers that file. As long as
a phony target is never a prerequisite of a real target, the phony target
recipe will be executed only when the phony target is a specified goal
(see [Arguments to Specify the Goals](#Goals)).

You should not declare an included makefile as phony. Phony targets are not
intended to represent real files, and because the target is always considered
out of date make will always rebuild it then re-execute itself
(see [How Makefiles Are Remade](#Remaking-Makefiles)). To avoid this,
`make` will not re-execute itself if an included file marked as phony is
re-built.

Phony targets can have prerequisites. When one directory contains multiple
programs, it is most convenient to describe all of the programs in one
makefile `./Makefile`. Since the target remade by default will be the
first one in the makefile, it is common to make this a phony target named
‘`all`’ and give it, as prerequisites, all the individual programs. For
example:

```
all : prog1 prog2 prog3
.PHONY : all

prog1 : prog1.o utils.o
        cc -o prog1 prog1.o utils.o

prog2 : prog2.o
        cc -o prog2 prog2.o

prog3 : prog3.o sort.o utils.o
        cc -o prog3 prog3.o sort.o utils.o
```

Now you can say just ‘`make`’ to remake all three programs, or
specify as arguments the ones to remake (as in ‘`make prog1
prog3`’). Phoniness is not inherited: the prerequisites of a phony
target are not themselves phony, unless explicitly declared to be so.

When one phony target is a prerequisite of another, it serves as a subroutine
of the other. For example, here ‘`make cleanall`’ will delete the
object files, the difference files, and the file `program`:

```
.PHONY: cleanall cleanobj cleandiff

cleanall : cleanobj cleandiff
        rm program

cleanobj :
        rm *.o

cleandiff :
        rm *.diff
```

---

Next: [Empty Target Files to Record Events](#Empty-Targets), Previous: [Phony Targets](#Phony-Targets), Up: [Writing Rules](#Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 4.7 Rules without Recipes or Prerequisites

If a rule has no prerequisites or recipe, and the target of the rule
is a nonexistent file, then `make` imagines this target to have
been updated whenever its rule is run. This implies that all targets
depending on this one will always have their recipe run.

An example will illustrate this:

```
clean: FORCE
        rm $(objects)
FORCE:
```

Here the target ‘`FORCE`’ satisfies the special conditions, so the
target `clean` that depends on it is forced to run its recipe.
There is nothing special about the name ‘`FORCE`’, but that is one
name commonly used this way.

As you can see, using ‘`FORCE`’ this way has the same results as using
‘`.PHONY: clean`’.

Using ‘`.PHONY`’ is more explicit and more efficient. However,
other versions of `make` do not support ‘`.PHONY`’; thus
‘`FORCE`’ appears in many makefiles. See [Phony Targets](#Phony-Targets).

---

Next: [Special Built-in Target Names](#Special-Targets), Previous: [Rules without Recipes or Prerequisites](#Force-Targets), Up: [Writing Rules](#Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 4.8 Empty Target Files to Record Events

The *empty target* is a variant of the phony target; it is used to hold
recipes for an action that you request explicitly from time to time.
Unlike a phony target, this target file can really exist; but the file’s
contents do not matter, and usually are empty.

The purpose of the empty target file is to record, with its
last-modification time, when the rule’s recipe was last executed. It
does so because one of the commands in the recipe is a `touch`
command to update the target file.

The empty target file should have some prerequisites (otherwise it
doesn’t make sense). When you ask to remake the empty target, the
recipe is executed if any prerequisite is more recent than the target;
in other words, if a prerequisite has changed since the last time you
remade the target. Here is an example:

```
print: foo.c bar.c
        lpr -p $?
        touch print
```

With this rule, ‘`make print`’ will execute the `lpr` command if
either source file has changed since the last ‘`make print`’. The
automatic variable ‘`$?`’ is used to print only those files that have
changed (see [Automatic Variables](#Automatic-Variables)).

---

Next: [Multiple Targets in a Rule](#Multiple-Targets), Previous: [Empty Target Files to Record Events](#Empty-Targets), Up: [Writing Rules](#Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 4.9 Special Built-in Target Names

Certain names have special meanings if they appear as targets.

`.PHONY`
:   The prerequisites of the special target `.PHONY` are considered to
    be phony targets. When it is time to consider such a target,
    `make` will run its recipe unconditionally, regardless of
    whether a file with that name exists or what its last-modification
    time is. See [Phony Targets](#Phony-Targets).

`.SUFFIXES`
:   The prerequisites of the special target `.SUFFIXES` are the list
    of suffixes to be used in checking for suffix rules.
    See [Old-Fashioned Suffix Rules](#Suffix-Rules).

`.DEFAULT`
:   The recipe specified for `.DEFAULT` is used for any target for
    which no rules are found (either explicit rules or implicit rules).
    See [Defining Last-Resort Default Rules](#Last-Resort). If a `.DEFAULT` recipe is specified, every
    file mentioned as a prerequisite, but not as a target in a rule, will have
    that recipe executed on its behalf. See [Implicit Rule Search Algorithm](#Implicit-Rule-Search).

`.PRECIOUS` [¶](#index-precious-targets)
:   The targets which `.PRECIOUS` depends on are given the following
    special treatment: if `make` is killed or interrupted during the
    execution of their recipes, the target is not deleted.
    See [Interrupting or Killing `make`](#Interrupts). Also, if the
    target is an intermediate file, it will not be deleted after it is no
    longer needed, as is normally done. See [Chains of
    Implicit Rules](#Chained-Rules). In this latter respect it overlaps with the
    `.SECONDARY` special target.

    You can also list the target pattern of an implicit rule (such as
    ‘`%.o`’) as a prerequisite file of the special target `.PRECIOUS`
    to preserve intermediate files created by rules whose target patterns
    match that file’s name.

`.INTERMEDIATE` [¶](#index-intermediate-targets_002c-explicit)
:   The targets which `.INTERMEDIATE` depends on are treated as
    intermediate files. See [Chains of Implicit Rules](#Chained-Rules).
    `.INTERMEDIATE` with no prerequisites has no effect.

`.NOTINTERMEDIATE` [¶](#index-not-intermediate-targets_002c-explicit)
:   Prerequisites of the special target `.NOTINTERMEDIATE` are never
    considered intermediate files. See [Chains of Implicit Rules](#Chained-Rules).
    `.NOTINTERMEDIATE` with no prerequisites causes all targets to be treated
    as not intermediate.

    If the prerequisite is a target pattern then targets that are built using that
    pattern rule are not considered intermediate.

`.SECONDARY` [¶](#index-secondary-targets)
:   The targets which `.SECONDARY` depends on are treated as
    intermediate files, except that they are never automatically deleted.
    See [Chains of Implicit Rules](#Chained-Rules).

    `.SECONDARY` can be used to avoid redundant rebuilds in some unusual
    situations. For example:

    ```
    hello.bin: hello.o bye.o
            $(CC) -o $@ $^

    %.o: %.c
            $(CC) -c -o $@ $<

    .SECONDARY: hello.o bye.o
    ```

    Suppose `hello.bin` is up to date in regards to the source files,
    *but* the object file `hello.o` is missing. Without
    `.SECONDARY` make would rebuild `hello.o` then rebuild
    `hello.bin` even though the source files had not changed. By declaring
    `hello.o` as `.SECONDARY` `make` will not need to rebuild it
    and won’t need to rebuild `hello.bin` either. Of course, if one of the
    source files *were* updated then all object files would be rebuilt so
    that the creation of `hello.bin` could succeed.

    `.SECONDARY` with no prerequisites causes all targets to be treated
    as secondary (i.e., no target is removed because it is considered
    intermediate).

`.SECONDEXPANSION`
:   If `.SECONDEXPANSION` is mentioned as a target anywhere in the
    makefile, then all prerequisite lists defined *after* it appears
    will be expanded a second time after all makefiles have been read in.
    See [Secondary Expansion](#Secondary-Expansion).

`.DELETE_ON_ERROR` [¶](#index-removing-targets-on-failure)
:   If `.DELETE_ON_ERROR` is mentioned as a target anywhere in the
    makefile, then `make` will delete the target of a rule if it has
    changed and its recipe exits with a nonzero exit status, just as it
    does when it receives a signal. See [Errors in Recipes](#Errors).

`.IGNORE`
:   If you specify prerequisites for `.IGNORE`, then `make` will
    ignore errors in execution of the recipe for those particular files.
    The recipe for `.IGNORE` (if any) is ignored.

    If mentioned as a target with no prerequisites, `.IGNORE` says to
    ignore errors in execution of recipes for all files. This usage of
    ‘`.IGNORE`’ is supported only for historical compatibility. Since
    this affects every recipe in the makefile, it is not very useful; we
    recommend you use the more selective ways to ignore errors in specific
    recipes. See [Errors in Recipes](#Errors).

`.LOW_RESOLUTION_TIME`
:   If you specify prerequisites for `.LOW_RESOLUTION_TIME`,
    `make` assumes that these files are created by commands that
    generate low resolution time stamps. The recipe for the
    `.LOW_RESOLUTION_TIME` target are ignored.

    The high resolution file time stamps of many modern file systems
    lessen the chance of `make` incorrectly concluding that a file
    is up to date. Unfortunately, some hosts do not provide a way to set a
    high resolution file time stamp, so commands like ‘`cp -p`’ that
    explicitly set a file’s time stamp must discard its sub-second part.
    If a file is created by such a command, you should list it as a
    prerequisite of `.LOW_RESOLUTION_TIME` so that `make`
    does not mistakenly conclude that the file is out of date. For
    example:

    ```
    .LOW_RESOLUTION_TIME: dst
    dst: src
            cp -p src dst
    ```

    Since ‘`cp -p`’ discards the sub-second part of `src`’s time
    stamp, `dst` is typically slightly older than `src` even when
    it is up to date. The `.LOW_RESOLUTION_TIME` line causes
    `make` to consider `dst` to be up to date if its time stamp
    is at the start of the same second that `src`’s time stamp is in.

    Due to a limitation of the archive format, archive member time stamps
    are always low resolution. You need not list archive members as
    prerequisites of `.LOW_RESOLUTION_TIME`, as `make` does this
    automatically.

`.SILENT`
:   If you specify prerequisites for `.SILENT`, then `make` will
    not print the recipe used to remake those particular files before
    executing them. The recipe for `.SILENT` is ignored.

    If mentioned as a target with no prerequisites, `.SILENT` says
    not to print any recipes before executing them. You may also use more
    selective ways to silence specific recipe command lines.
    See [Recipe Echoing](#Echoing). If you want to silence all recipes
    for a particular run of `make`, use the ‘`-s`’ or
    ‘`--silent`’ option (see [Summary of Options](#Options-Summary)).

`.EXPORT_ALL_VARIABLES`
:   Simply by being mentioned as a target, this tells `make` to export all
    variables to child processes by default. This is an alternative to using
    `export` with no arguments. See [Communicating
    Variables to a Sub-`make`](#Variables_002fRecursion).

`.NOTPARALLEL` [¶](#index-parallel-execution_002c-overriding)
:   If `.NOTPARALLEL` is mentioned as a target with no prerequisites, all
    targets in this invocation of `make` will be run serially, even if the
    ‘`-j`’ option is given. Any recursively invoked `make` command will
    still run recipes in parallel (unless its makefile also contains this target).

    If `.NOTPARALLEL` has targets as prerequisites, then all the
    prerequisites of those targets will be run serially. This implicitly adds a
    `.WAIT` between each prerequisite of the listed targets. See [Disabling Parallel Execution](#Parallel-Disable).

`.ONESHELL` [¶](#index-recipe-execution_002c-single-invocation)
:   If `.ONESHELL` is mentioned as a target, then when a target is
    built all lines of the recipe will be given to a single invocation of
    the shell rather than each line being invoked separately.
    See [Recipe Execution](#Execution).

`.POSIX` [¶](#index-POSIX_002dconforming-mode_002c-setting)
:   If `.POSIX` is mentioned as a target, then the makefile will be
    parsed and run in POSIX-conforming mode. This does *not* mean
    that only POSIX-conforming makefiles will be accepted: all advanced
    GNU `make` features are still available. Rather, this target
    causes `make` to behave as required by POSIX in those areas
    where `make`’s default behavior differs.

    In particular, if this target is mentioned then recipes will be
    invoked as if the shell had been passed the `-e` flag: the first
    failing command in a recipe will cause the recipe to fail immediately.

Any defined implicit rule suffix also counts as a special target if it
appears as a target, and so does the concatenation of two suffixes, such
as ‘`.c.o`’. These targets are suffix rules, an obsolete way of
defining implicit rules (but a way still widely used). In principle, any
target name could be special in this way if you break it in two and add
both pieces to the suffix list. In practice, suffixes normally begin with
‘`.`’, so these special target names also begin with ‘`.`’.
See [Old-Fashioned Suffix Rules](#Suffix-Rules).

---

Next: [Multiple Rules for One Target](#Multiple-Rules), Previous: [Special Built-in Target Names](#Special-Targets), Up: [Writing Rules](#Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 4.10 Multiple Targets in a Rule

When an explicit rule has multiple targets they can be treated in one
of two possible ways: as independent targets or as grouped targets.
The manner in which they are treated is determined by the separator that
appears after the list of targets.

#### Rules with Independent Targets

Rules that use the standard target separator, `:`, define
independent targets. This is equivalent to writing the same rule once
for each target, with duplicated prerequisites and recipes. Typically,
the recipe would use automatic variables such as ‘`$@`’ to specify
which target is being built.

Rules with independent targets are useful in two cases:

* You want just prerequisites, no recipe. For example:

  ```
  kbd.o command.o files.o: command.h
  ```

  gives an additional prerequisite to each of the three object files
  mentioned. It is equivalent to writing:

  ```
  kbd.o: command.h
  command.o: command.h
  files.o: command.h
  ```
* Similar recipes work for all the targets. The automatic variable
  ‘`$@`’ can be used to substitute the particular target to be
  remade into the commands (see [Automatic Variables](#Automatic-Variables)). For example:

  ```
  bigoutput littleoutput : text.g
          generate text.g -$(subst output,,$@) > $@
  ```

  is equivalent to

  ```
  bigoutput : text.g
          generate text.g -big > bigoutput
  littleoutput : text.g
          generate text.g -little > littleoutput
  ```

  Here we assume the hypothetical program `generate` makes two
  types of output, one if given ‘`-big`’ and one if given
  ‘`-little`’.
  See [Functions for String Substitution and Analysis](#Text-Functions),
  for an explanation of the `subst` function.

Suppose you would like to vary the prerequisites according to the
target, much as the variable ‘`$@`’ allows you to vary the recipe.
You cannot do this with multiple targets in an ordinary rule, but you
can do it with a *static pattern rule*. See [Static Pattern Rules](#Static-Pattern).

#### Rules with Grouped Targets

If instead of independent targets you have a recipe that generates multiple
files from a single invocation, you can express that relationship by declaring
your rule to use *grouped targets*. A grouped target rule uses the
separator `&:` (the ‘`&`’ here is used to imply “all”).

When `make` builds any one of the grouped targets, it understands that
all the other targets in the group are also updated as a result of the
invocation of the recipe. Furthermore, if only some of the grouped targets
are out of date or missing `make` will realize that running the recipe
will update all of the targets. Finally, if any of the grouped targets are
out of date, all the grouped targets are considered out of date.

As an example, this rule defines a grouped target:

```
foo bar biz &: baz boz
        echo $^ > foo
        echo $^ > bar
        echo $^ > biz
```

During the execution of a grouped target’s recipe, the automatic
variable ‘`$@`’ is set to the name of the particular target in the
group which triggered the rule. Caution must be used if relying on
this variable in the recipe of a grouped target rule.

Unlike independent targets, a grouped target rule *must* include
a recipe. However, targets that are members of a grouped target may
also appear in independent target rule definitions that do not have
recipes.

Each target may have only one recipe associated with it. If a grouped
target appears in either an independent target rule or in another
grouped target rule with a recipe, you will get a warning and the
latter recipe will replace the former recipe. Additionally the target
will be removed from the previous group and appear only in the new
group.

If you would like a target to appear in multiple groups, then you must
use the double-colon grouped target separator, `&::` when
declaring all of the groups containing that target. Grouped
double-colon targets are each considered independently, and each
grouped double-colon rule’s recipe is executed at most once, if at
least one of its multiple targets requires updating.

---

Next: [Static Pattern Rules](#Static-Pattern), Previous: [Multiple Targets in a Rule](#Multiple-Targets), Up: [Writing Rules](#Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 4.11 Multiple Rules for One Target

One file can be the target of several rules. All the prerequisites
mentioned in all the rules are merged into one list of prerequisites for
the target. If the target is older than any prerequisite from any rule,
the recipe is executed.

There can only be one recipe to be executed for a file. If more than
one rule gives a recipe for the same file, `make` uses the last
one given and prints an error message. (As a special case, if the
file’s name begins with a dot, no error message is printed. This odd
behavior is only for compatibility with other implementations of
`make`… you should avoid using it). Occasionally it is
useful to have the same target invoke multiple recipes which are
defined in different parts of your makefile; you can use
*double-colon rules* (see [Double-Colon Rules](#Double_002dColon)) for this.

An extra rule with just prerequisites can be used to give a few extra
prerequisites to many files at once. For example, makefiles often
have a variable, such as `objects`, containing a list of all the
compiler output files in the system being made. An easy way to say
that all of them must be recompiled if `config.h` changes is to
write the following:

```
objects = foo.o bar.o
foo.o : defs.h
bar.o : defs.h test.h
$(objects) : config.h
```

This could be inserted or taken out without changing the rules that really
specify how to make the object files, making it a convenient form to use if
you wish to add the additional prerequisite intermittently.

Another wrinkle is that the additional prerequisites could be
specified with a variable that you set with a command line argument to
`make` (see [Overriding Variables](#Overriding)). For example,

```
extradeps=
$(objects) : $(extradeps)
```

means that the command ‘`make extradeps=foo.h`’ will consider
`foo.h` as a prerequisite of each object file, but plain ‘`make`’
will not.

If none of the explicit rules for a target has a recipe, then `make`
searches for an applicable implicit rule to find one
see [Using Implicit Rules](#Implicit-Rules)).

---

Next: [Double-Colon Rules](#Double_002dColon), Previous: [Multiple Rules for One Target](#Multiple-Rules), Up: [Writing Rules](#Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 4.12 Static Pattern Rules

*Static pattern rules* are rules which specify multiple targets and
construct the prerequisite names for each target based on the target name.
They are more general than ordinary rules with multiple targets because the
targets do not have to have identical prerequisites. Their prerequisites must
be *analogous*, but not necessarily *identical*.

* [Syntax of Static Pattern Rules](#Static-Usage)
* [Static Pattern Rules versus Implicit Rules](#Static-versus-Implicit)

---

Next: [Static Pattern Rules versus Implicit Rules](#Static-versus-Implicit), Previous: [Static Pattern Rules](#Static-Pattern), Up: [Static Pattern Rules](#Static-Pattern)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 4.12.1 Syntax of Static Pattern Rules

Here is the syntax of a static pattern rule:

```
targets …: target-pattern: prereq-patterns …
        recipe
        …
```

The targets list specifies the targets that the rule applies to.
The targets can contain wildcard characters, just like the targets of
ordinary rules (see [Using Wildcard Characters in File
Names](#Wildcards)).

The target-pattern and prereq-patterns say how to compute the
prerequisites of each target. Each target is matched against the
target-pattern to extract a part of the target name, called the
*stem*. This stem is substituted into each of the prereq-patterns
to make the prerequisite names (one from each prereq-pattern).

Each pattern normally contains the character ‘`%`’ just once. When the
target-pattern matches a target, the ‘`%`’ can match any part of
the target name; this part is called the *stem*. The rest of the
pattern must match exactly. For example, the target `foo.o` matches
the pattern ‘`%.o`’, with ‘`foo`’ as the stem. The targets
`foo.c` and `foo.out` do not match that pattern.

The prerequisite names for each target are made by substituting the stem
for the ‘`%`’ in each prerequisite pattern. For example, if one
prerequisite pattern is `%.c`, then substitution of the stem
‘`foo`’ gives the prerequisite name `foo.c`. It is legitimate
to write a prerequisite pattern that does not contain ‘`%`’; then this
prerequisite is the same for all targets.

‘`%`’ characters in pattern rules can be quoted with preceding
backslashes (‘`\`’). Backslashes that would otherwise quote ‘`%`’
characters can be quoted with more backslashes. Backslashes that quote
‘`%`’ characters or other backslashes are removed from the pattern
before it is compared to file names or has a stem substituted into it.
Backslashes that are not in danger of quoting ‘`%`’ characters go
unmolested. For example, the pattern `the\%weird\\%pattern\\` has
‘`the%weird\`’ preceding the operative ‘`%`’ character, and
‘`pattern\\`’ following it. The final two backslashes are left alone
because they cannot affect any ‘`%`’ character.

Here is an example, which compiles each of `foo.o` and `bar.o`
from the corresponding `.c` file:

```
objects = foo.o bar.o

all: $(objects)

$(objects): %.o: %.c
        $(CC) -c $(CFLAGS) $< -o $@
```

Here ‘`$<`’ is the automatic variable that holds the name of the
prerequisite and ‘`$@`’ is the automatic variable that holds the name
of the target; see [Automatic Variables](#Automatic-Variables).

Each target specified must match the target pattern; a warning is issued
for each target that does not. If you have a list of files, only some of
which will match the pattern, you can use the `filter` function to
remove non-matching file names (see [Functions for String Substitution and Analysis](#Text-Functions)):

```
files = foo.elc bar.o lose.o

$(filter %.o,$(files)): %.o: %.c
        $(CC) -c $(CFLAGS) $< -o $@
$(filter %.elc,$(files)): %.elc: %.el
        emacs -f batch-byte-compile $<
```

In this example the result of ‘`$(filter %.o,$(files))`’ is
`bar.o lose.o`, and the first static pattern rule causes each of
these object files to be updated by compiling the corresponding C source
file. The result of ‘`$(filter %.elc,$(files))`’ is
`foo.elc`, so that file is made from `foo.el`.

Another example shows how to use `$*` in static pattern rules:

```
bigoutput littleoutput : %output : text.g
        generate text.g -$* > $@
```

When the `generate` command is run, `$*` will expand to the
stem, either ‘`big`’ or ‘`little`’.

---

Previous: [Syntax of Static Pattern Rules](#Static-Usage), Up: [Static Pattern Rules](#Static-Pattern)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 4.12.2 Static Pattern Rules versus Implicit Rules

A static pattern rule has much in common with an implicit rule defined as a
pattern rule (see [Defining and Redefining Pattern Rules](#Pattern-Rules)).
Both have a pattern for the target and patterns for constructing the
names of prerequisites. The difference is in how `make` decides
*when* the rule applies.

An implicit rule *can* apply to any target that matches its pattern,
but it *does* apply only when the target has no recipe otherwise
specified, and only when the prerequisites can be found. If more than one
implicit rule appears applicable, only one applies; the choice depends on
the order of rules.

By contrast, a static pattern rule applies to the precise list of targets
that you specify in the rule. It cannot apply to any other target and it
invariably does apply to each of the targets specified. If two conflicting
rules apply, and both have recipes, that’s an error.

The static pattern rule can be better than an implicit rule for these
reasons:

* You may wish to override the usual implicit rule for a few
  files whose names cannot be categorized syntactically but
  can be given in an explicit list.
* If you cannot be sure of the precise contents of the directories
  you are using, you may not be sure which other irrelevant files
  might lead `make` to use the wrong implicit rule. The choice
  might depend on the order in which the implicit rule search is done.
  With static pattern rules, there is no uncertainty: each rule applies
  to precisely the targets specified.

---

Next: [Generating Prerequisites Automatically](#Automatic-Prerequisites), Previous: [Static Pattern Rules](#Static-Pattern), Up: [Writing Rules](#Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 4.13 Double-Colon Rules

*Double-colon* rules are explicit rules written with ‘`::`’
instead of ‘`:`’ after the target names. They are handled
differently from ordinary rules when the same target appears in more
than one rule. Pattern rules with double-colons have an entirely
different meaning (see [Match-Anything Pattern Rules](#Match_002dAnything-Rules)).

When a target appears in multiple rules, all the rules must be the same
type: all ordinary, or all double-colon. If they are double-colon, each
of them is independent of the others. Each double-colon rule’s recipe
is executed if the target is older than any prerequisites of that rule.
If there are no prerequisites for that rule, its recipe is always
executed (even if the target already exists). This can result in
executing none, any, or all of the double-colon rules.

Double-colon rules with the same target are in fact completely separate
from one another. Each double-colon rule is processed individually, just
as rules with different targets are processed.

The double-colon rules for a target are executed in the order they appear
in the makefile. However, the cases where double-colon rules really make
sense are those where the order of executing the recipes would not matter.

Double-colon rules are somewhat obscure and not often very useful; they
provide a mechanism for cases in which the method used to update a target
differs depending on which prerequisite files caused the update, and such
cases are rare.

Each double-colon rule should specify a recipe; if it does not, an
implicit rule will be used if one applies.
See [Using Implicit Rules](#Implicit-Rules).

---

Previous: [Double-Colon Rules](#Double_002dColon), Up: [Writing Rules](#Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 4.14 Generating Prerequisites Automatically

In the makefile for a program, many of the rules you need to write often
say only that some object file depends on some header
file. For example, if `main.c` uses `defs.h` via an
`#include`, you would write:

```
main.o: defs.h
```

You need this rule so that `make` knows that it must remake
`main.o` whenever `defs.h` changes. You can see that for a
large program you would have to write dozens of such rules in your
makefile. And, you must always be very careful to update the makefile
every time you add or remove an `#include`.

To avoid this hassle, most modern C compilers can write these rules for
you, by looking at the `#include` lines in the source files.
Usually this is done with the ‘`-M`’ option to the compiler.
For example, the command:

```
cc -M main.c
```

generates the output:

```
main.o : main.c defs.h
```

Thus you no longer have to write all those rules yourself.
The compiler will do it for you.

Note that such a rule constitutes mentioning `main.o` in a
makefile, so it can never be considered an intermediate file by
implicit rule search. This means that `make` won’t ever remove
the file after using it; see [Chains of Implicit
Rules](#Chained-Rules).

With old `make` programs, it was traditional practice to use this
compiler feature to generate prerequisites on demand with a command like
‘`make depend`’. That command would create a file `depend`
containing all the automatically-generated prerequisites; then the
makefile could use `include` to read them in (see [Including Other Makefiles](#Include)).

In GNU `make`, the feature of remaking makefiles makes this
practice obsolete—you need never tell `make` explicitly to
regenerate the prerequisites, because it always regenerates any makefile
that is out of date. See [How Makefiles Are Remade](#Remaking-Makefiles).

The practice we recommend for automatic prerequisite generation is to have
one makefile corresponding to each source file. For each source file
`name.c` there is a makefile `name.d` which lists
what files the object file `name.o` depends on. That way
only the source files that have changed need to be rescanned to produce
the new prerequisites.

Here is the pattern rule to generate a file of prerequisites (i.e., a makefile)
called `name.d` from a C source file called `name.c`:

```
%.d: %.c
        @set -e; rm -f $@; \
         $(CC) -M $(CPPFLAGS) $< > $@.$$$$; \
         sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
         rm -f $@.$$$$
```

See [Defining and Redefining Pattern Rules](#Pattern-Rules), for information on defining pattern rules. The
‘`-e`’ flag to the shell causes it to exit immediately if the
`$(CC)` command (or any other command) fails (exits with a
nonzero status).

With the GNU C compiler, you may wish to use the ‘`-MM`’ flag instead
of ‘`-M`’. This omits prerequisites on system header files.
See [Options Controlling the Preprocessor](https://gcc.gnu.org/onlinedocs/gcc/Preprocessor-Options.html#Preprocessor-Options) in Using GNU CC, for details.

The purpose of the `sed` command is to translate (for example):

```
main.o : main.c defs.h
```

into:

```
main.o main.d : main.c defs.h
```

This makes each ‘`.d`’ file depend on all the source and header files
that the corresponding ‘`.o`’ file depends on. `make` then
knows it must regenerate the prerequisites whenever any of the source or
header files changes.

Once you’ve defined the rule to remake the ‘`.d`’ files,
you then use the `include` directive to read them all in.
See [Including Other Makefiles](#Include). For example:

```
sources = foo.c bar.c

include $(sources:.c=.d)
```

(This example uses a substitution variable reference to translate the
list of source files ‘`foo.c bar.c`’ into a list of prerequisite
makefiles, ‘`foo.d bar.d`’. See [Substitution References](#Substitution-Refs), for full
information on substitution references.) Since the ‘`.d`’ files are
makefiles like any others, `make` will remake them as necessary
with no further work from you. See [How Makefiles Are Remade](#Remaking-Makefiles).

Note that the ‘`.d`’ files contain target definitions; you should
be sure to place the `include` directive *after* the first,
default goal in your makefiles or run the risk of having a random
object file become the default goal.
See [How `make` Processes a Makefile](#How-Make-Works).

---

Next: [How to Use Variables](#Using-Variables), Previous: [Writing Rules](#Rules), Up: [GNU `make`](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]## 5 Writing Recipes in Rules

The recipe of a rule consists of one or more shell command lines to
be executed, one at a time, in the order they appear. Typically, the
result of executing these commands is that the target of the rule is
brought up to date.

Users use many different shell programs, but recipes in makefiles are
always interpreted by `/bin/sh` unless the makefile specifies
otherwise. See [Recipe Execution](#Execution).

* [Recipe Syntax](#Recipe-Syntax)
* [Recipe Echoing](#Echoing)
* [Recipe Execution](#Execution)
* [Parallel Execution](#Parallel)
* [Errors in Recipes](#Errors)
* [Interrupting or Killing `make`](#Interrupts)
* [Recursive Use of `make`](#Recursion)
* [Defining Canned Recipes](#Canned-Recipes)
* [Using Empty Recipes](#Empty-Recipes)

---

Next: [Recipe Echoing](#Echoing), Previous: [Writing Recipes in Rules](#Recipes), Up: [Writing Recipes in Rules](#Recipes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 5.1 Recipe Syntax

Makefiles have the unusual property that there are really two distinct
syntaxes in one file. Most of the makefile uses `make` syntax
(see [Writing Makefiles](#Makefiles)). However, recipes are meant
to be interpreted by the shell and so they are written using shell
syntax. The `make` program does not try to understand shell
syntax: it performs only a very few specific translations on the
content of the recipe before handing it to the shell.

Each line in the recipe must start with a tab (or the first character
in the value of the `.RECIPEPREFIX` variable; see [Other Special Variables](#Special-Variables)), except that the first recipe line may be attached to the
target-and-prerequisites line with a semicolon in between. *Any*
line in the makefile that begins with a tab and appears in a “rule
context” (that is, after a rule has been started until another rule
or variable definition) will be considered part of a recipe for that
rule. Blank lines and lines of just comments may appear among the
recipe lines; they are ignored.

Some consequences of these rules include:

* A blank line that begins with a tab is not blank: it’s an empty
  recipe (see [Using Empty Recipes](#Empty-Recipes)).
* A comment in a recipe is not a `make` comment; it will be
  passed to the shell as-is. Whether the shell treats it as a comment
  or not depends on your shell.
* A variable definition in a “rule context” which is indented by a tab
  as the first character on the line, will be considered part of a
  recipe, not a `make` variable definition, and passed to the
  shell.
* A conditional expression (`ifdef`, `ifeq`,
  etc. see [Syntax of Conditionals](#Conditional-Syntax)) in a “rule
  context” which is indented by a tab as the first character on the
  line, will be considered part of a recipe and be passed to the shell.

* [Splitting Recipe Lines](#Splitting-Recipe-Lines)
* [Using Variables in Recipes](#Variables-in-Recipes)

---

Next: [Using Variables in Recipes](#Variables-in-Recipes), Previous: [Recipe Syntax](#Recipe-Syntax), Up: [Recipe Syntax](#Recipe-Syntax)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 5.1.1 Splitting Recipe Lines

One of the few ways in which `make` does interpret recipes is
checking for a backslash just before the newline. As in normal
makefile syntax, a single logical recipe line can be split into
multiple physical lines in the makefile by placing a backslash before
each newline. A sequence of lines like this is considered a single
recipe line, and one instance of the shell will be invoked to run it.

However, in contrast to how they are treated in other places in a
makefile (see [Splitting Long Lines](#Splitting-Lines)),
backslash/newline pairs are *not* removed from the recipe. Both
the backslash and the newline characters are preserved and passed to
the shell. How the backslash/newline is interpreted depends on your
shell. If the first character of the next line after the
backslash/newline is the recipe prefix character (a tab by default;
see [Other Special Variables](#Special-Variables)), then that character (and only that
character) is removed. Whitespace is never added to the recipe.

For example, the recipe for the all target in this makefile:

```
all :
        @echo no\
space
        @echo no\
        space
        @echo one \
        space
        @echo one\
         space
```

consists of four separate shell commands where the output is:

```
nospace
nospace
one space
one space
```

As a more complex example, this makefile:

```
all : ; @echo 'hello \
        world' ; echo "hello \
    world"
```

will invoke one shell with a command of:

```
echo 'hello \
world' ; echo "hello \
    world"
```

which, according to shell quoting rules, will yield the following output:

```
hello \
world
hello     world
```

Notice how the backslash/newline pair was removed inside the string
quoted with double quotes (`"…"`), but not from the string
quoted with single quotes (`'…'`). This is the way the
default shell (`/bin/sh`) handles backslash/newline pairs. If
you specify a different shell in your makefiles it may treat them
differently.

Sometimes you want to split a long line inside of single quotes, but
you don’t want the backslash/newline to appear in the quoted content.
This is often the case when passing scripts to languages such as Perl,
where extraneous backslashes inside the script can change its meaning
or even be a syntax error. One simple way of handling this is to
place the quoted string, or even the entire command, into a
`make` variable then use the variable in the recipe. In this
situation the newline quoting rules for makefiles will be used, and
the backslash/newline will be removed. If we rewrite our example
above using this method:

```
HELLO = 'hello \
world'

all : ; @echo $(HELLO)
```

we will get output like this:

```
hello world
```

If you like, you can also use target-specific variables
(see [Target-specific Variable Values](#Target_002dspecific)) to obtain
a tighter correspondence between the variable and the recipe that
uses it.

---

Previous: [Splitting Recipe Lines](#Splitting-Recipe-Lines), Up: [Recipe Syntax](#Recipe-Syntax)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 5.1.2 Using Variables in Recipes

The other way in which `make` processes recipes is by expanding
any variable references in them (see [Basics of Variable
References](#Reference)). This occurs after make has finished reading all the
makefiles and the target is determined to be out of date; so, the
recipes for targets which are not rebuilt are never expanded.

Variable and function references in recipes have identical syntax and
semantics to references elsewhere in the makefile. They also have the
same quoting rules: if you want a dollar sign to appear in your
recipe, you must double it (‘`$$`’). For shells like the default
shell, that use dollar signs to introduce variables, it’s important to
keep clear in your mind whether the variable you want to reference is
a `make` variable (use a single dollar sign) or a shell variable
(use two dollar signs). For example:

```
LIST = one two three
all:
        for i in $(LIST); do \
            echo $$i; \
        done
```

results in the following command being passed to the shell:

```
for i in one two three; do \
    echo $i; \
done
```

which generates the expected result:

```
one
two
three
```

---

Next: [Recipe Execution](#Execution), Previous: [Recipe Syntax](#Recipe-Syntax), Up: [Writing Recipes in Rules](#Recipes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 5.2 Recipe Echoing

Normally `make` prints each line of the recipe before it is
executed. We call this *echoing* because it gives the appearance
that you are typing the lines yourself.

When a line starts with ‘`@`’, the echoing of that line is suppressed.
The ‘`@`’ is discarded before the line is passed to the shell.
Typically you would use this for a command whose only effect is to print
something, such as an `echo` command to indicate progress through
the makefile:

```
@echo About to make distribution files
```

When `make` is given the flag ‘`-n`’ or ‘`--just-print`’ it
only echoes most recipes, without executing them. See [Summary of Options](#Options-Summary). In this case even the recipe lines
starting with ‘`@`’ are printed. This flag is useful for finding
out which recipes `make` thinks are necessary without actually
doing them.

The ‘`-s`’ or ‘`--silent`’
flag to `make` prevents all echoing, as if all recipes
started with ‘`@`’. A rule in the makefile for the special target
`.SILENT` without prerequisites has the same effect
(see [Special Built-in Target Names](#Special-Targets)).

---

Next: [Parallel Execution](#Parallel), Previous: [Recipe Echoing](#Echoing), Up: [Writing Recipes in Rules](#Recipes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 5.3 Recipe Execution

When it is time to execute recipes to update a target, they are
executed by invoking a new sub-shell for each line of the recipe,
unless the `.ONESHELL` special target is in effect
(see [Using One Shell](#One-Shell)) (In practice, `make` may
take shortcuts that do not affect the results.)

**Please note:** this implies that setting shell variables and
invoking shell commands such as `cd` that set a context local to
each process will not affect the following lines in the recipe.[3](#FOOT3) If you want to use `cd` to affect the next statement,
put both statements in a single recipe line. Then `make` will
invoke one shell to run the entire line, and the shell will execute
the statements in sequence. For example:

```
foo : bar/lose
        cd $(<D) && gobble $(<F) > ../$@
```

Here we use the shell AND operator (`&&`) so that if the
`cd` command fails, the script will fail without trying to invoke
the `gobble` command in the wrong directory, which could cause
problems (in this case it would certainly cause `../foo` to be
truncated, at least).

* [Using One Shell](#One-Shell)
* [Choosing the Shell](#Choosing-the-Shell)

---

Next: [Choosing the Shell](#Choosing-the-Shell), Previous: [Recipe Execution](#Execution), Up: [Recipe Execution](#Execution)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 5.3.1 Using One Shell

Sometimes you would prefer that all the lines in the recipe be passed
to a single invocation of the shell. There are generally two
situations where this is useful: first, it can improve performance in
makefiles where recipes consist of many command lines, by avoiding
extra processes. Second, you might want newlines to be included in
your recipe command (for example perhaps you are using a very
different interpreter as your `SHELL`). If the `.ONESHELL`
special target appears anywhere in the makefile then *all*
recipe lines for each target will be provided to a single invocation
of the shell. Newlines between recipe lines will be preserved. For
example:

```
.ONESHELL:
foo : bar/lose
        cd $(<D)
        gobble $(<F) > ../$@
```

would now work as expected even though the commands are on different
recipe lines.

If `.ONESHELL` is provided, then only the first line of the
recipe will be checked for the special prefix characters (‘`@`’,
‘`-`’, and ‘`+`’). Subsequent lines will include the special
characters in the recipe line when the `SHELL` is invoked. If
you want your recipe to start with one of these special characters
you’ll need to arrange for them to not be the first characters on the
first line, perhaps by adding a comment or similar. For example, this
would be a syntax error in Perl because the first ‘`@`’ is removed
by make:

```
.ONESHELL:
SHELL = /usr/bin/perl
.SHELLFLAGS = -e
show :
        @f = qw(a b c);
        print "@f\n";
```

However, either of these alternatives would work properly:

```
.ONESHELL:
SHELL = /usr/bin/perl
.SHELLFLAGS = -e
show :
        # Make sure "@" is not the first character on the first line
        @f = qw(a b c);
        print "@f\n";
```

or

```
.ONESHELL:
SHELL = /usr/bin/perl
.SHELLFLAGS = -e
show :
        my @f = qw(a b c);
        print "@f\n";
```

As a special feature, if `SHELL` is determined to be a
POSIX-style shell, the special prefix characters in “internal”
recipe lines will be *removed* before the recipe is processed.
This feature is intended to allow existing makefiles to add the
`.ONESHELL` special target and still run properly without
extensive modifications. Since the special prefix characters are not
legal at the beginning of a line in a POSIX shell script this is not a
loss in functionality. For example, this works as expected:

```
.ONESHELL:
foo : bar/lose
        @cd $(@D)
        @gobble $(@F) > ../$@
```

Even with this special feature, however, makefiles with
`.ONESHELL` will behave differently in ways that could be
noticeable. For example, normally if any line in the recipe fails,
that causes the rule to fail and no more recipe lines are processed.
Under `.ONESHELL` a failure of any but the final recipe line will
not be noticed by `make`. You can modify `.SHELLFLAGS` to
add the `-e` option to the shell which will cause any failure
anywhere in the command line to cause the shell to fail, but this
could itself cause your recipe to behave differently. Ultimately you
may need to harden your recipe lines to allow them to work with
`.ONESHELL`.

---

Previous: [Using One Shell](#One-Shell), Up: [Recipe Execution](#Execution)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 5.3.2 Choosing the Shell

The program used as the shell is taken from the variable `SHELL`.
If this variable is not set in your makefile, the program
`/bin/sh` is used as the shell. The argument(s) passed to the
shell are taken from the variable `.SHELLFLAGS`. The default
value of `.SHELLFLAGS` is `-c` normally, or `-ec` in
POSIX-conforming mode.

Unlike most variables, the variable `SHELL` is never set from the
environment. This is because the `SHELL` environment variable is
used to specify your personal choice of shell program for interactive
use. It would be very bad for personal choices like this to affect the
functioning of makefiles. See [Variables from the
Environment](#Environment).

Furthermore, when you do set `SHELL` in your makefile that value
is *not* exported in the environment to recipe lines that
`make` invokes. Instead, the value inherited from the user’s
environment, if any, is exported. You can override this behavior by
explicitly exporting `SHELL` (see [Communicating Variables to a Sub-`make`](#Variables_002fRecursion)), forcing it to be
passed in the environment to recipe lines.

However, on MS-DOS and MS-Windows the value of `SHELL` in the
environment **is** used, since on those systems most users do not
set this variable, and therefore it is most likely set specifically to
be used by `make`. On MS-DOS, if the setting of `SHELL` is
not suitable for `make`, you can set the variable
`MAKESHELL` to the shell that `make` should use; if set it
will be used as the shell instead of the value of `SHELL`.

#### Choosing a Shell in DOS and Windows

Choosing a shell in MS-DOS and MS-Windows is much more complex than on
other systems.

On MS-DOS, if `SHELL` is not set, the value of the variable
`COMSPEC` (which is always set) is used instead.

The processing of lines that set the variable `SHELL` in Makefiles
is different on MS-DOS. The stock shell, `command.com`, is
ridiculously limited in its functionality and many users of `make`
tend to install a replacement shell. Therefore, on MS-DOS, `make`
examines the value of `SHELL`, and changes its behavior based on
whether it points to a Unix-style or DOS-style shell. This allows
reasonable functionality even if `SHELL` points to
`command.com`.

If `SHELL` points to a Unix-style shell, `make` on MS-DOS
additionally checks whether that shell can indeed be found; if not, it
ignores the line that sets `SHELL`. In MS-DOS, GNU `make`
searches for the shell in the following places:

1. In the precise place pointed to by the value of `SHELL`. For
   example, if the makefile specifies ‘`SHELL = /bin/sh`’, `make`
   will look in the directory `/bin` on the current drive.
2. In the current directory.
3. In each of the directories in the `PATH` variable, in order.

In every directory it examines, `make` will first look for the
specific file (`sh` in the example above). If this is not found,
it will also look in that directory for that file with one of the known
extensions which identify executable files. For example `.exe`,
`.com`, `.bat`, `.btm`, `.sh`, and some others.

If any of these attempts is successful, the value of `SHELL` will
be set to the full pathname of the shell as found. However, if none of
these is found, the value of `SHELL` will not be changed, and thus
the line that sets it will be effectively ignored. This is so
`make` will only support features specific to a Unix-style shell if
such a shell is actually installed on the system where `make` runs.

Note that this extended search for the shell is limited to the cases
where `SHELL` is set from the Makefile; if it is set in the
environment or command line, you are expected to set it to the full
pathname of the shell, exactly as things are on Unix.

The effect of the above DOS-specific processing is that a Makefile that
contains ‘`SHELL = /bin/sh`’ (as many Unix makefiles do), will work
on MS-DOS unaltered if you have e.g. `sh.exe` installed in some
directory along your `PATH`.

---

Next: [Errors in Recipes](#Errors), Previous: [Recipe Execution](#Execution), Up: [Writing Recipes in Rules](#Recipes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]
