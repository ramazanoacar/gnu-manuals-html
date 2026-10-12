# user supplies beginfile() and endfile() functions

FNR == 1 {
    if (_filename_ != "")
        endfile(_filename_)
    _filename_ = FILENAME
    beginfile(FILENAME)
}

END { endfile(_filename_) }
```

[Counting Things](#Wc-Program)
shows how this library function can be used and
how it simplifies writing the main program.

There is one small catch to how this program works.
It relies on the action being executed for the first
record in the file. If you happen to process a zero-length
file, `FNR` is never set, and `awk` just
moves on to the next file in the argument list.
If you’re worried about this, you’ll have to find a
workaround.

| So Why Does `gawk` Have `BEGINFILE` and `ENDFILE`? |
| --- |
| You are probably wondering, if `beginfile()` and `endfile()` functions can do the job, why does `gawk` have `BEGINFILE` and `ENDFILE` patterns?  Good question. Normally, if `awk` cannot open a file, this causes an immediate fatal error. In this case, there is no way for a user-defined function to deal with the problem, as the mechanism for calling it relies on the file being open and at the first record. Thus, the main reason for `BEGINFILE` is to give you a “hook” to catch files that cannot be processed. `ENDFILE` exists for symmetry, and because it provides an easy way to do per-file cleanup processing. For more information, refer to [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE).  One might next ask, so what’s the value in `ftrans.awk`? The answer is that it provides a *portable* way to implement beginning-of-file and end-of-file handling. This code should work with any POSIX-compliant version of `awk`. |

---

Next: [Checking for Readable Data files](#File-Checking), Previous: [Noting Data file Boundaries](#Filetrans-Function), Up: [Data file Management](#Data-File-Management)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.3.2 Rereading the Current File [¶](#Rereading-the-Current-File)

Another request for a new built-in function was for a
function that would make it possible to reread the current file.
The requesting user didn’t want to have to use `getline`
(see [Explicit Input with `getline`](#Getline))
inside a loop.

However, as long as you are not in the `END` rule, it is
quite easy to arrange to immediately close the current input file
and then start over with it from the top.
For lack of a better name, we’ll call the function `rewind()`:

```
# rewind.awk --- rewind the current file and start over

function rewind(    i)
{
    # shift remaining arguments up
    for (i = ARGC; i > ARGIND; i--)
        ARGV[i] = ARGV[i-1]

    # make sure gawk knows to keep going
    ARGC++

    # make current file next to get done
    ARGV[ARGIND+1] = FILENAME

    # do it
    nextfile
}
```

The `rewind()` function relies on the `ARGIND` variable
(see [Built-in Variables That Convey Information](#Auto_002dset)), which is specific to `gawk`. It also
relies on the `nextfile` keyword (see [The `nextfile` Statement](#Nextfile-Statement)).
Because of this, you should not call it from an `ENDFILE` rule.
(This isn’t necessary anyway, because `gawk` goes to the next
file as soon as an `ENDFILE` rule finishes!)

You need to be careful calling `rewind()`. You can end up
causing infinite recursion if you don’t pay attention. Here is an
example use:

```
$ cat data
-| a
-| b
-| c
-| d
-| e

$ cat test.awk
-| FNR == 3 && ! rewound {
-|    rewound = 1
-|    rewind()
-| }
-|
-| { print FILENAME, FNR, $0 }

$ gawk -f rewind.awk -f test.awk data 
-| data 1 a
-| data 2 b
-| data 1 a
-| data 2 b
-| data 3 c
```

```
-| data 4 d
-| data 5 e
```

---

Next: [Checking for Zero-Length Files](#Empty-Files), Previous: [Rereading the Current File](#Rewind-Function), Up: [Data file Management](#Data-File-Management)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.3.3 Checking for Readable Data files [¶](#Checking-for-Readable-Data-files)

Normally, if you give `awk` a data file that isn’t readable,
it stops with a fatal error. There are times when you might want to
just ignore such files and keep going.[75](#FOOT75) You can do this by prepending
the following program to your `awk` program:

```
# readable.awk --- library file to skip over unreadable files

BEGIN {
    for (i = 1; i < ARGC; i++) {
        if (ARGV[i] ~ /^[a-zA-Z_][a-zA-Z0-9_]*=.*/ \
            || ARGV[i] == "-" || ARGV[i] == "/dev/stdin")
            continue    # assignment or standard input
        else if ((getline junk < ARGV[i]) < 0) # unreadable
            delete ARGV[i]
        else
            close(ARGV[i])
    }
}
```

This works, because the `getline` won’t be fatal.
Removing the element from `ARGV` with `delete`
skips the file (because it’s no longer in the list).
See also [Using `ARGC` and `ARGV`](#ARGC-and-ARGV).

Because `awk` variable names only allow the English letters,
the regular expression check purposely does not use character classes
such as ‘`[:alpha:]`’ and ‘`[:alnum:]`’
(see [Using Bracket Expressions](#Bracket-Expressions)).

---

Next: [Treating Assignments as File names](#Ignoring-Assigns), Previous: [Checking for Readable Data files](#File-Checking), Up: [Data file Management](#Data-File-Management)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.3.4 Checking for Zero-Length Files [¶](#Checking-for-Zero_002dLength-Files)

All known `awk` implementations silently skip over zero-length files.
This is a by-product of `awk`’s implicit
read-a-record-and-match-against-the-rules loop: when `awk`
tries to read a record from an empty file, it immediately receives an
end-of-file indication, closes the file, and proceeds on to the next
command-line data file, *without* executing any user-level
`awk` program code.

Using `gawk`’s `ARGIND` variable
(see [Predefined Variables](#Built_002din-Variables)), it is possible to detect when an empty
data file has been skipped. Similar to the library file presented
in [Noting Data file Boundaries](#Filetrans-Function), the following library file calls a function named
`zerofile()` that the user must provide. The arguments passed are
the file name and the position in `ARGV` where it was found:

```
# zerofile.awk --- library file to process empty input files
#
# user supplies zerofile() function

BEGIN { Argind = 0 }

ARGIND > Argind + 1 {
    for (Argind++; Argind < ARGIND; Argind++)
        zerofile(ARGV[Argind], Argind)
}

ARGIND != Argind { Argind = ARGIND }

END {
    if (ARGIND > Argind)
        for (Argind++; Argind <= ARGIND; Argind++)
            zerofile(ARGV[Argind], Argind)
}
```

The user-level variable `Argind` allows the `awk` program
to track its progress through `ARGV`. Whenever the program detects
that `ARGIND` is greater than ‘`Argind + 1`’, it means that one or
more empty files were skipped. The action then calls `zerofile()` for
each such file, incrementing `Argind` along the way.

The ‘`Argind != ARGIND`’ rule simply keeps `Argind` up to date
in the normal case.

Finally, the `END` rule catches the case of any empty files at
the end of the command-line arguments. Note that the test in the
condition of the `for` loop uses the ‘`<=`’ operator,
not ‘`<`’.

What if you’re not using `gawk`? You can write a separate
`BEGIN` rule that loops over the files named in `ARGV`
and attempts to read a record with `getline`. If `getline`
immediately returns zero (end of file), the file exists but is empty.
You can then delete it from `ARGV`, issue a warning, or do
anything else you think is appropriate.

Also, we note that this library routine was written when `gawk`
(like its maintainer) was considerably younger. In modern versions,
you could instead write it like this:

```
ENDFILE {
    if (FNR == 0)
            zerofile(FILENAME, ARGIND)
}
```

(With thanks to Andrew Schorr.)

---

Previous: [Checking for Zero-Length Files](#Empty-Files), Up: [Data file Management](#Data-File-Management)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.3.5 Treating Assignments as File names [¶](#Treating-Assignments-as-File-names)

Occasionally, you might not want `awk` to process command-line
variable assignments
(see [Assigning Variables on the Command Line](#Assignment-Options)).
In particular, if you have a file name that contains an ‘`=`’ character,
`awk` treats the file name as an assignment and does not process it.

Some users have suggested an additional command-line option for `gawk`
to disable command-line assignments. However, some simple programming with
a library file does the trick:

```
# noassign.awk --- library file to avoid the need for a
# special option that disables command-line assignments

function disable_assigns(argc, argv,    i)
{
    for (i = 1; i < argc; i++)
        if (argv[i] ~ /^[a-zA-Z_][a-zA-Z0-9_]*=.*/)
            argv[i] = ("./" argv[i])
}

BEGIN {
    if (No_command_assign)
        disable_assigns(ARGC, ARGV)
}
```

You then run your program this way:

```
awk -v No_command_assign=1 -f noassign.awk -f yourprog.awk *
```

The function works by looping through the arguments.
It prepends ‘`./`’ to
any argument that matches the form
of a variable assignment, turning that argument into a file name.

The use of `No_command_assign` allows you to disable command-line
assignments at invocation time, by giving the variable a true value.
When not set, it is initially zero (i.e., false), so the command-line arguments
are left alone.

---

Next: [Reading the User Database](#Passwd-Functions), Previous: [Data file Management](#Data-File-Management), Up: [A Library of `awk` Functions](#Library-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.4 Processing Command-Line Options [¶](#Processing-Command_002dLine-Options)

Most utilities on POSIX-compatible systems take options on
the command line that can be used to change the way a program behaves.
`awk` is an example of such a program
(see [Command-Line Options](#Options)).
Often, options take *arguments* (i.e., data that the program needs to
correctly obey the command-line option). For example, `awk`’s
`-F` option requires a string to use as the field separator.
The first occurrence on the command line of either `--` or a
string that does not begin with ‘`-`’ ends the options.

Modern Unix systems provide a C function named `getopt()` for processing
command-line arguments. The programmer provides a string describing the
one-letter options. If an option requires an argument, it is followed in the
string with a colon. `getopt()` is also passed the
count and values of the command-line arguments and is called in a loop.
`getopt()` processes the command-line arguments for option letters.
Each time around the loop, it returns a single character representing the
next option letter that it finds, or ‘`?`’ if it finds an invalid option.
When it returns −1, there are no options left on the command line.

When using `getopt()`, options that do not take arguments can be
grouped together. Furthermore, options that take arguments require that the
argument be present. The argument can immediately follow the option letter,
or it can be a separate command-line argument.

Given a hypothetical program that takes
three command-line options, `-a`, `-b`, and `-c`, where
`-b` requires an argument, all of the following are valid ways of
invoking the program:

```
prog -a -b foo -c data1 data2 data3
prog -ac -bfoo -- data1 data2 data3
prog -acbfoo data1 data2 data3
```

Notice that when the argument is grouped with its option, the rest of
the argument is considered to be the option’s argument.
In this example, `-acbfoo` indicates that all of the
`-a`, `-b`, and `-c` options were supplied,
and that ‘`foo`’ is the argument to the `-b` option.

`getopt()` provides four external variables that the programmer can use:

`optind`
:   The index in the argument value array (`argv`) where the first
    nonoption command-line argument can be found.

`optarg`
:   The string value of the argument to an option.

`opterr`
:   Usually `getopt()` prints an error message when it finds an invalid
    option. Setting `opterr` to zero disables this feature. (An
    application might want to print its own error message.)

`optopt`
:   The letter representing the command-line option.

The following C fragment shows how `getopt()` might process command-line
arguments for `awk`:

```
int
main(int argc, char *argv[])
{
    ...
    /* print our own message */
    opterr = 0;
    while ((c = getopt(argc, argv, "v:f:F:W:")) != -1) {
        switch (c) {
        case 'f':    /* file */
            ...
            break;
        case 'F':    /* field separator */
            ...
            break;
        case 'v':    /* variable assignment */
            ...
            break;
        case 'W':    /* extension */
            ...
            break;
        case '?':
        default:
            usage();
            break;
        }
    }
    ...
}
```

The GNU project’s version of the original Unix utilities popularized
the use of long command line options. For example, `--help`
in addition to `-h`. Arguments to long options are either provided
as separate command line arguments (‘`--source 'program-text'`’)
or separated from the option with an ‘`=`’ sign
(‘`--source='program-text'`’).

As a side point, `gawk` actually uses the GNU `getopt_long()`
function to process both normal and GNU-style long options
(see [Command-Line Options](#Options)).

The abstraction provided by `getopt()` is very useful and is quite
handy in `awk` programs as well. Following is an `awk`
version of `getopt()` that accepts both short and long options.
(Support for long options was supplied by Greg Minshall. We thank him.)

This function highlights one of the
greatest weaknesses in `awk`, which is that it is very poor at
manipulating single characters. The function needs repeated calls to
`substr()` in order to access individual characters
(see [String-Manipulation Functions](#String-Functions)).[76](#FOOT76)

The discussion that follows walks through the code a bit at a time:

```
# getopt.awk --- Do C library getopt(3) function in awk
#                Also supports long options.

# External variables:
#    Optind -- index in ARGV of first nonoption argument
#    Optarg -- string value of argument to current option
#    Opterr -- if nonzero, print our own diagnostic
#    Optopt -- current option letter

# Returns:
#    -1     at end of options
#    "?"    for unrecognized option
#    <s>    a string representing the current option

# Private Data:
#    _opti  -- index in multiflag option, e.g., -abc
```

The function starts out with comments presenting
a list of the global variables it uses,
what the return values are, what they mean, and any global variables that
are “private” to this library function. Such documentation is essential
for any program, and particularly for library functions.

The `getopt()` function first checks that it was indeed called with
a string of options (the `options` parameter). If both
`options` and `longoptions` have a zero length,
`getopt()` immediately returns −1:

```
function getopt(argc, argv, options, longopts,    thisopt, i, j)
{
    if (length(options) == 0 && length(longopts) == 0)
        return -1                # no options given
```

```
    if (argv[Optind] == "--") {  # all done
        Optind++
        _opti = 0
        return -1
```

```
    } else if (argv[Optind] !~ /^-[^:[:space:]]/) {
        _opti = 0
        return -1
    }
```

The next thing to check for is the end of the options. A `--`
ends the command-line options, as does any command-line argument that
does not begin with a ‘`-`’ (unless it is an argument to a preceding
option). `Optind` steps through
the array of command-line arguments; it retains its value across calls
to `getopt()`, because it is a global variable.

The regular expression `/^-[^:[:space:]/`
checks for a ‘`-`’ followed by anything
that is not whitespace and not a colon.
If the current command-line argument does not match this pattern,
it is not an option, and it ends option processing.
Now, we
check to see if we are processing a short (single letter) option, or a
long option (indicated by two dashes, e.g., ‘`--filename`’). If it
is a short option, we continue on:

```
    if (argv[Optind] !~ /^--/) {        # if this is a short option
        if (_opti == 0)
            _opti = 2
        thisopt = substr(argv[Optind], _opti, 1)
        Optopt = thisopt
        i = index(options, thisopt)
        if (i == 0) {
            if (Opterr)
                printf("%c -- invalid option\n", thisopt) > "/dev/stderr"
            if (_opti >= length(argv[Optind])) {
                Optind++
                _opti = 0
            } else
                _opti++
            return "?"
        }
```

The `_opti` variable tracks the position in the current command-line
argument (`argv[Optind]`). If multiple options are
grouped together with one ‘`-`’ (e.g., `-abx`), it is necessary
to return them to the user one at a time.

If `_opti` is equal to zero, it is set to two, which is the index in
the string of the next character to look at (we skip the ‘`-`’, which
is at position one). The variable `thisopt` holds the character,
obtained with `substr()`. It is saved in `Optopt` for the main
program to use.

If `thisopt` is not in the `options` string, then it is an
invalid option. If `Opterr` is nonzero, `getopt()` prints an error
message on the standard error that is similar to the message from the C
version of `getopt()`.

Because the option is invalid, it is necessary to skip it and move on to the
next option character. If `_opti` is greater than or equal to the
length of the current command-line argument, it is necessary to move on
to the next argument, so `Optind` is incremented and `_opti` is reset
to zero. Otherwise, `Optind` is left alone and `_opti` is merely
incremented.

In any case, because the option is invalid, `getopt()` returns `"?"`.
The main program can examine `Optopt` if it needs to know what the
invalid option letter actually is. Continuing on:

```
        if (substr(options, i + 1, 1) == ":") {
            # get option argument
            if (length(substr(argv[Optind], _opti + 1)) > 0)
                Optarg = substr(argv[Optind], _opti + 1)
            else
                Optarg = argv[++Optind]
            _opti = 0
        } else
            Optarg = ""
```

If the option requires an argument, the option letter is followed by a colon
in the `options` string. If there are remaining characters in the
current command-line argument (`argv[Optind]`), then the rest of that
string is assigned to `Optarg`. Otherwise, the next command-line
argument is used (‘`-xFOO`’ versus ‘`-x FOO`’). In either case,
`_opti` is reset to zero, because there are no more characters left to
examine in the current command-line argument. Continuing:

```
        if (_opti == 0 || _opti >= length(argv[Optind])) {
            Optind++
            _opti = 0
        } else
            _opti++
        return thisopt
```

Finally, for a short option, if `_opti` is either zero or greater
than the length of the current command-line argument, it means this
element in `argv` is through being processed, so `Optind` is
incremented to point to the next element in `argv`. If neither
condition is true, then only `_opti` is incremented, so that the
next option letter can be processed on the next call to `getopt()`.

On the other hand, if the earlier test found that this was a long
option, we take a different branch:

```
    } else {
        j = index(argv[Optind], "=")
        if (j > 0)
            thisopt = substr(argv[Optind], 3, j - 3)
        else
            thisopt = substr(argv[Optind], 3)
        Optopt = thisopt
```

First, we search this option for a possible embedded equal sign, as the
specification of long options allows an argument to an option
‘`--someopt`’ to be specified as ‘`--someopt=answer`’ as well as
‘`--someopt answer`’.

```
        i = match(longopts, "(^|,)" thisopt "($|[,:])")
        if (i == 0) {
            if (Opterr)
                 printf("%s -- invalid option\n", thisopt) > "/dev/stderr"
            Optind++
            return "?"
        }
```

Next, we try to find the current option in `longopts`. The regular
expression given to `match()`, `"(^|,)" thisopt "($|[,:])"`,
matches this option at the beginning of `longopts`, or at the
beginning of a subsequent long option (the previous long option would
have been terminated by a comma), and, in any case, either at the end of
the `longopts` string (‘`$`’), or followed by a comma
(separating this option from a subsequent option) or a colon (indicating
this long option takes an argument (‘`[,:]`’).

Using this regular expression, we check to see if the current option
might possibly be in `longopts` (if `longopts` is not
specified, this test will also fail). In case of an error, we possibly
print an error message and then return `"?"`. Continuing on:

```
        if (substr(longopts, i-1+RLENGTH, 1) == ":") {
            if (j > 0)
                Optarg = substr(argv[Optind], j + 1)
            else
                Optarg = argv[++Optind]
        } else
            Optarg = ""
```

We now check to see if this option takes an argument and, if so, we set
`Optarg` to the value of that argument (either a value after an
equal sign specified on the command line, immediately adjoining the long
option string, or as the next argument on the command line).

```
        Optind++
        return thisopt
    }
}
```

We increase `Optind` (which we already increased once if a required
argument was separated from its option by an equal sign), and return the
long option (minus its leading dashes).

The `BEGIN` rule initializes both `Opterr` and `Optind` to one.
`Opterr` is set to one, because the default behavior is for `getopt()`
to print a diagnostic message upon seeing an invalid option. `Optind`
is set to one, because there’s no reason to look at the program name, which is
in `ARGV[0]`:

```
BEGIN {
    Opterr = 1    # default is to diagnose
    Optind = 1    # skip ARGV[0]

    # test program
    if (_getopt_test) {
        _myshortopts = "ab:cd"
        _mylongopts = "longa,longb:,otherc,otherd"

        while ((_go_c = getopt(ARGC, ARGV, _myshortopts, _mylongopts)) != -1)
            printf("c = <%s>, Optarg = <%s>\n", _go_c, Optarg)
        printf("non-option arguments:\n")
        for (; Optind < ARGC; Optind++)
            printf("\tARGV[%d] = <%s>\n", Optind, ARGV[Optind])
    }
}
```

The rest of the `BEGIN` rule is a simple test program. Here are the
results of some sample runs of the test program:

```
$ awk -f getopt.awk -v _getopt_test=1 -- -a -cbARG bax -x
-| c = <a>, Optarg = <>
-| c = <c>, Optarg = <>
-| c = <b>, Optarg = <ARG>
-| non-option arguments:
-|         ARGV[3] = <bax>
-|         ARGV[4] = <-x>

$ awk -f getopt.awk -v _getopt_test=1 -- -a -x -- xyz abc
-| c = <a>, Optarg = <>
error→ x -- invalid option
-| c = <?>, Optarg = <>
-| non-option arguments:
-|         ARGV[4] = <xyz>
-|         ARGV[5] = <abc>

$ awk -f getopt.awk -v _getopt_test=1 -- -a \
> --longa -b xx --longb=foo=bar --otherd --otherc arg1 arg2
-| c = <a>, Optarg = <>
-| c = <longa>, Optarg = <>
-| c = <b>, Optarg = <xx>
-| c = <longb>, Optarg = <foo=bar>
-| c = <otherd>, Optarg = <>
-| c = <otherc>, Optarg = <>
-| non-option arguments:
-|        ARGV[8] = <arg1>
-|        ARGV[9] = <arg2>
```

In all the runs, the first `--` terminates the arguments to
`awk`, so that it does not try to interpret the `-a`,
etc., as its own options.

> **NOTE:** After `getopt()` is through,
> user-level code must clear out all the elements of `ARGV` from 1
> to `Optind`, so that `awk` does not try to process the
> command-line options as file names.

Using ‘`#!`’ with the `-E` option may help avoid
conflicts between your program’s options and `gawk`’s options,
as `-E` causes `gawk` to abandon processing of
further options
(see [Executable `awk` Programs](#Executable-Scripts) and
see [Command-Line Options](#Options)).

Several of the sample programs presented in
[Practical `awk` Programs](#Sample-Programs),
use `getopt()` to process their arguments.

---

Next: [Reading the Group Database](#Group-Functions), Previous: [Processing Command-Line Options](#Getopt-Function), Up: [A Library of `awk` Functions](#Library-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.5 Reading the User Database [¶](#Reading-the-User-Database)

The `PROCINFO` array
(see [Predefined Variables](#Built_002din-Variables))
provides access to the current user’s real and effective user and group ID
numbers, and, if available, the user’s supplementary group set.
However, because these are numbers, they do not provide very useful
information to the average user. There needs to be some way to find the
user information associated with the user and group ID numbers. This
section presents a suite of functions for retrieving information from the
user database. See [Reading the Group Database](#Group-Functions)
for a similar suite that retrieves information from the group database.

The POSIX standard does not define the file where user information is
kept. Instead, it provides the `<pwd.h>` header file
and several C language subroutines for obtaining user information.
The primary function is `getpwent()`, for “get password entry.”
The “password” comes from the original user database file,
`/etc/passwd`, which stores user information along with the
encrypted passwords (hence the name).

Although an `awk` program could simply read `/etc/passwd`
directly, this file may not contain complete information about the
system’s set of users.[77](#FOOT77) To be sure you are able to
produce a readable and complete version of the user database, it is necessary
to write a small C program that calls `getpwent()`. `getpwent()`
is defined as returning a pointer to a `struct passwd`. Each time it
is called, it returns the next entry in the database. When there are
no more entries, it returns `NULL`, the null pointer. When this
happens, the C program should call `endpwent()` to close the database.
Following is `pwcat`, a C program that “cats” the password database:

```
/*
 * pwcat.c
 *
 * Generate a printable version of the password database.
 */
#include <stdio.h>
#include <pwd.h>

int
main(int argc, char **argv)
{
    struct passwd *p;

    while ((p = getpwent()) != NULL)
        printf("%s:%s:%ld:%ld:%s:%s:%s\n",
            p->pw_name, p->pw_passwd, (long) p->pw_uid,
            (long) p->pw_gid, p->pw_gecos, p->pw_dir, p->pw_shell);

    endpwent();
    return 0;
}
```

If you don’t understand C, don’t worry about it.
The output from `pwcat` is the user database, in the traditional
`/etc/passwd` format of colon-separated fields. The fields are:

Login name
:   The user’s login name.

Encrypted password
:   The user’s encrypted password. This may not be available on some systems.

User-ID
:   The user’s numeric user ID number.
    (On some systems, it’s a C `long`, and not an `int`. Thus,
    we cast it to `long` for all cases.)

Group-ID
:   The user’s numeric group ID number.
    (Similar comments about `long` versus `int` apply here.)

Full name
:   The user’s full name, and perhaps other information associated with the
    user.

Home directory
:   The user’s login (or “home”) directory (familiar to shell programmers as
    `$HOME`).

Login shell
:   The program that is run when the user logs in. This is usually a
    shell, such as Bash.

A few lines representative of `pwcat`’s output are as follows:

```
$ pwcat
-| root:x:0:1:Operator:/:/bin/sh
-| nobody:*:65534:65534::/:
-| daemon:*:1:1::/:
-| sys:*:2:2::/:/bin/csh
-| bin:*:3:3::/bin:
-| arnold:xyzzy:2076:10:Arnold Robbins:/home/arnold:/bin/sh
-| miriam:yxaay:112:10:Miriam Robbins:/home/miriam:/bin/sh
-| andy:abcca2:113:10:Andy Jacobs:/home/andy:/bin/sh
...
```

With that introduction, following is a group of functions for getting user
information. There are several functions here, corresponding to the C
functions of the same names:

```
# passwd.awk --- access password file information

BEGIN {
    # tailor this to suit your system
    _pw_awklib = "/usr/local/libexec/awk/"
}

function _pw_init(    oldfs, oldrs, olddol0, pwcat, using_fw, using_fpat)
{
    if (_pw_inited)
        return

    oldfs = FS
    oldrs = RS
    olddol0 = $0
    using_fw = (PROCINFO["FS"] == "FIELDWIDTHS")
    using_fpat = (PROCINFO["FS"] == "FPAT")
    FS = ":"
    RS = "\n"

    pwcat = _pw_awklib "pwcat"
    while ((pwcat | getline) > 0) {
        _pw_byname[$1] = $0
        _pw_byuid[$3] = $0
        _pw_bycount[++_pw_total] = $0
    }
    close(pwcat)
    _pw_count = 0
    _pw_inited = 1
    FS = oldfs
    if (using_fw)
        FIELDWIDTHS = FIELDWIDTHS
    else if (using_fpat)
        FPAT = FPAT
    RS = oldrs
    $0 = olddol0
}
```

The `BEGIN` rule sets a private variable to the directory where
`pwcat` is stored. Because it is used to help out an `awk` library
routine, we have chosen to put it in `/usr/local/libexec/awk`;
however, you might want it to be in a different directory on your system.

The function `_pw_init()` fills three copies of the user information
into three associative arrays. The arrays are indexed by username
(`_pw_byname`), by user ID number (`_pw_byuid`), and by order of
occurrence (`_pw_bycount`).
The variable `_pw_inited` is used for efficiency, as `_pw_init()`
needs to be called only once.

Because this function uses `getline` to read information from
`pwcat`, it first saves the values of `FS`, `RS`, and `$0`.
It notes in the variable `using_fw` whether field splitting
with `FIELDWIDTHS` is in effect or not.
Doing so is necessary, as these functions could be called
from anywhere within a user’s program, and the user may have his
or her own way of splitting records and fields.
This makes it possible to restore the correct
field-splitting mechanism later. The test can only be true for
`gawk`. It is false if using `FS` or `FPAT`,
or on some other `awk` implementation.

The code that checks for using `FPAT`, using `using_fpat`
and `PROCINFO["FS"]`, is similar.

The main part of the function uses a loop to read database lines, split
the lines into fields, and then store the lines into each array as necessary.
When the loop is done, `_pw_init()` cleans up by closing the pipeline,
setting `_pw_inited` to one, and restoring `FS`
(and `FIELDWIDTHS` or `FPAT`
if necessary), `RS`, and `$0`.
The use of `_pw_count` is explained shortly.

The `getpwnam()` function takes a username as a string argument. If that
user is in the database, it returns the appropriate line. Otherwise, it
relies on the array reference to a nonexistent
element to create the element with the null string as its value:

```
function getpwnam(name)
{
    _pw_init()
    return _pw_byname[name]
}
```

Similarly, the `getpwuid()` function takes a user ID number
argument. If that user number is in the database, it returns the
appropriate line. Otherwise, it returns the null string:

```
function getpwuid(uid)
{
    _pw_init()
    return _pw_byuid[uid]
}
```

The `getpwent()` function simply steps through the database, one entry at
a time. It uses `_pw_count` to track its current position in the
`_pw_bycount` array:

```
function getpwent()
{
    _pw_init()
    if (_pw_count < _pw_total)
        return _pw_bycount[++_pw_count]
    return ""
}
```

The `endpwent()` function resets `_pw_count` to zero, so that
subsequent calls to `getpwent()` start over again:

```
function endpwent()
{
    _pw_count = 0
}
```

A conscious design decision in this suite is that each subroutine calls
`_pw_init()` to initialize the database arrays.
The overhead of running
a separate process to generate the user database, and the I/O to scan it,
are only incurred if the user’s main program actually calls one of these
functions. If this library file is loaded along with a user’s program, but
none of the routines are ever called, then there is no extra runtime overhead.
(The alternative is move the body of `_pw_init()` into a
`BEGIN` rule, which always runs `pwcat`. This simplifies the
code but runs an extra process that may never be needed.)

In turn, calling `_pw_init()` is not too expensive, because the
`_pw_inited` variable keeps the program from reading the data more than
once. If you are worried about squeezing every last cycle out of your
`awk` program, the check of `_pw_inited` could be moved out of
`_pw_init()` and duplicated in all the other functions. In practice,
this is not necessary, as most `awk` programs are I/O-bound,
and such a change would clutter up the code.

The `id` program in [Printing Out User Information](#Id-Program)
uses these functions.

---

Next: [Traversing Arrays of Arrays](#Walking-Arrays), Previous: [Reading the User Database](#Passwd-Functions), Up: [A Library of `awk` Functions](#Library-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.6 Reading the Group Database [¶](#Reading-the-Group-Database)

Much of the discussion presented in
[Reading the User Database](#Passwd-Functions)
applies to the group database as well. Although there has traditionally
been a well-known file (`/etc/group`) in a well-known format, the POSIX
standard only provides a set of C library routines
(`<grp.h>` and `getgrent()`)
for accessing the information.
Even though this file may exist, it may not have
complete information. Therefore, as with the user database, it is necessary
to have a small C program that generates the group database as its output.
`grcat`, a C program that “cats” the group database,
is as follows:

```
/*
 * grcat.c
 *
 * Generate a printable version of the group database.
 */
#include <stdio.h>
#include <grp.h>

int
main(int argc, char **argv)
{
    struct group *g;
    int i;

    while ((g = getgrent()) != NULL) {
        printf("%s:%s:%ld:", g->gr_name, g->gr_passwd,
                                     (long) g->gr_gid);
        for (i = 0; g->gr_mem[i] != NULL; i++) {
            printf("%s", g->gr_mem[i]);
```

```
            if (g->gr_mem[i+1] != NULL)
                putchar(',');
        }
```

```
        putchar('\n');
    }
    endgrent();
    return 0;
}
```

Each line in the group database represents one group. The fields are
separated with colons and represent the following information:

Group Name
:   The group’s name.

Group Password
:   The group’s encrypted password. In practice, this field is never used;
    it is usually empty or set to ‘`*`’.

Group ID Number
:   The group’s numeric group ID number;
    the association of name to number must be unique within the file.
    (On some systems it’s a C `long`, and not an `int`. Thus,
    we cast it to `long` for all cases.)

Group Member List
:   A comma-separated list of usernames. These users are members of the group.
    Modern Unix systems allow users to be members of several groups
    simultaneously. If your system does, then there are elements
    `"group1"` through `"groupN"` in `PROCINFO`
    for those group ID numbers.
    (Note that `PROCINFO` is a `gawk` extension;
    see [Predefined Variables](#Built_002din-Variables).)

Here is what running `grcat` might produce:

```
$ grcat
-| wheel:*:0:arnold
-| nogroup:*:65534:
-| daemon:*:1:
-| kmem:*:2:
-| staff:*:10:arnold,miriam,andy
-| other:*:20:
...
```

Here are the functions for obtaining information from the group database.
There are several, modeled after the C library functions of the same names:

```
# group.awk --- functions for dealing with the group file

BEGIN {
    # Change to suit your system
    _gr_awklib = "/usr/local/libexec/awk/"
}

function _gr_init(    oldfs, oldrs, olddol0, grcat,
                             using_fw, using_fpat, n, a, i)
{
    if (_gr_inited)
        return

    oldfs = FS
    oldrs = RS
    olddol0 = $0
    using_fw = (PROCINFO["FS"] == "FIELDWIDTHS")
    using_fpat = (PROCINFO["FS"] == "FPAT")
    FS = ":"
    RS = "\n"

    grcat = _gr_awklib "grcat"
    while ((grcat | getline) > 0) {
        if ($1 in _gr_byname)
            _gr_byname[$1] = _gr_byname[$1] "," $4
        else
            _gr_byname[$1] = $0
        if ($3 in _gr_bygid)
            _gr_bygid[$3] = _gr_bygid[$3] "," $4
        else
            _gr_bygid[$3] = $0

        n = split($4, a, "[ \t]*,[ \t]*")
        for (i = 1; i <= n; i++)
            if (a[i] in _gr_groupsbyuser)
                _gr_groupsbyuser[a[i]] = _gr_groupsbyuser[a[i]] " " $1
            else
                _gr_groupsbyuser[a[i]] = $1

        _gr_bycount[++_gr_count] = $0
    }
    close(grcat)
    _gr_count = 0
    _gr_inited++
    FS = oldfs
    if (using_fw)
        FIELDWIDTHS = FIELDWIDTHS
    else if (using_fpat)
        FPAT = FPAT
    RS = oldrs
    $0 = olddol0
}
```

The `BEGIN` rule sets a private variable to the directory where
`grcat` is stored. Because it is used to help out an `awk` library
routine, we have chosen to put it in `/usr/local/libexec/awk`. You might
want it to be in a different directory on your system.

These routines follow the same general outline as the user database routines
(see [Reading the User Database](#Passwd-Functions)).
The `_gr_inited` variable is used to
ensure that the database is scanned no more than once.
The `_gr_init()` function first saves `FS`,
`RS`, and
`$0`, and then sets `FS` and `RS` to the correct values for
scanning the group information.
It also takes care to note whether `FIELDWIDTHS` or `FPAT`
is being used, and to restore the appropriate field-splitting mechanism.

The group information is stored in several associative arrays.
The arrays are indexed by group name (`_gr_byname`), by group ID number
(`_gr_bygid`), and by position in the database (`_gr_bycount`).
There is an additional array indexed by username (`_gr_groupsbyuser`),
which is a space-separated list of groups to which each user belongs.

Unlike in the user database, it is possible to have multiple records in the
database for the same group. This is common when a group has a large number
of members. A pair of such entries might look like the following:

```
tvpeople:*:101:johnny,jay,arsenio
tvpeople:*:101:david,conan,tom,joan
```

For this reason, `_gr_init()` looks to see if a group name or
group ID number is already seen. If so, the usernames are
simply concatenated onto the previous list of users.[78](#FOOT78)

Finally, `_gr_init()` closes the pipeline to `grcat`, restores
`FS` (and `FIELDWIDTHS` or `FPAT`, if necessary), `RS`, and `$0`,
initializes `_gr_count` to zero
(it is used later), and makes `_gr_inited` nonzero.

The `getgrnam()` function takes a group name as its argument, and if that
group exists, it is returned.
Otherwise, it
relies on the array reference to a nonexistent
element to create the element with the null string as its value:

```
function getgrnam(group)
{
    _gr_init()
    return _gr_byname[group]
}
```

The `getgrgid()` function is similar; it takes a numeric group ID and
looks up the information associated with that group ID:

```
function getgrgid(gid)
{
    _gr_init()
    return _gr_bygid[gid]
}
```

The `getgruser()` function does not have a C counterpart. It takes a
username and returns the list of groups that have the user as a member:

```
function getgruser(user)
{
    _gr_init()
    return _gr_groupsbyuser[user]
}
```

The `getgrent()` function steps through the database one entry at a time.
It uses `_gr_count` to track its position in the list:

```
function getgrent()
{
    _gr_init()
    if (++_gr_count in _gr_bycount)
        return _gr_bycount[_gr_count]
```

```
    return ""
}
```

The `endgrent()` function resets `_gr_count` to zero so that `getgrent()` can
start over again:

```
function endgrent()
{
    _gr_count = 0
}
```

As with the user database routines, each function calls `_gr_init()` to
initialize the arrays. Doing so only incurs the extra overhead of running
`grcat` if these functions are used (as opposed to moving the body of
`_gr_init()` into a `BEGIN` rule).

Most of the work is in scanning the database and building the various
associative arrays. The functions that the user calls are themselves very
simple, relying on `awk`’s associative arrays to do work.

The `id` program in [Printing Out User Information](#Id-Program)
uses these functions.

---

Next: [Summary](#Library-Functions-Summary), Previous: [Reading the Group Database](#Group-Functions), Up: [A Library of `awk` Functions](#Library-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.7 Traversing Arrays of Arrays [¶](#Traversing-Arrays-of-Arrays)

[Arrays of Arrays](#Arrays-of-Arrays) described how `gawk`
provides arrays of arrays. In particular, any element of
an array may be either a scalar or another array. The
`isarray()` function (see [Getting Type Information](#Type-Functions))
lets you distinguish an array
from a scalar.
The following function, `walk_array()`, recursively traverses
an array, printing the element indices and values.
You call it with the array and a string representing the name
of the array:

```
function walk_array(arr, name,      i)
{
    for (i in arr) {
        if (isarray(arr[i]))
            walk_array(arr[i], (name "[" i "]"))
        else
            printf("%s[%s] = %s\n", name, i, arr[i])
    }
}
```

It works by looping over each element of the array. If any given
element is itself an array, the function calls itself recursively,
passing the subarray and a new string representing the current index.
Otherwise, the function simply prints the element’s name, index, and value.
Here is a main program to demonstrate:

```
BEGIN {
    a[1] = 1
    a[2][1] = 21
    a[2][2] = 22
    a[3] = 3
    a[4][1][1] = 411
    a[4][2] = 42

    walk_array(a, "a")
}
```

When run, the program produces the following output:

```
$ gawk -f walk_array.awk
-| a[1] = 1
-| a[2][1] = 21
-| a[2][2] = 22
-| a[3] = 3
-| a[4][1][1] = 411
-| a[4][2] = 42
```

The function just presented simply prints the
name and value of each scalar array element. However, it is easy to
generalize it, by passing in the name of a function to call
when walking an array. The modified function looks like this:

```
function process_array(arr, name, process, do_arrays,   i, new_name)
{
    for (i in arr) {
        new_name = (name "[" i "]")
        if (isarray(arr[i])) {
            if (do_arrays)
                @process(new_name, arr[i])
            process_array(arr[i], new_name, process, do_arrays)
        } else
            @process(new_name, arr[i])
    }
}
```

The arguments are as follows:

`arr`
:   The array.

`name`
:   The name of the array (a string).

`process`
:   The name of the function to call.

`do_arrays`
:   If this is true, the function can handle elements that are subarrays.

If subarrays are to be processed, that is done before walking them further.

When run with the following scaffolding, the function produces the same
results as does the earlier version of `walk_array()`:

```
BEGIN {
    a[1] = 1
    a[2][1] = 21
    a[2][2] = 22
    a[3] = 3
    a[4][1][1] = 411
    a[4][2] = 42

    process_array(a, "a", "do_print", 0)
}

function do_print(name, element)
{
    printf "%s = %s\n", name, element
}
```

---

Next: [Exercises](#Library-Exercises), Previous: [Traversing Arrays of Arrays](#Walking-Arrays), Up: [A Library of `awk` Functions](#Library-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.8 Summary [¶](#Summary-9)

* Reading programs is an excellent way to learn Good Programming.
  The functions and programs provided in this chapter and the next
  are intended to serve that purpose.
* When writing general-purpose library functions, put some thought into how
  to name any global variables so that they won’t conflict with variables
  from a user’s program.
* The functions presented here fit into the following categories:

  General problems
  :   Number-to-string conversion, testing assertions, rounding, random number
      generation, converting characters to numbers, joining strings, getting
      easily usable time-of-day information, and reading a whole file in
      one shot

  Managing data files
  :   Noting data file boundaries, rereading the current file, checking for
      readable files, checking for zero-length files, and treating assignments
      as file names

  Processing command-line options
  :   An `awk` version of the standard C `getopt()` function

  Reading the user and group databases
  :   Two sets of routines that parallel the C library versions

  Traversing arrays of arrays
  :   Two functions that traverse an array of arrays to any depth

---

Previous: [Summary](#Library-Functions-Summary), Up: [A Library of `awk` Functions](#Library-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.9 Exercises [¶](#Exercises-2)

1. In [Checking for Zero-Length Files](#Empty-Files), we presented the `zerofile.awk` program,
   which made use of `gawk`’s `ARGIND` variable. Can this
   problem be solved without relying on `ARGIND`? If so, how?
2. As a related challenge, revise that code to handle the case where
   an intervening value in `ARGV` is a variable assignment.

---

Next: [Advanced Features of `gawk`](#Advanced-Features), Previous: [A Library of `awk` Functions](#Library-Functions), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 11 Practical `awk` Programs [¶](#Practical-awk-Programs)

[A Library of `awk` Functions](#Library-Functions),
presents the idea that reading programs in a language contributes to
learning that language. This chapter continues that theme,
presenting a potpourri of `awk` programs for your reading
enjoyment.
There are three sections.
The first describes how to run the programs presented
in this chapter.

The second presents `awk`
versions of several common POSIX utilities.
These are programs that you are hopefully already familiar with,
and therefore whose problems are understood.
By reimplementing these programs in `awk`,
you can focus on the `awk`-related aspects of solving
the programming problems.

The third is a grab bag of interesting programs.
These solve a number of different data-manipulation and management
problems. Many of the programs are short, which emphasizes `awk`’s
ability to do a lot in just a few lines of code.

Many of these programs use library functions presented in
[A Library of `awk` Functions](#Library-Functions).

* [Running the Example Programs](#Running-Examples)
* [Reinventing Wheels for Fun and Profit](#Clones)
* [A Grab Bag of `awk` Programs](#Miscellaneous-Programs)
* [Summary](#Programs-Summary)
* [Exercises](#Programs-Exercises)

---

Next: [Reinventing Wheels for Fun and Profit](#Clones), Up: [Practical `awk` Programs](#Sample-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 11.1 Running the Example Programs [¶](#Running-the-Example-Programs)

To run a given program, you would typically do something like this:

```
awk -f program -- options files
```

Here, program is the name of the `awk` program (such as
`cut.awk`), options are any command-line options for the
program that start with a ‘`-`’, and files are the actual data files.

If your system supports the ‘`#!`’ executable interpreter mechanism
(see [Executable `awk` Programs](#Executable-Scripts)),
you can instead run your program directly:

```
cut.awk -c1-8 myfiles > results
```

If your `awk` is not `gawk`, you may instead need to use this:

```
cut.awk -- -c1-8 myfiles > results
```

---

Next: [A Grab Bag of `awk` Programs](#Miscellaneous-Programs), Previous: [Running the Example Programs](#Running-Examples), Up: [Practical `awk` Programs](#Sample-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 11.2 Reinventing Wheels for Fun and Profit [¶](#Reinventing-Wheels-for-Fun-and-Profit)

This section presents a number of POSIX utilities implemented in
`awk`. Reinventing these programs in `awk` is often enjoyable,
because the algorithms can be very clearly expressed, and the code is usually
very concise and simple. This is true because `awk` does so much for you.

It should be noted that these programs are not necessarily intended to
replace the installed versions on your system.
Nor may all of these programs be fully compliant with the most recent
POSIX standard. This is not a problem; their
purpose is to illustrate `awk` language programming for “real-world”
tasks.

The programs are presented in alphabetical order.

* [Cutting Out Fields and Columns](#Cut-Program)
* [Searching for Regular Expressions in Files](#Egrep-Program)
* [Printing Out User Information](#Id-Program)
* [Splitting a Large File into Pieces](#Split-Program)
* [Duplicating Output into Multiple Files](#Tee-Program)
* [Printing Nonduplicated Lines of Text](#Uniq-Program)
* [Counting Things](#Wc-Program)

---

Next: [Searching for Regular Expressions in Files](#Egrep-Program), Up: [Reinventing Wheels for Fun and Profit](#Clones)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.2.1 Cutting Out Fields and Columns [¶](#Cutting-Out-Fields-and-Columns)

The `cut` utility selects, or “cuts,” characters or fields
from its standard input and sends them to its standard output.
Fields are separated by TABs by default,
but you may supply a command-line option to change the field
*delimiter* (i.e., the field-separator character). `cut`’s
definition of fields is less general than `awk`’s.

A common use of `cut` might be to pull out just the login names of
logged-on users from the output of `who`. For example, the following
pipeline generates a sorted, unique list of the logged-on users:

```
who | cut -c1-8 | sort | uniq
```

The options for `cut` are:

`-c list`
:   Use list as the list of characters to cut out. Items within the list
    may be separated by commas, and ranges of characters can be separated with
    dashes. The list ‘`1-8,15,22-35`’ specifies characters 1 through
    8, 15, and 22 through 35.

`-d delim`
:   Use delim as the field-separator character instead of the TAB
    character.

`-f list`
:   Use list as the list of fields to cut out.

`-s`
:   Suppress printing of lines that do not contain the field delimiter.

The `awk` implementation of `cut` uses the `getopt()` library
function (see [Processing Command-Line Options](#Getopt-Function))
and the `join()` library function
(see [Merging an Array into a String](#Join-Function)).

The current POSIX version of `cut` has options to cut fields based on
both bytes and characters. This version does not attempt to implement those options,
as `awk` works exclusively in terms of characters.

The program begins with a comment describing the options, the library
functions needed, and a `usage()` function that prints out a usage
message and exits. `usage()` is called if invalid arguments are
supplied:

```
# cut.awk --- implement cut in awk

# Options:
#    -c list     Cut characters
#    -f list     Cut fields
#    -d c        Field delimiter character
#
#    -s          Suppress lines without the delimiter
#
# Requires getopt() and join() library functions
```

```
function usage()
{
    print("usage: cut [-f list] [-d c] [-s] [files...]") > "/dev/stderr"
    print("       cut [-c list] [files...]") > "/dev/stderr"
    exit 1
}
```

Next comes a `BEGIN` rule that parses the command-line options.
It sets `FS` to a single TAB character, because that is `cut`’s
default field separator. The rule then sets the output field separator to be the
same as the input field separator. A loop using `getopt()` steps
through the command-line options. Exactly one of the variables
`by_fields` or `by_chars` is set to true, to indicate that
processing should be done by fields or by characters, respectively.
When cutting by characters, the output field separator is set to the null
string:

```
BEGIN {
    FS = "\t"    # default
    OFS = FS
    while ((c = getopt(ARGC, ARGV, "sf:c:d:")) != -1) {
        if (c == "f") {
            by_fields = 1
            fieldlist = Optarg
        } else if (c == "c") {
            by_chars = 1
            fieldlist = Optarg
            OFS = ""
        } else if (c == "d") {
            if (length(Optarg) > 1) {
                printf("cut: using first character of %s" \
                       " for delimiter\n", Optarg) > "/dev/stderr"
                Optarg = substr(Optarg, 1, 1)
            }
            fs = FS = Optarg
            OFS = FS
            if (FS == " ")    # defeat awk semantics
                FS = "[ ]"
        } else if (c == "s")
            suppress = 1
        else
            usage()
    }

    # Clear out options
    for (i = 1; i < Optind; i++)
        ARGV[i] = ""
```

The code must take
special care when the field delimiter is a space. Using
a single space (`" "`) for the value of `FS` is
incorrect—`awk` would separate fields with runs of spaces,
TABs, and/or newlines, and we want them to be separated with individual
spaces.
To this end, we save the original space character in the variable
`fs` for later use; after setting `FS` to `"[ ]"` we can’t
use it directly to see if the field delimiter character is in the string.

Also remember that after `getopt()` is through
(as described in [Processing Command-Line Options](#Getopt-Function)),
we have to
clear out all the elements of `ARGV` from 1 to `Optind`,
so that `awk` does not try to process the command-line options
as file names.

After dealing with the command-line options, the program verifies that the
options make sense. Only one or the other of `-c` and `-f`
should be used, and both require a field list. Then the program calls
either `set_fieldlist()` or `set_charlist()` to pull apart the
list of fields or characters:

```
    if (by_fields && by_chars)
        usage()

    if (by_fields == 0 && by_chars == 0)
        by_fields = 1    # default
```

```
    if (fieldlist == "") {
        print "cut: needs list for -c or -f" > "/dev/stderr"
        exit 1
    }
```

```
    if (by_fields)
        set_fieldlist()
    else
        set_charlist()
}
```

`set_fieldlist()` splits the field list apart at the commas
into an array. Then, for each element of the array, it looks to
see if the element is actually a range, and if so, splits it apart.
The function checks the range
to make sure that the first number is smaller than the second.
Each number in the list is added to the `flist` array, which
simply lists the fields that will be printed. Normal field splitting
is used. The program lets `awk` handle the job of doing the
field splitting:

```
function set_fieldlist(        n, m, i, j, k, f, g)
{
    n = split(fieldlist, f, ",")
    j = 1    # index in flist
    for (i = 1; i <= n; i++) {
        if (index(f[i], "-") != 0) { # a range
            m = split(f[i], g, "-")
```

```
            if (m != 2 || g[1] >= g[2]) {
                printf("cut: bad field list: %s\n",
                                  f[i]) > "/dev/stderr"
                exit 1
            }
```

```
            for (k = g[1]; k <= g[2]; k++)
                flist[j++] = k
        } else
            flist[j++] = f[i]
    }
    nfields = j - 1
}
```

The `set_charlist()` function is more complicated than
`set_fieldlist()`.
The idea here is to use `gawk`’s `FIELDWIDTHS` variable
(see [Reading Fixed-Width Data](#Constant-Size)),
which describes constant-width input. When using a character list, that is
exactly what we have.

Setting up `FIELDWIDTHS` is more complicated than simply listing the
fields that need to be printed. We have to keep track of the fields to
print and also the intervening characters that have to be skipped.
For example, suppose you wanted characters 1 through 8, 15, and
22 through 35. You would use ‘`-c 1-8,15,22-35`’. The necessary value
for `FIELDWIDTHS` is `"8 6 1 6 14"`. This yields five
fields, and the fields to print
are `$1`, `$3`, and `$5`.
The intermediate fields are *filler*,
which is stuff in between the desired data.
`flist` lists the fields to print, and `t` tracks the
complete field list, including filler fields:

```
function set_charlist(    field, i, j, f, g, n, m, t,
                          filler, last, len)
{
    field = 1   # count total fields
    n = split(fieldlist, f, ",")
    j = 1       # index in flist
    for (i = 1; i <= n; i++) {
        if (index(f[i], "-") != 0) { # range
            m = split(f[i], g, "-")
            if (m != 2 || g[1] >= g[2]) {
                printf("cut: bad character list: %s\n",
                               f[i]) > "/dev/stderr"
                exit 1
            }
            len = g[2] - g[1] + 1
            if (g[1] > 1)  # compute length of filler
                filler = g[1] - last - 1
            else
                filler = 0
```

```
            if (filler)
                t[field++] = filler
```

```
            t[field++] = len  # length of field
            last = g[2]
            flist[j++] = field - 1
        } else {
            if (f[i] > 1)
                filler = f[i] - last - 1
            else
                filler = 0
            if (filler)
                t[field++] = filler
            t[field++] = 1
            last = f[i]
            flist[j++] = field - 1
        }
    }
    FIELDWIDTHS = join(t, 1, field - 1)
    nfields = j - 1
}
```

Next is the rule that processes the data. If the `-s` option
is given, then `suppress` is true. The first `if` statement
makes sure that the input record does have the field separator. If
`cut` is processing fields, `suppress` is true, and the field
separator character is not in the record, then the record is skipped.

If the record is valid, then `gawk` has split the data
into fields, either using the character in `FS` or using fixed-length
fields and `FIELDWIDTHS`. The loop goes through the list of fields
that should be printed. The corresponding field is printed if it contains data.
If the next field also has data, then the separator character is
written out between the fields:

```
{
    if (by_fields && suppress && index($0, fs) == 0)
        next

    for (i = 1; i <= nfields; i++) {
        if ($flist[i] != "") {
            printf "%s", $flist[i]
            if (i < nfields && $flist[i+1] != "")
                printf "%s", OFS
        }
    }
    print ""
}
```

This version of `cut` relies on `gawk`’s `FIELDWIDTHS`
variable to do the character-based cutting. It is possible in
other `awk` implementations to use `substr()`
(see [String-Manipulation Functions](#String-Functions)), but
it is also extremely painful.
The `FIELDWIDTHS` variable supplies an elegant solution to the problem
of picking the input line apart by characters.

---

Next: [Printing Out User Information](#Id-Program), Previous: [Cutting Out Fields and Columns](#Cut-Program), Up: [Reinventing Wheels for Fun and Profit](#Clones)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.2.2 Searching for Regular Expressions in Files [¶](#Searching-for-Regular-Expressions-in-Files)

The `grep` family of programs searches files for patterns.
These programs have an unusual history.
Initially there was `grep` (Global Regular Expression Print),
which used what are now called Basic Regular Expressions (BREs).
Later there was `egrep` (Extended `grep`) which used
what are now called Extended Regular Expressions (EREs). (These are almost
identical to those available in `awk`; see [Regular Expressions](#Regexp)).
There was also `fgrep` (Fast `grep`), which searched
for matches of one or more fixed strings.

POSIX chose to combine these three programs into one, simply named
`grep`. On a POSIX system, `grep`’s default behavior
is to search using BREs. You use `-E` to specify the use
of EREs, and `-F` to specify searching for fixed strings.

In practice, systems continue to come with separate `egrep`
and `fgrep` utilities, for backwards compatibility. This
section provides an `awk` implementation of `egrep`,
which supports all of the POSIX-mandated options.
You invoke it as follows:

```
egrep [options] 'pattern' files ...
```

The pattern is a regular expression. In typical usage, the regular
expression is quoted to prevent the shell from expanding any of the
special characters as file name wildcards. Normally, `egrep`
prints the lines that matched. If multiple file names are provided on
the command line, each output line is preceded by the name of the file
and a colon.

The options to `egrep` are as follows:

`-c`
:   Print a count of the lines that matched the pattern, instead of the
    lines themselves.

`-e pattern`
:   Use pattern as the regexp to match. The purpose of the `-e`
    option is to allow patterns that start with a ‘`-`’.

`-i`
:   Ignore case distinctions in both the pattern and the input data.

`-l`
:   Only print (list) the names of the files that matched, not the lines that matched.

`-q`
:   Be quiet. No output is produced and the exit value indicates whether
    the pattern was matched.

`-s`
:   Be silent. Do not print error messages for files that could
    not be opened.

`-v`
:   Invert the sense of the test. `egrep` prints the lines that do
    *not* match the pattern and exits successfully if the pattern is not
    matched.

`-x`
:   Match the entire input line in order to consider the match as having
    succeeded.

This version uses the `getopt()` library function
(see [Processing Command-Line Options](#Getopt-Function)) and `gawk`’s
`BEGINFILE` and `ENDFILE` special patterns
(see [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE)).

The program begins with descriptive comments and then a `BEGIN` rule
that processes the command-line arguments with `getopt()`. The `-i`
(ignore case) option is particularly easy with `gawk`; we just use the
`IGNORECASE` predefined variable
(see [Predefined Variables](#Built_002din-Variables)):

```
# egrep.awk --- simulate egrep in awk

# Options:
#    -c    count of lines
#    -e    argument is pattern
#    -i    ignore case
#    -l    print filenames only
#    -n    add line number to output
#    -q    quiet - use exit value
#    -s    silent - don't print errors
#    -v    invert test, success if no match
#    -x    the entire line must match
#
# Requires getopt library function
# Uses IGNORECASE, BEGINFILE and ENDFILE
# Invoke using gawk -f egrep.awk -- options ...

BEGIN {
    while ((c = getopt(ARGC, ARGV, "ce:ilnqsvx")) != -1) {
        if (c == "c")
            count_only++
        else if (c == "e")
            pattern = Optarg
        else if (c == "i")
            IGNORECASE = 1
        else if (c == "l")
            filenames_only++
        else if (c == "n")
            line_numbers++
        else if (c == "q")
            no_print++
        else if (c == "s")
            no_errors++
        else if (c == "v")
            invert++
        else if (c == "x")
            full_line++
        else
            usage()
    }
```

Note the comment about invocation: Because several of the options overlap
with `gawk`’s, a `--` is needed to tell `gawk`
to stop looking for options.

Next comes the code that handles the `egrep`-specific behavior.
`egrep` uses the first nonoption on the command line
if no pattern is supplied with `-e`.
If the pattern is empty, that means no pattern was supplied, so it’s
necessary to print an error message and exit.
The command-line arguments up to `ARGV[Optind]`
are cleared, so that `gawk` won’t try to process them as files. If no
files are specified, the standard input is used, and if multiple files are
specified, we make sure to note this so that the file names can precede the
matched lines in the output:

```
    if (pattern == "")
        pattern = ARGV[Optind++]

    if (pattern == "")
      usage()

    for (i = 1; i < Optind; i++)
        ARGV[i] = ""

    if (Optind >= ARGC) {
        ARGV[1] = "-"
        ARGC = 2
    } else if (ARGC - Optind > 1)
        do_filenames++
}
```

The `BEGINFILE` rule executes
when each new file is processed. In this case, it is fairly simple; it
initializes a variable `fcount` to zero. `fcount` tracks
how many lines in the current file matched the pattern.

Here also is where we implement the `-s` option. We check
if `ERRNO` has been set, and if `-s` was supplied.
In that case, it’s necessary to move on to the next file. Otherwise
`gawk` would exit with an error:

```
BEGINFILE {
    fcount = 0
    if (ERRNO && no_errors)
        nextfile
}
```

The `ENDFILE` rule executes after each file has been processed.
It affects the output only when the user wants a count of the number of lines that
matched. `no_print` is true only if the exit status is desired.
`count_only` is true if line counts are desired. `egrep`
therefore only prints line counts if printing and counting are enabled.
The output format must be adjusted depending upon the number of files to
process. Finally, `fcount` is added to `total`, so that we
know the total number of lines that matched the pattern:

```
ENDFILE {
    if (! no_print && count_only) {
        if (do_filenames)
            print file ":" fcount
        else
            print fcount
    }
```

```
    total += fcount
}
```

The following rule does most of the work of matching lines. The variable
`matches` is true (non-zero) if the line matched the pattern.
If the user specified that the entire line must match (with `-x`),
the code checks this condition by looking at the values of
`RSTART` and `RLENGTH`. If those indicate that the match
is not over the full line, `matches` is set to zero (false).

If the user
wants lines that did not match, we invert the sense of `matches`
using the ‘`!`’ operator. We then increment `fcount` with the value of
`matches`, which is either one or zero, depending upon a
successful or unsuccessful match. If the line does not match, the
`next` statement just moves on to the next input line.

We make a number of additional tests, but only if we
are not counting lines. First, if the user only wants the exit status
(`no_print` is true), then it is enough to know that *one*
line in this file matched, and we can skip on to the next file with
`nextfile`. Similarly, if we are only printing file names, we can
print the file name, and then skip to the next file with `nextfile`.
Finally, each line is printed, with a leading file name,
optional colon and line number, and the final colon
if necessary:

```
{
    matches = match($0, pattern)
    if (matches && full_line && (RSTART != 1 || RLENGTH != length()))
         matches = 0

    if (invert)
        matches = ! matches

    fcount += matches    # 1 or 0

    if (! matches)
        next

    if (! count_only) {
        if (no_print)
            nextfile

        if (filenames_only) {
            print FILENAME
            nextfile
        }

        if (do_filenames)
            if (line_numbers)
               print FILENAME ":" FNR ":" $0
            else
               print FILENAME ":" $0
        else
            print
    }
}
```

The `END` rule takes care of producing the correct exit status. If
there are no matches, the exit status is one; otherwise, it is zero:

```
END {
    exit (total == 0)
}
```

The `usage()` function prints a usage message in case of invalid options,
and then exits:

```
function usage()
{
    print("Usage:\tegrep [-cilnqsvx] [-e pat] [files ...]") > "/dev/stderr"
    print("\tegrep [-cilnqsvx] pat [files ...]") > "/dev/stderr"
    exit 1
}
```

---

Next: [Splitting a Large File into Pieces](#Split-Program), Previous: [Searching for Regular Expressions in Files](#Egrep-Program), Up: [Reinventing Wheels for Fun and Profit](#Clones)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.2.3 Printing Out User Information [¶](#Printing-Out-User-Information)

The `id` utility lists a user’s real and effective user ID numbers,
real and effective group ID numbers, and the user’s group set, if any.
`id` only prints the effective user ID and group ID if they are
different from the real ones. If possible, `id` also supplies the
corresponding user and group names. The output might look like this:

```
$ id
-| uid=1000(arnold) gid=1000(arnold) groups=1000(arnold),4(adm),7(lp),27(sudo)
```

This information is part of what is provided by `gawk`’s
`PROCINFO` array (see [Predefined Variables](#Built_002din-Variables)).
However, the `id` utility provides a more palatable output than just
individual numbers.

The POSIX version of `id` takes several options that give you
control over the output’s format, such as printing only real ids, or printing
only numbers or only names. Additionally, you can print the information
for a specific user, instead of that of the current user.

Here is a version of POSIX `id` written in `awk`.
It uses the `getopt()` library function
(see [Processing Command-Line Options](#Getopt-Function)),
the user database library functions
(see [Reading the User Database](#Passwd-Functions)),
and the group database library functions
(see [Reading the Group Database](#Group-Functions))
from [A Library of `awk` Functions](#Library-Functions).

The program is moderately straightforward. All the work is done in the
`BEGIN` rule.
It starts with explanatory comments, a list of options,
and then a `usage()` function:

```
# id.awk --- implement id in awk
#
# Requires user and group library functions and getopt

# output is:
# uid=12(foo) euid=34(bar) gid=3(baz) \
#             egid=5(blat) groups=9(nine),2(two),1(one)

# Options:
#   -G Output all group ids as space separated numbers (ruid, euid, groups)
#   -g Output only the euid as a number
#   -n Output name instead of the numeric value (with -g/-G/-u)
#   -r Output ruid/rguid instead of effective id
#   -u Output only effective user id, as a number
```

```
function usage()
{
    printf("Usage:\n" \
           "\tid [user]\n" \
           "\tid -G [-n] [user]\n" \
           "\tid -g [-nr] [user]\n" \
           "\tid -u [-nr] [user]\n") > "/dev/stderr"

    exit 1
}
```

The first step is to parse the options using `getopt()`,
and to set various flag variables according to the options given:

```
BEGIN {
    # parse args
    while ((c = getopt(ARGC, ARGV, "Ggnru")) != -1) {
        if (c == "G")
            groupset_only++
        else if (c == "g")
            egid_only++
        else if (c == "n")
            names_not_groups++
        else if (c == "r")
            real_ids_only++
        else if (c == "u")
            euid_only++
        else
            usage()
    }
```

The next step is to check that no conflicting options were
provided. `-G` and `-r` are mutually exclusive.
It is also not allowed to provide more than one user name
on the command line:

```
    if (groupset_only && real_ids_only)
        usage()
    else if (ARGC - Optind > 1)
        usage()
```

The user and group ID numbers are obtained from
`PROCINFO` for the current user, or from the
user and password databases for a user supplied on
the command line. In the latter case, `real_ids_only`
is set, since it’s not possible to print information about
the effective user and group IDs:

```
    if (ARGC - Optind == 0) {
        # gather info for current user
        uid = PROCINFO["uid"]
        euid = PROCINFO["euid"]
        gid = PROCINFO["gid"]
        egid = PROCINFO["egid"]
        for (i = 1; ("group" i) in PROCINFO; i++)
            groupset[i] = PROCINFO["group" i]
    } else {
        fill_info_for_user(ARGV[ARGC-1])
        real_ids_only++
    }
```

The test in the `for` loop is worth noting.
Any supplementary groups in the `PROCINFO` array have the
indices `"group1"` through `"groupN"` for some
N (i.e., the total number of supplementary groups).
However, we don’t know in advance how many of these groups
there are.

This loop works by starting at one, concatenating the value with
`"group"`, and then using `in` to see if that value is
in the array (see [Referring to an Array Element](#Reference-to-Elements)). Eventually, `i` increments past
the last group in the array and the loop exits.

The loop is also correct if there are *no* supplementary
groups; then the condition is false the first time it’s
tested, and the loop body never executes.

Now, based on the options, we decide what information to print.
For `-G` (print just the group set), we then select
whether to print names or numbers. In either case, when done
we exit:

```
    if (groupset_only) {
        if (names_not_groups) {
            for (i = 1; i in groupset; i++) {
                entry = getgrgid(groupset[i])
                name = get_first_field(entry)
                printf("%s", name)
                if ((i + 1) in groupset)
                    printf(" ")
            }
        } else {
            for (i = 1; i in groupset; i++) {
                printf("%u", groupset[i])
                if ((i + 1) in groupset)
                    printf(" ")
            }
        }

        print ""    # final newline
        exit 0
    }
```

Otherwise, for `-g` (effective group ID only), we
check if `-r` was also provided, in which case we
use the real group ID. Then based on `-n`, we decide
whether to print names or numbers. Here too, when done,
we exit:

```
    else if (egid_only) {
        id = real_ids_only ? gid : egid
        if (names_not_groups) {
            entry = getgrgid(id)
            name = get_first_field(entry)
            printf("%s\n", name)
        } else {
            printf("%u\n", id)
        }

        exit 0
    }
```

The `get_first_field()` function extracts the group name from
the group database entry for the given group ID.

Similar processing logic applies to `-u` (effective user ID only),
combined with `-r` and `-n`:

```
    else if (euid_only) {
        id = real_ids_only ? uid : euid
        if (names_not_groups) {
            entry = getpwuid(id)
            name = get_first_field(entry)
            printf("%s\n", name)
        } else {
            printf("%u\n", id)
        }

        exit 0
    }
```

At this point, we haven’t exited yet, so we print
the regular, default output, based either on the current
user’s information, or that of the user whose name was
provided on the command line. We start with the real user ID:

```
    printf("uid=%d", uid)
    pw = getpwuid(uid)
    print_first_field(pw)
```

The `print_first_field()` function prints the user’s
login name from the password file entry, surrounded by
parentheses. It is shown soon.
Printing the effective user ID is next:

```
    if (euid != uid && ! real_ids_only) {
        printf(" euid=%d", euid)
        pw = getpwuid(euid)
        print_first_field(pw)
    }
```

Similar logic applies to the real and effective group IDs:

```
    printf(" gid=%d", gid)
    pw = getgrgid(gid)
    print_first_field(pw)

    if (egid != gid && ! real_ids_only) {
        printf(" egid=%d", egid)
        pw = getgrgid(egid)
        print_first_field(pw)
    }
```

Finally, we print the group set and the terminating newline:

```
    for (i = 1; i in groupset; i++) {
        if (i == 1)
            printf(" groups=")
        group = groupset[i]
        printf("%d", group)
        pw = getgrgid(group)
        print_first_field(pw)
        if ((i + 1) in groupset)
            printf(",")
    }

    print ""
}
```

The `get_first_field()` function extracts the first field
from a password or group file entry for use as a user or group
name. Fields are separated by ‘`:`’ characters:

```
function get_first_field(str,  a)
{
    if (str != "") {
        split(str, a, ":")
        return a[1]
    }
}
```

This function is then used by `print_first_field()` to
output the given name surrounded by parentheses:

```
function print_first_field(str)
{
    first = get_first_field(str)
    printf("(%s)", first)
}
```

These two functions simply isolate out some code that is used repeatedly,
making the whole program shorter and cleaner. In particular, moving the
check for the empty string into `get_first_field()` saves several
lines of code.

Finally, `fill_info_for_user()` fetches user, group, and group
set information for the user named on the command. The code is fairly
straightforward, merely requiring that we exit if the given user doesn’t
exist:

```
function fill_info_for_user(user,
                            pwent, fields, groupnames, grent, groups, i)
{
    pwent = getpwnam(user)
    if (pwent == "") {
        printf("id: '%s': no such user\n", user) > "/dev/stderr"
        exit 1
    }

    split(pwent, fields, ":")
    uid = fields[3] + 0
    gid = fields[4] + 0
```

Getting the group set is a little awkward. The library routine
`getgruser()` returns a list of group *names*. These
have to be gone through and turned back into group numbers,
so that the rest of the code will work as expected:

```
    groupnames = getgruser(user)
    split(groupnames, groups, " ")
    for (i = 1; i in groups; i++) {
        grent = getgrnam(groups[i])
        split(grent, fields, ":")
        groupset[i] = fields[3] + 0
    }
}
```

---

Next: [Duplicating Output into Multiple Files](#Tee-Program), Previous: [Printing Out User Information](#Id-Program), Up: [Reinventing Wheels for Fun and Profit](#Clones)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.2.4 Splitting a Large File into Pieces [¶](#Splitting-a-Large-File-into-Pieces)

The `split` utility splits large text files into smaller pieces.
The usage follows the POSIX standard for `split` and is as follows:

```
split [-l count] [-a suffix-len] [file [outname]]
split -b N[k|m]] [-a suffix-len] [file [outname]]
```

By default, the output files are named `xaa`, `xab`, and so
on. Each file has 1,000 lines in it, with the likely exception of the
last file.

The `split` program has evolved over time, and the current POSIX
version is more complicated than the original Unix version. The options
and what they do are as follows:

`-a` suffix-len
:   Use suffix-len characters for the suffix. For example, if suffix-len
    is four, the output files would range from `xaaaa` to `xzzzz`.

`-b` N[`k`|`m`]]
:   Instead of each file containing a specified number of lines, each file
    should have (at most) N bytes. Supplying a trailing ‘`k`’
    multiplies N by 1,024, yielding kilobytes. Supplying a trailing
    ‘`m`’ multiplies N by 1,048,576 (*1,024 \* 1,024*)
    yielding megabytes. (This option is mutually exclusive with `-l`).

`-l` count
:   Each file should have at most count lines, instead of the default
    1,000. (This option is mutually exclusive with `-b`).

If supplied, file is the input file to read. Otherwise standard
input is processed. If supplied, outname is the leading prefix
to use for file names, instead of ‘`x`’.

In order to use the `-b` option, `gawk` should be invoked
with its `-b` option (see [Command-Line Options](#Options)), or with the environment
variable `LC_ALL` set to ‘`C`’, so that each input byte is treated
as a separate character.[79](#FOOT79)

Here is an implementation of `split` in `awk`. It uses the
`getopt()` function presented in [Processing Command-Line Options](#Getopt-Function).

The program begins with a standard descriptive comment and then
a `usage()` function describing the options. The variable
`common` keeps the function’s lines short so that they
look nice on the page:

```
# split.awk --- do split in awk
#
