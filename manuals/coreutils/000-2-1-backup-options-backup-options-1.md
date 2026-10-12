Next: [Introduction](#Introduction), Up: [(dir)](/manual)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]# GNU Coreutils [¶](#GNU-Coreutils)

This manual documents version 9.7 of the GNU core
utilities, including the standard programs for text and file manipulation.

Copyright © 1994–2025 Free Software Foundation, Inc.

> Permission is granted to copy, distribute and/or modify this document
> under the terms of the GNU Free Documentation License, Version 1.3 or
> any later version published by the Free Software Foundation; with no
> Invariant Sections, with no Front-Cover Texts, and with no Back-Cover
> Texts. A copy of the license is included in the section entitled “GNU
> Free Documentation License”.## Short Table of Contents

* [1 Introduction](#toc-Introduction-1)
* [2 Common options](#toc-Common-options-1)
* [3 Output of entire files](#toc-Output-of-entire-files-1)
* [4 Formatting file contents](#toc-Formatting-file-contents-1)
* [5 Output of parts of files](#toc-Output-of-parts-of-files-1)
* [6 Summarizing files](#toc-Summarizing-files-1)
* [7 Operating on sorted files](#toc-Operating-on-sorted-files-1)
* [8 Operating on fields](#toc-Operating-on-fields-1)
* [9 Operating on characters](#toc-Operating-on-characters-1)
* [10 Directory listing](#toc-Directory-listing-1)
* [11 Basic operations](#toc-Basic-operations-1)
* [12 Special file types](#toc-Special-file-types-1)
* [13 Changing file attributes](#toc-Changing-file-attributes-1)
* [14 File space usage](#toc-File-space-usage-1)
* [15 Printing text](#toc-Printing-text-1)
* [16 Conditions](#toc-Conditions-1)
* [17 Redirection](#toc-Redirection-1)
* [18 File name manipulation](#toc-File-name-manipulation-1)
* [19 Working context](#toc-Working-context-1)
* [20 User information](#toc-User-information-1)
* [21 System context](#toc-System-context-1)
* [22 SELinux context](#toc-SELinux-context-1)
* [23 Modified command invocation](#toc-Modified-command-invocation-1)
* [24 Process control](#toc-Process-control-1)
* [25 Delaying](#toc-Delaying-1)
* [26 Numeric operations](#toc-Numeric-operations-1)
* [27 File permissions](#toc-File-permissions-1)
* [28 File timestamps](#toc-File-timestamps-1)
* [29 Date input formats](#toc-Date-input-formats-1)
* [30 Version sort ordering](#toc-Version-sort-ordering-1)
* [31 Opening the Software Toolbox](#toc-Opening-the-Software-Toolbox)
* [Appendix A GNU Free Documentation License](#toc-GNU-Free-Documentation-License-1)
* [Index](#toc-Index)## Table of Contents

* [1 Introduction](#Introduction)
* [2 Common options](#Common-options)
  + [2.1 Backup options](#Backup-options)
  + [2.2 Block size](#Block-size)
  + [2.3 Signal specifications](#Signal-specifications)
  + [2.4 chown, chgrp, chroot, id: Disambiguating user names and IDs](#Disambiguating-names-and-IDs)
  + [2.5 Sources of random data](#Random-sources)
  + [2.6 Target directory](#Target-directory)
  + [2.7 Trailing slashes](#Trailing-slashes)
  + [2.8 Traversing symlinks](#Traversing-symlinks)
  + [2.9 Treating `/` specially](#Treating-_002f-specially)
  + [2.10 Special built-in utilities](#Special-built_002din-utilities)
  + [2.11 Exit status](#Exit-status)
  + [2.12 Floating point numbers](#Floating-point)
  + [2.13 Standards conformance](#Standards-conformance)
  + [2.14 `coreutils`: Multi-call program](#Multi_002dcall-invocation)
* [3 Output of entire files](#Output-of-entire-files)
  + [3.1 `cat`: Concatenate and write files](#cat-invocation)
  + [3.2 `tac`: Concatenate and write files in reverse](#tac-invocation)
  + [3.3 `nl`: Number lines and write files](#nl-invocation)
  + [3.4 `od`: Write files in octal or other formats](#od-invocation)
  + [3.5 `base32`: Transform data into printable data](#base32-invocation)
  + [3.6 `base64`: Transform data into printable data](#base64-invocation)
  + [3.7 `basenc`: Transform data into printable data](#basenc-invocation)
* [4 Formatting file contents](#Formatting-file-contents)
  + [4.1 `fmt`: Reformat paragraph text](#fmt-invocation)
  + [4.2 `pr`: Paginate or columnate files for printing](#pr-invocation)
  + [4.3 `fold`: Wrap input lines to fit in specified width](#fold-invocation)
* [5 Output of parts of files](#Output-of-parts-of-files)
  + [5.1 `head`: Output the first part of files](#head-invocation)
  + [5.2 `tail`: Output the last part of files](#tail-invocation)
  + [5.3 `split`: Split a file into pieces.](#split-invocation)
  + [5.4 `csplit`: Split a file into context-determined pieces](#csplit-invocation)
* [6 Summarizing files](#Summarizing-files)
  + [6.1 `wc`: Print newline, word, and byte counts](#wc-invocation)
  + [6.2 `sum`: Print checksum and block counts](#sum-invocation)
  + [6.3 `cksum`: Print and verify file checksums](#cksum-invocation)
    - [6.3.1 cksum output modes](#cksum-output-modes)
    - [6.3.2 cksum general options](#cksum-general-options)
    - [6.3.3 cksum common options](#cksum-common-options)
  + [6.4 `md5sum`: Print or check MD5 digests](#md5sum-invocation)
  + [6.5 `b2sum`: Print or check BLAKE2 digests](#b2sum-invocation)
  + [6.6 `sha1sum`: Print or check SHA-1 digests](#sha1sum-invocation)
  + [6.7 sha2 utilities: Print or check SHA-2 digests](#sha2-utilities)
* [7 Operating on sorted files](#Operating-on-sorted-files)
  + [7.1 `sort`: Sort text files](#sort-invocation)
  + [7.2 `shuf`: Shuffling text](#shuf-invocation)
  + [7.3 `uniq`: Uniquify files](#uniq-invocation)
  + [7.4 `comm`: Compare two sorted files line by line](#comm-invocation)
  + [7.5 `ptx`: Produce permuted indexes](#ptx-invocation)
    - [7.5.1 General options](#General-options-in-ptx)
    - [7.5.2 Charset selection](#Charset-selection-in-ptx)
    - [7.5.3 Word selection and input processing](#Input-processing-in-ptx)
    - [7.5.4 Output formatting](#Output-formatting-in-ptx)
    - [7.5.5 The GNU extensions to `ptx`](#Compatibility-in-ptx)
  + [7.6 `tsort`: Topological sort](#tsort-invocation)
    - [7.6.1 `tsort`: Background](#tsort-background)
* [8 Operating on fields](#Operating-on-fields)
  + [8.1 `cut`: Print selected parts of lines](#cut-invocation)
  + [8.2 `paste`: Merge lines of files](#paste-invocation)
  + [8.3 `join`: Join lines on a common field](#join-invocation)
    - [8.3.1 General options](#General-options-in-join)
    - [8.3.2 Pre-sorting](#Sorting-files-for-join)
    - [8.3.3 Working with fields](#Working-with-fields)
    - [8.3.4 Controlling `join`’s field matching](#Paired-and-unpaired-lines)
    - [8.3.5 Header lines](#Header-lines)
    - [8.3.6 Union, Intersection and Difference of files](#Set-operations)
* [9 Operating on characters](#Operating-on-characters)
  + [9.1 `tr`: Translate, squeeze, and/or delete characters](#tr-invocation)
    - [9.1.1 Specifying arrays of characters](#Character-arrays)
    - [9.1.2 Translating](#Translating)
    - [9.1.3 Squeezing repeats and deleting](#Squeezing-and-deleting)
  + [9.2 `expand`: Convert tabs to spaces](#expand-invocation)
  + [9.3 `unexpand`: Convert spaces to tabs](#unexpand-invocation)
* [10 Directory listing](#Directory-listing)
  + [10.1 `ls`: List directory contents](#ls-invocation)
    - [10.1.1 Which files are listed](#Which-files-are-listed)
    - [10.1.2 What information is listed](#What-information-is-listed)
    - [10.1.3 Sorting the output](#Sorting-the-output)
    - [10.1.4 General output formatting](#General-output-formatting)
    - [10.1.5 Formatting file timestamps](#Formatting-file-timestamps)
    - [10.1.6 Formatting the file names](#Formatting-the-file-names)
  + [10.2 `dir`: Briefly list directory contents](#dir-invocation)
  + [10.3 `vdir`: Verbosely list directory contents](#vdir-invocation)
  + [10.4 `dircolors`: Color setup for `ls`](#dircolors-invocation)
* [11 Basic operations](#Basic-operations)
  + [11.1 `cp`: Copy files and directories](#cp-invocation)
  + [11.2 `dd`: Convert and copy a file](#dd-invocation)
  + [11.3 `install`: Copy files and set attributes](#install-invocation)
  + [11.4 `mv`: Move (rename) files](#mv-invocation)
  + [11.5 `rm`: Remove files or directories](#rm-invocation)
  + [11.6 `shred`: Remove files more securely](#shred-invocation)
* [12 Special file types](#Special-file-types)
  + [12.1 `link`: Make a hard link via the link syscall](#link-invocation)
  + [12.2 `ln`: Make links between files](#ln-invocation)
  + [12.3 `mkdir`: Make directories](#mkdir-invocation)
  + [12.4 `mkfifo`: Make FIFOs (named pipes)](#mkfifo-invocation)
  + [12.5 `mknod`: Make block or character special files](#mknod-invocation)
  + [12.6 `readlink`: Print value of a symlink or canonical file name](#readlink-invocation)
  + [12.7 `rmdir`: Remove empty directories](#rmdir-invocation)
  + [12.8 `unlink`: Remove files via the unlink syscall](#unlink-invocation)
* [13 Changing file attributes](#Changing-file-attributes)
  + [13.1 `chown`: Change file owner and group](#chown-invocation)
  + [13.2 `chgrp`: Change group ownership](#chgrp-invocation)
  + [13.3 `chmod`: Change access permissions](#chmod-invocation)
  + [13.4 `touch`: Change file timestamps](#touch-invocation)
* [14 File space usage](#File-space-usage)
  + [14.1 `df`: Report file system space usage](#df-invocation)
  + [14.2 `du`: Estimate file space usage](#du-invocation)
  + [14.3 `stat`: Report file or file system status](#stat-invocation)
  + [14.4 `sync`: Synchronize cached writes to persistent storage](#sync-invocation)
  + [14.5 `truncate`: Shrink or extend the size of a file](#truncate-invocation)
* [15 Printing text](#Printing-text)
  + [15.1 `echo`: Print a line of text](#echo-invocation)
  + [15.2 `printf`: Format and print data](#printf-invocation)
  + [15.3 `yes`: Print a string until interrupted](#yes-invocation)
* [16 Conditions](#Conditions)
  + [16.1 `false`: Do nothing, unsuccessfully](#false-invocation)
  + [16.2 `true`: Do nothing, successfully](#true-invocation)
  + [16.3 `test`: Check file types and compare values](#test-invocation)
    - [16.3.1 File type tests](#File-type-tests)
    - [16.3.2 Access permission tests](#Access-permission-tests)
    - [16.3.3 File characteristic tests](#File-characteristic-tests)
    - [16.3.4 String tests](#String-tests)
    - [16.3.5 Numeric tests](#Numeric-tests)
    - [16.3.6 Connectives for `test`](#Connectives-for-test)
  + [16.4 `expr`: Evaluate expressions](#expr-invocation)
    - [16.4.1 String expressions](#String-expressions)
    - [16.4.2 Numeric expressions](#Numeric-expressions)
    - [16.4.3 Relations for `expr`](#Relations-for-expr)
    - [16.4.4 Examples of using `expr`](#Examples-of-expr)
* [17 Redirection](#Redirection)
  + [17.1 `tee`: Redirect output to multiple files or processes](#tee-invocation)
* [18 File name manipulation](#File-name-manipulation)
  + [18.1 `basename`: Strip directory and suffix from a file name](#basename-invocation)
  + [18.2 `dirname`: Strip last file name component](#dirname-invocation)
  + [18.3 `pathchk`: Check file name validity and portability](#pathchk-invocation)
  + [18.4 `mktemp`: Create temporary file or directory](#mktemp-invocation)
  + [18.5 `realpath`: Print the resolved file name.](#realpath-invocation)
    - [18.5.1 Realpath usage examples](#Realpath-usage-examples)
* [19 Working context](#Working-context)
  + [19.1 `pwd`: Print working directory](#pwd-invocation)
  + [19.2 `stty`: Print or change terminal characteristics](#stty-invocation)
    - [19.2.1 Control settings](#Control)
    - [19.2.2 Input settings](#Input)
    - [19.2.3 Output settings](#Output)
    - [19.2.4 Local settings](#Local)
    - [19.2.5 Combination settings](#Combination)
    - [19.2.6 Special characters](#Characters)
    - [19.2.7 Special settings](#Special)
  + [19.3 `printenv`: Print all or some environment variables](#printenv-invocation)
  + [19.4 `tty`: Print file name of terminal on standard input](#tty-invocation)
* [20 User information](#User-information)
  + [20.1 `id`: Print user identity](#id-invocation)
  + [20.2 `logname`: Print current login name](#logname-invocation)
  + [20.3 `whoami`: Print effective user name](#whoami-invocation)
  + [20.4 `groups`: Print group names a user is in](#groups-invocation)
  + [20.5 `users`: Print login names of users currently logged in](#users-invocation)
  + [20.6 `who`: Print who is currently logged in](#who-invocation)
  + [20.7 `pinky`: Print information about users](#pinky-invocation)
* [21 System context](#System-context)
  + [21.1 `date`: Print or set system date and time](#date-invocation)
    - [21.1.1 Specifying the format of `date` output](#Date-format-specifiers)
      * [21.1.1.1 Time conversion specifiers](#Time-conversion-specifiers)
      * [21.1.1.2 Date conversion specifiers](#Date-conversion-specifiers)
      * [21.1.1.3 Literal conversion specifiers](#Literal-conversion-specifiers)
      * [21.1.1.4 Padding and other flags](#Padding-and-other-flags)
    - [21.1.2 Setting the time](#Setting-the-time)
    - [21.1.3 Options for `date`](#Options-for-date)
    - [21.1.4 Examples of `date`](#Examples-of-date)
  + [21.2 `arch`: Print machine hardware name](#arch-invocation)
  + [21.3 `nproc`: Print the number of available processors](#nproc-invocation)
  + [21.4 `uname`: Print system information](#uname-invocation)
  + [21.5 `hostname`: Print or set system name](#hostname-invocation)
  + [21.6 `hostid`: Print numeric host identifier](#hostid-invocation)
  + [21.7 `uptime`: Print system uptime and load](#uptime-invocation)
* [22 SELinux context](#SELinux-context)
  + [22.1 `chcon`: Change SELinux context of file](#chcon-invocation)
  + [22.2 `runcon`: Run a command in specified SELinux context](#runcon-invocation)
* [23 Modified command invocation](#Modified-command-invocation)
  + [23.1 `chroot`: Run a command with a different root directory](#chroot-invocation)
  + [23.2 `env`: Run a command in a modified environment](#env-invocation)
    - [23.2.1 General options](#General-options-2)
    - [23.2.2 `-S`/`--split-string` usage in scripts](#g_t_002dS_002f_002d_002dsplit_002dstring-usage-in-scripts)
      * [Testing and troubleshooting](#Testing-and-troubleshooting)
    - [23.2.3 `-S`/`--split-string` syntax](#g_t_002dS_002f_002d_002dsplit_002dstring-syntax)
      * [Splitting arguments by whitespace](#Splitting-arguments-by-whitespace)
      * [Escape sequences](#Escape-sequences)
      * [Comments](#Comments)
      * [Environment variable expansion](#Environment-variable-expansion)
  + [23.3 `nice`: Run a command with modified niceness](#nice-invocation)
  + [23.4 `nohup`: Run a command immune to hangups](#nohup-invocation)
  + [23.5 `stdbuf`: Run a command with modified I/O stream buffering](#stdbuf-invocation)
  + [23.6 `timeout`: Run a command with a time limit](#timeout-invocation)
* [24 Process control](#Process-control)
  + [24.1 `kill`: Send a signal to processes](#kill-invocation)
* [25 Delaying](#Delaying)
  + [25.1 `sleep`: Delay for a specified time](#sleep-invocation)
* [26 Numeric operations](#Numeric-operations)
  + [26.1 `factor`: Print prime factors](#factor-invocation)
  + [26.2 `numfmt`: Reformat numbers](#numfmt-invocation)
    - [26.2.1 General options](#General-options-3)
    - [26.2.2 Possible units:](#Possible-units_003a)
    - [26.2.3 Examples of using `numfmt`](#Examples-of-using-numfmt)
  + [26.3 `seq`: Print numeric sequences](#seq-invocation)
* [27 File permissions](#File-permissions)
  + [27.1 Structure of File Mode Bits](#Mode-Structure)
  + [27.2 Symbolic Modes](#Symbolic-Modes)
    - [27.2.1 Setting Permissions](#Setting-Permissions)
    - [27.2.2 Copying Existing Permissions](#Copying-Permissions)
    - [27.2.3 Changing Special Mode Bits](#Changing-Special-Mode-Bits)
    - [27.2.4 Conditional Executability](#Conditional-Executability)
    - [27.2.5 Making Multiple Changes](#Multiple-Changes)
    - [27.2.6 The Umask and Protection](#Umask-and-Protection)
  + [27.3 Numeric Modes](#Numeric-Modes)
  + [27.4 Operator Numeric Modes](#Operator-Numeric-Modes)
  + [27.5 Directories and the Set-User-ID and Set-Group-ID Bits](#Directory-Setuid-and-Setgid)
* [28 File timestamps](#File-timestamps)
* [29 Date input formats](#Date-input-formats)
  + [29.1 General date syntax](#General-date-syntax)
  + [29.2 Calendar date items](#Calendar-date-items)
  + [29.3 Time of day items](#Time-of-day-items)
  + [29.4 Time zone items](#Time-zone-items)
  + [29.5 Combined date and time of day items](#Combined-date-and-time-of-day-items)
  + [29.6 Day of week items](#Day-of-week-items)
  + [29.7 Relative items in date strings](#Relative-items-in-date-strings)
  + [29.8 Pure numbers in date strings](#Pure-numbers-in-date-strings)
  + [29.9 Seconds since the Epoch](#Seconds-since-the-Epoch)
  + [29.10 Specifying time zone rules](#Specifying-time-zone-rules)
  + [29.11 Authors of `parse_datetime`](#Authors-of-parse_005fdatetime)
* [30 Version sort ordering](#Version-sort-ordering)
  + [30.1 Version sort overview](#Version-sort-overview)
    - [30.1.1 Using version sort in GNU Coreutils](#Using-version-sort-in-GNU-Coreutils)
    - [30.1.2 Version sort and natural sort](#Version-sort-and-natural-sort)
    - [30.1.3 Variations in version sort order](#Variations-in-version-sort-order)
  + [30.2 Version sort implementation](#Version-sort-implementation)
    - [30.2.1 Version-sort ordering rules](#Version_002dsort-ordering-rules)
    - [30.2.2 Version sort is not the same as numeric sort](#Version-sort-is-not-the-same-as-numeric-sort)
    - [30.2.3 Version sort punctuation](#Version-sort-punctuation)
    - [30.2.4 Punctuation vs letters](#Punctuation-vs-letters)
    - [30.2.5 The tilde ‘`~`’](#The-tilde-_007e)
    - [30.2.6 Version sort ignores locale](#Version-sort-ignores-locale)
  + [30.3 Differences from Debian version sort](#Differences-from-Debian-version-sort)
    - [30.3.1 Hyphen-minus ‘`-`’ and colon ‘`:`’](#Hyphen_002dminus-and-colon)
    - [30.3.2 Special priority in GNU Coreutils version sort](#Special-priority-in-GNU-Coreutils-version-sort)
    - [30.3.3 Special handling of file extensions](#Special-handling-of-file-extensions)
    - [30.3.4 Comparing two strings using Debian’s algorithm](#Comparing-two-strings-using-Debian_0027s-algorithm)
  + [30.4 Advanced Topics](#Advanced-version-sort-topics)
    - [30.4.1 Reporting version sort bugs](#Reporting-version-sort-bugs)
    - [30.4.2 Other version/natural sort implementations](#Other-version_002fnatural-sort-implementations)
    - [30.4.3 Related source code](#Related-source-code)
* [31 Opening the Software Toolbox](#Opening-the-software-toolbox)
  + [Toolbox Introduction](#Toolbox-introduction)
  + [I/O Redirection](#I_002fO-redirection)
  + [The `who` Command](#The-who-command)
  + [The `cut` Command](#The-cut-command)
  + [The `sort` Command](#The-sort-command)
  + [The `uniq` Command](#The-uniq-command)
  + [Putting the Tools Together](#Putting-the-tools-together)
* [Appendix A GNU Free Documentation License](#GNU-Free-Documentation-License)
* [Index](#Concept-index)

---

Next: [Common options](#Common-options), Previous: [GNU Coreutils](#Top), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 1 Introduction [¶](#Introduction-1)

This manual is a work in progress: many sections make no attempt to explain
basic concepts in a way suitable for novices. Thus, if you are interested,
please get involved in improving this manual. The entire GNU community
will benefit.

The GNU utilities documented here are mostly compatible with the
POSIX standard.

Please report bugs to [bug-coreutils@gnu.org](mailto:bug-coreutils@gnu.org).
Include the version number, machine architecture, input files, and
any other information needed to reproduce the bug: your input, what you
expected, what you got, and why it is wrong.

If you have a problem with `sort` or `date`, try using the
`--debug` option, as it can often help find and fix problems without
having to wait for an answer to a bug report. If the debug output
does not suffice to fix the problem on your own, please compress and
attach it to the rest of your bug report.

Although diffs are welcome,
please include a description of the problem as well, since this is
sometimes difficult to infer. See [Bugs](https://gcc.gnu.org/onlinedocs/gcc/Bugs.html#Bugs) in Using and Porting GNU CC.

This manual was originally derived from the Unix man pages in the
distributions, which were written by David MacKenzie and updated by Jim
Meyering. What you are reading now is the authoritative documentation
for these utilities; the man pages are no longer being maintained. The
original `fmt` man page was written by Ross Paterson. François
Pinard did the initial conversion to Texinfo format. Karl Berry did the
indexing, some reorganization, and editing of the results. Brian
Youmans of the Free Software Foundation office staff combined the
manuals for textutils, fileutils, and sh-utils to produce the present
omnibus manual. Richard Stallman contributed his usual invaluable
insights to the overall process.

---

Next: [Output of entire files](#Output-of-entire-files), Previous: [Introduction](#Introduction), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 2 Common options [¶](#Common-options-1)

Certain options are available in all of these programs. Rather than
writing identical descriptions for each of the programs, they are
described here. (In fact, every GNU program accepts (or should accept)
these options.)

Normally options and operands can appear in any order, and programs act
as if all the options appear before any operands. For example,
‘`sort -r passwd -t :`’ acts like ‘`sort -r -t : passwd`’, since
‘`:`’ is an option-argument of `-t`. However, if the
`POSIXLY_CORRECT` environment variable is set, options must appear
before operands, unless otherwise specified for a particular command.

A few programs can usefully have trailing operands with leading
‘`-`’. With such a program, options must precede operands even if
`POSIXLY_CORRECT` is not set, and this fact is noted in the
program description. For example, the `env` command’s options
must appear before its operands, since in some cases the operands
specify a command that itself contains options.

Most programs that accept long options recognize unambiguous
abbreviations of those options. For example, ‘`rmdir
--ignore-fail-on-non-empty`’ can be invoked as ‘`rmdir
--ignore-fail`’ or even ‘`rmdir --i`’. Ambiguous options, such as
‘`ls --h`’, are identified as such.

Some of these programs recognize the `--help` and `--version`
options only when one of them is the sole command line argument. For
these programs, abbreviations of the long options are not always recognized.

‘`--help`’ [¶](#index-_002d_002dhelp)
:   Print a usage message listing all available options, then exit successfully.

‘`--version`’ [¶](#index-_002d_002dversion)
:   Print the version number, then exit successfully.

‘`--`’ [¶](#index-_002d_002d)
:   Delimit the option list. Later arguments, if any, are treated as
    operands even if they begin with ‘`-`’. For example, ‘`sort --
    -r`’ reads from the file named `-r`.

A single ‘`-`’ operand is not really an option, though it looks like one. It
stands for a file operand, and some tools treat it as standard input, or as
standard output if that is clear from the context. For example, ‘`sort -`’
reads from standard input, and is equivalent to plain ‘`sort`’. Unless
otherwise specified, a ‘`-`’ can appear as any operand that requires a file
name.

* [Backup options](#Backup-options)
* [Block size](#Block-size)
* [Signal specifications](#Signal-specifications)
* [chown, chgrp, chroot, id: Disambiguating user names and IDs](#Disambiguating-names-and-IDs)
* [Sources of random data](#Random-sources)
* [Target directory](#Target-directory)
* [Trailing slashes](#Trailing-slashes)
* [Traversing symlinks](#Traversing-symlinks)
* [Treating `/` specially](#Treating-_002f-specially)
* [Special built-in utilities](#Special-built_002din-utilities)
* [Exit status](#Exit-status)
* [Floating point numbers](#Floating-point)
* [Standards conformance](#Standards-conformance)
* [`coreutils`: Multi-call program](#Multi_002dcall-invocation)

---

Next: [Block size](#Block-size), Up: [Common options](#Common-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 2.1 Backup options [¶](#Backup-options-1)

Some GNU programs (at least `cp`, `install`,
`ln`, and `mv`) optionally make backups of files
before writing new versions.
These options control the details of these backups. The options are also
briefly mentioned in the descriptions of the particular programs.

‘`-b`’ [¶](#index-_002db)

‘`--backup[=method]`’
:   Make a backup of each file that would otherwise be overwritten or removed.
    Without this option, the original versions are destroyed.
    Use method to determine the type of backups to make.
    When this option is used but method is not specified,
    then the value of the `VERSION_CONTROL`
    environment variable is used. And if `VERSION_CONTROL` is not set,
    the default backup type is ‘`existing`’.

    Using `-b` is equivalent to using `--backup=existing`;
    `-b` does not accept any argument.

    This option corresponds to the Emacs variable ‘`version-control`’;
    the values for method are the same as those used in Emacs.
    This option also accepts more descriptive names.
    The valid methods are (unique abbreviations are accepted):

    ‘`none`’ [¶](#index-none-backup-method)

    ‘`off`’
    :   Never make backups.

    ‘`numbered`’ [¶](#index-numbered-backup-method)

    ‘`t`’
    :   Always make numbered backups.

    ‘`existing`’ [¶](#index-existing-backup-method)

    ‘`nil`’
    :   Make numbered backups of files that already have them, simple backups
        of the others.

    ‘`simple`’ [¶](#index-simple-backup-method)

    ‘`never`’
    :   Always make simple backups. Do not confuse ‘`never`’ with ‘`none`’.

‘`-S suffix`’ [¶](#index-_002dS)

‘`--suffix=suffix`’
:   Append suffix to each backup file made with `-b`. If this
    option is not specified, the value of the `SIMPLE_BACKUP_SUFFIX`
    environment variable is used. And if `SIMPLE_BACKUP_SUFFIX` is not
    set, the default is ‘`~`’, just as in Emacs.

---

Next: [Signal specifications](#Signal-specifications), Previous: [Backup options](#Backup-options), Up: [Common options](#Common-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 2.2 Block size [¶](#Block-size-1)

Some GNU programs (at least `df`, `du`, and
`ls`) display sizes in “blocks”. You can adjust the block size
and method of display to make sizes easier to read. The block size
used for display is independent of any file system block size.
Fractional block counts are rounded up to the nearest integer.

The default block size is chosen by examining the following environment
variables in turn; the first one that is set determines the block size.

`DF_BLOCK_SIZE`
:   This specifies the default block size for the `df` command.
    Similarly, `DU_BLOCK_SIZE` specifies the default for `du` and
    `LS_BLOCK_SIZE` for `ls`.

`BLOCK_SIZE`
:   This specifies the default block size for all three commands, if the
    above command-specific environment variables are not set.

`BLOCKSIZE`
:   This specifies the default block size for all values that are normally
    printed as blocks, if neither `BLOCK_SIZE` nor the above
    command-specific environment variables are set. Unlike the other
    environment variables, `BLOCKSIZE` does not affect values that are
    normally printed as byte counts, e.g., the file sizes contained in
    `ls -l` output.

`POSIXLY_CORRECT`
:   If neither `command_BLOCK_SIZE`, nor `BLOCK_SIZE`, nor
    `BLOCKSIZE` is set, but this variable is set, the block size
    defaults to 512.

If none of the above environment variables are set, the block size
currently defaults to 1024 bytes in most contexts, but this number may
change in the future. For `ls` file sizes, the block size
defaults to 1 byte.

A block size specification can be a positive integer specifying the number
of bytes per block, or it can be `human-readable` or `si` to
select a human-readable format. Integers may be followed by suffixes
that are upward compatible with the
[SI prefixes](http://www.bipm.org/en/publications/si-brochure/chapter3.html)
for decimal multiples and with the
[ISO/IEC 80000-13
(formerly IEC 60027-2) prefixes](https://physics.nist.gov/cuu/Units/binary.html) for binary multiples.

With human-readable formats, output sizes are followed by a size letter
such as ‘`M`’ for megabytes. `BLOCK_SIZE=human-readable` uses
powers of 1024; ‘`M`’ stands for 1,048,576 bytes.
`BLOCK_SIZE=si` is similar, but uses powers of 1000 and appends
‘`B`’; ‘`MB`’ stands for 1,000,000 bytes.

A block size specification preceded by ‘`'`’ causes output sizes to
be displayed with thousands separators. The `LC_NUMERIC` locale
specifies the thousands separator and grouping. For example, in an
American English locale, ‘`--block-size="'1kB"`’ would cause a size
of 1234000 bytes to be displayed as ‘`1,234`’. In the default C
locale, there is no thousands separator so a leading ‘`'`’ has no
effect.

An integer block size can be followed by a suffix to specify a
multiple of that size. A bare size letter,
or one followed by ‘`iB`’, specifies
a multiple using powers of 1024. A size letter followed by ‘`B`’
specifies powers of 1000 instead. For example, ‘`1M`’ and
‘`1MiB`’ are equivalent to ‘`1048576`’, whereas ‘`1MB`’ is
equivalent to ‘`1000000`’.

A plain suffix without a preceding integer acts as if ‘`1`’ were
prepended, except that it causes a size indication to be appended to
the output. For example, ‘`--block-size="kB"`’ displays 3000 as
‘`3kB`’.

The following suffixes are defined. Large sizes like `1Q`
may be rejected by your computer due to limitations of its arithmetic.

‘`kB`’ [¶](#index-kilobyte_002c-definition-of)
:   kilobyte: *10^3 = 1000*.

‘`k`’ [¶](#index-kibibyte_002c-definition-of)

‘`K`’

‘`KiB`’
:   kibibyte: *2^{10} = 1024*. ‘`K`’ is special: the SI prefix is
    ‘`k`’ and the ISO/IEC 80000-13 prefix is ‘`Ki`’, but tradition and
    POSIX use ‘`k`’ to mean ‘`KiB`’.

‘`MB`’ [¶](#index-megabyte_002c-definition-of)
:   megabyte: *10^6 = 1,000,000*.

‘`M`’ [¶](#index-mebibyte_002c-definition-of)

‘`MiB`’
:   mebibyte: *2^{20} = 1,048,576*.

‘`GB`’ [¶](#index-gigabyte_002c-definition-of)
:   gigabyte: *10^9 = 1,000,000,000*.

‘`G`’ [¶](#index-gibibyte_002c-definition-of)

‘`GiB`’
:   gibibyte: *2^{30} = 1,073,741,824*.

‘`TB`’ [¶](#index-terabyte_002c-definition-of)
:   terabyte: *10^{12} = 1,000,000,000,000*.

‘`T`’ [¶](#index-tebibyte_002c-definition-of)

‘`TiB`’
:   tebibyte: *2^{40} = 1,099,511,627,776*.

‘`PB`’ [¶](#index-petabyte_002c-definition-of)
:   petabyte: *10^{15} = 1,000,000,000,000,000*.

‘`P`’ [¶](#index-pebibyte_002c-definition-of)

‘`PiB`’
:   pebibyte: *2^{50} = 1,125,899,906,842,624*.

‘`EB`’ [¶](#index-exabyte_002c-definition-of)
:   exabyte: *10^{18} = 1,000,000,000,000,000,000*.

‘`E`’ [¶](#index-exbibyte_002c-definition-of)

‘`EiB`’
:   exbibyte: *2^{60} = 1,152,921,504,606,846,976*.

‘`ZB`’ [¶](#index-zettabyte_002c-definition-of)
:   zettabyte: *10^{21} = 1,000,000,000,000,000,000,000*

‘`Z`’

‘`ZiB`’
:   zebibyte: *2^{70} = 1,180,591,620,717,411,303,424*.

‘`YB`’ [¶](#index-yottabyte_002c-definition-of)
:   yottabyte: *10^{24} = 1,000,000,000,000,000,000,000,000*.

‘`Y`’

‘`YiB`’
:   yobibyte: *2^{80} = 1,208,925,819,614,629,174,706,176*.

‘`RB`’ [¶](#index-ronnabyte_002c-definition-of)
:   ronnabyte: *10^{27} = 1,000,000,000,000,000,000,000,000,000*.

‘`R`’

‘`RiB`’
:   robibyte: *2^{90} = 1,237,940,039,285,380,274,899,124,224*.

‘`QB`’ [¶](#index-quettabyte_002c-definition-of)
:   quettabyte: *10^{30} = 1,000,000,000,000,000,000,000,000,000,000*.

‘`Q`’

‘`QiB`’
:   quebibyte: *2^{100} = 1,267,650,600,228,229,401,496,703,205,376*.

Block size defaults can be overridden by an explicit
`--block-size=size` option. The `-k`
option is equivalent to `--block-size=1K`, which
is the default unless the `POSIXLY_CORRECT` environment variable is
set. The `-h` or `--human-readable` option is equivalent to
`--block-size=human-readable`. The `--si` option is
equivalent to `--block-size=si`. Note for `ls`
the `-k` option does not control the display of the
apparent file sizes, whereas the `--block-size` option does.

---

Next: [chown, chgrp, chroot, id: Disambiguating user names and IDs](#Disambiguating-names-and-IDs), Previous: [Block size](#Block-size), Up: [Common options](#Common-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 2.3 Signal specifications [¶](#Signal-specifications-1)

A signal may be a signal name like ‘`HUP`’, or a signal
number like ‘`1`’, or an exit status of a process terminated by the
signal. A signal name can be given in canonical form or prefixed by
‘`SIG`’. The case of the letters is ignored.

The signal ‘`0`’ pseudo signal is synonymous with the name ‘`EXIT`’
with the GNU `kill` command, and `bash` at least,
as `trap foo 0` and `trap foo EXIT` are equivalent.

The following signal names and numbers are supported
on all POSIX compliant systems:

‘`HUP`’
:   1. Hangup.

‘`INT`’
:   2. Terminal interrupt.

‘`QUIT`’
:   3. Terminal quit.

‘`ABRT`’
:   6. Process abort.

‘`KILL`’
:   9. Kill (cannot be caught or ignored).

‘`ALRM`’
:   14. Alarm Clock.

‘`TERM`’
:   15. Termination.

Other supported signal names have system-dependent corresponding
numbers. All systems conforming to POSIX 1003.1-2001 also
support the following signals:

‘`BUS`’
:   Access to an undefined portion of a memory object.

‘`CHLD`’
:   Child process terminated, stopped, or continued.

‘`CONT`’
:   Continue executing, if stopped.

‘`FPE`’
:   Erroneous arithmetic operation.

‘`ILL`’
:   Illegal Instruction.

‘`PIPE`’
:   Write on a pipe with no one to read it.

‘`SEGV`’
:   Invalid memory reference.

‘`STOP`’
:   Stop executing (cannot be caught or ignored).

‘`TSTP`’
:   Terminal stop.

‘`TTIN`’
:   Background process attempting read.

‘`TTOU`’
:   Background process attempting write.

‘`URG`’
:   High bandwidth data is available at a socket.

‘`USR1`’
:   User-defined signal 1.

‘`USR2`’
:   User-defined signal 2.

POSIX 1003.1-2001 systems that support the XSI extension
also support the following signals:

‘`POLL`’
:   Pollable event.

‘`PROF`’
:   Profiling timer expired.

‘`SYS`’
:   Bad system call.

‘`TRAP`’
:   Trace/breakpoint trap.

‘`VTALRM`’
:   Virtual timer expired.

‘`XCPU`’
:   CPU time limit exceeded.

‘`XFSZ`’
:   File size limit exceeded.

POSIX 1003.1-2001 systems that support the XRT extension
also support at least eight real-time signals called ‘`RTMIN`’,
‘`RTMIN+1`’, …, ‘`RTMAX-1`’, ‘`RTMAX`’.

---

Next: [Sources of random data](#Random-sources), Previous: [Signal specifications](#Signal-specifications), Up: [Common options](#Common-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 2.4 chown, chgrp, chroot, id: Disambiguating user names and IDs [¶](#chown_002c-chgrp_002c-chroot_002c-id_003a-Disambiguating-user-names-and-IDs)

Since the user and group arguments to these commands
may be specified as names or numeric IDs, there is an
apparent ambiguity.
What if a user or group *name* is a string of digits?
Should the command interpret it as a user name or as an ID?
(Using a number as a user name is common in some environments.)
POSIX requires that these commands
first attempt to resolve the specified string as a name, and
only once that fails, then try to interpret it as an ID.
This is troublesome when you want to specify a numeric ID, say 42,
and it must work even in a pathological situation where
‘`42`’ is a user name that maps to some other user ID, say 1000.
Simply invoking `chown 42 F`, will set `F`s owner ID to
1000 – not what you intended.

GNU `chown`, `chgrp`, `chroot`, and `id`
provide a way to work around this, that at the same time may result in a
significant performance improvement by eliminating a database look-up.
Simply precede each numeric user ID and/or group ID with a ‘`+`’,
in order to force its interpretation as an integer:

```
chown +42 F
chgrp +$numeric_group_id another-file
chown +0:+0 /
```

The name look-up process is skipped for each ‘`+`’-prefixed string,
because a string containing ‘`+`’ is never a valid user or group name.
This syntax is accepted on most common Unix systems, but not on Solaris 10.

---

Next: [Target directory](#Target-directory), Previous: [chown, chgrp, chroot, id: Disambiguating user names and IDs](#Disambiguating-names-and-IDs), Up: [Common options](#Common-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 2.5 Sources of random data [¶](#Sources-of-random-data)

The `shuf`, `shred`, and `sort` commands
sometimes need random data to do their work. For example, ‘`sort
-R`’ must choose a hash function at random, and it needs random data to
make this selection.

By default these commands use an internal pseudo-random generator
initialized by a small amount of entropy, but can be directed to use
an external source with the `--random-source=file` option.
An error is reported if file does not contain enough bytes.

For example, the device file `/dev/urandom` could be used as the
source of random data. Typically, this device gathers environmental
noise from device drivers and other sources into an entropy pool, and
uses the pool to generate random bits. If the pool is short of data,
the device reuses the internal pool to produce more bits, using a
cryptographically secure pseudo-random number generator. But be aware
that this device is not designed for bulk random data generation
and is relatively slow.

`/dev/urandom` suffices for most practical uses, but applications
requiring high-value or long-term protection of private data may
require an alternate data source like `/dev/random` or
`/dev/arandom`. The set of available sources depends on your
operating system.

To reproduce the results of an earlier invocation of a command, you
can save some random data into a file and then use that file as the
random source in earlier and later invocations of the command.
Rather than depending on a file, one can generate a reproducible
arbitrary amount of pseudo-random data given a seed value, using
for example:

```
get_seeded_random()
{
  seed="$1"
  openssl enc -aes-256-ctr -pass pass:"$seed" -nosalt \
    </dev/zero 2>/dev/null
}

shuf -i1-100 --random-source=<(get_seeded_random 42)
```

---

Next: [Trailing slashes](#Trailing-slashes), Previous: [Sources of random data](#Random-sources), Up: [Common options](#Common-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 2.6 Target directory [¶](#Target-directory-1)

The `cp`, `install`, `ln`, and `mv`
commands normally treat the last operand specially when it is a
directory or a symbolic link to a directory. For example, ‘`cp
source dest`’ is equivalent to ‘`cp source dest/source`’ if
`dest` is a directory. Sometimes this behavior is not exactly
what is wanted, so these commands support the following options to
allow more fine-grained control:

‘`-T`’ [¶](#index-_002d_002dno_002dtarget_002ddirectory)

‘`--no-target-directory`’
:   Do not treat the last operand specially when it is a directory or a
    symbolic link to a directory. This can help avoid race conditions in
    programs that operate in a shared area. For example, when the command
    ‘`mv /tmp/source /tmp/dest`’ succeeds, there is no guarantee that
    `/tmp/source` was renamed to `/tmp/dest`: it could have been
    renamed to `/tmp/dest/source` instead, if some other process
    created `/tmp/dest` as a directory. However, if `mv
    -T /tmp/source /tmp/dest` succeeds, there is no
    question that `/tmp/source` was renamed to `/tmp/dest`.

    In the opposite situation, where you want the last operand to be
    treated as a directory and want a diagnostic otherwise, you can use
    the `--target-directory` (`-t`) option.

‘`-t directory`’ [¶](#index-_002d_002dtarget_002ddirectory)

‘`--target-directory=directory`’
:   Use directory as the directory component of each destination
    file name.

    The interface for most programs is that after processing options and a
    finite (possibly zero) number of fixed-position arguments, the remaining
    argument list is either expected to be empty, or is a list of items
    (usually files) that will all be handled identically. The `xargs`
    program is designed to work well with this convention.

    The commands in the `mv`-family are unusual in that they take
    a variable number of arguments with a special case at the *end*
    (namely, the target directory). This makes it nontrivial to perform some
    operations, e.g., “move all files from here to ../d/”, because
    `mv * ../d/` might exhaust the argument space, and `ls | xargs ...`
    doesn’t have a clean way to specify an extra final argument for each
    invocation of the subject command. (It can be done by going through a
    shell command, but that requires more human labor and brain power than
    it should.)

    The `--target-directory` (`-t`) option allows the `cp`,
    `install`, `ln`, and `mv` programs to be used
    conveniently with `xargs`. For example, you can move the files
    from the current directory to a sibling directory, `d` like this:

    ```
    ls | xargs mv -t ../d --
    ```

    However, this doesn’t move files whose names begin with ‘`.`’.
    If you use the GNU `find` program, you can move those
    files too, with this command:

    ```
    find . -mindepth 1 -maxdepth 1 \
      | xargs mv -t ../d
    ```

    But both of the above approaches fail if there are no files in the
    current directory, or if any file has a name containing a blank or
    some other special characters.
    The following example removes those limitations and requires both
    GNU `find` and GNU `xargs`:

    ```
    find . -mindepth 1 -maxdepth 1 -print0 \
      | xargs --null --no-run-if-empty \
          mv -t ../d
    ```

The `--target-directory` (`-t`) and
`--no-target-directory` (`-T`)
options cannot be combined.

---

Next: [Traversing symlinks](#Traversing-symlinks), Previous: [Target directory](#Target-directory), Up: [Common options](#Common-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 2.7 Trailing slashes [¶](#Trailing-slashes-1)

Some GNU programs (at least `cp` and `mv`) allow you to
remove any trailing slashes from each source argument before
operating on it. The `--strip-trailing-slashes` option enables
this behavior.

This is useful when a source argument may have a trailing slash and
specify a symbolic link to a directory. This scenario is in fact rather
common because some shells can automatically append a trailing slash when
performing file name completion on such symbolic links. Without this
option, `mv`, for example, (via the system’s rename function) must
interpret a trailing slash as a request to dereference the symbolic link
and so must rename the indirectly referenced *directory* and not
the symbolic link. Although it may seem surprising that such behavior
be the default, it is required by POSIX and is consistent with
other parts of that standard.

---

Next: [Treating `/` specially](#Treating-_002f-specially), Previous: [Trailing slashes](#Trailing-slashes), Up: [Common options](#Common-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 2.8 Traversing symlinks [¶](#Traversing-symlinks-1)

The following options modify how `chmod`, `chown`,
and `chgrp` traverse a hierarchy when
the `--recursive` (`-R`) option is also specified.
If more than one of the following options is specified, only the final
one takes effect.
These options specify whether processing a symbolic link to a directory
entails operating on just the symbolic link or on all files in the
hierarchy rooted at that directory.

These options are independent of `--dereference` and
`--no-dereference` (`-h`), which control whether to modify
a symlink or its referent.

‘`-H`’ [¶](#index-_002dH)
:   If `--recursive` (`-R`) is specified and
    a command line argument is a symbolic link to a directory, traverse it.

‘`-L`’ [¶](#index-_002dL)
:   In a recursive traversal, traverse every symbolic link to a directory
    that is encountered.

‘`-P`’ [¶](#index-_002dP)
:   Do not traverse any symbolic links.

    This is the default if none of `-H`, `-L`,
    or `-P` is specified.

---

Next: [Special built-in utilities](#Special-built_002din-utilities), Previous: [Traversing symlinks](#Traversing-symlinks), Up: [Common options](#Common-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 2.9 Treating `/` specially [¶](#Treating-_002f-specially-1)

Certain commands can operate destructively on entire hierarchies.
For example, if a user with appropriate privileges mistakenly runs
‘`rm -rf / tmp/junk`’, that may remove
all files on the entire system. Since there are so few
legitimate uses for such a command,
GNU `rm` normally declines to operate on any directory
that resolves to `/`. If you really want to try to remove all
the files on your system, you can use the `--no-preserve-root`
option, but the default behavior, specified by the
`--preserve-root` option, is safer for most purposes.

The commands `chgrp`, `chmod` and `chown`
can also operate destructively on entire hierarchies, so they too
support these options. Although, unlike `rm`, they don’t
actually unlink files, these commands are arguably more dangerous
when operating recursively on `/`, since they often work much
more quickly, and hence damage more files before an alert user can
interrupt them. Tradition and POSIX require these commands
to operate recursively on `/`, so they default to
`--no-preserve-root`, but using the `--preserve-root`
option makes them safer for most purposes. For convenience you can
specify `--preserve-root` in an alias or in a shell function.

The `--preserve-root` option also ensures
that `chgrp` and `chown` do not modify `/`
even when dereferencing a symlink pointing to `/`.

---

Next: [Exit status](#Exit-status), Previous: [Treating `/` specially](#Treating-_002f-specially), Up: [Common options](#Common-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 2.10 Special built-in utilities [¶](#Special-built_002din-utilities-1)

Some programs like `nice` can invoke other programs; for
example, the command ‘`nice cat file`’ invokes the program
`cat` by executing the command ‘`cat file`’. However,
*special built-in utilities* like `exit` cannot be invoked
this way. For example, the command ‘`nice exit`’ does not have a
well-defined behavior: it may generate an error message instead of
exiting.

Here is a list of the special built-in utilities that are standardized
by POSIX 1003.1-2004.

> `. : break continue eval exec exit export readonly
> return set shift times trap unset`

For example, because ‘`.`’, ‘`:`’, and ‘`exec`’ are special,
the commands ‘`nice . foo.sh`’, ‘`nice :`’, and ‘`nice exec
pwd`’ do not work as you might expect.

Many shells extend this list. For example, Bash has several extra
special built-in utilities like `history`, and
`suspend`, and with Bash the command ‘`nice suspend`’
generates an error message instead of suspending.

---

Next: [Floating point numbers](#Floating-point), Previous: [Special built-in utilities](#Special-built_002din-utilities), Up: [Common options](#Common-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 2.11 Exit status [¶](#Exit-status-1)

Nearly every command invocation yields an integral *exit status*
that can be used to change how other commands work.
For the vast majority of commands, an exit status of zero indicates
success. Failure is indicated by a nonzero value – typically
‘`1`’, though it may differ on unusual platforms as POSIX
requires only that it be nonzero.

However, some of the programs documented here do produce
other exit status values and a few associate different
meanings with the values ‘`0`’ and ‘`1`’.
Here are the exceptions:
`chroot`, `env`, `expr`, `ls`,
`nice`, `nohup`, `numfmt`, `printenv`,
`runcon`, `sort`, `stdbuf`, `test`,
`timeout`, `tty`.

---

Next: [Standards conformance](#Standards-conformance), Previous: [Exit status](#Exit-status), Up: [Common options](#Common-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 2.12 Floating point numbers [¶](#Floating-point-numbers)

Commands that accept or produce floating point numbers employ the
floating point representation of the underlying system, and suffer
from rounding error, overflow, and similar floating-point issues.
Almost all modern systems use IEEE-754 floating point, and it is
typically portable to assume IEEE-754 behavior these days. IEEE-754
has positive and negative infinity, distinguishes positive from
negative zero, and uses special values called NaNs to represent
invalid computations such as dividing zero by itself. For more
information, please see David Goldberg’s paper
[What Every Computer Scientist Should Know About Floating-Point Arithmetic](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html).

Commands that accept floating point numbers as options, operands or
input use the standard C functions `strtod` and `strtold` to
convert from text to floating point numbers. These floating point
numbers therefore can use scientific notation like `1.0e-34` and
`-10e100`. Commands that parse floating point also understand
case-insensitive `inf`, `infinity`, and `NaN`, although
whether such values are useful depends on the command in question.
Modern C implementations also accept hexadecimal floating point
numbers such as `-0x.ep-3`, which stands for −14/16 times
*2^{-3}*, which equals −0.109375. See [Parsing of
Floats](https://www.gnu.org/software/libc/manual/html_mono/libc.html#Parsing-of-Floats) in The GNU C Library Reference Manual.

Normally the `LC_NUMERIC` locale determines the decimal-point
character. However, some commands’ descriptions specify that they
accept numbers in either the current or the C locale; for example,
they treat ‘`3.14`’ like ‘`3,14`’ if the current locale uses
comma as a decimal point.

---

Next: [`coreutils`: Multi-call program](#Multi_002dcall-invocation), Previous: [Floating point numbers](#Floating-point), Up: [Common options](#Common-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 2.13 Standards conformance [¶](#Standards-conformance-1)

In a few cases, the GNU utilities’ default behavior is
incompatible with the POSIX standard. To suppress these
incompatibilities, define the `POSIXLY_CORRECT` environment
variable. Unless you are checking for POSIX conformance, you
probably do not need to define `POSIXLY_CORRECT`.

Newer versions of POSIX are occasionally incompatible with older
versions. For example, older versions of POSIX required the
command ‘`sort +1`’ to sort based on the second and succeeding
fields in each input line, but in POSIX 1003.1-2001
the same command is required to sort the file named `+1`, and you
must instead use the command ‘`sort -k 2`’ to get the field-based
sort. To complicate things further, POSIX 1003.1-2008 allows an
implementation to have either the old or the new behavior.

The GNU utilities normally conform to the version of POSIX
that is standard for your system. To cause them to conform to a
different version of POSIX, define the `_POSIX2_VERSION`
environment variable to a value of the form yyyymm specifying
the year and month the standard was adopted. Three values are currently
supported for `_POSIX2_VERSION`: ‘`199209`’ stands for
POSIX 1003.2-1992, ‘`200112`’ stands for POSIX
1003.1-2001, and ‘`200809`’ stands for POSIX 1003.1-2008.
For example, if you have a POSIX 1003.1-2001 system but are running software
containing traditional usage like ‘`sort +1`’ or ‘`tail +10`’,
you can work around the compatibility problems by setting
‘`_POSIX2_VERSION=200809`’ in your environment.

---

Previous: [Standards conformance](#Standards-conformance), Up: [Common options](#Common-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 2.14 `coreutils`: Multi-call program [¶](#coreutils_003a-Multi_002dcall-program)

The `coreutils` command invokes an individual utility, either
implicitly selected by the last component of the name used to invoke
`coreutils`, or explicitly with the
`--coreutils-prog` option. Synopsis:

```
coreutils --coreutils-prog=PROGRAM ...
```

The `coreutils` command is not installed by default, so
portable scripts should not rely on its existence.

---

Next: [Formatting file contents](#Formatting-file-contents), Previous: [Common options](#Common-options), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 3 Output of entire files [¶](#Output-of-entire-files-1)

These commands read and write entire files, possibly transforming them
in some way.

* [`cat`: Concatenate and write files](#cat-invocation)
* [`tac`: Concatenate and write files in reverse](#tac-invocation)
* [`nl`: Number lines and write files](#nl-invocation)
* [`od`: Write files in octal or other formats](#od-invocation)
* [`base32`: Transform data into printable data](#base32-invocation)
* [`base64`: Transform data into printable data](#base64-invocation)
* [`basenc`: Transform data into printable data](#basenc-invocation)

---

Next: [`tac`: Concatenate and write files in reverse](#tac-invocation), Up: [Output of entire files](#Output-of-entire-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 3.1 `cat`: Concatenate and write files [¶](#cat_003a-Concatenate-and-write-files)

`cat` copies each file (‘`-`’ means standard input), or
standard input if none are given, to standard output. Synopsis:

```
cat [option] [file]...
```

The program accepts the following options. Also see [Common options](#Common-options).

‘`-A`’ [¶](#index-_002dA)

‘`--show-all`’
:   Equivalent to `-vET`.

‘`-b`’ [¶](#index-_002db-1)

‘`--number-nonblank`’
:   Number all nonempty output lines, starting with 1.

‘`-e`’ [¶](#index-_002de)
:   Equivalent to `-vE`.

‘`-E`’ [¶](#index-_002dE)

‘`--show-ends`’
:   Display a ‘`$`’ after the end of each line.
    The `\r\n` combination is shown as ‘`^M$`’.

‘`-n`’ [¶](#index-_002dn)

‘`--number`’
:   Number all output lines, starting with 1. This option is ignored
    if `-b` is in effect.

‘`-s`’ [¶](#index-_002ds)

‘`--squeeze-blank`’
:   Suppress repeated adjacent blank lines; output just one empty line
    instead of several.

‘`-t`’ [¶](#index-_002dt)
:   Equivalent to `-vT`.

‘`-T`’ [¶](#index-_002dT)

‘`--show-tabs`’
:   Display TAB characters as ‘`^I`’.

‘`-u`’ [¶](#index-_002du)
:   Ignored; for POSIX compatibility.

‘`-v`’ [¶](#index-_002dv)

‘`--show-nonprinting`’
:   Display control characters except for LFD and TAB using
    ‘`^`’ notation and precede characters that have the high bit set with
    ‘`M-`’.

On systems like MS-DOS that distinguish between text and binary files,
`cat` normally reads and writes in binary mode. However,
`cat` reads in text mode if one of the options
`-bensAE` is used or if `cat` is reading from standard
input and standard input is a terminal. Similarly, `cat`
writes in text mode if one of the options `-bensAE` is used or
if standard output is a terminal.

An exit status of zero indicates success,
and a nonzero value indicates failure.

Examples:

```
# Output f's contents, then standard input, then g's contents.
cat f - g

# Copy standard input to standard output.
cat
```

---

Next: [`nl`: Number lines and write files](#nl-invocation), Previous: [`cat`: Concatenate and write files](#cat-invocation), Up: [Output of entire files](#Output-of-entire-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 3.2 `tac`: Concatenate and write files in reverse [¶](#tac_003a-Concatenate-and-write-files-in-reverse)

`tac` copies each file (‘`-`’ means standard input), or
standard input if none are given, to standard output, reversing the
records (lines by default) in each separately. Synopsis:

```
tac [option]... [file]...
```

*Records* are separated by instances of a string (newline by
default). By default, this separator string is attached to the end of
the record that it follows in the file.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-b`’ [¶](#index-_002db-2)

‘`--before`’
:   The separator is attached to the beginning of the record that it
    precedes in the file.

‘`-r`’ [¶](#index-_002dr)

‘`--regex`’
:   Treat the separator string as a regular expression.

‘`-s separator`’ [¶](#index-_002ds-1)

‘`--separator=separator`’
:   Use separator as the record separator, instead of newline.
    Note an empty separator is treated as a zero byte.
    I.e., input and output items are delimited with ASCII NUL.

On systems like MS-DOS that distinguish between text and binary files,
`tac` reads and writes in binary mode.

An exit status of zero indicates success,
and a nonzero value indicates failure.

Example:

```
# Reverse a file character by character.
tac -r -s 'x\|[^x]'
```

---

Next: [`od`: Write files in octal or other formats](#od-invocation), Previous: [`tac`: Concatenate and write files in reverse](#tac-invocation), Up: [Output of entire files](#Output-of-entire-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 3.3 `nl`: Number lines and write files [¶](#nl_003a-Number-lines-and-write-files)

`nl` writes each file (‘`-`’ means standard input), or
standard input if none are given, to standard output, with line numbers
added to some or all of the lines. Synopsis:

```
nl [option]... [file]...
```

`nl` decomposes its input into (logical) page sections;
by default, the line number is reset to 1 at each logical page section.
`nl` treats all of the input files as a single document;
it does not reset line numbers or logical pages between files.

A logical page consists of three sections: header, body, and footer.
Any of the sections can be empty. Each can be numbered in a different
style from the others.

The beginnings of the sections of logical pages are indicated in the
input file by a line containing exactly one of these delimiter strings:

‘`\:\:\:`’
:   start of header;

‘`\:\:`’
:   start of body;

‘`\:`’
:   start of footer.

The characters from which these strings are made can be changed from
‘`\`’ and ‘`:`’ via options (see below), but the pattern
of each string cannot be changed.

A section delimiter is replaced by an empty line on output. Any text
that comes before the first section delimiter string in the input file
is considered to be part of a body section, so `nl` treats a
file that contains no section delimiters as a single body section.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-b style`’ [¶](#index-_002db-3)

‘`--body-numbering=style`’
:   Select the numbering style for lines in the body section of each
    logical page. When a line is not numbered, the current line number
    is not incremented, but the line number separator character is still
    prepended to the line. The styles are:

    ‘`a`’
    :   number all lines,

    ‘`t`’
    :   number only nonempty lines (default for body),

    ‘`n`’
    :   do not number lines (default for header and footer),

    ‘`pbre`’
    :   number only lines that contain a match for the basic regular
        expression bre.
        See [Regular Expressions](https://www.gnu.org/software/grep/manual/grep.html#Regular-Expressions) in The GNU Grep Manual.

‘`-d cd`’ [¶](#index-_002dd)

‘`--section-delimiter=cd`’
:   Set the section delimiter characters to cd; default is
    ‘`\:`’. If only c is given, the second remains ‘`:`’.
    As a GNU extension more than two characters can be specified,
    and also if cd is empty (`-d ''`), then section
    matching is disabled.
    (Remember to protect ‘`\`’ or other metacharacters from shell
    expansion with quotes or extra backslashes.)

‘`-f style`’ [¶](#index-_002df)

‘`--footer-numbering=style`’
:   Analogous to `--body-numbering`.

‘`-h style`’ [¶](#index-_002dh-1)

‘`--header-numbering=style`’
:   Analogous to `--body-numbering`.

‘`-i number`’ [¶](#index-_002di)

‘`--line-increment=number`’
:   Increment line numbers by number (default 1).
    number can be negative to decrement.

‘`-l number`’ [¶](#index-_002dl)

‘`--join-blank-lines=number`’
:   Consider number (default 1) consecutive empty lines to be one
    logical line for numbering, and only number the last one. Where fewer
    than number consecutive empty lines occur, do not number them.
    An empty line is one that contains no characters, not even spaces
    or tabs.

‘`-n format`’ [¶](#index-_002dn-1)

‘`--number-format=format`’
:   Select the line numbering format (default is `rn`):

    ‘`ln`’ [¶](#index-ln-format-for-nl)
    :   left justified, no leading zeros;

    ‘`rn`’ [¶](#index-rn-format-for-nl)
    :   right justified, no leading zeros;

    ‘`rz`’ [¶](#index-rz-format-for-nl)
    :   right justified, leading zeros.

‘`-p`’ [¶](#index-_002dp)

‘`--no-renumber`’
:   Do not reset the line number at the start of a logical page.

‘`-s string`’ [¶](#index-_002ds-2)

‘`--number-separator=string`’
:   Separate the line number from the text line in the output with
    string (default is the TAB character).

‘`-v number`’ [¶](#index-_002dv-1)

‘`--starting-line-number=number`’
:   Set the initial line number on each logical page to number (default 1).
    The starting number can be negative.

‘`-w number`’ [¶](#index-_002dw)

‘`--number-width=number`’
:   Use number characters for line numbers (default 6).

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`base32`: Transform data into printable data](#base32-invocation), Previous: [`nl`: Number lines and write files](#nl-invocation), Up: [Output of entire files](#Output-of-entire-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 3.4 `od`: Write files in octal or other formats [¶](#od_003a-Write-files-in-octal-or-other-formats)

`od` writes an unambiguous representation of each file
(‘`-`’ means standard input), or standard input if none are given.
Synopses:

```
od [option]... [file]...
od [-abcdfilosx]... [file] [[+]offset[.][b]]
od [option]... --traditional [file] [[+]offset[.][b] [[+]label[.][b]]]
```

Each line of output consists of the offset in the input, followed by
groups of data from the file. By default, `od` prints the offset in
octal, and each group of file data is a C `short int`’s worth of input
printed as a single octal number.

If offset is given, it specifies how many input bytes to skip
before formatting and writing. By default, it is interpreted as an
octal number, but the optional trailing decimal point causes it to be
interpreted as decimal. If no decimal is specified and the offset
begins with ‘`0x`’ or ‘`0X`’ it is interpreted as a hexadecimal
number. If there is a trailing ‘`b`’, the number of bytes skipped
will be offset multiplied by 512.

If a command is of both the first and second forms, the second form is
assumed if the last operand begins with ‘`+`’ or (if there are two
operands) a digit. For example, in ‘`od foo 10`’ and ‘`od +10`’
the ‘`10`’ is an offset, whereas in ‘`od 10`’ the ‘`10`’ is a
file name.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-A radix`’ [¶](#index-_002dA-1)

‘`--address-radix=radix`’
:   Select the base in which file offsets are printed. radix can
    be one of the following:

    ‘`d`’
    :   decimal;

    ‘`o`’
    :   octal;

    ‘`x`’
    :   hexadecimal;

    ‘`n`’
    :   none (do not print offsets).

    The default is octal.

‘`--endian=order`’ [¶](#index-_002d_002dendian)
:   Reorder input bytes, to handle inputs with differing byte orders,
    or to provide consistent output independent of the endian convention
    of the current system. Swapping is performed according to the
    specified `--type` size and endian order, which can be
    ‘`little`’ or ‘`big`’.

‘`-j bytes`’ [¶](#index-_002dj)

‘`--skip-bytes=bytes`’
:   Skip bytes input bytes before formatting and writing. If
    bytes begins with ‘`0x`’ or ‘`0X`’, it is interpreted in
    hexadecimal; otherwise, if it begins with ‘`0`’, in octal; otherwise,
    in decimal.
    bytes may be, or may be an integer optionally followed by,
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

‘`-N bytes`’ [¶](#index-_002dN)

‘`--read-bytes=bytes`’
:   Output at most bytes bytes of the input. Prefixes and suffixes on
    `bytes` are interpreted as for the `-j` option.

‘`-S bytes`’ [¶](#index-_002dS-1)

‘`--strings[=bytes]`’
:   Instead of the normal output, output only *string constants*: at
    least bytes consecutive printable characters,
    followed by a zero byte (ASCII NUL).
    Prefixes and suffixes on bytes are interpreted as for the
    `-j` option.

    If bytes is omitted with `--strings`, the default is 3.

‘`-t type`’ [¶](#index-_002dt-1)

‘`--format=type`’
:   Select the format in which to output the file data. type is a
    string of one or more of the below type indicator characters. If you
    include more than one type indicator character in a single type
    string, or use this option more than once, `od` writes one copy
    of each output line using each of the data types that you specified,
    in the order that you specified.

    Adding a trailing “z” to any type specification appends a display
    of the single byte character representation of the printable characters
    to the output line generated by the type specification.

    ‘`a`’
    :   named character, ignoring high-order bit

    ‘`c`’
    :   printable single byte character, C backslash escape
        or a 3 digit octal sequence

    ‘`d`’
    :   signed decimal

    ‘`f`’
    :   floating point (see [Floating point numbers](#Floating-point))

    ‘`o`’
    :   octal

    ‘`u`’
    :   unsigned decimal

    ‘`x`’
    :   hexadecimal

    The type `a` outputs things like ‘`sp`’ for space, ‘`nl`’ for
    newline, and ‘`nul`’ for a zero byte. Only the least significant
    seven bits of each byte is used; the high-order bit is ignored.
    Type `c` outputs
    ‘’, ‘`\n`’, and `\0`, respectively.

    Except for types ‘`a`’ and ‘`c`’, you can specify the number
    of bytes to use in interpreting each number in the given data type
    by following the type indicator character with a decimal integer.
    Alternately, you can specify the size of one of the C compiler’s
    built-in data types by following the type indicator character with
    one of the following characters. For integers (‘`d`’, ‘`o`’,
    ‘`u`’, ‘`x`’):

    ‘`C`’
    :   char

    ‘`S`’
    :   short

    ‘`I`’
    :   int

    ‘`L`’
    :   long

    For floating point (`f`):

    B
    :   [brain 16 bit float](https://en.wikipedia.org/wiki/Bfloat16_floating-point_format)

    H
    :   [half precision float](https://en.wikipedia.org/wiki/Half-precision_floating-point_format)

    F
    :   float

    D
    :   double

    L
    :   long double

‘`-v`’ [¶](#index-_002dv-2)

‘`--output-duplicates`’
:   Output consecutive lines that are identical. By default, when two or
    more consecutive output lines would be identical, `od` outputs only
    the first line, and puts just an asterisk on the following line to
    indicate the elision.

‘`-w[n]`’ [¶](#index-_002dw-1)

‘`--width[=n]`’
:   Dump `n` input bytes per output line. This must be a multiple of
    the least common multiple of the sizes associated with the specified
    output types.

    If this option is not given at all, the default is 16. If n is
    omitted, the default is 32.

The next several options are shorthands for format specifications.
GNU `od` accepts any combination of shorthands and format
specification options. These options accumulate.

‘`-a`’ [¶](#index-_002da)
:   Output as named characters. Equivalent to ‘`-t a`’.

‘`-b`’ [¶](#index-_002db-4)
:   Output as octal bytes. Equivalent to ‘`-t o1`’.

‘`-c`’ [¶](#index-_002dc)
:   Output as printable single byte characters, C backslash escapes
    or 3 digit octal sequences. Equivalent to ‘`-t c`’.

‘`-d`’ [¶](#index-_002dd-1)
:   Output as unsigned decimal two-byte units. Equivalent to ‘`-t u2`’.

‘`-f`’ [¶](#index-_002df-1)
:   Output as floats. Equivalent to ‘`-t fF`’.

‘`-i`’ [¶](#index-_002di-1)
:   Output as decimal ints. Equivalent to ‘`-t dI`’.

‘`-l`’ [¶](#index-_002dl-1)
:   Output as decimal long ints. Equivalent to ‘`-t dL`’.

‘`-o`’ [¶](#index-_002do)
:   Output as octal two-byte units. Equivalent to `-t o2`.

‘`-s`’ [¶](#index-_002ds-3)
:   Output as decimal two-byte units. Equivalent to `-t d2`.

‘`-x`’ [¶](#index-_002dx)
:   Output as hexadecimal two-byte units. Equivalent to ‘`-t x2`’.

‘`--traditional`’ [¶](#index-_002d_002dtraditional)
:   Recognize the non-option label argument that traditional `od`
    accepted. The following syntax:

    ```
    od --traditional [file] [[+]offset[.][b] [[+]label[.][b]]]
    ```

    can be used to specify at most one file and optional arguments
    specifying an offset and a pseudo-start address, label.
    The label argument is interpreted
    just like offset, but it specifies an initial pseudo-address. The
    pseudo-addresses are displayed in parentheses following any normal
    address.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`base64`: Transform data into printable data](#base64-invocation), Previous: [`od`: Write files in octal or other formats](#od-invocation), Up: [Output of entire files](#Output-of-entire-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 3.5 `base32`: Transform data into printable data [¶](#base32_003a-Transform-data-into-printable-data)

`base32` transforms data read from a file, or standard input,
into (or from) base32 encoded form. The base32 encoded form uses
printable ASCII characters to represent binary data.
The usage and options of this command are precisely the
same as for `base64`. See [`base64`: Transform data into printable data](#base64-invocation).
For more general encoding functionality see [`basenc`: Transform data into printable data](#basenc-invocation).

---

Next: [`basenc`: Transform data into printable data](#basenc-invocation), Previous: [`base32`: Transform data into printable data](#base32-invocation), Up: [Output of entire files](#Output-of-entire-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 3.6 `base64`: Transform data into printable data [¶](#base64_003a-Transform-data-into-printable-data)

`base64` transforms data read from a file, or standard input,
into (or from) base64 encoded form. The base64 encoded form uses
printable ASCII characters to represent binary data.
Synopses:

```
base64 [option]... [file]
base64 --decode [option]... [file]
```

The base64 encoding expands data to roughly 133% of the original.
The base32 encoding expands data to roughly 160% of the original.
The format conforms to
[RFC 4648](https://datatracker.ietf.org/doc/rfc4648/).

For more general encoding functionality see [`basenc`: Transform data into printable data](#basenc-invocation).

The program accepts the following options. Also see [Common options](#Common-options).

‘`-w cols`’ [¶](#index-_002dw-2)

‘`--wrap=cols`’
:   During encoding, wrap lines after cols characters. This must be
    a positive number.

    The default is to wrap after 76 characters. Use the value 0 to
    disable line wrapping altogether.

‘`-d`’ [¶](#index-_002dd-2)

‘`--decode`’
:   Change the mode of operation, from the default of encoding data, to
    decoding data. Input is expected to be base64 encoded data, and the
    output will be the original data.

‘`-i`’ [¶](#index-_002di-2)

‘`--ignore-garbage`’
:   When decoding, newlines are always accepted.
    During decoding, ignore unrecognized bytes,
    to permit distorted data to be decoded.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Previous: [`base64`: Transform data into printable data](#base64-invocation), Up: [Output of entire files](#Output-of-entire-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 3.7 `basenc`: Transform data into printable data [¶](#basenc_003a-Transform-data-into-printable-data)

`basenc` transforms data read from a file, or standard input,
into (or from) various common encoding forms. The encoded form uses
printable ASCII characters to represent binary data.

Synopses:

```
basenc encoding [option]... [file]
basenc encoding --decode [option]... [file]
```

The encoding argument is required. If file is omitted,
`basenc` reads from standard input.
The `-w/--wrap`,`-i/--ignore-garbage`,
`-d/--decode` options of this command are precisely the same as
for `base64`. See [`base64`: Transform data into printable data](#base64-invocation).

Supported encodings are:

‘`--base64`’ [¶](#index-_002d_002dbase64)
:   Encode into (or decode from with `-d/--decode`) base64 form.
    The format conforms to
    [RFC 4648#4](https://datatracker.ietf.org/doc/html/rfc4648#section-4).
    Equivalent to the `base64` command.

‘`--base64url`’ [¶](#index-_002d_002dbase64url)
:   Encode into (or decode from with `-d/--decode`) file-and-url-safe
    base64 form (using ‘`_`’ and ‘`-`’ instead of ‘`+`’ and ‘`/`’).
    The format conforms to
    [RFC 4648#5](https://datatracker.ietf.org/doc/html/rfc4648#section-5).

‘`--base32`’ [¶](#index-_002d_002dbase32)
:   Encode into (or decode from with `-d/--decode`) base32 form.
    The encoded data uses the ‘`ABCDEFGHIJKLMNOPQRSTUVWXYZ234567=`’ characters.
    The format conforms to
    [RFC 4648#6](https://datatracker.ietf.org/doc/html/rfc4648#section-6).
    Equivalent to the `base32` command.

‘`--base32hex`’ [¶](#index-_002d_002dbase32hex)
:   Encode into (or decode from with `-d/--decode`) Extended Hex Alphabet
    base32 form. The encoded data uses the
    ‘`0123456789ABCDEFGHIJKLMNOPQRSTUV=`’ characters. The format conforms to
    [RFC 4648#7](https://datatracker.ietf.org/doc/html/rfc4648#section-7).

‘`--base16`’ [¶](#index-_002d_002dbase16)
:   Encode into (or decode from with `-d/--decode`) base16 (hexadecimal)
    form. The encoded data uses the ‘`0123456789ABCDEF`’ characters. The format
    conforms to
    [RFC 4648#8](https://datatracker.ietf.org/doc/html/rfc4648#section-8).

‘`--base2lsbf`’ [¶](#index-_002d_002dbase2lsbf)
:   Encode into (or decode from with `-d/--decode`) binary string form
    (‘`0`’ and ‘`1`’) with the *least* significant bit of every byte
    first.

‘`--base2msbf`’ [¶](#index-_002d_002dbase2msbf)
:   Encode into (or decode from with `-d/--decode`) binary string form
    (‘`0`’ and ‘`1`’) with the *most* significant bit of every byte
    first.

‘`--z85`’ [¶](#index-_002d_002dz85)
:   Encode into (or decode from with `-d/--decode`) Z85 form
    (a modified Ascii85 form). The encoded data uses the
    ‘`0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTU VWXYZ.-:+=^!/*?&<>()[]{}@%$#`’.
    characters. The format conforms to
    [ZeroMQ spec:32/Z85](https://rfc.zeromq.org/spec:32/Z85/).

    When encoding with `--z85`, input length must be a multiple of 4;
    when decoding with `--z85`, input length must be a multiple of 5.

Encoding/decoding examples:

```
$ printf '\376\117\202' | basenc --base64
/k+C

$ printf '\376\117\202' | basenc --base64url
_k-C

$ printf '\376\117\202' | basenc --base32
7ZHYE===

$ printf '\376\117\202' | basenc --base32hex
VP7O4===

$ printf '\376\117\202' | basenc --base16
FE4F82

$ printf '\376\117\202' | basenc --base2lsbf
011111111111001001000001

$ printf '\376\117\202' | basenc --base2msbf
111111100100111110000010

$ printf '\376\117\202\000' | basenc --z85
@.FaC

$ printf 01010100 | basenc --base2msbf --decode
T

$ printf 01010100 | basenc --base2lsbf --decode
*
```

---

Next: [Output of parts of files](#Output-of-parts-of-files), Previous: [Output of entire files](#Output-of-entire-files), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 4 Formatting file contents [¶](#Formatting-file-contents-1)

These commands reformat the contents of files.

* [`fmt`: Reformat paragraph text](#fmt-invocation)
* [`pr`: Paginate or columnate files for printing](#pr-invocation)
* [`fold`: Wrap input lines to fit in specified width](#fold-invocation)

---

Next: [`pr`: Paginate or columnate files for printing](#pr-invocation), Up: [Formatting file contents](#Formatting-file-contents)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]
