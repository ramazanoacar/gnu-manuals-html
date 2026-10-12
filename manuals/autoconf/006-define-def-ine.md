# define([def], ine)
⇒# define([def], ine)
```

Each time there can be a macro expansion, there is a quotation
expansion, i.e., one level of quotes is stripped:

```
int tab[10];
⇒int tab10;
[int tab[10];]
⇒int tab[10];
```

Without this in mind, the reader might try hopelessly to use her macro
`array`:

```
define([array], [int tab[10];])
array
⇒int tab10;
[array]
⇒array
```

How can you correctly output the intended results[4](#FOOT4)?

---

Next: [Quoting and Parameters](#Quoting-and-Parameters), Previous: [Active Characters](#Active-Characters), Up: [M4 Quotation](#M4-Quotation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.1.2 One Macro Call [¶](#One-Macro-Call-1)

Let’s proceed on the interaction between active characters and macros
with this small macro, which just returns its first argument:

```
define([car], [$1])
```

The two pairs of quotes above are not part of the arguments of
`define`; rather, they are understood by the top level when it
tries to find the arguments of `define`. Therefore, assuming
`car` is not already defined, it is equivalent to write:

```
define(car, $1)
```

But, while it is acceptable for a `configure.ac` to avoid unnecessary
quotes, it is bad practice for Autoconf macros which must both be more
robust and also advocate perfect style.

At the top level, there are only two possibilities: either you
quote or you don’t:

```
car(foo, bar, baz)
⇒foo
[car(foo, bar, baz)]
⇒car(foo, bar, baz)
```

Let’s pay attention to the special characters:

```
car(#)
error→EOF in argument list
```

The closing parenthesis is hidden in the comment; with a hypothetical
quoting, the top level understood it this way:

```
car([#)]
```

Proper quotation, of course, fixes the problem:

```
car([#])
⇒#
```

Here are more examples:

```
car(foo, bar)
⇒foo
car([foo, bar])
⇒foo, bar
car((foo, bar))
⇒(foo, bar)
car([(foo], [bar)])
⇒(foo
define([a], [b])
⇒
car(a)
⇒b
car([a])
⇒b
car([[a]])
⇒a
car([[[a]]])
⇒[a]
```

---

Next: [Quotation and Nested Macros](#Quotation-and-Nested-Macros), Previous: [One Macro Call](#One-Macro-Call), Up: [M4 Quotation](#M4-Quotation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.1.3 Quoting and Parameters [¶](#Quoting-and-Parameters-1)

When M4 encounters ‘`$`’ within a macro definition, followed
immediately by a character it recognizes (‘`0`’…‘`9`’,
‘`#`’, ‘`@`’, or ‘`*`’), it will perform M4 parameter
expansion. This happens regardless of how many layers of quotes the
parameter expansion is nested within, or even if it occurs in text that
will be rescanned as a comment.

```
define([none], [$1])
⇒
define([one], [[$1]])
⇒
define([two], [[[$1]]])
⇒
define([comment], [# $1])
⇒
define([active], [ACTIVE])
⇒
none([active])
⇒ACTIVE
one([active])
⇒active
two([active])
⇒[active]
comment([active])
⇒# active
```

On the other hand, since autoconf generates shell code, you often want
to output shell variable expansion, rather than performing M4 parameter
expansion. To do this, you must use M4 quoting to separate the ‘`$`’
from the next character in the definition of your macro. If the macro
definition occurs in single-quoted text, then insert another level of
quoting; if the usage is already inside a double-quoted string, then
split it into concatenated strings.

```
define([foo], [a single-quoted $[]1 definition])
⇒
define([bar], [[a double-quoted $][1 definition]])
⇒
foo
⇒a single-quoted $1 definition
bar
⇒a double-quoted $1 definition
```

POSIX states that M4 implementations are free to provide implementation
extensions when ‘`${`’ is encountered in a macro definition.
Autoconf reserves the longer sequence ‘`${{`’ for use with planned
extensions that will be available in the future GNU M4 2.0,
but guarantees that all other instances of ‘`${`’ will be output
literally. Therefore, this idiom can also be used to output shell code
parameter references:

```
define([first], [${1}])first
⇒${1}
```

POSIX also states that ‘`$11`’ should expand to the first parameter
concatenated with a literal ‘`1`’, although some versions of
GNU M4 expand the eleventh parameter instead. For
portability, you should only use single-digit M4 parameter expansion.

With this in mind, we can explore the cases where macros invoke
macros...

---

Next: [`changequote` is Evil](#Changequote-is-Evil), Previous: [Quoting and Parameters](#Quoting-and-Parameters), Up: [M4 Quotation](#M4-Quotation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.1.4 Quotation and Nested Macros [¶](#Quotation-and-Nested-Macros-1)

The examples below use the following macros:

```
define([car], [$1])
define([active], [ACT, IVE])
define([array], [int tab[10]])
```

Each additional embedded macro call introduces other possible
interesting quotations:

```
car(active)
⇒ACT
car([active])
⇒ACT, IVE
car([[active]])
⇒active
```

In the first case, the top level looks for the arguments of `car`,
and finds ‘`active`’. Because M4 evaluates its arguments
before applying the macro, ‘`active`’ is expanded, which results in:

```
car(ACT, IVE)
⇒ACT
```

In the second case, the top level gives ‘`active`’ as first and only
argument of `car`, which results in:

```
active
⇒ACT, IVE
```

i.e., the argument is evaluated *after* the macro that invokes it.
In the third case, `car` receives ‘`[active]`’, which results in:

```
[active]
⇒active
```

exactly as we already saw above.

The example above, applied to a more realistic example, gives:

```
car(int tab[10];)
⇒int tab10;
car([int tab[10];])
⇒int tab10;
car([[int tab[10];]])
⇒int tab[10];
```

Huh? The first case is easily understood, but why is the second wrong,
and the third right? To understand that, you must know that after
M4 expands a macro, the resulting text is immediately subjected
to macro expansion and quote removal. This means that the quote removal
occurs twice—first before the argument is passed to the `car`
macro, and second after the `car` macro expands to the first
argument.

As the author of the Autoconf macro `car`, you then consider it to
be incorrect that your users have to double-quote the arguments of
`car`, so you “fix” your macro. Let’s call it `qar` for
quoted car:

```
define([qar], [[$1]])
```

and check that `qar` is properly fixed:

```
qar([int tab[10];])
⇒int tab[10];
```

Ahhh! That’s much better.

But note what you’ve done: now that the result of `qar` is always
a literal string, the only time a user can use nested macros is if she
relies on an *unquoted* macro call:

```
qar(active)
⇒ACT
qar([active])
⇒active
```

leaving no way for her to reproduce what she used to do with `car`:

```
car([active])
⇒ACT, IVE
```

Worse yet: she wants to use a macro that produces a set of `cpp`
macros:

```
define([my_includes], [#include <stdio.h>])
car([my_includes])
⇒#include <stdio.h>
qar(my_includes)
error→EOF in argument list
```

This macro, `qar`, because it double quotes its arguments, forces
its users to leave their macro calls unquoted, which is dangerous.
Commas and other active symbols are interpreted by M4 before
they are given to the macro, often not in the way the users expect.
Also, because `qar` behaves differently from the other macros,
it’s an exception that should be avoided in Autoconf.

---

Next: [Quadrigraphs](#Quadrigraphs), Previous: [Quotation and Nested Macros](#Quotation-and-Nested-Macros), Up: [M4 Quotation](#M4-Quotation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.1.5 `changequote` is Evil [¶](#changequote-is-Evil)

The temptation is often high to bypass proper quotation, in particular
when it’s late at night. Then, many experienced Autoconf hackers
finally surrender to the dark side of the force and use the ultimate
weapon: `changequote`.

The M4 builtin `changequote` belongs to a set of primitives that
allow one to adjust the syntax of the language to adjust it to one’s
needs. For instance, by default M4 uses ‘`` ` ``’ and ‘`'`’ as
quotes, but in the context of shell programming (and actually of most
programming languages), that’s about the worst choice one can make:
because of strings and back-quoted expressions in shell code (such as
‘`'this'`’ and ‘`` `that` ``’), and because of literal characters in usual
programming languages (as in ‘`'0'`’), there are many unbalanced
‘`` ` ``’ and ‘`'`’. Proper M4 quotation then becomes a nightmare, if
not impossible. In order to make M4 useful in such a context, its
designers have equipped it with `changequote`, which makes it
possible to choose another pair of quotes. M4sugar, M4sh, Autoconf, and
Autotest all have chosen to use ‘`[`’ and ‘`]`’. Not especially
because they are unlikely characters, but *because they are
characters unlikely to be unbalanced*.

There are other magic primitives, such as `changecom` to specify
what syntactic forms are comments (it is common to see
‘`changecom(<!--, -->)`’ when M4 is used to produce HTML pages),
`changeword` and `changesyntax` to change other syntactic
details (such as the character to denote the nth argument, ‘`$`’ by
default, the parentheses around arguments, etc.).

These primitives are really meant to make M4 more useful for specific
domains: they should be considered like command line options:
`--quotes`, `--comments`, `--words`, and
`--syntax`. Nevertheless, they are implemented as M4 builtins, as
it makes M4 libraries self contained (no need for additional options).

There lies the problem...

  

The problem is that it is then tempting to use them in the middle of an
M4 script, as opposed to its initialization. This, if not carefully
thought out, can lead to disastrous effects: *you are changing the
language in the middle of the execution*. Changing and restoring the
syntax is often not enough: if you happened to invoke macros in between,
these macros are lost, as the current syntax is probably not
the one they were implemented with.

---

Next: [Dealing with unbalanced parentheses](#Balancing-Parentheses), Previous: [`changequote` is Evil](#Changequote-is-Evil), Up: [M4 Quotation](#M4-Quotation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.1.6 Quadrigraphs [¶](#Quadrigraphs-1)

When writing an Autoconf macro you may occasionally need to generate
special characters that are difficult to express with the standard
Autoconf quoting rules. For example, you may need to output the regular
expression ‘`[^[]`’, which matches any character other than ‘`[`’.
This expression contains unbalanced brackets so it cannot be put easily
into an M4 macro.

Additionally, there are a few m4sugar macros (such as `m4_split`
and `m4_expand`) which internally use special markers in addition
to the regular quoting characters. If the arguments to these macros
contain the literal strings ‘`-=<{(`’ or ‘`)}>=-`’, the macros
might behave incorrectly.

You can work around these problems by using one of the following
*quadrigraphs*:

‘`@<:@`’
:   ‘`[`’

‘`@:>@`’
:   ‘`]`’

‘`@S|@`’
:   ‘`$`’

‘`@%:@`’
:   ‘`#`’

‘`@{:@`’
:   ‘`(`’

‘`@:}@`’
:   ‘`)`’

‘`@&t@`’
:   Expands to nothing.

Quadrigraphs are replaced at a late stage of the translation process,
after `m4` is run, so they do not get in the way of M4 quoting.
For example, the string ‘`^@<:@`’, independently of its quotation,
appears as ‘`^[`’ in the output.

The empty quadrigraph can be used:

* to mark trailing spaces explicitly

  Trailing spaces are smashed by `autom4te`. This is a feature.
* to produce quadrigraphs and other strings reserved by m4sugar

  For instance ‘`@<@&t@:@`’ produces ‘`@<:@`’. For a more
  contrived example:

  ```
  m4_define([a], [A])m4_define([b], [B])m4_define([c], [C])dnl
  m4_split([a )}>=- b -=<{( c])
  ⇒[a], [], [B], [], [c]
  m4_split([a )}@&t@>=- b -=<@&t@{( c])
  ⇒[a], [)}>=-], [b], [-=<{(], [c]
  ```
* to escape *occurrences* of forbidden patterns

  For instance you might want to mention `AC_FOO` in a comment, while
  still being sure that `autom4te` still catches unexpanded
  ‘`AC_*`’. Then write ‘`AC@&t@_FOO`’.

The name ‘`@&t@`’ was suggested by Paul Eggert:

> I should give some credit to the ‘`@&t@`’ pun. The ‘`&`’ is my
> own invention, but the ‘`t`’ came from the source code of the
> ALGOL68C compiler, written by Steve Bourne (of Bourne shell fame),
> and which used ‘`mt`’ to denote the empty string. In C, it would
> have looked like something like:
>
> ```
> char const mt[] = "";
> ```
>
> but of course the source code was written in Algol 68.
>
> I don’t know where he got ‘`mt`’ from: it could have been his own
> invention, and I suppose it could have been a common pun around the
> Cambridge University computer lab at the time.

---

Next: [Quotation Rule Of Thumb](#Quotation-Rule-Of-Thumb), Previous: [Quadrigraphs](#Quadrigraphs), Up: [M4 Quotation](#M4-Quotation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.1.7 Dealing with unbalanced parentheses [¶](#Dealing-with-unbalanced-parentheses)

One of the pitfalls of portable shell programming is that
if you intend your script to run with obsolescent shells,
`case` statements require unbalanced parentheses.
See [Limitations of Shell Builtins](#case).
With syntax highlighting
editors, the presence of unbalanced ‘`)`’ can interfere with editors
that perform syntax highlighting of macro contents based on finding the
matching ‘`(`’. Another concern is how much editing must be done
when transferring code snippets between shell scripts and macro
definitions. But most importantly, the presence of unbalanced
parentheses can introduce expansion bugs.

For an example, here is an underquoted attempt to use the macro
`my_case`, which happens to expand to a portable `case`
statement:

```
AC_DEFUN([my_case],
[case $file_name in
  *.c) file_type='C source code';;
esac])
AS_IF(:, my_case)
```

In the above example, the `AS_IF` call under-quotes its arguments.
As a result, the unbalanced ‘`)`’ generated by the premature
expansion of `my_case` results in expanding `AS_IF` with a
truncated parameter, and the expansion is syntactically invalid:

```
if :
then :
  case $file_name in
  *.c
fi file_type='C source code';;
esac)
```

If nothing else, this should emphasize the importance of the quoting
arguments to macro calls. On the other hand, there are several
variations for defining `my_case` to be more robust, even when used
without proper quoting, each with some benefits and some drawbacks.

* Use left parenthesis before pattern

  ```
  AC_DEFUN([my_case],
  [case $file_name in
    (*.c) file_type='C source code';;
  esac])
  ```

  This is simple and provides balanced parentheses. Although this is not
  portable to obsolescent shells (notably Solaris 10 `/bin/sh`),
  platforms with these shells invariably have a more-modern shell
  available somewhere so this approach typically suffices nowadays.
* Creative literal shell comment

  ```
  AC_DEFUN([my_case],
  [case $file_name in #(
    *.c) file_type='C source code';;
  esac])
  ```

  This version provides balanced parentheses to several editors, and can
  be copied and pasted into a terminal as is. Unfortunately, it is still
  unbalanced as an Autoconf argument, since ‘`#(`’ is an M4 comment
  that masks the normal properties of ‘`(`’.
* Quadrigraph shell comment

  ```
  AC_DEFUN([my_case],
  [case $file_name in @%:@(
    *.c) file_type='C source code';;
  esac])
  ```

  This version provides balanced parentheses to even more editors, and can
  be used as a balanced Autoconf argument. Unfortunately, it requires
  some editing before it can be copied and pasted into a terminal, and the
  use of the quadrigraph ‘`@%:@`’ for ‘`#`’ reduces readability.
* Quoting just the parenthesis

  ```
  AC_DEFUN([my_case],
  [case $file_name in
    *.c[)] file_type='C source code';;
  esac])
  ```

  This version quotes the ‘`)`’, so that it can be used as a balanced
  Autoconf argument. As written, this is not balanced to an editor, but
  it can be coupled with ‘`[#(]`’ to meet that need, too. However, it
  still requires some edits before it can be copied and pasted into a
  terminal.
* Double-quoting the entire statement

  ```
  AC_DEFUN([my_case],
  [[case $file_name in #(
    *.c) file_type='C source code';;
  esac]])
  ```

  Since the entire macro is double-quoted, there is no problem with using
  this as an Autoconf argument; and since the double-quoting is over the
  entire statement, this code can be easily copied and pasted into a
  terminal. However, the double quoting prevents the expansion of any
  macros inside the case statement, which may cause its own set of
  problems.
* Using `AS_CASE`

  ```
  AC_DEFUN([my_case],
  [AS_CASE([$file_name],
    [*.c], [file_type='C source code'])])
  ```

  This version avoids the balancing issue altogether, by relying on
  `AS_CASE` (see [Common Shell Constructs](#Common-Shell-Constructs)); it also allows for the
  expansion of `AC_REQUIRE` to occur prior to the entire case
  statement, rather than within a branch of the case statement that might
  not be taken. However, the abstraction comes with a penalty that it is
  no longer a quick copy, paste, and edit to get back to shell code.

---

Previous: [Dealing with unbalanced parentheses](#Balancing-Parentheses), Up: [M4 Quotation](#M4-Quotation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.1.8 Quotation Rule Of Thumb [¶](#Quotation-Rule-Of-Thumb-1)

To conclude, the quotation rule of thumb is:

*One pair of quotes per pair of parentheses.*

Never over-quote, never under-quote, in particular in the definition of
macros. In the few places where the macros need to use brackets
(usually in C program text or regular expressions), properly quote
*the arguments*!

It is common to read Autoconf programs with snippets like:

```
AC_TRY_LINK(
changequote(<<, >>)dnl
<<#include <time.h>
#ifndef tzname /* For SGI.  */
extern char *tzname[]; /* RS6000 and others reject char **tzname.  */
#endif>>,
changequote([, ])dnl
[atoi (*tzname);], ac_cv_var_tzname=yes, ac_cv_var_tzname=no)
```

which is incredibly useless since `AC_TRY_LINK` is *already*
double quoting, so you just need:

```
AC_TRY_LINK(
[#include <time.h>
#ifndef tzname /* For SGI.  */
extern char *tzname[]; /* RS6000 and others reject char **tzname.  */
#endif],
            [atoi (*tzname);],
            [ac_cv_var_tzname=yes],
            [ac_cv_var_tzname=no])
```

The M4-fluent reader might note that these two examples are rigorously
equivalent, since M4 swallows both the ‘`changequote(<<, >>)`’
and ‘`<<`’ ‘`>>`’ when it *collects* the arguments: these
quotes are not part of the arguments!

Simplified, the example above is just doing this:

```
changequote(<<, >>)dnl
<<[]>>
changequote([, ])dnl
```

instead of simply:

```
[[]]
```

With macros that do not double quote their arguments (which is the
rule), double-quote the (risky) literals:

```
AC_LINK_IFELSE([AC_LANG_PROGRAM(
[[#include <time.h>
#ifndef tzname /* For SGI.  */
extern char *tzname[]; /* RS6000 and others reject char **tzname.  */
#endif]],
                                [atoi (*tzname);])],
               [ac_cv_var_tzname=yes],
               [ac_cv_var_tzname=no])
```

Please note that the macro `AC_TRY_LINK` is obsolete, so you really
should be using `AC_LINK_IFELSE` instead.

See [Quadrigraphs](#Quadrigraphs), for what to do if you run into a hopeless case
where quoting does not suffice.

When you create a `configure` script using newly written macros,
examine it carefully to check whether you need to add more quotes in
your macros. If one or more words have disappeared in the M4
output, you need more quotes. When in doubt, quote.

However, it’s also possible to put on too many layers of quotes. If
this happens, the resulting `configure` script may contain
unexpanded macros. The `autoconf` program checks for this problem
by looking for the string ‘`AC_`’ in `configure`. However, this
heuristic does not work in general: for example, it does not catch
overquoting in `AC_DEFINE` descriptions.

---

Next: [Programming in M4sugar](#Programming-in-M4sugar), Previous: [M4 Quotation](#M4-Quotation), Up: [Programming in M4](#Programming-in-M4)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 8.2 Using `autom4te` [¶](#Using-autom4te-1)

The Autoconf suite, including M4sugar, M4sh, and Autotest, in addition
to Autoconf per se, heavily rely on M4. All these different uses
revealed common needs factored into a layer over M4:
`autom4te`[5](#FOOT5).

`autom4te` is a preprocessor that is like `m4`.
It supports M4 extensions designed for use in tools like Autoconf.

* [Invoking `autom4te`](#autom4te-Invocation)
* [Customizing `autom4te`](#Customizing-autom4te)

---

Next: [Customizing `autom4te`](#Customizing-autom4te), Up: [Using `autom4te`](#Using-autom4te)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.2.1 Invoking `autom4te` [¶](#Invoking-autom4te)

The command line arguments are modeled after M4’s:

```
autom4te options files
```

where the files are directly passed to `m4`. By default,
GNU M4 is found during configuration, but the environment
variable
`M4` can be set to tell `autom4te` where to look. In addition
to the regular expansion, it handles the replacement of the quadrigraphs
(see [Quadrigraphs](#Quadrigraphs)), and of ‘`__oline__`’, the current line in the
output. It supports an extended syntax for the files:

`file.m4f`
:   This file is an M4 frozen file. Note that *all the previous files
    are ignored*. See the `--melt` option for the rationale.

`file?`
:   If found in the library path, the file is included for expansion,
    otherwise it is ignored instead of triggering a failure.

  

Of course, it supports the Autoconf common subset of options:

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
:   Don’t remove the temporary files and be even more verbose.

`--include=dir`

`-I dir`
:   Also look for input files in dir. Multiple invocations
    accumulate.

`--output=file`

`-o file`
:   Save output (script or trace) to file. The file `-` stands
    for the standard output.

  

As an extension of `m4`, it includes the following options:

`--warnings=category[,category...]` [¶](#index-WARNINGS-3)

`-Wcategory[,category...]`
:   Enable or disable warnings related to each category.
    See [m4\_warn](#m4_005fwarn), for a comprehensive list of categories.
    Special values include:

    ‘`all`’
    :   Enable all categories of warnings.

    ‘`none`’
    :   Disable all categories of warnings.

    ‘`error`’
    :   Treat all warnings as errors.

    ‘`no-category`’
    :   Disable warnings falling into category.

    The environment variable `WARNINGS` may also be set to a
    comma-separated list of warning categories to enable or disable.
    It is interpreted exactly the same way as the argument of
    `--warnings`, but unknown categories are silently ignored.
    The command line takes precedence; for instance, if `WARNINGS`
    is set to `obsolete`, but `-Wnone` is given on the
    command line, no warnings will be issued.

    Some categories of warnings are on by default.
    Again, for details see [m4\_warn](#m4_005fwarn).

`--melt`

`-M`
:   Do not use frozen files. Any argument `file.m4f` is
    replaced by `file.m4`. This helps tracing the macros which
    are executed only when the files are frozen, typically
    `m4_define`. For instance, running:

    ```
    autom4te --melt 1.m4 2.m4f 3.m4 4.m4f input.m4
    ```

    is roughly equivalent to running:

    ```
    m4 1.m4 2.m4 3.m4 4.m4 input.m4
    ```

    while

    ```
    autom4te 1.m4 2.m4f 3.m4 4.m4f input.m4
    ```

    is equivalent to:

    ```
    m4 --reload-state=4.m4f input.m4
    ```

`--freeze`

`-F`
:   Produce a frozen state file. `autom4te` freezing is stricter
    than M4’s: it must produce no warnings, and no output other than empty
    lines (a line with white space is *not* empty) and comments
    (starting with ‘`#`’). Unlike `m4`’s similarly-named option,
    this option takes no argument:

    ```
    autom4te 1.m4 2.m4 3.m4 --freeze --output=3.m4f
    ```

    corresponds to

    ```
    m4 1.m4 2.m4 3.m4 --freeze-state=3.m4f
    ```

`--mode=octal-mode`

`-m octal-mode`
:   Set the mode of the non-traces output to octal-mode; by default
    ‘`0666`’.

  

As another additional feature over `m4`, `autom4te`
caches its results. GNU M4 is able to produce a regular
output and traces at the same time. Traces are heavily used in the
GNU Build System: `autoheader` uses them to build
`config.h.in`, `autoreconf` to determine what
GNU Build System components are used, `automake` to
“parse” `configure.ac` etc. To avoid recomputation,
traces are cached while performing regular expansion,
and conversely. This cache is (actually, the caches are) stored in
the directory `autom4te.cache`. *It can safely be removed*
at any moment (especially if for some reason `autom4te`
considers it trashed).

`--cache=directory`

`-C directory`
:   Specify the name of the directory where the result should be cached.
    Passing an empty value disables caching. Be sure to pass a relative
    file name, as for the time being, global caches are not supported.

`--no-cache`
:   Don’t cache the results.

`--force`

`-f`
:   If a cache is used, consider it obsolete (but update it anyway).

  

Because traces are so important to the GNU Build System,
`autom4te` provides high level tracing features as compared to
M4, and helps exploiting the cache:

`--trace=macro[:format]`

`-t macro[:format]`
:   Trace the invocations of macro according to the format.
    Multiple `--trace` arguments can be used to list several macros.
    Multiple `--trace` arguments for a single macro are not
    cumulative; instead, you should just make format as long as
    needed.

    The format is a regular string, with newlines if desired, and
    several special escape codes. It defaults to ‘`$f:$l:$n:$%`’. It can
    use the following special escapes:

    ‘`$$`’
    :   The character ‘`$`’.

    ‘`$f`’
    :   The file name from which macro is called.

    ‘`$l`’
    :   The line number from which macro is called.

    ‘`$d`’
    :   The depth of the macro call. This is an M4 technical detail that
        you probably don’t want to know about.

    ‘`$n`’
    :   The name of the macro.

    ‘`$num`’
    :   The numth argument of the call to macro.

    ‘`$@`’

    ‘`$sep@`’

    ‘`${separator}@`’
    :   All the arguments passed to macro, separated by the character
        sep or the string separator (‘`,`’ by default). Each
        argument is quoted, i.e., enclosed in a pair of square brackets.

    ‘`$*`’

    ‘`$sep*`’

    ‘`${separator}*`’
    :   As above, but the arguments are not quoted.

    ‘`$%`’

    ‘`$sep%`’

    ‘`${separator}%`’
    :   As above, but the arguments are not quoted, all new line characters in
        the arguments are smashed, and the default separator is ‘`:`’.

        The escape ‘`$%`’ produces single-line trace outputs (unless you put
        newlines in the ‘`separator`’), while ‘`$@`’ and ‘`$*`’ do
        not.

    See [Using `autoconf` to Create `configure`](#autoconf-Invocation), for examples of trace uses.

`--preselect=macro`

`-p macro`
:   Cache the traces of macro, but do not enable traces. This is
    especially important to save CPU cycles in the future. For instance,
    when invoked, `autoconf` pre-selects all the macros that
    `autoheader`, `automake`, `autoreconf`, etc.,
    trace, so that running `m4` is not needed to trace them: the
    cache suffices. This results in a huge speed-up.

  

Finally, `autom4te` introduces the concept of *Autom4te
libraries*. They consists in a powerful yet extremely simple feature:
sets of combined command line arguments:

`--language=language`

`-l language`
:   Use the language Autom4te library. Current languages include:

    `M4sugar`
    :   create M4sugar output.

    `M4sh`
    :   create M4sh executable shell scripts.

    `Autotest`
    :   create Autotest executable test suites.

    `Autoconf-without-aclocal-m4`
    :   create Autoconf executable configure scripts without
        reading `aclocal.m4`.

    `Autoconf`
    :   create Autoconf executable configure scripts. This language inherits
        all the characteristics of `Autoconf-without-aclocal-m4` and
        additionally reads `aclocal.m4`.

`--prepend-include=dir`

`-B dir`
:   Prepend directory dir to the search path. This is used to include
    the language-specific files before any third-party macros.

As an example, if Autoconf is installed in its default location,
`/usr/local`, the command ‘`autom4te -l m4sugar foo.m4`’ is
strictly equivalent to the command:

```
autom4te --prepend-include /usr/local/share/autoconf \
  m4sugar/m4sugar.m4f foo.m4
```

Recursive expansion applies here: the command ‘`autom4te -l m4sh foo.m4`’
is the same as ‘`autom4te --language M4sugar m4sugar/m4sh.m4f
foo.m4`’, i.e.:

```
autom4te --prepend-include /usr/local/share/autoconf \
  m4sugar/m4sugar.m4f m4sugar/m4sh.m4f --mode 777 foo.m4
```

The definition of the languages is stored in `autom4te.cfg`.

---

Previous: [Invoking `autom4te`](#autom4te-Invocation), Up: [Using `autom4te`](#Using-autom4te)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.2.2 Customizing `autom4te` [¶](#Customizing-autom4te-1)

One can customize `autom4te` via `~/.autom4te.cfg` (i.e.,
as found in the user home directory), and `./.autom4te.cfg` (i.e.,
as found in the directory from which `autom4te` is run). The
order is first reading `autom4te.cfg`, then `~/.autom4te.cfg`,
then `./.autom4te.cfg`, and finally the command line arguments.

In these text files, comments are introduced with `#`, and empty
lines are ignored. Customization is performed on a per-language basis,
wrapped in between a ‘`begin-language: "language"`’,
‘`end-language: "language"`’ pair.

Customizing a language stands for appending options (see [Invoking `autom4te`](#autom4te-Invocation)) to the current definition of the language. Options, and
more generally arguments, are introduced by ‘`args:
arguments`’. You may use the traditional shell syntax to quote the
arguments.

As an example, to disable Autoconf caches (`autom4te.cache`)
globally, include the following lines in `~/.autom4te.cfg`:

```
## ------------------ ##
## User Preferences.  ##
## ------------------ ##

begin-language: "Autoconf-without-aclocal-m4"
args: --no-cache
end-language: "Autoconf-without-aclocal-m4"
```

---

Next: [Debugging via autom4te](#Debugging-via-autom4te), Previous: [Using `autom4te`](#Using-autom4te), Up: [Programming in M4](#Programming-in-M4)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
