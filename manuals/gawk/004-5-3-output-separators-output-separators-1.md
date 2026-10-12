### 5.3 Output Separators [¶](#Output-Separators-1)

As mentioned previously, a `print` statement contains a list
of items separated by commas. In the output, the items are normally
separated by single spaces. However, this doesn’t need to be the case;
a single space is simply the default. Any string of
characters may be used as the *output field separator* by setting the
predefined variable `OFS`. The initial value of this variable
is the string `" "` (i.e., a single space).

The output from an entire `print` statement is called an *output
record*. Each `print` statement outputs one output record, and
then outputs a string called the *output record separator* (or
`ORS`). The initial value of `ORS` is the string `"\n"`
(i.e., a newline character). Thus, each `print` statement normally
makes a separate line.

In order to change how output fields and records are separated, assign
new values to the variables `OFS` and `ORS`. The usual
place to do this is in the `BEGIN` rule
(see [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)), so
that it happens before any input is processed. It can also be done
with assignments on the command line, before the names of the input
files, or using the `-v` command-line option
(see [Command-Line Options](#Options)).
The following example prints the first and second fields of each input
record, separated by a semicolon, with a blank line added after each
newline:

```
$ awk 'BEGIN { OFS = ";"; ORS = "\n\n" }
>            { print $1, $2 }' mail-list
-| Amelia;555-5553
-|
-| Anthony;555-3412
-|
-| Becky;555-7685
-|
-| Bill;555-1675
-|
-| Broderick;555-0542
-|
-| Camilla;555-2912
-|
-| Fabius;555-1234
-|
-| Julie;555-6699
-|
-| Martin;555-6480
-|
-| Samuel;555-3430
-|
-| Jean-Paul;555-2127
-|
```

If the value of `ORS` does not contain a newline, the program’s output
runs together on a single line.

---

Next: [Using `printf` Statements for Fancier Printing](#Printf), Previous: [Output Separators](#Output-Separators), Up: [Printing Output](#Printing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.4 Controlling Numeric Output with `print` [¶](#Controlling-Numeric-Output-with-print)

When printing numeric values with the `print` statement,
`awk` internally converts each number to a string of characters
and prints that string. `awk` uses the `sprintf()` function
to do this conversion
(see [String-Manipulation Functions](#String-Functions)).
For now, it suffices to say that the `sprintf()`
function accepts a *format specification* that tells it how to format
numbers (or strings), and that there are a number of different ways in which
numbers can be formatted. The different format specifications are discussed
more fully in
[Format-Control Letters](#Control-Letters).

The predefined variable `OFMT` contains the format specification
that `print` uses with `sprintf()` when it wants to convert a
number to a string for printing.
The default value of `OFMT` is `"%.6g"`.
The way `print` prints numbers can be changed
by supplying a different format specification
for the value of `OFMT`, as shown in the following example:

```
$ awk 'BEGIN {
>   OFMT = "%.0f"  # print numbers as integers (rounds)
>   print 17.23, 17.54 }'
-| 17 18
```

More detail on how `awk` converts numeric values into
strings is provided in [How `awk` Converts Between Strings and Numbers](#Strings-And-Numbers). In particular, for
`print`, `awk` uses the value of `OFMT` instead of
that of `CONVFMT`, but otherwise behaves exactly the same
as described in that section.

According to the POSIX standard, `awk`’s behavior is undefined
if `OFMT` contains anything but a floating-point conversion specification.
(d.c.)

---

Next: [Redirecting Output of `print` and `printf`](#Redirection), Previous: [Controlling Numeric Output with `print`](#OFMT), Up: [Printing Output](#Printing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.5 Using `printf` Statements for Fancier Printing [¶](#Using-printf-Statements-for-Fancier-Printing)

For more precise control over the output format than what is
provided by `print`, use `printf`.
With `printf` you can
specify the width to use for each item, as well as various
formatting choices for numbers (such as what output base to use, whether to
print an exponent, whether to print a sign, and how many digits to print
after the decimal point).

* [Introduction to the `printf` Statement](#Basic-Printf)
* [Format-Control Letters](#Control-Letters)
* [Modifiers for `printf` Formats](#Format-Modifiers)
* [Examples Using `printf`](#Printf-Examples)

---

Next: [Format-Control Letters](#Control-Letters), Up: [Using `printf` Statements for Fancier Printing](#Printf)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 5.5.1 Introduction to the `printf` Statement [¶](#Introduction-to-the-printf-Statement)

A simple `printf` statement looks like this:

```
printf format, item1, item2, ...
```

As for `print`, the entire list of arguments may optionally be
enclosed in parentheses. Here too, the parentheses are necessary if any
of the item expressions uses the ‘`>`’ relational operator; otherwise,
it can be confused with an output redirection (see [Redirecting Output of `print` and `printf`](#Redirection)).

The difference between `printf` and `print` is the format
argument. This is an expression whose value is taken as a string; it
specifies how to output each of the other arguments. It is called the
*format string*.

The format string is very similar to that in the ISO C library function
`printf()`. Most of format is text to output verbatim.
Scattered among this text are *format specifiers*—one per item.
Each format specifier says to output the next item in the argument list
at that place in the format.

The `printf` statement does not automatically append a newline
to its output. It outputs only what the format string specifies.
So if a newline is needed, you must include one in the format string.
The output separator variables `OFS` and `ORS` have no effect
on `printf` statements. For example:

```
$ awk 'BEGIN {
>    ORS = "\nOUCH!\n"; OFS = "+"
>    msg = "Don\47t Panic!"
>    printf "%s\n", msg
> }'
-| Don't Panic!
```

Here, neither the ‘`+`’ nor the ‘`OUCH!`’ appears in
the output message.

---

Next: [Modifiers for `printf` Formats](#Format-Modifiers), Previous: [Introduction to the `printf` Statement](#Basic-Printf), Up: [Using `printf` Statements for Fancier Printing](#Printf)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 5.5.2 Format-Control Letters [¶](#Format_002dControl-Letters)

A format specifier starts with the character ‘`%`’ and ends with
a *format-control letter*—it tells the `printf` statement
how to output one item. The format-control letter specifies what *kind*
of value to print. The rest of the format specifier is made up of
optional *modifiers* that control *how* to print the value, such as
the field width. Here is a list of the format-control letters:

`%a`, `%A`
:   A floating point number of the form
    [`-`]`0xh.hhhhp+-dd`
    (C99 hexadecimal floating point format).
    For `%A`,
    uppercase letters are used instead of lowercase ones.

    > **NOTE:** The current POSIX standard requires support for `%a` and `%A` in
    > `awk`. As far as we know, besides `gawk`, the only other
    > version of `awk` that actually implements it is BWK `awk`.
    > It’s use is thus highly nonportable!
    >
    > Furthermore, these formats are not available on any system where the
    > underlying C library `printf()` function does not support them. As
    > of this writing, among current systems, only older OpenVMS versions are known
    > to not support them.

`%c`
:   Print a number as a character; thus, ‘`printf "%c",
    65`’ outputs the letter ‘`A`’. The output for a string value is
    the first character of the string.

    > **NOTE:** The POSIX standard says the first character of a string is printed.
    > In locales with multibyte characters, `gawk` attempts to
    > convert the leading bytes of the string into a valid wide character
    > and then to print the multibyte encoding of that character.
    > Similarly, when printing a numeric value, `gawk` allows the
    > value to be within the numeric range of values that can be held
    > in a wide character.
    > If the conversion to multibyte encoding fails, `gawk`
    > uses the low eight bits of the value as the character to print.
    >
    > Other `awk` versions generally restrict themselves to printing
    > the first byte of a string or to numeric values within the range of
    > a single byte (0–255).
    > (d.c.)

`%d`, `%i`
:   Print a decimal integer.
    The two control letters are equivalent.
    (The ‘`%i`’ specification is for compatibility with ISO C.)

`%e`, `%E`
:   Print a number in scientific (exponential) notation.
    For example:

    ```
    printf "%4.3e\n", 1950
    ```

    prints ‘`1.950e+03`’, with a total of four significant figures, three of
    which follow the decimal point.
    (The ‘`4.3`’ represents two modifiers,
    discussed in the next subsection.)
    ‘`%E`’ uses ‘`E`’ instead of ‘`e`’ in the output.

`%f`
:   Print a number in floating-point notation.
    For example:

    ```
    printf "%4.3f", 1950
    ```

    prints ‘`1950.000`’, with a minimum of four significant figures, three of
    which follow the decimal point.
    (The ‘`4.3`’ represents two modifiers,
    discussed in the next subsection.)

    On systems supporting IEEE 754 floating-point format, values
    representing negative
    infinity are formatted as
    ‘`-inf`’ or ‘`-infinity`’,
    and positive infinity as
    ‘`inf`’ or ‘`infinity`’.
    The special “not a number” value formats as ‘`-nan`’ or ‘`nan`’
    (see [Floating Point Values They Didn’t Talk About In School](#Strange-values)).

`%F`
:   Like ‘`%f`’, but the infinity and “not a number” values are spelled
    using uppercase letters.

    The ‘`%F`’ format is a POSIX extension to ISO C; not all systems
    support it. On those that don’t, `gawk` uses ‘`%f`’ instead.

`%g`, `%G`
:   Print a number in either scientific notation or in floating-point
    notation, whichever uses fewer characters; if the result is printed in
    scientific notation, ‘`%G`’ uses ‘`E`’ instead of ‘`e`’.

`%o`
:   Print an unsigned octal integer
    (see [Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers)).

`%s`
:   Print a string.

`%u`
:   Print an unsigned decimal integer.
    (This format is of marginal use, because all numbers in `awk`
    are floating point; it is provided primarily for compatibility with C.)

`%x`, `%X`
:   Print an unsigned hexadecimal integer;
    ‘`%X`’ uses the letters ‘`A`’ through ‘`F`’
    instead of ‘`a`’ through ‘`f`’
    (see [Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers)).

`%%`
:   Print a single ‘`%`’.
    This does not consume an
    argument and it ignores any modifiers.

> **NOTE:** When using the integer format-control letters for values that are
> outside the range of the widest C integer type, `gawk` switches to
> the ‘`%g`’ format specifier. If `--lint` is provided on the
> command line (see [Command-Line Options](#Options)), `gawk`
> warns about this. Other versions of `awk` may print invalid
> values or do something else entirely.
> (d.c.)

> **NOTE:** The IEEE 754 standard for floating-point arithmetic allows for special
> values that represent “infinity” (positive and negative) and values
> that are “not a number” (NaN).
>
> Input and output of these values occurs as text strings. This is
> somewhat problematic for the `awk` language, which predates
> the IEEE standard. Further details are provided in
> [Standards Versus Existing Practice](#POSIX-Floating-Point-Problems); please see there.

---

Next: [Examples Using `printf`](#Printf-Examples), Previous: [Format-Control Letters](#Control-Letters), Up: [Using `printf` Statements for Fancier Printing](#Printf)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 5.5.3 Modifiers for `printf` Formats [¶](#Modifiers-for-printf-Formats)

A format specification can also include *modifiers* that can control
how much of the item’s value is printed, as well as how much space it gets.
The modifiers come between the ‘`%`’ and the format-control letter.
We use the bullet symbol “•” in the following examples to
represent
spaces in the output. Here are the possible modifiers, in the order in
which they may appear:

`N$` [¶](#index-positional-specifiers_002c-printf-statement)
:   An integer constant followed by a ‘`$`’ is a *positional specifier*.
    Normally, format specifications are applied to arguments in the order
    given in the format string. With a positional specifier, the format
    specification is applied to a specific argument, instead of what
    would be the next argument in the list. Positional specifiers begin
    counting with one. Thus:

    ```
    printf "%s %s\n", "don't", "panic"
    printf "%2$s %1$s\n", "panic", "don't"
    ```

    prints the famous friendly message twice.

    At first glance, this feature doesn’t seem to be of much use.
    It is in fact a `gawk` extension, intended for use in translating
    messages at runtime.
    See [Rearranging `printf` Arguments](#Printf-Ordering),
    which describes how and why to use positional specifiers.
    For now, we ignore them.

`-` (Minus)
:   The minus sign, used before the width modifier (see later on in
    this list),
    says to left-justify
    the argument within its specified width. Normally, the argument
    is printed right-justified in the specified width. Thus:

    ```
    printf "%-4s", "foo"
    ```

    prints ‘`foo•`’.

space
:   For numeric conversions, prefix positive values with a space and
    negative values with a minus sign.

`+`
:   The plus sign, used before the width modifier (see later on in
    this list),
    says to always supply a sign for numeric conversions, even if the data
    to format is positive. The ‘`+`’ overrides the space modifier.

`#`
:   Use an “alternative form” for certain control letters.
    For ‘`%o`’, supply a leading zero.
    For ‘`%x`’ and ‘`%X`’, supply a leading ‘`0x`’ or ‘`0X`’ for
    a nonzero result.
    For ‘`%e`’, ‘`%E`’, ‘`%f`’, and ‘`%F`’, the result always
    contains a decimal point.
    For ‘`%g`’ and ‘`%G`’, trailing zeros are not removed from the result.

`0`
:   A leading ‘`0`’ (zero) acts as a flag indicating that output should be
    padded with zeros instead of spaces.
    This applies only to the numeric output formats.
    This flag only has an effect when the field width is wider than the
    value to print.

`'`
:   A single quote or apostrophe character is a POSIX extension to ISO C.
    It indicates that the integer part of a floating-point value, or the
    entire part of an integer decimal value, should have a thousands-separator
    character in it. This only works in locales that support such characters.
    For example:

    ```
    $ cat thousands.awk          Show source program
    -| BEGIN { printf "%'d\n", 1234567 }
    $ LC_ALL=C gawk -f thousands.awk
    -| 1234567                   Results in "C" locale
    $ LC_ALL=en_US.UTF-8 gawk -f thousands.awk
    -| 1,234,567                 Results in US English UTF locale
    ```

    For more information about locales and internationalization issues,
    see [Where You Are Makes a Difference](#Locales).

    > **NOTE:** The ‘`'`’ flag is a nice feature, but its use complicates things: it
    > becomes difficult to use it in command-line programs. For information
    > on appropriate quoting tricks, see [Shell Quoting Issues](#Quoting).

width
:   This is a number specifying the desired minimum width of a field. Inserting any
    number between the ‘`%`’ sign and the format-control character forces the
    field to expand to this width. The default way to do this is to
    pad with spaces on the left. For example:

    ```
    printf "%4s", "foo"
    ```

    prints ‘`•foo`’.

    The value of width is a minimum width, not a maximum. If the item
    value requires more than width characters, it can be as wide as
    necessary. Thus, the following:

    ```
    printf "%4s", "foobar"
    ```

    prints ‘`foobar`’.

    Preceding the width with a minus sign causes the output to be
    padded with spaces on the right, instead of on the left.

`.prec`
:   A period followed by an integer constant
    specifies the precision to use when printing.
    The meaning of the precision varies by control letter:

    `%d`, `%i`, `%o`, `%u`, `%x`, `%X`
    :   Minimum number of digits to print.

    `%e`, `%E`, `%f`, `%F`
    :   Number of digits to the right of the decimal point.

    `%g`, `%G`
    :   Maximum number of significant digits.

    `%s`
    :   Maximum number of characters from the string that should print.

    Thus, the following:

    ```
    printf "%.4s", "foobar"
    ```

    prints ‘`foob`’.

The C library `printf`’s dynamic width and prec
capability (e.g., `"%*.*s"`) is supported. Instead of
supplying explicit width and/or prec values in the format
string, they are passed in the argument list. For example:

```
w = 5
p = 3
s = "abcdefg"
printf "%*.*s\n", w, p, s
```

is exactly equivalent to:

```
s = "abcdefg"
printf "%5.3s\n", s
```

Both programs output ‘`••abc`’.
Earlier versions of `awk` did not support this capability.
If you must use such a version, you may simulate this feature by using
concatenation to build up the format string, like so:

```
w = 5
p = 3
s = "abcdefg"
printf "%" w "." p "s\n", s
```

This is not particularly easy to read, but it does work.

C programmers may be used to supplying additional modifiers (‘`h`’,
‘`j`’, ‘`l`’, ‘`L`’, ‘`t`’, and ‘`z`’) in `printf`
format strings. These are not valid in `awk`. Most `awk`
implementations silently ignore them. If `--lint` is provided
on the command line (see [Command-Line Options](#Options)), `gawk` warns about their
use. If `--posix` is supplied, their use is a fatal error.

---

Previous: [Modifiers for `printf` Formats](#Format-Modifiers), Up: [Using `printf` Statements for Fancier Printing](#Printf)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 5.5.4 Examples Using `printf` [¶](#Examples-Using-printf)

The following simple example shows
how to use `printf` to make an aligned table:

```
awk '{ printf "%-10s %s\n", $1, $2 }' mail-list
```

This command
prints the names of the people (`$1`) in the file
`mail-list` as a string of 10 characters that are left-justified. It also
prints the phone numbers (`$2`) next on the line. This
produces an aligned two-column table of names and phone numbers,
as shown here:

```
$ awk '{ printf "%-10s %s\n", $1, $2 }' mail-list
-| Amelia     555-5553
-| Anthony    555-3412
-| Becky      555-7685
-| Bill       555-1675
-| Broderick  555-0542
-| Camilla    555-2912
-| Fabius     555-1234
-| Julie      555-6699
-| Martin     555-6480
-| Samuel     555-3430
-| Jean-Paul  555-2127
```

In this case, the phone numbers had to be printed as strings because
the numbers are separated by dashes. Printing the phone numbers as
numbers would have produced just the first three digits: ‘`555`’.
This would have been pretty confusing.

It wasn’t necessary to specify a width for the phone numbers because
they are last on their lines. They don’t need to have spaces
after them.

The table could be made to look even nicer by adding headings to the
tops of the columns. This is done using a `BEGIN` rule
(see [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND))
so that the headers are only printed once, at the beginning of
the `awk` program:

```
awk 'BEGIN { print "Name      Number"
             print "----      ------" }
           { printf "%-10s %s\n", $1, $2 }' mail-list
```

The preceding example mixes `print` and `printf` statements in
the same program. Using just `printf` statements can produce the
same results:

```
awk 'BEGIN { printf "%-10s %s\n", "Name", "Number"
             printf "%-10s %s\n", "----", "------" }
           { printf "%-10s %s\n", $1, $2 }' mail-list
```

Printing each column heading with the same format specification
used for the column elements ensures that the headings
are aligned just like the columns.

The fact that the same format specification is used three times can be
emphasized by storing it in a variable, like this:

```
awk 'BEGIN { format = "%-10s %s\n"
             printf format, "Name", "Number"
             printf format, "----", "------" }
           { printf format, $1, $2 }' mail-list
```

---

Next: [Special Files for Standard Preopened Data Streams](#Special-FD), Previous: [Using `printf` Statements for Fancier Printing](#Printf), Up: [Printing Output](#Printing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.6 Redirecting Output of `print` and `printf` [¶](#Redirecting-Output-of-print-and-printf)

So far, the output from `print` and `printf` has gone
to the standard
output, usually the screen. Both `print` and `printf` can
also send their output to other places.
This is called *redirection*.

> **NOTE:** When `--sandbox` is specified (see [Command-Line Options](#Options)),
> redirecting output to files, pipes, and coprocesses is disabled.

A redirection appears after the `print` or `printf` statement.
Redirections in `awk` are written just like redirections in shell
commands, except that they are written inside the `awk` program.

There are four forms of output redirection: output to a file, output
appended to a file, output through a pipe to another command, and output
to a coprocess. We show them all for the `print` statement,
but they work identically for `printf`:

`print items > output-file`
:   This redirection prints the items into the output file named
    output-file. The file name output-file can be any
    expression. Its value is changed to a string and then used as a
    file name (see [Expressions](#Expressions)).

    When this type of redirection is used, the output-file is erased
    before the first output is written to it. Subsequent writes to the same
    output-file do not erase output-file, but append to it.
    (This is different from how you use redirections in shell scripts.)
    If output-file does not exist, it is created. For example, here
    is how an `awk` program can write a list of peoples’ names to one
    file named `name-list`, and a list of phone numbers to another file
    named `phone-list`:

    ```
    $ awk '{ print $2 > "phone-list"
    >        print $1 > "name-list" }' mail-list
    $ cat phone-list
    -| 555-5553
    -| 555-3412
    ...
    $ cat name-list
    -| Amelia
    -| Anthony
    ...
    ```

    Each output file contains one name or number per line.

`print items >> output-file`
:   This redirection prints the items into the preexisting output file
    named output-file. The difference between this and the
    single-‘`>`’ redirection is that the old contents (if any) of
    output-file are not erased. Instead, the `awk` output is
    appended to the file.
    If output-file does not exist, then it is created.

`print items | command`
:   It is possible to send output to another program through a pipe
    instead of into a file. This redirection opens a pipe to
    command, and writes the values of items through this pipe
    to another process created to execute command.

    The redirection argument command is actually an `awk`
    expression. Its value is converted to a string whose contents give
    the shell command to be run. For example, the following produces two
    files, one unsorted list of peoples’ names, and one list sorted in reverse
    alphabetical order:

    ```
    awk '{ print $1 > "names.unsorted"
           command = "sort -r > names.sorted"
           print $1 | command }' mail-list
    ```

    The unsorted list is written with an ordinary redirection, while
    the sorted list is written by piping through the `sort` utility.

    The next example uses redirection to mail a message to the mailing
    list `bug-system`. This might be useful when trouble is encountered
    in an `awk` script run periodically for system maintenance:

    ```
    report = "mail bug-system"
    print("Awk script failed:", $0) | report
    print("at record number", FNR, "of", FILENAME) | report
    close(report)
    ```

    The `close()` function is called here because it’s a good idea to close
    the pipe as soon as all the intended output has been sent to it.
    See [Closing Input and Output Redirections](#Close-Files-And-Pipes)
    for more information.

    This example also illustrates the use of a variable to represent
    a file or command—it is not necessary to always
    use a string constant. Using a variable is generally a good idea,
    because (if you mean to refer to that same file or command)
    `awk` requires that the string value be written identically
    every time.

`print items |& command`
:   This redirection prints the items to the input of command.
    The difference between this and the
    single-‘`|`’ redirection is that the output from command
    can be read with `getline`.
    Thus, command is a *coprocess*, which works together with
    but is subsidiary to the `awk` program.

    This feature is a `gawk` extension, and is not available in
    POSIX `awk`.
    See [Using `getline` from a Coprocess](#Getline_002fCoprocess),
    for a brief discussion.
    See [Two-Way Communications with Another Process](#Two_002dway-I_002fO),
    for a more complete discussion.

Redirecting output using ‘`>`’, ‘`>>`’, ‘`|`’, or ‘`|&`’
asks the system to open a file, pipe, or coprocess only if the particular
file or command you specify has not already been written
to by your program or if it has been closed since it was last written to.
In other words, files, pipes, and coprocesses remain open until
explicitly closed. All further `print` and `printf` statements
continue to write to the same open file, pipe, or coprocess.

In the shell, when you are building up a file a line at a time, you
first use ‘`>`’ to create the file, and then you use ‘`>>`’ for
subsequent additions to it, like so:

```
echo Name: Arnold Robbins > data
echo Street Address: 1234 A Pretty Street, NE >> data
echo City and State: MyTown, MyState 12345-6789 >> data
```

In `awk`, the ‘`>`’ and ‘`>>`’ operators are subtly
different. The operator you use the *first time* you write to a
file determines how `awk` will open (or create) the file.
If you use ‘`>`’, the file is truncated, and then all subsequent
output appends data to the file, even if additional `print` or
`printf` statements continue to use ‘`>`’. If you use ‘`>>`’
the first time, then existing data is not truncated, and all subsequent
`print` or `printf` statements append data to the file.

You should be consistent and always use the same operator for all output
to the same file. (You can mix ‘`>`’ and ‘`>>`’, and nothing bad
will happen, but mixing the operators is considered to be bad style in
`awk`. If invoked with the `--lint` option, `gawk`
issues a warning when it encounters both operators being used for the
same open file.)

As mentioned earlier
(see [Points to Remember About `getline`](#Getline-Notes)),
many
Many
older
`awk` implementations limit the number of pipelines that an `awk`
program may have open to just one! In `gawk`, there is no such limit.
`gawk` allows a program to
open as many pipelines as the underlying operating system permits.

| Piping into `sh` |
| --- |
| A particularly powerful way to use redirection is to build command lines and pipe them into the shell, `sh`. For example, suppose you have a list of files brought over from a system where all the file names are stored in uppercase, and you wish to rename them to have names in all lowercase. The following program is both simple and efficient:  ``` { printf("mv %s %s\n", $0, tolower($0)) | "sh" }  END { close("sh") } ```  The `tolower()` function returns its argument string with all uppercase characters converted to lowercase (see [String-Manipulation Functions](#String-Functions)). The program builds up a list of command lines, using the `mv` utility to rename the files. It then sends the list to the shell for execution.  See [Quoting Strings to Pass to the Shell](#Shell-Quoting) for a function that can help in generating command lines to be fed to the shell. |

---

Next: [Special File names in `gawk`](#Special-Files), Previous: [Redirecting Output of `print` and `printf`](#Redirection), Up: [Printing Output](#Printing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.7 Special Files for Standard Preopened Data Streams [¶](#Special-Files-for-Standard-Preopened-Data-Streams)

Running programs conventionally have three input and output streams
already available to them for reading and writing. These are known
as the *standard input*, *standard output*, and *standard
error output*. These open streams (and any other open files or pipes)
are often referred to by the technical term *file descriptors*.

These streams are, by default, connected to your keyboard and screen, but
they are often redirected with the shell, via the ‘`<`’, ‘`<<`’,
‘`>`’, ‘`>>`’, ‘`>&`’, and ‘`|`’ operators. Standard error
is typically used for writing error messages; the reason there are two separate
streams, standard output and standard error, is so that they can be
redirected separately.

In traditional implementations of `awk`, the only way to write an error
message to standard error in an `awk` program is as follows:

```
print "Serious error detected!" | "cat 1>&2"
```

This works by opening a pipeline to a shell command that can access the
standard error stream that it inherits from the `awk` process.
This is far from elegant, and it also requires a
separate process. So people writing `awk` programs often
don’t do this. Instead, they send the error messages to the
screen, like this:

```
print "Serious error detected!" > "/dev/tty"
```

(`/dev/tty` is a special file supplied by the operating system
that is connected to your keyboard and screen. It represents the
“terminal,”[30](#FOOT30) which on modern systems is a keyboard
and screen, not a serial console.)
This generally has the same effect, but not always: although the
standard error stream is usually the screen, it can be redirected; when
that happens, writing to the screen is not correct. In fact, if
`awk` is run from a background job, it may not have a
terminal at all.
Then opening `/dev/tty` fails.

`gawk`, BWK `awk`, and `mawk` provide
special file names for accessing the three standard streams.
If the file name matches one of these special names when `gawk`
(or one of the others) redirects input or output, then it directly uses
the descriptor that the file name stands for. These special
file names work for all operating systems that `gawk`
has been ported to, not just those that are POSIX-compliant:

`/dev/stdin`
:   The standard input (file descriptor 0).

`/dev/stdout`
:   The standard output (file descriptor 1).

`/dev/stderr`
:   The standard error output (file descriptor 2).

With these facilities,
the proper way to write an error message then becomes:

```
print "Serious error detected!" > "/dev/stderr"
```

Note the use of double quotes around the file name.
Like with any other redirection, the value must be a string.
It is a common error to omit the double quotes, which leads
to confusing results.

`gawk` does not treat these file names as special when
in POSIX-compatibility mode. However, because BWK `awk`
supports them, `gawk` does support them even when
invoked with the `--traditional` option (see [Command-Line Options](#Options)).

---

Next: [Closing Input and Output Redirections](#Close-Files-And-Pipes), Previous: [Special Files for Standard Preopened Data Streams](#Special-FD), Up: [Printing Output](#Printing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.8 Special File names in `gawk` [¶](#Special-File-names-in-gawk)

Besides access to standard input, standard output, and standard error,
`gawk` provides access to any open file descriptor.
Additionally, there are special file names reserved for
TCP/IP networking.

* [Accessing Other Open Files with `gawk`](#Other-Inherited-Files)
* [Special Files for Network Communications](#Special-Network)
* [Special File name Caveats](#Special-Caveats)

---

Next: [Special Files for Network Communications](#Special-Network), Up: [Special File names in `gawk`](#Special-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 5.8.1 Accessing Other Open Files with `gawk` [¶](#Accessing-Other-Open-Files-with-gawk)

Besides the `/dev/stdin`, `/dev/stdout`, and `/dev/stderr`
special file names mentioned earlier, `gawk` provides syntax
for accessing any other inherited open file:

`/dev/fd/N`
:   The file associated with file descriptor N. Such a file must
    be opened by the program initiating the `awk` execution (typically
    the shell). Unless special pains are taken in the shell from which
    `gawk` is invoked, only descriptors 0, 1, and 2 are available.

The file names `/dev/stdin`, `/dev/stdout`, and `/dev/stderr`
are essentially aliases for `/dev/fd/0`, `/dev/fd/1`, and
`/dev/fd/2`, respectively. However, those names are more self-explanatory.

Note that using `close()` on a file name of the
form `"/dev/fd/N"`, for file descriptor numbers
above two, does actually close the given file descriptor.

---

Next: [Special File name Caveats](#Special-Caveats), Previous: [Accessing Other Open Files with `gawk`](#Other-Inherited-Files), Up: [Special File names in `gawk`](#Special-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 5.8.2 Special Files for Network Communications [¶](#Special-Files-for-Network-Communications)

`gawk` programs
can open a two-way
TCP/IP connection, acting as either a client or a server.
This is done using a special file name of the form:

```
/net-type/protocol/local-port/remote-host/remote-port
```

The net-type is one of ‘`inet`’, ‘`inet4`’, or ‘`inet6`’.
The protocol is one of ‘`tcp`’ or ‘`udp`’,
and the other fields represent the other essential pieces of information
for making a networking connection.
These file names are used with the ‘`|&`’ operator for communicating
with a coprocess
(see [Two-Way Communications with Another Process](#Two_002dway-I_002fO)).
This is an advanced feature, mentioned here only for completeness.
Full discussion is delayed until
[Using `gawk` for Network Programming](#TCP_002fIP-Networking).

---

Previous: [Special Files for Network Communications](#Special-Network), Up: [Special File names in `gawk`](#Special-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 5.8.3 Special File name Caveats [¶](#Special-File-name-Caveats)

Here are some things to bear in mind when using the
special file names that `gawk` provides:

* Recognition of the file names for the three standard preopened
  files is disabled only in POSIX mode.
* Recognition of the other special file names is disabled if `gawk` is in
  compatibility mode (either `--traditional` or `--posix`;
  see [Command-Line Options](#Options)).
* `gawk` *always*
  interprets these special file names.
  For example, using ‘`/dev/fd/4`’
  for output actually writes on file descriptor 4, and not on a new
  file descriptor that is `dup()`ed from file descriptor 4. Most of
  the time this does not matter; however, it is important to *not*
  close any of the files related to file descriptors 0, 1, and 2.
  Doing so results in unpredictable behavior.

---

Next: [Speeding Up Pipe Output](#Noflush), Previous: [Special File names in `gawk`](#Special-Files), Up: [Printing Output](#Printing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.9 Closing Input and Output Redirections [¶](#Closing-Input-and-Output-Redirections)

If the same file name or the same shell command is used with `getline`
more than once during the execution of an `awk` program
(see [Explicit Input with `getline`](#Getline)),
the file is opened (or the command is executed) the first time only.
At that time, the first record of input is read from that file or command.
The next time the same file or command is used with `getline`,
another record is read from it, and so on.

Similarly, when a file or pipe is opened for output, `awk` remembers
the file name or command associated with it, and subsequent
writes to the same file or command are appended to the previous writes.
The file or pipe stays open until `awk` exits.

This implies that special steps are necessary in order to read the same
file again from the beginning, or to rerun a shell command (rather than
reading more output from the same command). The `close()` function
makes these things possible:

```
close(filename)
```

or:

```
close(command)
```

The argument filename or command can be any expression. Its
value must *exactly* match the string that was used to open the file or
start the command (spaces and other “irrelevant” characters
included). For example, if you open a pipe with this:

```
"sort -r names" | getline foo
```

then you must close it with this:

```
close("sort -r names")
```

Once this function call is executed, the next `getline` from that
file or command, or the next `print` or `printf` to that
file or command, reopens the file or reruns the command.
Because the expression that you use to close a file or pipeline must
exactly match the expression used to open the file or run the command,
it is good practice to use a variable to store the file name or command.
The previous example becomes the following:

```
sortcom = "sort -r names"
sortcom | getline foo
```

```
...
close(sortcom)
```

This helps avoid hard-to-find typographical errors in your `awk`
programs. Here are some of the reasons for closing an output file:

* To write a file and read it back later on in the same `awk`
  program. Close the file after writing it, then
  begin reading it with `getline`.
* To write numerous files, successively, in the same `awk`
  program. If the files aren’t closed, eventually `awk` may exceed a
  system limit on the number of open files in one process. It is best to
  close each one when the program has finished writing it.
* To make a command finish. When output is redirected through a pipe,
  the command reading the pipe normally continues to try to read input
  as long as the pipe is open. Often this means the command cannot
  really do its work until the pipe is closed. For example, if
  output is redirected to the `mail` program, the message is not
  actually sent until the pipe is closed.
* To run the same program a second time, with the same arguments.
  This is not the same thing as giving more input to the first run!

  For example, suppose a program pipes output to the `mail` program.
  If it outputs several lines redirected to this pipe without closing
  it, they make a single message of several lines. By contrast, if the
  program closes the pipe after each line of output, then each line makes
  a separate message.

If you use more files than the system allows you to have open,
`gawk` attempts to multiplex the available open files among
your data files. `gawk`’s ability to do this depends upon the
facilities of your operating system, so it may not always work. It is
therefore both good practice and good portability advice to always
use `close()` on your files when you are done with them.
In fact, if you are using a lot of pipes, it is essential that
you close commands when done. For example, consider something like this:

```
{
    ...
    command = ("grep " $1 " /some/file | my_prog -q " $3)
    while ((command | getline) > 0) {
        process output of command
    }
    # need close(command) here
}
```

This example creates a new pipeline based on data in *each* record.
Without the call to `close()` indicated in the comment, `awk`
creates child processes to run the commands, until it eventually
runs out of file descriptors for more pipelines.

Even though each command has finished (as indicated by the end-of-file
return status from `getline`), the child process is not
terminated;[31](#FOOT31)
more importantly, the file descriptor for the pipe
is not closed and released until `close()` is called or
`awk` exits.

`close()` silently does nothing if given an argument that
does not represent a file, pipe, or coprocess that was opened with
a redirection. In such a case, it returns a negative value,
indicating an error. In addition, `gawk` sets `ERRNO`
to a string indicating the error.

Note also that ‘`close(FILENAME)`’ has no “magic” effects on the
implicit loop that reads through the files named on the command line.
It is, more likely, a close of a file that was never opened with a
redirection, so `awk` silently does nothing, except return
a negative value.

When using the ‘`|&`’ operator to communicate with a coprocess,
it is occasionally useful to be able to close one end of the two-way
pipe without closing the other.
This is done by supplying a second argument to `close()`.
As in any other call to `close()`,
the first argument is the name of the command or special file used
to start the coprocess.
The second argument should be a string, with either of the values
`"to"` or `"from"`. Case does not matter.
As this is an advanced feature, discussion is
delayed until
[Two-Way Communications with Another Process](#Two_002dway-I_002fO),
which describes it in more detail and gives an example.

* [Using `close()`’s Return Value](#Close-Return-Value)

---

Up: [Closing Input and Output Redirections](#Close-Files-And-Pipes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 5.9.1 Using `close()`’s Return Value [¶](#Using-close_0028_0029_0027s-Return-Value)

In many older versions of Unix `awk`, the `close()` function
is actually a statement.
(d.c.)
It is a syntax error to try and use the return
value from `close()`:

```
command = "..."
command | getline info
retval = close(command)  # syntax error in many Unix awks
```

`gawk` treats `close()` as a function.
The return value is −1 if the argument names something
that was never opened with a redirection, or if there is
a system problem closing the file or process.
In these cases, `gawk` sets the predefined variable
`ERRNO` to a string describing the problem.

In `gawk`, starting with version 4.2, when closing a pipe or
coprocess (input or output), the return value is the exit status of the
command, as described in [Table 5.1](#table_002dclose_002dpipe_002dreturn_002dvalues).[32](#FOOT32) Otherwise, it is the return value from the system’s `close()`
or `fclose()` C functions when closing input or output files,
respectively. This value is zero if the close succeeds, or −1
if it fails.
Recent versions of BWK `awk` also return the same values
from `close()`.

| Situation | Return value from `close()` |
| --- | --- |
| Normal exit of command | Command’s exit status |
| Death by signal of command | 256 + number of murderous signal |
| Death by signal of command with core dump | 512 + number of murderous signal |
| Some kind of error | −1 |

**Table 5.1:** Return values from `close()` of a pipe

The POSIX standard is very vague; it says that `close()`
returns zero on success and a nonzero value otherwise. In general,
different implementations vary in what they report when closing
pipes; thus, the return value cannot be used portably.
(d.c.)
In POSIX mode (see [Command-Line Options](#Options)), `gawk` just returns zero
when closing a pipe.

---

Next: [Enabling Nonfatal Output](#Nonfatal), Previous: [Closing Input and Output Redirections](#Close-Files-And-Pipes), Up: [Printing Output](#Printing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.10 Speeding Up Pipe Output [¶](#Speeding-Up-Pipe-Output)

This section describes a `gawk`-specific feature.

Normally, when you send data down a pipeline to a command with
`print` or `printf`, `gawk` *flushes* the
output down the pipe. That is, output is not buffered, but
written directly. This assures, that pipeline output
intermixed with `gawk`’s output comes out in the
expected order:

```
print "something"                       # goes to standard output
print "something else" | "some-command" # also to standard output
print "more stuff"                      # and this too
```

There can be a price to pay for this; flushing data down
the pipeline uses more CPU time, and in certain environments
this can become expensive.

You can tell `gawk` not to flush buffered data in
one of two ways:

* Set `PROCINFO["BUFFERPIPE"]` to any value. When this is done,
  `gawk` will buffer data for all pipelines.
* Set `PROCINFO["command", "BUFFERPIPE"]` to any value.
  In this case, only command’s data will be fully buffered.

You *must* create one or the other of these elements
in `PROCINFO` before the first `print` or
`printf` to the pipeline. Doing so after output has
already been sent is too late.

Be aware that using this feature may change the output behavior of
your programs, so exercise caution.

---

Next: [Summary](#Output-Summary), Previous: [Speeding Up Pipe Output](#Noflush), Up: [Printing Output](#Printing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.11 Enabling Nonfatal Output [¶](#Enabling-Nonfatal-Output)

This section describes a `gawk`-specific feature.

In standard `awk`, output with `print` or `printf`
to a nonexistent file, or some other I/O error (such as filling up the
disk) is a fatal error.

```
$ gawk 'BEGIN { print "hi" > "/no/such/file" }'
error→ gawk: cmd. line:1: fatal: can't redirect to `/no/such/file' (No
error→ such file or directory)
```

`gawk` makes it possible to detect that an error has
occurred, allowing you to possibly recover from the error, or
at least print an error message of your choosing before exiting.
You can do this in one of two ways:

* For all output files, by assigning any value to `PROCINFO["NONFATAL"]`.
* On a per-file basis, by assigning any value to
  `PROCINFO[filename, "NONFATAL"]`.
  Here, filename is the name of the file to which
  you wish output to be nonfatal.

Once you have enabled nonfatal output, you must check `ERRNO`
after every relevant `print` or `printf` statement to
see if something went wrong. It is also a good idea to initialize
`ERRNO` to zero before attempting the output. For example:

```
$ gawk '
> BEGIN {
>     PROCINFO["NONFATAL"] = 1
>     ERRNO = 0
>     print "hi" > "/no/such/file"
>     if (ERRNO) {
>         print("Output failed:", ERRNO) > "/dev/stderr"
>         exit 1
>     }
> }'
error→ Output failed: No such file or directory
```

Here, `gawk` did not produce a fatal error; instead
it let the `awk` program code detect the problem and handle it.

This mechanism works also for standard output and standard error.
For standard output, you may use `PROCINFO["-", "NONFATAL"]`
or `PROCINFO["/dev/stdout", "NONFATAL"]`. For standard error, use
`PROCINFO["/dev/stderr", "NONFATAL"]`.

When attempting to open a TCP/IP socket (see [Using `gawk` for Network Programming](#TCP_002fIP-Networking)),
`gawk` tries multiple times. The `GAWK_SOCK_RETRIES`
environment variable (see [Other Environment Variables](#Other-Environment-Variables)) allows you to
override `gawk`’s builtin default number of attempts. However,
once nonfatal I/O is enabled for a given socket, `gawk` only
retries once, relying on `awk`-level code to notice that there
was a problem.

---

Next: [Exercises](#Output-Exercises), Previous: [Enabling Nonfatal Output](#Nonfatal), Up: [Printing Output](#Printing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.12 Summary [¶](#Summary-4)

* The `print` statement prints comma-separated expressions. Each
  expression is separated by the value of `OFS` and terminated by
  the value of `ORS`. `OFMT` provides the conversion format
  for numeric values for the `print` statement.
* The `printf` statement provides finer-grained control over output,
  with format-control letters for different data types and various flags
  that modify the behavior of the format-control letters.
* Output from both `print` and `printf` may be redirected to
  files, pipes, and coprocesses.
* `gawk` provides special file names for access to standard input,
  output, and error, and for network communications.
* Use `close()` to close open file, pipe, and coprocess redirections.
  For coprocesses, it is possible to close only one direction of the
  communications.
* Normally errors with `print` or `printf` are fatal.
  `gawk` lets you make output errors be nonfatal either for
  all files or on a per-file basis. You must then check for errors
  after every relevant output statement.

---

Previous: [Summary](#Output-Summary), Up: [Printing Output](#Printing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.13 Exercises [¶](#Exercises-1)

1. Rewrite the program:

   ```
   awk 'BEGIN { print "Month Crates"
                print "----- ------" }
              { print $1, "     ", $2 }' inventory-shipped
   ```

   from [Output Separators](#Output-Separators), by using a new value of `OFS`.
2. Use the `printf` statement to line up the headings and table data
   for the `inventory-shipped` example that was covered in [The `print` Statement](#Print).
3. What happens if you forget the double quotes when redirecting
   output, as follows:

   ```
   BEGIN { print "Serious error detected!" > /dev/stderr }
   ```

---

Next: [Patterns, Actions, and Variables](#Patterns-and-Actions), Previous: [Printing Output](#Printing), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 6 Expressions [¶](#Expressions-1)

Expressions are the basic building blocks of `awk` patterns
and actions. An expression evaluates to a value that you can print, test,
or pass to a function. Additionally, an expression
can assign a new value to a variable or a field by using an assignment operator.

An expression can serve as a pattern or action statement on its own.
Most other kinds of
statements contain one or more expressions that specify the data on which to
operate. As in other languages, expressions in `awk` can include
variables, array references, constants, and function calls, as well as
combinations of these with various operators.

* [Constants, Variables, and Conversions](#Values)
* [Operators: Doing Something with Values](#All-Operators)
* [Truth Values and Conditions](#Truth-Values-and-Conditions)
* [Function Calls](#Function-Calls)
* [Operator Precedence (How Operators Nest)](#Precedence)
* [Where You Are Makes a Difference](#Locales)
* [Summary](#Expressions-Summary)

---

Next: [Operators: Doing Something with Values](#All-Operators), Up: [Expressions](#Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.1 Constants, Variables, and Conversions [¶](#Constants_002c-Variables_002c-and-Conversions)

Expressions are built up from values and the operations performed
upon them. This section describes the elementary objects
that provide the values used in expressions.

* [Constant Expressions](#Constants)
* [Using Regular Expression Constants](#Using-Constant-Regexps)
* [Variables](#Variables)
* [Conversion of Strings and Numbers](#Conversion)

---

Next: [Using Regular Expression Constants](#Using-Constant-Regexps), Up: [Constants, Variables, and Conversions](#Values)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.1.1 Constant Expressions [¶](#Constant-Expressions)

The simplest type of expression is the *constant*, which always has
the same value. There are three types of constants: numeric,
string, and regular expression.

Each is used in the appropriate context when you need a data
value that isn’t going to change. Numeric constants can
have different forms, but are internally stored in an identical manner.

* [Numeric and String Constants](#Scalar-Constants)
* [Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers)
* [Regular Expression Constants](#Regexp-Constants)

---

Next: [Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers), Up: [Constant Expressions](#Constants)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.1.1.1 Numeric and String Constants [¶](#Numeric-and-String-Constants)

A *numeric constant* stands for a number. This number can be an
integer, a decimal fraction, or a number in scientific (exponential)
notation.[33](#FOOT33)
Here are some examples of numeric constants that all
have the same value:

```
105
1.05e+2
1050e-1
```

A *string constant* consists of a sequence of characters enclosed in
double quotation marks. For example:

```
"parrot"
```

represents the string whose contents are ‘`parrot`’. Strings in
`gawk` can be of any length, and they can contain any of the possible
eight-bit ASCII characters, including ASCII NUL (character code zero).
Other `awk`
implementations may have difficulty with some character codes.

Some languages allow you to continue long strings across
multiple lines by ending the line with a backslash. For example in C:

```
#include <stdio.h>

int main()
{
    printf("hello, \
world\n");
    return 0;
}
```

In such a case, the C compiler removes both the backslash and the newline,
producing a string as if it had been typed ‘`"hello, world\n"`’.
This is useful when a single string needs to contain a large amount of text.

The POSIX standard says explicitly that newlines are not allowed inside string
constants. And indeed, all `awk` implementations report an error
if you try to do so. For example:

```
$ gawk 'BEGIN { print "hello, 
> world" }'
-| gawk: cmd. line:1: BEGIN { print "hello,
-| gawk: cmd. line:1:               ^ unterminated string
-| gawk: cmd. line:1: BEGIN { print "hello,
-| gawk: cmd. line:1:               ^ syntax error
```

Although POSIX doesn’t define what happens if you use an escaped
newline, as in the previous C example, all known versions of
`awk` allow you to do so. Unfortunately, what each one
does with such a string varies. (d.c.) `gawk`,
`mawk`, and the OpenSolaris POSIX `awk`
(see [Other Freely Available `awk` Implementations](#Other-Versions)) elide the backslash and newline, as in C:

```
$ gawk 'BEGIN { print "hello, \
> world" }'
-| hello, world
```

In POSIX mode (see [Command-Line Options](#Options)), `gawk` does not
allow escaped newlines. Otherwise, it behaves as just described.

BWK `awk`[34](#FOOT34) and BusyBox `awk`
remove the backslash but leave the newline
intact, as part of the string:

```
$ nawk 'BEGIN { print "hello, \
> world" }'
-| hello,
-| world
```

---

Next: [Regular Expression Constants](#Regexp-Constants), Previous: [Numeric and String Constants](#Scalar-Constants), Up: [Constant Expressions](#Constants)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.1.1.2 Octal and Hexadecimal Numbers [¶](#Octal-and-Hexadecimal-Numbers)

In `awk`, all numbers are in decimal (i.e., base 10). Many other
programming languages allow you to specify numbers in other bases, often
octal (base 8) and hexadecimal (base 16).
In octal, the numbers go 0, 1, 2, 3, 4, 5, 6, 7, 10, 11, 12, and so on.
Just as ‘`11`’ in decimal is 1 times 10 plus 1, so
‘`11`’ in octal is 1 times 8 plus 1. This equals 9 in decimal.
In hexadecimal, there are 16 digits. Because the everyday decimal
number system only has ten digits (‘`0`’–‘`9`’), the letters
‘`a`’ through ‘`f`’ represent the rest.
(Case in the letters is usually irrelevant; hexadecimal ‘`a`’ and ‘`A`’
have the same value.)
Thus, ‘`11`’ in
hexadecimal is 1 times 16 plus 1, which equals 17 in decimal.

Just by looking at plain ‘`11`’, you can’t tell what base it’s in.
So, in C, C++, and other languages derived from C,
there is a special notation to signify the base.
Octal numbers start with a leading ‘`0`’,
and hexadecimal numbers start with a leading ‘`0x`’ or ‘`0X`’:

`11`
:   Decimal value 11

`011`
:   Octal 11, decimal value 9

`0x11`
:   Hexadecimal 11, decimal value 17

This example shows the difference:

```
$ gawk 'BEGIN { printf "%d, %d, %d\n", 011, 11, 0x11 }'
-| 9, 11, 17
```

Being able to use octal and hexadecimal constants in your programs is most
useful when working with data that cannot be represented conveniently as
characters or as regular numbers, such as binary data of various sorts.

`gawk` allows the use of octal and hexadecimal
constants in your program text. However, such numbers in the input data
are not treated differently; doing so by default would break old
programs.
(If you really need to do this, use the `--non-decimal-data`
command-line option;
see [Allowing Nondecimal Input Data](#Nondecimal-Data).)
If you have octal or hexadecimal data,
you can use the `strtonum()` function
(see [String-Manipulation Functions](#String-Functions))
to convert the data into a number.
Most of the time, you will want to use octal or hexadecimal constants
when working with the built-in bit-manipulation functions;
see [Bit-Manipulation Functions](#Bitwise-Functions)
for more information.

Unlike in some early C implementations, ‘`8`’ and ‘`9`’ are not
valid in octal constants. For example, `gawk` treats ‘`018`’
as decimal 18:

```
$ gawk 'BEGIN { print "021 is", 021 ; print 018 }'
-| 021 is 17
-| 18
```

Hexadecimal integer constants have been part of the C language
since the beginning. The C99 standard added support for
hexadecimal floating-point values. This is
a floating point number of the form
[`-`]`0xh`[`.hhhh`]`p+-dd`.
The ‘`p`’ introduces the exponent, and is *required*.
The radix point and following digits are optional.
As with integer hexadecimal constants, both ‘`0x`’ and ‘`0X`’
may be used, and case in the letters ‘`a`’ through ‘`f`’
does not matter.

Beginning with version 5.4.0, `gawk` supports hexadecimal
floating point values, as constants in `awk` program source
code, with the `strtonum()` built-in function, and with the
`--non-decimal-data` command-line option.
Here is an example:

```
$ gawk 'BEGIN { print 0xdeadbeefp-1 }'
-| 1.86796e+09
```

For this to work correctly, the version of `strtod()` in the
system’s underlying C library must understand such constants.
On modern systems this isn’t a problem.

Octal and hexadecimal source code constants are a `gawk` extension.
If `gawk` is in compatibility mode
(see [Command-Line Options](#Options)),
they are not available.

| A Constant’s Base Does Not Affect Its Value |
| --- |
| Once a numeric constant has been converted internally into a number, `gawk` no longer remembers what the original form of the constant was; the internal value is always used. This has particular consequences for conversion of numbers to strings:  ``` $ gawk 'BEGIN { printf "0x11 is <%s>\n", 0x11 }' -| 0x11 is <17> ``` |

---

Previous: [Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers), Up: [Constant Expressions](#Constants)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.1.1.3 Regular Expression Constants [¶](#Regular-Expression-Constants)

A *regexp constant* is a regular expression description enclosed in
slashes, such as `/^beginning and end$/`. Most regexps used in
`awk` programs are constant, but the ‘`~`’ and ‘`!~`’
matching operators can also match computed or dynamic regexps
(which are typically just ordinary strings or variables that contain a regexp,
but could be more complex expressions).

---

Next: [Variables](#Variables), Previous: [Constant Expressions](#Constants), Up: [Constants, Variables, and Conversions](#Values)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.1.2 Using Regular Expression Constants [¶](#Using-Regular-Expression-Constants)

Regular expression constants consist of text describing
a regular expression enclosed in slashes (such as `/the +answer/`).
This section describes how such constants work in
POSIX `awk` and `gawk`, and then goes on to describe
*strongly typed regexp constants*, which are a `gawk` extension.

* [Standard Regular Expression Constants](#Standard-Regexp-Constants)
* [Strongly Typed Regexp Constants](#Strong-Regexp-Constants)

---

Next: [Strongly Typed Regexp Constants](#Strong-Regexp-Constants), Up: [Using Regular Expression Constants](#Using-Constant-Regexps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.1.2.1 Standard Regular Expression Constants [¶](#Standard-Regular-Expression-Constants)

When used on the righthand side of the ‘`~`’ or ‘`!~`’
operators, a regexp constant merely stands for the regexp that is to be
matched.
However, regexp constants (such as `/foo/`) may be used like simple expressions.
When a
regexp constant appears by itself, it has the same meaning as if it appeared
in a pattern (i.e., ‘`($0 ~ /foo/)`’).
(d.c.)
See [Expressions as Patterns](#Expression-Patterns).
This means that the following two code segments:

```
if ($0 ~ /barfly/ || $0 ~ /camelot/)
    print "found"
```

and:

```
if (/barfly/ || /camelot/)
    print "found"
```

are exactly equivalent.
One rather bizarre consequence of this rule is that the following
Boolean expression is valid, but does not do what its author probably
intended:

```
