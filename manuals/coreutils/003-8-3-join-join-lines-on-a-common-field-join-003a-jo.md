### 8.3 `join`: Join lines on a common field [¶](#join_003a-Join-lines-on-a-common-field)

`join` writes to standard output a line for each pair of input
lines that have identical join fields. Synopsis:

```
join [option]... file1 file2
```

Either file1 or file2 (but not both) can be ‘`-`’,
meaning standard input. file1 and file2 should be
sorted on the join fields.

```
$ cat file1
a 1
b 2
e 5

$ cat file2
a X
e Y
f Z

$ join file1 file2
a 1 X
e 5 Y
```

`join`’s default behavior (when no options are given):

* the join field is the first field in each line;
* fields in the input are separated by one or more blanks, with leading
  blanks on the line ignored;
* fields in the output are separated by a space;
* each output line consists of the join field, the remaining
  fields from file1, then the remaining fields from file2.

* [General options](#General-options-in-join)
* [Pre-sorting](#Sorting-files-for-join)
* [Working with fields](#Working-with-fields)
* [Controlling `join`’s field matching](#Paired-and-unpaired-lines)
* [Header lines](#Header-lines)
* [Union, Intersection and Difference of files](#Set-operations)

---

Next: [Pre-sorting](#Sorting-files-for-join), Up: [`join`: Join lines on a common field](#join-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 8.3.1 General options [¶](#General-options-1)

The program accepts the following options. Also see [Common options](#Common-options).

‘`-a file-number`’ [¶](#index-_002da-4)
:   Print a line for each unpairable line in file file-number (either
    ‘`1`’ or ‘`2`’), in addition to the normal output.

‘`--check-order`’
:   Fail with an error message if either input file is wrongly ordered.

‘`--nocheck-order`’
:   Do not check that both input files are in sorted order. This is the default.

‘`-e string`’ [¶](#index-_002de-3)
:   Replace those output fields that are missing in the input with string.
    I.e., missing fields specified with the `-12jo` options.

‘`--header`’ [¶](#index-_002d_002dheader-1)
:   Treat the first line of each input file as a header line. The header lines
    will be joined and printed as the first output line. If `-o` is used to
    specify output format, the header line will be printed according to the
    specified format. The header lines will not be checked for ordering even if
    `--check-order` is specified. Also if the header lines from each file
    do not match, the heading fields from the first file will be used.

‘`-i`’ [¶](#index-_002di-8)

‘`--ignore-case`’
:   Ignore differences in case when comparing keys.
    With this option, the lines of the input files must be ordered in the same way.
    Use ‘`sort -f`’ to produce this ordering.

‘`-1 field`’ [¶](#index-_002d1-1)
:   Join on field field (a positive integer) of file 1.

‘`-2 field`’ [¶](#index-_002d2-1)
:   Join on field field (a positive integer) of file 2.

‘`-j field`’
:   Equivalent to `-1 field -2 field`.

‘`-o field-list`’

‘`-o auto`’
:   If the keyword ‘`auto`’ is specified, infer the output format from
    the first line in each file. This is the same as the default output format
    but also ensures the same number of fields are output for each line.
    Missing fields are replaced with the `-e` option and extra fields
    are discarded.

    Otherwise, construct each output line according to the format in
    field-list. Each element in field-list is either the single
    character ‘`0`’ or has the form m.n where the file number, m,
    is ‘`1`’ or ‘`2`’ and n is a positive field number.

    A field specification of ‘`0`’ denotes the join field.
    In most cases, the functionality of the ‘`0`’ field spec
    may be reproduced using the explicit m.n that corresponds
    to the join field. However, when printing unpairable lines
    (using either of the `-a` or `-v` options), there is no way
    to specify the join field using m.n in field-list
    if there are unpairable lines in both files.
    To give `join` that functionality, POSIX invented the ‘`0`’
    field specification notation.

    The elements in field-list
    are separated by commas or blanks.
    Blank separators typically need to be quoted for the shell. For
    example, the commands ‘`join -o 1.2,2.2`’ and ‘`join -o '1.2
    2.2'`’ are equivalent.

    All output lines – including those printed because of any `-a`
    or `-v` option – are subject to the specified field-list.

‘`-t char`’
:   Use character char as the input and output field separator.
    Treat as significant each occurrence of char in the input file.
    Use ‘`sort -t char`’, without the `-b` option of
    ‘`sort`’, to produce this ordering. If ‘`join -t ''`’ is specified,
    the whole line is considered, matching the default operation of sort.
    If ‘`-t '\0'`’ is specified then the ASCII NUL
    character is used to delimit the fields.

‘`-v file-number`’
:   Print a line for each unpairable line in file file-number
    (either ‘`1`’ or ‘`2`’), instead of the normal output.

‘`-z`’ [¶](#index-_002dz-10)

‘`--zero-terminated`’
:   Delimit items with a zero byte rather than a newline (ASCII LF).
    I.e., treat input as items separated by ASCII NUL
    and terminate output items with ASCII NUL.
    This option can be useful in conjunction with ‘`perl -0`’ or
    ‘`find -print0`’ and ‘`xargs -0`’ which do the same in order to
    reliably handle arbitrary file names (even those containing blanks
    or other special characters).
    With `-z` the newline character is treated as a field separator.

An exit status of zero indicates success,
and a nonzero value indicates failure.

If the `--check-order` option is given, unsorted inputs will
cause a fatal error message. If the option `--nocheck-order`
is given, unsorted inputs will never cause an error message. If neither
of these options is given, wrongly sorted inputs are diagnosed
only if an input file is found to contain unpairable
lines, and when both input files are non empty.
If an input file is diagnosed as being unsorted, the `join`
command will exit with a nonzero status (and the output should not be used).

Forcing `join` to process wrongly sorted input files
containing unpairable lines by specifying `--nocheck-order` is
not guaranteed to produce any particular output. The output will
probably not correspond with whatever you hoped it would be.

---

Next: [Working with fields](#Working-with-fields), Previous: [General options](#General-options-in-join), Up: [`join`: Join lines on a common field](#join-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 8.3.2 Pre-sorting [¶](#Pre_002dsorting)

`join` requires sorted input files. Each input file should be
sorted according to the key (=field/column number) used in
`join`. The recommended sorting option is ‘`sort -k 1b,1`’
(assuming the desired key is in the first column).

Typical usage:

```
$ sort -k 1b,1 file1 > file1.sorted
$ sort -k 1b,1 file2 > file2.sorted
$ join file1.sorted file2.sorted > file3
```

Normally, the sort order is that of the
collating sequence specified by the `LC_COLLATE` locale. Unless
the `-t` option is given, the sort comparison ignores blanks at
the start of the join field, as in `sort -b`. If the
`--ignore-case` option is given, the sort comparison ignores
the case of characters in the join field, as in `sort -f`:

```
$ sort -k 1bf,1 file1 > file1.sorted
$ sort -k 1bf,1 file2 > file2.sorted
$ join --ignore-case file1.sorted file2.sorted > file3
```

The `sort` and `join` commands should use consistent
locales and options if the output of `sort` is fed to
`join`. You can use a command like ‘`sort -k 1b,1`’ to
sort a file on its default join field, but if you select a non-default
locale, join field, separator, or comparison options, then you should
do so consistently between `join` and `sort`.

To avoid any locale-related issues, it is recommended to use the
‘`C`’ locale for both commands:

```
$ LC_ALL=C sort -k 1b,1 file1 > file1.sorted
$ LC_ALL=C sort -k 1b,1 file2 > file2.sorted
$ LC_ALL=C join file1.sorted file2.sorted > file3
```

---

Next: [Controlling `join`’s field matching](#Paired-and-unpaired-lines), Previous: [Pre-sorting](#Sorting-files-for-join), Up: [`join`: Join lines on a common field](#join-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 8.3.3 Working with fields [¶](#Working-with-fields-1)

Use `-1`,`-2` to set the key fields for each of the input files.
Ensure the preceding `sort` commands operated on the same fields.

The following example joins two files, using the values from seventh field
of the first file and the third field of the second file:

```
$ sort -k 7b,7 file1 > file1.sorted
$ sort -k 3b,3 file2 > file2.sorted
$ join -1 7 -2 3 file1.sorted file2.sorted > file3
```

If the field number is the same for both files, use `-j`:

```
$ sort -k4b,4 file1 > file1.sorted
$ sort -k4b,4 file2 > file2.sorted
$ join -j4    file1.sorted file2.sorted > file3
```

Both `sort` and `join` operate of whitespace-delimited
fields. To specify a different delimiter, use `-t` in *both*:

```
$ sort -t, -k3b,3 file1 > file1.sorted
$ sort -t, -k3b,3 file2 > file2.sorted
$ join -t, -j3    file1.sorted file2.sorted > file3
```

To specify a tab (ASCII 0x09) character instead of whitespace,
use:[2](#FOOT2)

```
$ sort -t$'\t' -k3b,3 file1 > file1.sorted
$ sort -t$'\t' -k3b,3 file2 > file2.sorted
$ join -t$'\t' -j3    file1.sorted file2.sorted > file3
```

If ‘`join -t ''`’ is specified then the whole line is considered which
matches the default operation of sort:

```
$ sort file1 > file1.sorted
$ sort file2 > file2.sorted
$ join -t '' file1.sorted file2.sorted > file3
```

---

Next: [Header lines](#Header-lines), Previous: [Working with fields](#Working-with-fields), Up: [`join`: Join lines on a common field](#join-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 8.3.4 Controlling `join`’s field matching [¶](#Controlling-join_0027s-field-matching)

In this section the `sort` commands are omitted for brevity.
Sorting the files before joining is still required.

`join`’s default behavior is to print only lines common to
both input files. Use `-a` and `-v` to print unpairable lines
from one or both files.

All examples below use the following two (pre-sorted) input files:

|  |  |
| --- | --- |
| ``` $ cat file1 a 1 b 2 ``` | ``` $ cat file2 a A c C ``` |

| Command | Outcome |
| --- | --- |
| ``` $ join file1 file2 a 1 A ``` | common lines (*intersection*) |
| ``` $ join -a 1 file1 file2 a 1 A b 2 ``` | common lines *and* unpaired lines from the first file |
| ``` $ join -a 2 file1 file2 a 1 A c C ``` | common lines *and* unpaired lines from the second file |
| ``` $ join -a 1 -a 2 file1 file2 a 1 A b 2 c C ``` | all lines (paired and unpaired) from both files (*union*).   see note below regarding `-o auto`. |
| ``` $ join -v 1 file1 file2 b 2 ``` | unpaired lines from the first file (*difference*) |
| ``` $ join -v 2 file1 file2 c C ``` | unpaired lines from the second file (*difference*) |
| ``` $ join -v 1 -v 2 file1 file2 b 2 c C ``` | unpaired lines from both files, omitting common lines (*symmetric difference*). |

The `-o auto -e X` options are useful when dealing with unpaired lines.
The following example prints all lines (common and unpaired) from both files.
Without `-o auto` it is not easy to discern which fields originate from
which file:

```
$ join -a 1 -a 2 file1 file2
a 1 A
b 2
c C

$ join -o auto -e X -a 1 -a 2 file1 file2
a 1 A
b 2 X
c X C
```

If the input has no unpairable lines, a GNU extension is
available; the sort order can be any order that considers two fields
to be equal if and only if the sort comparison described above
considers them to be equal. For example:

```
$ cat file1
a a1
c c1
b b1

$ cat file2
a a2
c c2
b b2

$ join file1 file2
a a1 a2
c c1 c2
b b1 b2
```

---

Next: [Union, Intersection and Difference of files](#Set-operations), Previous: [Controlling `join`’s field matching](#Paired-and-unpaired-lines), Up: [`join`: Join lines on a common field](#join-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 8.3.5 Header lines [¶](#Header-lines-1)

The `--header` option can be used when the files to join
have a header line which is not sorted:

```
$ cat file1
Name     Age
Alice    25
Charlie  34

$ cat file2
Name   Country
Alice  France
Bob    Spain

$ join --header -o auto -e NA -a1 -a2 file1 file2
Name     Age   Country
Alice    25    France
Bob      NA    Spain
Charlie  34    NA
```

To sort a file with a header line, use GNU `sed -u`.
The following example sort the files but keeps the first line of each
file in place:

```
$ ( sed -u 1q ; sort -k2b,2 ) < file1 > file1.sorted
$ ( sed -u 1q ; sort -k2b,2 ) < file2 > file2.sorted
$ join --header -o auto -e NA -a1 -a2 file1.sorted file2.sorted > file3
```

---

Previous: [Header lines](#Header-lines), Up: [`join`: Join lines on a common field](#join-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 8.3.6 Union, Intersection and Difference of files [¶](#Union_002c-Intersection-and-Difference-of-files)

Combine `sort`, `uniq` and `join` to
perform the equivalent of set operations on files:

| Command | outcome |
| --- | --- |
| `sort -u file1 file2` | Union of unsorted files |
| `sort file1 file2 | uniq -d` | Intersection of unsorted files |
| `sort file1 file1 file2 | uniq -u` | Difference of unsorted files |
| `sort file1 file2 | uniq -u` | Symmetric Difference of unsorted files |
| `join -t '' -a1 -a2 file1 file2` | Union of sorted files |
| `join -t '' file1 file2` | Intersection of sorted files |
| `join -t '' -v2 file1 file2` | Difference of sorted files |
| `join -t '' -v1 -v2 file1 file2` | Symmetric Difference of sorted files |

All examples above operate on entire lines and not on specific fields:
`sort` without `-k` and `join -t ''` both consider
entire lines as the key.

---

Next: [Directory listing](#Directory-listing), Previous: [Operating on fields](#Operating-on-fields), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 9 Operating on characters [¶](#Operating-on-characters-1)

These commands operate on individual characters.

* [`tr`: Translate, squeeze, and/or delete characters](#tr-invocation)
* [`expand`: Convert tabs to spaces](#expand-invocation)
* [`unexpand`: Convert spaces to tabs](#unexpand-invocation)

---

Next: [`expand`: Convert tabs to spaces](#expand-invocation), Up: [Operating on characters](#Operating-on-characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 9.1 `tr`: Translate, squeeze, and/or delete characters [¶](#tr_003a-Translate_002c-squeeze_002c-and_002for-delete-characters)

Synopsis:

```
tr [option]... string1 [string2]
```

`tr` copies standard input to standard output, performing
one of the following operations:

* translate, and optionally squeeze repeated characters in the result,
* squeeze repeated characters,
* delete characters,
* delete characters, then squeeze repeated characters from the result.

The string1 and string2 operands define arrays of
characters array1 and array2. By default array1
lists input characters that `tr` operates on, and array2
lists corresponding translations. In some cases the second operand is
omitted.

The program accepts the following options. Also see [Common options](#Common-options).
Options must precede operands.

‘`-c`’ [¶](#index-_002dc-11)

‘`-C`’

‘`--complement`’
:   Instead of array1, use its complement (all characters not
    specified by string1), in ascending order. Use this option with
    caution in multibyte locales where its meaning is not always clear
    or portable; see [Specifying arrays of characters](#Character-arrays).

‘`-d`’ [¶](#index-_002dd-9)

‘`--delete`’
:   Delete characters in array1; do not translate.

‘`-s`’ [¶](#index-_002ds-14)

‘`--squeeze-repeats`’
:   Replace each sequence of a repeated character that is listed in
    the last specified array, with a single occurrence of that character.

‘`-t`’ [¶](#index-_002dt-7)

‘`--truncate-set1`’
:   Truncate array1 to the length of array2.

An exit status of zero indicates success,
and a nonzero value indicates failure.

* [Specifying arrays of characters](#Character-arrays)
* [Translating](#Translating)
* [Squeezing repeats and deleting](#Squeezing-and-deleting)

---

Next: [Translating](#Translating), Up: [`tr`: Translate, squeeze, and/or delete characters](#tr-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 9.1.1 Specifying arrays of characters [¶](#Specifying-arrays-of-characters)

The string1 and string2 operands are not regular
expressions, even though they may look similar. Instead, they
merely represent arrays of characters. As a GNU extension to POSIX,
an empty string operand represents an empty array of characters.

The interpretation of string1 and string2 depends on locale.
GNU `tr` fully supports only safe single-byte locales,
where each possible input byte represents a single character.
Unfortunately, this means GNU `tr` will not handle commands
like ‘`tr ö Ł`’ the way you might expect,
since (assuming a UTF-8 encoding) this is equivalent to
‘`tr '\303\266' '\305\201'`’ and GNU `tr` will
simply transliterate all ‘`\303`’ bytes to ‘`\305`’ bytes, etc.
POSIX does not clearly specify the behavior of `tr` in locales
where characters are represented by byte sequences instead of by
individual bytes, or where data might contain invalid bytes that are
encoding errors. To avoid problems in this area, you can run
`tr` in a safe single-byte locale by using a shell command
like ‘`LC_ALL=C tr`’ instead of plain `tr`.

Although most characters simply represent themselves in string1
and string2, the strings can contain shorthands listed below,
for convenience. Some shorthands can be used only in string1 or
string2, as noted below.

Backslash escapes [¶](#index-backslash-escapes)
:   The following backslash escape sequences are recognized:

    ‘`\a`’
    :   Bell (BEL, Control-G).

    ‘`\b`’
    :   Backspace (BS, Control-H).

    ‘`\f`’
    :   Form feed (FF, Control-L).

    ‘`\n`’
    :   Newline (LF, Control-J).

    ‘`\r`’
    :   Carriage return (CR, Control-M).

    ‘`\t`’
    :   Tab (HT, Control-I).

    ‘`\v`’
    :   Vertical tab (VT, Control-K).

    ‘`\ooo`’
    :   The eight-bit byte with the value given by ooo, which is the longest
        sequence of one to three octal digits following the backslash.
        For portability, ooo should represent a value that fits in eight bits.
        As a GNU extension to POSIX, if the value would not fit, then only the
        first two digits of ooo are used, e.g., ‘`\400`’
        is equivalent to ‘`\0400`’ and represents a two-byte sequence.

    ‘`\\`’
    :   A backslash.

    It is an error if no character follows an unescaped backslash.
    As a GNU extension, a backslash followed by a character not listed
    above is interpreted as that character, removing any special
    significance; this can be used to escape the characters
    ‘`[`’ and ‘`-`’ when they would otherwise be special.

Ranges [¶](#index-ranges)
:   The notation ‘`m-n`’ expands to the characters
    from m through n, in ascending order. m should
    not collate after n; if it does, an error results. As an example,
    ‘`0-9`’ is the same as ‘`0123456789`’.

    GNU `tr` does not support the System V syntax that uses square
    brackets to enclose ranges. Translations specified in that format
    sometimes work as expected, since the brackets are often transliterated
    to themselves. However, they should be avoided because they sometimes
    behave unexpectedly. For example, ‘`tr -d '[0-9]'`’ deletes brackets
    as well as digits.

    Many historically common and even accepted uses of ranges are not fully
    portable. For example, on EBCDIC hosts using the ‘`A-Z`’
    range will not do what most would expect because ‘`A`’ through ‘`Z`’
    are not contiguous as they are in ASCII.
    One way to work around this is to use character classes (see below).
    Otherwise, it is most portable (and most ugly) to enumerate the members
    of the ranges.

Repeated characters [¶](#index-repeated-characters)
:   The notation ‘`[c*n]`’ in string2 expands to n
    copies of character c. Thus, ‘`[y*6]`’ is the same as
    ‘`yyyyyy`’. The notation ‘`[c*]`’ in string2 expands
    to as many copies of c as are needed to make array2 as long as
    array1. If n begins with ‘`0`’, it is interpreted in
    octal, otherwise in decimal. A zero-valued n is treated as if
    it were absent.

Character classes [¶](#index-character-classes)
:   The notation ‘`[:class:]`’ expands to all characters in
    the (predefined) class class. When the `--delete` (`-d`)
    and `--squeeze-repeats` (`-s`) options are both given, any
    character class can be used in string2. Otherwise, only the
    character classes `lower` and `upper` are accepted in
    string2, and then only if the corresponding character class
    (`upper` and `lower`, respectively) is specified in the same
    relative position in string1. Doing this specifies case conversion.
    Except for case conversion, a class’s characters appear in no particular order.
    The class names are given below; an error results when an invalid class
    name is given.

    `alnum` [¶](#index-alnum)
    :   Letters and digits.

    `alpha` [¶](#index-alpha)
    :   Letters.

    `blank` [¶](#index-blank)
    :   Horizontal whitespace.

    `cntrl` [¶](#index-cntrl)
    :   Control characters.

    `digit` [¶](#index-digit)
    :   Digits.

    `graph` [¶](#index-graph)
    :   Printable characters, not including space.

    `lower` [¶](#index-lower)
    :   Lowercase letters.

    `print` [¶](#index-print)
    :   Printable characters, including space.

    `punct` [¶](#index-punct)
    :   Punctuation characters.

    `space` [¶](#index-space)
    :   Horizontal or vertical whitespace.

    `upper` [¶](#index-upper)
    :   Uppercase letters.

    `xdigit` [¶](#index-xdigit)
    :   Hexadecimal digits.

Equivalence classes [¶](#index-equivalence-classes)
:   The syntax ‘`[=c=]`’ expands to all characters equivalent to
    c, in no particular order. These equivalence classes are
    allowed in string2 only when `--delete` (`-d`) and
    `--squeeze-repeats` `-s` are both given.

    Although equivalence classes are intended to support non-English alphabets,
    there seems to be no standard way to define them or determine their
    contents. Therefore, they are not fully implemented in GNU `tr`;
    each character’s equivalence class consists only of that character,
    which is of no particular use.

---

Next: [Squeezing repeats and deleting](#Squeezing-and-deleting), Previous: [Specifying arrays of characters](#Character-arrays), Up: [`tr`: Translate, squeeze, and/or delete characters](#tr-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 9.1.2 Translating [¶](#Translating-1)

`tr` performs translation when string1 and string2 are
both given and the `--delete` (`-d`) option is not given.
`tr` translates each character of its input that is in array1
to the corresponding character in array2. Characters not in
array1 are passed through unchanged.

As a GNU extension to POSIX, when a character appears more than once
in array1, only the final instance is used. For example, these
two commands are equivalent:

```
tr aaa xyz
tr a z
```

A common use of `tr` is to convert lowercase characters to
uppercase. This can be done in many ways. Here are three of them:

```
tr abcdefghijklmnopqrstuvwxyz ABCDEFGHIJKLMNOPQRSTUVWXYZ
tr a-z A-Z
tr '[:lower:]' '[:upper:]'
```

However, ranges like `a-z` are not portable outside the C locale.

When `tr` is performing translation, array1 and array2
typically have the same length. If array1 is shorter than
array2, the extra characters at the end of array2 are ignored.

On the other hand, making array1 longer than array2 is not
portable; POSIX says that the result is undefined. In this situation,
BSD `tr` pads array2 to the length of array1 by repeating
the last character of array2 as many times as necessary. System V
`tr` truncates array1 to the length of array2.

By default, GNU `tr` handles this case like BSD `tr`.
When the `--truncate-set1` (`-t`) option is given,
GNU `tr` handles this case like the System V `tr`
instead. This option is ignored for operations other than translation.

Acting like System V `tr` in this case breaks the relatively common
BSD idiom:

```
tr -cs A-Za-z0-9 '\012'
```

because it converts only zero bytes (the first element in the
complement of array1), rather than all non-alphanumerics, to
newlines.

By the way, the above idiom is not portable because it uses ranges, and
it assumes that the octal code for newline is 012. Here is a better
way to write it:

```
tr -cs '[:alnum:]' '[\n*]'
```

---

Previous: [Translating](#Translating), Up: [`tr`: Translate, squeeze, and/or delete characters](#tr-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 9.1.3 Squeezing repeats and deleting [¶](#Squeezing-repeats-and-deleting)

When given just the `--delete` (`-d`) option, `tr`
removes any input characters that are in array1.

When given just the `--squeeze-repeats` (`-s`) option
and not translating, `tr` replaces each input sequence of a
repeated character that is in array1 with a single occurrence of
that character.

When given both `--delete` and `--squeeze-repeats`, `tr`
first performs any deletions using array1, then squeezes repeats
from any remaining characters using array2.

The `--squeeze-repeats` option may also be used when translating,
in which case `tr` first performs translation, then squeezes
repeats from any remaining characters using array2.

Here are some examples to illustrate various combinations of options:

* Remove all zero bytes:

  ```
  tr -d '\0'
  ```
* Put all words on lines by themselves. This converts all
  non-alphanumeric characters to newlines, then squeezes each string
  of repeated newlines into a single newline:

  ```
  tr -cs '[:alnum:]' '[\n*]'
  ```
* Convert each sequence of repeated newlines to a single newline.
  I.e., delete empty lines:

  ```
  tr -s '\n'
  ```
* Find doubled occurrences of words in a document.
  For example, people often write “the the” with the repeated words
  separated by a newline. The Bourne shell script below works first
  by converting each sequence of punctuation and blank characters to a
  single newline. That puts each “word” on a line by itself.
  Next it maps all uppercase characters to lower case, and finally it
  runs `uniq` with the `-d` option to print out only the words
  that were repeated.

  ```
  #!/bin/sh
  cat -- "$@" \
    | tr -s '[:punct:][:blank:]' '[\n*]' \
    | tr '[:upper:]' '[:lower:]' \
    | uniq -d
  ```
* Deleting a small set of characters is usually straightforward. For example,
  to remove all ‘`a`’s, ‘`x`’s, and ‘`M`’s you would do this:

  ```
  tr -d axM
  ```

  However, when ‘`-`’ is one of those characters, it can be tricky because
  ‘`-`’ has special meanings. Performing the same task as above but also
  removing all ‘`-`’ characters, we might try `tr -d -axM`, but
  that would fail because `tr` would try to interpret `-a` as
  a command-line option. Alternatively, we could try putting the hyphen
  inside the string, `tr -d a-xM`, but that wouldn’t work either because
  it would make `tr` interpret `a-x` as the range of characters
  ‘`a`’…‘`x`’ rather than the three.
  One way to solve the problem is to put the hyphen at the end of the list
  of characters:

  ```
  tr -d axM-
  ```

  Or you can use ‘`--`’ to terminate option processing:

  ```
  tr -d -- -axM
  ```

---

Next: [`unexpand`: Convert spaces to tabs](#unexpand-invocation), Previous: [`tr`: Translate, squeeze, and/or delete characters](#tr-invocation), Up: [Operating on characters](#Operating-on-characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 9.2 `expand`: Convert tabs to spaces [¶](#expand_003a-Convert-tabs-to-spaces)

`expand` writes the contents of each given file, or standard
input if none are given or for a file of ‘`-`’, to standard
output, with tab characters converted to the appropriate number of
spaces. Synopsis:

```
expand [option]... [file]...
```

By default, `expand` converts all tabs to spaces. It preserves
backspace characters in the output; they decrement the column count for
tab calculations. The default action is equivalent to `-t 8` (set
tabs every 8 columns).

The program accepts the following options. Also see [Common options](#Common-options).

‘`-t tab1[,tab2]…`’ [¶](#index-_002dt-8)

‘`--tabs=tab1[,tab2]…`’
:   If only one tab stop is given, set the tabs tab1 spaces apart
    (default is 8). Otherwise, set the tabs at columns tab1,
    tab2, … (numbered from 0), and replace any tabs beyond the
    last tab stop given with single spaces.
    Tab stops can be separated by blanks as well as by commas.

    As a GNU extension the last tab specified can be prefixed
    with a ‘`/`’ to indicate a tab size to use for remaining positions.
    For example, `--tabs=2,4,/8` will set tab stops at position 2 and 4,
    and every multiple of 8 after that.

    Also the last tab specified can be prefixed with a ‘`+`’ to indicate
    a tab size to use for remaining positions, offset from the final explicitly
    specified tab stop.
    For example, to ignore the 1 character gutter present in diff output,
    one can specify a 1 character offset using `--tabs=1,+8`,
    which will set tab stops at positions 1,9,17,…

    For compatibility, GNU `expand` also accepts the obsolete
    option syntax, `-t1[,t2]…`. New scripts
    should use `-t t1[,t2]…` instead.

‘`-i`’ [¶](#index-_002di-9)

‘`--initial`’
:   Only convert initial tabs (those that precede all non-space or non-tab
    characters) on each line to spaces.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Previous: [`expand`: Convert tabs to spaces](#expand-invocation), Up: [Operating on characters](#Operating-on-characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 9.3 `unexpand`: Convert spaces to tabs [¶](#unexpand_003a-Convert-spaces-to-tabs)

`unexpand` writes the contents of each given file, or
standard input if none are given or for a file of ‘`-`’, to
standard output, converting blanks at the beginning of each line into
as many tab characters as needed. In the default POSIX
locale, a *blank* is a space or a tab; other locales may specify
additional blank characters. Synopsis:

```
unexpand [option]... [file]...
```

By default, `unexpand` converts only initial blanks (those
that precede all non-blank characters) on each line. It
preserves backspace characters in the output; they decrement the column
count for tab calculations. By default, tabs are set at every 8th
column.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-t tab1[,tab2]…`’ [¶](#index-_002dt-9)

‘`--tabs=tab1[,tab2]…`’
:   If only one tab stop is given, set the tabs tab1 columns apart
    instead of the default 8. Otherwise, set the tabs at columns
    tab1, tab2, … (numbered from 0), and leave blanks
    beyond the tab stops given unchanged.
    Tab stops can be separated by blanks as well as by commas.

    As a GNU extension the last tab specified can be prefixed
    with a ‘`/`’ to indicate a tab size to use for remaining positions.
    For example, `--tabs=2,4,/8` will set tab stops at position 2 and 4,
    and every multiple of 8 after that.

    Also the last tab specified can be prefixed with a ‘`+`’ to indicate
    a tab size to use for remaining positions, offset from the final explicitly
    specified tab stop.
    For example, to ignore the 1 character gutter present in diff output,
    one can specify a 1 character offset using `--tabs=1,+8`,
    which will set tab stops at positions 1,9,17,…

    This option implies the `-a` option.

    For compatibility, GNU `unexpand` supports the obsolete option syntax,
    `-tab1[,tab2]…`, where tab stops must be
    separated by commas. (Unlike `-t`, this obsolete option does
    not imply `-a`.) New scripts should use `--first-only -t
    tab1[,tab2]…` instead.

‘`-a`’ [¶](#index-_002da-5)

‘`--all`’
:   Also convert all sequences of two or more blanks just before a tab stop,
    even if they occur after non-blank characters in a line.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [Basic operations](#Basic-operations), Previous: [Operating on characters](#Operating-on-characters), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 10 Directory listing [¶](#Directory-listing-1)

This chapter describes the `ls` command and its variants `dir`
and `vdir`, which list information about files.

* [`ls`: List directory contents](#ls-invocation)
* [`dir`: Briefly list directory contents](#dir-invocation)
* [`vdir`: Verbosely list directory contents](#vdir-invocation)
* [`dircolors`: Color setup for `ls`](#dircolors-invocation)

---

Next: [`dir`: Briefly list directory contents](#dir-invocation), Up: [Directory listing](#Directory-listing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 10.1 `ls`: List directory contents [¶](#ls_003a-List-directory-contents)

The `ls` program lists information about files (of any type,
including directories). Options and file arguments can be intermixed
arbitrarily, as usual. Later options override earlier options that
are incompatible.

For non-option command-line arguments that are directories, by default
`ls` lists the contents of directories, not recursively, and
omitting files with names beginning with ‘`.`’. For other non-option
arguments, by default `ls` lists just the file name. If no
non-option argument is specified, `ls` operates on the current
directory, acting as if it had been invoked with a single argument of ‘`.`’.

By default, `ls` lists each directory’s contents alphabetically,
according to the locale
settings in effect.[3](#FOOT3)
If standard output is
a terminal, the output is in columns (sorted vertically) and control
characters are output as question marks; otherwise, the output is listed
one per line and control characters are output as-is.

Because `ls` is such a fundamental program, it has accumulated many
options over the years. They are described in the subsections below;
within each section, options are listed alphabetically (ignoring case).
The division of options into the subsections is not absolute, since some
options affect more than one aspect of `ls`’s operation.

Exit status:

```
0 success
1 minor problems  (e.g., failure to access a file or directory not
  specified as a command line argument.  This happens when listing a
  directory in which entries are actively being removed or renamed.)
2 serious trouble (e.g., memory exhausted, invalid option, failure
  to access a file or directory specified as a command line argument
  or a directory loop)
```

Also see [Common options](#Common-options).

* [Which files are listed](#Which-files-are-listed)
* [What information is listed](#What-information-is-listed)
* [Sorting the output](#Sorting-the-output)
* [General output formatting](#General-output-formatting)
* [Formatting file timestamps](#Formatting-file-timestamps)
* [Formatting the file names](#Formatting-the-file-names)

---

Next: [What information is listed](#What-information-is-listed), Up: [`ls`: List directory contents](#ls-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 10.1.1 Which files are listed [¶](#Which-files-are-listed-1)

These options determine which files `ls` lists information for.
By default, `ls` lists files and the contents of any
directories on the command line, except that in directories it ignores
files whose names start with ‘`.`’.

‘`-a`’ [¶](#index-_002da-6)

‘`--all`’
:   In directories, do not ignore file names that start with ‘`.`’.

‘`-A`’ [¶](#index-_002dA-3)

‘`--almost-all`’
:   In directories, do not ignore all file names that start with ‘`.`’;
    ignore only `.` and `..`. The `--all` (`-a`)
    option overrides this option.

‘`-B`’ [¶](#index-_002dB)

‘`--ignore-backups`’
:   In directories, ignore files that end with ‘`~`’. This option is
    equivalent to ‘`--ignore='*~' --ignore='.*~'`’.

‘`-d`’ [¶](#index-_002dd-10)

‘`--directory`’
:   List just the names of directories, as with other types of files, rather
    than listing their contents.
    Do not follow symbolic links listed on the
    command line unless the `--dereference-command-line` (`-H`),
    `--dereference` (`-L`), or
    `--dereference-command-line-symlink-to-dir` options are specified.

‘`-H`’ [¶](#index-_002dH-1)

‘`--dereference-command-line`’
:   If a command line argument specifies a symbolic link, show information
    for the file the link references rather than for the link itself.

‘`--dereference-command-line-symlink-to-dir`’ [¶](#index-_002d_002ddereference_002dcommand_002dline_002dsymlink_002dto_002ddir)
:   Do not dereference symbolic links, with one exception:
    if a command line argument specifies a symbolic link that refers to
    a directory, show information for that directory rather than for the
    link itself.
    This is the default behavior unless long format is being used
    or any of the following options is in effect:
    `--classify` (`-F`),
    `--directory` (`-d`),
    `--dereference` (`-L`), or
    `--dereference-command-line` (`-H`)).

‘`--hide=PATTERN`’ [¶](#index-_002d_002dhide_003dpattern)
:   In directories, ignore files whose names match the shell pattern
    pattern, unless the `--all` (`-a`) or
    `--almost-all` (`-A`) is also given. This
    option acts like `--ignore=pattern` except that it has no
    effect if `--all` (`-a`) or `--almost-all`
    (`-A`) is also given.

    This option can be useful in shell aliases. For example, if
    `lx` is an alias for ‘`ls --hide='*~'`’ and `ly` is
    an alias for ‘`ls --ignore='*~'`’, then the command ‘`lx -A`’
    lists the file `README~` even though ‘`ly -A`’ would not.

‘`-I pattern`’ [¶](#index-_002dI)

‘`--ignore=pattern`’
:   In directories, ignore files whose names match the shell pattern
    (not regular expression) pattern. As
    in the shell, an initial ‘`.`’ in a file name does not match a
    wildcard at the start of pattern. Sometimes it is useful
    to give this option several times. For example,

    ```
    $ ls --ignore='.??*' --ignore='.[^.]' --ignore='#*'
    ```

    The first option ignores names of length 3 or more that start with ‘`.`’,
    the second ignores all two-character names that start with ‘`.`’
    except ‘`..`’, and the third ignores names that start with ‘`#`’.

‘`-L`’ [¶](#index-_002dL-2)

‘`--dereference`’
:   When showing file information for a symbolic link, show information
    for the file the link references rather than the link itself.
    However, even with this option, `ls` still prints the name
    of the link itself, not the name of the file that the link points to.

‘`-R`’ [¶](#index-_002dR-2)

‘`--recursive`’
:   List the contents of all directories recursively.

---

Next: [Sorting the output](#Sorting-the-output), Previous: [Which files are listed](#Which-files-are-listed), Up: [`ls`: List directory contents](#ls-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 10.1.2 What information is listed [¶](#What-information-is-listed-1)

These options affect the information that `ls` displays. By
default, only file names are shown.

‘`--author`’ [¶](#index-_002d_002dauthor)
:   In long format, list each file’s author.
    In GNU/Hurd, file authors can differ from their owners, but in other
    operating systems the two are the same.

‘`-D`’ [¶](#index-_002dD-1)

‘`--dired`’
:   Print an additional line after the main output:

    ```
    //DIRED// beg1 end1 beg2 end2 ...
    ```

    The begn and endn are unsigned integers that record the
    byte position of the beginning and end of each file name in the output.
    This makes it easy for Emacs to find the names, even when they contain
    unusual characters such as space or newline, without fancy searching.

    If directories are being listed recursively via
    `--recursive` (`-R`), output a similar
    line with offsets for each subdirectory name:

    ```
    //SUBDIRED// beg1 end1 ...
    ```

    Finally, output a line of the form:

    ```
    //DIRED-OPTIONS// --quoting-style=word
    ```

    where word is the quoting style (see [Formatting the file names](#Formatting-the-file-names)).

    Here is an actual example:

    ```
    $ mkdir -p a/sub/deeper a/sub2
    $ touch a/f1 a/f2
    $ touch a/sub/deeper/file
    $ ls -gloRF --dired a
      a:
      total 8
      -rw-r--r-- 1    0 Jun 10 12:27 f1
      -rw-r--r-- 1    0 Jun 10 12:27 f2
      drwxr-xr-x 3 4096 Jun 10 12:27 sub/
      drwxr-xr-x 2 4096 Jun 10 12:27 sub2/

      a/sub:
      total 4
      drwxr-xr-x 2 4096 Jun 10 12:27 deeper/

      a/sub/deeper:
      total 0
      -rw-r--r-- 1 0 Jun 10 12:27 file

      a/sub2:
      total 0
    //DIRED// 48 50 84 86 120 123 158 162 217 223 282 286
    //SUBDIRED// 2 3 167 172 228 240 290 296
    //DIRED-OPTIONS// --quoting-style=literal
    ```

    The pairs of offsets on the ‘`//DIRED//`’ line above delimit
    these names: `f1`, `f2`, `sub`, `sub2`, `deeper`,
    `file`.
    The offsets on the ‘`//SUBDIRED//`’ line delimit the following
    directory names: `a`, `a/sub`, `a/sub/deeper`, `a/sub2`.

    Here is an example of how to extract the fifth entry name, ‘`deeper`’,
    corresponding to the pair of offsets, 222 and 228:

    ```
    $ ls -gloRF --dired a > out
    $ dd bs=1 skip=222 count=6 < out 2>/dev/null; echo
    deeper
    ```

    Although the listing above includes a trailing slash
    for the ‘`deeper`’ entry, the offsets select the name without
    the trailing slash. However, if you invoke `ls` with `--dired`
    (`-D`) along with an option like
    `--escape` (`-b`) and operate
    on a file whose name contains special characters, the backslash
    *is* included:

    ```
    $ touch 'a b'
    $ ls -blog --dired 'a b'
      -rw-r--r-- 1 0 Jun 10 12:28 a\ b
    //DIRED// 30 34
    //DIRED-OPTIONS// --quoting-style=escape
    ```

    If you use a quoting style like `--quoting-style=c` (`-Q`)
    that adds quote marks, then the offsets include the quote marks.
    So beware that the user may select the quoting style via the environment
    variable `QUOTING_STYLE`. Hence, applications using `--dired`
    should either specify an explicit `--quoting-style=literal`
    (`-N`) option on the command line, or else be
    prepared to parse the escaped names.

    The `--dired` (`-D`) option implies long format output
    with hyperlinks disabled, and takes precedence over previously specified
    output formats or hyperlink mode.

‘`--full-time`’ [¶](#index-_002d_002dfull_002dtime)
:   Produce long format, and list times in full. It is
    equivalent to using `--format=long` (`-l`) with
    `--time-style=full-iso` (see [Formatting file timestamps](#Formatting-file-timestamps)).

‘`-g`’ [¶](#index-_002dg-3)
:   Produce long format, but omit owner information.

‘`-G`’ [¶](#index-_002dG)

‘`--no-group`’
:   Inhibit display of group information in long format.
    (This is the default in some non-GNU versions of `ls`, so we
    provide this option for compatibility.)

‘`-h`’ [¶](#index-_002dh-4)

‘`--human-readable`’
:   Append a size letter to each size, such as ‘`M`’ for mebibytes.
    Powers of 1024 are used, not 1000; ‘`M`’ stands for 1,048,576 bytes.
    This option is equivalent to `--block-size=human-readable`.
    Use the `--si` option if you prefer powers of 1000.

‘`-i`’ [¶](#index-_002di-10)

‘`--inode`’
:   Print the inode number (also called the file serial number and index
    number) of each file to the left of the file name. (This number
    uniquely identifies each file within a particular file system.)

‘`-l`’ [¶](#index-_002dl-7)

‘`--format=long`’

‘`--format=verbose`’
:   Produce long format.
    In addition to the name of each file, print the file type, file mode bits,
    number of hard links, owner name, group name, size, and
    timestamp (see [Formatting file timestamps](#Formatting-file-timestamps)), normally
    the modification timestamp (the mtime, see [File timestamps](#File-timestamps)).
    If the owner or group name cannot be determined, print
    the owner or group ID instead, right-justified as a cue
    that it is a number rather than a textual name.
    Print question marks for other information that
    cannot be determined.

    For block special files and character special files, the size field is
    replaced by the corresponding device major and minor numbers as two decimal
    numbers separated by a comma and at least one space.

    Normally the size is printed as a byte count without punctuation, but
    this can be overridden (see [Block size](#Block-size)).
    For example, `--human-readable` (`-h`)
    prints an abbreviated, human-readable count, and
    ‘`--block-size="'1"`’ prints a byte count with the thousands
    separator of the current locale.

    For each directory that is listed, preface the files with a line
    ‘`total blocks`’, where blocks is the file system allocation
    for all files in that directory. The block size currently defaults to 1024
    bytes, but this can be overridden (see [Block size](#Block-size)).
    The blocks computed counts each hard link separately;
    this is arguably a deficiency.

    The file type is one of the following characters:

    ‘`-`’
    :   regular file

    ‘`b`’
    :   block special file

    ‘`c`’
    :   character special file

    ‘`C`’
    :   high performance (“contiguous data”) file

    ‘`d`’
    :   directory

    ‘`D`’
    :   door (Solaris)

    ‘`l`’
    :   symbolic link

    ‘`M`’
    :   off-line (“migrated”) file (Cray DMF)

    ‘`n`’
    :   network special file (HP-UX)

    ‘`p`’
    :   FIFO (named pipe)

    ‘`P`’
    :   port (Solaris)

    ‘`s`’
    :   socket

    ‘`?`’
    :   some other file type

    The file mode bits listed are similar to symbolic mode specifications
    (see [Symbolic Modes](#Symbolic-Modes)). But `ls` combines multiple bits into the
    third character of each set of permissions as follows:

    ‘`s`’
    :   If the set-user-ID or set-group-ID bit and the corresponding executable bit
        are both set.

    ‘`S`’
    :   If the set-user-ID or set-group-ID bit is set but the corresponding
        executable bit is not set.

    ‘`t`’
    :   If the restricted deletion flag or sticky bit, and the
        other-executable bit, are both set. The restricted deletion flag is
        another name for the sticky bit. See [Structure of File Mode Bits](#Mode-Structure).

    ‘`T`’
    :   If the restricted deletion flag or sticky bit is set but the
        other-executable bit is not set.

    ‘`x`’
    :   If the executable bit is set and none of the above apply.

    ‘`-`’
    :   Otherwise.

    Following the file mode bits is a single character that specifies
    whether an alternate access method such as an access control list
    applies to the file. When the character following the file mode bits is a
    space, there is no alternate access method. When it is a printing
    character, then there is such a method.

    GNU `ls` uses a ‘`.`’ character to indicate a file
    with a security context, but no other alternate access method.

    A file with any other combination of alternate access methods
    is marked with a ‘`+`’ character.

    `ls` uses a ‘`?`’ character to indicate it is unable to determine
    whether alternate access methods apply to the file, which may happen for
    example with some NFS setups with files without read permission.

‘`-n`’ [¶](#index-_002dn-10)

‘`--numeric-uid-gid`’
:   Produce long format, but
    display right-justified numeric user and group IDs
    instead of left-justified owner and group names.

‘`-o`’ [¶](#index-_002do-5)
:   Produce long format, but omit group information.
    It is equivalent to using `--format=long` (`-l`)
    with `--no-group` (`-G`).

‘`-s`’ [¶](#index-_002ds-15)

‘`--size`’
:   Print the file system allocation of each file to the left of the file name.
    This is the amount of file system space used by the file, which is usually a
    bit more than the file’s size, but it can be less if the file has holes.

    Normally the allocation is printed in units of
    1024 bytes, but this can be overridden (see [Block size](#Block-size)).

    For files that are NFS-mounted from an HP-UX system to a BSD system,
    this option reports sizes that are half the correct values. On HP-UX
    systems, it reports sizes that are twice the correct values for files
    that are NFS-mounted from BSD systems. This is due to a flaw in HP-UX;
    it also affects the HP-UX `ls` program.

‘`--si`’ [¶](#index-_002d_002dsi-1)
:   Append an SI-style abbreviation to each size, such as ‘`M`’ for
    megabytes. Powers of 1000 are used, not 1024; ‘`M`’ stands for
    1,000,000 bytes. This option is equivalent to
    `--block-size=si`. Use the `-h` or
    `--human-readable` option if
    you prefer powers of 1024.

‘`-Z`’ [¶](#index-_002dZ)

‘`--context`’
:   Display the SELinux security context or ‘`?`’ if none is found.
    In long format, print the security context to the left of the size column.

---

Next: [General output formatting](#General-output-formatting), Previous: [What information is listed](#What-information-is-listed), Up: [`ls`: List directory contents](#ls-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 10.1.3 Sorting the output [¶](#Sorting-the-output-1)

These options change the order in which `ls` outputs
information for the files in a directory.
(Command-line operands are always processed left to right.)
By default, files are sorted alphabetically by name, using the
character collating sequence specified by the `LC_COLLATE` locale.

‘`-c`’ [¶](#index-_002dc-12)

‘`--time=ctime`’

‘`--time=status`’
:   In long format,
    print the status change timestamp (the ctime) instead of the mtime.
    When sorting by time or when not using long format,
    sort according to the ctime. See [File timestamps](#File-timestamps).

‘`-f`’ [¶](#index-_002df-9)
:   Do not sort, and list all files.
    This is like `--sort=none` (`-U`) combined
    with `--all` (`-a`).

‘`--group-directories-first`’ [¶](#index-_002d_002dgroup_002ddirectories_002dfirst)
:   When listing a directory’s files,
    group all subdirectories before non-directories
    and then sort the subdirectories and the non-directories separately.
    That is, this option specifies a primary sort key,
    and the other sorting options specify a secondary key.
    However, any use of `--sort=none`
    (`-U`) disables this option altogether.

‘`-r`’ [¶](#index-_002dr-6)

‘`--reverse`’
:   Reverse whatever the sorting method is – e.g., list files in reverse
    alphabetical order, youngest first, smallest first, or whatever.
    This option has no effect when `--sort=none` (`-U`)
    is in effect.

‘`-S`’ [¶](#index-_002dS-5)

‘`--sort=size`’
:   Sort by file size, largest first.

‘`-t`’ [¶](#index-_002dt-10)

‘`--sort=time`’
:   Sort by modification timestamp (mtime) by default, newest first.
    The timestamp to order by can be changed with the `--time` option.
    See [File timestamps](#File-timestamps).

‘`-u`’ [¶](#index-_002du-5)

‘`--time=atime`’

‘`--time=access`’

‘`--time=use`’
:   In long format, print the last access timestamp (the atime).
    When sorting by time or when not using long format,
    sort according to the atime.
    See [File timestamps](#File-timestamps).

‘`--time=mtime`’ [¶](#index-_002d_002dtime-2)

‘`--time=modification`’
:   This is the default timestamp display and sorting mode.
    In long format, print the last data modification timestamp (the mtime).
    When sorting by time or when not using long format,
    sort according to the mtime.
    See [File timestamps](#File-timestamps).

‘`--time=birth`’ [¶](#index-_002d_002dtime-3)

‘`--time=creation`’
:   In long format, print the file creation timestamp if available,
    falling back to the file modification timestamp (mtime) if not.
    When sorting by time or when not using long format,
    sort according to the birth time.
    See [File timestamps](#File-timestamps).

‘`-U`’ [¶](#index-_002dU)

‘`--sort=none`’
:   Do not sort; list the files in whatever order they are
    stored in the directory. This can be useful when listing large
    directories, where sorting can take some time.

    Unlike `-f`, this option does not imply `--all`
    (`-a`).

‘`-v`’ [¶](#index-_002dv-6)

‘`--sort=version`’
:   Sort by version name and number, lowest first. It behaves like a default
    sort, except that each sequence of decimal digits is treated numerically
    as an index/version number. See [Version sort ordering](#Version-sort-ordering).

‘`--sort=name`’ [¶](#index-_002d_002dsort-9)
:   Sort by file name. This is the default operation of `ls`,
    and can be explicitly specified to override any
    previously specified sorting option.

‘`--sort=width`’ [¶](#index-_002d_002dsort-10)
:   Sort by printed width of file names.
    This can be useful with the `--format=vertical` (`-C`)
    output format, to most densely display the listed files.

‘`-X`’ [¶](#index-_002dX)

‘`--sort=extension`’
:   Sort directory contents alphabetically by file extension (characters
    after the last ‘`.`’); files with no extension are sorted first.

---

Next: [Formatting file timestamps](#Formatting-file-timestamps), Previous: [Sorting the output](#Sorting-the-output), Up: [`ls`: List directory contents](#ls-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 10.1.4 General output formatting [¶](#General-output-formatting-1)

These options affect the appearance of the overall output.

‘`--format=single-column`’ [¶](#index-_002d_002dformat-2)
:   List one file name per line, with no other information.
    This is the default for `ls` when standard
    output is not a terminal. See also the `--escape` (`-b`),
    `--hide-control-chars` (`-q`), and `--zero` options
    to disambiguate output of file names containing newline characters.

‘`-1`’ [¶](#index-_002d1-2)
:   List one file per line. This is like `--format=single-column`
    except that it has no effect if long format is also in effect.

‘`-C`’ [¶](#index-_002dC-2)

‘`--format=vertical`’
:   List files in columns, sorted vertically, with no other information.
    This is the default for `ls` if standard output is a terminal.
    It is always the default for the `dir` program.
    GNU `ls` uses variable width columns to display as many files as
    possible in the fewest lines.

‘`--color [=when]`’ [¶](#index-_002d_002dcolor)
:   Specify whether to use color for distinguishing file types; when
    may be omitted, or one of:

    * none
      - Do not use color at all. This is the default.
    * auto

      - Only use color if standard output is a terminal.
    * always
      - Always use color.

    Specifying `--color` and no when is equivalent to
    `--color=always`.
    If piping a colored listing through a pager like `less`,
    use the pager’s `-R` option to pass the color codes to the terminal.
    See [`dircolors`: Color setup for `ls`](#dircolors-invocation) to configure the colors used.

    Using the `--color` option may incur a noticeable
    performance penalty when run in a large directory,
    because the default settings require that `ls` `stat` every
    single file it lists.
    However, if you would like most of the file-type coloring
    but can live without the other coloring options (e.g.,
    executable, orphan, sticky, other-writable, capability), use
    `dircolors` to set the `LS_COLORS` environment variable like this,

    ```
    eval $(dircolors -p | perl -pe \
      's/^((CAP|S[ET]|O[TR]|M|E)\w+).*/$1 00/' | dircolors -)
    ```

    and on a `dirent.d_type`-capable file system, `ls`
    will perform only one `stat` call per command line argument.

‘`-F`’ [¶](#index-_002dF-3)

‘`--classify [=when]`’

‘`--indicator-style=classify`’
:   Append a character to each file name indicating the file type. Also,
    for regular files that are executable, append ‘`*`’. The file type
    indicators are ‘`/`’ for directories, ‘`@`’ for symbolic links,
    ‘`|`’ for FIFOs, ‘`=`’ for sockets, ‘`>`’ for doors,
    and nothing for regular files.
    when may be omitted, or one of:

    * none
      - Do not classify. This is the default.
    * auto

      - Only classify if standard output is a terminal.
    * always
      - Always classify.

    Specifying `--classify` and no when is equivalent to
    `--classify=always`.
    Do not follow symbolic links listed on the
    command line unless the `--dereference-command-line` (`-H`),
    `--dereference` (`-L`), or
    `--dereference-command-line-symlink-to-dir` options are specified.

‘`--file-type`’ [¶](#index-_002d_002dfile_002dtype)

‘`--indicator-style=file-type`’
:   Append a character to each file name indicating the file type. This is
    like `--classify` (`-F`, except that executables are not marked.

‘`--hyperlink [=when]`’ [¶](#index-_002d_002dhyperlink)
:   Output codes recognized by some terminals to link
    to files using the ‘`file://`’ URI format.
    when may be omitted, or one of:

    * none
      - Do not use hyperlinks at all. This is the default.
    * auto

      - Only use hyperlinks if standard output is a terminal.
    * always
      - Always use hyperlinks.

    Specifying `--hyperlink` and no when is equivalent to
    `--hyperlink=always`.

‘`--indicator-style=word`’ [¶](#index-_002d_002dindicator_002dstyle-2)
:   Append a character indicator with style word to file names,
    as follows:

    ‘`none`’
    :   Do not append any character indicator; this is the default.

    ‘`slash`’
    :   Append ‘`/`’ for directories. This is the same as the `-p`
        option.

    ‘`file-type`’
    :   Append ‘`/`’ for directories, ‘`@`’ for symbolic links, ‘`|`’
        for FIFOs, ‘`=`’ for sockets, and nothing for regular files. This is
        the same as the `--file-type` option.

    ‘`classify`’
    :   Append ‘`*`’ for executable regular files, otherwise behave as for
        ‘`file-type`’. This is the same as the `--classify`
        (`-F`) option.

‘`-k`’ [¶](#index-_002dk-3)

‘`--kibibytes`’
:   Set the default block size to its normal value of 1024 bytes,
    overriding any contrary specification in environment variables
    (see [Block size](#Block-size)). If `--block-size`,
    `--human-readable` (`-h`), or `--si` options are used,
    they take precedence even if `--kibibytes` (`-k`) is placed after
    the other options.

    The `--kibibytes` (`-k`) option affects the
    per-directory block count written in long format,
    and the file system allocation written by the `--size` (`-s`)
    option. It does not affect the file size in bytes that is written in
    long format.

‘`-m`’ [¶](#index-_002dm-3)

‘`--format=commas`’
:   List files horizontally, with as many as will fit on each line,
    separated by ‘`,` ’ (a comma and a space),
    and with no other information.

‘`-p`’ [¶](#index-_002dp-1)

‘`--indicator-style=slash`’
:   Append a ‘`/`’ to directory names.

‘`-x`’ [¶](#index-_002dx-2)

‘`--format=across`’

‘`--format=horizontal`’
:   List the files in columns, sorted horizontally.

‘`-T cols`’ [¶](#index-_002dT-4)

‘`--tabsize=cols`’
:   Assume that each tab stop is cols columns wide. The default is 8.
    `ls` uses tabs where possible in the output, for efficiency. If
    cols is zero, do not use tabs at all.

    Some terminal emulators might not properly align columns to the right of a
    TAB following a non-ASCII byte. You can avoid that issue by using the
    `-T0` option or put `TABSIZE=0` in your environment, to tell
    `ls` to align using spaces, not tabs.

    If you set a terminal’s hardware tabs to anything other than the default,
    you should also use a `--tabsize` option or `TABSIZE`
    environment variable either to match the hardware tabs, or to disable
    the use of hardware tabs. Otherwise, the output of `ls` may
    not line up. For example, if you run the shell command ‘`tabs -4`’
    to set hardware tabs to every four columns, you should also run
    ‘`export TABSIZE=4`’ or ‘`export TABSIZE=0`’, or use the
    corresponding `--tabsize` options.

‘`-w cols`’ [¶](#index-_002dw-10)

‘`--width=cols`’
:   Assume the screen is cols columns wide. The default is taken
    from the terminal settings if possible; otherwise the environment
    variable `COLUMNS` is used if it is set; otherwise the default
    is 80. With a cols value of ‘`0`’, there is no limit on
    the length of the output line, and that single output line will
    be delimited with spaces, not tabs.

‘`--zero`’ [¶](#index-_002d_002dzero-1)
:   Output a zero byte (ASCII NUL) at the end of each line,
    rather than a newline. This option enables other programs to parse the
    output even when that output would contain data with embedded newlines.
    This option is incompatible with the `--dired` (`-D`) option.
    This option also implies the options `--show-control-chars`,
    `-1`, `--color=none`, and
    `--quoting-style=literal` (`-N`).

---

Next: [Formatting the file names](#Formatting-the-file-names), Previous: [General output formatting](#General-output-formatting), Up: [`ls`: List directory contents](#ls-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 10.1.5 Formatting file timestamps [¶](#Formatting-file-timestamps-1)

By default, file timestamps are listed in abbreviated form, using
a date like ‘`Mar 30  2020`’ for non-recent timestamps, and a
date-without-year and time like ‘`Mar 30 23:45`’ for recent timestamps.
This format can change depending on the current locale as detailed below.

A timestamp is considered to be *recent* if it is less than six
months old, and is not dated in the future. If a timestamp dated
today is not listed in recent form, the timestamp is in the future,
which means you probably have clock skew problems which may break
programs like `make` that rely on file timestamps.
See [File timestamps](#File-timestamps).

Timestamps are listed according to the time zone rules specified by
the `TZ` environment variable, or by the system default rules if
`TZ` is not set. See [Specifying the Time Zone
with `TZ`](https://www.gnu.org/software/libc/manual/html_mono/libc.html#TZ-Variable) in The GNU C Library Reference Manual.

The following option changes how file timestamps are printed.

‘`--time-style=style`’ [¶](#index-_002d_002dtime_002dstyle)
:   List timestamps in style style. The style should
    be one of the following:

    ‘`+format`’ [¶](#index-LC_005fTIME-2)
    :   List timestamps using format, where format is interpreted
        like the format argument of `date` (see [`date`: Print or set system date and time](#date-invocation)).
        For example, `--time-style="+%Y-%m-%d %H:%M:%S"` causes
        `ls` to list timestamps like ‘`2020-03-30 23:45:56`’. As
        with `date`, format’s interpretation is affected by the
        `LC_TIME` locale category.

        If format contains two format strings separated by a newline,
        the former is used for non-recent files and the latter for recent
        files; if you want output columns to line up, you may need to insert
        spaces in one of the two formats.

    ‘`full-iso`’
    :   List timestamps in full using ISO 8601-like date, time, and time zone
        components with nanosecond precision, e.g., ‘`2020-07-21
        23:45:56.477817180 -0400`’. This style is equivalent to
        ‘`+%Y-%m-%d %H:%M:%S.%N %z`’.

        This is useful because the time output includes all the information that
        is available from the operating system. For example, this can help
        explain `make`’s behavior, since GNU `make`
        uses the full timestamp to determine whether a file is out of date.

    ‘`long-iso`’
    :   List ISO 8601 date and time components with minute precision, e.g.,
        ‘`2020-03-30 23:45`’. These timestamps are shorter than
        ‘`full-iso`’ timestamps, and are usually good enough for everyday
        work. This style is equivalent to ‘`+%Y-%m-%d %H:%M`’.

    ‘`iso`’
    :   List ISO 8601 dates for non-recent timestamps (e.g.,
        ‘`2020-03-30`’), and ISO 8601-like month, day, hour, and
        minute for recent timestamps (e.g., ‘`03-30 23:45`’). These
        timestamps are uglier than ‘`long-iso`’ timestamps, but they carry
        nearly the same information in a smaller space and their brevity helps
        `ls` output fit within traditional 80-column output lines.
        The following two `ls` invocations are equivalent:

        ```
        newline='
        '
        ls -l --time-style="+%Y-%m-%d $newline%m-%d %H:%M"
        ls -l --time-style="iso"
        ```

    ‘`locale`’ [¶](#index-LC_005fTIME-3)
    :   List timestamps in a locale-dependent form. For example, a French
        locale might list non-recent timestamps like ‘`30 mars   2020`’
        and recent timestamps like ‘`30 mars  23:45`’. Locale-dependent
        timestamps typically consume more space than ‘`iso`’ timestamps and
        are harder for programs to parse because locale conventions vary so
        widely, but they are easier for many people to read.

        The `LC_TIME` locale category specifies the timestamp format. The
        default POSIX locale uses timestamps like ‘`Mar 30  2020`’ and ‘`Mar 30 23:45`’; in this locale, the following two
        `ls` invocations are equivalent:

        ```
        newline='
        '
        ls -l --time-style="+%b %e  %Y$newline%b %e %H:%M"
        ls -l --time-style="locale"
        ```

        Other locales behave differently. For example, in a German locale,
        `--time-style="locale"` might be equivalent to
        `--time-style="+%e. %b %Y $newline%e. %b %H:%M"`
        and might generate timestamps like ‘`30. Mär 2020`’ and
        ‘`30. Mär 23:45`’.

    ‘`posix-style`’ [¶](#index-LC_005fTIME-4)
    :   List POSIX-locale timestamps if the `LC_TIME` locale
        category is POSIX, style timestamps otherwise. For
        example, the ‘`posix-long-iso`’ style lists
        timestamps like ‘`Mar 30  2020`’ and ‘`Mar 30 23:45`’ when in
        the POSIX locale, and like ‘`2020-03-30 23:45`’ otherwise.

You can specify the default value of the `--time-style` option
with the environment variable `TIME_STYLE`; if `TIME_STYLE` is not set
the default style is ‘`locale`’. GNU Emacs 21.3 and
later use the `--dired` option and therefore can parse any date
format, but if you are using Emacs 21.1 or 21.2 and specify a
non-POSIX locale you may need to set
‘`TIME_STYLE="posix-long-iso"`’.

To avoid certain denial-of-service attacks, timestamps that would be
longer than 1000 bytes may be treated as errors.

---

Previous: [Formatting file timestamps](#Formatting-file-timestamps), Up: [`ls`: List directory contents](#ls-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 10.1.6 Formatting the file names [¶](#Formatting-the-file-names-1)

These options change how file names themselves are printed.

‘`-b`’ [¶](#index-_002db-12)

‘`--escape`’

‘`--quoting-style=escape`’
:   Quote nongraphic characters in file names using alphabetic and octal
    backslash sequences like those used in C.

‘`-N`’ [¶](#index-_002dN-2)

‘`--literal`’

‘`--quoting-style=literal`’
:   Do not quote file names. However, with `ls` nongraphic
    characters are still printed as question marks if the output is a
    terminal and you do not specify the `--show-control-chars`
    option.

‘`-q`’ [¶](#index-_002dq-3)

‘`--hide-control-chars`’
:   Print question marks instead of nongraphic characters in file names.
    This is the default if the output is a terminal and the program is
    `ls`.

‘`-Q`’ [¶](#index-_002dQ)

‘`--quote-name`’

‘`--quoting-style=c`’
:   Enclose file names in double quotes and quote nongraphic characters as
    in C.

‘`--quoting-style=word`’ [¶](#index-_002d_002dquoting_002dstyle-3)
:   Use style word to quote file names and other strings that may
    contain arbitrary characters. The word should
    be one of the following:

    ‘`literal`’
    :   Output strings as-is; this is the same as the `--literal` (`-N`)
        option.

    ‘`shell`’
    :   Quote strings for the shell if they contain shell metacharacters or would
        cause ambiguous output.
        The quoting is suitable for POSIX-compatible shells like
        `bash`, but it does not always work for incompatible shells
        like `csh`.

    ‘`shell-always`’
    :   Quote strings for the shell, even if they would normally not require quoting.

    ‘`shell-escape`’
    :   Like ‘`shell`’, but also quoting non-printable characters using the POSIX
        ‘`$''`’ syntax suitable for most shells.

    ‘`shell-escape-always`’
    :   Like ‘`shell-escape`’, but quote strings even if they would
        normally not require quoting.

    ‘`c`’
    :   Quote strings as for C character string literals, including the
        surrounding double-quote characters; this is the same as the
        `--quote-name` (`-Q`) option.

    ‘`c-maybe`’
    :   Quote strings as for C character string literals, except omit the
        surrounding double-quote if no escaping is required.

    ‘`escape`’
    :   Quote strings as for C character string literals, except omit the
        surrounding double-quote
        characters; this is the same as the `--escape` (`-b`) option.

    ‘`clocale`’
    :   Quote strings as for C character string literals, except use
        surrounding quotation marks appropriate for the
        locale.

    ‘`locale`’
    :   Quote strings as for C character string literals, except use
        surrounding quotation marks appropriate for the locale, and quote
        `'like this'` instead of `"like
        this"` in the default C locale. This looks nicer on many displays.

    You can specify the default value of the `--quoting-style` option
    with the environment variable `QUOTING_STYLE`. If that environment
    variable is not set, the default value is ‘`shell-escape`’ when the
    output is a terminal, and ‘`literal`’ otherwise.

‘`--show-control-chars`’ [¶](#index-_002d_002dshow_002dcontrol_002dchars-1)
:   Print nongraphic characters as-is in file names.
    This is the default unless the output is a terminal and the program is
    `ls`.

---

Next: [`vdir`: Verbosely list directory contents](#vdir-invocation), Previous: [`ls`: List directory contents](#ls-invocation), Up: [Directory listing](#Directory-listing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 10.2 `dir`: Briefly list directory contents [¶](#dir_003a-Briefly-list-directory-contents)

`dir` is equivalent to `ls -C
-b`; that is, by default files are listed in columns, sorted vertically,
and special characters are represented by backslash escape sequences.

See [`ls`](#ls-invocation).

---

Next: [`dircolors`: Color setup for `ls`](#dircolors-invocation), Previous: [`dir`: Briefly list directory contents](#dir-invocation), Up: [Directory listing](#Directory-listing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 10.3 `vdir`: Verbosely list directory contents [¶](#vdir_003a-Verbosely-list-directory-contents)

`vdir` is equivalent to `ls -l
-b`; that is, by default files are listed in long format and special
characters are represented by backslash escape sequences.

See [`ls`](#ls-invocation).

---

Previous: [`vdir`: Verbosely list directory contents](#vdir-invocation), Up: [Directory listing](#Directory-listing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 10.4 `dircolors`: Color setup for `ls` [¶](#dircolors_003a-Color-setup-for-ls)

`dircolors` outputs a sequence of shell commands to set up the
terminal for color output from `ls` (and `dir`, etc.).
Typical usage:

```
eval "$(dircolors [option]... [file])"
```

If file is specified, `dircolors` reads it to determine which
colors to use for which file types and extensions. Otherwise, a
precompiled database is used. For details on the format of these files,
run ‘`dircolors --print-database`’.

To make `dircolors` read a `~/.dircolors` file if it
exists, you can put the following lines in your `~/.bashrc` (or
adapt them to your favorite shell):

```
d=.dircolors
test -r $d && eval "$(dircolors $d)"
```

The output is a shell command to set the `LS_COLORS` environment
variable. You can specify the shell syntax to use on the command line,
or `dircolors` will guess it from the value of the `SHELL`
environment variable.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-b`’ [¶](#index-_002db-13)

‘`--sh`’

‘`--bourne-shell`’
:   Output Bourne shell commands. This is the default if the `SHELL`
    environment variable is set and does not end with ‘`csh`’ or
    ‘`tcsh`’.

‘`-c`’ [¶](#index-_002dc-13)

‘`--csh`’

‘`--c-shell`’
:   Output C shell commands. This is the default if `SHELL` ends with
    `csh` or `tcsh`.

‘`-p`’ [¶](#index-_002dp-2)

‘`--print-database`’
:   Print the (compiled-in) default color configuration database. This
    output is itself a valid configuration file, and is fairly descriptive
    of the possibilities.

‘`--print-ls-colors`’ [¶](#index-_002d_002dprint_002dls_002dcolors)
:   Print the LS\_COLORS entries on separate lines,
    each colored as per the color they represent.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [Special file types](#Special-file-types), Previous: [Directory listing](#Directory-listing), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 11 Basic operations [¶](#Basic-operations-1)

This chapter describes the commands for basic file manipulation:
copying, moving (renaming), and deleting (removing).

* [`cp`: Copy files and directories](#cp-invocation)
* [`dd`: Convert and copy a file](#dd-invocation)
* [`install`: Copy files and set attributes](#install-invocation)
* [`mv`: Move (rename) files](#mv-invocation)
* [`rm`: Remove files or directories](#rm-invocation)
* [`shred`: Remove files more securely](#shred-invocation)

---

Next: [`dd`: Convert and copy a file](#dd-invocation), Up: [Basic operations](#Basic-operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]
