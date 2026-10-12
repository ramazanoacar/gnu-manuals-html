### 9.2 Arguments to Specify the Goals

The *goals* are the targets that `make` should strive ultimately
to update. Other targets are updated as well if they appear as
prerequisites of goals, or prerequisites of prerequisites of goals, etc.

By default, the goal is the first target in the makefile (not counting
targets that start with a period). Therefore, makefiles are usually
written so that the first target is for compiling the entire program or
programs they describe. If the first rule in the makefile has several
targets, only the first target in the rule becomes the default goal, not
the whole list. You can manage the selection of the default goal from
within your makefile using the `.DEFAULT_GOAL` variable
(see [Other Special Variables](#Special-Variables)).

You can also specify a different goal or goals with command line
arguments to `make`. Use the name of the goal as an argument.
If you specify several goals, `make` processes each of them in
turn, in the order you name them.

Any target in the makefile may be specified as a goal (unless it
starts with ‘`-`’ or contains an ‘`=`’, in which case it will be
parsed as a switch or variable definition, respectively). Even
targets not in the makefile may be specified, if `make` can find
implicit rules that say how to make them.

`Make` will set the special variable `MAKECMDGOALS` to the
list of goals you specified on the command line. If no goals were given
on the command line, this variable is empty. Note that this variable
should be used only in special circumstances.

An example of appropriate use is to avoid including `.d` files
during `clean` rules (see [Generating Prerequisites Automatically](#Automatic-Prerequisites)), so
`make` won’t create them only to immediately remove them
again:

```
sources = foo.c bar.c

ifeq (,$(filter clean,$(MAKECMDGOALS))
include $(sources:.c=.d)
endif
```

One use of specifying a goal is if you want to compile only a part of
the program, or only one of several programs. Specify as a goal each
file that you wish to remake. For example, consider a directory containing
several programs, with a makefile that starts like this:

```
.PHONY: all
all: size nm ld ar as
```

If you are working on the program `size`, you might want to say
‘`make size`’ so that only the files of that program are recompiled.

Another use of specifying a goal is to make files that are not normally
made. For example, there may be a file of debugging output, or a
version of the program that is compiled specially for testing, which has
a rule in the makefile but is not a prerequisite of the default goal.

Another use of specifying a goal is to run the recipe associated with
a phony target (see [Phony Targets](#Phony-Targets)) or empty target (see [Empty Target Files to Record Events](#Empty-Targets)). Many makefiles contain
a phony target named `clean` which deletes everything except source
files. Naturally, this is done only if you request it explicitly with
‘`make clean`’. Following is a list of typical phony and empty
target names. See [Standard Targets for Users](#Standard-Targets), for a detailed list of all the
standard target names which GNU software packages use.

`all` [¶](#index-all-_0028standard-target_0029)
:   Make all the top-level targets the makefile knows about.

`clean` [¶](#index-clean-_0028standard-target_0029)
:   Delete all files that are normally created by running `make`.

`mostlyclean` [¶](#index-mostlyclean-_0028standard-target_0029)
:   Like ‘`clean`’, but may refrain from deleting a few files that people
    normally don’t want to recompile. For example, the ‘`mostlyclean`’
    target for GCC does not delete `libgcc.a`, because recompiling it
    is rarely necessary and takes a lot of time.

`distclean` [¶](#index-clobber-_0028standard-target_0029)

`realclean`

`clobber`
:   Any of these targets might be defined to delete *more* files than
    ‘`clean`’ does. For example, this would delete configuration files
    or links that you would normally create as preparation for compilation,
    even if the makefile itself cannot create these files.

`install` [¶](#index-install-_0028standard-target_0029)
:   Copy the executable file into a directory that users typically search
    for commands; copy any auxiliary files that the executable uses into
    the directories where it will look for them.

`print` [¶](#index-print-_0028standard-target_0029)
:   Print listings of the source files that have changed.

`tar` [¶](#index-tar-_0028standard-target_0029)
:   Create a tar file of the source files.

`shar` [¶](#index-shar-_0028standard-target_0029)
:   Create a shell archive (shar file) of the source files.

`dist` [¶](#index-dist-_0028standard-target_0029)
:   Create a distribution file of the source files. This might
    be a tar file, or a shar file, or a compressed version of one of the
    above, or even more than one of the above.

`TAGS` [¶](#index-TAGS-_0028standard-target_0029)
:   Update a tags table for this program.

`check` [¶](#index-test-_0028standard-target_0029)

`test`
:   Perform self tests on the program this makefile builds.

---

Next: [Avoiding Recompilation of Some Files](#Avoiding-Compilation), Previous: [Arguments to Specify the Goals](#Goals), Up: [How to Run `make`](#Running)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 9.3 Instead of Executing Recipes

The makefile tells `make` how to tell whether a target is up to date,
and how to update each target. But updating the targets is not always
what you want. Certain options specify other activities for `make`.

‘`-n`’ [¶](#index-_002d_002djust_002dprint-1)

‘`--just-print`’

‘`--dry-run`’

‘`--recon`’
:   “No-op”. Causes `make` to print the recipes that are needed to
    make the targets up to date, but not actually execute them. Note that
    some recipes are still executed, even with this flag (see [How the `MAKE` Variable Works](#MAKE-Variable)). Also any recipes
    needed to update included makefiles are still executed
    (see [How Makefiles Are Remade](#Remaking-Makefiles)).

‘`-t`’ [¶](#index-_002d_002dtouch)

‘`--touch`’
:   “Touch”. Marks targets as up to date without actually changing
    them. In other words, `make` pretends to update the targets but
    does not really change their contents; instead only their modified
    times are updated.

‘`-q`’ [¶](#index-_002d_002dquestion)

‘`--question`’
:   “Question”. Silently check whether the targets are up to date, but
    do not execute recipes; the exit code shows whether any updates are
    needed.

‘`-W file`’ [¶](#index-_002d_002dwhat_002dif)

‘`--what-if=file`’

‘`--assume-new=file`’

‘`--new-file=file`’
:   “What if”. Each ‘`-W`’ flag is followed by a file name. The given
    files’ modification times are recorded by `make` as being the present
    time, although the actual modification times remain the same.
    You can use the ‘`-W`’ flag in conjunction with the ‘`-n`’ flag
    to see what would happen if you were to modify specific files.

With the ‘`-n`’ flag, `make` prints the recipe that it would
normally execute but usually does not execute it.

With the ‘`-t`’ flag, `make` ignores the recipes in the rules
and uses (in effect) the command `touch` for each target that needs to
be remade. The `touch` command is also printed, unless ‘`-s`’ or
`.SILENT` is used. For speed, `make` does not actually invoke
the program `touch`. It does the work directly.

With the ‘`-q`’ flag, `make` prints nothing and executes no
recipes, but the exit status code it returns is zero if and only if the
targets to be considered are already up to date. If the exit status is
one, then some updating needs to be done. If `make` encounters an
error, the exit status is two, so you can distinguish an error from a
target that is not up to date.

It is an error to use more than one of these three flags in the same
invocation of `make`.

The ‘`-n`’, ‘`-t`’, and ‘`-q`’ options do not affect recipe
lines that begin with ‘`+`’ characters or contain the strings
‘`$(MAKE)`’ or ‘`${MAKE}`’. Note that only the line containing
the ‘`+`’ character or the strings ‘`$(MAKE)`’ or ‘`${MAKE}`’
is run regardless of these options. Other lines in the same rule are
not run unless they too begin with ‘`+`’ or contain ‘`$(MAKE)`’ or
‘`${MAKE}`’ (See [How the `MAKE` Variable Works](#MAKE-Variable).)

The ‘`-t`’ flag prevents phony targets (see [Phony Targets](#Phony-Targets)) from
being updated, unless there are recipe lines beginning with ‘`+`’
or containing ‘`$(MAKE)`’ or ‘`${MAKE}`’.

The ‘`-W`’ flag provides two features:

* If you also use the ‘`-n`’ or ‘`-q`’ flag, you can see what
  `make` would do if you were to modify some files.
* Without the ‘`-n`’ or ‘`-q`’ flag, when `make` is actually
  executing recipes, the ‘`-W`’ flag can direct `make` to act as
  if some files had been modified, without actually running the recipes
  for those files.

Note that the options ‘`-p`’ and ‘`-v`’ allow you to obtain other
information about `make` or about the makefiles in use
(see [Summary of Options](#Options-Summary)).

---

Next: [Overriding Variables](#Overriding), Previous: [Instead of Executing Recipes](#Instead-of-Execution), Up: [How to Run `make`](#Running)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 9.4 Avoiding Recompilation of Some Files

Sometimes you may have changed a source file but you do not want to
recompile all the files that depend on it. For example, suppose you add
a macro or a declaration to a header file that many other files depend
on. Being conservative, `make` assumes that any change in the
header file requires recompilation of all dependent files, but you know
that they do not need to be recompiled and you would rather not waste
the time waiting for them to compile.

If you anticipate the problem before changing the header file, you can
use the ‘`-t`’ flag. This flag tells `make` not to run the
recipes in the rules, but rather to mark the target up to date by
changing its last-modification date. You would follow this procedure:

1. Use the command ‘`make`’ to recompile the source files that really
   need recompilation, ensuring that the object files are up-to-date
   before you begin.
2. Make the changes in the header files.
3. Use the command ‘`make -t`’ to mark all the object files as
   up to date. The next time you run `make`, the changes in the
   header files will not cause any recompilation.

If you have already changed the header file at a time when some files
do need recompilation, it is too late to do this. Instead, you can
use the ‘`-o file`’ flag, which marks a specified file as
“old” (see [Summary of Options](#Options-Summary)). This means
that the file itself will not be remade, and nothing else will be
remade on its account. Follow this procedure:

1. Recompile the source files that need compilation for reasons independent
   of the particular header file, with ‘`make -o headerfile`’.
   If several header files are involved, use a separate ‘`-o`’ option
   for each header file.
2. Touch all the object files with ‘`make -t`’.

---

Next: [Testing the Compilation of a Program](#Testing), Previous: [Avoiding Recompilation of Some Files](#Avoiding-Compilation), Up: [How to Run `make`](#Running)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 9.5 Overriding Variables

An argument that contains ‘`=`’ specifies the value of a variable:
‘`v=x`’ sets the value of the variable v to x.
If you specify a value in this way, all ordinary assignments of the same
variable in the makefile are ignored; we say they have been
*overridden* by the command line argument.

The most common way to use this facility is to pass extra flags to
compilers. For example, in a properly written makefile, the variable
`CFLAGS` is included in each recipe that runs the C compiler, so a
file `foo.c` would be compiled something like this:

```
cc -c $(CFLAGS) foo.c
```

Thus, whatever value you set for `CFLAGS` affects each compilation
that occurs. The makefile probably specifies the usual value for
`CFLAGS`, like this:

```
CFLAGS=-g
```

Each time you run `make`, you can override this value if you
wish. For example, if you say ‘`make CFLAGS='-g -O'`’, each C
compilation will be done with ‘`cc -c -g -O`’. (This also
illustrates how you can use quoting in the shell to enclose spaces and
other special characters in the value of a variable when you override
it.)

The variable `CFLAGS` is only one of many standard variables that
exist just so that you can change them this way. See [Variables Used by Implicit Rules](#Implicit-Variables), for a complete list.

You can also program the makefile to look at additional variables of your
own, giving the user the ability to control other aspects of how the
makefile works by changing the variables.

When you override a variable with a command line argument, you can
define either a recursively-expanded variable or a simply-expanded
variable. The examples shown above make a recursively-expanded
variable; to make a simply-expanded variable, write ‘`:=`’ or
‘`::=`’ instead of ‘`=`’. But, unless you want to include a
variable reference or function call in the *value* that you
specify, it makes no difference which kind of variable you create.

There is one way that the makefile can change a variable that you have
overridden. This is to use the `override` directive, which is a line
that looks like this: ‘`override variable = value`’
(see [The `override` Directive](#Override-Directive)).

---

Next: [Temporary Files](#Temporary-Files), Previous: [Overriding Variables](#Overriding), Up: [How to Run `make`](#Running)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 9.6 Testing the Compilation of a Program

Normally, when an error happens in executing a shell command, `make`
gives up immediately, returning a nonzero status. No further recipes are
executed for any target. The error implies that the goal cannot be
correctly remade, and `make` reports this as soon as it knows.

When you are compiling a program that you have just changed, this is not
what you want. Instead, you would rather that `make` try compiling
every file that can be tried, to show you as many compilation errors
as possible.

On these occasions, you should use the ‘`-k`’ or
‘`--keep-going`’ flag. This tells `make` to continue to
consider the other prerequisites of the pending targets, remaking them
if necessary, before it gives up and returns nonzero status. For
example, after an error in compiling one object file, ‘`make -k`’
will continue compiling other object files even though it already
knows that linking them will be impossible. In addition to continuing
after failed shell commands, ‘`make -k`’ will continue as much as
possible after discovering that it does not know how to make a target
or prerequisite file. This will always cause an error message, but
without ‘`-k`’, it is a fatal error (see [Summary of Options](#Options-Summary)).

The usual behavior of `make` assumes that your purpose is to get the
goals up to date; once `make` learns that this is impossible, it might
as well report the failure immediately. The ‘`-k`’ flag says that the
real purpose is to test as much as possible of the changes made in the
program, perhaps to find several independent problems so that you can
correct them all before the next attempt to compile. This is why Emacs’
`M-x compile` command passes the ‘`-k`’ flag by default.

---

Next: [Summary of Options](#Options-Summary), Previous: [Testing the Compilation of a Program](#Testing), Up: [How to Run `make`](#Running)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 9.7 Temporary Files

In some situations, `make` will need to create its own temporary files.
These files must not be disturbed while `make` is running, including all
recursively-invoked instances of `make`.

If the environment variable `MAKE_TMPDIR` is set then all temporary files
created by `make` will be placed there.

If `MAKE_TMPDIR` is not set, then the standard location for temporary
files for the current operating system will be used. For POSIX systems this
will be the location set in the `TMPDIR` environment variable, or else
the system’s default location (e.g., `/tmp`) is used. On Windows,
first `TMP` then `TEMP` will be checked, then `TMPDIR`, and
finally the system default temporary file location will be used.

Note that this directory must already exist or `make` will fail:
`make` will not attempt to create it.

These variables *cannot* be set from within a makefile: GNU `make`
must have access to this location before it begins reading the makefiles.

---

Previous: [Temporary Files](#Temporary-Files), Up: [How to Run `make`](#Running)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 9.8 Summary of Options

Here is a table of all the options `make` understands:

‘`-b`’ [¶](#index-_002dm)

‘`-m`’
:   These options are ignored for compatibility with other versions of `make`.

‘`-B`’ [¶](#index-_002d_002dalways_002dmake)

‘`--always-make`’
:   Consider all targets out-of-date. GNU `make` proceeds to
    consider targets and their prerequisites using the normal algorithms;
    however, all targets so considered are always remade regardless of the
    status of their prerequisites. To avoid infinite recursion, if
    `MAKE_RESTARTS` (see [Other Special
    Variables](#Special-Variables)) is set to a number greater than 0 this option is disabled
    when considering whether to remake makefiles (see [How Makefiles Are Remade](#Remaking-Makefiles)).

‘`-C dir`’ [¶](#index-_002d_002ddirectory-1)

‘`--directory=dir`’
:   Change to directory dir before reading the makefiles. If multiple
    ‘`-C`’ options are specified, each is interpreted relative to the
    previous one: ‘`-C / -C etc`’ is equivalent to ‘`-C /etc`’.
    This is typically used with recursive invocations of `make`
    (see [Recursive Use of `make`](#Recursion)).

‘`-d`’ [¶](#index-_002dd)
:   Print debugging information in addition to normal processing. The
    debugging information says which files are being considered for
    remaking, which file-times are being compared and with what results,
    which files actually need to be remade, which implicit rules are
    considered and which are applied—everything interesting about how
    `make` decides what to do. The `-d` option is equivalent to
    ‘`--debug=a`’ (see below).

‘`--debug[=options]`’ [¶](#index-_002d_002ddebug)
:   Print debugging information in addition to normal processing. Various
    levels and types of output can be chosen. With no arguments, print the
    “basic” level of debugging. Possible arguments are below; only the
    first character is considered, and values must be comma- or
    space-separated.

    `a (all)`
    :   All types of debugging output are enabled. This is equivalent to using
        ‘`-d`’.

    `b (basic)`
    :   Basic debugging prints each target that was found to be out-of-date, and
        whether the build was successful or not.

    `v (verbose)`
    :   A level above ‘`basic`’; includes messages about which makefiles were
        parsed, prerequisites that did not need to be rebuilt, etc. This option
        also enables ‘`basic`’ messages.

    `i (implicit)`
    :   Prints messages describing the implicit rule searches for each target.
        This option also enables ‘`basic`’ messages.

    `j (jobs)`
    :   Prints messages giving details on the invocation of specific sub-commands.

    `m (makefile)`
    :   By default, the above messages are not enabled while trying to remake
        the makefiles. This option enables messages while rebuilding makefiles,
        too. Note that the ‘`all`’ option does enable this option. This
        option also enables ‘`basic`’ messages.

    `p (print)`
    :   Prints the recipe to be executed, even when the recipe is normally
        silent (due to `.SILENT` or ‘`@`’). Also prints the makefile
        name and line number where the recipe was defined.

    `w (why)`
    :   Explains why each target must be remade by showing which prerequisites
        are more up to date than the target.

    `n (none)`
    :   Disable all debugging currently enabled. If additional debugging
        flags are encountered after this they will still take effect.

‘`-e`’ [¶](#index-_002d_002denvironment_002doverrides)

‘`--environment-overrides`’
:   Give variables taken from the environment precedence
    over variables from makefiles.
    See [Variables from the Environment](#Environment).

‘`-E string`’ [¶](#index-_002dE)

‘`--eval=string`’ [¶](#index-_002d_002deval)
:   Evaluate string as makefile syntax. This is a command-line
    version of the `eval` function (see [The `eval` Function](#Eval-Function)). The
    evaluation is performed after the default rules and variables have
    been defined, but before any makefiles are read.

‘`-f file`’ [¶](#index-_002d_002dmakefile-2)

‘`--file=file`’

‘`--makefile=file`’
:   Read the file named file as a makefile.
    See [Writing Makefiles](#Makefiles).

‘`-h`’ [¶](#index-_002d_002dhelp)

‘`--help`’
:   Remind you of the options that `make` understands and then exit.

‘`-i`’ [¶](#index-_002d_002dignore_002derrors-1)

‘`--ignore-errors`’
:   Ignore all errors in recipes executed to remake files.
    See [Errors in Recipes](#Errors).

‘`-I dir`’ [¶](#index-_002d_002dinclude_002ddir-1)

‘`--include-dir=dir`’
:   Specifies a directory dir to search for included makefiles.
    See [Including Other Makefiles](#Include). If several ‘`-I`’
    options are used to specify several directories, the directories are
    searched in the order specified. If the directory dir is a
    single dash (`-`) then any already-specified directories up to
    that point (including the default directory paths) will be discarded.
    You can examine the current list of directories to be searched via the
    `.INCLUDE_DIRS` variable.

‘`-j [jobs]`’ [¶](#index-_002d_002djobs-1)

‘`--jobs[=jobs]`’
:   Specifies the number of recipes (jobs) to run simultaneously. With no
    argument, `make` runs as many recipes simultaneously as possible.
    If there is more than one ‘`-j`’ option, the last one is effective.
    See [Parallel Execution](#Parallel), for more information on how
    recipes are run. Note that this option is ignored on MS-DOS.

‘`--jobserver-style=[style]`’ [¶](#index-_002d_002djobserver_002dstyle)
:   Chooses the style of jobserver to use. This option only has effect if
    parallel builds are enabled (see [Parallel Execution](#Parallel)). On POSIX
    systems style can be one of `fifo` (the default) or `pipe`.
    On Windows the only acceptable style is `sem` (the default). This
    option is useful if you need to use an older versions of GNU `make`, or a
    different tool that requires a specific jobserver style.

‘`-k`’ [¶](#index-_002d_002dkeep_002dgoing-2)

‘`--keep-going`’
:   Continue as much as possible after an error. While the target that
    failed, and those that depend on it, cannot be remade, the other
    prerequisites of these targets can be processed all the same.
    See [Testing the Compilation of a Program](#Testing).

‘`-l [load]`’ [¶](#index-_002d_002dmax_002dload-1)

‘`--load-average[=load]`’

‘`--max-load[=load]`’
:   Specifies that no new recipes should be started if there are other
    recipes running and the load average is at least load (a
    floating-point number). With no argument, removes a previous load
    limit. See [Parallel Execution](#Parallel).

‘`-L`’ [¶](#index-_002d_002dcheck_002dsymlink_002dtimes)

‘`--check-symlink-times`’
:   On systems that support symbolic links, this option causes `make`
    to consider the timestamps on any symbolic links in addition to the
    timestamp on the file referenced by those links. When this option is
    provided, the most recent timestamp among the file and the symbolic
    links is taken as the modification time for this target file.

‘`-n`’ [¶](#index-_002d_002drecon-2)

‘`--just-print`’

‘`--dry-run`’

‘`--recon`’
:   Print the recipe that would be executed, but do not execute it (except
    in certain circumstances).
    See [Instead of Executing Recipes](#Instead-of-Execution).

‘`-o file`’ [¶](#index-_002d_002dassume_002dold-1)

‘`--old-file=file`’

‘`--assume-old=file`’
:   Do not remake the file file even if it is older than its
    prerequisites, and do not remake anything on account of changes in
    file. Essentially the file is treated as very old and its rules
    are ignored. See [Avoiding Recompilation of
    Some Files](#Avoiding-Compilation).

‘`-O[type]`’ [¶](#index-_002d_002doutput_002dsync-1)

‘`--output-sync[=type]`’
:   Ensure that the complete output from each recipe is printed in one
    uninterrupted sequence. This option is only useful when using the
    `--jobs` option to run multiple recipes simultaneously
    (see [Parallel Execution](#Parallel)) Without this option output
    will be displayed as it is generated by the recipes.

    With no type or the type ‘`target`’, output from the entire recipe
    of each target is grouped together. With the type ‘`line`’, output
    from each line in the recipe is grouped together. With the type
    ‘`recurse`’, the output from an entire recursive make is grouped
    together. With the type ‘`none`’, no output synchronization is
    performed. See [Output During Parallel Execution](#Parallel-Output).

‘`-p`’ [¶](#index-_002d_002dprint_002ddata_002dbase)

‘`--print-data-base`’
:   Print the data base (rules and variable values) that results from
    reading the makefiles; then execute as usual or as otherwise
    specified. This also prints the version information given by the
    ‘`-v`’ switch (see below). To print the data base without trying
    to remake any files, use ‘`make -qp`’. To print the data base
    of predefined rules and variables, use ‘`make -p -f /dev/null`’.
    The data base output contains file name and line number information for
    recipe and variable definitions, so it can be a useful debugging tool
    in complex environments.

‘`-q`’ [¶](#index-_002d_002dquestion-1)

‘`--question`’
:   “Question mode”. Do not run any recipes, or print anything; just
    return an exit status that is zero if the specified targets are already
    up to date, one if any remaking is required, or two if an error is
    encountered. See [Instead of Executing
    Recipes](#Instead-of-Execution).

‘`-r`’ [¶](#index-_002d_002dno_002dbuiltin_002drules)

‘`--no-builtin-rules`’
:   Eliminate use of the built-in implicit rules (see [Using Implicit Rules](#Implicit-Rules)). You can still define your own by writing
    pattern rules (see [Defining and Redefining Pattern
    Rules](#Pattern-Rules)). The ‘`-r`’ option also clears out the default list of
    suffixes for suffix rules (see [Old-Fashioned Suffix
    Rules](#Suffix-Rules)). But you can still define your own suffixes with a rule for
    `.SUFFIXES`, and then define your own suffix rules. Note that only
    *rules* are affected by the `-r` option; default variables
    remain in effect (see [Variables Used by Implicit
    Rules](#Implicit-Variables)); see the ‘`-R`’ option below.

‘`-R`’ [¶](#index-_002d_002dno_002dbuiltin_002dvariables)

‘`--no-builtin-variables`’
:   Eliminate use of the built-in rule-specific variables (see [Variables Used by Implicit Rules](#Implicit-Variables)). You can still define
    your own, of course. The ‘`-R`’ option also automatically enables
    the ‘`-r`’ option (see above), since it doesn’t make sense to have
    implicit rules without any definitions for the variables that they use.

‘`-s`’ [¶](#index-_002d_002dquiet-1)

‘`--silent`’

‘`--quiet`’
:   Silent operation; do not print the recipes as they are executed.
    See [Recipe Echoing](#Echoing).

‘`-S`’ [¶](#index-_002d_002dstop)

‘`--no-keep-going`’

‘`--stop`’
:   Cancel the effect of the ‘`-k`’ option. This is never necessary
    except in a recursive `make` where ‘`-k`’ might be inherited
    from the top-level `make` via `MAKEFLAGS`
    (see [Recursive Use of `make`](#Recursion))
    or if you set ‘`-k`’ in `MAKEFLAGS` in your environment.

‘`--shuffle[=mode]`’ [¶](#index-_002d_002dshuffle)
:   This option enables a form of fuzz-testing of prerequisite relationships.
    When parallelism is enabled (‘`-j`’) the order in which targets are
    built becomes less deterministic. If prerequisites are not fully declared
    in the makefile this can lead to intermittent and hard-to-track-down build
    failures.

    The ‘`--shuffle`’ option forces `make` to purposefully reorder goals
    and prerequisites so target/prerequisite relationships still hold, but
    ordering of prerequisites of a given target are reordered as described below.

    The order in which prerequisites are listed in automatic variables is not
    changed by this option.

    The `.NOTPARALLEL` pseudo-target disables shuffling for that makefile.
    Also any prerequisite list which contains `.WAIT` will not be shuffled.
    See [Disabling Parallel Execution](#Parallel-Disable).

    The ‘`--shuffle=`’ option accepts these values:

    `random`
    :   Choose a random seed for the shuffle. This is the default if no mode is
        specified. The chosen seed is also provided to sub-`make` commands. The
        seed is included in error messages so that it can be re-used in future runs to
        reproduce the problem or verify that it has been resolved.

    `reverse`
    :   Reverse the order of goals and prerequisites, rather than a random shuffle.

    `seed`
    :   Use ‘`random`’ shuffle initialized with the specified seed value. The
        seed is an integer.

    `none`
    :   Disable shuffling. This negates any previous ‘`--shuffle`’ options.

‘`-t`’ [¶](#index-_002d_002dtouch-1)

‘`--touch`’
:   Touch files (mark them up to date without really changing them)
    instead of running their recipes. This is used to pretend that the
    recipes were done, in order to fool future invocations of
    `make`. See [Instead of Executing Recipes](#Instead-of-Execution).

‘`--trace`’ [¶](#index-_002d_002dtrace)
:   Show tracing information for `make` execution. Using `--trace` is
    shorthand for `--debug=print,why`.

‘`-v`’ [¶](#index-_002d_002dversion)

‘`--version`’
:   Print the version of the `make` program plus a copyright, a list
    of authors, and a notice that there is no warranty; then exit.

‘`-w`’ [¶](#index-_002d_002dprint_002ddirectory)

‘`--print-directory`’
:   Print a message containing the working directory both before and after
    executing the makefile. This may be useful for tracking down errors
    from complicated nests of recursive `make` commands.
    See [Recursive Use of `make`](#Recursion). (In practice, you
    rarely need to specify this option since ‘`make`’ does it for you;
    see [The ‘`--print-directory`’ Option](#g_t_002dw-Option).)

‘`--no-print-directory`’ [¶](#index-_002d_002dno_002dprint_002ddirectory-1)
:   Disable printing of the working directory under `-w`.
    This option is useful when `-w` is turned on automatically,
    but you do not want to see the extra messages.
    See [The ‘`--print-directory`’ Option](#g_t_002dw-Option).

‘`-W file`’ [¶](#index-_002d_002dassume_002dnew-1)

‘`--what-if=file`’

‘`--new-file=file`’

‘`--assume-new=file`’
:   Pretend that the target file has just been modified. When used
    with the ‘`-n`’ flag, this shows you what would happen if you were
    to modify that file. Without ‘`-n`’, it is almost the same as
    running a `touch` command on the given file before running
    `make`, except that the modification time is changed only in the
    imagination of `make`.
    See [Instead of Executing Recipes](#Instead-of-Execution).

‘`--warn-undefined-variables`’ [¶](#index-_002d_002dwarn_002dundefined_002dvariables)
:   Issue a warning message whenever `make` sees a reference to an
    undefined variable. This can be helpful when you are trying to debug
    makefiles which use variables in complex ways.

---

Next: [Using `make` to Update Archive Files](#Archives), Previous: [How to Run `make`](#Running), Up: [GNU `make`](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]## 10 Using Implicit Rules

Certain standard ways of remaking target files are used very often. For
example, one customary way to make an object file is from a C source file
using the C compiler, `cc`.

*Implicit rules* tell `make` how to use customary techniques so
that you do not have to specify them in detail when you want to use
them. For example, there is an implicit rule for C compilation. File
names determine which implicit rules are run. For example, C
compilation typically takes a `.c` file and makes a `.o` file.
So `make` applies the implicit rule for C compilation when it sees
this combination of file name endings.

A chain of implicit rules can apply in sequence; for example, `make`
will remake a `.o` file from a `.y` file by way of a `.c` file.

The built-in implicit rules use several variables in their recipes so
that, by changing the values of the variables, you can change the way the
implicit rule works. For example, the variable `CFLAGS` controls the
flags given to the C compiler by the implicit rule for C compilation.

You can define your own implicit rules by writing *pattern rules*.

*Suffix rules* are a more limited way to define implicit rules.
Pattern rules are more general and clearer, but suffix rules are
retained for compatibility.

* [Using Implicit Rules](#Using-Implicit)
* [Catalogue of Built-In Rules](#Catalogue-of-Rules)
* [Variables Used by Implicit Rules](#Implicit-Variables)
* [Chains of Implicit Rules](#Chained-Rules)
* [Defining and Redefining Pattern Rules](#Pattern-Rules)
* [Defining Last-Resort Default Rules](#Last-Resort)
* [Old-Fashioned Suffix Rules](#Suffix-Rules)
* [Implicit Rule Search Algorithm](#Implicit-Rule-Search)

---

Next: [Catalogue of Built-In Rules](#Catalogue-of-Rules), Previous: [Using Implicit Rules](#Implicit-Rules), Up: [Using Implicit Rules](#Implicit-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 10.1 Using Implicit Rules

To allow `make` to find a customary method for updating a target
file, all you have to do is refrain from specifying recipes yourself.
Either write a rule with no recipe, or don’t write a rule at all.
Then `make` will figure out which implicit rule to use based on
which kind of source file exists or can be made.

For example, suppose the makefile looks like this:

```
foo : foo.o bar.o
        cc -o foo foo.o bar.o $(CFLAGS) $(LDFLAGS)
```

Because you mention `foo.o` but do not give a rule for it, `make`
will automatically look for an implicit rule that tells how to update it.
This happens whether or not the file `foo.o` currently exists.

If an implicit rule is found, it can supply both a recipe and one or
more prerequisites (the source files). You would want to write a rule
for `foo.o` with no recipe if you need to specify additional
prerequisites, such as header files, that the implicit rule cannot
supply.

Each implicit rule has a target pattern and prerequisite patterns. There may
be many implicit rules with the same target pattern. For example, numerous
rules make ‘`.o`’ files: one, from a ‘`.c`’ file with the C compiler;
another, from a ‘`.p`’ file with the Pascal compiler; and so on. The rule
that actually applies is the one whose prerequisites exist or can be made.
So, if you have a file `foo.c`, `make` will run the C compiler;
otherwise, if you have a file `foo.p`, `make` will run the Pascal
compiler; and so on.

Of course, when you write the makefile, you know which implicit rule you
want `make` to use, and you know it will choose that one because you
know which possible prerequisite files are supposed to exist.
See [Catalogue of Built-In Rules](#Catalogue-of-Rules),
for a catalogue of all the predefined implicit rules.

Above, we said an implicit rule applies if the required prerequisites “exist
or can be made”. A file “can be made” if it is mentioned explicitly in
the makefile as a target or a prerequisite, or if an implicit rule can be
recursively found for how to make it. When an implicit prerequisite is the
result of another implicit rule, we say that *chaining* is occurring.
See [Chains of Implicit Rules](#Chained-Rules).

In general, `make` searches for an implicit rule for each target, and
for each double-colon rule, that has no recipe. A file that is mentioned
only as a prerequisite is considered a target whose rule specifies nothing,
so implicit rule search happens for it. See [Implicit Rule Search Algorithm](#Implicit-Rule-Search), for the
details of how the search is done.

Note that explicit prerequisites do not influence implicit rule search.
For example, consider this explicit rule:

```
foo.o: foo.p
```

The prerequisite on `foo.p` does not necessarily mean that
`make` will remake `foo.o` according to the implicit rule to
make an object file, a `.o` file, from a Pascal source file, a
`.p` file. For example, if `foo.c` also exists, the implicit
rule to make an object file from a C source file is used instead,
because it appears before the Pascal rule in the list of predefined
implicit rules (see [Catalogue of Built-In
Rules](#Catalogue-of-Rules)).

If you do not want an implicit rule to be used for a target that has no
recipe, you can give that target an empty recipe by writing a semicolon
(see [Defining Empty Recipes](#Empty-Recipes)).

---

Next: [Variables Used by Implicit Rules](#Implicit-Variables), Previous: [Using Implicit Rules](#Using-Implicit), Up: [Using Implicit Rules](#Implicit-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 10.2 Catalogue of Built-In Rules

Here is a catalogue of predefined implicit rules which are always
available unless the makefile explicitly overrides or cancels them.
See [Canceling Implicit Rules](#Canceling-Rules), for information on
canceling or overriding an implicit rule. The ‘`-r`’ or
‘`--no-builtin-rules`’ option cancels all predefined rules.

This manual only documents the default rules available on POSIX-based
operating systems. Other operating systems, such as VMS, Windows,
OS/2, etc. may have different sets of default rules. To see the full
list of default rules and variables available in your version of GNU
`make`, run ‘`make -p`’ in a directory with no makefile.

Not all of these rules will always be defined, even when the ‘`-r`’
option is not given. Many of the predefined implicit rules are
implemented in `make` as suffix rules, so which ones will be
defined depends on the *suffix list* (the list of prerequisites of
the special target `.SUFFIXES`). The default suffix list is:
`.out`, `.a`, `.ln`, `.o`, `.c`, `.cc`,
`.C`, `.cpp`, `.p`, `.f`, `.F`, `.m`,
`.r`, `.y`, `.l`, `.ym`, `.lm`, `.s`,
`.S`, `.mod`, `.sym`, `.def`, `.h`,
`.info`, `.dvi`, `.tex`, `.texinfo`, `.texi`,
`.txinfo`, `.w`, `.ch` `.web`, `.sh`,
`.elc`, `.el`. All of the implicit rules described below
whose prerequisites have one of these suffixes are actually suffix
rules. If you modify the suffix list, the only predefined suffix
rules in effect will be those named by one or two of the suffixes that
are on the list you specify; rules whose suffixes fail to be on the
list are disabled. See [Old-Fashioned Suffix Rules](#Suffix-Rules),
for full details on suffix rules.

Compiling C programs [¶](#index-C_002c-rule-to-compile)
:   `n.o` is made automatically from `n.c` with
    a recipe of the form ‘`$(CC) $(CPPFLAGS) $(CFLAGS) -c`’.

Compiling C++ programs [¶](#index-C_002b_002b_002c-rule-to-compile)
:   `n.o` is made automatically from `n.cc`,
    `n.cpp`, or `n.C` with a recipe of the form
    ‘`$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c`’. We encourage you to use the
    suffix ‘`.cc`’ or ‘`.cpp`’ for C++ source files instead of ‘`.C`’ to
    better support case-insensitive file systems.

Compiling Pascal programs [¶](#index-Pascal_002c-rule-to-compile)
:   `n.o` is made automatically from `n.p`
    with the recipe ‘`$(PC) $(PFLAGS) -c`’.

Compiling Fortran and Ratfor programs [¶](#index-Fortran_002c-rule-to-compile)
:   `n.o` is made automatically from `n.r`,
    `n.F` or `n.f` by running the
    Fortran compiler. The precise recipe used is as follows:

    ‘`.f`’
    :   ‘`$(FC) $(FFLAGS) -c`’.

    ‘`.F`’
    :   ‘`$(FC) $(FFLAGS) $(CPPFLAGS) -c`’.

    ‘`.r`’
    :   ‘`$(FC) $(FFLAGS) $(RFLAGS) -c`’.

Preprocessing Fortran and Ratfor programs
:   `n.f` is made automatically from `n.r` or
    `n.F`. This rule runs just the preprocessor to convert a
    Ratfor or preprocessable Fortran program into a strict Fortran
    program. The precise recipe used is as follows:

    ‘`.F`’
    :   ‘`$(FC) $(CPPFLAGS) $(FFLAGS) -F`’.

    ‘`.r`’
    :   ‘`$(FC) $(FFLAGS) $(RFLAGS) -F`’.

Compiling Modula-2 programs [¶](#index-Modula_002d2_002c-rule-to-compile)
:   `n.sym` is made from `n.def` with a recipe of the form
    ‘`$(M2C) $(M2FLAGS) $(DEFFLAGS)`’. `n.o` is made from
    `n.mod`; the form is: ‘`$(M2C) $(M2FLAGS) $(MODFLAGS)`’.

Assembling and preprocessing assembler programs [¶](#index-assembly_002c-rule-to-compile)
:   `n.o` is made automatically from `n.s` by
    running the assembler, `as`. The precise recipe is
    ‘`$(AS) $(ASFLAGS)`’.

    `n.s` is made automatically from `n.S` by
    running the C preprocessor, `cpp`. The precise recipe is
    ‘`$(CPP) $(CPPFLAGS)`’.

Linking a single object file [¶](#index-linking_002c-predefined-rule-for)
:   `n` is made automatically from `n.o` by running the C
    compiler to link the program. The precise recipe used is ‘`$(CC) $(LDFLAGS) n.o $(LOADLIBES) $(LDLIBS)`’.

    This rule does the right thing for a simple program with only one
    source file. It will also do the right thing if there are multiple
    object files (presumably coming from various other source files), one
    of which has a name matching that of the executable file. Thus,

    ```
    x: y.o z.o
    ```

    when `x.c`, `y.c` and `z.c` all exist will execute:

    ```
    cc -c x.c -o x.o
    cc -c y.c -o y.o
    cc -c z.c -o z.o
    cc x.o y.o z.o -o x
    rm -f x.o
    rm -f y.o
    rm -f z.o
    ```

    In more complicated cases, such as when there is no object file whose
    name derives from the executable file name, you must write an explicit
    recipe for linking.

    Each kind of file automatically made into ‘`.o`’ object files will
    be automatically linked by using the compiler (‘`$(CC)`’,
    ‘`$(FC)`’ or ‘`$(PC)`’; the C compiler ‘`$(CC)`’ is used to
    assemble ‘`.s`’ files) without the ‘`-c`’ option. This could be
    done by using the ‘`.o`’ object files as intermediates, but it is
    faster to do the compiling and linking in one step, so that’s how it’s
    done.

Yacc for C programs [¶](#index-yacc-1)
:   `n.c` is made automatically from `n.y` by
    running Yacc with the recipe ‘`$(YACC) $(YFLAGS)`’.

Lex for C programs [¶](#index-lex)
:   `n.c` is made automatically from `n.l` by
    running Lex. The actual recipe is ‘`$(LEX) $(LFLAGS)`’.

Lex for Ratfor programs
:   `n.r` is made automatically from `n.l` by
    running Lex. The actual recipe is ‘`$(LEX) $(LFLAGS)`’.

    The convention of using the same suffix ‘`.l`’ for all Lex files
    regardless of whether they produce C code or Ratfor code makes it
    impossible for `make` to determine automatically which of the two
    languages you are using in any particular case. If `make` is
    called upon to remake an object file from a ‘`.l`’ file, it must
    guess which compiler to use. It will guess the C compiler, because
    that is more common. If you are using Ratfor, make sure `make`
    knows this by mentioning `n.r` in the makefile. Or, if you
    are using Ratfor exclusively, with no C files, remove ‘`.c`’ from
    the list of implicit rule suffixes with:

    ```
    .SUFFIXES:
    .SUFFIXES: .o .r .f .l …
    ```

Making Lint Libraries from C, Yacc, or Lex programs [¶](#index-lint)
:   `n.ln` is made from `n.c` by running `lint`.
    The precise recipe is ‘`$(LINT) $(LINTFLAGS) $(CPPFLAGS) -i`’.
    The same recipe is used on the C code produced from
    `n.y` or `n.l`.

TeX and Web [¶](#index-TeX_002c-rule-to-run)
:   `n.dvi` is made from `n.tex` with the recipe
    ‘`$(TEX)`’. `n.tex` is made from `n.web` with
    ‘`$(WEAVE)`’, or from `n.w` (and from `n.ch` if
    it exists or can be made) with ‘`$(CWEAVE)`’. `n.p` is
    made from `n.web` with ‘`$(TANGLE)`’ and `n.c`
    is made from `n.w` (and from `n.ch` if it exists
    or can be made) with ‘`$(CTANGLE)`’.

Texinfo and Info [¶](#index-Texinfo_002c-rule-to-format)
:   `n.dvi` is made from `n.texinfo`,
    `n.texi`, or `n.txinfo`, with the recipe
    ‘`$(TEXI2DVI) $(TEXI2DVI_FLAGS)`’. `n.info` is made from
    `n.texinfo`, `n.texi`, or `n.txinfo`, with
    the recipe ‘`$(MAKEINFO) $(MAKEINFO_FLAGS)`’.

RCS [¶](#index-RCS_002c-rule-to-extract-from)
:   Any file `n` is extracted if necessary from an RCS file
    named either `n,v` or `RCS/n,v`. The precise
    recipe used is ‘`$(CO) $(COFLAGS)`’. `n` will not be
    extracted from RCS if it already exists, even if the RCS file is
    newer. The rules for RCS are terminal
    (see [Match-Anything Pattern Rules](#Match_002dAnything-Rules)),
    so RCS files cannot be generated from another source; they must
    actually exist.

SCCS [¶](#index-SCCS_002c-rule-to-extract-from)
:   Any file `n` is extracted if necessary from an SCCS file
    named either `s.n` or `SCCS/s.n`. The precise
    recipe used is ‘`$(GET) $(GFLAGS)`’. The rules for SCCS are
    terminal (see [Match-Anything Pattern Rules](#Match_002dAnything-Rules)),
    so SCCS files cannot be generated from another source; they must
    actually exist.

    For the benefit of SCCS, a file `n` is copied from
    `n.sh` and made executable (by everyone). This is for
    shell scripts that are checked into SCCS. Since RCS preserves the
    execution permission of a file, you do not need to use this feature
    with RCS.

    We recommend that you avoid using of SCCS. RCS is widely held to be
    superior, and is also free. By choosing free software in place of
    comparable (or inferior) proprietary software, you support the free
    software movement.

Usually, you want to change only the variables listed in the table
above, which are documented in the following section.

However, the recipes in built-in implicit rules actually use
variables such as `COMPILE.c`, `LINK.p`, and
`PREPROCESS.S`, whose values contain the recipes listed above.

`make` follows the convention that the rule to compile a
`.x` source file uses the variable `COMPILE.x`.
Similarly, the rule to produce an executable from a `.x`
file uses `LINK.x`; and the rule to preprocess a
`.x` file uses `PREPROCESS.x`.

Every rule that produces an object file uses the variable
`OUTPUT_OPTION`. `make` defines this variable either to
contain ‘`-o $@`’, or to be empty, depending on a compile-time
option. You need the ‘`-o`’ option to ensure that the output goes
into the right file when the source file is in a different directory,
as when using `VPATH` (see [Searching Directories for Prerequisites](#Directory-Search)). However,
compilers on some systems do not accept a ‘`-o`’ switch for object
files. If you use such a system, and use `VPATH`, some
compilations will put their output in the wrong place.
A possible workaround for this problem is to give `OUTPUT_OPTION`
the value ‘`; mv $*.o $@`’.

---

Next: [Chains of Implicit Rules](#Chained-Rules), Previous: [Catalogue of Built-In Rules](#Catalogue-of-Rules), Up: [Using Implicit Rules](#Implicit-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 10.3 Variables Used by Implicit Rules

The recipes in built-in implicit rules make liberal use of certain
predefined variables. You can alter the values of these variables in
the makefile, with arguments to `make`, or in the environment to
alter how the implicit rules work without redefining the rules
themselves. You can cancel all variables used by implicit rules with
the ‘`-R`’ or ‘`--no-builtin-variables`’ option.

For example, the recipe used to compile a C source file actually says
‘`$(CC) -c $(CFLAGS) $(CPPFLAGS)`’. The default values of the variables
used are ‘`cc`’ and nothing, resulting in the command ‘`cc -c`’. By
redefining ‘`CC`’ to ‘`ncc`’, you could cause ‘`ncc`’ to be
used for all C compilations performed by the implicit rule. By redefining
‘`CFLAGS`’ to be ‘`-g`’, you could pass the ‘`-g`’ option to
each compilation. *All* implicit rules that do C compilation use
‘`$(CC)`’ to get the program name for the compiler and *all*
include ‘`$(CFLAGS)`’ among the arguments given to the compiler.

The variables used in implicit rules fall into two classes: those that are
names of programs (like `CC`) and those that contain arguments for the
programs (like `CFLAGS`). (The “name of a program” may also contain
some command arguments, but it must start with an actual executable program
name.) If a variable value contains more than one argument, separate them
with spaces.

The following tables describe of some of the more commonly-used predefined
variables. This list is not exhaustive, and the default values shown here may
not be what `make` selects for your environment. To see the
complete list of predefined variables for your instance of GNU `make` you
can run ‘`make -p`’ in a directory with no makefiles.

Here is a table of some of the more common variables used as names of
programs in built-in rules:

`AR` [¶](#index-AR)
:   Archive-maintaining program; default ‘`ar`’.

`AS` [¶](#index-AS)
:   Program for compiling assembly files; default ‘`as`’.

`CC` [¶](#index-CC)
:   Program for compiling C programs; default ‘`cc`’.

`CXX` [¶](#index-CXX)
:   Program for compiling C++ programs; default ‘`g++`’.

`CPP` [¶](#index-CPP)
:   Program for running the C preprocessor, with results to standard output;
    default ‘`$(CC) -E`’.

`FC` [¶](#index-FC)
:   Program for compiling or preprocessing Fortran and Ratfor programs;
    default ‘`f77`’.

`M2C` [¶](#index-M2C)
:   Program to use to compile Modula-2 source code; default ‘`m2c`’.

`PC` [¶](#index-PC)
:   Program for compiling Pascal programs; default ‘`pc`’.

`CO` [¶](#index-CO)
:   Program for extracting a file from RCS; default ‘`co`’.

`GET` [¶](#index-GET)
:   Program for extracting a file from SCCS; default ‘`get`’.

`LEX` [¶](#index-LEX)
:   Program to use to turn Lex grammars into source code; default ‘`lex`’.

`YACC` [¶](#index-YACC)
:   Program to use to turn Yacc grammars into source code; default ‘`yacc`’.

`LINT` [¶](#index-LINT)
:   Program to use to run lint on source code; default ‘`lint`’.

`MAKEINFO` [¶](#index-MAKEINFO)
:   Program to convert a Texinfo source file into an Info file; default
    ‘`makeinfo`’.

`TEX` [¶](#index-TEX)
:   Program to make TeX DVI files from TeX source;
    default ‘`tex`’.

`TEXI2DVI` [¶](#index-TEXI2DVI)
:   Program to make TeX DVI files from Texinfo source;
    default ‘`texi2dvi`’.

`WEAVE` [¶](#index-WEAVE)
:   Program to translate Web into TeX; default ‘`weave`’.

`CWEAVE` [¶](#index-CWEAVE)
:   Program to translate C Web into TeX; default ‘`cweave`’.

`TANGLE` [¶](#index-TANGLE)
:   Program to translate Web into Pascal; default ‘`tangle`’.

`CTANGLE` [¶](#index-CTANGLE)
:   Program to translate C Web into C; default ‘`ctangle`’.

`RM` [¶](#index-RM)
:   Command to remove a file; default ‘`rm -f`’.

Here is a table of variables whose values are additional arguments for the
programs above. The default values for all of these is the empty
string, unless otherwise noted.

`ARFLAGS` [¶](#index-ARFLAGS)
:   Flags to give the archive-maintaining program; default ‘`rv`’.

`ASFLAGS` [¶](#index-ASFLAGS)
:   Extra flags to give to the assembler (when explicitly
    invoked on a ‘`.s`’ or ‘`.S`’ file).

`CFLAGS` [¶](#index-CFLAGS)
:   Extra flags to give to the C compiler.

`CXXFLAGS` [¶](#index-CXXFLAGS)
:   Extra flags to give to the C++ compiler.

`COFLAGS` [¶](#index-COFLAGS)
:   Extra flags to give to the RCS `co` program.

`CPPFLAGS` [¶](#index-CPPFLAGS)
:   Extra flags to give to the C preprocessor and programs
    that use it (the C and Fortran compilers).

`FFLAGS` [¶](#index-FFLAGS)
:   Extra flags to give to the Fortran compiler.

`GFLAGS` [¶](#index-GFLAGS)
:   Extra flags to give to the SCCS `get` program.

`LDFLAGS` [¶](#index-LDFLAGS)
:   Extra flags to give to compilers when they are supposed to invoke the linker,
    ‘`ld`’, such as `-L`. Libraries (`-lfoo`) should be
    added to the `LDLIBS` variable instead.

`LDLIBS` [¶](#index-LDLIBS)
:   Library flags or names given to compilers when they are supposed to
    invoke the linker, ‘`ld`’. `LOADLIBES` is a deprecated (but
    still supported) alternative to `LDLIBS`. Non-library linker
    flags, such as `-L`, should go in the `LDFLAGS` variable.

`LFLAGS` [¶](#index-LFLAGS)
:   Extra flags to give to Lex.

`YFLAGS` [¶](#index-YFLAGS)
:   Extra flags to give to Yacc.

`PFLAGS` [¶](#index-PFLAGS)
:   Extra flags to give to the Pascal compiler.

`RFLAGS` [¶](#index-RFLAGS)
:   Extra flags to give to the Fortran compiler for Ratfor programs.

`LINTFLAGS` [¶](#index-LINTFLAGS)
:   Extra flags to give to lint.

---

Next: [Defining and Redefining Pattern Rules](#Pattern-Rules), Previous: [Variables Used by Implicit Rules](#Implicit-Variables), Up: [Using Implicit Rules](#Implicit-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 10.4 Chains of Implicit Rules

Sometimes a file can be made by a sequence of implicit rules. For example,
a file `n.o` could be made from `n.y` by running
first Yacc and then `cc`. Such a sequence is called a *chain*.

If the file `n.c` exists, or is mentioned in the makefile, no
special searching is required: `make` finds that the object file can
be made by C compilation from `n.c`; later on, when considering
how to make `n.c`, the rule for running Yacc is
used. Ultimately both `n.c` and `n.o` are
updated.

However, even if `n.c` does not exist and is not mentioned,
`make` knows how to envision it as the missing link between
`n.o` and `n.y`! In this case, `n.c` is
called an *intermediate file*. Once `make` has decided to use the
intermediate file, it is entered in the data base as if it had been
mentioned in the makefile, along with the implicit rule that says how to
create it.

Intermediate files are remade using their rules just like all other
files. But intermediate files are treated differently in two ways.

The first difference is what happens if the intermediate file does not
exist. If an ordinary file b does not exist, and `make`
considers a target that depends on b, it invariably creates
b and then updates the target from b. But if b is
an intermediate file, then `make` can leave well enough alone:
it won’t create b unless one of its prerequisites is out of
date. This means the target depending on b won’t be rebuilt
either, unless there is some other reason to update that target: for
example the target doesn’t exist or a different prerequisite is newer
than the target.

The second difference is that if `make` *does* create b in
order to update something else, it deletes b later on after it is no
longer needed. Therefore, an intermediate file which did not exist before
`make` also does not exist after `make`. `make` reports the
deletion to you by printing a ‘`rm`’ command showing which file it is
deleting.

You can explicitly mark a file as intermediate by listing it as a prerequisite
of the special target `.INTERMEDIATE`. This takes effect even if the
file is mentioned explicitly in some other way.

A file cannot be intermediate if it is mentioned in the makefile as a target
or prerequisite, so one way to avoid the deletion of intermediate files is by
adding it as a prerequisite to some target. However, doing so can cause make
to do extra work when searching pattern rules (see [Implicit Rule Search Algorithm](#Implicit-Rule-Search)).

As an alternative, listing a file as a prerequisite of the special target
`.NOTINTERMEDIATE` forces it to not be considered intermediate (just as
any other mention of the file will do). Also, listing the target pattern of a
pattern rule as a prerequisite of `.NOTINTERMEDIATE` ensures that no
targets generated using that pattern rule are considered intermediate.

You can disable intermediate files completely in your makefile by
providing `.NOTINTERMEDIATE` as a target with no prerequisites:
in that case it applies to every file in the makefile.

If you do not want `make` to create a file merely because it does
not already exist, but you also do not want `make` to
automatically delete the file, you can mark it as a *secondary*
file. To do this, list it as a prerequisite of the special target
`.SECONDARY`. Marking a file as secondary also marks it as
intermediate.

A chain can involve more than two implicit rules. For example, it is
possible to make a file `foo` from `RCS/foo.y,v` by running RCS,
Yacc and `cc`. Then both `foo.y` and `foo.c` are
intermediate files that are deleted at the end.

No single implicit rule can appear more than once in a chain. This means
that `make` will not even consider such a ridiculous thing as making
`foo` from `foo.o.o` by running the linker twice. This
constraint has the added benefit of preventing any infinite loop in the
search for an implicit rule chain.

There are some special implicit rules to optimize certain cases that would
otherwise be handled by rule chains. For example, making `foo` from
`foo.c` could be handled by compiling and linking with separate
chained rules, using `foo.o` as an intermediate file. But what
actually happens is that a special rule for this case does the compilation
and linking with a single `cc` command. The optimized rule is used in
preference to the step-by-step chain because it comes earlier in the
ordering of rules.

Finally, for performance reasons `make` will not consider non-terminal
match-anything rules (i.e., ‘`%:`’) when searching for a rule to
build a prerequisite of an implicit rule (see [Match-Anything Pattern Rules](#Match_002dAnything-Rules)).

---

Next: [Defining Last-Resort Default Rules](#Last-Resort), Previous: [Chains of Implicit Rules](#Chained-Rules), Up: [Using Implicit Rules](#Implicit-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]
