### 2.11 Summary [¶](#Summary-1)

* `gawk` parses arguments on the command line, left to right, to
  determine if they should be treated as options or as non-option arguments.
* `gawk` recognizes several options which control its operation,
  as described in [Command-Line Options](#Options). All options begin with ‘`-`’.
* Any argument that is not recognized as an option is treated as a
  non-option argument, even if it begins with ‘`-`’.
  + However, when an option itself requires an argument, and the option is separated
    from that argument on the command line by at least one space, the space
    is ignored, and the argument is considered to be related to the option. Thus, in
    the invocation, ‘`gawk -F x`’, the ‘`x`’ is treated as belonging to the
    `-F` option, not as a separate non-option argument.
* Once `gawk` finds a non-option argument, it stops looking for
  options. Therefore, all following arguments are also non-option arguments,
  even if they resemble recognized options.
* If no `-e` or `-f` options are present, `gawk`
  expects the program text to be in the first non-option argument.
* All non-option arguments, except program text provided in the first
  non-option argument, are placed in `ARGV` as explained in
  [Using `ARGC` and `ARGV`](#ARGC-and-ARGV), and are processed as described in [Other Command-Line Arguments](#Other-Arguments).
  Adjusting `ARGC` and `ARGV`
  affects how `awk` processes input.
* The three standard options for all versions of `awk` are
  `-f`, `-F`, and `-v`. `gawk` supplies these
  and many others, as well as corresponding GNU-style long options.
* Nonoption command-line arguments are usually treated as file names,
  unless they have the form ‘`var=value`’, in which case
  they are taken as variable assignments to be performed at that point
  in processing the input.
* You can use a single minus sign (‘`-`’) to refer to standard input
  on the command line. `gawk` also lets you use the special
  file name `/dev/stdin`.
* `gawk` pays attention to a number of environment variables.
  `AWKPATH`, `AWKLIBPATH`, and `POSIXLY_CORRECT` are the
  most important ones.
* `gawk`’s exit status conveys information to the program
  that invoked it. Use the `exit` statement from within
  an `awk` program to set the exit status.
* `gawk` allows you to include other `awk` source files into
  your program using the `@include` directive and/or the `-i`
  and `-f` command-line options.
* `gawk` allows you to load additional functions written in C
  or C++ using the `@load` directive and/or the `-l` option.
  (This advanced feature is described later, in [Writing Extensions for `gawk`](#Dynamic-Extensions).)

---

Next: [Reading Input Files](#Reading-Files), Previous: [Running `awk` and `gawk`](#Invoking-Gawk), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 3 Regular Expressions [¶](#Regular-Expressions)

A *regular expression*, or *regexp*, is a way of describing a
set of strings.
Because regular expressions are such a fundamental part of `awk`
programming, their format and use deserve a separate chapter.

A regular expression enclosed in slashes (‘`/`’)
is an `awk` pattern that matches every input record whose text
belongs to that set.
The simplest regular expression is a sequence of letters, numbers, or
both. Such a regexp matches any string that contains that sequence.
Thus, the regexp ‘`foo`’ matches any string containing ‘`foo`’.
Thus, the pattern `/foo/` matches any input record containing
the three adjacent characters ‘`foo`’ *anywhere* in the record. Other
kinds of regexps let you specify more complicated classes of strings.

Initially, the examples in this chapter are simple.
As we explain more about how
regular expressions work, we present more complicated instances.

* [How to Use Regular Expressions](#Regexp-Usage)
* [Escape Sequences](#Escape-Sequences)
* [Regular Expression Operators](#Regexp-Operators)
* [Using Bracket Expressions](#Bracket-Expressions)
* [How Much Text Matches?](#Leftmost-Longest)
* [Using Dynamic Regexps](#Computed-Regexps)
* [`gawk`-Specific Regexp Operators](#GNU-Regexp-Operators)
* [Case Sensitivity in Matching](#Case_002dsensitivity)
* [Selecting the Regexp Matching Engine](#Choosing-Matchers)
* [Summary](#Regexp-Summary)

---

Next: [Escape Sequences](#Escape-Sequences), Up: [Regular Expressions](#Regexp)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.1 How to Use Regular Expressions [¶](#How-to-Use-Regular-Expressions)

A regular expression can be used as a pattern by enclosing it in
slashes. Then the regular expression is tested against the
entire text of each record. (Normally, it only needs
to match some part of the text in order to succeed.) For example, the
following prints the second field of each record where the string
‘`li`’ appears anywhere in the record:

```
$ awk '/li/ { print $2 }' mail-list
-| 555-5553
-| 555-0542
-| 555-6699
-| 555-3430
```

Regular expressions can also be used in matching expressions. These
expressions allow you to specify the string to match against; it need
not be the entire current input record. The two operators ‘`~`’
and ‘`!~`’ perform regular expression comparisons. Expressions
using these operators can be used as patterns, or in `if`,
`while`, `for`, and `do` statements.
(See [Control Statements in Actions](#Statements).)
For example, the following is true if the expression exp (taken
as a string) matches regexp:

```
exp ~ /regexp/
```

This example matches, or selects, all input records with the uppercase
letter ‘`J`’ somewhere in the first field:

```
$ awk '$1 ~ /J/' inventory-shipped
-| Jan  13  25  15 115
-| Jun  31  42  75 492
-| Jul  24  34  67 436
-| Jan  21  36  64 620
```

So does this:

```
awk '{ if ($1 ~ /J/) print }' inventory-shipped
```

This next example is true if the expression exp
(taken as a character string)
does *not* match regexp:

```
exp !~ /regexp/
```

The following example matches,
or selects, all input records whose first field *does not* contain
the uppercase letter ‘`J`’:

```
$ awk '$1 !~ /J/' inventory-shipped
-| Feb  15  32  24 226
-| Mar  15  24  34 228
-| Apr  31  52  63 420
-| May  16  34  29 208
...
```

When a regexp is enclosed in slashes, such as `/foo/`, we call it
a *regexp constant*, much like `5.27` is a numeric constant and
`"foo"` is a string constant.

---

Next: [Regular Expression Operators](#Regexp-Operators), Previous: [How to Use Regular Expressions](#Regexp-Usage), Up: [Regular Expressions](#Regexp)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.2 Escape Sequences [¶](#Escape-Sequences-1)

Some characters cannot be included literally in string constants
(`"foo"`) or regexp constants (`/foo/`).
Instead, they should be represented with *escape sequences*,
which are character sequences beginning with a backslash (‘`\`’).
One use of an escape sequence is to include a double quote character in
a string constant. Because a plain double quote ends the string, you
must use ‘`\"`’ to represent an actual double quote character as a
part of the string. For example:

```
$ awk 'BEGIN { print "He said \"hi!\" to her." }'
-| He said "hi!" to her.
```

The backslash character itself is another character that cannot be
included normally; you must write ‘`\\`’ to put one backslash in the
string or regexp. Thus, the string whose contents are the two characters
‘`"`’ and ‘`\`’ must be written `"\"\\"`.

Other escape sequences represent unprintable characters
such as TAB or newline. There is nothing to stop you from entering most
unprintable characters directly in a string constant or regexp constant,
but they may look ugly.

The following list presents
all the escape sequences used in `awk` and
what they represent. Unless noted otherwise, all these escape
sequences apply to both string constants and regexp constants:

`\\`
:   A literal backslash, ‘`\`’.

`\a`
:   The “alert” character, `Ctrl-g`, ASCII code 7 (BEL).
    (This often makes some sort of audible noise.)

`\b`
:   Backspace, `Ctrl-h`, ASCII code 8 (BS).

`\f`
:   Formfeed, `Ctrl-l`, ASCII code 12 (FF).

`\n`
:   Newline, `Ctrl-j`, ASCII code 10 (LF).

`\r`
:   Carriage return, `Ctrl-m`, ASCII code 13 (CR).

`\t`
:   Horizontal TAB, `Ctrl-i`, ASCII code 9 (HT).

`\v`
:   Vertical TAB, `Ctrl-k`, ASCII code 11 (VT).

`\nnn`
:   The octal value nnn, where nnn stands for 1 to 3 digits
    between ‘`0`’ and ‘`7`’. For example, the code for the ASCII ESC
    (escape) character is ‘`\033`’.

`\xhh…`
:   The hexadecimal value hh, where hh stands for a sequence
    of hexadecimal digits (‘`0`’–‘`9`’, and either ‘`A`’–‘`F`’
    or ‘`a`’–‘`f`’). A maximum of two digits are allowed after
    the ‘`\x`’. Any further hexadecimal digits are treated as simple
    letters or numbers. (c.e.)
    (The ‘`\x`’ escape sequence is not allowed in POSIX awk.)

    > **CAUTION:** In ISO C, the escape sequence continues until the first nonhexadecimal
    > digit is seen.
    > For many years, `gawk` would continue incorporating
    > hexadecimal digits into the value until a non-hexadecimal digit
    > or the end of the string was encountered.
    > However, using more than two hexadecimal digits produced
    > undefined results.
    > As of version 4.2, only two digits
    > are processed.

`\uhh…`
:   The hexadecimal value hh, where hh stands for a sequence
    of one or more hexadecimal digits (‘`0`’–‘`9`’, and either
    ‘`A`’–‘`F`’ or ‘`a`’–‘`f`’). A maximum of eight digits
    are allowed after the ‘`\u`’. Any further hexadecimal digits are
    treated as simple letters or numbers. (c.e.) (The ‘`\u`’
    escape sequence is not allowed in POSIX awk.)

    This escape sequence is intended for designating a character in
    the current locale’s character set.[16](#FOOT16) `gawk` first
    converts the given digits into an integer and then translates the given
    “wide character” value into the current locale’s multibyte encoding. If
    the wide character value does not represent a valid character, or if
    the character is valid but cannot be encoded into the current locale’s
    multibyte encoding, the value becomes `"?"`. `gawk` issues
    a warning message when this happens.

`\/`
:   A literal slash (should be used for regexp constants only).
    This sequence is used when you want to write a regexp
    constant that contains a slash
    (such as `/.*:\/home\/[[:alnum:]]+:.*/`; the ‘`[[:alnum:]]`’
    notation is discussed in [Using Bracket Expressions](#Bracket-Expressions)).
    Because the regexp is delimited by
    slashes, you need to escape any slash that is part of the pattern,
    in order to tell `awk` to keep processing the rest of the regexp.

`\"`
:   A literal double quote (should be used for string constants only).
    This sequence is used when you want to write a string
    constant that contains a double quote
    (such as `"He said \"hi!\" to her."`).
    Because the string is delimited by
    double quotes, you need to escape any double quote that is part of the string,
    in order to tell `awk` to keep processing the rest of the string.

In `gawk`, a number of additional two-character sequences that begin
with a backslash have special meaning in regexps.
See [`gawk`-Specific Regexp Operators](#GNU-Regexp-Operators).

In a regexp, a backslash before any character that is not in the previous list
and not listed in
[`gawk`-Specific Regexp Operators](#GNU-Regexp-Operators)
means that the next character should be taken literally, even if it would
normally be a regexp operator. For example, `/a\+b/` matches the three
characters ‘`a+b`’.

For complete portability, do not use a backslash before any character not
shown in the previous list or that is not a regular expression operator.
(The 2024 POSIX standard explicitly lists the operators that can be
escaped, leaving it undefined as to what happens for any other
escaped character. But the bottom line is as described previously.)

| Backslash Before Regular Characters |
| --- |
| If you place a backslash in a string constant before something that is not one of the characters previously listed, POSIX `awk` purposely leaves what happens as undefined. There are two choices:  Strip the backslash out  This is what BWK `awk` and `gawk` both do. For example, `"a\qc"` is the same as `"aqc"`. (Because this is such an easy bug both to introduce and to miss, `gawk` warns you about it.) Consider ‘`FS = "[ \t]+\|[ \t]+"`’ to use vertical bars surrounded by whitespace as the field separator. There should be two backslashes in the string: ‘`FS = "[ \t]+\\|[ \t]+"`’.  Leave the backslash alone [¶](#index-mawk-utility)  Some other `awk` implementations do this. In such implementations, typing `"a\qc"` is the same as typing `"a\\qc"`. |

To summarize:

* The escape sequences in the preceding list are always processed first,
  for both string constants and regexp constants. This happens very early,
  as soon as `awk` reads your program.
* `gawk` processes both regexp constants and dynamic regexps
  (see [Using Dynamic Regexps](#Computed-Regexps)),
  for the special operators listed in
  [`gawk`-Specific Regexp Operators](#GNU-Regexp-Operators).
* A backslash before any other character means to treat that character
  literally.

| Escape Sequences for Metacharacters |
| --- |
| Suppose you use an octal or hexadecimal (`\x` or `\u`) escape to represent a regexp metacharacter. (See [Regular Expression Operators](#Regexp-Operators).) Does `awk` treat the character as a literal character or as a regexp operator?  Historically, such characters were taken literally. (d.c.) However, the POSIX standard indicates that they should be treated as real metacharacters, which is what `gawk` does. In compatibility mode (see [Command-Line Options](#Options)), `gawk` treats the characters represented by octal and hexadecimal escape sequences literally when used in regexp constants. Thus, `/a\52b/` is equivalent to `/a\*b/`. |

---

Next: [Using Bracket Expressions](#Bracket-Expressions), Previous: [Escape Sequences](#Escape-Sequences), Up: [Regular Expressions](#Regexp)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.3 Regular Expression Operators [¶](#Regular-Expression-Operators)

You can combine regular expressions with special characters,
called *regular expression operators* or *metacharacters*, to
increase the power and versatility of regular expressions.

* [Regexp Operators in `awk`](#Regexp-Operator-Details)
* [Some Notes On Interval Expressions](#Interval-Expressions)
* [Shortest Match, or “Non-greedy” Regexp Operators](#Non_002dgreedy-operators)

---

Next: [Some Notes On Interval Expressions](#Interval-Expressions), Up: [Regular Expression Operators](#Regexp-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.3.1 Regexp Operators in `awk` [¶](#Regexp-Operators-in-awk)

The escape sequences described
earlier
in [Escape Sequences](#Escape-Sequences)
are valid inside a regexp. They are introduced by a ‘`\`’ and
are recognized and converted into corresponding real characters as
the very first step in processing regexps.

Here is a list of metacharacters. All characters that are not escape
sequences and that are not listed here stand for themselves:

`\`
:   This suppresses the special meaning of a character when
    matching. For example, ‘`\$`’
    matches the character ‘`$`’.

`^`
:   This matches the beginning of a string. ‘`^@chapter`’
    matches ‘`@chapter`’ at the beginning of a string,
    for example, and can be used
    to identify chapter beginnings in Texinfo source files.
    The ‘`^`’ is known as an *anchor*, because it anchors the pattern to
    match only at the beginning of the string.

    It is important to realize that ‘`^`’ does not match the beginning of
    a line (the point right after a ‘`\n`’ newline character) embedded in a string.
    The condition is not true in the following example:

    ```
    if ("line1\nLINE 2" ~ /^L/) ...
    ```

`$`
:   This is similar to ‘`^`’, but it matches only at the end of a string.
    For example, ‘`p$`’
    matches a record that ends with a ‘`p`’. The ‘`$`’ is an anchor
    and does not match the end of a line
    (the point right before a ‘`\n`’ newline character)
    embedded in a string.
    The condition in the following example is not true:

    ```
    if ("line1\nLINE 2" ~ /1$/) ...
    ```

`.` (period) [¶](#index-_002e-_0028period_0029_002c-regexp-operator)
:   This matches any single character,
    *including* the newline character. For example, ‘`.P`’
    matches any single character followed by a ‘`P`’ in a string. Using
    concatenation, we can make a regular expression such as ‘`U.A`’, which
    matches any three-character sequence that begins with ‘`U`’ and ends
    with ‘`A`’.

    In strict POSIX mode (see [Command-Line Options](#Options)),
    ‘`.`’ does not match the NUL
    character, which is a character with all bits equal to zero.
    Otherwise, NUL is just another character. Other versions of `awk`
    may not be able to match the NUL character.

`[`…`]` [¶](#index-_005b_005d-_0028square-brackets_0029_002c-regexp-operator)
:   This is called a *bracket expression*.[17](#FOOT17)
    It matches any *one* of the characters that are enclosed in
    the square brackets. For example, ‘`[MVX]`’ matches any one of
    the characters ‘`M`’, ‘`V`’, or ‘`X`’ in a string. A full
    discussion of what can be inside the square brackets of a bracket expression
    is given in
    [Using Bracket Expressions](#Bracket-Expressions).

`[^`…`]`
:   This is a *complemented bracket expression*. The first character after
    the ‘`[`’ *must* be a ‘`^`’. It matches any characters
    *except* those in the square brackets. For example, ‘`[^awk]`’
    matches any character that is not an ‘`a`’, ‘`w`’,
    or ‘`k`’.

`|` [¶](#index-_007c-_0028vertical-bar_0029)
:   This is the *alternation operator* and it is used to specify
    alternatives. The ‘`|`’ has the lowest precedence of all the regular
    expression operators. For example, ‘`^P|[aeiouy]`’ matches any string
    that matches either ‘`^P`’ or ‘`[aeiouy]`’. This means it matches
    any string that starts with ‘`P`’ or contains (anywhere within it)
    a lowercase English vowel.

    The alternation applies to the largest possible regexps on either side.

`(`…`)`
:   Parentheses are used for grouping in regular expressions, as in
    arithmetic. They can be used to concatenate regular expressions
    containing the alternation operator, ‘`|`’. For example,
    ‘`@(samp|code)\{[^}]+\}`’ matches both ‘`@code{foo}`’ and
    ‘`@samp{bar}`’.
    (These are Texinfo formatting control sequences. The ‘`+`’ is
    explained further on in this list.)

    The left or opening parenthesis is always a metacharacter; to match
    one literally, precede it with a backslash. However, the right or
    closing parenthesis is only special when paired with a left parenthesis;
    an unpaired right parenthesis is (silently) treated as a regular character.

`*`
:   This symbol means that the preceding regular expression should be
    repeated as many times as necessary to find a match. For example, ‘`ph*`’
    applies the ‘`*`’ symbol to the preceding ‘`h`’ and looks for matches
    of one ‘`p`’ followed by any number of ‘`h`’s. This also matches
    just ‘`p`’ if no ‘`h`’s are present.

    There are two subtle points to understand about how ‘`*`’ works.
    First, the ‘`*`’ applies only to the single preceding regular expression
    component (e.g., in ‘`ph*`’, it applies just to the ‘`h`’).
    To cause ‘`*`’ to apply to a larger subexpression, use parentheses:
    ‘`(ph)*`’ matches ‘`ph`’, ‘`phph`’, ‘`phphph`’, and so on.

    Second, ‘`*`’ finds as many repetitions as possible. If the text
    to be matched is ‘`phhhhhhhhhhhhhhooey`’, ‘`ph*`’ matches all of
    the ‘`h`’s.

`*?`
:   Shortest match or *non-greedy* version of the ‘`*`’ operator.
    This operator and the other non-greedy operators are discussed separately,
    shortly, in [Shortest Match, or “Non-greedy” Regexp Operators](#Non_002dgreedy-operators).

`+`
:   This symbol is similar to ‘`*`’, except that the preceding expression must be
    matched at least once. This means that ‘`wh+y`’
    would match ‘`why`’ and ‘`whhy`’, but not ‘`wy`’, whereas
    ‘`wh*y`’ would match all three.

`+?`
:   Shortest match or non-greedy version of the ‘`+`’ operator.

`?`
:   This symbol is similar to ‘`*`’, except that the preceding expression can be
    matched either once or not at all. For example, ‘`fe?d`’
    matches ‘`fed`’ and ‘`fd`’, but nothing else.

`??`
:   Shortest match or non-greedy version of the ‘`?`’ operator.

`{`n`}` [¶](#index-interval-expressions_002c-regexp-operator)

`{`n`,}`

`{`n`,`m`}`
:   One or two numbers inside braces denote an *interval expression*.
    If there is one number in the braces, the preceding regexp is repeated
    n times.
    If there are two numbers separated by a comma, the preceding regexp is
    repeated n to m times.
    If there is one number followed by a comma, then the preceding regexp
    is repeated at least n times:

    `wh{3}y`
    :   Matches ‘`whhhy`’, but not ‘`why`’ or ‘`whhhhy`’.

    `wh{3,5}y`
    :   Matches ‘`whhhy`’, ‘`whhhhy`’, or ‘`whhhhhy`’ only.

    `wh{2,}y`
    :   Matches ‘`whhy`’, ‘`whhhy`’, and so on.

`{`n`}?`

`{`n`,}?`

`{`n`,`m`}?`
:   Shortest match or non-greedy versions of the ‘`{}`’ operators.

In regular expressions, the ‘`*`’, ‘`+`’, and ‘`?`’ operators,
as well as the braces ‘`{`’ and ‘`}`’,
have
the highest precedence, followed by concatenation, and finally by ‘`|`’.
As in arithmetic, parentheses can change how operators are grouped.

According to the POSIX specification, when ‘`*`’, ‘`+`’, ‘`?`’,
or ‘`{`’ are not preceded by a character, the behavior is
“undefined.”
In practice, for `gawk`, the ‘`*`’, ‘`+`’, ‘`?`’ and
‘`{`’ operators stand for themselves when there is nothing in the
regexp that precedes them. For example, `/+/` matches a literal
plus sign. However, many other versions of `awk` treat such a
usage as a syntax error.

| What About The Empty Regexp? |
| --- |
| We describe here an advanced regexp usage. Feel free to skip it upon first reading.  You can supply an empty regexp constant (‘`//`’) in all places where a regexp is expected. Is this useful? What does it match?  It is useful. It matches the (invisible) empty string at the start and end of a string of characters, as well as the empty string between characters. This is best illustrated with the `gsub()` function, which makes global substitutions in a string (see [String-Manipulation Functions](#String-Functions)). Normal usage of `gsub()` is like so:  ``` $ awk ' > BEGIN { >     x = "ABC_CBA" >     gsub(/B/, "bb", x) >     print x > }' -| AbbC_CbbA ```  We can use `gsub()` to see where the empty strings are that match the empty regexp:  ``` $ awk ' > BEGIN { >     x = "ABC" >     gsub(//, "x", x) >     print x > }' -| xAxBxCx ``` |

---

Next: [Shortest Match, or “Non-greedy” Regexp Operators](#Non_002dgreedy-operators), Previous: [Regexp Operators in `awk`](#Regexp-Operator-Details), Up: [Regular Expression Operators](#Regexp-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.3.2 Some Notes On Interval Expressions [¶](#Some-Notes-On-Interval-Expressions)

Interval expressions were not traditionally available in `awk`.
They were added as part of the POSIX standard to make `awk`
and `egrep` consistent with each other.

Initially, because old programs may use ‘`{`’ and ‘`}`’ in regexp
constants,
`gawk` did *not* match interval expressions
in regexps.

However, beginning with version 4.0,
`gawk` does match interval expressions by default.
This is because compatibility with POSIX has become more
important to most `gawk` users than compatibility with
old programs.

For programs that use ‘`{`’ and ‘`}`’ in regexp constants,
it is good practice to always escape them with a backslash. Then the
regexp constants are valid and work the way you want them to, using
any version of `awk`.[18](#FOOT18)

When ‘`{`’ and ‘`}`’ appear in regexp constants
in a way that cannot be interpreted as an interval expression
(such as `/q{a}/`), then they stand for themselves.

As mentioned, interval expressions were not traditionally available
in `awk`. In March of 2019, BWK `awk` (finally) acquired them.
Starting with version 5.2, `gawk`’s
`--traditional` option no longer disables interval
expressions in regular expressions.

POSIX says that interval expressions containing repetition counts greater
than 255 produce unspecified results.

In the manual for GNU `grep`, Paul Eggert notes the following:

> Interval expressions may be implemented internally via repetition.
> For example, ‘`^(a|bc){2,4}$`’ might be implemented as
> ‘`^(a|bc)(a|bc)((a|bc)(a|bc)?)?$`’. A large repetition count may
> exhaust memory or greatly slow matching. Even small counts can cause
> problems if cascaded; for example, ‘`grep -E
> ".*{10,}{10,}{10,}{10,}{10,}"`’ is likely to overflow a
> stack. Fortunately, regular expressions like these are typically
> artificial, and cascaded repetitions do not conform to POSIX so cannot
> be used in portable programs anyway.

This same caveat applies to `gawk`.

---

Previous: [Some Notes On Interval Expressions](#Interval-Expressions), Up: [Regular Expression Operators](#Regexp-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.3.3 Shortest Match, or “Non-greedy” Regexp Operators [¶](#Shortest-Match_002c-or-_0060_0060Non_002dgreedy_0027_0027-Regexp-Operators)

Traditionally, regexps have always matched the leftmost, longest
sequence of characters. For example, given the text ‘`abxxxcd`’,
the regexp ‘`x+`’ could match one, two, or all three ‘`x`’ characters.
By defining a regexp to match the longest possible sequence, we know that
‘`x+`’ will match all three ‘`x`’s. Such behavior may
be termed *greedy*, since a regexp will match as many characters
as possible.

The Perl language introduced “non-greedy,” or shortest-match operators.
In the above example, ‘`x+`’ would only match a single ‘`x`’.

The 2024 POSIX standard introduced shortest-match operators into POSIX
EREs. Syntactically, you create a shortest-match operator by appending
a ‘`?`’ to one of the traditional regexp operators: ‘`*`’, ‘`+`’,
‘`?`’, or ‘`{}`’.

The shortest-match operators don’t always match what you might think would
be the smallest sequence of characters, since the general, leftmost-longest
rule still applies to the overall regexp.

To illustrate the differences, we use a program named `shortest-match.awk`.
This program takes several strings, and replaces the text matching a given
pattern with a single ‘`X`’. The program uses a number of `gawk`-specific
features which haven’t yet been described. We therefore delay presenting the
code until [Demonstrating Shortest and Longest Match Operators](#Shortest-match-program).

For each string, the program prints the following:

* The original string, in quotes.
* The pattern used with a shortest match operator and the result, indented.
* Further indented is a list of the start and lengths of the submatches.
  Submatch zero is the whole string, submatch one is the first
  parenthesized expression, and so on.
* The pattern used with a regular match operator and the result, indented.
* The same start plus length information for submatches of
  the long pattern.

Here is the output, with commentary:

```
"aaaxxxzzz"
    shortpat: /x+?/, result: "aaaXxxzzz"
        0: (s: 4, l: 1) -> "x"
    longpat: /x+/, result: "aaaXzzz"
        0: (s: 4, l: 3) -> "xxx"
```

For the shortest match, only one ‘`x`’ is replaced.
For the longest match, every ‘`x`’ is replaced.
This is pretty straightforward.

```
"aaaxxxyzzz"
    shortpat: /x+?y/, result: "aaaXzzz"
        0: (s: 4, l: 4) -> "xxxy"
    longpat: /x+y/, result: "aaaXzzz"
        0: (s: 4, l: 4) -> "xxxy"
```

In this case, the result is the same, since the longest
possible string of characters is matched.

```
"aaaxxxxxxxxxxxxxxxxzzz"
    shortpat: /(x+?)(x+)(x+?)(x+)/, result: "aaaXzzz"
        0: (s: 4, l: 16) -> "xxxxxxxxxxxxxxxx"
        1: (s: 4, l: 1) -> "x"
        2: (s: 5, l: 13) -> "xxxxxxxxxxxxx"
        3: (s: 18, l: 1) -> "x"
        4: (s: 19, l: 1) -> "x"
    longpat: /(x+)(x+)(x+)(x+)/, result: "aaaXzzz"
        0: (s: 4, l: 16) -> "xxxxxxxxxxxxxxxx"
        1: (s: 4, l: 13) -> "xxxxxxxxxxxxx"
        2: (s: 17, l: 1) -> "x"
        3: (s: 18, l: 1) -> "x"
        4: (s: 19, l: 1) -> "x"
```

This case is more interesting.
The same total amount of text is matched for both patterns.
However, the text matched by the subexpressions differs
between the two cases. In the longest-match case, the
longest possible subexpressions match first.

```
"aaaxyxxyxxyxzzz"
    shortpat: /((x+)(y+?)(x+))+/, result: "aaaXyxxyxzzz"
        0: (s: 4, l: 4) -> "xyxx"
        1: (s: 4, l: 4) -> "xyxx"
        2: (s: 4, l: 1) -> "x"
        3: (s: 5, l: 1) -> "y"
        4: (s: 6, l: 2) -> "xx"
    longpat: /((x+)(y+)(x+))+/, result: "aaaXzzz"
        0: (s: 4, l: 9) -> "xyxxyxxyx"
        1: (s: 10, l: 3) -> "xyx"
        2: (s: 10, l: 1) -> "x"
        3: (s: 11, l: 1) -> "y"
        4: (s: 12, l: 1) -> "x"
```

This case needs careful examination. There is a ‘`+`’ on the outside,
meaning that the total inner expression may match multiple times.
For the longest-match expression, that indeed happens. The subexpressions
two through four have the start and length values for the last occasion
where the inner expression matched. The shortest-match expression
only matched the inner expression once.

```
"aaaxyxxyxxyxzzz"
    shortpat: /((x+)(y+?)(x+)){2}/, result: "aaaXyxzzz"
        0: (s: 4, l: 7) -> "xyxxyxx"
        1: (s: 7, l: 4) -> "xyxx"
        2: (s: 7, l: 1) -> "x"
        3: (s: 8, l: 1) -> "y"
        4: (s: 9, l: 2) -> "xx"
    longpat: /((x+)(y+)(x+)){2}/, result: "aaaXyxzzz"
        0: (s: 4, l: 7) -> "xyxxyxx"
        1: (s: 7, l: 4) -> "xyxx"
        2: (s: 7, l: 1) -> "x"
        3: (s: 8, l: 1) -> "y"
        4: (s: 9, l: 2) -> "xx"
```

Here, the results are identical; both the nature of the string
to be matched and the POSIX longest-leftmost rule, cause this to happen.

The MinRX regular expression matching engine, which is `gawk`’s
default matcher (see [Selecting the Regexp Matching Engine](#Choosing-Matchers)), supports the shortest-match
operators. The older GNU matchers do not. Also, when invoked with the
`--traditional` option, shortest-match operators are not supported,
since traditional `awk` does not have these operators.

---

Next: [How Much Text Matches?](#Leftmost-Longest), Previous: [Regular Expression Operators](#Regexp-Operators), Up: [Regular Expressions](#Regexp)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.4 Using Bracket Expressions [¶](#Using-Bracket-Expressions)

As mentioned earlier, a bracket expression matches any character among
those listed between the opening and closing square brackets.

Within a bracket expression, a *range expression* consists of two
characters separated by a hyphen. It matches any single character that
sorts between the two characters, based upon the system’s native character
set. For example, ‘`[0-9]`’ is equivalent to ‘`[0123456789]`’.
(See [Regexp Ranges and Locales: A Long Sad Story](#Ranges-and-Locales) for an explanation of how the POSIX
standard and `gawk` have changed over time. This is mainly
of historical interest.)

With the increasing popularity of the
[Unicode character standard](https://www.unicode.org),
there is an additional wrinkle to consider. Octal and hexadecimal
escape sequences inside bracket expressions are taken to represent
only single-byte characters (characters whose values fit within
the range 0–255). To match a range of characters where the endpoints
of the range are larger than 255, enter the multibyte encodings of
the characters directly, or use the `\u` escape sequence.

To include one of the characters ‘`\`’, ‘`]`’, ‘`-`’, or ‘`^`’ in a
bracket expression, put a ‘`\`’ in front of it. For example:

```
[d\]]
```

matches either ‘`d`’ or ‘`]`’.
Additionally, if you place ‘`]`’ right after the opening
‘`[`’ (‘`[]d]`’), the closing bracket is treated as one of the
characters to be matched.
Inside bracket expressions, it’s not necessary to escape
the other standard regular expression operators, such as ‘`*`’ and ‘`?`’,
and for full portability you should not.

> **NOTE:** Note that the additional regular expression operators that begin with a
> backslash, such as ‘`\<`’, or ‘`\w`’, have no meaning
> when used inside a bracket expression. There, the backslash is taken
> to mean escape the following character, so ‘`[\w]`’ is the same
> as ‘`[w]`’, and ‘`[\<]`’ is the same as ‘`[<]`’.

The treatment of ‘`\`’ in bracket expressions
is compatible with other `awk`
implementations and is also mandated by POSIX.[19](#FOOT19)
The regular expressions in `awk` are a superset
of the POSIX specification for Extended Regular Expressions (EREs).
POSIX EREs are based on the regular expressions accepted by the
traditional `egrep` utility.

*Character classes* are a feature introduced in the POSIX standard.
A character class is a special notation for describing
lists of characters that have a specific attribute, but the
actual characters can vary from country to country and/or
from character set to character set. For example, the notion of what
is an alphabetic character differs between the United States and France.

A character class is only valid in a regexp *inside* the
brackets of a bracket expression. Character classes consist of ‘`[:`’,
a keyword denoting the class, and ‘`:]`’.
[Table 3.1](#table_002dchar_002dclasses) lists the character classes defined by the
POSIX standard.

| Class | Meaning |
| --- | --- |
| `[:alnum:]` | Alphanumeric characters |
| `[:alpha:]` | Alphabetic characters |
| `[:blank:]` | Space and TAB characters |
| `[:cntrl:]` | Control characters |
| `[:digit:]` | Numeric characters |
| `[:graph:]` | Characters that are both printable and visible (a space is printable but not visible, whereas an ‘`a`’ is both) |
| `[:lower:]` | Lowercase alphabetic characters |
| `[:print:]` | Printable characters (characters that are not control characters) |
| `[:punct:]` | Punctuation characters (characters that are not letters, digits, control characters, or space characters) |
| `[:space:]` | Space characters (these are: space, TAB, newline, carriage return, formfeed and vertical tab) |
| `[:upper:]` | Uppercase alphabetic characters |
| `[:xdigit:]` | Characters that are hexadecimal digits |

**Table 3.1:** POSIX character classes

For example, before the POSIX standard, you had to write `/[A-Za-z0-9]/`
to match alphanumeric characters. If your
character set had other alphabetic characters in it, this would not
match them.
With the POSIX character classes, you can write
`/[[:alnum:]]/` to match the alphabetic
and numeric characters in your character set.

Some utilities that match regular expressions provide a nonstandard
‘`[:ascii:]`’ character class; `awk` does not. However, you
can simulate such a construct using ‘`[\x00-\x7F]`’. This matches
all values numerically between zero and 127, which is the defined
range of the ASCII character set. Use a complemented character list
(‘`[^\x00-\x7F]`’) to match any single-byte characters that are not
in the ASCII range.

> **NOTE:** Some older versions of Unix `awk`
> treat `[:blank:]` like `[:space:]`, incorrectly matching
> more characters than they should. Caveat Emptor.

Two additional special sequences can appear in bracket expressions.
These apply to non-ASCII character sets, which can have single symbols
(called *collating elements*) that are represented with more than one
character. They can also have several characters that are equivalent for
*collating*, or sorting, purposes. (For example, in French, a plain “e”
and a grave-accented “è” are equivalent.)
These sequences are:

Collating symbols [¶](#index-collating-symbols)
:   Multicharacter collating elements enclosed between
    ‘`[.`’ and ‘`.]`’. For example, if ‘`ch`’ is a collating element,
    then ‘`[[.ch.]]`’ is a regexp that matches this collating element, whereas
    ‘`[ch]`’ is a regexp that matches either ‘`c`’ or ‘`h`’.

Equivalence classes
:   Locale-specific names for a list of
    characters that are equal. The name is enclosed between
    ‘`[=`’ and ‘`=]`’.
    For example, the name ‘`e`’ might be used to represent all of
    “e,” “ê,” “è,” and “é.” In this case, ‘`[[=e=]]`’ is a regexp
    that matches any of ‘`e`’, ‘`ê`’, ‘`é`’, or ‘`è`’.

These features are very valuable in non-English-speaking locales.

> **CAUTION:** The library functions that `gawk` uses for regular
> expression matching currently recognize only POSIX character classes;
> they do not recognize collating symbols or equivalence classes.

Inside a bracket expression, an opening bracket (‘`[`’) that does
not start a character class, collating element or equivalence class is
taken literally. This is also true of ‘`.`’ and ‘`*`’.

---

Next: [Using Dynamic Regexps](#Computed-Regexps), Previous: [Using Bracket Expressions](#Bracket-Expressions), Up: [Regular Expressions](#Regexp)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.5 How Much Text Matches? [¶](#How-Much-Text-Matches_003f)

Consider the following:

```
echo aaaabcd | awk '{ sub(/a+/, "<A>"); print }'
```

This example uses the `sub()` function to make a change to the input
record. (`sub()` replaces the first instance of any text matched
by the first argument with the string provided as the second argument;
see [String-Manipulation Functions](#String-Functions).) Here, the regexp `/a+/` indicates “one
or more ‘`a`’ characters,” and the replacement text is ‘`<A>`’.

The input contains four ‘`a`’ characters.
`awk` (and POSIX) regular expressions always match
the leftmost, *longest* sequence of input characters that can
match. Thus, all four ‘`a`’ characters are
replaced with ‘`<A>`’ in this example:

```
$ echo aaaabcd | awk '{ sub(/a+/, "<A>"); print }'
-| <A>bcd
```

For simple match/no-match tests, this is not so important. But when doing
text matching and substitutions with the `match()`, `sub()`, `gsub()`,
and `gensub()` functions, it is very important.
Understanding this principle is also important for regexp-based record
and field splitting (see [How Input Is Split into Records](#Records),
and also see [Specifying How Fields Are Separated](#Field-Separators)).

---

Next: [`gawk`-Specific Regexp Operators](#GNU-Regexp-Operators), Previous: [How Much Text Matches?](#Leftmost-Longest), Up: [Regular Expressions](#Regexp)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.6 Using Dynamic Regexps [¶](#Using-Dynamic-Regexps)

The righthand side of a ‘`~`’ or ‘`!~`’ operator need not be a
regexp constant (i.e., a string of characters between slashes). It may
be any expression. The expression is evaluated and converted to a string
if necessary; the contents of the string are then used as the
regexp. A regexp computed in this way is called a *dynamic
regexp* or a *computed regexp*:

```
BEGIN { digits_regexp = "[[:digit:]]+" }
$0 ~ digits_regexp    { print }
```

This sets `digits_regexp` to a regexp that describes one or more digits,
and tests whether the input record matches this regexp.

> **NOTE:** When using the ‘`~`’ and ‘`!~`’
> operators, be aware that there is a difference between a regexp constant
> enclosed in slashes and a string constant enclosed in double quotes.
> If you are going to use a string constant, you have to understand that
> the string is, in essence, scanned *twice*: the first time when
> `awk` reads your program, and the second time when it goes to
> match the string on the lefthand side of the operator with the pattern
> on the right. This is true of any string-valued expression (such as
> `digits_regexp`, shown in the previous example), not just string constants.

What difference does it make if the string is
scanned twice? The answer has to do with escape sequences, and particularly
with backslashes. To get a backslash into a regular expression inside a
string, you have to type two backslashes.

For example, `/\*/` is a regexp constant for a literal ‘`*`’.
Only one backslash is needed. To do the same thing with a string,
you have to type `"\\*"`. The first backslash escapes the
second one so that the string actually contains the
two characters ‘`\`’ and ‘`*`’.

Given that you can use both regexp and string constants to describe
regular expressions, which should you use? The answer is “regexp
constants,” for several reasons:

* String constants are more complicated to write and
  more difficult to read. Using regexp constants makes your programs
  less error-prone. Not understanding the difference between the two
  kinds of constants is a common source of errors.
* It is more efficient to use regexp constants. `awk` can note
  that you have supplied a regexp and store it internally in a form that
  makes pattern matching more efficient. When using a string constant,
  `awk` must first convert the string into this internal form and
  then perform the pattern matching.
* Using regexp constants is better form; it shows clearly that you
  intend a regexp match.

| Using `\n` in Bracket Expressions of Dynamic Regexps |
| --- |
| Some older versions of `awk` do not allow the newline character to be used inside a bracket expression for a dynamic regexp:  ``` $ awk '$0 ~ "[ \t\n]"' error→ awk: newline in character class [ error→ ]... error→  source line number 1 error→  context is error→        $0 ~ "[ >>>  \t\n]" <<< ```  But a newline in a regexp constant works with no problem:  ``` $ awk '$0 ~ /[ \t\n]/' here is a sample line -| here is a sample line Ctrl-d ```  `gawk` does not have this problem, and it isn’t likely to occur often in practice, but it’s worth noting for future reference. |

---

Next: [Case Sensitivity in Matching](#Case_002dsensitivity), Previous: [Using Dynamic Regexps](#Computed-Regexps), Up: [Regular Expressions](#Regexp)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.7 `gawk`-Specific Regexp Operators [¶](#gawk_002dSpecific-Regexp-Operators)

GNU software that deals with regular expressions provides a number of
additional regexp operators. These operators are described in this
section and are specific to `gawk`;
they are not available in other `awk` implementations.
Most of the additional operators deal with word matching.
For our purposes, a *word* is a sequence of one or more letters, digits,
or underscores (‘`_`’):

`\s`
:   Matches any space character as defined by the current locale.
    Think of it as shorthand for
    ‘`[[:space:]]`’.

`\S`
:   Matches any character that is not a space, as defined by the current locale.
    Think of it as shorthand for
    ‘`[^[:space:]]`’.

`\w`
:   Matches any word-constituent character—that is, it matches any
    letter, digit, or underscore. Think of it as shorthand for
    ‘`[[:alnum:]_]`’.

`\W`
:   Matches any character that is not word-constituent.
    Think of it as shorthand for
    ‘`[^[:alnum:]_]`’.

`\<`
:   Matches the empty string at the beginning of a word.
    For example, `/\<away/` matches ‘`away`’ but not
    ‘`stowaway`’.

`\>`
:   Matches the empty string at the end of a word.
    For example, `/stow\>/` matches ‘`stow`’ but not ‘`stowaway`’.

`\y` [¶](#index-word-boundaries_002c-matching)
:   Matches the empty string at either the beginning or the
    end of a word (i.e., the word boundar**y**). For example, ‘`\yballs?\y`’
    matches either ‘`ball`’ or ‘`balls`’, as a separate word.

`\B`
:   Matches the empty string that occurs between two
    word-constituent characters. For example,
    `/\Brat\B/` matches ‘`crate`’, but it does not match ‘`dirty rat`’.
    ‘`\B`’ is essentially the opposite of ‘`\y`’.
    Another way to think of this is that ‘`\B`’ matches the empty string
    provided it’s not at the edge of a word.

There are two other operators that work on buffers. In Emacs, a
*buffer* is, naturally, an Emacs buffer.
Other GNU programs, including `gawk`,
consider the entire string to match as the buffer.
The operators are:

`` \` ``
:   Matches the empty string at the
    beginning of a buffer (string)

`\'`
:   Matches the empty string at the
    end of a buffer (string)

Because ‘`^`’ and ‘`$`’ always work in terms of the beginning
and end of strings, these operators don’t add any new capabilities
for `awk`. They are provided for compatibility with other
GNU software.

In other GNU software, the word-boundary operator is ‘`\b`’. However,
that conflicts with the `awk` language’s definition of ‘`\b`’
as backspace, so `gawk` uses a different letter.
An alternative method would have been to require two backslashes in the
GNU operators, but this was deemed too confusing. The current
method of using ‘`\y`’ for the GNU ‘`\b`’ appears to be the
lesser of two evils.

| Backreferences Are Not Supported |
| --- |
| In POSIX Basic Regular Expressions (BREs), you can specify what are called *backreferences* in regular expressions. For instance, in `grep`, a regexp like ‘`\(..\)\1`’ lets you match two instances of the same subexpression in a row, such as ‘`abab`’ or ‘`2323`’.  This construct is not supported in POSIX Extended Regular Expressions (EREs) such as are used in `awk` and `gawk`. Besides being less efficient for matching, the numeric escape (‘`\1`’ in the example) would conflict with the ability to have octal escape sequences in regular expressions (see [Escape Sequences](#Escape-Sequences)).  This is true even though the underlying regexp matching engine(s) used by `gawk` or other `awk` implementations might support such a feature.  We are told that BusyBox `awk` (see [Other Freely Available `awk` Implementations](#Other-Versions)) is an exception, and that it allows backreferences, but only in dynamic regexps; for example ‘`"aa" ~ "(.)\1"`’, whereas it treats the ‘`\1`’ in ‘`"aa" ~ /(.)\1/`’ as an octal escape sequence. |

The various command-line options
(see [Command-Line Options](#Options))
control how `gawk` interprets characters in regexps:

No options
:   In the default case, `gawk` provides all the facilities of
    POSIX regexps and the
    previously described
    GNU regexp operators.
    GNU regexp operators described
    in [Regular Expression Operators](#Regexp-Operators).

`--posix`
:   Match only POSIX regexps; the GNU operators are not special
    (e.g., ‘`\w`’ matches a literal ‘`w`’). Interval expressions
    are allowed.

`--traditional` [¶](#index-Brian-Kernighan_0027s-awk-2)
:   Match traditional Unix `awk` regexps. The GNU operators
    are not special. Because BWK `awk` supports them,
    the POSIX character classes (‘`[[:alnum:]]`’, etc.) are available.
    So too, interval expressions are allowed.
    Characters described by octal and hexadecimal escape sequences are
    treated literally, even if they represent regexp metacharacters.

`--re-interval`
:   This option remains for backwards compatibility but no longer has any
    real effect.

---

Next: [Selecting the Regexp Matching Engine](#Choosing-Matchers), Previous: [`gawk`-Specific Regexp Operators](#GNU-Regexp-Operators), Up: [Regular Expressions](#Regexp)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.8 Case Sensitivity in Matching [¶](#Case-Sensitivity-in-Matching)

Case is normally significant in regular expressions, both when matching
ordinary characters (i.e., not metacharacters) and inside bracket
expressions. Thus, a ‘`w`’ in a regular expression matches only a lowercase
‘`w`’ and not an uppercase ‘`W`’.

The simplest way to do a case-independent match is to use a bracket
expression—for example, ‘`[Ww]`’. However, this can be cumbersome if
you need to use it often, and it can make the regular expressions harder
to read. There are two alternatives that you might prefer.

One way to perform a case-insensitive match at a particular point in the
program is to convert the data to a single case, using the
`tolower()` or `toupper()` built-in string functions (which we
haven’t discussed yet;
see [String-Manipulation Functions](#String-Functions)).
For example:

```
tolower($1) ~ /foo/  { ... }
```

converts the first field to lowercase before matching against it.
This works in any POSIX-compliant `awk`.

Another method, specific to `gawk`, is to set the variable
`IGNORECASE` to a nonzero value (see [Predefined Variables](#Built_002din-Variables)).
When `IGNORECASE` is not zero, *all* regexp and string
operations ignore case.

Changing the value of `IGNORECASE` dynamically controls the
case sensitivity of the program as it runs. Case is significant by
default because `IGNORECASE` (like most variables) is initialized
to zero:

```
x = "aB"
if (x ~ /ab/) ...   # this test will fail

IGNORECASE = 1
if (x ~ /ab/) ...   # now it will succeed
```

In general, you cannot use `IGNORECASE` to make certain rules
case insensitive and other rules case sensitive, as there is no
straightforward way
to set `IGNORECASE` just for the pattern of
a particular rule.[20](#FOOT20)
To do this, use either bracket expressions or `tolower()`. However, one
thing you can do with `IGNORECASE` only is dynamically turn
case sensitivity on or off for all the rules at once.

`IGNORECASE` can be set on the command line or in a `BEGIN` rule
(see [Other Command-Line Arguments](#Other-Arguments); also
see [Startup and Cleanup Actions](#Using-BEGIN_002fEND)).
Setting `IGNORECASE` from the command line is a way to make
a program case insensitive without having to edit it.

In multibyte locales, the equivalences between upper- and lowercase
characters are tested based on the wide-character values of the locale’s
character set. Prior to version 5.0, single-byte characters were
tested based on the ISO-8859-1 (ISO Latin-1) character set. However, as
of version 5.0, single-byte characters are also tested based on
the values of the locale’s character set.[21](#FOOT21)

The value of `IGNORECASE` has no effect if `gawk` is in
compatibility mode (see [Command-Line Options](#Options)).
Case is always significant in compatibility mode.

---

Next: [Summary](#Regexp-Summary), Previous: [Case Sensitivity in Matching](#Case_002dsensitivity), Up: [Regular Expressions](#Regexp)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.9 Selecting the Regexp Matching Engine [¶](#Selecting-the-Regexp-Matching-Engine)

Release 5.4.0 of `gawk` introduced a new
regular expression matching engine, named
[MinRX](https://github.com/mikehaertel/minrx).

MinRX is fully compliant with the POSIX standard for Extended
Regular Expressions (EREs), including the additional features needed
by `awk` and `gawk`. It is also a little stricter
that the original matchers are
in terms of accepting valid regular expression syntax when specifying
a regexp. (These restrictions apply to corner cases that should
not come up in day-to-day use.)

Previously, `gawk` used GNU `regex` and `dfa`
from GNULIB. These matchers are fast and generally robust, albeit not
fully POSIX compliant. MinRX replaces both of them.

Because regular expression matching is such a fundamental part of what
`awk` programs do, introducing a new regular expression engine
has some risk associated with it. To alleviate the risk, for the
term of one major release, `gawk` continues to provide access
to the original regexp matchers should that be needed.

If the environment variable `GAWK_GNU_MATCHERS` exists, then
`gawk` switches to using GNU `regex` and `dfa`,
as previously. Otherwise, the MinRX matcher is the default and
that is what it uses.

Should you find a need to switch from MinRX to the original matchers,
*please* submit a bug report describing what did not work
(see [Reporting Problems and Bugs](#Bugs)). Doing so is very important, as it will help the
maintainers and the MinRX author fix any issues that are found.

After one major release, the old matchers, and the use of the
`GAWK_GNU_MATCHERS` environment variable, will be removed
from `gawk`.

---

Previous: [Selecting the Regexp Matching Engine](#Choosing-Matchers), Up: [Regular Expressions](#Regexp)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.10 Summary [¶](#Summary-2)

* Regular expressions describe sets of strings to be matched.
  In `awk`, regular expression constants are written enclosed
  between slashes: `/`…`/`.
* Regexp constants may be used standalone in patterns and
  in conditional expressions, or as part of matching expressions
  using the ‘`~`’ and ‘`!~`’ operators.
* Escape sequences let you represent nonprintable characters and
  also let you represent regexp metacharacters as literal characters
  to be matched.
* Regexp operators provide grouping, alternation, and repetition.
* Bracket expressions give you a shorthand for specifying sets
  of characters that can match at a particular point in a regexp.
  Within bracket expressions, POSIX character classes let you specify
  certain groups of characters in a locale-independent fashion.
* Regular expressions match the leftmost longest text in the string being
  matched. This matters for cases where you need to know the extent of
  the match, such as for text substitution and when the record separator
  is a regexp.
* Matching expressions may use dynamic regexps (i.e., string values
  treated as regular expressions).
* `gawk`’s `IGNORECASE` variable lets you control the
  case sensitivity of regexp matching. In other `awk`
  versions, use `tolower()` or `toupper()`.

---

Next: [Printing Output](#Printing), Previous: [Regular Expressions](#Regexp), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 4 Reading Input Files [¶](#Reading-Input-Files)

In the typical `awk` program,
`awk` reads all input either from the
standard input (by default, this is the keyboard, but often it is a pipe from another
command) or from files whose names you specify on the `awk`
command line. If you specify input files, `awk` reads them
in order, processing all the data from one before going on to the next.
The name of the current input file can be found in the predefined variable
`FILENAME`
(see [Predefined Variables](#Built_002din-Variables)).

The input is read in units called *records*, and is processed by the
rules of your program one record at a time.
By default, each record is one line. Each
record is automatically split into chunks called *fields*.
This makes it more convenient for programs to work on the parts of a record.

On rare occasions, you may need to use the `getline` function.
The `getline` function is valuable both because it
can do explicit input from any number of files, and because the files
used with it do not have to be named on the `awk` command line
(see [Explicit Input with `getline`](#Getline)).

* [How Input Is Split into Records](#Records)
* [Examining Fields](#Fields)
* [Nonconstant Field Numbers](#Nonconstant-Fields)
* [Changing the Contents of a Field](#Changing-Fields)
* [Specifying How Fields Are Separated](#Field-Separators)
* [Reading Fixed-Width Data](#Constant-Size)
* [Defining Fields by Content](#Splitting-By-Content)
* [Checking How `gawk` Is Splitting Records](#Testing-field-creation)
* [Multiple-Line Records](#Multiple-Line)
* [Explicit Input with `getline`](#Getline)
* [Reading Input with a Timeout](#Read-Timeout)
* [Retrying Reads After Certain Input Errors](#Retrying-Input)
* [Directories on the Command Line](#Command_002dline-directories)
* [Summary](#Input-Summary)
* [Exercises](#Input-Exercises)

---

Next: [Examining Fields](#Fields), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.1 How Input Is Split into Records [¶](#How-Input-Is-Split-into-Records)

`awk` divides the input for your program into records and fields.
It keeps track of the number of records that have been read so far from
the current input file. This value is stored in a predefined variable
called `FNR`, which is reset to zero every time a new file is started.
Another predefined variable, `NR`, records the total number of input
records read so far from all data files. It starts at zero, but is
never automatically reset to zero.

Normally, records are separated by newline characters. You can control how
records are separated by assigning values to the built-in variable `RS`.
If `RS` is any single character, that character separates records.
Otherwise (in `gawk`), `RS` is treated as a regular expression.
This mechanism is explained in greater detail shortly.

> **NOTE:** When `gawk` is invoked with the `--csv` option, nothing
> in this section applies. See [Working With Comma Separated Value Files](#Comma-Separated-Fields), for the
> details.

* [Record Splitting with Standard `awk`](#awk-split-records)
* [Record Splitting with `gawk`](#gawk-split-records)

---

Next: [Record Splitting with `gawk`](#gawk-split-records), Up: [How Input Is Split into Records](#Records)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.1.1 Record Splitting with Standard `awk` [¶](#Record-Splitting-with-Standard-awk)

Records are separated by a character called the *record separator*.
By default, the record separator is the newline character.
This is why records are, by default, single lines.
To use a different character for the record separator,
simply assign that character to the predefined variable `RS`.

Like any other variable,
the value of `RS` can be changed in the `awk` program
with the assignment operator, ‘`=`’
(see [Assignment Expressions](#Assignment-Ops)).
The new record-separator character should be enclosed in quotation marks,
which indicate a string constant. Often, the right time to do this is
at the beginning of execution, before any input is processed,
so that the very first record is read with the proper separator.
To do this, use the special `BEGIN` pattern
(see [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)).
For example:

```
awk 'BEGIN { RS = "u" }
     { print $0 }' mail-list
```

changes the value of `RS` to ‘`u`’, before reading any input.
The new value is a string whose first character is the letter “u”; as a result, records
are separated by the letter “u”. Then the input file is read, and the second
rule in the `awk` program (the action with no pattern) prints each
record. Because each `print` statement adds a newline at the end of
its output, this `awk` program copies the input
with each ‘`u`’ changed to a newline. Here are the results of running
the program on `mail-list`:

```
$ awk 'BEGIN { RS = "u" }
>      { print $0 }' mail-list
```

```
-| Amelia       555-5553     amelia.zodiac
-| sq
-| e@gmail.com    F
-| Anthony      555-3412     anthony.assert
-| ro@hotmail.com   A
-| Becky        555-7685     becky.algebrar
-| m@gmail.com      A
-| Bill         555-1675     bill.drowning@hotmail.com       A
-| Broderick    555-0542     broderick.aliq
-| otiens@yahoo.com R
-| Camilla      555-2912     camilla.inf
-| sar
-| m@skynet.be     R
-| Fabi
-| s       555-1234     fabi
-| s.
-| ndevicesim
-| s@
-| cb.ed
-|     F
-| J
-| lie        555-6699     j
-| lie.perscr
-| tabor@skeeve.com   F
-| Martin       555-6480     martin.codicib
-| s@hotmail.com    A
-| Sam
-| el       555-3430     sam
-| el.lanceolis@sh
-| .ed
-|         A
-| Jean-Pa
-| l    555-2127     jeanpa
-| l.campanor
-| m@ny
-| .ed
-|      R
-|
```

Note that the entry for the name ‘`Bill`’ is not split.
In the original data file
(see [Data files for the Examples](#Sample-Data-Files)),
the line looks like this:

```
Bill         555-1675     bill.drowning@hotmail.com       A
```

It contains no ‘`u`’, so there is no reason to split the record,
unlike the others, which each have one or more occurrences of the ‘`u`’.
In fact, this record is treated as part of the previous record;
the newline separating them in the output
is the original newline in the data file, not the one added by
`awk` when it printed the record!

Another way to change the record separator is on the command line,
using the variable-assignment feature
(see [Other Command-Line Arguments](#Other-Arguments)):

```
awk '{ print $0 }' RS="u" mail-list
```

This sets `RS` to ‘`u`’ before processing `mail-list`.

Using an alphabetic character such as ‘`u`’ for the record separator
is highly likely to produce strange results.
Using an unusual character such as ‘`/`’ is more likely to
produce correct behavior in the majority of cases, but there
are no guarantees. The moral is: Know Your Data.

`gawk` allows `RS` to be a full regular expression
(discussed shortly; see [Record Splitting with `gawk`](#gawk-split-records)). Even so, using
a regular expression metacharacter, such as ‘`.`’ as the single
character in the value of `RS` has no special effect: it is
treated literally. This is required for backwards compatibility with
both Unix `awk` and with POSIX.

Reaching the end of an input file terminates the current input record,
even if the last character in the file is not the character in `RS`.
(d.c.)

The empty string `""` (a string without any characters)
has a special meaning
as the value of `RS`. It means that records are separated
by one or more blank lines and nothing else.
See [Multiple-Line Records](#Multiple-Line) for more details.

If you change the value of `RS` in the middle of an `awk` run,
the new value is used to delimit subsequent records, but the record
currently being processed, as well as records already processed, are not
affected.

After the end of the record has been determined, `gawk`
sets the variable `RT` to the text in the input that matched
`RS`.

---

Previous: [Record Splitting with Standard `awk`](#awk-split-records), Up: [How Input Is Split into Records](#Records)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.1.2 Record Splitting with `gawk` [¶](#Record-Splitting-with-gawk)

When using `gawk`, the value of `RS` is not limited to a
one-character string. If it contains more than one character, it is
treated as a regular expression
(see [Regular Expressions](#Regexp)). (c.e.)
In general, each record
ends at the next string that matches the regular expression; the next
record starts at the end of the matching string. This general rule is
actually at work in the usual case, where `RS` contains just a
newline: a record ends at the beginning of the next matching string (the
next newline in the input), and the following record starts just after
the end of this string (at the first character of the following line).
The newline, because it matches `RS`, is not part of either record.

When `RS` is a single character, `RT`
contains the same single character. However, when `RS` is a
regular expression, `RT` contains
the actual input text that matched the regular expression.

If the input file ends without any text matching `RS`,
`gawk` sets `RT` to the null string.

The following example illustrates both of these features.
It sets `RS` equal to a regular expression that
matches either a newline or a series of one or more uppercase letters
with optional leading and/or trailing whitespace:

```
$ echo record 1 AAAA record 2 BBBB record 3 |
> gawk 'BEGIN { RS = "\n|( *[[:upper:]]+ *)" }
>             { print "Record =", $0,"and RT = [" RT "]" }'
```

```
-| Record = record 1 and RT = [ AAAA ]
-| Record = record 2 and RT = [ BBBB ]
-| Record = record 3 and RT = [
-| ]
```

The square brackets delineate the contents of `RT`, letting you
see the leading and trailing whitespace. The final value of
`RT` is a newline.
See [A Simple Stream Editor](#Simple-Sed) for a more useful example
of `RS` as a regexp and `RT`.

If you set `RS` to a regular expression that allows optional
trailing text, such as ‘`RS = "abc(XYZ)?"`’, it is possible, due
to implementation constraints, that `gawk` may match the leading
part of the regular expression, but not the trailing part, particularly
if the input text that could match the trailing part is fairly long.
`gawk` attempts to avoid this problem, but currently, there’s
no guarantee that this will never happen.

| Caveats When Using Regular Expressions for `RS` |
| --- |
| Remember that in `awk`, the ‘`^`’ and ‘`$`’ anchor metacharacters match the beginning and end of a *string*, and not the beginning and end of a *line*. As a result, something like ‘`RS = "^[[:upper:]]"`’ can only match at the beginning of a file. This is because `gawk` views the input file as one long string that happens to contain newline characters. It is thus best to avoid anchor metacharacters in the value of `RS`.  Record splitting with regular expressions works differently than regexp matching with the `sub()`, `gsub()`, and `gensub()` (see [String-Manipulation Functions](#String-Functions)). Those functions allow a regexp to match the empty string; record splitting does not. Thus, for example ‘`RS = "()"`’ does *not* split records between characters. |

The use of `RS` as a regular expression and the `RT`
variable are `gawk` extensions; they are not available in
compatibility mode
(see [Command-Line Options](#Options)).
In compatibility mode, only the first character of the value of
`RS` determines the end of the record.

`mawk` has allowed `RS` to be a regexp for decades.
As of October, 2019, BWK `awk` also supports it. Neither
version supplies `RT`, however.

| `RS = "\0"` Is Not Portable |
| --- |
| There are times when you might want to treat an entire data file as a single record. The only way to make this happen is to give `RS` a value that you know doesn’t occur in the input file. This is hard to do in a general way, such that a program always works for arbitrary input files.  You might think that for text files, the NUL character, which consists of a character with all bits equal to zero, is a good value to use for `RS` in this case:  ``` BEGIN { RS = "\0" }  # whole file becomes one record? ```  `gawk` in fact accepts this, and uses the NUL character for the record separator. This works for certain special files, such as `/proc/environ` on GNU/Linux systems, where the NUL character is in fact the record separator. However, this usage is *not* portable to most other `awk` implementations.  Almost all other `awk` implementations[22](#FOOT22) store strings internally as C-style strings. C strings use the NUL character as the string terminator. In effect, this means that ‘`RS = "\0"`’ is the same as ‘`RS = ""`’. (d.c.)  It happens that recent versions of `mawk` can use the NUL character as a record separator. However, this is a special case: `mawk` does not allow embedded NUL characters in strings. (This may change in a future version of `mawk`.)    See [Reading a Whole File at Once](#Readfile-Function) for an interesting way to read whole files. If you are using `gawk`, see [Reading an Entire File](#Extension-Sample-Readfile) for another option. |

---

Next: [Nonconstant Field Numbers](#Nonconstant-Fields), Previous: [How Input Is Split into Records](#Records), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.2 Examining Fields [¶](#Examining-Fields)

When `awk` reads an input record, the record is
automatically *parsed* or separated by the `awk` utility into chunks
called *fields*. By default, fields are separated by *whitespace*,
like words in a line.
Whitespace in `awk` means any string of one or more spaces,
TABs, or newlines; other characters
that are considered whitespace by other languages
(such as formfeed, vertical tab, etc.) are *not* considered
whitespace by `awk`.

The purpose of fields is to make it more convenient for you to refer to
these pieces of the record. You don’t have to use them—you can
operate on the whole record if you want—but fields are what make
simple `awk` programs so powerful.

You use a dollar sign (‘`$`’)
to refer to a field in an `awk` program,
followed by the number of the field you want. Thus, `$1`
refers to the first field, `$2` to the second, and so on.
(Unlike in the Unix shells, the field numbers are not limited to single digits.
`$127` is the 127th field in the record.)
For example, suppose the following is a line of input:

```
This seems like a pretty nice example.
```

Here the first field, or `$1`, is ‘`This`’, the second field, or
`$2`, is ‘`seems`’, and so on. Note that the last field,
`$7`, is ‘`example.`’. Because there is no space between the
‘`e`’ and the ‘`.`’, the period is considered part of the seventh
field.

`NF` is a predefined variable whose value is the number of fields
in the current record. `awk` automatically updates the value
of `NF` each time it reads a record. No matter how many fields
there are, the last field in a record can be represented by `$NF`.
So, `$NF` is the same as `$7`, which is ‘`example.`’.
If you try to reference a field beyond the last
one (such as `$8` when the record has only seven fields), you get
the empty string. If used in a numeric operation, you get zero.[23](#FOOT23)

The use of `$0`, which looks like a reference to the “zeroth” field, is
a special case: it represents the whole input record. Use it
when you are not interested in specific fields.
Here are some more examples:

```
$ awk '$1 ~ /li/ { print $0 }' mail-list
-| Amelia       555-5553     amelia.zodiacusque@gmail.com    F
-| Julie        555-6699     julie.perscrutabor@skeeve.com   F
```

This example prints each record in the file `mail-list` whose first
field contains the string ‘`li`’.

By contrast, the following example looks for ‘`li`’ in *the
entire record* and prints the first and last fields for each matching
input record:

```
$ awk '/li/ { print $1, $NF }' mail-list
-| Amelia F
-| Broderick R
-| Julie F
-| Samuel A
```

---

Next: [Changing the Contents of a Field](#Changing-Fields), Previous: [Examining Fields](#Fields), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.3 Nonconstant Field Numbers [¶](#Nonconstant-Field-Numbers)

A field number need not be a constant. Any expression in
the `awk` language can be used after a ‘`$`’ to refer to a
field. The value of the expression specifies the field number. If the
value is a string, rather than a number, it is converted to a number.
Consider this example:

```
awk '{ print $NR }'
```

Recall that `NR` is the number of records read so far: one in the
first record, two in the second, and so on. So this example prints the first
field of the first record, the second field of the second record, and so
on. For the twentieth record, field number 20 is printed; most likely,
the record has fewer than 20 fields, so this prints a blank line.
Here is another example of using expressions as field numbers:

```
awk '{ print $(2*2) }' mail-list
```

`awk` evaluates the expression ‘`(2*2)`’ and uses
its value as the number of the field to print. The ‘`*`’
represents multiplication, so the expression ‘`2*2`’ evaluates to four.
The parentheses are used so that the multiplication is done before the
‘`$`’ operation; they are necessary whenever there is a binary
operator[24](#FOOT24)
in the field-number expression. This example, then, prints the
type of relationship (the fourth field) for every line of the file
`mail-list`. (All of the `awk` operators are listed, in
order of decreasing precedence, in
[Operator Precedence (How Operators Nest)](#Precedence).)

If the field number you compute is zero, you get the entire record.
Thus, ‘`$(2-2)`’ has the same value as `$0`.
Similarly, string expressions that evaluate to zero also yield the entire
record. For example, ‘`$"answer"`’, ‘`$"0foo"`’, or even
‘`$("foo" "bar")`’.

Negative field
numbers are not allowed; trying to reference one usually terminates
the program. (The POSIX standard does not define
what happens when you reference a negative field number. `gawk`
notices this and terminates your program. Other `awk`
implementations may behave differently.)

As mentioned in [Examining Fields](#Fields),
`awk` stores the current record’s number of fields in the built-in
variable `NF` (also see [Predefined Variables](#Built_002din-Variables)). Thus, the expression
`$NF` is not a special feature—it is the direct consequence of
evaluating `NF` and using its value as a field number.

---

Next: [Specifying How Fields Are Separated](#Field-Separators), Previous: [Nonconstant Field Numbers](#Nonconstant-Fields), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.4 Changing the Contents of a Field [¶](#Changing-the-Contents-of-a-Field)

The contents of a field, as seen by `awk`, can be changed within an
`awk` program; this changes what `awk` perceives as the
current input record. (The actual input is untouched; `awk` *never*
modifies the input file.)
Consider the following example and its output:

```
$ awk '{ nboxes = $3 ; $3 = $3 - 10
>        print nboxes, $3 }' inventory-shipped
-| 25 15
-| 32 22
-| 24 14
...
```

The program first saves the original value of field three in the variable
`nboxes`.
The ‘`-`’ sign represents subtraction, so this program reassigns
field three, `$3`, as the original value of field three minus ten:
‘`$3 - 10`’. (See [Arithmetic Operators](#Arithmetic-Ops).)
Then it prints the original and new values for field three.
(Someone in the warehouse made a consistent mistake while inventorying
the red boxes.)

For this to work, the text in `$3` must make sense
as a number; the string of characters must be converted to a number
for the computer to do arithmetic on it. The number resulting
from the subtraction is converted back to a string of characters that
then becomes field three.
See [Conversion of Strings and Numbers](#Conversion).

When the value of a field is changed (as perceived by `awk`), the
text of the input record is recalculated to contain the new field where
the old one was. In other words, `$0` changes to reflect the altered
field. Thus, this program
prints a copy of the input file, with 10 subtracted from the second
field of each line:

```
$ awk '{ $2 = $2 - 10; print $0 }' inventory-shipped
-| Jan 3 25 15 115
-| Feb 5 32 24 226
-| Mar 5 24 34 228
...
```

It is also possible to assign contents to fields that are out
of range. For example:

```
$ awk '{ $6 = ($5 + $4 + $3 + $2)
>        print $6 }' inventory-shipped
-| 168
-| 297
-| 301
...
```

We’ve just created `$6`, whose value is the sum of fields
`$2`, `$3`, `$4`, and `$5`. The ‘`+`’ sign
represents addition. For the file `inventory-shipped`, `$6`
represents the total number of parcels shipped for a particular month.

Creating a new field changes `awk`’s internal copy of the current
input record, which is the value of `$0`. Thus, if you do ‘`print $0`’
after adding a field, the record printed includes the new field, with
the appropriate number of field separators between it and the previously
existing fields.

This recomputation affects and is affected by
`NF` (the number of fields; see [Examining Fields](#Fields)).
For example, the value of `NF` is set to the number of the highest
field you create.
The exact format of `$0` is also affected by a feature that has not been discussed yet:
the *output field separator*, `OFS`,
used to separate the fields (see [Output Separators](#Output-Separators)).

Note, however, that merely *referencing* an out-of-range field
does *not* change the value of either `$0` or `NF`.
Referencing an out-of-range field only produces an empty string. For
example:

```
if ($(NF+1) != "")
    print "can't happen"
else
    print "everything is normal"
```

should print ‘`everything is normal`’, because `NF+1` is certain
to be out of range. (See [The `if`-`else` Statement](#If-Statement)
for more information about `awk`’s `if-else` statements.
See [Variable Typing and Comparison Expressions](#Typing-and-Comparison)
for more information about the ‘`!=`’ operator.)

It is important to note that making an assignment to an existing field
changes the
value of `$0` but does not change the value of `NF`,
even when you assign the empty string to a field. For example:

```
$ echo a b c d | awk '{ OFS = ":"; $2 = ""
>                       print $0; print NF }'
-| a::c:d
-| 4
```

The field is still there; it just has an empty value, delimited by
the two colons between ‘`a`’ and ‘`c`’.
This example shows what happens if you create a new field:

```
$ echo a b c d | awk '{ OFS = ":"; $2 = ""; $6 = "new"
>                       print $0; print NF }'
-| a::c:d::new
-| 6
```

The intervening field, `$5`, is created with an empty value
(indicated by the second pair of adjacent colons),
and `NF` is updated with the value six.

Decrementing `NF` throws away the values of the fields
after the new value of `NF` and recomputes `$0`.
(d.c.)
Here is an example:

```
$ echo a b c d e f | awk '{ print "NF =", NF;
>                           NF = 3; print $0 }'
-| NF = 6
-| a b c
```

> **CAUTION:** Some versions of `awk` don’t
> rebuild `$0` when `NF` is decremented.
> Until August, 2018, this included BWK `awk`; fortunately
> his version now handles this correctly.

Finally, there are times when it is convenient to force
`awk` to rebuild the entire record, using the current
values of the fields and `OFS`. To do this, use the
seemingly innocuous assignment:

```
$1 = $1   # force record to be reconstituted
print $0  # or whatever else with $0
```

This forces `awk` to rebuild the record. It does help
to add a comment, as we’ve shown here.

There is a flip side to the relationship between `$0` and
the fields. Any assignment to `$0` causes the record to be
reparsed into fields using the *current* value of `FS`.
This also applies to any built-in function that updates `$0`,
such as `sub()` and `gsub()`
(see [String-Manipulation Functions](#String-Functions)).

| Understanding `$0` |
| --- |
| It is important to remember that `$0` is the *full* record, exactly as it was read from the input. This includes any leading or trailing whitespace, and the exact whitespace (or other characters) that separates the fields.  It is a common error to try to change the field separators in a record simply by setting `FS` and `OFS`, and then expecting a plain ‘`print`’ or ‘`print $0`’ to print the modified record.  But this does not work, because nothing was done to change the record itself. Instead, you must force the record to be rebuilt, typically with a statement such as ‘`$1 = $1`’, as described earlier. |

---

Next: [Reading Fixed-Width Data](#Constant-Size), Previous: [Changing the Contents of a Field](#Changing-Fields), Up: [Reading Input Files](#Reading-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
