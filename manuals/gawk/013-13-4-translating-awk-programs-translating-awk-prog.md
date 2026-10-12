### 13.4 Translating `awk` Programs [¶](#Translating-awk-Programs)

Once a program’s translatable strings have been marked, they must
be extracted to create the initial `.pot` file.
As part of translation, it is often helpful to rearrange the order
in which arguments to `printf` are output.

`gawk`’s `--gen-pot` command-line option extracts
the messages and is discussed next.
After that, `printf`’s ability to
rearrange the order for `printf` arguments at runtime
is covered.

* [Extracting Marked Strings](#String-Extraction)
* [Rearranging `printf` Arguments](#Printf-Ordering)
* [`awk` Portability Issues](#I18N-Portability)

---

Next: [Rearranging `printf` Arguments](#Printf-Ordering), Up: [Translating `awk` Programs](#Translator-i18n)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 13.4.1 Extracting Marked Strings [¶](#Extracting-Marked-Strings)

Once your `awk` program is working, and all the strings have
been marked and you’ve set (and perhaps bound) the text domain,
it is time to produce translations.
First, use the `--gen-pot` command-line option to create
the initial `.pot` file:

```
gawk --gen-pot -f guide.awk > guide.pot
```

When run with `--gen-pot`, `gawk` does not execute your
program. Instead, it parses it as usual and prints all marked strings
to standard output in the format of a GNU `gettext` Portable Object
file. Also included in the output are any constant strings that
appear as the first argument to `dcgettext()` or as the first and
second argument to `dcngettext()`.[94](#FOOT94)
You should distribute the generated `.pot` file with
your `awk` program; translators will eventually use it
to provide you translations that you can also then distribute.
See [A Simple Internationalization Example](#I18N-Example)
for the full list of steps to go through to create and test
translations for `guide`.

---

Next: [`awk` Portability Issues](#I18N-Portability), Previous: [Extracting Marked Strings](#String-Extraction), Up: [Translating `awk` Programs](#Translator-i18n)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 13.4.2 Rearranging `printf` Arguments [¶](#Rearranging-printf-Arguments)

Format strings for `printf` and `sprintf()`
(see [Using `printf` Statements for Fancier Printing](#Printf))
present a special problem for translation.
Consider the following:[95](#FOOT95)

```
printf(_"String `%s' has %d characters\n",
          string, length(string)))
```

A possible German translation for this might be:

```
"%d Zeichen lang ist die Zeichenkette `%s'\n"
```

The problem should be obvious: the order of the format
specifications is different from the original!
Even though `gettext()` can return the translated string
at runtime,
it cannot change the argument order in the call to `printf`.

To solve this problem, `printf` format specifiers may have
an additional optional element, which we call a *positional specifier*.
For example:

```
"%2$d Zeichen lang ist die Zeichenkette `%1$s'\n"
```

Here, the positional specifier consists of an integer count, which indicates which
argument to use, and a ‘`$`’. Counts are one-based, and the
format string itself is *not* included. Thus, in the following
example, ‘`string`’ is the first argument and ‘`length(string)`’ is the second:

```
$ gawk 'BEGIN {
>     string = "Don\47t Panic"
>     printf "%2$d characters live in \"%1$s\"\n",
>                         string, length(string)
> }'
-| 11 characters live in "Don't Panic"
```

If present, positional specifiers come first in the format specification,
before the flags, the field width, and/or the precision.

Positional specifiers can be used with the dynamic field width and
precision capability:

```
$ gawk 'BEGIN {
>    printf("%*.*s\n", 10, 20, "hello")
>    printf("%3$*2$.*1$s\n", 20, 10, "hello")
> }'
-|      hello
-|      hello
```

> **NOTE:** When using ‘`*`’ with a positional specifier, the ‘`*`’
> comes first, then the integer position, and then the ‘`$`’.
> This is somewhat counterintuitive.

`gawk` does not allow you to mix regular format specifiers
and those with positional specifiers in the same string:

```
$ gawk 'BEGIN { printf "%d %3$s\n", 1, 2, "hi" }'
error→ gawk: cmd. line:1: fatal: must use `count$' on all formats or none
```

> **NOTE:** There are some pathological cases that `gawk` may fail to
> diagnose. In such cases, the output may not be what you expect.
> It’s still a bad idea to try mixing them, even if `gawk`
> doesn’t detect it.

Although positional specifiers can be used directly in `awk` programs,
their primary purpose is to help in producing correct translations of
format strings into languages different from the one in which the program
is first written.

---

Previous: [Rearranging `printf` Arguments](#Printf-Ordering), Up: [Translating `awk` Programs](#Translator-i18n)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 13.4.3 `awk` Portability Issues [¶](#awk-Portability-Issues)

`gawk`’s internationalization features were purposely chosen to
have as little impact as possible on the portability of `awk`
programs that use them to other versions of `awk`.
Consider this program:

```
BEGIN {
    TEXTDOMAIN = "guide"
    if (Test_Guide)   # set with -v
        bindtextdomain("/test/guide/messages")
    print _"don't panic!"
}
```

As written, it won’t work on other versions of `awk`.
However, it is actually almost portable, requiring very little
change:

* Assignments to `TEXTDOMAIN` won’t have any effect,
  because `TEXTDOMAIN` is not special in other `awk` implementations.
* Non-GNU versions of `awk` treat marked strings
  as the concatenation of a variable named `_` with the string
  following it.[96](#FOOT96) Typically, the variable `_` has
  the null string (`""`) as its value, leaving the original string constant as
  the result.
* By defining “dummy” functions to replace `dcgettext()`, `dcngettext()`,
  and `bindtextdomain()`, the `awk` program can be made to run, but
  all the messages are output in the original language.
  For example:

  ```
  function bindtextdomain(dir, domain)
  {
      return dir
  }

  function dcgettext(string, domain, category)
  {
      return string
  }

  function dcngettext(string1, string2, number, domain, category)
  {
      return (number == 1 ? string1 : string2)
  }
  ```
* The use of positional specifications in `printf` or
  `sprintf()` is *not* portable.
  To support `gettext()` at the C level, many systems’ C versions of
  `sprintf()` do support positional specifiers. But it works only if
  enough arguments are supplied in the function call. Many versions of
  `awk` pass `printf` formats and arguments unchanged to the
  underlying C library version of `sprintf()`, but only one format and
  argument at a time. What happens if a positional specification is
  used is anybody’s guess.
  However, because the positional specifications are primarily for use in
  *translated* format strings, and because non-GNU `awk`s never
  retrieve the translated string, this should not be a problem in practice.

---

Next: [`gawk` Can Speak Your Language](#Gawk-I18N), Previous: [Translating `awk` Programs](#Translator-i18n), Up: [Internationalization with `gawk`](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.5 A Simple Internationalization Example [¶](#A-Simple-Internationalization-Example)

Now let’s look at a step-by-step example of how to internationalize and
localize a simple `awk` program, using `guide.awk` as our
original source:

```
BEGIN {
    TEXTDOMAIN = "guide"
    bindtextdomain(".")  # for testing
    print _"Don't Panic"
    print _"The Answer Is", 42
    print "Pardon me, Zaphod who?"
}
```

Run ‘`gawk --gen-pot`’ to create the `.pot` file:

```
$ gawk --gen-pot -f guide.awk > guide.pot
```

This produces:

```
#: guide.awk:4
msgid "Don't Panic"
msgstr ""

#: guide.awk:5
msgid "The Answer Is"
msgstr ""
```

This original portable object template file is saved and reused for each language
into which the application is translated. The `msgid`
is the original string and the `msgstr` is the translation.

> **NOTE:** Strings not marked with a leading underscore do not
> appear in the `guide.pot` file.

Next, the messages must be translated.
Here is a translation to a hypothetical dialect of English,
called “Mellow”:[97](#FOOT97)

```
$ cp guide.pot guide-mellow.po
Add translations to guide-mellow.po ...
```

Following are the translations:

```
#: guide.awk:4
msgid "Don't Panic"
msgstr "Hey man, relax!"

#: guide.awk:5
msgid "The Answer Is"
msgstr "Like, the scoop is"
```

> **NOTE:** The following instructions apply to GNU/Linux with the GNU C Library. Be
> aware that the actual steps may change over time, that the following
> description may not be accurate for all GNU/Linux distributions, and
> that things may work entirely differently on other operating systems.

The next step is to make the directory to hold the binary message object
file and then to create the `guide.mo` file.
The directory has the form `locale/LC_MESSAGES`, where
locale is a locale name known to the C `gettext` routines.

How do we know which locale to use? It turns out that there are
four different environment variables used by the C `gettext` routines.
In order, they are `$LANGUAGE`, `$LC_ALL`, `$LANG`, and
`$LC_MESSAGES`.[98](#FOOT98)
Thus, we check the value of `$LANGUAGE`:

```
$ echo $LANGUAGE
-| en_US.UTF-8
```

We next make the directories:

```
$ mkdir en_US.UTF-8 en_US.UTF-8/LC_MESSAGES
```

The `msgfmt` utility converts the human-readable
`.po` file into a machine-readable `.mo` file.
By default, `msgfmt` creates a file named `messages`.
This file must be renamed and placed in the proper directory (using
the `-o` option) so that `gawk` can find it:

```
$ msgfmt guide-mellow.po -o en_US.UTF-8/LC_MESSAGES/guide.mo
```

Finally, we run the program to test it:

```
$ gawk -f guide.awk
-| Hey man, relax!
-| Like, the scoop is 42
-| Pardon me, Zaphod who?
```

If the three replacement functions for `dcgettext()`, `dcngettext()`,
and `bindtextdomain()`
(see [`awk` Portability Issues](#I18N-Portability))
are in a file named `libintl.awk`,
then we can run `guide.awk` unchanged as follows:

```
$ gawk --posix -f guide.awk -f libintl.awk
-| Don't Panic
-| The Answer Is 42
-| Pardon me, Zaphod who?
```

---

Next: [Summary](#I18N-Summary), Previous: [A Simple Internationalization Example](#I18N-Example), Up: [Internationalization with `gawk`](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.6 `gawk` Can Speak Your Language [¶](#gawk-Can-Speak-Your-Language)

`gawk` itself has been internationalized
using the GNU `gettext` package.
(GNU `gettext` is described in
complete detail in
[GNU `gettext` utilities](https://www.gnu.org/software/gettext/manual/).)
As of this writing, the latest version of GNU `gettext` is
[version 0.19.8.1](ftp://ftp.gnu.org/gnu/gettext/gettext-0.19.8.1.tar.gz).

If a translation of `gawk`’s messages exists,
then `gawk` produces usage messages, warnings,
and fatal errors in the local language.

---

Previous: [`gawk` Can Speak Your Language](#Gawk-I18N), Up: [Internationalization with `gawk`](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.7 Summary [¶](#Summary-12)

* Internationalization means writing a program such that it can use multiple
  languages without requiring source code changes. Localization means
  providing the data necessary for an internationalized program to work
  in a particular language.
* `gawk` uses GNU `gettext` to let you internationalize
  and localize `awk` programs. A program’s text domain identifies
  the program for grouping all messages and other data together.
* You mark a program’s strings for translation by preceding them with
  an underscore. Once that is done, the strings are extracted into a
  `.pot` file. This file is copied for each language into a `.po`
  file, and the `.po` files are compiled into `.gmo` files for
  use at runtime.
* You can use positional specifications with `sprintf()` and
  `printf` to rearrange the placement of argument values in formatted
  strings and output. This is useful for the translation of format
  control strings.
* The internationalization features have been designed so that they
  can be easily worked around in a standard `awk`.
* `gawk` itself has been internationalized and ships with
  a number of translations for its messages.

---

Next: [Namespaces in `gawk`](#Namespaces), Previous: [Internationalization with `gawk`](#Internationalization), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 14 Debugging `awk` Programs [¶](#Debugging-awk-Programs)

It would be nice if computer programs worked perfectly the first time they
were run, but in real life, this rarely happens for programs of
any complexity. Thus, most programming languages have facilities available
for “debugging” programs, and `awk` is no exception.

The `gawk` debugger is purposely modeled after
[the GNU Debugger (GDB)](https://www.gnu.org/software/gdb/)
command-line debugger. If you are familiar with GDB, learning
how to use `gawk` for debugging your programs is easy.

* [Introduction to the `gawk` Debugger](#Debugging)
* [Sample `gawk` Debugging Session](#Sample-Debugging-Session)
* [Main Debugger Commands](#List-of-Debugger-Commands)
* [Readline Support](#Readline-Support)
* [Limitations](#Limitations)
* [Summary](#Debugging-Summary)

---

Next: [Sample `gawk` Debugging Session](#Sample-Debugging-Session), Up: [Debugging `awk` Programs](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.1 Introduction to the `gawk` Debugger [¶](#Introduction-to-the-gawk-Debugger)

This section introduces debugging in general and begins
the discussion of debugging in `gawk`.

* [Debugging in General](#Debugging-Concepts)
* [Debugging Concepts](#Debugging-Terms)
* [`awk` Debugging](#Awk-Debugging)

---

Next: [Debugging Concepts](#Debugging-Terms), Up: [Introduction to the `gawk` Debugger](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.1.1 Debugging in General [¶](#Debugging-in-General)

(If you have used debuggers in other languages, you may want to skip
ahead to [`awk` Debugging](#Awk-Debugging).)

Of course, a debugging program cannot remove bugs for you, because it has
no way of knowing what you or your users consider a “bug” versus a
“feature.” (Sometimes, we humans have a hard time with this ourselves.)
In that case, what can you expect from such a tool? The answer to that
depends on the language being debugged, but in general, you can expect at
least the following:

* The ability to watch a program execute its instructions one by one,
  giving you, the programmer, the opportunity to think about what is happening
  on a time scale of seconds, minutes, or hours, rather than the nanosecond
  time scale at which the code usually runs.
* The opportunity to not only passively observe the operation of your
  program, but to control it and try different paths of execution, without
  having to change your source files.
* The chance to see the values of data in the program at any point in
  execution, and also to change that data on the fly, to see how that
  affects what happens afterward. (This often includes the ability
  to look at internal data structures besides the variables you actually
  defined in your code.)
* The ability to obtain additional information about your program’s state
  or even its internal structure.

All of these tools provide a great amount of help in using your own
skills and understanding of the goals of your program to find where it
is going wrong (or, for that matter, to better comprehend a perfectly
functional program that you or someone else wrote).

---

Next: [`awk` Debugging](#Awk-Debugging), Previous: [Debugging in General](#Debugging-Concepts), Up: [Introduction to the `gawk` Debugger](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.1.2 Debugging Concepts [¶](#Debugging-Concepts-1)

Before diving in to the details, we need to introduce several
important concepts that apply to just about all debuggers.
The following list defines terms used throughout the rest of
this chapter:

*Stack frame* [¶](#index-stack-frame-_0028debugger_0029)
:   Programs generally call functions during the course of their execution.
    One function can call another, or a function can call itself (recursion).
    You can view the chain of called functions (main program calls A, which
    calls B, which calls C), as a stack of executing functions: the currently
    running function is the topmost one on the stack, and when it finishes
    (returns), the next one down then becomes the active function.
    Such a stack is termed a *call stack*.

    For each function on the call stack, the system maintains a data area
    that contains the function’s parameters, local variables, and return value,
    as well as any other “bookkeeping” information needed to manage the
    call stack. This data area is termed a *stack frame*.

    `gawk` also follows this model, and gives you
    access to the call stack and to each stack frame. You can see the
    call stack, as well as from where each function on the stack was
    invoked. Commands that print the call stack print information about
    each stack frame (as detailed later on).

*Breakpoint* [¶](#index-breakpoint)
:   During debugging, you often wish to let the program run until it
    reaches a certain point, and then continue execution from there one
    statement (or instruction) at a time. The way to do this is to set
    a *breakpoint* within the program. A breakpoint is where the
    execution of the program should break off (stop), so that you can
    take over control of the program’s execution. You can add and remove
    as many breakpoints as you like.

*Watchpoint* [¶](#index-watchpoint-_0028debugger_0029)
:   A watchpoint is similar to a breakpoint. The difference is that
    breakpoints are oriented around the code: stop when a certain point in the
    code is reached. A watchpoint, however, specifies that program execution
    should stop when a *data value* is changed. This is useful, as
    sometimes it happens that a variable receives an erroneous value, and it’s
    hard to track down where this happens just by looking at the code.
    By using a watchpoint, you can stop whenever a variable is assigned to,
    and usually find the errant code quite quickly.

---

Previous: [Debugging Concepts](#Debugging-Terms), Up: [Introduction to the `gawk` Debugger](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.1.3 `awk` Debugging [¶](#awk-Debugging)

Debugging an `awk` program has some specific aspects that are
not shared with programs written in other languages.

First of all, the fact that `awk` programs usually take input
line by line from a file or files and operate on those lines using specific
rules makes it especially useful to organize viewing the execution of
the program in terms of these rules. As we will see, each `awk`
rule is treated almost like a function call, with its own specific block
of instructions.

In addition, because `awk` is by design a very concise language,
it is easy to lose sight of everything that is going on “inside”
each line of `awk` code. The debugger provides the opportunity
to look at the individual primitive instructions carried out
by the higher-level `awk` commands.[99](#FOOT99)

---

Next: [Main Debugger Commands](#List-of-Debugger-Commands), Previous: [Introduction to the `gawk` Debugger](#Debugging), Up: [Debugging `awk` Programs](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.2 Sample `gawk` Debugging Session [¶](#Sample-gawk-Debugging-Session)

In order to illustrate the use of `gawk` as a debugger, let’s look at a sample
debugging session. We will use the `awk` implementation of the
POSIX `uniq` command presented earlier (see [Printing Nonduplicated Lines of Text](#Uniq-Program))
as our example.

* [How to Start the Debugger](#Debugger-Invocation)
* [Finding the Bug](#Finding-The-Bug)

---

Next: [Finding the Bug](#Finding-The-Bug), Up: [Sample `gawk` Debugging Session](#Sample-Debugging-Session)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.2.1 How to Start the Debugger [¶](#How-to-Start-the-Debugger)

Starting the debugger is almost exactly like running `gawk` normally,
except you have to pass an additional option, `--debug`, or the
corresponding short option, `-D`. The file(s) containing the
program and any supporting code are given on the command line as arguments
to one or more `-f` options. (`gawk` is not designed
to debug command-line programs, only programs contained in files.)
In our case, we invoke the debugger like this:

```
$ gawk -D -f getopt.awk -f join.awk -f uniq.awk -- -1 inputfile
```

where both `getopt.awk` and `uniq.awk` are in `$AWKPATH`.
(Experienced users of GDB or similar debuggers should note that
this syntax is slightly different from what you are used to.
With the `gawk` debugger, you give the arguments for running the program
in the command line to the debugger rather than as part of the `run`
command at the debugger prompt.)
The `--` ends `gawk`’s command line options. It’s not
strictly necessary here, but it is needed if an option to the `awk`
program conflicts with a `gawk` option.
The `-1` is an option to `uniq.awk`.

Instead of immediately running the program on `inputfile`, as
`gawk` would ordinarily do, the debugger merely loads all
the program source files, compiles them internally, and then gives
us a prompt:

```
gawk>
```

from which we can issue commands to the debugger. At this point, no
code has been executed.

---

Previous: [How to Start the Debugger](#Debugger-Invocation), Up: [Sample `gawk` Debugging Session](#Sample-Debugging-Session)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.2.2 Finding the Bug [¶](#Finding-the-Bug)

Let’s say that we are having a problem using (a faulty version of)
`uniq.awk` in “field-skipping” mode, and it doesn’t seem to be
catching lines which should be identical when skipping the first field,
such as:

```
awk is a wonderful program!
gawk is a wonderful program!
```

This could happen if we were thinking (C-like) of the fields in a record
as being numbered in a zero-based fashion, so instead of the lines:

```
clast = join(alast, fcount+1, n)
cline = join(aline, fcount+1, m)
```

we wrote:

```
clast = join(alast, fcount, n)
cline = join(aline, fcount, m)
```

The first thing we usually want to do when trying to investigate a
problem like this is to put a breakpoint in the program so that we can
watch it at work and catch what it is doing wrong. A reasonable spot for
a breakpoint in `uniq.awk` is at the beginning of the function
`are_equal()`, which compares the current line with the previous one. To set
the breakpoint, use the `b` (breakpoint) command:

```
gawk> b are_equal
-| Breakpoint 1 set at file `awklib/eg/prog/uniq.awk', line 63
```

The debugger tells us the file and line number where the breakpoint is.
Now type ‘`r`’ or ‘`run`’ and the program runs until it hits
the breakpoint for the first time:

```
gawk> r
-| Starting program:
-| Stopping in Rule ...
-| Breakpoint 1, are_equal(n, m, clast, cline, alast, aline)
         at `awklib/eg/prog/uniq.awk':63
-| 63          if (fcount == 0 && charcount == 0)
gawk>
```

Now we can look at what’s going on inside our program. First of all,
let’s see how we got to where we are. At the prompt, we type ‘`bt`’
(short for “backtrace”), and the debugger responds with a
listing of the current stack frames:

```
gawk> bt
-| #0  are_equal(n, m, clast, cline, alast, aline)
         at `awklib/eg/prog/uniq.awk':68
-| #1  in main() at `awklib/eg/prog/uniq.awk':88
```

This tells us that `are_equal()` was called by the main program at
line 88 of `uniq.awk`. (This is not a big surprise, because this
is the only call to `are_equal()` in the program, but in more complex
programs, knowing who called a function and with what parameters can be
the key to finding the source of the problem.)

Now that we’re in `are_equal()`, we can start looking at the values
of some variables. Let’s say we type ‘`p n`’
(`p` is short for “print”). We would expect to see the value of
`n`, a parameter to `are_equal()`. Actually, the debugger
gives us:

```
gawk> p n
-| n = untyped variable
```

In this case, `n` is an uninitialized local variable, because the
function was called without arguments (see [Function Calls](#Function-Calls)).

A more useful variable to display might be the current record:

```
gawk> p $0
-| $0 = "gawk is a wonderful program!"
```

This might be a bit puzzling at first, as this is the second line of
our test input. Let’s look at `NR`:

```
gawk> p NR
-| NR = 2
```

So we can see that `are_equal()` was only called for the second record
of the file. Of course, this is because our program contains a rule for
‘`NR == 1`’:

```
NR == 1 {
    last = $0
    next
}
```

OK, let’s just check that that rule worked correctly:

```
gawk> p last
-| last = "awk is a wonderful program!"
```

Everything we have done so far has verified that the program has worked as
planned, up to and including the call to `are_equal()`, so the problem must
be inside this function. To investigate further, we must begin
“stepping through” the lines of `are_equal()`. We start by typing
‘`n`’ (for “next”):

```
gawk> n
-| 66          if (fcount > 0) {
```

This tells us that `gawk` is now ready to execute line 66, which
decides whether to give the lines the special “field-skipping” treatment
indicated by the `-1` command-line option. (Notice that we skipped
from where we were before, at line 63, to here, because the condition
in line 63, ‘`if (fcount == 0 && charcount == 0)`’, was false.)

Continuing to step, we now get to the splitting of the current and
last records:

```
gawk> n
-| 67              n = split(last, alast)
gawk> n
-| 68              m = split($0, aline)
```

At this point, we should be curious to see what our records were split
into, so we try to look:

```
gawk> p n m alast aline
-| n = 5
-| m = untyped variable
-| alast = array, 5 elements
-| aline = untyped variable
```

(The `p` command can take more than one argument, similar to
`awk`’s `print` statement.)

This is kind of disappointing, though. All we found out is that there
are five elements in `alast`; `m` and `aline` don’t have
values because we are at line 68 but haven’t executed it yet.
This information is useful enough (we now know that
none of the words were accidentally left out), but what if we want to see
inside the array?

The first choice would be to use subscripts:

```
gawk> p alast[0]
-| "0" not in array `alast'
```

Oops!

```
gawk> p alast[1]
-| alast["1"] = "awk"
```

This would be kind of slow for a 100-member array, though, so
`gawk` provides a shortcut (reminiscent of another language
not to be mentioned):

```
gawk> p @alast
-| alast["1"] = "awk"
-| alast["2"] = "is"
-| alast["3"] = "a"
-| alast["4"] = "wonderful"
-| alast["5"] = "program!"
```

It looks like we got this far OK. Let’s take another step
or two:

```
gawk> n
-| 69              clast = join(alast, fcount, n)
gawk> n
-| 70              cline = join(aline, fcount, m)
```

Well, here we are at our error (sorry to spoil the suspense). What we
had in mind was to join the fields starting from the second one to make
the virtual record to compare, and if the first field were numbered zero,
this would work. Let’s look at what we’ve got:

```
gawk> p cline clast
-| cline = "gawk is a wonderful program!"
-| clast = "awk is a wonderful program!"
```

Hey, those look pretty familiar! They’re just our original, unaltered
input records. A little thinking (the human brain is still the best
debugging tool), and we realize that we were off by one!

We get out of the debugger:

```
gawk> q
-| The program is running. Exit anyway (y/n)? y
```

Then we get into an editor:

```
clast = join(alast, fcount+1, n)
cline = join(aline, fcount+1, m)
```

and problem solved!

---

Next: [Readline Support](#Readline-Support), Previous: [Sample `gawk` Debugging Session](#Sample-Debugging-Session), Up: [Debugging `awk` Programs](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.3 Main Debugger Commands [¶](#Main-Debugger-Commands)

The `gawk` debugger command set can be divided into the
following categories:

* Breakpoint control
* Execution control
* Viewing and changing data
* Working with the stack
* Getting information
* Miscellaneous

Each of these are discussed in the following subsections.
In the following descriptions, commands that may be abbreviated
show the abbreviation on a second description line.
A debugger command name may also be truncated if that partial
name is unambiguous. The debugger has the built-in capability to
automatically repeat the previous command just by hitting `Enter`.
This works for the commands `list`, `next`, `nexti`,
`step`, `stepi`, and `continue` executed without any
argument.

* [Control of Breakpoints](#Breakpoint-Control)
* [Control of Execution](#Debugger-Execution-Control)
* [Viewing and Changing Data](#Viewing-And-Changing-Data)
* [Working with the Stack](#Execution-Stack)
* [Obtaining Information About the Program and the Debugger State](#Debugger-Info)
* [Miscellaneous Commands](#Miscellaneous-Debugger-Commands)

---

Next: [Control of Execution](#Debugger-Execution-Control), Up: [Main Debugger Commands](#List-of-Debugger-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.3.1 Control of Breakpoints [¶](#Control-of-Breakpoints)

As we saw earlier, the first thing you probably want to do in a debugging
session is to get your breakpoints set up, because your program
will otherwise just run as if it was not under the debugger. The commands for
controlling breakpoints are:

`break` [[filename`:`]n | function] [`"expression"`]

`b` [[filename`:`]n | function] [`"expression"`]
:   Without any argument, set a breakpoint at the next instruction
    to be executed in the selected stack frame.
    Arguments can be one of the following:

    n
    :   Set a breakpoint at line number n in the current source file.

    filename`:`n
    :   Set a breakpoint at line number n in source file filename.

    function
    :   Set a breakpoint at entry to (the first instruction of)
        function function.

    Each breakpoint is assigned a number that can be used to delete it from
    the breakpoint list using the `delete` command.

    With a breakpoint, you may also supply a condition. This is an
    `awk` expression (enclosed in double quotes) that the debugger
    evaluates whenever the breakpoint is reached. If the condition is true,
    then the debugger stops execution and prompts for a command. Otherwise,
    it continues executing the program.

`clear` [[filename`:`]n | function]
:   Without any argument, delete any breakpoint at the next instruction
    to be executed in the selected stack frame. If the program stops at
    a breakpoint, this deletes that breakpoint so that the program
    does not stop at that location again. Arguments can be one of the following:

    n
    :   Delete breakpoint(s) set at line number n in the current source file.

    filename`:`n
    :   Delete breakpoint(s) set at line number n in source file filename.

    function
    :   Delete breakpoint(s) set at entry to function function.

`condition` n `"expression"`
:   Add a condition to existing breakpoint or watchpoint n. The
    condition is an `awk` expression *enclosed in double quotes*
    that the debugger evaluates
    whenever the breakpoint or watchpoint is reached. If the condition is true, then
    the debugger stops execution and prompts for a command. Otherwise,
    the debugger continues executing the program. If the condition expression is
    not specified, any existing condition is removed (i.e., the breakpoint or
    watchpoint is made unconditional).

`delete` [n1 n2 …] [n–m]

`d` [n1 n2 …] [n–m]
:   Delete specified breakpoints or a range of breakpoints. Delete
    all defined breakpoints if no argument is supplied.

`disable` [n1 n2 … | n–m]
:   Disable specified breakpoints or a range of breakpoints. Without
    any argument, disable all breakpoints.

`enable` [`del` | `once`] [n1 n2 …] [n–m] [¶](#index-enable-debugger-command)

`e` [`del` | `once`] [n1 n2 …] [n–m]
:   Enable specified breakpoints or a range of breakpoints. Without
    any argument, enable all breakpoints.
    Optionally, you can specify how to enable the breakpoints:

    `del`
    :   Enable the breakpoints temporarily, then delete each one when
        the program stops at it.

    `once`
    :   Enable the breakpoints temporarily, then disable each one when
        the program stops at it.

`ignore` n count [¶](#index-ignore-debugger-command)
:   Ignore breakpoint number n the next count times it is
    hit.

`tbreak` [[filename`:`]n | function] [¶](#index-tbreak-debugger-command)

`t` [[filename`:`]n | function]
:   Set a temporary breakpoint (enabled for only one stop).
    The arguments are the same as for `break`.

---

Next: [Viewing and Changing Data](#Viewing-And-Changing-Data), Previous: [Control of Breakpoints](#Breakpoint-Control), Up: [Main Debugger Commands](#List-of-Debugger-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.3.2 Control of Execution [¶](#Control-of-Execution)

Now that your breakpoints are ready, you can start running the program
and observing its behavior. There are more commands for controlling
execution of the program than we saw in our earlier example:

`commands` [n] [¶](#index-commands-to-execute-at-breakpoint)

`silent`

…

`end`
:   Set a list of commands to be executed upon stopping at
    a breakpoint or watchpoint. n is the breakpoint or watchpoint number.
    Without a number, the last one set is used. The actual commands follow,
    starting on the next line, and terminated by the `end` command.
    If the command `silent` is in the list, the usual messages about
    stopping at a breakpoint and the source line are not printed. Any command
    in the list that resumes execution (e.g., `continue`) terminates the list
    (an implicit `end`), and subsequent commands are ignored.
    For example:

    ```
    gawk> commands
    > silent
    > printf "A silent breakpoint; i = %d\n", i
    > info locals
    > set i = 10
    > continue
    > end
    gawk>
    ```

`continue` [count] [¶](#index-continue-program_002c-in-debugger)

`c` [count]
:   Resume program execution. If continued from a breakpoint and count is
    specified, ignore the breakpoint at that location the next count times
    before stopping.

`finish` [¶](#index-finish-debugger-command)
:   Execute until the selected stack frame returns.
    Print the returned value.

`next` [count] [¶](#index-next-debugger-command)

`n` [count]
:   Continue execution to the next source line, stepping over function calls.
    The argument count controls how many times to repeat the action, as
    in `step`.

`nexti` [count] [¶](#index-nexti-debugger-command)

`ni` [count]
:   Execute one (or count) instruction(s), stepping over function calls.

`return` [value] [¶](#index-return-debugger-command)
:   Cancel execution of a function call. If value (either a string or a
    number) is specified, it is used as the function’s return value. If used in a
    frame other than the innermost one (the currently executing function; i.e.,
    frame number 0), discard all inner frames in addition to the selected one,
    and the caller of that frame becomes the innermost frame.

`run` [¶](#index-run-debugger-command)

`r`
:   Start/restart execution of the program. When restarting, the debugger
    retains the current breakpoints, watchpoints, command history,
    automatic display variables, and debugger options.

`step` [count] [¶](#index-step-debugger-command)

`s` [count]
:   Continue execution until control reaches a different source line in the
    current stack frame, stepping inside any function called within
    the line. If the argument count is supplied, steps that many times before
    stopping, unless it encounters a breakpoint or watchpoint.

`stepi` [count] [¶](#index-stepi-debugger-command)

`si` [count]
:   Execute one (or count) instruction(s), stepping inside function calls.
    (For illustration of what is meant by an “instruction” in `gawk`,
    see the output shown under `dump` in [Miscellaneous Commands](#Miscellaneous-Debugger-Commands).)

`until` [[filename`:`]n | function] [¶](#index-until-debugger-command)

`u` [[filename`:`]n | function]
:   Without any argument, continue execution until a line past the current
    line in the current stack frame is reached. With an argument,
    continue execution until the specified location is reached, or the current
    stack frame returns.

---

Next: [Working with the Stack](#Execution-Stack), Previous: [Control of Execution](#Debugger-Execution-Control), Up: [Main Debugger Commands](#List-of-Debugger-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.3.3 Viewing and Changing Data [¶](#Viewing-and-Changing-Data)

The commands for viewing and changing variables inside of `gawk` are:

`display` [var | `$`n] [¶](#index-display-debugger-command)
:   Add variable var (or field `$n`) to the display list.
    The value of the variable or field is displayed each time the program stops.
    Each variable added to the list is identified by a unique number:

    ```
    gawk> display x
    -| 10: x = 1
    ```

    This displays the assigned item number, the variable name, and its current value.
    If the display variable refers to a function parameter, it is silently
    deleted from the list as soon as the execution reaches a context where
    no such variable of the given name exists.
    Without argument, `display` displays the current values of
    items on the list.

`eval "awk statements"` [¶](#index-eval-debugger-command)
:   Evaluate awk statements in the context of the running program.
    You can do anything that an `awk` program would do: assign
    values to variables, call functions, and so on.

    > **NOTE:** You cannot use `eval` to execute a statement containing
    > any of the following:
    > `exit`,
    > `getline`,
    > `next`,
    > `nextfile`,
    > or
    > `return`.

`eval` param, …

awk statements

`end`
:   This form of `eval` is similar, but it allows you to define
    “local variables” that exist in the context of the
    awk statements, instead of using variables or function
    parameters defined by the program.

`print` var1[`,` var2 …] [¶](#index-print-debugger-command)

`p` var1[`,` var2 …]
:   Print the value of a `gawk` variable or field.
    Fields must be referenced by constants:

    ```
    gawk> print $3
    ```

    This prints the third field in the input record (if the specified field does not
    exist, it prints ‘`Null field`’). A variable can be an array element, with
    the subscripts being constant string values. To print the contents of an array,
    prefix the name of the array with the ‘`@`’ symbol:

    ```
    gawk> print @a
    ```

    This prints the indices and the corresponding values for all elements in
    the array `a`.

`printf` format [`,` arg …] [¶](#index-printf-debugger-command)
:   Print formatted text. The format may include escape sequences,
    such as ‘`\n`’
    (see [Escape Sequences](#Escape-Sequences)).
    No newline is printed unless one is specified.

`set` var`=`value [¶](#index-set-debugger-command)
:   Assign a constant (number or string) value to an `awk` variable
    or field.
    String values must be enclosed between double quotes (`"`…`"`).

    You can also set special `awk` variables, such as `FS`,
    `NF`, `NR`, and so on.

`watch` var | `$`n [`"expression"`] [¶](#index-watch-debugger-command)

`w` var | `$`n [`"expression"`]
:   Add variable var (or field `$n`) to the watch list.
    The debugger then stops whenever
    the value of the variable or field changes. Each watched item is assigned a
    number that can be used to delete it from the watch list using the
    `unwatch` command.

    With a watchpoint, you may also supply a condition. This is an
    `awk` expression (enclosed in double quotes) that the debugger
    evaluates whenever the watchpoint is reached. If the condition is true,
    then the debugger stops execution and prompts for a command. Otherwise,
    `gawk` continues executing the program.

`undisplay` [n] [¶](#index-undisplay-debugger-command)
:   Remove item number n (or all items, if no argument) from the
    automatic display list.

`unwatch` [n] [¶](#index-unwatch-debugger-command)
:   Remove item number n (or all items, if no argument) from the
    watch list.

---

Next: [Obtaining Information About the Program and the Debugger State](#Debugger-Info), Previous: [Viewing and Changing Data](#Viewing-And-Changing-Data), Up: [Main Debugger Commands](#List-of-Debugger-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.3.4 Working with the Stack [¶](#Working-with-the-Stack)

Whenever you run a program that contains any function calls,
`gawk` maintains a stack of all of the function calls leading up
to where the program is right now. You can see how you got to where you are,
and also move around in the stack to see what the state of things was in the
functions that called the one you are in. The commands for doing this are:

`backtrace` [count] [¶](#index-traceback_002c-display-in-debugger)

`bt` [count]

`where` [count]
:   Print a backtrace of all function calls (stack frames), or innermost count
    frames if count > 0. Print the outermost count frames if
    count < 0. The backtrace displays the name and arguments to each
    function, the source file name, and the line number.
    The alias `where` for `backtrace` is provided for longtime
    GDB users who may be used to that command.

`down` [count] [¶](#index-down-debugger-command)
:   Move count (default 1) frames down the stack toward the innermost frame.
    Then select and print the frame.

`frame` [n] [¶](#index-frame-debugger-command)

`f` [n]
:   Select and print stack frame n. Frame 0 is the currently executing,
    or *innermost*, frame (function call); frame 1 is the frame that
    called the innermost one. The highest-numbered frame is the one for the
    main program. The printed information consists of the frame number,
    function and argument names, source file, and the source line.

`up` [count] [¶](#index-up-debugger-command)
:   Move count (default 1) frames up the stack toward the outermost frame.
    Then select and print the frame.

---

Next: [Miscellaneous Commands](#Miscellaneous-Debugger-Commands), Previous: [Working with the Stack](#Execution-Stack), Up: [Main Debugger Commands](#List-of-Debugger-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.3.5 Obtaining Information About the Program and the Debugger State [¶](#Obtaining-Information-About-the-Program-and-the-Debugger-State)

Besides looking at the values of variables, there is often a need to get
other sorts of information about the state of your program and of the
debugging environment itself. The `gawk` debugger has one command that
provides this information, appropriately called `info`. `info`
is used with one of a number of arguments that tell it exactly what
you want to know:

`info` what [¶](#index-info-debugger-command)

`i` what
:   The value for what should be one of the following:

    `args` [¶](#index-show-in-debugger-function-arguments)
    :   List arguments of the selected frame.

    `break` [¶](#index-show-in-debugger-breakpoints)
    :   List all currently set breakpoints.

    `display` [¶](#index-automatic-displays_002c-in-debugger)
    :   List all items in the automatic display list.

    `frame` [¶](#index-describe-call-stack-frame_002c-in-debugger)
    :   Give a description of the selected stack frame.

    `functions` [¶](#index-list-function-definitions_002c-in-debugger)
    :   List all function definitions including source file names and
        line numbers.

    `locals` [¶](#index-show-in-debugger-local-variables)
    :   List local variables of the selected frame.

    `source` [¶](#index-show-in-debugger-name-of-current-source-file)
    :   Print the name of the current source file. Each time the program stops, the
        current source file is the file containing the current instruction.
        When the debugger first starts, the current source file is the first file
        included via the `-f` option. The
        ‘`list filename:lineno`’ command can
        be used at any time to change the current source.

    `sources` [¶](#index-show-in-debugger-all-source-files)
    :   List all program sources.

    `variables` [¶](#index-list-all-global-variables_002c-in-debugger)
    :   List all global variables.

    `watch` [¶](#index-show-in-debugger-watchpoints)
    :   List all items in the watch list.

Additional commands give you control over the debugger, the ability to
save the debugger’s state, and the ability to run debugger commands
from a file. The commands are:

`option` [name[`=`value]]

`o` [name[`=`value]]
:   Without an argument, display the available debugger options
    and their current values. ‘`option name`’ shows the current
    value of the named option. ‘`option name=value`’ assigns
    a new value to the named option.
    The available options are:

    `history_size` [¶](#index-debugger-history-size)
    :   Set the maximum number of lines to keep in the history file
        `./.gawk_history`. The default is 100.

    `listsize` [¶](#index-debugger-default-list-amount)
    :   Specify the number of lines that `list` prints. The default is 15.

    `outfile` [¶](#index-redirect-gawk-output_002c-in-debugger)
    :   Send `gawk` output to a file; debugger output still goes
        to standard output. An empty string (`""`) resets output to
        standard output.

    `prompt` [¶](#index-debugger-prompt-1)
    :   Change the debugger prompt. The default is ‘`gawk>`’.

    `save_history` [`on` | `off`] [¶](#index-debugger-history-file)
    :   Save command history to file `./.gawk_history`.
        The default is `on`.

    `save_options` [`on` | `off`] [¶](#index-save-debugger-options)
    :   Save current options to file `./.gawkrc` upon exit.
        The default is `on`.
        Options are read back into the next session upon startup.

    `trace` [`on` | `off`] [¶](#index-instruction-tracing_002c-in-debugger)
    :   Turn instruction tracing on or off. The default is `off`.

`save` filename
:   Save the commands from the current session to the given file name,
    so that they can be replayed using the `source` command.

`source` filename [¶](#index-debugger-read-commands-from-a-file)
:   Run command(s) from a file; an error in any command does not
    terminate execution of subsequent commands. Comments (lines starting
    with ‘`#`’) are allowed in a command file.
    Empty lines are ignored; they do *not*
    repeat the last command.
    You can’t restart the program by having more than one `run`
    command in the file. Also, the list of commands may include additional
    `source` commands; however, the `gawk` debugger will not source the
    same file more than once in order to avoid infinite recursion.

    In addition to, or instead of, the `source` command, you can use
    the `-D file` or `--debug=file` command-line
    options to execute commands from a file non-interactively
    (see [Command-Line Options](#Options)).

---

Previous: [Obtaining Information About the Program and the Debugger State](#Debugger-Info), Up: [Main Debugger Commands](#List-of-Debugger-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.3.6 Miscellaneous Commands [¶](#Miscellaneous-Commands)

There are a few more commands that do not fit into the
previous categories, as follows:

`dump` [filename] [¶](#index-dump-debugger-command)
:   Dump byte code of the program to standard output or to the file
    named in filename. This prints a representation of the internal
    instructions that `gawk` executes to implement the `awk`
    commands in a program. This can be very enlightening, as the following
    partial dump of Davide Brini’s obfuscated code
    (see [And Now for Something Completely Different](#Signature-Program)) demonstrates:

    ```
    gawk> dump
    -|        # BEGIN
    -|
    -| [  1:0xfcd340] Op_rule           : [in_rule = BEGIN] [source_file = brini.awk]
    ```

    ```
    -| [  1:0xfcc240] Op_push_i         : "~" [MALLOC|STRING|STRCUR]
    -| [  1:0xfcc2a0] Op_push_i         : "~" [MALLOC|STRING|STRCUR]
    -| [  1:0xfcc280] Op_match          :
    -| [  1:0xfcc1e0] Op_store_var      : O
    -| [  1:0xfcc2e0] Op_push_i         : "==" [MALLOC|STRING|STRCUR]
    -| [  1:0xfcc340] Op_push_i         : "==" [MALLOC|STRING|STRCUR]
    -| [  1:0xfcc320] Op_equal          :
    -| [  1:0xfcc200] Op_store_var      : o
    -| [  1:0xfcc380] Op_push           : o
    -| [  1:0xfcc360] Op_plus_i         : 0 [MALLOC|NUMCUR|NUMBER]
    -| [  1:0xfcc220] Op_push_lhs       : o [do_reference = true]
    -| [  1:0xfcc300] Op_assign_plus    :
    -| [   :0xfcc2c0] Op_pop            :
    -| [  1:0xfcc400] Op_push           : O
    -| [  1:0xfcc420] Op_push_i         : "" [MALLOC|STRING|STRCUR]
    -| [   :0xfcc4a0] Op_no_op          :
    -| [  1:0xfcc480] Op_push           : O
    -| [   :0xfcc4c0] Op_concat         : [expr_count = 3] [concat_flag = 0]
    -| [  1:0xfcc3c0] Op_store_var      : x
    -| [  1:0xfcc440] Op_push_lhs       : X [do_reference = true]
    -| [  1:0xfcc3a0] Op_postincrement  :
    -| [  1:0xfcc4e0] Op_push           : x
    -| [  1:0xfcc540] Op_push           : o
    -| [  1:0xfcc500] Op_plus           :
    -| [  1:0xfcc580] Op_push           : o
    -| [  1:0xfcc560] Op_plus           :
    -| [  1:0xfcc460] Op_leq            :
    -| [   :0xfcc5c0] Op_jmp_false      : [target_jmp = 0xfcc5e0]
    -| [  1:0xfcc600] Op_push_i         : "%c" [MALLOC|STRING|STRCUR]
    -| [   :0xfcc660] Op_no_op          :
    -| [  1:0xfcc520] Op_assign_concat  : c
    -| [   :0xfcc620] Op_jmp            : [target_jmp = 0xfcc440]
    ...
    -| [     2:0xfcc5a0] Op_K_printf         : [expr_count = 17] [redir_type = ""]
    -| [      :0xfcc140] Op_no_op            :
    -| [      :0xfcc1c0] Op_atexit           :
    -| [      :0xfcc640] Op_stop             :
    -| [      :0xfcc180] Op_no_op            :
    -| [      :0xfcd150] Op_after_beginfile  :
    ```

    ```
    -| [      :0xfcc160] Op_no_op            :
    -| [      :0xfcc1a0] Op_after_endfile    :
    gawk>
    ```

`exit` [¶](#index-exit-debugger-command)
:   Exit the debugger.
    See the entry for ‘`quit`’, later in this list.

`help` [¶](#index-help-debugger-command)

`h`
:   Print a list of all of the `gawk` debugger commands with a short
    summary of their usage. ‘`help command`’ prints the information
    about the command command.

`list` [`-` | `+` | n | filename`:`n | n–m | function] [¶](#index-list-debugger-command)

`l` [`-` | `+` | n | filename`:`n | n–m | function]
:   Print the specified lines (default 15) from the current source file
    or the file named filename. The possible arguments to `list`
    are as follows:

    `-` (Minus)
    :   Print lines before the lines last printed.

    `+`
    :   Print lines after the lines last printed.
        `list` without any argument does the same thing.

    n
    :   Print lines centered around line number n.

    n–m
    :   Print lines from n to m.

    filename`:`n
    :   Print lines centered around line number n in
        source file filename. This command may change the current source file.

    function
    :   Print lines centered around the beginning of the
        function function. This command may change the current source file.

`quit` [¶](#index-quit-debugger-command)

`q`
:   Exit the debugger. Debugging is great fun, but sometimes we all have
    to tend to other obligations in life, and sometimes we find the bug
    and are free to go on to the next one! As we saw earlier, if you are
    running a program, the debugger warns you when you type
    ‘`q`’ or ‘`quit`’, to make sure you really want to quit.

`trace` [`on` | `off`] [¶](#index-trace-debugger-command)
:   Turn on or off continuous printing of the instructions that are about to
    be executed, along with the `awk` lines they
    implement. The default is `off`.

    It is to be hoped that most of the “opcodes” in these instructions are
    fairly self-explanatory, and using `stepi` and `nexti` while
    `trace` is on will make them into familiar friends.

---

Next: [Limitations](#Limitations), Previous: [Main Debugger Commands](#List-of-Debugger-Commands), Up: [Debugging `awk` Programs](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.4 Readline Support [¶](#Readline-Support-1)

If `gawk` is compiled with
[the GNU Readline library](https://tiswww.case.edu/php/chet/readline/rltop.html), you can take advantage of that library’s
command completion and history expansion features. The following types
of completion are available:

Command completion
:   Command names.

Source file name completion
:   Source file names. Relevant commands are
    `break`,
    `clear`,
    `list`,
    `tbreak`,
    and
    `until`.

Argument completion
:   Non-numeric arguments to a command.
    Relevant commands are `enable` and `info`.

Variable name completion
:   Global variable names, and function arguments in the current context
    if the program is running. Relevant commands are
    `display`,
    `print`,
    `set`,
    and
    `watch`.

---

Next: [Summary](#Debugging-Summary), Previous: [Readline Support](#Readline-Support), Up: [Debugging `awk` Programs](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.5 Limitations [¶](#Limitations-1)

We hope you find the `gawk` debugger useful and enjoyable to work with,
but as with any program, especially in its early releases, it still has
some limitations. A few that it’s worth being aware of are:

* At this point, the debugger does not give a detailed explanation of
  what you did wrong when you type in something it doesn’t like. Rather, it just
  responds ‘`syntax error`’. When you do figure out what your mistake was,
  though, you’ll feel like a real guru.
* If you perused the dump of opcodes in [Miscellaneous Commands](#Miscellaneous-Debugger-Commands)
  (or if you are already familiar with `gawk` internals),
  you will realize that much of the internal manipulation of data
  in `gawk`, as in many interpreters, is done on a stack.
  `Op_push`, `Op_pop`, and the like are the “bread and butter” of
  most `gawk` code.

  Unfortunately, as of now, the `gawk`
  debugger does not allow you to examine the stack’s contents.
  That is, the intermediate results of expression evaluation are on the
  stack, but cannot be printed. Rather, only variables that are defined
  in the program can be printed. Of course, a workaround for
  this is to use more explicit variables at the debugging stage and then
  change back to obscure, perhaps more optimal code later.
* There is no way to look “inside” the process of compiling
  regular expressions to see if you got it right. As an `awk`
  programmer, you are expected to know the meaning of
  `/[^[:alnum:][:blank:]]/`.
* The `gawk` debugger is designed to be used by running a program (with all its
  parameters) on the command line, as described in [How to Start the Debugger](#Debugger-Invocation).
  There is no way (as of now) to attach or “break into” a running program.
  This seems reasonable for a language that is used mainly for quickly
  executing, short programs.
* The `gawk` debugger only accepts source code supplied with the `-f` option.
  If you have a shell script that provides an `awk` program as a command
  line parameter, and you need to use the debugger, you can write the script
  to a temporary file, and use that as the program, with the `-f` option. This
  might look like this:

  ```
  cat << \EOF > /tmp/script.$$
  ...                                  Your program here
  EOF
  gawk -D -f /tmp/script.$$
  rm /tmp/script.$$
  ```

---

Previous: [Limitations](#Limitations), Up: [Debugging `awk` Programs](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.6 Summary [¶](#Summary-13)

* Programs rarely work correctly the first time. Finding bugs
  is called debugging, and a program that helps you find bugs is a
  debugger. `gawk` has a built-in debugger that works very
  similarly to the GNU Debugger, GDB.
* Debuggers let you step through your program one statement at a time,
  examine and change variable and array values, and do a number of other
  things that let you understand what your program is actually doing (as
  opposed to what it is supposed to do).
* Like most debuggers, the `gawk` debugger works in terms of stack
  frames, and lets you set both breakpoints (stop at a point in the code)
  and watchpoints (stop when a data value changes).
* The debugger command set is fairly complete, providing control over
  breakpoints, execution, viewing and changing data, working with the stack,
  getting information, and other tasks.
* If the GNU Readline library is available when `gawk` is
  compiled, it is used by the debugger to provide command-line history
  and editing.
* Usually, the debugger does not affect the
  program being debugged, but occasionally it can.

---

Next: [Arithmetic and Arbitrary-Precision Arithmetic with `gawk`](#Arbitrary-Precision-Arithmetic), Previous: [Debugging `awk` Programs](#Debugger), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 15 Namespaces in `gawk` [¶](#Namespaces-in-gawk)

This chapter describes a feature that is specific to `gawk`.

> **CAUTION:** This feature described in this chapter is new. It is entirely
> possible, and even likely, that there are dark corners (if not bugs)
> still lurking within the implementation. If you find any such,
> please report them (See [Reporting Problems and Bugs](#Bugs)).

* [Standard `awk`’s Single Namespace](#Global-Namespace)
* [Qualified Names](#Qualified-Names)
* [The Default Namespace](#Default-Namespace)
* [Changing The Namespace](#Changing-The-Namespace)
* [Namespace and Component Naming Rules](#Naming-Rules)
* [Internal Name Management](#Internal-Name-Management)
* [Namespace Example](#Namespace-Example)
* [Including A File Without Changing The Namespace](#Inclusion-For-Namespaces)
* [Namespaces and Other `gawk` Features](#Namespace-And-Features)
* [Summary](#Namespace-Summary)

---

Next: [Qualified Names](#Qualified-Names), Up: [Namespaces in `gawk`](#Namespaces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.1 Standard `awk`’s Single Namespace [¶](#Standard-awk_0027s-Single-Namespace)

In standard `awk`, there is a single, global, *namespace*.
This means that *all* function names and global variable names must
be unique. For example, two different `awk` source files cannot
both define a function named `min()`, or define the same identifier,
used as a scalar in one and as an array in the other.

This situation is okay when programs are small, say a few hundred
lines, or even a few thousand, but it prevents the development of
reusable libraries of `awk` functions, and can inadvertently
cause independently-developed library files to accidentally step on each
other’s “private” global variables
(see [Naming Library Function Global Variables](#Library-Names)).

Most other programming languages solve this issue by providing some kind
of namespace control: a way to say “this function is in namespace xxx,
and that function is in namespace yyy.” (Of course, there is then
still a single namespace for the namespaces, but the hope is that there
are much fewer namespaces in use by any given program, and thus much
less chance for collisions.) These facilities are sometimes referred
to as *packages* or *modules*.

Starting with version 5.0, `gawk` provides a
simple mechanism to put functions and global variables into separate namespaces.

---

Next: [The Default Namespace](#Default-Namespace), Previous: [Standard `awk`’s Single Namespace](#Global-Namespace), Up: [Namespaces in `gawk`](#Namespaces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.2 Qualified Names [¶](#Qualified-Names-1)

A *qualified name* is an identifier that includes a namespace name,
the namespace separator `::`, and a *component* name. For example, one
might have a function named `posix::getpid()`. Here, the namespace
is `posix` and the function name within the namespace (the component)
is `getpid()`. The namespace and component names are separated by
a double-colon. Only one such separator is allowed in a qualified name.

> **NOTE:** Unlike C++, the `::` is *not* an operator. No spaces are
> allowed between the namespace name, the `::`, and the component name.

You must use qualified names from one namespace to access variables
and functions in another. This is especially important when using
variable names to index the special `SYMTAB` array (see [Built-in Variables That Convey Information](#Auto_002dset)),
and when making indirect function calls (see [Indirect Function Calls](#Indirect-Calls)).

---

Next: [Changing The Namespace](#Changing-The-Namespace), Previous: [Qualified Names](#Qualified-Names), Up: [Namespaces in `gawk`](#Namespaces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.3 The Default Namespace [¶](#The-Default-Namespace)

The default namespace, not surprisingly, is `awk`.
All of the predefined `awk` and `gawk` variables
are in this namespace, and thus have qualified names like
`awk::ARGC`, `awk::NF`, and so on.

Furthermore, even when you have changed the namespace for your
current source file (see [Changing The Namespace](#Changing-The-Namespace)), `gawk`
forces unqualified identifiers whose names are all uppercase letters
to be in the `awk` namespace. This makes it possible for you to easily
reference `gawk`’s global variables from different namespaces.
It also keeps your code looking natural.

---

Next: [Namespace and Component Naming Rules](#Naming-Rules), Previous: [The Default Namespace](#Default-Namespace), Up: [Namespaces in `gawk`](#Namespaces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.4 Changing The Namespace [¶](#Changing-The-Namespace-1)

In order to set the current namespace, use an `@namespace` directive
at the top level of your program:

```
@namespace "passwd"

BEGIN { ... }
...
```

After this directive, all simple non-completely-uppercase identifiers are
placed into the `passwd` namespace.

You can change the namespace multiple times within a single
source file, although this is likely to become confusing if you
do it too much.

> **NOTE:** Association of unqualified identifiers to a namespace is handled while
> `gawk` parses your program, *before* it starts to run. There is
> no concept of a “current” namespace once your program starts executing.
> Be sure you understand this.

Each source file for `-i` and `-f` starts out with
an implicit ‘`@namespace "awk"`’. Similarly, each chunk of
command-line code supplied with `-e` has such an implicit
initial statement (see [Command-Line Options](#Options)).

Files included with `@include` (see [Including Other Files into Your Program](#Include-Files)) “push”
and “pop” the current namespace. That is, each `@include` saves
the current namespace and starts over with an implicit ‘`@namespace
"awk"`’ which remains in effect until an explicit `@namespace`
directive is seen. When `gawk` finishes processing the included
file, the saved namespace is restored and processing continues where it
left off in the original file.

The use of `@namespace` has no influence upon the order of execution
of `BEGIN`, `BEGINFILE`, `END`, and `ENDFILE` rules.

---

Next: [Internal Name Management](#Internal-Name-Management), Previous: [Changing The Namespace](#Changing-The-Namespace), Up: [Namespaces in `gawk`](#Namespaces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.5 Namespace and Component Naming Rules [¶](#Namespace-and-Component-Naming-Rules)

A number of rules apply to the namespace and component names, as follows.

* It is a syntax error to use qualified names for function parameter names.
* It is a syntax error to use any standard `awk` reserved word (such
  as `if` or `for`), or the name of any standard built-in function
  (such as `sin()` or `gsub()`) as either part of a qualified name.
  Thus, the following produces a syntax error:

  ```
  @namespace "example"

  function gsub(str, pat, result) { ... }
  ```
* Outside the `awk` namespace, the names of the additional `gawk`
  built-in functions (such as `gensub()` or `strftime()`) *may*
  be used as component names (see [Gawk Extension Functions](#Gawk-Extension-Functions)).
  The same set of names may be used as namespace
  names, although this has the potential to be confusing.
* The additional `gawk` built-in functions may still be called
  from outside the `awk` namespace by qualifying them. For example,
  `awk::systime()`. Here is a somewhat silly example demonstrating
  this rule and the previous one:

  ```
  BEGIN {
      print "in awk namespace, systime() =", systime()
  }

  @namespace "testing"

  function systime()
  {
      print "in testing namespace, systime() =", awk::systime()
  }

  BEGIN {
      systime()
  }
  ```

  When run, it produces output like this:

  ```
  $ gawk -f systime.awk
  -| in awk namespace, systime() = 1500488503
  -| in testing namespace, systime() = 1500488503
  ```
* `gawk` pre-defined variable names may be used:
  `NF::NR` is valid, if possibly not all that useful.

---

Next: [Namespace Example](#Namespace-Example), Previous: [Namespace and Component Naming Rules](#Naming-Rules), Up: [Namespaces in `gawk`](#Namespaces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.6 Internal Name Management [¶](#Internal-Name-Management-1)

For backwards compatibility, all identifiers in the `awk` namespace
are stored internally as unadorned identifiers (that is, without a
leading ‘`awk::`’). This is mainly relevant
when using such identifiers as indices for `SYMTAB`, `FUNCTAB`,
and `PROCINFO["identifiers"]` (see [Built-in Variables That Convey Information](#Auto_002dset)), and for use in
indirect function calls (see [Indirect Function Calls](#Indirect-Calls)).

In program code, to refer to variables and functions in the `awk`
namespace from another namespace, you must still use the ‘`awk::`’
prefix. For example:

```
@namespace "awk"          This is the default namespace

BEGIN {
    Title = "My Report"   Qualified name is awk::Title
}

@namespace "report"       Now in report namespace

function compute()        This is really report::compute()
{
    print awk::Title      But would be SYMTAB["Title"]
    ...
}
```

---

Next: [Including A File Without Changing The Namespace](#Inclusion-For-Namespaces), Previous: [Internal Name Management](#Internal-Name-Management), Up: [Namespaces in `gawk`](#Namespaces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.7 Namespace Example [¶](#Namespace-Example-1)

The following example is a revised version of the suite of routines
developed in [Reading the User Database](#Passwd-Functions). See there for an explanation
of how the code works.

The formulation here, due mainly to Andrew Schorr, is rather elegant.
All of the implementation functions and variables are in the
`passwd` namespace, whereas the main interface functions are
defined in the `awk` namespace.

```
# ns_passwd.awk --- access password file information

@namespace "passwd"

BEGIN {
    # tailor this to suit your system
    Awklib = "/usr/local/libexec/awk/"
}

function Init(    oldfs, oldrs, olddol0, pwcat, using_fw, using_fpat)
{
    if (Inited)
        return

    oldfs = FS
    oldrs = RS
    olddol0 = $0
    using_fw = (PROCINFO["FS"] == "FIELDWIDTHS")
    using_fpat = (PROCINFO["FS"] == "FPAT")
    FS = ":"
    RS = "\n"

    pwcat = Awklib "pwcat"
    while ((pwcat | getline) > 0) {
        Byname[$1] = $0
        Byuid[$3] = $0
        Bycount[++Total] = $0
    }
    close(pwcat)
    Count = 0
    Inited = 1
    FS = oldfs
    if (using_fw)
        FIELDWIDTHS = FIELDWIDTHS
    else if (using_fpat)
        FPAT = FPAT
    RS = oldrs
    $0 = olddol0
}

function awk::getpwnam(name)
{
    Init()
    return Byname[name]
}

function awk::getpwuid(uid)
{
    Init()
    return Byuid[uid]
}

function awk::getpwent()
{
    Init()
    if (Count < Total)
        return Bycount[++Count]
    return ""
}

function awk::endpwent()
{
    Count = 0
}
```

As you can see, this version also follows the convention mentioned in
[Naming Library Function Global Variables](#Library-Names), whereby global variable and function names
start with a capital letter.

Here is a simple test program. Since it’s in a separate file, unadorned
identifiers are sought for in the `awk` namespace:

```
BEGIN {
    while ((p = getpwent()) != "")
        print p
}
```

Here’s what happens when it’s run:

```
$ gawk -f ns_passwd.awk -f testpasswd.awk
-| root:x:0:0:root:/root:/bin/bash
-| daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
-| bin:x:2:2:bin:/bin:/usr/sbin/nologin
-| sys:x:3:3:sys:/dev:/usr/sbin/nologin
...
```

---

Next: [Namespaces and Other `gawk` Features](#Namespace-And-Features), Previous: [Namespace Example](#Namespace-Example), Up: [Namespaces in `gawk`](#Namespaces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.8 Including A File Without Changing The Namespace [¶](#Including-A-File-Without-Changing-The-Namespace)

As mentioned at the end of [Including Other Files into Your Program](#Include-Files), files included with
`@include` are treated as if they had ‘`@namespace "awk"`’
at their beginning. Sometimes, though, this behavior is undesirable.

Consider, for example, the case where you have some significant functions
in one namespace:

```
@namespace "mylib"

function major_func1(a, b, c) { ... }
function major_func2(a, b, c) { ... }
...
```

From another namespace, you want to call these functions,
frequently:

```
@namespace "myproj1"
function job1(a, b, c) {
    ...
    if (mylib::major_func1(a, b, c))
        ...
    else if (mylib::major_func2(a, b, c))
        ...
    ...
}
... and so on ...
```

Using the ‘`mylib::`’ prefix everywhere starts to get painful.

You might then try to write some *forwarding functions*, which you
would `@include` in each namespace. For example, something
like this in a file named `mylib_forward.awk`:

```
function major_func1(a, b, c)
{
    return mylib::major_func1(a, b, c)
}

function major_func2(a, b, c)
{
    return mylib::major_func2(a, b, c)
}
```

You might then try to `@include` this file in each namespace:

```
@namespace "myproj1"
@include "mylib_forward.awk"

function job1(a, b, c) {
    ...
    if (major_func1(a, b, c))       # calls forwarding function
        ...
    else if (major_func2(a, b, c))  # ditto
        ...
    ...
}
...

@namespace "myproj2"
@include "mylib_forward.awk"
...
```

But this won’t work, since `@include` resets the namespace
to ‘`awk`’.

For this reason, `@nsinclude` was added. It works exactly
the same as `@include`, but it does not change the namespace:

```
@namespace "myproj1"
@nsinclude "mylib_forward.awk"

function job1(a, b, c) {
    ...
    if (major_func1(a, b, c))       # calls forwarding function
        ...
    else if (major_func2(a, b, c))  # ditto
        ...
    ...
}
...

@namespace "myproj2"
@nsinclude "mylib_forward.awk"
...
```

This will work the way we want it to.

---

Next: [Summary](#Namespace-Summary), Previous: [Including A File Without Changing The Namespace](#Inclusion-For-Namespaces), Up: [Namespaces in `gawk`](#Namespaces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.9 Namespaces and Other `gawk` Features [¶](#Namespaces-and-Other-gawk-Features)

This section looks briefly at how the namespace facility interacts
with other important `gawk` features.

The profiler and pretty-printer (see [Profiling Your `awk` Programs](#Profiling)) have been enhanced
to understand namespaces and the namespace naming rules presented in
[Namespace and Component Naming Rules](#Naming-Rules). In particular, the output groups functions in the same
namespace together, and has `@namespace` directives in front
of rules as necessary. This allows component names to be
simple identifiers, instead of using qualified identifiers everywhere.

Interaction with the debugger (see [Introduction to the `gawk` Debugger](#Debugging)) has not had to change
(at least as of this writing). Some of the internal byte codes changed
in order to accommodate namespaces, and the debugger’s `dump` command
was adjusted to match.

The extension API (see [Writing Extensions for `gawk`](#Dynamic-Extensions)) has always allowed for
placing functions into a different namespace, although this was not
previously implemented. However, the symbol lookup and symbol update
routines did not have provision for including a namespace. That has now
been corrected (see [Variable Access and Update by Name](#Symbol-table-by-name)).
See [Enabling In-Place File Editing](#Extension-Sample-Inplace), for a nice example of an extension that
leverages a namespace shared by cooperating `awk` and C code.

---

Previous: [Namespaces and Other `gawk` Features](#Namespace-And-Features), Up: [Namespaces in `gawk`](#Namespaces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.10 Summary [¶](#Summary-14)

* Standard `awk` provides a single namespace for all global
  identifiers (scalars, arrays, and functions). This is limiting when
  one wants to develop libraries of reusable functions or function suites.
* `gawk` provides multiple namespaces by using qualified names:
  names consisting of a namespace name, a double colon, `::`, and a
  component name. Namespace names might still possibly conflict, but this
  is true of any language providing namespaces, modules, or packages.
* The default namespace is `awk`. The rules for namespace and
  component names are provided in [Namespace and Component Naming Rules](#Naming-Rules). The rules are
  designed in such a way as to make namespace-aware code continue to
  look and work naturally while still providing the necessary power and
  flexibility.
* Other parts of `gawk` have been extended as necessary to integrate
  namespaces smoothly with their operation. This applies most notably to
  the profiler / pretty-printer (see [Profiling Your `awk` Programs](#Profiling)) and to the extension
  facility (see [Writing Extensions for `gawk`](#Dynamic-Extensions)).
* Overall, the namespace facility was designed and implemented such that
  backwards compatibility is paramount. Programs that don’t use namespaces
  should see absolutely no difference in behavior when run by a namespace-capable
  version of `gawk`.

---

Next: [Writing Extensions for `gawk`](#Dynamic-Extensions), Previous: [Namespaces in `gawk`](#Namespaces), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 16 Arithmetic and Arbitrary-Precision Arithmetic with `gawk` [¶](#Arithmetic-and-Arbitrary_002dPrecision-Arithmetic-with-gawk)

This chapter introduces some basic concepts relating to
how computers do arithmetic and defines some important terms.
It then proceeds to describe floating-point arithmetic,
which is what `awk` uses for all its computations, including a
discussion of arbitrary-precision floating-point arithmetic, which is
a feature available only in `gawk`. It continues on to present
arbitrary-precision integers, and concludes with a description of some
points where `gawk` and the POSIX standard are not quite in
agreement.

> **NOTE:** Most users of `gawk` can safely skip this chapter.
> But if you want to do scientific calculations with `gawk`,
> this is the place to be.

* [A General Description of Computer Arithmetic](#Computer-Arithmetic)
* [Other Stuff to Know](#Math-Definitions)
* [Arbitrary-Precision Arithmetic Features in `gawk`](#MPFR-features)
* [Floating-Point Arithmetic: Caveat Emptor!](#FP-Math-Caution)
* [Arbitrary-Precision Integer Arithmetic with `gawk`](#Arbitrary-Precision-Integers)
* [How To Check If MPFR Is Available](#Checking-for-MPFR)
* [Standards Versus Existing Practice](#POSIX-Floating-Point-Problems)
* [Summary](#Floating-point-summary)

---

Next: [Other Stuff to Know](#Math-Definitions), Up: [Arithmetic and Arbitrary-Precision Arithmetic with `gawk`](#Arbitrary-Precision-Arithmetic)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
