### 3.2 Using `tar` Options

GNU `tar` has a total of eight operating modes which
allow you to perform a variety of tasks. You are required to choose
one operating mode each time you employ the `tar` program by
specifying one, and only one operation as an argument to the
`tar` command (the corresponding options may be found
at [The Three Most Frequently Used Operations](#frequent-operations) and [The Five Advanced `tar` Operations](#Operations)). Depending on
circumstances, you may also wish to customize how the chosen operating
mode behaves. For example, you may wish to change the way the output
looks, or the format of the files that you wish to archive may require
you to do something special in order to make the archive look right.

You can customize and control `tar`’s performance by running
`tar` with one or more options (such as `--verbose`
(`-v`), which we used in the tutorial). As we said in the
tutorial, *options* are arguments to `tar` which are (as
their name suggests) optional. Depending on the operating mode, you
may specify one or more options. Different options will have different
effects, but in general they all change details of the operation, such
as archive format, archive name, or level of user interaction. Some
options make sense with all operating modes, while others are
meaningful only with particular modes. You will likely use some
options frequently, while you will only use others infrequently, or
not at all. (A full list of options is available in see [All `tar` Options](#All-Options).)

The `TAR_OPTIONS` environment variable specifies default options to
be placed in front of any explicit options. For example, if
`TAR_OPTIONS` is ‘`-v --unlink-first`’, `tar` behaves as
if the two options `-v` and `--unlink-first` had been
specified before any explicit options. Option specifications are
separated by whitespace. A backslash escapes the next character, so it
can be used to specify an option containing whitespace or a backslash.

Note that `tar` options are case sensitive. For example, the
options `-T` and `-t` are different; the first requires an
argument for stating the name of a file providing a list of names,
while the second does not require an argument and is another way to
write `--list` (`-t`).

In addition to the eight operations, there are many options to
`tar`, and three different styles for writing both: long (mnemonic)
form, short form, and old style. These styles are discussed below.
Both the options and the operations can be written in any of these three
styles.

---

Next: [All `tar` Options](#All-Options), Previous: [Using `tar` Options](#using-tar-options), Up: [Invoking GNU `tar`](#tar-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 3.3 The Three Option Styles

There are three styles for writing operations and options to the command
line invoking `tar`. The different styles were developed at
different times during the history of `tar`. These styles will be
presented below, from the most recent to the oldest.

Some options must take an argument[2](#FOOT2). Where you *place* the
arguments generally depends on which style of options you choose. We
will detail specific information relevant to each option style in the
sections on the different option styles, below. The differences are
subtle, yet can often be very important; incorrect option placement
can cause you to overwrite a number of important files. We urge you
to note these differences, and only use the option style(s) which
makes the most sense to you until you feel comfortable with the others.

Some options *may* take an argument. Such options may have at
most long and short forms, they do not have old style equivalent. The
rules for specifying an argument for such options are stricter than
those for specifying mandatory arguments. Please, pay special
attention to them.

* [Long Option Style](#Long-Options)
* [Short Option Style](#Short-Options)
* [Old Option Style](#Old-Options)
* [Mixing Option Styles](#Mixing)

---

Next: [Short Option Style](#Short-Options), Up: [The Three Option Styles](#Styles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 3.3.1 Long Option Style

Each option has at least one *long* (or *mnemonic*) name starting with two
dashes in a row, e.g., `--list`. The long names are more clear than
their corresponding short or old names. It sometimes happens that a
single long option has many different names which are
synonymous, such as `--compare` and `--diff`. In addition,
long option names can be given unique abbreviations. For example,
`--cre` can be used in place of `--create` because there is no
other long option which begins with ‘`cre`’. (One way to find
this out is by trying it and seeing what happens; if a particular
abbreviation could represent more than one option, `tar` will tell
you that that abbreviation is ambiguous and you’ll know that that
abbreviation won’t work. You may also choose to run ‘`tar --help`’
to see a list of options. Be aware that if you run `tar` with a
unique abbreviation for the long name of an option you didn’t want to
use, you are stuck; `tar` will perform the command as ordered.)

Long options are meant to be obvious and easy to remember, and their
meanings are generally easier to discern than those of their
corresponding short options (see below). For example:

```
$ tar --create --verbose --blocking-factor=20 --file=/dev/rmt0
```

gives a fairly good set of hints about what the command does, even
for those not fully acquainted with `tar`.

Long options which require arguments take those arguments
immediately following the option name. There are two ways of
specifying a mandatory argument. It can be separated from the
option name either by an equal sign, or by any amount of
white space characters. For example, the `--file` option (which
tells the name of the `tar` archive) is given a file such as
`archive.tar` as argument by using any of the following notations:
`--file=archive.tar` or `--file archive.tar`.

In contrast, optional arguments must always be introduced using
an equal sign. For example, the `--backup` option takes
an optional argument specifying backup type. It must be used
as `--backup=backup-type`.

---

Next: [Old Option Style](#Old-Options), Previous: [Long Option Style](#Long-Options), Up: [The Three Option Styles](#Styles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 3.3.2 Short Option Style

Most options also have a *short option* name. Short options start with
a single dash, and are followed by a single character, e.g., `-t`
(which is equivalent to `--list`). The forms are absolutely
identical in function; they are interchangeable.

The short option names are faster to type than long option names.

Short options which require arguments take their arguments immediately
following the option, usually separated by white space. It is also
possible to stick the argument right after the short option name, using
no intervening space. For example, you might write `-f archive.tar` or `-farchive.tar` instead of using
`--file=archive.tar`. Both `--file=archive-name` and
`-f archive-name` denote the option which indicates a
specific archive, here named `archive.tar`.

Short options which take optional arguments take their arguments
immediately following the option letter, *without any intervening
white space characters*.

Short options’ letters may be clumped together, but you are not
required to do this (as compared to old options; see below). When
short options are clumped as a set, use one (single) dash for them
all, e.g., ‘`tar -cvf`’. Only the last option in
such a set is allowed to have an argument[3](#FOOT3).

When the options are separated, the argument for each option which requires
an argument directly follows that option, as is usual for Unix programs.
For example:

```
$ tar -c -v -b 20 -f /dev/rmt0
```

If you reorder short options’ locations, be sure to move any arguments
that belong to them. If you do not move the arguments properly, you may
end up overwriting files.

---

Next: [Mixing Option Styles](#Mixing), Previous: [Short Option Style](#Short-Options), Up: [The Three Option Styles](#Styles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 3.3.3 Old Option Style

As far as we know, all `tar` programs, GNU and
non-GNU, support *old options*: that is, if the first
argument does not start with ‘`-`’, it is assumed to specify option
letters. GNU `tar` supports old options not only for historical
reasons, but also because many people are used to them. If the first
argument does not start with a dash, you are announcing the old option
style instead of the short option style; old options are decoded
differently.

Like short options, old options are single letters. However, old options
must be written together as a single clumped set, without spaces separating
them or dashes preceding them. This set
of letters must be the first to appear on the command line, after the
`tar` program name and some white space; old options cannot appear
anywhere else. The letter of an old option is exactly the same letter as
the corresponding short option. For example, the old option ‘`t`’ is
the same as the short option `-t`, and consequently, the same as the
long option `--list`. So for example, the command ‘`tar cv`’ specifies the option `-v` in addition to the operation `-c`.

When options that need arguments are given together with the command,
all the associated arguments follow, in the same order as the options.
Thus, the example given previously could also be written in the old
style as follows:

```
$ tar cvbf 20 /dev/rmt0
```

Here, ‘`20`’ is the argument of `-b` and ‘`/dev/rmt0`’ is
the argument of `-f`.

The old style syntax can make it difficult to match
option letters with their corresponding arguments, and is often
confusing. In the command ‘`tar cvbf 20 /dev/rmt0`’, for example,
‘`20`’ is the argument for `-b`, ‘`/dev/rmt0`’ is the
argument for `-f`, and `-v` does not have a corresponding
argument. Even using short options like in ‘`tar -c -v -b 20 -f /dev/rmt0`’ is clearer, putting all arguments next to the option they
pertain to.

If you want to reorder the letters in the old option argument, be
sure to reorder any corresponding argument appropriately.

This old way of writing `tar` options can surprise even experienced
users. For example, the two commands:

```
tar cfz archive.tar.gz file
tar -cfz archive.tar.gz file
```

are quite different. The first example uses `archive.tar.gz` as
the value for option ‘`f`’ and recognizes the option ‘`z`’. The
second example, however, uses `z` as the value for option
‘`f`’ — probably not what was intended.

This second example could be corrected in many ways, among which the
following are equivalent:

```
tar -czf archive.tar.gz file
tar -cf archive.tar.gz -z file
tar cf archive.tar.gz -z file
```

---

Previous: [Old Option Style](#Old-Options), Up: [The Three Option Styles](#Styles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 3.3.4 Mixing Option Styles

All three styles may be intermixed in a single `tar` command,
so long as the rules for each style are fully
respected[4](#FOOT4). Old style options and either of the modern styles of
options may be mixed within a single `tar` command. However,
old style options must be introduced as the first arguments only,
following the rule for old options (old options must appear directly
after the `tar` command and some white space). Modern options
may be given only after all arguments to the old options have been
collected. If this rule is not respected, a modern option might be
falsely interpreted as the value of the argument to one of the old
style options.

For example, all the following commands are wholly equivalent, and
illustrate the many combinations and orderings of option styles.

```
tar --create --file=archive.tar
tar --create -f archive.tar
tar --create -farchive.tar
tar --file=archive.tar --create
tar --file=archive.tar -c
tar -c --file=archive.tar
tar -c -f archive.tar
tar -c -farchive.tar
tar -cf archive.tar
tar -cfarchive.tar
tar -f archive.tar --create
tar -f archive.tar -c
tar -farchive.tar --create
tar -farchive.tar -c
tar c --file=archive.tar
tar c -f archive.tar
tar c -farchive.tar
tar cf archive.tar
tar f archive.tar --create
tar f archive.tar -c
tar fc archive.tar
```

On the other hand, the following commands are *not* equivalent to
the previous set:

```
tar -f -c archive.tar
tar -fc archive.tar
tar -fcarchive.tar
tar -farchive.tarc
tar cfarchive.tar
```

These last examples mean something completely different from what the
user intended (judging based on the example in the previous set which
uses long options, whose intent is therefore very clear). The first
four specify that the `tar` archive would be a file named
`-c`, ‘`c`’, ‘`carchive.tar`’ or ‘`archive.tarc`’,
respectively. The first two examples also specify a single non-option,
name argument having the value ‘`archive.tar`’. The last
example contains only old style option letters (repeating option
‘`c`’ twice), not all of which are meaningful (eg., ‘`.`’,
‘`h`’, or ‘`i`’), with no argument value.

---

Next: [GNU `tar` documentation](#help), Previous: [The Three Option Styles](#Styles), Up: [Invoking GNU `tar`](#tar-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 3.4 All `tar` Options

The coming manual sections contain an alphabetical listing of all
`tar` operations and options, with brief descriptions and
cross-references to more in-depth explanations in the body of the manual.
They also contain an alphabetically arranged table of the short option
forms with their corresponding long option. You can use this table as
a reference for deciphering `tar` commands in scripts.

* [Operations](#Operation-Summary)
* [`tar` Options](#Option-Summary)
* [Short Options Cross Reference](#Short-Option-Summary)
* [Position-Sensitive Options](#Position_002dSensitive-Options)

---

Next: [`tar` Options](#Option-Summary), Up: [All `tar` Options](#All-Options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 3.4.1 Operations

`--append`

`-r`
:   Appends files to the end of the archive. See [How to Add Files to Existing Archives: `--append`](#append).

`--catenate`

`-A`
:   Same as `--concatenate`. See [Combining Archives with `--concatenate`](#concatenate).

`--compare`

`-d`
:   Compares archive members with their counterparts in the file
    system, and reports differences in file size, mode, owner,
    modification date and contents. See [Comparing Archive Members with the File System](#compare).

`--concatenate`

`-A`
:   Appends other `tar` archives to the end of the archive.
    See [Combining Archives with `--concatenate`](#concatenate).

`--create`

`-c`
:   Creates a new `tar` archive. See [How to Create Archives](#create).

`--delete`
:   Deletes members from the archive. Don’t try this on an archive on a
    tape! See [Removing Archive Members Using `--delete`](#delete).

`--diff`

`-d`
:   Same as `--compare`. See [Comparing Archive Members with the File System](#compare).

`--extract`

`-x`
:   Extracts members from the archive into the file system. See [How to Extract Members from an Archive](#extract).

`--get`

`-x`
:   Same as `--extract`. See [How to Extract Members from an Archive](#extract).

`--list`

`-t`
:   Lists the members in an archive. See [How to List Archives](#list).

`--update`

`-u`
:   Adds files to the end of the archive, but only if they are newer than
    their counterparts already in the archive, or if they do not already
    exist in the archive. See [Updating an Archive](#update).

---

Next: [Short Options Cross Reference](#Short-Option-Summary), Previous: [Operations](#Operation-Summary), Up: [All `tar` Options](#All-Options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 3.4.2 `tar` Options

`--add-file=file`
:   This option has the same effect as adding file to the command
    line. It is useful if file begins with a ‘`-`’ and can be
    confused with a command-line option (see [add-file](#add_002dfile)).

`--absolute-names`

`-P`
:   Normally when creating an archive, `tar` strips an initial
    ‘`/`’ from member names, and when extracting from an archive `tar`
    treats names specially if they have initial ‘`/`’ or internal
    ‘`..`’. This option disables that behavior. See [Absolute File Names](#absolute).

`--acls`
:   Enable POSIX ACLs support. See [acls](#Extended-File-Attributes).

`--after-date`
:   (See `--newer`, see [Operating Only on New Files](#after))

`--anchored`
:   A pattern must match an initial subsequence of the name’s components.
    See [Controlling Pattern-Matching](#controlling-pattern_002dmatching).

`--atime-preserve`

`--atime-preserve=replace`

`--atime-preserve=system`
:   Attempt to preserve the access time of files when reading them. This
    option currently is effective only on files that you own, unless you
    have superuser privileges.

    `--atime-preserve=replace` remembers the access time of a file
    before reading it, and then restores the access time afterwards. This
    may cause problems if other programs are reading the file at the same
    time, as the times of their accesses will be lost. On most platforms
    restoring the access time also requires `tar` to restore the
    data modification time too, so this option may also cause problems if
    other programs are writing the file at the same time (`tar` attempts
    to detect this situation, but cannot do so reliably due to race
    conditions). Worse, on most platforms restoring the access time also
    updates the status change time, which means that this option is
    incompatible with incremental backups.

    `--atime-preserve=system` avoids changing time stamps on files,
    without interfering with time stamp updates
    caused by other programs, so it works better with incremental backups.
    However, it requires a special `O_NOATIME` option from the
    underlying operating and file system implementation, and it also requires
    that searching directories does not update their access times. As of
    this writing (November 2005) this works only with Linux, and only with
    Linux kernels 2.6.8 and later. Worse, there is currently no reliable
    way to know whether this feature actually works. Sometimes
    `tar` knows that it does not work, and if you use
    `--atime-preserve=system` then `tar` complains and
    exits right away. But other times `tar` might think that the
    option works when it actually does not.

    Currently `--atime-preserve` with no operand defaults to
    `--atime-preserve=replace`, but this may change in the future
    as support for `--atime-preserve=system` improves.

    If your operating or file system does not support
    `--atime-preserve=system`, you might be able to preserve access
    times reliably by using the `mount` command. For example,
    you can mount the file system read-only, or access the file system via
    a read-only loopback mount, or use the ‘`noatime`’ mount option
    available on some systems. However, mounting typically requires
    superuser privileges and can be a pain to manage.

`--auto-compress`

`-a`
:   During a `--create` operation, enables automatic compressed
    format recognition based on the archive suffix. The effect of this
    option is cancelled by `--no-auto-compress`. See [Creating and Reading Compressed Archives](#gzip).

`--backup=backup-type`
:   Rather than deleting files from the file system, `tar` will
    back them up using simple or numbered backups, depending upon
    backup-type. See [Backup options](#backup).

`--block-number`

`-R`
:   With this option present, `tar` prints error messages for read errors
    with the block number in the archive file. See [block-number](#block_002dnumber).

`--blocking-factor=blocking`

`-b blocking`
:   Sets the blocking factor `tar` uses to blocking x 512 bytes per
    record. See [The Blocking Factor of an Archive](#Blocking-Factor).

`--bzip2`

`-j`
:   This option tells `tar` to read or write archives through
    `bzip2`. See [Creating and Reading Compressed Archives](#gzip).

`--check-device`
:   Check device numbers when creating a list of modified files for
    incremental archiving. This is the default. See [device numbers](#device-numbers),
    for a detailed description.

`--checkpoint[=number]`
:   This option directs `tar` to print periodic checkpoint
    messages as it reads through the archive. It is intended for when you
    want a visual indication that `tar` is still running, but
    don’t want to see `--verbose` output. You can also instruct
    `tar` to execute a list of actions on each checkpoint, see
    `--checkpoint-action` below. For a detailed description, see
    [Checkpoints](#checkpoints).

`--checkpoint-action=action`
:   Instruct `tar` to execute an action upon hitting a
    breakpoint. Here we give only a brief outline. See [Checkpoints](#checkpoints),
    for a complete description.

    The action argument can be one of the following:

    bell
    :   Produce an audible bell on the console.

    dot

    .
    :   Print a single dot on the standard listing stream.

    echo
    :   Display a textual message on the standard error, with the status and
        number of the checkpoint. This is the default.

    echo=string
    :   Display string on the standard error. Before output, the string
        is subject to meta-character expansion.

    exec=command
    :   Execute the given command.

    sleep=time
    :   Wait for time seconds.

    ttyout=string
    :   Output string on the current console (`/dev/tty`).

    totals
    :   Print statistics (see see [totals](#totals)).

    wait=signo
    :   Wait for signal signo.

    Several `--checkpoint-action` options can be specified. The
    supplied actions will be executed in order of their appearance in the
    command line.

    Using `--checkpoint-action` without `--checkpoint`
    assumes default checkpoint frequency of one checkpoint per 10 records.

`--check-links`

`-l`
:   If this option was given, `tar` will check the number of links
    dumped for each processed file. If this number does not match the
    total number of hard links for the file, a warning message will be
    output [5](#FOOT5).

    See [Hard Links](#hard-links).

`--compress`

`--uncompress`

`-Z`
:   `tar` will use the `compress` program when reading or
    writing the archive. This allows you to directly act on archives
    while saving space. See [Creating and Reading Compressed Archives](#gzip).

`--clamp-mtime`
:   (See `--mtime`.)

`--confirmation`
:   (See `--interactive`.) See [Asking for Confirmation During Operations](#interactive).

`--delay-directory-restore`
:   Delay setting modification times and permissions of extracted
    directories until the end of extraction. See [Directory Modification Times and Permissions](#Directory-Modification-Times-and-Permissions).

`--dereference`

`-h`
:   When reading or writing a file to be archived, `tar` accesses
    the file that a symbolic link points to, rather than the symlink
    itself. This a dangerous option, as it can cause `tar` to
    access files outside the working directory. See [Symbolic Links](#dereference).

`--directory=dir`

`-C dir`
:   When this option is specified, `tar` will change its working directory
    to dir before performing file archival or extraction
    operations. This option is *position-sensitive*, i.e. it
    affects the arguments that follow it. In particular, any relative file names
    supplied in the command line after it (or read from a file given with
    the `-T` option appearing after it) are assumed to be relative to
    dir. It does not, however, affect file name arguments to other
    *options*, such as `-f`, `-T`, or the like.

    See [Changing the Working Directory](#directory), for a detailed discussion of `-C` and its
    effect, and [Position-Sensitive Options](#Position_002dSensitive-Options), for a discussion of
    position-sensitive options.

`--exclude=pattern`
:   When performing operations, `tar` will skip files that match
    pattern. See [Excluding Some Files](#exclude).

`--exclude-backups`
:   Exclude backup and lock files. See [exclude-backups](#exclude).

`--exclude-from=file`

`-X file`
:   Similar to `--exclude`, except `tar` will use the list of
    patterns in the file file. See [Excluding Some Files](#exclude).

`--exclude-caches`
:   Exclude from dump any directory containing a valid cache directory
    tag file, but still dump the directory node and the tag file itself.

    See [exclude-caches](#exclude).

`--exclude-caches-under`
:   Exclude from dump any directory containing a valid cache directory
    tag file, but still dump the directory node itself.

    See [Excluding Some Files](#exclude).

`--exclude-caches-all`
:   Exclude from dump any directory containing a valid cache directory
    tag file. See [Excluding Some Files](#exclude).

`--exclude-ignore=file`
:   Before dumping a directory, `tar` checks if it contains
    file. If so, exclusion patterns are read from this file.
    The patterns affect only the directory itself. See [Excluding Some Files](#exclude).

`--exclude-ignore-recursive=file`
:   Before dumping a directory, `tar` checks if it contains
    file. If so, exclusion patterns are read from this file.
    The patterns affect the directory and all its subdirectories.
    See [Excluding Some Files](#exclude).

`--exclude-tag=file`
:   Exclude from dump any directory containing file named file, but
    dump the directory node and file itself. See [exclude-tag](#exclude).

`--exclude-tag-under=file`
:   Exclude from dump the contents of any directory containing file
    named file, but dump the directory node itself. See [exclude-tag-under](#exclude).

`--exclude-tag-all=file`
:   Exclude from dump any directory containing file named file.
    See [exclude-tag-all](#exclude).

`--exclude-vcs`
:   Exclude from dump directories and files, that are internal for some
    widely used version control systems.

    See [exclude-vcs](#exclude_002dvcs).

`--exclude-vcs-ignores`
:   Exclude files that match patterns read from VCS-specific ignore
    files. Supported files are: `.cvsignore`, `.gitignore`,
    `.bzrignore`, and `.hgignore`. The semantics of each file
    is the same as for the corresponding VCS, e.g. patterns read from
    `.gitignore` affect the directory and all its subdirectories.
    See [exclude-vcs-ignores](#exclude_002dvcs_002dignores).

`--file=archive`

`-f archive`
:   `tar` will use the file archive as the `tar` archive it
    performs operations on, rather than `tar`’s compilation dependent
    default. See [The `--file` Option](#file-tutorial).

`--files-from=file`

`-T file`
:   `tar` will use the contents of file as a list of archive members
    or files to operate on, in addition to those specified on the
    command-line. See [Reading Names from a File](#files).

`--force-local`
:   Forces `tar` to interpret the file name given to `--file`
    as a local file, even if it looks like a remote tape drive name.
    See [local and remote archives](#local-and-remote-archives).

`--format=format`

`-H format`
:   Selects output archive format. Format may be one of the
    following:

    ‘`v7`’
    :   Creates an archive that is compatible with Unix V7 `tar`.

    ‘`oldgnu`’
    :   Creates an archive that is compatible with GNU `tar` version
        1.12 or earlier.

    ‘`gnu`’
    :   Creates archive in GNU tar 1.13 format. Basically it is the same as
        ‘`oldgnu`’ with the only difference in the way it handles long
        numeric fields.

    ‘`ustar`’
    :   Creates a POSIX.1-1988 compatible archive.

    ‘`posix`’
    :   Creates a POSIX.1-2001 archive.

    See [Controlling the Archive Format](#Formats), for a detailed discussion of these formats.

`--full-time`
:   This option instructs `tar` to print file times to their full
    resolution. Usually this means 1-second resolution, but that depends
    on the underlying file system. The `--full-time` option takes
    effect only when detailed output (verbosity level 2 or higher) has
    been requested using the `--verbose` option, e.g., when listing
    or extracting archives:

    ```
    $ tar -t -v --full-time -f archive.tar
    ```

    or, when creating an archive:

    ```
    $ tar -c -vv --full-time -f archive.tar .
    ```

    Notice, thar when creating the archive you need to specify
    `--verbose` twice to get a detailed output (see [The `--verbose` Option](#verbose-tutorial)).

`--group=group`
:   Files added to the `tar` archive will have a group ID of group,
    rather than the group from the source file. group can specify a
    symbolic name, or a numeric ID, or both as
    name:id. See [Overriding File Metadata](#override).

    Also see the `--group-map` option and comments for the
    `--owner=user` option.

`--group-map=file`
:   Read owner group translation map from file. This option allows to
    translate only certain group names and/or UIDs. See [Overriding File Metadata](#override), for a
    detailed description. When used together with `--group`
    option, the latter affects only those files whose owner group is not listed
    in the file.

    This option does not affect extraction from archives.

`--gzip`

`--gunzip`

`--ungzip`

`-z`
:   This option tells `tar` to read or write archives through
    `gzip`, allowing `tar` to directly operate on several
    kinds of compressed archives transparently. See [Creating and Reading Compressed Archives](#gzip).

`--hard-dereference`
:   When creating an archive, dereference hard links and store the files
    they refer to, instead of creating usual hard link members.

    See [Hard Links](#hard-links).

`--help`

`-?`
:   `tar` will print out a short message summarizing the operations and
    options to `tar` and exit. See [GNU `tar` documentation](#help).

`--hole-detection=method`
:   Use method to detect holes in sparse files. This option implies
    `--sparse`. Valid methods are ‘`seek`’ and ‘`raw`’.
    Default is ‘`seek`’ with fallback to ‘`raw`’ when not
    applicable. See [Archiving Sparse Files](#sparse).

`--ignore-case`
:   Ignore case when matching member or file names with
    patterns. See [Controlling Pattern-Matching](#controlling-pattern_002dmatching).

`--ignore-command-error`
:   Ignore exit codes of subprocesses. See [Writing to an External Program](#Writing-to-an-External-Program).

`--ignore-failed-read`
:   Do not exit unsuccessfully merely because reading failed.
    See [Ignore Failed Read](#Ignore-Failed-Read).

`--ignore-zeros`

`-i`
:   With this option, `tar` will ignore zeroed blocks in the
    archive, which normally signals EOF. This option also suppresses
    warnings about missing or incomplete zero blocks at the end of the
    archive. See [Ignoring Blocks of Zeros](#Ignore-Zeros).

`--incremental`

`-G`
:   Informs `tar` that it is working with an old
    GNU-format incremental backup archive. It is intended
    primarily for backwards compatibility only. See [Using `tar` to Perform Incremental Dumps](#Incremental-Dumps),
    for a detailed discussion of incremental archives.

`--index-file=file`
:   Send verbose output to file instead of to standard output.

`--info-script=command`

`--new-volume-script=command`

`-F command`
:   When `tar` is performing multi-tape backups, command is run
    at the end of each tape. If it exits with nonzero status,
    `tar` fails immediately. See [info-script](#info_002dscript), for a detailed
    discussion of this feature.

`--interactive`

`--confirmation`

`-w`
:   Specifies that `tar` should ask the user for confirmation before
    performing potentially destructive options, such as overwriting files.
    See [Asking for Confirmation During Operations](#interactive).

`--keep-directory-symlink`
:   This option changes the behavior of tar when it encounters a symlink
    with the same name as the directory that it is about to extract. By
    default, in this case tar would first remove the symlink and then
    proceed extracting the directory.

    The `--keep-directory-symlink` option disables this behavior
    and instructs tar to follow symlinks to directories when extracting
    from the archive.

    It is mainly intended to provide compatibility with the Slackware
    installation scripts.

`--keep-newer-files`
:   Do not replace existing files that are newer than their archive copies
    when extracting files from an archive.

`--keep-old-files`

`-k`
:   Do not overwrite existing files when extracting files from an
    archive. Return error if such files exist. See also
    [--skip-old-files](#g_t_002d_002dskip_002dold_002dfiles).

    See [Keep Old Files](#Keep-Old-Files).

`--label=name`

`-V name`
:   When creating an archive, instructs `tar` to write name
    as a name record in the archive. When extracting or listing archives,
    `tar` will only operate on archives that have a label matching
    the pattern specified in name. See [Tape Files](#Tape-Files).

`--level=n`
:   Force incremental backup of level n. As of GNU `tar` version
    1.35.90, the option `--level=0` truncates the snapshot
    file, thereby forcing the level 0 dump. Other values of n are
    effectively ignored. See [--level=0](#g_t_002d_002dlevel_003d0), for details and examples.

    The use of this option is valid only in conjunction with the
    `--listed-incremental` option. See [Using `tar` to Perform Incremental Dumps](#Incremental-Dumps),
    for a detailed description.

`--listed-incremental=snapshot-file`

`-g snapshot-file`
:   During a `--create` operation, specifies that the archive that
    `tar` creates is a new GNU-format incremental
    backup, using snapshot-file to determine which files to backup.
    With other operations, informs `tar` that the archive is in
    incremental format. See [Using `tar` to Perform Incremental Dumps](#Incremental-Dumps).

`--lzip`
:   This option tells `tar` to read or write archives through
    `lzip`. See [Creating and Reading Compressed Archives](#gzip).

`--lzma`
:   This option tells `tar` to read or write archives through
    `lzma`. See [Creating and Reading Compressed Archives](#gzip).

`--lzop`
:   This option tells `tar` to read or write archives through
    `lzop`. See [Creating and Reading Compressed Archives](#gzip).

`--mode=permissions`
:   When adding files to an archive, `tar` will use
    permissions for the archive members, rather than the permissions
    from the files. permissions can be specified either as an octal
    number or as symbolic permissions, like with
    `chmod`. See [Overriding File Metadata](#override).

`--mtime=date`
:   When adding files to an archive, `tar` will use date as
    the modification time of members when creating archives, instead of
    their actual modification times. The value of date can be
    either a textual date representation (see [Date input formats](#Date-input-formats)) or a
    name of the existing file, starting with ‘`/`’ or ‘`.`’. In the
    latter case, the modification time of that file is used. See [Overriding File Metadata](#override).

    When `--clamp-mtime` is also specified, files with
    modification times earlier than date will retain their actual
    modification times, and date will be used only for files with
    modification times later than date.

`--multi-volume`

`-M`
:   Informs `tar` that it should create or otherwise operate on a
    multi-volume `tar` archive. See [Using Multiple Tapes](#Using-Multiple-Tapes).

`--new-volume-script`
:   (see `--info-script`)

`--newer=date`

`--after-date=date`

`-N`
:   When creating an archive, `tar` will only add files that have changed
    since date. If date begins with ‘`/`’ or ‘`.`’, it
    is taken to be the name of a file whose data modification time specifies
    the date. See [Operating Only on New Files](#after).

`--newer-mtime=date`
:   Like `--newer`, but add only files whose
    contents have changed (as opposed to just `--newer`, which will
    also back up files for which any status information has
    changed). See [Operating Only on New Files](#after).

`--no-acls`
:   Disable the POSIX ACLs support. See [acls](#Extended-File-Attributes).

`--no-anchored`
:   An exclude pattern can match any subsequence of the name’s components.
    See [Controlling Pattern-Matching](#controlling-pattern_002dmatching).

`--no-auto-compress`
:   Disables automatic compressed format recognition based on the archive
    suffix. See [--auto-compress](#g_t_002d_002dauto_002dcompress). See [Creating and Reading Compressed Archives](#gzip).

`--no-check-device`
:   Do not check device numbers when creating a list of modified files
    for incremental archiving. See [device numbers](#device-numbers), for
    a detailed description.

`--no-delay-directory-restore`
:   Modification times and permissions of extracted
    directories are set when all files from this directory have been
    extracted. This is the default.
    See [Directory Modification Times and Permissions](#Directory-Modification-Times-and-Permissions).

`--no-ignore-case`
:   Use case-sensitive matching.
    See [Controlling Pattern-Matching](#controlling-pattern_002dmatching).

`--no-ignore-command-error`
:   Print warnings about subprocesses that terminated with a nonzero exit
    code. See [Writing to an External Program](#Writing-to-an-External-Program).

`--no-null`
:   If the `--null` option was given previously, this option
    cancels its effect, so that any following `--files-from`
    options will expect their file lists to be newline-terminated.

`--no-overwrite-dir`
:   Preserve metadata of existing directories when extracting files
    from an archive. See [Overwrite Old Files](#Overwrite-Old-Files).

`--no-quote-chars=string`
:   Remove characters listed in string from the list of quoted
    characters set by the previous `--quote-chars` option
    (see [Quoting Member Names](#quoting-styles)).

`--no-recursion`
:   With this option, `tar` will not recurse into directories.
    See [Descending into Directories](#recurse).

`--no-same-owner`

`-o`
:   When extracting an archive, do not attempt to preserve the owner
    specified in the `tar` archive. This the default behavior
    for ordinary users.

`--no-same-permissions`
:   When extracting an archive, subtract the user’s umask from files from
    the permissions specified in the archive. This is the default behavior
    for ordinary users.

`--no-seek`
:   The archive media does not support seeks to arbitrary
    locations. Usually `tar` determines automatically whether
    the archive can be seeked or not. Use this option to disable this
    mechanism.

`--no-selinux`
:   Disable SELinux context support. See [SELinux](#Extended-File-Attributes).

`--no-unquote`
:   Treat all input file or member names literally, do not interpret
    escape sequences. See [input name quoting](#input-name-quoting).

`--no-verbatim-files-from`
:   Instructs GNU `tar` to treat each line read from a file list as if it
    were supplied in the command line. I.e., leading and trailing
    whitespace is removed and, if the result begins with a dash, it is
    treated as a GNU `tar` command line option.

    This is default behavior. This option is provided as a way to restore
    it after `--verbatim-files-from` option.

    It is implied by the `--no-null` option.

    See [no-verbatim-files-from](#no_002dverbatim_002dfiles_002dfrom).

`--no-wildcards`
:   Do not use wildcards.
    See [Controlling Pattern-Matching](#controlling-pattern_002dmatching).

`--no-wildcards-match-slash`
:   Wildcards do not match ‘`/`’.
    See [Controlling Pattern-Matching](#controlling-pattern_002dmatching).

`--no-xattrs`
:   Disable extended attributes support. See [xattrs](#Extended-File-Attributes).

`--null`
:   When `tar` is using the `--files-from` option, this option
    instructs `tar` to expect file names terminated with
    NUL, and to process file names verbatim.

    This means that `tar` correctly works with file names that
    contain newlines or begin with a dash.

    See [`NUL`-Terminated File Names](#nul).

    See also [verbatim-files-from](#verbatim_002dfiles_002dfrom).

`--numeric-owner`
:   This option will notify `tar` that it should use numeric user
    and group IDs when creating a `tar` file, rather than names.
    See [Handling File Attributes](#Attributes).

`-o`
:   The function of this option depends on the action `tar` is
    performing. When extracting files, `-o` is a synonym for
    `--no-same-owner`, i.e., it prevents `tar` from
    restoring ownership of files being extracted.

    When creating an archive, it is a synonym for
    `--old-archive`. This behavior is for compatibility
    with previous versions of GNU `tar`, and will be
    removed in future releases.

    See [Changes](#Changes), for more information.

`--occurrence[=number]`
:   This option can be used in conjunction with one of the subcommands
    `--delete`, `--diff`, `--extract` or
    `--list` when a list of files is given either on the command
    line or via `-T` option.

    This option instructs `tar` to process only the numberth
    occurrence of each named file. Number defaults to 1, so

    ```
    tar -x -f archive.tar --occurrence filename
    ```

    will extract the first occurrence of the member `filename` from `archive.tar`
    and will terminate without scanning to the end of the archive.

`--old-archive`
:   Synonym for `--format=v7`.

`--one-file-system`
:   Used when creating an archive. Prevents `tar` from recursing into
    directories that are on different file systems from the working
    directory.

`--one-top-level[=dir]`
:   Tells `tar` to create a new directory beneath the extraction directory
    (or the one passed to `-C`) and use it to prevent `tar`
    from modifying files outside that directory.
    If dir is present, it must be a relative file name.
    If it is absent, the name of the new directory
    is the base name of the archive minus any recognized archive suffix.
    If multiple `-C` options are present,
    each has its own subdirectory with the same name.
    Any member names that do not begin
    with that directory name (after
    transformations from `--transform` and
    `--strip-components`) will be prefixed with it. Recognized
    file name suffixes are ‘`.tar`’, and any compression suffixes
    recognizable by See [--auto-compress](#g_t_002d_002dauto_002dcompress).

`--overwrite`
:   Overwrite existing files and directory metadata when extracting files
    from an archive. See [Overwrite Old Files](#Overwrite-Old-Files).

`--overwrite-dir`
:   Overwrite the metadata of existing directories when extracting files
    from an archive. See [Overwrite Old Files](#Overwrite-Old-Files).

`--owner=user`
:   Specifies that `tar` should use user as the owner of members
    when creating archives, instead of the user associated with the source
    file. user can specify a symbolic name, or a numeric
    ID, or both as name:id.
    See [Overriding File Metadata](#override).

    This option does not affect extraction from archives. See also
    `--owner-map`, below.

`--owner-map=file`
:   Read owner translation map from file. This option allows to
    translate only certain owner names or UIDs. See [Overriding File Metadata](#override), for a
    detailed description. When used together with `--owner`
    option, the latter affects only those files whose owner is not listed
    in the file.

    This option does not affect extraction from archives.

`--pax-option=keyword-list`
:   This option enables creation of the archive in POSIX.1-2001
    format (see [GNU `tar` and POSIX `tar`](#posix)) and modifies the way `tar` handles the
    extended header keywords. Keyword-list is a comma-separated
    list of keyword options. See [Controlling Extended Header Keywords](#PAX-keywords), for a detailed
    discussion.

`--portability`

`--old-archive`
:   Synonym for `--format=v7`.

`--posix`
:   Same as `--format=posix`.

`--preserve-order`
:   (See `--same-order`; see [Same Order](#Same-Order).)

`--preserve-permissions`

`--same-permissions`

`-p`
:   When `tar` is extracting an archive, it normally subtracts the
    users’ umask from the permissions specified in the archive and uses
    that number as the permissions to create the destination file.
    Specifying this option instructs `tar` that it should use the
    permissions directly from the archive. See [Setting Access Permissions](#Setting-Access-Permissions).

`--quote-chars=string`
:   Always quote characters from string, even if the selected
    quoting style would not quote them (see [Quoting Member Names](#quoting-styles)).

`--quoting-style=style`
:   Set quoting style to use when printing member and file names
    (see [Quoting Member Names](#quoting-styles)). Valid style values are:
    `literal`, `shell`, `shell-always`, `c`,
    `escape`, `locale`, and `clocale`. Default quoting
    style is `escape`, unless overridden while configuring the
    package.

`--read-full-records`

`-B`
:   Specifies that `tar` should reblock its input, for reading
    from pipes on systems with buggy implementations. See [Options to Help Read Archives](#Reading).

`--record-size=size[suf]`
:   Instructs `tar` to use size bytes per record when accessing the
    archive. The argument can be suffixed with a *size suffix*, e.g.
    `--record-size=10K` for 10 Kilobytes. See [Table 9.1](#size_002dsuffixes),
    for a list of valid suffixes. See [The Blocking Factor of an Archive](#Blocking-Factor), for a detailed
    description of this option.

`--recursion`
:   With this option, `tar` recurses into directories (default).
    See [Descending into Directories](#recurse).

`--recursive-unlink`
:   Remove existing
    directory hierarchies before extracting directories of the same name
    from the archive. See [Recursive Unlink](#Recursive-Unlink).

`--remove-files`
:   Directs `tar` to remove the source file from the file system after
    appending it to an archive. See [Removing Files](#remove-files).

`--restrict`
:   Disable use of some potentially harmful `tar` options.
    Currently this option disables shell invocation from multi-volume menu
    (see [Using Multiple Tapes](#Using-Multiple-Tapes)).

`--rmt-command=cmd`
:   Notifies `tar` that it should use cmd instead of
    the default `/usr/libexec/rmt` (see [Remote Tape Server](#Remote-Tape-Server)).

`--rsh-command=cmd`
:   Notifies `tar` that is should use cmd to communicate with remote
    devices. See [Device Selection and Switching](#Device).

`--same-order`

`--preserve-order`

`-s`
:   This option is an optimization for `tar` when running on machines with
    small amounts of memory. It informs `tar` that the list of file
    arguments has already been sorted to match the order of files in the
    archive. See [Same Order](#Same-Order).

`--same-owner`
:   When extracting an archive, `tar` will attempt to preserve the owner
    specified in the `tar` archive with this option present.
    This is the default behavior for the superuser; this option has an
    effect only for ordinary users. See [Handling File Attributes](#Attributes).

`--same-permissions`
:   (See `--preserve-permissions`; see [Setting Access Permissions](#Setting-Access-Permissions).)

`--seek`

`-n`
:   Assume that the archive media supports seeks to arbitrary
    locations. Usually `tar` determines automatically whether
    the archive can be seeked or not. This option is intended for use
    in cases when such recognition fails. It takes effect only if the
    archive is open for reading (e.g. with `--list` or
    `--extract` options).

`--selinux`
:   Enable the SELinux context support.
    See [selinux](#Extended-File-Attributes).

`--show-defaults`
:   Displays the default options used by `tar` and exits
    successfully. This option is intended for use in shell scripts.
    Here is an example of what you can see using this option:

    ```
    $ tar --show-defaults
    --format=gnu -f- -b20 --quoting-style=escape
    --rmt-command=/usr/libexec/rmt --rsh-command=/usr/bin/rsh
    ```

    Notice, that this option outputs only one line. The example output
    above has been split to fit page boundaries. See [Obtaining GNU `tar` default values](#defaults).

`--show-omitted-dirs`
:   Instructs `tar` to mention the directories it is skipping when
    operating on a `tar` archive. See [show-omitted-dirs](#show_002domitted_002ddirs).

`--show-snapshot-field-ranges`
:   Displays the range of values allowed by this version of `tar`
    for each field in the snapshot file, then exits successfully.
    See [Format of the Incremental Snapshot Files](#Snapshot-Files).

`--show-transformed-names`

`--show-stored-names`
:   Display file or member names after applying any transformations
    (see [Modifying File and Member Names](#transform)). In particular, when used in conjunction with one of
    the archive creation operations it instructs `tar` to list the
    member names stored in the archive, as opposed to the actual file
    names. See [listing member and file names](#listing-member-and-file-names).

`--skip-old-files`
:   Do not overwrite existing files when extracting files from an
    archive. See [Keep Old Files](#Keep-Old-Files).

    This option differs from `--keep-old-files` in that it does not
    treat such files as an error, instead it just silently avoids
    overwriting them.

    The `--warning=existing-file` option can be used together with
    this option to produce warning messages about existing old files
    (see [Controlling Warning Messages](#warnings)).

`--sort=order`
:   Specify the directory sorting order when reading directories.
    Order may be one of the following:

    ‘`none`’
    :   No directory sorting is performed. This is the default.

    ‘`name`’
    :   Sort the directory entries on name. The operating system may deliver
        directory entries in a more or less random order, and sorting them
        makes archive creation more reproducible. See [Making `tar` Archives More Reproducible](#Reproducibility).

    ‘`inode`’
    :   Sort the directory entries on inode number. Sorting directories on
        inode number may reduce the amount of disk seek operations when
        creating an archive for some file systems.

`--sparse`

`-S`
:   Invokes a GNU extension when adding files to an archive that handles
    sparse files efficiently. See [Archiving Sparse Files](#sparse).

`--sparse-version=version`
:   Specifies the *format version* to use when archiving sparse
    files. Implies `--sparse`. See [Archiving Sparse Files](#sparse). For the description
    of the supported sparse formats, See [Storing Sparse Files](#Sparse-Formats).

`--starting-file=name`

`-K name`
:   This option affects extraction only; `tar` will skip extracting
    files in the archive until it finds one that matches name.
    See [Coping with Scarce Resources](#Scarce).

`--strip-components=number`
:   Strip given number of leading components from file names before
    extraction. For example, if archive `archive.tar` contained
    `/some/file/name`, then running

    ```
    tar --extract --file archive.tar --strip-components=2
    ```

    would extract this file to file `name`.

    See [Modifying File and Member Names](#transform).

`--suffix=suffix`
:   Alters the suffix `tar` uses when backing up files from the default
    ‘`~`’. See [Backup options](#backup).

`--tape-length=num[suf]`

`-L num[suf]`
:   Specifies the length of tapes that `tar` is writing as being
    num x 1024 bytes long. If optional suf is given, it
    specifies a multiplicative factor to be used instead of 1024. For
    example, ‘`-L2M`’ means 2 megabytes. See [Table 9.1](#size_002dsuffixes), for a
    list of allowed suffixes. See [Using Multiple Tapes](#Using-Multiple-Tapes), for a detailed
    discussion of this option.

`--test-label`
:   Reads the volume label. If an argument is specified, test whether it
    matches the volume label. See [--test-label option](#g_t_002d_002dtest_002dlabel-option).

`--to-command=command`
:   During extraction `tar` will pipe extracted files to the
    standard input of command. See [Writing to an External Program](#Writing-to-an-External-Program).

`--to-stdout`

`-O`
:   During extraction, `tar` will extract files to stdout rather
    than to the file system. See [Writing to Standard Output](#Writing-to-Standard-Output).

`--totals[=signo]`
:   Displays the total number of bytes transferred when processing an
    archive. If an argument is given, these data are displayed on
    request, when signal signo is delivered to `tar`.
    See [totals](#totals).

`--touch`

`-m`
:   Sets the data modification time of extracted files to the extraction time,
    rather than the data modification time stored in the archive.
    See [Setting Data Modification Times](#Data-Modification-Times).

`--transform=sed-expr`

`--xform=sed-expr`
:   Transform file or member names using `sed` replacement expression
    sed-expr. For example,

    ```
    $ tar cf archive.tar --transform 's,^\./,usr/,' .
    ```

    will add to `archive` files from the working directory,
    replacing initial ‘`./`’ prefix with ‘`usr/`’. For the detailed
    discussion, See [Modifying File and Member Names](#transform).

    To see transformed member names in verbose listings, use
    `--show-transformed-names` option
    (see [show-transformed-names](#show_002dtransformed_002dnames)).

`--uncompress`
:   (See `--compress`, see [Creating and Reading Compressed Archives](#gzip))

`--ungzip`
:   (See `--gzip`, see [Creating and Reading Compressed Archives](#gzip))

`--unlink-first`

`-U`
:   Directs `tar` to remove the corresponding file from the file
    system before extracting it from the archive. See [Unlink First](#Unlink-First).

`--unquote`
:   Enable unquoting input file or member names (default). See [input name quoting](#input-name-quoting).

`--use-compress-program=prog`

`-I=prog`
:   Instructs `tar` to access the archive through prog, which is
    presumed to be a compression program of some sort. See [Creating and Reading Compressed Archives](#gzip).

`--utc`
:   Display file modification dates in UTC. This option implies
    `--verbose`.

`--verbatim-files-from`
:   Instructs GNU `tar` to treat each line read from a file list as a file
    name, even if it starts with a dash.

    File lists are supplied with the `--files-from` (`-T`)
    option. By default, each line read from a file list is first trimmed
    off the leading and trailing whitespace and, if the result begins with
    a dash, it is treated as a GNU `tar` command line option.

    Use the `--verbatim-files-from` option to disable this special
    handling. This facilitates the use of `tar` with file lists
    created by `file` command.

    This option affects all `--files-from` options that occur after
    it in the command line. Its effect is reverted by the
    `--no-verbatim-files-from` option.

    This option is implied by the `--null` option.

    See [verbatim-files-from](#verbatim_002dfiles_002dfrom).

`--verbose`

`-v`
:   Specifies that `tar` should be more verbose about the
    operations it is performing. This option can be specified multiple
    times for some operations to increase the amount of information displayed.
    See [Checking `tar` progress](#verbose).

`--verify`

`-W`
:   Verifies that the archive was correctly written when creating an
    archive. See [Verifying Data as It is Stored](#verify).

`--version`
:   Print information about the program’s name, version, origin and legal
    status, all on standard output, and then exit successfully.
    See [GNU `tar` documentation](#help).

`--volno-file=file`
:   Used in conjunction with `--multi-volume`. `tar` will
    keep track of which volume of a multi-volume archive it is working in
    file. See [volno-file](#volno_002dfile).

`--warning=keyword`
:   Enable or disable warning messages identified by keyword. The
    messages are suppressed if keyword is prefixed with ‘`no-`’.
    See [Controlling Warning Messages](#warnings).

`--wildcards`
:   Use wildcards when matching member names with patterns.
    See [Controlling Pattern-Matching](#controlling-pattern_002dmatching).

`--wildcards-match-slash`
:   Wildcards match ‘`/`’.
    See [Controlling Pattern-Matching](#controlling-pattern_002dmatching).

`--xattrs`
:   Enable extended attributes support. See [xattrs](#Extended-File-Attributes).

`--xattrs-exclude=pattern`
:   Specify exclude pattern for xattr keys.
    See [xattrs-exclude](#Extended-File-Attributes).

`--xattrs-include=pattern.`
:   Specify include pattern for xattr keys. pattern is a globbing
    pattern, e.g. ‘`--xattrs-include='user.*'`’ to include
    only attributes from the user namespace.
    See [xattrs-include](#Extended-File-Attributes).

`--xz`

`-J`
:   Use `xz` for compressing or decompressing the archives. See [Creating and Reading Compressed Archives](#gzip).

`--zstd`
:   Use `zstd` for compressing or decompressing the archives. See [Creating and Reading Compressed Archives](#gzip).

---

Next: [Position-Sensitive Options](#Position_002dSensitive-Options), Previous: [`tar` Options](#Option-Summary), Up: [All `tar` Options](#All-Options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 3.4.3 Short Options Cross Reference

Here is an alphabetized list of all of the short option forms, matching
them with the equivalent long option.

| Short Option | Reference |
| --- | --- |
| -A | [--concatenate](#g_t_002d_002dconcatenate). |
| -B | [--read-full-records](#g_t_002d_002dread_002dfull_002drecords). |
| -C | [--directory](#g_t_002d_002ddirectory). |
| -F | [--info-script](#g_t_002d_002dinfo_002dscript). |
| -G | [--incremental](#g_t_002d_002dincremental). |
| -J | [--xz](#g_t_002d_002dxz). |
| -K | [--starting-file](#g_t_002d_002dstarting_002dfile). |
| -L | [--tape-length](#g_t_002d_002dtape_002dlength). |
| -M | [--multi-volume](#g_t_002d_002dmulti_002dvolume). |
| -N | [--newer](#g_t_002d_002dnewer). |
| -O | [--to-stdout](#g_t_002d_002dto_002dstdout). |
| -P | [--absolute-names](#g_t_002d_002dabsolute_002dnames). |
| -R | [--block-number](#g_t_002d_002dblock_002dnumber). |
| -S | [--sparse](#g_t_002d_002dsparse). |
| -T | [--files-from](#g_t_002d_002dfiles_002dfrom). |
| -U | [--unlink-first](#g_t_002d_002dunlink_002dfirst). |
| -V | [--label](#g_t_002d_002dlabel). |
| -W | [--verify](#g_t_002d_002dverify). |
| -X | [--exclude-from](#g_t_002d_002dexclude_002dfrom). |
| -Z | [--compress](#g_t_002d_002dcompress). |
| -b | [--blocking-factor](#g_t_002d_002dblocking_002dfactor). |
| -c | [--create](#g_t_002d_002dcreate). |
| -d | [--compare](#g_t_002d_002dcompare). |
| -f | [--file](#g_t_002d_002dfile). |
| -g | [--listed-incremental](#g_t_002d_002dlisted_002dincremental). |
| -h | [--dereference](#g_t_002d_002ddereference). |
| -i | [--ignore-zeros](#g_t_002d_002dignore_002dzeros). |
| -j | [--bzip2](#g_t_002d_002dbzip2). |
| -k | [--keep-old-files](#g_t_002d_002dkeep_002dold_002dfiles). |
| -l | [--check-links](#g_t_002d_002dcheck_002dlinks). |
| -m | [--touch](#g_t_002d_002dtouch). |
| -o | When extracting, same as [--no-same-owner](#g_t_002d_002dno_002dsame_002downer). When creating, – [--old-archive](#g_t_002d_002dold_002darchive). The latter usage is deprecated. It is retained for compatibility with the earlier versions of GNU `tar`. In future releases `-o` will be equivalent to `--no-same-owner` only. |
| -p | [--preserve-permissions](#g_t_002d_002dpreserve_002dpermissions). |
| -r | [--append](#g_t_002d_002dappend). |
| -s | [--same-order](#g_t_002d_002dsame_002dorder). |
| -t | [--list](#g_t_002d_002dlist). |
| -u | [--update](#g_t_002d_002dupdate). |
| -v | [--verbose](#g_t_002d_002dverbose). |
| -w | [--interactive](#g_t_002d_002dinteractive). |
| -x | [--extract](#g_t_002d_002dextract). |
| -z | [--gzip](#g_t_002d_002dgzip). |

---

Previous: [Short Options Cross Reference](#Short-Option-Summary), Up: [All `tar` Options](#All-Options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 3.4.4 Position-Sensitive Options

Some GNU `tar` options can be used multiple times in the same
invocation and affect all arguments that appear after them. These are
options that control how file names are selected and what kind of
pattern matching is used.

The most obvious example is the `-C` option. It instructs `tar`
to change to the directory given as its argument prior to processing
the rest of command line (see [Changing the Working Directory](#directory)). Thus, in the following
command:

```
tar -c -f a.tar -C /etc passwd -C /var log spool
```

the file `passwd` will be searched in the directory `/etc`,
and files `log` and `spool` – in `/var`.

These options can also be used in a file list supplied with the
`--files-from` (`-T`) option (see [Reading Names from a File](#files)). In that
case they affect all files (patterns) appearing in that file after
them and remain in effect for any arguments processed after that file.
For example, if the file `list.txt` contained:

```
README
-C src
main.c
```

and `tar` were invoked as follows:

```
tar -c -f a.tar -T list.txt Makefile
```

then the file `README` would be looked up in the working
directory, and files `main.c` and `Makefile` would be looked
up in the directory `src`.

Many options can be prefixed with `--no-` to cancel the effect
of the original option.

For example, the `--recursion` option controls whether to
recurse in the subdirectories. It’s counterpart
`--no-recursion` disables this. Consider the command below. It will
store in the archive the directory `/usr` with all files and
directories that are located in it as well as any files and
directories in `/var`, without recursing into them[6](#FOOT6):

```
tar -cf a.tar --recursion /usr --no-recursion /var/*
```

During archive creation, GNU `tar` keeps track of positional options
used and arguments affected by them. If it finds out that any such
options are used in an obviously erroneous way, the fact is reported
and exit code is set to 2. E.g.:

```
$ tar -cf a.tar . --exclude '*.o'
tar: The following options were used after any non-optional
arguments in archive create or update mode.  These options are
positional and affect only arguments that follow them.  Please,
rearrange them properly.
tar: --exclude '*.o' has no effect
tar: Exiting with failure status due to previous errors
```

The following table summarizes all position-sensitive options.

`--directory=dir`

`-C dir`
:   See [Changing the Working Directory](#directory).

`--null`

`--no-null`
:   See [`NUL`-Terminated File Names](#nul).

`--unquote`

`--no-unquote`
:   See [input name quoting](#input-name-quoting).

`--verbatim-files-from`

`--no-verbatim-files-from`
:   See [verbatim-files-from](#verbatim_002dfiles_002dfrom).

`--recursion`

`--no-recursion`
:   See [Descending into Directories](#recurse).

`--anchored`

`--no-anchored`
:   See [anchored patterns](#anchored-patterns).

`--ignore-case`

`--no-ignore-case`
:   See [case-insensitive matches](#case_002dinsensitive-matches).

`--wildcards`

`--no-wildcards`
:   See [Controlling Pattern-Matching](#controlling-pattern_002dmatching).

`--wildcards-match-slash`

`--no-wildcards-match-slash`
:   See [Controlling Pattern-Matching](#controlling-pattern_002dmatching).

`--exclude`
:   See [Excluding Some Files](#exclude).

`--exclude-from`

`-X`

`--exclude-caches`

`--exclude-caches-under`

`--exclude-caches-all`

`--exclude-tag`

`--exclude-ignore`

`--exclude-ignore-recursive`

`--exclude-tag-under`

`--exclude-tag-all`

`--exclude-vcs`

`--exclude-vcs-ignores`

`--exclude-backups`
:   See [Excluding Some Files](#exclude).

---

Next: [Obtaining GNU `tar` default values](#defaults), Previous: [All `tar` Options](#All-Options), Up: [Invoking GNU `tar`](#tar-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 3.5 GNU `tar` documentation

Being careful, the first thing is really checking that you are using
GNU `tar`, indeed. The `--version` option
causes `tar` to print information about its name, version,
origin and legal status, all on standard output, and then exit
successfully. For example, ‘`tar --version`’ might print:

```
tar (GNU tar) 1.35.90
Copyright (C) 2026 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by John Gilmore and Jay Fenlason.
```

The first occurrence of ‘`tar`’ in the result above is the program
name in the package (for example, `rmt` is another program),
while the second occurrence of ‘`tar`’ is the name of the package
itself, containing possibly many programs. The package is currently
named ‘`tar`’, after the name of the main program it
contains[7](#FOOT7).

Another thing you might want to do is check the spelling or meaning
of some particular `tar` option, without resorting to this
manual, once you have carefully read it. GNU `tar`
has a short help feature, triggerable through the
`--help` option. By using this option, `tar` will
print a usage message listing all available options on standard
output, then exit successfully, without doing anything else and
ignoring all other options. Even if this is only a brief summary, it
may be several screens long. So, if you are not using some kind of
scrollable window, you might prefer to use something like:

```
$ tar --help | less
```

presuming, here, that you like using `less` for a pager. Other
popular pagers are `more` and `pg`. If you know about some
keyword which interests you and do not want to read all the
`--help` output, another common idiom is doing:

```
tar --help | grep keyword
```

for getting only the pertinent lines. Notice, however, that some
`tar` options have long description lines and the above
command will list only the first of them.

The exact look of the option summary displayed by `tar --help` is
configurable. See [Configuring Help Summary](#Configuring-Help-Summary), for a detailed description.

If you only wish to check the spelling of an option, running `tar
--usage` may be a better choice. This will display a terse list of
`tar` options without accompanying explanations.

The short help output is quite succinct, and you might have to get
back to the full documentation for precise points. If you are reading
this paragraph, you already have the `tar` manual in some
form. This manual is available in a variety of forms from
<https://www.gnu.org/software/tar/manual>. It may be printed out of the GNU `tar`
distribution, provided you have TeX already installed somewhere,
and a laser printer around. Just configure the distribution, execute
the command ‘`make dvi`’, then print `doc/tar.dvi` the
usual way (contact your local guru to know how). If GNU `tar`
has been conveniently installed at your place, this
manual is also available in interactive, hypertextual form as an Info
file. Just call ‘`info tar`’ or, if you do not have the
`info` program handy, use the Info reader provided within
GNU Emacs, calling ‘`tar`’ from the main Info menu.

Since 2014, GNU `tar` also has a `man` page.
It briefly explains all the options and operations.
This might be preferable when you don’t need any background.
But bear in mind that the authoritative source of
information about GNU `tar` is this Texinfo documentation.

---

Next: [Checking `tar` progress](#verbose), Previous: [GNU `tar` documentation](#help), Up: [Invoking GNU `tar`](#tar-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 3.6 Obtaining GNU `tar` default values

GNU `tar` has some predefined defaults that are used when you do not
explicitly specify another values. To obtain a list of such
defaults, use `--show-defaults` option. This will output the
values in the form of `tar` command line options:

```
$ tar --show-defaults
--format=gnu -f- -b20 --quoting-style=escape
--rmt-command=/etc/rmt --rsh-command=/usr/bin/rsh
```

Notice, that this option outputs only one line. The example output above
has been split to fit page boundaries.

The above output shows that this version of GNU `tar` defaults to
using ‘`gnu`’ archive format (see [Controlling the Archive Format](#Formats)), it uses standard
output as the archive, if no `--file` option has been given
(see [The `--file` Option](#file-tutorial)), the default blocking factor is 20
(see [The Blocking Factor of an Archive](#Blocking-Factor)). It also shows the default locations where
`tar` will look for `rmt` and `rsh` binaries.

---

Next: [Checkpoints](#checkpoints), Previous: [Obtaining GNU `tar` default values](#defaults), Up: [Invoking GNU `tar`](#tar-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 3.7 Checking `tar` progress

Typically, `tar` performs most operations without reporting any
information to the user except error messages. When using `tar`
with many options, particularly ones with complicated or
difficult-to-predict behavior, it is possible to make serious mistakes.
`tar` provides several options that make observing `tar`
easier. These options cause `tar` to print information as it
progresses in its job, and you might want to use them just for being
more careful about what is going on, or merely for entertaining
yourself. If you have encountered a problem when operating on an
archive, however, you may need more information than just an error
message in order to solve the problem. The following options can be
helpful diagnostic tools.

Normally, the `--list` (`-t`) command to list an archive
prints just the file names (one per line) and the other commands are
silent. When used with most operations, the `--verbose`
(`-v`) option causes `tar` to print the name of each
file or archive member as it is processed. This and the other options
which make `tar` print status information can be useful in
monitoring `tar`.

With `--create` or `--extract`, `--verbose` used
once just prints the names of the files or members as they are processed.
Using it twice causes `tar` to print a longer listing
(See [verbose member listing](#verbose-member-listing), for the description) for each member.
Since `--list` already prints the names of the members,
`--verbose` used once with `--list` causes `tar`
to print an ‘`ls -l`’ type listing of the files in the archive.
The following examples both extract members with long list output:

```
$ tar --extract --file=archive.tar --verbose --verbose
$ tar xvvf archive.tar
```

Verbose output appears on the standard output except when an archive is
being written to the standard output, as with ‘`tar --create
--file=- --verbose`’ (‘`tar cvf -`’, or even ‘`tar cv`’—if the
installer let standard output be the default archive). In that case
`tar` writes verbose output to the standard error stream.

If `--index-file=file` is specified, `tar` sends
verbose output to file rather than to standard output or standard
error.

The `--totals` option causes `tar` to print on the
standard error the total amount of bytes transferred when processing
an archive. When creating or appending to an archive, this option
prints the number of bytes written to the archive and the average
speed at which they have been written, e.g.:

```
$ tar -c -f archive.tar --totals /home
Total bytes written: 7924664320 (7.4GiB, 85MiB/s)
```

When reading an archive, this option displays the number of bytes
read:

```
$ tar -x -f archive.tar --totals
Total bytes read: 7924664320 (7.4GiB, 95MiB/s)
```

Notice, that since `tar` operates on *records*, the number
of bytes reported can be rounded up to the nearest full record. This
can happen, in particular, when the last record in the archive is
partial. See [Blocking](#Blocking).

Finally, when deleting from an archive, the `--totals` option
displays both numbers plus number of bytes removed from the archive:

```
$ tar --delete -f foo.tar --totals --wildcards '*~'
Total bytes read: 9543680 (9.2MiB, 201MiB/s)
Total bytes written: 3829760 (3.7MiB, 81MiB/s)
Total bytes deleted: 1474048
```

You can also obtain this information on request. When
`--totals` is used with an argument, this argument is
interpreted as a symbolic name of a signal, upon delivery of which the
statistics is to be printed:

`--totals=signo`
:   Print statistics upon delivery of signal signo. Valid arguments
    are: `SIGHUP`, `SIGQUIT`, `SIGINT`, `SIGUSR1` and
    `SIGUSR2`. Shortened names without ‘`SIG`’ prefix are also
    accepted.

Both forms of `--totals` option can be used simultaneously.
Thus, `tar -x --totals --totals=USR1` instructs `tar` to
extract all members from its default archive and print statistics
after finishing the extraction, as well as when receiving signal
`SIGUSR1`.

The `--checkpoint` option prints an occasional message
as `tar` reads or writes the archive. It is designed for
those who don’t need the more detailed (and voluminous) output of
`--block-number` (`-R`), but do want visual confirmation
that `tar` is actually making forward progress. By default it
prints a message each 10 records read or written. This can be changed
by giving it a numeric argument after an equal sign:

```
$ tar -c --checkpoint=1000 /var
tar: Write checkpoint 1000
tar: Write checkpoint 2000
tar: Write checkpoint 3000
```

This example shows the default checkpoint message used by
`tar`. If you place a dot immediately after the equal
sign, it will print a ‘`.`’ at each checkpoint[8](#FOOT8). For example:

```
$ tar -c --checkpoint=.1000 /var
...
```

The `--checkpoint` option provides a flexible mechanism for
executing arbitrary actions upon hitting checkpoints, see the next
section (see [Checkpoints](#checkpoints)), for more information on it.

The `--show-omitted-dirs` option, when reading an archive—with
`--list` or `--extract`, for example—causes a message
to be printed for each directory in the archive which is skipped.
This happens regardless of the reason for skipping: the directory might
not have been named on the command line (implicitly or explicitly),
it might be excluded by the use of the
`--exclude=pattern` option, or some other reason.

If `--block-number` (`-R`) is used, `tar` prints, along with
every message it would normally produce, the block number within the
archive where the message was triggered. Also, supplementary messages
are triggered when reading blocks full of NULs, or when hitting end of
file on the archive. As of now, if the archive is properly terminated
with a NUL block, the reading of the file may stop before end of file
is met, so the position of end of file will not usually show when
`--block-number` (`-R`) is used. Note that GNU `tar`
drains the archive before exiting when reading the
archive from a pipe.

This option is especially useful when reading damaged archives, since
it helps pinpoint the damaged sections. It can also be used with
`--list` (`-t`) when listing a file-system backup tape, allowing you to
choose among several backup tapes when retrieving a file later, in
favor of the tape where the file appears earliest (closest to the
front of the tape). See [Backup options](#backup).

---

Next: [Controlling Warning Messages](#warnings), Previous: [Checking `tar` progress](#verbose), Up: [Invoking GNU `tar`](#tar-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]
