### 7.1 Pattern Elements [¶](#Pattern-Elements)

Patterns in `awk` control the execution of rules—a rule is
executed when its pattern matches the current input record.
The following is a summary of the types of `awk` patterns:

`/regular expression/`
:   A regular expression. It matches when the text of the
    input record fits the regular expression.
    (See [Regular Expressions](#Regexp).)

`expression`
:   A single expression. It matches when its value
    is nonzero (if a number) or non-null (if a string).
    (See [Expressions as Patterns](#Expression-Patterns).)

`begpat, endpat`
:   A pair of patterns separated by a comma, specifying a *range* of records.
    The range includes both the initial record that matches begpat and
    the final record that matches endpat.
    (See [Specifying Record Ranges with Patterns](#Ranges).)

`BEGIN`

`END`
:   Special patterns for you to supply startup or cleanup actions for your
    `awk` program.
    (See [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND).)

`BEGINFILE`

`ENDFILE`
:   Special patterns for you to supply startup or cleanup actions to be
    done on a per-file basis.
    (See [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE).)

`empty`
:   The empty pattern matches every input record.
    (See [The Empty Pattern](#Empty).)

* [Regular Expressions as Patterns](#Regexp-Patterns)
* [Expressions as Patterns](#Expression-Patterns)
* [Specifying Record Ranges with Patterns](#Ranges)
* [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)
* [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE)
* [The Empty Pattern](#Empty)

---

Next: [Expressions as Patterns](#Expression-Patterns), Up: [Pattern Elements](#Pattern-Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.1 Regular Expressions as Patterns [¶](#Regular-Expressions-as-Patterns)

Regular expressions are one of the first kinds of patterns presented
in this book.
This kind of pattern is simply a regexp constant in the pattern part of
a rule. Its meaning is ‘`$0 ~ /pattern/`’.
The pattern matches when the input record matches the regexp.
For example:

```
/foo|bar|baz/  { buzzwords++ }
END            { print buzzwords, "buzzwords seen" }
```

---

Next: [Specifying Record Ranges with Patterns](#Ranges), Previous: [Regular Expressions as Patterns](#Regexp-Patterns), Up: [Pattern Elements](#Pattern-Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.2 Expressions as Patterns [¶](#Expressions-as-Patterns)

Any `awk` expression is valid as an `awk` pattern.
The pattern matches if the expression’s value is nonzero (if a
number) or non-null (if a string).
The expression is reevaluated each time the rule is tested against a new
input record. If the expression uses fields such as `$1`, the
value depends directly on the new input record’s text; otherwise, it
depends on only what has happened so far in the execution of the
`awk` program.

Comparison expressions, using the comparison operators described in
[Variable Typing and Comparison Expressions](#Typing-and-Comparison),
are a very common kind of pattern.
Regexp matching and nonmatching are also very common expressions.
The left operand of the ‘`~`’ and ‘`!~`’ operators is a string.
The right operand is either a constant regular expression enclosed in
slashes (`/regexp/`), or any expression whose string value
is used as a dynamic regular expression
(see [Using Dynamic Regexps](#Computed-Regexps)).
The following example prints the second field of each input record
whose first field is precisely ‘`li`’:

```
$ awk '$1 == "li" { print $2 }' mail-list
```

(There is no output, because there is no person with the exact name ‘`li`’.)
Contrast this with the following regular expression match, which
accepts any record with a first field that contains ‘`li`’:

```
$ awk '$1 ~ /li/ { print $2 }' mail-list
-| 555-5553
-| 555-6699
```

A regexp constant as a pattern is also a special case of an expression
pattern. The expression `/li/` has the value one if ‘`li`’
appears in the current input record. Thus, as a pattern, `/li/`
matches any record containing ‘`li`’.

Boolean expressions are also commonly used as patterns.
Whether the pattern
matches an input record depends on whether its subexpressions match.
For example, the following command prints all the records in
`mail-list` that contain both ‘`edu`’ and ‘`li`’:

```
$ awk '/edu/ && /li/' mail-list
-| Samuel       555-3430     samuel.lanceolis@shu.edu        A
```

The following command prints all records in
`mail-list` that contain *either* ‘`edu`’ or ‘`li`’
(or both, of course):

```
$ awk '/edu/ || /li/' mail-list
-| Amelia       555-5553     amelia.zodiacusque@gmail.com    F
-| Broderick    555-0542     broderick.aliquotiens@yahoo.com R
-| Fabius       555-1234     fabius.undevicesimus@ucb.edu    F
-| Julie        555-6699     julie.perscrutabor@skeeve.com   F
-| Samuel       555-3430     samuel.lanceolis@shu.edu        A
-| Jean-Paul    555-2127     jeanpaul.campanorum@nyu.edu     R
```

The following command prints all records in
`mail-list` that do *not* contain the string ‘`li`’:

```
$ awk '! /li/' mail-list
-| Anthony      555-3412     anthony.asserturo@hotmail.com   A
-| Becky        555-7685     becky.algebrarum@gmail.com      A
-| Bill         555-1675     bill.drowning@hotmail.com       A
-| Camilla      555-2912     camilla.infusarum@skynet.be     R
-| Fabius       555-1234     fabius.undevicesimus@ucb.edu    F
```

```
-| Martin       555-6480     martin.codicibus@hotmail.com    A
-| Jean-Paul    555-2127     jeanpaul.campanorum@nyu.edu     R
```

The subexpressions of a Boolean operator in a pattern can be constant regular
expressions, comparisons, or any other `awk` expressions. Range
patterns are not expressions, so they cannot appear inside Boolean
patterns. Likewise, the special patterns `BEGIN`, `END`,
`BEGINFILE`, and `ENDFILE`,
which never match any input record, are not expressions and cannot
appear inside Boolean patterns.

The precedence of the different operators that can appear in
patterns is described in [Operator Precedence (How Operators Nest)](#Precedence).

---

Next: [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND), Previous: [Expressions as Patterns](#Expression-Patterns), Up: [Pattern Elements](#Pattern-Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.3 Specifying Record Ranges with Patterns [¶](#Specifying-Record-Ranges-with-Patterns)

A *range pattern* is made of two patterns separated by a comma, in
the form ‘`begpat, endpat`’. It is used to match ranges of
consecutive input records. The first pattern, begpat, controls
where the range begins, while endpat controls where
the pattern ends. For example, the following:

```
awk '$1 == "on", $1 == "off"' myfile
```

prints every record in `myfile` between ‘`on`’/‘`off`’ pairs, inclusive.

A range pattern starts out by matching begpat against every
input record. When a record matches begpat, the range pattern is
*turned on*, and the range pattern matches this record as well. As long as
the range pattern stays turned on, it automatically matches every input
record read. The range pattern also matches endpat against every
input record; when this succeeds, the range pattern is *turned off* again
for the following record. Then the range pattern goes back to checking
begpat against each record.

The record that turns on the range pattern and the one that turns it
off both match the range pattern. If you don’t want to operate on
these records, you can write `if` statements in the rule’s action
to distinguish them from the records you are interested in.

It is possible for a pattern to be turned on and off by the same
record. If the record satisfies both conditions, then the action is
executed for just that record.
For example, suppose there is text between two identical markers (e.g.,
the ‘`%`’ symbol), each on its own line, that should be ignored.
A first attempt would be to
combine a range pattern that describes the delimited text with the
`next` statement
(not discussed yet, see [The `next` Statement](#Next-Statement)).
This causes `awk` to skip any further processing of the current
record and start over again with the next input record. Such a program
looks like this:

```
/^%$/,/^%$/    { next }
               { print }
```

This program fails because the range pattern is both turned on and turned off
by the first line, which just has a ‘`%`’ on it. To accomplish this task,
write the program in the following manner, using a flag:

```
/^%$/     { skip = ! skip; next }
skip == 1 { next } # skip lines with `skip' set
```

In a range pattern, the comma (‘`,`’) has the lowest precedence of
all the operators (i.e., it is evaluated last). Thus, the following
program attempts to combine a range pattern with another, simpler test:

```
echo Yes | awk '/1/,/2/ || /Yes/'
```

The intent of this program is ‘`(/1/,/2/) || /Yes/`’.
However, `awk` interprets this as ‘`/1/, (/2/ || /Yes/)`’.
This cannot be changed or worked around; range patterns do not combine
with other patterns:

```
$ echo Yes | gawk '(/1/,/2/) || /Yes/'
error→ gawk: cmd. line:1: (/1/,/2/) || /Yes/
error→ gawk: cmd. line:1:           ^ syntax error
```

As a minor point of interest, although it is poor style,
POSIX allows you to put a newline after the comma in
a range pattern. (d.c.)

---

Next: [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE), Previous: [Specifying Record Ranges with Patterns](#Ranges), Up: [Pattern Elements](#Pattern-Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.4 The `BEGIN` and `END` Special Patterns [¶](#The-BEGIN-and-END-Special-Patterns)

All the patterns described so far are for matching input records.
The `BEGIN` and `END` special patterns are different.
They supply startup and cleanup actions for `awk` programs.
`BEGIN` and `END` rules must have actions; there is no default
action for these rules because there is no current record when they run.
`BEGIN` and `END` rules are often referred to as
“`BEGIN` and `END` blocks” by longtime `awk`
programmers.

* [Startup and Cleanup Actions](#Using-BEGIN_002fEND)
* [Input/Output from `BEGIN` and `END` Rules](#I_002fO-And-BEGIN_002fEND)

---

Next: [Input/Output from `BEGIN` and `END` Rules](#I_002fO-And-BEGIN_002fEND), Up: [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.4.1 Startup and Cleanup Actions [¶](#Startup-and-Cleanup-Actions)

A `BEGIN` rule is executed once only, before the first input record
is read. Likewise, an `END` rule is executed once only, after all the
input is read. For example:

```
$ awk '
> BEGIN { print "Analysis of \"li\"" }
> /li/  { ++n }
> END   { print "\"li\" appears in", n, "records." }' mail-list
-| Analysis of "li"
-| "li" appears in 4 records.
```

This program finds the number of records in the input file `mail-list`
that contain the string ‘`li`’. The `BEGIN` rule prints a title
for the report. There is no need to use the `BEGIN` rule to
initialize the counter `n` to zero, as `awk` does this
automatically (see [Variables](#Variables)).
The second rule increments the variable `n` every time a
record containing the pattern ‘`li`’ is read. The `END` rule
prints the value of `n` at the end of the run.

The special patterns `BEGIN` and `END` cannot be used in ranges
or with Boolean operators (indeed, they cannot be used with any operators).
An `awk` program may have multiple `BEGIN` and/or `END`
rules. They are executed in the order in which they appear: all the `BEGIN`
rules at startup and all the `END` rules at termination.

`BEGIN` and `END` rules may be intermixed with other rules.
This feature was added in the 1987 version of `awk` and is included
in the POSIX standard.
The original (1978) version of `awk`
required the `BEGIN` rule to be placed at the beginning of the
program, the `END` rule to be placed at the end, and only allowed one of
each.
This is no longer required, but it is a good idea to follow this template
in terms of program organization and readability.

Multiple `BEGIN` and `END` rules are useful for writing
library functions, because each library file can have its own `BEGIN` and/or
`END` rule to do its own initialization and/or cleanup.
The order in which library functions are named on the command line
controls the order in which their `BEGIN` and `END` rules are
executed. Therefore, you have to be careful when writing such rules in
library files so that the order in which they are executed doesn’t matter.
See [Command-Line Options](#Options) for more information on
using library functions.
See [A Library of `awk` Functions](#Library-Functions),
for a number of useful library functions.

If an `awk` program has only `BEGIN` rules and no
other rules, then the program exits after the `BEGIN` rules are
run.[42](#FOOT42) However, if an
`END` rule exists, then the input is read, even if there are
no other rules in the program. This is necessary in case the `END`
rule checks the `FNR` and `NR` variables, or the fields.

---

Previous: [Startup and Cleanup Actions](#Using-BEGIN_002fEND), Up: [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.4.2 Input/Output from `BEGIN` and `END` Rules [¶](#Input_002fOutput-from-BEGIN-and-END-Rules)

There are several (sometimes subtle) points to be aware of when doing I/O
from a `BEGIN` or `END` rule.
The first has to do with the value of `$0` in a `BEGIN`
rule. Because `BEGIN` rules are executed before any input is read,
there simply is no input record, and therefore no fields, when
executing `BEGIN` rules. References to `$0` and the fields
yield a null string or zero, depending upon the context. One way
to give `$0` a real value is to execute a `getline` function
without a variable (see [Explicit Input with `getline`](#Getline)).
Another way is simply to assign a value to `$0`.

The second point is similar to the first, but from the other direction.
Traditionally, due largely to implementation issues, `$0` and
`NF` were *undefined* inside an `END` rule.
The POSIX standard specifies that `NF` is available in an `END`
rule. It contains the number of fields from the last input record.
Most probably due to an oversight, the standard does not say that `$0`
is also preserved, although logically one would think that it should be.
In fact, all of BWK `awk`, `mawk`, and `gawk`
preserve the value of `$0` for use in `END` rules. Be aware,
however, that some other implementations and many older versions
of Unix `awk` do not.

The third point follows from the first two. The meaning of ‘`print`’
inside a `BEGIN` or `END` rule is the same as always:
‘`print $0`’. If `$0` is the null string, then this prints an
empty record. Many longtime `awk` programmers use an unadorned
‘`print`’ in `BEGIN` and `END` rules to mean ‘`print ""`’,
relying on `$0` being null. Although one might generally get away with
this in `BEGIN` rules, it is a very bad idea in `END` rules,
at least in `gawk`. It is also poor style, because if an empty
line is needed in the output, the program should print one explicitly.

Finally, the `next` and `nextfile` statements are not allowed
in a `BEGIN` rule, because the implicit
read-a-record-and-match-against-the-rules loop has not started yet. Similarly, those statements
are not valid in an `END` rule, because all the input has been read.
(See [The `next` Statement](#Next-Statement) and
see [The `nextfile` Statement](#Nextfile-Statement).)

---

Next: [The Empty Pattern](#Empty), Previous: [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND), Up: [Pattern Elements](#Pattern-Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.5 The `BEGINFILE` and `ENDFILE` Special Patterns [¶](#The-BEGINFILE-and-ENDFILE-Special-Patterns)

This section describes a `gawk`-specific feature.

Two special kinds of rule, `BEGINFILE` and `ENDFILE`, give
you “hooks” into `gawk`’s command-line file processing loop.
As with the `BEGIN` and `END` rules
(see [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)),
`BEGINFILE` rules in a program execute in the order they are
read by `gawk`. Similarly, all `ENDFILE` rules also execute in
the order they are read.

The bodies of the `BEGINFILE` rules execute just before
`gawk` reads the first record from a file. `FILENAME`
is set to the name of the current file, and `FNR` is set to zero.

Prior to version 5.1.1 of `gawk`, as an accident of the
implementation, `$0` and the fields retained any previous values
they had in `BEGINFILE` rules. Starting with version
5.1.1, `$0` and the fields are cleared, since no record has been
read yet from the file that is about to be processed.

The `BEGINFILE` rule provides you the opportunity to accomplish two tasks
that would otherwise be difficult or impossible to perform:

* You can test if the file is readable. Normally, it is a fatal error if a
  file named on the command line cannot be opened for reading. However,
  you can bypass the fatal error and move on to the next file on the
  command line.

  You do this by checking if the `ERRNO` variable is not the empty
  string; if so, then `gawk` was not able to open the file. In
  this case, your program can execute the `nextfile` statement
  (see [The `nextfile` Statement](#Nextfile-Statement)). This causes `gawk` to skip
  the file entirely. Otherwise, `gawk` exits with the usual
  fatal error.
* If you have written extensions that modify the record handling (by
  inserting an “input parser”; see [Customized Input Parsers](#Input-Parsers)), you can invoke
  them at this point, before `gawk` has started processing the file.
  (This is a *very* advanced feature, currently used only by the
  [`gawkextlib` project](https://sourceforge.net/projects/gawkextlib).)

The `ENDFILE` rule is called when `gawk` has finished processing
the last record in an input file. For the last input file,
it will be called before any `END` rules.
The `ENDFILE` rule is executed even for empty input files.

Normally, when an error occurs when reading input in the normal
input-processing loop, the error is fatal. However, if a `BEGINFILE`
rule is present, the error becomes non-fatal, and instead `ERRNO`
is set. This makes it possible to catch and process I/O errors at the
level of the `awk` program.

The `next` statement (see [The `next` Statement](#Next-Statement)) is not allowed inside
either a `BEGINFILE` or an `ENDFILE` rule. The `nextfile`
statement is allowed only inside a
`BEGINFILE` rule, not inside an `ENDFILE` rule.

The `getline` function (see [Explicit Input with `getline`](#Getline)) is restricted inside
both `BEGINFILE` and `ENDFILE`: only redirected
forms of `getline` are allowed.

`BEGINFILE` and `ENDFILE` are `gawk` extensions.
In most other `awk` implementations, or if `gawk` is in
compatibility mode (see [Command-Line Options](#Options)), they are not special.

---

Previous: [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE), Up: [Pattern Elements](#Pattern-Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.6 The Empty Pattern [¶](#The-Empty-Pattern)

An empty (i.e., nonexistent) pattern is considered to match *every*
input record. For example, the program:

```
awk '{ print $1 }' mail-list
```

prints the first field of every record.

---

Next: [Actions](#Action-Overview), Previous: [Pattern Elements](#Pattern-Overview), Up: [Patterns, Actions, and Variables](#Patterns-and-Actions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.2 Using Shell Variables in Programs [¶](#Using-Shell-Variables-in-Programs)

`awk` programs are often used as components in larger
programs written in shell.
For example, it is very common to use a shell variable to
hold a pattern that the `awk` program searches for.
There are two ways to get the value of the shell variable
into the body of the `awk` program.

A common method is to use shell quoting to substitute
the variable’s value into the program inside the script.
For example, consider the following program:

```
printf "Enter search pattern: "
read pattern
awk "/$pattern/ "'{ nmatches++ }
     END { print nmatches, "found" }' /path/to/data
```

The `awk` program consists of two pieces of quoted text
that are concatenated together to form the program.
The first part is double-quoted, which allows substitution of
the `pattern` shell variable inside the quotes.
The second part is single-quoted.

Variable substitution via quoting works, but can potentially be
messy. It requires a good understanding of the shell’s quoting rules
(see [Shell Quoting Issues](#Quoting)),
and it’s often difficult to correctly
match up the different quotes when reading the program.

A better method is to use `awk`’s variable assignment feature
(see [Assigning Variables on the Command Line](#Assignment-Options))
to assign the shell variable’s value to an `awk` variable.
Then use dynamic regexps to match the pattern
(see [Using Dynamic Regexps](#Computed-Regexps)).
The following shows how to redo the
previous example using this technique:

```
printf "Enter search pattern: "
read pattern
awk -v pat="$pattern" '$0 ~ pat { nmatches++ }
       END { print nmatches, "found" }' /path/to/data
```

Now, the `awk` program is just one single-quoted string.
The assignment ‘`-v pat="$pattern"`’ still requires double quotes,
in case there is whitespace in the value of `$pattern`.
The `awk` variable `pat` could be named `pattern`
too, but that would be more confusing. Using a variable also
provides more flexibility, as the variable can be used anywhere inside
the program—for printing, as an array subscript, or for any other
use—without requiring the quoting tricks at every point in the program.

---

Next: [Control Statements in Actions](#Statements), Previous: [Using Shell Variables in Programs](#Using-Shell-Variables), Up: [Patterns, Actions, and Variables](#Patterns-and-Actions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.3 Actions [¶](#Actions)

An `awk` program or script consists of a series of
rules and function definitions interspersed. (Functions are
described later. See [User-Defined Functions](#User_002ddefined).)
A rule contains a pattern and an action, either of which (but not
both) may be omitted. The purpose of the *action* is to tell
`awk` what to do once a match for the pattern is found. Thus,
in outline, an `awk` program generally looks like this:

```
[pattern]  { action }
 pattern  [{ action }]
...
function name(args) { ... }
...
```

An action consists of one or more `awk` *statements*, enclosed
in braces (‘`{…}`’). Each statement specifies one
thing to do. The statements are separated by newlines or semicolons.
The braces around an action must be used even if the action
contains only one statement, or if it contains no statements at
all. However, if you omit the action entirely, omit the braces as
well. An omitted action is equivalent to ‘`{ print $0 }`’:

```
/foo/  { }     match foo, do nothing --- empty action
/foo/          match foo, print the record --- omitted action
```

The following types of statements are supported in `awk`:

Expressions
:   Call functions or assign values to variables
    (see [Expressions](#Expressions)). Executing
    this kind of statement simply computes the value of the expression.
    This is useful when the expression has side effects
    (see [Assignment Expressions](#Assignment-Ops)).

Control statements
:   Specify the control flow of `awk`
    programs. The `awk` language gives you C-like constructs
    (`if`, `for`, `while`, and `do`) as well as a few
    special ones (see [Control Statements in Actions](#Statements)).

Compound statements
:   Enclose one or more statements in braces. A compound statement
    is used in order to put several statements together in the body of an
    `if`, `while`, `do`, or `for` statement.

Input statements
:   Use the `getline` function
    (see [Explicit Input with `getline`](#Getline)).
    Also supplied in `awk` are the `next`
    statement (see [The `next` Statement](#Next-Statement))
    and the `nextfile` statement
    (see [The `nextfile` Statement](#Nextfile-Statement)).

Output statements
:   Such as `print` and `printf`.
    See [Printing Output](#Printing).

Deletion statements
:   For deleting array elements.
    See [The `delete` Statement](#Delete).

---

Next: [Predefined Variables](#Built_002din-Variables), Previous: [Actions](#Action-Overview), Up: [Patterns, Actions, and Variables](#Patterns-and-Actions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.4 Control Statements in Actions [¶](#Control-Statements-in-Actions)

*Control statements*, such as `if`, `while`, and so on,
control the flow of execution in `awk` programs. Most of `awk`’s
control statements are patterned after similar statements in C.

All the control statements start with special keywords, such as `if`
and `while`, to distinguish them from simple expressions.
Many control statements contain other statements. For example, the
`if` statement contains another statement that may or may not be
executed. The contained statement is called the *body*.
To include more than one statement in the body, group them into a
single *compound statement* with braces, separating them with
newlines or semicolons.

* [The `if`-`else` Statement](#If-Statement)
* [The `while` Statement](#While-Statement)
* [The `do`-`while` Statement](#Do-Statement)
* [The `for` Statement](#For-Statement)
* [The `switch` Statement](#Switch-Statement)
* [The `break` Statement](#Break-Statement)
* [The `continue` Statement](#Continue-Statement)
* [The `next` Statement](#Next-Statement)
* [The `nextfile` Statement](#Nextfile-Statement)
* [The `exit` Statement](#Exit-Statement)

---

Next: [The `while` Statement](#While-Statement), Up: [Control Statements in Actions](#Statements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.1 The `if`-`else` Statement [¶](#The-if_002delse-Statement)

The `if`-`else` statement is `awk`’s decision-making
statement. It looks like this:

```
if (condition) then-body [else else-body]
```

The condition is an expression that controls what the rest of the
statement does. If the condition is true, then-body is
executed; otherwise, else-body is executed.
The `else` part of the statement is
optional. The condition is considered false if its value is zero or
the null string; otherwise, the condition is true.
Refer to the following:

```
if (x % 2 == 0)
    print "x is even"
else
    print "x is odd"
```

In this example, if the expression ‘`x % 2 == 0`’ is true (i.e.,
if the value of `x` is evenly divisible by two), then the first
`print` statement is executed; otherwise, the second `print`
statement is executed.
If the `else` keyword appears on the same line as then-body and
then-body is not a compound statement (i.e., not surrounded by
braces), then a semicolon must separate then-body from
the `else`.
To illustrate this, the previous example can be rewritten as:

```
if (x % 2 == 0) print "x is even"; else
        print "x is odd"
```

If the ‘`;`’ is left out, `awk` can’t interpret the statement and
it produces a syntax error. Don’t actually write programs this way,
because a human reader might fail to see the `else` if it is not
the first thing on its line.

---

Next: [The `do`-`while` Statement](#Do-Statement), Previous: [The `if`-`else` Statement](#If-Statement), Up: [Control Statements in Actions](#Statements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.2 The `while` Statement [¶](#The-while-Statement)

In programming, a *loop* is a part of a program that can
be executed two or more times in succession.
The `while` statement is the simplest looping statement in
`awk`. It repeatedly executes a statement as long as a condition is
true. For example:

```
while (condition)
  body
```

body is a statement called the *body* of the loop,
and condition is an expression that controls how long the loop
keeps running.
The first thing the `while` statement does is test the condition.
If the condition is true, it executes the statement body.
After body has been executed,
condition is tested again, and if it is still true, body
executes again. This process repeats until the condition is no longer
true. If the condition is initially false, the body of the loop
never executes and `awk` continues with the statement following
the loop.
This example prints the first three fields of each record, one per line:

```
awk '
{
    i = 1
    while (i <= 3) {
        print $i
        i++
    }
}' inventory-shipped
```

The body of this loop is a compound statement enclosed in braces,
containing two statements.
The loop works in the following manner: first, the value of `i` is set to one.
Then, the `while` statement tests whether `i` is less than or equal to
three. This is true when `i` equals one, so the `i`th
field is printed. Then the ‘`i++`’ increments the value of `i`
and the loop repeats. The loop terminates when `i` reaches four.

A newline is not required between the condition and the
body; however, using one makes the program clearer unless the body is a
compound statement or else is very simple. The newline after the open brace
that begins the compound statement is not required either, but the
program is harder to read without it.

---

Next: [The `for` Statement](#For-Statement), Previous: [The `while` Statement](#While-Statement), Up: [Control Statements in Actions](#Statements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.3 The `do`-`while` Statement [¶](#The-do_002dwhile-Statement)

The `do` loop is a variation of the `while` looping statement.
The `do` loop executes the body once and then repeats the
body as long as the condition is true. It looks like this:

```
do
  body
while (condition)
```

Even if the condition is false at the start, the body
executes at least once (and only once, unless executing body
makes condition true). Contrast this with the corresponding
`while` statement:

```
while (condition)
    body
```

This statement does not execute the body even once if the
condition is false to begin with. The following is an example of
a `do` statement:

```
{
    i = 1
    do {
        print $0
        i++
    } while (i <= 10)
}
```

This program prints each input record 10 times. However, it isn’t a very
realistic example, because in this case an ordinary `while` would do
just as well. This situation reflects actual experience; only
occasionally is there a real use for a `do` statement.

---

Next: [The `switch` Statement](#Switch-Statement), Previous: [The `do`-`while` Statement](#Do-Statement), Up: [Control Statements in Actions](#Statements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.4 The `for` Statement [¶](#The-for-Statement)

The `for` statement makes it more convenient to count iterations of a
loop. The general form of the `for` statement looks like this:

```
for (initialization; condition; increment)
  body
```

The initialization, condition, and increment parts are
arbitrary `awk` expressions, and body stands for any
`awk` statement.

The `for` statement starts by executing initialization.
Then, as long
as the condition is true, it repeatedly executes body and then
increment. Typically, initialization sets a variable to
either zero or one, increment adds one to it, and condition
compares it against the desired number of iterations.
For example:

```
awk '
{
    for (i = 1; i <= 3; i++)
        print $i
}' inventory-shipped
```

This prints the first three fields of each input record, with one
input field per output line.

C and C++ programmers might expect to be able to use the comma
operator to set more than one variable in the initialization
part of the `for` loop, or to increment multiple variables in the
increment part of the loop, like so:

```
for (i = 0, j = length(a); i < j; i++, j--) ...   C/C++, not awk!
```

You cannot do this; the comma operator is not supported in `awk`.
There are workarounds, but they are nonobvious and can lead to
code that is difficult to read and understand. It is best, therefore,
to simply write additional initializations as separate statements
preceding the `for` loop and to place additional increment statements
at the end of the loop’s body.

Most often, increment is an increment expression, as in the earlier
example. But this is not required; it can be any expression
whatsoever. For example, the following statement prints all the powers of two
between 1 and 100:

```
for (i = 1; i <= 100; i *= 2)
    print i
```

If there is nothing to be done, any of the three expressions in the
parentheses following the `for` keyword may be omitted. Thus,
‘`for (; x > 0;)`’ is equivalent to ‘`while (x > 0)`’. If the
condition is omitted, it is treated as true, effectively
yielding an *infinite loop* (i.e., a loop that never terminates).

In most cases, a `for` loop is an abbreviation for a `while`
loop, as shown here:

```
initialization
while (condition) {
  body
  increment
}
```

The only exception is when the `continue` statement
(see [The `continue` Statement](#Continue-Statement)) is used
inside the loop. Changing a `for` statement to a `while`
statement in this way can change the effect of the `continue`
statement inside the loop.

The `awk` language has a `for` statement in addition to a
`while` statement because a `for` loop is often both less work to
type and more natural to think of. Counting the number of iterations is
very common in loops. It can be easier to think of this counting as part
of looping rather than as something to do inside the loop.

There is an alternative version of the `for` loop, for iterating over
all the indices of an array:

```
for (i in array)
    do something with array[i]
```

See [Scanning All Elements of an Array](#Scanning-an-Array)
for more information on this version of the `for` loop.

---

Next: [The `break` Statement](#Break-Statement), Previous: [The `for` Statement](#For-Statement), Up: [Control Statements in Actions](#Statements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.5 The `switch` Statement [¶](#The-switch-Statement)

This section describes a `gawk`-specific feature.
If `gawk` is in compatibility mode (see [Command-Line Options](#Options)),
it is not available.

The `switch` statement allows the evaluation of an expression and
the execution of statements based on a `case` match. Case statements
are checked for a match in the order they are defined. If no suitable
`case` is found, the `default` section is executed, if supplied.

Each `case` contains a single constant, be it numeric, string,
or regexp. The `switch` expression is evaluated, and then each
`case`’s constant is compared against the result in turn. The
type of constant determines the comparison: numeric or string do the
usual comparisons. A regexp constant (either regular, `/foo/`, or
strongly typed, `@/foo/`) does a regular expression match against
the string value of the original expression. The general form of the
`switch` statement looks like this:

```
switch (expression) {
case value or regular expression:
    case-body
default:
    default-body
}
```

Control flow in
the `switch` statement works as it does in C. Once a match to a given
case is made, the case statement bodies execute until a `break`,
`continue`, `next`, `nextfile`, or `exit` is encountered,
or the end of the `switch` statement itself. For example:

```
while ((c = getopt(ARGC, ARGV, "aksx")) != -1) {
    switch (c) {
    case "a":
        # report size of all files
        all_files = TRUE;
        break
    case "k":
        BLOCK_SIZE = 1024       # 1K block size
        break
    case "s":
        # do sums only
        sum_only = TRUE
        break
    case "x":
        # don't cross filesystems
        fts_flags = or(fts_flags, FTS_XDEV)
        break
    case "?":
    default:
        usage()
        break
    }
}
```

Note that if none of the statements specified here halt execution
of a matched `case` statement, execution falls through to the
next `case` until execution halts. In this example, the
`case` for `"?"` falls through to the `default`
case, which is to call a function named `usage()`.
(The `getopt()` function being called here is
described in [Processing Command-Line Options](#Getopt-Function).)

---

Next: [The `continue` Statement](#Continue-Statement), Previous: [The `switch` Statement](#Switch-Statement), Up: [Control Statements in Actions](#Statements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.6 The `break` Statement [¶](#The-break-Statement)

The `break` statement serves two purposes.
One purpose is to break out of the
`switch` statement.
This was discussed in [The `switch` Statement](#Switch-Statement).

The other purpose is that it jumps out of the innermost `for`,
`while`, or `do` loop that encloses it. The following example
finds the smallest divisor of any integer, and also identifies prime
numbers:

```
# find smallest divisor of num
{
    num = $1
    for (divisor = 2; divisor * divisor <= num; divisor++) {
        if (num % divisor == 0)
            break
    }
```

```
    if (num % divisor == 0)
        printf "Smallest divisor of %d is %d\n", num, divisor
    else
        printf "%d is prime\n", num
}
```

When the remainder is zero in the first `if` statement, `awk`
immediately *breaks out* of the containing `for` loop. This means
that `awk` proceeds immediately to the statement following the loop
and continues processing. (This is very different from the `exit`
statement, which stops the entire `awk` program.
See [The `exit` Statement](#Exit-Statement).)

The following program illustrates how the condition of a `for`
or `while` statement could be replaced with a `break` inside
an `if`:

```
# find smallest divisor of num
{
    num = $1
    for (divisor = 2; ; divisor++) {
        if (num % divisor == 0) {
            printf "Smallest divisor of %d is %d\n", num, divisor
            break
        }
        if (divisor * divisor > num) {
            printf "%d is prime\n", num
            break
        }
    }
}
```

Note that `break` breaks out of only the innermost
enclosing statement. Thus a `break` inside a `switch` statement
inside a loop only breaks out of the `switch`.

The `break` statement has no meaning when
used outside the body of a loop or `switch`.
However, although it was never documented,
historical implementations of `awk` treated the `break`
statement outside of a loop as if it were a `next` statement
(see [The `next` Statement](#Next-Statement)).
(d.c.)
Recent versions of BWK `awk` no longer allow this usage,
nor does `gawk`.

---

Next: [The `next` Statement](#Next-Statement), Previous: [The `break` Statement](#Break-Statement), Up: [Control Statements in Actions](#Statements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.7 The `continue` Statement [¶](#The-continue-Statement)

Similar to `break`, the `continue` statement is used only inside
`for`, `while`, and `do` loops. It skips
over the rest of the loop body, causing the next cycle around the loop
to begin immediately. Contrast this with `break`, which jumps out
of the loop altogether.

The `continue` statement in a `for` loop directs `awk` to
skip the rest of the body of the loop and resume execution with the
increment-expression of the `for` statement. The following program
illustrates this fact:

```
BEGIN {
     for (x = 0; x <= 20; x++) {
         if (x == 5)
             continue
         printf "%d ", x
     }
     print ""
}
```

This program prints all the numbers from 0 to 20—except for 5, for
which the `printf` is skipped. Because the increment ‘`x++`’
is not skipped, `x` does not remain stuck at 5. Contrast the
`for` loop from the previous example with the following `while` loop:

```
BEGIN {
     x = 0
     while (x <= 20) {
         if (x == 5)
             continue
         printf "%d ", x
         x++
     }
     print ""
}
```

This program loops forever once `x` reaches 5, because
the increment (‘`x++`’) is never reached.

The `continue` statement has no special meaning with respect to the
`switch` statement, nor does it have any meaning when used outside the
body of a loop. Historical versions of `awk` treated a `continue`
statement outside a loop the same way they treated a `break`
statement outside a loop: as if it were a `next`
statement
(see [The `next` Statement](#Next-Statement)).
(d.c.)
Recent versions of BWK `awk` no longer work this way, nor
does `gawk`.

---

Next: [The `nextfile` Statement](#Nextfile-Statement), Previous: [The `continue` Statement](#Continue-Statement), Up: [Control Statements in Actions](#Statements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.8 The `next` Statement [¶](#The-next-Statement)

The `next` statement forces `awk` to immediately stop processing
the current record and go on to the next record. This means that no
further rules are executed for the current record, and the rest of the
current rule’s action isn’t executed.

Contrast this with the effect of the `getline` function
(see [Explicit Input with `getline`](#Getline)). That also causes
`awk` to read the next record immediately, but it does not alter the
flow of control in any way (i.e., the rest of the current action executes
with a new input record).

At the highest level, `awk` program execution is a loop that reads
an input record and then tests each rule’s pattern against it. If you
think of this loop as a `for` statement whose body contains the
rules, then the `next` statement is analogous to a `continue`
statement. It skips to the end of the body of this implicit loop and
executes the increment (which reads another record).

For example, suppose an `awk` program works only on records
with four fields, and it shouldn’t fail when given bad input. To avoid
complicating the rest of the program, write a “weed out” rule near
the beginning, in the following manner:

```
NF != 4 {
    printf("%s:%d: skipped: NF != 4\n", FILENAME, FNR) > "/dev/stderr"
    next
}
```

Because of the `next` statement,
the program’s subsequent rules won’t see the bad record. The error
message is redirected to the standard error output stream, as error
messages should be.
For more detail, see
[Special File names in `gawk`](#Special-Files).

If the `next` statement causes the end of the input to be reached,
then the code in any `END` rules is executed.
See [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND).

The `next` statement is not allowed inside `BEGINFILE` and
`ENDFILE` rules. See [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE).

According to the POSIX standard, the behavior is undefined if the
`next` statement is used in a `BEGIN` or `END` rule.
`gawk` treats it as a syntax error. Although POSIX does not disallow it,
most other `awk` implementations don’t allow the `next`
statement inside function bodies (see [User-Defined Functions](#User_002ddefined)). Just as with any
other `next` statement, a `next` statement inside a function
body reads the next record and starts processing it with the first rule
in the program.

---

Next: [The `exit` Statement](#Exit-Statement), Previous: [The `next` Statement](#Next-Statement), Up: [Control Statements in Actions](#Statements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.9 The `nextfile` Statement [¶](#The-nextfile-Statement)

The `nextfile` statement
is similar to the `next` statement.
However, instead of abandoning processing of the current record, the
`nextfile` statement instructs `awk` to stop processing the
current data file.

Upon execution of the `nextfile` statement,
`FILENAME` is
updated to the name of the next data file listed on the command line,
`FNR` is reset to one,
and processing
starts over with the first rule in the program.
If the `nextfile` statement causes the end of the input to be reached,
then the code in any `END` rules is executed. An exception to this is
when `nextfile` is invoked during execution of any statement in an
`END` rule; in this case, it causes the program to stop immediately.
See [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND).

The `nextfile` statement is useful when there are many data files
to process but it isn’t necessary to process every record in every file.
Without `nextfile`,
in order to move on to the next data file, a program
would have to continue scanning the unwanted records. The `nextfile`
statement accomplishes this much more efficiently.

In `gawk`, execution of `nextfile` causes additional things
to happen: any `ENDFILE` rules are executed if `gawk` is
not currently in an `END` rule, `ARGIND` is
incremented, and any `BEGINFILE` rules are executed. (`ARGIND`
hasn’t been introduced yet. See [Predefined Variables](#Built_002din-Variables).)

There is an additional, special, use case
with `gawk`. `nextfile` is useful inside a `BEGINFILE`
rule to skip over a file that would otherwise cause `gawk`
to exit with a fatal error. In this special case, `ENDFILE` rules are not
executed. See [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE).

Although it might seem that ‘`close(FILENAME)`’ would accomplish
the same as `nextfile`, this isn’t true. `close()` is
reserved for closing files, pipes, and coprocesses that are
opened with redirections. It is not related to the main processing that
`awk` does with the files listed in `ARGV`.

> **NOTE:** For many years, `nextfile` was a
> common extension. In September 2012, it was accepted for
> inclusion into the POSIX standard.
> See [the Austin Group website](https://austingroupbugs.net/view.php?id=607).

The current version of BWK `awk` and `mawk`
also support `nextfile`. However, they don’t allow the
`nextfile` statement inside function bodies (see [User-Defined Functions](#User_002ddefined)).
`gawk` does; a `nextfile` inside a function body reads the
first record from the next file and starts processing it with the first
rule in the program, just as any other `nextfile` statement.

---

Previous: [The `nextfile` Statement](#Nextfile-Statement), Up: [Control Statements in Actions](#Statements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.10 The `exit` Statement [¶](#The-exit-Statement)

The `exit` statement causes `awk` to immediately stop
executing the current rule and to stop processing input; any remaining input
is ignored. The `exit` statement is written as follows:

```
exit [return code]
```

When an `exit` statement is executed from a `BEGIN` rule, the
program stops processing everything immediately. No input records are
read. However, if an `END` rule is present,
as part of executing the `exit` statement,
the `END` rule is executed
(see [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)).
If `exit` is used in the body of an `END` rule, it causes
the program to stop immediately.

An `exit` statement that is not part of a `BEGIN` or `END`
rule stops the execution of any further automatic rules for the current
record, skips reading any remaining input records, and executes the
`END` rule if there is one. `gawk` also skips
any `ENDFILE` rules; they do not execute.

In such a case,
if you don’t want the `END` rule to do its job, set a variable
to a nonzero value before the `exit` statement and check that variable in
the `END` rule.
See [Assertions](#Assert-Function)
for an example that does this.

If an argument is supplied to `exit`, its value is used as the exit
status code for the `awk` process. If no argument is supplied,
`exit` causes `awk` to return a “success” status.
In the case where an argument
is supplied to a first `exit` statement, and then `exit` is
called a second time from an `END` rule with no argument,
`awk` uses the previously supplied exit value. (d.c.)
See [`gawk`’s Exit Status](#Exit-Status) for more information.

For example, suppose an error condition occurs that is difficult or
impossible to handle. Conventionally, programs report this by
exiting with a nonzero status. An `awk` program can do this
using an `exit` statement with a nonzero argument, as shown
in the following example:

```
BEGIN {
    if (("date" | getline date_now) <= 0) {
        print "Can't get system date" > "/dev/stderr"
        exit 1
    }
```

```
    print "current date is", date_now
    close("date")
}
```

> **NOTE:** For full portability, exit values should be between zero and 126, inclusive.
> Negative values, and values of 127 or greater, may not produce consistent
> results across different operating systems.

---

Next: [Summary](#Pattern-Action-Summary), Previous: [Control Statements in Actions](#Statements), Up: [Patterns, Actions, and Variables](#Patterns-and-Actions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
