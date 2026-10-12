### 4.1 `fmt`: Reformat paragraph text [¶](#fmt_003a-Reformat-paragraph-text)

`fmt` fills and joins lines to produce output lines of (at most)
a given number of characters (75 by default). Synopsis:

```
fmt [option]... [file]...
```

`fmt` reads from the specified file arguments (or standard
input if none are given), and writes to standard output.

By default, blank lines, spaces between words, and indentation are
preserved in the output; successive input lines with different
indentation are not joined; tabs are expanded on input and introduced on
output.

`fmt` prefers breaking lines at the end of a sentence, and tries to
avoid line breaks after the first word of a sentence or before the last
word of a sentence. A *sentence break* is defined as either the end
of a paragraph or a word ending in any of ‘`.?!`’, followed by two
spaces or end of line, ignoring any intervening parentheses or quotes.
Like TeX, `fmt` reads entire “paragraphs” before choosing line
breaks; the algorithm is a variant of that given by Donald E. Knuth
and Michael F. Plass in “Breaking Paragraphs Into Lines”,
Software: Practice & Experience **11**, 11 (November 1981),
1119–1184.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-c`’ [¶](#index-_002dc-1)

‘`--crown-margin`’
:   *Crown margin* mode: preserve the indentation of the first two
    lines within a paragraph, and align the left margin of each subsequent
    line with that of the second line.

‘`-t`’ [¶](#index-_002dt-2)

‘`--tagged-paragraph`’
:   *Tagged paragraph* mode: like crown margin mode, except that if
    indentation of the first line of a paragraph is the same as the
    indentation of the second, the first line is treated as a one-line
    paragraph.

‘`-s`’ [¶](#index-_002ds-4)

‘`--split-only`’
:   Split lines only. Do not join short lines to form longer ones. This
    prevents sample lines of code, and other such “formatted” text from
    being unduly combined.

‘`-u`’ [¶](#index-_002du-1)

‘`--uniform-spacing`’
:   Uniform spacing. Reduce spacing between words to one space, and spacing
    between sentences to two spaces.

‘`-width`’ [¶](#index-_002dwidth)

‘`-w width`’

‘`--width=width`’
:   Fill output lines up to width characters (default 75 or goal
    plus 10, if goal is provided).

‘`-g goal`’ [¶](#index-_002dg)

‘`--goal=goal`’
:   `fmt` initially tries to make lines goal characters wide.
    By default, this is 7% shorter than width.

‘`-p prefix`’

‘`--prefix=prefix`’
:   Only lines beginning with prefix (possibly preceded by whitespace)
    are subject to formatting. The prefix and any preceding whitespace are
    stripped for the formatting and then re-attached to each formatted output
    line. One use is to format certain kinds of program comments, while
    leaving the code unchanged.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`fold`: Wrap input lines to fit in specified width](#fold-invocation), Previous: [`fmt`: Reformat paragraph text](#fmt-invocation), Up: [Formatting file contents](#Formatting-file-contents)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 4.2 `pr`: Paginate or columnate files for printing [¶](#pr_003a-Paginate-or-columnate-files-for-printing)

`pr` writes each file (‘`-`’ means standard input), or
standard input if none are given, to standard output, paginating and
optionally outputting in multicolumn format; optionally merges all
files, printing all in parallel, one per column. Synopsis:

```
pr [option]... [file]...
```

By default, a 5-line header is printed at each page: two blank lines;
a line with the date, the file name, and the page count; and two more
blank lines. A footer of five blank lines is also printed.
The default page\_length is 66
lines. The default number of text lines is therefore 56.
The text line of the header takes the form
‘`date string page`’, with spaces inserted around
string so that the line takes up the full page\_width. Here,
date is the date (see the `-D` or `--date-format`
option for details), string is the centered header string, and
page identifies the page number. The `LC_MESSAGES` locale
category affects the spelling of page; in the default C locale, it
is ‘`Page number`’ where number is the decimal page
number.

Form feeds in the input cause page breaks in the output. Multiple form
feeds produce empty pages.

Columns are of equal width, separated by an optional string (default
is ‘`space`’). For multicolumn output, lines will always be truncated to
page\_width (default 72), unless you use the `-J` option.
For single
column output no line truncation occurs by default. Use `-W` option to
truncate lines in that case.

The program accepts the following options. Also see [Common options](#Common-options).

‘`+first_page[:last_page]`’

‘`--pages=first_page[:last_page]`’
:   Begin printing with page first\_page and stop with last\_page.
    Missing ‘`:last_page`’ implies end of file. While estimating
    the number of skipped pages each form feed in the input file results
    in a new page. Page counting with and without ‘`+first_page`’
    is identical. By default, counting starts with the first page of input
    file (not first page printed). Line numbering may be altered by `-N`
    option.

‘`-column`’ [¶](#index-_002dcolumn)

‘`--columns=column`’
:   With each single file, produce column columns of output
    (default is 1) and print columns down, unless `-a` is used. The
    column width is automatically decreased as column increases; unless
    you use the `-W/-w` option to increase page\_width as well.
    This option might well cause some lines to be truncated. The number of
    lines in the columns on each page are balanced. The options `-e`
    and `-i` are on for multiple text-column output. Together with
    `-J` option column alignment and line truncation is turned off.
    Since spaces are converted to TABs in multicolumn output, they can be converted
    back by further processing through `pr -t -e` or `expand`.
    Lines of full length are joined in a free field format and `-S`
    option may set field separators. `-column` may not be used
    with the `-m` option.

‘`-a`’ [¶](#index-_002da-1)

‘`--across`’
:   With each single file, print columns across rather than down. The
    `-column` option must be given with column greater than one.
    If a line is too long to fit in a column, it is truncated.

‘`-c`’ [¶](#index-_002dc-2)

‘`--show-control-chars`’
:   Print control characters using hat notation (e.g., ‘`^G`’); print
    other nonprinting characters in octal backslash notation. By default,
    nonprinting characters are not changed.

‘`-d`’ [¶](#index-_002dd-3)

‘`--double-space`’
:   Double space the output.

‘`-D format`’ [¶](#index-time-formats)

‘`--date-format=format`’
:   Format header dates using format, using the same conventions as
    for the command ‘`date +format`’. See [`date`: Print or set system date and time](#date-invocation).
    Except for directives, which start with
    ‘`%`’, characters in format are printed unchanged. You can use
    this option to specify an arbitrary string in place of the header date,
    e.g., `--date-format="Monday morning"`.

    The default date format is ‘`%Y-%m-%d %H:%M`’ (for example,
    ‘`2020-07-09 23:59`’);
    but if the `POSIXLY_CORRECT` environment variable is set
    and the `LC_TIME` locale category specifies the POSIX
    locale, the default is ‘`%b %e %H:%M %Y`’ (for example,
    ‘`Jul  9 23:59 2020`’.

    Timestamps are listed according to the time zone rules specified by
    the `TZ` environment variable, or by the system default rules if
    `TZ` is not set. See [Specifying the Time Zone
    with `TZ`](https://www.gnu.org/software/libc/manual/html_mono/libc.html#TZ-Variable) in The GNU C Library Reference Manual.

‘`-e[in-tabchar[in-tabwidth]]`’ [¶](#index-_002de-1)

‘`--expand-tabs[=in-tabchar[in-tabwidth]]`’
:   Expand tabs to spaces on input. Optional argument in-tabchar is
    the input tab character (default is the TAB character). Second optional
    argument in-tabwidth is the input tab character’s width (default
    is 8).

‘`-f`’ [¶](#index-_002dF)

‘`-F`’

‘`--form-feed`’
:   Use a form feed instead of newlines to separate output pages. This does
    not alter the default page length of 66 lines.

‘`-h header`’ [¶](#index-_002dh-2)

‘`--header=header`’
:   Replace the file name in the header with the centered string header.
    When using the shell, header should be quoted and should be
    separated from `-h` by a space.

‘`-i[out-tabchar[out-tabwidth]]`’ [¶](#index-_002di-3)

‘`--output-tabs[=out-tabchar[out-tabwidth]]`’
:   Replace spaces with tabs on output. Optional argument out-tabchar
    is the output tab character (default is the TAB character). Second optional
    argument out-tabwidth is the output tab character’s width (default
    is 8).

‘`-J`’ [¶](#index-_002dJ)

‘`--join-lines`’
:   Merge lines of full length. Used together with the column options
    `-column`, `-a -column` or `-m`. Turns off
    `-W/-w` line truncation;
    no column alignment used; may be used with
    `--sep-string[=string]`. `-J` has been introduced
    (together with `-W` and `--sep-string`)
    to disentangle the old (POSIX-compliant) options `-w` and
    `-s` along with the three column options.

‘`-l page_length`’ [¶](#index-_002dl-2)

‘`--length=page_length`’
:   Set the page length to page\_length (default 66) lines, including
    the lines of the header [and the footer]. If page\_length is less
    than or equal to 10, the header and footer are omitted, as if the
    `-t` option had been given.

‘`-m`’ [¶](#index-_002dm)

‘`--merge`’
:   Merge and print all files in parallel, one in each column. If a
    line is too long to fit in a column, it is truncated, unless the `-J`
    option is used. `--sep-string[=string]` may be used.
    Empty pages in
    some files (form feeds set) produce empty columns, still marked
    by string. The result is a continuous line numbering and column
    marking throughout the whole merged file. Completely empty merged pages
    show no separators or line numbers. The default header becomes
    ‘`date page`’ with spaces inserted in the middle; this
    may be used with the `-h` or `--header` option to fill up
    the middle blank part.

‘`-n[number-separator[digits]]`’ [¶](#index-_002dn-2)

‘`--number-lines[=number-separator[digits]]`’
:   Provide digits digit line numbering (default for digits is
    5). With multicolumn output the number occupies the first digits
    column positions of each text column or only each line of `-m`
    output. With single column output the number precedes each line just as
    `-m` does. Default counting of the line numbers starts with the
    first line of the input file (not the first line printed, compare the
    `--page` option and `-N` option).
    Optional argument number-separator is the character appended to
    the line number to separate it from the text followed. The default
    separator is the TAB character. In a strict sense a TAB is always
    printed with single column output only. The TAB width varies
    with the TAB position, e.g., with the left margin specified
    by `-o` option. With multicolumn output priority is given to
    ‘`equal width of output columns`’ (a POSIX specification).
    The TAB width is fixed to the value of the first column and does
    not change with different values of left margin. That means a
    fixed number of spaces is always printed in the place of the
    number-separator TAB. The tabification depends upon the output
    position.

‘`-N line_number`’ [¶](#index-_002dN-1)

‘`--first-line-number=line_number`’
:   Start line counting with the number line\_number at first line of
    first page printed (in most cases not the first line of the input file).

‘`-o margin`’ [¶](#index-_002do-1)

‘`--indent=margin`’
:   Indent each line with a margin margin spaces wide (default is zero).
    The total page width is the size of the margin plus the page\_width
    set with the `-W/-w` option. A limited overflow may occur with
    numbered single column output (compare `-n` option).

‘`-r`’ [¶](#index-_002dr-1)

‘`--no-file-warnings`’
:   Do not print a warning message when an argument file cannot be
    opened. (The exit status will still be nonzero, however.)

‘`-s[char]`’ [¶](#index-_002ds-5)

‘`--separator[=char]`’
:   Separate columns by a single character char. The default for
    char is the TAB character without `-w` and ‘`no
    character`’ with `-w`. Without `-s` the default separator
    ‘`space`’ is set. `-s[char]` turns off line truncation of all
    three column options (`-COLUMN`|`-a -COLUMN`|`-m`) unless
    `-w` is set. This is a POSIX-compliant formulation.

‘`-S[string]`’ [¶](#index-_002dS-2)

‘`--sep-string[=string]`’
:   Use string to separate output columns. The `-S` option doesn’t
    affect the `-W/-w` option, unlike the `-s` option which does. It
    does not affect line truncation or column alignment.
    Without `-S`, and with `-J`, `pr` uses the default output
    separator, TAB.
    Without `-S` or `-J`, `pr` uses a ‘`space`’
    (same as `-S" "`).
    If no ‘`string`’ argument is specified, ‘`""`’ is assumed.

‘`-t`’ [¶](#index-_002dt-3)

‘`--omit-header`’
:   Do not print the usual header [and footer] on each page, and do not fill
    out the bottom of pages (with blank lines or a form feed). No page
    structure is produced, but form feeds set in the input files are retained.
    The predefined pagination is not changed. `-t` or `-T` may be
    useful together with other options; e.g.: `-t -e4`, expand TAB characters
    in the input file to 4 spaces but don’t make any other changes. Use of
    `-t` overrides `-h`.

‘`-T`’ [¶](#index-_002dT-1)

‘`--omit-pagination`’
:   Do not print header [and footer]. In addition eliminate all form feeds
    set in the input files.

‘`-v`’ [¶](#index-_002dv-3)

‘`--show-nonprinting`’
:   Print nonprinting characters in octal backslash notation.

‘`-w page_width`’ [¶](#index-_002dw-4)

‘`--width=page_width`’
:   Set page width to page\_width characters for multiple text-column
    output only (default for page\_width is 72). The specified
    page\_width is rounded down so that columns have equal width.
    `-s[CHAR]` turns off the default page width and any line truncation
    and column alignment.
    Lines of full length are merged, regardless of the column options
    set. No page\_width setting is possible with single column output.
    A POSIX-compliant formulation.

‘`-W page_width`’ [¶](#index-_002dW)

‘`--page_width=page_width`’
:   Set the page width to page\_width characters, honored with and
    without a column option. With a column option, the specified page\_width
    is rounded down so that columns have equal width. Text lines are truncated,
    unless `-J` is used. Together with one of the three column options
    (`-column`, `-a -column` or `-m`) column
    alignment is always used. The separator options `-S` or `-s`
    don’t disable the `-W` option. Default is 72 characters. Without
    `-W page_width` and without any of the column options NO line
    truncation is used (defined to keep downward compatibility and to meet
    most frequent tasks). That’s equivalent to `-W 72 -J`. The header
    line is never truncated.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Previous: [`pr`: Paginate or columnate files for printing](#pr-invocation), Up: [Formatting file contents](#Formatting-file-contents)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 4.3 `fold`: Wrap input lines to fit in specified width [¶](#fold_003a-Wrap-input-lines-to-fit-in-specified-width)

`fold` writes each file (`-` means standard input), or
standard input if none are given, to standard output, breaking long
lines. Synopsis:

```
fold [option]... [file]...
```

By default, `fold` breaks lines wider than 80 columns. The output
is split into as many lines as necessary.

`fold` counts screen columns by default; thus, a tab may count more
than one column, backspace decreases the column count, and carriage
return sets the column to zero.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-b`’ [¶](#index-_002db-5)

‘`--bytes`’
:   Count bytes rather than columns, so that tabs, backspaces, and carriage
    returns are each counted as taking up one column, just like other
    characters.

‘`-s`’ [¶](#index-_002ds-6)

‘`--spaces`’
:   Break at word boundaries: the line is broken after the last blank before
    the maximum line length. If the line contains no such blanks, the line
    is broken at the maximum line length as usual.

‘`-w width`’ [¶](#index-_002dw-5)

‘`--width=width`’
:   Use a maximum line length of width columns instead of 80.

    For compatibility `fold` supports an obsolete option syntax
    `-width`. New scripts should use `-w width`
    instead.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [Summarizing files](#Summarizing-files), Previous: [Formatting file contents](#Formatting-file-contents), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 5 Output of parts of files [¶](#Output-of-parts-of-files-1)

These commands output pieces of the input.

* [`head`: Output the first part of files](#head-invocation)
* [`tail`: Output the last part of files](#tail-invocation)
* [`split`: Split a file into pieces.](#split-invocation)
* [`csplit`: Split a file into context-determined pieces](#csplit-invocation)

---

Next: [`tail`: Output the last part of files](#tail-invocation), Up: [Output of parts of files](#Output-of-parts-of-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 5.1 `head`: Output the first part of files [¶](#head_003a-Output-the-first-part-of-files)

`head` prints the first part (10 lines by default) of each
file; it reads from standard input if no files are given or
when given a file of `-`. Synopsis:

```
head [option]... [file]...
```

If more than one file is specified, `head` prints a
one-line header consisting of:

```
==> file name <==
```

before the output for each file.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-c [-]num`’ [¶](#index-_002dc-3)

‘`--bytes=[-]num`’
:   Print the first num bytes, instead of initial lines.
    However, if num is prefixed with a ‘`-`’,
    print all but the last num bytes of each file.
    num may be, or may be an integer optionally followed by,
    one of the following multiplicative suffixes:

    ```
    ‘b’  =>            512 ("blocks")
    ‘KB’ =>           1000 (KiloBytes)
    ‘K’  =>           1024 (KibiBytes)
    ‘MB’ =>      1000*1000 (MegaBytes)
    ‘M’  =>      1024*1024 (MebiBytes)
    ‘GB’ => 1000*1000*1000 (GigaBytes)
    ‘G’  => 1024*1024*1024 (GibiBytes)
    ```

    and so on for ‘`T`’, ‘`P`’, ‘`E`’, ‘`Z`’, ‘`Y`’,
    ‘`R`’, and ‘`Q`’.
    Binary prefixes can be used, too: ‘`KiB`’=‘`K`’, ‘`MiB`’=‘`M`’,
    and so on.

‘`-n [-]num`’ [¶](#index-_002dn-3)

‘`--lines=[-]num`’
:   Output the first num lines.
    However, if num is prefixed with a ‘`-`’,
    print all but the last num lines of each file.
    Size multiplier suffixes are the same as with the `-c` option.

‘`-q`’ [¶](#index-_002dq)

‘`--quiet`’

‘`--silent`’
:   Never print file name headers.

‘`-v`’ [¶](#index-_002dv-4)

‘`--verbose`’
:   Always print file name headers.

‘`-z`’ [¶](#index-_002dz)

‘`--zero-terminated`’
:   Delimit items with a zero byte rather than a newline (ASCII LF).
    I.e., treat input as items separated by ASCII NUL
    and terminate output items with ASCII NUL.
    This option can be useful in conjunction with ‘`perl -0`’ or
    ‘`find -print0`’ and ‘`xargs -0`’ which do the same in order to
    reliably handle arbitrary file names (even those containing blanks
    or other special characters).

For compatibility `head` also supports an obsolete option syntax
`-[num][bkm][cqv]`, which is recognized only if it is
specified first. num is a decimal number optionally followed
by a size letter (‘`b`’, ‘`k`’, ‘`m`’) as in `-c`, or
‘`l`’ to mean count by lines, or other option letters (‘`cqv`’).
Scripts intended for standard hosts should use `-c num`
or `-n num` instead. If your script must also run on
hosts that support only the obsolete syntax, it is usually simpler to
avoid `head`, e.g., by using ‘`sed 5q`’ instead of
‘`head -5`’.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`split`: Split a file into pieces.](#split-invocation), Previous: [`head`: Output the first part of files](#head-invocation), Up: [Output of parts of files](#Output-of-parts-of-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 5.2 `tail`: Output the last part of files [¶](#tail_003a-Output-the-last-part-of-files)

`tail` prints the last part (10 lines by default) of each
file; it reads from standard input if no files are given or
when given a file of ‘`-`’. Synopsis:

```
tail [option]... [file]...
```

If more than one file is specified, `tail` prints a
one-line header before the output for each file, consisting of:

```
==> file name <==
```

For further processing of tail output, it can be useful to convert the
file headers to line prefixes, which can be done like:

```
tail ... |
awk '
  /^==> .* <==$/ {prefix=substr($0,5,length-8)":"; next}
  {print prefix$0}
' | ...
```

GNU `tail` can output any amount of data (some other versions of
`tail` cannot). It also has no `-r` option (print in
reverse), since reversing a file is really a different job from printing
the end of a file; BSD `tail` (which is the one with `-r`) can
only reverse files that are at most as large as its buffer, which is
typically 32 KiB. A more reliable and versatile way to reverse files is
the GNU `tac` command.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-c [+]num`’ [¶](#index-_002dc-4)

‘`--bytes=[+]num`’
:   Output the last num bytes, instead of final lines.
    If num is prefixed with a ‘`+`’, start printing with
    byte num from the start of each file. For example to skip the first byte
    use `tail -c +2`, while to skip all but the last byte use `tail -c 1`.
    num may be, or may be an integer optionally followed by,
    one of the following multiplicative suffixes:

    ```
    ‘b’  =>            512 ("blocks")
    ‘KB’ =>           1000 (KiloBytes)
    ‘K’  =>           1024 (KibiBytes)
    ‘MB’ =>      1000*1000 (MegaBytes)
    ‘M’  =>      1024*1024 (MebiBytes)
    ‘GB’ => 1000*1000*1000 (GigaBytes)
    ‘G’  => 1024*1024*1024 (GibiBytes)
    ```

    and so on for ‘`T`’, ‘`P`’, ‘`E`’, ‘`Z`’, ‘`Y`’,
    ‘`R`’, and ‘`Q`’.
    Binary prefixes can be used, too: ‘`KiB`’=‘`K`’, ‘`MiB`’=‘`M`’,
    and so on.

‘`-f`’ [¶](#index-_002df-3)

‘`--follow[=how]`’
:   Loop forever trying to read more characters at the end of the file,
    presumably because the file is growing.
    If more than one file is given, `tail` prints a header whenever it
    gets output from a different file, to indicate which file that output is
    from.

    There are two ways to specify how you’d like to track files with this option,
    but that difference is noticeable only when a followed file is removed or
    renamed.
    If you’d like to continue to track the end of a growing file even after
    it has been unlinked, use `--follow=descriptor`. This is the default
    behavior, but it is not useful if you’re tracking a log file that may be
    rotated (removed or renamed, then reopened). In that case, use
    `--follow=name` to track the named file, perhaps by reopening it
    periodically to see if it has been removed and recreated by some other program.
    The inotify-based implementation handles this case without
    the need for any periodic reopening.

    No matter which method you use, if the tracked file is determined to have
    shrunk, `tail` prints a message saying the file has been truncated
    and resumes tracking from the start of the file, assuming it has been
    truncated to 0, which is the usual truncation operation for log files.

    When a file is removed, `tail`’s behavior depends on whether it is
    following the name or the descriptor. When following by name, tail can
    detect that a file has been removed and gives a message to that effect,
    and if `--retry` has been specified it will continue checking
    periodically to see if the file reappears.
    When following a descriptor, tail does not detect that the file has
    been unlinked or renamed and issues no message; even though the file
    may no longer be accessible via its original name, it may still be
    growing.

    The option values ‘`descriptor`’ and ‘`name`’ may be specified only
    with the long form of the option, not with `-f`.

    The `-f` option is ignored if
    no file operand is specified and standard input is a FIFO or a pipe.
    Likewise, the `-f` option has no effect for any
    operand specified as ‘`-`’, when standard input is a FIFO or a pipe.

    With kernel inotify support, output is triggered by file changes
    and is generally very prompt.
    Otherwise, `tail` sleeps for one second between checks –
    use `--sleep-interval=n` to change that default – which can
    make the output appear slightly less responsive or bursty.
    When using tail without inotify support, you can make it more responsive
    by using a sub-second sleep interval, e.g., via an alias like this:

    ```
    alias tail='tail -s.1'
    ```

‘`-F`’ [¶](#index-_002dF-1)
:   This option is the same as `--follow=name --retry`. That is, tail
    will attempt to reopen a file when it is removed. Should this fail, tail
    will keep trying until it becomes accessible again.

‘`--max-unchanged-stats=n`’ [¶](#index-_002d_002dmax_002dunchanged_002dstats)
:   When tailing a file by name, if there have been n (default
    n=5) consecutive
    iterations for which the file has not changed, then
    `open`/`fstat` the file to determine if that file name is
    still associated with the same device/inode-number pair as before.
    When following a log file that is rotated, this is approximately the
    number of seconds between when tail prints the last pre-rotation lines
    and when it prints the lines that have accumulated in the new log file.
    This option is meaningful only when polling (i.e., without inotify)
    and when following by name.

‘`-n [+]num`’ [¶](#index-_002dn-4)

‘`--lines=[+]`’
:   Output the last num lines.
    If num is prefixed with a ‘`+`’, start printing with
    line num from the start of each file. For example to skip the first line
    use `tail -n +2`, while to skip all but the last line use `tail -n 1`.
    Size multiplier suffixes are the same as with the `-c` option.

‘`--pid=pid`’ [¶](#index-_002d_002dpid)
:   When following by name or by descriptor, you may specify the process ID,
    pid, of one or more (by repeating `--pid`) writers of the
    file arguments. Then, shortly after all the identified
    processes terminate, tail will also terminate. This will
    work properly only if the writers and the tailing process are running on
    the same machine. For example, to save the output of a build in a file
    and to watch the file grow, if you invoke `make` and `tail`
    like this then the tail process will stop when your build completes.
    Without this option, you would have had to kill the `tail -f`
    process yourself.

    ```
    $ make >& makerr & tail --pid=$! -f makerr
    ```

    If you specify a pid that is not in use or that does not correspond
    to the process that is writing to the tailed files, then `tail`
    may terminate long before any files stop growing or it may not
    terminate until long after the real writer has terminated.
    On some systems, `--pid` is not supported and `tail`
    outputs a warning.

‘`-q`’ [¶](#index-_002dq-1)

‘`--quiet`’

‘`--silent`’
:   Never print file name headers.

‘`--retry`’ [¶](#index-_002d_002dretry)
:   Indefinitely try to open the specified file.
    This option is useful mainly when following (and otherwise issues a warning).

    When following by file descriptor (i.e., with `--follow=descriptor`),
    this option only affects the initial open of the file, as after a successful
    open, `tail` will start following the file descriptor.

    When following by name (i.e., with `--follow=name`), `tail`
    infinitely retries to re-open the given files until killed.

    Without this option, when `tail` encounters a file that doesn’t
    exist or is otherwise inaccessible, it reports that fact and
    never checks it again.

‘`-s number`’ [¶](#index-_002ds-7)

‘`--sleep-interval=number`’
:   Change the number of seconds to wait between iterations (the default is 1.0).
    During one iteration, every specified file is checked to see if it has
    changed size.
    When `tail` uses inotify, this polling-related option
    is usually ignored. However, if you also specify `--pid=p`,
    `tail` checks whether process p is alive at least
    every number seconds.
    The number must be non-negative and can be a floating-point number
    in either the current or the C locale. See [Floating point numbers](#Floating-point).

‘`-v`’ [¶](#index-_002dv-5)

‘`--verbose`’
:   Always print file name headers.

‘`-z`’ [¶](#index-_002dz-1)

‘`--zero-terminated`’
:   Delimit items with a zero byte rather than a newline (ASCII LF).
    I.e., treat input as items separated by ASCII NUL
    and terminate output items with ASCII NUL.
    This option can be useful in conjunction with ‘`perl -0`’ or
    ‘`find -print0`’ and ‘`xargs -0`’ which do the same in order to
    reliably handle arbitrary file names (even those containing blanks
    or other special characters).

For compatibility `tail` also supports an obsolete usage
‘`tail -[num][bcl][f] [file]`’, which is recognized
only if it does not conflict with the usage described
above. This obsolete form uses exactly one option and at most one
file. In the option, num is an optional decimal number optionally
followed by a size letter (‘`b`’, ‘`c`’, ‘`l`’) to mean count
by 512-byte blocks, bytes, or lines, optionally followed by ‘`f`’
which has the same meaning as `-f`.

On systems not conforming to POSIX 1003.1-2001, the leading ‘`-`’
can be replaced by ‘`+`’ in the traditional option syntax with the
same meaning as in counts, and on obsolete systems predating POSIX
1003.1-2001 traditional usage overrides normal usage when the two
conflict. This behavior can be controlled with the
`_POSIX2_VERSION` environment variable (see [Standards conformance](#Standards-conformance)).

Scripts intended for use on standard hosts should avoid traditional
syntax and should use `-c num[b]`, `-n
num`, and/or `-f` instead. If your script must also
run on hosts that support only the traditional syntax, you can often
rewrite it to avoid problematic usages, e.g., by using ‘`sed -n
'$p'`’ rather than ‘`tail -1`’. If that’s not possible, the script
can use a test like ‘`if tail -c +1 </dev/null >/dev/null 2>&1;
then …`’ to decide which syntax to use.

Even if your script assumes the standard behavior, you should still
beware usages whose behaviors differ depending on the POSIX
version. For example, avoid ‘`tail - main.c`’, since it might be
interpreted as either ‘`tail main.c`’ or as ‘`tail -- -
main.c`’; avoid ‘`tail -c 4`’, since it might mean either ‘`tail
-c4`’ or ‘`tail -c 10 4`’; and avoid ‘`tail +4`’, since it might
mean either ‘`tail ./+4`’ or ‘`tail -n +4`’.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`csplit`: Split a file into context-determined pieces](#csplit-invocation), Previous: [`tail`: Output the last part of files](#tail-invocation), Up: [Output of parts of files](#Output-of-parts-of-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 5.3 `split`: Split a file into pieces. [¶](#split_003a-Split-a-file-into-pieces_002e)

`split` creates output files containing consecutive or interleaved
sections of input (standard input if none is given or input
is ‘`-`’). Synopsis:

```
split [option] [input [prefix]]
```

By default, `split` puts 1000 lines of input (or whatever is
left over for the last section), into each output file.

The output files’ names consist of prefix (‘`x`’ by default)
followed by a group of characters (‘`aa`’, ‘`ab`’, … by
default), such that concatenating the output files in traditional
sorted order by file name produces the original input file (except
`-nr/n`). By default split will initially create files
with two generated suffix characters, and will increase this width by two
when the next most significant position reaches the last character.
(‘`yz`’, ‘`zaaa`’, ‘`zaab`’, …). In this way an arbitrary
number of output files are supported, which sort as described above,
even in the presence of an `--additional-suffix` option.
If the `-a` option is specified and the output file names are
exhausted, `split` reports an error without deleting the
output files that it did create.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-l lines`’ [¶](#index-_002dl-3)

‘`--lines=lines`’
:   Put lines lines of input into each output file.
    If `--separator` is specified, then lines determines
    the number of records.

    For compatibility `split` also supports an obsolete
    option syntax `-lines`. New scripts should use
    `-l lines` instead.

‘`-b size`’ [¶](#index-_002db-6)

‘`--bytes=size`’
:   Put size bytes of input into each output file.
    size may be, or may be an integer optionally followed by,
    one of the following multiplicative suffixes:

    ```
    ‘b’  =>            512 ("blocks")
    ‘KB’ =>           1000 (KiloBytes)
    ‘K’  =>           1024 (KibiBytes)
    ‘MB’ =>      1000*1000 (MegaBytes)
    ‘M’  =>      1024*1024 (MebiBytes)
    ‘GB’ => 1000*1000*1000 (GigaBytes)
    ‘G’  => 1024*1024*1024 (GibiBytes)
    ```

    and so on for ‘`T`’, ‘`P`’, ‘`E`’, ‘`Z`’, ‘`Y`’,
    ‘`R`’, and ‘`Q`’.
    Binary prefixes can be used, too: ‘`KiB`’=‘`K`’, ‘`MiB`’=‘`M`’,
    and so on.

‘`-C size`’ [¶](#index-_002dC)

‘`--line-bytes=size`’
:   Put into each output file as many complete lines of input as
    possible without exceeding size bytes. Individual lines or records
    longer than size bytes are broken into multiple files.
    size has the same format as for the `--bytes` option.
    If `--separator` is specified, then lines determines
    the number of records.

‘`--filter=command`’ [¶](#index-_002d_002dfilter)
:   With this option, rather than simply writing to each output file,
    write through a pipe to the specified shell command for each output file.
    command should use the $FILE environment variable, which is set
    to a different output file name for each invocation of the command.
    For example, imagine that you have a 1TiB compressed file
    that, if uncompressed, would be too large to reside on secondary storage,
    yet you must split it into individually-compressed pieces
    of a more manageable size.
    To do that, you might run this command:

    ```
    xz -dc BIG.xz | split -b200G --filter='xz > $FILE.xz' - big-
    ```

    Assuming a 10:1 compression ratio, that would create about fifty 20GiB files
    with names `big-aa.xz`, `big-ab.xz`, `big-ac.xz`, etc.

‘`-n chunks`’ [¶](#index-_002dn-5)

‘`--number=chunks`’
:   Split input to chunks output files where chunks may be:

    ```
    n      generate n files based on current size of input
    k/n    output only kth of n to standard output
    l/n    generate n files without splitting lines or records
    l/k/n  likewise but output only kth of n to stdout
    r/n    like ‘l’ but use round robin distribution
    r/k/n  likewise but output only kth of n to stdout
    ```

    If the input size is not a multiple of n, early output files are
    one byte longer than later output files, to make up the difference.
    Any excess bytes appearing after the initial calculation are discarded
    (except when using ‘`r`’ mode).

    All n files are created even if there are fewer than n lines,
    or the input is truncated.

    For ‘`l`’ mode, chunks are approximately input size / n.
    Although the input is still partitioned as before into n regions
    of approximately equal size, if a line *starts* within a partition
    it is written completely to the corresponding file. Since lines or records
    are not split even if they overlap a partition, the files written
    can be larger or smaller than the partition size, and even empty
    if a line/record is so long as to completely overlap the partition.

    When the input is a pipe or some other special file where the size
    cannot easily be determined, there is no trouble for ‘`r`’ mode
    because the size of the input is irrelevant. For other modes, such an
    input is first copied to a temporary to determine its size.

‘`-a length`’ [¶](#index-_002da-2)

‘`--suffix-length=length`’
:   Use suffixes of length length. If a length of 0 is specified,
    this is the same as if (any previous) `-a` was not specified, and
    thus enables the default behavior, which starts the suffix length at 2,
    and unless `-n` or `--numeric-suffixes=from` is
    specified, will auto increase the length by 2 as required.

‘`-d`’ [¶](#index-_002dd-4)

‘`--numeric-suffixes[=from]`’
:   Use digits in suffixes rather than lower-case letters. The numerical
    suffix counts from from if specified, 0 otherwise.

    from is supported with the long form option, and is used to either set the
    initial suffix for a single run, or to set the suffix offset for independently
    split inputs, and consequently the auto suffix length expansion described above
    is disabled. Therefore you may also want to use option `-a` to allow
    suffixes beyond ‘`99`’. If option `--number` is specified and
    the number of files is less than from, a single run is assumed and the
    minimum suffix length required is automatically determined.

‘`-x`’ [¶](#index-_002dx-1)

‘`--hex-suffixes[=from]`’
:   Like `--numeric-suffixes`, but use hexadecimal numbers (in lower case).

‘`--additional-suffix=suffix`’ [¶](#index-_002d_002dadditional_002dsuffix)
:   Append an additional suffix to output file names. suffix
    must not contain slash.

‘`-e`’ [¶](#index-_002de-2)

‘`--elide-empty-files`’
:   Suppress the generation of zero-length output files. This can happen
    with the `--number` option if a file is (truncated to be) shorter
    than the number requested, or if a line is so long as to completely
    span a chunk. The output file sequence numbers, always run consecutively
    even when this option is specified.

‘`-t separator`’ [¶](#index-_002dt-4)

‘`--separator=separator`’
:   Use character separator as the record separator instead of the default
    newline character (ASCII LF).
    To specify ASCII NUL as the separator, use the two-character string ‘`\0`’,
    e.g., ‘`split -t '\0'`’.

‘`-u`’ [¶](#index-_002du-2)

‘`--unbuffered`’
:   Immediately copy input to output in `--number r/…` mode,
    which is a much slower mode of operation.

‘`--verbose`’ [¶](#index-_002d_002dverbose-2)
:   Write a diagnostic just before each output file is opened.

An exit status of zero indicates success,
and a nonzero value indicates failure.

Here are a few examples to illustrate how the
`--number` (`-n`) option works:

Notice how, by default, one line may be split onto two or more:

```
$ seq -w 6 10 > k; split -n3 k; head xa?
==> xaa <==
06
07
==> xab <==

08
0
==> xac <==
9
10
```

Use the "l/" modifier to suppress that:

```
$ seq -w 6 10 > k; split -nl/3 k; head xa?
==> xaa <==
06
07

==> xab <==
08
09

==> xac <==
10
```

Use the "r/" modifier to distribute lines in a round-robin fashion:

```
$ seq -w 6 10 > k; split -nr/3 k; head xa?
==> xaa <==
06
09

==> xab <==
07
10

==> xac <==
08
```

You can also extract just the Kth chunk.
This extracts and prints just the 7th "chunk" of 33:

```
$ seq 100 > k; split -nl/7/33 k
20
21
22
```

---

Previous: [`split`: Split a file into pieces.](#split-invocation), Up: [Output of parts of files](#Output-of-parts-of-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 5.4 `csplit`: Split a file into context-determined pieces [¶](#csplit_003a-Split-a-file-into-context_002ddetermined-pieces)

`csplit` creates zero or more output files containing sections of
input (standard input if input is ‘`-`’). Synopsis:

```
csplit [option]... input pattern...
```

The contents of the output files are determined by the pattern
arguments, as detailed below. An error occurs if a pattern
argument refers to a nonexistent line of the input file (e.g., if no
remaining line matches a given regular expression). After every
pattern has been matched, any remaining input is copied into one
last output file.

By default, `csplit` prints the number of bytes written to each
output file after it has been created.

The types of pattern arguments are:

‘`n`’
:   Create an output file containing the input up to but not including line
    n (a positive integer). If followed by a repeat count, also
    create an output file containing the next n lines of the input
    file once for each repeat.

‘`/regexp/[offset]`’
:   Create an output file containing the current line up to (but not
    including) the next line of the input file that contains a match for
    regexp. The optional offset is an integer, that can
    be preceded by ‘`+`’ or ‘`-`’.
    If it is given, the input up to (but not including) the
    matching line plus or minus offset is put into the output file,
    and the line after that begins the next section of input.
    Lines within a negative offset of a regexp pattern
    are not matched in subsequent regexp patterns.

‘`%regexp%[offset]`’
:   Like the previous type, except that it does not create an output
    file, so that section of the input file is effectively ignored.

‘`{repeat-count}`’
:   Repeat the previous pattern repeat-count additional
    times. The repeat-count can either be a positive integer or an
    asterisk, meaning repeat as many times as necessary until the input is
    exhausted.

The output files’ names consist of a prefix (‘`xx`’ by default)
followed by a suffix. By default, the suffix is an ascending sequence
of two-digit decimal numbers from ‘`00`’ to ‘`99`’. In any case,
concatenating the output files in sorted order by file name produces the
original input file, excluding portions skipped with a %regexp%
pattern or the `--suppress-matched` option.

By default, if `csplit` encounters an error or receives a hangup,
interrupt, quit, or terminate signal, it removes any output files
that it has created so far before it exits.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-f prefix`’ [¶](#index-_002df-4)

‘`--prefix=prefix`’
:   Use prefix as the output file name prefix.

‘`-b format`’ [¶](#index-_002db-7)

‘`--suffix-format=format`’
:   Use format as the output file name suffix. When this option is
    specified, the suffix string must include exactly one
    `printf(3)`-style conversion specification, possibly including
    format specification flags, a field width, a precision specification,
    or all of these kinds of modifiers. The format letter must convert a
    binary unsigned integer argument to readable form. The format letters
    ‘`d`’ and ‘`i`’ are aliases for ‘`u`’, and the
    ‘`u`’, ‘`o`’, ‘`x`’, and ‘`X`’ conversions are allowed. The
    entire format is given (with the current output file number) to
    `sprintf(3)` to form the file name suffixes for each of the
    individual output files in turn. If this option is used, the
    `--digits` option is ignored.

‘`-n digits`’ [¶](#index-_002dn-6)

‘`--digits=digits`’
:   Use output file names containing numbers that are digits digits
    long instead of the default 2.

‘`-k`’ [¶](#index-_002dk-1)

‘`--keep-files`’
:   Do not remove output files when errors are encountered.

‘`--suppress-matched`’ [¶](#index-_002d_002dsuppress_002dmatched)
:   Do not output lines matching the specified pattern.
    I.e., suppress the boundary line from the start of the second
    and subsequent splits.

‘`-z`’ [¶](#index-_002dz-2)

‘`--elide-empty-files`’
:   Suppress the generation of zero-length output files. (In cases where
    the section delimiters of the input file are supposed to mark the first
    lines of each of the sections, the first output file will generally be a
    zero-length file unless you use this option.) The output file sequence
    numbers always run consecutively starting from 0, even when this option
    is specified.

‘`-s`’ [¶](#index-_002ds-8)

‘`-q`’

‘`--silent`’

‘`--quiet`’
:   Do not print counts of output file sizes.

An exit status of zero indicates success,
and a nonzero value indicates failure.

Here is an example of its usage.
First, create an empty directory for the exercise,
and cd into it:

```
$ mkdir d && cd d
```

Now, split the sequence of 1..14 on lines that end with 0 or 5:

```
$ seq 14 | csplit - '/[05]$/' '{*}'
8
10
15
```

Each number printed above is the size of an output
file that csplit has just created.
List the names of those output files:

```
$ ls
xx00  xx01  xx02
```

Use `head` to show their contents:

```
$ head xx*
==> xx00 <==
1
2
3
4

==> xx01 <==
5
6
7
8
9

==> xx02 <==
10
11
12
13
14
```

Example of splitting input by empty lines:

```
$ csplit --suppress-matched input.txt '/^$/' '{*}'
```

---

Next: [Operating on sorted files](#Operating-on-sorted-files), Previous: [Output of parts of files](#Output-of-parts-of-files), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 6 Summarizing files [¶](#Summarizing-files-1)

These commands generate just a few numbers representing entire
contents of files.

* [`wc`: Print newline, word, and byte counts](#wc-invocation)
* [`sum`: Print checksum and block counts](#sum-invocation)
* [`cksum`: Print and verify file checksums](#cksum-invocation)
* [`md5sum`: Print or check MD5 digests](#md5sum-invocation)
* [`b2sum`: Print or check BLAKE2 digests](#b2sum-invocation)
* [`sha1sum`: Print or check SHA-1 digests](#sha1sum-invocation)
* [sha2 utilities: Print or check SHA-2 digests](#sha2-utilities)

---

Next: [`sum`: Print checksum and block counts](#sum-invocation), Up: [Summarizing files](#Summarizing-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 6.1 `wc`: Print newline, word, and byte counts [¶](#wc_003a-Print-newline_002c-word_002c-and-byte-counts)

`wc` counts the number of bytes, characters, words, and newlines
in each given file, or standard input if none are given
or for a file of ‘`-`’. A word is a nonempty sequence of non white
space delimited by white space characters or by start or end of input.
Synopsis:

```
wc [option]... [file]...
```

`wc` prints one line of counts for each file, and if the file was
given as an argument, it prints the file name following the counts. By default
if more than one file is given, `wc` prints a final line
containing the cumulative counts, with the file name `total`.
This ‘`total`’ line can be controlled with the `--total` option,
which is a GNU extension.
The counts are printed in this order: newlines, words, characters, bytes,
maximum line length.
Each count is printed right-justified in a field with at least one
space between fields so that the numbers and file names normally line
up nicely in columns. The width of the count fields varies depending
on the inputs, so you should not depend on a particular field width.
However, as a GNU extension, if only one count is printed,
it is guaranteed to be printed without leading spaces.

By default, `wc` prints three counts: the newline, words, and byte
counts. Options can specify that only certain counts be printed.
Options do not undo others previously given, so

```
wc --bytes --words
```

prints both the byte counts and the word counts.

With the `--max-line-length` option, `wc` prints the length
of the longest line per file, and if there is more than one file it
prints the maximum (not the sum) of those lengths. The line lengths here
are measured in screen columns, according to the current locale and
assuming tab positions in every 8th column.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-c`’ [¶](#index-_002dc-5)

‘`--bytes`’
:   Print only the byte counts.

‘`-m`’ [¶](#index-_002dm-1)

‘`--chars`’
:   Print only the character counts, as per the current locale.
    Encoding errors are not counted.

‘`-w`’ [¶](#index-_002dw-6)

‘`--words`’
:   Print only the word counts. A word is a nonempty sequence of non white
    space delimited by white space characters or by start or end of input.
    The current locale determines which characters are white space.
    GNU `wc` treats encoding errors as non white space.

    Unless the environment variable `POSIXLY_CORRECT` is set,
    GNU `wc` treats the following Unicode characters as white
    space even if the current locale does not: U+00A0 NO-BREAK SPACE,
    U+2007 FIGURE SPACE, U+202F NARROW NO-BREAK SPACE, and U+2060 WORD
    JOINER.

‘`-l`’ [¶](#index-_002dl-4)

‘`--lines`’
:   Print only the newline character counts.
    If a file ends in a non-newline character,
    its trailing partial line is not counted.

‘`-L`’ [¶](#index-_002dL-1)

‘`--max-line-length`’
:   Print only the maximum display widths.
    Tabs are set at every 8th column.
    Display widths of wide characters are considered.
    Non-printable characters are given 0 width.

‘`--total=when`’ [¶](#index-_002d_002dtotal_003dwhen)
:   Control when and how the final line with cumulative counts is printed.
    when is one of:

    * auto
      - This is the default mode of `wc` when no `--total`
      option is specified. Output a total line if more than one file
      is specified.
    * always
      - Always output a total line, irrespective of the number of files processed.
    * only
      - Only output total counts. I.e., don’t print individual file counts,
      suppress any leading spaces, and don’t print the ‘`total`’ word itself,
      to simplify subsequent processing.
    * never
      - Never output a total line.

‘`--files0-from=file`’ [¶](#index-_002d_002dfiles0_002dfrom_003dfile)
:   Disallow processing files named on the command line, and instead process
    those named in file file; each name being terminated by a zero byte
    (ASCII NUL).
    This is useful
    when the list of file names is so long that it may exceed a command line
    length limitation.
    In such cases, running `wc` via `xargs` is undesirable
    because it splits the list into pieces and makes `wc` print
    a total for each sublist rather than for the entire list.
    One way to produce a list of ASCII NUL terminated file
    names is with GNU
    `find`, using its `-print0` predicate.
    If file is ‘`-`’ then the ASCII NUL terminated
    file names are read from standard input.

    For example, to find the length of the longest line in any `.c` or
    `.h` file in the current hierarchy, do this:

    ```
    find . -name '*.[ch]' -print0 |
      wc -L --files0-from=- | tail -n1
    ```

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`cksum`: Print and verify file checksums](#cksum-invocation), Previous: [`wc`: Print newline, word, and byte counts](#wc-invocation), Up: [Summarizing files](#Summarizing-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 6.2 `sum`: Print checksum and block counts [¶](#sum_003a-Print-checksum-and-block-counts)

`sum` computes a 16-bit checksum for each given file, or
standard input if none are given or for a file of ‘`-`’. Synopsis:

```
sum [option]... [file]...
```

`sum` prints the checksum for each file followed by the
number of blocks in the file (rounded up). If at least one file
is given, file names are also printed.

By default, GNU `sum` computes checksums using an algorithm
compatible with BSD `sum` and prints file sizes in units of
1024-byte blocks.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-r`’ [¶](#index-_002dr-2)
:   Use the default (BSD compatible) algorithm. This option is included for
    compatibility with the System V `sum`. Unless `-s` was also
    given, it has no effect.

‘`-s`’ [¶](#index-_002ds-9)

‘`--sysv`’
:   Compute checksums using an algorithm compatible with System V
    `sum`’s default, and print file sizes in units of 512-byte blocks.

`sum` is provided for compatibility; the `cksum` program (see
next section) is preferable in new applications.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`md5sum`: Print or check MD5 digests](#md5sum-invocation), Previous: [`sum`: Print checksum and block counts](#sum-invocation), Up: [Summarizing files](#Summarizing-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 6.3 `cksum`: Print and verify file checksums [¶](#cksum_003a-Print-and-verify-file-checksums)

`cksum` by default computes a 32-bit cyclic redundancy check (CRC)
checksum for each given file, or standard input if none are given or for
a file of ‘`-`’.

cksum also supports the `-a/--algorithm` option to select the
digest algorithm to use. `cksum` is the preferred interface
to these digests, subsuming the other standalone checksumming utilities,
which can be emulated using `cksum -a md5 --untagged "$@"` etc.
Synopsis:

```
cksum [option]... [file]...
```

`cksum` is typically used to ensure that files have not been corrupted,
by comparing the `cksum` output for the received files with the
`cksum` output for the original files (typically given in the
distribution).

* [cksum output modes](#cksum-output-modes)
* [cksum general options](#cksum-general-options)
* [cksum common options](#cksum-common-options)

---

Next: [cksum general options](#cksum-general-options), Up: [`cksum`: Print and verify file checksums](#cksum-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 6.3.1 cksum output modes [¶](#cksum-output-modes-1)

Legacy output format
:   `cksum` by default prints the POSIX standard CRC checksum
    for each file along with the number of bytes in the file,
    and the file name unless no arguments were given.
    The 32-bit CRC used is based on the polynomial used
    for CRC error checking in the ISO/IEC 8802-3:1996 standard (Ethernet).
    Similar output formats are used for the other legacy checksums
    selectable with `--algorithm=crc32b`, and
    `--algorithm=sysv` or `--algorithm=bsd`
    detailed at [`sum`: Print checksum and block counts](#sum-invocation).

Tagged output format
:   With the `--algorithm` option selecting non legacy checksums,
    the `cksum` command defaults to output of the form:

    ```
    digest_name (file name) = digest
    ```

    The standalone checksum utilities can select this output
    mode by using the `--tag` option.

Untagged output format
:   With the `--untagged` option and the `--algorithm` option
    selecting non legacy checksums, the following output format is used.
    This is the default output format of the standalone checksum utilities.
    For each file, we print the checksum, a space, a flag indicating
    binary or text input mode, and the file name.
    Binary mode is indicated with ‘`*`’, text mode with ‘’ (space).
    Binary mode is the default on systems where it’s significant,
    otherwise text mode is the default.

Without `--zero`, and with non legacy output formats,
if file contains a backslash, newline, or carriage return,
the line is started with a backslash, and each problematic character
in the file name is escaped with a backslash, making the output unambiguous
even in the presence of arbitrary file names.
Since the backslash character itself is escaped, any other backslash
escape sequences are reserved for future use.

---

Next: [cksum common options](#cksum-common-options), Previous: [cksum output modes](#cksum-output-modes), Up: [`cksum`: Print and verify file checksums](#cksum-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 6.3.2 cksum general options [¶](#cksum-general-options-1)

‘`-a`’ [¶](#index-_002da-3)

‘`--algorithm`’
:   Compute checksums using the specified digest algorithm.

    Supported legacy checksums (which are not supported by `--check`):

    ```
    ‘sysv’      equivalent to sum -s
    ‘bsd’       equivalent to sum -r
    ‘crc’       equivalent to cksum (the default)
    ‘crc32b’    only available through cksum
    ```

    Supported more modern digest algorithms are:

    ```
    ‘md5’       equivalent to md5sum
    ‘sha1’      equivalent to sha1sum
    ‘sha224’    equivalent to sha224sum
    ‘sha256’    equivalent to sha256sum
    ‘sha384’    equivalent to sha384sum
    ‘sha512’    equivalent to sha512sum
    ‘blake2b’   equivalent to b2sum
    ‘sm3’       only available through cksum
    ```

‘`--base64`’ [¶](#index-_002d_002dbase64-1)
:   Print base64-encoded digests not hexadecimal.
    This option is ignored with `--check`.
    The format conforms to
    [RFC 4648#4](https://datatracker.ietf.org/doc/html/rfc4648#section-4).

    Each base64-encoded digest has zero, one or two trailing padding
    (‘`=`’) bytes. The length of that padding is the checksum-bit-length
    modulo 3, and the `--check` parser requires precisely the same
    input digest string as what is output. I.e., removing or adding any
    ‘`=`’ padding renders a digest non-matching.

‘`--debug`’ [¶](#index-_002d_002ddebug)
:   Output extra information to stderr, like the checksum implementation being used.

‘`-l`’ [¶](#index-_002dl-5)

‘`--length`’
:   Change (shorten) the default digest length.
    This is specified in bits and thus must be a multiple of 8.
    This option is ignored when `--check` is specified,
    as the length is automatically determined when checking.

‘`--raw`’ [¶](#index-_002d_002draw)
:   Print only the unencoded raw binary digest for a single input.
    Do not output the file name or anything else.
    Use network byte order (big endian) where applicable:
    for ‘`bsd`’, ‘`crc`’, ‘`crc32b`’, and ‘`sysv`’.
    This option works only with a single input.
    Unlike other output formats, `cksum` provides no way to
    `--check` a `--raw` checksum.

‘`--untagged`’ [¶](#index-_002d_002duntagged)
:   Output using the original Coreutils format used by the other
    standalone checksum utilities like `md5sum` for example.
    This format has the checksum at the start of the line, and may be
    more amenable to further processing by other utilities,
    especially in combination with the `--zero` option.
    This does not identify the digest algorithm used for the checksum.
    See [cksum output modes](#cksum-output-modes) for details of this format.

---

Previous: [cksum general options](#cksum-general-options), Up: [`cksum`: Print and verify file checksums](#cksum-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 6.3.3 cksum common options [¶](#cksum-common-options-1)

‘`-b`’ [¶](#index-_002db-8)

‘`--binary`’
:   This option is not supported by the `cksum` command,
    as it operates in binary mode exclusively.
    Treat each input file as binary, by reading it in binary mode and
    outputting a ‘`*`’ flag. This is the inverse of `--text`.
    On systems like GNU that do not distinguish between binary
    and text files, this option merely flags each input mode as binary:
    the checksum is unaffected. This option is the default on systems
    like MS-DOS that distinguish between binary and text files, except
    for reading standard input when standard input is a terminal.

‘`-c`’

‘`--check`’
:   Read file names and checksum information (not data) from each
    file (or from standard input if no file was specified) and report
    whether the checksums match the contents of the named files.
    The input to this mode is usually the output of
    a prior, checksum-generating run of the command.

    Three input formats are supported. Either the default output
    format described above, the `--tag` output format,
    or the BSD reversed mode format which is similar to the default mode,
    but doesn’t use a character to distinguish binary and text modes.

    For the `cksum` command, the `--check` option
    supports auto-detecting the digest algorithm to use,
    when presented with checksum information in the `--tag` output format.

    Also for the `cksum` command, the `--check` option
    auto-detects the digest encoding, accepting both standard hexadecimal
    checksums and those generated via `cksum` with its
    `--base64` option.

    Output with `--zero` enabled is not supported by `--check`.

      

    For each such line, `cksum` reads the named file and computes its
    checksum. Then, if the computed message digest does not match the
    one on the line with the file name, the file is noted as having
    failed the test. Otherwise, the file passes the test.
    By default, for each valid line, one line is written to standard
    output indicating whether the named file passed the test.
    After all checks have been performed, if there were any failures,
    a warning is issued to standard error.
    Use the `--status` option to inhibit that output.
    If any listed file cannot be opened or read, if any valid line has
    a checksum inconsistent with the associated file, or if no valid
    line is found, `cksum` exits with nonzero status. Otherwise,
    it exits successfully.
    The `cksum` command does not support `--check`
    with the older ‘`sysv`’, ‘`bsd`’, ‘`crc`’ or ‘`crc32b`’ algorithms.

‘`--ignore-missing`’ [¶](#index-_002d_002dignore_002dmissing)
:   This option is useful only when verifying checksums.
    When verifying checksums, don’t fail or report any status
    for missing files. This is useful when verifying a subset
    of downloaded files given a larger list of checksums.

‘`--quiet`’ [¶](#index-_002d_002dquiet-3)
:   This option is useful only when verifying checksums.
    When verifying checksums, don’t generate an ’OK’ message per successfully
    checked file. Files that fail the verification are reported in the
    default one-line-per-file format. If there is any checksum mismatch,
    print a warning summarizing the failures to standard error.

‘`--status`’ [¶](#index-_002d_002dstatus)
:   This option is useful only when verifying checksums.
    When verifying checksums, don’t generate the default one-line-per-file
    diagnostic and don’t output the warning summarizing any failures.
    Failures to open or read a file still evoke individual diagnostics to
    standard error.
    If all listed files are readable and are consistent with the associated
    checksums, exit successfully. Otherwise exit with a status code
    indicating there was a failure.

‘`--tag`’ [¶](#index-_002d_002dtag)
:   Output BSD style checksums, which indicate the checksum algorithm used.
    As a GNU extension, if `--zero` is not used, file names with problematic
    characters are escaped as described above, using the same escaping indicator of
    ‘`\`’ at the start of the line, as used with the other output format.
    The `--tag` option implies binary mode, and is disallowed with
    `--text` mode as supporting that would unnecessarily complicate
    the output format, while providing little benefit.
    See [cksum output modes](#cksum-output-modes) for details of this format.
    The `cksum` command, uses `--tag` as its default output format.

‘`-t`’ [¶](#index-_002dt-5)

‘`--text`’
:   This option is not supported by the `cksum` command.
    Treat each input file as text, by reading it in text mode and
    outputting a ‘’ flag. This is the inverse of `--binary`.
    This option is the default on systems like GNU that do not
    distinguish between binary and text files. On other systems, it is
    the default for reading standard input when standard input is a
    terminal. This mode is never defaulted to if `--tag` is used.

‘`-w`’ [¶](#index-_002dw-7)

‘`--warn`’
:   When verifying checksums, warn about improperly formatted checksum lines.
    This option is useful only if all but a few lines in the checked input
    are valid.

‘`--strict`’ [¶](#index-_002d_002dstrict)
:   When verifying checksums,
    if one or more input line is invalid,
    exit nonzero after all warnings have been issued.

‘`-z`’ [¶](#index-_002dz-3)

‘`--zero`’
:   Output a zero byte (ASCII NUL) at the end of each line,
    rather than a newline. This option enables other programs to parse the
    output even when that output would contain data with embedded newlines.
    Also file name escaping is not used.

---

Next: [`b2sum`: Print or check BLAKE2 digests](#b2sum-invocation), Previous: [`cksum`: Print and verify file checksums](#cksum-invocation), Up: [Summarizing files](#Summarizing-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 6.4 `md5sum`: Print or check MD5 digests [¶](#md5sum_003a-Print-or-check-MD5-digests)

`md5sum` computes a 128-bit checksum (or *fingerprint* or
*message-digest*) for each specified file.

The MD5 digest is more reliable than a simple CRC (provided by
the `cksum` command) for detecting accidental file corruption,
as the chances of accidentally having two files with identical MD5
are vanishingly small. However, it should not be considered secure
against malicious tampering: although finding a file with a given MD5
fingerprint is considered infeasible at the moment, it is known how
to modify certain files, including digital certificates, so that they
appear valid when signed with an MD5 digest. For more secure hashes,
consider using SHA-2 or `b2sum`.
See [sha2 utilities: Print or check SHA-2 digests](#sha2-utilities). See [`b2sum`: Print or check BLAKE2 digests](#b2sum-invocation).

If a file is specified as ‘`-`’ or if no files are given
`md5sum` computes the checksum for the standard input.
`md5sum` can also determine whether a file and checksum are
consistent. Synopsis:

```
md5sum [option]... [file]...
```

`md5sum` uses the ‘`Untagged output format`’
for each specified file, as described at [cksum output modes](#cksum-output-modes).

The program accepts [cksum common options](#cksum-common-options). Also see [Common options](#Common-options).

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`sha1sum`: Print or check SHA-1 digests](#sha1sum-invocation), Previous: [`md5sum`: Print or check MD5 digests](#md5sum-invocation), Up: [Summarizing files](#Summarizing-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 6.5 `b2sum`: Print or check BLAKE2 digests [¶](#b2sum_003a-Print-or-check-BLAKE2-digests)

`b2sum` computes a 512-bit checksum for each specified
file.

If a file is specified as ‘`-`’ or if no files are given
`b2sum` computes the checksum for the standard input.
`b2sum` can also determine whether a file and checksum are
consistent. Synopsis:

```
b2sum [option]... [file]...
```

`b2sum` uses the ‘`Untagged output format`’
for each specified file, as described at [cksum output modes](#cksum-output-modes).

The program accepts [cksum common options](#cksum-common-options). Also see [Common options](#Common-options).

In addition `b2sum` supports the following options.

‘`-l`’ [¶](#index-_002dl-6)

‘`--length`’
:   Change (shorten) the default digest length.
    This is specified in bits and thus must be a multiple of 8.
    This option is ignored when `--check` is specified,
    as the length is automatically determined when checking.

---

Next: [sha2 utilities: Print or check SHA-2 digests](#sha2-utilities), Previous: [`b2sum`: Print or check BLAKE2 digests](#b2sum-invocation), Up: [Summarizing files](#Summarizing-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 6.6 `sha1sum`: Print or check SHA-1 digests [¶](#sha1sum_003a-Print-or-check-SHA_002d1-digests)

`sha1sum` computes a 160-bit checksum for each specified file.

The SHA-1 digest is more reliable than a simple CRC (provided by
the `cksum` command) for detecting accidental file corruption,
as the chances of accidentally having two files with identical SHA-1
are vanishingly small. However, it should not be considered secure
against malicious tampering: although finding a file with a given SHA-1
fingerprint is considered infeasible at the moment, it is known how
to modify certain files, including digital certificates, so that they
appear valid when signed with an SHA-1 digest. For more secure hashes,
consider using SHA-2 or `b2sum`.
See [sha2 utilities: Print or check SHA-2 digests](#sha2-utilities). See [`b2sum`: Print or check BLAKE2 digests](#b2sum-invocation).

If a file is specified as ‘`-`’ or if no files are given
`sha1sum` computes the checksum for the standard input.
`sha1sum` can also determine whether a file and checksum are
consistent. Synopsis:

```
sha1sum [option]... [file]...
```

`sha1sum` uses the ‘`Untagged output format`’
for each specified file, as described at [cksum output modes](#cksum-output-modes).

The program accepts [cksum common options](#cksum-common-options). Also see [Common options](#Common-options).

---

Previous: [`sha1sum`: Print or check SHA-1 digests](#sha1sum-invocation), Up: [Summarizing files](#Summarizing-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 6.7 sha2 utilities: Print or check SHA-2 digests [¶](#sha2-utilities_003a-Print-or-check-SHA_002d2-digests)

The commands `sha224sum`, `sha256sum`,
`sha384sum` and `sha512sum` compute checksums of
various lengths (respectively 224, 256, 384 and 512 bits),
collectively known as the SHA-2 hashes.

If a file is specified as ‘`-`’ or if no files are given
`sha???sum` computes the checksum for the standard input.
`sha???sum` can also determine whether a file and checksum are
consistent. Synopsis:

```
sha???sum [option]... [file]...
```

`sha???sum` uses the ‘`Untagged output format`’
for each specified file, as described at [cksum output modes](#cksum-output-modes).

The program accepts [cksum common options](#cksum-common-options). Also see [Common options](#Common-options).

---

Next: [Operating on fields](#Operating-on-fields), Previous: [Summarizing files](#Summarizing-files), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 7 Operating on sorted files [¶](#Operating-on-sorted-files-1)

These commands work with (or produce) sorted files.

* [`sort`: Sort text files](#sort-invocation)
* [`shuf`: Shuffling text](#shuf-invocation)
* [`uniq`: Uniquify files](#uniq-invocation)
* [`comm`: Compare two sorted files line by line](#comm-invocation)
* [`ptx`: Produce permuted indexes](#ptx-invocation)
* [`tsort`: Topological sort](#tsort-invocation)

---

Next: [`shuf`: Shuffling text](#shuf-invocation), Up: [Operating on sorted files](#Operating-on-sorted-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]
