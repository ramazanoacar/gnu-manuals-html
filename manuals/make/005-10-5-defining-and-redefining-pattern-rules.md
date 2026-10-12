### 10.5 Defining and Redefining Pattern Rules

You define an implicit rule by writing a *pattern rule*. A pattern
rule looks like an ordinary rule, except that its target contains the
character ‘`%`’ (exactly one of them). The target is considered a
pattern for matching file names; the ‘`%`’ can match any nonempty
substring, while other characters match only themselves. The prerequisites
likewise use ‘`%`’ to show how their names relate to the target name.

Thus, a pattern rule ‘`%.o : %.c`’ says how to make any file
`stem.o` from another file `stem.c`.

Note that expansion using ‘`%`’ in pattern rules occurs
**after** any variable or function expansions, which take place
when the makefile is read. See [How to Use
Variables](#Using-Variables), and [Functions for Transforming Text](#Functions).

* [Introduction to Pattern Rules](#Pattern-Intro)
* [Pattern Rule Examples](#Pattern-Examples)
* [Automatic Variables](#Automatic-Variables)
* [How Patterns Match](#Pattern-Match)
* [Match-Anything Pattern Rules](#Match_002dAnything-Rules)
* [Canceling Implicit Rules](#Canceling-Rules)

---

Next: [Pattern Rule Examples](#Pattern-Examples), Previous: [Defining and Redefining Pattern Rules](#Pattern-Rules), Up: [Defining and Redefining Pattern Rules](#Pattern-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 10.5.1 Introduction to Pattern Rules

A pattern rule contains the character ‘`%`’ (exactly one of them)
in the target; otherwise, it looks exactly like an ordinary rule. The
target is a pattern for matching file names; the ‘`%`’ matches any
nonempty substring, while other characters match only themselves.

For example, ‘`%.c`’ as a pattern matches any file name that ends in
‘`.c`’. ‘`s.%.c`’ as a pattern matches any file name that starts
with ‘`s.`’, ends in ‘`.c`’ and is at least five characters long.
(There must be at least one character to match the ‘`%`’.) The substring
that the ‘`%`’ matches is called the *stem*.

‘`%`’ in a prerequisite of a pattern rule stands for the same stem
that was matched by the ‘`%`’ in the target. In order for the
pattern rule to apply, its target pattern must match the file name
under consideration and all of its prerequisites (after pattern
substitution) must name files that exist or can be made. These files
become prerequisites of the target.

Thus, a rule of the form

```
%.o : %.c ; recipe…
```

specifies how to make a file `n.o`, with another file
`n.c` as its prerequisite, provided that `n.c`
exists or can be made.

There may also be prerequisites that do not use ‘`%`’; such a prerequisite
attaches to every file made by this pattern rule. These unvarying
prerequisites are useful occasionally.

A pattern rule need not have any prerequisites that contain ‘`%`’, or
in fact any prerequisites at all. Such a rule is effectively a general
wildcard. It provides a way to make any file that matches the target
pattern. See [Defining Last-Resort Default Rules](#Last-Resort).

More than one pattern rule may match a target. In this case
`make` will choose the “best fit” rule. See [How Patterns Match](#Pattern-Match).

Pattern rules may have more than one target; however, every target must
contain a `%` character. Multiple target patterns in pattern rules are
always treated as grouped targets (see [Multiple Targets
in a Rule](#Multiple-Targets)) regardless of whether they use the `:` or `&:`
separator.

There is one exception: if a pattern target is out of date or does
not exist and the makefile does not need to build it, then it will not cause
the other targets to be considered out of date. Note that this historical
exception will be removed in future versions of GNU `make` and should not
be relied on. If this situation is detected `make` will generate a
warning *pattern recipe did not update peer target*; however, `make`
cannot detect all such situations. Please be sure that your recipe updates
*all* the target patterns when it runs.

---

Next: [Automatic Variables](#Automatic-Variables), Previous: [Introduction to Pattern Rules](#Pattern-Intro), Up: [Defining and Redefining Pattern Rules](#Pattern-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 10.5.2 Pattern Rule Examples

Here are some examples of pattern rules actually predefined in
`make`. First, the rule that compiles ‘`.c`’ files into ‘`.o`’
files:

```
%.o : %.c
        $(CC) -c $(CFLAGS) $(CPPFLAGS) $< -o $@
```

defines a rule that can make any file `x.o` from
`x.c`. The recipe uses the automatic variables ‘`$@`’ and
‘`$<`’ to substitute the names of the target file and the source file
in each case where the rule applies (see [Automatic Variables](#Automatic-Variables)).

Here is a second built-in rule:

```
% :: RCS/%,v
        $(CO) $(COFLAGS) $<
```

defines a rule that can make any file `x` whatsoever from a
corresponding file `x,v` in the sub-directory `RCS`. Since
the target is ‘`%`’, this rule will apply to any file whatever, provided
the appropriate prerequisite file exists. The double colon makes the rule
*terminal*, which means that its prerequisite may not be an intermediate
file (see [Match-Anything Pattern Rules](#Match_002dAnything-Rules)).

This pattern rule has two targets:

```
%.tab.c %.tab.h: %.y
        bison -d $<
```

This tells `make` that the recipe ‘`bison -d x.y`’ will
make both `x.tab.c` and `x.tab.h`. If the file
`foo` depends on the files `parse.tab.o` and `scan.o`
and the file `scan.o` depends on the file `parse.tab.h`,
when `parse.y` is changed, the recipe ‘`bison -d parse.y`’
will be executed only once, and the prerequisites of both
`parse.tab.o` and `scan.o` will be satisfied. (Presumably
the file `parse.tab.o` will be recompiled from `parse.tab.c`
and the file `scan.o` from `scan.c`, while `foo` is
linked from `parse.tab.o`, `scan.o`, and its other
prerequisites, and it will execute happily ever after.)

---

Next: [How Patterns Match](#Pattern-Match), Previous: [Pattern Rule Examples](#Pattern-Examples), Up: [Defining and Redefining Pattern Rules](#Pattern-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 10.5.3 Automatic Variables

Suppose you are writing a pattern rule to compile a ‘`.c`’ file into a
‘`.o`’ file: how do you write the ‘`cc`’ command so that it operates
on the right source file name? You cannot write the name in the recipe,
because the name is different each time the implicit rule is applied.

What you do is use a special feature of `make`, the *automatic
variables*. These variables have values computed afresh for each rule that
is executed, based on the target and prerequisites of the rule. In this
example, you would use ‘`$@`’ for the object file name and ‘`$<`’
for the source file name.

It’s very important that you recognize the limited scope in which
automatic variable values are available: they only have values within
the recipe. In particular, you cannot use them anywhere
within the target list of a rule; they have no value there and will
expand to the empty string. Also, they cannot be accessed directly
within the prerequisite list of a rule. A common mistake is
attempting to use `$@` within the prerequisites list; this will
not work. However, there is a special feature of GNU `make`,
secondary expansion (see [Secondary Expansion](#Secondary-Expansion)), which will allow
automatic variable values to be used in prerequisite lists.

Here is a table of automatic variables:

`$@`
:   The file name of the target of the rule. If the target is an archive
    member, then ‘`$@`’ is the name of the archive file. In a pattern
    rule that has multiple targets (see [Introduction to
    Pattern Rules](#Pattern-Intro)), ‘`$@`’ is the name of whichever target caused the
    rule’s recipe to be run.

`$%`
:   The target member name, when the target is an archive member.
    See [Using `make` to Update Archive Files](#Archives). For example, if the target is `foo.a(bar.o)` then
    ‘`$%`’ is `bar.o` and ‘`$@`’ is `foo.a`. ‘`$%`’ is
    empty when the target is not an archive member.

`$<`
:   The name of the first prerequisite. If the target got its recipe from
    an implicit rule, this will be the first prerequisite added by the
    implicit rule (see [Using Implicit Rules](#Implicit-Rules)).

`$?`
:   The names of all the prerequisites that are newer than the target, with
    spaces between them. If the target does not exist, all prerequisites
    will be included. For prerequisites which are archive members, only the
    named member is used (see [Using `make` to Update Archive Files](#Archives)).

    ‘`$?`’ is useful even in explicit rules when you wish to operate on only
    the prerequisites that have changed. For example, suppose that an archive
    named `lib` is supposed to contain copies of several object files.
    This rule copies just the changed object files into the archive:

    ```
    lib: foo.o bar.o lose.o win.o
            ar r lib $?
    ```

`$^`
:   The names of all the prerequisites, with spaces between them. For
    prerequisites which are archive members, only the named member is used
    (see [Using `make` to Update Archive Files](#Archives)). A target has only one prerequisite on each other file
    it depends on, no matter how many times each file is listed as a
    prerequisite. So if you list a prerequisite more than once for a target,
    the value of `$^` contains just one copy of the name. This list
    does **not** contain any of the order-only prerequisites; for those
    see the ‘`$|`’ variable, below.

`$+`
:   This is like ‘`$^`’, but prerequisites listed more than once are
    duplicated in the order they were listed in the makefile. This is
    primarily useful for use in linking commands where it is meaningful to
    repeat library file names in a particular order.

`$|`
:   The names of all the order-only prerequisites, with spaces between
    them.

`$*`
:   The stem with which an implicit rule matches (see [How
    Patterns Match](#Pattern-Match)). If the target is `dir/a.foo.b` and the target
    pattern is `a.%.b` then the stem is `dir/foo`. The stem is
    useful for constructing names of related files.

    In a static pattern rule, the stem is part of the file name that matched
    the ‘`%`’ in the target pattern.

    In an explicit rule, there is no stem; so ‘`$*`’ cannot be determined
    in that way. Instead, if the target name ends with a recognized suffix
    (see [Old-Fashioned Suffix Rules](#Suffix-Rules)), ‘`$*`’ is set to
    the target name minus the suffix. For example, if the target name is
    ‘`foo.c`’, then ‘`$*`’ is set to ‘`foo`’, since ‘`.c`’ is a
    suffix. GNU `make` does this bizarre thing only for compatibility
    with other implementations of `make`. You should generally avoid
    using ‘`$*`’ except in implicit rules or static pattern rules.

    If the target name in an explicit rule does not end with a recognized
    suffix, ‘`$*`’ is set to the empty string for that rule.

Of the variables listed above, four have values that are single file
names, and three have values that are lists of file names. These
seven have variants that get just the file’s directory name or just
the file name within the directory. The variant variables’ names are
formed by appending ‘`D`’ or ‘`F`’, respectively. The functions
`dir` and `notdir` can be used to obtain a similar effect
(see [Functions for File Names](#File-Name-Functions)). Note,
however, that the ‘`D`’ variants all omit the trailing slash which
always appears in the output of the `dir` function. Here is a
table of the variants:

‘`$(@D)`’
:   The directory part of the file name of the target, with the trailing
    slash removed. If the value of ‘`$@`’ is `dir/foo.o` then
    ‘`$(@D)`’ is `dir`. This value is `.` if ‘`$@`’ does
    not contain a slash.

‘`$(@F)`’
:   The file-within-directory part of the file name of the target. If the
    value of ‘`$@`’ is `dir/foo.o` then ‘`$(@F)`’ is
    `foo.o`. ‘`$(@F)`’ is equivalent to ‘`$(notdir $@)`’.

‘`$(*D)`’

‘`$(*F)`’
:   The directory part and the file-within-directory
    part of the stem; `dir` and `foo` in this example.

‘`$(%D)`’

‘`$(%F)`’
:   The directory part and the file-within-directory part of the target
    archive member name. This makes sense only for archive member targets
    of the form `archive(member)` and is useful only when
    member may contain a directory name. (See [Archive Members as Targets](#Archive-Members).)

‘`$(<D)`’

‘`$(<F)`’
:   The directory part and the file-within-directory
    part of the first prerequisite.

‘`$(^D)`’

‘`$(^F)`’
:   Lists of the directory parts and the file-within-directory
    parts of all prerequisites.

‘`$(+D)`’

‘`$(+F)`’
:   Lists of the directory parts and the file-within-directory
    parts of all prerequisites, including multiple instances of duplicated
    prerequisites.

‘`$(?D)`’

‘`$(?F)`’
:   Lists of the directory parts and the file-within-directory parts of
    all prerequisites that are newer than the target.

Note that we use a special stylistic convention when we talk about these
automatic variables; we write “the value of ‘`$<`’”, rather than
“the variable `<`” as we would write for ordinary variables
such as `objects` and `CFLAGS`. We think this convention
looks more natural in this special case. Please do not assume it has a
deep significance; ‘`$<`’ refers to the variable named `<` just
as ‘`$(CFLAGS)`’ refers to the variable named `CFLAGS`.
You could just as well use ‘`$(<)`’ in place of ‘`$<`’.

---

Next: [Match-Anything Pattern Rules](#Match_002dAnything-Rules), Previous: [Automatic Variables](#Automatic-Variables), Up: [Defining and Redefining Pattern Rules](#Pattern-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 10.5.4 How Patterns Match

A target pattern is composed of a ‘`%`’ between a prefix and a suffix,
either or both of which may be empty. The pattern matches a file name only
if the file name starts with the prefix and ends with the suffix, without
overlap. The text between the prefix and the suffix is called the
*stem*. Thus, when the pattern ‘`%.o`’ matches the file name
`test.o`, the stem is ‘`test`’. The pattern rule prerequisites are
turned into actual file names by substituting the stem for the character
‘`%`’. Thus, if in the same example one of the prerequisites is written
as ‘`%.c`’, it expands to ‘`test.c`’.

When the target pattern does not contain a slash (and it usually does
not), directory names in the file names are removed from the file name
before it is compared with the target prefix and suffix. After the
comparison of the file name to the target pattern, the directory
names, along with the slash that ends them, are added on to the
prerequisite file names generated from the pattern rule’s prerequisite
patterns and the file name. The directories are ignored only for the
purpose of finding an implicit rule to use, not in the application of
that rule. Thus, ‘`e%t`’ matches the file name `src/eat`,
with ‘`src/a`’ as the stem. When prerequisites are turned into file
names, the directories from the stem are added at the front, while the
rest of the stem is substituted for the ‘`%`’. The stem
‘`src/a`’ with a prerequisite pattern ‘`c%r`’ gives the file name
`src/car`.

A pattern rule can be used to build a given file only if there is a
target pattern that matches the file name, *and* all
prerequisites in that rule either exist or can be built. The rules
you write take precedence over those that are built in. Note however,
that a rule which can be satisfied without chaining other implicit
rules (for example, one which has no prerequisites or its
prerequisites already exist or are mentioned) always takes priority
over a rule with prerequisites that must be made by chaining other
implicit rules.

It is possible that more than one pattern rule will meet these
criteria. In that case, `make` will choose the rule with the
shortest stem (that is, the pattern that matches most specifically).
If more than one pattern rule has the shortest stem, `make` will
choose the first one found in the makefile.

This algorithm results in more specific rules being preferred over
more generic ones; for example:

```
%.o: %.c
        $(CC) -c $(CFLAGS) $(CPPFLAGS) $< -o $@

%.o : %.f
        $(COMPILE.F) $(OUTPUT_OPTION) $<

lib/%.o: lib/%.c
        $(CC) -fPIC -c $(CFLAGS) $(CPPFLAGS) $< -o $@
```

Given these rules and asked to build `bar.o` where both
`bar.c` and `bar.f` exist, `make` will choose the first
rule and compile `bar.c` into `bar.o`. In the same
situation where `bar.c` does not exist, then `make` will
choose the second rule and compile `bar.f` into `bar.o`.

If `make` is asked to build `lib/bar.o` and both
`lib/bar.c` and `lib/bar.f` exist, then the third rule will
be chosen since the stem for this rule (‘`bar`’) is shorter than
the stem for the first rule (‘`lib/bar`’). If `lib/bar.c`
does not exist then the third rule is not eligible and the second rule
will be used, even though the stem is longer.

---

Next: [Canceling Implicit Rules](#Canceling-Rules), Previous: [How Patterns Match](#Pattern-Match), Up: [Defining and Redefining Pattern Rules](#Pattern-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 10.5.5 Match-Anything Pattern Rules

When a pattern rule’s target is just ‘`%`’, it matches any file name
whatever. We call these rules *match-anything* rules. They are very
useful, but it can take a lot of time for `make` to think about them,
because it must consider every such rule for each file name listed either
as a target or as a prerequisite.

Suppose the makefile mentions `foo.c`. For this target, `make`
would have to consider making it by linking an object file `foo.c.o`,
or by C compilation-and-linking in one step from `foo.c.c`, or by
Pascal compilation-and-linking from `foo.c.p`, and many other
possibilities.

We know these possibilities are ridiculous since `foo.c` is a C source
file, not an executable. If `make` did consider these possibilities,
it would ultimately reject them, because files such as `foo.c.o` and
`foo.c.p` would not exist. But these possibilities are so
numerous that `make` would run very slowly if it had to consider
them.

To gain speed, we have put various constraints on the way `make`
considers match-anything rules. There are two different constraints that
can be applied, and each time you define a match-anything rule you must
choose one or the other for that rule.

One choice is to mark the match-anything rule as *terminal* by defining
it with a double colon. When a rule is terminal, it does not apply unless
its prerequisites actually exist. Prerequisites that could be made with
other implicit rules are not good enough. In other words, no further
chaining is allowed beyond a terminal rule.

For example, the built-in implicit rules for extracting sources from RCS
and SCCS files are terminal; as a result, if the file `foo.c,v` does
not exist, `make` will not even consider trying to make it as an
intermediate file from `foo.c,v.o` or from `RCS/SCCS/s.foo.c,v`.
RCS and SCCS files are generally ultimate source files, which should not be
remade from any other files; therefore, `make` can save time by not
looking for ways to remake them.

If you do not mark the match-anything rule as terminal, then it is
non-terminal. A non-terminal match-anything rule cannot apply to a
prerequisite of an implicit rule, or to a file name that indicates a
specific type of data. A file name indicates a specific type of data
if some non-match-anything implicit rule target matches it.

For example, the file name `foo.c` matches the target for the pattern
rule ‘`%.c : %.y`’ (the rule to run Yacc). Regardless of whether this
rule is actually applicable (which happens only if there is a file
`foo.y`), the fact that its target matches is enough to prevent
consideration of any non-terminal match-anything rules for the file
`foo.c`. Thus, `make` will not even consider trying to make
`foo.c` as an executable file from `foo.c.o`, `foo.c.c`,
`foo.c.p`, etc.

The motivation for this constraint is that non-terminal match-anything
rules are used for making files containing specific types of data (such as
executable files) and a file name with a recognized suffix indicates some
other specific type of data (such as a C source file).

Special built-in dummy pattern rules are provided solely to recognize
certain file names so that non-terminal match-anything rules will not be
considered. These dummy rules have no prerequisites and no recipes, and
they are ignored for all other purposes. For example, the built-in
implicit rule

```
%.p :
```

exists to make sure that Pascal source files such as `foo.p` match a
specific target pattern and thereby prevent time from being wasted looking
for `foo.p.o` or `foo.p.c`.

Dummy pattern rules such as the one for ‘`%.p`’ are made for every
suffix listed as valid for use in suffix rules (see [Old-Fashioned Suffix Rules](#Suffix-Rules)).

---

Previous: [Match-Anything Pattern Rules](#Match_002dAnything-Rules), Up: [Defining and Redefining Pattern Rules](#Pattern-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 10.5.6 Canceling Implicit Rules

You can override a built-in implicit rule (or one you have defined
yourself) by defining a new pattern rule with the same target and
prerequisites, but a different recipe. When the new rule is defined, the
built-in one is replaced. The new rule’s position in the sequence of
implicit rules is determined by where you write the new rule.

You can cancel a built-in implicit rule by defining a pattern rule with the
same target and prerequisites, but no recipe. For example, the following
would cancel the rule that runs the assembler:

```
%.o : %.s
```

---

Next: [Old-Fashioned Suffix Rules](#Suffix-Rules), Previous: [Defining and Redefining Pattern Rules](#Pattern-Rules), Up: [Using Implicit Rules](#Implicit-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 10.6 Defining Last-Resort Default Rules

You can define a last-resort implicit rule by writing a terminal
match-anything pattern rule with no prerequisites (see [Match-Anything Pattern Rules](#Match_002dAnything-Rules)). This is just like any other pattern rule; the only thing
special about it is that it will match any target. So such a rule’s
recipe is used for all targets and prerequisites that have no recipe
of their own and for which no other implicit rule applies.

For example, when testing a makefile, you might not care if the source
files contain real data, only that they exist. Then you might do this:

```
%::
        touch $@
```

to cause all the source files needed (as prerequisites) to be created
automatically.

You can instead define a recipe to be used for targets for which there
are no rules at all, even ones which don’t specify recipes. You do
this by writing a rule for the target `.DEFAULT`. Such a rule’s
recipe is used for all prerequisites which do not appear as targets in
any explicit rule, and for which no implicit rule applies. Naturally,
there is no `.DEFAULT` rule unless you write one.

If you use `.DEFAULT` with no recipe or prerequisites:

```
.DEFAULT:
```

the recipe previously stored for `.DEFAULT` is cleared. Then
`make` acts as if you had never defined `.DEFAULT` at all.

If you do not want a target to get the recipe from a match-anything
pattern rule or `.DEFAULT`, but you also do not want any recipe
to be run for the target, you can give it an empty recipe
(see [Defining Empty Recipes](#Empty-Recipes)).

You can use a last-resort rule to override part of another makefile.
See [Overriding Part of Another Makefile](#Overriding-Makefiles).

---

Next: [Implicit Rule Search Algorithm](#Implicit-Rule-Search), Previous: [Defining Last-Resort Default Rules](#Last-Resort), Up: [Using Implicit Rules](#Implicit-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 10.7 Old-Fashioned Suffix Rules

*Suffix rules* are the old-fashioned way of defining implicit rules for
`make`. Suffix rules are obsolete because pattern rules are more
general and clearer. They are supported in GNU `make` for
compatibility with old makefiles. They come in two kinds:
*double-suffix* and *single-suffix*.

A double-suffix rule is defined by a pair of suffixes: the target
suffix and the source suffix. It matches any file whose name ends
with the target suffix. The corresponding implicit prerequisite is
made by replacing the target suffix with the source suffix in the file
name. A two-suffix rule ‘`.c.o`’ (whose target and source suffixes
are ‘`.o`’ and ‘`.c`’) is equivalent to the pattern rule
‘`%.o : %.c`’.

A single-suffix rule is defined by a single suffix, which is the source
suffix. It matches any file name, and the corresponding implicit
prerequisite name is made by appending the source suffix. A single-suffix
rule whose source suffix is ‘`.c`’ is equivalent to the pattern rule
‘`% : %.c`’.

Suffix rule definitions are recognized by comparing each rule’s target
against a defined list of known suffixes. When `make` sees a rule
whose target is a known suffix, this rule is considered a single-suffix
rule. When `make` sees a rule whose target is two known suffixes
concatenated, this rule is taken as a double-suffix rule.

For example, ‘`.c`’ and ‘`.o`’ are both on the default list of
known suffixes. Therefore, if you define a rule whose target is
‘`.c.o`’, `make` takes it to be a double-suffix rule with source
suffix ‘`.c`’ and target suffix ‘`.o`’. Here is the old-fashioned
way to define the rule for compiling a C source file:

```
.c.o:
        $(CC) -c $(CFLAGS) $(CPPFLAGS) -o $@ $<
```

Suffix rules cannot have any prerequisites of their own. If they have any,
they are treated as normal files with funny names, not as suffix rules.
Thus, the rule:

```
.c.o: foo.h
        $(CC) -c $(CFLAGS) $(CPPFLAGS) -o $@ $<
```

tells how to make the file `.c.o` from the prerequisite file
`foo.h`, and is not at all like the pattern rule:

```
%.o: %.c foo.h
        $(CC) -c $(CFLAGS) $(CPPFLAGS) -o $@ $<
```

which tells how to make ‘`.o`’ files from ‘`.c`’ files, and makes all
‘`.o`’ files using this pattern rule also depend on `foo.h`.

Suffix rules with no recipe are also meaningless. They do not remove
previous rules as do pattern rules with no recipe (see [Canceling Implicit Rules](#Canceling-Rules)). They simply enter the suffix or
pair of suffixes concatenated as a target in the data base.

The known suffixes are simply the names of the prerequisites of the special
target `.SUFFIXES`. You can add your own suffixes by writing a rule
for `.SUFFIXES` that adds more prerequisites, as in:

```
.SUFFIXES: .hack .win
```

which adds ‘`.hack`’ and ‘`.win`’ to the end of the list of suffixes.

If you wish to eliminate the default known suffixes instead of just adding
to them, write a rule for `.SUFFIXES` with no prerequisites. By
special dispensation, this eliminates all existing prerequisites of
`.SUFFIXES`. You can then write another rule to add the suffixes you
want. For example,

```
.SUFFIXES:            # Delete the default suffixes
.SUFFIXES: .c .o .h   # Define our suffix list
```

The ‘`-r`’ or ‘`--no-builtin-rules`’ flag causes the default
list of suffixes to be empty.

The variable `SUFFIXES` is defined to the default list of suffixes
before `make` reads any makefiles. You can change the list of suffixes
with a rule for the special target `.SUFFIXES`, but that does not alter
this variable.

---

Previous: [Old-Fashioned Suffix Rules](#Suffix-Rules), Up: [Using Implicit Rules](#Implicit-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 10.8 Implicit Rule Search Algorithm

Here is the procedure `make` uses for searching for an implicit rule
for a target t. This procedure is followed for each double-colon
rule with no recipe, for each target of ordinary rules none of which have
a recipe, and for each prerequisite that is not the target of any rule. It
is also followed recursively for prerequisites that come from implicit
rules, in the search for a chain of rules.

Suffix rules are not mentioned in this algorithm because suffix rules are
converted to equivalent pattern rules once the makefiles have been read in.

For an archive member target of the form
‘`archive(member)`’, the following algorithm is run
twice, first using the entire target name t, and second using
‘`(member)`’ as the target t if the first run found no
rule.

1. Split t into a directory part, called d, and the rest,
   called n. For example, if t is ‘`src/foo.o`’, then
   d is ‘`src/`’ and n is ‘`foo.o`’.
2. Make a list of all the pattern rules one of whose targets matches
   t or n. If the target pattern contains a slash, it is
   matched against t; otherwise, against n.
3. If any rule in that list is *not* a match-anything rule, or if
   t is a prerequisite of an implicit rule, then remove all
   non-terminal match-anything rules from the list.
4. Remove from the list all rules with no recipe.
5. For each pattern rule in the list:
   1. Find the stem s, which is the nonempty part of t or n
      matched by the ‘`%`’ in the target pattern.
   2. Compute the prerequisite names by substituting s for ‘`%`’; if
      the target pattern does not contain a slash, append d to
      the front of each prerequisite name.
   3. Test whether all the prerequisites exist or ought to exist. (If a
      file name is mentioned in the makefile as a target or as an explicit
      prerequisite of target T, then we say it ought to exist.)

      If all prerequisites exist or ought to exist, or there are no prerequisites,
      then this rule applies.
6. If no pattern rule has been found so far, try harder.
   For each pattern rule in the list:
   1. If the rule is terminal, ignore it and go on to the next rule.
   2. Compute the prerequisite names as before.
   3. Test whether all the prerequisites exist or ought to exist.
   4. For each prerequisite that does not exist, follow this algorithm
      recursively to see if the prerequisite can be made by an implicit
      rule.
   5. If all prerequisites exist, ought to exist, or can be
      made by implicit rules, then this rule applies.
7. If no pattern rule has been found then try step 5 and step 6 again with a
   modified definition of “ought to exist”: if a filename is mentioned as a
   target or as an explicit prerequisite of *any* target, then it ought to
   exist. This check is only present for backward-compatibility with older
   versions of GNU Make: we don’t recommend relying on it.
8. If no implicit rule applies, the rule for `.DEFAULT`, if any,
   applies. In that case, give t the same recipe that
   `.DEFAULT` has. Otherwise, there is no recipe for t.

Once a rule that applies has been found, for each target pattern of
the rule other than the one that matched t or n, the
‘`%`’ in the pattern is replaced with s and the resultant
file name is stored until the recipe to remake the target file t
is executed. After the recipe is executed, each of these stored file
names are entered into the data base and marked as having been updated
and having the same update status as the file t.

When the recipe of a pattern rule is executed for t, the
automatic variables are set corresponding to the target and
prerequisites. See [Automatic Variables](#Automatic-Variables).

---

Next: [Extending GNU `make`](#Extending-make), Previous: [Using Implicit Rules](#Implicit-Rules), Up: [GNU `make`](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]## 11 Using `make` to Update Archive Files

*Archive files* are files containing named sub-files called
*members*; they are maintained with the program `ar` and their
main use is as subroutine libraries for linking.

* [Archive Members as Targets](#Archive-Members)
* [Implicit Rule for Archive Member Targets](#Archive-Update)
* [Dangers When Using Archives](#Archive-Pitfalls)
* [Suffix Rules for Archive Files](#Archive-Suffix-Rules)

---

Next: [Implicit Rule for Archive Member Targets](#Archive-Update), Previous: [Using `make` to Update Archive Files](#Archives), Up: [Using `make` to Update Archive Files](#Archives)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 11.1 Archive Members as Targets

An individual member of an archive file can be used as a target or
prerequisite in `make`. You specify the member named member in
archive file archive as follows:

```
archive(member)
```

This construct is available only in targets and prerequisites, not in
recipes! Most programs that you might use in recipes do not support
this syntax and cannot act directly on archive members. Only
`ar` and other programs specifically designed to operate on
archives can do so. Therefore, valid recipes to update an archive
member target probably must use `ar`. For example, this rule
says to create a member `hack.o` in archive `foolib` by
copying the file `hack.o`:

```
foolib(hack.o) : hack.o
        ar cr foolib hack.o
```

In fact, nearly all archive member targets are updated in just this way
and there is an implicit rule to do it for you. **Please note:** The
‘`c`’ flag to `ar` is required if the archive file does not
already exist.

To specify several members in the same archive, you can write all the
member names together between the parentheses. For example:

```
foolib(hack.o kludge.o)
```

is equivalent to:

```
foolib(hack.o) foolib(kludge.o)
```

You can also use shell-style wildcards in an archive member reference.
See [Using Wildcard Characters in File Names](#Wildcards). For
example, ‘`foolib(*.o)`’ expands to all existing members of the
`foolib` archive whose names end in ‘`.o`’; perhaps
‘`foolib(hack.o) foolib(kludge.o)`’.

---

Next: [Dangers When Using Archives](#Archive-Pitfalls), Previous: [Archive Members as Targets](#Archive-Members), Up: [Using `make` to Update Archive Files](#Archives)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 11.2 Implicit Rule for Archive Member Targets

Recall that a target that looks like `a(m)` stands for the
member named m in the archive file a.

When `make` looks for an implicit rule for such a target, as a special
feature it considers implicit rules that match `(m)`, as well as
those that match the actual target `a(m)`.

This causes one special rule whose target is `(%)` to match. This
rule updates the target `a(m)` by copying the file m
into the archive. For example, it will update the archive member target
`foo.a(bar.o)` by copying the *file* `bar.o` into the
archive `foo.a` as a *member* named `bar.o`.

When this rule is chained with others, the result is very powerful.
Thus, ‘`make "foo.a(bar.o)"`’ (the quotes are needed to protect the
‘`(`’ and ‘`)`’ from being interpreted specially by the shell) in
the presence of a file `bar.c` is enough to cause the following
recipe to be run, even without a makefile:

```
cc -c bar.c -o bar.o
ar r foo.a bar.o
rm -f bar.o
```

Here `make` has envisioned the file `bar.o` as an intermediate
file. See [Chains of Implicit Rules](#Chained-Rules).

Implicit rules such as this one are written using the automatic variable
‘`$%`’. See [Automatic Variables](#Automatic-Variables).

An archive member name in an archive cannot contain a directory name, but
it may be useful in a makefile to pretend that it does. If you write an
archive member target `foo.a(dir/file.o)`, `make` will perform
automatic updating with this recipe:

```
ar r foo.a dir/file.o
```

which has the effect of copying the file `dir/file.o` into a member
named `file.o`. In connection with such usage, the automatic variables
`%D` and `%F` may be useful.

* [Updating Archive Symbol Directories](#Archive-Symbols)

---

Previous: [Implicit Rule for Archive Member Targets](#Archive-Update), Up: [Implicit Rule for Archive Member Targets](#Archive-Update)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 11.2.1 Updating Archive Symbol Directories

An archive file that is used as a library usually contains a special member
named `__.SYMDEF` that contains a directory of the external symbol
names defined by all the other members. After you update any other
members, you need to update `__.SYMDEF` so that it will summarize the
other members properly. This is done by running the `ranlib` program:

```
ranlib archivefile
```

Normally you would put this command in the rule for the archive file,
and make all the members of the archive file prerequisites of that rule.
For example,

```
libfoo.a: libfoo.a(x.o y.o …)
        ranlib libfoo.a
```

The effect of this is to update archive members `x.o`, `y.o`,
etc., and then update the symbol directory member `__.SYMDEF` by
running `ranlib`. The rules for updating the members are not shown
here; most likely you can omit them and use the implicit rule which copies
files into the archive, as described in the preceding section.

This is not necessary when using the GNU `ar` program, which
updates the `__.SYMDEF` member automatically.

---

Next: [Suffix Rules for Archive Files](#Archive-Suffix-Rules), Previous: [Implicit Rule for Archive Member Targets](#Archive-Update), Up: [Using `make` to Update Archive Files](#Archives)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 11.3 Dangers When Using Archives

The built-in rules for updating archives are incompatible with parallel
builds. These rules (required by the POSIX standard) add each object file
into the archive as it’s compiled. When parallel builds are enabled this
allows multiple `ar` commands to be updating the same archive
simultaneously, which is not supported.

If you want to use parallel builds with archives you can override the default
rules by adding these lines to your makefile:

```
(%) : % ;
%.a : ; $(AR) $(ARFLAGS) $@ $?
```

The first line changes the rule that updates an individual object in the
archive to do nothing, and the second line changes the default rule for
building an archive to update all the outdated objects (`$?`) in one
command.

Of course you will still need to declare the prerequisites of your library
using the archive syntax:

```
libfoo.a: libfoo.a(x.o y.o …)
```

If you prefer to write an explicit rule you can use:

```
libfoo.a: libfoo.a(x.o y.o …)
        $(AR) $(ARFLAGS) $@ $?
```

---

Previous: [Dangers When Using Archives](#Archive-Pitfalls), Up: [Using `make` to Update Archive Files](#Archives)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 11.4 Suffix Rules for Archive Files

You can write a special kind of suffix rule for dealing with archive
files. See [Old-Fashioned Suffix Rules](#Suffix-Rules), for a full explanation of suffix rules.
Archive suffix rules are obsolete in GNU `make`, because pattern
rules for archives are a more general mechanism (see [Implicit Rule for Archive Member Targets](#Archive-Update)). But they are retained for compatibility with other
`make`s.

To write a suffix rule for archives, you simply write a suffix rule
using the target suffix ‘`.a`’ (the usual suffix for archive files).
For example, here is the old-fashioned suffix rule to update a library
archive from C source files:

```
.c.a:
        $(CC) $(CFLAGS) $(CPPFLAGS) -c $< -o $*.o
        $(AR) r $@ $*.o
        $(RM) $*.o
```

This works just as if you had written the pattern rule:

```
(%.o): %.c
        $(CC) $(CFLAGS) $(CPPFLAGS) -c $< -o $*.o
        $(AR) r $@ $*.o
        $(RM) $*.o
```

In fact, this is just what `make` does when it sees a suffix rule
with ‘`.a`’ as the target suffix. Any double-suffix rule
‘`.x.a`’ is converted to a pattern rule with the target
pattern ‘`(%.o)`’ and a prerequisite pattern of ‘`%.x`’.

Since you might want to use ‘`.a`’ as the suffix for some other kind
of file, `make` also converts archive suffix rules to pattern rules
in the normal way (see [Old-Fashioned Suffix Rules](#Suffix-Rules)). Thus a double-suffix rule
‘`.x.a`’ produces two pattern rules: ‘`(%.o):
%.x`’ and ‘`%.a: %.x`’.

---

Next: [Integrating GNU `make`](#Integrating-make), Previous: [Using `make` to Update Archive Files](#Archives), Up: [GNU `make`](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]## 12 Extending GNU `make`

GNU `make` provides many advanced capabilities, including many
useful functions. However, it does not contain a complete programming
language and so it has limitations. Sometimes these limitations can be
overcome through use of the `shell` function to invoke a separate
program, although this can be inefficient.

In cases where the built-in capabilities of GNU `make` are
insufficient to your requirements there are two options for extending
`make`. On systems where it’s provided, you can utilize GNU
Guile as an embedded scripting language (see [GNU
Guile Integration](#Guile-Integration)). On systems which support dynamically loadable
objects, you can write your own extension in any language (which can
be compiled into such an object) and load it to provide extended
capabilities (see [The `load` Directive](#load-Directive)).

* [GNU Guile Integration](#Guile-Integration)
* [Loading Dynamic Objects](#Loading-Objects)

---

Next: [Loading Dynamic Objects](#Loading-Objects), Previous: [Extending GNU `make`](#Extending-make), Up: [Extending GNU `make`](#Extending-make)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 12.1 GNU Guile Integration

GNU `make` may be built with support for GNU Guile as an embedded
extension language. Guile implements the Scheme language. A review
of GNU Guile and the Scheme language and its features is beyond the
scope of this manual: see the documentation for GNU Guile and Scheme.

You can determine if `make` contains support for Guile by
examining the `.FEATURES` variable; it will contain the word
guile if Guile support is available.

The Guile integration provides one new `make` function: `guile`.
The `guile` function takes one argument which is first expanded
by `make` in the normal fashion, then passed to the GNU Guile
evaluator. The result of the evaluator is converted into a string and
used as the expansion of the `guile` function in the makefile.

In addition, GNU `make` exposes Guile procedures for use in Guile
scripts.

* [Conversion of Guile Types](#Guile-Types)
* [Interfaces from Guile to `make`](#Guile-Interface)
* [Example Using Guile in `make`](#Guile-Example)

---

Next: [Interfaces from Guile to `make`](#Guile-Interface), Previous: [GNU Guile Integration](#Guile-Integration), Up: [GNU Guile Integration](#Guile-Integration)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 12.1.1 Conversion of Guile Types

There is only one “data type” in `make`: a string. GNU Guile,
on the other hand, provides a rich variety of different data types.
An important aspect of the interface between `make` and GNU Guile
is the conversion of Guile data types into `make` strings.

This conversion is relevant in two places: when a makefile invokes the
`guile` function to evaluate a Guile expression, the result of
that evaluation must be converted into a make string so it can be
further evaluated by `make`. And secondly, when a Guile script
invokes one of the procedures exported by `make` the argument
provided to the procedure must be converted into a string.

The conversion of Guile types into `make` strings is as below:

`#f`
:   False is converted into the empty string: in `make` conditionals
    the empty string is considered false.

`#t`
:   True is converted to the string ‘`#t`’: in `make` conditionals
    any non-empty string is considered true.

`symbol`

`number`
:   A symbol or number is converted into the string representation of that
    symbol or number.

`character`
:   A printable character is converted to the same character.

`string`
:   A string containing only printable characters is converted to the same
    string.

`list`
:   A list is converted recursively according to the above rules. This
    implies that any structured list will be flattened (that is, a result
    of ‘`'(a b (c d) e)`’ will be converted to the `make` string
    ‘`a b c d e`’).

`other`
:   Any other Guile type results in an error. In future versions of
    `make`, other Guile types may be converted.

The translation of ‘`#f`’ (to the empty string) and ‘`#t`’ (to
the non-empty string ‘`#t`’) is designed to allow you to use Guile
boolean results directly as `make` boolean conditions. For
example:

```
$(if $(guile (access? "myfile" R_OK)),$(info myfile exists))
```

As a consequence of these conversion rules you must consider the
result of your Guile script, as that result will be converted into a
string and parsed by `make`. If there is no natural result for
the script (that is, the script exists solely for its side-effects),
you should add ‘`#f`’ as the final expression in order to avoid
syntax errors in your makefile.

---

Next: [Example Using Guile in `make`](#Guile-Example), Previous: [Conversion of Guile Types](#Guile-Types), Up: [GNU Guile Integration](#Guile-Integration)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 12.1.2 Interfaces from Guile to `make`

In addition to the `guile` function available in makefiles,
`make` exposes some procedures for use in your Guile scripts. At
startup `make` creates a new Guile module, `gnu make`, and
exports these procedures as public interfaces from that module:

`gmk-expand` [¶](#index-gmk_002dexpand)
:   This procedure takes a single argument which is converted into a
    string. The string is expanded by `make` using normal
    `make` expansion rules. The result of the expansion is converted
    into a Guile string and provided as the result of the procedure.

`gmk-eval` [¶](#index-gmk_002deval)
:   This procedure takes a single argument which is converted into a
    string. The string is evaluated by `make` as if it were a
    makefile. This is the same capability available via the `eval`
    function (see [The `eval` Function](#Eval-Function)). The result of the `gmk-eval`
    procedure is always the empty string.

    Note that `gmk-eval` is not quite the same as using
    `gmk-expand` with the `eval` function: in the latter case
    the evaluated string will be expanded *twice*; first by
    `gmk-expand`, then again by the `eval` function.

---

Previous: [Interfaces from Guile to `make`](#Guile-Interface), Up: [GNU Guile Integration](#Guile-Integration)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 12.1.3 Example Using Guile in `make`

Here is a very simple example using GNU Guile to manage writing to a
file. These Guile procedures simply open a file, allow writing to the
file (one string per line), and close the file. Note that because we
cannot store complex values such as Guile ports in `make`
variables, we’ll keep the port as a global variable in the Guile
interpreter.

You can create Guile functions easily using `define`/`endef`
to create a Guile script, then use the `guile` function to
internalize it:

```
define GUILEIO
;; A simple Guile IO library for GNU Make

(define MKPORT #f)

(define (mkopen name mode)
  (set! MKPORT (open-file name mode))
  #f)

(define (mkwrite s)
  (display s MKPORT)
  (newline MKPORT)
  #f)

(define (mkclose)
  (close-port MKPORT)
  #f)

#f
endef

# Internalize the Guile IO functions
$(guile $(GUILEIO))
```

If you have a significant amount of Guile support code, you might
consider keeping it in a different file (e.g., `guileio.scm`) and
then loading it in your makefile using the `guile` function:

```
$(guile (load "guileio.scm"))
```

An advantage to this method is that when editing `guileio.scm`,
your editor will understand that this file contains Scheme syntax
rather than makefile syntax.

Now you can use these Guile functions to create files. Suppose you
need to operate on a very large list, which cannot fit on the command
line, but the utility you’re using accepts the list as input as well:

```
prog: $(PREREQS)
        @$(guile (mkopen "tmp.out" "w")) \
         $(foreach X,$^,$(guile (mkwrite "$(X)"))) \
         $(guile (mkclose))
        $(LINK) < tmp.out
```

A more comprehensive suite of file manipulation procedures is possible
of course. You could, for example, maintain multiple output files at
the same time by choosing a symbol for each one and using it as the
key to a hash table, where the value is a port, then returning the
symbol to be stored in a `make` variable.

---

Previous: [GNU Guile Integration](#Guile-Integration), Up: [Extending GNU `make`](#Extending-make)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 12.2 Loading Dynamic Objects

|  |
| --- |
| **Warning:** The `load` directive and extension capability is considered a “technology preview” in this release of GNU Make. We encourage you to experiment with this feature and we appreciate any feedback on it. However we cannot guarantee to maintain backward-compatibility in the next release. Consider using GNU Guile instead for extending GNU Make (see [The `guile` Function](#Guile-Function)). |

Many operating systems provide a facility for dynamically loading
compiled objects. If your system provides this facility, GNU
`make` can make use of it to load dynamic objects at runtime,
providing new capabilities which may then be invoked by your makefile.

The `load` directive is used to load a dynamic object. Once the
object is loaded, a “setup” function will be invoked to allow the
object to initialize itself and register new facilities with GNU
`make`. A dynamic object might include new `make` functions,
for example, and the “setup” function would register them with GNU
`make`’s function handling system.

* [The `load` Directive](#load-Directive)
* [How Loaded Objects Are Remade](#Remaking-Loaded-Objects)
* [Loaded Object Interface](#Loaded-Object-API)
* [Example Loaded Object](#Loaded-Object-Example)

---

Next: [How Loaded Objects Are Remade](#Remaking-Loaded-Objects), Previous: [Loading Dynamic Objects](#Loading-Objects), Up: [Loading Dynamic Objects](#Loading-Objects)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 12.2.1 The `load` Directive

Objects are loaded into GNU `make` by placing the `load`
directive into your makefile. The syntax of the `load` directive
is as follows:

```
load object-file …
```

or:

```
load object-file(symbol-name) …
```

The file object-file is dynamically loaded by GNU `make`.
If object-file does not include a directory path then it is
first looked for in the current directory. If it is not found there,
or a directory path is included, then system-specific paths will be
searched. If the load fails for any reason, `make` will print a
message and exit.

If the load succeeds `make` will invoke an initializing function.

If symbol-name is provided, it will be used as the name of the
initializing function.

If no symbol-name is provided, the initializing function name is
created by taking the base file name of object-file, up to the
first character which is not a valid symbol name character
(alphanumerics and underscores are valid symbol name characters). To
this prefix will be appended the suffix `_gmk_setup`.

More than one object file may be loaded with a single `load`
directive, and both forms of `load` arguments may be used in the
same directive.

The initializing function will be provided the file name and line
number of the invocation of the `load` operation. It should
return a value of type `int`, which must be `0` on failure
and non-`0` on success. If the return value is `-1`, then
GNU Make will *not* attempt to rebuild the object file
(see [How Loaded Objects Are Remade](#Remaking-Loaded-Objects)).

For example:

```
load ../mk_funcs.so
```

will load the dynamic object `../mk_funcs.so`. After the object
is loaded, `make` will invoke the function (assumed to be defined
by the shared object) `mk_funcs_gmk_setup`.

On the other hand:

```
load ../mk_funcs.so(init_mk_func)
```

will load the dynamic object `../mk_funcs.so`. After the object
is loaded, `make` will invoke the function `init_mk_func`.

Regardless of how many times an object file appears in a `load`
directive, it will only be loaded (and its setup function will only
be invoked) once.

After an object has been successfully loaded, its file name is
appended to the `.LOADED` variable.

If you would prefer that failure to load a dynamic object not be
reported as an error, you can use the `-load` directive instead
of `load`. GNU `make` will not fail and no message will be
generated if an object fails to load. The failed object is not added
to the `.LOADED` variable, which can then be consulted to
determine if the load was successful.

---

Next: [Loaded Object Interface](#Loaded-Object-API), Previous: [The `load` Directive](#load-Directive), Up: [Loading Dynamic Objects](#Loading-Objects)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 12.2.2 How Loaded Objects Are Remade

Loaded objects undergo the same re-make procedure as makefiles
(see [How Makefiles Are Remade](#Remaking-Makefiles)). If any
loaded object is recreated, then `make` will start from scratch
and re-read all the makefiles, and reload the object files again. It
is not necessary for the loaded object to do anything special to
support this.

It’s up to the makefile author to provide the rules needed for
rebuilding the loaded object.

---

Next: [Example Loaded Object](#Loaded-Object-Example), Previous: [How Loaded Objects Are Remade](#Remaking-Loaded-Objects), Up: [Loading Dynamic Objects](#Loading-Objects)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 12.2.3 Loaded Object Interface

|  |
| --- |
| **Warning:** For this feature to be useful your extensions will need to invoke various functions internal to GNU `make`. The programming interfaces provided in this release should not be considered stable: functions may be added, removed, or change calling signatures or implementations in future versions of GNU `make`. |

To be useful, loaded objects must be able to interact with GNU
`make`. This interaction includes both interfaces the loaded
object provides to makefiles and also interfaces `make` provides
to the loaded object to manipulate `make`’s operation.

The interface between loaded objects and `make` is defined by the
`gnumake.h` C header file. All loaded objects written in C
should include this header file. Any loaded object not written in C
will need to implement the interface defined in this header file.

Typically, a loaded object will register one or more new GNU
`make` functions using the `gmk_add_function` routine from
within its setup function. The implementations of these `make`
functions may make use of the `gmk_expand` and `gmk_eval`
routines to perform their tasks, then optionally return a string as
the result of the function expansion.

#### Loaded Object Licensing

Every dynamic extension should define the global symbol
`plugin_is_GPL_compatible` to assert that it has been licensed
under a GPL-compatible license. If this symbol does not exist,
`make` emits a fatal error and exits when it tries to load your
extension.

The declared type of the symbol should be `int`. It does not need
to be in any allocated section, though. The code merely asserts that
the symbol exists in the global scope. Something like this is enough:

```
int plugin_is_GPL_compatible;
```

#### Data Structures

`gmk_floc`
:   This structure represents a filename/location pair. It is provided
    when defining items, so GNU `make` can inform the user later
    where the definition occurred if necessary.

#### Registering Functions

There is currently one way for makefiles to invoke operations provided
by the loaded object: through the `make` function call
interface. A loaded object can register one or more new functions
which may then be invoked from within the makefile in the same way as
any other function.

Use `gmk_add_function` to create a new `make` function. Its
arguments are as follows:

`name`
:   The function name. This is what the makefile should use to invoke the
    function. The name must be between 1 and 255 characters long and it
    may only contain alphanumeric, period (‘`.`’), dash (‘`-`’), and
    underscore (‘`_`’) characters. It may not begin with a period.

`func_ptr`
:   A pointer to a function that `make` will invoke when it expands
    the function in a makefile. This function must be defined by the
    loaded object.

`min_args`
:   The minimum number of arguments the function will accept. Must be
    between 0 and 255. GNU `make` will check this and fail before
    invoking `func_ptr` if the function was invoked with too few
    arguments.

`max_args`
:   The maximum number of arguments the function will accept. Must be
    between 0 and 255. GNU `make` will check this and fail before
    invoking `func_ptr` if the function was invoked with too many
    arguments. If the value is 0, then any number of arguments is
    accepted. If the value is greater than 0, then it must be greater
    than or equal to `min_args`.

`flags`
:   Flags that specify how this function will operate; the desired flags
    should be OR’d together. If the `GMK_FUNC_NOEXPAND` flag is
    given then the function arguments will not be expanded before the
    function is called; otherwise they will be expanded first.

#### Registered Function Interface

A function registered with `make` must match the
`gmk_func_ptr` type. It will be invoked with three parameters:
`name` (the name of the function), `argc` (the number of
arguments to the function), and `argv` (an array of pointers to
arguments to the function). The last pointer (that is,
`argv[argc]`) will be null (`0`).

The return value of the function is the result of expanding the
function. If the function expands to nothing the return value may be
null. Otherwise, it must be a pointer to a string created with
`gmk_alloc`. Once the function returns, `make` owns this
string and will free it when appropriate; it cannot be accessed by the
loaded object.

#### GNU `make` Facilities

There are some facilities exported by GNU `make` for use by
loaded objects. Typically these would be run from within the
setup function and/or the functions registered via
`gmk_add_function`, to retrieve or modify the data `make`
works with.

`gmk_expand` [¶](#index-gmk_005fexpand)
:   This function takes a string and expands it using `make`
    expansion rules. The result of the expansion is returned in a
    nil-terminated string buffer. The caller is responsible for calling
    `gmk_free` with a pointer to the returned buffer when done.

`gmk_eval` [¶](#index-gmk_005feval)
:   This function takes a buffer and evaluates it as a segment of makefile
    syntax. This function can be used to define new variables, new rules,
    etc. It is equivalent to using the `eval` `make` function.

Note that there is a difference between `gmk_eval` and calling
`gmk_expand` with a string using the `eval` function: in
the latter case the string will be expanded *twice*; once by
`gmk_expand` and then again by the `eval` function. Using
`gmk_eval` the buffer is only expanded once, at most (as it’s
read by the `make` parser).

#### Memory Management

Some systems allow for different memory management schemes. Thus you
should never pass memory that you’ve allocated directly to any
`make` function, nor should you attempt to directly free any
memory returned to you by any `make` function. Instead, use the
`gmk_alloc` and `gmk_free` functions.

In particular, the string returned to `make` by a function
registered using `gmk_add_function` *must* be allocated
using `gmk_alloc`, and the string returned from the `make`
`gmk_expand` function *must* be freed (when no longer
needed) using `gmk_free`.

`gmk_alloc` [¶](#index-gmk_005falloc)
:   Return a pointer to a newly-allocated buffer. This function will
    always return a valid pointer; if not enough memory is available
    `make` will exit. `gmk_alloc` does not initialize allocated memory.

`gmk_free` [¶](#index-gmk_005ffree)
:   Free a buffer returned to you by `make`. Once the
    `gmk_free` function returns the string will no longer be valid.
    If NULL is passed to `gmk_free`, no operation is performed.

---

Previous: [Loaded Object Interface](#Loaded-Object-API), Up: [Loading Dynamic Objects](#Loading-Objects)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 12.2.4 Example Loaded Object

Let’s suppose we wanted to write a new GNU `make` function that
would create a temporary file and return its name. We would like our
function to take a prefix as an argument. First we can write the
function in a file `mk_temp.c`:

```
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <errno.h>

#include <gnumake.h>

int plugin_is_GPL_compatible;

char *
gen_tmpfile(const char *nm, int argc, char **argv)
{
  int fd;

  /* Compute the size of the filename and allocate space for it.  */
  int len = strlen (argv[0]) + 6 + 1;
  char *buf = gmk_alloc (len);

  strcpy (buf, argv[0]);
  strcat (buf, "XXXXXX");

  fd = mkstemp(buf);
  if (fd >= 0)
    {
      /* Don't leak the file descriptor.  */
      close (fd);
      return buf;
    }

  /* Failure.  */
  fprintf (stderr, "mkstemp(%s) failed: %s\n", buf, strerror (errno));
  gmk_free (buf);
  return NULL;
}

int
mk_temp_gmk_setup (const gmk_floc *floc)
{
  printf ("mk_temp plugin loaded from %s:%lu\n", floc->filenm, floc->lineno);
  /* Register the function with make name "mk-temp".  */
  gmk_add_function ("mk-temp", gen_tmpfile, 1, 1, 1);
  return 1;
}
```

Next, we will write a `Makefile` that can build this shared object, load
it, and use it:

```
all:
        @echo Temporary file: $(mk-temp tmpfile.)

load mk_temp.so

mk_temp.so: mk_temp.c
        $(CC) -shared -fPIC -o $@ $<
```

On MS-Windows, due to peculiarities of how shared objects are
produced, the compiler needs to scan the *import library* produced
when building `make`, typically called
`libgnumake-version.dll.a`, where version is the
version of the load object API. So the recipe to produce a shared
object will look on Windows like this (assuming the API version is 1):

```
mk_temp.dll: mk_temp.c
        $(CC) -shared -o $@ $< -lgnumake-1
```

Now when you run `make` you’ll see something like:

```
$ make
mk_temp plugin loaded from Makefile:4
cc -shared -fPIC -o mk_temp.so mk_temp.c
Temporary filename: tmpfile.A7JEwd
```

---

Next: [Features of GNU `make`](#Features), Previous: [Extending GNU `make`](#Extending-make), Up: [GNU `make`](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]## 13 Integrating GNU `make`

GNU `make` is often one component in a larger system of tools,
including integrated development environments, compiler toolchains,
and others. The role of `make` is to start commands and
determine whether they succeeded or not: no special integration is
needed to accomplish that. However, sometimes it is convenient to
bind `make` more tightly with other parts of the system, both
higher-level (tools that invoke `make`) and lower-level (tools
that `make` invokes).

* [Sharing Job Slots with GNU `make`](#Job-Slots)
* [Synchronized Terminal Output](#Terminal-Output)

---

Next: [Synchronized Terminal Output](#Terminal-Output), Previous: [Integrating GNU `make`](#Integrating-make), Up: [Integrating GNU `make`](#Integrating-make)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

### 13.1 Sharing Job Slots with GNU `make`

GNU `make` has the ability to run multiple recipes in parallel
(see [Parallel Execution](#Parallel)) and to cap the total number of
parallel jobs even across recursive invocations of `make`
(see [Communicating Options to a
Sub-`make`](#Options_002fRecursion)). Tools that `make` invokes which are also able
to run multiple operations in parallel, either using multiple threads
or multiple processes, can be enhanced to participate in GNU
`make`’s job management facility to ensure that the total number
of active threads/processes running on the system does not exceed the
maximum number of slots provided to GNU `make`.

GNU `make` uses a method called the “jobserver” to control the
number of active jobs across recursive invocations. The actual
implementation of the jobserver varies across different operating
systems, but some fundamental aspects are always true.

First, `make` will provide information necessary for accessing the
jobserver through the environment to its children, in the `MAKEFLAGS`
environment variable. Tools which want to participate in the jobserver
protocol will need to parse this environment variable and find the word
starting with `--jobserver-auth=`. The value of this option will
describe how to communicate with the jobserver. The interpretation of this
value is described in the sections below.

Be aware that the `MAKEFLAGS` variable may contain multiple instances of
the `--jobserver-auth=` option. Only the *last* instance is
relevant.

Second, every command `make` starts has one implicit job slot
reserved for it before it starts. Any tool which wants to participate
in the jobserver protocol should assume it can always run one job
without having to contact the jobserver at all.

Finally, it’s critical that tools that participate in the jobserver
protocol return the exact number of slots they obtained from the
jobserver back to the jobserver before they exit, even under error
conditions. Remember that the implicit job slot should **not**
be returned to the jobserver! Returning too few slots means that
those slots will be lost for the rest of the build process; returning
too many slots means that extra slots will be available. The
top-level `make` command will print an error message at the end
of the build if it detects an incorrect number of slots available in
the jobserver.

As an example, suppose you are implementing a linker which provides
for multithreaded operation. You would like to enhance the linker so
that if it is invoked by GNU `make` it can participate in the
jobserver protocol to control how many threads are used during link.
First you will need to modify the linker to determine if the
`MAKEFLAGS` environment variable is set. Next you will need to
parse the value of that variable to determine if the jobserver is
available, and how to access it. If it is available then you can
access it to obtain job slots controlling how much parallelism your
tool can use. Once done your tool must return those job slots back to
the jobserver.

* [POSIX Jobserver Interaction](#POSIX-Jobserver)
* [Windows Jobserver Interaction](#Windows-Jobserver)

---

Next: [Windows Jobserver Interaction](#Windows-Jobserver), Previous: [Sharing Job Slots with GNU `make`](#Job-Slots), Up: [Sharing Job Slots with GNU `make`](#Job-Slots)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 13.1.1 POSIX Jobserver Interaction

On POSIX systems the jobserver is implemented in one of two ways: on systems
that support it, GNU `make` will create a named pipe and use that for the
jobserver. In this case the auth option will have the form
`--jobserver-auth=fifo:PATH` where ‘`PATH`’ is the pathname of the
named pipe. To access the jobserver you should open the named pipe path and
read/write to it as described below.

If the system doesn’t support named pipes, or if the user provided the
`--jobserver-style` option and specified ‘`pipe`’, then the jobserver
will be implemented as a simple UNIX pipe. In this case the auth option will
have the form `--jobserver-auth=R,W` where ‘`R`’ and ‘`W`’ are
non-negative integers representing file descriptors: ‘`R`’ is the read file
descriptor and ‘`W`’ is the write file descriptor. If either or both of
these file descriptors are negative, it means the jobserver is disabled for
this process.

When using a simple pipe, only command lines that `make` understands to
be recursive invocations of `make` (see [How the
`MAKE` Variable Works](#MAKE-Variable)) will have access to the jobserver. When writing
makefiles you must be sure to mark the command as recursive (most commonly by
prefixing the command line with the `+` indicator (see [Recursive Use of `make`](#Recursion)). Note that the read side of the jobserver
pipe is set to “blocking” mode. This should not be changed.

In both implementations of the jobserver, the pipe will be pre-loaded with one
single-character token for each available job. To obtain an extra slot you
must read a single character from the jobserver; to release a slot you must
write a single character back into the jobserver.

It’s important that when you release the job slot, you write back the same
character you read. Don’t assume that all tokens are the same character;
different characters may have different meanings to GNU `make`. The
order is not important, since `make` has no idea in what order jobs will
complete anyway.

There are various error conditions you must consider to ensure your
implementation is robust:

* If you have a command-line argument controlling the parallel operation of your
  tool, consider whether your tool should detect situations where both the
  jobserver and the command-line argument are specified, and how it should
  react.
* If your tool does not recognize the format of the `--jobserver-auth`
  string, it should assume the jobserver is using a different style and it
  cannot connect.
* If your tool determines that the `--jobserver-auth` option references a
  simple pipe but that the file descriptors specified are closed, this means
  that the calling `make` process did not think that your tool was a
  recursive `make` invocation (e.g., the command line was not prefixed with
  a `+` character). You should notify your users of this situation.
* Your tool should be sure to write back the tokens it read, even under error
  conditions. This includes not only errors in your tool but also outside
  influences such as interrupts (`SIGINT`), etc. You may want to install
  signal handlers to manage this write-back.
* Your tool may also examine the first word of the `MAKEFLAGS` variable and
  look for the character `n`. If this character is present then
  `make` was invoked with the ‘`-n`’ option and your tool may want to
  stop without performing any operations.

---

Previous: [POSIX Jobserver Interaction](#POSIX-Jobserver), Up: [Sharing Job Slots with GNU `make`](#Job-Slots)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]

#### 13.1.2 Windows Jobserver Interaction

On Windows systems the jobserver is implemented as a named semaphore.
The semaphore will be set with an initial count equal to the number of
available slots; to obtain a slot you must wait on the semaphore (with
or without a timeout). To release a slot, release the semaphore.

To access the semaphore you must parse the `MAKEFLAGS` variable and
look for the argument string `--jobserver-auth=NAME` where
‘`NAME`’ is the name of the named semaphore. Use this name with
`OpenSemaphore` to create a handle to the semaphore.

The only valid style for `--jobserver-style` is ‘`sem`’.

There are various error conditions you must consider to ensure your
implementation is robust:

* Usually you will have a command-line argument controlling the parallel
  operation of your tool. Consider whether your tool should detect
  situations where both the jobserver and the command-line argument are
  specified, and how it should react.
* Your tool should be sure to release the semaphore for the tokens it
  read, even under error conditions. This includes not only errors in
  your tool but also outside influences such as interrupts
  (`SIGINT`), etc. You may want to install signal handlers to
  manage this write-back.

---

Previous: [Sharing Job Slots with GNU `make`](#Job-Slots), Up: [Integrating GNU `make`](#Integrating-make)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-Index "Index")]
