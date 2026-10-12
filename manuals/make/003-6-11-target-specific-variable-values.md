### 6.11 Target-specific Variable Values

Variable values in `make` are usually global; that is, they are the
same regardless of where they are evaluated (unless they’re reset, of
course). Exceptions to that are variables defined with the `let`
function (see [The `let` Function](#Let-Function)) or the `foreach` function
(see [The `foreach` Function](#Foreach-Function), and automatic variables
(see [Automatic Variables](#Automatic-Variables)).

Another exception are *target-specific variable values*. This
feature allows you to define different values for the same variable,
based on the target that `make` is currently building. As with
automatic variables, these values are only available within the context
of a target’s recipe (and in other target-specific assignments).

Set a target-specific variable value like this:

```
target … : variable-assignment
```

Target-specific variable assignments can be prefixed with any or all of the
special keywords `export`, `unexport`, `override`, or
`private`; these apply their normal behavior to this instance of the
variable only.

Multiple target values create a target-specific variable value for
each member of the target list individually.

The variable-assignment can be any valid form of assignment; recursive
(‘`=`’), simple (‘`:=`’ or ‘`::=`’), immediate (‘`::=`’),
appending (‘`+=`’), or conditional (‘`?=`’). All variables that appear
within the variable-assignment are evaluated within the context of the
target: thus, any previously-defined target-specific variable values will be
in effect. Note that this variable is actually distinct from any “global”
value: the two variables do not have to have the same flavor (recursive vs.
simple).

Target-specific variables have the same priority as any other makefile
variable. Variables provided on the command line (and in the
environment if the ‘`-e`’ option is in force) will take precedence.
Specifying the `override` directive will allow the target-specific
variable value to be preferred.

There is one more special feature of target-specific variables: when
you define a target-specific variable that variable value is also in
effect for all prerequisites of this target, and all their
prerequisites, etc. (unless those prerequisites override that variable
with their own target-specific variable value). So, for example, a
statement like this:

```
prog : CFLAGS = -g
prog : prog.o foo.o bar.o
```

will set `CFLAGS` to ‘`-g`’ in the recipe for `prog`, but
it will also set `CFLAGS` to ‘`-g`’ in the recipes that create
`prog.o`, `foo.o`, and `bar.o`, and any recipes which
create their prerequisites.

Be aware that a given prerequisite will only be built once per
invocation of make, at most. If the same file is a prerequisite of
multiple targets, and each of those targets has a different value for
the same target-specific variable, then the first target to be built
will cause that prerequisite to be built and the prerequisite will
inherit the target-specific value from the first target. It will
ignore the target-specific values from any other targets.

---

Next: [Suppressing Inheritance](#Suppressing-Inheritance), Previous: [Target-specific Variable Values](#Target_002dspecific), Up: [How to Use Variables](#Using-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 6.12 Pattern-specific Variable Values

In addition to target-specific variable values
(see [Target-specific Variable Values](#Target_002dspecific)), GNU
`make` supports pattern-specific variable values. In this form,
the variable is defined for any target that matches the pattern
specified.

Set a pattern-specific variable value like this:

```
pattern … : variable-assignment
```

where pattern is a %-pattern. As with target-specific variable
values, multiple pattern values create a pattern-specific variable
value for each pattern individually. The variable-assignment can
be any valid form of assignment. Any command line variable setting will
take precedence, unless `override` is specified.

For example:

```
%.o : CFLAGS = -O
```

will assign `CFLAGS` the value of ‘`-O`’ for all targets
matching the pattern `%.o`.

If a target matches more than one pattern, the matching pattern-specific
variables with longer stems are interpreted first. This results in more
specific variables taking precedence over the more generic ones, for
example:

```
%.o: %.c
        $(CC) -c $(CFLAGS) $(CPPFLAGS) $< -o $@

lib/%.o: CFLAGS := -fPIC -g
%.o: CFLAGS := -g

all: foo.o lib/bar.o
```

In this example the first definition of the `CFLAGS` variable
will be used to update `lib/bar.o` even though the second one
also applies to this target. Pattern-specific variables which result
in the same stem length are considered in the order in which they
were defined in the makefile.

Pattern-specific variables are searched after any target-specific
variables defined explicitly for that target, and before target-specific
variables defined for the parent target.

---

Next: [Other Special Variables](#Special-Variables), Previous: [Pattern-specific Variable Values](#Pattern_002dspecific), Up: [How to Use Variables](#Using-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 6.13 Suppressing Inheritance

As described in previous sections, `make` variables are inherited
by prerequisites. This capability allows you to modify the behavior
of a prerequisite based on which targets caused it to be rebuilt. For
example, you might set a target-specific variable on a `debug`
target, then running ‘`make debug`’ will cause that variable to be
inherited by all prerequisites of `debug`, while just running
‘`make all`’ (for example) would not have that assignment.

Sometimes, however, you may not want a variable to be inherited. For
these situations, `make` provides the `private` modifier.
Although this modifier can be used with any variable assignment, it
makes the most sense with target- and pattern-specific variables. Any
variable marked `private` will be visible to its local target but
will not be inherited by prerequisites of that target. A global
variable marked `private` will be visible in the global scope but
will not be inherited by any target, and hence will not be visible
in any recipe.

As an example, consider this makefile:

```
EXTRA_CFLAGS =

prog: private EXTRA_CFLAGS = -L/usr/local/lib
prog: a.o b.o
```

Due to the `private` modifier, `a.o` and `b.o` will not
inherit the `EXTRA_CFLAGS` variable assignment from the
`prog` target.

---

Previous: [Suppressing Inheritance](#Suppressing-Inheritance), Up: [How to Use Variables](#Using-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 6.14 Other Special Variables

GNU `make` supports some variables that have special properties.

`MAKEFILE_LIST`
:   Contains the name of each makefile that is parsed by `make`, in
    the order in which it was parsed. The name is appended just
    before `make` begins to parse the makefile. Thus, if the first
    thing a makefile does is examine the last word in this variable, it
    will be the name of the current makefile. Once the current makefile
    has used `include`, however, the last word will be the
    just-included makefile.

    If a makefile named `Makefile` has this content:

    ```
    name1 := $(lastword $(MAKEFILE_LIST))

    include inc.mk

    name2 := $(lastword $(MAKEFILE_LIST))

    all:
            @echo name1 = $(name1)
            @echo name2 = $(name2)
    ```

    then you would expect to see this output:

    ```
    name1 = Makefile
    name2 = inc.mk
    ```

`.DEFAULT_GOAL`
:   Sets the default goal to be used if no targets were specified on the
    command line (see [Arguments to Specify the Goals](#Goals)). The
    `.DEFAULT_GOAL` variable allows you to discover the current
    default goal, restart the default goal selection algorithm by clearing
    its value, or to explicitly set the default goal. The following
    example illustrates these cases:

    ```
    # Query the default goal.
    ifeq ($(.DEFAULT_GOAL),)
      $(warning no default goal is set)
    endif

    .PHONY: foo
    foo: ; @echo $@

    $(warning default goal is $(.DEFAULT_GOAL))

    # Reset the default goal.
    .DEFAULT_GOAL :=

    .PHONY: bar
    bar: ; @echo $@

    $(warning default goal is $(.DEFAULT_GOAL))

    # Set our own.
    .DEFAULT_GOAL := foo
    ```

    This makefile prints:

    ```
    no default goal is set
    default goal is foo
    default goal is bar
    foo
    ```

    Note that assigning more than one target name to `.DEFAULT_GOAL` is
    invalid and will result in an error.

`MAKE_RESTARTS`
:   This variable is set only if this instance of `make` has
    restarted (see [How Makefiles Are Remade](#Remaking-Makefiles)): it
    will contain the number of times this instance has restarted. Note
    this is not the same as recursion (counted by the `MAKELEVEL`
    variable). You should not set, modify, or export this variable.

`MAKE_TERMOUT`

`MAKE_TERMERR`
:   When `make` starts it will check whether stdout and stderr will
    show their output on a terminal. If so, it will set
    `MAKE_TERMOUT` and `MAKE_TERMERR`, respectively, to the name
    of the terminal device (or `true` if this cannot be determined).
    If set these variables will be marked for export. These variables
    will not be changed by `make` and they will not be modified if
    already set.

    These values can be used (particularly in combination with output
    synchronization (see [Output During Parallel
    Execution](#Parallel-Output)) to determine whether `make` itself is writing to a
    terminal; they can be tested to decide whether to force recipe
    commands to generate colorized output for example.

    If you invoke a sub-`make` and redirect its stdout or stderr it
    is your responsibility to reset or unexport these variables as well,
    if your makefiles rely on them.

`.RECIPEPREFIX`
:   The first character of the value of this variable is used as the
    character make assumes is introducing a recipe line. If the variable
    is empty (as it is by default) that character is the standard tab
    character. For example, this is a valid makefile:

    ```
    .RECIPEPREFIX = >
    all:
    > @echo Hello, world
    ```

    The value of `.RECIPEPREFIX` can be changed multiple times; once set
    it stays in effect for all rules parsed until it is modified.

`.VARIABLES`
:   Expands to a list of the *names* of all global variables defined
    so far. This includes variables which have empty values, as well as
    built-in variables (see [Variables Used by
    Implicit Rules](#Implicit-Variables)), but does not include any variables which are only
    defined in a target-specific context. Note that any value you assign
    to this variable will be ignored; it will always return its special
    value.

`.FEATURES`
:   Expands to a list of special features supported by this version of
    `make`. Possible values include, but are not limited to:

    ‘`archives`’
    :   Supports `ar` (archive) files using special file name syntax.
        See [Using `make` to Update Archive Files](#Archives).

    ‘`check-symlink`’
    :   Supports the `-L` (`--check-symlink-times`) flag.
        See [Summary of Options](#Options-Summary).

    ‘`else-if`’
    :   Supports “else if” non-nested conditionals. See [Syntax of Conditionals](#Conditional-Syntax).

    ‘`extra-prereqs`’
    :   Supports the `.EXTRA_PREREQS` special target.

    ‘`grouped-target`’
    :   Supports grouped target syntax for explicit rules. See [Multiple Targets in a Rule](#Multiple-Targets).

    ‘`guile`’
    :   Has GNU Guile available as an embedded extension language.
        See [GNU Guile Integration](#Guile-Integration).

    ‘`jobserver`’
    :   Supports “job server” enhanced parallel builds. See [Parallel Execution](#Parallel).

    ‘`jobserver-fifo`’
    :   Supports “job server” enhanced parallel builds using named pipes.
        See [Integrating GNU `make`](#Integrating-make).

    ‘`load`’
    :   Supports dynamically loadable objects for creating custom extensions.
        See [Loading Dynamic Objects](#Loading-Objects).

    ‘`notintermediate`’
    :   Supports the `.NOTINTERMEDIATE` special target.
        See [Integrating GNU `make`](#Integrating-make).

    ‘`oneshell`’
    :   Supports the `.ONESHELL` special target. See [Using
        One Shell](#One-Shell).

    ‘`order-only`’
    :   Supports order-only prerequisites. See [Types
        of Prerequisites](#Prerequisite-Types).

    ‘`output-sync`’
    :   Supports the `--output-sync` command line option. See [Summary of Options](#Options-Summary).

    ‘`second-expansion`’
    :   Supports secondary expansion of prerequisite lists.

    ‘`shell-export`’
    :   Supports exporting `make` variables to `shell` functions.

    ‘`shortest-stem`’
    :   Uses the “shortest stem” method of choosing which pattern, of
        multiple applicable options, will be used. See [How
        Patterns Match](#Pattern-Match).

    ‘`target-specific`’
    :   Supports target-specific and pattern-specific variable assignments.
        See [Target-specific Variable Values](#Target_002dspecific).

    ‘`undefine`’
    :   Supports the `undefine` directive. See [Undefining Variables](#Undefine-Directive).

`.INCLUDE_DIRS`
:   Expands to a list of directories that `make` searches for
    included makefiles (see [Including Other Makefiles](#Include)).
    Note that modifying this variable’s value does not change the list of
    directories which are searched.

`.EXTRA_PREREQS`
:   Each word in this variable is a new prerequisite which is added to
    targets for which it is set. These prerequisites differ from normal
    prerequisites in that they do not appear in any of the automatic
    variables (see [Automatic Variables](#Automatic-Variables)). This allows prerequisites to
    be defined which do not impact the recipe.

    Consider a rule to link a program:

    ```
    myprog: myprog.o file1.o file2.o
           $(CC) $(CFLAGS) $(LDFLAGS) -o $@ $^ $(LDLIBS)
    ```

    Now suppose you want to enhance this makefile to ensure that updates
    to the compiler cause the program to be re-linked. You can add the
    compiler as a prerequisite, but you must ensure that it’s not passed
    as an argument to link command. You’ll need something like this:

    ```
    myprog: myprog.o file1.o file2.o $(CC)
           $(CC) $(CFLAGS) $(LDFLAGS) -o $@ \
               $(filter-out $(CC),$^) $(LDLIBS)
    ```

    Then consider having multiple extra prerequisites: they would all have
    to be filtered out. Using `.EXTRA_PREREQS` and target-specific
    variables provides a simpler solution:

    ```
    myprog: myprog.o file1.o file2.o
           $(CC) $(CFLAGS) $(LDFLAGS) -o $@ $^ $(LDLIBS)
    myprog: .EXTRA_PREREQS = $(CC)
    ```

    This feature can also be useful if you want to add prerequisites to a
    makefile you cannot easily modify: you can create a new file such as
    `extra.mk`:

    ```
    myprog: .EXTRA_PREREQS = $(CC)
    ```

    then invoke `make -f extra.mk -f Makefile`.

    Setting `.EXTRA_PREREQS` globally will cause those prerequisites
    to be added to all targets (which did not themselves override it with
    a target-specific value). Note `make` is smart enough not to add
    a prerequisite listed in `.EXTRA_PREREQS` as a prerequisite to
    itself.

---

Next: [Functions for Transforming Text](#Functions), Previous: [How to Use Variables](#Using-Variables), Up: [GNU `make`](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]## 7 Conditional Parts of Makefiles

A *conditional* directive causes part of a makefile to be obeyed
or ignored depending on the values of variables. Conditionals can
compare the value of one variable to another, or the value of a
variable to a constant string. Conditionals control what `make`
actually “sees” in the makefile, so they *cannot* be used to
control recipes at the time of execution.

* [Example of a Conditional](#Conditional-Example)
* [Syntax of Conditionals](#Conditional-Syntax)
* [Conditionals that Test Flags](#Testing-Flags)

---

Next: [Syntax of Conditionals](#Conditional-Syntax), Previous: [Conditional Parts of Makefiles](#Conditionals), Up: [Conditional Parts of Makefiles](#Conditionals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 7.1 Example of a Conditional

The following example of a conditional tells `make` to use one
set of libraries if the `CC` variable is ‘`gcc`’, and a
different set of libraries otherwise. It works by controlling which
of two recipe lines will be used for the rule. The result is that
‘`CC=gcc`’ as an argument to `make` changes not only which
compiler is used but also which libraries are linked.

```
libs_for_gcc = -lgnu
normal_libs =

foo: $(objects)
ifeq ($(CC),gcc)
        $(CC) -o foo $(objects) $(libs_for_gcc)
else
        $(CC) -o foo $(objects) $(normal_libs)
endif
```

This conditional uses three directives: one `ifeq`, one `else`
and one `endif`.

The `ifeq` directive begins the conditional, and specifies the
condition. It contains two arguments, separated by a comma and surrounded
by parentheses. Variable substitution is performed on both arguments and
then they are compared. The lines of the makefile following the
`ifeq` are obeyed if the two arguments match; otherwise they are
ignored.

The `else` directive causes the following lines to be obeyed if the
previous conditional failed. In the example above, this means that the
second alternative linking command is used whenever the first alternative
is not used. It is optional to have an `else` in a conditional.

The `endif` directive ends the conditional. Every conditional must
end with an `endif`. Unconditional makefile text follows.

As this example illustrates, conditionals work at the textual level:
the lines of the conditional are treated as part of the makefile, or
ignored, according to the condition. This is why the larger syntactic
units of the makefile, such as rules, may cross the beginning or the
end of the conditional.

When the variable `CC` has the value ‘`gcc`’, the above example has
this effect:

```
foo: $(objects)
        $(CC) -o foo $(objects) $(libs_for_gcc)
```

When the variable `CC` has any other value, the effect is this:

```
foo: $(objects)
        $(CC) -o foo $(objects) $(normal_libs)
```

Equivalent results can be obtained in another way by conditionalizing a
variable assignment and then using the variable unconditionally:

```
libs_for_gcc = -lgnu
normal_libs =

ifeq ($(CC),gcc)
  libs=$(libs_for_gcc)
else
  libs=$(normal_libs)
endif

foo: $(objects)
        $(CC) -o foo $(objects) $(libs)
```

---

Next: [Conditionals that Test Flags](#Testing-Flags), Previous: [Example of a Conditional](#Conditional-Example), Up: [Conditional Parts of Makefiles](#Conditionals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 7.2 Syntax of Conditionals

The syntax of a simple conditional with no `else` is as follows:

```
conditional-directive
text-if-true
endif
```

The text-if-true may be any lines of text, to be considered as part
of the makefile if the condition is true. If the condition is false, no
text is used instead.

The syntax of a complex conditional is as follows:

```
conditional-directive
text-if-true
else
text-if-false
endif
```

or:

```
conditional-directive-one
text-if-one-is-true
else conditional-directive-two
text-if-two-is-true
else
text-if-one-and-two-are-false
endif
```

There can be as many “`else` conditional-directive”
clauses as necessary. Once a given condition is true,
text-if-true is used and no other clause is used; if no
condition is true then text-if-false is used. The
text-if-true and text-if-false can be any number of lines
of text.

The syntax of the conditional-directive is the same whether the
conditional is simple or complex; after an `else` or not. There
are four different directives that test different conditions. Here is
a table of them:

`ifeq (arg1, arg2)`

`ifeq 'arg1' 'arg2'`

`ifeq "arg1" "arg2"`

`ifeq "arg1" 'arg2'`

`ifeq 'arg1' "arg2"`
:   Expand all variable references in arg1 and arg2 and
    compare them. If they are identical, the text-if-true is
    effective; otherwise, the text-if-false, if any, is effective.

    Often you want to test if a variable has a non-empty value. When the
    value results from complex expansions of variables and functions,
    expansions you would consider empty may actually contain whitespace
    characters and thus are not seen as empty. However, you can use the
    `strip` function (see [Functions for String Substitution and Analysis](#Text-Functions)) to avoid interpreting
    whitespace as a non-empty value. For example:

    ```
    ifeq ($(strip $(foo)),)
    text-if-empty
    endif
    ```

    will evaluate text-if-empty even if the expansion of
    `$(foo)` contains whitespace characters.

`ifneq (arg1, arg2)`

`ifneq 'arg1' 'arg2'`

`ifneq "arg1" "arg2"`

`ifneq "arg1" 'arg2'`

`ifneq 'arg1' "arg2"`
:   Expand all variable references in arg1 and arg2 and
    compare them. If they are different, the text-if-true is
    effective; otherwise, the text-if-false, if any, is effective.

`ifdef variable-name`
:   The `ifdef` form takes the *name* of a variable as its
    argument, not a reference to a variable. If the value of that
    variable has a non-empty value, the text-if-true is effective;
    otherwise, the text-if-false, if any, is effective. Variables
    that have never been defined have an empty value. The text
    variable-name is expanded, so it could be a variable or function
    that expands to the name of a variable. For example:

    ```
    bar = true
    foo = bar
    ifdef $(foo)
    frobozz = yes
    endif
    ```

    The variable reference `$(foo)` is expanded, yielding `bar`,
    which is considered to be the name of a variable. The variable
    `bar` is not expanded, but its value is examined to determine if
    it is non-empty.

    Note that `ifdef` only tests whether a variable has a value. It
    does not expand the variable to see if that value is nonempty.
    Consequently, tests using `ifdef` return true for all definitions
    except those like `foo =`. To test for an empty value, use
    `ifeq ($(foo),)`. For example,

    ```
    bar =
    foo = $(bar)
    ifdef foo
    frobozz = yes
    else
    frobozz = no
    endif
    ```

    sets ‘`frobozz`’ to ‘`yes`’, while:

    ```
    foo =
    ifdef foo
    frobozz = yes
    else
    frobozz = no
    endif
    ```

    sets ‘`frobozz`’ to ‘`no`’.

`ifndef variable-name`
:   If the variable variable-name has an empty value, the
    text-if-true is effective; otherwise, the text-if-false,
    if any, is effective. The rules for expansion and testing of
    variable-name are identical to the `ifdef` directive.

Extra spaces are allowed and ignored at the beginning of the
conditional directive line, but a tab is not allowed. (If the line
begins with a tab, it will be considered part of a recipe for a rule.)
Aside from this, extra spaces or tabs may be inserted with no effect
anywhere except within the directive name or within an argument. A
comment starting with ‘`#`’ may appear at the end of the line.

The other two directives that play a part in a conditional are `else`
and `endif`. Each of these directives is written as one word, with no
arguments. Extra spaces are allowed and ignored at the beginning of the
line, and spaces or tabs at the end. A comment starting with ‘`#`’ may
appear at the end of the line.

Conditionals affect which lines of the makefile `make` uses. If
the condition is true, `make` reads the lines of the
text-if-true as part of the makefile; if the condition is false,
`make` ignores those lines completely. It follows that syntactic
units of the makefile, such as rules, may safely be split across the
beginning or the end of the conditional.

`make` evaluates conditionals when it reads a makefile.
Consequently, you cannot use automatic variables in the tests of
conditionals because they are not defined until recipes are run
(see [Automatic Variables](#Automatic-Variables)).

To prevent intolerable confusion, it is not permitted to start a
conditional in one makefile and end it in another. However, you may
write an `include` directive within a conditional, provided you do
not attempt to terminate the conditional inside the included file.

---

Previous: [Syntax of Conditionals](#Conditional-Syntax), Up: [Conditional Parts of Makefiles](#Conditionals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 7.3 Conditionals that Test Flags

You can write a conditional that tests `make` command flags such as
‘`-t`’ by using the variable `MAKEFLAGS` together with the
`findstring` function
(see [Functions for String Substitution and Analysis](#Text-Functions)).
This is useful when `touch` is not enough to make a file appear up
to date.

Recall that `MAKEFLAGS` will put all single-letter options (such as
‘`-t`’) into the first word, and that word will be empty if no
single-letter options were given. To work with this, it’s helpful to add a
value at the start to ensure there’s a word: for example
‘`-$(MAKEFLAGS)`’.

The `findstring` function determines whether one string appears as a
substring of another. If you want to test for the ‘`-t`’ flag, use
‘`t`’ as the first string and the first word of `MAKEFLAGS` as the
other.

For example, here is how to arrange to use ‘`ranlib -t`’ to finish
marking an archive file up to date:

```
archive.a: …
ifneq (,$(findstring t,$(firstword -$(MAKEFLAGS))))
        +touch archive.a
        +ranlib -t archive.a
else
        ranlib archive.a
endif
```

The ‘`+`’ prefix marks those recipe lines as “recursive” so that
they will be executed despite use of the ‘`-t`’ flag.
See [Recursive Use of `make`](#Recursion).

---

Next: [How to Run `make`](#Running), Previous: [Conditional Parts of Makefiles](#Conditionals), Up: [GNU `make`](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]## 8 Functions for Transforming Text

*Functions* allow you to do text processing in the makefile to
compute the files to operate on or the commands to use in recipes.
You use a function in a *function call*, where you give the name
of the function and some text (the *arguments*) for the function
to operate on. The result of the function’s processing is substituted
into the makefile at the point of the call, just as a variable might
be substituted.

* [Function Call Syntax](#Syntax-of-Functions)
* [Functions for String Substitution and Analysis](#Text-Functions)
* [Functions for File Names](#File-Name-Functions)
* [Functions for Conditionals](#Conditional-Functions)
* [The `let` Function](#Let-Function)
* [The `foreach` Function](#Foreach-Function)
* [The `file` Function](#File-Function)
* [The `call` Function](#Call-Function)
* [The `value` Function](#Value-Function)
* [The `eval` Function](#Eval-Function)
* [The `origin` Function](#Origin-Function)
* [The `flavor` Function](#Flavor-Function)
* [Functions That Control Make](#Make-Control-Functions)
* [The `shell` Function](#Shell-Function)
* [The `guile` Function](#Guile-Function)

---

Next: [Functions for String Substitution and Analysis](#Text-Functions), Previous: [Functions for Transforming Text](#Functions), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.1 Function Call Syntax

A function call resembles a variable reference. It can appear
anywhere a variable reference can appear, and it is expanded using the
same rules as variable references. A function call looks like this:

```
$(function arguments)
```

or like this:

```
${function arguments}
```

Here function is a function name; one of a short list of names
that are part of `make`. You can also essentially create your own
functions by using the `call` built-in function.

The arguments are the arguments of the function. They are separated
from the function name by one or more spaces or tabs, and if there is more
than one argument, then they are separated by commas. Such whitespace and
commas are not part of an argument’s value. The delimiters which you use to
surround the function call, whether parentheses or braces, can appear in an
argument only in matching pairs; the other kind of delimiters may appear
singly. If the arguments themselves contain other function calls or variable
references, it is wisest to use the same kind of delimiters for all the
references; write ‘`$(subst a,b,$(x))`’, not ‘`$(subst a,b,${x})`’. This is because it is clearer, and because only one type of
delimiter is matched to find the end of the reference.

Each argument is expanded before the function is invoked, unless otherwise
noted below. The substitution is done in the order in which the arguments
appear.

#### Special Characters

When using characters that are special to `make` as function arguments,
you may need to hide them. GNU `make` doesn’t support escaping
characters with backslashes or other escape sequences; however, because
arguments are split before they are expanded you can hide them by putting them
into variables.

Characters you may need to hide include:

* Commas
* Initial whitespace in the first argument
* Unmatched open parenthesis or brace
* An open parenthesis or brace if you don’t want it to start a matched pair

For example, you can define variables `comma` and `space` whose
values are isolated comma and space characters, then substitute these
variables where such characters are wanted, like this:

```
comma:= ,
empty:=
space:= $(empty) $(empty)
foo:= a b c
bar:= $(subst $(space),$(comma),$(foo))
# bar is now ‘a,b,c’.
```

Here the `subst` function replaces each space with a comma, through
the value of `foo`, and substitutes the result.

---

Next: [Functions for File Names](#File-Name-Functions), Previous: [Function Call Syntax](#Syntax-of-Functions), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.2 Functions for String Substitution and Analysis

Here are some functions that operate on strings:

`$(subst from,to,text)` [¶](#index-subst-1)
:   Performs a textual replacement on the text text: each occurrence
    of from is replaced by to. The result is substituted for
    the function call. For example,

    ```
    $(subst ee,EE,feet on the street)
    ```

    produces the value ‘`fEEt on the strEEt`’.

`$(patsubst pattern,replacement,text)` [¶](#index-patsubst-1)
:   Finds whitespace-separated words in text that match
    pattern and replaces them with replacement. Here
    pattern may contain a ‘`%`’ which acts as a wildcard,
    matching any number of any characters within a word. If
    replacement also contains a ‘`%`’, the ‘`%`’ is replaced
    by the text that matched the ‘`%`’ in pattern. Words that do
    not match the pattern are kept without change in the output. Only the
    first ‘`%`’ in the pattern and replacement is treated
    this way; any subsequent ‘`%`’ is unchanged.

    ‘`%`’ characters in `patsubst` function invocations can be
    quoted with preceding backslashes (‘`\`’). Backslashes that would
    otherwise quote ‘`%`’ characters can be quoted with more backslashes.
    Backslashes that quote ‘`%`’ characters or other backslashes are
    removed from the pattern before it is compared file names or has a stem
    substituted into it. Backslashes that are not in danger of quoting
    ‘`%`’ characters go unmolested. For example, the pattern
    `the\%weird\\%pattern\\` has ‘`the%weird\`’ preceding the
    operative ‘`%`’ character, and ‘`pattern\\`’ following it. The
    final two backslashes are left alone because they cannot affect any
    ‘`%`’ character.

    Whitespace between words is folded into single space characters;
    leading and trailing whitespace is discarded.

    For example,

    ```
    $(patsubst %.c,%.o,x.c.c bar.c)
    ```

    produces the value ‘`x.c.o bar.o`’.

    Substitution references (see [Substitution
    References](#Substitution-Refs)) are a simpler way to get the effect of the `patsubst`
    function:

    ```
    $(var:pattern=replacement)
    ```

    is equivalent to

    ```
    $(patsubst pattern,replacement,$(var))
    ```

    The second shorthand simplifies one of the most common uses of
    `patsubst`: replacing the suffix at the end of file names.

    ```
    $(var:suffix=replacement)
    ```

    is equivalent to

    ```
    $(patsubst %suffix,%replacement,$(var))
    ```

    For example, you might have a list of object files:

    ```
    objects = foo.o bar.o baz.o
    ```

    To get the list of corresponding source files, you could simply write:

    ```
    $(objects:.o=.c)
    ```

    instead of using the general form:

    ```
    $(patsubst %.o,%.c,$(objects))
    ```

`$(strip string)` [¶](#index-stripping-whitespace)
:   Removes leading and trailing whitespace from string and replaces
    each internal sequence of one or more whitespace characters with a
    single space. Thus, ‘`$(strip a b c )`’ results in ‘`a b c`’.

    The function `strip` can be very useful when used in conjunction
    with conditionals. When comparing something with the empty string
    ‘’ using `ifeq` or `ifneq`, you usually want a string of
    just whitespace to match the empty string (see [Conditional Parts of Makefiles](#Conditionals)).

    Thus, the following may fail to have the desired results:

    ```
    .PHONY: all
    ifneq   "$(needs_made)" ""
    all: $(needs_made)
    else
    all:;@echo 'Nothing to make!'
    endif
    ```

    Replacing the variable reference ‘`$(needs_made)`’ with the
    function call ‘`$(strip $(needs_made))`’ in the `ifneq`
    directive would make it more robust.

`$(findstring find,in)` [¶](#index-findstring)
:   Searches in for an occurrence of find. If it occurs, the
    value is find; otherwise, the value is empty. You can use this
    function in a conditional to test for the presence of a specific
    substring in a given string. Thus, the two examples,

    ```
    $(findstring a,a b c)
    $(findstring a,b c)
    ```

    produce the values ‘`a`’ and ‘’ (the empty string),
    respectively. See [Conditionals that Test Flags](#Testing-Flags), for a practical application of
    `findstring`.

`$(filter pattern…,text)`
:   Returns all whitespace-separated words in text that *do* match
    any of the pattern words, removing any words that *do not*
    match. The patterns are written using ‘`%`’, just like the patterns
    used in the `patsubst` function above.

    The `filter` function can be used to separate out different types
    of strings (such as file names) in a variable. For example:

    ```
    sources := foo.c bar.c baz.s ugh.h
    foo: $(sources)
            cc $(filter %.c %.s,$(sources)) -o foo
    ```

    says that `foo` depends of `foo.c`, `bar.c`,
    `baz.s` and `ugh.h` but only `foo.c`, `bar.c` and
    `baz.s` should be specified in the command to the
    compiler.

`$(filter-out pattern…,text)` [¶](#index-filter_002dout)
:   Returns all whitespace-separated words in text that *do not*
    match any of the pattern words, removing the words that *do*
    match one or more. This is the exact opposite of the `filter`
    function.

    For example, given:

    ```
    objects=main1.o foo.o main2.o bar.o
    mains=main1.o main2.o
    ```

    the following generates a list which contains all the object files not
    in ‘`mains`’:

    ```
    $(filter-out $(mains),$(objects))
    ```

`$(sort list)`
:   Sorts the words of list in lexical order, removing duplicate
    words. The output is a list of words separated by single spaces.
    Thus,

    ```
    $(sort foo bar lose)
    ```

    returns the value ‘`bar foo lose`’.

    Incidentally, since `sort` removes duplicate words, you can use
    it for this purpose even if you don’t care about the sort order.

`$(word n,text)` [¶](#index-word)
:   Returns the nth word of text. The legitimate values of
    n start from 1. If n is bigger than the number of words
    in text, the value is empty. For example,

    ```
    $(word 2, foo bar baz)
    ```

    returns ‘`bar`’.

`$(wordlist s,e,text)` [¶](#index-wordlist)
:   Returns the list of words in text starting with word s and
    ending with word e (inclusive). The legitimate values of s
    start from 1; e may start from 0. If s is bigger than the
    number of words in text, the value is empty. If e is
    bigger than the number of words in text, words up to the end of
    text are returned. If s is greater than e, nothing
    is returned. For example,

    ```
    $(wordlist 2, 3, foo bar baz)
    ```

    returns ‘`bar baz`’.

`$(words text)` [¶](#index-words)
:   Returns the number of words in text. Thus, the last word of text
    is `$(word $(words text),text)`.

`$(firstword names…)` [¶](#index-firstword)
:   The argument names is regarded as a series of names, separated
    by whitespace. The value is the first name in the series. The rest
    of the names are ignored.

    For example,

    ```
    $(firstword foo bar)
    ```

    produces the result ‘`foo`’. Although `$(firstword
    text)` is the same as `$(word 1,text)`, the
    `firstword` function is retained for its simplicity.

`$(lastword names…)` [¶](#index-lastword)
:   The argument names is regarded as a series of names, separated
    by whitespace. The value is the last name in the series.

    For example,

    ```
    $(lastword foo bar)
    ```

    produces the result ‘`bar`’. Although `$(lastword
    text)` is the same as `$(word $(words text),text)`,
    the `lastword` function was added for its simplicity and better
    performance.

Here is a realistic example of the use of `subst` and
`patsubst`. Suppose that a makefile uses the `VPATH` variable
to specify a list of directories that `make` should search for
prerequisite files
(see [`VPATH` Search Path for All Prerequisites](#General-Search)).
This example shows how to
tell the C compiler to search for header files in the same list of
directories.

The value of `VPATH` is a list of directories separated by colons,
such as ‘`src:../headers`’. First, the `subst` function is used to
change the colons to spaces:

```
$(subst :, ,$(VPATH))
```

This produces ‘`src ../headers`’. Then `patsubst` is used to turn
each directory name into a ‘`-I`’ flag. These can be added to the
value of the variable `CFLAGS`, which is passed automatically to the C
compiler, like this:

```
override CFLAGS += $(patsubst %,-I%,$(subst :, ,$(VPATH)))
```

The effect is to append the text ‘`-Isrc -I../headers`’ to the
previously given value of `CFLAGS`. The `override` directive is
used so that the new value is assigned even if the previous value of
`CFLAGS` was specified with a command argument (see [The `override` Directive](#Override-Directive)).

---

Next: [Functions for Conditionals](#Conditional-Functions), Previous: [Functions for String Substitution and Analysis](#Text-Functions), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.3 Functions for File Names

Several of the built-in expansion functions relate specifically to
taking apart file names or lists of file names.

Each of the following functions performs a specific transformation on a
file name. The argument of the function is regarded as a series of file
names, separated by whitespace. (Leading and trailing whitespace is
ignored.) Each file name in the series is transformed in the same way and
the results are concatenated with single spaces between them.

`$(dir names…)` [¶](#index-dir)
:   Extracts the directory-part of each file name in names. The
    directory-part of the file name is everything up through (and
    including) the last slash in it. If the file name contains no slash,
    the directory part is the string ‘`./`’. For example,

    ```
    $(dir src/foo.c hacks)
    ```

    produces the result ‘`src/ ./`’.

`$(notdir names…)` [¶](#index-notdir)
:   Extracts all but the directory-part of each file name in names.
    If the file name contains no slash, it is left unchanged. Otherwise,
    everything through the last slash is removed from it.

    A file name that ends with a slash becomes an empty string. This is
    unfortunate, because it means that the result does not always have the
    same number of whitespace-separated file names as the argument had;
    but we do not see any other valid alternative.

    For example,

    ```
    $(notdir src/foo.c hacks)
    ```

    produces the result ‘`foo.c hacks`’.

`$(suffix names…)` [¶](#index-suffix)
:   Extracts the suffix of each file name in names. If the file name
    contains a period, the suffix is everything starting with the last
    period. Otherwise, the suffix is the empty string. This frequently
    means that the result will be empty when names is not, and if
    names contains multiple file names, the result may contain fewer
    file names.

    For example,

    ```
    $(suffix src/foo.c src-1.0/bar.c hacks)
    ```

    produces the result ‘`.c .c`’.

`$(basename names…)` [¶](#index-basename-1)
:   Extracts all but the suffix of each file name in names. If the
    file name contains a period, the basename is everything starting up to
    (and not including) the last period. Periods in the directory part are
    ignored. If there is no period, the basename is the entire file name.
    For example,

    ```
    $(basename src/foo.c src-1.0/bar hacks)
    ```

    produces the result ‘`src/foo src-1.0/bar hacks`’.

`$(addsuffix suffix,names…)` [¶](#index-addsuffix)
:   The argument names is regarded as a series of names, separated
    by whitespace; suffix is used as a unit. The value of
    suffix is appended to the end of each individual name and the
    resulting larger names are concatenated with single spaces between
    them. For example,

    ```
    $(addsuffix .c,foo bar)
    ```

    produces the result ‘`foo.c bar.c`’.

`$(addprefix prefix,names…)` [¶](#index-addprefix)
:   The argument names is regarded as a series of names, separated
    by whitespace; prefix is used as a unit. The value of
    prefix is prepended to the front of each individual name and the
    resulting larger names are concatenated with single spaces between
    them. For example,

    ```
    $(addprefix src/,foo bar)
    ```

    produces the result ‘`src/foo src/bar`’.

`$(join list1,list2)` [¶](#index-join)
:   Concatenates the two arguments word by word: the two first words (one
    from each argument) concatenated form the first word of the result, the
    two second words form the second word of the result, and so on. So the
    nth word of the result comes from the nth word of each
    argument. If one argument has more words that the other, the extra
    words are copied unchanged into the result.

    For example, ‘`$(join a b,.c .o)`’ produces ‘`a.c b.o`’.

    Whitespace between the words in the lists is not preserved; it is
    replaced with a single space.

    This function can merge the results of the `dir` and
    `notdir` functions, to produce the original list of files which
    was given to those two functions.

`$(wildcard pattern)` [¶](#index-wildcard-2)
:   The argument pattern is a file name pattern, typically containing
    wildcard characters (as in shell file name patterns). The result of
    `wildcard` is a space-separated list of the names of existing files
    that match the pattern.
    See [Using Wildcard Characters in File Names](#Wildcards).

`$(realpath names…)` [¶](#index-realpath-1)
:   For each file name in names return the canonical absolute name.
    A canonical name does not contain any `.` or `..` components,
    nor any repeated path separators (`/`) or symlinks. In case of a
    failure the empty string is returned. Consult the `realpath(3)`
    documentation for a list of possible failure causes.

`$(abspath names…)` [¶](#index-abspath-1)
:   For each file name in names return an absolute name that does
    not contain any `.` or `..` components, nor any repeated path
    separators (`/`). Note that, in contrast to `realpath`
    function, `abspath` does not resolve symlinks and does not require
    the file names to refer to an existing file or directory. Use the
    `wildcard` function to test for existence.

---

Next: [The `let` Function](#Let-Function), Previous: [Functions for File Names](#File-Name-Functions), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.4 Functions for Conditionals

There are four functions that provide conditional expansion. A key
aspect of these functions is that not all of the arguments are
expanded initially. Only those arguments which need to be expanded,
will be expanded.

`$(if condition,then-part[,else-part])` [¶](#index-if-1)
:   The `if` function provides support for conditional expansion in a
    functional context (as opposed to the GNU `make` makefile
    conditionals such as `ifeq` (see [Syntax of
    Conditionals](#Conditional-Syntax))).

    The first argument, condition, first has all preceding and
    trailing whitespace stripped, then is expanded. If it expands to any
    non-empty string, then the condition is considered to be true. If it
    expands to an empty string, the condition is considered to be false.

    If the condition is true then the second argument, then-part, is
    evaluated and this is used as the result of the evaluation of the entire
    `if` function.

    If the condition is false then the third argument, else-part, is
    evaluated and this is the result of the `if` function. If there is
    no third argument, the `if` function evaluates to nothing (the
    empty string).

    Note that only one of the then-part or the else-part will be
    evaluated, never both. Thus, either can contain side-effects (such as
    `shell` function calls, etc.)

`$(or condition1[,condition2[,condition3…]])` [¶](#index-or)
:   The `or` function provides a “short-circuiting” OR operation.
    Each argument is expanded, in order. If an argument expands to a
    non-empty string the processing stops and the result of the expansion
    is that string. If, after all arguments are expanded, all of them are
    false (empty), then the result of the expansion is the empty string.

`$(and condition1[,condition2[,condition3…]])` [¶](#index-and)
:   The `and` function provides a “short-circuiting” AND operation.
    Each argument is expanded, in order. If an argument expands to an
    empty string the processing stops and the result of the expansion is
    the empty string. If all arguments expand to a non-empty string then
    the result of the expansion is the expansion of the last argument.

`$(intcmp lhs,rhs[,lt-part[,eq-part[,gt-part]]])` [¶](#index-intcmp)
:   The `intcmp` function provides support for numerical comparison of
    integers. This function has no counterpart among the GNU `make` makefile
    conditionals.

    The left-hand side, lhs, and right-hand side, rhs, are expanded
    and parsed as integral numbers in base 10. Expansion of the remaining
    arguments is controlled by how the numerical left-hand side compares to the
    numerical right-hand side.

    If there are no further arguments, then the function expands to empty if the
    left-hand side and right-hand side do not compare equal, or to their numerical
    value if they do compare equal.

    Else if the left-hand side is strictly less than the right-hand side, the
    `intcmp` function evaluates to the expansion of the third argument,
    lt-part. If both sides compare equal, then the `intcmp` function
    evaluates to the expansion of the fourth argument, eq-part. If the
    left-hand side is strictly greater than the right-hand side, then the
    `intcmp` function evaluates to the expansion of the fifth argument,
    gt-part.

    If gt-part is missing, it defaults to eq-part. If eq-part
    is missing, it defaults to the empty string. Thus both ‘`$(intcmp
    9,7,hello)`’ and ‘`$(intcmp 9,7,hello,world,)`’ evaluate to the empty
    string, while ‘`$(intcmp 9,7,hello,world)`’ (notice the absence of a comma
    after `world`) evaluates to ‘`world`’.

---

Next: [The `foreach` Function](#Foreach-Function), Previous: [Functions for Conditionals](#Conditional-Functions), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.5 The `let` Function

The `let` function provides a means to limit the scope of a
variable. The assignment of the named variables in a `let`
expression is in effect only within the text provided by the
`let` expression, and this assignment doesn’t impact that named
variable in any outer scope.

Additionally, the `let` function enables list unpacking by
assigning all unassigned values to the last named variable.

The syntax of the `let` function is:

```
$(let var [var ...],[list],text)
```

The first two arguments, var and list, are expanded before
anything else is done; note that the last argument, text, is
**not** expanded at the same time. Next, each word of the
expanded value of list is bound to each of the variable names,
var, in turn, with the final variable name being bound to the
remainder of the expanded list. In other words, the first word
of list is bound to the first variable var, the second
word to the second variable var, and so on.

If there are more variable names in var than there are words in
list, the remaining var variable names are set to the
empty string. If there are fewer vars than words in list
then the last var is set to all remaining words in list.

The variables in var are assigned as simply-expanded variables
during the execution of `let`. See [The Two Flavors
of Variables](#Flavors).

After all variables are thus bound, text is expanded to provide
the result of the `let` function.

For example, this macro reverses the order of the words in the list
that it is given as its first argument:

```
reverse = $(let first rest,$1,\
            $(if $(rest),$(call reverse,$(rest)) )$(first))

all: ; @echo $(call reverse,d c b a)
```

will print `a b c d`. When first called, `let` will expand
$1 to `d c b a`. It will then assign first to
`d` and assign rest to `c b a`. It will then expand
the if-statement, where `$(rest)` is not empty so we recursively
invoke the reverse function with the value of rest which
is now `c b a`. The recursive invocation of `let` assigns
first to `c` and rest to `b a`. The recursion
continues until `let` is called with just a single value,
`a`. Here first is `a` and rest is empty, so we
do not recurse but simply expand `$(first)` to `a` and
return, which adds  `b`, etc.

After the reverse call is complete, the first and
rest variables are no longer set. If variables by those names
existed beforehand, they are not affected by the expansion of the
`reverse` macro.

---

Next: [The `file` Function](#File-Function), Previous: [The `let` Function](#Let-Function), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.6 The `foreach` Function

The `foreach` function is similar to the `let` function, but very
different from other functions. It causes one piece of text to be used
repeatedly, each time with a different substitution performed on it. The
`foreach` function resembles the `for` command in the
shell `sh` and the `foreach` command in the C-shell `csh`.

The syntax of the `foreach` function is:

```
$(foreach var,list,text)
```

The first two arguments, var and list, are expanded before
anything else is done; note that the last argument, text, is
**not** expanded at the same time. Then for each word of the expanded
value of list, the variable named by the expanded value of var
is set to that word, and text is expanded. Presumably text
contains references to that variable, so its expansion will be different
each time.

The result is that text is expanded as many times as there are
whitespace-separated words in list. The multiple expansions of
text are concatenated, with spaces between them, to make the result
of `foreach`.

This simple example sets the variable ‘`files`’ to the list of all files
in the directories in the list ‘`dirs`’:

```
dirs := a b c d
files := $(foreach dir,$(dirs),$(wildcard $(dir)/*))
```

Here text is ‘`$(wildcard $(dir)/*)`’. The first repetition
finds the value ‘`a`’ for `dir`, so it produces the same result
as ‘`$(wildcard a/*)`’; the second repetition produces the result
of ‘`$(wildcard b/*)`’; and the third, that of ‘`$(wildcard c/*)`’.

This example has the same result (except for setting ‘`dirs`’) as
the following example:

```
files := $(wildcard a/* b/* c/* d/*)
```

When text is complicated, you can improve readability by giving it
a name, with an additional variable:

```
find_files = $(wildcard $(dir)/*)
dirs := a b c d
files := $(foreach dir,$(dirs),$(find_files))
```

Here we use the variable `find_files` this way. We use plain ‘`=`’
to define a recursively-expanding variable, so that its value contains an
actual function call to be re-expanded under the control of `foreach`;
a simply-expanded variable would not do, since `wildcard` would be
called only once at the time of defining `find_files`.

Like the `let` function, the `foreach` function has no permanent
effect on the variable var; its value and flavor after the
`foreach` function call are the same as they were beforehand. The
other values which are taken from list are in effect only
temporarily, during the execution of `foreach`. The variable
var is a simply-expanded variable during the execution of
`foreach`. If var was undefined before the `foreach`
function call, it is undefined after the call.
See [The Two Flavors of Variables](#Flavors).

You must take care when using complex variable expressions that result in
variable names because many strange things are valid variable names, but
are probably not what you intended. For example,

```
files := $(foreach Esta-escrito-en-espanol!,b c ch,$(find_files))
```

might be useful if the value of `find_files` references the variable
whose name is ‘`Esta-escrito-en-espanol!`’ (es un nombre bastante largo,
no?), but it is more likely to be a mistake.

---

Next: [The `call` Function](#Call-Function), Previous: [The `foreach` Function](#Foreach-Function), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.7 The `file` Function

The `file` function allows the makefile to write to or read from
a file. Two modes of writing are supported: overwrite, where the text
is written to the beginning of the file and any existing content is
lost, and append, where the text is written to the end of the file,
preserving the existing content. In both cases the file is created if
it does not exist. It is a fatal error if the file cannot be opened
for writing, or if the write operation fails. The `file`
function expands to the empty string when writing to a file.

When reading from a file, the `file` function expands to the
verbatim contents of the file, except that the final newline (if there
is one) will be stripped. Attempting to read from a non-existent file
expands to the empty string.

The syntax of the `file` function is:

```
$(file op filename[,text])
```

When the `file` function is evaluated all its arguments are
expanded first, then the file indicated by filename will be
opened in the mode described by op.

The operator op can be `>` to indicate the file will be
overwritten with new content, `>>` to indicate the current
contents of the file will be appended to, or `<` to indicate the
contents of the file will be read in. The filename specifies
the file to be written to or read from. There may optionally be
whitespace between the operator and the file name.

When reading files, it is an error to provide a text value.

When writing files, text will be written to the file. If
text does not already end in a newline a final newline will be
written (even if text is the empty string). If the text
argument is not given at all, nothing will be written.

For example, the `file` function can be useful if your build
system has a limited command line size and your recipe runs a command
that can accept arguments from a file as well. Many commands use the
convention that an argument prefixed with an `@` specifies a
file containing more arguments. Then you might write your recipe in
this way:

```
program: $(OBJECTS)
        $(file >$@.in,$^)
        $(CMD) $(CMDFLAGS) @$@.in
        @rm $@.in
```

If the command required each argument to be on a separate line of the
input file, you might write your recipe like this:

```
program: $(OBJECTS)
        $(file >$@.in) $(foreach O,$^,$(file >>$@.in,$O))
        $(CMD) $(CMDFLAGS) @$@.in
        @rm $@.in
```

---

Next: [The `value` Function](#Value-Function), Previous: [The `file` Function](#File-Function), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.8 The `call` Function

The `call` function is unique in that it can be used to create new
parameterized functions. You can write a complex expression as the
value of a variable, then use `call` to expand it with different
values.

The syntax of the `call` function is:

```
$(call variable,param,param,…)
```

When `make` expands this function, it assigns each param to
temporary variables `$(1)`, `$(2)`, etc. The variable
`$(0)` will contain variable. There is no maximum number of
parameter arguments. There is no minimum, either, but it doesn’t make
sense to use `call` with no parameters.

Then variable is expanded as a `make` variable in the context
of these temporary assignments. Thus, any reference to `$(1)` in
the value of variable will resolve to the first param in the
invocation of `call`.

Note that variable is the *name* of a variable, not a
*reference* to that variable. Therefore you would not normally use
a ‘`$`’ or parentheses when writing it. (You can, however, use a
variable reference in the name if you want the name not to be a
constant.)

If variable is the name of a built-in function, the built-in function
is always invoked (even if a `make` variable by that name also
exists).

The `call` function expands the param arguments before
assigning them to temporary variables. This means that variable
values containing references to built-in functions that have special
expansion rules, like `foreach` or `if`, may not work as you
expect.

Some examples may make this clearer.

This macro simply reverses its arguments:

```
reverse = $(2) $(1)

foo = $(call reverse,a,b)
```

Here `foo` will contain ‘`b a`’.

This one is slightly more interesting: it defines a macro to search for
the first instance of a program in `PATH`:

```
pathsearch = $(firstword $(wildcard $(addsuffix /$(1),$(subst :, ,$(PATH)))))

LS := $(call pathsearch,ls)
```

Now the variable `LS` contains `/bin/ls` or similar.

The `call` function can be nested. Each recursive invocation gets
its own local values for `$(1)`, etc. that mask the values of
higher-level `call`. For example, here is an implementation of a
*map* function:

```
map = $(foreach a,$(2),$(call $(1),$(a)))
```

Now you can `map` a function that normally takes only one argument,
such as `origin`, to multiple values in one step:

```
o = $(call map,origin,o map MAKE)
```

and end up with `o` containing something like ‘`file file default`’.

A final caution: be careful when adding whitespace to the arguments to
`call`. As with other functions, any whitespace contained in the
second and subsequent arguments is kept; this can cause strange
effects. It’s generally safest to remove all extraneous whitespace when
providing parameters to `call`.

---

Next: [The `eval` Function](#Eval-Function), Previous: [The `call` Function](#Call-Function), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.9 The `value` Function

The `value` function provides a way for you to use the value of a
variable *without* having it expanded. Please note that this
does not undo expansions which have already occurred; for example if
you create a simply expanded variable its value is expanded during the
definition; in that case the `value` function will return the
same result as using the variable directly.

The syntax of the `value` function is:

```
$(value variable)
```

Note that variable is the *name* of a variable, not a
*reference* to that variable. Therefore you would not normally
use a ‘`$`’ or parentheses when writing it. (You can, however, use
a variable reference in the name if you want the name not to be a
constant.)

The result of this function is a string containing the value of
variable, without any expansion occurring. For example, in this
makefile:

```
FOO = $PATH

all:
        @echo $(FOO)
        @echo $(value FOO)
```

The first output line would be `ATH`, since the “$P” would be
expanded as a `make` variable, while the second output line would
be the current value of your `$PATH` environment variable, since
the `value` function avoided the expansion.

The `value` function is most often used in conjunction with the
`eval` function (see [The `eval` Function](#Eval-Function)).

---

Next: [The `origin` Function](#Origin-Function), Previous: [The `value` Function](#Value-Function), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.10 The `eval` Function

The `eval` function is very special: it allows you to define new
makefile constructs that are not constant; which are the result of
evaluating other variables and functions. The argument to the
`eval` function is expanded, then the results of that expansion
are parsed as makefile syntax. The expanded results can define new
`make` variables, targets, implicit or explicit rules, etc.

The result of the `eval` function is always the empty string;
thus, it can be placed virtually anywhere in a makefile without
causing syntax errors.

It’s important to realize that the `eval` argument is expanded
*twice*; first by the `eval` function, then the results of
that expansion are expanded again when they are parsed as makefile
syntax. This means you may need to provide extra levels of escaping
for “$” characters when using `eval`. The `value`
function (see [The `value` Function](#Value-Function)) can sometimes be useful in these
situations, to circumvent unwanted expansions.

Here is an example of how `eval` can be used; this example
combines a number of concepts and other functions. Although it might
seem overly complex to use `eval` in this example, rather than
just writing out the rules, consider two things: first, the template
definition (in `PROGRAM_template`) could need to be much more
complex than it is here; and second, you might put the complex,
“generic” part of this example into another makefile, then include
it in all the individual makefiles. Now your individual makefiles are
quite straightforward.

```
PROGRAMS    = server client

server_OBJS = server.o server_priv.o server_access.o
server_LIBS = priv protocol

client_OBJS = client.o client_api.o client_mem.o
client_LIBS = protocol

# Everything after this is generic

.PHONY: all
all: $(PROGRAMS)

define PROGRAM_template =
 $(1): $$($(1)_OBJS) $$($(1)_LIBS:%=-l%)
 ALL_OBJS   += $$($(1)_OBJS)
endef

$(foreach prog,$(PROGRAMS),$(eval $(call PROGRAM_template,$(prog))))

$(PROGRAMS):
        $(LINK.o) $^ $(LDLIBS) -o $@

clean:
        rm -f $(ALL_OBJS) $(PROGRAMS)
```

---

Next: [The `flavor` Function](#Flavor-Function), Previous: [The `eval` Function](#Eval-Function), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.11 The `origin` Function

The `origin` function is unlike most other functions in that it does
not operate on the values of variables; it tells you something *about*
a variable. Specifically, it tells you where it came from.

The syntax of the `origin` function is:

```
$(origin variable)
```

Note that variable is the *name* of a variable to inquire about,
not a *reference* to that variable. Therefore you would not normally
use a ‘`$`’ or parentheses when writing it. (You can, however, use a
variable reference in the name if you want the name not to be a constant.)

The result of this function is a string telling you how the variable
variable was defined:

‘`undefined`’
:   if variable was never defined.

‘`default`’
:   if variable has a default definition, as is usual with `CC`
    and so on. See [Variables Used by Implicit Rules](#Implicit-Variables).
    Note that if you have redefined a default variable, the `origin`
    function will return the origin of the later definition.

‘`environment`’
:   if variable was inherited from the environment provided to
    `make`.

‘`environment override`’
:   if variable was inherited from the environment provided to
    `make`, and is overriding a setting for variable in the
    makefile as a result of the ‘`-e`’ option (see [Summary of Options](#Options-Summary)).

‘`file`’
:   if variable was defined in a makefile.

‘`command line`’
:   if variable was defined on the command line.

‘`override`’
:   if variable was defined with an `override` directive in a
    makefile (see [The `override` Directive](#Override-Directive)).

‘`automatic`’
:   if variable is an automatic variable defined for the execution
    of the recipe for each rule (see [Automatic Variables](#Automatic-Variables)).

This information is primarily useful (other than for your curiosity) to
determine if you want to believe the value of a variable. For example,
suppose you have a makefile `foo` that includes another makefile
`bar`. You want a variable `bletch` to be defined in `bar`
if you run the command ‘`make -f bar`’, even if the environment contains
a definition of `bletch`. However, if `foo` defined
`bletch` before including `bar`, you do not want to override that
definition. This could be done by using an `override` directive in
`foo`, giving that definition precedence over the later definition in
`bar`; unfortunately, the `override` directive would also
override any command line definitions. So, `bar` could
include:

```
ifdef bletch
ifeq "$(origin bletch)" "environment"
bletch = barf, gag, etc.
endif
endif
```

If `bletch` has been defined from the environment, this will redefine
it.

If you want to override a previous definition of `bletch` if it came
from the environment, even under ‘`-e`’, you could instead write:

```
ifneq "$(findstring environment,$(origin bletch))" ""
bletch = barf, gag, etc.
endif
```

Here the redefinition takes place if ‘`$(origin bletch)`’ returns either
‘`environment`’ or ‘`environment override`’.
See [Functions for String Substitution and Analysis](#Text-Functions).

---

Next: [Functions That Control Make](#Make-Control-Functions), Previous: [The `origin` Function](#Origin-Function), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.12 The `flavor` Function

The `flavor` function, like the `origin` function, does not
operate on the values of variables but rather it tells you something
*about* a variable. Specifically, it tells you the flavor of a
variable (see [The Two Flavors of Variables](#Flavors)).

The syntax of the `flavor` function is:

```
$(flavor variable)
```

Note that variable is the *name* of a variable to inquire about,
not a *reference* to that variable. Therefore you would not normally
use a ‘`$`’ or parentheses when writing it. (You can, however, use a
variable reference in the name if you want the name not to be a constant.)

The result of this function is a string that identifies the flavor of the
variable variable:

‘`undefined`’
:   if variable was never defined.

‘`recursive`’
:   if variable is a recursively expanded variable.

‘`simple`’
:   if variable is a simply expanded variable.

---

Next: [The `shell` Function](#Shell-Function), Previous: [The `flavor` Function](#Flavor-Function), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.13 Functions That Control Make

These functions control the way make runs. Generally, they are used to
provide information to the user of the makefile or to cause make to stop
if some sort of environmental error is detected.

`$(error text…)` [¶](#index-error)
:   Generates a fatal error where the message is text. Note that
    the error is generated whenever this function is evaluated. So, if
    you put it inside a recipe or on the right side of a recursive
    variable assignment, it won’t be evaluated until later. The
    text will be expanded before the error is generated.

    For example,

    ```
    ifdef ERROR1
    $(error error is $(ERROR1))
    endif
    ```

    will generate a fatal error during the read of the makefile if the
    `make` variable `ERROR1` is defined. Or,

    ```
    ERR = $(error found an error!)

    .PHONY: err
    err: ; $(ERR)
    ```

    will generate a fatal error while `make` is running, if the
    `err` target is invoked.

`$(warning text…)` [¶](#index-warning)
:   This function works similarly to the `error` function, above,
    except that `make` doesn’t exit. Instead, text is expanded
    and the resulting message is displayed, but processing of the makefile
    continues.

    The result of the expansion of this function is the empty string.

`$(info text…)` [¶](#index-info)
:   This function does nothing more than print its (expanded) argument(s)
    to standard output. No makefile name or line number is added. The
    result of the expansion of this function is the empty string.

---

Next: [The `guile` Function](#Guile-Function), Previous: [Functions That Control Make](#Make-Control-Functions), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.14 The `shell` Function

The `shell` function is unlike any other function other than the
`wildcard` function
(see [The Function `wildcard`](#Wildcard-Function)) in that it
communicates with the world outside of `make`.

The `shell` function provides for `make` the same facility that
backquotes (‘`` ` ``’) provide in most shells: it does *command expansion*.
This means that it takes as an argument a shell command and expands to the
output of the command. The only processing `make` does on the result is
to convert each newline (or carriage-return / newline pair) to a single space.
If there is a trailing (carriage-return and) newline it will simply be
removed.

The commands run by calls to the `shell` function are run when the
function calls are expanded (see [How `make` Reads
a Makefile](#Reading-Makefiles)). Because this function involves spawning a new shell, you should
carefully consider the performance implications of using the `shell`
function within recursively expanded variables vs. simply expanded variables
(see [The Two Flavors of Variables](#Flavors)).

An alternative to the `shell` function is the ‘`!=`’ assignment
operator; it provides a similar behavior but has subtle differences
(see [Setting Variables](#Setting)). The ‘`!=`’ assignment operator is
included in newer POSIX standards.

After the `shell` function or ‘`!=`’ assignment operator is
used, its exit status is placed in the `.SHELLSTATUS` variable.

Here are some examples of the use of the `shell` function:

```
contents := $(shell cat foo)
```

sets `contents` to the contents of the file `foo`, with a space
(rather than a newline) separating each line.

```
files := $(shell echo *.c)
```

sets `files` to the expansion of ‘`*.c`’. Unless `make` is
using a very strange shell, this has the same result as
‘`$(wildcard *.c)`’ (as long as at least one ‘`.c`’ file
exists).

All variables that are marked as `export` will also be passed to the
shell started by the `shell` function. It is possible to create a
variable expansion loop: consider this `makefile`:

```
export HI = $(shell echo hi)
all: ; @echo $$HI
```

When `make` wants to run the recipe it must add the variable HI to
the environment; to do so it must be expanded. The value of this variable
requires an invocation of the `shell` function, and to invoke it we must
create its environment. Since HI is exported, we need to expand it to
create its environment. And so on. In this obscure case `make` will use
the value of the variable from the environment provided to `make`, or
else the empty string if there was none, rather than looping or issuing an
error. This is often what you want; for example:

```
export PATH = $(shell echo /usr/local/bin:$$PATH)
```

However, it would be simpler and more efficient to use a simply-expanded
variable here (‘`:=`’) in the first place.

---

Previous: [The `shell` Function](#Shell-Function), Up: [Functions for Transforming Text](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 8.15 The `guile` Function

If GNU `make` is built with support for GNU Guile as an embedded
extension language then the `guile` function will be available.
The `guile` function takes one argument which is first expanded
by `make` in the normal fashion, then passed to the GNU Guile
evaluator. The result of the evaluator is converted into a string and
used as the expansion of the `guile` function in the makefile.
See [GNU Guile Integration](#Guile-Integration) for details on
writing extensions to `make` in Guile.

You can determine whether GNU Guile support is available by checking
the `.FEATURES` variable for the word guile.

---

Next: [Using Implicit Rules](#Implicit-Rules), Previous: [Functions for Transforming Text](#Functions), Up: [GNU `make`](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]## 9 How to Run `make`

A makefile that says how to recompile a program can be used in more
than one way. The simplest use is to recompile every file that is out
of date. Usually, makefiles are written so that if you run
`make` with no arguments, it does just that.

But you might want to update only some of the files; you might want to use
a different compiler or different compiler options; you might want just to
find out which files are out of date without changing them.

By giving arguments when you run `make`, you can do any of these
things and many others.

The exit status of `make` is always one of three values:

`0`
:   The exit status is zero if `make` is successful.

`2`
:   The exit status is two if `make` encounters any errors.
    It will print messages describing the particular errors.

`1`
:   The exit status is one if you use the ‘`-q`’ flag and `make`
    determines that some target is not already up to date.
    See [Instead of Executing Recipes](#Instead-of-Execution).

* [Arguments to Specify the Makefile](#Makefile-Arguments)
* [Arguments to Specify the Goals](#Goals)
* [Instead of Executing Recipes](#Instead-of-Execution)
* [Avoiding Recompilation of Some Files](#Avoiding-Compilation)
* [Overriding Variables](#Overriding)
* [Testing the Compilation of a Program](#Testing)
* [Temporary Files](#Temporary-Files)
* [Summary of Options](#Options-Summary)

---

Next: [Arguments to Specify the Goals](#Goals), Previous: [How to Run `make`](#Running), Up: [How to Run `make`](#Running)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 9.1 Arguments to Specify the Makefile

The way to specify the name of the makefile is with the ‘`-f`’ or
‘`--file`’ option (‘`--makefile`’ also works). For example,
‘`-f altmake`’ says to use the file `altmake` as the makefile.

If you use the ‘`-f`’ flag several times and follow each ‘`-f`’
with an argument, all the specified files are used jointly as
makefiles.

If you do not use the ‘`-f`’ or ‘`--file`’ flag, the default is
to try `GNUmakefile`, `makefile`, and `Makefile`, in
that order, and use the first of these three which exists or can be made
(see [Writing Makefiles](#Makefiles)).

---

Next: [Instead of Executing Recipes](#Instead-of-Execution), Previous: [Arguments to Specify the Makefile](#Makefile-Arguments), Up: [How to Run `make`](#Running)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]
