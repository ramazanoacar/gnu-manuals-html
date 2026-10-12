# Requires getopt() library function.

function usage(     common)
{
    common = "[-a suffix-len] [file [outname]]"
    printf("usage: split [-l count]  %s\n", common) > "/dev/stderr"
    printf("       split [-b N[k|m]] %s\n", common) > "/dev/stderr"
    exit 1
}
```

Next, in a `BEGIN` rule we set the default values and parse the arguments.
After that we initialize the data structures used to cycle the suffix
from ‘`aa…`’ to ‘`zz…`’. Finally we set the name of
the first output file:

```
BEGIN {
    # Set defaults:
    Suffix_length = 2
    Line_count = 1000
    Byte_count = 0
    Outfile = "x"

    parse_arguments()

    init_suffix_data()

    Output = (Outfile compute_suffix())
}
```

Parsing the arguments is straightforward. The program follows our
convention (see [Naming Library Function Global Variables](#Library-Names)) of having important global variables
start with an uppercase letter:

```
function parse_arguments(   i, c, l, modifier)
{
    while ((c = getopt(ARGC, ARGV, "a:b:l:")) != -1) {
        if (c == "a")
            Suffix_length = Optarg + 0
        else if (c == "b") {
            Byte_count = Optarg + 0
            Line_count = 0

            l = length(Optarg)
            modifier = substr(Optarg, l, 1)
            if (modifier == "k")
                Byte_count *= 1024
            else if (modifier == "m")
                Byte_count *= 1024 * 1024
        } else if (c == "l") {
            Line_count = Optarg + 0
            Byte_count = 0
        } else
            usage()
    }

    # Clear out options
    for (i = 1; i < Optind; i++)
        ARGV[i] = ""

    # Check for filename
    if (ARGV[Optind]) {
        Optind++

        # Check for different prefix
        if (ARGV[Optind]) {
            Outfile = ARGV[Optind]
            ARGV[Optind] = ""

            if (++Optind < ARGC)
                usage()
        }
    }
}
```

Managing the file name suffix is interesting.
Given a suffix of length three, say, the values go from
‘`aaa`’, ‘`aab`’, ‘`aac`’ and so on, all the way to
‘`zzx`’, ‘`zzy`’, and finally ‘`zzz`’.
There are two important aspects to this:

* We have to be
  able to easily generate these suffixes, and in particular
  easily handle “rolling over”; for example, going from
  ‘`abz`’ to ‘`aca`’.
* We have to tell when we’ve finished with the last file,
  so that if we still have more input data we can print an
  error message and exit. The trick is to handle this *after*
  using the last suffix, and not when the final suffix is created.

The computation is handled by `compute_suffix()`.
This function is called every time a new file is opened.

The flow here is messy, because we want to generate ‘`zzzz`’ (say),
and use it, and only produce an error after all the file name
suffixes have been used up. The logical steps are as follows:

1. Generate the suffix, saving the value in `result` to return.
   To do this, the supplementary array `Suffix_ind` contains one
   element for each letter in the suffix. Each element ranges from 1 to
   26, acting as the index into a string containing all the lowercase
   letters of the English alphabet.
   It is initialized by `init_suffix_data()`.
   `result` is built up one letter at a time, using each `substr()`.
2. Prepare the data structures for the next time `compute_suffix()`
   is called. To do this, we loop over `Suffix_ind`, *backwards*.
   If the current element is less than 26, it’s incremented and the loop
   breaks (‘`abq`’ goes to ‘`abr`’). Otherwise, the element is
   reset to one and we move down the list (‘`abz`’ to ‘`aca`’).
   Thus, the `Suffix_ind` array is always “one step ahead” of the actual
   file name suffix to be returned.
3. Check if we’ve gone past the limit of possible file names.
   If `Reached_last` is true, print a message and exit. Otherwise,
   check if `Suffix_ind` describes a suffix where all the letters are
   ‘`z`’. If that’s the case we’re about to return the final suffix. If
   so, we set `Reached_last` to true so that the *next* call to
   `compute_suffix()` will cause a failure.

Physically, the steps in the function occur in the order 3, 1, 2:

```
function compute_suffix(    i, result, letters)
{
    # Logical step 3
    if (Reached_last) {
        printf("split: too many files!\n") > "/dev/stderr"
        exit 1
    } else if (on_last_file())
        Reached_last = 1    # fail when wrapping after 'zzz'

    # Logical step 1
    result = ""
    letters = "abcdefghijklmnopqrstuvwxyz"
    for (i = 1; i <= Suffix_length; i++)
        result = result substr(letters, Suffix_ind[i], 1)

    # Logical step 2
    for (i = Suffix_length; i >= 1; i--) {
        if (++Suffix_ind[i] > 26) {
            Suffix_ind[i] = 1
        } else
            break
    }

    return result
}
```

The `Suffix_ind` array and `Reached_last` are initialized
by `init_suffix_data()`:

```
function init_suffix_data(  i)
{
    for (i = 1; i <= Suffix_length; i++)
        Suffix_ind[i] = 1

    Reached_last = 0
}
```

The function `on_last_file()` returns true if `Suffix_ind` describes
a suffix where all the letters are ‘`z`’ by checking that all the elements
in the array are equal to 26:

```
function on_last_file(  i, on_last)
{
    on_last = 1
    for (i = 1; i <= Suffix_length; i++) {
        on_last = on_last && (Suffix_ind[i] == 26)
    }

    return on_last
}
```

The actual work of splitting the input file is done by the next two rules.
Since splitting by line count and splitting by byte count are mutually
exclusive, we simply use two separate rules, one for when `Line_count`
is greater than zero, and another for when `Byte_count` is greater than zero.

The variable `tcount` counts how many lines have been processed so far.
When it exceeds `Line_count`, it’s time to close the previous file and
switch to a new one:

```
Line_count > 0 {
    if (++tcount > Line_count) {
        close(Output)
        Output = (Outfile compute_suffix())
        tcount = 1
    }
    print > Output
}
```

The rule for handling bytes is more complicated. Since lines most likely
vary in length, the `Byte_count` boundary may be hit in the middle of
an input record. In that case, `split` has to write enough of the
first bytes of the input record to finish up `Byte_count` bytes, close
the file, open a new file, and write the rest of the record to the new file.
The logic here does all that:

```
Byte_count > 0 {
    # `+ 1' is for the final newline
    if (tcount + length($0) + 1 > Byte_count) { # would overflow
        # compute leading bytes
        leading_bytes = Byte_count - tcount

        # write leading bytes
        printf("%s", substr($0, 1, leading_bytes)) > Output

        # close old file, open new file
        close(Output)
        Output = (Outfile compute_suffix())

        # set up first bytes for new file
        $0 = substr($0, leading_bytes + 1)  # trailing bytes
        tcount = 0
    }

    # write full record or trailing bytes
    tcount += length($0) + 1
    print > Output
}
```

Finally, the `END` rule cleans up by closing the last output file:

```
END {
    close(Output)
}
```

---

Next: [Printing Nonduplicated Lines of Text](#Uniq-Program), Previous: [Splitting a Large File into Pieces](#Split-Program), Up: [Reinventing Wheels for Fun and Profit](#Clones)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.2.5 Duplicating Output into Multiple Files [¶](#Duplicating-Output-into-Multiple-Files)

The `tee` program is known as a “pipe fitting.” `tee` copies
its standard input to its standard output and also duplicates it to the
files named on the command line. Its usage is as follows:

```
tee [-a] file ...
```

The `-a` option tells `tee` to append to the named files, instead of
truncating them and starting over.

The `BEGIN` rule first makes a copy of all the command-line arguments
into an array named `copy`.
`ARGV[0]` is not needed, so it is not copied.
`tee` cannot use `ARGV` directly, because `awk` attempts to
process each file name in `ARGV` as input data.

If the first argument is `-a`, then the flag variable
`append` is set to true, and both `ARGV[1]` and
`copy[1]` are deleted. If `ARGC` is less than two, then no
file names were supplied and `tee` prints a usage message and exits.
Finally, `awk` is forced to read the standard input by setting
`ARGV[1]` to `"-"` and `ARGC` to two:

```
# tee.awk --- tee in awk
#
# Copy standard input to all named output files.
# Append content if -a option is supplied.

BEGIN {
    for (i = 1; i < ARGC; i++)
        copy[i] = ARGV[i]

    if (ARGV[1] == "-a") {
        append = 1
        delete ARGV[1]
        delete copy[1]
        ARGC--
    }
    if (ARGC < 2) {
        print "usage: tee [-a] file ..." > "/dev/stderr"
        exit 1
    }
    ARGV[1] = "-"
    ARGC = 2
}
```

The following single rule does all the work. Because there is no pattern, it is
executed for each line of input. The body of the rule simply prints the
line into each file on the command line, and then to the standard output:

```
{
    # moving the if outside the loop makes it run faster
    if (append)
        for (i in copy)
            print >> copy[i]
    else
        for (i in copy)
            print > copy[i]
    print
}
```

It is also possible to write the loop this way:

```
for (i in copy)
    if (append)
        print >> copy[i]
```

```
    else
        print > copy[i]
```

This is more concise, but it is also less efficient. The ‘`if`’ is
tested for each record and for each output file. By duplicating the loop
body, the ‘`if`’ is only tested once for each input record. If there are
N input records and M output files, the first method only
executes N ‘`if`’ statements, while the second executes
N`*`M ‘`if`’ statements.

Finally, the `END` rule cleans up by closing all the output files:

```
END {
    for (i in copy)
        close(copy[i])
}
```

---

Next: [Counting Things](#Wc-Program), Previous: [Duplicating Output into Multiple Files](#Tee-Program), Up: [Reinventing Wheels for Fun and Profit](#Clones)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.2.6 Printing Nonduplicated Lines of Text [¶](#Printing-Nonduplicated-Lines-of-Text)

The `uniq` utility reads sorted lines of data on its standard
input, and by default removes duplicate lines. In other words, it only
prints unique lines—hence the name. `uniq` has a number of
options. The usage is as follows:

```
uniq [-udc [-f n] [-s n]] [inputfile [outputfile]]
```

The options for `uniq` are:

`-d`
:   Print only repeated (duplicated) lines.

`-u`
:   Print only nonrepeated (unique) lines.

`-c`
:   Count lines. This option overrides `-d` and `-u`. Both repeated
    and nonrepeated lines are counted.

`-f n`
:   Skip n fields before comparing lines. The definition of fields
    is similar to `awk`’s default: nonwhitespace characters separated
    by runs of spaces and/or TABs.

`-s n`
:   Skip n characters before comparing lines. Any fields specified with
    `-f` are skipped first.

`inputfile`
:   Data is read from the input file named on the command line, instead of from
    the standard input.

`outputfile`
:   The generated output is sent to the named output file, instead of to the
    standard output.

Normally `uniq` behaves as if both the `-d` and
`-u` options are provided.

`uniq` uses the
`getopt()` library function
(see [Processing Command-Line Options](#Getopt-Function))
and the `join()` library function
(see [Merging an Array into a String](#Join-Function)).

The program begins with a `usage()` function and then a brief outline of
the options and their meanings in comments:

```
# uniq.awk --- do uniq in awk
#
# Requires getopt() and join() library functions
```

```
function usage()
{
    print("Usage: uniq [-udc [-f fields] [-s chars]] " \
          "[ in [ out ]]") > "/dev/stderr"
    exit 1
}

# -c    count lines. overrides -d and -u
# -d    only repeated lines
# -u    only nonrepeated lines
# -f n  skip n fields
# -s n  skip n characters, skip fields first
```

The POSIX standard for `uniq` allows options to start with
‘`+`’ as well as with ‘`-`’. An initial `BEGIN` rule
traverses the arguments changing any leading ‘`+`’ to ‘`-`’
so that the `getopt()` function can parse the options:

```
# As of 2020, '+' can be used as the option character in addition to '-'
# Previously allowed use of -N to skip fields and +N to skip
# characters is no longer allowed, and not supported by this version.

BEGIN {
    # Convert + to - so getopt can handle things
    for (i = 1; i < ARGC; i++) {
        first = substr(ARGV[i], 1, 1)
        if (ARGV[i] == "--" || (first != "-" && first != "+"))
            break
        else if (first == "+")
            # Replace "+" with "-"
            ARGV[i] = "-" substr(ARGV[i], 2)
    }
}
```

The next `BEGIN` rule deals with the command-line arguments and options.
If no options are supplied, then the default is taken, to print both
repeated and nonrepeated lines. The output file, if provided, is assigned
to `outputfile`. Early on, `outputfile` is initialized to the
standard output, `/dev/stdout`:

```
BEGIN {
    count = 1
    outputfile = "/dev/stdout"
    opts = "udcf:s:"
    while ((c = getopt(ARGC, ARGV, opts)) != -1) {
        if (c == "u")
            non_repeated_only++
        else if (c == "d")
            repeated_only++
        else if (c == "c")
            do_count++
        else if (c == "f")
            fcount = Optarg + 0
        else if (c == "s")
            charcount = Optarg + 0
        else
            usage()
    }

    for (i = 1; i < Optind; i++)
        ARGV[i] = ""

    if (repeated_only == 0 && non_repeated_only == 0)
        repeated_only = non_repeated_only = 1

    if (ARGC - Optind == 2) {
        outputfile = ARGV[ARGC - 1]
        ARGV[ARGC - 1] = ""
    }
}
```

The following function, `are_equal()`, compares the current line,
`$0`, to the previous line, `last`. It handles skipping fields
and characters. If no field count and no character count are specified,
`are_equal()` returns one or zero depending upon the result of a
simple string comparison of `last` and `$0`.

Otherwise, things get more complicated. If fields have to be skipped,
each line is broken into an array using `split()` (see [String-Manipulation Functions](#String-Functions)); the desired fields are then joined back into a line
using `join()`. The joined lines are stored in `clast` and
`cline`. If no fields are skipped, `clast` and `cline`
are set to `last` and `$0`, respectively. Finally, if
characters are skipped, `substr()` is used to strip off the leading
`charcount` characters in `clast` and `cline`. The two
strings are then compared and `are_equal()` returns the result:

```
function are_equal(    n, m, clast, cline, alast, aline)
{
    if (fcount == 0 && charcount == 0)
        return (last == $0)
```

```
    if (fcount > 0) {
        n = split(last, alast)
        m = split($0, aline)
        clast = join(alast, fcount+1, n)
        cline = join(aline, fcount+1, m)
    } else {
        clast = last
        cline = $0
    }
    if (charcount) {
        clast = substr(clast, charcount + 1)
        cline = substr(cline, charcount + 1)
    }
```

```
    return (clast == cline)
}
```

The following two rules are the body of the program. The first one is
executed only for the very first line of data. It sets `last` equal to
`$0`, so that subsequent lines of text have something to be compared to.

The second rule does the work. The variable `equal` is one or zero,
depending upon the results of `are_equal()`’s comparison. If `uniq`
is counting repeated lines, and the lines are equal, then it increments the `count` variable.
Otherwise, it prints the line and resets `count`,
because the two lines are not equal.

If `uniq` is not counting, and if the lines are equal, `count` is incremented.
Nothing is printed, as the point is to remove duplicates.
Otherwise, if `uniq` is counting repeated lines and more than
one line is seen, or if `uniq` is counting nonrepeated lines
and only one line is seen, then the line is printed, and `count`
is reset.

Finally, similar logic is used in the `END` rule to print the final
line of input data:

```
NR == 1 {
    last = $0
    next
}

{
    equal = are_equal()

    if (do_count) {    # overrides -d and -u
        if (equal)
            count++
        else {
            printf("%4d %s\n", count, last) > outputfile
            last = $0
            count = 1    # reset
        }
        next
    }

    if (equal)
        count++
    else {
        if ((repeated_only && count > 1) ||
            (non_repeated_only && count == 1))
                print last > outputfile
        last = $0
        count = 1
    }
}

END {
    if (do_count)
        printf("%4d %s\n", count, last) > outputfile
```

```
    else if ((repeated_only && count > 1) ||
            (non_repeated_only && count == 1))
        print last > outputfile
    close(outputfile)
}
```

As a side note, this program does not follow our recommended convention of naming
global variables with a leading capital letter. Doing that would
make the program a little easier to follow.

---

Previous: [Printing Nonduplicated Lines of Text](#Uniq-Program), Up: [Reinventing Wheels for Fun and Profit](#Clones)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.2.7 Counting Things [¶](#Counting-Things)

The `wc` (word count) utility counts lines, words, characters
and bytes in one or more input files.

* [Modern Character Sets](#Bytes-vs_002e-Characters)
* [A Brief Introduction To Extensions](#Using-extensions)
* [Code for `wc.awk`](#wc-program)

---

Next: [A Brief Introduction To Extensions](#Using-extensions), Up: [Counting Things](#Wc-Program)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.2.7.1 Modern Character Sets [¶](#Modern-Character-Sets)

In the early days of computing, single bytes were used for storing
characters. The most common character sets were ASCII and EBCDIC,
which each provided all the English upper- and lowercase letters, the 10
Hindu-Arabic numerals from 0 through 9, and a number of other standard
punctuation and control characters.

Today, the most popular character set in use is Unicode (of which ASCII
is a pure subset). Unicode provides tens of thousands of unique characters
(called *code points*) to cover most existing human languages, living
and dead. (Sadly, proposals to add some nonhuman ones, such as Klingon and
J.R.R. Tolkien’s Elvish languages, were not accepted).

To save space in files, Unicode code points are *encoded*, where each
character takes from one to four bytes in the file. UTF-8 is possibly
the most popular of such *multibyte encodings*.

The POSIX standard requires that `awk` function in terms
of characters, not bytes. Thus in `gawk`, `length()`,
`substr()`, `split()`, `match()` and the other string
functions (see [String-Manipulation Functions](#String-Functions)) all work in terms of characters in
the local character set, and not in terms of bytes. (Not all `awk`
implementations do so, though).

There is no standard, built-in way to distinguish characters from bytes
in an `awk` program. For an `awk` implementation of
`wc`, which needs to make such a distinction, we will have to
use an external extension.

---

Next: [Code for `wc.awk`](#wc-program), Previous: [Modern Character Sets](#Bytes-vs_002e-Characters), Up: [Counting Things](#Wc-Program)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.2.7.2 A Brief Introduction To Extensions [¶](#A-Brief-Introduction-To-Extensions)

Loadable extensions are presented in full detail in [Writing Extensions for `gawk`](#Dynamic-Extensions).
They provide a way to add functions to `gawk` which can call
out to other facilities written in C or C++.

For the purposes of
`wc.awk`, it’s enough to know that the extension is loaded
with the `@load` directive, and the additional function we
will use is called `mbs_length()`. This function returns the
number of bytes in a string, not the number of characters.

The `"mbs"` extension comes from the `gawkextlib`
project. See [The `gawkextlib` Project](#gawkextlib) for more information.

---

Previous: [A Brief Introduction To Extensions](#Using-extensions), Up: [Counting Things](#Wc-Program)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.2.7.3 Code for `wc.awk` [¶](#Code-for-wc_002eawk)

The usage for `wc` is as follows:

```
wc [-lwcm] [files ...]
```

If no files are specified on the command line, `wc` reads its standard
input. If there are multiple files, it also prints total counts for all
the files. The options and their meanings are as follows:

`-c`
:   Count only bytes.
    Once upon a time, the ‘`c`’ in this option stood for “characters.”
    But, as explained earlier, bytes and character are no longer synonymous
    with each other.

`-l`
:   Count only lines.

`-m`
:   Count only characters.

`-w`
:   Count only words.
    A “word” is a contiguous sequence of nonwhitespace characters, separated
    by spaces and/or TABs. Luckily, this is the normal way `awk` separates
    fields in its input data.

Implementing `wc` in `awk` is particularly elegant,
because `awk` does a lot of the work for us; it splits lines into
words (i.e., fields) and counts them, it counts lines (i.e., records),
and it can easily tell us how long a line is in characters.

This program uses the `getopt()` library function
(see [Processing Command-Line Options](#Getopt-Function))
and the file-transition functions
(see [Noting Data file Boundaries](#Filetrans-Function)).

This version has one notable difference from older versions of
`wc`: it always prints the counts in the order lines, words,
characters and bytes. Older versions note the order of the `-l`,
`-w`, and `-c` options on the command line, and print the
counts in that order. POSIX does not mandate this behavior, though.

The `BEGIN` rule does the argument processing. The variable
`print_total` is true if more than one file is named on the
command line:

```
# wc.awk --- count lines, words, characters, bytes

# Options:
#    -l    only count lines
#    -w    only count words
#    -c    only count bytes
#    -m    only count characters
#
# Default is to count lines, words, bytes
#
# Requires getopt() and file transition library functions
# Requires mbs extension from gawkextlib

@load "mbs"

BEGIN {
    # let getopt() print a message about
    # invalid options. we ignore them
    while ((c = getopt(ARGC, ARGV, "lwcm")) != -1) {
        if (c == "l")
            do_lines = 1
        else if (c == "w")
            do_words = 1
        else if (c == "c")
            do_bytes = 1
        else if (c == "m")
            do_chars = 1
    }
    for (i = 1; i < Optind; i++)
        ARGV[i] = ""

    # if no options, do lines, words, bytes
    if (! do_lines && ! do_words && ! do_chars && ! do_bytes)
        do_lines = do_words = do_bytes = 1

    print_total = (ARGC - i > 1)
}
```

The `beginfile()` function is simple; it just resets the counts of lines,
words, characters and bytes to zero, and saves the current file name in
`fname`:

```
function beginfile(file)
{
    lines = words = chars = bytes = 0
    fname = FILENAME
}
```

The `endfile()` function adds the current file’s numbers to the
running totals of lines, words, and characters. It then prints out those
numbers for the file that was just read. It relies on `beginfile()`
to reset the numbers for the following data file:

```
function endfile(file)
{
    tlines += lines
    twords += words
    tchars += chars
    tbytes += bytes
    if (do_lines)
        printf "\t%d", lines
```

```
    if (do_words)
        printf "\t%d", words
```

```
    if (do_chars)
        printf "\t%d", chars
    if (do_bytes)
        printf "\t%d", bytes
    printf "\t%s\n", fname
}
```

There is one rule that is executed for each line. It adds the length of
the record, plus one, to `chars`. Adding one plus the record length
is needed because the newline character separating records (the value
of `RS`) is not part of the record itself, and thus not included
in its length. Similarly, it adds the length of the record in bytes,
plus one, to `bytes`. Next, `lines` is incremented for each
line read, and `words` is incremented by the value of `NF`,
which is the number of “words” on this line:

```
# do per line
{
    chars += length($0) + 1    # get newline
    bytes += mbs_length($0) + 1
    lines++
    words += NF
}
```

Finally, the `END` rule simply prints the totals for all the files:

```
END {
    if (print_total) {
        if (do_lines)
            printf "\t%d", tlines
        if (do_words)
            printf "\t%d", twords
        if (do_chars)
            printf "\t%d", tchars
        if (do_bytes)
            printf "\t%d", tbytes
        print "\ttotal"
    }
}
```

---

Next: [Summary](#Programs-Summary), Previous: [Reinventing Wheels for Fun and Profit](#Clones), Up: [Practical `awk` Programs](#Sample-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 11.3 A Grab Bag of `awk` Programs [¶](#A-Grab-Bag-of-awk-Programs)

This section is a large “grab bag” of miscellaneous programs.
We hope you find them both interesting and enjoyable.

* [Finding Duplicated Words in a Document](#Dupword-Program)
* [An Alarm Clock Program](#Alarm-Program)
* [Transliterating Characters](#Translate-Program)
* [Printing Mailing Labels](#Labels-Program)
* [Generating Word-Usage Counts](#Word-Sorting)
* [Removing Duplicates from Unsorted Text](#History-Sorting)
* [Extracting Programs from Texinfo Source Files](#Extract-Program)
* [A Simple Stream Editor](#Simple-Sed)
* [An Easy Way to Use Library Functions](#Igawk-Program)
* [Finding Anagrams from a Dictionary](#Anagram-Program)
* [And Now for Something Completely Different](#Signature-Program)
* [Demonstrating Shortest and Longest Match Operators](#Shortest-match-program)

---

Next: [An Alarm Clock Program](#Alarm-Program), Up: [A Grab Bag of `awk` Programs](#Miscellaneous-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.1 Finding Duplicated Words in a Document [¶](#Finding-Duplicated-Words-in-a-Document)

A common error when writing large amounts of prose is to accidentally
duplicate words. Typically you will see this in text as something like “the
the program does the following…” When the text is online, often
the duplicated words occur at the end of one line and at the
beginning of
another, making them very difficult to spot.

This program, `dupword.awk`, scans through a file one line at a time
and looks for adjacent occurrences of the same word. It also saves the last
word on a line (in the variable `prev`) for comparison with the first
word on the next line.

The first two statements make sure that the line is all lowercase,
so that, for example, “The” and “the” compare equal to each other.
The next statement replaces nonalphanumeric and nonwhitespace characters
with spaces, so that punctuation does not affect the comparison either.
The characters are replaced with spaces so that formatting controls
don’t create nonsense words (e.g., the Texinfo ‘`@code{NF}`’
becomes ‘`codeNF`’ if punctuation is simply deleted). The record is
then resplit into fields, yielding just the actual words on the line,
and ensuring that there are no empty fields.

If there are no fields left after removing all the punctuation, the
current record is skipped. Otherwise, the program loops through each
word, comparing it to the previous one:

```
# dupword.awk --- find duplicate words in text

{
    $0 = tolower($0)
    gsub(/[^[:alnum:][:blank:]]/, " ");
    $0 = $0         # re-split
    if (NF == 0)
        next
    if ($1 == prev)
        printf("%s:%d: duplicate %s\n",
            FILENAME, FNR, $1)
    for (i = 2; i <= NF; i++)
        if ($i == $(i-1))
            printf("%s:%d: duplicate %s\n",
                FILENAME, FNR, $i)
    prev = $NF
}
```

---

Next: [Transliterating Characters](#Translate-Program), Previous: [Finding Duplicated Words in a Document](#Dupword-Program), Up: [A Grab Bag of `awk` Programs](#Miscellaneous-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.2 An Alarm Clock Program [¶](#An-Alarm-Clock-Program)

> *Nothing cures insomnia like a ringing alarm clock.*

— *Arnold Robbins*

> *Sleep is for web developers.*

— *Erik Quanstrom*

The following program is a simple “alarm clock” program.
You give it a time of day and an optional message. At the specified time,
it prints the message on the standard output. In addition, you can give it
the number of times to repeat the message as well as a delay between
repetitions.

This program uses the `getlocaltime()` function from
[Managing the Time of Day](#Getlocaltime-Function).

All the work is done in the `BEGIN` rule. The first part is argument
checking and setting of defaults: the delay, the count, and the message to
print. If the user supplied a message without the ASCII BEL
character (known as the “alert” character, `"\a"`), then it is added to
the message. (On many systems, printing the ASCII BEL generates an
audible alert. Thus, when the alarm goes off, the system calls attention
to itself in case the user is not looking at the computer.)
Just for a change, this program uses a `switch` statement
(see [The `switch` Statement](#Switch-Statement)), but the processing could be done with a series of
`if`-`else` statements instead.
Here is the program:

```
# alarm.awk --- set an alarm
#
# Requires getlocaltime() library function

# usage: alarm time [ "message" [ count [ delay ] ] ]

BEGIN {
    # Initial argument sanity checking
    usage1 = "usage: alarm time ['message' [count [delay]]]"
    usage2 = sprintf("\t(%s) time ::= hh:mm", ARGV[1])

    if (ARGC < 2) {
        print usage1 > "/dev/stderr"
        print usage2 > "/dev/stderr"
        exit 1
    }
    switch (ARGC) {
    case 5:
        delay = ARGV[4] + 0
        # fall through
    case 4:
        count = ARGV[3] + 0
        # fall through
    case 3:
        message = ARGV[2]
        break
    default:
        if (ARGV[1] !~ /[[:digit:]]?[[:digit:]]:[[:digit:]]{2}/) {
            print usage1 > "/dev/stderr"
            print usage2 > "/dev/stderr"
            exit 1
        }
        break
    }

    # set defaults for once we reach the desired time
    if (delay == 0)
        delay = 180    # 3 minutes
```

```
    if (count == 0)
        count = 5
```

```
    if (message == "")
        message = sprintf("\aIt is now %s!\a", ARGV[1])
    else if (index(message, "\a") == 0)
        message = "\a" message "\a"
```

The next section of code turns the alarm time into hours and minutes,
converts it (if necessary) to a 24-hour clock, and then turns that
time into a count of the seconds since midnight. Next it turns the current
time into a count of seconds since midnight. The difference between the two
is how long to wait before setting off the alarm:

```
    # split up alarm time
    split(ARGV[1], atime, ":")
    hour = atime[1] + 0    # force numeric
    minute = atime[2] + 0  # force numeric

    # get current broken down time
    getlocaltime(now)

    # if time given is 12-hour hours and it's after that
    # hour, e.g., `alarm 5:30' at 9 a.m. means 5:30 p.m.,
    # then add 12 to real hour
    if (hour < 12 && now["hour"] > hour)
        hour += 12

    # set target time in seconds since midnight
    target = (hour * 60 * 60) + (minute * 60)

    # get current time in seconds since midnight
    current = (now["hour"] * 60 * 60) + \
               (now["minute"] * 60) + now["second"]

    # how long to sleep for
    naptime = target - current
    if (naptime <= 0) {
        print "alarm: time is in the past!" > "/dev/stderr"
        exit 1
    }
```

Finally, the program uses the `system()` function
(see [Input/Output Functions](#I_002fO-Functions))
to call the `sleep` utility. The `sleep` utility simply pauses
for the given number of seconds. If the exit status is not zero,
the program assumes that `sleep` was interrupted and exits. If
`sleep` exited with an OK status (zero), then the program prints the
message in a loop, again using `sleep` to delay for however many
seconds are necessary:

```
    # zzzzzz..... go away if interrupted
    if (system(sprintf("sleep %d", naptime)) != 0)
        exit 1

    # time to notify!
    command = sprintf("sleep %d", delay)
    for (i = 1; i <= count; i++) {
        print message
        # if sleep command interrupted, go away
        if (system(command) != 0)
            break
    }

    exit 0
}
```

---

Next: [Printing Mailing Labels](#Labels-Program), Previous: [An Alarm Clock Program](#Alarm-Program), Up: [A Grab Bag of `awk` Programs](#Miscellaneous-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.3 Transliterating Characters [¶](#Transliterating-Characters)

The system `tr` utility transliterates characters. For example, it is
often used to map uppercase letters into lowercase for further processing:

```
generate data | tr 'A-Z' 'a-z' | process data ...
```

`tr` requires two lists of characters.[80](#FOOT80) When processing the input, the
first character in the first list is replaced with the first character
in the second list, the second character in the first list is replaced
with the second character in the second list, and so on. If there are
more characters in the “from” list than in the “to” list, the last
character of the “to” list is used for the remaining characters in the
“from” list.

Once upon a time,
a user proposed adding a transliteration function
to `gawk`.
The following program was written to
prove that character transliteration could be done with a user-level
function. This program is not as complete as the system `tr` utility,
but it does most of the job.

The `translate` program was written long before `gawk`
acquired the ability to split each character in a string into separate
array elements. Thus, it makes repeated use of the `substr()`,
`index()`, and `gsub()` built-in functions (see [String-Manipulation Functions](#String-Functions)). There are two functions. The first, `stranslate()`,
takes three arguments:

`from`
:   A list of characters from which to translate

`to`
:   A list of characters to which to translate

`target`
:   The string on which to do the translation

Associative arrays make the translation part fairly easy. `t_ar` holds
the “to” characters, indexed by the “from” characters. Then a simple
loop goes through `from`, one character at a time. For each character
in `from`, if the character appears in `target`,
it is replaced with the corresponding `to` character.

The `translate()` function calls `stranslate()`, using `$0`
as the target. The main program sets two global variables, `FROM` and
`TO`, from the command line, and then changes `ARGV` so that
`awk` reads from the standard input.

Finally, the processing rule simply calls `translate()` for each record:

```
# translate.awk --- do tr-like stuff

# Bugs: does not handle things like tr A-Z a-z; it has
# to be spelled out. However, if `to' is shorter than `from',
# the last character in `to' is used for the rest of `from'.

function stranslate(from, to, target,     lf, lt, ltarget, t_ar, i, c,
                                                               result)
{
    lf = length(from)
    lt = length(to)
    ltarget = length(target)
    for (i = 1; i <= lt; i++)
        t_ar[substr(from, i, 1)] = substr(to, i, 1)
    if (lt < lf)
        for (; i <= lf; i++)
            t_ar[substr(from, i, 1)] = substr(to, lt, 1)
    for (i = 1; i <= ltarget; i++) {
        c = substr(target, i, 1)
        if (c in t_ar)
            c = t_ar[c]
        result = result c
    }
    return result
}

function translate(from, to)
{
    return $0 = stranslate(from, to, $0)
}

# main program
BEGIN {
```

```
    if (ARGC < 3) {
        print "usage: translate from to" > "/dev/stderr"
        exit
    }
```

```
    FROM = ARGV[1]
    TO = ARGV[2]
    ARGC = 2
    ARGV[1] = "-"
}

{
    translate(FROM, TO)
    print
}
```

It is possible to do character transliteration in a user-level
function, but it is not necessarily efficient, and we (the `gawk`
developers) started to consider adding a built-in function. However,
shortly after writing this program, we learned that Brian Kernighan
had added the `toupper()` and `tolower()` functions to his
`awk` (see [String-Manipulation Functions](#String-Functions)). These functions handle the
vast majority of the cases where character transliteration is necessary,
and so we chose to simply add those functions to `gawk` as well
and then leave well enough alone.

An obvious improvement to this program would be to set up the
`t_ar` array only once, in a `BEGIN` rule. However, this
assumes that the “from” and “to” lists
will never change throughout the lifetime of the program.

Another obvious improvement is to enable the use of ranges,
such as ‘`a-z`’, as allowed by the `tr` utility.
Look at the code for `cut.awk` (see [Cutting Out Fields and Columns](#Cut-Program))
for inspiration.

---

Next: [Generating Word-Usage Counts](#Word-Sorting), Previous: [Transliterating Characters](#Translate-Program), Up: [A Grab Bag of `awk` Programs](#Miscellaneous-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.4 Printing Mailing Labels [¶](#Printing-Mailing-Labels)

Here is a “real-world”[81](#FOOT81)
program. This
script reads lists of names and
addresses and generates mailing labels. Each page of labels has 20 labels
on it, two across and 10 down. The addresses are guaranteed to be no more
than five lines of data. Each address is separated from the next by a blank
line.

The basic idea is to read 20 labels’ worth of data. Each line of each label
is stored in the `line` array. The single rule takes care of filling
the `line` array and printing the page when 20 labels have been read.

The `BEGIN` rule simply sets `RS` to the empty string, so that
`awk` splits records at blank lines
(see [How Input Is Split into Records](#Records)).
It sets `MAXLINES` to 100, because 100 is the maximum number
of lines on the page
(20 \* 5 = 100).

Most of the work is done in the `printpage()` function.
The label lines are stored sequentially in the `line` array. But they
have to print horizontally: `line[1]` next to `line[6]`,
`line[2]` next to `line[7]`, and so on. Two loops
accomplish this. The outer loop, controlled by `i`, steps through
every 10 lines of data; this is each row of labels. The inner loop,
controlled by `j`, goes through the lines within the row.
As `j` goes from 0 to 4, ‘`i+j`’ is the `j`th line in
the row, and ‘`i+j+5`’ is the entry next to it. The output ends up
looking something like this:

```
line 1          line 6
line 2          line 7
line 3          line 8
line 4          line 9
line 5          line 10
...
```

The `printf` format string ‘`%-41s`’ left-aligns
the data and prints it within a fixed-width field.

As a final note, an extra blank line is printed at lines 21 and 61, to keep
the output lined up on the labels. This is dependent on the particular
brand of labels in use when the program was written. You will also note
that there are two blank lines at the top and two blank lines at the bottom.

The `END` rule arranges to flush the final page of labels; there may
not have been an even multiple of 20 labels in the data:

```
# labels.awk --- print mailing labels

# Each label is 5 lines of data that may have blank lines.
# The label sheets have 2 blank lines at the top and 2 at
# the bottom.

BEGIN    { RS = "" ; MAXLINES = 100 }

function printpage(    i, j)
{
    if (Nlines <= 0)
        return

    printf "\n\n"        # header

    for (i = 1; i <= Nlines; i += 10) {
        if (i == 21 || i == 61)
            print ""
        for (j = 0; j < 5; j++) {
            if (i + j > MAXLINES)
                break
            printf "   %-41s %s\n", line[i+j], line[i+j+5]
        }
        print ""
    }

    printf "\n\n"        # footer

    delete line
}

# main rule
{
    if (Count >= 20) {
        printpage()
        Count = 0
        Nlines = 0
    }
    n = split($0, a, "\n")
    for (i = 1; i <= n; i++)
        line[++Nlines] = a[i]
    for (; i <= 5; i++)
        line[++Nlines] = ""
    Count++
}

END {
    printpage()
}
```

---

Next: [Removing Duplicates from Unsorted Text](#History-Sorting), Previous: [Printing Mailing Labels](#Labels-Program), Up: [A Grab Bag of `awk` Programs](#Miscellaneous-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.5 Generating Word-Usage Counts [¶](#Generating-Word_002dUsage-Counts)

When working with large amounts of text, it can be interesting to know
how often different words appear. For example, an author may overuse
certain words, in which case he or she might wish to find synonyms to substitute
for words that appear too often. This subsection develops a
program for counting words and presenting the frequency information
in a useful format.

At first glance, a program like this would seem to do the job:

```
# wordfreq-first-try.awk --- print list of word frequencies

{
    for (i = 1; i <= NF; i++)
        freq[$i]++
}
```

```
END {
    for (word in freq)
        printf "%s\t%d\n", word, freq[word]
}
```

The program relies on `awk`’s default field-splitting
mechanism to break each line up into “words” and uses an
associative array named `freq`, indexed by each word, to count
the number of times the word occurs. In the `END` rule,
it prints the counts.

This program has several problems that prevent it from being
useful on real text files:

* The `awk` language considers upper- and lowercase characters to be
  distinct. Therefore, “bartender” and “Bartender” are not treated
  as the same word. This is undesirable, because words are capitalized
  if they begin sentences in normal text, and a frequency analyzer should
  not be sensitive to capitalization.
* Words are detected using the `awk` convention that fields are
  separated just by whitespace. Other characters in the input (except
  newlines) don’t have any special meaning to `awk`. This means that
  punctuation characters count as part of words.
* The output does not come out in any useful order. You’re more likely to be
  interested in which words occur most frequently or in having an alphabetized
  table of how frequently each word occurs.

The first problem can be solved by using `tolower()` to remove case
distinctions. The second problem can be solved by using `gsub()`
to remove punctuation characters. Finally, we solve the third problem
by using the system `sort` utility to process the output of the
`awk` script. Here is the new version of the program:

```
# wordfreq.awk --- print list of word frequencies

{
    $0 = tolower($0)    # remove case distinctions
    # remove punctuation
    gsub(/[^[:alnum:]_[:blank:]]/, "", $0)
    for (i = 1; i <= NF; i++)
        freq[$i]++
}

END {
    for (word in freq)
        printf "%s\t%d\n", word, freq[word]
}
```

The regexp `/[^[:alnum:]_[:blank:]]/` might have been written
`/[[:punct:]]/`, but then underscores would also be removed,
and we want to keep them.

Assuming we have saved this program in a file named `wordfreq.awk`,
and that the data is in `file1`, the following pipeline:

```
awk -f wordfreq.awk file1 | sort -k 2nr
```

produces a table of the words appearing in `file1` in order of
decreasing frequency.

The `awk` program suitably massages the
data and produces a word frequency table, which is not ordered.
The `awk` script’s output is then sorted by the `sort`
utility and printed on the screen.

The options given to `sort`
specify a sort that uses the second field of each input line (skipping
one field), that the sort keys should be treated as numeric quantities
(otherwise ‘`15`’ would come before ‘`5`’), and that the sorting
should be done in descending (reverse) order.

The `sort` could even be done from within the program, by changing
the `END` action to:

```
END {
    sort = "sort -k 2nr"
    for (word in freq)
        printf "%s\t%d\n", word, freq[word] | sort
    close(sort)
}
```

This way of sorting must be used on systems that do not
have true pipes at the command-line (or batch-file) level.
See the general operating system documentation for more information on how
to use the `sort` program.

---

Next: [Extracting Programs from Texinfo Source Files](#Extract-Program), Previous: [Generating Word-Usage Counts](#Word-Sorting), Up: [A Grab Bag of `awk` Programs](#Miscellaneous-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.6 Removing Duplicates from Unsorted Text [¶](#Removing-Duplicates-from-Unsorted-Text)

The `uniq` program
(see [Printing Nonduplicated Lines of Text](#Uniq-Program))
removes duplicate lines from *sorted* data.

Suppose, however, you need to remove duplicate lines from a data file but
that you want to preserve the order the lines are in. A good example of
this might be a shell history file. The history file keeps a copy of all
the commands you have entered, and it is not unusual to repeat a command
several times in a row. Occasionally you might want to compact the history
by removing duplicate entries. Yet it is desirable to maintain the order
of the original commands.

This simple program does the job. It uses two arrays. The `data`
array is indexed by the text of each line.
For each line, `data[$0]` is incremented.
If a particular line has not
been seen before, then `data[$0]` is zero.
In this case, the text of the line is stored in `lines[count]`.
Each element of `lines` is a unique command, and the indices of
`lines` indicate the order in which those lines are encountered.
The `END` rule simply prints out the lines, in order:

```
# histsort.awk --- compact a shell history file
# Thanks to Byron Rakitzis for the general idea
```

```
{
    if (data[$0]++ == 0)
        lines[++count] = $0
}
```

```

```

```
END {
    for (i = 1; i <= count; i++)
        print lines[i]
}
```

This program also provides a foundation for generating other useful
information. For example, using the following `print` statement in the
`END` rule indicates how often a particular command is used:

```
print data[lines[i]], lines[i]
```

This works because `data[$0]` is incremented each time a line is
seen.

Rick van Rein offers the following one-liner to do the same job of
removing duplicates from unsorted text:

```
awk '{ if (! seen[$0]++) print }'
```

This can be simplified even further, at the risk of becoming
almost too obscure:

```
awk '! seen[$0]++'
```

This version uses the expression as a pattern, relying on
`awk`’s default action of printing the line when
the pattern is true.

---

Next: [A Simple Stream Editor](#Simple-Sed), Previous: [Removing Duplicates from Unsorted Text](#History-Sorting), Up: [A Grab Bag of `awk` Programs](#Miscellaneous-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.7 Extracting Programs from Texinfo Source Files [¶](#Extracting-Programs-from-Texinfo-Source-Files)

Both this chapter and the previous chapter
([A Library of `awk` Functions](#Library-Functions))
present a large number of `awk` programs.
If you want to experiment with these programs, it is tedious to type
them in by hand. Here we present a program that can extract parts of a
Texinfo input file into separate files.

This Web page is written in [Texinfo](https://www.gnu.org/software/texinfo/),
the GNU Project’s document formatting language.
A single Texinfo source file can be used to produce both
printed documentation, with TeX, and online documentation.
(Texinfo is fully documented in the book
Texinfo—The GNU Documentation Format,
available from the Free Software Foundation,
and also available [online](https://www.gnu.org/software/texinfo/manual/texinfo/).)

For our purposes, it is enough to know three things about Texinfo input
files:

* The “at” symbol (‘`@`’) is special in Texinfo, much as
  the backslash (‘`\`’) is in C
  or `awk`. Literal ‘`@`’ symbols are represented in Texinfo source
  files as ‘`@@`’.
* Comments start with either ‘`@c`’ or ‘`@comment`’.
  The file-extraction program works by using special comments that start
  at the beginning of a line.
* Lines containing ‘`@group`’ and ‘`@end group`’ commands bracket
  example text that should not be split across a page boundary.
  (Unfortunately, TeX isn’t always smart enough to do things exactly right,
  so we have to give it some help.)

The following program, `extract.awk`, reads through a Texinfo source
file and does two things, based on the special comments.
Upon seeing ‘`@c system …`’,
it runs a command, by extracting the command text from the
control line and passing it on to the `system()` function
(see [Input/Output Functions](#I_002fO-Functions)).
Upon seeing ‘`@c file filename`’, each subsequent line is sent to
the file filename, until ‘`@c endfile`’ is encountered.
The rules in `extract.awk` match either ‘`@c`’ or
‘`@comment`’ by letting the ‘`omment`’ part be optional.
Lines containing ‘`@group`’ and ‘`@end group`’ are simply removed.
`extract.awk` uses the `join()` library function
(see [Merging an Array into a String](#Join-Function)).

The example programs in the online Texinfo source for GAWK: Effective AWK Programming
(`gawk.texi`) have all been bracketed inside ‘`file`’ and
‘`endfile`’ lines. The `gawk` distribution uses a copy of
`extract.awk` to extract the sample programs and install many
of them in a standard directory where `gawk` can find them.
The Texinfo file looks something like this:

```
...
This program has a @code{BEGIN} rule
that prints a nice message:

@example
@c file examples/messages.awk
BEGIN @{ print "Don't panic!" @}
@c endfile
@end example

It also prints some final advice:

@example
@c file examples/messages.awk
END @{ print "Always avoid bored archaeologists!" @}
@c endfile
@end example
...
```

`extract.awk` begins by setting `IGNORECASE` to one, so that
mixed upper- and lowercase letters in the directives won’t matter.

The first rule handles calling `system()`, checking that a command is
given (`NF` is at least three) and also checking that the command
exits with a zero exit status, signifying OK:

```
# extract.awk --- extract files and run programs from Texinfo files

BEGIN    { IGNORECASE = 1 }

/^@c(omment)?[ \t]+system/ {
    if (NF < 3) {
        e = ("extract: " FILENAME ":" FNR)
        e = (e  ": badly formed `system' line")
        print e > "/dev/stderr"
        next
    }
    $1 = ""
    $2 = ""
    stat = system($0)
    if (stat != 0) {
        e = ("extract: " FILENAME ":" FNR)
        e = (e ": warning: system returned " stat)
        print e > "/dev/stderr"
    }
}
```

The variable `e` is used so that the rule
fits nicely on the screen.

The second rule handles moving data into files. It verifies that a
file name is given in the directive. If the file named is not the
current file, then the current file is closed. Keeping the current file
open until a new file is encountered allows the use of the ‘`>`’
redirection for printing the contents, keeping open-file management
simple.

The `for` loop does the work. It reads lines using `getline`
(see [Explicit Input with `getline`](#Getline)).
For an unexpected end-of-file, it calls the `unexpected_eof()`
function. If the line is an “endfile” line, then it breaks out of
the loop.
If the line is an ‘`@group`’ or ‘`@end group`’ line, then it
ignores it and goes on to the next line.
Similarly, comments within examples are also ignored.

Most of the work is in the following few lines. If the line has no ‘`@`’
symbols, the program can print it directly.
Otherwise, each leading ‘`@`’ must be stripped off.
To remove the ‘`@`’ symbols, the line is split into separate elements of
the array `a`, using the `split()` function
(see [String-Manipulation Functions](#String-Functions)).
The ‘`@`’ symbol is used as the separator character.
Each element of `a` that is empty indicates two successive ‘`@`’
symbols in the original line. For each two empty elements (‘`@@`’ in
the original file), we have to add a single ‘`@`’ symbol back in.

When the processing of the array is finished, `join()` is called with the
value of `SUBSEP` (see [Multidimensional Arrays](#Multidimensional)),
to rejoin the pieces back into a single
line. That line is then printed to the output file:

```
/^@c(omment)?[ \t]+file/ {
    if (NF != 3) {
        e = ("extract: " FILENAME ":" FNR ": badly formed `file' line")
        print e > "/dev/stderr"
        next
    }
    if ($3 != curfile) {
        if (curfile != "")
            filelist[curfile] = 1   # save to close later
        curfile = $3
    }

    for (;;) {
        if ((getline line) <= 0)
            unexpected_eof()
        if (line ~ /^@c(omment)?[ \t]+endfile/)
            break
        else if (line ~ /^@(end[ \t]+)?group/)
            continue
        else if (line ~ /^@c(omment+)?[ \t]+/)
            continue
        if (index(line, "@") == 0) {
            print line > curfile
            continue
        }
        n = split(line, a, "@")
        # if a[1] == "", means leading @,
        # don't add one back in.
        for (i = 2; i <= n; i++) {
            if (a[i] == "") { # was an @@
                a[i] = "@"
                if (a[i+1] == "")
                    i++
            }
        }
```

```
        print join(a, 1, n, SUBSEP) > curfile
    }
}
```

An important thing to note is the use of the ‘`>`’ redirection.
Output done with ‘`>`’ only opens the file once; it stays open and
subsequent output is appended to the file
(see [Redirecting Output of `print` and `printf`](#Redirection)).
This makes it easy to mix program text and explanatory prose for the same
sample source file (as has been done here!) without any hassle. The file is
only closed when a new data file name is encountered or at the end of the
input file.

When a new file name is encountered, instead of closing the file,
the program saves the name of the current file in `filelist`.
This makes it possible to interleave the code for more than one file in
the Texinfo input file. (Previous versions of this program *did*
close the file. But because of the ‘`>`’ redirection, a file whose
parts were not all one after the other ended up getting clobbered.)
An `END` rule then closes all the open files when processing
is finished:

```
END {
    close(curfile)          # close the last one
    for (f in filelist)     # close all the rest
        close(f)
}
```

Finally, the function `unexpected_eof()` prints an appropriate
error message and then exits:

```
function unexpected_eof()
{
    printf("extract: %s:%d: unexpected EOF or error\n",
                     FILENAME, FNR) > "/dev/stderr"
    exit 1
}
```

---

Next: [An Easy Way to Use Library Functions](#Igawk-Program), Previous: [Extracting Programs from Texinfo Source Files](#Extract-Program), Up: [A Grab Bag of `awk` Programs](#Miscellaneous-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.8 A Simple Stream Editor [¶](#A-Simple-Stream-Editor)

The `sed` utility is a *stream editor*, a program that reads a
stream of data, makes changes to it, and passes it on.
It is often used to make global changes to a large file or to a stream
of data generated by a pipeline of commands.
Although `sed` is a complicated program in its own right, its most common
use is to perform global substitutions in the middle of a pipeline:

```
command1 < orig.data | sed 's/old/new/g' | command2 > result
```

Here, ‘`s/old/new/g`’ tells `sed` to look for the regexp
‘`old`’ on each input line and globally replace it with the text
‘`new`’ (i.e., all the occurrences on a line). This is similar to
`awk`’s `gsub()` function
(see [String-Manipulation Functions](#String-Functions)).

The following program, `awksed.awk`, accepts at least two command-line
arguments: the pattern to look for and the text to replace it with. Any
additional arguments are treated as data file names to process. If none
are provided, the standard input is used:

```
# awksed.awk --- do s/foo/bar/g using just print
#    Thanks to Michael Brennan for the idea

function usage()
{
    print "usage: awksed pat repl [files...]" > "/dev/stderr"
    exit 1
}
```

```
BEGIN {
    # validate arguments
    if (ARGC < 3)
        usage()
```

```
    RS = ARGV[1]
    ORS = ARGV[2]

    # don't use arguments as files
    ARGV[1] = ARGV[2] = ""
}
```

```
# look ma, no hands!
{
    if (RT == "")
        printf "%s", $0
    else
        print
}
```

The program relies on `gawk`’s ability to have `RS` be a regexp,
as well as on the setting of `RT` to the actual text that terminates the
record (see [How Input Is Split into Records](#Records)).

The idea is to have `RS` be the pattern to look for. `gawk`
automatically sets `$0` to the text between matches of the pattern.
This is text that we want to keep, unmodified. Then, by setting `ORS`
to the replacement text, a simple `print` statement outputs the
text we want to keep, followed by the replacement text.

There is one wrinkle to this scheme, which is what to do if the last record
doesn’t end with text that matches `RS`. Using a `print`
statement unconditionally prints the replacement text, which is not correct.
However, if the file did not end in text that matches `RS`, `RT`
is set to the null string. In this case, we can print `$0` using
`printf`
(see [Using `printf` Statements for Fancier Printing](#Printf)).

The `BEGIN` rule handles the setup, checking for the right number
of arguments and calling `usage()` if there is a problem. Then it sets
`RS` and `ORS` from the command-line arguments and sets
`ARGV[1]` and `ARGV[2]` to the null string, so that they are
not treated as file names
(see [Using `ARGC` and `ARGV`](#ARGC-and-ARGV)).

The `usage()` function prints an error message and exits.
Finally, the single rule handles the printing scheme outlined earlier,
using `print` or `printf` as appropriate, depending upon the
value of `RT`.

---

Next: [Finding Anagrams from a Dictionary](#Anagram-Program), Previous: [A Simple Stream Editor](#Simple-Sed), Up: [A Grab Bag of `awk` Programs](#Miscellaneous-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.9 An Easy Way to Use Library Functions [¶](#An-Easy-Way-to-Use-Library-Functions)

In [Including Other Files into Your Program](#Include-Files), we saw how `gawk` provides a built-in
file-inclusion capability. However, this is a `gawk` extension.
This section provides the motivation for making file inclusion
available for standard `awk`, and shows how to do it using a
combination of shell and `awk` programming.

Using library functions in `awk` can be very beneficial. It
encourages code reuse and the writing of general functions. Programs are
smaller and therefore clearer.
However, using library functions is only easy when writing `awk`
programs; it is painful when running them, requiring multiple `-f`
options. If `gawk` is unavailable, then so too is the `AWKPATH`
environment variable and the ability to put `awk` functions into a
library directory (see [Command-Line Options](#Options)).
It would be nice to be able to write programs in the following manner:

```
# library functions
@include getopt.awk
@include join.awk
...

# main program
BEGIN {
    while ((c = getopt(ARGC, ARGV, "a:b:cde")) != -1)
        ...
    ...
}
```

The following program, `igawk.sh`, provides this service.
It simulates `gawk`’s searching of the `AWKPATH` variable
and also allows *nested* includes (i.e., a file that is included
with `@include` can contain further `@include` directives).
`igawk` makes an effort to only include files once, so that nested
includes don’t accidentally include a library function twice.

`igawk` should behave just like `gawk` externally. This
means it should accept all of `gawk`’s command-line arguments,
including the ability to have multiple source files specified via
`-f` and the ability to mix command-line and library source files.

The program is written using the POSIX Shell (`sh`) command
language.[82](#FOOT82) It works as follows:

1. Loop through the arguments, saving anything that doesn’t represent
   `awk` source code for later, when the expanded program is run.
2. For any arguments that do represent `awk` text, put the arguments into
   a shell variable that will be expanded. There are two cases:
   1. Literal text, provided with `-e` or `--source`. This
      text is just appended directly.
   2. Source file names, provided with `-f`. We use a neat trick and
      append ‘`@include filename`’ to the shell variable’s contents.
      Because the file-inclusion program works the way `gawk` does, this
      gets the text of the file included in the program at the correct point.
3. Run an `awk` program (naturally) over the shell variable’s contents to expand
   `@include` directives. The expanded program is placed in a second
   shell variable.
4. Run the expanded program with `gawk` and any other original command-line
   arguments that the user supplied (such as the data file names).

This program uses shell variables extensively: for storing command-line arguments and
the text of the `awk` program that will expand the user’s program, for the
user’s original program, and for the expanded program. Doing so removes some
potential problems that might arise were we to use temporary files instead,
at the cost of making the script somewhat more complicated.

The initial part of the program turns on shell tracing if the first
argument is ‘`debug`’.

The next part loops through all the command-line arguments.
There are several cases of interest:

`--`
:   This ends the arguments to `igawk`. Anything else should be passed on
    to the user’s `awk` program without being evaluated.

`-W`
:   This indicates that the next option is specific to `gawk`. To make
    argument processing easier, the `-W` is appended to the front of the
    remaining arguments and the loop continues. (This is an `sh`
    programming trick. Don’t worry about it if you are not familiar with
    `sh`.)

`-v`, `-F`
:   These are saved and passed on to `gawk`.

`-f`, `--file`, `--file=`, `-Wfile=`
:   The file name is appended to the shell variable `program` with an
    `@include` directive.
    The `expr` utility is used to remove the leading option part of the
    argument (e.g., ‘`--file=`’).
    (Typical `sh` usage would be to use the `echo` and `sed`
    utilities to do this work. Unfortunately, some versions of `echo` evaluate
    escape sequences in their arguments, possibly mangling the program text.
    Using `expr` avoids this problem.)

`--source`, `--source=`, `-Wsource=`
:   The source text is appended to `program`.

`--version`, `-Wversion`
:   `igawk` prints its version number, runs ‘`gawk --version`’
    to get the `gawk` version information, and then exits.

If none of the `-f`, `--file`, `-Wfile`, `--source`,
or `-Wsource` arguments are supplied, then the first nonoption argument
should be the `awk` program. If there are no command-line
arguments left, `igawk` prints an error message and exits.
Otherwise, the first argument is appended to `program`.
In any case, after the arguments have been processed,
the shell variable
`program` contains the complete text of the original `awk`
program.

The program is as follows:

```
#! /bin/sh
# igawk --- like gawk but do @include processing

if [ "$1" = debug ]
then
    set -x
    shift
fi

# A literal newline, so that program text is formatted correctly
n='
'

# Initialize variables to empty
program=
opts=

while [ $# -ne 0 ] # loop over arguments
do
    case $1 in
    --)     shift
            break ;;

    -W)     shift
            # The ${x?'message here'} construct prints a
            # diagnostic if $x is the null string
            set -- -W"${@?'missing operand'}"
            continue ;;

    -[vF])  opts="$opts $1 '${2?'missing operand'}'"
            shift ;;

    -[vF]*) opts="$opts '$1'" ;;

    -f)     program="$program$n@include ${2?'missing operand'}"
            shift ;;

    -f*)    f=$(expr "$1" : '-f\(.*\)')
            program="$program$n@include $f" ;;

    -[W-]file=*)
            f=$(expr "$1" : '-.file=\(.*\)')
            program="$program$n@include $f" ;;

    -[W-]file)
            program="$program$n@include ${2?'missing operand'}"
            shift ;;

    -[W-]source=*)
            t=$(expr "$1" : '-.source=\(.*\)')
            program="$program$n$t" ;;

    -[W-]source)
            program="$program$n${2?'missing operand'}"
            shift ;;

    -[W-]version)
            echo igawk: version 3.0 1>&2
            gawk --version
            exit 0 ;;

    -[W-]*) opts="$opts '$1'" ;;

    *)      break ;;
    esac
    shift
done

if [ -z "$program" ]
then
     program=${1?'missing program'}
     shift
fi

# At this point, `program' has the program.
```

The `awk` program to process `@include` directives
is stored in the shell variable `expand_prog`. Doing this keeps
the shell script readable. The `awk` program
reads through the user’s program, one line at a time, using `getline`
(see [Explicit Input with `getline`](#Getline)). The input
file names and `@include` directives are managed using a stack.
As each `@include` is encountered, the current file name is
“pushed” onto the stack and the file named in the `@include`
directive becomes the current file name. As each file is finished,
the stack is “popped,” and the previous input file becomes the current
input file again. The process is started by making the original file
the first one on the stack.

The `pathto()` function does the work of finding the full path to
a file. It simulates `gawk`’s behavior when searching the
`AWKPATH` environment variable
(see [The `AWKPATH` Environment Variable](#AWKPATH-Variable)).
If a file name has a ‘`/`’ in it, no path search is done.
Similarly, if the file name is `"-"`, then that string is
used as-is. Otherwise,
the file name is concatenated with the name of each directory in
the path, and an attempt is made to open the generated file name.
The only way to test if a file can be read in `awk` is to go
ahead and try to read it with `getline`; this is what `pathto()`
does.[83](#FOOT83)
If the file can be read, it is closed and the file name
is returned:

```
expand_prog='

function pathto(file,    i, t, junk)
{
    if (index(file, "/") != 0)
        return file

    if (file == "-")
        return file

    for (i = 1; i <= ndirs; i++) {
        t = (pathlist[i] "/" file)
```

```
        if ((getline junk < t) > 0) {
            # found it
            close(t)
            return t
        }
```

```
    }
    return ""
}
```

The main program is contained inside one `BEGIN` rule. The first thing it
does is set up the `pathlist` array that `pathto()` uses. After
splitting the path on ‘`:`’, null elements are replaced with `"."`,
which represents the current directory:

```
BEGIN {
    path = ENVIRON["AWKPATH"]
    ndirs = split(path, pathlist, ":")
    for (i = 1; i <= ndirs; i++) {
        if (pathlist[i] == "")
            pathlist[i] = "."
    }
```

The stack is initialized with `ARGV[1]`, which will be `"/dev/stdin"`.
The main loop comes next. Input lines are read in succession. Lines that
do not start with `@include` are printed verbatim.
If the line does start with `@include`, the file name is in `$2`.
`pathto()` is called to generate the full path. If it cannot, then the program
prints an error message and continues.

The next thing to check is if the file is included already. The
`processed` array is indexed by the full file name of each included
file and it tracks this information for us. If the file is
seen again, a warning message is printed. Otherwise, the new file name is
pushed onto the stack and processing continues.

Finally, when `getline` encounters the end of the input file, the file
is closed and the stack is popped. When `stackptr` is less than zero,
the program is done:

```
    stackptr = 0
    input[stackptr] = ARGV[1] # ARGV[1] is first file

    for (; stackptr >= 0; stackptr--) {
        while ((getline < input[stackptr]) > 0) {
            if (tolower($1) != "@include") {
                print
                continue
            }
            fpath = pathto($2)
            if (fpath == "") {
                printf("igawk: %s:%d: cannot find %s\n",
                    input[stackptr], FNR, $2) > "/dev/stderr"
                continue
            }
            if (! (fpath in processed)) {
                processed[fpath] = input[stackptr]
                input[++stackptr] = fpath  # push onto stack
            } else
                print $2, "included in", input[stackptr],
                    "already included in",
                    processed[fpath] > "/dev/stderr"
        }
        close(input[stackptr])
    }
}'  # closing single quote ends `expand_prog' variable

processed_program=$(gawk -- "$expand_prog" /dev/stdin << EOF
$program
EOF
)
```

The shell construct ‘`command << marker`’ is called
a *here document*. Everything in the shell script up to the
marker is fed to command as input. The shell processes
the contents of the here document for variable and command substitution
(and possibly other things as well, depending upon the shell).

The shell construct ‘`$(…)`’ is called *command substitution*.
The output of the command inside the parentheses is substituted
into the command line.
Because the result is used in a variable assignment,
it is saved as a single string, even if the results contain whitespace.

The expanded program is saved in the variable `processed_program`.
It’s done in these steps:

1. Run `gawk` with the `@include`-processing program (the
   value of the `expand_prog` shell variable) reading standard input.
2. Standard input is the contents of the user’s program,
   from the shell variable `program`.
   Feed its contents to `gawk` via a here document.
3. Save the results of this processing in the shell variable
   `processed_program` by using command substitution.

The last step is to call `gawk` with the expanded program,
along with the original
options and command-line arguments that the user supplied:

```
eval gawk $opts -- '"$processed_program"' '"$@"'
```

The `eval` command is a shell construct that reruns the shell’s parsing
process. This keeps things properly quoted.

This version of `igawk` represents the fifth version of this program.
There are four key simplifications that make the program work better:

* Using `@include` even for the files named with `-f` makes building
  the initial collected `awk` program much simpler; all the
  `@include` processing can be done once.
* Not trying to save the line read with `getline`
  in the `pathto()` function when testing for the
  file’s accessibility for use with the main program simplifies things
  considerably.
* Using a `getline` loop in the `BEGIN` rule does it all in one
  place. It is not necessary to call out to a separate loop for processing
  nested `@include` directives.
* Instead of saving the expanded program in a temporary file, putting it in a shell variable
  avoids some potential security problems.
  This has the disadvantage that the script relies upon more features
  of the `sh` language, making it harder to follow for those who
  aren’t familiar with `sh`.

Also, this program illustrates that it is often worthwhile to combine
`sh` and `awk` programming together. You can usually
accomplish quite a lot, without having to resort to low-level programming
in C or C++, and it is frequently easier to do certain kinds of string
and argument manipulation using the shell than it is in `awk`.

Finally, `igawk` shows that it is not always necessary to add new
features to a program; they can often be layered on top.[84](#FOOT84)

Before `gawk` acquired its built-in `@include` mechanism,
`igawk` and its manual page were installed as part of the regular
`gawk` installation (‘`make install`’). This is no longer
done, because it’s no longer necessary. But we’ve kept the program in this
Web page for its educational value.

---

Next: [And Now for Something Completely Different](#Signature-Program), Previous: [An Easy Way to Use Library Functions](#Igawk-Program), Up: [A Grab Bag of `awk` Programs](#Miscellaneous-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.10 Finding Anagrams from a Dictionary [¶](#Finding-Anagrams-from-a-Dictionary)

An interesting programming challenge is to
search for *anagrams* in a
word list (such as
`/usr/share/dict/words` on many GNU/Linux systems).
One word is an anagram of another if both words contain
the same letters
(e.g., “babbling” and “blabbing”).

Column 2, Problem C, of Jon Bentley’s Programming Pearls, Second
Edition, presents an elegant algorithm. The idea is to give words that
are anagrams a common signature, sort all the words together by their
signatures, and then print them. Dr. Bentley observes that taking the
letters in each word and sorting them produces those common signatures.

The following program uses arrays of arrays to bring together
words with the same signature and array sorting to print the words
in sorted order:

```
# anagram.awk --- An implementation of the anagram-finding algorithm
#                 from Jon Bentley's "Programming Pearls," 2nd edition.
#                 Addison Wesley, 2000, ISBN-10: 0-201-65788-0,
#                 ISBN-13: 978-0-201-65788-3.
#                 Column 2, Problem C, section 2.8, pp 18-20.

/'s$/   { next }        # Skip possessives
```

The program starts with a header, and then a rule to skip
possessives in the dictionary file. The next rule builds
up the data structure. The first dimension of the array
is indexed by the signature; the second dimension is the word
itself:

```
{
    key = word2key($1)  # Build signature
    data[key][$1] = $1  # Store word with signature
}
```

The `word2key()` function creates the signature.
It splits the word apart into individual letters,
sorts the letters, and then joins them back together:

```
# word2key --- split word apart into letters, sort, and join back together

function word2key(word,     a, i, n, result)
{
    n = split(word, a, "")
    asort(a)

    for (i = 1; i <= n; i++)
        result = result a[i]

    return result
}
```

Finally, the `END` rule traverses the array
and prints out the anagram lists. It sends the output
to the system `sort` command because otherwise
the anagrams would appear in arbitrary order:

```
END {
    sort = "sort"
    for (key in data) {
        # Sort words with same key
        nwords = asorti(data[key], words)
        if (nwords == 1)
            continue

        # And print. Minor glitch: trailing space at end of each line
        for (j = 1; j <= nwords; j++)
            printf("%s ", words[j]) | sort
        print "" | sort
    }
    close(sort)
}
```

Here is some partial output when the program is run:

```
$ gawk -f anagram.awk /usr/share/dict/words | grep '^b'
...
babbled blabbed
babbler blabber brabble
babblers blabbers brabbles
babbling blabbing
babbly blabby
babel bable
babels beslab
babery yabber
...
```

---

Next: [Demonstrating Shortest and Longest Match Operators](#Shortest-match-program), Previous: [Finding Anagrams from a Dictionary](#Anagram-Program), Up: [A Grab Bag of `awk` Programs](#Miscellaneous-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.11 And Now for Something Completely Different [¶](#And-Now-for-Something-Completely-Different)

The following program was written by Davide Brini
and is published on [his website](https://backreference.org/2011/02/03/obfuscated-awk/).
It serves as his signature in the Usenet group `comp.lang.awk`.
He supplies the following copyright terms:

> Copyright © 2008 Davide Brini
>
> Copying and distribution of the code published in this page, with or without
> modification, are permitted in any medium without royalty provided the copyright
> notice and this notice are preserved.

Here is the program:

```
awk 'BEGIN{O="~"~"~";o="=="=="==";o+=+o;x=O""O;while(X++<=x+o+o)c=c"%c";
printf c,(x-O)*(x-O),x*(x-o)-o,x*(x-O)+x-O-o,+x*(x-O)-x+o,X*(o*o+O)+x-O,
X*(X-x)-o*o,(x+X)*o*o+o,x*(X-x)-O-O,x-O+(O+o+X+x)*(o+O),X*X-X*(x-O)-x+O,
O+X*(o*(o+O)+O),+x+O+X*o,x*(x-o),(o+X+x)*o*o-(x-O-O),O+(X-x)*(X+O),x-O}'
```

We leave it to you to determine what the program does. (If you are
truly desperate to understand it, see Chris Johansen’s explanation,
which is embedded in the Texinfo source file for this Web page.)

---

Previous: [And Now for Something Completely Different](#Signature-Program), Up: [A Grab Bag of `awk` Programs](#Miscellaneous-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.12 Demonstrating Shortest and Longest Match Operators [¶](#Demonstrating-Shortest-and-Longest-Match-Operators)

In [Shortest Match, or “Non-greedy” Regexp Operators](#Non_002dgreedy-operators), we described the
POSIX 2024 shortest-match operators, and show the output
of a program that demonstrated the differences.
Here is the program, `shortest-match.awk`:

```
# shortest-match.awk --- Demonstrate shortest match operators

BEGIN {
    text[1] = "aaaxxxzzz";
        shortpat[1] = @/x+?/
        longpat[1] = @/x+/
    text[2] = "aaaxxxyzzz";
        shortpat[2] = @/x+?y/
        longpat[2] = @/x+y/
    text[3] = "aaaxxxxxxxxxxxxxxxxzzz";
        shortpat[3] = @/(x+?)(x+)(x+?)(x+)/
        longpat[3] = @/(x+)(x+)(x+)(x+)/
    text[4] = "aaaxyxxyxxyxzzz"
        shortpat[4] = @/((x+)(y+?)(x+))+/
        longpat[4] = @/((x+)(y+)(x+))+/
    text[5] = "aaaxyxxyxxyxzzz"
        shortpat[5] = @/((x+)(y+?)(x+)){2}/
        longpat[5] = @/((x+)(y+)(x+)){2}/

    count = length(text)
    for (i = 1; i <= count; i++) {
        show(text[i], shortpat[i], longpat[i])
    }
}

# show --- show the results of using shortest- and longest-match operators

function show(text, shortpat, longpat,
          s_offsets, l_offsets, i, n)   # locals
{
    printf("\"%s\"\n", text)

    match(text, shortpat, s_offsets)
    subresult = gensub(shortpat, "X", 1, text)
    printf("\tshortpat: /%s/, result: \"%s\"\n", shortpat, subresult)
    dump(text, s_offsets)

    match(text, longpat, l_offsets)
    subresult = gensub(longpat, "X", 1, text)
    printf("\tlongpat: /%s/, result: \"%s\"\n", longpat, subresult)
    dump(text, l_offsets)
}

# dump --- dump out the start and length of subpattern matches

function dump(text, offsets,    i, n, s, l)
{
    n = length(offsets)
    for (i = 0; i <= n; i++) {
        if ((i, "start") in offsets) {
            s = offsets[i, "start"]
            l = offsets[i, "length"]
            printf("\t\t%d: (s: %d, l: %d) -> \"%s\"\n", i,
                s, l, substr(text, s, l))
        }
    }
}
```

The program makes use of strongly typed
regexp constants (see [Strongly Typed Regexp Constants](#Strong-Regexp-Constants)),
the optional fourth argument to the `match()`
function, and the `gensub()` and `substr()`
functions (see [String-Manipulation Functions](#String-Functions)).

---

Next: [Exercises](#Programs-Exercises), Previous: [A Grab Bag of `awk` Programs](#Miscellaneous-Programs), Up: [Practical `awk` Programs](#Sample-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
