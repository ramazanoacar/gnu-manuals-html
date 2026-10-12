### 5.4 Parallel Execution

GNU `make` knows how to execute several recipes at once. Normally,
`make` will execute only one recipe at a time, waiting for it to finish
before executing the next. However, the ‘`-j`’ or ‘`--jobs`’ option
tells `make` to execute many recipes simultaneously. You can inhibit
parallelism for some or all targets from within the makefile (see [Disabling Parallel Execution](#Parallel-Disable)).

On MS-DOS, the ‘`-j`’ option has no effect, since that system doesn’t
support multi-processing.

If the ‘`-j`’ option is followed by an integer, this is the number of
recipes to execute at once; this is called the number of *job slots*.
If there is nothing looking like an integer after the ‘`-j`’ option,
there is no limit on the number of job slots. The default number of job
slots is one, which means serial execution (one thing at a time).

Handling recursive `make` invocations raises issues for parallel
execution. For more information on this, see [Communicating Options to a Sub-`make`](#Options_002fRecursion).

If a recipe fails (is killed by a signal or exits with a nonzero
status), and errors are not ignored for that recipe (see [Errors in Recipes](#Errors)), the remaining recipe lines to remake the same
target will not be run. If a recipe fails and the ‘`-k`’ or
‘`--keep-going`’ option was not given (see [Summary of Options](#Options-Summary)), `make` aborts execution. If make
terminates for any reason (including a signal) with child processes
running, it waits for them to finish before actually exiting.

When the system is heavily loaded, you will probably want to run fewer jobs
than when it is lightly loaded. You can use the ‘`-l`’ option to tell
`make` to limit the number of jobs to run at once, based on the load
average. The ‘`-l`’ or ‘`--max-load`’
option is followed by a floating-point number. For
example,

```
-l 2.5
```

will not let `make` start more than one job if the load average is
above 2.5. The ‘`-l`’ option with no following number removes the
load limit, if one was given with a previous ‘`-l`’ option.

More precisely, when `make` goes to start up a job, and it already has
at least one job running, it checks the current load average; if it is not
lower than the limit given with ‘`-l`’, `make` waits until the load
average goes below that limit, or until all the other jobs finish.

By default, there is no load limit.

* [Disabling Parallel Execution](#Parallel-Disable)
* [Output During Parallel Execution](#Parallel-Output)
* [Input During Parallel Execution](#Parallel-Input)

---

Next: [Output During Parallel Execution](#Parallel-Output), Previous: [Parallel Execution](#Parallel), Up: [Parallel Execution](#Parallel)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 5.4.1 Disabling Parallel Execution

If a makefile completely and accurately defines the dependency relationships
between all of its targets, then `make` will correctly build the goals
regardless of whether parallel execution is enabled or not. This is the ideal
way to write makefiles.

However, sometimes some or all of the targets in a makefile cannot be executed
in parallel and it’s not feasible to add the prerequisites needed to inform
`make`. In that case the makefile can use various methods to disable
parallel execution.

If the `.NOTPARALLEL` special target with no prerequisites is specified
anywhere then the entire instance of `make` will be run serially,
regardless of the parallel setting. For example:

```
all: one two three
one two three: ; @sleep 1; echo $@

.NOTPARALLEL:
```

Regardless of how `make` is invoked, the targets `one`, `two`,
and `three` will be run serially.

If the `.NOTPARALLEL` special target has prerequisites, then each of
those prerequisites will be considered a target and all prerequisites of these
targets will be run serially. Note that only when building this target will
the prerequisites be run serially: if some other target lists the same
prerequisites and is not in `.NOTPARALLEL` then these prerequisites may
be run in parallel. For example:

```
all: base notparallel

base: one two three
notparallel: one two three

one two three: ; @sleep 1; echo $@

.NOTPARALLEL: notparallel
```

Here ‘`make -j base`’ will run the targets `one`, `two`, and
`three` in parallel, while ‘`make -j notparallel`’ will run them
serially. If you run ‘`make -j all`’ then they *will* be run in
parallel since `base` lists them as prerequisites and is not serialized.

The `.NOTPARALLEL` target should not have commands.

Finally you can control the serialization of specific prerequisites in a
fine-grained way using the `.WAIT` special target. When this target
appears in a prerequisite list and parallel execution is enabled, `make`
will not build any of the prerequisites to the *right* of `.WAIT`
until all prerequisites to the *left* of `.WAIT` have completed.
For example:

```
all: one two .WAIT three
one two three: ; @sleep 1; echo $@
```

If parallel execution is enabled, `make` will try to build `one` and
`two` in parallel but will not try to build `three` until both are
complete.

As with targets provided to `.NOTPARALLEL`, `.WAIT` takes effect
only when building the target in whose prerequisite list it appears. If the
same prerequisites are present in other targets, without `.WAIT`, then
they may still be run in parallel. Because of this, neither
`.NOTPARALLEL` with targets nor `.WAIT` are as reliable for
controlling parallel execution as defining a prerequisite relationship.
However they are easy to use and may be sufficient in less complex situations.

The `.WAIT` prerequisite will not be present in any of the automatic
variables for the rule.

You can create an actual target `.WAIT` in your makefile for portability
but this is not required to use this feature. If a `.WAIT` target is
created it should not have prerequisites or commands.

The `.WAIT` feature is also implemented in other versions of `make`
and it’s specified in the POSIX standard for `make`.

---

Next: [Input During Parallel Execution](#Parallel-Input), Previous: [Disabling Parallel Execution](#Parallel-Disable), Up: [Parallel Execution](#Parallel)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 5.4.2 Output During Parallel Execution

When running several recipes in parallel the output from each
recipe appears as soon as it is generated, with the result that
messages from different recipes may be interspersed, sometimes even
appearing on the same line. This can make reading the output very
difficult.

To avoid this you can use the ‘`--output-sync`’ (‘`-O`’) option.
This option instructs `make` to save the output from the commands
it invokes and print it all once the commands are completed.
Additionally, if there are multiple recursive `make` invocations
running in parallel, they will communicate so that only one of them is
generating output at a time.

If working directory printing is enabled (see [The
‘`--print-directory`’ Option](#g_t_002dw-Option)), the enter/leave messages are
printed around each output grouping. If you prefer not to see these
messages add the ‘`--no-print-directory`’ option to `MAKEFLAGS`.

There are four levels of granularity when synchronizing output,
specified by giving an argument to the option (e.g., ‘`-Oline`’ or
‘`--output-sync=recurse`’).

`none`
:   This is the default: all output is sent directly as it is generated and
    no synchronization is performed.

`line`
:   Output from each individual line of the recipe is grouped and printed
    as soon as that line is complete. If a recipe consists of multiple
    lines, they may be interspersed with lines from other recipes.

`target`
:   Output from the entire recipe for each target is grouped and printed
    once the target is complete. This is the default if the
    `--output-sync` or `-O` option is given with no argument.

`recurse`
:   Output from each recursive invocation of `make` is grouped and
    printed once the recursive invocation is complete.

Regardless of the mode chosen, the total build time will be the same.
The only difference is in how the output appears.

The ‘`target`’ and ‘`recurse`’ modes both collect the output of
the entire recipe of a target and display it uninterrupted when the
recipe completes. The difference between them is in how recipes that
contain recursive invocations of `make` are treated
(see [Recursive Use of `make`](#Recursion)). For all recipes
which have no recursive lines, the ‘`target`’ and ‘`recurse`’
modes behave identically.

If the ‘`recurse`’ mode is chosen, recipes that contain recursive
`make` invocations are treated the same as other targets: the
output from the recipe, including the output from the recursive
`make`, is saved and printed after the entire recipe is complete.
This ensures output from all the targets built by a given recursive
`make` instance are grouped together, which may make the output
easier to understand. However it also leads to long periods of time
during the build where no output is seen, followed by large bursts of
output. If you are not watching the build as it proceeds, but instead
viewing a log of the build after the fact, this may be the best option
for you.

If you are watching the output, the long gaps of quiet during the
build can be frustrating. The ‘`target`’ output synchronization
mode detects when `make` is going to be invoked recursively,
using the standard methods, and it will not synchronize the output of
those lines. The recursive `make` will perform the
synchronization for its targets and the output from each will be
displayed immediately when it completes. Be aware that output from
recursive lines of the recipe are not synchronized (for example if
the recursive line prints a message before running `make`, that
message will not be synchronized).

The ‘`line`’ mode can be useful for front-ends that are watching
the output of `make` to track when recipes are started and
completed.

Some programs invoked by `make` may behave differently if they
determine they’re writing output to a terminal versus a file (often
described as “interactive” vs. “non-interactive” modes). For
example, many programs that can display colorized output will not do
so if they determine they are not writing to a terminal. If your
makefile invokes a program like this then using the output
synchronization options will cause the program to believe it’s running
in “non-interactive” mode even though the output will ultimately go
to the terminal.

---

Previous: [Output During Parallel Execution](#Parallel-Output), Up: [Parallel Execution](#Parallel)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 5.4.3 Input During Parallel Execution

Two processes cannot both take input from the same device at the same
time. To make sure that only one recipe tries to take input from the
terminal at once, `make` will invalidate the standard input
streams of all but one running recipe. If another recipe attempts to
read from standard input it will usually incur a fatal error (a
‘`Broken pipe`’ signal).

It is unpredictable which recipe will have a valid standard input stream
(which will come from the terminal, or wherever you redirect the standard
input of `make`). The first recipe run will always get it first, and
the first recipe started after that one finishes will get it next, and so
on.

We will change how this aspect of `make` works if we find a better
alternative. In the mean time, you should not rely on any recipe using
standard input at all if you are using the parallel execution feature; but
if you are not using this feature, then standard input works normally in
all recipes.

---

Next: [Interrupting or Killing `make`](#Interrupts), Previous: [Parallel Execution](#Parallel), Up: [Writing Recipes in Rules](#Recipes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 5.5 Errors in Recipes

After each shell invocation returns, `make` looks at its exit
status. If the shell completed successfully (the exit status is
zero), the next line in the recipe is executed in a new shell; after
the last line is finished, the rule is finished.

If there is an error (the exit status is nonzero), `make` gives up on
the current rule, and perhaps on all rules.

Sometimes the failure of a certain recipe line does not indicate a problem.
For example, you may use the `mkdir` command to ensure that a
directory exists. If the directory already exists, `mkdir` will
report an error, but you probably want `make` to continue regardless.

To ignore errors in a recipe line, write a ‘`-`’ at the beginning
of the line’s text (after the initial tab). The ‘`-`’ is discarded
before the line is passed to the shell for execution.

For example,

```
clean:
        -rm -f *.o
```

This causes `make` to continue even if `rm` is unable to
remove a file.

When you run `make` with the ‘`-i`’ or ‘`--ignore-errors`’
flag, errors are ignored in all recipes of all rules. A rule in the
makefile for the special target `.IGNORE` has the same effect, if
there are no prerequisites. This is less flexible but sometimes useful.

When errors are to be ignored, because of either a ‘`-`’ or the
‘`-i`’ flag, `make` treats an error return just like success,
except that it prints out a message that tells you the status code
the shell exited with, and says that the error has been ignored.

When an error happens that `make` has not been told to ignore,
it implies that the current target cannot be correctly remade, and neither
can any other that depends on it either directly or indirectly. No further
recipes will be executed for these targets, since their preconditions
have not been achieved.

Normally `make` gives up immediately in this circumstance, returning a
nonzero status. However, if the ‘`-k`’ or ‘`--keep-going`’
flag is specified, `make`
continues to consider the other prerequisites of the pending targets,
remaking them if necessary, before it gives up and returns nonzero status.
For example, after an error in compiling one object file, ‘`make -k`’
will continue compiling other object files even though it already knows
that linking them will be impossible. See [Summary of Options](#Options-Summary).

The usual behavior assumes that your purpose is to get the specified
targets up to date; once `make` learns that this is impossible, it
might as well report the failure immediately. The ‘`-k`’ option says
that the real purpose is to test as many of the changes made in the
program as possible, perhaps to find several independent problems so
that you can correct them all before the next attempt to compile. This
is why Emacs’ `compile` command passes the ‘`-k`’ flag by
default.

Usually when a recipe line fails, if it has changed the target file at all,
the file is corrupted and cannot be used—or at least it is not
completely updated. Yet the file’s time stamp says that it is now up to
date, so the next time `make` runs, it will not try to update that
file. The situation is just the same as when the shell is killed by a
signal; see [Interrupting or Killing `make`](#Interrupts). So generally the right thing to do is to
delete the target file if the recipe fails after beginning to change
the file. `make` will do this if `.DELETE_ON_ERROR` appears
as a target. This is almost always what you want `make` to do, but
it is not historical practice; so for compatibility, you must explicitly
request it.

---

Next: [Recursive Use of `make`](#Recursion), Previous: [Errors in Recipes](#Errors), Up: [Writing Recipes in Rules](#Recipes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 5.6 Interrupting or Killing `make`

If `make` gets a fatal signal while a shell is executing, it may
delete the target file that the recipe was supposed to update. This is
done if the target file’s last-modification time has changed since
`make` first checked it.

The purpose of deleting the target is to make sure that it is remade from
scratch when `make` is next run. Why is this? Suppose you type
`Ctrl-c` while a compiler is running, and it has begun to write an
object file `foo.o`. The `Ctrl-c` kills the compiler, resulting
in an incomplete file whose last-modification time is newer than the source
file `foo.c`. But `make` also receives the `Ctrl-c` signal
and deletes this incomplete file. If `make` did not do this, the next
invocation of `make` would think that `foo.o` did not require
updating—resulting in a strange error message from the linker when it
tries to link an object file half of which is missing.

You can prevent the deletion of a target file in this way by making the
special target `.PRECIOUS` depend on it. Before remaking a target,
`make` checks to see whether it appears on the prerequisites of
`.PRECIOUS`, and thereby decides whether the target should be deleted
if a signal happens. Some reasons why you might do this are that the
target is updated in some atomic fashion, or exists only to record a
modification-time (its contents do not matter), or must exist at all
times to prevent other sorts of trouble.

Although `make` does its best to clean up there are certain situations
in which cleanup is impossible. For example, `make` may be killed by
an uncatchable signal. Or, one of the programs make invokes may be killed
or crash, leaving behind an up-to-date but corrupt target file: `make`
will not realize that this failure requires the target to be cleaned. Or
`make` itself may encounter a bug and crash.

For these reasons it’s best to write *defensive recipes*, which won’t
leave behind corrupted targets even if they fail. Most commonly these
recipes create temporary files rather than updating the target directly,
then rename the temporary file to the final target name. Some compilers
already behave this way, so that you don’t need to write a defensive recipe.

---

Next: [Defining Canned Recipes](#Canned-Recipes), Previous: [Interrupting or Killing `make`](#Interrupts), Up: [Writing Recipes in Rules](#Recipes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 5.7 Recursive Use of `make`

Recursive use of `make` means using `make` as a command in a
makefile. This technique is useful when you want separate makefiles for
various subsystems that compose a larger system. For example, suppose you
have a sub-directory `subdir` which has its own makefile, and you would
like the containing directory’s makefile to run `make` on the
sub-directory. You can do it by writing this:

```
subsystem:
        cd subdir && $(MAKE)
```

or, equivalently, this (see [Summary of Options](#Options-Summary)):

```
subsystem:
        $(MAKE) -C subdir
```

You can write recursive `make` commands just by copying this example,
but there are many things to know about how they work and why, and about
how the sub-`make` relates to the top-level `make`. You may
also find it useful to declare targets that invoke recursive
`make` commands as ‘`.PHONY`’ (for more discussion on when
this is useful, see [Phony Targets](#Phony-Targets)).

For your convenience, when GNU `make` starts (after it has
processed any `-C` options) it sets the variable `CURDIR` to
the pathname of the current working directory. This value is never
touched by `make` again: in particular note that if you include
files from other directories the value of `CURDIR` does not
change. The value has the same precedence it would have if it were
set in the makefile (by default, an environment variable `CURDIR`
will not override this value). Note that setting this variable has no
impact on the operation of `make` (it does not cause `make`
to change its working directory, for example).

* [How the `MAKE` Variable Works](#MAKE-Variable)
* [Communicating Variables to a Sub-`make`](#Variables_002fRecursion)
* [Communicating Options to a Sub-`make`](#Options_002fRecursion)
* [The ‘`--print-directory`’ Option](#g_t_002dw-Option)

---

Next: [Communicating Variables to a Sub-`make`](#Variables_002fRecursion), Previous: [Recursive Use of `make`](#Recursion), Up: [Recursive Use of `make`](#Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 5.7.1 How the `MAKE` Variable Works

Recursive `make` commands should always use the variable `MAKE`,
not the explicit command name ‘`make`’, as shown here:

```
subsystem:
        cd subdir && $(MAKE)
```

The value of this variable is the file name with which `make` was
invoked. If this file name was `/bin/make`, then the recipe executed
is ‘`cd subdir && /bin/make`’. If you use a special version of
`make` to run the top-level makefile, the same special version will be
executed for recursive invocations.

As a special feature, using the variable `MAKE` in the recipe of
a rule alters the effects of the ‘`-t`’ (‘`--touch`’), ‘`-n`’
(‘`--just-print`’), or ‘`-q`’ (‘`--question`’) option.
Using the `MAKE` variable has the same effect as using a ‘`+`’
character at the beginning of the recipe line. See [Instead of Executing the Recipes](#Instead-of-Execution). This special feature
is only enabled if the `MAKE` variable appears directly in the
recipe: it does not apply if the `MAKE` variable is referenced
through expansion of another variable. In the latter case you must
use the ‘`+`’ token to get these special effects.

Consider the command ‘`make -t`’ in the above example. (The
‘`-t`’ option marks targets as up to date without actually running
any recipes; see [Instead of Executing Recipes](#Instead-of-Execution).) Following the usual
definition of ‘`-t`’, a ‘`make -t`’ command in the example would
create a file named `subsystem` and do nothing else. What you
really want it to do is run ‘`cd subdir && make -t`’; but
that would require executing the recipe, and ‘`-t`’ says not to
execute recipes.

The special feature makes this do what you want: whenever a recipe
line of a rule contains the variable `MAKE`, the flags ‘`-t`’,
‘`-n`’ and ‘`-q`’ do not apply to that line. Recipe lines
containing `MAKE` are executed normally despite the presence of a
flag that causes most recipes not to be run. The usual
`MAKEFLAGS` mechanism passes the flags to the sub-`make`
(see [Communicating Options to a
Sub-`make`](#Options_002fRecursion)), so your request to touch the files, or print the
recipes, is propagated to the subsystem.

---

Next: [Communicating Options to a Sub-`make`](#Options_002fRecursion), Previous: [How the `MAKE` Variable Works](#MAKE-Variable), Up: [Recursive Use of `make`](#Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 5.7.2 Communicating Variables to a Sub-`make`

Variable values of the top-level `make` can be passed to the
sub-`make` through the environment by explicit request. These
variables are defined in the sub-`make` as defaults, but they do
not override variables defined in the makefile used by
the sub-`make` unless you use the ‘`-e`’ switch (see [Summary of Options](#Options-Summary)).

To pass down, or *export*, a variable, `make` adds the
variable and its value to the environment for running each line of the
recipe. The sub-`make`, in turn, uses the environment to
initialize its table of variable values. See [Variables from the Environment](#Environment).

Except by explicit request, `make` exports a variable only if it
is either defined in the environment initially, or if set on the command
line and its name consists only of letters, numbers, and underscores.

The value of the `make` variable `SHELL` is not exported.
Instead, the value of the `SHELL` variable from the invoking
environment is passed to the sub-`make`. You can force
`make` to export its value for `SHELL` by using the
`export` directive, described below. See [Choosing the Shell](#Choosing-the-Shell).

The special variable `MAKEFLAGS` is always exported (unless you
unexport it). `MAKEFILES` is exported if you set it to anything.

`make` automatically passes down variable values that were defined
on the command line, by putting them in the `MAKEFLAGS` variable.
See [Communicating Options to a Sub-`make`](#Options_002fRecursion).

Variables are *not* normally passed down if they were created by
default by `make` (see [Variables Used by
Implicit Rules](#Implicit-Variables)). The sub-`make` will define these for
itself.

If you want to export specific variables to a sub-`make`, use the
`export` directive, like this:

```
export variable …
```

If you want to *prevent* a variable from being exported, use the
`unexport` directive, like this:

```
unexport variable …
```

In both of these forms, the arguments to `export` and
`unexport` are expanded, and so could be variables or functions
which expand to a (list of) variable names to be (un)exported.

As a convenience, you can define a variable and export it at the same
time by doing:

```
export variable = value
```

has the same result as:

```
variable = value
export variable
```

and

```
export variable := value
```

has the same result as:

```
variable := value
export variable
```

Likewise,

```
export variable += value
```

is just like:

```
variable += value
export variable
```

See [Appending More Text to Variables](#Appending).

You may notice that the `export` and `unexport` directives
work in `make` in the same way they work in the shell, `sh`.

If you want all variables to be exported by default, you can use
`export` by itself:

```
export
```

This tells `make` that variables which are not explicitly mentioned in an
`export` or `unexport` directive should be exported. Any variable
given in an `unexport` directive will still *not* be exported.

The behavior elicited by an `export` directive by itself was the default
in older versions of GNU `make`. If your makefiles depend on this
behavior and you want to be compatible with old versions of `make`, you
can add the special target `.EXPORT_ALL_VARIABLES` to your makefile
instead of using the `export` directive. This will be ignored by old
`make`s, while the `export` directive will cause a syntax
error.

When using `export` by itself or `.EXPORT_ALL_VARIABLES` to export
variables by default, only variables whose names consist solely of
alphanumerics and underscores will be exported. To export other variables you
must specifically mention them in an `export` directive.

Adding a variable’s value to the environment requires it to be expanded. If
expanding a variable has side-effects (such as the `info` or `eval`
or similar functions) then these side-effects will be seen every time a
command is invoked. You can avoid this by ensuring that such variables have
names which are not exportable by default. However, a better solution is to
*not* use this “export by default” facility at all, and instead
explicitly `export` the relevant variables by name.

You can use `unexport` by itself to tell `make` *not* to export
variables by default. Since this is the default behavior, you would only need
to do this if `export` had been used by itself earlier (in an included
makefile, perhaps). You **cannot** use `export` and `unexport`
by themselves to have variables exported for some recipes and not for others.
The last `export` or `unexport` directive that appears by itself
determines the behavior for the entire run of `make`.

As a special feature, the variable `MAKELEVEL` is changed when it
is passed down from level to level. This variable’s value is a string
which is the depth of the level as a decimal number. The value is
‘`0`’ for the top-level `make`; ‘`1`’ for a sub-`make`,
‘`2`’ for a sub-sub-`make`, and so on. The incrementation
happens when `make` sets up the environment for a recipe.

The main use of `MAKELEVEL` is to test it in a conditional
directive (see [Conditional Parts of Makefiles](#Conditionals)); this
way you can write a makefile that behaves one way if run recursively and
another way if run directly by you.

You can use the variable `MAKEFILES` to cause all sub-`make`
commands to use additional makefiles. The value of `MAKEFILES` is
a whitespace-separated list of file names. This variable, if defined in
the outer-level makefile, is passed down through the environment; then
it serves as a list of extra makefiles for the sub-`make` to read
before the usual or specified ones. See [The
Variable `MAKEFILES`](#MAKEFILES-Variable).

---

Next: [The ‘`--print-directory`’ Option](#g_t_002dw-Option), Previous: [Communicating Variables to a Sub-`make`](#Variables_002fRecursion), Up: [Recursive Use of `make`](#Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 5.7.3 Communicating Options to a Sub-`make`

Flags such as ‘`-s`’ and ‘`-k`’ are passed automatically to the
sub-`make` through the variable `MAKEFLAGS`. This variable is
set up automatically by `make` to contain the flag letters that
`make` received. Thus, if you do ‘`make -ks`’ then
`MAKEFLAGS` gets the value ‘`ks`’.

As a consequence, every sub-`make` gets a value for `MAKEFLAGS` in
its environment. In response, it takes the flags from that value and
processes them as if they had been given as arguments. See [Summary of Options](#Options-Summary). This means that, unlike other environment variables,
`MAKEFLAGS` specified in the environment take precedence over
`MAKEFLAGS` specified in the makefile.

The value of `MAKEFLAGS` is a possibly empty group of characters
representing single-letter options that take no argument, followed by a space
and any options that take arguments or which have long option names. If an
option has both single-letter and long options, the single-letter option is
always preferred. If there are no single-letter options on the command line,
then the value of `MAKEFLAGS` starts with a space.

Likewise variables defined on the command line are passed to the
sub-`make` through `MAKEFLAGS`. Words in the value of
`MAKEFLAGS` that contain ‘`=`’, `make` treats as variable
definitions just as if they appeared on the command line.
See [Overriding Variables](#Overriding).

The options ‘`-C`’, ‘`-f`’, ‘`-o`’, and ‘`-W`’ are not put
into `MAKEFLAGS`; these options are not passed down.

The ‘`-j`’ option is a special case (see [Parallel Execution](#Parallel)).
If you set it to some numeric value ‘`N`’ and your operating system
supports it (most any UNIX system will; others typically won’t), the
parent `make` and all the sub-`make`s will communicate to
ensure that there are only ‘`N`’ jobs running at the same time
between them all. Note that any job that is marked recursive
(see [Instead of Executing Recipes](#Instead-of-Execution))
doesn’t count against the total jobs (otherwise we could get ‘`N`’
sub-`make`s running and have no slots left over for any real work!)

If your operating system doesn’t support the above communication, then
no ‘`-j`’ is added to `MAKEFLAGS`, so that sub-`make`s
run in non-parallel mode. If the ‘`-j`’ option were passed down
to sub-`make`s you would get many more jobs running in parallel
than you asked for. If you give ‘`-j`’ with no numeric argument,
meaning to run as many jobs as possible in parallel, this is passed
down, since multiple infinities are no more than one.

If you do not want to pass the other flags down, you must change the
value of `MAKEFLAGS`, for example like this:

```
subsystem:
        cd subdir && $(MAKE) MAKEFLAGS=
```

The command line variable definitions really appear in the variable
`MAKEOVERRIDES`, and `MAKEFLAGS` contains a reference to this
variable. If you do want to pass flags down normally, but don’t want to
pass down the command line variable definitions, you can reset
`MAKEOVERRIDES` to empty, like this:

```
MAKEOVERRIDES =
```

This is not usually useful to do. However, some systems have a small
fixed limit on the size of the environment, and putting so much
information into the value of `MAKEFLAGS` can exceed it. If you
see the error message ‘`Arg list too long`’, this may be the problem.

(For strict compliance with POSIX.2, changing `MAKEOVERRIDES` does
not affect `MAKEFLAGS` if the special target ‘`.POSIX`’ appears
in the makefile. You probably do not care about this.)

A similar variable `MFLAGS` exists also, for historical
compatibility. It has the same value as `MAKEFLAGS` except that it
does not contain the command line variable definitions, and it always
begins with a hyphen unless it is empty (`MAKEFLAGS` begins with a
hyphen only when it begins with an option that has no single-letter
version, such as ‘`--warn-undefined-variables`’). `MFLAGS` was
traditionally used explicitly in the recursive `make` command, like
this:

```
subsystem:
        cd subdir && $(MAKE) $(MFLAGS)
```

but now `MAKEFLAGS` makes this usage redundant. If you want your
makefiles to be compatible with old `make` programs, use this
technique; it will work fine with more modern `make` versions too.

The `MAKEFLAGS` variable can also be useful if you want to have
certain options, such as ‘`-k`’ (see [Summary of
Options](#Options-Summary)), set each time you run `make`. You simply put a value for
`MAKEFLAGS` in your environment. You can also set `MAKEFLAGS` in
a makefile, to specify additional flags that should also be in effect for
that makefile. (Note that you cannot use `MFLAGS` this way. That
variable is set only for compatibility; `make` does not interpret a
value you set for it in any way.)

When `make` interprets the value of `MAKEFLAGS` (either from the
environment or from a makefile), it first prepends a hyphen if the value
does not already begin with one. Then it chops the value into words
separated by blanks, and parses these words as if they were options given
on the command line (except that ‘`-C`’, ‘`-f`’, ‘`-h`’,
‘`-o`’, ‘`-W`’, and their long-named versions are ignored; and there
is no error for an invalid option).

If you do put `MAKEFLAGS` in your environment, you should be sure not
to include any options that will drastically affect the actions of
`make` and undermine the purpose of makefiles and of `make`
itself. For instance, the ‘`-t`’, ‘`-n`’, and ‘`-q`’ options, if
put in one of these variables, could have disastrous consequences and would
certainly have at least surprising and probably annoying effects.

If you’d like to run other implementations of `make` in addition
to GNU `make`, and hence do not want to add GNU
`make`-specific flags to the `MAKEFLAGS` variable, you can
add them to the `GNUMAKEFLAGS` variable instead. This variable
is parsed just before `MAKEFLAGS`, in the same way as
`MAKEFLAGS`. When `make` constructs `MAKEFLAGS` to
pass to a recursive `make` it will include all flags, even those
taken from `GNUMAKEFLAGS`. As a result, after parsing
`GNUMAKEFLAGS` GNU `make` sets this variable to the empty
string to avoid duplicating flags during recursion.

It’s best to use `GNUMAKEFLAGS` only with flags which won’t
materially change the behavior of your makefiles. If your makefiles
require GNU Make anyway then simply use `MAKEFLAGS`. Flags such
as ‘`--no-print-directory`’ or ‘`--output-sync`’ may be
appropriate for `GNUMAKEFLAGS`.

---

Previous: [Communicating Options to a Sub-`make`](#Options_002fRecursion), Up: [Recursive Use of `make`](#Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 5.7.4 The ‘`--print-directory`’ Option

If you use several levels of recursive `make` invocations, the
‘`-w`’ or ‘`--print-directory`’ option can make the output a
lot easier to understand by showing each directory as `make`
starts processing it and as `make` finishes processing it. For
example, if ‘`make -w`’ is run in the directory `/u/gnu/make`,
`make` will print a line of the form:

```
make: Entering directory `/u/gnu/make'.
```

before doing anything else, and a line of the form:

```
make: Leaving directory `/u/gnu/make'.
```

when processing is completed.

Normally, you do not need to specify this option because ‘`make`’
does it for you: ‘`-w`’ is turned on automatically when you use the
‘`-C`’ option, and in sub-`make`s. `make` will not
automatically turn on ‘`-w`’ if you also use ‘`-s`’, which says to
be silent, or if you use ‘`--no-print-directory`’ to explicitly
disable it.

---

Next: [Using Empty Recipes](#Empty-Recipes), Previous: [Recursive Use of `make`](#Recursion), Up: [Writing Recipes in Rules](#Recipes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 5.8 Defining Canned Recipes

When the same sequence of commands is useful in making various
targets, you can define it as a canned sequence with the `define`
directive, and refer to the canned sequence from the recipes for those
targets. The canned sequence is actually a variable, so the name must
not conflict with other variable names.

Here is an example of defining a canned recipe:

```
define run-yacc =
yacc $(firstword $^)
mv y.tab.c $@
endef
```

Here `run-yacc` is the name of the variable being defined;
`endef` marks the end of the definition; the lines in between are the
commands. The `define` directive does not expand variable references
and function calls in the canned sequence; the ‘`$`’ characters,
parentheses, variable names, and so on, all become part of the value of the
variable you are defining.
See [Defining Multi-Line Variables](#Multi_002dLine),
for a complete explanation of `define`.

The first command in this example runs Yacc on the first prerequisite of
whichever rule uses the canned sequence. The output file from Yacc is
always named `y.tab.c`. The second command moves the output to the
rule’s target file name.

To use the canned sequence, substitute the variable into the recipe of a
rule. You can substitute it like any other variable
(see [Basics of Variable References](#Reference)).
Because variables defined by `define` are recursively expanded
variables, all the variable references you wrote inside the `define`
are expanded now. For example:

```
foo.c : foo.y
        $(run-yacc)
```

‘`foo.y`’ will be substituted for the variable ‘`$^`’ when it occurs in
`run-yacc`’s value, and ‘`foo.c`’ for ‘`$@`’.

This is a realistic example, but this particular one is not needed in
practice because `make` has an implicit rule to figure out these
commands based on the file names involved
(see [Using Implicit Rules](#Implicit-Rules)).

In recipe execution, each line of a canned sequence is treated just as
if the line appeared on its own in the rule, preceded by a tab. In
particular, `make` invokes a separate sub-shell for each line. You
can use the special prefix characters that affect command lines
(‘`@`’, ‘`-`’, and ‘`+`’) on each line of a canned sequence.
See [Writing Recipes in Rules](#Recipes).
For example, using this canned sequence:

```
define frobnicate =
@echo "frobnicating target $@"
frob-step-1 $< -o $@-step-1
frob-step-2 $@-step-1 -o $@
endef
```

`make` will not echo the first line, the `echo` command.
But it *will* echo the following two recipe lines.

On the other hand, prefix characters on the recipe line that refers to
a canned sequence apply to every line in the sequence. So the rule:

```
frob.out: frob.in
        @$(frobnicate)
```

does not echo *any* recipe lines.
(See [Recipe Echoing](#Echoing), for a full explanation of ‘`@`’.)

---

Previous: [Defining Canned Recipes](#Canned-Recipes), Up: [Writing Recipes in Rules](#Recipes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 5.9 Using Empty Recipes

It is sometimes useful to define recipes which do nothing. This is done
simply by giving a recipe that consists of nothing but whitespace. For
example:

```
target: ;
```

defines an empty recipe for `target`. You could also use a line
beginning with a recipe prefix character to define an empty recipe,
but this would be confusing because such a line looks empty.

You may be wondering why you would want to define a recipe that does
nothing. One reason this is useful is to prevent a target from
getting implicit recipes (from implicit rules or the `.DEFAULT`
special target; see [Using Implicit Rules](#Implicit-Rules) and see [Defining Last-Resort Default Rules](#Last-Resort)).

Empty recipes can also be used to avoid errors for targets that will
be created as a side-effect of another recipe: if the target does not
exist the empty recipe ensures that `make` won’t complain that it
doesn’t know how to build the target, and `make` will assume the
target is out of date.

You may be inclined to define empty recipes for targets that are not
actual files, but only exist so that their prerequisites can be
remade. However, this is not the best way to do that, because the
prerequisites may not be remade properly if the target file actually
does exist. See [Phony Targets](#Phony-Targets), for a better way to
do this.

---

Next: [Conditional Parts of Makefiles](#Conditionals), Previous: [Writing Recipes in Rules](#Recipes), Up: [GNU `make`](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]## 6 How to Use Variables

A *variable* is a name defined in a makefile to represent a string
of text, called the variable’s *value*. These values are
substituted by explicit request into targets, prerequisites, recipes,
and other parts of the makefile. (In some other versions of `make`,
variables are called *macros*.)

Variables and functions in all parts of a makefile are expanded when
read, except for in recipes, the right-hand sides of variable
definitions using ‘`=`’, and the bodies of variable definitions
using the `define` directive. The value a variable expands to is
that of its most recent definition at the time of expansion. In other
words, variables are dynamically scoped.

Variables can represent lists of file names, options to pass to compilers,
programs to run, directories to look in for source files, directories to
write output in, or anything else you can imagine.

A variable name may be any sequence of characters not containing
‘`:`’, ‘`#`’, ‘`=`’, or whitespace. However, variable names
containing characters other than letters, numbers, and underscores
should be considered carefully, as in some shells they cannot be
passed through the environment to a sub-`make`
(see [Communicating Variables to a
Sub-`make`](#Variables_002fRecursion)). Variable names beginning with ‘`.`’ and an
uppercase letter may be given special meaning in future versions of
`make`.

Variable names are case-sensitive. The names ‘`foo`’, ‘`FOO`’,
and ‘`Foo`’ all refer to different variables.

It is traditional to use upper case letters in variable names, but we
recommend using lower case letters for variable names that serve internal
purposes in the makefile, and reserving upper case for parameters that
control implicit rules or for parameters that the user should override with
command options (see [Overriding Variables](#Overriding)).

A few variables have names that are a single punctuation character or
just a few characters. These are the *automatic variables*, and
they have particular specialized uses. See [Automatic Variables](#Automatic-Variables).

* [Basics of Variable References](#Reference)
* [The Two Flavors of Variables](#Flavors)
* [Advanced Features for Reference to Variables](#Advanced)
* [How Variables Get Their Values](#Values)
* [Setting Variables](#Setting)
* [Appending More Text to Variables](#Appending)
* [The `override` Directive](#Override-Directive)
* [Defining Multi-Line Variables](#Multi_002dLine)
* [Undefining Variables](#Undefine-Directive)
* [Variables from the Environment](#Environment)
* [Target-specific Variable Values](#Target_002dspecific)
* [Pattern-specific Variable Values](#Pattern_002dspecific)
* [Suppressing Inheritance](#Suppressing-Inheritance)
* [Other Special Variables](#Special-Variables)

---

Next: [The Two Flavors of Variables](#Flavors), Previous: [How to Use Variables](#Using-Variables), Up: [How to Use Variables](#Using-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 6.1 Basics of Variable References

To substitute a variable’s value, write a dollar sign followed by the name
of the variable in parentheses or braces: either ‘`$(foo)`’ or
‘`${foo}`’ is a valid reference to the variable `foo`. This
special significance of ‘`$`’ is why you must write ‘`$$`’ to have
the effect of a single dollar sign in a file name or recipe.

Variable references can be used in any context: targets, prerequisites,
recipes, most directives, and new variable values. Here is an
example of a common case, where a variable holds the names of all the
object files in a program:

```
objects = program.o foo.o utils.o
program : $(objects)
        cc -o program $(objects)

$(objects) : defs.h
```

Variable references work by strict textual substitution. Thus, the rule

```
foo = c
prog.o : prog.$(foo)
        $(foo)$(foo) -$(foo) prog.$(foo)
```

could be used to compile a C program `prog.c`. Since spaces before
the variable value are ignored in variable assignments, the value of
`foo` is precisely ‘`c`’. (Don’t actually write your makefiles
this way!)

A dollar sign followed by a character other than a dollar sign,
open-parenthesis or open-brace treats that single character as the
variable name. Thus, you could reference the variable `x` with
‘`$x`’. However, this practice can lead to confusion (e.g.,
‘`$foo`’ refers to the variable `f` followed by the string
`oo`) so we recommend using parentheses or braces around all
variables, even single-letter variables, unless omitting them gives
significant readability improvements. One place where readability is
often improved is automatic variables (see [Automatic Variables](#Automatic-Variables)).

---

Next: [Advanced Features for Reference to Variables](#Advanced), Previous: [Basics of Variable References](#Reference), Up: [How to Use Variables](#Using-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 6.2 The Two Flavors of Variables

There are different ways that a variable in GNU `make` can get a value;
we call them the *flavors* of variables. The flavors are distinguished in
how they handle the values they are assigned in the makefile, and in how those
values are managed when the variable is later used and expanded.

* [Recursively Expanded Variable Assignment](#Recursive-Assignment)
* [Simply Expanded Variable Assignment](#Simple-Assignment)
* [Immediately Expanded Variable Assignment](#Immediate-Assignment)
* [Conditional Variable Assignment](#Conditional-Assignment)

---

Next: [Simply Expanded Variable Assignment](#Simple-Assignment), Previous: [The Two Flavors of Variables](#Flavors), Up: [The Two Flavors of Variables](#Flavors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 6.2.1 Recursively Expanded Variable Assignment

The first flavor of variable is a *recursively expanded* variable.
Variables of this sort are defined by lines using ‘`=`’
(see [Setting Variables](#Setting)) or by the `define` directive
(see [Defining Multi-Line Variables](#Multi_002dLine)). The value you specify
is installed verbatim; if it contains references to other variables,
these references are expanded whenever this variable is substituted (in
the course of expanding some other string). When this happens, it is
called *recursive expansion*.

For example,

```
foo = $(bar)
bar = $(ugh)
ugh = Huh?

all:;echo $(foo)
```

will echo ‘`Huh?`’: ‘`$(foo)`’ expands to ‘`$(bar)`’ which
expands to ‘`$(ugh)`’ which finally expands to ‘`Huh?`’.

This flavor of variable is the only sort supported by most other
versions of `make`. It has its advantages and its disadvantages.
An advantage (most would say) is that:

```
CFLAGS = $(include_dirs) -O
include_dirs = -Ifoo -Ibar
```

will do what was intended: when ‘`CFLAGS`’ is expanded in a recipe,
it will expand to ‘`-Ifoo -Ibar -O`’. A major disadvantage is that you
cannot append something on the end of a variable, as in

```
CFLAGS = $(CFLAGS) -O
```

because it will cause an infinite loop in the variable expansion.
(Actually `make` detects the infinite loop and reports an error.)

Another disadvantage is that any functions
(see [Functions for Transforming Text](#Functions))
referenced in the definition will be executed every time the variable is
expanded. This makes `make` run slower; worse, it causes the
`wildcard` and `shell` functions to give unpredictable results
because you cannot easily control when they are called, or even how many
times.

---

Next: [Immediately Expanded Variable Assignment](#Immediate-Assignment), Previous: [Recursively Expanded Variable Assignment](#Recursive-Assignment), Up: [The Two Flavors of Variables](#Flavors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 6.2.2 Simply Expanded Variable Assignment

To avoid the problems and inconveniences of recursively expanded
variables, there is another flavor: simply expanded variables.

*Simply expanded variables* are defined by lines using ‘`:=`’
or ‘`::=`’ (see [Setting Variables](#Setting)). Both forms are
equivalent in GNU `make`; however only the ‘`::=`’ form is
described by the POSIX standard (support for ‘`::=`’ is added to
the POSIX standard for POSIX Issue 8).

The value of a simply expanded variable is scanned once, expanding any
references to other variables and functions, when the variable is
defined. Once that expansion is complete the value of the variable is
never expanded again: when the variable is used the value is copied
verbatim as the expansion. If the value contained variable references
the result of the expansion will contain their values *as of the
time this variable was defined*. Therefore,

```
x := foo
y := $(x) bar
x := later
```

is equivalent to

```
y := foo bar
x := later
```

Here is a somewhat more complicated example, illustrating the use of
‘`:=`’ in conjunction with the `shell` function.
(See [The `shell` Function](#Shell-Function).) This example
also shows use of the variable `MAKELEVEL`, which is changed
when it is passed down from level to level.
(See [Communicating Variables to a
Sub-`make`](#Variables_002fRecursion), for information about `MAKELEVEL`.)

```
ifeq (0,${MAKELEVEL})
whoami    := $(shell whoami)
host-type := $(shell arch)
MAKE := ${MAKE} host-type=${host-type} whoami=${whoami}
endif
```

An advantage of this use of ‘`:=`’ is that a typical
‘descend into a directory’ recipe then looks like this:

```
${subdirs}:
        ${MAKE} -C $@ all
```

Simply expanded variables generally make complicated makefile programming
more predictable because they work like variables in most programming
languages. They allow you to redefine a variable using its own value (or
its value processed in some way by one of the expansion functions) and to
use the expansion functions much more efficiently
(see [Functions for Transforming Text](#Functions)).

You can also use them to introduce controlled leading whitespace into
variable values. Leading whitespace characters are discarded from your
input before substitution of variable references and function calls;
this means you can include leading spaces in a variable value by
protecting them with variable references, like this:

```
nullstring :=
space := $(nullstring) # end of the line
```

Here the value of the variable `space` is precisely one space. The
comment ‘`# end of the line`’ is included here just for clarity.
Since trailing space characters are *not* stripped from variable
values, just a space at the end of the line would have the same effect
(but be rather hard to read). If you put whitespace at the end of a
variable value, it is a good idea to put a comment like that at the end
of the line to make your intent clear. Conversely, if you do *not*
want any whitespace characters at the end of your variable value, you
must remember not to put a random comment on the end of the line after
some whitespace, such as this:

```
dir := /foo/bar    # directory to put the frobs in
```

Here the value of the variable `dir` is ‘`/foo/bar`’
(with four trailing spaces), which was probably not the intention.
(Imagine something like ‘`$(dir)/file`’ with this definition!)

---

Next: [Conditional Variable Assignment](#Conditional-Assignment), Previous: [Simply Expanded Variable Assignment](#Simple-Assignment), Up: [The Two Flavors of Variables](#Flavors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 6.2.3 Immediately Expanded Variable Assignment

Another form of assignment allows for immediate expansion, but unlike simple
assignment the resulting variable is recursive: it will be re-expanded again
on every use. In order to avoid unexpected results, after the value is
immediately expanded it will automatically be quoted: all instances of
`$` in the value after expansion will be converted into `$$`. This
type of assignment uses the ‘`:::=`’ operator. For example,

```
var = first
OUT :::= $(var)
var = second
```

results in the `OUT` variable containing the text ‘`first`’, while here:

```
var = one$$two
OUT :::= $(var)
var = three$$four
```

results in the `OUT` variable containing the text ‘`one$$two`’. The
value is expanded when the variable is assigned, so the result is the
expansion of the first value of `var`, ‘`one$two`’; then the value is
re-escaped before the assignment is complete giving the final result of
‘`one$$two`’.

The variable `OUT` is thereafter considered a recursive variable, so it
will be re-expanded when it is used.

This seems functionally equivalent to the ‘`:=`’ / ‘`::=`’ operators,
but there are a few differences:

First, after assignment the variable is a normal recursive variable; when you
append to it with ‘`+=`’ the value on the right-hand side is not expanded
immediately. If you prefer the ‘`+=`’ operator to expand the right-hand
side immediately you should use the ‘`:=`’ / ‘`::=`’ assignment instead.

Second, these variables are slightly less efficient than simply expanded
variables since they do need to be re-expanded when they are used, rather than
merely copied. However since all variable references are escaped this
expansion simply un-escapes the value, it won’t expand any variables or run
any functions.

Here is another example:

```
var = one$$two
OUT :::= $(var)
OUT += $(var)
var = three$$four
```

After this, the value of `OUT` is the text ‘`one$$two $(var)`’. When
this variable is used it will be expanded and the result will be
‘`one$two three$four`’.

This style of assignment is equivalent to the traditional BSD `make`
‘`:=`’ operator; as you can see it works slightly differently than the GNU
`make` ‘`:=`’ operator. The `:::=` operator is added to the
POSIX specification in Issue 8 to provide portability.

---

Previous: [Immediately Expanded Variable Assignment](#Immediate-Assignment), Up: [The Two Flavors of Variables](#Flavors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 6.2.4 Conditional Variable Assignment

There is another assignment operator for variables, ‘`?=`’. This
is called a conditional variable assignment operator, because it only
has an effect if the variable is not yet defined. This statement:

```
FOO ?= bar
```

is exactly equivalent to this
(see [The `origin` Function](#Origin-Function)):

```
ifeq ($(origin FOO), undefined)
  FOO = bar
endif
```

Note that a variable set to an empty value is still defined, so
‘`?=`’ will not set that variable.

---

Next: [How Variables Get Their Values](#Values), Previous: [The Two Flavors of Variables](#Flavors), Up: [How to Use Variables](#Using-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 6.3 Advanced Features for Reference to Variables

This section describes some advanced features you can use to reference
variables in more flexible ways.

* [Substitution References](#Substitution-Refs)
* [Computed Variable Names](#Computed-Names)

---

Next: [Computed Variable Names](#Computed-Names), Previous: [Advanced Features for Reference to Variables](#Advanced), Up: [Advanced Features for Reference to Variables](#Advanced)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 6.3.1 Substitution References

A *substitution reference* substitutes the value of a variable with
alterations that you specify. It has the form
‘`$(var:a=b)`’ (or
‘`${var:a=b}`’) and its meaning is to take the value
of the variable var, replace every a at the end of a word with
b in that value, and substitute the resulting string.

When we say “at the end of a word”, we mean that a must appear
either followed by whitespace or at the end of the value in order to be
replaced; other occurrences of a in the value are unaltered. For
example:

```
foo := a.o b.o l.a c.o
bar := $(foo:.o=.c)
```

sets ‘`bar`’ to ‘`a.c b.c l.a c.c`’. See [Setting Variables](#Setting).

A substitution reference is shorthand for the `patsubst`
expansion function (see [Functions for String Substitution and Analysis](#Text-Functions)):
‘`$(var:a=b)`’ is equivalent to
‘`$(patsubst %a,%b,var)`’. We provide
substitution references as well as `patsubst` for compatibility
with other implementations of `make`.

Another type of substitution reference lets you use the full power of
the `patsubst` function. It has the same form
‘`$(var:a=b)`’ described above, except that now
a must contain a single ‘`%`’ character. This case is
equivalent to ‘`$(patsubst a,b,$(var))`’.
See [Functions for String Substitution and Analysis](#Text-Functions),
for a description of the `patsubst` function. For example:

```
foo := a.o b.o l.a c.o
bar := $(foo:%.o=%.c)
```

sets ‘`bar`’ to ‘`a.c b.c l.a c.c`’.

---

Previous: [Substitution References](#Substitution-Refs), Up: [Advanced Features for Reference to Variables](#Advanced)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 6.3.2 Computed Variable Names

Computed variable names are an advanced concept, very useful in more
sophisticated makefile programming. In simple situations you need not
consider them, but they can be extremely useful.

Variables may be referenced inside the name of a variable. This is
called a *computed variable name* or a *nested variable
reference*. For example,

```
x = y
y = z
a := $($(x))
```

defines `a` as ‘`z`’: the ‘`$(x)`’ inside ‘`$($(x))`’ expands
to ‘`y`’, so ‘`$($(x))`’ expands to ‘`$(y)`’ which in turn expands
to ‘`z`’. Here the name of the variable to reference is not stated
explicitly; it is computed by expansion of ‘`$(x)`’. The reference
‘`$(x)`’ here is nested within the outer variable reference.

The previous example shows two levels of nesting, but any number of levels
is possible. For example, here are three levels:

```
x = y
y = z
z = u
a := $($($(x)))
```

Here the innermost ‘`$(x)`’ expands to ‘`y`’, so ‘`$($(x))`’
expands to ‘`$(y)`’ which in turn expands to ‘`z`’; now we have
‘`$(z)`’, which becomes ‘`u`’.

References to recursively-expanded variables within a variable name are
re-expanded in the usual fashion. For example:

```
x = $(y)
y = z
z = Hello
a := $($(x))
```

defines `a` as ‘`Hello`’: ‘`$($(x))`’ becomes ‘`$($(y))`’
which becomes ‘`$(z)`’ which becomes ‘`Hello`’.

Nested variable references can also contain modified references and function
invocations (see [Functions for Transforming Text](#Functions)), just like
any other reference. For example, using the `subst` function
(see [Functions for String Substitution and Analysis](#Text-Functions)):

```
x = variable1
variable2 := Hello
y = $(subst 1,2,$(x))
z = y
a := $($($(z)))
```

eventually defines `a` as ‘`Hello`’. It is doubtful that anyone
would ever want to write a nested reference as convoluted as this one, but
it works: ‘`$($($(z)))`’ expands to ‘`$($(y))`’ which becomes
‘`$($(subst 1,2,$(x)))`’. This gets the value ‘`variable1`’ from
`x` and changes it by substitution to ‘`variable2`’, so that the
entire string becomes ‘`$(variable2)`’, a simple variable reference
whose value is ‘`Hello`’.

A computed variable name need not consist entirely of a single variable
reference. It can contain several variable references, as well as some
invariant text. For example,

```
a_dirs := dira dirb
1_dirs := dir1 dir2
```

```

```

```
a_files := filea fileb
1_files := file1 file2
```

```

```

```
ifeq "$(use_a)" "yes"
a1 := a
else
a1 := 1
endif
```

```

```

```
ifeq "$(use_dirs)" "yes"
df := dirs
else
df := files
endif

dirs := $($(a1)_$(df))
```

will give `dirs` the same value as `a_dirs`, `1_dirs`,
`a_files` or `1_files` depending on the settings of `use_a`
and `use_dirs`.

Computed variable names can also be used in substitution references:

```
a_objects := a.o b.o c.o
1_objects := 1.o 2.o 3.o

sources := $($(a1)_objects:.o=.c)
```

defines `sources` as either ‘`a.c b.c c.c`’ or ‘`1.c 2.c 3.c`’,
depending on the value of `a1`.

The only restriction on this sort of use of nested variable references
is that they cannot specify part of the name of a function to be called.
This is because the test for a recognized function name is done before
the expansion of nested references. For example,

```
ifdef do_sort
func := sort
else
func := strip
endif
```

```

```

```
bar := a d b g q c
```

```

```

```
foo := $($(func) $(bar))
```

attempts to give ‘`foo`’ the value of the variable ‘`sort a d b g
q c`’ or ‘`strip a d b g q c`’, rather than giving ‘`a d b g q c`’
as the argument to either the `sort` or the `strip` function.
This restriction could be removed in the future if that change is shown
to be a good idea.

You can also use computed variable names in the left-hand side of a
variable assignment, or in a `define` directive, as in:

```
dir = foo
$(dir)_sources := $(wildcard $(dir)/*.c)
define $(dir)_print =
lpr $($(dir)_sources)
endef
```

This example defines the variables ‘`dir`’, ‘`foo_sources`’, and
‘`foo_print`’.

Note that *nested variable references* are quite different from
*recursively expanded variables*
(see [The Two Flavors of Variables](#Flavors)), though both are
used together in complex ways when doing makefile programming.

---

Next: [Setting Variables](#Setting), Previous: [Advanced Features for Reference to Variables](#Advanced), Up: [How to Use Variables](#Using-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 6.4 How Variables Get Their Values

Variables can get values in several different ways:

* You can specify an overriding value when you run `make`.
  See [Overriding Variables](#Overriding).
* You can specify a value in the makefile, either
  with an assignment (see [Setting Variables](#Setting)) or with a
  verbatim definition (see [Defining Multi-Line Variables](#Multi_002dLine)).
* You can specify a short-lived value with the `let` function
  (see [The `let` Function](#Let-Function)) or with the `foreach` function
  (see [The `foreach` Function](#Foreach-Function)).
* Variables in the environment become `make` variables.
  See [Variables from the Environment](#Environment).
* Several *automatic* variables are given new values for each rule.
  Each of these has a single conventional use.
  See [Automatic Variables](#Automatic-Variables).
* Several variables have constant initial values.
  See [Variables Used by Implicit Rules](#Implicit-Variables).

---

Next: [Appending More Text to Variables](#Appending), Previous: [How Variables Get Their Values](#Values), Up: [How to Use Variables](#Using-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 6.5 Setting Variables

To set a variable from the makefile, write a line starting with the variable
name followed by one of the assignment operators ‘`=`’, ‘`:=`’,
‘`::=`’, or ‘`:::=`’. Whatever follows the operator and any initial
whitespace on the line becomes the value. For example,

```
objects = main.o foo.o bar.o utils.o
```

defines a variable named `objects` to contain the value ‘`main.o
foo.o bar.o utils.o`’. Whitespace around the variable name and immediately
after the ‘`=`’ is ignored.

Variables defined with ‘`=`’ are *recursively expanded* variables.
Variables defined with ‘`:=`’ or ‘`::=`’ are *simply expanded*
variables; these definitions can contain variable references which will be
expanded before the definition is made. Variables defined with ‘`:::=`’
are *immediately expanded* variables. The different assignment operators
are described in See [The Two Flavors of Variables](#Flavors).

The variable name may contain function and variable references, which
are expanded when the line is read to find the actual variable name to use.

There is no limit on the length of the value of a variable except the
amount of memory on the computer. You can split the value of a
variable into multiple physical lines for readability
(see [Splitting Long Lines](#Splitting-Lines)).

Most variable names are considered to have the empty string as a value if
you have never set them. Several variables have built-in initial values
that are not empty, but you can set them in the usual ways
(see [Variables Used by Implicit Rules](#Implicit-Variables)).
Several special variables are set
automatically to a new value for each rule; these are called the
*automatic* variables (see [Automatic Variables](#Automatic-Variables)).

If you’d like a variable to be set to a value only if it’s not already
set, then you can use the shorthand operator ‘`?=`’ instead of
‘`=`’. These two settings of the variable ‘`FOO`’ are identical
(see [The `origin` Function](#Origin-Function)):

```
FOO ?= bar
```

and

```
ifeq ($(origin FOO), undefined)
FOO = bar
endif
```

The shell assignment operator ‘`!=`’ can be used to execute a
shell script and set a variable to its output. This operator first
evaluates the right-hand side, then passes that result to the shell
for execution. If the result of the execution ends in a newline, that
one newline is removed; all other newlines are replaced by spaces.
The resulting string is then placed into the named
recursively-expanded variable. For example:

```
hash != printf '\043'
file_list != find . -name '*.c'
```

If the result of the execution could produce a `$`, and you don’t
intend what follows that to be interpreted as a make variable or
function reference, then you must replace every `$` with
`$$` as part of the execution. Alternatively, you can set a
simply expanded variable to the result of running a program using the
`shell` function call. See [The `shell`
Function](#Shell-Function). For example:

```
hash := $(shell printf '\043')
var := $(shell find . -name "*.c")
```

As with the `shell` function, the exit status of the just-invoked
shell script is stored in the `.SHELLSTATUS` variable.

---

Next: [The `override` Directive](#Override-Directive), Previous: [Setting Variables](#Setting), Up: [How to Use Variables](#Using-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 6.6 Appending More Text to Variables

Often it is useful to add more text to the value of a variable already defined.
You do this with a line containing ‘`+=`’, like this:

```
objects += another.o
```

This takes the value of the variable `objects`, and adds the text
‘`another.o`’ to it (preceded by a single space, if it has a value
already). Thus:

```
objects = main.o foo.o bar.o utils.o
objects += another.o
```

sets `objects` to ‘`main.o foo.o bar.o utils.o another.o`’.

Using ‘`+=`’ is similar to:

```
objects = main.o foo.o bar.o utils.o
objects := $(objects) another.o
```

but differs in ways that become important when you use more complex values.

When the variable in question has not been defined before, ‘`+=`’
acts just like normal ‘`=`’: it defines a recursively-expanded
variable. However, when there *is* a previous definition, exactly
what ‘`+=`’ does depends on what flavor of variable you defined
originally. See [The Two Flavors of Variables](#Flavors), for an
explanation of the two flavors of variables.

When you add to a variable’s value with ‘`+=`’, `make` acts
essentially as if you had included the extra text in the initial definition of
the variable. If you defined it first with ‘`:=`’ or ‘`::=`’, making it
a simply-expanded variable, ‘`+=`’ adds to that simply-expanded definition,
and expands the new text before appending it to the old value just as
‘`:=`’ does (see [Setting Variables](#Setting), for a full explanation
of ‘`:=`’ or ‘`::=`’). In fact,

```
variable := value
variable += more
```

is exactly equivalent to:

```
variable := value
variable := $(variable) more
```

On the other hand, when you use ‘`+=`’ with a variable that you defined
first to be recursively-expanded using plain ‘`=`’ or ‘`:::=`’,
`make` appends the un-expanded text to the existing value, whatever it
is. This means that

```
variable = value
variable += more
```

is roughly equivalent to:

```
temp = value
variable = $(temp) more
```

except that of course it never defines a variable called `temp`.
The importance of this comes when the variable’s old value contains
variable references. Take this common example:

```
CFLAGS = $(includes) -O
…
CFLAGS += -pg # enable profiling
```

The first line defines the `CFLAGS` variable with a reference to another
variable, `includes`. (`CFLAGS` is used by the rules for C
compilation; see [Catalogue of Built-In Rules](#Catalogue-of-Rules).)
Using ‘`=`’ for the definition makes `CFLAGS` a recursively-expanded
variable, meaning ‘`$(includes) -O`’ is *not* expanded when
`make` processes the definition of `CFLAGS`. Thus, `includes`
need not be defined yet for its value to take effect. It only has to be
defined before any reference to `CFLAGS`. If we tried to append to the
value of `CFLAGS` without using ‘`+=`’, we might do it like this:

```
CFLAGS := $(CFLAGS) -pg # enable profiling
```

This is pretty close, but not quite what we want. Using ‘`:=`’
redefines `CFLAGS` as a simply-expanded variable; this means
`make` expands the text ‘`$(CFLAGS) -pg`’ before setting the
variable. If `includes` is not yet defined, we get ‘`-O -pg`’, and a later definition of `includes` will have no effect.
Conversely, by using ‘`+=`’ we set `CFLAGS` to the
*unexpanded* value ‘`$(includes) -O -pg`’. Thus we preserve
the reference to `includes`, so if that variable gets defined at
any later point, a reference like ‘`$(CFLAGS)`’ still uses its
value.

---

Next: [Defining Multi-Line Variables](#Multi_002dLine), Previous: [Appending More Text to Variables](#Appending), Up: [How to Use Variables](#Using-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 6.7 The `override` Directive

If a variable has been set with a command argument
(see [Overriding Variables](#Overriding)),
then ordinary assignments in the makefile are ignored. If you want to set
the variable in the makefile even though it was set with a command
argument, you can use an `override` directive, which is a line that
looks like this:

```
override variable = value
```

or

```
override variable := value
```

To append more text to a variable defined on the command line, use:

```
override variable += more text
```

See [Appending More Text to Variables](#Appending).

Variable assignments marked with the `override` flag have a
higher priority than all other assignments, except another
`override`. Subsequent assignments or appends to this variable
which are not marked `override` will be ignored.

The `override` directive was not invented for escalation in the war
between makefiles and command arguments. It was invented so you can alter
and add to values that the user specifies with command arguments.

For example, suppose you always want the ‘`-g`’ switch when you run the
C compiler, but you would like to allow the user to specify the other
switches with a command argument just as usual. You could use this
`override` directive:

```
override CFLAGS += -g
```

You can also use `override` directives with `define` directives.
This is done as you might expect:

```
override define foo =
bar
endef
```

See [Defining Multi-Line Variables](#Multi_002dLine).

---

Next: [Undefining Variables](#Undefine-Directive), Previous: [The `override` Directive](#Override-Directive), Up: [How to Use Variables](#Using-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 6.8 Defining Multi-Line Variables

Another way to set the value of a variable is to use the `define`
directive. This directive has an unusual syntax which allows newline
characters to be included in the value, which is convenient for
defining both canned sequences of commands (see [Defining Canned Recipes](#Canned-Recipes)), and also sections of makefile syntax to
use with `eval` (see [The `eval` Function](#Eval-Function)).

The `define` directive is followed on the same line by the name
of the variable being defined and an (optional) assignment operator,
and nothing more. The value to give the variable appears on the
following lines. The end of the value is marked by a line containing
just the word `endef`.

Aside from this difference in syntax, `define` works just like
any other variable definition. The variable name may contain function
and variable references, which are expanded when the directive is read
to find the actual variable name to use.

The final newline before the `endef` is not included in the
value; if you want your value to contain a trailing newline you must
include a blank line. For example in order to define a variable that
contains a newline character you must use *two* empty lines, not one:

```
define newline

endef
```

You may omit the variable assignment operator if you prefer. If
omitted, `make` assumes it to be ‘`=`’ and creates a
recursively-expanded variable (see [The Two Flavors of Variables](#Flavors)).
When using a ‘`+=`’ operator, the value is appended to the previous
value as with any other append operation: with a single space
separating the old and new values.

You may nest `define` directives: `make` will keep track of
nested directives and report an error if they are not all properly
closed with `endef`. Note that lines beginning with the recipe
prefix character are considered part of a recipe, so any `define`
or `endef` strings appearing on such a line will not be
considered `make` directives.

```
define two-lines
echo foo
echo $(bar)
endef
```

When used in a recipe, the previous example is functionally equivalent
to this:

```
two-lines = echo foo; echo $(bar)
```

since two commands separated by semicolon behave much like two separate
shell commands. However, note that using two separate lines means
`make` will invoke the shell twice, running an independent sub-shell
for each line. See [Recipe Execution](#Execution).

If you want variable definitions made with `define` to take
precedence over command-line variable definitions, you can use the
`override` directive together with `define`:

```
override define two-lines =
foo
$(bar)
endef
```

See [The `override` Directive](#Override-Directive).

---

Next: [Variables from the Environment](#Environment), Previous: [Defining Multi-Line Variables](#Multi_002dLine), Up: [How to Use Variables](#Using-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 6.9 Undefining Variables

If you want to clear a variable, setting its value to empty is usually
sufficient. Expanding such a variable will yield the same result (empty
string) regardless of whether it was set or not. However, if you are
using the `flavor` (see [The `flavor` Function](#Flavor-Function)) and
`origin` (see [The `origin` Function](#Origin-Function)) functions, there is a difference
between a variable that was never set and a variable with an empty value.
In such situations you may want to use the `undefine` directive to
make a variable appear as if it was never set. For example:

```
foo := foo
bar = bar

undefine foo
undefine bar

$(info $(origin foo))
$(info $(flavor bar))
```

This example will print “undefined” for both variables.

If you want to undefine a command-line variable definition, you can use
the `override` directive together with `undefine`, similar to
how this is done for variable definitions:

```
override undefine CFLAGS
```

---

Next: [Target-specific Variable Values](#Target_002dspecific), Previous: [Undefining Variables](#Undefine-Directive), Up: [How to Use Variables](#Using-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 6.10 Variables from the Environment

Variables in `make` can come from the environment in which
`make` is run. Every environment variable that `make` sees
when it starts up is transformed into a `make` variable with the
same name and value. However, an explicit assignment in the makefile,
or with a command argument, overrides the environment. (If the
‘`-e`’ flag is specified, then values from the environment override
assignments in the makefile. See [Summary of
Options](#Options-Summary). But this is not recommended practice.)

Thus, by setting the variable `CFLAGS` in your environment, you can
cause all C compilations in most makefiles to use the compiler switches you
prefer. This is safe for variables with standard or conventional meanings
because you know that no makefile will use them for other things. (Note
this is not totally reliable; some makefiles set `CFLAGS` explicitly
and therefore are not affected by the value in the environment.)

When `make` runs a recipe, some variables defined in the makefile
are placed into the environment of each command `make` invokes.
By default, only variables that came from the `make`’s
environment or set on its command line are placed into the environment
of the commands. You can use the `export` directive to pass
other variables. See [Communicating Variables
to a Sub-`make`](#Variables_002fRecursion), for full details.

Other use of variables from the environment is not recommended. It is not
wise for makefiles to depend for their functioning on environment variables
set up outside their control, since this would cause different users to get
different results from the same makefile. This is against the whole
purpose of most makefiles.

Such problems would be especially likely with the variable
`SHELL`, which is normally present in the environment to specify
the user’s choice of interactive shell. It would be very undesirable
for this choice to affect `make`; so, `make` handles the
`SHELL` environment variable in a special way; see [Choosing the Shell](#Choosing-the-Shell).

---

Next: [Pattern-specific Variable Values](#Pattern_002dspecific), Previous: [Variables from the Environment](#Environment), Up: [How to Use Variables](#Using-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]
