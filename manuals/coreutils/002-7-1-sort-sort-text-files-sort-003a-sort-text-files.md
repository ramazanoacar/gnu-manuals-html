### 7.1 `sort`: Sort text files [¶](#sort_003a-Sort-text-files)

`sort` sorts, merges, or compares all the lines from the given
files, or standard input if none are given or for a file of
‘`-`’. By default, `sort` writes the results to standard
output. Synopsis:

```
sort [option]... [file]...
```

Many options affect how `sort` compares lines; if the results
are unexpected, try the `--debug` option to see what happened.
A pair of lines is compared as follows:
`sort` compares each pair of fields (see `--key`), in the
order specified on the command line, according to the associated
ordering options, until a difference is found or no fields are left.
If no key fields are specified, `sort` uses a default key of
the entire line. Finally, as a last resort when all keys compare
equal, `sort` compares entire lines as if no ordering options
other than `--reverse` (`-r`) were specified. The
`--stable` (`-s`) option disables this *last-resort
comparison* so that lines in which all fields compare equal are left
in their original relative order. The `--unique`
(`-u`) option also disables the last-resort comparison.

Unless otherwise specified, all comparisons use the character collating
sequence specified by the `LC_COLLATE` locale.[1](#FOOT1)
A line’s trailing newline is not part of the line for comparison
purposes. If the final byte of an input file is not a newline, GNU
`sort` silently supplies one. GNU `sort` (as
specified for all GNU utilities) has no limit on input line length or
restrictions on bytes allowed within lines.

`sort` has three modes of operation: sort (the default), merge,
and check for order. The following options change the operation
mode:

‘`-c`’ [¶](#index-_002dc-6)

‘`--check`’

‘`--check=diagnose-first`’
:   Check whether the given file is already sorted: if it is not all
    sorted, print a diagnostic containing the first out-of-order line and
    exit with a status of 1.
    Otherwise, exit successfully.
    At most one input file can be given.

‘`-C`’ [¶](#index-_002dc-7)

‘`--check=quiet`’

‘`--check=silent`’
:   Exit successfully if the given file is already sorted, and
    exit with status 1 otherwise.
    At most one input file can be given.
    This is like `-c`, except it does not print a diagnostic.

‘`-m`’ [¶](#index-_002dm-2)

‘`--merge`’
:   Merge the given files by sorting them as a group. Each input file must
    always be individually sorted. It always works to sort instead of
    merge; merging is provided because it is faster, in the case where it
    works.

Exit status:

```
0 if no error occurred
1 if invoked with -c or -C and the input is not sorted
2 if an error occurred
```

If the environment variable `TMPDIR` is set, `sort` uses its
value as the directory for temporary files instead of `/tmp`. The
`--temporary-directory` (`-T`) option in turn overrides
the environment variable.

The following options affect the ordering of output lines. They may be
specified globally or as part of a specific key field. If no key
fields are specified, global options apply to comparison of entire
lines; otherwise the global options are inherited by key fields that do
not specify any special options of their own. In pre-POSIX
versions of `sort`, global options affect only later key fields,
so portable shell scripts should specify global options first.

‘`-b`’ [¶](#index-_002db-9)

‘`--ignore-leading-blanks`’
:   Ignore leading blanks when finding sort keys in each line.
    By default a blank is a space or a tab, but the `LC_CTYPE` locale
    can change this. Blanks may be ignored by your locale’s collating
    rules, but without this option they will be significant for character
    positions specified in keys with the `-k` option.

‘`-d`’ [¶](#index-_002dd-5)

‘`--dictionary-order`’
:   Sort in *phone directory* order: ignore all characters except
    letters, digits and blanks when sorting.
    By default letters and digits are those of ASCII and a blank
    is a space or a tab, but the `LC_CTYPE` locale can change this.

‘`-f`’ [¶](#index-_002df-5)

‘`--ignore-case`’
:   Fold lowercase characters into the equivalent uppercase characters when
    comparing so that, for example, ‘`b`’ and ‘`B`’ sort as equal.
    The `LC_CTYPE` locale determines character types.
    When used with `--unique` those lower case equivalent lines are
    thrown away. (There is currently no way to throw away the upper case
    equivalent instead. (Any `--reverse` given would only affect
    the final result, after the throwing away.))

‘`-g`’ [¶](#index-_002dg-1)

‘`--general-numeric-sort`’

‘`--sort=general-numeric`’
:   Sort numerically, converting a prefix of each line to a long
    double-precision floating point number. See [Floating point numbers](#Floating-point).
    Do not report overflow, underflow, or conversion errors.
    Use the following collating sequence:

    * Lines that do not start with numbers (all considered to be equal).
    * NaNs (“Not a Number” values, in IEEE floating point arithmetic)
      in a consistent but machine-dependent order.
    * Minus infinity.
    * Finite numbers in ascending numeric order (with *-0* and *+0* equal).
    * Plus infinity.

    Use this option only if there is no alternative; it is much slower than
    `--numeric-sort` (`-n`) and it can lose information when
    converting to floating point.

    You can use this option to sort hexadecimal numbers prefixed with
    ‘`0x`’ or ‘`0X`’, where those numbers are not fixed width,
    or of varying case. However for hex numbers of consistent case,
    and left padded with ‘`0`’ to a consistent width, a standard
    lexicographic sort will be faster.

‘`-h`’ [¶](#index-_002dh-3)

‘`--human-numeric-sort`’

‘`--sort=human-numeric`’
:   Sort numerically, first by numeric sign (negative, zero, or positive);
    then by SI suffix (either empty, or ‘`k`’ or ‘`K`’, or
    one of ‘`MGTPEZYRQ`’, in that order; see [Block size](#Block-size)); and finally
    by numeric value. For example, ‘`1023M`’ sorts before ‘`1G`’
    because ‘`M`’ (mega) precedes ‘`G`’ (giga) as an SI
    suffix. This option sorts values that are consistently scaled to the
    nearest suffix, regardless of whether suffixes denote powers of 1000
    or 1024, and it therefore sorts the output of any single invocation of
    the `df`, `du`, or `ls` commands that are
    invoked with their `--human-readable` or `--si` options.
    The syntax for numbers is the same as for the `--numeric-sort`
    option; the SI suffix must immediately follow the number.
    To sort more accurately, you can use the `numfmt` command
    to reformat numbers to human format *after* the sort.

‘`-i`’ [¶](#index-_002di-4)

‘`--ignore-nonprinting`’
:   Ignore nonprinting characters.
    The `LC_CTYPE` locale determines character types.
    This option has no effect if the stronger `--dictionary-order`
    (`-d`) option is also given.

‘`-M`’ [¶](#index-_002dM)

‘`--month-sort`’

‘`--sort=month`’
:   An initial string, consisting of any amount of blanks, followed
    by a month name abbreviation, is folded to UPPER case and
    compared in the order ‘`JAN`’ < ‘`FEB`’ < … < ‘`DEC`’.
    Invalid names compare low to valid names. The `LC_TIME` locale
    category determines the month spellings.
    By default a blank is a space or a tab, but the `LC_CTYPE` locale
    can change this.

‘`-n`’ [¶](#index-_002dn-7)

‘`--numeric-sort`’

‘`--sort=numeric`’
:   Sort numerically. The number begins each line and consists
    of optional blanks, an optional ‘`-`’ sign, and zero or more
    digits possibly separated by thousands separators, optionally followed
    by a decimal-point character and zero or more digits. An empty
    number is treated as ‘`0`’. Signs on zeros and leading zeros do
    not affect ordering.

    Comparison is exact; there is no rounding error.

    The `LC_CTYPE` locale specifies which characters are blanks and
    the `LC_NUMERIC` locale specifies the thousands separator and
    decimal-point character. In the C locale, spaces and tabs are blanks,
    there is no thousands separator, and ‘`.`’ is the decimal point.

    Neither a leading ‘`+`’ nor exponential notation is recognized.
    To compare such strings numerically, use the
    `--general-numeric-sort` (`-g`) option.

‘`-V`’ [¶](#index-_002dV)

‘`--version-sort`’
:   Sort by version name and number. It behaves like a standard sort,
    except that each sequence of decimal digits is treated numerically
    as an index/version number. (See [Version sort ordering](#Version-sort-ordering).)

‘`-r`’ [¶](#index-_002dr-3)

‘`--reverse`’
:   Reverse the result of comparison, so that lines with greater key values
    appear earlier in the output instead of later.

‘`-R`’ [¶](#index-_002dR)

‘`--random-sort`’

‘`--sort=random`’
:   Sort by hashing the input keys and then sorting the hash values.
    Choose the hash function at random, ensuring that it is free of
    collisions so that differing keys have differing hash values. This is
    like a random permutation of the inputs (see [`shuf`: Shuffling text](#shuf-invocation)),
    except that keys with the same value sort together.

    If multiple random sort fields are specified, the same random hash
    function is used for all fields. To use different random hash
    functions for different fields, you can invoke `sort` more
    than once.

    The choice of hash function is affected by the
    `--random-source` option.

Other options are:

‘`--compress-program=prog`’
:   Compress any temporary files with the program prog.

    With no arguments, prog must compress standard input to standard
    output, and when given the `-d` option it must decompress
    standard input to standard output.

    Terminate with an error if prog exits with nonzero status.

    White space and the backslash character should not appear in
    prog; they are reserved for future use.

‘`--files0-from=file`’ [¶](#index-_002d_002dfiles0_002dfrom_003dfile-1)
:   Disallow processing files named on the command line, and instead process
    those named in file file; each name being terminated by a zero byte
    (ASCII NUL).
    This is useful
    when the list of file names is so long that it may exceed a command line
    length limitation.
    In such cases, running `sort` via `xargs` is undesirable
    because it splits the list into pieces and makes `sort` print
    sorted output for each sublist rather than for the entire list.
    One way to produce a list of ASCII NUL terminated file
    names is with GNU
    `find`, using its `-print0` predicate.
    If file is ‘`-`’ then the ASCII NUL terminated
    file names are read from standard input.

‘`-k pos1[,pos2]`’ [¶](#index-_002dk-2)

‘`--key=pos1[,pos2]`’
:   Specify a sort field that consists of the part of the line between
    pos1 and pos2 (or the end of the line, if pos2 is
    omitted), *inclusive*.

    In its simplest form pos specifies a field number (starting with 1),
    with fields being separated by runs of blank characters, and by default
    those blanks being included in the comparison at the start of each field.
    To adjust the handling of blank characters see the `-b` and
    `-t` options.

    More generally,
    each pos has the form ‘`f[.c][opts]`’,
    where f is the number of the field to use, and c is the number
    of the first character from the beginning of the field. Fields and character
    positions are numbered starting with 1; a character position of zero in
    pos2 indicates the field’s last character. If ‘`.c`’ is
    omitted from pos1, it defaults to 1 (the beginning of the field);
    if omitted from pos2, it defaults to 0 (the end of the field).
    opts are ordering options, allowing individual keys to be sorted
    according to different rules; see below for details. Keys can span
    multiple fields.

    Example: To sort on the second field, use `--key=2,2`
    (`-k 2,2`). See below for more notes on keys and more examples.
    See also the `--debug` option to help determine the part
    of the line being used in the sort.

‘`--debug`’
:   Highlight the portion of each line used for sorting.
    Also issue warnings about questionable usage to standard error.

‘`--batch-size=nmerge`’ [¶](#index-_002d_002dbatch_002dsize)
:   Merge at most nmerge inputs at once.

    When `sort` has to merge more than nmerge inputs,
    it merges them in groups of nmerge, saving the result in
    a temporary file, which is then used as an input in a subsequent merge.

    A large value of nmerge may improve merge performance and decrease
    temporary storage utilization at the expense of increased memory usage
    and I/O. Conversely a small value of nmerge may reduce memory
    requirements and I/O at the expense of temporary storage consumption and
    merge performance.

    The value of nmerge must be at least 2. The default value is
    currently 16, but this is implementation-dependent and may change in
    the future.

    The value of nmerge may be bounded by a resource limit for open
    file descriptors. The commands ‘`ulimit -n`’ or ‘`getconf
    OPEN_MAX`’ may display limits for your systems; these limits may be
    modified further if your program already has some files open, or if
    the operating system has other limits on the number of open files. If
    the value of nmerge exceeds the resource limit, `sort`
    silently uses a smaller value.

‘`-o output-file`’ [¶](#index-_002do-2)

‘`--output=output-file`’
:   Write output to output-file instead of standard output.
    Normally, `sort` reads all input before opening
    output-file, so you can sort a file in place by using
    commands like `sort -o F F` and `cat F | sort -o F`.
    However, it is often safer to output to an otherwise-unused file, as
    data may be lost if the system crashes or `sort` encounters
    an I/O or other serious error while a file is being sorted in place.
    Also, `sort` with `--merge` (`-m`) can open
    the output file before reading all input, so a command like `cat
    F | sort -m -o F - G` is not safe as `sort` might start
    writing `F` before `cat` is done reading it.

    On newer systems, `-o` cannot appear after an input file if
    `POSIXLY_CORRECT` is set, e.g., ‘`sort F -o F`’. Portable
    scripts should specify `-o output-file` before any input
    files.

‘`--random-source=file`’ [¶](#index-_002d_002drandom_002dsource)
:   Use file as a source of random data used to determine which
    random hash function to use with the `-R` option. See [Sources of random data](#Random-sources).

‘`-s`’ [¶](#index-_002ds-10)

‘`--stable`’
:   Make `sort` stable by disabling its last-resort comparison.
    This option has no effect if no fields or global ordering options
    other than `--reverse` (`-r`) are specified.

‘`-S size`’ [¶](#index-_002dS-3)

‘`--buffer-size=size`’
:   Use a main-memory sort buffer of the given size. By default,
    size is in units of 1024 bytes. Appending ‘`%`’ causes
    size to be interpreted as a percentage of physical memory.
    Appending ‘`K`’ multiplies size by 1024 (the default),
    ‘`M`’ by 1,048,576, ‘`G`’ by 1,073,741,824, and so on for
    ‘`T`’, ‘`P`’, ‘`E`’, ‘`Z`’, ‘`Y`’, ‘`R`’, and ‘`Q`’.
    Appending
    ‘`b`’ causes size to be interpreted as a byte count, with no
    multiplication.

    This option can improve the performance of `sort` by causing it
    to start with a larger or smaller sort buffer than the default.
    However, this option affects only the initial buffer size. The buffer
    grows beyond size if `sort` encounters input lines larger
    than size.

‘`-t separator`’ [¶](#index-_002dt-6)

‘`--field-separator=separator`’
:   Use character separator as the field separator when finding the
    sort keys in each line. By default, fields are separated by the empty
    string between a non-blank character and a blank character.
    By default a blank is a space or a tab, but the `LC_CTYPE` locale
    can change this.

    That is, given the input line ‘`foo bar`’, `sort` breaks it
    into fields ‘`foo`’ and ‘`bar`’. The field separator is
    not considered to be part of either the field preceding or the field
    following, so with ‘`sort -t " "`’ the same input line has
    three fields: an empty field, ‘`foo`’, and ‘`bar`’.
    However, fields that extend to the end of the line,
    as `-k 2`, or fields consisting of a range, as `-k 2,3`,
    retain the field separators present between the endpoints of the range.

    To specify ASCII NUL as the field separator,
    use the two-character string ‘`\0`’, e.g., ‘`sort -t '\0'`’.

‘`-T tempdir`’ [¶](#index-_002dT-2)

‘`--temporary-directory=tempdir`’
:   Use directory tempdir to store temporary files, overriding the
    `TMPDIR` environment variable. If this option is given more than
    once, temporary files are stored in all the directories given. If you
    have a large sort or merge that is I/O-bound, you can often improve
    performance by using this option to specify directories on different
    file systems.

‘`--parallel=n`’ [¶](#index-_002d_002dparallel)
:   Set the number of sorts run in parallel to n. By default,
    n is set to the number of available processors, but limited
    to 8, as performance gains diminish after that.
    Using n threads increases the memory usage by
    a factor of log n. Also see [`nproc`: Print the number of available processors](#nproc-invocation).

‘`-u`’ [¶](#index-_002du-3)

‘`--unique`’
:   Normally, output only the first of a sequence of lines that compare
    equal. For the `--check` (`-c` or `-C`) option,
    check that no pair of consecutive lines compares equal.

    This option also disables the default last-resort comparison.

    The commands `sort -u` and `sort | uniq` are equivalent, but
    this equivalence does not extend to arbitrary `sort` options.
    For example, `sort -n -u` inspects only the value of the initial
    numeric string when checking for uniqueness, whereas `sort -n |
    uniq` inspects the entire line. See [`uniq`: Uniquify files](#uniq-invocation).

‘`-z`’ [¶](#index-_002dz-4)

‘`--zero-terminated`’
:   Delimit items with a zero byte rather than a newline (ASCII LF).
    I.e., treat input as items separated by ASCII NUL
    and terminate output items with ASCII NUL.
    This option can be useful in conjunction with ‘`perl -0`’ or
    ‘`find -print0`’ and ‘`xargs -0`’ which do the same in order to
    reliably handle arbitrary file names (even those containing blanks
    or other special characters).

Historical (BSD and System V) implementations of `sort` have
differed in their interpretation of some options, particularly
`-b`, `-f`, and `-n`.
GNU sort follows the POSIX
behavior, which is usually (but not always!) like the System V behavior.
According to POSIX, `-n` no longer implies `-b`. For
consistency, `-M` has been changed in the same way. This may
affect the meaning of character positions in field specifications in
obscure cases. The only fix is to add an explicit `-b`.

A position in a sort field specified with `-k` may have any
of the option letters ‘`MbdfghinRrV`’ appended to it, in which case no
global ordering options are inherited by that particular field. The
`-b` option may be independently attached to either or both of
the start and end positions of a field specification, and if it is
inherited from the global options it will be attached to both.
If input lines can contain leading or adjacent blanks and `-t`
is not used, then `-k` is typically combined with `-b` or
an option that implicitly ignores leading blanks (‘`Mghn`’) as otherwise
the varying numbers of leading blanks in fields can cause confusing results.

If the start position in a sort field specifier falls after the end of
the line or after the end field, the field is empty. If the `-b`
option was specified, the ‘`.c`’ part of a field specification
is counted from the first nonblank character of the field.

On systems not conforming to POSIX 1003.1-2001,
`sort` supports a traditional origin-zero
syntax ‘`+pos1 [-pos2]`’ for specifying sort keys.
The traditional command ‘`sort +a.x -b.y`’
is equivalent to ‘`sort -k a+1.x+1,b`’ if y
is ‘`0`’ or absent, otherwise it is equivalent to ‘`sort -k
a+1.x+1,b+1.y`’.

This traditional behavior can be controlled with the
`_POSIX2_VERSION` environment variable (see [Standards conformance](#Standards-conformance)); it can also be enabled when `POSIXLY_CORRECT` is
not set by using the traditional syntax with ‘`-pos2`’ present.

Scripts intended for use on standard hosts should avoid traditional
syntax and should use `-k` instead. For example, avoid
‘`sort +2`’, since it might be interpreted as either ‘`sort
./+2`’ or ‘`sort -k 3`’. If your script must also run on hosts that
support only the traditional syntax, it can use a test like ‘`if sort
-k 1 </dev/null >/dev/null 2>&1; then …`’ to decide which syntax
to use.

Here are some examples to illustrate various combinations of options.

* Sort in descending (reverse) numeric order.

  ```
  sort -n -r
  ```
* Run no more than 4 sorts concurrently, using a buffer size of 10M.

  ```
  sort --parallel=4 -S 10M
  ```
* Sort alphabetically, omitting the first and second fields
  and the blanks at the start of the third field.
  This uses a single key composed of the characters beginning
  at the start of the first nonblank character in field three
  and extending to the end of each line.

  ```
  sort -k 3b
  ```
* Sort numerically on the second field and resolve ties by sorting
  alphabetically on the third and fourth characters of field five.
  Use ‘`:`’ as the field delimiter.

  ```
  sort -t : -k 2,2n -k 5.3,5.4
  ```

  If you had written `-k 2n` instead of `-k 2,2n`
  `sort` would have used all characters beginning in the second field
  and extending to the end of the line as the primary *numeric*
  key. For the large majority of applications, treating keys spanning
  more than one field as numeric will not do what you expect.

  Also, the ‘`n`’ modifier was applied to the field-end
  specifier for the first key. It would have been equivalent to
  specify `-k 2n,2` or `-k 2n,2n`. All modifiers except
  ‘`b`’ apply to the associated *field*, regardless of whether
  the modifier character is attached to the field-start and/or the
  field-end part of the key specifier.
* Sort the password file on the fifth field and ignore any
  leading blanks. Sort lines with equal values in field five
  on the numeric user ID in field three. Fields are separated
  by ‘`:`’.

  ```
  sort -t : -k 5b,5 -k 3,3n /etc/passwd
  sort -t : -n -k 5b,5 -k 3,3 /etc/passwd
  sort -t : -b -k 5,5 -k 3,3n /etc/passwd
  ```

  These three commands have equivalent effect. The first specifies that
  the first key’s start position ignores leading blanks and the second
  key is sorted numerically. The other two commands rely on global
  options being inherited by sort keys that lack modifiers. The inheritance
  works in this case because `-k 5b,5b` and `-k 5b,5` are
  equivalent, as the location of a field-end lacking a ‘`.c`’
  character position is not affected by whether initial blanks are
  skipped.
* Sort a set of log files, primarily by IPv4 address and secondarily by
  timestamp. If two lines’ primary and secondary keys are identical,
  output the lines in the same order that they were input. The log
  files contain lines that look like this:

  ```
  4.150.156.3 - - [01/Apr/2020:06:31:51 +0000] message 1
  211.24.3.231 - - [24/Apr/2020:20:17:39 +0000] message 2
  ```

  Fields are separated by exactly one space. Sort IPv4 addresses
  lexicographically, e.g., 212.61.52.2 sorts before 212.129.233.201
  because 61 is less than 129.

  ```
  sort -s -t ' ' -k 4.9n -k 4.5M -k 4.2n -k 4.14,4.21 file*.log |
  sort -s -t '.' -k 1,1n -k 2,2n -k 3,3n -k 4,4n
  ```

  This example cannot be done with a single POSIX `sort` invocation,
  since IPv4 address components are separated by ‘`.`’ while dates
  come just after a space. So it is broken down into two invocations of
  `sort`: the first sorts by timestamp and the second by IPv4
  address. The timestamp is sorted by year, then month, then day, and
  finally by hour-minute-second field, using `-k` to isolate each
  field. Except for hour-minute-second there’s no need to specify the
  end of each key field, since the ‘`n`’ and ‘`M`’ modifiers sort
  based on leading prefixes that cannot cross field boundaries. The
  IPv4 addresses are sorted lexicographically. The second sort uses
  ‘`-s`’ so that ties in the primary key are broken by the secondary
  key; the first sort uses ‘`-s`’ so that the combination of the two
  sorts is stable. As a GNU extension, the above example could
  be achieved in a single `sort` invocation by sorting the
  IPv4 address field using a ‘`V`’ version type, like ‘`-k1,1V`’.
* Generate a tags file in case-insensitive sorted order.

  ```
  find src -type f -print0 | sort -z -f | xargs -0 etags --append
  ```

  The use of `-print0`, `-z`, and `-0` in this case means
  that file names that contain blanks or other special characters are
  not broken up
  by the sort operation.
* Use the common DSU, Decorate Sort Undecorate idiom to
  sort lines according to their length.

  ```
  getent passwd |
  awk '{print length, $0}' | sort -n | cut -f2- -d' '
  ```

  In general this technique can be used to sort data that the `sort`
  command does not support, or is inefficient at, sorting directly.
* Use the same DSU idiom as above to sort lines by their last field,
  and in this specific example the presented lines are users’ full names.

  ```
  getent passwd | grep -v nologin | cut -d: -f5 | grep ' ' |
  awk '{print $NF, $0}' | sort -k1,1 | cut -f2- -d' '
  ```
* Shuffle a list of directories, but preserve the order of files within
  each directory. For instance, one could use this to generate a music
  playlist in which albums are shuffled but the songs of each album are
  played in order.

  ```
  ls */* | sort -t / -k 1,1R -k 2,2
  ```

---

Next: [`uniq`: Uniquify files](#uniq-invocation), Previous: [`sort`: Sort text files](#sort-invocation), Up: [Operating on sorted files](#Operating-on-sorted-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 7.2 `shuf`: Shuffling text [¶](#shuf_003a-Shuffling-text)

`shuf` shuffles its input by outputting a random permutation
of its input lines. Each output permutation is equally likely.
Synopses:

```
shuf [option]... [file]
shuf -e [option]... [arg]...
shuf -i lo-hi [option]...
```

`shuf` has three modes of operation that affect where it
obtains its input lines. By default, it reads lines from standard
input. The following options change the operation mode:

‘`-e`’ [¶](#index-_002dc-8)

‘`--echo`’
:   Treat each command-line operand as an input line.

‘`-i lo-hi`’ [¶](#index-_002di-5)

‘`--input-range=lo-hi`’
:   Act as if input came from a file containing the range of unsigned
    decimal integers lo…hi, one per line.

`shuf`’s other options can affect its behavior in all
operation modes:

‘`-n count`’ [¶](#index-_002dn-8)

‘`--head-count=count`’
:   Output at most count lines. By default, all input lines are
    output.

‘`-o output-file`’ [¶](#index-_002do-3)

‘`--output=output-file`’
:   Write output to output-file instead of standard output.
    `shuf` reads all input before opening
    output-file, so you can safely shuffle a file in place by using
    commands like `shuf -o F <F` and `cat F | shuf -o F`.

‘`--random-source=file`’ [¶](#index-_002d_002drandom_002dsource-1)
:   Use file as a source of random data used to determine which
    permutation to generate. See [Sources of random data](#Random-sources).

‘`-r`’ [¶](#index-_002dr-4)

‘`--repeat`’
:   Repeat output values, that is, select with replacement. With this
    option the output is not a permutation of the input; instead, each
    output line is randomly chosen from all the inputs. This option is
    typically combined with `--head-count`; if
    `--head-count` is not given, `shuf` repeats
    indefinitely.

‘`-z`’ [¶](#index-_002dz-5)

‘`--zero-terminated`’
:   Delimit items with a zero byte rather than a newline (ASCII LF).
    I.e., treat input as items separated by ASCII NUL
    and terminate output items with ASCII NUL.
    This option can be useful in conjunction with ‘`perl -0`’ or
    ‘`find -print0`’ and ‘`xargs -0`’ which do the same in order to
    reliably handle arbitrary file names (even those containing blanks
    or other special characters).

For example:

```
shuf <<EOF
A man,
a plan,
a canal:
Panama!
EOF
```

might produce the output

```
Panama!
A man,
a canal:
a plan,
```

Similarly, the command:

```
shuf -e clubs hearts diamonds spades
```

might output:

```
clubs
diamonds
spades
hearts
```

and the command ‘`shuf -i 1-4`’ might output:

```
4
2
1
3
```

The above examples all have four input lines, so `shuf` might
produce any of the twenty-four possible permutations of the input. In
general, if there are n input lines, there are n! (i.e.,
n factorial, or n \* (n - 1) \* … \* 1) possible
output permutations.

To output 50 random numbers each in the range 0 through 9, use:

```
shuf -r -n 50 -i 0-9
```

To simulate 100 coin flips, use:

```
shuf -r -n 100 -e Head Tail
```

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`comm`: Compare two sorted files line by line](#comm-invocation), Previous: [`shuf`: Shuffling text](#shuf-invocation), Up: [Operating on sorted files](#Operating-on-sorted-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 7.3 `uniq`: Uniquify files [¶](#uniq_003a-Uniquify-files)

`uniq` writes the unique lines in the given `input`, or
standard input if nothing is given or for an input name of
‘`-`’. Synopsis:

```
uniq [option]... [input [output]]
```

By default, `uniq` prints its input lines, except that
it discards all but the first of adjacent repeated lines, so that
no output lines are repeated. Optionally, it can instead discard
lines that are not repeated, or all repeated lines.

The input need not be sorted, but repeated input lines are detected
only if they are adjacent. If you want to discard non-adjacent
duplicate lines, perhaps you want to use `sort -u`.
See [`sort`: Sort text files](#sort-invocation).

Comparisons honor the rules specified by the `LC_COLLATE`
locale category.

If no output file is specified, `uniq` writes to standard
output.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-f n`’ [¶](#index-_002df-6)

‘`--skip-fields=n`’
:   Skip n fields on each line before checking for uniqueness. Use
    a null string for comparison if a line has fewer than n fields.
    Fields are a sequence of blank characters followed by non-blank characters.
    Field numbers are one based, i.e., `-f 1` will skip the first
    field (which may optionally have leading blanks).

    For compatibility `uniq` supports a traditional option syntax
    `-n`. New scripts should use `-f n` instead.

‘`-s n`’ [¶](#index-_002ds-11)

‘`--skip-chars=n`’
:   Skip n characters before checking for uniqueness. Use a null string
    for comparison if a line has fewer than n characters. If you use both
    the field and character skipping options, fields are skipped over first.

    On systems not conforming to POSIX 1003.1-2001,
    `uniq` supports a traditional option syntax
    `+n`.
    Although this traditional behavior can be controlled with the
    `_POSIX2_VERSION` environment variable (see [Standards conformance](#Standards-conformance)), portable scripts should avoid commands whose
    behavior depends on this variable.
    For example, use ‘`uniq ./+10`’ or ‘`uniq -s 10`’ rather than
    the ambiguous ‘`uniq +10`’.

‘`-c`’ [¶](#index-_002dc-9)

‘`--count`’
:   Print the number of times each line occurred along with the line.

‘`-i`’ [¶](#index-_002di-6)

‘`--ignore-case`’
:   Ignore differences in case when comparing lines.

‘`-d`’ [¶](#index-_002dd-6)

‘`--repeated`’
:   Discard lines that are not repeated. When used by itself, this option
    causes `uniq` to print the first copy of each repeated line,
    and nothing else.

‘`-D`’ [¶](#index-_002dD)

‘`--all-repeated[=delimit-method]`’
:   Do not discard the second and subsequent repeated input lines,
    but discard lines that are not repeated.
    This option is useful mainly in conjunction with other options e.g.,
    to ignore case or to compare only selected fields.
    The optional delimit-method, supported with the long form option,
    specifies how to delimit groups of repeated lines, and must be one of the
    following:

    ‘`none`’
    :   Do not delimit groups of repeated lines.
        This is equivalent to `--all-repeated` (`-D`).

    ‘`prepend`’
    :   Output a newline before each group of repeated lines.
        With `--zero-terminated` (`-z`), use a zero
        byte (ASCII NUL) instead of a newline as the delimiter.

    ‘`separate`’
    :   Separate groups of repeated lines with a single newline.
        This is the same as using ‘`prepend`’, except that
        no delimiter is inserted before the first group, and hence
        may be better suited for output direct to users.
        With `--zero-terminated` (`-z`), use a zero
        byte (ASCII NUL) instead of a newline as the delimiter.

    Output is ambiguous when groups are delimited and the input stream
    contains empty lines.
    To avoid that, filter the input through ‘`tr -s '\n'`’ to
    remove blank lines.

    This is a GNU extension.

‘`--group[=delimit-method]`’ [¶](#index-_002d_002dgroup)
:   Output all lines, and delimit each unique group.
    With `--zero-terminated` (`-z`), use a zero
    byte (ASCII NUL) instead of a newline as the delimiter.
    The optional delimit-method specifies how to delimit
    groups, and must be one of the following:

    ‘`separate`’
    :   Separate unique groups with a single delimiter.
        This is the default delimiting method if none is specified,
        and better suited for output direct to users.

    ‘`prepend`’
    :   Output a delimiter before each group of unique items.

    ‘`append`’
    :   Output a delimiter after each group of unique items.

    ‘`both`’
    :   Output a delimiter around each group of unique items.

    Output is ambiguous when groups are delimited and the input stream
    contains empty lines.
    To avoid that, filter the input through ‘`tr -s '\n'`’ to
    remove blank lines.

    This is a GNU extension.

‘`-u`’ [¶](#index-_002du-4)

‘`--unique`’
:   Discard the last line that would be output for a repeated input group.
    When used by itself, this option causes `uniq` to print unique
    lines, and nothing else.

‘`-w n`’ [¶](#index-_002dw-8)

‘`--check-chars=n`’
:   Compare at most n characters on each line (after skipping any specified
    fields and characters). By default the entire rest of the lines are
    compared.

‘`-z`’ [¶](#index-_002dz-6)

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

---

Next: [`ptx`: Produce permuted indexes](#ptx-invocation), Previous: [`uniq`: Uniquify files](#uniq-invocation), Up: [Operating on sorted files](#Operating-on-sorted-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 7.4 `comm`: Compare two sorted files line by line [¶](#comm_003a-Compare-two-sorted-files-line-by-line)

`comm` writes to standard output lines that are common, and lines
that are unique, to two input files; a file name of ‘`-`’ means
standard input. Synopsis:

```
comm [option]... file1 file2
```

Before `comm` can be used, the input files must be sorted using the
collating sequence specified by the `LC_COLLATE` locale.
If an input file ends in a non-newline
character, a newline is silently appended. The `sort` command with
no options always outputs a file that is suitable input to `comm`.

With no options, `comm` produces three-column output. Column one
contains lines unique to file1, column two contains lines unique
to file2, and column three contains lines common to both files.
Columns are separated by a single TAB character.

The options `-1`, `-2`, and `-3` suppress printing of
the corresponding columns (and separators). Also see [Common options](#Common-options).

Unlike some other comparison utilities, `comm` has an exit
status that does not depend on the result of the comparison.
Upon normal completion `comm` produces an exit code of zero.
If there is an error it exits with nonzero status.

If the `--check-order` option is given, unsorted inputs will
cause a fatal error message. If the option `--nocheck-order`
is given, unsorted inputs will never cause an error message. If neither
of these options is given, wrongly sorted inputs are diagnosed
only if an input file is found to contain unpairable
lines.
If an input file is diagnosed as being unsorted, the `comm`
command will exit with a nonzero status (and the output should not be used).

Forcing `comm` to process wrongly sorted input files
containing unpairable lines by specifying `--nocheck-order` is
not guaranteed to produce any particular output. The output will
probably not correspond with whatever you hoped it would be.

‘`--check-order`’
:   Fail with an error message if either input file is wrongly ordered.

‘`--nocheck-order`’
:   Do not check that both input files are in sorted order.

    Other options are:

‘`--output-delimiter=str`’
:   Print str between adjacent output columns,
    rather than the default of a single TAB character.

    The delimiter str may be empty, in which case
    the ASCII NUL character is used to delimit output columns.

‘`--total`’
:   Output a summary at the end.

    Similar to the regular output,
    column one contains the total number of lines unique to file1,
    column two contains the total number of lines unique to file2, and
    column three contains the total number of lines common to both files,
    followed by the word ‘`total`’ in the additional column four.

    In the following example, `comm` omits the regular output
    (`-123`), thus just printing the summary:

    ```
    $ printf '%s\n' a b c d e     > file1
    $ printf '%s\n'   b c d e f g > file2
    $ comm --total -123 file1 file2
    1       2       4       total
    ```

    This option is a GNU extension. Portable scripts should use `wc` to
    get the totals, e.g. for the above example files:

    ```
    $ comm -23 file1 file2 | wc -l    # number of lines only in file1
    1
    $ comm -13 file1 file2 | wc -l    # number of lines only in file2
    2
    $ comm -12 file1 file2 | wc -l    # number of lines common to both files
    4
    ```

‘`-z`’ [¶](#index-_002dz-7)

‘`--zero-terminated`’
:   Delimit items with a zero byte rather than a newline (ASCII LF).
    I.e., treat input as items separated by ASCII NUL
    and terminate output items with ASCII NUL.
    This option can be useful in conjunction with ‘`perl -0`’ or
    ‘`find -print0`’ and ‘`xargs -0`’ which do the same in order to
    reliably handle arbitrary file names (even those containing blanks
    or other special characters).

---

Next: [`tsort`: Topological sort](#tsort-invocation), Previous: [`comm`: Compare two sorted files line by line](#comm-invocation), Up: [Operating on sorted files](#Operating-on-sorted-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 7.5 `ptx`: Produce permuted indexes [¶](#ptx_003a-Produce-permuted-indexes)

`ptx` reads a text file and essentially produces a permuted index, with
each keyword in its context. The calling sketch is either one of:

```
ptx [option ...] [file ...]
ptx -G [option ...] [input [output]]
```

The `-G` (or its equivalent: `--traditional`) option disables
all GNU extensions and reverts to traditional mode, thus introducing some
limitations and changing several of the program’s default option values.
When `-G` is not specified, GNU extensions are always enabled.
GNU extensions to `ptx` are documented wherever appropriate in this
document. See [The GNU extensions to `ptx`](#Compatibility-in-ptx), for the full list.

Individual options are explained in the following sections.

When GNU extensions are enabled, there may be zero, one or several
files after the options. If there is no file, the program
reads the standard input. If there is one or several files, they
give the name of input files which are all read in turn, as if all the
input files were concatenated. However, there is a full contextual
break between each file and, when automatic referencing is requested,
file names and line numbers refer to individual text input files. In
all cases, the program outputs the permuted index to the standard
output.

When GNU extensions are *not* enabled, that is, when the program
operates in traditional mode, there may be zero, one or two parameters
besides the options. If there are no parameters, the program reads the
standard input and outputs the permuted index to the standard output.
If there is only one parameter, it names the text input to be read
instead of the standard input. If two parameters are given, they give
respectively the name of the input file to read and the name of
the output file to produce. *Be very careful:*
in this case, the contents of file given by the second parameter is
destroyed. This behavior is dictated by System V `ptx`
compatibility; GNU Standards normally discourage output parameters not
introduced by an option.

For *any* file named as the value of an option or as an
input text file, a single dash ‘`-`’ may be used, in which case
standard input is assumed. However, it would not make sense to use this
convention more than once per program invocation.

* [General options](#General-options-in-ptx)
* [Charset selection](#Charset-selection-in-ptx)
* [Word selection and input processing](#Input-processing-in-ptx)
* [Output formatting](#Output-formatting-in-ptx)
* [The GNU extensions to `ptx`](#Compatibility-in-ptx)

---

Next: [Charset selection](#Charset-selection-in-ptx), Up: [`ptx`: Produce permuted indexes](#ptx-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 7.5.1 General options [¶](#General-options)

‘`-G`’

‘`--traditional`’
:   As already explained, this option disables all GNU extensions to
    `ptx` and switches to traditional mode.

‘`--help`’
:   Print a short help on standard output, then exit without further
    processing.

‘`--version`’
:   Print the program version on standard output, then exit without further
    processing.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [Word selection and input processing](#Input-processing-in-ptx), Previous: [General options](#General-options-in-ptx), Up: [`ptx`: Produce permuted indexes](#ptx-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 7.5.2 Charset selection [¶](#Charset-selection)

As it is set up now, `ptx` assumes that the input file is coded
using 8-bit characters, and it may not work well in multibyte locales.
In a single-byte locale, the default regular expression
for a keyword allows foreign or diacriticized letters. Keyword sorting,
however, is still crude; it obeys the underlying character set ordering
quite blindly.

The output of `ptx` assumes the locale’s character encoding.
For example, with `ptx`’s `-T` option, if the locale
uses the Latin-1 encoding you may need a LaTeX directive like
‘`\usepackage[latin1]{inputenc}`’ to render non-ASCII characters
correctly.

‘`-f`’ [¶](#index-_002df-7)

‘`--ignore-case`’
:   Fold lower case letters to upper case for sorting.

---

Next: [Output formatting](#Output-formatting-in-ptx), Previous: [Charset selection](#Charset-selection-in-ptx), Up: [`ptx`: Produce permuted indexes](#ptx-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 7.5.3 Word selection and input processing [¶](#Word-selection-and-input-processing)

‘`-b file`’ [¶](#index-_002db-10)

‘`--break-file=file`’
:   This option provides an alternative (to `-W`) method of describing
    which characters make up words. It introduces the name of a
    file which contains a list of characters which can*not* be part of
    one word; this file is called the *Break file*. Any character which
    is not part of the Break file is a word constituent. If both options
    `-b` and `-W` are specified, then `-W` has precedence and
    `-b` is ignored.

    When GNU extensions are enabled, the only way to avoid newline as a
    break character is to write all the break characters in the file with no
    newline at all, not even at the end of the file. When GNU extensions
    are disabled, spaces, tabs and newlines are always considered as break
    characters even if not included in the Break file.

‘`-i file`’ [¶](#index-_002di-7)

‘`--ignore-file=file`’
:   The file associated with this option contains a list of words which will
    never be taken as keywords in concordance output. It is called the
    *Ignore file*. The file contains exactly one word in each line; the
    end of line separation of words is not subject to the value of the
    `-S` option.

‘`-o file`’ [¶](#index-_002do-4)

‘`--only-file=file`’
:   The file associated with this option contains a list of words which will
    be retained in concordance output; any word not mentioned in this file
    is ignored. The file is called the *Only file*. The file contains
    exactly one word in each line; the end of line separation of words is
    not subject to the value of the `-S` option.

    There is no default for the Only file. When both an Only file and an
    Ignore file are specified, a word is considered a keyword only
    if it is listed in the Only file and not in the Ignore file.

‘`-r`’ [¶](#index-_002dr-5)

‘`--references`’
:   On each input line, the leading sequence of non-white space characters will be
    taken to be a reference that has the purpose of identifying this input
    line in the resulting permuted index.
    See [Output formatting](#Output-formatting-in-ptx),
    for more information about reference production.
    Using this option changes the default value for option `-S`.

    Using this option, the program does not try very hard to remove
    references from contexts in output, but it succeeds in doing so
    *when* the context ends exactly at the newline. If option
    `-r` is used with `-S` default value, or when GNU extensions
    are disabled, this condition is always met and references are completely
    excluded from the output contexts.

‘`-S regexp`’ [¶](#index-_002dS-4)

‘`--sentence-regexp=regexp`’
:   This option selects which regular expression will describe the end of a
    line or the end of a sentence. In fact, this regular expression is not
    the only distinction between end of lines or end of sentences, and input
    line boundaries have no special significance outside this option. By
    default, when GNU extensions are enabled and if `-r` option is not
    used, end of sentences are used. In this case, this regex is
    imported from GNU Emacs:

    ```
    [.?!][]\"')}]*\\($\\|\t\\|  \\)[ \t\n]*
    ```

    Whenever GNU extensions are disabled or if `-r` option is used, end
    of lines are used; in this case, the default regexp is just:

    ```
    \n
    ```

    Using an empty regexp is equivalent to completely disabling end of
    line or end of sentence recognition. In this case, the whole file is
    considered to be a single big line or sentence. The user might want to
    disallow all truncation flag generation as well, through option `-F
    ""`. See [Syntax of Regular Expressions](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Regexps) in The GNU Emacs
    Manual.

    When the keywords happen to be near the beginning of the input line or
    sentence, this often creates an unused area at the beginning of the
    output context line; when the keywords happen to be near the end of the
    input line or sentence, this often creates an unused area at the end of
    the output context line. The program tries to fill those unused areas
    by wrapping around context in them; the tail of the input line or
    sentence is used to fill the unused area on the left of the output line;
    the head of the input line or sentence is used to fill the unused area
    on the right of the output line.

    As a matter of convenience to the user, many usual backslashed escape
    sequences from the C language are recognized and converted to the
    corresponding characters by `ptx` itself.

‘`-W regexp`’ [¶](#index-_002dW-1)

‘`--word-regexp=regexp`’
:   This option selects which regular expression will describe each keyword.
    By default, if GNU extensions are enabled, a word is a sequence of
    letters; the regexp used is ‘`\w+`’. When GNU extensions are
    disabled, a word is by default anything which ends with a space, a tab
    or a newline; the regexp used is ‘`[^ \t\n]+`’.

    An empty regexp is equivalent to not using this option.
    See [Syntax of Regular Expressions](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Regexps) in The GNU Emacs
    Manual.

    As a matter of convenience to the user, many usual backslashed escape
    sequences, as found in the C language, are recognized and converted to
    the corresponding characters by `ptx` itself.

---

Next: [The GNU extensions to `ptx`](#Compatibility-in-ptx), Previous: [Word selection and input processing](#Input-processing-in-ptx), Up: [`ptx`: Produce permuted indexes](#ptx-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 7.5.4 Output formatting [¶](#Output-formatting)

Output format is mainly controlled by the `-O` and `-T` options
described in the table below. When neither `-O` nor `-T` are
selected, and if GNU extensions are enabled, the program chooses an
output format suitable for a dumb terminal. Each keyword occurrence is
output to the center of one line, surrounded by its left and right
contexts. Each field is properly justified, so the concordance output
can be readily observed. As a special feature, if automatic
references are selected by option `-A` and are output before the
left context, that is, if option `-R` is *not* selected, then
a colon is added after the reference; this nicely interfaces with GNU
Emacs `next-error` processing. In this default output format, each
white space character, like newline and tab, is merely changed to
exactly one space, with no special attempt to compress consecutive
spaces. This might change in the future. Except for those white space
characters, every other character of the underlying set of 256
characters is transmitted verbatim.

Output format is further controlled by the following options.

‘`-g number`’ [¶](#index-_002dg-2)

‘`--gap-size=number`’
:   Select the size of the minimum white space gap between the fields on the
    output line.

‘`-w number`’ [¶](#index-_002dw-9)

‘`--width=number`’
:   Select the maximum output width of each final line. If references are
    used, they are included or excluded from the maximum output width
    depending on the value of option `-R`. If this option is not
    selected, that is, when references are output before the left context,
    the maximum output width takes into account the maximum length of all
    references. If this option is selected, that is, when references are
    output after the right context, the maximum output width does not take
    into account the space taken by references, nor the gap that precedes
    them.

‘`-A`’ [¶](#index-_002dA-2)

‘`--auto-reference`’
:   Select automatic references. Each input line will have an automatic
    reference made up of the file name and the line ordinal, with a single
    colon between them. However, the file name will be empty when standard
    input is being read. If both `-A` and `-r` are selected, then
    the input reference is still read and skipped, but the automatic
    reference is used at output time, overriding the input reference.

‘`-R`’ [¶](#index-_002dR-1)

‘`--right-side-refs`’
:   In the default output format, when option `-R` is not used, any
    references produced by the effect of options `-r` or `-A` are
    placed to the far right of output lines, after the right context. With
    default output format, when the `-R` option is specified, references
    are rather placed at the beginning of each output line, before the left
    context. For any other output format, option `-R` is
    ignored, with one exception: with `-R` the width of references
    is *not* taken into account in total output width given by `-w`.

    This option is automatically selected whenever GNU extensions are
    disabled.

‘`-F string`’ [¶](#index-_002dF-2)

‘`--flag-truncation=string`’
:   This option will request that any truncation in the output be reported
    using the string string. Most output fields theoretically extend
    towards the beginning or the end of the current line, or current
    sentence, as selected with option `-S`. But there is a maximum
    allowed output line width, changeable through option `-w`, which is
    further divided into space for various output fields. When a field has
    to be truncated because it cannot extend beyond the beginning or the end of
    the current line to fit in, then a truncation occurs. By default,
    the string used is a single slash, as in `-F /`.

    string may have more than one character, as in `-F …`.
    Also, in the particular case when string is empty (`-F ""`),
    truncation flagging is disabled, and no truncation marks are appended in
    this case.

    As a matter of convenience to the user, many usual backslashed escape
    sequences, as found in the C language, are recognized and converted to
    the corresponding characters by `ptx` itself.

‘`-M string`’ [¶](#index-_002dM-1)

‘`--macro-name=string`’
:   Select another string to be used instead of ‘`xx`’, while
    generating output suitable for `nroff`, `troff` or TeX.

‘`-O`’ [¶](#index-_002dO)

‘`--format=roff`’
:   Choose an output format suitable for `nroff` or `troff`
    processing. Each output line will look like:

    ```
    .xx "tail" "before" "keyword_and_after" "head" "ref"
    ```

    so it will be possible to write a ‘`.xx`’ roff macro to take care of
    the output typesetting. This is the default output format when GNU
    extensions are disabled. Option `-M` can be used to change
    ‘`xx`’ to another macro name.

    In this output format, each non-graphical character, like newline and
    tab, is merely changed to exactly one space, with no special attempt to
    compress consecutive spaces. Each quote character ‘`"`’ is doubled
    so it will be correctly processed by `nroff` or `troff`.

‘`-T`’ [¶](#index-_002dT-3)

‘`--format=tex`’
:   Choose an output format suitable for TeX processing. Each output
    line will look like:

    ```
    \xx {tail}{before}{keyword}{after}{head}{ref}
    ```

    so it will be possible to write a `\xx` definition to take care of
    the output typesetting. When references are not being
    produced, that is, neither option `-A` nor option `-r` is
    selected, the last parameter of each `\xx` call is inhibited.
    Option `-M` can be used to change ‘`xx`’ to another macro
    name.

    In this output format, some special characters, like ‘`$`’, ‘`%`’,
    ‘`&`’, ‘`#`’ and ‘`_`’ are automatically protected with a
    backslash. Curly brackets ‘`{`’, ‘`}`’ are protected with a
    backslash and a pair of dollar signs (to force mathematical mode). The
    backslash itself produces the sequence `\backslash{}`.
    Circumflex and tilde diacritical marks produce the sequence `^\{ }` and
    `~\{ }` respectively. Other diacriticized characters of the
    underlying character set produce an appropriate TeX sequence as far
    as possible. The other non-graphical characters, like newline and tab,
    and all other characters which are not part of ASCII, are merely
    changed to exactly one space, with no special attempt to compress
    consecutive spaces. Let me know how to improve this special character
    processing for TeX.

---

Previous: [Output formatting](#Output-formatting-in-ptx), Up: [`ptx`: Produce permuted indexes](#ptx-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 7.5.5 The GNU extensions to `ptx` [¶](#The-GNU-extensions-to-ptx)

This version of `ptx` contains a few features which do not exist in
System V `ptx`. These extra features are suppressed by using the
`-G` command line option, unless overridden by other command line
options. Some GNU extensions cannot be recovered by overriding, so the
simple rule is to avoid `-G` if you care about GNU extensions.
Here are the differences between this program and System V `ptx`.

* This program can read many input files at once, it always writes the
  resulting concordance on standard output. On the other hand, System V
  `ptx` reads only one file and sends the result to standard output
  or, if a second file parameter is given on the command, to that
  file.

  Having output parameters not introduced by options is a dangerous
  practice which GNU avoids as far as possible. So, for using `ptx`
  portably between GNU and System V, you should always use it with a
  single input file, and always expect the result on standard output. You
  might also want to automatically configure in a `-G` option to
  `ptx` calls in products using `ptx`, if the configurator finds
  that the installed `ptx` accepts `-G`.
* The only options available in System V `ptx` are options `-b`,
  `-f`, `-g`, `-i`, `-o`, `-r`, `-t` and
  `-w`. All other options are GNU extensions and are not repeated in
  this enumeration. Moreover, some options have a slightly different
  meaning when GNU extensions are enabled, as explained below.
* By default, concordance output is not formatted for `troff` or
  `nroff`. It is rather formatted for a dumb terminal. `troff`
  or `nroff` output may still be selected through option `-O`.
* Unless `-R` option is used, the maximum reference width is
  subtracted from the total output line width. With GNU extensions
  disabled, width of references is not taken into account in the output
  line width computations.
* All 256 bytes, even ASCII NUL bytes, are always read and
  processed from input file with no adverse effect, even if GNU extensions
  are disabled. However, System V `ptx` does not accept 8-bit
  characters, a few control characters are rejected, and the tilde
  ‘`~`’ is also rejected.
* Input line length is only limited by available memory, even if GNU
  extensions are disabled. However, System V `ptx` processes only
  the first 200 characters in each line.
* The break (non-word) characters default to be every character except all
  letters of the underlying character set, diacriticized or not. When GNU
  extensions are disabled, the break characters default to space, tab and
  newline only.
* The program makes better use of output line width. If GNU extensions
  are disabled, the program rather tries to imitate System V `ptx`,
  but still, there are some slight disposition glitches this program does
  not completely reproduce.
* The user can specify both an Ignore file and an Only file. This is not
  allowed with System V `ptx`.

---

Previous: [`ptx`: Produce permuted indexes](#ptx-invocation), Up: [Operating on sorted files](#Operating-on-sorted-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 7.6 `tsort`: Topological sort [¶](#tsort_003a-Topological-sort)

`tsort` performs a topological sort on the given file, or
standard input if no input file is given or for a file of
‘`-`’. For more details and some history, see [`tsort`: Background](#tsort-background).
Synopsis:

```
tsort [option] [file]
```

`tsort` reads its input as pairs of strings, separated by blanks,
indicating a partial ordering. The output is a total ordering that
corresponds to the given partial ordering.

For example

```
tsort <<EOF
a b c
d
e f
b c d e
EOF
```

will produce the output

```
a
b
c
d
e
f
```

Consider a more realistic example.
You have a large set of functions all in one file, and they may all be
declared static except one. Currently that one (say `main`) is the
first function defined in the file, and the ones it calls directly follow
it, followed by those they call, etc. Let’s say that you are determined
to take advantage of prototypes, so you have to choose between declaring
all of those functions (which means duplicating a lot of information from
the definitions) and rearranging the functions so that as many as possible
are defined before they are used. One way to automate the latter process
is to get a list for each function of the functions it calls directly.
Many programs can generate such lists. They describe a call graph.
Consider the following list, in which a given line indicates that the
function on the left calls the one on the right directly.

```
main parse_options
main tail_file
main tail_forever
tail_file pretty_name
tail_file write_header
tail_file tail
tail_forever recheck
tail_forever pretty_name
tail_forever write_header
tail_forever dump_remainder
tail tail_lines
tail tail_bytes
tail_lines start_lines
tail_lines dump_remainder
tail_lines file_lines
tail_lines pipe_lines
tail_bytes xlseek
tail_bytes start_bytes
tail_bytes dump_remainder
tail_bytes pipe_bytes
file_lines dump_remainder
recheck pretty_name
```

then you can use `tsort` to produce an ordering of those
functions that satisfies your requirement.

```
example$ tsort call-graph | tac
dump_remainder
start_lines
file_lines
pipe_lines
xlseek
start_bytes
pipe_bytes
tail_lines
tail_bytes
pretty_name
write_header
tail
recheck
parse_options
tail_file
tail_forever
main
```

`tsort` detects any cycles in the input and writes the first cycle
encountered to standard error.

For a given partial ordering, generally there is no unique
total ordering. In the context of the call graph above, the function
`parse_options` may be placed anywhere in the list as long as it
precedes `main`.

The only options are `--help` and `--version`. See [Common options](#Common-options).

An exit status of zero indicates success,
and a nonzero value indicates failure.

* [`tsort`: Background](#tsort-background)

---

Up: [`tsort`: Topological sort](#tsort-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 7.6.1 `tsort`: Background [¶](#tsort_003a-Background)

`tsort` exists because very early versions of the Unix linker processed
an archive file exactly once, and in order. As `ld` read each object
in the archive, it decided whether it was needed in the program based on
whether it defined any symbols which were undefined at that point in
the link.

This meant that dependencies within the archive had to be handled
specially. For example, `scanf` probably calls `read`. That means
that in a single pass through an archive, it was important for `scanf.o`
to appear before read.o, because otherwise a program which calls
`scanf` but not `read` might end up with an unexpected unresolved
reference to `read`.

The way to address this problem was to first generate a set of
dependencies of one object file on another. This was done by a shell
script called `lorder`. The GNU tools don’t provide a version of
lorder, as far as I know, but you can still find it in BSD
distributions.

Then you ran `tsort` over the `lorder` output, and you used the
resulting sort to define the order in which you added objects to the archive.

This whole procedure has been obsolete since about 1980, because
Unix archives now contain a symbol table (traditionally built by
`ranlib`, now generally built by `ar` itself), and the Unix
linker uses the symbol table to effectively make multiple passes over
an archive file.

Anyhow, that’s where tsort came from. To solve an old problem with
the way the linker handled archive files, which has since been solved
in different ways.

---

Next: [Operating on characters](#Operating-on-characters), Previous: [Operating on sorted files](#Operating-on-sorted-files), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 8 Operating on fields [¶](#Operating-on-fields-1)

* [`cut`: Print selected parts of lines](#cut-invocation)
* [`paste`: Merge lines of files](#paste-invocation)
* [`join`: Join lines on a common field](#join-invocation)

---

Next: [`paste`: Merge lines of files](#paste-invocation), Up: [Operating on fields](#Operating-on-fields)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 8.1 `cut`: Print selected parts of lines [¶](#cut_003a-Print-selected-parts-of-lines)

`cut` writes to standard output selected parts of each line of each
input file, or standard input if no files are given or for a file name of
‘`-`’. Synopsis:

```
cut option... [file]...
```

In the table which follows, the byte-list, character-list,
and field-list are one or more numbers or ranges (two numbers
separated by a dash) separated by commas. Bytes, characters, and
fields are numbered starting at 1. Incomplete ranges may be
given: `-m` means ‘`1-m`’; ‘`n-`’ means
‘`n`’ through end of line or last field. The list elements
can be repeated, can overlap, and can be specified in any order; but
the selected input is written in the same order that it is read, and
is written exactly once.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-b byte-list`’ [¶](#index-_002db-11)

‘`--bytes=byte-list`’
:   Select for printing only the bytes in positions listed in
    byte-list. Tabs and backspaces are treated like any other
    character; they take up 1 byte. If an output delimiter is specified,
    (see the description of `--output-delimiter`), then output that
    string between ranges of selected bytes.

‘`-c character-list`’ [¶](#index-_002dc-10)

‘`--characters=character-list`’
:   Select for printing only the characters in positions listed in
    character-list. The same as `-b` for now, but
    internationalization will change that. Tabs and backspaces are
    treated like any other character; they take up 1 character. If an
    output delimiter is specified, (see the description of
    `--output-delimiter`), then output that string between ranges
    of selected bytes.

‘`-f field-list`’ [¶](#index-_002df-8)

‘`--fields=field-list`’
:   Select for printing only the fields listed in field-list.
    Fields are separated by a TAB character by default. Also print any
    line that contains no delimiter character, unless the
    `--only-delimited` (`-s`) option is specified.

    The `awk` command supports more sophisticated field processing,
    like reordering fields, and handling fields aligned with blank characters.
    By default `awk` uses (and discards) runs of blank characters
    to separate fields, and ignores leading and trailing blanks.

    ```
    awk '{print $2}'      # print the second field
    awk '{print $(NF-1)}' # print the penultimate field
    awk '{print $2,$1}'   # reorder the first two fields
    ```

    While `cut` accepts field specifications in
    arbitrary order, output is always in the order encountered in the file.

    In the unlikely event that `awk` is unavailable,
    one can use the `join` command, to process blank
    characters as `awk` does above.

    ```
    join -a1 -o 1.2     - /dev/null # print the second field
    join -a1 -o 1.2,1.1 - /dev/null # reorder the first two fields
    ```

‘`-d input_delim_byte`’ [¶](#index-_002dd-7)

‘`--delimiter=input_delim_byte`’
:   With `-f`, use the first byte of input\_delim\_byte as
    the input fields separator (default is TAB).

‘`-n`’ [¶](#index-_002dn-9)
:   Do not split multi-byte characters (no-op for now).

‘`-s`’ [¶](#index-_002ds-12)

‘`--only-delimited`’
:   For `-f`, do not print lines that do not contain the field separator
    character. Normally, any line without a field separator is printed verbatim.

‘`--output-delimiter=output_delim_string`’ [¶](#index-_002d_002doutput_002ddelimiter)
:   With `-f`, output fields are separated by output\_delim\_string.
    The default with `-f` is to use the input delimiter.
    When using `-b` or `-c` to select ranges of byte or
    character offsets (as opposed to ranges of fields),
    output output\_delim\_string between non-overlapping
    ranges of selected bytes.

‘`--complement`’ [¶](#index-_002d_002dcomplement)
:   This option is a GNU extension.
    Select for printing the complement of the bytes, characters or fields
    selected with the `-b`, `-c` or `-f` options.
    In other words, do *not* print the bytes, characters or fields
    specified via those options. This option is useful when you have
    many fields and want to print all but a few of them.

‘`-z`’ [¶](#index-_002dz-8)

‘`--zero-terminated`’
:   Delimit items with a zero byte rather than a newline (ASCII LF).
    I.e., treat input as items separated by ASCII NUL
    and terminate output items with ASCII NUL.
    This option can be useful in conjunction with ‘`perl -0`’ or
    ‘`find -print0`’ and ‘`xargs -0`’ which do the same in order to
    reliably handle arbitrary file names (even those containing blanks
    or other special characters).

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`join`: Join lines on a common field](#join-invocation), Previous: [`cut`: Print selected parts of lines](#cut-invocation), Up: [Operating on fields](#Operating-on-fields)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 8.2 `paste`: Merge lines of files [¶](#paste_003a-Merge-lines-of-files)

`paste` writes to standard output lines consisting of sequentially
corresponding lines of each given file, separated by a TAB character.
Standard input is used for a file name of ‘`-`’ or if no input files
are given.

Synopsis:

```
paste [option]... [file]...
```

For example, with:

```
$ cat num2
1
2
$ cat let3
a
b
c
```

Take lines sequentially from each file:

```
$ paste num2 let3
1       a
2       b
        c
```

Duplicate lines from a file:

```
$ paste num2 let3 num2
1       a      1
2       b      2
        c
```

Intermix lines from standard input:

```
$ paste - let3 - < num2
1       a      2
        b
        c
```

Join consecutive lines with a space:

```
$ seq 4 | paste -d ' ' - -
1 2
3 4
```

The program accepts the following options. Also see [Common options](#Common-options).

‘`-s`’ [¶](#index-_002ds-13)

‘`--serial`’
:   Paste the lines of one file at a time rather than one line from each
    file. Using the above example data:

    ```
    $ paste -s num2 let3
    1       2
    a       b       c
    ```

‘`-d delim-list`’ [¶](#index-_002dd-8)

‘`--delimiters=delim-list`’
:   Consecutively use the characters in delim-list instead of
    TAB to separate merged lines. When delim-list is
    exhausted, start again at its beginning. Using the above example data:

    ```
    $ paste -d '%_' num2 let3 num2
    1%a_1
    2%b_2
    %c_
    ```

‘`-z`’ [¶](#index-_002dz-9)

‘`--zero-terminated`’
:   Delimit items with a zero byte rather than a newline (ASCII LF).
    I.e., treat input as items separated by ASCII NUL
    and terminate output items with ASCII NUL.
    This option can be useful in conjunction with ‘`perl -0`’ or
    ‘`find -print0`’ and ‘`xargs -0`’ which do the same in order to
    reliably handle arbitrary file names (even those containing blanks
    or other special characters).

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Previous: [`paste`: Merge lines of files](#paste-invocation), Up: [Operating on fields](#Operating-on-fields)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]
