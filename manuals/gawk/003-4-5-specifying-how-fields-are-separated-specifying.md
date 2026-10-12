### 4.5 Specifying How Fields Are Separated [¶](#Specifying-How-Fields-Are-Separated)

The *field separator*, which is either a single character or a regular
expression, controls the way `awk` splits an input record into fields.
`awk` scans the input record for character sequences that
match the separator; the fields themselves are the text between the matches.

In the examples that follow, we use the bullet symbol (•) to
represent spaces in the output.
If the field separator is ‘`oo`’, then the following line:

```
moo goo gai pan
```

is split into three fields: ‘`m`’, ‘`•g`’, and
‘`•gai•pan`’.
Note the leading spaces in the values of the second and third fields.

The field separator is represented by the predefined variable `FS`.
Shell programmers take note: `awk` does *not* use the
name `IFS` that is used by the POSIX-compliant shells (such as
the Unix Bourne shell, `sh`, or Bash).

The value of `FS` can be changed in the `awk` program with the
assignment operator, ‘`=`’ (see [Assignment Expressions](#Assignment-Ops)).
Often, the right time to do this is at the beginning of execution
before any input has been processed, so that the very first record
is read with the proper separator. To do this, use the special
`BEGIN` pattern
(see [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)).
For example, here we set the value of `FS` to the string
`":"`:

```
awk 'BEGIN { FS = ":" } ; { print $2 }'
```

Given the input line:

```
John Q. Smith: 29 Oak St.: Walamazoo: MI 42139
```

this `awk` program extracts and prints the string
‘`•29•Oak•St.`’.

Sometimes the input data contains separator characters that don’t
separate fields the way you thought they would. For instance, the
person’s name in the example we just used might have a title or
suffix attached, such as:

```
John Q. Smith: LXIX: 29 Oak St.: Walamazoo: MI 42139
```

The same program would extract ‘`•LXIX`’ instead of
‘`•29•Oak•St.`’.
If you were expecting the program to print the
address, you would be surprised. The moral is to choose your data layout and
separator characters carefully to prevent such problems.
(If the data is not in a form that is easy to process, perhaps you
can massage it first with a separate `awk` program.)

* [Whitespace Normally Separates Fields](#Default-Field-Splitting)
* [Using Regular Expressions to Separate Fields](#Regexp-Field-Splitting)
* [Making Each Character a Separate Field](#Single-Character-Fields)
* [Working With Comma Separated Value Files](#Comma-Separated-Fields)
* [Setting `FS` from the Command Line](#Command-Line-Field-Separator)
* [Making the Full Line Be a Single Field](#Full-Line-Fields)
* [Field-Splitting Summary](#Field-Splitting-Summary)

---

Next: [Using Regular Expressions to Separate Fields](#Regexp-Field-Splitting), Up: [Specifying How Fields Are Separated](#Field-Separators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.5.1 Whitespace Normally Separates Fields [¶](#Whitespace-Normally-Separates-Fields)

Fields are normally separated by whitespace sequences
(spaces, TABs, and newlines), not by single spaces. Two spaces in a row do not
delimit an empty field. The default value of the field separator `FS`
is a string containing a single space, `" "`. If `awk`
interpreted this value in the usual way, each space character would separate
fields, so two spaces in a row would make an empty field between them.
The reason this does not happen is that a single space as the value of
`FS` is a special case—it is taken to specify the default manner
of delimiting fields.

If `FS` is any other single character, such as `","`, then
each occurrence of that character separates two fields. Two consecutive
occurrences delimit an empty field. If the character occurs at the
beginning or the end of the line, that too delimits an empty field. The
space character is the only single character that does not follow these
rules.

---

Next: [Making Each Character a Separate Field](#Single-Character-Fields), Previous: [Whitespace Normally Separates Fields](#Default-Field-Splitting), Up: [Specifying How Fields Are Separated](#Field-Separators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.5.2 Using Regular Expressions to Separate Fields [¶](#Using-Regular-Expressions-to-Separate-Fields)

The previous subsection
discussed the use of single characters or simple strings as the
value of `FS`.
More generally, the value of `FS` may be a string containing any
regular expression. In this case, each match in the record for the regular
expression separates fields. For example, the assignment:

```
FS = ", \t"
```

makes every area of an input line that consists of a comma followed by a
space and a TAB into a field separator.

For a less trivial example of a regular expression, try using
single spaces to separate fields the way single commas are used.
`FS` can be set to `"[ ]"` (left bracket, space, right
bracket). This regular expression matches a single space and nothing else
(see [Regular Expressions](#Regexp)).

There is an important difference between the two cases of ‘`FS = " "`’
(a single space) and ‘`FS = "[ \t\n]+"`’
(a regular expression matching one or more spaces, TABs, or newlines).
For both values of `FS`, fields are separated by *runs*
(multiple adjacent occurrences) of spaces, TABs,
and/or newlines. However, when the value of `FS` is `" "`,
`awk` first strips leading and trailing whitespace from
the record and then decides where the fields are.
For example, the following pipeline prints ‘`b`’:

```
$ echo ' a b c d ' | awk '{ print $2 }'
-| b
```

However, this pipeline prints ‘`a`’ (note the extra spaces around
each letter):

```
$ echo ' a  b  c  d ' | awk 'BEGIN { FS = "[ \t\n]+" }
>                                  { print $2 }'
-| a
```

In this case, the first field is null, or empty.

The stripping of leading and trailing whitespace also comes into
play whenever `$0` is recomputed. For instance, study this pipeline:

```
$ echo '   a b c d' | awk '{ print; $2 = $2; print }'
-|    a b c d
-| a b c d
```

The first `print` statement prints the record as it was read,
with leading whitespace intact. The assignment to `$2` rebuilds
`$0` by concatenating `$1` through `$NF` together,
separated by the value of `OFS` (which is a space by default).
Because the leading whitespace was ignored when finding `$1`,
it is not part of the new `$0`. Finally, the last `print`
statement prints the new `$0`.

There is an additional subtlety to be aware of when using regular expressions
for field splitting.
It is not well specified in the POSIX standard, or anywhere else, what ‘`^`’
means when splitting fields. Does the ‘`^`’ match only at the beginning of
the entire record? Or is each field separator a new string? It turns out that
different `awk` versions answer this question differently, and you
should not rely on any specific behavior in your programs.
(d.c.)

As a point of information, BWK `awk` allows ‘`^`’
to match only at the beginning of the record. `gawk`
also works this way. For example:

```
$ echo 'xxAA  xxBxx  C' |
> gawk -F '(^x+)|( +)' '{ for (i = 1; i <= NF; i++)
>                             printf "-->%s<--\n", $i }'
-| --><--
-| -->AA<--
-| -->xxBxx<--
-| -->C<--
```

Finally, field splitting with regular expressions works differently than
regexp matching with the `sub()`, `gsub()`, and `gensub()`
(see [String-Manipulation Functions](#String-Functions)). Those functions allow a regexp to match the
empty string; field splitting does not. Thus, for example ‘`FS =
"()"`’ does *not* split fields between characters.

---

Next: [Working With Comma Separated Value Files](#Comma-Separated-Fields), Previous: [Using Regular Expressions to Separate Fields](#Regexp-Field-Splitting), Up: [Specifying How Fields Are Separated](#Field-Separators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.5.3 Making Each Character a Separate Field [¶](#Making-Each-Character-a-Separate-Field)

There are times when you may want to examine each character
of a record separately. This can be done in `gawk` by
simply assigning the null string (`""`) to `FS`. (c.e.)
In this case,
each individual character in the record becomes a separate field.
For example:

```
$ echo a b | gawk 'BEGIN { FS = "" }
>                  {
>                      for (i = 1; i <= NF; i = i + 1)
>                          print "Field", i, "is", $i
>                  }'
-| Field 1 is a
-| Field 2 is
-| Field 3 is b
```

Traditionally, the behavior of `FS` equal to `""` was not defined.
In this case, most versions of Unix `awk` simply treat the entire record
as only having one field.
(d.c.)
In compatibility mode
(see [Command-Line Options](#Options)),
if `FS` is the null string, then `gawk` also
behaves this way.

---

Next: [Setting `FS` from the Command Line](#Command-Line-Field-Separator), Previous: [Making Each Character a Separate Field](#Single-Character-Fields), Up: [Specifying How Fields Are Separated](#Field-Separators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.5.4 Working With Comma Separated Value Files [¶](#Working-With-Comma-Separated-Value-Files)

Many commonly-used tools use a comma to separate fields, instead of whitespace.
This is particularly true of popular spreadsheet programs. There is no
universally accepted standard for the format of these files, although
[RFC 4180](https://www.ietf.org/rfc/rfc4180.html) lists the common
practices.

For decades, anyone wishing to work with CSV files and `awk`
had to “roll their own” solution.
(For an example, see [Defining Fields by Content](#Splitting-By-Content)).
In 2023, Brian Kernighan decided to add CSV support to his version of
`awk`. In order to keep up, `gawk` too provides the same
support as his version.
To use CSV data, invoke `gawk` with either of the
`-k` or `--csv` options.

Fields in CSV files are separated by commas. In order to allow a comma
to appear inside a field (i.e., as data), the field may be quoted
by beginning and ending it with double quotes. In order to allow a double
quote inside a field, the field *must* be quoted, and two double quotes
represent an actual double quote.
The double quote that starts a quoted field must be the first
character after the comma.
[Table 4.1](#table_002dcsv_002dexamples) shows some examples.

| Input | Field Contents |
| --- | --- |
| `abc def` | `abc def` |
| `"quoted data"` | `quoted data` |
| `"quoted, data"` | `quoted, data` |
| `"She said ""Stop!""."` | `She said "Stop!".` |

**Table 4.1:** Examples of CSV data

Additionally, and here’s where it gets messy, newlines are also
allowed inside double-quoted fields!
In order to deal with such things, when processing CSV files,
`gawk` scans the input data looking for newlines that
are not enclosed in double quotes. Thus, use of the `--csv` option
totally overrides normal record processing with `RS` (see [How Input Is Split into Records](#Records)),
as well as field splitting with any of `FS`, `FIELDWIDTHS`,
or `FPAT`.

| Carriage-Return–Line-Feed Line Endings In CSV Files |
| --- |
| `\r\n` *is the invention of the devil.*  — *Brian Kernighan*  Many CSV files are imported from systems where the line terminator for text files is a carriage-return–line-feed pair (CR-LF, ‘`\r`’ followed by ‘`\n`’). For ease of use, when processing CSV files, `gawk` converts CR-LF pairs into a single newline. That is, the ‘`\r`’ is removed.  This occurs only when a CR is paired with an LF; a standalone CR is left alone. This behavior is consistent with Windows systems which automatically convert CR-LF in files into a plain LF in memory, and also with the commonly available `unix2dos` utility program. |

The behavior of the `split()` function (not formally discussed
yet, see [String-Manipulation Functions](#String-Functions)) differs slightly when processing CSV
files. When called with two arguments
(‘`split(string, array)`’), `split()`
does CSV-based splitting. Otherwise, it behaves normally.

If `--csv` has been used, `PROCINFO["CSV"]` will
exist. Otherwise, it will not. See [Built-in Variables That Convey Information](#Auto_002dset).

Finally, if `--csv` has been used, assigning a value
to any of `FS`, `FIELDWIDTHS`, `FPAT`, or
`RS` generates a warning message.

To be clear, `gawk` takes
[RFC 4180](https://www.ietf.org/rfc/rfc4180.html) as its
specification for CSV input data. There are no mechanisms
for accepting nonstandard CSV data, such as files that use
a semicolon instead of a comma as the separator.

---

Next: [Making the Full Line Be a Single Field](#Full-Line-Fields), Previous: [Working With Comma Separated Value Files](#Comma-Separated-Fields), Up: [Specifying How Fields Are Separated](#Field-Separators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.5.5 Setting `FS` from the Command Line [¶](#Setting-FS-from-the-Command-Line)

`FS` can be set on the command line. Use the `-F` option to
do so. For example:

```
awk -F, 'program' input-files
```

sets `FS` to the ‘`,`’ character. Notice that the option uses
an uppercase ‘`F`’ instead of a lowercase ‘`f`’. The latter
option (`-f`) specifies a file containing an `awk` program.

The value used for the argument to `-F` is processed in exactly the
same way as assignments to the predefined variable `FS`.
Any special characters in the field separator must be escaped
appropriately. For example, to use a ‘`\`’ as the field separator
on the command line, you would have to type:

```
# same as FS = "\\"
awk -F\\\\ '...' files ...
```

Because ‘`\`’ is used for quoting in the shell, `awk` sees
‘`-F\\`’. Then `awk` processes the ‘`\\`’ for escape
characters (see [Escape Sequences](#Escape-Sequences)), finally yielding
a single ‘`\`’ to use for the field separator.

As a special case, in compatibility mode
(see [Command-Line Options](#Options)),
if the argument to `-F` is ‘`t`’, then `FS` is set to
the TAB character. If you type ‘`-F\t`’ at the
shell, without any quotes, the ‘`\`’ gets deleted, so `awk`
figures that you really want your fields to be separated with TABs and
not ‘`t`’s. Use ‘`-v FS="t"`’ or ‘`-F"[t]"`’ on the command line
if you really do want to separate your fields with ‘`t`’s.
Use ‘`-F '\t'`’ when not in compatibility mode to specify that TABs
separate fields.

As an example, let’s use an `awk` program file called `edu.awk`
that contains the pattern `/edu/` and the action ‘`print $1`’:

```
/edu/   { print $1 }
```

Let’s also set `FS` to be the ‘`-`’ character and run the
program on the file `mail-list`. The following command prints a
list of the names of the people that work at or attend a university, and
the first three digits of their phone numbers:

```
$ awk -F- -f edu.awk mail-list
-| Fabius       555
-| Samuel       555
-| Jean
```

Note the third line of output. The third line
in the original file looked like this:

```
Jean-Paul    555-2127     jeanpaul.campanorum@nyu.edu     R
```

The ‘`-`’ as part of the person’s name was used as the field
separator, instead of the ‘`-`’ in the phone number that was
originally intended. This demonstrates why you have to be careful in
choosing your field and record separators.

Perhaps the most common use of a single character as the field separator
occurs when processing the Unix system password file. On many Unix
systems, each user has a separate entry in the system password file, with one
line per user. The information in these lines is separated by colons.
The first field is the user’s login name and the second is the user’s
encrypted or shadow password. (A shadow password is indicated by the
presence of a single ‘`x`’ in the second field.) A password file
entry might look like this:

```
arnold:x:2076:10:Arnold Robbins:/home/arnold:/bin/bash
```

The following program searches the system password file and prints
the entries for users whose full name is not indicated:

```
awk -F: '$5 == ""' /etc/passwd
```

---

Next: [Field-Splitting Summary](#Field-Splitting-Summary), Previous: [Setting `FS` from the Command Line](#Command-Line-Field-Separator), Up: [Specifying How Fields Are Separated](#Field-Separators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.5.6 Making the Full Line Be a Single Field [¶](#Making-the-Full-Line-Be-a-Single-Field)

Occasionally, it’s useful to treat the whole input line as a
single field. This can be done easily and portably simply by
setting `FS` to `"\n"` (a newline):[25](#FOOT25)

```
awk -F'\n' 'program' files ...
```

When you do this, `$1` is the same as `$0`.

| Changing `FS` Does Not Affect the Fields |
| --- |
| According to the POSIX standard, `awk` is supposed to behave as if each record is split into fields at the time it is read. In particular, this means that if you change the value of `FS` after a record is read, the values of the fields (i.e., how they were split) should reflect the old value of `FS`, not the new one.    However, many older implementations of `awk` do not work this way. Instead, they defer splitting the fields until a field is actually referenced. The fields are split using the *current* value of `FS`! (d.c.) This behavior can be difficult to diagnose. The following example illustrates the difference between the two methods:  ``` sed 1q /etc/passwd | awk '{ FS = ":" ; print $1 }' ```  which usually prints:  ``` root ```  on an incorrect implementation of `awk`, while `gawk` prints the full first line of the file, something like:  ``` root:x:0:0:Root:/: ```  (The `sed`[26](#FOOT26) command prints just the first line of `/etc/passwd`.) |

---

Previous: [Making the Full Line Be a Single Field](#Full-Line-Fields), Up: [Specifying How Fields Are Separated](#Field-Separators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.5.7 Field-Splitting Summary [¶](#Field_002dSplitting-Summary)

It is important to remember that when you assign a string constant
as the value of `FS`, it undergoes normal `awk` string
processing. For example, with Unix `awk` and `gawk`,
the assignment ‘`FS = "\.."`’ assigns the character string `".."`
to `FS` (the backslash is stripped). This creates a regexp meaning
“fields are separated by occurrences of any two characters.”
If instead you want fields to be separated by a literal period followed
by any single character, use ‘`FS = "\\.."`’.

The following list summarizes how fields are split, based on the value
of `FS` (‘`==`’ means “is equal to”):

`gawk was invoked with --csv`
:   Field splitting follows the rules given in [Working With Comma Separated Value Files](#Comma-Separated-Fields).
    The value of `FS` is ignored.

`FS == " "`
:   Fields are separated by runs of whitespace. Leading and trailing
    whitespace are ignored. This is the default.

`FS == any other single character`
:   Fields are separated by each occurrence of the character. Multiple
    successive occurrences delimit empty fields, as do leading and
    trailing occurrences.
    The character can even be a regexp metacharacter; it does not need
    to be escaped.

`FS == regexp`
:   Fields are separated by occurrences of characters that match regexp.
    Leading and trailing matches of regexp delimit empty fields.

`FS == ""`
:   Each individual character in the record becomes a separate field.
    (This is a common extension; it is not specified by the POSIX standard.)

| `FS` and `IGNORECASE` |
| --- |
| The `IGNORECASE` variable (see [Built-in Variables That Control `awk`](#User_002dmodified)) affects field splitting *only* when the value of `FS` is a regexp. It has no effect when `FS` is a single character, even if that character is a letter. Thus, in the following code:  ``` FS = "c" IGNORECASE = 1 $0 = "aCa" print $1 ```  The output is ‘`aCa`’. If you really want to split fields on an alphabetic character while ignoring case, use a regexp that will do it for you (e.g., ‘`FS = "[c]"`’). In this case, `IGNORECASE` will take effect. |

---

Next: [Defining Fields by Content](#Splitting-By-Content), Previous: [Specifying How Fields Are Separated](#Field-Separators), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.6 Reading Fixed-Width Data [¶](#Reading-Fixed_002dWidth-Data)

This section discusses an advanced
feature of `gawk`. If you are a novice `awk` user,
you might want to skip it on the first reading.

`gawk` provides a facility for dealing with fixed-width fields
with no distinctive field separator. We discuss this feature in
the following subsections.

* [Processing Fixed-Width Data](#Fixed-width-data)
* [Skipping Intervening Fields](#Skipping-intervening)
* [Capturing Optional Trailing Data](#Allowing-trailing-data)
* [Field Values With Fixed-Width Data](#Fields-with-fixed-data)

---

Next: [Skipping Intervening Fields](#Skipping-intervening), Up: [Reading Fixed-Width Data](#Constant-Size)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.6.1 Processing Fixed-Width Data [¶](#Processing-Fixed_002dWidth-Data)

An example of fixed-width data would be the input for old Fortran programs
where numbers are run together, or the output of programs that did not
anticipate the use of their output as input for other programs.

An example of the latter is a table where all the columns are lined up
by the use of a variable number of spaces and *empty fields are
just spaces*. Clearly, `awk`’s normal field splitting based
on `FS` does not work well in this case. Although a portable
`awk` program can use a series of `substr()` calls on
`$0` (see [String-Manipulation Functions](#String-Functions)), this is awkward and inefficient
for a large number of fields.

The splitting of an input record into fixed-width fields is specified by
assigning a string containing space-separated numbers to the built-in
variable `FIELDWIDTHS`. Each number specifies the width of the
field, *including* columns between fields. If you want to ignore
the columns between fields, you can specify the width as a separate
field that is subsequently ignored. It is a fatal error to supply a
field width that has a negative value.

The following data is the output of the Unix `w` utility. It is useful
to illustrate the use of `FIELDWIDTHS`:

```
 10:06pm  up 21 days, 14:04,  23 users
User     tty       login  idle   JCPU   PCPU  what
hzuo     ttyV0     8:58pm            9      5  vi p24.tex
hzang    ttyV3     6:37pm    50                -csh
eklye    ttyV5     9:53pm            7      1  em thes.tex
dportein ttyV6     8:17pm  1:47                -csh
gierd    ttyD3    10:00pm     1                elm
dave     ttyD4     9:47pm            4      4  w
brent    ttyp0    26Jun91  4:46  26:46   4:41  bash
dave     ttyq4    26Jun9115days     46     46  wnewmail
```

The following program takes this input, converts the idle time to
number of seconds, and prints out the first two fields and the calculated
idle time:

```
BEGIN  { FIELDWIDTHS = "9 6 10 6 7 7 35" }
NR > 2 {
    idle = $4
    sub(/^ +/, "", idle)   # strip leading spaces
    if (idle == "")
        idle = 0
    if (idle ~ /:/) {      # hh:mm
        split(idle, t, ":")
        idle = t[1] * 60 + t[2]
    }
    if (idle ~ /days/)
        idle *= 24 * 60 * 60

    print $1, $2, idle
}
```

> **NOTE:** The preceding program uses a number of `awk` features that
> haven’t been introduced yet.

Running the program on the data produces the following results:

```
hzuo      ttyV0  0
hzang     ttyV3  50
eklye     ttyV5  0
dportein  ttyV6  107
gierd     ttyD3  1
dave      ttyD4  0
brent     ttyp0  286
dave      ttyq4  1296000
```

Another (possibly more practical) example of fixed-width input data
is the input from a deck of balloting cards. In some parts of
the United States, voters mark their choices by punching holes in computer
cards. These cards are then processed to count the votes for any particular
candidate or on any particular issue. Because a voter may choose not to
vote on some issue, any column on the card may be empty. An `awk`
program for processing such data could use the `FIELDWIDTHS` feature
to simplify reading the data. (Of course, getting `gawk` to run on
a system with card readers is another story!)

---

Next: [Capturing Optional Trailing Data](#Allowing-trailing-data), Previous: [Processing Fixed-Width Data](#Fixed-width-data), Up: [Reading Fixed-Width Data](#Constant-Size)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.6.2 Skipping Intervening Fields [¶](#Skipping-Intervening-Fields)

Starting in version 4.2, each field width may optionally be
preceded by a colon-separated value specifying the number of characters
to skip before the field starts. Thus, the preceding program could be
rewritten to specify `FIELDWIDTHS` like so:

```
BEGIN  { FIELDWIDTHS = "8 1:5 4:7 6 1:6 1:6 2:33" }
```

This strips away some of the white space separating the fields. With such
a change, the program produces the following results:

```
hzang    ttyV3 50
eklye    ttyV5 0
dportein ttyV6 107
gierd    ttyD3 1
dave     ttyD4 0
brent    ttyp0 286
dave     ttyq4 1296000
```

---

Next: [Field Values With Fixed-Width Data](#Fields-with-fixed-data), Previous: [Skipping Intervening Fields](#Skipping-intervening), Up: [Reading Fixed-Width Data](#Constant-Size)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.6.3 Capturing Optional Trailing Data [¶](#Capturing-Optional-Trailing-Data)

There are times when fixed-width data may be followed by additional data
that has no fixed length. Such data may or may not be present, but if
it is, it should be possible to get at it from an `awk` program.

Starting with version 4.2, in order to provide a way to say “anything
else in the record after the defined fields,” `gawk`
allows you to add a final ‘`*`’ character to the value of
`FIELDWIDTHS`. There can only be one such character, and it must
be the final non-whitespace character in `FIELDWIDTHS`.
For example:

```
$ cat fw.awk                         Show the program
-| BEGIN { FIELDWIDTHS = "2 2 *" }
-| { print NF, $1, $2, $3 }
$ cat fw.in                          Show sample input
-| 1234abcdefghi
$ gawk -f fw.awk fw.in               Run the program
-| 3 12 34 abcdefghi
```

---

Previous: [Capturing Optional Trailing Data](#Allowing-trailing-data), Up: [Reading Fixed-Width Data](#Constant-Size)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.6.4 Field Values With Fixed-Width Data [¶](#Field-Values-With-Fixed_002dWidth-Data)

So far, so good. But what happens if there isn’t as much data as there
should be based on the contents of `FIELDWIDTHS`? Or, what happens
if there is more data than expected?

For many years, what happens in these cases was not well defined. Starting
with version 4.2, the rules are as follows:

Enough data for some fields
:   For example, if `FIELDWIDTHS` is set to `"2 3 4"` and the
    input record is ‘`aabbb`’. In this case, `NF` is set to two.

Not enough data for a field
:   For example, if `FIELDWIDTHS` is set to `"2 3 4"` and the
    input record is ‘`aab`’. In this case, `NF` is set to two and
    `$2` has the value `"b"`. The idea is that even though there
    aren’t as many characters as were expected, there are some, so the data
    should be made available to the program.

Too much data
:   For example, if `FIELDWIDTHS` is set to `"2 3 4"` and the
    input record is ‘`aabbbccccddd`’. In this case, `NF` is set to
    three and the extra characters (‘`ddd`’) are ignored. If you want
    `gawk` to capture the extra characters, supply a final ‘`*`’
    in the value of `FIELDWIDTHS`.

Too much data, but with ‘`*`’ supplied
:   For example, if `FIELDWIDTHS` is set to `"2 3 4 *"` and the
    input record is ‘`aabbbccccddd`’. In this case, `NF` is set to
    four, and `$4` has the value `"ddd"`.

---

Next: [Checking How `gawk` Is Splitting Records](#Testing-field-creation), Previous: [Reading Fixed-Width Data](#Constant-Size), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.7 Defining Fields by Content [¶](#Defining-Fields-by-Content)

This section discusses an advanced
feature of `gawk`. If you are a novice `awk` user,
you might want to skip it on the first reading.

Normally, when using `FS`, `gawk` defines the fields as the parts
of the record that occur in between each field separator. In other words,
`FS` defines what a field *is not*, instead of what a field
*is*. However, there are times when we really want to define the fields
by what they are, and not by what they are not.

The `FPAT` variable offers a solution for cases like this. The value
of `FPAT` should be a string that provides a regular expression. This
regular expression describes the contents of each field.

We can explore the strengths, and some limitations, of
`FPAT` using the case of comma-separated values (CSV)
data. This case is somewhat obsolete as `gawk` now has
built-in CSV parsing
(see [Working With Comma Separated Value Files](#Comma-Separated-Fields)).
Nonetheless, it remains useful as an example of what `FPAT`-based field
parsing can do. It is also useful for versions of
`gawk` prior to version 5.3.

Many spreadsheet programs, for example, can export their data into text
files, where each record is terminated with a newline, and fields are
separated by commas. If commas only separated the data, there wouldn’t
be an issue with using ‘`FS = ","`’ to split the data into fields. The
problem comes when one of the fields contains an *embedded* comma. In
such cases, most programs embed the field in double quotes.[27](#FOOT27)
So, we might have data like this:

```
Robbins,Arnold,,"1234 A Pretty Street, NE",MyTown,MyState,12345-6789,USA
```

In the case of CSV data as presented here, each field is either “anything that
is not a comma,” or “a double quote, anything that is not a double quote, and
a closing double quote.” We also need to bear in mind that some fields may be
empty. If written as a regular expression constant (see [Regular Expressions](#Regexp)),
we would have `/([^,]*)|("[^"]+")/`.
Writing this as a string requires us to escape the double quotes, leading to:

```
FPAT = "([^,]*)|(\"[^\"]+\")"
```

Putting this to use, here is a simple program to parse the data:

```
BEGIN {
    FPAT = "([^,]*)|(\"[^\"]+\")"
}
```

```

```

```
{
    print "NF =", NF
    for (i = 1; i <= NF; i++) {
        printf("$%d = <%s>\n", i, $i)
    }
}
```

When run, we get the following:

```
$ gawk -f simple-csv.awk addresses.csv
-| NF = 8
-| $1 = <Robbins>
-| $2 = <Arnold>
-| $3 = <>
-| $4 = <"1234 A Pretty Street, NE">
-| $5 = <MyTown>
-| $6 = <MyState>
-| $7 = <12345-6789>
-| $8 = <USA>
```

Note the empty data field in the value of `$3` and the embedded comma in
the value of `$4`, in which the data remains wrapped in its enclosing
double quotes.

The use of enclosing double quotes has a consequence for fields that contain
double quotes as part of the data itself. For those fields, a double quote
appearing inside a field must be escaped by preceding it with another double
quote, as shown in the third and fourth lines of the following example:

```
1,2,3
p,"q,r",s
p,"q""r",s
p,"q,""r",s
p,"",s
p,,s
```

Manuel Collado suggests that the simplest `FPAT` expression that
recognizes this kind of CSV field is `/([^,]*)|("([^"]|"")+")/`.

The following program uses this improved `FPAT` expression to split the
example CSV fields above, extracts the underlying data from any double-quoted
fields, and finally prints the data as tab-separated values.
The latter is accomplished by setting `OFS` to a TAB character.

```
BEGIN {
    FPAT = "([^,]*)|(\"([^\"]|\"\")+\")"
    OFS = "\t"    # Print tab-separated values
}
```

```

```

```
{
    for (i = 1; i <= NF; i++) {
        # Extract data from double-quoted fields
        if (substr($i, 1, 1) == "\"") {
            gsub(/^"|"$/, "", $i)    # Remove enclosing quotes
            gsub(/""/, "\"", $i)    # Convert "" to "
        }
    }
    $1 = $1    # force rebuild of the record
    print
}
```

When run, it produces:

```
$ gawk -f quoted-csv.awk sample.csv
-| 1       2       3
-| p       q,r     s
-| p       q"r     s
-| p       q,"r    s
-| p               s
-| p               s
```

Some programs export CSV data that contain *embedded newlines* between
the double quotes, and here we run into a limitation of `FPAT`: it
provides no way to deal with this. Hence, using `FPAT` to do your own CSV
parsing is an elegant approach for the majority of cases, but not all.

For a more general solution to working with CSV data, see
[Working With Comma Separated Value Files](#Comma-Separated-Fields). If your version of `gawk`
is prior to version 5.3, we recommend that you use Manuel Collado’s
[`CSVMODE` library for
`gawk`](https://mcollado.z15.es/gawk-extras/).

As with `FS`, the `IGNORECASE` variable (see [Built-in Variables That Control `awk`](#User_002dmodified))
affects field splitting with `FPAT`.

Assigning a value to `FPAT` overrides field splitting
with `FS` and with `FIELDWIDTHS`.

Finally, the `patsplit()` function makes the same functionality
available for splitting regular strings (see [String-Manipulation Functions](#String-Functions)).

* [`FS` Versus `FPAT`: A Subtle Difference](#FS-versus-FPAT)

---

Up: [Defining Fields by Content](#Splitting-By-Content)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.7.1 `FS` Versus `FPAT`: A Subtle Difference [¶](#FS-Versus-FPAT_003a-A-Subtle-Difference)

As we discussed earlier, `FS` describes the data between fields (“what fields are not”)
and `FPAT` describes the fields themselves (“what fields are”).
This leads to a subtle difference in how fields are found when using regexps as the value
for `FS` or `FPAT`.

In order to distinguish one field from another, there must be a non-empty separator between
each field. This makes intuitive sense—otherwise one could not distinguish fields from
separators.

Thus, regular expression matching as done when splitting fields with `FS` is not
allowed to match the null string; it must always match at least one character, in order
to be able to proceed through the entire record.

On the other hand, regular expression matching with `FPAT` can match the null
string, and the non-matching intervening characters function as the separators.

This same difference is reflected in how matching is done with the `split()`
and `patsplit()` functions (see [String-Manipulation Functions](#String-Functions)).

---

Next: [Multiple-Line Records](#Multiple-Line), Previous: [Defining Fields by Content](#Splitting-By-Content), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.8 Checking How `gawk` Is Splitting Records [¶](#Checking-How-gawk-Is-Splitting-Records)

As we’ve seen, `gawk` provides three independent methods to split
input records into fields. The mechanism used is based on which of the
three variables—`FS`, `FIELDWIDTHS`, or `FPAT`—was
last assigned to. In addition, an API input parser may choose to override
the record parsing mechanism; please refer to [Customized Input Parsers](#Input-Parsers) for
further information about this feature.

To restore normal field splitting after using `FIELDWIDTHS`
and/or `FPAT`, simply assign a value to `FS`.
You can use ‘`FS = FS`’ to do this,
without having to know the current value of `FS`.

In order to tell which kind of field splitting is in effect,
use `PROCINFO["FS"]` (see [Built-in Variables That Convey Information](#Auto_002dset)).
The value is `"FS"` if regular field splitting is being used,
`"FIELDWIDTHS"` if fixed-width field splitting is being used,
or `"FPAT"` if content-based field splitting is being used:

```
if ("CSV" in PROCINFO)
    CSV-based field splitting ...
else if (PROCINFO["FS"] == "FS")
    regular field splitting ...
else if (PROCINFO["FS"] == "FIELDWIDTHS")
    fixed-width field splitting ...
else if (PROCINFO["FS"] == "FPAT")
    content-based field splitting ...
else
    API input parser field splitting ... (advanced feature)
```

This information is useful when writing a function that needs to
temporarily change `FS`, `FIELDWIDTHS`, or `FPAT`, read some records,
and then restore the original settings (see [Reading the User Database](#Passwd-Functions) for an
example of such a function).

---

Next: [Explicit Input with `getline`](#Getline), Previous: [Checking How `gawk` Is Splitting Records](#Testing-field-creation), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.9 Multiple-Line Records [¶](#Multiple_002dLine-Records)

In some databases, a single line cannot conveniently hold all the
information in one entry. In such cases, you can use multiline
records. The first step in doing this is to choose your data format.

One technique is to use an unusual character or string to separate
records. For example, you could use the formfeed character (written
‘`\f`’ in `awk`, as in C) to separate them, making each record
a page of the file. To do this, just set the variable `RS` to
`"\f"` (a string containing the formfeed character). Any
other character could equally well be used, as long as it won’t be part
of the data in a record.

Another technique is to have blank lines separate records. By a special
dispensation, an empty string as the value of `RS` indicates that
records are separated by one or more blank lines. When `RS` is set
to the empty string, each record always ends at the first blank line
encountered. The next record doesn’t start until the first nonblank
line that follows. No matter how many blank lines appear in a row, they
all act as one record separator.
(Blank lines must be completely empty; lines that contain only
whitespace do not count.)

You can achieve the same effect as ‘`RS = ""`’ by assigning the
string `"\n\n+"` to `RS`. This regexp matches the newline
at the end of the record and one or more blank lines after the record.
In addition, a regular expression always matches the longest possible
sequence when there is a choice
(see [How Much Text Matches?](#Leftmost-Longest)).
So, the next record doesn’t start until
the first nonblank line that follows—no matter how many blank lines
appear in a row, they are considered one record separator.

However, there is an important difference between ‘`RS = ""`’ and
‘`RS = "\n\n+"`’. In the first case, leading newlines in the input
data file are ignored, and if a file ends without extra blank lines
after the last record, the final newline is removed from the record.
In the second case, this special processing is not done.
(d.c.)

Now that the input is separated into records, the second step is to
separate the fields in the records. One way to do this is to divide each
of the lines into fields in the normal manner. This happens by default
as the result of a special feature. When `RS` is set to the empty
string *and* `FS` is set to a single character,
the newline character *always* acts as a field separator.
This is in addition to whatever field separations result from
`FS`.

> **NOTE:** When `FS` is the null string (`""`)
> or a regexp, this special feature of `RS` does not apply.
> It does apply to the default field separator of a single space:
> ‘`FS = " "`’.
>
> Note that language in the POSIX specification implies that
> this special feature should apply when `FS` is a regexp.
> However, Unix `awk` has never behaved that way, nor has
> `gawk`. This is essentially a bug in POSIX.

The original motivation for this special exception was probably to provide
useful behavior in the default case (i.e., `FS` is equal
to `" "`). This feature can be a problem if you really don’t
want the newline character to separate fields, because there is no way to
prevent it. However, you can work around this by using the `split()`
function to break up the record manually
(see [String-Manipulation Functions](#String-Functions)).
If you have a single-character field separator, you can work around
the special feature in a different way, by making `FS` into a
regexp for that single character. For example, if the field
separator is a percent character, instead of
‘`FS = "%"`’, use ‘`FS = "[%]"`’.

Another way to separate fields is to
put each field on a separate line: to do this, just set the
variable `FS` to the string `"\n"`.
(This single-character separator matches a single newline.)
A practical example of a data file organized this way might be a mailing
list, where blank lines separate the entries. Consider a mailing
list in a file named `addresses`, which looks like this:

```
Jane Doe
123 Main Street
Anywhere, SE 12345-6789

John Smith
456 Tree-lined Avenue
Smallville, MW 98765-4321
...
```

A simple program to process this file is as follows:

```
# addrs.awk --- simple mailing list program

# Records are separated by blank lines.
# Each line is one field.
BEGIN { RS = "" ; FS = "\n" }

{
      print "Name is:", $1
      print "Address is:", $2
      print "City and State are:", $3
      print ""
}
```

Running the program produces the following output:

```
$ awk -f addrs.awk addresses
-| Name is: Jane Doe
-| Address is: 123 Main Street
-| City and State are: Anywhere, SE 12345-6789
-|
-| Name is: John Smith
-| Address is: 456 Tree-lined Avenue
-| City and State are: Smallville, MW 98765-4321
-|
...
```

See [Printing Mailing Labels](#Labels-Program) for a more realistic program dealing with
address lists. The following list summarizes how records are split,
based on the value of
`RS`:

`RS == "\n"`
:   Records are separated by the newline character (‘`\n`’). In effect,
    every line in the data file is a separate record, including blank lines.
    This is the default.

`RS == any single character`
:   Records are separated by each occurrence of the character. Multiple
    successive occurrences delimit empty records.

`RS == ""`
:   Records are separated by runs of blank lines.
    When `FS` is a single character, then
    the newline character
    always serves as a field separator, in addition to whatever value
    `FS` may have. Leading and trailing newlines in a file are ignored.

`RS == regexp`
:   Records are separated by occurrences of characters that match regexp.
    Leading and trailing matches of regexp delimit empty records.
    (This is a `gawk` extension; it is not specified by the
    POSIX standard.)

If not in compatibility mode (see [Command-Line Options](#Options)), `gawk` sets
`RT` to the input text that matched the value specified by `RS`.
But if the input file ended without any text that matches `RS`,
then `gawk` sets `RT` to the null string.

---

Next: [Reading Input with a Timeout](#Read-Timeout), Previous: [Multiple-Line Records](#Multiple-Line), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.10 Explicit Input with `getline` [¶](#Explicit-Input-with-getline)

So far we have been getting our input data from `awk`’s main
input stream—either the standard input (usually your keyboard, sometimes
the output from another program) or the
files specified on the command line. The `awk` language has a
special built-in function called `getline` that
can be used to read input under your explicit control.

The `getline` function is used in several different ways and should
*not* be used by beginners.
The examples that follow the explanation of the `getline` function
include material that has not been covered yet. Therefore, come back
and study the `getline` function *after* you have reviewed the
rest of
this Web page
and have a good knowledge of how `awk` works.

The `getline` function returns 1 if it finds a record and 0 if
it encounters the end of the file. If there is some error in getting
a record, such as a file that cannot be opened, then `getline`
returns −1. In this case, `gawk` sets the variable
`ERRNO` to a string describing the error that occurred.

If `ERRNO` indicates that the I/O operation may be
retried, and `PROCINFO["input", "RETRY"]` is set,
then `getline` returns −2
instead of −1, and further calls to `getline`
may be attempted. See [Retrying Reads After Certain Input Errors](#Retrying-Input) for further information about
this feature.

In the following examples, command stands for a string value that
represents a shell command.

> **NOTE:** When `--sandbox` is specified (see [Command-Line Options](#Options)),
> reading lines from files, pipes, and coprocesses is disabled.

* [Using `getline` with No Arguments](#Plain-Getline)
* [Using `getline` into a Variable](#Getline_002fVariable)
* [Using `getline` from a File](#Getline_002fFile)
* [Using `getline` into a Variable from a File](#Getline_002fVariable_002fFile)
* [Using `getline` from a Pipe](#Getline_002fPipe)
* [Using `getline` into a Variable from a Pipe](#Getline_002fVariable_002fPipe)
* [Using `getline` from a Coprocess](#Getline_002fCoprocess)
* [Using `getline` into a Variable from a Coprocess](#Getline_002fVariable_002fCoprocess)
* [Points to Remember About `getline`](#Getline-Notes)
* [Summary of `getline` Variants](#Getline-Summary)

---

Next: [Using `getline` into a Variable](#Getline_002fVariable), Up: [Explicit Input with `getline`](#Getline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.10.1 Using `getline` with No Arguments [¶](#Using-getline-with-No-Arguments)

The `getline` function can be used without arguments to read input
from the current input file. All it does in this case is read the next
input record and split it up into fields. This is useful if you’ve
finished processing the current record, but want to do some special
processing on the next record *right now*. For example:

```
# Remove text between /* and */, inclusive
{
    while ((start = index($0, "/*")) != 0) {
        out = substr($0, 1, start - 1)  # leading part of the string
        rest = substr($0, start + 2)    # ... */ ...
        while ((end = index(rest, "*/")) == 0) {  # is */ in trailing part?
            # get more text
            if (getline <= 0) {
                print("unexpected EOF or error:", ERRNO) > "/dev/stderr"
                exit
            }
            # build up the line using string concatenation
            rest = rest $0
        }
        rest = substr(rest, end + 2)  # remove comment
        # build up the output line using string concatenation
        $0 = out rest
    }
    print $0
}
```

This `awk` program deletes C-style comments (‘`/* …
*/`’) from the input.
It uses a number of features we haven’t covered yet, including
string concatenation
(see [String Concatenation](#Concatenation))
and the `index()` and `substr()` built-in
functions
(see [String-Manipulation Functions](#String-Functions)).
By replacing the ‘`print $0`’ with other
statements, you could perform more complicated processing on the
decommented input, such as searching for matches of a regular
expression.

Here is some sample input:

```
mon/*comment*/key
rab/*commen
t*/bit
horse /*comment*/more text
part 1 /*comment*/part 2 /*comment*/part 3
no comment
```

When run, the output is:

```
$ awk -f strip_comments.awk example_text
-| monkey
-| rabbit
-| horse more text
-| part 1 part 2 part 3
-| no comment
```

This form of the `getline` function sets `NF`,
`NR`, `FNR`, `RT`, and the value of `$0`.

> **NOTE:** The new value of `$0` is used to test
> the patterns of any subsequent rules. The original value
> of `$0` that triggered the rule that executed `getline`
> is lost.
> By contrast, the `next` statement reads a new record
> but immediately begins processing it normally, starting with the first
> rule in the program. See [The `next` Statement](#Next-Statement).

---

Next: [Using `getline` from a File](#Getline_002fFile), Previous: [Using `getline` with No Arguments](#Plain-Getline), Up: [Explicit Input with `getline`](#Getline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.10.2 Using `getline` into a Variable [¶](#Using-getline-into-a-Variable)

You can use ‘`getline var`’ to read the next record from
`awk`’s input into the variable var. No other processing is
done.
For example, suppose the next line is a comment or a special string,
and you want to read it without triggering
any rules. This form of `getline` allows you to read that line
and store it in a variable so that the main
read-a-line-and-check-each-rule loop of `awk` never sees it.
The following example swaps every two lines of input:

```
{
     if ((getline tmp) > 0) {
          print tmp
          print $0
     } else
          print $0
}
```

It takes the following list:

```
wan
tew
free
phore
```

and produces these results:

```
tew
wan
phore
free
```

The `getline` function used in this way sets only the variables
`NR`, `FNR`, and `RT` (and, of course, var).
The record is not
split into fields, so the values of the fields (including `$0`) and
the value of `NF` do not change.

---

Next: [Using `getline` into a Variable from a File](#Getline_002fVariable_002fFile), Previous: [Using `getline` into a Variable](#Getline_002fVariable), Up: [Explicit Input with `getline`](#Getline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.10.3 Using `getline` from a File [¶](#Using-getline-from-a-File)

Use ‘`getline < file`’ to read the next record from file.
Here, file is a string-valued expression that
specifies the file name. ‘`< file`’ is called a *redirection*
because it directs input to come from a different place.
For example, the following
program reads its input record from the file `secondary.input` when it
encounters a first field with a value equal to 10 in the current input
file:

```
{
    if ($1 == 10) {
         getline < "secondary.input"
         print
    } else
         print
}
```

Because the main input stream is not used, the values of `NR` and
`FNR` are not changed. However, the record it reads is split into fields in
the normal manner, so the values of `$0` and the other fields are
changed, resulting in a new value of `NF`.
`RT` is also set.

According to POSIX, ‘`getline < expression`’ is ambiguous if
expression contains unparenthesized operators other than
‘`$`’; for example, ‘`getline < dir "/" file`’ is ambiguous
because the concatenation operator (not discussed yet; see [String Concatenation](#Concatenation))
is not parenthesized. You should write it as ‘`getline < (dir "/" file)`’ if
you want your program to be portable to all `awk` implementations.

---

Next: [Using `getline` from a Pipe](#Getline_002fPipe), Previous: [Using `getline` from a File](#Getline_002fFile), Up: [Explicit Input with `getline`](#Getline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.10.4 Using `getline` into a Variable from a File [¶](#Using-getline-into-a-Variable-from-a-File)

Use ‘`getline var < file`’ to read input
from the file
file, and put it in the variable var. As earlier, file
is a string-valued expression that specifies the file from which to read.

In this version of `getline`, none of the predefined variables are
changed and the record is not split into fields. The only variable
changed is var.[28](#FOOT28)
For example, the following program copies all the input files to the
output, except for records that say ‘`@include filename`’.
Such a record is replaced by the contents of the file
filename:

```
{
     if (NF == 2 && $1 == "@include") {
          while ((getline line < $2) > 0)
               print line
          close($2)
     } else
          print
}
```

Note here how the name of the extra input file is not built into
the program; it is taken directly from the data, specifically from the second field on
the `@include` line.

The `close()` function is called to ensure that if two identical
`@include` lines appear in the input, the entire specified file is
included twice.
See [Closing Input and Output Redirections](#Close-Files-And-Pipes).

One deficiency of this program is that it does not process nested
`@include` directives
(i.e., `@include` directives in included files)
the way a true macro preprocessor would.
See [An Easy Way to Use Library Functions](#Igawk-Program) for a program
that does handle nested `@include` directives.

---

Next: [Using `getline` into a Variable from a Pipe](#Getline_002fVariable_002fPipe), Previous: [Using `getline` into a Variable from a File](#Getline_002fVariable_002fFile), Up: [Explicit Input with `getline`](#Getline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.10.5 Using `getline` from a Pipe [¶](#Using-getline-from-a-Pipe)

> *Omniscience has much to recommend it.
> Failing that, attention to details would be useful.*

— *Brian Kernighan*

The output of a command can also be piped into `getline`, using
‘`command | getline`’. In
this case, the string command is run as a shell command and its output
is piped into `awk` to be used as input. This form of `getline`
reads one record at a time from the pipe.
For example, the following program copies its input to its output, except for
lines that begin with ‘`@execute`’, which are replaced by the output
produced by running the rest of the line as a shell command:

```
{
     if ($1 == "@execute") {
          tmp = substr($0, 10)        # Remove "@execute"
          while ((tmp | getline) > 0)
               print
          close(tmp)
     } else
          print
}
```

The `close()` function is called to ensure that if two identical
‘`@execute`’ lines appear in the input, the command is run for
each one.
See [Closing Input and Output Redirections](#Close-Files-And-Pipes).
Given the input:

```
foo
bar
baz
@execute who
bletch
```

the program might produce:

```
foo
bar
baz
arnold     ttyv0   Jul 13 14:22
miriam     ttyp0   Jul 13 14:23     (murphy:0)
bill       ttyp1   Jul 13 14:23     (murphy:0)
bletch
```

Notice that this program ran the command `who` and printed the result.
(If you try this program yourself, you will of course get different results,
depending upon who is logged in on your system.)

This variation of `getline` splits the record into fields, sets the
value of `NF`, and recomputes the value of `$0`. The values of
`NR` and `FNR` are not changed.
`RT` is set.

According to POSIX, ‘`expression | getline`’ is ambiguous if
expression contains unparenthesized operators other than
‘`$`’—for example, ‘`"echo " "date" | getline`’ is ambiguous
because the concatenation operator is not parenthesized. You should
write it as ‘`("echo " "date") | getline`’ if you want your program
to be portable to all `awk` implementations.

> **NOTE:** Unfortunately, `gawk` has not been consistent in its treatment
> of a construct like ‘`"echo " "date" | getline`’.
> Most versions, including the current version, treat it as
> ‘`("echo " "date") | getline`’.
> (This is also how BWK `awk` behaves.)
> Some versions instead treat it as
> ‘`"echo " ("date" | getline)`’.
> (This is how `mawk` behaves.)
> In short, *always* use explicit parentheses, and then you won’t
> have to worry.

---

Next: [Using `getline` from a Coprocess](#Getline_002fCoprocess), Previous: [Using `getline` from a Pipe](#Getline_002fPipe), Up: [Explicit Input with `getline`](#Getline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.10.6 Using `getline` into a Variable from a Pipe [¶](#Using-getline-into-a-Variable-from-a-Pipe)

When you use ‘`command | getline var`’, the
output of command is sent through a pipe to
`getline` and into the variable var. For example, the
following program reads the current date and time into the variable
`current_time`, using the `date` utility, and then
prints it:

```
BEGIN {
     "date" | getline current_time
     close("date")
     print "Report printed on " current_time
}
```

In this version of `getline`, none of the predefined variables are
changed and the record is not split into fields. However, `RT` is set.

---

Next: [Using `getline` into a Variable from a Coprocess](#Getline_002fVariable_002fCoprocess), Previous: [Using `getline` into a Variable from a Pipe](#Getline_002fVariable_002fPipe), Up: [Explicit Input with `getline`](#Getline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.10.7 Using `getline` from a Coprocess [¶](#Using-getline-from-a-Coprocess)

Reading input into `getline` from a pipe is a one-way operation.
The command that is started with ‘`command | getline`’ only
sends data *to* your `awk` program.

On occasion, you might want to send data to another program
for processing and then read the results back.
`gawk` allows you to start a *coprocess*, with which two-way
communications are possible. This is done with the ‘`|&`’
operator.
Typically, you write data to the coprocess first and then
read the results back, as shown in the following:

```
print "some query" |& "db_server"
"db_server" |& getline
```

which sends a query to `db_server` and then reads the results.

The values of `NR` and
`FNR` are not changed,
because the main input stream is not used.
However, the record is split into fields in
the normal manner, thus changing the values of `$0`, of the other fields,
and of `NF` and `RT`.

Coprocesses are an advanced feature. They are discussed here only because
this is the section on `getline`.
See [Two-Way Communications with Another Process](#Two_002dway-I_002fO),
where coprocesses are discussed in more detail.

---

Next: [Points to Remember About `getline`](#Getline-Notes), Previous: [Using `getline` from a Coprocess](#Getline_002fCoprocess), Up: [Explicit Input with `getline`](#Getline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.10.8 Using `getline` into a Variable from a Coprocess [¶](#Using-getline-into-a-Variable-from-a-Coprocess)

When you use ‘`command |& getline var`’, the output from
the coprocess command is sent through a two-way pipe to `getline`
and into the variable var.

In this version of `getline`, none of the predefined variables are
changed and the record is not split into fields. The only variable
changed is var.
However, `RT` is set.

---

Next: [Summary of `getline` Variants](#Getline-Summary), Previous: [Using `getline` into a Variable from a Coprocess](#Getline_002fVariable_002fCoprocess), Up: [Explicit Input with `getline`](#Getline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.10.9 Points to Remember About `getline` [¶](#Points-to-Remember-About-getline)

Here are some miscellaneous points about `getline` that
you should bear in mind:

* When `getline` changes the value of `$0` and `NF`,
  `awk` does *not* automatically jump to the start of the
  program and start testing the new record against every pattern.
  However, the new record is tested against any subsequent rules.
* Some very old `awk` implementations limit the number of pipelines that an `awk`
  program may have open to just one. In `gawk`, there is no such limit.
  You can open as many pipelines (and coprocesses) as the underlying operating
  system permits.
* An interesting side effect occurs if you use `getline` without a
  redirection inside a `BEGIN` rule. Because an unredirected `getline`
  reads from the command-line data files, the first `getline` function
  causes `awk` to set the value of `FILENAME`. Normally,
  `FILENAME` does not have a value inside `BEGIN` rules, because you
  have not yet started to process the command-line data files.
  (d.c.)
  (See [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND);
  also see [Built-in Variables That Convey Information](#Auto_002dset).)
* Using `FILENAME` with `getline`
  (‘`getline < FILENAME`’)
  is likely to be a source of
  confusion. `awk` opens a separate input stream from the
  current input file. However, by not using a variable, `$0`
  and `NF` are still updated. If you’re doing this, it’s
  probably by accident, and you should reconsider what it is you’re
  trying to accomplish.
* [Summary of `getline` Variants](#Getline-Summary),
  presents a table summarizing the
  `getline` variants and which variables they can affect.
  It is worth noting that those variants that do not use redirection
  can cause `FILENAME` to be updated if they cause
  `awk` to start reading a new input file.
* `getline` is not a statement (unlike `print`), it’s an
  expression. It has a result value, and can be used as part as a
  larger expression, in control statements, and so on.
  Here are examples of the “read until EOF/error” idiom:

  ```
  while ("sort FILE" | getline line > 0)
      print line
  while (getline line < "file.txt" > 0)
      print line
  ```

  If you need to test the error code for being less than zero,
  you need to enclose `getline` in parentheses, to avoid
  it being interpreted as input redirection:

  ```
  if ((getline VAR) < 0)
      print "Read error";
  ```

  It is, in fact, best to parenthesize calls to `getline`
  in all control expressions, as some versions of `awk`
  require this. Thus, the previous examples are best written
  this way:

  ```
  while (("sort FILE" | getline line) > 0)
      print line
  while ((getline line < "file.txt") > 0)
      print line
  ```
* If the variable being assigned is an expression with side effects,
  different versions of `awk` behave differently upon encountering
  end-of-file. Some versions don’t evaluate the expression; many versions
  (including `gawk`) do. Here is an example, courtesy of Duncan Moore:

  ```
  BEGIN {
      system("echo 1 > f")
      while ((getline a[++c] < "f") > 0) { }
      print c
  }
  ```

  Here, the side effect is the ‘`++c`’. Is `c` incremented if
  end-of-file is encountered before the element in `a` is assigned?

  Despite the lack of parentheses when calling `getline`,
  `gawk` evaluates
  the expression ‘`a[++c]`’ before attempting to read from `f`.
  However, some versions of `awk` only evaluate the expression once they
  know that there is a string value to be assigned.

---

Previous: [Points to Remember About `getline`](#Getline-Notes), Up: [Explicit Input with `getline`](#Getline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.10.10 Summary of `getline` Variants [¶](#Summary-of-getline-Variants)

[Table 4.2](#table_002dgetline_002dvariants)
summarizes the eight variants of `getline`,
listing which predefined variables are set by each one,
and whether the variant is standard or a `gawk` extension.
Note: for each variant, `gawk` sets the `RT` predefined variable.

| Variant | Effect | `awk` / `gawk` |
| --- | --- | --- |
| `getline` | Sets `$0`, `NF`, `FNR`, `NR`, and `RT` | `awk` |
| `getline` var | Sets var, `FNR`, `NR`, and `RT` | `awk` |
| `getline <` file | Sets `$0`, `NF`, and `RT` | `awk` |
| `getline var < file` | Sets var and `RT` | `awk` |
| command `| getline` | Sets `$0`, `NF`, and `RT` | `awk` |
| command `| getline` var | Sets var and `RT` | `awk` |
| command `|& getline` | Sets `$0`, `NF`, and `RT` | `gawk` |
| command `|& getline` var | Sets var and `RT` | `gawk` |

**Table 4.2:** `getline` variants and what they set

---

Next: [Retrying Reads After Certain Input Errors](#Retrying-Input), Previous: [Explicit Input with `getline`](#Getline), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.11 Reading Input with a Timeout [¶](#Reading-Input-with-a-Timeout)

This section describes a feature that is specific to `gawk`.

You may specify a timeout in milliseconds for reading input from the keyboard,
a pipe, or two-way communication, including TCP/IP sockets. This can be done
on a per-input, per-command, or per-connection basis, by setting a special
element in the `PROCINFO` array (see [Built-in Variables That Convey Information](#Auto_002dset)):

```
PROCINFO["input_name", "READ_TIMEOUT"] = timeout in milliseconds
```

When set, this causes `gawk` to time out and return failure
if no data is available to read within the specified timeout period.
For example, a TCP client can decide to give up on receiving
any response from the server after a certain amount of time:

```
Service = "/inet/tcp/0/localhost/daytime"
PROCINFO[Service, "READ_TIMEOUT"] = 100
if ((Service |& getline) > 0)
    print $0
else if (ERRNO != "")
    print ERRNO
```

Here is how to read interactively from the user[29](#FOOT29) without waiting
for more than five seconds:

```
PROCINFO["/dev/stdin", "READ_TIMEOUT"] = 5000
while ((getline < "/dev/stdin") > 0)
    print $0
```

`gawk` terminates the read operation if input does not
arrive after waiting for the timeout period, returns failure,
and sets `ERRNO` to an appropriate string value.
A negative or zero value for the timeout is the same as specifying
no timeout at all.

A timeout can also be set for reading from the keyboard in the implicit
loop that reads input records and matches them against patterns,
like so:

```
$ gawk 'BEGIN { PROCINFO["-", "READ_TIMEOUT"] = 5000 }
> { print "You entered: " $0 }'
gawk
-| You entered: gawk
```

In this case, failure to respond within five seconds results in the following
error message:

```
error→ gawk: cmd. line:2: (FILENAME=- FNR=1) fatal: error reading input file `-': Connection timed out
```

The timeout can be set or changed at any time, and will take effect on the
next attempt to read from the input device. In the following example,
we start with a timeout value of one second, and progressively
reduce it by one-tenth of a second until we wait indefinitely
for the input to arrive:

```
PROCINFO[Service, "READ_TIMEOUT"] = 1000
while ((Service |& getline) > 0) {
    print $0
    PROCINFO[Service, "READ_TIMEOUT"] -= 100
}
```

> **NOTE:** You should not assume that the read operation will block
> exactly after the tenth record has been printed. It is possible that
> `gawk` will read and buffer more than one record’s
> worth of data the first time. Because of this, changing the value
> of timeout like in the preceding example is not very useful.

If the `PROCINFO` element is not present and the
`GAWK_READ_TIMEOUT` environment variable exists,
`gawk` uses its value to initialize the timeout value.
The exclusive use of the environment variable to specify timeout
has the disadvantage of not being able to control it
on a per-command or per-connection basis.

`gawk` considers a timeout event to be an error even though
the attempt to read from the underlying device may
succeed in a later attempt. This is a limitation, and it also
means that you cannot use this to multiplex input from
two or more sources. See [Retrying Reads After Certain Input Errors](#Retrying-Input) for a way to enable
later I/O attempts to succeed.

Assigning a timeout value prevents read operations from being
blocked indefinitely. But bear in mind that there are other ways
`gawk` can stall waiting for an input device to be ready.
A network client can sometimes take a long time to establish
a connection before it can start reading any data,
or the attempt to open a FIFO special file for reading can be blocked
indefinitely until some other process opens it for writing.

---

Next: [Directories on the Command Line](#Command_002dline-directories), Previous: [Reading Input with a Timeout](#Read-Timeout), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.12 Retrying Reads After Certain Input Errors [¶](#Retrying-Reads-After-Certain-Input-Errors)

This section describes a feature that is specific to `gawk`.

When `gawk` encounters an error while reading input, by
default `getline` returns −1, and subsequent attempts to
read from that file result in an end-of-file indication. However, you
may optionally instruct `gawk` to allow I/O to be retried when
certain errors are encountered by setting a special element in
the `PROCINFO` array (see [Built-in Variables That Convey Information](#Auto_002dset)):

```
PROCINFO["input_name", "RETRY"] = 1
```

When this element exists, `gawk` checks the value of the system
(C language)
`errno` variable when an I/O error occurs. If `errno` indicates
a subsequent I/O attempt may succeed, `getline` instead returns
−2 and
further calls to `getline` may succeed. This applies to the `errno`
values `EAGAIN`, `EWOULDBLOCK`, `EINTR`, or `ETIMEDOUT`.

This feature is useful in conjunction with
`PROCINFO["input_name", "READ_TIMEOUT"]` or situations where a file
descriptor has been configured to behave in a non-blocking fashion.

---

Next: [Summary](#Input-Summary), Previous: [Retrying Reads After Certain Input Errors](#Retrying-Input), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.13 Directories on the Command Line [¶](#Directories-on-the-Command-Line)

According to the POSIX standard, files named on the `awk`
command line must be text files; it is a fatal error if they are not.
Most versions of `awk` treat a directory on the command line as
a fatal error.

By default, `gawk` produces a warning for a directory on the
command line, but otherwise ignores it. This makes it easier to use
shell wildcards with your `awk` program:

```
$ gawk -f whizprog.awk *        Directories could kill this program
```

If either of the `--posix`
or `--traditional` options is given, then `gawk` reverts
to treating a directory on the command line as a fatal error.

See [Reading Directories](#Extension-Sample-Readdir) for a way to treat directories
as usable data from an `awk` program.

---

Next: [Exercises](#Input-Exercises), Previous: [Directories on the Command Line](#Command_002dline-directories), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.14 Summary [¶](#Summary-3)

* Input is split into records based on the value of `RS`.
  The possibilities are as follows:

  | Value of `RS` | Records are split on … | `awk` / `gawk` |
  | --- | --- | --- |
  | Any single character | That character | `awk` |
  | The empty string (`""`) | Runs of two or more newlines | `awk` |
  | A regexp | Text that matches the regexp | `gawk` |
* `FNR` indicates how many records have been read from the current input file;
  `NR` indicates how many records have been read in total.
* `gawk` sets `RT` to the text matched by `RS`.
* After splitting the input into records, `awk` further splits
  the records into individual fields, named `$1`, `$2`, and so
  on. `$0` is the whole record, and `NF` indicates how many
  fields there are. The default way to split fields is between whitespace
  characters.
* Fields may be referenced using a variable, as in `$NF`. Fields
  may also be assigned values, which causes the value of `$0` to be
  recomputed when it is later referenced. Assigning to a field with a number
  greater than `NF` creates the field and rebuilds the record, using
  `OFS` to separate the fields. Incrementing `NF` does the same
  thing. Decrementing `NF` throws away fields and rebuilds the record.
* Field splitting is more complicated than record splitting:

  | Field separator value | Fields are split … | `awk` / `gawk` |
  | --- | --- | --- |
  | `FS == " "` | On runs of whitespace | `awk` |
  | `FS == any single character` | On that character | `awk` |
  | `FS == regexp` | On text matching the regexp | `awk` |
  | `FS == ""` | Such that each individual character is a separate field | `gawk` |
  | `FIELDWIDTHS == list of columns` | Based on character position | `gawk` |
  | `FPAT == regexp` | On the text surrounding text matching the regexp | `gawk` |
* Using ‘`FS = "\n"`’ causes the entire record to be a single field
  (assuming that newlines separate records).
* `FS` may be set from the command line using the `-F` option.
  This can also be done using command-line variable assignment.
* Use `PROCINFO["FS"]` to see how fields are being split.
* Use `getline` in its various forms to read additional records
  from the default input stream, from a file, or from a pipe or coprocess.
* Use `PROCINFO[file, "READ_TIMEOUT"]` to cause reads to time out
  for file.
* Directories on the command line are fatal for standard `awk`;
  `gawk` ignores them if not in POSIX mode.

---

Previous: [Summary](#Input-Summary), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.15 Exercises [¶](#Exercises)

1. Using the `FIELDWIDTHS` variable (see [Reading Fixed-Width Data](#Constant-Size)),
   write a program to read election data, where each record represents
   one voter’s votes. Come up with a way to define which columns are
   associated with each ballot item, and print the total votes,
   including abstentions, for each item.

---

Next: [Expressions](#Expressions), Previous: [Reading Input Files](#Reading-Files), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 5 Printing Output [¶](#Printing-Output)

One of the most common programming actions is to *print*, or output,
some or all of the input. Use the `print` statement
for simple output, and the `printf` statement
for fancier formatting.
The `print` statement is not limited when
computing *which* values to print. However, with two exceptions,
you cannot specify *how* to print them—how many
columns, whether to use exponential notation or not, and so on.
(For the exceptions, see [Output Separators](#Output-Separators) and
[Controlling Numeric Output with `print`](#OFMT).)
For printing with specifications, you need the `printf` statement
(see [Using `printf` Statements for Fancier Printing](#Printf)).

Besides basic and formatted printing, this chapter
also covers I/O redirections to files and pipes, introduces
the special file names that `gawk` processes internally,
and discusses the `close()` built-in function.

* [The `print` Statement](#Print)
* [`print` Statement Examples](#Print-Examples)
* [Output Separators](#Output-Separators)
* [Controlling Numeric Output with `print`](#OFMT)
* [Using `printf` Statements for Fancier Printing](#Printf)
* [Redirecting Output of `print` and `printf`](#Redirection)
* [Special Files for Standard Preopened Data Streams](#Special-FD)
* [Special File names in `gawk`](#Special-Files)
* [Closing Input and Output Redirections](#Close-Files-And-Pipes)
* [Speeding Up Pipe Output](#Noflush)
* [Enabling Nonfatal Output](#Nonfatal)
* [Summary](#Output-Summary)
* [Exercises](#Output-Exercises)

---

Next: [`print` Statement Examples](#Print-Examples), Up: [Printing Output](#Printing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.1 The `print` Statement [¶](#The-print-Statement)

Use the `print` statement to produce output with simple, standardized
formatting. You specify only the strings or numbers to print, in a
list separated by commas. They are output, separated by single spaces,
followed by a newline. The statement looks like this:

```
print item1, item2, ...
```

The entire list of items may be optionally enclosed in parentheses. The
parentheses are necessary if any of the item expressions uses the ‘`>`’
relational operator; otherwise it could be confused with an output redirection
(see [Redirecting Output of `print` and `printf`](#Redirection)).

The items to print can be constant strings or numbers, fields of the
current record (such as `$1`), variables, or any `awk`
expression. Numeric values are converted to strings and then printed.

The simple statement ‘`print`’ with no items is equivalent to
‘`print $0`’: it prints the entire current record. To print a blank
line, use ‘`print ""`’.
To print a fixed piece of text, use a string constant, such as
`"Don't Panic"`, as one item. If you forget to use the
double quote characters, your text is taken as an `awk`
expression, and you will probably get an error. Keep in mind that a
space is printed between any two items.

Note that the `print` statement is a statement and not an
expression—you can’t use it in the pattern part of a
pattern–action statement, for example.

---

Next: [Output Separators](#Output-Separators), Previous: [The `print` Statement](#Print), Up: [Printing Output](#Printing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.2 `print` Statement Examples [¶](#print-Statement-Examples)

Each `print` statement makes at least one line of output. However, it
isn’t limited to only one line. If an item value is a string containing a
newline, the newline is output along with the rest of the string. A
single `print` statement can make any number of lines this way.

The following is an example of printing a string that contains embedded
newlines
(the ‘`\n`’ is an escape sequence, used to represent the newline
character; see [Escape Sequences](#Escape-Sequences)):

```
$ awk 'BEGIN { print "line one\nline two\nline three" }'
-| line one
-| line two
-| line three
```

The next example, which is run on the `inventory-shipped` file,
prints the first two fields of each input record, with a space between
them:

```
$ awk '{ print $1, $2 }' inventory-shipped
-| Jan 13
-| Feb 15
-| Mar 15
...
```

A common mistake in using the `print` statement is to omit the comma
between two items. This often has the effect of making the items run
together in the output, with no space. The reason for this is that
juxtaposing two string expressions in `awk` means to concatenate
them. Here is the same program, without the comma:

```
$ awk '{ print $1 $2 }' inventory-shipped
-| Jan13
-| Feb15
-| Mar15
...
```

To someone unfamiliar with the `inventory-shipped` file, neither
example’s output makes much sense. A heading line at the beginning
would make it clearer. Let’s add some headings to our table of months
(`$1`) and green crates shipped (`$2`). We do this using
a `BEGIN` rule (see [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)) so that the headings are only
printed once:

```
awk 'BEGIN {  print "Month Crates"
              print "----- ------" }
           {  print $1, $2 }' inventory-shipped
```

When run, the program prints the following:

```
Month Crates
----- ------
Jan 13
Feb 15
Mar 15
...
```

The only problem, however, is that the headings and the table data
don’t line up! We can fix this by printing some spaces between the
two fields:

```
awk 'BEGIN { print "Month Crates"
             print "----- ------" }
           { print $1, "     ", $2 }' inventory-shipped
```

Lining up columns this way can get pretty
complicated when there are many columns to fix. Counting spaces for two
or three columns is simple, but any more than this can take up
a lot of time. This is why the `printf` statement was
created (see [Using `printf` Statements for Fancier Printing](#Printf));
one of its specialties is lining up columns of data.

> **NOTE:** You can continue either a `print` or
> `printf` statement simply by putting a newline after any comma
> (see [`awk` Statements Versus Lines](#Statements_002fLines)).

---

Next: [Controlling Numeric Output with `print`](#OFMT), Previous: [`print` Statement Examples](#Print-Examples), Up: [Printing Output](#Printing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
