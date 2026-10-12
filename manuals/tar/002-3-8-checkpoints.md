### 3.8 Checkpoints

A *checkpoint* is a moment of time before writing nth record to
the archive (a *write checkpoint*), or before reading nth record
from the archive (a *read checkpoint*). Checkpoints allow to
periodically execute arbitrary actions.

The checkpoint facility is enabled using the following option:

`--checkpoint[=n]`
:   Schedule checkpoints before writing or reading each nth record.
    The default value for n is 10.

A list of arbitrary *actions* can be executed at each checkpoint.
These actions include: pausing, displaying textual messages, and
executing arbitrary external programs. Actions are defined using
the `--checkpoint-action` option.

`--checkpoint-action=action`
:   Execute an action at each checkpoint.

The simplest value of action is ‘`echo`’. It instructs
`tar` to display the default message on the standard error
stream upon arriving at each checkpoint. The default message is (in
POSIX locale) ‘`Write checkpoint n`’, for write
checkpoints, and ‘`Read checkpoint n`’, for read checkpoints.
Here, n represents ordinal number of the checkpoint.

In another locales, translated versions of this message are used.

This is the default action, so running:

```
$ tar -c --checkpoint=1000 --checkpoint-action=echo /var
```

is equivalent to:

```
$ tar -c --checkpoint=1000 /var
```

The ‘`echo`’ action also allows to supply a customized message.
You do so by placing an equals sign and the message right after it,
e.g.:

```
--checkpoint-action="echo=Hit %s checkpoint #%u"
```

The ‘`%s`’ and ‘`%u`’ in the above example are
*format specifiers*. The ‘`%s`’ specifier is replaced with
the *type* of the checkpoint: ‘`write`’ or
‘`read`’ (or a corresponding translated version in locales other
than POSIX). The ‘`%u`’ specifier is replaced with
the ordinal number of the checkpoint. Thus, the above example could
produce the following output when used with the `--create`
option:

```
tar: Hit write checkpoint #10
tar: Hit write checkpoint #20
tar: Hit write checkpoint #30
```

The complete list of available format specifiers follows. Some of
them can take optional arguments. These arguments, if given, are
supplied in curly braces between the percent sign and the specifier
letter.

‘`%s`’
:   Print type of the checkpoint (‘`write`’ or ‘`read`’).

‘`%u`’
:   Print number of the checkpoint.

‘`%{r,w,d}T`’
:   Print number of bytes transferred so far and approximate transfer
    speed. Optional arguments supply prefixes to be used before number
    of bytes read, written and deleted, correspondingly. If absent,
    they default to ‘`R`’. ‘`W`’, ‘`D`’. Any or all of them can
    be omitted, so, that e.g. ‘`%{}T`’ means to print corresponding
    statistics without any prefixes. Any surplus arguments, if present,
    are silently ignored.

    ```
    $ tar --delete -f f.tar --checkpoint-action=echo="#%u: %T" main.c
    tar: #1: R: 0 (0B, 0B/s),W: 0 (0B, 0B/s),D: 0
    tar: #2: R: 10240 (10KiB, 19MiB/s),W: 0 (0B, 0B/s),D: 10240
    ```

    See also the ‘`totals`’ action, described below.

‘`%{fmt}t`’
:   Output current local time using fmt as format for `strftime`
    (see [strftime](strftime(3).html#strftime) in strftime(3) man page). The
    ‘`{fmt}`’ part is optional. If not present, the default
    format is ‘`%c`’, i.e. the preferred date and time representation
    for the current locale.

‘`%{n}*`’
:   Pad output with spaces to the nth column. If the
    ‘`{n}`’ part is omitted, the current screen width
    is assumed.

‘`%c`’
:   This is a shortcut for ‘`%{%Y-%m-%d %H:%M:%S}t: %ds, %{read,wrote}T%*\r`’,
    intended mainly for use with ‘`ttyout`’ action (see below).

Aside from format expansion, the message string is subject to
*unquoting*, during which the backslash *escape sequences* are
replaced with their corresponding ASCII characters
(see [escape sequences](#escape-sequences)). E.g. the following action will produce an
audible bell and the message described above at each checkpoint:

```
--checkpoint-action='echo=\aHit %s checkpoint #%u'
```

There is also a special action which produces an audible signal:
‘`bell`’. It is not equivalent to ‘`echo='\a'`’, because
‘`bell`’ sends the bell directly to the console (`/dev/tty`),
whereas ‘`echo='\a'`’ sends it to the standard error.

The ‘`ttyout=string`’ action outputs string to
`/dev/tty`, so it can be used even if the standard output is
redirected elsewhere. The string is subject to the same
modifications as with ‘`echo`’ action. In contrast to the latter,
‘`ttyout`’ does not prepend `tar` executable name to the
string, nor does it output a newline after it. For example, the
following action will print the checkpoint message at the same screen
line, overwriting any previous message:

```
--checkpoint-action="ttyout=Hit %s checkpoint #%u%*\r"
```

Notice the use of ‘`%*`’ specifier to clear out any eventual
remains of the prior output line. As as more complex example,
consider this:

```
--checkpoint-action=ttyout='%{%Y-%m-%d %H:%M:%S}t (%d sec): #%u, %T%*\r'
```

This prints the current local time, number of seconds expired since
tar was started, the checkpoint ordinal number, transferred bytes and
average computed I/O speed.

Another available checkpoint action is ‘`dot`’ (or ‘`.`’). It
instructs `tar` to print a single dot on the standard listing
stream, e.g.:

```
$ tar -c --checkpoint=1000 --checkpoint-action=dot /var
...
```

For compatibility with previous GNU `tar` versions, this action can
be abbreviated by placing a dot in front of the checkpoint frequency,
as shown in the previous section.

The ‘`totals`’ action prints the total number of bytes transferred
so far. The format of the data is the same as for the
`--totals` option (see [totals](#totals)). See also ‘`%T`’ format
specifier of the ‘`echo`’ or ‘`ttyout`’ action.

Yet another action, ‘`sleep`’, pauses `tar` for a specified
amount of seconds. The following example will stop for 30 seconds at each
checkpoint:

```
$ tar -c --checkpoint=1000 --checkpoint-action=sleep=30
```

The `wait=signo` action stops further execution until the
signal signo is delivered. Valid values for signo are:
`SIGHUP`, `SIGQUIT`, `SIGINT`, `SIGUSR1` and
`SIGUSR2`. The ‘`SIG`’ prefix is optional. For example:

```
$ tar -c -f arc --checkpoint=1000 --checkpoint-action wait=USR1 .
```

In this example, GNU `tar` will stop archivation at each 1000th
checkpoint. wait until the ‘`SIGUSR1`’ signal is delivered,
and resume processing.

This action is used by the `genfile` utility to perform
modifications on the input files upon hitting certain checkpoints
(see [genfile](#Exec-Mode)).

Finally, the `exec` action executes a given external command.
For example:

```
$ tar -c --checkpoint=1000 --checkpoint-action=exec=/sbin/cpoint
```

The supplied command can be any valid command invocation, with or
without additional command line arguments. If it does contain
arguments, don’t forget to quote it to prevent it from being split by
the shell. See [Running External Commands](#external), for more detail.

The command gets a copy of `tar`’s environment plus the
following variables:

`TAR_VERSION`
:   GNU `tar` version number.

`TAR_ARCHIVE`
:   The name of the archive `tar` is processing.

`TAR_BLOCKING_FACTOR`
:   Current blocking factor (see [Blocking](#Blocking)).

`TAR_CHECKPOINT`
:   Number of the checkpoint.

`TAR_SUBCOMMAND`
:   A short option describing the operation `tar` is executing.
    See [The Five Advanced `tar` Operations](#Operations), for a complete list of subcommand options.

`TAR_FORMAT`
:   Format of the archive being processed. See [Controlling the Archive Format](#Formats), for a complete
    list of archive format names.

These environment variables can also be passed as arguments to the
command, provided that they are properly escaped, for example:

```
tar -c -f arc.tar \
     --checkpoint-action='exec=/sbin/cpoint $TAR_CHECKPOINT'
```

Notice single quotes to prevent variable names from being expanded by
the shell when invoking `tar`.

Any number of actions can be defined, by supplying several
`--checkpoint-action` options in the command line. For
example, the command below displays two messages, pauses
execution for 30 seconds and executes the `/sbin/cpoint` script:

```
$ tar -c -f arc.tar \
       --checkpoint-action='\aecho=Hit %s checkpoint #%u' \
       --checkpoint-action='echo=Sleeping for 30 seconds' \
       --checkpoint-action='sleep=30' \
       --checkpoint-action='exec=/sbin/cpoint'
```

This example also illustrates the fact that
`--checkpoint-action` can be used without
`--checkpoint`. In this case, the default checkpoint frequency
(at each 10th record) is assumed.

---

Next: [Asking for Confirmation During Operations](#interactive), Previous: [Checkpoints](#checkpoints), Up: [Invoking GNU `tar`](#tar-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 3.9 Controlling Warning Messages

Sometimes, while performing the requested task, GNU `tar` notices
some conditions that are not exactly errors, but which the user
should be aware of. When this happens, `tar` issues a
*warning message* describing the condition. Warning messages
are output to the standard error and they do not affect the exit
code of `tar` command.

GNU `tar` allows the user to suppress some or all of its warning
messages:

`--warning=keyword`
:   Control display of the warning messages identified by keyword.
    If keyword starts with the prefix ‘`no-`’, such messages are
    suppressed. Otherwise, they are enabled.

    Multiple `--warning` options accumulate.

By default, GNU `tar` enables all messages, except those that are
enabled in verbose mode (see [The `--verbose` Option](#verbose-tutorial)). See [Default Warning Settings](#Warning-Defaults), for details.

The subsections below discuss allowed values for keyword along with the
warning messages they control.

* [Keywords controlling `tar` operation](#General-Warnings)
* [Keywords applicable for `tar --create`](#Archive-Creation-Warnings)
* [Keywords applicable for `tar --extract`](#Archive-Extraction-Warnings)
* [Keywords controlling incremental extraction](#Incremental-Extraction-Warnings)
* [Warning Classes](#Warning-Classes)
* [Default Warning Settings](#Warning-Defaults)

---

Next: [Keywords applicable for `tar --create`](#Archive-Creation-Warnings), Up: [Controlling Warning Messages](#warnings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 3.9.1 Keywords controlling `tar` operation

These keywords control warnings that may appear in any GNU `tar`
operation mode:

warning: **filename-with-nuls** [¶](#index-filename_002dwith_002dnuls)
:   ‘`file name read contains nul character`’

warning: **filename-with-nuls** [¶](#index-filename_002dwith_002dnuls-1)
:   ‘`%s: file name read contains nul character`’

warning: **alone-zero-block** [¶](#index-alone_002dzero_002dblock)
:   ‘`A lone zero block at %s`’. Notice, that this warning is
    suppressed if `--ignore-zeros` is in effect (see [Ignoring Blocks of Zeros](#Ignore-Zeros)).

warning: **empty-transform** [¶](#index-empty_002dtransform)
:   ‘`transforms to empty name`’. See [Modifying File and Member Names](#transform).

warning: **missing-zero-blocks** [¶](#index-missing_002dzero_002dblocks)
:   ‘`Terminating zero blocks missing at %s`’. This warning is
    suppressed if `--ignore-zeros` is in effect (see [Ignoring Blocks of Zeros](#Ignore-Zeros)).

---

Next: [Keywords applicable for `tar --extract`](#Archive-Extraction-Warnings), Previous: [Keywords controlling `tar` operation](#General-Warnings), Up: [Controlling Warning Messages](#warnings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 3.9.2 Keywords applicable for `tar --create`

The following keywords control messages that can be issued while
creating archives.

warning: **cachedir** [¶](#index-cachedir)
:   ‘`%s: contains a cache directory tag %s; %s`’

warning: **file-shrank** [¶](#index-file_002dshrank)
:   ‘`%s: File shrank by %s bytes; padding with zeros`’

warning: **xdev** [¶](#index-xdev)
:   ‘`%s: file is on a different filesystem; not dumped`’

warning: **file-ignored** [¶](#index-file_002dignored)
:   ‘`%s: Unknown file type; file ignored`’
      
    ‘`%s: socket ignored`’
      
    ‘`%s: door ignored`’

warning: **file-unchanged** [¶](#index-file_002dunchanged)
:   ‘`%s: file is unchanged; not dumped`’

warning: **ignore-archive** [¶](#index-ignore_002darchive)
:   ‘`%s: archive cannot contain itself; not dumped`’

warning: **file-removed** [¶](#index-file_002dremoved)
:   ‘`%s: File removed before we read it`’

warning: **file-changed** [¶](#index-file_002dchanged)
:   ‘`%s: file changed as we read it`’

    Suppresses warnings about read failures, which can occur if files
    or directories are unreadable, or if they change while being read. This
    keyword applies only if used together with the `--ignore-failed-read`
    option. See [Ignore Failed Read](#Ignore-Failed-Read).

---

Next: [Keywords controlling incremental extraction](#Incremental-Extraction-Warnings), Previous: [Keywords applicable for `tar --create`](#Archive-Creation-Warnings), Up: [Controlling Warning Messages](#warnings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 3.9.3 Keywords applicable for `tar --extract`

The following keywords control warnings that can be issued during
archive extraction.

warning: **existing-file** [¶](#index-existing_002dfile)
:   ‘`%s: skipping existing file`’

warning: **timestamp** [¶](#index-timestamp)
:   ‘`%s: implausibly old time stamp %s`’
      
    ‘`%s: time stamp %s is %s s in the future`’

warning: **contiguous-cast** [¶](#index-contiguous_002dcast)
:   ‘`Extracting contiguous files as regular files`’

warning: **symlink-cast** [¶](#index-symlink_002dcast)
:   ‘`Attempting extraction of symbolic links as hard links`’, warning message
    ‘`Attempting extraction of symbolic links as hard links`’

warning: **unknown-cast** [¶](#index-unknown_002dcast)
:   ‘`%s: Unknown file type '%c', extracted as normal file`’

warning: **ignore-newer** [¶](#index-ignore_002dnewer)
:   ‘`Current %s is newer or same age`’

warning: **unknown-keyword** [¶](#index-unknown_002dkeyword)
:   ‘`Ignoring unknown extended header keyword '%s'`’

warning: **decompress-program** [¶](#index-decompress_002dprogram)
:   Controls verbose description of failures occurring when trying to run
    alternative decompressor programs (see [alternative decompression programs](#alternative-decompression-programs)). This warning is disabled by default (unless
    `--verbose` is used). A common example of what you can get
    when using this warning is:

    ```
    $ tar --warning=decompress-program -x -f archive.Z
    tar (child): cannot run compress: No such file or directory
    tar (child): trying gzip
    ```

    This means that `tar` first tried to decompress
    `archive.Z` using `compress`, and, when that
    failed, switched to `gzip`.

warning: **record-size** [¶](#index-record_002dsize)
:   ‘`Record size = %lu blocks`’

---

Next: [Warning Classes](#Warning-Classes), Previous: [Keywords applicable for `tar --extract`](#Archive-Extraction-Warnings), Up: [Controlling Warning Messages](#warnings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 3.9.4 Keywords controlling incremental extraction

These keywords control warnings that may appear when extracting from
incremental archives.

warning: **rename-directory** [¶](#index-rename_002ddirectory)
:   ‘`%s: Directory has been renamed from %s`’
      
    ‘`%s: Directory has been renamed`’

warning: **new-directory** [¶](#index-new_002ddirectory)
:   ‘`%s: Directory is new`’

warning: **xdev** [¶](#index-xdev-1)
:   ‘`%s: directory is on a different device: not purging`’

warning: **bad-dumpdir** [¶](#index-bad_002ddumpdir)
:   ‘`Malformed dumpdir: 'X' never used`’

---

Next: [Default Warning Settings](#Warning-Defaults), Previous: [Keywords controlling incremental extraction](#Incremental-Extraction-Warnings), Up: [Controlling Warning Messages](#warnings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 3.9.5 Warning Classes

These convenience keywords define *warning classes*. When used,
they affect several warnings at once.

warning: **all** [¶](#index-all)
:   Enable all warning messages.

warning: **none** [¶](#index-none)
:   Disable all warning messages.

warning: **verbose** [¶](#index-verbose-1)
:   A shorthand for all messages enabled when `--verbose`
    (see [The `--verbose` Option](#verbose-tutorial)) is in effect. These are:
    `decompress-program`, `existing-file`, `new-directory`,
    `record-size`, `rename-directory`.

---

Previous: [Warning Classes](#Warning-Classes), Up: [Controlling Warning Messages](#warnings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 3.9.6 Default Warning Settings

GNU `tar` default settings correspond to:

```
--warning=all --warning=no-verbose --warning=no-missing-zero-blocks
```

---

Next: [Running External Commands](#external), Previous: [Controlling Warning Messages](#warnings), Up: [Invoking GNU `tar`](#tar-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 3.10 Asking for Confirmation During Operations

Typically, `tar` carries out a command without stopping for
further instructions. In some situations however, you may want to
exclude some files and archive members from the operation (for instance
if disk or storage space is tight). You can do this by excluding
certain files automatically (see [Choosing Files and Names for `tar`](#Choosing)), or by performing
an operation interactively, using the `--interactive` (`-w`) option.
`tar` also accepts `--confirmation` for this option.

When the `--interactive` (`-w`) option is specified, before
reading, writing, or deleting files, `tar` first prints a message
for each such file, telling what operation it intends to take, then asks
for confirmation on the terminal. The actions which require
confirmation include adding a file to the archive, extracting a file
from the archive, deleting a file from the archive, and deleting a file
from disk. To confirm the action, you must type a line of input
beginning with ‘`y`’. If your input line begins with anything other
than ‘`y`’, `tar` skips that file.

If `tar` is reading the archive from the standard input,
`tar` opens the file `/dev/tty` to support the interactive
communications.

Verbose output is normally sent to standard output, separate from
other error messages. However, if the archive is produced directly
on standard output, then verbose output is mixed with errors on
`stderr`. Producing the archive on standard output may be used
as a way to avoid using disk space, when the archive is soon to be
consumed by another process reading it, say. Some people felt the need
of producing an archive on stdout, still willing to segregate between
verbose output and error output. A possible approach would be using a
named pipe to receive the archive, and having the consumer process to
read from that named pipe. This has the advantage of letting standard
output free to receive verbose output, all separate from errors.

---

Previous: [Asking for Confirmation During Operations](#interactive), Up: [Invoking GNU `tar`](#tar-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 3.11 Running External Commands

Certain GNU `tar` operations imply running external commands that you
supply on the command line. One of such operations is checkpointing,
described above (see [checkpoint exec](#checkpoint-exec)). Another example of this
feature is the `-I` option, which allows you to supply the
program to use for compressing or decompressing the archive
(see [use-compress-program](#use_002dcompress_002dprogram)).

Whenever such operation is requested, `tar` first splits the
supplied command into words much like the shell does. It then treats
the first word as the name of the program or the shell script to execute
and the rest of words as its command line arguments. The program,
unless given as an absolute file name, is searched in the shell’s
`PATH`.

Any additional information is normally supplied to external commands
in environment variables, specific to each particular operation. For
example, the `--checkpoint-action=exec` option, defines the
`TAR_ARCHIVE` variable to the name of the archive being worked
upon. You can, should the need be, use these variables in the
command line of the external command. For example:

```
$ tar -x -f archive.tar \
    --checkpoint-action=exec='printf "%04d in %32s\r" $TAR_CHECKPOINT $TAR_ARCHIVE'
```

This command prints for each checkpoint its number and the name of the
archive, using the same output line on the screen.

Notice the use of single quotes to prevent variable names from being
expanded by the shell when invoking `tar`.

---

Next: [Performing Backups and Restoring Files](#Backups), Previous: [Invoking GNU `tar`](#tar-invocation), Up: [GNU tar: an archiver tool](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]## 4 GNU `tar` Operations

* [Basic GNU `tar` Operations](#Basic-tar)
* [Advanced GNU `tar` Operations](#Advanced-tar)
* [Options Used by `--create`](#create-options)
* [Options Used by `--extract`](#extract-options)
* [Backup options](#backup)
* [Looking Ahead: The Rest of this Manual](#looking-ahead)

---

Next: [Advanced GNU `tar` Operations](#Advanced-tar), Up: [GNU `tar` Operations](#operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 4.1 Basic GNU `tar` Operations

The basic `tar` operations, `--create` (`-c`),
`--list` (`-t`) and `--extract` (`--get`,
`-x`), are currently presented and described in the tutorial
chapter of this manual. This section provides some complementary notes
for these operations.

`--create`

`-c`
:   Creating an empty archive would have some kind of elegance. One can
    initialize an empty archive and later use `--append`
    (`-r`) for adding all members. Some applications would not
    welcome making an exception in the way of adding the first archive
    member. On the other hand, many people reported that it is
    dangerously too easy for `tar` to destroy a magnetic tape with
    an empty archive[9](#FOOT9). The two most common errors are:

    1. Mistakenly using `create` instead of `extract`, when the
       intent was to extract the full contents of an archive. This error
       is likely: keys `c` and `x` are right next to each other on
       the QWERTY keyboard. Instead of being unpacked, the archive then
       gets wholly destroyed. When users speak about *exploding* an
       archive, they usually mean something else :-).
    2. Forgetting the argument to `file`, when the intent was to create
       an archive with a single file in it. This error is likely because a
       tired user can easily add the `f` key to the cluster of option
       letters, by the mere force of habit, without realizing the full
       consequence of doing so. The usual consequence is that the single
       file, which was meant to be saved, is rather destroyed.

    So, recognizing the likelihood and the catastrophic nature of these
    errors, GNU `tar` now takes some distance from elegance, and
    cowardly refuses to create an archive when `--create` option is
    given, there are no arguments besides options, and
    `--files-from` (`-T`) option is *not* used. To get
    around the cautiousness of GNU `tar` and nevertheless create an
    archive with nothing in it, one may still use, as the value for the
    `--files-from` option, a file with no names in it, as shown in
    the following commands:

    ```
    tar --create --file=empty-archive.tar --files-from=/dev/null
    tar -cf empty-archive.tar -T /dev/null
    ```

`--extract`

`--get`

`-x`
:   A socket is stored, within a GNU `tar` archive, as a pipe.

`--list (-t)`
:   GNU `tar` now shows dates as ‘`1996-08-30`’,
    while it used to show them as ‘`Aug 30 1996`’. Preferably,
    people should get used to ISO 8601 dates. Local American dates should
    be made available again with full date localization support, once
    ready. In the meantime, programs not being localizable for dates
    should prefer international dates, that’s really the way to go.

    Look up <http://www.cl.cam.ac.uk/~mgk25/iso-time.html> if you
    are curious, it contains a detailed explanation of the ISO 8601 standard.

---

Next: [Options Used by `--create`](#create-options), Previous: [Basic GNU `tar` Operations](#Basic-tar), Up: [GNU `tar` Operations](#operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 4.2 Advanced GNU `tar` Operations

Now that you have learned the basics of using GNU `tar`, you may want
to learn about further ways in which `tar` can help you.

This chapter presents five, more advanced operations which you probably
won’t use on a daily basis, but which serve more specialized functions.
We also explain the different styles of options and why you might want
to use one or another, or a combination of them in your `tar`
commands. Additionally, this chapter includes options which allow you to
define the output from `tar` more carefully, and provide help and
error correction in special circumstances.

* [The Five Advanced `tar` Operations](#Operations)
* [How to Add Files to Existing Archives: `--append`](#append)
* [Updating an Archive](#update)
* [Combining Archives with `--concatenate`](#concatenate)
* [Removing Archive Members Using `--delete`](#delete)
* [Comparing Archive Members with the File System](#compare)

---

Next: [How to Add Files to Existing Archives: `--append`](#append), Up: [Advanced GNU `tar` Operations](#Advanced-tar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.2.1 The Five Advanced `tar` Operations

In the last chapter, you learned about the first three operations to
`tar`. This chapter presents the remaining five operations to
`tar`: `--append`, `--update`, `--concatenate`,
`--delete`, and `--compare`.

You are not likely to use these operations as frequently as those
covered in the last chapter; however, since they perform specialized
functions, they are quite useful when you do need to use them. We
will give examples using the same directory and files that you created
in the last chapter. As you may recall, the directory is called
`practice`, the files are ‘`jazz`’, ‘`blues`’, ‘`folk`’,
and the two archive files you created are
‘`collection.tar`’ and ‘`music.tar`’.

We will also use the archive files ‘`afiles.tar`’ and
‘`bfiles.tar`’. The archive ‘`afiles.tar`’ contains the members ‘`apple`’,
‘`angst`’, and ‘`aspic`’; ‘`bfiles.tar`’ contains the members
‘`./birds`’, ‘`baboon`’, and ‘`./box`’.

Unless we state otherwise, all practicing you do and examples you follow
in this chapter will take place in the `practice` directory that
you created in the previous chapter; see [Preparing a Practice Directory for Examples](#prepare-for-examples).
(Below in this section, we will remind you of the state of the examples
where the last chapter left them.)

The five operations that we will cover in this chapter are:

`--append`

`-r`
:   Add new entries to an archive that already exists.

`--update`

`-u`
:   Add more recent copies of archive members to the end of an archive, if
    they exist.

`--concatenate`

`--catenate`

`-A`
:   Add one or more pre-existing archives to the end of another archive.

`--delete`
:   Delete items from an archive (does not work on tapes).

`--compare`

`--diff`

`-d`
:   Compare archive members to their counterparts in the file system.

---

Next: [Updating an Archive](#update), Previous: [The Five Advanced `tar` Operations](#Operations), Up: [Advanced GNU `tar` Operations](#Advanced-tar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.2.2 How to Add Files to Existing Archives: `--append`

If you want to add files to an existing archive, you don’t need to
create a new archive; you can use `--append` (`-r`).
The archive must already exist in order to use `--append`. (A
related operation is the `--update` operation; you can use this
to add newer versions of archive members to an existing archive. To learn how to
do this with `--update`, see [Updating an Archive](#update).)

If you use `--append` to add a file that has the same name as an
archive member to an archive containing that archive member, then the
old member is not deleted. What does happen, however, is somewhat
complex. `tar` *allows* you to have infinite number of files
with the same name. Some operations treat these same-named members no
differently than any other set of archive members: for example, if you
view an archive with `--list` (`-t`), you will see all
of those members listed, with their data modification times, owners, etc.

Other operations don’t deal with these members as perfectly as you might
prefer; if you were to use `--extract` to extract the archive,
only the most recently added copy of a member with the same name as
other members would end up in the working directory. This is because
`--extract` extracts an archive in the order the members appeared
in the archive; the most recently archived members will be extracted
last. Additionally, an extracted member will *replace* a file of
the same name which existed in the directory already, and `tar`
will not prompt you about this[10](#FOOT10). Thus, only
the most recently archived member will end up being extracted, as it
will replace the one extracted before it, and so on.

There exists a special option that allows you to get around this
behavior and extract (or list) only a particular copy of the file.
This is `--occurrence` option. If you run `tar` with
this option, it will extract only the first copy of the file. You
may also give this option an argument specifying the number of
copy to be extracted. Thus, for example if the archive
`archive.tar` contained three copies of file `myfile`, then
the command

```
tar --extract --file archive.tar --occurrence=2 myfile
```

would extract only the second copy. See [—occurrence](#Option-Summary), for the description of `--occurrence`
option.

If you want to replace an archive member, use `--delete` to
delete the member you want to remove from the archive, and then use
`--append` to add the member you want to be in the archive. Note
that you can not change the order of the archive; the most recently
added member will still appear last. In this sense, you cannot truly
“replace” one member with another. (Replacing one member with another
will not work on certain types of media, such as tapes; see [Removing Archive Members Using `--delete`](#delete)
and [Tapes and Other Archive Media](#Media), for more information.)

* [Appending Files to an Archive](#appending-files)
* [Multiple Members with the Same Name](#multiple)

---

Next: [Multiple Members with the Same Name](#multiple), Up: [How to Add Files to Existing Archives: `--append`](#append)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.2.2.1 Appending Files to an Archive

The simplest way to add a file to an already existing archive is the
`--append` (`-r`) operation, which writes specified
files into the archive whether or not they are already among the
archived files.

When you use `--append`, you *must* specify file name
arguments, as there is no default. If you specify a file that already
exists in the archive, another copy of the file will be added to the
end of the archive. As with other operations, the member names of the
newly added files will be exactly the same as their names given on the
command line. The `--verbose` (`-v`) option will print
out the names of the files as they are written into the archive.

`--append` cannot be performed on some tape drives, unfortunately,
due to deficiencies in the formats those tape drives use. The archive
must be a valid `tar` archive, or else the results of using this
operation will be unpredictable. See [Tapes and Other Archive Media](#Media).

To demonstrate using `--append` to add a file to an archive,
create a file called `rock` in the `practice` directory.
Make sure you are in the `practice` directory. Then, run the
following `tar` command to add `rock` to
`collection.tar`:

```
$ tar --append --file=collection.tar rock
```

If you now use the `--list` (`-t`) operation, you will see that
`rock` has been added to the archive:

```
$ tar --list --file=collection.tar
-rw-r--r-- me/user          28 1996-10-18 16:31 jazz
-rw-r--r-- me/user          21 1996-09-23 16:44 blues
-rw-r--r-- me/user          20 1996-09-23 16:44 folk
-rw-r--r-- me/user          20 1996-09-23 16:44 rock
```

---

Previous: [Appending Files to an Archive](#appending-files), Up: [How to Add Files to Existing Archives: `--append`](#append)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.2.2.2 Multiple Members with the Same Name

You can use `--append` (`-r`) to add copies of files
which have been updated since the archive was created. (However, we
do not recommend doing this since there is another `tar`
option called `--update`; See [Updating an Archive](#update), for more information.
We describe this use of `--append` here for the sake of
completeness.) When you extract the archive, the older version will
be effectively lost. This works because files are extracted from an
archive in the order in which they were archived. Thus, when the
archive is extracted, a file archived later in time will replace a
file of the same name which was archived earlier, even though the
older version of the file will remain in the archive unless you delete
all versions of the file.

Supposing you change the file `blues` and then append the changed
version to `collection.tar`. As you saw above, the original
`blues` is in the archive `collection.tar`. If you change the
file and append the new version of the file to the archive, there will
be two copies in the archive. When you extract the archive, the older
version of the file will be extracted first, and then replaced by the
newer version when it is extracted.

You can append the new, changed copy of the file `blues` to the
archive in this way:

```
$ tar --append --verbose --file=collection.tar blues
blues
```

Because you specified the `--verbose` option, `tar` has
printed the name of the file being appended as it was acted on. Now
list the contents of the archive:

```
$ tar --list --verbose --file=collection.tar
-rw-r--r-- me/user          28 1996-10-18 16:31 jazz
-rw-r--r-- me/user          21 1996-09-23 16:44 blues
-rw-r--r-- me/user          20 1996-09-23 16:44 folk
-rw-r--r-- me/user          20 1996-09-23 16:44 rock
-rw-r--r-- me/user          58 1996-10-24 18:30 blues
```

The newest version of `blues` is now at the end of the archive
(note the different creation dates and file sizes). If you extract
the archive, the older version of the file `blues` will be
replaced by the newer version. You can confirm this by extracting
the archive and running ‘`ls`’ on the directory.

If you wish to extract the first occurrence of the file `blues`
from the archive, use `--occurrence` option, as shown in
the following example:

```
$ tar --extract -vv --occurrence --file=collection.tar blues
-rw-r--r-- me/user          21 1996-09-23 16:44 blues
```

See [Changing How `tar` Writes Files](#Writing), for more information on `--extract` and
see [–occurrence](#Option-Summary), for a description of
`--occurrence` option.

---

Next: [Combining Archives with `--concatenate`](#concatenate), Previous: [How to Add Files to Existing Archives: `--append`](#append), Up: [Advanced GNU `tar` Operations](#Advanced-tar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.2.3 Updating an Archive

In the previous section, you learned how to use `--append` to
add a file to an existing archive. A related operation is
`--update` (`-u`). The `--update` operation
updates a `tar` archive by comparing the date of the specified
archive members against the date of the file with the same name. If
the file has been modified more recently than the archive member, then
the newer version of the file is added to the archive (as with
`--append`).

Unfortunately, you cannot use `--update` with magnetic tape drives.
The operation will fail.

Both `--update` and `--append` work by adding to the end
of the archive. When you extract a file from the archive, only the
version stored last will wind up in the file system, unless you use
the `--backup` option. See [Multiple Members with the Same Name](#multiple), for a detailed discussion.

* [How to Update an Archive Using `--update`](#how-to-update)

---

Up: [Updating an Archive](#update)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.2.3.1 How to Update an Archive Using `--update`

You must use file name arguments with the `--update`
(`-u`) operation. If you don’t specify any files,
`tar` won’t act on any files and won’t tell you that it didn’t
do anything (which may end up confusing you).

To see the `--update` option at work, create a new file,
`classical`, in your practice directory, and some extra text to the
file `blues`, using any text editor. Then invoke `tar` with
the ‘`update`’ operation and the `--verbose` (`-v`)
option specified, using the names of all the files in the `practice`
directory as file name arguments:

```
$ tar --update -v -f collection.tar blues folk rock classical
blues
classical
$
```

Because we have specified verbose mode, `tar` prints out the names
of the files it is working on, which in this case are the names of the
files that needed to be updated. If you run ‘`tar --list`’ and look
at the archive, you will see `blues` and `classical` at its
end. There will be a total of two versions of the member ‘`blues`’;
the one at the end will be newer and larger, since you added text before
updating it.

The reason `tar` does not overwrite the older file when updating
it is that writing to the middle of a section of tape is a difficult
process. Tapes are not designed to go backward. See [Tapes and Other Archive Media](#Media), for more
information about tapes.

`--update` (`-u`) is not suitable for performing backups for two
reasons: it does not change directory content entries, and it
lengthens the archive every time it is used. The GNU `tar`
options intended specifically for backups are more
efficient. If you need to run backups, please consult [Performing Backups and Restoring Files](#Backups).

---

Next: [Removing Archive Members Using `--delete`](#delete), Previous: [Updating an Archive](#update), Up: [Advanced GNU `tar` Operations](#Advanced-tar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.2.4 Combining Archives with `--concatenate`

Sometimes it may be convenient to add a second archive onto the end of
an archive rather than adding individual files to the archive. To add
one or more archives to the end of another archive, you should use the
`--concatenate` (`--catenate`, `-A`) operation.

To use `--concatenate`, give the first archive with
`--file` option and name the rest of archives to be
concatenated on the command line. The members, and their member
names, will be copied verbatim from those archives to the first
one[11](#FOOT11).
The new, concatenated archive will be called by the same name as the
one given with the `--file` option. As usual, if you omit
`--file`, `tar` will use the value of the environment
variable `TAPE`, or, if this has not been set, the default archive name.

To demonstrate how `--concatenate` works, create two small archives
called `bluesrock.tar` and `folkjazz.tar`, using the relevant
files from `practice`:

```
$ tar -cvf bluesrock.tar blues rock
blues
rock
$ tar -cvf folkjazz.tar folk jazz
folk
jazz
```

If you like, you can run ‘`tar --list`’ to make sure the archives
contain what they are supposed to:

```
$ tar -tvf bluesrock.tar
-rw-r--r-- melissa/user    105 1997-01-21 19:42 blues
-rw-r--r-- melissa/user     33 1997-01-20 15:34 rock
$ tar -tvf jazzfolk.tar
-rw-r--r-- melissa/user     20 1996-09-23 16:44 folk
-rw-r--r-- melissa/user     65 1997-01-30 14:15 jazz
```

We can concatenate these two archives with `tar`:

```
$ tar --concatenate --file=bluesrock.tar jazzfolk.tar
```

If you now list the contents of the `bluesrock.tar`, you will see
that now it also contains the archive members of `jazzfolk.tar`:

```
$ tar --list --file=bluesrock.tar
blues
rock
folk
jazz
```

When you use `--concatenate`, the source and target archives must
already exist and must have been created using compatible format
parameters. Notice, that `tar` does not check whether the
archives it concatenates have compatible formats, it does not
even check if the files are really tar archives.

Like `--append` (`-r`), this operation cannot be performed on some
tape drives, due to deficiencies in the formats those tape drives use.

It may seem more intuitive to you to want or try to use `cat` to
concatenate two archives instead of using the `--concatenate`
operation; after all, `cat` is the utility for combining files.

However, `tar` archives incorporate an end-of-file marker which
must be removed if the concatenated archives are to be read properly as
one archive. `--concatenate` removes the end-of-archive marker
from the target archive before each new archive is appended. If you use
`cat` to combine the archives, the result will not be a valid
`tar` format archive. If you need to retrieve files from an
archive that was added to using the `cat` utility, use the
`--ignore-zeros` (`-i`) option. See [Ignoring Blocks of Zeros](#Ignore-Zeros), for further
information on dealing with archives improperly combined using the
`cat` shell utility.

---

Next: [Comparing Archive Members with the File System](#compare), Previous: [Combining Archives with `--concatenate`](#concatenate), Up: [Advanced GNU `tar` Operations](#Advanced-tar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.2.5 Removing Archive Members Using `--delete`

You can remove members from an archive by using the `--delete`
option. Specify the name of the archive with `--file`
(`-f`) and then specify the names of the members to be deleted;
if you list no member names, nothing will be deleted. The
`--verbose` option will cause `tar` to print the names
of the members as they are deleted. As with `--extract`, you
must give the exact member names when using ‘`tar --delete`’.
`--delete` will remove all versions of the named file from the
archive. The `--delete` operation can run very slowly.

Unlike other operations, `--delete` has no short form.

This operation will rewrite the archive. You can only use
`--delete` on an archive if the archive device allows you to
write to any point on the media, such as a disk; because of this, it
does not work on magnetic tapes. Do not try to delete an archive member
from a magnetic tape; the action will not succeed, and you will be
likely to scramble the archive and damage your tape. There is no safe
way (except by completely re-writing the archive) to delete files from
most kinds of magnetic tape. See [Tapes and Other Archive Media](#Media).

To delete all versions of the file `blues` from the archive
`collection.tar` in the `practice` directory, make sure you
are in that directory, and then,

```
$ tar --list --file=collection.tar
blues
folk
jazz
rock
$ tar --delete --file=collection.tar blues
$ tar --list --file=collection.tar
folk
jazz
rock
```

The `--delete` option has been reported to work properly when
`tar` acts as a filter from `stdin` to `stdout`.

---

Previous: [Removing Archive Members Using `--delete`](#delete), Up: [Advanced GNU `tar` Operations](#Advanced-tar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.2.6 Comparing Archive Members with the File System

The `--compare` (`-d`), or `--diff` operation compares
specified archive members against files with the same names, and then
reports differences in file size, mode, owner, modification date and
contents. You should *only* specify archive member names, not file
names. If you do not name any members, then `tar` will compare the
entire archive. If a file is represented in the archive but does not
exist in the file system, `tar` reports a difference.

You have to specify the record size of the archive when modifying an
archive with a non-default record size.

`tar` ignores files in the file system that do not have
corresponding members in the archive.

The following example compares the archive members `rock`,
`blues` and `funk` in the archive `bluesrock.tar` with
files of the same name in the file system. (Note that there is no file,
`funk`; `tar` will report an error message.)

```
$ tar --compare --file=bluesrock.tar rock blues funk
rock
blues
tar: funk not found in archive
```

The spirit behind the `--compare` (`--diff`,
`-d`) option is to check whether the archive represents the
current state of files on disk, more than validating the integrity of
the archive media. For this latter goal, see [Verifying Data as It is Stored](#verify).

---

Next: [Options Used by `--extract`](#extract-options), Previous: [Advanced GNU `tar` Operations](#Advanced-tar), Up: [GNU `tar` Operations](#operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 4.3 Options Used by `--create`

The previous chapter described the basics of how to use
`--create` (`-c`) to create an archive from a set of files.
See [How to Create Archives](#create). This section described advanced options to be used with
`--create`.

* [Overriding File Metadata](#override)
* [Extended File Attributes](#Extended-File-Attributes)
* [Ignore Failed Read](#Ignore-Failed-Read)

---

Next: [Extended File Attributes](#Extended-File-Attributes), Up: [Options Used by `--create`](#create-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.3.1 Overriding File Metadata

As described above, a `tar` archive keeps, for each member it contains,
its *metadata*, such as modification time, mode and ownership of
the file. GNU `tar` allows to replace these data with other values
when adding files to the archive. The options described in this
section affect creation of archives of any type. For POSIX archives,
see also [Controlling Extended Header Keywords](#PAX-keywords), for additional ways of controlling
metadata, stored in the archive.

`--mode=permissions`
:   When adding files to an archive, `tar` will use
    permissions for the archive members, rather than the permissions
    from the files. permissions can be specified either as an octal
    number or as symbolic permissions, like with
    `chmod` (See [File
    permissions](https://www.gnu.org/software/coreutils/manual/coreutils#File-permissions) in GNU core utilities. This reference
    also has useful information for those not being overly familiar with
    the UNIX permission system). Using latter syntax allows for
    more flexibility. For example, the value ‘`a+rw`’ adds read and write
    permissions for everybody, while retaining executable bits on directories
    or on any other file already marked as executable:

    ```
    $ tar -c -f archive.tar --mode='a+rw' .
    ```

`--mtime=date` [¶](#index-mtime)
:   When adding files to an archive, `tar` uses date as
    the modification time of members when creating archives, instead of
    their actual modification times. The argument date can be
    either a textual date representation in almost arbitrary format
    (see [Date input formats](#Date-input-formats)) or a name of an existing file, starting
    with ‘`/`’ or ‘`.`’. In the latter case, the modification time
    of that file is used.

    The following example sets the modification date to 00:00:00 UTC on
    January 1, 1970:

    ```
    $ tar -c -f archive.tar --mtime='@0' .
    ```

    When used with `--verbose` (see [The `--verbose` Option](#verbose-tutorial)) GNU `tar`
    converts the specified date back to a textual form and compares it
    with the one given with `--mtime`.
    If the two forms differ, `tar` prints both forms in a message,
    to help the user check that the right date is being used.

    For example:

    ```
    $ tar -c -f archive.tar -v --mtime=yesterday .
    tar: Option --mtime: Treating date 'yesterday' as 2006-06-20
    13:06:29.152478
    …
    ```

    When used with `--clamp-mtime` GNU `tar` sets the
    modification date to date only on files whose actual modification
    date is later than date. This makes it easier to build
    reproducible archives given a common timestamp for generated files
    while still retaining the original timestamps of untouched files.
    See [Making `tar` Archives More Reproducible](#Reproducibility).

    ```
    $ tar -c -f archive.tar --clamp-mtime --mtime="$SOURCE_EPOCH" .
    ```

`--owner=user` [¶](#index-owner)
:   Specifies that `tar` should use user as the owner of members
    when creating archives, instead of the user associated with the source
    file.

    If user contains a colon, it is taken to be of the form
    name:id where a nonempty name specifies the user
    name and a nonempty id specifies the decimal numeric user
    ID. If user does not contain a colon, it is taken to
    be a user number if it is one or more decimal digits; otherwise it is
    taken to be a user name.

    If a name is given but no number, the number is inferred from the
    current host’s user database if possible, and the file’s user number
    is used otherwise. If a number is given but no name, the name is
    inferred from the number if possible, and an empty name is used
    otherwise. If both name and number are given, the user database is
    not consulted, and the name and number need not be valid on the
    current host.

    There is no value indicating a missing number, and ‘`0`’ usually means
    `root`. Some people like to force ‘`0`’ as the value to offer in
    their distributions for the owner of files, because the `root` user is
    anonymous anyway, so that might as well be the owner of anonymous
    archives. For example:

    ```
    $ tar -c -f archive.tar --owner=0 .
    ```

    or:

    ```
    $ tar -c -f archive.tar --owner=root .
    ```

`--group=group` [¶](#index-group)
:   Files added to the `tar` archive will have a group ID of group,
    rather than the group from the source file. As with `--owner`,
    the argument group can be an existing group symbolic name, or a
    decimal numeric group ID, or name:id.

The `--owner` and `--group` options affect all files
added to the archive. GNU `tar` provides also two options that allow
for more detailed control over owner translation:

`--owner-map=file`
:   Read UID translation map from file.

    When reading, empty lines are ignored. The ‘`#`’ sign, unless
    quoted, introduces a comment, which extends to the end of the line.
    Each nonempty line defines mapping for a single UID. It must consist
    of two fields separated by any amount of whitespace. The first field
    defines original username and UID. It can be a valid user name or
    a valid UID prefixed with a plus sign. In both cases the
    corresponding UID or user name is inferred from the current host’s
    user database.

    The second field defines the UID and username to map the original one
    to. Its format can be the same as described above. Otherwise, it can
    have the form newname:newuid, in which case neither
    newname nor newuid are required to be valid as per the
    user database.

    For example, consider the following file:

    ```
    +10     bin
    smith   root:0
    ```

    Given this file, each input file that is owner by UID 10 will be
    stored in archive with owner name ‘`bin`’ and owner UID
    corresponding to ‘`bin`’. Each file owned by user ‘`smith`’
    will be stored with owner name ‘`root`’ and owner ID 0. Other
    files will remain unchanged.

    When used together with `--owner-map`, the `--owner`
    option affects only files whose owner is not listed in the map file.

`--group-map=file`
:   Read GID translation map from file.

    The format of file is the same as for `--owner-map`
    option:

    Each nonempty line defines mapping for a single GID. It must consist
    of two fields separated by any amount of whitespace. The first field
    defines original group name and GID. It can be a valid group name or
    a valid GID prefixed with a plus sign. In both cases the
    corresponding GID or user name is inferred from the current host’s
    group database.

    The second field defines the GID and group name to map the original one
    to. Its format can be the same as described above. Otherwise, it can
    have the form newname:newgid, in which case neither
    newname nor newgid are required to be valid as per the
    group database.

    When used together with `--group-map`, the `--group`
    option affects only files whose owner group is not rewritten using the
    map file.

---

Next: [Ignore Failed Read](#Ignore-Failed-Read), Previous: [Overriding File Metadata](#override), Up: [Options Used by `--create`](#create-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.3.2 Extended File Attributes

Extended file attributes are name-value pairs that can be
associated with each node in a file system. Despite the fact that
POSIX.1e draft which proposed them has been withdrawn, the extended
file attributes are supported by many file systems. GNU `tar` can
store extended file attributes along with the files. This feature is
controlled by the following command line arguments:

`--xattrs`
:   Enable extended attributes support. When used with `--create`,
    this option instructs GNU `tar` to store extended file attribute in the
    created archive. This implies POSIX.1-2001 archive format
    (`--format=pax`).

    When used with `--extract`, this option tells `tar`,
    for each file extracted, to read stored attributes from the archive
    and to apply them to the file.

`--no-xattrs`
:   Disable extended attributes support. This is the default.

Attribute names are strings prefixed by a *namespace* name and a dot.
Currently, four namespaces exist: ‘`user`’, ‘`trusted`’,
‘`security`’ and ‘`system`’. By default, when `--xattrs`
is used, all names are stored in the archive (with `--create`),
but only ‘`user`’ namespace is extracted (if using `--extract`).
The reason for this behavior is that any other, system defined attributes
don’t provide us sufficient compatibility promise. Storing all attributes
is safe operation for the archiving purposes. Though extracting those
(often security related) attributes on a different system than originally
archived can lead to extraction failures, or even misinterpretations.
This behavior can be controlled using the following options:

`--xattrs-exclude=pattern`
:   Specify exclude pattern for extended attributes.

`--xattrs-include=pattern`
:   Specify include pattern for extended attributes.

Here, the pattern is a globbing pattern. For example, the
following command:

```
$ tar --xattrs --xattrs-exclude='user.*' -cf a.tar .
```

will include in the archive `a.tar` all attributes, except those
from the ‘`user`’ namespace.

Users shall check the attributes are binary compatible with the target system
before any other namespace is extracted with an explicit
`--xattrs-include` option.

Any number of these options can be given, thereby creating lists of
include and exclude patterns.

When both options are used, first `--xattrs-include` is applied
to select the set of attribute names to keep, and then
`--xattrs-exclude` is applied to the resulting set. In other
words, only those attributes will be stored, whose names match one
of the regexps in `--xattrs-include` and don’t match any of
the regexps from `--xattrs-exclude`.

When listing the archive, if both `--xattrs` and
`--verbose` options are given, files that have extended
attributes are marked with an asterisk following their permission
mask. For example:

```
-rw-r--r--* smith/users      110 2016-03-16 16:07 file
```

When two or more `--verbose` options are given, a detailed
listing of extended attributes is printed after each file entry. Each
attribute is listed on a separate line, which begins with two spaces
and the letter ‘`x`’ indicating extended attribute. It is followed
by a colon, length of the attribute and its name, e.g.:

```
-rw-r--r--* smith/users      110 2016-03-16 16:07 file
  x:  7 user.mime_type
  x: 32 trusted.md5sum
```

File access control lists (*ACL*) are another actively used feature
proposed by the POSIX.1e standard. Each ACL consists of a set of ACL
entries, each of which describes the access permissions on the file for
an individual user or a group of users as a combination of read, write
and search/execute permissions.

Whether or not to use ACLs is controlled by the following two options:

`--acls`
:   Enable POSIX ACLs support. When used with `--create`,
    this option instructs GNU `tar` to store ACLs in the
    created archive. This implies POSIX.1-2001 archive format
    (`--format=pax`).

    When used with `--extract`, this option tells `tar`,
    to restore ACLs for each file extracted (provided they are present
    in the archive).

`--no-acls`
:   Disable POSIX ACLs support. This is the default.

When listing the archive, if both `--acls` and
`--verbose` options are given, files that have ACLs are marked
with a plus sign following their permission mask. For example:

```
-rw-r--r--+ smith/users      110 2016-03-16 16:07 file
```

When two or more `--verbose` options are given, a detailed
listing of ACL is printed after each file entry:

```
-rw-r--r--+ smith/users      110 2016-03-16 16:07 file
  a: user::rw-,user:gray:-w-,group::r--,mask::rw-,other::r--
```

*Security-Enhanced Linux* (*SELinux* for short) is a Linux
kernel security module that provides a mechanism for supporting access
control security policies, including so-called mandatory access
controls (*MAC*). Support for SELinux attributes is controlled by
the following command line options:

`--selinux`
:   Enable the SELinux context support.

`--no-selinux`
:   Disable SELinux context support.

---

Previous: [Extended File Attributes](#Extended-File-Attributes), Up: [Options Used by `--create`](#create-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.3.3 Ignore Failed Read

`--ignore-failed-read` [¶](#index-ignore_002dfailed_002dread)
:   Do not exit with nonzero if there are mild problems while reading.

This option has effect only during creation. It instructs tar to
treat as mild conditions any missing or unreadable files (directories),
or files that change while reading.
Such failures don’t affect the program exit code, and the
corresponding diagnostic messages are marked as warnings, not errors.
These warnings can be suppressed using the
`--warning=failed-read` option (see [Controlling Warning Messages](#warnings)).

---

Next: [Backup options](#backup), Previous: [Options Used by `--create`](#create-options), Up: [GNU `tar` Operations](#operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]
