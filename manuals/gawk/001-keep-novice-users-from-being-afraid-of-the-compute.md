# keep novice users from being afraid of the computer.
BEGIN    { print "Don't Panic!" }
```

You can put comment lines into keyboard-composed throwaway `awk`
programs, but this usually isn’t very useful; the purpose of a
comment is to help you or another person understand the program
when reading it at a later time.

> **CAUTION:** As mentioned in
> [One-Shot Throwaway `awk` Programs](#One_002dshot),
> you can enclose short to medium-sized programs in single quotes,
> in order to keep
> your shell scripts self-contained. When doing so, *don’t* put
> an apostrophe (i.e., a single quote) into a comment (or anywhere else
> in your program). The shell interprets the quote as the closing
> quote for the entire program. As a result, usually the shell
> prints a message about mismatched quotes, and if `awk` actually
> runs, it will probably print strange messages about syntax errors.
> For example, look at the following:
>
> ```
> $ awk 'BEGIN { print "hello" } # let's be cute'
> >
> ```
>
> The shell sees that the first two quotes match, and that
> a new quoted object begins at the end of the command line.
> It therefore prompts with the secondary prompt, waiting for more input.
> With BWK `awk`, closing the quoted string produces this result:
>
> ```
> $ nawk '{ print "hello" } # let's be cute'
> > '
> error→ awk: can't open file be
> error→  source line number 1
> ```
>
>
>
> Putting a backslash before the single quote in ‘`let's`’ wouldn’t help,
> because backslashes are not special inside single quotes.
> The next subsection describes the shell’s quoting rules.

---

Previous: [Comments in `awk` Programs](#Comments), Up: [How to Run `awk` Programs](#Running-gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.1.6 Shell Quoting Issues [¶](#Shell-Quoting-Issues)

For short to medium-length `awk` programs, it is most convenient
to enter the program on the `awk` command line.
This is best done by enclosing the entire program in single quotes.
This is true whether you are entering the program interactively at
the shell prompt, or writing it as part of a larger shell script:

```
awk 'program text' input-file1 input-file2 ...
```

Once you are working with the shell, it is helpful to have a basic
knowledge of shell quoting rules. The following rules apply only to
POSIX-compliant, Bourne-style shells (such as Bash, the GNU Bourne-Again
Shell). If you use the C shell, you’re on your own.

Before diving into the rules, we introduce a concept that appears
throughout this Web page, which is that of the *null*,
or empty, string.

The null string is character data that has no value.
In other words, it is empty. It is written in `awk` programs
like this: `""`. In the shell, it can be written using single
or double quotes: `''` or `""`. Although the null string has
no characters in it, it does exist. For example, consider this command:

```
$ echo ""
```

Here, the `echo` utility receives a single argument, even
though that argument has no characters in it. In the rest of this
Web page, we use the terms *null string* and *empty string*
interchangeably. Now, on to the quoting rules:

* Quoted items can be concatenated with nonquoted items as well as with other
  quoted items. The shell turns everything into one argument for
  the command.
* Preceding any single character with a backslash (‘`\`’) quotes
  that character. The shell removes the backslash and passes the quoted
  character on to the command.
* Single quotes protect everything between the opening and closing quotes.
  The shell does no interpretation of the quoted text, passing it on verbatim
  to the command.
  It is *impossible* to embed a single quote inside single-quoted text.
  Refer back to
  [Comments in `awk` Programs](#Comments)
  for an example of what happens if you try.
* Double quotes protect most things between the opening and closing quotes.
  The shell does at least variable and command substitution on the quoted text.
  Different shells may do additional kinds of processing on double-quoted text.

  Because certain characters within double-quoted text are processed by the shell,
  they must be *escaped* within the text. Of note are the characters
  ‘`$`’, ‘`` ` ``’, ‘`\`’, and ‘`"`’, all of which must be preceded by
  a backslash within double-quoted text if they are to be passed on literally
  to the program. (The shell strips the leading backslash first.)
  Thus, the example seen
  previously
  in [Running `awk` Without Input Files](#Read-Terminal):

  ```
  awk 'BEGIN { print "Don\47t Panic!" }'
  ```

  could instead be written this way:

  ```
  $ awk "BEGIN { print \"Don't Panic!\" }"
  -| Don't Panic!
  ```

  Note that the single quote is not special within double quotes.
* Null strings are removed when they occur as part of a non-null
  command-line argument, while explicit null objects are kept.
  For example, to specify that the field separator `FS` should
  be set to the null string, use:

  ```
  awk -F "" 'program' files # correct
  ```

  Don’t use this:

  ```
  awk -F"" 'program' files  # wrong!
  ```

  In the second case, `awk` attempts to use the text of the program
  as the value of `FS`, and the first file name as the text of the program!
  This results in syntax errors at best, and confusing behavior at worst.

Mixing single and double quotes is difficult. You have to resort
to shell quoting tricks, like this:

```
$ awk 'BEGIN { print "Here is a single quote <'"'"'>" }'
-| Here is a single quote <'>
```

This program consists of three concatenated quoted strings. The first and the
third are single-quoted, and the second is double-quoted.

This can be “simplified” to:

```
$ awk 'BEGIN { print "Here is a single quote <'\''>" }'
-| Here is a single quote <'>
```

Judge for yourself which of these two is the more readable.

Another option is to use double quotes, escaping the embedded, `awk`-level
double quotes:

```
$ awk "BEGIN { print \"Here is a single quote <'>\" }"
-| Here is a single quote <'>
```

This option is also painful, because double quotes, backslashes, and dollar signs
are very common in more advanced `awk` programs.

A third option is to use the octal escape sequence equivalents
(see [Escape Sequences](#Escape-Sequences))
for the
single- and double-quote characters, like so:

```
$ awk 'BEGIN { print "Here is a single quote <\47>" }'
-| Here is a single quote <'>
$ awk 'BEGIN { print "Here is a double quote <\42>" }'
-| Here is a double quote <">
```

This works nicely, but you should comment clearly what the
escape sequences mean.

A fourth option is to use command-line variable assignment, like this:

```
$ awk -v sq="'" 'BEGIN { print "Here is a single quote <" sq ">" }'
-| Here is a single quote <'>
```

(Here, the two string constants and the value of `sq` are concatenated
into a single string that is printed by `print`.)

If you really need both single and double quotes in your `awk`
program, it is probably best to move it into a separate file, where
the shell won’t be part of the picture and you can say what you mean.

* [Quoting in MS-Windows Batch Files](#DOS-Quoting)

---

Up: [Shell Quoting Issues](#Quoting)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.1.6.1 Quoting in MS-Windows Batch Files [¶](#Quoting-in-MS_002dWindows-Batch-Files)

Although this Web page generally only worries about POSIX systems and the
POSIX shell, the following issue arises often enough for many users that
it is worth addressing.

The “shells” on Microsoft Windows systems use the double-quote
character for quoting, and make it difficult or impossible to include an
escaped double quote character in a command-line script. The following
example, courtesy of Jeroen Brink, shows how to escape the double quotes
from this one liner script that prints all lines in a file surrounded by
double quotes:

```
{ print "\"" $0 "\"" }
```

In an MS-Windows command-line the one-liner script above may be passed as
follows:

```
gawk "{ print \"\042\" $0 \"\042\" }" file
```

In this example the ‘`\042`’ is the octal code for a double quote;
`gawk` converts it into a real double-quote for output by
the `print` statement.

In MS-Windows escaping double quotes is a little tricky because you use
backslashes to escape double quotes, but backslashes themselves are not
escaped in the usual way; indeed they are either duplicated or not,
depending upon whether there is a subsequent double quote. The MS-Windows
rule for double-quoting a string is the following:

1. For each double quote in the original string, let N be the number
   of backslash(es) before it, N might be zero. Replace these N
   backslash(es) by *2\*N+1* backslash(es)
2. Let N be the number of backslash(es) tailing the original string,
   N might be zero. Replace these N backslash(es) by
   *2\*N* backslash(es)
3. Surround the resulting string by double quotes.

So to double-quote the one-liner script ‘`{ print "\"" $0 "\"" }`’
from the previous example you would do it this way:

```
gawk "{ print \"\\\"\" $0 \"\\\"\" }" file
```

However, the use of ‘`\042`’ instead of ‘`\\\"`’ is also possible
and easier to read, because backslashes that are not followed by a
double quote don’t need duplication.

---

Next: [Some Simple Examples](#Very-Simple), Previous: [How to Run `awk` Programs](#Running-gawk), Up: [Getting Started with `awk`](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.2 Data files for the Examples [¶](#Data-files-for-the-Examples)

Many of the examples in this Web page take their input from two sample
data files. The first, `mail-list`, represents a list of peoples’ names
together with their email addresses and information about those people.
The second data file, called `inventory-shipped`, contains
information about monthly shipments. In both files,
each line is considered to be one *record*.

In `mail-list`, each record contains the name of a person,
his/her phone number, his/her email address, and a code for his/her relationship
with the author of the list.
The columns are aligned using spaces.
An ‘`A`’ in the last column
means that the person is an acquaintance. An ‘`F`’ in the last
column means that the person is a friend.
An ‘`R`’ means that the person is a relative:

```
Amelia       555-5553     amelia.zodiacusque@gmail.com    F
Anthony      555-3412     anthony.asserturo@hotmail.com   A
Becky        555-7685     becky.algebrarum@gmail.com      A
Bill         555-1675     bill.drowning@hotmail.com       A
Broderick    555-0542     broderick.aliquotiens@yahoo.com R
Camilla      555-2912     camilla.infusarum@skynet.be     R
Fabius       555-1234     fabius.undevicesimus@ucb.edu    F
Julie        555-6699     julie.perscrutabor@skeeve.com   F
Martin       555-6480     martin.codicibus@hotmail.com    A
Samuel       555-3430     samuel.lanceolis@shu.edu        A
Jean-Paul    555-2127     jeanpaul.campanorum@nyu.edu     R
```

The data file `inventory-shipped` represents
information about shipments during the year.
Each record contains the month, the number
of green crates shipped, the number of red boxes shipped, the number of
orange bags shipped, and the number of blue packages shipped,
respectively. There are 16 entries, covering the 12 months of last year
and the first four months of the current year.
An empty line separates the data for the two years:

```
Jan  13  25  15 115
Feb  15  32  24 226
Mar  15  24  34 228
Apr  31  52  63 420
May  16  34  29 208
Jun  31  42  75 492
Jul  24  34  67 436
Aug  15  34  47 316
Sep  13  55  37 277
Oct  29  54  68 525
Nov  20  87  82 577
Dec  17  35  61 401

Jan  21  36  64 620
Feb  26  58  80 652
Mar  24  75  70 495
Apr  21  70  74 514
```

The sample files are included in the `gawk` distribution,
in the directory `awklib/eg/data`.

---

Next: [An Example with Two Rules](#Two-Rules), Previous: [Data files for the Examples](#Sample-Data-Files), Up: [Getting Started with `awk`](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.3 Some Simple Examples [¶](#Some-Simple-Examples)

The following command runs a simple `awk` program that searches the
input file `mail-list` for the character string ‘`li`’ (a
grouping of characters is usually called a *string*;
the term *string* is based on similar usage in English, such
as “a string of pearls” or “a string of cars in a train”):

```
awk '/li/ { print $0 }' mail-list
```

When lines containing ‘`li`’ are found, they are printed because
‘`print $0`’ means print the current line. (Just ‘`print`’ by
itself means the same thing, so we could have written that
instead.)

You will notice that slashes (‘`/`’) surround the string ‘`li`’
in the `awk` program. The slashes indicate that ‘`li`’
is the pattern to search for. This type of pattern is called a
*regular expression*, which is covered in more detail later
(see [Regular Expressions](#Regexp)).
The pattern is allowed to match parts of words.
There are
single quotes around the `awk` program so that the shell won’t
interpret any of it as special shell characters.

Here is what this program prints:

```
$ awk '/li/ { print $0 }' mail-list
-| Amelia       555-5553     amelia.zodiacusque@gmail.com    F
-| Broderick    555-0542     broderick.aliquotiens@yahoo.com R
-| Julie        555-6699     julie.perscrutabor@skeeve.com   F
-| Samuel       555-3430     samuel.lanceolis@shu.edu        A
```

In an `awk` rule, either the pattern or the action can be omitted,
but not both. If the pattern is omitted, then the action is performed
for *every* input line. If the action is omitted, the default
action is to print all lines that match the pattern.

Thus, we could leave out the action (the `print` statement and the
braces) in the previous example and the result would be the same:
`awk` prints all lines matching the pattern ‘`li`’. By comparison,
omitting the `print` statement but retaining the braces makes an
empty action that does nothing (i.e., no lines are printed).

Many practical `awk` programs are just a line or two long. Following is a
collection of useful, short programs to get you started. Some of these
programs contain constructs that haven’t been covered yet. (The description
of the program will give you a good idea of what is going on, but you’ll
need to read the rest of the Web page to become an `awk` expert!)
Most of the examples use a data file named `data`. This is just a
placeholder; if you use these programs yourself, substitute
your own file names for `data`.

Some of the following examples use the output of ‘`ls -l`’ as input.
`ls` is a system command that gives you a listing of the files in a
directory. With the `-l` option, this listing includes each file’s
size and the date the file was last modified. Its output looks like this:

```
-rw-r--r--  1 arnold   user   1933 Nov  7 13:05 Makefile
-rw-r--r--  1 arnold   user  10809 Nov  7 13:03 awk.h
-rw-r--r--  1 arnold   user    983 Apr 13 12:14 awk.tab.h
-rw-r--r--  1 arnold   user  31869 Jun 15 12:20 awkgram.y
-rw-r--r--  1 arnold   user  22414 Nov  7 13:03 awk1.c
-rw-r--r--  1 arnold   user  37455 Nov  7 13:03 awk2.c
-rw-r--r--  1 arnold   user  27511 Dec  9 13:07 awk3.c
-rw-r--r--  1 arnold   user   7989 Nov  7 13:03 awk4.c
```

The first field contains read-write permissions, the second field contains
the number of links to the file, and the third field identifies the
file’s owner. The fourth field identifies the file’s group. The fifth
field contains the file’s size in bytes. The sixth, seventh, and eighth
fields contain the month, day, and time, respectively, that the file
was last modified. Finally, the ninth field contains the file name.

For future reference, note that there is often more than
one way to do things in `awk`. At some point, you may want
to look back at these examples and see if
you can come up with different ways to do the same things shown here:

* Print every line that is longer than 80 characters:

  ```
  awk 'length($0) > 80' data
  ```

  The sole rule has a relational expression as its pattern and has no
  action—so it uses the default action, printing the record.
* Print the length of the longest input line:

  ```
  awk '{ if (length($0) > max) max = length($0) }
       END { print max }' data
  ```

  The code associated with `END` executes after all
  input has been read; it’s the other side of the coin to `BEGIN`.
* Print the length of the longest line in `data`:

  ```
  expand data | awk '{ if (x < length($0)) x = length($0) }
                     END { print "maximum line length is " x }'
  ```

  This example differs slightly from the previous one:
  the input is processed by the `expand` utility to change TABs
  into spaces, so the widths compared are actually the right-margin columns,
  as opposed to the number of input characters on each line.
* Print every line that has at least one field:

  ```
  awk 'NF > 0' data
  ```

  This is an easy way to delete blank lines from a file (or rather, to
  create a new file similar to the old file but from which the blank lines
  have been removed).
* Print seven random numbers from 0 to 100, inclusive:

  ```
  awk 'BEGIN { for (i = 1; i <= 7; i++)
                   print int(101 * rand()) }'
  ```
* Print the total number of bytes used by files:

  ```
  ls -l files | awk '{ x += $5 }
                     END { print "total bytes: " x }'
  ```
* Print the total number of kilobytes used by files:

  ```
  ls -l files | awk '{ x += $5 }
     END { print "total K-bytes:", x / 1024 }'
  ```
* Print a sorted list of the login names of all users:

  ```
  awk -F: '{ print $1 }' /etc/passwd | sort
  ```
* Count the lines in a file:

  ```
  awk 'END { print NR }' data
  ```
* Print the even-numbered lines in the data file:

  ```
  awk 'NR % 2 == 0' data
  ```

  If you used the expression ‘`NR % 2 == 1`’ instead,
  the program would print the odd-numbered lines.

---

Next: [A More Complex Example](#More-Complex), Previous: [Some Simple Examples](#Very-Simple), Up: [Getting Started with `awk`](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.4 An Example with Two Rules [¶](#An-Example-with-Two-Rules)

The `awk` utility reads the input files one line at a
time. For each line, `awk` tries the patterns of each rule.
If several patterns match, then several actions execute in the order in
which they appear in the `awk` program. If no patterns match, then
no actions run.

After processing all the rules that match the line (and perhaps there are none),
`awk` reads the next line. (However,
see [The `next` Statement](#Next-Statement)
and also see [The `nextfile` Statement](#Nextfile-Statement).)
This continues until the program reaches the end of the file.
For example, the following `awk` program contains two rules:

```
/12/  { print $0 }
/21/  { print $0 }
```

The first rule has the string ‘`12`’ as the
pattern and ‘`print $0`’ as the action. The second rule has the
string ‘`21`’ as the pattern and also has ‘`print $0`’ as the
action. Each rule’s action is enclosed in its own pair of braces.

This program prints every line that contains the string
‘`12`’ *or* the string ‘`21`’. If a line contains both
strings, it is printed twice, once by each rule.

This is what happens if we run this program on our two sample data files,
`mail-list` and `inventory-shipped`:

```
$ awk '/12/ { print $0 }
>      /21/ { print $0 }' mail-list inventory-shipped
-| Anthony      555-3412     anthony.asserturo@hotmail.com   A
-| Camilla      555-2912     camilla.infusarum@skynet.be     R
-| Fabius       555-1234     fabius.undevicesimus@ucb.edu    F
-| Jean-Paul    555-2127     jeanpaul.campanorum@nyu.edu     R
-| Jean-Paul    555-2127     jeanpaul.campanorum@nyu.edu     R
-| Jan  21  36  64 620
-| Apr  21  70  74 514
```

Note how the line beginning with ‘`Jean-Paul`’
in `mail-list` was printed twice, once for each rule.

---

Next: [`awk` Statements Versus Lines](#Statements_002fLines), Previous: [An Example with Two Rules](#Two-Rules), Up: [Getting Started with `awk`](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.5 A More Complex Example [¶](#A-More-Complex-Example)

Now that we’ve mastered some simple tasks, let’s look at
what typical `awk`
programs do. This example shows how `awk` can be used to
summarize, select, and rearrange the output of another utility. It uses
features that haven’t been covered yet, so don’t worry if you don’t
understand all the details:

```
ls -l | awk '$6 == "Nov" { sum += $5 }
             END { print sum }'
```

This command prints the total number of bytes in all the files in the
current directory that were last modified in November (of any year).

As a reminder, the output of ‘`ls -l`’ gives you a listing of the
files in a directory, including each file’s size and the date the file
was last modified. The first field contains read-write permissions,
the second field contains the number of links to the file, and the
third field identifies the file’s owner. The fourth field identifies
the file’s group. The fifth field contains the file’s size in bytes.
The sixth, seventh, and eighth fields contain the month, day, and time,
respectively, that the file was last modified. Finally, the ninth field
contains the file name.

The ‘`$6 == "Nov"`’ in our `awk` program is an expression that
tests whether the sixth field of the output from ‘`ls -l`’
matches the string ‘`Nov`’. Each time a line has the string
‘`Nov`’ for its sixth field, `awk` performs the action
‘`sum += $5`’. This adds the fifth field (the file’s size) to the variable
`sum`. As a result, when `awk` has finished reading all the
input lines, `sum` is the total of the sizes of the files whose
lines matched the pattern. (This works because `awk` variables
are automatically initialized to zero.)

After the last line of output from `ls` has been processed, the
`END` rule executes and prints the value of `sum`.
In this example, the value of `sum` is 80600.

These more advanced `awk` techniques are covered in later
sections
(see [Actions](#Action-Overview)). Before you can move on to more
advanced `awk` programming, you have to know how `awk` interprets
your input and displays your output. By manipulating fields and using
`print` statements, you can produce some very useful and
impressive-looking reports.

---

Next: [Other Features of `awk`](#Other-Features), Previous: [A More Complex Example](#More-Complex), Up: [Getting Started with `awk`](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.6 `awk` Statements Versus Lines [¶](#awk-Statements-Versus-Lines)

Most often, each line in an `awk` program is a separate statement or
separate rule, like this:

```
awk '/12/  { print $0 }
     /21/  { print $0 }' mail-list inventory-shipped
```

However, `gawk` ignores newlines after any of the following
symbols and keywords:

```
,    {    ?    :    ||    &&    do    else
```

A newline at any other point is considered the end of the
statement.[9](#FOOT9)

If you would like to split a single statement into two lines at a point
where a newline would terminate it, you can *continue* it by ending the
first line with a backslash character (‘`\`’). The backslash must be
the final character on the line in order to be recognized as a continuation
character. A backslash followed by a newline is allowed anywhere in the statement, even
in the middle of a string or regular expression. For example:

```
awk '/This regular expression is too long, so continue it\
 on the next line/ { print $1 }'
```

We have generally not used backslash continuation in our sample programs.
`gawk` places no limit on the
length of a line, so backslash continuation is never strictly necessary;
it just makes programs more readable. For this same reason, as well as
for clarity, we have kept most statements short in the programs
presented throughout the Web page.

Backslash continuation is
most useful when your `awk` program is in a separate source file
instead of entered from the command line. You should also note that
many `awk` implementations are more particular about where you
may use backslash continuation. For example, they may not allow you to
split a string constant using backslash continuation. Thus, for maximum
portability of your `awk` programs, it is best not to split your
lines in the middle of a regular expression or a string.

> **CAUTION:** *Backslash continuation does not work as described
> with the C shell.* It works for `awk` programs in files and
> for one-shot programs, *provided* you are using a POSIX-compliant
> shell, such as the Unix Bourne shell or Bash. But the C shell behaves
> differently! There you must use two backslashes in a row, followed by
> a newline. Note also that when using the C shell, *every* newline
> in your `awk` program must be escaped with a backslash. To illustrate:
>
> ```
> % awk 'BEGIN { \
> ?   print \\
> ?       "hello, world" \
> ? }'
> -| hello, world
> ```
>
> Here, the ‘`%`’ and ‘`?`’ are the C shell’s primary and secondary
> prompts, analogous to the standard shell’s ‘`$`’ and ‘`>`’.
>
> Compare the previous example to how it is done with a POSIX-compliant shell:
>
> ```
> $ awk 'BEGIN {
> >   print \
> >       "hello, world"
> > }'
> -| hello, world
> ```

`awk` is a line-oriented language. Each rule’s action has to
begin on the same line as the pattern. To have the pattern and action
on separate lines, you *must* use backslash continuation; there
is no other option.

Another thing to keep in mind is that backslash continuation and
comments do not mix. As soon as `awk` sees the ‘`#`’ that
starts a comment, it ignores *everything* on the rest of the
line. For example:

```
$ gawk 'BEGIN { print "don\47t panic" # a friendly \
>                                    BEGIN rule
> }'
error→ gawk: cmd. line:2:                BEGIN rule
error→ gawk: cmd. line:2:                ^ syntax error
```

In this case, it looks like the backslash would continue the comment onto the
next line. However, the backslash-newline combination is never even
noticed because it is “hidden” inside the comment. Thus, the
`BEGIN` is noted as a syntax error.

Backslash continuation comes into play in an additional, unexpected situation.
Consider:

```
gawk -F'\
a' '...'
```

This command line assigns a value to `FS`. But what value?
There are several possibilities, and in fact different versions of
`awk` do different things. `gawk` treats this as
if it were written:

```
BEGIN { FS = "\
a"
}
...
```

In short, the backslash and newline are removed, assigning `"a"`
to `FS`. This same treatment applies to variable assignments
made with the `-v` option (see [Command-Line Options](#Options))
and to regular command-line variable assignments (see [Assigning Variables on the Command Line](#Assignment-Options)).

If you’re interested, see
<https://lists.gnu.org/archive/html/bug-gawk/2022-10/msg00025.html>
for a source code patch that allows lines to be continued when inside
parentheses. This patch was not added to `gawk` since it would
quietly decrease the portability of `awk` programs.

When `awk` statements within one rule are short, you might want to put
more than one of them on a line. This is accomplished by separating the statements
with a semicolon (‘`;`’).
This also applies to the rules themselves.
Thus, the program shown at the start of this section
could also be written this way:

```
/12/ { print $0 } ; /21/ { print $0 }
```

> **NOTE:** The requirement that states that rules on the same line must be
> separated with a semicolon was not in the original `awk`
> language; it was added for consistency with the treatment of statements
> within an action.

---

Next: [When to Use `awk`](#When), Previous: [`awk` Statements Versus Lines](#Statements_002fLines), Up: [Getting Started with `awk`](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.7 Other Features of `awk` [¶](#Other-Features-of-awk)

The `awk` language provides a number of predefined, or
*built-in*, variables that your programs can use to get information
from `awk`. There are other variables your program can set
as well to control how `awk` processes your data.

In addition, `awk` provides a number of built-in functions for doing
common computational and string-related operations.
`gawk` provides built-in functions for working with timestamps,
performing bit manipulation, for runtime string translation (internationalization),
determining the type of a variable,
and array sorting.

As we develop our presentation of the `awk` language, we will introduce
most of the variables and many of the functions. They are described
systematically in [Predefined Variables](#Built_002din-Variables) and in
[Built-in Functions](#Built_002din).

---

Next: [Summary](#Intro-Summary), Previous: [Other Features of `awk`](#Other-Features), Up: [Getting Started with `awk`](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.8 When to Use `awk` [¶](#When-to-Use-awk)

Now that you’ve seen some of what `awk` can do,
you might wonder how `awk` could be useful for you. By using
utility programs, advanced patterns, field separators, arithmetic
statements, and other selection criteria, you can produce much more
complex output. The `awk` language is very useful for producing
reports from large amounts of raw data, such as summarizing information
from the output of other utility programs like `ls`.
(See [A More Complex Example](#More-Complex).)

Programs written with `awk` are usually much smaller than they would
be in other languages. This makes `awk` programs easy to compose and
use. Often, `awk` programs can be quickly composed at your keyboard,
used once, and thrown away. Because `awk` programs are interpreted, you
can avoid the (usually lengthy) compilation part of the typical
edit-compile-test-debug cycle of software development.

Complex programs have been written in `awk`, including a complete
retargetable assembler for
eight-bit microprocessors (see [Glossary](#Glossary), for more information),
and a microcode assembler for a special-purpose Prolog
computer.
The original `awk`’s capabilities were strained by tasks
of such complexity, but modern versions are more capable.

If you find yourself writing `awk` scripts of more than, say,
a few hundred lines, you might consider using a different programming
language. The shell is good at string and pattern matching; in addition,
it allows powerful use of the system utilities. Python offers a nice
balance between high-level ease of programming and access to system
facilities.[10](#FOOT10)

---

Previous: [When to Use `awk`](#When), Up: [Getting Started with `awk`](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.9 Summary [¶](#Summary)

* Programs in `awk` consist of pattern–action pairs.
* An action without a pattern always runs. The default
  action for a pattern without one is ‘`{ print $0 }`’.
* Use either
  ‘`awk 'program' files`’
  or
  ‘`awk -f program-file files`’
  to run `awk`.
* You may use the special ‘`#!`’ header line to create `awk`
  programs that are directly executable.
* Comments in `awk` programs start with ‘`#`’ and continue to
  the end of the same line.
* Be aware of quoting issues when writing `awk` programs as
  part of a larger shell script (or MS-Windows batch file).
* You may use backslash continuation to continue a source line.
  Lines are automatically continued after
  a comma, open brace, question mark, colon,
  ‘`||`’, ‘`&&`’, `do`, and `else`.

---

Next: [Regular Expressions](#Regexp), Previous: [Getting Started with `awk`](#Getting-Started), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 2 Running `awk` and `gawk` [¶](#Running-awk-and-gawk)

This chapter covers how to run `awk`, both POSIX-standard
and `gawk`-specific command-line options, and what
`awk` and
`gawk` do with nonoption arguments.
It then proceeds to cover how `gawk` searches for source files,
reading standard input along with other files, `gawk`’s
environment variables, `gawk`’s exit status, using include files,
and obsolete and undocumented options and/or features.

Many of the options and features described here are discussed in
more detail later in the Web page; feel free to skip over
things in this chapter that don’t interest you right now.

* [Invoking `awk`](#Command-Line)
* [Command-Line Options](#Options)
* [Other Command-Line Arguments](#Other-Arguments)
* [Naming Standard Input](#Naming-Standard-Input)
* [The Environment Variables `gawk` Uses](#Environment-Variables)
* [`gawk`’s Exit Status](#Exit-Status)
* [Including Other Files into Your Program](#Include-Files)
* [Loading Dynamic Extensions into Your Program](#Loading-Shared-Libraries)
* [Obsolete Options and/or Features](#Obsolete)
* [Undocumented Options and Features](#Undocumented)
* [Summary](#Invoking-Summary)

---

Next: [Command-Line Options](#Options), Up: [Running `awk` and `gawk`](#Invoking-Gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.1 Invoking `awk` [¶](#Invoking-awk)

There are two ways to run `awk`—with an explicit program or with
one or more program files. Here are templates for both of them; items
enclosed in […] in these templates are optional:

```
awk [options] -f progfile [--] file ...
awk [options] [--] 'program' file ...
```

In addition to traditional one-letter POSIX-style options, `gawk` also
supports GNU long options.

It is possible to invoke `awk` with an empty program:

```
awk '' datafile1 datafile2
```

Doing so makes little sense, though; `awk` exits
silently when given an empty program.
(d.c.)
If `--lint` has
been specified on the command line, `gawk` issues a
warning that the program is empty.

---

Next: [Other Command-Line Arguments](#Other-Arguments), Previous: [Invoking `awk`](#Command-Line), Up: [Running `awk` and `gawk`](#Invoking-Gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.2 Command-Line Options [¶](#Command_002dLine-Options)

Options begin with a dash and consist of a single character.
GNU-style long options consist of two dashes and a keyword.
The keyword can be abbreviated, as long as the abbreviation allows the option
to be uniquely identified. If the option takes an argument, either the
keyword is immediately followed by an equals sign (‘`=`’) and the
argument’s value, or the keyword and the argument’s value are separated
by whitespace (spaces or TABs).
If a particular option with a value is given more than once, it is (usually)
the last value that counts.

Each long option for `gawk` has a corresponding
POSIX-style short option.
The long and short options are
interchangeable in all contexts.
The following list describes options mandated by the POSIX standard:

`-F fs` [¶](#index-_002dF-option)

`--field-separator fs`
:   Set the `FS` variable to fs
    (see [Specifying How Fields Are Separated](#Field-Separators)).

`-f source-file` [¶](#index-_002df-option-1)

`--file source-file`
:   Read the `awk` program source from source-file
    instead of in the first nonoption argument.
    This option may be given multiple times; the `awk`
    program consists of the concatenation of the contents of
    each specified source-file.

    Files named with `-f` are treated as if they had ‘`@namespace "awk"`’
    at their beginning. See [Changing The Namespace](#Changing-The-Namespace), for more information
    on this advanced feature.

`-v var=val` [¶](#index-_002dv-option)

`--assign var=val`
:   Set the variable var to the value val *before*
    execution of the program begins. Such variable values are available
    inside the `BEGIN` rule
    (see [Other Command-Line Arguments](#Other-Arguments)).

    The `-v` option can only set one variable, but it can be used
    more than once, setting another variable each time, like this:
    ‘`awk -v foo=1 -v bar=2 …`’.

    > **CAUTION:** Using `-v` to set the values of the built-in
    > variables may lead to surprising results. `awk` will reset the
    > values of those variables as it needs to, possibly ignoring any
    > initial value you may have given.

`-W gawk-opt` [¶](#index-_002dW-option)
:   Provide an implementation-specific option.
    This is the POSIX convention for providing implementation-specific options.
    These options
    also have corresponding GNU-style long options.
    Note that the long options may be abbreviated, as long as
    the abbreviations remain unique.
    The full list of `gawk`-specific options is provided next.

`--` [¶](#index-command-line-options-end-of)
:   Signal the end of the command-line options. The following arguments
    are not treated as options even if they begin with ‘`-`’. This
    interpretation of `--` follows the POSIX argument parsing
    conventions.

    This is useful if you have file names that start with ‘`-`’,
    or in shell scripts, if you have file names that will be specified
    by the user that could start with ‘`-`’.
    It is also useful for passing options on to the `awk`
    program; see [Processing Command-Line Options](#Getopt-Function).

The following list describes `gawk`-specific options:

`-b` [¶](#index-_002db-option)

`--characters-as-bytes`
:   Cause `gawk` to treat all input data as single-byte characters.
    In addition, all output written with `print` or `printf`
    is treated as single-byte characters.

    Normally, `gawk` follows the POSIX standard and attempts to process
    its input data according to the current locale (see [Where You Are Makes a Difference](#Locales)). This can often involve
    converting multibyte characters into wide characters (internally), and
    can lead to problems or confusion if the input data does not contain valid
    multibyte characters. This option is an easy way to tell `gawk`,
    “Hands off my data!”

`-c` [¶](#index-_002dc-option)

`--traditional`
:   Specify *compatibility mode*, in which the GNU extensions to
    the `awk` language are disabled, so that `gawk` behaves just
    like BWK `awk`.
    See [Extensions in `gawk` Not in POSIX `awk`](#POSIX_002fGNU),
    which summarizes the extensions.
    Also see
    [Downward Compatibility and Debugging](#Compatibility-Mode).

`-C` [¶](#index-_002dC-option)

`--copyright`
:   Print the short version of the General Public License and then exit.

`-d`[file] [¶](#index-_002dd-option)

`--dump-variables`[`=`file]
:   Print a sorted list of global variables, their types, and final values
    to file. If no file is provided, print this
    list to a file named `awkvars.out` in the current directory.
    No space is allowed between the `-d` and file, if
    file is supplied.

    Having a list of all global variables is a good way to look for
    typographical errors in your programs.
    You would also use this option if you have a large program with a lot of
    functions, and you want to be sure that your functions don’t
    inadvertently use global variables that you meant to be local.
    (This is a particularly easy mistake to make with simple variable
    names like `i`, `j`, etc.)

`-D`[file] [¶](#index-_002dD-option)

`--debug`[`=`file]
:   Enable debugging of `awk` programs
    (see [Introduction to the `gawk` Debugger](#Debugging)).
    By default, the debugger reads commands interactively from the keyboard
    (standard input).
    The optional file argument allows you to specify a file with a list
    of commands for the debugger to execute noninteractively.
    No space is allowed between the `-D` and file, if
    file is supplied.

`-e` program-text [¶](#index-_002de-option)

`--source` program-text
:   Provide program source code in the program-text.
    This option allows you to mix source code in files with source
    code that you enter on the command line.
    This is particularly useful
    when you have library functions that you want to use from your command-line
    programs (see [The `AWKPATH` Environment Variable](#AWKPATH-Variable)).

    Note that `gawk` treats each string as if it ended with
    a newline character (even if it doesn’t). This makes building
    the total program easier.

    > **CAUTION:** Prior to version 5.0, there was
    > no requirement that each program-text
    > be a full syntactic unit. I.e., the following worked:
    >
    > ```
    > $ gawk -e 'BEGIN { a = 5 ;' -e 'print a }'
    > -| 5
    > ```
    >
    > However, this is no longer true. If you have any scripts that
    > rely upon this feature, you should revise them.
    >
    >
    >
    >
    > This is because each program-text is treated as if it had
    > ‘`@namespace "awk"`’ at its beginning. See [Changing The Namespace](#Changing-The-Namespace),
    > for more information.

`-E` file [¶](#index-_002dE-option)

`--exec` file
:   Similar to `-f`, read `awk` program text from file.
    There are two differences from `-f`:

    * This option terminates option processing; anything
      else on the command line is passed on directly to the `awk` program.
    * Command-line variable assignments of the form
      ‘`var=value`’ are disallowed.

    This option is particularly necessary for World Wide Web CGI applications
    that pass arguments through the URL; using this option prevents a malicious
    (or other) user from passing in options, assignments, or `awk` source
    code (via `-e`) to the CGI application.[11](#FOOT11)
    This option should be used
    with ‘`#!`’ scripts (see [Executable `awk` Programs](#Executable-Scripts)), like so:

    ```
    #! /usr/local/bin/gawk -E

    awk program here ...
    ```

`-g` [¶](#index-_002dg-option)

`--gen-pot`
:   Analyze the source program and
    generate a GNU `gettext` portable object template file on standard
    output for all string constants that have been marked for translation.
    See [Internationalization with `gawk`](#Internationalization),
    for information about this option.

`-h` [¶](#index-_002dh-option)

`--help`
:   Print a “usage” message summarizing the short- and long-style options
    that `gawk` accepts and then exit.

`-i` source-file [¶](#index-_002di-option)

`--include` source-file
:   Read an `awk` source library from source-file. This option
    is completely equivalent to using the `@include` directive inside
    your program. It is very similar to the `-f` option,
    but there are two important differences. First, when `-i` is
    used, the program source is not loaded if it has been previously
    loaded, whereas with `-f`, `gawk` always loads the file.
    Second, because this option is intended to be used with code libraries,
    `gawk` does not recognize such files as constituting main program
    input. Thus, after processing an `-i` argument, `gawk`
    still expects to find the main source code via the `-f` option
    or on the command line.

    Files named with `-i` are treated as if they had ‘`@namespace "awk"`’
    at their beginning. See [Changing The Namespace](#Changing-The-Namespace), for more information.

`-I` [¶](#index-_002dI-option)

`--trace`
:   Print the internal byte code names as they are executed when running
    the program. The trace is printed to standard error. Each “op code”
    is preceded by a `+`
    sign in the output.

`-k` [¶](#index-_002dk-option)

`--csv`
:   Enable special processing for files with comma separated values
    (CSV). See [Working With Comma Separated Value Files](#Comma-Separated-Fields).
    This option cannot be used with `--posix`. Attempting to do
    causes a fatal error.

`-l` ext [¶](#index-_002dl-option)

`--load` ext
:   Load a dynamic extension named ext. Extensions
    are stored as system shared libraries.
    This option searches for the library using the `AWKLIBPATH`
    environment variable. The correct library suffix for your platform will be
    supplied by default, so it need not be specified in the extension name.
    The extension initialization routine should be named `dl_load()`.
    An alternative is to use the `@load` directive inside the program to load
    a shared library. This advanced feature is described in detail in [Writing Extensions for `gawk`](#Dynamic-Extensions).

`-L`[value] [¶](#index-_002dl-option-1)

`--lint`[`=`value]
:   Warn about constructs that are dubious or nonportable to
    other `awk` implementations.
    No space is allowed between the `-L` and value, if
    value is supplied.
    Some warnings are issued when `gawk` first reads your program. Others
    are issued at runtime, as your program executes. The optional
    argument may be one of the following:

    `fatal`
    :   Cause lint warnings become fatal errors.
        This may be drastic, but its use will certainly encourage the
        development of cleaner `awk` programs.

    `invalid`
    :   Only issue warnings about things
        that are actually invalid are issued. (This is not fully implemented yet.)

    `no-ext`
    :   Disable warnings about `gawk` extensions.

    Some warnings are only printed once, even if the dubious constructs they
    warn about occur multiple times in your `awk` program. Thus,
    when eliminating problems pointed out by `--lint`, you should take
    care to search for all occurrences of each inappropriate construct. As
    `awk` programs are usually short, doing so is not burdensome.

`-M` [¶](#index-_002dM-option)

`--bignum`
:   Select arbitrary-precision arithmetic on numbers. This option has no effect
    if `gawk` is not compiled to use the GNU MPFR and MP libraries
    (see [Arithmetic and Arbitrary-Precision Arithmetic with `gawk`](#Arbitrary-Precision-Arithmetic)).

    As of version 5.2,
    the arbitrary precision arithmetic features in `gawk`
    are “on parole.”
    The primary maintainer is no longer willing to support this feature,
    but another member of the development team has stepped up to take it over.
    As long as this situation remains stable, MPFR will be supported. If it
    changes, the MPFR support will be removed from `gawk`.

`-n` [¶](#index-_002dn-option)

`--non-decimal-data`
:   Enable automatic interpretation of octal and hexadecimal
    values in input data
    (see [Allowing Nondecimal Input Data](#Nondecimal-Data)).

    > **CAUTION:** This option can severely break old programs. Use with care.

`-N` [¶](#index-_002dN-option)

`--use-lc-numeric`
:   Force the use of the locale’s decimal point character
    when parsing numeric input data (see [Where You Are Makes a Difference](#Locales)).

`-o`[file] [¶](#index-_002do-option)

`--pretty-print`[`=`file]
:   Enable pretty-printing of `awk` programs.
    Implies `--no-optimize`.
    By default, the output program is created in a file named `awkprof.out`
    (see [Profiling Your `awk` Programs](#Profiling)).
    The optional file argument allows you to specify a different
    file name for the output.
    No space is allowed between the `-o` and file, if
    file is supplied.

    > **NOTE:** In the past, this option would also execute your program.
    > This is no longer the case.

`-O` [¶](#index-_002d_002doptimize-option)

`--optimize`
:   Enable `gawk`’s default optimizations on the internal
    representation of the program. At the moment, this includes just simple
    constant folding.

    Optimization is enabled by default.
    This option remains primarily for backwards compatibility. However, it may
    be used to cancel the effect of an earlier `-s` option
    (see later in this list).

`-p`[file] [¶](#index-_002dp-option)

`--profile`[`=`file]
:   Enable profiling of `awk` programs
    (see [Profiling Your `awk` Programs](#Profiling)).
    Implies `--no-optimize`.
    By default, profiles are created in a file named `awkprof.out`.
    The optional file argument allows you to specify a different
    file name for the profile file.
    No space is allowed between the `-p` and file, if
    file is supplied.

    The profile contains execution counts for each statement in the program
    in the left margin, and function call counts for each function.

`-P` [¶](#index-_002dP-option)

`--posix`
:   Operate in strict POSIX mode. This disables all `gawk`
    extensions (just like `--traditional`) and
    disables all extensions not allowed by POSIX.
    See [Common Extensions Summary](#Common-Extensions) for a summary of the extensions
    in `gawk` that are disabled by this option.
    Also,
    the following additional
    restrictions apply:

    * Newlines are not allowed after ‘`?`’ or ‘`:`’
      (see [Conditional Expressions](#Conditional-Exp)).
    * Specifying ‘`-Ft`’ on the command line does not set the value
      of `FS` to be a single TAB character
      (see [Specifying How Fields Are Separated](#Field-Separators)).
    * The locale’s decimal point character is used for parsing input
      data (see [Where You Are Makes a Difference](#Locales)).

    If you supply both `--traditional` and `--posix` on the
    command line, `--posix` takes precedence. `gawk`
    issues a warning if both options are supplied.

`-r` [¶](#index-_002dr-option)

`--re-interval`
:   Allow interval expressions
    (see [Regular Expression Operators](#Regexp-Operators))
    in regexps.
    This is now `gawk`’s default behavior.
    Nevertheless, this option remains for backward compatibility.

`-s` [¶](#index-_002d_002dno_002doptimize-option)

`--no-optimize`
:   Disable `gawk`’s default optimizations on the internal
    representation of the program.

`-S` [¶](#index-_002dS-option)

`--sandbox`
:   Disable the `system()` function,
    input redirections with `getline`,
    output redirections with `print` and `printf`,
    and dynamic extensions.
    Also, disallow adding file names to `ARGV` that were
    not there when `gawk` started running.
    This is particularly useful when you want to run `awk` scripts
    from questionable sources and need to make sure the scripts
    can’t access your system (other than the specified input data files).

`-t` [¶](#index-_002dL-option)

`--lint-old`
:   Warn about constructs that are not available in the original version of
    `awk` from Version 7 Unix
    (see [Major Changes Between V7 and SVR3.1](#V7_002fSVR3_002e1)).

`-V` [¶](#index-_002dV-option)

`--version`
:   Print version information for this particular copy of `gawk`.
    This allows you to determine if your copy of `gawk` is up to date
    with respect to whatever the Free Software Foundation is currently
    distributing.
    It is also useful for bug reports
    (see [Reporting Problems and Bugs](#Bugs)).

`--`
:   Mark the end of all options.
    Any command-line arguments following `--` are placed in `ARGV`,
    even if they start with a minus sign.

In compatibility mode,
as long as program text has been supplied,
any other options are flagged as invalid with a warning message but
are otherwise ignored.

In compatibility mode, as a special case, if the value of fs supplied
to the `-F` option is ‘`t`’, then `FS` is set to the TAB
character (`"\t"`). This is true only for `--traditional` and not
for `--posix`
(see [Specifying How Fields Are Separated](#Field-Separators)).

The `-f` option may be used more than once on the command line.
If it is, `awk` reads its program source from all of the named files, as
if they had been concatenated together into one big file. This is
useful for creating libraries of `awk` functions. These functions
can be written once and then retrieved from a standard place, instead
of having to be included in each individual program.
The `-i` option is similar in this regard.
(As mentioned in
[Function Definition Syntax](#Definition-Syntax),
function names must be unique.)

With standard `awk`, library functions can still be used, even
if the program is entered at the keyboard,
by specifying ‘`-f /dev/tty`’. After typing your program,
type `Ctrl-d` (the end-of-file character) to terminate it.
(You may also use ‘`-f -`’ to read program source from the standard
input, but then you will not be able to also use the standard input as a
source of data.)

Because it is clumsy using the standard `awk` mechanisms to mix
source file and command-line `awk` programs, `gawk`
provides the `-e` option. This does not require you to
preempt the standard input for your source code, and it allows you to easily
mix command-line and library source code (see [The `AWKPATH` Environment Variable](#AWKPATH-Variable)).
As with `-f`, the `-e` and `-i`
options may also be used multiple times on the command line.

If no `-f` option (or `-e` option for `gawk`)
is specified, then `awk` uses the first nonoption command-line
argument as the text of the program source code. Arguments on
the command line that follow the program text are entered into the
`ARGV` array; `awk` does *not* continue to parse the
command line looking for options.

If the environment variable `POSIXLY_CORRECT` exists,
then `gawk` behaves in strict POSIX mode, exactly as if
you had supplied `--posix`.
Many GNU programs look for this environment variable to suppress
extensions that conflict with POSIX, but `gawk` behaves
differently: it suppresses all extensions, even those that do not
conflict with POSIX, and behaves in
strict POSIX mode. If `--lint` is supplied on the command line
and `gawk` turns on POSIX mode because of `POSIXLY_CORRECT`,
then it issues a warning message indicating that POSIX
mode is in effect.
You would typically set this variable in your shell’s startup file.
For a Bourne-compatible shell (such as Bash), you would add these
lines to the `.profile` file in your home directory:

```
POSIXLY_CORRECT=true
export POSIXLY_CORRECT
```

For a C shell-compatible
shell,[12](#FOOT12)
you would add this line to the `.login` file in your home directory:

```
setenv POSIXLY_CORRECT true
```

Having `POSIXLY_CORRECT` set is not recommended for daily use,
but it is good for testing the portability of your programs to other
environments.

---

Next: [Naming Standard Input](#Naming-Standard-Input), Previous: [Command-Line Options](#Options), Up: [Running `awk` and `gawk`](#Invoking-Gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.3 Other Command-Line Arguments [¶](#Other-Command_002dLine-Arguments)

Any additional arguments on the command line are normally treated as
input files to be processed in the order specified. However, an
argument that has the form `var=value`, assigns
the value value to the variable var—it does not specify a
file at all. (See [Assigning Variables on the Command Line](#Assignment-Options).) In the following example,
‘`count=1`’ is a variable assignment, not a file name:

```
awk -f program.awk file1 count=1 file2
```

As a side point, should you really need to have `awk`
process a file named `count=1` (or any file whose name looks like
a variable assignment), precede the file name with ‘`./`’, like so:

```
awk -f program.awk file1 ./count=1 file2
```

All the command-line arguments are made available to your `awk` program in the
`ARGV` array (see [Predefined Variables](#Built_002din-Variables)). Command-line options
and the program text (if present) are omitted from `ARGV`.
All other arguments, including variable assignments, are
included. As each element of `ARGV` is processed, `gawk`
sets `ARGIND` to the index in `ARGV` of the
current element. (`gawk` makes the full command line,
including program text and options, available in `PROCINFO["argv"]`;
see [Built-in Variables That Convey Information](#Auto_002dset).)

Changing `ARGC` and `ARGV` in your `awk` program lets
you control how `awk` processes the input files; this is described
in more detail in [Using `ARGC` and `ARGV`](#ARGC-and-ARGV).

The distinction between file name arguments and variable-assignment
arguments is made when `awk` is about to open the next input file.
At that point in execution, it checks the file name to see whether
it is really a variable assignment; if so, `awk` sets the variable
instead of reading a file.

Therefore, the variables actually receive the given values after all
previously specified files have been read. In particular, the values of
variables assigned in this fashion are *not* available inside a
`BEGIN` rule
(see [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)),
because such rules are run before `awk` begins scanning the argument list.

The variable values given on the command line are processed for escape
sequences (see [Escape Sequences](#Escape-Sequences)).
(d.c.)

In some very early implementations of `awk`, when a variable assignment
occurred before any file names, the assignment would happen *before*
the `BEGIN` rule was executed. `awk`’s behavior was thus
inconsistent; some command-line assignments were available inside the
`BEGIN` rule, while others were not. Unfortunately,
some applications came to depend
upon this “feature.” When `awk` was changed to be more consistent,
the `-v` option was added to accommodate applications that depended
upon the old behavior.

The variable assignment feature is most useful for assigning to variables
such as `RS`, `OFS`, and `ORS`, which control input and
output formats, before scanning the data files. It is also useful for
controlling state if multiple passes are needed over a data file. For
example:

```
awk 'pass == 1  { pass 1 stuff }
     pass == 2  { pass 2 stuff }' pass=1 mydata pass=2 mydata
```

Given the variable assignment feature, the `-F` option for setting
the value of `FS` is not
strictly necessary. It remains for historical compatibility.

| Quoting Shell Variables On The `awk` Command Line |
| --- |
| Small `awk` programs are often embedded in larger shell scripts, so it’s worthwhile to understand some shell basics. Consider the following:  ``` f="" awk '{ print("hi") }' $f ```  In this case, `awk` reads from standard input instead of trying to open any command line files. To the unwary, this looks like `awk` is hanging.  However `awk` doesn’t see an explicit empty string. When a variable expansion is the null string, *and* it’s not quoted, the shell simply removes it from the command line. To demonstrate:  ``` $ f="" $ awk 'BEGIN { print ARGC }' $f -| 1 $ awk 'BEGIN { print ARGC }' "$f" -| 2 ``` |

---

Next: [The Environment Variables `gawk` Uses](#Environment-Variables), Previous: [Other Command-Line Arguments](#Other-Arguments), Up: [Running `awk` and `gawk`](#Invoking-Gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.4 Naming Standard Input [¶](#Naming-Standard-Input-1)

Often, you may wish to read standard input together with other files.
For example, you may wish to read one file, read standard input coming
from a pipe, and then read another file.

The way to name the standard input, with all versions of `awk`,
is to use a single, standalone minus sign or dash, ‘`-`’. For example:

```
some_command | awk -f myprog.awk file1 - file2
```

Here, `awk` first reads `file1`, then it reads
the output of some\_command, and finally it reads
`file2`.

You may also use `"-"` to name standard input when reading
files with `getline` (see [Using `getline` from a File](#Getline_002fFile)).
And, you can even use `"-"` with the `-f` option
to read program source code from standard input (see [Command-Line Options](#Options)).

In addition, `gawk` allows you to specify the special
file name `/dev/stdin`, both on the command line and
with `getline`.
Some other versions of `awk` also support this, but it
is not standard.
(Some operating systems provide a `/dev/stdin` file
in the filesystem; however, `gawk` always processes
this file name itself.)

---

Next: [`gawk`’s Exit Status](#Exit-Status), Previous: [Naming Standard Input](#Naming-Standard-Input), Up: [Running `awk` and `gawk`](#Invoking-Gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.5 The Environment Variables `gawk` Uses [¶](#The-Environment-Variables-gawk-Uses)

A number of environment variables influence how `gawk`
behaves.

* [The `AWKPATH` Environment Variable](#AWKPATH-Variable)
* [The `AWKLIBPATH` Environment Variable](#AWKLIBPATH-Variable)
* [Other Environment Variables](#Other-Environment-Variables)

---

Next: [The `AWKLIBPATH` Environment Variable](#AWKLIBPATH-Variable), Up: [The Environment Variables `gawk` Uses](#Environment-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.1 The `AWKPATH` Environment Variable [¶](#The-AWKPATH-Environment-Variable)

In most `awk`
implementations, you must supply a precise pathname for each program
file, unless the file is in the current directory.
But with `gawk`, if the file name supplied to the `-f`
or `-i` options
does not contain a directory separator ‘`/`’, then `gawk` searches a list of
directories (called the *search path*) one by one, looking for a
file with the specified name.

The search path is a string consisting of directory names
separated by colons.[13](#FOOT13)
`gawk` gets its search path from the
`AWKPATH` environment variable. If that variable does not exist,
or if it has an empty value,
`gawk` uses a default path (described shortly).

The search path feature is particularly helpful for building libraries
of useful `awk` functions. The library files can be placed in a
standard directory in the default path and then specified on
the command line with a short file name. Otherwise, you would have to
type the full file name for each file.

By using the `-i` or `-f` options, your command-line
`awk` programs can use facilities in `awk` library files
(see [A Library of `awk` Functions](#Library-Functions)).
Path searching is always done, even if `gawk` is in compatibility mode.
This is true for both `--traditional` and `--posix`.
See [Command-Line Options](#Options).

If the source code file is not found after the initial search, the path is searched
again after adding the suffix ‘`.awk`’ to the file name.

`gawk`’s path search mechanism is similar
to the shell’s.
(See [The Bourne-Again SHell manual](https://www.gnu.org/software/bash/manual/).)
It treats a null entry in the path as indicating the current
directory.
(A null entry is indicated by starting or ending the path with a
colon or by placing two colons next to each other [‘`::`’].)

> **NOTE:** To include the current directory in the path, either place `.`
> as an entry in the path or write a null entry in the path.
>
> Different past versions of `gawk` would also look explicitly in
> the current directory, either before or after the path search. As of
> version 4.1.2, this no longer happens; if you wish to look
> in the current directory, you must include `.` either as a separate
> entry or as a null entry in the search path.

The default value for `AWKPATH` is
‘`.:/usr/local/share/awk`’.[14](#FOOT14) Since `.` is included at the beginning, `gawk`
searches first in the current directory and then in `/usr/local/share/awk`.
In practice, this means that you will rarely need to change the
value of `AWKPATH`.

See [Shell Startup Files](#Shell-Startup-Files), for information on functions that help to
manipulate the `AWKPATH` variable.

`gawk` places the value of the search path that it used into
`ENVIRON["AWKPATH"]`. This provides access to the actual search
path value from within an `awk` program.

Although you can change `ENVIRON["AWKPATH"]` within your `awk`
program, this has no effect on the running program’s behavior. This makes
sense: the `AWKPATH` environment variable is used to find the program
source files. Once your program is running, all the files have been
found, and `gawk` no longer needs to use `AWKPATH`.

---

Next: [Other Environment Variables](#Other-Environment-Variables), Previous: [The `AWKPATH` Environment Variable](#AWKPATH-Variable), Up: [The Environment Variables `gawk` Uses](#Environment-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.2 The `AWKLIBPATH` Environment Variable [¶](#The-AWKLIBPATH-Environment-Variable)

The `AWKLIBPATH` environment variable is similar to the `AWKPATH`
variable, but it is used to search for loadable extensions (stored as
system shared libraries) specified with the `-l` option rather
than for source files. If the extension is not found, the path is
searched again after adding the appropriate shared library suffix for
the platform. For example, on GNU/Linux systems, the suffix ‘`.so`’
is used. The search path specified is also used for extensions loaded
via the `@load` directive (see [Loading Dynamic Extensions into Your Program](#Loading-Shared-Libraries)).

If `AWKLIBPATH` does not exist in the environment, or if it has
an empty value, `gawk` uses a default path; this
is typically ‘`/usr/local/lib/gawk`’, although it can vary depending
upon how `gawk` was built.[15](#FOOT15)

See [Shell Startup Files](#Shell-Startup-Files), for information on functions that help to
manipulate the `AWKLIBPATH` variable.

`gawk` places the value of the search path that it used into
`ENVIRON["AWKLIBPATH"]`. This provides access to the actual search
path value from within an `awk` program.

Although you can change `ENVIRON["AWKLIBPATH"]` within your
`awk` program, this has no effect on the running program’s
behavior. This makes sense: the `AWKLIBPATH` environment variable
is used to find any requested extensions, and they are loaded before
the program starts to run. Once your program is running, all the
extensions have been found, and `gawk` no longer needs to use
`AWKLIBPATH`.

---

Previous: [The `AWKLIBPATH` Environment Variable](#AWKLIBPATH-Variable), Up: [The Environment Variables `gawk` Uses](#Environment-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.3 Other Environment Variables [¶](#Other-Environment-Variables-1)

A number of other environment variables affect `gawk`’s
behavior, but they are more specialized. Those in the following
list are meant to be used by regular users:

`GAWK_MSEC_SLEEP`
:   Specifies the interval between connection retries,
    in milliseconds. On systems that do not support
    the `usleep()` system call,
    the value is rounded up to an integral number of seconds.

`GAWK_PERSIST_FILE`
:   Specifies the backing file to use for persistent storage
    of `gawk`’s variables and arrays.
    See [Preserving Data Between Runs](#Persistent-Memory).

`GAWK_READ_TIMEOUT`
:   Specifies the time, in milliseconds, for `gawk` to
    wait for input before returning with an error.
    See [Reading Input with a Timeout](#Read-Timeout).

`GAWK_SOCK_RETRIES`
:   Controls the number of times `gawk` attempts to
    retry a two-way TCP/IP (socket) connection before giving up.
    See [Using `gawk` for Network Programming](#TCP_002fIP-Networking).
    Note that when nonfatal I/O is enabled (see [Enabling Nonfatal Output](#Nonfatal)),
    `gawk` only tries to open a TCP/IP socket once.

`PMA_VERBOSITY`
:   Controls the verbosity of the persistent memory allocator.
    See [Preserving Data Between Runs](#Persistent-Memory).

`POSIXLY_CORRECT`
:   Causes `gawk` to switch to POSIX-compatibility
    mode, disabling all traditional and GNU extensions.
    See [Command-Line Options](#Options).

The environment variables in the following list are meant
for use by the `gawk` developers for testing and tuning.
They are subject to change. The variables are:

`AWKBUFSIZE`
:   This variable only affects `gawk` on POSIX-compliant systems.
    With a value of ‘`exact`’, `gawk` uses the size of each input
    file as the size of the memory buffer to allocate for I/O. Otherwise,
    the value should be a number, and `gawk` uses that number as
    the size of the buffer to allocate. (When this variable is not set,
    `gawk` uses the smaller of the file’s size and the “default”
    blocksize, which is usually the filesystem’s I/O blocksize.)

`AWK_HASH`
:   If this variable exists with a value of ‘`gst`’, `gawk`
    switches to using the hash function from GNU Smalltalk for
    managing arrays.
    With a value of ‘`fnv1a`’, `gawk` uses the
    [FNV1-A hash function](http://www.isthe.com/chongo/tech/comp/fnv/index.html).
    These functions may be marginally faster than the standard function.

`AWKREADFUNC`
:   If this variable exists, `gawk` switches to reading source
    files one line at a time, instead of reading in blocks. This exists
    for debugging problems on filesystems on non-POSIX operating systems
    where I/O is performed in records, not in blocks.

`GAWK_MSG_SRC`
:   If this variable exists, `gawk` includes the file name
    and line number within the `gawk` source code
    from which warning and/or fatal messages
    are generated. Its purpose is to help isolate the source of a
    message, as there are multiple places that produce the
    same warning or error message.

`GAWK_LOCALE_DIR`
:   Specifies the location of compiled message object files
    for `gawk` itself. This is passed to the `bindtextdomain()`
    function when `gawk` starts up.

`GAWK_NO_DFA`
:   If this variable exists, `gawk` does not use the DFA regexp matcher
    for “does it match” kinds of tests. This can cause `gawk`
    to be slower. Its purpose is to help isolate differences between the
    two regexp matchers that `gawk` uses internally. (There aren’t
    supposed to be differences, but occasionally theory and practice don’t
    coordinate with each other.)

`GAWK_STACKSIZE`
:   This specifies the amount by which `gawk` should grow its
    internal evaluation stack, when needed.

`INT_CHAIN_MAX`
:   This specifies intended maximum number of items `gawk` will maintain on a
    hash chain for managing arrays indexed by integers.

`STR_CHAIN_MAX`
:   This specifies intended maximum number of items `gawk` will maintain on a
    hash chain for managing arrays indexed by strings.

`TIDYMEM`
:   If this variable exists, `gawk` uses the `mtrace()` library
    calls from the GNU C library to help track down possible memory leaks.
    This cannot be used together with the persistent memory allocator.

---

Next: [Including Other Files into Your Program](#Include-Files), Previous: [The Environment Variables `gawk` Uses](#Environment-Variables), Up: [Running `awk` and `gawk`](#Invoking-Gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.6 `gawk`’s Exit Status [¶](#gawk_0027s-Exit-Status)

If the `exit` statement is used with a value
(see [The `exit` Statement](#Exit-Statement)), then `gawk` exits with
the numeric value given to it.

Otherwise, if there were no problems during execution,
`gawk` exits with the value of the C constant
`EXIT_SUCCESS`. This is usually zero.

If an error occurs, `gawk` exits with the value of
the C constant `EXIT_FAILURE`. This is usually one.

If `gawk` exits because of a fatal error, the exit
status is two. On non-POSIX systems, this value may be mapped
to `EXIT_FAILURE`.

---

Next: [Loading Dynamic Extensions into Your Program](#Loading-Shared-Libraries), Previous: [`gawk`’s Exit Status](#Exit-Status), Up: [Running `awk` and `gawk`](#Invoking-Gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.7 Including Other Files into Your Program [¶](#Including-Other-Files-into-Your-Program)

This section describes a feature that is specific to `gawk`.
It discusses how one source file may include another.

The `@include` directive can be used to read external `awk` source
files. This gives you the ability to split large `awk` source files
into smaller, more manageable pieces, and also lets you reuse common `awk`
code from various `awk` scripts. In other words, you can group
together `awk` functions used to carry out specific tasks
into external files. These files can be used just like function libraries,
using the `@include` directive in conjunction with the `AWKPATH`
environment variable. Note that source files may also be included
using the `-i` option.

Let’s see an example.
We’ll start with two (trivial) `awk` scripts, namely
`test1` and `test2`. Here is the `test1` script:

```
BEGIN {
    print "This is script test1."
}
```

and here is `test2`:

```
@include "test1"
BEGIN {
    print "This is script test2."
}
```

Running `gawk` with `test2`
produces the following result:

```
$ gawk -f test2
-| This is script test1.
-| This is script test2.
```

`gawk` runs the `test2` script, which includes `test1`
using the `@include`
directive. So, to include external `awk` source files, you just
use `@include` followed by the name of the file to be included,
enclosed in double quotes.

> **NOTE:** Keep in mind that this is a language construct and the file name cannot
> be a string variable, but rather just a literal string constant in double quotes.

The files to be included may be nested; e.g., given a third
script, namely `test3`:

```
@include "test2"
BEGIN {
    print "This is script test3."
}
```

Running `gawk` with the `test3` script produces the
following results:

```
$ gawk -f test3
-| This is script test1.
-| This is script test2.
-| This is script test3.
```

The file name can, of course, be a pathname. For example:

```
@include "../io_funcs"
```

and:

```
@include "/usr/awklib/network"
```

are both valid. The `AWKPATH` environment variable can be of great
value when using `@include`. The same rules for the use
of the `AWKPATH` variable in command-line file searches
(see [The `AWKPATH` Environment Variable](#AWKPATH-Variable)) apply to
`@include` also.

This is very helpful in constructing `gawk` function libraries.
If you have a large script with useful, general-purpose `awk`
functions, you can break it down into library files and put those files
in a special directory. You can then include those “libraries,”
either by using the full pathnames of the files, or by setting the `AWKPATH`
environment variable accordingly and then using `@include` with
just the file part of the full pathname. Of course,
you can keep library files in more than one directory;
the more complex the working
environment is, the more directories you may need to organize the files
to be included.

Given the ability to specify multiple `-f` options, the
`@include` mechanism is not strictly necessary.
However, the `@include` directive
can help you in constructing self-contained `gawk` programs,
thus reducing the need for writing complex and tedious command lines.
In particular, `@include` is very useful for writing CGI scripts
to be run from web pages.

The `@include` directive and the `-i`/`--include`
command line option are completely equivalent. An included program
source is not loaded if it has been previously loaded.

The rules for finding a source file described in [The `AWKPATH` Environment Variable](#AWKPATH-Variable) also
apply to files loaded with `@include`.

Finally, files included with `@include`
are treated as if they had ‘`@namespace "awk"`’
at their beginning. See [Changing The Namespace](#Changing-The-Namespace), for more information.
However, there is a separate `@nsinclude` command which
does not change the namespace; see [Including A File Without Changing The Namespace](#Inclusion-For-Namespaces).

---

Next: [Obsolete Options and/or Features](#Obsolete), Previous: [Including Other Files into Your Program](#Include-Files), Up: [Running `awk` and `gawk`](#Invoking-Gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.8 Loading Dynamic Extensions into Your Program [¶](#Loading-Dynamic-Extensions-into-Your-Program)

This section describes a feature that is specific to `gawk`.

The `@load` directive can be used to read external `awk` extensions
(stored as system shared libraries).
This allows you to link in compiled code that may offer superior
performance and/or give you access to extended capabilities not supported
by the `awk` language. The `AWKLIBPATH` variable is used to
search for the extension. Using `@load` is completely equivalent
to using the `-l` command-line option.

If the extension is not initially found in `AWKLIBPATH`, another
search is conducted after appending the platform’s default shared library
suffix to the file name. For example, on GNU/Linux systems, the suffix
‘`.so`’ is used:

```
$ gawk '@load "ordchr"; BEGIN {print chr(65)}'
-| A
```

This is equivalent to the following example:

```
$ gawk -lordchr 'BEGIN {print chr(65)}'
-| A
```

For command-line usage, the `-l` option is more convenient,
but `@load` is useful for embedding inside an `awk` source file
that requires access to an extension.

[Writing Extensions for `gawk`](#Dynamic-Extensions), describes how to write extensions (in C or C++)
that can be loaded with either `@load` or the `-l` option.
It also describes the `ordchr` extension.

---

Next: [Undocumented Options and Features](#Undocumented), Previous: [Loading Dynamic Extensions into Your Program](#Loading-Shared-Libraries), Up: [Running `awk` and `gawk`](#Invoking-Gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.9 Obsolete Options and/or Features [¶](#Obsolete-Options-and_002for-Features)

This section describes features and/or command-line options from
previous releases of `gawk` that either are not available in the
current version or are still supported but deprecated (meaning that
they will *not* be in a future release).

As of
`gawk` version 5.2,
the arbitrary precision arithmetic feature is “on parole.”
This feature is now being
supported by a volunteer in the development team and not by the primary
maintainer. If this situation changes, then the feature will be removed.
For more information see [Arbitrary Precision Arithmetic is On Parole!](#MPFR-On-Parole).

---

Next: [Summary](#Invoking-Summary), Previous: [Obsolete Options and/or Features](#Obsolete), Up: [Running `awk` and `gawk`](#Invoking-Gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.10 Undocumented Options and Features [¶](#Undocumented-Options-and-Features)

> *Use the Source, Luke!*

— *Obi-Wan*

This section intentionally left
blank.

---

Previous: [Undocumented Options and Features](#Undocumented), Up: [Running `awk` and `gawk`](#Invoking-Gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
